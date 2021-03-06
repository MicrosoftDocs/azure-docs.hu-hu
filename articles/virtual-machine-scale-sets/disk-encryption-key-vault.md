---
title: Key Vault létrehozása és konfigurálása az Azure Disk Encryptionhöz
description: Ez a cikk a kulcstartók létrehozásához és konfigurálásához nyújt lépéseket a Azure Disk Encryption
author: ju-shim
ms.author: jushiman
ms.topic: tutorial
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 10/10/2019
ms.reviewer: mimckitt
ms.custom: mimckitt, devx-track-azurecli
ms.openlocfilehash: c9706176e537ec926a5335b54d667081773d76b1
ms.sourcegitcommit: 260a2541e5e0e7327a445e1ee1be3ad20122b37e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/21/2021
ms.locfileid: "107817358"
---
# <a name="create-and-configure-a-key-vault-for-azure-disk-encryption"></a>Kulcstartó létrehozása és konfigurálása Azure Disk Encryption

Azure Disk Encryption a Azure Key Vault a lemeztitkosítási kulcsok és titkos kulcsok vezérlésére és kezelésére.  A kulcstartókról további információt a Biztonsági [funkciókkal](../key-vault/general/overview.md) Azure Key Vault és Azure Key Vault [kapcsolatos Azure Key Vault tartalmaz.](../key-vault/general/security-features.md)

A kulcstartók létrehozása és konfigurálása a Azure Disk Encryption három lépésből áll:

1. Erőforráscsoport létrehozása, ha szükséges.
2. Kulcstartó létrehozása. 
3. Kulcstartó speciális hozzáférési szabályzatának beállítása.

Ezeket a lépéseket az alábbi rövid útmutatók mutatják be:

Szükség esetén létrehozhat vagy importálhat kulcstitkosítási kulcsot (KEK).

## <a name="install-tools-and-connect-to-azure"></a>Eszközök telepítése és csatlakozás az Azure-hoz

A cikkben található lépések az [Azure CLI,](/cli/azure/)a [Azure PowerShell Az](/powershell/azure/)modul vagy a [Azure Portal.](https://portal.azure.com)

### <a name="connect-to-your-azure-account"></a>Csatlakozás az Azure-fiókhoz

Az Azure CLI vagy a Azure PowerShell előtt csatlakoznia kell az Azure-előfizetéséhez. Ezt az [Azure CLI-val](/cli/azure/authenticate-azure-cli)való bejelentkezéssel, a [Azure PowerShell-val](/powershell/azure/authenticate-azureps)való bejelentkezéssel, vagy a hitelesítő adatoknak a Azure Portal kell megadnia.

```azurecli-interactive
az login
```

```azurepowershell-interactive
Connect-AzAccount
```

[!INCLUDE [disk-encryption-key-vault](../../includes/disk-encryption-key-vault.md)]
 
## <a name="next-steps"></a>Következő lépések

- [Azure Disk Encryption áttekintés](disk-encryption-overview.md)
- [Virtuálisgép-méretezési készletek titkosítása az Azure CLI használatával](disk-encryption-cli.md)
- [Virtuálisgép-méretezési készletek titkosítása a Azure PowerShell](disk-encryption-powershell.md)
