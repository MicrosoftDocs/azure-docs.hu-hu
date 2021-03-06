---
title: Azure-beli automanage-beli virtuális gép áthelyezése régiók között
description: Ismerje meg, hogyan helyezhet át egy autofelügyelt virtuális gépet régiók között
author: asinn826
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: how-to
ms.date: 02/05/2021
ms.author: alsin
ms.custom: subject-moving-resources
ms.openlocfilehash: 99371b8618756c196b75858288c5c4785272a7e8
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/29/2021
ms.locfileid: "101650465"
---
# <a name="move-an-azure-automanage-virtual-machine-to-a-different-region"></a>Azure-beli automanage-beli virtuális gép áthelyezése egy másik régióba
Ez a cikk azt ismerteti, hogyan lehet engedélyezni az automanage szolgáltatást egy virtuális gépen (VM), ha másik régióba helyezi át. Előfordulhat, hogy több okból szeretné áthelyezni a virtuális gépeket egy másik régióba. Például egy új Azure-régió kihasználása érdekében a belső házirend-és irányítási követelmények teljesítéséhez, vagy a kapacitás megtervezésének követelményeire reagálva. Előfordulhat, hogy az Ön által áthelyezett virtuális gépek jelenleg önfelügyelt állapotban vannak, és azt szeretné, hogy az áthelyezést követően a rendszer ne kezelje őket.

## <a name="prerequisites"></a>Előfeltételek
* Győződjön meg arról, hogy a célként megadott régiót az [automanage támogatja](./automanage-virtual-machines.md#prerequisites).
* Győződjön meg arról, hogy az Log Analytics munkaterület-régió, az Automation-fiók régiója és a célként megadott régió a régió-hozzárendelések által támogatott összes régiót [támogatja.](../automation/how-to/region-mappings.md)

## <a name="prepare-your-automanaged-vms-for-moving"></a>Készítse elő az autofelügyelt virtuális gépeket a mozgatáshoz
Tiltsa le az automanaged-t az autofelügyelt virtuális gépeken. Ezt úgy teheti meg, hogy kijelöli a virtuális gépeket az automanage (felügyelet) panelen, és az automanage (automanagement) panelen a **felügyelet letiltása** elemre

## <a name="move-your-automanaged-vms-and-re-enable-automanage"></a>Az autofelügyelt virtuális gépek mozgatása és az autokezelés újbóli engedélyezése
A virtuális gépek áthelyezésével kapcsolatos részletekért tekintse meg ezt a [cikket](../resource-mover/tutorial-move-region-virtual-machines.md).

Miután áthelyezte a virtuális gépeket a régiók között, újra engedélyezheti újból a felügyeletet. A részletek [itt](./automanage-virtual-machines.md#enabling-automanage-for-vms-in-azure-portal)érhetők el.

## <a name="next-steps"></a>Következő lépések
* [További információ az Azure automanage szolgáltatásról](./automanage-virtual-machines.md)
* [Gyakran ismételt kérdések megtekintése az Azure automanagetel kapcsolatban](./faq.md)