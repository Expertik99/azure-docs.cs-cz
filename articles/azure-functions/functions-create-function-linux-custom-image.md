---
title: Vytvoření funkce v Linuxu pomocí vlastní image (verze Preview) | Microsoft Docs
description: Naučte se vytvářet funkce služby Azure Functions běžící na vlastní imagi Linuxu.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 11/15/2017
ms.topic: tutorial
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: fab67b503d060c8c01b5a3692c8a07b24c425c78
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/02/2018
ms.locfileid: "39437402"
---
# <a name="create-a-function-on-linux-using-a-custom-image-preview"></a>Vytvoření funkce v Linuxu pomocí vlastní image (verze Preview)

Služba Azure Functions umožňuje hostovat funkce v Linuxu ve vašem vlastním kontejneru. Můžete také [hostovat ve výchozím kontejneru služby Azure App Service](functions-create-first-azure-function-azure-cli-linux.md). Tato funkce je aktuálně ve verzi Preview a vyžaduje [modul runtime Functions 2.0](functions-versions.md), který je také ve verzi Preview.

V tomto kurzu se dozvíte, jak nasadit aplikaci Function App jako vlastní image Dockeru. Tento vzor je užitečný v případě, že potřebujete přizpůsobit vestavěnou image kontejneru služby App Service. Když vaše funkce vyžadují určitou jazykovou verzi nebo konkrétní závislost nebo konfiguraci, kterou vestavěná image neposkytuje, můžete chtít použít vlastní image.

V tomto kurzu se dozvíte, jak pomocí služby Azure Functions vytvořit vlastní image a odeslat ji do Docker Hubu. Tuto image potom použijete jako zdroj nasazení aplikace Function App, která běží na systému Linux. Pomocí Dockeru tuto image sestavíte a odešlete. Pomocí rozhraní Azure CLI vytvoříte aplikaci Function App a nasadíte image z Docker Hubu. 

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Sestavit vlastní image pomocí Dockeru
> * Publikovat vlastní image do registru kontejneru 
> * Vytvořit účet služby Azure Storage 
> * Vytvořit plán služby App Service pro Linux 
> * Nasadit aplikaci Function App z Docker Hubu
> * Přidat do aplikace Function App nastavení aplikace 

Následující kroky se podporují na počítačích se systémem Mac, Windows a Linux.  

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

* [Git](https://git-scm.com/downloads)
* Aktivní [předplatné Azure](https://azure.microsoft.com/pricing/free-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Docker](https://docs.docker.com/install/)
* [Účet Docker Hubu](https://docs.docker.com/docker-id/)

[!INCLUDE [Free trial note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Stažení ukázky

V okně terminálu naklonujte spuštěním následujícího příkazu úložiště ukázkové aplikace do místního počítače a pak přejděte do adresáře, který obsahuje vzorový kód.

```bash
git clone https://github.com/Azure-Samples/functions-linux-custom-image.git --config core.autocrlf=input
cd functions-linux-custom-image
```

## <a name="build-the-image-from-the-docker-file"></a>Sestavení image ze souboru Dockeru

V úložišti Git si prohlédněte soubor _Dockerfile_. Tento soubor popisuje prostředí potřebné pro spuštění aplikace Function App v Linuxu. 

```docker
# Base the image on the built-in Azure Functions Linux image.
FROM microsoft/azure-functions-runtime:2.0.0-jessie
ENV AzureWebJobsScriptRoot=/home/site/wwwroot

# Add files from this repo to the root site folder.
COPY . /home/site/wwwroot 
```
>[!NOTE]
> Při hostování image v privátním registru kontejneru byste měli pomocí proměnných **ENV** v souboru Dockerfile přidat do aplikace Function App nastavení připojení. Jelikož tento kurz nemůže zaručit, že použijete privátní registr, nastavení připojení se v rámci osvědčeného postupu v oblasti zabezpečení [přidávají po nasazení pomocí rozhraní Azure CLI](#configure-the-function-app).   

### <a name="run-the-build-command"></a>Spuštění příkazu k sestavení
Pokud chcete sestavit image Dockeru, spusťte příkaz `docker build` a zadejte název `mydockerimage` a značku `v1.0.0`. Položku `<docker-id>` nahraďte ID vašeho účtu Docker Hubu.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

Příkaz vygeneruje výstup podobný následujícímu:

```bash
Sending build context to Docker daemon  169.5kB
Step 1/3 : FROM microsoft/azure-functions-runtime:v2.0.0-jessie
v2.0.0-jessie: Pulling from microsoft/azure-functions-runtime
b178b12f7913: Pull complete
2d9ce077a781: Pull complete
4775d4ba55c8: Pull complete
Digest: sha256:073f45fc167b3b5c6642ef4b3c99064430d6b17507095...
Status: Downloaded newer image for microsoft/azure-functions-runtime:v2.0.0-jessie
 ---> 217799efa500
Step 2/3 : ENV AzureWebJobsScriptRoot /home/site/wwwroot
 ---> Running in 528fa2077d17
 ---> 7cc6323b8ae0
Removing intermediate container 528fa2077d17
Step 3/3 : COPY . /home/site/wwwroot
 ---> 5bdac9878423
Successfully built 5bdac9878423
Successfully tagged ggailey777/mydockerimage:v1.0.0
```

### <a name="test-the-image-locally"></a>Testování image v místním prostředí
Ověřte si funkčnost sestavené image tak, že image Dockeru spustíte v místní kontejneru. Zadejte příkaz [docker run](https://docs.docker.com/engine/reference/commandline/run/) a předejte mu název a značku image. Nezapomeňte zadat port pomocí argumentu `-p`.

```bash
docker run -p 8080:80 -it <docker-ID>/mydockerimage:v1.0.0
```

Když už v místním kontejneru Dockeru běží vlastní image, ověřte správnou funkčnost aplikace funkcí a kontejneru tak, že přejdete na adresu <http://localhost:8080>.

![Místní testování aplikace Function App](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

Po ověření aplikace Function App v kontejneru můžete aplikaci zastavit. Teď můžete vlastní image odeslat do svého účtu Docker Hubu.

## <a name="push-the-custom-image-to-docker-hub"></a>Odeslání vlastní image do Docker Hubu

Registr je aplikace, která hostuje image a poskytuje image služeb a služby kontejneru. Pokud chcete svou image sdílet, musíte ji odeslat do registru. Docker Hub je registr pro image Dockeru, který umožňuje hostovat vlastní veřejná nebo privátní úložiště. 

Než budete moct odeslat image, musíte se přihlásit k Docker Hubu pomocí příkazu [docker login](https://docs.docker.com/engine/reference/commandline/login/). Položku `<docker-id>` nahraďte názvem svého účtu a po zobrazení výzvy zadejte do konzoly své heslo. Další možnosti hesla do Docker Hubu najdete v [dokumentaci k příkazu docker login](https://docs.docker.com/engine/reference/commandline/login/).

```bash
docker login --username <docker-id> 
```

Zpráva „login succeeded“ (Přihlášení proběhlo úspěšně) potvrzuje, že jste přihlášení. Po přihlášení odešlete image do Docker Hubu pomocí příkazu [docker push](https://docs.docker.com/engine/reference/commandline/push/).

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

Prozkoumejte výstup příkazu a ověřte si, jestli odeslání proběhlo úspěšně.

```bash
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
24d81eb139bf: Pushed
fd9e998161c9: Mounted from microsoft/azure-functions-runtime
e7796c35add2: Mounted from microsoft/azure-functions-runtime
ae9a05b85848: Mounted from microsoft/azure-functions-runtime
45c86e20670d: Mounted from microsoft/azure-functions-runtime
v1.0.0: digest: sha256:be080d80770df71234eb893fbe4d... size: 2422
```
Teď můžete image použít jako zdroj nasazení nové aplikace Function App v Azure. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a používat rozhraní příkazového řádku místně, musíte mít verzi Azure CLI 2.0.21 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Vytvoření plánu služby App Service pro Linux

Plány Consumption v současné době nepodporují hostování služby Functions v Linuxu. Je třeba použít plán služby App Service pro Linux. Další informace o hostování najdete v [porovnání plánů hostování služby Azure Functions](functions-scale.md). 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]


## <a name="create-and-deploy-the-custom-image"></a>Vytvoření a nasazení vlastní image

Aplikace Function App hostuje provádění vašich funkcí. Aplikaci Function App vytvoříte z image z Docker Hubu pomocí příkazu [az functionapp create](/cli/azure/functionapp#az-functionapp-create). 

V následujícím příkazu nahraďte zástupný symbol `<app_name>` jedinečným názvem vaší aplikace funkcí a `<storage_name>` názvem účtu úložiště. Jako výchozí doména DNS pro příslušnou aplikaci Function App se použije `<app_name>`, a proto musí být název mezi všemi aplikacemi v Azure jedinečný. Stejně jako předtím má váš účet Dockeru název `<docker-id>`.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-container-image-name <docker-id>/mydockerimage:v1.0.0 
```
Po vytvoření aplikace Function App se v Azure CLI zobrazí podobné informace jako v následujícím příkladu:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

Parametr _deployment-container-image-name_ určuje image hostovanou v Docker Hubu, která se má použít k vytvoření aplikace Function App. 


## <a name="configure-the-function-app"></a>Konfigurace aplikace Function App

Funkce potřebuje k připojení k výchozímu účtu úložiště připojovací řetězec. Při publikování vlastní image do účtu privátního kontejneru byste měli místo toho určit toto nastavení aplikace jako proměnné prostředí v souboru Dockerfile pomocí [instrukce ENV](https://docs.docker.com/engine/reference/builder/#env) nebo podobně. 

V tomto případě je `<storage_account>` název účtu úložiště, který jste vytvořili. Připojovací řetězec zobrazíte pomocí příkazu [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string). Pomocí příkazu [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) přidejte tato nastavení aplikace do aplikace Function App.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings AzureWebJobsDashboard=$storageConnectionString \
AzureWebJobsStorage=$storageConnectionString
```

Teď můžete svoje funkce běžící na Linuxu v Azure otestovat.

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Sestavit vlastní image pomocí Dockeru
> * Publikovat vlastní image do registru kontejneru 
> * Vytvořit účet služby Azure Storage 
> * Vytvořit plán služby App Service pro Linux 
> * Nasadit aplikaci Function App z Docker Hubu
> * Přidat do aplikace Function App nastavení aplikace

Zjistěte, jak povolit funkci průběžné integrace, která je integrovaná v základní platformě App Service. Aplikaci funkcí můžete nakonfigurovat tak, aby se při aktualizaci image v Centru Dockeru znovu nasadil kontejner.

> [!div class="nextstepaction"] 
> [Průběžné nasazování pomocí služby Web App for Containers](../app-service/containers/app-service-linux-ci-cd.md)
