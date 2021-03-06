---
title: Služba LUIS předem připravených entit dimenze reference – Azure | Dokumentace Microsoftu
titleSuffix: Azure
description: Tento článek obsahuje dimenze informace předem připravených entit v Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: 3c923e7791c58255690100b04700577eb5c3f5dd
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/25/2018
ms.locfileid: "39237260"
---
# <a name="dimension-entity"></a>Entita Dimension
Různé typy dimenzí, bez ohledu na jazykovou verzi aplikace LUIS rozpozná dimenze předem připravených entit. Protože tato entita je už vytrénovaný, není potřeba přidat příklad projevy obsahující rozměry záměry aplikace. Dimenze entity se podporuje v [mnoha jazykových verzí](luis-reference-prebuilt-entities.md). 

## <a name="types-of-dimension"></a>Typy dimenze

Spravuje se z dimenze [rozpoznávání textu](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml) úložiště Github


## <a name="resolution-for-dimension-entity"></a>Řešení pro entity dimenze
Následující příklad ukazuje rozlišení **builtin.dimension** entity.

```JSON
{
  "query": "it takes more than 10 1/2 miles of cable and wire to hook it all up , and 23 computers.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.762141049
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.762141049
    }
  ],
  "entities": [
    {
      "entity": "10 1/2 miles",
      "type": "builtin.dimension",
      "startIndex": 19,
      "endIndex": 30,
      "resolution": {
        "unit": "Mile",
        "value": "10.5"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Další postup

Další informace o [e-mailu](luis-reference-prebuilt-email.md), [číslo](luis-reference-prebuilt-number.md), a [ordinální](luis-reference-prebuilt-ordinal.md) entity. 