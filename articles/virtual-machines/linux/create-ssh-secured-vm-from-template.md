---
title: "Vytvoření virtuálního počítače s Linuxem v Azure ze šablony | Microsoft Docs"
description: "Jak používat 2.0 rozhraní příkazového řádku Azure k vytvoření virtuálního počítače s Linuxem ze šablony Resource Manageru"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 12/18/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a7bd5b8c0534a51c6b6c9e8871be513194d38788
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/19/2017
---
# <a name="how-to-create-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Jak vytvořit virtuální počítač s Linuxem pomocí šablony Azure Resource Manager
Tento článek ukazuje, jak rychle nasadit virtuální počítač s Linuxem (VM) pomocí šablony Azure Resource Manager a Azure CLI 2.0. K provedení těchto kroků můžete také využít [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md).


## <a name="templates-overview"></a>Přehled šablon
Šablony Azure Resource Manageru jsou soubory JSON, které definují, infrastruktury a konfigurace řešení Azure. Pomocí šablony můžete řešení opakovaně nasadit v průběhu životního cyklu a mít přitom jistotu, že se prostředky nasadí konzistentně. Další informace o formátu šablony a jak vytvořit, najdete v části [vytvoření vaší první šablony Azure Resource Manager](../../azure-resource-manager/resource-manager-create-first-template.md). Syntaxi JSON pro typy prostředků najdete v tématu [Definování prostředků v šablonách Azure Resource Manageru](/azure/templates/).


## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků
Skupina prostředků Azure je logický kontejner, ve kterém se nasazují a spravují prostředky Azure. Před virtuálního počítače je třeba vytvořit skupinu prostředků. Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupVM* v *eastus* oblasti:

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a>Vytvoření virtuálního počítače
Následující příklad vytvoří virtuální počítač z [této šablony Azure Resource Manageru](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) s [vytvořit nasazení skupiny az](/cli/azure/group/deployment#create). Je povoleno pouze ověřování SSH. Po zobrazení výzvy zadejte hodnotu vlastní veřejný klíč SSH, jako je například obsah *~/.ssh/id_rsa.pub*. Pokud potřebujete vytvořit dvojici klíčů SSH, přečtěte si [postup vytvoření a používání dvojici klíčů SSH pro virtuální počítače s Linuxem v Azure](mac-create-ssh-keys.md).

```azurecli
az group deployment create --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

V předchozím příkladu zadat šablonu uložené v Githubu. Můžete také stáhnout nebo vytvořit šablonu a zadejte místní cestu s `--template-file` parametr.


## <a name="connect-to-virtual-machine"></a>Připojení k virtuálnímu počítači
Chcete-li SSH k virtuálnímu počítači, získat veřejnou IP adresu s [az virtuálních počítačů zobrazit](/cli/azure/vm#show):

```azurecli
az vm show \
    --resource-group myResourceGroup \
    --name sshvm \
    --show-details \
    --query publicIps \
    --output tsv
```

Pak můžete SSH pro virtuální počítač jako normální. Zadejte vlastní veřejnou IP adresu z předchozí příkaz:

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a>Další kroky
V tomto příkladu jste vytvořili základní virtuální počítač s Linuxem. Pro další šablony správce prostředků, které zahrnují aplikační architektury nebo vytvořit složitější prostředí, přejděte [galerii šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/).