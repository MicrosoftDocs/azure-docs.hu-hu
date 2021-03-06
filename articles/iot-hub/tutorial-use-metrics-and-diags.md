---
title: Oktatóanyag – Metrikák és naplók beállítása és használata Azure IoT Hubbal
description: Oktatóanyag – Megtudhatja, hogyan állíthat be és használhat metrikákat és naplókat egy Azure IoT Hubbal. Ez olyan adatokat biztosít, amelyek elemzésének segítségével diagnosztizálhatja a hub által esetleg okozott problémákat.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 10/29/2020
ms.author: robinsh
ms.custom:
- mvc
- mqtt
- devx-track-azurecli
- devx-track-csharp
ms.openlocfilehash: 96a9f7c50f3e30d86497c7a612ddda248db3f703
ms.sourcegitcommit: 2aeb2c41fd22a02552ff871479124b567fa4463c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/22/2021
ms.locfileid: "107865692"
---
# <a name="tutorial-set-up-and-use-metrics-and-logs-with-an-iot-hub"></a>Oktatóanyag: Metrikák és naplók beállítása és használata IoT Hubbal

A Azure Monitor gyűjtheti az IoT Hub metrikákat és naplókat, amelyek segítségével figyelheti a megoldás működését, és elháríthatja a problémákat azok előfordulásakor. Ebben a cikkben látni fogja, hogyan hozhat létre metrikákon alapuló diagramokat, hogyan hozhat létre metrikákat aktiváló riasztásokat, hogyan küldhet műveleteket és hibákat IoT Hub Azure Monitor-naplókba, és hogyan ellenőrizheti, hogy vannak-e hibák a naplókban.

Ez az oktatóanyag a [.NET Telemetria](quickstart-send-telemetry-dotnet.md) küldése rövid útmutatóBól származó Azure-mintát használja üzenetek az IoT Hubra való küldelmének. Az üzenetek küldését mindig használhatja egy eszközön vagy egy másik mintán, de előfordulhat, hogy ennek megfelelően módosítania kell néhány lépést.

Az oktatóanyag Azure Monitor előtt hasznos lehet a fogalmak ismerete. További információ: [Monitor IoT Hub.](monitor-iot-hub.md) További információ a monitorozási adatok referenciája IoT Hub által kibocsátott metrikákról és [erőforrásnaplókról.](monitor-iot-hub-reference.md)

Az oktatóanyagban az alábbi feladatokat fogja végrehajtani:

> [!div class="checklist"]
>
> * Az Azure CLI használatával létrehozhat egy IoT Hubot, regisztrálhat egy szimulált eszközt, és létrehozhat egy Log Analytics-munkaterületet.  
> * A IoT Hub és az eszköz telemetriai erőforrásnaplóit a Log Analytics-munkaterületen Azure Monitor naplókba.
> * A Metrikakezelővel létrehozhat egy diagramot a kiválasztott metrikák alapján, és rögzítheti azt az irányítópulton.
> * Hozzon létre metrikariasztásokat, így e-mailben értesítheti, ha fontos feltételek lépnek fel.
> * Letölthet és futtathat egy olyan alkalmazást, amely egy IoT-eszközt szimulál, és üzeneteket küld az IoT Hubnak.
> * A feltételek bekövetkeztének riasztásai megtekintése.
> * Tekintse meg a metrikadiagramot az irányítópulton.
> * Tekintse IoT Hub hibákat és műveleteket a Azure Monitor naplókban.

## <a name="prerequisites"></a>Előfeltételek

- Azure-előfizetés. Ha még nincs Azure-előfizetése, kezdés előtt hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

- A fejlesztői gépen .NET Core SDK 2.1-es vagy annál nagyobb virtuális gépre lesz szüksége. A .NET Core SDK-t többféle platformra a [.NET](https://dotnet.microsoft.com/download) oldaláról töltheti le.

  A C# aktuális verzióját a következő paranccsal ellenőrizheti a fejlesztői gépen:

  ```cmd/sh
  dotnet --version
  ```

- E-mail fogadására alkalmas e-mail-fiók.

- Győződjön meg arról, hogy a 8883-as port nyitva van a tűzfalon. Az oktatóanyagban található eszközminta MQTT protokollt használ, amely a 8883-as porton keresztül kommunikál. Előfordulhat, hogy egyes vállalati és oktatási hálózati környezetekben ez a port le van tiltva. További információ és a probléma megoldásának módjai: Csatlakozás IoT Hub [(MQTT) .](iot-hub-mqtt-support.md#connecting-to-iot-hub)

[!INCLUDE [azure-cli-prepare-your-environment-no-header.md](../../includes/azure-cli-prepare-your-environment-no-header.md)]

## <a name="set-up-resources"></a>Erőforrások beállítása

Ehhez az oktatóanyaghoz szüksége lesz egy IoT Hubra, egy Log Analytics-munkaterületre és egy szimulált IoT-eszközre. Ezek az erőforrások létrehozhatók az Azure CLI vagy az Azure PowerShell használatával. Ugyanazt az erőforráscsoportot és helyet használja minden erőforráshoz. Az oktatóanyag befejezése után egyetlen lépésben távolíthat el mindent az erőforráscsoport törlésével.

A szükséges lépések a következőek.

1. Hozzon létre [egy erőforráscsoportot.](../azure-resource-manager/management/overview.md)

2. IoT Hub létrehozása.

3. Egy Log Analytics-munkaterület létrehozása.

4. Eszközidentitás regisztrálása ahhoz a szimulált eszközhöz, amely üzeneteket küld az IoT Hubnak. Mentse az eszköz kapcsolati sztringet a szimulált eszköz konfigurálására.

### <a name="set-up-resources-using-azure-cli"></a>Erőforrások beállítása az Azure CLI használatával

Másolja és illessze be az alábbi szkriptet a Cloud Shellbe. A szolgáltatás azt feltételezi, hogy már bejelentkezett, és soronként futtatja a szkriptet. Egyes parancsok végrehajtása némi időt is vegyen el. Az új erőforrások a *ContosoResources erőforráscsoportban vannak létrehozva.*

Egyes erőforrások nevének egyedinek kell lennie az Azure-ban. A szkript véletlenszerű értéket hoz létre a függvény `$RANDOM` használatával, és egy változóban tárolja azt. Ezekhez az erőforrásokhoz a szkript hozzáfűzi ezt a véletlenszerű értéket az erőforrás alapnevével, így az erőforrás neve egyedi lesz.

Előfizetésenként csak egy ingyenes IoT Hub engedélyezett. Ha már rendelkezik ingyenes IoT Hubbal az előfizetésében, törölje azt a szkript futtatása előtt, vagy módosítsa a szkriptet úgy, hogy az ingyenes IoT Hubot vagy a standard vagy alapszintű szintet használó IoT Hub-t használja.

A szkript kinyomtatja az IoT Hub nevét, a Log Analytics-munkaterület nevét és a regisztrált eszköz kapcsolati sztringét. Ezeket jegyezze fel, mert később szüksége lesz rájuk ebben a cikkben.

```azurecli-interactive

# This is the IOT Extension for Azure CLI.
# You only need to install this the first time.
# You need it to create the device identity.
az extension add --name azure-iot

# Set the values for the resource names that don't have to be globally unique.
# The resources that have to have unique names are named in the script below
#   with a random number concatenated to the name so you can probably just
#   run this script, and it will work with no conflicts.
location=westus
resourceGroup=ContosoResources
iotDeviceName=Contoso-Test-Device
randomValue=$RANDOM

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique, so add a random number to the end.
iotHubName=ContosoTestHub$randomValue
echo "IoT hub name = " $iotHubName

# Create the IoT hub in the Free tier. Partition count must be 2.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --partition-count 2 \
    --sku F1 --location $location

# The Log Analytics workspace name must be globally unique, so add a random number to the end.
workspaceName=contoso-la-workspace$randomValue
echo "Log Analytics workspace name = " $workspaceName


# Create the Log Analytics workspace
az monitor log-analytics workspace create --resource-group $resourceGroup \
    --workspace-name $workspaceName --location $location

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName

# Retrieve the primary connection string for the device identity, then copy it to
#   Notepad. You need this to run the device simulation during the testing phase.
az iot hub device-identity show-connection-string --device-id $iotDeviceName \
    --hub-name $iotHubName

```

>[!NOTE]
>Az eszközidentitás létrehozásakor a következő hibaüzenet jelenhet meg: *A ContosoTestHub szabályzatának iothubowner IoT Hub jelenik meg.* A hiba kijavításhoz frissítse az Azure CLI IoT-bővítményt, majd futtassa újra az utolsó két parancsot a szkriptben. 
>
>Itt van a bővítmény frissítésére való parancs. Futtassa ezt a parancsot a Cloud Shell példányában.
>
>```cli
>az extension update --name azure-iot
>```

## <a name="collect-logs-for-connections-and-device-telemetry"></a>Kapcsolatok és eszköz-telemetria naplóinak gyűjtése

IoT Hub több működési kategóriához bocsát ki erőforrásnaplókat; Ahhoz azonban, hogy megtekintse ezeket a naplókat, létre kell hoznia egy diagnosztikai beállítást, amely elküldi őket a célhelyre. Az egyik ilyen Azure Monitor Log Analytics-munkaterületen gyűjtött naplók. IoT Hub erőforrásnaplók különböző kategóriákba vannak csoportosítva. A diagnosztikai beállításban kiválaszthatja, hogy mely kategóriákat Azure Monitor naplókba. Ebben a cikkben naplókat gyűjtünk az olyan műveletekről és hibákról, amelyek a kapcsolatokkal és az eszköz-telemetriával kapcsolatosak. Az erőforrásnaplókhoz támogatott kategóriák teljes IoT Hub lásd: [IoT Hub erőforrásnaplók.](monitor-iot-hub-reference.md#resource-logs)

Diagnosztikai beállítás létrehozásához, amely az erőforrásnaplókat IoT Hub naplókba Azure Monitor naplókba, kövesse az alábbi lépéseket:

1. Először is, ha még nincs a központon  a portálon, válassza az Erőforráscsoportok lehetőséget, majd a ContosoResources erőforráscsoportot. Válassza ki az IoT Hubot a megjelenő erőforrások listájából.

1. Keresse meg **a Figyelés** szakaszt a IoT Hub panelen. Válassza **a Diagnosztikai beállítások lehetőséget.** Ezután válassza **a Diagnosztikai beállítás hozzáadása lehetőséget.**

   :::image type="content" source="media/tutorial-use-metrics-and-diags/open-diagnostic-settings.png" alt-text="Képernyőkép a Monitorozás szakaszban található Diagnosztikai beállítások kiemelésével.":::

1. A **Diagnosztika beállításpanelen** adjon meg egy leíró nevet a beállításhoz, például: "Kapcsolatok és telemetria küldése a naplókba".

1. A **Kategória részletei alatt** válassza a Kapcsolatok **és** **eszköztelemetelemet.**

1. A **Destination details (Cél adatai)** alatt válassza a Send to Log Analytics (Küldés **a Log Analyticsbe)** lehetőséget, majd a Log Analytics-munkaterületválasztóval válassza ki a korábban feljegyzett munkaterületet. Ha végzett, a diagnosztikai beállításnak az alábbi képernyőképhez hasonlóan kell kinéznie:

   :::image type="content" source="media/tutorial-use-metrics-and-diags/add-diagnostic-setting.png" alt-text="Képernyőkép a diagnosztikai napló végső beállításairól.":::

1. A beállítások mentéséhez válassza a **Mentés** lehetőséget. Zárja be **a Diagnosztika beállításpanelt.** Az új beállítás megjelenik a diagnosztikai beállítások listájában.

## <a name="set-up-metrics"></a>Metrikák beállítása

Most a Metrikakezelővel létrehozunk egy diagramot, amely megjeleníti a követni kívánt metrikákat. Ezt a diagramot az alapértelmezett irányítópulton rögzítheti a Azure Portal.

1. Az IoT Hub bal oldali panelen válassza  a **Monitorozás szakaszban a Metrikák** lehetőséget.

1. A képernyő tetején válassza az **Elmúlt 24 óra (automatikus) lehetőséget.** A megjelenő legördülő menüben az Időtartomány beállításnál válassza az Elmúlt **4** óra lehetőséget, állítsa az Idő **részletességét**  **1** percre, az Idő megjelenítése mezőben pedig válassza a **Helyi** lehetőséget. A **beállítások mentéséhez** válassza az Alkalmaz lehetőséget. A beállításnál a következőnek **kell lennie: Helyi idő: Elmúlt 4 óra (1 perc).**

   :::image type="content" source="media/tutorial-use-metrics-and-diags/metrics-select-time-range.png" alt-text="Képernyőkép a metrikák időbeállításairól.":::

1. A diagramon látható egy részleges metrikabeállítás, amely az IoT Hub hatókörében jelenik meg. A Hatókör **és** a **Metrika névtér értékeit** hagyja az alapértelmezett értéken. Válassza ki **a Metrika** beállítást, írja be a "Telemetria" parancsot, majd válassza a **legördülő menüből** küldött telemetriai üzeneteket. **Az aggregáció** automatikusan a **Sum**(Összeg) lesz. Figyelje meg, hogy a diagram címe is megváltozik.

   :::image type="content" source="media/tutorial-use-metrics-and-diags/metrics-telemetry-messages-sent.png" alt-text="Képernyőkép a diagramnak küldött telemetriai üzenetek hozzáadásáról.":::

1. Most válassza **a Metrika hozzáadása** lehetőséget egy másik metrika diagramhoz való hozzáadásához. A **Metrika** alatt válassza a Total number of messages used (Használt **üzenetek teljes száma) lehetőséget.** **Az összesítés automatikusan** Avg **(Átlag)** lesz. Figyelje meg ismét, hogy a diagram címe megváltozott, hogy tartalmazza ezt a metrikát.

   A képernyőn most az elküldött *telemetriai* üzenetek minimális mérőszáma látható, valamint a használt üzenetek teljes száma új *metrikát.*

   :::image type="content" source="media/tutorial-use-metrics-and-diags/metrics-total-number-of-messages-used.png" alt-text="Képernyőkép a diagramhoz használt összes metrika hozzáadásáról.":::

1. A diagram jobb felső sarkában válassza a Rögzítés **az irányítópulton lehetőséget.**

   :::image type="content" source="media/tutorial-use-metrics-and-diags/metrics-total-number-of-messages-used-pin.png" alt-text="Képernyőkép a Rögzítés az irányítópulton gomb kiemelésről.":::

1. A Rögzítés **az irányítópulton panelen** válassza a Meglévő **lapot.** Válassza a **Privát,** **majd** az Irányítópult lehetőséget az Irányítópult legördülő menüből. Végül válassza a **Rögzítés lehetőséget** a diagram alapértelmezett irányítópultra való Azure Portal. Ha nem tűzi ki a diagramot egy irányítópultra, a rendszer nem őrzi meg a beállításokat, amikor kilép a Metrikáterből.

   :::image type="content" source="media/tutorial-use-metrics-and-diags/pin-to-dashboard.png" alt-text="Képernyőkép a Rögzítés az irányítópulton beállításról.":::

## <a name="set-up-metric-alerts"></a>Metrikákra vonatkozó riasztások beállítása

Most riasztásokat állíthatunk be két metrika aktiváló telemetriai üzenetei és a használt *üzenetek teljes száma alapján.* 

*Az elküldött telemetriai* üzenetek jól monitorozott metrikák, amelyek nyomon követik az üzenetek átviteli sebességét, és elkerülik a szabályozást. Az IoT Hub szinten a szabályozási korlát másodpercenként 100 üzenet. Egyetlen eszköz esetén nem tudjuk elérni ezt az átviteli sebességet, ezért ehelyett úgy fogjuk beállítani a riasztást, hogy akkor aktiválódik, ha az üzenetek száma 5 percen belül meghaladja az 1000-et. Éles környezetben a jelet nagyobb értékre állíthatja az IoT Hub rétege, kiadása és egységeinek száma alapján.

*A felhasznált üzenetek teljes száma* nyomon követi a napi üzenetek számát. Ez a metrika minden nap 00:00(UTC) időpontban áll alaphelyzetbe. Ha túllépi a napi kvótát egy adott küszöbértéken túl, a IoT Hub többé nem fogadja el az üzeneteket. Az IoT Hub szinten a napi üzenetkvóta 8000. Úgy fogjuk beállítani a riasztást, hogy akkor aktiválódik, ha az üzenetek teljes száma meghaladja a 4000-et, azaz a kvóta 50%-át. A gyakorlatban ezt a százalékos arányt valószínűleg magasabb értékre kellene állítani. A napi kvóta értéke az IoT Hub rétegtől, kiadástól és egységek számtól függ.

A kvóta- és szabályozási korlátokkal kapcsolatos további IoT Hub lásd: [Kvóták és szabályozás.](iot-hub-devguide-quotas-throttling.md)

Metrikariasztás beállítása:

1. Az IoT Hubra a következő Azure Portal.

1. A **Figyelés alatt** válassza a **Riasztások lehetőséget.** Ezután válassza az **Új riasztási szabály lehetőséget.**  Megnyílik **a Riasztási szabály létrehozása** panel.

    :::image type="content" source="media/tutorial-use-metrics-and-diags/create-alert-rule-pane.png" alt-text="A Riasztási szabály létrehozása panel képernyőképe.":::

    A **Riasztási szabály létrehozása panelen** négy szakasz látható:

    * **A** hatókör már be van állítva az IoT Hubra, ezért ezt a szakaszt nem hagyjuk ki.
    * **A Condition** beállítja a riasztást kiváltó jelet és feltételeket.
    * **A műveletek** konfigurálják, hogy mi történjen, ha a riasztás aktiválódik.
    * **A riasztási szabály részletei** lehetővé teszi a riasztás nevének és leírásának beállítását.

1. Először konfigurálja azt a feltételt, amely alapján a riasztás aktiválódik.

    1. A **Feltétel alatt** válassza a Feltétel hozzáadása **lehetőséget.** A **Jellogika konfigurálása panelen** írja be a "telemetria" elemet a keresőmezőbe, és válassza az **Elküldött telemetriai üzenetek lehetőséget.**

       :::image type="content" source="media/tutorial-use-metrics-and-diags/configure-signal-logic-telemetry-messages-sent.png" alt-text="A metrika kiválasztását bemutató képernyőkép.":::

    1. A **Jellogika konfigurálása panelen** állítsa be vagy erősítse meg a következő mezőket a Riasztási **logika területen** (a diagramot figyelmen kívül hagyhatja):

       **Küszöbérték:**  *Statikus*.

       **Operátor:** *Nagyobb, mint*.

       **Összesítés típusa:** *Összesen*.

       **Küszöbérték:** 1000.

       **Összesítés részletessége (Időszak):** *5 perc.*

       **Értékelés gyakorisága:** *1 percenként*

        :::image type="content" source="media/tutorial-use-metrics-and-diags/configure-signal-logic-set-conditions.png" alt-text="A riasztási feltételek beállításait bemutató képernyőkép.":::

       Ezek a beállítások az üzenetek teljes számát 5 percesre állítva állítják a jelet. A rendszer percenként kiértékeli ezt az összeget, és ha az előző 5 perc összege meghaladja az 1000 üzenetet, a riasztás aktiválódik.

       A **jellogika** mentéshez válassza a Kész lehetőséget.

1. Most konfigurálja a műveletet a riasztáshoz.

    1. A Riasztási szabály létrehozása panel **műveletek alatt** válassza a **Műveletcsoportok hozzáadása lehetőséget.** A **Riasztási szabályhoz csatolni kívánt** műveletcsoport kiválasztása panelen válassza a **Műveletcsoport létrehozása lehetőséget.**

    1. A **Műveletcsoport létrehozása** panel Alapvető beállítások **lapján** adjon meg egy nevet és egy megjelenítendő nevet a műveletcsoportnak.

        :::image type="content" source="media/tutorial-use-metrics-and-diags/create-action-group-basics.png" alt-text="A Műveletcsoport létrehozása panel Alapvető beállítások lapját bemutató képernyőkép.":::

    1. Válassza az **Értesítések** lapot. Az **Értesítés típusa mezőben** válassza az **E-mail/SMS-üzenet/Leküldés/Hang** lehetőséget a legördülő menüből. Megnyílik **az E-mail/SMS-üzenet/Leküldés/Hang** panel.

    1. Az **E-mail/SMS-üzenet/Leküldés/Hang** panelen válassza az e-mail lehetőséget, adja meg az e-mail-címét, majd kattintson az **OK gombra.**

        :::image type="content" source="media/tutorial-use-metrics-and-diags/set-email-address.png" alt-text="Képernyőkép az e-mail-cím beállításról.":::

    1. Az Értesítések **panelre visszatérve** adja meg az értesítés nevét.

        :::image type="content" source="media/tutorial-use-metrics-and-diags/create-action-group-notification-complete.png" alt-text="Képernyőkép a befejezett értesítések panelről.":::

    1. (Nem kötelező) Ha a **Műveletek lapot,** majd  a Művelet típusa legördülő menüt választja, láthatja a riasztással aktiválható műveleteket. Ebben a cikkben csak értesítéseket fogunk használni, így figyelmen kívül hagyhatja a lap alatti beállításokat.

        :::image type="content" source="media/tutorial-use-metrics-and-diags/action-types.png" alt-text="A Műveletek panelen elérhető művelettípusokat bemutató képernyőkép.":::

    1. Válassza az **Áttekintés és létrehozás lapot,** ellenőrizze a beállításokat, majd válassza a **Létrehozás lehetőséget.**

        :::image type="content" source="media/tutorial-use-metrics-and-diags/create-action-group-review-and-create.png" alt-text="Az Áttekintés és létrehozás panel képernyőképe.":::

    1. A **Riasztási szabály létrehozása panelre** visszatérve figyelje meg, hogy az új műveletcsoport hozzá lett adva a riasztáshoz szükséges műveletekhez.  

1. Végül konfigurálja a riasztási szabály részleteit, és mentse a riasztási szabályt.

    1. A **Riasztási szabály létrehozása panel** Riasztási szabály részletei alatt adja meg a riasztás nevét és leírását; Például: "Riasztás 1000-esnél több üzenet esetén 5 percen keresztül". Győződjön meg arról, **hogy a Riasztási szabály engedélyezése létrehozáskor beállítás be** van jelölve. A kész riasztási szabály az alábbi képernyőképhez hasonlóan fog kinézni.

        :::image type="content" source="media/tutorial-use-metrics-and-diags/create-alert-rule-final.png" alt-text="A kész Riasztási szabály létrehozása panel képernyőképe.":::

    1. Az **új szabály mentéshez válassza a** Riasztási szabály létrehozása lehetőséget.

1. Most állítson be egy újabb riasztást a *használt üzenetek teljes számára.* Ez a metrika akkor hasznos, ha riasztást szeretne küldeni, ha a használt üzenetek száma megközelíti az IoT Hub napi kvótáját, és az IoT Hub elkezdi elutasítani az üzeneteket. Kövesse a korábban tett lépéseket az alábbi eltérésekkel.

    * A Jellogika konfigurálása **panelen** található jelhez válassza a **Használt üzenetek teljes száma lehetőséget.**

    * A **Jellogika konfigurálása panelen** állítsa be vagy erősítse meg a következő mezőket (figyelmen kívül hagyhatja a diagramot):

       **Küszöbérték:**  *Statikus*.

       **Operátor:** *Nagyobb, mint*.

       **Összesítés típusa:** *Maximum*.

       **Küszöbérték:** 4000.

       **Összesítés részletessége (időszak):** *1 perc.*

       **Értékelés gyakorisága:** *1 percenként*

       Ezek a beállítások úgy állítják a jelet, hogy akkor jelezzen, ha az üzenetek száma eléri a 4000-et. A rendszer percenként kiértékeli a metrikát.

    * Amikor megadja a riasztási szabályhoz szükséges műveletet, csak válassza ki a korábban létrehozott műveletcsoportot.

    * A riasztás részleteihez válasszon egy másik nevet és leírást, mint korábban.

1. Az IoT  Hub bal oldali panelének Figyelés részen válassza a Riasztások lehetőséget. Most válassza **a Riasztási szabályok kezelése** lehetőséget a Riasztások panel felső **menüjében.** Megnyílik **a Szabályok** panel. Most már látnia kell a két riasztást:

   :::image type="content" source="media/tutorial-use-metrics-and-diags/rules-management.png" alt-text="Képernyőkép a Szabályok panelről az új riasztási szabályokkal.":::

1. Zárja be **a Szabályok panelt.**

Ezekkel a beállításokkal egy riasztás aktiválódik, és e-mail-értesítést kap, ha 5 percen belül több mint 1000 üzenetet küld, és ha a felhasznált üzenetek teljes száma meghaladja a 4000-et (az ingyenes szinten található IoT Hub napi kvótáinak 50%-a).

## <a name="run-the-simulated-device-app"></a>A szimulált eszközalkalmazás futtatása

Az Erőforrások [beállítása szakaszban](#set-up-resources) regisztrált egy eszközidentitást, amely egy IoT-eszköz használatával szimulálható. Ebben a szakaszban egy .NET-konzolalkalmazást tölt le, amely egy eszközt szimulál, amely az eszközről a felhőbe küld üzeneteket egy IoT Hub-nek, konfigurálja úgy, hogy ezeket az üzeneteket az IoT Hubra küldje, majd futtassa.

> [!IMPORTANT]
>
> A riasztások teljes konfigurálása és engedélyezése akár 10 percet is IoT Hub. Várjon legalább 10 percet az utolsó riasztás konfigurálása és a szimulált eszközalkalmazás futtatása között.

Az [IoT-eszköz szimulációjára](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) szolgáló megoldás letöltése. Ez a hivatkozás letölt egy olyan repo-t, amely több alkalmazást is ad; A keresett eszköz az iot-hub/Quickstarts/simulated-device/.

1. Egy helyi terminálablakban keresse meg a megoldás gyökérmappát. Ezután lépjen az **iot-hub\Quickstarts\simulated-device** mappába.

1. Nyissa meg a **SimulatedDevice.cs** fájlt egy Ön által választott szövegszerkesztőben.

    1. Cserélje le a változó értékét arra az eszközkapcsolati sztringre, amely az erőforrások beállítását a szkript futtatásakor `s_connectionString` feljegyzett.

    1. A metódusban módosítsa az 1000-ről 1-re, ami csökkenti az üzenetek küldése közötti időt 1 másodpercről `SendDeviceToCloudMessagesAsync` `Task.Delay` 0,001 másodpercre. A késés lerövidítése növeli az elküldött üzenetek számát. (Valószínűleg nem fog másodpercenként 100 üzenetet kapni.)

        ```csharp
        await Task.Delay(1);
        ```

    1. Mentse a **SimulatedDevice.cs módosításait.**

1. A szimulálteszköz-alkalmazáshoz szükséges csomagok telepítéséhez futtassa a következő parancsot a helyi terminálablakban:

    ```cmd/sh
    dotnet restore
    ```

1. Futtassa az alábbi parancsot a helyi terminálablakban a szimulálteszköz-alkalmazás létrehozásához és futtatásához:

    ```cmd/sh
    dotnet run
    ```

    A következő képernyőképen az a kimenet látható, amikor a szimulálteszköz-alkalmazás telemetriát küld az IoT Hubnak:

    :::image type="content" source="media/tutorial-use-metrics-and-diags/simulated-device-output.png" alt-text="Képernyőkép a szimulált eszköz kimenetéről.":::

Hagyja, hogy az alkalmazás legalább 10–15 percig fusson. Ideális esetben hagyja futni, amíg le nem áll az üzenetek küldése (körülbelül 20–30 perc). Ez akkor fordul elő, ha túllépte az IoT Hub napi üzenetkvótáját, és már nem fogad több üzenetet.

> [!NOTE]
> Ha az eszközalkalmazást hosszabb ideig hagyja futni, miután az már nem küld üzeneteket, kivételt okozhat. Nyugodtan figyelmen kívül hagyhatja ezt a kivételt, és bezárhatja az alkalmazásablakot.

## <a name="view-metrics-chart-on-your-dashboard"></a>Metrikadiagram megtekintése az irányítópulton

1. A képernyő bal felső sarkában Azure Portal a portál menüjét, majd válassza az Irányítópult **lehetőséget.**

   :::image type="content" source="media/tutorial-use-metrics-and-diags/select-dashboard.png" alt-text="Képernyőkép az irányítópult kiválasztásáról.":::

1. Keresse meg a korábban rögzített diagramot, és kattintson a csempe bármely útjára a diagramadatokon kívül annak kibontásához. Megjeleníti az elküldött telemetriai üzeneteket és a diagramon használt üzenetek teljes számát. A legutóbbi számok a diagram alján jelennek meg. A kurzort a diagramon mozgatva adott időpontokban láthatja a metrikaértékeket. A diagram tetején módosíthatja az időértéket és a részletességet is, így leszűkítheti vagy kibővítheti az adatokat egy adott időszakra.

   :::image type="content" source="media/tutorial-use-metrics-and-diags/metrics-on-dashboard-last-hour.png" alt-text="Képernyőkép a metrikadiagramról.":::

   Ebben a forgatókönyvben a szimulált eszköz üzenetátvitele nem elég nagy ahhoz, hogy IoT Hub az üzeneteket. Egy olyan forgatókönyvben, amely ténylegesen szabályozást tartalmaz, előfordulhat, hogy az elküldött telemetriai üzenetek egy korlátozott ideig túllépik az IoT Hub szabályozási korlátját. Ennek az a fontos, hogy alkalmazkodni tud a forgalom megnomlott forgalmához. Részletekért lásd a [forgalomalakítást.](iot-hub-devguide-quotas-throttling.md#traffic-shaping)

## <a name="view-the-alerts"></a>A riasztások megtekintése

Ha az elküldött üzenetek száma meghaladja a riasztási szabályokban beállított korlátokat, elkezd e-mailes riasztásokat kapni.

Ha látnia kell, hogy vannak-e  aktív riasztások, válassza a Figyelés lehetőséget az IoT Hub bal oldali panelen.  A **Riasztások** panel a riasztások számát jeleníti meg súlyosság szerint rendezve a megadott időtartományban.

   :::image type="content" source="media/tutorial-use-metrics-and-diags/view-alerts.png" alt-text="A riasztások összegzését bemutató képernyőkép.":::

Válassza ki a 3. súlyosságú sort. Megnyílik **a Minden riasztás** panel, és kilistázza az Sev 3 riasztást, amelyek el vannak bocsátva.

   :::image type="content" source="media/tutorial-use-metrics-and-diags/view-all-alerts.png" alt-text="A Minden riasztás panel képernyőképe.":::

Válassza ki az egyik riasztást a riasztás részleteinek kiválasztásához.

   :::image type="content" source="media/tutorial-use-metrics-and-diags/view-individual-alert.png" alt-text="Képernyőkép a riasztás részleteiről.":::

Ellenőrizze, hogy van-e e-mail-e-mail a Microsoft Azure. A tárgysor az aktivált riasztást írja le. Például: *Azure: Aktivált súlyosság: 3 Riasztás 5 perc alatt több mint 1000 üzenet esetén.* A törzs az alábbi képen láthatóhoz hasonlóan fog kinézni:

   :::image type="content" source="media/tutorial-use-metrics-and-diags/alert-mail.png" alt-text="Képernyőkép az e-mailről, amely azt mutatja, hogy a riasztások el vannak küldve.":::

## <a name="view-azure-monitor-logs"></a>Naplók Azure Monitor megtekintése

A [Kapcsolatok és](#collect-logs-for-connections-and-device-telemetry) eszköz-telemetria naplóinak gyűjtése szakaszban létrehozott egy diagnosztikai beállítást, amely az IoT Hub által kibocsátott erőforrásnaplókat továbbítja a kapcsolati és eszköz-telemetriaműveleteket a Azure Monitor naplókba. Ebben a szakaszban kusto-lekérdezést fog futtatni a Azure Monitor naplókban, hogy megfigyelje az esetlegesen előforduló hibákat.

1. Az **IoT** Hub bal oldali panelének Figyelés Azure Portal válassza a **Naplók lehetőséget.** Ha megnyílik, **zárja** be a kezdeti Lekérdezések ablakot.

1. Az Új lekérdezés panelen válassza a **Lekérdezések** lapot, majd bontsa ki a IoT Hub **az** alapértelmezett lekérdezések listájának listájához.

   :::image type="content" source="media/tutorial-use-metrics-and-diags/new-query-pane.png" alt-text="Az alapértelmezett IoT Hub képernyőképe.":::

1. Válassza ki *a Hibaösszegzés lekérdezést.* A lekérdezés megjelenik a Lekérdezésszerkesztő panelen. Válassza **a Futtatás** lehetőséget a szerkesztőpanelen, és figyelje meg a lekérdezés eredményeit. Bontsa ki az egyik sort a részletekért.

   :::image type="content" source="media/tutorial-use-metrics-and-diags/logs-errors.png" alt-text="Képernyőkép az Errors summary lekérdezés által visszaadott naplókról.":::

   > [!NOTE]
   > Ha nem lát hibát, próbálja meg lefutni a *Legutóbb csatlakoztatott eszközök lekérdezést.* Ennek egy sort kell visszaadni a szimulált eszközhöz.

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Az oktatóanyagban létrehozott összes erőforrás eltávolításához törölje az erőforráscsoportot. Ez a művelet törli a csoportban lévő összes erőforrást. Ebben az esetben eltávolítja az IoT Hubot, a Log Analytics-munkaterületet és magát az erőforráscsoportot. Ha rögzített metrikák diagramokat az irányítópulton, manuálisan kell eltávolítania őket. Kattintson az egyes diagramok jobb felső sarkában található három pontra, majd válassza az Eltávolítás **gombra.** A diagramok törlése után mentse a módosításokat.

Az erőforráscsoport az [az group delete](/cli/azure/group#az_group_delete) paranccsal távolítható el.

```azurecli-interactive
az group delete --name ContosoResources
```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban megtanulta, hogyan használhatja IoT Hub metrikákat és naplókat a következő feladatok elvégzésével:

> [!div class="checklist"]
>
> * Az Azure CLI használatával létrehozhat egy IoT Hubot, regisztrálhat egy szimulált eszközt, és létrehozhat egy Log Analytics-munkaterületet.  
> * Küldjön IoT Hub és az eszköz telemetriai erőforrásnaplóit a Log Analytics Azure Monitor naplóiba.
> * A Metrikakezelővel létrehozhat egy diagramot a kiválasztott metrikák alapján, és rögzítheti azt az irányítópulton.
> * Hozzon létre metrikariasztásokat, így e-mailben értesítheti, ha fontos feltételek lépnek fel.
> * Letölthet és futtathat egy olyan alkalmazást, amely egy IoT-eszközt szimulál, és üzeneteket küld az IoT Hubnak.
> * A feltételek bekövetkeztének riasztásai megtekintése.
> * Tekintse meg a metrikadiagramot az irányítópulton.
> * Tekintse IoT Hub hibákat és műveleteket a Azure Monitor naplókban.

A következő oktatóanyag az IoT-eszközök állapotának kezelését mutatja be. 

> [!div class="nextstepaction"]
> [Eszközök konfigurálása háttérszolgáltatásból](tutorial-device-twins.md)