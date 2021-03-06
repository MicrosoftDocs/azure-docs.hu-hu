---
title: Ügyfél- és kiszolgálói architektúra
titleSuffix: An Azure Communication Services concept document
description: Ismerje meg a kommunikációs szolgáltatások architektúráját.
author: mikben
manager: mikben
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: conceptual
ms.service: azure-communication-services
ms.openlocfilehash: 5ee33c032293329a6af69a0b2be691092c2a8da4
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105729643"
---
# <a name="client-and-server-architecture"></a>Ügyfél és kiszolgáló architektúrája

Minden Azure Communication Services-alkalmazáshoz olyan **ügyfélalkalmazások** tartoznak, amelyek **szolgáltatásokat** használnak a személyek közötti kapcsolat megkönnyítésére. Ez az oldal számos különböző forgatókönyvben mutatja be a közös építészeti elemeket.

## <a name="user-access-management"></a>Felhasználói hozzáférés kezelése

Az Azure kommunikációs szolgáltatások SDK `user access tokens` -k a kommunikációs szolgáltatások erőforrásainak biztonságos elérését igénylik. `User access tokens` egy megbízható szolgáltatásnak kell létrehoznia és felügyelni a jogkivonat bizalmas jellege és a létrehozásához szükséges kapcsolati karakterlánc miatt. A hozzáférési tokenek megfelelő kezelésének elmulasztása további díjakat eredményezhet az erőforrásokkal való visszaélés miatt. Javasoljuk, hogy használjon megbízható szolgáltatást a felhasználók felügyeletéhez. A megbízható szolgáltatás létrehozza a jogkivonatokat, és a megfelelő titkosítás használatával továbbítja azokat az ügyfélnek. A minta architektúra folyamata az alábbiakban található:

:::image type="content" source="../media/scenarios/archdiagram-access.png" alt-text="A felhasználói hozzáférési jogkivonat architektúráját bemutató ábra.":::

További információkért tekintse át a [legjobb Identitáskezelés-kezelési gyakorlatot](../../security/fundamentals/identity-management-best-practices.md)

## <a name="browser-communication"></a>Böngészőalapú kommunikáció

Az Azure Communications JavaScript SDK-k lehetővé teszik a webes alkalmazások gazdag szöveg-, hang-és videó-interakcióval való kezelését. Az alkalmazás közvetlenül az Azure kommunikációs szolgáltatásokkal kommunikál az SDK-ban az adatsíkon való hozzáféréshez és valós idejű szöveg-, hang-és videó-kommunikációhoz. A minta architektúra folyamata az alábbiakban található:

:::image type="content" source="../media/scenarios/archdiagram-browser.png" alt-text="A kommunikációs szolgáltatások böngésző-architektúráját bemutató ábra.":::

## <a name="native-app-communication"></a>Natív alkalmazás kommunikációja

Számos forgatókönyv a legalkalmasabb a natív alkalmazásokhoz. Az Azure kommunikációs szolgáltatások a böngészők közötti és az alkalmazások közötti kommunikációt is támogatják.  Natív alkalmazások létrehozásakor a leküldéses értesítések lehetővé teszik, hogy a felhasználók akkor is fogadhasson hívásokat, ha az alkalmazás nem fut. Az Azure kommunikációs szolgáltatások megkönnyítik az integrált leküldéses értesítéseket a Google Firebase, a Apple Push Notification Service és a Windows leküldéses értesítésekhez. A minta architektúra folyamata az alábbiakban található:

:::image type="content" source="../media/scenarios/archdiagram-app.png" alt-text="A kommunikációs szolgáltatások architektúráját bemutató ábra a natív alkalmazással való kommunikációhoz.":::

## <a name="voice-and-sms-over-the-public-switched-telephony-network-pstn"></a>Hang és SMS a nyilvános kapcsolóval rendelkező telefonos hálózaton (PSTN) keresztül

A telefonos rendszeren keresztüli kommunikáció jelentősen növelheti az alkalmazás elérhetőségét. A PSTN hang-és SMS-forgatókönyvek támogatásához az Azure kommunikációs szolgáltatásokkal közvetlenül a Azure Portal, illetve a REST API-k és SDK-k használatával [szerezhetők be telefonszámok](../quickstarts/telephony-sms/get-phone-number.md) . A telefonszámok beszerzése után a rendszer felhasználhatja az ügyfeleket a PSTN-hívással és az SMS-sel együtt a bejövő és kimenő helyzetekben is. A minta architektúra folyamata az alábbiakban található:

> [!Note]
> A nyilvános előzetes verzióban az Egyesült államokbeli telefonszámok kiosztása az Egyesült Államokban és Kanadában található számlázási címmel rendelkező ügyfelek számára érhető el.

:::image type="content" source="../media/scenarios/archdiagram-pstn.png" alt-text="A kommunikációs szolgáltatások PSTN-architektúráját bemutató ábra.":::

A PSTN-telefonszámokkal kapcsolatos további információkért lásd: [telefonszám-típusok](../concepts/telephony-sms/plan-solution.md)

## <a name="humans-communicating-with-bots-and-other-services"></a>A botokkal és más szolgáltatásokkal kommunikáló emberek

Az Azure kommunikációs szolgáltatások az Azure kommunikációs szolgáltatások adatsíkjával közvetlenül hozzáférő szolgáltatásokkal támogatják az emberi és a rendszer közötti kommunikációt, de szöveges és hangcsatornákat is. Lehet például, hogy a bot megválaszolja a bejövő telefonhívásokat, vagy részt vesz egy webes csevegésben. Az Azure kommunikációs szolgáltatások olyan SDK-kat biztosít, amelyek lehetővé teszik ezen forgatókönyvek meghívását és csevegését. A minta architektúra folyamata az alábbiakban található:

:::image type="content" source="../media/scenarios/archdiagram-bot.png" alt-text="A kommunikációs szolgáltatások bot architektúráját bemutató ábra.":::

## <a name="networking"></a>Hálózatkezelés

Előfordulhat, hogy tetszőleges adatokat kell cserélnie a felhasználók között, például szinkronizálnia kell a közös, vegyes valóságot vagy játékélményt. A szöveg-, hang-és videó-kommunikációhoz használt valós idejű adatsíkok kétféleképpen érhetők el:

- Az **SDK meghívása** egy hívásban az API-k számára az adatok hívási csatornán keresztül történő küldéséhez és fogadásához is hozzáférnek. Ez a legegyszerűbb módszer arra, hogy adatkommunikációt vegyen fel egy meglévő interakcióba.
- A **kábító/turn** -Azure kommunikációs szolgáltatások szabványoknak megfelelő kábítást tesznek lehetővé, és a szolgáltatások elérhetővé válnak. Ez lehetővé teszi egy nagy mértékben testreszabott átviteli réteg létrehozását ezen szabványosított primitívek felett. Létrehozhat saját szabványoknak megfelelő ügyfelet, vagy használhat nyílt forráskódú kódtárakat, például a [WinRTC](https://github.com/microsoft/winrtc)-t.

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [Felhasználói hozzáférési tokenek létrehozása](../quickstarts/access-tokens.md)

További információért tekintse át a következő cikkeket:

- A [hitelesítés](../concepts/authentication.md) ismertetése
- Tudnivalók a [telefonszámok típusairól](../concepts/telephony-sms/plan-solution.md)

- [Csevegés hozzáadása az alkalmazáshoz](../quickstarts/chat/get-started.md)
- [Hanghívás hozzáadása az alkalmazáshoz](../quickstarts/voice-video-calling/getting-started-with-calling.md)
