---
title: Článek o omezeních známé problémy a migrace s online migraci do Azure SQL Database | Dokumentace Microsoftu
description: Přečtěte si o známých problémech a migrace omezení online migraci do Azure SQL Database.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 08/24/2018
ms.openlocfilehash: 1f8e3ede4140ab5346285f7c247864f8ef8e2d48
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/24/2018
ms.locfileid: "42889604"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-sql-db"></a>Známé problémy a migrace omezení online migrace do Azure SQL DB

Známé problémy a omezení související s online migraci z SQL serveru do služby Azure SQL Database jsou popsané níže.

- Migrace z dočasné tabulky nejsou podporovány

    **Příznak**

    Pokud zdrojová databáze obsahuje jeden nebo více dočasných tabulek, migrace databází selže během operace "úplné načtení dat" a může se zobrazit následující zpráva:

    {"ID prostředku": "/subscriptions/<subscription id>/resourceGroups/migrateready/providers/Microsoft.DataMigration/services/<DMS Service name>", "errorType": "Chyba migrace databáze", "errorEvents": "[" funkce zachytávání nelze nastavit. RetCode: SqlState SQL_ERROR: 42000 NativeError: 13570 zprávy: [Microsoft] [SQL Server Native Client 11.0] [SQL Server] použití replikace není podporována se systémovou správou dočasná tabulka "[aplikace. Měst]' řádek: 1 sloupec: hodnota -1 "]"}
 
   ![Příklad chyby dočasnou tabulku](media\known-issues-azure-sql-online\dms-temporal-tables-errors.png)

   **Alternativní řešení**

   1. Najdete dočasných tabulek ve schématu zdroje pomocí dotazu níže.
        ``` 
       select name,temporal_type,temporal_type_desc,* from sys.tables where temporal_type <>0
        ```
   2. Vyloučit z těchto tabulek **nakonfigurovat nastavení migrace** okno, ve kterém zadáte tabulky k migraci.
   3. Znovu spusťte aktivitu migrace.

    **Prostředky**

    Další informace najdete v článku [dočasných tabulek se](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables?view=sql-server-2017).
 
- Migrace tabulky obsahuje jeden nebo více sloupců datového typu hierarchyid

    **Příznak**

    Může zobrazit výjimka SQL navrhuje "ntext není kompatibilní s hierarchyid" během operace "úplné načtení dat":
     
    ![Příklad hierarchyid chyby](media\known-issues-azure-sql-online\dms-hierarchyid-errors.png)

    **Alternativní řešení**

    1. Najdete uživatele tabulky, které obsahují sloupce datového typu hierarchyid pomocí dotazu níže.

        ``` 
        select object_name(object_id) 'Table name' from sys.columns where system_type_id =240 and object_id in (select object_id from sys.objects where type='U')
        ``` 

    2.  Vyloučit z těchto tabulek **nakonfigurovat nastavení migrace** okno, ve kterém zadáte tabulky k migraci.
    3.  Znovu spusťte aktivitu migrace.

- Selhání migrace s různými porušení integrity s aktivní aktivačními událostmi ve schématu během "úplné načtení dat" nebo "Přírůstková synchronizace dat"

    **Alternativní řešení**
    1. Najdete aktivační události, které jsou aktuálně aktivní zdrojové databáze pomocí dotazu níže:
        ```
        select * from sys.triggers where is_disabled =0
        ```
    2.  Zakázat aktivační události na zdrojové databázi pomocí kroků uvedených v následujícím článku [DISABLE TRIGGER (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/disable-trigger-transact-sql?view=sql-server-2017).
    3.  Opětovné spuštění aktivity migrace.

- Podpora pro typy dat LOB

    **Příznak**

    Pokud délka sloupec velkého objektu (LOB) je větší než 32 KB, může získat data zkrácena v cílovém. Délka sloupce LOB pomocí dotazu níže, můžete zkontrolovat: 

    ``` 
    SELECT max(len(ColumnName)) as LEN from TableName
    ```

    **Alternativní řešení**

    Pokud máte sloupec LOB, který je větší než 32 KB, obraťte se na technický tým na [ dmsfeedback@microsoft.com ](mailto:dmsfeedback@microsoft.com).

- Problémy s sloupce časového razítka

    **Příznak**

    DMS nemigruje hodnotu časové razítko zdroje Místo toho DMS generuje novou hodnotu časového razítka v cílové tabulce.

    **Alternativní řešení**

    Pokud potřebujete DMS k migraci přesné časové razítko hodnoty uložené ve zdrojové tabulce, obraťte se na technický tým na [ dmsfeedback@microsoft.com ](mailto:dmsfeedback@microsoft.com).

- Chyby při migraci dat se neposkytuje další podrobnosti v okně podrobný stav databáze.

    **Příznak**

    Když dojde k selhání migrace v zobrazení stavu podrobnosti databáze, vyberete **chyby při migraci dat** odkaz na horním pásu karet nemusí poskytnout další podrobnosti, které jsou specifické pro selhání migrace.

     ![chyby při migraci dat žádné podrobnosti o příklad](media\known-issues-azure-sql-online\dms-data-migration-errors-no-details.png)

    **Alternativní řešení**

    Získat podrobnosti o konkrétní chybě, postupujte podle následujících kroků.

    1.  Zavřete okno podrobný stav databáze zobrazte obrazovku aktivitu migrace.

     ![obrazovka aktivita migrace](media\known-issues-azure-sql-online\dms-migration-activity-screen.png)

    2. Vyberte **viz podrobnosti o chybě** zobrazíte specifické chybové zprávy, které vám umožní řešit chyby při migraci.
