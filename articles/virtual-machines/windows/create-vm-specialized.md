---
title: Vytvoření virtuálního počítače s Windows ze specializovaného virtuálního pevného disku v Azure | Dokumentace Microsoftu
description: Vytvořte nový virtuální počítač Windows připojením specializované spravovaného disku jako disku s operačním systémem pomocí v modelu nasazení Resource Manageru.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: cynthn
ms.openlocfilehash: 34bfe7733c60337d6ab7d81c498d2fb0fd15e1fd
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338482"
---
# <a name="create-a-windows-vm-from-a-specialized-disk-using-powershell"></a>Vytvoření virtuálního počítače s Windows ze specializovaného disku s využitím Powershellu

Vytvoření nového virtuálního počítače připojením specializované spravovaného disku jako disku s operačním systémem. Specializovaného disku je kopie virtuálního pevného disku (VHD) ze stávajícího virtuálního počítače, který uchovává uživatelské účty, aplikace a další data o stavu z původního virtuálního počítače. 

Při vytvoření nového virtuálního počítače pomocí specializovaného virtuálního pevného disku, nový virtuální počítač zachová název počítače z původního virtuálního počítače. Další informace specifické pro počítač je také zachována, a v některých případech se tyto duplicitní informace může způsobit problémy. Mějte na jaké typy informací specifických pro počítač aplikací využívají při kopírování virtuálního počítače.

Máte několik možností:
* [Použití existujícího spravovaného disku](#option-1-use-an-existing-disk). To je užitečné, pokud máte virtuální počítač, který nepracuje správně. Můžete odstranit virtuální počítač opakované použití spravovaného disku k vytvoření nového virtuálního počítače. 
* [Nahrání virtuálního pevného disku](#option-2-upload-a-specialized-vhd) 
* [Kopie existujícího virtuálního počítače Azure pomocí funkce vytváření snímků](#option-3-copy-an-existing-azure-vm)

Můžete použít také na webu Azure portal [vytvoření nového virtuálního počítače z specializovaného VHD](create-vm-specialized-portal.md).

V tomto tématu se dozvíte, jak používat spravované disky. Pokud máte starší verzi nasazení, která vyžaduje použití účtu úložiště, najdete v článku [vytvoření virtuálního počítače ze specializovaného VHD v účtu úložiště](sa-create-vm-specialized.md)

## <a name="before-you-begin"></a>Než začnete
Pokud používáte PowerShell, ujistěte se, že máte nejnovější verzi modul AzureRM.Compute Powershellu. 

```powershell
Install-Module AzureRM -RequiredVersion 6.0.0
```
Další informace najdete v tématu [Správa verzí Azure Powershellu](/powershell/azure/overview).

## <a name="option-1-use-an-existing-disk"></a>Možnost 1: Použití existujícího disku

Pokud jste měli virtuální počítač, který jste odstranili a znovu použít disk s operačním systémem vytvořte nový virtuální počítač, použijte [Get-AzureRmDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermdisk?view=azurermps-6.8.1).

```powershell
$resourceGroupName = 'myResourceGroup'
$osDiskName = 'myOsDisk'
$osDisk = Get-AzureRmDisk `
-ResourceGroupName $resourceGroupName `
-DiskName $osDiskName
```
Teď můžete připojit tento disk jako disk s operačním systémem k [nový virtuální počítač](#create-the-new-vm).

## <a name="option-2-upload-a-specialized-vhd"></a>Možnost 2: Nahrání specializovaného virtuálního pevného disku

Můžete nahrát virtuální pevný disk ze specializovaného virtuálního počítače vytvoří místní virtualizace nástroj, jako je Hyper-V nebo virtuální počítač exportovat z jiného cloudu.

### <a name="prepare-the-vm"></a>Příprava virtuálního počítače
Pokud máte v úmyslu použít virtuální pevný disk jako – k vytvoření nového virtuálního počítače, ověřte následující kroky. 
  
  * [Příprava virtuálního pevného disku Windows k nahrání do Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Ne** generalizace virtuálního počítače pomocí nástroje Sysprep.
  * Odeberte všechny hostované virtualizačních nástrojů a agenty, kteří jsou nainstalováni na virtuálním počítači (např. nástroje VMware).
  * Zajistěte, aby že virtuální počítač je nakonfigurovaný na vyžádání jeho IP adresu a nastavení DNS prostřednictvím protokolu DHCP. Tím se zajistí, že server při spuštění získá IP adresu v rámci virtuální sítě. 


### <a name="get-the-storage-account"></a>Získat účet úložiště.
Budete potřebovat účet úložiště v Azure k ukládání nahraný virtuální pevný disk. Můžete použít existující účet úložiště nebo vytvořte novou. 

Pokud chcete zobrazit účty úložiště k dispozici, zadejte:

```powershell
Get-AzureRmStorageAccount
```

Pokud chcete použít existující účet úložiště, pokračujte [nahrát virtuální pevný disk](#upload-the-vhd-to-your-storage-account) oddílu.

Pokud potřebujete vytvořit účet úložiště, postupujte podle těchto kroků:

1. Budete potřebovat název skupiny prostředků, ve kterém by měl vytvořit účet úložiště. Pokud chcete zjistit všechny skupiny prostředků, které jsou ve vašem předplatném, zadejte:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    Chcete-li vytvořit skupinu prostředků s názvem *myResourceGroup* v *USA – západ* oblast, typ:

    ```powershell
    New-AzureRmResourceGroup `
       -Name myResourceGroup `
       -Location "West US"
    ```

2. Vytvoření účtu úložiště s názvem *mystorageaccount* v této skupině prostředků s použitím [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) rutiny:
   
    ```powershell
    New-AzureRmStorageAccount `
       -ResourceGroupName myResourceGroup `
       -Name mystorageaccount `
       -Location "West US" `
       -SkuName "Standard_LRS" `
       -Kind "Storage"
    ```

### <a name="upload-the-vhd-to-your-storage-account"></a>Nahrání virtuálního pevného disku do účtu úložiště 
Použití [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) rutiny k nahrání virtuálního pevného disku do kontejneru v účtu úložiště. Tento příklad nahraje soubor *myVHD.vhd* z `"C:\Users\Public\Documents\Virtual hard disks\"` na účet úložiště s názvem *mystorageaccount* v *myResourceGroup* skupinu prostředků. Soubor je uložen v kontejneru s názvem *mycontainer* a nový název souboru bude *myUploadedVHD.vhd*.

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $resourceGroupName `
   -Destination $urlOfUploadedVhd `
   -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


V případě úspěchu se získat odpověď bude vypadat nějak takto:

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

V závislosti na připojení k síti a velikost souboru virtuálního pevného disku tohoto příkazu může trvat dobu

### <a name="create-a-managed-disk-from-the-vhd"></a>Vytvoření spravovaného disku z virtuálního pevného disku

Vytvoření spravovaného disku ze specializovaného VHD v účtu úložiště pomocí [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Tento příklad používá **myOSDisk1** pro název disku umístí disku *Standard_LRS* úložiště a použije *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* jako identifikátor URI pro zdrojový virtuální pevný disk.

Vytvořte novou skupinu prostředků pro nový virtuální počítač.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location `
   -Name $destinationResourceGroup
```

Vytvoření nového disku operačního systému z nahraných virtuálního pevného disku. 

```powershell
$sourceUri = (https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd)
$osDiskName = 'myOsDisk'
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig -AccountType Standard_LRS  `
    -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-3-copy-an-existing-azure-vm"></a>Možnost 3: Zkopírování existujícího virtuálního počítače Azure

Můžete vytvořit kopii tohoto virtuálního počítače, že používá spravované disky podle pořízení snímku virtuálního počítače a pak pomocí tohoto snímku pro vytvoření nového spravovaného disku a nového virtuálního počítače.


### <a name="take-a-snapshot-of-the-os-disk"></a>Pořízení snímku disku s operačním systémem

Udělat snímek z a celý virtuální počítač (včetně všech disků) nebo pouze jedním diskem. Následující kroky ukazují, jak k vytvoření snímku jenom disk s operačním systémem virtuálního počítače pomocí [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) rutiny. 

Nastavení některých parametrů. 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

Získejte objekt virtuálního počítače.

```powershell
$vm = Get-AzureRmVM -Name $vmName `
   -ResourceGroupName $resourceGroupName
```
Získejte název disku operačního systému.

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName `
   -DiskName $vm.StorageProfile.OsDisk.Name
```

Vytvořte snímek konfigurace. 

 ```powershell
$snapshotConfig =  New-AzureRmSnapshotConfig `
   -SourceUri $disk.Id `
   -OsType Windows `
   -CreateOption Copy `
   -Location $location 
```

Snímek.

```powershell
$snapShot = New-AzureRmSnapshot `
   -Snapshot $snapshotConfig `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName
```


Pokud máte v plánu pomocí tohoto snímku rychle vytvořit virtuální počítač, který má být vysoké provádění, použijte parametr `-AccountType Premium_LRS` pomocí příkazu New-AzureRmSnapshot. Parametr vytvoří snímek tak, aby se ukládá jako spravovaný Disk úrovně Premium. Jsou dražší než Standard Managed Disks úrovně Premium. Proto ujistěte se, že opravdu potřebujete Premium před použitím parametru.

### <a name="create-a-new-disk-from-the-snapshot"></a>Vytvoření nového disku ze snímku

Vytvoření spravovaného disku ze snímku pomocí [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Tento příklad používá *myOSDisk* pro název disku.

Vytvořte novou skupinu prostředků pro nový virtuální počítač.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location `
   -Name $destinationResourceGroup
```

Nastavte název disku operačního systému. 

```powershell
$osDiskName = 'myOsDisk'
```

Vytvoření spravovaného disku.

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-the-new-vm"></a>Vytvoření nového virtuálního počítače 

Vytvoření sítě a další prostředky virtuálních počítačů pro nový virtuální počítač.

### <a name="create-the-subnet-and-vnet"></a>Vytvoření podsítě a virtuální sítě

Vytvořit virtuální síť a podsíť [virtuální sítě](../../virtual-network/virtual-networks-overview.md).

Vytvořte podsíť. Tento příklad vytvoří podsíť s názvem **mySubNet**, ve skupině prostředků **myDestinationResourceGroup**a nastaví předponu adresy podsítě na **10.0.0.0/24**.
   
```powershell
$subnetName = 'mySubNet'
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig `
   -Name $subnetName `
   -AddressPrefix 10.0.0.0/24
```

Vytvořte virtuální síť. V tomto příkladu nastaví název virtuální sítě bude **myVnetName**, umístění pro **USA – západ**a předpony adresy pro virtuální síť k **10.0.0.0/16**. 
   
```powershell
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork `
   -Name $vnetName -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -AddressPrefix 10.0.0.0/16 `
   -Subnet $singleSubnet
```    


### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Vytvořte skupinu zabezpečení sítě a pravidlo protokolu RDP
Abyste mohli pro přihlášení k virtuálnímu počítači pomocí protokolu RDP, budete muset mít pravidlo zabezpečení, které povoluje přístup protokolu RDP na portu 3389. Vzhledem k tomu, že virtuální pevný disk pro nový virtuální počítač byl vytvořen z existujícího specializovaného virtuálního počítače, můžete použít účet ze zdrojového virtuálního počítače pro protokol RDP.

V tomto příkladu nastaví název skupiny NSG na **myNsg** a názvu pravidlo protokolu RDP **myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup `
   -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -Name $nsgName -SecurityRules $rdpRule
    
```

Další informace o koncových bodů a pravidel NSG najdete v tématu [otevření portů k virtuálnímu počítači v Azure pomocí Powershellu](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="create-a-public-ip-address-and-nic"></a>Vytvoření veřejné IP adresy a síťové karty
Pokud chcete povolit komunikaci s virtuálním počítačem ve virtuální síti, budete potřebovat [veřejnou adresu IP](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) a síťové rozhraní.

Vytvoření veřejné IP adresy. V tomto příkladu je název veřejné IP adresy nastavena **myIP**.
   
```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress `
   -Name $ipName -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -AllocationMethod Dynamic
```       

Vytvořte síťové rozhraní V tomto příkladu je název síťové karty nastavený na **myNicName**.
   
```powershell
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName `
   -ResourceGroupName $destinationResourceGroup `
   -Location $location -SubnetId $vnet.Subnets[0].Id `
   -PublicIpAddressId $pip.Id `
   -NetworkSecurityGroupId $nsg.Id
```



### <a name="set-the-vm-name-and-size"></a>Nastavte název virtuálního počítače a velikost

V tomto příkladu nastaví na název virtuálního počítače *myVM* a velikost virtuálního počítače do *Standard_A2*.

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a>Přidání síťového rozhraní
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-the-os-disk"></a>Přidat disk s operačním systémem 

Přidat disk s operačním systémem pomocí konfigurace [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk). V tomto příkladu nastaví velikost disku, který se *128 GB* a připojí spravovaného disku jako *Windows* disk s operačním systémem.
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType Standard_LRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-the-vm"></a>Dokončete tento virtuální počítač 

Vytvoření virtuálního počítače pomocí [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)konfigurace, které jsme právě vytvořili.

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

Pokud tento příkaz byl úspěšný, zobrazí se výstup podobný tomuto:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a>Ověřte, že se vytvořil virtuální počítač
Nově vytvořený virtuální počítač by se měla zobrazit buď v [webu Azure portal](https://portal.azure.com)v části **Procházet** > **virtuálních počítačů**, nebo pomocí následujících příkazů Powershellu:

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a>Další postup
Přihlaste se k novému virtuálnímu počítači. Další informace najdete v tématu [jak se připojit a přihlaste se na virtuálním počítači Azure s Windows](connect-logon.md).

