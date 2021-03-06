---
title: Azure Policy Azure biztonsági alapterve
description: A Azure Policy biztonsági alapterve az Azure biztonsági Teljesítménytesztben meghatározott biztonsági javaslatok megvalósítására szolgáló eljárási útmutatást és erőforrásokat biztosít.
author: msmbaldwin
ms.service: azure-policy
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 6e8bb4cf715c6cb8d0729399c1985376de18687b
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105561287"
---
# <a name="azure-security-baseline-for-azure-policy"></a>Azure Policy Azure biztonsági alapterve

Ez a biztonsági alapkonfiguráció az [Azure biztonsági teljesítménytesztre](../../../security/benchmarks/overview.md) vonatkozó útmutatást alkalmazza Azure Policyra. Az Azure Security Benchmark ajánlásokat ad arra nézve, hogy hogyan tehetők biztonságossá a felhőalapú megoldások az Azure-ban. A tartalom a **megfelelőségi tartományok** és az Azure biztonsági teljesítményteszt által meghatározott **biztonsági vezérlők** , valamint a Azure Policy vonatkozó kapcsolódó útmutatás szerint van csoportosítva. A Azure Policy nem alkalmazható **vezérlők** ki vannak zárva. Ha szeretné megtekinteni, hogyan Azure Policy teljes mértékben leképezni az Azure biztonsági Teljesítménytesztét, tekintse meg a [teljes Azure Policy biztonsági alapterv-leképezési fájlt](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

Az Azure Security teljesítményteszt-vezérlők beépített szabályzat-definíciók alapján történő leképezéséhez a beépített kezdeményezéssel tekintse meg a jogszabályoknak [való megfelelést: Azure biztonsági teljesítményteszt](../samples/azure-security-benchmark.md).

A Azure Policy a _felelősség_ helyett a _tulajdonos_ kifejezést használja. A _tulajdonjoggal_ kapcsolatos részletekért lásd: [Azure Policy szabályzat-definíciók](./definition-structure.md#type) és [közös felelősség a felhőben](../../../security/fundamentals/shared-responsibility.md).

## <a name="logging-and-monitoring"></a>Naplózás és monitorozás

*További információt az [Azure biztonsági teljesítményteszt: naplózás és figyelés](../../../security/benchmarks/security-control-logging-monitoring.md)című témakörben talál.*

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: az Azure-erőforrások naplózásának engedélyezése

**Útmutató**: a Azure Policy a tevékenység naplóit használja, amelyek automatikusan engedélyezve vannak az eseményforrás, a dátum, a felhasználó, az időbélyeg, a forráscím, a célhelyek és más hasznos elemek belefoglalásához.

- [Platform-naplók és-metrikák összegyűjtése Azure Monitor](../../../azure-monitor/essentials/diagnostic-settings.md)

- [A naplózás és a különböző naplózási típusok megismerése az Azure-ban](../../../azure-monitor/essentials/platform-logs-overview.md)

**Felelősség**: Ügyfél

**Azure Security Center figyelés**: nincs

## <a name="identity-and-access-control"></a>Identitás- és hozzáférés-vezérlés

*További információt az [Azure biztonsági teljesítményteszt: Identity and Access Control](../../../security/benchmarks/security-control-identity-access-control.md)című témakörben talál.*

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: dedikált rendszergazdai fiókok használata

**Útmutató**: szabványos üzemeltetési eljárások létrehozása a dedikált rendszergazdai fiókok használata körül. A rendszergazdai fiókok számának figyeléséhez használja a Azure Security Center identitás-és hozzáférés-kezelés lehetőséget. A [Azure Active Directory (Azure ad) Privileged Identity Management](../../../active-directory/privileged-identity-management/pim-configure.md) rendszerjogosultságú szerepkörök vagy [Azure Resource Manager](../../../azure-resource-manager/management/overview.md)használatával is engedélyezheti az igény szerinti és a elég hozzáférési megoldást.

**Felelősség**: Ügyfél

**Azure Security Center monitorozás**: az [Azure biztonsági teljesítményteszt](/azure/governance/policy/samples/azure-security-benchmark) a Security Center alapértelmezett házirend-kezdeményezése, és a [Security Center ajánlásainak](/azure/security-center/security-center-recommendations)alapja. A vezérlőhöz kapcsolódó Azure Policy-definíciók Security Center automatikusan engedélyezve vannak. Az ehhez a vezérlőhöz kapcsolódó riasztásokhoz szükség lehet egy [Azure Defender](/azure/security-center/azure-defender) -csomagra a kapcsolódó szolgáltatásokhoz.

**Azure Policy beépített definíciók – Microsoft. GuestConfiguration**:

[!INCLUDE [Resource Policy for Microsoft.GuestConfiguration 3.3](../../../../includes/policy/standards/asb/rp-controls/microsoft.guestconfiguration-3-3.md)]

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3,6: biztonságos, Azure által felügyelt munkaállomások használata felügyeleti feladatokhoz

**Útmutató**: az Azure-erőforrások bejelentkezéséhez és konfigurálásához konfigurált, többtényezős hitelesítéssel rendelkező emelt szintű hozzáférésű munkaállomások (mancsok) használata.

- [További tudnivalók a privilegizált hozzáférésű munkaállomásokról](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Többtényezős hitelesítés engedélyezése az Azure-ban](../../../active-directory/authentication/howto-mfa-getstarted.md)

**Felelősség**: Ügyfél

**Azure Security Center figyelés**: nincs

## <a name="data-protection"></a>Adatvédelem

*További információ: [Azure Security Benchmark: Adatvédelem](../../../security/benchmarks/security-control-data-protection.md).*

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4,6: az erőforrásokhoz való hozzáférés szabályozása az Azure RBAC 

**Útmutató**: az Azure szerepköralapú hozzáférés-vezérlés (Azure RBAC) használata a Azure Policyhoz való hozzáférés szabályozásához.

- [Azure RBAC engedélyek a Azure Policyban](../overview.md#azure-rbac-permissions-in-azure-policy)

- [Az Azure RBAC konfigurálása](../../../role-based-access-control/role-assignments-portal.md)

**Felelősség**: Ügyfél

**Azure Security Center figyelés**: nincs

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: a kritikus Azure-erőforrások változásainak naplózása és riasztása

**Útmutató**: a Azure monitor a tevékenység-naplók használatával riasztásokat hozhat létre, amikor a módosítások Azure Policyban lépnek életbe.

- [Riasztások létrehozása az Azure-tevékenységek naplózási eseményeihez](../../../azure-monitor/alerts/alerts-activity-log.md)

**Felelősség**: Ügyfél

**Azure Security Center figyelés**: nincs

## <a name="inventory-and-asset-management"></a>Leltár-és eszközfelügyelet

*További információt az [Azure biztonsági teljesítményteszt: leltár és eszközkezelés](../../../security/benchmarks/security-control-inventory-asset-management.md)című témakörben talál.*

### <a name="62-maintain-asset-metadata"></a>6,2: az eszköz metaadatainak fenntartása

**Útmutató**: címkéket alkalmazhat az Azure-erőforrásokra, így a metaadatok logikailag rendszerezve lesznek a besorolásban. A megfelelőségi és konzisztens címkézési szabályozáshoz használja a Azure Policy _módosítás_ hatása jelentést.

- [Oktatóanyag: házirendek létrehozása és kezelése](../tutorials/create-and-manage.md)

- [Oktatóanyag: a címke irányításának kezelése](../tutorials/govern-tags.md)

**Felelősség**: Ügyfél

**Azure Security Center figyelés**: nincs

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6,4: a jóváhagyott Azure-erőforrások leltárának meghatározása és karbantartása

**Útmutató**: hozzon létre egy leltárt a jóváhagyott szabályzat-definíciók és szabályzat-hozzárendelések alapján a szervezeti igényeknek megfelelően.

**Felelősség**: Ügyfél

**Azure Security Center figyelés**: nincs

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: a nem jóváhagyott Azure-erőforrások figyelése

**Útmutató**: a Azure Policy használatával korlátozásokat állíthat be az előfizetésekben létrehozható erőforrásokra vonatkozóan.

- [Az Azure Policy konfigurálása és kezelése](../tutorials/create-and-manage.md)

**Felelősség**: Ügyfél

**Azure Security Center figyelés**: nincs

## <a name="next-steps"></a>Következő lépések

- [Az Azure Security Benchmark v2 áttekintésének](../../../security/benchmarks/overview.md) megtekintése
- További tudnivalók az [Azure biztonsági alapterveiről](../../../security/benchmarks/security-baselines-overview.md)