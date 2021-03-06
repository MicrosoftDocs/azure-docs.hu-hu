---
title: A foglalási kedvezmények ismertetése – Egykiszolgálós Azure Database for PostgreSQL
description: Megtudhatja, hogyan alkalmazható foglalási kedvezmény egykiszolgálós Azure Database for PostgreSQL-re.
author: mksuni
ms.author: sumuth
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: ace362872f0b7ba8e2f3d0302c887e2465c62982
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/03/2020
ms.locfileid: "93240341"
---
# <a name="how-a-reservation-discount-is-applied-to-azure-database-for-postgresql-single-server"></a>A foglalási kedvezmény alkalmazása egykiszolgálós Azure Database for PostgreSQL-re

A fenntartott egykiszolgálós Azure Database for PostgreSQL-kapacitás megvásárlása után automatikusan alkalmazva lesz a foglalási kedvezmény a foglalás attribútumaival és mennyiségével egyező egykiszolgálós PostgreSQL-adatbázisokra. A foglalás csak az egykiszolgálós Azure Database for PostgreSQL számítási költségeit fedezi. A tárolásért és a hálózatkezelésért a normál díjakat kell fizetnie.

## <a name="how-reservation-discount-is-applied"></a>A foglalási kedvezmény alkalmazása

A foglalási kedvezmény csak akkor érvényes, ha * **folyamatosan igénybe veszi** _. Ez azt jelenti, hogy ha nem rendelkezik megfelelő erőforrásokkal egy adott órában, akkor az arra az órára vonatkozó foglalási mennyiség elveszik. A lefoglalt, de fel nem használt órák nem vihetők tovább.</br>

Egy erőforrás leállításakor a rendszer a foglalási kedvezményt automatikusan a megadott hatókör egy másik egyező erőforrására alkalmazza. Ha nem találhatók egyező erőforrások a megadott hatókörben, akkor a lefoglalt órák elvesznek.

## <a name="discount-applied-to-azure-database-for-postgresql-single-server"></a>Az egykiszolgálós Azure Database for PostgreSQL-re érvényesített kedvezmény

A fenntartott egykiszolgálós Azure Database for PostgreSQL-kapacitásra érvényes kedvezményt a rendszer óránként alkalmazza a futó egykiszolgálós PostgreSQL-re. A megvásárolt foglalást a rendszer megfelelteti a futó egykiszolgálós Azure Database for PostgreSQL általi számításierőforrás-használatnak. Azon egykiszolgálós PostgreSQL-ek esetén, amelyek nem futnak egy teljes órán át, a rendszer automatikusan a foglalási attribútumokkal egyező más egykiszolgálós Azure Database for PostgreSQL-re alkalmazza a foglalást. A kedvezmény a párhuzamosan futó egykiszolgálós Azure Database for PostgreSQL-ekre is alkalmazható. Ha nem rendelkezik a foglalási attribútumokkal egyező olyan önálló PostgreSQL-kiszolgálóval, amely egy teljes órán át fut, akkor arra az órára nem kapja meg a foglalási kedvezménnyel járó teljes előnyt.

Az alábbi példák bemutatják, hogyan lesz alkalmazva a fenntartott önálló Azure Database for PostgreSQL-kiszolgáló kapacitására érvényes kedvezmény a megvásárolt magok száma alapján, és annak alapján, hogy mikor futnak.

_ **1. példa** : Fenntartott önálló Azure Database for PostgreSQL-kiszolgáló kapacitást vásárolt 8 virtuális maghoz. Ha 16 virtuális magos önálló Azure Database for PostgreSQL-kiszolgálót futtat, amely egyezik a foglalás többi attribútumával, az önálló PostgreSQL-kiszolgáló 8 virtuális magjának számításierőforrás-használata után a használatalapú árat kell fizetnie, a foglalási kedvezményt pedig a 8 magos önálló PostgreSQL-kiszolgáló egy órányi számításierőforrás-használatára kapja meg.</br>

A többi példa esetében azt feltételezzük, hogy a fenntartott önálló Azure Database for PostgreSQL-kiszolgáló kapacitását egy 16 virtuális magos önálló Azure Database for PostgreSQL-kiszolgálóhoz vásárolta, és a többi foglalási attribútum megegyezik a futó önálló PostgreSQL-kiszolgálókkal.

* **2. példa** : Két 8 virtuális magos önálló Azure Database for PostgreSQL-kiszolgálót futtat egy-egy óráig. A 16 virtuális magos foglalási kedvezményt a rendszer mindkét 8 virtuális magos önálló Azure Database for PostgreSQL-kiszolgáló számításierőforrás-használatára alkalmazza.

* **3. példa** : Egy 16 virtuális magos önálló Azure Database for PostgreSQL-kiszolgálót futtat 13:00 órától 13:45-ig. Egy másik 16 virtuális magos önálló Azure Database for PostgreSQL-kiszolgálót 13:30-tól 14:00 óráig futtat. A foglalási kedvezmény mindkettőt fedezi.

* **4. példa** : Egy 16 virtuális magos önálló Azure Database for PostgreSQL-kiszolgálót futtat 13:00 órától 13:45-ig. Egy másik 16 virtuális magos önálló Azure Database for PostgreSQL-kiszolgálót 13:30-tól 14:00 óráig futtat. A 15 perces átfedésért használatalapú díjat kell fizetnie. A fennmaradó idő számításierőforrás-használatára érvényes a foglalási kedvezmény.

Az Azure Reservations számlázási használati jelentésekben történő alkalmazásának megismeréséhez és megtekintéséhez lásd [az Azure Reservations használatát ismertető](./understand-reserved-instance-usage-ea.md) cikket.

## <a name="next-steps"></a>További lépések

Ha kérdése van vagy segítségre van szüksége, [hozzon létre egy támogatási kérést](https://go.microsoft.com/fwlink/?linkid=2083458).