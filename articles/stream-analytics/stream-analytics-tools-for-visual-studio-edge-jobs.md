---
title: Hraniční úlohy v Azure Stream Analytics tools for Visual Studio
description: Tento článek popisuje, jak vytvářet, ladit a vytvoření úlohy Stream Analytics Edge pomocí Stream Analytics tools pro Visual Studio.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/13/2018
ms.openlocfilehash: 5dc90a1334b525c02be3eae2985900ab07cf2e05
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696244"
---
# <a name="develop-stream-analytics-edge-jobs-using-visual-studio-tools"></a>Vývoj úloh Stream Analytics Edge pomocí nástrojů sady Visual Studio

V tomto kurzu se dozvíte, jak používat nástroje Stream Analytics pro Visual Studio k vytváření, ladění a vytvoření úlohy Stream Analytics Edge. Po vytvoření a testovací úlohy, můžete přejít na web Azure Portal k nasazení do zařízení. 

## <a name="prerequisites"></a>Požadavky

Budete potřebovat k dokončení tohoto kurzu následující požadavky:

* Nainstalujte [Visual Studio 2017](https://www.visualstudio.com/downloads/), [Visual Studio 2015](https://www.visualstudio.com/vs/older-downloads/), nebo [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=45326). Podporují se edice Enterprise (Ultimate nebo Premium), Professional a Community. Edice Express se nepodporuje.  

* Postupujte podle [pokyny k instalaci](stream-analytics-tools-for-visual-studio-edge-jobs.md) instalace nástroje Stream Analytics pro Visual Studio.
 
## <a name="create-a-stream-analytics-edge-project"></a>Vytvořte projekt Stream Analytics Edge 

Ze sady Visual Studio, vyberte **souboru** > **nový** > **projektu**. Přejděte **šablony** seznamu na levé straně > rozbalte **Azure Stream Analytics** > **Stream Analytics Edge**  >   **Azure Stream Analytics hraniční aplikace**. Zadejte název projektu a vyberte název, umístění a řešení **OK**.

![Nový projekt Edge](./media/stream-analytics-tools-for-visual-studio-edge-jobs/new-edge-project.png)

Jakmile se vytvoří projekt, přejděte na **Průzkumníka řešení** Chcete-li zobrazit hierarchii složek.

![Zobrazení Průzkumníka řešení](./media/stream-analytics-tools-for-visual-studio-edge-jobs/edge-project-in-solution-explorer.png)

 
## <a name="choose-the-correct-subscription"></a>Vyberte správné předplatné

1. Z Visual Studia **zobrazení** nabídce vyberte možnost **Průzkumníka serveru**.  

2. Klikněte pravým tlačítkem na **Azure** > vyberte **připojit k předplatnému Microsoft Azure** > a potom se přihlaste pomocí svého účtu Azure.

## <a name="define-inputs"></a>Definování vstupy

1. Z **Průzkumníka řešení**, rozbalte **vstupy** uzlu by se měla zobrazit vstupní hodnota s názvem **EdgeInput.json**. Dvakrát klikněte a zobrazte její nastavení.  

2. Ujistěte se, že typ zdroje je nastavený na **datový Stream** > zdroj je nastavena na **Centrum Edge** > nastavte formát serializace události na **Json** > a kódování je nastavená na **UTF8**. Volitelně můžete přejmenovat **vstupní Alias**, jako je v tomto příkladu Ponecháme. V případě, že přejmenujete vstupní alias, použijte název, který jste zadali při definování dotazu. Vyberte **Uložit** a nastavení se uloží.  
   ![Konfigurace vstupu](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-input-configuration.png)
 


## <a name="define-outputs"></a>Definování výstupy

1. Z **Průzkumníka řešení**, rozbalte **výstupy** uzlu byste měli vidět výstup s názvem **EdgeOutput.json**. Dvakrát klikněte a zobrazte její nastavení.  

2. Zkontrolujte, že jímky je nastavený vyberte **centrum Edge** > nastavte formát serializace události na **Json** > a kódování je nastavená na **UTF8** > a formátu je nastavená na  **Pole**. Volitelně můžete přejmenovat **Alias pro výstup**, jako je v tomto příkladu Ponecháme. V případě, že přejmenujete alias pro výstup, použijte název, který jste zadali při definování dotazu. Vyberte **Uložit** a nastavení se uloží. 
   ![Konfigurace výstupu](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-output-configuration.png)
 
## <a name="define-the-transformation-query"></a>Definice transformačního dotazu

Nasazení v prostředích hraniční úlohy Stream Analytics podporují většinu [dotazovací jazyk Stream Analytics odkaz](https://msdn.microsoft.com/azure/stream-analytics/reference/stream-analytics-query-language-reference?f=255&MSPPError=-2147217396), ale pro hraniční úlohy se zatím nepodporují následující operace: 


|**Kategorie**  | **Příkaz**  |
|---------|---------|
|Geoprostorové operátory |<ul><li>CreatePoint</li><li>: CreatePolygon</li><li>CreateLineString</li><li>ST_DISTANCE</li><li>ST_WITHIN</li><li>ST_OVERLAPS</li><li>ST_INTERSECTS</li></ul> |
|Ostatní operátory | <ul><li>ODDÍL</li><li>ČASOVÉ RAZÍTKO PODLE PŘES</li><li>DISTINCT</li><li>Parametr výrazu v COUNT – operátor</li><li>Úrovni mikrosekund v funkce DATE a TIME</li><li>UDA JavaScriptu (Tato funkce je stále ve verzi preview pro úlohy nasazené v cloudu)</li></ul>   |

Hraniční úlohy vytvořené na portálu kompilátor automaticky upozornění, pokud nepoužíváte operátor podporované.

Ve Visual Studio, definovat následující transformační dotaz v editoru dotazů (**script.asaql souboru**)

```sql
SELECT * INTO EdgeOutput
FROM EdgeInput 
```

## <a name="test-the-job-locally"></a>Testovací úloha místně

Otestujte dotaz místně, byste měli nahrávat ukázková data. Stáhněte si registrační data z můžete získat ukázková data [úložiště GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/Registration.json) a uložte ho do místního počítače. 

1. K nahrání ukázkových dat > klikněte pravým tlačítkem na **EdgeInput.json** soubor > zvolte **přidat místní vstup**  

2. V automaticky otevíraném okně > **Procházet** ukázková data z místní cesta > vyberte **Uložit**.
   ![Konfigurace místního vstupu](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-local-input-configuration.png)
 
3. Soubor s názvem **local_EdgeInput.json** je automaticky přidán do složky vstupy.  
4. můžete ji spustit místně, nebo odešlete do Azure. Otestujte dotaz > vyberte **spustit místně**.  
   ![Možnosti spuštění](./media/stream-analytics-tools-for-visual-studio-edge-jobs/run-options.png)
 
5. V okně příkazového řádku zobrazí stav úlohy. Po úspěšném spuštění úlohy, vytvoří složku, která vypadá jako "2018-02-23-11-31-42" ve složce vašeho projektu "2015\Projects\MyASAEdgejob\MyASAEdgejob\ASALocalRun\2018-02-23-11-31-42 sady Visual Studio". Přejděte do umístění složky k zobrazení výsledků v místním adresáři:

   Můžete také přihlásit k webu Azure portal a ověřte, že úloha je vytvořena. 

   ![Složka výsledků](./media/stream-analytics-tools-for-visual-studio-edge-jobs/result-folder.png)

## <a name="submit-the-job-to-azure"></a>Odeslat úlohu do Azure

1. Před odesláním úlohy do Azure, musíte se připojit k předplatnému Azure. Otevřít **Průzkumníka serveru** > klikněte pravým tlačítkem na **Azure** > **připojit k předplatnému Microsoft Azure** > Přihlaste se ke svému předplatnému Azure.  

2. Chcete-li odeslat úlohu do Azure, přejděte do editoru dotazů > vyberte **odeslat do Azure**.  

3. Otevře se automaticky otevírané okno, kde můžete aktualizovat existující hraniční úlohy nebo vytvořte novou. Při aktualizaci existující úlohy nahradí všechny úlohy konfigurace, v tomto scénáři, kterou budete publikovat nové úlohy. Vyberte **vytvořit novou úlohu Azure Stream Analytics** > zadejte název pro vaši úlohu něco jako **MyASAEdgeJob** > zvolte požadovaný **předplatné**, **Skupiny prostředků**, a **umístění** > vyberte **odeslat**.

   ![Odeslat do Azure](./media/stream-analytics-tools-for-visual-studio-edge-jobs/submit-to-azure.png)
 
   Teď se vytvořil úlohu Stream Analytics Edge, můžete se podívat do [spouštění úloh ve službě IoT Edge kurzu](stream-analytics-edge.md) se naučíte nasadit do zařízení. 

## <a name="manage-the-job"></a>Spravovat úlohy 

Můžete zobrazit stav úlohy a diagram úloh v Průzkumníku serveru. Z **Průzkumníka serveru** > **Stream Analytics** > rozšířit předplatné a skupinu prostředků, kam jste nasadili hraniční úloha > můžete zobrazit MyASAEdgejob se stavem  **Vytvoření**. Rozbalte uzel projektu a dvakrát klikněte na něj a otevřete zobrazení úloh.

![Možnosti Průzkumníka serveru](./media/stream-analytics-tools-for-visual-studio-edge-jobs/server-explorer-options.png)
 
V okně zobrazení úlohy umožňuje operace, jako jsou aktualizace úlohy odstranění úlohy, otevřete úlohu z portálu Azure atd.

![Diagram úloh a další možnosti](./media/stream-analytics-tools-for-visual-studio-edge-jobs/job-diagram-and-other-options.png) 

## <a name="next-steps"></a>Další postup

* [Další informace o Azure Iot Edge](../iot-edge/about-iot-edge.md)
* [Azure Stream Analytics na hraničních zařízeních IoT kurz](../iot-edge/tutorial-deploy-stream-analytics.md)
* [Odeslat zpětnou vazbu týmu pomocí tohoto průzkumu](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2czagZ-i_9Cg6NhAZlH9ypUMjNEM0RDVU9CVTBQWDdYTlk0UDNTTFdUTC4u) 
