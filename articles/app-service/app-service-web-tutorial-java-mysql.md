---
title: "Vytvoření webové aplikace Java využívající databázi MySQL v Azure"
description: "Zjistěte, jak získat aplikaci Java, která se připojuje k databázové službě Azure MySQL fungující ve službě Azure App Service."
services: app-service\web
documentationcenter: Java
author: bbenz
manager: jeffsand
editor: jasonwhowell
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: tutorial
ms.date: 05/22/2017
ms.author: bbenz
ms.custom: mvc
ms.openlocfilehash: 2df08c8e3dbadbfc1a9d2cfb3adcda4f5bae2851
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="build-a-java-and-mysql-web-app-in-azure"></a>Vytvoření webové aplikace Java využívající databázi MySQL v Azure

> [!NOTE]
> Tento článek nasadí aplikaci do služby App Service ve Windows. Pokud chcete nasadit službu App Service v _Linuxu_, přečtěte si téma [Nasazení kontejnerizované aplikace Spring Boot do Azure](/java/azure/spring-framework/deploy-containerized-spring-boot-java-app-with-maven-plugin).
>

Tento kurz předvádí postup při vytváření webové aplikace Java v Azure a jejím připojení k databázi MySQL. Až budete hotovi, budete mít aplikaci [Spring Boot](https://projects.spring.io/spring-boot/) ukládající data ve službě [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/overview) spuštěné v [Azure App Service Web Apps](app-service-web-overview.md).

![Aplikace Java spuštěná ve službě Azure App Service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvořit databázi MySQL v Azure
> * Připojit k databázi ukázkovou aplikaci
> * Nasadit aplikaci do Azure
> * Aktualizace a opětovné nasazení aplikace
> * Streamovat diagnostické protokoly z Azure
> * Monitorovat aplikaci na webu Azure Portal

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Požadavky

1. [Stažení a instalace Gitu](https://git-scm.com/)
1. [Stažení a instalace sady Java 7 JDK nebo novější](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
1. [Stažení, instalace a spuštění MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

## <a name="prepare-local-mysql"></a>Příprava místního MySQL 

V tomto kroku vytvoříte na místním serveru MySQL databázi pro použití při místním testování aplikace na vašem počítači.

### <a name="connect-to-mysql-server"></a>Připojení k serveru MySQL

V okně terminálu se připojte k místnímu serveru MySQL. Toto okno terminálu můžete používat ke spuštění všech příkazů v tomto kurzu.

```bash
mysql -u root -p
```

Pokud se zobrazí výzva k zadání hesla, zadejte heslo k účtu `root`. Pokud si heslo ke kořenovému účtu nepamatujete, projděte si článek [MySQL: Resetování kořenového hesla](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Pokud se váš příkaz úspěšně provede, znamená to, že je váš server MySQL už spuštěný. Pokud ne, provedením [kroků po instalaci MySQL](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html) zkontrolujte, jestli je místní server MySQL spuštěný.

### <a name="create-a-database"></a>Vytvoření databáze 

Na příkazovém řádku `mysql` vytvořte databázi a tabulku pro položky úkolů.

```sql
CREATE DATABASE tododb;
```

Ukončete připojení k serveru zadáním příkazu `quit`.

```sql
quit
```

## <a name="create-and-run-the-sample-app"></a>Vytvoření a spuštění ukázkové aplikace 

V tomto kroku naklonujete ukázkovou aplikaci Spring Boot, nakonfigurujete ji tak, aby používala místní databázi MySQL, a spustíte ji na svém počítači. 

### <a name="clone-the-sample"></a>Vytvoření klonu ukázky

V okně terminálu přejděte do pracovního adresáře a naklonujte ukázkové úložiště. 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-the-app-to-use-the-mysql-database"></a>Konfigurace aplikace pro používání databáze MySQL

V souboru *spring-boot-mysql-todo/src/main/resources/application.properties* aktualizujte hodnotu `spring.datasource.password` na stejné kořenové heslo, jaké jste použili k otevření příkazového řádku MySQL:

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-the-sample"></a>Sestavení a spuštění ukázky

Sestavte a spusťte ukázku pomocí obálky Maven, která je součástí úložiště:

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

Otevřete prohlížeč na adrese `http://localhost:8080` a podívejte se na ukázku v akci. S tím, jak budete přidávat úkoly do seznamu, můžete pomocí následujících příkazů na příkazovém řádku MySQL zobrazit data uložená v MySQL.

```SQL
use testdb;
select * from todo_item;
```

Zastavte aplikaci stisknutím `Ctrl`+`C` v terminálu. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-an-azure-mysql-database"></a>Vytvoření databáze MySQL Azure

V tomto kroku pomocí [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) vytvoříte instanci služby [Azure Database for MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md). V pozdější části tohoto kurzu nakonfigurujete ukázkovou aplikaci tak, aby používala tuto databázi.

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte [skupinu prostředků](../azure-resource-manager/resource-group-overview.md) pomocí příkazu [`az group create`](/cli/azure/group#az_group_create). Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují související prostředky, jako například webové aplikace, databáze a účty úložiště. 

Následující příklad vytvoří skupinu prostředků v oblasti Severní Evropa:

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

K zobrazení možných hodnot, které se dají použít pro `--location`, použijte příkaz [`az appservice list-locations`](/cli/azure/appservice#list-locations).

### <a name="create-a-mysql-server"></a>Vytvoření serveru MySQL

Ve službě Cloud Shell pomocí příkazu [`az mysql server create`](/cli/azure/mysql/server#az_mysql_server_create) vytvořte v Azure Database for MySQL (Preview) server. Zástupný text `<mysql_server_name>` nahraďte vlastním jedinečným názvem serveru MySQL. Tento název je součástí názvu hostitele serveru MySQL, `<mysql_server_name>.mysql.database.azure.com`, proto musí být globálně jedinečný. Nahraďte vlastními hodnotami také položky `<admin_user>` a `<admin_password>`.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user <admin_user> --admin-password <admin_password>
```

Po vytvoření serveru MySQL se v Azure CLI zobrazí podobné informace jako v následujícím příkladu:

```json
{
  "administratorLogin": "admin_user",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mysql_server_name.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/mysql_server_name",
  "location": "northeurope",
  "name": "mysql_server_name",
  "resourceGroup": "mysqlJavaResourceGroup",
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-server-firewall"></a>Konfigurace brány firewall serveru

Ve službě Cloud Shell pomocí příkazu [`az mysql server firewall-rule create`](/cli/azure/mysql/server/firewall-rule#az_mysql_server_firewall_rule_create) vytvořte pro svůj server MySQL pravidlo brány firewall umožňující klientská připojení. 

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure Database for MySQL (Preview) v současné době neumožňuje automaticky připojení ze služeb Azure. Jelikož se IP adresy v Azure přidělují dynamicky, je lepší prozatím povolit všechny IP adresy. V průběhu verze Preview služby budou zapnuté lepší metody pro zabezpečení databáze.

## <a name="configure-the-azure-mysql-database"></a>Konfigurace databáze MySQL Azure

V místním okně terminálu se připojte k serveru MySQL v Azure. U položek `<admin_user>` a `<mysql_server_name>` použijte hodnoty, které jste zadali předtím.

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a>Vytvoření databáze 

Na příkazovém řádku `mysql` vytvořte databázi a tabulku pro položky úkolů.

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a>Vytvoření uživatele s oprávněními

Vytvořte uživatele databáze a přidělte mu všechna oprávnění k databázi `tododb`. Zástupné texty `<Javaapp_user>` a `<Javaapp_password>` nahraďte vlastním jedinečným názvem aplikace.

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* TO '<Javaapp_user>';
```

Ukončete připojení k serveru zadáním příkazu `quit`.

```sql
quit
```

## <a name="deploy-the-sample-to-azure-app-service"></a>Nasazení ukázky do služby Azure App Service

Pomocí příkazu [`az appservice plan create`](/cli/azure/appservice/plan#az_appservice_plan_create) rozhraní příkazového řádku vytvořte plán služby App Service s cenovou úrovní **FREE**. Plán služby App Service definuje fyzické prostředky používané k hostování vašich aplikací. Všechny aplikace přiřazené k plánu služby App Service sdílí tyto prostředky, a tím umožňují snížení nákladů při hostování více aplikací. 

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Jakmile bude plán připravený, v Azure CLI se zobrazí podobný výstup jako v následujícím příkladu:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a>Vytvoření webové aplikace Azure

Ve službě Cloud Shell pomocí příkazu [`az webapp create`](/cli/azure/appservice/web#az_appservice_web_create) rozhraní příkazového řádku vytvořte definici webové aplikace v plánu služby App Service `myAppServicePlan`. Definice webové aplikace poskytuje adresu URL pro přístup k aplikaci a konfiguruje několik možností pro nasazení kódu do Azure. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Zástupný text `<app_name>` nahraďte vlastním jedinečným názvem aplikace. Tento jedinečný název je součástí výchozího názvu domény pro příslušnou webovou aplikaci, proto musí být mezi všemi aplikacemi v Azure jedinečný. Na webovou aplikaci můžete namapovat vlastní název domény, než ji zpřístupníte uživatelům.

Jakmile bude definice webové aplikace připravená, v Azure CLI se zobrazí podobné informace jako v následujícím příkladu: 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
   ...
  < Output has been truncated for readability >
}
```

### <a name="configure-java"></a>Konfigurace Javy 

Ve službě Cloud Shell pomocí příkazu [`az webapp config set`](/cli/azure/webapp/config#az_webapp_config_set) nastavte konfiguraci modulu runtime Java podle potřeb vaší aplikace.

Následující příkaz nakonfiguruje spouštění webové aplikace na nejnovější sadě Java 8 JDK a [Apache Tomcat](http://tomcat.apache.org/) 8.0.

```azurecli-interactive
az webapp config set --name <app_name> --resource-group myResourceGroup --java-version 1.8 --java-container Tomcat --java-container-version 8.0
```

### <a name="configure-the-app-to-use-the-azure-sql-database"></a>Konfigurace aplikace pro používání databáze SQL Azure

Před spuštěním ukázkové aplikace nastavte v nastavení webové aplikace používání databáze MySQL Azure, kterou jste vytvořili v Azure. Tyto vlastnosti se zpřístupňují webové aplikaci jako proměnné prostředí a přepisují hodnoty nastavené v souboru application.properties uvnitř zabalené webové aplikace. 

Ve službě Cloud Shell nastavte pomocí příkazu [`az webapp config appsettings`](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) v CLI nastavení aplikace:

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_URL="jdbc:mysql://<mysql_server_name>.mysql.database.azure.com:3306/tododb?verifyServerCertificate=true&useSSL=true&requireSSL=false" --resource-group myResourceGroup --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_USERNAME=Javaapp_user@mysql_server_name --resource-group myResourceGroup --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_PASSWORD=Javaapp_password --resource-group myResourceGroup --name <app_name>
```

### <a name="get-ftp-deployment-credentials"></a>Získání přihlašovacích údajů pro nasazení přes protokol FTP 
K nasazení aplikace do služby Azure App Service můžete použít různé způsoby, včetně protokolu FTP, místního Gitu, GitHubu, Visual Studio Team Services nebo BitBucketu. Pro účely tohoto příkladu pomocí protokolu FTP nasaďte do služby Azure App Service soubor .WAR, který jste sestavili dříve na svém místním počítači.

Pokud chcete určit, jaké přihlašovací údaje máte předat do webové aplikace v příkazu FTP, použijte ve službě Cloud Shell příkaz [`az appservice web deployment list-publishing-profiles`](https://docs.microsoft.com/cli/azure/appservice/web/deployment#az_appservice_web_deployment_list_publishing_profiles): 

```azurecli-interactive
az webapp deployment list-publishing-profiles --name <app_name> --resource-group myResourceGroup --query "[?publishMethod=='FTP'].{URL:publishUrl, Username:userName,Password:userPWD}" --output json
```

```JSON
[
  {
    "Password": "aBcDeFgHiJkLmNoPqRsTuVwXyZ",
    "URL": "ftp://waws-prod-blu-069.ftp.azurewebsites.windows.net/site/wwwroot",
    "Username": "app_name\\$app_name"
  }
]
```

### <a name="upload-the-app-using-ftp"></a>Nahrání aplikace pomocí protokolu FTP

Pomocí oblíbeného nástroje FTP nasaďte soubor .WAR do složky */site/wwwroot/webapps* na adrese serveru, kterou jste získali z pole `URL` v předchozím příkazu. Odeberte existující výchozí adresář aplikace (ROOT) a nahraďte existující soubor ROOT.war souborem .WAR sestaveným v dřívější části tohoto kurzu.

```bash
ftp waws-prod-blu-069.ftp.azurewebsites.windows.net
Connected to waws-prod-blu-069.drip.azurewebsites.windows.net.
220 Microsoft FTP Service
Name (waws-prod-blu-069.ftp.azurewebsites.windows.net:raisa): app_name\$app_name
331 Password required
Password:
cd /site/wwwroot/webapps
mdelete -i ROOT/*
rmdir ROOT/
put target/TodoDemo-0.0.1-SNAPSHOT.war ROOT.war
```

### <a name="test-the-web-app"></a>Test webové aplikace

Přejděte na adresu `http://<app_name>.azurewebsites.net/` a přidejte do seznamu několik úkolů. 

![Aplikace Java spuštěná ve službě Azure App Service](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

**Blahopřejeme!** Teď máte ve službě Azure App Service spuštěnou aplikaci Java řízenou daty.

## <a name="update-the-app-and-redeploy"></a>Aktualizace a opětovné nasazení aplikace

Aktualizujte aplikaci a vložte do seznamu úkolů další sloupec pro den vytvoření položky. Spring Boot se v případě změn datového modelu stará o aktualizaci schématu databáze za vás, a to beze změn existujících záznamů v databázi.

1. V místním systému otevřete soubor *src/main/java/com/example/fabrikam/TodoItem.java* a přidejte do třídy následující příkazy import:   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. Přidejte do souboru *src/main/java/com/example/fabrikam/TodoItem.java* vlastnost `timeCreated` typu `String`, která se při vytvoření objektu inicializuje s časovým razítkem. Když upravujete tento soubor, přidejte pro novou vlastnost `timeCreated` metody getter a setter.

    ```java
    private String name;
    private boolean complete;
    private String timeCreated;
    ...

    public TodoItem(String category, String name) {
       this.category = category;
       this.name = name;
       this.complete = false;
       this.timeCreated = new SimpleDateFormat("MMMM dd, YYYY").format(Calendar.getInstance().getTime());
    }
    ...
    public void setTimeCreated(String timeCreated) {
       this.timeCreated = timeCreated;
    }

    public String getTimeCreated() {
        return timeCreated;
    }
    ```

3. Aktualizujte soubor *src/main/java/com/example/fabrikam/TodoDemoController.java* a do metody `updateTodo` přidejte řádek nastavující časové razítko:

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. Přidejte podporu nového pole do šablony `Thymeleaf`. Aktualizujte soubor *src/main/resources/templates/index.html* přidáním nového záhlaví tabulky pro časové razítko a nového pole pro zobrazení hodnoty časového razítka na každém řádku dat tabulky.

    ```html
    <th>Name</th>
    <th>Category</th>
    <th>Time Created</th>
    <th>Complete</th>
    ...
    <td th:text="${item.category}">item_category</td><input type="hidden" th:field="*{todoList[__${i.index}__].category}"/>
    <td th:text="${item.timeCreated}">item_time_created</td><input type="hidden" th:field="*{todoList[__${i.index}__].timeCreated}"/>
    <td><input type="checkbox" th:checked="${item.complete} == true" th:field="*{todoList[__${i.index}__].complete}"/></td>
    ```

5. Znovu sestavte aplikaci:

    ```bash
    mvnw clean package 
    ```

6. Stejným způsobem jako předtím nahrajte aktualizovaný soubor .WAR pomocí protokolu FTP – odeberte adresář *site/wwwroot/webapps/ROOT* a soubor *ROOT.war* a pak nahrajte aktualizovaný soubor .WAR jako soubor ROOT.war. 

Po aktualizaci aplikace se teď zobrazí sloupec **Time Created** (Čas vytvoření). Když přidáte nový úkol, aplikace vyplní časové razítko automaticky. Vaše stávající úkoly zůstanou beze změny a budou s aplikací fungovat i přesto, že se změnil základní datový model. 

![Aktualizovaná aplikace Java s novým sloupcem](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a>Streamování diagnostických protokolů 

Zatímco je vaše aplikace Java spuštěná ve službě Azure App Service, můžete směrovat protokoly konzoly přímo do svého terminálu. Tímto způsobem můžete získat stejné diagnostické zprávy, které vám pomůžou ladit chyby aplikace.

Ke spuštění streamování protokolů použijte příkaz [`az webapp log tail`](/cli/azure/webapp/log?view=azure-cli-latest#az_webapp_log_tail) ve službě Cloud Shell.

```azurecli-interactive 
az webapp log tail --name <app_name> --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a>Správa webové aplikace Azure

Přejděte na web [Azure Portal](https://portal.azure.com) k webové aplikaci, kterou jste vytvořili.

V levé nabídce klikněte na **App Service** a pak klikněte na název vaší webové aplikace Azure.

![Navigace portálem k webové aplikaci Azure](./media/app-service-web-tutorial-java-mysql/access-portal.png)

Ve výchozím nastavení se na stránce vaší webové aplikace zobrazí stránka **Přehled**. Tato stránka poskytuje přehled, jak si vaše aplikace stojí. Tady můžete také provádět úlohy správy, jako je zastavení, spuštění, restartování a odstranění. Karty na levé straně stránky obsahují různé stránky konfigurace, které můžete otevřít.

![Stránka služby App Service na webu Azure Portal](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

Tyto karty na stránce zobrazují mnoho skvělých funkcí, které můžete do své webové aplikace přidat. Následující seznam obsahuje jen několik možností:
* Mapování vlastního názvu DNS
* Vazba vlastního certifikátu SSL
* Konfigurace průběžného nasazování
* Vertikální i horizontální navýšení kapacity
* Přidání ověřování uživatelů

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud tyto prostředky nepotřebujete pro další kurz (viz [Další kroky](#next)), můžete je odstranit spuštěním následujícího příkazu ve službě Cloud Shell: 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## <a name="next-steps"></a>Další kroky

> [!div class="checklist"]
> * Vytvořit databázi MySQL v Azure
> * Připojit k MySQL ukázkovou aplikaci Java
> * Nasadit aplikaci do Azure
> * Aktualizace a opětovné nasazení aplikace
> * Streamovat diagnostické protokoly z Azure
> * Spravovat aplikaci na webu Azure Portal

V dalším kurzu se dozvíte, jak namapovat na aplikaci vlastní název DNS.

> [!div class="nextstepaction"] 
> [Mapování existujícího vlastního názvu DNS na Azure Web Apps](app-service-web-tutorial-custom-domain.md)
