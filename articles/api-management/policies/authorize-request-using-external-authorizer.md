---
title: Zásady správy Azure API ukázkový – autorizace žádosti o pomocí externí prvek authorizer | Microsoft Docs
description: Azure API management zásad ukázka - ukazuje, jak autorizaci požadavků na používání externí prvek authorizer zapouzdřením logiku ověřování/autorizace vlastní nebo starší verze.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: apimpm
ms.openlocfilehash: 7d172a40f2aad65b595026fc656634060a1f7193
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284868"
---
# <a name="authorize-requests-using-external-authorizer"></a>Autorizaci požadavků na používání externí prvek authorizer

Tento článek ukazuje rozhraní API služby Azure správy zásad vzorku, který ukazuje, jak zabezpečit přístup pomocí rozhraní API pomocí externí prvek authorizer zapouzdřením logiku vlastní ověřování nebo autorizace. Chcete-li nastavit nebo upravit kód zásad, postupujte podle kroků popsaných v [sadu nebo upravit zásadu](../set-edit-policies.md). Další příklady najdete v sekci [ukázky zásad](../policy-samples.md).

## <a name="policy"></a>Zásada

Vložte kód do **příchozí** bloku.

[!code-xml[Main](../../../api-management-policy-samples/examples/Authorize requests using external authorizer.policy.xml)]

## <a name="next-steps"></a>Další postup

Další informace o zásadách APIM:

+ [Zásady omezení přístupu](../api-management-access-restriction-policies.md)
+ [Ukázky zásad](../policy-samples.md)