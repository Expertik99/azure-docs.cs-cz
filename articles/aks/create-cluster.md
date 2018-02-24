---
title: "Vytvoření clusteru Azure Container Service (AKS)"
description: "Vytvoření clusteru AKS pomocí rozhraní příkazového řádku nebo portálu Azure."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 02/12/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 37d6dfc0aa6b3e4fcd88a53e83a3a3d7f2157681
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/23/2018
---
# <a name="create-an-azure-container-service-aks-cluster"></a>Vytvoření clusteru Azure Container Service (AKS)

Clusteru Azure Container Service (AKS) lze vytvořit pomocí rozhraní příkazového řádku Azure nebo portálu Azure.

## <a name="azure-cli"></a>Azure CLI

Použití [vytvořit az aks] [ az-aks-create] příkaz k vytvoření clusteru AKS.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myAKSCluster
```

Jsou k dispozici následující možnosti `az aks create` příkaz.

| Argument | Popis | Požaduje se |
|---|---|:---:|
| `--name` `-n` | Název prostředku pro spravovaný cluster. | ano |
| `--resource-group` `-g` | Název skupiny prostředků Azure Container Service. | ano |
| `--admin-username` `-u` | Uživatelské jméno pro virtuální počítače se systémem Linux.  Výchozí: azureuser. | ne |
| ` --client-secret` | Tajný klíč přidružený k hlavní službě. | ne |
| `--dns-name-prefix` `-p` | Předpona DNS pro clustery veřejnou ip adresu. | ne |
| `--generate-ssh-keys` | Pokud chybí, generovat SSH soubory veřejného a privátního klíče. | ne |
| `--kubernetes-version` `-k` | Verze Kubernetes používat pro vytváření clusteru, například '1.7.9' nebo '1.8.2'.  Výchozí: 1.7.7. | ne |
| `--no-wait` | Není počkejte na dokončení operace časově náročné. | ne |
| `--node-count` `-c` | Výchozí počet uzlů pro fondy uzlu.  Výchozí: 3. | ne |
| `--node-osdisk-size` | OsDisk velikost v GB fondu uzlu virtuálního počítače. | ne |
| `--node-vm-size` `-s` | Velikost virtuálního počítače.  Výchozí: Standard_D1_v2. | ne |
| `--service-principal` | Objekt zabezpečení služby použít k ověření clusteru. | ne |
| `--ssh-key-value` | SSH soubor klíče hodnotu nebo klíč cesta k souboru.  Default: ~/.ssh/id_rsa.pub. | ne |
| `--tags` | Místo oddělené značky v "klíčem [= value]" formátu. Použití s vymažete stávající značky. | ne |

## <a name="azure-portal"></a>Azure Portal

Pokyny k nasazení clusteru služby AKS pomocí portálu Azure, najdete v Azure Container Service (AKS) [Azure rychlý start portálu][aks-portal-quickstart]. 

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az_aks_create
[aks-portal-quickstart]: kubernetes-walkthrough-portal.md
