---
title: Provozní kontinuita v cloudu – obnovení databází – SQL Database | Dokumentace Microsoftu
description: Zjistěte, jak Azure SQL Database podporuje provozní kontinuitu v cloudu a obnovení databází a jak pomáhá udržovat klíčové cloudové aplikace v chodu.
keywords: provozní kontinuita, provozní kontinuita v cloudu, zotavení databáze po havárii, obnovení databáze
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.workload: On Demand
ms.date: 07/25/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: ce0684f9ab06b5362ccdf25aeaff15ea668ce96c
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/22/2018
ms.locfileid: "42444144"
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Přehled provozní kontinuity se službou Azure SQL Database

Azure SQL Database je implementace nejnovější stabilní verze databázového stroje SQL Server nakonfigurovaný a optimalizované pro Azure cloudové prostředí, které poskytuje [vysoké dostupnosti](sql-database-high-availability.md) a odolnost proti chybám, které by mohly ovlivnit vaši obchodních procesů. **Kontinuita podnikových procesů** ve službě Azure SQL Database odkazuje na mechanismy, zásady a postupy, které umožňují obchodní pokračovat i v případě přerušení, zejména pro její výpočetní infrastrukturu.  Ve většině případů Azure SQL Database bude zpracovávat rušivé události, které může dojít v cloudovém prostředí a udržovat vaše obchodní procesy spuštění. Existují však některé rušivé události, které nemohou být zpracovány SQL Database jako:
 - Uživatel omylem odstranit nebo aktualizovat řádek v tabulce.
 - Útočník úspěšně odstranit data nebo odstranit databázi.
 - Zemětřesení způsobila výpadku a dočasné zakázané datového centra.
 
Tyto případy nemůže ovlivnit služba Azure SQL Database, bylo by proto nutné použít funkce provozní kontinuity ve službě SQL Database, která umožňuje obnovení dat a vaše aplikace spuštěná.

Tento přehled popisuje možnosti, které služba Azure SQL Database nabízí pro zajištění provozní kontinuity a zotavení po havárii. Další informace o možnosti, doporučení a kurzy pro zotavení z ničivých událostí, které by mohly způsobit ztrátu dat nebo nedostupnost databáze a aplikace přestanou být dostupné. Zjistěte, co můžete dělat, když chyba uživatele nebo aplikace ovlivňuje integritu dat, má k výpadku oblasti Azure nebo aplikace vyžaduje údržbu.

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>Funkce služby SQL Database, pomocí kterých můžete zajistit provozní kontinuitu

Z hlediska databáze existují čtyři hlavní scénáře potenciální přerušení:
- **Místní selhání hardwaru nebo softwaru** by to ovlivnilo uzel databáze, jako je například selhání disku.
- **Poškození dat nebo odstranění** – obvykle způsobené chybě aplikace nebo lidské chyby.  Taková selhání jsou vnitřně specifické pro aplikaci a nelze jako pravidlo zjistit nebo automaticky omezeny infrastrukturu.
- **Výpadku datového centra**, může být způsobené přírodní katastrofě.  Tento scénář vyžaduje určitou úroveň geografické redundance pomocí aplikace převzetí služeb při selhání na alternativních datové centrum.
- **Upgrade nebo údržbu chyby** – neočekávané problémy, ke kterým dochází při plánované aktualizace nebo údržby tak, aby aplikace nebo databáze může vyžadovat rychlé vrácení zpět do stavu předchozího databáze.

SQL Database nabízí několik funkce provozní kontinuity, včetně automatizovaných záloh a volitelné replikace databáze, která lze zmírnit tyto scénáře. Nejprve je třeba pochopit, jak SQL Database [architektura pro vysokou dostupnost](sql-database-high-availability.md) zajišťuje 99,99 % dostupnost a odolnost proti chybám pro některé rušivé události, které by mohly ovlivnit obchodních procesů.
Potom se dozvíte další mechanismy, které můžete použít k zotavení z ničivých událostí, které nemohou být zpracovány architektura pro vysokou dostupnost databáze SQL, jako například:
 - [Dočasné tabulky](sql-database-temporal-tables.md) vám umožní obnovit verze řádků z libovolného bodu v čase.
 - [Integrované automatické zálohování](sql-database-automated-backups.md) a [obnovení k časovému okamžiku](sql-database-recovery-using-backups.md#point-in-time-restore) umožňuje obnovit kompletní databáze do určitého bodu v čase za posledních 35 dnů.
 - Je možné [obnovení odstraněné databáze](sql-database-recovery-using-backups.md#deleted-database-restore) do bodu, ve kterém byl odstraněn, pokud **logický server, nebyla Odstraněná**.
 - [Dlouhodobé uchovávání záloh](sql-database-long-term-retention.md) umožňuje držet krok zálohy na 10 let.
 - [Geografická replikace](sql-database-geo-replication-overview.md) umožňuje, aby aplikace k provádění rychlé zotavení po havárii v případě výpadku datového centra škálování.

Každá má jiné vlastnosti ohledně odhadovaného času obnovení (ERT) a potenciální ztráty dat posledních transakcí. Jakmile tyto možnosti pochopíte, můžete si mezi nimi vybírat a ve většině scénářů je spolu kombinovat a používat pro různé scénáře. Při vývoji plánu provozní kontinuity musíte pochopit maximální přijatelnou dobu, než úplného obnovení aplikace po ničivé události. Čas potřebný pro aplikaci, aby se úplně zotavily se označuje jako plánovaná doba obnovení (RTO). Také musíte pochopit maximální období posledních aktualizací dat (časový interval) aplikace může tolerovat možnost, ztráty při obnovení po ničivé události. Časové období aktualizací, které si může dovolit přijít o se označuje jako cíl bodu obnovení (RPO).

Následující tabulka porovnává ERT a RPO pro každou vrstvu služby pro tři nejběžnější scénáře.

| Schopnost | Basic | Standard | Premium  | Obecné použití | Pro důležité obchodní informace
| --- | --- | --- | --- |--- |--- |
| Obnovení k určitému bodu v čase ze zálohy |Libovolný bod obnovení do 7 dní |Libovolný bod obnovení do 35 dní |Libovolný bod obnovení do 35 dní |Libovolný bod obnovení v rámci nakonfigurované doby (až po 35 dnů)|Libovolný bod obnovení v rámci nakonfigurované doby (až po 35 dnů)|
| Geografické obnovení z geograficky replikovaných záloh |ERT < 12 h, RPO < 1 h |ERT < 12 h, RPO < 1 h |ERT < 12 h, RPO < 1 h |ERT < 12 h, RPO < 1 h|ERT < 12 h, RPO < 1 h|
| Obnovení z dlouhodobě uchovávaných SQL |ERT < 12 h, RPO < 1 týdnem |ERT < 12 h, RPO < 1 týdnem |ERT < 12 h, RPO < 1 týdnem |ERT < 12 h, RPO < 1 týdnem|ERT < 12 h, RPO < 1 týdnem|
| Aktivní geografická replikace |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s |ERT < 30 s, RPO < 5 s|ERT < 30 s, RPO < 5 s|

## <a name="recover-a-database-to-the-existing-server"></a>Obnovení databáze do existujícího serveru

SQL Database automaticky provádí kombinaci zálohování úplné databáze každý týden, rozdílovými zálohami prováděnými každou hodinu a transakce zálohu protokolu každých 5 – 10 minut na chránit vaši firmu před ztrátou dat. Zálohy jsou uložené v úložišti RA-GRS po dobu 35 dní pro všechny úrovně služby s výjimkou základních jednotek DTU úrovně služeb ukládat zálohy po dobu 7 dní. Další informace najdete v tématu [automatické zálohování databází](sql-database-automated-backups.md). Můžete obnovit existující databáze formulář automatizovaných záloh k dřívějšímu bodu v čase jako novou databázi na stejném logickém serveru pomocí webu Azure portal, Powershellu nebo rozhraní REST API. Další informace najdete v tématu [v daném okamžiku obnovení](sql-database-recovery-using-backups.md#point-in-time-restore).

Pokud maximální podporované v daném okamžiku obnovit (PITR) doba uchovávání není pro vaši aplikaci dostatečná, můžete ji rozšířit konfigurací zásad dlouhodobého uchovávání (LTR) pro databáze. Další informace najdete v tématu [dlouhodobého uchovávání záloh](sql-database-long-term-retention.md).

Tyto automatické zálohy databáze můžete použít k obnovení databáze po různých ničivých událostech, a to jak v rámci vašeho datového centra, tak do jiného datového centra. Při použití automatických záloh databáze závisí odhadovaný čas obnovení na několika faktorech. Patří mezi ně celkový počet obnovovaných databází ve stejném regionu a ve stejnou dobu, velikost databáze, velikost protokolu transakcí a šířka pásma sítě. Čas obnovení je obvykle méně než 12 hodin. Může trvat delší dobu velmi velký nebo aktivní databázi obnovit. Další informace o čase obnovení najdete v tématu [databáze čas obnovení](sql-database-recovery-using-backups.md#recovery-time). Při obnovování do jiné oblasti dat je potenciální ztráta dat omezena na 1 hodinu díky geograficky redundantnímu úložišti s rozdílovými zálohami prováděnými každou hodinu.

Automatizované zálohování použijte a [obnovení k určitému bodu v čase](sql-database-recovery-using-backups.md#point-in-time-restore) jako obchodní kontinuity podnikových procesů a zotavení mechanismus Pokud vaše aplikace:

* Není považována za klíčovou.
* Nemá zavazující smlouvu SLA – výpadek 24 hodin nebo již nemá za následek finanční závazky.
* Pracuje s nízkou mírou změn dat (málo transakcí za hodinu) a ztráta změn provedených během až jedné hodiny je přijatelnou ztrátou dat.
* Je citlivá na změny nákladů.

Pokud potřebujete rychlejší obnovení, použijte [aktivní geografickou replikaci](sql-database-geo-replication-overview.md) (věnujeme se jí). Pokud potřebujete mít možnost obnovit data z období staršího než 35 dní, použijte [dlouhodobé uchovávání](sql-database-long-term-retention.md). 

## <a name="recover-a-database-to-another-region"></a>Obnovení databáze do jiné oblasti
<!-- Explain this scenario -->

Přestože je taková situace výjimečná, i u datového centra Azure může dojít k výpadku. Při výpadku dojde k narušení provozu, které může trvat jen několik minut nebo až několik hodin.

* Jednou z možností je počkat, až výpadek skončí a databáze se vrátí do režimu online. Tento postup funguje pro aplikace, které si mohou dovolit mít databázi v režimu offline. Například vývojový projekt nebo bezplatná zkušební verze, na které nemusíte neustále pracovat. Pokud datové centrum má k výpadku, můžete není známo, jak dlouho může trvat výpadek, proto tato možnost funguje jenom v případě nepotřebujete databázi nějakou dobu.
* Další možností je k obnovení databáze na libovolném serveru v libovolné oblasti Azure pomocí [geograficky redundantních záloh databáze](sql-database-recovery-using-backups.md#geo-restore) (geografické obnovení). Geografické obnovení pomocí geograficky redundantní zálohy jako zdroj a slouží k obnovení databáze, i když je nejsou dostupné kvůli výpadku databáze nebo datového centra.
* Nakonec můžete rychle zvýšit úroveň sekundárního v jiné oblasti dat na primární (také nazývané převzetí služeb při selhání) a konfigurace aplikací pro připojení k primární přesunutá, pokud používáte aktivní geografickou replikaci. Můžou existovat menšího množství ztráty dat posledních transakcí kvůli povaze asynchronní replikace. Pomocí skupin automatického – převzetí služeb při selhání, můžete upravit zásady převzetí služeb při selhání minimalizovat potenciální ztráty dat. Ve všech případech se uživatelé setkají s krátkým výpadkem a budou se muset znovu připojit. Převzetí služeb při selhání trvá jenom pár sekund při obnovení databáze ze záloh trvá hodiny.

Aby bylo možné převzetí služeb při selhání do jiné oblasti, můžete použít [aktivní geografickou replikaci](sql-database-geo-replication-overview.md) nakonfigurovat databázi, aby měla až čtyři čitelné sekundární databáze v oblastech podle vašeho výběru. Tyto sekundární databáze jsou synchronizovány s primární databází pomocí mechanismu asynchronní replikace. 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>


> [!IMPORTANT]
> Pokud chcete použít aktivní geografickou replikaci a skupiny – automatické převzetí služeb při selhání, musí být vlastníkem předplatného nebo mít oprávnění správce v systému SQL Server. Můžete nakonfigurovat a převzetí služeb při selhání pomocí Azure portal, Powershellu nebo rozhraní REST API použití oprávnění pro předplatné Azure nebo pomocí příkazů jazyka Transact-SQL s oprávněními systému SQL Server.
> 

Tato funkce slouží k ochraně před narušením provozu, pokud dojde k výpadku datového centra nebo během upgradu aplikace. Povolit automatické a transparentní převzetí služeb při selhání geograficky replikované databáze by měl uspořádat do skupin pomocí [-automatické převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md) funkce služby SQL Database. Aktivní geografická replikace a Automatická – převzetí služeb při selhání skupiny použijte, pokud vaše aplikace splňuje některá z těchto kritérií:

* Je zvlášť důležitá.
* Má smlouvu SLA, která nepovoluje výpadek delší než 24 hodin.
* Výpadek může mít za následek finanční závazky.
* Pracuje s vysokou mírou změn dat a ztráta dat za jednu hodinu je nepřijatelná.
* Další náklady na aktivní geografickou replikaci jsou nižší než potenciální finanční závazky a související ztráta podnikání.

Když přijmete opatření, jak dlouho trvá, vám umožní obnovit a množství ztracených dat v závisí na tom, jak se rozhodnete pomocí těchto funkcí provozní kontinuity ve vaší aplikaci. Ve skutečnosti můžete rozhodnout pro použití kombinace záloh databáze a aktivní geografickou replikaci v závislosti na požadavcích aplikace. Diskuzi o aspektech návrhu aplikací pro samostatné databáze a pro elastické fondy pomocí těchto funkcí provozní kontinuity najdete v tématech [Návrh aplikace pro zotavení po havárii cloudu](sql-database-designing-cloud-solutions-for-disaster-recovery.md) a [Strategie zotavení po havárii elastických fondů](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

Následující části poskytují přehled postupů k obnovení pomocí záloh databáze nebo aktivní geografické replikace. Podrobné pokyny, včetně plánování požadavky, postupů po obnovení a informace o simulaci výpadku provedení postupu zotavení po havárii, najdete v článku [obnovení služby SQL Database po výpadku](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Příprava na výpadek
Bez ohledu na funkce provozní kontinuity, které používáte, musíte:

* Určit a připravit cílový server, včetně pravidel brány firewall na úrovni serveru, přihlášení a oprávnění na úrovni hlavní databáze
* Určit, jak přesměrovat klienty a klientské aplikace na nový server
* Zdokumentovat další závislosti, například nastavení auditování a výstrahy

Pokud jste není nepřipravíte, může převod aplikací online po převzetí služeb při selhání nebo obnovení databáze bude vyžadovat čas navíc a pravděpodobně také vyžadovat řešení potíží ve vypjaté situaci - dobrá kombinace.

### <a name="fail-over-to-a-geo-replicated-secondary-database"></a>Převzetí služeb při selhání do geograficky replikované sekundární databáze
Pokud používáte jako mechanismus obnovení aktivní geografickou replikaci a skupiny – automatické převzetí služeb při selhání, můžete nakonfigurovat zásadu automatického převzetí služeb při selhání nebo použít [ruční převzetí služeb při selhání](sql-database-disaster-recovery.md#fail-over-to-geo-replicated-secondary-server-in-the-failover-group). Po zahájení převzetí služeb způsobí, že sekundární k nové primární a připravená zaznamenávat nové transakce a reagovat na dotazy – minimální ztráty dat ještě nebyl replikován. Informace o návrhu procesu převzetí služeb při selhání najdete v tématu [návrh aplikace pro zotavení po havárii cloudu](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> Pokud datové centrum vrátí do režimu online staré primárek automaticky znovu připojit k nové primární a k sekundární databází. Pokud budete potřebovat k přesunutí primární zpět do původní oblasti, můžete spustit plánované převzetí služeb při selhání ručně (navrácení služeb po obnovení). 
> 

### <a name="perform-a-geo-restore"></a>Provedení geografického obnovení
Pokud používáte automatizované zálohování s replikací geograficky redundantního úložiště jako mechanismus obnovení, [zahajte obnovení databáze pomocí geografického obnovení](sql-database-disaster-recovery.md#recover-using-geo-restore). K obnovení obvykle dojde během 12 hodin – se ztrátou dat až za jednu hodinu v závislosti na času pořízení a replikace poslední hodinové rozdílové zálohy. Dokud se obnovení nedokončí, databáze není schopná zaznamenávat žádné transakce ani reagovat na dotazy. Při obnovení databáze na poslední dostupný bod v čase, obnovení geo-secondary do libovolného bodu v čase se momentálně nepodporuje.

> [!NOTE]
> Pokud datové centrum vrátí do režimu online předtím, než přepnete aplikace k obnovené databázi, můžete zrušit obnovení.  
>
>

### <a name="perform-post-failover--recovery-tasks"></a>Provedení úloh po převzetí služeb při selhání nebo obnovení
Po obnovení s použitím libovolného mechanismu musíte provést následující dodatečné úlohy, abyste pro uživatele zprovoznili své aplikace:

* Přesměrujte klienty a klientské aplikace na nový server a obnovenou databázi.
* Ujistěte se, že platí odpovídající pravidla brány firewall na úrovni serveru, aby se uživatelé mohli připojit (nebo použijte [brány firewall na úrovni databáze](sql-database-firewall-configure.md#creating-and-managing-firewall-rules)).
* Ujistěte se, že se používají odpovídající přihlášení a oprávnění na úrovni hlavní databáze (nebo použijte [obsažené uživatelé](https://msdn.microsoft.com/library/ff929188.aspx))
* Podle potřeby nakonfigurujte auditování.
* Podle potřeby nakonfigurujte výstrahy.

## <a name="upgrade-an-application-with-minimal-downtime"></a>Upgrade aplikace s minimálními výpadky
Někdy aplikace musí být převedeno do režimu offline kvůli plánované údržbě, jako je upgrade aplikace. [Správa upgradů aplikací](sql-database-manage-application-rolling-upgrade.md) popisuje, jak pomocí aktivní geografické replikace povolit postupné upgrady cloudových aplikací, abyste minimalizovali prostoje během upgradu a zadejte cestu k obnovení, pokud se něco nepovede. 

## <a name="next-steps"></a>Další postup
Diskuzi o aspektech návrhu aplikací pro samostatné databáze a pro elastické fondy najdete v tématech [Návrh aplikace pro zotavení po havárii cloudu](sql-database-designing-cloud-solutions-for-disaster-recovery.md) a [Strategie zotavení po havárii elastických fondů](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
