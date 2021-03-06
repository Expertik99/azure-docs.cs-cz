---
title: Postup přidání referenční sady dat do prostředí Azure Time Series Insights
description: Tento článek popisuje postup přidání referenční sady dat pro rozšíření dat ve vašem prostředí Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 02/15/2018
ms.openlocfilehash: 2cf9c8baf715acf0eef36d640c22f355435c9ca5
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/08/2018
ms.locfileid: "39631040"
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-azure-portal"></a>Vytvoření referenční sady dat pro prostředí Time Series Insights pomocí webu Azure portal

Tento článek popisuje postup přidání referenční sady dat do prostředí Azure Time Series Insights. Referenční data jsou užitečné pro připojení k vašim datům zdroje k posílení hodnoty.

Referenční sady dat je kolekce položek, které rozšiřují události ze zdroje událostí. Modul příchozího přenosu dat čas Series Insights spojí každé události ze zdroje událostí s odpovídající řádek dat v referenční sadě dat. Tato rozšířená událost je pak k dispozici pro dotaz. Toto připojení je založené na sloupců primárního klíče, které jsou definované v referenční sadě dat.

Referenční data není připojený zpětně. To znamená, že pouze aktuální a budoucí příchozího přenosu dat je odpovídající a připojený k sady referenční data, jakmile byl nakonfigurován a nahráli.

## <a name="add-a-reference-data-set"></a>Přidání referenční sady dat

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).

2. Vyhledejte existující prostředí Time Series Insights. Klikněte na tlačítko **všechny prostředky** v nabídce na levé straně webu Azure portal. Vyberte vaše prostředí Time Series Insights.

3. Vyberte **přehled** stránky. Vyhledejte **URL Průzkumníka Time Series Insights** a otevřete odkaz.  

   Zobrazte v Průzkumníku TSI prostředí.

4. Rozšiřte výběr prostředí v Průzkumníku TSI. Zvolte aktivní prostředí. Vyberte ikonu referenční data v pravém horním rohu na stránce explorer.

   ![Přidejte referenční data](media/add-reference-data-set/add_reference_data.png)

5. Vyberte **+ přidat datovou sadu** tlačítko začnete přidávat nové datové sady.

   ![Přidat sadu dat](media/add-reference-data-set/add_data_set.png)

6. Na **nový odkaz datové sady** stránky, vyberte formát data: 
   - Zvolte **sdíleného svazku clusteru** pro data oddělených čárkami. První řádek je považován za řádek záhlaví. 
   - Zvolte **pole JSON** pro javascript object notation (JSON) ve formátu data.

   ![Zvolte formát data.](media/add-reference-data-set/add_data.png)

7. Zadejte data, pomocí jedné ze dvou způsobů:
   - Vložte data do textového editoru. Vyberte **Parsovat referenční data** tlačítko.
   - Vyberte **zvolit soubor** tlačítko pro přidání dat z místního textového souboru. 

   Například vložit data ve formátu CSV: ![data vložit sdíleného svazku clusteru](media/add-reference-data-set/csv_data_pasted.png)

   Například vložit data pole JSON: ![dat vložte JSON](media/add-reference-data-set/json_data_pasted.png)

   Pokud dojde k chybě při analýze hodnoty dat, chyba se zobrazí červeně v dolní části stránky, jako například `CSV parsing error, no rows extracted`.

8. Jakmile se úspěšně analyzovat data, datové mřížce se zobrazuje zobrazení sloupce a řádky, které představují data.  Zobrazit datovou mřížku zajistit správnost.

   ![Přidejte referenční data](media/add-reference-data-set/parse_data.png)

9. Projděte si jednotlivé sloupce do viz předpokládá, že datový typ a změnit datový typ v případě potřeby.  Vyberte datový typ symbol v záhlaví sloupce: ** # ** pro double (číselná data), **T | F** pro logickou hodnotu, nebo **Abc** řetězce.

   ![Vyberte datové typy na záhlaví sloupce.](media/add-reference-data-set/choose_datatypes.png)

10. Přejmenujte záhlaví sloupců v případě potřeby. Název klíčového sloupce je potřebné pro připojení k příslušné vlastnosti ve zdroji událostí. Ujistěte se, že referenční názvy klíčových sloupců dat se přesně shodovat název události k příchozí data, včetně rozlišování. Názvy neklíčových sloupců se používají k posílení příchozích dat s odpovídajícími hodnotami referenční data.

11. Klikněte na tlačítko **přidejte řádek** nebo **přidat sloupec** přidat více hodnot dat odkaz, podle potřeby.

12. Zadejte hodnotu do **filtrovat řádky... ** pole, které chcete zkontrolovat konkrétní řádky podle potřeby. Filtr je užitečné při data, ale není použita, když data nahrávají.
 
13. Název datové sady, vyplněním **název datové sady** pole nad datové mřížce.

   ![Název datové sady.](media/add-reference-data-set/name_reference_dataset.png)

14. Zadejte **primární klíč** sloupců v datové sadě, tak, že vyberete rozevírací nabídky výše datové mřížce.

   ![Vyberte sloupce klíče.](media/add-reference-data-set/set_primary_key.png)

   Volitelně můžete vybrat ** + ** tlačítko pro přidání sekundární klíčový sloupec jako složený primární klíč. Pokud je potřeba zrušit výběr, zvolte prázdnou hodnotu z rozevíracího seznamu k odebrání sekundární klíč.

15.  Pokud chcete nahrát data, vyberte **nahrát řádky** tlačítko.

   ![Odeslat](media/add-reference-data-set/upload_rows.png)

   Na stránce potvrdí dokončené nahrávání a zobrazení zprávy **byl úspěšně nahrán datovou sadu**.

## <a name="next-steps"></a>Další postup
* [Spravujte referenční data](time-series-insights-manage-reference-data-csharp.md) prostřednictvím kódu programu.
* Úplnou referenční dokumentaci k rozhraní API najdete v dokumentu [Rozhraní API referenčních dat](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api).
