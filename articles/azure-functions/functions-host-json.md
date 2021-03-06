---
title: referenční materiály k Host.JSON pro Azure Functions
description: Referenční dokumentace k host.json souboru Azure Functions.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 02/12/2018
ms.author: glenga
ms.openlocfilehash: 11bf136897b5d5b8140fc7ff1bb259c657a71921
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092186"
---
# <a name="hostjson-reference-for-azure-functions"></a>referenční materiály k Host.JSON pro Azure Functions

*Host.json* soubor metadat obsahuje možnosti globální konfigurace, které ovlivňují všechny funkce aplikace function App. Tento článek obsahuje seznam nastavení, které jsou k dispozici. Schéma JSON je na http://json.schemastore.org/host.

Existují další možnosti globální konfiguraci v [nastavení aplikace](functions-app-settings.md) a [local.settings.json](functions-run-local.md#local-settings-file) souboru.

## <a name="sample-hostjson-file"></a>Ukázkový soubor host.json

Následující ukázka *host.json* soubor obsahuje všechny zadané možnosti.

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    },
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    },
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    },
    "id": "9f4ea53c5136457d883d685e57164f08",
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    },
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    },
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    },
    "watchDirectories": [ "Shared" ],
}
```

Následující části tohoto článku popisují jednotlivé vlastnosti nejvyšší úrovně. Všechny jsou volitelné, pokud není uvedeno jinak.

## <a name="aggregator"></a>Agregátor

Určuje, kolik obsahující záznamy volání funkcí jsou při agregované [výpočet metrik pro službu Application Insights](functions-monitoring.md#configure-the-aggregator). 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Vlastnost |Výchozí  | Popis |
|---------|---------|---------| 
|batchSize|1000|Maximální počet požadavků, které k agregaci.| 
|flushTimeout|00:00:30|Maximální doba období agregace.| 

Volání funkce se agregují při první z nich omezuje se dosáhne.

## <a name="applicationinsights"></a>applicationInsights

Ovládací prvky [vzorkování funkcí ve službě Application Insights](functions-monitoring.md#configure-sampling).

```json
{
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|hodnotu isEnabled|true (pravda)|Povolí nebo zakáže vzorkování.| 
|maxTelemetryItemsPerSecond|5|Prahová hodnota, na které vzorkování začíná.| 

## <a name="durabletask"></a>durableTask

Nastavení konfigurace pro [Durable Functions](durable-functions-overview.md).

```json
{
  "durableTask": {
    "HubName": "MyTaskHub",
    "ControlQueueBatchSize": 32,
    "PartitionCount": 4,
    "ControlQueueVisibilityTimeout": "00:05:00",
    "WorkItemQueueVisibilityTimeout": "00:05:00",
    "MaxConcurrentActivityFunctions": 10,
    "MaxConcurrentOrchestratorFunctions": 10,
    "AzureStorageConnectionStringName": "AzureWebJobsStorage",
    "TraceInputsAndOutputs": false,
    "LogReplayEvents": false,
    "EventGridTopicEndpoint": "https://topic_name.westus2-1.eventgrid.azure.net/api/events",
    "EventGridKeySettingName":  "EventGridKey",
    "EventGridPublishRetryCount": 3,
    "EventGridPublishRetryInterval": "00:00:30"
  }
}
```

Centrum názvy úloh musí začínat písmenem a obsahovat jenom písmena a číslice. Pokud není zadán, výchozí název centra úloh aplikace function App je **DurableFunctionsHub**. Další informace najdete v tématu [úkolů rozbočovače](durable-functions-task-hubs.md).

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------|
|HubName|DurableFunctionsHub|Alternativní [centra úloh](durable-functions-task-hubs.md) názvy můžete použít k izolaci více aplikací Durable Functions od sebe navzájem, i v případě, že používají stejný back-endu úložiště.|
|ControlQueueBatchSize|32|Počet zpráv o přijetí změn z fronty ovládací prvek v čase.|
|PartitionCount |4|Počet oddílů pro frontu ovládacího prvku. Může být kladné celé číslo od 1 do 16.|
|ControlQueueVisibilityTimeout |5 minut|Časový limit viditelnosti ovládacího prvku vyřazených z fronty zpráv.|
|WorkItemQueueVisibilityTimeout |5 minut|Časový limit viditelnosti zpráv vyřazených z fronty pracovní položku.|
|MaxConcurrentActivityFunctions |10 × počet procesorů na aktuálním počítači|Maximální počet funkce aktivity, které je možné zpracovávat současně na jednom hostiteli instance.|
|MaxConcurrentOrchestratorFunctions |10 × počet procesorů na aktuálním počítači|Maximální počet funkce aktivity, které je možné zpracovávat současně na jednom hostiteli instance.|
|AzureStorageConnectionStringName |AzureWebJobsStorage|Název nastavení aplikace, které obsahuje připojovací řetězec služby Azure Storage používá ke správě základní prostředky služby Azure Storage.|
|TraceInputsAndOutputs |false (nepravda)|Hodnota označující, zda se pro sledování vstupů a výstupů volání funkce. Výchozí chování při trasování událostí spuštění funkce se zahrnou počet bajtů v serializovaném vstupy a výstupy pro volání funkce. To poskytuje minimální informace o vstupy a výstupy vypadat bez nadměrnému nárůstu velikosti protokolů nebo neúmyslně odhalují citlivé informace do protokolů. Nastavení této vlastnosti na hodnotu true způsobí, že je výchozí funkce protokolování do protokolu celý obsah vstupy a výstupy funkcí.|
|LogReplayEvents|false (nepravda)|Hodnota určující, jestli se má zapsat události opakování Orchestrace do Application Insights.|
|EventGridTopicEndpoint ||Adresa URL koncového bodu služby Azure Event Grid vlastního tématu. Pokud je tato vlastnost nastavena, Orchestrace životní cyklus oznámení události se publikují do tohoto koncového bodu. Tato vlastnost podporuje překlad nastavení aplikace.|
|EventGridKeySettingName ||Název nastavení aplikace, který obsahuje klíč používaný k ověřování pomocí Azure Event Grid vlastního tématu v `EventGridTopicEndpoint`.|
|EventGridPublishRetryCount|0|Počet pokusů o zopakování Pokud publikování do tématu Event gridu se nezdaří.|
|EventGridPublishRetryInterval|5 minut|Interval opakování v publikování služby Event Grid *hh: mm:* formátu.|

Mnoho z nich je pro optimalizaci výkonu. Další informace najdete v tématu [výkon a škálování](durable-functions-perf-and-scale.md).

## <a name="eventhub"></a>centra událostí

Nastavení konfigurace pro [centra Event aktivačními událostmi a vazbami](functions-bindings-event-hubs.md).

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="functions"></a>functions

Seznam funkcí, které se spustí úloha hostitele. Prázdné pole znamená, že spuštění všech funkcí. Určený k použití pouze tehdy, když [spuštěná místně](functions-run-local.md). V aplikace function App, použijte *function.json* `disabled` vlastnosti namísto této vlastnosti v *host.json*.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Určuje dobu trvání časového limitu pro všechny funkce. V plány Consumption platný rozsah je od 1 sekundy do 10 minut a výchozí hodnota je 5 minut. V plánech služby App Service neexistuje žádné omezení výchozí hodnota je null, což znamená žádný časový limit

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>healthMonitor

Nastavení konfigurace pro [monitorování stavu hostitelů](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor).

```
{
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|povoleno|true (pravda)|Určuje, zda je povolena funkce. | 
|healthCheckInterval|10 sekund|Časový interval mezi stavu na pozadí pravidelně kontroluje. | 
|healthCheckWindow|2 minut|Použít ve spojení s klouzavého časového období `healthCheckThreshold` nastavení.| 
|healthCheckThreshold|6|Maximální počet pokusů o kontrolu stavu může selhat, předtím, než je zahájeno recyklace hostitele.| 
|counterThreshold|0,80|Prahová hodnota, na které čítače výkonu se budou považovat za není v pořádku.| 

## <a name="http"></a>http

Nastavení konfigurace pro [http triggerů a vazeb](functions-bindings-http-webhook.md).

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="id"></a>id

Jedinečné ID pro úlohu hostitele. Mohou být malé písmeno identifikátor GUID s pomlčkami odebrány. Vyžadováno při místním spuštění. Při spuštění ve službě Azure Functions, je Pokud automaticky vygenerované ID `id` je vynechán.

Pokud sdílíte mezi více aplikací funkcí účet úložiště, ujistěte se, že každá aplikace function app má jinou `id`. Můžete vynechat `id` vlastnost nebo ručně nastavit vaší aplikace funkcí `id` na jinou hodnotu. Trigger časovače používá úložiště zámek k zajištění, že bude existovat pouze jedna instance časovač při aplikaci function app horizontálně navýší kapacitu na několik instancí. Pokud dvě aplikace function App sdílet stejný `id` a každý používá aktivaci časovačem, bude spuštěna pouze jedna časovače.


```json
{
    "id": "9f4ea53c5136457d883d685e57164f08"
}
```

## <a name="logger"></a>Protokolovací nástroj

Ovládací prvky filtrování protokolů autorem [objektu ILogger](functions-monitoring.md#write-logs-in-c-functions) nebo [context.log](functions-monitoring.md#write-logs-in-javascript-functions).

```json
{
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|categoryFilter|neuvedeno|Určuje filtrování podle kategorie| 
|defaultLevel|Informace|Pro všechny kategorie, není zadáno v `categoryLevels` pole, odeslat protokoly na této úrovni a novější do služby Application Insights.| 
|categoryLevels|neuvedeno|Pole kategorií, které určuje úroveň protokolu minimální k odeslání do Application Insights pro každou kategorii. Kategorie tady zadané, řídí všechny kategorie, které začínají stejnou hodnotu a delší hodnoty přednost. V předchozím příkladu *host.json* soubor, všechny kategorie, které začínají řetězcem "Host.Aggregator" protokolu v `Information` úroveň. Všechny kategorie, které začínají řetězcem "Hostitel", jako je například "Host.Executor" protokolu `Error` úroveň.| 

## <a name="queues"></a>fronty

Nastavení konfigurace pro [úložiště fronty aktivačními událostmi a vazbami](functions-bindings-storage-queue.md).

[!INCLUDE [functions-host-json-queues](../../includes/functions-host-json-queues.md)]

## <a name="servicebus"></a>služby Service Bus

Nastavení konfigurace pro [aktivační události služby Service Bus a vazby](functions-bindings-service-bus.md).

[!INCLUDE [functions-host-json-service-bus](../../includes/functions-host-json-service-bus.md)]

## <a name="singleton"></a>singleton

Nastavení konfigurace pro chování zámku typu Singleton. Další informace najdete v tématu [problém Githubu o podpoře typu singleton](https://github.com/Azure/azure-webjobs-sdk-script/issues/912).

```json
{
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|lockPeriod|00:00:15|Dobu, po kterou úroveň zámků funkce pořízeny pro. Zámky automatického obnovení.| 
|listenerLockPeriod|00:01:00|Období, ve kterém zamkne naslouchací proces pořízeny pro.| 
|listenerLockRecoveryPollingInterval|00:01:00|Časový interval použít pro obnovení zámku naslouchací proces, pokud při spuštění nebylo možné získat zámek naslouchací proces.| 
|lockAcquisitionTimeout|00:01:00|Maximální množství času se modul runtime pokusí se získat zámek.| 
|lockAcquisitionPollingInterval|neuvedeno|Interval mezi pokusy o získání zámku.| 

## <a name="tracing"></a>trasování

Nastavení konfigurace, které vytvoříte pomocí `TraceWriter` objektu. Zobrazit [jazyka C# protokolování](functions-reference-csharp.md#logging) a [Node.js protokolování](functions-reference-node.md#writing-trace-output-to-the-console). 

```json
{
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    }
}
```

|Vlastnost  |Výchozí | Popis |
|---------|---------|---------| 
|consoleLevel|informace|Úroveň trasování pro protokolování konzoly. Možnosti jsou: `off`, `error`, `warning`, `info`, a `verbose`.|
|fileLoggingMode|debugOnly|Úroveň trasování pro protokolování do souboru. Možnosti jsou `never`, `always`, `debugOnly`.| 

## <a name="watchdirectories"></a>watchDirectories

Sada [sdílených adresářů kód](functions-reference-csharp.md#watched-directories) , který je potřeba sledovat změny.  Zajišťuje, že při změně kódu v těchto adresářích, změny se prodlouží vašich funkcí.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Zjistěte, jak aktualizovat soubor host.json](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Podívejte se globální nastavení proměnné prostředí](functions-app-settings.md)
