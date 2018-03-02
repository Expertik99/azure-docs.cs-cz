---
title: "Zjistěte, jak zařadit řešení pro správu aktualizací, sledování změn a inventáře v Azure Automation."
description: "Zjistěte, jak zařadit do služby Azure virtuální počítač s řešeními správy aktualizací, sledování změn a inventáře, které jsou součástí Azure Automation."
services: automation
keywords: 
author: georgewallace
ms.author: gwallace
ms.date: 02/28/2018
ms.topic: article
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 670a0c2a11ebfe09406233ab4b454b2e9c2ba0e0
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions"></a>Zařadit řešení správy aktualizací, sledování změn a inventáře

Azure Automation nabízí řešení pro správu aktualizací zabezpečení operačního systému, sledování změn a inventáře nainstalovaných v počítačích. Existuje více způsobů zařadit počítače, můžete zaváděním řešení [z virtuálního počítače](automation-onboard-solutions-from-vm.md), z vašeho účtu Automation, nebo pomocí [runbook](automation-onboard-solutions.md). Tento článek se zabývá registrace těchto řešení z vašeho účtu Automation.

## <a name="log-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k Azure na adrese https://portal.azure.com.

## <a name="enable-solutions"></a>Povolit řešení

Přejděte na svůj účet Automation a vyberte buď **inventáře** nebo **sledování změn** pod **CONFIGURATION MANAGEMENT**.

Vyberte pracovní prostor analýzy protokolů a účet Automation a klikněte na **povolit** povolit řešení. Řešení bude povolit až 15 minut.

![Zařadit řešení inventáře](media/automation-onboard-solutions-from-automation-account/onboardsolutions.png)

Sledování změn a inventáře řešení poskytuje schopnost [sledování změn](automation-vm-change-tracking.md) a [inventáře](automation-vm-inventory.md) na virtuálních počítačích. V tomto kroku povolíte řešení na virtuálním počítači.

Po dokončení inventáře a sledování řešení registrace oznámení o změně, klikněte na **správy aktualizací** pod **CONFIGURATION MANAGEMENT**.

Řešení pro správu aktualizací můžete spravovat aktualizace a opravy pro virtuální počítače Windows Azure. Můžete vyhodnotit stav dostupné aktualizace, naplánovat instalaci požadovaných aktualizací, a zobrazit výsledky nasazení aktualizace byly úspěšně použity k virtuálnímu počítači. Tato akce Povolit řešení pro virtuální počítač.

Vyberte **Správa aktualizací** pod **správy aktualizací**. Vybraný pracovní prostor Log analytics je ve stejném pracovním prostoru použité v předchozím kroku. Klikněte na tlačítko **povolit** se budou registrovat do řešení pro správu aktualizací. Řešení bude povolit až 15 minut.

![Řešení zařadit aktualizací](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="scope-configuration"></a>Konfigurace oboru

Každé řešení používá konfigurace oboru v pracovním prostoru pro cílové počítače, které získání řešení. Konfigurace obor je skupina jednoho nebo více uložená hledání, která se používá k omezení oboru řešení, aby konkrétní počítače. Pro přístup k oboru konfigurace, v účtu Automation v části **související prostředky**, vyberte **prostoru**. V pracovním prostoru v **zdroje dat pracovního prostoru**, vyberte **konfigurace oboru**.

Dvě konfigurace oboru vytvořily ve výchozím nastavení **vlastnost MicrosoftDefaultScopeConfig ChangeTracking** a **MicrosoftDefaultScopeConfig aktualizace**.

## <a name="saved-searches"></a>Uložená hledání

Když počítač přidán do správy aktualizací nebo řešení inventáře a sledování změn, budou přidány do mezi dvěma uložená hledání v pracovním prostoru. Tato uložená hledání jsou dotazy, které obsahují počítače, které jsou cílem těchto řešení.

Přejděte na svůj účet Automation a vyberte **uložená hledání** pod **Obecné**. Dva uložená hledání používá tato řešení můžete vidět v následující tabulce:

|Název     |Kategorie  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  Vlastnost ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Aktualizace        | Updates__MicrosoftDefaultComputerGroup         |

Vyberte buď uloženého hledání, chcete-li zobrazit dotaz používá k naplnění skupiny. Následující obrázek ukazuje dotaz a jeho výsledky:

![Uložená hledání](media/automation-onboard-solutions-from-automation-account/savedsearch.png)

## <a name="onboard-an-azure-machine"></a>Zařadit počítač Azure

Účet, vyberte z vaší automatizace **inventáře** nebo **sledování změn** pod **CONFIGURATION MANAGEMENT**, nebo **Správa aktualizací** v části **správy aktualizací**.

Klikněte na tlačítko **+ přidat virtuální počítač Azure**, vyberte virtuální počítač ze seznamu. Na **správy aktualizací** klikněte na tlačítko **povolit**. Tento postup přidá aktuální virtuální počítač do skupiny počítačů, které jsou uložené hledání řešení.

## <a name="onboard-a-non-azure-machine"></a>Zařadit počítače mimo Azure

Účet, vyberte z vaší automatizace **inventáře** nebo **sledování změn** pod **CONFIGURATION MANAGEMENT**, nebo **Správa aktualizací** v části **správy aktualizací**.

Klikněte na tlačítko **počítače mimo Azure přidat**. Otevře se nové okno prohlížeče s pokyny k instalaci a konfiguraci agenta Microsoft Monitoring Agent na počítači, na počítač, můžete začít vytváření sestav k řešení. Pokud se registrace počítače, který v současné době spravovány nástrojem System Center Operations Manager, bude nový agent není vyžadováno, je zadání informací o prostoru do existujícího agenta.

## <a name="onboard-machines-in-the-workspace"></a>Zařadit počítačů v pracovním prostoru

Účet, vyberte z vaší automatizace **inventáře** nebo **sledování změn** pod **CONFIGURATION MANAGEMENT**, nebo **Správa aktualizací** v části **správy aktualizací**.

Vyberte **spravovat počítače**. Otevře **spravovat počítače** stránky. Tato stránka umožňuje povolit řešení na vybranou skupinu počítačů, všechny dostupné počítače, nebo povolit řešení pro všechny aktuální počítače a jeho povolení pro všechny budoucí počítače.

![Uložená hledání](media/automation-onboard-solutions-from-automation-account/managemachines.png)

### <a name="selected-machines"></a>Vybrané počítače

Chcete-li řešení pro jeden nebo více počítačů, vyberte **povolit na vybrané počítače** a klikněte na tlačítko **přidat** vedle každého počítače, které chcete přidat do řešení. Tato úloha přidá názvy vybraných počítačů do skupiny počítačů, které jsou uložené vyhledávací dotaz pro řešení.

### <a name="all-available-machines"></a>Všechny počítače k dispozici

Chcete-li řešení pro všechny počítače k dispozici, vyberte **povolit na všech počítačů s dostupností**. To zakáže ovládacího prvku se přidat počítače jednotlivě. Tato úloha přidá všechny názvy počítačů, vytváření sestav do pracovního prostoru do skupiny počítačů, které jsou uložené vyhledávací dotaz.

### <a name="all-available-and-future-machines"></a>Všechny dostupné a budoucí počítače

Chcete-li řešení pro všechny dostupné počítače a všechny budoucí počítače, vyberte **povolit na všech počítačích k dispozici a budoucí**. Tahle možnost odstraní uložená hledání a konfigurace oboru z pracovního prostoru. Otevře se řešení, aby všechny Azure a počítače mimo Azure, které podávají zprávy do pracovního prostoru.

## <a name="next-steps"></a>Další postup

Nadále kurzy o řešení se dozvíte, jak je používat.

* [Kurz – Správa aktualizací pro virtuální počítač](automation-tutorial-update-management.md)

* [Kurz – identifikaci softwaru na virtuálním počítači](automation-tutorial-installed-software.md)

* [Kurz – řešení potíží s změny na virtuálním počítači](automation-tutorial-troubleshoot-changes.md)