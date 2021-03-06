---
title: Odstranit pracovní prostor služby Azure Log Analytics | Microsoft Docs
description: Zjistěte, jak odstranit pracovní prostor analýzy protokolů, pokud jste vytvořili v odběru osobní nebo změny struktury modelu pracovního prostoru.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 54f2af60751ed0d9c64e71efad6fa9aa3ef06589
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/29/2018
ms.locfileid: "37129111"
---
# <a name="delete-an-azure-log-analytics-workspace-with-the-azure-portal"></a>Odstranit pracovní prostor služby Azure Log Analytics pomocí portálu Azure
Tento článek ukazuje, jak pomocí portálu Azure můžete odstranit pracovní prostor analýzy protokolů, které už můžou vyžadovat. 

## <a name="to-delete-a-workspace"></a>Odstranění pracovního prostoru 
Při odstranění pracovní prostor analýzy protokolů všech dat spojených s pracovního prostoru je odstranit ze služby do 30 dní.  Chcete postupujte opatrně při odstranění pracovního prostoru, protože může být důležitá data a konfiguraci, která může mít negativní vliv na vaše operace služby. Vezměte v úvahu další služby Azure a zdroje, které ukládají data v analýzy protokolů, jako například:

* Application Insights
* Azure Security Center
* Azure Automation
* Agentů spuštěných na virtuálních počítačích s Windows a Linux
* Agentů spuštěných v systému Windows a Linux počítačů ve vašem prostředí
* System Center Operations Manager
* Řešení pro správu 

Všechny agenty a skupin pro správu System Center Operations Manager nakonfigurovat tak, aby sestavy do pracovního prostoru i nadále v osamocené stavu.  Jaké agenty řešení, inventáře a jinými službami Azure jsou integrované s pracovním prostoru, než budete pokračovat.   
 
Pokud jste správce a k pracovnímu prostoru bylo přidruženo víc uživatelů, přidružení těchto uživatelů s pracovním prostorem se přeruší. Pokud byli tito uživatelé přidruženi s jinými pracovními prostory, můžou pokračovat v používání služby Log Analytics s těmito prostory. Ale pokud nejsou přidruženy jiných pracovních prostorech pak potřebují k vytvoření pracovního prostoru analýzy protokolů použít. 

1. Přihlaste se k webu [Azure Portal](http://portal.azure.com). 
2. Na webu Azure Portal klikněte v levém dolním rohu na **Další služby**. V seznamu prostředků zadejte **Log Analytics**. Seznam se průběžně filtruje podle zadávaného textu. Vyberte **Log Analytics**.
3. V podokně analýzy protokolů předplatných, vyberte pracovní prostor a pak klikněte na tlačítko **odstranit** z horní části podokna uprostřed.<br><br> ![Odstranit možnost z podokna Vlastnosti pracovního prostoru](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace.png)<br>  
4. Když se zobrazí okno potvrzovací zpráva s výzvou k potvrzení odstranění pracovního prostoru, klikněte na tlačítko **Ano**.<br><br> ![Potvrdit odstranění pracovního prostoru](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace-confirm.png)

