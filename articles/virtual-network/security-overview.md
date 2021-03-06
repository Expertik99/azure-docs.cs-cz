---
title: Přehled skupin zabezpečení Azure | Microsoft Docs
description: Přečtěte si víc o skupinách zabezpečení sítě a aplikací. Skupiny zabezpečení pomáhají filtrovat síťový provoz mezi prostředky Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2018
ms.author: jdial
ms.openlocfilehash: f01ff2dfff19d066fe3ecd8924a5b54cd6cc68a5
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39264160"
---
# <a name="security-groups"></a>Skupiny zabezpečení
<a name="network-security-groups"></a>

Pomocí skupiny zabezpečení sítě můžete filtrovat síťový provoz do a z prostředků Azure ve [virtuální síti](virtual-networks-overview.md) Azure. Skupina zabezpečení sítě obsahuje [pravidla zabezpečení](#security-rules) umožňující povolit nebo odepřít příchozí nebo odchozí síťový provoz několika typů prostředků Azure. Informace o prostředcích Azure, které je možné nasadit do virtuální sítě a ke kterým je možné přidružit skupiny zabezpečení sítě, najdete v tématu [Integrace virtuální sítě pro služby Azure](virtual-network-for-azure-services.md). Pro každé pravidlo můžete určit zdroj a cíl, port a protokol.

Tento článek vysvětluje koncepty skupin zabezpečení sítě, které vám pomůžou je efektivně využívat. Pokud jste ještě nikdy skupinu zabezpečení sítě nevytvářeli, můžete si projít rychlý [kurz](tutorial-filter-network-traffic.md), ve kterém se seznámíte s jejím vytvořením. Pokud už skupiny zabezpečení sítě znáte a potřebujete je spravovat, přečtěte si téma [Správa skupiny zabezpečení sítě](manage-network-security-group.md). Pokud máte problémy s komunikací a potřebujete řešit potíže se skupinami zabezpečení sítě, přečtěte si téma [Diagnostika potíží s filtrováním síťového provozu virtuálních počítačů](diagnose-network-traffic-filter-problem.md). Můžete povolit [protokoly toků skupin zabezpečení sítě](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json), abyste mohli [analyzovat síťový provoz](../network-watcher/traffic-analytics.md?toc=%2fazure%2fvirtual-network%2ftoc.json) směřující do a z prostředků s přidruženou skupinou zabezpečení sítě.

## <a name="security-rules"></a>Pravidla zabezpečení

Skupina zabezpečení sítě nemusí obsahovat žádná pravidla nebo může podle potřeby obsahovat libovolný počet pravidel v rámci [omezení](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) předplatného Azure. Každé pravidlo určuje následující vlastnosti:

|Vlastnost  |Vysvětlení  |
|---------|---------|
|Název|Jedinečný název v rámci skupiny zabezpečení sítě.|
|Priorita | Číslo v rozsahu od 100 do 4096. Pravidla se zpracovávají v pořadí podle priority, přičemž nižší čísla, která mají vyšší prioritu, se zpracovávají před vyššími čísly. Jakmile provoz odpovídá pravidlu, zpracování se zastaví. V důsledku toho se nezpracují žádná existující pravidla s nižší prioritou (vyšší čísla), která mají stejné atributy jako pravidla s vyšší prioritou.|
|Zdroj nebo cíl| Všechny nebo určitá IP adresa, blok CIDR (například 10.0.0.0/24), [značka služby](#service-tags) nebo [skupina zabezpečení aplikace](#application-security-groups). Pokud zadáváte adresu prostředku Azure, zadejte privátní IP adresu přiřazenou k tomuto prostředku. Skupiny zabezpečení sítě se zpracovávají poté, co Azure přeloží veřejnou IP adresu na privátní IP adresu pro příchozí provoz, a před tím, než Azure přeloží privátní IP adresu na veřejnou IP adresu pro odchozí provoz. Další informace o [IP adresách](virtual-network-ip-addresses-overview-arm.md) Azure. Zadání rozsahu, značky služby nebo skupiny zabezpečení aplikace umožňuje vytvářet méně pravidel zabezpečení. Možnost zadat v pravidlu několik jednotlivých IP adres a rozsahů (není možné zadat více značek služeb ani skupin aplikací) se označuje jako [rozšířená pravidla zabezpečení](#augmented-security-rules). Rozšířená pravidla zabezpečení je možné vytvářet pouze ve skupinách zabezpečení sítě vytvořených prostřednictvím modelu nasazení Resource Manager. Ve skupinách zabezpečení sítě vytvořených prostřednictvím modelu nasazení Classic není možné zadat více IP adres ani rozsahů IP adres. Další informace o [modelech nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).|
|Protocol (Protokol)     | TCP, UDP nebo Všechny, což zahrnuje protokoly TCP, UDP a ICMP. Samotný protokol ICMP nejde zadat, takže pokud potřebujete protokol ICMP, použijte možnost Všechny. |
|Směr| Určuje, jestli se pravidlo vztahuje na příchozí nebo odchozí provoz.|
|Rozsah portů     |Můžete zadat určitý port nebo rozsah portů. Můžete zadat například 80 nebo 10000-10005. Zadání rozsahů umožňuje vytvářet méně pravidel zabezpečení. Rozšířená pravidla zabezpečení je možné vytvářet pouze ve skupinách zabezpečení sítě vytvořených prostřednictvím modelu nasazení Resource Manager. Ve skupinách zabezpečení sítě vytvořených prostřednictvím modelu nasazení Classic není možné zadat několik portů ani rozsahů portů ve stejném pravidlu zabezpečení.   |
|Akce     | Povolení nebo odepření.        |

Pravidla zabezpečení skupin zabezpečení sítě se vyhodnocují podle priority s použitím informací o řazené kolekci 5 členů (zdroj, zdrojový port, cíl, cílový port a protokol) a určují, jestli se má provoz povolit nebo odepřít. Pro stávající připojení se vytvoří záznam toku. Komunikace se povoluje nebo odepírá na základě stavu připojení v záznamu toku. Záznam toku umožňuje, aby skupina zabezpečení sítě byla stavová. Pokud zadáte odchozí pravidlo zabezpečení pro všechny adresy například přes port 80, není potřeba zadávat příchozí pravidlo zabezpečení pro reakci na odchozí provoz. Příchozí pravidlo zabezpečení je potřeba zadat pouze v případě, že se komunikace zahajuje externě. Opačně to platí také. Pokud je přes port povolený příchozí provoz, není potřeba zadávat odchozí pravidlo zabezpečení pro reakci na provoz přes tento port.
Pokud odeberete pravidlo zabezpečení, které povolilo tok, nesmí se přerušit žádné stávající připojení. Toky provozu se přeruší v případě zastavení připojení a toku provozu oběma směry po dobu alespoň několika minut.

Počet pravidel zabezpečení, která můžete ve skupině zabezpečení sítě vytvořit, je omezený. Podrobnosti najdete v tématu věnovaném [omezením Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="augmented-security-rules"></a>Rozšířená pravidla zabezpečení

Rozšířená pravidla zabezpečení zjednodušují definici zabezpečení pro virtuální sítě tím, že umožňují definovat větší a složitější zásady zabezpečení sítě při použití menšího počtu pravidel. Můžete zkombinovat více portů a explicitních IP adres a rozsahů do jediného, snadno pochopitelného pravidla zabezpečení. Rozšířená pravidla používejte v polích pravidla pro zdroj, cíl a port. Pokud chcete zjednodušit údržbu definice pravidla zabezpečení, zkombinujte rozšířená pravidla zabezpečení se [značkami služeb](#service-tags) nebo [skupinami zabezpečení aplikací](#application-security-groups). Počet adres, rozsahů a portů, které můžete v pravidle určit, je omezený. Podrobnosti najdete v tématu věnovaném [omezením Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="service-tags"></a>Značky služeb

 Značka služby představuje skupinu předpon IP adres a tím pomáhá minimalizovat složitost vytváření pravidla zabezpečení. Nemůžete vytvořit vlastní značku služby ani určit, které IP adresy jsou ve značce zahrnuté. Předpony adres zahrnuté ve značce služby spravuje Microsoft, a pokud se adresy změní, automaticky značku služby aktualizuje. Značky služeb můžete používat místo konkrétních IP adres při vytváření pravidel zabezpečení. Pro použití v definici pravidla zabezpečení jsou k dispozici následující značky služeb. Jejich názvy se mírně liší mezi [modely nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

* **VirtualNetwork** (Resource Manager) (**VIRTUAL_NETWORK** v případě klasického modelu): Tato značka zahrnuje adresní prostor virtuální sítě (všechny rozsahy CIDR definované pro virtuální síť), všechny připojené místní adresní prostory a [partnerské](virtual-network-peering-overview.md) virtuální sítě nebo virtuální sítě připojené k [bráně virtuální sítě](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* **AzureLoadBalancer** (Resource Manager) (**AZURE_LOADBALANCER** v případě klasického modelu): Tato značka označuje nástroj pro vyrovnávání zatížení infrastruktury Azure. Značka se přeloží na [IP adresu datacentra Azure](https://www.microsoft.com/download/details.aspx?id=41653), kde mají původ sondy stavu Azure. Pokud nepoužíváte nástroj pro vyrovnávání zatížení Azure, můžete toto pravidlo přepsat.
* **Internet** (Resource Manager) (**INTERNET** v případě klasického modelu): Tato značka označuje adresní prostor IP adres, který se nachází mimo virtuální síť a je dostupný prostřednictvím veřejného internetu. Rozsah adres zahrnuje [veřejný adresní prostor IP adres vlastněný Azure](https://www.microsoft.com/download/details.aspx?id=41653).
* **AzureTrafficManager** (pouze Resource Manager): Tato značka označuje adresní prostor IP adres sondy pro Azure Traffic Manager. Další informace o IP adresách sondy pro Traffic Manager najdete v tématu [Azure Traffic Manager – nejčastější dotazy](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs). Můžete si stáhnout seznam předpon přiřazených k této značce pro [veřejný](https://www.microsoft.com/download/details.aspx?id=56519) cloud Azure a cloudy Azure [US Government](https://www.microsoft.com/download/details.aspx?id=57063), [China](https://www.microsoft.com/download/details.aspx?id=57062) a [Germany](https://www.microsoft.com/download/details.aspx?id=57064).
* **Storage** (pouze Resource Manager): Tato značka označuje adresní prostor IP adres pro službu Azure Storage. Pokud jako hodnotu zadáte *Storage*, provoz směřující do úložiště se povolí nebo zakáže. Pokud chcete povolit přístup k úložišti pouze v konkrétní [oblasti](https://azure.microsoft.com/regions), můžete zadat tuto oblast. Pokud například chcete povolit přístup pouze ke službě Azure Storage v oblasti Východní USA, můžete jako značku služby zadat *Storage.EastUS*. Značka představuje službu, ale ne konkrétní instance služby. Značka například představuje službu Azure Storage, ale ne konkrétní účet služby Azure Storage. Všechny předpony adres reprezentované touto značkou jsou reprezentované také značkou **Internet**. Můžete si stáhnout seznam předpon přiřazených k této značce pro [veřejný](https://www.microsoft.com/download/details.aspx?id=56519) cloud Azure a cloudy Azure [US Government](https://www.microsoft.com/download/details.aspx?id=57063), [China](https://www.microsoft.com/download/details.aspx?id=57062) a [Germany](https://www.microsoft.com/download/details.aspx?id=57064).
* **Sql** (pouze Resource Manager): Tato značka označuje předpony adres služeb Azure SQL Database a Azure SQL Data Warehouse. Pokud jako hodnotu zadáte *SQL*, provoz směřující do SQL se povolí nebo zakáže. Pokud chcete povolit přístup k SQL jenom v konkrétní [oblasti](https://azure.microsoft.com/regions), můžete zadat tuto oblast. Pokud například chcete povolit přístup pouze ke službě Azure SQL Database v oblasti Východní USA, můžete jako značku služby zadat *Storage.EastUS*. Značka představuje službu, ale ne konkrétní instance služby. Značka například představuje službu Azure SQL Database, ale ne konkrétní server nebo databázi SQL. Všechny předpony adres reprezentované touto značkou jsou reprezentované také značkou **Internet**. Můžete si stáhnout seznam předpon přiřazených k této značce pro [veřejný](https://www.microsoft.com/download/details.aspx?id=56519) cloud Azure a cloudy Azure [US Government](https://www.microsoft.com/download/details.aspx?id=57063), [China](https://www.microsoft.com/download/details.aspx?id=57062) a [Germany](https://www.microsoft.com/download/details.aspx?id=57064).
* **AzureCosmosDB** (pouze Resource Manager): Tato značka označuje předpony adres služby Azure Cosmos Database. Pokud jako hodnotu zadáte *AzureCosmosDB*, provoz směřující do služby Azure Cosmos DB se povolí nebo zakáže. Pokud chcete povolit přístup ke službě Azure Cosmos DB pouze v konkrétní [oblasti](https://azure.microsoft.com/regions), můžete oblast zadat v následujícím formátu: AzureCosmosDB.[název_oblasti]. Můžete si stáhnout seznam předpon přiřazených k této značce pro [veřejný](https://www.microsoft.com/download/details.aspx?id=56519) cloud Azure a cloudy Azure [US Government](https://www.microsoft.com/download/details.aspx?id=57063), [China](https://www.microsoft.com/download/details.aspx?id=57062) a [Germany](https://www.microsoft.com/download/details.aspx?id=57064).
* **AzureKeyVault** (pouze Resource Manager): Tato značka označuje předpony adres služby Azure Key Vault. Pokud jako hodnotu zadáte *AzureKeyVault*, provoz směřující do služby Azure Key Vault se povolí nebo zakáže. Pokud chcete povolit přístup ke službě Azure Key Vault pouze v konkrétní [oblasti](https://azure.microsoft.com/regions), můžete oblast zadat v následujícím formátu: AzureKeyVault.[název_oblasti]. Můžete si stáhnout seznam předpon přiřazených k této značce pro [veřejný](https://www.microsoft.com/download/details.aspx?id=56519) cloud Azure a cloudy Azure [US Government](https://www.microsoft.com/download/details.aspx?id=57063), [China](https://www.microsoft.com/download/details.aspx?id=57062) a [Germany](https://www.microsoft.com/download/details.aspx?id=57064).

> [!NOTE]
> Značky služeb Azure označují předpony adres z konkrétního používaného cloudu. Místní značky služeb se nepodporují v národních cloudech, pouze v globálním formátu. Například *Storage* a *Sql*.

> [!NOTE]
> Pokud implementujete [koncový bod služby virtuální sítě](virtual-network-service-endpoints-overview.md) pro službu, jako je Azure Storage nebo Azure SQL Database, Azure pro tuto službu přidá [trasu](virtual-networks-udr-overview.md#optional-default-routes) k podsíti virtuální sítě. Předpony adres pro trasu jsou stejné předpony adres nebo rozsahy CIDR jako u odpovídající značky služby.

## <a name="default-security-rules"></a>Výchozí pravidla zabezpečení

Azure v každé skupině zabezpečení sítě, kterou vytvoříte, vytvoří následující výchozí pravidla:

### <a name="inbound"></a>Příchozí

#### <a name="allowvnetinbound"></a>AllowVNetInBound

|Priorita|Zdroj|Zdrojové porty|Cíl|Cílové porty|Protocol (Protokol)|Access|
|---|---|---|---|---|---|---|
|65000|VirtualNetwork|0-65535|VirtualNetwork|0-65535|Vše|Povolit|

#### <a name="allowazureloadbalancerinbound"></a>AllowAzureLoadBalancerInBound

|Priorita|Zdroj|Zdrojové porty|Cíl|Cílové porty|Protocol (Protokol)|Access|
|---|---|---|---|---|---|---|
|65001|AzureLoadBalancer|0-65535|0.0.0.0/0|0-65535|Vše|Povolit|

#### <a name="denyallinbound"></a>DenyAllInbound

|Priorita|Zdroj|Zdrojové porty|Cíl|Cílové porty|Protocol (Protokol)|Access|
|---|---|---|---|---|---|---|
|65500|0.0.0.0/0|0-65535|0.0.0.0/0|0-65535|Vše|Odepřít|

### <a name="outbound"></a>Odchozí

#### <a name="allowvnetoutbound"></a>AllowVnetOutBound

|Priorita|Zdroj|Zdrojové porty| Cíl | Cílové porty | Protocol (Protokol) | Access |
|---|---|---|---|---|---|---|
| 65000 | VirtualNetwork | 0-65535 | VirtualNetwork | 0-65535 | Vše | Povolit |

#### <a name="allowinternetoutbound"></a>AllowVnetOutBound

|Priorita|Zdroj|Zdrojové porty| Cíl | Cílové porty | Protocol (Protokol) | Access |
|---|---|---|---|---|---|---|
| 65001 | 0.0.0.0/0 | 0-65535 | Internet | 0-65535 | Vše | Povolit |

#### <a name="denyalloutbound"></a>DenyAllOutBound

|Priorita|Zdroj|Zdrojové porty| Cíl | Cílové porty | Protocol (Protokol) | Access |
|---|---|---|---|---|---|---|
| 65500 | 0.0.0.0/0 | 0-65535 | 0.0.0.0/0 | 0-65535 | Vše | Odepřít |

Ve sloupcích **Zdroj** a **Cíl** jsou hodnoty *VirtualNetwork*, *AzureLoadBalancer* a *Internet* [značky služeb](#service-tags), a nikoli IP adresy. Hodnota **Všechny** ve sloupci Protokol zahrnuje protokoly TCP, UDP a ICMP. Při vytváření pravidla můžete zadat protokol TCP, UDP nebo Všechny, ale nemůžete zadat samotný protokol ICMP. Proto pokud vaše pravidlo vyžaduje protokol ICMP, vyberte jako protokol hodnotu *Všechny*. Hodnota *0.0.0.0/0* ve sloupcích **Zdroj** a **Cíl** představuje všechny adresy.
 
Výchozí pravidla nemůžete odebrat, ale můžete je přepsat vytvořením pravidel s vyšší prioritou.

## <a name="application-security-groups"></a>Skupiny zabezpečení aplikací

Skupiny zabezpečení aplikací umožňují konfigurovat zabezpečení sítě jako přirozené rozšíření struktury aplikace. Můžete seskupovat virtuální počítače a na základě těchto skupin definovat zásady zabezpečení sítě. Zásady zabezpečení můžete opakovaně používat ve velkém měřítku bez potřeby ruční údržby explicitních IP adres. O složitost explicitních IP adres a několika skupin pravidel se stará platforma a vy se tak můžete zaměřit na obchodní logiku. Pro lepší pochopení skupin zabezpečení aplikací si představte následující příklad:

![Skupiny zabezpečení aplikací](./media/security-groups/application-security-groups.png)

Na předchozím obrázku jsou *NIC1* a *NIC2* členy skupiny zabezpečení aplikace *AsgWeb*. *NIC3* je členem skupiny zabezpečení aplikace *AsgLogic*. *NIC4* je členem skupiny zabezpečení aplikace *AsgDb*. Přestože jsou všechna síťová rozhraní v tomto příkladu členy pouze jedné skupiny zabezpečení aplikace, síťové rozhraní může být členem více skupin zabezpečení aplikací, a to až do limitu daného [omezeními Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Žádné ze síťových rozhraní nemá přidruženou skupinu zabezpečení sítě. Skupina *NSG1* je přidružená k oběma podsítím a obsahuje následující pravidla:

### <a name="allow-http-inbound-internet"></a>Allow-HTTP-Inbound-Internet

Toto pravidlo je potřeba k povolení provozu směřujícího z internetu na webové servery. Vzhledem k tomu, že výchozí pravidlo zabezpečení [DenyAllInbound](#denyallinbound) odepírá příchozí provoz z internetu, není pro skupiny zabezpečení aplikací *AsgLogic* a *AsgDb* potřeba žádné další pravidlo.

|Priorita|Zdroj|Zdrojové porty| Cíl | Cílové porty | Protocol (Protokol) | Access |
|---|---|---|---|---|---|---|
| 100 | Internet | * | AsgWeb | 80 | TCP | Povolit |

### <a name="deny-database-all"></a>Deny-Database-All

Vzhledem k tomu, že výchozí pravidlo zabezpečení [AllowVNetInBound](#allowvnetinbound) povoluje veškerou komunikaci mezi prostředky ve stejné virtuální síti, je toto pravidlo potřeba k odepření provozu ze všech prostředků.

|Priorita|Zdroj|Zdrojové porty| Cíl | Cílové porty | Protocol (Protokol) | Access |
|---|---|---|---|---|---|---|
| 120 | * | * | AsgDb | 1433 | Vše | Odepřít |

### <a name="allow-database-businesslogic"></a>Allow-Database-BusinessLogic

Toto pravidlo povoluje provoz ze skupiny zabezpečení aplikace *AsgLogic* do skupiny zabezpečení aplikace *AsgDb*. Priorita tohoto pravidla je vyšší než priorita pravidla *Deny-Database-All*. Díky tomu se toto pravidlo zpracuje před pravidlem *Deny-Database-All*, takže se povolí provoz ze skupiny zabezpečení aplikace *AsgLogic*, zatímco veškerý ostatní provoz se zablokuje.

|Priorita|Zdroj|Zdrojové porty| Cíl | Cílové porty | Protocol (Protokol) | Access |
|---|---|---|---|---|---|---|
| 110 | AsgLogic | * | AsgDb | 1433 | TCP | Povolit |

Pravidla určující skupinu zabezpečení aplikace jako zdroj nebo cíl se použijí pouze na síťová rozhraní, která jsou členy příslušné skupiny zabezpečení aplikace. Pokud síťové rozhraní není členem žádné skupiny zabezpečení aplikace, pravidlo se pro něj nepoužije, ani když je k podsíti přiřazená skupina zabezpečení sítě.

Pro skupiny zabezpečení aplikací platí následující omezení:

-   Počet skupin zabezpečení aplikací, které můžete mít v předplatném, je omezený. V souvislosti se skupinami zabezpečení aplikací platí také další omezení. Podrobnosti najdete v tématu věnovaném [omezením Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Jako zdroj a cíl můžete v pravidlu zabezpečení zadat jednu skupinu zabezpečení aplikace. Více skupin zabezpečení aplikací ve zdroji ani cíli zadat nemůžete.
- Všechna síťová rozhraní přiřazená ke skupině zabezpečení aplikace musí existovat ve stejné virtuální síti jako první síťové rozhraní přiřazené ke skupině zabezpečení aplikace. Pokud se například první síťové rozhraní přiřazené ke skupině zabezpečení aplikace *AsgWeb* nachází ve virtuální síti *VNet1*, pak všechna další síťová rozhraní přiřazená ke skupině zabezpečení aplikace *ASGWeb* musí existovat ve virtuální síti *VNet1*. Do stejné skupiny zabezpečení aplikace nemůžete přidat síťová rozhraní z různých virtuálních sítí.
- Pokud zadáte skupinu zabezpečení aplikací jako zdroj a cíl v pravidle zabezpečení, síťová rozhraní v obou skupinách zabezpečení aplikací musí existovat ve stejné virtuální síti. Kdyby například skupina *AsgLogic* obsahovala síťová rozhraní z virtuální sítě *VNet1* a skupina *AsgDb* obsahovala síťová rozhraní z virtuální sítě *VNet2*, nemohli byste v pravidle přiřadit skupinu *AsgLogic* jako zdroj a skupinu *AsgDb* jako cíl. Všechna síťová rozhraní pro zdrojové i cílové skupiny zabezpečení aplikací musí existovat ve stejné virtuální síti.

> [!TIP]
> Pokud chcete minimalizovat počet potřebných pravidel zabezpečení a nutnost jejich změn, naplánujte potřebné skupiny zabezpečení aplikací a vytvářejte pravidla s použitím značek služeb nebo skupin zabezpečení aplikací, a ne jednotlivých IP adres nebo rozsahů IP adres, pokud je to možné.

## <a name="how-traffic-is-evaluated"></a>Způsob vyhodnocování provozu

Do virtuální sítě Azure můžete nasadit prostředky z několika služeb Azure. Úplný seznam najdete v tématu popisujícím [služby, které je možné nasadit do virtuální sítě](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Ke každé [podsíti](virtual-network-manage-subnet.md#change-subnet-settings) virtuální sítě a každému [síťovému rozhraní](virtual-network-network-interface.md#associate-or-dissociate-a-network-security-group) na virtuálním počítači můžete přiřadit jednu nebo žádnou skupinu zabezpečení sítě. Stejnou skupinu zabezpečení sítě můžete přidružit k libovolnému počtu podsítí a síťových rozhraní.

Následující obrázek ukazuje různé scénáře nasazení skupin zabezpečení sítě pro povolení síťového provozu do a z internetu na portu TCP 80:

![Zpracování NSG](./media/security-groups/nsg-interaction.png)

Předchozí obrázek společně s následujícím textem vám pomůže porozumět způsobu, jakým Azure zpracovává příchozí a odchozí pravidla pro skupiny zabezpečení sítě:

### <a name="inbound-traffic"></a>Příchozí provoz

V případě příchozího provozu zpracuje Azure nejprve pravidla ve skupině zabezpečení sítě přidružené k příslušné podsíti, pokud taková skupina existuje, a pak pravidla ve skupině zabezpečení sítě přidružené k síťovému rozhraní, pokud taková skupina existuje.

- **VM1:** Zpracují se pravidla ve skupině *NSG1*, protože je přidružená k podsíti *Subnet1* a virtuální počítač *VM1* se nachází v podsíti *Subnet1*. Pokud jste nevytvořili pravidlo povolující příchozí provoz na portu 80, výchozí pravidlo zabezpečení [DenyAllInbound](#denyallinbound) provoz odepře a skupina *NSG2* ho nikdy nevyhodnotí, protože skupina *NSG2* je přidružená k síťovému rozhraní. Pokud skupina *NSG1* obsahuje pravidlo zabezpečení povolující port 80, provoz se pak zpracuje skupinou *NSG2*. Pokud pro virtuální počítač chcete povolit příchozí provoz přes port 80, skupina *NSG1* i *NSG2* musí obsahovat pravidlo povolující příchozí provoz přes port 80 z internetu.
- **VM2:** Zpracují se pravidla ve skupině *NSG1*, protože virtuální počítač *VM2* se také nachází v podsíti *Subnet1*. Vzhledem k tomu, že k síťovému rozhraní virtuálního počítače *VM2* není přidružená žádná skupina zabezpečení sítě, přijme toto rozhraní veškerý provoz povolený skupinou *NSG1* nebo se mu odepře veškerý provoz zakázaný skupinou *NSG1*. Pokud je skupina zabezpečení sítě přidružená k podsíti, povolí se nebo se odepře provoz do všech prostředků ve stejné podsíti.
- **VM3:** Vzhledem k tomu, že k podsíti *Subnet2* není přidružená žádná skupina zabezpečení sítě, povolí se příchozí provoz do této podsítě a zpracuje se skupinou *NSG2*, protože skupina *NSG2* je přidružená k síťovému rozhraní připojenému k virtuálnímu počítači *VM3*.
- **VM4:** Provoz do virtuálního počítače *VM4* se povolí, protože k podsíti *Subnet3* ani k síťovému rozhraní na virtuálním počítači není přidružená žádná skupina zabezpečení sítě. Pokud k podsíti a síťovému rozhraní není přidružená žádná skupina zabezpečení sítě, povolí se přes ně průchod veškerého síťového provozu.

### <a name="outbound-traffic"></a>Odchozí provoz

V případě odchozího provozu zpracuje Azure nejprve pravidla ve skupině zabezpečení sítě přidružené k příslušnému síťovému rozhraní, pokud taková skupina existuje, a pak pravidla ve skupině zabezpečení sítě přidružené k podsíti, pokud taková skupina existuje.

- **VM1:** Zpracují se pravidla zabezpečení ve skupině *NSG2*. Pokud nevytvoříte pravidlo zabezpečení zakazující odchozí provoz přes port 80 do internetu, výchozí pravidlo zabezpečení [AllowInternetOutbound](#allowinternetoutbound) ve skupině *NSG1* i *NSG2* provoz povolí. Pokud skupina *NSG2* obsahuje pravidlo zabezpečení zakazující port 80, provoz se odepře a skupina *NSG1* ho nikdy nevyhodnotí. Pokud chcete na virtuálním počítači zakázat odchozí provoz přes port 80, jedna ze skupin zabezpečení sítě nebo obě musí obsahovat pravidlo zakazující odchozí provoz přes port 80 do internetu.
- **VM2:** Veškerý provoz se odešle přes síťové rozhraní do podsítě, protože k síťovému rozhraní připojenému k virtuálnímu počítači *VM2* není přidružená žádná skupina zabezpečení sítě. Zpracují se pravidla ve skupině *NSG1*.
- **VM3:** Pokud skupina *NSG2* obsahuje pravidlo zabezpečení zakazující port 80, provoz se odepře. Pokud skupina *NSG2* obsahuje pravidlo zabezpečení povolující port 80, povolí se odchozí provoz přes port 80 do internetu, protože k podsíti *Subnet2* není přidružená žádná skupina zabezpečení sítě.
- **VM4:** Veškerý odchozí provoz z virtuálního počítače *VM4* se povolí, protože k síťovému rozhraní připojenému k virtuálnímu počítači ani k podsíti *Subnet3* není přidružená žádná skupina zabezpečení sítě.

Agregovaná pravidla použitá na síťové rozhraní můžete snadno zobrazit v [platných pravidlech zabezpečení](virtual-network-network-interface.md#view-effective-security-rules) pro síťové rozhraní. Pomocí funkce [Ověření toku protokolu IP](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json) v nástroji Azure Network Watcher můžete také určit, jestli je povolená komunikace směřující do síťového rozhraní nebo z něj. Ověření toku protokolu IP vám řekne, jestli je povolená nebo zakázaná komunikace, a které pravidlo zabezpečení sítě povoluje nebo odepírá provoz.

> [!NOTE]
> Skupiny zabezpečení sítě se nepřidružují k síťovým rozhraním v modelu nasazení Resource Manager, ale k podsítím nebo virtuálním počítačům a cloudovým službám nasazeným pomocí modelu nasazení Classic. Další informace o modelech nasazení Azure najdete v článku [Vysvětlení modelů nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

> [!TIP]
> Pokud k tomu nemáte konkrétní důvod, doporučujeme přidružit skupinu zabezpečení sítě k podsíti nebo k síťovému rozhraní, ale ne k oběma. Vzhledem k tomu, že může docházet ke konfliktům mezi pravidly ve skupině zabezpečení sítě přidružené k podsíti a pravidly ve skupině zabezpečení sítě přidružené k síťovému rozhraní, můžou nastat neočekávané problémy s komunikací vyžadující řešení.

## <a name="azure-platform-considerations"></a>Důležité informace o platformě Azure

- **Virtuální IP adresa uzlu hostitele:** Základní služby infrastruktury, například DHCP, DNS a sledování stavu, jsou poskytované prostřednictvím virtualizovaných IP adres hostitele 168.63.129.16 a 169.254.169.254. Tyto veřejné IP adresy patří společnosti Microsoft a jsou to jediné virtualizované IP adresy používané pro tento účel ve všech oblastech. Adresy se mapují na fyzickou IP adresu počítače serveru (uzel hostitele) hostujícího virtuální počítač. Uzel hostitele funguje jako přenos DHCP, rekurzivní překladač DNS a zdroj testů pro test stavu a zdroj testu pro test stavu nástroje pro vyrovnávání zatížení a test stavu počítače. Komunikace směřující na tyto IP adresy nepředstavuje útok. Pokud zablokujete provoz směřující na tyto IP adresy nebo z nich, virtuální počítač možná nebude fungovat správně.
- **Licencování (Služba správy klíčů):** Image Windows spuštěné na virtuálních počítačích musí být licencované. Aby se zajistilo licencování, odesílají se žádosti o licenci na hostitelské servery Služby správy klíčů, které takové dotazy zpracovávají. Požadavek odchází přes port 1688. Pro nasazení využívající konfiguraci [výchozí trasy 0.0.0.0/0](virtual-networks-udr-overview.md#default-route) bude toto pravidlo platformy zakázané.
- **Virtuální počítače ve fondech s vyrovnáváním zatížení:** Použitý zdrojový port a rozsah adres odpovídá zdrojovému počítači, a nikoli nástroji pro vyrovnávání zatížení. Cílový port a rozsah adres odpovídá cílovému počítači, a nikoli nástroji pro vyrovnávání zatížení.
- **Instance služeb Azure:** V podsítích virtuální sítě jsou nasazené instance několika služeb Azure, například HDInsight, prostředí aplikačních služeb a škálovací sady virtuálních počítačů. Úplný seznam služeb, které můžete nasadit do virtuální sítě, najdete v tématu [Virtuální síť pro služby Azure](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Před použitím skupiny zabezpečení sítě na podsíť, ve které je nasazený prostředek, se ujistěte, že znáte požadavky jednotlivých služeb na porty. Pokud odepřete porty, které služba vyžaduje, nebude správně fungovat.
- **Odesílání odchozích e-mailů:** Microsoft doporučuje k odesílání e-mailů ze služby Azure Virtual Machines využívat služby pro přenos přes ověřený protokol SMTP (obvykle připojené přes port TCP 587, ale často i jiný). Služby pro přenos přes protokol SMTP se specializují na reputaci odesílatele, aby se minimalizovala možnost odmítnutí zpráv poskytovateli e-mailu třetích stran. Mezi takové služby pro přenos přes protokol SMTP patří mimo jiné Exchange Online Protection a SendGrid. Používání služeb pro přenos přes protokol SMTP v Azure není nijak omezeno, a to bez ohledu na typ předplatného. 

  Pokud jste své předplatné Azure vytvořili před 15. listopadem 2017, kromě možnosti používat služby pro přenos přes protokol SMTP můžete e-maily odesílat také přímo přes port TCP 25. Pokud jste své předplatné vytvořili po 15. listopadu 2017, možná nebudete moci odesílat e-maily přímo přes port 25. Chování odchozí komunikace přes port 25 závisí na typu vašeho předplatného, a to následujícím způsobem:

     - **Smlouva Enterprise:** Odchozí komunikace přes port 25 je povolená. Odchozí emaily můžete z virtuálních počítačů odesílat přímo externím poskytovatelům e-mailu bez jakýchkoli omezení platformy Azure. 
     - **Průběžné platby:** Odchozí komunikace přes port 25 ze všech prostředků je blokovaná. Pokud potřebujete odesílat e-maily z virtuálního počítače přímo externím poskytovatelům e-mailu (bez použití přenosu přes zabezpečený protokol SMTP), můžete vytvořit žádost o odebrání tohoto omezení. Žádosti se posuzují a schvalují na základě vlastního uvážení Microsoftu a vyhoví se jim pouze po provedení kontrol v souvislosti s možnými podvody. Pokud chcete vytvořit žádost, otevřete případ podpory s typem problému *Technický*, *Možnosti připojení k virtuální síti*, *Nelze odesílat e-maily (SMTP/port 25)*. Do případu podpory zahrňte podrobnosti o tom, proč vaše předplatné potřebuje odesílat e-maily přímo poskytovatelům e-mailu místo používání přenosu přes ověřený protokol SMTP. Pokud má vaše předplatné výjimku, odchozí komunikace přes port 25 jsou schopné pouze virtuální počítače vytvořené po datu udělení výjimky.
     - **MSDN, Azure Pass, Azure v rámci licenčního programu Open License, Azure ve vzdělávání, BizSpark a bezplatná zkušební verze:** Odchozí komunikace přes port 25 ze všech prostředků je blokovaná. Není možné vytvořit žádost o odebrání omezení, protože takovým žádostem se nevyhovuje. Pokud z virtuálního počítače potřebujete odesílat e-maily, musíte k tomu použít službu pro přenos přes protokol SMTP.
     - **Poskytovatel cloudových služeb:** Zákazníci, kteří využívají prostředky Azure prostřednictvím poskytovatele cloudových služeb, si mohou u poskytovatele cloudových služeb vytvořit případ podpory a požádat ho, aby v jejich zastoupení vytvořil případ odblokování, pokud se nedá použít zabezpečený přenos SMTP.

  Pokud vám Azure povolí odesílat e-maily přes port 25, Microsoft nemůže zaručit přijetí příchozích e-mailů z vašeho virtuálního počítače poskytovateli e-mailu. Pokud konkrétní poskytovatel odmítá e-maily z vašeho virtuálního počítače, spolupracujte přímo s daným poskytovatelem a vyřešte případné problémy s doručováním zpráv nebo filtrováním nevyžádané pošty, nebo použijte službu pro přenos přes ověřený protokol SMTP.

## <a name="next-steps"></a>Další kroky

* Zjistěte, jak [vytvořit skupinu zabezpečení sítě](tutorial-filter-network-traffic.md).
