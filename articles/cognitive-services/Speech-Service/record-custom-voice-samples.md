---
title: Jak zaznamenat hlas ukázky pro vytvoření vlastní hlasový vstup
titleSuffix: Microsoft Cognitive Services
description: Díky přípravu robustní skriptu, Náboroví talentu dobré hlasové a záznam profesionálně hlasový vstup costum produkční kvality.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/5/2018
ms.author: v-jerkin
ms.openlocfilehash: ebd9943ad7f54a329dee16d57ab980b882d508f3
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715103"
---
# <a name="how-to-record-voice-samples-for-a-custom-voice"></a>Jak zaznamenat hlas ukázky pro vlastní hlasové funkce

Vytváření vysoce kvalitní produkční vlastní hlasové od začátku není příležitostné podniku. Ústřední součást vlastní hlasový vstup je velkou kolekci zvukové vzorky lidské řeči. Je důležité, že se tyto zvukové záznamy vysokou kvalitu. Zvolte hlasové talentu, který má prostředí pro vytváření těchto druhů záznamů a potom kliknul zaznamenaných pracovníkem příslušný záznam pomocí profesionální zařízení.

Než provedete tyto záznamy, ale je nutné skript: slova, která se budou vašeho talentu hlasové vytvoření zvukové vzorky. Nejlepších výsledků dosáhnete musí mít váš skript dobré zapsané ve fonetické pokrytí a různorodost dostatečná pro trénování modelu vlastní hlasové.

Mnoho podrobností malou, avšak důležitou přejít do vytváření profesionální hlasový záznam. Tento průvodce je Průvodce procesem, který vám pomůže získat dobrou a konzistentní výsledky.

> [!TIP]
> Nejvyšší kvality výsledků zvažte, Microsoft vám pomůžou s vývojem vašeho hlasu, vlastní zapojení. Microsoft má rozsáhlé možnosti vytváření vysoce kvalitních hlasy pro své vlastní produkty, včetně Cortana a Office.

## <a name="voice-recording-roles"></a>Role záznam hlasu

Existují čtyři základní role v projektu vlastní hlasové nahrávání.

Role|Účel
-|-
Talentu hlasu        |Tato osoba hlasové budou tvořit základ vlastní hlasu.
Záznam inženýr  |Dohlíží technické aspekty záznam a funguje záznam zařízení.
Ředitel            |Připraví skript a coaches talentu hlasové výkonu.
Editor              |Dokončí zvukové soubory a připravit je pro nahrání na portál vlastní hlasové.

Jednotlivec může vyplnit více než jednu roli. Tato příručka předpokládá, že jste se se primárně vyplnění roli ředitel a nabírat hlasové, talentů a pracovníkem záznam. Existuje nějaké informace týkající role softwarového inženýra záznam v případě, že chcete, aby záznamy sami.

## <a name="choosing-voice-talent"></a>Výběr hlasu talentu

Actors s prostředím voiceover nebo hlasu znak práci Ujistěte se, talentů dobré vlastní hlasové. Kromě toho často najdete vhodné talentu mezi announcers a programy pro čtení příspěvků.

Zvolte hlasové talentu, jehož přirozeného hlasu je například. Je možné vytvořit tak jedinečný "znak" hlasů, ale je mnohem obtížnější pro většinu talentu k jejich provedení konzistentně a úsilí může způsobit kmen hlasu.

> [!TIP]
> Obecně platí, vyhněte se použití rozpoznatelných hlasy k vytvoření vlastní hlasové – Pokud je ale samozřejmé, vaším cílem je vytvořit celebrit hlasový vstup. Hlasy méně známé jsou obvykle méně rušivé uživatelům.

Jediným nejdůležitějším faktorem pro výběr hlasu talentu je konzistence. Vašich nahrávek všechny zvukové jako byly provedeny v jednom dni ve stejné místnosti. Může přistupovat tato ideální prostřednictvím dobré záznam postupy a technologie. 

Talentu váš hlas je ta druhá půlka rovnice. Uživatel musí být schopni číst obsah s frekvence konzistentní vzhledem k aplikacím, úroveň hlasitosti, výšku a tón. Vymazat diction je nezbytnost. Vašeho talentu také musí být striktně řídit jeho výška variace, citové vliv a zkoušky prezentace řeči.

Záznam vlastní hlasové ukázky může být více fatiguing než jiné druhy práce hlasu. Většina talentu hlasové můžete zaznamenat pro dvě nebo tři hodiny denně. Omezení relací na tři nebo čtyři za týden, s denně vypnout mezi Pokud je to možné.

Pro model hlasové nahrávky by měl být život neutrální. To znamená že odcházíte utterance by neměly být čtení způsobem, že odcházíte. Náladu lze přidat k řečového později prostřednictvím prosody ovládacích prvků. Práce s vašeho talentu hlasu pro vývoj "osoby", který definuje celkový zvuku a citové tón kolegiální vlastní hlasové. V procesu budete identifikovat jaké "neutrální" Výslovnost pro této osoby.

Uživateli může mít například přirozeně upbeat posouzení vašich osobnostních. "Jejich" hlas tak mohou provádět poznámku optimismus i v případě, že neutrally mluvit. Však posouzení vašich osobnostních vlastnost by měl být konzistentní vzhledem k aplikacím a současně lákavé. Ve stávající hlasů, kde získáte představu o jste snaha o poslouchat čtení.

> [!TIP]
> Obvykle budete chtít vlastní hlasové záznamy, které provedete. Váš hlas talentu by měl být vydávání kompaktních kontrakt práce pro zařazení pro projekt.

## <a name="creating-a-script"></a>Vytváření skriptů

Výchozí bod všechny vlastní hlasový záznam relace je skript, který obsahuje projevů budou podle vašeho talentu hlasu. (Termín "projevy" zahrnuje úplné věty a kratší fráze).

Projevy ve skriptu můžou pocházet z libovolného místa: fiction, bez fiction, záznamy o studiu projevů, zprávy a něco jinak k dispozici v tisk formuláře. Pokud chcete zajistit, aby že vašeho hlasu, jak se i na konkrétní druhy slova (jako jsou lékařské terminologie nebo programovací žargonu), můžete zahrnout vět z umožňuje zrychlit Odborný Paper nebo technické dokumentace. (Ale uvidí [Legalities](#legalities) níže.) Můžete taky psát vlastní text.

Vaše projevy není musí pocházet ze stejného zdroje nebo stejný druh zdroje. Ještě není potřeba nic provádět mezi sebou. Pokud ale budete pomocí nastavená fráze (například "úspěšně přihlásíte") ve vaší aplikaci řeči, nezapomeňte zahrnout je do vašeho skriptu. To vám poskytne vlastní hlasové větší šanci dobře vyslovení tyto věty. A pokud by se rozhodnete použít záznam místo řečového, budete již mít ho ve stejné hlasu.

Konzistence je klíč při výběru hlasové talentu, je různých hallmark dobré skriptu. Váš skript by měl obsahovat mnoho různých slov a vět s řadou věty délky, struktury a náladu. Každý zvuk v jazyce by měl být reprezentována více doby a v mnoha kontextech (volá *zapsané ve fonetické pokrytí).* 

Navíc text by měl obsahovat všechny způsoby, jakými mohou být zastoupeny v psaní konkrétní zvuk a umístit každý zvuk na různých místech v těchto větách. Deklarativní vět a otázek by měl zahrnuté a číst pomocí odpovídající intonací.

Je obtížné je napsat skript, který poskytuje *právě dostatek* data, aby mohly portál Custom Speech vytvářet vhodné hlasový vstup. V praxi je nejjednodušší způsob, jak vytvořit skript, který dosahuje robustní zapsané ve fonetické pokrytí velký počet vzorků, které patří. Standardní hlasy společnosti Microsoft byly vytvořeny z desítek tisíců projevy. Byste měli být připraveni zaznamenat pár několik tisíc projevy minimálně vytvářet vlastní hlasové produkční kvality.

Zkontrolujte pečlivě pro chyby skriptu. Pokud je to možné máte někdo jiný zkontrolujte příliš. Když spouštíte prostřednictvím skriptu pomocí vašeho talentu, budete pravděpodobně catch několik více chyb.

### <a name="script-format"></a>Formát skriptu

Můžete napsat skript v aplikaci Microsoft Word. Skript je pro použití při nahrávání relaci, můžete ho nastavit způsobem, jaký najdete usnadnění práce. Vytvořte textový soubor vyžaduje vlastní hlasové portál samostatně.

Formát základní skript obsahuje tři sloupce:

* Počet utterance, počínaje 1. Číslování usnadní pro všechny uživatele v nástroji studio odkazovat na konkrétní utterance ("můžeme opakujte číslo 356"). Číslování funkce Wordu odstavců můžete automaticky čísel řádků v tabulce.
* Prázdný sloupec, ve kterém budete psát v převzít číslo nebo kód každý utterance vám pomůžou najít v dokončení záznamu časové.
* Text utterance samotný.

![Ukázkový skript](media/custom-voice/script.png)

> [!NOTE]
> Většina studios záznam v krátkém segmenty říká *trvá.* Každá převezme obvykle obsahuje projevy deset až 24. Právě poznamenat, že převzít číslo bude stačit k vyhledání utterance později. Pokud nahráváte v sadě studio, který upřednostňuje aby delší záznamy, je vhodné si čas kódu. Zobrazení viditelného času budou mít sady studio.

Po každém řádku psát poznámky ponechte dostatek místa. Ujistěte se, že žádné utterance je rozdělit mezi stránkami. Číslo stránky a vytisknout skriptu na jedné straně papíru.

Tisk tři kopie skriptu: jeden pro talentu, jeden pro inženýr a jeden pro ředitele (vy). Dokument Galerie nahrazujícím staples: umělce zkušení hlasu bude jednotlivých stránek pro vyvarování šumu jako na stránkách se zapnou.

### <a name="legalities"></a>Legalities

Zákony o autorských právech může být prvek "actor" čtení autorským textu výkonu, pro který by měl Autor práce kompenzována. Tento výkon nebudou do konečného produktu, vlastní hlasové rozpoznat. I tak není dobře zavedený legálnosti použití autorským dílem pro tento účel. Společnost Microsoft nemůže poskytnout právní Rady, jak tento problém; Vyhledejte vlastní hlavního právního poradce.

Naštěstí je možné zcela se těmto problémům. Existuje mnoho zdrojů text, který můžete použít bez souhlasu nebo licence.

|Zdrojový text|Popis|
|-|-|
|[CMU Arctic souhrnu](http://festvox.org/cmu_arctic/)|Vybrat z předem copyright funguje speciálně pro použití v projektech syntézu řeči asi 1100 věty. Vynikající výchozí bod.|
|Už funguje<br>v části autorských práv|Funguje se obvykle vydávané před 1923. Pro angličtinu [projektu Gutenberg](https://www.gutenberg.org/) nabízí desítky tisíc tyto práce. Můžete chtít zaměřit na novější funguje jako jazyk bude blíže na moderní angličtinu.|
|Government&nbsp;funguje|Funguje vytvořené ze státní správy USA nejsou autorským právům ve Spojených státech amerických, i když vláda se mohou prohlásit copyright v dalších zemích.|
|Veřejné domény.|Funguje pro které copyright byl výslovně odmítnuté nebo mít byl vyhrazen pro veřejné domény. (To nemusí být možné zrušíme copyright zcela v některé jurisdikce.)|
|Permissively licenci funguje|Distribuováno za licenci funguje jako licence Creative Commons nebo bezplatná licence GNU v dokumentaci. Wikipedia používá GFDL. Některé licence, však může omezení výkonu licencovaný obsah, který může mít vliv na vytváření modelu vlastní hlasové, proto licence si pozorně přečtěte.|

## <a name="recording-your-script"></a>Záznam skriptu

Skriptu na profesionální záznam studio, která se specializuje na záznam hlasu práci. Budou mít stánku záznam, správné zařízení a lidé provozovat ho. Vyplatí se skimp na záznam.

Prodiskutujte projekt s pracovníkem technické záznamové sady studio a požadavkům na jeho poradenství. Záznam by měl mít žádné nebo téměř žádné komprese dynamických rozsahů (maximálně 4:1). Je velmi důležité, že zvuk mají konzistentní svazku a vysoký poměr signálu šumu a při zachování bez nežádoucí zvuky.

### <a name="doing-it-yourself"></a>Teď už sami

Pokud chcete, aby se nahrávání sami, nemuseli do nástroje studio záznam, zde je krátké Úvod do. Díky vzestup domácí záznam a podcasting je jednodušší než dříve pro hledání online materiály a Rady založené na dobrý záznam.

Vaše "záznam z mýtných bran" by měla být malá místnost bez znatelného echo nebo "místnosti tónu." Měla by být jako quiet a soundproof co nejvíc. Závěsy na zdi lze použít ke snížení odezvu a neutralizovat nebo "deaden" zvuk místnosti.

Použijte studio vysoce kvalitní mikrofon kondenzátoru ("Code" zkráceně) určený pro záznam hlasu. Sennheiser AKG a dokonce novější mikrofonů přiblížení může přinést dobré výsledky. Můžete zakoupit položí nebo poskytovat do nájmu z místní audiovizuální pronájem podniku. Vyhledá jednu s rozhraním USB. Tento typ mic pohodlně kombinuje prvku mikrofon, preamp a obdobu jmenovek digitální převaděč do jednoho balíčku, která zjednodušuje jejich propojení.

Můžete také použít analogové mikrofon. Obsahuje mnoho pronájem nabízejí "ročníku" mikrofonů uznávané jejich hlasové znaku. Všimněte si, že profesionální analogové zařízení používá vyvážené XLR konektory, spíše než 1/4" připojit do zařízení uživatelů. Budete-li analogové, budete také potřebovat preamp a zvukové rozhraní počítače pomocí těchto konektorů.

Nainstalujte mikrofon samostatné nebo Vida a pop filtru mikrofon k odstranění šumu z "plosive" výslovnost "p" a "b". Některé mikrofony součástí pozastavení připojení, který izoluje je od vibrace ve stojan, což je užitečné.

Talentu hlasu musí zůstat na konzistentní vzdálenost od mikrofon. K označení, kde by měl být používejte pásku na dolní mez. Pokud se vám sedět dává přednost talentu, věnujte zvláštní pozornost monitorování vzdálenost povinná kontrola úrovně důvěryhodnosti a vyhnutí se zbytečnému vytváření řetězce.

Použijte v případě k uložení skriptu. Vyhněte se angling samostatné, takže ji můžete sledovat, zvuk směrem k mikrofonu.

Osoba, provozní záznamové – inženýr – by měla být v místnosti oddělené od talentu, s nějakým způsobem ke komunikaci s talentu v záznamu z mýtných bran ( *talkback okruhu).*

Záznam by měl obsahovat jako malé šum, jako je to možné, s cílem 80 signálu šumu poměr db nebo vyšší.

Úzce poslouchejte záznam nečinnosti v vaší "z mýtných bran," zjistit, kde pochází z jakékoli šum a odstranění příčiny. Běžné zdroje šumu jsou air otvory, fluorescenční světla předřadníky, provoz v blízkosti silnicích zakázána a fanoušky zařízení (i přenosné počítače pravděpodobně ventilátorů). Mikrofon a kabely můžete vyzvednutí elektrické zbytečných dat blízké AC zapojení, obvykle šum nebo žhavých novinek.

> [!TIP]
> V některých případech může být možné použít ekvalizér nebo softwaru snížení šumu modulu plug-in k odstranění šumu z vašich nahrávek, i když je vždy vhodné ho zastavit v jejich zdroji.

Nastavit úrovně tak, že většina dostupné dynamických rozsahů digitální záznam se používá bez overdriving. To znamená loud, ale ne tak loud, deformuje zvuku. Níže je příklad zvukového průběhu dobré záznamu.

![dobré záznam zvukového průběhu](media/custom-voice/good-recording.png)

Tady se používá většina rozsah (výška), ale nejvyšší špičky signál: nebylo dosaženo horní nebo dolní části okna. Zjistíte také, že blíží nečinnosti v záznamu tenká vodorovná čára označující floor nízké šumu. Tento záznam má přijatelné dynamickým a poměr signálu šumu.

Záznam přímo do počítače pomocí vysoce kvalitních zvukového rozhraní nebo USB port, v závislosti na mikrofon používáte. Pro obdobu jmenovek, zjednodušení zvuku řetězce: povinná kontrola úrovně důvěryhodnosti, preamp, zvukové rozhraní, počítač. Obě [Avid profesionálních nástrojů](http://www.avid.com/en/pro-tools) a [Adobe Audition](https://www.adobe.com/products/audition.html) je možné licencovat měsíčně za rozumnou cenu. Pokud váš rozpočet je velmi vysoké, zkuste bezplatnou [Audacity](https://www.audacityteam.org/).

Záznam na 44,1 KHz 16bitové monophonic (CD kvality) nebo vyšší. Aktuální stav systému – moderní je 48 KHz 24-bit, pokud je vaše zařízení podporuje. Budete převzorkovat zvuku na 16 KHz 16 bitů než ji odešlete k portálu vlastní hlasové. Stále platí mít vysoce kvalitní původního záznamu v případě, že změny jsou potřeba.

V ideálním případě mají různí lidé slouží v rolích ředitel, inženýr a talentu. Nepokoušejte se to udělat všechny sami! Prstů ředitel a technika může být jedna osoba.

### <a name="before-the-session"></a>Před relací

Aby se zabránilo plýtvání časem studio, spuštěn prostřednictvím skriptu pomocí vašeho talentu hlasového záznamu. Zatímco talentu hlasové přestane být obeznámeni s textem, nezíská zpřehlednit výslovnost neznámého slov.

> [!NOTE]
> Většina studios záznam nabízejí elektronických zobrazení skriptů v záznamu z mýtných bran. V takovém případě zadejte poznámky průběh prezentace přímo do vašeho skriptu dokumentu. Stále můžete dělat poznámky během relace, i když kopii dokumentu. Většina technici výtisk, příliš vhodné. A budete pořád potřebovat že třetí vytisknout kopii jako záložní pro talentu v případě, že počítač je vypnutý.

Váš hlas talentu požádat aplikaci word chcete, aby oznámil v utterance ("rozhodnou slovo"). Sdělte jim, že chcete fyzická čtení s žádné zvláštní důraz. Zvýraznění můžete přidat, pokud je syntetizovat řeči; neměl by být součástí původní záznam.

Přímé talentu k vyslovte slova odděleně. Každé slovo skript by měl výraznější, jak je uvedená. Zvuky by neměly být vynechán nebo slurred společně, což je běžné v příležitostné řeči *Pokud byla napsána tak ve skriptu.*

|Psaný text|Nežádoucí příležitostné výslovnost|
|-|-|
|nikdy pak vám|nikdy pak vám|
|Existují čtyři světla|Existují čtyři světla|
|jak se počasí dnes|jak se th "počasí ještě dnes|
|přivítejte přítele malý|Dejme tomu, že hello na můj lil "typu friend|

By měl talentu *není* přidat různé pozastaví mezi slovy. Věty by stále tok samozřejmě i v průběhu zvukově trochu formální. Toto rozlišení jemné může trvat postup, chcete-li získat správný.

### <a name="the-recording-session"></a>Záznam relace

Vytvořit odkaz na záznam, nebo *shoda souboru* z typických utterance na začátku relace. Požádejte talentu zopakovat tento řádek každé stránky nebo tak. Pokaždé, když, porovnejte má nový záznam odkaz. Tento postup pomáhá talentu zůstávají konzistentní vzhledem k aplikacím ve svazku, tempo, rozteč a intonací. Mezitím můžete inženýr použít soubor shoda jako odkaz pro úrovně a celkovou konzistenci zvuku.

Soubor porovnání je zvlášť důležité při obnovení nahrávání po přerušení, nebo na jiný den. Budete chtít přehrát jej několikrát pro talentů a potom kliknul zopakovat to pokaždé, když dokud je odpovídajících dobře.

Coach vašeho talentu hloubkové dech a pozastavení na chvíli před každou utterance. Zaznamenejte na několik sekund mezi projevy nečinnosti. Slova musí projevit stejný způsobem pokaždé, když se zobrazí, vzhledem k tomu kontextu: "někam" sloveso je jinak vyslovováno "záznam" jako podstatné jméno.

Zaznamenejte dobré pěti sekund od nečinnosti před první záznam pro zachycení "místnosti tónu." To pomáhá portálu vlastní hlasové kompenzovat všechny zbývající šumu v záznamu.

> [!TIP]
> Vše, co skutečně potřebujete je hlas talentu, abyste měli monophonic nahrávání (single kanál) nebo pouze jejich řádky. Pokud je záznam v stereo, ale můžete použít druhý kanál pro záznam chatter v řídicí místnosti k zachycení diskuzi o konkrétní řádky nebo trvá. Odeberte toto sledování z verze nahráli na portál vlastní hlasové.

Naslouchání úzce, použít sluchátka, talentů hlasové výkonu. Hledáte dobrá, ale přirozené diction, správnou výslovnost a nedostatečná nežádoucí zvuky. Neváhejte a požádejte vašeho talentu znovu zaznamenat utterance, která nesplňuje těchto standardů. 

> [!TIP] 
> Pokud používáte velké množství projevy, jeden utterance nemá znatelný vliv na výsledný vlastní hlasové. Proto může být více účelné jednoduše mějte na paměti jakékoli projevy problémů, vyloučit z datové sady a podívejte se jak vlastní hlasové ukázalo. Vždy můžete přejít zpět do nástroje studio a poznamenejte chybějící ukázky později.

Poznamenejte si číslo vzít nebo čas kód na váš skript pro každý utterance. Pokud se označit každý utterance v tento záznam metadat nebo startovací seznam také požádejte technik.

Bere regulární konce a poskytne můžete dát chvilku pauzu pomáhají vašeho talentu hlasové zachovat své hlasové v dobrém stavu.

### <a name="after-the-session"></a>Po relaci

Moderní záznam aplikace spustit na počítačích. Na konci relace obdržíte jednu nebo více zvukové soubory, ne na pásku. Tyto soubory budou pravděpodobně být ve formátu WAV nebo AIFF CD kvality (44,1 KHz 16 bitů) nebo novějším. 48 kHz 24 bitů je běžné a žádoucí. Vyšší míra vzorkování, jako je například 96 KHz, obvykle nejsou potřeba.

Vlastní hlasové portál vyžaduje každý zadaný utterance ve vlastním souboru. Zvukové soubory přijatých nástroje studio každý obsahovat více projevy. Proto je primární úloha postprodukční Rozdělit záznamy a jejich přípravě k odeslání. Záznam inženýr pravděpodobně umístěny značky v souboru (nebo samostatné startovacího seznamu k dispozici) k označení, kde začíná každý utterance.

Použití poznámek k nalezení přesně přejdete a pak používat zvuk, nástroje pro úpravy, jako [Avid profesionálních nástrojů](http://www.avid.com/en/pro-tools), [Adobe Audition](https://www.adobe.com/products/audition.html), nebo bezplatnou [Audacity](https://www.audacityteam.org/) kopírování jednotlivých utterance do nového souboru.

Ponechte pouze o 0,2 sekundách nečinnosti na začátku a konce každého klipu s výjimkou prvního. Tento soubor by měl začínat plně pěti sekundách nečinnosti. Nepoužívejte zvuku editor "Vynulovat" tiché části souboru. Vlastní hlasové algoritmy kompenzovat všechny šum na pozadí plyne ze zbytkových pomůže včetně tón"místo".

Každý soubor poslouchejte pečlivě. V této fázi můžete upravit malé nežádoucí zvuky, které jste zmeškali během nahrávání jako lehká lip smack před řádku, ale dejte pozor, abyste odebrat všechny skutečné řeči. Soubor nelze opravit, odeberte ze sady dat, provádění poznámku, že jste tak učinili.

Převést každý soubor na 16 bitů a vzorkovací frekvence 16 kHz před uložením a pokud jste si poznamenali studio chatter, odeberte druhý kanál. Každý soubor uložte ve formátu WAV pojmenování souborů s číslem utterance z vašeho skriptu.

Nakonec vytvořte *přepisu* , která přidruží jednotlivých souborů WAV textovou verzi toho odpovídající utterance. [Vytvoření vlastního hlasového písma](how-to-customize-voice-font.md) obsahuje podrobnosti o požadovaném formátu. Zkopírujte text přímo z vašeho skriptu. Vytvořte soubor ZIP souborů WAV a text přepisu.

V případě, že je budete později potřebovat můžete archivujte původní záznamy na bezpečném místě. Skript a poznámky, zachovat příliš.

## <a name="next-steps"></a>Další postup

Jste připraveni nahrát vašich nahrávek a vytvořit svůj vlastní hlas!

> [!div class="nextstepaction"]
> [Vytvoření vlastního hlasového písma](how-to-customize-voice-font.md)
