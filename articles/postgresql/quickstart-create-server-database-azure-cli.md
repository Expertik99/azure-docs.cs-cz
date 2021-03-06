---
title: Rychlý start – Vytvoření Azure Database for PostgreSQL pomocí Azure CLI
description: Úvodní příručka k vytvoření a správě serveru Azure Database for PostgreSQL pomocí Azure CLI (rozhraní příkazového řádku).
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 04/01/2018
ms.custom: mvc
ms.openlocfilehash: 599d08668af75f6cdee2838cb16b76b04e759f32
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/27/2018
ms.locfileid: "37031222"
---
# <a name="quickstart-create-an-azure-database-for-postgresql-using-the-azure-cli"></a>Rychlý start: Vytvoření Azure Database for PostgreSQL pomocí Azure CLI
Azure Database for PostgreSQL je spravovaná služba, která umožňuje spouštět, spravovat a škálovat vysoce dostupné databáze PostgreSQL v cloudu. Azure CLI slouží k vytváření a správě prostředků Azure z příkazového řádku nebo ve skriptech. V tomto rychlém startu se dozvíte, jak vytvořit server Azure Database for PostgreSQL ve [skupině prostředků Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) pomocí rozhraní CLI Azure.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku (CLI) místně, musíte mít spuštěnou verzi Azure CLI 2.0 nebo novější. Pokud chcete zjistit nainstalovanou verzi, spusťte příkaz `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Pokud používáte rozhraní příkazového řádku místně, musíte se přihlásit ke svému účtu pomocí příkazu [az login](/cli/azure/authenticate-azure-cli?view=interactive-log-in). Z výstupu příkazu si poznamenejte vlastnost **id** pro odpovídající název předplatného.
```azurecli-interactive
az login
```

Pokud máte více předplatných, vyberte odpovídající předplatné, ve kterém se má prostředek účtovat. Ve svém účtu vyberte pomocí příkazu [az account set](/cli/azure/account#az_account_set) konkrétní ID předplatného. Zástupnou hodnotu id předplatného nahraďte vlastností **id** z výstupu příkazu **az login** pro vaše předplatné.
```azurecli-interactive
az account set --subscription <subscription id>
```

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte [skupinu prostředků Azure](../azure-resource-manager/resource-group-overview.md) pomocí příkazu [az group create](/cli/azure/group#az_group_create). Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky jako skupina. Měli byste zadat jedinečný název. Následující příklad vytvoří skupinu prostředků s názvem `myresourcegroup` v umístění `westus`.
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a>Vytvoření serveru Azure Database for PostgreSQL

Vytvořte [server Azure Database for PostgreSQL](overview.md) pomocí příkazu [az postgres server create](/cli/azure/postgres/server#az_postgres_server_create). Server obsahuje soubor databází spravovaných jako skupina. 

Následující příklad vytvoří server `mydemoserver` v umístění USA – západ, ve skupině prostředků `myresourcegroup` a s přihlašovacím jménem správce serveru `myadmin`. Jedná se o server pro **Obecné účely** **Gen 4** se 2 **virtuálními jádry**. Název serveru se mapuje na název DNS, a proto musí být v rámci Azure globálně jedinečný. Nahraďte položku `<server_admin_password>` vlastní hodnotou.
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mydemoserver  --location westus --admin-user myadmin --admin-password <server_admin_password> --sku-name GP_Gen4_2 --version 9.6
```
Hodnota parametru sku-name má formát {cenová_úroveň}\_{výpočetní_generace}\_{počet_virtuálních_jader} jako v následujících příkladech:
+ `--sku-name B_Gen4_4` se mapuje na úroveň Basic 4. generace se 4 virtuálními jádry.
+ `--sku-name GP_Gen5_32` se mapuje na úroveň pro obecné účely 5. generace se 32 virtuálními jádry.
+ `--sku-name MO_Gen5_2` se mapuje na úroveň optimalizovanou pro paměť 5. generace se 2 virtuálními jádry.

Vysvětlení platných hodnot pro jednotlivé oblasti a úrovně najdete v dokumentaci k [cenovým úrovním](./concepts-pricing-tiers.md).

> [!IMPORTANT]
> Zde zadané přihlašovací jméno a heslo správce serveru se vyžadují pro přihlášení k serveru a jeho databázím dále v tomto rychlém startu. Tyto informace si zapamatujte nebo poznamenejte pro pozdější použití.

Ve výchozím nastavení se databáze **postgres** vytvoří v rámci vašeho serveru. Databáze [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) je výchozí databáze určená pro uživatele, nástroje a aplikace třetích stran. 


## <a name="configure-a-server-level-firewall-rule"></a>Konfigurace pravidla brány firewall na úrovni serveru

Pomocí příkazu [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_create) vytvořte pravidlo brány firewall na úrovni serveru Azure PostgreSQL. Pravidlo brány firewall na úrovni serveru umožňuje externí aplikaci, jako je třeba [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) nebo [PgAdmin](https://www.pgadmin.org/), aby se k vašemu serveru připojila prostřednictvím brány firewall služby Azure PostgreSQL. 

Abyste se mohli připojit z vaší sítě, můžete nastavit pravidlo brány firewall, které pokrývá rozsah IP adres. Následující příklad vytvoří pomocí příkazu [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#az_postgres_server_firewall_rule_create) pravidlo brány firewall `AllowMyIP` pro jednu IP adresu.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mydemoserver --name AllowMyIP --start-ip-address 192.168.0.1 --end-ip-address 192.168.0.1
```

> [!NOTE]
> Server Azure PostgreSQL komunikuje přes port 5432. Pokud se připojujete z podnikové sítě, nemusí být odchozí provoz přes port 5432 bránou firewall vaší sítě povolený. Pořádejte IT oddělení, aby otevřeli port 5432 pro připojení k vašemu serveru Azure PostgreSQL.

## <a name="get-the-connection-information"></a>Získání informací o připojení

Pokud se chcete připojit k serveru, budete muset zadat informace o hostiteli a přihlašovací údaje pro přístup.
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mydemoserver
```

Výsledek je ve formátu JSON. Poznamenejte si položky **administratorLogin** a **fullyQualifiedDomainName**.
```json
{
  "administratorLogin": "myadmin",
  "earliestRestoreDate": null,
  "fullyQualifiedDomainName": "mydemoserver.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/mydemoserver",
  "location": "westus",
  "name": "mydemoserver",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 2,
    "family": "Gen4",
    "name": "GP_Gen4_2",
    "size": null,
    "tier": "GeneralPurpose"
  },
  "sslEnforcement": "Enabled",
  "storageProfile": {
    "backupRetentionDays": 7,
    "geoRedundantBackup": "Disabled",
    "storageMb": 5120
  },
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "9.6"
}
```

## <a name="connect-to-postgresql-database-using-psql"></a>Připojení k databázi PostgreSQL pomocí nástroje psql

Pokud má klientský počítač nainstalovaný systém PostgreSQL, můžete se připojit k serveru Azure PostgreSQL pomocí místní instance nástroje [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html). Teď se pomocí nástroje příkazového řádku psql připojíme k serveru Azure PostgreSQL.

1. Spusťte následující příkaz psql pro připojení k serveru Azure Database for PostgreSQL:
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  Třeba tento příkaz provádí pomocí přihlašovacích údajů pro přístup připojení k výchozí databázi s názvem **postgres** na serveru PostgreSQL **mydemoserver.postgres.database.azure.com**. Zadejte heslo `<server_admin_password>`, které jste zvolili při zobrazení výzvy k zadání hesla.
  
  ```azurecli-interactive
psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=myadmin@mydemoserver --dbname=postgres
```

2.  Po připojení k serveru vytvořte na příkazovém řádku prázdnou databázi.
```sql
CREATE DATABASE mypgsqldb;
```

3.  Na příkazovém řádku spusťte následující příkaz, který přepne připojení na nově vytvořenou databázi **mypgsqldb**:
```sql
\c mypgsqldb
```

## <a name="connect-to-the-postgresql-server-using-pgadmin"></a>Připojení k serveru PostgreSQL pomocí nástroje pgAdmin

pgAdmin je opensourcový nástroj používaný se systémem PostgreSQL. Nástroj pgAdmin můžete nainstalovat z [webu pgAdmin](http://www.pgadmin.org/). Verze nástroje pgAdmin, kterou používáte, se může lišit od verze používané v tomto rychlém startu. Pokud potřebujete další pokyny, přečtěte si dokumentaci nástroje pgAdmin.

1. Na klientském počítači otevřete aplikaci pgAdmin.

2. Na panelu nástrojů přejděte na **Objekt**, najeďte myší na **Vytvořit** a vyberte **Server**.

3. V dialogovém okně **Vytvořit – server** na kartě **Obecné** zadejte jedinečný popisný název serveru, jako například **mydemoserver**.

   ![Karta Obecné](./media/quickstart-create-server-database-azure-cli/9-pgadmin-create-server.png)

4. V dialogovém okně **Vytvořit – server** na kartě **Připojení** vyplňte tabulku nastavení.

   ![Karta Připojení](./media/quickstart-create-server-database-azure-cli/10-pgadmin-create-server.png)

    Parametr pgAdmin |Hodnota|Popis
    ---|---|---
    Název nebo adresa hostitele | Název serveru | Hodnota názvu serveru, kterou jste použili dříve při vytváření serveru Azure Database for PostgreSQL. Ukázkový server v příkladu je **mydemoserver.postgres.database.azure.com**. Použijte plně kvalifikovaný název domény (**\*.postgres.database.azure.com**), jak je znázorněno v příkladu. Pokud si název vašeho serveru nepamatujete, získejte informace o připojení pomocí postupu v předchozí části. 
    Port | 5432 | Port, který se použije pro připojení k serveru Azure Database for PostgreSQL. 
    Databáze údržby | *postgres* | Výchozí systémem vygenerovaný název databáze.
    Uživatelské jméno | Přihlašovací jméno správce serveru | Přihlašovací uživatelské jméno správce serveru, které jste zadali dříve při vytváření serveru Azure Database for PostgreSQL. Pokud si uživatelské jméno nepamatujete, získejte informace o připojení pomocí postupu v předchozí části. Formát je *username@servername*.
    Heslo | Vaše heslo správce | Heslo, které jste si zvolili při vytváření serveru dříve v tomto rychlém startu.
    Role | Ponechte prázdné | V tuto chvíli není nutné zadávat název role. Ponechte toto pole prázdné.
    Režim SSL | *Vyžadovat* | Na kartě SSL nástroje pgAdmin můžete nastavit režim SSL. Ve výchozím nastavení jsou všechny servery Azure Database for PostgreSQL vytvořené se zapnutým vynucováním SSL. Pokud chcete vynucování SSL vypnout, přečtěte si téma popisující [vynucování SSL](./concepts-ssl-connection-security.md).
    
5. Vyberte **Uložit**.

6. V levém podokně **Prohlížeč** rozbalte uzel **Servery**. Vyberte server, například **mydemoserver**. Kliknutím se k němu připojte.

7. Rozbalte uzel serveru a pod ním pak rozbalte **Databáze**. V seznamu by měla být vaše stávající databáze *postgres* a případné další databáze, které jste vytvořili. Na jednom serveru se službou Azure Database for PostgreSQL můžete vytvořit víc databází.

8. Klikněte pravým tlačítkem na **Databáze**, zvolte nabídku **Vytvořit** a pak vyberte **Databáze**.

9. Do pole **Databáze** zadejte libovolný název databáze, například **mypgsqldb2**.

10. Ze seznamu vyberte **Vlastníka** databáze. Zvolte přihlašovací jméno správce serveru, jako je **myadmin** v příkladu.

   ![Vytvoření databáze v nástroji pgAdmin](./media/quickstart-create-server-database-azure-cli/11-pgadmin-database.png)

11. Vyberte **Uložit** a vytvořte novou prázdnou databázi.

12. V podokně **Prohlížeč** se vytvořená databáze zobrazí v seznamu databází pod názvem vašeho serveru.




## <a name="clean-up-resources"></a>Vyčištění prostředků

Všechny prostředky, které jste v rychlém startu vytvořili, můžete vyčistit odstraněním [skupiny prostředků Azure](../azure-resource-manager/resource-group-overview.md).

> [!TIP]
> Další rychlé starty v této kolekci jsou postavené na tomto rychlém startu. Pokud chcete pokračovat v práci s dalšími rychlými starty, neprovádějte čištění prostředků vytvořených v rámci tohoto rychlého startu. Pokud pokračovat nechcete, pomocí následujících kroků odstraňte všechny prostředky, které jste v tomto rychlém startu vytvořili na webu Azure Portal.

```azurecli-interactive
az group delete --name myresourcegroup
```

Pokud chcete odstranit jenom nově vytvořený server, můžete spustit příkaz [az postgres server delete](/cli/azure/postgres/server#az_postgres_server_delete).
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="next-steps"></a>Další kroky
> [!div class="nextstepaction"]
> [Migrace vaší databáze pomocí exportu a importu](./howto-migrate-using-export-and-import.md)

