---
title: "Vyhledávání všech prostředků s Azure Log Analytics | Microsoft Docs"
description: "Tento článek popisuje, jak můžete dotazovat na prostředky z víc pracovních prostorů a app Insights aplikace v rámci vašeho předplatného."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2018
ms.author: magoedte
ms.openlocfilehash: 5485b1634013c73b58932aafa6e17d636558715d
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/24/2018
---
# <a name="perform-cross-resource-log-searches-in-log-analytics"></a>Vyhledávání prostředků mezi protokolu v analýzy protokolů  

Dříve s Azure Log Analytics může pouze analyzujete data přímo z aktuální pracovní prostor a je omezený možnost dotazu napříč více pracovní prostory definované v rámci vašeho předplatného.  Kromě toho může vyhledávat pouze položky telemetrie získané z vaší webové aplikace pomocí Application Insights přímo ve službě Application Insights, nebo ze sady Visual Studio.  To rovněž se data aplikací a výzvu k analýze nativně provozní společně.   

Teď se můžete dotazovat nejen napříč více analýzy protokolů pracovních prostorů, ale také data z konkrétní aplikaci Application Insights ve stejné skupině prostředků, jiné skupině prostředků nebo jiné předplatné. To poskytuje systémová zobrazení vaše data.  Můžete provést pouze tyto typy dotazů v [pokročilé portál](log-analytics-log-search-portals.md#advanced-analytics-portal), není v portálu Azure.  

## <a name="querying-across-log-analytics-workspaces-and-from-application-insights"></a>Dotazování napříč pracovních prostorů analýzy protokolů a z Application Insights
Chcete-li jiný pracovní prostor v dotazu, použijte [ *prostoru* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/workspace()) identifikátor a pro aplikace z Application Insights, použijte [ *aplikace* ](https://docs.loganalytics.io/docs/Language-Reference/Scope-functions/app())identifikátor.  

### <a name="identifying-workspace-resources"></a>Identifikační prostředky prostoru
Následující příklady ukazují dotazy napříč pracovních prostorů analýzy protokolů můžete vrátit souhrnnou počty podle jejich klasifikace z tabulky aktualizace z obou aktuální pracovní prostor potřebné aktualizace a jiné prostoru s názvem  *contosoretail it*. 

Identifikace pracovní prostor může být provedené z několika způsoby:

* Název prostředku - je popisný název pracovního prostoru, označovaných také jako *název komponenty*. 

    `workspace("contosoretail").Update | count`
 
    >[!NOTE]
    >Identifikace pracovního prostoru podle názvu předpokládá jedinečnosti ve všech předplatných přístupné. Pokud máte více aplikací se zadaným názvem, se dotaz nezdaří z důvodu nejednoznačnosti. V takovém případě musíte použít jeden z dalších identifikátorů.

* Kvalifikovaný název – je "úplný název" pracovního prostoru tvořený název odběru, skupinu prostředků a název součásti v tomto formátu: *Název_předplatného, resourceGroup nebo componentName*. 

    `workspace('contoso/contosoretail/development').requests | count `

    >[!NOTE]
    >Vzhledem k tomu, že nejsou jedinečné názvy předplatné Azure, tento identifikátor může být nejednoznačný. 
    >

* ID pracovního prostoru - ID pracovního prostoru je jedinečný, neměnné, identifikátor přiřazený k každém pracovním prostoru vyjádřené globálně jedinečný identifikátor (GUID).

    `workspace("b459b4u5-912x-46d5-9cb1-p43069212nb4").Update | count`

* Azure ID prostředku – definované Azure jedinečnou identitu pracovního prostoru. ID prostředku se používají, když název prostředku je nejednoznačný.  Pro pracovní prostory, není ve formátu: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. Pracovní prostory/OperationalInsights/componentName*.  

    Příklad:
    ``` 
    workspace("/subscriptions/e427519-5645-8x4e-1v67-3b84b59a1985/resourcegroups/ContosoAzureHQ/providers/Microsoft.OperationalInsights/workspaces/contosoretail").Event | count
    ```

### <a name="identifying-an-application"></a>Identifikace aplikace
Následující příklady vrátí souhrnnou počet požadavků na aplikaci s názvem přístup *fabrikamapp* ve službě Application Insights. 

Identifikace aplikace ve službě Application Insights můžete provést pomocí *app(Identifier)* výraz.  *Identifikátor* argument určuje aplikace pomocí jedné z následujících akcí:

* Název prostředku - je lidské čitelný název aplikace, označovaných také jako *název komponenty*.  

    `app("fabrikamapp")`

* Kvalifikovaný název – je "úplný název" aplikace skládá z název odběru, skupinu prostředků, název komponenty v tomto formátu: *Název_předplatného, resourceGroup nebo componentName*. 

    `app("AI-Prototype/Fabrikam/fabrikamapp").requests | count`

     >[!NOTE]
    >Vzhledem k tomu, že nejsou jedinečné názvy předplatné Azure, tento identifikátor může být nejednoznačný. 
    >

* ID – identifikátor GUID aplikace aplikace.

    `app("b459b4f6-912x-46d5-9cb1-b43069212ab4").requests | count`

* Azure ID prostředku - definované Azure jedinečnou identitu aplikace. ID prostředku se používají, když název prostředku je nejednoznačný. Formát je: */subscriptions/subscriptionId/resourcegroups/resourceGroup/providers/microsoft. OperationalInsights nebo součástí nebo componentName*.  

    Příklad:
    ```
    app("/subscriptions/b459b4f6-912x-46d5-9cb1-b43069212ab4/resourcegroups/Fabrikam/providers/microsoft.insights/components/fabrikamapp").requests | count
    ```

## <a name="next-steps"></a>Další postup

Zkontrolujte [analýzy protokolů protokolu vyhledávání odkaz](https://docs.loganalytics.io/docs/Language-Reference) zobrazíte všechny možnosti syntaxe dotazu, která je k dispozici v analýzy protokolů.    