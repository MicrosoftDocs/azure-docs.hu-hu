---
title: 'Oktatóanyag: létrehozás a portál használatával – webalkalmazási tűzfal'
description: Ebből az oktatóanyagból megtudhatja, hogyan hozhat létre Application Gateway-t webalkalmazási tűzfallal a Azure Portal használatával.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: tutorial
ms.date: 03/25/2021
ms.author: victorh
ms.openlocfilehash: 35bede052f06c0fcffe46460a376d10690fd4417
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105559628"
---
# <a name="tutorial-create-an-application-gateway-with-a-web-application-firewall-using-the-azure-portal"></a>Oktatóanyag: Application Gateway létrehozása webalkalmazási tűzfallal a Azure Portal használatával

Ez az oktatóanyag bemutatja, hogyan hozhat létre egy Application Gateway webalkalmazási tűzfallal (WAF) a Azure Portal használatával. A WAF [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project)-szabályokkal védi az alkalmazást. Ezek a szabályok olyan támadások ellen nyújtanak védelmet, mint az SQL-injektálás, a Cross-Site Scripting támadások és a munkamenet-eltérítések. Az Application Gateway létrehozása után tesztelje, hogy megfelelően működik-e. Az Azure Application Gateway használatával a figyelőket a portokhoz, a szabályok létrehozásához és az erőforrások háttér-készlethez való hozzáadásához irányíthatja az alkalmazás webes forgalmát adott erőforrásokra. Az egyszerűség kedvéért ez az oktatóanyag egy egyszerű telepítőt használ egy nyilvános előtér-IP-címmel, egy alapszintű figyelővel, amely egyetlen helyet üzemeltet az Application gatewayben, két virtuális gépet használ a háttér-készlethez, valamint egy alapszintű kérelem-útválasztási szabályt.

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Alkalmazásátjáró létrehozása engedélyezett WAF-fel
> * Háttér-kiszolgálóként használt virtuális gépek létrehozása
> * Tárfiók létrehozása és diagnosztika konfigurálása
> * Az alkalmazásátjáró tesztelése

![Példa webalkalmazási tűzfalra](../media/application-gateway-web-application-firewall-portal/scenario-waf.png)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

<!---If you prefer, you can complete this tutorial using [Azure PowerShell](tutorial-restrict-web-traffic-powershell.md) or [Azure CLI](tutorial-restrict-web-traffic-cli.md).--->

## <a name="prerequisites"></a>Előfeltételek

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

## <a name="sign-in-to-azure"></a>Bejelentkezés az Azure-ba

Jelentkezzen be az Azure Portalra a [https://portal.azure.com](https://portal.azure.com) webhelyen.

## <a name="create-an-application-gateway"></a>Application Gateway létrehozása

1. Válassza az **erőforrás létrehozása** elemet a Azure Portal bal oldali menüjében. Megjelenik az **új** ablak.

2. Válassza a **hálózatkezelés** lehetőséget, majd a **Kiemelt** listában válassza a **Application Gateway** lehetőséget.

### <a name="basics-tab"></a>Alapbeállítások lap

1. Az **alapok** lapon adja meg a következő Application Gateway-beállításokhoz tartozó értékeket:

   - **Erőforráscsoport**: válassza ki az erőforráscsoport **myResourceGroupAG** . Ha nem létezik, válassza az **új létrehozása** elemet a létrehozásához.
   - **Application Gateway neve**: írja be a *myAppGateway* nevet az Application Gateway neveként.
   - **Szintű**: válassza a **WAF v2** elemet.

     ![Új Application Gateway létrehozása: alapismeretek](../media/application-gateway-web-application-firewall-portal/application-gateway-create-basics.png)

2.  Ahhoz, hogy az Azure kommunikáljon a létrehozott erőforrások között, szüksége van egy virtuális hálózatra. Hozzon létre egy új virtuális hálózatot, vagy használjon egy meglévőt. Ebben a példában egy új virtuális hálózatot fog létrehozni az Application Gateway létrehozásakor. Application Gateway példányok külön alhálózatokban jönnek létre. Ebben a példában két alhálózatot hoz létre: egyet az Application Gateway számára, és egy másikat a háttér-kiszolgálók számára.

    A **virtuális hálózat konfigurálása** területen válassza az **új létrehozása** lehetőséget az új virtuális hálózat létrehozásához. A megnyíló **virtuális hálózat létrehozása** ablakban adja meg a következő értékeket a virtuális hálózat és két alhálózat létrehozásához:

    - **Név**: írja be a *myVNet* nevet a virtuális hálózat nevéhez.

    - **Alhálózat neve** (Application Gateway alhálózat): az **alhálózatok** rácsa egy *alapértelmezett* nevű alhálózatot fog megjeleníteni. Módosítsa az alhálózat nevét a *myAGSubnet* értékre.<br>Az Application Gateway-alhálózat csak Application Gateway átjárókat tartalmazhat. Más erőforrások nem engedélyezettek.

    - **Alhálózat neve** (háttérbeli kiszolgáló alhálózata): az **alhálózatok** rácsának második sorában adja meg az *myBackendSubnet* értéket az **alhálózat neve** oszlopban.

    - **Címtartomány** (háttér-kiszolgáló alhálózata): az **alhálózatok** rácsának második sorában olyan címtartományt adjon meg, amely nem fedi át a *myAGSubnet* címtartomány-tartományát. Ha például a *myAGSubnet* 10.0.0.0/24, a *10.0.1.0/24* értéket adja meg a *myBackendSubnet*-tartományhoz.

    A **virtuális hálózat létrehozása** ablak bezárásához és a virtuális hálózat beállításainak mentéséhez kattintson **az OK gombra** .

     ![Új Application Gateway létrehozása: virtuális hálózat](../media/application-gateway-web-application-firewall-portal/application-gateway-create-vnet.png)
    
3. Az **alapvető** beállítások lapon fogadja el az alapértelmezett értékeket a többi beállításnál, majd válassza a **Next (tovább): frontends** elemet.

### <a name="frontends-tab"></a>Frontendek lap

1. A **frontendek** lapon ellenőrizze, hogy a előtérbeli **IP-cím típusa** **nyilvános** értékre van-e állítva. <br>Az előtérbeli IP-címet úgy állíthatja be, hogy a használati esetnek megfelelően nyilvános vagy privát legyen. Ebben a példában egy nyilvános előtérbeli IP-címet választ.
   > [!NOTE]
   > A Application Gateway v2 SKU esetében csak **nyilvános** ELŐTÉRBELI IP-konfigurációt választhat. A privát előtérbeli IP-konfiguráció jelenleg nincs engedélyezve ehhez a v2 SKU-hoz.

2. Válassza a **nyilvános IP-cím** **új hozzáadása** lehetőséget, és adja meg a *myAGPublicIPAddress* a nyilvános IP-cím neveként, majd kattintson **az OK gombra**. 

     ![Új Application Gateway létrehozása: frontendek](../media/application-gateway-web-application-firewall-portal/application-gateway-create-frontends.png)

3. Válassza a Next (tovább) lehetőséget **: háttérrendszer**.

### <a name="backends-tab"></a>Háttérrendszer lap

A háttér-készlet arra szolgál, hogy a kérelmeket a kérést kiszolgáló háttér-kiszolgálókra irányítsa. A háttér-készletek a hálózati adapterek, a virtuálisgép-méretezési csoportok, a nyilvános IP-címek, a belső IP-címek, a teljes tartománynevek (FQDN) és a több-bérlős háttér-végpontok, például a Azure App Service tagjai lehetnek. Ebben a példában egy üres háttér-készletet hoz létre az Application Gateway használatával, majd később a háttérbeli célokat is hozzáadja a háttér-készlethez.

1. A **backends (háttérrendszer** ) lapon válassza a **háttérbeli készlet hozzáadása** elemet.

2. A megnyíló **háttérbeli készlet hozzáadása** ablakban adja meg a következő értékeket egy üres háttérbeli készlet létrehozásához:

    - **Név**: adja meg a *myBackendPool* nevét a háttér-készlet neveként.
    - **Háttérbeli készlet hozzáadása célok nélkül**: válassza az **Igen** lehetőséget, ha célokat nem tartalmazó háttér-készletet szeretne létrehozni. Az Application Gateway létrehozása után hozzá kell adni a háttérbeli célokat.

3. A háttérbeli **készlet hozzáadása** ablakban válassza a **Hozzáadás** elemet a háttérbeli készlet konfigurációjának mentéséhez, és térjen vissza a **háttérrendszer** lapra.

     ![Új Application Gateway létrehozása: háttérrendszer](../media/application-gateway-web-application-firewall-portal/application-gateway-create-backends.png)

4. A **háttérrendszer** lapon válassza a **Tovább: konfigurálás** lehetőséget.

### <a name="configuration-tab"></a>Konfiguráció lap

A **konfiguráció** lapon összekapcsolja az útválasztási szabály használatával létrehozott előtér-és háttér-készletet.

1. Válassza az útválasztási **szabály hozzáadása** lehetőséget az **útválasztási szabályok** oszlopban.

2. A megnyíló **útválasztási szabály hozzáadása** ablakban írja be a *MyRoutingRule* nevet a **szabály neveként**.

3. Egy útválasztási szabályhoz egy figyelő szükséges. Az **útválasztási szabály hozzáadása** ablak **figyelő** lapján adja meg az alábbi értékeket a figyelőhöz:

    - **Figyelő neve**: írja be a *myListener* nevet a figyelőnek.
    - Előtér **-IP**: válassza a **nyilvános** lehetőséget, hogy kiválassza a előtérhez létrehozott nyilvános IP-címet.
  
      Fogadja el az alapértelmezett értékeket a **figyelő** lapon a többi beállításnál, majd válassza a **háttérbeli célok** fület a többi útválasztási szabály konfigurálásához.

   ![Új Application Gateway létrehozása: figyelő](../media/application-gateway-web-application-firewall-portal/application-gateway-create-rule-listener.png)

4. A **háttérbeli célok** lapon válassza a **MyBackendPool** lehetőséget a **háttérbeli célként**.

5. A **http-beállításnál** válassza az **új hozzáadása** lehetőséget az új http-beállítás létrehozásához. A HTTP-beállítás határozza meg az útválasztási szabály viselkedését. A megnyíló **http-beállítás hozzáadása** ablakban írja be a *MyHTTPSetting* nevet a **http-beállítás neveként**. Fogadja el az alapértelmezett értékeket a további beállításokhoz a **http-beállítás hozzáadása** ablakban, majd válassza a **Hozzáadás** lehetőséget az **útválasztási szabály hozzáadása** ablakhoz való visszatéréshez. 

     ![Új Application Gateway létrehozása: HTTP-beállítás](../media/application-gateway-web-application-firewall-portal/application-gateway-create-httpsetting.png)

6. Az útválasztási szabály **hozzáadása** ablakban válassza a **Hozzáadás** lehetőséget az útválasztási szabály mentéséhez és a **konfiguráció** laphoz való visszatéréshez.

     ![Új Application Gateway létrehozása: útválasztási szabály](../media/application-gateway-web-application-firewall-portal/application-gateway-create-rule-backends.png)

7. Válassza a **Next (tovább** ) gombot, majd a **következőt: felülvizsgálat + létrehozás**.

### <a name="review--create-tab"></a>Felülvizsgálat + Létrehozás lap

Tekintse át a **felülvizsgálat + létrehozás** lapon található beállításokat, majd válassza a **Létrehozás** lehetőséget a virtuális hálózat, a nyilvános IP-cím és az Application Gateway létrehozásához. Az Azure az Application Gateway létrehozásához több percet is igénybe vehet. 

Várjon, amíg a telepítés sikeresen befejeződik, mielőtt továbblép a következő szakaszra.

## <a name="add-backend-targets"></a>Háttérbeli célok hozzáadása

Ebben a példában a virtuális gépeket célként használt háttérként fogja használni. Használhat meglévő virtuális gépeket, vagy újakat is létrehozhat. Hozzon létre két virtuális gépet, amelyeket az Azure háttér-kiszolgálóként használ az Application Gateway számára.

Ehhez a következőket kell tennie:

1. Hozzon létre két új virtuális gépet, *myVM* és *myVM2* a háttér-kiszolgálóként való használathoz.
2. Telepítse az IIS-t a virtuális gépeken annak ellenőrzéséhez, hogy az Application Gateway sikeresen létrejött-e.
3. Adja hozzá a háttér-kiszolgálókat a háttér-készlethez.

### <a name="create-a-virtual-machine"></a>Virtuális gép létrehozása

1. A Azure Portal válassza az **erőforrás létrehozása** lehetőséget. Megjelenik az **új** ablak.
2. Válassza a **Windows Server 2016 Datacenter** elemet a **népszerű** listában. Megjelenik a **virtuális gép létrehozása** lap.<br>Application Gateway átirányíthatja a forgalmat a háttér-készletben használt bármilyen típusú virtuális gépre. Ebben a példában egy Windows Server 2016 Datacenter rendszert használ.
3. Adja meg ezeket az értékeket az **alapok** lapon a következő virtuálisgép-beállításokhoz:

    - **Erőforráscsoport**: válassza ki a **myResourceGroupAG** az erőforráscsoport neveként.
    - **Virtuális gép neve**: írja be a *myVM* nevet a virtuális gép nevéhez.
    - **Felhasználónév**: adjon meg egy nevet a rendszergazda felhasználóneve számára.
    - **Password (jelszó**): adjon meg egy jelszót a rendszergazdai jelszóhoz.
4. Fogadja el a többi alapértelmezett értéket, majd válassza a **Next: Disks** elemet.  
5. Fogadja el a **lemezek** lap alapértelmezett értékeit, majd kattintson a **Tovább gombra: hálózatkezelés** elemre.
6. A **hálózatkezelés** lapon ellenőrizze, hogy a **virtuális hálózat** **myVNet** van-e kiválasztva, és az **alhálózat** **myBackendSubnet** értékre van-e állítva. Fogadja el a többi alapértelmezett értéket, majd válassza a **Tovább: kezelés** lehetőséget.<br>A Application Gateway képes kommunikálni a virtuális hálózaton kívüli példányokkal, de gondoskodnia kell az IP-kapcsolatról.
7. A **felügyelet** lapon **Tiltsa le** a **rendszerindítási diagnosztika** beállítást. Fogadja el a többi alapértelmezett értéket, majd válassza a **felülvizsgálat + létrehozás** lehetőséget.
8. A **felülvizsgálat + létrehozás** lapon tekintse át a beállításokat, javítsa ki az érvényesítési hibákat, majd válassza a **Létrehozás** lehetőséget.
9. A folytatás előtt várja meg, amíg a virtuális gép létrehozása befejeződik.

### <a name="install-iis-for-testing"></a>Az IIS telepítése teszteléshez

Ebben a példában az IIS-t csak akkor telepíti a virtuális gépekre, ha ellenőrzi, hogy az Azure sikeresen létrehozta-e az Application Gatewayt.

1. Nyissa meg [Azure PowerShell](../../cloud-shell/quickstart-powershell.md). Ehhez válassza ki a **Cloud Shell** elemet a Azure Portal felső navigációs sávján, majd válassza a **PowerShell** elemet a legördülő listából. 

    ![Egyéni bővítmény telepítése](../media/application-gateway-web-application-firewall-portal/application-gateway-extension.png)

2. Állítsa be a Location paramétert a környezethez, majd futtassa a következő parancsot az IIS telepítéséhez a virtuális gépen: 

    ```azurepowershell-interactive
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -ExtensionName IIS `
      -VMName myVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
      -Location EastUS
    ```

3. Hozzon létre egy második virtuális gépet, és telepítse az IIS-t a korábban befejezett lépések segítségével. Használja a *myVM2* a virtuális gép nevéhez és a **set-AzVMExtension** parancsmag **VMName** beállításához.

### <a name="add-backend-servers-to-backend-pool"></a>Háttér-kiszolgálók hozzáadása a háttérbeli készlethez

1. Válassza a **minden erőforrás** lehetőséget, majd válassza a **myAppGateway** lehetőséget.

2. Válassza ki a **háttér-készletek** elemet a bal oldali menüben.

3. Válassza a **myBackendPool** lehetőséget.

4. A **cél típusa** területen válassza a **virtuális gép** lehetőséget a legördülő listából.

5. A **cél** területen válassza ki a **myVM** társított hálózati adaptert a legördülő listából.
1. Ismételje meg a **myVM2**.

   :::image type="content" source="../media/application-gateway-web-application-firewall-portal/application-gateway-backend.png" alt-text="Háttérkiszolgálók hozzáadása":::


6. Kattintson a **Mentés** gombra.

7. Várjon, amíg a telepítés befejeződik, mielőtt továbblép a következő lépésre.

   
## <a name="create-and-link-a-web-application-firewall-policy"></a>Webalkalmazási tűzfal házirendjének létrehozása és csatolása

A WAF-testreszabások és-beállítások mindegyike egy külön objektumban található, amelyet WAF-szabályzatnak nevezünk. A szabályzatot társítani kell a Application Gatewayhoz. 

Hozzon létre egy alapszintű WAF szabályzatot egy felügyelt alapértelmezett szabálykészlet (DRS) létrehozásával.

1. A portál bal felső részén válassza az **erőforrás létrehozása** lehetőséget. Keresse meg a **WAF**, válassza a **webalkalmazási tűzfal** elemet, majd kattintson a **Létrehozás** gombra.
2. A **WAF házirend létrehozása** lap **alapok** lapján adja meg vagy válassza ki a következő adatokat, fogadja el az alapértelmezett értékeket a többi beállításnál, majd válassza a **felülvizsgálat + létrehozás**:

   |Beállítás  |Érték  |
   |---------|---------|
   |Házirend a következőhöz:     |Regionális WAF (Application Gateway)|
   |Előfizetés     |Adja meg az előfizetés nevét|
   |Erőforráscsoport     |**MyResourceGroupAG** kiválasztása|
   |Házirend neve     |Adjon egyedi nevet a WAF-házirendnek.|
1. Válassza a **Tovább: házirend-beállítások** elemet.
1. Fogadja el az alapértelmezett értékeket, majd válassza a **Tovább: felügyelt szabályok** elemet.
1. Fogadja el az alapértelmezett értéket, majd válassza a **Tovább: egyéni szabályok** elemet.
1. Válassza a **Tovább: társítás** lehetőséget.
1. Válassza a **társítás hozzáadása** lehetőséget, majd válassza a **Application Gateway** lehetőséget.
1. Jelölje be a **webalkalmazási tűzfal házirend-konfigurációjának alkalmazása jelölőnégyzetet, akkor is, ha az eltér az aktuális konfigurációtól**.
1. Válassza a **Hozzáadás** lehetőséget.
1. A **társítás** lapon válassza a **társítás hozzáadása** lehetőséget, majd válassza a **Application Gateway** lehetőséget.

   > [!NOTE]
   > Ha olyan házirendet rendel hozzá a Application Gatewayhoz (vagy figyelőhöz), amely már rendelkezik szabályzattal, a rendszer felülírja az eredeti szabályzatot, és felülírja az új házirendet.
4. Válassza a **Felülvizsgálat + létrehozás**, majd a **Létrehozás** lehetőséget.
1. Válassza a **Tovább: Címkék** lehetőséget.
1. Válassza a **Felülvizsgálat és létrehozás** lehetőséget.
1. Válassza a **Létrehozás** lehetőséget.

## <a name="test-the-application-gateway"></a>Az alkalmazásátjáró tesztelése

Bár az Application Gateway létrehozásához nem szükséges az IIS, telepítette annak ellenőrzéséhez, hogy az Azure sikeresen létrehozta-e az Application Gatewayt. Az IIS használata az Application Gateway teszteléséhez:

1. Keresse meg az Application Gateway nyilvános IP-címét az **Áttekintés** oldalon. ![ Az Application Gateway nyilvános IP-címének rögzítése](../media/application-gateway-web-application-firewall-portal/application-gateway-record-ag-address.png) 

   Vagy kijelölhet **minden erőforrást**, megadhatja a *myAGPublicIPAddress* kifejezést a keresőmezőbe, majd kiválaszthatja azt a keresési eredmények között. Az Azure megjeleníti a nyilvános IP-címet az **Áttekintés** oldalon.
1. Másolja a nyilvános IP-címet, majd illessze be a böngésző címsorába.
1. Keresse meg a választ. Egy érvényes válasz ellenőrzi, hogy az Application Gateway sikeresen létrejött-e, és hogy sikeresen tud-e kapcsolatot létesíteni a háttérrel.

   ![Az alkalmazásátjáró tesztelése](../media/application-gateway-web-application-firewall-portal/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha már nincs szüksége az Application Gateway használatával létrehozott erőforrásokra, távolítsa el az erőforráscsoportot. Az erőforráscsoport eltávolításával az Application Gateway és az ahhoz kapcsolódó összes erőforrást is eltávolítja. 

Az erőforráscsoport eltávolítása:

1. A Azure Portal bal oldali menüjében válassza az **erőforráscsoportok** lehetőséget.
2. Az **erőforráscsoportok** lapon keresse meg a **myResourceGroupAG** a listában, majd válassza ki azt.
3. Az **erőforráscsoport lapon** válassza az **erőforráscsoport törlése** elemet.
4. Írja be a *myResourceGroupAG* **nevet az erőforráscsoport neveként** , majd válassza a **Törlés** lehetőséget.

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [További információ a webalkalmazási tűzfalról](../overview.md)