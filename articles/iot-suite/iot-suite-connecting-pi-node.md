---
title: "Zřídit Malinová platformy pro vzdálené monitorování v Node.js – Azure | Microsoft Docs"
description: "Popisuje, jak se připojit k Azure IoT Suite předkonfigurované řešení vzdáleného monitorování pomocí aplikace napsané v Node.js malin platformy zařízení."
services: iot-suite
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2018
ms.author: dobett
ms.openlocfilehash: 7f489a6b26edb9a58b21d318785d3804197b33cb
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Připojte zařízení malin pí k předkonfigurovaného řešení vzdáleného monitorování (Node.js)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

V tomto kurzu se dozvíte, jak se připojit fyzického zařízení k předkonfigurovaného řešení vzdáleného monitorování. V tomto kurzu použijete Node.js, který je vhodný pro prostředí s minimálními prostředků omezení.

### <a name="required-hardware"></a>Požadovaný hardware

Stolní počítač, abyste mohli vzdáleně připojit k na příkazovém řádku malin pí.

[Microsoft IoT Starter Kit malin pí 3](https://azure.microsoft.com/develop/iot/starter-kits/) nebo ekvivalentní součásti. V tomto kurzu používá následující položky ze sady kit:

- Malinová pí 3
- Karta MicroSD (s NOOBS)
- Kabelu USB malé
- Kabel Ethernet

### <a name="required-desktop-software"></a>Požadovaný software plochy

Musíte klient SSH na umožňují vzdálený přístup na příkazovém řádku pí malin stolního počítače.

- Windows nezahrnuje klientem SSH. Doporučujeme používat [PuTTY](http://www.putty.org/).
- Většina Linuxových distribucích a Mac OS, obsahují nástroj příkazového řádku SSH. Další informace najdete v tématu [SSH pomocí Linux nebo Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-raspberry-pi-software"></a>Požadovaný software malin platformy

Pokud jste tak ještě neučinili, nainstalujte Node.js verze 4.0.0 nebo později na vaše malin pí. Následující kroky vám ukážou, jak nainstalovat Node.js v6 na vaše malin platformy:

1. Připojte se k malin pí pomocí `ssh`. Další informace najdete v tématu [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) na [web Pi malin](https://www.raspberrypi.org/).

1. Použijte následující příkaz k aktualizaci vašeho malin platformy:

    ```sh
    sudo apt-get update
    ```

1. Odebrání případné existující instalace Node.js z vaší malin platformy použijte následující příkazy:

    ```sh
    sudo apt-get remove nodered -y
    sudo apt-get remove nodejs nodejs-legacy -y
    sudo apt-get remove npm  -y
    ```

1. Použijte následující příkaz ke stažení a instalaci Node.js v6 na vaše malin platformy:

    ```sh
    curl -sL https://deb.nodesource.com/setup_6.x | sudo bash -
    sudo apt-get install nodejs -y
    ```

1. Pomocí následujícího příkazu ověřte, že jste úspěšně nainstalovali Node.js v6.11.4:

    ```sh
    node --version
    ```

## <a name="create-a-nodejs-solution"></a>Vytvoření řešení Node.js

Proveďte následující kroky pomocí `ssh` připojení k vaší malin platformy:

1. Vytvořte složku s názvem `remotemonitoring` do domovské složky na malin pí. Přejděte do této složky na příkazovém řádku:

    ```sh
    cd ~
    mkdir remotemonitoring
    cd remotemonitoring
    ```

1. Chcete-li stáhnout a nainstalovat balíčky, které potřebujete k dokončení ukázkové aplikace, spusťte následující příkazy:

    ```sh
    npm init
    npm install async azure-iot-device azure-iot-device-mqtt --save
    ```

1. V `remotemonitoring` složky, vytvořte soubor s názvem **remote_monitoring.js**. Otevřete tento soubor v textovém editoru. Na malin platformy, můžete použít `nano` nebo `vi` textové editory.

1. V **remote_monitoring.js** soubor, přidejte následující `require` příkazy:

    ```nodejs
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    var async = require('async');
    ```

1. Přidejte následující deklarace proměnných za příkazy `require`. Nahraďte hodnotu zástupného symbolu `{device connection string}` s hodnotou jste si poznamenali zařízení zřízené v řešení vzdáleného monitorování:

    ```nodejs
    var connectionString = '{device connection string}';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. Chcete-li definovat některé základní telemetrická data, přidejte následující proměnné:

    ```nodejs
    var temperature = 50;
    var temperatureUnit = 'F';
    var humidity = 50;
    var humidityUnit = '%';
    var pressure = 55;
    var pressureUnit = 'psig';
    ```

1. Chcete-li definovat některé hodnoty vlastnosti, přidejte následující proměnné:

    ```nodejs
    var temperatureSchema = 'chiller-temperature;v1';
    var humiditySchema = 'chiller-humidity;v1';
    var pressureSchema = 'chiller-pressure;v1';
    var interval = "00:00:05";
    var deviceType = "Chiller";
    var deviceFirmware = "1.0.0";
    var deviceFirmwareUpdateStatus = "";
    var deviceLocation = "Building 44";
    var deviceLatitude = 47.638928;
    var deviceLongitude = -122.13476;
    var deviceOnline = true;
    ```

1. Přidejte následující proměnnou k určení hlášené vlastností k odeslání do řešení. Tyto vlastnosti obsahovat metadata k popisu metody a používá telemetrická data zařízení:

    ```nodejs
    var reportedProperties = {
      "Protocol": "MQTT",
      "SupportedMethods": "Reboot,FirmwareUpdate,EmergencyValveRelease,IncreasePressure",
      "Telemetry": {
        "TemperatureSchema": {
          "Interval": interval,
          "MessageTemplate": "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\"}",
          "MessageSchema": {
            "Name": temperatureSchema,
            "Format": "JSON",
            "Fields": {
              "temperature": "Double",
              "temperature_unit": "Text"
            }
          }
        },
        "HumiditySchema": {
          "Interval": interval,
          "MessageTemplate": "{\"humidity\":${humidity},\"humidity_unit\":\"${humidity_unit}\"}",
          "MessageSchema": {
            "Name": humiditySchema,
            "Format": "JSON",
            "Fields": {
              "humidity": "Double",
              "humidity_unit": "Text"
            }
          }
        },
        "PressureSchema": {
          "Interval": interval,
          "MessageTemplate": "{\"pressure\":${pressure},\"pressure_unit\":\"${pressure_unit}\"}",
          "MessageSchema": {
            "Name": pressureSchema,
            "Format": "JSON",
            "Fields": {
              "pressure": "Double",
              "pressure_unit": "Text"
            }
          }
        }
      },
      "Type": deviceType,
      "Firmware": deviceFirmware,
      "FirmwareUpdateStatus": deviceFirmwareUpdateStatus,
      "Location": deviceLocation,
      "Latitude": deviceLatitude,
      "Longitude": deviceLongitude,
      "Online": deviceOnline
    }
    ```

1. Pokud chcete vytisknout výsledky operace, přidejte následující funkce, pomocné rutiny:

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. Přidejte následující pomocné funkce sloužící k náhodné telemetrie hodnoty:

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. Přidejte následující obecné funkci pro zpracování volání metody přímé z řešení. Funkce zobrazí informace o metodě direct, který byl vyvolán, ale v této ukázce neupravuje zařízení žádným způsobem. Toto řešení využívá přímé metody tak, aby fungoval na zařízení:

    ```nodejs
    function onDirectMethod(request, response) {
      // Implement logic asynchronously here.
      console.log('Simulated ' + request.methodName);

      // Complete the response
      response.send(200, request.methodName + ' was called on the device', function (err) {
        if (err) console.error('Error sending method response :\n' + err.toString());
        else console.log('200 Response to method \'' + request.methodName + '\' sent successfully.');
      });
    }
    ```

1. Přidejte následující funkci pro zpracování **FirmwareUpdate** přímé volání metod z řešení. Funkce ověřuje parametrů předaných v datové části přímá metoda a poté asynchronně spustí simulaci aktualizace firmwaru:

    ```node.js
    function onFirmwareUpdate(request, response) {
      // Get the requested firmware version from the JSON request body
      var firmwareVersion = request.payload.Firmware;
      var firmwareUri = request.payload.FirmwareUri;
      
      // Ensure we got a firmware values
      if (!firmwareVersion || !firmwareUri) {
        response.send(400, 'Missing firmware value', function(err) {
          if (err) console.error('Error sending method response :\n' + err.toString());
          else console.log('400 Response to method \'' + request.methodName + '\' sent successfully.');
        });
      } else {
        // Respond the cloud app for the device method
        response.send(200, 'Firmware update started.', function(err) {
          if (err) console.error('Error sending method response :\n' + err.toString());
          else {
            console.log('200 Response to method \'' + request.methodName + '\' sent successfully.');

            // Run the simulated firmware update flow
            runFirmwareUpdateFlow(firmwareVersion, firmwareUri);
          }
        });
      }
    }
    ```

1. Přidejte následující funkci k simulaci toku aktualizace firmwaru dlouho běžící, který sestavy průběhu řešení:

    ```node.js
    // Simulated firmwareUpdate flow
    function runFirmwareUpdateFlow(firmwareVersion, firmwareUri) {
      console.log('Simulating firmware update flow...');
      console.log('> Firmware version passed: ' + firmwareVersion);
      console.log('> Firmware URI passed: ' + firmwareUri);
      async.waterfall([
        function (callback) {
          console.log("Image downloading from " + firmwareUri);
          var patch = {
            FirmwareUpdateStatus: 'Downloading image..'
          };
          reportUpdateThroughTwin(patch, callback);
          sleep(10000, callback);
        },
        function (callback) {
          console.log("Downloaded, applying firmware " + firmwareVersion);
          deviceOnline = false;
          var patch = {
            FirmwareUpdateStatus: 'Applying firmware..',
            Online: false
          };
          reportUpdateThroughTwin(patch, callback);
          sleep(8000, callback);
        },
        function (callback) {
          console.log("Rebooting");
          var patch = {
            FirmwareUpdateStatus: 'Rebooting..'
          };
          reportUpdateThroughTwin(patch, callback);
          sleep(10000, callback);
        },
        function (callback) {
          console.log("Firmware updated to " + firmwareVersion);
          deviceOnline = true;
          var patch = {
            FirmwareUpdateStatus: 'Firmware updated',
            Online: true,
            Firmware: firmwareVersion
          };
          reportUpdateThroughTwin(patch, callback);
          callback(null);
        }
      ], function(err) {
        if (err) {
          console.error('Error in simulated firmware update flow: ' + err.message);
        } else {
          console.log("Completed simulated firmware update flow");
        }
      });

      // Helper function to update the twin reported properties.
      function reportUpdateThroughTwin(patch, callback) {
        console.log("Sending...");
        console.log(JSON.stringify(patch, null, 2));
        client.getTwin(function(err, twin) {
          if (!err) {
            twin.properties.reported.update(patch, function(err) {
              if (err) callback(err);
            });      
          } else {
            if (err) callback(err);
          }
        });
      }

      function sleep(milliseconds, callback) {
        console.log("Simulate a delay (milleseconds): " + milliseconds);
        setTimeout(function () {
          callback(null);
        }, milliseconds);
      }
    }
    ```

1. Přidejte následující kód do Neodesílat telemetrická data do řešení. Klientská aplikace přidá vlastnosti zprávy k identifikaci schéma zpráva:

    ```node.js
    function sendTelemetry(data, schema) {
      if (deviceOnline) {
        var d = new Date();
        var payload = JSON.stringify(data);
        var message = new Message(payload);
        message.properties.add('$$CreationTimeUtc', d.toISOString());
        message.properties.add('$$MessageSchema', schema);
        message.properties.add('$$ContentType', 'JSON');

        console.log('Sending device message data:\n' + payload);
        client.sendEvent(message, printErrorFor('send event'));
      } else {
        console.log('Offline, not sending telemetry');
      }
    }
    ```

1. Přidejte následující kód k vytvoření instance klienta:

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Přidejte následující kód do:

    * Otevření připojení.
    * Nastavte obslužnou rutinu pro požadované vlastnosti.
    * Odešlete oznámenou vlastnosti.
    * Zaregistrujte obslužné rutiny pro přímé metody. Příklad používá samostatné obslužnou rutinu pro metodu přímé aktualizace firmwaru.
    * Zahájit odesílání telemetrie.

    ```nodejs
    client.open(function (err) {
      if (err) {
        printErrorFor('open')(err);
      } else {
        // Create device Twin
        client.getTwin(function (err, twin) {
          if (err) {
            console.error('Could not get device twin');
          } else {
            console.log('Device twin created');

            twin.on('properties.desired', function (delta) {
              // Handle desired properties set by solution
              console.log('Received new desired properties:');
              console.log(JSON.stringify(delta));
            });

            // Send reported properties
            twin.properties.reported.update(reportedProperties, function (err) {
              if (err) throw err;
              console.log('Twin state reported');
            });

            // Register handlers for all the method names we are interested in.
            // Consider separate handlers for each method.
            client.onDeviceMethod('Reboot', onDirectMethod);
            client.onDeviceMethod('FirmwareUpdate', onFirmwareUpdate);
            client.onDeviceMethod('EmergencyValveRelease', onDirectMethod);
            client.onDeviceMethod('IncreasePressure', onDirectMethod);
          }
        });

        // Start sending telemetry
        var sendTemperatureInterval = setInterval(function () {
          temperature += generateRandomIncrement();
          var data = {
            'temperature': temperature,
            'temperature_unit': temperatureUnit
          };
          sendTelemetry(data, temperatureSchema)
        }, 5000);

        var sendHumidityInterval = setInterval(function () {
          humidity += generateRandomIncrement();
          var data = {
            'humidity': humidity,
            'humidity_unit': humidityUnit
          };
          sendTelemetry(data, humiditySchema)
        }, 5000);

        var sendPressureInterval = setInterval(function () {
          pressure += generateRandomIncrement();
          var data = {
            'pressure': pressure,
            'pressure_unit': pressureUnit
          };
          sendTelemetry(data, pressureSchema)
        }, 5000);

        client.on('error', function (err) {
          printErrorFor('client')(err);
          if (sendTemperatureInterval) clearInterval(sendTemperatureInterval);
          if (sendHumidityInterval) clearInterval(sendHumidityInterval);
          if (sendPressureInterval) clearInterval(sendPressureInterval);
          client.close(printErrorFor('client.close'));
        });
      }
    });
    ```

1. Uložit změny **remote_monitoring.js** souboru.

1. Spuštění ukázkové aplikace, spusťte následující příkaz na příkazovém řádku na malin platformy:

    ```sh
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
