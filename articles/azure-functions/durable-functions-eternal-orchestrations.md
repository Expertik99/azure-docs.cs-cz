---
title: Externí Orchestrace v Durable Functions – Azure
description: Zjistěte, jak implementovat externí Orchestrace pomocí rozšíření Durable Functions pro službu Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 98504534332b6faa7a7019aea9ab7b534d4c3faa
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44094435"
---
# <a name="eternal-orchestrations-in-durable-functions-azure-functions"></a>Externí Orchestrace v Durable Functions (Azure Functions)

*Externí Orchestrace* jsou funkce nástroje orchestrator, které nikdy ukončit. Jsou užitečné, pokud chcete použít [Durable Functions](durable-functions-overview.md) agregátorů a scénářích, které vyžadují nekonečnou smyčku.

## <a name="orchestration-history"></a>Historie Orchestrace

Jak je vysvětleno v [vytváření kontrolních bodů a opětovného přehrání](durable-functions-checkpointing-and-replay.md), trvalý Framework úloh uchovává informace o historii Orchestrace funkce. Tato historie roste nepřetržitě, dokud funkce orchestrátoru nadále naplánovat novou práci. Pokud funkce orchestrátoru přejde do nekonečné smyčky a průběžně plánuje práci, může tato historie růst kriticky velké a způsobit významné problémy s výkonem. *Externí Orchestrace* koncept je navržená pro zmírnění těchto typů problémů pro aplikace, které potřebují nekonečné smyčky.

## <a name="resetting-and-restarting"></a>Obnovení a restartování

Namísto použití nekonečné smyčky, funkce orchestrátoru obnovovat jejich stav voláním [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) metody. Tato metoda přebírá jediný parametr serializovat na JSON, který bude nový vstup pro další generaci funkce nástroje orchestrator.

Když `ContinueAsNew` nazývá instance zařadí zprávu na sebe sama předtím, než ho ukončí. Zpráva restartuje instanci s novou vstupní hodnotu. Zůstane stejné ID instance, ale funkce orchestrátoru historie efektivně zkrácen.

> [!NOTE]
> Trvalý Framework úkolů udržuje stejné ID instance, ale interně vytvoří novou *ID spuštění* pro funkce orchestrátoru, který získá resetováno `ContinueAsNew`. Toto ID spuštění není obecně dostupná externě, ale může být užitečné při ladění provádění Orchestrace vědět o.

> [!NOTE]
> `ContinueAsNew` Metoda ještě není k dispozici v jazyce JavaScript.

## <a name="periodic-work-example"></a>Příklad pravidelné práce

Jeden případ použití pro externí Orchestrace je kód, který je potřeba pravidelně práci po neomezenou dobu.

```csharp
[FunctionName("Periodic_Cleanup_Loop")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    await context.CallActivityAsync("DoCleanup");

    // sleep for one hour between cleanups
    DateTime nextCleanup = context.CurrentUtcDateTime.AddHours(1);
    await context.CreateTimer(nextCleanup, CancellationToken.None);

    context.ContinueAsNew(null);
}
```

Rozdíl mezi v tomto příkladu a funkce aktivované pomocí časovače je, že vyčištění aktivační událost časy zde nejsou na základě plánu. Můžete třeba plán CRON, který se spustí funkci každou hodinu se spustit ho v 1:00, 2:00, 3:00 atd. a může potenciálně narazíte na problémy překrývají. V tomto příkladu, ale pokud čištění trvá 30 minut, pak ho bude naplánována v 1:00, 2:30, 4:00 atd. a neexistuje možnost překrytí.

## <a name="exit-from-an-eternal-orchestration"></a>Výstup z externí Orchestrace

Pokud funkce orchestrátoru musí nakonec dokončí, je všechno, co potřebujete udělat *není* volání `ContinueAsNew` a nechat funkci ukončíte.

Pokud funkce orchestrátoru je v nekonečné smyčce a musí zastavit, použijte [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) metoda zastavte ji. Další informace najdete v tématu [Správa instancí](durable-functions-instance-management.md).

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Zjistěte, jak implementovat Orchestrace s jedním prvkem](durable-functions-singletons.md)
