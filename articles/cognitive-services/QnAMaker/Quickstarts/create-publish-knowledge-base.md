---
title: Rychlý start k vytvoření KB – QnA Maker - Azure Cognitive Services | Dokumentace Microsoftu
titleSuffix: Azure
description: Podrobný kurz týkající se vytváření znalostní báze v nástroje QnA Maker
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 49a21e8cf4e45fc4408f6c8039c3f0c7cc455e7a
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "44028833"
---
# <a name="create-train-and-publish-your-knowledge-base"></a>Vytváření, trénování a publikování znalostní báze

Nástroj QnA Maker znalostní bázi (KB) můžete vytvořit vlastní obsah, jako jsou nejčastější dotazy nebo produktových příruček. Znalostní BÁZE QnA Maker v tomto příkladu je vytvořena z jednoduchá webová stránka nejčastější dotazy k odpovědi na otázky na obnovení klíče Bitlockeru.

## <a name="prerequisite"></a>Požadavek

> [!div class="checklist"]
> * Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="create-a-qna-maker-knowledge-base"></a>Vytvoření znalostní báze QnA Maker

1. Přihlaste se k QnAMaker.ai pomocí přihlašovacích údajů Azure.

2. Na webu nástroje QnA Maker, vyberte **vytvoření znalostní báze**.

   ![Vytvořit nový KB](../media/qna-maker-create-kb.png)

3. Na **vytvořit** v kroku 1, vyberte na stránce **vytvořit službu QnA**. Budete přesměrováni [webu Azure portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) k nastavení služby QnA Maker v rámci vašeho předplatného. Pokud vyprší časový limit na webu Azure portal, vyberte **opakujte** v lokalitě. Po připojení se zobrazí řídicí panel Azure.

4. Jakmile úspěšně vytvořit novou službu QnA Maker v Azure, vraťte se na qnamaker.ai/create. Vyberte svoji službu QnA z rozevíracích seznamů v kroku 2. Pokud vytvoříte novou službu QnA, je nutné aktualizovat stránku.

   ![Vyberte službu QnA KB](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

5. V kroku 3 pojmenujte znalostní BÁZÍ **Moje KB QnA ukázka**.

6. Pokud chcete přidat obsah do znalostní BÁZÍ, vyberte tři typy datových zdrojů. V kroku 4, v části **naplnit znalostní BÁZÍ**, přidejte [nejčastější dotazy k obnovení nástroje BitLocker](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-overview-and-requirements-faq) adresu URL **adresy URL** pole.

   ![Vyberte službu QnA KB](../media/qnamaker-quickstart-kb/add-datasources.png)

7. V kroku 5 vyberte **vytvořit znalostní BÁZÍ**.

8. Když se vytváří KB, se zobrazí automaticky otevírané okno. Proces extrakce trvá několik minut, přečtěte si stránku HTML a identifikovat otázek a odpovědí.

9. Po úspěšném vytvoření KB **znalostní báze** otevře se stránka. Obsah znalostní BÁZE na této stránce můžete upravit.

10. V pravém horním rohu vyberte **QnA přidat pár** přidáte nový řádek v **redakční** část KB. V části **otázku**, zadejte **Dobrý den.** V části **odpovědí**, zadejte **Hello. Klást otázky bitlockeru.**

   ![Přidat pár QnA](../media/qnamaker-quickstart-kb/add-qna-pair.png)

11. V pravém horním rohu vyberte **uložit a jejich trénování** uložit provedené úpravy a trénování modelu QnA Maker. Úpravy nejsou zachovány, dokud jsou uloženy.

   ![Uložit a trénování](../media/qnamaker-quickstart-kb/add-qna-pair2.png)

12. V pravém horním rohu vyberte **testování** k testování, aby změny vstoupily v platnost. Zadejte **Dobrý den existuje** do pole a stiskněte Enter. Měli byste vidět odpovědi, kterou jste vytvořili jako odpověď.

13. Vyberte **zkontrolujte, jestli se** odpovědi podrobněji prozkoumat. V okně testu se používá k testování změny KB předtím, než se publikují.

   ![Test panelu](../media/qnamaker-quickstart-kb/inspect-panel.png)

14. Vyberte **testovací** zavřete **Test** místní.

15. V nabídce vedle **upravit**vyberte **publikovat**. Pokud chcete potvrdit, vyberte **publikovat** na stránce.

16. Služba QnA Maker se úspěšně publikováno. Můžete použít koncový bod v aplikaci nebo kód robota.

   ![Publikování](../media/qnamaker-quickstart-kb/publish-sucess.png)

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Vytvoření znalostní báze](../How-To/create-knowledge-base.md)
