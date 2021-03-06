---
title: Data scénáře zahrnující Gen1 úložiště Data Lake | Dokumentace Microsoftu
description: Pochopit různé scénáře a nástroje, pomocí kterých můžete ingestuje, zpracování, stahování a vizualizovat v Data Lake Storage Gen1 (dříve označované jako Azure Data Lake Store)
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: nitinme
ms.openlocfilehash: 19743dcd2866b8fc7b6ad1fdf387b134f7b3ca50
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/05/2018
ms.locfileid: "43783072"
---
# <a name="using-azure-data-lake-storage-gen1-for-big-data-requirements"></a>Použití Azure Data Lake Storage Gen1 pro potřeby velkého objemu dat

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Existují čtyři fáze klíče ve zpracování velkého objemu dat:

* Ingestovat velké objemy dat do úložiště dat v reálném čase nebo v dávkách
* Zpracování dat
* Stahování dat
* Vizualizace dat

V tomto článku se podíváme na těchto fází s ohledem na Azure Data Lake Store vám pomohou pochopit možnosti a nástroje k vašim potřebám velkých objemů dat k dispozici.

## <a name="ingest-data-into-data-lake-store"></a>Ingestovat data do Data Lake Store
Tato část se zaměřuje různých zdrojích dat a různými způsoby, ve které je možné ingestovat data do účtu Azure Data Lake Store.

![Ingestovat data do Data Lake Store](./media/data-lake-store-data-scenarios/ingest-data.png "Ingestovat data do Data Lake Store")

### <a name="ad-hoc-data"></a>Dat ad hoc
To představuje menší datové sady, které se používá pro vytváření prototypů velké objemy dat aplikace. Ingestuje dat ad hoc v závislosti na zdroji dat různými způsoby.

| Zdroj dat | Ingestování pomocí |
| --- | --- |
| Místní počítač |<ul> <li>[Azure Portal](data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Azure Cross-platform CLI 2.0](data-lake-store-get-started-cli-2.0.md)</li> <li>[Pomocí nástrojů Data Lake pro Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md) </li></ul> |
| Azure Storage Blob |<ul> <li>[Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)</li> <li>[Nástroj AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[DistCp běžící v clusteru HDInsight](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

### <a name="streamed-data"></a>Datové proudy
Reprezentuje data, která mohou být generovány různých zdrojů, jako jsou aplikace, zařízení, snímačů atd. Tato data je možné ingestovat v Data Lake Store pomocí různých nástrojů. Tyto nástroje se obvykle zachycení a zpracování dat na základě událostí událostí v reálném čase a napište událostí v dávkách do Data Lake Store tak, aby se mohou být dále zpracovány.

Toto jsou nástroje, které můžete použít:

* [Azure Stream Analytics](../stream-analytics/stream-analytics-data-lake-output.md) -události ingestuje do služby Event Hubs je možné zapisovat na Azure Data Lake pomocí Azure Data Lake Store výstup.
* [Azure HDInsight Storm](../hdinsight/storm/apache-storm-write-data-lake-store.md) – umožňuje zápis dat přímo do Data Lake Store z clusteru Storm.
* [EventProcessorHost](../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md) – může přijímat události ze služby Event Hubs a potom se zapsaly do Data Lake Store pomocí [Data Lake Store .NET SDK](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>Relační data
Můžete také zdroje dat z relačních databází. Po určitou dobu shromažďovat relačních databází obrovské objemy dat, které můžou poskytovat hlavní zjištěné poznatky, pokud se zpracovává prostřednictvím kanálu velké objemy dat. Tyto nástroje můžete použít k přesunutí těchto dat do Data Lake Store.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Data protokolu webového serveru (nahrávání pomocí vlastních aplikací)
Tento typ datové sady je konkrétně volat, protože analýzy dat protokolů webového serveru je běžným případem použití pro velké objemy dat aplikace a vyžaduje velké objemy soubory protokolů k nahrání do Data Lake Store. K zápisu vlastních skriptů nebo aplikací k odeslání těchto dat můžete použít některý z následujících nástrojů.

* [Azure Cross-platform CLI 2.0](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Data Lake Store .NET SDK](data-lake-store-get-started-net-sdk.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)

Pro nahrávání dat protokolů webového serveru a také pro nahrávání jiné druhy dat (např. sociální zabarvení dat) je dobrý přístup k zapisovat vlastní vlastních skriptů nebo aplikací, protože poskytuje flexibilní zahrnout data nahrávání komponenty jako součást větší velké objemy dat aplikace. V některých případech může být tento kód ve tvaru skriptu nebo nástroj příkazového řádku jednoduché. V ostatních případech kódu lze integrovat zpracování velkého objemu dat do podnikových aplikací nebo řešení.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Data související s využitím clusterů Azure HDInsight
Většina typy clusterů HDInsight (Hadoop, HBase, Storm) podporují Data Lake Store jako úložiště dat úložiště. Clustery HDInsight přistupovat k datům z objektů BLOB Azure Storage (WASB). Pro zajištění lepšího výkonu můžete zkopírovat data z WASB do účtu Data Lake Store přidružené ke clusteru. Ke zkopírování dat můžete použít následující nástroje.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy služby](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Data uložená v místním nebo IaaS Hadoop clusterů
Velké objemy dat mohou být uložena ve stávajících clusterů Hadoop, místně na počítači, které používají HDFS. Clusterů systému Hadoop, může být v místním nasazení nebo může být v rámci clusteru služby IaaS v Azure. Je možné požadavky ke zkopírování těchto dat do Azure Data Lake Store a přístup jednorázové nebo opakované způsobem. Existují různé možnosti, které vám umožní dosáhnout. Níže je seznam alternativních a přidružené kompromisy.

| Přístup | Podrobnosti | Výhody | Požadavky |
| --- | --- | --- | --- |
| Kopírování dat z clusterů Hadoop přímo do Azure Data Lake Store pomocí Azure Data Factory (ADF) |[ADF podporuje HDFS jako zdroj dat](../data-factory/connector-hdfs.md) |ADF poskytuje podporu out-of-the-box HDFS a prvotřídní end-to-end správy a monitorování |Vyžaduje bránu správy dat nasazená místně, nebo infrastruktury jako clusterů |
| Exportujte dat z Hadoopu jako soubory. Pak zkopírujte soubory do Azure Data Lake Store pomocí vhodný mechanismus. |Zkopírujte soubory do Azure Data Lake Store pomocí: <ul><li>[Prostředí Azure PowerShell pro operační systém Windows](data-lake-store-get-started-powershell.md)</li><li>[Azure Cross-platform CLI 2.0 pro Windows OS](data-lake-store-get-started-cli-2.0.md)</li><li>Vlastní aplikace s použitím jakékoli sady SDK pro Data Lake Store</li></ul> |Rychle začít. Můžete provést vlastní nahrávání |Vícefázový proces, který zahrnuje více technologií. Monitorování a správa přibude být problém v čase vzhledem k povaze vlastní nástroje |
| Použití Distcp ke kopírování dat z Hadoopu do služby Azure Storage. Zkopírujte data ze služby Azure Storage k Data Lake Store pomocí vhodný mechanismus. |Pomocí Data Lake Store můžete kopírování dat ze služby Azure Storage: <ul><li>[Azure Data Factory](../data-factory/copy-activity-overview.md)</li><li>[Nástroj AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Apache DistCp spouštění v clusterech HDInsight](data-lake-store-copy-data-wasb-distcp.md)</li></ul> |Můžete použít open source nástroje. |Vícefázový proces, který zahrnuje více technologií |

### <a name="really-large-datasets"></a>Velmi rozsáhlé datové sady
Pro nahrání datových sad, které v rozsahu v několika terabajtů, pomocí metod popsaných výše někdy může být pomalé a nákladná. V takovém případě můžete pomocí následujících možností.

* **Pomocí Azure ExpressRoute**. Azure ExpressRoute umožňuje vytvářet privátní připojení mezi datacentry Azure a infrastrukturou ve vlastních prostorách. To poskytuje spolehlivé variantou při přenosech velkých objemů dat. Další informace najdete v tématu [dokumentace ke službě Azure ExpressRoute](../expressroute/expressroute-introduction.md).
* **"Do režimu offline" nahrávání dat**. Pokud pomocí Azure ExpressRoute není vhodná z jakéhokoli důvodu, můžete použít [služba Azure Import/Export](../storage/common/storage-import-export-service.md) dodávat pevných disků se svými daty a datového centra Azure. Vaše data se nejprve nahrát do objektů BLOB Azure Storage. Pak můžete použít [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) nebo [AdlCopy nástroj](data-lake-store-copy-data-azure-storage-blob.md) ke zkopírování dat z Azure Storage BLOB do Data Lake Store.

  > [!NOTE]
  > Při použití služby Import/Export, velikosti souborů na discích, které se dodávají k datovému centru Azure by neměla být větší než 195 GB.
  >
  >

## <a name="process-data-stored-in-data-lake-store"></a>Zpracování dat uložených v Data Lake Store
Jakmile jsou data dostupná v Data Lake Store můžete spustit analýzu na těchto datech pomocí aplikací podporovaných velké objemy dat. V současné době můžete použít Azure HDInsight a Azure Data Lake Analytics můžete spouštět úlohy analýzy dat na data uložená v Data Lake Store.

![Analýza dat v Data Lake Store](./media/data-lake-store-data-scenarios/analyze-data.png "analýzy dat v Data Lake Store")

Můžete si prohlédnout následující příklady.

* [Vytvoření clusteru HDInsight s Data Lake Store jako úložiště](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)

## <a name="download-data-from-data-lake-store"></a>Stáhnout data z Data Lake Store
Můžete také chtít stahovat nebo přesun dat z Azure Data Lake Store pro scénáře, jako například:

* Přesun dat do jiných úložišť rozhraní s vaší stávající kanálů zpracování dat. Můžete například chtít přesunout data z Data Lake Store do Azure SQL Database nebo místní SQL Server.
* Stahování dat do místního počítače pro zpracování v prostředí (IDE) při vytváření prototypů aplikací.

![Výchozí přenos dat data z Data Lake Store](./media/data-lake-store-data-scenarios/egress-data.png "výchozí přenos dat data z Data Lake Store")

V takovém případě můžete použít některý z následujících možností:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/copy-activity-overview.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

Následující metody můžete také napsat vlastní skript nebo aplikaci stáhnout data z Data Lake Store.

* [Azure Cross-platform CLI 2.0](data-lake-store-get-started-cli-2.0.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure Data Lake Store .NET SDK](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Vizualizace dat v Data Lake Store
Chcete-li vytvořit vizuální reprezentace dat uložených v Data Lake Store můžete použít kombinaci služeb.

![Vizualizace dat v Data Lake Store](./media/data-lake-store-data-scenarios/visualize-data.png "vizualizace dat v Data Lake Store")

* Můžete spustit pomocí [Azure Data Factory k přesunu dat z Data Lake Store do Azure SQL Data Warehouse](../data-factory/copy-activity-overview.md)
* Potom můžete [integrace Power BI s využitím Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-visualize-with-power-bi.md) vizuální znázornění dat vytvořit.
