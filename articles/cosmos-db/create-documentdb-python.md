---
title: "Databáze Azure Cosmos: Sestavení aplikace v jazyce Python s rozhraním API DocumentDB | Dokumentace Microsoftu"
description: "Představuje ukázku kódu Python, který můžete použít k připojení a dotazování do rozhraní API DocumentDB služby Azure Cosmos DB."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 51c11be2-af6d-425f-a86a-39cbfe61da29
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 05/13/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: 08d467ea27484e7d1d07d6c21b2e04b6525fbcd8
ms.contentlocale: cs-cz
ms.lasthandoff: 06/01/2017


---
# Databáze Azure Cosmos: Sestavení aplikace s rozhraním API DocumentDB pomocí kódu Python a webu Azure Portal
<a id="azure-cosmos-db-build-a-documentdb-api-app-with-python-and-the-azure-portal" class="xliff"></a>

Databáze Azure Cosmos je databázová služba Microsoftu s více modely použitelná v celosvětovém měřítku. Můžete snadno vytvořit a dotazovat databáze dotazů, klíčů/hodnot a grafů, které tak můžou využívat výhody použitelnosti v celosvětovém měřítku a možností horizontálního škálování v jádru Azure Cosmos DB. 

Tento rychlý start popisuje způsob vytvoření účtu databáze Azure Cosmos, databáze dokumentů a kolekce pomocí webu Azure Portal. Potom sestavíte a spustíte aplikaci konzoly založenou na [rozhraní API Python DocumentDB](documentdb-sdk-python.md).

## Požadavky
<a id="prerequisites" class="xliff"></a>

* Než budete moct tuto ukázku spustit, je potřeba splnit následující požadavky:
    * [Visual Studio 2015](http://www.visualstudio.com/) nebo vyšší.
    * Python Tools for Visual Studio z [GitHubu](http://microsoft.github.io/PTVS/). V tomto kurzu se používá Python Tools for VS 2015.
    * Python 2.7 z webu [python.org](https://www.python.org/downloads/release/python-2712/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## Vytvoření účtu databáze
<a id="create-a-database-account" class="xliff"></a>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## Přidání kolekce
<a id="add-a-collection" class="xliff"></a>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## Klonování ukázkové aplikace
<a id="clone-the-sample-application" class="xliff"></a>

Teď naklonujeme aplikaci s rozhraním API DocumentDB z GitHubu, nastavíme připojovací řetězec a spustíme ji. Uvidíte, jak snadno se pracuje s daty prostřednictvím kódu programu. 

1. Otevřete okno terminálu Git, jako je třeba Git Bash, a pomocí `cd` přejděte do pracovního adresáře.  

2. Ukázkové úložiště naklonujete spuštěním následujícího příkazu. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-python-getting-started.git
    ```  
## Kontrola kódu
<a id="review-the-code" class="xliff"></a>

Ještě jednou se stručně podívejme na to, co se v aplikaci děje. Otevřete soubor DocumentDBGetStarted.py a zjistíte, že tyto řádky kódu vytvářejí prostředky Azure Cosmos DB. 


* Inicializuje se DocumentClient.

    ```python
    # Initialize the Python DocumentDB client
    client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})
    ```

* Vytvoří se nová databáze.

    ```python
    # Create a database
    db = client.CreateDatabase({ 'id': config['DOCUMENTDB_DATABASE'] })
    ```

* Vytvoří se nová kolekce.

    ```python
    # Create collection options
    options = {
        'offerEnableRUPerMinuteThroughput': True,
        'offerVersion': "V2",
        'offerThroughput': 400
    }

    # Create a collection
    collection = client.CreateCollection(db['_self'], { 'id': config['DOCUMENTDB_COLLECTION'] }, options)
    ```

* Vytvoří se některé dokumenty.

    ```python
    # Create some documents
    document1 = client.CreateDocument(collection['_self'],
        { 
            'id': 'server1',
            'Web Site': 0,
            'Cloud Service': 0,
            'Virtual Machine': 0,
            'name': 'some' 
        })
    ```

* Provede se dotaz v jazyce SQL.

    ```python
    # Query them in SQL
    query = { 'query': 'SELECT * FROM server s' }    
            
    options = {} 
    options['enableCrossPartitionQuery'] = True
    options['maxItemCount'] = 2

    result_iterable = client.QueryDocuments(collection['_self'], query, options)
    results = list(result_iterable);

    print(results)
    ```

## Aktualizace připojovacího řetězce
<a id="update-your-connection-string" class="xliff"></a>

Teď se vraťte zpátky na portál Azure Portal, kde najdete informace o připojovacím řetězci, a zkopírujte je do aplikace.

1. Na webu [Azure Portal](http://portal.azure.com/) klikněte v účtu databáze Azure Cosmos v levém navigačním panelu na možnost **Klíče** a potom klikněte na **Klíče pro čtení i zápis**. V dalším kroku zkopírujete pomocí tlačítek kopírování na pravé straně obrazovky identifikátor URI a primární klíč do souboru `DocumentDBGetStarted.py`.

    ![Zobrazení a zkopírování přístupového klíče na webu Azure Portal v okně Klíče](./media/create-documentdb-dotnet/keys.png)

2. Otevřete soubor `DocumentDBGetStarted.py`. 

3. Z portálu zkopírujte hodnotu identifikátoru URI (pomocí tlačítka kopírování) a nastavte ji jako hodnotu klíče koncového bodu v souboru `DocumentDBGetStarted.py`. 

    `config.ENDPOINT : "https://FILLME.documents.azure.com"`

4. Potom z portálu zkopírujte hodnotu PRIMÁRNÍHO KLÍČE a nastavte ji jako hodnotu `config.MASTERKEY` v souboru `DocumentDBGetStarted.py`. Teď jste aktualizovali aplikaci a zadali do ní všechny informace potřebné ke komunikaci s databází Azure Cosmos. 

    `config.MASTERKEY : "FILLME"`
    
## Spuštění aplikace
<a id="run-the-app" class="xliff"></a>
1. V sadě Visual Studio klikněte pravým tlačítkem na projekt v **Průzkumníku řešení**,vyberte aktuální prostředí Python a potom klikněte pravým tlačítkem myši.

2. Vyberte Možnost Instalovat balíček Pythonu a potom zadejte **pydocumentdb**.

3. Stisknutím klávesy F5 spusťte aplikaci. Aplikace se zobrazí v prohlížeči. 

Teď se můžete vrátit do Průzkumníku dat a zobrazit dotaz nebo provést úpravy a pracovat s těmito novými daty. 

## Ověření podmínek SLA na portálu Azure Portal
<a id="review-slas-in-the-azure-portal" class="xliff"></a>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## Vyčištění prostředků
<a id="clean-up-resources" class="xliff"></a>

Pokud nebudete tuto aplikace nadále používat, odstraňte na základě následujícího postupu z portálu Azure Portal všechny prostředky vytvořené podle tohoto rychlého startu:

1. V nabídce vlevo na portálu Azure Portal klikněte na **Skupiny prostředků** a pak klikněte na název vytvořeného prostředku. 
2. Na stránce skupiny prostředků klikněte na **Odstranit**, do textového pole zadejte prostředek, který chcete odstranit, a pak klikněte na **Odstranit**.

## Další kroky
<a id="next-steps" class="xliff"></a>

V tomto rychlém startu jsme se seznámili s postupem vytvoření účtu databáze Azure Cosmos, vytvoření kolekce pomocí Průzkumníka dat a spuštění aplikace. Teď můžete do účtu databáze Cosmos importovat další data. 

> [!div class="nextstepaction"]
> [Import dat do databáze Azure Cosmos pro rozhraní API DocumentDB](import-data.md)



