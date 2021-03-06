---
title: A modell verziójának meghatározása a Text Analytics v3-ban
titleSuffix: Azure Cognitive Services
description: Megtudhatja, hogyan határozhatja meg az adatain használt Text Analytics API modellt.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 02/17/2021
ms.author: aahi
ms.openlocfilehash: ee3a56fed87f493d79796c0d91e5324c94e87eca
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/30/2021
ms.locfileid: "104599357"
---
# <a name="model-versioning-in-the-text-analytics-api"></a>A modell verziószámozása a Text Analytics API

A Text Analytics API 3. verziója lehetővé teszi, hogy kiválassza a modell azon verzióját, amelyet az adatain használni fog. A választható `model-version` paraméter használatával kiválaszthatja a modell verzióját az API-kérelmekben. Példa: `<resource-url>/text/analytics/v3.0/sentiment?model-version=2020-04-01`. Ha ez a paraméter nincs megadva, az API alapértelmezés szerint a legújabb stabil verziót fogja megadni. 

## <a name="available-versions"></a>Elérhető verziók

Az alábbi táblázat segítségével megtalálhatja, hogy az egyes futtatott végpontok milyen modell-verziókat támogatnak.


| Végpont                        | Támogatott verziók                                     | legújabb verzió |
|---------------------------------|--------------------------------------------------------|----------------|
| `/sentiment`                    | `2019-10-01`, `2020-04-01`                             | `2020-04-01`   |
| `/languages`                    | `2019-10-01`, `2020-07-01`, `2020-09-01`, `2021-01-05` | `2021-01-05`   |
| `/entities/linking`             | `2019-10-01`, `2020-02-01`                             | `2020-02-01`   |
| `/entities/recognition/general` | `2019-10-01`, `2020-02-01`, `2020-04-01`,`2021-01-15`  | `2021-01-15`   |
| `/entities/recognition/pii`     | `2019-10-01`, `2020-02-01`, `2020-04-01`,`2020-07-01`, `2021-01-15`  | `2021-01-15`   |
| `/entities/health`              | `2021-03-01`                           | `2021-03-01`   |
| `/keyphrases`                   | `2019-10-01`, `2020-07-01`                             | `2020-07-01`   |


Ezen modellek frissítéseiről a Újdonságok című útmutatóban talál [részleteket.](../whats-new.md)

## <a name="text-analytics-for-health"></a>Egészségügyi Text Analytics

Az [állapot](../how-tos/text-analytics-for-health.md) -tároló Text Analytics a fenti API-végpontok helyett a modell eltérő verzióját használja.  Vegye figyelembe, hogy a tárolók rendszerképében csak egy modell verziója érhető el.

| Végpont                        | Tároló képcímkéje                     | Modell verziója |
|---------------------------------|-----------------------------------------|---------------|
| `/entities/health`              | `3.0.015370001-onprem-amd64` vagy legújabb          | `2021-03-01`  |
| `/entities/health`              | `1.1.013530001-amd64-preview`           | `2020-09-03`  |
| `/entities/health`              | `1.1.013150001-amd64-preview`           | `2020-07-24`  |
| `/domains/health`               | `1.1.012640001-amd64-preview`           | `2020-05-08`  |
| `/domains/health`               | `1.1.012420001-amd64-preview`           | `2020-05-08`  |
| `/domains/health`               | `1.1.012070001-amd64-preview`           | `2020-04-16`  |


## <a name="next-steps"></a>Következő lépések

* [A Text Analytics áttekintése](../overview.md)
* [Hangulatelemzés](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Entitások felismerése](../how-tos/text-analytics-how-to-entity-linking.md)
