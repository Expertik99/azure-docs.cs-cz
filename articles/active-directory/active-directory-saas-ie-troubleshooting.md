---
title: Řešení potíží s rozšíření Azure přístupového panelu pro aplikaci Internet Explorer | Dokumentace Microsoftu
description: Jak používat zásady skupiny nasadit doplněk aplikace Internet Explorer pro Moje aplikace z portálu.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/30/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 682973e6781a1de2c8d9628e39347650a3852b81
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364773"
---
# <a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Řešení potíží s rozšíření přístupového panelu pro aplikaci Internet Explorer
Tento článek pomáhá řešit následující problémy:

* Budete moci přistupovat k vaší aplikace na portálu Moje aplikace při použití aplikace Internet Explorer.
* Zobrazí se zpráva "Instalace softwaru" i když jste již nainstalovali software.

Pokud jste správcem, viz také: [jak pro aplikaci Internet Explorer pomocí zásad skupiny nasadit rozšíření přístupového panelu](active-directory-saas-ie-group-policy.md)

## <a name="run-the-diagnostic-tool"></a>Spusťte nástroj pro diagnostiku
Stažením a instalací diagnostický nástroj přístupového panelu můžete diagnostikovat potíže s instalací s rozšíření přístupového panelu:

1. [Kliknutím sem můžete stáhnout nástroj pro diagnostiku.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. Otevřete soubor a stiskněte klávesu **extrahovat všechny** tlačítko.
   
    ![Stisknutím klávesy Extrahovat vše](./media/active-directory-saas-ie-troubleshooting/extract1.png)
3. Stiskněte klávesu **extrahovat** tlačítko Pokračovat.
   
    ![Stisknutím klávesy Extract](./media/active-directory-saas-ie-troubleshooting/extract2.png)
4. Chcete-li spustit nástroj, klikněte pravým tlačítkem na soubor s názvem **AccessPanelExtensionDiagnosticTool**a pak vyberte **otevřít v programu > Microsoft Windows na základě Script Host**.
   
    ![Otevřít v programu > skriptu hostitele se systémem Microsoft Windows](./media/active-directory-saas-ie-troubleshooting/open_tool.png)
5. Zobrazí se následující okno diagnostiky, které popisuje, co mohou být další potíže s instalací.
   
    ![Ukázka diagnostiky okna](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)
6. Klikněte na tlačítko "**Ano**" nechte program opravte tyto problémy, které byly nalezeny.
7. Pokud chcete tyto změny uložit, Zavřít každé okno aplikace Internet Explorer a pak znovu spusťte aplikaci Internet Explorer.<br />Pokud pořád nemáte přístup k vaší aplikace, zkuste následující postup.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Zkontrolujte, že je povolené rozšíření přístupového panelu
Pokud chcete ověřit, zda je povoleno rozšíření přístupového panelu v aplikaci Internet Explorer:

1. V aplikaci Internet Explorer, klikněte na tlačítko **ikonu ozubeného kola** v pravém horním rohu okna. Potom vyberte **Možnosti Internetu**.<br />(Ve starších verzích Internet Exploreru se nachází v části **nástroje > Možnosti Internetu**.
   
    ![Přejděte na Nástroje > Možnosti Internetu](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)
2. Klikněte na tlačítko **programy** kartu a potom klikněte na tlačítko **spravovat doplňky** tlačítko.
   
    ![Klikněte na Spravovat doplňky](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)
3. V tomto dialogovém okně vyberte **rozšíření přístupového panelu** a potom klikněte na tlačítko **povolit** tlačítko.
   
    ![Klikněte na možnost povolit](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)
4. Pokud chcete tyto změny uložit, Zavřít každé okno aplikace Internet Explorer a pak znovu spusťte aplikaci Internet Explorer.

## <a name="enable-extensions-for-inprivate-browsing"></a>Povolit rozšíření pro procházení InPrivate
Pokud používáte režim procházení InPrivate:

1. V aplikaci Internet Explorer, klikněte na tlačítko **ikonu ozubeného kola** v pravém horním rohu okna. Potom vyberte **Možnosti Internetu**.<br />(Ve starších verzích Internet Exploreru se nachází v části **nástroje > Možnosti Internetu**.
   
    ![Ukázka diagnostiky okna](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)
2. Přejděte na **ochrany osobních údajů** kartě pak **zrušte zaškrtnutí políčka** zaškrtávací políčko s popiskem **zakázat panely nástrojů a rozšíření při spuštění procházení InPrivate**</p>
   
    ![Zrušte zaškrtnutí políčka Zakázat panely nástrojů a rozšíření při spuštění procházení InPrivate](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)
3. Pokud chcete tyto změny uložit, Zavřít každé okno aplikace Internet Explorer a pak znovu spusťte aplikaci Internet Explorer.

## <a name="uninstall-the-access-panel-extension"></a>Odinstalace rozšíření přístupového panelu
Chcete-li z počítače odinstalovat rozšíření přístupového panelu:

1. Na klávesnici, stiskněte **Windows klíč** k otevření nabídky Start. Při otevření nabídky zadáte cokoli, co provedete hledání. Zadejte "Ovládacích panelech" a pak otevřete **ovládací panely** když se zobrazí ve výsledcích hledání.
   
    ![Hledat ovládací prvek Panel](./media/active-directory-saas-ie-troubleshooting/search_sm.png)
2. V pravém horním rohu ovládacího panelu změnit **zobrazit** umožňuje **velké ikony**. Poté vyhledejte a klikněte **programy a funkce** tlačítko.
   
    ![Chang zobrazení na Zobrazit velké ikony](./media/active-directory-saas-ie-troubleshooting/control_panel.png)
3. V seznamu vyberte **rozšíření přístupového panelu**a potom **odinstalovat** tlačítko.
   
    ![Klikněte na tlačítko Odinstalovat](./media/active-directory-saas-ie-troubleshooting/uninstall.png)
4. Potom můžete nainstalovat rozšíření znovu, abyste viděli, pokud byl problém vyřešen.

Pokud narazíte na problémy odinstalovat rozšíření, můžete také odebrat pomocí [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) nástroj.

## <a name="related-articles"></a>Související články
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](active-directory-apps-index.md)
* [Přístup k aplikaci a jednotné přihlašování s Azure Active Directory](manage-apps/what-is-single-sign-on.md)
* [Jak nasadit rozšíření přístupového panelu pro aplikaci Internet Explorer pomocí zásad skupiny](active-directory-saas-ie-group-policy.md)

