---
title: fájl belefoglalása
description: fájl belefoglalása
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 08/17/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: cd778da5fd53cb8744a9f267384fcc8e11941582
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105645358"
---
## <a name="enable-the-event-grid-resource-provider"></a>A Event Grid erőforrás-szolgáltató engedélyezése

Ha korábban még nem használta Event Grid az Azure-előfizetésében, előfordulhat, hogy regisztrálnia kell a Event Grid erőforrás-szolgáltatót. A szolgáltató regisztrálásához futtassa az alábbi parancsot:

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

Eltarthat egy kis ideig, amíg a regisztráció befejeződik. Az állapot ellenőrzéséhez futtassa a következőt:

```azurecli-interactive
az provider show --namespace Microsoft.EventGrid --query "registrationState"
```

Ha a `registrationState``Registered` értékű, készen áll a folytatásra.
