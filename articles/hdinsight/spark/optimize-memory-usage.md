---
title: Memóriahasználat optimalizálása Apache Sparkban – Azure HDInsight
description: Ismerje meg, hogyan optimalizálhatja az Azure HDInsight Apache Spark memóriahasználat használatát.
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/20/2020
ms.custom: contperf-fy21q1
ms.openlocfilehash: 4e23c5977b2492d2ea8a7a8cc050c77c512c3e16
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/30/2021
ms.locfileid: "104868375"
---
# <a name="memory-usage-optimization-for-apache-spark"></a>Memóriahasználat optimalizálása Apache Spark

Ez a cikk azt ismerteti, hogyan optimalizálható a Apache Spark-fürt memóriájának kezelése a legjobb teljesítmény érdekében az Azure HDInsight.

## <a name="overview"></a>Áttekintés

A Spark úgy működik, hogy a memóriába helyezi az adatforgalmat. Így a memória erőforrásainak kezelése kulcsfontosságú szempont a Spark-feladatok végrehajtásának optimalizálásához.  Több módszer is alkalmazható a fürt memóriájának hatékony használatára.

* A particionálási stratégiában a kisebb adatpartíciók és a fiókok mérete, típusai és eloszlása is előnyben részesített.
* Vegye figyelembe az újabb, hatékonyabb [`Kryo data serialization`](https://github.com/EsotericSoftware/kryo) , az alapértelmezett Java-szerializálás helyett.
* A FONALat inkább a Batch által elválasztva használja `spark-submit` .
* A Spark konfigurációs beállításainak figyelése és finomhangolása.

A következő képen látható a Spark-memória szerkezete és néhány kulcsfontosságú végrehajtói memória paraméter.

## <a name="spark-memory-considerations"></a>A Spark memória szempontjai

Ha Apache Hadoop FONALat használ, akkor a FONALak az egyes Spark-csomópontokon lévő összes tároló által használt memóriát vezérlik.  A következő ábrán a legfontosabb objektumok és azok kapcsolatai láthatók.

:::image type="content" source="./media/apache-spark-perf/apache-yarn-spark-memory.png" alt-text="A fonal Spark memóriájának kezelése" border="false":::

A "memórián kívüli" üzenetek megoldásához próbálkozzon a következővel:

* Tekintse át a DAG felügyeletének véletlenszerű működését. Csökkentse a leképezési oldali újrabontást, a partíció előtti (vagy bucketize) adatforrásokat, maximalizálja az egyetlen véletlenszerű sorrendet, és csökkentse a továbbított adatmennyiséget.
* Inkább a `ReduceByKey` rögzített memória korlátja `GroupByKey` , amely az összesítéseket, az ablakokat és más funkciókat tartalmaz, de Ann nem kötött memória korlátja van.
* A rendszer inkább a `TreeReduce` végrehajtók vagy a partíciók több munkáját hajtja végre a (z `Reduce` ) rendszeren, amely az illesztőprogramon végzett összes munkát végzi.
* Az alsó szintű RDD-objektumok helyett használjon DataFrames.
* Hozzon létre olyan ComplexTypes, amelyek műveleteket (például "Top N"), különböző összesítéseket vagy ablakkezelő műveleteket ágyaznak be.

További hibaelhárítási lépésekért lásd: [működése OutOfMemoryError-kivételek Apache Spark az Azure HDInsight](apache-spark-troubleshoot-outofmemory.md).

## <a name="next-steps"></a>Következő lépések

* [Az adatfeldolgozás optimalizálása Apache Spark](optimize-cluster-configuration.md)
* [Az adattároló optimalizálása Apache Spark](optimize-data-storage.md)
* [A fürt konfigurációjának optimalizálása Apache Spark](optimize-cluster-configuration.md)
