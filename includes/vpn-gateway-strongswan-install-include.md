---
title: fájl belefoglalása
description: fájl belefoglalása
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/14/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 089ad704a466590a9649107fef245a88d6a4d6e3
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/29/2021
ms.locfileid: "102244989"
---
Az alábbi konfigurációt használták a következő lépésekhez:

- Számítógép: Ubuntu Server 18,04
- Függőségek: alapú strongswan


A szükséges alapú strongswan-konfiguráció telepítéséhez használja az alábbi parancsokat:

```
sudo apt install strongswan
```

```
sudo apt install strongswan-pki
```

```
sudo apt install libstrongswan-extra-plugins
```

Az Azure parancssori felületének telepítéséhez használja az alábbi parancsot:

```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

[További tudnivalók az Azure CLI telepítéséről](/cli/azure/install-azure-cli-apt)