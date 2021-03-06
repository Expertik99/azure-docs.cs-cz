---
title: Řazení událostí připojení zařízení ze služby Azure IoT Hub pomocí služby Azure Cosmos DB | Dokumentace Microsoftu
description: Tento článek popisuje, jak uspořádat a zaznamenávat události připojení zařízení ze služby Azure IoT Hub pomocí služby Azure Cosmos DB k údržbě nejnovější stav připojení
services: iot-hub
documentationcenter: ''
author: ash2017
manager: briz
editor: ''
ms.service: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/06/2018
ms.author: asrastog
ms.openlocfilehash: dd3c750e93646624e46c46afd871ef75af905bf0
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/28/2018
ms.locfileid: "43145942"
---
# <a name="order-device-connection-events-from-azure-iot-hub-using-azure-cosmos-db"></a>Objednat zařízení události připojení ze služby Azure IoT Hub pomocí služby Azure Cosmos DB

Azure Event Grid umožňuje vytvářet aplikace založené na události a jednoduchá integrace událostí IoT v business solutions. Tento článek vás provede sadu který slouží ke sledování a uložit nejnovější stav připojení zařízení do služby Cosmos DB. Společnost Microsoft použije pořadové číslo, které jsou k dispozici v událostech zařízení připojené a odpojené zařízení a nejnovější stav uložený ve službě Cosmos DB. Budeme používat uložené procedury, která je aplikační logiky, která se provede na kolekce v databázi Cosmos DB.

Pořadové číslo je řetězcové vyjádření šestnáctkového čísla. Porovnání řetězců můžete použít k identifikaci většímu počtu. Při převodu řetězce do šestnáctkové soustavy, číslo bude číslo 256 bitů. Pořadové číslo výhradně roste a nejnovější událost může mít větší číslo než jiné události. To je užitečné, pokud máte časté zařízení připojí a odpojí a mít jistotu, že pouze nejnovější událost se používá k aktivaci podřízené akce, jako je Azure Event Grid nepodporuje řazení událostí.

## <a name="prerequisites"></a>Požadavky

* Aktivní účet Azure. Pokud žádný nemáte, můžete si [vytvořit bezplatný účet](http://azure.microsoft.com/pricing/free-trial/).
* Aktivní účet rozhraní SQL API služby Azure Cosmos DB. Pokud ještě jeden ještě nevytvořili, přečtěte si téma [vytvoření databázového účtu](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-dotnet#create-a-database-account) návod.
* Kolekce v databázi. Zobrazit [přidat kolekci](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-dotnet#add-a-collection) návod.
* Centrum IoT v Azure. Pokud jste si ještě žádné nevytvořili, přečtěte si téma [Začínáme se službou IoT Hub](../iot-hub/iot-hub-csharp-csharp-getstarted.md), kde najdete návod. 

## <a name="create-a-stored-procedure"></a>Vytvoření uložené procedury

Nejprve vytvořte uloženou proceduru a nastavte ji do spustit logiku, která porovná čísla pořadí příchozích událostí a zaznamenává nejnovější událost za každou zařízení v databázi.

1. V rozhraní SQL API Cosmos DB vyberte **Průzkumník dat** > **položky** > **novou uloženou proceduru**.

   ![Vytvořit uloženou proceduru](./media/iot-hub-how-to-order-connection-state-events/create-stored-procedure.png)

2. Zadejte id uložené procedury a vložte následující text uložené procedury"". Všimněte si, že tento kód by měl nahraďte existující kód v těle uloženou proceduru. Tento kód udržuje jeden řádek na ID zařízení a zaznamenává nejnovější stav připojení toto id zařízení díky identifikaci nejvyšší pořadové číslo. 

    ```javascript
    // SAMPLE STORED PROCEDURE
    function UpdateDevice(deviceId, moduleId, hubName, connectionState, connectionStateUpdatedTime, sequenceNumber) {
      var collection = getContext().getCollection();
      var response = {};
      
      var docLink = getDocumentLink(deviceId, moduleId);

      var isAccepted = collection.readDocument(docLink, function(err, doc) {
        if (err) {
          console.log('Cannot find device ' + docLink + ' - ');
          createDocument();
        } else {
          console.log('Document Found - ');
          replaceDocument(doc);
        }
      });

      function replaceDocument(document) {
        console.log(
          'Old Seq :' +
            document.sequenceNumber +
            ' New Seq: ' +
            sequenceNumber +
            ' - '
        );
        if (sequenceNumber > document.sequenceNumber) {
          document.connectionState = connectionState;
          document.connectionStateUpdatedTime = connectionStateUpdatedTime;
          document.sequenceNumber = sequenceNumber;

          console.log('replace doc - ');

          isAccepted = collection.replaceDocument(docLink, document, function(
            err,
            updated
          ) {
            if (err) {
              getContext()
                .getResponse()
                .setBody(err);
            } else {
              getContext()
                .getResponse()
                .setBody(updated);
            }
          });
        } else {
          getContext()
            .getResponse()
            .setBody('Old Event - current: ' + document.sequenceNumber + ' Incoming: ' + sequenceNumber);
        }
      }
      function createDocument() {
        document = {
          id: deviceId + '-' + moduleId,
          deviceId: deviceId,
          moduleId: moduleId,
          hubName: hubName,
          connectionState: connectionState,
          connectionStateUpdatedTime: connectionStateUpdatedTime,
          sequenceNumber: sequenceNumber
        };
        console.log('Add new device - ' + collection.getAltLink());
        isAccepted = collection.createDocument(
          collection.getAltLink(),
          document,
          function(err, doc) {
            if (err) {
              getContext()
                .getResponse()
                .setBody(err);
            } else {
              getContext()
                .getResponse()
                .setBody(doc);
            }
          }
        );
      }

      function getDocumentLink(deviceId, moduleId) {
        return collection.getAltLink() + '/docs/' + deviceId + '-' + moduleId;
      }
    }
    ```

3. Uložte uložená procedura: 

    ![uložit uložené procedury](./media/iot-hub-how-to-order-connection-state-events/save-stored-procedure.png)

## <a name="create-a-logic-app"></a>Vytvoření aplikace logiky

Napřed vytvořte aplikaci logiky a přidejte trigger služby Event Grid, který ve skupině prostředků monitoruje váš virtuální počítač. 

### <a name="create-a-logic-app-resource"></a>Vytvořte prostředek aplikace logiky

1. V [webu Azure portal](https://portal.azure.com)vyberte **nový** > **integrace** > **aplikace logiky**.

   ![Vytvoření aplikace logiky](./media/iot-hub-how-to-order-connection-state-events/select-logic-app.png)

2. Pojmenujte svoji aplikaci logiky jedinečným názvem v rámci vašeho předplatného a potom vyberte stejné předplatné, skupinu prostředků a umístění, jako má vaše centrum IoT. 
3. Až to budete mít, vyberte **Připnout na řídicí panel** a zvolte **Vytvořit**.

   Právě jste vytvořili prostředek Azure pro vaši aplikaci logiky. Až Azure nasadí aplikaci logiky, v Návrháři pro Logic Apps se zobrazí šablony pro obvyklé scénáře, abyste mohli začít rychleji.

   > [!NOTE] 
   > Když vyberete **Připnout na řídicí panel**, aplikace logiky se automaticky otevře v návrháři pro Logic Apps. Nebo můžete aplikaci logiky najít a otevřít ručně.

4. V návrháři aplikace logiky v části **Šablony** zvolte **Prázdná aplikace logiky**, abyste mohli sestavit zcela novou aplikaci logiky.

### <a name="select-a-trigger"></a>Výběr triggeru

Trigger je konkrétní událost, která spustí aplikaci logiky. V tomto kurzu trigger, který spustí pracovní postup, přijímá žádost přes protokol HTTP.  

1. Do panelu hledání pro konektory a triggery zadejte **HTTP**.
2. Vyberte jako trigger **Žádost – Při přijetí požadavku HTTP**. 

   ![Výběr triggeru požadavku HTTP](./media/iot-hub-how-to-order-connection-state-events/http-request-trigger.png)

3. Vyberte **K vygenerování schématu použijte ukázkovou datovou část**. 

   ![Výběr triggeru požadavku HTTP](./media/iot-hub-how-to-order-connection-state-events/sample-payload.png)

4. Do textového pole vložte následující ukázkový kód JSON a potom vyberte **Hotovo**:

   ```json
   [{
    "id": "fbfd8ee1-cf78-74c6-dbcf-e1c58638ccbd",
    "topic":
      "/SUBSCRIPTIONS/DEMO5CDD-8DAB-4CF4-9B2F-C22E8A755472/RESOURCEGROUPS/EGTESTRG/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/MYIOTHUB",
    "subject": "devices/Demo-Device-1",
    "eventType": "Microsoft.Devices.DeviceConnected",
    "eventTime": "2018-07-03T23:20:11.6921933+00:00",
    "data": {
      "deviceConnectionStateEventInfo": {
        "sequenceNumber":
          "000000000000000001D4132452F67CE200000002000000000000000000000001"
      },
      "hubName": "MYIOTHUB",
      "deviceId": "48e44e11-1437-4907-83b1-4a8d7e89859e",
      "moduleId": ""
    },
    "dataVersion": "1",
    "metadataVersion": "1"
   }]
   ```

5. Můžete se zobrazit automaticky otevírané okno s oznámením **Nezapomeňte do svého požadavku přidat hlavičku Content-Type nastavenou na application/json**. Tento návrh můžete v klidu ignorovat a přejít k další části. 

### <a name="create-a-condition"></a>Vytvořit podmínku

Nápověda podmínky spuštění konkrétní akce po úspěšném určité podmínky, ve které jste pracovní postup aplikace logiky. Když je splněna podmínka, lze definovat požadovanou akci. Pro účely tohoto kurzu je podmínka zkontroluje, jestli je typ eventType zařízení připojení nebo odpojení zařízení. Akce, budete moct spustit uloženou proceduru v databázi. 

1. Vyberte **nový krok** pak **předdefinované** a **podmínku**. 

2. Zadejte podmínku, jak je znázorněno níže na spustit pouze pro události zařízení připojené a odpojené zařízení:
  * Zvolte hodnotu: **typ události**
  * Změna je rovno" **končí**
  * Zvolte hodnotu: **nected**

   ![Zadejte podmínku](./media/iot-hub-how-to-order-connection-state-events/condition-detail.png)

3. Pokud je podmínka pravdivá, klikněte na **přidat akci**.
  
   ![Přidání akce, pokud je true](./media/iot-hub-how-to-order-connection-state-events/action-if-true.png)

4. Vyhledání služby Cosmos DB a klikněte na **Azure Cosmos DB – spustit uloženou proceduru**

   ![Vyhledávání pro službu cosmos DB](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-search.png)

5. Naplnění formuláře pro spuštění uložené pořídit výběrem hodnoty z databáze. Zadejte hodnotu klíče oddílu a parametry, jak vidíte níže. 

   ![naplnění akce aplikace logiky](./media/iot-hub-how-to-order-connection-state-events/logicapp-stored-procedure.png)

6. Uložte svou aplikaci logiky. 

### <a name="copy-the-http-url"></a>Zkopírujte adresu URL protokolu HTTP

Než odejdete z návrháře aplikace logiky, zkopírujte adresu URL, kterou vaše aplikace logiky sleduje kvůli triggeru. Pomocí této adresy URL nakonfigurujete Event Grid. 

1. Kliknutím rozbalte konfigurační pole triggeru **Při přijetí požadavku HTTP**. 
2. Tlačítkem vedle hodnoty **Adresa URL operace HTTP POST** tuto hodnotu zkopírujte. 

   ![Zkopírování adresy URL operace HTTP POST](./media/iot-hub-how-to-order-connection-state-events/copy-url.png)

3. Adresu URL si uložte, abyste na ni mohli odkazovat v další části. 

## <a name="configure-subscription-for-iot-hub-events"></a>Konfigurace odběru událostí služby IoT Hub

V této části nakonfigurujete v IoT Hubu publikování událostí, když k nim dojde. 

1. Na webu Azure Portal přejděte do svého centra IoT. 
2. Vyberte **Události**.

   ![Otevření podrobností Event Gridu](./media/iot-hub-how-to-order-connection-state-events/event-grid.png)

3. Vyberte **Odběr události**. 

   ![Vytvoření nového odběru události](./media/iot-hub-how-to-order-connection-state-events/event-subscription.png)

4. Vytvořte odběr události s následujícími hodnotami: 
   * **Typ události**: zrušte zaškrtnutí políčka přihlášení k odběru všech typů událostí a vyberte **zařízení připojeno** a **odpojení zařízení** z nabídky.
   * **Podrobnosti o koncovém bodu**: Vyberte typ koncového bodu jako **Webhook** a klikněte na Vybrat koncový bod a vložte adresu URL, kterou jste zkopírovali z aplikace logiky a potvrďte výběr.

   ![Vyberte adresu url koncového bodu](./media/iot-hub-how-to-order-connection-state-events/endpoint-url.png)

   * **Podrobnosti o předplatném události**: Zadejte popisný název a vyberte **Event Grid schématu** až to budete mít formulář by měl vypadat jako v následujícím příkladu: 

   ![Ukázkový formulář odběru události](./media/iot-hub-how-to-order-connection-state-events/subscription-form.png)

5. Výběrem možnosti **Vytvořit** uložte odběr události.

## <a name="observe-events"></a>Sledovat události
Teď, když je nastavení odběru události, můžeme otestovat připojení zařízení.

### <a name="register-a-device-in-iot-hub"></a>Registrace zařízení ve službě IoT Hub

1. V centru IoT vyberte **Zařízení IoT**. 
2. Vyberte **Přidat**.
3. Pro **ID zařízení** zadejte `Demo-Device-1`.
4. Vyberte **Uložit**. 
5. Můžete přidat víc zařízení přes různé identifikátory zařízení.

   ![Jak výsledku](./media/iot-hub-how-to-order-connection-state-events/AddIoTDevice.png)

6. Kopírovat **připojovací řetězec – primární klíč** pro pozdější použití.

   ![Jak výsledku](./media/iot-hub-how-to-order-connection-state-events/DeviceConnString.png)

### <a name="start-raspberry-pi-simulator"></a>Spusťte simulátor Raspberry Pi

1. Umožňuje použít webový simulátor Raspberry Pi pro simulaci zařízení připojení.

[Spusťte simulátor Raspberry Pi](https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted)

### <a name="run-a-sample-applciation-on-the-raspberry-pi-web-simulator"></a>Spuštění ukázkové aplikace na webový simulátor Raspberry Pi
Tím se aktivuje událost připojené zařízení.

1. V oblasti kódování nahraďte zástupný text v řádku 15 připojovací řetězec zařízení Azure IoT Hub.

   ![Jak výsledku](./media/iot-hub-how-to-order-connection-state-events/raspconnstring.png)

2. Spusťte aplikaci kliknutím na **spustit**.

Měli byste vidět následující výstup, který zobrazuje data ze senzorů a zprávy, které se odesílají do služby IoT hub.

   ![Jak výsledku](./media/iot-hub-how-to-order-connection-state-events/raspmsg.png)

   Klikněte na tlačítko **Zastavit** zastavit simulátor a aktivační události **odpojení zařízení** událostí.

Nyní jste spustili ukázkovou aplikaci shromažďovat data ze senzorů a odesílat je do služby IoT hub. 

### <a name="observe-events-in-cosmos-db"></a>Sledovat události ve službě Cosmos DB

Zobrazí se výsledky provést uloženou proceduru v dokumentu Cosmos DB. Tady je to bude vypadat. Všimněte si, že každý řádek obsahuje nejnovější stav připojení zařízení na zařízení

   ![Jak výsledku](./media/iot-hub-how-to-order-connection-state-events/cosmosDB-outcome.png)

## <a name="use-the-azure-cli"></a>Použití Azure CLI

Místo použití webu Azure Portal můžete provést kroky služby IoT Hub pomocí rozhraní příkazového řádku Azure CLI. Podrobnosti najdete na stránkách rozhraní příkazového řádku Azure pro [vytvoření odběru událostí](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) a [vytvoření zařízení IoT](https://docs.microsoft.com/cli/azure/iot/device).

## <a name="clean-up-resources"></a>Vyčištění prostředků

Tento kurz využívá prostředky, za které vám můžou být v předplatném Azure účtovány poplatky. Po dokončení tohoto zkušebního kurzu a otestování výsledků zakažte nebo odstraňte prostředky, které si nechcete nechat. 

Pokud nechcete přijít o práci na aplikaci logiky, místo odstranění ji zakažte. 

1. Přejděte do aplikace logiky.
2. V okně **Přehled** vyberte **Odstranit** nebo **Zakázat**. 

Každý odběr může mít jedno bezplatné centrum IoT. Pokud jste vytvořili bezplatné centrum pro účely tohoto kurzu, tak ho nemusíte odstraňovat, aby se vám nic neúčtovalo.

1. Přejděte do svého centra IoT. 
2. V okně **Přehled** vyberte **Odstranit**. 

I když si centrum IoT necháte, bude vhodné odstranit odběr události, který jste vytvořili. 

1. V centru IoT vyberte **Mřížka událostí**.
2. Vyberte odběr události, který chcete odebrat. 
3. Vyberte **Odstranit**. 

Odebrat účet služby Azure Cosmos DB na webu Azure Portal, práva klikněte na název účtu a klepněte na tlačítko **odstranit účet**. Přečtěte si podrobné pokyny pro [odstranění účtu služby Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/manage-account#delete).

## <a name="next-steps"></a>Další postup

* Další informace o [reakce na události služby IoT Hub s využitím služby Event Grid pro aktivaci akcí](../iot-hub/iot-hub-event-grid.md)
* [Projděte si kurz událostí služby IoT Hub](../event-grid/publish-iot-hub-events-to-logic-apps.md)
* Další informace o tom, co jiného vám pomůžou s [služby Event Grid](../event-grid/overview.md)


