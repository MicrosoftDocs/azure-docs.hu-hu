---
title: fájl belefoglalása
description: fájl belefoglalása
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 03/10/2021
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: 24a5c92164e0eace41224edfd2153c6142f7ea49
ms.sourcegitcommit: b28e9f4d34abcb6f5ccbf112206926d5434bd0da
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/09/2021
ms.locfileid: "107251287"
---
[!INCLUDE [Public Preview Notice](../../../includes/public-preview-include-chat.md)]

## <a name="prerequisites"></a>Előfeltételek
Az első lépések előtt ügyeljen a következőre:

- Aktív előfizetéssel rendelkező Azure-fiók létrehozása. Részletekért tekintse meg a [fiók ingyenes létrehozását](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)ismertető témakört. 
- Telepítse a [Xcode](https://developer.apple.com/xcode/) és a [CocoaPods](https://cocoapods.org/). A Xcode használatával iOS-alkalmazást hozhat létre a gyors útmutatóhoz, és CocoaPods a függőségek telepítéséhez.
- Hozzon létre egy Azure kommunikációs szolgáltatások erőforrást. Részletekért lásd: gyors útmutató [: kommunikációs szolgáltatások erőforrásainak létrehozása és kezelése](../../create-communication-resource.md). Ebben a rövid útmutatóban rögzítenie kell az erőforrás-végpontot.
- Hozzon létre két felhasználót az Azure kommunikációs szolgáltatásokban, és adjon ki egy [felhasználói hozzáférési jogkivonatot](../../access-tokens.md). Ügyeljen arra, hogy a hatókört állítsa be `chat` , és jegyezze fel a `token` karakterláncot és a `userId` karakterláncot is. Ebben a rövid útmutatóban egy kezdeti résztvevővel rendelkező szálat hoz létre, majd egy második résztvevőt ad hozzá a szálhoz.

## <a name="setting-up"></a>Beállítás

### <a name="create-a-new-ios-application"></a>Új iOS-alkalmazás létrehozása

Nyissa meg a Xcode, és válassza **az új Xcode-projekt létrehozása** lehetőséget. Ezután válassza az **iOS** lehetőséget a sablonhoz és az **alkalmazáshoz** .

A projekt neve mezőbe írja be a következőt: **ChatQuickstart**. Ezután válassza a **storyboard** lehetőséget az illesztőfelületként, **UIKit az alkalmazás-delegálást** , és a **Swift** nyelvet.

Válassza a **tovább** lehetőséget, majd válassza ki azt a könyvtárat, ahol létre szeretné hozni a projektet.

### <a name="install-the-libraries"></a>A kódtárak telepítése

A CocoaPods használatával telepítse a szükséges kommunikációs szolgáltatások függőségeit.

A parancssorban lépjen az iOS-projekt gyökérkönyvtárában `ChatQuickstart` . Hozzon létre egy Cocoapods a következő paranccsal: `pod init` .

Nyissa meg a Cocoapods, és adja hozzá a következő függőségeket a `ChatQuickstart` célhoz:

```
pod 'AzureCommunication', '~> 1.0.0-beta.11'
pod 'AzureCommunicationChat', '~> 1.0.0-beta.11'
```

Telepítse a függőségeket a következő paranccsal: `pod install` . Vegye figyelembe, hogy ez egy Xcode-munkaterületet is létrehoz.

A Futtatás után `pod install` nyissa meg újra a projektet a Xcode-ben az újonnan létrehozott lehetőség kiválasztásával `.xcworkspace` .

### <a name="set-up-the-placeholders"></a>A helyőrzők beállítása

Nyissa meg a munkaterületet a `ChatQuickstart.xcworkspace` Xcode, majd nyissa meg `ViewController.swift` .

Ebben a rövid útmutatóban hozzáadja a kódját a alkalmazáshoz `viewController` , és megtekinti a kimenetet a Xcode-konzolon. Ez a rövid útmutató nem foglalkozik a felhasználói felület iOS-ben való létrehozásával. 

A (z) felső részén `viewController.swift` importálja a `AzureCommunication` és a `AzureCommunicatonChat` kódtárakat:

```
import AzureCommunication
import AzureCommunicationChat
```

Másolja a következő kódot a `viewDidLoad()` metódusba `ViewController` :

```
override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.

        let semaphore = DispatchSemaphore(value: 0)
        DispatchQueue.global(qos: .background).async {
            do {
                // <CREATE A CHAT CLIENT>
                
                // <CREATE A CHAT THREAD>

                // <LIST ALL CHAT THREADS>

                // <GET A CHAT THREAD CLIENT>

                // <SEND A MESSAGE>

                // <SEND A READ RECEIPT >

                // <RECEIVE MESSAGES>

                // <ADD A USER>
                
                // <LIST USERS>
            } catch {
                print("Quickstart failed: \(error.localizedDescription)")
            }
        }
    }
```

Demonstrációs célokra egy szemafor használatával szinkronizáljuk a kódot. A következő lépésekben az Azure kommunikációs szolgáltatások csevegési könyvtárával cseréli le a helyőrzőket a mintakód használatával.


### <a name="create-a-chat-client"></a>Csevegési ügyfél létrehozása

Az `<CREATE A CHAT CLIENT>` megjegyzés helyére írja be az alábbi kódot:

```
let endpoint = "<ACS_RESOURCE_ENDPOINT>"
let credential =
try CommunicationTokenCredential(
    token: "<ACCESS_TOKEN>"
)
let options = AzureCommunicationChatClientOptions()

let chatClient = try ChatClient(
    endpoint: endpoint,
    credential: credential,
    withOptions: options
)
```

Cserélje le az helyére az `<ACS_RESOURCE_ENDPOINT>` Azure kommunikációs szolgáltatások erőforrásának végpontját. Cserélje le a `<ACCESS_TOKEN>` elemet egy érvényes kommunikációs szolgáltatás hozzáférési jogkivonatára.

Ez a rövid útmutató nem fedi le a csevegési alkalmazás jogkivonatait kezelő szolgáltatási szintet, de ajánlott. További információkért lásd a [csevegési koncepciók](../../../concepts/chat/concepts.md)"csevegési architektúra" című szakaszát.

A felhasználói hozzáférési jogkivonatokkal kapcsolatos további információkért lásd [: rövid útmutató: hozzáférési tokenek létrehozása és kezelése](../../access-tokens.md).

## <a name="object-model"></a>Objektummodell 

A következő osztályok és felületek a JavaScripthez készült Azure Communication Services csevegő SDK főbb funkcióit kezelik.

| Név                                   | Leírás                                                                                                                                                                           |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ChatClient` | Ez az osztály szükséges a csevegési funkciókhoz. Létrehozza azt az előfizetési adatokkal, és felhasználhatja a szálak létrehozására, lekérésére és törlésére. |
| `ChatThreadClient` | Ez az osztály szükséges a csevegési szál működéséhez. A példányt a használatával szerezheti be, `ChatClient` és használhatja az üzenetek küldéséhez, fogadásához, frissítéséhez és törléséhez. Azt is megteheti, hogy felveszi, eltávolítja és beolvassa a felhasználókat, begépelési értesítéseket és olvasási visszaigazolásokat küld, és előfizet a csevegési eseményekre. |

## <a name="start-a-chat-thread"></a>Csevegési szál elindítása

Most a használatával `ChatClient` létrehoz egy új szálat egy kezdeti felhasználóval.

Az `<CREATE A CHAT THREAD>` megjegyzés helyére írja be az alábbi kódot:

```
let request = CreateChatThreadRequest(
    topic: "Quickstart",
    participants: [
        ChatParticipant(
            id: CommunicationUserIdentifier("<USER_ID>"),
            displayName: "Jack"
        )
    ]
)

var threadId: String?
chatClient.create(thread: request) { result, _ in
    switch result {
    case let .success(result):
        threadId = result.chatThread?.id

    case .failure:
        fatalError("Failed to create thread.")
    }
    semaphore.signal()
}
semaphore.wait()
```

Cserélje le `<USER_ID>` egy érvényes kommunikációs szolgáltatások felhasználói azonosítóját.

Itt egy szemafort használ, hogy megvárja a befejezési kezelőt a folytatás előtt. A későbbi lépések során a a `threadId` kimenetet a befejezési kezelőnek visszaadott válasz alapján fogja használni.

## <a name="list-all-chat-threads"></a>Az összes csevegési szál listázása

Csevegési szál létrehozása után listázhatja az összes csevegési szálat a metódus meghívásával `listChatThreads` `ChatClient` . Az `<LIST ALL CHAT THREADS>` megjegyzés helyére írja be az alábbi kódot:

```
chatClient.listThreads { result, _ in
    switch result {
    case let .success(chatThreadItems):
        var iterator = chatThreadItems.syncIterator
            while let chatThreadItem = iterator.next() {
                print("Thread id: \(chatThreadItem.id)")
            }
    case .failure:
        print("Failed to list threads")
    }
    semaphore.signal()
}
semaphore.wait()
```

## <a name="get-a-chat-thread-client"></a>Csevegési szál ügyfelének beolvasása

Most, hogy létrehozott egy csevegési szálat, beszerezhet egy `ChatThreadClient` műveletet a szálon belül.

Az `<GET A CHAT THREAD CLIENT>` megjegyzés helyére írja be az alábbi kódot:

```
let chatThreadClient = try chatClient.createClient(forThread: threadId!)
```

## <a name="send-a-message-to-a-chat-thread"></a>Üzenet küldése csevegési szálnak

Az `<SEND A MESSAGE>` megjegyzés helyére írja be az alábbi kódot:

```
let message = SendChatMessageRequest(
    content: "Hello!",
    senderDisplayName: "Jack"
)

var messageId: String?

chatThreadClient.send(message: message) { result, _ in
    switch result {
    case let .success(result):
        print("Message sent, message id: \(result.id)")
        messageId = result.id
    case .failure:
        print("Failed to send message")
    }
    semaphore.signal()
}
semaphore.wait()
```

Először hozza létre a (z `SendChatMessageRequest` )-t, amely tartalmazza a tartalom és a küldő megjelenítendő nevét. Ez a kérelem a megosztási előzmények időpontját is tartalmazhatja, ha azt szeretné felvenni. A befejezési kezelőnek visszaadott válasz tartalmazza az elküldött üzenet AZONOSÍTÓját.


## <a name="send-a-read-receipt"></a>Olvasási visszaigazolás küldése

A metódus meghívásával olvasási visszaigazolást küldhet egy adott üzenethez `ChatThreadClients` `sendReadReceipt` . Az `<SEND A READ RECEIPT>` megjegyzés helyére írja be az alábbi kódot:

```
if let id = messageId {
    chatThreadClient.sendReadReceipt(forMessage: id) { result, _ in
        switch result {
        case .success:
            print("Read receipt sent")
        case .failure:
            print("Failed to send read receipt")
        }
        semaphore.signal()
    }
    semaphore.wait()
} else {
    print("Cannot send read receipt without a message id")
}
```

## <a name="receive-chat-messages-from-a-chat-thread"></a>Csevegési üzenetek fogadása csevegési szálból

A csevegési szálból fogadhat üzeneteket, ha meghívja a `listMessages()` metódust a alkalmazásból `ChatThreadClient` . Az üzenetek listázása rendszerüzeneteket és a felhasználó által küldött üzeneteket is tartalmazza. További információ az üzenetek típusairól: [üzenet típusai](https://docs.microsoft.com/azure/communication-services/concepts/chat/concepts#message-types)

Az `<RECEIVE MESSAGES>` megjegyzés helyére írja be az alábbi kódot:

```
chatThreadClient.listMessages { result, _ in
    switch result {
    case let .success(messages):
        var iterator = messages.syncIterator
        while let message = iterator.next() {
            print("Received message of type \(message.type)")
        }

    case .failure:
        print("Failed to receive messages")
    }
    semaphore.signal()
}
semaphore.wait()
```

## <a name="add-a-user-as-a-participant-to-the-chat-thread"></a>Felhasználó felvétele a csevegési szálba résztvevőként

Az `<ADD A USER>` megjegyzés helyére írja be az alábbi kódot:

```
let user = ChatParticipant(
    id: CommunicationUserIdentifier("<USER_ID>"),
    displayName: "Jane"
)

chatThreadClient.add(participants: [user]) { result, _ in
    switch result {
    case let .success(result):
        (result.invalidParticipants != nil) ? print("Added participant") : print("Error adding participant")
    case .failure:
        print("Failed to add the participant")
    }
    semaphore.signal()
}
semaphore.wait()
```

A helyére írja `<USER_ID>` be a hozzáadni kívánt felhasználó kommunikációs szolgáltatások felhasználói azonosítóját.

Ha a résztvevőt egy szálhoz adja hozzá, akkor a visszaadott válasz hibákat tartalmazhat. Ezek a hibák nem felelnek meg bizonyos résztvevők hozzáadásának.

## <a name="list-users-in-a-thread"></a>Egy szál felhasználóinak listázása

Az `<LIST USERS>` megjegyzést írja felül az alábbi kóddal:

```
chatThreadClient.listParticipants { result, _ in
    switch result {
    case let .success(participants):
        var iterator = participants.syncIterator
        while let participant = iterator.next() {
            let user = participant.id as! CommunicationUserIdentifier
            print("User with id: \(user.identifier)")
        }
    case .failure:
        print("Failed to list participants")
    }
    semaphore.signal()
}
semaphore.wait()
```

## <a name="run-the-code"></a>A kód futtatása

A Xcode nyomja meg a Futtatás gombot a projekt felépítéséhez és futtatásához. A konzolon megtekintheti a kód kimenetét és a ChatClient származó naplózó kimenetet.

