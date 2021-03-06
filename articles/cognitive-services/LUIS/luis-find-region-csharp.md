---
title: Najít oblast koncový bod pomocí jazyka C# v LUIS
titleSuffix: Azure Cognitive Services
description: Prostřednictvím kódu programu publikovat najít oblast s klíče koncového bodu a aplikace ID LUIS.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 7af58f141730557f103c61a6c591908cc7ec69d0
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057351"
---
# <a name="find-endpoint-region-with-c"></a>Najít oblast koncový bod pomocí jazyka C# 
Pokud máte LUIS ID aplikace a služby LUIS ID předplatného, můžete najít jaké oblasti se má použít pro koncový bod dotazy.

> [!NOTE] 
> Kompletní řešení C# je k dispozici [ **LUIS-Samples** úložiště Github](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/find-region/csharp/).

## <a name="luis-endpoint-query-strategy"></a>Strategie dotazu koncový bod služby LUIS
Každý dotaz LUIS koncového bodu vyžaduje:

* Klíč rozhraní koncového bodu
* ID aplikace
* Oblast

Pokud dotaz koncový bod služby LUIS používá správný koncový bod klíč a app ID ale nesprávné oblasti, je kód odpovědi 401. 401 požadavek se počítá směrem k kvóty předplatného. Tento požadavek proměňte strategie zaměřené na dotazování všech oblastech se najít správný oblast. Jediným požadavkem, který vrátí stavový kód 2xx je správný oblast. 

|Kód odpovědi|Parametry|
|--|--|
|2xx|klíč správný koncový bod<br>ID správné aplikace<br>oblast správného hostitele|
|401|klíč správný koncový bod<br>ID správné aplikace<br>_nesprávný_ oblasti hostitele|

## <a name="c-class-code-to-find-region"></a>Třída kódu jazyka C# se najít oblast
Konzolová aplikace přijímá ID aplikace LUIS a klíče koncového bodu a vrátí všechny oblasti s ním spojená. V současné době se vytvoří klíče rozhraní koncového bodu podle oblastí by měl vrátit tak pouze jedna oblast.

Zahrnout závislosti knihoven .net:

[!code-csharp[Add the dependencies](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=1-6 "Add the dependencies")]

Přidejte tuto vlastní třídu LUIS navržená tak, aby najít oblast. Nahraďte hodnoty proměnných pro `luisAppId` a `luisSubscriptionKey` vlastními hodnotami. Všechny oblasti, které vracejí 401, se zapíšou do konzolou pro ladění. 

[!code-csharp[Add the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=10-83 "Add the LUIS class")]

Toto je příklad volání vlastní třídu LUIS v metodu Main konzolové aplikace:

[!code-csharp[Call the LUIS class](~/samples-luis/documentation-samples/find-region/csharp/ConsoleAppLUISRegion/Program.cs?range=85-101 "Call the LUIS class")]

Při spuštění aplikace konzoly zobrazuje oblast pro ID aplikace.

![Snímek obrazovky zobrazující oblast LUIS aplikace konzoly](./media/find-region-csharp/console.png)

## <a name="next-steps"></a>Další postup

Další informace o LUIS [oblastech](luis-reference-regions.md).
