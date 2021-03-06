---
title: Zkontrolujte projevy koncový bod pro Language Understanding (LUIS)
titleSuffix: Azure Cognitive Services
description: Převratné funkce ze služby LUIS je koncept aktivně učit. Jakmile vaše LUIS dotazy koncový bod, active learning kvality výsledků zlepšuje vybere projevy, které je jistí, jaké. Pokud označíte popiskem těchto projevů, trénování a publikování, pak LUIS identifikuje projevy přesněji.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 3ec791d534fb73a9d88f2dcdb81e445d6c26ab69
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44057348"
---
# <a name="review-endpoint-utterances"></a>Kontrola promluv koncového bodu

Převratné funkce ze služby LUIS je [koncept](luis-concept-review-endpoint-utterances.md) aktivní učení. Jakmile vaše LUIS dotazy koncový bod, LUIS používá active learning a zlepšovat tak kvalitu výsledků. V procesu aktivně učit LUIS prozkoumá všechny projevy koncového bodu a vybere projevy, které je jistí, jaké. Pokud označíte popiskem těchto projevů, trénování a publikování, pak LUIS identifikuje projevy přesněji. 

## <a name="filter-utterances"></a>Filtrovat projevy
1. Otevřete aplikaci (například TravelAgent) tak, že vyberete jeho název na **Moje aplikace** stránce a pak vyberte **sestavení** v horním panelu.

2. V části **zvýšit výkon aplikace**vyberte **zkontrolujte koncový bod projevy**.

3. Na **zkontrolujte koncový bod projevy** stránky, vyberte v **seznam filtrů záměr nebo entity** textového pole. Tento rozevírací seznam obsahuje všechny příkazy v rámci **záměry** a všechny entity v rámci **entity**.

    ![Projevy filtru](./media/label-suggested-utterances/filter.png)

4. V rozevíracím seznamu vyberte kategorii (záměry a entity) a zkontrolujte projevy.

    ![Záměr projevů](./media/label-suggested-utterances/intent-utterances.png)

## <a name="label-entities"></a>Popis entity
Služba LUIS nahradí entity tokeny (slova) názvy entit modře zvýrazněna. Pokud utterance má neoznačených entity, označte je utterance. 

1. Vyberte na slova v utterance. 

2. Vyberte entitu ze seznamu.

    ![Popis entity](./media/label-suggested-utterances/label-entity.png)

## <a name="align-single-utterance"></a>Zarovnat jednoho utterance

Každý utterance má navrhované záměr zobrazí v **zarovnané záměr** sloupce. 

1. Pokud budete souhlasit s návrhem, vyberte na zaškrtávací políčko.

    ![Zachovat zarovnané záměr](./media/label-suggested-utterances/align-intent-check.png)

2. Pokud jste Nesouhlasím s návrhem, vyberte správné záměr zarovnané záměru rozevíracího seznamu a potom vyberte na značku zaškrtnutí vpravo zarovnaný záměr. 

    ![Zarovnat záměr](./media/label-suggested-utterances/align-intent.png)

3. Po výběru na značku zaškrtnutí utterance se odebere ze seznamu. 

## <a name="align-several-utterances"></a>Zarovnat několik projevy

Chcete-li zarovnat několik projevy, zaškrtněte políčko nalevo od projevy a pak vyberte na **přidat vybrané** tlačítko. 

![Zarovnat několik](./media/label-suggested-utterances/add-selected.png)

## <a name="verify-aligned-intent"></a>Ověřte zarovnané záměr
Můžete ověřit, utterance byla souladu s správné záměr tak, že přejdete **záměry** stránky, vyberte název záměru a kontrola projevy. Utterance z **zkontrolujte koncový bod projevy** je v seznamu.

## <a name="delete-utterance"></a>Odstranit utterance
Každý utterance lze odstranit ze seznamu revize. Po odstranění, nebude se zobrazovat v seznamu znovu. To platí i v případě, že uživatel zadá stejný utterance z koncového bodu. 

Pokud jste si jisti, jestli byste měli odstranit utterance, buď ji přesunout do záměru, None nebo vytvořit nové záměr, jako je například "různé" a přesunout utterance tohoto záměru. 

## <a name="delete-several-utterances"></a>Odstranit několik projevy
Chcete-li odstranit několik projevy, vyberte všechny položky a vyberte na koše napravo od **přidat vybrané** tlačítko.

![Odstranit více](./media/label-suggested-utterances/delete-several.png)

## <a name="next-steps"></a>Další postup

Pokud chcete otestovat, jak vylepšuje výkon po popisek navrhované projevy, dostanete testovací konzole tak, že vyberete **testování** v horním panelu. Pokyny o tom, jak testovat svou aplikaci pomocí testovací konzole najdete v tématu [trénování a testování vaší aplikace](luis-interactive-test.md).
