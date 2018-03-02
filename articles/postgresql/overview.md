---
title: "Přehled služby relačních databází Azure Database for PostgreSQL"
description: "Přehled služby relačních databází Azure Database for PostgreSQL."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 02/28/2018
ms.openlocfilehash: ef7f710fb39c3745ff5a20a73cd4dc5c1fa1fece
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="what-is-azure-database-for-postgresql"></a>Co je Azure Database for PostgreSQL?

Azure Database for PostgreSQL je služba relačních databází v cloudu Microsoftu vytvořená pro vývojáře na základě komunitní verze opensourcového databázového stroje [PostgreSQL](https://www.postgresql.org/). Tato služba je ve verzi Public Preview. Azure Database for PostgreSQL nabízí:

- Integrovanou vysokou dostupnost bez dalších poplatků
- Předvídatelný výkon a všeobecné ceny při průběžných platbách
- Průběžné škálování během několika sekund
- Zabezpečení pro ochranu neaktivních uložených dat i přenášených dat
- Automatické zálohování a obnovení k určitému bodu v čase po dobu až 35 dnů
- Zabezpečení a dodržování předpisů na podnikové úrovni

Všechny tyto možnosti nevyžadují téměř žádnou správu a jsou k dispozici bez dalších poplatků. Díky těmto možnostem se můžete zaměřit na rychlý vývoj aplikací a zkrácení doby dodání produktu na trh, namísto vynakládání prostředků a drahocenného času na správu virtuálních počítačů a infrastruktury. Kromě toho můžete pokračovat ve vývoji aplikací pomocí libovolných opensourcových nástrojů a platforem a dodávat je tak rychle a efektivně, jak váš podnik vyžaduje, aniž byste se museli učit nové dovednosti. 

Tento článek je úvodem do základních konceptů služby Azure Database for PostgreSQL a funkcí týkajících se výkonu, škálovatelnosti a možností správy. Tyto rychlé starty vám pomůžou začít:

- [Vytvoření Azure Database for PostgreSQL pomocí webu Azure Portal](quickstart-create-server-database-portal.md)
- [Vytvoření Azure Database for PostgreSQL pomocí Azure CLI](quickstart-create-server-database-azure-cli.md)

Řadu ukázek v Azure CLI najdete tady:

- [Ukázky v Azure CLI pro službu Azure Database for PostgreSQL](./sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Úprava výkonu a škálování během několika sekund
Služba Azure Database for PostgreSQL nabízí tři cenové úrovně: Basic, Obecné účely a Optimalizovaný pro paměť. Každá úroveň nabízí různé schopnosti prostředků pro podporu vašich úloh databáze. Svou první aplikaci můžete postavit na malé databázi za pár dolarů na měsíc a pak upravit škálování tak, aby vyhovovalo potřebám vašeho řešení. Dynamická škálovatelnost umožňuje databázím transparentně reagovat na rychle se měnící požadavky na prostředky. Platíte jenom za prostředky, které potřebujete, a jen tehdy, když je potřebujete. Podrobnosti najdete v tématu [Cenové úrovně](concepts-pricing-tiers.md).

## <a name="monitoring-and-alerting"></a>Monitorování a upozorňování
Jak se rozhodnout, kdy přidat a kdy odebrat výkon? Můžete použít integrované funkce monitorování a upozorňování Azure. Pomocí těchto nástrojů můžete rychle posoudit dopad vertikálního navýšení nebo snížení kapacity v závislosti na stávajících nebo předpokládaných požadavcích na výkon nebo úložiště. Podrobnosti najdete v tématu [Upozornění](howto-alert-on-metric.md).

## <a name="keep-your-app-and-business-running"></a>Udržujte své aplikace a podnikáni v chodu
Dostupnost služby Azure se smlouvou o úrovní služeb (SLA) dosahuje špičkové hodnoty 99,99 % (není k dispozici ve verzi Preview), protože staví na globální síti Microsoftem spravovaných datacenter. Může tedy udržet vaše aplikace v nepřetržitém provozu každý den po celý rok. S každým serverem Azure Database for PostgreSQL využíváte integrované zabezpečení, odolnost proti chybám a ochranu dat, které byste jinak museli kupovat nebo navrhovat, sestavovat a spravovat. Azure Database for PostgreSQL nabízí v každé cenové úrovni komplexní sadu funkcí a možností pro zajištění provozní kontinuity, které můžete využít k zahájení a udržení provozu. Můžete využít [obnovení databáze do určitého bodu v čase](howto-restore-server-portal.md), a to až 35 dnů zpět. Kromě toho, pokud dojde k výpadku datacentra hostujícího vaše databáze, můžete databáze obnovit z geograficky redundantních kopií nedávných záloh.

## <a name="secure-your-data"></a>Zabezpečení dat
Databázové služby Azure mají tradici zabezpečení dat, kterou služba Azure Database for PostgreSQL zachovává díky funkcím, které omezují přístup, chrání data a pomáhají s monitorováním aktivity. Informace o zabezpečení platformy Azure najdete v [Centru zabezpečení Azure](https://www.microsoft.com/TrustCenter/Security/default.aspx).

Služba Azure Database for PostgreSQL používá pro neaktivní uložená data šifrování úložiště. Data včetně záloh se šifrují na disku (s výjimkou dočasných souborů vytvořených databázovým strojem při spouštění dotazů). Služba používá 256bitové šifrování AES, které je součástí šifrování úložiště Azure, a klíče jsou spravované systémem. Šifrování úložiště je vždycky aktivní a není možné ho zakázat.

Ve výchozím nastavení je ve službě Azure Database for PostgreSQL nakonfigurované vyžadování [zabezpečení připojení protokolem SSL](./concepts-ssl-connection-security.md) pro data přenášená přes síť. Díky vynucování připojení SSL mezi databázovým serverem a klientskými aplikacemi se šifruje datový proud mezi serverem a vaší aplikací, což pomáhá chránit před napadením útočníky, kteří se vydávají za prostředníky.  Volitelně můžete vyžadování SSL pro připojení k vaší databázové službě zakázat, pokud vaše klientská aplikace nepodporuje připojení SSL.

## <a name="next-steps"></a>Další kroky
- Na [stránce s cenami](https://azure.microsoft.com/pricing/details/postgresql/) najdete porovnání nákladů a kalkulačky.
- Začněte [vytvořením první služby Azure Database for PostgreSQL](./quickstart-create-server-database-portal.md).
- Sestavte svou první aplikaci v jazyce Python, PHP, Ruby, C\#, Java, Node.js: [Knihovny připojení](./concepts-connection-libraries.md)
