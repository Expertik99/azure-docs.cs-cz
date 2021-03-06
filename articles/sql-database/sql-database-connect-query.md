---
title: Připojení ke službě Azure SQL Database a její dotazování – Rychlé starty | Microsoft Docs
description: Rychlé starty pro službu Azure SQL Database, které ukazují připojení k databázi SQL Azure a její dotazování.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: mvc
ms.devlang: ''
ms.topic: quickstart
ms.date: 04/24/2018
ms.author: carlrab
ms.openlocfilehash: ec39c5ad0771c2bc78655e52c58949db6e9b3353
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/28/2018
ms.locfileid: "32186007"
---
# <a name="azure-sql-database-connect-and-query-quickstarts"></a>Připojení ke službě Azure SQL Database a její dotazování – Rychlé starty

Následující dokument obsahuje odkazy na příklady Azure, které ukazují připojení k databázi SQL Azure a její dotazování. Obsahuje také několik doporučení týkající se protokolu TLS (Transport Level Security).

## <a name="quickstarts"></a>Šablony Rychlý start

| |  |
|---|---|
|[SQL Server Management Studio](sql-database-connect-query-ssms.md)|Tento rychlý start ukazuje použití SSMS k připojení k databázi SQL Azure a následné použití příkazů jazyka Transact-SQL k dotazování, vkládání, aktualizaci a odstraňování dat v databázi.|
|[SQL Operations Studio](https://docs.microsoft.com/sql/sql-operations-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)|Tento rychlý start ukazuje použití nástroje SQL Operations Studio (Preview) pro připojení k databázi SQL Azure a následné použití příkazů jazyka Transact-SQL (T-SQL) k vytvoření databáze TutorialDB používané v kurzech pro SQL Operations Studio (Preview).|
|[Azure Portal](sql-database-connect-query-portal.md)|Tento rychlý start ukazuje použití editoru dotazů pro připojení k databázi SQL a následné použití příkazů jazyka Transact-SQL k dotazování, vkládání, aktualizaci a odstraňování dat v databázi.|
|[Visual Studio Code](sql-database-connect-query-vscode.md)|Tento rychlý start ukazuje použití Visual Studio Code pro připojení k databázi SQL Azure a následné použití příkazů jazyka Transact-SQL k dotazování, vkládání, aktualizaci a odstraňování dat v databázi.|
|[.NET se sadou Visual Studio](sql-database-connect-query-dotnet-visual-studio.md)|Tento rychlý start ukazuje použití rozhraní .NET Framework a sady Visual Studio k vytvoření programu v jazyce C# pro připojení k databázi SQL Azure a použití příkazů jazyka Transact-SQL k dotazování dat.|
|[.NET Core](sql-database-connect-query-dotnet-core.md)|Tento rychlý start ukazuje použití .NET Core v systému Windows, Linux nebo macOS k vytvoření programu v jazyce C# pro připojení k databázi SQL Azure a použití příkazů jazyka Transact-SQL k dotazování dat.|
|[Go](sql-database-connect-query-go.md)|Tento rychlý start ukazuje použití jazyka Go pro připojení k databázi SQL Azure. Ukazuje se také použití příkazů jazyka Transact-SQL k dotazování a úpravě dat.|
|[Java](sql-database-connect-query-java.md)|Tento rychlý start ukazuje použití Javy pro připojení k databázi SQL Azure a následné použití příkazů jazyka Transact-SQL k dotazování dat.|
|[Node.js](sql-database-connect-query-nodejs.md)|Tento rychlý start ukazuje použití Node.js k vytvoření programu pro připojení k databázi SQL Azure a použití příkazů jazyka Transact-SQL k dotazování dat.|
|[PHP](sql-database-connect-query-php.md)|Tento rychlý start ukazuje použití PHP k vytvoření programu pro připojení k databázi SQL Azure a použití příkazů jazyka Transact-SQL k dotazování dat.|
|[Python](sql-database-connect-query-python.md)|Tento rychlý start ukazuje použití Pythonu pro připojení k databázi SQL Azure a použití příkazů jazyka Transact-SQL k dotazování dat. |
|[Ruby](sql-database-connect-query-ruby.md)|Tento rychlý start ukazuje použití Ruby k vytvoření programu pro připojení k databázi SQL Azure a použití příkazů jazyka Transact-SQL k dotazování dat.|
|||

## <a name="tls-considerations-for-sql-database-connectivity"></a>Důležité informace o použití protokolu TLS pro připojení k databázi SQL
Protokol TLS (Transport Layer Security) používají všechny ovladače, které Microsoft používá nebo podporuje pro připojení k databázi SQL Azure. Není potřeba žádná zvláštní konfigurace. Pro všechna připojení k serveru SQL Server nebo k databázi SQL Azure doporučujeme nastavit u všech aplikací následující konfigurace nebo jejich ekvivalenty:

 - **Encrypt = On**
 - **TrustServerCertificate = Off**

Některé systémy pro uvedená klíčová slova konfigurací používají rozdílná, ale ekvivalentní klíčová slova. Tyto konfigurace zajistí, že ovladač klienta ověří identitu certifikátu TLS přijatého ze serveru.

Také doporučujeme zakázat v klientovi protokoly TLS 1.1 a 1.0, pokud potřebujete dodržovat standard PCI–DSS (Payment Card Industry – Data Security Standard).

Ovladače od jiných výrobců nemusí ve výchozím nastavení protokol TLS používat. To může při připojování k databázi SQL Azure hrát důležitou roli. Aplikace s integrovanými ovladači vám nemusí umožnit řídit tato nastavení připojení. Doporučujeme, abyste si ověřili zabezpečení takovýchto ovladačů a aplikací, dříve než je použijete v systémech, které pracují s důvěrnými osobními údaji.

## <a name="next-steps"></a>Další kroky

Informace o připojení k architektuře najdete v článku o [architektuře připojení k databázi SQL Azure](sql-database-connectivity-architecture.md).