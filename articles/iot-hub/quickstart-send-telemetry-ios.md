---
title: Rychlý start k odesílání telemetrických dat do služby Azure IoT Hub | Microsoft Docs
description: V tomto rychlém startu spustíte ukázkovou aplikaci pro iOS, která odesílá simulovaná telemetrická data do centra IoT a čte z centra IoT telemetrická data pro účely zpracování v cloudu.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: quickstart
ms.custom: mvc
ms.date: 04/20/2018
ms.author: kgremban
ms.openlocfilehash: dbc1cc4a72d0346c92d506358c39a66a4d780b32
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38309741"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-ios"></a>Rychlý start: Odesílání telemetrických dat ze zařízení do centra IoT (iOS)

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub je služba Azure, která umožňuje ingestovat velké objemy telemetrických dat ze zařízení IoT do cloudu pro účely uložení nebo zpracování. V tomto článku budete do služby IoT Hub odesílat telemetrická data z aplikace simulovaného zařízení. Pak můžete data zobrazit v back-endové aplikaci. 

V tomto článku se k odesílání telemetrických dat používá předem napsaná aplikace Swift a ke čtení telemetrických dat ze služby IoT Hub se používá nástroj příkazového řádku. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="prerequisites"></a>Požadavky

- Stažení vzorového kódu z [ukázek Azure](https://github.com/Azure-Samples/azure-iot-samples-ios/archive/master.zip). 
- Nejnovější verze [XCode](https://developer.apple.com/xcode/) používající nejnovější verzi sady SDK pro iOS. Tento rychlý start byl testován s XCode 9.3 a iOS 11.3.
- Nejnovější verze [CocoaPods](https://guides.cocoapods.org/using/getting-started.html).
- Nástroj příkazového řádku iothub-explorer, který čte telemetrická data ze služby IoT Hub. Pokud ho chcete nainstalovat, nainstalujte nejprve [Node.js](https://nodejs.org) verze 4.x.x nebo novější a pak spusťte následující příkaz: 

   ```sh
   sudo npm install -g iothub-explorer
   ```

## <a name="create-an-iot-hub"></a>Vytvoření centra IoT

[!INCLUDE [iot-hub-quickstarts-create-hub](../../includes/iot-hub-quickstarts-create-hub.md)]


## <a name="register-a-device"></a>Registrování zařízení

Zařízení musí být zaregistrované ve vašem centru IoT, aby se mohlo připojit. V tomto rychlém startu pomocí Azure CLI zaregistrujete simulované zařízení.

1. Přidejte rozšíření rozhraní příkazového řádku IoT Hub a vytvořte identitu zařízení. Nahraďte `{YourIoTHubName}` názvem vašeho centra IoT:

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   az iot hub device-identity create --hub-name {YourIoTHubName} --device-id myiOSdevice
   ```

    Pokud si zvolíte jiný název zařízení, změňte ho také v ukázkových aplikacích, než je spustíte.

1. Spuštěním následujícího příkazu získejte _připojovací řetězec zařízení_ pro zařízení, které jste právě zaregistrovali:

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id myiOSdevice --output table
   ```

   Poznamenejte si připojovací řetězec zařízení, který vypadá nějak takto: `Hostname=...=`. Tuto hodnotu použijete později v tomto článku.

1. Potřebujete také _připojovací řetězec služby_, který back-endovým aplikacím umožní připojení k vašemu centru IoT a načtení zpráv ve směru zařízení-cloud. Následující příkaz načte připojovací řetězec služby pro vaše centrum IoT:

   ```azurecli-interactive
   az iot hub show-connection-string --hub-name {YourIoTHubName} --output table
   ```

   Poznamenejte si připojovací řetězec služby, který vypadá nějak takto: `Hostname=...=`. Tuto hodnotu použijete později v tomto článku.

## <a name="send-simulated-telemetry"></a>Odesílání simulovaných telemetrických dat

Ukázková aplikace se spouští na zařízení iOS, které se připojuje ke koncovému bodu vašeho centra IoT pro konkrétní zařízení a odesílá simulovaná telemetrická data o teplotě a vlhkosti vzduchu. 

### <a name="install-cocoapods"></a>Instalace CocoaPods

CocoaPods spravují závislosti pro projekty iOS využívající knihovny třetích stran.

V okně terminálu přejděte do složky Azure-IoT-Samples-iOS, kterou jste stáhli v rámci požadavků. Pak přejděte do ukázkového projektu:

```sh
cd quickstart/sample-device
```

Ujistěte se, že je XCode zavřené, a pak spuštěním následujícího příkazu nainstalujte CocoaPods deklarované v souboru **podfile**:

```sh
pod install
```

Kromě instalace požadovaných podů pro váš projekt příkaz k instalaci vytvořil také soubor pracovního prostoru XCode, který je předem nakonfigurovaný tak, aby používal pody pro závislosti. 

### <a name="run-the-sample-application"></a>Spuštění ukázkové aplikace 

1. Otevřete ukázkový pracovní prostor v XCode.

   ```sh
   open "MQTT Client Sample.xcworkspace"
   ```

2. Rozbalte projekt **MQTT Client Sample** a pak rozbalte složku se stejným názvem.  
3. Otevřete soubor **ViewController.swift** pro úpravy v XCode. 
4. Vyhledejte proměnnou **connectionString** a aktualizujte její hodnotu s použitím připojovacího řetězce zařízení, který jste si poznamenali dříve.
5. Uložte provedené změny. 
6. Spusťte projekt v emulátoru zařízení pomocí tlačítka **Build and run** (Sestavit a spustit) nebo kombinace kláves **command + r**. 

   ![Spuštění projektu](media/quickstart-send-telemetry-ios/run-sample.png)

7. Jakmile se otevře emulátor, vyberte v ukázkové aplikaci **Start** (Spustit).

Následující snímek obrazovky ukazuje příklad výstupu, zatímco aplikace odesílá simulovaná telemetrická dat do vašeho centra IoT:

   ![Spuštění simulovaného zařízení](media/quickstart-send-telemetry-ios/view-d2c.png)

## <a name="read-the-telemetry-from-your-hub"></a>Čtení telemetrických dat z centra

Ukázková aplikace, kterou jste spustili v emulátoru XCode, ukazuje data o zprávách odeslaných ze zařízení. Můžete zobrazit také přijatá data procházející přes vaše centrum IoT. Nástroj příkazového řádku `iothub-explorer` se připojí ke koncovému bodu **Events** (Události) na straně služby ve vaší službě IoT Hub. 

Otevřete nové okno terminálu. Spusťte následující příkaz, ve kterém nahraďte {your hub service connection string} připojovacím řetězcem služby, který jste získali na začátku tohoto článku:

```sh
iothub-explorer monitor-events myiOSdevice --login "{your hub service connection string}"
```

Následující snímek obrazovky ukazuje typ telemetrických dat, která se zobrazí v okně terminálu:

![Zobrazení telemetrických dat](media/quickstart-send-telemetry-ios/view-telemetry.png)

Pokud při spuštění příkazu iothub-explorer dojde k chybě, pečlivě zkontrolujte, že používáte *připojovací řetězec služby* pro vaše centrum IoT, a ne *připojovací řetězec zařízení* pro vaše zařízení IoT. Oba připojovací řetězce začínají na **Hostname={iothubname}**, ale připojovací řetězec služby obsahuje vlastnost **SharedAccessKeyName**, zatímco připojovací řetězec zařízení obsahuje **DeviceID**. 

## <a name="clean-up-resources"></a>Vyčištění prostředků

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources.md)]

## <a name="next-steps"></a>Další kroky

V tomto článku jste nastavili centrum IoT, zaregistrovali zařízení, odeslali do centra simulovaná telemetrická data ze zařízení iOS a četli telemetrická data z centra. 

Informace o tom, jak řídit simulované zařízení z back-endové aplikace, najdete v dalším rychlém startu.

> [!div class="nextstepaction"]
> [Rychlý start: Řízení zařízení připojeného k centru IoT](quickstart-control-device-node.md)

<!-- Links -->
[lnk-process-d2c-tutorial]: tutorial-routing.md
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
