---
title: Azure Monitor podporované metriky podle typu prostředku
description: Seznam metrik, které jsou dostupné pro jednotlivé typy prostředků pomocí Azure monitoru.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 03/30/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 3219f8e61a0aa469775a972e6b240eb2069c2cd9
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929963"
---
# <a name="supported-metrics-with-azure-monitor"></a>Podporované metriky ve službě Azure Monitor
Platforma Azure Monitor poskytuje několik způsobů, jak pracovat s metriky, včetně grafů na portálu, přístup přes rozhraní REST API nebo dotazování je pomocí Powershellu nebo rozhraní příkazového řádku. Níže je úplný seznam všech metrik aktuálně k dispozici pro monitorování Azure metriky kanálu. Jiné metriky, může být k dispozici na portálu nebo pomocí starší verze rozhraní API. Tento seznam níže obsahuje pouze metriky, které jsou k dispozici prostřednictvím konsolidované kanálu metrik Azure monitoru. K vyhledání a přístup k těmto metrikám prosím použijte [2018-01-01 verze api-version](https://docs.microsoft.com/rest/api/monitor/metricdefinitions)

> [!NOTE]
> Odesílání vícedimenzionálních metrik přes nastavení diagnostiky se v současné době nepodporuje. Metriky s dimenzemi se exportují jako ploché jednodimenzionální metriky agregované napříč hodnotami dimenzí.
>
> *Příklad:* Metriku Příchozí zprávy v centru událostí je možné zkoumat a převést na graf na úrovni jednotlivých front. Pokud se však metrika exportuje přes nastavení diagnostiky, bude reprezentovaná jako všechny příchozí zprávy ve všech frontách v centru událostí.
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|qpu_metric|QPU|Počet|Průměr|QPU. Rozsah 0 až 100 pro S1, 0 až 200 pro S2 a 0 až 400 pro S4|ServerResourceType|
|memory_metric|Memory (Paměť)|B|Průměr|Paměť. V rozsahu 0-25 GB pro S1, 0 – 50 GB pro S2 a 0 – 100 GB pro S4|ServerResourceType|
|TotalConnectionRequests|Celkem žádostí o spojení|Počet|Průměr|Celkem žádostí o spojení. Jedná se o doručení.|ServerResourceType|
|SuccessfullConnectionsPerSec|Úspěšná spojení za sekundu|CountPerSecond|Průměr|Míra úspěšně navázaných spojení.|ServerResourceType|
|TotalConnectionFailures|Celkem nezdařených spojení|Počet|Průměr|Celkový počet neúspěšných pokusů o připojení.|ServerResourceType|
|CurrentUserSessions|Aktuální uživatelské relace|Počet|Průměr|Aktuální počet navázaných uživatelských relací.|ServerResourceType|
|QueryPoolBusyThreads|Zaneprázdněná vlákna fondu dotazů|Počet|Průměr|Počet zaneprázdněných vláken ve fondu vláken dotazů.|ServerResourceType|
|CommandPoolJobQueueLength|Délka fronty fondu úloh příkazu|Počet|Průměr|Počet úloh ve frontě fondu vláken příkazů.|ServerResourceType|
|ProcessingPoolJobQueueLength|Délka fronty fondu úloh zpracování|Počet|Průměr|Počet úloh bez vstupně-ve frontě fondu vláken zpracování.|ServerResourceType|
|CurrentConnections|Připojení: Aktuální počet připojení|Počet|Průměr|Aktuální počet navázaných připojení klientů.|ServerResourceType|
|CleanerCurrentPrice|Paměť: Aktuální cena čisticího modulu|Počet|Průměr|Aktuální cena paměti a $/ bajt/čas, normalizovaná na 1000.|ServerResourceType|
|CleanerMemoryShrinkable|Paměť: Zmenšitelná paměť čisticího modulu|B|Průměr|Velikost paměti v bajtech, která čisticí procesem na pozadí se vyprázdňuje.|ServerResourceType|
|CleanerMemoryNonshrinkable|Paměť: Nezmenšitelná paměť|B|Průměr|Velikost paměti v bajtech, která není v souladu s čisticí vyprazdňování procesem na pozadí.|ServerResourceType|
|Parametru MemoryUsage|Paměť: Využití paměti|B|Průměr|Využití paměti procesu serveru, jak se používají při výpočtu cena čisticího modulu paměti. Rovnat čítači Process\PrivateBytes plus velikost dat mapovaných do paměti, ignoruje se jakákoli paměť, která byla mapována nebo přidělena analýzu v paměti modulu xVelocity (VertiPaq) Limit paměti modulu xVelocity.|ServerResourceType|
|MemoryLimitHard|Paměť: Limit paměti – pevná|B|Průměr|Limit pevné paměti, z konfiguračního souboru.|ServerResourceType|
|Hodnota MemoryLimitHigh|Paměť: Limit paměti – vysoká|B|Průměr|Limit vysoké paměti, z konfiguračního souboru.|ServerResourceType|
|MemoryLimitLow|Paměť: Limit paměti – nízká|B|Průměr|Limit nízké paměti, z konfiguračního souboru.|ServerResourceType|
|MemoryLimitVertiPaq|Paměť: Limit paměti – VertiPaq|B|Průměr|Limit v paměti, z konfiguračního souboru.|ServerResourceType|
|Kvóta|Paměť: kvóta|B|Průměr|Aktuální kvóta paměti, v bajtech. Kvóta paměti se taky říká rezervace paměti grant nebo paměti.|ServerResourceType|
|QuotaBlocked|Paměť: Kvóta – blokováno|Počet|Průměr|Aktuální počet požadavků kvóty, které jsou blokovány, dokud jsou uvolněny jiné kvóty paměti.|ServerResourceType|
|VertiPaqNonpaged|Paměť: VertiPaq nestránkované|B|Průměr|Počet bajtů paměti uzamčených v pracovní sadě pro použití modulem v paměti.|ServerResourceType|
|VertiPaqPaged|Paměť: VertiPaq stránkované|B|Průměr|Počet bajtů stránkované paměti používaných pro data v paměti.|ServerResourceType|
|RowsReadPerSec|Zpracování: Počet přečtených řádků za sekundu|CountPerSecond|Průměr|Rychlost čtení řádků ze všech relačních databází.|ServerResourceType|
|RowsConvertedPerSec|Zpracování: Řádky převést za sekundu|CountPerSecond|Průměr|Rychlost převodu řádků během zpracování.|ServerResourceType|
|RowsWrittenPerSec|Zpracování: Počet zapsaných řádků za sekundu|CountPerSecond|Průměr|Rychlost zápisu řádků během zpracování.|ServerResourceType|
|CommandPoolBusyThreads|Vlákna: Zaneprázdněná vlákna fondu příkazů|Počet|Průměr|Počet zaneprázdněných vláken ve fondu vláken příkazů.|ServerResourceType|
|CommandPoolIdleThreads|Vlákna: Nečinná vlákna fondu příkazů|Počet|Průměr|Počet nečinných vláken ve fondu vláken příkazů.|ServerResourceType|
|LongParsingBusyThreads|Vlákna: Zaneprázdněná vlákna dlouhého parsování|Počet|Průměr|Počet zaneprázdněných vláken ve fondu vláken dlouhého parsování.|ServerResourceType|
|LongParsingIdleThreads|Vlákna: Nečinná vlákna dlouhého parsování|Počet|Průměr|Počet nečinných vláken ve fondu vláken dlouhého parsování.|ServerResourceType|
|LongParsingJobQueueLength|Vlákna: Dlouhého parsování délka fronty úloh|Počet|Průměr|Počet úloh ve frontě fondu vláken dlouhého parsování.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|Vlákna: Zaneprázdněná vlákna úloh vstupně-výstupní operace fondu zpracování|Počet|Průměr|Počet vláken ve fondu vláken zpracování spuštění vstupně-výstupních operací úloh.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|Vlákna: Zaneprázdněná vlákna jiných vstupně-fondu zpracování|Počet|Průměr|Počet vláken, spouštění úloh bez vstupně-ve fondu vláken zpracování.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|Vláken: Vstupně-výstupních operací délka fronty úloh fondu zpracování|Počet|Průměr|Počet vstupně-výstupních operací úloh ve frontě fondu vláken zpracování.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|Vlákna: Nečinná vlákna úloh vstupně-výstupní operace fondu zpracování|Počet|Průměr|Počet nečinných vláken pro vstupně-výstupní úlohy do fondu vláken zpracování.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|Vlákna: Nečinná vlákna jiných vstupně-fondu zpracování|Počet|Průměr|Počet nečinných vláken ve fondu vláken zpracování vyhrazeném pro úlohy bez vstupně.|ServerResourceType|
|QueryPoolIdleThreads|Vlákna: Nečinná vlákna fondu dotazů|Počet|Průměr|Počet nečinných vláken pro vstupně-výstupní úlohy do fondu vláken zpracování.|ServerResourceType|
|QueryPoolJobQueueLength|Vlákna: Dotazování élka fronty fondu úloh|Počet|Průměr|Počet úloh ve frontě fondu vláken dotazů.|ServerResourceType|
|ShortParsingBusyThreads|Vlákna: Zaneprázdněná vlákna krátkého parsování|Počet|Průměr|Počet zaneprázdněných vláken ve fondu vláken krátkého parsování.|ServerResourceType|
|ShortParsingIdleThreads|Vlákna: Nečinná vlákna krátkého parsování|Počet|Průměr|Počet nečinných vláken ve fondu vláken krátkého parsování.|ServerResourceType|
|ShortParsingJobQueueLength|Vlákna: Krátkého parsování délka fronty úloh|Počet|Průměr|Počet úloh ve frontě fondu vláken krátkého parsování.|ServerResourceType|
|memory_thrashing_metric|Thrashing paměti|Procento|Průměr|Průměrný thrashing paměti.|ServerResourceType|
|mashup_engine_qpu_metric|QPU modulu M|Počet|Průměr|Využití QPU procesy modulu mashupu|ServerResourceType|
|mashup_engine_memory_metric|Paměť modulu M|B|Průměr|Využití paměti procesy modulu mashupu|ServerResourceType|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TotalRequests|Požadavky brány celkem|Počet|Celkem|Počet požadavků brány|Umístění, název hostitele|
|SuccessfulRequests|Úspěšné požadavky brány|Počet|Celkem|Počet úspěšné požadavky brány|Umístění, název hostitele|
|UnauthorizedRequests|Neautorizované požadavky brány|Počet|Celkem|Počet Neautorizované požadavky brány|Umístění, název hostitele|
|FailedRequests|Požadavky na brány po nezdařeném nasazení|Počet|Celkem|Počet selhání v požadavky brány|Umístění, název hostitele|
|OtherRequests|Ostatní požadavky brány|Počet|Celkem|Počet ostatní požadavky brány|Umístění, název hostitele|
|Doba trvání|Celková doba trvání požadavků na bránu|Milisekundy|Průměr|Celková doba trvání z požadavky brány v milisekundách|Umístění, název hostitele|
|Kapacita|Kapacita (verze Preview)|Procento|Průměr|Metriky využití pro službu ApiManagement|Umístění|

## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TotalJob|Celkový počet úloh|Počet|Celkem|Celkový počet úloh|Sady Runbook, stav|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|CoreCount|Počet vyhrazených jader|Počet|Celkem|Celkový počet vyhrazených jader v účtu batch|Žádné dimenze|
|TotalNodeCount|Počet vyhrazených uzlů|Počet|Celkem|Celkový počet vyhrazených uzlů v účtu batch|Žádné dimenze|
|LowPriorityCoreCount|Počet jader LowPriority|Počet|Celkem|Celkový počet jader s nízkou prioritou na účet batch|Žádné dimenze|
|TotalLowPriorityNodeCount|Počet uzlů s nízkou prioritou|Počet|Celkem|Celkový počet uzlů s nízkou prioritou na účtu batch|Žádné dimenze|
|CreatingNodeCount|Vytváří se počet uzlů|Počet|Celkem|Počet uzlů, které vytváří|Žádné dimenze|
|StartingNodeCount|Počáteční počet uzlů|Počet|Celkem|Počet uzlů spuštění|Žádné dimenze|
|WaitingForStartTaskNodeCount|Čekání na pro počet uzlů spuštění úkolu|Počet|Celkem|Počet uzlů čekání na dokončení úlohy spustit|Žádné dimenze|
|StartTaskFailedNodeCount|Spuštění úlohy se nezdařilo počet uzlů|Počet|Celkem|Počet uzlů, ve kterém se nezdařilo spustit úkol|Žádné dimenze|
|IdleNodeCount|Počet nečinných uzlů|Počet|Celkem|Počet nečinných uzlů|Žádné dimenze|
|OfflineNodeCount|Offline počet uzlů|Počet|Celkem|Počet uzlů v režimu offline|Žádné dimenze|
|RebootingNodeCount|Restartování počet uzlů|Počet|Celkem|Počtu restartování uzlů|Žádné dimenze|
|ReimagingNodeCount|Obnovování z Image počet uzlů|Počet|Celkem|Počet uzlů, obnovování z Image|Žádné dimenze|
|RunningNodeCount|Spuštění počet uzlů|Počet|Celkem|Počet spuštěných uzly|Žádné dimenze|
|LeavingPoolNodeCount|Opuštění počet uzlů fondu|Počet|Celkem|Počet uzlů, které byste museli opustit fondu|Žádné dimenze|
|UnusableNodeCount|Nepoužitelné počet uzlů|Počet|Celkem|Počet uzlů nepoužitelné|Žádné dimenze|
|PreemptedNodeCount|Ke zrušení přidělením počet uzlů|Počet|Celkem|Počet uzlů přerušeno|Žádné dimenze|
|TaskStartEvent|Události spuštění úloh|Počet|Celkem|Celkový počet úloh, které byly spuštěny|Žádné dimenze|
|TaskCompleteEvent|Události dokončení úkolu|Počet|Celkem|Celkový počet úloh, které byly dokončeny|Žádné dimenze|
|TaskFailEvent|Události selhání úloh|Počet|Celkem|Celkový počet úloh, které byly dokončeny v neúspěšném stavu|Žádné dimenze|
|PoolCreateEvent|Vytvoření fondu události|Počet|Celkem|Celkový počet fondů, které byly vytvořeny|Žádné dimenze|
|PoolResizeStartEvent|Události zahájení změny velikosti fondu|Počet|Celkem|Celkový počet změny velikosti fondu, které byly spuštěny|Žádné dimenze|
|PoolResizeCompleteEvent|Události dokončení změny velikosti fondu|Počet|Celkem|Celkový počet změny velikosti fondu, které byly dokončeny|Žádné dimenze|
|PoolDeleteStartEvent|Události zahájení odstranění fondu|Počet|Celkem|Celkový počet odstranění fondu, které byly spuštěny|Žádné dimenze|
|PoolDeleteCompleteEvent|Události dokončení odstranění fondu|Počet|Celkem|Celkový počet odstranění fondu, které byly dokončeny|Žádné dimenze|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|connectedclients|Počet připojených klientů|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed|Operace celkem|Počet|Celkem||Žádné dimenze|
|mezipaměti|Přístupy do mezipaměti|Počet|Celkem||Žádné dimenze|
|cachemisses|Neúspěšné přístupy do mezipaměti|Počet|Celkem||Žádné dimenze|
|getcommands|Operace Get|Počet|Celkem||Žádné dimenze|
|setcommands|Sady|Počet|Celkem||Žádné dimenze|
|operationsPerSecond|Operací za sekundu|Počet|Celkem||Žádné dimenze|
|evictedkeys|Vyloučené klíče|Počet|Celkem||Žádné dimenze|
|totalkeys|Celkový počet klíčů|Počet|Maximum||Žádné dimenze|
|expiredkeys|Prošlé klíče|Počet|Celkem||Žádné dimenze|
|usedmemory|Použitá paměť|B|Maximum||Žádné dimenze|
|usedmemoryRss|Využitá paměť RSS|B|Maximum||Žádné dimenze|
|serverLoad|Zatížení serveru|Procento|Maximum||Žádné dimenze|
|cacheWrite|Zápis do mezipaměti|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead|Čtení z mezipaměti|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime|Procesor|Procento|Maximum||Žádné dimenze|
|connectedclients0|Připojených klientů (horizontálních oddílů 0)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed0|Celkový počet operací (horizontálních oddílů 0)|Počet|Celkem||Žádné dimenze|
|cachehits0|Přístupy do mezipaměti (horizontálních oddílů 0)|Počet|Celkem||Žádné dimenze|
|cachemisses0|Neúspěšné přístupy do mezipaměti (horizontálních oddílů 0)|Počet|Celkem||Žádné dimenze|
|getcommands0|Získá (horizontálních oddílů 0)|Počet|Celkem||Žádné dimenze|
|setcommands0|Nastaví (horizontálních oddílů 0)|Počet|Celkem||Žádné dimenze|
|operationsPerSecond0|Operací za sekundu (horizontálních oddílů 0)|Počet|Celkem||Žádné dimenze|
|evictedkeys0|Vyloučené klíče (horizontálních oddílů 0)|Počet|Celkem||Žádné dimenze|
|totalkeys0|Celkový počet klíčů (horizontálních oddílů 0)|Počet|Maximum||Žádné dimenze|
|expiredkeys0|Prošlé klíče (horizontálních oddílů 0)|Počet|Celkem||Žádné dimenze|
|usedmemory0|Využitá paměť (horizontálních oddílů 0)|B|Maximum||Žádné dimenze|
|usedmemoryRss0|Využitá paměť RSS (horizontálních oddílů 0)|B|Maximum||Žádné dimenze|
|serverLoad0|Zatížení serveru (horizontálních oddílů 0)|Procento|Maximum||Žádné dimenze|
|cacheWrite0|Zápis do mezipaměti (horizontálních oddílů 0)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead0|Čtení z mezipaměti (horizontálních oddílů 0)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime0|Procesor (horizontálních oddílů 0)|Procento|Maximum||Žádné dimenze|
|connectedclients1|Připojených klientů (horizontálních oddílů 1)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed1|Celkový počet operací (horizontálních oddílů 1)|Počet|Celkem||Žádné dimenze|
|cachehits1|Přístupy do mezipaměti (horizontálních oddílů 1)|Počet|Celkem||Žádné dimenze|
|cachemisses1|Neúspěšné přístupy do mezipaměti (horizontálních oddílů 1)|Počet|Celkem||Žádné dimenze|
|getcommands1|Získá (horizontálních oddílů 1)|Počet|Celkem||Žádné dimenze|
|setcommands1|Nastaví (horizontálních oddílů 1)|Počet|Celkem||Žádné dimenze|
|operationsPerSecond1|Operací za sekundu (horizontálních oddílů 1)|Počet|Celkem||Žádné dimenze|
|evictedkeys1|Vyloučené klíče (horizontálních oddílů 1)|Počet|Celkem||Žádné dimenze|
|totalkeys1|Celkový počet klíčů (horizontálních oddílů 1)|Počet|Maximum||Žádné dimenze|
|expiredkeys1|Prošlé klíče (horizontálních oddílů 1)|Počet|Celkem||Žádné dimenze|
|usedmemory1|Využitá paměť (horizontálních oddílů 1)|B|Maximum||Žádné dimenze|
|usedmemoryRss1|Využitá paměť RSS (horizontálních oddílů 1)|B|Maximum||Žádné dimenze|
|serverLoad1|Zatížení serveru (horizontálních oddílů 1)|Procento|Maximum||Žádné dimenze|
|cacheWrite1|Zápis do mezipaměti (horizontálních oddílů 1)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead1|Čtení z mezipaměti (horizontálních oddílů 1)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime1|Procesor (horizontálních oddílů 1)|Procento|Maximum||Žádné dimenze|
|connectedclients2|Připojených klientů (horizontálních oddílů 2)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed2|Celkový počet operací (horizontálních oddílů 2)|Počet|Celkem||Žádné dimenze|
|cachehits2|Přístupy do mezipaměti (horizontálních oddílů 2)|Počet|Celkem||Žádné dimenze|
|cachemisses2|Neúspěšné přístupy do mezipaměti (horizontálních oddílů 2)|Počet|Celkem||Žádné dimenze|
|getcommands2|Získá (horizontálních oddílů 2)|Počet|Celkem||Žádné dimenze|
|setcommands2|Nastaví (horizontálních oddílů 2)|Počet|Celkem||Žádné dimenze|
|operationsPerSecond2|Operací za sekundu (horizontálních oddílů 2)|Počet|Celkem||Žádné dimenze|
|evictedkeys2|Vyloučené klíče (horizontálních oddílů 2)|Počet|Celkem||Žádné dimenze|
|totalkeys2|Celkový počet klíčů (horizontálních oddílů 2)|Počet|Maximum||Žádné dimenze|
|expiredkeys2|Prošlé klíče (horizontálních oddílů 2)|Počet|Celkem||Žádné dimenze|
|usedmemory2|Využitá paměť (horizontálních oddílů 2)|B|Maximum||Žádné dimenze|
|usedmemoryRss2|Využitá paměť RSS (horizontálních oddílů 2)|B|Maximum||Žádné dimenze|
|serverLoad2|Zatížení serveru (horizontálních oddílů 2)|Procento|Maximum||Žádné dimenze|
|cacheWrite2|Zápis do mezipaměti (horizontálních oddílů 2)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead2|Čtení z mezipaměti (horizontálních oddílů 2)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime2|Procesor (horizontálních oddílů 2)|Procento|Maximum||Žádné dimenze|
|connectedclients3|Připojených klientů (horizontálních oddílů 3)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed3|Celkový počet operací (horizontálních oddílů 3)|Počet|Celkem||Žádné dimenze|
|cachehits3|Přístupy do mezipaměti (horizontálních oddílů 3)|Počet|Celkem||Žádné dimenze|
|cachemisses3|Neúspěšné přístupy do mezipaměti (horizontálních oddílů 3)|Počet|Celkem||Žádné dimenze|
|getcommands3|Získá (horizontálních oddílů 3)|Počet|Celkem||Žádné dimenze|
|setcommands3|Nastaví (horizontálních oddílů 3)|Počet|Celkem||Žádné dimenze|
|operationsPerSecond3|Operací za sekundu (horizontálních oddílů 3)|Počet|Celkem||Žádné dimenze|
|evictedkeys3|Vyloučené klíče (horizontálních oddílů 3)|Počet|Celkem||Žádné dimenze|
|totalkeys3|Celkový počet klíčů (horizontálních oddílů 3)|Počet|Maximum||Žádné dimenze|
|expiredkeys3|Prošlé klíče (horizontálních oddílů 3)|Počet|Celkem||Žádné dimenze|
|usedmemory3|Využitá paměť (horizontálních oddílů 3)|B|Maximum||Žádné dimenze|
|usedmemoryRss3|Využitá paměť RSS (horizontálních oddílů 3)|B|Maximum||Žádné dimenze|
|serverLoad3|Zatížení serveru (horizontálních oddílů 3)|Procento|Maximum||Žádné dimenze|
|cacheWrite3|Zápis do mezipaměti (horizontálních oddílů 3)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead3|Čtení z mezipaměti (horizontálních oddílů 3)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime3|Procesor (horizontálních oddílů 3)|Procento|Maximum||Žádné dimenze|
|connectedclients4|Připojených klientů (horizontálních oddílů 4)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed4|Celkový počet operací (horizontálních oddílů 4)|Počet|Celkem||Žádné dimenze|
|cachehits4|Přístupy do mezipaměti (horizontálních oddílů 4)|Počet|Celkem||Žádné dimenze|
|cachemisses4|Neúspěšné přístupy do mezipaměti (horizontálních oddílů 4)|Počet|Celkem||Žádné dimenze|
|getcommands4|Získá (horizontálních oddílů 4)|Počet|Celkem||Žádné dimenze|
|setcommands4|Nastaví (horizontálních oddílů 4)|Počet|Celkem||Žádné dimenze|
|operationsPerSecond4|Operací za sekundu (horizontálních oddílů 4)|Počet|Celkem||Žádné dimenze|
|evictedkeys4|Vyloučené klíče (horizontálních oddílů 4)|Počet|Celkem||Žádné dimenze|
|totalkeys4|Celkový počet klíčů (horizontálních oddílů 4)|Počet|Maximum||Žádné dimenze|
|expiredkeys4|Prošlé klíče (horizontálních oddílů 4)|Počet|Celkem||Žádné dimenze|
|usedmemory4|Využitá paměť (horizontálních oddílů 4)|B|Maximum||Žádné dimenze|
|usedmemoryRss4|Využitá paměť RSS (horizontálních oddílů 4)|B|Maximum||Žádné dimenze|
|serverLoad4|Zatížení serveru (horizontálních oddílů 4)|Procento|Maximum||Žádné dimenze|
|cacheWrite4|Zápis do mezipaměti (horizontálních oddílů 4)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead4|Čtení z mezipaměti (horizontálních oddílů 4)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime4|Procesor (horizontálních oddílů 4)|Procento|Maximum||Žádné dimenze|
|connectedclients5|Připojených klientů (horizontální oddíl 5)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed5|Celkový počet operací (horizontální oddíl 5)|Počet|Celkem||Žádné dimenze|
|cachehits5|Přístupy do mezipaměti (horizontální oddíl 5)|Počet|Celkem||Žádné dimenze|
|cachemisses5|Neúspěšné přístupy do mezipaměti (horizontální oddíl 5)|Počet|Celkem||Žádné dimenze|
|getcommands5|Získá (horizontální oddíl 5)|Počet|Celkem||Žádné dimenze|
|setcommands5|Nastaví (horizontální oddíl 5)|Počet|Celkem||Žádné dimenze|
|operationsPerSecond5|Operací za sekundu (horizontální oddíl 5)|Počet|Celkem||Žádné dimenze|
|evictedkeys5|Vyloučené klíče (horizontální oddíl 5)|Počet|Celkem||Žádné dimenze|
|totalkeys5|Celkový počet klíčů (horizontální oddíl 5)|Počet|Maximum||Žádné dimenze|
|expiredkeys5|Prošlé klíče (horizontální oddíl 5)|Počet|Celkem||Žádné dimenze|
|usedmemory5|Využitá paměť (horizontální oddíl 5)|B|Maximum||Žádné dimenze|
|usedmemoryRss5|Využitá paměť RSS (horizontální oddíl 5)|B|Maximum||Žádné dimenze|
|serverLoad5|Zatížení serveru (horizontální oddíl 5)|Procento|Maximum||Žádné dimenze|
|cacheWrite5|Zápis do mezipaměti (horizontální oddíl 5)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead5|Čtení z mezipaměti (horizontální oddíl 5)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime5|Procesor (horizontální oddíl 5)|Procento|Maximum||Žádné dimenze|
|connectedclients6|Připojených klientů (horizontální oddíl 6)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed6|Celkový počet operací (horizontální oddíl 6)|Počet|Celkem||Žádné dimenze|
|cachehits6|Přístupy do mezipaměti (horizontální oddíl 6)|Počet|Celkem||Žádné dimenze|
|cachemisses6|Neúspěšné přístupy do mezipaměti (horizontální oddíl 6)|Počet|Celkem||Žádné dimenze|
|getcommands6|Získá (horizontální oddíl 6)|Počet|Celkem||Žádné dimenze|
|setcommands6|Nastaví (horizontální oddíl 6)|Počet|Celkem||Žádné dimenze|
|operationsPerSecond6|Operací za sekundu (horizontální oddíl 6)|Počet|Celkem||Žádné dimenze|
|evictedkeys6|Vyloučené klíče (horizontální oddíl 6)|Počet|Celkem||Žádné dimenze|
|totalkeys6|Celkový počet klíčů (horizontální oddíl 6)|Počet|Maximum||Žádné dimenze|
|expiredkeys6|Prošlé klíče (horizontální oddíl 6)|Počet|Celkem||Žádné dimenze|
|usedmemory6|Využitá paměť (horizontální oddíl 6)|B|Maximum||Žádné dimenze|
|usedmemoryRss6|Využitá paměť RSS (horizontální oddíl 6)|B|Maximum||Žádné dimenze|
|serverLoad6|Zatížení serveru (horizontální oddíl 6)|Procento|Maximum||Žádné dimenze|
|cacheWrite6|Zápis do mezipaměti (horizontální oddíl 6)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead6|Čtení z mezipaměti (horizontální oddíl 6)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime6|Procesor (horizontální oddíl 6)|Procento|Maximum||Žádné dimenze|
|connectedclients7|Připojených klientů (horizontální oddíl 7)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed7|Celkový počet operací (horizontální oddíl 7)|Počet|Celkem||Žádné dimenze|
|cachehits7|Přístupy do mezipaměti (horizontální oddíl 7)|Počet|Celkem||Žádné dimenze|
|cachemisses7|Neúspěšné přístupy do mezipaměti (horizontální oddíl 7)|Počet|Celkem||Žádné dimenze|
|getcommands7|Získá (horizontální oddíl 7)|Počet|Celkem||Žádné dimenze|
|setcommands7|Nastaví (horizontální oddíl 7)|Počet|Celkem||Žádné dimenze|
|operationsPerSecond7|Operací za sekundu (horizontální oddíl 7)|Počet|Celkem||Žádné dimenze|
|evictedkeys7|Vyloučené klíče (horizontální oddíl 7)|Počet|Celkem||Žádné dimenze|
|totalkeys7|Celkový počet klíčů (horizontální oddíl 7)|Počet|Maximum||Žádné dimenze|
|expiredkeys7|Prošlé klíče (horizontální oddíl 7)|Počet|Celkem||Žádné dimenze|
|usedmemory7|Využitá paměť (horizontální oddíl 7)|B|Maximum||Žádné dimenze|
|usedmemoryRss7|Využitá paměť RSS (horizontální oddíl 7)|B|Maximum||Žádné dimenze|
|serverLoad7|Zatížení serveru (horizontální oddíl 7)|Procento|Maximum||Žádné dimenze|
|cacheWrite7|Zápis do mezipaměti (horizontální oddíl 7)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead7|Čtení z mezipaměti (horizontální oddíl 7)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime7|Procesor (horizontální oddíl 7)|Procento|Maximum||Žádné dimenze|
|connectedclients8|Připojených klientů (horizontální oddíl 8)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed8|Celkový počet operací (horizontální oddíl 8)|Počet|Celkem||Žádné dimenze|
|cachehits8|Přístupy do mezipaměti (horizontální oddíl 8)|Počet|Celkem||Žádné dimenze|
|cachemisses8|Neúspěšné přístupy do mezipaměti (horizontální oddíl 8)|Počet|Celkem||Žádné dimenze|
|getcommands8|Získá (horizontální oddíl 8)|Počet|Celkem||Žádné dimenze|
|setcommands8|Nastaví (horizontální oddíl 8)|Počet|Celkem||Žádné dimenze|
|operationsPerSecond8|Operací za sekundu (horizontální oddíl 8)|Počet|Celkem||Žádné dimenze|
|evictedkeys8|Vyloučené klíče (horizontální oddíl 8)|Počet|Celkem||Žádné dimenze|
|totalkeys8|Celkový počet klíčů (horizontální oddíl 8)|Počet|Maximum||Žádné dimenze|
|expiredkeys8|Prošlé klíče (horizontální oddíl 8)|Počet|Celkem||Žádné dimenze|
|usedmemory8|Využitá paměť (horizontální oddíl 8)|B|Maximum||Žádné dimenze|
|usedmemoryRss8|Využitá paměť RSS (horizontální oddíl 8)|B|Maximum||Žádné dimenze|
|serverLoad8|Zatížení serveru (horizontální oddíl 8)|Procento|Maximum||Žádné dimenze|
|cacheWrite8|Zápis do mezipaměti (horizontální oddíl 8)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead8|Čtení z mezipaměti (horizontální oddíl 8)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime8|Procesor (horizontální oddíl 8)|Procento|Maximum||Žádné dimenze|
|connectedclients9|Připojených klientů (horizontální oddíl 9)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed9|Celkový počet operací (horizontální oddíl 9)|Počet|Celkem||Žádné dimenze|
|cachehits9|Přístupy do mezipaměti (horizontální oddíl 9)|Počet|Celkem||Žádné dimenze|
|cachemisses9|Neúspěšné přístupy do mezipaměti (horizontální oddíl 9)|Počet|Celkem||Žádné dimenze|
|getcommands9|Získá (horizontální oddíl 9)|Počet|Celkem||Žádné dimenze|
|setcommands9|Nastaví (horizontální oddíl 9)|Počet|Celkem||Žádné dimenze|
|operationsPerSecond9|Operací za sekundu (horizontální oddíl 9)|Počet|Celkem||Žádné dimenze|
|evictedkeys9|Vyloučené klíče (horizontální oddíl 9)|Počet|Celkem||Žádné dimenze|
|totalkeys9|Celkový počet klíčů (horizontální oddíl 9)|Počet|Maximum||Žádné dimenze|
|expiredkeys9|Prošlé klíče (horizontální oddíl 9)|Počet|Celkem||Žádné dimenze|
|usedmemory9|Využitá paměť (horizontální oddíl 9)|B|Maximum||Žádné dimenze|
|usedmemoryRss9|Využitá paměť RSS (horizontální oddíl 9)|B|Maximum||Žádné dimenze|
|serverLoad9|Zatížení serveru (horizontální oddíl 9)|Procento|Maximum||Žádné dimenze|
|cacheWrite9|Zápis do mezipaměti (horizontální oddíl 9)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead9|Čtení z mezipaměti (horizontální oddíl 9)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime9|Procesor (horizontální oddíl 9)|Procento|Maximum||Žádné dimenze|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Procento CPU|Procento CPU|Procento|Průměr|Procento přidělených výpočetních jednotek, které virtuální počítače aktuálně používají|Žádné dimenze|
|Síťové vstupy|Síťové vstupy|B|Celkem|Počet bajtů přijatých virtuálními počítači na všech síťových rozhraních (příchozí provoz)|Žádné dimenze|
|Síťové výstupy|Síťové výstupy|B|Celkem|Počet bajtů odchozích ze všech síťových rozhraní virtuálních počítačů (odchozí provoz)|Žádné dimenze|
|Bajty čtení z disku/s|Čtení z disku|BytesPerSecond|Průměr|Průměrný počet bajtů přečtený z disku během období monitorování|Žádné dimenze|
|Bajty zápisu na disk/s|Zápis na disk|BytesPerSecond|Průměr|Průměrný počet bajtů zapsaný na disk během období monitorování|Žádné dimenze|
|Čtení z disku – operace/s|Čtení z disku – operace/s|CountPerSecond|Průměr|Čtení z disku – IOPS|Žádné dimenze|
|Zápis na disk – operace/s|Zápis na disk – operace/s|CountPerSecond|Průměr|Zápis na disk – IOPS|Žádné dimenze|

## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Microsoft.ClassicCompute/domainNames/slots/roles

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Procento CPU|Procento CPU|Procento|Průměr|Procento přidělených výpočetních jednotek, které virtuální počítače aktuálně používají|Žádné dimenze|
|Síťové vstupy|Síťové vstupy|B|Celkem|Počet bajtů přijatých virtuálními počítači na všech síťových rozhraních (příchozí provoz)|Žádné dimenze|
|Síťové výstupy|Síťové výstupy|B|Celkem|Počet bajtů odchozích ze všech síťových rozhraní virtuálních počítačů (odchozí provoz)|Žádné dimenze|
|Bajty čtení z disku/s|Čtení z disku|BytesPerSecond|Průměr|Průměrný počet bajtů přečtený z disku během období monitorování|Žádné dimenze|
|Bajty zápisu na disk/s|Zápis na disk|BytesPerSecond|Průměr|Průměrný počet bajtů zapsaný na disk během období monitorování|Žádné dimenze|
|Čtení z disku – operace/s|Čtení z disku – operace/s|CountPerSecond|Průměr|Čtení z disku – IOPS|Žádné dimenze|
|Zápis na disk – operace/s|Zápis na disk – operace/s|CountPerSecond|Průměr|Zápis na disk – IOPS|Žádné dimenze|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TotalCalls|Celkem volání|Počet|Celkem|Celkový počet volání|Žádné dimenze|
|SuccessfulCalls|Úspěšná volání|Počet|Celkem|Počet úspěšných volání|Žádné dimenze|
|TotalErrors|Celkem chyb|Počet|Celkem|Celkový počet volání s chybovou odpovědí (kód odpovědi HTTP 4xx nebo 5xx)|Žádné dimenze|
|BlockedCalls|Blokovaná volání|Počet|Celkem|Počet volání nad limit vyplývající ze sazby nebo kvóty|Žádné dimenze|
|ServerErrors|Chyby serveru|Počet|Celkem|Počet volání s vnitřní chybou služby (kód odpovědi HTTP 5xx)|Žádné dimenze|
|ClientErrors|Chyby klientů|Počet|Celkem|Počet volání s chybou na straně klienta (kód odpovědi HTTP 4xx)|Žádné dimenze|
|DataIn|Vstupní data|B|Celkem|Velikost příchozích dat v bajtech|Žádné dimenze|
|DataOut|Výstupní data|B|Celkem|Velikost odchozích dat v bajtech|Žádné dimenze|
|Latence|Latence|Milisekundy|Průměr|Latence v milisekundách|Žádné dimenze|
|CharactersTranslated|Přeložené znaky|Počet|Celkem|Celkový počet znaků v příchozí textové žádosti|Žádné dimenze|
|SpeechSessionDuration|Doba trvání řečové relace|Sekundy|Celkem|Celková doba trvání řečové relace v sekundách|Žádné dimenze|
|TotalTransactions|Transakce celkem|Počet|Celkem|Celkový počet transakcí|Žádné dimenze|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Procento CPU|Procento CPU|Procento|Průměr|Procento přidělených výpočetních jednotek, které virtuální počítače aktuálně používají|Žádné dimenze|
|Síťové vstupy|Síťové vstupy|B|Celkem|Počet bajtů přijatých virtuálními počítači na všech síťových rozhraních (příchozí provoz)|Žádné dimenze|
|Síťové výstupy|Síťové výstupy|B|Celkem|Počet bajtů odchozích ze všech síťových rozhraní virtuálních počítačů (odchozí provoz)|Žádné dimenze|
|Čtení z disku – bajty|Čtení z disku – bajty|B|Celkem|Celkový počet bajtů přečtený z disku během monitorování|Žádné dimenze|
|Zápis na disk – bajty|Zápis na disk – bajty|B|Celkem|Celkový počet bajtů zapsaný na disk během monitorování|Žádné dimenze|
|Čtení z disku – operace/s|Čtení z disku – operace/s|CountPerSecond|Průměr|Čtení z disku – IOPS|Žádné dimenze|
|Zápis na disk – operace/s|Zápis na disk – operace/s|CountPerSecond|Průměr|Zápis na disk – IOPS|Žádné dimenze|
|Zbývající kredity CPU|Zbývající kredity CPU|Počet|Průměr|Celkové množství kreditu, které se dá využít|Žádné dimenze|
|Spotřebované kredity CPU|Spotřebované kredity CPU|Počet|Průměr|Celkové množství kreditu, které spotřeboval virtuální počítač|Žádné dimenze|
|Přečtené bajty podle disku za sekundu|Bajty přečtené z datového disku za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet bajtů za sekundu přečtených z jednoho disku během období monitorování|SlotId|
|Zapsané bajty podle disku za sekundu|Bajty zapsané na datový disk za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet bajtů za sekundu zapsaných na jeden disk během období monitorování|SlotId|
|Operace čtení podle disku za sekundu|Operace čtení z datového disku za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet IOPS provedených při čtení z jednoho disku během období monitorování|SlotId|
|Operace zápisu podle disku za sekundu|Operace zápisu na datový disk za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet IOPS provedených při zápisu na jeden disk během období monitorování|SlotId|
|HF podle disku|HF datového disku (Preview)|Počet|Průměr|Hloubka fronty datového disku (nebo délka fronty)|SlotId|
|Přečtené bajty v operačním systému podle disku za sekundu|Bajty přečtené z disku s operačním systémem za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet bajtů za sekundu přečtených z jednoho disku během období monitorování pro disk s operačním systémem|Žádné dimenze|
|Zapsané bajty v operačním systému podle disku za sekundu|Bajty zapsané na disk s operačním systémem za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet bajtů za sekundu zapsaných na jeden disk během období monitorování pro disk s operačním systémem|Žádné dimenze|
|Operace čtení v operačním systému podle disku za sekundu|Operace čtení z disku s operačním systémem za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet IOPS provedených při čtení z jednoho disku během období monitorování pro disk s operačním systémem|Žádné dimenze|
|Operace zápisu v operačním systému podle disku za sekundu|Operace zápisu na disk s operačním systémem za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet IOPS provedených při zápisu na jeden disk během období monitorování pro disk s operačním systémem|Žádné dimenze|
|HF v operačním systému podle disku|HF disku s operačním systémem (Preview)|Počet|Průměr|Hloubka fronty disku s operačním systémem (nebo délka fronty)|Žádné dimenze|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Procento CPU|Procento CPU|Procento|Průměr|Procento přidělených výpočetních jednotek, které virtuální počítače aktuálně používají|Žádné dimenze|
|Síťové vstupy|Síťové vstupy|B|Celkem|Počet bajtů přijatých virtuálními počítači na všech síťových rozhraních (příchozí provoz)|Žádné dimenze|
|Síťové výstupy|Síťové výstupy|B|Celkem|Počet bajtů odchozích ze všech síťových rozhraní virtuálních počítačů (odchozí provoz)|Žádné dimenze|
|Čtení z disku – bajty|Čtení z disku – bajty|B|Celkem|Celkový počet bajtů přečtený z disku během monitorování|Žádné dimenze|
|Zápis na disk – bajty|Zápis na disk – bajty|B|Celkem|Celkový počet bajtů zapsaný na disk během monitorování|Žádné dimenze|
|Čtení z disku – operace/s|Čtení z disku – operace/s|CountPerSecond|Průměr|Čtení z disku – IOPS|Žádné dimenze|
|Zápis na disk – operace/s|Zápis na disk – operace/s|CountPerSecond|Průměr|Zápis na disk – IOPS|Žádné dimenze|
|Zbývající kredity CPU|Zbývající kredity CPU|Počet|Průměr|Celkové množství kreditu, které se dá využít|Žádné dimenze|
|Spotřebované kredity CPU|Spotřebované kredity CPU|Počet|Průměr|Celkové množství kreditu, které spotřeboval virtuální počítač|Žádné dimenze|
|Přečtené bajty podle disku za sekundu|Bajty přečtené z datového disku za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet bajtů za sekundu přečtených z jednoho disku během období monitorování|SlotId|
|Zapsané bajty podle disku za sekundu|Bajty zapsané na datový disk za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet bajtů za sekundu zapsaných na jeden disk během období monitorování|SlotId|
|Operace čtení podle disku za sekundu|Operace čtení z datového disku za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet IOPS provedených při čtení z jednoho disku během období monitorování|SlotId|
|Operace zápisu podle disku za sekundu|Operace zápisu na datový disk za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet IOPS provedených při zápisu na jeden disk během období monitorování|SlotId|
|HF podle disku|HF datového disku (Preview)|Počet|Průměr|Hloubka fronty datového disku (nebo délka fronty)|SlotId|
|Přečtené bajty v operačním systému podle disku za sekundu|Disk s operačním systémem přečtené bajty/s|CountPerSecond|Průměr|Celkový počet bajtů za sekundu přečtených z jednoho disku během období monitorování pro disk s operačním systémem|Žádné dimenze|
|Zapsané bajty v operačním systému podle disku za sekundu|Bajty zapsané na disk s operačním systémem za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet bajtů za sekundu zapsaných na jeden disk během období monitorování pro disk s operačním systémem|Žádné dimenze|
|Operace čtení v operačním systému podle disku za sekundu|Operace čtení z disku s operačním systémem za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet IOPS provedených při čtení z jednoho disku během období monitorování pro disk s operačním systémem|Žádné dimenze|
|Operace zápisu v operačním systému podle disku za sekundu|Operace zápisu na disk s operačním systémem za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet IOPS provedených při zápisu na jeden disk během období monitorování pro disk s operačním systémem|Žádné dimenze|
|HF v operačním systému podle disku|HF disku s operačním systémem (Preview)|Počet|Průměr|Hloubka fronty disku s operačním systémem (nebo délka fronty)|Žádné dimenze|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Procento CPU|Procento CPU|Procento|Průměr|Procento přidělených výpočetních jednotek, které virtuální počítače aktuálně používají|Žádné dimenze|
|Síťové vstupy|Síťové vstupy|B|Celkem|Počet bajtů přijatých virtuálními počítači na všech síťových rozhraních (příchozí provoz)|Žádné dimenze|
|Síťové výstupy|Síťové výstupy|B|Celkem|Počet bajtů odchozích ze všech síťových rozhraní virtuálních počítačů (odchozí provoz)|Žádné dimenze|
|Čtení z disku – bajty|Čtení z disku – bajty|B|Celkem|Celkový počet bajtů přečtený z disku během monitorování|Žádné dimenze|
|Zápis na disk – bajty|Zápis na disk – bajty|B|Celkem|Celkový počet bajtů zapsaný na disk během monitorování|Žádné dimenze|
|Čtení z disku – operace/s|Čtení z disku – operace/s|CountPerSecond|Průměr|Čtení z disku – IOPS|Žádné dimenze|
|Zápis na disk – operace/s|Zápis na disk – operace/s|CountPerSecond|Průměr|Zápis na disk – IOPS|Žádné dimenze|
|Zbývající kredity CPU|Zbývající kredity CPU|Počet|Průměr|Celkové množství kreditu, které se dá využít|Žádné dimenze|
|Spotřebované kredity CPU|Spotřebované kredity CPU|Počet|Průměr|Celkové množství kreditu, které spotřeboval virtuální počítač|Žádné dimenze|
|Přečtené bajty podle disku za sekundu|Bajty přečtené z datového disku za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet bajtů za sekundu přečtených z jednoho disku během období monitorování|SlotId|
|Zapsané bajty podle disku za sekundu|Bajty zapsané na datový disk za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet bajtů za sekundu zapsaných na jeden disk během období monitorování|SlotId|
|Operace čtení podle disku za sekundu|Operace čtení z datového disku za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet IOPS provedených při čtení z jednoho disku během období monitorování|SlotId|
|Operace zápisu podle disku za sekundu|Operace zápisu na datový disk za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet IOPS provedených při zápisu na jeden disk během období monitorování|SlotId|
|HF podle disku|HF datového disku (Preview)|Počet|Průměr|Hloubka fronty datového disku (nebo délka fronty)|SlotId|
|Přečtené bajty v operačním systému podle disku za sekundu|Bajty přečtené z disku s operačním systémem za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet bajtů za sekundu přečtených z jednoho disku během období monitorování pro disk s operačním systémem|Žádné dimenze|
|Zapsané bajty v operačním systému podle disku za sekundu|Bajty zapsané na disk s operačním systémem za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet bajtů za sekundu zapsaných na jeden disk během období monitorování pro disk s operačním systémem|Žádné dimenze|
|Operace čtení v operačním systému podle disku za sekundu|Operace čtení z disku s operačním systémem za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet IOPS provedených při čtení z jednoho disku během období monitorování pro disk s operačním systémem|Žádné dimenze|
|Operace zápisu v operačním systému podle disku za sekundu|Operace zápisu na disk s operačním systémem za sekundu (Preview)|CountPerSecond|Průměr|Celkový počet IOPS provedených při zápisu na jeden disk během období monitorování pro disk s operačním systémem|Žádné dimenze|
|HF v operačním systému podle disku|HF disku s operačním systémem (Preview)|Počet|Průměr|Hloubka fronty disku s operačním systémem (nebo délka fronty)|Žádné dimenze|

## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft.ContainerInstance/containerGroups

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|CpuUsage|Využití procesoru|Počet|Průměr|Využití procesoru u všech jader v jednotkách millicore|containerName|
|Parametru MemoryUsage|Využití paměti|B|Průměr|Celkové využití paměti v bajtech|containerName|

## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft.ContainerService/managedClusters

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|kube_node_status_allocatable_cpu_cores|Celkový počet dostupných jader procesoru v spravovaný cluster|Počet|Celkem|Celkový počet dostupných jader procesoru v spravovaný cluster|Žádné dimenze|
|kube_node_status_allocatable_memory_bytes|Celkové množství dostupné paměti v spravovaný cluster|B|Celkem|Celkové množství dostupné paměti v spravovaný cluster|Žádné dimenze|
|kube_pod_status_ready|Počet podů ve stavu Připraveno|Počet|Celkem|Počet podů ve stavu Připraveno|obor názvů, pod|
|kube_node_status_condition|Stavy pro různé podmínky uzlu|Počet|Celkem|Stavy pro různé podmínky uzlu|Stav, stav, uzlu|
|kube_pod_status_phase|Počet podů podle fáze|Počet|Celkem|Počet podů podle fáze|fáze, obor názvů, pod|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|DCIApiCalls|Volání Insights API zákazníka|Počet|Celkem||Žádné dimenze|
|DCIMappingImportOperationSuccessfulLines|Mapování Import operace úspěšné řádky|Počet|Celkem||Žádné dimenze|
|DCIMappingImportOperationFailedLines|Mapování operace importu se nezdařila řádky|Počet|Celkem||Žádné dimenze|
|DCIMappingImportOperationTotalLines|Mapování Import operace celkový počet řádků|Počet|Celkem||Žádné dimenze|
|DCIMappingImportOperationRuntimeInSeconds|Mapování modulu Runtime operace importu v řádu sekund|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundProfileExportSucceeded|Exportovat odchozí profilu proběhlo úspěšně|Počet|Celkem||Žádné dimenze|
|DCIOutboundProfileExportFailed|Exportovat odchozí profilu se nezdařila|Počet|Celkem||Žádné dimenze|
|DCIOutboundProfileExportDuration|Doba trvání exportu odchozí profilu|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundKpiExportSucceeded|Odchozí klíčového ukazatele výkonu Export byl úspěšný|Počet|Celkem||Žádné dimenze|
|DCIOutboundKpiExportFailed|Exportovat odchozí klíčového ukazatele výkonu se nezdařilo|Počet|Celkem||Žádné dimenze|
|DCIOutboundKpiExportDuration|Doba trvání odchozí Export klíčového ukazatele výkonu|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundKpiExportStarted|Spuštěn Export odchozí klíčového ukazatele výkonu|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundKpiRecordCount|Počet záznamů odchozí klíčového ukazatele výkonu|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundProfileExportCount|Počet odchozích profilu exportu|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundInitialProfileExportFailed|Počáteční odchozí profilu exportu se nezdařila|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundInitialProfileExportSucceeded|Počáteční odchozí profilu exportu byla úspěšná|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundInitialKpiExportFailed|Odchozí počáteční klíčového ukazatele výkonu Export se nezdařil|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundInitialKpiExportSucceeded|Odchozí počáteční klíčového ukazatele výkonu Export byl úspěšný|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundInitialProfileExportDurationInSeconds|Doba trvání exportu odchozí počáteční profil v sekundách|Sekundy|Celkem||Žádné dimenze|
|AdlaJobForStandardKpiFailed|Úloha Adla pro standardní klíčového ukazatele výkonu se nezdařilo v řádu sekund|Sekundy|Celkem||Žádné dimenze|
|AdlaJobForStandardKpiTimeOut|Úloha Adla pro standardní klíčového ukazatele výkonu časový limit v sekundách|Sekundy|Celkem||Žádné dimenze|
|AdlaJobForStandardKpiCompleted|Úloha Adla Standard klíčových ukazatelů výkonu v sekundách|Sekundy|Celkem||Žádné dimenze|
|ImportASAValuesFailed|Počet neúspěšných hodnoty import Azure Stream Analytics|Počet|Celkem||Žádné dimenze|
|ImportASAValuesSucceeded|Počet úspěšně import ASA hodnoty|Počet|Celkem||Žádné dimenze|
|DCIProfilesCount|Počet instancí profilu|Počet|Poslední||Žádné dimenze|
|DCIInteractionsPerMonthCount|Interakcí za měsíc počet|Počet|Poslední||Žádné dimenze|
|DCIKpisCount|Počet klíčových ukazatelů výkonu|Počet|Poslední||Žádné dimenze|
|DCISegmentsCount|Počet segmentů|Počet|Poslední||Žádné dimenze|
|DCIPredictiveMatchPoliciesCount|Prediktivní počet shod|Počet|Poslední||Žádné dimenze|
|DCIPredictionsCount|Počet predikcí|Počet|Poslední||Žádné dimenze|

## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|FailedRuns|Neúspěšná spuštění|Počet|Celkem||pipelineName, activityName, windowEnd, windowStart|
|SuccessfulRuns|Úspěšná spuštění|Počet|Celkem||pipelineName, activityName, windowEnd, windowStart|

## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|PipelineFailedRuns|Se nezdařilo metrika spuštění kanálu|Počet|Celkem||FailureType, název|
|PipelineSucceededRuns|Úspěšné metriky spuštění kanálu|Počet|Celkem||FailureType, název|
|ActivityFailedRuns|Metriky aktivity spuštění se nezdařilo|Počet|Celkem||ActivityType, PipelineName, FailureType, název|
|ActivitySucceededRuns|Úspěšné běhy metriky aktivity|Počet|Celkem||ActivityType, PipelineName, FailureType, název|
|TriggerFailedRuns|Se nezdařilo metrika spuštění aktivační události|Počet|Celkem||Název, FailureType|
|TriggerSucceededRuns|Aktivační událost metriky spuštění bylo úspěšné|Počet|Celkem||Název, FailureType|
|IntegrationRuntimeCpuPercentage|Využití Integration runtime – procesor|Procento|Průměr||IntegrationRuntimeName NodeName|
|IntegrationRuntimeAvailableMemory|Integration runtime – paměť k dispozici|B|Průměr||IntegrationRuntimeName NodeName|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|JobEndedSuccess|Úspěšné úlohy|Počet|Celkem|Počet úspěšných úloh.|Žádné dimenze|
|JobEndedFailure|Neúspěšné úlohy|Počet|Celkem|Počet nezdařených úloh.|Žádné dimenze|
|JobEndedCancelled|Zrušené úlohy|Počet|Celkem|Počet zrušených úloh.|Žádné dimenze|
|JobAUEndedSuccess|Úspěšné čas AU|Sekundy|Celkem|Celkový čas AU u úspěšných úloh.|Žádné dimenze|
|JobAUEndedFailure|Čas selhání AU|Sekundy|Celkem|Celkový čas AU pro neúspěšné úlohy.|Žádné dimenze|
|JobAUEndedCancelled|Zrušené čas AU|Sekundy|Celkem|Celkový čas AU pro zrušené úlohy.|Žádné dimenze|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TotalStorage|Celková velikost úložiště|B|Maximum|Celkové množství dat uložených v účtu.|Žádné dimenze|
|DataWritten|Zapsaná data|B|Celkem|Celkové množství dat zapsaných do účtu.|Žádné dimenze|
|Přečtená data|Přečtená data|B|Celkem|Celkové množství dat číst z účtu.|Žádné dimenze|
|WriteRequests|Požadavky na zápis|Počet|Celkem|Počet dat požadavky na zápis k účtu.|Žádné dimenze|
|ReadRequests|Žádosti o čtení|Počet|Celkem|Počet dat, přečtěte si požadavky na účet.|Žádné dimenze|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|cpu_percent|Procento CPU|Procento|Průměr|Procento CPU|Žádné dimenze|
|memory_percent|Paměť v procentech|Procento|Průměr|Paměť v procentech|Žádné dimenze|
|io_consumption_percent|V/v úrovně procent|Procento|Průměr|V/v úrovně procent|Žádné dimenze|
|storage_percent|Procento úložiště|Procento|Průměr|Procento úložiště|Žádné dimenze|
|storage_used|Využité úložiště|B|Průměr|Využité úložiště|Žádné dimenze|
|storage_limit|Limit úložiště.|B|Průměr|Limit úložiště.|Žádné dimenze|
|serverlog_storage_percent|Procento úložiště protokolů serveru|Procento|Průměr|Procento úložiště protokolů serveru|Žádné dimenze|
|serverlog_storage_usage|Využité úložiště protokolů serveru|B|Průměr|Využité úložiště protokolů serveru|Žádné dimenze|
|serverlog_storage_limit|Limit úložiště protokolů serveru|B|Průměr|Limit úložiště protokolů serveru|Žádné dimenze|
|active_connections|Celkový počet aktivních připojení|Počet|Průměr|Celkový počet aktivních připojení|Žádné dimenze|
|connections_failed|Celkový počet selhání připojení|Počet|Celkem|Celkový počet selhání připojení|Žádné dimenze|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|cpu_percent|Procento CPU|Procento|Průměr|Procento CPU|Žádné dimenze|
|memory_percent|Paměť v procentech|Procento|Průměr|Paměť v procentech|Žádné dimenze|
|io_consumption_percent|V/v úrovně procent|Procento|Průměr|V/v úrovně procent|Žádné dimenze|
|storage_percent|Procento úložiště|Procento|Průměr|Procento úložiště|Žádné dimenze|
|storage_used|Využité úložiště|B|Průměr|Využité úložiště|Žádné dimenze|
|storage_limit|Limit úložiště.|B|Průměr|Limit úložiště.|Žádné dimenze|
|serverlog_storage_percent|Procento úložiště protokolů serveru|Procento|Průměr|Procento úložiště protokolů serveru|Žádné dimenze|
|serverlog_storage_usage|Využité úložiště protokolů serveru|B|Průměr|Využité úložiště protokolů serveru|Žádné dimenze|
|serverlog_storage_limit|Limit úložiště protokolů serveru|B|Průměr|Limit úložiště protokolů serveru|Žádné dimenze|
|active_connections|Celkový počet aktivních připojení|Počet|Celkem|Celkový počet aktivních připojení|Žádné dimenze|
|connections_failed|Celkový počet selhání připojení|Počet|Celkem|Celkový počet selhání připojení|Žádné dimenze|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Pokusy o odeslat zpráva telemetrie|Počet|Celkem|Počet zpráv typu zařízení cloud telemetrie, které se pokusil odeslat do služby IoT hub|Žádné dimenze|
|d2c.telemetry.ingress.success|Telemetrické zprávy odesílané|Počet|Celkem|Počet zpráv typu zařízení cloud telemetrie úspěšně odeslán do služby IoT hub|Žádné dimenze|
|c2d.Commands.egress.COMPLETE.Success|Dokončení příkazů|Počet|Celkem|Počet příkazů typu cloud zařízení zařízení byla úspěšně dokončena|Žádné dimenze|
|c2d.commands.egress.abandon.success|Příkazy opuštění|Počet|Celkem|Počet opuštěných zařízení příkazy typu cloud zařízení|Žádné dimenze|
|c2d.Commands.egress.Reject.Success|Příkazy zamítnuto|Počet|Celkem|Počet odmítnutých zařízení příkazy typu cloud zařízení|Žádné dimenze|
|devices.totalDevices|Celkem zařízení|Počet|Celkem|Počet zařízení zaregistrovaný do služby IoT hub|Žádné dimenze|
|devices.connectedDevices.allProtocol|Připojená zařízení|Počet|Celkem|Počet zařízení připojená ke službě IoT hub|Žádné dimenze|
|d2c.telemetry.egress.success|Telemetrické zprávy doručí|Počet|Celkem|Počet pokusů, které zprávy se úspěšně zapsaly do koncových bodů (celkem)|Žádné dimenze|
|d2c.telemetry.egress.dropped|Vyřazené zprávy|Počet|Celkem|Počet zpráv, které zahozena, protože koncový bod doručování byl dead|Žádné dimenze|
|d2c.telemetry.egress.orphaned|Osamocené zprávy|Počet|Celkem|Počet zpráv neshodují všechny trasy, včetně postupu pro použití náhradní lokality|Žádné dimenze|
|d2c.telemetry.egress.invalid|Neplatné zprávy|Počet|Celkem|Počet zpráv nelze doručit z důvodu nekompatibility s koncovým bodem|Žádné dimenze|
|d2c.telemetry.egress.fallback|Zprávy odpovídající podmínku pro použití náhradní lokality|Počet|Celkem|Počet zpráv zapsaných do záložního koncového bodu|Žádné dimenze|
|d2c.endpoints.egress.eventHubs|Doručování zpráv do koncových bodů centra událostí|Počet|Celkem|Počet pokusů, které zprávy se úspěšně zapsaly do koncových bodů centra událostí|Žádné dimenze|
|d2c.endpoints.latency.eventHubs|Zpráva latence pro koncové body centra událostí|Milisekundy|Průměr|Průměrná latence mezi zprávy příchozího přenosu dat do služby IoT hub a příchozího přenosu dat zprávu do koncového bodu centra událostí, v milisekundách|Žádné dimenze|
|d2c.endpoints.egress.serviceBusQueues|Doručování zpráv do fronty služby Service Bus koncových bodů|Počet|Celkem|Počet pokusů, které zprávy se úspěšně zapsaly do koncových bodů frontu služby Service Bus|Žádné dimenze|
|d2c.endpoints.latency.serviceBusQueues|Zpráva latence pro koncové body fronty Service Bus|Milisekundy|Průměr|Průměrná latence mezi zprávy příchozího přenosu dat do služby IoT hub a příchozího přenosu dat zprávy do fronty služby Service Bus koncového bodu v milisekundách|Žádné dimenze|
|d2c.endpoints.egress.serviceBusTopics|Doručování zpráv do koncových bodů služby Service Bus|Počet|Celkem|Počet pokusů, které zprávy se úspěšně zapsaly do koncových bodů služby Service Bus|Žádné dimenze|
|d2c.endpoints.latency.serviceBusTopics|Zpráva latence pro koncové body služby Service Bus|Milisekundy|Průměr|Průměrná latence mezi zprávy příchozího přenosu dat do služby IoT hub a příchozího přenosu dat zprávu do koncového bodu služby Service Bus v milisekundách|Žádné dimenze|
|d2c.endpoints.egress.builtIn.events|Doručování zpráv do integrovaného koncového bodu (zprávy/events)|Počet|Celkem|Počet pokusů, které zprávy se úspěšně zapsaly do integrovaného koncového bodu (zprávy/events)|Žádné dimenze|
|d2c.endpoints.latency.builtIn.events|Zpráva latence pro integrovaný koncový bod (zprávy/events)|Milisekundy|Průměr|Průměrná latence mezi zprávy příchozího přenosu dat do služby IoT hub a příchozího přenosu dat zprávy do integrovaného koncového bodu (zprávy událostí), v milisekundách |Žádné dimenze|
|d2c.endpoints.egress.Storage|Doručování zpráv do koncových bodů úložiště|Počet|Celkem|Počet pokusů, které zprávy se úspěšně zapsaly do koncových bodů úložiště|Žádné dimenze|
|d2c.endpoints.latency.Storage|Zpráva latence pro koncové body úložiště|Milisekundy|Průměr|Průměrná latence mezi zprávy příchozího přenosu dat do služby IoT hub a příchozího přenosu dat zprávu do koncového bodu úložiště v milisekundách|Žádné dimenze|
|d2c.endpoints.egress.Storage.bytes|Data zapsaná do úložiště|B|Celkem|Množství dat v bajtech, zapsán do koncových bodů úložiště|Žádné dimenze|
|d2c.endpoints.egress.Storage.BLOBS|Zapsat do úložiště objektů BLOB|Počet|Celkem|Počet zapsaných do koncových bodů úložiště objektů BLOB|Žádné dimenze|
|d2c.twin.read.success|Čtení dvojčat úspěšné ze zařízení|Počet|Celkem|Počet všech úspěšných čtení dvojčat zařízení iniciované.|Žádné dimenze|
|d2c.Twin.Read.failure|Čtení dvojčat ze zařízení se nezdařila|Počet|Celkem|Počet všech se nezdařilo čtení dvojčat zařízení inicioval.|Žádné dimenze|
|d2c.twin.read.size|Velikost odpovědi čtení dvojčat zařízení|B|Průměr|Průměrné, minimální a maximální ze všech úspěšné, zařízení iniciované dvojčete čtení.|Žádné dimenze|
|d2c.twin.update.success|Aktualizace dvojčat úspěšné ze zařízení|Počet|Celkem|Počet všech úspěšných aktualizací dvojčete zařízení iniciované.|Žádné dimenze|
|d2c.Twin.Update.failure|Aktualizace dvojčat ze zařízení s chybami|Počet|Celkem|Počet všech neúspěšných dvojčete zařízení iniciované aktualizací.|Žádné dimenze|
|d2c.twin.update.size|Velikost aktualizace dvojčete zařízení|B|Průměr|Průměrné a minimální a maximální velikost všech úspěšné, zařízení iniciované dvojčete aktualizace.|Žádné dimenze|
|c2d.methods.success|Volání úspěšné přímé metody|Počet|Celkem|Počet všech úspěšných volání přímé metody.|Žádné dimenze|
|c2d.Methods.failure|Přímé metody vyvolání se nezdařilo|Počet|Celkem|Počet všech se nezdařilo volání metody s přímým přístupem.|Žádné dimenze|
|c2d.Methods.requestSize|Žádost o velikosti vyvolání přímé metody|B|Průměr|Průměrné, minimální a maximální počet všech úspěšných požadavků přímé metody.|Žádné dimenze|
|c2d.methods.responseSize|Velikost odpovědi vyvolání přímé metody|B|Průměr|Průměrné, minimální a maximální všechny úspěšné přímé metody odpovědí.|Žádné dimenze|
|c2d.twin.read.success|Čtení dvojčat úspěšné z back-endu|Počet|Celkem|Počet všech úspěšných čtení dvojčat iniciované back-end.|Žádné dimenze|
|c2d.Twin.Read.failure|Čtení dvojčat se nezdařilo z back-endu|Počet|Celkem|Počet všech se nezdařilo čtení dvojčat iniciované back-end.|Žádné dimenze|
|c2d.twin.read.size|Velikost odpovědi čtení dvojčat z back-endu|B|Průměr|Průměr a minimální a maximální velikost všech úspěšné, iniciované back-end dvojčete čtení.|Žádné dimenze|
|c2d.twin.update.success|Aktualizace dvojčat úspěšné z back-endu|Počet|Celkem|Počet všech úspěšné aktualizace dvojčete iniciované back-end.|Žádné dimenze|
|c2d.Twin.Update.failure|Aktualizace dvojčat se nezdařilo z back-endu|Počet|Celkem|Počet všech neúspěšných iniciované back-end dvojčete aktualizací.|Žádné dimenze|
|c2d.twin.update.size|Velikost dvojčete aktualizací z back-endu|B|Průměr|Průměrné, minimální a maximální velikost všech úspěšné, iniciované back-end dvojčete aktualizace.|Žádné dimenze|
|twinQueries.success|Úspěšné dvojčete dotazy|Počet|Celkem|Počet všech úspěšných dvojčete dotazů.|Žádné dimenze|
|twinQueries.failure|Neúspěšné dvojčete dotazy|Počet|Celkem|Počet všech neúspěšných dvojčete dotazů.|Žádné dimenze|
|twinQueries.resultSize|Velikost výsledku dotazy dvojčete|B|Průměr|Průměrné, minimální a maximální velikosti výsledek všech dotazů na dvojčata úspěšné.|Žádné dimenze|
|jobs.createTwinUpdateJob.success|Úspěšné vytvoření úlohy aktualizace dvojčete|Počet|Celkem|Počet všech po úspěšném vytvoření úlohy aktualizace dvojčete.|Žádné dimenze|
|jobs.createTwinUpdateJob.failure|Neúspěšné vytvoření úlohy aktualizace dvojčete|Počet|Celkem|Počet všech neúspěšných vytváření úlohy aktualizace dvojčete.|Žádné dimenze|
|jobs.createDirectMethodJob.success|Úspěšné vytvoření úloh vyvolání – metoda|Počet|Celkem|Počet všech po úspěšném vytvoření úlohy vyvolání přímé metody.|Žádné dimenze|
|jobs.createDirectMethodJob.failure|Neúspěšné vytvoření úloh vyvolání – metoda|Počet|Celkem|Počet všech neúspěšných vytváření úloh vyvolání přímé metody.|Žádné dimenze|
|jobs.listJobs.success|Úspěšné volání do seznamu úloh|Počet|Celkem|Počet všech úspěšných volání do seznamu úloh.|Žádné dimenze|
|jobs.listJobs.failure|Neúspěšná volání do seznamu úloh|Počet|Celkem|Počet všech neúspěšných volání do seznamu úloh.|Žádné dimenze|
|jobs.cancelJob.success|V případě úspěšné úlohy|Počet|Celkem|Počet všech úspěšných volání zrušit úlohu.|Žádné dimenze|
|jobs.cancelJob.failure|Zrušení úlohy, která selhala|Počet|Celkem|Počet všech neúspěšných volání zrušit úlohu.|Žádné dimenze|
|jobs.queryJobs.success|Úspěšné úlohy dotazů|Počet|Celkem|Počet všech úspěšných volání do úlohy dotaz.|Žádné dimenze|
|jobs.queryJobs.failure|Neúspěšné úlohy dotazy|Počet|Celkem|Počet všech neúspěšných volání do úlohy dotaz.|Žádné dimenze|
|jobs.completed|Dokončené úlohy|Počet|Celkem|Počet všech dokončených úloh.|Žádné dimenze|
|jobs.Failed|Neúspěšné úlohy|Počet|Celkem|Počet všech neúspěšných úloh.|Žádné dimenze|
|d2c.telemetry.ingress.sendThrottle|Počet chyb omezení|Počet|Celkem|Omezí počet chyb omezení kvůli propustnost zařízení|Žádné dimenze|
|dailyMessageQuotaUsed|Celkový počet zpráv používá|Počet|Průměr|Počet celkový počet zpráv, které se dnes používají. Jedná se o kumulativní hodnotu, která se resetuje na nulu v 00:00 UTC každý den.|Žádné dimenze|
|deviceDataUsage|Celkový počet devicedata využití|Počet|Celkem|Bajtů přenesených do a z jakékoli zařízení připojené k IOT hub|Žádné dimenze|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|RegistrationAttempts|Pokusy o registraci|Počet|Celkem|Počet k pokusu o registraci zařízení|Stav ProvisioningServiceName IotHubName,|
|DeviceAssignments|Přiřazená zařízení|Počet|Celkem|Počet zařízení přiřazená do služby IoT hub|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Pokusy o ověření identity|Počet|Celkem|Počet zařízení atestací pokus o|ProvisioningServiceName, stav protokolu|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|MetadataRequests|Požadavky na metadata|Počet|Počet|Počet žádostí o metadata. Cosmos DB udržuje kolekci metadat systému pro každý účet, který umožňuje vytvoření výčtu kolekce, databázím atd., a jejich konfigurací, které jsou zdarma.|GlobalDatabaseAccountName, DatabaseName, CollectionName, oblast, StatusCode|
|MongoRequestCharge|Zátěž žádostí mongo|Počet|Celkem|Spotřebované jednotky žádostí mongo|GlobalDatabaseAccountName, DatabaseName, CollectionName, oblast, CommandName, kód chyby|
|MongoRequests|Požadavky mongo|Počet|Počet|Počet zpracovaných požadavků Mongo|GlobalDatabaseAccountName, DatabaseName, CollectionName, oblast, CommandName, kód chyby|
|TotalRequestUnits|Celkový požadavek jednotky|Počet|Celkem|Požadavku že spotřebované jednotky|GlobalDatabaseAccountName, DatabaseName, CollectionName, oblast, StatusCode|
|TotalRequests|Celkový počet žádostí|Počet|Počet|Počet zpracovaných požadavků|GlobalDatabaseAccountName, DatabaseName, CollectionName, oblast, StatusCode|


## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|SuccessfulRequests|Úspěšné požadavky (Preview)|Počet|Celkem|Úspěšné požadavky pro Microsoft.EventHub (Preview)|EntityName|
|ServerErrors|Chyby serveru (Preview)|Počet|Celkem|Chyby serveru pro Microsoft.EventHub (Preview)|EntityName|
|UserErrors|Chyby uživatele (Preview)|Počet|Celkem|Chyby uživatele pro Microsoft.EventHub (Preview)|EntityName|
|QuotaExceededErrors|Chyby překročení kvóty (Preview)|Počet|Celkem|Chyby překročení kvóty pro Microsoft.EventHub (Preview)|EntityName|
|ThrottledRequests|Omezené požadavky (Preview)|Počet|Celkem|Omezené požadavky pro Microsoft.EventHub (Preview)|EntityName|
|IncomingRequests|Příchozí žádosti (Preview)|Počet|Celkem|Příchozí požadavky pro Microsoft.EventHub (Preview)|EntityName|
|IncomingMessages|Příchozí zprávy (Preview)|Počet|Celkem|Příchozí zprávy pro Microsoft.EventHub (Preview)|EntityName|
|OutgoingMessages|Odchozí zprávy (Preview)|Počet|Celkem|Odchozí zprávy pro Microsoft.EventHub (Preview)|EntityName|
|IncomingBytes|Příchozí bajty (Preview)|B|Celkem|Příchozí bajty pro Microsoft.EventHub (Preview)|EntityName|
|OutgoingBytes|Odchozí bajty (Preview)|B|Celkem|Odchozí bajty pro Microsoft.EventHub (Preview)|EntityName|
|ActiveConnections|ActiveConnections (Preview)|Počet|Průměr|Celkový počet aktivních připojení pro Microsoft.EventHub (Preview)|Žádné dimenze|
|ConnectionsOpened|Otevřená připojení (Preview)|Počet|Průměr|Otevřená připojení pro Microsoft.EventHub (Preview)|EntityName|
|ConnectionsClosed|Ukončená připojení (Preview)|Počet|Průměr|Ukončená připojení pro Microsoft.EventHub (Preview)|EntityName|
|CaptureBacklog|Zachytit backlog (Preview)|Počet|Celkem|Zachytit backlog pro Microsoft.EventHub (Preview)|EntityName|
|CapturedMessages|Zachycené zprávy (Preview)|Počet|Celkem|Zachycené zprávy pro Microsoft.EventHub (Preview)|EntityName|
|CapturedBytes|Zachycené bajty (Preview)|B|Celkem|Zachycené bajty pro Microsoft.EventHub (Preview)|EntityName|
|Velikost|Velikost (Preview)|B|Průměr|Velikost centra událostí v bajtech (Preview)|EntityName|
|INREQS|Příchozí požadavky|Počet|Celkem|Celkový počet příchozích žádostí o odeslání pro obor názvů|Žádné dimenze|
|SUCCREQ|Úspěšné požadavky|Počet|Celkem|Celkový počet úspěšných žádostí pro obor názvů|Žádné dimenze|
|FAILREQ|Neúspěšné žádosti|Počet|Celkem|Celkový počet neúspěšných žádostí pro obor názvů|Žádné dimenze|
|SVRBSY|Chyby kvůli zaneprázdněnosti serveru|Počet|Celkem|Celkový počet chyb kvůli zaneprázdnění serveru pro obor názvů|Žádné dimenze|
|INTERR|Vnitřní chyby serveru|Počet|Celkem|Celkový počet vnitřních chyb serveru pro obor názvů|Žádné dimenze|
|MISCERR|Jiné chyby|Počet|Celkem|Celkový počet neúspěšných žádostí pro obor názvů|Žádné dimenze|
|INMSGS|Příchozí zprávy (zastaralé)|Počet|Celkem|Celkový počet příchozích zpráv pro obor názvů. Tato metrika je zastaralá. Použijte místo toho metriku příchozí zprávy|Žádné dimenze|
|EHINMSGS|Příchozí zprávy|Počet|Celkem|Celkový počet příchozích zpráv pro obor názvů|Žádné dimenze|
|OUTMSGS|Odchozí zprávy (zastaralé)|Počet|Celkem|Celkový počet odchozích zpráv pro obor názvů. Tato metrika je zastaralá. Použijte místo toho metriku odchozí zprávy|Žádné dimenze|
|EHOUTMSGS|Odchozí zprávy|Počet|Celkem|Celkový počet odchozích zpráv pro obor názvů|Žádné dimenze|
|EHINMBS|Příchozí bajty (zastaralé)|B|Celkem|Příchozí propustnost zpráv centra událostí pro obor názvů. Tato metrika je zastaralá. Použijte místo toho metriku Příchozí bajty|Žádné dimenze|
|EHINBYTES|Příchozí bajty|B|Celkem|Propustnost příchozích zpráv centra událostí pro obor názvů|Žádné dimenze|
|EHOUTMBS|Odchozí bajty (zastaralé)|B|Celkem|Odchozí propustnost zpráv centra událostí pro obor názvů. Tato metrika je zastaralá. Použijte místo toho metriku Odchozí bajty|Žádné dimenze|
|EHOUTBYTES|Odchozí bajty|B|Celkem|Propustnost odchozích zpráv centra událostí pro obor názvů|Žádné dimenze|
|EHABL|Archivovat zprávy backlogu|Počet|Celkem|Archivované zprávy centra událostí v backlogu pro obor názvů|Žádné dimenze|
|EHAMSGS|Archivace zpráv|Počet|Celkem|Archivované zprávy centra událostí v oboru názvů|Žádné dimenze|
|EHAMBS|Archivovat propustnost zpráv|B|Celkem|Propustnost archivovaných zpráv Centra událostí v oboru názvů|Žádné dimenze|

## <a name="microsofthdinsightclusters"></a>Microsoft.HDInsight/clusters

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|GatewayRequests|Požadavky brány|Počet|Celkem|Počet požadavků brány|ClusterDnsName, HttpStatus|
|CategorizedGatewayRequests|Požadavky brány podle kategorií|Počet|Celkem|Počet požadavků brány podle kategorií (1xx/2xx nebo 3xx/4xx a 5xx)|ClusterDnsName, HttpStatus|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|ObservedMetricValue|Zjištěná hodnota metriky|Počet|Průměr|Hodnota vypočítaná automatickým škálováním při jeho spuštění|MetricTriggerSource|
|MetricThreshold|Mezní hodnota metriky|Počet|Průměr|Konfigurovaná mezní hodnota automatického škálování, když se automatické škálování spustilo|MetricTriggerRule|
|ObservedCapacity|Zjištěná kapacita|Počet|Průměr|Kapacita ohlášená automatickému škálování při jeho spuštění|Žádné dimenze|
|ScaleActionsInitiated|Zahájené akce škálování|Počet|Celkem|Směr operace škálování|ScaleDirection|

## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|ServiceApiHit|Celkem přístupů k rozhraní API služby|Počet|Počet|Celkový počet přístupů k rozhraní API služby|ActivityType, ActivityName|
|ServiceApiLatency|Celková latence rozhraní API služby|Milisekundy|Průměr|Celková latence požadavků na rozhraní API služby|ActivityType, ActivityName, StatusCode|
|ServiceApiResult|Celkem výsledků rozhraní API služby|Počet|Počet|Celkový počet výsledků rozhraní API služby|ActivityType, ActivityName, StatusCode|

## <a name="microsoftlocationbasedservicesaccounts"></a>Microsoft.LocationBasedServices/accounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Latence|Latence|Milisekundy|Průměr|Doba trvání volání rozhraní API|OperationName, výsledek|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|RunsStarted|Spuštěné běhy|Počet|Celkem|Počet spuštěných běhů pracovního postupu.|Žádné dimenze|
|RunsCompleted|Dokončené běhy|Počet|Celkem|Počet dokončených běhů pracovního postupu.|Žádné dimenze|
|RunsSucceeded|Úspěšné běhy|Počet|Celkem|Počet úspěšných běhů pracovního postupu.|Žádné dimenze|
|RunsFailed|Neúspěšné běhy|Počet|Celkem|Počet neúspěšných běhů pracovního postupu.|Žádné dimenze|
|RunsCancelled|Zrušené běhy|Počet|Celkem|Počet zrušených běhů pracovního postupu.|Žádné dimenze|
|RunLatency|Latence běhu|Sekundy|Průměr|Latence dokončených běhů pracovního postupu.|Žádné dimenze|
|RunSuccessLatency|Latence úspěšných běhů|Sekundy|Průměr|Latence úspěšných běhů pracovního postupu.|Žádné dimenze|
|RunThrottledEvents|Omezené události běhu|Počet|Celkem|Počet akcí pracovního postupu nebo omezených událostí triggeru.|Žádné dimenze|
|RunFailurePercentage|Procento selhání spuštění|Procento|Celkem|Procento neúspěšných spuštění pracovních postupů|Žádné dimenze|
|ActionsStarted|Spuštěné akce |Počet|Celkem|Počet spuštěných akcí pracovního postupu.|Žádné dimenze|
|ActionsCompleted|Dokončené akce |Počet|Celkem|Počet dokončených akcí pracovního postupu.|Žádné dimenze|
|ActionsSucceeded|Úspěšné akce |Počet|Celkem|Počet úspěšných akcí pracovního postupu.|Žádné dimenze|
|ActionsFailed|Neúspěšné akce|Počet|Celkem|Počet neúspěšných akcí pracovního postupu.|Žádné dimenze|
|ActionsSkipped|Vynechané akce |Počet|Celkem|Počet vynechaných akcí pracovního postupu.|Žádné dimenze|
|ActionLatency|Latence akcí |Sekundy|Průměr|Latence dokončených akcí pracovního postupu.|Žádné dimenze|
|ActionSuccessLatency|Latence úspěšných akcí |Sekundy|Průměr|Latence úspěšných akcí pracovního postupu.|Žádné dimenze|
|ActionThrottledEvents|Omezené události akcí|Počet|Celkem|Počet omezených událostí akcí pracovního postupu.|Žádné dimenze|
|TriggersStarted|Spuštěné triggery |Počet|Celkem|Počet spuštěných triggerů pracovního postupu.|Žádné dimenze|
|TriggersCompleted|Dokončené triggery |Počet|Celkem|Počet dokončených triggerů pracovního postupu.|Žádné dimenze|
|TriggersSucceeded|Úspěšné triggery |Počet|Celkem|Počet úspěšných triggerů pracovního postupu.|Žádné dimenze|
|TriggersFailed|Neúspěšné triggery |Počet|Celkem|Počet neúspěšných triggerů pracovního postupu.|Žádné dimenze|
|TriggersSkipped|Vynechané triggery|Počet|Celkem|Počet vynechaných triggerů pracovního postupu.|Žádné dimenze|
|TriggersFired|Vyvolané triggery |Počet|Celkem|Počet aktivovaných triggerů pracovního postupu.|Žádné dimenze|
|TriggerLatency|Latence triggeru |Sekundy|Průměr|Latence dokončených triggerů pracovního postupu.|Žádné dimenze|
|TriggerFireLatency|Latence při vyvolání triggeru |Sekundy|Průměr|Latence aktivovaných triggerů pracovního postupu.|Žádné dimenze|
|TriggerSuccessLatency|Latence úspěšného triggeru |Sekundy|Průměr|Latence úspěšných triggerů pracovního postupu.|Žádné dimenze|
|TriggerThrottledEvents|Omezené události triggeru|Počet|Celkem|Počet omezených událostí triggeru pracovního postupu.|Žádné dimenze|
|BillableActionExecutions|Fakturovatelné operace provedení akce|Počet|Celkem|Počet fakturovaných provedení akcí pracovního postupu|Žádné dimenze|
|BillableTriggerExecutions|Fakturovatelné operace provedení aktivačních událostí|Počet|Celkem|Počet fakturovaných provedení aktivačních událostí pracovního postupu|Žádné dimenze|
|TotalBillableExecutions|Fakturovatelné operace provedení celkem|Počet|Celkem|Počet fakturovaných provedení pracovního postupu|Žádné dimenze|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|VipAvailability|Dostupnost virtuálních IP adres|Počet|Průměr|Virtuální IP adresy koncových bodů, na základě výsledků testu dostupnosti|Virtuální IP adresa, VipPort|
|DipAvailability|Dostupnost vyhrazené IP adresy|Počet|Průměr|Koncové body DIP, v závislosti na výsledcích testu dostupnosti|ProtocolType, DipPort, virtuální IP adresa, VipPort, DipAddress|
|ByteCount|Počet bajtů|Počet|Celkem|Celkový počet bajtů přenesených v časovém období|Virtuální IP adresa, VipPort, směr|
|PacketCount|Počet paketů|Počet|Celkem|Celkový počet paketů přenášet v časovém období|Virtuální IP adresa, VipPort, směr|
|SYNCount|Počet SYN|Počet|Celkem|Celkový počet paketů SYN přenášet v časovém období|Virtuální IP adresa, VipPort, směr|
|SnatConnectionCount|Počet připojení SNAT|Počet|Celkem|Celkový počet nových připojení SNAT vytvořených v časovém období|Virtuální IP adresa, DipAddress, ConnectionState|

## <a name="microsoftnetworkdnszones"></a>Microsoft.Network/dnszones

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|QueryVolume|Množství dotazů|Počet|Celkem|Počet dotazů, které jsou zpracovány pro zónu DNS|Žádné dimenze|
|RecordSetCount|Počet sady záznamů|Počet|Maximum|Počet sad záznamů v zóně DNS|Žádné dimenze|
|RecordSetCapacityUtilization|Sada záznamů využití kapacity|Procento|Maximum|Procento kapacity sady záznamů využívaných zóny DNS|Žádné dimenze|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|PacketsInDDoS|Příchozí pakety před útoky DDoS|CountPerSecond|Maximum|Příchozí pakety před útoky DDoS|Žádné dimenze|
|PacketsDroppedDDoS|Příchozí pakety se zahodí před útoky DDoS|CountPerSecond|Maximum|Příchozí pakety se zahodí před útoky DDoS|Žádné dimenze|
|PacketsForwardedDDoS|Příchozí pakety předané před útoky DDoS|CountPerSecond|Maximum|Příchozí pakety předané před útoky DDoS|Žádné dimenze|
|TCPPacketsInDDoS|Příchozí pakety TCP před útoky DDoS|CountPerSecond|Maximum|Příchozí pakety TCP před útoky DDoS|Žádné dimenze|
|TCPPacketsDroppedDDoS|Příchozí pakety TCP zahodí před útoky DDoS|CountPerSecond|Maximum|Příchozí pakety TCP zahodí před útoky DDoS|Žádné dimenze|
|TCPPacketsForwardedDDoS|Příchozí pakety TCP předávají před útoky DDoS|CountPerSecond|Maximum|Příchozí pakety TCP předávají před útoky DDoS|Žádné dimenze|
|UDPPacketsInDDoS|Příchozí pakety UDP před útoky DDoS|CountPerSecond|Maximum|Příchozí pakety UDP před útoky DDoS|Žádné dimenze|
|UDPPacketsDroppedDDoS|Příchozí pakety UDP zahodí před útoky DDoS|CountPerSecond|Maximum|Příchozí pakety UDP zahodí před útoky DDoS|Žádné dimenze|
|UDPPacketsForwardedDDoS|Příchozí pakety UDP předávají před útoky DDoS|CountPerSecond|Maximum|Příchozí pakety UDP předávají před útoky DDoS|Žádné dimenze|
|BytesInDDoS|Příchozí bajty před útoky DDoS|BytesPerSecond|Maximum|Příchozí bajty před útoky DDoS|Žádné dimenze|
|BytesDroppedDDoS|Příchozí bajty vyřazeny před útoky DDoS|BytesPerSecond|Maximum|Příchozí bajty vyřazeny před útoky DDoS|Žádné dimenze|
|BytesForwardedDDoS|Příchozí bajty předané před útoky DDoS|BytesPerSecond|Maximum|Příchozí bajty předané před útoky DDoS|Žádné dimenze|
|TCPBytesInDDoS|Příchozí bajty TCP před útoky DDoS|BytesPerSecond|Maximum|Příchozí bajty TCP před útoky DDoS|Žádné dimenze|
|TCPBytesDroppedDDoS|Příchozí bajty TCP vyřazeny před útoky DDoS|BytesPerSecond|Maximum|Příchozí bajty TCP vyřazeny před útoky DDoS|Žádné dimenze|
|TCPBytesForwardedDDoS|Příchozí bajty TCP předané před útoky DDoS|BytesPerSecond|Maximum|Příchozí bajty TCP předané před útoky DDoS|Žádné dimenze|
|UDPBytesInDDoS|Příchozí bajty UDP před útoky DDoS|BytesPerSecond|Maximum|Příchozí bajty UDP před útoky DDoS|Žádné dimenze|
|UDPBytesDroppedDDoS|Příchozí bajty UDP vyřazeny před útoky DDoS|BytesPerSecond|Maximum|Příchozí bajty UDP vyřazeny před útoky DDoS|Žádné dimenze|
|UDPBytesForwardedDDoS|Příchozí bajty UDP předané před útoky DDoS|BytesPerSecond|Maximum|Příchozí bajty UDP předané před útoky DDoS|Žádné dimenze|
|IfUnderDDoSAttack|V části před útoky DDoS útoku nebo ne|Počet|Maximum|V části před útoky DDoS útoku nebo ne|Žádné dimenze|
|DDoSTriggerTCPPackets|Příchozí pakety protokolu TCP pro aktivaci omezení rizik útoků DDoS|CountPerSecond|Maximum|Příchozí pakety protokolu TCP pro aktivaci omezení rizik útoků DDoS|Žádné dimenze|
|DDoSTriggerUDPPackets|Příchozí pakety UDP k aktivaci omezení rizik útoků DDoS|CountPerSecond|Maximum|Příchozí pakety UDP k aktivaci omezení rizik útoků DDoS|Žádné dimenze|
|DDoSTriggerSYNPackets|Příchozí pakety SYN aktivovat omezení rizik útoků DDoS|CountPerSecond|Maximum|Příchozí pakety SYN aktivovat omezení rizik útoků DDoS|Žádné dimenze|
|VipAvailability|Dostupnost|Počet|Průměr|Průměrná dostupnost IPAddress v časovém období|Port|
|ByteCount|Počet bajtů|Počet|Celkem|Celkový počet bajtů přenesených v časovém období|Portů, směr|
|PacketCount|Počet paketů|Počet|Celkem|Celkový počet paketů přenášet v časovém období|Portů, směr|
|SynCount|Počet SYN|Počet|Celkem|Celkový počet paketů SYN přenášet v časovém období|Portů, směr|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Propustnost|Propustnost|BytesPerSecond|Celkem|Počet bajtů za sekundu, které má služby Application Gateway|Žádné dimenze|
|UnhealthyHostCount|Není v pořádku. počet hostitelů|Počet|Průměr|Počet hostitelů back-end není v pořádku. Můžete filtrovat základě back-endový fond zobrazíte v pořádku a není v pořádku hostitelů v konkrétním back-endový fond.|BackendSettingsPool|
|HealthyHostCount|V pořádku. počet hostitelů|Počet|Průměr|Počet hostitelů v dobrém stavu back-endu. Můžete filtrovat základě back-endový fond zobrazíte v pořádku a není v pořádku hostitelů v konkrétním back-endový fond.|BackendSettingsPool. |
|TotalRequests|Celkový počet žádostí|Počet|Celkem|Počet úspěšných požadavků, které má služba Application Gateway obsluhuje|BackendSettingsPool|
|FailedRequests|Neúspěšné žádosti|Počet|Celkem|Počet neúspěšných žádostí, které má služba Application Gateway obsluhuje|BackendSettingsPool|
|ResponseStatus|Stav odpovědi|Počet|Celkem|Stav odpovědi HTTP vrácené Application Gateway. Rozdělení kódů odpovědí stav může být dále categoized k zobrazení odpovědi v 2xx, 3xx, 4xx a 5xx kategorií.|HttpStatusGroup|
|CurrentConnections|Aktuální počet připojení|Počet|Celkem|Počet aktuální připojení ke službě Application Gateway|Žádné dimenze|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TunnelAverageBandwidth|Šířka pásma tunelu|BytesPerSecond|Průměr|Průměrná šířka pásma tunelu v bajtech za sekundu|ConnectionName RemoteIP|
|TunnelEgressBytes|Odchozí bajty tunelového propojení|B|Celkem|Odchozí bajty tunelového propojení|ConnectionName RemoteIP|
|TunnelIngressBytes|Příchozí bajty tunelového propojení|B|Celkem|Příchozí bajty tunelového propojení|ConnectionName RemoteIP|
|TunnelEgressPackets|Odchozí pakety tunelového propojení|Počet|Celkem|Počet odchozích paketů tunelového propojení|ConnectionName RemoteIP|
|TunnelIngressPackets|Příchozí pakety tunelového propojení|Počet|Celkem|Počet příchozích paketů tunelového propojení|ConnectionName RemoteIP|
|TunnelEgressPacketDropTSMismatch|Zahození paketu neodpovídá odchozího přenosu dat TS tunelového propojení|Počet|Celkem|Počet odchozích paketů přetažení z neshoda selektor provozu tunelového propojení|ConnectionName RemoteIP|
|TunnelIngressPacketDropTSMismatch|Zahození paketu neodpovídá příchozího přenosu dat TS tunelového propojení|Počet|Celkem|Počet příchozích paketů přetažení z neshoda selektor provozu tunelového propojení|ConnectionName RemoteIP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Průměr|Služba BITS ingressing Azure za sekundu|Žádné dimenze|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Průměr|Služba BITS egressing Azure za sekundu|Žádné dimenze|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|QpsByEndpoint|Dotazy podle koncový bod vrátil|Počet|Celkem|Počet pokusů, které v daném časovém rámci byl vrácen koncových bodů Traffic Manageru|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Stav koncového bodu pomocí koncového bodu|Počet|Maximum|1, pokud koncového bodu sondu stav "povoleno", jinak 0.|EndpointName|

## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft.Network/networkWatchers/connectionMonitors

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|ProbesFailedPercent|% Testů paměti se nezdařilo|Procento|Průměr|% testy monitorování připojení se nezdařilo|Žádné dimenze|
|AverageRoundtripMs|Střední Doba odezvy (ms)|Milisekundy|Průměr|Průměrná síťové operace round-trip doba (ms) pro připojení k monitorování mezi zdrojem a cílem odeslané testy paměti|Žádné dimenze|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Registration.all|Registrace – operace|Počet|Celkem|Počet všech úspěšných operací s registrací (vytvoření, aktualizace, dotazy a odstranění) |Žádné dimenze|
|Registration.Create|Registrace – vytvořit operace|Počet|Celkem|Počet všech úspěšných vytvoření registrací|Žádné dimenze|
|Registration.Update|Registrace – aktualizovat operace|Počet|Celkem|Počet všech úspěšných aktualizací registrací|Žádné dimenze|
|Registration.Get|Registrace – přečíst operace|Počet|Celkem|Počet všech úspěšných dotazů registrací|Žádné dimenze|
|registration.delete|Registrace – odstranit operace|Počet|Celkem|Počet všech úspěšných odstranění registrací|Žádné dimenze|
|příchozí|Příchozí zprávy|Počet|Celkem|Počet všech úspěšných volání rozhraní API, pomocí kterých se odesílají data |Žádné dimenze|
|incoming.scheduled|Odeslaná plánovaná nabízená oznámení|Počet|Celkem|Zrušená plánovaná nabízená oznámení|Žádné dimenze|
|incoming.scheduled.cancel|Zrušená plánovaná nabízená oznámení|Počet|Celkem|Zrušená plánovaná nabízená oznámení|Žádné dimenze|
|Scheduled.Pending|Čekající plánovaná oznámení|Počet|Celkem|Čekající plánovaná oznámení|Žádné dimenze|
|Installation.all|Operace správy instalace|Počet|Celkem|Operace správy instalace|Žádné dimenze|
|Installation.Get|Operace získání instalace|Počet|Celkem|Operace získání instalace|Žádné dimenze|
|Installation.upsert|Vytvořit nebo aktualizovat operace instalace|Počet|Celkem|Vytvořit nebo aktualizovat operace instalace|Žádné dimenze|
|Installation.patch|Opravit operace instalace|Počet|Celkem|Opravit operace instalace|Žádné dimenze|
|Installation.delete|Operace odstranění instalace|Počet|Celkem|Operace odstranění instalace|Žádné dimenze|
|outgoing.allpns.Success|Úspěšná oznámení|Počet|Celkem|Počet všech úspěšných oznámení|Žádné dimenze|
|outgoing.allpns.invalidpayload|Chyby datové části|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože systém oznámení platformy vrátil chybu, která se týká chybné datové části|Žádné dimenze|
|outgoing.allpns.pnserror|Chyby externího systému oznámení|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože při komunikaci se systémem oznámení platformy došlo k problému (nezahrnuje problémy s ověřováním)|Žádné dimenze|
|outgoing.allpns.channelerror|Chyby kanálů|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože kanál nebyl platný nebo přidružený ke správné aplikaci, byl omezený, nebo jeho platnost vypršela|Žádné dimenze|
|outgoing.allpns.badorexpiredchannel|Chyby – chybný kanál nebo vypršení časového limitu kanálu|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože v registraci vypršel nebo není platný kanál, token nebo ID registrace|Žádné dimenze|
|outgoing.wns.Success|WNS – úspěšná oznámení|Počet|Celkem|Počet všech úspěšných oznámení|Žádné dimenze|
|outgoing.wns.invalidcredentials|WNS – chyby autorizace (neplatné přihlašovací údaje)|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože systém oznámení platformy nepřijal poskytnuté přihlašovací údaje, nebo jsou přihlašovací údaje blokované (Windows Live nerozpozná přihlašovací údaje).|Žádné dimenze|
|outgoing.wns.badchannel|WNS – chyba špatného kanálu|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože se v registraci nerozpoznal parametr ChannelURI (stav Služby nabízených oznámení Windows: 404 Nenalezeno)|Žádné dimenze|
|outgoing.wns.expiredchannel|WNS – chyba vypršení časového limitu kanálu|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože vypršela platnost parametru ChannelURI (stav Služby nabízených oznámení Windows: 410 Trvale není k dispozici)|Žádné dimenze|
|outgoing.wns.throttled|WNS – omezená oznámení|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože Služba nabízených oznámení Windows omezuje tuto aplikaci (stav Služby nabízených oznámení Windows: 406 Nepřijatelný požadavek)|Žádné dimenze|
|outgoing.wns.tokenproviderunreachable|WNS – chyby autorizace (nedostupné)|Počet|Celkem|Služby Windows Live nejsou dostupné.|Žádné dimenze|
|outgoing.wns.invalidtoken|WNS – chyby autorizace (neplatný token)|Počet|Celkem|Token, který se poskytl Službě nabízených oznámení Windows, není platný (stav Služby nabízených oznámení Windows: 401 Neautorizováno).|Žádné dimenze|
|outgoing.wns.wrongtoken|WNS – chyby autorizace (chybný token)|Počet|Celkem|Token se poskytl službě nabízených oznámení Windows je platný, ale pro jinou aplikaci (stav služby nabízených oznámení Windows: 403 Zakázáno). To může nastat, pokud je parametr ChannelURI v registraci přidružený k jiné aplikaci. Zkontrolujte, že klientská aplikace souvisí s stejnou aplikaci, jehož přihlašovací údaje jsou v centru oznámení.|Žádné dimenze|
|outgoing.wns.invalidnotificationformat|WNS – neplatný formát oznámení|Počet|Celkem|Formát oznámení není platný (stav služby nabízených oznámení Windows: 400). Všimněte si, že služby nabízených oznámení Windows není odmítnout všechny neplatné datové části.|Žádné dimenze|
|outgoing.wns.invalidnotificationsize|WNS – chyba neplatné velikosti oznámení|Počet|Celkem|Datová část oznámení je moc velká (stav Služby nabízených oznámení Windows: 413).|Žádné dimenze|
|outgoing.wns.channelthrottled|WNS – omezený kanál|Počet|Celkem|Oznámení se ignoruje, protože v registraci se omezuje parametr ChannelURI (hlavička odpovědi Služby nabízených oznámení Windows: X-WNS-NotificationStatus:channelThrottled).|Žádné dimenze|
|outgoing.wns.channeldisconnected|WNS – odpojený kanál|Počet|Celkem|Oznámení se ignoruje, protože v registraci se omezuje parametr ChannelURI (hlavička odpovědi Služby nabízených oznámení Windows: X-WNS-DeviceConnectionStatus: disconnected).|Žádné dimenze|
|outgoing.wns.dropped|WNS – vynechaná oznámení|Počet|Celkem|Oznámení se ignoruje, protože v registraci se omezuje parametr ChannelURI (X-WNS-NotificationStatus: dropped, ale ne X-WNS-DeviceConnectionStatus: disconnected).|Žádné dimenze|
|outgoing.wns.pnserror|WNS – chyby|Počet|Celkem|Oznámení se nedoručilo kvůli chybám při komunikaci se Službou nabízených oznámení Windows.|Žádné dimenze|
|outgoing.wns.authenticationerror|WNS – chyby ověřování|Počet|Celkem|Oznámení se nedoručilo kvůli chybám při komunikaci s Windows Live, neplatným přihlašovacím údajům, nebo chybnému tokenu.|Žádné dimenze|
|outgoing.apns.success|APNS – úspěšná oznámení|Počet|Celkem|Počet všech úspěšných oznámení|Žádné dimenze|
|outgoing.apns.invalidcredentials|Chyby autorizace APNs|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože systém oznámení platformy nepřijal poskytnuté přihlašovací údaje, nebo jsou přihlašovací údaje blokované|Žádné dimenze|
|outgoing.apns.badchannel|Chyba APNS – špatný kanál|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože token je neplatný (kód stavu APNs: 8)|Žádné dimenze|
|outgoing.apns.expiredchannel|Chyba APNS – kanálu vypršel časový limit|Počet|Celkem|Počet tokenů, kterým kanál zpětné vazby APNs zrušil platnost|Žádné dimenze|
|outgoing.apns.invalidnotificationsize|APNS – chyba neplatné velikosti oznámení|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože datová část byla moc velká (kód stavu APNs: 7)|Žádné dimenze|
|outgoing.apns.pnserror|APNS – chyby|Počet|Celkem|Počet nabídek, které nebyly úspěšné kvůli chybám při komunikaci s APNs|Žádné dimenze|
|outgoing.gcm.success|GCM – úspěšná oznámení|Počet|Celkem|Počet všech úspěšných oznámení|Žádné dimenze|
|outgoing.gcm.invalidcredentials|GCM – chyby autorizace (neplatné přihlašovací údaje)|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože systém oznámení platformy nepřijal poskytnuté přihlašovací údaje, nebo jsou přihlašovací údaje blokované|Žádné dimenze|
|outgoing.gcm.badchannel|GCM – chyba špatného kanálu|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože se v registraci nerozpoznal parametr registrationId (výsledek GCM: Invalid Registration)|Žádné dimenze|
|outgoing.gcm.expiredchannel|GCM – chyba vypršení časového limitu kanálu|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože vypršela platnost registrationId v registraci (výsledek GCM: NotRegistered)|Žádné dimenze|
|outgoing.gcm.throttled|GCM – omezená oznámení|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože služba GCM omezila tuto aplikaci (kód stavu GCM: 501–599 nebo výsledek: Unavailable)|Žádné dimenze|
|outgoing.gcm.invalidnotificationformat|GCM – neplatný formát oznámení|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože datová část neměla správný formát (výsledek GCM: InvalidDataKey nebo InvalidTtl)|Žádné dimenze|
|outgoing.gcm.invalidnotificationsize|GCM – chyba neplatné velikosti oznámení|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože datová část byla moc velká (výsledek GCM: MessageTooBig)|Žádné dimenze|
|outgoing.gcm.wrongchannel|GCM – chyba nesprávného kanálu|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože registrationId v registraci není přidružené k aktuální aplikaci (výsledek GCM: InvalidPackageName)|Žádné dimenze|
|outgoing.gcm.pnserror|GCM – chyby|Počet|Celkem|Počet nabídek, které nebyly úspěšné kvůli chybám při komunikaci s GCM|Žádné dimenze|
|outgoing.gcm.authenticationerror|Chyby ověřování GCM|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože systém oznámení platformy nepřijal poskytnuté přihlašovací údaje, nebo jsou přihlašovací údaje blokované, nebo v aplikaci není správně nakonfigurované SenderId (výsledek GCM: MismatchedSenderId)|Žádné dimenze|
|outgoing.mpns.success|MPNS – úspěšná oznámení|Počet|Celkem|Počet všech úspěšných oznámení|Žádné dimenze|
|outgoing.mpns.invalidcredentials|MPNS – neplatné přihlašovací údaje|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože systém oznámení platformy nepřijal poskytnuté přihlašovací údaje, nebo jsou přihlašovací údaje blokované|Žádné dimenze|
|outgoing.mpns.badchannel|MPNS – chyba špatného kanálu|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože se v registraci nerozpoznal parametr ChannelURI (stav MPNS: 404 Nenalezeno)|Žádné dimenze|
|outgoing.mpns.throttled|MPNS – omezená oznámení|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože MPNS omezuje tuto aplikaci (WNS MPNS: 406 Nepřijatelný požadavek)|Žádné dimenze|
|outgoing.mpns.invalidnotificationformat|MPNS – neplatný formát oznámení|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože datová část v oznámení byla moc velká|Žádné dimenze|
|outgoing.mpns.channeldisconnected|MPNS – odpojený kanál|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože parametr ChannelURI v registraci byl odpojený (stav MPNS: 412 Nenalezeno)|Žádné dimenze|
|outgoing.mpns.dropped|MPNS – vynechaná oznámení|Počet|Celkem|Počet nabídek, které služba MPNS ignorovala (hlavička odpovědi MPNS: X-NotificationStatus: QueueFull nebo Suppressed)|Žádné dimenze|
|outgoing.mpns.pnserror|MPNS – chyby|Počet|Celkem|Počet nabídek, které nebyly úspěšné kvůli chybám při komunikaci s MPNS|Žádné dimenze|
|outgoing.mpns.authenticationerror|MPNS – chyby ověřování|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože systém oznámení platformy nepřijal poskytnuté přihlašovací údaje, nebo jsou přihlašovací údaje blokované|Žádné dimenze|
|notificationhub.pushes|Všechna odchozí oznámení|Počet|Celkem|Všechna odchozí oznámení centra oznámení|Žádné dimenze|
|incoming.all.requests|Všechny příchozí požadavky|Počet|Celkem|Celkový počet příchozích požadavků pro centrum oznámení|Žádné dimenze|
|Incoming.all.failedrequests|Všechny neúspěšné příchozí požadavky|Počet|Celkem|Celkový počet neúspěšných příchozích požadavků pro centrum oznámení|Žádné dimenze|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/workspaces
(Public Preview)

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
Average_ % volných uzlů Inode|% Volných uzlů Inode|Počet|Průměr|Average_ % volných uzlů Inode|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_ % volného místa|% Volného místa|Počet|Průměr|Average_ % volného místa|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_ % použitých uzlů Inode|% Použitých uzlů Inode|Počet|Průměr|Average_ % použitých uzlů Inode|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_ % využitého místa|% Využitého místa|Počet|Průměr|Average_ % využitého místa|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Disk přečtené bajty/s|Bajty čtení z disku/s|Počet|Průměr|Average_Disk přečtené bajty/s|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Disk přečtené strany/s|Čtení disku/s|Počet|Průměr|Average_Disk přečtené strany/s|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Disk přenosy/s|Přenosy disku/s|Počet|Průměr|Average_Disk přenosy/s|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Disk zapsané bajty/s|Bajty zapisování na disk/s|Počet|Průměr|Average_Disk zapsané bajty/s|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Disk zapsané strany/s|Zápis disku/s|Počet|Průměr|Average_Disk zapsané strany/s|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Free v megabajtech|Volné megabajty|Počet|Průměr|Average_Free v megabajtech|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Logical bajtů disku/s|Bajtů logického disku/s|Počet|Průměr|Average_Logical bajtů disku/s|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Dostupná paměť v % Average_|Dostupná paměť v %|Počet|Průměr|Dostupná paměť v % Average_|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_ % dostupného odkládacího prostoru|% Dostupného odkládacího prostoru|Počet|Průměr|Average_ % dostupného odkládacího prostoru|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_ % využité paměti|% Využité paměti|Počet|Průměr|Average_ % využité paměti|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Využitý prostor záměny v Average_ %|Využitý prostor záměny v %|Počet|Průměr|Využitý prostor záměny v Average_ %|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Available paměť v MB|Dostupná paměť v MB|Počet|Průměr|Average_Available paměť v MB|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Available prostor záměny v MB|Dostupný prostor záměny v MB|Počet|Průměr|Average_Available prostor záměny v MB|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Page přečtené strany/s|Čtení stránek/s|Počet|Průměr|Average_Page přečtené strany/s|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Page zapsané strany/s|Zápisy stránek/s|Počet|Průměr|Average_Page zapsané strany/s|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Pages za sekundu|Stránky/s|Počet|Průměr|Average_Pages za sekundu|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Used prostor záměny v MB|Dostupná paměť v MB|Počet|Průměr|Average_Used prostor záměny v MB|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Used paměť v MB|Dostupný prostor záměny v MB|Počet|Průměr|Average_Used paměť v MB|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Odeslané bajty Average_Total|Celkový počet bajtů přenesených|Počet|Průměr|Odeslané bajty Average_Total|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Přijaté bajty Average_Total|Celkový počet přijatých bajtů|Počet|Průměr|Přijaté bajty Average_Total|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Total bajtů|Bajty celkem|Počet|Průměr|Average_Total bajtů|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Total pakety odesílané informace|Celkový počet paketů odesílané informace|Počet|Průměr|Average_Total pakety odesílané informace|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Total obdržených paketů|Celkový počet přijatých paketů|Počet|Průměr|Average_Total obdržených paketů|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Chyby příjmu Average_Total|Chyby celkem příjmu|Počet|Průměr|Chyby příjmu Average_Total|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Chyby odeslání Average_Total|Celkový počet odesílání chyb|Počet|Průměr|Chyby odeslání Average_Total|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Total kolizí|Celkový počet kolizí|Počet|Průměr|Average_Total kolizí|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Avg. Doba disku/čtení|Střední Doba disku/čtení|Počet|Průměr|Average_Avg. Doba disku/čtení|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Avg. Doba disku/přenos|Střední Doba disku/přenos|Počet|Průměr|Average_Avg. Doba disku/přenos|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Avg. Doby disku/zápis|Střední Doby disku/zápis|Počet|Průměr|Average_Avg. Doby disku/zápis|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Physical bajtů disku/s|Bajtů fyzického disku/s|Počet|Průměr|Average_Physical bajtů disku/s|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Pct privilegovaného času|Procento privilegovaného času|Počet|Průměr|Average_Pct privilegovaného času|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Pct uživatelského času|Procento uživatelského času|Počet|Průměr|Average_Pct uživatelského času|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
KB paměti Average_Used|Využité paměti kB|Počet|Průměr|KB paměti Average_Used|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Virtual sdílené paměti|Sdílené virtuální paměti|Počet|Průměr|Average_Virtual sdílené paměti|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
% Času DPC Average_|Čas DPC v %|Počet|Průměr|% Času DPC Average_|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_ čas nečinnosti v %|Čas nečinnosti v %|Počet|Průměr|Average_ čas nečinnosti v %|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
% Času přerušení Average_|Čas přerušení v %|Počet|Průměr|% Času přerušení Average_|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Doba čekání na vstupně-výstupních operací Average_ %|Doba čekání % vstupně-výstupních operací|Počet|Průměr|Doba čekání na vstupně-výstupních operací Average_ %|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_ dobrý čas v %|Dobrý čas v %|Počet|Průměr|Average_ dobrý čas v %|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_ % privilegovaného času|% Privilegovaného času|Počet|Průměr|Average_ % privilegovaného času|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_ % času procesoru|% Času procesoru|Počet|Průměr|Average_ % času procesoru|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_ % uživatelského času|Uživatelský čas v %|Počet|Průměr|Average_ % uživatelského času|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Free fyzické paměti|Volná fyzická paměť|Počet|Průměr|Average_Free fyzické paměti|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Free místo ve stránkovacích souborech|Volné místo ve stránkovacích souborech|Počet|Průměr|Average_Free místo ve stránkovacích souborech|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Free virtuální paměti|Volná virtuální paměť|Počet|Průměr|Average_Free virtuální paměti|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Processes|Procesy|Počet|Průměr|Average_Processes|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Size uložená v stránkovacích souborech|Velikost uložená ve stránkovacích souborech|Počet|Průměr|Average_Size uložená v stránkovacích souborech|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Uptime|Doba provozu|Počet|Průměr|Average_Uptime|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Users|Uživatelé|Počet|Průměr|Average_Users|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Délka fronty disku Average_Current|Aktuální délka fronty disku|Počet|Průměr|Délka fronty disku Average_Current|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Available paměť v megabajtech|Počet MB k dispozici|Počet|Průměr|Average_Available paměť v megabajtech|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_ % potvrzených bajtů|% Využití potvrzených bajtů|Počet|Průměr|Average_ % potvrzených bajtů|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Přijaté Average_Bytes/s|Přijaté bajty/s|Počet|Průměr|Přijaté Average_Bytes/s|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Odeslané Average_Bytes/s|Odeslané bajty/s|Počet|Průměr|Odeslané Average_Bytes/s|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Average_Bytes celkem/s|Bajty celkem/s|Počet|Průměr|Average_Bytes celkem/s|Počítače, název_objektu, InstanceName, Cesta_k_čítači, SourceSystem|
Prezenční signál|Prezenční signál|Počet|Průměr|Prezenční signál|Počítače, OSType, verze, SourceComputerId|
Aktualizace|Aktualizace|Počet|Průměr|Aktualizace|Počítače, produktů, klasifikace, UpdateState volitelné, schválené|
Událost|Událost|Počet|Průměr|Událost|Zdroj protokolu událostí, počítače, EventCategory, EventLevel, EventLevelName, ID události|


## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|QueryDuration|Doba trvání dotazu|Milisekundy|Průměr|Doba trvání dotazu jazyka DAX v posledního intervalu|Žádné dimenze|
|QueryPoolJobQueueLength|Vlákna: Dotazování délka fronty fondu úloh|Počet|Průměr|Počet úloh ve frontě fondu vláken dotazů.|Žádné dimenze|
|qpu_high_utilization_metric|Vysoké využití QPU|Počet|Celkem|Vysoké využití QPU za poslední minutu, 1 pro využití vysoké QPU, jinak 0|Žádné dimenze|
|memory_metric|Memory (Paměť)|B|Průměr|Paměť. Rozsah 0 – 3 GB pro A1, 0 – 5 GB pro A2, A3 0 až 10 GB, 0-25 GB pro A4, 0 – 50 GB pro A5 a 0 – 100 GB pro A6|Žádné dimenze|
|memory_thrashing_metric|Thrashing paměti|Procento|Průměr|Průměrný thrashing paměti.|Žádné dimenze|

## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|ListenerConnections-Success|ListenerConnections-Success|Počet|Celkem|Úspěšná připojení ListenerConnections pro Microsoft.Relay|EntityName|
|ListenerConnections-ClientError|ListenerConnections-ClientError|Počet|Celkem|ClientError v ListenerConnections pro Microsoft.Relay|EntityName|
|ListenerConnections-ServerError|ListenerConnections-ServerError|Počet|Celkem|ServerError na ListenerConnections pro Microsoft.Relay|EntityName|
|SenderConnections-Success|SenderConnections-Success|Počet|Celkem|Úspěšná připojení SenderConnections pro Microsoft.Relay|EntityName|
|SenderConnections-ClientError|SenderConnections-ClientError|Počet|Celkem|ClientError v SenderConnections pro Microsoft.Relay|EntityName|
|SenderConnections-ServerError|SenderConnections-ServerError|Počet|Celkem|ServerError na SenderConnections pro Microsoft.Relay|EntityName|
|ListenerConnections-TotalRequests|ListenerConnections-TotalRequests|Počet|Celkem|ListenerConnections celkem pro Microsoft.Relay|EntityName|
|SenderConnections-TotalRequests|SenderConnections-TotalRequests|Počet|Celkem|Celkem žádostí SenderConnections pro Microsoft.Relay|EntityName|
|ActiveConnections|ActiveConnections|Počet|Celkem|ActiveConnections celkem pro Microsoft.Relay|EntityName|
|ActiveListeners|ActiveListeners|Počet|Celkem|ActiveListeners celkem pro Microsoft.Relay|EntityName|
|BytesTransferred|BytesTransferred|Počet|Celkem|BytesTransferred celkem pro Microsoft.Relay|EntityName|
|ListenerDisconnects|ListenerDisconnects|Počet|Celkem|ListenerDisconnects celkem pro Microsoft.Relay|EntityName|
|SenderDisconnects|SenderDisconnects|Počet|Celkem|SenderDisconnects celkem pro Microsoft.Relay|EntityName|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|SearchLatency|Latence vyhledávání|Sekundy|Průměr|Hledání Průměrná latence pro službu search|Žádné dimenze|
|SearchQueriesPerSecond|Vyhledávací dotazy za sekundu|CountPerSecond|Průměr|Vyhledávací dotazy za sekundu pro vyhledávací službu|Žádné dimenze|
|ThrottledSearchQueriesPercentage|Procento omezených vyhledávacích dotazů|Procento|Průměr|Procento vyhledávacích dotazů, které byly omezené služby search|Žádné dimenze|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|SuccessfulRequests|Úspěšné požadavky (Preview)|Počet|Celkem|Celkový počet úspěšných žádostí pro obor názvů (Preview)|EntityName, |
|ServerErrors|Chyby serveru (Preview)|Počet|Celkem|Chyby serveru pro Microsoft.ServiceBus (Preview)|EntityName, |
|UserErrors|Chyby uživatele (Preview)|Počet|Celkem|Chyby uživatele pro Microsoft.ServiceBus (Preview)|EntityName, |
|ThrottledRequests|Omezené požadavky (Preview)|Počet|Celkem|Omezené požadavky pro Microsoft.ServiceBus (Preview)|EntityName, |
|IncomingRequests|Příchozí žádosti (Preview)|Počet|Celkem|Příchozí požadavky pro Microsoft.ServiceBus (Preview)|EntityName|
|IncomingMessages|Příchozí zprávy (Preview)|Počet|Celkem|Příchozí zprávy pro Microsoft.ServiceBus (Preview)|EntityName|
|OutgoingMessages|Odchozí zprávy (Preview)|Počet|Celkem|Odchozí zprávy pro Microsoft.ServiceBus (Preview)|EntityName|
|ActiveConnections|ActiveConnections (Preview)|Počet|Celkem|Celkový počet aktivních připojení pro Microsoft.ServiceBus (Preview)|Žádné dimenze|
|Velikost|Velikost (Preview)|B|Průměr|Velikost fronty nebo tématu v bajtech (Preview)|EntityName|
|Zprávy|Počet zpráv ve frontě nebo tématu (Preview)|Počet|Průměr|Počet zpráv ve frontě nebo tématu (Preview)|EntityName|
|ActiveMessages|Počet aktivních zpráv ve frontě nebo tématu (Preview)|Počet|Průměr|Počet aktivních zpráv ve frontě nebo tématu (Preview)|EntityName|
|CPUXNS|Využití CPU na obor názvů|Procento|Maximum|Metrika využití procesoru v oboru názvů služby Service Bus na úrovni Premium |Žádné dimenze|
|WSXNS|Využití paměti na obor názvů|Procento|Maximum|Metrika využití paměti v oboru názvů služby Service Bus na úrovni Premium |Žádné dimenze|

## <a name="microsoftsignalrservicesignalr"></a>Microsoft.SignalRService/SignalR

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|ConnectionCount|Počet připojení|Počet|Maximum|Počet připojení uživatelů.|Žádné dimenze|
|ConnectionCountPerSecond|Počet připojení za sekundu|CountPerSecond|Průměr|Počet průměrné připojení za sekundu.|Žádné dimenze|
|MessageCount|Počet zpráv|Počet|Maximum|Celkový počet zpráv za měsíc|Žádné dimenze|
|MessageCountPerSecond|Počet zpráv za sekundu|CountPerSecond|Průměr|Průměrný počet zpráv|Žádné dimenze|
|MessageUsed|Zpráva|Procento|Maximum|Procento zpráv, které se používají v měsíci|Žádné dimenze|
|ConnectionUsed|Připojení použité|Procento|Maximum|Procento připojení se používají.|Žádné dimenze|
|UserErrors|Chyby uživatele|Procento|Maximum|Procento chyb uživatele|Žádné dimenze|
|SystemErrors|Chyby systému|Procento|Maximum|Procento chyb systému|Žádné dimenze|
|SystemLoad|Zatížení systému|Procento|Maximum|Procento zatížení systému|Žádné dimenze|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|cpu_percent|Procento CPU|Procento|Průměr|Procento CPU|Žádné dimenze|
|physical_data_read_percent|Procento datových V/V|Procento|Průměr|Procento datových V/V|Žádné dimenze|
|log_write_percent|Procento v/v protokolu|Procento|Průměr|Procento v/v protokolu|Žádné dimenze|
|dtu_consumption_percent|Procento DTU|Procento|Průměr|Procento DTU|Žádné dimenze|
|úložiště|Celkovou velikost databáze|B|Maximum|Celkovou velikost databáze|Žádné dimenze|
|connection_successful|Úspěšná připojení|Počet|Celkem|Úspěšná připojení|Žádné dimenze|
|connection_failed|Neúspěšná připojení|Počet|Celkem|Neúspěšná připojení|Žádné dimenze|
|blocked_by_firewall|Blokovaná bránou Firewall|Počet|Celkem|Blokovaná bránou Firewall|Žádné dimenze|
|Zablokování|Zablokování|Počet|Celkem|Zablokování|Žádné dimenze|
|storage_percent|Procento velikosti databáze|Procento|Maximum|Procento velikosti databáze|Žádné dimenze|
|xtp_storage_percent|Procento úložiště OLTP v paměti|Procento|Průměr|Procento úložiště OLTP v paměti|Žádné dimenze|
|workers_percent|Procento pracovních procesů|Procento|Průměr|Procento pracovních procesů|Žádné dimenze|
|sessions_percent|Procento relací|Procento|Průměr|Procento relací|Žádné dimenze|
|dtu_limit|Omezení jednotek DTU|Počet|Průměr|Omezení jednotek DTU|Žádné dimenze|
|dtu_used|DTU použít|Počet|Průměr|DTU použít|Žádné dimenze|
|dwu_limit|Limit jednotky|Počet|Maximum|Limit jednotky|Žádné dimenze|
|dwu_consumption_percent|Procento DWU|Procento|Maximum|Procento DWU|Žádné dimenze|
|dwu_used|Použít DWU|Počet|Maximum|Použít DWU|Žádné dimenze|
|dw_cpu_percent|Úrovni uzlu DW procento využití procesoru|Procento|Průměr|Úrovni uzlu DW procento využití procesoru|DwLogicalNodeId|
|dw_physical_data_read_percent|Procento datových v/v úrovně uzlu DW|Procento|Průměr|Procento datových v/v úrovně uzlu DW|DwLogicalNodeId|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|cpu_percent|Procento CPU|Procento|Průměr|Procento CPU|Žádné dimenze|
|physical_data_read_percent|Procento datových V/V|Procento|Průměr|Procento datových V/V|Žádné dimenze|
|log_write_percent|Procento v/v protokolu|Procento|Průměr|Procento v/v protokolu|Žádné dimenze|
|dtu_consumption_percent|Procento DTU|Procento|Průměr|Procento DTU|Žádné dimenze|
|storage_percent|Procento úložiště|Procento|Průměr|Procento úložiště|Žádné dimenze|
|workers_percent|Procento pracovních procesů|Procento|Průměr|Procento pracovních procesů|Žádné dimenze|
|sessions_percent|Procento relací|Procento|Průměr|Procento relací|Žádné dimenze|
|eDTU_limit|omezení eDTU|Počet|Průměr|omezení eDTU|Žádné dimenze|
|storage_limit|Limit úložiště.|B|Průměr|Limit úložiště.|Žádné dimenze|
|eDTU_used|použít eDTU|Počet|Průměr|použít eDTU|Žádné dimenze|
|storage_used|Využité úložiště|B|Průměr|Využité úložiště|Žádné dimenze|
|xtp_storage_percent|Procento úložiště OLTP v paměti|Procento|Průměr|Procento úložiště OLTP v paměti|Žádné dimenze|

## <a name="microsoftsqlservers"></a>Microsoft.Sql/servers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|dtu_consumption_percent|Procento DTU|Procento|Průměr|Procento DTU|ElasticPoolResourceId|
|storage_used|Využité úložiště|B|Průměr|Využité úložiště|ElasticPoolResourceId|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|UsedCapacity|Použitá kapacita|B|Průměr|Kapacita využitá účtem|Žádné dimenze|
|Transakce|Transakce|Počet|Celkem|Počet požadavků provedených na službu úložiště nebo zadanou operaci rozhraní API. Toto číslo zahrnuje úspěšné i neúspěšné požadavky a požadavky, které došlo k chybě. Hodnota ResponseType dimenzi používejte pro počet různých typů odpovědi.|Hodnota ResponseType, GeoType, ApiName|
|Příchozí přenos dat|Příchozí přenos dat|B|Celkem|Množství příchozích dat v bajtech. Toto číslo zahrnuje příchozí přenos dat z externího klienta do služby Azure Storage i příchozí přenos dat v rámci Azure.|GeoType ApiName|
|Výchozí přenos|Výchozí přenos|B|Celkem|Objem odchozích přenosů dat v bajtech. Toto číslo zahrnuje výchozí přenos dat z externího klienta do služby Azure Storage i výchozí přenos dat v rámci Azure. Kvůli tomu toto číslo nepředstavuje fakturovatelný výchozí přenos dat.|GeoType ApiName|
|SuccessServerLatency|Latence serveru při úspěchu|Milisekundy|Průměr|Průměrná latence služby Azure Storage ke zpracování úspěšného požadavku v milisekundách. Tato hodnota nezahrnuje síťovou latenci určenou v AverageE2ELatency.|GeoType ApiName|
|SuccessE2ELatency|Celková latence při úspěchu|Milisekundy|Průměr|Průměrná latence začátku do konce úspěšných požadavků provedených na službu úložiště nebo zadanou operaci rozhraní API v milisekundách. Tato hodnota zahrnuje čas zpracování ve službě Azure Storage potřebný k přečtení požadavku, odeslání odpovědi a přijetí potvrzení dané odpovědi.|GeoType ApiName|
|Dostupnost|Dostupnost|Procento|Průměr|Procento dostupnosti pro službu úložiště nebo zadanou operaci rozhraní API. Dostupnost se počítá tak, že hodnota TotalBillableRequests vydělí počtem příslušných požadavků, včetně těch, ke které došlo k neočekávané chybě. Všechny neočekávané chyby za následek sníženou dostupnost pro službu úložiště nebo zadanou operaci rozhraní API.|GeoType ApiName|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|BlobCapacity|Kapacita služby Blob|B|Celkem|Velikost úložiště využitá službou Blob Service účtu úložiště v bajtech|BlobType|
|BlobCount|Počet objektů blob|Počet|Celkem|Počet objektů blob ve službě Blob Service účtu úložiště|BlobType|
|ContainerCount|Počet kontejnerů služby Blob|Počet|Průměr|Počet kontejnerů ve službě Blob Service účtu úložiště|Žádné dimenze|
|Transakce|Transakce|Počet|Celkem|Počet požadavků provedených na službu úložiště nebo zadanou operaci rozhraní API. Toto číslo zahrnuje úspěšné i neúspěšné požadavky a požadavky, které došlo k chybě. Hodnota ResponseType dimenzi používejte pro počet různých typů odpovědi.|Hodnota ResponseType, GeoType, ApiName|
|Příchozí přenos dat|Příchozí přenos dat|B|Celkem|Množství příchozích dat v bajtech. Toto číslo zahrnuje příchozí přenos dat z externího klienta do služby Azure Storage i příchozí přenos dat v rámci Azure.|GeoType ApiName|
|Výchozí přenos|Výchozí přenos|B|Celkem|Objem odchozích přenosů dat v bajtech. Toto číslo zahrnuje výchozí přenos dat z externího klienta do služby Azure Storage i výchozí přenos dat v rámci Azure. Kvůli tomu toto číslo nepředstavuje fakturovatelný výchozí přenos dat.|GeoType ApiName|
|SuccessServerLatency|Latence serveru při úspěchu|Milisekundy|Průměr|Průměrná latence služby Azure Storage ke zpracování úspěšného požadavku v milisekundách. Tato hodnota nezahrnuje síťovou latenci určenou v AverageE2ELatency.|GeoType ApiName|
|SuccessE2ELatency|Celková latence při úspěchu|Milisekundy|Průměr|Průměrná latence začátku do konce úspěšných požadavků provedených na službu úložiště nebo zadanou operaci rozhraní API v milisekundách. Tato hodnota zahrnuje čas zpracování ve službě Azure Storage potřebný k přečtení požadavku, odeslání odpovědi a přijetí potvrzení dané odpovědi.|GeoType ApiName|
|Dostupnost|Dostupnost|Procento|Průměr|Procento dostupnosti pro službu úložiště nebo zadanou operaci rozhraní API. Dostupnost se počítá tak, že hodnota TotalBillableRequests vydělí počtem příslušných požadavků, včetně těch, ke které došlo k neočekávané chybě. Všechny neočekávané chyby za následek sníženou dostupnost pro službu úložiště nebo zadanou operaci rozhraní API.|GeoType ApiName|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TableCapacity|Kapacita služby Table|B|Průměr|Velikost úložiště využitá službou Table Service účtu úložiště v bajtech|Žádné dimenze|
|TableCount|Počet tabulek|Počet|Průměr|Počet tabulek ve službě Table Service účtu úložiště|Žádné dimenze|
|TableEntityCount|Počet entit tabulek|Počet|Průměr|Počet entit tabulek ve službě Table Service účtu úložiště|Žádné dimenze|
|Transakce|Transakce|Počet|Celkem|Počet požadavků provedených na službu úložiště nebo zadanou operaci rozhraní API. Toto číslo zahrnuje úspěšné i neúspěšné požadavky a požadavky, které došlo k chybě. Hodnota ResponseType dimenzi používejte pro počet různých typů odpovědi.|Hodnota ResponseType, GeoType, ApiName|
|Příchozí přenos dat|Příchozí přenos dat|B|Celkem|Množství příchozích dat v bajtech. Toto číslo zahrnuje příchozí přenos dat z externího klienta do služby Azure Storage i příchozí přenos dat v rámci Azure.|GeoType ApiName|
|Výchozí přenos|Výchozí přenos|B|Celkem|Objem odchozích přenosů dat v bajtech. Toto číslo zahrnuje výchozí přenos dat z externího klienta do služby Azure Storage i výchozí přenos dat v rámci Azure. Kvůli tomu toto číslo nepředstavuje fakturovatelný výchozí přenos dat.|GeoType ApiName|
|SuccessServerLatency|Latence serveru při úspěchu|Milisekundy|Průměr|Průměrná latence služby Azure Storage ke zpracování úspěšného požadavku v milisekundách. Tato hodnota nezahrnuje síťovou latenci určenou v AverageE2ELatency.|GeoType ApiName|
|SuccessE2ELatency|Celková latence při úspěchu|Milisekundy|Průměr|Průměrná latence začátku do konce úspěšných požadavků provedených na službu úložiště nebo zadanou operaci rozhraní API v milisekundách. Tato hodnota zahrnuje čas zpracování ve službě Azure Storage potřebný k přečtení požadavku, odeslání odpovědi a přijetí potvrzení dané odpovědi.|GeoType ApiName|
|Dostupnost|Dostupnost|Procento|Průměr|Procento dostupnosti pro službu úložiště nebo zadanou operaci rozhraní API. Dostupnost se počítá tak, že hodnota TotalBillableRequests vydělí počtem příslušných požadavků, včetně těch, ke které došlo k neočekávané chybě. Všechny neočekávané chyby za následek sníženou dostupnost pro službu úložiště nebo zadanou operaci rozhraní API.|GeoType ApiName|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|QueueCapacity|Kapacita služby Queue|B|Průměr|Velikost úložiště využitá službou Queue účtu úložiště v bajtech|Žádné dimenze|
|QueueCount|Počet front|Počet|Průměr|Počet front ve službě Queue účtu úložiště|Žádné dimenze|
|QueueMessageCount|Počet zpráv fronty|Počet|Průměr|Přibližný počet zpráv fronty ve službě Queue účtu úložiště|Žádné dimenze|
|Transakce|Transakce|Počet|Celkem|Počet požadavků provedených na službu úložiště nebo zadanou operaci rozhraní API. Toto číslo zahrnuje úspěšné i neúspěšné požadavky a požadavky, které došlo k chybě. Hodnota ResponseType dimenzi používejte pro počet různých typů odpovědi.|Hodnota ResponseType, GeoType, ApiName|
|Příchozí přenos dat|Příchozí přenos dat|B|Celkem|Množství příchozích dat v bajtech. Toto číslo zahrnuje příchozí přenos dat z externího klienta do služby Azure Storage i příchozí přenos dat v rámci Azure.|GeoType ApiName|
|Výchozí přenos|Výchozí přenos|B|Celkem|Objem odchozích přenosů dat v bajtech. Toto číslo zahrnuje výchozí přenos dat z externího klienta do služby Azure Storage i výchozí přenos dat v rámci Azure. Kvůli tomu toto číslo nepředstavuje fakturovatelný výchozí přenos dat.|GeoType ApiName|
|SuccessServerLatency|Latence serveru při úspěchu|Milisekundy|Průměr|Průměrná latence služby Azure Storage ke zpracování úspěšného požadavku v milisekundách. Tato hodnota nezahrnuje síťovou latenci určenou v AverageE2ELatency.|GeoType ApiName|
|SuccessE2ELatency|Celková latence při úspěchu|Milisekundy|Průměr|Průměrná latence začátku do konce úspěšných požadavků provedených na službu úložiště nebo zadanou operaci rozhraní API v milisekundách. Tato hodnota zahrnuje čas zpracování ve službě Azure Storage potřebný k přečtení požadavku, odeslání odpovědi a přijetí potvrzení dané odpovědi.|GeoType ApiName|
|Dostupnost|Dostupnost|Procento|Průměr|Procento dostupnosti pro službu úložiště nebo zadanou operaci rozhraní API. Dostupnost se počítá tak, že hodnota TotalBillableRequests vydělí počtem příslušných požadavků, včetně těch, ke které došlo k neočekávané chybě. Všechny neočekávané chyby za následek sníženou dostupnost pro službu úložiště nebo zadanou operaci rozhraní API.|GeoType ApiName|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|FileCapacity|Kapacita služby File|B|Průměr|Velikost úložiště využitá službou File účtu úložiště v bajtech|Žádné dimenze|
|FileCount|Počet souborů|Počet|Průměr|Počet souborů ve službě File účtu úložiště|Žádné dimenze|
|FileShareCount|Počet sdílených složek služby File|Počet|Průměr|Počet sdílených složek ve službě File účtu úložiště|Žádné dimenze|
|Transakce|Transakce|Počet|Celkem|Počet požadavků provedených na službu úložiště nebo zadanou operaci rozhraní API. Toto číslo zahrnuje úspěšné i neúspěšné požadavky a požadavky, které došlo k chybě. Hodnota ResponseType dimenzi používejte pro počet různých typů odpovědi.|Hodnota ResponseType, GeoType, ApiName|
|Příchozí přenos dat|Příchozí přenos dat|B|Celkem|Množství příchozích dat v bajtech. Toto číslo zahrnuje příchozí přenos dat z externího klienta do služby Azure Storage i příchozí přenos dat v rámci Azure.|GeoType ApiName|
|Výchozí přenos|Výchozí přenos|B|Celkem|Objem odchozích přenosů dat v bajtech. Toto číslo zahrnuje výchozí přenos dat z externího klienta do služby Azure Storage i výchozí přenos dat v rámci Azure. Kvůli tomu toto číslo nepředstavuje fakturovatelný výchozí přenos dat.|GeoType ApiName|
|SuccessServerLatency|Latence serveru při úspěchu|Milisekundy|Průměr|Průměrná latence služby Azure Storage ke zpracování úspěšného požadavku v milisekundách. Tato hodnota nezahrnuje síťovou latenci určenou v AverageE2ELatency.|GeoType ApiName|
|SuccessE2ELatency|Celková latence při úspěchu|Milisekundy|Průměr|Průměrná latence začátku do konce úspěšných požadavků provedených na službu úložiště nebo zadanou operaci rozhraní API v milisekundách. Tato hodnota zahrnuje čas zpracování ve službě Azure Storage potřebný k přečtení požadavku, odeslání odpovědi a přijetí potvrzení dané odpovědi.|GeoType ApiName|
|Dostupnost|Dostupnost|Procento|Průměr|Procento dostupnosti pro službu úložiště nebo zadanou operaci rozhraní API. Dostupnost se počítá tak, že hodnota TotalBillableRequests vydělí počtem příslušných požadavků, včetně těch, ke které došlo k neočekávané chybě. Všechny neočekávané chyby za následek sníženou dostupnost pro službu úložiště nebo zadanou operaci rozhraní API.|GeoType ApiName|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|ResourceUtilization|% využití SU|Procento|Maximum|% využití SU|Žádné dimenze|
|Situací|Vstupní události|Počet|Celkem|Vstupní události|Žádné dimenze|
|InputEventBytes|Bajty vstupních událostí|B|Celkem|Bajty vstupních událostí|Žádné dimenze|
|LateInputEvents|Pozdní vstupní události|Počet|Celkem|Pozdní vstupní události|Žádné dimenze|
|Výstupní události|Výstupní události|Počet|Celkem|Výstupní události|Žádné dimenze|
|ConversionErrors|Chyby převodu dat|Počet|Celkem|Chyby převodu dat|Žádné dimenze|
|Chyby|Chyby za běhu|Počet|Celkem|Chyby za běhu|Žádné dimenze|
|DroppedOrAdjustedEvents|Události mimo pořadí|Počet|Celkem|Události mimo pořadí|Žádné dimenze|
|AMLCalloutRequests|Požadavky na funkce|Počet|Celkem|Požadavky na funkce|Žádné dimenze|
|AMLCalloutFailedRequests|Nezdařené požadavky na funkce|Počet|Celkem|Nezdařené požadavky na funkce|Žádné dimenze|
|AMLCalloutInputEvents|Události funkcí|Počet|Celkem|Události funkcí|Žádné dimenze|
|DeserializationError|Chyby deserializace vstupu|Počet|Celkem|Chyby deserializace vstupu|Žádné dimenze|
|EarlyInputEvents|Události, jejichž čas aplikace je dřívější než čas jejich přijetí.|Počet|Celkem|Události, jejichž čas aplikace je dřívější než čas jejich přijetí.|Žádné dimenze|

## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft.TimeSeriesInsights/environments

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|IngressReceivedMessages|Příchozí přenos dat přijatých zpráv|Počet|Celkem|Počet zpráv, které číst ze všech centra událostí nebo službu IoT hub zdroje událostí|Žádné dimenze|
|IngressReceivedInvalidMessages|Příchozí přenos dat přijatých neplatné zprávy|Počet|Celkem|Počet zpráv neplatné čtení z všechna centra událostí nebo službu IoT hub zdroje událostí|Žádné dimenze|
|IngressReceivedBytes|Příchozí přenos dat přijatých bajtů|B|Celkem|Počet bajtů načtených ze všech zdrojů událostí|Žádné dimenze|
|IngressStoredBytes|Příchozí přenos dat uložené bajtů|B|Celkem|Celková velikost událostí úspěšně zpracovaná a k dispozici pro dotaz|Žádné dimenze|
|IngressStoredEvents|Příchozí přenos dat uložených událostí|Počet|Celkem|Počet událostí plochá úspěšně zpracovaná a k dispozici pro dotaz|Žádné dimenze|
|IngressReceivedMessagesTimeLag|Příchozí přenos dat přijatých zpráv časový interval|Sekundy|Maximum|Rozdíl mezi časem, který je zpráva zařazených do fronty ve zdroji událostí a čas, kdy se zpracovávají v příchozího přenosu dat|Žádné dimenze|
|IngressReceivedMessagesCountLag|Prodleva počet přijatých zpráv příchozího přenosu dat|Počet|Průměr|Rozdíl mezi pořadové číslo poslední zprávy ve frontě událostí zdroje oddílu a pořadovým číslem zprávy jsou zpracovávány v příchozího přenosu dat|Žádné dimenze|

## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft.TimeSeriesInsights/environments/eventsources

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|IngressReceivedMessages|Příchozí přenos dat přijatých zpráv|Počet|Celkem|Počet zpráv, které číst ze zdroje události|Žádné dimenze|
|IngressReceivedInvalidMessages|Příchozí přenos dat přijatých neplatné zprávy|Počet|Celkem|Počet zpráv neplatná číst ze zdroje události|Žádné dimenze|
|IngressReceivedBytes|Příchozí přenos dat přijatých bajtů|B|Celkem|Počet bajtů načtených ze zdroje události|Žádné dimenze|
|IngressStoredBytes|Příchozí přenos dat uložené bajtů|B|Celkem|Celková velikost událostí úspěšně zpracovaná a k dispozici pro dotaz|Žádné dimenze|
|IngressStoredEvents|Příchozí přenos dat uložených událostí|Počet|Celkem|Počet událostí plochá úspěšně zpracovaná a k dispozici pro dotaz|Žádné dimenze|
|IngressReceivedMessagesTimeLag|Příchozí přenos dat přijatých zpráv časový interval|Sekundy|Maximum|Rozdíl mezi časem, který je zpráva zařazených do fronty ve zdroji událostí a čas, kdy se zpracovávají v příchozího přenosu dat|Žádné dimenze|
|IngressReceivedMessagesCountLag|Prodleva počet přijatých zpráv příchozího přenosu dat|Počet|Průměr|Rozdíl mezi pořadové číslo poslední zprávy ve frontě událostí zdroje oddílu a pořadovým číslem zprávy jsou zpracovávány v příchozího přenosu dat|Žádné dimenze|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|CpuPercentage|Procento procesoru|Procento|Průměr|Procento procesoru|Instance|
|MemoryPercentage|Procento paměti|Procento|Průměr|Procento paměti|Instance|
|DiskQueueLength|Délka fronty disku|Počet|Průměr|Délka fronty disku|Instance|
|HttpQueueLength|Délka fronty HTTP|Počet|Průměr|Délka fronty HTTP|Instance|
|BytesReceived|Vstupní data|B|Celkem|Vstupní data|Instance|
|BytesSent|Výstupní data|B|Celkem|Výstupní data|Instance|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (s výjimkou funkce)

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|CpuTime|Čas procesoru|Sekundy|Celkem|Čas procesoru|Instance|
|Požadavky|Požadavky|Počet|Celkem|Požadavky|Instance|
|BytesReceived|Vstupní data|B|Celkem|Vstupní data|Instance|
|BytesSent|Výstupní data|B|Celkem|Výstupní data|Instance|
|Http101|HTTP 101|Počet|Celkem|HTTP 101|Instance|
|Http2xx|HTTP 2xx|Počet|Celkem|HTTP 2xx|Instance|
|Http3xx|HTTP 3xx|Počet|Celkem|HTTP 3xx|Instance|
|Http401|HTTP 401|Počet|Celkem|HTTP 401|Instance|
|Http403|HTTP 403|Počet|Celkem|HTTP 403|Instance|
|Http404|HTTP 404|Počet|Celkem|HTTP 404|Instance|
|Http406|HTTP 406|Počet|Celkem|HTTP 406|Instance|
|Http4xx|HTTP 4xx|Počet|Celkem|HTTP 4xx|Instance|
|Http5xx|Chyby serveru HTTP|Počet|Celkem|Chyby serveru HTTP|Instance|
|MemoryWorkingSet|Pracovní sada paměti|B|Průměr|Pracovní sada paměti|Instance|
|AverageMemoryWorkingSet|Průměrná pracovní sada paměti|B|Průměr|Průměrná pracovní sada paměti|Instance|
|AverageResponseTime|Průměrná doba odezvy|Sekundy|Průměr|Průměrná doba odezvy|Instance|
|AppConnections|Připojení|Počet|Průměr|Připojení|Instance|
|Popisovače|Počet popisovačů|Počet|Průměr|Počet popisovačů|Instance|
|Vlákna|Počet vláken|Počet|Průměr|Počet vláken|Instance|
|IoReadBytesPerSecond|V/V – přečtené bajty za sekundu|BytesPerSecond|Celkem|V/V – přečtené bajty za sekundu|Instance|
|IoWriteBytesPerSecond|V/V – zapsané bajty za sekundu|BytesPerSecond|Celkem|V/V – zapsané bajty za sekundu|Instance|
|IoOtherBytesPerSecond|V/V – ostatní bajty za sekundu|BytesPerSecond|Celkem|V/V – ostatní bajty za sekundu|Instance|
|IoReadOperationsPerSecond|V/V – operace čtení za sekundu|BytesPerSecond|Celkem|V/V – operace čtení za sekundu|Instance|
|IoWriteOperationsPerSecond|V/V – operace zápisu za sekundu|BytesPerSecond|Celkem|V/V – operace zápisu za sekundu|Instance|
|IoOtherOperationsPerSecond|V/V – ostatní operace za sekundu|BytesPerSecond|Celkem|V/V – ostatní operace za sekundu|Instance|
|RequestsInApplicationQueue|Požadavky ve frontě aplikace|Počet|Průměr|Požadavky ve frontě aplikace|Instance|
|CurrentAssemblies|Aktuální sestavení|Počet|Průměr|Aktuální sestavení|Instance|
|TotalAppDomains|Celkový počet domén aplikace|Počet|Průměr|Celkový počet domén aplikace|Instance|
|TotalAppDomainsUnloaded|Celkový počet uvolněných domén aplikace|Počet|Průměr|Celkový počet uvolněných domén aplikace|Instance|
|Gen0Collections|Uvolnění paměti 0. generace|Počet|Celkem|Uvolnění paměti 0. generace|Instance|
|Gen1Collections|Uvolnění paměti 1. generace|Počet|Celkem|Uvolnění paměti 1. generace|Instance|
|Gen2Collections|Uvolnění paměti 2. generace|Počet|Celkem|Uvolnění paměti 2. generace|Instance|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (funkce)

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|BytesReceived|Vstupní data|B|Celkem|Vstupní data|Instance|
|BytesSent|Výstupní data|B|Celkem|Výstupní data|Instance|
|Http5xx|Chyby serveru HTTP|Počet|Celkem|Chyby serveru HTTP|Instance|
|MemoryWorkingSet|Pracovní sada paměti|B|Průměr|Pracovní sada paměti|Instance|
|AverageMemoryWorkingSet|Průměrná pracovní sada paměti|B|Průměr|Průměrná pracovní sada paměti|Instance|
|FunctionExecutionUnits|Jednotky provádění funkcí|Počet|Celkem|Jednotky provádění funkcí|Instance|
|Rozsah|Počet spuštění funkce|Počet|Celkem|Počet spuštění funkce|Instance|
|IoReadBytesPerSecond|V/V – přečtené bajty za sekundu|BytesPerSecond|Celkem|V/V – přečtené bajty za sekundu|Instance|
|IoWriteBytesPerSecond|V/V – zapsané bajty za sekundu|BytesPerSecond|Celkem|V/V – zapsané bajty za sekundu|Instance|
|IoOtherBytesPerSecond|V/V – ostatní bajty za sekundu|BytesPerSecond|Celkem|V/V – ostatní bajty za sekundu|Instance|
|IoReadOperationsPerSecond|V/V – operace čtení za sekundu|BytesPerSecond|Celkem|V/V – operace čtení za sekundu|Instance|
|IoWriteOperationsPerSecond|V/V – operace zápisu za sekundu|BytesPerSecond|Celkem|V/V – operace zápisu za sekundu|Instance|
|IoOtherOperationsPerSecond|V/V – ostatní operace za sekundu|BytesPerSecond|Celkem|V/V – ostatní operace za sekundu|Instance|
|RequestsInApplicationQueue|Požadavky ve frontě aplikace|Počet|Průměr|Požadavky ve frontě aplikace|Instance|
|CurrentAssemblies|Aktuální sestavení|Počet|Průměr|Aktuální sestavení|Instance|
|TotalAppDomains|Celkový počet domén aplikace|Počet|Průměr|Celkový počet domén aplikace|Instance|
|TotalAppDomainsUnloaded|Celkový počet uvolněných domén aplikace|Počet|Průměr|Celkový počet uvolněných domén aplikace|Instance|
|Gen0Collections|Uvolnění paměti 0. generace|Počet|Celkem|Uvolnění paměti 0. generace|Instance|
|Gen1Collections|Uvolnění paměti 1. generace|Počet|Celkem|Uvolnění paměti 1. generace|Instance|
|Gen2Collections|Uvolnění paměti 2. generace|Počet|Celkem|Uvolnění paměti 2. generace|Instance|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|CpuTime|Čas procesoru|Sekundy|Celkem|Čas procesoru|Instance|
|Požadavky|Požadavky|Počet|Celkem|Požadavky|Instance|
|BytesReceived|Vstupní data|B|Celkem|Vstupní data|Instance|
|BytesSent|Výstupní data|B|Celkem|Výstupní data|Instance|
|Http101|HTTP 101|Počet|Celkem|HTTP 101|Instance|
|Http2xx|HTTP 2xx|Počet|Celkem|HTTP 2xx|Instance|
|Http3xx|HTTP 3xx|Počet|Celkem|HTTP 3xx|Instance|
|Http401|HTTP 401|Počet|Celkem|HTTP 401|Instance|
|Http403|HTTP 403|Počet|Celkem|HTTP 403|Instance|
|Http404|HTTP 404|Počet|Celkem|HTTP 404|Instance|
|Http406|HTTP 406|Počet|Celkem|HTTP 406|Instance|
|Http4xx|HTTP 4xx|Počet|Celkem|HTTP 4xx|Instance|
|Http5xx|Chyby serveru HTTP|Počet|Celkem|Chyby serveru HTTP|Instance|
|MemoryWorkingSet|Pracovní sada paměti|B|Průměr|Pracovní sada paměti|Instance|
|AverageMemoryWorkingSet|Průměrná pracovní sada paměti|B|Průměr|Průměrná pracovní sada paměti|Instance|
|AverageResponseTime|Průměrná doba odezvy|Sekundy|Průměr|Průměrná doba odezvy|Instance|
|FunctionExecutionUnits|Jednotky provádění funkcí|Počet|Celkem|Jednotky provádění funkcí|Instance|
|Rozsah|Počet spuštění funkce|Počet|Celkem|Počet spuštění funkce|Instance|
|AppConnections|Připojení|Počet|Průměr|Připojení|Instance|
|Popisovače|Počet popisovačů|Počet|Průměr|Počet popisovačů|Instance|
|Vlákna|Počet vláken|Počet|Průměr|Počet vláken|Instance|
|IoReadBytesPerSecond|V/V – přečtené bajty za sekundu|BytesPerSecond|Celkem|V/V – přečtené bajty za sekundu|Instance|
|IoWriteBytesPerSecond|V/V – zapsané bajty za sekundu|BytesPerSecond|Celkem|V/V – zapsané bajty za sekundu|Instance|
|IoOtherBytesPerSecond|V/V – ostatní bajty za sekundu|BytesPerSecond|Celkem|V/V – ostatní bajty za sekundu|Instance|
|IoReadOperationsPerSecond|V/V – operace čtení za sekundu|BytesPerSecond|Celkem|V/V – operace čtení za sekundu|Instance|
|IoWriteOperationsPerSecond|V/V – operace zápisu za sekundu|BytesPerSecond|Celkem|V/V – operace zápisu za sekundu|Instance|
|IoOtherOperationsPerSecond|V/V – ostatní operace za sekundu|BytesPerSecond|Celkem|V/V – ostatní operace za sekundu|Instance|
|RequestsInApplicationQueue|Požadavky ve frontě aplikace|Počet|Průměr|Požadavky ve frontě aplikace|Instance|
|CurrentAssemblies|Aktuální sestavení|Počet|Průměr|Aktuální sestavení|Instance|
|TotalAppDomains|Celkový počet domén aplikace|Počet|Průměr|Celkový počet domén aplikace|Instance|
|TotalAppDomainsUnloaded|Celkový počet uvolněných domén aplikace|Počet|Průměr|Celkový počet uvolněných domén aplikace|Instance|
|Gen0Collections|Uvolnění paměti 0. generace|Počet|Celkem|Uvolnění paměti 0. generace|Instance|
|Gen1Collections|Uvolnění paměti 1. generace|Počet|Celkem|Uvolnění paměti 1. generace|Instance|
|Gen2Collections|Uvolnění paměti 2. generace|Počet|Celkem|Uvolnění paměti 2. generace|Instance|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Požadavky|Požadavky|Počet|Celkem|Požadavky|Instance|
|BytesReceived|Vstupní data|B|Celkem|Vstupní data|Instance|
|BytesSent|Výstupní data|B|Celkem|Výstupní data|Instance|
|Http101|HTTP 101|Počet|Celkem|HTTP 101|Instance|
|Http2xx|HTTP 2xx|Počet|Celkem|HTTP 2xx|Instance|
|Http3xx|HTTP 3xx|Počet|Celkem|HTTP 3xx|Instance|
|Http401|HTTP 401|Počet|Celkem|HTTP 401|Instance|
|Http403|HTTP 403|Počet|Celkem|HTTP 403|Instance|
|Http404|HTTP 404|Počet|Celkem|HTTP 404|Instance|
|Http406|HTTP 406|Počet|Celkem|HTTP 406|Instance|
|Http4xx|HTTP 4xx|Počet|Celkem|HTTP 4xx|Instance|
|Http5xx|Chyby serveru HTTP|Počet|Celkem|Chyby serveru HTTP|Instance|
|AverageResponseTime|Průměrná doba odezvy|Sekundy|Průměr|Průměrná doba odezvy|Instance|
|CpuPercentage|Procento procesoru|Procento|Průměr|Procento procesoru|Instance|
|MemoryPercentage|Procento paměti|Procento|Průměr|Procento paměti|Instance|
|DiskQueueLength|Délka fronty disku|Počet|Průměr|Délka fronty disku|Instance|
|HttpQueueLength|Délka fronty HTTP|Počet|Průměr|Délka fronty HTTP|Instance|
|ActiveRequests|Aktivní požadavky|Počet|Celkem|Aktivní požadavky|Instance|
|TotalFrontEnds|Celkový počet front-endů|Počet|Průměr|Celkový počet front-endů|Žádné dimenze|
|SmallAppServicePlanInstances|Malé pracovní procesy plánu služby App Service|Počet|Průměr|Malé pracovní procesy plánu služby App Service|Žádné dimenze|
|MediumAppServicePlanInstances|Střední pracovní procesy plánu služby App Service|Počet|Průměr|Střední pracovní procesy plánu služby App Service|Žádné dimenze|
|LargeAppServicePlanInstances|Velké pracovní procesy plánu služby App Service|Počet|Průměr|Velké pracovní procesy plánu služby App Service|Žádné dimenze|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Hodnoty WorkersTotal|Celkový počet pracovních procesů|Počet|Průměr|Celkový počet pracovních procesů|Žádné dimenze|
|WorkersAvailable|Dostupné pracovní procesy|Počet|Průměr|Dostupné pracovní procesy|Žádné dimenze|
|Workersused za|Využité pracovní procesy|Počet|Průměr|Využité pracovní procesy|Žádné dimenze|
|CpuPercentage|Procento procesoru|Procento|Průměr|Procento procesoru|Instance|
|MemoryPercentage|Procento paměti|Procento|Průměr|Procento paměti|Instance|

## <a name="next-steps"></a>Další postup
* [Přečtěte si informace o metriky ve službě Azure Monitor](monitoring-overview-metrics.md)
* [Vytváření upozornění na metriky](insights-receive-alert-notifications.md)
* [Export metrik úložiště, Centrum událostí a Log Analytics](monitoring-overview-of-diagnostic-logs.md)
