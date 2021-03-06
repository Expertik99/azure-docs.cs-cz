---
title: 'Kurz: Ladění webové aplikace Azure Service Fabric Mesh | Microsoft Docs'
description: V tomto kurzu se dozvíte, jak ladit aplikaci Azure Service Fabric Mesh, která je spuštěná v místním clusteru.
services: service-fabric-mesh
documentationcenter: .net
author: TylerMSFT
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2018
ms.author: twhitney
ms.custom: mvc, devcenter
ms.openlocfilehash: c519d24a8401823039f3d6598276890ec6498bbc
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/17/2018
ms.locfileid: "41918613"
---
# <a name="tutorial-debug-a-service-fabric-mesh-web-application"></a>Kurz: Ladění webové aplikace Service Fabric Mesh

V tomto kurzu, který je druhou částí série, se dozvíte, jak ladit webovou aplikaci Azure Service Fabric Mesh v místním vývojovém clusteru.

V tomto kurzu se dozvíte:

> [!div class="checklist"]
> * Co se stane, když sestavíte aplikaci Azure Service Fabric Mesh
> * Jak nastavit zarážky, abyste mohli sledovat volání služba-služba

V této sérii kurzů se naučíte:
> [!div class="checklist"]
> * [Sestavit aplikaci Service Fabric Mesh](service-fabric-mesh-tutorial-create-dotnetcore.md)
> * Ladit aplikaci místně
> * [Publikovat aplikaci do Azure](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)

[!INCLUDE [preview note](./includes/include-preview-note.md)]

## <a name="prerequisites"></a>Požadavky

Než začnete s tímto kurzem:

* Pokud ještě nemáte předplatné Azure, můžete si [vytvořit bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.

* Zkontrolujte, že jste [nastavili vývojové prostředí](service-fabric-mesh-howto-setup-developer-environment-sdk.md), což zahrnuje instalaci modulu runtime Service Fabric, sady SDK, Dockeru a sady Visual Studio 2017.

## <a name="download-the-to-do-sample-application"></a>Stažení ukázkové aplikace pro seznam úkolů

Pokud jste si nevytvořili ukázkovou aplikaci pro seznam úkolů v [první části této série kurzů](service-fabric-mesh-tutorial-create-dotnetcore.md), můžete si ji stáhnout. V příkazovém okně naklonujte spuštěním následujícího příkazu úložiště ukázkové aplikace do místního počítače.

```
git clone https://github.com/azure-samples/service-fabric-mesh
```

Aplikace se nachází v adresáři `src\todolistapp`.

## <a name="build-and-debug-on-your-local-cluster"></a>Sestavení a ladění v místním clusteru

Po načtení projektu se image Dockeru automaticky sestaví a nasadí do místního clusteru. Tento proces může chvíli trvat. Pokud chcete monitorovat průběh v sadě Visual Studio v podokně **Výstup**, nastavte v rozevíracím seznamu **Zobrazit výstup z** položku **Nástroje Service Fabric**.

Stisknutím klávesy **F5** zkompilujete a spustíte vaši službu místně. Visual Studio při místním spuštění a ladění projektu provede tyto akce:

* Zkontroluje, jestli je Docker for Windows spuštěný a nastavený tak, aby jako operační systém kontejneru používal Windows.
* Stáhne všechny chybějící základní image Dockeru. Tato část může nějakou dobu trvat.
* Sestaví (nebo znovu sestaví) image Dockeru, která se používá k hostování projektu kódu.
* Nasadí a spustí příslušný kontejner na místním vývojovém clusteru Service Fabric.
* Spustí vaše služby a dostane se k případným zarážkám, které jste nastavili.

Až se místní nasazení dokončí a Visual Studio spustí vaši aplikaci, otevře se okno prohlížeče s výchozí ukázkovou webovou stránkou.

**Tipy pro ladění**

V současné době dochází k problému, který způsobuje, že volání `using (HttpResponseMessage response = client.GetAsync("").GetAwaiter().GetResult())` selže při připojování ke službě. Může k tomu dojít, kdykoli se změní IP adresa hostitele. Řešení je následující:

1. Odeberte aplikaci z místního clusteru (v sadě Visual Studio vyberte **Sestavení** > **Vyčistit řešení**).
2. V nástroji Service Fabric Local Cluster Manager vyberte **Stop Local CLuster** (Zastavit místní cluster) a pak **Start Local Cluster** (Spustit místní cluster).
3. Znovu nasaďte aplikaci (v sadě Visual Studio stiskněte **F5**).

Pokud se zobrazí chyba se sdělením, že **žádný místní cluster Service Fabric není spuštěný**, zkontrolujte, že je spuštěný nástroj Service Fabric Local Custer Manager (LCM), klikněte pravým tlačítkem na ikonu LCM na hlavním panelu a pak klikněte na **Start Local Cluster** (Spustit místní cluster). Po jeho spuštění se vraťte do sady Visual Studio a stiskněte klávesu **F5**.

Pokud se po spuštění aplikace zobrazí chyba **404**, může to znamenat, že proměnné prostředí v souboru **service.yaml** nejsou správné. Podle pokynů pro [vytvoření proměnných prostředí](https://docs.microsoft.com/azure/service-fabric-mesh/service-fabric-mesh-tutorial-create-dotnetcore#create-environment-variables) zkontrolujte, jestli jsou správně nastavené proměnné `ApiHostPort` a `ToDoServiceName`.

Pokud se zobrazí chyby sestavení v **service.yaml**, zkontrolujte, že se k odsazení řádků používají mezery a ne tabulátory. Navíc je prozatím potřeba sestavit aplikaci s anglickým národním prostředím.

### <a name="debug-in-visual-studio"></a>Ladění v sadě Visual Studio

Při ladění aplikace Service Fabric Mesh v sadě Visual Studio používáte místní vývojový cluster Service Fabric. Pokud chcete zobrazit, jak se položky úkolů načítají z back-endové služby, zaměřte se při ladění na metodu OnGet().
1. V projektu **WebFrontEnd** otevřete **Stránky** > **Index.cshtml** > **Index.cshtml.cs** a nastavte zarážku v metodě **Get** (řádek 17).
2. V projektu **ToDoService** otevřete **TodoController.cs** a nastavte zarážku v metodě **OnGet** (řádek 15).
3. Vraťte se do prohlížeče a aktualizujte stránku. Dostanete se k zarážce v metodě `OnGet()` front-endu. Můžete se podívat na proměnnou `backendUrl`, abyste zjistili, jak se proměnné prostředí definované v souboru **service.yaml** zkombinují do adresy URL, která se používá ke kontaktování back-endové služby.
4. Vynechejte (F10) volání `client.GetAsync(backendUrl).GetAwaiter().GetResult())` a dostanete se k zarážce `Get()` kontroleru. V této metodě můžete vidět, jak se seznam položek úkolů načítá ze seznamu v paměti.
5. Až skončíte, zastavte ladění projektu v sadě Visual Studio stisknutím klávesy **Shift+F5**.
 
## <a name="next-steps"></a>Další kroky

V této části kurzu jste se naučili:

> [!div class="checklist"]
> * Co se stane, když sestavíte aplikaci Azure Service Fabric Mesh
> * Jak nastavit zarážky, abyste mohli sledovat volání služba-služba

Přejděte k dalšímu kurzu:
> [!div class="nextstepaction"]
> [Publikování webové aplikace Service Fabric Mesh](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)