---
title: Analýza webových protokolů pomocí Azure Data Lake Analytics
description: Zjistěte, jak analýza webových protokolů pomocí Data Lake Analytics.
services: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: 3a196735-d0d9-4deb-ba68-c4b3f3be8403
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 12/05/2016
ms.openlocfilehash: 5d25aed196dd6b4ce92ae1cf18e556d1c32295d3
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "43051963"
---
# <a name="analyze-website-logs-using-azure-data-lake-analytics"></a>Analýza webových protokolů pomocí Azure Data Lake Analytics
Zjistěte, jak analýza webových protokolů pomocí Data Lake Analytics, zvláště na hledání, které odkazující servery nastaly chyby při pokusu o přejděte na webovou stránku.

## <a name="prerequisites"></a>Požadavky
* **Visual Studio 2015 nebo Visual Studio 2013**.
* **[Nástroje Data Lake pro Visual Studio](http://aka.ms/adltoolsvs)**.

    Po instalaci nástrojů Data Lake pro Visual Studio se zobrazí **Data Lake** položky v **nástroje** nabídky v sadě Visual Studio:

    ![Nabídka U-SQL Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)
* **Základní znalosti o Data Lake Analytics a Data Lake Tools pro Visual Studio**. Abyste mohli začít, najdete v článku:

  * [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* **Účet Data Lake Analytics.**  Zobrazit [vytvoření účtu Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
* **Nainstalujte ukázková data.** Na webu Azure Portal otevření účtu Data Lake Analytics a klikněte na tlačítko **ukázkové skripty** v nabídce vlevo klikněte **kopírovat ukázková Data**. 

## <a name="connect-to-azure"></a>Připojení k Azure
Předtím, než můžete vytvářet a testovat skripty U-SQL, musíte nejdřív připojit k Azure.

**Postup připojení ke službě Data Lake Analytics**

1. Otevřete sadu Visual Studio.
2. Klikněte na tlačítko **Data Lake > Možnosti a nastavení**.
3. Klikněte na tlačítko **Sign In**, nebo **změnit uživatele** Pokud někdo přihlásil a postupujte podle pokynů.
4. Klikněte na tlačítko **OK** zavřete dialogové okno Možnosti a nastavení.

**Procházet vaše účty Data Lake Analytics**

1. Ze sady Visual Studio, otevřete **Průzkumníka serveru** podle stiskněte **CTRL + ALT + S**.
2. V **Průzkumníku serveru** rozbalte položku **Azure** a pak rozbalte položku **Data Lake Analytics**. Zobrazí se seznam účtů Data Lake Analytics, pokud nějaké máte. Nelze vytvořit účty Data Lake Analytics ze sady studio. Pokud chcete vytvořit účet, informace najdete v tématu [Začínáme s Azure Data Lake Analytics pomocí Portálu Azure](data-lake-analytics-get-started-portal.md) nebo [Začínáme s Azure Data Lake Analytics pomocí Azure PowerShell](data-lake-analytics-get-started-powershell.md).

## <a name="develop-u-sql-application"></a>Vývoj aplikace v U-SQL
Aplikace v jazyce U-SQL se především skript U-SQL. Další informace o U-SQL najdete v tématu [Začínáme s jazykem U-SQL](data-lake-analytics-u-sql-get-started.md).

Uživatelem definované operátory sčítání můžete přidat do aplikace.  Další informace najdete v tématu [vývoj U-SQL uživatelem definované operátory pro úlohy Data Lake Analytics](data-lake-analytics-u-sql-develop-user-defined-operators.md).

**Postup vytvoření a odeslání úlohy Data Lake Analytics**

1. Klikněte na tlačítko **soubor > Nový > projekt**.
2. Vyberte typ projekt v U-SQL.

    ![Nový projekt U-SQL Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. Klikněte na **OK**. Visual studio vytvoří řešení se souborem Script.usql.
4. Zadejte následující skript do souboru Script.usql:

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            DISTRIBUTED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    U-SQL najdete v tématu [Začínáme s jazykem Data Lake Analytics U-SQL](data-lake-analytics-u-sql-get-started.md).    
5. Přidejte do projektu nový skript U-SQL a zadejte následující příkaz:

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();
6. Přepněte zpět na první skript U-SQL a vedle položky **odeslat** tlačítko, zadejte svůj účet Analytics.
7. V **Průzkumníku řešení** klikněte pravým tlačítkem na položku **Script.usql** a pak klikněte na možnost **Sestavit skript**. Ověřte výsledek v podokně výstup.
8. V **Průzkumníku řešení** klikněte pravým tlačítkem na položku **Script.usql** a pak klikněte na položku **Odeslat skript**.
9. Ověřte, **účet Analytics** je ten, ve kterém chcete spustit úlohu a potom klikněte na tlačítko **odeslat**. Jakmile je odeslání hotové, v okně Výsledky nástrojů Data Lake pro Visual Studio jsou dostupné výsledky odeslání a odkaz na úlohu.
10. Počkejte, dokud je úloha dokončena úspěšně.  Pokud úloha se nezdařila, pravděpodobně neobsahuje zdrojového souboru.  Podrobnosti naleznete v části požadavků tohoto kurzu. Další informace o odstraňování potíží, najdete v části [monitorování úloh Azure Data Lake Analytics a odstraňování potíží](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    Po dokončení úlohy se zobrazí následující obrazovka:

    ![data lake analytics analyzovat webové protokoly webových protokolů](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)
11. Teď zopakujte kroky 7 až 10 pro **Script1.usql**.

**Postup zobrazení výstupu úlohy**

1. V **Průzkumníku serveru** rozbalte položku **Azure**, rozbalte položku **Data Lake Analytics**, rozbalte účet Data Lake Analytics, rozbalte položku **Účty Storage**, klikněte pravým tlačítkem na výchozí účet Data Lake Storage a potom klikněte na položku **Explorer**.
2. Dvakrát klikněte na panel **ukázky** otevřete složku a potom dvakrát klikněte na panel **výstupy**.
3. Dvakrát klikněte na panel **UnsuccessfulResponsees.log**.
4. Můžete také dvakrát kliknout na výstupní soubor v zobrazení grafu úlohy aby bylo možné přejít přímo do výstupu.

## <a name="see-also"></a>Další informace najdete v tématech
Pokud chcete začít s Data Lake Analytics pomocí různých nástrojů, projděte si témata:

* [Začínáme se službou Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-get-started-portal.md)
* [Začínáme se službou Data Lake Analytics pomocí Azure PowerShellu](data-lake-analytics-get-started-powershell.md)
* [Začínáme se službou Data Lake Analytics pomocí sady .NET SDK](data-lake-analytics-get-started-net-sdk.md)
