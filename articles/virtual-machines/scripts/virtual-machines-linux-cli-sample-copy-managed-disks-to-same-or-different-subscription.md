---
title: Ukázkový skript Azure CLI – Kopírování (přesun) spravovaných disků do stejného nebo jiného předplatného | Microsoft Docs
description: Ukázkový skript Azure CLI – Kopírování (přesun) spravovaných disků do stejného nebo jiného předplatného
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: dfdbc0563810447a1a214356b5153afe38d9cf2f
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
ms.locfileid: "29846787"
---
# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a>Kopírování spravovaných disků do stejného nebo jiného předplatného pomocí rozhraní příkazového řádku

Tento skript zkopíruje spravovaný disk do stejného nebo jiného předplatného ve stejné oblasti. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Ukázkový skript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]


## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript k vytvoření nového spravovaného disku v cílovém předplatném pomocí ID zdrojového spravovaného disku používá následující příkazy. Každý příkaz v tabulce odkazuje na příslušnou část dokumentace.

| Příkaz | Poznámky |
|---|---|
| [az disk show](https://docs.microsoft.com/cli/azure/disk#az_disk_show) | Získá všechny vlastnosti spravovaného disku s použitím názvu a vlastností skupiny prostředků spravovaného disku. Vlastnost ID se použije ke zkopírování spravovaného disku do jiného předplatného.  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk#az_disk_create) | Zkopíruje spravovaný disk vytvořením nového spravovaného disku v jiném předplatném s použitím ID a názvu nadřazeného spravovaného disku.  |

## <a name="next-steps"></a>Další kroky

[Vytvoření virtuálního počítače ze spravovaného disku](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Další informace o Azure CLI najdete v [dokumentaci k Azure CLI](https://docs.microsoft.com/cli/azure).

Další ukázkové skripty rozhraní příkazového řádku pro virtuální počítače a spravované disky najdete v [dokumentaci k virtuálním počítačům Azure s Linuxem](../../app-service/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
