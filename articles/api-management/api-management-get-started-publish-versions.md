---
title: Publikování verzí rozhraní API pomocí služby Azure API Management | Microsoft Docs
description: Pomocí kroků v tomto kurzu se naučíte publikovat několik verzí pomocí služby API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 1cbe63184578f7d1e72992577a11c58b9b83a002
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
ms.locfileid: "33937309"
---
# <a name="publish-multiple-versions-of-your-api"></a>Publikování několika verzí rozhraní API 

Občas je nepraktické, aby všichni volající vašeho rozhraní API používali naprosto stejnou verzi. Když volající chtějí upgradovat na novější verzi, chtějí mít možnost to udělat snadno pochopitelným způsobem. Ve službě Azure API Management je možné toho docílit s využitím **verzí**. Další informace najdete v tématu [Verze a revize](https://blogs.msdn.microsoft.com/apimanagement/2017/09/14/versions-revisions/).

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Přidání nové verze stávajícího rozhraní API
> * Výběr schématu verze
> * Přidání verze do produktu
> * Zobrazení verze na portálu pro vývojáře

![Verze zobrazená na portálu pro vývojáře](media/api-management-getstarted-publish-versions/azure_portal.PNG)

## <a name="prerequisites"></a>Požadavky

* Projděte si následující rychlý start: [Vytvoření instance Azure API Managementu](get-started-create-service-instance.md).
* Projděte si také následující kurz: Navíc kurzu: [Import a publikování vašeho prvního rozhraní API](import-and-publish.md).

## <a name="add-a-new-version"></a>Přidání nové verze

![Místní nabídka rozhraní API – přidání verze](media/api-management-getstarted-publish-versions/AddVersionMenu.png)

1. V seznamu rozhraní API vyberte rozhraní **Conference API**.
2. Vyberte místní nabídku (**...**) vedle něj.
3. Vyberte **+ Přidat verzi**.

    > [!TIP]
    > Verze je možné povolit také při vytváření nového rozhraní API – vyberte **Chcete vytvořit verzi tohoto rozhraní API?** na obrazovce **Přidat rozhraní API**.

## <a name="choose-a-versioning-scheme"></a>Výběr schématu vytváření verzí

Azure API Management umožňuje zvolit způsob, jakým umožníte volajícím určit požadovanou verzi rozhraní API. Verzi rozhraní API, která se má použít, zadáte výběrem **schématu vytváření verzí**. Toto schéma může být **cesta, hlavička nebo řetězec dotazu**. V následujícím příkladu se k výběru schématu vytváření verzí používá cesta.

![Obrazovka Přidat verzi](media/api-management-getstarted-publish-versions/AddVersion.PNG)

1. Jako **schéma vytváření verzí** ponechte vybranou možnost **cesta**.
2. Jako **identifikátor verze** přidejte **v1**.

    > [!TIP]
    > Pokud jako schéma vytváření verzí vyberete **hlavičku** nebo **řetězec dotazu**, je potřeba zadat další hodnotu – název hlavičky nebo parametr řetězce dotazu.

3. Pokud chcete, zadejte popis.
4. Vyberte **Vytvořit** a nastavte svou novou verzi.
5. V seznamu rozhraní API se teď pod položkou **Big Conference API** zobrazí dvě různá rozhraní API – **Original** (Původní) a **v1**.

    ![Verze uvedené pod rozhraním API na webu Azure Portal](media/api-management-getstarted-publish-versions/VersionList.PNG)

    > [!Note]
    > Pokud přidáte verzi k rozhraní bez správy verzí, automaticky se vytvoří verze **Original**, která odpovídá na výchozí adrese URL. Tím se zajistí, že proces přidání verze negativně neovlivní žádné stávající volající. Pokud vytvoříte nové rozhraní API, které má už od začátku povolené verze, verze Original se nevytvoří.

6. Teď můžete upravit a nakonfigurovat verzi **v1** jako rozhraní API, které je oddělené od verze **Original**. Změny jedné verze nemají vliv na jinou verzi.

## <a name="add-the-version-to-a-product"></a>Přidání verze do produktu

Aby se volajícím zobrazila nová verze, musí se přidat do **produktu**.

1. Na stránce modelu nasazení Classic vyberte **Produkty**.
2. Vyberte **Neomezeno**.
3. Vyberte **Rozhraní API**.
4. Vyberte **Přidat**.
5. Vyberte **Conference API, verze v1**.
6. Přejděte na stránku pro správu služby a vyberte **Rozhraní API**.

## <a name="browse-the-developer-portal-to-see-the-version"></a>Zobrazení verze na portálu pro vývojáře

1. V horní nabídce vyberte **Portál pro vývojáře**.
2. Vyberte **Rozhraní API** a všimněte si, že pro rozhraní **Conference API** se zobrazí verze **Original** a **v1**.
3. Vyberte **v1**.
4. Všimněte si **adresy URL požadavku** první operace na seznamu. Ukazuje, že cesta URL rozhraní API zahrnuje **v1**.

    ![Místní nabídka rozhraní API – přidání verze](media/api-management-getstarted-publish-versions/developer_portal.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Přidání nové verze stávajícího rozhraní API
> * Výběr schématu verze 
> * Přidání verze do produktu
> * Zobrazení verze na portálu pro vývojáře

Přejděte k dalšímu kurzu:

> [!div class="nextstepaction"]
> [Upgrade a škálování](upgrade-and-scale.md)