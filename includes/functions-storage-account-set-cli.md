---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/26/2019
ms.author: glenga
ms.openlocfilehash: 4ace70abe0112e0fe27d177c02bcb697746c92cc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/29/2021
ms.locfileid: "78262371"
---
### <a name="set-the-storage-account-connection"></a>A Storage-fiók kapcsolatainak beállítása

Nyissa meg a local.settings.jsfájlt, és másolja ki a értékét `AzureWebJobsStorage` , amely a Storage-fiókhoz tartozó kapcsolatok karakterlánca. Állítsa a `AZURE_STORAGE_CONNECTION_STRING` környezeti változót a kapcsolódási karakterláncra a bash parancs használatával:

```bash
AZURE_STORAGE_CONNECTION_STRING="<STORAGE_CONNECTION_STRING>"
```

Ha a kapcsolati karakterláncot a `AZURE_STORAGE_CONNECTION_STRING` környezeti változóban állítja be, akkor anélkül férhet hozzá a Storage-fiókhoz, hogy minden alkalommal hitelesítést kellene biztosítania.