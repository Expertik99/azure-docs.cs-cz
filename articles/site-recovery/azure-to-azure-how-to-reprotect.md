---
title: Operace opětovného zapnutí ochrany převzetí služeb při selhání virtuálních počítačů Azure zpět do primární oblasti Azure pomocí Azure Site Recovery | Dokumentace Microsoftu
description: Popisuje, jak znovunastavení ochrany virtuálních počítačů Azure do sekundární oblasti po převzetí služeb při selhání z primární oblasti, pomocí Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: rajanaki
ms.openlocfilehash: 9759e209f15622d70aaa833a993234863ac1053c
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2018
ms.locfileid: "37918862"
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>Operace opětovného zapnutí ochrany převzetí služeb při selhání virtuálních počítačů Azure do primární oblasti


Pokud jste [převzetí služeb při selhání](site-recovery-failover.md) virtuálních počítačů Azure z jedné oblasti do druhé pomocí [Azure Site Recovery](site-recovery-overview.md), spouštění virtuálních počítačů v sekundární oblasti, ocitne v nechráněném stavu. Pokud se navrácení služeb po obnovení virtuálních počítačů do primární oblasti, je třeba provést následující kroky:

- Znovunastavení ochrany virtuálních počítačů v sekundární oblasti, aby se začaly replikovat do primární oblasti. 
- Po dokončení opětovného nastavování ochrany a replikaci virtuálních počítačů, můžete je převzít služby ze sekundární do primární oblasti.

> [!WARNING]
> Pokud jste [migrovat](migrate-overview.md#what-do-we-mean-by-migration) počítače z primární do sekundární oblasti, virtuální počítač přesunout do jiné skupiny prostředků nebo odstranění virtuálního počítače Azure, a nemůžete znovunastavení ochrany virtuálního počítače nebo po obnovení navrátit.


## <a name="prerequisites"></a>Požadavky
1. Převzetí služeb virtuálního počítače z primární do sekundární oblasti musí být potvrzeny.
2. Cílové primární lokalitě musí být k dispozici. proto byste měli mít přístup k nebo vytvářet prostředky v této oblasti.

## <a name="reprotect-a-vm"></a>Znovunastavení ochrany virtuálního počítače

1. V **trezor** > **replikované položky**, klikněte pravým tlačítkem na neúspěšný selhání pro virtuální počítač a vyberte **znovu nastavit ochranu**. Směr opětovného nastavování ochrany by měly vykazovat ze sekundární do primární. 

  ![Operace opětovného zapnutí ochrany](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Projděte si sady skupiny, sítě, úložiště a dostupnosti prostředků. Pak klikněte na **OK**. Pokud jsou všechny prostředky označené jako nové, jsou vytvořeny jako součást procesu opětovného nastavování ochrany.
3. Úloha opětovného nastavování ochrany přidá do cílové lokality nejnovější data. Po dokončení, která se provádí rozdílová replikace. Potom můžete převzít služby zpět do primární lokality. Můžete vybrat účet úložiště nebo sítě, kterou chcete použít během znovu nastavit ochranu, pomocí možnosti přizpůsobit.

  ![Možnost přizpůsobení](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>Přizpůsobení nastavení zpětné replikace

Můžete přizpůsobit následující vlastnosti cíle VMe během opětovného nastavování ochrany.

![Přizpůsobení](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Vlastnost |Poznámky  |
|---------|---------|
|Cílová skupina prostředků     | Upravte cílovou skupinu prostředků, ve kterém se vytvoří virtuální počítač. Jako součást opětovného nastavování ochrany, cílový virtuální počítač je odstraněný. Můžete zvolit novou skupinu prostředků, pod kterým chcete vytvořit virtuální počítač po převzetí služeb při selhání.        |
|Cílová virtuální síť     | Cílová síť nelze změnit během úlohy opětovného zapnutí ochrany. Chcete-li změnit síť znovu mapování sítě.         |
|Cílové úložiště (sekundární virtuální počítač nepoužívá spravované disky)     | Můžete změnit účet úložiště, který používá virtuální počítač po převzetí služeb při selhání.         |
|Repliky spravovaných disků (sekundární virtuální počítač používá spravované disky)    | Site Recovery vytvoří v primární oblasti zrcadlící spravované disky sekundárního virtuálního počítače repliky spravovaných disků.         | 
|Úložiště mezipaměti     | Můžete zadat účet úložiště mezipaměti, které se použijí při replikaci. Ve výchozím nastavení je možné vytvořit nový účet úložiště mezipaměti, pokud neexistuje.         |
|Skupina dostupnosti     |Pokud virtuální počítač v sekundární oblasti je součástí skupiny dostupnosti, můžete sadu dostupnosti pro cílový virtuální počítač v primární oblasti. Ve výchozím nastavení Site Recovery se pokusí vyhledat existující dostupnosti v primární oblasti a použít ho. Během přizpůsobování můžete zadat novou skupinu dostupnosti.         |


### <a name="what-happens-during-reprotection"></a>Co se stane během opětovného nastavování ochrany?

Ve výchozím nastavení dojde k následujícímu:

1. Účet úložiště mezipaměti se vytvoří v primární oblasti
2. Pokud cílový účet úložiště (původního účtu úložiště v primární oblasti) neexistuje, vytvoří se nový. Název účtu úložiště přiřazené je název účtu úložiště používá sekundární virtuální počítač, příponu "Azure Site Recovery".
3. Pokud se váš virtuální počítač používá spravované disky, repliky spravovaných disků se vytvoří v primární oblast pro ukládání dat replikovaných z disků sekundárního virtuálního počítače. 
4. Pokud cílová skupina dostupnosti neexistuje, je vytvořen nový jako součást úlohy opětovného zapnutí ochrany v případě potřeby. Pokud jste upravili nastavení opětovného nastavování ochrany, použije se vybrané sady.

Když spustíte úlohu znovunastavení ochrany a cílový virtuální počítač existuje, dojde k následující položky:

1. Požadované součásti jsou vytvořeny jako součást operace opětovného zapnutí ochrany. Pokud již existují, jsou opakovaně využívány.
2. Straně cíle, které virtuální počítač je zapnutý vypnuto, pokud běží.
3. Cílový disk virtuálního počítače na straně je službou Site Recovery zkopírován do kontejneru, jako objekt blob pro dosazení hodnot.
4. Na cílové straně virtuálního počítače se pak odstraní.
5. Používá aktuální zdroj objektu blob počáteční hodnoty na straně (sekundární) virtuálního počítače k replikaci. Tím se zajistí, že jsou replikovány pouze rozdíly.
6. Hlavní změny mezi zdrojový disk a blob počáteční hodnoty jsou synchronizovány. To může trvat nějakou dobu.
7. Po dokončení operace opětovného zapnutí ochrany úlohy rozdílové replikace začne a vytváří bod obnovení podle zásady replikace.
8. Po znovunastavení ochrany úloha úspěšně dokončí, virtuální počítač přejde do chráněném stavu.

## <a name="next-steps"></a>Další postup

Po aktivaci ochrany virtuálního počítače můžete spustit převzetí služeb při selhání. Převzetí služeb při vypnutí virtuálního počítače v sekundární oblasti a vytvoří a spustí virtuální počítač v primární oblasti s malý výpadek. Doporučujeme proto vyberte čas a spustit testovací převzetí služeb, ale inicializaci úplné převzetí služeb při selhání do primární lokality. [Další informace](site-recovery-failover.md) o převzetí služeb při selhání.

