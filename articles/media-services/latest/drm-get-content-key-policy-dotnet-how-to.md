---
title: Aláíró kulcs beszerzése egy házirend .NET-ből
description: Ez a témakör bemutatja, hogyan szerezhet be egy aláíró kulcsot a meglévő házirendből Media Services v3 .NET SDK használatával.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: how-to
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: seodec18
ms.openlocfilehash: 71d4b00d3ebb56d72cbb16cd42394c720f29849f
ms.sourcegitcommit: 02bc06155692213ef031f049f5dcf4c418e9f509
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/03/2021
ms.locfileid: "106277641"
---
# <a name="get-a-signing-key-from-the-existing-policy"></a>Aláírókulcs lekérése a meglévő szabályzatból

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

A v3 API egyik fő tervezési alapelve az API biztonságosabbá tétele. a V3 API-k nem adnak vissza titkokat vagy hitelesítő adatokat a **Get** vagy a **List** műveletekhez. Tekintse meg a részletes magyarázatot. További információ: [Azure RBAC és Media Services fiókok](security-rbac-concept.md)

A cikkben szereplő példa azt mutatja be, hogyan lehet a .NET használatával beolvasni az aláíró kulcsot a meglévő szabályzatból. 
 
## <a name="download"></a>Letöltés 

A következő parancs használatával klónozott egy GitHub-tárházat, amely a teljes .NET-mintát tartalmazza a gépen:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
A Secrets ContentKeyPolicy példa a [EncryptWithDRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM) mappában található.

## <a name="get-contentkeypolicy-with-secrets"></a>ContentKeyPolicy beolvasása a titkokkal 

A kulcs eléréséhez használja a **GetPolicyPropertiesWithSecretsAsync** az alábbi példában látható módon.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="next-steps"></a>Következő lépések

[Hozzáférés-vezérléssel ellátott Multi-DRM-rendszerek tervezése](architecture-design-multi-drm-system.md) 
