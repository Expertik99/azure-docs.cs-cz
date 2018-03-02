---
title: "Zjistěte, jak připojit řešení správy aktualizací, sledování změn a inventáře z virtuální počítač Azure"
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
ms.openlocfilehash: a850189406b394e7935763206f9e3a191b415170
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Zařadit řešení správy aktualizací, sledování změn a inventáře z virtuální počítač Azure

Azure Automation nabízí řešení pro správu aktualizací zabezpečení operačního systému, sledování změn a inventáře nainstalovaných v počítačích. Existuje více způsobů zařadit počítače, můžete zaváděním řešení z virtuálního počítače, [z vašeho účtu Automation](automation-onboard-solutions-from-automation-account.md), nebo pomocí [runbook](automation-onboard-solutions.md). Tento článek se zabývá registrace těchto řešení z virtuální počítač Azure.

## <a name="log-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k Azure na adrese https://portal.azure.com.

## <a name="enable-the-solutions"></a>Povolit řešení

Přejděte do existujícího virtuálního počítače a vyberte buď **Správa aktualizací**, **inventáře**, nebo **sledování změn** pod **OPERATIONS**.

Vyberte pracovní prostor a automatizace účet analýzy protokolů a klikněte na **povolit** povolit řešení. Řešení bude povolit až 15 minut.

![Zařadit řešení aktualizace](media/automation-onboard-solutions-from-vm/onboard-solution.png)

Přejděte do jiných řešení a klikněte na tlačítko **povolit**, analýzy protokolů a účet Automation, rozevírací seznamy jsou zakázané, které používají stejný účet prostoru a automatizace jako dříve povoleno řešení.

![Zařadit řešení aktualizace](media/automation-onboard-solutions-from-vm/onboard-solutions2.png)

> [!NOTE]
> **Sledování změn** a **inventáře** použít stejné řešení, když je povolena jedna dalších je povolen také.

## <a name="scope-configuration"></a>Konfigurace oboru

Každé řešení používá konfigurace oboru v pracovním prostoru pro cílové počítače, které získání řešení. Konfigurace obor je skupina jednoho nebo více uložená hledání, která se používá k omezení oboru řešení, aby konkrétní počítače. Pro přístup k oboru konfigurace, v účtu Automation v části **související prostředky**, vyberte **prostoru** pak v pracovním prostoru v **zdroje dat pracovního prostoru**, Vyberte **konfigurace oboru**.

Konfigurace dvou oboru vytvořily ve výchozím nastavení jsou **vlastnost MicrosoftDefaultScopeConfig ChangeTracking** a **MicrosoftDefaultScopeConfig aktualizace**.

Klikněte na výpustky (...) na žádném z konfigurací a vyberte **upravit**. Na **konfiguraci úpravy rozsahu** vyberte **vyberte skupiny počítačů** otevřete **skupiny počítačů** stránky. Tato stránka zobrazuje uložená hledání, které se používají k vytvoření konfigurace oboru.

## <a name="saved-searches"></a>Uložená hledání

Když počítač přidán do správy aktualizací nebo řešení inventáře a sledování změn, budou přidány do mezi dvěma uložená hledání v pracovním prostoru. Tato uložená hledání jsou dotazy, které obsahují počítače, které jsou cílem těchto řešení.

Přejděte do pracovního prostoru a vyberte **uložená hledání** pod **Obecné**. Dva uložená hledání používá tato řešení můžete vidět v následující tabulce:

|Název     |Kategorie  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  Vlastnost ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Aktualizace        | Updates__MicrosoftDefaultComputerGroup         |

Vyberte buď uloženého hledání, chcete-li zobrazit dotaz používá k naplnění skupiny. Následující obrázek ukazuje dotaz a jeho výsledky.

![Uložená hledání](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="next-steps"></a>Další postup

Nadále kurzy o řešení se dozvíte, jak je používat.

* [Kurz – Správa aktualizací pro virtuální počítač](automation-tutorial-update-management.md)

* [Kurz – identifikaci softwaru na virtuálním počítači](automation-tutorial-installed-software.md)

* [Kurz – řešení potíží s změny na virtuálním počítači](automation-tutorial-troubleshoot-changes.md)
