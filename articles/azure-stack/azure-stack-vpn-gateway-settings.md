---
title: Nastavení služby VPN gateway pro Azure Stack | Dokumentace Microsoftu
description: Přečtěte si o nastaveních pro brány VPN Gateway, které používáte pro Azure Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: fa8d3adc-8f5a-4b4f-8227-4381cf952c56
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/14/2018
ms.author: brenduns
ms.openlocfilehash: 6380936766bb0f3848811be305783c274867b0fc
ms.sourcegitcommit: a3a0f42a166e2e71fa2ffe081f38a8bd8b1aeb7b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/01/2018
ms.locfileid: "43381863"
---
# <a name="vpn-gateway-configuration-settings-for-azure-stack"></a>Konfigurace nastavení služby VPN gateway pro Azure Stack

*Platí pro: Azure Stack integrované systémy a Azure Stack Development Kit*

Brána VPN je typem brány virtuální sítě, která odesílá šifrovaný síťový provoz mezi vaší virtuální sítí ve službě Azure Stack a vzdálenou bránu VPN. Vzdálené brány VPN může být v Azure, zařízení ve vašem datovém centru nebo v jiné lokalitě.  Pokud je síťové připojení mezi dva koncové body, můžete navázat zabezpečené připojení VPN typu Site-to-Site (S2S) mezi těmito dvěma sítěmi.

Připojení brány VPN se spoléhá na konfiguraci více zdrojů, z nichž každý obsahuje konfigurovatelné nastavení. Tento článek popisuje prostředky a nastavení, které se týkají brány sítě VPN pro virtuální síť, kterou vytvoříte v modelu nasazení Resource Manager. Můžete najít popisy a diagramy topologie pro každé připojení řešení [informace o VPN Gateway pro Azure Stack](azure-stack-vpn-gateway-about-vpn-gateways.md).

## <a name="vpn-gateway-settings"></a>Nastavení služby VPN gateway

### <a name="gateway-types"></a>Typy bran

Každá virtuální síť Azure Stack podporuje brány jedné virtuální sítě, který musí být typu **Vpn**.  Tato podpora se liší od Azure, což podporuje další typy.  

Při vytváření brány virtuální sítě, musíte se ujistit, že je typ brány odpovídá vaší konfiguraci. Vyžaduje bránu sítě VPN `-GatewayType Vpn`, například:

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn
-VpnType RouteBased
```

### <a name="gateway-skus"></a>Skladové jednotky (SKU) brány

Při vytváření brány virtuální sítě musíte určit SKU brány, které chcete použít. Vyberte SKU, které splňují vaše požadavky na základě typů úloh, propustnosti, funkcí a SLA.

Azure Stack nabízí VPN gateway skladové položky uvedené v následující tabulce.

|   | Propustnost brány sítě VPN |Brána VPN maximální počet tunelových propojení IPsec |
|-------|-------|-------|
|**Základní SKU**  | 100 Mb/s  | 10    |
|**Standardní SKU**           | 100 Mb/s  | 10    |
|**SKU pro vysoký výkon** | 200 Mb/s    | 5 |

### <a name="resizing-gateway-skus"></a>Změna velikosti SKU brány

Azure Stack nepodporuje změny velikosti mezi podporované starší skladové položky SKU.

Podobně Azure Stack nepodporuje změnu velikosti z podporovaných starší verze SKU (Basic, Standard a HighPerformance) na novější SKU nepodporuje v Azure (VpnGw1, VpnGw2 a VpnGw3.)

### <a name="configure-the-gateway-sku"></a>Konfigurace skladové položky brány

#### <a name="azure-stack-portal"></a>Portál Azure Stack

Pokud použijete k vytvoření brány virtuální sítě Resource Manageru na portálu Azure Stack, můžete pomocí rozevíracího seznamu vyberte SKU brány. Možnosti, které máte na výběr odpovídají typ brány a typ sítě VPN, kterou jste vybrali.

#### <a name="powershell"></a>Prostředí Power Shell

Následující příklad Powershellu Určuje, **- GatewaySku** jako VpnGw1.

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1
-GatewayType Vpn -VpnType RouteBased
```

### <a name="connection-types"></a>Typy připojení

V modelu nasazení Resource Manager Každá konfigurace vyžaduje typ připojení brány konkrétní virtuální sítě. Hodnoty dostupné prostředí PowerShell Resource Manageru pro **- ConnectionType** jsou:

* Protokol IPsec

V následujícím příkladu Powershellu se vytvoří připojení S2S, který vyžaduje typ připojení IPsec.

```PowerShell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

### <a name="vpn-types"></a>Typy sítě VPN

Při vytváření brány virtuální sítě pro konfiguraci brány VPN, musíte zadat typ sítě VPN. Typ sítě VPN, kterou zvolíte, závisí na topologie připojení, který chcete vytvořit.  Typ sítě VPN může také záviset na hardwaru, který používáte. Konfigurace S2S vyžadují zařízení VPN. Některá zařízení VPN podporují pouze určitého typu sítě VPN.

> [!IMPORTANT]  
> Azure Stack v současné době podporuje pouze typ sítě VPN na základě trasy. Pokud vaše zařízení podporuje pouze sítě VPN na základě zásad, nejsou podporována připojení na těchto zařízeních ze služby Azure Stack.  
>
> Kromě toho Azure Stack nepodporuje používání selektory provozu na základě zásad pro brány podle postupu v tuto chvíli, protože vlastní konfigurace zásad IPSec/IKE nejsou podporovány.

* **PolicyBased**: sítě VPN založené na zásadách šifrují pakety a směrují je do tunelových propojení IPsec na základě zásad IPsec nakonfigurovaných pomocí kombinace předpon adres mezi vaší místní sítí a virtuální sítě Azure Stack. Zásady nebo selektor provozu, je obvykle přístupový seznam v konfiguraci zařízení VPN.

  >[!NOTE]
  >PolicyBased se nepodporuje v Azure, ale ne ve službě Azure Stack.

* **RouteBased**: konfiguraci IP předávání nebo směrovací tabulce ke směrování paketů do svých příslušných rozhraní tunelových propojení sítí VPN RouteBased použití tras. Rozhraní tunelového propojení potom šifrují nebo dešifrují pakety směřující do tunelových propojení nebo z nich. Zásady nebo selektor provozu pro sítě VPN typu RouteBased jsou nakonfigurované jako any-to-any (nebo použijte zástupné znaky.) Ve výchozím nastavení nedá se změnit. Typ sítě VPN typu RouteBased hodnotu typu RouteBased.

Následující příklad Powershellu Určuje **- VpnType** jako RouteBased. Při vytváření brány, ujistěte se, že **- VpnType** je správný pro vaši konfiguraci.

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig
-GatewayType Vpn -VpnType RouteBased
```

### <a name="gateway-requirements"></a>Požadavky na bránu

V následující tabulce jsou uvedeny požadavky pro brány VPN Gateway.

| |Brány sítě VPN PolicyBased Basic | Brána VPN typu RouteBased Basic | Brána VPN typu RouteBased Standard | RouteBased vysoce výkonná brána sítě VPN|
|--|--|--|--|--|
| **Připojení Site-to-Site (S2S připojení)** | Není podporováno | Konfigurace VPN typu RouteBased | Konfigurace VPN typu RouteBased | Konfigurace VPN typu RouteBased |
| **Metoda ověřování**  | Není podporováno | Předsdílený klíč pro připojení S2S  | Předsdílený klíč pro připojení S2S  | Předsdílený klíč pro připojení S2S  |   
| **Maximální počet připojení S2S**  | Není podporováno | 10 | 10| 5|
|**Podpora aktivního směrování (BGP)** | Nepodporuje se | Nepodporuje se | Podporováno | Podporováno |

### <a name="gateway-subnet"></a>Podsíť brány

Než vytvoříte bránu VPN, musíte vytvořit podsíť brány. Podsíť brány obsahuje IP adresy, které používají bránu virtuální sítě virtuálních počítačů a služeb. Při vytváření brány virtuální sítě, virtuální počítače brány se nasazují do podsítě brány a nakonfigurovanou povinné nastavení služby VPN gateway. **Není** nenasazujte nic jiného (třeba dalších virtuálních počítačů) do podsítě brány.

>[!IMPORTANT]
>Pro správné fungování podsítě brány je nutné, aby měla název **GatewaySubnet**. Azure Stack používá tento název k identifikaci podsíť, kterou chcete nasadit virtuální počítače brány virtuální sítě a služby.

Při vytváření podsítě brány zadáte počet IP adres, které podsíť obsahuje. Virtuální počítače brány a služby brány se přidělují IP adresy v podsíti brány. Některé konfigurace vyžadují víc IP adres než jiné. Podívejte se na pokyny pro konfiguraci, kterou chcete vytvořit a ověřit, že podsíť brány, kterou chcete vytvořit tyto požadavky splňuje.

Navíc je dobré mít že podsíť brány obsahuje dostatek IP adres pro zpracování dalších budoucích konfiguracích. I když můžete vytvořit podsíť brány malá jako minimální velikostí/29, doporučujeme že vytvořit podsíť brány o velikosti/28 nebo větší (/ 28, / 27, / 26 atd.) Tímto způsobem, pokud v budoucnu přidat funkce nemusíte dovolí bránu, pak odstraňte a znovu vytvořte podsíť brány umožňující další IP adresy.

Následující příklad Powershellu pro Resource Manager ukazuje podsíť brány s názvem GatewaySubnet. Uvidíte, že zápis CIDR Určuje velikost/27, která zajistíte dostatek IP adres u většiny konfigurací, které momentálně existují.

```PowerShell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

> [!IMPORTANT]
> Při práci s podsítěmi brány nepřidružujte skupinu zabezpečení sítě (NSG) k podsíti brány. Pokud byste k této podsíti přidružili skupinu zabezpečení sítě, brána sítě VPN by mohla přestat fungovat podle očekávání. Další informace o skupinách zabezpečení sítě najdete v tématu [co je skupina zabezpečení sítě?](/azure/virtual-network/virtual-networks-nsg).

### <a name="local-network-gateways"></a>Brány místní sítě

Při vytváření konfiguraci brány VPN v Azure, místní síťové brány často představuje místní umístění. Ve službě Azure Stack představuje všechny vzdálené zařízení VPN, který je umístěný mimo Azure Stack. To může být zařízení VPN ve vašem datovém centru (i vzdálené datacentrum) nebo VPN Gateway v Azure.

Pojmenujte bránu místní sítě, veřejnou IP adresu zařízení VPN a zadáte předpony adres, které jsou na místní umístění. Azure zjistí pro síťový provoz předpony cílových adres consults konfiguraci, kterou jste zadali pro bránu místní sítě a směruje pakety odpovídajícím způsobem.

Následující příklad Powershellu vytvoří novou bránu místní sítě:

```PowerShell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```

V některých případech budete muset upravit nastavení místní síťové brány. Například když přidáváte nebo odebíráte rozsah adres, nebo pokud IP adresa zařízení VPN bude měnit. Zobrazit [úprava nastavení místní síťové brány pomocí Powershellu](/azure/vpn-gateway/vpn-gateway-modify-local-network-gateway).

## <a name="ipsecike-parameters"></a>Parametry protokolu IPsec/IKE

Při nastavování připojení VPN ve službě Azure Stack, musíte nakonfigurovat na obou koncích připojení.  Při konfiguraci připojení VPN mezi Azure Stack a hardwarové zařízení, jako je přepínač nebo směrovač, který funguje jako brána VPN, zařízení může výzvu k zadání dalších nastavení.

Na rozdíl od Azure, která podporuje několik nabídek jako iniciátor i respondér, Azure Stack podporuje jenom jednu nabídku.

### <a name="ike-phase-1-main-mode-parameters"></a>Parametry protokolu IKE fáze 1 (hlavní režim)

| Vlastnost              | Hodnota|
|-|-|
| Verze IKE           | IKEv2 |
|Skupina Diffie-Hellman   | Skupina 2 (1 024 bitů) |
| Metoda ověření | Předsdílený klíč |
|Algoritmy šifrování a hash | AES256, SHA256 |
|Životnost SA (čas)     | 28 800 sekund|

### <a name="ike-phase-2-quick-mode-parameters"></a>Parametry protokolu IKE fáze 2 (rychlý režim)

| Vlastnost| Hodnota|
|-|-|
|Verze IKE |IKEv2 |
|Šifrování a hash algoritmy (šifrování)     | GCMAES256|
|Šifrování a hash algoritmy (ověřování) | GCMAES256|
|Životnost SA (čas)  | 27 000 sekund  |
|Životnost SA (bajty) | 33,553,408     |
|Metoda Perfect Forward Secrecy (PFS) |Žádný<sup>viz poznámka 1</sup> |
|Detekce mrtvých partnerských zařízení | Podporováno|  

* *Poznámka 1:* starší než verze 1807, používá Azure Stack hodnotu PFS2048 pro ideální Forward Secrecy (PFS).

## <a name="next-steps"></a>Další postup

[Připojení pomocí ExpressRoute](azure-stack-connect-expressroute.md)
