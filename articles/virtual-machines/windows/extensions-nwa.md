---
title: "Rozšíření virtuálního počítače Azure sítě sledovacích procesů agenta pro Windows | Microsoft Docs"
description: "Nasaďte agenta sledovací proces sítě v systému Windows virtuálního počítače pomocí rozšíření virtuálního počítače."
services: virtual-machines-windows
documentationcenter: 
author: dennisg
manager: amku
editor: 
tags: azure-resource-manager
ms.assetid: 27e46af7-2150-45e8-b084-ba33de8c5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 68855e0070916dc672914fbc8ca3587a5d3c25f6
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/20/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a>Sítě rozšíření virtuálního počítače sledovacích procesů agenta pro Windows

## <a name="overview"></a>Přehled

[Azure sledovací proces sítě](../../network-watcher/network-watcher-monitoring-overview.md) je sítě výkonu monitorování, diagnostiku a analýzy služba, která umožňuje monitorování sítě Azure. Rozšíření sítě sledovacích procesů agenta virtuálního počítače není pro zachytávání síťových přenosů na vyžádání a další pokročilé funkce na virtuálních počítačích Azure.


Tento dokument podrobně popisuje podporované platformy a možnosti nasazení pro rozšíření sítě sledovacích procesů agenta virtuálního počítače pro systém Windows.

## <a name="prerequisites"></a>Požadavky

### <a name="operating-system"></a>Operační systém

Rozšíření sítě sledovacích procesů agenta pro Windows můžete spustit na Windows Server 2008 R2, 2012, 2012 R2 a 2016 uvolní. Nano Server není podporován.

### <a name="internet-connectivity"></a>Připojení k internetu

Některé funkce sítě sledovacích procesů agenta vyžaduje, aby cílový virtuální počítač připojen k Internetu. Bez možnosti navázat odchozí připojení sítě sledovacích procesů agenta nebude možné odeslat paket zachycení do svého účtu úložiště. Další podrobnosti najdete v tématu [sledovací proces sítě dokumentaci](../../network-watcher/network-watcher-monitoring-overview.md).

## <a name="extension-schema"></a>Rozšíření schématu

Následujícím kódu JSON znázorňuje schéma pro rozšíření sítě sledovacích procesů agenta. Rozšíření ani jeden z nich vyžaduje, ani podporuje, všechna nastavení, uživatelem zadané a spoléhá na jeho výchozí konfigurace.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentWindows",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Hodnoty vlastností

| Název | Hodnota nebo příklad |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Vydavatele | Microsoft.Azure.NetworkWatcher |
| type | NetworkWatcherAgentWindows |
| typeHandlerVersion | 1.4 |


## <a name="template-deployment"></a>Nasazení šablon

Můžete nasadit rozšíření virtuálního počítače Azure pomocí šablon Azure Resource Manager. Schéma JSON, které jsou popsané v předchozí části v šablonu Azure Resource Manager můžete použít ke spuštění rozšíření sítě sledovacích procesů agenta při nasazení šablony Azure Resource Manager.

## <a name="powershell-deployment"></a>Nasazení prostředí PowerShell

Použití `Set-AzureRmVMExtension` příkazu nasaďte rozšíření sítě sledovacích procesů agenta virtuálního počítače do existujícího virtuálního počítače:

```powershell
Set-AzureRmVMExtension `
  -ResourceGroupName "myResourceGroup1" `
  -Location "WestUS" `
  -VMName "myVM1" `
  -Name "networkWatcherAgent" `
  -Publisher "Microsoft.Azure.NetworkWatcher" `
  -Type "NetworkWatcherAgentWindows" `
  -TypeHandlerVersion "1.4"
```

## <a name="troubleshooting-and-support"></a>Řešení potíží a podpora

### <a name="troubleshooting"></a>Řešení potíží

Z portálu Azure a prostředí PowerShell můžete načítat data o stavu nasazení rozšíření. Pokud chcete zobrazit stav nasazení rozšíření pro daný virtuální počítač, spusťte následující příkaz modulu Azure PowerShell:

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

Výstupu spuštění rozšíření se zaznamenává soubory, které jsou v následujícím adresáři:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a>Podpora

Pokud potřebujete další pomoc v libovolném bodě v tomto článku, můžete v uživatelské příručce sledovací proces sítě dokumentaci nebo se obraťte na Azure odborníky [fórech MSDN Azure a Stack Overflow](https://azure.microsoft.com/en-us/support/forums/). Alternativně můžete soubor incidentu podpory Azure. Přejděte na [podporu Azure lokality](https://azure.microsoft.com/en-us/support/options/) a vyberte Get podpory. Informace o používání Azure podporovat, najdete v tématu [podporu Microsoft Azure – nejčastější dotazy](https://azure.microsoft.com/en-us/support/faq/).
