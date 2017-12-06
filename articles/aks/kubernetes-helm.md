---
title: "Nasazení kontejnerů s Helm v Kubernetes v Azure"
description: "Pomocí nástroje balení Helm nasazení kontejnerů v clusteru s podporou Kubernetes v AKS"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7065ceaf87f0cb5ebf46c53c71c6df4b069b2deb
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/06/2017
---
# <a name="use-helm-with-azure-container-service-aks"></a>Použití Helm s Azure Container Service (AKS)

[Helm](https://github.com/kubernetes/helm/) je nástroj balení open source, který vám pomůže nainstalovat a spravovat životní cyklus aplikace Kubernetes. Podobně jako správce balíčku Linux jako *byt č* a *Yum*, Helm slouží ke správě Kubernetes grafy, které jsou balíčky předkonfigurované Kubernetes prostředků.

Tento dokument postup prostřednictvím konfiguraci a použití Helm v clusteru s podporou Kubernetes na AKS.

## <a name="before-you-begin"></a>Než začnete

Podrobně popsané kroky v tomto dokumentu předpokládají, že jste vytvořili cluster AKS a navázali s ním připojení přes kubectl. Pokud potřebujete tyto položky, přečtěte si [Rychlý úvod ke službě AKS](./kubernetes-walkthrough.md).

## <a name="install-helm-cli"></a>Nainstalujte Helm rozhraní příkazového řádku

Rozhraní příkazového řádku Helm je klient, který běží ve vývojovém systému a umožňuje spuštění, zastavení a správu aplikací s Helm grafy.

Pokud používáte Azure CloudShell, rozhraní příkazového řádku Helm je již nainstalován. Instalace rozhraní příkazového řádku Helm na použití Mac `brew`. Instalace dalších možností najdete v tématu [instalace Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md).

```console
brew install kubernetes-helm
```

Výstup:

```
==> Downloading https://homebrew.bintray.com/bottles/kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/kubernetes-helm/2.6.2: 50 files, 132.4MB
```

## <a name="configure-helm"></a>Konfigurace Helm

[Helm init](https://docs.helm.sh/helm/#helm-init) příkaz slouží k instalaci součástí Helm v clusteru s podporou Kubernetes a proveďte konfigurace na straně klienta. Helm je předinstalovaným v clusterech AKS, takže je potřeba pouze konfiguraci klienta. Spusťte následující příkaz, abyste mohli nakonfigurovat klienta Helm.

```azurecli-interactive
helm init --client-only
```

Výstup:

```
$HELM_HOME has been configured at /Users/neilpeterson/.helm.
Not installing Tiller due to 'client-only' flag having been set
Happy Helming!
```

## <a name="find-helm-charts"></a>Najít Helm grafy

Helm grafy se používají k nasazení aplikací do clusteru s podporou Kubernetes. Chcete-li vyhledat předem vytvořené Helm grafy, použijte [helm vyhledávání](https://docs.helm.sh/helm/#helm-search) příkaz.

```azurecli-interactive
helm search
```

Vypadá výstup podobný následujícímu, ale s mnoha další grafy.

```
NAME                            VERSION DESCRIPTION
stable/acs-engine-autoscaler    2.0.0   Scales worker nodes within agent pools
stable/artifactory              6.1.0   Universal Repository Manager supporting all maj...
stable/aws-cluster-autoscaler   0.3.1   Scales worker nodes within autoscaling groups.
stable/buildkite                0.2.0   Agent for Buildkite
stable/centrifugo               2.0.0   Centrifugo is a real-time messaging server.
stable/chaoskube                0.5.0   Chaoskube periodically kills random pods in you...
stable/chronograf               0.3.0   Open-source web application written in Go and R...
stable/cluster-autoscaler       0.2.0   Scales worker nodes within autoscaling groups.
stable/cockroachdb              0.5.0   CockroachDB is a scalable, survivable, strongly...
stable/concourse                0.7.0   Concourse is a simple and scalable CI system.
stable/consul                   0.4.1   Highly available and distributed service discov...
stable/coredns                  0.5.0   CoreDNS is a DNS server that chains middleware ...
stable/coscale                  0.2.0   CoScale Agent
stable/dask-distributed         2.0.0   Distributed computation in Python
stable/datadog                  0.8.0   DataDog Agent
...
```

Chcete-li aktualizovat seznam grafy, použijte [helm úložišti aktualizace](https://docs.helm.sh/helm/#helm-repo-update) příkaz.

```azurecli-interactive
helm repo update
```

Výstup:

```
Hang tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

## <a name="run-helm-charts"></a>Spustit Helm grafy

Chcete-li nasadit řadič NGINX příjem příchozích dat, použijte [helm instalace](https://docs.helm.sh/helm/#helm-install) příkaz.

```azurecli-interactive
helm install stable/nginx-ingress
```

Výstup bude vypadat podobně jako následující, ale obsahuje další informace, jako je například pokyny o tom, jak použít Kubernetes nasazení.

```
NAME:   tufted-ocelot
LAST DEPLOYED: Thu Oct  5 00:48:04 2017
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                                    DATA  AGE
tufted-ocelot-nginx-ingress-controller  1     5s

==> v1/Service
NAME                                         CLUSTER-IP   EXTERNAL-IP  PORT(S)                     AGE
tufted-ocelot-nginx-ingress-controller       10.0.140.10  <pending>    80:30486/TCP,443:31358/TCP  5s
tufted-ocelot-nginx-ingress-default-backend  10.0.34.132  <none>       80/TCP                      5s

==> v1beta1/Deployment
NAME                                         DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
tufted-ocelot-nginx-ingress-controller       1        1        1           0          5s
tufted-ocelot-nginx-ingress-default-backend  1        1        1           1          5s
...
```

Další informace o používání řadič NGINX příjem příchozích dat s Kubernetes najdete v tématu [NGINX příjem příchozích dat řadiče](https://github.com/kubernetes/ingress/tree/master/controllers/nginx).

## <a name="list-helm-charts"></a>Seznam Helm grafy

Chcete-li zobrazit seznam grafy nainstalován na váš cluster, použijte [helm seznamu](https://docs.helm.sh/helm/#helm-list) příkaz.

```azurecli-interactive
helm list
```

Výstup:

```
NAME            REVISION    UPDATED                     STATUS      CHART               NAMESPACE
bilging-ant     1           Thu Oct  5 00:11:11 2017    DEPLOYED    nginx-ingress-0.8.7 default
```

## <a name="next-steps"></a>Další kroky

Další informace o správě Kubernetes grafy naleznete v dokumentaci k Helm.

> [!div class="nextstepaction"]
> [Helm dokumentace](https://github.com/kubernetes/helm/blob/master/docs/index.md)