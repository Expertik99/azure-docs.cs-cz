---
title: Logické servery Azure SQL | Dokumentace Microsoftu
description: Další informace o logického serveru Azure SQL Database a jejich správu.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 09/07/2018
ms.author: carlrab
ms.openlocfilehash: f30d15c70b33200565f1933a4d8ebe8ed654638c
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44055229"
---
# <a name="azure-sql-database-logical-servers-and-their-management"></a>Logické servery Azure SQL Database a jejich správa

## <a name="what-is-an-azure-sql-logical-server"></a>Co je logický server Azure SQL?

Logický server funguje jako centrální administrativní bod pro více jedním nebo [ve fondu](sql-database-elastic-pool.md) databází, [přihlášení](sql-database-manage-logins.md), [pravidla brány firewall](sql-database-firewall-configure.md), [auditování pravidla](sql-database-auditing.md), [zásad detekce hrozeb](sql-database-threat-detection.md), a [skupiny převzetí služeb při selhání](sql-database-geo-replication-overview.md). Logický server může být v jiné oblasti než skupinou prostředků. Logický server, musí existovat předtím, než budete moct vytvořit databázi Azure SQL. Všechny databáze na serveru se vytvoří ve stejné oblasti jako logický server.

Logický server je logická konstrukce, která se liší od instance SQL serveru, který je pravděpodobně znáte místního prostředí. Služba SQL Database zejména neposkytuje žádnou záruku ohledně umístění databází ve vztahu k jejich logickým serverům a nezveřejňuje žádné funkce ani možnosti přístupu na úrovni instance. Naproti tomu serveru do SQL Database Managed Instance je podobný instanci SQL serveru, který je pravděpodobně znáte místního prostředí.

Když vytvoříte logický server, zadejte server přihlašovací účet a heslo, které má oprávnění správce pro hlavní databázi na daném serveru a všech databází na tomto serveru vytvořit. Tento počáteční účet je účet přihlášení SQL. Azure SQL Database podporuje ověřování SQL a ověřování Azure Active Directory pro ověřování. Informace o přihlašování a ověřování najdete v tématu [Správa databází a přihlášení ve službě Azure SQL Database](sql-database-manage-logins.md). Ověřování systému Windows se nepodporuje. 

Logický server Azure Database:

- Je vytvořen v rámci předplatného Azure, ale je možné ho i s obsaženými prostředky přenést do jiného předplatného.
- Je nadřazeným prostředkem pro databáze, elastické fondy a datové sklady.
- Poskytuje obor názvů pro databáze, elastické fondy a datové sklady
- Je logický kontejner se silnou sémantikou životního cyklu – odstraníte server a odstraní databáze s omezením, elastické fondy a datové sklady
- Účastní se [řízení přístupu Azure na základě rolí (RBAC)](/azure/role-based-access-control/overview) – databáze, elastické fondy a datové sklady v rámci serveru dědí přístupová práva od serveru
- Je nejvyšším prvkem identity databází, elastické fondy a datové sklady pro prostředky Azure (viz schéma adresy URL pro databáze a fondy) účely správy
- Uspořádává prostředky v oblasti.
- Poskytuje koncový bod připojení pro přístup k databázi (<serverName>.database.windows.net).
- Poskytuje přístup k metadatům, která se vztahují k obsaženým prostředkům, přes zobrazení dynamických zpráv díky připojení k hlavní databázi. 
- Poskytuje obor pro zásady správy, které se vztahují na jeho databáze - přihlášení, brána firewall, auditování, analýzy hrozeb, zjišťování atd. 
- Je omezený kvótou nadřazeného předplatného (šest serverů na předplatné, ve výchozím nastavení - [viz limity předplatného](../azure-subscription-service-limits.md))
- Poskytuje obor pro kvóty databáze a DTU nebo vCore kvóty pro prostředky, které obsahuje (jako je například 45 000 DTU)
- Je oborem správy verzí pro možnosti povolené u obsažených prostředků 
- Hlavní přihlášení na úrovni serveru můžou spravovat všechny databáze na serveru.
- Může obsahovat přihlašovací údaje podobné těm, která se místně používají v instancích SQL Serveru, a udělit jim přístup k jedné nebo několika databázím na serveru spolu s omezenými právy pro správu. Další informace najdete v tématu [Přihlašovací údaje](sql-database-manage-logins.md).
- Je výchozí kolaci pro všechny uživatele databáze vytvořené na logickém serveru `SQL_LATIN1_GENERAL_CP1_CI_AS`, kde `LATIN1_GENERAL` je angličtina (Spojené státy), `CP1` je znakovou sadu 1252, `CI` je velká a malá písmena, a `AS` je diakritiky.

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-portal"></a>Spravovat servery Azure SQL, databáze a brány firewall s využitím webu Azure portal

Můžete vytvořit skupinu prostředků Azure SQL database předem nebo při vytváření samotný server. Existuje několik metod pro získání nového formuláře SQL serveru tak, že vytvoříte nový SQL server nebo jako součást vytváření nové databáze. 

### <a name="create-a-blank-sql-server-logical-server"></a>Vytvoření prázdné SQL serveru (logický server)

K vytvoření serveru (bez databáze) databáze SQL Azure pomocí [webu Azure portal](https://portal.azure.com), přejděte na prázdný formulář SQL server (logický server).  

### <a name="create-a-blank-or-sample-sql-database"></a>Vytvoření prázdné nebo ukázková databáze SQL

K vytvoření databáze Azure SQL pomocí [webu Azure portal](https://portal.azure.com), přejděte na prázdný formulář databáze SQL a zadejte požadované informace. Můžete vytvořit skupinu prostředků a logický server předem nebo při vytváření samotná databáze Azure SQL database. Můžete vytvořit prázdnou databázi nebo vytvoření ukázkové databáze založené na Adventure Works LT. 

  ![create database-1](./media/sql-database-get-started-portal/create-database-1.png)

> [!IMPORTANT]
> Další informace o výběru cenová úroveň pro vaši databázi, naleznete v tématu [nákupní model založený na DTU](sql-database-service-tiers-dtu.md) a [nákupní model založený na virtuálních jádrech](sql-database-service-tiers-vcore.md).

Vytvoření Managed Instance najdete v tématu [vytvoříte Managed Instance](sql-database-managed-instance-get-started.md)

### <a name="manage-an-existing-sql-server"></a>Správa existujícího serveru SQL

Chcete-li spravovat existující server, přejděte na server pomocí několika metod – jako třeba konkrétní stránce databáze SQL, **SQL servery** stránky, nebo **všechny prostředky** stránky. 

Chcete-li spravovat stávající databázi, přejděte na **databází SQL** stránky a klikněte na databázi, kterou chcete spravovat. Následující snímek obrazovky ukazuje, jak začít, nastavení brány firewall úrovni serveru pro databázi z **přehled** stránku pro databázi. 

   ![pravidlo brány firewall serveru](./media/sql-database-get-started-portal/server-firewall-rule.png) 

> [!IMPORTANT]
> Postup konfigurace vlastností výkonu u databáze, najdete v článku [nákupní model založený na DTU](sql-database-service-tiers-dtu.md) a [nákupní model založený na virtuálních jádrech](sql-database-service-tiers-vcore.md).
>

> [!TIP]
> Azure portal rychlém startu najdete v části [vytvořit databázi Azure SQL na webu Azure Portal](sql-database-get-started-portal.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-powershell"></a>Spravovat servery Azure SQL, databáze a brány firewall pomocí Powershellu

K vytváření a správě serveru Azure SQL, databáze a brány firewall pomocí Azure Powershellu, použijte následující rutiny Powershellu. Pokud potřebujete instalaci nebo upgrade prostředí PowerShell, najdete v článku [instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps). Vytváření a správa elastických fondů najdete v tématu [elastické fondy](sql-database-elastic-pool.md).

| Rutina | Popis |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Vytvoří databázi |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Získá jednu nebo více databází|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Nastaví vlastnosti pro databáze nebo přesune databázi do elastického fondu|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Odebere databázi|
|[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)|Vytvoří skupinu prostředků|
|[New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver)|Vytvoří server|
|[Get-AzureRmSqlServer](/powershell/module/azurerm.sql/get-azurermsqlserver)|Vrátí informace o serverech|
|[Set-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlserver)|Upraví vlastnosti serveru|
|[Remove-AzureRmSqlServer](/powershell/module/azurerm.sql/remove-azurermsqlserver)|Odebere server|
|[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule)|Vytvoří pravidlo brány firewall na úrovni serveru |
|[Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule)|Získá pravidla brány firewall pro server|
|[Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule)|Upraví pravidla brány firewall na serveru|
|[Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule)|Odstraní pravidlo brány firewall ze serveru.|
| New-AzureRmSqlServerVirtualNetworkRule | Vytvoří [ *pravidlo virtuální sítě*](sql-database-vnet-service-endpoint-rule-overview.md)založená na podsíť, která je koncový bod služby virtuální sítě. |

> [!TIP]
> Prostředí PowerShell rychlém startu najdete v části [vytvoření izolované databáze Azure SQL pomocí Powershellu](sql-database-get-started-portal.md). Příklady skriptů Powershellu, najdete v části [použití Powershellu k vytvoření izolované databáze Azure SQL a konfigurace pravidla brány firewall](scripts/sql-database-create-and-configure-database-powershell.md) a [sledování a škálování jednoho SQL database s využitím Powershellu](scripts/sql-database-monitor-and-scale-database-powershell.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-cli"></a>Spravovat servery Azure SQL, databáze a brány firewall pomocí Azure CLI

K vytváření a správě serveru Azure SQL, databáze a brány firewall se [rozhraní příkazového řádku Azure](/cli/azure), použijte následující [databáze SQL Azure CLI](/cli/azure/sql/db) příkazy. Rozhraní příkazového řádku můžete spustit v prohlížeči pomocí [Cloud Shellu](/azure/cloud-shell/overview) nebo [nainstalovat](/cli/azure/install-azure-cli) v systémech macOS, Linux nebo Windows. Vytváření a správa elastických fondů najdete v tématu [elastické fondy](sql-database-elastic-pool.md).

| Rutina | Popis |
| --- | --- |
|[az sql db create](/cli/azure/sql/db#az_sql_db_create) |Vytvoří databázi|
|[AZ sql db list](/cli/azure/sql/db#az_sql_db_list)|Obsahuje seznam všech databází a datových skladů na serveru, nebo všechny databáze v elastickém fondu|
|[AZ sql db list-editions](/cli/azure/sql/db#az_sql_db_list_editions)|Seznamy dostupnou službu cíle a omezení úložiště|
|[AZ sql db list-usages](/cli/azure/sql/db#az_sql_db_list_usages)|Vrací databáze využití|
|[AZ sql db show](/cli/azure/sql/db#az_sql_db_show)|Získá databázi ani na datový sklad|
|[az sql db update](/cli/azure/sql/db#az_sql_db_update)|Aktualizace databáze|
|[AZ sql db delete](/cli/azure/sql/db#az_sql_db_delete)|Odebere databázi|
|[az group create](/cli/azure/group#az_group_create)|Vytvoří skupinu prostředků|
|[az sql server create](/cli/azure/sql/server#az_sql_server_create)|Vytvoří server|
|[AZ sql server list](/cli/azure/sql/server#az_sql_server_list)|Vytvoří seznam serverů|
|[AZ sql server list-usages](/cli/azure/sql/server#az_sql_server_list_usages)|Vrátí použití serveru|
|[AZ sql server show](/cli/azure/sql/server#az_sql_server_show)|Získá serveru|
|[aktualizace az sql server](/cli/azure/sql/server#az_sql_server_update)|Aktualizace serveru|
|[AZ sql server delete](/cli/azure/sql/server#az_sql_server_delete)|Odstraní server|
|[Vytvoření az sql server firewall-rule](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create)|Vytvoří pravidlo brány firewall serveru|
|[AZ sql server firewall-rule list](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_list)|Obsahuje seznam pravidel brány firewall na serveru|
|[AZ sql server firewall-rule show](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_show)|Zobrazí podrobnosti pravidla brány firewall|
|[AZ sql server firewall-rule update](/cli/azure/sql/server/firewall-rule##az_sql_server_firewall_rule_update)|Aktualizuje pravidlo brány firewall|
|[AZ sql server firewall-rule delete](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_delete)|Odstraní pravidlo brány firewall|

> [!TIP]
> Rychlý start Azure CLI najdete v části [vytvoření izolované databáze Azure SQL pomocí Azure CLI](sql-database-cli-samples.md). Příklad skripty rozhraní příkazového řádku Azure, najdete v části [pomocí rozhraní příkazového řádku k vytvoření izolované databáze Azure SQL a konfigurace pravidla brány firewall](scripts/sql-database-create-and-configure-database-cli.md) a [pomocí rozhraní příkazového řádku pro monitorování a škálování izolované databáze SQL](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-transact-sql"></a>Spravovat servery Azure SQL, databáze a brány firewall pomocí příkazů jazyka Transact-SQL

K vytváření a správě serveru Azure SQL, databáze a brány firewall pomocí příkazů jazyka Transact-SQL, použijte následující příkazy T-SQL. Můžete použít tyto příkazy pomocí webu Azure portal, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), nebo jiný program, který můžete připojit k serveru Azure SQL Database a předat příkazů jazyka Transact-SQL příkazy. Elastické fondy, přečtěte si téma [elastické fondy](sql-database-elastic-pool.md).

> [!IMPORTANT]
> Nejde vytvořit nebo odstranit server pomocí příkazů jazyka Transact-SQL.
>

| Příkaz | Popis |
| --- | --- |
|[Vytvoření databáze (Azure SQL Database)](/sql/t-sql/statements/create-database-azure-sql-database)|Vytvoří novou databázi. Musíte být připojení k hlavní databázi, a vytvořit novou databázi.|
| [Příkaz ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Upraví databázi Azure SQL. |
|[Příkaz ALTER DATABASE (Azure SQL Data Warehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Upraví službu Azure SQL Data Warehouse.|
|[DROP DATABASE (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Odstraní databázi.|
|[sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Vrátí edition (úroveň služby), cíl služby (cenová úroveň) a název elastického fondu, pokud existují pro službu Azure SQL database nebo Azure SQL Data Warehouse. Pokud přihlášení k hlavní databázi na serveru Azure SQL Database, vrátí informace ve všech databázích. Pro službu Azure SQL Data Warehouse musí být připojené k hlavní databázi.|
|[Sys.dm_db_resource_stats (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Vrátí spotřeby procesoru, vstupně-výstupní operace a paměti pro databázi Azure SQL Database. Jeden řádek existuje pro každých 15 sekund, i v případě, že neexistuje žádná aktivita v databázi.|
|[Sys.resource_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|Vrátí data o využití a úložiště CPU pro službu Azure SQL Database. Data se shromažďují a agregují v pětiminutovém intervalu.|
|[Sys.database_connection_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|Obsahuje statistiku pro události připojení databáze SQL Database, nabízí přehled databáze připojení úspěchy a selhání. |
|[Sys.event_log (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Vrátí úspěšná připojení databáze Azure SQL Database, selhání připojení a zablokování. Tyto informace můžete použít ke sledování nebo řešení potíží s aktivitu své databáze s využitím SQL Database.|
|[příkaz sp_set_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|Vytvoří nebo aktualizuje nastavení brány firewall na úrovni serveru pro váš server SQL Database. Tuto uloženou proceduru lze pouze v hlavní databázi, a hlavní přihlášení na úrovni serveru. Pravidlo brány firewall na úrovni serveru můžete vytvořit pouze pomocí příkazů jazyka Transact-SQL po vytvoření prvního pravidla brány firewall na úrovni serveru uživatelem s oprávněními úrovně Azure|
|[Sys.firewall_rules (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Vrátí informace o nastavení brány firewall na úrovni serveru přidružené k Microsoft Azure SQL Database.|
|[sp_delete_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|Nastavení brány firewall na úrovni serveru odstraní z databáze SQL serveru. Tuto uloženou proceduru lze pouze v hlavní databázi, a hlavní přihlášení na úrovni serveru.|
|[sp_set_database_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Vytvoří nebo aktualizuje pravidla brány firewall na úrovni databáze pro Azure SQL Database nebo SQL Data Warehouse. Pravidla brány firewall databáze je možné nakonfigurovat pro hlavní databázi a uživatelských databází v SQL Database. Pravidla brány firewall databáze jsou užitečné při použití uživatelé databáze s omezením. |
|[sys.database_firewall_rules (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Vrátí informace o nastavení brány firewall na úrovni databáze, které jsou přidružené k Microsoft Azure SQL Database. |
|[sp_delete_database_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Nastavení brány firewall na úrovni databáze odebere z Azure SQL Database nebo SQL Data Warehouse. |


> [!TIP]
> Rychlý start pomocí SQL Server Management Studio na Microsoft Windows, naleznete v tématu [Azure SQL Database: použití SQL Server Management Studio k připojení a dotazování dat](sql-database-connect-query-ssms.md). Rychlý start v systému macOS, Linux nebo Windows pomocí Visual Studio Code, naleznete v tématu [Azure SQL Database: použití Visual Studio Code k připojení a dotazování dat](sql-database-connect-query-vscode.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-rest-api"></a>Spravovat servery Azure SQL, databáze a brány firewall pomocí rozhraní REST API

K vytváření a správě serveru Azure SQL, databází a bran firewall, použijte tyto požadavky rozhraní REST API.

| Příkaz | Popis |
| --- | --- |
|[Servery – vytvořit nebo aktualizovat](/rest/api/sql/servers/createorupdate)|Vytvoří nebo aktualizuje nový server.|
|[Servery – odstranit](/rest/api/sql/servers/delete)|Odstraní systému SQL server.|
|[Servery – Get](/rest/api/sql/servers/get)|Získá serveru.|
|[Servery – seznam](/rest/api/sql/servers/list)|Vrátí seznam serverů.|
|[Servery – seznam podle skupin prostředků](/rest/api/sql/servers/listbyresourcegroup)|Vrátí seznam serverů ve skupině prostředků.|
|[Servery – aktualizace](/rest/api/sql/servers/update)|Aktualizuje existující server.|
|[Databáze – vytvořit nebo aktualizovat](/rest/api/sql/databases/createorupdate)|Vytvoří novou databázi nebo aktualizuje existující databázi.|
|[Databáze - Get](/rest/api/sql/databases/get)|Získá databázi.|
|[Databáze – seznam podle elastického fondu](/rest/api/sql/databases/listbyelasticpool)|Vrátí seznam databází v elastickém fondu.|
|[Databáze – seznam serverem](/rest/api/sql/databases/listbyserver)|Vrátí seznam databází na serveru.|
|[Databáze – aktualizace](/rest/api/sql/databases/update)|Aktualizuje existující databázi.|
|[Pravidla – brány firewall vytvořit nebo aktualizovat](/rest/api/sql/firewallrules/createorupdate)|Vytvoří nebo aktualizuje pravidla brány firewall.|
|[Pravidla brány firewall – odstranit](/rest/api/sql/firewallrules/delete)|Odstraní pravidlo brány firewall.|
|[Pravidla brány firewall – Get](/rest/api/sql/firewallrules/get)|Získá pravidla brány firewall.|
|[Pravidla brány firewall – seznam serverem](/rest/api/sql/firewallrules/listbyserver)|Vrátí seznam pravidel brány firewall.|

## <a name="next-steps"></a>Další postup

- Další informace o migraci databáze SQL serveru do Azure najdete v tématu [migrace do služby Azure SQL Database](sql-database-cloud-migrate.md).
- Informace o podporovaných funkcích najdete v tématu [Funkce](sql-database-features.md).
