---
title: "Přijímat události z události mřížky Azure pro koncový bod HTTP"
description: "Popisuje, jak ověřit koncový bod protokolu HTTP, pak přijímat a deserializaci události z události mřížky Azure"
services: event-grid
author: banisadr
manager: darosa
ms.service: event-grid
ms.topic: article
ms.date: 02/16/2018
ms.author: babanisa
ms.openlocfilehash: da4d1aff45c39d8b4ce7cdd003dd4af35dcd33b3
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/22/2018
---
# <a name="receive-events-to-an-http-endpoint"></a>Přijímat události pro koncový bod HTTP

Tento článek popisuje, jak [ověření koncový bod HTTP](security-authentication.md#webhook-event-delivery) přijímat události z události předplatného a přijímat deserializovat události. Tento článek používá funkce Azure pro demonstrační účely, ale koncepty platí bez ohledu na to, který je hostitelem aplikace.

> [!NOTE]
> Je **důrazně** doporučeno používat [aktivační událost mřížky](../azure-functions/functions-bindings-event-grid.md) při spuštění funkce Azure s událostí mřížky. Obecný WebHook aktivační události zde slouží demonstrative.

## <a name="prerequisites"></a>Požadavky

* Budete potřebovat funkce aplikace pomocí [HTTP aktivované funkce](../azure-functions/functions-create-generic-webhook-triggered-function.md)

## <a name="add-dependencies"></a>Přidat závislosti

Pokud vyvíjíte v rozhraní .net, [Přidat závislost](../azure-functions/functions-reference-csharp.md#referencing-custom-assemblies) na funkce pro `Microsoft.Azure.EventGrid` [balíček Nuget](https://www.nuget.org/packages/Microsoft.Azure.EventGrid). Sady SDK pro jiné jazyky jsou dostupné prostřednictvím [publikování sady SDK](./sdk-overview.md#publish-sdks) odkaz. Tyto balíčky obsahovat modely pro typy nativní události, jako například `EventGridEvent`, `StorageBlobCreatedEventData`, a `EventHubCaptureFileCreatedEventData`.

To provedete kliknutím na odkaz "zobrazení soubory. ve vaší funkci Azure (na portálu Azure functions pravé většina podokno) a vytvořte soubor s názvem souboru project.json. Přidejte následující obsah `project.json` souboru a uložte jej: 
 
 ```json
{ 
  "frameworks": { 
    "net46":{ 
      "dependencies": { 
        "Microsoft.Azure.EventGrid": "1.1.0-preview" 
      } 
    } 
   } 
} 
```

![Přidání balíčku Nuget](./media/receive-events/add-dependencies.png)

## <a name="endpoint-validation"></a>Koncový bod ověření

První věc, kterou chcete udělat, je zpracování `Microsoft.EventGrid.SubscriptionValidationEvent` události. Pokaždé, když se vytvoří nový odběr událostí, odešle událost mřížky událost ověření koncový bod s `validationCode` v datovou částí. Koncový bod je potřeba echo tomto zpět v textu odpovědi na [prokázat koncový bod je platná a ve vlastnictví můžete](security-authentication.md#webhook-event-delivery). Pokud používáte [aktivační událost mřížky](../azure-functions/functions-bindings-event-grid.md) místo WebHook, jehož aktivuje funkce, koncový bod ověření zpracovává za vás.

Použijte následující kód pro zpracování ověření předplatného:

```csharp
using System.Net; 
using Newtonsoft.Json; 
using Newtonsoft.Json.Linq; 
using Newtonsoft.Json.Serialization; 
using Microsoft.Azure.EventGrid.Models; 
 
class SubscriptionValidationEventData 
{ 
    public string ValidationCode { get; set; } 
} 
 
class SubscriptionValidationResponseData 
{ 
    public string ValidationResponse { get; set; } 
} 
 
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log) 
{ 

    log.Info($"C# HTTP trigger function begun");  
    string response = string.Empty; 
    const string SubscriptionValidationEvent = "Microsoft.EventGrid.SubscriptionValidationEvent"; 
 
    string requestContent = await req.Content.ReadAsStringAsync(); 
    EventGridEvent[] eventGridEvents = JsonConvert.DeserializeObject<EventGridEvent[]>(requestContent); 
 
    foreach (EventGridEvent eventGridEvent in eventGridEvents) 
    { 
        JObject dataObject = eventGridEvent.Data as JObject; 
       
        // Deserialize the event data into the appropriate type based on event type 
        if (string.Equals(eventGridEvent.EventType, SubscriptionValidationEvent, StringComparison.OrdinalIgnoreCase)) 
        { 
            var eventData = dataObject.ToObject<SubscriptionValidationEventData>(); 
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}"); 
            // Do any additional validation (as required) and then return back the below response 
            var responseData = new SubscriptionValidationResponseData(); 
            responseData.ValidationResponse = eventData.ValidationCode; 
            return req.CreateResponse(HttpStatusCode.OK, responseData);    
        }             
    } 
     
    return req.CreateResponse(HttpStatusCode.OK, response);     
} 

```

```javascript
var http = require('http');

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";

    for (var events in req.body) {
        var body = req.body[events];
        // Deserialize the event data into the appropriate type based on event type  
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }
    }
    context.done();
}; 
```

### <a name="test-validation-response"></a>Test ověření odpovědi

Testování funkce odpovědi ověření zadáním nebo vložením událost vzorku do pole testování pro funkci:

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-01-25T22:12:19.4556811Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
```

Po kliknutí na tlačítko spustit, výstup by měl být 200 OK a `{"ValidationResponse":"512d38b6-c7b8-40c8-89fe-f46f9e9622b6"}` v textu:

![Odpověď ověřování](./media/receive-events/validation-response.png)

## <a name="handle-blob-storage-events"></a>Zpracování událostí úložiště objektů Blob

Nyní jsme můžete rozšířit funkce pro zpracování `Microsoft.Storage.BlobCreated`:

```cs
using System.Net; 
using Newtonsoft.Json; 
using Newtonsoft.Json.Linq; 
using Newtonsoft.Json.Serialization; 
using Microsoft.Azure.EventGrid.Models; 
 
class SubscriptionValidationEventData 
{ 
    public string ValidationCode { get; set; } 
} 
 
class SubscriptionValidationResponseData 
{ 
    public string ValidationResponse { get; set; } 
} 
 
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log) 
{ 
    log.Info($"C# HTTP trigger function begun");  
    string response = string.Empty; 
    const string SubscriptionValidationEvent = "Microsoft.EventGrid.SubscriptionValidationEvent";
    const string StorageBlobCreatedEvent = "Microsoft.Storage.BlobCreated"; 
 
 
    string requestContent = await req.Content.ReadAsStringAsync(); 
    EventGridEvent[] eventGridEvents = JsonConvert.DeserializeObject<EventGridEvent[]>(requestContent); 
 
    foreach (EventGridEvent eventGridEvent in eventGridEvents) 
    { 
        JObject dataObject = eventGridEvent.Data as JObject; 
       
        // Deserialize the event data into the appropriate type based on event type 
        if (string.Equals(eventGridEvent.EventType, SubscriptionValidationEvent, StringComparison.OrdinalIgnoreCase)) 
        { 
            var eventData = dataObject.ToObject<SubscriptionValidationEventData>(); 
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");

            // Do any additional validation (as required) and then return back the below response 
            var responseData = new SubscriptionValidationResponseData(); 
            responseData.ValidationResponse = eventData.ValidationCode; 
            return req.CreateResponse(HttpStatusCode.OK, responseData);    
        }

        else if (string.Equals(eventGridEvent.EventType, StorageBlobCreatedEvent, StringComparison.OrdinalIgnoreCase)) 
        { 
            var eventData = dataObject.ToObject<StorageBlobCreatedEventData>(); 
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}"); 
        }    
    } 
     
    return req.CreateResponse(HttpStatusCode.OK, response);     
}

```

```javascript
var http = require('http');

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";
    var storageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";

    for (var events in req.body) {
        var body = req.body[events];
        // Deserialize the event data into the appropriate type based on event type  
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }

        else if (body.data && body.eventType == storageBlobCreatedEvent) {
            var blobCreatedEventData = body.data;
            context.log("Relaying received blob created event payload:" + JSON.stringify(blobCreatedEventData));
        }
    }
    context.done();
};
```

### <a name="test-blob-created-event-handling"></a>Test vytvořit objekt Blob zpracování událostí

Otestovat novou funkčnost funkce umístěním [událostí úložiště objektů Blob](./event-schema-blob-storage.md#example-event) do testovací pole a spuštění:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "0x8D4BCC2E4835CD0",
    "contentType": "text/plain",
    "contentLength": 524288,
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]

```

Byste měli vidět výstup URL objektu blob v protokolu funkce:

![Výstup protokolu](./media/receive-events/blob-event-response.png)

Tuto funkci za provozu můžete také otestovat vytvořením účtu úložiště Blob nebo obecné účely V2 účtu úložiště (GPv2) [přidávání a událostí předplatné](../storage/blobs/storage-blob-event-quickstart.md)a nastavení koncového bodu na adresu URL, funkce:

![URL – funkce](./media/receive-events/function-url.png)

## <a name="handle-custom-events"></a>Popisovač vlastní události

Nakonec umožňuje rozšířit funkce jednou tak, aby ho může také zpracovat vlastních událostí. Přidáme kontrolu pro vlastní událost `Contoso.Items.ItemReceived`. Poslední kód by měl vypadat takto:

```cs
using System.Net; 
using Newtonsoft.Json; 
using Newtonsoft.Json.Linq; 
using Newtonsoft.Json.Serialization; 
using Microsoft.Azure.EventGrid.Models; 
 
class SubscriptionValidationEventData 
{ 
    public string ValidationCode { get; set; } 
} 
 
class SubscriptionValidationResponseData 
{ 
    public string ValidationResponse { get; set; } 
} 

class ContosoItemReceivedEventData 
{ 
    public string id { get; set; }     
    public string message { get; set; }   
    public string time { get; set; }
} 
 
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log) 
{ 
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty; 
    const string SubscriptionValidationEvent = "Microsoft.EventGrid.SubscriptionValidationEvent";
    const string StorageBlobCreatedEvent = "Microsoft.Storage.BlobCreated"; 
    const string CustomTopicEvent = "Contoso.Items.ItemReceived";
 
 
    string requestContent = await req.Content.ReadAsStringAsync(); 
    EventGridEvent[] eventGridEvents = JsonConvert.DeserializeObject<EventGridEvent[]>(requestContent); 
 
    foreach (EventGridEvent eventGridEvent in eventGridEvents) 
    { 
        JObject dataObject = eventGridEvent.Data as JObject; 
       
        // Deserialize the event data into the appropriate type based on event type 
        if (string.Equals(eventGridEvent.EventType, SubscriptionValidationEvent, StringComparison.OrdinalIgnoreCase)) 
        { 
            var eventData = dataObject.ToObject<SubscriptionValidationEventData>(); 
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}"); 
            // Do any additional validation (as required) and then return back the below response 
            var responseData = new SubscriptionValidationResponseData(); 
            responseData.ValidationResponse = eventData.ValidationCode; 
            return req.CreateResponse(HttpStatusCode.OK, responseData);    
        }

        else if (string.Equals(eventGridEvent.EventType, StorageBlobCreatedEvent, StringComparison.OrdinalIgnoreCase)) 
        { 
            var eventData = dataObject.ToObject<StorageBlobCreatedEventData>(); 
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}"); 
        }

        else if (string.Equals(eventGridEvent.EventType, CustomTopicEvent, StringComparison.OrdinalIgnoreCase)) 
        { 
            var eventData = dataObject.ToObject<ContosoItemReceivedEventData>(); 
            log.Info($"Got ContosoItemReceived event data, item URI {eventData.id}"); 
        } 
    } 
     
    return req.CreateResponse(HttpStatusCode.OK, response);     
}

```

```javascript
var http = require('http');
var t = require('tcomb');

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";
    var storageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";
    var customEventType = "Contoso.Items.ItemReceived";
    var ContosoItemReceivedEventData = t.struct({
        id: t.Str,
        message: t.Str,
        time: t.Str,
    })

    for (var events in req.body) {
        var body = req.body[events];
        // Deserialize the event data into the appropriate type based on event type  
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }

        else if (body.data && body.eventType == storageBlobCreatedEvent) {
            var blobCreatedEventData = body.data;
            context.log("Relaying received blob created event payload:" + JSON.stringify(blobCreatedEventData));
        }

        else if (body.data && body.eventType == customEventType) {
            var payload = new ContosoItemReceivedEventData(body.data);
            context.log("Relaying received custom payload:" + JSON.stringify(payload));
            context.log(payload instanceof ContosoItemReceivedEventData);
        }
    }
    context.done();
};
```

### <a name="test-custom-event-handling"></a>Zpracování událostí vlastní test

Nakonec testovací rozšířeného funkce dokáže zpracovat teď vaše vlastní typ události:

```json
[{
    "subject": "Contoso/foo/bar/items",
    "eventType": "Microsoft.EventGrid.CustomEventType",
    "eventTime": "2017-08-16T01:57:26.005121Z",
    "id": "602a88ef-0001-00e6-1233-1646070610ea",
    "data": { 
            "id": "831e1650-001e-001b-66ab-eeb76e069631",
            "message": "Contoso item Foo",
            "time": "2017-06-26T18:41:00.9584103Z"
            },
    "dataVersion": "",
    "metadataVersion": "1"
}]
```

Můžete také otestovat tuto funkci za provozu pomocí [odeslání vlastní události s CURL z portálu](./custom-event-quickstart-portal.md) nebo [publikování na vlastní téma](./post-to-custom-topic.md) pomocí některé služby nebo aplikace, které vystavit koncový bod například [Postman](https://www.getpostman.com/). Vytvořte vlastní téma a odběr událostí s koncovým bodem nastavit jako adresu URL funkce.

## <a name="next-steps"></a>Další postup

* Prozkoumejte [Management mřížky Azure událostí a publikovat sady SDK](./sdk-overview.md)
* Zjistěte, jak [post do vlastní tématu](./post-to-custom-topic.md)
* Použijte jeden z podrobné kurzy událostí mřížky a funkce, jako například [Změna velikosti obrázků nahrán do úložiště objektů Blob](resize-images-on-storage-blob-upload-event.md)
