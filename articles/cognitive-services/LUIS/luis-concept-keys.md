---
title: Principy klíčů LUIS – Azure | Dokumentace Microsoftu
description: Použití klíčů Language Understanding (LUIS) k vytváření vaší aplikace a vaše endpoing dotazování.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/23/2018
ms.author: diberry
ms.openlocfilehash: b40ca74999be1821ffa329224ff419646591960e
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225172"
---
# <a name="keys-in-luis"></a>Klíče v LUIS
Služba LUIS používá dva klíče: [vytváření](#programmatic-key) a [koncový bod](#endpoint-key). Vytváření klíč je vytvořen automaticky při vytvoření účtu služby LUIS. Až budete připraveni k publikování aplikace LUIS, budete muset [vytvořit klíče koncového bodu](luis-how-to-azure-subscription.md#create-luis-endpoint-key), [ji přiřadit](luis-how-to-manage-keys.md#assign-endpoint-key) do vaší aplikace LUIS a [pomocí dotazu koncový bod](#use-endpoint-key-in-query). 

|Klíč|Účel|
|--|--|
|[Klíč pro tvorbu](#programmatic-key)|Vytváření, publikování, správě spolupracovníkům, Správa verzí|
|[Klíč koncového bodu](#endpoint-key)| Dotazování|

Je důležité vytvářet aplikace LUIS v [oblastech](luis-reference-regions.md#publishing-regions) také místo k publikování a dotazování.

<a name="programmatic-key" ></a>
## <a name="authoring-key"></a>Klíč pro tvorbu

Vytváření klíč, označované také jako počáteční klíč, je vytvořen automaticky při vytvoření účtu služby LUIS a je zdarma. Máte jeden klíč pro vytváření obsahu napříč všemi vašimi aplikacemi LUIS pro každou vytváření [oblasti](luis-reference-regions.md). Vytváření klíč poskytuje vytvářet aplikace LUIS nebo testování dotazů koncový bod. 

Najít klíč pro vytváření, přihlaste se k [LUIS](luis-reference-regions.md#luis-website) a klikněte na název účtu v pravém horním navigačním panelu otevřete **nastavení účtu**.

![Klíč pro tvorbu](./media/luis-concept-keys/programatic-key.png)

Pokud chcete provést **produkční koncový bod dotazy**, vytvoření Azure [LUIS předplatné](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/). 

> [!CAUTION]
> Pro usnadnění práce z ukázky používat klíč pro tvorbu vzhledem k tomu, že poskytuje několik volání koncového bodu v jeho [kvóty](luis-boundaries.md#key-limits).  

## <a name="endpoint-key"></a>Klíč koncového bodu
 Pokud potřebujete **produkční koncový bod dotazy**, vytvořit [LUIS klíč](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) na webu Azure Portal. Mějte na paměti název používaný k vytvoření klíče, budete potřebovat při přidávání klíče do aplikace...

Po dokončení procesu předplatné LUIS [přidat klíč](luis-how-to-manage-keys.md#assign-endpoint-key) do aplikace na **publikovat** stránky. 

Klíč koncového bodu umožňuje kvótu přístupů do koncového bodu na základě plánu využití, které jste zadali při vytváření klíče. Zobrazit [ceny služby Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/?v=17.23h) informace o cenách.

Klíč koncového bodu je možné pro všechny vaše aplikace LUIS nebo pro konkrétní aplikace LUIS. 

Nepoužívejte klíče koncového bodu pro vytváření aplikací služby LUIS. 

## <a name="use-endpoint-key-in-query"></a>Použít klíče koncového bodu v dotazu
Koncový bod služby LUIS přijímá dva styly dotazu, používají klíče, ale koncový bod na různých místech:

|Příkaz|Příklad adresy url a klíč umístění|
|--|--|
|[GET](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn%20on%20the%20lights`<br><br>Hodnota řetězce dotazu pro `subscription-key`<br><br>Změnit hodnotu dotazu váš koncový bod pro `subscription-key` z vytváření klíč (starter), a nový klíč koncového bodu, chcete-li použít míra klíče kvót koncový bod služby LUIS. -Li vytvořit klíč a přiřadit klíč, ale neměňte hodnotu dotazu koncový bod pro klíč předplatného se, že nepoužíváte kvótu klíče koncového bodu.|
|[POST](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2`<br><br> Hodnota hlavičky pro `Ocp-Apim-Subscription-Key`<br><br>Změnit hodnotu dotazu váš koncový bod pro `Ocp-Apim-Subscription-Key` z vytváření klíč (starter), a nový klíč koncového bodu, chcete-li použít míra klíče kvót koncový bod služby LUIS. Pokud vytvoříte klíč a přiřadit klíč, ale neměňte hodnotu dotazu koncový bod pro `Ocp-Apim-Subscription-Key`, nepoužíváte kvótu klíče koncového bodu.|

ID aplikace použité v předchozím adresy URL, `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, je veřejná aplikace IoT používané k [interaktivní ukázku](https://azure.microsoft.com/en-us/services/cognitive-services/language-understanding-intelligent-service/). 

## <a name="api-usage-of-ocp-apim-subscription-key"></a>Použití rozhraní API z Ocp-Apim-Subscription-Key
Rozhraní LUIS API použít záhlaví `Ocp-Apim-Subscription-Key`. Název hlavičky nezmění podle která klíč a sadu rozhraní API, kterou používáte. Nastavte hlavičku na vytváření klíč rozhraní API pro vytváření. Pokud používáte koncový bod, nastavte hlavičku do klíče koncového bodu. 

Nelze předat klíč koncového bodu pro rozhraní API pro vytváření. Pokud tak učiníte, získáte chyba 401 - Přístup byl odmítnut neplatný koncový bod klíč. 

## <a name="key-limits"></a>Omezení klíčů
Zobrazit [klíče omezení](luis-boundaries.md#key-limits) a [oblastí Azure](luis-reference-regions.md). Vytváření klíč je zdarma a slouží pro vytváření obsahu. Klíč koncového bodu služby LUIS obsahuje volné vrstvy, ale musí být vytvořené a přidružené aplikace LUIS na **publikovat** stránky. Nelze použít pro vytváření obsahu, ale pouze dotazy koncový bod.

Publikování oblastech se liší od vytváření oblastí. Ujistěte se, že vytvoříte aplikaci pro vytváření oblasti odpovídajících k publikování oblast, kterou chcete.

## <a name="key-limit-errors"></a>Omezení klíče chyby
Při překročení vaší za druhé kvóty, obdržíte chybu HTTP 429. Při překročení vaší za měsíc kvóty, obdržíte chybu HTTP 403. Tyto chyby opravit tím, že získáme LUIS [koncový bod](#endpoint-key) klíč, [přiřazení](luis-how-to-manage-keys.md#assign-endpoint-key) klíč do aplikace na **publikovat** stránku [LUIS](luis-reference-regions.md#luis-website) webu.

## <a name="next-steps"></a>Další postup

* Přečtěte si [koncepty](luis-how-to-manage-keys.md#assign-endpoint-key) o klíčích pro vytváření a koncového bodu.