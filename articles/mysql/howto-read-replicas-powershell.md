---
title: Olvasási replikák kezelése – Azure PowerShell-Azure Database for MySQL
description: Ismerje meg, hogyan állíthat be és kezelhet olvasási replikákat Azure Database for MySQL a PowerShell használatával.
author: savjani
ms.author: pariks
ms.service: mysql
ms.topic: how-to
ms.date: 8/24/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: fe33730fc11bfc18b7d67471e1077fb9490385d4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/29/2021
ms.locfileid: "94541929"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mysql-using-powershell"></a>Olvasási replikák létrehozása és kezelése a Azure Database for MySQL a PowerShell használatával

Ebből a cikkből megtudhatja, hogyan hozhat létre és kezelhet olvasási replikákat a Azure Database for MySQL szolgáltatásban a PowerShell használatával. Az olvasási replikákkal kapcsolatos további tudnivalókért tekintse meg az [áttekintést](concepts-read-replicas.md).

## <a name="azure-powershell"></a>Azure PowerShell

Az olvasási replikákat a PowerShell használatával hozhatja létre és kezelheti.

## <a name="prerequisites"></a>Előfeltételek

A útmutató lépéseinek elvégzéséhez a következőkre lesz szüksége:

- Az az [PowerShell-modul](/powershell/azure/install-az-ps) helyileg vagy [Azure Cloud Shell](https://shell.azure.com/) telepítve a böngészőben
- Egy [Azure Database for MySQL-kiszolgáló](quickstart-create-mysql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> Az az. MySql PowerShell-modul előzetes verzióban érhető el, és a következő paranccsal külön kell telepítenie az az PowerShell-modulból: `Install-Module -Name Az.MySql -AllowPrerelease` .
> Amint az az. MySql PowerShell-modul általánosan elérhetővé válik, az a PowerShell modul kiadásainak része lesz, és natív módon elérhető a Azure Cloud Shellon belülről.

Ha a PowerShell helyi használatát választja, kapcsolódjon az Azure-fiókjához a [AzAccount](/powershell/module/az.accounts/Connect-AzAccount) parancsmag használatával.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

> [!IMPORTANT]
> Az olvasási replika funkció csak a általános célú vagy a memória optimalizált árképzési szintjein Azure Database for MySQL-kiszolgálókon érhető el. Győződjön meg arról, hogy a forráskiszolgáló ezen díjszabási szintek egyikében található.

### <a name="create-a-read-replica"></a>Olvasási replika létrehozása

> [!IMPORTANT]
> Ha olyan forráshoz hoz létre replikát, amely nem tartalmaz meglévő replikákat, a forrás először újraindul, hogy felkészüljön a replikálásra. Ezt vegye figyelembe, és hajtsa végre ezeket a műveleteket egy leállási időszakon belül.

A következő paranccsal hozhat létre olvasási replika-kiszolgálót:

```azurepowershell-interactive
Get-AzMySqlServer -Name mydemoserver -ResourceGroupName myresourcegroup |
  New-AzMySqlReplica -Name mydemoreplicaserver -ResourceGroupName myresourcegroup
```

A `New-AzMySqlReplica` parancshoz a következő paraméterek szükségesek:

| Beállítás | Példaérték | Leírás  |
| --- | --- | --- |
| ResourceGroupName |  myResourceGroup |  Az az erőforráscsoport, amelyben a replika-kiszolgáló létrejön.  |
| Name | mydemoreplicaserver | A létrehozott új replika-kiszolgáló neve. |

Egy több régióból származó olvasási replika létrehozásához használja a **Location** paramétert. Az alábbi példa egy replikát hoz létre az **USA nyugati** régiójában.

```azurepowershell-interactive
Get-AzMySqlServer -Name mrdemoserver -ResourceGroupName myresourcegroup |
  New-AzMySqlReplica -Name mydemoreplicaserver -ResourceGroupName myresourcegroup -Location westus
```

Ha többet szeretne megtudni arról, hogy mely régiókban hozhat létre replikát, látogasson el a [replika áttekintése című cikkben](concepts-read-replicas.md).

Alapértelmezés szerint a rendszer az olvasási replikákat ugyanazzal a kiszolgáló-konfigurációval hozza létre, mint a forrást, kivéve, ha meg van adva az **SKU** paraméter.

> [!NOTE]
> Azt javasoljuk, hogy a replika-kiszolgáló konfigurációját a forrásnál egyenlő vagy annál nagyobb értékekkel kell megőrizni, hogy a replika képes legyen lépést tartani a főkiszolgálóval.

### <a name="list-replicas-for-a-source-server"></a>Forráskiszolgáló replikáinak listázása

Egy adott forráskiszolgáló összes replikájának megtekintéséhez futtassa a következő parancsot:

```azurepowershell-interactive
Get-AzMySqlReplica -ResourceGroupName myresourcegroup -ServerName mydemoserver
```

A `Get-AzMySqlReplica` parancshoz a következő paraméterek szükségesek:

| Beállítás | Példaérték | Leírás  |
| --- | --- | --- |
| ResourceGroupName |  myResourceGroup |  Az az erőforráscsoport, amelybe a replika-kiszolgáló létre lesz hozva.  |
| ServerName | mydemoserver | A forráskiszolgáló neve vagy azonosítója. |

### <a name="delete-a-replica-server"></a>Replika-kiszolgáló törlése

Az olvasási replika kiszolgáló törlését a parancsmag futtatásával teheti meg `Remove-AzMySqlServer` .

```azurepowershell-interactive
Remove-AzMySqlServer -Name mydemoreplicaserver -ResourceGroupName myresourcegroup
```

### <a name="delete-a-source-server"></a>Forráskiszolgáló törlése

> [!IMPORTANT]
> A forráskiszolgáló törlése leállítja a replikálást az összes replikakiszolgálón, magát a forráskiszolgálót pedig törli. A replikakiszolgálókból különálló kiszolgálók lesznek, amelyek az olvasási és írási műveleteket egyaránt támogatják.

A forráskiszolgáló törléséhez futtathatja a `Remove-AzMySqlServer` parancsmagot.

```azurepowershell-interactive
Remove-AzMySqlServer -Name mydemoserver -ResourceGroupName myresourcegroup
```

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [Azure Database for MySQL kiszolgáló újraindítása a PowerShell-lel](howto-restart-server-powershell.md)
