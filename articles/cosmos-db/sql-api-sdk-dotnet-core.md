---
title: 'Azure Cosmos DB: SQL API pro .NET Core, sady SDK a prostředky | Dokumentace Microsoftu'
description: Další informace o SQL API pro .NET Core a sady SDK, včetně data vydání, vyřazení dat a změny provedené mezi každou verzi sady Azure Cosmos DB .NET Core SDK.
services: cosmos-db
author: rnagpal
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/22/2018
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f3c0efe7fbeb3d3259d1e8505a965499cfb941e9
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44049384"
---
# <a name="azure-cosmos-db-net-core-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB .NET Core SDK pro rozhraní SQL API: poznámky k verzi a prostředky
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [Kanál změn .NET](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [Poskytovatel prostředků REST](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [BulkExecutor – .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor – Java](sql-api-sdk-bulk-executor-java.md)

<table>

<tr><td>**Stažení sady SDK**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)</td></tr>

<tr><td>**Dokumentace k rozhraní API**</td><td>[Referenční dokumentace rozhraní API .NET](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td>**Ukázky**</td><td>[Ukázky kódu .NET](sql-api-dotnet-samples.md)</td></tr>

<tr><td>**Začínáme**</td><td>[Začínáme s Azure Cosmos DB .NET Core SDK](sql-api-dotnetcore-get-started.md)</td></tr>

<tr><td>**Kurz vývoje webové aplikace**</td><td>[Vývoj webových aplikací pomocí služby Azure Cosmos DB](sql-api-dotnet-application.md)</td></tr>

<tr><td>**Aktuální podporované architektury**</td><td>[.NET standard 1.6 a .NET Standard 1.5](https://www.nuget.org/packages/NETStandard.Library)</td></tr>
</table></br>

## <a name="release-notes"></a>Poznámky k verzi

Azure Cosmos DB .NET Core SDK má paritu funkcí s nejnovější verzí [.NET SDK služby Azure Cosmos DB](sql-api-sdk-dotnet.md).

### <a name="a-name200-preview2200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-preview2

* Přidání žádosti o podporu zrušení.
* Přidání SetCurrentLocation k ConnectionPolicy, který automaticky naplní upřednostňované umístění podle oblastí.
* Oprava chyby v různé dotazy oddílu s Min/Max a filtr, který odpovídá žádné dokumenty v jednotlivých oddílů.

### <a name="a-name200-preview200-preview"></a><a name="2.0.0-preview"/>2.0.0-Preview

* DocumentClient metody mají nyní se IDocumentClient.
* Aktualizované s přímým přístupem přenosu protokolů TCP k omezení počtu připojení.
* Přidání podpory pro přímý režim TCP pro klienty než Windows.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0

* Přidání vlastnosti ConsistencyLevel do FeedOptions.
* Přidání JsonSerializerSettings RequestOptions a FeedOptions.
* Přidání EnableReadRequestsFallback k ConnectionPolicy.

### <a name="a-name191191"></a><a name="1.9.1"/>1.9.1

* Oprava keynotfoundexception – pro různé uspořádání oddílu na základě dotazů v krajních případech.
* Oprava chyby, kde není respektováno JsonPropery atribut v klauzuli select dotazů LINQ.

### <a name="a-name182182"></a><a name="1.8.2"/>1.8.2

* Oprava chyby, který je přístupů za určitých podmínek závodu, jehož výsledkem přerušované "Microsoft.Azure.Documents.NotFoundException: čtení relace není k dispozici pro token vstupní relace" chyby při použití úrovně konzistence relace.

### <a name="a-name181181"></a><a name="1.8.1"/>1.8.1

* Oprava regrese kde FeedOptions.MaxItemCount = -1 došlo System.ArithmeticException: velikost stránky je záporný.
* Přidat novou funkci ToString() do QueryMetrics.
* Vystavená statistiky oddílu na kolekce pro čtení.
* Přidání vlastnosti PartitionKey do ChangeFeedOptions.
* Menšími opravami chyb.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
 
 * Přidá schopnost určit jedinečné indexy pro dokumenty pomocí vlastnosti UniqueKeyPolicy na DocumentCollection.
 * Je opravená chyba, ve kterém nebyly respektováno vlastní nastavení serializátor JsonSerializer pro některé dotazy a spuštění uložené procedury.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
 
 * Branding změny z Azure DocumentDB do služby Azure Cosmos DB v referenční dokumentaci rozhraní API, dokumentaci, informace o metadatech v sestaveních a balíček NuGet. 
 * Diagnostické informace a čekací doba odpovědi žádosti odeslané s režimem přímé připojení k zpřístupnit. Názvy vlastností, které jsou ve třídě ResourceResponse RequestDiagnosticsString a RequestLatency.
 * Tato verze sady SDK vyžaduje nejnovější verzi Azure emulátor služby Cosmos DB k dispozici ke stažení z https://aka.ms/cosmosdb-emulator.
 
### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0

* Přidat několik vylepšení a oprav spolehlivosti.

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1 

* Interní změny související podpora [Microsoft.Azure.Graphs](https://docs.microsoft.com/azure/cosmos-db/graph-sdk-dotnet)

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0 

* Přidání podpory pro PartitionKeyRangeId jako FeedOption pro vymezení výsledky dotazu na rozsah klíče hodnotu konkrétního oddílu. 
* Přidání podpory pro StartTime jako ChangeFeedOption, abyste mohli začít sledovat změny po uplynutí této doby. 

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1

*   Opravili jsme problém ve třídě JsonSerializable, který může způsobit výjimku přetečení zásobníku.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0

*   Přidání podpory pro zadání vlastní JsonSerializerSettings při tvorbě instance [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) instance.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2

*   Podpora .NET Standard 1.5 jako jeden z cílových platforem.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1

*   Opravili jsme problém, který x64 měla vliv na počítače, které není podpora SSE4 instrukce a vyvolat SEHException – při spuštění dotazů služby Azure Cosmos DB.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

*   Volá se přidání podpory pro nové úrovně konzistence ConsistentPrefix.
*   Přidání podpory pro dotaz metriky pro jednotlivé oddíly.
*   Přidání podpory pro omezení velikosti token pro pokračování pro dotazy.
*   Přidání podpory pro podrobné trasování pro chybné žádosti.
*   Provedli několik vylepšení výkonu v sadě SDK.

### <a name="a-name122122"></a><a name="1.2.2"/>1.2.2

* Opravili jsme problém, který ignoruje hodnotu PartitionKey podle FeedOptions agregačních dotazů.
* Opravili jsme problém v transparentního zpracování oddílu správy během napříč oddíly uprostřed letu klauzule Order By provádění dotazů.

### <a name="a-name121121"></a><a name="1.2.1"/>1.2.1

* Opravili jsme chybu, která způsobila zablokování v některé z asynchronní rozhraní API při použití v rámci kontextu technologie ASP.NET.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Opravy, aby sada SDK odolnější automatické převzetí služeb při selhání za určitých podmínek.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Opraven problém, který občas způsobí, že o výjimku WebException: vzdálený název nelze rozpoznat.
* Přidání podpory pro přímo tak, že přidáte nová přetížení ReadDocumentAsync rozhraní API pro čtení typu dokumentu.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* Přidání podpory LINQ dotazů agregace (počet, MIN, MAX, součet a průměr).
* Opravili problém nevrácení paměti pro objekt ConnectionPolicy způsobené použijte obslužnou rutinu události.
* Oprava problému, ve které nebyl UpsertAttachmentAsync pracovat, když se použil značku ETag.
* Oprava problému, ve které se mezi oddíl klauzule order by dotazu pokračování práce při řazení na pole řetězce.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Přidání podpory pro dotazy agregace (počet, MIN, MAX, součet a průměr). Zobrazit [podporu agregace](sql-api-sql-query.md#Aggregates).
* Snížili minimální propustnost na dělené kolekce z 10,100 RU/s na 2 500 RU/s.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

Azure Cosmos DB .NET Core SDK umožňuje vytváření rychlých a multiplatformní [ASP.NET Core](https://www.asp.net/core) a [.NET Core](https://www.microsoft.com/net/core#windows) aplikace pro Windows, Mac a Linux. Nejnovější verzi sady Azure Cosmos DB .NET Core SDK je plně [Xamarin](https://www.xamarin.com) kompatibilní a použít pro vytváření aplikací určených pro iOS, Android a Mono (Linux).  

### <a name="a-name010-preview010-preview"></a><a name="0.1.0-preview"/>0.1.0-Preview

Azure Cosmos DB .NET Core ve verzi Preview SDK umožňuje vytváření rychlých a multiplatformní [ASP.NET Core](https://www.asp.net/core) a [.NET Core](https://www.microsoft.com/net/core#windows) aplikace pro Windows, Mac a Linux.

Azure Cosmos DB .NET Core ve verzi Preview SDK má paritu funkcí s nejnovější verzí [.NET SDK služby Azure Cosmos DB](sql-api-sdk-dotnet.md) a podporuje následující:
* Všechny [režimy připojení](performance-tips.md#networking): režim brány, s přímým přístupem TCP a HTTPs s přímým přístupem. 
* Všechny [úrovně konzistence](consistency-levels.md): silný, relace, omezená Neaktuálnost a konečná.
* [Dělené kolekce](partition-data.md). 
* [Účty databáze ve více oblastech a geografická replikace](distribute-data-globally.md).

Pokud máte dotazy související s touto sadou SDK, zveřejněte ji do [StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb), nebo přidejte problém v [úložiště github](https://github.com/Azure/azure-documentdb-dotnet/issues). 

## <a name="release--retirement-dates"></a>Verze & data vyřazení z provozu

| Verze | Datum vydání | Datum vyřazení z provozu |
| --- | --- | --- |
| [2.0.0-preview2](#2.0.0-preview2) |26. července 2018 |--- |
| [2.0.0-Preview](#2.0.0-preview) |11. května 2018 |--- |
| [1.9.1](#1.9.1) |09. března 2018 |--- |
| [1.8.2](#1.8.2) |21. února 2018 |--- |
| [1.8.1](#1.8.1) |05. února 2018 |--- |
| [1.7.1](#1.7.1) |16. listopadu 2017 |--- |
| [1.7.0](#1.7.0) |10. listopadu 2017 |--- |
| [1.6.0](#1.6.0) |17. října 2017 |--- |
| [1.5.1](#1.5.1) |02. října 2017 |--- |
| [1.5.0](#1.5.0) |10. srpna 2017 |--- | 
| [1.4.1](#1.4.1) |07. srpna 2017 |--- |
| [1.4.0](#1.4.0) |02. srpna 2017 |--- |
| [1.3.2](#1.3.2) |12. června 2017 |--- |
| [1.3.1](#1.3.1) |23. května 2017 |--- |
| [1.3.0](#1.3.0) |10. května 2017 |--- |
| [1.2.2](#1.2.2) |19. dubna 2017 |--- |
| [1.2.1](#1.2.1) |29. března 2017 |--- |
| [1.2.0](#1.2.0) |25. března 2017 |--- |
| [1.1.2](#1.1.2) |20. března 2017 |--- |
| [1.1.1](#1.1.1) |14. března 2017 |--- |
| [1.1.0](#1.1.0) |16. února 2017 |--- |
| [1.0.0](#1.0.0) |21. prosince 2016 |--- |
| [0.1.0-preview](#0.1.0-preview) |15. listopadu 2016 |Do 31. prosince 2016 |

## <a name="see-also"></a>Viz také
Další informace o službě Cosmos DB najdete v tématu [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) stránku služby. 

