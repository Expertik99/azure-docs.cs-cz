---
title: "Azure json zásady ukázkové – odepřít nástrojů přístup vrstvení pro účty úložiště | Microsoft Docs"
description: "Tato ukázková zásada json zakáže použití nástrojů přístup vrstvení pro účty úložiště blob."
services: azure-policy
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 
ms.service: azure-policy
ms.devlang: 
ms.topic: sample
ms.tgt_pltfrm: 
ms.workload: 
ms.date: 10/30/2017
ms.author: banders
ms.custom: mvc
ms.openlocfilehash: 47de69aaa7938a813ff9866cec6e492a8ef3b7a5
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
# <a name="deny-cool-access-tiering-for-storage-accounts"></a>Odepřít nástrojů přístup vrstvení pro účty úložiště

Tato zásada zakáže použití nástrojů přístup vrstvení pro účty úložiště blob.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Ukázka šablony

[!code-json[main](../../../policy-templates/samples/Storage/storage-account-access-tier/azurepolicy.json "Deny cool access tiering for storage accounts")]

Můžete nasadit pomocí této šablony [portál Azure](#deploy-with-the-portal), s [prostředí PowerShell](#deploy-with-powershell) nebo pomocí [rozhraní příkazového řádku Azure](#deploy-with-azure-cli).

## <a name="deploy-with-the-portal"></a>Nasazení s využitím portálu

[![Nasazení do Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FStorage%2Fstorage-account-access-tier%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>Nasazení s využitím PowerShellu

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = New-AzureRmPolicyDefinition -Name "storage-account-access-tier" -DisplayName "Deny cool access tiering for storage accounts" -description "Ensures there's no usage of cool access tiering for storage." -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Storage/storage-account-access-tier/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope>  -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>Vyčištění nasazení prostředí PowerShell

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>Nasazení pomocí rozhraní příkazového řádku Azure

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy definition create --name 'storage-account-access-tier' --display-name 'Deny cool access tiering for storage accounts' --description 'Ensures there's no usage of cool access tiering for storage.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Storage/storage-account-access-tier/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Storage/storage-account-access-tier/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "storage-account-access-tier"
```

### <a name="clean-up-azure-cli-deployment"></a>Vyčištění nasazení rozhraní příkazového řádku Azure

Spusťte následující příkaz pro odebrání skupiny prostředků, virtuální počítač a všechny související prostředky.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Další kroky

- Další ukázky šablony zásad Azure jsou [šablon pro Azure zásad](../json-samples.md).
