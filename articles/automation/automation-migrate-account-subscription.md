---
title: "Migraci účet Automation a prostředků | Microsoft Docs"
description: "Tento článek popisuje, jak přesunout účet Automation v Azure Automation a přidružených prostředků z jednoho předplatného."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 9c2db4a2-f324-48dc-8ce7-3343bf7230d5
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/29/2017
ms.author: magoedte
ms.openlocfilehash: 40567d1b1f450c499715682cf8c22f0c79c7d0c7
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="migrate-automation-account-and-resources"></a>Migraci účet Automation a prostředků
Pro účty Automation a jeho přidružené prostředky (tj. prostředky, sady runbook, moduly atd.), které jste vytvořili na portálu Azure a chcete provést migraci z jedné skupiny prostředků do jiné nebo z jedno předplatné na jiný můžete to provést snadno s [přesunout prostředky](../azure-resource-manager/resource-group-move-resources.md) funkce dostupné na portálu Azure. Před pokračováním této akce je však měli nejdřív si následující [kontrolní seznam před přesunutím prostředků](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) a kromě toho v následujícím seznamu specifické pro automatizaci.  

1. Cílového předplatného/skupiny prostředků musí být ve stejné oblasti jako zdroj. Znamená, nelze jej přesunout účty Automation v oblastech.
2. Při přesouvání prostředků (například sady runbook, úloh, atd.), skupině zdrojové a cílové skupiny jsou zamčené pro dobu trvání operace. Zápis a operace odstranění se v skupiny blokuje až po dokončení přesunu. 
3. Všechny runbooky a proměnné, které odkazují na ID prostředků nebo předplatného z existujícího předplatného, musí být aktualizován po dokončení migrace.  

> [!NOTE]
> Tato funkce nepodporuje přesunutí klasické prostředky služby automation.
>
>

## <a name="to-move-the-automation-account-using-the-portal"></a>Přesunutí účtu Automation pomocí portálu
1. Z vašeho účtu Automation, klikněte na tlačítko **přesunout** v horní části stránky.<br> ![Přesuňte možnost](media/automation-migrate-account-subscription/automation-menu-move.png)<br>
2. Vyberte účet automation přesunout do jiné skupiny prostředků nebo jiné předplatné.
3. Na **přesunout prostředky** podokně, Všimněte si, představuje zdroje související s účtu Automation a vaší skupiny prostředků. Vyberte **předplatné** a **skupiny prostředků** z rozevíracího seznamu, nebo vyberte možnost **vytvořit novou skupinu prostředků** a do pole zadejte nový název skupiny prostředků k dispozici. 
4. Zkontrolujte a zaškrtněte políčko můžete potvrdit *pochopit nástroje a skripty bude nutné aktualizovat, a použít nové ID prostředku po přesunutí prostředků* a pak klikněte na **OK**.<br> ![Přesunout prostředky podokno](media/automation-migrate-account-subscription/automation-move-resources-blade.png)<br>   

Tato akce může trvat několik minut na dokončení. V **oznámení**, zobrazí se stav každé akce, který probíhá - ověřování, migrace a potom nakonec po dokončení.    

## <a name="to-move-the-automation-account-using-powershell"></a>Přesunutí účtu Automation pomocí prostředí PowerShell
Chcete-li existující prostředky Automation přesunout do jiné skupiny prostředků nebo předplatného, použijte **Get-AzureRmResource** rutiny konkrétní účet Automation a potom **přesunutí AzureRmResource** rutiny provedení přesunu.

V prvním příkladu ukazuje, jak přesunout účet služby Automation do nové skupiny prostředků.

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup"
   ```

Po provedení výše uvedeném příkladu kódu, zobrazí se výzva k ověření, že chcete provést tuto akci. Po kliknutí na tlačítko **Ano** a povolit skript, který chcete-li pokračovat, když se provádí migrace neobdržíte žádné oznámení. 

Pokud chcete přesunout do nového předplatného, obsahovat hodnotu pro *DestinationSubscriptionId* parametr.

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup" -DestinationSubscriptionId "SubscriptionId"
   ```

Stejně jako u předchozí příklad, zobrazí se výzva k potvrzení, že se při přesunutí. 

## <a name="next-steps"></a>Další postup
* Další informace o přesunutí prostředků do nové skupiny prostředků nebo předplatného najdete v tématu [přesunutím prostředků do nové skupiny prostředků nebo předplatného](../azure-resource-manager/resource-group-move-resources.md)
* Další informace o řízení přístupu na základě rolí ve službě Azure Automation najdete v tématu [řízení přístupu na základě Role ve službě Azure Automation](automation-role-based-access-control.md).
* Další informace o rutinách prostředí PowerShell pro správu předplatného, najdete v části [pomocí prostředí Azure PowerShell s Resource Managerem](../azure-resource-manager/powershell-azure-resource-manager.md)
* Další informace o portálu funkce pro správu předplatného najdete v tématu [pomocí portálu Azure ke správě prostředků](../azure-resource-manager/resource-group-portal.md).
