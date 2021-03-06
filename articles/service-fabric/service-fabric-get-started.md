---
title: Nastavení vývojového prostředí ve Windows pro mikroslužby Azure | Dokumentace Microsoftu
description: Nainstalujte modul runtime, sadu SDK a nástroje a vytvořte místní vývojový cluster. Po dokončení této instalace a nastavení budete moci sestavovat aplikace ve Windows.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2018
ms.author: ryanwi
ms.openlocfilehash: ac6d3a23e3afcc3a4c17798db7f63d846b123fba
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "44022109"
---
# <a name="prepare-your-development-environment-on-windows"></a>Příprava vývojového prostředí ve Windows
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

Pokud chcete sestavovat a spouštět [aplikace Azure Service Fabric][1] na vývojovém počítači s Windows, nainstalujte modul runtime Service Fabric, sadu SDK a nástroje. Musíte také [povolit spouštění skriptů Windows PowerShellu](#enable-powershell-script-execution), které jsou součástí sady SDK.

## <a name="prerequisites"></a>Požadavky
### <a name="supported-operating-system-versions"></a>Podporované verze operačních systémů
Pro vývoj jsou podporovány tyto verze operačních systémů:

* Windows 7
* Windows 8 / Windows 8.1
* Windows Server 2012 R2
* Windows Server 2016
* Windows 10

> [!NOTE]
> Podpora Windows 7:
> - Windows 7 ve výchozím nastavení obsahuje jenom prostředí Windows PowerShell 2.0 Rutiny prostředí PowerShell pro Service Fabric vyžadují PowerShell 3.0 nebo novější. Můžete si [stáhnout prostředí Windows PowerShell 5.0][powershell5-download] z webu Microsoft Download Center.
> - Reverzní proxy Service Fabric není ve Windows 7 k dispozici.
>

## <a name="install-the-sdk-and-tools"></a>Instalace sady SDK a nástrojů
### <a name="to-use-visual-studio-2017"></a>Použití sady Visual Studio 2017
Nástroje Service Fabric Tools jsou součástí sady funkcí Vývoj pro Azure v sadě Visual Studio 2017. Povolte tuto úlohu jako součást instalace sady Visual Studio.
Kromě toho budete muset pomocí Instalace webové platformy nainstalovat sadu Microsoft Azure Service Fabric SDK a modul runtime.

* [Instalace sady Microsoft Azure Service Fabric SDK][core-sdk]

### <a name="to-use-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a>Použití sady Visual Studio 2015 (vyžaduje Visual Studio 2015 Update 2 nebo novější)
Pro sadu Visual Studio 2015 se nástroje Service Fabric nainstalují společně se sadou SDK a modulem runtime pomocí Instalace webové platformy:

* [Instalace sady Microsoft Azure Service Fabric SDK a nástrojů][full-bundle-vs2015]

### <a name="sdk-installation-only"></a>Jenom instalace sady SDK
Pokud potřebujete jenom sadu SDK, můžete nainstalovat tento balíček:
* [Instalace sady Microsoft Azure Service Fabric SDK][core-sdk]

Aktuální verze jsou:
* Service Fabric SDK a nástroje 3.2.176
* Modul runtime Service Fabric 6.3.176
* Service Fabric Tools pro Visual Studio 2015 2.3.10710.3
* Visual Studio 2017 15.7 obsahuje Service Fabric Tools for Visual Studio 2.3.10710.1 

Seznam podporovaných verzí najdete v tématu [Podpora pro Service Fabric](service-fabric-support.md)

> [!NOTE]
> Upgraduje se jeden počítač pro aplikaci nebo clusteru se nepodporují clustery (OneBox); Odstranění clusteru OneBox a znovu jej vytvořte, pokud je potřeba provést inovaci clusteru nebo jakýchkoli problémů provádí upgrade aplikace. 

## <a name="enable-powershell-script-execution"></a>Povolení spouštění skriptů prostředí PowerShell
Platforma Service Fabric používá skripty prostředí Windows PowerShell k vytvoření místního vývojového clusteru a k nasazení aplikací ze sady Visual Studio. Systém Windows ve výchozím nastavení spouštění těchto skriptů blokuje. Pokud je chcete povolit, musíte upravit zásady spouštění prostředí PowerShell. Otevřete prostředí PowerShell jako správce a zadejte tento příkaz:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```
## <a name="install-docker-optional"></a>Nainstalujte Docker (volitelné)
[Service Fabric je orchestrátor kontejnerů](service-fabric-containers-overview.md) pro nasazuje mikroslužby napříč clusterem počítačů. Ke spuštění aplikace typu kontejner Windows ve vašem místním vývojovém clusteru, musíte nejdřív nainstalovat Docker pro Windows. Získat [Docker CE pro Windows (stabilní verze)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Po nainstalování a spuštění Dockeru klikněte pravým tlačítkem myši na ikonu na hlavním panelu a vyberte **Switch to Windows containers** (Přepnout na kontejnery Windows). Tento krok se vyžaduje pro spuštění imagí Dockeru založených na Windows.

## <a name="next-steps"></a>Další postup
Teď, když jste dokončili nastavení vývojového prostředí, můžete začít sestavovat a spouštět aplikace.

* [Vytvořte první aplikaci Service Fabric v sadě Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
* [Naučte se nasadit a spravovat aplikace v místním clusteru](service-fabric-get-started-with-a-local-cluster.md)
* [Seznamte se s programovacími modely: Reliable Services a Reliable Actors](service-fabric-choose-framework.md)
* [Prohlédněte si ukázky kódu Service Fabric na GitHubu](https://aka.ms/servicefabricsamples)
* [Vizualizujte cluster pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md)
* [Postupujte podle studijního plánu Service Fabric a získejte obecný úvod k platformě](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)

[1]: http://azure.microsoft.com/campaigns/service-fabric/ "Stránka kampaně Service Fabric"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Odkaz na VS 2015 WebPI"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Odkaz na Dev15 WebPI"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Odkaz na Core SDK WebPI"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
