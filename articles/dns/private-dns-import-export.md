---
title: Tartományi zónafájl importálása és exportálása az Azure Private DNS-hez – Azure CLI
titleSuffix: Azure DNS
description: Ismerje meg, hogyan importálhat és exportálhat egy DNS-zónafájl az Azure Private DNS-be az Azure CLI használatával
services: dns
author: duongau
ms.service: dns
ms.date: 03/16/2021
ms.author: duau
ms.topic: how-to
ms.openlocfilehash: 2e5babb85dbf4aa7da707e22be5eeaf3ae89972d
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/06/2021
ms.locfileid: "106450989"
---
# <a name="import-and-export-a-private-dns-zone-file-for-azure-private-dns"></a>Privát DNS-zónafájl importálása és exportálása az Azure Private DNS-be

Ez a cikk bemutatja, hogyan importálhat és exportálhat Azure DNS DNS-zónákat az Azure CLI használatával.

## <a name="introduction-to-dns-zone-migration"></a>A DNS-zónák áttelepítésének bemutatása

A DNS-zónafájl szövegfájl, amely a zónában lévő összes DNS-rekord részleteit tartalmazza. Szabványos formátumot követ, amely lehetővé teszi a DNS-rekordok DNS-rendszerek közötti átvitelét. A zónafájl egy gyors, megbízható és kényelmes módszer a DNS-zónák Azure DNSba vagy onnan történő átvitelére.

Az Azure Private DNS támogatja a zónaadatok importálását és exportálását az Azure parancssori felület (CLI) használatával. A zónafájl importálása jelenleg **nem** támogatott Azure PowerShell vagy a Azure Portalon keresztül.

Az Azure CLI egy platformfüggetlen parancssori eszköz, amely az Azure-szolgáltatások kezelésére szolgál. A Windows, Mac és Linux platformokon elérhető az [Azure letöltések oldaláról](https://azure.microsoft.com/downloads/). A Többplatformos támogatás fontos a zónafájl importálásához és exportálásához, mert a leggyakoribb névkiszolgálói szoftver, a [kötés](https://www.isc.org/downloads/bind/)általában Linux rendszeren fut.

## <a name="obtain-your-existing-dns-zone-file"></a>Meglévő DNS-zóna fájljának beszerzése

A DNS-zónafájl Azure DNSba való importálása előtt be kell szereznie a zóna fájljának másolatát. A fájl forrása attól függ, hogy a DNS-zóna jelenleg hol van üzemeltetve.

* Ha a DNS-zónát egy partneri szolgáltatás (például egy tartományregisztráló, egy dedikált DNS-szolgáltató vagy egy alternatív felhőalapú szolgáltató) üzemelteti, a szolgáltatásnak meg kell adnia a DNS-zónafájl letöltésének lehetőségét.
* Ha a DNS-zóna a Windows DNS-ben üzemel, a zónafájl alapértelmezett mappája a **%systemroot%\System32\Dns**. Az egyes zónák teljes elérési útja a DNS-konzol **általános** lapján is látható.
* Ha a DNS-zóna a kötés használatával fut, az egyes zónákhoz tartozó zónafájl helye a **. conf nevű** kötési konfigurációs fájlban van megadva.

## <a name="import-a-dns-zone-file-into-azure-private-dns"></a>DNS-zóna fájljának importálása az Azure Private DNS-be

A zónafájl importálása egy új zónát hoz létre az Azure Private DNS-ben, ha még nem létezik ilyen. Ha a zóna már létezik, a zónafájl rekordhalmazát egyesíteni kell a meglévő rekordhalmazokkal.

### <a name="merge-behavior"></a>Egyesítési viselkedés

* Alapértelmezés szerint a meglévő és az új rekordhalmazok egyesülnek. Az egyesített rekordhalmazon belüli azonos rekordok megismétlődnek.
* A rekordhalmazok egyesítése esetén a rendszer a meglévő rekordhalmazok élettartamát (TTL) használja.
* A SOA-paramétereket (kivéve `host` ) mindig az importált zónafájl veszi át. Hasonlóképpen, ha a névkiszolgáló-rekord a zóna csúcsára van beállítva, az ÉLETTARTAMot mindig az importált zónafájl veszi át.
* Egy importált CNAME rekord nem cseréli le a meglévő CNAME rekordot ugyanazzal a névvel.  
* Ha ütközés lép fel egy CNAME rekord és egy azonos nevű, de eltérő típusú rekord között (függetlenül attól, hogy melyik a meglévő vagy az új), a rendszer megőrzi a meglévő rekordot. 

### <a name="additional-information-about-importing"></a>További információ az importálásról

A következő megjegyzések további technikai részleteket tartalmaznak a zóna importálási folyamatáról.

* Az `$TTL` irányelv nem kötelező, és támogatott. Ha nincs `$TTL` megadva direktíva, a rendszer az explicit TTL nélküli rekordokat a 3600 másodperces alapértelmezett TTL értékre állítja be. Ha ugyanabban a rekordhalmazban két rekord eltérő TTLs-t határoz meg, akkor az alacsonyabb értéket használja a rendszer.
* Az `$ORIGIN` irányelv nem kötelező, és támogatott. Ha nincs `$ORIGIN` beállítva, a rendszer az alapértelmezett értéket használja a parancssorban megadott zóna neve (valamint a "." megszakításával).
* A `$INCLUDE` és az `$GENERATE` irányelvek nem támogatottak.
* Ezek a rekordtípusok támogatottak: A, AAAA, CAA, CNAME, MX, NS, SOA, SRV és TXT.
* A SOA-rekordot a rendszer automatikusan hozza létre Azure DNS a zóna létrehozásakor. Zónafájl importálásakor a rendszer az összes SOA-paramétert a (z) paraméter *kivételével* a zónafájl alapján veszi át `host` . Ez a paraméter a Azure DNS által megadott értéket használja. Ennek az az oka, hogy ennek a paraméternek a Azure DNS által biztosított elsődleges névkiszolgáló-kiszolgálóra kell hivatkoznia.
* A zóna csúcsán lévő névkiszolgáló-rekordhalmazt a rendszer automatikusan létrehozza Azure DNS a zóna létrehozásakor. A rendszer csak a rekord TTL-értékét importálja. Ezek a rekordok tartalmazzák a Azure DNS által megadott névkiszolgálói neveket. A rekord adatait nem írja felül az importált zónafájl értékei.
* A nyilvános előzetes verzióban a Azure DNS csak az egyetlen karakterláncos TXT-rekordokat támogatja. A többkarakterláncos TXT-rekordok összefűzése és 255 karakter közötti csonkítása.

### <a name="cli-format-and-values"></a>CLI formátum és értékek

A DNS-zónák importálására szolgáló Azure CLI-parancs formátuma a következő:

```azurecli
az network private-dns zone import -g <resource group> -n <zone name> -f <zone file name>
```

Értékek:

* `<resource group>` a Azure DNS zónához tartozó erőforráscsoport neve.
* `<zone name>` a zóna neve.
* `<zone file name>` az importálandó zónafájl elérési útja/neve.

Ha egy ilyen nevű zóna nem létezik az erőforráscsoporthoz, a rendszer létrehozza az Ön számára. Ha a zóna már létezik, az importált rekordhalmazok egyesítve lesznek a meglévő rekordhalmazokkal.

### <a name="import-a-zone-file"></a>Zónafájl importálása

Zónafájl importálása a zóna **contoso.com**.

1. Ha még nem rendelkezik ilyennel, létre kell hoznia egy Resource Manager-erőforráscsoportot.

    ```azurecli
    az group create --resource-group myresourcegroup -l westeurope
    ```

2. Ha a **contoso.com** a fájlból **contoso.com.txt** egy új DNS-zónába szeretné importálni az erőforráscsoport **myresourcegroup**, futtassa a parancsot `az network private-dns zone import` .<BR>Ez a parancs betölti a zónafájl fájlját, és elemzi azt. A parancs végrehajt egy több parancsot a Azure DNS szolgáltatásban a zóna és a zóna összes rekordhalmazának létrehozásához. A parancs a konzol ablakban a hibákkal és figyelmeztetésekkel kapcsolatos folyamatokat is jelzi. Mivel a rekordhalmazok sorozatokban jönnek létre, eltarthat néhány percig, amíg a rendszer importál egy nagyméretű zónafájl-fájlt.

    ```azurecli
    az network private-dns zone import -g myresourcegroup -n contoso.com -f contoso.com.txt
    ```

### <a name="verify-the-zone"></a>A zóna ellenőrzése

Ha a DNS-zónát a fájl importálása után szeretné ellenőrizni, az alábbi módszerek bármelyikét használhatja:

* A rekordokat a következő Azure CLI-parancs használatával listázhatja:

    ```azurecli
    az network private-dns record-set list -g myresourcegroup -z contoso.com
    ```

* `nslookup`A (z) segítségével ellenőrizheti a rekordok névfeloldását. Mivel a zóna még nincs delegálva, explicit módon meg kell adnia a helyes Azure DNS névkiszolgálók nevét. Az alábbi példa bemutatja, hogyan kérhető le a zónához hozzárendelt névkiszolgálók neve. Ez azt is bemutatja, hogyan lehet lekérdezni a "www" rekordot a használatával `nslookup` .

## <a name="export-a-dns-zone-file-from-azure-dns"></a>DNS-zóna fájljának exportálása Azure DNS

A DNS-zónák exportálására szolgáló Azure CLI-parancs formátuma a következő:

```azurecli
az network private-dns zone export -g <resource group> -n <zone name> -f <zone file name>
```

Értékek:

* `<resource group>` a Azure DNS zónához tartozó erőforráscsoport neve.
* `<zone name>` a zóna neve.
* `<zone file name>` az exportálandó zóna fájljának elérési útja/neve.

A zóna importálásához hasonlóan először be kell jelentkeznie, ki kell választania az előfizetést, és az Azure CLI-t a Resource Manager üzemmód használatára kell beállítania.

### <a name="to-export-a-zone-file"></a>Zónafájl exportálása

Ha exportálni szeretné a meglévő Azure DNS zóna **contoso.com** az erőforráscsoport **myresourcegroup** a fájl **contoso.com.txt** (az aktuális mappában), futtassa a parancsot `azure network private-dns zone export` . Ezzel a paranccsal a rendszer meghívja a Azure DNS szolgáltatást a rekordhalmazok enumerálásához a zónában, és exportálja az eredményeket egy KÖTÉSsel kompatibilis zónafájl-fájlba.

```azurecli
az network private-dns zone export -g myresourcegroup -n contoso.com -f contoso.com.txt
```

## <a name="next-steps"></a>Következő lépések

* Ismerje meg, hogyan kezelheti a rekordhalmazokat [és rekordokat](./private-dns-getstarted-cli.md) a DNS-zónában.
