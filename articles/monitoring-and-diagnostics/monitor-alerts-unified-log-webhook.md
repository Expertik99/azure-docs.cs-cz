---
title: Akce Webhooku pro upozornění protokolů ve výstrahách Azure
description: Tento článek popisuje, jak na pravidla upozornění protokolů pomocí log analytics nebo application insights zařadí data jako HTTP webhook a podrobnosti o různá přizpůsobení je to možné.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: f20e102ee1d100ea02da53fe460b56f8f8390418
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426689"
---
# <a name="webhook-actions-for-log-alert-rules"></a>Akce Webhooku pro pravidla upozornění protokolů
Při [v Azure se vytvoří výstraha ](monitor-alerts-unified-usage.md), máte možnost [konfigurace pomocí skupin akcí](monitoring-action-groups.md) provést jednu nebo více akcí.  Tento článek popisuje různé webhooku akce, které jsou k dispozici a podrobnosti o tom, jak nakonfigurovat vlastní webhooku založenými na JSON.


## <a name="webhook-actions"></a>Akce Webhooku

Akce Webhooku vám umožňují spuštění externího procesu prostřednictvím jednoho požadavku HTTP POST.  Volané služby by měl podpora webhooků a zjistit, jak pomocí jakékoli datové části přijme.   Příklady použití webhooku v reakci na oznámení se odesílá zprávu [Slack](http://slack.com) nebo vytváření incidentu v [PagerDuty](http://pagerduty.com/).  

Akce Webhooku vyžadovat vlastnosti v následující tabulce:

| Vlastnost | Popis |
|:--- |:--- |
| Adresa URL webhooku |Adresa URL webhooku. |
| Vlastní datová část JSON |Vlastní datová část k odesílání pomocí webhooku, pokud tato možnost zvolená během vytváření výstrahy. Podrobnosti o adrese [spravovat upozornění pomocí Azure Alerts ](monitor-alerts-unified-usage.md) |

> [!NOTE]
> Otestovat Webhook tlačítko vedle *zahrnout vlastní datovou část JSON pro webhook* možnost pro výstrahy protokolu, se aktivuje fiktivní volání otestovat adresu URL webhooku. Skutečná data a zástupce schématu JSON pro upozornění protokolu neobsahuje. 

Webhooky zahrnují adresu URL a datové části je ve formátu JSON, který se data odeslaná do externí služby.  Ve výchozím nastavení, datová část obsahuje hodnoty v následující tabulce: je možné nahradit tuto datovou část vlastní sadou vlastní.  V takovém případě můžete použít proměnné v tabulce pro každý z parametrů zahrnout vlastní datovou část jejich hodnoty.


| Parametr | Proměnná | Popis |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Název pravidla výstrahy. |
| Severity |#severity |Závažnost nastavit upozornění aktivace protokolu. |
| AlertThresholdOperator |#thresholdoperator |Operátor prahové hodnoty pro pravidla upozornění.  *Větší než* nebo *menší než*. |
| AlertThresholdValue |#thresholdvalue |Prahová hodnota pro pravidla upozornění. |
| LinkToSearchResults |#linktosearchresults |Odkaz na portál Analytics, která vrací záznamy z dotazu, která upozornění vytvořila. |
| ResultCount |#searchresultcount |Počet záznamů ve výsledcích hledání. |
| Čas ukončení Interval vyhledávání |#searchintervalendtimeutc |Koncový čas pro dotaz ve standardu UTC, format - mm/dd/rrrr hh: mm: ss dop. / odp. |
| Interval vyhledávání |#searchinterval |Časový interval pro upozornění pravidla, formát: hh: mm:. |
| StartTime Interval vyhledávání |#searchintervalstarttimeutc |Spuštění dotazu ve standardu UTC, format - mm/dd/rrrr hh: mm: ss dop. / odp. 
| SearchQuery |#searchquery |Vyhledávací dotaz log použitý pravidlem upozornění. |
| SearchResults |"IncludeSearchResults": true|Záznamů vrácených dotazem jako tabulka JSON, omezená na prvních 1 000 záznamů. Pokud "IncludeSearchResults": true přidá vlastní definici JSON webhook jako vlastnost nejvyšší úrovně. |
| ID pracovního prostoru |#workspaceid |ID pracovního prostoru Log Analytics. |
| ID aplikace |#applicationid |ID Application Insights aplikaci. |
| ID předplatného |#subscriptionid |ID vašeho předplatného Azure používat s Application Insights. 

> [!NOTE]
> LinkToSearchResults předá parametrů, jako je čas SearchQuery, StartTime Interval hledání & Konec intervalu vyhledávání v adrese URL na webu Azure portal pro zobrazení v části Analytics. Azure portal má omezení velikosti URI 2000 znaků. přibl a otevře se hodnoty parametrů překročení těchto omezení. Uživatele můžete ručně zadat podrobnosti a zobrazit výsledky analýzy portálu nebo pomocí [REST API pro Application Insights Analytics](https://dev.applicationinsights.io/documentation/Using-the-API) nebo [rozhraní API REST služby Log Analytics](https://dev.loganalytics.io/reference) k načtení výsledků prostřednictvím kódu programu 

Například můžete například zadat následující vlastní datovou část, která obsahuje jeden parametr s názvem *text*.  Služba, která volá tento webhook by byl očekáván tento parametr.

```json

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }
```
Tato datová část příklad by přeložit na podobné následujícímu při odeslání k webhooku.

```json
    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }
```
Všechny proměnné v webhooku vlastní musí zadat v rámci skříň JSON jako "#searchinterval", výsledná webhooku má také data proměnných ve skříni, jako je "00: 05:00".

Do výsledků hledání zahrnout vlastní datovou část, ujistěte se, že **IncudeSearchResults** je nastaven jako vlastnost v datové části json nejvyšší úrovně. 

## <a name="sample-payloads"></a>Ukázkové datové části
Tato část ukazuje ukázkovou datovou část webhooku pro upozornění protokolů, včetně při datové části je pevná a při jeho vlastní.

> [!NOTE]
> K zajištění zpětné kompatibility, datová část standardní webhooku pro výstrahy pomocí Azure Log Analytics je stejný jako [správu upozornění Log Analytics](../log-analytics/log-analytics-alerts-creating.md). Ale pro výstrahy protokolu pomocí [Application Insights](../application-insights/app-insights-analytics.md), datová část webhooku standardní podle schématu skupiny akcí.

### <a name="standard-webhook-for-log-alerts"></a>Standardní Webhooku pro výstrahy protokolu 
Oba tyto příklady uvedli fiktivní datovou část s pouze dvěma sloupci a dva řádky.

#### <a name="log-alert-for-azure-log-analytics"></a>Upozornění protokolu pro Azure Log Analytics
Následuje ukázkovou datovou část pro akce webhooku standardní *bez možnosti vlastní Json* se používají pro upozornění na základě analýzy protokolů.

```json
{
    "WorkspaceId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Warning"
 }
 ```   

#### <a name="log-alert-for-azure-application-insights"></a>Upozornění protokolu pro službu Azure Application Insights
Následuje ukázkovou datovou část pro standardní webhook *bez možnosti vlastní Json* pro application insights výstrahy založené na protokolu-.
    
```json
{
    "schemaId":"Microsoft.Insights/LogAlert","data":
    { 
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://analytics.applicationinsights.io/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Error",
    "ApplicationId": "123123f0-01d3-12ab-123f-abc1ab01c0a1"
    }
}
```

#### <a name="log-alert-with-custom-json-payload"></a>Upozornění protokolu s vlastní datovou část JSON
Například pokud chcete vytvořit vlastní datovou část, která zahrnuje pouze název výstrahy a výsledky hledání, můžete použít následující: 

```json
    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }
```

Následuje ukázkovou datovou část pro akce webhooku vlastní žádné upozornění protokolu.
    
```json
    {
    "alertname":"AcmeRule","IncludeSearchResults":true,
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    }
```


## <a name="next-steps"></a>Další postup
- Další informace o [upozornění protokolů ve výstrahách Azure ](monitor-alerts-unified-log.md)
- Vytvoření a správa [skupiny akcí v Azure](monitoring-action-groups.md)
- Další informace o [Application Insights](../application-insights/app-insights-analytics.md)
- Další informace o [Log Analytics](../log-analytics/log-analytics-overview.md). 