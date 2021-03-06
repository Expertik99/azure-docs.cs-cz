---
title: Migrace s minimem výpadků ke službě Azure Database for MySQL
description: Tento článek popisuje, jak provést migraci minimálními prostoji databáze MySQL do služby Azure Database for MySQL pomocí Azure Database Migration Service.
services: mysql
author: HJToland3
ms.author: jtoland
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: 55d5cf97225508d6c7c490347cfe21ced832300e
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44091710"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Migrace s minimem výpadků ke službě Azure Database for MySQL
Můžete provést migraci MySQL do služby Azure Database for MySQL s minimálními prostoji pomocí nově zavedená **Průběžná synchronizace schopností** pro [Azure Database Migration Service](https://aka.ms/get-dms) (DMS). Tato funkce omezuje množství prostoje, které se účtují podle aplikace.

## <a name="overview"></a>Přehled
DMS provede počáteční načtení místní ke službě Azure Database for MySQL a potom průběžně synchronizuje všechny nové transakce do Azure během aplikace zůstane spuštěný. Po data vyrovnat na cílové straně Azure, zastavte aplikaci pro krátké chvíle (minimální výpadek), počkejte pro poslední dávku dat (od doby, dokud aplikace není dostupná efektivně provádět žádné nové přenosy dat se zastavit aplikaci) k zachycení až do cíle a pak aktualizujte připojovací řetězec tak, aby odkazoval do Azure. Až budete hotovi, vaše aplikace bude živě v Azure!

![Průběžná synchronizace s Azure Database Migration Service](./media/howto-migrate-online/ContinuousSync.png)

- DMS migrace MySQL zdrojů je aktuálně ve verzi public preview. Pokud chcete vyzkoušet na služby migrace úloh MySQL, pokračujte na portálu. Vaše zpětná vazba je neocenitelný při řízení a usnadnit tak další vylepšení služby.

## <a name="next-steps"></a>Další postup
- Zobrazení videa [snadnou migraci MySQL nebo PostgreSQL aplikace do Azure se spravovat služby](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201), která obsahuje ukázky, ukazuje, jak migrovat aplikace MySQL do služby Azure Database for MySQL.
- [Migrace MySQL do služby Azure Database for MySQL online pomocí DMS] (https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online).
