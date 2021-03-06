---
title: 'Oktatóanyag: a Peakon automatikus felhasználó-kiépítés konfigurálása a Azure Active Directoryrel | Microsoft Docs'
description: Megtudhatja, hogyan konfigurálhatja a Azure Active Directoryt, hogy automatikusan kiépítse és kiépítse a felhasználói fiókokat a Peakon.
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/28/2019
ms.author: zhchia
ms.openlocfilehash: c04bbd5459690262b484582e807569b965a0439b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/29/2021
ms.locfileid: "96349888"
---
# <a name="tutorial-configure-peakon-for-automatic-user-provisioning"></a>Oktatóanyag: az automatikus felhasználó-kiépítés Peakon konfigurálása

Az oktatóanyag célja annak bemutatása, hogy milyen lépéseket kell végrehajtani a Peakon és a Azure Active Directory (Azure AD) szolgáltatásban az Azure AD konfigurálásához, hogy a felhasználók és/vagy csoportok automatikusan kiépítsék és kiépítsék a Peakon.

> [!NOTE]
>  Ez az oktatóanyag az Azure AD-beli felhasználói kiépítési szolgáltatásra épülő összekötőt ismerteti. A szolgáltatás funkcióival, működésével és a gyakori kérdésekkel kapcsolatos fontos részletekért lásd: [Felhasználók átadásának és megszüntetésének automatizálása a SaaS-alkalmazásokban az Azure Active Directoryval](../app-provisioning/user-provisioning.md).
>
> Ez az összekötő jelenleg előzetes verzióban érhető el. Az előzetes verziójú funkciók általános Microsoft Azure használati feltételeivel kapcsolatos további információkért tekintse meg a [Microsoft Azure-előnézetek kiegészítő használati feltételeit](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
## <a name="prerequisites"></a>Előfeltételek

Az oktatóanyagban ismertetett forgatókönyv feltételezi, hogy már rendelkezik a következő előfeltételekkel

* Egy Azure AD-bérlő.
* [Egy Peakon-bérlő](https://peakon.com/us/pricing/).
* Rendszergazdai jogosultságokkal rendelkező felhasználói fiók a Peakon-ben.

## <a name="assigning-users-to-peakon"></a>Felhasználók kiosztása a Peakon

Azure Active Directory a *hozzárendelések* nevű koncepció használatával határozza meg, hogy mely felhasználók kapnak hozzáférést a kiválasztott alkalmazásokhoz. Az automatikus felhasználó-kiépítés kontextusában csak az Azure AD-alkalmazáshoz hozzárendelt felhasználók és/vagy csoportok lesznek szinkronizálva.

A felhasználók automatikus üzembe helyezésének konfigurálása és engedélyezése előtt döntse el, hogy az Azure AD mely felhasználóinak és/vagy csoportjai számára szükséges a Peakon való hozzáférés. Miután eldöntötte, ezeket a felhasználókat és/vagy csoportokat hozzárendelheti a Peakon az alábbi utasításokat követve:

* [Felhasználó vagy csoport társítása vállalati alkalmazáshoz](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-peakon"></a>Fontos Tippek a felhasználók Peakon való hozzárendeléséhez 

* Azt javasoljuk, hogy egyetlen Azure AD-felhasználó legyen hozzárendelve a Peakon-hoz az automatikus felhasználó-kiépítési konfiguráció teszteléséhez. Később további felhasználókat és/vagy csoportokat is hozzá lehet rendelni.

* Amikor Peakon rendel hozzá egy felhasználóhoz, a hozzárendelés párbeszédpanelen ki kell választania bármely érvényes alkalmazásspecifikus szerepkört (ha elérhető). Az **alapértelmezett hozzáférési** szerepkörrel rendelkező felhasználók ki vannak zárva a kiépítés alól.

## <a name="set-up-peakon-for-provisioning"></a>Peakon beállítása a kiépítés számára

1.  Jelentkezzen be a [Peakon felügyeleti konzolra](https://app.Peakon.com/login). Kattintson a **Konfigurálás** elemre. 

    ![Peakon felügyeleti konzol](media/Peakon-provisioning-tutorial/Peakon-admin-configuration.png)

2.  Válassza az **integrációk** lehetőséget.
    
    ![Képernyőkép a konfigurációs beállításokról a kinevezett integrációs lehetőséggel.](media/Peakon-provisioning-tutorial/Peakon-select-integration.png)

3.  Az **alkalmazottak üzembe** helyezésének engedélyezése.

    ![Képernyőkép az alkalmazott kiépítési szakaszáról az engedélyezés lehetőség kiválasztásával.](media/Peakon-provisioning-tutorial/peakon05.png)

4.  Másolja a **SCIM 2,0 URL-cím** és a **OAuth tulajdonosi jogkivonat** értékeit. Ezek az értékek a Peakon alkalmazás kiépítés lapjának **bérlő URL-címe** és **titkos jogkivonat** mezőjében lesznek megadva a Azure Portal.

    ![Peakon-létrehozási jogkivonat](media/Peakon-provisioning-tutorial/peakon04.png)

## <a name="add-peakon-from-the-gallery"></a>Peakon hozzáadása a gyűjteményből

Az Azure AD-vel való automatikus Peakon konfigurálásához hozzá kell adnia a Peakon az Azure AD Application Gallery-ből a felügyelt SaaS-alkalmazások listájához.

1. A **[Azure Portal](https://portal.azure.com)** a bal oldali navigációs panelen válassza a **Azure Active Directory** lehetőséget.

    ![A Azure Active Directory gomb](common/select-azuread.png)

2. Lépjen a **vállalati alkalmazások** elemre, majd válassza a **minden alkalmazás** lehetőséget.

    ![A vállalati alkalmazások panel](common/enterprise-applications.png)

3. Új alkalmazás hozzáadásához kattintson a panel tetején található **új alkalmazás** gombra.

    ![Az új alkalmazás gomb](common/add-new-app.png)

4. A keresőmezőbe írja be a **Peakon** kifejezést, válassza az **Peakon** elemet az eredmények panelen, majd kattintson a **Hozzáadás** gombra az alkalmazás hozzáadásához.

    ![Peakon az eredmények listájában](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-peakon"></a>Automatikus felhasználó-kiépítés beállítása a Peakon 

Ez a szakasz végigvezeti az Azure AD-kiépítési szolgáltatás konfigurálásának lépésein, hogy az Azure AD-ben felhasználói és/vagy Peakon alapuló felhasználókat és/vagy csoportokat hozzon létre, frissítsen és tiltsa le.

> [!TIP]
> Azt is megteheti, hogy engedélyezi az SAML-alapú egyszeri bejelentkezést a Peakon számára, az [Peakon egyszeri bejelentkezés oktatóanyagában](peakon-tutorial.md)megadott utasításokat követve. Az egyszeri bejelentkezés az automatikus felhasználó-kiépítés függetlenül is konfigurálható, bár ez a két funkció egymáshoz tartozik.

### <a name="to-configure-automatic-user-provisioning-for-peakon--in-azure-ad"></a>Az automatikus felhasználó-kiépítés konfigurálása a Peakon az Azure AD-ben:

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com). Válassza a **Vállalati alkalmazások** lehetőséget, majd a **Minden alkalmazás** elemet.

    ![Vállalati alkalmazások panel](common/enterprise-applications.png)

2. Az alkalmazások listában válassza a **Peakon** lehetőséget.

    ![Az Peakon hivatkozás az alkalmazások listájában](common/all-applications.png)

3. Válassza a **Kiépítés** lapot.

    ![Képernyőkép a felügyeleti lehetőségek kezeléséről a kiépítési lehetőséggel.](common/provisioning.png)

4. Állítsa a **Kiépítési mód** mezőt **Automatikus** értékre.

    ![Képernyőkép a kiépítési mód legördülő listájáról az automatikus lehetőséggel.](common/provisioning-automatic.png)

5. A **rendszergazdai hitelesítő adatok** szakaszban adja meg a **scim 2,0 URL-címet** és a **OAuth-tulajdonosi jogkivonat** azon értékeit, amelyek korábban a **bérlői URL-címben** és a **titkos jogkivonatban** szerepelnek. Kattintson a **kapcsolat tesztelése** lehetőségre, hogy az Azure ad képes legyen csatlakozni a Peakon. Ha a kapcsolat meghiúsul, győződjön meg arról, hogy a Peakon-fiókja rendszergazdai jogosultságokkal rendelkezik, és próbálkozzon újra.

    ![Bérlői URL + token](common/provisioning-testconnection-tenanturltoken.png)

7. Az **értesítési e-mail** mezőben adja meg egy olyan személy vagy csoport e-mail-címét, akinek meg kell kapnia a kiépítési hibákra vonatkozó értesítéseket, és jelölje be a jelölőnégyzetet – **e-mail-értesítés küldése hiba** esetén.

    ![Értesítés e-mailben](common/provisioning-notification-email.png)

8. Kattintson a **Mentés** gombra.

9. A **leképezések** szakaszban válassza a **Azure Active Directory felhasználók szinkronizálása a Peakon** lehetőséget.

    ![Peakon felhasználói leképezések](media/Peakon-provisioning-tutorial/Peakon-user-mappings.png)

10. Tekintse át az Azure AD-ből szinkronizált felhasználói attribútumokat az **attribútum-hozzárendelési** szakaszban található Peakon. Az **egyeztetési** tulajdonságokként kiválasztott attribútumok a Peakon felhasználói fiókjainak a frissítési műveletekhez való megfeleltetésére szolgálnak. A módosítások elvégzéséhez kattintson a **Save (Mentés** ) gombra.

    ![Peakon felhasználói attribútumai](media/Peakon-provisioning-tutorial/Peakon-user-attributes.png)

12. A hatóköri szűrők konfigurálásához tekintse meg az alábbi utasításokat a       [hatókör szűrője oktatóanyagban](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
    
    ![Átadási hatókör](common/provisioning-scope.png)

15. Amikor készen áll az átadásra, kattintson a **Mentés** gombra.

    ![Átadási konfiguráció mentése](common/provisioning-configuration-save.png)

Ez a művelet elindítja a **Beállítások** szakasz **hatókörében** meghatározott összes felhasználó és/vagy csoport kezdeti szinkronizálását. A kezdeti szinkronizálás hosszabb időt vesz igénybe, mint a későbbi szinkronizálások, amelyek körülbelül 40 percenként történnek, amíg az Azure AD kiépítési szolgáltatás fut. A **szinkronizálás részletei** szakasz segítségével figyelheti a folyamat előrehaladását, és követheti a kiépítési tevékenységre mutató hivatkozásokat, amelyek az Azure ad-kiépítési szolgáltatás által a Peakon-on végrehajtott összes műveletet ismertetik.

Az Azure AD-kiépítési naplók beolvasásával kapcsolatos további információkért lásd: [jelentéskészítés az automatikus felhasználói fiókok üzembe](../app-provisioning/check-status-user-account-provisioning.md)helyezéséhez.

## <a name="connector-limitations"></a>Összekötő korlátozásai

* A Peakon összes egyéni felhasználói attribútumát ki kell terjeszteni a Peakon egyéni SCIM felhasználói kiterjesztéséről `urn:ietf:params:scim:schemas:extension:peakon:2.0:User` .

## <a name="additional-resources"></a>További források

* [Felhasználói fiók átadásának kezelése vállalati alkalmazásokhoz](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryval?](../manage-apps/what-is-single-sign-on.md)
## <a name="next-steps"></a>További lépések

* [Tudnivalók a naplók áttekintéséről és az átadási tevékenységekkel kapcsolatos jelentések lekéréséről](../app-provisioning/check-status-user-account-provisioning.md)