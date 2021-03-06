---
title: Rövid útmutató – erőforrások létrehozása és kezelése az Azure kommunikációs szolgáltatásokban
titleSuffix: An Azure Communication Services quickstart
description: Ebből a rövid útmutatóból megtudhatja, hogyan hozhatja létre és kezelheti az első Azure kommunikációs szolgáltatások erőforrását.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: overview
ms.service: azure-communication-services
zone_pivot_groups: acs-plat-azp-azcli-net-ps
ms.openlocfilehash: aabb8bdf4105702aa623c45bc291770b05b8279e
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105726771"
---
# <a name="quickstart-create-and-manage-communication-services-resources"></a>Gyors útmutató: kommunikációs szolgáltatások erőforrásainak létrehozása és kezelése

Ismerkedjen meg az Azure kommunikációs szolgáltatásokkal az első kommunikációs szolgáltatások erőforrásának kiépítési folyamatával. A kommunikációs szolgáltatások erőforrásai a [Azure Portalon](https://portal.azure.com) vagy a .net Management SDK-val is kiterjeszthetők. A Management SDK és a Azure Portal lehetővé teszik az erőforrások, valamint a [Azure Resource Manager](../../azure-resource-manager/management/overview.md), az Azure üzembe helyezési és kezelési szolgáltatásával történő létrehozását, konfigurálását, frissítését és törlését. Az SDK-ban elérhető összes funkció elérhető a Azure Portal. 


> [!WARNING]
> Vegye figyelembe, hogy míg a kommunikációs szolgáltatások több földrajzi régióban is elérhetők, a telefonszám beszerzéséhez az erőforrásnak rendelkeznie kell egy "US" értékkel rendelkező adathellyel. Azt is vegye figyelembe, hogy a kommunikációs erőforrások nem vihetők át másik előfizetésre a nyilvános előzetes verzió során.

::: zone pivot="platform-azp"
[!INCLUDE [Azure portal](./includes/create-resource-azp.md)]
::: zone-end

::: zone pivot="platform-azcli"
[!INCLUDE [Azure CLI](./includes/create-resource-azcli.md)]
::: zone-end

::: zone pivot="platform-net"
[!INCLUDE [.NET](./includes/create-resource-net.md)]
::: zone-end

::: zone pivot="platform-powershell"
[!INCLUDE [PowerShell](./includes/create-resource-powershell.md)]
::: zone-end


## <a name="access-your-connection-strings-and-service-endpoints"></a>A kapcsolati karakterláncok és a szolgáltatási végpontok elérése

A kapcsolati karakterláncok lehetővé teszik a kommunikációs szolgáltatások SDK-k számára a kapcsolódást és a hitelesítést az Azure-ban. A kommunikációs szolgáltatáshoz tartozó kapcsolati karakterláncok és szolgáltatási végpontok a Azure Portal vagy programozott módon, Azure Resource Manager API-kkal érhetők el.

A kommunikációs szolgáltatások erőforráshoz való navigálás után válassza a navigációs menü **kulcsok** elemét, és másolja a **kapcsolati karakterláncot** vagy a **végpontok** értékeit a kommunikációs szolgáltatások SDK-k általi használathoz. Vegye figyelembe, hogy van hozzáférése az elsődleges és a másodlagos kulcsokhoz. Ez olyan esetekben lehet hasznos, amikor ideiglenes hozzáférést szeretne biztosítani a kommunikációs szolgáltatások erőforrásaihoz egy harmadik fél vagy átmeneti környezet számára.

:::image type="content" source="./media/key.png" alt-text="A kommunikációs szolgáltatások kulcs oldalának képernyőképe.":::

Az Azure CLI használatával is elérheti a kulcsokra vonatkozó információkat, például az erőforráscsoportot vagy egy adott erőforrás kulcsait. 

Telepítse az [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?tabs=azure-cli) -t, és a következő parancs használatával jelentkezzen be. Meg kell adnia a hitelesítő adatait az Azure-fiókkal való kapcsolódáshoz.
```azurecli
az login
```

Most már elérheti az erőforrásaival kapcsolatos fontos információkat.
```azurecli
az communication list --resource-group "<resourceGroup>"

az communication list-key --name "<communicationName>" --resource-group "<resourceGroup>"
```

Ha ki szeretne választani egy adott előfizetést, megadhatja a ```--subscription``` jelzőt is, és megadhatja az előfizetés azonosítóját.
```
az communication list --resource-group  "resourceGroup>"  --subscription "<subscriptionID>"

az communication list-key --name "<communicationName>" --resource-group "resourceGroup>" --subscription "<subscriptionID>"
```

## <a name="store-your-connection-string"></a>A kapcsolatok karakterláncának tárolása

A kommunikációs szolgáltatások SDK-k kapcsolati karakterláncokat használnak a kommunikációs szolgáltatásokhoz intézett kérések engedélyezéséhez. A következő több lehetőség áll rendelkezésre a kapcsolatok karakterláncának tárolásához:

* Az asztalon vagy eszközön futó alkalmazás a kapcsolati karakterláncot egy **app.config** vagy **web.config** fájlban tárolja. Adja hozzá a kapcsolódási karakterláncot a **appSettings** szakaszhoz ezekben a fájlokban.
* Egy Azure App Service futó alkalmazás a [app Service alkalmazás beállításaiban](../../app-service/configure-common.md)tárolhatja a kapcsolatok karakterláncát. Adja hozzá a kapcsolódási karakterláncot a portál Alkalmazásbeállítások lapján található **kapcsolódási karakterláncok** szakaszhoz.
* A [Azure Key Vaultban](../../data-factory/store-credentials-in-key-vault.md)tárolhatók a kapcsolatok karakterláncai.
* Ha helyileg futtatja az alkalmazást, érdemes lehet a kapcsolódási karakterláncot egy környezeti változóban tárolni.

### <a name="store-your-connection-string-in-an-environment-variable"></a>A kapcsolatok karakterláncának tárolása környezeti változóban

Környezeti változó konfigurálásához nyisson meg egy konzolablak ablakát, és válassza ki az operációs rendszert az alábbi lapok közül. Cserélje le a- `<yourconnectionstring>` t a tényleges kapcsolatok karakterláncára.

#### <a name="windows"></a>[Windows](#tab/windows)

Nyisson meg egy konzolablak ablakot, és írja be a következő parancsot:

```console
setx COMMUNICATION_SERVICES_CONNECTION_STRING "<yourconnectionstring>"
```

Miután hozzáadta a környezeti változót, előfordulhat, hogy újra kell indítania minden futó programot, amelynek szüksége lehet a környezeti változó beolvasására, beleértve a konzolablakot is. Ha például a Visual studiót használja szerkesztőként, indítsa újra a Visual studiót a példa futtatása előtt.

#### <a name="macos"></a>[macOS](#tab/unix)

Szerkessze a **. zshrc**, és adja hozzá a környezeti változót:

```bash
export COMMUNICATION_SERVICES_CONNECTION_STRING="<yourconnectionstring>"
```

A környezeti változó hozzáadását követően futtassa a `source ~/.zshrc` parancsot a konzolablakban a módosítások érvénybe léptetéséhez. Ha a környezeti változót az IDE megnyitva hozta létre, előfordulhat, hogy a változó eléréséhez be kell állítania és újra meg kell nyitnia a szerkesztőt, az IDE-t vagy a rendszerhéjat.

#### <a name="linux"></a>[Linux](#tab/linux)

Szerkessze **.bash_profile**, és adja hozzá a környezeti változót:

```bash
export COMMUNICATION_SERVICES_CONNECTION_STRING="<yourconnectionstring>"
```

A környezeti változó hozzáadását követően futtassa a `source ~/.bash_profile` parancsot a konzolablakban a módosítások érvénybe léptetéséhez. Ha a környezeti változót az IDE megnyitva hozta létre, előfordulhat, hogy a változó eléréséhez be kell állítania és újra meg kell nyitnia a szerkesztőt, az IDE-t vagy a rendszerhéjat.

---

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha törölni szeretné a kommunikációs szolgáltatások előfizetését, törölheti az erőforrást vagy az erőforráscsoportot. Az erőforráscsoport törlésével a hozzá társított egyéb erőforrások is törlődnek.

Ha az erőforrás törlése után bármilyen telefonszáma van rendelve az erőforráshoz, a telefonszámok automatikusan kikerülnek az erőforrásból egy időben.

## <a name="next-steps"></a>Következő lépések

Ebben a rövid útmutatóban megtanulta a következőket:

> [!div class="checklist"]
> * Communication Services-erőforrás létrehozása
> * Erőforrás-földrajz és-címkék konfigurálása
> * Hozzáférés az adott erőforrás kulcsaihoz
> * Erőforrás törlése

> [!div class="nextstepaction"]
> [Az első felhasználói hozzáférési jogkivonatok létrehozása](access-tokens.md)
