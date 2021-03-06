---
title: Příklady Node.js pro Azure Cosmos DB | Microsoft Docs
description: Na GitHubu najdete příklady v Node.js pro běžné úlohy ve službě Azure Cosmos DB, včetně operací CRUD.
keywords: příklady node.js
services: cosmos-db
author: deborahc
manager: kfile
editor: monicar
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: sample
ms.date: 07/31/18
ms.author: deborahc
ms.openlocfilehash: 28ea4b45c68336c78dc17b5c6753147e063139d6
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/07/2018
ms.locfileid: "39609068"
---
# <a name="azure-cosmos-db-nodejs-examples"></a>Příklady v Node.js pro Azure Cosmos DB
> [!div class="op_single_selector"]
> * [Příklady v .NET](sql-api-dotnet-samples.md)
> * [Příklady v Javě](sql-api-java-samples.md)
> * [Příklady v asynchronní Javě](sql-api-async-java-samples.md)
> * [Příklady v Node.js](sql-api-nodejs-samples.md)
> * [Příklady v Node.js – verze 2.0 Preview](sql-api-nodejs-samples-preview.md)
> * [Příklady v Pythonu](sql-api-python-samples.md)
> * [Galerie ukázkového kódu Azure](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

Ukázková řešení, která provádí operace CRUD a další běžné operace s prostředky služby Azure Cosmos DB, jsou součástí úložiště [azure-cosmos-js](https://github.com/Azure/azure-cosmos-js/tree/master/samples) na GitHubu. Tento článek obsahuje:

* Odkazy na úlohy v jednotlivých ukázkových souborech projektů v Node.js.
* Odkazy na související referenční obsah rozhraní API

**Požadavky**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- Můžete si [aktivovat výhody pro předplatitele sady Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Za své předplatné sady Visual Studio každý měsíc získáváte kredity, které můžete použít k placení za služby Azure.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Potřebujete také sadu [JavaScript SDK](sql-api-sdk-node.md).
   
   > [!NOTE]
   > Každá ukázka je samostatná, sama se nastaví a po ukončení příkladu vyčistí svoje prostředky. Ukázky provedou několik volání metody [Containers.create](https://docs.microsoft.com/javascript/api/%40azure/cosmos/containers?view=azure-node-latest#create). Pokaždé, když k tomu dojde, se bude pro vaše předplatné účtovat 1 hodina využití pro úroveň výkonu vytvářeného kontejneru.
   > 
   > 

## <a name="database-examples"></a>Příklady pro databáze
Soubor [app.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/DatabaseManagement/app.js) projektu [DatabaseManagement](https://github.com/Azure/azure-cosmos-js/tree/master/samples/DatabaseManagement) ukazuje, jak provádět následující úlohy.

| Úkol | API – referenční informace |
| --- | --- |
| [Vytvoření databáze, pokud ještě neexistuje](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/DatabaseManagement/app.js#L35-L37) |[Databases.createIfNotExists](https://docs.microsoft.com/javascript/api/%40azure/cosmos/databases?view=azure-node-latest#createifnotexists) |
| [Výpis databází pro účet](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/DatabaseManagement/app.js#L40-L42) |[Databases.readAll](https://docs.microsoft.com/javascript/api/%40azure/cosmos/databases?view=azure-node-latest#readall) |
| [Čtení databáze podle ID](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/DatabaseManagement/app.js#L40-L42) |[Database.read](https://docs.microsoft.com/javascript/api/%40azure/cosmos/database?view=azure-node-latest#read) |
| [Odstranění databáze](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/DatabaseManagement/app.js#L57-L60) |[Database.delete](https://docs.microsoft.com/javascript/api/%40azure/cosmos/database?view=azure-node-latest#delete) |

## <a name="container-examples"></a>Příklady pro kontejnery
Soubor [app.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ContainerManagement/app.js) projektu [ContainerManagement](https://github.com/Azure/azure-cosmos-js/tree/master/samples/ItemManagement) ukazuje, jak provádět následující úlohy.

| Úkol | API – referenční informace |
| --- | --- |
| [Vytvoření kontejneru, pokud ještě neexistuje](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ContainerManagement/app.js#L36-L37) |[Containers.createIfNotExists](https://docs.microsoft.com/javascript/api/%40azure/cosmos/containers?view=azure-node-latest#createifnotexists) |
| [Výpis kontejnerů pro účet](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ContainerManagement/app.js#L36-L37) |[Containers.readAll](https://docs.microsoft.com/javascript/api/%40azure/cosmos/containers?view=azure-node-latest#readall) |
| [Čtení kolekce podle ID](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ContainerManagement/app.js#L47-L51) |[Container.read](https://docs.microsoft.com/javascript/api/%40azure/cosmos/container?view=azure-node-latest#read) |
| [Odstranění kontejneru](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ContainerManagement/app.js#L54-L55) |[Container.delete](https://docs.microsoft.com/javascript/api/%40azure/cosmos/container?view=azure-node-latest#delete) |

## <a name="item-examples"></a>Příklady pro položky
Soubor [app.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement/app.js) projektu [ItemManagement](https://github.com/Azure/azure-cosmos-js/tree/master/samples/ItemManagement) ukazuje, jak provádět následující úlohy.

| Úkol | API – referenční informace |
| --- | --- |
| [Vytvoření položek](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L49-L56) |[Items.create](https://docs.microsoft.com/javascript/api/%40azure/cosmos/items?view=azure-node-latest#create) |
| [Čtení všech položek v kontejneru](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L59-L64) |[Items.readAll](https://docs.microsoft.com/javascript/api/%40azure/cosmos/items?view=azure-node-latest#readall) |
| [Čtení položky podle ID](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L59-L64) |[Item.read](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest#read) |
| [Čtení položky pouze v případě, že se změnila](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L73-L94) |[Item.read](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest#read)<br/>[RequestOptions.accessCondition](https://docs.microsoft.com/javascript/api/%40azure/cosmos/requestoptions?view=azure-node-latest#accesscondition) |
| [Dotazování dokumentů](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L97-L118) |[Items.query](https://docs.microsoft.com/javascript/api/%40azure/cosmos/items?view=azure-node-latest#query) |
| [Nahrazení položky](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L131-L136) |[Item.replace](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest#replace) |
| [Nahrazení položky pomocí podmíněné kontroly ETag](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L139-L160) |[Item.replace](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest#replace)<br/>[RequestOptions.accessCondition](https://docs.microsoft.com/javascript/api/%40azure/cosmos/requestoptions?view=azure-node-latest#accesscondition) |
| [Odstranění položky](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L162-L164) |[Item.delete](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest#delete) |

## <a name="indexing-examples"></a>Příklady pro indexování
Soubor [app.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/IndexManagement/app.js) projektu [IndexManagement](https://github.com/Azure/azure-cosmos-js/tree/master/samples/IndexManagement) ukazuje, jak provádět následující úlohy.

| Úkol | API – referenční informace |
| --- | --- |
| [Ruční indexování konkrétního dokumentu](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L135-L177) |[RequestOptions.indexingDirective: 'include'](https://docs.microsoft.com/javascript/api/%40azure/cosmos/requestoptions?view=azure-node-latest#indexingdirective) |
| [Ruční vyloučení konkrétního dokumentu z indexu](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L90-L131) |[RequestOptions.indexingDirective: 'exclude'](https://docs.microsoft.com/javascript/api/%40azure/cosmos/requestoptions?view=azure-node-latest#indexingdirective) |
| [Použití opožděného indexování pro hromadný import nebo čtení velkých kontejnerů](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L183-L214) |[IndexingMode.Lazy](https://docs.microsoft.com/javascript/api/%40azure/cosmos/indexingmode?view=azure-node-latest) |
| [Vyloučení cesty z indexu](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L352-L429) |[IndexingPolicy.ExcludedPath](https://docs.microsoft.com/javascript/api/%40azure/cosmos/indexingpolicy?view=azure-node-latest#excludedpaths) |
| [Povolení prohledávání v cestě řetězce během operace rozsahu](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L219-L275) |[FeedOptions.EnableScanInQuery](https://docs.microsoft.com/javascript/api/%40azure/cosmos/feedoptions?view=azure-node-latest#enablescaninquery) |
| [Vytvoření indexu rozsahu v cestě řetězce](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L281-L346) |[IndexKind.Range](https://docs.microsoft.com/javascript/api/%40azure/cosmos/indexkind?view=azure-node-latest), [IndexingPolicy](https://docs.microsoft.com/javascript/api/%40azure/cosmos/indexingpolicy?view=azure-node-latest), [Items.query](https://docs.microsoft.com/javascript/api/%40azure/cosmos/items?view=azure-node-latest#query) |
| [Vytvoření kontejneru s výchozí zásadou indexPolicy a její následná online aktualizace](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L435-L507) |[Containers.create](https://docs.microsoft.com/javascript/api/%40azure/cosmos/containers?view=azure-node-latest#create)


Další informace o indexování najdete v tématu pojednávajícím o [zásadách indexování služby Azure Cosmos DB](indexing-policies.md).

## <a name="server-side-programming-examples"></a>Příklady programování na straně serveru
Soubor [app.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ServerSideScripts/app.js) projektu [ServerSideScripts](https://github.com/Azure/azure-cosmos-js/tree/master/samples/ServerSideScripts) ukazuje, jak provádět následující úlohy.

| Úkol | API – referenční informace |
| --- | --- |
| [Vytvoření uložené procedury](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ServerSideScripts/JS/upsert.js#L12-L72) |[StoredProcedures.create](https://docs.microsoft.com/javascript/api/%40azure/cosmos/storedprocedures?view=azure-node-latest) |
| [Spuštění uložené procedury](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ServerSideScripts/app.js#L44) |[StoredProcedure.execute](https://docs.microsoft.com/javascript/api/%40azure/cosmos/storedprocedure?view=azure-node-latest#execute) |

Další informace o programování na straně serveru najdete v tématu o [programování na straně serveru pro službu Azure Cosmos DB: uložené procedury, aktivační události databáze a UDF](programming.md).

