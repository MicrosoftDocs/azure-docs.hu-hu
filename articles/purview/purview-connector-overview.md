---
title: A hatáskörébe-összekötő áttekintése
description: Ez a cikk a hatáskörébe tartozó különböző adattárakat és funkciókat ismerteti
author: chandrakavya
ms.author: kchandra
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 11/13/2020
ms.openlocfilehash: 08b22af8743082bab1d547205e51917cb9d92a11
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/29/2021
ms.locfileid: "101695770"
---
# <a name="supported-data-stores"></a>Támogatott adattárak

A hatáskörébe a következő adattárakat támogatja. Az egyes adattárokra kattintva megtudhatja a támogatott funkciókat és a megfelelő konfigurációkat a részletek között.

## <a name="purview-data-sources"></a>A hatáskörébe tartozó adatforrások

|**Kategória**|  **Adattár**  |**Metaadatok kinyerése**|**Teljes vizsgálat**|**Növekményes vizsgálat**|**Hatókörön belüli vizsgálat**|**Osztályozás**|**Lineage**|
|---|---|---|---|---|---|---|---|
| Azure | [Azure Blob Storage](register-scan-azure-blob-storage-source.md)| Igen| Igen| Igen| Igen| Igen| Yes|
||[Azure Cosmos DB](register-scan-azure-cosmos-database.md)|Igen| Igen| Igen| Igen| Igen| Yes|
||[Azure Data Explorer](register-scan-azure-data-explorer.md)|Igen| Igen| Igen| Igen| Igen| Yes|
||[1. generációs Azure Data Lake Storage](register-scan-adls-gen1.md)|Igen| Igen| Igen| Igen| Igen| Yes|
||[Azure Data Lake Storage Gen2](register-scan-adls-gen2.md)|Igen| Igen| Igen| Igen| Igen| Yes|
||[Azure SQL Database](register-scan-azure-sql-database.md)|Igen| Igen| Nem| Igen| Igen| Yes|
||[Felügyelt példány Azure SQL Database](register-scan-azure-sql-database-managed-instance.md)|Igen| Igen| Nem| Igen| Igen| Yes|
||[Azure Synapse Analytics (korábban SQL DW)](register-scan-azure-synapse-analytics.md)|Igen| Igen| Nem| Igen| Igen| Yes|
|Adatbázis|[Oracle DB](register-scan-oracle-source.md)|Igen| Igen| Nem| Nem| Nem| Igen|
||[SQL Server](register-scan-on-premises-sql-server.md)|Igen| Igen| Nem| Igen| Igen| Yes|
||[Teradata](register-scan-teradata-source.md)|Igen| Igen| Nem| Nem| Nem| Igen|
|Power BI|[Power BI](register-scan-power-bi-tenant.md)|Igen| Igen| Nem| Nem| Nem| Igen|
|Szolgáltatások és alkalmazások|[SAP ECC](register-scan-sapecc-source.md)|Igen| Igen| Nem| Igen| Igen| Yes|
||[SAP-S4HANA](register-scan-saps4hana-source.md)|Igen| Igen| Nem| Igen| Igen| Igen|

## <a name="next-steps"></a>Következő lépések

- [Azure Blob Storage-forrás regisztrálása és vizsgálata](register-scan-azure-blob-storage-source.md)