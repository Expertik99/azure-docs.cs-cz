---
title: Správa instancí v Durable Functions – Azure
description: Další informace o správě instance v rozšíření Durable Functions pro službu Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: azfuncdf
ms.openlocfilehash: 72ea5e54bf86ce408700c0456f6d37f5f3c29924
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44091778"
---
# <a name="manage-instances-in-durable-functions-azure-functions"></a>Správa instancí v Durable Functions (Azure Functions)

[Odolná služba Functions](durable-functions-overview.md) Orchestrace instance je možné spustit, byla ukončena, dotazovat a odeslána oznámení události. Všechny instance Správa se provádí pomocí [klient Orchestrace vazby](durable-functions-bindings.md). Tento článek probírá do podrobnosti o jednotlivých operacích správy instance.

## <a name="starting-instances"></a>Spouští se instance

[StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) metodu [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) spustí novou instanci třídy funkce orchestrátoru. Instance této třídy lze získat pomocí `orchestrationClient` vazby. Interně tato to metoda zařadí zprávu do fronty, ovládací prvek, který potom aktivuje spuštění funkce se zadaným názvem, který používá `orchestrationTrigger` aktivovat vazby. 

Dokončení úlohy při spuštění procesu Orchestrace. Během 30 sekund se měl spustit proces Orchestrace. Pokud to trvá déle, `TimeoutException` je vyvolána výjimka. 

Parametry tak, aby [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) jsou následující:

* **Název**: název funkce orchestrátoru, který chcete naplánovat.
* **Vstupní**: JSON serializovat data, která mají být předány jako vstup do funkce orchestrátoru.
* **InstanceId**: (volitelné) jedinečné ID instance. Pokud není zadaný, vygeneruje se náhodný instance ID.

Tady je jednoduchý příklad jazyka C#:

```csharp
[FunctionName("HelloWorldManualStart")]
public static async Task Run(
    [ManualTrigger] string input,
    [OrchestrationClient] DurableOrchestrationClient starter,
    TraceWriter log)
{
    string instanceId = await starter.StartNewAsync("HelloWorld", input);
    log.Info($"Started orchestration with ID = '{instanceId}'.");
}
```

Pro jazyky bez .NET funkci výstupní vazbu je možné spustit také nové instance. V takovém případě můžete použít libovolný JSON serializovatelný objekt, který má výše uvedených tří parametrů jako pole. Představte si třeba následující funkce jazyka JavaScript:

```js
module.exports = function (context, input) {
    var id = generateSomeUniqueId();
    context.bindings.starter = [{
        FunctionName: "HelloWorld",
        Input: input,
        InstanceId: id
    }];

    context.done(null);
};
```

> [!NOTE]
> Doporučujeme použít identifikátor náhodné pro ID instance. To vám pomůže zajistit distribuci zatížení pomocí stejné při horizontálním škálování funkcí nástroje orchestrator na víc virtuálních počítačů. Správný čas použití ID nenáhodný instancí je při ID musí pocházet z externího zdroje nebo při implementaci [singleton orchestrator](durable-functions-singletons.md) vzor.

## <a name="querying-instances"></a>Dotazování instancí

[GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) metodu [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) třída dotazuje se na stav instance Orchestrace. Trvá, než `instanceId` (povinné), `showHistory` (volitelné), a `showHistoryOutput` (volitelné) jako parametry. Pokud `showHistory` je nastavena na `true`, odpověď bude obsahovat historie spuštění. Pokud `showHistoryOutput` je nastavena na `true` , historie spouštění bude obsahovat výstupů aktivity. Metoda vrátí objekt s následujícími vlastnostmi:

* **Název**: název funkce nástroje orchestrator.
* **InstanceId**: ID instance orchestraci (by měl být stejný jako `instanceId` vstupu).
* **Čas vytvoření**: čas, ve kterém funkce nástroje orchestrator spuštěn.
* **LastUpdatedTime**: čas, kdy orchestraci poslední kontrolní bod.
* **Vstupní**: vstup funkce jako hodnotu JSON.
* **CustomStatus**: Stav vlastní Orchestrace ve formátu JSON. 
* **Výstup**: výstupní funkci jako hodnotu JSON (je-li funkci dokončí). Pokud selže funkce orchestrátoru, tato vlastnost bude obsahovat podrobnosti o chybě. Funkce orchestrátoru bylo ukončeno, tato vlastnost zahrne zadaný důvod ukončení (pokud existuje).
* **RuntimeStatus**: jeden z následujících hodnot:
    * **Čekající**: instance je naplánovaná, ale nebyla dosud spuštěna.
    * **Spuštění**: instance byla spuštěna.
    * **Dokončení**: instance byla dokončena normálně.
    * **ContinuedAsNew**: instance samotného s novou historie restartování. Jedná se o přechodný stav.
    * **Nepovedlo**: instance se nezdařilo s chybou.
    * **Byla ukončena**: instance se ukončí.
* **Historie**: historie spouštění orchestraci. Toto pole je vyplněný, pouze když `showHistory` je nastavena na `true`.
    
Tato metoda vrátí `null` Pokud instance neexistuje nebo nebyl dosud spuštěn.

```csharp
[FunctionName("GetStatus")]
public static async Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    var status = await client.GetStatusAsync(instanceId);
    // do something based on the current status.
}
```
## <a name="querying-all-instances"></a>Dotazování na všechny instance

Můžete použít `GetStatusAsync` metoda k dotazování stavy všech instancí Orchestrace. Nepřijímá žádné parametry, nebo můžete předat `CancellationToken` objektu v případě, že chcete ho zrušit. Metoda vrátí objekty se stejnými vlastnostmi, jako `GetStatusAsync` metodu s parametry s výjimkou ho nevrací historie. 

```csharp
[FunctionName("GetAllStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    TraceWriter log)
{
    IList<DurableOrchestrationStatus> instances = await starter.GetStatusAsync(); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.Info(JsonConvert.SerializeObject(instance));
    };
}
```

## <a name="terminating-instances"></a>Ukončení instance

Spuštěné instance Orchestrace lze ukončit pomocí [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) metodu [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) třídy. Jsou dva parametry `instanceId` a `reason` řetězec, který se zapisují do protokolů a stav instance. Zastavené instance se zastaví ihned poté, co dosáhne Další `await` bodu, nebo pokud dojde k ukončení okamžitě je již na `await`. 

```csharp
[FunctionName("TerminateInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "It was time to be done.";
    return client.TerminateAsync(instanceId, reason);
}
```

> [!NOTE]
> Ukončení instance nešíří aktuálně. Funkce aktivity a dílčí Orchestrace poběží až do ukončení bez ohledu na to, zda byl ukončen Orchestrace instanci, která je volána.

## <a name="sending-events-to-instances"></a>Odesílání událostí do instance

Je možné odeslat oznámení události spuštěné instance pomocí [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) metodu [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) třídy. Instance, které dokáže zpracovat tyto události jsou ty, které čekají na volání [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_). 

Parametry tak, aby [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) jsou následující:

* **InstanceId**: Jedinečný ID instance.
* **EventName**: název události k odeslání.
* **EventData**: JSON serializovat datovou část k odeslání do instance.

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

[FunctionName("RaiseEvent")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    int[] eventData = new int[] { 1, 2, 3 };
    return client.RaiseEventAsync(instanceId, "MyEvent", eventData);
}
```

> [!WARNING]
> Pokud neexistuje žádná instance orchestration se zadaným *instance ID* nebo pokud není instance čeká na zadaný *název události*, zpráva o události se zahodí. Další informace o tomto chování najdete v tématu [problém Githubu](https://github.com/Azure/azure-functions-durable-extension/issues/29).

## <a name="wait-for-orchestration-completion"></a>Počkat na dokončení Orchestrace

[DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) třídy zpřístupňuje [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_) rozhraní API, které lze použít k získání synchronně skutečné výstupu z instance Orchestrace. Metoda používá výchozí hodnota 10 sekund, `timeout` a 1 sekundu `retryInterval` Pokud nejsou nastavené.  

Tady je příklad funkce triggeru HTTP, který ukazuje, jak pomocí tohoto rozhraní API:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

Funkci lze volat pomocí 2 sekund časového limitu a interval opakování 0,5 druhé tento řádek:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

V závislosti na čas potřebný k získání odpovědi z instance Orchestrace existují dva možné případy:

1. Instance Orchestrace dokončit během definovaný časový limit (v tomto případě 2 sekundy), odpověď je výstup instance skutečné Orchestrace synchronně doručení:

    ```http
        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:14:29 GMT
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ]
    ```

2. Instance Orchestrace nelze dokončit v rámci definovaný časový limit (v tomto případě 2 sekundy), odpověď je výchozí, jeden je popsáno v **zjišťování adresy URL rozhraní API HTTP**:

    ```http
        HTTP/1.1 202 Accepted
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:13:51 GMT
        Location: http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}
        Retry-After: 10
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        {
            "id": "d3b72dddefce4e758d92f4d411567177",
            "sendEventPostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/raiseEvent/{eventName}?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "statusQueryGetUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "terminatePostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/terminate?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}"
        }
    ```

> [!NOTE]
> Formát adresy URL webhooku se může lišit v závislosti na tom, které verze hostitelů Azure Functions, kterou používáte. V předchozím příkladu je pro Azure Functions 2.0 hostitele.

## <a name="retrieving-http-management-webhook-urls"></a>Načítání adresy URL Webhooku správy protokolu HTTP

Externí systémy mohou komunikovat s Durable Functions prostřednictvím adresy URL webhooku, které jsou součástí výchozí odpověď je popsáno v [zjišťování adresy URL rozhraní API HTTP](durable-functions-http-api.md). Ale adresy URL webhooku také je možné programově přistupovat v klientovi Orchestrace nebo v činnosti funkci prostřednictvím [CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) metodu [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)třídy. 

[CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) má jeden parametr:

* **InstanceId**: Jedinečný ID instance.

Metoda vrátí instanci [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) s následujícími vlastnostmi řetězec:

* **ID**: ID instance orchestraci (by měl být stejný jako `InstanceId` vstupu).
* **StatusQueryGetUri**: adresa URL stavu instance Orchestrace.
* **SendEventPostUri**: "vyvolat událost" adresa URL instance Orchestrace.
* **TerminatePostUri**: "ukončit" adresa URL instance Orchestrace.

Aktivita funkce můžete odeslat instanci [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) k externím systémům ke sledování nebo vyvolat události na Orchestrace:

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static void SendInstanceInfo(
    [ActivityTrigger] DurableActivityContext ctx,
    [OrchestrationClient] DurableOrchestrationClient client,
    [DocumentDB(
        databaseName: "MonitorDB",
        collectionName: "HttpManagementPayloads",
        ConnectionStringSetting = "CosmosDBConnection")]out dynamic document)
{
    HttpManagementPayload payload = client.CreateHttpManagementPayload(ctx.InstanceId);

    // send the payload to Cosmos DB
    document = new { Payload = payload, id = ctx.InstanceId };
}
```

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Další informace o použití protokolu HTTP rozhraní API pro správu](durable-functions-http-api.md)
