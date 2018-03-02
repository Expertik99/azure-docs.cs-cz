---
title: "Cenové úrovně v Azure databáze pro databázi MySQL"
description: "Tento článek popisuje cenové úrovně ve službě Azure Database pro databázi MySQL."
services: mysql
author: jan-eng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 6bd24da05c337a902ce0e4a2b9acf22a809eb653
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="azure-database-for-mysql-pricing-tiers"></a>Azure databáze pro databázi MySQL cenové úrovně

Databázi Azure pro server databáze MySQL lze vytvořit v jednom ze tří různých cenových úrovní - Basic, obecné účely a paměťově optimalizovaná. Cenové úrovně jsou rozlišené pomocí objem výpočtů v vCores, který může být zřízen, paměť na vCore a technologie úložiště používat k ukládání dat. Všechny prostředky připravené na úrovni serveru MySQL. Server může mít jednu nebo více databází.

|    | **Basic** | **Obecné účely** | **Paměťově optimalizované** |
|:---|:----------|:--------------------|:---------------------|
| Výpočetní generování | Gen 4, Gen 5 | Gen 4, Gen 5 | Gen 5 |
| vCores | 1, 2 | 2, 4, 8, 16, 32 |2, 4, 8, 16, 32 |
| Paměť na vCore | 1x | 2x Basic | 2 x obecné účely |
| Velikost Storage | 5 GB až 1 TB | 5 GB až 1 TB | 5 GB až 1 TB |
| Typ úložiště | Úložiště Azure úrovně Standard | Azure Premium Storage | Azure Premium Storage |
| Doba uchovávání záloh databáze | 7 - 35 dní | 7 - 35 dní | 7 - 35 dní |

V následující tabulce lze použít jako východisko pro výběr cenové úrovně:

| Cenová úroveň | Cílová zátěž |
|:-------------|:-----------------|
| Basic | Úlohy vyžadující nízký výpočetní a I/O výkon. Příkladem mohou být servery používané pro vývoj a testování nebo pro malé a nepříliš často využívané aplikace. |
| Obecné použití | Většina obchodních úloh vyžaduje vyvážené výpočetní a paměťové prostředky se škálovatelnou I/O propustností. Mezi příklady patří server pro hostování webové a mobilní aplikace a jiné podnikové aplikace.|
| Paměťově optimalizované | Vysoce výkonné databáze procesy vyžadující výkonu v paměti pro rychlejší zpracování transakcí a vyšší souběžnosti. Mezi příklady patří server pro zpracování dat v reálném čase a vysoký výkon transakcí a analytické aplikací.|

Po vytvoření serveru počet vCores lze změnit nahoru nebo dolů v sekundách. Můžete také nezávisle upravit velikost úložiště nahoru a dobu uchovávání záloh nahoru nebo dolů s žádné výpadky aplikací. Najdete v části škálování níže další podrobnosti.

## <a name="compute-generations-vcores-and-memory"></a>Výpočetní generace, vCores a paměti

Výpočetní prostředky jsou uvedeny jako vCores, představující logického procesoru základní hardware. V současné době dvou generací výpočetní Gen 4 a 5 generace jsou nabízena můžete vybírat. Logické CPU 4. generace jsou založené na procesorech Intel E5-2673 v3 (Haswell) 2,4 GHz. Logické CPU 5. generace jsou založené na procesorech Intel E5-2673 v4 (Broadwell) 2,3 GHz.

V závislosti na cenovou úroveň je zajištěna každý vCore s určitou velikostí paměti. Při zvýšení nebo snížení počtu vCores pro váš server, paměť zvýší nebo sníží úměrně. Úroveň obecné účely poskytuje double množství paměti na vCore ve srovnání s základní vrstvě. Paměťově optimalizovaná vrstvy poskytuje double množství paměti ve srovnání s vrstvě obecné účely.

## <a name="storage"></a>Úložiště

Úložiště, který zřídíte je množství kapacity úložiště k dispozici k vaší databázi Azure pro server databáze MySQL. Úložiště se používá pro soubory databáze, dočasné soubory, transakční protokoly a MySQL server protokoly. Celkovou velikost úložiště, který zřídíte také definuje dostupná kapacita vstupně-výstupních operací na váš server:

|    | **Basic** | **Obecné účely** | **Paměťově optimalizované** |
|:---|:----------|:--------------------|:---------------------|
| Typ úložiště | Úložiště Azure úrovně Standard | Azure Premium Storage | Azure Premium Storage |
| Velikost Storage | 5 GB až 1 TB | 5 GB až 1 TB | 5 GB až 1 TB |
| Velikost úložiště přírůstku | 1 GB | 1 GB | 1 GB |
| IOPS | Proměnná |3 IOPS/GB<br/>Min 100 IOPS | 3 IOPS/GB<br/>Min 100 IOPS |

Během a po vytvoření serveru se dá přidat kapacitu další úložiště. Základní vrstvě neposkytuje záruku IOPS. V obecné účely a paměťově optimalizované cenové úrovně IOPS škálování se velikost zřízeného úložiště v poměru 3:1.

Můžete monitorovat vaší spotřeby vstupně-výstupních operací v portálu Azure nebo pomocí rozhraní příkazového řádku Azure. Metriku relevantní pro monitorování jsou [limit úložiště, procento úložiště, používá úložiště a vstupně-výstupní operace procent](concepts-monitoring.md).

## <a name="backup"></a>Backup

Služba automaticky provede zálohování serveru. Doba uchování minimální zálohy je sedm dní. Můžete nastavit dobu uchování o délce až 35 dnů. Uchovávání lze upravit kdykoli po dobu platnosti na server. Můžete zvolit místně redundantní a geograficky redundantní zálohy. Geograficky redundantní zálohy jsou uloženy ve [spárovat geografické oblasti](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) oblasti vašeho serveru se vytvoří v. To poskytuje úroveň ochrany v případě havárie. Získáte také možnost obnovit do jiné Azure oblasti, ve kterém je služba k dispozici s geograficky redundantní zálohy vašeho serveru. Všimněte si, že není možné změnit ze dvou možností úložiště záloh, jakmile je vytvořena serveru.

## <a name="scale-resources"></a>Škálování prostředků

Po vytvoření serveru, můžete nezávisle změnit vCores, velikost úložiště a dobu uchovávání záloh. Po vytvoření serveru nelze změnit typ úložiště pro zálohy nebo cenovou úroveň. vCores a období uchovávání záloh můžete škálovat nahoru nebo dolů. Velikost úložiště může být pouze zvýšena. Škálování prostředků můžete udělat buď prostřednictvím portálu nebo rozhraní příkazového řádku Azure. Příklad pro škálování pomocí rozhraní příkazového řádku můžete najít [zde](scripts/sample-scale-server.md).

Při změně počtu vCores, se s nové přidělení výpočetní vytvoří kopii původního serveru. Jakmile nový server je spuštěný a funkční, připojení jsou přepnutí na nový server. Během stručný chvíli, kdy systému přepne na nový server žádná nová připojení lze navázat a jsou všechny nepotvrzené transakce vráceny zpět. Toto okno se liší, ale ve většině případů je méně než minutu.

Škálování úložiště a změnit období uchovávání záloh jsou true online operace. Neexistuje žádný výpadek ani dopad do vaší aplikace. Jako IOPS škálování se velikost zřízeného úložiště, můžete zvýšit IOPS, která je k dispozici na váš server ve vertikálním navýšení kapacity úložiště.

## <a name="pricing"></a>Ceny

Zkontrolujte službu [stránce s cenami](https://azure.microsoft.com/pricing/details/mysql/) pro co nejaktuálnější informace o cenách. Zobrazit vaše náklady, požadované konfigurace, [portál Azure](https://portal.azure.com/#create/Microsoft.MySQLServer) ukazuje měsíční náklady v **cenová úroveň** karty založené na vybrané možnosti. Pokud nemáte předplatné Azure, můžete získat odhadované ceny Azure cenové kalkulačky. Navštivte [Azure cenové kalkulačky](https://azure.microsoft.com/pricing/calculator/) web, pak klikněte na tlačítko **přidat položky**, rozbalte položku **databáze** kategorie a zvolte **Azure Database pro databázi MySQL** přizpůsobit možnosti.

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [vytvoření databáze MySQL serveru v portálu.](howto-create-manage-server-portal.md)
- Zjistěte, jak [sledování a škálování Azure Database pro MySQL server pomocí rozhraní příkazového řádku Azure](scripts/sample-scale-server.md)
- Další informace o [služby omezení](concepts-limits.md)
