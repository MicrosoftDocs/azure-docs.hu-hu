---
title: Horgonyok létrehozása & C++/WinRT
description: Részletes útmutató a horgonyok létrehozásához és megkereséséhez az Azure térbeli horgonyok használatával a C++/WinRT.
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 11/20/2020
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: a6b51da695037fd84053ac0bf1b9be091781fbbf
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/23/2020
ms.locfileid: "96018868"
---
# <a name="how-to-create-and-locate-anchors-using-azure-spatial-anchors-in-cwinrt"></a>Horgonyok létrehozása és megkeresése az Azure térbeli horgonyok használatával a C++/WinRT

> [!div  class="op_single_selector"]
> * [Unity](create-locate-anchors-unity.md)
> * [Objective-C](create-locate-anchors-objc.md)
> * [Swift](create-locate-anchors-swift.md)
> * [Android Java](create-locate-anchors-java.md)
> * [C++/NDK](create-locate-anchors-cpp-ndk.md)
> * [C++/WinRT](create-locate-anchors-cpp-winrt.md)

Az Azure térbeli horgonyok lehetővé teszik, hogy a különböző eszközök között ossza meg a világbeli horgonyokat. Számos különböző fejlesztési környezetet támogat. Ebben a cikkben bemutatjuk, hogyan használhatja az Azure térbeli horgonyok SDK-t a C++/WinRT:

- Az Azure térbeli horgonyok munkamenetének megfelelő beállítása és kezelése.
- Hozzon létre és állítson be tulajdonságokat a helyi horgonyokon.
- Töltse fel őket a felhőbe.
- Felhőbeli térbeli horgonyok megkeresése és törlése.

## <a name="prerequisites"></a>Előfeltételek

Az útmutató elvégzéséhez győződjön meg arról, hogy rendelkezik a következővel:

- Olvassa el az [Azure térbeli horgonyok áttekintése című témakört](../overview.md).
- Az [5 perces rövid](../index.yml)útmutatók egyikét fejezte be.
- Alapszintű ismeretek a C++ és a <a href="/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt" target="_blank">Windows-futtatókörnyezet API</a>-khoz.

[!INCLUDE [Start](../../../includes/spatial-anchors-create-locate-anchors-start.md)]

További információ a [CloudSpatialAnchorSession](/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession) osztályról.

```cpp
    CloudSpatialAnchorSession m_cloudSession{ nullptr };

    m_cloudSession = CloudSpatialAnchorSession();
```

[!INCLUDE [Account Keys](../../../includes/spatial-anchors-create-locate-anchors-account-keys.md)]

További információ a [SessionConfiguration](/cpp/api/spatial-anchors/winrt/sessionconfiguration) osztályról.

```cpp
    auto configuration = m_cloudSession.Configuration();
    configuration.AccountKey(LR"(MyAccountKey)");
```

[!INCLUDE [Access Tokens](../../../includes/spatial-anchors-create-locate-anchors-access-tokens.md)]

```cpp
    auto configuration = m_cloudSession.Configuration();
    configuration.AccessToken(LR"(MyAccessToken)");
```

[!INCLUDE [Access Tokens Event](../../../includes/spatial-anchors-create-locate-anchors-access-tokens-event.md)]

További információ a [TokenRequiredDelegate](/cpp/api/spatial-anchors/winrt/tokenrequireddelegate) delegált szolgáltatásról.

```cpp
    m_accessTokenRequiredToken = m_cloudSession.TokenRequired(winrt::auto_revoke, [](auto&&, auto&& args) {
        args.AccessToken(LR"(MyAccessToken)");
    });
```

[!INCLUDE [Asynchronous Tokens](../../../includes/spatial-anchors-create-locate-anchors-asynchronous-tokens.md)]

```cpp
    m_accessTokenRequiredToken = m_cloudSession.TokenRequired(winrt::auto_revoke, [this](auto&&, auto&& args) {
        auto deferral = args.GetDeferral();
        MyGetTokenAsync([&deferral, &args](winrt::hstring const& myToken) {
            if (!myToken.empty()) args.AccessToken(myToken);
            deferral.Complete();
        });
    });
```

[!INCLUDE [Azure AD Tokens](../../../includes/spatial-anchors-create-locate-anchors-aad-tokens.md)]

```cpp
    auto configuration = m_cloudSession.Configuration();
    configuration.AuthenticationToken(LR"(MyAuthenticationToken)");
```

[!INCLUDE [Azure AD Tokens Event](../../../includes/spatial-anchors-create-locate-anchors-aad-tokens-event.md)]

```cpp
    m_accessTokenRequiredToken = m_cloudSession.TokenRequired(winrt::auto_revoke, [](auto&&, auto&& args) {
        args.AuthenticationToken(LR"(MyAuthenticationToken)");
    });
```

[!INCLUDE [Asynchronous Tokens](../../../includes/spatial-anchors-create-locate-anchors-asynchronous-tokens.md)]

```cpp
    m_accessTokenRequiredToken = m_cloudSession.TokenRequired(winrt::auto_revoke, [this](auto&&, auto&& args) {
        auto deferral = args.GetDeferral();
        MyGetTokenAsync([&deferral, &args](winrt::hstring const& myToken) {
            if (!myToken.empty()) args.AuthenticationToken(myToken);
            deferral.Complete();
        });
    });
```

[!INCLUDE [Setup](../../../includes/spatial-anchors-create-locate-anchors-setup-non-ios.md)]

További információ az [indítási](/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession#start) módszerről.

```cpp
    m_cloudSession.Start();
```

[!INCLUDE [Frames](../../../includes/spatial-anchors-create-locate-anchors-frames.md)]

További információ a [ProcessFrame](/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession) metódusról.

```cpp
    m_cloudSession->ProcessFrame(ar_frame_);
```

[!INCLUDE [Feedback](../../../includes/spatial-anchors-create-locate-anchors-feedback.md)]

További információ a [SessionUpdatedDelegate](/cpp/api/spatial-anchors/winrt/sessionupdateddelegate) delegált szolgáltatásról.

```cpp
    m_sessionUpdatedToken = m_cloudSession.SessionUpdated(winrt::auto_revoke, [this](auto&&, auto&& args)
    {
        auto status = args.Status();
        if (status.UserFeedback() == SessionUserFeedback::None) return;
        m_feedback = LR"(Feedback: )" + FeedbackToString(status.UserFeedback()) + LR"( -)" +
            LR"( Recommend Create=)" + FormatPercent(status.RecommendedForCreateProgress() * 100.f);
    });
```

[!INCLUDE [Creating](../../../includes/spatial-anchors-create-locate-anchors-creating.md)]

További információ a [CloudSpatialAnchor](/cpp/api/spatial-anchors/winrt/cloudspatialanchor) osztályról.

```cpp
    // Initialization
    SpatialStationaryFrameOfReference m_stationaryReferenceFrame = nullptr;
    HolographicDisplay defaultHolographicDisplay = HolographicDisplay::GetDefault();
    SpatialLocator spatialLocator = defaultHolographicDisplay.SpatialLocator();
    m_stationaryReferenceFrame = spatialLocator.CreateStationaryFrameOfReferenceAtCurrentLocation();

    // Create a local anchor, perhaps by positioning a SpatialAnchor a few meters in front of the user
    SpatialAnchor localAnchor{ nullptr };
    PerceptionTimestamp timestamp = PerceptionTimestampHelper::FromHistoricalTargetTime(DateTime::clock::now());
    SpatialCoordinateSystem currentCoordinateSystem = m_attachedReferenceFrame.GetStationaryCoordinateSystemAtTimestamp(timestamp);
    SpatialPointerPose pose = SpatialPointerPose::TryGetAtTimestamp(currentCoordinateSystem, timestamp);

    // Get the gaze direction relative to the given coordinate system.
    const float3 headPosition = pose.Head().Position();
    const float3 headDirection = pose.Head().ForwardDirection();

    // The anchor is positioned two meter(s) along the user's gaze direction.
    constexpr float distanceFromUser = 2.0f; // meters
    const float3 gazeAtTwoMeters = headPosition + (distanceFromUser * headDirection);

    localAnchor = SpatialAnchor::TryCreateRelativeTo(currentCoordinateSystem, gazeAtTwoMeters);

    // If the user is placing some application content in their environment,
    // you might show content at this anchor for a while, then save when
    // the user confirms placement.
    CloudSpatialAnchor cloudAnchor = CloudSpatialAnchor();
    cloudAnchor.LocalAnchor(localAnchor);
    co_await m_cloudSession.CreateAnchorAsync(cloudAnchor);
    m_feedback = LR"(Created a cloud anchor with ID=)" + cloudAnchor.Identifier();
```

[!INCLUDE [Session Status](../../../includes/spatial-anchors-create-locate-anchors-session-status.md)]

További információ a [GetSessionStatusAsync](/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession#getsessionstatusasync) metódusról.

```cpp
    SessionStatus status = co_await m_cloudSession.GetSessionStatusAsync();
    if (value.RecommendedForCreateProgress() < 1.0f) return;
    // Issue the creation request ...
```

[!INCLUDE [Setting Properties](../../../includes/spatial-anchors-create-locate-anchors-setting-properties.md)]

További információ a [AppProperties](/cpp/api/spatial-anchors/winrt/cloudspatialanchor#appproperties) metódusról.

```cpp
    CloudSpatialAnchor cloudAnchor = CloudSpatialAnchor();
    cloudAnchor.LocalAnchor(localAnchor);
    auto properties = m_cloudAnchor.AppProperties();
    properties.Insert(LR"(model-type)", LR"(frame)");
    properties.Insert(LR"(label)", LR"(my latest picture)");
    co_await m_cloudSession.CreateAnchorAsync(cloudAnchor);
```

[!INCLUDE [Update Anchor Properties](../../../includes/spatial-anchors-create-locate-anchors-updating-properties.md)]

További információ a [UpdateAnchorPropertiesAsync](/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession#updateanchorpropertiesasync) metódusról.

```cpp
    CloudSpatialAnchor anchor = /* locate your anchor */;
    anchor.AppProperties().Insert(LR"(last-user-access)", LR"(just now)");
    co_await m_cloudSession.UpdateAnchorPropertiesAsync(anchor);
```

[!INCLUDE [Getting Properties](../../../includes/spatial-anchors-create-locate-anchors-getting-properties.md)]

További információ a [GetAnchorPropertiesAsync](/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession#getanchorpropertiesasync) metódusról.

```cpp
    CloudSpatialAnchor anchor = co_await m_cloudSession.GetAnchorPropertiesAsync(LR"(anchorId)");
    if (anchor != nullptr)
    {
        anchor.AppProperties().Insert(LR"(last-user-access)", LR"(just now)");
        co_await m_cloudSession.UpdateAnchorPropertiesAsync(anchor);
    }
```

[!INCLUDE [Expiration](../../../includes/spatial-anchors-create-locate-anchors-expiration.md)]

További információ a [lejárati](/cpp/api/spatial-anchors/winrt/cloudspatialanchor#expiration) módszerről.

```cpp
    const int64_t oneWeekFromNowInHours = 7 * 24;
    const DateTime oneWeekFromNow = DateTime::clock::now() + std::chrono::hours(oneWeekFromNowInHours);
    cloudAnchor.Expiration(oneWeekFromNow);
```

[!INCLUDE [Locate](../../../includes/spatial-anchors-create-locate-anchors-locating.md)]

További információ a [CreateWatcher](/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession#createwatcher) metódusról.

```cpp
    AnchorLocateCriteria criteria = AnchorLocateCriteria();
    criteria.Identifiers({ LR"(id1)", LR"(id2)", LR"(id3)" });
    auto cloudSpatialAnchorWatcher = m_cloudSession.CreateWatcher(criteria);
```

[!INCLUDE [Locate Events](../../../includes/spatial-anchors-create-locate-anchors-locating-events.md)]

További információ a [AnchorLocatedDelegate](/cpp/api/spatial-anchors/winrt/anchorlocateddelegate) delegált szolgáltatásról.

```cpp
    m_anchorLocatedToken = m_cloudSession.AnchorLocated(winrt::auto_revoke, [this](auto&&, auto&& args)
    {
        switch (args.Status())
        {
            case LocateAnchorStatus::Located:
            {
                CloudSpatialAnchor foundAnchor = args.Anchor();
            }
                break;
            case LocateAnchorStatus::AlreadyTracked:
                // This anchor has already been reported and is being tracked
                break;
            case LocateAnchorStatus::NotLocatedAnchorDoesNotExist:
                // The anchor was deleted or never existed in the first place
                // Drop it, or show UI to ask user to anchor the content anew
                break;
            case LocateAnchorStatus::NotLocated:
                // The anchor hasn't been found given the location data
                // The user might in the wrong location, or maybe more data will help
                // Show UI to tell user to keep looking around
                break;
        }
    });
```

[!INCLUDE [Deleting](../../../includes/spatial-anchors-create-locate-anchors-deleting.md)]

További információ a [DeleteAnchorAsync](/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession#deleteanchorasync) metódusról.

```cpp
    co_await m_cloudSession.DeleteAnchorAsync(cloudAnchor);
    // Perform any processing you may want when delete finishes
```

[!INCLUDE [Stopping](../../../includes/spatial-anchors-create-locate-anchors-stopping.md)]

További információ a [leállítási](/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession#stop) módszerről.

```cpp
    m_cloudSession.Stop();
```

[!INCLUDE [Resetting](../../../includes/spatial-anchors-create-locate-anchors-resetting.md)]

További információ az [alaphelyzetbe állítási](/cpp/api/spatial-anchors/winrt/cloudspatialanchorsession#reset) módszerről.

```cpp
    m_cloudSession.Reset();
```

[!INCLUDE [Cleanup](../../../includes/spatial-anchors-create-locate-anchors-cleanup-others.md)]

```cpp
    m_cloudSession = nullptr;
```

[!INCLUDE [Next Steps](../../../includes/spatial-anchors-create-locate-anchors-next-steps.md)]