---
title: Limity založený na virtuálních jádrech prostředků Azure SQL Database – elastických fondů | Dokumentace Microsoftu
description: Tato stránka popisuje některé běžné prostředek založený na virtuálních jádrech limity pro elastické fondy Azure SQL Database.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: carlrab
ms.openlocfilehash: 068ecf8283b92873542a7cb9ab2202212fd2ad2c
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2018
ms.locfileid: "39495505"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-elastic-pools"></a>Založený na virtuálních jádrech zakoupení modelu limity pro elastické fondy Azure SQL Database

Tento článek obsahuje podrobné prostředků limity pro elastické fondy Azure SQL Database a databáze ve fondu pomocí nákupní model založený na virtuálních jádrech.

Založený na DTU nákupní model omezení najdete v tématu [omezení prostředků založený na DTU databáze SQL – elastických fondů](sql-database-dtu-resource-limits-elastic-pools.md).

> [!IMPORTANT]
> Za určitých okolností budete muset zmenšit databázi uvolnění nevyužívaného místa. Další informace najdete v tématu [spravovat místo souborů ve službě Azure SQL Database](sql-database-file-space-management.md).

## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>Elastický fond: velikosti úložiště a úrovně výkonu

Pro elastické fondy SQL Database následující tabulky popisují prostředky dostupné na všech úrovních úrovni a výkonu služby. Můžete nastavit úroveň služby, úroveň výkonu a úložiště pomocí částka [webu Azure portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), [rozhraní příkazového řádku Azure](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases), nebo [rozhraníRESTAPI](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases).

> [!NOTE]
> Omezení prostředků jednotlivých databází v elastických fondech jsou obvykle stejné jako u izolovaných databází mimo fondy, které se stejnou úrovní výkonu. Maximální počet souběžných pracovních procesů pro databázi GP_Gen4_1 je například 200 pracovních procesů. Maximální počet souběžných pracovních procesů pro databáze ve fondu GP_Gen4_1 je tedy také 200 pracovních procesů. Všimněte si, že se celkový počet souběžných pracovních procesů ve fondu GP_Gen4_1 je 210.

### <a name="general-purpose-service-tier"></a>Obecné účely úrovně služeb

#### <a name="generation-4-compute-platform"></a>Výpočetní platforma běžící generace 4
|Úroveň výkonu|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Generování H/W|4|4|4|4|4|4|
|Virtuální jádra|1|2|4|8|16|24|
|Paměť (GB)|7|14|28|56|112|168|
|Podpora Columnstore|Ano|Ano|Ano|Ano|Ano|Ano|
|Úložiště OLTP v paměti (GB)|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|
|Typ úložiště|Storage úrovně Premium (vzdálené)|Storage úrovně Premium (vzdálené)|Storage úrovně Premium (vzdálené)|Storage úrovně Premium (vzdálené)|Storage úrovně Premium (vzdálené)|Storage úrovně Premium (vzdálené)|
|Maximální velikost dat (GB)|512|756|1536|2 048|3584|4 096|
|Maximální velikost protokolu|154|227|461|614|1075|1229|
|Databáze TempDB size(DB)|32|64|128|256|384|384|
|Cíl vstupně-výstupních operací (64 KB)|500|1000|2000|4000|7000|7000|
|Vstupně-výstupní latence (přibližné)|5 – 7 ms (zápis)<br>5 až 10 ms (čtení)|5 – 7 ms (zápis)<br>5 až 10 ms (čtení)|5 – 7 ms (zápis)<br>5 až 10 ms (čtení)|5 – 7 ms (zápis)<br>5 až 10 ms (čtení)|5 – 7 ms (zápis)<br>5 až 10 ms (čtení)|5 – 7 ms (zápis)<br>5 až 10 ms (čtení)|5 – 7 ms (zápis)<br>5 až 10 ms (čtení)|
|Maximální počet souběžných pracovních procesů (požadavků)|210|420|840|1680|3360|5040|
|Maximální povolené relace|30000|30000|30000|30000|30000|30000|
|Maximální počet fondu hustota|100|200|500|500|500|500|
|Minimální/maximální elastického fondu kliknutím – zastavení|0, 0,25, 0,5, 1|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24|
|Počet replik|1|1|1|1|1|1|
|Více AZ|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|
|Přečtěte si horizontální navýšení kapacity|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|
|Zahrnuté úložiště zálohování|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|
|||

#### <a name="generation-5-compute-platform"></a>Výpočetní platforma běžící generace 5
|Úroveň výkonu|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|Generování H/W|5|5|5|5|5|5|5|5|
|Virtuální jádra|2|4|8|16|24|32|40|80|
|Paměť (GB)|11|22|44|88|132|176|220|440|
|Podpora Columnstore|Ano|Ano|Ano|Ano|Ano|Ano|Ano|Ano|
|Úložiště OLTP v paměti (GB)|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|
|Typ úložiště|Storage úrovně Premium (vzdálené)|Storage úrovně Premium (vzdálené)|Storage úrovně Premium (vzdálené)|Storage úrovně Premium (vzdálené)|Storage úrovně Premium (vzdálené)|Storage úrovně Premium (vzdálené)|Storage úrovně Premium (vzdálené)|Storage úrovně Premium (vzdálené)|
|Maximální velikost dat (GB)|512|756|1536|2 048|3072|4 096|4 096|4 096|
|Maximální velikost protokolu|154|227|461|614|922|1229|1229|1229|
|Databáze TempDB size(DB)|64|128|256|384|384|384|384|384|
|Cíl vstupně-výstupních operací (64 KB)|500|1000|2000|4000|6000|7000|7000|7000|
|Vstupně-výstupní latence (přibližné)|5 – 7 ms (zápis)<br>5 až 10 ms (čtení)|5 – 7 ms (zápis)<br>5 až 10 ms (čtení)|5 – 7 ms (zápis)<br>5 až 10 ms (čtení)|5 – 7 ms (zápis)<br>5 až 10 ms (čtení)|5 – 7 ms (zápis)<br>5 až 10 ms (čtení)|5 – 7 ms (zápis)<br>5 až 10 ms (čtení)|5 – 7 ms (zápis)<br>5 až 10 ms (čtení)|5 – 7 ms (zápis)<br>5 až 10 ms (čtení)|5 – 7 ms (zápis)<br>5 až 10 ms (čtení)|
|Maximální počet souběžných pracovních procesů (požadavků)|210|420|840|1680|2520|3360|4200|8400
|Maximální povolené relace|30000|30000|30000|30000|30000|30000|30000|30000|
|Maximální počet fondu hustota|100|200|500|500|500|500|500|500|
|Minimální/maximální elastického fondu kliknutím – zastavení|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24|0, 0,5, 1, 2, 4, 8, 16, 24, 32|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40, 80|
|Počet replik|1|1|1|1|1|1|1|1|
|Více AZ|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|
|Přečtěte si horizontální navýšení kapacity|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|
|Zahrnuté úložiště zálohování|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|
|||

### <a name="business-critical-service-tier"></a>Obchodní vrstvy kritické služby

#### <a name="generation-4-compute-platform"></a>Výpočetní platforma běžící generace 4
|Úroveň výkonu|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Generování H/W|4|4|4|4|4|4|
|Virtuální jádra|1|2|4|8|16|24|
|Paměť (GB)|7|14|28|56|112|168|
|Podpora Columnstore|Ano|Ano|Ano|Ano|Ano|Ano|
|Úložiště OLTP v paměti (GB)|1|2|4|8|20|36|
|Typ úložiště|Místní disk SSD|Místní disk SSD|Místní disk SSD|Místní disk SSD|Místní disk SSD|Místní disk SSD|
|Maximální velikost dat (GB)|1024|1024|1024|1024|1024|1024|
|Maximální velikost protokolu|307|307|307|307|307|307|
|Databáze TempDB size(DB)|32|64|128|256|384|384|
|Cíl vstupně-výstupních operací (64 KB)|5000|10000|20000|40000|80000|120000|
|Vstupně-výstupní latence (přibližné)|1 až 2 ms (zápis)<br>1 až 2 ms (čtení)|1 až 2 ms (zápis)<br>1 až 2 ms (čtení)|1 až 2 ms (zápis)<br>1 až 2 ms (čtení)|1 až 2 ms (zápis)<br>1 až 2 ms (čtení)|1 až 2 ms (zápis)<br>1 až 2 ms (čtení)|1 až 2 ms (zápis)<br>1 až 2 ms (čtení)|
|Maximální počet souběžných pracovních procesů (požadavků)|210|420|840|1680|3360|5040|
|Maximální povolené relace|30000|30000|30000|30000|30000|30000|
|Maximální počet fondu hustota|neuvedeno|50|100|100|100|100|
|Minimální/maximální elastického fondu kliknutím – zastavení|neuvedeno|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24|
|Počet replik|3|3|3|3|3|3|
|Více AZ|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|
|Přečtěte si horizontální navýšení kapacity|Ano|Ano|Ano|Ano|Ano|Ano|
|Zahrnuté úložiště zálohování|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|
|||

#### <a name="generation-5-compute-platform"></a>Výpočetní platforma běžící generace 5
|Úroveň výkonu|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|Generování H/W|5|5|5|5|5|5|5|5|
|Virtuální jádra|2|4|8|16|24|32|40|80|
|Paměť (GB)|11|22|44|88|132|176|220|440|
|Podpora Columnstore|Ano|Ano|Ano|Ano|Ano|Ano|Ano|Ano|
|Úložiště OLTP v paměti (GB)|1.571|3,142|6.284|15.768|25.252|37.936|52.22|131.64|
|Typ úložiště|Místní disk SSD|Místní disk SSD|Místní disk SSD|Místní disk SSD|Místní disk SSD|Místní disk SSD|Místní disk SSD|Místní disk SSD|
|Vstupně-výstupní latence (přibližné)|1 až 2 ms (zápis)<br>1 až 2 ms (čtení)|1 až 2 ms (zápis)<br>1 až 2 ms (čtení)|1 až 2 ms (zápis)<br>1 až 2 ms (čtení)|1 až 2 ms (zápis)<br>1 až 2 ms (čtení)|1 až 2 ms (zápis)<br>1 až 2 ms (čtení)|1 až 2 ms (zápis)<br>1 až 2 ms (čtení)|1 až 2 ms (zápis)<br>1 až 2 ms (čtení)|1 až 2 ms (zápis)<br>1 až 2 ms (čtení)|
|Maximální velikost dat (GB)|1024|1024|1024|1024|2 048|4 096|4 096|4 096|
|Maximální velikost protokolu|307|307|307|307|614|1229|1229|1229|
|Databáze TempDB size(DB)|64|128|256|384|384|384|384|384|
|Cíl vstupně-výstupních operací (64 KB)|5000|10000|20000|40000|60000|80000|100000|200000
|Maximální počet souběžných pracovních procesů (požadavků)|210|420|840|1680|2520|3360|5040|8400|
|Maximální povolené relace|30000|30000|30000|30000|30000|30000|30000|30000|
|Maximální počet fondu hustota|neuvedeno|50|100|100|100|100|100|100|
|Minimální/maximální elastického fondu kliknutím – zastavení|neuvedeno|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24|0, 0,5, 1, 2, 4, 8, 16, 24, 32|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40, 80|
|Počet replik|3|3|3|3|3|3|3|3|
|Více AZ|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|neuvedeno|
|Přečtěte si horizontální navýšení kapacity|Ano|Ano|Ano|Ano|Ano|Ano|Ano|Ano|
|Zahrnuté úložiště zálohování|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|Velikost databáze X 1|
|||

Pokud se všechny virtuální jádra pro elastický fond je zaneprázdněný, každá databáze ve fondu obdrží stejné množství výpočetních prostředků pro zpracování dotazů. Služba SQL Database poskytuje spravedlivé sdílení prostředků mezi databázemi tím, že zajišťuje rovnoměrné rozdělení výpočetního času. Spravedlivé sdílení prostředků elastického fondu je kromě libovolným množstvím prostředků jinak zaručena pro každou databázi, pokud je minimální počet virtuálních jader na databázi nastaven na nenulovou hodnotu.

### <a name="database-properties-for-pooled-databases"></a>Vlastnosti databáze pro databáze ve fondu

Následující tabulka popisuje vlastnosti pro databáze ve fondu.

| Vlastnost | Popis |
|:--- |:--- |
| Maximální počet virtuálních jader na databázi |Maximální počet virtuálních jader, které může použít libovolnou databázi ve fondu, pokud jsou dostupné v závislosti na využití ostatními databázemi ve fondu. Maximální počet virtuálních jader na databázi není garancí prostředků pro databázi. Toto nastavení je globální a platí pro všechny databáze ve fondu. Nastavit maximální počet virtuálních jader na databázi dostatečně vysoký, aby se pro zpracování špičky využití databáze. Očekává se určitý stupeň předimenzování, protože fond obecně předpokládá studené a horké vzory používání, při nichž nenastávají špičky u všech databází ve stejnou chvíli.|
| Min virtuálních jader na databázi |Minimální počet virtuálních jader, které je zaručeno, že všechny databáze ve fondu. Toto nastavení je globální a platí pro všechny databáze ve fondu. Min virtuálních jader na databázi může být nastaven na hodnotu 0 a výchozí hodnota je také. Tato vlastnost nastavena na libovolné místo mezi 0 a využití průměrné virtuálních jader na databázi. Součinu počtu databází ve fondu a minimální virtuálních jader na databázi nesmí přesáhnout virtuálních jader na fond.|
| Max. úložiště na databázi |Maximální velikosti databáze nastavena podle uživatele pro databázi ve fondu. Databáze ve fondu sdílejí úložiště přidělené fondu, proto je velikost databáze můžete oslovit omezena na menší zbývající úložiště fondu a velikost databáze. Maximální velikost databáze odkazuje na maximální velikost datových souborů a nezahrnuje místo, které využívají soubory protokolu. |
|||
 
## <a name="next-steps"></a>Další postup

- Zobrazit [nejčastější dotazy k SQL Database](sql-database-faq.md) odpovědi na nejčastější dotazy.
- Zobrazit [limity prostředků přehled Azure SQL Database](sql-database-resource-limits.md) informace o omezeních na úrovni serveru a předplatné.
- Informace o obecných omezeních Azure najdete v tématu [předplatného Azure a limity, kvóty a omezení](../azure-subscription-service-limits.md).
