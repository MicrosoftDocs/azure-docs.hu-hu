---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 04/04/2020
ms.author: mimart
ms.openlocfilehash: dd301cb3b18df5d3eb57ac38e9fb6432411d084b
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/31/2021
ms.locfileid: "106073093"
---
#### <a name="app-registrations"></a>[Alkalmazásregisztrációk](#tab/app-reg-ga/) 

1. Válassza a **Alkalmazásregisztrációk** lehetőséget, majd válassza ki azt a webalkalmazást, amelynek hozzá kell férnie az API-hoz. Például: *webapp1*.
1. A **kezelés** területen válassza az **API-engedélyek** lehetőséget.
1. A **konfigurált engedélyek** területen válassza **az engedély hozzáadása** elemet.
1. Válassza a **saját API** -k fület.
1. Válassza ki azt az API-t, amelyhez hozzáférést szeretne adni a webalkalmazásnak. Például: *webapi1*.
1. Az **engedély** területen bontsa ki a **bemutató** elemet, majd válassza ki a korábban definiált hatóköröket. Például: *bemutató. Read* és *demo. Write*.
1. Válassza az **engedélyek hozzáadása** lehetőséget.
1. Válassza a **rendszergazdai jóváhagyás megadása (a bérlő neve)** lehetőséget.
1. Ha a rendszer kéri, hogy válasszon ki egy fiókot, válassza ki a jelenleg bejelentkezett rendszergazdai fiókot, vagy jelentkezzen be egy olyan fiókkal a Azure AD B2C-bérlőben, amely legalább a *Cloud Application Administrator* szerepkörhöz van rendelve.
1. Válassza az **Igen** lehetőséget.
1. Válassza a **frissítés** lehetőséget, majd ellenőrizze, hogy a "engedélyezve..." mindkét hatókör **állapota** alatt jelenik meg.

#### <a name="applications-legacy"></a>[Alkalmazások (örökölt)](#tab/applications-legacy/)

1. Válassza az **alkalmazások (örökölt)** lehetőséget, majd válassza ki azt a webalkalmazást, amelynek hozzáférést szeretne biztosítani az API-hoz. Például: *webapp1*.
1. Válassza az **API-hozzáférés** lehetőséget, majd kattintson a **Hozzáadás** gombra.
1. Az **API kiválasztása** legördülő menüben válassza ki azt az API-t, amelyhez hozzáférést szeretne adni a webalkalmazásnak. Például: *webapi1*.
1. A **hatókörök kiválasztása** legördülő menüben válassza ki a korábban definiált hatóköröket. Például: *bemutató. Read* és *demo. Write*.
1. Válassza az **OK** lehetőséget.
