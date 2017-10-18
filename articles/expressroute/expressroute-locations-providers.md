---
title: "Umístění a poskytovatelé připojení: Azure ExpressRoute | Dokumentace Microsoftu"
description: "Tento článek obsahuje podrobný přehled o umístěních, kde jsou nabízené služby, a jak se připojit k oblastem Azure. Řazení je podle umístění."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
ms.assetid: feb67da3-5abc-4acb-bad4-f78e3c541ded
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/06/2017
ms.author: kaanan
ms.openlocfilehash: 951b8e1dfc86d8fc73cf8d6f596558b73169169c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="expressroute-partners-and-peering-locations"></a>Partneři ExpressRoute a umístění partnerského vztahu

> [!div class="op_single_selector"]
> * [Umístění podle poskytovatele](expressroute-locations.md)
> * [Poskytovatelé podle umístění](expressroute-locations-providers.md)


Tabulky v tomto článku poskytují informace o poskytovatelích připojení ExpressRoute, zeměpisném pokrytí ExpressRoute, cloudových službách Microsoftu podporovaných přes ExpressRoute a systémových integrátorech (SI) ExpressRoute.

## <a name="partners"></a>Poskytovatelé připojení ExpressRoute
Služba ExpressRoute je podporovaná ve všech umístěních a oblastech Azure. Následující mapa obsahuje seznam oblastí Azure a umístění ExpressRoute. Umístění ExpressRoute odkazují na ty, kde má Microsoft partnerský vztah s několika poskytovateli služeb.

![Mapa umístění][0]

Pokud jste připojení k aspoň jednomu umístění ExpressRoute v rámci geopolitické oblasti, budete mít přístup ke službám Azure napříč všemi oblastmi v rámci geopolitické oblasti. 

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Mapování oblastí Azure na umístění ExpressRoute v rámci geopolitické oblasti
Následující tabulka obsahuje mapování oblastí Azure na umístění ExpressRoute v rámci geopolitické oblasti.

| **Geopolitická oblast** | **Oblasti Azure** | **Umístění ExpressRoute** |
| --- | --- | --- |
| **Severní Amerika** |Východní USA, Západní USA, Východní USA 2, Západní USA 2, Střed USA, Střed USA – jih, Střed USA – sever, Střed USA – západ, Střední Kanada, Východní Kanada |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, New York, Seattle, Silicon Valley, Washington DC, Montreal, Quebec City, Toronto |
| **Jižní Amerika** |Brazílie – jih |Sao Paulo |
| **Evropa** |Severní Evropa, Západní Evropa, Spojené království – západ, Spojené království – jih |Amsterdam, Dublin, Londýn, Newport (Wales), Paříž |
| **Asie** |Východní Asie, Jihovýchodní Asie |Hongkong, Singapur |
| **Japonsko** |Japonsko – západ, Japonsko – východ |Ósaka, Tokio |
| **Austrálie** |Austrálie – jihovýchod, Austrálie – východ |Melbourne, Sydney |
| **Indie** |Indie – západ, Indie – střed, Indie – jih |Čennaj, Bombaj |
| **Jižní Korea** |Korea – střed, Korea – jih |Busan, Soul |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Oblasti a geopolitické hranice pro národní cloudy
Následující tabulka obsahuje informace o oblastech a geopolitických hranicích pro národní cloudy.

| **Geopolitická oblast** | **Oblasti Azure** | **Umístění ExpressRoute** |
| --- | --- | --- |
| **Cloud vlády USA** |USA (Gov) – Iowa, USA (Gov) – Virginia, US DoD – střed, US DoD – východ  |Chicago, Dallas, New York, Seattle, Silicon Valley, Washington DC |
| **Čína** |Severní Čína, Východní Čína |Peking, Šanghaj |
| **Německo** |Střední Německo, Východní Německo |Berlín, Frankfurt |

Připojení přes geopolitické oblasti není pomocí standardní SKU pro ExpressRoute podporované. Aby bylo podporované globální připojení, budete muset povolit doplněk ExpressRoute Premium. Připojení k prostředím národních cloudů není podporované. Podle potřeby můžete spolupracovat se svým poskytovatelem připojení.

## <a name="locations"></a>Umístění poskytovatele připojení

Následující tabulka uvádí umístění připojení a poskytovatele služby pro každé umístění. Seznam poskytovatelů služeb a umístění, ve kterých působí, najdete v článku [Umístění podle poskytovatelů služeb](expressroute-locations.md#locations). 


### <a name="production-azure"></a>Produkční prostředí Azure
| **Umístění** | **Poskytovatelé služeb** |
| --- | --- |
| **Amsterdam** |Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Internet Solutions – Cloud Connect, Interxion, KPN, Level 3 Communications, Megaport, Orange, Tata Communications, TeleCity Group, Telefonica, Telenor, Verizon, Zayo Group |
| **Atlanta** |Equinix |
| **Busan** |LG CNS |
| **Čennaj** | Airtel+, Global CloudXchange (GCX), SIFY, Tata Communications |
| **Chicago** |AT&T NetBond, Comcast, Coresite, Equinix, Level 3 Communications, Megaport, Verizon, Zayo Group |
| **Dallas** |Aryaka Networks, AT&T NetBond, Cologix, Equinix, Level 3 Communications, Megaport, Verizon, Zayo Group+ |
| **Denver** |CoreSite |
| **Dublin** |Colt, Interxion, Telecity Group |
| **Hongkong** |Aryaka Networks, British Telecom, China Telecom Global, Equinix, Megaport, Orange, PCCW Global Limited, Tata Communications, Verizon |
| **Las Vegas** |Level 3 Communications, Megaport |
| **Londýn** |AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions – Cloud Connect, Interxion, Jisc, Level 3 Communications, Megaport, MTN, NTT Communications, Orange, Tata Communications, Telecity Group, Telehouse – KDDI, Telenor, Verizon, Vodafone, Zayo Group+ |
| **Los Angeles** |CoreSite, Equinix, Megaport, NTT, Zayo Group |
| **Melbourne** |Equinix, Megaport, NEXTDC, Telstra Corporation |
| **Miami** |C3ntro+, Megaport, Neutrona Networks |
| **Montreal** |Bell Canada, Cologix |
| **Bombaj** |Airtel+, Global CloudXchange (GCX), Sify, Tata Communications |
| **New York** |Coresite, Equinix, Megaport, Zayo Group |
| **Newport(Wales)** |Next Generation Data |
| **Ósaka** |Equinix, Internet Initiative Japan Inc. – IIJ, NTT Communications, NTT SmartConnect, Softbank |
| **Paříž** |Colt, Intercloud, Interxion, Equinix, Orange |
| **Québec** | Megaport |
| **San Antonio** |Megaport |
| **Sao Paulo** |Ascenty Data Centers+, Equinix, Level 3 Communications, Neutrona Networks, Telefonica, UOLDIVEO |
| **Seattle** |Equinix, Level 3 Communications, Megaport |
| **Soul** |KINX, LG CNS, Sejong Telecom |
| **Silicon Valley** |Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink+, Comcast, Console, Coresite, Equinix, Level 3 Communications, Megaport, Orange, Tata Communications, Verizon, Zayo Group |
| **Singapur** |Aryaka Networks, AT&T NetBond, British Telecom, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Verizon |
| **Sydney** |AARNet, AT&T NetBond, British Telecom, Equinix, Megaport, NEXTDC, Orange, Telstra Corporation, Verizon |
| **Tokio** |Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, Internet Initiative Japan Inc. – IIJ, NTT Communications, Softbank, Verizon |
| **Toronto** |AT&T NetBond, Bell Canada, Cologix, Console, Equinix, Megaport, Telus, Zayo Group |
| **Washington, D.C.** |Aryaka Networks, AT&T NetBond, British Telecom, Comcast, Coresite, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, Verizon, Zayo Group |

 **+** označuje brzké uvedení.

### <a name="national-cloud-environments"></a>Prostředí národních cloudů

### <a name="us-government-cloud"></a>Cloud vlády USA
| **Umístění** | **Poskytovatelé služeb** |
| --- | --- |
| **Chicago** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |Equinix, Megaport, Verizon |
| **New York** |Equinix, Level 3 Communications+, Verizon |
| **Silicon Valley** | Equinix, Level 3 Communications |
| **Seattle** | Equinix |
| **Washington, D.C.** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |

### <a name="china"></a>Čína
| **Umístění** | **Poskytovatelé služeb** |
| --- | --- |
| **Peking** |China Telecom |
| **Šanghaj** |China Telecom |

Další informace najdete v tématu [ExpressRoute v Číně](http://www.windowsazure.cn/home/features/expressroute/).

### <a name="germany"></a>Německo
| **Umístění** | **Poskytovatelé služeb** |
| --- | --- |
| **Berlín** |Colt+, e-shelter, Megaport+, T-Systems |
| **Frankfurt** |Colt, Equinix, Interxion |

## <a name="c1partners"></a>Možnosti připojení prostřednictvím poskytovatelů Exchange
Pokud poskytovatel připojení není v předchozích částech uvedený, můžete přesto vytvořit připojení.

* Zkontrolujte u svého poskytovatele připojení, jestli je připojený k některé z výměn v předchozí tabulce. Můžete zkontrolovat následující odkazy, abyste získali další informace o službách nabízených poskytovateli výměny. Několik poskytovatelů připojení je už připojeno k ethernetovým výměnám.
  * [Cologix](http://www.cologix.com/)
  * [Console](https://www.consoleconnect.com/partners/cloudsaas/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [InterXion](http://www.interxion.com/)
  * [NextDC](http://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
* Rozšířil váš poskytovatel připojení vaši síť ke zvolenému umístění partnerského vztahu?
  * Ujistěte se, že poskytovatel připojení rozšiřuje vaše připojení vysoce dostupným způsobem tak, aby neexistovaly žádné jediné body selhání.
* Abyste se připojili k Microsoftu, objednejte si okruh ExpressRoute s výměnou jako poskytovatel připojení.
  * Při nastavení připojení postupujte podle kroků v tématu [Vytvoření okruhu ExpressRoute](expressroute-howto-circuit-classic.md).

## <a name="c1partners"></a>Možnosti připojení prostřednictvím dalších poskytovatelů služeb
| **Umístění** | **Výměna** | **Poskytovatelé připojení** |
| --- | --- | --- |
| **Amsterdam** | Equinix, Telecity | Eurofiber, Fastweb S.p.A, Nianet, Telia |
| **Chicago** | Equinix | Lightower, Windstream |
| **Dallas** | Equinix, Megaport | C3ntro Telecom, Cox Business, Data Foundry, Transtelco |
| **Frankfurt** | Telecity | Nianet, QSC AG |
| **Hongkong** | Equinix | Macroview Telecom |
| **Londýn** | Equinix, euNetworks, Telecity | Bezeq International Ltd., Epsilon, Exponential E, HSO, NexGen Networks, Tamares Telecom, Zain |
| **Los Angeles** | Equinix |Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix, Equinix | Airgate Technologies. Inc, Cogeco Peer 1, Rogers, Zirro |
| **New York** |Equinix, Megaport | Altice Business, Lightower, Webair |
| **Seattle** |Equinix | Alaska Communications |
| **Silicon Valley** |Equinix | Cox Business, Windstream |
| **Singapur** |Equinix |1CLOUDSTAR, Epsilon Telecommunications Limited, LGA Telecom, United Information Highway (UIH) |
| **Slough** | Equinix | HSO|
| **Sydney** | Megaport | Macquarie Telecom Group|
| **Tokio** | Equinix | ARTERIA Networks Corporation, BroadBand Tower, Inc. |
| **Toronto** | Equinix | Airgate Technologies. Inc, Cogeco Peer 1, Rogers, Thinktel, Zirro|
| **Washington, D.C.** |Equinix | Altice Business, Gtt Communications Inc, Epsilon, Lightower, Masergy, Windstream |

## <a name="expressroute-system-integrators"></a>Systémoví integrátoři ExpressRoute
Povolení soukromého připojení podle vlastních potřeb může být náročné, v závislosti na rozsahu vaší sítě. Můžete spolupracovat s kterýmkoli systémovým integrátorem uvedeným v následující tabulce, aby vám pomohl s připojením k ExpressRoute.

| **Kontinent** | **Systémoví integrátoři** |
| --- | --- |
| **Asie** |Avanade Inc., OneAs1a |
| **Austrálie** | Ensyst, IT Consultancy, MOQdigital, Vigilant.IT |
| **Evropa** |Avanade Inc., Altogee, Bright Skies GmbH, Inframon, MSG Services, New Signature, Nelite, Orange Networks, sol-tec |
| **Severní Amerika** |Avanade Inc., Equinix Professional Services, FlexManage, Perficient, Presidio |
| **Jižní Amerika** |Avanade Inc. |
## <a name="next-steps"></a>Další kroky
* Další informace o ExpressRoute najdete v tématu [ExpressRoute – nejčastější dotazy](expressroute-faqs.md).
* Zkontrolujte, že jsou splněné všechny požadavky. Viz [Požadavky služby ExpressRoute](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Mapa umístění"
