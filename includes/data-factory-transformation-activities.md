Azure Data Factory podporuje následující aktivity transformace, které lze přidat do kanálů jednotlivě nebo ve spojení s jinou aktivitou.

| Aktivita transformace dat | Výpočetní prostředí |
|:--- |:--- |
| [Hive](../articles/data-factory/v1/data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Pig](../articles/data-factory/v1/data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [MapReduce](../articles/data-factory/v1/data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Streamování Hadoop](../articles/data-factory/v1/data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Spark](../articles/data-factory/v1/data-factory-spark.md) | HDInsight [Hadoop] |
| [Aktivity Machine Learning: Dávkové spouštění a Aktualizace prostředku](../articles/data-factory/v1/data-factory-azure-ml-batch-execution-activity.md) |Virtuální počítač Azure |
| [Uložená procedura](../articles/data-factory/v1/data-factory-stored-proc-activity.md) |Azure SQL, Azure SQL Data Warehouse nebo SQL Server |
| [U-SQL Data Lake Analytics](../articles/data-factory/v1/data-factory-usql-activity.md) |Azure Data Lake Analytics |
| [DotNet](../articles/data-factory/v1/data-factory-use-custom-activities.md) |HDInsight [Hadoop] nebo Azure Batch |

> [!NOTE]
> Pomocí aktivity MapReduce můžete na svém clusteru HDInsight Spark spouštět programy Spark. Podrobnosti najdete v článku [Vyvolání programů Spark ze služby Azure Data Factory](../articles/data-factory/v1/data-factory-spark.md).
> Můžete vytvořit vlastní aktivitu pro spouštění skriptů jazyka R na svém clusteru HDInsight s nainstalovaným jazykem R. Viz [Spuštění skriptu jazyka R pomocí služby Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).
> 
> 

