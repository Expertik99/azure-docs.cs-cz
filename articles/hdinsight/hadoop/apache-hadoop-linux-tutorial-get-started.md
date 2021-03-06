---
title: 'Rychlý start: Začínáme s Hadoopem a Hivem ve službě Azure HDInsight pomocí šablony Resource Manageru '
description: Naučte se vytvářet clustery HDInsight a dotazy na data pomocí Hive.
keywords: začínáme používat hadoop, hadoop linux, hadoop rychlý start, hive začínáme, hive rychlý start
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
ms.reviewer: jasonh
ms.custom: hdinsightactive,hdiseo17may2017,mvc
ms.topic: quickstart
ms.date: 05/07/2018
ms.openlocfilehash: 9e58f0a9de20acd2a8d65f6a2252e2d439beb250
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "43049554"
---
# <a name="quickstart-get-started-with-hadoop-and-hive-in-azure-hdinsight-using-resource-manager-template"></a>Rychlý start: Začínáme s Hadoopem a Hivem ve službě Azure HDInsight pomocí šablony Resource Manageru

V tomto článku se dozvíte, jak pomocí šablony Resource Manageru vytvořit cluster [Hadoop](http://hadoop.apache.org/) ve službě HDInsight a pak ve službě HDInsight spustit úlohy Hive. Většina úloh Hadoop jsou dávkové úlohy. Vytvoříte cluster, spustíte některé úlohy a pak cluster odstraníte. V tomto článku provedete všechny tři úlohy.

V tomto rychlém startu pomocí šablony Resource Manageru vytvoříte cluster HDInsight Hadoop. K vytvoření clusteru můžete použít také [Azure Portal](apache-hadoop-linux-create-cluster-get-started-portal.md).

Aktuálně se HDInsight dodává se [sedmi různými typy clusteru](./apache-hadoop-introduction.md#cluster-types-in-hdinsight). Každý typ clusteru podporuje odlišnou sadu komponent. Všechny typy clusteru podporují Hive. Seznam podporovaných součásti v HDInsight najdete v tématu [Co je nového ve verzích clusterů Hadoop poskytovaných v HDInsight?](../hdinsight-component-versioning.md)  

Pokud ještě nemáte předplatné Azure, [vytvořte si bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

<a name="create-cluster"></a>
## <a name="create-a-hadoop-cluster"></a>Vytvoření clusteru Hadoop

V této části vytvoříte cluster Hadoop ve službě HDInsight pomocí šablony Azure Resource Manageru. Zkušenosti se šablonami Resource Manageru se k postupu podle tohoto článku nevyžadují. 

1. Klikněte na následující tlačítko **Nasadit do Azure**, přihlaste se k Azure a otevřete šablonu Resource Manageru na webu Azure Portal. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-ssh-password%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Zadejte nebo vyberte hodnoty navržené na následujícím snímku obrazovky:

    > [!NOTE]
    > Zadané hodnoty musí být jedinečné a měly by splňovat pokyny pro pojmenování. Šablona neprovádí ověřovací kontroly. Pokud se zadané hodnoty již používají nebo pokud nesplňují příslušné pokyny, po odeslání šablony se zobrazí chyba.       
    > 
    >
    
    ![Šablona Resource Manageru pro zahájení práce s HDInsight Linux na portálu](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-arm-template-on-portal.png "Nasazení clusteru Hadoop ve službě HDInsight pomocí webu Azure Portal a šablony správce skupin prostředků")

    Zadejte nebo vyberte tyto hodnoty:
    
    |Vlastnost  |Popis  |
    |---------|---------|
    |**Předplatné**     |  Vyberte své předplatné Azure. |
    |**Skupina prostředků**     | Vytvořte skupinu prostředků nebo vyberte existující.  Skupina prostředků je kontejner komponent Azure.  V tomto případě skupina prostředků obsahuje cluster HDInsight a závislý účet služby Azure Storage. |
    |**Umístění**     | Vyberte umístění Azure, ve kterém chcete cluster vytvořit.  Pro dosažení lepšího výkonu zvolte co nejbližší umístění. |
    |**Typ clusteru**     | Vyberte **hadoop**. |
    |**Název clusteru**     | Zadejte název clusteru Hadoop. Vzhledem k tomu, že všechny clustery ve službě HDInsight sdílejí stejný obor názvů DNS, musí být tento název jedinečný. Název může mít až 59 znaků a může obsahovat písmena, číslice a pomlčky. První a poslední znak názvu nemůže být pomlčka. |
    |**Přihlašovací jméno a heslo clusteru**     | Výchozí přihlašovací jméno je **admin** (správce). Heslo musí mít minimálně 10 znaků a musí obsahovat alespoň jedno číslo, jedno velké písmeno, jedno malé písmeno a jeden jiný než alfanumerický znak (kromě znaků ' " ` \). **Nezadávejte** běžné heslo, jako je „Pass@word1“.|
    |**Uživatelské jméno a heslo SSH**     | Výchozí uživatelské jméno je **sshuser** (uživatelssh).  Uživatelské jméno SSH můžete změnit.  Pro heslo uživatele SSH platí stejné požadavky jako pro přihlašovací heslo clusteru.|
       
    Některé vlastnosti jsou v šabloně pevně kódované.  Takové hodnoty můžete konfigurovat v šabloně. Podrobnější vysvětlení těchto vlastností najdete v tématu [Vytváření clusterů Hadoop ve službě HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).

3. Vyberte **Souhlasím s podmínkami a ujednáními uvedenými nahoře** a **Připnout na řídicí panel** a pak vyberte **Koupit**. Na řídicím panelu portálu by se měla zobrazit nová dlaždice s názvem **Odesílá se nasazení**. Vytvoření clusteru trvá přibližně 20 minut.

    ![Průběh nasazení šablony](./media/apache-hadoop-linux-tutorial-get-started/deployment-progress-tile.png "Průběh nasazení šablony Azure")

4. Jakmile se cluster vytvoří, popis dlaždice se změní na zadaný název skupiny prostředků. Na dlaždici je uvedený také cluster HDInsight vytvořený v rámci skupiny prostředků. 
   
    ![Počáteční skupina prostředků HDInsight Linux](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-resource-group.png "Skupina prostředků clusteru Azure HDInsight")
    
5. Na dlaždici je uvedené také výchozí úložiště přidružené ke clusteru. Každý cluster obsahuje závislost [účtu Azure Storage](../hdinsight-hadoop-use-blob-storage.md) nebo [účtu Azure Data Lake](../hdinsight-hadoop-use-data-lake-store.md). Označuje se jako výchozí účet úložiště. Cluster HDInsight a jeho výchozí účet úložiště musí být umístěny společně a nacházet se ve stejné oblasti Azure. Odstraněním clusterů nedojde k odstranění účtu úložiště.
    

> [!NOTE]
> Další metody vytváření clusterů a principy vlastnosti používaných v tomto kurzu najdete v části [Vytváření clusterů HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).       
> 
>

## <a name="use-vscode-to-run-hive-queries"></a>Použití VSCode ke spouštění dotazů Hive

Informace o tom, jak získáte nástroje HDInsight ve VSCode, najdete v tématu [Použití nástrojů HDInsight Tools pro Visual Studio Code](../hdinsight-for-vscode.md).

### <a name="submit-interactive-hive-queries"></a>Odesílání interaktivních dotazů Hive

Pomocí nástrojů HDInsight Tools pro VSCode můžete odesílat interaktivní dotazy Hive do clusterů interaktivních dotazů HDInsight.

1. Pokud ještě nemáte pracovní složku a soubor skriptu Hive, vytvořte nové.

2. Pokud jste to ještě neudělali, připojte se k účtu Azure a pak nakonfigurujte výchozí cluster.

3. Pak zkopírujte a vložte do souboru Hive následující kód a uložte ho.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
4. Klikněte pravým tlačítkem myši na editor skriptů a vyberte **HDInsight: Hive Interactive** (HDInsight: interaktivní Hive) a odešlete tak dotaz. Nástroje také umožňují odeslat pomocí místní nabídky místo celého souboru skriptu blok kódu. Brzy potom se na nové kartě zobrazí výsledky dotazu.

   ![Výsledky interaktivního Hivu](./media/apache-hadoop-linux-tutorial-get-started/interactive-hive-result.png)

    - Panel **RESULTS** (VÝSLEDKY): Celý výsledek můžete uložit do souboru CSV, JSON nebo Excel nebo můžete vybrat jenom několik řádků.

    - Panel **MESSAGES** (ZPRÁVY): Když vyberete číslo řádku **Line**, přejdete na první řádek spuštěného skriptu.

Spuštění interaktivního dotazu zabere mnohem kratší dobu než [spuštění dávkové úlohy Hive](#submit-hive-batch-scripts).

### <a name="submit-hive-batch-scripts"></a>Odeslání skriptů dávky Hive

1. Pokud ještě nemáte pracovní složku a soubor skriptu Hive, vytvořte nové.

2. Pokud jste to ještě neudělali, připojte se k účtu Azure a pak nakonfigurujte výchozí cluster.

3. Pak zkopírujte a vložte do souboru Hive následující kód a uložte ho.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
4. Klikněte pravým tlačítkem myši na editor skriptů a vyberte **HDInsight: Hive Batch** (HDInsight: Dávka Hive) a odešlete tak úlohu Hive. 

5. Vyberte cluster, do kterého ji chcete odeslat.  

    Po odeslání úlohy Hive se na panelu **OUTPUT** (VÝSTUP) zobrazí informace o úspěšném odeslání a ID úlohy. Úloha Hive otevře také **WEBOVÝ PROHLÍŽEČ**, ve kterém se zobrazí protokoly a stav úlohy v reálném čase.

   ![odeslání výsledku úlohy Hive](./media/apache-hadoop-linux-tutorial-get-started/submit-Hivejob-result.png)

[Odeslání interaktivních dotazů Hive](#submit-interactive-hive-queries) zabere mnohem kratší dobu než odeslání dávkové úlohy.

## <a name="use-visualstudio-to-run-hive-queries"></a>Použití sady Visual Studio ke spuštění dotazů Hive

Informace o tom, jak získáte nástroje HDInsight v sadě Visual Studiu, najdete v tématu [Použití nástrojů Data Lake Tools pro Visual Studio](./apache-hadoop-visual-studio-tools-get-started.md).

### <a name="run-hive-queries"></a>Spuštění dotazů Hive

Vytvářet a spouštět dotazy Hive můžete dvěma způsoby:

* Vytváření dotazů ad-hoc
* Vytvoření aplikace Hive

Vytváření a spouštění dotazů ad hoc:

1. V **Průzkumníku serveru** vyberte **Azure** > **Clustery HDInsight**.

2. Klikněte pravým tlačítkem na cluster, ve kterém chcete spustit dotaz, a pak vyberte **Napsat dotaz Hive**.  

3. Zadejte dotazy Hive. 

    Editor Hive podporuje technologii IntelliSense. Nástroje Data Lake pro Visual Studio podporují načítání vzdálených metadat při úpravách skriptu Hive. Pokud například zadáte **SELECT * FROM**, IntelliSense vypíše všechny navrhované názvy tabulek. Pokud zadáte název tabulky, IntelliSense vypíše názvy sloupců. Nástroje podporují většinu příkazů DML Hive, poddotazů a integrovaných UDF.
   
    ![Snímek obrazovky s IntelliSense ve Visual Studio Tools pro HDInsight – příklad 1](./media/apache-hadoop-linux-tutorial-get-started/vs-intellisense-table-name.png "U-SQL IntelliSense")
   
    ![Snímek obrazovky s IntelliSense ve Visual Studio Tools pro HDInsight – příklad 2](./media/apache-hadoop-linux-tutorial-get-started/vs-intellisense-column-name.png "U-SQL IntelliSense")
   
   > [!NOTE]
   > IntelliSense navrhuje pouze metadata clusteru vybraného na panelu nástrojů služby HDInsight.
   > 
   
4. Vyberte **Odeslat** nebo **Odeslat (rozšířené)**. 
   
    ![Snímek odesílání dotazu Hive](./media/apache-hadoop-linux-tutorial-get-started/vs-batch-query.png)

   Pokud jste použili možnost rozšířeného odeslání, nakonfigurujte pro skript **Název úlohy**, **Argumenty**, **Další konfigurace** a **Stavový adresář**:

    ![Snímek obrazovky s dotazem Hive v HDInsight Hadoop](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Odeslání dotazů")

   Spouštění interaktivních dotazů Hive

   * Kliknutím na šipku dolů vyberte **interactive** (interaktivní). 
   
   * Klikněte na tlačítko **Spustit**.

   ![Snímek spouštění interaktivních dotazů Hive](./media/apache-hadoop-linux-tutorial-get-started/vs-execute-hive-query.png)

Vytvoření a spuštění řešení Hive:

1. V nabídce **Soubor** vyberte **Nový** a pak vyberte **Projekt**.
2. V levém podokně vyberte **HDInsight**. V prostředním podokně vyberte **Aplikace Hive**. Zadejte vlastnosti a pak vyberte **OK**.
   
    ![Snímek obrazovky s novým projektem Hive ve Visual Studio Tools pro HDInsight](./media/apache-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png "Vytvoření aplikací Hive v sadě Visual Studio")
3. V **Průzkumníku řešení** dvojím kliknutím otevřete skript **Script.hql**.
4. Zadejte dotazy Hive a odešlete je. (Podívejte se na pokyny v krocích 3 a 4 výše.)  



## <a name="run-hive-queries"></a>Spuštění dotazů Hive

[Apache Hive](hdinsight-use-hive.md) je nejoblíbenější součástí používanou v HDInsight. Existuje mnoho způsobů spouštění úloh Hive v HDInsight. V tomto kurzu použijete zobrazení Ambari Hive z portálu. Další metody pro odesílání úloh Hive najdete v části [Použití Hive v HDInsight](hdinsight-use-hive.md).

1. Pokud chcete otevřít Ambari, vyberte **Řídicí panel clusteru**, jak je znázorněno na předchozím snímku obrazovky.  Můžete také přejít na adresu **https://&lt;název_clusteru>.azurehdinsight.net**, kde &lt;název_clusteru> je název clusteru vytvořeného v předchozí části.

    ![Počáteční řídicí panel clusteru HDInsight Linux](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-open-cluster-dashboard.png "Počáteční řídicí panel clusteru HDInsight Linux")

2. Zadejte uživatelské jméno a heslo Hadoop, které jste zadali při vytváření clusteru. Výchozí uživatelské jméno **admin**.

3. Otevřete **Zobrazení Hive**, jak je znázorněno na následujícím snímku obrazovky:
   
    ![Výběr zobrazení Ambari](./media/apache-hadoop-linux-tutorial-get-started/selecthiveview.png "Nabídka zobrazení Hive služby HDInsight")

4. Na kartě **DOTAZ** vložte následující příkazy HiveQL do pracovního listu:
   
        SHOW TABLES;

    ![Zobrazení Hive služby HDInsight](./media/apache-hadoop-linux-tutorial-get-started/hiveview-1.png "Editor dotazů zobrazení Hive služby HDInsight")
   
   > [!NOTE]
   > Hive vyžaduje středník.       
   > 
   > 

5. Vyberte **Provést**. Karta **VÝSLEDKY** se zobrazí pod kartou **DOTAZ** a zobrazí informace o úloze. 
   
    Po dokončení dotazu se na kartě **DOTAZ** zobrazí výsledky operace. Zobrazí jedna tabulka s názvem **hivesampletable**. Tato vzorová tabulka Hive obsahuje všechny clustery HDInsight.
   
    ![Zobrazení Hive služby HDInsight](./media/apache-hadoop-linux-tutorial-get-started/hiveview.png "Editor dotazů zobrazení Hive služby HDInsight")

6. Opakujte kroky 4 a 5 a spusťte následující dotaz:
   
        SELECT * FROM hivesampletable;
   
7. Výsledky dotazu můžete také uložit. Vyberte tlačítko s nabídkou na pravé straně a určete, jestli chcete stáhnout výsledky jako soubor CSV nebo je uložit do účtu úložiště přidruženého ke clusteru.

    ![Uložení výsledku dotazu Hive](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-linux-hive-view-save-results.png "Uložení výsledku dotazu Hive")

Po dokončení úlohy Hive můžete [Exportovat výsledky do databáze Azure SQL nebo databáze systému SQL Server](apache-hadoop-use-sqoop-mac-linux.md), můžete také [zobrazit výsledky pomocí aplikace Excel](apache-hadoop-connect-excel-power-query.md). Další informace o používání Hive v HDInsight najdete v části [Použití Hive a HiveQL s Hadoop v HDInsight k analýze ukázkového souboru Apache log4j](hdinsight-use-hive.md).

## <a name="troubleshoot"></a>Řešení potíží

Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](../hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="clean-up-resources"></a>Vyčištění prostředků
Jakmile budete s článkem hotovi, můžete cluster odstranit. Pomocí HDInsight jsou vaše data uložena v Azure Storage, takže můžete clusteru bezpečně odstranit, pokud není používán. Za cluster služby HDInsight se účtují poplatky, i když se nepoužívá. Vzhledem k tomu, že poplatky za cluster představují několikanásobek poplatků za úložiště, dává ekonomický smysl odstraňovat clustery, které nejsou používány. 

> [!NOTE]
> Pokud *rovnou* pokračujete k dalšímu kurzu, ve kterém se dozvíte, jak spouštět operace ETL s využitím Hadoopu ve službě HDInsight, můžete cluster nechat spuštěný. To proto, že v kurzu musíte cluster Hadoop vytvořit znovu. Pokud ale nebudete hned pokračovat dalším kurzem, musíte teď cluster odstranit.
> 
> 

**Postup odstranění clusteru a/nebo výchozího účtu úložiště**

1. Vraťte se na kartu prohlížeče s webem Azure Portal. Měli byste být na stránce s přehledem clusteru. Pokud chcete odstranit jenom cluster, ale zachovat výchozí účet úložiště, vyberte **Odstranit**.

    ![Odstranění clusteru HDInsight](./media/apache-hadoop-linux-tutorial-get-started/hdinsight-delete-cluster.png "Odstranění clusteru HDInsight")

2. Pokud chcete odstranit cluster i výchozí účet úložiště, vyberte název skupiny prostředků (zvýrazněný na předchozím snímku obrazovky) a otevřete stránku skupiny prostředků.

3. Vyberte **Odstranit skupinu prostředků** a odstraňte skupinu prostředků obsahující cluster a výchozí účet úložiště. Upozorňujeme, že odstraněním skupiny prostředků odstraníte účet úložiště. Pokud chcete zachovat účet úložiště, zvolte odstranění samotného clusteru.

## <a name="next-steps"></a>Další kroky
V tomto článku jste zjistili, jak vytvořit cluster HDInsight se systémem Linux pomocí šablony Resource Manageru, a jak provádět základní dotazy Hive. V dalším článku se dozvíte, jak pomocí Hadoopu ve službě HDInsight provést operaci ETL (extrakce, transformace a načítání).

> [!div class="nextstepaction"]
>[Extrakce, transformace a načítání dat pomocí Apache Hivu ve službě HDInsight](../hdinsight-analyze-flight-delay-data-linux.md)

Pokud chcete začít pracovat s vlastními daty a potřebujete další informace o ukládání dat službou HDInsight nebo o tom, jak data do této služby nahrát, přečtěte si následující články:

* Informace o tom, jak HDInsight používá Azure Storage, najdete v tématu [Používání Azure Storage s HDInsight](../hdinsight-hadoop-use-blob-storage.md).
* Informace o tom, jak vytvořit HDInsight cluster s Data Lake Storage najdete v tématu [rychlý start: nastavení clusterů v HDInsight](../../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md)
* Informace o tom, jak nahrát data do služby HDInsight, najdete v tématu [Nahrání dat do služby HDInsight](../hdinsight-upload-data.md).

Další informace o analýze dat pomocí HDInsight najdete v následujících článcích:

* Další informace o používání Hivu se službou HDInsight, včetně postupu provádění dotazů Hivu ze sady Visual Studio, najdete v tématu [Použití Hivu se službou HDInsight](hdinsight-use-hive.md).
* Další informace o jazyce Pig používaném k transformaci dat najdete v tématu [Použití Pigu se službou HDInsight](hdinsight-use-pig.md).
* Další informace o MapReduce, způsobu psaní programů, které zpracovávají data v Hadoopu, najdete v tématu [Použití MapReduce se službou HDInsight](hdinsight-use-mapreduce.md).
* Další informace o použití nástrojů HDInsight pro Visual Studio k analýze dat na HDInsight najdete v části [Začněte používat nástroje Visual Studio Hadoop pro HDInsight](apache-hadoop-visual-studio-tools-get-started.md).
* Informace o použití nástrojů HDInsight pro VSCode k analýze dat na HDInsight najdete v části [Použití nástrojů HDInsight Tools pro Visual Studio Code](../hdinsight-for-vscode.md).


Pokud potřebujete další informace o vytváření a správě clusteru HDInsight, přečtěte si následující články:

* Další informace o správě clusteru HDInsight se systémem Linux najdete v části [Správa clusterů HDInsight pomocí Ambari](../hdinsight-hadoop-manage-ambari.md).
* Další informace o možnostech, které můžete vybrat při vytváření clusteru služby HDInsight, najdete v tématu [Vytváření HDInsight na Linuxu pomocí vlastních možností](../hdinsight-hadoop-provision-linux-clusters.md).


[1]: ../HDInsight/apache-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


