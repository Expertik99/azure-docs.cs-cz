---
title: Rychlý start – Vytvoření prvního kontejneru služby Azure Container Instances pomocí PowerShellu
description: V tomto rychlém startu pomocí Azure PowerShellu nasadíte kontejner Windows ve službě Azure Container Instances.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: quickstart
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 4a1d338304dbd5e2845768b7bf0273eed23af0ec
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38453562"
---
# <a name="quickstart-create-your-first-container-in-azure-container-instances"></a>Rychlý start: Vytvoření prvního kontejneru ve službě Azure Container Instances

Služba Azure Container Instances usnadňuje vytváření a správu kontejnerů Dockeru v Azure, aniž byste museli zřizovat virtuální počítače nebo používat službu vyšší úrovně. V tomto rychlém startu vytvoříte kontejner Windows v Azure a zveřejníte ho na internetu s použitím plně kvalifikovaného názvu domény. K dokončení této operace stačí jediný příkaz. Za chvíli se ve vašem prohlížeči zobrazí spuštěná aplikace:

![Aplikace nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][qs-powershell-01]

Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Pokud se rozhodnete nainstalovat a používat PowerShell místně, musíte použít modul Azure PowerShell verze 5.5 nebo novější. Verzi zjistíte spuštěním příkazu `Get-Module -ListAvailable AzureRM`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps). Pokud používáte PowerShell místně, je také potřeba spustit příkaz `Connect-AzureRmAccount` pro vytvoření připojení k Azure.

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Vytvořte skupinu prostředků Azure pomocí příkazu [New-AzureRmResourceGroup][New-AzureRmResourceGroup]. Skupina prostředků je logický kontejner, ve kterém se nasazují a spravují prostředky Azure.

 ```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Vytvoření kontejneru

Kontejner můžete vytvořit zadáním názvu, image Dockeru a skupiny prostředků Azure do rutiny [New-AzureRmContainerGroup][New-AzureRmContainerGroup]. Volitelně můžete kontejner zveřejnit na internetu s použitím popisku názvu DNS.

Spuštěním následujícího příkazu spusťte kontejner Nano Server se spuštěnou Internetovou informační službou (IIS). Hodnota `-DnsNameLabel` musí být jedinečná v rámci oblasti Azure, ve které instanci vytváříte, takže ji možná budete muset pro zajištění jedinečnosti upravit.

 ```azurepowershell-interactive
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -DnsNameLabel aci-demo-win
```

Během několika sekund byste měli obdržet odpověď na váš požadavek. Zpočátku je kontejner ve stavu **Vytváření**, ale během několika minut by se měl spustit. Stav nasazení můžete zkontrolovat pomocí rutiny [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

Ve výstupu rutiny se zobrazí stav zřizování kontejneru a jeho plně kvalifikovaný název domény a IP adresa:

```console
PS Azure:\> Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer


ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials :
RestartPolicy            : Always
IpAddress                : 52.226.19.87
DnsNameLabel             : aci-demo-win
Fqdn                     : aci-demo-win.eastus.azurecontainer.io
Ports                    : {80}
OsType                   : Windows
Volumes                  :
State                    : Pending
Events                   : {}
```

Jakmile se **ProvisioningState** (Stav zřizování) clusteru změní na `Succeeded` (Úspěch), přejděte v prohlížeči na jeho plně kvalifikovaný název domény (`Fqdn`):

![Služba IIS nasazená pomocí služby Azure Container Instances zobrazená v prohlížeči][qs-powershell-01]

## <a name="clean-up-resources"></a>Vyčištění prostředků

Až s kontejnerem skončíte, odeberte ho pomocí rutiny [Remove-AzureRmContainerGroup][Remove-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste vytvořili instanci kontejneru Azure z image ve veřejném registru Docker Hub. Pokud si chcete sami sestavit image kontejneru a nasadit ji do služby Azure Container Instances z privátního registru kontejnerů Azure, pokračujte ke kurzu služby Azure Container Instances.

> [!div class="nextstepaction"]
> [Kurz služby Azure Container Instances](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png

<!-- LINKS -->
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/remove-azurermcontainergroup
