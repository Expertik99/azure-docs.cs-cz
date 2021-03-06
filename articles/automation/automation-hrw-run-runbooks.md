---
title: Spouštění runbooků v Azure Automation Hybrid Runbook Worker
description: Tento článek obsahuje informace o spouštění runbooků v počítačích v místním datovém centru nebo poskytovatele cloudu s rolí pracovního procesu Hybrid Runbook Worker.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 07/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8f21457a63470b88e93ead97454f996cea38073a
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "43103764"
---
# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Spouštění runbooků v procesu Hybrid Runbook Worker

Není žádný rozdíl ve struktuře sad runbook, které běží v Azure Automation a ty, které běží v procesu Hybrid Runbook Worker. Sady Runbook, které můžete používat s jednotlivými pravděpodobně výrazně lišit i když od sady runbook obvykle cílení Hybrid Runbook Worker správě prostředků v místním počítači, samotný nebo s prostředky v místním prostředí, kde je nasazen, při sady runbook v Azure Automation obvykle správu prostředků v cloudu Azure.

Při vytváření sady runbook spouštět v procesu Hybrid Runbook Worker, upravíte a testovat sady runbook na počítači, který je hostitelem Hybrid worker. Hostitelský počítač má všechny moduly Powershellu a přístup k síti, které potřebujete ke správě a přístup k místním prostředkům. Jakmile sady runbook byla upravovat a testovat na počítač hybridního pracovního procesu, ho můžete nahrát do prostředí Azure Automation, kde je k dispozici ke spuštění v procesu Hybrid worker. Je důležité vědět, že úlohy spustit pod účtem místního systému pro windows nebo zvláštní uživatelský účet **nxautomation** v Linuxu, které můžou představovat drobné rozdíly při autorizaci sad runbook pro Hybrid Runbook Worker, měl by to být vzít do úvahy.

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a>Spuštění runbooku v procesu Hybrid Runbook Worker

[Spuštění Runbooku ve službě Azure Automation](automation-starting-a-runbook.md) popisuje různé způsoby spuštění sady runbook. Přidá funkce hybrid Runbook Worker **RunOn** možnost zadávat název skupině Hybrid Runbook Worker. Pokud je skupina zadaná, je sada runbook načíst a spustit pomocí jedné z pracovních procesů v této skupině. Pokud není tato možnost zadána, pak spustí se ve službě Azure Automation jako za normálních okolností.

Při spuštění sady runbook na portálu Azure portal, zobrazí se **spouštět** možnost, kde můžete vybrat **Azure** nebo **Hybrid Worker**. Pokud vyberete **Hybrid Worker**, pak můžete vybrat skupinu z rozevíracího seznamu.

Použití **RunOn** parametru. Můžete spustit sadu runbook s názvem Test-Runbook ve skupině Hybrid Worker Runbooku s názvem MyHybridGroup pomocí Windows Powershellu následující příkaz.

```azurepowershell-interactive
Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"
```

> [!NOTE]
> **RunOn** parametr byl přidán do **Start AzureAutomationRunbook** rutiny ve verzi 0.9.1 prostředí Azure PowerShell. Měli byste [stáhnout nejnovější verzi](https://azure.microsoft.com/downloads/) Pokud máte některou z dřívějších verzí nainstalované. Stačí tuto verzi nainstalovat na pracovní stanici, kde jsou spuštění runbooku z Powershellu. Není nutné k instalaci na počítač pracovního procesu, pokud máte v úmyslu spouštění sad runbook z tohoto počítače"

## <a name="runbook-permissions"></a>Oprávnění sady Runbook

Runbooky, které běží v procesu Hybrid Runbook Worker nelze použít stejnou metodu, která se obvykle používá pro runbooky ověřované pro prostředky Azure, protože přistupují prostředky mimo Azure. Sada runbook může poskytnout vlastní ověřování k místním prostředkům, nebo můžete zadat účet Spustit jako pro poskytnutí kontextu uživatele pro všechny sady runbook.

### <a name="runbook-authentication"></a>Ověřování Runbooků

Ve výchozím nastavení, runbooky spuštěné v kontextu účtu místního systému pro Windows a zvláštní uživatelský účet **nxautomation** pro Linux v místním počítači, tedy musí zadat své vlastní ověřování k prostředkům, které přistupují k .

Můžete použít [přihlašovacích údajů](automation-credentials.md) a [certifikát](automation-certificates.md) prostředky ve vašem runbooku pomocí rutiny, které umožňují zadat přihlašovací údaje, abyste je mohli ověřit vůči různým prostředkům. Následující příklad ukazuje část sady runbook, který restartuje počítač. Načte přihlašovací údaje z asset přihlašovacích údajů a název počítače z variabilní prostředek a potom pomocí rutiny Restart-Computer použije tyto hodnoty.

```powershell
$Cred = Get-AutomationPSCredential -Name "MyCredential"
$Computer = Get-AutomationVariable -Name "ComputerName"

Restart-Computer -ComputerName $Computer -Credential $Cred
```

Můžete taky využít [InlineScript](automation-powershell-workflow.md#inlinescript), což vám umožní spustit bloky kódu na jiném počítači pomocí přihlašovacích údajů, které jsou určené [PSCredential společný parametr](/powershell/module/psworkflow/about/about_workflowcommonparameters).

### <a name="runas-account"></a>Účet Spustit jako

Ve výchozím nastavení používá funkce Hybrid Runbook Worker místní systém pro Windows a zvláštní uživatelský účet **nxautomation** pro Linux ke spuštění sady runbook. Namísto toho, aby runbooky, které poskytují vlastní ověřování k místním prostředkům, můžete zadat **RunAs** účet pro skupinu hybridních pracovních procesů. Můžete zadat [asset přihlašovacích údajů](automation-credentials.md) , který má přístup k místním prostředkům, a všechny runbooky spuštěné pod tyto přihlašovací údaje při spuštění v procesu Hybrid Runbook Worker ve skupině.

Uživatelské jméno pro přihlašovací údaje, které musí být v jednom z následujících formátů:

* domain\username
* username@domain
* uživatelské jméno (pro účty místní vůči v místním počítači)

Následující postup použijte k určení účtu RunAs pro skupinu hybridních pracovních procesů:

1. Vytvoření [asset přihlašovacích údajů](automation-credentials.md) s přístupem k místním prostředkům.
2. Na webu Azure Portal otevřete účet Automation.
3. Vyberte **skupiny hybridních pracovních procesů** dlaždici a pak vyberte skupinu.
4. Vyberte **všechna nastavení** a potom **nastavení skupiny hybridních pracovních procesů**.
5. Změna **spustit jako** z **výchozí** k **vlastní**.
6. Vyberte přihlašovací údaje a klikněte na tlačítko **Uložit**.

### <a name="automation-run-as-account"></a>Účet Spustit jako služby Automation

Jako součást procesu automatické sestavení pro nasazení prostředků v Azure můžou vyžadovat přístup k místním systémům podpory ve vašem nasazení pořadí úloh nebo sadu kroků. Pro podporu ověřování proti Azure pomocí účtu spustit jako, budete muset nainstalovat certifikátu účtu spustit jako.

Následující Powershellový runbook *Export RunAsCertificateToHybridWorker*, exportuje certifikát spustit jako z vašeho účtu Azure Automation a soubory ke stažení a naimportuje do úložiště certifikátů místního počítače na hybridní pracovního procesu připojen k stejný účet. Po dokončení tohoto kroku, tak ověří, že se že pracovní proces můžete provádět ověření do Azure pomocí účtu spustit jako.

```azurepowershell-interactive
<#PSScriptInfo
.VERSION 1.0
.GUID 3a796b9a-623d-499d-86c8-c249f10a6986
.AUTHOR Azure Automation Team
.COMPANYNAME Microsoft
.COPYRIGHT
.TAGS Azure Automation
.LICENSEURI
.PROJECTURI
.ICONURI
.EXTERNALMODULEDEPENDENCIES
.REQUIREDSCRIPTS
.EXTERNALSCRIPTDEPENDENCIES
.RELEASENOTES
#>

<#
.SYNOPSIS
Exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.

.DESCRIPTION
This runbook exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.
Run this runbook in the hybrid worker where you want the certificate installed.
This allows the use of the AzureRunAsConnection to authenticate to Azure and manage Azure resources from runbooks running in the hybrid worker.

.EXAMPLE
.\Export-RunAsCertificateToHybridWorker

.NOTES
AUTHOR: Azure Automation Team
LASTEDIT: 2016.10.13
#>

# Generate the password used for this certificate
Add-Type -AssemblyName System.Web -ErrorAction SilentlyContinue | Out-Null
$Password = [System.Web.Security.Membership]::GeneratePassword(25, 10)

# Stop on errors
$ErrorActionPreference = 'stop'

# Get the management certificate that will be used to make calls into Azure Service Management resources
$RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"

# location to store temporary certificate in the Automation service host
$CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"

# Save the certificate
$Cert = $RunAsCert.Export("pfx",$Password)
Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose

Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
$SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

# Test that authentication to Azure Resource Manager is working
$RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection"

Connect-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $RunAsConnection.TenantId `
    -ApplicationId $RunAsConnection.ApplicationId `
    -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

Set-AzureRmContext -SubscriptionId $RunAsConnection.SubscriptionID | Write-Verbose

# List automation accounts to confirm Azure Resource Manager calls are working
Get-AzureRmAutomationAccount | Select-Object AutomationAccountName
```

> [!IMPORTANT]
> **Add-AzureRmAccount** je nyní alias pro **Connect-AzureRMAccount**. Při vyhledávání knihovny položky, pokud se nezobrazí **Connect-AzureRMAccount**, můžete použít **Add-AzureRmAccount**, nebo ve vašem účtu Automation můžete aktualizovat moduly.

Uložit *Export RunAsCertificateToHybridWorker* sady runbook na počítači s `.ps1` rozšíření. Importovat do účtu Automation a upravit sadu runbook změnit hodnotu proměnné `$Password` s vlastní heslo. Publikování a pak spustit sadu runbook, které cílí na skupiny Hybrid Worker, spouštět a ověření runbooků pomocí účtu spustit jako. Datový proud úlohy sestav se importovat certifikát do úložiště místního počítače a následuje po zadání několika řádků v závislosti na tom, kolik účtů Automation jsou definovány v rámci vašeho předplatného, a pokud je ověření úspěšné.

## <a name="job-behavior"></a>Chování úlohy

Úlohy jsou zpracovány mírně liší na procesy Hybrid Runbook Worker než při spuštění v Azure karantény. Jedním klíčovým rozdílem je, že neexistuje žádné omezení na dobu trvání úlohy na procesy Hybrid Runbook Worker. Spuštění sady Runbook v Azure izolovaných prostorů jsou omezena na 3 hodin z důvodu [spravedlivé sdílení](automation-runbook-execution.md#fair-share). Pokud máte sadu runbook dlouhotrvající chcete zajistit, že je odolný vůči možné restartovat, například pokud restartování počítače, který je hostitelem Hybrid worker. Pokud Hybrid worker hostitelský počítač restartován, všechny spuštěné úlohy runbooku se restartuje, od začátku nebo od posledního kontrolního bodu pro runbooky pracovních postupů Powershellu. Pokud více než 3krát restartování úlohy runbooku, pak je pozastavený.

## <a name="run-only-signed-runbooks"></a>Spustit pouze podepsané sady Runbook

Procesy hybrid Runbook Worker může být nakonfigurován pro spouštění jenom podepsané runbooky s určitou konfigurací. Následující část popisuje, jak nastavit procesy Hybrid Runbook Worker pro spuštění sady runbook podepsaný a podepsání vaší sady runbook.

> [!NOTE]
> Po nakonfigurování Hybrid Runbook Worker pro spuštění sady runbook jenom podepsané sady runbook, které mají **není** byl podepsán se nepovedlo se provést na pracovní proces.

### <a name="create-signing-certificate"></a>Vytvořit certifikát pro podpis

Následující příklad vytvoří certifikát podepsaný svým držitelem, který lze použít pro podepisování sady runbook. Ukázka vytvoří certifikát a vyexportuje ho. Certifikát se později importovat do procesy Hybrid Runbook Worker. Vrátí kryptografický otisk se také že to umožňuje později odkazovat na certifikát.

```powershell
# Create a self-signed certificate that can be used for code signing
$SigningCert = New-SelfSignedCertificate -CertStoreLocation cert:\LocalMachine\my `
                                        -Subject "CN=contoso.com" `
                                        -KeyAlgorithm RSA `
                                        -KeyLength 2048 `
                                        -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
                                        -KeyExportPolicy Exportable `
                                        -KeyUsage DigitalSignature `
                                        -Type CodeSigningCert


# Export the certificate so that it can be imported to the hybrid workers
Export-Certificate -Cert $SigningCert -FilePath .\hybridworkersigningcertificate.cer

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Retrieve the thumbprint for later use
$SigningCert.Thumbprint
```

### <a name="configure-the-hybrid-runbook-workers"></a>Konfigurovat pracovní procesy Hybrid Runbook Worker

Zkopírujte certifikát vytvořený pro každého procesu Hybrid Runbook Worker ve skupině. Spusťte následující skript pro import certifikátu a konfigurace procesu Hybrid Worker používat ověřování podpisu pro sady runbook.

```powershell
# Install the certificate into a location that will be used for validation.
New-Item -Path Cert:\LocalMachine\AutomationHybridStore
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\AutomationHybridStore

# Import the certificate into the trusted root store so the certificate chain can be validated
Import-Certificate -FilePath .\hybridworkersigningcertificate.cer -CertStoreLocation Cert:\LocalMachine\Root

# Configure the hybrid worker to use signature validation on runbooks.
Set-HybridRunbookWorkerSignatureValidation -Enable $true -TrustedCertStoreLocation "Cert:\LocalMachine\AutomationHybridStore"
```

### <a name="sign-your-runbooks-using-the-certificate"></a>Podepsat vaší sady Runbook pomocí certifikátu

S Hybrid Runbook nakonfigurován pro použití pracovních procesů pouze podepsaný sady runbook, musíte podepsat sady runbook, které se mají použít v procesu Hybrid Runbook Worker. Použijte následující ukázku prostředí PowerShell k podepisování svých runbooků.

```powershell
$SigningCert = ( Get-ChildItem -Path cert:\LocalMachine\My\<CertificateThumbprint>)
Set-AuthenticodeSignature .\TestRunbook.ps1 -Certificate $SigningCert
```

Když sada runbook obsahuje podpis, musíte importovat do účtu Automation a publikovat s blok signatury. Informace o importování sad runbook najdete v tématu [import runbooku ze souboru do Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).

## <a name="troubleshoot"></a>Řešení potíží

Pokud vaše sady runbook nejsou úspěšně dokončit, zkontrolujte na Průvodce odstraňováním potíží [selhání spuštění sady runbook](troubleshoot/hybrid-runbook-worker.md#runbook-execution-fails).

## <a name="next-steps"></a>Další postup

* Další informace o různých metodách, které můžete použít ke spuštění sady runbook najdete v tématu [spuštění Runbooku ve službě Azure Automation](automation-starting-a-runbook.md).
* Vysvětlení různé postupy pro práci se sadami runbook Powershellu a pracovní postup prostředí PowerShell ve službě Azure Automation pomocí textového editoru, najdete v tématu [úpravy sady Runbook ve službě Azure Automation](automation-edit-textual-runbook.md)
