---
title: Co je Azure Machine Learning Studio? | Dokumenty Microsoft
description: Přehled nástroje Azure ML Studio, který umožňuje přetahováním rychle vytvářet modely z předpřipravených knihoven algoritmů a modulů
keywords: azure machine learning,azure ml,ml studio
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/28/2018
ms.openlocfilehash: 9911d56807a95a5f8199cecdff13ad06497fb33e
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34836949"
---
# <a name="what-is-azure-machine-learning-studio"></a>Co je Azure Machine Learning Studio?
Microsoft Azure Machine Learning Studio je nástroj pro spolupráci, ve kterém je možné přetahováním vytvářet, testovat a nasazovat řešení prediktivní analýzy dat. Machine Learning Studio publikuje modely jako webové služby, které je možné snadno využívat ve vlastních aplikacích nebo nástrojích BI, například v Excelu.

Machine Learning Studio je místo, kde se setkává datová věda, prediktivní analýza, cloudové prostředky a vaše data.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>Interaktivní pracovní prostor Machine Learning Studio
K vývoji modelu prediktivní analýzy zpravidla použijete data z jednoho nebo více zdrojů, transformujete a analyzujete je prostřednictvím různých statistických funkcí a funkcí pro manipulaci s daty a vygenerujete sadu výsledků. Vývoj takového modelu je iterativní proces. Úpravou různých funkcí a jejich parametrů se výsledky zpřesňují, dokud nebudete přesvědčeni, že máte natrénován efektivní model.

**Azure Machine Learning Studio** nabízí interaktivní vizuální pracovní prostor, abyste mohli snadno vytvářet, testovat a iterovat model prediktivní analýzy. Při práci přetahujete ***datové sady*** a analytické ***moduly*** na interaktivní ***plátno***, kde je vzájemně propojujete. Tím vzniká experiment, který pak spustíte v nástroji Machine Learning Studio. Iterace návrhu modelu probíhá tak, že experiment upravíte, v případě potřeby uložíte kopii, a spustíte jej znovu. Jakmile budete připraveni, můžete ***výukový experiment*** převést na ***prediktivní experiment*** a publikovat jej jako ***webovou službu***, aby k vašemu modelu měli přístup i jiní uživatelé.

Nevyžaduje se žádné programování – model prediktivní analýzy se konstruuje vizuálním propojováním datových sad a modulů.

> [!TIP]
> Pokud si chcete stáhnout a vytisknout diagram s přehledem funkcí nástroje Machine Learning Studio, nahlédněte do tématu [Diagram s přehledem možností nástroje Machine Learning Studio](studio-overview-diagram.md).
> 
> 

![Diagram nástroje Azure ML Studio: Vytváření experimentů, čtení dat z mnoha zdrojů, zápis dat se stanoveným skóre, zápis modelů][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Začínáme s nástrojem Machine Learning Studio
Když poprvé vstoupíte do nástroje [Machine Learning Studio](https://studio.azureml.net), zobrazí se stránka **Domů**. Zde si můžete zobrazit dokumentaci, videa a webináře a najdete tu i další přínosné materiály.

Vlevo nahoře klikněte na nabídku ![Nabídka](./media/what-is-ml-studio/menu.png) a zobrazí se několik možností.

### <a name="cortana-intelligence"></a>Cortana Intelligence
Po kliknutí na **Cortana Intelligence** přejdete na domovskou stránku sady [Cortana Intelligence Suite](https://www.microsoft.com/cloud-platform/cortana-intelligence-suite). Cortana Intelligence Suite je plně spravovaná sada pro pokročilou analýzu velkých objemů dat, která vám umožní získat z dat užitečné informace. Úplnou dokumentaci, včetně příběhů zákazníků, najdete na domovské stránce sady.

### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
Tady jsou dvě možnosti – **Domů**, což je stránka, na které jste začali, a **Studio**.

Kliknutím na **Studio** přejdete do nástroje **Azure Machine Learning Studio**. Nejprve se zobrazí výzva, abyste se přihlásili účtem Microsoft, případně školním nebo pracovním účtem. Jakmile se přihlásíte, na levé straně uvidíte následující karty:

* **PROJEKTY** – Kolekce experimentů, datových sad, poznámkových bloků a jiných prostředků představující jeden objekt
* **EXPERIMENTY** – Experimenty, které jste vytvořili a spustili nebo uložili jako koncepty
* **WEBOVÉ SLUŽBY** – Webové služby, které jste nasadili z experimentů
* **POZNÁMKOVÉ BLOKY** – Jupyter Notebooks, které jste vytvořili
* **DATOVÉ SADY** – Datové sady, které jste nahráli do nástroje Studio
* **TRÉNOVANÉ MODELY** – Modely, které jste v experimentech natrénovali a uložili je v nástroji Studio
* **NASTAVENÍ** – Kolekce nastavení, kterou můžete použít ke konfiguraci účtu a prostředků

### <a name="gallery"></a>Galerie
Kliknutím na **Galerie** přejdete na ke **[galerii Azure AI](http://gallery.cortanaintelligence.com/)**. Galerie je místo, kde komunita datových vědců a vývojářů sdílí řešení vytvořená pomocí komponent sady Cortana Intelligence Suite.

Další informace o této galerii najdete v tématu věnovaném [sdílení a hledání řešení v galerii Azure AI](gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Komponenty experimentu
Experiment sestává z datových sad, které poskytují data analytickým modulům. Ty pak lze vzájemně propojit a vytvořit tak model prediktivní analýzy. Platný experiment má konkrétně tyto charakteristiky:

* Experiment obsahuje alespoň jednu datovou sadu a jeden modul.
* Datová sady je možné připojit jen k modulům.
* Moduly je možné připojovat jak k datovým sadám, tak k jiným modulům.
* Všechny vstupní porty modulů musí určitým způsobem souviset s tokem dat.
* Musí být nastaveny všechny povinné parametry jednotlivých modulů.

Experiment můžete vytvořit zcela od začátku, ale také můžete využít existující ukázku jako šablonu. Další informace najdete v tématu [Vytváření nových experimentů se strojovým učením na základě kopírování příkladů experimentů](sample-experiments.md).

Příklad vytvoření jednoduchého experimentu najdete v tématu [Vytvoření jednoduchého experimentu v nástroji Azure Machine Learning Studio](create-experiment.md).

Obsáhlejší návod, jak vytvořit řešení prediktivní analýzy, najdete v tématu o [vývoji prediktivního řešení s Azure Machine Learningem](walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>Datové sady
Datová sada obsahuje data, která byla nahrána do nástroje Machine Learning Studio, aby je bylo možné použít v procesu modelování. Součástí nástroje Machine Learning Studio je několik ukázkových datových sad, se kterými můžete experimentovat. Dle potřeby můžete nahrávat další datové sady. Zde jsou některé příklady dodávaných datových sad:

* **Data o spotřebě paliv u různých automobilů** – Hodnoty spotřeby paliva u automobilů na jednotku vzdálenosti (MPG) stanovené podle počtu válců, koňských sil apod.
* **Data o rakovině prsu** – Diagnostická data rakoviny prsu
* **Data o lesních požárech** – Rozsahy lesních požárů v severovýchodním Portugalsku

Během vytváření experimentu je možné si vybírat ze seznamu datových sad, které jsou k dispozici v nabídce nalevo od plátna.

Seznam ukázkových datových sad obsažených v nástroji Machine Learning Studio, najdete v tématu o [využití ukázkových datových sad v nástroji Azure Machine Learning Studio](use-sample-datasets.md).

### <a name="modules"></a>Moduly
Modul je algoritmus, který je možné provést na datech. Machine Learning Studio obsahuje několik modulů od funkcí pro příjem dat po procesy trénování, stanovení skóre a ověřování. Zde jsou některé příklady dodávaných modulů:

* [Převod na ARFF][convert-to-arff] – Převede serializovanou datovou sadu .NET do formátu ARFF (Attribute-Relation File Format).
* [Výpočet základních statistik][elementary-statistics] – Vypočítá základní statistiky, jako je střední hodnota, směrodatná odchylka atd.
* [Lineární regrese][linear-regression] – Vytvoří online model lineární regrese na základě klesání gradientu.
* [Určení skóre modelu][score-model] – Stanoví skóre pro trénovaný klasifikační nebo regresní model.

Během vytváření experimentu je možné si vybírat ze seznamu modulů, které jsou k dispozici v nabídce nalevo od plátna.  

Modul může obsahovat sadu parametrů, pomocí kterých je možné konfigurovat jeho vnitřní algoritmy. Když na plátnu vyberete modul, parametry modulu se zobrazí v podokně **Vlastnosti** napravo od plátna. Úpravou parametrů v tomto podokně můžete model optimalizovat.

S procházením rozsáhlé knihovny dostupných algoritmů strojového učení vám pomůže téma [Jak zvolit algoritmy pro Microsoft Azure Machine Learning](algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Nasazení webové služby prediktivní analýzy
Jakmile je váš model prediktivní analýzy připraven, můžete jej přímo v nástroji Machine Learning Studio nasadit jako webovou službu. Další podrobnosti k tomuto procesu najdete v tématu [Nasazení webové služby Azure Machine Learning](publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/what-is-ml-studio/azure-ml-studio-diagram.jpg



## <a name="key-machine-learning-terms-and-concepts"></a>Klíčová terminologie a koncepty strojového učení
Pojmy z oblasti strojového učení nemusí být na první pohled úplně jasné. Ke klíčovým pojmům zde najdete definice, které vám leccos objasní. Do komentářů dole na stránce nám můžete napsat, jaké další termíny byste chtěli definovat.

### <a name="data-exploration-descriptive-analytics-and-predictive-analytics"></a>Zkoumání dat, deskriptivní analýza a prediktivní analýza

**Zkoumání dat** je proces shromažďování informací o velké a často nestrukturované datové sadě s cílem najít charakteristiky pro přesně zacílenou analýzu.

**Dolování dat** označuje automatizované zkoumání dat.

**Deskriptivní analýza** je proces analyzování datové sady s cílem shrnout, co se stalo. Velká většina obchodních analýz – například prodejní sestavy, webové metriky a analýzy sociálních sítí – je deskriptivních.

**Prediktivní analýza** je proces vytváření modelů na základě historických nebo aktuální dat s cílem předpovídat budoucí výsledky.

### <a name="supervised-and-unsupervised-learning"></a>Učení se supervizí a bez supervize
 V algoritmech **učení se supervizí** probíhá trénink na označených datech – jinými slovy na datech zahrnujících příklady žádoucích odpovědí. Například v modelu, který identifikuje podvodné použití platební karty, by trénink probíhal na datové sadě s označenými datovými body transakcí, o kterých se ví, že byly, resp. nebyly podvodné. Většina strojového učení probíhá se supervizí.

 **Učení bez supervize** probíhá na neoznačených datech a cílem je nalezení vztahů a souvislostí v nich. Cílem může být například nalezení demograficky definovaných skupin zákazníků s podobnými nákupními návyky.

### <a name="model-training-and-evaluation"></a>Trénování a hodnocení modelů
Model strojového učení je abstrakcí otázky, na kterou hledáte odpověď, nebo výsledku, který chcete předpovědět. Modely se trénují a vyhodnocují na základě existujících dat.

#### <a name="training-data"></a>Data pro trénink
Při tréninku modelů používáte známou datovou sadu a podle charakteru dat model upravujete tak, abyste co nejvíce zpřesnili jeho výstupy. V Azure Machine Learning se model sestavuje z algoritmického modulu, který zpracovává trénovací data a funkční moduly, například modul pro stanovení skóre.

Pokud v rámci učení se supervizí trénujete model pro odhalování podvodů, používáte sadu transakcí, které jsou označeny jako podvodné nebo platné. Datovou sadu náhodně rozdělíte a pak jednu část použijete k natrénování modelu a další část k otestování nebo vyhodnocení modelu.

#### <a name="evaluation-data"></a>Data pro vyhodnocení
Jakmile model natrénujete, vyhodnotíte ho na zbývajících testovacích datech. Využijete k tomu data, u kterých již znáte výsledky, aby bylo možné určit, zda je váš model schopen přesné predikce.

## <a name="other-common-machine-learning-terms"></a>Další běžné pojmy používané při strojovém učení
* **algoritmus**: Samostatná sada pravidel používaná k řešení problémů prostřednictvím zpracování dat, matematických výpočtů nebo automatizovaného posuzování.
* **detekce anomálií**: Model, který nachází a označuje neobvyklé události či hodnoty a pomáhá objevovat problémy. Detektor podvodů s platebními kartami například vyhledává nezvyklé nákupy.
* **kategorizovaná data**: Data, která jsou uspořádána do kategorií a která lze rozdělit do skupin. Kategorizovaná datová sada pro automobily by například mohla specifikovat rok výroby, značku, model a cenu.
* **klasifikace**: Model pro uspořádání datových bodů do kategorií na základě datové sady, u které je již seskupení do kategorií známo.
* **konstruování příznaků**: Proces extrahování nebo výběru příznaků či atributů souvisejících s datovou sadou s cílem zvýšit její využitelnost a vylepšit výsledky. Například využitelnost dat o leteckých tarifech lze vylepšit určením dnů v týdnu a svátků. Viz [Výběr a konstruování příznaků v Azure Machine Learning](../team-data-science-process/create-features.md).
* **modul**: Funkční součást modelu v nástroji Machine Learning Studio, například modul pro zadávání dat, který umožňuje zadávat a upravovat malé datové sady. Určitým typem modulu v nástroji Machine Learning Studio je i algoritmus.
* **model**: Model v rámci učení se supervizí představuje produkt experimentu strojového učení a skládá se z dat pro trénink, algoritmického modulu a funkčních modulů, jako je modul Určení skóre modelu.
* **číselná data**: Data, která mají význam jako měření (spojitá data) nebo počty (diskrétní neboli nespojitá data). Označují se také jako *kvantitativní data*.
* **rozdělení**: Metoda, podle které data rozdělíte do vzorků. Další informace naleznete v tématu [Rozdělení a vzorky](https://msdn.microsoft.com/library/azure/dn905960.aspx).
* **predikce**: Hodnota nebo hodnoty předpovězené z modelu strojového učení. Možná jste se také setkali s termínem „predikované skóre“. To však nepředstavuje konečný výstup modelu. Podle skóre se řídí vyhodnocení modelu.
* **regrese**: Model pro predikci hodnoty na základě nezávislých proměnných, například předpověď ceny automobilu na základě roku výroby a značky.
* **skóre**: Předpovězená hodnota generovaná z natrénovaného klasifikačního nebo regresního modelu na základě [modulu pro stanovení skóre](https://msdn.microsoft.com/library/azure/dn905995.aspx) v nástroji Machine Learning Studio. Klasifikační modely rovněž vracejí skóre pro pravděpodobnost předpovězené hodnoty. Po vygenerování skóre z modelu můžete vyhodnotit jeho přesnost pomocí [modulu pro vyhodnocení modelu](https://msdn.microsoft.com/library/azure/dn905915.aspx).
* **vzorek**: Část datové sady, která by měla představovat celek. Vzorky lze vybírat náhodně nebo podle konkrétních příznaků či atributů datové sady.

## <a name="next-steps"></a>Další kroky
Se základy prediktivní analýzy a strojového učení se můžete seznámit v [podrobném kurzu](create-experiment.md) a [na základě ukázek](sample-experiments.md).  


<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
