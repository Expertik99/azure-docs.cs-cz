---
title: "Azure metriky monitorování - podporované metriky na typ prostředku | Microsoft Docs"
description: "Seznam metriky, které jsou dostupné pro jednotlivé typy prostředků s Azure monitorování."
author: anirudhcavale
manager: ashwink
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 63d4ac65-1688-40d1-85c8-7cd408285b0f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/31/2018
ms.author: ancav
ms.openlocfilehash: a7d28de33090995b0a036d528fb82f9e0d7335bf
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="supported-metrics-with-azure-monitor"></a>Podporované metriky s monitorováním Azure
Monitorování Azure poskytuje několik způsobů, jak pracovat s metriky, včetně grafů, je na portálu, k nim přistupovat pomocí rozhraní REST API nebo je dotazování pomocí prostředí PowerShell nebo rozhraní příkazového řádku. Níže je úplný seznam všech metriky aktuálně k dispozici s Azure monitorování metriky kanálu.

> [!NOTE]
> Další metriky může být k dispozici v portálu nebo pomocí starší verze rozhraní API. Tento seznam obsahuje pouze metriky, které jsou k dispozici konsolidované monitorování Azure metriky zřetězením příkazů. Vyhledat a přístup k metriky s dimenzí použijte prosím [2017-05-01-preview verze rozhraní api.](https://docs.microsoft.com/rest/api/monitor/metricdefinitions)
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|qpu_metric|QPU|Počet|Průměr|QPU. Rozsah 0-100 S1, 0 – 200 S2 a 0 – 400 pro S4|ServerResourceType|
|memory_metric|Memory (Paměť)|B|Průměr|Paměť. V rozsahu 0-25 GB pro S1, 0 – 50 GB pro S2 a 0 – 100 GB pro S4|ServerResourceType|
|TotalConnectionRequests|Žádosti o připojení (celkem)|Počet|Průměr|Požadavky na celkový počet připojení. Jedná se o doručení.|ServerResourceType|
|SuccessfullConnectionsPerSec|Úspěšné připojení za sekundu|CountPerSecond|Průměr|Počet dokončených úspěšné připojení.|ServerResourceType|
|TotalConnectionFailures|Chyby připojení (celkem)|Počet|Průměr|Celkový počet neúspěšných pokusů o připojení.|ServerResourceType|
|CurrentUserSessions|Aktuální relace uživatele|Počet|Průměr|Aktuální počet uživatelských relací navázat.|ServerResourceType|
|QueryPoolBusyThreads|Dotaz z fondu podprocesů zaneprázdněn|Počet|Průměr|Počet vytížených vláken ve fondu vláken dotazu.|ServerResourceType|
|CommandPoolJobQueueLength|Příkaz délku fronty úloh|Počet|Průměr|Počet úloh ve frontě příkaz fondu vláken.|ServerResourceType|
|ProcessingPoolJobQueueLength|Délka fronty úloh zpracování fondu|Počet|Průměr|Počet úloh jiný I/O ve frontě fondu zpracování vláken.|ServerResourceType|
|CurrentConnections|Připojení: Aktuální připojení|Počet|Průměr|Aktuální počet připojení klienta.|ServerResourceType|
|CleanerCurrentPrice|Paměť: Čisticí aktuální cena|Počet|Průměr|Aktuální cena paměti, $a bajtů/čas, normalizovány na 1000.|ServerResourceType|
|CleanerMemoryShrinkable|Paměť: Čisticí paměti vypočítat|B|Průměr|Množství paměti v bajtech, podstoupí čisticí vyprazdňování pozadím.|ServerResourceType|
|CleanerMemoryNonshrinkable|Paměť: Čisticí nonshrinkable paměti|B|Průměr|Množství paměti v bajtech, není v souladu čisticí vyprazdňování pozadím.|ServerResourceType|
|Parametru MemoryUsage|Paměti: Využití paměti|B|Průměr|Využití paměti procesem serveru v rámci výpočet ceny čisticí paměti. Rovná se čítač Process\PrivateBytes plus velikost dat mapované paměti, ignoruje všechny paměti, které bylo namapované nebo přidělené modul xVelocity analýzy v paměti (VertiPaq) překračující modul xVelocity Limit paměti.|ServerResourceType|
|MemoryLimitHard|Paměti: Pevný Limit paměti|B|Průměr|Omezení pevné paměti z konfiguračního souboru.|ServerResourceType|
|MemoryLimitHigh|Paměť: Omezení paměti vysoká|B|Průměr|Limit velkého množství paměti, z konfiguračního souboru.|ServerResourceType|
|MemoryLimitLow|Paměti: Nízká Limit paměti|B|Průměr|Limit nedostatek paměti z konfiguračního souboru.|ServerResourceType|
|MemoryLimitVertiPaq|Paměti: VertiPaq Limit paměti|B|Průměr|Omezení v paměti z konfiguračního souboru.|ServerResourceType|
|Kvóta|Paměť: kvóty|B|Průměr|Aktuální kvótu paměti, v bajtech. Kvótu paměti se také označuje jako rezervace paměti grant nebo paměti.|ServerResourceType|
|QuotaBlocked|Paměti: Blokované kvótu|Počet|Průměr|Aktuální počet požadavků kvóty, které jsou blokovaný, dokud jsou uvolněny kvóty další paměť.|ServerResourceType|
|VertiPaqNonpaged|Paměť: VertiPaq nestránkovaného fondu|B|Průměr|Bajtů paměti uzamčena v pracovní sadě pro použití stroj v paměti.|ServerResourceType|
|VertiPaqPaged|Paměť: VertiPaq stránkovaného fondu|B|Průměr|Bajty stránkovaného paměti používané pro data v paměti.|ServerResourceType|
|RowsReadPerSec|Zpracování: Řádky čtení za sekundu|CountPerSecond|Průměr|Počet řádků přečíst ze všech relačních databází.|ServerResourceType|
|RowsConvertedPerSec|Zpracování: Řádky převést za sekundu|CountPerSecond|Průměr|Počet řádků převést během zpracování.|ServerResourceType|
|RowsWrittenPerSec|Zpracování: Řádků zapsaných za sekundu|CountPerSecond|Průměr|Počet řádků zapsaných během zpracování.|ServerResourceType|
|CommandPoolBusyThreads|Vláken: Příkaz zaneprázdněn z fondu podprocesů|Počet|Průměr|Počet vytížených vláken ve fondu vláken příkaz.|ServerResourceType|
|CommandPoolIdleThreads|Vláken: Příkaz nečinných vláken fondu|Počet|Průměr|Počet nečinných vláken ve fondu vláken příkaz.|ServerResourceType|
|LongParsingBusyThreads|Vláken: Analýza zaneprázdněn vláken dlouho|Počet|Průměr|Počet vytížených vláken ve fondu vláken dlouho analýzy.|ServerResourceType|
|LongParsingIdleThreads|Vláken: Analýza nečinných vláken dlouho|Počet|Průměr|Počet nečinných vláken ve fondu vláken dlouho analýzy.|ServerResourceType|
|LongParsingJobQueueLength|Vláken: Analýza dlouho délka fronty úloh|Počet|Průměr|Počet úloh ve frontě dlouho analýzy fondu vláken.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|Vláken: Fond zaneprázdněn vstupně-výstupní úlohy vláken zpracování|Počet|Průměr|Počet vláken, které jsou spuštěné úlohy vstupně-výstupních operací ve fondu zpracování vláken.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|Vláken: Zaneprázdněný jiný I/O vláken fondu zpracování|Počet|Průměr|Počet vláken, které jsou spuštěné úlohy bez I/O ve fondu zpracování vláken.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|Vláken: Fond délka fronty vstupně-výstupní úlohy zpracování|Počet|Průměr|Počet vstupně-výstupních úloh ve frontě fondu zpracování vláken.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|Vláken: Fond nečinnosti vstupně-výstupní úlohy vláken zpracování|Počet|Průměr|Počet vstupně-výstupních úloh ve fondu zpracování vláken nečinných vláken.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|Vláken: Nečinnosti vláken jiný I/O fondu zpracování|Počet|Průměr|Počet nečinných vláken ve fondu zpracování vláken, který je vyhrazený pro jiný I/O úlohy.|ServerResourceType|
|QueryPoolIdleThreads|Vláken: Dotaz nečinných vláken fondu|Počet|Průměr|Počet vstupně-výstupních úloh ve fondu zpracování vláken nečinných vláken.|ServerResourceType|
|QueryPoolJobQueueLength|Vláken: Dotaz lengt fronty úloh fondu|Počet|Průměr|Počet úloh ve frontě fondu vláken dotazu.|ServerResourceType|
|ShortParsingBusyThreads|Vláken: Analýza zaneprázdněn vláken krátké|Počet|Průměr|Počet vytížených vláken v krátké analýzy fondu vláken.|ServerResourceType|
|ShortParsingIdleThreads|Vláken: Analýza nečinných vláken krátké|Počet|Průměr|Počet nečinných vláken v krátké analýzy fondu vláken.|ServerResourceType|
|ShortParsingJobQueueLength|Vláken: Analýza délka fronty úloh krátké|Počet|Průměr|Počet úloh ve frontě krátké analýzy fondu vláken.|ServerResourceType|
|memory_thrashing_metric|Zahlcení paměti|Procento|Průměr|Průměrná paměti zahlcení.|ServerResourceType|
|mashup_engine_qpu_metric|Modul M QPU|Počet|Průměr|Využití QPU procesy modul hybridní webové aplikace|ServerResourceType|
|mashup_engine_memory_metric|M modul paměti|B|Průměr|Využití paměti procesy modul hybridní webové aplikace|ServerResourceType|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TotalRequests|Celkový počet brány požadavků|Počet|Celkem|Počet požadavků na bránu|Umístění, název hostitele|
|SuccessfulRequests|Požadavky pro úspěšné brány|Počet|Celkem|Počet požadavků úspěšná brány|Umístění, název hostitele|
|UnauthorizedRequests|Požadavky neoprávněným brány|Počet|Celkem|Počet požadavků neoprávněným brány|Umístění, název hostitele|
|FailedRequests|Požadavky na bránu|Počet|Celkem|Počet selhání v žádostech o brány|Umístění, název hostitele|
|OtherRequests|Ostatní požadavky brány|Počet|Celkem|Počet požadavků na jiné brány|Umístění, název hostitele|
|Doba trvání|Celková doba trvání brány požadavků|Milisekundy|Průměr|Celkové doby trvání z brány požadavků v milisekundách|Umístění, název hostitele|
|Kapacita|Kapacita (Preview)|Procento|Maximum|Metrika využití služby ApiManagement|Umístění|

## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TotalJob|Celkový počet úloh|Počet|Celkem|Celkový počet úloh|RunbookName, stav|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|CoreCount|Vyhrazené počet jader|Počet|Celkem|Celkový počet vyhrazených jader na účtu batch|Žádné dimenze|
|TotalNodeCount|Počet vyhrazených uzlů|Počet|Celkem|Celkový počet vyhrazených uzlů na účtu batch|Žádné dimenze|
|LowPriorityCoreCount|Počet jader LowPriority|Počet|Celkem|Celkový počet nízkou prioritu jader na účtu batch|Žádné dimenze|
|TotalLowPriorityNodeCount|Počet uzlů s nízkou prioritou|Počet|Celkem|Celkový počet uzlů nízkou prioritu na účtu batch|Žádné dimenze|
|CreatingNodeCount|Vytváření počet uzlů|Počet|Celkem|Počet uzlů, které vytváří|Žádné dimenze|
|StartingNodeCount|Počáteční počet uzlů|Počet|Celkem|Počet uzlů spouštění|Žádné dimenze|
|WaitingForStartTaskNodeCount|Čeká se na pro počet uzlů spuštění úloh|Počet|Celkem|Počet uzlů čekání na dokončení úlohy spustit|Žádné dimenze|
|StartTaskFailedNodeCount|Spuštění úlohy se nezdařilo počet uzlů|Počet|Celkem|Počet uzlů, kde se nezdařilo spustit úlohu|Žádné dimenze|
|IdleNodeCount|Počet nečinných uzlů|Počet|Celkem|Počet nečinných uzlů|Žádné dimenze|
|OfflineNodeCount|Offline počet uzlů|Počet|Celkem|Počet offline uzlů|Žádné dimenze|
|RebootingNodeCount|Restartování počet uzlů|Počet|Celkem|Počet restartování uzlů|Žádné dimenze|
|ReimagingNodeCount|Obnovování počet uzlů|Počet|Celkem|Počet uzlů obnovování|Žádné dimenze|
|RunningNodeCount|Spuštění počet uzlů|Počet|Celkem|Počet spuštěných uzly|Žádné dimenze|
|LeavingPoolNodeCount|Opouštění počet uzlů fondu|Počet|Celkem|Počet uzlů a fondu|Žádné dimenze|
|UnusableNodeCount|Nepoužitelná počet uzlů|Počet|Celkem|Počet nepoužitelná uzlů|Žádné dimenze|
|PreemptedNodeCount|Zrušené počet uzlů|Počet|Celkem|Počet uzlů zrušené|Žádné dimenze|
|TaskStartEvent|Úloha spuštění události|Počet|Celkem|Celkový počet úloh, které byly zahájeny|Žádné dimenze|
|TaskCompleteEvent|Dokončení událostí úlohy|Počet|Celkem|Celkový počet úloh, které byly dokončeny|Žádné dimenze|
|TaskFailEvent|Událostí selhání úlohy|Počet|Celkem|Celkový počet úloh, které byly dokončeny ve stavu selhání|Žádné dimenze|
|PoolCreateEvent|Události vytváření fondu|Počet|Celkem|Celkový počet fondy, které byly vytvořeny|Žádné dimenze|
|PoolResizeStartEvent|Události spuštění změny velikosti fondu|Počet|Celkem|Celkový počet změní velikost fondu, které byly zahájeny|Žádné dimenze|
|PoolResizeCompleteEvent|Dokončení události změny velikosti fondu|Počet|Celkem|Celkový počet změní velikost fondu, které byly dokončeny|Žádné dimenze|
|PoolDeleteStartEvent|Události spuštění odstranění fondu|Počet|Celkem|Celkový počet odstranění fondu, které byly zahájeny|Žádné dimenze|
|PoolDeleteCompleteEvent|Události dokončení odstranění fondu|Počet|Celkem|Celkový počet odstranění fondu, které byly dokončeny|Žádné dimenze|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|connectedclients|Připojení klienti|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed|Operace celkem|Počet|Celkem||Žádné dimenze|
|mezipaměti|Přístupy do mezipaměti|Počet|Celkem||Žádné dimenze|
|cachemisses|Neúspěšné přístupy do mezipaměti|Počet|Celkem||Žádné dimenze|
|getcommands|Klíče Get|Počet|Celkem||Žádné dimenze|
|setcommands|Sady|Počet|Celkem||Žádné dimenze|
|evictedkeys|Vyloučené klíče|Počet|Celkem||Žádné dimenze|
|totalkeys|Celkový počet klíčů|Počet|Maximum||Žádné dimenze|
|expiredkeys|Prošlé klíče|Počet|Celkem||Žádné dimenze|
|usedmemory|Použitá paměť|B|Maximum||Žádné dimenze|
|usedmemoryRss|Použitá paměť RSS|B|Maximum||Žádné dimenze|
|serverLoad|Zatížení serveru|Procento|Maximum||Žádné dimenze|
|cacheWrite|Zápis do mezipaměti|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead|Čtení z mezipaměti|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime|Procesor|Procento|Maximum||Žádné dimenze|
|connectedclients0|Připojených klientů (horizontálního oddílu 0)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed0|Celkový počet operací (horizontálního oddílu 0)|Počet|Celkem||Žádné dimenze|
|cachehits0|Přístupů k mezipaměti (horizontálního oddílu 0)|Počet|Celkem||Žádné dimenze|
|cachemisses0|Neúspěšné přístupy do mezipaměti (horizontálního oddílu 0)|Počet|Celkem||Žádné dimenze|
|getcommands0|Získá (horizontálního oddílu 0)|Počet|Celkem||Žádné dimenze|
|setcommands0|Nastaví (horizontálního oddílu 0)|Počet|Celkem||Žádné dimenze|
|evictedkeys0|Vyřazené klíče (horizontálního oddílu 0)|Počet|Celkem||Žádné dimenze|
|totalkeys0|Celkový počet klíčů (horizontálního oddílu 0)|Počet|Maximum||Žádné dimenze|
|expiredkeys0|Vypršela platnost klíče (horizontálního oddílu 0)|Počet|Celkem||Žádné dimenze|
|usedmemory0|Použitá paměť (horizontálního oddílu 0)|B|Maximum||Žádné dimenze|
|usedmemoryRss0|Použitá paměť RSS (horizontálního oddílu 0)|B|Maximum||Žádné dimenze|
|serverLoad0|Zatížení serveru (horizontálního oddílu 0)|Procento|Maximum||Žádné dimenze|
|cacheWrite0|Mezipaměť zápisu (horizontálního oddílu 0)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead0|Mezipaměti pro čtení (horizontálního oddílu 0)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime0|Procesor (horizontálního oddílu 0)|Procento|Maximum||Žádné dimenze|
|connectedclients1|Připojených klientů (horizontálních 1)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed1|Celkový počet operací (horizontálních 1)|Počet|Celkem||Žádné dimenze|
|cachehits1|Přístupů k mezipaměti (horizontálních 1)|Počet|Celkem||Žádné dimenze|
|cachemisses1|Neúspěšné přístupy do mezipaměti (horizontálních 1)|Počet|Celkem||Žádné dimenze|
|getcommands1|Získá (horizontálních 1)|Počet|Celkem||Žádné dimenze|
|setcommands1|Nastaví (horizontálních 1)|Počet|Celkem||Žádné dimenze|
|evictedkeys1|Vyřazené klíče (horizontálních 1)|Počet|Celkem||Žádné dimenze|
|totalkeys1|Celkový počet klíčů (horizontálních 1)|Počet|Maximum||Žádné dimenze|
|expiredkeys1|Vypršela platnost klíče (horizontálních 1)|Počet|Celkem||Žádné dimenze|
|usedmemory1|Použitá paměť (horizontálních 1)|B|Maximum||Žádné dimenze|
|usedmemoryRss1|Použitá paměť RSS (horizontálních 1)|B|Maximum||Žádné dimenze|
|serverLoad1|Zatížení serveru (horizontálních 1)|Procento|Maximum||Žádné dimenze|
|cacheWrite1|Mezipaměť zápisu (horizontálních 1)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead1|Mezipaměti pro čtení (horizontálních 1)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime1|Procesor (horizontálních 1)|Procento|Maximum||Žádné dimenze|
|connectedclients2|Připojených klientů (horizontálních 2)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed2|Celkový počet operací (horizontálních 2)|Počet|Celkem||Žádné dimenze|
|cachehits2|Přístupů k mezipaměti (horizontálních 2)|Počet|Celkem||Žádné dimenze|
|cachemisses2|Neúspěšné přístupy do mezipaměti (horizontálních 2)|Počet|Celkem||Žádné dimenze|
|getcommands2|Získá (horizontálních 2)|Počet|Celkem||Žádné dimenze|
|setcommands2|Nastaví (horizontálních 2)|Počet|Celkem||Žádné dimenze|
|evictedkeys2|Vyřazené klíče (horizontálních 2)|Počet|Celkem||Žádné dimenze|
|totalkeys2|Celkový počet klíčů (horizontálních 2)|Počet|Maximum||Žádné dimenze|
|expiredkeys2|Vypršela platnost klíče (horizontálních 2)|Počet|Celkem||Žádné dimenze|
|usedmemory2|Použitá paměť (horizontálních 2)|B|Maximum||Žádné dimenze|
|usedmemoryRss2|Použitá paměť RSS (horizontálních 2)|B|Maximum||Žádné dimenze|
|serverLoad2|Zatížení serveru (horizontálních 2)|Procento|Maximum||Žádné dimenze|
|cacheWrite2|Mezipaměť zápisu (horizontálních 2)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead2|Mezipaměti pro čtení (horizontálních 2)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime2|Procesor (horizontálních 2)|Procento|Maximum||Žádné dimenze|
|connectedclients3|Připojených klientů (horizontálních 3)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed3|Celkový počet operací (horizontálních 3)|Počet|Celkem||Žádné dimenze|
|cachehits3|Přístupů k mezipaměti (horizontálních 3)|Počet|Celkem||Žádné dimenze|
|cachemisses3|Neúspěšné přístupy do mezipaměti (horizontálních 3)|Počet|Celkem||Žádné dimenze|
|getcommands3|Získá (horizontálních 3)|Počet|Celkem||Žádné dimenze|
|setcommands3|Nastaví (horizontálních 3)|Počet|Celkem||Žádné dimenze|
|evictedkeys3|Vyřazené klíče (horizontálních 3)|Počet|Celkem||Žádné dimenze|
|totalkeys3|Celkový počet klíčů (horizontálních 3)|Počet|Maximum||Žádné dimenze|
|expiredkeys3|Vypršela platnost klíče (horizontálních 3)|Počet|Celkem||Žádné dimenze|
|usedmemory3|Použitá paměť (horizontálních 3)|B|Maximum||Žádné dimenze|
|usedmemoryRss3|Použitá paměť RSS (horizontálních 3)|B|Maximum||Žádné dimenze|
|serverLoad3|Zatížení serveru (horizontálních 3)|Procento|Maximum||Žádné dimenze|
|cacheWrite3|Mezipaměť zápisu (horizontálních 3)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead3|Mezipaměti pro čtení (horizontálních 3)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime3|Procesor (horizontálních 3)|Procento|Maximum||Žádné dimenze|
|connectedclients4|Připojených klientů (horizontálních 4)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed4|Celkový počet operací (horizontálních 4)|Počet|Celkem||Žádné dimenze|
|cachehits4|Přístupů k mezipaměti (horizontálních 4)|Počet|Celkem||Žádné dimenze|
|cachemisses4|Neúspěšné přístupy do mezipaměti (horizontálních 4)|Počet|Celkem||Žádné dimenze|
|getcommands4|Získá (horizontálních 4)|Počet|Celkem||Žádné dimenze|
|setcommands4|Nastaví (horizontálních 4)|Počet|Celkem||Žádné dimenze|
|evictedkeys4|Vyřazené klíče (horizontálních 4)|Počet|Celkem||Žádné dimenze|
|totalkeys4|Celkový počet klíčů (horizontálních 4)|Počet|Maximum||Žádné dimenze|
|expiredkeys4|Vypršela platnost klíče (horizontálních 4)|Počet|Celkem||Žádné dimenze|
|usedmemory4|Použitá paměť (horizontálních 4)|B|Maximum||Žádné dimenze|
|usedmemoryRss4|Použitá paměť RSS (horizontálních 4)|B|Maximum||Žádné dimenze|
|serverLoad4|Zatížení serveru (horizontálních 4)|Procento|Maximum||Žádné dimenze|
|cacheWrite4|Mezipaměť zápisu (horizontálních 4)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead4|Mezipaměti pro čtení (horizontálních 4)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime4|Procesor (horizontálních 4)|Procento|Maximum||Žádné dimenze|
|connectedclients5|Připojených klientů (horizontálních 5)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed5|Celkový počet operací (horizontálních 5)|Počet|Celkem||Žádné dimenze|
|cachehits5|Přístupů k mezipaměti (horizontálních 5)|Počet|Celkem||Žádné dimenze|
|cachemisses5|Neúspěšné přístupy do mezipaměti (horizontálních 5)|Počet|Celkem||Žádné dimenze|
|getcommands5|Získá (horizontálních 5)|Počet|Celkem||Žádné dimenze|
|setcommands5|Nastaví (horizontálních 5)|Počet|Celkem||Žádné dimenze|
|evictedkeys5|Vyřazené klíče (horizontálních 5)|Počet|Celkem||Žádné dimenze|
|totalkeys5|Celkový počet klíčů (horizontálních 5)|Počet|Maximum||Žádné dimenze|
|expiredkeys5|Vypršela platnost klíče (horizontálních 5)|Počet|Celkem||Žádné dimenze|
|usedmemory5|Použitá paměť (horizontálních 5)|B|Maximum||Žádné dimenze|
|usedmemoryRss5|Použitá paměť RSS (horizontálních 5)|B|Maximum||Žádné dimenze|
|serverLoad5|Zatížení serveru (horizontálních 5)|Procento|Maximum||Žádné dimenze|
|cacheWrite5|Mezipaměť zápisu (horizontálních 5)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead5|Mezipaměti pro čtení (horizontálních 5)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime5|Procesor (horizontálních 5)|Procento|Maximum||Žádné dimenze|
|connectedclients6|Připojených klientů (horizontálních 6)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed6|Celkový počet operací (horizontálních 6)|Počet|Celkem||Žádné dimenze|
|cachehits6|Přístupů k mezipaměti (horizontálních 6)|Počet|Celkem||Žádné dimenze|
|cachemisses6|Neúspěšné přístupy do mezipaměti (horizontálních 6)|Počet|Celkem||Žádné dimenze|
|getcommands6|Získá (horizontálních 6)|Počet|Celkem||Žádné dimenze|
|setcommands6|Nastaví (horizontálních 6)|Počet|Celkem||Žádné dimenze|
|evictedkeys6|Vyřazené klíče (horizontálních 6)|Počet|Celkem||Žádné dimenze|
|totalkeys6|Celkový počet klíčů (horizontálních 6)|Počet|Maximum||Žádné dimenze|
|expiredkeys6|Vypršela platnost klíče (horizontálních 6)|Počet|Celkem||Žádné dimenze|
|usedmemory6|Použitá paměť (horizontálních 6)|B|Maximum||Žádné dimenze|
|usedmemoryRss6|Použitá paměť RSS (horizontálních 6)|B|Maximum||Žádné dimenze|
|serverLoad6|Zatížení serveru (horizontálních 6)|Procento|Maximum||Žádné dimenze|
|cacheWrite6|Mezipaměť zápisu (horizontálních 6)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead6|Mezipaměti pro čtení (horizontálních 6)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime6|Procesor (horizontálních 6)|Procento|Maximum||Žádné dimenze|
|connectedclients7|Připojených klientů (horizontálních 7)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed7|Celkový počet operací (horizontálních 7)|Počet|Celkem||Žádné dimenze|
|cachehits7|Přístupů k mezipaměti (horizontálních 7)|Počet|Celkem||Žádné dimenze|
|cachemisses7|Neúspěšné přístupy do mezipaměti (horizontálních 7)|Počet|Celkem||Žádné dimenze|
|getcommands7|Získá (horizontálních 7)|Počet|Celkem||Žádné dimenze|
|setcommands7|Nastaví (horizontálních 7)|Počet|Celkem||Žádné dimenze|
|evictedkeys7|Vyřazené klíče (horizontálních 7)|Počet|Celkem||Žádné dimenze|
|totalkeys7|Celkový počet klíčů (horizontálních 7)|Počet|Maximum||Žádné dimenze|
|expiredkeys7|Vypršela platnost klíče (horizontálních 7)|Počet|Celkem||Žádné dimenze|
|usedmemory7|Použitá paměť (horizontálních 7)|B|Maximum||Žádné dimenze|
|usedmemoryRss7|Použitá paměť RSS (horizontálních 7)|B|Maximum||Žádné dimenze|
|serverLoad7|Zatížení serveru (horizontálních 7)|Procento|Maximum||Žádné dimenze|
|cacheWrite7|Mezipaměť zápisu (horizontálních 7)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead7|Mezipaměti pro čtení (horizontálních 7)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime7|Procesor (horizontálních 7)|Procento|Maximum||Žádné dimenze|
|connectedclients8|Připojených klientů (horizontálních 8)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed8|Celkový počet operací (horizontálních 8)|Počet|Celkem||Žádné dimenze|
|cachehits8|Přístupů k mezipaměti (horizontálních 8)|Počet|Celkem||Žádné dimenze|
|cachemisses8|Neúspěšné přístupy do mezipaměti (horizontálních 8)|Počet|Celkem||Žádné dimenze|
|getcommands8|Získá (horizontálních 8)|Počet|Celkem||Žádné dimenze|
|setcommands8|Nastaví (horizontálních 8)|Počet|Celkem||Žádné dimenze|
|evictedkeys8|Vyřazené klíče (horizontálních 8)|Počet|Celkem||Žádné dimenze|
|totalkeys8|Celkový počet klíčů (horizontálních 8)|Počet|Maximum||Žádné dimenze|
|expiredkeys8|Vypršela platnost klíče (horizontálních 8)|Počet|Celkem||Žádné dimenze|
|usedmemory8|Použitá paměť (horizontálních 8)|B|Maximum||Žádné dimenze|
|usedmemoryRss8|Použitá paměť RSS (horizontálních 8)|B|Maximum||Žádné dimenze|
|serverLoad8|Zatížení serveru (horizontálních 8)|Procento|Maximum||Žádné dimenze|
|cacheWrite8|Mezipaměť zápisu (horizontálních 8)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead8|Mezipaměti pro čtení (horizontálních 8)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime8|Procesor (horizontálních 8)|Procento|Maximum||Žádné dimenze|
|connectedclients9|Připojených klientů (horizontálních 9)|Počet|Maximum||Žádné dimenze|
|totalcommandsprocessed9|Celkový počet operací (horizontálních 9)|Počet|Celkem||Žádné dimenze|
|cachehits9|Přístupů k mezipaměti (horizontálních 9)|Počet|Celkem||Žádné dimenze|
|cachemisses9|Neúspěšné přístupy do mezipaměti (horizontálních 9)|Počet|Celkem||Žádné dimenze|
|getcommands9|Získá (horizontálních 9)|Počet|Celkem||Žádné dimenze|
|setcommands9|Nastaví (horizontálních 9)|Počet|Celkem||Žádné dimenze|
|evictedkeys9|Vyřazené klíče (horizontálních 9)|Počet|Celkem||Žádné dimenze|
|totalkeys9|Celkový počet klíčů (horizontálních 9)|Počet|Maximum||Žádné dimenze|
|expiredkeys9|Vypršela platnost klíče (horizontálních 9)|Počet|Celkem||Žádné dimenze|
|usedmemory9|Použitá paměť (horizontálních 9)|B|Maximum||Žádné dimenze|
|usedmemoryRss9|Použitá paměť RSS (horizontálních 9)|B|Maximum||Žádné dimenze|
|serverLoad9|Zatížení serveru (horizontálních 9)|Procento|Maximum||Žádné dimenze|
|cacheWrite9|Mezipaměť zápisu (horizontálních 9)|BytesPerSecond|Maximum||Žádné dimenze|
|cacheRead9|Mezipaměti pro čtení (horizontálních 9)|BytesPerSecond|Maximum||Žádné dimenze|
|percentProcessorTime9|Procesor (horizontálních 9)|Procento|Maximum||Žádné dimenze|

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
|Latence|Latence|Počet milisekund|Průměr|Latence v milisekundách|Žádné dimenze|
|CharactersTranslated|Přeložené znaky|Počet|Celkem|Celkový počet znaků v příchozí textové žádosti|Žádné dimenze|
|SpeechSessionDuration|Doba trvání řečové relace|Sekundy|Celkem|Celková doba trvání řečové relace v sekundách|Žádné dimenze|
|TotalTransactions|Celkový počet transakcí|Počet|Celkem|Celkový počet transakcí|Žádné dimenze|

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

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|DCIApiCalls|Přehled rozhraní API volání zákazníka|Počet|Celkem||Žádné dimenze|
|DCIMappingImportOperationSuccessfulLines|Mapování importu operace úspěšné řádky|Počet|Celkem||Žádné dimenze|
|DCIMappingImportOperationFailedLines|Mapování importu se nezdařila řádky|Počet|Celkem||Žádné dimenze|
|DCIMappingImportOperationTotalLines|Mapování importu operaci celkový počet řádků|Počet|Celkem||Žádné dimenze|
|DCIMappingImportOperationRuntimeInSeconds|Mapování importu operaci za běhu v sekundách|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundProfileExportSucceeded|Odchozí profilu exportu byla úspěšná|Počet|Celkem||Žádné dimenze|
|DCIOutboundProfileExportFailed|Odchozí profilu exportu se nezdařila|Počet|Celkem||Žádné dimenze|
|DCIOutboundProfileExportDuration|Doba trvání odchozí profilu exportu|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundKpiExportSucceeded|Odchozí klíčového ukazatele výkonu Export byl úspěšný|Počet|Celkem||Žádné dimenze|
|DCIOutboundKpiExportFailed|Odchozí klíčového ukazatele výkonu Export se nezdařil|Počet|Celkem||Žádné dimenze|
|DCIOutboundKpiExportDuration|Doba trvání odchozí Export klíčového ukazatele výkonu|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundKpiExportStarted|Export odchozí klíčový ukazatel výkonu spustit|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundKpiRecordCount|Počet záznamů odchozí klíčového ukazatele výkonu|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundProfileExportCount|Počet Export odchozí profilů|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundInitialProfileExportFailed|Odchozí počáteční profilu exportu se nezdařila|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundInitialProfileExportSucceeded|Odchozí počáteční profilu exportu byla úspěšná|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundInitialKpiExportFailed|Odchozí počáteční klíčového ukazatele výkonu Export se nezdařil|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundInitialKpiExportSucceeded|Odchozí počáteční klíčového ukazatele výkonu Export byl úspěšný|Sekundy|Celkem||Žádné dimenze|
|DCIOutboundInitialProfileExportDurationInSeconds|Odchozí počáteční profilu exportu doby v sekundách|Sekundy|Celkem||Žádné dimenze|
|AdlaJobForStandardKpiFailed|Úloha Adla pro standardní klíčový ukazatel výkonu se nezdařilo s|Sekundy|Celkem||Žádné dimenze|
|AdlaJobForStandardKpiTimeOut|Úloha Adla pro standardní klíčový ukazatel výkonu časový limit v sekundách|Sekundy|Celkem||Žádné dimenze|
|AdlaJobForStandardKpiCompleted|Úloha Adla pro standardní klíčový ukazatel výkonu dokončit v sekundách|Sekundy|Celkem||Žádné dimenze|
|ImportASAValuesFailed|Hodnoty ASA importu se nezdařila, počet|Počet|Celkem||Žádné dimenze|
|ImportASAValuesSucceeded|Import ASA hodnoty počtu bylo úspěšně dokončeno.|Počet|Celkem||Žádné dimenze|
|DCIProfilesCount|Počet instancí profilu|Počet|Poslední||Žádné dimenze|
|DCIInteractionsPerMonthCount|Interakce na počet měsíc|Počet|Poslední||Žádné dimenze|
|DCIKpisCount|Počet klíčového ukazatele výkonu|Počet|Poslední||Žádné dimenze|
|DCISegmentsCount|Počet segmentů|Počet|Poslední||Žádné dimenze|
|DCIPredictiveMatchPoliciesCount|Prediktivní počet shod|Počet|Poslední||Žádné dimenze|
|DCIPredictionsCount|Počet předpovědi|Počet|Poslední||Žádné dimenze|

## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|SuccessfulRuns|Úspěšná spuštění|Počet|Celkem|Počet úspěšně spustí.|Žádné dimenze|
|FailedRuns|Neúspěšná spuštění|Počet|Celkem|Počet selhání spuštění.|Žádné dimenze|

## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|PipelineFailedRuns|Metriky spustí kanálu se nezdařilo|Počet|Celkem||Žádné dimenze|
|PipelineSucceededRuns|Úspěšné metriky spuštění kanálu|Počet|Celkem||Žádné dimenze|
|ActivityFailedRuns|Spustí metriky aktivity se nezdařilo|Počet|Celkem||Žádné dimenze|
|ActivitySucceededRuns|Úspěšné metriky spuštění aktivity|Počet|Celkem||Žádné dimenze|
|TriggerFailedRuns|Aktivační událost metriky spuštění se nezdařilo|Počet|Celkem||Žádné dimenze|
|TriggerSucceededRuns|Aktivační událost metriky spustí bylo úspěšné|Počet|Celkem||Žádné dimenze|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|JobEndedSuccess|Úspěšné úlohy|Počet|Celkem|Počet neúspěšných úloh.|Žádné dimenze|
|JobEndedFailure|Neúspěšné úlohy|Počet|Celkem|Počet nezdařených úloh.|Žádné dimenze|
|JobEndedCancelled|Zrušené úlohy|Počet|Celkem|Počet zrušených úloh.|Žádné dimenze|
|JobAUEndedSuccess|Úspěšné AU čas|Sekundy|Celkem|Celkový čas AU pro úspěšné úlohy.|Žádné dimenze|
|JobAUEndedFailure|Neúspěšné AU čas|Sekundy|Celkem|Celkový čas AU pro neúspěšné úlohy.|Žádné dimenze|
|JobAUEndedCancelled|Zrušené AU čas|Sekundy|Celkem|Celkový čas AU pro zrušené úlohy.|Žádné dimenze|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TotalStorage|Celkový počet úložiště|B|Maximum|Celkové množství dat uložených v účtu.|Žádné dimenze|
|DataWritten|Zapsaná data|B|Celkem|Celkové množství dat, zapisuje na účet.|Žádné dimenze|
|Přečtená data|Přečtená data|B|Celkem|Celkové množství dat číst z účtu.|Žádné dimenze|
|WriteRequests|Požadavků na zápis|Počet|Celkem|Počet dat požadavků na zápis k účtu.|Žádné dimenze|
|ReadRequests|Požadavky pro čtení|Počet|Celkem|Počet data načíst požadavky na účet.|Žádné dimenze|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|cpu_percent|Procento využití procesoru|Procento|Průměr|Procento využití procesoru|Žádné dimenze|
|compute_limit|Výpočetní jednotka limit|Počet|Průměr|Výpočetní jednotka limit|Žádné dimenze|
|compute_consumption_percent|Výpočetní jednotka procento|Procento|Průměr|Výpočetní jednotka procento|Žádné dimenze|
|memory_percent|Paměť v procentech|Procento|Průměr|Paměť v procentech|Žádné dimenze|
|io_consumption_percent|Vstupně-výstupní operace v procentech|Procento|Průměr|Vstupně-výstupní operace v procentech|Žádné dimenze|
|storage_percent|Procento úložiště|Procento|Průměr|Procento úložiště|Žádné dimenze|
|storage_used|Využité úložiště|B|Průměr|Využité úložiště|Žádné dimenze|
|storage_limit|Limit úložiště|B|Průměr|Limit úložiště|Žádné dimenze|
|active_connections|Celkový počet aktivních připojení|Počet|Průměr|Celkový počet aktivních připojení|Žádné dimenze|
|connections_failed|Celkový počet selhání připojení|Počet|Průměr|Celkový počet selhání připojení|Žádné dimenze|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|cpu_percent|Procento využití procesoru|Procento|Průměr|Procento využití procesoru|Žádné dimenze|
|compute_limit|Výpočetní jednotka limit|Počet|Průměr|Výpočetní jednotka limit|Žádné dimenze|
|compute_consumption_percent|Výpočetní jednotka procento|Procento|Průměr|Výpočetní jednotka procento|Žádné dimenze|
|memory_percent|Paměť v procentech|Procento|Průměr|Paměť v procentech|Žádné dimenze|
|io_consumption_percent|Vstupně-výstupní operace v procentech|Procento|Průměr|Vstupně-výstupní operace v procentech|Žádné dimenze|
|storage_percent|Procento úložiště|Procento|Průměr|Procento úložiště|Žádné dimenze|
|storage_used|Využité úložiště|B|Průměr|Využité úložiště|Žádné dimenze|
|storage_limit|Limit úložiště|B|Průměr|Limit úložiště|Žádné dimenze|
|active_connections|Celkový počet aktivních připojení|Počet|Průměr|Celkový počet aktivních připojení|Žádné dimenze|
|connections_failed|Celkový počet selhání připojení|Počet|Průměr|Celkový počet selhání připojení|Žádné dimenze|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Pokusy o odeslání zprávy telemetrie|Počet|Celkem|Počet zařízení cloud telemetrické zprávy se pokusil odeslat do služby IoT hub|Žádné dimenze|
|d2c.telemetry.ingress.Success|Telemetrické zprávy odeslané|Počet|Celkem|Počet zařízení cloud telemetrické zprávy úspěšně odeslán do služby IoT hub|Žádné dimenze|
|c2d.Commands.egress.COMPLETE.Success|Příkazy byla dokončena|Počet|Celkem|Počet příkazů typu cloud zařízení úspěšně dokončila, a zařízení|Žádné dimenze|
|c2d.Commands.egress.Abandon.Success|Příkazy opuštění|Počet|Celkem|Počet příkazů typu cloud zařízení opuštěny v rámci zařízení|Žádné dimenze|
|c2d.Commands.egress.Reject.Success|Příkazy odmítnut|Počet|Celkem|Počet zařízení byl odmítnut příkazů typu cloud zařízení|Žádné dimenze|
|devices.totalDevices|Celkový počet zařízení|Počet|Celkem|Počet zařízení zaregistrovat do služby IoT hub|Žádné dimenze|
|devices.connectedDevices.allProtocol|Připojená zařízení|Počet|Celkem|Počet zařízení připojených ke službě IoT hub|Žádné dimenze|
|d2c.telemetry.egress.Success|Telemetrické zprávy doručit|Počet|Celkem|Počet zpráv byla úspěšně zapsána do koncových bodů (celkem)|Žádné dimenze|
|d2c.telemetry.egress.dropped|Vyřazené zprávy|Počet|Celkem|Počet zpráv vyřadit, protože koncový bod doručení byla neaktivní|Žádné dimenze|
|d2c.telemetry.egress.orphaned|Osamocené zprávy|Počet|Celkem|Počet zpráv, na které se neshodují žádné cesty, včetně záložní trasy|Žádné dimenze|
|d2c.telemetry.egress.invalid|Neplatné zprávy|Počet|Celkem|Počet zpráv, které nejsou doručeny z důvodu nekompatibility s nástrojem koncový bod|Žádné dimenze|
|d2c.telemetry.egress.Fallback|Zprávy odpovídající záložního stavu|Počet|Celkem|Počet zpráv zapsaných do záložního koncového bodu|Žádné dimenze|
|d2c.endpoints.egress.eventHubs|Zpráv doručených do koncové body centra událostí|Počet|Celkem|Počet zpráv byla úspěšně zapsána do koncové body centra událostí|Žádné dimenze|
|d2c.endpoints.latency.eventHubs|Latence zprávy pro koncové body centra událostí|Milisekundy|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příchozí zprávy do koncového bodu centra událostí, v milisekundách|Žádné dimenze|
|d2c.endpoints.egress.serviceBusQueues|Doručování zpráv do fronty Service Bus koncové body|Počet|Celkem|Počet zpráv byla úspěšně zapsána do koncových bodů frontou Service Bus|Žádné dimenze|
|d2c.endpoints.latency.serviceBusQueues|Latence zprávy pro koncové body frontou Service Bus|Milisekundy|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příjem příchozích dat zpráv do fronty Service Bus koncový bod, v milisekundách|Žádné dimenze|
|d2c.endpoints.egress.serviceBusTopics|Doručování zpráv do tématu Service Bus koncové body|Počet|Celkem|Počet zpráv byly úspěšně zapsána do tématu Service Bus koncové body|Žádné dimenze|
|d2c.endpoints.latency.serviceBusTopics|Latence zprávy pro koncové body témata sběrnice|Milisekundy|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příchozí zprávy do tématu Service Bus koncový bod, v milisekundách|Žádné dimenze|
|d2c.endpoints.egress.builtIn.events|Zpráv doručených do vestavěným koncovým bodem (zprávy nebo události)|Počet|Celkem|Počet zpráv byla úspěšně zapsána do vestavěným koncovým bodem (zprávy nebo události)|Žádné dimenze|
|d2c.endpoints.latency.builtIn.events|Zpráva latence pro předdefinovaný koncový bod (zprávy nebo události)|Milisekundy|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příjem příchozích dat zpráva do vestavěným koncovým bodem (zprávy událostí), v milisekundách |Žádné dimenze|
|d2c.endpoints.egress.Storage|Zpráv doručených do koncové body úložiště|Počet|Celkem|Počet zpráv byla úspěšně zapsána do koncové body úložiště|Žádné dimenze|
|d2c.endpoints.latency.Storage|Latence zprávy pro koncové body úložiště|Milisekundy|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příjem příchozích dat zpráva do koncový bod úložiště, v milisekundách|Žádné dimenze|
|d2c.endpoints.egress.Storage.bytes|Data zapsaná do úložiště|B|Celkem|Množství dat v bajtech, zapisovat do úložiště koncové body|Žádné dimenze|
|d2c.endpoints.egress.Storage.BLOBS|Zapisovat do úložiště objektů BLOB|Počet|Celkem|Počet zapsána do koncové body úložiště objektů BLOB|Žádné dimenze|
|d2c.Twin.Read.Success|Úspěšné twin čte ze zařízení|Počet|Celkem|Počet čtení všechny úspěšné twin spouštěná zařízení.|Žádné dimenze|
|d2c.Twin.Read.failure|Čtení twin ze zařízení se nezdařila|Počet|Celkem|Počet všech nepodařilo spouštěná zařízení twin čtení.|Žádné dimenze|
|d2c.Twin.Read.Size|Velikost odpovědi twin čtení ze zařízení|B|Průměr|Průměr, min a max všechny úspěšné, spouštěná zařízení twin čtení.|Žádné dimenze|
|d2c.Twin.Update.Success|Aktualizace úspěšná twin ze zařízení|Počet|Celkem|Počet aktualizací všechny úspěšné twin spouštěná zařízení.|Žádné dimenze|
|d2c.Twin.Update.failure|Aktualizace twin ze zařízení se nezdařila|Počet|Celkem|Počet všech se nezdařila aktualizace twin spouštěná zařízení.|Žádné dimenze|
|d2c.Twin.Update.Size|Velikost twin aktualizace ze zařízení|B|Průměr|Průměr, minimální a maximální velikost všech úspěšné, spouštěná zařízení twin aktualizace.|Žádné dimenze|
|c2d.Methods.Success|Volání úspěšné přímá metoda|Počet|Celkem|Počet všech úspěšných volání přímá metoda.|Žádné dimenze|
|c2d.Methods.failure|Přímá metoda volání se nezdařilo|Počet|Celkem|Počet všech se nezdařila, volání metod direct.|Žádné dimenze|
|c2d.Methods.requestSize|Žádost o velikosti volání přímá metoda|B|Průměr|Průměr, min a max všechny úspěšné požadavky přímá metoda.|Žádné dimenze|
|c2d.Methods.responseSize|Velikost odpovědi volání přímá metoda|B|Průměr|Průměr, min a max všechny úspěšné odpovědi přímá metoda.|Žádné dimenze|
|c2d.Twin.Read.Success|Úspěšné twin čte z back-end|Počet|Celkem|Počet všech úspěšně spustil back-end twin čtení.|Žádné dimenze|
|c2d.Twin.Read.failure|Neúspěšné twin čte z back-end|Počet|Celkem|Počet všech se nezdařilo čtení twin iniciované back-end.|Žádné dimenze|
|c2d.Twin.Read.Size|Velikost odpovědi twin čtení z back-end|B|Průměr|Průměr, min a max všechny úspěšné, iniciované back-end twin čtení.|Žádné dimenze|
|c2d.Twin.Update.Success|Aktualizace úspěšná twin z back-end|Počet|Celkem|Počet aktualizací všechny úspěšné twin iniciované back-end.|Žádné dimenze|
|c2d.Twin.Update.failure|Aktualizace se nezdařila twin z back-end|Počet|Celkem|Počet všech se nezdařila aktualizace twin iniciované back-end.|Žádné dimenze|
|c2d.Twin.Update.Size|Velikost twin aktualizace z back-end|B|Průměr|Průměr, minimální a maximální velikost všech úspěšné, iniciované back-end twin aktualizace.|Žádné dimenze|
|twinQueries.success|Úspěšné twin dotazy|Počet|Celkem|Počet všech úspěšné twin dotazů.|Žádné dimenze|
|twinQueries.failure|Neúspěšné twin dotazy|Počet|Celkem|Počet všechny neúspěšné twin dotazy.|Žádné dimenze|
|twinQueries.resultSize|Objem výsledků dotazů Twin|B|Průměr|Průměr, minimální a maximální velikosti výsledek všechny úspěšné twin dotazy.|Žádné dimenze|
|jobs.createTwinUpdateJob.success|Úspěšné vytvoření úloh aktualizace twin|Počet|Celkem|Počet všech úspěšném vytvoření twin aktualizace úloh.|Žádné dimenze|
|jobs.createTwinUpdateJob.failure|Neúspěšné vytvoření úloh aktualizace twin|Počet|Celkem|Počet všech se nezdařilo vytvoření twin aktualizace úlohy.|Žádné dimenze|
|jobs.createDirectMethodJob.success|Úspěšné vytvoření úloh volání – metoda|Počet|Celkem|Počet všech úspěšném vytvoření přímá metoda volání úloh.|Žádné dimenze|
|jobs.createDirectMethodJob.failure|Neúspěšné vytvoření úloh volání – metoda|Počet|Celkem|Počet všech se nezdařilo vytvoření přímá metoda vyvolání úlohy.|Žádné dimenze|
|jobs.listJobs.success|Úspěšné volání do seznamu úloh|Počet|Celkem|Počet všech úspěšných volání do seznamu úloh.|Žádné dimenze|
|jobs.listJobs.failure|Volání do seznamu úloh se nezdařilo|Počet|Celkem|Počet všech selhání volání do seznamu úloh.|Žádné dimenze|
|jobs.cancelJob.success|Zrušení úloh úspěšné|Počet|Celkem|Počet všech úspěšných volání zrušení úlohy.|Žádné dimenze|
|jobs.cancelJob.failure|Informace o neúspěšné úloze zrušení|Počet|Celkem|Počet všech volání se nezdařilo zrušit úlohu.|Žádné dimenze|
|jobs.queryJobs.success|Až se úloha úspěšně dotazy|Počet|Celkem|Počet všech úspěšných volání do úlohy dotaz.|Žádné dimenze|
|jobs.queryJobs.failure|Úloha se dotazuje se nezdařilo|Počet|Celkem|Počet všech selhání volání do úlohy dotaz.|Žádné dimenze|
|jobs.completed|Dokončené úlohy|Počet|Celkem|Počet všech dokončené úlohy.|Žádné dimenze|
|jobs.Failed|Neúspěšné úlohy|Počet|Celkem|Počet všechny neúspěšné úlohy.|Žádné dimenze|
|d2c.telemetry.ingress.sendThrottle|Počet omezení chyb|Počet|Celkem|Omezí počet omezení chyb z důvodu propustnost zařízení generovaný|Žádné dimenze|
|dailyMessageQuotaUsed|Celkový počet zpráv, které používá|Počet|Průměr|Počet celkový počet zpráv v současné době používá. Toto je kumulativní hodnotu, která je nastaven na hodnotu nula v 00:00 UTC každý den.|Žádné dimenze|
|deviceDataUsage|Celkový počet devicedata využití|Počet|Celkem|Bajtů přenesených do a z jakékoli zařízení připojené k platformě IotHub|Žádné dimenze|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|RegistrationAttempts|Pokusy o registraci|Počet|Celkem|Počet pokus o registraci zařízení|Stav ProvisioningServiceName, IotHubName,|
|DeviceAssignments|Zařízení přiřazená|Počet|Celkem|Počet zařízení přiřazená do služby IoT hub|ProvisioningServiceName IotHubName|
|AttestationAttempts|Pokusy o ověření identity|Počet|Celkem|Počet zařízení atestace podle pokus|Protokol ProvisioningServiceName, stav,|

## <a name="microsoftdeviceselasticpools"></a>Microsoft.Devices/ElasticPools

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|elasticPool.requestedUsageRate|míra požadovaný využití|Procento|Průměr|míra požadovaný využití|Žádné dimenze|

## <a name="microsoftdeviceselasticpoolsiothubtenants"></a>Microsoft.Devices/ElasticPools/IotHubTenants

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|tenantHub.requestedUsageRate|míra požadovaný využití|Procento|Průměr|míra požadovaný využití|Žádné dimenze|
|deviceDataUsage|Celkový počet devicedata využití|Počet|Celkem|Bajtů přenesených do a z jakékoli zařízení připojené k platformě IotHub|Žádné dimenze|
|d2c.telemetry.ingress.allProtocol|Pokusy o odeslání zprávy telemetrie|Počet|Celkem|Počet zařízení cloud telemetrické zprávy se pokusil odeslat do služby IoT hub|Žádné dimenze|
|d2c.telemetry.ingress.Success|Telemetrické zprávy odeslané|Počet|Celkem|Počet zařízení cloud telemetrické zprávy úspěšně odeslán do služby IoT hub|Žádné dimenze|
|c2d.Commands.egress.COMPLETE.Success|Příkazy byla dokončena|Počet|Celkem|Počet příkazů typu cloud zařízení úspěšně dokončila, a zařízení|Žádné dimenze|
|c2d.Commands.egress.Abandon.Success|Příkazy opuštění|Počet|Celkem|Počet příkazů typu cloud zařízení opuštěny v rámci zařízení|Žádné dimenze|
|c2d.Commands.egress.Reject.Success|Příkazy odmítnut|Počet|Celkem|Počet zařízení byl odmítnut příkazů typu cloud zařízení|Žádné dimenze|
|devices.totalDevices|Celkový počet zařízení|Počet|Celkem|Počet zařízení zaregistrovat do služby IoT hub|Žádné dimenze|
|devices.connectedDevices.allProtocol|Připojená zařízení|Počet|Celkem|Počet zařízení připojených ke službě IoT hub|Žádné dimenze|
|d2c.telemetry.egress.Success|Telemetrické zprávy doručit|Počet|Celkem|Počet zpráv byla úspěšně zapsána do koncových bodů (celkem)|Žádné dimenze|
|d2c.telemetry.egress.dropped|Vyřazené zprávy|Počet|Celkem|Počet zpráv vyřadit, protože koncový bod doručení byla neaktivní|Žádné dimenze|
|d2c.telemetry.egress.orphaned|Osamocené zprávy|Počet|Celkem|Počet zpráv, na které se neshodují žádné cesty, včetně záložní trasy|Žádné dimenze|
|d2c.telemetry.egress.invalid|Neplatné zprávy|Počet|Celkem|Počet zpráv, které nejsou doručeny z důvodu nekompatibility s nástrojem koncový bod|Žádné dimenze|
|d2c.telemetry.egress.Fallback|Zprávy odpovídající záložního stavu|Počet|Celkem|Počet zpráv zapsaných do záložního koncového bodu|Žádné dimenze|
|d2c.endpoints.egress.eventHubs|Zpráv doručených do koncové body centra událostí|Počet|Celkem|Počet zpráv byla úspěšně zapsána do koncové body centra událostí|Žádné dimenze|
|d2c.endpoints.latency.eventHubs|Latence zprávy pro koncové body centra událostí|Milisekundy|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příchozí zprávy do koncového bodu centra událostí, v milisekundách|Žádné dimenze|
|d2c.endpoints.egress.serviceBusQueues|Doručování zpráv do fronty Service Bus koncové body|Počet|Celkem|Počet zpráv byla úspěšně zapsána do koncových bodů frontou Service Bus|Žádné dimenze|
|d2c.endpoints.latency.serviceBusQueues|Latence zprávy pro koncové body frontou Service Bus|Milisekundy|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příjem příchozích dat zpráv do fronty Service Bus koncový bod, v milisekundách|Žádné dimenze|
|d2c.endpoints.egress.serviceBusTopics|Doručování zpráv do tématu Service Bus koncové body|Počet|Celkem|Počet zpráv byly úspěšně zapsána do tématu Service Bus koncové body|Žádné dimenze|
|d2c.endpoints.latency.serviceBusTopics|Latence zprávy pro koncové body témata sběrnice|Milisekundy|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příchozí zprávy do tématu Service Bus koncový bod, v milisekundách|Žádné dimenze|
|d2c.endpoints.egress.builtIn.events|Zpráv doručených do vestavěným koncovým bodem (zprávy nebo události)|Počet|Celkem|Počet zpráv byla úspěšně zapsána do vestavěným koncovým bodem (zprávy nebo události)|Žádné dimenze|
|d2c.endpoints.latency.builtIn.events|Zpráva latence pro předdefinovaný koncový bod (zprávy nebo události)|Milisekundy|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příjem příchozích dat zpráva do vestavěným koncovým bodem (zprávy událostí), v milisekundách |Žádné dimenze|
|d2c.endpoints.egress.Storage|Zpráv doručených do koncové body úložiště|Počet|Celkem|Počet zpráv byla úspěšně zapsána do koncové body úložiště|Žádné dimenze|
|d2c.endpoints.latency.Storage|Latence zprávy pro koncové body úložiště|Milisekundy|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příjem příchozích dat zpráva do koncový bod úložiště, v milisekundách|Žádné dimenze|
|d2c.endpoints.egress.Storage.bytes|Data zapsaná do úložiště|B|Celkem|Množství dat v bajtech, zapisovat do úložiště koncové body|Žádné dimenze|
|d2c.endpoints.egress.Storage.BLOBS|Zapisovat do úložiště objektů BLOB|Počet|Celkem|Počet zapsána do koncové body úložiště objektů BLOB|Žádné dimenze|
|d2c.Twin.Read.Success|Úspěšné twin čte ze zařízení|Počet|Celkem|Počet čtení všechny úspěšné twin spouštěná zařízení.|Žádné dimenze|
|d2c.Twin.Read.failure|Čtení twin ze zařízení se nezdařila|Počet|Celkem|Počet všech nepodařilo spouštěná zařízení twin čtení.|Žádné dimenze|
|d2c.Twin.Read.Size|Velikost odpovědi twin čtení ze zařízení|B|Průměr|Průměr, min a max všechny úspěšné, spouštěná zařízení twin čtení.|Žádné dimenze|
|d2c.Twin.Update.Success|Aktualizace úspěšná twin ze zařízení|Počet|Celkem|Počet aktualizací všechny úspěšné twin spouštěná zařízení.|Žádné dimenze|
|d2c.Twin.Update.failure|Aktualizace twin ze zařízení se nezdařila|Počet|Celkem|Počet všech se nezdařila aktualizace twin spouštěná zařízení.|Žádné dimenze|
|d2c.Twin.Update.Size|Velikost twin aktualizace ze zařízení|B|Průměr|Průměr, minimální a maximální velikost všech úspěšné, spouštěná zařízení twin aktualizace.|Žádné dimenze|
|c2d.Methods.Success|Volání úspěšné přímá metoda|Počet|Celkem|Počet všech úspěšných volání přímá metoda.|Žádné dimenze|
|c2d.Methods.failure|Přímá metoda volání se nezdařilo|Počet|Celkem|Počet všech se nezdařila, volání metod direct.|Žádné dimenze|
|c2d.Methods.requestSize|Žádost o velikosti volání přímá metoda|B|Průměr|Průměr, min a max všechny úspěšné požadavky přímá metoda.|Žádné dimenze|
|c2d.Methods.responseSize|Velikost odpovědi volání přímá metoda|B|Průměr|Průměr, min a max všechny úspěšné odpovědi přímá metoda.|Žádné dimenze|
|c2d.Twin.Read.Success|Úspěšné twin čte z back-end|Počet|Celkem|Počet všech úspěšně spustil back-end twin čtení.|Žádné dimenze|
|c2d.Twin.Read.failure|Neúspěšné twin čte z back-end|Počet|Celkem|Počet všech se nezdařilo čtení twin iniciované back-end.|Žádné dimenze|
|c2d.Twin.Read.Size|Velikost odpovědi twin čtení z back-end|B|Průměr|Průměr, min a max všechny úspěšné, iniciované back-end twin čtení.|Žádné dimenze|
|c2d.Twin.Update.Success|Aktualizace úspěšná twin z back-end|Počet|Celkem|Počet aktualizací všechny úspěšné twin iniciované back-end.|Žádné dimenze|
|c2d.Twin.Update.failure|Aktualizace se nezdařila twin z back-end|Počet|Celkem|Počet všech se nezdařila aktualizace twin iniciované back-end.|Žádné dimenze|
|c2d.Twin.Update.Size|Velikost twin aktualizace z back-end|B|Průměr|Průměr, minimální a maximální velikost všech úspěšné, iniciované back-end twin aktualizace.|Žádné dimenze|
|twinQueries.success|Úspěšné twin dotazy|Počet|Celkem|Počet všech úspěšné twin dotazů.|Žádné dimenze|
|twinQueries.failure|Neúspěšné twin dotazy|Počet|Celkem|Počet všechny neúspěšné twin dotazy.|Žádné dimenze|
|twinQueries.resultSize|Objem výsledků dotazů Twin|B|Průměr|Průměr, minimální a maximální velikosti výsledek všechny úspěšné twin dotazy.|Žádné dimenze|
|jobs.createTwinUpdateJob.success|Úspěšné vytvoření úloh aktualizace twin|Počet|Celkem|Počet všech úspěšném vytvoření twin aktualizace úloh.|Žádné dimenze|
|jobs.createTwinUpdateJob.failure|Neúspěšné vytvoření úloh aktualizace twin|Počet|Celkem|Počet všech se nezdařilo vytvoření twin aktualizace úlohy.|Žádné dimenze|
|jobs.createDirectMethodJob.success|Úspěšné vytvoření úloh volání – metoda|Počet|Celkem|Počet všech úspěšném vytvoření přímá metoda volání úloh.|Žádné dimenze|
|jobs.createDirectMethodJob.failure|Neúspěšné vytvoření úloh volání – metoda|Počet|Celkem|Počet všech se nezdařilo vytvoření přímá metoda vyvolání úlohy.|Žádné dimenze|
|jobs.listJobs.success|Úspěšné volání do seznamu úloh|Počet|Celkem|Počet všech úspěšných volání do seznamu úloh.|Žádné dimenze|
|jobs.listJobs.failure|Volání do seznamu úloh se nezdařilo|Počet|Celkem|Počet všech selhání volání do seznamu úloh.|Žádné dimenze|
|jobs.cancelJob.success|Zrušení úloh úspěšné|Počet|Celkem|Počet všech úspěšných volání zrušení úlohy.|Žádné dimenze|
|jobs.cancelJob.failure|Informace o neúspěšné úloze zrušení|Počet|Celkem|Počet všech volání se nezdařilo zrušit úlohu.|Žádné dimenze|
|jobs.queryJobs.success|Až se úloha úspěšně dotazy|Počet|Celkem|Počet všech úspěšných volání do úlohy dotaz.|Žádné dimenze|
|jobs.queryJobs.failure|Úloha se dotazuje se nezdařilo|Počet|Celkem|Počet všech selhání volání do úlohy dotaz.|Žádné dimenze|
|jobs.completed|Dokončené úlohy|Počet|Celkem|Počet všech dokončené úlohy.|Žádné dimenze|
|jobs.Failed|Neúspěšné úlohy|Počet|Celkem|Počet všechny neúspěšné úlohy.|Žádné dimenze|
|d2c.telemetry.ingress.sendThrottle|Počet omezení chyb|Počet|Celkem|Omezí počet omezení chyb z důvodu propustnost zařízení generovaný|Žádné dimenze|
|dailyMessageQuotaUsed|Celkový počet zpráv, které používá|Počet|Průměr|Počet celkový počet zpráv v současné době používá. Toto je kumulativní hodnotu, která je nastaven na hodnotu nula v 00:00 UTC každý den.|Žádné dimenze|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|MetadataRequests|Požadavků na metadata|Počet|Počet|Počet požadavků na metadata. Cosmos DB udržuje kolekci metadat systému pro každý účet, který umožňuje vytvořit výčet kolekce, databáze atd., a jejich konfiguraci, které jsou zdarma.|GlobalDatabaseAccountName, DatabaseName, název_kolekce, oblast, StatusCode|
|MongoRequestCharge|Mongo požadavek zdarma|Počet|Celkem|Jednotek žádosti mongo použití|GlobalDatabaseAccountName, DatabaseName, název_kolekce, oblast, CommandName, kód chyby|
|MongoRequests|Mongo požadavky|Počet|Počet|Počet zpracovaných požadavků Mongo|GlobalDatabaseAccountName, DatabaseName, název_kolekce, oblast, CommandName, kód chyby|
|TotalRequestUnits|Celkový požadavek jednotky|Počet|Celkem|Požadavku že využívat jednotky|GlobalDatabaseAccountName, DatabaseName, název_kolekce, oblast, StatusCode|
|TotalRequests|Požadavky celkem|Počet|Počet|Počet zpracovaných požadavků|GlobalDatabaseAccountName, DatabaseName, název_kolekce, oblast, StatusCode|


## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|SuccessfulRequests|Úspěšné požadavky (Preview)|Počet|Celkem|Úspěšné požadavky pro Microsoft.EventHub (Preview)|entityName|
|ServerErrors|Chyby serveru (Preview)|Počet|Celkem|Chyby serveru pro Microsoft.EventHub (Preview)|entityName|
|UserErrors|Chyby uživatele (Preview)|Počet|Celkem|Chyby uživatele pro Microsoft.EventHub (Preview)|entityName|
|QuotaExceededErrors|Chyby překročení kvóty (Preview)|Počet|Celkem|Chyby překročení kvóty pro Microsoft.EventHub (Preview)|entityName|
|ThrottledRequests|Omezené požadavky (Preview)|Počet|Celkem|Omezené požadavky pro Microsoft.EventHub (Preview)|entityName|
|IncomingRequests|Příchozí žádosti (Preview)|Počet|Celkem|Příchozí požadavky pro Microsoft.EventHub (Preview)|entityName|
|IncomingMessages|Příchozí zprávy (Preview)|Počet|Celkem|Příchozí zprávy pro Microsoft.EventHub (Preview)|entityName|
|OutgoingMessages|Odchozí zprávy (Preview)|Počet|Celkem|Odchozí zprávy pro Microsoft.EventHub (Preview)|entityName|
|IncomingBytes|Příchozí bajty (Preview)|B|Celkem|Příchozí bajty pro Microsoft.EventHub (Preview)|entityName|
|OutgoingBytes|Odchozí bajty (Preview)|B|Celkem|Odchozí bajty pro Microsoft.EventHub (Preview)|entityName|
|ActiveConnections|ActiveConnections (Preview)|Počet|Celkem|Celkový počet aktivních připojení pro Microsoft.EventHub (Preview)|entityName|
|ConnectionsOpened|Otevřená připojení (Preview)|Počet|Celkem|Otevřená připojení pro Microsoft.EventHub (Preview)|entityName|
|ConnectionsClosed|Ukončená připojení (Preview)|Počet|Celkem|Ukončená připojení pro Microsoft.EventHub (Preview)|entityName|
|CaptureBacklog|Zachytit backlog (Preview)|Počet|Celkem|Zachytit backlog pro Microsoft.EventHub (Preview)|entityName|
|CapturedMessages|Zachycené zprávy (Preview)|Počet|Celkem|Zachycené zprávy pro Microsoft.EventHub (Preview)|entityName|
|CapturedBytes|Zachycené bajty (Preview)|B|Celkem|Zachycené bajty pro Microsoft.EventHub (Preview)|entityName|
|INREQS|Příchozí požadavky|Počet|Celkem|Celkový počet příchozích žádostí o odeslání pro obor názvů|Žádné dimenze|
|SUCCREQ|Úspěšné požadavky|Počet|Celkem|Celkový počet úspěšných žádostí pro obor názvů|Žádné dimenze|
|FAILREQ|Neúspěšné požadavky|Počet|Celkem|Celkový počet neúspěšných žádostí pro obor názvů|Žádné dimenze|
|SVRBSY|Chyby kvůli zaneprázdněnosti serveru|Počet|Celkem|Celkový počet chyb kvůli zaneprázdnění serveru pro obor názvů|Žádné dimenze|
|INTERR|Vnitřní chyby serveru|Počet|Celkem|Celkový počet vnitřních chyb serveru pro obor názvů|Žádné dimenze|
|MISCERR|Jiné chyby|Počet|Celkem|Celkový počet neúspěšných žádostí pro obor názvů|Žádné dimenze|
|INMSGS|Příchozí zprávy (zastaralé)|Počet|Celkem|Celkový počet příchozích zpráv pro obor názvů. Tato metrika je zastaralý. Místo toho použijte metrika příchozí zprávy|Žádné dimenze|
|EHINMSGS|Příchozí zprávy|Počet|Celkem|Celkový počet příchozích zpráv pro obor názvů|Žádné dimenze|
|OUTMSGS|Odchozí zprávy (zastaralé)|Počet|Celkem|Celkový počet odchozích zpráv pro obor názvů. Tato metrika je zastaralý. Místo toho použijte metrika odchozích zpráv|Žádné dimenze|
|EHOUTMSGS|Odchozí zprávy|Počet|Celkem|Celkový počet odchozích zpráv pro obor názvů|Žádné dimenze|
|EHINMBS|Příchozí bajty (zastaralé)|B|Celkem|Události rozbočovače příchozí zprávy propustnost pro obor názvů. Tato metrika je zastaralý. Místo toho použijte metrika Příchozí bajty|Žádné dimenze|
|EHINBYTES|Příchozí bajty|B|Celkem|Propustnost příchozích zpráv centra událostí pro obor názvů|Žádné dimenze|
|EHOUTMBS|Odchozí bajty (zastaralé)|B|Celkem|Události rozbočovače odchozí zprávy propustnost pro obor názvů. Tato metrika je zastaralý. Místo toho použijte metrika Odchozí bajty|Žádné dimenze|
|EHOUTBYTES|Odchozí bajty|B|Celkem|Propustnost odchozích zpráv centra událostí pro obor názvů|Žádné dimenze|
|EHABL|Archivovat zprávy backlogu|Počet|Celkem|Archivované zprávy centra událostí v backlogu pro obor názvů|Žádné dimenze|
|EHAMSGS|Archivace zpráv|Počet|Celkem|Archivované zprávy centra událostí v oboru názvů|Žádné dimenze|
|EHAMBS|Archivovat propustnost zpráv|B|Celkem|Propustnost archivovaných zpráv Centra událostí v oboru názvů|Žádné dimenze|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|ObservedMetricValue|Zjištěná hodnota metriky|Počet|Průměr|Hodnota vypočítaná automatickým škálováním při jeho spuštění|MetricTriggerSource|
|MetricThreshold|Mezní hodnota metriky|Počet|Průměr|Konfigurovaná mezní hodnota automatického škálování, když se automatické škálování spustilo|MetricTriggerRule|
|ObservedCapacity|Zjištěná kapacita|Počet|Průměr|Kapacita ohlášená automatickému škálování při jeho spuštění|Žádné dimenze|
|ScaleActionsInitiated|Zahájené akce škálování|Počet|Celkem|Směr operace škálování|ScaleDirection|

## <a name="microsoftlocationbasedservicesaccounts"></a>Microsoft.LocationBasedServices/accounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Latence|Latence|Milisekundy|Průměr|Doba volání API|OperationName, výsledek|

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
|VipAvailability|Dostupnost virtuálních IP adres|Počet|Průměr|Dostupnost virtuální IP adresy koncových bodů, na základě výsledků testu|Virtuální IP adresa, VipPort|
|DipAvailability|Dostupnost vyhrazené IP adresy|Počet|Průměr|Dostupnost vyhrazené IP adresy koncových bodů, na základě výsledků testu|Typprotokolu, DipPort, virtuální IP adresa, VipPort, DipAddress|
|ByteCount|Počet bajtů|Počet|Celkem|Celkový počet bajtů přenesených v časovém období|Virtuální IP adresa, VipPort, směr|
|PacketCount|Počet paketů|Počet|Celkem|Celkový počet paketů odeslaných v časovém období|Virtuální IP adresa, VipPort, směr|
|SYNCount|Počet SYN|Počet|Celkem|Celkový počet paketů SYN přenášených v časovém období|Virtuální IP adresa, VipPort, směr|
|SnatConnectionCount|Počet připojení překládat pomocí SNAT|Počet|Celkem|Celkový počet nových připojení překládat pomocí SNAT vytvořených v časovém období|Virtuální IP adresa, DipAddress, ConnectionState|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|PacketsInDDoS|Příchozí pakety DDoS|CountPerSecond|Maximum|Příchozí pakety DDoS|Žádné dimenze|
|PacketsDroppedDDoS|Příchozí pakety vyřadit DDoS|CountPerSecond|Maximum|Příchozí pakety vyřadit DDoS|Žádné dimenze|
|PacketsForwardedDDoS|Příchozí pakety předávané DDoS|CountPerSecond|Maximum|Příchozí pakety předávané DDoS|Žádné dimenze|
|TCPPacketsInDDoS|Příchozí pakety TCP DDoS|CountPerSecond|Maximum|Příchozí pakety TCP DDoS|Žádné dimenze|
|TCPPacketsDroppedDDoS|Příchozí pakety TCP vyřadit DDoS|CountPerSecond|Maximum|Příchozí pakety TCP vyřadit DDoS|Žádné dimenze|
|TCPPacketsForwardedDDoS|Příchozí pakety TCP předávají DDoS|CountPerSecond|Maximum|Příchozí pakety TCP předávají DDoS|Žádné dimenze|
|UDPPacketsInDDoS|Příchozí pakety UDP DDoS|CountPerSecond|Maximum|Příchozí pakety UDP DDoS|Žádné dimenze|
|UDPPacketsDroppedDDoS|Příchozí pakety UDP vyřadit DDoS|CountPerSecond|Maximum|Příchozí pakety UDP vyřadit DDoS|Žádné dimenze|
|UDPPacketsForwardedDDoS|Příchozí pakety UDP předávají DDoS|CountPerSecond|Maximum|Příchozí pakety UDP předávají DDoS|Žádné dimenze|
|BytesInDDoS|Příchozí bajty DDoS|BytesPerSecond|Maximum|Příchozí bajty DDoS|Žádné dimenze|
|BytesDroppedDDoS|Příchozí bajty vyřadit DDoS|BytesPerSecond|Maximum|Příchozí bajty vyřadit DDoS|Žádné dimenze|
|BytesForwardedDDoS|Příchozí bajty předávaných DDoS|BytesPerSecond|Maximum|Příchozí bajty předávaných DDoS|Žádné dimenze|
|TCPBytesInDDoS|Příchozí bajty TCP DDoS|BytesPerSecond|Maximum|Příchozí bajty TCP DDoS|Žádné dimenze|
|TCPBytesDroppedDDoS|Příchozí bajty TCP vyřadit DDoS|BytesPerSecond|Maximum|Příchozí bajty TCP vyřadit DDoS|Žádné dimenze|
|TCPBytesForwardedDDoS|Příchozí bajty TCP předávaných DDoS|BytesPerSecond|Maximum|Příchozí bajty TCP předávaných DDoS|Žádné dimenze|
|UDPBytesInDDoS|Příchozí bajty UDP DDoS|BytesPerSecond|Maximum|Příchozí bajty UDP DDoS|Žádné dimenze|
|UDPBytesDroppedDDoS|Příchozí bajty UDP vyřadit DDoS|BytesPerSecond|Maximum|Příchozí bajty UDP vyřadit DDoS|Žádné dimenze|
|UDPBytesForwardedDDoS|Příchozí bajty UDP předávaných DDoS|BytesPerSecond|Maximum|Příchozí bajty UDP předávaných DDoS|Žádné dimenze|
|IfUnderDDoSAttack|V části DDoS útokům nebo ne|Počet|Maximum|V části DDoS útokům nebo ne|Žádné dimenze|
|DDoSTriggerTCPPackets|Příchozí pakety TCP k aktivaci DDoS zmírnění|CountPerSecond|Maximum|Příchozí pakety TCP k aktivaci DDoS zmírnění|Žádné dimenze|
|DDoSTriggerUDPPackets|Příchozí pakety UDP pro aktivaci DDoS zmírnění|CountPerSecond|Maximum|Příchozí pakety UDP pro aktivaci DDoS zmírnění|Žádné dimenze|
|DDoSTriggerSYNPackets|Příchozí pakety SYN k aktivaci DDoS zmírnění|CountPerSecond|Maximum|Příchozí pakety SYN k aktivaci DDoS zmírnění|Žádné dimenze|
|VipAvailability|Dostupnost|Počet|Průměr|Průměrná dostupnost IPAddress v časovém období|Port|
|ByteCount|Počet bajtů|Počet|Celkem|Celkový počet bajtů přenesených v časovém období|Port, směr|
|PacketCount|Počet paketů|Počet|Celkem|Celkový počet paketů odeslaných v časovém období|Port, směr|
|SynCount|Počet SYN|Počet|Celkem|Celkový počet paketů SYN přenášených v časovém období|Port, směr|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Propustnost|Propustnost|BytesPerSecond|Celkem|Počet bajtů za sekundu, které se má zpracovat aplikační bránu|Žádné dimenze|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TunnelAverageBandwidth|Tunelové propojení šířky pásma|BytesPerSecond|Průměr|Průměrná šířky pásma tunelu v bajtech za sekundu|ConnectionName, Vzdálená adresa IP|
|TunnelEgressBytes|Tunelové propojení Odchozí bajty|B|Celkem|Odchozí bajty tunelového propojení|ConnectionName, Vzdálená adresa IP|
|TunnelIngressBytes|Příchozí bajty tunelového propojení|B|Celkem|Příchozí bajty tunelového propojení|ConnectionName, Vzdálená adresa IP|
|TunnelEgressPackets|Odchozí pakety tunelového propojení|Počet|Celkem|Počet odchozích paketů tunelového propojení|ConnectionName, Vzdálená adresa IP|
|TunnelIngressPackets|Příchozí pakety tunelového propojení|Počet|Celkem|Příchozí paket počet přes tunelové propojení|ConnectionName, Vzdálená adresa IP|
|TunnelEgressPacketDropTSMismatch|Tunelové propojení odchozí TS neshoda paketu rozevírací|Počet|Celkem|Odchozí počet vyřazení z provozu selektor neshoda tunelového propojení|ConnectionName, Vzdálená adresa IP|
|TunnelIngressPacketDropTSMismatch|Tunelové propojení příchozího TS neshoda paketu rozevírací|Počet|Celkem|Příchozí počet vyřazení z provozu selektor neshoda tunelového propojení|ConnectionName, Vzdálená adresa IP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Průměr|Služba BITS ingressing Azure za sekundu|Žádné dimenze|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Průměr|Služba BITS egressing Azure za sekundu|Žádné dimenze|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|QpsByEndpoint|Dotazy pomocí vrátil koncový bod|Počet|Celkem|Kolikrát koncový bod Traffic Manager vrátil v zadaném časovém období|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Stav koncový bod koncovým bodem|Počet|Maximum|1, pokud koncového bodu testu stav "povoleno", jinak hodnota 0.|EndpointName|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|Registration.all|Registrace – operace|Počet|Celkem|Počet všech úspěšných operací s registrací (vytvoření, aktualizace, dotazy a odstranění) |Žádné dimenze|
|Registration.Create|Registrace – vytvořit operace|Počet|Celkem|Počet všech úspěšných vytvoření registrací|Žádné dimenze|
|Registration.Update|Registrace – aktualizovat operace|Počet|Celkem|Počet všech úspěšných aktualizací registrací|Žádné dimenze|
|Registration.Get|Registrace – přečíst operace|Počet|Celkem|Počet všech úspěšných dotazů registrací|Žádné dimenze|
|Registration.delete|Registrace – odstranit operace|Počet|Celkem|Počet všech úspěšných odstranění registrací|Žádné dimenze|
|příchozí|Příchozí zprávy|Počet|Celkem|Počet všech úspěšných volání rozhraní API, pomocí kterých se odesílají data |Žádné dimenze|
|Incoming.Scheduled|Odeslaná plánovaná nabízená oznámení|Počet|Celkem|Zrušená plánovaná nabízená oznámení|Žádné dimenze|
|Incoming.Scheduled.Cancel|Zrušená plánovaná nabízená oznámení|Počet|Celkem|Zrušená plánovaná nabízená oznámení|Žádné dimenze|
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
|outgoing.wns.invalidcredentials|WNS – chyby autorizace (neplatné přihlašovací údaje)|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože systém oznámení platformy nepřijal poskytnuté přihlašovací údaje, nebo jsou přihlašovací údaje blokované (Windows Live nerozpoznal přihlašovací údaje).|Žádné dimenze|
|outgoing.wns.badchannel|WNS – chyba špatného kanálu|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože se v registraci nerozpoznal parametr ChannelURI (stav Služby nabízených oznámení Windows: 404 Nenalezeno)|Žádné dimenze|
|outgoing.wns.expiredchannel|WNS – chyba vypršení časového limitu kanálu|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože vypršela platnost parametru ChannelURI (stav Služby nabízených oznámení Windows: 410 Trvale není k dispozici)|Žádné dimenze|
|outgoing.wns.throttled|WNS – omezená oznámení|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože Služba nabízených oznámení Windows omezuje tuto aplikaci (stav Služby nabízených oznámení Windows: 406 Nepřijatelný požadavek)|Žádné dimenze|
|outgoing.wns.tokenproviderunreachable|WNS – chyby autorizace (nedostupné)|Počet|Celkem|Služby Windows Live nejsou dostupné.|Žádné dimenze|
|outgoing.wns.invalidtoken|WNS – chyby autorizace (neplatný token)|Počet|Celkem|Token, který se poskytl Službě nabízených oznámení Windows, není platný (stav Služby nabízených oznámení Windows: 401 Neautorizováno).|Žádné dimenze|
|outgoing.wns.wrongtoken|WNS – chyby autorizace (chybný token)|Počet|Celkem|Zadaný pro WNS token je platný, ale pro jinou aplikaci (WNS stav: 403 Zakázáno). To může nastat, když ChannelURI v registraci souvisí s jinou aplikací. Zkontrolujte, jestli klientská aplikace je přidružená ke stejné aplikaci, jehož pověření se v centru oznámení.|Žádné dimenze|
|outgoing.wns.invalidnotificationformat|WNS – neplatný formát oznámení|Počet|Celkem|Formát oznámení není platný (stav WNS: 400). Všimněte si, že WNS není odmítnout všechny neplatné datové části.|Žádné dimenze|
|outgoing.wns.invalidnotificationsize|WNS – chyba neplatné velikosti oznámení|Počet|Celkem|Datová část oznámení je moc velká (stav Služby nabízených oznámení Windows: 413).|Žádné dimenze|
|outgoing.wns.channelthrottled|WNS – omezený kanál|Počet|Celkem|Oznámení se ignoruje, protože v registraci se omezuje parametr ChannelURI (hlavička odpovědi Služby nabízených oznámení Windows: X-WNS-NotificationStatus:channelThrottled).|Žádné dimenze|
|outgoing.wns.channeldisconnected|WNS – odpojený kanál|Počet|Celkem|Oznámení se ignoruje, protože v registraci se omezuje parametr ChannelURI (hlavička odpovědi Služby nabízených oznámení Windows: X-WNS-DeviceConnectionStatus: disconnected).|Žádné dimenze|
|outgoing.wns.dropped|WNS – vynechaná oznámení|Počet|Celkem|Oznámení se ignoruje, protože v registraci se omezuje parametr ChannelURI (X-WNS-NotificationStatus: dropped, ale ne X-WNS-DeviceConnectionStatus: disconnected).|Žádné dimenze|
|outgoing.wns.pnserror|WNS – chyby|Počet|Celkem|Oznámení se nedoručilo kvůli chybám při komunikaci se Službou nabízených oznámení Windows.|Žádné dimenze|
|outgoing.wns.authenticationerror|WNS – chyby ověřování|Počet|Celkem|Oznámení se nedoručilo kvůli chybám při komunikaci s Windows Live, neplatným přihlašovacím údajům, nebo chybnému tokenu.|Žádné dimenze|
|outgoing.apns.Success|APNS – úspěšná oznámení|Počet|Celkem|Počet všech úspěšných oznámení|Žádné dimenze|
|outgoing.apns.invalidcredentials|Chyby autorizace APNs|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože systém oznámení platformy nepřijal poskytnuté přihlašovací údaje, nebo jsou přihlašovací údaje blokované|Žádné dimenze|
|outgoing.apns.badchannel|Chyba APNS – špatný kanál|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože token je neplatný (kód stavu APNs: 8)|Žádné dimenze|
|outgoing.apns.expiredchannel|Chyba APNS – kanálu vypršel časový limit|Počet|Celkem|Počet tokenů, kterým kanál zpětné vazby APNs zrušil platnost|Žádné dimenze|
|outgoing.apns.invalidnotificationsize|APNS – chyba neplatné velikosti oznámení|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože datová část byla moc velká (kód stavu APNs: 7)|Žádné dimenze|
|outgoing.apns.pnserror|APNS – chyby|Počet|Celkem|Počet nabídek, které nebyly úspěšné kvůli chybám při komunikaci s APNs|Žádné dimenze|
|outgoing.gcm.Success|GCM – úspěšná oznámení|Počet|Celkem|Počet všech úspěšných oznámení|Žádné dimenze|
|outgoing.gcm.invalidcredentials|GCM – chyby autorizace (neplatné přihlašovací údaje)|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože systém oznámení platformy nepřijal poskytnuté přihlašovací údaje, nebo jsou přihlašovací údaje blokované|Žádné dimenze|
|outgoing.gcm.badchannel|GCM – chyba špatného kanálu|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože se v registraci nerozpoznal parametr registrationId (výsledek GCM: Invalid Registration)|Žádné dimenze|
|outgoing.gcm.expiredchannel|GCM – chyba vypršení časového limitu kanálu|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože vypršela platnost registrationId v registraci (výsledek GCM: NotRegistered)|Žádné dimenze|
|outgoing.gcm.throttled|GCM – omezená oznámení|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože služba GCM omezila tuto aplikaci (kód stavu GCM: 501–599 nebo výsledek: Unavailable)|Žádné dimenze|
|outgoing.gcm.invalidnotificationformat|GCM – neplatný formát oznámení|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože datová část neměla správný formát (výsledek GCM: InvalidDataKey nebo InvalidTtl)|Žádné dimenze|
|outgoing.gcm.invalidnotificationsize|GCM – chyba neplatné velikosti oznámení|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože datová část byla moc velká (výsledek GCM: MessageTooBig)|Žádné dimenze|
|outgoing.gcm.wrongchannel|GCM – chyba nesprávného kanálu|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože registrationId v registraci není přidružené k aktuální aplikaci (výsledek GCM: InvalidPackageName)|Žádné dimenze|
|outgoing.gcm.pnserror|GCM – chyby|Počet|Celkem|Počet nabídek, které nebyly úspěšné kvůli chybám při komunikaci s GCM|Žádné dimenze|
|outgoing.gcm.authenticationerror|Chyby ověřování GCM|Počet|Celkem|Počet nabídek, které nebyly úspěšné, protože systém oznámení platformy nepřijal poskytnuté přihlašovací údaje, nebo jsou přihlašovací údaje blokované, nebo v aplikaci není správně nakonfigurované SenderId (výsledek GCM: MismatchedSenderId)|Žádné dimenze|
|outgoing.mpns.Success|MPNS – úspěšná oznámení|Počet|Celkem|Počet všech úspěšných oznámení|Žádné dimenze|
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
|incoming.all.failedrequests|Všechny neúspěšné příchozí požadavky|Počet|Celkem|Celkový počet neúspěšných příchozích požadavků pro centrum oznámení|Žádné dimenze|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|QueryDuration||Počet|Průměr||Žádné dimenze|
|QueryPoolJobQueueLength|Vláken: Délka fronty úloh fondu dotazu|Počet|Průměr|Počet úloh ve frontě fondu vláken dotazu.|Žádné dimenze|

## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|ListenerConnections-Success|ListenerConnections-Success|Počet|Celkem|Úspěšná připojení ListenerConnections pro Microsoft.Relay|entityName|
|ListenerConnections-ClientError|ListenerConnections-ClientError|Počet|Celkem|ClientError v ListenerConnections pro Microsoft.Relay|entityName|
|ListenerConnections-ServerError|ListenerConnections-ServerError|Počet|Celkem|ServerError na ListenerConnections pro Microsoft.Relay|entityName|
|SenderConnections-Success|SenderConnections-Success|Počet|Celkem|Úspěšná připojení SenderConnections pro Microsoft.Relay|entityName|
|SenderConnections-ClientError|SenderConnections-ClientError|Počet|Celkem|ClientError v SenderConnections pro Microsoft.Relay|entityName|
|SenderConnections-ServerError|SenderConnections-ServerError|Počet|Celkem|ServerError na SenderConnections pro Microsoft.Relay|entityName|
|ListenerConnections-TotalRequests|ListenerConnections-TotalRequests|Počet|Celkem|ListenerConnections celkem pro Microsoft.Relay|entityName|
|SenderConnections-TotalRequests|SenderConnections-TotalRequests|Počet|Celkem|Celkem žádostí SenderConnections pro Microsoft.Relay|entityName|
|ActiveConnections|ActiveConnections|Počet|Celkem|ActiveConnections celkem pro Microsoft.Relay|entityName|
|ActiveListeners|ActiveListeners|Počet|Celkem|ActiveListeners celkem pro Microsoft.Relay|entityName|
|BytesTransferred|BytesTransferred|Počet|Celkem|BytesTransferred celkem pro Microsoft.Relay|entityName|
|ListenerDisconnects|ListenerDisconnects|Počet|Celkem|ListenerDisconnects celkem pro Microsoft.Relay|entityName|
|SenderDisconnects|SenderDisconnects|Počet|Celkem|SenderDisconnects celkem pro Microsoft.Relay|entityName|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|SearchLatency|Latence vyhledávání|Sekundy|Průměr|Hledání Průměrná latence pro službu vyhledávání.|Žádné dimenze|
|SearchQueriesPerSecond|Vyhledávací dotazy za sekundu|CountPerSecond|Průměr|Vyhledávací dotazy za sekundu pro službu vyhledávání.|Žádné dimenze|
|ThrottledSearchQueriesPercentage|Procento omezených vyhledávacích dotazů|Procento|Průměr|Procento vyhledávací dotazy, které byly omezené pro službu vyhledávání.|Žádné dimenze|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|SuccessfulRequests|Úspěšné požadavky (Preview)|Počet|Celkem|Celkový počet úspěšných požadavků pro obor názvů (Preview)|entityName|
|ServerErrors|Chyby serveru (Preview)|Počet|Celkem|Chyby serveru pro Microsoft.ServiceBus (Preview)|entityName|
|UserErrors|Chyby uživatele (Preview)|Počet|Celkem|Chyby uživatele pro Microsoft.ServiceBus (Preview)|entityName|
|ThrottledRequests|Omezené požadavky (Preview)|Počet|Celkem|Omezené požadavky pro Microsoft.ServiceBus (Preview)|entityName|
|IncomingRequests|Příchozí žádosti (Preview)|Počet|Celkem|Příchozí požadavky pro Microsoft.ServiceBus (Preview)|entityName|
|IncomingMessages|Příchozí zprávy (Preview)|Počet|Celkem|Příchozí zprávy pro Microsoft.ServiceBus (Preview)|entityName|
|OutgoingMessages|Odchozí zprávy (Preview)|Počet|Celkem|Odchozí zprávy pro Microsoft.ServiceBus (Preview)|entityName|
|ActiveConnections|ActiveConnections (Preview)|Počet|Celkem|Celkový počet aktivních připojení pro Microsoft.ServiceBus (Preview)|entityName|
|ConnectionsOpened|Otevřená připojení (Preview)|Počet|Celkem|Otevřená připojení pro Microsoft.ServiceBus (Preview)|entityName|
|ConnectionsClosed|Ukončená připojení (Preview)|Počet|Celkem|Ukončená připojení pro Microsoft.ServiceBus (Preview)|entityName|
|CPUXNS|Využití CPU na obor názvů|Procento|Maximum|Metrika využití procesoru v oboru názvů služby Service Bus na úrovni Premium |Žádné dimenze|
|WSXNS|Využití paměti na obor názvů|Procento|Maximum|Metrika využití paměti v oboru názvů služby Service Bus na úrovni Premium |Žádné dimenze|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|cpu_percent|Procento CPU|Procento|Průměr|Procento CPU|Žádné dimenze|
|physical_data_read_percent|Procento datových V/V|Procento|Průměr|Procento datových V/V|Žádné dimenze|
|log_write_percent|Procento vstupně-výstupní operace protokolu|Procento|Průměr|Procento vstupně-výstupní operace protokolu|Žádné dimenze|
|dtu_consumption_percent|Procento DTU|Procento|Průměr|Procento DTU|Žádné dimenze|
|úložiště|Velikost celkový databáze|B|Maximum|Velikost celkový databáze|Žádné dimenze|
|connection_successful|Úspěšné připojení|Počet|Celkem|Úspěšné připojení|Žádné dimenze|
|connection_failed|Neúspěšné připojení|Počet|Celkem|Neúspěšné připojení|Žádné dimenze|
|blocked_by_firewall|Blokováno bránou Firewall|Počet|Celkem|Blokováno bránou Firewall|Žádné dimenze|
|zablokování|Zablokování|Počet|Celkem|Zablokování|Žádné dimenze|
|storage_percent|Procento velikosti databáze|Procento|Maximum|Procento velikosti databáze|Žádné dimenze|
|xtp_storage_percent|Procento úložiště OLTP v paměti|Procento|Průměr|Procento úložiště OLTP v paměti|Žádné dimenze|
|workers_percent|Procento pracovních procesů|Procento|Průměr|Procento pracovních procesů|Žádné dimenze|
|sessions_percent|Procento relací|Procento|Průměr|Procento relací|Žádné dimenze|
|dtu_limit|Omezení jednotek DTU|Počet|Průměr|Omezení jednotek DTU|Žádné dimenze|
|dtu_used|Použít DTU|Počet|Průměr|Použít DTU|Žádné dimenze|
|dwu_limit|DWU limit|Počet|Maximum|DWU limit|Žádné dimenze|
|dwu_consumption_percent|Procento DWU|Procento|Maximum|Procento DWU|Žádné dimenze|
|dwu_used|DWU použít|Počet|Maximum|DWU použít|Žádné dimenze|
|dw_cpu_percent|Úroveň uzlu DW procento využití procesoru|Procento|Průměr|Úroveň uzlu DW procento využití procesoru|dw_logical_node_id|
|dw_physical_data_read_percent|Procento úroveň vstupně-výstupní operace dat datového skladu uzlu|Procento|Průměr|Procento úroveň vstupně-výstupní operace dat datového skladu uzlu|dw_logical_node_id|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|cpu_percent|Procento CPU|Procento|Průměr|Procento CPU|Žádné dimenze|
|physical_data_read_percent|Procento datových V/V|Procento|Průměr|Procento datových V/V|Žádné dimenze|
|log_write_percent|Procento vstupně-výstupní operace protokolu|Procento|Průměr|Procento vstupně-výstupní operace protokolu|Žádné dimenze|
|dtu_consumption_percent|Procento DTU|Procento|Průměr|Procento DTU|Žádné dimenze|
|storage_percent|Procento úložiště|Procento|Průměr|Procento úložiště|Žádné dimenze|
|workers_percent|Procento pracovních procesů|Procento|Průměr|Procento pracovních procesů|Žádné dimenze|
|sessions_percent|Procento relací|Procento|Průměr|Procento relací|Žádné dimenze|
|eDTU_limit|omezení eDTU|Počet|Průměr|omezení eDTU|Žádné dimenze|
|storage_limit|Limit úložiště|B|Průměr|Limit úložiště|Žádné dimenze|
|eDTU_used|eDTU použít|Počet|Průměr|eDTU použít|Žádné dimenze|
|storage_used|Využité úložiště|B|Průměr|Využité úložiště|Žádné dimenze|
|xtp_storage_percent|Procento úložiště OLTP v paměti|Procento|Průměr|Procento úložiště OLTP v paměti|Žádné dimenze|

## <a name="microsoftsqlservers"></a>Microsoft.Sql/servers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|dtu_consumption_percent|Procento DTU|Procento|Průměr|Procento DTU|ElasticPoolResourceId|
|storage_used|Využité úložiště|B|Průměr|Využité úložiště|ElasticPoolResourceId|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts.

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|UsedCapacity|Použitá kapacita|B|Průměr|Kapacita využitá účtem|Žádné dimenze|
|Transakce|Transakce|Počet|Celkem|Počet požadavků na služby úložiště nebo zadaná operace rozhraní API. Tato hodnota zahrnuje úspěšné i neúspěšné požadavky, jakož i požadavků, které vytváří chyby. Hodnota ResponseType dimenzi používejte pro počet jiný typ odpovědi.|ResponseType, GeoType, ApiName|
|Příchozí přenos dat|Příchozí přenos dat|B|Celkem|Množství dat příjem příchozích dat v bajtech. Tato hodnota zahrnuje příjem příchozích dat z externí klienta do Azure Storage, jakož i příjem příchozích dat v rámci Azure.|GeoType, ApiName|
|Výchozí přenos|Výchozí přenos|B|Celkem|Množství odchozí data v bajtech. Tato hodnota zahrnuje odchozí z externí klienta do Azure Storage a také odchozí v rámci Azure. Toto číslo v důsledku toho nemusí odpovídat fakturovatelný odchozí.|GeoType, ApiName|
|SuccessServerLatency|Latence serveru při úspěchu|Milisekundy|Průměr|Průměrná latence úložiště Azure používá ke zpracování v případě úspěšné žádosti v milisekundách. Tato hodnota nezahrnuje latenci sítě, zadaný v AverageE2ELatency.|GeoType, ApiName|
|SuccessE2ELatency|Celková latence při úspěchu|Milisekundy|Průměr|Průměrná latence začátku do konce úspěšné požadavky na služby úložiště nebo zadaná operace rozhraní API, v milisekundách. Tato hodnota zahrnuje požadovaná doba zpracování v rámci Azure Storage pro čtení požadavku, odeslání odpovědi a přijímat potvrzení odpovědi.|GeoType, ApiName|
|Dostupnost|Dostupnost|Procento|Průměr|Procento dostupnosti pro službu úložiště nebo zadaná operace rozhraní API. Dostupnost se vypočítává tak, že hodnota TotalBillableRequests dělení podle počtu příslušné požadavky, včetně těch, které vytváří neočekávaným chybám. Všechny neočekávaným chybám výsledkem je omezená dostupnost pro službu úložiště nebo zadaná operace rozhraní API.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|BlobCapacity|Kapacita služby Blob|B|Průměr|Velikost úložiště využitá službou Blob účtu úložiště v bajtech|BlobType|
|BlobCount|Počet objektů blob|Počet|Průměr|Počet objektů blob ve službě Blob účtu úložiště|BlobType|
|ContainerCount|Počet kontejnerů služby Blob|Počet|Průměr|Počet kontejnerů ve službě Blob účtu úložiště|Žádné dimenze|
|Transakce|Transakce|Počet|Celkem|Počet požadavků na služby úložiště nebo zadaná operace rozhraní API. Tato hodnota zahrnuje úspěšné i neúspěšné požadavky, jakož i požadavků, které vytváří chyby. Hodnota ResponseType dimenzi používejte pro počet jiný typ odpovědi.|ResponseType, GeoType, ApiName|
|Příchozí přenos dat|Příchozí přenos dat|B|Celkem|Množství dat příjem příchozích dat v bajtech. Tato hodnota zahrnuje příjem příchozích dat z externí klienta do Azure Storage, jakož i příjem příchozích dat v rámci Azure.|GeoType, ApiName|
|Výchozí přenos|Výchozí přenos|B|Celkem|Množství odchozí data v bajtech. Tato hodnota zahrnuje odchozí z externí klienta do Azure Storage a také odchozí v rámci Azure. Toto číslo v důsledku toho nemusí odpovídat fakturovatelný odchozí.|GeoType, ApiName|
|SuccessServerLatency|Latence serveru při úspěchu|Milisekundy|Průměr|Průměrná latence úložiště Azure používá ke zpracování v případě úspěšné žádosti v milisekundách. Tato hodnota nezahrnuje latenci sítě, zadaný v AverageE2ELatency.|GeoType, ApiName|
|SuccessE2ELatency|Celková latence při úspěchu|Milisekundy|Průměr|Průměrná latence začátku do konce úspěšné požadavky na služby úložiště nebo zadaná operace rozhraní API, v milisekundách. Tato hodnota zahrnuje požadovaná doba zpracování v rámci Azure Storage pro čtení požadavku, odeslání odpovědi a přijímat potvrzení odpovědi.|GeoType, ApiName|
|Dostupnost|Dostupnost|Procento|Průměr|Procento dostupnosti pro službu úložiště nebo zadaná operace rozhraní API. Dostupnost se vypočítává tak, že hodnota TotalBillableRequests dělení podle počtu příslušné požadavky, včetně těch, které vytváří neočekávaným chybám. Všechny neočekávaným chybám výsledkem je omezená dostupnost pro službu úložiště nebo zadaná operace rozhraní API.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|TableCapacity|Kapacita služby Table|B|Průměr|Velikost úložiště využitá službou Table účtu úložiště v bajtech|Žádné dimenze|
|TableCount|Počet tabulek|Počet|Průměr|Počet tabulek ve službě Table účtu úložiště|Žádné dimenze|
|TableEntityCount|Počet entit tabulek|Počet|Průměr|Počet entit tabulek ve službě Table účtu úložiště|Žádné dimenze|
|Transakce|Transakce|Počet|Celkem|Počet požadavků na služby úložiště nebo zadaná operace rozhraní API. Tato hodnota zahrnuje úspěšné i neúspěšné požadavky, jakož i požadavků, které vytváří chyby. Hodnota ResponseType dimenzi používejte pro počet jiný typ odpovědi.|ResponseType, GeoType, ApiName|
|Příchozí přenos dat|Příchozí přenos dat|B|Celkem|Množství dat příjem příchozích dat v bajtech. Tato hodnota zahrnuje příjem příchozích dat z externí klienta do Azure Storage, jakož i příjem příchozích dat v rámci Azure.|GeoType, ApiName|
|Výchozí přenos|Výchozí přenos|B|Celkem|Množství odchozí data v bajtech. Tato hodnota zahrnuje odchozí z externí klienta do Azure Storage a také odchozí v rámci Azure. Toto číslo v důsledku toho nemusí odpovídat fakturovatelný odchozí.|GeoType, ApiName|
|SuccessServerLatency|Latence serveru při úspěchu|Milisekundy|Průměr|Průměrná latence úložiště Azure používá ke zpracování v případě úspěšné žádosti v milisekundách. Tato hodnota nezahrnuje latenci sítě, zadaný v AverageE2ELatency.|GeoType, ApiName|
|SuccessE2ELatency|Celková latence při úspěchu|Milisekundy|Průměr|Průměrná latence začátku do konce úspěšné požadavky na služby úložiště nebo zadaná operace rozhraní API, v milisekundách. Tato hodnota zahrnuje požadovaná doba zpracování v rámci Azure Storage pro čtení požadavku, odeslání odpovědi a přijímat potvrzení odpovědi.|GeoType, ApiName|
|Dostupnost|Dostupnost|Procento|Průměr|Procento dostupnosti pro službu úložiště nebo zadaná operace rozhraní API. Dostupnost se vypočítává tak, že hodnota TotalBillableRequests dělení podle počtu příslušné požadavky, včetně těch, které vytváří neočekávaným chybám. Všechny neočekávaným chybám výsledkem je omezená dostupnost pro službu úložiště nebo zadaná operace rozhraní API.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|QueueCapacity|Kapacita služby Queue|B|Průměr|Velikost úložiště využitá službou Queue účtu úložiště v bajtech|Žádné dimenze|
|QueueCount|Počet front|Počet|Průměr|Počet front ve službě Queue účtu úložiště|Žádné dimenze|
|QueueMessageCount|Počet zpráv fronty|Počet|Průměr|Přibližný počet zpráv fronty ve službě Queue účtu úložiště|Žádné dimenze|
|Transakce|Transakce|Počet|Celkem|Počet požadavků na služby úložiště nebo zadaná operace rozhraní API. Tato hodnota zahrnuje úspěšné i neúspěšné požadavky, jakož i požadavků, které vytváří chyby. Hodnota ResponseType dimenzi používejte pro počet jiný typ odpovědi.|ResponseType, GeoType, ApiName|
|Příchozí přenos dat|Příchozí přenos dat|B|Celkem|Množství dat příjem příchozích dat v bajtech. Tato hodnota zahrnuje příjem příchozích dat z externí klienta do Azure Storage, jakož i příjem příchozích dat v rámci Azure.|GeoType, ApiName|
|Výchozí přenos|Výchozí přenos|B|Celkem|Množství odchozí data v bajtech. Tato hodnota zahrnuje odchozí z externí klienta do Azure Storage a také odchozí v rámci Azure. Toto číslo v důsledku toho nemusí odpovídat fakturovatelný odchozí.|GeoType, ApiName|
|SuccessServerLatency|Latence serveru při úspěchu|Milisekundy|Průměr|Průměrná latence úložiště Azure používá ke zpracování v případě úspěšné žádosti v milisekundách. Tato hodnota nezahrnuje latenci sítě, zadaný v AverageE2ELatency.|GeoType, ApiName|
|SuccessE2ELatency|Celková latence při úspěchu|Milisekundy|Průměr|Průměrná latence začátku do konce úspěšné požadavky na služby úložiště nebo zadaná operace rozhraní API, v milisekundách. Tato hodnota zahrnuje požadovaná doba zpracování v rámci Azure Storage pro čtení požadavku, odeslání odpovědi a přijímat potvrzení odpovědi.|GeoType, ApiName|
|Dostupnost|Dostupnost|Procento|Průměr|Procento dostupnosti pro službu úložiště nebo zadaná operace rozhraní API. Dostupnost se vypočítává tak, že hodnota TotalBillableRequests dělení podle počtu příslušné požadavky, včetně těch, které vytváří neočekávaným chybám. Všechny neočekávaným chybám výsledkem je omezená dostupnost pro službu úložiště nebo zadaná operace rozhraní API.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|FileCapacity|Kapacita služby File|B|Průměr|Velikost úložiště využitá službou File účtu úložiště v bajtech|Žádné dimenze|
|FileCount|Počet souborů|Počet|Průměr|Počet souborů ve službě File účtu úložiště|Žádné dimenze|
|FileShareCount|Počet sdílených složek služby File|Počet|Průměr|Počet sdílených složek ve službě File účtu úložiště|Žádné dimenze|
|Transakce|Transakce|Počet|Celkem|Počet požadavků na služby úložiště nebo zadaná operace rozhraní API. Tato hodnota zahrnuje úspěšné i neúspěšné požadavky, jakož i požadavků, které vytváří chyby. Hodnota ResponseType dimenzi používejte pro počet jiný typ odpovědi.|ResponseType, GeoType, ApiName|
|Příchozí přenos dat|Příchozí přenos dat|B|Celkem|Množství dat příjem příchozích dat v bajtech. Tato hodnota zahrnuje příjem příchozích dat z externí klienta do Azure Storage, jakož i příjem příchozích dat v rámci Azure.|GeoType, ApiName|
|Výchozí přenos|Výchozí přenos|B|Celkem|Množství odchozí data v bajtech. Tato hodnota zahrnuje odchozí z externí klienta do Azure Storage a také odchozí v rámci Azure. Toto číslo v důsledku toho nemusí odpovídat fakturovatelný odchozí.|GeoType, ApiName|
|SuccessServerLatency|Latence serveru při úspěchu|Milisekundy|Průměr|Průměrná latence úložiště Azure používá ke zpracování v případě úspěšné žádosti v milisekundách. Tato hodnota nezahrnuje latenci sítě, zadaný v AverageE2ELatency.|GeoType, ApiName|
|SuccessE2ELatency|Celková latence při úspěchu|Milisekundy|Průměr|Průměrná latence začátku do konce úspěšné požadavky na služby úložiště nebo zadaná operace rozhraní API, v milisekundách. Tato hodnota zahrnuje požadovaná doba zpracování v rámci Azure Storage pro čtení požadavku, odeslání odpovědi a přijímat potvrzení odpovědi.|GeoType, ApiName|
|Dostupnost|Dostupnost|Procento|Průměr|Procento dostupnosti pro službu úložiště nebo zadaná operace rozhraní API. Dostupnost se vypočítává tak, že hodnota TotalBillableRequests dělení podle počtu příslušné požadavky, včetně těch, které vytváří neočekávaným chybám. Všechny neočekávaným chybám výsledkem je omezená dostupnost pro službu úložiště nebo zadaná operace rozhraní API.|GeoType, ApiName|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|ResourceUtilization|% využití SU|Procento|Maximum|% využití SU|Žádné dimenze|
|InputEvents|Vstupní události|Počet|Celkem|Vstupní události|Žádné dimenze|
|InputEventBytes|Bajty vstupních událostí|B|Celkem|Bajty vstupních událostí|Žádné dimenze|
|LateInputEvents|Pozdní vstupní události|Počet|Celkem|Pozdní vstupní události|Žádné dimenze|
|OutputEvents|Výstupní události|Počet|Celkem|Výstupní události|Žádné dimenze|
|ConversionErrors|Chyby převodu dat|Počet|Celkem|Chyby převodu dat|Žádné dimenze|
|Chyby|Chyby za běhu|Počet|Celkem|Chyby za běhu|Žádné dimenze|
|DroppedOrAdjustedEvents|Události mimo pořadí|Počet|Celkem|Události mimo pořadí|Žádné dimenze|
|AMLCalloutRequests|Požadavky na funkce|Počet|Celkem|Požadavky na funkce|Žádné dimenze|
|AMLCalloutFailedRequests|Nezdařené požadavky na funkce|Počet|Celkem|Nezdařené požadavky na funkce|Žádné dimenze|
|AMLCalloutInputEvents|Události funkcí|Počet|Celkem|Události funkcí|Žádné dimenze|

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

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (funkce)

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|Dimenze|
|---|---|---|---|---|---|
|BytesReceived|Vstupní data|B|Celkem|Vstupní data|Instance|
|BytesSent|Výstupní data|B|Celkem|Výstupní data|Instance|
|Http5xx|Chyby serveru HTTP|Počet|Celkem|Chyby serveru HTTP|Instance|
|MemoryWorkingSet|Pracovní sada paměti|B|Průměr|Pracovní sada paměti|Instance|
|AverageMemoryWorkingSet|Průměrná pracovní sada paměti|B|Průměr|Průměrná pracovní sada paměti|Instance|
|FunctionExecutionUnits|Jednotky provádění funkcí|Počet|Celkem|Jednotky provádění funkcí|Instance|
|FunctionExecutionCount|Počet spuštění funkce|Počet|Celkem|Počet spuštění funkce|Instance|

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
|FunctionExecutionCount|Počet spuštění funkce|Počet|Celkem|Počet spuštění funkce|Instance|
|AppConnections|Připojení|Počet|Průměr|Připojení|Instance|

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
* [Přečtěte si informace o metriky v Azure monitorování](monitoring-overview-metrics.md)
* [Vytvářet upozornění na metriky](insights-receive-alert-notifications.md)
* [Export metriky pro úložiště, centra událostí nebo analýzy protokolů](monitoring-overview-of-diagnostic-logs.md)
