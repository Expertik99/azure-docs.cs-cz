---
title: Přehled služby Service Fabric Mesh v Azure | Microsoft Docs
description: Přečtěte si podrobnosti o službě Azure Service Fabric Mesh. Pomocí Service Fabric Mesh můžete nasadit a škálovat aplikace bez starostí o zajištění potřebné infrastruktury.
services: service-fabric-mesh
keywords: ''
author: rwike77
ms.author: ryanwi
ms.date: 06/27/2018
ms.topic: overview
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 65a9b1afcc0e1e6d4fcbb60a38ab0764e6fe2f18
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/24/2018
ms.locfileid: "39226440"
---
# <a name="what-is-service-fabric-mesh"></a>Co je Service Fabric Mesh?

Azure Service Fabric Mesh je plně spravovaná služba, která vývojářům umožňuje nasazovat aplikace zajišťující mikroslužby, aniž by museli spravovat virtuální počítače, úložiště nebo sítě. Aplikace hostované ve službě Service Fabric Mesh se spouštějí a škálují, aniž by bylo třeba starat se o jejich infrastrukturu.  Service Fabric Mesh tvoří clustery složené z tisíců počítačů.  Veškeré operace správy clusteru jsou před vývojáři skryté. Jednoduše nahrajte svůj kód a určete potřebné prostředky, požadavky na dostupnost a limity prostředků.  Service Fabric Mesh automaticky přidělí infrastrukturu potřebnou pro nasazení vaší aplikace a také se postará o případná selhání infrastruktury, aby vaše aplikace měla trvale vysokou dostupnost. Na vás bude jen starost o stav a rychlost reakce aplikace, nikoli o infrastrukturu.  

[!INCLUDE [preview note](./includes/include-preview-note.md)]

Tento článek obsahuje přehled klíčových výhod služby Service Fabric Mesh.

## <a name="great-developer-experience"></a>Skvělé prostředí pro vývojáře

Service Fabric Mesh podporuje každý programovací jazyk nebo architekturu, které lze spustit v kontejneru. Podpora nástrojů Visual Studio Code a sady Visual Studio 2017 poskytuje výkonné prostředí pro úpravy a ladění pro aplikace .NET a .NET Core. 

Služba Service Fabric Mesh umožňuje:

- Migraci existujících aplikací do kontejnerů metodou „lift and shift“ a tím i možnost modernizace a spouštění aplikací ve velkém měřítku. 
- Sestavování a nasazování nových aplikací mikroslužeb ve velkém měřítku v Azure.  Integraci s jinými službami Azure nebo existujícími aplikacemi spouštěnými v kontejnerech. Každá mikroslužba je součástí zabezpečené aplikace izolované od sítě a s určenými zásadami řízení prostředků definovanými pro jádra procesoru, paměť, místo na disku a další vlastnosti.
- Integraci a rozšiřování stávajících aplikací, aniž by bylo nutné je upravovat. Zapojení existujících aplikací do nové aplikace pomocí vaší vlastní virtuální sítě.  
- Modernizaci existujících aplikací Cloud Services migrací na službu Service Fabric Mesh.  

## <a name="simple-operational-lifecycle"></a>Jednoduchý provozní životní cyklus

Snadno spravujte spuštěné aplikace včetně upgradů a správy verzí, sledování aplikací a ladění v produkčních prostředích. Tyto aplikace se mohou skládat z jedné mikroslužby, nebo z několika mikroslužeb izolovaných ve svých vlastních sítích. Aplikace běží efektivně díky rychlému nasazení, umístění a převzetí služeb při selhání.

Služba Service Fabric Mesh umožňuje:

- Nasazení a správu aplikací bez nutnosti explicitně zřizovat a spravovat infrastrukturu.  Service Fabric Mesh zřídí, aktualizuje, opravuje a udržuje potřebnou infrastrukturu za vás.
- Nastavení nepřetržité integrace pomocí integrovaných nástrojů pro snadné balíčkování a nasazování aplikací.
- Využití všech funkcí prostředků Azure Resource Manageru (například záznam pro audit a [řízení přístupu na základě rolí (RBAC)](/azure/role-based-access-control/overview)) díky tomu, že všechny prostředky (například aplikace, služby, tajné kódy atd.) nasazené do služby SF Mesh v Azure jsou současně prostředky Azure Resource Manageru. 
- Nasazení a správu prostředků pomocí [webu Azure Portal](https://portal.azure.com), šablon Správce prostředků nebo knihoven Azure CLI/PowerShell.
- Nastavení sledování provozu a upozorňování pomocí služby [Application Insights](/azure/application-insights/) (nebo jiného nástroje podle vašeho výběru) a zachycování provozních a diagnostických dat z platformy. 
- Přístup k informacím o diagnostice získaným z aplikačního modelu pomocí [Application Insights](/azure/application-insights/) nebo jiného nástroje podle vašeho výběru.
- Optimalizaci využití prostředků zadáním pravidel automatického škálování pro služby přímo v definici aplikace.  (připravuje se)
- Vytvoření izolované sítě a hranic zabezpečení, což jsou výkonné funkce zvláště v kombinaci s kontejnery Hyper-V. Izolování síťového provozu do a od služby pomocí několika IP adres pro každou službu a izolovaných virtuálních sítí pro jednotlivé služby.  (připravuje se) 


## <a name="mission-critical-platform-capabilities"></a>Možnosti platformy pro kritické aplikace

Service Fabric Mesh vytvoří kolekci clusterů, které překračují hranice [zóny dostupnosti Azure](/azure/availability-zones/az-overview) nebo místní geopolitické hranice. Aplikace jsou popsány sadou záměrů, jako jsou například škálování, požadavky na hardware, požadavky na odolnost a zásady zabezpečení.  Po nasazení aplikace najde Service Fabric Mesh optimální místo, kde ji spustit.

Služba Service Fabric Mesh umožňuje:

- Využít vysokou dostupnost, škálování nahoru i dolů, vyhledatelnost, orchestraci, směrování zpráv, spolehlivé zasílání zpráv, upgrady bez výpadků, správu zabezpečení a tajných kódů, zotavení po havárii, správa stavů, správu konfigurací a distribuované transakce.
- Vybírat při vytváření aplikace z několika různých aplikačních modelů.
- Využít možností platformy poskytovaných koncovými body REST prostřednictvím vazeb specifických pro různé jazyky generovaných pomocí Swaggeru.
- Nasazení aplikací napříč [zónami dostupnosti](/azure/availability-zones/az-overview) a různými oblastmi pro geografickou spolehlivost.
- Využít všechny možnosti zabezpečení a dodržování předpisů, které Azure nabízí.

## <a name="next-steps"></a>Další kroky

K nasazení ukázkového projektu ze sady Visual Studio stačí jen pár jednoduchých kroků. Další informace najdete v článku [Vytvoření webu ASP.NET Core](service-fabric-mesh-quickstart-dotnet-core.md). 


<!-- Links -->

[service-fabric-overview]: ../service-fabric/service-fabric-overview.md
