---
title: Függvényalkalmazás létrehozása egy App Service tervben – Azure CLI
description: Azure CLI-szkriptminta – Függvényalkalmazás létrehozása egy App Service-csomagban
ms.assetid: 0e221db6-ee2d-4e16-9bf6-a456cd05b6e7
ms.topic: sample
ms.date: 07/03/2018
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: ea368714ded56d77a10e24b853144bdf1dd42dfe
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/20/2021
ms.locfileid: "107786314"
---
# <a name="create-a-function-app-in-an-app-service-plan"></a>Függvényalkalmazás létrehozása egy App Service-csomagban

Ez az Azure Functions-példaszkript egy függvényalkalmazást hoz létre, amely az Ön függvényeinek tárolójaként szolgál. A létrehozott függvényalkalmazás egy dedikált App Service-csomagot használ, ami azt jelenti, hogy a kiszolgáló-erőforrások mindig működnek.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Ehhez az oktatóanyaghoz az Azure CLI 2.0-s vagy újabb verziójára lesz szükség. Ha a Azure Cloud Shell, a legújabb verzió már telepítve van. 

## <a name="sample-script"></a>Példaszkript

Ez a szkript egy Azure-függvényalkalmazást hoz létre egy dedikált [App Service-csomag](../dedicated-plan.md) használatával.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/create-function-app-app-service-plan/create-function-app-app-service-plan.sh "Create an Azure Function on an App Service plan")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Szkript ismertetése

A táblázatban lévő összes parancs a hozzá tartozó dokumentációra hivatkozik. Ez a szkript a következő parancsokat használja:

| Parancs | Jegyzetek |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | Létrehoz egy erőforráscsoportot, amely az összes erőforrást tárolja. |
| [az storage account create](/cli/azure/storage/account#az_storage_account_create) | Létrehoz egy Azure Storage-fiókot. |
| [az functionapp plan create](/cli/azure/functionapp/plan#az_functionapp_plan_create) | Prémium szintű csomagokat hoz létre. |
| [az functionapp create](/cli/azure/functionapp#az_functionapp_create) | Létrehoz egy függvényalkalmazást az App Service-csomagban. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI-vel kapcsolatos további információért lásd az [Azure CLI dokumentációját](/cli/azure).

További Azure Functions CLI-példaszkripteket az [Azure Functions dokumentációjában](../functions-cli-samples.md) találhat.
