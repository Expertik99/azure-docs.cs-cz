---
title: Informace o službě QnA Maker – Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: QnA Maker umožňuje provozovat službu otázek a odpovědí na základě částečně strukturovaného obsahu, jako jsou dokumenty nebo adresy URL s nejčastějšími dotazy a příručky k produktům.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: overview
ms.date: 06/15/2018
ms.author: nolachar
ms.openlocfilehash: f34cec047c18a6db10b5adda82800c51d44c1155
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/06/2018
ms.locfileid: "36295141"
---
# <a name="what-is-qna-maker"></a>Co je QnA Maker?

[QnA Maker](https://qnamaker.ai) umožňuje provozovat službu otázek a odpovědí na základě částečně strukturovaného obsahu, jako jsou dokumenty nebo adresy URL s nejčastějšími dotazy a příručky k produktům. Můžete sestavit model otázek a odpovědí, který flexibilně reaguje na dotazy uživatelů a poskytuje odpovědi, a natrénovat chatbota, aby tyto odpovědi používat přirozeným konverzačním způsobem.

Snadno použitelné grafické uživatelské rozhraní umožňuje vytvořit, natrénovat a zprovoznit službu bez předchozích zkušeností s vývojem.

![Přehled](../media/qnamaker-overview-learnabout/overview.png)

## <a name="highlights"></a>Nejzajímavější body

- Ucelené prostředí **bez kódu** pro [vytvoření chatbota pro nejčastější dotazy](https://aka.ms/qnamaker-docs-create-faqbot).
- **Už žádné omezování využití sítě**. Plaťte za hostování služby, a ne za počet transakcí. Další podrobnosti najdete na [stránce s cenami](https://aka.ms/qnamaker-docs-pricing).
- **Škálování podle potřeby**. Zvolte vhodné skladové položky jednotlivých komponent, které budou vyhovovat vašemu scénáři. Podívejte se, jak [zvolit kapacitu](https://aka.ms/qnamaker-docs-capacity) pro vaši službu QnA Maker.
- **Úplné dodržování předpisů pro data**. Data a součásti modulu runtime se nasazují do předplatného Azure uživatelů a splňují jejich požadavky na dodržování předpisů. Další podrobnosti najdete níže.

## <a name="key-qna-maker-processes"></a>Klíčové procesy služby QnA Maker

QnA Maker pro vaše data zajišťuje dvě klíčové služby:

* **Extrakce:** Strukturovaná data otázek a odpovědí se extrahují ze zdrojů částečně strukturovaných dat, jako jsou nejčastější dotazy a příručky k produktům. Tato extrakce se provádí při vytváření znalostní báze. Znalostní bázi můžete vytvořit [tady](https://aka.ms/qnamaker-docs-createkb).

* **Hledání shody:** Po [natrénování a otestování](https://aka.ms/qnamaker-docs-trainkb) znalostní báze ji můžete [publikovat](https://aka.ms/qnamaker-docs-publishkb). Tím se povolí koncový bod pro znalostní bázi QnA Maker, který pak můžete použít ve svém chatbotu nebo aplikaci. Tento koncový bod přijímá otázky uživatelů a jako odpověď nabízí nejlepší shodu otázky a odpovědi ve znalostní bázi společně se skóre spolehlivosti shody.

## <a name="qna-maker-architecture"></a>Architektura služby QnA Maker

Stack služby QnA Maker se skládá z následujících částí:

1. **Služby správy pro službu QnA Maker (rovina řízení):** Prostředí pro správu znalostní báze QnA Maker od počátečního vytvoření, aktualizací, trénování až po publikování. Tyto aktivity je možné provádět přes [portál](https://qnamaker.ai) nebo [rozhraní API pro správu](https://aka.ms/qnamaker-v4-apis). Služby správy komunikují se součástí modulu runtime uvedenou níže.

2. **Modul runtime služby QnA Maker (rovina dat):** Data a modul runtime se nasazují do předplatného Azure uživatelů v oblasti podle jejich výběru. Obsah otázek zákazníků a odpovědí se ukládá ve službě [Azure Search](https://azure.microsoft.com/services/search/) a modul runtime se nasazuje jako [App Service](https://azure.microsoft.com/services/app-service/). Volitelně můžete nasadit také prostředek [Application Insights](https://azure.microsoft.com/services/application-insights/) pro účely analýz.

![Architektura](../media/qnamaker-overview-learnabout/architecture.png)

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Vytvoření služby QnA Maker](../how-to/set-up-qnamaker-service-azure.md)
