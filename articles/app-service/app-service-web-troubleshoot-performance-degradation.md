---
title: Zpomalení výkonu webových aplikací ve službě App Service | Dokumentace Microsoftu
description: Tento článek pomůže při řešení problémů s výkonem pomalých webových aplikací ve službě Azure App Service.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: erikre
editor: ''
tags: top-support-issue
keywords: výkonu webových aplikací, pomalé aplikace, aplikace je pomalá
ms.assetid: b8783c10-3a4a-4dd6-af8c-856baafbdde5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: cephalin
ms.openlocfilehash: 0f053c0293488f19adcd058f1ba8304d8d238738
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/28/2018
ms.locfileid: "43128302"
---
# <a name="troubleshoot-slow-web-app-performance-issues-in-azure-app-service"></a>Řešení problémů s výkonem pomalých webových aplikací ve službě Azure App Service
V tomto článku vám pomůžou s řešením problémů s výkonem pomalých webových aplikací v [služby Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).

Pokud potřebujete další nápovědu v libovolném bodě v tomto článku, můžete se obrátit odborníků na Azure na [MSDN Azure a Stack Overflow fóra](https://azure.microsoft.com/support/forums/). Alternativně můžete také soubor incidentu podpory Azure. Přejděte [web podpory Azure](https://azure.microsoft.com/support/options/) a klikněte na **získat podporu**.

## <a name="symptom"></a>Příznak
Při procházení webové aplikace, načtení stránek pomalu a někdy vypršení časového limitu.

## <a name="cause"></a>Příčina
Tento problém je často způsobeno úrovně problémů v aplikacích, jako například:

* síťové požadavky trvá příliš dlouho
* aplikace kódu nebo databáze dotazů se neefektivní
* aplikace s použitím vysoké paměti a procesoru
* aplikace k chybám z důvodu výjimky

## <a name="troubleshooting-steps"></a>Postup při řešení potíží
Řešení potíží je možné rozdělit do tří odlišných úkolech v postupném pořadí:

1. [Sledovat a monitorovat chování aplikace](#observe)
2. [Shromažďování dat](#collect)
3. [Tyto problémy zmírnit](#mitigate)

[App Service Web Apps](app-service-web-overview.md) nabízí různé možnosti v každém kroku.

<a name="observe" />

### <a name="1-observe-and-monitor-application-behavior"></a>1. Sledovat a monitorovat chování aplikace
#### <a name="track-service-health"></a>Sledování stavu služby
Microsoft Azure publicizes pokaždé, když je služba došlo k přerušení nebo výkonu snížení. Můžete sledovat stav služby na [webu Azure portal](https://portal.azure.com/). Další informace najdete v tématu [sledování stavu služby](../monitoring-and-diagnostics/insights-service-health.md).

#### <a name="monitor-your-web-app"></a>Monitorování webové aplikace
Tato možnost umožňuje zjistit, pokud vaše aplikace má nějaké problémy. V okně webové aplikace, klikněte na tlačítko **požadavky a chyby** dlaždici. **Metrika** okno se zobrazí všechny metriky můžete přidat.

Některé metriky, které můžete chtít monitorovat webové aplikace

* Průměrná pracovní sada paměti
* Průměrná doba odezvy
* Čas procesoru
* Pracovní sada paměti
* Požadavky

![monitorování výkonu webových aplikací](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

Další informace naleznete v tématu:

* [Monitorovat můžete webové aplikace ve službě Azure App Service](web-sites-monitor.md)
* [Zobrazování oznámení o výstrahách](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

#### <a name="monitor-web-endpoint-status"></a>Stav koncového bodu monitorování webu
Pokud používáte vaší webové aplikace **standardní** cenovou úroveň, Web Apps vám umožní monitorovat dva koncové body služby ze tří zeměpisné umístění.

Monitorování koncových bodů nakonfiguruje webové testy z geograficky distribuované umístění, které testují doba odezvy a doby provozu z webové adresy URL. Test provede operaci HTTP GET na adresu URL webové k určení doby odezvy a doby provozu z každého umístění. Každý nakonfigurovaného umístění spustí test každých pět minut.

Doba provozu je monitorovat pomocí kódy odpovědí protokolu HTTP a doby odezvy se měří v milisekundách. Monitorování test selže, pokud kód odpovědi HTTP je větší než nebo rovna hodnotě 400 nebo odpovědi trvá déle než 30 sekund. Koncový bod se považuje za k dispozici, pokud jeho monitorovací testy úspěšné z určitých umístění.

Nastavit, najdete v článku [monitorování aplikací ve službě Azure App Service](web-sites-monitor.md).

Viz také [udržování modelu weby Azure si navíc monitorování koncových bodů – se Stefan Schackow](https://channel9.msdn.com/Shows/Azure-Friday/Keeping-Azure-Web-Sites-up-plus-Endpoint-Monitoring-with-Stefan-Schackow) video o monitorování koncových bodů.

#### <a name="application-performance-monitoring-using-extensions"></a>Sledování výkonu aplikací pomocí rozšíření
Můžete také sledovat výkon vašich aplikací s využitím *rozšíření webu*.

Každá webová aplikace App Service poskytuje rozšiřitelná Správa koncový bod, který umožňuje používat sadu výkonných nástrojů nasazených jako rozšíření webu. Rozšíření patří: 

- Editorů zdrojového kódu, jako jsou [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx). 
- Nástroje pro správu pro připojené prostředky, jako jsou databáze MySQL připojený do webové aplikace.

[Azure Application Insights](https://azure.microsoft.com/services/application-insights/) je lokality rozšíření, která je také k dispozici pro monitorování výkonu. Jak pomocí Application Insights, znovu sestavte svůj kód pomocí sady SDK. Můžete také nainstalovat rozšíření, která poskytuje přístup k dalším datům. Sada SDK umožňuje napsat kód ke sledování využití a výkonu vaší aplikace podrobněji. Další informace najdete v tématu [monitorování výkonu webových aplikací](../application-insights/app-insights-web-monitor-performance.md).

<a name="collect" />

### <a name="2-collect-data"></a>2. Shromažďování dat
Prostředí webové aplikace poskytuje diagnostické funkce pro protokolování informací z webového serveru a webové aplikace. Informace je rozdělené na web server diagnostiky a konzole application diagnostics.

#### <a name="enable-web-server-diagnostics"></a>Povolte diagnostiku webového serveru
Můžete povolit nebo zakázat následující typy protokolů:

* **Podrobné protokolování chyb** – podrobné informace o chybě pro stavové kódy HTTP, které indikují chybu (stavový kód 400 nebo vyšší). To může obsahovat informace, které vám pomůže určit, proč server vrátil kód chyby.
* **Se nezdařilo, trasování požadavku** – podrobné informace o neúspěšných požadavcích, včetně trasování součásti služby IIS používá ke zpracování požadavku a doba trvání v jednotlivých komponentách. To může být užitečné, pokud se pokoušíte zlepšení výkonu webových aplikací a izolovat co způsobuje konkrétní chyba protokolu HTTP.
* **Web, protokolování na Server** – informace o transakce HTTP pomocí rozšířeném formátu protokolu W3C souboru. To je užitečné při určování celková metrika webové aplikace, jako je třeba počet požadavků zpracovaných nebo kolik žádostí se z konkrétní IP adresu.

#### <a name="enable-application-diagnostics"></a>Povolení rozhraní application diagnostics
Existuje několik možností, jak shromažďovat údaje o výkonu aplikací z webové aplikace, profil svou aplikaci živě ze sady Visual Studio nebo upravit kód aplikace nemusel Další informace a trasování. Můžete zvolit možnosti založené na nástroje vyjadřuje rozsah přístupu budete muset aplikaci a zjištěnými z monitorování.

##### <a name="use-application-insights-profiler"></a>Použití Application Insights Profiler
Můžete povolit Application Insights Profiler spustit zachytávání trasování podrobné výkonu. Přistupujete k trasování zachytit až pět dnů když potřebujete k prozkoumání problémů došlo v minulosti. Tuto možnost můžete za předpokladu, máte přístup k prostředku Application Insights pro webové aplikace na webu Azure portal.

Application Insights Profiler poskytuje statistické údaje na dobu odezvy jednotlivých webových volání a trasování, která určuje, který řádek kódu způsobil pomalé odezvy. Aplikace služby App Service je někdy pomalý, protože některé kód není zapsán výkonným způsobem. Mezi příklady patří sekvenční kód, který je možné spustit v paralelní a nežádoucí databáze zámků. Odebrání těchto problémová místa v kódu zvyšuje výkon vaší aplikace, ale jsou obtížné rozpoznat bez nastavování propracované trasování a protokolování. Trasování shromážděných službou Application Insights Profiler pomáhá identifikaci řádků kódu, který může zpomalit aplikaci a pro aplikace služby App Service pro tento problém vyřešili.

 Další informace najdete v tématu [profilace živé webové aplikace Azure pomocí Application Insights](../application-insights/app-insights-profiler.md).

##### <a name="use-remote-profiling"></a>Použití vzdálené profilování
Ve službě Azure App Service Web Apps, API Apps a WebJobs můžete vzdáleně Profilovat. Tuto možnost zvolte, pokud máte přístup k prostředku webové aplikace a víte, jak pro reprodukci problému nebo pokud znáte přesnou časový interval se stane tyto problémy s výkonem.

Vzdálená profilace je užitečné, pokud je vysoké využití procesoru procesem a váš proces běží pomaleji, než nebo latence požadavků protokolu HTTP jsou vyšší než normální, můžete vzdáleně Profilovat procesu a získat zásobníky volání vzorkování procesoru k analýze procesu aktivita a kód horké cesty.

Další informace najdete v tématu [podpoře vzdáleného profilování ve službě Azure App Service](https://azure.microsoft.com/blog/remote-profiling-support-in-azure-app-service).

##### <a name="set-up-diagnostic-traces-manually"></a>Ručně nastavit diagnostická trasování
Pokud máte přístup ke zdrojovému kódu aplikace webové konzole Application diagnostics umožňuje zaznamenávat informace vytvořené webové aplikace. Můžete použít aplikace ASP.NET `System.Diagnostics.Trace` třídy k protokolování informací do protokolu diagnostiky aplikace. Ale budete muset změnit kód a znova nasazovat aplikaci. Tato metoda se doporučuje, když vaše aplikace běží v testovacím prostředí.

Podrobné pokyny o tom, jak nakonfigurovat svoji aplikaci pro protokolování najdete v tématu [povolit protokolování diagnostiky pro webové aplikace ve službě Azure App Service](web-sites-enable-diagnostic-log.md).

#### <a name="use-the-azure-app-service-support-portal"></a>Použití portálu pro podporu služby Azure App Service
Funkce Web Apps poskytuje schopnost řešit problémy spojené s vaší webové aplikace pomocí protokolu HTTP protokolů, protokoly událostí, výpisy stavu procesu, a další. Dostanete tyto informace použít portál pro podporu na **http://&lt;název vaší aplikace >.scm.azurewebsites.net/Support**

Portál podpory Azure App Service nabízí tři samostatné karty pro podporu tři kroky pro běžné scénáře řešení potíží:

1. Sledovat aktuální chování
2. Analýza tím, že shromažďování diagnostických informací integrované analyzátory
3. Oprava

Pokud tento problém se děje právě teď, klikněte na tlačítko **analyzovat** > **diagnostiky** > **diagnostikovat nyní** diagnostické relace vytvořit za vás, které shromažďuje protokoly HTTP, protokoly Prohlížeče událostí, výpisy paměti, protokoly chyb PHP a sestava procesu PHP.

Jakmile jsou data shromážděna, portál podpory spustí analýzu dat a poskytne vám zprávu ve formátu HTML.

V případě, že budete chtít stahovat data, ve výchozím nastavení, by být uloženy ve složce D:\home\data\DaaS.

Další informace na podporu portálu služby Azure App Service najdete v tématu [nové aktualizace rozšíření webu podpory pro Azure Websites](https://azure.microsoft.com/blog/new-updates-to-support-site-extension-for-azure-websites).

#### <a name="use-the-kudu-debug-console"></a>Použijte konzolu pro ladění Kudu
Funkce Web Apps se dodává s konzolou pro ladění, který můžete použít pro ladění, zkoumat a nahrávání souborů, stejně jako koncové body JSON pro získání informací o vašem prostředí. Tato konzola se volá *konzola Kudu* nebo *řídicího panelu SCM* pro vaši webovou aplikaci.

Tento řídicí panel můžete přistupovat tak, že přejdete na odkaz **https://&lt;název vaší aplikace >.scm.azurewebsites.net/**.

Zde jsou některé z akcí, které poskytuje Kudu:

* nastavení prostředí pro vaši aplikaci
* stream protokolů
* diagnostické výpisu stavu systému
* ladění konzoly, ve kterém můžete spouštět rutiny prostředí Powershell a základní příkazy DOS.

Další užitečnou funkci Kudu je, že v případě, že vaše aplikace vyvolává výjimkách first-chance, můžete použít Kudu a vypíše nástroj SysInternals Procdump vytvořit paměti. Tyto výpisy paměti jsou snímky procesu a často vám můžou pomoct vyřešit problémy složitější s vaší webovou aplikací.

Další informace o funkcích dostupných v Kudu najdete v tématu [nástrojů Azure Websites Team Services byste měli vědět o](https://azure.microsoft.com/blog/windows-azure-websites-online-tools-you-should-know-about/).

<a name="mitigate" />

### <a name="3-mitigate-the-issue"></a>3. Tyto problémy zmírnit
#### <a name="scale-the-web-app"></a>Škálování webové aplikace
Ve službě Azure App Service pro vyšší výkon a propustnost, můžete upravit škálování, ve kterém je spuštěna vaše aplikace. Škálování webové aplikace vyžaduje dvě související akce: Změna vašeho plánu služby App Service na vyšší cenovou úroveň a určitá nastavení konfigurace, poté, co jste přešli na vyšší cenovou úroveň.

Další informace o škálování najdete v tématu [škálování webové aplikace ve službě Azure App Service](web-sites-scale.md).

Kromě toho můžete spustit aplikaci ve více než jednu instanci. Horizontální navýšení kapacity nejen vám poskytne další možnosti zpracování, ale také poskytuje určitou míru odolnosti proti chybám. Pokud proces přestane fungovat na jednu instanci, dalších instancí se dál obsluhovat požadavky.

Můžete nastavit jako ruční nebo automatické škálování.

#### <a name="use-autoheal"></a>Používají AutoHeal
AutoHeal recykluje pracovní proces pro vaši aplikaci na základě nastavení, které zvolíte (například změny konfigurace, požadavky, omezení na základě paměti nebo doba potřebná k provedení požadavku). Ve většině případů, recyklace procesu je nejrychlejší způsob, jak zotavení po chybě. I když můžete vždy restartovat webovou aplikaci z přímo z portálu Azure portal, AutoHeal postará automaticky za vás. Všechno, co musíte udělat, je přidání některých aktivačních událostí v kořenovém souboru web.config pro vaši webovou aplikaci. Tato nastavení bude fungovat stejným způsobem, i když vaše aplikace není aplikace .net.

Další informace najdete v tématu [Samoopravení weby Azure](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites/).

#### <a name="restart-the-web-app"></a>Restartujte webovou aplikaci
Restartuje se často je nejjednodušší způsob, jak zotavení při jednorázových potížích. Na [webu Azure portal](https://portal.azure.com/), v okně webové aplikace, máte možnost zastavit nebo restartovat aplikaci.

 ![restartování webové aplikace k řešení problémů s výkonem](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Můžete také spravovat webové aplikace pomocí Azure Powershellu. Další informace najdete v tématu [Použití Azure PowerShellu s Azure Resource Managerem](../powershell-azure-resource-manager.md).
