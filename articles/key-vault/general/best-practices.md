---
title: Ajánlott eljárások a Key Vault – Azure Key Vault | Microsoft Docs
description: Ismerje meg az ajánlott eljárásokat a Azure Key Vault, beleértve a hozzáférés szabályozását, a különálló kulcstartók használatát, a biztonságimentést, a naplózást és a helyreállítási lehetőségeket.
services: key-vault
author: msmbaldwin
tags: azure-key-vault
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 01/29/2021
ms.author: mbaldwin
ms.openlocfilehash: 7cfa2059cc03b96db39183cfa5056c9934a02290
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2021
ms.locfileid: "107814351"
---
# <a name="best-practices-to-use-key-vault"></a>Ajánlott eljárások a Key Vault

## <a name="use-separate-key-vaults"></a>Különálló kulcstartók használata

Azt javasoljuk, hogy környezetenként (fejlesztés, éles üzem előtti és éles környezet) alkalmazásonként használjon tárolót. Ez segít abban, hogy ne ossza meg a titkos okat a környezetek között, és csökkenti a biztonsági incidensek esetén jelentett fenyegetést.

## <a name="control-access-to-your-vault"></a>A tárolóhoz való hozzáférés szabályozása

Azure Key Vault egy felhőszolgáltatás, amely védi a titkosítási kulcsokat és titkos kódokat, például a tanúsítványokat, a kapcsolati sztringeket és a jelszavakat. Mivel ezek az adatok bizalmasak és üzleti szempontból kritikus fontosságúak, csak a jogosult alkalmazások és felhasználók engedélyezésével kell biztonságossá teszi a kulcstartókhoz való hozzáférést. Ez [a cikk](security-features.md) áttekintést nyújt a Key Vault hozzáférési modelljéről. Ismerteti a hitelesítést és az engedélyezést, valamint a kulcstartókhoz való hozzáférés biztonságossá való használatát.

A tárolóhoz való hozzáférés szabályozására vonatkozó javaslatok a következők:
1. Az előfizetéshez, erőforráscsoporthoz és Kulcstartókhoz (Azure RBAC) való hozzáférés zárolása
2. Hozzáférési szabályzatok létrehozása minden tárolóhoz
3. Hozzáférés megadása a legkisebb jogosultsági szintű jogosultsággal rendelkező rendszerbiztonsági tag használatával
4. A tűzfal és [a VNET-szolgáltatásvégpont bekapcsolás](overview-vnet-service-endpoints.md)

## <a name="backup"></a>Backup

Mindenképpen készítsen rendszeres biztonságimentéseket a tárolón belüli objektumok frissítéséhez/törléséhez/létrehozásához.

### <a name="azure-powershell-backup-commands"></a>Azure PowerShell biztonsági mentési parancsok használata

* [Biztonsági mentési tanúsítvány](/powershell/module/azurerm.keyvault/Backup-AzureKeyVaultCertificate)
* [Biztonsági mentési kulcs](/powershell/module/azurerm.keyvault/Backup-AzureKeyVaultKey)
* [Biztonsági másolat titkos másolata](/powershell/module/azurerm.keyvault/Backup-AzureKeyVaultSecret)

### <a name="azure-cli-backup-commands"></a>Az Azure CLI Biztonsági mentési parancsai

* [Biztonsági mentési tanúsítvány](/cli/azure/keyvault/certificate#az_keyvault_certificate_backup)
* [Biztonsági mentési kulcs](/cli/azure/keyvault/key#az_keyvault_key_backup)
* [Biztonsági másolat titkos másolata](/cli/azure/keyvault/secret#az_keyvault_secret_backup)


## <a name="turn-on-logging"></a>Naplózás bekapcsolása

[Kapcsolja be a tároló](logging.md) naplózását. Riasztásokat is beállít.

## <a name="turn-on-recovery-options"></a>Helyreállítási beállítások bekapcsolás

1. Kapcsolja be a [Soft Delete (Szoftveres törlés) gombra.](soft-delete-overview.md)
2. Kapcsolja be a végleges törlés elleni védelmet, ha a titkos kulcs/tároló kényszerített törlését még akkor is meg szeretné őrizni, ha a soft-delete be van kapcsolva.