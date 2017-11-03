---
title: "Použití Zeppelin ke spouštění dotazů Hive v Azure HDInsight | Microsoft Docs"
description: "Další informace o použití Zeppelin ke spouštění dotazů Hive."
keywords: "hdinsight hadoop, hive, interaktivní dotaz, LLAP"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: jgao
ms.openlocfilehash: b44321619f2aa94a6d98624ab1ee35a598fb6fc8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-zeppelin-to-run-hive-queries-in-azure-hdinsight"></a>Použití Zeppelin ke spouštění dotazů Hive v Azure HDInsight 

Clustery HDInsight interaktivní dotazu zahrnují poznámkových bloků Zeppelin, které můžete použít ke spuštění interaktivních dotazů Hive. V tomto článku zjistěte, jak používat Zeppelin ke spouštění dotazů Hive v Azure HDInsight. 

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto článku, musíte mít následující položky:

* **Cluster HDInsight interaktivní dotazu**. V tématu [vytvořit cluster](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster) k vytvoření clusteru HDInsight.  Ujistěte se, že vyberte typ interaktivní dotazu. 

## <a name="create-a-zeppelin-note"></a>Vytvoření Zeppelin poznámky

1. Přejděte na následující adresu URL:

        https://CLUSTERNAME.azurehdinsight.net/zeppelin
    Nahraďte **CLUSTERNAME** názvem vašeho clusteru.

2. Zadejte Hadoop uživatelské jméno a heslo. Na stránce Zeppelin můžete buď vytvořit novou poznámku nebo otevřete stávající poznámky. HiveSample obsahuje několik ukázkových dotazů nástroje Hive.  

    ![Interaktivní dotazu HDInsight zeppelin](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin.png)
3. Klikněte na tlačítko **vytvořit novou poznámku**.
4. Zadejte nebo vyberte tyto hodnoty:

    - Poznámka: název: Zadejte název pro poznámku.
    - Výchozí překladač: vyberte **JDBC**.

5. Klikněte na tlačítko **vytvořit Poznámka**.
6. Spuštěním následujícího dotazu Hive:

        %jdbc(hive)
        show tables

    ![Interaktivní dotazu HDInsight zeppelin spustí dotaz](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin-query.png)

    **%Jdbc(hive)** příkaz na prvním řádku sděluje poznámkového bloku používat překladač Hive JDBC.

    Dotaz vrátí jeden tabulkou Hive se nazývají *hivesampletable*.

    Níže jsou uvedeny dva další dotazy Hive, které se dají spouštět i proti hivesampletable. 

        %jdbc(hive)
        select * from hivesampletable limit 10

        %jdbc(hive)
        select ${group_name}, count(*) as total_count
        from hivesampletable
        group by ${group_name=market,market|deviceplatform|devicemake}
        limit ${total_count=10}

    Porovnání s tradiční Hive, výsledky dotazu vraťte musí rychlejší.


## <a name="next-steps"></a>Další kroky
V tomto článku jste zjistili, jak k vizualizaci dat z prostředí HDInsight pomocí Power BI.  Další informace naleznete v následujících článcích:

* [Vizualizovat data Hive s Microsoft Power BI v Azure HDInsight](./hdinsight-connect-hive-power-bi.md).
* [Připojení aplikace Excel do HDInsight pomocí ovladače ODBC Microsoft Hive](./hdinsight-connect-excel-hive-odbc-driver.md).
* [Připojení aplikace Excel k systému Hadoop pomocí Power Query](./hdinsight-connect-excel-power-query.md).
* [Připojení k Azure HDInsight a spouštět dotazy Hive pomocí nástrojů Data Lake pro Visual Studio](./hdinsight-hadoop-visual-studio-tools-get-started.md).
* [Pomocí nástroje Azure HDInsight pro Visual Studio Code](hdinsight-for-vscode.md).
* [Nahrání dat do HDInsight](./hdinsight-upload-data.md).
