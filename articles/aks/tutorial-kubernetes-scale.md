---
title: Kurz Kubernetes v Azure – Škálování aplikace
description: V tomto kurzu Azure Kubernetes Service (AKS) zjistíte, jak škálovat uzly a pody v Kubernetes a jak implementovat automatické horizontální škálování podů.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 5ffe7b4c7830500e5eeeeb61c57730d9a0d9df47
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/15/2018
ms.locfileid: "41919899"
---
# <a name="tutorial-scale-applications-in-azure-kubernetes-service-aks"></a>Kurz: Škálování aplikací ve službě Azure Kubernetes Service (AKS)

Pokud jste postupovali podle kurzů, máte funkční cluster Kubernetes ve službě AKS a nasadili jste aplikaci Azure Vote. V tomto kurzu, který je pátou částí sedmidílné série, budete škálovat pody v této aplikaci a vyzkoušíte si jejich automatické škálování. Naučíte se také, jak škálovat počet uzlů virtuálního počítače Azure, aby se změnila kapacita clusteru pro hostování úloh. Získáte informace o těchto tématech:

> [!div class="checklist"]
> * Škálování uzlů Kubernetes
> * Ruční škálování podů Kubernetes, ve kterých se spouští vaše aplikace
> * Konfigurace automatického škálování podů, ve kterých se spouští front-end aplikace

V následujících kurzech se aplikace Azure Vote aktualizuje na novou verzi.

## <a name="before-you-begin"></a>Než začnete

V předchozích kurzech se aplikace zabalila do image kontejneru, tato image se odeslala do Azure Container Registry a vytvořil se cluster Kubernetes. Aplikace se potom spustila v tomto clusteru Kubernetes. Pokud jste tyto kroky neprovedli a chcete si je projít, vraťte se ke [kurzu 1 – Vytváření imagí kontejneru][aks-tutorial-prepare-app].

Tento kurz vyžaduje použití Azure CLI verze 2.0.38 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI][azure-cli-install].

## <a name="manually-scale-pods"></a>Ruční škálování podů

Při nasazení front-endu Azure Vote a instance Redis v předchozích kurzech se vytvořila jedna replika. Pokud chcete zobrazit počet a stav podů ve vašem clusteru, použijte příkaz [kubectl get][kubectl-get] následujícím způsobem:

```console
kubectl get pods
```

Následující příklad výstupu ukazuje jeden pod front-endu a jeden pod back-endu:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Pokud chcete ručně změnit počet podů v nasazení *azure-vote-front*, použijte příkaz [kubectl scale][kubectl-scale]. Následující příklad zvýší počet podů front-endu na *5*:

```console
kubectl scale --replicas=5 deployment/azure-vote-front
```

Znovu spusťte příkaz [kubectl get pods][kubectl-get] a ověřte, že platforma Kubernetes vytvořila další pody. Asi za minutu budou další pody dostupné ve vašem clusteru:

```console
$ kubectl get pods

                                    READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Automatické škálování podů

Kubernetes podporuje [horizontální automatické škálování podů][kubernetes-hpa] pro úpravu počtu podů v nasazení v závislosti na využití procesoru nebo jiné vybrané metrice. [Server metrik][metrics-server] se používá pro poskytování využití prostředků v Kubernetes. Pokud chcete nainstalovat server metrik, naklonujte úložiště GitHub `metrics-server` a nainstalujte vzorové definice prostředků. Obsah těchto definicí YAML si můžete zobrazit na stránce se [serverem metrik pro Kuberenetes 1.8+][metrics-server-github].

```console
git clone https://github.com/kubernetes-incubator/metrics-server.git
kubectl create -f metrics-server/deploy/1.8+/
```

Aby bylo možné použít modul automatického škálování, pody musí mít definovaná omezení a požadavky na procesor. V nasazení `azure-vote-front` kontejner front-endu vyžaduje 0,25 CPU s limitem 0,5 CPU. Nastavení vypadá takto:

```yaml
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

Následující příklad využívá příkaz [kubectl autoscale][kubectl-autoscale] k automatickému škálování počtu podů v nasazení *azure-vote-front*. Pokud využití procesoru přesáhne 50 %, modul automatického škálování zvýší počet podů až do maximálního počtu 10 instancí:

```console
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Pokud chcete zobrazit stav modulu automatického škálování, použijte příkaz `kubectl get hpa` následujícím způsobem:

```
$ kubectl get hpa

NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Po několika minutách se díky minimálnímu zatížení aplikace Azure Vote počet replik podů automaticky sníží na 3. Opětovným použitím příkazu `kubectl get pods` můžete zobrazit odebírání nepotřebných podů.

## <a name="manually-scale-aks-nodes"></a>Ruční škálování uzlů AKS

Pokud jste cluster Kubernetes vytvořili pomocí příkazů v předchozím kurzu, má jeden uzel. Pokud ve vašem clusteru plánujete více nebo méně úloh kontejneru, můžete počet uzlů upravit ručně.

Následující příklad zvýší počet uzlů v clusteru Kubernetes s názvem *myAKSCluster* na tři. Dokončení tohoto příkazu trvá několik minut.

```azurecli
az aks scale --resource-group=myResourceGroup --name=myAKSCluster --node-count 3
```

Výstup je podobný tomuto:

```
"agentPoolProfiles": [
  {
    "count": 3,
    "dnsPrefix": null,
    "fqdn": null,
    "name": "myAKSCluster",
    "osDiskSizeGb": null,
    "osType": "Linux",
    "ports": null,
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_D2_v2",
    "vnetSubnetId": null
  }
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste v clusteru Kubernetes využili různé funkce škálování. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Škálování uzlů Kubernetes
> * Ruční škálování podů Kubernetes, ve kterých se spouští vaše aplikace
> * Konfigurace automatického škálování podů, ve kterých se spouští front-end aplikace

V dalším kurzu se dozvíte, jak aktualizovat aplikaci v Kubernetes.

> [!div class="nextstepaction"]
> [Aktualizace aplikace v Kubernetes][aks-tutorial-update-app]

<!-- LINKS - external -->
[kubectl-autoscale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-scale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale
[kubernetes-hpa]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/
[metrics-server-github]: https://github.com/kubernetes-incubator/metrics-server/tree/master/deploy/1.8%2B
[metrics-server]: https://kubernetes.io/docs/tasks/debug-application-cluster/core-metrics-pipeline/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-update-app]: ./tutorial-kubernetes-app-update.md
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[azure-cli-install]: /cli/azure/install-azure-cli
