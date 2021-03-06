---
title: Upgrade služby Azure IoT Hub | Dokumentace Microsoftu
description: Změňte úroveň cen a škálování pro službu IoT Hub získat další funkce pro správu zasílání zpráv a zařízení.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/02/2018
ms.author: kgremban
ms.openlocfilehash: e1342ed574d84ed5b4edd5060c2d6d3ec8bca1a8
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003107"
---
# <a name="how-to-upgrade-your-iot-hub"></a>Postup upgradu služby IoT hub

S růstem vašeho řešení IoT, Azure IoT Hub můžete vertikálně navýšit kapacitu připravený. Azure IoT Hub nabízí dvě úrovně, basic (B) a standard (S), aby zákazníkům, kteří mají používat různé funkce. V rámci jednotlivých úrovní jsou tři velikosti (1, 2 a 3), které určují počet zpráv, které mohou být odesílány denně. 

Pokud máte více zařízení a potřebujete další funkce, existují tři způsoby, jak nastavit službu IoT hub tak, aby odpovídala vašim potřebám:

* Přidáte jednotky ve službě IoT hub. Například každá další jednotky v B1 IoT hub umožňuje další 400 000 zpráv denně. 
* Změna velikosti služby IoT hub. Třeba migrate z vrstvy B1 do vrstvy B2 zvýšit počet zpráv, které může podporovat každá jednotka za den.
* Upgrade na vyšší úroveň. Například upgradujte z úrovně B1 na úroveň S1 pro zasílání zpráv kapacitu, ale pomocí pokročilých funkcí, které jsou na úrovni standard.

Tyto změny můžou nastat bez přerušení existující operace.

Pokud chcete přejít na nižší verzi služby IoT hub, můžete odebrat jednotky a snížení velikosti služby IoT hub. Ale nemůžete přejít na nižší úroveň. Můžete například přesunout z vrstvy S2 pro úroveň S1, ale ne z vrstvy S2 na úroveň B1. Všimněte si také, že pouze jeden typ [edition](https://azure.microsoft.com/pricing/details/iot-hub/) v rámci úrovně je možné zvolit jednotlivé služby IoT Hub. Můžete například vytvořit IoT Hub s více jednotek úrovně S1, ale ne s kombinaci jednotek z různých edicích, jako je například S1 a K3 nebo S1 a S2.

Tyto příklady jsou určené k vám pomohou pochopit, jak nastavit službu IoT hub jako vaše změny v řešení. Konkrétní informace o jednotlivých úrovních funkce byste měli vždy použít [ceny služby Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/). 

## <a name="upgrade-your-existing-iot-hub"></a>Upgrade stávající služby IoT hub 

1. Přihlaste se k [webu Azure portal](https://portal.azure.com/) a přejděte do služby IoT hub. 
2. Vyberte **cen a škálování**. 

   ![Ceny a škálování](./media/iot-hub-upgrade/pricing-scale.png)

3. Pokud chcete změnit úroveň pro vaše centrum, vyberte **úrovně cen a škálování**. Zvolte Nová úroveň a potom klikněte na tlačítko **vyberte**.

   ![Ceny a škálování](./media/iot-hub-upgrade/select-tier.png)

4. Chcete-li změnit počet jednotek v centru, zadejte novou hodnotu v rámci **jednotek služby IoT Hub**. 
5. Vyberte **Uložit** uložte provedené změny. 

Služby IoT hub se nyní upraví a vaše konfigurace jsou beze změny. Všimněte si, že oddíl maximální limit pro úroveň basic služby IoT Hub je 8 a pro úroveň standard je 32. Většina centra IoT hub stačí jenom 4 oddíly. Omezení počtu oddílů je vybrán při vytvoření služby IoT Hub a souvisí s počtem souběžných čtenářů tyto zprávy zprávy typu zařízení cloud. Tato hodnota zůstane beze změny, když migrujete z úrovně basic na úroveň standard. 

## <a name="next-steps"></a>Další postup

Přečtěte si další podrobnosti o [návodu k výběru správné úrovně služby IoT Hub](iot-hub-scaling.md). 

