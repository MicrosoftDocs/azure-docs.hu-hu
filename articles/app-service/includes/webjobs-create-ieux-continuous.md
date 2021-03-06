---
author: ggailey777
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.topic: include
ms.date: 10/16/2018
ms.title: include
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 33dc766643355a5f5ebb6138e000595fd1bfe6fc
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/29/2021
ms.locfileid: "101745664"
---
## <a name="create-a-continuous-webjob"></a><a name="CreateContinuous"></a> Folyamatos Webjobs létrehozása

1. A [Azure Portal](https://portal.azure.com).
1. Lépjen a **app Service** <abbr title="Az alkalmazás-erőforrás lehet egy webalkalmazás, egy API-alkalmazás vagy egy mobil alkalmazás.">Alkalmazás-erőforrás</abbr>.
1. Válassza a **Webjobs** elemet.

   ![Webjobs-feladatok kiválasztása](../media/web-sites-create-web-jobs/select-webjobs.png)

1. A **webjobs** lapon válassza a **Hozzáadás** lehetőséget.

    ![Webjobs lap](../media/web-sites-create-web-jobs/wjblade.png)

1. Használja a táblázatban megadott Webjobs-beállítások **hozzáadása** beállítást.

   ![Képernyőfelvétel: a konfigurálni kívánt Webjobs-beállítások hozzáadása.](../media/web-sites-create-web-jobs/addwjcontinuous.png)

   | Beállítás      | Mintaérték   | 
   | ------------ | ----------------- | 
   | <abbr title="Egy App Service alkalmazáson belül egyedi név. Betűvel vagy számmal kell kezdődnie, és a és a nem tartalmazhat különleges `-` karaktereket `_` .">Name</abbr> | myContinuousWebJob | 
   | <abbr title=" A végrehajtható fájlt vagy parancsfájlt tartalmazó *. zip* fájl, valamint a program vagy a parancsfájl futtatásához szükséges összes támogató fájl.">Fájlfeltöltés</abbr> | ConsoleApp.zip |
   | <abbr title="A típusok között folyamatos, aktivált.">Típus</abbr> | Folyamatos | 
   | <abbr title="Csak a folyamatos webjobs-feladatok esetében érhető el. Meghatározza, hogy a program vagy a parancsfájl az összes példányon vagy csak egy példányon fusson-e. A több példányon való futtatás lehetősége nem vonatkozik az ingyenes vagy a közös díjszabási szintekre.">Méretezés</abbr> | Több példány | 

1. Kattintson az **OK** gombra.

    Az új Webjobs megjelenik a **webjobs** oldalon.

    ![Webjobs-feladatok listája](../media/web-sites-create-web-jobs/listallwebjobs.png)

1. Folyamatos Webjobs **leállításához vagy újraindításához** kattintson a jobb gombbal a webjobs a listában, majd kattintson a **Leállítás** vagy az **Indítás** parancsra.

   ![Folyamatos Webjobs leállítása](../media/web-sites-create-web-jobs/continuousstop.png)
