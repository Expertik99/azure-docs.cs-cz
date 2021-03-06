---
title: Vyzkoušení cloudového řešení vzdáleného monitorování IoT v Azure | Microsoft Docs
description: V tomto rychlém startu nasadíte akcelerátor řešení vzdáleného monitorování Azure IoT, přihlásíte se k řídicímu panelu řešení a budete s ním pracovat.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: quickstart
ms.custom: mvc
ms.date: 07/12/2018
ms.author: dobett
ms.openlocfilehash: 50005e38214bf22aa664c2d2b0cc4f86da412818
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/19/2018
ms.locfileid: "39144490"
---
# <a name="quickstart-try-a-cloud-based-remote-monitoring-solution"></a>Rychlý start: Vyzkoušení cloudového řešení vzdáleného monitorování

V tomto rychlém startu se dozvíte, jak nasadit akcelerátor řešení vzdáleného monitorování Azure IoT, který použijete jako cloudové řešení vzdáleného monitorování. Po nasazení akcelerátoru řešení použijete stránku **Řídicí panel** řešení k vizualizaci simulovaných zařízení na mapě a stránku **Údržba** k reakci na upozornění na tlak ze simulovaného chladícího zařízení. Tento akcelerátor řešení můžete využít jako výchozí bod pro vlastní implementaci nebo jako učební nástroj.

Ve počátečním nasazení se akcelerátor řešení vzdáleného monitorování nakonfiguruje pro společnost Contoso. Contoso spravuje výběr různých typů zařízení, například chladiče, nasazených v různých fyzických prostředích. Chladící zařízení do akcelerátoru řešení vzdáleného monitorování odesílá telemetrická data o teplotě, vlhkosti a tlaku.

K dokončení tohoto rychlého startu potřebujete aktivní předplatné Azure.

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

## <a name="deploy-the-solution"></a>Nasazení řešení

Při nasazování akcelerátoru řešení do předplatného Azure je potřeba nastavit několik možností konfigurace.

Přihlaste se k webu [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) pomocí přihlašovacích údajů svého účtu Azure.

Klikněte na **Vyzkoušejte** na dlaždici **Vzdálené monitorování**.

![Výběr vzdáleného monitorování](./media/quickstart-remote-monitoring-deploy/remotemonitoring.png)

Na stránce **Vytvořit řešení vzdáleného monitorování** vyberte **Základní** nasazení. Pokud akcelerátor řešení nasazujete kvůli tomu, abyste zjistili, jak funguje, nebo spustili ukázku, zvolte možnost **Základní**, abyste minimalizovali náklady.

Jako jazyk zvolte **.NET**. Implementace Java a .NET obsahují identické funkce.

Zadejte jedinečný **Název řešení** pro váš akcelerátor řešení vzdáleného monitorování. V tomto rychlém startu používáme název **contoso-rm2**.

Vyberte **Předplatné** a **Oblast**, které chcete při nasazování akcelerátoru řešení použít. Obvykle byste měli zvolit oblast, která je vám nejblíže. V tomto rychlém startu používáme možnosti **Visual Studio Enterprise** a **Západní Evropa**. V předplatném musíte být [globálním správcem nebo uživatelem](iot-accelerators-permissions.md).

Kliknutím na **Vytvořit řešení** zahájíte nasazování. Dokončení tohoto procesu trvá minimálně pět minut:

![Podrobnosti řešení vzdáleného monitorování](./media/quickstart-remote-monitoring-deploy/createform.png)

## <a name="sign-in-to-the-solution"></a>Přihlášení k řešení

Po dokončení nasazení předplatného Azure se na dlaždici řešení zobrazí zelené zaškrtnutí a **Připraveno**. Teď se můžete přihlásit k řídicímu panelu akcelerátoru řešení vzdáleného monitorování.

Na stránce **Zřízená řešení** klikněte na nový akcelerátor řešení vzdáleného monitorování:

![Výběr nového řešení](./media/quickstart-remote-monitoring-deploy/choosenew.png)

Na panelu, který se zobrazí, si můžete prohlédnout informace o akcelerátoru řešení vzdáleného monitorování. Zvolte **Řídicí panel řešení** a zobrazte akcelerátor řešení vzdáleného monitorování:

![Panel řešení](./media/quickstart-remote-monitoring-deploy/solutionpanel.png)

Kliknutím na **Přijmout** přijměte žádost o oprávnění. Ve vašem prohlížeči se zobrazí řídicí panel řešení vzdáleného monitorování:

[![Řídicí panel řešení](./media/quickstart-remote-monitoring-deploy/solutiondashboard-inline.png)](./media/quickstart-remote-monitoring-deploy/solutiondashboard-expanded.png#lightbox)

## <a name="view-your-devices"></a>Zobrazení zařízení

Na řídicím panelu řešení se zobrazí následující informace o simulovaných zařízeních společnosti Contoso:

* **Statistika zařízení** ukazuje souhrnné informace o upozorněních a celkový počet zařízení. Ve výchozím nasazení má Contoso 10 simulovaných zařízení různých typů.

* **Umístění zařízení** ukazuje fyzické umístění zařízení. Barva připínáčku značí případná upozornění ze zařízení.

* **Upozornění** ukazuje podrobnosti o upozorněních ze zařízení.

* **Telemetrie** ukazuje telemetrická data ze zařízení. Kliknutím na různé typy telemetrie v horní části můžete zobrazit různé datové proudy telemetrie.

* **Analýza** ukazuje kombinované informace o upozorněních ze zařízení.

## <a name="respond-to-an-alert"></a>Reakce na upozornění

Jako operátor ve společnosti Contoso můžete z řídicího panelu řešení monitorovat vaše zařízení. Panel **Statistika zařízení** ukazuje, že došlo k několika kritickým upozorněním, a panel **Upozornění** ukazuje, že většina z nich pochází z chladícího zařízení. V případě chladících zařízení společnosti Contoso vnitřní tlak vyšší než 250 PSI značí, že zařízení nefunguje správně.

### <a name="identify-the-issue"></a>Identifikace problému

Na stránce **Řídicí panel** na panelu **Upozornění** si můžete všimnout upozornění **Příliš vysoký tlak chladiče**. Chladič má na mapě červený připínáček (možná budete muset mapu posunout a přiblížit):

[![Řídicí panel ukazující upozornění na tlak a zařízení na mapě](./media/quickstart-remote-monitoring-deploy/dashboardalarm-inline.png)](./media/quickstart-remote-monitoring-deploy/dashboardalarm-expanded.png#lightbox)

Na panelu **Upozornění** klikněte ve sloupci **Prozkoumat** na **...** vedle pravidla **Příliš vysoký tlak chladiče**. Tato akce vás přesměruje na stránku **Údržba**, kde si můžete prohlédnout podrobnosti o pravidle, které upozornění aktivovalo.

Na stránce údržby **Příliš vysoký tlak chladiče** se zobrazí podrobnosti o pravidle, které aktivovalo upozornění. Stránka také uvádí, kdy k upozorněním došlo a které zařízení je aktivovalo:

[![Stránka údržby ukazující seznam aktivovaných upozornění](./media/quickstart-remote-monitoring-deploy/maintenancealarmlist-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenancealarmlist-expanded.png#lightbox)

Právě jste identifikovali problém, který upozornění aktivoval, a přidružené zařízení. Jako operátor byste dále měli upozornění vzít na vědomí a opravit problém.

### <a name="fix-the-issue"></a>Oprava problému

Abyste ostatním operátorům dali najevo, že na upozornění pracujete, vyberte ho a změňte **Stav upozornění** na **Potvrzeno**:

[![Výběr a potvrzení upozornění](./media/quickstart-remote-monitoring-deploy/maintenanceacknowledge-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenanceacknowledge-expanded.png#lightbox)

Hodnota ve sloupci stavu se změní na **Potvrzeno**.

Pokud chcete pracovat na chladiči, posuňte se dolů do části **Související informace**, v seznamu **Zařízení s oznámeními** vyberte chladící zařízení a pak zvolte **Úlohy**:

[![Výběr zařízení a naplánování akce](./media/quickstart-remote-monitoring-deploy/maintenanceschedule-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenanceschedule-expanded.png#lightbox)

Na panelu **Úlohy** vyberte **Spustit metodu** a pak metodu **EmergencyValveRelease**. Přidejte název úlohy **ChillerPressureRelease** a klikněte na **Použít**. Tato nastavení vytvoří úlohu, která se okamžitě spustí.

Pokud chcete zobrazit stav úlohy, vraťte se na stránku **Údržba** a prohlédněte si seznam úloh v zobrazení **Úlohy**. Možná budete muset několik sekund počkat, než se zobrazí spuštění úlohy za účelem uvolnění tlaku ventilu na chladiči:

[![Stav úlohy v zobrazení Úlohy](./media/quickstart-remote-monitoring-deploy/maintenancerunningjob-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenancerunningjob-expanded.png#lightbox)

### <a name="check-the-pressure-is-back-to-normal"></a>Kontrola návratu tlaku do normálního stavu

Pokud chcete zobrazit telemetrická data o tlaku v chladiči, přejděte na stránku **Řídicí panel**, na panelu telemetrie vyberte **Tlak** a zkontrolujte, že je tlak v zařízení **chiller-02.0** opět v normálu:

[![Tlak opět v normálu](./media/quickstart-remote-monitoring-deploy/pressurenormal-inline.png)](./media/quickstart-remote-monitoring-deploy/pressurenormal-expanded.png#lightbox)

Pokud chcete incident uzavřít, přejděte na stránku **Údržba**, vyberte upozornění a nastavte jeho stav na **Uzavřeno**:

[![Výběr a uzavření upozornění](./media/quickstart-remote-monitoring-deploy/maintenanceclose-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenanceclose-expanded.png#lightbox)

Hodnota ve sloupci stavu se změní na **Uzavřeno**.

## <a name="clean-up-resources"></a>Vyčištění prostředků

Pokud se budete chtít přesunout ke kurzům, ponechte akcelerátor řešení vzdáleného monitorování nasazený.

Pokud už akcelerátor řešení nepotřebujete, odstraňte ho na stránce [Zřízená řešení](https://www.azureiotsolutions.com/Accelerators#dashboard) tak, že ho vyberete a pak kliknete na **Odstranit řešení**:

![Odstranění řešení](media/quickstart-remote-monitoring-deploy/deletesolution.png)

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste nasadili akcelerátor řešení vzdáleného monitorování a dokončili jste úlohu monitorování s využitím simulovaných zařízení ve výchozím nasazení Contoso.

Další informace o akcelerátoru řešení se simulovanými zařízeními získáte v následujícím kurzu.

> [!div class="nextstepaction"]
> [Kurz: Monitorování zařízení IoT](iot-accelerators-remote-monitoring-monitor.md)