---
title: "Vytvoření clusteru Apache Spark ve službě Azure HDInsight | Dokumentace Microsoftu"
description: "Rychlý start k HDInsight Spark týkající se vytvoření clusteru Apache Spark ve službě HDInsight."
keywords: spark quickstart, interactive spark, interactive query, hdinsight spark, azure spark
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/29/2017
ms.author: jgao
ms.openlocfilehash: 516c48424ef5d1256296240541fb544c1e5d9205
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/05/2017
---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a>Vytvoření clusteru Apache Spark ve službě Azure HDInsight

Zjistěte, jak vytvořit cluster Apache Spark v Azure HDInsight a jak spustit dotazy Spark SQL na tabulky Hive. Informace o Apache Spark ve službě HDInsight najdete v tématu [Přehled: Apache Spark v Azure HDInsight](apache-spark-overview.md).

## <a name="prerequisites"></a>Požadavky

* **Předplatné Azure**. Než začnete tento kurz, musíte mít předplatné Azure. Přečtěte si téma [Bezplatné vytvoření účtu Microsoft Azure](https://azure.microsoft.com/free).

## <a name="create-hdinsight-spark-cluster"></a>Vytvoření clusteru HDInsight Spark

Vytvořte cluster HDInsight Spark pomocí [šablony Azure Resource Manageru](../hdinsight-hadoop-create-linux-clusters-arm-templates.md). Šablonu jde najít v [GitHubu](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/). Ostatní metody tvorby clusteru najdete v části [Tvorba clusterů HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).

1. Kliknutím na následující obrázek otevřete šablonu na portálu Azure Portal.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Zadejte následující hodnoty:

    ![Vytvoření clusteru HDInsight Spark pomocí šablony Azure Resource Manageru](./media/apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Vytvoření clusteru Spark ve službě HDInsight pomocí šablony Azure Resource Manageru")

    * **Předplatné:** Vyberte předplatné Azure použité k vytvoření tohoto clusteru.
    * **Skupina prostředků:** Vytvořte skupinu prostředků nebo vyberte stávající. Skupina prostředků slouží ke správě prostředků Azure pro vaše projekty.
    * **Umístění:** Vyberte umístění pro skupinu prostředků. Šablona toto umístění používá k vytvoření clusteru i jako výchozí úložiště clusteru.
    * **Název clusteru:** Zadejte název pro cluster HDInsight, který chcete vytvořit.
    * **Přihlašovací jméno a heslo clusteru**: výchozí přihlašovací jméno je admin.
    * **Uživatelské jméno a heslo SSH**.

3. Vyberte **Souhlasím s podmínkami a ujednáními uvedenými nahoře**, vyberte **Připnout na řídicí panel** a potom klikněte na **Koupit**. Zobrazí se nová dlaždice s názvem Odeslání nasazení pro šablonu nasazení. Vytvoření clusteru trvá přibližně 20 minut.

Pokud narazíte na problém s vytvářením clusterů HDInsight, může to být způsobeno tím, že k tomu nemáte správná oprávnění. Další informace najdete v tématu popisujícím [požadavky na řízení přístupu](../hdinsight-administer-use-portal-linux.md#create-clusters).

> [!NOTE]
> Tento článek vytvoří cluster Spark, který využívá [Azure Storage Blob jako úložiště clusteru](../hdinsight-hadoop-use-blob-storage.md). Můžete také vytvořit cluster Spark, který jako výchozí úložiště používá [Azure Data Lake Store](../hdinsight-hadoop-use-data-lake-store.md). Pokyny naleznete v tématu [Vytvoření clusteru HDInsight pomocí Data Lake Store](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).
>
>

## <a name="create-a-jupyter-notebook"></a>Vytvoření poznámkového bloku Jupyter

[Poznámkový blok Jupyter](http://jupyter.org) je interaktivní prostředí poznámkového bloku podporující různé programovací jazyky, které vám umožní pracovat s daty, kombinovat kód s textem markdownu a provádět jednoduché vizualizace. Spark v HDInsight také zahrnuje [poznámkový blok Zeppelin](apache-spark-zeppelin-notebook.md). V tomto kurzu se používá poznámkový blok Jupyter.

**Vytvoření poznámkového bloku Jupyter**

1. Otevřete web [Azure Portal](https://portal.azure.com/).

2. Otevřete cluster Spark, který jste vytvořili. Pokyny najdete v popisu [výpisu a zobrazení clusterů](../hdinsight-administer-use-portal-linux.md#list-and-show-clusters).

3. V části **Rychlé odkazy** klikněte na **Řídicí panely clusteru** a potom klikněte na **Poznámkový blok Jupyter**. Po vyzvání zadejte přihlašovací údaje správce clusteru.

   ![Otevření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Otevření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL")

   > [!NOTE]
   > K poznámkovému bloku Jupyter pro váš cluster se dostanete také otevřením následující adresy URL v prohlížeči. Nahraďte **CLUSTERNAME** názvem clusteru:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Klikněte na tlačítko **Nový** a pak klikněte na tlačítko **PySpark**, aby se vytvořil poznámkový blok. Poznámkové bloky Jupyter v clusterech HDInsight podporují tři jádra – **PySpark**, **PySpark3** a **Spark**. V tomto kurzu se používá jádro **PySpark**. Další informace o jádrech a výhody použití jádra **PySpark** najdete v tématu s popisem [použití jader poznámkových bloků Jupyter s clustery Apache Spark v HDInsight](apache-spark-jupyter-notebook-kernels.md).

   ![Vytvoření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Vytvoření poznámkového bloku Jupyter pro spuštění interaktivního dotazu Spark SQL")

   Nový poznámkový blok se vytvoří a otevře s názvem Bez názvu (Bez názvu.pynb).

4. Pokud chcete, klikněte na název poznámkového bloku v horní části a zadejte popisný název.

    ![Zadání názvu, ze kterého má poznámkový blok Jupyter spustit interaktivní dotaz Spark](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Zadání názvu, ze kterého má poznámkový blok Jupyter spustit interaktivní dotaz Spark")

## <a name="run-spark-sql-statements-on-a-hive-table"></a>Spouštění příkazů Spark SQL na tabulku Hive

Jazyk SQL (Structured Query Language) je nejběžnějším a široce používaným jazykem pro dotazování a definování dat. Spark SQL funguje jako rozšíření Apache Spark pro zpracování strukturovaných dat a používá známou syntaxi jazyka SQL.

Spark SQL jako dotazovací jazyky podporuje SQL i HiveQL. Mezi jeho schopnosti patří vytváření vazeb v jazycích Python, Scala a Java. Můžete díky němu dotazovat data uložená v mnoha umístěních, jako jsou externí databáze, strukturované datové soubory (například JSON) a tabulky Hive.

Příklad čtení dat ze souboru CSV místo tabulky Hive najdete v části [Spouštění interaktivních dotazů v clusterech Spark v HDInsight](apache-spark-load-data-run-query.md).

**Spuštění Spark SQL**

1.  V poznámkovém bloku vložte do prázdné buňky následující kód a stisknutím **SHIFT + ENTER** kód spusťte. 

    ```PySpark
    %%sql
    SELECT * FROM hivesampletable LIMIT 10
    ```

    ![Dotaz Hive v HDInsight Spark](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Dotaz Hive v HDInsight Spark")

    Pokud používáte poznámkový blok Jupyter s clusterem HDInsight Spark, získáte přednastavený kontext `sqlContext`, který můžete použít ke spouštění dotazů Hive pomocí Spark SQL. `%%sql` říká poznámkovému bloku Jupyter, aby ke spuštění dotazu Hive použil přednastavený kontext `sqlContext`. Dotaz načte prvních 10 řádků z tabulky Hive (**hivesampletable**), která je ve výchozím nastavení k dispozici na všech clusterech HDInsight. Další informace o magickém příkazu `%%sql` a přednastavených kontextech najdete v tématu [Dostupná jádra Jupyter pro cluster HDInsight](apache-spark-jupyter-notebook-kernels.md).

    Při každém spuštění dotazu v Jupyter se v názvu okna webového prohlížeče zobrazí stav **(Busy)** (Zaneprázdněn) společně s názvem poznámkového bloku. Zobrazí se také plný kroužek vedle textu **PySpark** v pravém horním rohu. Po dokončení úlohy se změní na prázdný kruh.
    
    Obrazovka by se měla aktualizovat a zobrazit výstup dotazu.

    ![Výstup dotazu Hive v HDInsight Spark](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Výstup dotazu Hive v HDInsight Spark")

2. V nabídce **Soubor** poznámkového bloku klikněte na **Zavřít a zastavit**. Ukončením poznámkového bloku se uvolní prostředky clusteru.

3. Pokud se chystáte další kroky dokončit později, nezapomeňte odstranit cluster HDInsight, který jste vytvořili v rámci tohoto článku. 

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-step"></a>Další krok 

V tomto článku jste zjistili, jak vytvořit cluster HDInsight Spark a spustit základní dotaz Spark SQL. Přejděte k dalšímu článku a zjistěte, jak pomocí clusteru HDInsight Spark spouštět interaktivní dotazy na ukázková data.

> [!div class="nextstepaction"]
>[Spouštění interaktivních dotazů v clusteru HDInsight Spark.](apache-spark-load-data-run-query.md)



