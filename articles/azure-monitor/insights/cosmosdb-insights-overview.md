---
title: Azure Cosmos DB figyelése Azure Monitorekkel Cosmos DBhoz | Microsoft Docs
description: Ez a cikk a Cosmos DB funkció Azure Monitor ismerteti, amely Cosmos DB tulajdonosokat biztosít a CosmosDB-fiókokkal kapcsolatos teljesítmény-és kihasználtsági problémák gyors megismeréséhez.
author: lgayhardt
ms.author: lagayhar
ms.topic: conceptual
ms.date: 05/11/2020
ms.openlocfilehash: d88bf65f1bd94e29bd9f60f5597d655f0040623b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/29/2021
ms.locfileid: "101725762"
---
# <a name="explore-azure-monitor-for-azure-cosmos-db"></a>Azure Cosmos DB Azure Monitor megismerése

A Azure Cosmos DB Azure Monitor az egységes interaktív felhasználói felület összes Azure Cosmos DB erőforrásának teljes teljesítményét, hibáit, kapacitását és működési állapotát jeleníti meg. Ez a cikk segítséget nyújt az új figyelési élmény előnyeinek megismeréséhez, valamint arról, hogy miként módosíthatja és igazíthatja a felhasználói élményt a szervezet egyedi igényeinek megfelelően.   

## <a name="introduction"></a>Bevezetés

A tapasztalatok megismerése előtt meg kell ismernie, hogyan jeleníti meg és jeleníti meg az információkat. 

A következőket biztosítja:

* A Azure Cosmos DB erőforrások **méretezési perspektívája** egyetlen helyen az összes előfizetéshez, és a szelektív hatókör csak azokra az előfizetésekre és erőforrásokra vonatkozik, amelyeket érdekel.

* Egy adott Azure CosmosDB-erőforrás **elemzésének részletezése** a problémák diagnosztizálásához, illetve a kategória-kihasználtság, a hibák, a kapacitás és a műveletek részletes elemzésének elvégzéséhez. Bármelyik lehetőség kiválasztásával részletesen áttekintheti a vonatkozó Azure Cosmos DB metrikákat.  

* **Testreszabható** – ez a felület Azure monitor munkafüzet-sablonokra épül, így módosíthatja, hogy milyen mérőszámok jelenjenek meg, módosíthatók vagy állíthatók be a határértékekhez igazodó küszöbértékek, majd egy egyéni munkafüzetbe menthetők. A munkafüzetek diagramjai ezután rögzíthetők az Azure-irányítópultokon.  

Ez a funkció nem igényli, hogy bármit engedélyezzen vagy konfiguráljan, ezek a Azure Cosmos DB-metrikák gyűjtése alapértelmezés szerint történik.

>[!NOTE]
>A szolgáltatáshoz való hozzáférés díjmentes, és a [Azure monitor díjszabása](https://azure.microsoft.com/pricing/details/monitor/) lapon leírtak szerint csak az Ön által konfigurált vagy engedélyezett Azure monitor alapvető funkciókért kell fizetnie.

## <a name="view-utilization-and-performance-metrics-for-azure-cosmos-db"></a>A Azure Cosmos DB kihasználtságának és teljesítményének metrikáinak megtekintése

Ha szeretné megtekinteni a Storage-fiókok kihasználtságát és teljesítményét az összes előfizetésében, hajtsa végre a következő lépéseket.

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).

2. Keressen rá a **figyelőre** , és válassza a **figyelő** lehetőséget.

    ![A "monitor" szót tartalmazó keresőmező és egy olyan legördülő lista, amely egy sebességmérő stílusú képpel rendelkező szolgáltatásokat mutat](./media/cosmosdb-insights-overview/search-monitor.png)

3. Válassza a **Cosmos db** lehetőséget.

    ![Képernyőkép a Cosmos DB áttekintése munkafüzetről](./media/cosmosdb-insights-overview/cosmos-db.png)

### <a name="overview"></a>Áttekintés

Az **Áttekintés** oldalon a táblázat interaktív Azure Cosmos db metrikákat jelenít meg. Az eredményeket az alábbi legördülő listából kiválasztott beállítások alapján szűrheti:

* **Előfizetések** – csak a Azure Cosmos db erőforrással rendelkező előfizetések jelennek meg.  

* **Cosmos db** – kiválaszthatja az összes, egy részhalmaz vagy egy Azure Cosmos db erőforrást.

* **Időtartomány** – alapértelmezés szerint az utolsó 4 órányi információt jeleníti meg a megfelelő beállítások alapján.

A legördülő lista alatti számláló csempe a kijelölt előfizetésekben lévő Azure Cosmos DB erőforrások teljes számát összesíti. A munkafüzet oszlopaihoz feltételes színkódolás vagy intenzitástérképei van a tranzakciós metrikák jelentéséhez. A legmélyebb szín a legmagasabb és a világosabb szín a legalacsonyabb értékeken alapul. 

Ha az egyik Azure Cosmos DB melletti legördülő nyílra kattint, a teljesítmény-metrikák részletezése az egyes adatbázis-tárolók szintjén történik:

![Kibontott legördülő lista az egyes adatbázis-tárolók és a hozzájuk kapcsolódó teljesítmény-bontás megjelenítéséhez](./media/cosmosdb-insights-overview/container-view.png)

Ha a kék színnel jelölt Azure Cosmos DB erőforrást választja, akkor a társított Azure Cosmos DB-fiók alapértelmezett **áttekintését** fogja végrehajtani. 

### <a name="failures"></a>Hibák

Válassza ki a **hibák** elemet az oldal tetején, és megnyílik a munkafüzet sablonjának **hibák** szakasza. Az összes kérést megjeleníti a kérelmeket alkotó válaszok eloszlásával:

![Képernyőfelvétel a HTTP-kérelem típusa szerinti bontásban fellépő hibákról](./media/cosmosdb-insights-overview/failures.png)

| Code |  Leírás       | 
|-----------|:--------------------|
| `200 OK`  | A következő REST-műveletek egyike sikeres volt: </br>– Erőforrás lekérése. </br> -Erőforrásra kerül. </br> – KÖZZÉTÉTEL egy erőforráson. </br> – KÖZZÉTÉTEL a tárolt eljárási erőforráson a tárolt eljárás végrehajtásához.|
| `201 Created` | Az erőforrás-létrehozás utáni művelet sikeres. |
| `404 Not Found` | A művelet olyan erőforráson próbálkozik, amely már nem létezik. Előfordulhat például, hogy az erőforrás már törölve lett. |

Az állapotkódok teljes listájáért olvassa el a [Azure Cosmos db http-állapotkód című cikket](/rest/api/cosmos-db/http-status-codes-for-cosmosdb).

### <a name="capacity"></a>Kapacitás

Válassza ki a **kapacitás** elemet az oldal tetején, és megnyílik a munkafüzet-sablon **kapacitás** része. Itt láthatja, hogy hány dokumentum van, a dokumentum az idő múlásával, az adatok kihasználtságával és a rendelkezésre álló tárterület teljes mennyiségétől számítva.  Ez segítséget nyújt a lehetséges tárolási és adatkihasználtsági problémák azonosításához.

![Kapacitás munkafüzet](./media/cosmosdb-insights-overview/capacity.png) 

Az áttekintő munkafüzethez hasonlóan az **előfizetés** oszlopban egy Azure Cosmos db erőforrás melletti legördülő lista is megjelenik, amely az adatbázist alkotó egyes tárolók részletezését mutatja.

### <a name="operations"></a>Üzemeltetés

Válassza a lap tetején a **műveletek** lehetőséget, majd megnyílik a munkafüzet sablonjának **műveletek** rész. Lehetővé teszi, hogy a kérések típusa szerinti bontásban megtekintse a kérelmeket.

Tehát az alábbi példában láthatja, hogy `eastus-billingint` az olvasási kérelmeket elsődlegesen fogadja, de kis számú upsert és a létrehozási kéréseket. Míg a `westeurope-billingint` kérelem szempontjából csak olvasható, legalább az elmúlt négy órában, hogy a munkafüzet aktuális hatóköre a Time Range paraméterén keresztül történjen.

![Műveleti munkafüzet](./media/cosmosdb-insights-overview/operation.png)

## <a name="view-from-an-azure-cosmos-db-resource"></a>Megtekintés Azure Cosmos DB erőforrásból

1. Keresse meg vagy válassza ki a meglévő Azure Cosmos DB fiókokat.

:::image type="content" source="./media/cosmosdb-insights-overview/cosmosdb-search.png" alt-text="Azure Cosmos DB keresése." border="true":::

2. Miután megnyitotta a Azure Cosmos DB fiókját, a figyelés szakaszban válassza az elemzések **(előzetes verzió)** vagy a **munkafüzetek** lehetőséget az átviteli sebesség, a kérelmek, a tárolás, a rendelkezésre állás, a késés, a rendszer és a fiókok kezelésének további elemzéséhez.

:::image type="content" source="./media/cosmosdb-insights-overview/cosmosdb-overview.png" alt-text="Cosmos DB információk áttekintése." border="true":::

### <a name="time-range"></a>Időtartomány

Alapértelmezés szerint az **időtartomány** mező az **elmúlt 24 órában** jeleníti meg az adatait. Az időtartományt úgy módosíthatja, hogy az utolsó 5 perctől az elmúlt hét napig bárhol megjelenjen az adatok. Az időtartomány-választó olyan **Egyéni** üzemmódot is tartalmaz, amely lehetővé teszi a kezdő/befejező dátumok beírását a kiválasztott fiók rendelkezésre álló adatokon alapuló egyéni időkeretének megtekintéséhez.

:::image type="content" source="./media/cosmosdb-insights-overview/cosmosdb-time-range.png" alt-text="Cosmos DB időtartomány." border="true":::

### <a name="insights-overview"></a>Az információk áttekintése

Az **Áttekintés** lapon a kiválasztott Azure Cosmos db fiók leggyakoribb metrikái láthatók, többek között a következők:

* Összes kérelem
* Sikertelen kérelmek (429s)
* Normalizált RU-felhasználás (max.)
* Adatok & indexelési használat
* Fiók metrikáinak Cosmos DB gyűjtemény szerint

**Kérelmek összesen:** Ez a gráf a fiókra vonatkozó összes kérést megjeleníti, állapotkód szerint lebontva. A gráf alján lévő egységek az adott időszakra vonatkozó összes kérelem összege.

:::image type="content" source="./media/cosmosdb-insights-overview/cosmosdb-total-requests.png" alt-text="Cosmos DB összes kérelem gráfja." border="true":::

**Sikertelen kérelmek (429s)**: Ez a gráf a sikertelen kérelmeket a 429-es állapotkód szerint jeleníti meg. A gráf alján lévő egységek az adott időszakra vonatkozó összes sikertelen kérelem összege.

:::image type="content" source="./media/cosmosdb-insights-overview/cosmosdb-429.png" alt-text="Cosmos DB sikertelen kérelmek gráfja." border="true":::

**Normalizált ru-használat (max.)**: Ez a diagram a megadott időszakra vonatkozóan a NORMALIZÁLt ru-fogyasztási egységek 0-100%-a maximális százalékos arányát adja meg.

:::image type="content" source="./media/cosmosdb-insights-overview/cosmosdb-normalized-ru.png" alt-text="Cosmos DB normalizált RU-felhasználás." border="true":::

## <a name="pin-export-and-expand"></a>PIN-kód, exportálás és Kibontás

A metrikus szakaszok bármelyikét rögzítheti egy [Azure-irányítópultra](../../azure-portal/azure-portal-dashboards.md) , ha a szakasz jobb felső sarkában található gombostű ikonra kattint.

![Metrikus szakasz rögzítése az irányítópulton – példa](./media/cosmosdb-insights-overview/pin.png)

Az Excel formátumba való exportáláshoz kattintson a gombostű ikon bal oldalán található lefelé mutató nyíl ikonra.

![Munkafüzet exportálása ikon](./media/cosmosdb-insights-overview/export.png)

A munkafüzet összes legördülő nézetének kibontásához vagy összecsukásához válassza az Exportálás ikon bal oldalán található Kibontás ikont:

![Munkafüzet kibontása ikon](./media/cosmosdb-insights-overview/expand.png)

## <a name="customize-azure-monitor-for-azure-cosmos-db"></a>Azure Cosmos DB Azure Monitor testreszabása

Mivel ez a felület Azure monitor munkafüzet-sablonokra épül, **testreszabhatja** a  >   módosítást, és **mentheti** a módosított verzió egy példányát egy egyéni munkafüzetbe. 

![Sáv testreszabása](./media/cosmosdb-insights-overview/customize.png)

A munkafüzetek egy erőforráscsoporthoz lesznek mentve, vagy az Ön számára magánjellegű **saját jelentések** szakaszban, vagy a **megosztott jelentések** szakaszban, amely mindenki számára elérhető az erőforráscsoporthoz való hozzáféréssel. Az Egyéni munkafüzet mentése után nyissa meg a munkafüzet-katalógust, és indítsa el.

![A munkafüzet-katalógus indítása a parancssáv alapján](./media/cosmosdb-insights-overview/gallery.png)

## <a name="troubleshooting"></a>Hibaelhárítás

A hibaelhárítással kapcsolatos útmutatásért tekintse meg a dedikált munkafüzet-alapú információkkal [kapcsolatos hibaelhárítási cikket](troubleshoot-workbooks.md).

## <a name="next-steps"></a>Következő lépések

* A [metrikai riasztások](../alerts/alerts-metric.md) és a [szolgáltatás állapotára vonatkozó értesítések](../../service-health/alerts-activity-log-service-notifications-portal.md) konfigurálása automatizált riasztások beállításához a problémák észlelése érdekében.

* Ismerkedjen meg a forgatókönyvekkel, amelyek támogatják az új és a meglévő jelentések testreszabását, valamint az [interaktív jelentések Azure monitor-munkafüzetekkel való létrehozását](../visualize/workbooks-overview.md)ismertető áttekintést.
