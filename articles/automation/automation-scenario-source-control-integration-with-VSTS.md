---
title: "Integrovat Azure Automation Visual Stuido Team Services zdrojového kódu | Microsoft Docs"
description: "Scénář vás provede procesem nastavení integrace s účet Azure Automation a zdrojového kódu Visual Stuido Team Services."
services: automation
documentationcenter: 
author: eamono
manager: 
editor: 
keywords: "služby VSTS, Azure powershell zdrojového kódu automatizace"
ms.assetid: a43b395a-e740-41a3-ae62-40eac9d0ec00
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.openlocfilehash: 5afccc4aa7b751958952d1401182f93109cff358
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-visual-studio-team-services"></a>Azure Automation scénář – integrace ovládacích prvků zdrojového automatizace s Visual Studio Team Services

V tomto scénáři máte projekt Visual Studio Team Services, který používáte ke správě Azure Automation runbook nebo konfigurace DSC ve správě zdrojového kódu.
Tento článek popisuje postup pro integraci služby VSTS prostředí Azure Automation, aby průběžnou integraci se stane, pro každý vrácení se změnami.

## <a name="getting-the-scenario"></a>Získání scénáře

Tento scénář se skládá ze dvou Powershellové runbooky, které můžete importovat přímo z [Galerie Runbooků](automation-runbook-gallery.md) v portálu Azure nebo stažení [Galerie prostředí PowerShell](https://www.powershellgallery.com).

### <a name="runbooks"></a>Runbooky

Runbook | Popis| 
--------|------------|
Sync-VSTS | Import sady runbook nebo konfigurace z služby VSTS zdrojového kódu, pokud se provádí vrácení se změnami. Je-li spustit ručně, naimportuje a publikuje všechny sady runbook nebo konfigurace do účtu Automation.| 
Sync-VSTSGit | Import sady runbook nebo konfigurace ze služby VSTS ve správě zdrojového Git Pokud se provádí vrácení se změnami. Je-li spustit ručně, naimportuje a publikuje všechny sady runbook nebo konfigurace do účtu Automation.|

### <a name="variables"></a>Proměnné

Proměnná | Popis|
-----------|------------|
VSToken | Zabezpečte variabilní prostředek, který vytvoříte, který obsahuje osobní přístupový token služby VSTS. Zjistěte, jak vytvořit osobní přístupový token služby VSTS na [stránka ověřování služby VSTS](/vsts/accounts/use-personal-access-tokens-to-authenticate).
## <a name="installing-and-configuring-this-scenario"></a>Instalace a konfigurace tohoto scénáře

Vytvoření [osobní přístupový token](/vsts/accounts/use-personal-access-tokens-to-authenticate) v služby VSTS, který použijete k synchronizaci sady runbook nebo konfigurace do vašeho účtu automation.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

Vytvoření [zabezpečené proměnná](automation-variables.md) ve vašem účtu automation k uchování osobní přístupový token, aby mohli ověřit služby VSTS a synchronizace sady runbook nebo konfigurace do účtu Automation runbook. Tato VSToken můžete pojmenovat. 

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

Naimportujte sadu runbook, který je synchronizován sady runbook nebo konfigurace do účtu automation. Můžete použít [ukázkové sady runbook služby VSTS](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) nebo [VSTS s Git ukázkové sady runbook] (https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) z PowerShellGallery.com podle toho, pokud používáte služby VSTS zdroje služby VSTS s Gitem nebo ovládací prvek a nasazení do vašeho účtu automation.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

Teď můžete [publikování](automation-creating-importing-runbook.md#publishing-a-runbook) této sady runbook, takže si můžete vytvořit webhooku. 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

Vytvoření [webhooku](automation-webhooks.md) pro tento runbook služby synchronizace VSTS a vyplňte parametry, jak je uvedeno níže. Ujistěte se, že potřebujete pro služby háku v služby VSTS se zkopírujte adresu url webhooku. VSAccessTokenVariableName je název (VSToken) zabezpečené proměnné, kterou jste vytvořili dříve, aby udržení osobní přístupový token. 

Integrace s služby VSTS (synchronizace-VSTS.ps1) mají následující parametry:
### <a name="sync-vsts-parameters"></a>Služby synchronizace VSTS parametry

Parametr | Popis| 
--------|------------|
WebhookData | Tato položka obsahuje vrácení se změnami informace odesílané ze služby hák služby VSTS. Tento parametr by měl ponechat prázdné.| 
ResourceGroup | Toto je název skupiny prostředků, zda má účet automation v.|
AutomationAccountName | Název účtu služby automation, který je synchronizován s služby VSTS.|
VSFolder | Název složky v služby VSTS existuje sady runbook a konfigurace.|
VSAccount | Název účtu, Visual Studio Team Services.| 
VSAccessTokenVariableName | Název proměnné zabezpečené (VSToken), která uchovává osobní přístupový token služby VSTS.| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

Pokud používáte služby VSTS s GITEM (synchronizace-VSTSGit.ps1) bude trvat následující parametry.

Parametr | Popis|
--------|------------|
WebhookData | To bude obsahovat informace o vrácení se změnami z hák služby VSTS služby. Tento parametr by měl ponechat prázdné.| ResourceGroup | Tento název skupiny prostředků, zda má účet automation v.|
AutomationAccountName | Název účtu služby automation, který je synchronizován s služby VSTS.|
VSAccount | Název účtu, Visual Studio Team Services.|
VSProject | Název projektu v služby VSTS existuje sady runbook a konfigurace.|
GitRepo | Název úložiště Git.|
GitBranch | Název větve v úložišti služby VSTS Git.|
Složka | Název složky v Git služby VSTS větev.|
VSAccessTokenVariableName | Název proměnné zabezpečené (VSToken), která uchovává osobní přístupový token služby VSTS.|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

Vytvoření služby háku v služby VSTS pro vrácení se změnami do složky, která aktivuje tento webhook na změnami kódu. Vyberte **nástrojů Web Hooks** jako službu, kterou chcete integrovat při vytváření nové předplatné. Další informace o službách v [dokumentace služby VSTS služeb Jenkins](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

Teď by měla být moci provést všechny vrácení se změnami runbooky a konfiguraci do služby VSTS a mají tyto automaticky synched do vašeho účtu automation.

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

Pokud ručně spuštění této sady runbook bez se aktivuje pomocí služby VSTS parametr webhookdata můžete nechat prázdné a provede úplnou synchronizaci ze služby VSTS složce určené.

Pokud chcete odinstalovat tento scénář, odeberte hák služby ze služby VSTS, odstraňte sadu runbook a proměnná VSToken.
