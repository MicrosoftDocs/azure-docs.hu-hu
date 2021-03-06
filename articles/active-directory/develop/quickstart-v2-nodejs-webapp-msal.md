---
title: 'Rövid útmutató: hitelesítés hozzáadása egy Node-webalkalmazáshoz a MSAL Node használatával | Azure'
titleSuffix: Microsoft identity platform
description: Ebből a rövid útmutatóból megtudhatja, hogyan valósítható meg a hitelesítés egy Node.js webalkalmazással és a Microsoft Authentication Library (MSAL) szolgáltatással a Node.jshoz.
services: active-directory
author: mmacy
manager: celested
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 10/22/2020
ms.author: marsma
ms.custom: aaddev, scenarios:getting-started, languages:js, devx-track-js
ms.openlocfilehash: 72eb6e77cfbcae662181f642393085185514eed6
ms.sourcegitcommit: b0557848d0ad9b74bf293217862525d08fe0fc1d
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106550948"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-a-node-web-app-using-the-auth-code-flow"></a>Gyors útmutató: bejelentkezés a felhasználókba és hozzáférési jogkivonat beszerzése egy Node-webalkalmazásban az Auth Code flow használatával

Ebben a rövid útmutatóban egy kód mintát tölt le és futtat, amely bemutatja, hogyan jelentkezhet be Node.js webalkalmazás a felhasználók számára az engedélyezési kód folyamatával. A kód minta azt is bemutatja, hogyan kérhető le Microsoft Graph API-t meghívó hozzáférési jogkivonat. 

Nézze meg, [Hogyan működik a minta](#how-the-sample-works) egy ábrán.

Ez a rövid útmutató a Microsoft hitelesítési függvénytárát használja Node.jshoz (MSAL node) az engedélyezési kód folyamatával.

## <a name="prerequisites"></a>Előfeltételek

* Azure-előfizetés – [ingyenes Azure-előfizetés létrehozása](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/download) vagy más Kódszerkesztő

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>A gyors üzembe helyezési alkalmazás regisztrálása és letöltése
>
> #### <a name="step-1-register-your-application"></a>1. lépés: Alkalmazás regisztrálása
>
> 1. Jelentkezzen be az <a href="https://portal.azure.com/" target="_blank">Azure Portalra</a>.
> 1. Ha több bérlőhöz fér hozzá, a felső menüben a **könyvtár + előfizetés** szűrő használatával :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: válassza ki azt a bérlőt, amelyben regisztrálni kíván egy alkalmazást.
> 1. A **kezelés** területen válassza a **Alkalmazásregisztrációk**  >  **új regisztráció** lehetőséget.
> 1. Adja meg az alkalmazás **nevét** . Előfordulhat, hogy az alkalmazás felhasználói láthatják ezt a nevet, és később is megváltoztathatók.
> 1. A **támogatott fiókok típusai** területen válassza a **fiókok lehetőséget bármely szervezeti címtárban és személyes Microsoft-fiókban**.
> 1. Állítsa be az **átirányítási URI** értékét a következőre: `http://localhost:3000/redirect` .
> 1. Válassza a **Regisztráció** lehetőséget. 
> 1. Az alkalmazás **áttekintése** lapon jegyezze fel az **alkalmazás (ügyfél) azonosítójának** értékét későbbi használatra.
> 1. A **kezelés** területen válassza a **tanúsítványok & titkos kulcsok**  >  **új ügyfél titka** lehetőséget.  Hagyja üresen a leírást és az alapértelmezett lejáratot, majd válassza a **Hozzáadás** lehetőséget.
> 1. Jegyezze fel az **ügyfél titkos kulcsának** **értékét** későbbi használatra.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-the-application-in-azure-portal"></a>1. lépés: az alkalmazás konfigurálása Azure Portalban
> Ahhoz, hogy a rövid útmutatóhoz tartozó mintakód működjön, létre kell hoznia egy ügyfél-titkot, és hozzá kell adnia egy válasz URL-címet **http://localhost:3000/redirect** .
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [A módosítás alkalmazása]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Már konfigurált](media/quickstart-v2-windows-desktop/green-check.png) Az alkalmazása már konfigurálva van ezekkel az attribútumokkal.

#### <a name="step-2-download-the-project"></a>2. lépés: A projekt letöltése

> [!div renderon="docs"]
> Ha Node.js használatával szeretné futtatni a projektet egy webkiszolgálóval, [töltse le az alapprojekt fájljait](https://github.com/Azure-Samples/ms-identity-node/archive/main.zip).

> [!div renderon="portal" class="sxs-lookup"]
> A projekt futtatása webkiszolgálóval Node.js használatával

> [!div renderon="portal" class="sxs-lookup" id="autoupdate" class="nextstepaction"]
> [A mintakód letöltése](https://github.com/Azure-Samples/ms-identity-node/archive/main.zip)

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-node-app"></a>3. lépés: a Node-alkalmazás konfigurálása
>
> Bontsa ki a projektet, és nyissa meg az *MS-Identity-Node-Main* mappát, majd nyissa meg a *index.js* fájlt.
> Állítsa be az `clientID` **alkalmazást az alkalmazás (ügyfél) azonosítójával**.
> Állítsa be az `clientSecret` értéket az **ügyfél titkának** **értékével** .
>
>```javascript
>const config = {
>    auth: {
>        clientId: "Enter_the_Application_Id_Here",
>        authority: "https://login.microsoftonline.com/common",
>        clientSecret: "Enter_the_Client_Secret_Here"
>    },
>    system: {
>        loggerOptions: {
>            loggerCallback(loglevel, message, containsPii) {
>                console.log(message);
>            },
>            piiLoggingEnabled: false,
>            logLevel: msal.LogLevel.Verbose,
>        }
>    }
>};
> ```

> [!div renderon="docs"]
>
> Módosítsa a szakasz értékeit az `config` itt leírtak szerint:
>
> - `Enter_the_Application_Id_Here` a regisztrált alkalmazáshoz tartozó **alkalmazás (ügyfél) azonosítója** .
>
>    Az **alkalmazás (ügyfél) azonosító** értékének megkereséséhez nyissa meg a Azure Portal az alkalmazás regisztrációjának **Áttekintés** lapját.
> - `Enter_the_Client_Secret_Here`a regisztrált alkalmazáshoz tartozó **ügyfél-titkos kulcs** **értéke** .
>
>    Új **ügyfél-titkos** kód lekéréséhez vagy létrehozásához a **kezelés** területen válassza a **tanúsítványok & titkok** elemet.
>
> Az alapértelmezett `authority` érték a fő (globális) Azure-felhőt jelenti:
>
> ```javascript
> authority: "https://login.microsoftonline.com/common",
> ```
>
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>3. lépés: az alkalmazás konfigurálva van, és készen áll a futtatásra
>
> [!div renderon="docs"]
>
> #### <a name="step-4-run-the-project"></a>4. lépés: a projekt futtatása

Futtassa a projektet Node.js használatával:

1. A kiszolgáló elindításához futtassa a következő parancsokat a projekt könyvtárából:
    ```console
    npm install
    npm start
    ```
1. Nyissa meg a következő címet: `http://localhost:3000/`.

1. A bejelentkezési folyamat elindításához válassza **a bejelentkezés** lehetőséget.

    Amikor először jelentkezik be, a rendszer arra kéri, hogy adja meg az engedélyt, hogy az alkalmazás hozzáférhessen a profiljához, és jelentkezzen be. Miután sikeresen bejelentkezett, megjelenik egy naplófájl a parancssorban.

## <a name="more-information"></a>További információ

### <a name="how-the-sample-works"></a>A minta működése

A futtatáskor a rendszer egy webkiszolgálót üzemeltet a localhost-on, a 3000-es portot. Amikor egy webböngésző hozzáfér a webhelyhez, a minta azonnal átirányítja a felhasználót egy Microsoft-hitelesítési lapra. Emiatt a minta nem tartalmaz *HTML* -vagy megjelenített elemeket. A sikeres hitelesítés az "OK" üzenetet jeleníti meg.

### <a name="msal-node"></a>MSAL csomópont

A MSAL-csomópont könyvtára aláírja a felhasználókat, és a Microsoft Identity platform által védett API eléréséhez használt jogkivonatokat kéri. A legújabb verziót a Node.js Package Manager (NPM) segítségével töltheti le:

```console
npm install @azure/msal-node
```

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [Hitelesítés hozzáadása meglévő webalkalmazáshoz – GitHub-mintakód >](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/samples/msal-node-samples/auth-code)
