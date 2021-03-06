---
title: 'Azure portal: geografická replikace SQL Database | Dokumentace Microsoftu'
description: Konfigurace geografické replikace pro Azure SQL Database webu Azure portal a zahájit převzetí služeb při selhání
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: carlrab
ms.openlocfilehash: 27fb8f369ad23592902c05fe5275fc54bc6cf148
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090465"
---
# <a name="configure-active-geo-replication-for-azure-sql-database-in-the-azure-portal-and-initiate-failover"></a>Konfigurace aktivní geografické replikace pro Azure SQL Database webu Azure portal a zahájit převzetí služeb při selhání

V tomto článku se dozvíte, jak nakonfigurovat aktivní geografickou replikaci pro databázi SQL v [webu Azure portal](http://portal.azure.com) a k zahájení převzetí služeb při selhání.

K zahájení převzetí služeb při selhání pomocí webu Azure portal, najdete v článku [zahájit plánované nebo neplánované převzetí služeb při selhání pro službu Azure SQL Database pomocí webu Azure portal](sql-database-geo-replication-portal.md).

Pokud chcete nakonfigurovat aktivní geografickou replikaci s využitím webu Azure portal, potřebujete následující prostředek:

* Azure SQL database: primární databáze, který chcete replikovat do jiné geografické oblasti.

> [!Note]
Aktivní geografickou replikaci musí být mezi databázemi ve stejném předplatném.

## <a name="add-a-secondary-database"></a>Přidání sekundární databáze
Následující kroky vytvoření nové sekundární databáze v partnerství geografické replikace.  

Přidání sekundární databáze, musí být vlastníkem předplatného nebo spoluvlastník.

Sekundární databáze má stejný název jako primární databáze a ve výchozím nastavení, má stejnou úroveň služby. Sekundární databáze může být izolované databáze nebo databáze v elastickém fondu. Další informace najdete v tématu [nákupní model založený na DTU](sql-database-service-tiers-dtu.md) a [nákupní model založený na virtuálních jádrech](sql-database-service-tiers-vcore.md).
Jakmile sekundární se vytvoří a nasadí, zahájení dat replikace z primární databáze nové sekundární databáze.

> [!NOTE]
> Pokud partnerská databáze již existuje (například v důsledku ukončení předchozí relaci geografické replikace) příkaz se nezdaří.
> 

1. V [webu Azure portal](http://portal.azure.com), přejděte do databáze, kterou chcete nastavit pro geografickou replikaci.
2. Na stránce služby SQL database, vyberte **geografickou replikaci**a pak vyberte oblasti se má vytvořit sekundární databáze. Můžete vybrat libovolnou oblast jiné než oblasti, který je hostitelem primární databáze, ale doporučujeme, abyste [spárované oblasti](../best-practices-availability-paired-regions.md).
   
    ![Konfigurace geografické replikace](./media/sql-database-geo-replication-portal/configure-geo-replication.png)
3. Vybrat nebo nakonfigurovat server a cenovou úroveň pro sekundární databázi.
   
    ![Konfigurovat sekundární](./media/sql-database-geo-replication-portal/create-secondary.png)
4. Volitelně můžete přidat sekundární databáze do elastického fondu. Chcete-li vytvořit sekundární databáze ve fondu, klikněte na tlačítko **elastického fondu** a vyberte fond na cílovém serveru. Fond už musí existovat v cílovém serveru. Tento pracovní postup nelze vytvořit fond.
5. Klikněte na tlačítko **vytvořit** přidat sekundární.
6. Sekundární databáze se vytvoří a spustí proces osazení.
   
    ![Konfigurovat sekundární](./media/sql-database-geo-replication-portal/seeding0.png)
7. Po dokončení procesu osazení se sekundární databáze zobrazí jeho stav.
   
    ![Synchronizace replik indexů dokončení](./media/sql-database-geo-replication-portal/seeding-complete.png)

## <a name="initiate-a-failover"></a>Zahájit převzetí služeb při selhání

Sekundární databáze můžete přepnout na primární.  

1. V [webu Azure portal](http://portal.azure.com), přejděte na primární databázi v partnerství geografickou replikaci.
2. V okně databáze SQL vyberte **všechna nastavení** > **geografickou replikaci**.
3. V **sekundárních replik** vyberte databáze, kterou chcete nové primární a klikněte na tlačítko **převzetí služeb při selhání**.
   
    ![Převzetí služeb při selhání](./media/sql-database-geo-replication-failover-portal/secondaries.png)
4. Klikněte na tlačítko **Ano** zahájíte převzetí služeb při selhání.

Příkaz okamžitě přepne sekundární databázi do primární role. 

Je na krátkou dobu, během které jsou obě databáze není k dispozici (v řádu 0 až 25 sekund), zatímco jsou přepnuté role. Pokud primární databáze má více sekundární databází, tento příkaz automaticky změní konfiguraci sekundární databáze pro připojení k nové primární. Celá operace by měla trvat necelou minutu dokončit za normálních okolností. 

> [!NOTE]
> Tento příkaz je určená pro rychlé obnovení databáze i v případě výpadku. Převzetí služeb při selhání se aktivuje bez synchronizace dat (Vynucené převzetí služeb při selhání).  Pokud je primární online a potvrzování transakcí při vydání příkazu se ztrátě dat může dojít. 
> 
> 

## <a name="remove-secondary-database"></a>Odebrání sekundární databáze
Tato operace trvale ukončí replikaci do sekundární databáze a roli sekundární databáze se změní na standardní databázi pro čtení i zápis. Pokud nefunguje připojení k sekundární databázi, příkazu úspěšné, ale sekundární fakturuje se nestane čtení a zápis až po připojení se obnoví.  

1. V [webu Azure portal](http://portal.azure.com), přejděte na primární databázi v partnerství geografickou replikaci.
2. Na stránce služby SQL database, vyberte **geografickou replikaci**.
3. V **sekundárních replik** vyberte databáze, kterou chcete odebrat z partnerství geografickou replikaci.
4. Klikněte na tlačítko **zastavení replikace**.
   
    ![Odeberte sekundární](./media/sql-database-geo-replication-portal/remove-secondary.png)
5. Otevře se okno s potvrzením. Klikněte na tlačítko **Ano** odebrání partnerství geografickou replikaci databáze. (Nastavit do databáze pro čtení a zápis není součástí žádné replikační.)

## <a name="next-steps"></a>Další postup
* Další informace o aktivní geografickou replikaci, najdete v článku [aktivní geografickou replikaci](sql-database-geo-replication-overview.md).
* Přehled zajištění provozní kontinuity podnikání a scénáře, naleznete v tématu [přehled zajištění provozní kontinuity firmy](sql-database-business-continuity.md).

