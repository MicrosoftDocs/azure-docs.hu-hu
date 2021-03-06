---
title: Rendszerváltozók a Azure Data Factoryban
description: Ez a cikk a Azure Data Factory által támogatott rendszerváltozókat ismerteti. Ezeket a változókat kifejezésekben használhatja Data Factory entitások definiálásához.
author: dcstwh
ms.author: weetok
ms.reviewer: jburchel
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/12/2018
ms.openlocfilehash: b85efa7ac4481ab9eb2b2637aee7d9e5e76e8f3f
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/30/2021
ms.locfileid: "104786056"
---
# <a name="system-variables-supported-by-azure-data-factory"></a>Azure Data Factory által támogatott rendszerváltozók

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Ez a cikk a Azure Data Factory által támogatott rendszerváltozókat ismerteti. Ezeket a változókat kifejezésekben használhatja Data Factory entitások definiálásához.

## <a name="pipeline-scope"></a>Folyamat hatóköre

Ezeket a rendszerváltozókat a folyamat JSON-ban bárhol lehet hivatkozni.

| Változó neve | Leírás |
| --- | --- |
| @pipeline(). DataFactory |Annak az adatelőállítónak a neve, amelyen a folyamat fut |
| @pipeline(). Folyamat |A folyamat neve |
| @pipeline(). RunId |Az adott folyamat futtatásának azonosítója |
| @pipeline(). TriggerType |Az adatfolyamatot meghívó eseményindító típusa (például `ScheduleTrigger` , `BlobEventsTrigger` ). A támogatott eseményindító-típusok listáját lásd: [folyamat-végrehajtás és eseményindítók Azure Data Factoryban](concepts-pipeline-execution-triggers.md). Az trigger típusa `Manual` azt jelzi, hogy a folyamat manuálisan lett aktiválva. |
| @pipeline(). TriggerId|A folyamatot meghívó eseményindító azonosítója |
| @pipeline(). TriggerName|A folyamatot meghívó Eseményindító neve |
| @pipeline(). TriggerTime|Az eseményindító futtatásának ideje, amely meghívja a folyamatot. Ez az az idő, amikor az eseményindító **ténylegesen** elindította a folyamat futását, és az eseményindító ütemezett idejétől némileg eltérő lehet.  |

>[!NOTE]
>Az triggerrel kapcsolatos dátum/idő rendszerváltozók (a folyamat és az aktiválási hatókörök esetében) az UTC dátumokat ISO 8601 formátumban adják vissza, például: `2017-06-01T22:20:00.4061448Z` .

## <a name="schedule-trigger-scope"></a>Trigger hatókörének ütemterve

Ezeket a rendszerváltozókat az eseményindító JSON-ban bárhol lehet hivatkozni [ScheduleTrigger](concepts-pipeline-execution-triggers.md#schedule-trigger)típusú eseményindítók esetén.

| Változó neve | Leírás |
| --- | --- |
| @trigger().scheduledTime |Az az idő, amikor az eseményindító ütemezve lett a folyamat futásának meghívására. |
| @trigger(). kezdő időpont |Az az idő, amikor az eseményindító **ténylegesen** elindította a folyamat futtatását. Ez kis mértékben eltérhet a trigger ütemezett idejétől. |

## <a name="tumbling-window-trigger-scope"></a>Kiesési ablak triggerének hatóköre

Ezeket a rendszerváltozókat az eseményindító JSON-ban bárhol lehet hivatkozni [TumblingWindowTrigger](concepts-pipeline-execution-triggers.md#tumbling-window-trigger)típusú eseményindítók esetén.

| Változó neve | Leírás |
| --- | --- |
| @trigger(). outputs. windowStartTime |Az trigger futtatásához társított ablak kezdete. |
| @trigger(). outputs. windowEndTime |Az trigger futtatásához társított ablak vége. |
| @trigger().scheduledTime |Az az idő, amikor az eseményindító ütemezve lett a folyamat futásának meghívására. |
| @trigger(). kezdő időpont |Az az idő, amikor az eseményindító **ténylegesen** elindította a folyamat futtatását. Ez kis mértékben eltérhet a trigger ütemezett idejétől. |

## <a name="storage-event-trigger-scope"></a>Tárolási esemény eseményindítójának hatóköre

Ezeket a rendszerváltozókat az eseményindító JSON-ban bárhol lehet hivatkozni [BlobEventsTrigger](concepts-pipeline-execution-triggers.md#event-based-trigger)típusú eseményindítók esetén.

| Változó neve | Leírás |
| --- | --- |
| @triggerBody(). fájlnév  |Annak a fájlnak a neve, amelynek létrehozása vagy törlése miatt a trigger tüzet okozott.   |
| @triggerBody(). mappanév  |A által megadott fájlt tartalmazó mappa elérési útja `@triggerBody().fileName` . A mappa elérési útjának első szegmense az Azure Blob Storage tároló neve.  |
| @trigger(). kezdő időpont |Az az idő, amikor a trigger kezdeményezte a folyamat futtatását. |

## <a name="custom-event-trigger-scope"></a>Egyéni eseményindító hatóköre

Ezeket a rendszerváltozókat az eseményindító JSON-ban bárhol lehet hivatkozni [CustomEventsTrigger](concepts-pipeline-execution-triggers.md#event-based-trigger)típusú eseményindítók esetén.

>[!NOTE]
>Azure Data Factory azt várja, hogy az egyéni esemény [Azure Event Grid eseményvezérelt sémával](../event-grid/event-schema.md)legyen formázva.

| Változó neve | Leírás
| --- | --- |
| @triggerBody(). Event. eventType | Az egyéni eseményindító futtatását kiváltó események típusa. Az esemény típusa ügyfél által definiált mező, és tetszőleges karakterlánc típusú értékeket is igénybe veheti. |
| @triggerBody(). Event. subject | Az eseményindítót a tüzet kiváltó egyéni esemény tárgya. |
| @triggerBody(). Event. adattal. _Kulcsnév_ | Az egyéni esemény adatmezője egy ingyenes a JSON-blobtól, amelyet az ügyfél az üzenetek és adatok küldésére használhat. Használja az adathalmazt. az egyes mezőkre mutató _Kulcsnév_ . Például: @triggerBody (). az Event. revisszahívás az _adatok_ alatt tárolt _visszahívási_ mező értékét adja vissza. |
| @trigger(). kezdő időpont | Az az idő, amikor a trigger kezdeményezte a folyamat futtatását. |

## <a name="next-steps"></a>Következő lépések

* További információ a változók kifejezésekben való használatáról: [Expression language & functions](control-flow-expression-language-functions.md).
* A folyamat trigger-hatóköri rendszerváltozóinak használatához tekintse meg a [folyamat trigger-metaadatainak hivatkozása](how-to-use-trigger-parameterization.md) című témakört.
