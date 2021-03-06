---
title: Podřízené runbooky ve službě Azure Automation
description: Popisuje různé metody pro spuštění sady runbook ve službě Azure Automation z jiného runbooku a sdílení informací mezi nimi.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 08/14/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 037c2714d146bd59b30573df874794342d743e03
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/05/2018
ms.locfileid: "43782228"
---
# <a name="child-runbooks-in-azure-automation"></a>Podřízené runbooky ve službě Azure Automation

To je osvědčený postup ve službě Azure Automation zapisovat opakovaně použitelné modulární runbooky se samostatnou funkcí, které mohou používat ostatní runbooky. Nadřazený runbook bude často volat jeden nebo víc runbooků k provedení požadované funkce. Existují dva způsoby, jak volat podřízené runbooky, a každá má významné rozdíly, které byste měli rozumět tak, aby bylo možné určit, která bude nejlepší pro různé scénáře.

## <a name="invoking-a-child-runbook-using-inline-execution"></a>Vyvolání podřízeného runbooku pomocí přiřazeného provedení

Chcete-li vyvolání runbooku přiřazeného z jiného runbooku, použijte název sady runbook a zadejte hodnoty jeho parametrů stejně, jako byste používali aktivitu nebo rutinu.  Všechny sady runbook v rámci stejného účtu Automation jsou k dispozici pro všechny ostatní pro použití tímto způsobem. Nadřazený runbook bude čekat na dokončení podřízeného runbooku před přechodem na další řádek a jakýkoliv výstup se vrátí přímo do nadřazeného objektu.

Když vyvoláte přiřazený runbook, spustí se ve stejné úloze jako nadřízený runbook. Nebudou žádné označení do historie úlohy podřízené sady runbook, který ji spustil. Jakékoli výjimky a žádné výstupní datový proud z podřízeného runbooku budou přidružené k nadřazenému. Výsledkem míň úloh a jejich jednodušší sledování a řešení potíží od veškeré výjimky vyvolané příkazem podřízené sady runbook a některé z jeho datového proudu výstupy jsou přidruženy k nadřazené úloze.

Při publikování runbooku musí všechny podřízené runbooky, které volá již publikována. Je to proto, že Azure Automation vytvoří přidružení se všemi podřízenými runbooky při kompilaci sady runbook. Pokud nejsou, nadřízený runbook správně publikuje se zobrazí, ale vygeneruje výjimku, když je spuštěno. Pokud k tomu dojde, můžete znovu publikovat nadřízený runbook aby bylo možné řádně se odkazovat na podřízené runbooky. Nemusíte znovu publikovat nadřízený runbook, pokud některý z podřízených runbooků se změnit, protože přidružení se již byly vytvořeny.

Parametry podřízeného runbooku s přiřazeným voláním můžou být jakéhokoli datového typu včetně složitých objektů a neexistuje žádná [serializace JSON](automation-starting-a-runbook.md#runbook-parameters) je při spuštění runbooku pomocí portálu Azure nebo se Rutina Start-AzureRmAutomationRunbook.

### <a name="runbook-types"></a>Typy runbooků

Typy, které můžete volat mezi sebou:

* A [Powershellového runbooku](automation-runbook-types.md#powershell-runbooks) a [grafické runbooky](automation-runbook-types.md#graphical-runbooks) můžete volat každou další (obojí jsou na základě prostředí PowerShell).
* A [runbook Powershellového Workflow](automation-runbook-types.md#powershell-workflow-runbooks) a sady runbook grafický Powershellový pracovní postup můžete volat každou další (obojí jsou na základě pracovního postupu Powershellu)
* Typy prostředí PowerShell a typy pracovních postupů Powershellu nelze volat mezi sebou a musí používat Start-AzureRmAutomationRunbook.

Při publikování pořadí věci:

* Publikovat záleží na pořadí, sad runbook pouze pro runbooky pracovního postupu Powershellu a grafický Powershellový pracovní postup.

Při volání pomocí přiřazeného provedení podřízeného runbooku grafický nebo pracovních postupů Powershellu, stačí použít název sady runbook.  Při volání podřízeného runbooku Powershellu, musíte spustit stejný název jako *.\\*  k určení, že skript je umístěn v místním adresáři.

### <a name="example"></a>Příklad:

Následující příklad popisuje vyvolání podřízeného testovacího runbooku, který přijímá tři parametry, komplexní objekt, celé číslo a logickou hodnotu. Výstup podřízeného runbooku se přiřadí k proměnné.  V takovém případě podřízeného runbooku se runbook pracovního postupu Powershellu.

```azurepowershell-interactive
$vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true
```

Toto je stejný příklad pomocí Powershellového runbooku jako podřízený.

```azurepowershell-interactive
$vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = .\PS-ChildRunbook.ps1 –VM $vm –RepeatCount 2 –Restart $true
```

## <a name="starting-a-child-runbook-using-cmdlet"></a>Spouštění podřízeného runbooku pomocí rutiny

Můžete použít [Start-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) ke spuštění sady runbook, jak je popsáno v [spuštění runbooku pomocí prostředí Windows PowerShell](automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). Existují dva způsoby použití pro tuto rutinu.  V jednom režimu rutina vrátí id úlohy poté, co je podřízená úloha vytvořena pro podřízený runbook.  V jiných režimu, které povolíte tak, že zadáte **– počkejte** parametr, rutina bude čekat až do podřízené úlohy dokončí a vrátí výstup z podřízeného runbooku.

Úloha z podřízeného runbooku spuštěná pomocí rutiny poběží v samostatné úloze nezávisle na nadřízeném runbooku. Výsledkem víc úloh než při vyvolání přiřazený runbook a provede jejich obtížnější sledování. Nadřazené můžete asynchronně spustit víc podřízených runbooků bez čekání na dokončení. Pro stejný druh paralelního provádění volání přiřazených runbooků muselo, by bylo potřeba použít nadřazená sada runbook [paralelní klíčové slovo](automation-powershell-workflow.md#parallel-processing).

Výstup z podřízených runbooků se nevrátíte do nadřízeného runbooku spolehlivě kvůli časování. Také určité proměnné jako $VerbosePreference $WarningPreference, a ostatními nemusí být předány podřízené runbooky. Pokud se chcete těmto potížím vyhnout, můžete vyvolat podřízené runbooky jako samostatné úlohy automatizace pomocí `Start-AzureRmAutomationRunbook` rutinu s `-Wait` přepnout. Nadřazená sada runbook to blokuje až do dokončení podřízeného runbooku.

Pokud nechcete, aby nadřízený runbook zablokuje na čekání, můžete vyvolat podřízeného runbooku pomocí `Start-AzureRmAutomationRunbook` rutiny bez `-Wait` přepnout. Pak je třeba použít `Get-AzureRmAutomationJob` čekání na dokončení úlohy a `Get-AzureRmAutomationJobOutput` a `Get-AzureRmAutomationJobOutputRecord` k načtení výsledků.

Parametry pro podřízený runbook spuštěný pomocí rutiny se poskytují jako zatřiďovací tabulka, jak je popsáno v [parametry Runbooku](automation-starting-a-runbook.md#runbook-parameters). Je možné jenom jednoduché datové typy. Pokud má runbook parametr komplexního datového typu, pak ho musí být volaný jako přiřazený.

Kontext předplatného může být ztraceny po vyvolání podřízené runbooky jako samostatné úlohy. V pořadí pro podřízený runbook k vyvolání rutiny Azure RM pro požadované předplatné Azure podřízené sady runbook musí ověřovat pro toto předplatné nezávisle na nadřazený runbook.

Pokud úlohy v rámci stejného účtu Automation pracovat s více předplatnými, když se vybere předplatné jedna úloha může změnit kontext aktuálně vybraném předplatném pro jiné úlohy, která obvykle není žádoucí. Pokud se chcete vyhnout tomuto problému, uložit výsledek `Select-AzureRmSubscription` vyvolání rutiny a předejte jí to do objektu `DefaultProfile` parametr všechny následné Azure RM rutiny u volání rozbočovače. Tento model je nutné konzistentně použít na všechny runbooky, které běží v rámci tohoto účtu Automation.

### <a name="example"></a>Příklad:

Následující příklad spouští podřízený runbook s parametry a potom počká na dokončení pomocí Start-AzureRmAutomationRunbook – počkejte parametru. Po dokončení se shromáždí výstup podřízeného runbooku z podřízeného runbooku. Chcete-li použít `Start-AzureRmAutomationRunbook`, je třeba ověřit ke svému předplatnému Azure.

```azurepowershell-interactive
# Connect to Azure with RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -DefaultProfile $AzureContext `
    –Parameters $params –wait
```

## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Porovnání metod pro volání podřízeného runbooku

Následující tabulka shrnuje rozdíly mezi dvěma způsoby volání runbooku z jiného runbooku.

|  | Vložené | Rutina |
|:--- |:--- |:--- |
| Úloha |Podřízené runbooky spuštěné ve stejné úloze jako nadřazený. |Pro podřízený runbook se vytvoří samostatná úloha. |
| Spouštěcí |Nadřízený runbook čeká na dokončení podřízeného runbooku před pokračováním. |Nadřízený runbook pokračuje hned po spuštění podřízeného runbooku *nebo* nadřízený runbook čeká na dokončení úlohy podřízené. |
| Výstup |Nadřazená sada runbook může získat výstup přímo z podřízeného runbooku. |Nadřízený runbook musí načíst výstup z úlohy podřízeného runbooku *nebo* nadřazená sada runbook může získat výstup přímo z podřízeného runbooku. |
| Parametry |Hodnoty pro parametry podřízeného runbooku se zadávají samostatně a můžete použít libovolného datového typu. |Hodnoty pro parametry podřízeného runbooku se musí zkombinovat do jedné zatřiďovací tabulky a můžou zahrnovat jenom jednoduché, pole a objektu datové typy této využívají serializaci JSON. |
| Účet Automation |Nadřízený runbook lze použít pouze podřízené sady runbook v rámci stejného účtu automation. |Pokud jste připojení k němu, můžete použít nadřazená sada runbook podřízeného runbooku z jakékoli účtu automation s předplatným Azure a dokonce i jiného předplatného. |
| Publikování |Podřízené sady runbook musí být zveřejněna před publikováním nadřízeného runbooku. |Podřízené sady runbook musí být publikovaný kdykoli před spuštěním nadřízeného runbooku. |

## <a name="next-steps"></a>Další postup

* [Spuštění runbooku ve službě Azure Automation](automation-starting-a-runbook.md)
* [Sada Runbook výstup a zprávy ve službě Azure Automation](automation-runbook-output-and-messages.md)
