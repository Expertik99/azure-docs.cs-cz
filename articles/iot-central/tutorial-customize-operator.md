---
title: Přizpůsobení zobrazení operátora v Azure IoT Central | Microsoft Docs
description: Jako tvůrce můžete přizpůsobit zobrazení operátora ve vaší aplikaci Azure IoT Central.
author: sandeeppujar
ms.author: sandeepu
ms.date: 04/16/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
ms.openlocfilehash: c0b42c3efd5e015eaf1fbd750f835d8de8818de9
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2018
ms.locfileid: "43185847"
---
# <a name="tutorial-customize-the-azure-iot-central-operators-view"></a>Kurz: Přizpůsobení zobrazení Azure IoT Central pro operátora

Tento kurz vám jako tvůrci ukáže, jak přizpůsobit zobrazení vaší aplikace pro operátora. Když měníte aplikaci jako tvůrce, můžete zobrazit náhled zobrazení operátor v aplikaci Microsoft Azure IoT Central.

V tomto kurzu přizpůsobíte aplikaci tak, aby operátorovi zobrazovala relevantní informace o připojeném klimatizačním zařízení. Vaše přizpůsobení umožní operátorovi spravovat klimatizační zařízení připojená k aplikaci.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Konfigurace řídicího panelu zařízení
> * Konfigurace rozložení nastavení zařízení
> * Konfigurace rozložení vlastností zařízení
> * Zobrazení náhledu zařízení jako operátor
> * Konfigurace výchozí domovské stránky
> * Zobrazení náhledu výchozí domovské stránky jako operátor

## <a name="prerequisites"></a>Požadavky

Než začnete, měli byste dokončit dva předchozí kurzy:

* [Definování nového typu zařízení v aplikaci Azure IoT Central](tutorial-define-device-type.md)
* [Konfigurace pravidel a akcí pro zařízení](tutorial-configure-rules.md)

## <a name="configure-your-device-dashboard"></a>Konfigurace řídicího panelu zařízení

Jako tvůrce můžete definovat, které informace se zobrazí na řídicím panelu zařízení. V kurzu [Definování nového typu zařízení ve vaší aplikaci](tutorial-define-device-type.md) jste na řídicí panel **Connected Air Conditioner-1** přidali čárový graf a další informace.

1. Pokud chcete upravit šablonu zařízení **Connected Air Conditioner**, v levé navigační nabídce zvolte **Explorer** (Průzkumník):

    ![Stránka Explorer (Průzkumník)](media/tutorial-customize-operator/explorer.png)

2. Pokud chcete začít s přizpůsobováním řídicího panelu připojeného klimatizačního zařízení, vyberte šablonu zařízení **Connected Air Conditioner (1.0.0)**. Zvolte zařízení **Connected Air Conditioner-1**, které jste vytvořili v kurzu [Definování nového typu zařízení ve vaší aplikaci](tutorial-define-device-type.md):

    ![Výběr připojeného klimatizačního zařízení](media/tutorial-customize-operator/selectdevice.png)

    Když provádíte změny zařízení, jako je třeba **Connected Air Conditioner-1**, měníte příslušnou šablonu. Další informace najdete v tématu [Vytvoření nové verze šablony zařízení](howto-version-devicetemplate.md).

3. Pokud chcete upravit řídicí panel, zvolte **Dashboard** (Řídicí panel):

    ![Stránka řídicího panelu šablony zařízení](media/tutorial-customize-operator/dashboard.png)

4. Pokud chcete na řídicí panel přidat klíčový ukazatel výkonu, zvolte **KPI**:

    ![Přidání KPI](media/tutorial-customize-operator/addkpi.png)

    Při definování klíčového ukazatele výkonu použijte informace v následující tabulce:

    | Nastavení     | Hodnota |
    | ----------- | ----- |
    | Název        | Maximální teplota |
    | Měření | Teplota |
    | Agregace | Maximum |
    | Časové rozmezí  | Poslední 1 týden |

5. Zvolte **Uložit**. Teď na řídicím panelu vidíte dlaždici KPI:

    ![Dlaždice KPI](media/tutorial-customize-operator/temperaturekpi.png)

6. Pokud chcete přesunout dlaždici na řídicím panelu nebo změnit její velikost, přesuňte ukazatel myši na dlaždici. Dlaždici můžete přetáhnout na nové umístění nebo změnit její velikost:

    ![Úprava rozložení řídicího panelu](media/tutorial-customize-operator/dashboardlayout.png)

## <a name="configure-your-settings-layout"></a>Konfigurace rozložení nastavení

Jako tvůrce můžete také nakonfigurovat zobrazení nastavení zařízení pro operátora. Operátor používá stránku nastavení zařízení ke konfiguraci zařízení. Operátor použije stránku nastavení například k nastavení cílové teploty pro ledničku.

1. Pokud chcete upravit rozložení nastavení pro připojenou klimatizaci, zvolte **Settings** (Nastavení):

    ![Stránka Nastavení](media/tutorial-customize-operator/settings.png)

2. Dlaždice nastavení můžete přesunout a můžete také upravit jejich velikost:

    ![Úprava rozložení nastavení](media/tutorial-customize-operator/settingslayout.png)

> [!NOTE]
> V režimu **Design Mode** (Režim návrhu) nemůžete upravovat hodnoty nastavení.

## <a name="configure-your-properties-layout"></a>Konfigurace rozložení vlastností

Kromě řídicího panelu a nastavení můžete zobrazení operátora nakonfigurovat také pro vlastnosti zařízení. Operátor používá stránku vlastností zařízení ke správě metadat zařízení. Operátor použije stránku vlastností například k zobrazení sériového čísla zařízení nebo aktualizaci kontaktních údajů pro výrobce.

1. Pokud chcete upravit rozložení vlastností pro připojenou klimatizaci, zvolte **Properties** (Vlastnosti):

    ![Stránka Vlastnosti](media/tutorial-customize-operator/properties.png)

2. Pole vlastností můžete přesunout a můžete také upravit jejich velikost:

    ![Úprava rozložení vlastností](media/tutorial-customize-operator/propertieslayout.png)

> [!NOTE]
> V režimu **Design Mode** (Režim návrhu) nemůžete upravovat hodnoty vlastností.

## <a name="preview-the-connected-air-conditioner-device-as-an-operator"></a>Zobrazení náhledu připojeného klimatizačního zařízení jako operátor

V režimu **Design Mode** (Režim návrhu) můžete přizpůsobit řídicí panel, stránku vlastností a stránku nastavení pro operátora. Pokud vypnete režim **Design Mode** (Režim návrhu), můžete aplikaci zobrazit jako operátor.

1. Pokud chcete zobrazit připojené klimatizační zařízení jako operátor, musíte vypnout **Design Mode** (Režim návrhu). **Design Mode** můžete vypnout pomocí přepínače **Design Mode** v pravém horním rohu stránky.

2. Pokud chcete aktualizovat sériové číslo tohoto zařízení, upravte hodnotu na dlaždici se sériovým číslem a potom zvolte **Save** (Uložit):

    ![Úprava nastavení vlastnosti](media/tutorial-customize-operator/editproperty.png)

3. Pokud chcete připojené klimatizaci odeslat nastavení, zvolte **Settings** (Nastavení),změňte hodnotu nastavení na dlaždici a potom zvolte **Update** (Aktualizovat):

    ![Odesílání nastavení do zařízení](media/tutorial-customize-operator/sendsetting.png)

    Jakmile zařízení rozpozná změnu nastavení, nastavení se na dlaždici zobrazí jako **synced** (Synchronizováno).

4. Jako operátor můžete zobrazit řídicí panel zařízení tak, jak ho nakonfiguroval tvůrce:

    ![Zobrazení řídicího panelu zařízení pro operátora](media/tutorial-customize-operator/operatordashboard.png)

## <a name="configure-the-default-home-page"></a>Konfigurace výchozí domovské stránky

Když tvůrce nebo operátor přihlásí k aplikaci Azure IoT Central, uvidí domovskou stránku. Jako tvůrce můžete nakonfigurovat obsah této domovské stránky tak, aby zahrnovala obsah, který je pro operátory nejužitečnější a nejrelevantnější.

1. Pokud chcete přizpůsobit výchozí domovskou stránku, přejděte na stránku **Home** a v pravé horní části stránky zapněte **Design Mode** (Režim návrhu). Po zapnutí režimu **Design Mode** (Režim návrhu) se zprava vysune seznam objektů, které můžete přidat na domovskou stránku.

    ![Stránka Application Builder (Tvůrce aplikací)](media/tutorial-customize-operator/builderhome.png)

2. Pokud chcete přizpůsobit na domovskou stránku, přidejte dlaždice z **knihovny**. Zvolte **Link** (Odkaz) a přidejte podrobné informace o webu vaší organizace. Potom zvolte **Save** (Uložit):

    ![Přidání odkazu na domovskou stránku](media/tutorial-customize-operator/addlink.png)

    > [!NOTE]
    > Můžete také přidat odkazy na stránky ve vaší aplikaci Azure IoT Central. Můžete třeba přidat odkaz na řídicí panel zařízení nebo na stránku nastavení.

3. Volitelně můžete zvolit **Image** (Obrázek) a nahrát obrázek, který se zobrazí na vaší domovské stránce. Obrázek může mít adresu URL, na kterou přejdete po kliknutí na něj:

    ![Přidání obrázku na domovskou stránku](media/tutorial-customize-operator/addimage.png)

    Další informace najdete v tématu věnovaném [postupu při přípravě a nahrávání obrázků do aplikace Azure IoT Central](howto-prepare-images.md).

## <a name="preview-the-default-home-page-as-an-operator"></a>Zobrazení náhledu výchozí domovské stránky jako operátor

Pokud chcete zobrazit náhled domovské stránky tak, jak ji uvidí operátor, vypněte **Design Mode** (Režim návrhu) v pravé horní části stránky:

![Přepnutí režimu návrhu](media/tutorial-customize-operator/operatorviewhome.png)

Když kliknete na dlaždice s obrázky a odkazy, přejdete na adresy URL, které jste nastavili jako tvůrce.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili, jak přizpůsobit zobrazení aplikace pro operátora.

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Konfigurace řídicího panelu zařízení
> * Konfigurace rozložení nastavení zařízení
> * Konfigurace rozložení vlastností zařízení
> * Zobrazení náhledu zařízení jako operátor
> * Konfigurace výchozí domovské stránky
> * Zobrazení náhledu výchozí domovské stránky jako operátor

Teď když jste se naučili, jak přizpůsobit zobrazení aplikace pro operátora, můžete přejít k dalším navrhovaným krokům:

* [Monitorování zařízení (jako operátor)](tutorial-monitor-devices.md)
* [Přidání nového zařízení do aplikace (jako operátor a vývojář zařízení)](tutorial-add-device.md)
