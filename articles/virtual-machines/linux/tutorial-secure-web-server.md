---
title: 'Oktatóanyag: Webkiszolgáló biztonságossá beállítása TLS-/SSL-tanúsítványokkal'
description: Ez az oktatóanyag bemutatja, hogyan védheti meg az NGINX-webkiszolgálót futtató Linux rendszerű virtuális gépet az Azure Key Vaultban tárolt SSL-tanúsítványok és az Azure CLI használatával.
author: cynthn
ms.service: virtual-machines
ms.collection: linux
ms.topic: tutorial
ms.date: 04/20/2021
ms.author: cynthn
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 4b2290d29d1f6fbf8acd2e2652cd47a321fe674f
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2021
ms.locfileid: "107813163"
---
# <a name="tutorial-use-tlsssl-certificates-to-secure-a-web-server"></a>Oktatóanyag: TLS-/SSL-tanúsítványok használata a webkiszolgálók biztonságának biztosítása érdekében
A webkiszolgálók biztonságossá Transport Layer Security (TLS), korábbi nevén SSL (SSL) tanúsítvány használható a webes forgalom titkosítására. Ezek a TLS-/SSL-tanúsítványok a Azure Key Vault, és lehetővé teszik a tanúsítványok biztonságos üzembe helyezését Linux rendszerű virtuális gépeken az Azure-ban. Ezen oktatóanyag segítségével megtanulhatja a következőket:

> [!div class="checklist"]
> * Azure Key Vault létrehozása;
> * tanúsítvány létrehozása vagy feltöltése a Key Vaultba;
> * virtuális gép létrehozása és az NGINX-webkiszolgáló telepítése;
> * A tanúsítvány betöltése a virtuális gépbe és az NGINX konfigurálása TLS-kötéssel

Ez az oktatóanyag a cli-t használja [a Azure Cloud Shell,](../../cloud-shell/overview.md)amely folyamatosan frissül a legújabb verzióra. A kód Cloud Shell válassza a **Kipróbálom** lehetőséget bármely kódblokk tetején.

Ha a parancssori felület helyi telepítését és használatát választja, akkor ehhez az oktatóanyaghoz az Azure CLI 2.0.30-as vagy újabb verziójára lesz szükség. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI telepítése]( /cli/azure/install-azure-cli).


## <a name="overview"></a>Áttekintés
Az Azure Key Vault megvédi a titkosítási kulcsokat és titkos kódokat, például a tanúsítványokat és jelszavakat. A Key Vault leegyszerűsíti a tanúsítványkezelési folyamatot, valamint lehetővé teszi a tanúsítványokhoz hozzáférő kulcsok feletti teljes körű felügyeletet. Létrehozhat egy önaláírt tanúsítványt a Key Vaultban, vagy feltölthet egy meglévő, megbízható tanúsítványt.

Beépített tanúsítványokat tartalmazó egyéni virtuális gép rendszerkép használata helyett a futó virtuális gépeken helyezi el a tanúsítványokat. Ez a folyamat biztosítja, hogy a legnaprakészebb tanúsítványok legyenek telepítve a webkiszolgálókon az üzembe helyezés alatt. Ha frissíti vagy lecseréli a tanúsítványt, nem kell új rendszerképet is létrehoznia a virtuális gépekről. A további virtuális gépek létrehozásakor a rendszer automatikusan alkalmazza a legújabb tanúsítványokat. A tanúsítványok a folyamat során egyszer sem hagyják el az Azure platformot, és nem jelennek meg szkriptekben, a parancssori előzményekben vagy sablonokban.


## <a name="create-an-azure-key-vault"></a>Azure Key Vault létrehozása;
Mielőtt létrehozhatna egy Key Vaultot és a tanúsítványokat, létre kell hoznia egy erőforráscsoportot az [az group create](/cli/azure/group) paranccsal. A következő példában létrehozunk egy *myResourceGroupSecureWeb* nevű erőforráscsoportot az *eastus* helyen:

```azurecli-interactive 
az group create --name myResourceGroupSecureWeb --location eastus
```

Ezután hozzon létre egy Key Vaultot az [az keyvault create](/cli/azure/keyvault) paranccsal, és engedélyezze a használatát a virtuális gépek üzembe helyezésekor. Mindegyik Key Vaultnak egyedi névvel kell rendelkeznie, amely csak kisbetűkből állhat. A következő példában cserélje le a *\<mykeyvault>* helyére a saját egyedi Key Vault nevét:

```azurecli-interactive 
keyvault_name=<mykeyvault>
az keyvault create \
    --resource-group myResourceGroupSecureWeb \
    --name $keyvault_name \
    --enabled-for-deployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Tanúsítvány létrehozása és tárolása a Key Vaultban
Éles környezetben importálnia kell egy megbízható szolgáltató által aláírt érvényes tanúsítványt az [az keyvault certificate import](/cli/azure/keyvault/certificate) paranccsal. Ebben az oktatóanyagban a következő példa mutatja be, hogyan hozhat létre olyan önaláírt tanúsítványt az [az keyvault certificate create](/cli/azure/keyvault/certificate) paranccsal, amely az alapértelmezett tanúsítványszabályzatot használja:

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```

### <a name="prepare-a-certificate-for-use-with-a-vm"></a>Tanúsítvány előkészítése virtuális géppel való használatra
Ha a virtuális gép létrehozása alatt szeretné használni a tanúsítványt, szerezze be a tanúsítvány azonosítóját az [az keyvault secret list-versions](/cli/azure/keyvault/secret) paranccsal. Alakítsa át a tanúsítványt az [az vm secret format](/cli/azure/vm/secret#az_vm_secret_format) paranccsal. A következő példa ezen parancsok kimenetét ezekhez változókhoz rendeli, hogy könnyen használhatók legyenek a következő lépésekben:

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm secret format --secrets "$secret" -g myResourceGroupSecureWeb --keyvault $keyvault_name)
```

### <a name="create-a-cloud-init-config-to-secure-nginx"></a>Cloud-init konfiguráció létrehozása az NGINX védelméhez
A [cloud-init](https://cloudinit.readthedocs.io) egy széles körben használt módszer a Linux rendszerű virtuális gépek első indításkor való testreszabásához. A cloud-init használatával csomagokat telepíthet és fájlokat írhat, vagy beállíthatja a felhasználókat és a biztonságot. Mivel a cloud-init a kezdeti rendszerindítás során fut, nincs szükség további lépésekre vagy ügynökökre a konfiguráció alkalmazásához.

Virtuális gép létrehozásakor a tanúsítványokat és a kulcsokat a védett */var/lib/waagent/* könyvtár tárolja. A tanúsítvány virtuális géphez való hozzáadásának automatizálásához és a webkiszolgáló konfigurálásához használja a cloud-init konfigurációt. Ebben a példában az NGINX-webkiszolgálót telepíti és konfigurálja. Ugyanezzel a módszerrel telepítheti és konfigurálhatja az Apache-webkiszolgálókat is. 

Hozzon létre egy *cloud-init-web-server.txt* nevű fájlt, és illessze be a következő konfigurációt:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
      }
runcmd:
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
```

### <a name="create-a-secure-vm"></a>Biztonságos virtuális gép létrehozása
Most hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm) paranccsal. A tanúsítványadatokat a `--secrets` paraméterrel szúrhatja be a Key Vaultból. Alkalmazza a cloud-init konfigurációt a `--custom-data` paraméterrel:

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-web-server.txt \
    --secrets "$vm_secret"
```

A virtuális gép létrehozása, a csomagok telepítése és az alkalmazás elindítása néhány percig tart. A virtuális gép létrehozása után jegyezze fel az Azure CLI által megjelenített `publicIpAddress` értéket. Ezzel a címmel érhető el a hely egy webböngészőben.

Ahhoz, hogy a biztonságos webes adatforgalom elérje a virtuális gépét, nyissa meg az internetről a 443-as portot az [az vm open-port](/cli/azure/vm) paranccsal:

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --port 443
```


### <a name="test-the-secure-web-app"></a>A biztonságos webalkalmazás tesztelése
Most megnyithat egy webböngészőt, és beírhatja a *https: \/ \/ \<publicIpAddress>* címet a címsorba. Adja meg a saját nyilvános IP-címét, amelyet a virtuális gép létrehozásakor kapott. Fogadja el a biztonsági figyelmeztetést, ha önaláírt tanúsítványt használt:

![Webböngésző biztonsági figyelmeztetésének elfogadása](./media/tutorial-secure-web-server/browser-warning.png)

Ekkor a biztonságos NGINX-webhely a következő példához hasonlóan jelenik meg:

![Futó biztonságos NGINX-webhely megtekintése](./media/tutorial-secure-web-server/secured-nginx.png)


## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban egy NGINX-webkiszolgálót biztosított egy TLS/SSL-tanúsítvánnyal, amely a Azure Key Vault. Megtanulta végrehajtani az alábbi műveleteket:

> [!div class="checklist"]
> * Azure Key Vault létrehozása;
> * tanúsítvány létrehozása vagy feltöltése a Key Vaultba;
> * virtuális gép létrehozása és az NGINX-webkiszolgáló telepítése;
> * A tanúsítvány betöltése a virtuális gépbe és az NGINX konfigurálása TLS-kötéssel

Kövesse ezt a hivatkozást az előre felépített virtuálisgép-szkriptek mintáinak megtekintéséhez.

> [!div class="nextstepaction"]
> [Linux rendszerű virtuális gép szkriptjeinek mintái](https://github.com/Azure-Samples/azure-cli-samples/tree/master/virtual-machine)