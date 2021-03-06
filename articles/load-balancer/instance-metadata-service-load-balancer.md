---
title: Terheléselosztó információinak beolvasása az Azure Instance Metadata Service használatával
titleSuffix: Azure Load Balancer
description: Ismerkedjen meg az Azure Instance Metadata Service használatának megismerésével a terheléselosztó adatainak lekéréséhez.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: allensu
ms.openlocfilehash: 0d7d08eb5e77e744fb6ce0abefc550bc79de4c8c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/29/2021
ms.locfileid: "100519082"
---
# <a name="retrieve-load-balancer-information-by-using-the-azure-instance-metadata-service"></a>Terheléselosztó információinak beolvasása az Azure Instance Metadata Service használatával

A IMDS (Azure Instance Metadata Service) információt nyújt a jelenleg futó virtuálisgép-példányokról. A szolgáltatás egy REST API, amely jól ismert, nem irányítható IP-címen (169.254.169.254) érhető el. 

Ha a virtuális gépet vagy virtuálisgép-készletet állít be egy Azure-standard Load Balancer mögött, a IMDS segítségével lekérheti a terheléselosztó és a példányok metaadatait.

A metaadatok a következő információkat tartalmazzák a virtuális gépekhez vagy a virtuálisgép-méretezési csoportokhoz:

* Standard SKU nyilvános IP-címe.
* A hálózati adapter minden magánhálózati IP-címéhez tartozó terheléselosztó bejövő szabályának konfigurációi.
* A hálózati adapter minden magánhálózati IP-címéhez tartozó terheléselosztó kimenő szabályának konfigurációi.

## <a name="access-the-load-balancer-metadata-using-the-imds"></a>A terheléselosztó metaadatainak elérése a IMDS használatával

További információ a terheléselosztó metaadatainak eléréséről: [Az Azure instance metadata Service használata a terheléselosztó adatainak eléréséhez](howto-load-balancer-imds.md).

## <a name="troubleshoot-common-error-codes"></a>Gyakori hibakódok – problémamegoldás

A gyakori hibakódokról és azok enyhítési módszereiről további információt a [gyakori hibakódok hibaelhárítása a IMDS használata esetén](troubleshoot-load-balancer-imds.md)című témakörben talál. 

## <a name="support"></a>Támogatás

Ha több kísérlet után nem tud metaadat-választ beolvasni, hozzon létre egy támogatási problémát a Azure Portal.

## <a name="next-steps"></a>Következő lépések
További információ az [Azure instance metadata Service](../virtual-machines/windows/instance-metadata-service.md)

[Standard Load Balancer üzembe helyezése](quickstart-load-balancer-standard-public-portal.md)

