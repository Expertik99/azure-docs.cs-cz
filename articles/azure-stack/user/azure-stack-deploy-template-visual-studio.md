---
title: Nasazení šablon pomocí sady Visual Studio ve službě Azure Stack | Dokumentace Microsoftu
description: Informace o nasazení šablon pomocí sady Visual Studio ve službě Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 628da2ae-64cc-42e0-b8b7-a6a3724cb974
ms.service: azure-stack
ms.custom: vs-azure
ms.workload: azure-vs
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 585f890b11ab71f9c10479ff65aff74922a30ed1
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/22/2018
ms.locfileid: "42147047"
---
# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Nasazení šablon ve službě Azure Stack pomocí sady Visual Studio

*Platí pro: Azure Stack integrované systémy a Azure Stack Development Kit*

Visual Studio můžete použít k nasazení šablony Azure Resource Manageru ke službě Azure Stack.

## <a name="to-deploy-a-template"></a>K nasazení šablony

1. [Nainstalujte a připojte](azure-stack-install-visual-studio.md) do služby Azure Stack pomocí sady Visual Studio.
2. Otevřete sadu Visual Studio.
3. Vyberte **souboru**a pak vyberte **nový**. V **nový projekt**vyberte **skupiny prostředků Azure**.
4. Zadejte **název** pro nový projekt a pak vyberte **OK**.
5. V **vybrat šablonu Azure**, vyberte **Quickstart pro Azure Stack** z rozevíracího seznamu.
6. Vyberte **101-create-storage-account**a pak vyberte **OK**.
7. V novém projektu, rozbalte **šablony** uzel v **Průzkumníka řešení** zobrazíte dostupné šablony.
8. V **Průzkumníka řešení**, vyberte název projektu a pak vyberte **nasadit**. Vyberte **nové nasazení**.
9. V **nasadit do skupiny prostředků**, použijte **předplatné** rozevíracího seznamu vyberte předplatné Microsoft Azure Stack.
10. Z **skupiny prostředků** seznamu vyberte existující skupinu prostředků nebo vytvořte novou.
11. Z **umístění skupiny prostředků** seznamu, vyberte umístění a pak vyberte **nasadit**.
12. V **upravit parametry**, zadejte hodnoty pro parametry (které se liší podle šablony) a pak vyberte **Uložit**.

## <a name="next-steps"></a>Další postup

* [Nasazení šablon pomocí příkazového řádku](azure-stack-deploy-template-command-line.md)
* [Vývoj šablon pro Azure Stack](azure-stack-develop-templates.md)
