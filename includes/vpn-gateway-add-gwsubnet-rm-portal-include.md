---
title: zahrnout soubor
description: zahrnout soubor
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: a1fa4d58cefa82e70c036d697957254531042b9c
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/23/2018
ms.locfileid: "30196876"
---
1. Na [portálu](http://portal.azure.com) přejděte na virtuální síť Resource Manageru, pro kterou chcete vytvořit bránu virtuální sítě.
2. Na stránce vaší virtuální sítě v části **Nastavení** kliknutím na **Podsítě** rozbalte stránku Podsítě.
3. Na stránce **Podsítě** kliknutím na **+Podsíť brány** otevřete stránku **Přidat podsíť**.

  ![Přidání podsítě brány](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addgwsub.png "Přidání podsítě brány")
4. **Název** podsítě se automaticky vyplní hodnotou GatewaySubnet. To je požadovaná hodnota, aby služba Azure podsíť rozpoznala jako podsíť brány. Upravte automaticky vyplněné hodnoty v poli **Adresní prostor** tak, aby odpovídaly požadavkům vaší konfigurace, a pak kliknutím na **OK** v dolní části stránky vytvořte podsíť.

  ![Přidání podsítě](./media/vpn-gateway-add-gwsubnet-rm-portal-include/addsubnetgw.png "Přidání podsítě")