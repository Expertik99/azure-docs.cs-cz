---
title: Automatickým přeposíláním entity pro zasílání zpráv Azure Service Bus | Dokumentace Microsoftu
description: Jak řetězit frontu služby Service Bus nebo předplatné do jiné fronty nebo tématu.
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: f7060778-3421-402c-97c7-735dbf6a61e8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2018
ms.author: spelluru
ms.openlocfilehash: 563fa6f38bb5baffb9a4ae86f944b7597d325d30
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/05/2018
ms.locfileid: "43698991"
---
# <a name="chaining-service-bus-entities-with-auto-forwarding"></a>Řetězení entit služby Service Bus s automatickým přeposíláním

Service Bus *automatickým přeposíláním* funkce vám umožní řetězit k fronty nebo odběru do jiné fronty nebo tématu, který je součástí stejný obor názvů. Pokud je automatické přeposílání povoleno, Service Bus automaticky odebere zprávy, které jsou umístěné v první frontě nebo odběru (zdroj) a vloží je do druhé fronty nebo tématu (cíl). Všimněte si, že je stále možné přímo odeslání zprávy do cílové entity. Kromě toho není možné zřetězit dílčí, jako jsou fronty nedoručených zpráv, do jiné fronty nebo tématu.

## <a name="using-auto-forwarding"></a>Pomocí automatickým přeposíláním

Můžete povolit automatickým přeposíláním nastavením [QueueDescription.ForwardTo] [ QueueDescription.ForwardTo] nebo [SubscriptionDescription.ForwardTo] [ SubscriptionDescription.ForwardTo] vlastnosti [QueueDescription] [ QueueDescription] nebo [SubscriptionDescription] [ SubscriptionDescription] objekty pro zdroj, stejně jako Následující příklad:

```csharp
SubscriptionDescription srcSubscription = new SubscriptionDescription (srcTopic, srcSubscriptionName);
srcSubscription.ForwardTo = destTopic;
namespaceManager.CreateSubscription(srcSubscription));
```

Cílová entita musí existovat v době, kdy se vytvoří zdrojové entity. Pokud Cílová entita neexistuje, vrátí služby Service Bus výjimku, když se zobrazí výzva k vytvoření zdrojové entity.

Automatickým přeposíláním můžete horizontálně navýšit kapacitu jednotlivých tématu. Omezení služby Service Bus [počet předplatných na dané téma](service-bus-quotas.md) do 2000. Další předplatná můžete přizpůsobit tak, že vytvoříte témata druhé úrovně. I v případě, že Service Bus omezení nejsou vázány na počet předplatných, přidává druhou úroveň témata zlepšují celkovou propustnost vašeho tématu.

![Scénář automatickým přeposíláním][0]

Můžete také použít automatickým přeposíláním oddělit odesílatelé zpráv od příjemce. Představte si třeba ERP systémem, který se skládá ze tří modulů: pořadí zpracování, řízení zásob a řízení vztahů se zákazníky. Každý z těchto modulů generuje zprávy, které jsou ve frontě do příslušné téma. Alice a Bob jsou obchodní zástupci, které zajímá všechny zprávy, které se vztahují na zákazníky. Pro příjem těchto zpráv, Alice a Bob vytvořit osobní fronty a předplatné na každé z témat ERP, které automaticky předávat všechny zprávy do jejich fronty.

![Scénář automatickým přeposíláním][1]

Pokud Alice přejde na dovolenou, své osobní fronty, nikoli tématu ERP, zaplní. V tomto scénáři protože obchodní zástupce nedostal žádné zprávy, žádná témata ERP někdy dosažení kvóty.

## <a name="auto-forwarding-considerations"></a>Důležité informace o automatickým přeposíláním

Pokud Cílová entita nahromadí příliš velkého počtu zpráv a překračuje kvótu nebo Cílová entita je zakázaná, zdrojové entitě přidá zprávy, které mají jeho [fronty nedoručených zpráv](service-bus-dead-letter-queues.md) až do místa v cílovém umístění (nebo entitu nebude znovu povoleno). Tyto zprávy se nadále live ve frontě nedoručených zpráv, takže se musí explicitně zobrazí a jejich zpracování z fronty nedoručených zpráv.

Při zřetězení jednotlivá témata k získání složené téma s mnoha předplatnými, doporučuje se, že máte střední počet odběrů na téma na první úrovni a mnoha předplatná na témata druhé úrovně. Například téma první úrovně 20 předplatná, každý z nich připojený k tématu druhé úrovně se 200 odběry, umožňuje vyšší propustnost než tématu první úrovně se 200 odběry, každý připojený k tématu druhé úrovně se 20 odběry.

Service Bus se účtuje jedna operace pro každou přesměrovaná zpráva. Například odeslání zprávy do tématu s 20 předplatnými, každý z nich nakonfigurovat tak, aby auto-forward zprávy do jiné fronty nebo tématu, se účtuje jako 21 operace Pokud všechna předplatná na první úrovni obdrží kopii zprávy.

Jak vytvoříte odběr, který je zřetězené do jiné fronty nebo tématu, musíte mít Tvůrce odběru **spravovat** oprávnění pro zdrojové i cílové entity. Odesílání zpráv do tématu zdroje vyžaduje pouze **odeslat** oprávnění k danému tématu zdroje.

## <a name="next-steps"></a>Další postup

Podrobné informace o automatickým přeposíláním najdete v následujících tématech:

* [ForwardTo][QueueDescription.ForwardTo]
* [QueueDescription][QueueDescription]
* [SubscriptionDescription][SubscriptionDescription]

Další informace o vylepšení výkonu služby Service Bus, najdete v článku 

* [Osvědčené postupy pro zlepšení výkonu pomocí zasílání zpráv Service Bus](service-bus-performance-improvements.md)
* [Segmentované entity zasílání zpráv][Partitioned messaging entities].

[QueueDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.forwardto#Microsoft_ServiceBus_Messaging_QueueDescription_ForwardTo
[SubscriptionDescription.ForwardTo]: /dotnet/api/microsoft.servicebus.messaging.subscriptiondescription.forwardto#Microsoft_ServiceBus_Messaging_SubscriptionDescription_ForwardTo
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[SubscriptionDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[0]: ./media/service-bus-auto-forwarding/IC628631.gif
[1]: ./media/service-bus-auto-forwarding/IC628632.gif
[Partitioned messaging entities]: service-bus-partitioning.md
