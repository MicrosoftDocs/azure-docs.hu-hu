---
title: MongoDB-alkalmazás csatlakoztatása az Azure Cosmos DB-hez
description: Megtudhatja, hogyan csatlakoztathat egy MongoDB alkalmazást a Azure Cosmos DBhoz a kapcsolati sztring lekérésével a Azure Portal
author: timsander1
ms.author: tisande
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 03/02/2021
ms.reviewer: sngun
adobe-target: true
adobe-target-activity: DocsExp-A/B-384740-MongoDB-2.8.2021
adobe-target-experience: Experience B
adobe-target-content: ./connect-mongodb-account-experimental
ms.openlocfilehash: b3e10931307914f1471b8a6fbffd38953ee4717b
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/29/2021
ms.locfileid: "101659477"
---
# <a name="connect-a-mongodb-application-to-azure-cosmos-db"></a>MongoDB-alkalmazás csatlakoztatása az Azure Cosmos DB-hez
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

Megtudhatja, hogyan csatlakoztathatja a MongoDB-alkalmazását egy Azure Cosmos DB-adatbázishoz MongoDB kapcsolati sztring használatával. Ezt követően az Azure Cosmos-adatbázist használhatja a MongoDB-alkalmazása adattáraként.

Ez az oktatóanyag két módszert ismertet a kapcsolati sztringgel kapcsolatos információk lekéréséhez:

- [A](#get-the-mongodb-connection-string-by-using-the-quick-start)gyors üzembe helyezési módszer .net-, Node.js-, MongoDB-rendszerhéj-, Java-és Python-illesztőprogramokkal való használatra
- [Az egyéni kapcsolatok karakterláncának módszere](#get-the-mongodb-connection-string-to-customize), a más illesztőprogramokkal való használathoz

## <a name="prerequisites"></a>Előfeltételek

- Egy Azure-fiók. Ha nem rendelkezik Azure-fiókkal, hozzon létre most egy [ingyenes Azure-fiókot](https://azure.microsoft.com/free/) .
- Egy Cosmos-fiók. Útmutatásért lásd: [Webalkalmazás létrehozása a MongoDB és a .net SDK-hoz készült Azure Cosmos db API-](create-mongodb-dotnet.md)val.

## <a name="get-the-mongodb-connection-string-by-using-the-quick-start"></a>A MongoDB-kapcsolatok karakterláncának beolvasása a gyors üzembe helyezés használatával

1. Egy böngészőben jelentkezzen be a [Azure Portalba](https://portal.azure.com).
2. A **Azure Cosmos db** panelen válassza ki az API-t.
3. A fiók panel bal oldali paneljén kattintson a **gyors üzembe helyezés** elemre.
4. Válassza ki a platformot (**.net**, **Node.js**, **MongoDB Shell**, **Java**, **Python**). Ha nem látja a felsorolt illesztőprogramot vagy eszközt, ne aggódjon – a további kapcsolatok kódrészleteit folyamatosan dokumentáljuk. Kérjük, írja le alább, hogy mit szeretne látni. Ha szeretné megismerni a saját kapcsolatok kiépítésének módját, olvassa el [a fiók kapcsolódási karakterláncával kapcsolatos információkat](#get-the-mongodb-connection-string-to-customize).
5. Másolja és illessze be a kódrészletet a MongoDB alkalmazásba.

    :::image type="content" source="./media/connect-mongodb-account/QuickStartBlade.png" alt-text="Gyors üzembe helyezés panel":::

## <a name="get-the-mongodb-connection-string-to-customize"></a>A MongoDB-kapcsolatok karakterláncának beolvasása a testreszabáshoz

1. Egy böngészőben jelentkezzen be a [Azure Portalba](https://portal.azure.com).
2. A **Azure Cosmos db** panelen válassza ki az API-t.
3. A fiók panel bal oldali ablaktábláján kattintson a **kapcsolatok karakterlánc** elemre.
4. Megnyílik a **kapcsolatok karakterlánca** panel. Minden olyan információt tartalmaz, amely a fiókhoz való kapcsolódáshoz szükséges a MongoDB illesztőprogramjának használatával, beleértve az előépített kapcsolati karakterláncot is.

   :::image type="content" source="./media/connect-mongodb-account/ConnectionStringBlade.png" alt-text="Kapcsolati sztring panel" lightbox= "./media/connect-mongodb-account/ConnectionStringBlade.png" :::

## <a name="connection-string-requirements"></a>A kapcsolatok karakterláncának követelményei

> [!Important]
> Azure Cosmos DB szigorú biztonsági követelményekkel és szabványokkal rendelkezik. Azure Cosmos DB fiókoknak hitelesítésre és biztonságos kommunikációra van szükségük *TLS protokollon* keresztül.

A Azure Cosmos DB támogatja a szabványos MongoDB-kapcsolati karakterlánc URI-formátumát, néhány konkrét követelménysel: a Azure Cosmos DB fiókoknak hitelesítésre és biztonságos kommunikációra van szükség a TLS protokollon keresztül. Tehát a kapcsolatok karakterláncának formátuma a következőket teszi:

`mongodb://username:password@host:port/[database]?ssl=true`

A karakterlánc értékei a korábban látható **kapcsolatok karakterlánc** paneljén érhetők el:

* Username (kötelező): Cosmos-fiók neve.
* Password (kötelező): Cosmos-fiók jelszava.
* Gazdagép (kötelező): a Cosmos-fiók teljes tartományneve.
* Port (kötelező): 10255.
* Adatbázis (nem kötelező): a-adatbázis, amelyet a kapcsolatok használ. Ha nincs megadva adatbázis, az alapértelmezett adatbázis a "test".
* SSL = True (kötelező)

Tegyük fel például, hogy a fiók megjelenik a **kapcsolatok karakterlánca** panelen. Érvényes a kapcsolatok karakterlánca:

`mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@contoso123.documents.azure.com:10255/mydatabase?ssl=true`

## <a name="driver-requirements"></a>Illesztőprogram-követelmények

Az 3,4-es vagy annál nagyobb átviteli protokollt támogató illesztőprogramok támogatják Azure Cosmos DB API-t a MongoDB számára.

Az ügyfél-illesztőprogramoknak támogatnia kell a szolgáltatásnév-azonosító (SNI) TLS-bővítményt és/vagy a appName-kapcsolatok karakterláncának beállítását. Ha a `appName` paraméter meg van megadva, akkor azt a Azure Portal a kapcsolatok karakterláncának értéke tartalmazza.

## <a name="next-steps"></a>Következő lépések

- Ismerje meg, hogyan [használhatja a Studio 3T](mongodb-mongochef.md) Azure Cosmos db API-ját a MongoDB.
- Ismerje meg, hogyan [használhatja a Robo 3T](mongodb-robomongo.md) -t a Azure Cosmos db API-MongoDB.
- Ismerkedjen meg a MongoDB- [mintákkal](mongodb-samples.md) Azure Cosmos db API-val a MongoDB.
