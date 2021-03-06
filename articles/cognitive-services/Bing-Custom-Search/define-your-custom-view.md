---
title: 'Vlastní vyhledávání Bingu: Definování vlastního zobrazení | Dokumentace Microsoftu'
description: Popisuje, jak vytvořit web a vertikální vyhledávací služby
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 8ef8aabc7363db88317a6428301512b0a0d4c055
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158768"
---
# <a name="configure-your-custom-search-experience"></a>Konfigurace prostředí pro vlastní vyhledávání
Instanci vlastního vyhledávání umožňuje přizpůsobit možnosti vyhledávání zahrnout obsah jenom z webů, které vaši uživatelé záleží. Místo prohledávání celé webové vyhledává Bing jenom jeden řez webu, který vás zajímá.
Chcete-li vytvořit vlastní zobrazení webu, použijte vlastní vyhledávání Bingu [portál](https://customsearch.ai). Informace o přihlášení k portálu najdete v tématu [vytvořit první instanci vlastního vyhledávání Bingu](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/quick-start). Na portálu umožňuje vytvořit instanci hledání, který určuje domény, podstránky a webové stránky, které chcete, aby Bingu pro vyhledávání a ty, které nechcete, aby ji k prohledávání. Kromě zadání adresy URL, o kterém víte o obsahu, můžete také klást na portálu pro návrh obsah, který chcete přidat do zobrazení. Tady jsou způsoby, že můžete definovat určitý řez webu: 

1.  Domény. Řez domény zahrnuje veškerý obsah v rámci rozsahu internetové domény nebyl nalezen. Například www.microsoft.com. Vynechání "www" způsobí, že Bingu také hledat subdomény doméně. Například pokud chcete zadat microsoft.com, Bingu také vrátí výsledky z webu support.microsoft.com nebo technet.microsoft.com.
2.  Podstránku. Podstránku řez zahrnuje veškerý obsah v podstránku a cesty pod ní. V cestě můžete zadat maximálně dvě podstránky. Například www.microsoft.com/en-us/windows/ 
3.  Webová stránka. Řez webová stránka může obsahovat pouze webové stránce Vlastní vyhledávání. Volitelně můžete zadat, jestli se má zahrnout podstránky.

Všechny domény, podstránky a webové stránky, které zadáte, musí být veřejné a indexované bingem. Pokud máte licence, kterou chcete do hledání zahrnout veřejné lokality a Bing ještě indexovat jej, najdete v článku Bing [správce webového serveru dokumentaci](https://www.bing.com/webmaster/help/webmaster-guidelines-30fba23a) podrobnosti o tom, jak Bingu se. Podrobnosti o tom, jak Bingu aktualizovat procházené lokalitu, pokud je aktuální index také v dokumentaci správce webového serveru.

## <a name="adding-slices-to-your-custom-search"></a>Přidání řezy do vašeho vlastního hledání
Při definování vaší instance vlastního hledání zadáte aktivní a blokovaných domén, podstránky a webové stránky, které chcete vyhledat nebo není hledání.  

- Aktivní: Seznam domén, podstránky nebo webové stránky, které mají být zahrnuty do hledání. 
- Blokováno: Seznam domény, podstránky nebo webové stránky, které chcete vyloučit z hledání. Položky, které jste bloku by měl být obsahu v doménách a podstránky uvedené v seznamu aktivní.

Každý seznam, klikněte na tlačítko na kartách aktivní a uzavřeno ve vaší instance vlastního hledání. 

<a name="active-and-blocked-lists"></a>
## <a name="active-and-blocked-lists"></a>Aktivní a uzavřeno seznamy 
K určení určitý řez, který chcete Bingu pro vyhledávání webu, klikněte na tlačítko **aktivní** kartu a seznam domén, podstránky a webových stránek pro hledání. Můžete přidat určitý řez přímo do seznamu nebo přidat více než jeden řez tak, že nahrajete do textového souboru pomocí nahrát ikonu.

Podrobnosti nahrávání souborů: 

- Nahrávání souborů je k dispozici pouze pro přidání kolekce obsahuje nějaké řezy do seznamu aktivní, nelze ho použít k přidání kolekce obsahuje nějaké řezy do seznamu Blokované. 
- Vytvořte textový soubor a zadejte jednu doménu, podstránku nebo webové stránky na řádek. Celý nahrávání byl odmítnut, pokud dojde k chybě. 
- Pokud blokováno seznam obsahuje doménu, podstránku nebo webovou stránku, která jste zadali v souboru k odeslání, služba odebere ze seznamu blokovaných a přidá do seznamu aktivní. 
- Služba bude ignorovat duplicity v souboru k odeslání.

Upravit nebo odstranit řezů, použijte možnosti ve sloupci ovládací prvky. 

Řezy můžete podobně, přidat do seznamu blokovaných (s výjimkou nahrání souboru nelze použít k určení řezy).

## <a name="pinned-list"></a>Připnuté seznamu
Na portálu můžete taky připnout konkrétní webové stránky k hornímu okraji výsledek hledání, pokud uživatel zadá konkrétní hledaný termín. **Pinned** karta obsahuje seznam dvojic dotazu období a webové stránky, které určují webovou stránku, která se zobrazí jako nejlepší výsledek pro konkrétní dotaz. Výraz dotazu uživatele musí přesně odpovídat termín připnuté dotazu.
Informace o připínání výsledků najdete v tématu [upravit pořadí](#adjustrank).

Připnutí výsledky není k dispozici pro vyhledávání obrázků.

## <a name="site-suggestions"></a>Návrhy lokality
Po přidání do seznamu aktivní řezů, generuje tato služba webu a podstránku návrhy, které můžete chtít přidat k vašemu hledání. **Můžete chtít přidat** oddíl obsahuje návrhy. Na stránce nastavení instance obsahuje tato část jenom v případě, že jsou k dispozici návrhy. 

Návrhy přidat do seznamu aktivní, klikněte ikonu +.  Protože služba generuje návrhy na základě svého nastavení, nezapomeňte kliknout na **aktualizovat** po přidání návrhy. 

## <a name="preview-pane"></a>Podokno náhledu
Můžete otestovat na vaše instance hledání v podokně náhledu na pravé straně posílat vyhledávací dotazy a zobrazit výsledky. Vyberte **Moje Instance**, vyberte filtru bezpečného hledání a co uvedení na trh pro vyhledávání (naleznete v tématu [parametry dotazu](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters). Zadejte dotaz a stiskněte enter nebo klikněte na ikonu Hledat a zobrazit výsledky z aktuální konfiguraci. Pokud chcete zobrazit výsledky z webu klikněte na tlačítko **webové**, abyste si zobrazili výsledky hledání obrázků, klikněte na tlačítko **Image**. 

 Použití podokna ve verzi preview, můžete také zkontrolovat výsledky Bingu tak, že vyberete **Bingu** místo **Moje Instance**. To může být užitečné k porovnání výsledků z prostředí pro vyhledávání na výsledky vrácené Bingu.

<a name="adjustrank"></a>
## <a name="adjust-rank"></a>Upravit pořadí
Na portálu umožňuje nastavit hodnocení k manipulaci s výsledky, které Bing vrátí. V podokně náhledu zadejte hledaný termín a spusťte dotaz. V podokně náhledu seznam výsledků hledání pro dotaz. Napravo od každého výsledku je seznam úpravy, můžete provádět. 

- blok. Přesune do seznamu blokovaných domény, podstránku nebo webové stránky. Můžete vybrat úroveň blokování. Bing vyloučí obsah z vybraného webu ve výsledcích hledání. 
- Boost. Zvyšuje obsah z domény nebo podstránku vyšší ve výsledcích hledání. Vyberte, jestli se má zvýšit obsah z domény nebo podstránky, že webová stránka patří.
- Snížení úrovně. Sníží úroveň obsah z domény nebo podstránku nižší ve výsledcích hledání. Vyberte, jestli se má obsah z domény degradovat nebo podstránky, že webová stránka patří. 
- Připnout nahoru. Definování webové stránky, která se zobrazí v horní části výsledků, pokud uživatele dotazu termín přesně odpovídá termín dotazu použít. Seznam aktivních nemusí obsahovat webové stránce si můžete připnout. 

Úprava pořadí není k dispozici pro vyhledávání obrázků.

## <a name="boosting-and-demoting"></a>Zvýšení a snížení
Můžete mimořádně zvýšit, zvýšit, nebo snížení úrovně do libovolné domény nebo podstránky v seznamu aktivní. Ve výchozím nastavení se přidají všechny řezy se stejnou váhou. Položky, které jsou mimořádně boosted nebo Boosted jsou řazeny výše ve výsledcích hledání (s pořadím super boost vyšší než boost). Ve výsledcích hledání jsou hodnoceny nižší položky, které jsou interpretovány.

Je důležité si uvědomit, který mimořádně zvýšit, zvýšení a snížení úrovně varianty váhu takto: poskytněte do domény nebo podstránky. Toto je pouze jedna z mnoha signály použité ve klasifikátor pro určení pořadí výsledky. To znamená, že jejich vliv pro konkrétní dotaz není zaručeno, že jako mnoho dalších faktorů, můžou ovlivnit celkovou hodnocení webové výsledky.  Možné účinnosti dané zvýšení nebo snížení úrovně má klasifikátor, testovacím možnosti vyhledávání v podokně náhledu.

Můžete mimořádně zvýšit, zvýšit, nebo degradovat položky pomocí ovládacích prvků hodnocení upravit v seznamu aktivní nebo s použitím nárůst a snížení úrovně ovládací prvky v podokně náhledu. Služba přidá do seznamu Active řezu a upraví hodnocení odpovídajícím způsobem.

Mimořádně zvýšit, zvýšení a snížení úrovně se automaticky uloží změny a okamžitě odrážejí na koncový bod vašeho vlastního hledání. 

Mimořádně zvýšit, zvýšení a snížení úrovně nejsou k dispozici pro vyhledávání obrázků.

## <a name="pin-to-top"></a>Připnout nahoru
Pro Připnutí webové stránky do horní části výsledků vyhledávání pro konkrétní dotaz, vyberte jednu z následujících možností:

1.  Na kartě Pinned zadejte adresu URL webové stránky pro Připnutí k hornímu okraji výsledky a přesný dotaz, který se aktivuje Připnutí. 
2.  V podokně náhledu zadejte termín, dotaz a klikněte na tlačítko Hledat. V dalším kroku určete webovou stránku v výsledky, které chcete připnout k hornímu okraji výsledky, pokud uživatel zadá stejný dotaz. Potom klikněte na připnout nahoru. Služba přidá do seznamu Pinned webové stránky a dotazu. 

Váš PIN kódy v kartě Pinned můžete sledovat. Kolíky jsou uvedeny jako\<dotazu\>, \<webová stránka\>"dvojice. 

Webová stránka je připnutá pouze v případě, že se dotaz uživatele přesně odpovídá vašemu dotazu. 

Pro konkrétní dotaz můžete připnout lze maximálně jedna webová stránka k hornímu okraji výsledky.

Kódy PIN nejsou k dispozici pro vyhledávání obrázků.

## <a name="use-bing-to-specify-slices"></a>Můžete určit pomocí Bingu
Existuje několik způsobů, jak můžete určit na webu, které tvoří vašeho vlastního hledání. Pokud znáte řezy, které chcete zahrnout do vaší instance, je přidejte do seznamu aktivní vaší instance. Informace o přidání položky do seznamu aktivní sami, najdete v části [aktivní a uzavřeno seznamy](#active-and-blocked-lists).
Ale pokud si nejste jistí, řezy, které chcete zahrnout, můžete spustit Bingu dotazy v podokně náhledu a zjistit, jaké vrací Bingu. Pak můžete vybrat kolekce obsahuje nějaké řezy, které chcete zahrnout do vašeho vlastního hledání. Budete pravděpodobně muset spustit více termíny dotazu, abyste měli jistotu, že určíte všechny řezy, které chcete pro vaši instanci. 

Použijte následující postup slouží k přidání kolekce obsahuje nějaké řezy do vaší instance vlastního vyhledávání Bingu. 
1.  Přihlaste se k vlastní vyhledávání Bingu [portál](https://customsearch.ai).
2.  Vytvoření instance nebo vyberte instanci pro aktualizaci.
3.  V podokně náhledu na pravé straně vyberte z rozevíracího seznamu Bingu.
4.  Do vyhledávacího pole zadejte výraz dotazu, který je relevantní pro vaši instanci.
5.  Klikněte na tlačítko **přidat web** vedle výsledků, které chcete zahrnout.
6.  Klikněte na tlačítko **OK**.

[!INCLUDE [publish or revert](./includes/publish-revert.md)]

## <a name="view-statistics"></a>Zobrazení statistik
Pokud jste přihlášení k odběru vlastního vyhledávání na příslušné úrovni (najdete v článku [stránkách s cenami](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), **statistiky** kartu se přidá do produkčních instancí. Statistiky karta zobrazuje podrobnosti o vašich koncových bodů vlastního vyhledávání používání, včetně objemy volání, nejčastější dotazy, geografické distribuce, kódy odpovědí a bezpečného hledání. Můžete filtrovat podrobně popisuje použití zadaný ovládací prvky.

## <a name="understanding-quota"></a>Principy kvóty
- Pro každou instanci vlastního vyhledávání, maximální počet hodnocení úpravy, které může provádět **aktivní** a **blokováno** řezy je omezená na 400.
- Přidání řezu na aktivní nebo Uzavřeno karty se počítá jako jeden hodnocení úpravy.
- Zvýšení a snížení počtu jako dvě pořadí úpravy.
- Pro každou instanci vlastního vyhledávání je maximální počet kódů PIN, které může být omezená na 200.

## <a name="next-steps"></a>Další postup

- [Volání vlastního vyhledávání](./search-your-custom-view.md)
- [Konfigurace prostředí uživatelského rozhraní](./hosted-ui.md)
- [Použití dekorace značek zvýraznění textu](./hit-highlighting.md)
- [Stránka webové stránky](./page-webpages.md)