---
title: Pomocí smyček T-SQL ve službě Azure SQL Data Warehouse | Dokumentace Microsoftu
description: Tipy pro pomocí smyček T-SQL a pro vývoj řešení pro nahrazení kurzory ve službě Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: b7c21566916c9728900e69dc6480098fadae7622
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/30/2018
ms.locfileid: "43301204"
---
# <a name="using-t-sql-loops-in-sql-data-warehouse"></a>Pomocí smyček T-SQL ve službě SQL Data Warehouse
Tipy pro pomocí smyček T-SQL a pro vývoj řešení pro nahrazení kurzory ve službě Azure SQL Data Warehouse.

## <a name="purpose-of-while-loops"></a>Účelem smyček WHILE

SQL Data Warehouse podporuje [při](/sql/t-sql/language-elements/while-transact-sql) smyčky pro opakované spuštění výkazu bloků. Tuto smyčku WHILE pokračuje, pokud jsou zadané podmínky nastavena hodnota true, nebo dokud kód konkrétně ukončí smyčku pomocí klíčového slova přerušení. Smyčky jsou užitečné pro nahrazení kurzory definované v kódu SQL. Naštěstí téměř všechny ukazatele, které jsou napsány v SQL kódu mají různé rychloposuv vpřed, jen pro čtení. Proto [a] jsou skvělou alternativou k nahrazení kurzory smyčky.

## <a name="replacing-cursors-in-sql-data-warehouse"></a>Nahrazení kurzory ve službě SQL Data Warehouse
Ale než se nejprve podíváme v hlavní zeptejte se sami na následující otázku: "měl by tento kurzor být přepsán používání založeným na set operace?." V mnoha případech odpověď je Ano a je často nejlepším řešením. Operace založeným na set často provádí rychleji než metodiky iterativní, řádek po řádku.

Rychloposuv vpřed jen pro čtení ukazatele lze snadno nahradit uvozuje konstruktor cyklu. Níže je jednoduchý příklad. Tento příklad kódu aktualizuje statistiku pro všechny tabulky v databázi. Pomocí provádí iterace tabulek ve smyčce, každý příkaz spouští v sekvenci.

Nejprve vytvořte dočasné tabulky obsahující číslo jedinečném řádku použít k identifikaci jednotlivých příkazů:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Za druhé inicializujte proměnné potřebná k provedení smyčky:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Nyní ve smyčce příkazy, které spouští jeden po druhém:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Nakonec vyřaďte dočasné tabulky vytvořené v prvním kroku

```
DROP TABLE #tbl;
```

## <a name="next-steps"></a>Další postup
Další tipy pro vývoj najdete v části [přehled vývoje](sql-data-warehouse-overview-develop.md).

