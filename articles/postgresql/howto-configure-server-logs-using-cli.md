---
title: "Konfigurace a přístup k protokolům serveru pro PostgreSQL pomocí rozhraní příkazového řádku Azure"
description: "Tento článek popisuje postup konfigurace a přístup v protokolech serveru v Azure databázi PostgreSQL pomocí příkazového řádku Azure CLI."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: e12a8907b641b4591ed5ff9fdd5d8458eb75525e
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Konfigurace a přístup k protokolům serveru pomocí rozhraní příkazového řádku Azure
Můžete si stáhnout PostgreSQL protokoly chyb serveru pomocí rozhraní příkazového řádku (Azure CLI). Přístup k protokoly transakcí však není podporován. 

## <a name="prerequisites"></a>Požadavky
Chcete-li krok tímto průvodcem postupy, je třeba:
- [Azure databázi PostgreSQL serveru.](quickstart-create-server-database-azure-cli.md)
- Nainstalujte [Azure CLI 2.0](/cli/azure/install-azure-cli) nástroj příkazového řádku nebo pomocí prostředí cloudové služby Azure v prohlížeči.

## <a name="configure-logging-for-azure-database-for-postgresql"></a>Konfigurace protokolování pro databázi Azure pro PostgreSQL
Můžete nastavit server pro přístup k dotazu protokoly a protokoly chyb. Protokoly chyb může obsahovat informace o automatické vakuu, připojení a kontrolní body.
1. Povolení protokolování
2. Aktualizace protokolu\_prohlášení a protokolu\_min\_doba trvání\_příkaz Povolit protokolování dotazu
3. Doba uchování aktualizace

Další informace najdete v tématu [přizpůsobení parametry konfigurace serveru](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-database-for-postgresql-server"></a>Seznam protokolů pro databázi Azure pro PostgreSQL server
Chcete-li zobrazit seznam souborů k dispozici protokolu pro váš server, spusťte [az postgres protokoly serveru seznamu](/cli/azure/postgres/server-logs#az_postgres_server_logs_list) příkaz.

Můžete seznam souborů protokolu pro server **mydemoserver.postgres.database.azure.com** ve skupině prostředků **myresourcegroup**a přesměrování na textový soubor s názvem **protokolu\_ soubory\_seznam.txt.**
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Stažení protokolů místně ze serveru
[Az postgres – protokoly serveru stáhnout](/cli/azure/postgres/server-logs#az_postgres_server_logs_download) příkazu lze stáhnout jednotlivých protokolových souborů pro váš server. 

Tento příklad stáhne konkrétním souboru protokolu pro server **mydemoserver.postgres.database.azure.com** ve skupině prostředků **myresourcegroup** na vašem místním prostředí.
```azurecli-interactive
az postgres server-logs download --name 20170414-mydemoserver-postgresql.log --resource-group myresourcegroup --server mydemoserver
```
## <a name="next-steps"></a>Další postup
- Další informace o protokolech serveru najdete v tématu [protokoly serveru v Azure databáze PostgreSQL](concepts-server-logs.md)
- Další informace o parametry serveru, najdete v části [přizpůsobit parametry konfigurace serveru pomocí rozhraní příkazového řádku Azure](howto-configure-server-parameters-using-cli.md)
