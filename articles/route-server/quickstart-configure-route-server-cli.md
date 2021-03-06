---
title: 'Gyors útmutató: útválasztási kiszolgáló létrehozása és konfigurálása az Azure CLI használatával'
description: Ebből a rövid útmutatóból megtudhatja, hogyan hozhat létre és konfigurálhat útválasztási kiszolgálót az Azure CLI használatával.
services: route-server
author: duongau
ms.service: route-server
ms.topic: quickstart
ms.date: 03/02/2021
ms.author: duau
ms.openlocfilehash: e9c583db7493afc04b2c66553801f62d364b0a80
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/30/2021
ms.locfileid: "103419608"
---
# <a name="quickstart-create-and-configure-route-server-using-azure-cli"></a>Gyors útmutató: útválasztási kiszolgáló létrehozása és konfigurálása az Azure CLI használatával 

Ebből a cikkből megtudhatja, hogyan konfigurálhatja az Azure Route Servert a virtuális hálózatban lévő hálózati virtuális berendezésekkel (NVA) az Azure CLI használatával. Az Azure Route Server megkeresi az útvonalakat a NVA, és a virtuális hálózatban lévő virtuális gépekhez programjuk azokat. Emellett a virtuális hálózati útvonalakat is meghirdeti a NVA. További információért olvassa el az [Azure Route Servert](overview.md).

> [!IMPORTANT]
> Az Azure Route Server (előzetes verzió) jelenleg nyilvános előzetes verzióban érhető el.
> Erre az előzetes verzióra nem vonatkozik szolgáltatói szerződés, és a használata nem javasolt éles számítási feladatok esetén. Előfordulhat, hogy néhány funkció nem támogatott, vagy korlátozott képességekkel rendelkezik.
> További információ: [Kiegészítő használati feltételek a Microsoft Azure előzetes verziójú termékeihez](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

##  <a name="prerequisites"></a>Előfeltételek 

* Aktív előfizetéssel rendelkező Azure-fiók. [Hozzon létre egy fiókot ingyenesen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
* Győződjön meg arról, hogy rendelkezik a legújabb Azure CLI-vel, vagy a portálon Azure Cloud Shell is használhatja. 
* Tekintse át az [Azure Route Server szolgáltatási korlátozásait](route-server-faq.md#limitations). 

##  <a name="create-a-route-server"></a>Útvonal-kiszolgáló létrehozása 

###  <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Jelentkezzen be Azure-fiókjába, és válassza ki előfizetését. 

A konfiguráció megkezdéséhez jelentkezzen be az Azure-fiókjába. Ha a "kipróbálás" Cloud Shell használja, automatikusan bejelentkezett. Az alábbi példák segítséget nyújtanak a kapcsolódáshoz:

```azurecli-interactive
az login
```

Keresse meg a fiókot az előfizetésekben.

```azurecli-interactive
az account list
```

Válassza ki azt az előfizetést, amelyhez ExpressRoute-áramkört kíván létrehozni.

```azurecli-interactive
az account set --subscription "<subscription ID>"
```

### <a name="create-a-resource-group-and-virtual-network"></a>Erőforráscsoport és virtuális hálózat létrehozása 

Az Azure Route Server létrehozása előtt szüksége lesz egy virtuális hálózatra az üzemelő példány üzemeltetéséhez. Az alábbi parancs használatával hozzon létre egy erőforráscsoportot és egy virtuális hálózatot. Ha már rendelkezik virtuális hálózattal, ugorjon a következő szakaszra.

```azurecli-interactive
az group create -n "RouteServerRG" -l "westus" 
az network vnet create -g "RouteServerRG" -n "myVirtualNetwork" --address-prefix "10.0.0.0/16" 
``` 

### <a name="add-a-subnet"></a>Alhálózat hozzáadása 

1. Adjon hozzá egy *RouteServerSubnet* nevű alhálózatot az Azure Route-kiszolgáló üzembe helyezéséhez a alkalmazásban. Ez az alhálózat csak az Azure Route Server dedikált alhálózata. A RouteServerSubnet/27 vagy egy rövidebb előtagnak kell lennie (például/26,/25), vagy hibaüzenet jelenik meg az Azure-útválasztó kiszolgáló hozzáadásakor.

    ```azurecli-interactive 
    az network vnet subnet create -g "RouteServerRG" --vnet-name "myVirtualNetwork" --name "RouteServerSubnet" --address-prefix "10.0.0.0/24"  
    ``` 

1. Szerezze be a RouteServerSubnet AZONOSÍTÓját. A virtuális hálózat összes alhálózatának erőforrás-AZONOSÍTÓjának megtekintéséhez használja az alábbi parancsot: 

    ```azurecli-interactive 
    $subnet_id = $(az network vnet subnet show -n "RouteServerSubnet" --vnet-name "myVirtualNetwork" -g "RouteServerRG" --query id -o tsv) 
    ``` 

A RouteServerSubnet-azonosító a következőhöz hasonlóan néz ki: 

`/subscriptions/<subscriptionID>/resourceGroups/RouteServerRG/providers/Microsoft.Network/virtualNetworks/myVirtualNetwork/subnets/RouteServerSubnet`

## <a name="create-the-route-server"></a>Az útvonal-kiszolgáló létrehozása 

Hozza létre az útválasztási kiszolgálót a következő paranccsal: 

```azurecli-interactive
az network routeserver create -n "myRouteServer" -g "RouteServerRG" --hosted-subnet $subnet_id  
``` 

A helynek meg kell egyeznie a virtuális hálózat helyével. A HostedSubnet az előző szakaszban beszerzett RouteServerSubnet-azonosító. 

## <a name="create-peering-with-an-nva"></a>Peering létrehozása NVA 

Használja az alábbi parancsot az útválasztási kiszolgálóról a NVA való társítás létrehozásához: 

```azurecli-interactive 

az network routeserver peering create --routeserver-name "myRouteServer" -g "RouteServerRG" --peer-ip "nva_ip" --peer-asn "nva_asn" -n "NVA1_name" 

``` 

a "nva_ip" a NVA hozzárendelt virtuális hálózati IP-cím. a "nva_asn" a NVA konfigurált autonóm rendszer száma (ASN). Az ASN bármely 16 bites szám lehet, amely nem a 65515-65520-es tartományba esik. A ASN ezen tartományát a Microsoft foglalta le. 

A következő parancs használatával állíthatja be a társítást különböző NVA vagy ugyanazon NVA egy másik példányával a redundancia érdekében:

```azurecli-interactive 

az network routeserver peering create --routeserver-name "myRouteServer" -g "RouteServerRG" --peer-ip "nva_ip" --peer-asn "nva_asn" -n "NVA2_name" 
``` 

## <a name="complete-the-configuration-on-the-nva"></a>A konfiguráció befejezése a NVA 

A NVA konfigurálásának befejezéséhez és a BGP-munkamenetek engedélyezéséhez szüksége lesz az Azure Route Server IP-címére és ASN-ra. Ezt az információt a következő paranccsal kérheti le: 

```azurecli-interactive 
az network routeserver show -g "RouteServerRG" -n "myRouteServer" 
``` 

A kimeneten a következő információk szerepelnek. 

```azurecli-interactive 
RouteServerAsn  : 65515 

RouteServerIps  : {10.5.10.4, 10.5.10.5}  "virtualRouterAsn": 65515, 

  "virtualRouterIps": [ 

    "10.0.0.4", 

    "10.0.0.5" 

  ], 

``` 

## <a name="configure-route-exchange"></a>Útvonal-Exchange konfigurálása 

Ha egy ExpressRoute-átjáróval és egy Azure-beli VPN-átjáróval rendelkezik ugyanabban a VNet, és szeretné Exchange-útvonalakat cserélni, engedélyezheti az útválasztási Exchange használatát az Azure Route Serveren.

> [!IMPORTANT]
> A zöldmezős üzembe helyezések esetén győződjön meg arról, hogy létrehozta az Azure VPN Gatewayt az Azure Route Server létrehozása előtt; Ellenkező esetben az Azure VPN Gateway üzembe helyezése sikertelen lesz.
> 

1. Az Azure Route Server és az átjáró (k) közötti útvonal-csere engedélyezéséhez használja a következő parancsot:

```azurecli-interactive 
az network routeserver update -g "RouteServerRG" -n "myRouteServer" --allow-b2b-traffic true 

``` 

2. Az Azure Route Server és az átjáró (k) közötti útválasztási váltás letiltásához használja a következő parancsot:

```azurecli-interactive
az network routeserver update -g "RouteServerRG" -n "myRouteServer" --allow-b2b-traffic false 
``` 

## <a name="troubleshooting"></a>Hibaelhárítás 

Az Azure Route Server által hirdetett és fogadott útvonalakat az alábbi paranccsal tekintheti meg:

```azurecli-interactive 
az network routeserver peering list-advertised-routes -g RouteServerRG --vrouter-name myRouteServer -n NVA1_name 
az network routeserver peering list-learned-routes -g RouteServerRG --vrouter-name myRouteServer -n NVA1_name 
``` 

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha már nincs szüksége az Azure Route Serverre, az alábbi parancsokkal távolítsa el a BGP-társat, majd távolítsa el az útválasztási kiszolgálót. 

1. Távolítsa el a BGP-társat az Azure Route Server és egy NVA között a következő paranccsal:

```azurecli-interactive
az network routeserver peering delete --routeserver-name "myRouteServer" -g "RouteServerRG" -n "NVA2_name" 
``` 

2. Az Azure Route Server eltávolítása ezzel a paranccsal: 

```azurecli-interactive 
az network routeserver delete -n "myRouteServer" -g "RouteServerRG" 
``` 

## <a name="next-steps"></a>Következő lépések

Az Azure Route Server létrehozása után folytassa a további tudnivalókat arról, hogy az Azure Route Server hogyan kommunikál a ExpressRoute és a VPN-átjárókkal: 

> [!div class="nextstepaction"]
> [Azure ExpressRoute és Azure VPN-támogatás](expressroute-vpn-support.md)
 
