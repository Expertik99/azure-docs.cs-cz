---
title: "Triggerů a vazeb v Azure Functions"
description: "Další informace o použití triggerů a vazeb v Azure Functions k připojení vašeho provádění kódu k online události a cloudové služby."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "funkce azure, funkce, zpracování událostí, webhook, dynamické výpočty, architektura bez serverů"
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: e7141d92a186bec67c374bd5046ee08047feedec
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/24/2018
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure funkce triggerů a vazeb koncepty

Tento článek je koncepční přehled triggerů a vazeb v Azure Functions. Funkce, které jsou společné pro všechny vazby a všechny podporované jazyky jsou popsané v tomto poli.

## <a name="overview"></a>Přehled

A *aktivační událost* definuje způsob volání funkce. Funkce musí mít přesně jeden aktivační události. Aktivační události mají související data, která je obvykle datové části, která aktivuje funkce.

Vstup a výstup *vazby* poskytnout deklarativní způsob, jak se připojit k datům z vašeho kódu. Vazby jsou volitelné a funkci můžete mít více vstup a výstup vazby. 

Triggerů a vazeb umožňují vyhnout hardcoding podrobnosti o služby, které pracujete s. Funkce je přijímá data (například obsah zprávy fronty) v parametry funkce. Odesílat data (například pro vytvoření zprávy fronty) pomocí návratovou hodnotu funkce, `out` parametr nebo [objekt kolekce](functions-reference-csharp.md#writing-multiple-output-values).

Při vývoji funkce pomocí portálu Azure, triggerů a vazeb konfigurované v *function.json* souboru. Na portálu poskytuje uživatelské rozhraní pro tuto konfiguraci, ale můžete upravit soubor přímo změna **pokročilé editor**.

Při vývoji funkce pomocí sady Visual Studio k vytvoření knihovny tříd, konfigurujete triggerů a vazeb a architekturu metody a parametry s atributy.

## <a name="supported-bindings"></a>Podporované vazby

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Informace o tom, které jsou ve verzi preview vazby, nebo jsou schváleny pro použití v provozním prostředí najdete v tématu [podporované jazyky](supported-languages.md).

## <a name="example-trigger-and-binding"></a>Příklad aktivační události a vazby

Předpokládejme, že chcete k zápisu nového řádku do Azure Table storage, vždy, když v rámci Azure Queue storage se objeví nová zpráva. Tento scénář může být implementovaná pomocí Azure Queue aktivační událost úložiště a Azure Table storage výstupní vazby. 

Tady je *function.json* souboru pro tento scénář. 

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

Prvním elementem v `bindings` pole je aktivační událost fronty úložiště. `type` a `direction` vlastnosti identifikovat aktivační událost. `name` Vlastnost identifikuje parametr funkce, která bude přijímat obsah zprávy fronty. Název fronty k monitorování se `queueName`, a připojovací řetězec je v nastavení aplikace identifikovaný `connection`.

Druhý prvkem v `bindings` pole je Azure Table Storage výstup vazby. `type` a `direction` vlastnosti identifikovat vazby. `name` Vlastnost určuje, jak funkce zajistí řádku nové tabulky v tomto případě pomocí funkce návratovou hodnotu. Název tabulky se `tableName`, a připojovací řetězec je v nastavení aplikace identifikovaný `connection`.

K zobrazení a úprava obsahu *function.json* na portálu Azure klikněte na tlačítko **pokročilé editor** možnost **integrací** kartě funkce.

> [!NOTE]
> Hodnota `connection` je název nastavení aplikace, který obsahuje připojovací řetězec, ne samotný připojovací řetězec. Vazby používat připojení řetězce, které jsou uložené v nastavení aplikace vynutit nejlepší praxi, který *function.json* neobsahuje tajné klíče služby.

Zde je C# kód skriptu, který funguje s této aktivační události a vazby. Všimněte si, že je název parametru, který poskytuje obsah zprávy fronty `order`; tento název je povinný, protože `name` hodnoty vlastností v *function.json* je `order` 

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table storage
// The method return value creates a new row in Table Storage
public static Person Run(JObject order, TraceWriter log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}
 
public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

Pomocí funkce JavaScript lze použít ke stejnému souboru function.json:

```javascript
// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The second parameter to context.done is used as the value for the new row
module.exports = function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.done(null, order);
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```

Knihovny tříd, stejné aktivační události a informace o vazbě &mdash; názvu tabulky a fronty, účty úložiště, funkce parametry pro vstup a výstup &mdash; poskytuje atributy místo function.json souboru. Tady je příklad:

```csharp
 public static class QueueTriggerTableOutput
 {
     [FunctionName("QueueTriggerTableOutput")]
     [return: Table("outTable", Connection = "MY_TABLE_STORAGE_ACCT_APP_SETTING")]
     public static Person Run(
         [QueueTrigger("myqueue-items", Connection = "MY_STORAGE_ACCT_APP_SETTING")]JObject order, 
         TraceWriter log)
     {
         return new Person() {
                 PartitionKey = "Orders",
                 RowKey = Guid.NewGuid().ToString(),
                 Name = order["Name"].ToString(),
                 MobileNumber = order["MobileNumber"].ToString() };
     }
 }

 public class Person
 {
     public string PartitionKey { get; set; }
     public string RowKey { get; set; }
     public string Name { get; set; }
     public string MobileNumber { get; set; }
 }
```

## <a name="binding-direction"></a>Směr vazby

Mají všechny triggerů a vazeb `direction` vlastnost *function.json* souboru:

- Pro aktivační události směr je vždy `in`
- Vstupní a výstupní vazby používat `in` a `out`
- Některé vazby podporují speciální směr `inout`. Pokud používáte `inout`, jenom **pokročilé editor** je k dispozici v **integrací** kartě.

Při použití [atributy v knihovně tříd](functions-dotnet-class-library.md) konfigurace triggerů a vazeb, směr uvedené v atributu konstruktoru nebo odvozené od typu parametru.

## <a name="using-the-function-return-value"></a>Pomocí návratové hodnoty funkce

V jazycích, které mají návratovou hodnotu můžete vázat vazbu výstup na návratovou hodnotu:

* V jazyce C# knihovny tříd použijte atribut vazby výstup na návratovou hodnotu metody.
* V jiných jazycích, nastavte `name` vlastnost *function.json* k `$return`.

Pokud potřebujete napsat více než jednu položku, použijte [objekt kolekce](functions-reference-csharp.md#writing-multiple-output-values) místo návratovou hodnotu. Pokud existuje víc vazeb výstup, použijte návratovou hodnotu pouze pro jeden z nich.

Podívejte se na konkrétní jazyk příklad:

* [C#](#c-example)
* [C# skript (.csx)](#c-script-example)
* [F#](#f-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>Příklad jazyka C#

Zde uvádíme C# kód, který používá pro vazbu výstup, za nímž následuje příklad asynchronní návratovou hodnotu:

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static string Run([QueueTrigger("inputqueue")]WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static Task<string> Run([QueueTrigger("inputqueue")]WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

### <a name="c-script-example"></a>Příklad skriptu jazyka C#

Zde je vazba výstup v *function.json* souboru:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Tady je C# kód skriptu, za nímž následuje příklad asynchronní:

```cs
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

### <a name="f-example"></a>Příklad F #

Zde je vazba výstup v *function.json* souboru:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Tady je kód F #:

```fsharp
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

### <a name="javascript-example"></a>Příklad v jazyce JavaScript

Zde je vazba výstup v *function.json* souboru:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

V jazyce JavaScript, je třeba návratovou hodnotu do druhý parametr pro `context.done`:

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

## <a name="binding-datatype-property"></a>Vlastnost dataType vazby

V rozhraní .NET použijte k definování typu dat pro vstupní data, typ parametru. Například použijte `string` k vytvoření vazby na text aktivační procedury fronty, bajtové pole pro čtení jako binární a vlastní typ k deserializaci na objekt objektů POCO.

Pro jazyky, které jsou zadány dynamicky například JavaScript, použijte `dataType` vlastnost *function.json* souboru. Například číst obsah požadavku HTTP v binárním formátu, nastavte `dataType` k `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Další možnosti pro `dataType` jsou `stream` a `string`.

## <a name="binding-expressions-and-patterns"></a>Výrazy vazba a vzory

Jedním z nejúčinnějších funkce triggerů a vazeb je *vazby výrazy*. V *function.json* soubor a parametry funkce nebo kód, můžete v výrazy, které odkazují na hodnoty z různých zdrojů.

Většina výrazy jsou identifikovány jejich zabalení do složených závorek. Například ve frontě aktivační události funkci `{queueTrigger}` přeloží na text zprávy fronty. Pokud `path` vlastností pro objekt blob výstup vazba je `container/{queueTrigger}` a funkce se aktivuje zprávu fronty `HelloWorld`, objekt blob s názvem `HelloWorld` je vytvořena.

Typy výrazů vazby

* [Nastavení aplikace](#binding-expressions---app-settings)
* [Název souboru aktivační události](#binding-expressions---trigger-file-name)
* [Aktivační událost metadat](#binding-expressions---trigger-metadata)
* [Datové části JSON](#binding-expressions---json-payloads)
* [Nový identifikátor GUID](#binding-expressions---create-guids)
* [Aktuální datum a čas](#binding-expressions---current-time)

### <a name="binding-expressions---app-settings"></a>Výrazy vazby – nastavení aplikace

Jako osvědčený postup tajné klíče a připojovací řetězce musí být řízen pomocí nastavení aplikace, nikoli konfigurační soubory. To omezuje přístup na těchto tajných klíčů a umožňuje bezpečné uložení souborů, jako *function.json* ve veřejné zdrojová ovládací prvek úložiště.

Nastavení aplikace jsou užitečné také vždy, když chcete změnit konfiguraci na základě prostředí. Například v testovacím prostředí, můžete monitorovat jiný kontejner fronty nebo objekt blob úložiště.

Výrazy vazby nastavení aplikace jsou identifikovány jinak z jiné výrazy vazby: jsou zabaleny v znaky procenta spíše než složené závorky. Například pokud je cesta k objektu blob výstup vazby `%Environment%/newblob.txt` a `Environment` hodnota nastavení aplikace je `Development`, vytvoří se objekt blob v `Development` kontejneru.

Když funkce běží místně, hodnoty nastavení aplikace pocházet z *local.settings.json* souboru.

Všimněte si, že `connection` vlastnost triggerů a vazeb je zvláštní případ a automaticky vyřeší hodnoty jako nastavení aplikace, bez znaky procenta. 

Následující příklad je aktivační událost INSTEAD Azure Queue Storage, který používá nastavení aplikace `%input-queue-name%` k definování fronty k aktivaci na.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "%input-queue-name%",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

Můžete použít ve stejný přístup v knihovny tříd:

```csharp
[FunctionName("QueueTrigger")]
public static void Run(
    [QueueTrigger("%input-queue-name%")]string myQueueItem, 
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
}
```

### <a name="binding-expressions---trigger-file-name"></a>Výrazy vazby – název souboru aktivační události

`path` Pro objekt Blob může být aktivační událost vzor, který vám umožní odkaz na název objektu blob spouštěcí v jiných vazby a funkce kódu. Vzor může také zahrnovat kritéria filtrování, které určují, které objekty BLOB můžete aktivovat volání funkce.

Například v následující aktivační události objektu Blob vazba `path` vzor je `sample-images/{filename}`, která vytvoří výraz vazba s názvem `filename`:

```json
{
  "bindings": [
    {
      "name": "image",
      "type": "blobTrigger",
      "path": "sample-images/{filename}",
      "direction": "in",
      "connection": "MyStorageConnection"
    },
    ...
```

Výraz `filename` lze potom použít v vazbu výstup zadat název objektu blob vytváří:

```json
    ...
    {
      "name": "imageSmall",
      "type": "blob",
      "path": "sample-images-sm/{filename}",
      "direction": "out",
      "connection": "MyStorageConnection"
    }
  ],
}
```

Kód funkce má přístup k této stejnou hodnotu pomocí `filename` jako název parametru:

```csharp
// C# example of binding to {filename}
public static void Run(Stream image, string filename, Stream imageSmall, TraceWriter log)  
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->

Stejné možnost používat výrazy vazba a vzory se vztahuje na atributy v knihovny tříd. V následujícím příkladu parametry konstruktoru atributu jsou stejné `path` hodnoty jako předchozí *function.json* příklady: 

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{filename}")] Stream image,
    [Blob("sample-images-sm/{filename}", FileAccess.Write)] Stream imageSmall,
    string filename,
    TraceWriter log)
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
}

```

Můžete také vytvořit výrazy pro částí názvu souboru, jako je například rozšíření. Další informace o tom, jak používat výrazy a vzory v řetězec cesty objektu Blob najdete v tématu [odkaz vazby úložiště objektů blob](functions-bindings-storage-blob.md).
 
### <a name="binding-expressions---trigger-metadata"></a>Výrazy vazby - metadata aktivační události

Kromě datová poskytuje mnoho aktivační události aktivační událost (jako je třeba obsah zprávy ve frontě, která aktivuje funkci), zadejte hodnoty dalších metadat. Tyto hodnoty lze použít jako vstupní parametry v C# a F # nebo vlastnosti na `context.bindings` objektu v jazyce JavaScript. 

Například aktivační procedury fronty Azure storage podporuje následující vlastnosti:

* QueueTrigger - aktivován obsah zprávy, pokud platný řetězec
* DequeueCount
* ExpirationTime
* ID
* InsertionTime
* NextVisibleTime
* PopReceipt

Tyto hodnoty metadata jsou dostupné v *function.json* vlastnosti souboru. Předpokládejme například, použít aktivační událost fronty a zprávy ve frontě obsahuje název objektu blob, které chcete číst. V *function.json* souboru, můžete použít `queueTrigger` metadata vlastnost v objektu blob `path` vlastnost, jak je znázorněno v následujícím příkladu:

```json
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "direction": "in",
      "connection": "MyStorageConnection"
    }
  ]
```

Podrobnosti o vlastnosti metadat pro jednotlivé aktivační události jsou popsané v článku na odpovídající odkaz. Příklad, naleznete v části [frontě aktivační události metadata](functions-bindings-storage-queue.md#trigger---message-metadata). Je také dostupná v dokumentaci **integrací** karta portálu v **dokumentace** části níže oblast konfigurace vazby.  

### <a name="binding-expressions---json-payloads"></a>Výrazy vazby - datové části JSON

Pokud je aktivační událost datové části JSON, najdete jeho vlastnosti v konfiguraci pro jiné vazby v stejné funkce a funkce kódu.

Následující příklad ukazuje *function.json* souboru webhooku funkce, která přijímá název objektu blob ve formátu JSON: `{"BlobName":"HelloWorld.txt"}`. Vstupní vazbu objektu Blob přečte objekt blob a HTTP Výstupní vazba vrátí obsah objektů blob v odpovědi HTTP. Všimněte si, že vstupní vazba Blob získá název objektu blob tím, že odkazuje přímo na `BlobName` vlastnost (`"path": "strings/{BlobName}"`)

```json
{
  "bindings": [
    {
      "name": "info",
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "genericJson"
    },
    {
      "name": "blobContents",
      "type": "blob",
      "direction": "in",
      "path": "strings/{BlobName.FileName}.{BlobName.Extension}",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

Tento postup práce v C# a F # musíte třídu, která definuje pole ke deserializovat, jako v následujícím příkladu:

```csharp
using System.Net;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    return req.CreateResponse(HttpStatusCode.OK, new {
        data = $"{blobContents}"
    });
}
```

V jazyce JavaScript je automaticky provést deserializaci JSON.

```javascript
module.exports = function (context, info) {
    if ('BlobName' in info) {
        context.res = {
            body: { 'data': context.bindings.blobContents }
        }
    }
    else {
        context.res = {
            status: 404
        };
    }
    context.done();
}
```

#### <a name="dot-notation"></a>Zápisu s tečkou

Pokud některé vlastnosti v vaše datové části JSON jsou objekty s vlastnostmi, mohou odkazovat na ty přímo pomocí zápisu s tečkou. Předpokládejme například, že vaše struktury JSON vypadá takto:

```json
{"BlobName": {
  "FileName":"HelloWorld",
  "Extension":"txt"
  }
}
```

Můžete se podívat přímo na `FileName` jako `BlobName.FileName`. Pomocí tohoto formátu JSON, zde je co `path` bude vypadat vlastnost v předchozím příkladu:

```json
"path": "strings/{BlobName.FileName}.{BlobName.Extension}",
```

V jazyce C# bude potřebovat dvě třídy:

```csharp
public class BlobInfo
{
    public BlobName BlobName { get; set; }
}
public class BlobName
{
    public string FileName { get; set; }
    public string Extension { get; set; }
}
```

### <a name="binding-expressions---create-guids"></a>Vytvoření vazby výrazy - identifikátory GUID

`{rand-guid}` Vazby výraz vytvoří identifikátor GUID. V následující cestě objektů blob `function.json` soubor vytvoří objekt blob s názvem, jako je *50710cb5-84b9 - 4d 87 9d 83-a03d6976a682.txt*.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="binding-expressions---current-time"></a>Výrazy vazby - aktuální čas

Výraz vazby `DateTime` přeloží na `DateTime.UtcNow`. V následující cestě objektů blob `function.json` soubor vytvoří objekt blob s názvem, jako je *2018-02-16T17-59-55Z.txt*.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="binding-at-runtime"></a>Vazba za běhu

V jazyce C# a jinými jazyky rozhraní .NET, můžete použít imperativní vazby vzoru oproti deklarativní vazeb v *function.json* a atributy. Imperativní vazba je užitečné, když vázané parametry muset počítaný v době běhu spíše než návrhu. Další informace najdete v tématu [C# referenční informace pro vývojáře](functions-dotnet-class-library.md#binding-at-runtime) nebo [C# skript referenční informace pro vývojáře](functions-reference-csharp.md#binding-at-runtime).

## <a name="functionjson-file-schema"></a>Schéma souboru Function.JSON

*Function.json* schéma souboru je k dispozici na [http://json.schemastore.org/function](http://json.schemastore.org/function).

## <a name="handling-binding-errors"></a>Zpracování chyb vazby

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

Odkazy na všechny relevantní chyby pro různé služby nepodporuje funkce, najdete v článku [vazby kódy chyb](functions-bindings-error-pages.md#binding-error-codes) části [zpracování chyb Azure Functions](functions-bindings-error-pages.md) přehledu.  

## <a name="next-steps"></a>Další postup

Další informace o konkrétní vazbu najdete v následujících článcích:

- [HTTP a webhooky](functions-bindings-http-webhook.md)
- [Časovač](functions-bindings-timer.md)
- [Queue Storage](functions-bindings-storage-queue.md)
- [Blob Storage](functions-bindings-storage-blob.md)
- [Table Storage](functions-bindings-storage-table.md)
- [Centrum událostí](functions-bindings-event-hubs.md)
- [Service Bus](functions-bindings-service-bus.md)
- [Azure Cosmos DB](functions-bindings-cosmosdb.md)
- [Microsoft Graph](functions-bindings-microsoft-graph.md)
- [SendGrid](functions-bindings-sendgrid.md)
- [Twilio](functions-bindings-twilio.md)
- [Notification Hubs](functions-bindings-notification-hubs.md)
- [Mobile Apps](functions-bindings-mobile-apps.md)
- [Externí soubor](functions-bindings-external-file.md)
