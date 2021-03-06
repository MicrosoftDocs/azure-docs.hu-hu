---
title: Üzembe helyezés & HPA-metrikák a Container bepillantást tartalmazó szolgáltatással | Microsoft Docs
description: Ez a cikk azt ismerteti, hogy milyen üzembe helyezési & HPA (horizontális Pod autoscaleer) mérőszámok gyűjtése a Container bepillantást.
ms.topic: conceptual
ms.date: 08/09/2020
ms.openlocfilehash: c8bb100b756ea92d73e1c3a698f119b4f8157930
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/29/2021
ms.locfileid: "101717706"
---
# <a name="deployment--hpa-metrics-with-container-insights"></a>Üzembe helyezés & HPA-metrikák a Container bepillantást

Az ügynök verziójának *ciprod08072020* kezdődően a Container bepillantások – az integrált ügynök most az üzembe helyezések metrikáit gyűjti & HPAs.

## <a name="deployment-metrics"></a>Üzembe helyezési metrikák

A tároló-ellenőrzési szolgáltatás automatikusan elindítja az üzembe helyezések figyelését a következő metrikák 60 mp-es intervallumok szerinti gyűjtésével és a **InsightMetrics** táblában való tárolásával:

|Metrika neve |Metrikus dimenzió (címkék) |Leírás |
|------------|------------------------|------------|
|kube_deployment_status_replicas_ready |container.azm.ms/clusterId, container.azm.ms/clusterName, creationTime, üzembe helyezés, deploymentStrategy, k8sNamespace, spec_replicas, status_replicas_available, status_replicas_updated (Status. updatedReplicas) | A központi telepítés által megcélozható kész hüvelyek teljes száma (Status. readyReplicas). Az alábbiakban a metrika dimenziói láthatók. <ul> <li> központi telepítés – a központi telepítés neve </li> <li> k8sNamespace – Kubernetes névtér az üzemelő példányhoz </li> <li> deploymentStrategy – üzembe helyezési stratégia a hüvelyek újakkal való lecseréléséhez (spec. Strategy. Type)</li><li> creationTime – központi telepítés időbélyege </li> <li> spec_replicas – a kívánt hüvelyek (spec. replikák) száma </li> <li>status_replicas_available – az üzemelő példány által megcélozható rendelkezésre álló hüvelyek teljes száma (legalább minReadySeconds) (Status. availableReplicas)</li><li>status_replicas_updated – az üzemelő példány által megadott, nem lezárt hüvelyek teljes száma, amelyeken a kívánt sablon spec (Status. updatedReplicas) szerepel </li></ul>|

## <a name="hpa-metrics"></a>HPA-metrikák

A tároló-felismerések automatikusan elindítják a HPAs figyelését a következő metrikák 60 mp-es intervallumokban való összegyűjtésével és a **InsightMetrics** táblában való tárolásával:

|Metrika neve |Metrikus dimenzió (címkék) |Leírás |
|------------|------------------------|------------|
|kube_hpa_status_current_replicas |container.azm.ms/clusterId, container.azm.ms/clusterName, creationTime, hPa, k8sNamespace, lastScaleTime, spec_max_replicas, spec_min_replicas, status_desired_replicas, targetKind, targetName | Az ebben az autoskálázásban (Status. currentReplicas) kezelt hüvelyek replikáinak aktuális száma. Az alábbiakban a metrika dimenziói láthatók. <ul> <li> hPa – a HPA neve </li> <li> k8sNamespace – Kubernetes-névtér a HPA számára </li> <li> lastScaleTime – utolsó alkalommal, amikor a HPA a hüvelyek számát (Status. lastScaleTime) méretezi</li><li> creationTime – HPA létrehozás időbélyege </li> <li> spec_max_replicas – az autoskálázás által beállítható hüvelyek számának felső korlátja (spec. maxReplicas) </li> <li> spec_min_replicas – az automéretező által lekicsinyíthető replikák számának alsó határértéke (spec. minReplicas) </li><li>status_desired_replicas az ezen az autoskálázással kezelt hüvelyek replikáinak kívánt száma (Status. desiredReplicas)</li><li>targetKind – a HPA céljának típusa (spec. scaleTargetRef. Kind) </li><li>targetName – a HPA céljának neve (spec.scaleTargetRef.name) </li></ul>|

## <a name="deployment--hpa-charts"></a>Üzembe helyezés & HPA-diagramok 

A Container-adatok a táblázatban korábban felsorolt mérőszámok előre konfigurált diagramjait tartalmazzák, mint az összes fürthöz tartozó munkafüzet. A központi telepítések & HPA-munkafüzetek üzembe helyezését **& hPa** -t közvetlenül egy AK-fürtről, a bal oldali ablaktáblán található **munkafüzetek** kiválasztásával, valamint az elemzés **munkafüzetek megtekintése** legördülő listából.

## <a name="next-steps"></a>Következő lépések

- Tekintse át [az Kube mérőszámait a Kubernetes-ben](https://github.com/kubernetes/kube-state-metrics/tree/master/docs) , és ismerkedjen meg a Kube mérőszámokkal.