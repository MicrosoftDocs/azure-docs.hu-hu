---
title: PowerShell-szkript az Azure Cosmos MongoDB API-adatbázis és-gyűjtemény létrehozásához az autoscale használatával
description: Azure PowerShell szkript – Azure Cosmos MongoDB API-adatbázis és-gyűjtemény létrehozása az autoscale használatával
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: sample
ms.date: 07/30/2020
ms.author: mjbrown
ms.openlocfilehash: 64f4a4ce0e5ef35a0ba8dc314bbe6fb4f1d1030b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/29/2021
ms.locfileid: "98675438"
---
# <a name="create-a-database-and-collection-with-autoscale-for-azure-cosmos-db---mongodb-api"></a>Adatbázis és gyűjtemény létrehozása az Azure Cosmos DB-MongoDB API-val való autoskálázással
[!INCLUDE[appliesto-mongodb-api](../../../includes/appliesto-mongodb-api.md)]

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

Ehhez a mintához Azure PowerShell az 5.4.0 vagy újabb verzió szükséges. `Get-Module -ListAvailable Az`A Futtatás gombra kattintva megtekintheti, hogy mely verziók vannak telepítve.
Ha telepítenie kell a-t, olvassa el a [Azure PowerShell modul telepítése](/powershell/azure/install-az-ps)című témakört.

Futtassa a [AzAccount-](/powershell/module/az.accounts/connect-azaccount) t az Azure-ba való bejelentkezéshez.

## <a name="sample-script"></a>Példaszkript

[!code-powershell[main](../../../../../powershell_scripts/cosmosdb/mongodb/ps-mongodb-autoscale.ps1 "Create a database and collection with autoscale for MongoDB API")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása

A példaszkript futtatása után a következő paranccsal távolítható el az erőforráscsoport és az összes ahhoz kapcsolódó erőforrás.

```powershell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Szkript ismertetése

A szkript a következő parancsokat használja. A táblázatban lévő összes parancs a hozzá tartozó dokumentációra hivatkozik.

| Parancs | Jegyzetek |
|---|---|
|**Azure Cosmos DB**| |
| [Új – AzCosmosDBAccount](/powershell/module/az.cosmosdb/new-azcosmosdbaccount) | Létrehoz egy Cosmos DB fiókot. |
| [Új – AzCosmosDBMongoDBDatabase](/powershell/module/az.cosmosdb/new-azcosmosdbmongodbdatabase) | Létrehoz egy MongoDB API-adatbázist. |
| [Új – AzCosmosDBMongoDBIndex](/powershell/module/az.cosmosdb/new-azcosmosdbmongodbindex) | Létrehoz egy MongoDB API-indexet. |
| [Új – AzCosmosDBMongoDBCollection](/powershell/module/az.cosmosdb/new-azcosmosdbmongodbcollection) | Létrehoz egy MongoDB API-gyűjteményt. |
|**Azure-erőforráscsoportok**| |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Töröl egy erőforráscsoportot az összes beágyazott erőforrással együtt. |
|||

## <a name="next-steps"></a>Következő lépések

Az Azure PowerShellről további tudnivalókért tekintse meg az [Azure PowerShell dokumentációt](/powershell/).
