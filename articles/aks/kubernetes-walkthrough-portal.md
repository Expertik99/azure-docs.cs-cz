---
title: Rychlý start – rychlý start práce na portálu Azure s clusterem Kubernetes
description: Rychle se naučíte, jak pomocí portálu Azure vytvořit cluster Kubernetes pro kontejnery Linuxu ve službě AKS.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: quickstart
ms.date: 07/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: aceddc2594065c9c36f8dbf63fce2ad03577a383
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/02/2018
ms.locfileid: "39443363"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster"></a>Rychlý start: Nasazení clusteru Azure Kubernetes Service (AKS)

V tomto rychlém startu nasadíte cluster AKS pomocí portálu Azure. Následně se na tomto clusteru spustí vícekontejnerová aplikace skládající se z front-endu webu a instance Redis. Po dokončení bude aplikace přístupná přes internet.

![Obrázek přechodu na ukázkovou aplikaci Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

Tento rychlý start předpokládá základní znalosti konceptů Kubernetes. Podrobné informace o Kubernetes najdete v [dokumentaci ke Kubernetes][kubernetes-documentation].

## <a name="sign-in-to-azure"></a>Přihlášení k Azure

Přihlaste se k webu Azure Portal na adrese http://portal.azure.com.

## <a name="create-an-aks-cluster"></a>Vytvoření clusteru AKS

V levém horním rohu webu Azure Portal vyberte **Vytvořit prostředek** > **Kubernetes Service**.

Pokud chcete vytvořit cluster AKS, proveďte následující kroky:

1. **Základy:** Nakonfigurujte následující možnosti:
    - *PODROBNOSTI O PROJEKTU:* Vyberte předplatné Azure a pak vyberte nebo vytvořte skupinu prostředků Azure, například *myResourceGroup*. Zadejte **Název clusteru Kubernetes**, například *myAKSCluster*.
    - *PODROBNOSTI O CLUSTERU:* Vyberte oblast, verzi Kubernetes a předponu názvu DNS pro cluster AKS.
    - *ŠKÁLOVÁNÍ:* Vyberte velikost virtuálního počítače pro uzly AKS. Velikost virtuálního počítače **nejde** změnit po nasazení clusteru AKS.
        - Vyberte počet uzlů, které se mají do clusteru nasadit. Pro účely tohoto rychlého startu nastavte **Počet uzlů** na hodnotu *1*. Počet uzlů **jde** upravit po nasazení clusteru.
    
    ![Vytvoření clusteru AKS – zadání základních informací](media/kubernetes-walkthrough-portal/create-cluster-1.png)

    Po dokončení vyberte **Další: Ověřování**.

1. **Ověřování:** Nakonfigurujte následující možnosti:
    - Vytvořte nový instanční objekt nebo *nakonfigurujte* použití existujícího. Pokud použijete stávající hlavní název služby (SPN), je potřeba zadat ID klienta a tajný klíč SPN.
    - Povolte možnost řízení přístupu na základě role (RBAC) v Kubernetes. Tyto ovládací prvky poskytují podrobnější řízení přístupu k prostředkům Kubernetes nasazeným ve vašem clusteru AKS.

    ![Vytvoření clusteru AKS – konfigurace ověřování](media/kubernetes-walkthrough-portal/create-cluster-2.png)

    Jakmile budete hotovi, vyberte **Další: Sítě**.

1. **Sítě:** Nakonfigurujte následující možnosti sítě, které by se měly nastavit jako výchozí:
    
    - **Směrování aplikace HTTP** – Vyberte **Ano** a nakonfigurujte integrovaný kontroler příchozího přenosu dat tak, aby automaticky vytvořil veřejný název DNS. Další informace o směrování HTTP najdete v tématu [Směrování HTTP a DNS ve službě AKS][http-routing].
    - **Konfigurace sítě** – Vyberte **Základní** konfiguraci sítě s využitím modulu plug-in Kubernetes [kubenet][kubenet] místo pokročilé konfigurace sítě s využitím [Azure CNI][azure-cni]. Další informace o možnostech sítí najdete v tématu [Přehled sítí AKS][aks-network].
    
    Jakmile budete hotovi, vyberte **Další: Monitorování**.

1. Při nasazování clusteru AKS můžete nakonfigurovat službu Azure Container Insights tak, monitorovala stav clusteru AKS a podů spuštěných v clusteru. Další informace o monitorování stavu clusteru najdete v tématu [Monitorování stavu služby Azure Kubernetes Service][aks-monitor].

    Výběrem možnosti **Ano** povolte monitorování kontejneru a vyberte existující pracovní prostor Log Analytics nebo vytvořte nový.
    
    Vyberte **Zkontrolovat a vytvořit** a jakmile budete připraveni, vyberte **Vytvořit**.

Vytvoření clusteru AKS a jeho příprava k použití trvá několik minut. Přejděte ke skupině prostředků clusteru AKS, například *myResourceGroup*, a vyberte prostředek AKS, například *myAKSCluster*. Zobrazí se řídicí panel clusteru AKS, podobně jako na následujícím ukázkovém snímku obrazovky:

![Příklad řídicího panelu AKS na webu Azure Portal](media/kubernetes-walkthrough-portal/aks-portal-dashboard.png)

## <a name="connect-to-the-cluster"></a>Připojení ke clusteru

Ke správě clusteru Kubernetes použijte klienta příkazového řádku Kubernetes [kubectl][kubectl]. Klient `kubectl` je předinstalovaný ve službě Azure Cloud Shell.

Otevřete službu Cloud Shell pomocí tlačítka v pravém horním rohu portálu Azure.

![Portál s otevřenou službou Azure Cloud Shell](media/kubernetes-walkthrough-portal/aks-cloud-shell.png)

Pomocí příkazu [az aks get-credentials][az-aks-get-credentials] nakonfigurujte klienta `kubectl` pro připojení k vašemu clusteru Kubernetes. Následující příklad získá přihlašovací údaje pro název clusteru *myAKSCluster* ve skupině prostředků *myResourceGroup*:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Pokud chcete ověřit připojení ke clusteru, použijte příkaz [kubectl get][kubectl-get], který vrátí seznam uzlů clusteru.

```azurecli-interactive
kubectl get nodes
```

Následující příklad výstupu ukazuje jeden uzel vytvořený v předchozích krocích.

```
NAME                       STATUS    ROLES     AGE       VERSION
aks-agentpool-14693408-0   Ready     agent     10m       v1.10.5
```

## <a name="run-the-application"></a>Spuštění aplikace

Soubor manifestu Kubernetes definuje požadovaný stav clusteru, včetně toho, jaké image kontejnerů mají být spuštěné. V tomto rychlém startu manifest slouží k vytvoření všech objektů potřebných ke spuštění ukázkové aplikace Azure Vote. Mezi tyto objekty patří dvě [nasazení Kubernetes][kubernetes-deployment] – jedno pro front-end aplikace Azure Vote a druhé pro instanci Redis. Vytvoří se také dvě [služby Kubernetes][kubernetes-service] – interní služba pro instanci Redis a externí služba pro přístup k aplikaci Azure Vote z internetu.

Vytvořte soubor `azure-vote.yaml` a zkopírujte do něj následující kód YAML. Pokud pracujete ve službě Azure Cloud Shell, vytvořte tento soubor pomocí editoru `vi` nebo `Nano`, stejně jako kdybyste pracovali na virtuálním nebo fyzickém systému.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      containers:
      - name: azure-vote-back
        image: redis
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:v1
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Pomocí příkazu [kubectl apply][kubectl-apply] spusťte aplikaci.

```azurecli-interactive
kubectl create -f azure-vote.yaml
```

Následující příklad výstupu ukazuje prostředky Kubernetes vytvořené ve vašem clusteru AKS:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Testování aplikace

Při spuštění aplikace se vytvoří [služba Kubernetes][kubernetes-service], která zveřejní aplikaci na internetu. Dokončení tohoto procesu může trvat několik minut.

Pomocí příkazu [kubectl get service][kubectl-get] s argumentem `--watch` můžete sledovat průběh.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Na začátku se bude adresa *EXTERNAL-IP* pro službu *azure-vote-front* zobrazovat ve stavu *Probíhá*.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

Jakmile se stav adresy *EXTERNAL-IP* změní ze stavu *Probíhá* na hodnotu *IP adresa*, pomocí klávesové zkratky `CTRL-C` zastavte sledovací proces kubectl.

```
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Otevřete webový prohlížeč a přejděte na externí IP adresu vaší služby. Zobrazí se aplikace Azure Vote, jak je znázorněno v následujícím příkladu:

![Obrázek přechodu na ukázkovou aplikaci Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

## <a name="monitor-health-and-logs"></a>Monitorování stavu a protokolů

Při vytváření clusteru se povolilo monitorování přehledů o kontejnerech. Tato funkce monitorování poskytuje metriky stavu clusteru AKS i podů spuštěných na tomto clusteru. Další informace o monitorování stavu clusteru najdete v tématu [Monitorování stavu služby Azure Kubernetes Service][aks-monitor].

Naplnění těchto dat na webu Azure Portal může trvat několik minut. Pokud chcete zobrazit aktuální stav, dobu provozu a využití prostředků pro pody Azure Vote, na webu Azure Portal přejděte zpět k prostředku AKS, například *myAKSCluster*. Zvolte **Monitorovat stav kontejneru**, vyberte **výchozí** obor názvů a pak vyberte **Kontejnery**.  Zobrazí se kontejnery *azure-vote-back* a *azure-vote-front*:

![Zobrazení stavu spuštěných kontejnerů v AKS](media/kubernetes-walkthrough-portal/monitor-containers.png)

Pokud chcete zobrazit protokoly pro pod `azure-vote-front`, vyberte odkaz **Zobrazit protokoly** na pravé straně seznamu kontejnerů. Tyto protokoly obsahují streamy výstupů *stdout* a *stderr* z kontejneru.

![Zobrazení protokolů kontejneru v AKS](media/kubernetes-walkthrough-portal/monitor-containers-logs.png)

## <a name="delete-cluster"></a>Odstranění clusteru

Když už cluster nepotřebujete, odstraňte prostředek clusteru, čímž odstraníte všechny související prostředky. Tuto operaci můžete provést na webu Azure Portal výběrem tlačítka **Odstranit** na řídicím panelu clusteru AKS. Případně můžete použít příkaz [az aks delete][az-aks-delete] ve službě Cloud Shell:

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster --no-wait
```

## <a name="get-the-code"></a>Získání kódu

V tomto rychlém startu se k vytvoření nasazení Kubernetes použily předem vytvořené image kontejnerů. Související kód aplikace, soubor Dockerfile a soubor manifestu Kubernetes jsou k dispozici na GitHubu.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Další kroky

V tomto rychlém startu jste nasadili cluster Kubernetes a do něj jste nasadili vícekontejnerovou aplikaci.

Další informace o službě AKS a podrobné vysvětlení kompletního příkladu od kódu až po nasazení najdete v kurzu clusteru Kubernetes.

> [!div class="nextstepaction"]
> [Kurz AKS][aks-tutorial]

<!-- LINKS - external -->
[azure-vote-app]: https://github.com/Azure-Samples/azure-voting-app-redis.git
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[kubernetes-deployment]: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-delete]: /cli/azure/aks#az-aks-delete
[aks-monitor]: ../monitoring/monitoring-container-health.md
[aks-network]: ./networking-overview.md
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[http-routing]: ./http-application-routing.md
