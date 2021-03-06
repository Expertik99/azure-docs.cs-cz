---
title: zahrnout soubor
description: zahrnout soubor
services: event-hubs
author: sethmanheim
ms.service: event-hubs
ms.topic: include
ms.date: 02/26/2018
ms.author: sethm
ms.custom: include file
ms.openlocfilehash: ab4c5b98ed9f6fcc8c271797db2d81dcc7ec4449
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38755612"
---
Následující tabulka uvádí kvóty a omezení na konkrétní [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Informace o cenách služby Event Hubs najdete v tématu [ceny služby Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

| Omezení | Rozsah | Poznámky | Hodnota |
| --- | --- | --- | --- | --- |
| Počet událostí centra na obor názvů |Obor názvů |Odeslání dalších žádostí o vytvoření nové Centrum událostí se budou odmítnuty. |10 |
| Počet oddílů na Centrum událostí |Entita |- |32 |
| Počet skupin uživatelů na Centrum událostí |Entita |- |20 |
| Počet připojení AMQP na obor názvů |Obor názvů |Odeslání dalších žádostí o další připojení budou odmítnuty a obdrží výjimku ve volajícím kódu. |5 000 |
| Maximální velikost události služby Event Hubs|Entita |- |256 kB |
| Maximální velikost název centra událostí |Entita |- |50 znaků. |
| Počet přijímačů bez epocha na skupinu uživatelů |Entita |- |5 |
| Maximální doba uchovávání dat událostí |Entita |- |1-7 dní |
| Maximální počet jednotek propustnosti |Obor názvů |Překročení omezení jednotek propustnosti způsobí, že se data omezí a generuje  **[ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)**. Můžete požádat o větší počet jednotek propustnosti pro standardní úrovně podání [žádost o podporu](/azure/azure-supportability/how-to-create-azure-support-request). [Další jednotky propustnosti](../articles/event-hubs/event-hubs-auto-inflate.md) jsou k dispozici v blocích po 20 na základě potvrzení nákupu. |20 |
| Počet ověřovacích pravidel na obor názvů |Obor názvů|Odeslání dalších žádostí o vytvoření pravidla autorizace budou odmítnuty.|12 |
