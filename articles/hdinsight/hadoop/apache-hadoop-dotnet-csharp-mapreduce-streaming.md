---
title: Použití jazyka C# s MapReduce pro Hadoop v HDInsight – Azure
description: Další informace o použití jazyka C# k vytvoření řešení MapReduce s Hadoop v Azure HDInsight.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: jasonh
ms.openlocfilehash: 91558ae4fd592c7490aef0a0f2fd930e970f498c
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "43049059"
---
# <a name="use-c-with-mapreduce-streaming-on-hadoop-in-hdinsight"></a>Použití jazyka C# s MapReduce, streaming na platformě Hadoop v HDInsight

Další informace o použití jazyka C# k vytvoření řešení MapReduce v HDInsight.

> [!IMPORTANT]
> HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v tématu [Správa verzí komponenty HDInsight](../hdinsight-component-versioning.md).

Streamování Hadoop je nástroj, který umožňuje spouštět úlohy mapreduce je možné pomocí skriptu nebo spustitelného souboru. V tomto příkladu se používá .NET k implementaci mapovací a redukční funkci řešení aplikace word count.

## <a name="net-on-hdinsight"></a>.NET pro HDInsight

__HDInsight se systémem Linux__ clusterů použijte [Mono (https://mono-project.com) ](https://mono-project.com) ke spouštění aplikací .NET. Mono verze 4.2.1 je součástí HDInsight verze 3.6. Další informace o verzi Mono součástí HDInsight najdete v tématu [verzí komponenty HDInsight](../hdinsight-component-versioning.md). Pokud chcete použít konkrétní verzi nástroje Mono, najdete v článku [instalace nebo aktualizace Mono](../hdinsight-hadoop-install-mono.md) dokumentu.

Další informace o Mono kompatibilitu s verzí rozhraní .NET Framework najdete v tématu [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/).

## <a name="how-hadoop-streaming-works"></a>Jak funguje streamování Hadoop

Základní proces používají pro streamování v tomto dokumentu je následujícím způsobem:

1. Hadoop předá data mapper (v tomto příkladu mapper.exe) na STDIN.
2. Mapovač zpracuje data a generuje páry klíč/hodnota oddělený tabulátory do STDOUT.
3. Výstup je číst hadoop a potom předány redukční funkci (v tomto příkladu reducer.exe) na STDIN.
4. Redukční funkci přečte dvojice klíč/hodnota oddělený tabulátory, zpracuje data a pak vydá výsledek jako páry klíč/hodnota oddělený tabulátory StdOut.
5. Výstup je číst hadoop a zapisovat do výstupního adresáře.

Další informace o datových proudů, naleznete v tématu [streamování Hadoop](https://hadoop.apache.org/docs/r2.7.1/hadoop-streaming/HadoopStreaming.html).

## <a name="prerequisites"></a>Požadavky

* Důvěrně při psaní a vytváření kódu jazyka C#, který cílí na rozhraní .NET Framework 4.5. Kroky v tomto dokumentu pomocí sady Visual Studio 2017.

* Způsob, jak nahrát soubory .exe do clusteru. Kroky v tomto dokumentu pomocí nástrojů Data Lake pro Visual Studio k nahrání souborů do primárního úložiště pro cluster.

* Prostředí Azure PowerShell nebo klienta SSH.

* Hadoop v clusteru HDInsight. Další informace týkající se vytvoření clusteru najdete v tématu [vytvořit HDInsight cluster](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="create-the-mapper"></a>Vytvoření Mapovač

V sadě Visual Studio vytvořte nový __konzolovou aplikaci__ s názvem __Mapovač__. Pomocí následujícího kódu pro aplikace:

```csharp
using System;
using System.Text.RegularExpressions;

namespace mapper
{
    class Program
    {
        static void Main(string[] args)
        {
            string line;
            //Hadoop passes data to the mapper on STDIN
            while((line = Console.ReadLine()) != null)
            {
                // We only want words, so strip out punctuation, numbers, etc.
                var onlyText = Regex.Replace(line, @"\.|;|:|,|[0-9]|'", "");
                // Split at whitespace.
                var words = Regex.Matches(onlyText, @"[\w]+");
                // Loop over the words
                foreach(var word in words)
                {
                    //Emit tab-delimited key/value pairs.
                    //In this case, a word and a count of 1.
                    Console.WriteLine("{0}\t1",word);
                }
            }
        }
    }
}
```

Po vytvoření aplikace, sestavte ho vytvořit `/bin/Debug/mapper.exe` soubor v adresáři projektu.

## <a name="create-the-reducer"></a>Vytvořte redukční funkci

V sadě Visual Studio vytvořte nový __konzolovou aplikaci__ s názvem __redukční funkci__. Pomocí následujícího kódu pro aplikace:

```csharp
using System;
using System.Collections.Generic;

namespace reducer
{
    class Program
    {
        static void Main(string[] args)
        {
            //Dictionary for holding a count of words
            Dictionary<string, int> words = new Dictionary<string, int>();

            string line;
            //Read from STDIN
            while ((line = Console.ReadLine()) != null)
            {
                // Data from Hadoop is tab-delimited key/value pairs
                var sArr = line.Split('\t');
                // Get the word
                string word = sArr[0];
                // Get the count
                int count = Convert.ToInt32(sArr[1]);

                //Do we already have a count for the word?
                if(words.ContainsKey(word))
                {
                    //If so, increment the count
                    words[word] += count;
                } else
                {
                    //Add the key to the collection
                    words.Add(word, count);
                }
            }
            //Finally, emit each word and count
            foreach (var word in words)
            {
                //Emit tab-delimited key/value pairs.
                //In this case, a word and a count of 1.
                Console.WriteLine("{0}\t{1}", word.Key, word.Value);
            }
        }
    }
}
```

Po vytvoření aplikace, sestavte ho vytvořit `/bin/Debug/reducer.exe` soubor v adresáři projektu.

## <a name="upload-to-storage"></a>Nahrání do úložiště

1. V sadě Visual Studio, otevřete **Průzkumníka serveru**.

2. Rozbalte položku **Azure** a pak rozbalte **HDInsight**.

3. Pokud se zobrazí výzva, zadejte svoje přihlašovací údaje předplatného Azure a pak klikněte na tlačítko **Sign In**.

4. Rozbalte položku, kterou chcete nasadit tuto aplikaci do clusteru HDInsight. Položka s textem __(výchozí účet úložiště)__ je uvedena.

    ![Průzkumník serveru zobrazující účet úložiště pro cluster](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/storage.png)

    * Pokud tuto položku lze rozšířit, používáte __účet služby Azure Storage__ jako výchozí úložiště pro cluster. Chcete-li zobrazit soubory na výchozí úložiště pro cluster, rozbalte položku a potom dvakrát klikněte __(výchozí kontejner)__.

    * Pokud tuto položku nelze rozšířit, používáte __Azure Data Lake Store__ jako výchozího úložiště pro cluster. Chcete-li zobrazit soubory na výchozí úložiště pro cluster, dvakrát klikněte __(výchozí účet úložiště)__ položka.

5. Pokud chcete nahrát soubory .exe, použijte jednu z následujících metod:

    * Pokud používáte __účet služby Azure Storage__, klikněte na tlačítko Nahrát ikonu a potom vyhledejte **bin\debug** složku pro **Mapovač** projektu. Nakonec vyberte **mapper.exe** souboru a klikněte na tlačítko **Ok**.

        ![Nahrát ikonu](./media/apache-hadoop-dotnet-csharp-mapreduce-streaming/upload.png)
    
    * Pokud používáte __Azure Data Lake Store__, klikněte pravým tlačítkem na prázdnou oblast v seznamu souboru a pak vyberte __nahrát__. Nakonec vyberte **mapper.exe** souboru a klikněte na tlačítko **otevřít**.

    Jednou __mapper.exe__ nahrávání dokončí, opakujte proces nahrávání __reducer.exe__ souboru.

## <a name="run-a-job-using-an-ssh-session"></a>Spuštění úlohy: relace SSH pomocí

1. Pomocí SSH se připojte ke clusteru HDInsight. Další informace najdete v tématu [Použití SSH se službou HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

2. Pomocí jedné z následujících příkazů spusťte úlohu MapReduce:

    * Pokud používáte __Data Lake Store__ jako výchozí úložiště:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files adl:///mapper.exe,adl:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```
    
    * Pokud používáte __služby Azure Storage__ jako výchozí úložiště:

        ```bash
        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files wasb:///mapper.exe,wasb:///reducer.exe -mapper mapper.exe -reducer reducer.exe -input /example/data/gutenberg/davinci.txt -output /example/wordcountout
        ```

    Následující seznam popisuje, co dělá každý parametr:

    * `hadoop-streaming.jar`: Soubor jar, který obsahuje funkce datových proudů MapReduce.
    * `-files`: Přidá `mapper.exe` a `reducer.exe` soubory do této úlohy. `adl:///` Nebo `wasb:///` před každého souboru je cesta ke kořenové výchozí úložiště pro cluster.
    * `-mapper`: Určuje soubor, který implementuje Mapovač.
    * `-reducer`: Určuje soubor, který implementuje redukční funkci.
    * `-input`: Vstupní data.
    * `-output`: Výstupní adresář.

3. Po dokončení úlohy MapReduce, použijte následující postupy k zobrazení výsledků:

    ```bash
    hdfs dfs -text /example/wordcountout/part-00000
    ```

    Následující text je příkladem data vrácená tímto příkazem:

        you     1128
        young   38
        younger 1
        youngest        1
        your    338
        yours   4
        yourself        34
        yourselves      3
        youth   17

## <a name="run-a-job-using-powershell"></a>Spuštění úlohy: pomocí Powershellu

Pomocí následujícího skriptu prostředí PowerShell spustit úlohu MapReduce a stáhněte si výsledky.

[!code-powershell[main](../../../powershell_scripts/hdinsight/use-csharp-mapreduce/use-csharp-mapreduce.ps1?range=5-87)]

Tento skript vás vyzve k zadání název účtu přihlášení clusteru a heslo a současně název clusteru HDInsight. Po dokončení úlohy, výstup se stáhne do souboru s názvem `output.txt`. Následující text je příklad dat v `output.txt` souboru:

    you     1128
    young   38
    younger 1
    youngest        1
    your    338
    yours   4
    yourself        34
    yourselves      3
    youth   17

## <a name="next-steps"></a>Další postup

Další informace o používání MapReduce s HDInsight naleznete v tématu [použití MapReduce se službou HDInsight](hdinsight-use-mapreduce.md).

Informace o používání jazyka C# s Hivem a Pig, naleznete v tématu [pomocí jazyka C# uživatelem definované funkce s Hivem a Pig](apache-hadoop-hive-pig-udf-dotnet-csharp.md).

Informace o používání jazyka C# se Stormem v HDInsight najdete v tématu [vývoj topologií C# pro Storm v HDInsight](../storm/apache-storm-develop-csharp-visual-studio-topology.md).
