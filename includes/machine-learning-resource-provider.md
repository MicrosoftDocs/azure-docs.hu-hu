---
author: blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 01/09/2020
ms.author: larryfr
ms.openlocfilehash: 8fd774f8a3a73ceaffa7902b35e1b1dff12ef5af
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/30/2021
ms.locfileid: "75893971"
---
Azure Machine Learning munkaterület vagy a munkaterület által használt erőforrás létrehozásakor a következő üzenetekhez hasonló hibaüzenet jelenhet meg:

* `No registered resource provider found for location {location}`
* `The subscription is not registered to use namespace {resource-provider-namespace}`

A legtöbb erőforrás-szolgáltató automatikusan regisztrálva van, de nem az összes. Ha ezt az üzenetet kapja, regisztrálnia kell a megemlített szolgáltatót.

Az erőforrás-szolgáltatók regisztrálásával kapcsolatos információkért lásd: [hibák elhárítása az erőforrás-szolgáltató regisztrálásához](../articles/azure-resource-manager/templates/error-register-resource-provider.md).