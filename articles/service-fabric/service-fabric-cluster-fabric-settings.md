---
title: "Změňte nastavení clusteru Azure Service Fabric | Microsoft Docs"
description: "Tento článek popisuje nastavení prostředků infrastruktury a infrastruktury upgradu zásady, které můžete přizpůsobit."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: 
ms.assetid: 7ced36bf-bd3f-474f-a03a-6ebdbc9677e2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: chackdan
ms.openlocfilehash: bfbfc5b46985d3413510ba69cbdfc688f46f0f1b
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="customize-service-fabric-cluster-settings-and-fabric-upgrade-policy"></a>Přizpůsobení nastavení clusteru Service Fabric a zásady upgradu prostředků infrastruktury
Tento dokument vysvětluje, jak přizpůsobit různá nastavení prostředků infrastruktury a infrastruktury upgradovat zásady pro váš cluster Service Fabric. Přizpůsobit pomocí [portál Azure](https://portal.azure.com) nebo pomocí šablony Azure Resource Manager.

> [!NOTE]
> Ne všechna nastavení jsou k dispozici na portálu. V případě, že níže uvedených nastavení není k dispozici prostřednictvím portálu přizpůsobte pomocí šablony Azure Resource Manager.
> 

## <a name="customize-cluster-settings-using-resource-manager-templates"></a>Přizpůsobení nastavení clusteru pomocí šablony Resource Manageru
Následující kroky ukazují, jak přidat nové nastavení *MaxDiskQuotaInMB* k *diagnostiky* části.

1. Přejděte na https://resources.azure.com
2. Přejděte k vašemu předplatnému rozšířením **odběry** -> **skupiny prostředků** -> **Microsoft.ServiceFabric**  ->   **\<Název clusteru >**
3. V pravém horním rohu, vyberte **pro čtení a zápis.**
4. Vyberte **upravit** a aktualizovat `fabricSettings` JSON element a přidejte nový element:

```
      {
        "name": "Diagnostics",
        "parameters": [
          {
            "name": "MaxDiskQuotaInMB",
            "value": "65536"
          }
        ]
      }
```

Následuje seznam infrastruktury nastavení, které můžete přizpůsobit, uspořádané podle části.

### <a name="section-name-diagnostics"></a>Název oddílu: diagnostiky
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| ConsumerInstances |Řetězec | Dynamická |Seznam instancí DCA příjemce. |
| ProducerInstances |Řetězec | Dynamická |Seznam instancí producent DCA. |
| AppEtwTraceDeletionAgeInDays |Int, výchozí hodnota je 3 | Dynamická |Počet dní, po kterých jsme odstranit staré ETL soubory obsahující trasování ETW aplikací. |
| AppDiagnosticStoreAccessRequiresImpersonation |BOOL, výchozí hodnota je true | Dynamická |Jestli je potřeba zosobnění, při přístupu k diagnostiky ukládá jménem aplikace. |
| MaxDiskQuotaInMB |Int, výchozí hodnota je 65536 | Dynamická |Kvóty disku v MB pro technologie Windows Fabric protokolových souborech. |
| DiskFullSafetySpaceInMB |Int, výchozí hodnota je 1 024 | Dynamická |Zbývající místo na disku v MB k ochraně před používá DCA. |
| ApplicationLogsFormatVersion |Int, výchozí hodnota je 0 | Dynamická |Verze pro aplikaci zaznamená formátu. Podporované hodnoty jsou 0 a 1. Verze 1 obsahuje více polí ze záznamu události trasování událostí pro Windows než verze 0. |
| ClusterId |Řetězec | Dynamická |Jedinečné id clusteru. Tím se vygeneruje, když je vytvořen cluster. |
| EnableTelemetry |BOOL, výchozí hodnota je true | Dynamická |To bude povolit nebo zakázat telemetrii. |
| EnableCircularTraceSession |BOOL, výchozí hodnota je false | Statická |Příznak určuje, zda by měl použít relace cyklické trasování. |

### <a name="section-name-traceetw"></a>Název oddílu: Trasování nebo trasování
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| Úroveň |Int, výchozí hodnota je 4 | Dynamická |Úroveň trasování etw může trvat hodnoty 1, 2, 3, 4. Podporovaná úroveň trasování je nutné zachovat na 4 |

### <a name="section-name-performancecounterlocalstore"></a>Název oddílu: PerformanceCounterLocalStore
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| Hodnotu IsEnabled |BOOL, výchozí hodnota je true | Dynamická |Příznak určuje, zda je povoleno sběru dat čítače výkonu v místním uzlu. |
| SamplingIntervalInSeconds |Int, výchozí hodnota je 60 | Dynamická |Interval vzorkování pro čítače výkonu shromažďovaných. |
| Čítače |Řetězec | Dynamická |Čárkami oddělený seznam čítačů výkonu ke shromažďování. |
| MaxCounterBinaryFileSizeInMB |Int, výchozí hodnota je 1 | Dynamická |Maximální velikost (v MB) pro každý soubor binární čítače výkonu. |
| NewCounterBinaryFileCreationIntervalInMinutes |Int, výchozí hodnota je 10 | Dynamická |Maximální interval (v sekundách), která po jejímž uplynutí je vytvořen nový soubor binární čítače výkonu. |

### <a name="section-name-setup"></a>Název oddílu: Instalační program
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| Proměnná FabricDataRoot |Řetězec | Není povoleno |Service Fabric data kořenový adresář. Výchozí pro Azure je d:\svcfab |
| Adresáři FabricLogRoot |Řetězec | Není povoleno |Služba fabric protokolu kořenový adresář. Toto je, kde jsou umístěny SF protokoly a trasování. |
| ServiceRunAsAccountName |Řetězec | Není povoleno |Název účtu, pod kterým chcete-li spustit služby fabric host. |
| SkipFirewallConfiguration |BOOL, výchozí hodnota je false | Není povoleno |Určuje, pokud nastavení brány firewall je nutné nastavit v systému, nebo ne. To platí jenom v případě, že používáte bránu windows firewall. Pokud používáte brány firewall třetích stran, pak musíte otevřít porty pro systém a aplikace na používání |
|NodesToBeRemoved|Výchozí hodnota je řetězec, ""| Dynamická |Uzly, které byste měli odebrat jako součást konfigurace upgradu. (Jenom pro samostatné nasazení)|
|ContainerNetworkSetup|BOOL, výchozí hodnotu FALSE| Statická |Určuje, zda nastavení sítě kontejneru.|
|ContainerNetworkName|řetězec, výchozí je L""| Statická |Název sítě, který se má použít při vytváření kontejneru sítě.|

### <a name="section-name-transactionalreplicator"></a>Název oddílu: TransactionalReplicator
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| MaxCopyQueueSize |Uint, výchozí hodnota je 16384 | Statická |Toto je maximální hodnota definuje počáteční velikost fronty, který udržuje operací replikace. Všimněte si, že musí být násobkem 2. Pokud během doby běhu zvětšuje, aby tato operace velikost fronty budou omezeny mezi primárním a sekundárním replikátory. |
| BatchAcknowledgementInterval | Čas v sekundách, výchozí hodnota je 0,015 | Statická | Zadejte časový interval v sekundách. Určuje množství času, kterou Replikátor čeká po přijetí operace před odesláním zpět potvrzení. Jiné operace přijata během této doby bude mít jejich potvrzení odeslána zpět v do jedné zprávy -> Redukční síťový provoz, ale potenciálně omezení propustnosti replikátoru. |
| Maxreplicationmessagesize. |Uint, výchozí hodnota je 52428800 | Statická | Maximální velikost zprávy operací replikace. Výchozí hodnota je 50MB. |
| ReplicatorAddress |řetězec, výchozí je "localhost:0" | Statická | Koncový bod v podobě řetězce-'IP: port, který je používán Replikátor Windows Fabric ke zřízení spojení s dalšími replikami k odesílání a přijímání operace. |
| InitialPrimaryReplicationQueueSize |Uint, výchozí hodnota je 64 | Statická |Tato hodnota určuje počáteční velikost fronty, který udržuje operací replikace na primárním. Všimněte si, že musí být násobkem 2.|
| MaxPrimaryReplicationQueueSize |Uint, výchozí hodnota je 8192 | Statická |Toto je maximální počet operací, které může existovat ve frontě primární replikace. Všimněte si, že musí být násobkem 2. |
| MaxPrimaryReplicationQueueMemorySize |Uint, výchozí hodnota je 0 | Statická |Toto je maximální hodnota primární replikace fronty v bajtech. |
| InitialSecondaryReplicationQueueSize |Uint, výchozí hodnota je 64 | Statická |Tato hodnota určuje počáteční velikost fronty, který udržuje operací replikace na sekundárním. Všimněte si, že musí být násobkem 2. |
| MaxSecondaryReplicationQueueSize |Uint, výchozí hodnota je 16384 | Statická |Toto je maximální počet operací, které může existovat ve frontě sekundární replikace. Všimněte si, že musí být násobkem 2. |
| MaxSecondaryReplicationQueueMemorySize |Uint, výchozí hodnota je 0 | Statická |Toto je maximální hodnota sekundární replikační fronty v bajtech. |
| SecondaryClearAcknowledgedOperations |BOOL, výchozí hodnota je false | Statická |Logická hodnota, která ovládá, zda operací na sekundární Replikátor jsou vymazány, jakmile jsou potvrzené na primární (vyprazdňuje na disk). Nastavení na hodnotu TRUE může mít za následek další disk čtení na nový primární při zachytávání repliky po převzetí služeb. |
| MaxMetadataSizeInKB |Int, výchozí hodnota je 4 |Není povoleno|Maximální velikost metadat datový proud protokolu. |
| MaxRecordSizeInKB |Uint, výchozí hodnota je 1 024 |Není povoleno| Maximální velikost datového proudu záznam protokolu. |
| CheckpointThresholdInMB |Int, výchozí hodnota je 50 |Statická|Kontrolní bod se zahájí, při použití protokolu překročí tuto hodnotu. |
| MaxAccumulatedBackupLogSizeInMB |Int, výchozí hodnota je 800 |Statická|Maximální počet shromážděných velikost (v MB) zálohování protokolů v řetězci dané zálohy protokolu. Požadavky přírůstkové zálohování se nezdaří, pokud Přírůstková záloha by vygeneroval zálohy protokolu, které by způsobily Akumulovaná protokoly zálohování od relevantní úplné zálohování musí být větší než tato velikost. V takových případech uživatele je potřeba provést úplnou zálohu. |
| MaxWriteQueueDepthInKB |Int, výchozí hodnota je 0 |Není povoleno| Int pro maximální zápis hloubky fronty, která základní protokolovacího nástroje můžete použít jako zadaný v kilobajtech pro protokol, který je přidružen této repliky. Tato hodnota je maximální počet bajtů, které se dají nezpracovaných během aktualizace základní protokolovacího nástroje. To může být 0 pro základní protokolovač k výpočtu odpovídající hodnotu nebo násobkem 4. |
| SharedLogId |Řetězec |Není povoleno|Identifikátor sdílené protokolu. Toto je identifikátor guid a musí být jedinečný pro každý sdílený protokolu. |
| SharedLogPath |Řetězec |Není povoleno|Cesta ke sdílené protokolu. Pokud tato hodnota je prázdná se používá protokol sdílené výchozí. |
| SlowApiMonitoringDuration |Čas v sekundách, výchozí hodnota je 300 |Statická| Předtím, než je aktivována událost stavu upozornění, zadejte dobu trvání pro rozhraní api.|
| MinLogSizeInMB |Int, výchozí hodnota je 0 |Statická|Minimální velikost transakčního protokolu. Protokol nebudou povolena, aby došlo ke zkrácení velikost pod toto nastavení. Hodnota 0 znamená, že Replikátor určí minimální velikost protokolu podle dalších nastavení. Zvýšení hodnoty zvyšuje pravděpodobnost plnění od pravděpodobnost příslušných protokolových záznamy, které ke zkrácení je snížena částečné kopie a přírůstkové zálohování. |

### <a name="section-name-fabricclient"></a>Název oddílu: FabricClient
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| NodeAddresses |Výchozí hodnota je řetězec, "" |Statická|Kolekce adres (připojovací řetězce) v různých uzlech, které lze použít ke komunikaci s služby DNS. Původně se klient připojí, zvolením některé z adresy náhodně. Pokud je zadán více než jeden připojovací řetězec a připojení se nezdaří z důvodu komunikaci nebo vypršení časového limitu; přepínače klienta, která se mají použít další adresa postupně. V tématu Naming adresu služby opakujte části Podrobnosti o opakování sémantiku. |
| ConnectionInitializationTimeout |Čas v sekundách, výchozí hodnota je 2 |Dynamická|Zadejte časový interval v sekundách. Interval vypršení časového limitu připojení pro každé připojení klienta se pokusí otevřít připojení k bráně. |
| Parametr PartitionLocationCacheLimit |Int, výchozí hodnota je 100000. |Statická|Počet oddílů uložená v mezipaměti pro řešení služby (nastavena na hodnotu 0 pro žádné omezení). |
| ServiceChangePollInterval |Čas v sekundách, výchozí hodnota je 120. |Dynamická|Zadejte časový interval v sekundách. Interval mezi po sobě jdoucích dotazování pro službu změní z klienta na bráně pro registrovanou službu změnu oznámení zpětných volání. |
| KeepAliveIntervalInSeconds |Int, výchozí hodnota je 20 |Statická|Interval, ve kterém přenos FabricClient odesílá zprávy keep-alive k bráně. Pro uživatele 0; keepAlive je zakázána. Musí být kladná hodnota. |
| HealthOperationTimeout |Čas v sekundách, výchozí hodnota je 120. |Dynamická|Zadejte časový interval v sekundách. Časový limit pro sestavu zprávy odeslané do Správce stavu. |
| HealthReportSendInterval |Čas v sekundách, výchozí hodnota je 30 |Dynamická|Zadejte časový interval v sekundách. Interval, ve kterém vytváření sestav, součást odešle sestavy Akumulovaná stavu do Správce stavu. |
| HealthReportRetrySendInterval |Čas v sekundách, výchozí hodnota je 30 |Dynamická|Zadejte časový interval v sekundách. Interval, ve kterém vytváření sestav, součást znovu odešle sestavy Akumulovaná stavu do Správce stavu. |
| RetryBackoffInterval |Čas v sekundách, výchozí hodnota je 3 |Dynamická|Zadejte časový interval v sekundách. Back – vypnuté interval před opakováním operace. |
| MaxFileSenderThreads |Uint, výchozí hodnota je 10 |Statická|Maximální počet souborů, které se přenáší paralelně. |

### <a name="section-name-common"></a>Název oddílu: běžné
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| PerfMonitorInterval |Čas v sekundách, výchozí hodnota je 1 |Dynamická|Zadejte časový interval v sekundách. Interval monitorování výkonu. Nastavení na hodnotu 0 nebo záporné zakáže monitorování. |

### <a name="section-name-healthmanager"></a>Název oddílu: HealthManager
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| EnableApplicationTypeHealthEvaluation |BOOL, výchozí hodnota je false |Statická|Cluster zásad vyhodnocení stavu: Povolit za vyhodnocení stavu typu aplikace. |

### <a name="section-name-nodedomainids"></a>Název oddílu: NodeDomainIds
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| UpgradeDomainId |Výchozí hodnota je řetězec, "" |Statická|Popisuje upgradovací doméně, ke které patří uzlu. |
| PropertyGroup – |NodeFaultDomainIdCollection |Statická|Popisuje domén selhání, ke které patří uzlu. Doména selhání je definována pomocí identifikátoru URI, který popisuje umístění uzlu v datovém centru.  Identifikátory URI domény selhání jsou ve formátu fd: / fd/následuje segment cesty identifikátoru URI.|

### <a name="section-name-nodeproperties"></a>Název oddílu: NodeProperties
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| PropertyGroup – |NodePropertyCollectionMap |Statická|Kolekce párů klíč hodnota řetězce pro vlastnosti uzlu. |

### <a name="section-name-nodecapacities"></a>Název oddílu: NodeCapacities
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| PropertyGroup – |NodeCapacityCollectionMap |Statická|Kolekce uzlu kapacity pro jiné metriky. |

### <a name="section-name-fabricnode"></a>Název oddílu: FabricNode
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| StateTraceInterval |Čas v sekundách, výchozí hodnota je 300 |Statická|Zadejte časový interval v sekundách. Interval pro stav uzlu na každém uzlu a až uzly na FM/FMM trasování. |
| StartApplicationPortRange |Int, výchozí hodnota je 0 |Statická|Spuštění aplikace porty spravuje hostování subsystému. Vyžaduje, pokud EndpointFilteringEnabled platí v Hosting. |
| EndApplicationPortRange |Int, výchozí hodnota je 0 |Statická|Konec (ne včetně) porty aplikaci spravuje hostování subsystému. Vyžaduje, pokud EndpointFilteringEnabled platí v Hosting. |
| ClusterX509StoreName |řetězec, výchozí hodnota je "Moje" |Dynamická|Název úložiště certifikátu X.509, který obsahuje certifikát clusteru pro zabezpečení komunikace mezi clustery. |
| ClusterX509FindType |řetězec, výchozí je "FindByThumbprint" |Dynamická|Určuje, jak hledat clusteru certifikátu v úložišti určeného ClusterX509StoreName podporované hodnoty: "FindByThumbprint"; "FindBySubjectName" s "FindBySubjectName"; Pokud existuje více shod; To se nejvzdálenější vypršení platnosti se používá. |
| ClusterX509FindValue |Výchozí hodnota je řetězec, "" |Dynamická|Hodnota filtru hledání používaná k nalezení clusteru certifikátu. |
| ClusterX509FindValueSecondary |Výchozí hodnota je řetězec, "" |Dynamická|Hodnota filtru hledání používaná k nalezení clusteru certifikátu. |
| ServerAuthX509StoreName |řetězec, výchozí hodnota je "Moje" |Dynamická|Název úložiště certifikátu X.509, který obsahuje certifikát serveru pro službu entrée. |
| ServerAuthX509FindType |řetězec, výchozí je "FindByThumbprint" |Dynamická|Určuje, jak hledat certifikátu serveru v úložišti určená hodnotou podporované ServerAuthX509StoreName: FindByThumbprint; FindBySubjectName. |
| ServerAuthX509FindValue |Výchozí hodnota je řetězec, "" |Dynamická|Hodnota filtru hledání používaná k nalezení certifikát serveru. |
| ServerAuthX509FindValueSecondary |Výchozí hodnota je řetězec, "" |Dynamická|Hodnota filtru hledání používaná k nalezení certifikát serveru. |
| ClientAuthX509StoreName |řetězec, výchozí hodnota je "Moje" |Dynamická|Název úložiště certifikátu X.509, který obsahuje certifikát pro roli správce výchozí FabricClient. |
| ClientAuthX509FindType |řetězec, výchozí je "FindByThumbprint" |Dynamická|Určuje, jak hledat certifikátu v úložišti určená hodnotou podporované ClientAuthX509StoreName: FindByThumbprint; FindBySubjectName. |
| ClientAuthX509FindValue |Výchozí hodnota je řetězec, "" | Dynamická|Hodnota filtru hledání používaná k nalezení certifikát pro roli správce výchozí FabricClient. |
| ClientAuthX509FindValueSecondary |Výchozí hodnota je řetězec, "" |Dynamická|Hodnota filtru hledání používaná k nalezení certifikát pro roli správce výchozí FabricClient. |
| UserRoleClientX509StoreName |řetězec, výchozí hodnota je "Moje" |Dynamická|Název úložiště certifikátu X.509, který obsahuje certifikát pro výchozí roli uživatele FabricClient. |
| UserRoleClientX509FindType |řetězec, výchozí je "FindByThumbprint" |Dynamická|Určuje, jak hledat certifikátu v úložišti určená hodnotou podporované UserRoleClientX509StoreName: FindByThumbprint; FindBySubjectName. |
| UserRoleClientX509FindValue |Výchozí hodnota je řetězec, "" |Dynamická|Hodnota filtru hledání používaná k nalezení certifikát pro výchozí roli uživatele FabricClient. |
| UserRoleClientX509FindValueSecondary |Výchozí hodnota je řetězec, "" |Dynamická|Hodnota filtru hledání používaná k nalezení certifikát pro výchozí roli uživatele FabricClient. |

### <a name="section-name-paas"></a>Název oddílu: Paas
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| ClusterId |Výchozí hodnota je řetězec, "" |Není povoleno|X509 certificate úložiště používané konvencemi prostředků infrastruktury pro ochranu konfigurace. |

### <a name="section-name-fabrichost"></a>Název oddílu: hostitele FabricHost
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| StopTimeout |Čas v sekundách, výchozí hodnota je 300 |Dynamická|Zadejte časový interval v sekundách. Časový limit pro hostovanou službu Aktivace; deaktivace a upgradu. |
| StartTimeout |Čas v sekundách, výchozí hodnota je 300 |Dynamická|Zadejte časový interval v sekundách. Vypršení časového limitu pro spuštění fabricactivationmanager. |
| ActivationRetryBackoffInterval |Čas v sekundách, výchozí hodnota je 5 |Dynamická|Zadejte časový interval v sekundách. Interval omezení rychlosti na každé selhání aktivace; V každé selhání průběžné aktivace systému bude opakovat akci během aktivace pro až MaxActivationFailureCount. Interval opakování na každé zkuste je produkt selhání průběžné aktivace a interval aktivace zpět – vypnuté. |
| ActivationMaxRetryInterval |Čas v sekundách, výchozí hodnota je 300 |Dynamická|Zadejte časový interval v sekundách. Maximální interval opakování pro aktivaci. Na každých průběžné selhání interval opakování vypočítá se jako minimální (ActivationMaxRetryInterval; Průběžné počet selhání * ActivationRetryBackoffInterval). |
| ActivationMaxFailureCount |Int, výchozí hodnota je 10 |Dynamická|Toto je maximální počet, pro který bude opakovat systému se nezdařila aktivace, než se předá. |
| EnableServiceFabricAutomaticUpdates |BOOL, výchozí hodnota je false |Dynamická|Toto je chcete povolit automatické aktualizace prostředků infrastruktury pomocí služby Windows Update. |
| EnableServiceFabricBaseUpgrade |BOOL, výchozí hodnota je false |Dynamická|To je umožnit základní aktualizace pro server. |
| EnableRestartManagement |BOOL, výchozí hodnota je false |Dynamická|To je umožnit restartování serveru. |


### <a name="section-name-failovermanager"></a>Název oddílu: FailoverManager
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| UserReplicaRestartWaitDuration |Čas v sekundách, výchozí hodnota je 60.0 * 30 |Dynamická|Zadejte časový interval v sekundách. Pokud repliku trvalou ocitne mimo provoz; Technologie Windows Fabric čeká na této hodnotě duration pro repliku opět online před vytvořením nové repliky nahrazení (které by vyžadovaly kopii stav). |
| QuorumLossWaitDuration |Čas v sekundách, výchozí hodnota je MaxValue |Dynamická|Zadejte časový interval v sekundách. Toto je maximální doba, pro které jsou povoleny oddíl ve stavu ztrátě kvora. Pokud oddílu je stále ve ztrátě kvora po této hodnotě duration; oddíl je obnovena před ztrátou kvora v úvahu dolů repliky jako ztracené. Všimněte si, že to může potenciálně dojít ke ztrátě dat. |
| UserStandByReplicaKeepDuration |Čas v sekundách, výchozí hodnota je 3600.0 * 24 * 7 |Dynamická|Zadejte časový interval v sekundách. Pokud repliku trvalou vraťte z dolní stav; může mít již bylo nahrazeno. Tento časovač Určuje, jak dlouho FM ponechá pohotovostní repliky před budou zrušeny. |
| UserMaxStandByReplicaCount |Int, výchozí hodnota je 1 |Dynamická|Výchozí maximální počet replik StandBy, které systém udržuje pro uživatele služby. |
| ExpectedClusterSize|Int, výchozí hodnota je 1|Dynamická|Při prvním spuštění clusteru FM vyčká pro tento velký počet uzlů na sestavu sami si před zahájením umístění jiných služeb; včetně služeb systému jako názvy.  Zvýšení hodnoty tuto zvyšuje doby trvání cluster spustil; ale zabrání časná uzly z přetížení měřidla a také další přesune, které bude nutné jako více uzlů do režimu online.  Tato hodnota by měla obecně nastavená na některé malá část počáteční velikost. |
|ClusterPauseThreshold|Int, výchozí hodnota je 1|Dynamická|Pokud počet uzlů v systému přejděte pod tuto hodnotu pak umístění; načíst vyrovnávání; a převzetí služeb při selhání se zastaví. |
|TargetReplicaSetSize|Int, výchozí hodnota je 7|Není povoleno|Toto je počet cíl FM replik, které se budou udržovat technologie Windows Fabric.  Větší číslo, které jsou výsledkem vyšší spolehlivosti dat FM; s kompromis výkonu. |
|MinReplicaSetSize|Int, výchozí hodnota je 3|Není povoleno|Toto je velikost minimální sady replik FM.  Pokud počet aktivní repliky FM klesne pod tuto hodnotu; FM odmítnou změny do clusteru, dokud alespoň minimální počet replik je obnovena. |
|ReplicaRestartWaitDuration|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(60.0 * 30)|Není povoleno|Zadejte časový interval v sekundách. Toto je ReplicaRestartWaitDuration pro FMService |
|StandByReplicaKeepDuration|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(3600.0 * 24 * 7)|Není povoleno|Zadejte časový interval v sekundách. Toto je StandByReplicaKeepDuration pro FMService |
|PlacementConstraints|řetězec, výchozí je L""|Není povoleno|Žádná omezení umístění pro repliky Správce převzetí služeb při selhání |
|ExpectedNodeFabricUpgradeDuration|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(60.0 * 30)|Dynamická|Zadejte časový interval v sekundách. Toto je očekávané trvání pro uzel k upgradu během upgradu technologie Windows Fabric. |
|ExpectedReplicaUpgradeDuration|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(60.0 * 30)|Dynamická|Zadejte časový interval v sekundách. Toto je očekávané trvání pro všechny repliky k upgradu na uzlu během upgradu aplikace. |
|ExpectedNodeDeactivationDuration|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(60.0 * 30)|Dynamická|Zadejte časový interval v sekundách. Toto je očekávané trvání pro uzel k dokončení deaktivace v. |
|IsSingletonReplicaMoveAllowedDuringUpgrade|má hodnotu TRUE, BOOL, výchozí|Dynamická|Pokud nastaven na hodnotu true; repliky s velikost cílové sady replik 1 bude moci přesunout během upgradu. |
|ReconfigurationTimeLimit|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(300)|Dynamická|Zadejte časový interval v sekundách. Časový limit pro Rekonfigurace; po kterém bude inicializován sestavy stavu upozornění |
|BuildReplicaTimeLimit|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(3600)|Dynamická|Zadejte časový interval v sekundách. Časový limit pro vytváření repliku stavová; po kterém bude inicializován sestavy stavu upozornění |
|CreateInstanceTimeLimit|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(300)|Dynamická|Zadejte časový interval v sekundách. Časový limit pro vytvoření bezstavové instance; po kterém bude inicializován sestavy stavu upozornění |
|PlacementTimeLimit|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(600)|Dynamická|Zadejte časový interval v sekundách. Časový limit pro dosažení počtu cílových replik; po kterém bude inicializován sestavy stavu upozornění |

### <a name="section-name-namingservice"></a>Název oddílu: NamingService
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| TargetReplicaSetSize |Int, výchozí hodnota je 7 |Není povoleno|Nastaví počet replik pro každý oddíl úložiště služby DNS. Zvýšením počtu sady replik zvyšuje úroveň spolehlivosti informace v úložišti služby pojmenování; snížení změny, které informace bude ztraceno v důsledku selhání uzlu; náklady na technologie Windows Fabric a množství času, zvýšené zatížení trvá provedení aktualizace na pojmenování data.|
|MinReplicaSetSize | Int, výchozí hodnota je 3 |Není povoleno| Minimální počet replik služby DNS, které jsou potřebné k zápisu do dokončení aktualizace. Pokud jsou repliky méně než tento v systému aktivní spolehlivost systému odmítne aktualizace do úložiště služby pojmenování až do obnovení repliky. Tato hodnota musí být nikdy větší než TargetReplicaSetSize. |
|ReplicaRestartWaitDuration | Čas v sekundách, výchozí hodnota je (60.0 * 30)|Není povoleno| Zadejte časový interval v sekundách. Když se replika služby DNS ocitne mimo provoz; Tento časovač se spustí.  Po jeho vypršení FM začnou nahradit replik, které jsou dolů (ho ještě nepovažuje jejich ztráty). |
|QuorumLossWaitDuration | Čas v sekundách, výchozí hodnota je MaxValue |Není povoleno| Zadejte časový interval v sekundách. Pokud získá služby DNS do ztrátě kvora; Tento časovač se spustí.  Po jeho vypršení FM zvažovat dolů repliky jako ztracené; a pokus o obnovení kvora. Ne to, jestli to může způsobit ztrátu dat. |
|StandByReplicaKeepDuration | Čas v sekundách, výchozí hodnota je 3600.0 * 2 |Není povoleno| Zadejte časový interval v sekundách. Pokud repliku služby DNS vraťte z dolní stav; může mít již bylo nahrazeno.  Tento časovač Určuje, jak dlouho FM ponechá pohotovostní repliky před budou zrušeny. |
|PlacementConstraints | Výchozí hodnota je řetězec, "" |Není povoleno| Omezení umístění pro služby DNS. |
|ServiceDescriptionCacheLimit | Int, výchozí hodnota je 0 |Statická| Maximální počet záznamů v mezipaměti popis služby hodnoty nejdelšího Nepoužití v úložišti služby DNS (nastavena na hodnotu 0 pro žádné omezení) udržuje. |
|RepairInterval | Čas v sekundách, výchozí hodnota je 5 |Statická| Zadejte časový interval v sekundách. Interval, ve kterém se spustí pojmenování opravit nekonzistence mezi autority vlastníka a název vlastníka. |
|MaxNamingServiceHealthReports | Int, výchozí hodnota je 10 |Dynamická|Maximální počet pomalé operací, které ukládají pojmenování služby najednou sestavy není v pořádku. Pokud 0; všechny operace pomalé se odesílají. |
| MaxMessageSize |Int, výchozí hodnota je 4\*1024\*1024 |Statická|Maximální velikost zprávy pro komunikaci klienta uzlu při použití názvy. Zmírnění útok DOS; Výchozí hodnota je 4 MB volného místa. |
| MaxFileOperationTimeout |Čas v sekundách, výchozí hodnota je 30 |Dynamická|Zadejte časový interval v sekundách. Maximální časový limit povolená pro operace služby úložiště souborů. Určení delší časový limit žádosti budou odmítnuty. |
| Jako MaxOperationTimeout |Čas v sekundách, výchozí hodnota je 600 |Dynamická|Zadejte časový interval v sekundách. Maximální časový limit povolená pro operace klienta. Určení delší časový limit žádosti budou odmítnuty. |
| MaxClientConnections |Int, výchozí hodnota je 1 000 |Dynamická|Maximální povolený počet připojení klientů k jednomu brány. |
| ServiceNotificationTimeout |Čas v sekundách, výchozí hodnota je 30 |Dynamická|Zadejte časový interval v sekundách. Časový limit používá při doručování oznámení služby klienta. |
| MaxOutstandingNotificationsPerClient |Int, výchozí hodnota je 1 000 |Dynamická|Maximální počet nezpracovaných oznámení před registrace klienta je vynuceně ukončeno brány. |
| MaxIndexedEmptyPartitions |Int, výchozí hodnota je 1 000 |Dynamická|Maximální počet prázdné oddíly, které zůstanou uloženy v mezipaměti oznámení pro synchronizaci opětovně se připojujícího klientů. Všechny prázdné oddíly výše toto číslo se odebere z indexu ve vzestupném pořadí verze vyhledávání. Připojení klienti stále synchronizovat a přijímat aktualizace zmeškaných prázdný oddílu; ale protokol synchronizace stane nákladnější. |
| GatewayServiceDescriptionCacheLimit |Int, výchozí hodnota je 0 |Statická|Maximální počet záznamů v mezipaměti popis služby hodnoty nejdelšího Nepoužití v bráně pojmenování (nastavena na hodnotu 0 pro žádné omezení) udržuje. |
| PartitionCount |Int, výchozí hodnota je 3 |Není povoleno|Počet oddílů služby DNS úložiště vytvořit. Každý oddíl vlastní klíč jeden oddíl, který odpovídá jeho index; Ano klíče oddílů [0; PartitionCount) neexistuje. Zvýšit počet oddílů zvyšuje služby DNS měřítka, kterému služby DNS můžete provádět v snížením průměrný objem data ukládaná společností všechny repliky základní nastavení; za cenu vyšší využití prostředků (od PartitionCount * ReplicaSetSize služby repliky musí být zachovaná).|

### <a name="section-name-runas"></a>Název oddílu: RunAs
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| RunAsAccountName |Výchozí hodnota je řetězec, "" |Dynamická|Určuje název účtu RunAs. To je potřeba jenom pro "Typu DomainUser" nebo "ManagedServiceAccount" účet typu. Platné hodnoty jsou "doména\uživatel" nebo "user@domain". |
|RunAsAccountType|Výchozí hodnota je řetězec, "" |Dynamická|Určuje typ účtu RunAs. To je potřeba pro všechny RunAs části platné hodnoty jsou "Typu DomainUser nebo NetworkService nebo ManagedServiceAccount nebo LocalSystem".|
|Spustit_jako_heslo|Výchozí hodnota je řetězec, "" |Dynamická|Určuje heslo k účtu RunAs. To je potřeba jenom pro typ účtu "Typu DomainUser". |

### <a name="section-name-runasfabric"></a>Název oddílu: RunAs_Fabric
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| RunAsAccountName |Výchozí hodnota je řetězec, "" |Dynamická|Určuje název účtu RunAs. To je potřeba jenom pro "Typu DomainUser" nebo "ManagedServiceAccount" účet typu. Platné hodnoty jsou "doména\uživatel" nebo "user@domain". |
|RunAsAccountType|Výchozí hodnota je řetězec, "" |Dynamická|Určuje typ účtu RunAs. To je potřeba pro všechny RunAs části platné hodnoty jsou "LocalUser/typu DomainUser nebo NetworkService nebo ManagedServiceAccount nebo LocalSystem". |
|Spustit_jako_heslo|Výchozí hodnota je řetězec, "" |Dynamická|Určuje heslo k účtu RunAs. To je potřeba jenom pro typ účtu "Typu DomainUser". |

### <a name="section-name-runashttpgateway"></a>Název oddílu: RunAs_HttpGateway
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| RunAsAccountName |Výchozí hodnota je řetězec, "" |Dynamická|Určuje název účtu RunAs. To je potřeba jenom pro "Typu DomainUser" nebo "ManagedServiceAccount" účet typu. Platné hodnoty jsou "doména\uživatel" nebo "user@domain". |
|RunAsAccountType|Výchozí hodnota je řetězec, "" |Dynamická|Určuje typ účtu RunAs. To je potřeba pro všechny RunAs části platné hodnoty jsou "LocalUser/typu DomainUser nebo NetworkService nebo ManagedServiceAccount nebo LocalSystem". |
|Spustit_jako_heslo|Výchozí hodnota je řetězec, "" |Dynamická|Určuje heslo k účtu RunAs. To je potřeba jenom pro typ účtu "Typu DomainUser". |

### <a name="section-name-runasdca"></a>Název oddílu: RunAs_DCA
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| RunAsAccountName |Výchozí hodnota je řetězec, "" |Dynamická|Určuje název účtu RunAs. To je potřeba jenom pro "Typu DomainUser" nebo "ManagedServiceAccount" účet typu. Platné hodnoty jsou "doména\uživatel" nebo "user@domain". |
|RunAsAccountType|Výchozí hodnota je řetězec, "" |Dynamická|Určuje typ účtu RunAs. To je potřeba pro všechny RunAs části platné hodnoty jsou "LocalUser/typu DomainUser nebo NetworkService nebo ManagedServiceAccount nebo LocalSystem". |
|Spustit_jako_heslo|Výchozí hodnota je řetězec, "" |Dynamická|Určuje heslo k účtu RunAs. To je potřeba jenom pro typ účtu "Typu DomainUser". |

### <a name="section-name-httpgateway"></a>Název oddílu: HttpGateway
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
|Hodnotu IsEnabled|BOOL, výchozí hodnota je false |Statická| Povolí nebo zakáže HttpGateway. HttpGateway ve výchozím nastavení vypnutá. |
|ActiveListeners |Uint, výchozí hodnota je 50 |Statická| Počet čtení odeslat do fronty serveru http. Tato volba určuje počet souběžných požadavků, které může obsloužit HttpGateway. |
|MaxEntityBodySize |Uint, výchozí hodnota je 4194304 |Dynamická|Poskytuje maximální velikost textu, které se dají očekávat z požadavku http. Výchozí hodnota je 4 MB volného místa. Httpgateway nepodaří žádost, pokud má text velikosti > tuto hodnotu. Velikost minimální čtení bloku je 4096 bajtů. Aby to musí být > = 4096. |
|HttpGatewayHealthReportSendInterval |Čas v sekundách, výchozí hodnota je 30 |Statická|Zadejte časový interval v sekundách. Interval, ve kterém bránu Http odesílá Akumulovaná stavu sestav pro správce stavu. |

### <a name="section-name-ktllogger"></a>Název oddílu: KtlLogger
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
|AutomaticMemoryConfiguration |Int, výchozí hodnota je 1 |Dynamická|Příznak, který indikuje, pokud nastavení paměti by měl být automaticky a dynamicky nakonfigurovaný. Pokud nula a nastavení konfigurace paměti se používají přímo a se nezmění na základě systému podmínek. Pokud jeden pak nastavení paměti jsou konfigurovány automaticky a může se změnit na základě systému podmínek. |
|WriteBufferMemoryPoolMinimumInKB |Int, výchozí hodnota je 8388608 |Dynamická|Počet KB původně přidělit pro fond zápis vyrovnávací paměti. Použijte hodnotu 0 k označení omezení není nastaveno výchozí by měla být v souladu s SharedLogSizeInMB níže. |
|WriteBufferMemoryPoolMaximumInKB | Int, výchozí hodnota je 0 |Dynamická|Počet KB až povolit růst až fondu zápis vyrovnávací paměti. 0 slouží k označení, není nijak omezena. |
|MaximumDestagingWriteOutstandingInKB | Int, výchozí hodnota je 0 |Dynamická|Počet KB povolit protokol sdílené posunut před vyhrazené protokolu. 0 slouží k označení, není nijak omezena.
|SharedLogPath |Výchozí hodnota je řetězec, "" |Statická|Cesta a název souboru do umístění pro sdílené protokolu kontejneru. Použití "" pro použití výchozí cestu v kořenovém adresáři data prostředků infrastruktury. |
|SharedLogId |Výchozí hodnota je řetězec, "" |Statická|Jedinečný identifikátor guid pro kontejner sdílené protokolu. Použití "" Pokud se používá výchozí cestu v kořenovém adresáři data prostředků infrastruktury. |
|SharedLogSizeInMB |Int, výchozí hodnota je 8192 |Statická|Počet MB k přidělení v kontejneru sdílené protokolu. |

### <a name="section-name-applicationgatewayhttp"></a>Název oddílu: ApplicationGateway/Http
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
|Hodnotu IsEnabled |BOOL, výchozí hodnota je false |Statická| Povolí nebo zakáže HttpApplicationGateway. Ve výchozím nastavení vypnutá HttpApplicationGateway a tato konfigurace je třeba nastavit povolit. |
|NumberOfParallelOperations | Uint, výchozí hodnota je 5000 |Statická|Počet čtení odeslat do fronty serveru http. Tato volba určuje počet souběžných požadavků, které může obsloužit HttpGateway. |
|DefaultHttpRequestTimeout |Čas v sekundách. Výchozí hodnota je 120. |Dynamická|Zadejte časový interval v sekundách.  Poskytuje výchozí časový limit žádosti pro požadavky http zpracovávána v bráně aplikace http. |
|ResolveServiceBackoffInterval |Čas v sekundách, výchozí hodnota je 5 |Dynamická|Zadejte časový interval v sekundách.  Poskytuje řešení výchozí back vypnout interval před opakovaným pokusem o nezdařené operace služby. |
|BodyChunkSize |Uint, výchozí hodnota je 16384 |Dynamická| Dává velikost bloku dat v bajtech použít ke čtení textu. |
|GatewayAuthCredentialType |řetězec, výchozí je "Žádný" |Statická| Určuje typ pověření zabezpečení pro použití v http aplikace brána koncový bod platné hodnoty jsou "žádné / X 509. |
|GatewayX509CertificateStoreName |řetězec, výchozí hodnota je "Moje" |Dynamická| Název úložiště certifikátu X.509, který obsahuje certifikát pro bránu aplikace http. |
|GatewayX509CertificateFindType |řetězec, výchozí je "FindByThumbprint" |Dynamická| Určuje, jak hledat certifikátu v úložišti určená hodnotou podporované GatewayX509CertificateStoreName: FindByThumbprint; FindBySubjectName. |
|GatewayX509CertificateFindValue | Výchozí hodnota je řetězec, "" |Dynamická| Hodnota filtru hledání používaná k nalezení certifikát brány aplikace http. Tento certifikát je nakonfigurovaná na koncový bod https a můžete také použít k ověření identity aplikace v případě potřeby službami. FindValue se hledá první; a pokud neexistuje; FindValueSecondary vyhledávat. |
|GatewayX509CertificateFindValueSecondary | Výchozí hodnota je řetězec, "" |Dynamická|Hodnota filtru hledání používaná k nalezení certifikát brány aplikace http. Tento certifikát je nakonfigurovaná na koncový bod https a můžete také použít k ověření identity aplikace v případě potřeby službami. FindValue se hledá první; a pokud neexistuje; FindValueSecondary vyhledávat.|
|HttpRequestConnectTimeout|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(5)|Dynamická|Zadejte časový interval v sekundách.  Poskytuje časový limit připojení pro požadavky http odeslané z brány aplikace http.  |
|RemoveServiceResponseHeaders|řetězec, výchozí je L "datum; Server"|Statická|Částečně dvojtečkou / čárkami oddělený seznam hlaviček odpovědi, které se odeberou z odpověď služby; Před předáním do klienta. Pokud je nastavená na prázdný řetězec; předat všechny hlavičky vrácený službou jako-je. jednofaktorovému Nepřepisovat datum a serveru |
|ApplicationCertificateValidationPolicy|řetězec, výchozí je L "Žádný"|Statická| ApplicationCertificateValidationPolicy: None: nelze ověřit certifikát serveru; úspěšné žádosti. ServiceCertificateThumbprints: Naleznete konfigurace ServiceCertificateThumbprints pro čárkami oddělený seznam kryptografické otisky vzdálené certifikátů, které můžete důvěřovat reverzní proxy server. ServiceCommonNameAndIssuer: Naleznete konfigurace ServiceCommonNameAndIssuer pro předmět názvu a vystavitele kryptografický otisk vzdálené certifikátů, které můžete důvěřovat reverzní proxy server. |
|ServiceCertificateThumbprints|řetězec, výchozí je L""|Dynamická| |
|CrlCheckingFlag|uint, výchozí je 0x40000000 |Dynamická| Příznaky pro ověřování řetězu certifikátů služba nebo aplikace; například kontroly seznamu odvolaných certifikátů 0x10000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT CERT_CHAIN_REVOCATION_CHECK_CHAIN 0x40000000 CERT_CHAIN_REVOCATION_CHECK_END_CERT 0x20000000 0x80000000 CERT_CHAIN_REVOCATION_CHECK_CACHE_ONLY nastavení na hodnotu 0 Zakáže CRL kontrola úplný seznam podporovaných hodnot je zdokumentovat, dwFlags CertGetCertificateChain: http://msdn.microsoft.com/library/windows/desktop/aa376078 (v=vs.85).aspx  |
|IgnoreCrlOfflineError|má hodnotu TRUE, BOOL, výchozí|Dynamická|Jestli se má ignorovat offline chyba seznamu odvolaných certifikátů pro ověření certifikátu aplikace nebo služby. |
|SecureOnlyMode|BOOL, výchozí hodnotu FALSE|Dynamická| SecureOnlyMode: true: reverzní proxy server jenom předá do služby, které publikovat zabezpečené koncové body. FALSE: reverzní proxy server může předávat požadavky na zabezpečení nebo nezabezpečené koncové body.  |
|ForwardClientCertificate|BOOL, výchozí hodnotu FALSE|Dynamická| |

### <a name="section-name-applicationgatewayhttpservicecommonnameandissuer"></a>Název oddílu: ApplicationGateway/Http/ServiceCommonNameAndIssuer
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
|PropertyGroup –|X509NameMap, výchozí hodnota je žádné|Dynamická|  |

### <a name="section-name-management"></a>Název oddílu: správy
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| Parametr ImageStoreConnectionString |SecureString |Statická|Připojovací řetězec do kořenového adresáře pro úložiště bitových kopií. |
| ImageStoreMinimumTransferBPS | Int, výchozí hodnota je 1 024 |Dynamická|Minimální přenosová rychlost mezi clusterem a úložiště bitových kopií. Tato hodnota se používá k určení časový limit při přístupu k externí úložiště bitových kopií. Tuto hodnotu změňte, pouze v případě, že latence mezi clusterem a úložiště bitových kopií je vysoká času pro cluster můžete stáhnout z externího úložiště bitových kopií. |
|AzureStorageMaxWorkerThreads | Int, výchozí hodnota je 25 |Dynamická|Maximální počet pracovních vláken současně. |
|AzureStorageMaxConnections | Int, výchozí hodnota je 5000 |Dynamická|Maximální počet souběžných připojení k úložišti azure. |
|AzureStorageOperationTimeout | Čas v sekundách, výchozí hodnota je 6000 |Dynamická|Zadejte časový interval v sekundách. Vypršení časového limitu pro xstore na dokončení operace. |
|ImageCachingEnabled | BOOL, výchozí hodnota je true |Statická|Tato konfigurace umožňuje povolit nebo zakázat ukládání do mezipaměti. |
|DisableChecksumValidation | BOOL, výchozí hodnota je false |Statická| Tato konfigurace umožňuje povolit nebo zakázat ověření kontrolního součtu při zřizování aplikace. |
|DisableServerSideCopy | BOOL, výchozí hodnota je false |Statická|Tato konfigurace povolí nebo zakáže serverové kopii balíčku aplikace v úložišti ImageStore při zřizování aplikace. |

### <a name="section-name-healthmanagerclusterhealthpolicy"></a>Název oddílu: HealthManager/ClusterHealthPolicy
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| ConsiderWarningAsError |BOOL, výchozí hodnota je false |Statická|Cluster zásad vyhodnocení stavu: upozornění jsou považovány za chyby. |
| MaxPercentUnhealthyNodes | Int, výchozí hodnota je 0 |Statická|Cluster zásad vyhodnocení stavu: povolená maximální procento není v pořádku uzly clusteru jako v pořádku. |
| MaxPercentUnhealthyApplications | Int, výchozí hodnota je 0 |Statická|Cluster zásad vyhodnocení stavu: maximální procento aplikace není v pořádku, povolen pro cluster jako v pořádku. |

### <a name="section-name-healthmanagerclusterupgradehealthpolicy"></a>Název oddílu: HealthManager/ClusterUpgradeHealthPolicy
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
|MaxPercentDeltaUnhealthyNodes|Int, výchozí hodnota je 10|Statická|Cluster zásad vyhodnocení stavu upgradu: povolená maximální procento rozdílů není v pořádku uzly clusteru jako v pořádku |
|MaxPercentUpgradeDomainDeltaUnhealthyNodes|Int, výchozí hodnota je 15|Statická|Cluster zásad vyhodnocení stavu upgradu: maximální procento rozdílů není v pořádku uzlů v doméně upgradu povolené pro cluster jako v pořádku |

### <a name="section-name-faultanalysisservice"></a>Název oddílu: FaultAnalysisService
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| TargetReplicaSetSize |Int, výchozí hodnota je 0 |Není povoleno|NOT_PLATFORM_UNIX_START TargetReplicaSetSize pro FaultAnalysisService. |
| MinReplicaSetSize |Int, výchozí hodnota je 0 |Není povoleno|MinReplicaSetSize pro FaultAnalysisService. |
| ReplicaRestartWaitDuration |Čas v sekundách, výchozí hodnota je 60 minut|Statická|Zadejte časový interval v sekundách. ReplicaRestartWaitDuration pro FaultAnalysisService. |
| QuorumLossWaitDuration | Čas v sekundách, výchozí hodnota je MaxValue |Statická|Zadejte časový interval v sekundách. QuorumLossWaitDuration pro FaultAnalysisService. |
| StandByReplicaKeepDuration| Čas v sekundách, výchozí hodnota je (60*24*7) minut |Statická|Zadejte časový interval v sekundách. StandByReplicaKeepDuration pro FaultAnalysisService. |
| PlacementConstraints | Výchozí hodnota je řetězec, ""|Statická| PlacementConstraints pro FaultAnalysisService. |
| StoredActionCleanupIntervalInSeconds | Int, výchozí hodnota je 3600 |Statická|Toto je, jak často se vyčistí úložišti.  Pouze akce ve stavu terminálu; a která alespoň dokončena CompletedActionKeepDurationInSeconds před bude odebrán. |
| CompletedActionKeepDurationInSeconds | Int, výchozí hodnota je 604800 |Statická| Toto je přibližně jak dlouho chcete ponechat akce, které jsou ve stavu terminálu.  To také závisí na StoredActionCleanupIntervalInSeconds; vzhledem k tomu, že práce, kterou čištění se provádí pouze v tomto intervalu. 604800 je 7 dní. |
| StoredChaosEventCleanupIntervalInSeconds | Int, výchozí hodnota je 3600 |Statická|Toto je, jak často auditování úložiště pro čištění; Pokud počet událostí je více než 30000; vyčištění bude nové. |
|DataLossCheckWaitDurationInSeconds|Int, výchozí hodnota je 25|Statická|Celkové množství času; v sekundách; bude systém čekat ztráty dat provést.  Používá se interně při volání rozhraní api StartPartitionDataLossAsync(). |
|DataLossCheckPollIntervalInSeconds|int, výchozí je 5|Statická|Toto je čas mezi kontroly, které provede systém čekání ztráty dat provést.  Počet opakovaných číslo ztrátě dat se ověří na interní iterace je DataLossCheckWaitDurationInSeconds, tento. |
|ReplicaDropWaitDurationInSeconds|int, výchozí je 600|Statická|Tento parametr je použit při ztrátě dat volání rozhraní api.  Určuje, jak dlouho bude systém čekat repliku ke získání vyřazen po odebrání repliky je interně volá na něm. |

### <a name="section-name-filestoreservice"></a>Název oddílu: FileStoreService
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| NamingOperationTimeout |Čas v sekundách, výchozí hodnota je 60 |Dynamická|Zadejte časový interval v sekundách. Časový limit pro provádění pojmenování operace. |
| QueryOperationTimeout | Čas v sekundách, výchozí hodnota je 60 |Dynamická|Zadejte časový interval v sekundách. K provedení operace dotazu vypršel časový limit. |
| MaxCopyOperationThreads | Uint, výchozí hodnota je 0 |Dynamická| Maximální počet paralelní souborů to sekundární můžete zkopírovat z primární. '0' == počet jader. |
| MaxFileOperationThreads | Uint, výchozí hodnota je 100 |Statická| Maximální počet paralelních vláken provést FileOperations (kopírování nebo přesunutí) v primární. '0' == počet jader. |
| MaxStoreOperations | Uint, výchozí hodnota je 4096. |Statická|Maximální počet operací transakce paralelní úložiště povolené na primárním prostředku. '0' == počet jader. |
| MaxRequestProcessingThreads | Uint, výchozí hodnota je 200 |Statická|Maximální počet paralelních vláken povolen pro zpracování požadavků v primární. '0' == počet jader. |
| MaxSecondaryFileCopyFailureThreshold | Uint, výchozí hodnota je 25|Dynamická|Maximální počet opakování kopírování souborů na sekundárním, než se předá. |
| AnonymousAccessEnabled | BOOL, výchozí hodnota je true |Statická|Povolí nebo zakáže anonymní přístup ke sdíleným složkám FileStoreService. |
| PrimaryAccountType | Výchozí hodnota je řetězec, "" |Statická|Primární AccountType objektu do řízení přístupu FileStoreService sdílí. |
| PrimaryAccountUserName | Výchozí hodnota je řetězec, "" |Statická|Primární účet uživatelské jméno objektu do řízení přístupu FileStoreService sdílí. |
| PrimaryAccountUserPassword | SecureString, výchozí hodnota je prázdná |Statická|Heslo k účtu primární objektu do řízení přístupu FileStoreService sdílí. |
| PrimaryAccountNTLMPasswordSecret | SecureString, výchozí hodnota je prázdná |Statická| Tajný klíč heslo, které používá jako počáteční hodnoty pro generovat stejné heslo, je-li použít ověřování NTLM. |
| PrimaryAccountNTLMX509StoreLocation | řetězec, výchozí je "LocalMachine"|Statická| Umístění úložiště X509 certifikát používaný k vygenerování HMAC na PrimaryAccountNTLMPasswordSecret při použití ověřování protokolem NTLM. |
| PrimaryAccountNTLMX509StoreName | řetězec, výchozí je "MY"|Statická| Název úložiště X509 certifikát používaný k vygenerování HMAC na PrimaryAccountNTLMPasswordSecret při použití ověřování protokolem NTLM. |
| PrimaryAccountNTLMX509Thumbprint | Výchozí hodnota je řetězec, ""|Statická|Kryptografický otisk X509 certifikát používaný k vygenerování HMAC na PrimaryAccountNTLMPasswordSecret při použití ověřování protokolem NTLM. |
| SecondaryAccountType | Výchozí hodnota je řetězec, ""|Statická| Sekundární AccountType objektu do řízení přístupu FileStoreService sdílí. |
| SecondaryAccountUserName | Výchozí hodnota je řetězec, ""| Statická|Sekundární účet uživatelské jméno objektu do řízení přístupu FileStoreService sdílí. |
| SecondaryAccountUserPassword | SecureString, výchozí hodnota je prázdná |Statická|Heslo k účtu sekundárního objektu do řízení přístupu FileStoreService sdílí.  |
| SecondaryAccountNTLMPasswordSecret | SecureString, výchozí hodnota je prázdná |Statická| Tajný klíč heslo, které používá jako počáteční hodnoty pro generovat stejné heslo, je-li použít ověřování NTLM. |
| SecondaryAccountNTLMX509StoreLocation | řetězec, výchozí je "LocalMachine" |Statická|Umístění úložiště X509 certifikát používaný k vygenerování HMAC na SecondaryAccountNTLMPasswordSecret při použití ověřování protokolem NTLM. |
| SecondaryAccountNTLMX509StoreName | řetězec, výchozí je "MY" |Statická|Název úložiště X509 certifikát používaný k vygenerování HMAC na SecondaryAccountNTLMPasswordSecret při použití ověřování protokolem NTLM. |
| SecondaryAccountNTLMX509Thumbprint | Výchozí hodnota je řetězec, ""| Statická|Kryptografický otisk X509 certifikát používaný k vygenerování HMAC na SecondaryAccountNTLMPasswordSecret při použití ověřování protokolem NTLM. |
|CommonNameNtlmPasswordSecret|SecureString, výchozí hodnota je Common::SecureString(L"")| Statická|Tajný klíč heslo, které používá jako počáteční generovaný stejné heslo při použití ověřování protokolem NTLM |
|CommonName1Ntlmx509StoreLocation|řetězec, výchozí je L "LocalMachine"|Statická|Umístění úložiště X509 certifikát používaný k vygenerování HMAC na CommonName1NtlmPasswordSecret při použití ověřování protokolem NTLM |
|CommonName1Ntlmx509StoreName|řetězec, výchozí je L "MY"| Statická|Název úložiště X509 certifikát používaný k vygenerování HMAC na CommonName1NtlmPasswordSecret při použití ověřování protokolem NTLM |
|CommonName1Ntlmx509CommonName|řetězec, výchozí je L""|Statická| Běžný název X509 certifikát používaný k vygenerování HMAC na CommonName1NtlmPasswordSecret při použití ověřování protokolem NTLM |
|CommonName2Ntlmx509StoreLocation|řetězec, výchozí je L "LocalMachine"| Statická|Umístění úložiště X509 certifikát používaný k vygenerování HMAC na CommonName2NtlmPasswordSecret při použití ověřování protokolem NTLM |
|CommonName2Ntlmx509StoreName|řetězec, výchozí je L "MY"|Statická| Název úložiště X509 certifikát používaný k vygenerování HMAC na CommonName2NtlmPasswordSecret při použití ověřování protokolem NTLM |
|CommonName2Ntlmx509CommonName|řetězec, výchozí je L""|Statická|Běžný název X509 certifikát používaný k vygenerování HMAC na CommonName2NtlmPasswordSecret při použití ověřování protokolem NTLM |

### <a name="section-name-imagestoreservice"></a>Název oddílu: ImageStoreService
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| Povoleno |BOOL, výchozí hodnota je false |Statická|Příznak povoleno ImageStoreService. Výchozí: false |
| TargetReplicaSetSize | Int, výchozí hodnota je 7 |Není povoleno|TargetReplicaSetSize pro ImageStoreService. |
| MinReplicaSetSize | Int, výchozí hodnota je 3 |Není povoleno|MinReplicaSetSize pro ImageStoreService. |
| ReplicaRestartWaitDuration | Čas v sekundách, výchozí hodnota je 60.0 * 30 |Statická|Zadejte časový interval v sekundách. ReplicaRestartWaitDuration pro ImageStoreService. |
| QuorumLossWaitDuration | Čas v sekundách, výchozí hodnota je MaxValue |Statická| Zadejte časový interval v sekundách. QuorumLossWaitDuration pro ImageStoreService. |
| StandByReplicaKeepDuration | Čas v sekundách, výchozí hodnota je 3600.0 * 2 |Statická| Zadejte časový interval v sekundách. StandByReplicaKeepDuration pro ImageStoreService. |
| PlacementConstraints | Výchozí hodnota je řetězec, "" |Statická| PlacementConstraints pro ImageStoreService. |

### <a name="section-name-imagestoreclient"></a>Název oddílu: ImageStoreClient
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| ClientUploadTimeout |Čas v sekundách, výchozí hodnota je 1 800 |Dynamická|Zadejte časový interval v sekundách. Hodnota časového limitu pro nahrávání nejvyšší úrovně požadavek na služby úložiště bitové kopie. |
| ClientCopyTimeout | Čas v sekundách, výchozí hodnota je 1 800 |Dynamická| Zadejte časový interval v sekundách. Hodnota časového limitu pro požadavek nejvyšší úrovně kopie na služby úložiště bitové kopie. |
|ClientDownloadTimeout | Čas v sekundách, výchozí hodnota je 1 800 |Dynamická| Zadejte časový interval v sekundách. Hodnota časového limitu pro stahování nejvyšší úrovně požadavek na služby úložiště bitové kopie. |
|ClientListTimeout | Čas v sekundách, výchozí hodnota je 600 |Dynamická|Zadejte časový interval v sekundách. Hodnota časového limitu pro nejvyšší úrovně seznamu žádost o služby úložiště bitové kopie. |
|ClientDefaultTimeout | Čas v sekundách, výchozí hodnota je 180 |Dynamická| Zadejte časový interval v sekundách. Hodnota časového limitu pro všechny požadavky bez odeslání nebo stažení (např. existuje, odstraňte) službě úložiště bitové kopie. |

### <a name="section-name-tokenvalidationservice"></a>Název oddílu: TokenValidationService
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| Poskytovatelé |řetězec, výchozí je "Službu DSTS." |Statická|Čárkami oddělený seznam zprostředkovatelů ověření tokenu povolit (platný poskytovatelé jsou: DSTS; AAD). Aktuálně lze kdykoli povolit pouze jednoho zprostředkovatele. |

### <a name="section-name-upgradeorchestrationservice"></a>Název oddílu: UpgradeOrchestrationService
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| TargetReplicaSetSize |Int, výchozí hodnota je 0 |Není povoleno|TargetReplicaSetSize pro UpgradeOrchestrationService. |
| MinReplicaSetSize |Int, výchozí hodnota je 0 |Není povoleno|MinReplicaSetSize pro UpgradeOrchestrationService.
| ReplicaRestartWaitDuration | Čas v sekundách, výchozí hodnota je 60 minut|Statická| Zadejte časový interval v sekundách. ReplicaRestartWaitDuration pro UpgradeOrchestrationService. |
| QuorumLossWaitDuration | Čas v sekundách, výchozí hodnota je MaxValue |Statická| Zadejte časový interval v sekundách. QuorumLossWaitDuration pro UpgradeOrchestrationService. |
| StandByReplicaKeepDuration | Čas v sekundách, výchozí hodnota je 60*24*7 minut |Statická| Zadejte časový interval v sekundách. StandByReplicaKeepDuration pro UpgradeOrchestrationService. |
| PlacementConstraints | Výchozí hodnota je řetězec, "" |Statická| PlacementConstraints pro UpgradeOrchestrationService. |
| Autoupgradeenabled – | BOOL, výchozí hodnota je true |Statická| Automatické dotazování a upgradu akce v závislosti na soubor stavu cíle. |
| UpgradeApprovalRequired | BOOL, výchozí hodnota je false | Statická|Nastavení, aby kód upgradu vyžadovat schválení správce, než budete pokračovat. |

### <a name="section-name-upgradeservice"></a>Název oddílu: UpgradeService
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| PlacementConstraints |Výchozí hodnota je řetězec, "" |Není povoleno|PlacementConstraints upgradu služby. |
| TargetReplicaSetSize | Int, výchozí hodnota je 3 |Není povoleno| TargetReplicaSetSize pro UpgradeService. |
| MinReplicaSetSize | Int, výchozí hodnota je 2 |Není povoleno| MinReplicaSetSize pro UpgradeService. |
| CoordinatorType | řetězec, výchozí je "WUTest"|Není povoleno|CoordinatorType pro UpgradeService. |
| BaseUrl | Výchozí hodnota je řetězec, "" |Statická|BaseUrl pro UpgradeService. |
| ClusterId | Výchozí hodnota je řetězec, "" |Statická|ClusterId pro UpgradeService. |
| X509StoreName | řetězec, výchozí hodnota je "Moje"|Dynamická|X509StoreName pro UpgradeService. |
| X509StoreLocation | Výchozí hodnota je řetězec, "" |Dynamická| X509StoreLocation pro UpgradeService. |
| X509FindType | Výchozí hodnota je řetězec, ""|Dynamická| X509FindType pro UpgradeService. |
| X509FindValue | Výchozí hodnota je řetězec, "" |Dynamická| X509FindValue pro UpgradeService. |
| X509SecondaryFindValue | Výchozí hodnota je řetězec, "" |Dynamická| X509SecondaryFindValue pro UpgradeService. |
| OnlyBaseUpgrade | BOOL, výchozí hodnota je false |Dynamická|OnlyBaseUpgrade pro UpgradeService. |
| TestCabFolder | Výchozí hodnota je řetězec, "" |Statická| TestCabFolder pro UpgradeService. |

### <a name="section-name-security"></a>Název oddílu: zabezpečení
| **Parametr** | **Povolené hodnoty** |**Zásady upgradu**| **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
|ClusterCredentialType|řetězec, výchozí je L "Žádný"|Není povoleno|Určuje typ pověření zabezpečení pro použití při zabezpečování clusteru. Platné hodnoty jsou "None, X509, Windows" |
|ServerAuthCredentialType|řetězec, výchozí je L "Žádný"|Statická|Určuje typ pověření zabezpečení má použít k zabezpečení komunikace mezi FabricClient a Cluster. Platné hodnoty jsou "None, X509, Windows" |
|ClientRoleEnabled|BOOL, výchozí hodnotu FALSE|Statická|Indikuje, jestli je povolené role klientské; Pokud nastavíte na hodnotu true; Klienti jsou přiřazené role podle jejich identity. V2; povolení to znamená, že klient není v AdminClientCommonNames/AdminClientIdentities může být vykonávána pouze operace jen pro čtení. |
|ClusterCertThumbprints|řetězec, výchozí je L""|Dynamická|Kryptografické otisky certifikátů, které jsou povolené pro připojení ke clusteru; seznam názvů oddělených čárkami. |
|ServerCertThumbprints|řetězec, výchozí je L""|Dynamická|Kryptografické otisky certifikátů serveru použít clusteru, aby komunikoval s klientů. používají klienti pro ověření clusteru. Je seznam názvů oddělených čárkami. |
|ClientCertThumbprints|řetězec, výchozí je L""|Dynamická|Kryptografické otisky certifikátů používaných klienty, aby komunikoval s clusteru; cluster používá to autorizovat příchozích připojení. Je seznam názvů oddělených čárkami. |
|AdminClientCertThumbprints|řetězec, výchozí je L""|Dynamická|Kryptografické otisky certifikátů, které používají klienti v roli správce. Je seznam názvů oddělených čárkami. |
|CrlCheckingFlag|uint, výchozí je 0x40000000|Dynamická|Výchozí příznak ověření řetězu certifikátů; může být přepsána specifické komponenty příznak; například federační/X509CertChainFlags 0x10000000 CERT_CHAIN_REVOCATION_CHECK_END_CERT 0x20000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN 0x40000000 CERT_CHAIN_REVOCATION_CHECK_CHAIN_EXCLUDE_ROOT 0x80000000 CERT_CHAIN_REVOCATION_CHECK_CACHE_ POUZE nastavení na hodnotu 0 zakáže úplná kontrola CRL je seznam podporovaných hodnot zdokumentovat, dwFlags CertGetCertificateChain: http://msdn.microsoft.com/library/windows/desktop/aa376078 (v=vs.85).aspx |
|IgnoreCrlOfflineError|BOOL, výchozí hodnotu FALSE|Dynamická|Jestli se má seznam CRL offline chybu ignorovat, když serverovou ověří příchozí klientské certifikáty |
|IgnoreSvrCrlOfflineError|má hodnotu TRUE, BOOL, výchozí|Dynamická|Jestli se má seznam CRL offline chybu ignorovat, když na straně klienta ověří příchozí certifikáty serveru; Výchozí nastavení na hodnotu true. Útoky s certifikáty odvolané serveru vyžadují porušení zabezpečení DNS; těžší než s odvolané klientské certifikáty. |
|CrlDisablePeriod|Časový interval, výchozí hodnota je Common::TimeSpan::FromMinutes(15)|Dynamická|Zadejte časový interval v sekundách. Jak dlouho kontrola CRL je zakázán pro daný certifikát po zjištění chyby offline; Pokud seznam CRL offline chybu můžete ignorovat. |
|CrlOfflineHealthReportTtl|Časový interval, výchozí hodnota je Common::TimeSpan::FromMinutes(1440)|Dynamická|Zadejte časový interval v sekundách. |
|CertificateHealthReportingInterval|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(3600 * 8)|Statická|Zadejte časový interval v sekundách. Zadejte interval pro vykazování stavu certifikátu; Výchozí nastavení 8 hodin; nastavení na hodnotu 0 zakáže, vytváření sestav stavu certifikátu |
|CertificateExpirySafetyMargin|Časový interval, výchozí hodnota je Common::TimeSpan::FromMinutes(43200)|Statická|Zadejte časový interval v sekundách. Zabezpečení rozpětí pro vypršení platnosti certifikátu; certifikát stav sestavy změní z OK upozornění při vypršení platnosti je méně než to. Výchozí hodnota je 30 dní. |
|ClientClaimAuthEnabled|BOOL, výchozí hodnotu FALSE|Statická|Označuje, zda je povoleno ověřování na základě deklarace na klientských počítačích; Toto nastavení true implicitně nastaví ClientRoleEnabled. |
|ClientClaims|řetězec, výchozí je L""|Dynamická|Všechny možné deklarace identity očekávat od klientů pro připojení k bráně. Toto je seznam 'Nebo': ClaimsEntry || ClaimsEntry || ClaimsEntry... každý ClaimsEntry je seznam "A": typ ClaimType = ClaimValue & & Typ ClaimType = ClaimValue & & Typ ClaimType = ClaimValue... |
|AdminClientClaims|řetězec, výchozí je L""|Dynamická|Všechny možné deklarace identity očekávat od správce klientů. stejný formát jako ClientClaims; Tento seznam získá interně přidán do ClientClaims; proto není nutné také přidat do ClientClaims stejné položky. |
|ClusterSpn|řetězec, výchozí je L""|Není povoleno|Hlavní název služby clusteru; Pokud prostředků infrastruktury používá jako uživatel domény jednoho (uživatelský účet domény nebo gMSA). Je název SPN naslouchací procesy zapůjčení a moduly pro naslouchání v fabric.exe: moduly pro naslouchání federační; interní replikace naslouchací procesy; naslouchací proces služby modulu runtime a pojmenování naslouchací proces brány. To by měl být prázdné při spuštění prostředků infrastruktury jako účty počítače; v takovém případě připojování straně výpočetní naslouchací proces SPN z adresy naslouchací proces přenosu. |
|ClusterIdentities|řetězec, výchozí je L""|Dynamická|Windows identity uzlů clusteru; použít k ověřování členství v clusteru. Je čárkami oddělený seznam; Každý záznam je název účtu domény nebo název skupiny |
|ClientIdentities|řetězec, výchozí je L""|Dynamická|Windows identity FabricClient; pojmenování brána to používá k autorizaci příchozí připojení. Je čárkami oddělený seznam; Každý záznam je název účtu domény nebo název skupiny. Pro usnadnění práce; účet, který spouští fabric.exe je automaticky povolen; Proto jsou skupiny ServiceFabricAllowedUsers a ServiceFabricAdministrators. |
|AdminClientIdentities|řetězec, výchozí je L""|Dynamická|Identity Windows fabric klientů v roli správce; použitý k autorizaci operations privilegované prostředků infrastruktury. Je čárkami oddělený seznam; Každý záznam je název účtu domény nebo název skupiny. Pro usnadnění práce; účet, který spouští fabric.exe se automaticky přiřadí role správce; Proto je skupiny ServiceFabricAdministrators. |
|AADTenantId|řetězec, výchozí je L""|Statická|ID klienta (identifikátor GUID) |
|AADClusterApplication|řetězec, výchozí je L""|Statická|Název aplikace webového rozhraní API nebo ID představující clusteru |
|AADClientApplication|řetězec, výchozí je L""|Statická|ID představující klienti prostředků infrastruktury nebo nativní název aplikace klienta |
|X509Folder|řetězec, výchozí je /var/lib/waagent|Statická|Složka kde X509 certifikáty a soukromé klíče jsou k dispozici |
|FabricHostSpn| řetězec, výchozí je L"" |Statická| Hlavní název služby z hostitele FabricHost; Pokud je fabric spuštěna jako uživatel domény jednoho (uživatelský účet gMSA nebo domény) a hostitele FabricHost běží pod účtem počítače. Je název SPN IPC naslouchací proces pro hostitele FabricHost; který ve výchozím nastavení by mělo být ponecháno prázdné od hostitele FabricHost běží pod účtem počítače |
|DisableFirewallRuleForPublicProfile| má hodnotu TRUE, BOOL, výchozí | Statická|Indikuje, jestli pravidlo brány firewall nesmí být povolené pro veřejný profil |
|DisableFirewallRuleForPrivateProfile| má hodnotu TRUE, BOOL, výchozí |Statická| Indikuje, jestli pravidlo brány firewall nesmí být povolené pro soukromý profil | 
|DisableFirewallRuleForDomainProfile| má hodnotu TRUE, BOOL, výchozí |Statická| Indikuje, jestli pravidlo brány firewall nesmí být povolené pro profil domény |
|SettingsX509StoreName| řetězec, výchozí je L "MY"| Dynamická|X509 certifikátů používaných prostředků infrastruktury pro ochranu konfigurace úložiště |

### <a name="section-name-securityadminclientx509names"></a>Název oddílu: Zabezpečení nebo AdminClientX509Names
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
|PropertyGroup –|X509NameMap, výchozí hodnota je žádné|Dynamická| |

### <a name="section-name-securityclientx509names"></a>Název oddílu: Zabezpečení nebo ClientX509Names
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
PropertyGroup –|X509NameMap, výchozí hodnota je žádné|Dynamická| |

### <a name="section-name-securityclusterx509names"></a>Název oddílu: Zabezpečení nebo ClusterX509Names
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
PropertyGroup –|X509NameMap, výchozí hodnota je žádné|Dynamická| |

### <a name="section-name-securityserverx509names"></a>Název oddílu: Zabezpečení nebo ServerX509Names
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
PropertyGroup –|X509NameMap, výchozí hodnota je žádné|Dynamická| |

### <a name="section-name-securityclientaccess"></a>Název oddílu: Zabezpečení nebo ClientAccess
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| CreateName |řetězec, výchozí je "Admin" |Dynamická|Konfigurace zabezpečení pro identifikátor URI pojmenování vytvoření. |
| DeleteName |řetězec, výchozí je "Admin" |Dynamická|Konfigurace zabezpečení pro identifikátor URI pojmenování odstranění. |
| PropertyWriteBatch |řetězec, výchozí je "Admin" |Dynamická|Konfigurace zabezpečení pro pojmenování vlastnost operací zápisu. |
| CreateService |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení k tvorbě služeb. |
| CreateServiceFromTemplate |řetězec, výchozí je "Admin" |Dynamická|Konfigurace zabezpečení pro vytvoření služby ze šablony. |
| UpdateService |řetězec, výchozí je "Admin" |Dynamická|Konfigurace zabezpečení pro aktualizace služby. |
| DeleteService  |řetězec, výchozí je "Admin" |Dynamická|Konfigurace zabezpečení pro odstranění služby. |
| ProvisionApplicationType |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro aplikace typ zřizování. |
| CreateApplication |řetězec, výchozí je "Admin" | Dynamická|Konfigurace zabezpečení pro vytvoření aplikace. |
| DeleteApplication |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro odstranění aplikace. |
| UpgradeApplication |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro spuštění nebo přerušení upgrady aplikací. |
| RollbackApplicationUpgrade |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro vrácení zpět upgrady aplikací. |
| UnprovisionApplicationType |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení k rušení zajišťování typ aplikace. |
| MoveNextUpgradeDomain |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro obnovení upgradů aplikací s explicitní doménou upgradu. |
| ReportUpgradeHealth |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro obnovení upgradů aplikací s aktuální průběh upgradu. |
| ReportHealth |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro vytváření sestav stavu. |
| ProvisionFabric |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro MSI nebo clusteru manifestu zřizování. |
| UpgradeFabric |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro spuštění clusteru upgrady. |
| RollbackFabricUpgrade |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro vrácení zpět upgrady clusteru. |
| UnprovisionFabric |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro MSI nebo clusteru manifestu rušení zajišťování. |
| MoveNextFabricUpgradeDomain |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro obnovení upgradu clusteru s výslovně upgradu domény. |
| ReportFabricUpgradeHealth |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro obnovení upgradu clusteru s aktuální průběh upgradu. |
| StartInfrastructureTask |řetězec, výchozí je "Admin" | Dynamická|Konfigurace zabezpečení pro spuštění úlohy infrastruktury. |
| FinishInfrastructureTask |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro úlohy nepřipojujte infrastruktury. |
| ActivateNode |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro aktivaci a uzlu. |
| DeactivateNode |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro deaktivace uzlu. |
| DeactivateNodesBatch |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro deaktivace víc uzlů. |
| RemoveNodeDeactivations |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro navrácení deaktivace ve více uzlech. |
| GetNodeDeactivationStatus |řetězec, výchozí je "Admin" |Dynamická| Kontrola stavu deaktivace konfigurace zabezpečení. |
| NodeStateRemoved |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro vytváření sestav stavu uzel odebrán. |
| RecoverPartition |řetězec, výchozí je "Admin" | Dynamická|Konfigurace zabezpečení pro obnovení oddílu. |
| RecoverPartitions |řetězec, výchozí je "Admin" | Dynamická|Konfigurace zabezpečení pro obnovení oddíly. |
| RecoverServicePartitions |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro obnovení oddílů služby. |
| RecoverSystemPartitions |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro obnovení systému oddílů služby. |
| ReportFault |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro vytváření sestav chyb. |
| InvokeInfrastructureCommand |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro infrastrukturu úloh správy příkazy. |
| FileContent |řetězec, výchozí je "Admin" |Dynamická| Přenos souborů klienta (externí do clusteru) úložiště konfigurace zabezpečení pro bitovou kopii. |
| FileDownload |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro image store klienta soubor stažení spuštění (externí do clusteru). |
| InternalList |řetězec, výchozí je "Admin" | Dynamická|Konfigurace zabezpečení pro bitovou kopii ukládat operace seznamu souborů klienta (interní). |
| Odstranění |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro bitovou kopii ukládat operace odstranění klienta. |
| Odeslat |řetězec, výchozí je "Admin" | Dynamická|Operace nahrávání klienta úložiště konfigurace zabezpečení pro bitovou kopii. |
| GetStagingLocation |řetězec, výchozí je "Admin" |Dynamická| Klient pracovní umístění načtení úložiště konfigurace zabezpečení pro bitovou kopii. |
| GetStoreLocation |řetězec, výchozí je "Admin" |Dynamická| Načtení umístění úložiště klienta úložiště konfigurace zabezpečení pro bitovou kopii. |
| NodeControl |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro spuštění; Probíhá zastavování. a restartování uzlů. |
| CodePackageControl |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro restartování balíčky kódu. |
| UnreliableTransportControl |řetězec, výchozí je "Admin" |Dynamická| Nespolehlivém přenosu pro přidávání a odebírání chování. |
| MoveReplicaControl |řetězec, výchozí je "Admin" | Dynamická|Přesuňte repliky. |
| PredeployPackageToNode |řetězec, výchozí je "Admin" |Dynamická| Před nasazením rozhraní api. |
| StartPartitionDataLoss |řetězec, výchozí je "Admin" |Dynamická| Indukuje ztrátě dat pro oddíl. |
| StartPartitionQuorumLoss |řetězec, výchozí je "Admin" |Dynamická| Indukuje ztrátě kvora pro oddíl. |
| StartPartitionRestart |řetězec, výchozí je "Admin" |Dynamická| Současně restartuje některé nebo všechny repliky oddílu. |
| CancelTestCommand |řetězec, výchozí je "Admin" |Dynamická| Konkrétní TestCommand - zruší, pokud se nachází v cestě. |
| StartChaos |řetězec, výchozí je "Admin" |Dynamická| Spustí Chaos – Pokud ještě není spuštěný. |
| StopChaos |řetězec, výchozí je "Admin" |Dynamická| Chaos - zastaví, pokud byla spuštěna. |
| StartNodeTransition |řetězec, výchozí je "Admin" |Dynamická| Konfigurace zabezpečení pro spuštění uzlu přechod. |
| StartClusterConfigurationUpgrade |řetězec, výchozí je "Admin" |Dynamická| Indukuje StartClusterConfigurationUpgrade na oddíl. |
| GetUpgradesPendingApproval |řetězec, výchozí je "Admin" |Dynamická| Indukuje GetUpgradesPendingApproval na oddíl. |
| StartApprovedUpgrades |řetězec, výchozí je "Admin" |Dynamická| Indukuje StartApprovedUpgrades na oddíl. |
| Ping |řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Konfigurace zabezpečení pro příkazy ping klienta. |
| Dotaz |řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Konfigurace zabezpečení pro dotazy. |
| NameExists |řetězec, výchozí je "Admin\|\|Uživatel" | Dynamická|Konfigurace zabezpečení pro identifikátor URI pojmenování existence kontroly. |
| EnumerateSubnames |řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Konfigurace zabezpečení pro identifikátor URI pojmenování výčet. |
| EnumerateProperties |řetězec, výchozí je "Admin\|\|Uživatel" | Dynamická|Konfigurace zabezpečení u názvů běžných vlastnost výčtu. |
| PropertyReadBatch |řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Konfigurace zabezpečení pro pojmenování vlastnost operace čtení. |
| GetServiceDescription |řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Konfigurace zabezpečení pro dlouhé dotazování služby oznámení a čtení popis služby. |
| ResolveService |řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Konfigurace zabezpečení pro řešení služeb na základě předpisy. |
| ResolveNameOwner |řetězec, výchozí je "Admin\|\|Uživatel" | Dynamická|Konfigurace zabezpečení pro řešení identifikátor URI pojmenování vlastníka. |
| ResolvePartition |řetězec, výchozí je "Admin\|\|Uživatel" | Dynamická|Konfigurace zabezpečení pro řešení systémových služeb. |
| ServiceNotifications |řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Konfigurace zabezpečení na základě událostí služby oznámení. |
| PrefixResolveService |řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Konfigurace zabezpečení pro překlad předpony služeb na základě předpisy. |
| GetUpgradeStatus |řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Konfigurace zabezpečení pro dotazování na stav upgradu aplikace. |
| GetFabricUpgradeStatus |řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Konfigurace zabezpečení pro dotazování na stav upgradu clusteru. |
| InvokeInfrastructureQuery |řetězec, výchozí je "Admin\|\|Uživatel" | Dynamická|Konfigurace zabezpečení pro úlohy infrastruktury dotazování. |
| Seznam |řetězec, výchozí je "Admin\|\|Uživatel" | Dynamická|Konfigurace zabezpečení pro bitovou kopii ukládat operace seznamu souborů klienta. |
| ResetPartitionLoad |řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Konfigurace zabezpečení pro resetování načítání failoverUnit. |
| ToggleVerboseServicePlacementHealthReporting | řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Konfigurace zabezpečení pro přepnutím podrobné ServicePlacement HealthReporting. |
| GetPartitionDataLossProgress | řetězec, výchozí je "Admin\|\|Uživatel" | Dynamická|Načte průběh pro volání rozhraní api ke ztrátě dat invoke. |
| GetPartitionQuorumLossProgress | řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Načte průběh pro vyvolat volání api ztrátě kvora. |
| GetPartitionRestartProgress | řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Načte průběh pro volání rozhraní api oddílu restartování. |
| GetChaosReport | řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Načte stav Chaos v daném časovém rozsahu. |
| GetNodeTransitionProgress | řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Konfigurace zabezpečení pro získání průběhu na přechod příkaz uzlu. |
| GetClusterConfigurationUpgradeStatus | řetězec, výchozí je "Admin\|\|Uživatel" |Dynamická| Indukuje GetClusterConfigurationUpgradeStatus na oddíl. |
| GetClusterConfiguration | řetězec, výchozí je "Admin\|\|Uživatel" | Dynamická|Indukuje GetClusterConfiguration na oddíl. |
|CreateComposeDeployment|řetězec, výchozí je L "Admin"| Dynamická|Vytvoří nové nasazení popsané nové soubory |
|DeleteComposeDeployment|řetězec, výchozí je L "Admin"| Dynamická|Odstraní nové nasazení |
|UpgradeComposeDeployment|řetězec, výchozí je L "Admin"| Dynamická|Upgraduje nové nasazení |
|ResolveSystemService|řetězec, výchozí je L "Admin\|\|Uživatel"|Dynamická| Konfigurace zabezpečení pro řešení systémových služeb |
|GetUpgradeOrchestrationServiceState|řetězec, výchozí je L "Admin"| Dynamická|Indukuje GetUpgradeOrchestrationServiceState pro oddíl |
|SetUpgradeOrchestrationServiceState|řetězec, výchozí je L "Admin"| Dynamická|Indukuje SetUpgradeOrchestrationServiceState pro oddíl |

### <a name="section-name-reconfigurationagent"></a>Název oddílu: ReconfigurationAgent
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| ApplicationUpgradeMaxReplicaCloseDuration | Čas v sekundách, výchozí hodnota je 900 |Dynamická|Zadejte časový interval v sekundách. Dobu, pro kterou systému vyčká, než se ukončuje hostitelé služeb, které mají repliky, která se zasekla v automatickém zavřete během upgradu aplikace.|
| ServiceApiHealthDuration | Čas v sekundách, výchozí hodnota je 30 minut |Dynamická| Zadejte časový interval v sekundách. ServiceApiHealthDuration definuje, jak dlouho do jsme počkejte rozhraní API služby, který bude spuštěn před jsme nahlaste to není v pořádku. |
| ServiceReconfigurationApiHealthDuration | Čas v sekundách, výchozí hodnota je 30 |Dynamická| Zadejte časový interval v sekundách. ServiceReconfigurationApiHealthDuration definuje, jak dlouho do jsme čekat na rozhraní API služby spustit předtím, než vytvoříme sestavy není v pořádku. To platí pro volání rozhraní API, které mít dopad na dostupnost.|
| PeriodicApiSlowTraceInterval | Čas v sekundách, výchozí hodnota je 5 minut |Dynamická| Zadejte časový interval v sekundách. PeriodicApiSlowTraceInterval definuje interval, za které bude pomalého volání rozhraní API překreslována monitorování rozhraní API. |
| NodeDeactivationMaxReplicaCloseDuration | Čas v sekundách, výchozí hodnota je 900 |Dynamická|Zadejte časový interval v sekundách. Dobu, pro kterou systému vyčká, než se ukončuje hostitelé služeb, které mají repliky, která se zasekla v automatickém zavřete během deaktivace uzlu. |
| FabricUpgradeMaxReplicaCloseDuration | Čas v sekundách, výchozí hodnota je 900 |Dynamická| Zadejte časový interval v sekundách. Dobu, pro kterou systému vyčká, než se ukončuje hostitelé služeb, které mají repliky, která se zasekla v automatickém zavřete během upgrade pro fabric. |
|GracefulReplicaShutdownMaxDuration|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(120)|Dynamická|Zadejte časový interval v sekundách. Dobu, pro kterou systému vyčká, než se ukončuje hostitelé služeb, které mají repliky, která se zasekla v automatickém zavřete.|
|ReplicaChangeRoleFailureRestartThreshold|Int, výchozí hodnota je 10|Dynamická| Celé číslo. Zadejte počet selhání rozhraní API během primární povýšení, po které se použijí ke zmírnění automatické akce (repliky restartovat). |
|ReplicaChangeRoleFailureWarningReportThreshold|int, výchozí je 2147483647.|Dynamická| Celé číslo. Zadejte počet selhání rozhraní API během primární povýšení, po jejímž uplynutí sestava stavu upozornění, bude vyvolána.|

### <a name="section-name-placementandloadbalancing"></a>Název oddílu: PlacementAndLoadBalancing
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| TraceCRMReasons |BOOL, výchozí hodnota je true |Dynamická|Určuje, jestli chcete trasovat důvody CRM vydané pohybů kanálu provozní události. |
| ValidatePlacementConstraint | BOOL, výchozí hodnota je true |Dynamická| Určuje, zda výraz PlacementConstraint pro službu ověření při aktualizaci služby ServiceDescription. |
| PlacementConstraintValidationCacheSize | Int, výchozí hodnota je 10000. |Dynamická| Omezuje velikost tabulky použít pro rychlé ověření a ukládání do mezipaměti výrazů omezení umístění. |
|VerboseHealthReportLimit | Int, výchozí hodnota je 20 | Dynamická|Určuje počet, kolikrát má replika přejdete Neumístěná před upozornění stavu údajně pro něj (Pokud je povoleno oznamování stavu verbose). |
|ConstraintViolationHealthReportLimit | Int, výchozí hodnota je 50 |Dynamická| Určuje počet, kolikrát omezení bychom při tom porušili repliky musí být trvalé nevyřešené předtím, než jsou prováděny diagnostiky a stavu sestavy jsou vygenerované. |
|DetailedConstraintViolationHealthReportLimit | Int, výchozí hodnota je 200 |Dynamická| Určuje počet, kolikrát omezení bychom při tom porušili repliky musí před trvale nevyřešené diagnostiky jsou prováděny a podrobných stavu, které jsou vygenerované sestavy. |
|DetailedVerboseHealthReportLimit | Int, výchozí hodnota je 200 | Dynamická|Definuje počet opakovaných Neumístěná repliky musí být trvalé Neumístěná předtím, než jsou vygenerované stavu podrobné sestavy. |
|ConsecutiveDroppedMovementsHealthReportLimit | Int, výchozí hodnota je 20 | Dynamická|Definuje počet po sobě jdoucích pokusů vydané ResourceBalancer pohybů jsou vyřazen dříve, než jsou prováděny diagnostiky a jsou vygenerované stavu upozornění. Záporné: Žádná upozornění vygenerované v rámci této podmínky. |
|DetailedNodeListLimit | Int, výchozí hodnota je 15 |Dynamická| Definuje počet uzlů na omezení, které zahrnují před zkrácení v sestavách Neumístěná repliky. |
|DetailedPartitionListLimit | Int, výchozí hodnota je 15 |Dynamická| Definuje počet oddílů na diagnostiky záznam pro omezení zahrnout před zkrácení v diagnostice. |
|DetailedDiagnosticsInfoListLimit | Int, výchozí hodnota je 15 |Dynamická| Definuje počtem diagnostických záznamů (s podrobnými informacemi) na omezení, které zahrnují před zkrácení v diagnostice.|
|PLBRefreshGap | Čas v sekundách, výchozí hodnota je 1 |Dynamická| Zadejte časový interval v sekundách. Definuje minimální množství času, které musí uplynout, než PLB aktualizuje stav znovu. |
|MinPlacementInterval | Čas v sekundách, výchozí hodnota je 1 |Dynamická| Zadejte časový interval v sekundách. Definuje minimální množství času, který musí projít před zaokrouhlí dvě po sobě jdoucích umístění. |
|MinConstraintCheckInterval | Čas v sekundách, výchozí hodnota je 1 |Dynamická| Zadejte časový interval v sekundách. Definuje minimální množství času, které musí uplynout, než dvě po sobě jdoucích omezení zkontrolujte zaokrouhlí. |
|MinLoadBalancingInterval | Čas v sekundách, výchozí hodnota je 5 |Dynamická| Zadejte časový interval v sekundách. Definuje minimální množství času, který musí projít před dvě po sobě jdoucích vyrovnávání zaokrouhlí. |
|BalancingDelayAfterNodeDown | Čas v sekundách, výchozí hodnota je 120. |Dynamická|Zadejte časový interval v sekundách. Nespouštět vyrovnávání aktivit během tohoto období po uzlu událost vypnutí. |
|BalancingDelayAfterNewNode | Čas v sekundách, výchozí hodnota je 120. |Dynamická|Zadejte časový interval v sekundách. Nespouštět vyrovnávání aktivit během tohoto období, po přidání nového uzlu. |
|ConstraintFixPartialDelayAfterNodeDown | Čas v sekundách, výchozí hodnota je 120. |Dynamická| Zadejte časový interval v sekundách. To není narušení omezení opravte FaultDomain a UpgradeDomain během tohoto období po uzlu událost vypnutí. |
|ConstraintFixPartialDelayAfterNewNode | Čas v sekundách, výchozí hodnota je 120. |Dynamická| Zadejte časový interval v sekundách. DDo FaultDomain opravit a narušení omezení UpgradeDomain během tohoto období, po přidání nového uzlu. |
|GlobalMovementThrottleThreshold | Uint, výchozí hodnota je 1 000 |Dynamická| Maximální počet pohybů ve fázi vyrovnávání v posledních interval indikován GlobalMovementThrottleCountingInterval povoleny. |
|GlobalMovementThrottleThresholdForPlacement | Uint, výchozí hodnota je 0 |Dynamická| Maximální počet pohybů povolené ve fázi umístění v posledních intervalu indikován GlobalMovementThrottleCountingInterval.0 znamená bez omezení.|
|GlobalMovementThrottleThresholdForBalancing | Uint, výchozí hodnota je 0 | Dynamická|Maximální počet pohybů ve fázi vyrovnávání v posledních interval indikován GlobalMovementThrottleCountingInterval povoleny. 0 znamená bez omezení. |
|GlobalMovementThrottleCountingInterval | Čas v sekundách, výchozí hodnota je 600 |Statická| Zadejte časový interval v sekundách. Oznámení délku posledních intervalu, pro které chcete sledovat za pohybů repliky domény (používá se společně s GlobalMovementThrottleThreshold). Můžete nastavit na hodnotu 0 ignorovat globální omezení šířky pásma úplně. |
|MovementPerPartitionThrottleThreshold | Uint, výchozí hodnota je 50 |Dynamická| Přesun žádné související s vyrovnávání dojde pro oddíl v případě, že má dosáhli nebo Přesáhli jste MovementPerFailoverUnitThrottleThreshold v posledních intervalu indikován počet vyrovnávání související pohybů pro repliky tohoto oddílu MovementPerPartitionThrottleCountingInterval. |
|MovementPerPartitionThrottleCountingInterval | Čas v sekundách, výchozí hodnota je 600 |Statická| Zadejte časový interval v sekundách. Oznámení délku posledních intervalu, pro které chcete sledovat pohybů repliky pro každý oddíl (používá se společně s MovementPerPartitionThrottleThreshold). |
|PlacementSearchTimeout | Čas v sekundách, výchozí hodnota je 0,5 |Dynamická| Zadejte časový interval v sekundách. Při vkládání služeb; Vyhledejte maximálně tento dlouho před vrácením výsledku. |
|UseMoveCostReports | BOOL, výchozí hodnota je false | Dynamická|Dá pokyn LB ignorovat elementu náklady na funkci vyhodnocování; Výsledná potenciálně velkého počtu přesun lépe vyrovnáváním umístění. |
|PreventTransientOvercommit | BOOL, výchozí hodnota je false | Dynamická|Určuje, by měl PLB okamžitě počet na prostředcích, které uvolní initiated přesune. Ve výchozím nastavení; Přesun se můžete inicializovat PLB a přetížit přesunutí v na stejném uzlu, který můžete vytvořit přechodný. Nastavení tohoto parametru na hodnotu true, nebude možné tyto typy z overcommits a na vyžádání defragmentace (neboli placementWithMove) bude zakázán. |
|InBuildThrottlingEnabled | BOOL, výchozí hodnota je false |Dynamická| Určí, zda je povoleno omezení v sestavení. |
|InBuildThrottlingAssociatedMetric | Výchozí hodnota je řetězec, "" |Statická| Název přidružené metriky pro toto omezení. |
|InBuildThrottlingGlobalMaxValue | Int, výchozí hodnota je 0 |Dynamická|Maximální počet replik ve sestavení globálně povoleny. |
|SwapPrimaryThrottlingEnabled | BOOL, výchozí hodnota je false|Dynamická| Určí, zda je povoleno swap primární omezení. |
|SwapPrimaryThrottlingAssociatedMetric | Výchozí hodnota je řetězec, ""|Statická| Název přidružené metriky pro toto omezení. |
|SwapPrimaryThrottlingGlobalMaxValue | Int, výchozí hodnota je 0 |Dynamická| Maximální počet replik primárních prohození povoleny globálně. |
|PlacementConstraintPriority | Int, výchozí hodnota je 0 | Dynamická|Určuje prioritu omezení umístění: 0: pevné; 1: logicky; záporné: ignorovat. |
|PreferredLocationConstraintPriority | Int, výchozí hodnota je 2| Dynamická|Určuje prioritu omezení upřednostňované umístění: 0: pevné; 1: logicky; 2: optimalizace; záporné: Ignorovat |
|CapacityConstraintPriority | Int, výchozí hodnota je 0 | Dynamická|Určuje prioritu omezení kapacity: 0: pevné; 1: logicky; záporné: ignorovat. |
|AffinityConstraintPriority | Int, výchozí hodnota je 0 | Dynamická|Určuje prioritu omezení spřažení: 0: pevné; 1: logicky; záporné: ignorovat. |
|FaultDomainConstraintPriority | Int, výchozí hodnota je 0 |Dynamická| Určuje prioritu omezení domény chyby: 0: pevné; 1: logicky; záporné: ignorovat. |
|UpgradeDomainConstraintPriority | Int, výchozí hodnota je 1| Dynamická|Určuje prioritu omezení domény upgradu: 0: pevné; 1: logicky; záporné: ignorovat. |
|ScaleoutCountConstraintPriority | Int, výchozí hodnota je 0 |Dynamická| Určuje prioritu omezení počtu škálování: 0: pevné; 1: logicky; záporné: ignorovat. |
|ApplicationCapacityConstraintPriority | Int, výchozí hodnota je 0 | Dynamická|Určuje prioritu omezení kapacity: 0: pevné; 1: logicky; záporné: ignorovat. |
|MoveParentToFixAffinityViolation | BOOL, výchozí hodnota je false |Dynamická| Nastavení, která určuje, zda nadřazené repliky lze přesunout opravit spřažení omezení.|
|MoveExistingReplicaForPlacement | BOOL, výchozí hodnota je true |Dynamická|Nastavení, která určuje, pokud má přesunout existující repliku během umísťování. |
|UseSeparateSecondaryLoad | BOOL, výchozí hodnota je true | Dynamická|Nastavení, která určuje, zda použít jiný sekundární zatížení. |
|PlaceChildWithoutParent | BOOL, výchozí hodnota je true | Dynamická|Nastavení, která určuje, zda podřízené služby repliky mohou být umístěny, pokud žádnou repliku nadřazené je zapnutý. |
|PartiallyPlaceServices | BOOL, výchozí hodnota je true |Dynamická| Určuje, pokud všechny služby repliky v clusteru se uskuteční "všechny nebo nic" dané omezené vhodný uzly pro ně.|
|InterruptBalancingForAllFailoverUnitUpdates | BOOL, výchozí hodnota je false | Dynamická|Určuje, zda by měl jakýkoli typ aktualizace jednotky převzetí služeb při selhání přerušení rychlé nebo pomalé vyrovnávání spustit. Pomocí zadané "false" vyrovnávání spustit bude přerušena v případě FailoverUnit: je vytvořit/odstranit; Chybí repliky; změnit umístění primární repliku nebo změněné počet replik. Vyrovnávání spustit není přerušena v ostatních případech – Pokud FailoverUnit: má navíc repliky; změnit všechny repliky příznak; změnit jenom oddílu verze nebo jiném případu. |
|GlobalMovementThrottleThresholdPercentage|Double výchozí hodnota je 0|Dynamická|Maximální počet celkový počet pohybů v posledních interval indikován GlobalMovementThrottleCountingInterval povoleny ve fázích vyrovnávání a umístění (vyjádřeno jako procento celkového počtu replik v clusteru). 0 znamená bez omezení. Pokud tato a zadávají se GlobalMovementThrottleThreshold; potom se používá více konzervativní limit.|
|GlobalMovementThrottleThresholdPercentageForBalancing|Double výchozí hodnota je 0|Dynamická|Maximální počet pohybů ve fázi vyrovnávání (vyjádřeno jako procento celkového počtu replik v PLB) v posledních interval indikován GlobalMovementThrottleCountingInterval povoleny. 0 znamená bez omezení. Pokud tato a zadávají se GlobalMovementThrottleThresholdForBalancing; potom se používá více konzervativní limit.|
|AutoDetectAvailableResources|má hodnotu TRUE, BOOL, výchozí|Statická|Tato konfigurace se aktivuje automatické zjišťování prostředků dostupných na uzel (procesor a paměť) když tato konfigurace je nastavená na hodnotu true – jsme bude číst skutečné kapacity a opravte je, pokud uživatel zadán chybný uzlu kapacity nebo neuvedli je vůbec Pokud tuto konfigurace nastaven na hodnotu false - provedeme  trasování upozornění, že tento uživatel zadat chybné uzlu kapacity; ale jsme nebude opravte je; znamená, že uživatel chce, aby tak, aby měl kapacity zadaný jako > než Opravdu má uzel, nebo pokud kapacity nedefinované; bude předpokládat neomezené kapacity |

### <a name="section-name-hosting"></a>Název oddílu: hostování
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| ServiceTypeRegistrationTimeout |Čas v sekundách, výchozí hodnota je 300 |Dynamická|Maximální dobu povolenou pro ServiceType zaregistrovat u prostředků infrastruktury |
| ServiceTypeDisableFailureThreshold |Celé číslo, výchozí je 1 |Dynamická|Toto je prahová hodnota pro počet selhání, po jejímž uplynutí je FailoverManager (FM) informováni o zakažte typ služby v tomto uzlu a zkuste na jiný uzel pro umístění. |
| ActivationRetryBackoffInterval |Čas v sekundách, výchozí hodnota je 5 |Dynamická|Interval omezení rychlosti na každé selhání aktivace; Na každé selhání průběžné aktivace systému opakuje aktivace pro až MaxActivationFailureCount. Interval opakování na každé zkuste je produkt selhání průběžné aktivace a interval aktivace zpět – vypnuté. |
| ActivationMaxRetryInterval |Čas v sekundách, výchozí hodnota je 300 |Dynamická|Na každé selhání průběžné aktivace systému opakuje aktivace pro až ActivationMaxFailureCount. ActivationMaxRetryInterval Určuje časový interval čekání před opakováním po každé selhání aktivace |
| ActivationMaxFailureCount |Celé číslo, výchozí je 10 |Dynamická|Počet opakování systému se nezdařila aktivace, než se předá |
|ActivationTimeout| Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(180)|Dynamická| Zadejte časový interval v sekundách. Časový limit pro aktivaci aplikace; deaktivace a upgradu. |
|ApplicationHostCloseTimeout| Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(120)|Dynamická| Zadejte časový interval v sekundách. Při ukončení prostředků infrastruktury je zjištěn během svým aktivovat procesy; FabricRuntime zavře všechny repliky v procesu uživatele hostitele (hostitele aplikace). Toto je časový limit operace zavření. |
|ApplicationUpgradeTimeout| Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(360)|Dynamická| Zadejte časový interval v sekundách. Časový limit upgradu aplikace. Pokud je časový limit je menší než "ActivationTimeout" deployer se nezdaří. |
|CreateFabricRuntimeTimeout|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(120)|Dynamická| Zadejte časový interval v sekundách. Hodnota časového limitu pro synchronizaci FabricCreateRuntime volání |
|DeploymentMaxFailureCount|Int, výchozí hodnota je 20| Dynamická|Nasazení aplikace se bude opakovat pro DeploymentMaxFailureCount časů před selháním nasazení této aplikace na uzlu.| 
|DeploymentMaxRetryInterval| Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(3600)|Dynamická| Zadejte časový interval v sekundách. Maximální interval opakování pro nasazení. Na každých průběžné selhání interval opakování vypočítá se jako minimální (DeploymentMaxRetryInterval; Průběžné počet selhání * DeploymentRetryBackoffInterval) |
|DeploymentRetryBackoffInterval| Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(10)|Dynamická|Zadejte časový interval v sekundách. Back – vypnuté interval pro selhání nasazení. Na každé selhání průběžné nasazování systému znovu se pokusí nasazení pro až MaxDeploymentFailureCount. Interval opakování je produkt selhání průběžné nasazování a interval omezení rychlosti nasazení. |
|EnableActivateNoWindow| BOOL, výchozí hodnotu FALSE|Dynamická| Aktivovaný proces je vytvořen na pozadí bez žádné konzoly. |
|EnableProcessDebugging|BOOL, výchozí hodnotu FALSE|Dynamická| Umožňuje spouštění aplikací hostitele v ladicím programu |
|EndpointProviderEnabled| BOOL, výchozí hodnotu FALSE|Statická| Umožňuje správu zdrojů koncový bod prostředků infrastruktury. Vyžaduje, aby počáteční a koncové rozsah portů aplikace v FabricNode. |
|FabricContainerAppsEnabled| BOOL, výchozí hodnotu FALSE|Statická| |
|FirewallPolicyEnabled|BOOL, výchozí hodnotu FALSE|Statická| Umožňuje otevřít porty brány firewall pro koncový bod prostředků s explicitní portů zadaných v ServiceManifest |
|GetCodePackageActivationContextTimeout|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(120)|Dynamická|Zadejte časový interval v sekundách. Hodnota časového limitu pro volání CodePackageActivationContext. Tento parametr nelze použít pro služby ad hoc. |
|IPProviderEnabled|BOOL, výchozí hodnotu FALSE|Statická|Umožňuje správu IP adres. |
|NTLMAuthenticationEnabled|BOOL, výchozí hodnotu FALSE|Statická| Povolí podporu pro balíčky kódu, které jsou s jiným uživatelům, aby procesy napříč počítači bezpečně komunikovat pomocí protokolu NTLM. |
|NTLMAuthenticationPasswordSecret|SecureString, výchozí hodnota je Common::SecureString(L"")|Statická|Není že šifrované má, který se používá ke generování hesla pro uživatele, protokol NTLM. Musí být nastavena, pokud NTLMAuthenticationEnabled má hodnotu true. Ověřený nasazovacím modulu. |
|NTLMSecurityUsersByX509CommonNamesRefreshInterval|Časový interval, výchozí hodnota je Common::TimeSpan::FromMinutes(3)|Dynamická|Zadejte časový interval v sekundách. Pravidelné interval, ve které hostitelský hledá nové certifikáty, který se má použít pro konfiguraci protokolu NTLM FileStoreService nastavení pro konkrétní prostředí. |
|NTLMSecurityUsersByX509CommonNamesRefreshTimeout|Časový interval, výchozí hodnota je Common::TimeSpan::FromMinutes(4)|Dynamická| Zadejte časový interval v sekundách. Časový limit pro konfiguraci protokolu NTLM uživatele, kteří používají běžné názvy certifikátů. Uživatelé NTLM, je potřeba pro sdílené složky FileStoreService. |
|RegisterCodePackageHostTimeout|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(120)|Dynamická| Zadejte časový interval v sekundách. Hodnota časového limitu pro volání FabricRegisterCodePackageHost synchronizace. Tento krok platí pro pouze více kód balíčku aplikace hostitele jako FWP |
|RequestTimeout|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(30)|Dynamická| Zadejte časový interval v sekundách. To představuje časový limit pro komunikaci mezi hostitele aplikací a prostředků infrastruktury proces pro různé hostování související operace, jako je například vytváření registrace; uživatele Registrace modulu runtime. |
|RunAsPolicyEnabled| BOOL, výchozí hodnotu FALSE|Statická| Umožňuje spuštění kódu balíčky jako místní uživatel jiného, než uživatele, pod které fabric proces běží. Chcete-li povolit tuto zásadu Fabric musí být spuštěn jako systém, nebo jako uživatel, který má SeAssignPrimaryTokenPrivilege. |
|ServiceFactoryRegistrationTimeout| Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(120)|Dynamická|Zadejte časový interval v sekundách. Hodnota časového limitu pro synchronizaci volání ServiceFactory registrace (Stateless/Stateful) |
|ServiceTypeDisableGraceInterval|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(30)|Dynamická|Zadejte časový interval v sekundách. Časový interval, po které je možné zakázat typ služby |

### <a name="section-name-federation"></a>Název oddílu: Federation
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| LeaseDuration |Čas v sekundách, výchozí hodnota je 30 |Dynamická|Doba trvání zapůjčení trvající mezi uzlu a své okolí. |
| LeaseDurationAcrossFaultDomain |Čas v sekundách, výchozí hodnota je 30 |Dynamická|Doba trvání zapůjčení trvající mezi uzlu a sousední napříč doménami selhání. |

### <a name="section-name-clustermanager"></a>Název oddílu: ClusterManager
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu** | **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
| UpgradeStatusPollInterval |Čas v sekundách, výchozí hodnota je 60 |Dynamická|Frekvence dotazování na stav upgradu aplikace. Tato hodnota určuje počet aktualizace pro všechny GetApplicationUpgradeProgress volání |
| UpgradeHealthCheckInterval |Čas v sekundách, výchozí hodnota je 60 |Dynamická|Četnost stav kontroluje, při upgradech monitorovanou aplikaci. |
| FabricUpgradeStatusPollInterval |Čas v sekundách, výchozí hodnota je 60 |Dynamická|Frekvence dotazování na stav upgradu prostředků infrastruktury. Tato hodnota určuje počet aktualizace pro všechny GetFabricUpgradeProgress volání |
| FabricUpgradeHealthCheckInterval |Čas v sekundách, výchozí hodnota je 60 |Dynamická|Četnost stav zkontrolujte během monitorovaných upgrade pro Fabric |
|InfrastructureTaskProcessingInterval | Čas v sekundách, výchozí hodnota je 10 |Dynamická|Zadejte časový interval v sekundách. Interval zpracování, který používá infrastrukturu úlohy zpracování stavu počítače. |
|TargetReplicaSetSize |Int, výchozí hodnota je 7 |Není povoleno|TargetReplicaSetSize pro ClusterManager. |
|MinReplicaSetSize |Int, výchozí hodnota je 3 |Není povoleno|MinReplicaSetSize pro ClusterManager. |
|ReplicaRestartWaitDuration |Čas v sekundách, výchozí hodnota je (60.0 * 30)|Není povoleno|Zadejte časový interval v sekundách. ReplicaRestartWaitDuration pro ClusterManager. |
|QuorumLossWaitDuration |Čas v sekundách, výchozí hodnota je MaxValue |Není povoleno| Zadejte časový interval v sekundách. QuorumLossWaitDuration pro ClusterManager. |
|StandByReplicaKeepDuration | Čas v sekundách, výchozí hodnota je (3600.0 * 2)|Není povoleno|Zadejte časový interval v sekundách. StandByReplicaKeepDuration pro ClusterManager. |
|PlacementConstraints | Výchozí hodnota je řetězec, "" |Není povoleno|PlacementConstraints pro ClusterManager. |
|SkipRollbackUpdateDefaultService | BOOL, výchozí hodnota je false |Dynamická|CM pro přeskočí navrácení aktualizované výchozí služby během upgradu odvolání aplikace. |
|EnableDefaultServicesUpgrade | BOOL, výchozí hodnota je false |Dynamická|Povolte upgrade výchozí služby během upgradu aplikace. Po upgradu budou přepsána popisy výchozí služby. |
|InfrastructureTaskHealthCheckWaitDuration |Čas v sekundách, výchozí hodnota je 0|Dynamická| Zadejte časový interval v sekundách. Množství času čekat před zahájením kontroly stavu po po zpracování úloh infrastrukturu. |
|InfrastructureTaskHealthCheckStableDuration | Čas v sekundách, výchozí hodnota je 0|Dynamická| Zadejte časový interval v sekundách. Množství času, sledovat kontroly stavu po sobě jdoucích předaný před po zpracování úlohy infrastruktury příště úspěšně dokončena. Sledování selhání kontrolu obnoví tento časovač. |
|InfrastructureTaskHealthCheckRetryTimeout | Čas v sekundách, výchozí hodnota je 60 |Dynamická|Zadejte časový interval v sekundách. Množství času na opakování se nezdařilo kontroly stavu během po zpracování úloh infrastrukturu. Sledování kontrolu předaný stavu obnoví tento časovač. |
|ImageBuilderTimeoutBuffer |Čas v sekundách, výchozí hodnota je 3 |Dynamická|Zadejte časový interval v sekundách. Množství času, aby bylo možné Image Builder konkrétní vypršení časového limitu se vraťte do klienta. Pokud tato vyrovnávací paměť je příliš malý. Klient pak vyprší časový limit než server a získá chybu obecný časový limit. |
|MinOperationTimeout | Čas v sekundách, výchozí hodnota je 60 |Dynamická|Zadejte časový interval v sekundách. Minimální globálního časového limitu pro operace na ClusterManager interně zpracování. |
|Jako MaxOperationTimeout |Čas v sekundách, výchozí hodnota je MaxValue |Dynamická| Zadejte časový interval v sekundách. Maximální globálního časového limitu pro operace na ClusterManager interně zpracování. |
|MaxTimeoutRetryBuffer | Čas v sekundách, výchozí hodnota je 600 |Dynamická|Zadejte časový interval v sekundách. Časový limit maximální operace při interně z důvodu vypršení časových limitů opakování je <Original Time out>  +  <MaxTimeoutRetryBuffer>. V přírůstcích po MinOperationTimeout se přidá další časový limit. |
|MaxCommunicationTimeout |Čas v sekundách, výchozí hodnota je 600 |Dynamická|Zadejte časový interval v sekundách. Maximální časový limit pro interní komunikaci mezi ClusterManager a jiné systémy služeb (tj.); Naming Service; "Manager převzetí služeb při selhání a atd.). Tento časový limit musí být menší než globální jako MaxOperationTimeout (jak může existovat více komunikace mezi součástmi systému pro každou operaci klienta). |
|MaxDataMigrationTimeout |Čas v sekundách, výchozí hodnota je 600 |Dynamická|Zadejte časový interval v sekundách. Maximální časový limit operace obnovení migrace dat po neproběhla upgrade pro Fabric. |
|MaxOperationRetryDelay |Čas v sekundách, výchozí hodnota je 5|Dynamická| Zadejte časový interval v sekundách. Maximální zpoždění pro interní opakování, když došlo k selhání. |
|ReplicaSetCheckTimeoutRollbackOverride |Čas v sekundách, výchozí hodnota je 1200 |Dynamická| Zadejte časový interval v sekundách. Pokud je ReplicaSetCheckTimeout nastavená na maximální hodnotu DWORD; pak je přepsat s hodnotou této konfigurace pro účely vrácení zpět. Hodnota použitá pro dopředné nikdy přepsána. |
|ImageBuilderJobQueueThrottle |Int, výchozí hodnota je 10 |Dynamická|Počet omezení pro Image Builder fronty úloh proxy serveru na žádosti o aplikace přístup z více vláken. |
|MaxExponentialOperationRetryDelay|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(30)|Dynamická|Zadejte časový interval v sekundách. Maximální zpoždění exponenciální pro interní opakování, když jsou opakovaně došlo k selhání |

### <a name="section-name-defragmentationemptynodedistributionpolicy"></a>Název oddílu: DefragmentationEmptyNodeDistributionPolicy
| **Parametr** | **Povolené hodnoty** |**Zásady upgradu**| **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
|PropertyGroup –|KeyIntegerValueMap, výchozí hodnota je žádné|Dynamická|Určuje, že defragmentace zásad řídí při vyprazdňování uzlů. Pro danou metriku 0 znamená, že SF opakovat defragmentaci uzly rovnoměrně mezi UDs a FDs; 1 pouze znamená, že uzly musí defragmentovat |

### <a name="section-name-defragmentationmetrics"></a>Název oddílu: DefragmentationMetrics
| **Parametr** | **Povolené hodnoty** |**Zásady upgradu**| **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
|PropertyGroup –|KeyBoolValueMap, výchozí hodnota je žádné|Dynamická|Určuje sadu metriky, které se má použít pro defragmentaci a ne pro vyrovnávání zatížení. |

### <a name="section-name-defragmentationmetricspercentornumberofemptynodestriggeringthreshold"></a>Název oddílu: DefragmentationMetricsPercentOrNumberOfEmptyNodesTriggeringThreshold
| **Parametr** | **Povolené hodnoty** |**Zásady upgradu**| **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
|PropertyGroup –|KeyDoubleValueMap, výchozí hodnota je žádné|Dynamická|Určuje počet volné uzly, které jsou potřeba vzít v úvahu clusteru defragmentovat zadáním buď procent v rozsahu [0.0-1.0) nebo počet uzlů prázdný jako číslo > = 1.0 |

### <a name="section-name-dnsservice"></a>Název oddílu: DnsService
| **Parametr** | **Povolené hodnoty** |**Zásady upgradu**| **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
|Hodnotu IsEnabled|BOOL, výchozí hodnotu FALSE|Statická| |
|InstanceCount|int, výchozí je -1|Statická|  |

### <a name="section-name-metricactivitythresholds"></a>Název oddílu: MetricActivityThresholds
| **Parametr** | **Povolené hodnoty** |**Zásady upgradu**| **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
|PropertyGroup –|KeyIntegerValueMap, výchozí hodnota je žádné|Dynamická|Určuje sadu MetricActivityThresholds pro metriku v clusteru. Vyrovnávání bude fungovat, pokud je větší než MetricActivityThresholds maxNodeLoad. Pro defragmentační metriky definuje množství rovná zatížení, nebo pod které Service Fabric se uvažovat uzlu prázdný |

### <a name="section-name-metricbalancingthresholds"></a>Název oddílu: MetricBalancingThresholds
| **Parametr** | **Povolené hodnoty** |**Zásady upgradu**| **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
|PropertyGroup –|KeyDoubleValueMap, výchozí hodnota je žádné|Dynamická|Určuje sadu MetricBalancingThresholds pro metriku v clusteru. Vyrovnávání bude fungovat, pokud je větší než MetricBalancingThresholds maxNodeLoad/minNodeLoad. Defragmentace bude fungovat, pokud je menší než MetricBalancingThresholds maxNodeLoad/minNodeLoad v minimálně jeden FD nebo UD. |

### <a name="section-name-nodebufferpercentage"></a>Název oddílu: NodeBufferPercentage
| **Parametr** | **Povolené hodnoty** |**Zásady upgradu**| **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
|PropertyGroup –|KeyDoubleValueMap, výchozí hodnota je žádné|Dynamická|Procento kapacity uzlu za název metriky; Chcete-li zachovat některé volné místo na uzlu pro případ převzetí služeb při selhání se používá jako vyrovnávací paměť. |

### <a name="section-name-replication"></a>Název oddílu: replikace
| **Parametr** | **Povolené hodnoty** | **Zásady upgradu**| **Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
|MaxCopyQueueSize|Uint, výchozí hodnota je 1 024|Statická|Toto je maximální hodnota definuje počáteční velikost fronty, který udržuje operací replikace.  Všimněte si, že musí být násobkem 2.  Pokud během doby běhu zvětšuje, aby tato operace velikost fronty budou omezeny mezi primárním a sekundárním replikátory.|
|BatchAcknowledgementInterval|Časový interval, výchozí hodnota je Common::TimeSpan::FromMilliseconds(15)|Statická|Zadejte časový interval v sekundách. Určuje množství času, kterou Replikátor čeká po přijetí operace před odesláním zpět potvrzení. Jiné operace přijata během této doby bude mít jejich potvrzení odeslána zpět v do jedné zprávy -> Redukční síťový provoz, ale potenciálně omezení propustnosti replikátoru.|
|Maxreplicationmessagesize.|Uint, výchozí hodnota je 52428800|Statická|Maximální velikost zprávy operací replikace. Výchozí hodnota je 50MB.|
|ReplicatorAddress|řetězec, výchozí je L "localhost:0"|Statická|Koncový bod v podobě řetězce-'IP: port, který je používán Replikátor Windows Fabric ke zřízení spojení s dalšími replikami k odesílání a přijímání operace.|
|ReplicatorListenAddress|řetězec, výchozí je L "localhost:0"|Statická|Koncový bod v podobě řetězce-'IP: port, který je používán Replikátor Windows Fabric přijímat operace z jiné repliky.|
|ReplicatorPublishAddress|řetězec, výchozí je L "localhost:0"|Statická|Koncový bod v podobě řetězce-'IP: port, který je používán Replikátor Windows Fabric k odeslání operace do jiné repliky.|
|MaxPrimaryReplicationQueueSize|Uint, výchozí hodnota je 1 024|Statická|Toto je maximální počet operací, které může existovat ve frontě primární replikace. Všimněte si, že musí být násobkem 2.|
|MaxPrimaryReplicationQueueMemorySize|Uint, výchozí hodnota je 0|Statická|Toto je maximální hodnota primární replikace fronty v bajtech.|
|MaxSecondaryReplicationQueueSize|uint, výchozí je 2048|Statická|Toto je maximální počet operací, které může existovat ve frontě sekundární replikace. Všimněte si, že musí být násobkem 2.|
|MaxSecondaryReplicationQueueMemorySize|Uint, výchozí hodnota je 0|Statická|Toto je maximální hodnota sekundární replikační fronty v bajtech.|
|QueueHealthMonitoringInterval|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(30)|Statická|Zadejte časový interval v sekundách. Tato hodnota určuje časové období, které Replikátor používá k monitorování všechny události stavu upozornění nebo chyby ve frontách operace replikace. Hodnota '0' zakáže monitorování stavu |
|QueueHealthWarningAtUsagePercent|uint, výchozí je 80|Statická|Tato hodnota určuje využití fronty replikace (v procentech), po jejímž uplynutí vytvoříme sestavy upozornění o použití vysoké fronty. Budeme tak učinit po odkladu intervalu QueueHealthMonitoringInterval. Pokud využití fronty klesne pod tuto procentuální hodnotu v intervalu odkladu|
|RetryInterval|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(5)|Statická|Zadejte časový interval v sekundách. Při operaci dojde ke ztrátě nebo odmítnuté tento časovač Určuje, jak často bude Replikátor opakovat operaci odeslání.|

### <a name="section-name-transport"></a>Název oddílu: přenosu
| **Parametr** | **Povolené hodnoty** |**Zásady upgradu** |**Pokyny nebo krátký popis** |
| --- | --- | --- | --- |
|ResolveOption|řetězec, výchozí hodnota je L "neurčené"|Statická|Určuje, jak je vyřešit plně kvalifikovaný název domény.  Platné hodnoty jsou "neurčené nebo ipv4 nebo ipv6". |
|ConnectionOpenTimeout|Časový interval, výchozí hodnota je Common::TimeSpan::FromSeconds(60)|Statická|Zadejte časový interval v sekundách. Vypršení časového limitu pro nastavení připojení na příchozí a přijímá straně (včetně vyjednávání zabezpečení v zabezpečeném režimu) |

## <a name="next-steps"></a>Další kroky
Přečtěte si tyto články pro další informace o správě clusteru:

[Přidat, mění, odebrat certifikáty ze Azure clusteru](service-fabric-cluster-security-update-certs-azure.md) 

