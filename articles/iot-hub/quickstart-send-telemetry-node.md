---
title: Rychlý start k odesílání telemetrických dat do služby Azure IoT Hub (Node.js) | Microsoft Docs
description: V tomto rychlém startu spustíte dvě ukázkové aplikace Node.js, které odesílají simulovaná telemetrická data do centra IoT a čtou z centra IoT telemetrická data pro účely zpracování v cloudu.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: node
ms.topic: quickstart
ms.custom: mvc
ms.date: 06/19/2018
ms.author: dobett
ms.openlocfilehash: dc255a36e2347aac204f7bd32fe3e9cf25d54b19
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "42022900"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-read-the-telemetry-from-the-hub-with-a-back-end-application-nodejs"></a>Rychlý start: Odesílání telemetrických dat ze zařízení do centra IoT a čtení telemetrických dat z centra pomocí back-endové aplikace (Node.js)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub je služba Azure, která umožňuje ingestovat velké objemy telemetrických dat ze zařízení IoT do cloudu pro účely uložení nebo zpracování. V tomto rychlém startu odešlete telemetrická data z aplikace simulovaného zařízení prostřednictvím služby IoT Hub do back-endové aplikace za účelem zpracování.

V tomto rychlém startu se používají dvě předem napsané aplikace Node.js, jedna k odesílání telemetrických dat a jedna ke čtení telemetrických dat z centra. Před spuštěním těchto dvou aplikací vytvoříte centrum IoT a zaregistrujete v tomto centru zařízení.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

Dvě ukázkové aplikace, které spustíte v tomto rychlém startu, jsou napsány pomocí Node.js. Na počítači používaném pro vývoj potřebujete mít Node.js v4.x.x nebo novější.

Node.js pro různé platformy si můžete stáhnout z webu [nodejs.org](https://nodejs.org).

Aktuální verzi Node.js na počítači používaném pro vývoj můžete ověřit pomocí následujícího příkazu:

```cmd/sh
node --version
```

Stáhněte si ukázkový projekt Node.js z https://github.com/Azure-Samples/azure-iot-samples-node/archive/master.zip a extrahujte archiv ZIP.

## <a name="create-an-iot-hub"></a>Vytvoření centra IoT

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]

## <a name="register-a-device"></a>Registrování zařízení

Zařízení musí být zaregistrované ve vašem centru IoT, aby se mohlo připojit. V tomto rychlém startu pomocí Azure CLI zaregistrujete simulované zařízení.

1. Přidejte rozšíření rozhraní příkazového řádku IoT Hub a vytvořte identitu zařízení. `{YourIoTHubName}` nahraďte názvem, který jste vybrali pro centrum IoT:

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id MyNodeDevice
    ```

    Pokud si zvolíte jiný název zařízení, změňte ho také v ukázkových aplikacích, než je spustíte.

1. Spuštěním následujícího příkazu získejte _připojovací řetězec zařízení_ pro zařízení, které jste právě zaregistrovali:

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id MyNodeDevice --output table
    ```

    Poznamenejte si připojovací řetězec zařízení, který vypadá nějak takto: `Hostname=...=`. Tuto hodnotu použijete později v tomto rychlém startu.

1. Potřebujete také _připojovací řetězec služby_, který back-endové aplikaci umožní připojení k vašemu centru IoT a načtení zpráv. Následující příkaz načte připojovací řetězec služby pro vaše centrum IoT:

    ```azurecli-interactive
    az iot hub show-connection-string --name {YourIoTHubName} --output table
    ```

    Poznamenejte si připojovací řetězec služby, který vypadá nějak takto: `Hostname=...=`. Tuto hodnotu použijete později v tomto rychlém startu. Připojovací řetězec služby se liší od připojovacího řetězce zařízení.

## <a name="send-simulated-telemetry"></a>Odesílání simulovaných telemetrických dat

Aplikace simulovaného zařízení se připojí ke koncovému bodu vašeho centra IoT pro konkrétní zařízení a odesílá simulovaná telemetrická data o teplotě a vlhkosti vzduchu.

1. V okně terminálu přejděte do kořenové složky ukázkového projektu Node.js. Pak přejděte do složky **iot-hub\Quickstarts\simulated-device**.

1. V libovolném textovém editoru otevřete soubor **SimulatedDevice.js**.

    Hodnotu proměnné `connectionString` nahraďte připojovacím řetězcem zařízení, který jste si předtím poznamenali. Změny pak uložte do souboru **SimulatedDevice.js**.

1. V okně terminálu spusťte následující příkazy pro instalaci požadovaných knihoven a spuštění aplikace simulovaného zařízení:

    ```cmd/sh
    npm install
    node SimulatedDevice.js
    ```

    Následující snímek obrazovky ukazuje výstup, zatímco aplikace simulovaného zařízení odesílá telemetrická data do vašeho centra IoT:

    ![Spuštění simulovaného zařízení](media/quickstart-send-telemetry-node/SimulatedDevice.png)

## <a name="read-the-telemetry-from-your-hub"></a>Čtení telemetrických dat z centra

Back-endová aplikace se připojí ke koncovému bodu **Events** na straně služby ve vašem centru IoT. Aplikace přijímá zprávy typu zařízení-cloud odeslané ze simulovaného zařízení. Back-endová aplikace služby IoT Hub se obvykle spouští v cloudu, aby mohla přijímat a zpracovávat zprávy typu zařízení-cloud.

1. V jiném okně terminálu přejděte do kořenové složky ukázkového projektu Node.js. Potom přejděte do složky **read-d2c-messages**.

1. V libovolném textovém editoru otevřete soubor **iot-hub\Quickstarts\ReadDeviceToCloudMessages.js**.

    Hodnotu proměnné `connectionString` nahraďte připojovacím řetězcem služby, který jste si předtím poznamenali. Změny potom uložte do souboru **ReadDeviceToCloudMessages.js**.

1. V okně terminálu spusťte následující příkazy pro instalaci požadovaných knihoven a spuštění back-endové aplikace:

    ```cmd/sh
    npm install
    node ReadDeviceToCloudMessages.js
    ```

    Následující snímek obrazovky ukazuje výstup, zatímco back-endová aplikace přijímá telemetrická data odeslaná simulovaným zařízením do centra:

    ![Spuštění back-endové aplikace](media/quickstart-send-telemetry-node/ReadDeviceToCloud.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste nastavili centrum IoT, zaregistrovali zařízení, odeslali simulovaná telemetrická data do centra pomocí aplikace Node.js a přečetli telemetrická data z centra pomocí jednoduché back-endové aplikace.

Informace o tom, jak řídit simulované zařízení z back-endové aplikace, najdete v dalším rychlém startu.

> [!div class="nextstepaction"]
> [Rychlý start: Řízení zařízení připojeného k centru IoT](quickstart-control-device-node.md)
