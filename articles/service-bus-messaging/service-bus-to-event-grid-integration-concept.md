---
title: "Přehled integrace služby Azure Service Bus do služby Event Grid | Microsoft Docs"
description: "Popis integrace zasílání zpráv služby Service Bus a služby Event Grid"
services: service-bus-messaging
documentationcenter: .net
author: ChristianWolf42
manager: timlt
editor: 
ms.assetid: f99766cb-8f4b-4baf-b061-4b1e2ae570e4
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: get-started-article
ms.date: 02/15/2018
ms.author: chwolf
ms.openlocfilehash: bf771428505081cb60ca4417f87a4f6c2afbd25d
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/24/2018
---
# <a name="azure-service-bus-to-azure-event-grid-integration-overview"></a>Přehled integrace služby Azure Service Bus do služby Azure Event Grid

Služba Azure Service Bus spustila novou integraci do služby Azure Event Grid. Klíčovým scénářem, který tato funkce umožňuje, je, že se příjemce nemusí neustále dotazovat na zprávy z front nebo odběrů služby Service Bus, které obsahují malé množství zpráv. V případě, že fronta nebo odběr obsahuje zprávy a nejsou k dispozici žádní příjemci, teď může Service Bus vysílat události do služby Azure Event Grid. Pro obory názvů služby Service Bus můžete vytvářet odběry služby Azure Event Grid, které naslouchají těmto událostem a reagují na ně spuštěním příjemce. Díky této funkci teď můžete službu Service Bus využívat v reaktivních programovacích modelech.

K povolení této funkce potřebujete následující:

* Obor názvů služby Azure Service Bus úrovně Premium alespoň s jednou frontou služby Service Bus nebo jedním tématem služby Service Bus s alespoň jedním oděrem.
* Přístup přispěvatele k tomuto oboru názvů služby Azure Service Bus.
* Kromě toho potřebujete odběr služby Azure Event Grid pro tento obor názvů služby Service Bus. Tento odběr přijímá ze služby Azure Event Grid oznámení o zprávách čekajících na vyzvednutí. Typickými odběrateli můžou být služby Logic Apps a Azure Functions nebo webhook kontaktující webovou aplikaci, která pak zprávy zpracuje. 

![19][]

### <a name="verify-that-you-have-contributor-access"></a>Ověření, že máte přístup přispěvatele

Přejděte do svého oboru názvů služby Service Bus a vyberte Řízení přístupu (IAM), jak je znázorněno níže:

![1][]

### <a name="events-and-event-schemas"></a>Události a schémata událostí

Azure Service Bus v současné době odesílá události pro dva scénáře.

* [ActiveMessagesWithNoListenersAvailable](#active-messages-available-event)
* [DeadletterMessagesAvailable](#dead-lettered-messages-available-event)

Kromě toho využívá standardní [mechanismy ověřování](https://docs.microsoft.com/en-us/azure/event-grid/security-authentication) a zabezpečení služby Azure Event Grid.

Další informace o schématech událostí služby Event Grid najdete na [tomto](https://docs.microsoft.com/en-us/azure/event-grid/event-schema) odkazu.

#### <a name="active-messages-available-event"></a>Událost Active Messages Available (Jsou k dispozici aktivní zprávy)

Tato událost se generuje v případě, že ve frontě nebo odběru máte aktivní zprávy, ale žádný příjemce nenaslouchá.

Schéma této události je následující:

```JSON
{
  "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}
```

#### <a name="dead-lettered-messages-available-event"></a>Událost Dead Lettered Messages Available (Jsou k dispozici nedoručené zprávy)

Obdržíte alespoň jednu událost za každou frontu nedoručených zpráv, která obsahuje zprávy, ale nemá žádné aktivní příjemce.

Schéma této události je následující:

```JSON
[{
  "topic": "/subscriptions/<subscription id>/resourcegroups/DemoGroup/providers/Microsoft.ServiceBus/namespaces/<YOUR SERVICE BUS NAMESPACE WILL SHOW HERE>",
  "subject": "topics/<service bus topic>/subscriptions/<service bus subscription>",
  "eventType": "Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://YOUR-SERVICE-BUS-NAMESPACE-WILL-SHOW-HERE.servicebus.windows.net/TOPIC-NAME/subscriptions/SUBSCRIPTIONNAME/$deadletterqueue/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="how-often-and-how-many-events-are-emitted"></a>Jak často a v jakém množství se vysílají události?

Pokud v oboru názvů máte více front a témat nebo odběrů, obdržíte alespoň jednu událost za každou frontu a jednu událost za každý odběr. Události se vysílají ihned poté, co entita služby Service Bus neobsahující žádné zprávy přijme novou zprávu, nebo každé dvě minuty, pokud Azure Service Bus nezjistí aktivního příjemce. Procházení zpráv tyto události nepřeruší.

Ve výchozím nastavení Azure Service Bus vysílá události pro všechny entity v příslušném oboru názvů. Pokud chcete přijímat pouze události pro konkrétní entity, přečtěte si následující část věnovanou filtrování.

### <a name="filtering-limiting-from-where-you-get-events"></a>Filtrování a omezování zdroje přijímaných událostí

Pokud chcete přijímat události například pouze pro jednu frontu nebo jeden odběr v rámci vašeho oboru názvů, můžete k tomu použít filtry Začíná na a Končí na, které poskytuje Azure Event Grid. V některých rozhraních se tyto filtry označují jako Předpona a Přípona. Pokud chcete přijímat události pro více front a odběrů, ale ne pro všechny, můžete vytvořit více různých odběrů služby Azure Event Grid a pro každý z nich zadat filtr.

## <a name="how-to-create-azure-event-grid-subscriptions-for-service-bus-namespaces"></a>Vytváření odběrů služby Azure Event Grid pro obory názvů služby Service Bus

Odběry služby Event Grid pro obory názvů služby Service Bus můžete vytvořit třemi různými způsoby.

* [Azure Portal](#portal-instructions)
* [Azure CLI](#azure-cli-instructions)
* [PowerShell](#powershell-instructions)

## <a name="portal-instructions"></a>Pokyny pro portál

Pokud chcete vytvořit nový odběr služby Azure Event Grid, na webu Azure Portal přejděte do vašeho oboru názvů a vyberte okno Event Grid. Klikněte na + Odběr události. Níže se zobrazí obor názvů, který již obsahuje několik odběrů služby Event Grid.

![20][]

Následující snímek obrazovky ukazuje vytvoření odběru funkce Azure nebo webhooku bez konkrétního filtrování:

![21][]

## <a name="azure-cli-instructions"></a>Pokyny pro Azure CLI

Nejprve se ujistěte, že máte nainstalovanou alespoň verzi Azure CLI 2.0. Instalační program si můžete stáhnout odsud. Pak stiskněte Windows + X a otevřete novou konzolu PowerShellu s oprávněními správce. Případně může použít také příkazové prostředí na webu Azure Portal.

Spusťte následující kód:

```PowerShell
Az login

Aa account set -s “THE SUBSCRIPTION YOU WANT TO USE”

$namespaceid=(az resource show --namespace Microsoft.ServiceBus --resource-type namespaces --name “<yourNamespace>“--resource-group “<Your Resource Group Name>” --query id --output tsv)

az eventgrid event-subscription create --resource-id $namespaceid --name “<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>” --endpoint “<your_function_url>” --subject-ends-with “<YOUR SERVICE BUS SUBSCRIPTION NAME>”
```

## <a name="powershell-instructions"></a>Pokyny pro PowerShell

Ujistěte se, že máte nainstalovaný Azure PowerShell. Najdete ho tady. Pak stiskněte Windows + X a otevřete novou konzolu PowerShellu s oprávněními správce. Případně může použít také příkazové prostředí na webu Azure Portal.

```PowerShell
Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionName "<YOUR SUBSCRIPTION NAME>"

# This might be installed already
Install-Module AzureRM.ServiceBus

$NSID = (Get-AzureRmServiceBusNamespace -ResourceGroupName "<YOUR RESOURCE GROUP NAME>" -Na
mespaceName "<YOUR NAMESPACE NAME>").Id 

New-AzureRmEVentGridSubscription -EventSubscriptionName “<YOUR EVENT GRID SUBSCRIPTION NAME (CAN BE ANY NOT EXISTING)>” -ResourceId $NSID -Endpoint "<YOUR FUNCTION URL>” -SubjectEndsWith “<YOUR SERVICE BUS SUBSCRIPTION NAME>”
```

Teď můžete prozkoumat další možnosti instalace nebo [otestovat tok událostí](#test-that-events-are-flowing).

## <a name="next-steps"></a>Další kroky

* [Příklady](service-bus-to-event-grid-integration-example.md) pro službu Service Bus a Event Grid.
* Další informace o službě [Azure Event Grid](https://docs.microsoft.com/en-us/azure/azure-functions/).
* Další informace o službě [Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/).
* Další informace o službě [Azure Log Apps](https://docs.microsoft.com/en-us/azure/logic-apps/).
* Další informace o službě [Azure Service Bus](https://docs.microsoft.com/en-us/azure/azure-functions/).

[1]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgrid1.png
[19]: ./media/service-bus-to-event-grid-integration-concept/sbtoeventgriddiagram.png
[8]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid8.png
[9]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgrid9.png
[20]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal.png
[21]: ./media/service-bus-to-event-grid-integration-example/sbtoeventgridportal2.png