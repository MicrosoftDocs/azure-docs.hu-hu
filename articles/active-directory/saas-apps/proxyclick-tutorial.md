---
title: 'Oktatóanyag: Azure Active Directory integráció a Proxyclick-szel | Microsoft Docs'
description: Ebből az oktatóanyagból megtudhatja, hogyan konfigurálhat egyszeri bejelentkezést Azure Active Directory és Proxyclick között.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 23ae1a2c1371cda9435ea76f02cebc79c141c904
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/29/2021
ms.locfileid: "92522240"
---
# <a name="tutorial-azure-active-directory-integration-with-proxyclick"></a>Oktatóanyag: Azure Active Directory integráció a Proxyclick

Ebből az oktatóanyagból megtudhatja, hogyan integrálhatja a Proxyclick a Azure Active Directory (Azure AD) szolgáltatással.
Ez az integráció az alábbi előnyöket biztosítja:

* Az Azure AD segítségével szabályozhatja, hogy ki férhet hozzá a Proxyclick.
* Lehetővé teheti a felhasználók számára, hogy automatikusan bejelentkezzenek a Proxyclick (egyszeri bejelentkezés) az Azure AD-fiókjával.
* A fiókokat egyetlen központi helyen kezelheti: a Azure Portal.

Ha többet szeretne megtudni az Azure AD-vel való SaaS-alkalmazások integrálásáról, tekintse meg az [egyszeri bejelentkezést a Azure Active Directory alkalmazásaihoz](../manage-apps/what-is-single-sign-on.md)című témakört.

Ha nem rendelkezik Azure-előfizetéssel, [hozzon létre egy ingyenes fiókot](https://azure.microsoft.com/free/) a feladatok megkezdése előtt.

## <a name="prerequisites"></a>Előfeltételek

Az Azure AD-integráció Proxyclick való konfigurálásához a következők szükségesek:

* Egy Azure AD-előfizetés. Ha nem rendelkezik Azure AD-környezettel, regisztrálhat egy [hónapos próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).
* Az egyszeri bejelentkezést engedélyező Proxyclick-előfizetés.

## <a name="scenario-description"></a>Forgatókönyv leírása

Ebben az oktatóanyagban egy tesztkörnyezetben konfigurálja és teszteli az Azure AD egyszeri bejelentkezést.

* A Proxyclick támogatja az SP által kezdeményezett és a identitásszolgáltató által kezdeményezett egyszeri bejelentkezést.

## <a name="add-proxyclick-from-the-gallery"></a>Proxyclick hozzáadása a gyűjteményből

A Proxyclick Azure AD-be való integrálásának beállításához hozzá kell adnia a Proxyclick a katalógusból a felügyelt SaaS-alkalmazások listájához.

1. A [Azure Portal](https://portal.azure.com)a bal oldali ablaktáblán válassza a **Azure Active Directory**:

    ![Válassza az Azure Active Directory elemet.](common/select-azuread.png)

2. Lépjen a **vállalati alkalmazások**  >  **minden alkalmazás**:

    ![Vállalati alkalmazások panel](common/enterprise-applications.png)

3. Alkalmazás hozzáadásához válassza az ablak tetején található **új alkalmazás** elemet:

    ![Új alkalmazás kiválasztása](common/add-new-app.png)

4. A keresőmezőbe írja be a **Proxyclick** kifejezést. A keresési eredmények között válassza a **Proxyclick** lehetőséget, majd válassza a **Hozzáadás** lehetőséget.

     ![Keresési eredmények](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Az Azure AD egyszeri bejelentkezés konfigurálása és tesztelése

Ebben a szakaszban az Azure AD egyszeri bejelentkezést konfigurálja és teszteli a Proxyclick használatával egy Britta Simon nevű teszt felhasználó használatával.
Az egyszeri bejelentkezés engedélyezéséhez létre kell hoznia egy kapcsolatot az Azure AD-felhasználó és a megfelelő felhasználó között a Proxyclick-ben.

Az Azure AD egyszeri bejelentkezés Proxyclick való konfigurálásához és teszteléséhez a következő lépéseket kell elvégeznie:

1. **[Konfigurálja az Azure ad egyszeri bejelentkezést](#configure-azure-ad-single-sign-on)** , hogy engedélyezze a szolgáltatást a felhasználók számára.
2. **[Konfigurálja az Proxyclick egyszeri bejelentkezést](#configure-proxyclick-single-sign-on)** az alkalmazás oldalán.
3. **[Hozzon létre egy Azure ad-tesztelési felhasználót](#create-an-azure-ad-test-user)** az Azure ad egyszeri bejelentkezés teszteléséhez.
4. **[Az Azure ad-teszt felhasználójának hozzárendelésével](#assign-the-azure-ad-test-user)** engedélyezheti az Azure ad egyszeri bejelentkezést a felhasználó számára.
5. **[Hozzon létre egy Proxyclick-teszt felhasználót](#create-a-proxyclick-test-user)** , amely a felhasználó Azure ad-képviseletéhez van társítva.
6. Az **[egyszeri bejelentkezés tesztelésével](#test-single-sign-on)** ellenőrizheti, hogy a konfiguráció működik-e.

### <a name="configure-azure-ad-single-sign-on"></a>Az Azure AD egyszeri bejelentkezés konfigurálása

Ebben a szakaszban az Azure AD egyszeri bejelentkezést fogja engedélyezni a Azure Portal.

Az Azure AD egyszeri bejelentkezés Proxyclick való konfigurálásához hajtsa végre a következő lépéseket:

1. A [Azure Portal](https://portal.azure.com/)Proxyclick alkalmazás-integráció lapján válassza az **egyszeri bejelentkezés** lehetőséget:

    ![Egyszeri bejelentkezés kiválasztása](common/select-sso.png)

2. Az egyszeri bejelentkezés **módszerének kiválasztása** párbeszédpanelen válassza az **SAML/ws-fed** üzemmód lehetőséget az egyszeri bejelentkezés engedélyezéséhez:

    ![Egyszeri bejelentkezési módszer kiválasztása](common/select-saml-option.png)

3. Az **egyszeres Sign-On beállítása az SAML-vel** lapon kattintson a **Szerkesztés** ikonra az **alapszintű SAML-konfiguráció** párbeszédpanel megnyitásához:

    ![Szerkesztés ikon](common/edit-urls.png)

4. Ha az **alapszintű SAML-konfiguráció** párbeszédpanelen szeretné konfigurálni az alkalmazást identitásszolgáltató módban, hajtsa végre az alábbi lépéseket.

    ![Alapszintű SAML-konfiguráció párbeszédpanel](common/idp-intiated.png)

    1. Az **azonosító** mezőbe írjon be egy URL-címet ebben a mintában:
   
       `https://saml.proxyclick.com/init/<companyId>`

    1. A **Válasz URL-címe** mezőbe írjon be egy URL-címet ebben a mintában:

       `https://saml.proxyclick.com/consume/<companyId>`

5. Ha az alkalmazást SP-kezdeményezésű módban szeretné konfigurálni, válassza a **további URL-címek beállítása** lehetőséget. A **bejelentkezési URL** -cím mezőben adjon meg egy URL-címet ebben a mintában:
   
   `https://saml.proxyclick.com/init/<companyId>`

    ![Proxyclick tartomány és URL-címek egyszeri bejelentkezési adatai](common/metadata-upload-additional-signon.png)

    

    > [!NOTE]
    > Ezek az értékek helyőrzők. A tényleges azonosítót, a válasz URL-címét és a bejelentkezési URL-címet kell használnia. Ezen értékek lekérésének lépései az oktatóanyag későbbi részében olvashatók.

6. Az **egyszeres Sign-On beállítása az SAML** használatával lapon az **SAML aláíró tanúsítvány** szakaszban válassza a **tanúsítvány (Base64)** melletti **letöltési** hivatkozást, és az igényeinek megfelelő beállítást, és mentse a tanúsítványt a számítógépre:

    ![Tanúsítvány letöltési hivatkozása](common/certificatebase64.png)

7. A **Proxyclick beállítása** szakaszban másolja a megfelelő URL-címeket a követelmények alapján:

    ![A konfigurációs URL-címek másolása](common/copy-configuration-urls.png)

    1. **Bejelentkezési URL-cím**.

    1. **Azure ad-azonosító**.

    1. **Kijelentkezési URL-cím**.

### <a name="configure-proxyclick-single-sign-on"></a>Proxyclick egyszeri bejelentkezés konfigurálása

1. Egy új böngészőablakban jelentkezzen be a Proxyclick vállalati webhelyre rendszergazdaként.

2. **Fiók & beállításainak** kiválasztása:

    ![Fiók & beállításainak kiválasztása](./media/proxyclick-tutorial/configure1.png)

3. Görgessen le az **integrációk** szakaszhoz, és válassza az **SAML**:

    ![SAML kiválasztása](./media/proxyclick-tutorial/configure2.png)

4. Az **SAML** szakaszban hajtsa végre a következő lépéseket.

    ![SAML-szakasz](./media/proxyclick-tutorial/configure3.png)

    1. Másolja a **SAML-fogyasztói URL-címet** , és illessze be a **Válasz URL-cím** mezőbe az **alapszintű saml-konfiguráció** párbeszédpanelen a Azure Portal.

    1. Másolja az **SAML SSO-átirányítási URL-címet** , és illessze be a **bejelentkezési URL** -cím és **azonosító** mezőkbe a Azure Portal **alapszintű SAML-konfiguráció** párbeszédpanelén.

    1. Az **SAML-kérelem módszere** listán válassza a **http-átirányítás** lehetőséget.

    1. A **kiállító** mezőben illessze be a Azure Portalból másolt **Azure ad-azonosító** értékét.

    1. Az **SAML 2,0-végpont URL-címe** mezőben illessze be a Azure Portalból másolt **bejelentkezési URL** -értéket.

    1. Nyissa meg a Jegyzettömbben a Azure Portal letöltött tanúsítványfájl. Illessze be a fájl tartalmát a **tanúsítvány** mezőbe.

    1. Válassza a **módosítások mentése** lehetőséget.

### <a name="create-an-azure-ad-test-user"></a>Azure AD-tesztkörnyezet létrehozása

Ebben a szakaszban egy Britta Simon nevű teszt felhasználót hoz létre a Azure Portal.

1. A Azure Portal a bal oldali ablaktáblán válassza a **Azure Active Directory** lehetőséget, válassza a **felhasználók** lehetőséget, majd válassza a **minden felhasználó** lehetőséget:

    ![Válassza a Minden felhasználó lehetőséget](common/users.png)

2. Válassza ki a képernyő felső részén található **új felhasználó** elemet:

    ![Új felhasználó kiválasztása](common/new-user.png)

3. A **felhasználó** párbeszédpanelen hajtsa végre a következő lépéseket.

    ![Felhasználó párbeszédpanel](common/user-properties.png)

    1. A név mezőbe írja be a **BrittaSimon** **nevet** .
  
    1. A **Felhasználónév** mezőbe írja be a **BrittaSimon@ \<yourcompanydomain> . \<extension>**. (Például: BrittaSimon@contoso.com .)

    1. Válassza a **jelszó megjelenítése** lehetőséget, majd írja le a **jelszó** mezőben található értéket.

    1. Válassza a **Létrehozás** lehetőséget.

### <a name="assign-the-azure-ad-test-user"></a>Az Azure AD-teszt felhasználójának kiosztása

Ebben a szakaszban a Britta Simon használatával engedélyezheti az Azure egyszeri bejelentkezést azáltal, hogy hozzáférést biztosít a Proxyclick.

1. A Azure Portal válassza a **vállalati alkalmazások** lehetőséget, válassza a **minden alkalmazás** lehetőséget, majd válassza a **Proxyclick** lehetőséget.

    ![Vállalati alkalmazások panel](common/enterprise-applications.png)

2. Az alkalmazások listájában válassza a **Proxyclick** lehetőséget.

    ![Alkalmazások listája](common/all-applications.png)

3. A bal oldali ablaktáblán válassza a **felhasználók és csoportok** lehetőséget:

    ![Felhasználók és csoportok kiválasztása](common/users-groups-blade.png)

4. Válassza a **felhasználó hozzáadása** lehetőséget, majd a **hozzárendelés hozzáadása** párbeszédpanelen válassza a **felhasználók és csoportok** lehetőséget.

    ![Felhasználó hozzáadása kiválasztása](common/add-assign-user.png)

5. A **felhasználók és csoportok** párbeszédpanelen válassza a **Britta Simon** elemet a felhasználók listában, majd kattintson az ablak alján található **kiválasztás** gombra.

6. Ha az SAML-állításban a szerepkör értéke várható, a **szerepkör kiválasztása** párbeszédpanelen válassza ki a megfelelő szerepkört a felhasználó számára a listából. Kattintson az ablak alján található **kiválasztás** gombra.

7. A **hozzárendelés hozzáadása** párbeszédpanelen válassza a **hozzárendelés** lehetőséget.

### <a name="create-a-proxyclick-test-user"></a>Proxyclick-teszt felhasználó létrehozása

Ha engedélyezni szeretné, hogy az Azure AD-felhasználók bejelentkezzenek a Proxyclick, hozzá kell adnia őket a Proxyclick-hez. Ezeket manuálisan kell felvennie.

Felhasználói fiók létrehozásához hajtsa végre a következő lépéseket:

1. Jelentkezzen be a Proxyclick vállalati webhelyre rendszergazdaként.

1. Válassza ki a **munkatársakat** az ablak tetején:

    ![Munkatársak kiválasztása](./media/proxyclick-tutorial/user1.png)

1. Válassza a **munkatárs hozzáadása** lehetőséget:

    ![Válassza a munkatárs hozzáadása lehetőséget](./media/proxyclick-tutorial/user2.png)

1. A **munkatárs hozzáadása** szakaszban hajtsa végre az alábbi lépéseket.

    ![Munkatárs szakasz hozzáadása](./media/proxyclick-tutorial/user3.png)

    1. Az **e-mail** mezőbe írja be a felhasználó e-mail-címét. Ebben az esetben a **brittasimon \@ contoso.com**.

    1. A **keresztnév** mezőbe írja be a felhasználó utónevét. Ebben az esetben a **Britta**.

    1. A **vezetéknév** mezőbe írja be a felhasználó vezetéknevét. Ebben az esetben **Simon**.

    1. Válassza a **felhasználó hozzáadása** elemet.

### <a name="test-single-sign-on"></a>Az egyszeri bejelentkezés tesztelése

Most az Azure AD egyszeri bejelentkezési konfigurációját a hozzáférési panel használatával kell tesztelni.

Amikor kiválasztja a Proxyclick csempét a hozzáférési panelen, automatikusan be kell jelentkeznie arra a Proxyclick-példányra, amelyhez be szeretné állítani az egyszeri bejelentkezést. További információ a hozzáférési panelről: [alkalmazások elérése és használata a saját alkalmazások portálon](../user-help/my-apps-portal-end-user-access.md).

## <a name="additional-resources"></a>További források

- [Oktatóanyagok SaaS-alkalmazások az Azure Active Directoryval való integrálásához](./tutorial-list.md)

- [Mi az az alkalmazás-hozzáférés és az egyszeri bejelentkezés az Azure Active Directoryval?](../manage-apps/what-is-single-sign-on.md)

- [Mi a feltételes hozzáférés a Azure Active Directory?](../conditional-access/overview.md)