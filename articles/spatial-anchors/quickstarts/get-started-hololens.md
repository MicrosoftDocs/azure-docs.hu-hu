---
title: 'Rövid útmutató: HoloLens-alkalmazás létrehozása a DirectX szolgáltatással'
description: Ebből a rövid útmutatóból megtudhatja, hogyan hozhat létre HoloLens-alkalmazást térbeli horgonyok használatával.
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 11/20/2020
ms.topic: quickstart
ms.service: azure-spatial-anchors
ms.openlocfilehash: c96c45869ee1c9c96cd77d0b3eb10c733199666e
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/23/2020
ms.locfileid: "95993516"
---
# <a name="quickstart-create-a-hololens-app-with-azure-spatial-anchors-in-cwinrt-and-directx"></a>Gyors útmutató: HoloLens-alkalmazás létrehozása Azure térbeli Horgonyokkal, C++/WinRT és DirectX-ben

Ez a rövid útmutató bemutatja, hogyan hozhat létre HoloLens-alkalmazást az [Azure térbeli horgonyokkal](../overview.md) a C++/WinRT és a DirectX használatával. Az Azure térbeli horgonyok egy többplatformos fejlesztői szolgáltatás, amely lehetővé teszi, hogy vegyes valóságot hozzon létre olyan objektumok használatával, amelyek az adott helyen maradnak a helyükön az egyes eszközökön. Ha elkészült, egy HoloLens-alkalmazás fog rendelkezni, amely képes a térbeli horgonyok mentésére és visszahívására.

A következőket fogja megtanulni:

> [!div class="checklist"]
> * Térbeli horgonyok fiók létrehozása
> * A térbeli horgonyok fiókazonosító és a fiók kulcsának konfigurálása
> * Üzembe helyezés és Futtatás HoloLens-eszközön

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Előfeltételek

A rövid útmutató elvégzéséhez győződjön meg arról, hogy rendelkezik az alábbiakkal:
- A **univerzális Windows-platform-fejlesztési** számítási feladattal és a **Windows 10 SDK-val (10.0.18362.0 vagy újabb** verzióval) telepített Windows-gép a <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2019</a> -es verziójával. <a href="https://git-scm.com/download/win" target="_blank">A git for Windows és a</a> <a href="https://git-lfs.github.com/">git LFS</a>is telepítenie kell.
- A Visual studióhoz készült [C++/WinRT Visual Studio bővítményt (VSIX)](https://aka.ms/cppwinrt/vsix) a [Visual Studio piactérről](https://marketplace.visualstudio.com/)kell telepíteni.
- HoloLens-eszköz, amelyen engedélyezve van a [fejlesztői mód](/windows/mixed-reality/using-visual-studio) . Ehhez a cikkhez HoloLens-eszközre van szükség, amely a [Windows 10 2020](/windows/mixed-reality/whats-new/release-notes-may-2020)-es verziójának frissítését igényli. A HoloLens legújabb kiadásának frissítéséhez nyissa meg a **Beállítások** alkalmazást, lépjen a **frissítés & biztonság** elemre, majd kattintson a **frissítések keresése** gombra.
- Az alkalmazásnak a AppX-jegyzékben kell beállítania a **spatialPerception** képességet.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="open-the-sample-project"></a>A minta projekt megnyitása

[!INCLUDE [Clone Sample Repo](../../../includes/spatial-anchors-clone-sample-repository.md)]

Megnyitás `HoloLens\DirectX\SampleHoloLens.sln` a Visual Studióban.

## <a name="configure-account-identifier-and-key"></a>Fiók azonosítójának és kulcsának konfigurálása

A következő lépés az alkalmazás konfigurálása a fiók azonosítójának és a fiók kulcsának használatára. [A térbeli horgonyok erőforrásának beállításakor](#create-a-spatial-anchors-resource)egy szövegszerkesztőbe másolta őket.

Nyissa meg a következő fájlt: `HoloLens\DirectX\SampleHoloLens\ViewController.cpp`.

Keresse meg a `SpatialAnchorsAccountKey` mezőt, és cserélje le a `Set me` fiókot a fiók kulcsára.

Keresse meg a `SpatialAnchorsAccountId` mezőt, és cserélje le a azonosítót `Set me` a fiókazonosító értékre.

Keresse meg a `SpatialAnchorsAccountDomain` mezőt, és cserélje le a `Set me` fiókot a fiók tartományára.

## <a name="deploy-the-app-to-your-hololens"></a>Az alkalmazás üzembe helyezése a HoloLens

Módosítsa a **megoldás konfigurációját** a **kiadásra**, módosítsa a **megoldási platformot** **x86**-ra, majd válassza az **eszköz** lehetőséget a telepítési cél beállításai közül.

Ha a 2. HoloLens használja, a **ARM64** -et a **megoldási platformként** használhatja az **x86** helyett.

![Visual Studio-konfiguráció](./media/get-started-hololens/visual-studio-configuration.png)

Kapcsolja be a HoloLens eszközt, jelentkezzen be, és csatlakoztassa a számítógéphez egy USB-kábellel.

Válassza a **hibakeresés**  >  **indítása** az alkalmazás üzembe helyezéséhez és a hibakeresés megkezdéséhez lehetőséget.

A horgonyok elhelyezéséhez és felidézéséhez kövesse az alkalmazás utasításait.

A Visual Studióban állítsa le az alkalmazást úgy, hogy kiválasztja a **hibakeresés leállítása** vagy a **SHIFT + F5** billentyűkombinációt.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

[!INCLUDE [Next steps](../../../includes/spatial-anchors-quickstarts-nextsteps.md)]

> [!div class="nextstepaction"]
> [Oktatóanyag: térbeli horgonyok megosztása az eszközök között](../tutorials/tutorial-share-anchors-across-devices.md)