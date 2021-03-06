---
title: Az ügynök portfoliójának áttekintése és az operációs rendszer támogatása (előzetes verzió)
description: A IoT készült Azure Defender az eszközök típusától függően az ügynökök nagy portfólióját biztosítja.
ms.date: 1/20/2021
ms.topic: conceptual
ms.openlocfilehash: d2e463051d0897afe52981ea2d50ddd1f06bb54d
ms.sourcegitcommit: 77d7639e83c6d8eb6c2ce805b6130ff9c73e5d29
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/05/2021
ms.locfileid: "106383422"
---
# <a name="agent-portfolio-overview-and-os-support-preview"></a>Az ügynök portfoliójának áttekintése és az operációs rendszer támogatása (előzetes verzió)

A IoT készült Azure Defender az eszközök típusától függően az ügynökök nagy portfólióját biztosítja. 

## <a name="standalone-agent"></a>Önálló ügynök

Az önálló ügynök a Linux operációs rendszerek nagy részét fedi le, amelyek bináris csomagként vagy a belső vezérlőprogram részeként beépíthető forráskódként telepíthetők, és lehetővé teszik az ügyfelek igényei alapján történő módosítást és testreszabást. Példa az operációs rendszer támogatására: 

| Operációs rendszer | AMD64 | ARM32v7 |
|--|--|--|
| Debian 9 | ✓ | ✓ |
| Ubuntu 18.04 | ✓ |  |
| Ubuntu 20,04 | ✓ |  |

További részletekért, az operációs rendszer támogatásához vagy a forráskódhoz való hozzáférés kéréséhez, hogy az eszköz belső vezérlőprogramja részeként beilleszkedjen, lépjen kapcsolatba a fiók kezelőjével, vagy küldjön egy e-mailt a címre <defender_micro_agent@microsoft.com> . 

## <a name="azure-rtos-micro-agent"></a>Azure RTOS Micro-ügynök

A IoT Micro Agent Azure Defender átfogó és könnyű biztonsági megoldást kínál az Azure RTOS-t használó eszközökhöz. Az Azure Defender for IoT Micro Agent a gyakori fenyegetésekre, valamint a valós idejű operációs rendszer (RTOS) eszközeire vonatkozó lehetséges kártékony tevékenységek lefedettségét biztosítja. A Micro Agent az Azure RTOS NetX Duo összetevő részeként készült, és figyeli az eszköz hálózati tevékenységét. 

Az Azure Defender for IoT Micro Agent az Azure RTOS NetX Duo összetevő részeként készült, és figyeli az eszköz hálózati tevékenységét. A Micro Agent egy átfogó és könnyű biztonsági megoldásból áll, amely lefedettséget biztosít a gyakori fenyegetésekkel szemben, valamint a valós idejű operációs rendszerek (RTOS-eszközök) lehetséges kártékony tevékenységeit.

## <a name="next-steps"></a>Következő lépések

További információ az [önálló Micro Agent áttekintése (előzetes verzió)](concept-standalone-micro-agent-overview.md).