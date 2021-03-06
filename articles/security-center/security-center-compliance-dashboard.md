---
title: 'Oktatóanyag: Jogszabályi megfelelési ellenőrzések – Azure Security Center'
description: 'Oktatóanyag: Megtudhatja, hogyan javíthatja a jogszabályi megfelelést a Azure Security Center.'
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: tutorial
ms.date: 04/21/2021
ms.author: memildin
ms.openlocfilehash: c8ac9079321e47a1e6d9b8689be46bf55bdd4243
ms.sourcegitcommit: 3c460886f53a84ae104d8a09d94acb3444a23cdc
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2021
ms.locfileid: "107834612"
---
# <a name="tutorial-improve-your-regulatory-compliance"></a>Oktatóanyag: Jogszabályi megfelelés javítása

Azure Security Center a jogszabályi megfelelőségi irányítópult használatával leegyszerűsíti a szabályozási megfelelőségi követelmények **teljesítésének folyamatát.** 

Security Center a hibrid felhőkörnyezetet, hogy elemezze a kockázati tényezőket az előfizetéseire alkalmazott szabványok vezérlői és ajánlott eljárásai alapján. Az irányítópult a szabványoknak való megfelelés állapotát tükrözi. 

Amikor engedélyezi a Security Center azure-előfizetésben, a rendszer automatikusan hozzárendeli az [Azure-biztonsági](https://docs.microsoft.com/security/benchmark/azure/introduction) teljesítménytesztet az előfizetéshez. Ez a széles körben elterjedt teljesítményteszt a [Center for Internet Security (CIS)](https://www.cisecurity.org/benchmark/azure/) és a National Institute of Standards and Technology [(NIST)](https://www.nist.gov/) vezérlőire épül, és a felhőközpontú biztonságra összpontosít.

A jogszabályi megfelelési irányítópult megjeleníti a környezetben a választott szabványokra és szabályozásokra vonatkozó összes értékelés állapotát. A javaslatok betartása és a környezeti kockázati tényezők csökkentése során javul a megfelelőségi helyzet.

Ezen oktatóanyag segítségével megtanulhatja a következőket:

> [!div class="checklist"]
> * A jogszabályi megfelelőség kiértékelése a jogszabályi megfelelési irányítópult használatával
> * A megfelelőségi helyzet javítása a javaslatok alapján tett műveletekkel
> * Riasztások beállítása a megfelelőségi rendszer változásairól
> * Megfelelőségi adatok exportálása folyamatos streamként és heti pillanatképekként

Ha nem rendelkezik Azure-előfizetéssel, kezdés előtt hozzon létre egy [ingyenes](https://azure.microsoft.com/free/) fiókot.

## <a name="prerequisites"></a>Előfeltételek

Az oktatóanyagban szereplő funkciók végiglépése:

- [Azure Defender](azure-defender.md) engedélyezni kell. A Azure Defender 30 napig ingyenesen kipróbálhatja.
- Olyan fiókkal kell bejelentkeznie, amely olvasói hozzáféréssel rendelkezik a szabályzat megfelelőségi adataihoz **(a** biztonsági olvasó nem elegendő). Az előfizetés **globális** olvasói szerepköre működni fog. Legalább erőforrás-házirend-közreműködői és  biztonsági **rendszergazdai szerepkört kell hozzárendelni.**

##  <a name="assess-your-regulatory-compliance"></a>A jogszabályi megfelelés felmérése

A jogszabályi megfelelési irányítópulton a kiválasztott megfelelőségi szabványok megjelenik az összes követelményüknek megfelelően, ahol a támogatott követelmények a megfelelő biztonsági értékelésekhez vannak leképezve. Az értékelések állapota a szabványnak való megfelelőséget tükrözi.

A jogszabályi megfelelési irányítópult segítségével a kiválasztott szabványoknak és szabályozásoknak való megfelelés hiányosságaira összpontosíthat. Ez az összpontosított nézet azt is lehetővé teszi, hogy folyamatosan figyelje a megfelelőségét a dinamikus felhőben és a hibrid környezetekben.

1. A Security Center menüjében válassza a Jogszabályi **megfelelőség lehetőséget.**

    A képernyő tetején található egy irányítópult, amely a megfelelőségi állapot áttekintését és a támogatott megfelelőségi szabályozások készletét is bemutatja. Itt láthatja a teljes megfelelőségi pontszámot, valamint az egyes standardok által hozzárendelt átadási és sikertelen értékelések számát.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-dashboard.png" alt-text="Jogszabályi megfelelés irányítópultja" lightbox="./media/security-center-compliance-dashboard/compliance-dashboard.png":::

1. Válassza ki az Ön számára releváns megfelelőségi szabvány lapfülét (1). Látni fogja, hogy mely előfizetések esetén érvényes a szabvány (2), valamint az erre a szabványra vonatkozó összes vezérlő listáját (3). A vonatkozó vezérlők esetén megtekintheti a vezérlőhöz társított átadási és sikertelen értékelések részleteit (4), valamint az érintett erőforrások számát (5). Egyes vezérlők szürkén jelennek meg. Ezekhez a vezérlőkhöz nincs Security Center értékelés társítva. Ellenőrizze a követelményeket, és értékelje őket a környezetében. Ezek némelyike folyamattal kapcsolatos, és nem műszaki jellegű.

    :::image type="content" source="./media/security-center-compliance-dashboard/compliance-drilldown.png" alt-text="Egy adott szabványnak való megfelelés részleteinek feltárása":::

1. Egy adott szabvány aktuális megfelelőségi állapotának összegzését összegző PDF-jelentés létrehozásához válassza a **Jelentés letöltése lehetőséget.**

    A jelentés magas szintű összegzést nyújt a kiválasztott szabvány megfelelőségi állapotáról a Security Center adatok alapján. A jelentés az adott szabvány vezérlői szerint van rendezve. A jelentés megosztható az érintettekkel, és bizonyítékokat nyújthat a belső és külső auditorok számára.

    :::image type="content" source="./media/security-center-compliance-dashboard/download-report.png" alt-text="Megfelelőségi jelentés letöltése":::

## <a name="improve-your-compliance-posture"></a>A megfelelőségi helyzet javítása

A jogszabályi megfelelési irányítópulton található információk használatával javíthatja a megfelelőségi helyzeteket azáltal, hogy közvetlenül az irányítópulton belül feloldja a javaslatokat.

1.  Válassza ki az irányítópulton megjelenő hibás értékeléseket a javaslat részleteinek megtekintéséhez. Minden javaslat tartalmaz néhány javítási lépést a probléma megoldásához.

1.  Válasszon ki egy adott erőforrást a további részletek megtekintéséhez és az adott erőforrásra vonatkozó javaslat megoldásához. <br>Az **Azure CIS 1.1.0 szabványban** például válassza ki azt a javaslatot, hogy a lemeztitkosítást alkalmazni kell **a virtuális gépeken.**

    :::image type="content" source="./media/security-center-compliance-dashboard/sample-recommendation.png" alt-text="Javaslat kiválasztása standard érdeklődőkből közvetlenül a javaslat részleteit tartalmazó oldalra":::

1. Ebben a példában,  amikor a Javaslat részletei lapon a Művelet lehetőséget választja, a Azure Portal Azure-beli virtuális gép oldalára érkezik, ahol a Biztonság lapon engedélyezheti a **titkosítást:**

    :::image type="content" source="./media/security-center-compliance-dashboard/encrypting-vm-disks.png" alt-text="A szervizelési lehetőségekhez vezető Javaslat részletei oldalon található Művelet gomb":::

    A javaslatok alkalmazásával kapcsolatos további információkért lásd: [Implementing security recommendations in Azure Security Center.](security-center-recommendations.md)

1.  Miután lépéseket tett a javaslatok megoldásához, az eredmény adatokat fog látni a megfelelőségi irányítópult jelentésében, mivel a megfelelőségi pontszám javul.

    > [!NOTE]
    > Az értékelések körülbelül 12 óránként futnak, így csak a releváns értékelés következő futtatása után láthatja a megfelelőségi adatokra gyakorolt hatást.


## <a name="export-your-compliance-status-data"></a>A megfelelőségi állapotadatok exportálása

Ha szeretné nyomon követni a megfelelőségi állapotot a környezetben található más monitorozási eszközökkel, a Security Center egy exportálási mechanizmussal teszi ezt egyértelművé. Konfigurálja **a folyamatos exportálást** úgy, hogy a kiválasztott adatokat egy Azure Event Hubs- vagy Log Analytics-munkaterületre küldje.

Folyamatos exportálási adatok használata Azure Event Hubba vagy Log Analytics-munkaterületre:

- Exportálja az összes jogszabályi megfelelési adatot egy **folyamatos streambe:**

    :::image type="content" source="media/security-center-compliance-dashboard/export-compliance-data-stream.png" alt-text="Szabályozási megfelelőségi adatok streamének folyamatos exportálása" lightbox="media/security-center-compliance-dashboard/export-compliance-data-stream.png":::

- Heti **pillanatképek exportálása** a jogszabályi megfelelési adatokról:

    :::image type="content" source="media/security-center-compliance-dashboard/export-compliance-data-snapshot.png" alt-text="Szabályozási megfelelőségi adatok heti pillanatképének folyamatos exportálása" lightbox="media/security-center-compliance-dashboard/export-compliance-data-snapshot.png":::

A megfelelőségi adatok **PDF-/CSV-jelentését** közvetlenül a jogszabályi megfelelőségi irányítópultról is exportálhatja:

:::image type="content" source="media/security-center-compliance-dashboard/export-compliance-data-report.png" alt-text="Jogszabályi megfelelési adatok exportálása PDF- vagy CSV-jelentésként" lightbox="media/security-center-compliance-dashboard/export-compliance-data-report.png":::

További információ az adatok [folyamatos Security Center történő exportálásról.](continuous-export.md)


## <a name="run-workflow-automations-when-there-are-changes-to-your-compliance"></a>Munkafolyamat-automatizálások futtatása a megfelelőség módosítása esetén

Security Center munkafolyamat-automatizálási szolgáltatása akkor aktiválhat Logic Apps, amikor az egyik jogszabályi megfelelőségi értékelés állapotváltozást vált ki.

Előfordulhat például, hogy Security Center szeretne e-mailt küldeni egy adott felhasználónak, ha egy megfelelőségi értékelés meghiúsul. Először létre kell hoznia a logikai alkalmazást (a Azure Logic Apps [használatával),](../logic-apps/logic-apps-overview.md)majd be kell állítania az eseményindítót egy új munkafolyamat-automatizálásban a válaszok automatizálása az eseményindítókra való Security Center [leírtak szerint.](workflow-automation.md)

:::image type="content" source="media/release-notes/regulatory-compliance-triggers-workflow-automation.png" alt-text="A jogszabályi megfelelési értékelések változásainak használata munkafolyamat-automatizálás aktiválásához" lightbox="media/release-notes/regulatory-compliance-triggers-workflow-automation.png":::




## <a name="faq---regulatory-compliance-dashboard"></a>GYIK – Jogszabályi megfelelési irányítópult

- [Milyen szabványok támogatottak a megfelelőségi irányítópulton?](#what-standards-are-supported-in-the-compliance-dashboard)
- [Miért jelennek meg szürkén egyes vezérlők?](#why-do-some-controls-appear-grayed-out)
- [Hogyan távolítható el egy beépített szabvány, például a PCI-DSS, az ISO 27001 vagy a SOC2 TSP az irányítópultról?](#how-can-i-remove-a-built-in-standard-like-pci-dss-iso-27001-or-soc2-tsp-from-the-dashboard)
- [A javaslat alapján módosítottam a javasolt módosításokat, de ez nem jelenik meg az irányítópulton](#i-made-the-suggested-changed-based-on-the-recommendation-yet-it-isnt-being-reflected-in-the-dashboard)
- [Milyen engedélyekre van szükségem a megfelelőségi irányítópult eléréséhez?](#what-permissions-do-i-need-to-access-the-compliance-dashboard)
- [A jogszabályi megfelelési irányítópult nem töltődik be](#the-regulatory-compliance-dashboard-isnt-loading-for-me)
- [Hogyan tudom megtekinteni az irányítópultomon szabványonkénti átadási és sikertelen vezérlőkről szóló jelentést?](#how-can-i-view-a-report-of-passing-and-failing-controls-per-standard-in-my-dashboard)
- [Hogyan tölthetek le olyan jelentést, amely nem PDF formátumú megfelelőségi adatokat tartalmaz?](#how-can-i-download-a-report-with-compliance-data-in-a-format-other-than-pdf)
- [Hogyan hozhatok létre kivételeket egyes szabályzatokhoz a jogszabályi megfelelési irányítópulton?](#how-can-i-create-exceptions-for-some-of-the-policies-in-the-regulatory-compliance-dashboard)
- [Milyen Azure Defender vagy licencre van szükségem a jogszabályi megfelelési irányítópulthoz?](#what-azure-defender-plans-or-licenses-do-i-need-to-use-the-regulatory-compliance-dashboard)
- [Hogyan, hogy melyik teljesítménytesztet vagy szabványt kell használni?](#how-do-i-know-which-benchmark-or-standard-to-use)

### <a name="what-standards-are-supported-in-the-compliance-dashboard"></a>Milyen szabványok támogatottak a megfelelőségi irányítópulton?
Alapértelmezés szerint a jogszabályi megfelelési irányítópulton megjelenik az Azure biztonsági teljesítményteszt. Az Azure biztonsági teljesítménytesztje a Microsoft által írt, azure-specifikus irányelvek a biztonsághoz és a megfelelőséghez a közös megfelelőségi keretrendszerek alapján. További információ: [Az Azure biztonsági teljesítményteszt bemutatása.](../security/benchmarks/introduction.md)

A többi szabványnak való megfelelés nyomon követéséhez explicit módon hozzá kell azokat adni az irányítópulthoz.
 
További szabványokat adhat hozzá, mint például az Azure CIS 1.3.0, az NIST SP 800-53, az NIST SP 800-171, a SWIFT CSP CSCF-v2020, a UK Official és az UK NHS, a HIPAA, a Canada Federal PBMM, az ISO 27001, a SOC2-TSP és a PCI-DSS 3.2.1.  

Az irányítópulton további szabványok is fel lesznek adva, és szerepelnek a Szabványkészlet testreszabása a jogszabályi megfelelési [irányítópulton oldalon található információk között.](update-regulatory-compliance-packages.md)

### <a name="why-do-some-controls-appear-grayed-out"></a>Miért jelennek meg szürkén egyes vezérlők?
Az irányítópulton minden megfelelőségi szabványhoz megjelenik a szabvány vezérlőinek listája. A megfelelő vezérlők esetén megtekintheti az átadás és a sikertelen értékelések részleteit.

Egyes vezérlők szürkén jelennek meg. Ezekhez a vezérlőkhöz nem tartozik Security Center értékelés. Némelyik eljáráshoz vagy folyamathoz köthető, ezért nem ellenőrizhető a Security Center. Vannak, akik még nem valósítanak meg automatizált szabályzatokat vagy értékeléseket, de a jövőben lesznek. Egyes vezérlők pedig a platform felelősségét okozhatják a felhőben való megosztott [felelősség részben leírtak szerint.](../security/fundamentals/shared-responsibility.md) 

### <a name="how-can-i-remove-a-built-in-standard-like-pci-dss-iso-27001-or-soc2-tsp-from-the-dashboard"></a>Hogyan távolítható el egy beépített szabvány, például a PCI-DSS, az ISO 27001 vagy a SOC2 TSP az irányítópultról? 
Ha testre szeretné szabni a jogszabályi megfelelési irányítópultot, és csak az Ön számára érvényes szabványokra szeretne összpontosítani, eltávolíthatja a szervezet számára nem releváns megjelenített szabályozási szabványokat. A standardok eltávolításához kövesse a Szabványos eltávolítása [az irányítópultról útmutatását.](update-regulatory-compliance-packages.md#remove-a-standard-from-your-dashboard)

### <a name="i-made-the-suggested-changed-based-on-the-recommendation-yet-it-isnt-being-reflected-in-the-dashboard"></a>A javaslat alapján módosítottam a javasolt módosításokat, de ez nem jelenik meg az irányítópulton
Miután végrehajtotta a javaslatok megoldásához szükséges lépéseket, várjon 12 órát a megfelelőségi adatok változásainak adatokat való megfeleléshez. Az értékelések körülbelül 12 óránként futnak, így csak az értékelések futtatása után fogja látni a megfelelőségi adatokra gyakorolt hatást.
 
### <a name="what-permissions-do-i-need-to-access-the-compliance-dashboard"></a>Milyen engedélyekre van szükségem a megfelelőségi irányítópult eléréséhez?
A megfelelőségi adatok megtekintéséhez legalább  Olvasó hozzáféréssel kell rendelkezik a szabályzat megfelelőségi adataihoz is; így a biztonsági olvasó egyedül nem elegendő. Ha Ön globális olvasó az előfizetésben, az is elég lesz.

Az irányítópult elérésére és a szabványok kezelésére szolgáló szerepkörök minimális készlete az **Erőforrás-házirend közreműködője** és **a Biztonsági rendszergazda.**


### <a name="the-regulatory-compliance-dashboard-isnt-loading-for-me"></a>A jogszabályi megfelelési irányítópult nem töltődik be
A jogszabályi megfelelési irányítópult csak akkor használható Azure Security Center ha Azure Defender előfizetési szinten engedélyezve van. Ha az irányítópult nem töltődik be megfelelően, próbálkozzon a következő lépésekkel:

1. Törölje a böngésző gyorsítótárát.
1. Próbálkozzon egy másik böngészővel.
1. Próbálja meg megnyitni az irányítópultot egy másik hálózati helyről.


### <a name="how-can-i-view-a-report-of-passing-and-failing-controls-per-standard-in-my-dashboard"></a>Hogyan tudom megtekinteni az irányítópultomon szabványonkénti átadási és sikertelen vezérlőkről szóló jelentést?
A fő irányítópulton látható egy jelentés, amely az (1) "első 4" legalacsonyabb megfelelőségi szabvány (1) "átadási és sikertelen" vezérlőiről tartalmaz jelentést az irányítópulton. Az átadó/hibás vezérlők állapotának ellenőrzéshez válassza a (2) Show all x (where x is the number of standards you're tracking) (Az ***összes x*** megjelenítése (ahol x a nyomon követni kívánt szabványok száma). A környezetsík az összes nyomon követett szabvány megfelelőségi állapotát megjeleníti.

:::image type="content" source="media/security-center-compliance-dashboard/summaries-of-compliance-standards.png" alt-text="A jogszabályi megfelelési irányítópult Összefoglalás szakasza":::


### <a name="how-can-i-download-a-report-with-compliance-data-in-a-format-other-than-pdf"></a>Hogyan tölthetek le olyan jelentést, amely nem PDF formátumú megfelelőségi adatokat tartalmaz?
Ha a Jelentés **letöltése lehetőséget választja,** válassza ki a szabványt és a formátumot (PDF vagy CSV). Az eredményül kapott jelentés a portál szűrője által kiválasztott előfizetések aktuális készletét fogja tükrözni.

- A PDF-jelentés a kiválasztott szabvány összegző állapotát jeleníti meg
- A CSV-jelentés részletes eredményeket biztosít erőforrásonként, mivel az egyes vezérlőkhöz társított szabályzatokkal kapcsolatos

Az egyéni szabályzatok jelentéseit jelenleg nem lehet letölteni; csak a megadott szabályozási szabványoknak megfelelően.


### <a name="how-can-i-create-exceptions-for-some-of-the-policies-in-the-regulatory-compliance-dashboard"></a>Hogyan hozhatok létre kivételeket egyes szabályzatokhoz a jogszabályi megfelelési irányítópulton?
Az Security Center biztonsági pontszámba beépített és a biztonsági pontszámba beépített szabályzatok számára kivételeket hozhat létre közvetlenül a portálon egy vagy több erőforráshoz a Következő részben leírtak szerint: Erőforrások kivétele a biztonsági [pontszámból.](exempt-resource.md)

Más szabályzatok számára közvetlenül a házirendben hozhat létre kivételt a kivételstruktúrában [Azure Policy utasításait követve.](../governance/policy/concepts/exemption-structure.md)


### <a name="what-azure-defender-plans-or-licenses-do-i-need-to-use-the-regulatory-compliance-dashboard"></a>Milyen Azure Defender vagy licencre van szükségem a jogszabályi megfelelési irányítópulthoz?
Ha az összes Azure Defender-csomag engedélyezve van bármelyik Azure-erőforrástípuson, akkor az összes adatával együtt hozzáférhet a jogszabályi megfelelőségi irányítópulthoz a Security Center.


### <a name="how-do-i-know-which-benchmark-or-standard-to-use"></a>Hogyan, hogy melyik teljesítménytesztet vagy szabványt kell használni?
[Az Azure Security Benchmark](https://docs.microsoft.com/security/benchmark/azure/introduction) (ASB) a Microsoft által meghatározott, a gyakori megfelelőségi ellenőrzési keretrendszerekkel (például [CIS Microsoft Azure Foundations Benchmark](https://www.cisecurity.org/benchmark/azure/) és [NIST SP 800-53)](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final)összhangban meghatározott, a biztonsági javaslatok és ajánlott eljárások canonical készlete. Az ASB egy rendkívül átfogó teljesítményteszt, amely úgy lett kialakítva, hogy az Azure-szolgáltatások széles körének legfrissebb biztonsági képességeit javasolja. Az ASB-t olyan ügyfeleknek ajánljuk, akik szeretnék maximalizálni a biztonsági helyzetüket, és a megfelelőségi állapotukat az iparági szabványoknak megfelelően szeretnék igazítani.

A [CIS-teljesítménytesztet](https://www.cisecurity.org/benchmark/azure/) egy független entitás , a Center for Internet Security (CIS) készítője, amely az alapvető Azure-szolgáltatások egy részkészletére vonatkozó javaslatokat tartalmaz. A CIS-sel dolgozunk annak érdekében, hogy a javaslatok mindig naprakészek az Azure legújabb fejlesztéseivel, de néha lemaradnak, és elavulttá válnak. Néhány ügyfél azonban ezt a célt, a CIS harmadik féltől származó értékelését szeretné kiindulási és elsődleges biztonsági alapkonfigurációként használni.

Mivel kiadták az Azure biztonsági teljesítménytesztet, számos ügyfél úgy döntött, hogy a CIS-teljesítménytesztek helyettesítéseként áttér rá.


## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban megtanulta, hogyan felhasználásával Security Center megfelelőségi irányítópultját a következőre:

> [!div class="checklist"]
> * Az Ön számára fontos szabványokra és szabályozásra vonatkozó megfelelőségi állapot megtekintése és figyelése.
> * Javítsa a megfelelőségi állapotot a vonatkozó javaslatok feloldásával és a megfelelőségi pontszám javításának figyelésával.

A jogszabályi megfelelőségi irányítópult jelentősen leegyszerűsítheti a megfelelőségi folyamatot, és jelentősen csökkentheti az Azure-, hibrid- és többfelhős környezet megfelelőségi bizonyítékának gyűjtéséhez szükséges időt.

További tudnivalókért tekintse meg a következő kapcsolódó oldalakat:

- [A szabványok halmazának testreszabása](update-regulatory-compliance-packages.md) a jogszabályi megfelelési irányítópulton – Megtudhatja, hogyan választhatja ki, hogy mely szabványok jelenjenek meg a jogszabályi megfelelési irányítópulton. 
- [Biztonsági javaslatok kezelése a Azure Security Center](security-center-recommendations.md) – Megtudhatja, hogyan használhatja a Security Center az Azure-erőforrások védelméhez.