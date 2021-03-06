---
title: Hangsegéd-alkalmazás konfigurálása az Azure IoT Hub használatával
description: Hangsegéd-alkalmazás konfigurálása az Azure IoT Hub használatával
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/15/2021
ms.custom: template-how-to
ms.openlocfilehash: b22ef4ee0a8b5978bb2ec1c02fadf368815f3014
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102095782"
---
# <a name="configure-voice-assistant-application-using-azure-iot-hub"></a>Hangsegéd-alkalmazás konfigurálása az Azure IoT Hub használatával

Ez a cikk bemutatja, hogyan konfigurálhatja a hangsegéd alkalmazást a IoT Hub használatával. Egy lépésenkénti oktatóanyag, amely végigvezeti a hangsegédek bemutató sablon használatával történő létrehozásának folyamatán, lásd: [kód nélküli hangsegéd létrehozása az Azure Percept Studióval és az Azure Percept audio](./tutorial-no-code-speech.md)szolgáltatással.

## <a name="update-your-voice-assistant-configuration"></a>A hangsegéd konfigurációjának frissítése

1. Nyissa meg a [Azure Portalt](https://portal.azure.com) , és írja be a **IoT Hubt** a keresősávba. Kattintson az ikonra az IoT Hub lap megnyitásához.

1. A IoT Hub lapon válassza ki azt a IoT Hub, amelyhez az eszközt kiépítték.

1. A bal oldali navigációs menü **automatikus eszközkezelés** területén válassza a **IoT Edge** lehetőséget a IoT hubhoz csatlakoztatott összes eszköz megtekintéséhez.

1. Válassza ki azt az eszközt, amelyre a hangsegéd alkalmazást telepítette.

1. Kattintson a **modulok beállítása** elemre.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/set-modules.png" alt-text="Képernyőkép az eszköz oldaláról, a beállított modulok kiemelve.":::

1. Győződjön meg arról, hogy a következő bejegyzés szerepel a **Container Registry hitelesítő adatok** szakaszban. Szükség esetén adja meg a hitelesítő adatokat.

    |Name|Cím|Felhasználónév|Jelszó|
    |----|-------|--------|--------|
    |azureedgedevices|azureedgedevices.azurecr.io|devkitprivatepreviewpull|

1. A **IoT Edge-modulok** szakaszban válassza a **azureearspeechclientmodule** lehetőséget.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/modules.png" alt-text="Képernyőfelvétel: az eszközön található összes IoT Edge-modul listáját jeleníti meg.":::

1. Kattintson a **modul beállításai** fülre. Ellenőrizze a következő konfigurációt:

    Rendszerkép URI-ja|Újraindítási szabályzat|Kívánt állapot
    ---------|--------------|--------------
    mcr.microsoft.com/azureedgedevices/azureearspeechclientmodule:preload-devkit|mindig|fut

    Ha a beállítások nem egyeznek, szerkessze őket, és kattintson a **frissítés** gombra.

1. Kattintson a **környezeti változók** fülre. Ellenőrizze, hogy nincsenek-e definiálva környezeti változók.

1. Kattintson a **modul Twin beállítások** fülre. Frissítse a **speechConfigs** szakaszt a következőképpen:

    ```
    "speechConfigs": {
        "appId": "<Application id for custom command project>",
        "key": "<Speech Resource key for custom command project>",
        "region": "<Region for the speech service>",
        "keywordModelUrl": "https://aedsamples.blob.core.windows.net/speech/keyword-tables/computer.table",
        "keyword": "computer"
    }
    ```

    > [!NOTE]
    > A fent használt kulcsszó egy alapértelmezett nyilvánosan elérhető kulcsszó. Ha saját maga szeretné használni, hozzáadhatja saját egyéni kulcsszavait egy létrehozott Table-fájl blob Storage-tárolóba való feltöltésével. A blob Storage-t névtelen tároló-hozzáférés vagy névtelen blob-hozzáférés használatára kell konfigurálni.

## <a name="how-to-find-out-appid-key-and-region"></a>A appId, a kulcs és a régió megkeresése

A **appID**, a **kulcs** és a **régió** megkereséséhez nyissa meg a [Speech studiót](https://speech.microsoft.com/):

1. Jelentkezzen be, és válassza ki a megfelelő beszédfelismerési erőforrást.
1. A **Speech Studio** kezdőlapján kattintson az **egyéni parancsok** lehetőségre a **hangsegédek** területen.
1. Válassza ki a célként megadott projektet.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/project.png" alt-text="Képernyőkép a Project (projekt) oldalról a Speech Studióban.":::

1. Kattintson a **Beállítások** elemre a bal oldali menüsorban.
1. A **appID** és a **kulcs** az **általános** beállítások lapon található.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/general-settings.png" alt-text="Képernyőkép a beszédfelismerési projekt általános beállításairól.":::

1. A **régió** megkereséséhez nyissa meg a **Luis Resources** fület a beállítások között. A **szerzői erőforrás** kiválasztása a régió adatait fogja tartalmazni.

    :::image type="content" source="./media/manage-voice-assistant-using-iot-hub/luis-resources.png" alt-text="Képernyőkép a Speech Project LUIS erőforrásairól.":::

1. A **speechConfigs** adatainak megadása után kattintson a **frissítés** gombra.

1. Kattintson a **modulok beállítása** lap tetején található **útvonalak** fülre. Győződjön meg arról, hogy van egy útvonala a következő értékkel:

    ```
    FROM /messages/modules/azureearspeechclientmodule/outputs/* INTO $upstream
    ```

    Ha nem létezik, adja hozzá az útvonalat.

1. Kattintson a **Felülvizsgálat + létrehozás** elemre.

1. Kattintson a **Létrehozás** lehetőségre.


## <a name="next-steps"></a>Következő lépések

A hangsegéd konfigurációjának frissítése után térjen vissza az Azure Percept Studióban található bemutatóhoz, és lépjen kapcsolatba az alkalmazással.

