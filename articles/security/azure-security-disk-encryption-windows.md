---
title: Aktivace Azure Disk Encryption pro virtuální počítače IaaS s Windows | Dokumentace Microsoftu
description: Tento článek obsahuje pokyny týkající se povolení Microsoft Azure Disk Encryption pro Windows virtuální počítače IaaS.
services: security
documentationcenter: na
author: mestew
manager: MBaldwin
ms.assetid: 8b3905c8-844f-4ec7-ad95-b386e9843053
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2018
ms.author: mstewart
ms.openlocfilehash: 2d43c906fa717b036382a119efbaa2551fe50b1f
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/24/2018
ms.locfileid: "42887885"
---
# <a name="enable-azure-disk-encryption-for-windows-iaas-vms"></a>Aktivace Azure Disk Encryption pro virtuální počítače IaaS s Windows 

Můžete povolit řadu scénářů šifrování disku a kroků může lišit v závislosti scénáři. Následující části se věnují scénáře podrobněji pro virtuální počítače IaaS s Windows. Než budete moct použít šifrování disku, [požadavky Azure Disk Encryption](../security/azure-security-disk-encryption-prerequisites.md) musíte provést. 

Přijmout [snímku](../virtual-machines/windows/snapshot-copy-managed-disk.md) nebo před disky jsou šifrované zálohovat. Zálohy Ujistěte se, že možnost obnovení je možné, pokud dojde k neočekávané chybě při šifrování. Virtuální počítače se spravovanými disky vyžadují zálohu, než dojde k šifrování. Po provedení zálohy můžete použít rutinu Set-AzureRmVMDiskEncryptionExtension zadáním parametru - skipVmBackup šifrování spravované disky. Další informace o tom, jak zálohování a obnovení šifrovaných virtuálních počítačů najdete v tématu [Azure Backup](../backup/backup-azure-vms-encryption.md) článku. 

>[!WARNING]
> Azure Disk Encryption musí být umístěné ve stejné oblasti služby Key Vault a virtuální počítače. Vytvoření a použití služby Key Vault, která je ve stejné oblasti jako virtuální počítač k šifrování. 


## <a name="bkmk_RunningWinVM"></a> Povoluje šifrování na existující nebo spouštění virtuálních počítačů IaaS s Windows
V tomto scénáři můžete povolit šifrování pomocí šablony, rutin prostředí PowerShell nebo příkazů rozhraní příkazového řádku. Následující části podrobněji popisují jak povolit Azure Disk Encryption. 

>[!IMPORTANT]
 >Je nezbytně nutné k snímku a/nebo zálohování spravovaného disku na základě instance virtuálního počítače mimo a před povolením Azure Disk Encryption. Snímek spravovaného disku může být přijata z portálu, nebo [Azure Backup](../backup/backup-azure-vms-encryption.md) lze použít. Zálohy Ujistěte se, že možnost obnovení je možné v případě jakékoli došlo k neočekávané chybě při šifrování. Po provedení zálohy rutinu Set-AzureRmVMDiskEncryptionExtension slouží k šifrování zadáním parametru - skipVmBackup spravované disky. Příkaz Set-AzureRmVMDiskEncryptionExtension selže ve virtuálních počítačích spravovaných disků na základě dokud provedl zálohování a tento parametr nebyl zadán. 
>
>Šifrování nebo zakázáním šifrování může způsobit restartování virtuálního počítače. 
>

### <a name="bkmk_RunningWinVMPSH"></a> Povoluje šifrování na existující nebo spouštění virtuálních počítačů pomocí Azure Powershellu 
Použití [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) rutina pro povolení šifrování u spuštěného virtuálního počítače IaaS v Azure. 

-  **Šifrování virtuálního počítače s běžící:** níže uvedený skript inicializuje proměnných a spustí rutinu Set-AzureRmVMDiskEncryptionExtension. Skupinu prostředků, virtuální počítač a trezor klíčů by již byly vytvořeny jako požadavky. Nahraďte hodnoty MySecureRg MySecureVM a MySecureVault.

     ```azurepowershell-interactive
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;
    ```
- **Šifrování pomocí KEK spuštěného virtuálního počítače:** 

     ```azurepowershell-interactive
     $rgName = 'MySecureRg';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;

     ```
     
   >[!NOTE]
   > Syntaxe pro hodnotu parametru disk šifrování – trezor klíčů je úplný identifikátor řetězce: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Syntaxe pro hodnoty parametru klíč šifrovacího klíče je úplný identifikátor URI klíče KEK jako v: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Ověřte, disky jsou šifrované:** Pokud chcete zkontrolovat stav šifrování virtuálního počítače IaaS, použijte [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus) rutiny. 
     ```azurepowershell-interactive
     Get-AzureRmVmDiskEncryptionStatus -ResourceGroupName 'MySecureRg' -VMName 'MySecureVM'
     ```
    
- **Zakázat šifrování disku:** můžete zakázat šifrování, použijte [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption) rutiny. 
     ```azurepowershell-interactive
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```

### <a name="bkmk_RunningWinVMCLI"></a>Povoluje šifrování na existující nebo spouštění virtuálních počítačů pomocí Azure CLI
Použití [az vm encryption povolit](/cli/azure/vm/encryption#az-vm-encryption-enable) příkaz, který povoluje šifrování na spuštěný virtuální počítač IaaS v Azure.

-  **Šifrování spuštěného virtuálního počítače:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type [All|OS|Data]
     ```

- **Šifrování pomocí KEK spuštěného virtuálního počítače:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault" --key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type [All|OS|Data]
     ```

     >[!NOTE]
     > Syntaxe pro hodnotu parametru disk šifrování – trezor klíčů je úplný identifikátor řetězce: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name] </br> Syntaxe pro hodnoty parametru klíč šifrovacího klíče je úplný identifikátor URI klíče KEK jako v: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

- **Ověřte, disky jsou šifrované:** Pokud chcete zkontrolovat stav šifrování virtuálního počítače IaaS, použijte [az vm encryption show](/cli/azure/vm/encryption#az-vm-encryption-show) příkazu. 

     ```azurecli-interactive
     az vm encryption show --name "MySecureVM" --resource-group "MySecureRg"
     ```

- **Zakázat šifrování:** zakázat šifrování, použijte [az vm encryption zakázat](/cli/azure/vm/encryption#az-vm-encryption-disable) příkazu. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
 
 > [!NOTE]
 >Je nezbytně nutné k snímku a/nebo zálohování spravovaného disku na základě instance virtuálního počítače mimo a před povolením Azure Disk Encryption. Snímek spravovaného disku může být přijata z portálu, nebo [Azure Backup](../backup/backup-azure-vms-encryption.md) lze použít. Zálohy Ujistěte se, že možnost obnovení je možné v případě jakékoli došlo k neočekávané chybě při šifrování. Po provedení zálohy rutinu Set-AzureRmVMDiskEncryptionExtension slouží k šifrování zadáním parametru - skipVmBackup spravované disky. Tento příkaz se nezdaří ve virtuálních počítačích spravovaných disků na základě, dokud byly provedeny zálohy a byl zadán tento parametr. 
>
>Šifrování nebo zakázáním šifrování může způsobit restartování virtuálního počítače. 

### <a name="bkmk_RunningWinVMwRM"> </a>Pomocí šablony Resource Manageru
Můžete povolit šifrování disku v existující nebo běžící virtuální počítače IaaS s Windows v Azure s použitím [šablony Resource Manageru k šifrování spuštěného virtuálního počítače Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm-without-aad).


1. Na šablony Azure pro rychlý start, klikněte na tlačítko **nasadit do Azure**.

2. Vyberte předplatné, skupinu prostředků, umístění, nastavení, právní podmínky a smlouvy. Klikněte na tlačítko **nákupní** chcete povolit šifrování na existující nebo spuštěného virtuálního počítače IaaS.

V následující tabulce jsou uvedeny parametry šablony Resource Manageru pro existující nebo spouštění virtuálních počítačů:

| Parametr | Popis |
| --- | --- |
| vmName | Název virtuálního počítače ke spuštění operace šifrování. |
| keyVaultName | Název, který klíč Bitlockeru, musí být nahrán do trezoru klíčů. Můžete ho získat pomocí rutiny `(Get-AzureRmKeyVault -ResourceGroupName <MyResourceGroupName>). Vaultname` nebo příkazového řádku Azure "az keyvault seznamu--resource-group"MySecureGroup" |ConvertFrom-JSON.|
| keyVaultResourceGroup | Název skupiny prostředků obsahující trezor klíčů|
|  KeyEncryptionKeyURL | Adresa URL šifrovací klíč klíče, který se používá k šifrování vygenerovaný klíč Bitlockeru. Tento parametr je nepovinný, pokud vyberete **nokek** v rozevíracím seznamu UseExistingKek. Pokud vyberete **kek** v rozevíracím seznamu UseExistingKek, je nutné zadat _keyEncryptionKeyURL_ hodnotu. |
| VolumeType | Typ svazku, který provádí operace šifrování na. Platné hodnoty jsou _OS_, _Data_, a _všechny_. 
| forceUpdateTag | Pokaždé, když se operace musí být vynutit spuštění se předá jedinečnou hodnotu jako identifikátor GUID. |
| resizeOSDisk | Oddíl operačního systému velikost tak, aby obsadily úplný operační systém virtuálního pevného disku před rozdělením systémový svazek. |
| location | Umístění pro všechny prostředky. |

## <a name="encrypt-virtual-machine-scale-sets"></a>Šifrování škálovací sady virtuálních počítačů
[Škálovací sady virtuálních počítačů Azure](../virtual-machine-scale-sets/overview.md) umožňují vytvářet a spravovat skupiny identických, virtuálních počítačů s vyrovnáváním zatížení. Počet instancí virtuálních počítačů se může automaticky zvyšovat nebo snižovat s ohledem na požadavky nebo definovaný plán. K šifrování škálovací sady virtuálních počítačů pomocí Azure Powershellu nebo rozhraní příkazového řádku.

###  <a name="encrypt-virtual-machine-scale-sets-with-azure-powershell"></a>Šifrování virtuálního počítače škálovací sady pomocí Azure Powershellu
Použití [Set-AzureRmVmssDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmssdiskencryptionextension) rutiny, které chcete povolit šifrování na škálovací sadu virtuálních počítačů s Windows.

-  **Šifrování běžící škálovací sady virtuálních počítačů**:
    ```azurepowershell-interactive
     $rgName= "MySecureRg";
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgName;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId;


-  **Encrypt a running virtual machine scale set using KEK to wrap the key**:
    ```azurepowershell-interactive
     $rgName= "MySecureRg";
     $VmssName = "MySecureVmss";
     $KeyVaultName= "MySecureVault";
     $keyEncryptionKeyName = "MyKeyEncryptionKey";
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgName;
     $DiskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     Set-AzureRmVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $VmssName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId;
    ```

- **Načíst stav šifrování pro škálovací sadu virtuálních počítačů:** použití [Get-AzureRmVmssVMDiskEncryption](/powershell/module/azurerm.compute/get-azurermvmssvmdiskencryption) rutiny.
    
    ```azurepowershell-interactive
    get-AzureRmVmssVMDiskEncryption -ResourceGroupName "MySecureRG" -VMScaleSetName "MySecureVmss"
    ```

- **Zakažte šifrování u škálovací sady virtuálních počítačů**: použijte [Disable-AzureRmVmssDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmssdiskencryption) rutiny. 

    ```azurepowershell-interactive
    Disable-AzureRmVmssDiskEncryption -ResourceGroupName "MySecureRG" -VMScaleSetName "MySecureVmss"
    ```
###  <a name="encrypt-virtual-machine-scale-sets-with-azure-cli"></a>Šifrování virtuálního počítače škálovací sady pomocí Azure CLI
Použití [povolit šifrování az vmss](/cli/azure/vmss/encryption#az-vmss-encryption-enable) chcete povolit šifrování na škálovací sadu virtuálních počítačů s Windows. Pokud nastavíte zásady upgradu pro škálovací sady na ruční, spusťte šifrování s [az vmss update-instances](/cli/azure/vmss#az-vmss-update-instances). 

-  **Šifrování běžící škálovací sady virtuálních počítačů**
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MySecureRG" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" 
    ```

-  **Šifrování spuštěného virtuálního počítače škálovací sady s použitím KEK zabalit klíč**
    ```azurecli-interactive
     az vmss encryption enable --resource-group "MySecureRG" --name "MySecureVmss" --disk-encryption-keyvault "MySecureVault" --key-encryption-key "MyKEK" --key-encryption-keyvault "MySecureVault" 

     ```
- **Načíst stav šifrování pro škálovací sadu virtuálních počítačů:** použití [az vmss encryption show](/cli/azure/vmss/encryption#az-vmss-encryption-show)

    ```azurecli-interactive
     az vmss encryption show --resource-group "MySecureRG" --name "MySecureVmss"
    ```

- **Zakažte šifrování u škálovací sady virtuálních počítačů**: použití [zakázat šifrování az vmss](/cli/azure/vmss/encryption#az-vmss-encryption-disable)
    ```azurecli-interactive
     az vmss encryption disable --resource-group "MySecureRG" --name "MySecureVmss"
    ```

## <a name="bkmk_VHDpre"> </a>Nové virtuální počítače IaaS vytvořené z šifrované zákazníka virtuální pevný disk, šifrovacích klíčů
V tomto scénáři můžete povolit šifrování pomocí rutin prostředí PowerShell nebo příkazů rozhraní příkazového řádku. 

Postupujte podle pokynů v dodatku k přípravě předem šifrované imagí, které lze použít v Azure. Po vytvoření image můžete použít kroky v další části vytvořit šifrovaný virtuální počítač Azure.

* [Příprava virtuálního pevného disku předem šifrované Windows](azure-security-disk-encryption-appendix.md#bkmk_preWin)
* [Připravit předem šifrované linuxového virtuálního pevného disku](azure-security-disk-encryption-appendix.md#bkmk_preLinux)

>[!IMPORTANT]
 >Je nezbytně nutné k snímku a/nebo zálohování spravovaného disku na základě instance virtuálního počítače mimo a před povolením Azure Disk Encryption. Snímek spravovaného disku může být přijata z portálu, nebo [Azure Backup](../backup/backup-azure-vms-encryption.md) lze použít. Zálohy Ujistěte se, že možnost obnovení je možné v případě jakékoli došlo k neočekávané chybě při šifrování. Po provedení zálohy rutinu Set-AzureRmVMDiskEncryptionExtension slouží k šifrování zadáním parametru - skipVmBackup spravované disky. Příkaz Set-AzureRmVMDiskEncryptionExtension selže ve virtuálních počítačích spravovaných disků na základě dokud provedl zálohování a tento parametr nebyl zadán. 
>
>Šifrování nebo zakázáním šifrování může způsobit restartování virtuálního počítače. 


### <a name="bkmk_VHDprePSH"> </a> Šifrování virtuálních počítačů s předem šifrované virtuální pevné disky pomocí Azure Powershellu
Můžete povolit šifrování disku na virtuální pevný disk šifrovaný pomocí rutiny prostředí PowerShell [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk#examples). Následující příklad obsahuje některé společné parametry. 

```azurepowershell-interactive
$VirtualMachine = New-AzureRmVMConfig -VMName "MySecureVM" -VMSize "Standard_A1"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name "SecureOSDisk" -VhdUri "os.vhd" Caching ReadWrite -Windows -CreateOption "Attach" -DiskEncryptionKeyUrl "https://mytestvault.vault.azure.net/secrets/Test1/514ceb769c984379a7e0230bddaaaaaa" -DiskEncryptionKeyVaultId "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mytestvault"
New-AzureRmVM -VM $VirtualMachine -ResouceGroupName "MySecureRG"
```

## <a name="enable-encryption-on-a-newly-added-data-disk"></a>Povoluje šifrování na nově přidaných datového disku
Je možné [přidejte nový disk k virtuálnímu počítači s Windows pomocí Powershellu](../virtual-machines/windows/attach-disk-ps.md), nebo [prostřednictvím webu Azure portal](../virtual-machines/windows/attach-managed-disk-portal.md). 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-powershell"></a>Povoluje šifrování na nově přidaný disk pomocí Azure Powershellu
 Při použití Powershellu k šifrování nový disk pro virtuální počítače s Windows, musí být zadaný novou verzi pořadí. Pořadí verze musí být jedinečný. Níže uvedený skript vytvoří identifikátor GUID verze pořadí. V některých případech může být disk nově přidaná data automaticky šifrovat pomocí rozšíření Azure Disk Encryption. Pokud k tomu dojde, doporučujeme znovu spustit rutinu Set-AzureRmVmDiskEncryptionExtension s novou verzí pořadí.
 

-  **Šifrování virtuálního počítače s běžící:** níže uvedený skript inicializuje proměnných a spustí rutinu Set-AzureRmVMDiskEncryptionExtension. Skupinu prostředků, virtuální počítač a trezor klíčů by již byly vytvořeny jako požadavky. Nahraďte hodnoty MySecureRg MySecureVM a MySecureVault. Parametr - VolumeType je nastaven na datové disky, nikoli disk s operačním systémem. 

     ```azurepowershell-interactive
      $sequenceVersion = [Guid]::NewGuid();
      $rgName = 'MySecureRg';
      $vmName = 'MySecureVM';
      $KeyVaultName = 'MySecureVault';
      $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
      $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
      $KeyVaultResourceId = $KeyVault.ResourceId;

      Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId  –SequenceVersion $sequenceVersion;
    ```
- **Šifrování spuštěného virtuálního počítače pomocí KEK:** budete muset přidat parametr - VolumeType Pokud šifrujete datové disky, nikoli disk s operačním systémem. 

     ```azurepowershell-interactive
     $sequenceVersion = [Guid]::NewGuid();
     $rgName = 'MySecureRg';
     $vmName = 'MyExtraSecureVM';
     $KeyVaultName = 'MySecureVault';
     $keyEncryptionKeyName = 'MyKeyEncryptionKey';
     $KeyVault = Get-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $rgname;
     $diskEncryptionKeyVaultUrl = $KeyVault.VaultUri;
     $KeyVaultResourceId = $KeyVault.ResourceId;
     $keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $KeyVaultName -Name $keyEncryptionKeyName).Key.kid;

     Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgname -VMName $vmName -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $KeyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $KeyVaultResourceId –SequenceVersion $sequenceVersion;

     ```

    >[!NOTE]
    > Syntaxe pro hodnotu parametru disk šifrování – trezor klíčů je úplný identifikátor řetězce: / subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br> Syntaxe pro hodnoty parametru klíč šifrovacího klíče je úplný identifikátor URI klíče KEK jako v: https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id] 

### <a name="enable-encryption-on-a-newly-added-disk-with-azure-cli"></a>Povoluje šifrování na nově přidaný disk pomocí Azure CLI
 Pomocí příkazu Azure CLI automaticky poskytne nové verze pořadí, při spuštění příkazu povolit šifrování. 
-  **Šifrování spuštěného virtuálního počítače:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault "MySecureVault" --volume-type "Data"
     ```

- **Šifrování pomocí KEK spuštěného virtuálního počítače:**

     ```azurecli-interactive
     az vm encryption enable --resource-group "MySecureRg" --name "MySecureVM" --disk-encryption-keyvault  "MySecureVault"--key-encryption-key "MyKEK_URI" --key-encryption-keyvault "MySecureVaultContainingTheKEK" --volume-type "Data"
     ```


## <a name="disable-encryption"></a>Zakázat šifrování
Můžete zakázat šifrování pomocí Azure Powershellu, rozhraní příkazového řádku Azure nebo pomocí šablony Resource Manageru. 

- **Zakázat disk encryption pomocí Azure Powershellu:** můžete zakázat šifrování, použijte [Disable-AzureRmVMDiskEncryption](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption) rutiny. 
     ```azurepowershell-interactive
     Disable-AzureRmVMDiskEncryption -ResourceGroupName 'MySecureRG' -VMName 'MySecureVM'
     ```

- **Zakázat šifrování pomocí Azure CLI:** zakázat šifrování, použijte [az vm encryption zakázat](/cli/azure/vm/encryption#az-vm-encryption-disable) příkazu. 
     ```azurecli-interactive
     az vm encryption disable --name "MySecureVM" --resource-group "MySecureRg" --volume-type [ALL, DATA, OS]
     ```
- **Zakážete šifrování pomocí šablony Resource Manageru:** 

    1. Klikněte na tlačítko **nasadit do Azure** z [zakázat šifrování disku ve spuštění virtuálního počítače s Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm-without-aad) šablony.
    2. Vyberte předplatné, skupinu prostředků, umístění, virtuálního počítače, typ svazku, právní podmínky a smlouvy.
    3.  Klikněte na tlačítko **nákupní** můžete zakázat šifrování disků spuštěného virtuálního počítače Windows. 

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Aktivace Azure Disk Encryption pro Linux](azure-security-disk-encryption-linux.md)
