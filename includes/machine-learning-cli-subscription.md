---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/26/2020
ms.author: larryfr
ms.openlocfilehash: 493c674fa161bf33436e560fdcbb9196410db931
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/30/2021
ms.locfileid: "102209468"
---
> [!TIP]
> A bejelentkezést követően megjelenik az Azure-fiókjához társított előfizetések listája. Az előfizetési információ az az `isDefault: true` aktuálisan aktivált Azure CLI-parancsokra vonatkozó előfizetése. Ennek az előfizetésnek azonosnak kell lennie, amely tartalmazza a Azure Machine Learning munkaterületet. Az előfizetés AZONOSÍTÓját a [Azure Portal](https://portal.azure.com) a munkaterület áttekintés lapjára kattintva érheti el. Az SDK-t is használhatja az előfizetés AZONOSÍTÓjának lekéréséhez a munkaterület-objektumból. Például: `Workspace.from_config().subscription_id`.
> 
> Egy másik előfizetés kiválasztásához használja a `az account set -s <subscription name or ID>` parancsot, és adja meg az előfizetés nevét vagy azonosítóját a váltáshoz. További információ az előfizetés kiválasztásáról: [több Azure-előfizetés használata](/cli/azure/manage-azure-subscriptions-azure-cli).