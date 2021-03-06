---
title: Az Azure Digital Twins CLI használata
titleSuffix: Azure Digital Twins
description: Ismerje meg, hogyan kezdheti el az első lépéseket, és hogyan használhatja az Azure digitális Twins parancssori felületét.
author: baanders
ms.author: baanders
ms.date: 05/25/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 5037450d401153811899b8d769ca92af7ce4068e
ms.sourcegitcommit: 5f482220a6d994c33c7920f4e4d67d2a450f7f08
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/08/2021
ms.locfileid: "107103776"
---
# <a name="use-the-azure-digital-twins-cli"></a>Az Azure Digital Twins CLI használata

A Azure Portal Azure Digital Twins-példányának kezelése mellett az Azure Digital Twins egy olyan parancsot is tartalmaz **az [Azure CLI](/cli/azure/what-is-azure-cli) -hez** , amely a szolgáltatással kapcsolatos legtöbb főbb művelet végrehajtásához használható, beleértve a következőket:
* Azure digitális Twins-példány kezelése
* Modellek kezelése
* Digitális ikrek kezelése
* Kettős kapcsolatok kezelése
* Végpontok konfigurálása
* [Útvonalak](concepts-route-events.md) kezelése
* [Biztonság](concepts-security.md) konfigurálása az Azure szerepköralapú hozzáférés-vezérlés (Azure RBAC) használatával

A beállításhalmaz neve az **DT**, amely az [Azure CLI-hez készült Azure IoT-bővítmény](https://github.com/Azure/azure-iot-cli-extension)részét képezi. A parancsok és azok használatának teljes listáját a következő parancs dokumentációjában tekintheti meg `az iot` : az [ *DT* Command Reference](/cli/azure/dt).

## <a name="uses-deploy-and-validate"></a>Felhasználás (üzembe helyezés és ellenőrzés)

A példányok általános kezelése mellett a CLI is hasznos eszköz az üzembe helyezéshez és az érvényesítéshez.
* A vezérlési sík parancsaival az új példányok üzembe helyezése ismételhető vagy automatizált lehet.
* Az adatsík parancsai segítségével gyorsan ellenőrizheti a példányban lévő értékeket, és ellenőrizheti, hogy a műveletek a várt módon fejeződött-e be.

## <a name="get-the-command-set"></a>A parancs beolvasása

Az Azure Digital Twins-parancsok az Azure [CLI (Azure-IoT) Azure IoT-bővítményének](https://github.com/Azure/azure-iot-cli-extension)részét képezik, ezért a következő lépésekkel ellenőrizheti, hogy rendelkezik-e a legújabb `azure-iot` bővítménnyel az az **DT** paranccsal.

### <a name="cli-version-requirements"></a>A CLI verziójának követelményei

Ha az Azure CLI-t a PowerShell-lel használja, a kiterjesztési csomaghoz az Azure CLI-verziójának **2.3.1** -es vagy újabb verziójára van szükség.

Az Azure CLI verziószámát a CLI-paranccsal tekintheti meg:
```azurecli
az --version
```

Az Azure CLI újabb verzióra való telepítésével vagy frissítésével kapcsolatos utasításokért lásd: [*Az Azure CLI telepítése*](/cli/azure/install-azure-cli).

### <a name="get-the-extension"></a>Bővítmény beszerzése

Az Azure CLI automatikusan kérni fogja, hogy telepítse a bővítményt egy olyan parancs első használatára, amelyhez szükség van.

Azt is megteheti, hogy a következő paranccsal bármikor telepítheti a bővítményt (vagy frissítheti, ha kiderül, hogy már rendelkezik egy régebbi verzióval). A parancs a [Azure Cloud Shell](../cloud-shell/overview.md) vagy egy [helyi Azure CLI](/cli/azure/install-azure-cli)-ben is futtatható.

```azurecli-interactive
az extension add --upgrade -n azure-iot
```

## <a name="next-steps"></a>Következő lépések

Ismerkedjen meg a parancssori felülettel és a teljes körű parancsaival a dokumentációs dokumentációban:
* [*az DT* Command Reference](/cli/azure/dt)