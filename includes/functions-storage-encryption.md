---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: 0bc5977a581006dc760c0435f9d68371ced7e4cd
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/29/2021
ms.locfileid: "83648779"
---
Az Azure Storage minden olyan adattárolót titkosít, amely egy Storage-fiókban található. További információ: az [Azure Storage titkosítása inaktív adatokhoz](../articles/storage/common/storage-service-encryption.md).

Alapértelmezés szerint az adattitkosítás a Microsoft által kezelt kulcsokkal történik. A titkosítási kulcsok további szabályozásához megadhatja az ügyfél által felügyelt kulcsokat a blobok és a fájlok titkosításához. Ezeknek a kulcsoknak jelen kell lenniük a Azure Key Vaultban ahhoz, hogy a függvények hozzáférhessenek a Storage-fiókhoz. További információért lásd az [ügyfél által felügyelt kulcsok használatával történő titkosítást](../articles/azure-functions/configure-encrypt-at-rest-using-cmk.md)ismertető témakört.  