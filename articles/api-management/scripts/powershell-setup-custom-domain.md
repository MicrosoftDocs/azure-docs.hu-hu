---
title: Azure PowerShell-példaszkript – Egyéni tartomány beállítása | Microsoft Docs
description: Megtudhatja, hogyan állíthat be egyéni tartományt az API Management szolgáltatás proxy- vagy portálvégpontján. Tekintse meg a mintak szkripteket, és tekintse meg a további elérhető erőforrásokat.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.topic: sample
ms.date: 12/14/2017
ms.author: apimpm
ms.custom: mvc, devx-track-azurepowershell
ms.openlocfilehash: ce7ac9a27dd2708e3a32758c6e86011e493ea33a
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2021
ms.locfileid: "107817088"
---
# <a name="set-up-custom-domain"></a>Egyéni tartomány beállítása

Ez a példaszkript egy egyéni tartományt állít be az API Management szolgáltatás proxy- és portálvégpontján.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha a PowerShell helyi telepítését és használatát választja, az oktatóanyaghoz az Azure PowerShell 1.0-s vagy újabb verziójára lesz szükség. A verzió azonosításához futtassa a következőt: `Get-Module -ListAvailable Az`. Ha frissíteni szeretne, olvassa el [az Azure PowerShell-modul telepítését](/powershell/azure/install-Az-ps) ismertető cikket. Ha helyileg futtatja a PowerShellt, akkor emellett a `Connect-AzAccount` futtatásával kapcsolatot kell teremtenie az Azure-ral.

## <a name="sample-script"></a>Példaszkript

[!code-powershell[main](../../../powershell_scripts/api-management/setup-custom-domain/setup_custom_domain.ps1 "Set up custom domain")]

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha már nincs rá szükség, a [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) paranccsal eltávolíthatja az erőforráscsoportot és az összes kapcsolódó erőforrást.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

[!INCLUDE [api-management-custom-domain](../../../includes/api-management-custom-domain.md)]

## <a name="next-steps"></a>Következő lépések

Az Azure PowerShell modullal kapcsolatos további információért lásd az [Azure PowerShell dokumentációját](/powershell/azure/).

Az Azure API Managementhez a [PowerShell-minták](../powershell-samples.md) között találhat további Azure PowerShell-mintákat.
