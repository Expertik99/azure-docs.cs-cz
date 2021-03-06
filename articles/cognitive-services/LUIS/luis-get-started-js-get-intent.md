---
title: Kurz volání aplikace LUIS (Language Understanding Intelligent Service) pomocí Node.js | Microsoft Docs
description: V tomto kurzu zjistíte, jak volat aplikaci LUIS pomocí Node.js.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 62c6816e753ee2c29aee0b8f68dec0ebd9f4be44
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265400"
---
# <a name="tutorial-call-a-luis-endpoint-using-javascript"></a>Kurz: Volání koncového bodu služby LUIS pomocí JavaScriptu
Do koncového bodu služby LUIS můžete předávat promluvy a získávat zpět záměr a entity.

<!-- green checkmark -->
> [!div class="checklist"]
> * Vytvoření předplatného LUIS a zkopírování hodnoty klíče pro pozdější použití
> * Zobrazení výsledků z koncového bodu služby LUIS ve veřejné ukázkové aplikaci IoT v prohlížeči
> * Vytvoření konzolové aplikace jazyka C# v sadě Visual Studio pro volání koncového bodu služby LUIS přes protokol HTTPS

Pro účely tohoto článku potřebujete bezplatný účet [LUIS][LUIS], abyste mohli vytvořit svou aplikaci LUIS.

## <a name="create-luis-subscription-key"></a>Vytvoření klíče předplatného LUIS
Abyste mohli volat ukázkovou aplikaci LUIS použitou v tomto návodu, potřebujete klíč rozhraní API služeb Cognitive Services. 

Klíč rozhraní API získáte následujícím způsobem: 

1. Nejprve musíte na webu Azure Portal vytvořit [účet rozhraní API služeb Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account). Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

2. Přihlaste se k webu Azure Portal na adrese https://portal.azure.com. 

3. Získejte klíč podle postupu v tématu věnovaném [vytváření klíčů předplatného pomocí Azure](./luis-how-to-azure-subscription.md).

4. Vraťte se na web [LUIS](luis-reference-regions.md) a přihlaste se pomocí svého účtu Azure. 

    [![](media/luis-get-started-node-get-intent/app-list.png "Snímek obrazovky se seznamem aplikací")](media/luis-get-started-node-get-intent/app-list.png)

## <a name="understand-what-luis-returns"></a>Vysvětlení toho, co služba LUIS vrací

Pokud chcete zjistit, co aplikace LUIS vrací, můžete vložit adresu URL ukázkové aplikace LUIS do okna prohlížeče. Ukázková aplikace je aplikace IoT, která rozpozná, jestli chce uživatel zapnout nebo vypnout světlo.

1. Koncový bod ukázkové aplikace má tento formát: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_API_KEY>&verbose=false&q=turn%20on%20the%20bedroom%20light` Zkopírujte adresu URL a nahraďte hodnotu v poli `subscription-key` klíčem vašeho předplatného.

2. Vložte adresu URL do okna prohlížeče a stiskněte Enter. V prohlížeči se zobrazí výsledek JSON, který značí, že služba LUIS rozpoznala záměr `HomeAutomation.TurnOn` a entitu `HomeAutomation.Room` s hodnotou `bedroom`.

    ![Výsledek JSON s rozpoznaným záměrem TurnOn (Zapnout)](./media/luis-get-started-node-get-intent/turn-on-bedroom.png)

3. Změňte hodnotu parametru `q=` v adrese URL na `turn off the living room light` a stiskněte Enter. Výsledek teď značí, že služba LUIS rozpoznala záměr `HomeAutomation.TurnOff` a entitu `HomeAutomation.Room` s hodnotou `living room`. 

    ![Výsledek JSON s rozpoznaným záměrem TurnOff (Vypnout)](./media/luis-get-started-node-get-intent/turn-off-living-room.png)


## <a name="consume-a-luis-result-using-the-endpoint-api-with-javascript"></a>Využití výsledku ze služby LUIS pomocí rozhraní API pro koncové body a JavaScriptu 

Pomocí JavaScriptu můžete získat přístup ke stejným výsledkům, jako jste viděli v okně prohlížeče v předchozím kroku. 
1. Zkopírujte následující kód a uložte ho do souboru HTML:

   [!code-javascript[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/endpoint-api-samples/javascript/call-endpoint.html)]
2. V následujícím řádku kódu nahraďte `"YOUR SUBSCRIPTION KEY"` klíčem vašeho předplatného: `xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key","YOUR SUBSCRIPTION KEY");`

3. Ve webovém prohlížeči otevřete soubor, který jste uložili.  Mělo by se zobrazit automaticky otevírané okno upozornění se zprávou `Detected the following intent: TurnOn`.

![Automaticky otevírané okno se zprávou TurnOn](./media/luis-get-started-node-get-intent/popup-turn-on.png)

## <a name="clean-up-resources"></a>Vyčištění prostředků
V tomto kurzu se vytvořily dva prostředky: klíč předplatného LUIS a projekt jazyka C#. Na webu Azure Portal odstraňte klíč předplatného LUIS. Zavřete projekt sady Visual Studio a odeberte příslušný adresář ze systému souborů. 

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Přidání projevů](luis-get-started-javascript-add-utterance.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website