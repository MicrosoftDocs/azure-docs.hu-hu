---
title: Azure-beli virtuális gépek értékelésének létrehozása Azure Migrate felderítési és értékelési eszközzel | Microsoft Docs
description: Útmutató Azure-beli virtuális gépek értékelésének létrehozásához a Azure Migrate felderítési és értékelési eszközzel
author: rashi-ms
ms.author: rajosh
ms.manager: abhemraj
ms.topic: how-to
ms.date: 07/15/2019
ms.openlocfilehash: f901fe23d2ff04e7ad9ba920dd90ebab8a39246c
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/30/2021
ms.locfileid: "104786719"
---
# <a name="create-an-azure-vm-assessment"></a>Azure-beli virtuális gép felmérésének létrehozása

Ez a cikk bemutatja, hogyan hozhat létre Azure-beli virtuális gépeket a helyszíni kiszolgálókhoz a VMware, a Hyper-V vagy a fizikai/egyéb felhőalapú környezetekben Azure Migrate: felderítéssel és értékeléssel.

[Azure Migrate](migrate-services-overview.md) segítségével áttelepítheti az Azure-ba. A Azure Migrate központosított központot biztosít a helyszíni infrastruktúrák, alkalmazások és az Azure-ba irányuló adatfelderítés,-értékelés és-áttelepítés nyomon követéséhez. A hub Azure-eszközöket biztosít az értékeléshez és az áttelepítéshez, valamint a harmadik féltől származó független szoftvergyártói (ISV) ajánlatokhoz. 

## <a name="before-you-start"></a>Előkészületek

- Győződjön meg arról, hogy [létrehozott](./create-manage-projects.md) egy Azure Migrate projektet.
- Ha már létrehozott egy projektet, győződjön meg arról, hogy [felvette](how-to-assess.md) a Azure Migrate: Discovery and Assessment eszközt.
- Értékelés létrehozásához be kell állítania egy Azure Migrate készüléket a [VMware](how-to-set-up-appliance-vmware.md) vagy a [Hyper-V](how-to-set-up-appliance-hyper-v.md)rendszerhez. A készülék felderíti a helyszíni kiszolgálókat, és metaadatokat és teljesítményadatokat küld Azure Migratenak: felderítés és Értékelés. [További információk](migrate-appliance.md).


## <a name="azure-vm-assessment-overview"></a>Az Azure VM Assessment áttekintése
Kétféle méretezési feltétel használható az Azure-beli virtuális gépek felmérésének Azure Migrate: felderítés és értékelés használatával történő létrehozásához.

**Értékelés** | **Részletek** | **Adatok**
--- | --- | ---
**Teljesítményalapú** | Értékelések az összegyűjtött teljesítményadatok alapján | **Ajánlott** virtuálisgép-méret: a processzor-és memóriahasználat adatai alapján.<br/><br/> **Ajánlott lemez típusa (standard vagy prémium szintű felügyelt lemez)**: a helyszíni lemezek IOPS és átviteli sebessége alapján.
**Helyszíni** | Helyszíni méretezésen alapuló értékelések. | **Ajánlott** virtuálisgép-méret: a helyszíni virtuális gép méretétől függően<br/><br> **Ajánlott lemez típusa**: az értékeléshez kiválasztott tárolási típus alapján.

[További](concepts-assessment-calculation.md) információ az értékelésekről.

## <a name="run-an-assessment"></a>Értékelés futtatása

Az értékelést a következőképpen futtathatja:

1. Az **Áttekintés** oldalon > **Windows, Linux és SQL Server**, kattintson a **kiszolgálók értékelése és migrálása** elemre.

   ![Az értékelés és a kiszolgálók áttelepítése gomb helye](./media/tutorial-assess-vmware-azure-vm/assess.png)

2. **Azure Migrate: felderítés és értékelés**, kattintson az **értékelés** gombra, és válassza az **Azure virtuális gép** lehetőséget.

    ![Az értékelés gomb helye](./media/tutorial-assess-vmware-azure-vm/assess-servers.png)

3. A **kiszolgálók**  >  **értékelési típusának** értékelése
4. A **felderítés forrása**:

    - Ha a berendezést használó kiszolgálókat észlelt, válassza ki **a Azure Migrate készülékről felderített kiszolgálókat**.
    - Ha egy importált CSV-fájllal rendelkező kiszolgálókat észlelt, válassza az **importált kiszolgálók** elemet. 
    
1. Kattintson a **Szerkesztés** elemre az értékelési tulajdonságok áttekintéséhez.

    ![Az összes megtekintése gomb helye az értékelési tulajdonságok áttekintéséhez](./media/tutorial-assess-vmware-azure-vm/assessment-name.png)

1. Az **értékelés tulajdonságai**  >  **cél tulajdonságai**:
    - A **célhely** mezőben válassza ki azt az Azure-régiót, amelyre az áttelepítést szeretné végezni.
        - A méretre és a díjakra vonatkozó javaslatok a megadott helyen alapulnak. Ha a célhelyet az alapértelmezett értékre módosítja, a rendszer kérni fogja a **fenntartott példányok** és a virtuálisgép- **sorozatok** megadását.
        - Azure Government Az értékeléseket az [alábbi régiókban](migrate-support-matrix.md#supported-geographies-azure-government) lehet megcélozni
    - A **tárolási típus mezőben**
        - Ha teljesítmény-alapú adatmennyiséget szeretne használni az értékelésben, válassza az **automatikus** lehetőséget Azure MIGRATE a IOPS és az átviteli sebesség alapján a tárolási típust ajánljuk.
        - Másik lehetőségként válassza ki azt a tárolási típust, amelyet a virtuális géphez szeretne használni a Migrálás során.
    - A **fenntartott példányok** területen válassza ki, hogy a virtuális gép tartalék példányait szeretné-e használni az áttelepítéskor.
        - Ha fenntartott példány használatát választja, a "**kedvezmény (%)** vagy a **virtuális gép üzemidő** nem adható meg. 
        - [További információk](https://aka.ms/azurereservedinstances).
 1. **Virtuális gép mérete**:
     - A **méretezési feltétel** területen válassza ki, hogy szeretné-e alapozni az értékelést a kiszolgálói konfigurációs adatokra/metaadatokra vagy a teljesítmény alapú adatokra. Ha teljesítményadatokat használ:
        - A **teljesítmény előzményeiben** adja meg az adatok időtartamát, amely alapján az értékelést alapozni szeretné
        - A **percentilis kihasználtsága** mezőben határozza meg a teljesítmény mintához használni kívánt százalékos értéket. 
    - A virtuálisgép- **sorozat** mezőben határozza meg, hogy milyen Azure-beli virtuálisgép-sorozatot szeretne figyelembe venni.
        - Ha teljesítmény-alapú értékelést használ, a Azure Migrate egy értéket javasol Önnek.
        - Szükség szerint módosítsa a beállításokat. Ha például nem rendelkezik olyan éles környezettel, amely az Azure-beli sorozatú virtuális gépeket igényli, kizárhatja a sorozatot a sorozatok listájáról.
    - A **Comfort Factor (kényelmi tényező**) mezőben adja meg az értékelés során használni kívánt puffert. Ez olyan problémákhoz vezetett, mint például a szezonális használat, a rövid teljesítménybeli előzmények és a jövőbeli használat várható növekedése. Ha például két kényelmi tényezőt használ:
    
        **Összetevő** | **Hatékony kihasználtság** | **Komfort tényező hozzáadása (2,0)**
        --- | --- | ---
        Cores | 2  | 4
        Memória | 8 GB | 16 GB
   
1. A **díjszabásban**:
    - Az **ajánlat** mezőben válassza ki az [Azure-ajánlatot](https://azure.microsoft.com/support/legal/offer-details/) , ha regisztrálva van. Az értékelés az ajánlat költségeit becsüli.
    - A **Pénznem** területen válassza ki a fiókja számlázási pénznemét.
    - A **kedvezmény (%)** területen adja meg az Azure-ajánlaton keresztül kapott előfizetés-specifikus kedvezményeket. Az alapértelmezett beállítás 0%.
    - A **virtuális gép üzemidő** területen adja meg a virtuális gépek által futtatandó időtartamot (naponta, havonta/órában).
        - Ez olyan Azure-beli virtuális gépek esetében hasznos, amelyek nem futnak folyamatosan.
        - A becsült érték a megadott időtartamon alapul.
        - Az alapértelmezett érték havi 31 nap/napi 24 óra.
    - Az **EA-előfizetés** területen határozza meg, hogy szeretné-e a NAGYVÁLLALATI szerződés (EA) előfizetési kedvezményt figyelembe venni a költségbecslés során. 
    - A **Azure Hybrid Benefit** területen válassza ki, hogy már rendelkezik-e Windows Server-licenccel. Ha így tesz, és a Windows Server-előfizetések aktív frissítési garanciával rendelkezik, akkor az Azure-ba való licencek esetén a [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/) is alkalmazható.

1. Ha módosítja a módosításokat, kattintson a **Save (Mentés** ) gombra.

    ![Kiértékelés tulajdonságai](./media/tutorial-assess-vmware-azure-vm/assessment-properties.png)

1. A **kiszolgálók értékelése** > kattintson a **tovább** gombra.

1. A **kiszolgálók kiválasztása az**  >  **értékelés nevének** kiértékeléséhez > adja meg az értékelés nevét. 

1. A **válasszon ki vagy hozzon létre egy csoportot** > válassza az **új létrehozása** elemet, és adja meg a csoport nevét. 
    
     ![Virtuális gépek felvétele egy csoportba](./media/tutorial-assess-vmware-azure-vm/assess-group.png)


1. Válassza ki a készüléket, és válassza ki a csoportba felvenni kívánt virtuális gépeket. Ezután kattintson a **Tovább** gombra.


1. A **felülvizsgálat** és Értékelés létrehozása lapon tekintse át az értékelés részleteit, majd kattintson az **Értékelés létrehozása** elemre a csoport létrehozásához és az értékelés futtatásához.

1. Az értékelés létrehozása után tekintse meg a **kiszolgálók**  >  **Azure Migrate: felderítési és értékelési**  >  **értékelések**.

1. Az értékelés az **Értékelés exportálása** gombra kattintva Excel-fájlként letölthető.
    > [!NOTE]
    > A teljesítmény-alapú felmérések esetében javasoljuk, hogy várjon legalább egy napot a felderítés megkezdése után a felmérés létrehozása előtt. Ezzel a teljesítménnyel kapcsolatos adatok nagyobb megbízhatósággal gyűjthetők. Ideális esetben a felderítés megkezdése után várjon a megadott teljesítménybeli időtartamra (nap/hét/hónap) a magas megbízhatóságú minősítéshez.

## <a name="review-an-azure-vm-assessment"></a>Azure-beli virtuális gép kiértékelésének áttekintése

Az Azure-beli virtuális gépek kiértékelése a következőket tartalmazza:

- **Azure-kompatibilitás**: Megadja, hogy a kiszolgálók megfelelők-e az Azure-ba történő migráláshoz.
- **Havi költségbecslés**: a virtuális gépek Azure-ban való futtatásának becsült havi számítási és tárolási költségei.
- **Havi tárolási költségek becslése**: A lemezes tárolás becsült költségei a migrálás után.

### <a name="view-an-azure-vm-assessment"></a>Azure-beli virtuális gépek felmérésének megtekintése

1. **Windows, Linux és SQL Server**  >  **Azure Migrate: felderítés és értékelés**, kattintson az Azure-beli **virtuális gépek felmérése** melletti számra.
2. Az **Értékelések** területen válasszon ki egy kiértékelést a megnyitáshoz. Példa (csak becslések és költségek például): 

    ![Kiértékelés összegzése](./media/how-to-create-assessment/assessment-summary.png)

### <a name="review-azure-readiness"></a>Az Azure-kompatibilitás áttekintése

1. Az **Azure készültségi** területén ellenőrizze, hogy a kiszolgálók készen állnak-e az Azure-ba való áttelepítésre.
2. Tekintse át a kiszolgáló állapotát:
    - **Azure-beli használatra kész**: Az Azure Migrate javaslatot tesz a virtuálisgép-méretre, illetve költségbecslést ad az értékelésben szereplő virtuális gépekhez.
    - **Feltételekkel kész**: megjeleníti a problémákat és a javasolt szervizelést.
    - **Nem áll készen az Azure-ra: a** problémákat és a javasolt szervizelést jeleníti meg.
    - **Felkészültségi ismeretlen**: akkor használatos, ha az adatelérhetőségi problémák miatt Azure Migrate nem tudja felmérni a készültséget.

3. Kattintson egy **Azure-készültségi** állapotra. Megtekintheti a kiszolgáló készültségi adatait, és részletesen megtekintheti a kiszolgáló adatait, beleértve a számítási, tárolási és hálózati beállításokat.



### <a name="review-cost-details"></a>Költségadatok áttekintése

Ez a nézet a virtuális gépek az Azure-ban való futtatásával kapcsolatos becsült számítási és tárolási költséget mutatja.

1. A havi számítási és tárolási költségek áttekintése. A kiértékelt csoportban lévő kiszolgálók költségei összesítve jelennek meg.

    - A költségbecslés a kiszolgáló méretére vonatkozó javaslatokon, valamint a lemezek és a tulajdonságok alapján történik.
    - A becsült havi számítási és tárolási költségek jelennek meg.
    - A költségbecslés a helyszíni kiszolgálók IaaS virtuális gépekként való futtatására szolgál. Az Azure-beli virtuális gépek felmérése nem veszi figyelembe a Pásti vagy az SaaS költségeit.

2. Megtekintheti a havi tárolási díjak becsült összegét. Ez a nézet az értékelt csoport összesített tárolási költségeit jeleníti meg, a különböző típusú tárolóeszközök felosztásával.
3. Megtekintheti az egyes kiszolgálók részleteit.


### <a name="review-confidence-rating"></a>Megbízhatósági minősítés áttekintése

Ha teljesítmény-alapú értékeléseket futtat, a rendszer megbízhatósági minősítést rendel az értékeléshez.

![Megbízhatósági minősítés](./media/how-to-create-assessment/confidence-rating.png)

- A rendszer kinyeri az 1 csillagos (legalacsonyabb) és az 5 csillagos (legmagasabb) minősítést.
- A megbízhatósági minősítés segít megbecsülni az értékelés által biztosított méretre vonatkozó ajánlások megbízhatóságát.
- A megbízhatósági minősítés az értékelés kiszámításához szükséges adatpontok rendelkezésre állásán alapul.

Az értékelés megbízhatósági minősítése a következő.

**Adatpont rendelkezésre állása** | **Megbízhatósági minősítés**
--- | ---
0%–20% | 1 csillag
21%–40% | 2 csillag
41%–60% | 3 csillag
61%–80% | 4 csillag
81%–100% | 5 csillag




## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogyan hozhat létre nagy megbízhatóságú csoportokat a [függőségi leképezés](how-to-create-group-machine-dependencies.md) használatával.
- [További információk](concepts-assessment-calculation.md) az értékelések számításával kapcsolatban.