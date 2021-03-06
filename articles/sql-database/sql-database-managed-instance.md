---
title: Azure SQL Database Managed Instance přehled | Dokumentace Microsoftu
description: Toto téma popisuje Azure SQL Database Managed Instance a vysvětluje, jak to funguje a jak se liší od izolované databáze ve službě Azure SQL Database.
services: sql-database
author: bonova
ms.reviewer: carlrab
manager: craigg
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/30/2018
ms.author: bonova
ms.openlocfilehash: 5caafdfbaca1a701d2728afdfcb31e8e73b4fded
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44050302"
---
# <a name="what-is-a-managed-instance-preview"></a>Co je Managed Instance (preview)?

Azure SQL Database Managed Instance (preview) je nový model nasazení služby Azure SQL Database poskytuje téměř 100 % kompatibilitu s nejnovějšími serveru SQL Server v místním (Enterprise Edition) databázový stroj, poskytuje nativní [(virtuální sítě Virtuální síť)](../virtual-network/virtual-networks-overview.md) implementace, která řeší běžné problémy zabezpečení a [obchodního modelu](https://azure.microsoft.com/pricing/details/sql-database/) uspokojivým pro zákazníky v místním systému SQL Server. Managed Instance umožňuje stávající zákazníci systému SQL Server k přenosu svých místních aplikací do cloudu s minimálními změnami aplikace a databáze. Managed Instance ve stejnou dobu, zachová všechny možnosti modelu PaaS (automatické použití dílčích oprav a verze aktualizace [automatické zálohování](sql-database-automated-backups.md), [vysoké dostupnosti](sql-database-high-availability.md) ), který výrazně snižuje režii na správu a celkové náklady na vlastnictví.

> [!IMPORTANT]
> Seznam oblastí, ve kterých je Managed Instance aktuálně k dispozici, najdete v tématu [Migrace databází do plně spravované služby pomocí Azure SQL Database Managed Instance](https://azure.microsoft.com/blog/migrate-your-databases-to-a-fully-managed-service-with-azure-sql-database-managed-instance/).
 
Následující diagram popisuje klíčové funkce služby Managed Instance:

![klíčové funkce](./media/sql-database-managed-instance/key-features.png)

Azure SQL Database Managed Instance je určená pro zákazníky v úmyslu migrovat velký počet aplikací z místní nebo IaaS, samoobslužné vytvořen, nebo nezávislý výrobce softwaru ve formě prostředí tak, aby plně spravované cloudové prostředí PaaS s nízkou migrace úsilí co nejvíce. Použití plně automatizované [Data Migration Service (DMS)](../dms/tutorial-sql-server-to-managed-instance.md#create-an-azure-database-migration-service-instance) v Azure, zákazníci mohou přenést a shift jejich místního SQL serveru do Managed Instance, která nabízí kompatibilitu s místním SQL serverem a kompletní izolaci zákaznických instancí s nativní podporou virtuální sítě.  S programem Software Assurance, mohou vyměňovat své stávající licence pro zlevněné sazby pro SQL Database Managed Instance pomocí [zvýhodněné hybridní využití Azure pro SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).  SQL Database Managed Instance je nejlepším řešením migrace v cloudu pro instance systému SQL Server, vyžadujících vysoké zabezpečení a s rozsáhlou programovatelností pomocí povrchu. 

Podle všeobecné dostupnosti zaměřuje Managed Instance poskytovat blízko povrchu 100 % kompatibilitu s nejnovější verzí systému SQL Server v místním prostřednictvím plánu dvoufázové instalace verze. 

Rozhodování mezi izolovanou databázi Azure SQL Database, Azure SQL Database Managed Instance a SQL Server IaaS hostované ve virtuálních počítačů najdete v tématu [jak zvolit správnou verzi systému SQL Server v cloudu Azure](sql-database-paas-vs-sql-server-iaas.md).

## <a name="key-features-and-capabilities"></a>Klíčové funkce a možnosti 

Azure SQL Database Managed Instance jsou k dispozici nejlepší funkce, které jsou k dispozici v Azure SQL Database i SQL Server Database Engine.

> [!IMPORTANT]
> Managed Instance spouští všechny funkce nejnovější verzi SQL serveru, včetně online operace, plán automatické opravy a vylepšení výkonu jiné organizace. 

| **Výhody PaaS** | **Kontinuita podnikových procesů** |
| --- | --- |
|Žádný hardware, nákup a správu <br>Není potřeba Správa režie pro správu základní infrastruktury <br>Rychlé zřizování a škálování služby <br>Automatické použití dílčích oprav a verze upgradu <br>Integrace s dalšími službami PaaS dat |99,99 % smlouva SLA o provozuschopnosti  <br>Součástí [vysoké dostupnosti](sql-database-high-availability.md) <br>Data chráněná [automatické zálohování](sql-database-automated-backups.md) <br>Zákazník možnost konfigurace uchovávání záloh období (pevně 7 dní ve verzi Public Preview) <br>Uživatelem iniciované [zálohy](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql?view=azuresqldb-mi-current) <br>[Bod v čase obnovení databáze](sql-database-recovery-using-backups.md#point-in-time-restore) funkce |
|**Zabezpečení a dodržování předpisů** | **Správa**|
|Izolované prostředí ([integrace virtuální sítě](sql-database-managed-instance-vnet-configuration.md), jednoho tenanta služby, vyhrazené výpočetní prostředky a úložiště) <br>[Transparentní šifrování dat (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)<br>[Ověřování Azure AD](sql-database-aad-authentication.md), jednotné přihlašování – podpora <br>Dodržuje standardy pro dodržování předpisů stejně jako Azure SQL database <br>[Auditování SQL](sql-database-managed-instance-auditing.md) <br>[Detekce hrozeb](sql-database-managed-instance-threat-detection.md) |Rozhraní API Azure Resource Manageru pro automatizaci služby zřizování a škálování <br>Azure portal funkci pro ruční službu zřizování a škálování <br>Data Migration Service 

## <a name="vcore-based-purchasing-model"></a>nákupní model založený na virtuálních jádrech

[Nákupní model založený na virtuálních jádrech](sql-database-service-tiers-vcore.md) poskytuje flexibilitu, kontrolu, transparentnost a jednoduchý způsob převodu požadavků místních úlohy do cloudu. Tento model umožňuje škálování výpočetních prostředků, paměť a úložiště na základě jejich potřebám provádění úloh. Je také nárok až na 30 procent spoření s modelu virt. jader [zvýhodněné hybridní využití Azure pro SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

Virtuální jádro reprezentuje logický procesor nabízený s možností volby mezi generacemi hardwaru.
- Logické CPU 4. generace jsou založené na procesorech Intel E5-2673 v3 (Haswell) 2,4 GHz.
- Generace 5 logické procesory jsou založené na Intel E5-2673 v4 (Broadwell) 2,3 GHz procesorech.

Následující tabulka vám pomůže pochopit postup výběru optimální konfiguraci výpočetních, paměťových, úložiště a vstupně-výstupní prostředky.

||Gen 4|Gen 5|
|----|------|-----|
|Hardware|Intel E5-2673 v3 (Haswell) 2,4 GHz procesorech připojené SSD vCore = 1 PP (fyzických jader)|Intel E5-2673 v4 (Broadwell) 2,3 GHz procesorech rychlé eNVM SSD, vCore = 1 LP (hyper vlákno)|
|Úrovně výkonu|8, 16, 24 virtuálních jader|8, 16, 24, 32, 40, 64, 80 virtuálních jader|
|Memory (Paměť)|7 GB na vCore|5.5 GB na vCore|
||||

## <a name="managed-instance-service-tiers"></a>Spravovaná instanci úrovně služeb

Managed Instance je k dispozici ve dvou úrovních služeb:
- **Obecné účely**: určená pro aplikace s typické výkon a latenci vstupně-výstupních operací.
- **Důležité obchodní informace**: určená pro aplikace s nízkou latenci vstupně-výstupní operace a minimální dopad základní údržbových operací u zatížení.

Obě úrovně služeb 99,99 % dostupnost a vám umožní nezávisle na sobě vyberte velikost úložiště a výpočetní kapacitu. Další informace o architektura pro vysokou dostupnost služby Azure SQL Database najdete v tématu [vysokou dostupnost a Azure SQL Database](sql-database-high-availability.md).

> [!IMPORTANT]
> Změna vaší úrovně služby z obecné na důležité obchodní informace a naopak se nepodporuje ve verzi Public Preview. Pokud chcete migrovat vaše databáze v úrovních různé služby, můžete vytvořit novou instanci, obnovení databází pomocí bodu v čase z původní instance a potom vyřaďte původní instanci, pokud už není potřeba. 

### <a name="general-purpose-service-tier"></a>Obecné účely úrovně služeb

Následující seznam popisuje úrovni General Purpose služeb klíčovou vlastnost: 

- Návrh pro většinu obchodních aplikací s požadavky na typické výkon 
- Vysoce výkonné služby Azure Premium storage (8 TB) 
- 100 databází na instanci 

Následující seznam popisuje klíčové vlastnosti vrstvy služby s obecné účely:

|Funkce | Popis|
|---|---|
| Počet virtuálních jader * | 8, 16, 24 (gen 4)<br>8, 16, 24, 32, 40, 64, 80 (generace 5)|
| Verze systému SQL Server / build | SQL Server Database Engine (nejnovější stabilní verze) |
| Minimální velikost úložiště | 32 GB |
| Maximální velikost úložiště | 8 TB |
| Max. úložiště na databázi | Určuje maximální velikost úložiště na instanci |
| Očekávaný úložiště vstupně-výstupních operací | 500-7500 IOPS na datový soubor (závisí na datovém souboru). Zobrazit [Storage úrovně Premium](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes) |
| Počet datových souborů (řádků) na databázi | Několik | 
| Počet souborů protokolu (LOG) na databázi | 1 | 
| Spravované automatizované zálohování | Ano |
| OVĚŘENÍ STAVU | Data uložená ve službě Azure Storage a [Azure Service Fabric](../service-fabric/service-fabric-overview.md) |
| Integrovanou instanci a databázi sledování a metriky | Ano |
| Software automatické opravy | Ano |
| Připojení typu VNet - nasazení Azure Resource Manageru | Ano |
| Připojení typu VNet - model nasazení Classic | Ne |
| Portál podpory | Ano|
|||

\* Virtuální jádro reprezentuje logický procesor nabízený s možností volby mezi generacemi hardwaru. Gen 4 logické procesory jsou založeny na Intel E5-2673 v3 (Haswell) 2,4 GHz a logické procesory generace 5 jsou založené na Intel E5-2673 v4 (Broadwell) 2,3 GHz procesorech. 

Další informace najdete v části [dostupnost/Standard pro obecné účely a architektura](sql-database-high-availability.md#standardgeneral-purpose-availability) ve službě Azure SQL Database.

### <a name="business-critical-service-tier"></a>Obchodní vrstvy kritické služby

Obchodní vrstvy kritické služby je sestaven pro aplikace s vysokými požadavky na vstupně-výstupních operací. Nabízí nejvyšší odolnost proti selhání s využitím několika izolovaných Always On replik. 

Následující seznam popisuje klíčové vlastnosti pro důležité obchodní informace vrstvy služby: 
-   Je určená pro obchodní aplikace s nejvyšší výkon a požadavky na vysokou dostupnost 
-   Se dodává s mimořádně rychlé úložiště SSD (až 1 TB na Gen 4 a až 4 TB generace 5)
-   Podporuje až 100 databáze na instanci 
- Integrované další jen pro čtení, jež lze použít pro vytváření sestav a dalších úloh jen pro čtení
- [OLTP v paměti](sql-database-in-memory.md) , který lze použít pro úlohy s vysokou prefrmance požadavky  

|Funkce | Popis|
|---|---|
| Počet virtuálních jader * | 8, 16, 24, 32 (4. generace)<br>8, 16, 24, 32, 40, 64, 80 (generace 5)|
| Verze systému SQL Server / build | SQL Server nejnovější (k dispozici) |
| Další funkce | [OLTP v paměti](sql-database-in-memory.md)<br> 1 další repliky jen pro čtení ([horizontální navýšení kapacity pro čtení](sql-database-read-scale-out.md))
| Minimální velikost úložiště | 32 GB |
| Maximální velikost úložiště | Gen 4: 1 TB (všech velikostí vCore)<br> Generace 5:<ul><li>1 TB pro 8, 16 virtuálních jader</li><li>2 TB pro 24 virtuálních jader</li><li>4 TB pro 32, 40, 64, 80 virtuálních jader</ul>|
| Max. úložiště na databázi | Určuje maximální velikost úložiště na instanci |
| Počet datových souborů (řádků) na databázi | Několik | 
| Počet souborů protokolu (LOG) na databázi | 1 | 
| Spravované automatizované zálohování | Ano |
| OVĚŘENÍ STAVU | Data uložená v místní SSD a použití [skupin dostupnosti Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) a [Azure Service Fabric](../service-fabric/service-fabric-overview.md) |
| Integrovanou instanci a databázi sledování a metriky | Ano |
| Software automatické opravy | Ano |
| Připojení typu VNet - nasazení Azure Resource Manageru | Ano |
| Připojení typu VNet - model nasazení Classic | Ne |
| Portál podpory | Ano|
|||

Další informace najdete v části [úrovně Premium/důležitých dostupnosti a architektura](sql-database-high-availability.md#premiumbusiness-critical-availability) ve službě Azure SQL Database.

## <a name="advanced-security-and-compliance"></a>Pokročilé zabezpečení a dodržování předpisů 

Azure SQL Database Managed Instance kombinuje rozšířené funkce zabezpečení poskytované cloudu Azure a SQL Server Database Engine. 

### <a name="managed-instance-security-isolation-in-azure-cloud"></a>Managed Instance izolace zabezpečení v cloudu Azure 

Managed Instance poskytuje dodatečné zabezpečení izolaci z jiných tenantů v cloudu Azure. Izolace zabezpečení zahrnuje: 

- [Implementace nativní virtuální sítě](sql-database-managed-instance-vnet-configuration.md) a připojení k vaší místní prostředí pomocí Azure Express Route nebo VPN Gateway 
- Koncový bod SQL je k dispozici pouze prostřednictvím privátní IP adresy, což bezpečné připojení z Azure privátní nebo hybridní sítě
- Jedním tenantem pomocí vyhrazené základní infrastruktura (výpočetní prostředky, úložiště)

Následující diagram popisuje různé možnosti připojení pro vaše aplikace: 

![Vysoká dostupnost](./media/sql-database-managed-instance/application-deployment-topologies.png)  

Další podrobnosti o integrace virtuální sítě a sítě enforcements zásad na úrovni podsítě, najdete v článku [konfigurace virtuální sítě pro Azure SQL Database Managed Instance](sql-database-managed-instance-vnet-configuration.md) a [připojení aplikace ke službě Azure SQL Database Spravovaná Instance](sql-database-managed-instance-connect-app.md). 

> [!IMPORTANT]
> Umístíte více spravované instance ve stejné podsíti, bez ohledu na to, který povoluje vaše požadavky na zabezpečení, jako, který vám přinese další výhody. Tyto instance ve stejné podsíti, se výrazně zjednodušit údržbu síťové infrastruktury a snížit čas, protože doba trvání dlouhé zřizování je spojen s náklady na nasazení první spravované instance v podsíti zřízení instance.

### <a name="azure-sql-database-security-features"></a>Funkcí zabezpečení služby Azure SQL Database

Azure SQL Database poskytuje sadu rozšířené funkce zabezpečení, které je možné chránit vaše data.

- [Managed Instance auditování](sql-database-managed-instance-auditing.md) sleduje události databáze a zapisuje je do souboru protokolu auditování umístěny do svého účtu úložiště Azure. Auditování pomáhá zajistit dodržování předpisů, porozumět databázové aktivitě a proniknout do nesrovnalostí a anomálií, které můžou značit problémy obchodního charakteru nebo vzbuzovat podezření na narušení zabezpečení. 
- Šifrování dat při pohybu - Managed Instance chrání vaše data zajištěním šifrování pro data přenášená data pomocí Transport Layer Security. Kromě zabezpečení transportní vrstvy, spravované Instance SQL Database nabízí ochranu citlivých dat na cestě v klidovém stavu a průběhu zpracování dotazů pomocí [s funkcí Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Funkce Always Encrypted je špičkou v odvětví, která nabízí bezkonkurenční zabezpečení dat před porušením zabezpečení, včetně krádeže důležitých dat. Například s funkcí Always Encrypted, čísla platebních karet jsou uloženy v zašifrované podobě v databázi vždy, i během dotazu zpracování, která umožňují dešifrování Přejme během použití autorizované zaměstnance nebo aplikace, které potřebují zpracovat data. 
- [Detekce hrozeb](sql-database-managed-instance-threat-detection.md) doplňuje [Managed Instance auditování](sql-database-managed-instance-auditing.md) ve poskytuje další úroveň zabezpečení funkce intelligence integrované do služby, který zjistí a potenciálně nebezpečné pokusy o přístup nebo zneužití databáze. Upozorní vás na podezřelé aktivity, potenciální ohrožení zabezpečení a útoky prostřednictvím injektáže SQL, stejně jako vzory přístupu k databázi neobvyklé. Výstrahy detekce hrozeb můžete zobrazit v [Azure Security Center](https://azure.microsoft.com/services/security-center/) a zadejte podrobnosti o podezřelé aktivitě a doporučení akce k prošetření a zmírnění hrozby.  
- [Dynamické maskování dat](/sql/relational-databases/security/dynamic-data-masking) omezuje riziko ohrožení citlivých dat jejich maskováním pro neoprávněným uživatelům. Dynamické maskování dat pomáhá zabránit neoprávněnému přístupu k citlivým datům tím, že vám určit, kolik citlivých dat zobrazíte s minimálním dopadem na aplikační vrstvu. Je to funkce zabezpečení založená na zásadách, která skrývá citlivá data v sadě výsledků dotazu nad určenými poli databáze, zatímco data v databázi se nemění. 
- [Zabezpečení na úrovní řádků](/sql/relational-databases/security/row-level-security) umožňuje řídit přístup k řádkům v databázové tabulce na základě charakteristiky uživatele spouštějícího dotaz (například podle skupiny členství nebo kontext spuštění). Zabezpečení na úrovni řádku (RLS) zjednodušuje návrh a psaní kódu zabezpečení v aplikaci. RLS umožňuje implementovat omezení přístupu k datovým řádkům. Například pro zajištění, že pracovníci mají přístup pouze řádky dat, které se vztahují k jejich oddělení, nebo pro omezení přístupu k datům jenom na relevantní data. 
- [Transparentní šifrování dat (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) šifruje datové soubory spravované Instance Azure SQL, označované jako šifruje neaktivní uložená data. Transparentní šifrování dat provádí v reálném čase vstupně-výstupní operace šifrování a dešifrování souborů protokolu a data. Šifrování používá databázi šifrovací klíč (DEK), která je uložena v spouštěcí záznam databáze dostupnosti během obnovení. Všechny databáze ve spravované instanci pomocí transparentního šifrování dat můžete chránit. TDE je prověřená technologie SQL pro šifrování v klidovém stavu, kterou vyžaduje řada standardů dodržování předpisů z důvodu ochrany před krádeží úložného média. Ve verzi public preview se podporuje model automatické správy klíčů (prováděnou platforma PaaS). 

Azure Database Migration Service (DMS) nebo nativní obnovení se podporuje migrace šifrované databázi do spravované Instance SQL. Pokud máte v plánu migrovat šifrované databázi pomocí nativní obnovení, se migrace stávajícího certifikátu transparentní šifrování dat z SQL serveru na místní nebo virtuální počítač s SQL serverem do spravované instance o požadovaný krok. Další informace o možnostech migrace najdete v tématu [migrace instance SQL serveru do Azure SQL Database Managed Instance](sql-database-managed-instance-migrate.md).

## <a name="azure-active-directory-integration"></a>Integrace Azure Active Directory

Azure SQL Database Managed Instance podporuje tradiční přihlášeních SQL server Database engine a přihlašování integrované s Azure Active Directory (AAD). Přihlašovací údaje AAD se Azure cloudovou verzi služby Windows přihlašovací jména databáze, které používáte ve vašem místním prostředí.

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Integrace s Azure Active Directory a vícefaktorové ověřování 

Managed Instance umožňuje centrálně spravovat identity uživatele databáze a dalším službám Microsoftu s [integrace služby Azure Active Directory](sql-database-aad-authentication.md). Tato možnost zjednodušuje správu oprávnění a zvyšuje zabezpečení. Azure Active Directory podporuje [vícefaktorové ověřování (MFA)](sql-database-ssms-mfa-authentication-configure.md) pro zvýšení zabezpečení dat a aplikací při současné podpoře jednotného přihlašování. 

### <a name="authentication"></a>Authentication 
Spravovaná Instance ověřování odkazuje na tom, jak uživatele prokazovat svoji identitu při připojování k databázi. SQL Database podporuje dva typy ověřování:  

- Ověřování SQL, které používá uživatelské jméno a heslo.
- Ověřování Azure Active Directory, které používá identity spravované v Azure Active Directory a je podporované u spravovaných a integrovaných domén. 

### <a name="authorization"></a>Autorizace

Autorizace určuje, jak uživatel provádět ve službě Azure SQL Database jsou dané členstvím v databázových rolích a oprávnění na úrovni objektů vašeho účtu. Managed Instance má stejné možnosti autorizace jako SQL Server 2017. 

## <a name="database-migration"></a>Migrace databáze 

Spravovaná Instance cíle uživatelských scénářů s migrací velkokapacitního databáze z on-premises nebo implementace databáze IaaS. Managed Instance podporuje několik možností pro migraci databáze: 

### <a name="backup-and-restore"></a>Zálohování a obnovení  

Postup migrace využívá zálohování SQL na úložiště objektů blob v Azure. Zálohy uložené v Azure storage blob je možné obnovit přímo do Managed Instance pomocí [příkaz T-SQL RESTORE](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql?view=azuresqldb-mi-current). 
  - Kurz ukazuje, jak obnovit Wide World Importers – Standard záložního souboru, najdete v tématu [obnovení zálohy do spravované Instance](sql-database-managed-instance-get-started-restore.md). Tento kurz ukazuje, jak nahrát soubor zálohy úložištěm objektů BLOB Azure a zabezpečené ho pomocí sdíleného přístupového podpisu (SAS) klíče.
  - Informace o obnovení z adresy URL, najdete v části [nativní obnovit z adresy URL](sql-database-managed-instance-migrate.md#native-restore-from-url).
  
### <a name="data-migration-service"></a>Data Migration Service

Azure Database Migration Service je plně spravovaná služba, která umožňují bezproblémovou migraci z několika databázových zdrojů na platformu Azure Data s minimálními výpadky. Tato služba zjednodušuje úlohy potřebné k migraci existujícího třetích stran a databáze SQL serveru do Azure. Možnosti nasazení patří Azure SQL Database Managed Instance a SQL Server na virtuálním počítači Azure ve verzi Public Preview. Zobrazit [jak migrovat místní databázi do Managed Instance pomocí DMS](https://aka.ms/migratetoMIusingDMS).

## <a name="sql-features-supported"></a>Funkce SQL podporované 

Managed Instance jeho účelem je poskytovat blízko povrchu 100 % kompatibilitu s místní SQL Server už ve fázi až do obecné dostupnosti služby. Pro funkce a seznam porovnání, naleznete v tématu [porovnání funkcí SQL Database](sql-database-features.md)a seznam rozdílů jazyka T-SQL ve spravovaných instancí oproti serveru SQL Server najdete v tématu [Managed Instance T-SQL rozdíl oproti serveru SQL Server](sql-database-managed-instance-transact-sql-information.md).
 
Managed Instance podporuje zpětná kompatibilita na databáze SQL 2008. Přímé migrace z databázové servery SQL 2005 je podporovaná, se aktualizují úroveň kompatibility pro migrované databáze SQL 2005 na SQL 2008. 
 
Následující diagram popisuje styčné plochy kompatibility ve spravované instanci:  

![Migrace](./media/sql-database-managed-instance/migration.png) 

### <a name="key-differences-between-sql-server-on-premises-and-managed-instance"></a>Hlavní rozdíly mezi místním SQL serverem a Managed Instance 

Managed Instance výhody z se vždycky nahoru – k datu v cloudu, což znamená, že některé funkce v místním SQL serveru může být buď zastaralé, vyřazený nebo mít alternativy. Existují zvláštní případy při nástroje třeba rozpoznat, že konkrétní funkce funguje trochu jinak nebo že služba není spuštěná v prostředí, které nebudete plně řídit: 

- Je součástí a předem nakonfigurované s vysokou dostupností. Funkce vysoké dostupnosti Always On se nezobrazí stejným způsobem, jak vypadá v implementacích SQL IaaS 
- Automatizované zálohování a bodu v čase. Zákazník může iniciovat `copy-only` zálohy, které nejsou v konfliktu s řetězec automatického zálohování. 
- Managed Instance nepovoluje zadání úplné fyzické cesty, takže všechny odpovídající scénáře a jinak není podporován: obnovení databáze nepodporuje se přesun, vytvořit DB neumožňuje fyzické cesty, BULK INSERT funguje s objekty BLOB Azure pouze atd. 
- Managed Instance podporuje [ověřování Azure AD](sql-database-aad-authentication.md) jako cloudové alternativu k ověřování Windows. 
- Spravovaná Instance automaticky spravuje skupiny souborů XTP a soubory pro databáze, které obsahují objekty OLTP v paměti
- Podporuje spravovanou instanci SQL Server Integration Services (SSIS) a můžete hostitele katalogu služby SSIS (SSISDB), která ukládá balíčků služby SSIS, ale jsou prováděna na spravované prostředí Azure-SSIS Integration Runtime (IR) v Azure Data Factory (ADF), najdete v článku [Create Prostředí Azure-SSIS IR ve službě ADF](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). Porovnání funkcí služby SSIS v SQL Database a Managed Instance, najdete v článku [porovnání SQL Database a spravované Instance (Preview)](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-and-managed-instance-preview).

### <a name="managed-instance-administration-features"></a>Funkce správy spravované Instance  

Managed Instance povolení správce systému a zaměřte se na to, co je nejdůležitější pro firmy. Řada aktivit Správce/DBA systému nejsou vyžadovány, nebo se dají snadno. Například operačního systému nebo RDBMS instalace a používání dílčích oprav, dynamické instanci změny velikosti a konfigurace, zálohování, replikace databáze (včetně systémové databáze), konfigurace vysoké dostupnosti a konfiguraci stavu a výkonu monitorování dat datové proudy. 

> [!IMPORTANT]
> Seznam podporovaných, částečně podporované a nepodporované funkce najdete v tématu [funkcí služby SQL Database](sql-database-features.md). Seznam rozdílů jazyka T-SQL ve spravovaných instancí oproti serveru SQL Server najdete v tématu [Managed Instance T-SQL rozdíl oproti serveru SQL Server](sql-database-managed-instance-transact-sql-information.md)

### <a name="how-to-programmatically-identify-a-managed-instance"></a>Jak prostřednictvím kódu programu identifikovat Managed Instance

V následující tabulce jsou uvedeny několik vlastností, které jsou přístupné prostřednictvím jazyka Transact SQL, můžete použít ke zjištění, že vaše aplikace pracuje s Managed Instance a načíst důležité vlastnosti.

|Vlastnost|Hodnota|Poznámka|
|---|---|---|
|`@@VERSION`|Microsoft SQL Azure (RTM) - 2018-03-07 12.0.2000.8 Copyright (C) 2018 Microsoft Corporation.|Tato hodnota je stejné jako u databáze SQL.|
|`SERVERPROPERTY ('Edition')`|SQL Azure|Tato hodnota je stejné jako u databáze SQL.|
|`SERVERPROPERTY('EngineEdition')`|8|Tato hodnota jednoznačně identifikuje Managed Instance.|
|`@@SERVERNAME`, `SERVERPROPERTY ('ServerName')`|Název DNS úplná instance v následujícím formátu:<instanceName>.<dnsPrefix>. Database.Windows.NET, kde <instanceName> je poskytnutá výhradně zákazník, zatímco <dnsPrefix> je automaticky generovanou částí názvu zaručující globální jedinečnost názvu DNS ("wcus17662feb9ce98", například)|Příklad: Moje managed-instance.wcus17662feb9ce98.database.windows.net|

## <a name="next-steps"></a>Další postup

- Zjistěte, jak vytvořit první Managed Instance, najdete v článku [příručky rychlý start](sql-database-managed-instance-get-started.md).
- Pro funkce a seznam porovnání, naleznete v tématu [běžné funkce SQL](sql-database-features.md).
- Další informace o konfiguraci virtuální sítě najdete v tématu [Konfigurace virtuální sítě pro Managed Instance](sql-database-managed-instance-vnet-configuration.md).
- Kurz, který vytváří Managed Instance a obnoví databázi ze zálohy, najdete v tématu [vytvoříte Managed Instance](sql-database-managed-instance-get-started.md).
- Kurz migrace pomocí Azure Database Migration Service (DMS) najdete v tématu věnovaném [migraci Managed Instance pomocí DMS](../dms/tutorial-sql-server-to-managed-instance.md).
- Informace o cenách najdete v tématu [ceny SQL Database Managed Instance](https://azure.microsoft.com/pricing/details/sql-database/managed/).
