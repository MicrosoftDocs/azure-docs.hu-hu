---
title: fájl belefoglalása
description: fájl belefoglalása
author: lrtoyou1223
ms.service: data-factory
ms.topic: include
ms.date: 10/09/2019
ms.author: lle
ms.openlocfilehash: d24e8957dc63024a46495e8a66bad7dbb3790f38
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/29/2021
ms.locfileid: "100389498"
---
| Tartománynevek                  | Kimenő portok | Leírás                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.core.windows.net`          | 443            | A saját üzemeltetésű integrációs modul használja az Azure Storage-fiókhoz való csatlakozásra az előkészített másolási szolgáltatás használatakor. |
| `*.database.windows.net`      | 1433           | Csak akkor szükséges, ha Azure SQL Database vagy az Azure szinapszis Analytics szolgáltatásból másol, vagy nem kötelező. A szakaszos másolási szolgáltatással az 1433-as port megnyitása nélkül másolhat az adatSQL Database vagy az Azure szinapszis Analytics szolgáltatásba. |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | Csak akkor szükséges, ha a vagy a rendszerből másol Azure Data Lake Store és opcionálisan. |
