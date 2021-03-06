---
title: Közzététel on = helyszíni SharePoint az Application proxyval – Azure AD
description: Ismerteti a helyszíni SharePoint Server és az SAML rendszerhez készült Azure AD Application Proxy integrálásának alapjait.
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 10/02/2019
ms.author: kenwith
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 34aaafcd03e737b1e59529f8001e0c008bd39b70
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/30/2021
ms.locfileid: "104888872"
---
# <a name="integrate-with-sharepoint-saml"></a>Integrálás a SharePoint (SAML) szolgáltatással

Ez a részletes útmutató azt ismerteti, hogyan lehet biztonságossá tenni a [helyszíni SharePoint (SAML) Azure Active Directory](../saas-apps/sharepoint-on-premises-tutorial.md) az Azure ad Application proxy használatával, ahol a szervezet felhasználói (Azure ad, B2B) az interneten keresztül csatlakoznak a sharepointhoz.

> [!NOTE] 
> Ha még nem ismeri az Azure AD Application Proxyt, és többet szeretne megtudni, tekintse [meg a helyszíni alkalmazások távoli elérését az Azure-on keresztül ad Application proxy](./application-proxy.md).

A beállítás három fő előnnyel jár:

- Az Azure AD Application Proxy biztosítja, hogy a hitelesített forgalom elérje a belső hálózatot és a SharePoint-kiszolgálót.
- A felhasználók a szokásos módon érhetik el a SharePoint-webhelyeket VPN használata nélkül.
- Az Azure AD Application Proxy szintjén szabályozhatja a felhasználói hozzárendelések hozzáférését, és növelheti a biztonságot az Azure AD-szolgáltatások, például a feltételes hozzáférés és a Multi-Factor Authentication (MFA) segítségével.

Ehhez a folyamathoz két vállalati alkalmazás szükséges. Az egyik a SharePoint helyszíni példánya, amelyet a katalógusból a felügyelt SaaS-alkalmazások listájára tesz közzé. A második egy helyszíni alkalmazás (nem Gallery-alkalmazás), amelyet az első nagyvállalati katalógus alkalmazás közzétételéhez fog használni.

## <a name="prerequisites"></a>Előfeltételek

A konfiguráció végrehajtásához a következő erőforrásokra van szükség:
 - Egy SharePoint 2013-Farm vagy újabb. A SharePoint-farmnak [integrálva](../saas-apps/sharepoint-on-premises-tutorial.md)kell lennie az Azure ad-vel.
 - Egy Azure AD-bérlő az alkalmazásproxy részét képező csomaggal. További információ az [Azure ad-csomagokról és a díjszabásról](https://azure.microsoft.com/pricing/details/active-directory/).
 - Egy [Egyéni, ellenőrzött tartomány](../fundamentals/add-custom-domain.md) az Azure ad-bérlőben. Az ellenőrzött tartománynak meg kell egyeznie a SharePoint URL-utótagjának.
 - SSL-tanúsítvány szükséges. Tekintse meg az [egyéni tartomány-közzététel](./application-proxy-configure-custom-domain.md)részleteit.
 - A helyszíni Active Directory a felhasználókat szinkronizálni kell Azure AD Connectval, és be kell állítani az Azure-ba való [bejelentkezést](../hybrid/plan-connect-user-signin.md). 
 - Csak felhőalapú és B2B vendég felhasználók számára [hozzáférést kell biztosítania egy vendég fiókhoz a helyszíni SharePoint számára a Azure Portal](../saas-apps/sharepoint-on-premises-tutorial.md#grant-access-to-a-guest-account-to-sharepoint-on-premises-in-the-azure-portal).
 - Egy, a vállalati tartományon belüli gépen telepített és futó alkalmazásproxy-összekötő.


## <a name="step-1-integrate-sharepoint-on-premises-with-azure-ad"></a>1. lépés: a helyszíni SharePoint integrálása az Azure AD-vel 

1. Konfigurálja a helyszíni SharePoint-alkalmazást. További információ: [oktatóanyag: Azure Active Directory egyszeri bejelentkezéses integráció a helyszíni SharePoint-](../saas-apps/sharepoint-on-premises-tutorial.md)környezettel.
2. A következő lépésre való áttérés előtt érvényesítse a konfigurációt. Az ellenőrzéshez próbálja meg a belső hálózatról a helyszíni SharePointhoz hozzáférni, és ellenőrizze, hogy az elérhető-e belsőleg. 


## <a name="step-2-publish-the-sharepoint-on-premises-application-with-application-proxy"></a>2. lépés: a SharePoint helyszíni alkalmazás közzététele az Application proxyval

Ebben a lépésben létrehoz egy alkalmazást az Azure AD-bérlőben, amely alkalmazásproxy-t használ. A külső URL-címet kell megadnia, és meg kell adnia a belső URL-címet, mindkettőt a SharePointban később használják.

> [!NOTE] 
> A belső és külső URL-címnek meg kell egyeznie az 1. lépésben az SAML-alapú alkalmazás konfigurációjának **bejelentkezési URL-címével** .

   ![A bejelentkezési URL-cím értékét megjelenítő képernyőkép.](./media/application-proxy-integrate-with-sharepoint-server/sso-url-saml.png)


 1. Hozzon létre egy új Azure AD Application Proxy alkalmazást egyéni tartománnyal. Részletes útmutatásért lásd: [Egyéni tartományok az Azure ad Application proxyban](./application-proxy-configure-custom-domain.md).

    - Belső URL-cím: " https://portal.contoso.com/ "
    - Külső URL-cím: " https://portal.contoso.com/ "
    - Előzetes hitelesítés: Azure Active Directory
    - URL-címek lefordítása a fejlécekben: nem
    - URL-címek fordítása az alkalmazás törzsében: nem

        ![Képernyőkép, amely az alkalmazás létrehozásához használt beállításokat jeleníti meg.](./media/application-proxy-integrate-with-sharepoint-server/create-application-azure-active-directory.png)

2. Rendelje hozzá a helyszíni SharePoint Gallery-alkalmazáshoz rendelt [csoportokat](../saas-apps/sharepoint-on-premises-tutorial.md#create-an-azure-ad-security-group-in-the-azure-portal) .

3. Végül nyissa meg a **Properties (Tulajdonságok** ) szakaszt, és a felhasználók **számára** **láthatóvá válik?** Ez a beállítás biztosítja, hogy csak az első alkalmazás ikonja megjelenjen a saját alkalmazások portálján ( https://myapplications.microsoft.com) .

   ![Képernyőkép arról, hogy hol állítható be a felhasználók számára láthatóvá? beállítás.](./media/application-proxy-integrate-with-sharepoint-server/configure-properties.png)
 
## <a name="step-3-test-your-application"></a>3. lépés: az alkalmazás tesztelése

Ha külső hálózaton található számítógépről használ böngészőt, navigáljon a közzétételi lépés során konfigurált hivatkozáshoz. Ellenőrizze, hogy be tud-e jelentkezni a beállított tesztelési fiókkal.