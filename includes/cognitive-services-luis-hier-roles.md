---
title: zahrnout soubor
description: zahrnout soubor
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: include
ms.custom: include file
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: dd99218765c873120c4117a3ec1712fe0a605e66
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2018
ms.locfileid: "39480366"
---
**Dotaz**: byste měli použít hierarchická entity nebo vzor s jednoduchou entitu s rolemi? 

**Odpověď**:, které závisí. Vzory a příklad projevy jsou srovnatelné, které představují utterance uživatele a jsou specifické pro záměru.  

Pokud projevy nemají vymazat vzor, používejte hierarchické entity. 

|hierarchické entity|Jednoduché entity s rolemi|
|--|--|
|musí mít projevy příklad s podřízené entity označené v záměry|Příklad projevy, musí mít **role nemohou být označeny v záměrů**|
|můžete použít ve vzorcích|**musíte** použít ve vzorcích|
|Možná bude nutné **Další** projevy příklad v záměr|Možná bude nutné ** méně projevy příklad v záměr|