---
title: Ukázky v Azure Event Hubs | Dokumentace Microsoftu
description: Ukázky v Azure Event Hubs
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2018
ms.author: shvija
ms.openlocfilehash: fbde6e5a5ed053d6c151b3af25535c397a496ef4
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005330"
---
# <a name="event-hubs-samples"></a>Ukázky služby Event Hubs 
Ukázky služby Event Hubs můžete najít na [Githubu](https://github.com/Azure/azure-event-hubs/tree/master/samples). Tyto ukázky ukazují klíčové funkce v [Azure Event Hubs](/azure/event-hubs/). Tento článek slouží ke kategorizaci a popisuje ukázek dostupných s odkazy na každý.

## <a name="net-samples"></a>Ukázky .NET

| Ukázkový název | Popis | 
| ----------- | ----------- | 
| [SampleSender](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) | Tento příklad ukazuje, jak napsat konzolovou aplikaci .NET Core, který odesílá události do centra událostí. |
| [SampleEHReceiver](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) | Tento příklad ukazuje, jak psát aplikace konzoly .NET Core, která přijímá sadu událostí z centra událostí pomocí knihovny Event Processor Host.  | 

## <a name="java-samples"></a>Ukázky Java

| Ukázkový název | Popis | 
| ----------- | ----------- | 
| [SendBatch](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SendBatch)  | Tento příklad ukazuje, jak ingestování dávky události do vašeho centra událostí. | 
| [SimpleSend](https://github.com/Azure/azure-event-hubs/tree/master/samples/Java/Basic/SimpleSend) | Tento příklad ukazuje, jak pro zpracování příjmu událostí do centra událostí. |
| [AdvanceSendOptions](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/AdvancedSendOptions) | Tento příklad ukazuje různé možnosti k dispozici s Event Hubs k ingestování událostí. |
| [ReceiveByDateTime](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveByDateTime) | Tento příklad ukazuje, jak přijímat události z oddílu centra událostí pomocí konkrétní posun data a času. |
| [ReceiveUsingOffset](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveUsingOffset) | Tento příklad ukazuje, jak přijímat události z oddílu centra událostí pomocí specifických dat posun. |  
| [ReceiveUsingSequenceNumber](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/ReceiveUsingSequenceNumber) | Tento příklad ukazuje, jak můžou přijímat z oddílů centra událostí pomocí pořadové číslo. |   
| [EventProcessorSample](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Basic/EventProcessorSample) |Tento příklad ukazuje, jak přijímat události z centra událostí pomocí třídy event processor host, který poskytuje automatické oddílu výběru a převzetí služeb při selhání mezi několik příjemců souběžných. | 
| [AutoScaleOnIngress](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Benchmarks/AutoScaleOnIngress) | Tento příklad ukazuje, jak v Centru událostí automaticky vertikálně navýšit kapacitu na vysoké zatížení. Ukázka bude odesílat události s rychlostí, které právě překračují nakonfigurované frekvence centra událostí, způsobí vertikálně navýšit kapacitu centra událostí. | 
| [IngressBenchmark](https://github.com/Azure/azure-event-hubs/blob/master/samples/Java/Benchmarks/IngressBenchmark) | Tato ukázka umožňuje měřit rychlost příchozího přenosu dat. | 

## <a name="next-steps"></a>Další postup
Dozvíte víc o službě Event Hubs v následujících článcích:

- [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
- [Funkce Event Hubs](event-hubs-features.md)
- [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)