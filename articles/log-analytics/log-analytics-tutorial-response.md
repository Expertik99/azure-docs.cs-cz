---
title: Reakce na události s využitím upozornění Azure Log Analytics | Microsoft Docs
description: V tomto kurzu se naučíte používat upozornění služby Log Analytics ke zjišťování důležitých informací o vašem pracovním prostoru nebo k preventivnímu upozorňování na problémy a volání akcí, které se je pokusí opravit.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/30/2018
ms.author: magoedte
ms.custom: mvc
ms.component: na
ms.openlocfilehash: c6c7b3f897e38fbd67098c9f881380bc073f13da
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/02/2018
ms.locfileid: "39432646"
---
# <a name="respond-to-events-with-azure-monitor-alerts"></a>Reakce na události s upozorněními služby Azure Monitor
Pravidla prohledávání protokolu vytváří služba Azure Alerts pro automatické spouštění zadaných dotazů na protokoly v pravidelných intervalech.  Pokud výsledky dotazu na protokol splňují konkrétní kritéria, vytvoří se záznam upozornění. Pravidlo potom může automaticky spustit jednu nebo více akcí pomocí [skupin akcí](../monitoring-and-diagnostics/monitoring-action-groups.md).  Tento kurz je pokračováním kurzu [Vytváření a sdílení řídicích panelů s daty Log Analytics](log-analytics-tutorial-dashboards.md).   

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Vytvoření pravidla upozornění
> * Konfigurace skupiny akcí na posílání e-mailového oznámení

K dokončení příkladu v tomto kurzu potřebujete existující virtuální počítač [připojený k pracovnímu prostoru Log Analytics](log-analytics-quick-collect-azurevm.md).

## <a name="sign-in-to-azure-portal"></a>Přihlášení k webu Azure Portal
Přihlaste se k webu Azure Portal na adrese [https://portal.azure.com](https://portal.azure.com).

## <a name="create-alerts"></a>Vytváření upozornění
Upozornění vytvářejí pravidla upozornění služby Azure Monitor. Pravidla mohou v pravidelných intervalech automaticky spouštět uložené dotazy nebo vlastní prohledávání protokolů.  Můžete vytvářet upozornění na základě konkrétních metrik výkonu, vytvoření určitých událostí, chybějící události nebo počtu událostí vytvořených v konkrétním časovém intervalu.  Upozornění můžete například použít, když chcete oznámit, že průměrné využití procesoru překročilo určitou prahovou hodnotu, že byla zjištěna chybějící aktualizace nebo ke generování události, protože bylo zjištěno, že neběží určitá služba Windows nebo linuxový démon (proces).  Pokud výsledky prohledávání protokolu odpovídají určitým kritériím, vytvoří se upozornění. Pravidlo pak může automaticky spustit jednu nebo více akcí. Může vás třeba upozornit nebo volat jiný proces.

V následujícím příkladu vytvoříte pravidlo upozornění na naměřenou hodnotu, které vychází z dotazu na *využití procesoru virtuálních počítačů Azure* uloženého v [kurzu o vizualizaci dat](log-analytics-tutorial-dashboards.md). Upozornění se vytvoří pro každý virtuální počítač, který překročí prahovou hodnotu 90 %.

1. Na webu Azure Portal klikněte na **Všechny služby**. V seznamu prostředků zadejte **Monitor**. Seznam se průběžně filtruje podle zadávaného textu. Vyberte **Monitor**.
1. V levém podokně vyberte **Upozornění** a potom nahoře na stránce klikněte na **Nové pravidlo upozornění** a vytvořte nové upozornění.

    ![Vytvoření nového pravidla upozornění](./media/log-analytics-tutorial-response/alert-rule-02.png)

1. V prvním kroku vyberte v části **Vytvořit upozornění** jako zdroj pracovní prostor Log Analytics, protože jde o výstražný signál založený na protokolu.  Vyfiltrujte výsledky. Pokud máte více předplatných, vyberte v rozevíracím seznamu určité **předplatné**, které obsahuje dříve vytvořený virtuální počítač a pracovní prostor Log Analytics.  Vyfiltrujte **typ prostředku** tím, že v rozevíracím seznamu vyberete **Log Analytics**.  Nakonec vyberte v poli **Prostředek** položku **DefaultLAWorkspace** a pak klikněte na **Hotovo**.

    ![Vytvoření upozornění – 1. krok](./media/log-analytics-tutorial-response/alert-rule-03.png)

1. V části **Kritéria upozornění** klikněte na **Přidat kritéria** a definujte dotaz a potom zadejte logiku, která je pro pravidlo upozornění závazná. V okně **Konfigurovat logiku signálů** jako název signálu vyberte **Vlastní prohledávání protokolu** a svůj dotaz zadejte do pole **Vyhledávací dotaz**.

    Příklad:
    ```
    Perf
    | where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total"
    | summarize AggregatedValue=avg(CounterValue) by bin(TimeGenerated, 1m)
    ```

    Podokno se aktualizuje, aby zobrazovalo nastavení konfigurace upozornění.  Nahoře se zobrazují výsledky za posledních 30 minut vybraného signálu.

1. Nakonfigurujte upozornění podle následujících informací:  
   a. V rozevíracím seznamu **Na základě* vyberte **Měření metriky**.  Měření metriky vytvoří pro každý objekt dotazu upozornění s hodnotou, která překračuje zadanou prahovou hodnotu.  
   b. V poli **Podmínka** vyberte **Větší než** a jako **prahovou hodnotu** zadejte **90**.  
   c. V části Aktivovat upozornění na základě vyberte **Po sobě jdoucí porušení**, v rozevíracím seznamu vyberte **Větší než** a zadejte hodnotu 3.  
   d. V části Vyhodnoceno na základě potvrďte výchozí hodnoty. Pravidlo se spustí každých pět minut a vrátí záznamy vytvořené v tomto časovém intervalu.  
1. Klikněte na **Hotovo** a dokončete pravidlo upozornění.

    ![Konfigurace signálu upozornění](./media/log-analytics-tutorial-response/alert-signal-logic-02.png)

1. Přejděte ke druhému kroku, ve kterém do pole **Název pravidla upozornění** zadáte název upozornění, například **Využití CPU na více než 90 procent**.  Do pole **Popis** zadejte podrobné informace o upozornění a v poli **Závažnost** vyberte **Kritické (záv. 0)**.

    ![Konfigurace podrobností upozornění](./media/log-analytics-tutorial-response/alert-signal-logic-04.png)

1. Pokud chcete vytvořené pravidlo ihned aktivovat, potvrďte výchozí hodnotu přepínače **Po vytvoření povolit pravidlo**.  
1. Ve třetím a posledním kroku zadejte **Skupinu akcí**, abyste zajistili, že se při každé aktivaci upozornění provedou stejné akce. Skupinu akcí můžete použít pro každé definované pravidlo.  Ke konfiguraci nové skupiny akcí použijte následující informace:  
   a. Vyberte **Nová skupina akcí**. Zobrazí se podokno **Přidat skupinu akcí**.  
   b. Do pole **Název skupiny akcí** zadejte název, třeba **Operace IT – oznámení** a do pole **Krátký název** zadejte třeba **itop-ozn**.  
   c. Zkontrolujte správnost výchozích hodnot v polích **Předplatné** a **Skupina prostředků**. Pokud nejsou správné, vyberte správné hodnoty v rozevíracím seznamu.  
   d. V oddílu Akce zadejte název akce, třeba **Poslat e-mail** a jako **Typ akce** vyberte v rozevíracím seznamu **E-mail/SMS/Vytažení/Hlasová**. Vpravo se otevře podokno vlastností **E-mail/SMS/Vytažení/Hlasová**, kde můžete zadat další informace.  
   e. V podokně **E-mail/SMS/Vytažení/Hlasová** aktivujte **E-mail** a zadejte platnou e-mailovou adresu SMTP pro zasílání zpráv.  
   f. Klikněte na tlačítko **OK** a uložte změny.  
       ![Vytvoření nové skupiny akcí](./media/log-analytics-tutorial-response/action-group-properties-01.png)

1. Skupinu akcí dokončete kliknutím na **OK**.
1. K dokončení pravidla upozornění klikněte na **Vytvořit pravidlo upozornění**. Pravidlo se okamžitě spustí.

    ![Dokončení nového pravidla upozornění](./media/log-analytics-tutorial-response/alert-rule-01.png)

## <a name="view-your-alerts-in-azure-portal"></a>Zobrazení upozornění na webu Azure Portal
Vytvořená upozornění si v Azure můžete prohlédnout v jediném podokně, kde můžete také spravovat všechna pravidla upozornění ve všech svých předplatných Azure. V seznamu jsou všechna pravidla upozornění (povolená i zakázaná). Můžete je řadit podle cílových prostředků, skupin prostředků, názvu pravidla nebo stavu. Je tu i agregovaný souhrn všech aktivovaných upozornění a celkový počet nakonfigurovaných/povolených pravidel upozornění.

![Stránka stavu služby Azure Alerts](./media/log-analytics-tutorial-response/azure-alerts-02.png)

Když se výstraha aktivuje, zobrazí se stav v tabulce, včetně informace o počtu výskytů ve vybraném časovém intervalu (výchozí je posledních šest hodin).  V doručené poště byste měli mít příslušný e-mail, který bude vypadat podobně jako následující příklad. Informuje o problematických virtuálních počítačích a nejdůležitějších výsledcích, které odpovídají nastavenému vyhledávacímu dotazu.

![Příklad akce upozornění e-mailem](./media/log-analytics-tutorial-response/azure-alert-email-notification-01.png)

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste zjistili, jak můžou pravidla upozornění proaktivně identifikovat problémy spouštěním prohledávání protokolů v naplánovaných intervalech a reagovat na problémy v případě, že výsledky prohledávání splní konkrétní kritéria.

Na tomto odkazu najdete předem připravené ukázky skriptů pro Log Analytics.

> [!div class="nextstepaction"]
> [Ukázky skriptů pro Log Analytics](powershell-samples.md)
