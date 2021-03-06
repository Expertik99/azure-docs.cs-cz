---
title: Doba příjmu dat ve službě Azure Log Analytics | Dokumentace Microsoftu
description: Vysvětluje různé faktory, které mají vliv latence shromažďování dat ve službě Azure Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/10/2018
ms.author: bwren
ms.openlocfilehash: 0e513cc4f6a7d5d030ded807870de9eb0fdc0ed8
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972639"
---
# <a name="data-ingestion-time-in-log-analytics"></a>Doba příjem dat v Log Analytics
Azure Log Analytics je služba data velkém měřítku, která slouží tisíce zákazníků odesílání terabajty dat měsíčně rostoucí tempem. Jsou často dotazy týkající se čas potřebný pro data k dispozici ve službě Log Analytics po shromáždění zpracovat. Tento článek vysvětluje různé faktory ovlivňující tuto latenci.

## <a name="typical-latency"></a>Typické latence
Latence odkazuje na čas, který data se vytvoří v monitorovaném systému a čas, který je k dispozici pro analýzy ve službě Log Analytics. Typické latenci ingestovat data do Log Analytics je od 3 do 10 minut, s 95 % dat přijatých za méně než 7 minut. Konkrétní latence pro všechna data, zejména se liší v závislosti na řadě faktorů, které je popsáno níže.

## <a name="sla-for-log-analytics"></a>Smlouva SLA pro službu Log Analytics
[Log Analytics Service smlouva o úrovni (SLA)](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_1/) je smlouva právní vazby, který definuje, kdy Microsoft náhrad zákazníkům Pokud služba nesplňuje svých cílů. To není založena na typické výkonu systému, ale jeho nejhorším případě účty pro potenciální katastrofické situacích.

## <a name="factors-affecting-latency"></a>Faktory ovlivňující latence
Ingestování celkový čas pro konkrétní sadu data dají rozdělit do následujících oblastech vyšší úrovně. 

- Agent čas – čas zjištění událost, shromažďování vyžadováno a následně je odeslat do Log Analytics ingestování bod jako záznam protokolu. Ve většině případů se tento proces zařizuje Služba agenta.
- Kanál čas – čas potřebný pro ingestování kanálu ke zpracování záznamu protokolu. Jedná se o analýze vlastnosti události a potenciálně přidáním počítaných informací.
- Indexování čas – čas strávený umožňující ingestování záznam protokolu do úložišti velkých objemů dat Log Analytics.

Podrobnosti o různých latencí v tomto procesu jsou popsané níže.

### <a name="agent-collection-latency"></a>Latence kolekce agenta
Agenti a řešení pro správu používají různé strategie ke shromažďování dat z virtuálního počítače, který může mít vliv na latenci. Některé konkrétní příklady patří:

- Okamžitě se shromažďují události Windows, události procesu syslog a metriky výkonu. Čítače výkonu systému Linux jsou dotazovat každých 30 sekund.
- Jakmile se změní jejich časového razítka se shromažďují protokoly služby IIS a vlastní protokoly. Pro protokoly služby IIS, to je ovlivněno [nakonfigurovaný ve službě IIS plán výměny](log-analytics-data-sources-iis-logs.md). 
- Řešení Active Directory replikace provede posouzení každých pět dní, během řešení Active Directory Assessment provádí týdenní hodnocení infrastruktury služby Active Directory. Agent bude shromažďovat protokoly pouze po dokončení hodnocení.

### <a name="agent-upload-frequency"></a>Frekvence odesílání agenta
K zajištění, že agenta Log Analytics je jednoduché, agent ukládá do vyrovnávací paměti protokoly a pravidelně je odesílá do služby Log Analytics. Nahrát frekvence se pohybuje mezi 30 sekund a 2 minuty. záleží na typu dat. Většina dat je nahraný v části 1 minuta. Stavy sítě může mít nepříznivý vliv latence těchto dat k dosažení bodem ingestování Log Analytics.

### <a name="azure-logs-and-metrics"></a>Metriky a protokoly Azure 
Data protokolu aktivit, bude trvat přibližně 5 minut do režimu k dispozici ve službě Log Analytics. Data z diagnostické protokoly a metriky může trvat 1 – 5 minut, než bude k dispozici, v závislosti na službu Azure. Pak bude trvat dalších 30 – 60 sekund pro protokoly a metriky pro data 3 minuty, k odeslání do Log Analytics ingestování datových bodů.

### <a name="management-solutions-collection"></a>Kolekce řešení správy
Některá řešení neshromažďují svá data z agenta a mohou používat metodu kolekce, která zavádí další latenci. Některá řešení bez pokusu o kolekci téměř v reálném čase shromažďovat data v pravidelných intervalech. Konkrétní příklady patří:

- Řešení pro Office 365 se dotazuje správu aktivity rozhraní API Office 365, který aktuálně neposkytuje záruky latence jakékoli téměř v reálném čase pomocí protokolů aktivit.
- Řešení na denní frekvence shromažďuje data řešení (informace o kompatibilitě pro příklad) Windows Analytics.

Naleznete v dokumentaci pro každé řešení určit její interval shromažďování.

### <a name="pipeline-process-time"></a>Čas zpracování kanálu
Jakmile jsou záznamy protokolu přijatých do kanálu Log Analytics, jsou napsaná do dočasného úložiště zajistit izolaci klientů a ujistěte se, že data nejsou ztracena. Tento postup přidá obvykle 5 až 15 sekund. Některá řešení pro správu implementují těžší algoritmy ke shromáždění dat a vyvoďte z nich jako streamování dat v. Například monitorování výkonu sítě agreguje příchozích dat přes 3minutové intervaly efektivně přidání 3minutové latence.

### <a name="new-custom-data-types-provisioning"></a>Nové vlastní datové typy zřizování
Při vytvoření nového typu vlastních dat z [vlastního protokolu](../log-analytics/log-analytics-data-sources-custom-logs.md) nebo [rozhraní API kolekce dat](../log-analytics/log-analytics-data-collector-api.md), systém vytvoří kontejner vyhrazeného úložiště. Toto je jednorázová režijní náklady, ke které dochází pouze na první výskyt tento typ dat.

### <a name="surge-protection"></a>Nárůst ochrany
Hlavní prioritou Log Analytics je zajistit, že žádná zákaznická data nejsou ztracena, tak systém obsahuje integrovanou ochranu pro data nárůstů. To zahrnuje vyrovnávací paměti k zajištění, že i v rámci obrovské zatížení, systém bude nadále funkční. Při normálním zatížení tyto ovládací prvky přidat méně než minutu, ale v extrémní podmínky a selhání, přidat spoustu času, zatímco je zajištěna dat je bezpečné.

### <a name="indexing-time"></a>Indexování čas
Je integrované vyrovnávání pro každou platformu pro velké objemy dat mezi analytics a na rozdíl od okamžitý přístup k datům poskytuje pokročilé vyhledávací funkce. Azure Log Analytics umožňuje spouštět výkonné dotazy na miliard záznamů a získání výsledků během pár sekund. To je provést je to možné, protože infrastruktury výrazně transformuje data během jeho ingestování a ukládá ho do struktury jedinečné compact. Systém ukládá do vyrovnávací paměti dat. dokud dost není k dispozici k vytvoření těchto struktur. Je nutné dokončit předtím, než se ve výsledcích hledání zobrazí záznam protokolu.

Tento proces aktuálně trvá přibližně 5 minut. Pokud je malé množství dat, ale méně času na vyšší datové sazby. To zdá být neintuitivní, ale tento proces umožní optimalizace v případě velkého objemu produkční úlohy.



## <a name="checking-ingestion-time"></a>Kontrola ingestování času
Můžete použít **prezenčního signálu** tabulky k výpočtu odhadu latence pro data z agentů. Od prezenčního signálu je po odeslání minutu, rozdíl mezi aktuálním časem a poslední záznam prezenčního signálu v ideálním případě bude co nejblíže minutu nejvíce.

Použijte tento dotaz můžete vytvořit seznam počítačů s nejvyšší latencí.

    Heartbeat 
    | summarize IngestionTime = now() - max(TimeGenerated) by Computer 
    | top 50 by IngestionTime asc

 
Pomocí následujícího dotazu ve velkých prostředích shrnutí latence pro různé procenta celkový počet počítačů.

    Heartbeat 
    | summarize IngestionTime = now() - max(TimeGenerated) by Computer 
    | summarize percentiles(IngestionTime, 50,95,99)



## <a name="next-steps"></a>Další postup
* Čtení [smlouvu o úrovni (SLA) služeb](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_1/) ke službě Log Analytics.

