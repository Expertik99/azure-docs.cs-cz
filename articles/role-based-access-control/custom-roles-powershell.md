---
title: Vytvoření vlastní role pomocí Azure Powershellu | Dokumentace Microsoftu
description: Zjistěte, jak vytvořit vlastní role řízení přístupu na základě rolí (RBAC) pomocí Azure Powershellu. Jedná se o seznam, vytvářet, aktualizovat a odstraňovat vlastní role.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: c8e5f34bb6b38a3f187d86a1ebc0c7019c7f1046
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37437014"
---
# <a name="create-custom-roles-using-azure-powershell"></a>Vytvoření vlastní role pomocí Azure Powershellu

Pokud [předdefinované role](built-in-roles.md) nesplňují konkrétní požadavky vaší organizace, můžete vytvořit své vlastní role. Tento článek popisuje, jak vytvořit a spravovat vlastní role pomocí Azure Powershellu.

## <a name="prerequisites"></a>Požadavky

Pokud chcete vytvořit vlastní role, budete potřebovat:

- Oprávnění k vytváření vlastních rolí, například [Vlastník](built-in-roles.md#owner) nebo [Správce přístupu uživatelů](built-in-roles.md#user-access-administrator)
- Místně nainstalovaný [Azure PowerShell](/powershell/azure/install-azurerm-ps)

## <a name="list-custom-roles"></a>Výpis vlastních rolí

K zobrazení seznamu rolí, které jsou k dispozici pro přiřazení v oboru, použijte [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) příkazu. Následující příklad vypíše všechny role, které jsou k dispozici pro přiřazení ve vybraném předplatném.

```azurepowershell
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

```Example
Name                                              IsCustom
----                                              --------
Virtual Machine Operator                              True
AcrImageSigner                                       False
AcrQuarantineReader                                  False
AcrQuarantineWriter                                  False
API Management Service Contributor                   False
...
```

Následující příklad zobrazí seznam právě vlastních rolí, které jsou k dispozici pro přiřazení ve vybraném předplatném.

```azurepowershell
Get-AzureRmRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom
```

```Example
Name                     IsCustom
----                     --------
Virtual Machine Operator     True
```

Pokud vybrané předplatné není v `AssignableScopes` role, nebudou uvedené vlastní roli.

## <a name="create-a-custom-role"></a>Vytvoření vlastní role

Chcete-li vytvořit vlastní roli, použijte [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) příkazu. Existují dva způsoby strukturování roli, pomocí `PSRoleDefinition` objektu nebo šablony JSON. 

### <a name="get-operations-for-a-resource-provider"></a>Získat operace pro poskytovatele prostředků.

Při vytváření vlastních rolí, je důležité vědět, všechny možné operace od poskytovatelů prostředků.
Můžete zobrazit seznam [operace poskytovatele prostředků](resource-provider-operations.md) nebo můžete použít [Get-AzureRMProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) příkazu získat tyto informace.
Pokud chcete zkontrolovat všechny operace, které jsou k dispozici pro virtuální počítače, použijte tento příkaz:

```azurepowershell
Get-AzureRMProviderOperation <operation> | FT OperationName, Operation, Description -AutoSize
```

```Example
PS C:\> Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation, Description -AutoSize

OperationName                                  Operation                                                      Description
-------------                                  ---------                                                      -----------
Get Virtual Machine                            Microsoft.Compute/virtualMachines/read                         Get the propertie...
Create or Update Virtual Machine               Microsoft.Compute/virtualMachines/write                        Creates a new vir...
Delete Virtual Machine                         Microsoft.Compute/virtualMachines/delete                       Deletes the virtu...
Start Virtual Machine                          Microsoft.Compute/virtualMachines/start/action                 Starts the virtua...
...
```

### <a name="create-a-custom-role-with-the-psroledefinition-object"></a>Vytvořit vlastní roli s objektem PSRoleDefinition

Při použití Powershellu k vytvoření vlastní roli, můžete použít jednu z [předdefinované role](built-in-roles.md) jako výchozí bod nebo je můžete začít úplně od začátku. První příklad v této části začíná předdefinovanou roli a potom přizpůsobí s více oprávnění. Upravit atributy pro přidání `Actions`, `NotActions`, nebo `AssignableScopes` a následně změny uložte novou roli.

V následujícím příkladu začíná [Přispěvatel virtuálních počítačů](built-in-roles.md#virtual-machine-contributor) předdefinovaná role, chcete-li vytvořit vlastní roli s názvem *virtuálního počítače operátor*. Nová role uděluje přístup pro všechny operace čtení z *Microsoft.Compute*, *Microsoft.Storage*, a *Microsoft.Network* poskytovatelů a uděluje přístup k prostředkům spuštění , restartování a monitorovat virtuální počítače. Vlastní roli je možné ve dvou předplatných.

```azurepowershell
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/00000000-0000-0000-0000-000000000000")
$role.AssignableScopes.Add("/subscriptions/11111111-1111-1111-1111-111111111111")
New-AzureRmRoleDefinition -Role $role
```

Následující příklad ukazuje další způsob, jak vytvořit *virtuálního počítače operátor* vlastní roli. Začne tím, že vytvoříte nový `PSRoleDefinition` objektu. Operace akce jsou uvedeny v `perms` proměnné a jsou nastaveny na `Actions` vlastnost. `NotActions` Načtením je hodnota nastavena `NotActions` z [Přispěvatel virtuálních počítačů](built-in-roles.md#virtual-machine-contributor) předdefinovaná role. Protože [Přispěvatel virtuálních počítačů](built-in-roles.md#virtual-machine-contributor) nemá žádný `NotActions`, tento řádek není potřeba, ale ukazuje, jak načíst informace z jiné role.

```azurepowershell
$role = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
$role.Name = 'Virtual Machine Operator 2'
$role.Description = 'Can monitor and restart virtual machines.'
$role.IsCustom = $true
$perms = 'Microsoft.Storage/*/read','Microsoft.Network/*/read','Microsoft.Compute/*/read'
$perms += 'Microsoft.Compute/virtualMachines/start/action','Microsoft.Compute/virtualMachines/restart/action'
$perms += 'Microsoft.Authorization/*/read','Microsoft.Resources/subscriptions/resourceGroups/read'
$perms += 'Microsoft.Insights/alertRules/*','Microsoft.Support/*'
$role.Actions = $perms
$role.NotActions = (Get-AzureRmRoleDefinition -Name 'Virtual Machine Contributor').NotActions
$subs = '/subscriptions/00000000-0000-0000-0000-000000000000','/subscriptions/11111111-1111-1111-1111-111111111111'
$role.AssignableScopes = $subs
New-AzureRmRoleDefinition -Role $role
```

### <a name="create-a-custom-role-with-json-template"></a>Vytvoření vlastní role pomocí šablony JSON

Šablonu JSON může sloužit jako zdroj definice pro vlastní roli. Následující příklad vytvoří vlastní roli, která umožňuje přístup pro čtení do úložiště a výpočetních prostředků, přístup k podpoře, a tuto roli přidá dva odběry. Vytvořte nový soubor `C:\CustomRoles\customrole1.json` v následujícím příkladu. Id by mělo být nastavené `null` na vytvoření počáteční role jako nové ID se vygeneruje automaticky. 

```json
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Přidejte roli pro předplatná, spusťte následující příkaz Powershellu:

```azurepowershell
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="update-a-custom-role"></a>Aktualizace vlastní role

Podobné jako vytvoření vlastní roli, můžete upravit stávající vlastní role pomocí `PSRoleDefinition` objektu nebo šablony JSON.

### <a name="update-a-custom-role-with-the-psroledefinition-object"></a>Aktualizovat vlastní roli s objektem PSRoleDefinition

Pokud chcete upravit vlastní roli, nejprve pomocí [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) příkaz načíst definici role. Za druhé proveďte požadované změny do definice role. Nakonec použijte [Set-AzureRmRoleDefinition](/powershell/module/azurerm.resources/set-azurermroledefinition) příkazu uložte upravené role definition.

Následující příklad přidá `Microsoft.Insights/diagnosticSettings/*` operace *virtuálního počítače operátor* vlastní roli.

```azurepowershell
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

```Example
PS C:\> $role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
PS C:\> $role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
PS C:\> Set-AzureRmRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}
```

Následující příklad přidá do přiřaditelnými obory z předplatného Azure *virtuálního počítače operátor* vlastní roli.

```azurepowershell
Get-AzureRmSubscription -SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
Set-AzureRmRoleDefinition -Role $role
```

```Example
PS C:\> Get-AzureRmSubscription -SubscriptionName Production3

Name     : Production3
Id       : 22222222-2222-2222-2222-222222222222
TenantId : 99999999-9999-9999-9999-999999999999
State    : Enabled

PS C:\> $role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
PS C:\> $role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
PS C:\> Set-AzureRmRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111,
                   /subscriptions/22222222-2222-2222-2222-222222222222}
```

### <a name="update-a-custom-role-with-a-json-template"></a>Aktualizovat vlastní roli pomocí šablony JSON

Pomocí předchozího šablony JSON, můžete snadno upravit stávající vlastní role pro přidání nebo odebrání akce. Aktualizovat šablonu JSON a přidejte další akci pro sítě, jak je znázorněno v následujícím příkladu. Definice uvedené v šabloně se nepoužije kumulativně existující definice, což znamená, že role se zobrazí přesně tak, jak jste zadali v šabloně. Také musíte aktualizovat Id pole s ID role. Pokud si nejste jistí, co je tato hodnota, můžete použít [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) rutiny pro získání těchto informací.

```json
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Pokud chcete aktualizovat existující role, spusťte následující příkaz Powershellu:

```azurepowershell
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>Odstranění vlastní role

Pokud chcete odstranit vlastní roli, použijte [Remove-AzureRmRoleDefinition](/powershell/module/azurerm.resources/remove-azurermroledefinition) příkazu.

Následující příklad odebere *virtuálního počítače operátor* vlastní roli.

```azurepowershell
Get-AzureRmRoleDefinition "Virtual Machine Operator"
Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Virtual Machine Operator"

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}

PS C:\> Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition

Confirm
Are you sure you want to remove role definition with name 'Virtual Machine Operator'.
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
```

## <a name="next-steps"></a>Další postup

- [Kurz: Vytvoření vlastní role pomocí Azure Powershellu](tutorial-custom-role-powershell.md)
- [Vlastní role v Azure](custom-roles.md)
- [Operace poskytovatele prostředků Azure Resource Manageru](resource-provider-operations.md)
