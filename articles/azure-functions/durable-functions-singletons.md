---
title: Jednotlivé prvky pro Durable Functions – Azure
description: Jak používat jednotlivé prvky v trvalé Functons rozšíření pro službu Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 8177fb6e8dea83ab2b8b12183cdcca6e43f92ade
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44095092"
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Jednotlivý prvek orchestrátorů v Durable Functions (Azure Functions)

Pro úlohy na pozadí nebo objektu actor – vizuální styl Orchestrace, je často potřeba zajistit, že pouze jedna instance byla najednou spuštěna konkrétní orchestrator. To můžete udělat [Durable Functions](durable-functions-overview.md) přiřazením konkrétní ID instance pro orchestrátor při jejím vytváření.

## <a name="singleton-example"></a>Příklad typu singleton

Následující příklad jazyka C# ukazuje funkci triggeru HTTP, která vytvoří Orchestrace úloh na pozadí typu singleton. Kód se zajistí, že existuje pouze jedna instance pro ID zadané instance.

```cs
[FunctionName("HttpStartSingle")]
public static async Task<HttpResponseMessage> RunSingle(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}/{instanceId}")] HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient starter,
    string functionName,
    string instanceId,
    TraceWriter log)
{
    // Check if an instance with the specified ID already exists.
    var existingInstance = await starter.GetStatusAsync(instanceId);
    if (existingInstance == null)
    {
        // An instance with the specified ID doesn't exist, create one.
        dynamic eventData = await req.Content.ReadAsAsync<object>();
        await starter.StartNewAsync(functionName, instanceId, eventData);
        log.Info($"Started orchestration with ID = '{instanceId}'.");
        return starter.CreateCheckStatusResponse(req, instanceId);
    }
    else
    {
        // An instance with the specified ID exists, don't create one.
        return req.CreateErrorResponse(
            HttpStatusCode.Conflict,
            $"An instance with ID '{instanceId}' already exists.");
    }
}
```

Ve výchozím nastavení instance, ID se náhodně generované identifikátory GUID. Ale v takovém případě je předané instance ID v datech trasy z adresy URL. Kód volá [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetStatusAsync_) ke kontrole, pokud je již spuštěna s zadané ID instance. Pokud ne, je vytvořena instance s číslem ID této.

> [!NOTE]
> V této ukázce je potenciální konflikt časování. Pokud se dvě instance **HttpStartSingle** být prováděna současně, výsledkem by bylo možné dva různé vytvořit instance typu singleton, jednu, která přepíše druhé. V závislosti na požadavcích může to mít nežádoucí vedlejší účinky. Z tohoto důvodu je důležité zajistit, že žádné dva požadavky mohou být tato funkce pro aktivaci prováděna současně.

Podrobnosti implementace funkce orchestrátoru není ve skutečnosti důležitá. Může to být funkce regulární orchestrátoru, který při spuštění a dokončení, nebo to může být ten, který spouští navždy (to znamená, [externí Orchestrace](durable-functions-eternal-orchestrations.md)). Důležité je, že existuje pouze někdy jedna instance spuštění najednou.

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Zjistěte, jak volat dílčí Orchestrace](durable-functions-sub-orchestrations.md)
