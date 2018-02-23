---
title: "Nejčastější dotazy k Azure Container Service"
description: "Poskytuje odpovědi na některé běžné otázky o Azure Container Service."
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/14/2018
ms.author: nepeters
ms.openlocfilehash: 59dceded1e72e6e0e3d1a2bb25ca63bd023a9d21
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/21/2018
---
# <a name="frequently-asked-questions-about-azure-container-service-aks"></a>Nejčastější dotazy o Azure Container Service (AKS)

Tento článek adresy časté otázky o Azure Container Service (AKS).

## <a name="which-azure-regions-provide-the-azure-container-service-aks-today"></a>Které oblasti Azure ještě dnes poskytovat Azure Container Service (AKS)?

- Střední Kanada 
- Východní Kanada 
- Střed USA 
- Východ USA 
- Jihovýchodní Asie 
- Západní Evropa 
- Západní USA 2 

## <a name="when-will-additional-regions-be-added"></a>Pokud bude přidán další oblasti? 

Další oblasti jsou přidány jako rostoucími.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Platí pro uzly agenta AKS aktualizace zabezpečení? 

Azure automaticky použije opravy zabezpečení pro uzly v clusteru na noční plánu. Ale jste zodpovědní za zajištění, že uzly se restartují podle potřeby. Máte několik možností pro provádění restartování uzlu:

- Ručně prostřednictvím portálu Azure nebo Azure CLI. 
- Upgradem AKS clusteru. Upgrade clusteru automaticky [cordon a vyprazdňování uzlů](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/), aby byly zálohování nejnovější Ubuntu Image. Můžete aktualizovat image operačního systému na uzly beze změny Kubernetes verze zadáním aktuální verze clusteru v `az aks upgrade`.
- Pomocí [Kured](https://github.com/weaveworks/kured), Kubernetes démon procesu restartování open source. Kured běží jako [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) a sleduje každý uzel pro přítomnost souboru, která udává, že je vyžadován restart. Potom ji orchestruje těchto restartování napříč clusterem, stejné cordon a vyprazdňování proces popsaný výše.

## <a name="do-you-recommend-customers-use-acs-or-aks"></a>Zákazníkům doporučujeme použití služby ACS nebo AKS? 

Když AKS zůstává ve verzi preview, ale doporučujeme vytvářet produkčních clusterů pomocí služby ACS Kubernetes nebo [acs modul](https://github.com/azure/acs-engine). AKS můžete použít pro testování konceptu nasazení a prostředí pro vývoj/testování.

## <a name="when-will-acs-be-deprecated"></a>Pokud bude ACS zastaralá? 

Služba ACS přestanou přibližně v okamžiku, kdy AKS stane všeobecné Budete mít dobu 12 měsíců po tomto datu migrace clusterů do AKS. Během 12 měsíců můžete spustit všechny operace služby ACS.

## <a name="does-aks-support-node-autoscaling"></a>Podporuje AKS automatické škálování uzlu? 

Automatické škálování uzlu není podporován, ale je plánovaná. Můžete chtít podívejte se na to open source [automatické škálování implementace][auto-scaler].

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>Podporuje AKS Kubernetes řízení přístupu na základě role (RBAC)?

Ne, RBAC v AKS aktuálně nepodporuje, ale bude brzy k dispozici.   

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Můžete nasadit AKS do mé existující virtuální síť?

Ne, to ještě není k dispozici ale bude brzy k dispozici.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Azure Key Vault integrovat AKS? 

Ne, není ale je plánovaná této integrace. Mezitím si můžete vyzkoušet následující řešení z [Hexadite][hexadite]. 

## <a name="can-i-run-windows-server-containers-on-aks"></a>Můžete spustit systém Windows Server kontejnery na AKS?

Ne, AKS neposkytuje aktuálně uzlů agent systémem Windows Server, proto nelze spustit systém Windows Server kontejnery. Pokud potřebujete spustit systém Windows Server kontejnery na Kubernetes v Azure, přejděte na téma [dokumentace pro modul služby acs](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/windows.md).

## <a name="why-are-two-resource-groups-created-with-aks"></a>Proč jsou dvě skupiny prostředků vytvořené pomocí AKS? 

Každé nasazení AKS zahrnuje dvě skupiny prostředků. První je vytvořené vámi a obsahuje pouze AKS prostředek. Zprostředkovatel prostředků AKS automaticky vytvoří druhý během nasazení s názvem, jako je *MC_myResourceGRoup_myAKSCluster_eastus*. Druhý skupina prostředků obsahuje všechny prostředky infrastruktury spojené s clusterem, jako jsou virtuální počítače, sítě a úložiště. Vytváří se ke zjednodušení vyčištění prostředků. 

Pokud vytváříte prostředky, které budou použity s AKS cluster, například účty úložiště nebo vyhrazené veřejné IP adresy, měli byste je umístit ve skupině automaticky generované prostředků.

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent  