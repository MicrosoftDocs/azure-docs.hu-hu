---
title: Az Azure Percept DK-összetevők testetlen és összegyűjtése
description: Ismerje meg, hogyan foglalhat össze, csatlakozhat és hogyan kapcsolható be az Azure Percept DK
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: quickstart
ms.date: 02/16/2021
ms.custom: template-quickstart
ms.openlocfilehash: efa255ba38f7e00785335bf458ecc0ed91da646b
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/30/2021
ms.locfileid: "104608180"
---
# <a name="quickstart-unbox-and-assemble-your-azure-percept-dk-components"></a>Gyors útmutató: az Azure Percept DK-összetevők testetlen és összegyűjtése

Ha megkapta az Azure Percept DK-t, a jelen útmutatóban tájékozódhat az összetevők csatlakoztatásáról és az eszközön való bekapcsolásról.

## <a name="prerequisites"></a>Előfeltételek

- Azure Percept DK (fejlesztői készlet)
- P7 csavarhúzó (opcionális, a tápkábel-összekötő légifuvarozóhoz való védelméhez használatos)

## <a name="unbox-and-assemble-your-device"></a>Az eszköz testetlen és összegyűjtése

1. Az Azure Percept DK-összetevők kimutatása.

    A fejlesztői készlet tartalmaz egy Carrier-táblát, az Azure Percept-jövőképét, az antennákat és a kábeleket tartalmazó kellékeket, valamint egy olyan üdvözlő kártyát, amely egy hexadecimális kulccsal rendelkezik.

1. A fejlesztői készlet-összetevők összekötése.

    > [!NOTE]
    > A hálózati adapter portja a szállítói tábla jobb oldalán található. A fennmaradó portok (2x USB-A, 1x USB-C és 1x Ethernet) és a főkapcsoló gomb a szállítói tábla bal oldalán található.

    1. A Wi-Fi antennákat a szállítói táblába is becsavarozhatja.

    1. Csatlakoztassa a víziós SoM-t a Carrier kártya USB-C portjához az USB-C kábellel.

    1. Csatlakoztassa a tápkábelt a hálózati adapterhez.

    1. Távolítsa el a fennmaradó műanyag csomagolást az eszközökről.

    1. Csatlakoztassa a hálózati adaptert/kábelt a szállítói táblához és egy fali konnektorhoz. Ha a tápkábel-összekötőt teljesen biztonságossá szeretné tenni a P7, használjon egy csavarhúzót (a fejlesztői készlet nem tartalmazza) az összekötő csavarok szigorításához.

    1. Miután csatlakoztatta a tápkábelt egy fali konnektorhoz, a rendszer automatikusan bekapcsolja az eszközt. A rendszer megvilágítja a szállítói tábla bal oldalán lévő főkapcsoló gombot. Várjon egy ideig, amíg az eszköz elindult.

        > [!NOTE]
        > A főkapcsoló gomb az eszköz kikapcsolására vagy újraindítására szolgál, miközben csatlakoztatva van egy konnektorhoz. Áramszünet esetén az eszköz automatikusan újraindul.

A fejlesztői készlet szerelvény vizualizációs bemutatásához tekintse meg az alábbi videó 0:00 – 0:50:

</br>

> [!VIDEO https://www.youtube.com/embed/-dmcE2aQkDE]

## <a name="next-steps"></a>Következő lépések

Most, hogy a fejlesztői készlet csatlakoztatva van és be van kapcsolva, az eszköz telepítésének befejezéséhez tekintse meg az [Azure PERCEPT DK telepítési élményének útmutatóját](./quickstart-percept-dk-set-up.md) . A telepítési élmény lehetővé teszi a fejlesztői készlet Wi-Fi hálózathoz való összekapcsolását, SSH-bejelentkezés beállítását, IoT Hub létrehozását és az Azure-fiókhoz való fejlesztői készlet kiépítését. Az eszköz beállításának befejezése után készen áll a prototípusok elindítására.