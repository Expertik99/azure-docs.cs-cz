---
title: Azure SQL Database Managed Instance T-SQL ve službě | Dokumentace Microsoftu
description: Tento článek popisuje rozdíly T-SQL Azure SQL Database Managed Instance a SQL Server.
services: sql-database
author: jovanpop-msft
ms.reviewer: carlrab, bonova
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 0813/2018
ms.author: jovanpop
manager: craigg
ms.openlocfilehash: 57c6b52df3e8f6c47eb794cda4b47bfa2d7de374
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44051234"
---
# <a name="azure-sql-database-managed-instance-t-sql-differences-from-sql-server"></a>Rozdíly ve službě Azure SQL Database Managed Instance T-SQL z SQL serveru 

Azure SQL Database Managed Instance (preview) poskytuje vysokou kompatibilitu díky místní SQL Server Database Engine. Většina funkcí databázový stroj SQL Server jsou podporovány ve spravované instanci. Protože stále existují určité rozdíly v syntaxi a chování, tento článek shrnuje a popisuje tyto rozdíly.
 - [Rozdíly v jazyce T-SQL a nepodporované funkce](#Differences)
 - [Funkce, které mají různé chování v Managed Instance](#Changes)
 - [Dočasná omezení a známé problémy](#Issues)

## <a name="Differences"></a> Rozdíly T-SQL z SQL serveru 

Tento oddíl shrnuje hlavní rozdíly v syntaxi T-SQL a chování mezi Managed Instance a místní SQL Server Database Engine, jakož i nepodporované funkce.

### <a name="always-on-availability"></a>Dostupnosti Always On

[Vysoká dostupnost](sql-database-high-availability.md) je integrovaná do Managed Instance a nemůže se dá nastavit podle uživatelů. Nejsou podporovány následující příkazy:
 - [VYTVOŘENÍ KONCOVÉHO BODU... PRO DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql)
 - [VYTVOŘENÍ SKUPINY DOSTUPNOSTI](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql)
 - [PŘÍKAZ ALTER AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql)
 - [PŘETAŽENÍ SKUPINY DOSTUPNOSTI](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql)
 - [SET HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) klauzuli [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql) – příkaz

### <a name="auditing"></a>Auditování 
 
Hlavní rozdíly mezi auditování SQL v Managed Instance, Azure SQL Database a SQL Server v místním jsou:
- Ve spravované instanci SQL Audit funguje na úrovni serveru nebo úložišti `.xel` účtu úložiště objektů blob soubory v Azure.  
- Ve službě Azure SQL Database SQL Audit funguje na úrovni databáze.
- V místním SQL serverem / virtuální počítač SQL Audit funguje na úrovni serveru, ale ukládá události do protokolů událostí systému a soubory.  
  
Relace XEvent auditování ve spravované instanci podporuje cíle úložiště objektů blob v Azure. Protokolování souborů a systému windows nejsou podporované.    
 
Klíč rozdíly v `CREATE AUDIT` syntaxe pro auditování do Azure blob storage jsou:
- Novou syntaxi `TO URL` je k dispozici a umožní vám zadat adresu URL kontejneru objektů blob v Azure Storage, ve kterém `.xel` budou umístěné soubory 
- Syntaxe `TO FILE` se nepodporuje, protože Managed Instance nemá přístup ke sdílené složky Windows. 
 
Další informace naleznete v tématu:  
- [VYTVOŘIT SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)  
- [PŘÍKAZ ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql) 
- [Auditování](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)     

### <a name="backup"></a>Backup 

Managed Instance má automatické zálohování a umožňuje uživatelům vytvářet kompletní databáze `COPY_ONLY` zálohy. Rozdílové zálohování, protokol a zálohy snímků souborů nejsou podporovány.  
- Managed Instance můžete zálohovat databáze jenom pro účet služby Azure Blob Storage: 
 - Pouze `BACKUP TO URL` je podporováno 
 - `FILE`, `TAPE`, a nejsou podporovány zálohovací zařízení  
- Většina Obecné `WITH` možnosti jsou podporovány. 
 - `COPY_ONLY` je povinné.
 - `FILE_SNAPSHOT` Nepodporuje se  
 - Možnosti pásky: `REWIND`, `NOREWIND`, `UNLOAD`, a `NOUNLOAD` nejsou podporovány 
 - Možnosti specifické pro protokol: `NORECOVERY`, `STANDBY`, a `NO_TRUNCATE` nejsou podporovány 
 
Omezení:  
- Managed Instance můžete zálohovat databáze do zálohy s až 32 pruhy, což je dostatečná pro databáze až do 4 TB, pokud se používá komprese záloh.
- Maximální velikost záložního stripe je 195 GB (objekt blob maximální velikost). Zvýšíte počet pruhy v příkazu backup můžete snižovat velikost jednotlivých stripe a zůstat v rámci tohoto limitu. 

> [!TIP]
> Obejít tato omezení on-premises zálohování `DISK` místo zálohování `URL`, nahrát záložní soubor do objektu blob a potom obnovit. Podporuje větší soubory obnovte, protože se používá typ jiný objektu blob.  

Informace o zálohách pomocí jazyka T-SQL najdete v tématu [zálohování](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

### <a name="buffer-pool-extension"></a>Rozšíření fondu vyrovnávací paměti 
 
- [Rozšíření fondu vyrovnávací paměti](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) se nepodporuje.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` není podporováno. Zobrazit [ALTER SERVER CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql). 
 
### <a name="bulk-insert--openrowset"></a>Příkaz Bulk insert / openrowset

Spravovaná Instance nedaří sdílených složek a složek Windows, tak soubory musí být importovány z úložiště objektů blob v Azure.
- `DATASOURCE` je nutné v `BULK INSERT` příkaz při importu souborů z Azure blob storage. Zobrazit [HROMADNÉ vložení](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE` je nutné v `OPENROWSET` fungovat-li si přečíst obsah souboru z úložiště objektů blob v Azure. Zobrazit [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
 
### <a name="certificates"></a>Certifikáty 

Spravovaná Instance nelze získat přístup k sdílených složek a složek Windows, platí následující omezení: 
- `CREATE FROM`/`BACKUP TO` soubor není podporovaný pro certifikáty
- `CREATE`/`BACKUP` certifikát z `FILE` / `ASSEMBLY` se nepodporuje. Soubory soukromých klíčů nelze použít.  
 
Zobrazit [vytvořit certifikát](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) a [ZÁLOŽNÍ certifikát](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql).  
  
> [!TIP]
> Alternativní řešení: skript certifikátu a privátního klíče, uložit jako soubor .sql a vytvořit z binárního souboru: 
> 
> ``` 
CREATE CERTIFICATE  
 FROM BINARY = asn_encoded_certificate    
WITH PRIVATE KEY ( <private_key_options> ) 
>```   
 
### <a name="clr"></a>CLR 

Spravovaná Instance nelze získat přístup k sdílených složek a složek Windows, platí následující omezení: 
- Pouze `CREATE ASSEMBLY FROM BINARY` je podporována. Zobrazit [vytvořit sestavení z binární](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).  
- `CREATE ASSEMBLY FROM FILE` není podporováno. Zobrazit [vytvořit sestavení ze souboru](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` nemůže odkazovat na soubory. Zobrazit [změna sestavení](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).
 
### <a name="compatibility-levels"></a>Úrovně kompatibility 
 
- Úrovně kompatibility podporované jsou: 100, 110, 120, 130, 140  
- Nejsou podporované úrovně kompatibility nižší než 100. 
- Výchozí úroveň kompatibility pro nové databáze je 140. Pro obnovenou databází zůstane beze změny úrovní kompatibility i pokud se jednalo o 100 a vyšší.

Zobrazit [úroveň kompatibility ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).
 
### <a name="credential"></a>Přihlašovací údaj 
 
Pouze Azure Key Vault a `SHARED ACCESS SIGNATURE` identity podporují. Uživatelé Windows se nepodporují.
 
V tématu [vytvořit pověření](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) a [ALTER pověření](https://docs.microsoft.com/sql/t-sql/statements/alter-credential-transact-sql). 
 
### <a name="cryptographic-providers"></a>Zprostředkovatelé kryptografických služeb

Spravovaná Instance nemají přístup k souborům, proto nelze vytvořit zprostředkovatelé kryptografických služeb:
- `CREATE CRYPTOGRAPHIC PROVIDER` není podporováno. Zobrazit [vytvořit zprostředkovatele KRYPTOGRAFICKÝCH služeb](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` není podporováno. Zobrazit [Změna zprostředkovatele KRYPTOGRAFICKÝCH služeb](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql). 

### <a name="collation"></a>Kolace 
 
Kolace serveru je `SQL_Latin1_General_CP1_CI_AS` a nedá se změnit. Zobrazit [kolace](https://docs.microsoft.com/sql/t-sql/statements/collations).
 
### <a name="database-options"></a>Možnosti databáze 
 
- Víc souborů protokolů nejsou podporovány. 
- Objekty v paměti nejsou podporovány v rámci úrovně služeb pro obecné účely.  
- Platí limit 280 souborů na jednu instanci zdání maximální 280 počet souborů v databázi. Soubory protokolu a data se počítají do tohoto limitu.  
- Databáze nemůže obsahovat příkaz, který obsahuje filestream data.  Obnovení se nezdaří, pokud obsahuje .bak `FILESTREAM` data.  
- Každý soubor je umístěn ve službě Azure Premium storage. Vstupně-výstupních operací a propustnosti na soubor závisí na velikosti jednotlivých souborů, stejným způsobem jako pro disky Azure Premium Storage. Zobrazit [výkon disku úrovně Premium pro Azure](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes)  
 
#### <a name="create-database-statement"></a>Příkaz CREATE DATABASE

Toto jsou `CREATE DATABASE` omezení: 
- Soubory a skupiny souborů nelze definovat.  
- `CONTAINMENT` možnost není podporována.  
- `WITH`možnosti nejsou podporovány.  
   > [!TIP]
   > Jako alternativní řešení, použijte `ALTER DATABASE` po `CREATE DATABASE` nastavit možnosti databáze k přidání souborů nebo k nastavení členství ve skupině.  

- `FOR ATTACH` možnost není podporována. 
- `AS SNAPSHOT OF` možnost není podporována. 

Další informace najdete v tématu [CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>Příkaz ALTER DATABASE

Nelze nastavit nebo změnit některé vlastnosti souboru:
- Cesta k souboru nemůže být zadán `ALTER DATABASE ADD FILE (FILENAME='path')` příkazu T-SQL. Odebrat `FILENAME` ze skriptu protože spravované Instance automaticky umístí soubory.  
- Název souboru nelze změnit pomocí `ALTER DATABASE` příkazu.

Tyto možnosti jsou ve výchozím nastavení a nedá se změnit: 
- `MULTI_USER` 
- `ENABLE_BROKER ON` 
- `AUTO_CLOSE OFF` 

Nelze změnit následující možnosti: 
- `AUTO_CLOSE` 
- `AUTOMATIC_TUNING(CREATE_INDEX=ON|OFF)` 
- `AUTOMATIC_TUNING(DROP_INDEX=ON|OFF)` 
- `DISABLE_BROKER` 
- `EMERGENCY` 
- `ENABLE_BROKER` 
- `FILESTREAM` 
- `HADR`   
- `NEW_BROKER` 
- `OFFLINE` 
- `PAGE_VERIFY` 
- `PARTNER` 
- `READ_ONLY` 
- `RECOVERY BULK_LOGGED` 
- `RECOVERY_SIMPLE` 
- `REMOTE_DATA_ARCHIVE`  
- `RESTRICTED_USER` 
- `SINGLE_USER` 
- `WITNESS`

Upravit název není podporován.

Další informace najdete v tématu [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options).

### <a name="database-mirroring"></a>Zrcadlení databáze

Zrcadlení databáze se nepodporuje.
 - `ALTER DATABASE SET PARTNER` a `SET WITNESS` možnosti nejsou podporovány.
 - `CREATE ENDPOINT … FOR DATABASE_MIRRORING` není podporováno.

Další informace najdete v tématu [ALTER DATABASE SET PARTNER a SET WITNESS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) a [vytvořit koncový bod... PRO DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="dbcc"></a>DBCC 
 
Managed Instance nepodporuje nedokumentované příkazů DBCC, u kterých jde v systému SQL Server.
- `Trace Flags` nejsou podporovány. Zobrazit [příznaky trasování](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- `DBCC TRACEOFF` není podporováno. Zobrazit [příkaz DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql).
- `DBCC TRACEON` není podporováno. Zobrazit [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql).

### <a name="distributed-transactions"></a>Distribuované transakce

Ani jedna služba MSDTC ani [elastické transakce](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-transactions-overview) jsou aktuálně podporované ve spravované instanci.

### <a name="extended-events"></a>Rozšířené události 

Některé cíle Windows specifické pro události Xevent nepodporuje:
- `etw_classic_sync target` není podporováno. Store `.xel` souborů v Azure blob storage. Zobrazit [etw_classic_sync cílové](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etwclassicsynctarget-target). 
- `event_file target`není podporováno. Store `.xel` souborů v Azure blob storage. Zobrazit [event_file cílové](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#eventfile-target).

### <a name="external-libraries"></a>Externí knihovny

V databázi R a Python externí knihovny se zatím nepodporují. Zobrazit [SQL Server Machine Learning Services](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>FileStream a Filetable

- FileStream data není podporován. 
- Databáze nesmí obsahovat skupiny souborů s `FILESTREAM` dat
- `FILETABLE` není podporováno
- Tabulky nemůže mít `FILESTREAM` typy
- Nejsou podporovány následující funkce:
 - `GetPathLocator()` 
 - `GET_FILESTREAM_TRANSACTION_CONTEXT()` 
 - `PathName()` 
 - `GetFileNamespacePath()` 
 - `FileTableRootPath()` 

Další informace najdete v tématu [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) a [Filetable](https://docs.microsoft.com/sql/relational-databases/blob/filetables-sql-server).

### <a name="full-text-semantic-search"></a>Sémantické vyhledávání

[Sémantické vyhledávání](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) se nepodporuje.

### <a name="linked-servers"></a>Propojené servery
 
Propojené servery ve spravované instanci podporuje omezený počet cílů: 
- Podporované cíle: SQL Server a SQL Database
- Nepodporuje cíle: soubory služby Analysis Services a další relační databázový systém.

Operace

- Různé instance zápisu transakce nejsou podporovány.
- `sp_dropserver` platí pro odkazovaný server vyřadit. Zobrazit [sp_dropserver](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- `OPENROWSET` funkci můžete použít k provádění dotazů pouze na instancích systému SQL Server (buď spravovaných, místně nebo ve virtuálních počítačích). Zobrazit [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- `OPENDATASOURCE` funkci můžete použít k provádění dotazů pouze na instancích systému SQL Server (buď spravovaných, místně nebo ve virtuálních počítačích). Pouze `SQLNCLI`, `SQLNCLI11`, a `SQLOLEDB` hodnoty jsou podporovány jako zprostředkovatel. Například: `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee`. Zobrazit [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql).
 
### <a name="logins--users"></a>Přihlášení / uživatelé 

- Vytvoření přihlášení SQL `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY`, a `FROM SID` jsou podporovány. Zobrazit [vytvořit přihlášení](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- Přihlašovací údaje Windows vytvořené pomocí `CREATE LOGIN ... FROM WINDOWS` syntaxe nejsou podporovány.
- Azure Active Directory (Azure AD) uživatele, který vytvořil instanci má [neomezená oprávnění správce](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins#unrestricted-administrative-accounts).
- Je možné vytvářet uživatelé bez oprávnění správce služby Azure Active Directory (Azure AD) úrovni databáze pomocí `CREATE USER ... FROM EXTERNAL PROVIDER` syntaxe. Zobrazit [vytvořit uživatele... EXTERNÍ ZPROSTŘEDKOVATELE](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins#non-administrator-users)
 
### <a name="polybase"></a>Polybase

Odkazování na soubory v HDFS nebo Azure blob storage externí tabulky nejsou podporovány. Informace o Polybase najdete v tématu [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Replikace 
 
Replikace se nepodporuje na Managed Instance. Informace o replikaci najdete v tématu [replikace systému SQL Server](http://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance).
 
### <a name="restore-statement"></a>OBNOVENÍ – příkaz 
 
- Podporované syntaxe  
   - `RESTORE DATABASE` 
   - `RESTORE FILELISTONLY ONLY` 
   - `RESTORE HEADER ONLY` 
   - `RESTORE LABELONLY ONLY` 
   - `RESTORE VERIFYONLY ONLY` 
- Nepodporovaná syntaxe 
   - `RESTORE LOG ONLY` 
   - `RESTORE REWINDONLY ONLY`
- Zdroj  
 - `FROM URL` (Úložiště objektů blob v azure) je pouze podporované možnosti.
 - `FROM DISK`/`TAPE`/ zálohovací zařízení se nepodporuje.
 - Zálohovacích skladů nejsou podporovány. 
- `WITH` možnosti nejsou podporovány (ne `DIFFERENTIAL`, `STATS`atd.)     
- `ASYNC RESTORE` – Obnovení pokračovat i v případě, že přestane fungovat připojení klienta. Pokud vaše připojení se ukončí, můžete zkontrolovat `sys.dm_operation_status` zobrazení stavu operace obnovení (stejně jako pro vytvoření a DROP database). Zobrazit [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database).  
 
Následující možnosti databáze jsou set/přepsat a není možné později změnit:  
- `NEW_BROKER` (Pokud je zprostředkovatel není povolen v souboru .bak)  
- `ENABLE_BROKER` (Pokud je zprostředkovatel není povolen v souboru .bak)  
- `AUTO_CLOSE=OFF` (Pokud je databáze v souboru .bak `AUTO_CLOSE=ON`)  
- `RECOVERY FULL` (Pokud je databáze v souboru .bak `SIMPLE` nebo `BULK_LOGGED` režimu obnovení)
- Přidán a volají XTP, pokud nebylo ve zdrojovém souboru .bak paměťově optimalizované skupiny souborů  
- Žádné existující skupiny paměťově optimalizovaných souborů bylo přejmenováno na XTP  
- `SINGLE_USER` a `RESTRICTED_USER` možnosti jsou převedeny na `MULTI_USER`   
Omezení:  
- `.BAK` nelze obnovit, soubory, které obsahují více sad záloh. 
- `.BAK` nelze obnovit, soubory, které obsahují víc souborů protokolů. 
- Obnovení se nezdaří, pokud obsahuje .bak `FILESTREAM` data.
- Zálohy obsahující databáze, které mají aktivní v paměti objektů nelze nyní obnovit.  
- Zálohování obsahující databáze, kde v určitém okamžiku existoval objektů v paměti nelze nyní obnovit.   
- Zálohování, který obsahuje databáze v režimu jen pro čtení nelze nyní obnovit. Toto omezení bude brzy odebráno.   
 
Informace o příkazech obnovení najdete v tématu [obnovit příkazy](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

### <a name="service-broker"></a>Služba Service broker 
 
- Různé instance služby Service broker není podporován. 
 - `sys.routes` -Předpoklad: Vyberte adresu sys.routes. Adresa musí být místní na každý trasy. Zobrazit [sys.routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
 - `CREATE ROUTE` – Nemůžete `CREATE ROUTE` s `ADDRESS` jiné než `LOCAL`. Zobrazit [TRASY vytvořit](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
 - `ALTER ROUTE` nelze `ALTER ROUTE` s `ADDRESS` jiné než `LOCAL`. Zobrazit [TRASY ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql).  
 
### <a name="service-key-and-service-master-key"></a>Služby a klíče hlavní klíč služby 
 
- [Záloha hlavního klíče](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) se nepodporuje (spravované službou SQL Database) 
- [Obnovení hlavního klíče](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) se nepodporuje (spravované službou SQL Database) 
- [Záloha hlavního klíče služby](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) se nepodporuje (spravované službou SQL Database) 
- [Obnovení hlavního klíče služby](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) se nepodporuje (spravované službou SQL Database) 
 
### <a name="stored-procedures-functions-triggers"></a>Uložené procedury, funkce, aktivační události 
 
- `NATIVE_COMPILATION` není aktuálně podporováno. 
- Následující [uložené procedury sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql) možnosti nejsou podporovány: 
 - `allow polybase export` 
 - `allow updates` 
 - `filestream_access_level` 
 - `max text repl size` 
 - `remote data archive` 
 - `remote proc trans` 
- `sp_execute_external_scripts` není podporováno. Zobrazit [sp_execute_external_scripts](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` není podporováno. Zobrazit [xp_cmdshell](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `Extended stored procedures` nejsou podporované. zahrnuje `sp_addextendedproc` a `sp_dropextendedproc`. Zobrazit [rozšířené uložené procedury](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql)
- `sp_attach_db`, `sp_attach_single_file_db`, a `sp_detach_db` nejsou podporovány. Zobrazit [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql), a [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).
- `sp_renamedb` není podporováno. Zobrazit [sp_renamedb](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-renamedb-transact-sql).

### <a name="sql-server-agent"></a>Agent SQL Server 
 
- Nastavení agenta SQL jsou jen pro čtení. Postup `sp_set_agent_properties` není ve spravované instanci podporováno.  
- Úlohy – pouze kroky úlohy T-SQL jsou aktuálně podporované (Další kroky budou přidány ve verzi public preview).
 - Služby SSIS se ještě nepodporuje. 
 - Replikace se ještě nepodporuje.  
  - Čtečky protokolů transakcí se ještě nepodporuje.  
  - Snímek se ještě nepodporuje.  
  - Distributor není dosud podporován.  
  - Sloučení se nepodporuje.  
  - Čtečky fronty se nepodporuje.  
 - Příkazové okno se ještě nepodporuje. 
  - Spravovaná Instance nelze přístup k externím prostředkům (například sdílené síťové složky prostřednictvím nástroje robocopy).  
 - Prostředí PowerShell se ještě nepodporuje.
 - Analysis Services nejsou podporovány.  
- Oznámení jsou podporovány jen částečně.
 - E-mailových oznámení je podporováno, vyžaduje konfiguraci profil databázového e-mailu. Může existovat pouze jedna databáze profil e-mailu a musí být volána `AzureManagedInstance_dbmail_profile` ve verzi public preview (dočasné omezení).  
 - Operátor není podporován.  
 - Příkazu není podporován. 
 - Výstrahy se ještě nepodporuje.
 - Nepodporuje proxy servery.  
- Protokol událostí se nepodporuje. 
 
Následující funkce nejsou aktuálně podporovány, ale bude v budoucnu povoleno:  
- Proxy
- Plánování úloh na nečinnosti procesoru 
- Povolení nebo zakázání agenta
- Výstrahy

Informace o agenta systému SQL Server najdete v tématu [agenta systému SQL Server](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent).
 
### <a name="tables"></a>Tabulky 

Není podporované toto: 
- `FILESTREAM` 
- `FILETABLE` 
- `EXTERNAL TABLE`
- `MEMORY_OPTIMIZED`  

Informace o vytváření a změny tabulek naleznete v tématu [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) a [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).

## <a name="Changes"></a> Změny chování 
 
Následující proměnné, funkce a zobrazení vrátí odlišné výsledky:  
- `SERVERPROPERTY('EngineEdition')` Vrátí hodnotu 8. Tato vlastnost jednoznačně identifikuje Managed Instance. Zobrazit [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` Vrátí hodnotu NULL, protože pro SQL Server se nevztahuje na Managed Instance existuje koncept instance, jak ho. Zobrazit [SERVERPROPERTY('InstanceName')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` Vrátí úplný "umožňující připojení k" název DNS, třeba Moje instance.wcus17662feb9ce98.database.windows.net spravované. Zobrazit [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql).  
- `SYS.SERVERS` -vrátí úplnou "umožňující připojení k" název DNS, jako například `myinstance.domain.database.windows.net` pro vlastnosti "name" a "data_source". Zobrazit [SYS. SERVERY](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql). 
- `@@SERVICENAME` Vrátí hodnotu NULL, protože koncept služby, protože existuje pro SQL Server se nevztahuje na Managed Instance. Zobrazit [@@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).   
- `SUSER_ID` je podporován. Vrátí hodnotu NULL, pokud není v sys.syslogins přihlášení AAD. Zobrazit [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql).  
- `SUSER_SID` není podporováno. Vrátí nesprávná data (dočasný známý problém). Zobrazit [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql). 
- `GETDATE()` a další předdefinované datum a čas funkce vždy vrátí čas v časovém pásmu UTC. Zobrazit [GETDATE](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql).

## <a name="Issues"></a> Známé problémy a omezení

### <a name="tempdb-size"></a>Velikost databáze TEMPDB

`tempdb` je rozdělený do 12 soubory s maximální velikost 14 GB na soubor. Tento maximální velikost jednoho souboru nelze změnit, a nové soubory nelze přidat do `tempdb`. Toto omezení bude brzy odebráno. Některé dotazy může vrátit chybu, pokud je nutné do více než 168 GB `tempdb`.

### <a name="exceeding-storage-space-with-small-database-files"></a>Překročení prostoru úložiště se soubory malé databáze

Každý Managed Instance má vyhrazené pro místa na disku Azure Premium storage 35 TB a každý databázový soubor je umístěn na jiném fyzickém disku. Disky o velikosti může být 128 GB, 256 GB, 512 GB, 1 TB nebo 4 TB. Nevyužité místo na disku se neúčtuje, ale celkový součet velikosti disků typu Premium Azure nemůže být delší než 35 TB. V některých případech může překročit Managed Instance, která nepotřebuje 8 TB celkem 35 TB Azure omezí velikost úložiště, z důvodu interní fragmentace. 

Managed Instance může mít například jeden soubor 1,2 TB velikosti, který je umístěn na disku 4 TB a 248 soubory 1 GB velikost in jsou umístěné na různých discích 128 GB. V tomto příkladu 
* Celková velikost úložiště je 1 × 4 TB + 248 x 128 GB = 35 TB. 
* Celkový počet vyhrazené místo pro databáze na instanci je 1 x 1.2 TB + 248 x 1 GB = 1,4 TB.
To ukazuje, že za určitých okolností, z důvodu velmi specifický distribuce souborů, může kontaktovat Managed Instance 35 TB vyhrazené pro připojený Disk Premium Azure při nemusí očekávání. 

V tomto příkladu budou nadále fungovat stávající databáze a můžou růst bez problémů, tak dlouho, dokud nejsou přidány nové soubory. Nové databáze však nelze vytvořit ani obnovit, protože není dostatek místa pro nových diskových jednotek, i když celková velikost všech databází: nebylo dosaženo omezení velikosti instance. Chyba, která je vrácena v takovém případě není jasný.

### <a name="incorrect-configuration-of-sas-key-during-database-restore"></a>Nesprávná konfigurace SAS klíč při databáze obnovení

`RESTORE DATABASE` který čte soubor .bak může být neustále opakování pokusu o číst soubor .bak a vrátí chybové po dlouhou dobu, pokud sdíleného přístupového podpisu v `CREDENTIAL` je nesprávný. Před obnovením databáze, ujistěte se, že se klíč SAS pro správné spuštění RESTORE HEADERONLY.
Ujistěte se, že odeberete úvodní `?` z klíč SAS vygenerovaný pomocí webu Azure portal.

### <a name="tooling"></a>Nástroje

SQL Server Management Studio a SQL Server Data Tools může mít některé problémy při přístupu k Managed Instance. Budou všechny nástroje problémy řešeny před obecnou dostupností.

### <a name="incorrect-database-names"></a>Názvy nesprávné databází

Managed Instance se můžou zobrazovat hodnota identifikátoru guid místo názvu databáze během obnovení nebo v některé chybové zprávy. Tyto problémy se opraví, před obecnou dostupností.

### <a name="database-mail-profile"></a>Profil databázového e-mailu
Může existovat pouze jedna databáze profil e-mailu a musí být volána `AzureManagedInstance_dbmail_profile`. Jedná se o dočasné omezení, která bude brzy odebráno.

## <a name="next-steps"></a>Další postup

- Podrobnosti o Managed Instance najdete v tématu [co je Managed Instance?](sql-database-managed-instance.md)
- Pro funkce a seznam porovnání, naleznete v tématu [běžné funkce SQL](sql-database-features.md).
- Kurz ukazuje, jak vytvořit nový Managed Instance, najdete v tématu [vytvoření Managed Instance](sql-database-managed-instance-get-started.md).
