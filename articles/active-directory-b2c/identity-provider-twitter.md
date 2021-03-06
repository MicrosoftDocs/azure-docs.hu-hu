---
title: Regisztráció és bejelentkezés beállítása Twitter-fiókkal
titleSuffix: Azure AD B2C
description: Az alkalmazások Twitter-fiókkal való regisztrációját és bejelentkezését Azure Active Directory B2C használatával biztosíthatja az ügyfeleknek.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 04/06/2021
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 97a8134e858112d7e1deff6744b5555c172692f2
ms.sourcegitcommit: d40ffda6ef9463bb75835754cabe84e3da24aab5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/07/2021
ms.locfileid: "107028179"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Twitter-fiókkal való regisztráció és bejelentkezés beállítása Azure Active Directory B2C használatával

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]
::: zone pivot="b2c-custom-policy"

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

::: zone-end

## <a name="prerequisites"></a>Előfeltételek

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="create-an-application"></a>Alkalmazás létrehozása

Ha Azure AD B2C Twitter-fiókkal rendelkező felhasználók számára szeretné engedélyezni a bejelentkezést, létre kell hoznia egy Twitter-alkalmazást. Ha még nem rendelkezik Twitter-fiókkal, regisztrálhat a következő címen: [https://twitter.com/signup](https://twitter.com/signup) . [Egy fejlesztői fiókra](https://developer.twitter.com/en/apply/user.html)is szüksége lesz. További információ: [Apply for Access](https://developer.twitter.com/en/apply-for-access).

1. Jelentkezzen be a [Twitter fejlesztői portálra](https://developer.twitter.com/portal/projects-and-apps) a Twitter-fiókja hitelesítő adataival.
1. Az **önálló alkalmazások** területen válassza az **+ alkalmazás létrehozása** lehetőséget.
1. Adja meg az **alkalmazás nevét**, majd kattintson a **Befejezés** gombra.
1. Másolja az **alkalmazás kulcsának** és az API- **kulcs titkának** értékét.  Mindkettőt a Twitter identitás-szolgáltatóként való konfigurálásához használja a bérlőben. 
1. **Az alkalmazás beállítása** területen válassza az **Alkalmazásbeállítások** lehetőséget.
1. A **hitelesítési beállítások** területen válassza a **Szerkesztés** lehetőséget.
    1. Jelölje be a **3 lábú OAuth engedélyezése** jelölőnégyzetet.
    1. Válassza **a kérelem e-mail címe a felhasználók számára** jelölőnégyzetet.
    1. A **visszahívási URL-címek** esetében adja meg a értéket `https://your-tenant.b2clogin.com/your-tenant-name.onmicrosoft.com/your-user-flow-Id/oauth1/authresp` .  Ha [Egyéni tartományt](custom-domain.md)használ, írja be a értéket `https://your-domain-name/your-tenant-name.onmicrosoft.com/your-user-flow-Id/oauth1/authresp` . Ha a bérlő nevét és a felhasználói folyamat AZONOSÍTÓját adja meg, akkor is használja az összes kisbetűt, ha Azure AD B2C nagybetűvel vannak meghatározva. Cserélje le ezt:
        - `your-tenant-name` a bérlő nevének nevével.
        - `your-domain-name` az egyéni tartománnyal.
        - `your-user-flow-Id` a felhasználói folyamat azonosítójával. Például: `b2c_1a_signup_signin_twitter`. 
    
    1. A **webhely URL-címe** mezőbe írja be a következőt: `https://your-tenant.b2clogin.com` . Cserélje le a helyére a `your-tenant` bérlő nevét. Például: `https://contosob2c.b2clogin.com`. Ha [Egyéni tartományt](custom-domain.md)használ, írja be a értéket `https://your-domain-name` .
    1. Adja meg a **szolgáltatási feltételek** URL-címét, például: `http://www.contoso.com/tos` . A szabályzat URL-címe egy olyan oldal, amelyet fenntart az alkalmazás használati feltételeinek megadásához.
    1. Adja meg az **adatvédelmi szabályzat** URL-címét, például: `http://www.contoso.com/privacy` . A szabályzat URL-címe olyan oldal, amelyet az alkalmazásra vonatkozó adatvédelmi információk biztosítására tart fenn.
    1. Kattintson a **Mentés** gombra.

::: zone pivot="b2c-user-flow"

## <a name="configure-twitter-as-an-identity-provider"></a>A Twitter konfigurálása identitás-szolgáltatóként

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com/) az Azure AD B2C-bérlő globális rendszergazdájaként.
1. Győződjön meg arról, hogy a Azure AD B2C bérlőjét tartalmazó könyvtárat használja, majd a felső menüben válassza ki a **címtár + előfizetés** szűrőt, és válassza ki a bérlőt tartalmazó könyvtárat.
1. Válassza az Azure Portal bal felső sarkában található **Minden szolgáltatás** lehetőséget, majd keresse meg és válassza ki az **Azure AD B2C**-t.
1. Válassza az **identitás-szolgáltatók**, majd a **Twitter** lehetőséget.
1. Adjon meg egy **nevet**. Például: *Twitter*.
1. Az **ügyfél-azonosító** mezőben adja meg a korábban létrehozott Twitter-alkalmazás *API-kulcsát* .
1. Az **ügyfél titka** mezőben adja meg a rögzített *API-kulcs titkát* .
1. Kattintson a **Mentés** gombra.

## <a name="add-twitter-identity-provider-to-a-user-flow"></a>Twitter-identitás szolgáltatójának hozzáadása felhasználói folyamathoz 

Ezen a ponton a Twitter-identitás szolgáltatója be van állítva, de a bejelentkezési oldalakon még nem érhető el. Twitter-identitás szolgáltató hozzáadása felhasználói folyamathoz:

1. A Azure AD B2C-bérlőben válassza a **felhasználói folyamatok** lehetőséget.
1. Válassza ki azt a felhasználói folyamatot, amelyhez hozzá szeretné adni a Twitter-identitás szolgáltatóját.
1. A **közösségi identitás-szolgáltatók** területen válassza a **Twitter** lehetőséget.
1. Kattintson a **Mentés** gombra.
1. A szabályzat teszteléséhez válassza a **felhasználói folyamat futtatása** lehetőséget.
1. Az **alkalmazás** lapon válassza ki a korábban regisztrált *testapp1* nevű webalkalmazást. A **Válasz URL-címének** meg kell jelennie `https://jwt.ms` .
1. Kattintson a **felhasználói folyamat futtatása** gombra.
1. A regisztrációs vagy bejelentkezési oldalon válassza a **Twitter** lehetőséget a Twitter-fiókkal való bejelentkezéshez.

Ha a bejelentkezési folyamat sikeres, a rendszer átirányítja a böngészőt `https://jwt.ms` , amely a Azure ad B2C által visszaadott jogkivonat tartalmát jeleníti meg.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="create-a-policy-key"></a>Házirend-kulcs létrehozása

A Azure AD B2C bérlőben korábban rögzített titkos kulcsot kell tárolnia.

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).
2. Győződjön meg arról, hogy a Azure AD B2C bérlőjét tartalmazó könyvtárat használja. Válassza ki a **címtár + előfizetés** szűrőt a felső menüben, és válassza ki a bérlőt tartalmazó könyvtárat.
3. Válassza ki az **összes szolgáltatást** a Azure Portal bal felső sarkában, majd keresse meg és válassza ki a **Azure ad B2C**.
4. Az Áttekintés lapon válassza az **identitási élmény keretrendszert**.
5. Válassza a **szabályzat kulcsok** lehetőséget, majd kattintson a **Hozzáadás** gombra.
6. A **Beállítások** területen válassza a lehetőséget `Manual` .
7. Adja meg a szabályzat kulcsának **nevét** . Például: `TwitterSecret`. A rendszer automatikusan hozzáadja az előtagot a `B2C_1A_` kulcs nevéhez.
8. A **Secret (titkos kulcs**) mezőben adja meg a korábban rögzített ügyfél-titkot.
9. A **kulcshasználat** beállításnál válassza a elemet `Encryption` .
10. Kattintson a **Létrehozás** lehetőségre.

## <a name="configure-twitter-as-an-identity-provider"></a>A Twitter konfigurálása identitás-szolgáltatóként

Annak engedélyezéséhez, hogy a felhasználók Twitter-fiókkal jelentkezzenek be, meg kell adnia a fiókot jogcím-szolgáltatóként, amely Azure AD B2C tud kommunikálni egy végponton keresztül. A végpont olyan jogcímeket biztosít, amelyeket a Azure AD B2C használ annak ellenőrzéséhez, hogy egy adott felhasználó hitelesítve van-e.

A Twitter-fiókot jogcím-szolgáltatóként is meghatározhatja, ha hozzáadja azt a **ClaimsProviders** elemhez a szabályzat bővítmény fájljában.

1. Nyissa meg a *TrustFrameworkExtensions.xml*.
2. Keresse meg a **ClaimsProviders** elemet. Ha nem létezik, adja hozzá a gyökérelem elemhez.
3. Vegyen fel egy új **ClaimsProvider** a következőképpen:

    ```xml
    <ClaimsProvider>
      <Domain>twitter.com</Domain>
      <DisplayName>Twitter</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Twitter-OAuth1">
          <DisplayName>Twitter</DisplayName>
          <Protocol Name="OAuth1" />
          <Metadata>
            <Item Key="ProviderName">Twitter</Item>
            <Item Key="authorization_endpoint">https://api.twitter.com/oauth/authenticate</Item>
            <Item Key="access_token_endpoint">https://api.twitter.com/oauth/access_token</Item>
            <Item Key="request_token_endpoint">https://api.twitter.com/oauth/request_token</Item>
            <Item Key="ClaimsEndpoint">https://api.twitter.com/1.1/account/verify_credentials.json?include_email=true</Item>
            <Item Key="ClaimsResponseFormat">json</Item>
            <Item Key="client_id">Your Twitter application API key</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_TwitterSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="user_id" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="screen_name" />
            <OutputClaim ClaimTypeReferenceId="email" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="twitter.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. Cserélje le a **client_id** értékét a korábban rögzített *API-kulcs titkára* .
5. Mentse a fájlt.

[!INCLUDE [active-directory-b2c-add-identity-provider-to-user-journey](../../includes/active-directory-b2c-add-identity-provider-to-user-journey.md)]


```xml
<OrchestrationStep Order="1" Type="CombinedSignInAndSignUp" ContentDefinitionReferenceId="api.signuporsignin">
  <ClaimsProviderSelections>
    ...
    <ClaimsProviderSelection TargetClaimsExchangeId="TwitterExchange" />
  </ClaimsProviderSelections>
  ...
</OrchestrationStep>

<OrchestrationStep Order="2" Type="ClaimsExchange">
  ...
  <ClaimsExchanges>
    <ClaimsExchange Id="TwitterExchange" TechnicalProfileReferenceId="Twitter-OAuth1" />
  </ClaimsExchanges>
</OrchestrationStep>
```

[!INCLUDE [active-directory-b2c-configure-relying-party-policy](../../includes/active-directory-b2c-configure-relying-party-policy-user-journey.md)]

## <a name="test-your-custom-policy"></a>Egyéni szabályzat tesztelése

1. Válassza ki a függő entitás házirendjét, például: `B2C_1A_signup_signin` .
1. **Alkalmazás** esetén válasszon ki egy [korábban regisztrált](tutorial-register-applications.md)webalkalmazást. A **Válasz URL-címének** meg kell jelennie `https://jwt.ms` .
1. Kattintson a **Futtatás most** gombra.
1. A regisztrációs vagy bejelentkezési oldalon válassza a **Twitter** lehetőséget a Twitter-fiókkal való bejelentkezéshez.

Ha a bejelentkezési folyamat sikeres, a rendszer átirányítja a böngészőt `https://jwt.ms` , amely a Azure ad B2C által visszaadott jogkivonat tartalmát jeleníti meg.

::: zone-end
