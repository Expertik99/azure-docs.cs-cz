---
title: Vyberte cenové úrovně nebo SKU pro službu Azure Search | Dokumentace Microsoftu
description: 'Služba Azure Search je možné zřídit za tyto skladové položky: Free, Basic a Standard, ve kterém Standard je k dispozici v různých konfigurace prostředků a kapacity úrovně.'
services: search
author: HeidiSteen
manager: cgronlun
tags: azure-portal
ms.service: search
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: heidist
ms.openlocfilehash: f7cf471a69395cef0aef7d5dd2e3c77218bf97a3
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715276"
---
# <a name="choose-a-pricing-tier-for-azure-search"></a>Zvolte cenovou úroveň pro službu Azure Search

Ve službě Azure Search [je služba zřízena za](search-create-service-portal.md) na konkrétní cenové úrovně nebo SKU. Mezi možnosti patří **Free**, **základní**, nebo **standardní**, kde **standardní** je k dispozici v několika konfiguracích a kapacity. 

Cílem tohoto článku je vám pomůže vybrat do vrstvy. Doplňuje [stránce s cenami](https://azure.microsoft.com/pricing/details/search/) a [omezení služby](search-limits-quotas-capacity.md) stránka s hodnotou hash fakturační koncepty a vzory využití související s různými úrovněmi. Také doporučuje iteračního postupu pochopit, jaké úroveň nejlepší bude vyhovovat vašim potřebám. 

Úrovně určit kapacitu, není funkce. Pokud kapacita na úrovni se ukázalo příliš nízké, budete muset zřizovat nové služby vyšší úrovně a poté [znovu načíst indexů](search-howto-reindex.md). Neexistuje žádné místní upgrade stejné služby z jedné skladové položky do jiného.

Dostupnost funkcí není potřeba primární úrovně. U všech úrovní, včetně **Free** vrstvy, nabízí paritu funkcí s výjimkou podpora indexeru pro S3HD. Ale omezení indexování a prostředků efektivně můžete omezit rozsah používání funkcí. Například [kognitivního vyhledávání](cognitive-search-concept-intro.md) indexování má dlouhotrvající dovednosti tento časový limit na bezplatné služby není-li být velmi malé datové sady se stane s.

> [!TIP]
> Většina zákazníků začíná **Free** vrstvy pro vyhodnocení a potom jim dodejte **standardní** pro vývoj. Po zvolení vrstvu a [při zřizování vyhledávací služby](search-create-service-portal.md), můžete [zvýšit počet replik a oddílu](search-capacity-planning.md) pro optimalizaci výkonu. Další informace o kdy a proč by upravit kapacitu, najdete v části [aspekty týkající se výkonu a optimalizace](search-performance-optimization.md).
>

## <a name="billing-concepts"></a>Koncepty fakturace

Koncepty, kterým je třeba porozumět pro výběr úrovně zahrnují definice kapacity, limity pro služby a služby jednotky. 

### <a name="capacity"></a>Kapacita

Kapacita má strukturu *repliky* a *oddíly*. 

+ Repliky jsou instancemi služby vyhledávání, ve kterém každá replika hostuje jednu kopii tohoto indexu s vyrovnáváním zatížení. Služba s 6 replik má například 6 kopií každý index načíst služby. 

+ Oddíly ukládat indexy a automaticky rozdělí prohledávatelná data: dva oddíly rozdělit třetiny indexu v polovině, tři oddíly a tak dále. Z hlediska kapacity *oddílu velikost* je primární odlišující funkce napříč úrovněmi.

> [!NOTE]
> Všechny **standardní** úrovní podpory [flexibilní kombinace repliky a oddíly](search-capacity-planning.md#chart) , abyste mohli [váha systému rychlost ani prostředky úložiště](search-performance-optimization.md) změnou zůstatek na účtu. **Základní** nabízí tři repliky vysokou dostupnost, ale má pouze jeden oddíl. **Bezplatné** úrovně se neposkytuje vyhrazené prostředky: výpočetní prostředky, které jsou sdíleny více bezplatných služeb.

### <a name="search-units"></a>Jednotky vyhledávání

Nejdůležitější fakturační koncept pochopit je *jednotka služby search* (SU), což je fakturační jednotka pro službu Azure Search. Protože Azure Search závisí na repliky a oddíly na funkci, nemá smysl pro fakturaci podle jedné nebo druhé. Místo toho fakturace vychází složeného obou. Formulaically, SU je produkt repliky a oddíly, které jsou používané službou: (R X P = SU). Minimálně každá služba začíná 1 SU (jednu repliku a jeden oddíl), ale realističtější model ještě nemusí být repliky 3, 3 oddíly služby účtovat jako 9 su. 

I když každá úroveň nabízí postupně vyšší kapacitu, můžete použít část celkové kapacity online, uchovávající zbývající ve fondu. Z hlediska fakturace, je počet oddílů a replik uvést online, vypočítané pomocí vzorce SU, která určuje, co skutečně platíte.

Fakturační sazby je za SU, kde každá úroveň má jiné míry po hodinách. Sazby za pro každou vrstvu můžete najít na [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/search/).

### <a name="limits"></a>Omezení

Služby prostředky hostitele, jako jsou indexy, indexery a tak dále. Každá úroveň má [omezení služby](search-limits-quotas-capacity.md) na množství prostředků můžete vytvořit. Limit počtu indexy (a dalších objektů) v důsledku toho je druhá odlišující funkce napříč úrovněmi. Při kontrole jednotlivé možnosti na portálu, mějte na paměti omezený počet indexů. Jiné prostředky, jako jsou indexování zdrojů dat a dovednosti, je propojen s limity indexu.

## <a name="consumption-patterns"></a>Vzory využití

Většina zákazníků začíná **Free** služby, které se zachovat po neomezenou dobu a pak vyberte jednu z **standardní** úrovní pro úlohy vážné vývojové nebo produkční prostředí. 

![Služba Azure search úrovně](./media/search-sku-tier/tiers.png "cenových úrovní Azure search")

Na obou koncích **základní** a **S3 HD, High Density** pro jejich vzorce spotřeby důležitý, ale netypických neexistují. **Základní** je pro malé produkční: nabízí smlouvu SLA, vyhrazené prostředky, vysokou dostupnost, ale středně velká úložiště, přičemž navýšení kapacity na celkem 2 GB. Tato úroveň byla navržena pro zákazníky, kteří který konzistentně pod využívaných dostupné kapacity. Na konci úplně **S3 HD, High Density** je pro úlohy typické pro nezávislé výrobce softwaru, partnery, [víceklientské řešení](search-modeling-multitenant-saas-applications.md), nebo žádnou konfiguraci volání pro velký počet malých indexy. Často je samozřejmé při **základní** nebo **S3 HD, High Density** úroveň je správné přizpůsobit, ale pokud chcete, aby potvrzení můžete vytvořit [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) nebo [obraťte se na Azure Podpora](https://azure.microsoft.com/support/options/) další pokyny.

Posunutí fokus pro běžně používané úrovně standard **S1 na S3** jsou průběh zvýšit limity kapacity s důležitý body na velikost oddílu a maximální hodnoty na čísla indexů, indexerů a corollary prostředků:

|  | S1 | S2 | S3 |  |  |  |  |
|--|----|----|----|--|--|--|--|
| Velikost oddílu|  25 GB | 100 GB | 250 GB |  |  |  |  |
| omezení indexu a indexeru| 50 | 200 | 200 |  |  |  |  |

**S1** je běžné volbou až vyhrazených prostředků a několik oddílů budou nezbytné. S oddíly 25 GB až 12 oddílů, omezení za jednoúčelovou **S1** je celkem 300 GB, pokud maximalizovat oddíly přes repliky (naleznete v tématu [přidělit oddílů a replik](search-capacity-planning.md#chart) pro více s vyrovnáváním sestavení.)

Stránky portálu a cenové zaměření na velikost oddílu a úložiště, ale pro každou vrstvu všechny výpočetní možnosti (kapacita disku, procesory rychlost) lineárně roste s cenou. **S2** replika je rychlejší než **S1**, a **S3** je rychlejší než **S2**. **S3** úrovně přerušit obecně lineární vzorku ceny za výpočetní prostředky se neúměrně rychlejší vstupně-výstupních operací. Pokud očekáváte, že vstupně-výstupní operace jako kritický bod, **S3** poskytuje mnohem více vstupně-výstupních operací než nižší úrovně.

**S3** a **S3 HD, High Density** jsou identické vysoké kapacity infrastruktury, ale každý jeden dosáhne svého maximálního limitu různými způsoby. **S3** cílí na menší počet velmi velké indexů. V důsledku toho jeho maximální limit je vázán na zdroj (pro každou službu 2,4 TB). **S3 HD, High Density** cílí na větší počet indexů velmi malý. Za 1000 indexů **S3 HD, High Density** dosaženo omezení v podobě omezení indexu. Pokud jste **S3 HD, High Density** zákazníka, který vyžaduje více než 1000 indexů, obraťte se na Microsoft Support informace o tom, aby bylo možné pokračovat.

> [!NOTE]
> Dříve limity pro dokumenty byly potřeba, ale již nejsou použitelné pro většinu služeb Azure Search zřízených po. ledna 2018. Další informace o podmínkách, u kterých se omezení dokumentů vztahuje stále účtují najdete v tématu [omezení služby: dokumentu limity](search-limits-quotas-capacity.md#document-limits).
>

## <a name="evaluate-capacity"></a>Vyhodnocení kapacity

Kapacitu a náklady na provoz služby přejděte ručně v rukou. Úrovně uplatňují omezení na dvou úrovních (úložiště a prostředků), takže by měl přemýšlíte o obou protože kteréhokoli z nich nejprve nedostanete je omezení účinný. 

Obchodní požadavky stanovují obvykle počet indexů, které budete potřebovat. Například globální index pro velké úložiště dokumentů nebo možná více indexů, podle oblasti, aplikace nebo volné místo na firmy.

Chcete-li určit velikost indexu, budete muset [vytvořit takovou](search-create-index-portal.md). Struktura dat ve službě Azure Search je primárně [obrácený index](https://en.wikipedia.org/wiki/Inverted_index), který má jiné vlastnosti než zdrojová data. Pro obrácenou index velikost a složitost jsou určeny obsah, nemusí nutně jít množství dat, která kanál do něj. Zdroj velkých objemů dat s masivní redundance by mohlo způsobit index na menší než menší datové sady s velmi proměnlivá obsahem.  V důsledku toho je zřídka možné odvodit velikost indexu na základě velikosti původní datové sady.

### <a name="step-1-develop-rough-estimates-using-the-free-tier"></a>Krok 1: Vývoj hrubé odhady pomocí na úrovni Free

Jedním z přístupů k odhadování kapacity je začít s **Free** vrstvy. Vzpomeňte si, že **Free** služba nabízí až 3 indexy, 50 MB úložiště a indexování čas 2 minut. Může být náročné odhadnout velikost předpokládané indexu se těmto omezením, ale následující příklad ukazuje postup:

+ [Vytvoření bezplatné služby](search-create-service-portal.md)
+ Příprava malé, reprezentativní datové sady (Předpokládejme monitorovalo dokumentů a velikost vzorku 10 %)
+ [Vytvoření počátečního indexu](search-create-index-portal.md) a poznamenejte si jeho velikost na portálu (Předpokládejme 30 MB)

Předpokládáme, že byla ukázka zástupce a 10 % celý zdroj dat. a 30 MB indexu bude přibližně 300 MB Pokud všechny dokumenty se indexují. Ozbrojené s tímto číslem předběžné, může dvojnásobek velikosti s rozpočtem pro dvě indexy (vývoj a produkčním prostředí), a cena celkem 600 MB v požadavky na úložiště. To je snadno splněno **základní** vrstvy, takže byste začali existuje.

### <a name="step-2-develop-refined-estimates-using-a-billable-tier"></a>Krok 2: Vývoj kontrast odhady pomocí fakturovatelné úrovni.

Zákazníci, kteří dávají přednost začínat vyhrazených prostředcích, které může pojmout větší vzorkování a časy zpracování a pak vytvářet realistické odhady množství index, velikost a svazků dotazu během vývoje. Zpočátku služba se zřizuje podle odhad nejlepší odhad a pak zrání vývojového projektu, týmy obvykle zjistit, zda existující službu nad nebo pod kapacity pro předpokládané produkční úlohy. 

1. [Zkontrolujte limity pro služby v jednotlivých vrstvách](https://docs.microsoft.com/en-us/azure/search/search-limits-quotas-capacity#index-limits) můžete určit, zda nižší úrovně můžou podporovat množství indexy, které potřebujete. Mezi **základní**-**S1**- **S2** úrovně, omezení indexu jsou 15-50-200, v uvedeném pořadí.

1. [Vytvoření služby na fakturovatelné úrovni](search-create-service-portal.md):

    + Začněte s nízkou, na **základní** nebo **S1** Pokud jste na začátku učení křivky.
    + Spustit na vysokou **S2** nebo dokonce **S3**, pokud ve velkém měřítku indexování a dotazování načítají samozřejmé.

1. [Vytvoření počátečního indexu](search-create-index-portal.md) k určení, jak přeloží zdroje dat do indexu. Toto je jediný způsob, jak odhadnout velikost indexu.

1. [Monitorování úložiště, limity pro služby, množství dotazů a latence](search-monitor-usage.md) na portálu. Na portálu se zobrazí dotazy za druhé, omezené dotazy a latence hledání; všechny z nich může pomoci při rozhodování, pokud jste na správné úrovni. Kromě portálu metriky, můžete nakonfigurovat hloubkového monitorování, jako je například interaktivní analýza povolením [Analýza provozu vyhledávání](search-traffic-analytics.md). 

Index počtu a velikosti jsou stejně důležité pro analýzu, protože je dosaženo omezení maximální prostřednictvím plného využití úložiště (oddíly) nebo maximálními limity prostředků (indexy, indexery a tak dále), podle toho, co nastane dřív. Na portálu, vám pomůže sledovat obě, zobrazuje aktuální využití a omezení maximální vedle sebe na stránce Přehled.

> [!NOTE]
> Požadavky na úložiště může být cokoli navíc zvýšeným, pokud dokumenty obsahují nadbytečná data. V ideálním případě dokumenty obsahují pouze data, které potřebujete pro vyhledávání. Binární data bez lze prohledávat a je třeba uložit samostatně (třeba ve službě Azure tabulkou nebo objektem blob storage) s pole v indexu na odkaz URL na externí data. Maximální velikost jednotlivý dokument je 16 MB (nebo méně if jsou hromadné nahrávání v jedné žádosti více dokumentů). [Omezení služby Azure Search](search-limits-quotas-capacity.md) obsahuje další informace.
>

**Důležité informace o svazku dotazu**

Dotazů za sekundu (QPS) je metriku, která získá význačnost během optimalizace výkonu, ale není obvykle potřeba úroveň Pokud očekáváte, že svazek vysoce dotaz na začátku.

Všechny úrovně standard rovnováhu replik doručovat do oddílů, podpora obsloužit dotaz prostřednictvím další repliky pro načítání vyrovnávání a další oddíly pro paralelní zpracování. Po zřízení služby můžete vyladit výkon.

Zákazníky, kteří očekávají strong trvalejší dotazu svazky od samého počátku zvažte vyšší úrovně, se opírá o výkonnější hardware. Můžete pak proveďte oddílů a replik v režimu offline nebo dokonce přepnout na nižší úroveň služby, pokud tyto svazky dotaz sloučit. Další informace o tom, jak vypočítat propustnost dotazů, najdete v části [Azure Search výkon a optimalizace](search-performance-optimization.md).


**Smlouvy o úrovni služeb**

**Free** funkce vrstvy a preview nezahrnují [smlouvami o úrovni (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Pro všechny úrovně fakturovatelná smlouvy o úrovni služeb se projeví při zřizování dostatečná redundanci pro vaši službu. Dva nebo více replik jsou požadovány pro smlouvu SLA dotazování (čtení). Tři nebo více replik jsou požadovány pro dotazy a indexování SLA (čtení a zápis). Počet oddílů není k posouzení smlouvu SLA. 

## <a name="tips-for-tier-evaluation"></a>Tipy pro úroveň hodnocení

+ Zjistěte, jak vytvářet efektivní indexy a které metodologie aktualizace jsou nejnižší impactful. Doporučujeme [Analýza provozu vyhledávání](search-traffic-analytics.md) pro poznatky získané v aktivitě dotazu.

+ Povolit metriky pro sestavení kolem dotazů a shromažďování dat celého vzorce používání (dotazy během pracovní doby, indexování během hodin mimo špičku) a tato data použít k informování budoucími rozhodnutí o zřízení. Zatímco na úrovni hodinových nebo denních není praktické, můžete dynamicky upravit oddíly a prostředky, abyste plánované změny ve svazcích dotazu, nebo neplánované ale trvalé změny Pokud úrovně dostatečně dlouhá, aby zaručujete akce.

+ Mějte na paměti, že pouze nevýhodou zřizování je, že bude pravděpodobně nutné dovolí služby, pokud jsou větší než na kolik máte odhadované skutečné požadavky. Pokud chcete zabránit přerušení služby, by vytvořit novou službu v rámci stejného předplatného na vyšší úrovni a spustit ji vedle sebe, dokud všechny aplikace a žádosti o cílit na nový koncový bod.

## <a name="next-steps"></a>Další postup

Začněte **Free** vrstvy a vytvoření počátečního indexu pomocí některé podsady z vašich dat lépe pochopit jeho vlastnosti. Struktury dat ve službě Azure Search je obrácenou index, kde je velikost a složitost obrácenou indexu se určují podle obsahu. Mějte na paměti, že vysoce redundantní obsah obvykle za následek menší index než vysoce nestandardní obsah. V důsledku toho je obsah vlastnosti spíše než velikost datové sady, která určuje požadavky na úložiště indexů.

Jakmile budete mít představu počáteční velikost indexu [zřízení vám začne fakturovat služba](search-create-service-portal.md) na jednu z úrovní popisovaných v tomto článku, buď **základní** nebo **standardní** vrstvy. Zmírnit všechny umělých omezení na podmnožiny dat a [opětovné sestavení indexu](search-howto-reindex.md) zahrnout všechna data ve skutečnosti chcete umožnit prohledávání.

[Přidělit oddílů a replik](search-capacity-planning.md) podle potřeby můžete získat výkon a škálování, budete potřebovat.

Pokud výkon a kapacitu bez problémů, budete hotovi. V opačném případě znovu vytvořte službu vyhledávání na jinou úroveň, která více přesně odpovídá vašim potřebám.

> [!NOTE]
> Další pomoc s vašimi dotazy účtovat [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) nebo [obraťte se na podporu Azure](https://azure.microsoft.com/support/options/).