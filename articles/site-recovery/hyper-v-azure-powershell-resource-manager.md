---
title: Replikace virtuálních počítačů Hyper-V pomocí Powershellu a Azure Resource Manageru | Dokumentace Microsoftu
description: Automatizace replikace virtuálních počítačů Hyper-V do Azure pomocí Azure Site Recovery pomocí Powershellu a Azure Resource Manageru.
services: site-recovery
author: bsiva
manager: abhiag
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: bsiva
ms.openlocfilehash: 721bb725538b0b1f6eb0e7132b99e75491b6f969
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/10/2018
ms.locfileid: "42055430"
---
# <a name="set-up-disaster-recovery-to-azure-for-hyper-v-vms-using-powershell-and-azure-resource-manager"></a>Nastavení zotavení po havárii do Azure pro virtuální počítače Hyper-V pomocí Powershellu a Azure Resource Manageru

[Azure Site Recovery](site-recovery-overview.md) přispívá k vaší obchodní kontinuity podnikových procesů a strategie po havárii (BCDR) tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů Azure (virtuální počítače) a místních virtuálních počítačů a fyzických serverů.

Tento článek popisuje, jak replikovat virtuální počítače Hyper-V do Azure pomocí prostředí Windows PowerShell, společně s Azure Resource Manageru. Příklad použitý v tomto článku se dozvíte, jak replikovat jeden virtuální počítač běží na hostiteli Hyper-V, do Azure.

## <a name="azure-powershell"></a>Azure PowerShell

Prostředí Azure PowerShell poskytuje rutiny pro správu Azure pomocí prostředí Windows PowerShell. Rutiny prostředí PowerShell pro obnovení lokality pomocí Azure Powershellu pro Azure Resource Manageru umožňují ochrana a obnovení vašich serverů v Azure.

Nemusíte být odborné prostředí PowerShell pro použití v tomto článku, ale musíte pochopit základní koncepty, jako jsou moduly, rutiny a relace. Čtení [Začínáme s prostředím Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx), a [pomocí Azure Powershellu s Azure Resource Managerem](../powershell-azure-resource-manager.md).

> [!NOTE]
> Partneři Microsoftu v programu Cloud Solution Provider (CSP) můžete konfigurovat a spravovat ochranu zákazníků serverů za účelem jejich odpovídajících předplatných CSP (tenanta předplatná).
>
>

## <a name="before-you-start"></a>Než začnete
Ujistěte se, že jsou splněné tyto požadavky:

* A [Microsoft Azure](https://azure.microsoft.com/) účtu. Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/). Kromě toho si můžete přečíst o [ceny za Azure Site Recovery Manager](https://azure.microsoft.com/pricing/details/site-recovery/).
* Azure PowerShell 1.0. Informace o této verzi a její instalaci najdete v tématu [příkazového řádku Azure PowerShell 1.0.](https://azure.microsoft.com/)
* [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) a [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) moduly. Můžete získat nejnovější verze těchto modulů z [Galerie prostředí PowerShell](https://www.powershellgallery.com/)

Kromě toho konkrétní příklad popsaných v tomto článku má následující požadavky:

* Hostitel Hyper-V systémem Windows Server 2012 R2 nebo Microsoft Hyper-V Server 2012 R2 obsahující jeden nebo více virtuálních počítačů. Servery Hyper-V musí být připojené k Internetu, buď přímo nebo prostřednictvím proxy serveru.
* Virtuální počítače, které chcete replikovat musí být v souladu s [těchto nezbytných podmínkách](hyper-v-azure-support-matrix.md#replicated-vms).

## <a name="step-1-sign-in-to-your-azure-account"></a>Krok 1: Přihlaste se ke svému účtu Azure

1. Otevřete konzolu Powershellu a spusťte tento příkaz pro přihlášení ke svému účtu Azure. Vyvolá rutina na webové stránce vás vyzve k zadání přihlašovacích údajů k účtu: **Connect-AzureRmAccount**.
    - Alternativně můžete zahrnout přihlašovacích údajů k účtu jako parametr v **Connect-AzureRmAccount** rutiny, pomocí **-Credential** parametr.
    - Pokud jste partner CSP, který spolupracuje jménem klienta, určení zákazníka jako tenant, pomocí názvu primární doména tenanta nebo ID Tenanta. Příklad: **Connect-AzureRmAccount-Tenant "fabrikam.com"**
2. Přidružte předplatné, které chcete používat účet, protože účet může mít několik předplatných:

    `Select-AzureRmSubscription -SubscriptionName $SubscriptionName`

3. Ověřte, že vaše předplatné je registrovaný k použití Azure poskytovatelů služby Recovery Services a Site Recovery, pomocí následujících příkazů:

    `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

4. Ověřte, že ve výstupu tohoto příkazu **RegistrationState** je nastavena na **registrované**, můžete přejít ke kroku 2. V opačném případě byste měli zaregistrovat chybějící poskytovatele v rámci vašeho předplatného, spuštěním těchto příkazů:

    `Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery` `Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices`

5. Ověřte, že zprostředkovatele úspěšně zaregistrován, pomocí následujících příkazů:

    `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.

## <a name="step-2-set-up-the-vault"></a>Krok 2: Nastavení trezoru

1. Vytvořte skupinu prostředků Azure Resource Manageru, ve kterém chcete vytvořit trezor, nebo použijte existující skupinu prostředků. Následujícím způsobem vytvořte novou skupinu prostředků. $ResourceGroupName proměnná obsahuje název skupiny prostředků, kterou chcete vytvořit, a proměnná $Geo obsahuje oblast Azure, ve kterém chcete vytvořit skupinu prostředků (například "Brazílie – jih").

    `New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo` 

2. K získání seznamu skupin prostředků ve vašem předplatném, spusťte **Get-AzureRmResourceGroup** rutiny.
2. Vytvořte nový trezor služeb zotavení Azure následujícím způsobem:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Můžete načíst seznam existujících trezorů klíčů s **Get-AzureRmRecoveryServicesVault** rutiny.


## <a name="step-3-set-the-recovery-services-vault-context"></a>Krok 3: Nastavte kontext trezoru služby Recovery Services

Nastavte kontext trezoru následujícím způsobem:

`Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault`

## <a name="step-4-create-a-hyper-v-site"></a>Krok 4: Vytvoření do lokality Hyper-V

1. Vytvořte novou lokalitu Hyper-V následujícím způsobem:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

2. Tato rutina spustí úlohu Site Recovery Vytvořte lokalitu a vrátí objekt úlohy Site Recovery. Počkejte dokončení úlohy a ověřte, že úloha byla úspěšně dokončena.
3. Použití **rutiny Get-AzureRmSiteRecoveryJob**, k načtení objektu úlohy a zkontrolovat aktuální stav úlohy.
4. Vygenerování a stažení registračního klíče pro lokalitu, následujícím způsobem:

    ```
    $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path
    ```

5. Zkopírujte stažený klíč na hostitele Hyper-V. Budete potřebovat klíč k registraci hostitele Hyper-V do lokality.

## <a name="step-5-install-the-provider-and-agent"></a>Krok 5: Instalace zprostředkovatele a agenta

1. Stažení instalačního programu pro nejnovější verzi zprostředkovatele ze [Microsoft](https://aka.ms/downloaddra).
2. Spusťte instalační program na hostiteli theHyper-V.
3. Na konci instalace pokračujte krokem registrace.
4. Po zobrazení výzvy zadejte klíč stažený a dokončete registraci hostitele Hyper-V.
5. Ověřte, že je hostitel Hyper-V zaregistrovaný k lokalitě:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy"></a>Krok 6: Vytvoření zásady replikace

Než začnete, mějte na paměti, že zadaný účet úložiště musí být ve stejné oblasti Azure jako trezor a musí mít povolenou geografickou replikací.

1. Vytvoření zásady replikace takto:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

2. Kontrola vrácené úlohy k zajištění, že bude úspěšné vytvoření zásady replikace.

3. Načtěte kontejner ochrany, která odpovídá na web, následujícím způsobem:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. Kontejner ochrany přidružte k zásadě replikace takto:

     $Policy = get-AzureRmSiteRecoveryPolicy - FriendlyName $PolicyName $associationJob = začátku AzureRmSiteRecoveryPolicyAssociationJob – zásady $Policy - PrimaryProtectionContainer $protectionContainer

4. Čekání úlohy přidružení úspěšně dokončit.

## <a name="step-7-enable-vm-protection"></a>Krok 7: Povolte ochranu virtuálního počítače

1. Načtete entita ochrany, která odpovídá virtuální počítač, který chcete chránit následujícím způsobem:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. Ochranu virtuálního počítače. Pokud chráníte virtuální počítač má více než jeden disk připojený k němu, zadat disk s operačním systémem pomocí *OSDiskName* parametru.

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

3. Vyčkat, než dosáhne stavu chráněné po počáteční replikaci virtuálních počítačů. To může nějakou dobu trvat, v závislosti na faktorech jako objem dat se musí replikovat a dostupnou šířku pásma pro odesílání dat do Azure. Když chráněném stavu, je na místě, se stav úlohy a StateDescription aktualizují následujícím způsobem: 

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. Aktualizovat vlastnosti obnovení (jako je například velikost role virtuálního počítače) a síť Azure, ke kterému chcete připojit síťové rozhraní virtuálnímu počítači po převzetí služeb při selhání.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob

        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Krok 8: Spuštění testovací převzetí služeb při selhání
1. Spustíte testovací převzetí služeb takto:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. Ověření testovacího virtuálního počítače se vytvoří v Azure. Testovací převzetí služeb při selhání úlohy je pozastavený, po vytvoření testovacího virtuálního počítače v Azure.
3. Pokud chcete vyčistit a dokončete testovací převzetí služeb, spusťte:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a>Další postup
[Další informace](https://docs.microsoft.com/powershell/module/azurerm.siterecovery) o Azure Site Recovery pomocí rutin Powershellu pro Azure Resource Manager.
