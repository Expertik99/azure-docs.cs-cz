---
title: zahrnout soubor
description: zahrnout soubor
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 05/21/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 5dc4ce00685c74b2974cf1bfb5e8606eb3063e8d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38740535"
---
Pokud chcete do skupiny prostředků přidat dvě značky, použijte příkaz [Set-AzureRmResourceGroup](/powershell/module/azurerm.resources/set-azurermresourcegroup):

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ Dept="IT"; Environment="Test" }
```

Předpokládejme, že chcete přidat třetí značku. Pokaždé, když použijete značky na prostředek nebo skupinu prostředků, přepíšete pro daný prostředek nebo skupinu prostředků existující značky. Pokud chcete novou značku přidat bez ztráty stávajících značek, je nutné stávající značky načíst, přidat novou značku a potom kolekci značek znovu použít:

```azurepowershell-interactive
# Get existing tags and add a new tag
$tags = (Get-AzureRmResourceGroup -Name myResourceGroup).Tags
$tags.Add("Project", "Documentation")

# Reapply the updated set of tags 
Set-AzureRmResourceGroup -Tag $tags -Name myResourceGroup
```

Prostředky nedědí značky ze skupiny prostředků. Vaše skupiny prostředků momentálně má tři značky, ale prostředky nemají žádné. Pokud chcete všechny značky ze skupiny prostředků použít na prostředky a zachovat existující značky u prostředků, které nejsou duplikáty, použijte tento skript:

```azurepowershell-interactive
# Get the resource group
$group = Get-AzureRmResourceGroup myResourceGroup

if ($group.Tags -ne $null) {
    # Get the resources in the resource group
    $resources = Get-AzureRmResource -ResourceGroupName $group.ResourceGroupName

    # Loop through each resource
    foreach ($r in $resources)
    {
        # Get the tags for this resource
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
        
        # If the resource has existing tags, add new ones
        if ($resourcetags)
        {
            foreach ($key in $group.Tags.Keys)
            {
                if (-not($resourcetags.ContainsKey($key)))
                {
                    $resourcetags.Add($key, $group.Tags[$key])
                }
            }

            # Reapply the updated tags to the resource 
            Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
        else
        {
            Set-AzureRmResource -Tag $group.Tags -ResourceId $r.ResourceId -Force
        }
    }
}
```

Další možností je použít pro prostředky značky ze skupiny prostředků a nezachovávat stávající značky:

```azurepowershell-interactive
# Get the resource group
$g = Get-AzureRmResourceGroup -Name myResourceGroup

# Find all the resources in the resource group, and for each resource apply the tags from the resource group
Get-AzureRmResource -ResourceGroupName $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
```

Pokud chcete sloučit několik hodnot v jedné značce, použijte řetězec JSON.

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ CostCenter="{`"Dept`":`"IT`",`"Environment`":`"Test`"}" }
```

Pokud chcete odebrat všechny značky, předejte prázdnou zatřiďovací tabulku.

```azurepowershell-interactive
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ }
```
