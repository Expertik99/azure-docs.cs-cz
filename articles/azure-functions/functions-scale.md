---
title: "Azure Functions hostování plány porovnání | Microsoft Docs"
description: "Zjistěte, jak si vybrat mezi plánu spotřeby funkce Azure a plán služby App Service."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Azure funkce, funkce, plánu spotřeby, plán služby app service, zpracování událostí, webhooků, dynamické výpočetní, bez serveru architektura"
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/12/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 09bb662e30a97e2741303e2e4630582625954909
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/15/2017
---
# <a name="azure-functions-hosting-plans-comparison"></a>Azure Functions hostování plány porovnání

Azure Functions můžete spustit ve dvou různých režimech: plánu spotřeby a plán služby Azure App Service. Plánu spotřeby automaticky přiděluje výpočetní výkon, když kód běží, horizontálně navýší kapacitu podle potřeby pro zpracování zatížení a potom škáluje, pokud kód není spuštěna. Ano nemusí platit pro nečinnosti virtuální počítače a nemusíte předem záložní kapacita. Tento článek se týká plánu spotřeby [bez serveru](https://azure.microsoft.com/overview/serverless-computing/) modelu aplikace. Podrobnosti o tom, jak funguje plán služby App Service najdete v tématu [podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

>[!NOTE]  
> Hostování Linux je momentálně dostupná jenom na plán služby App Service.

Pokud se nevyznáte v Azure Functions, přečtěte si téma [přehled Azure Functions](functions-overview.md).

Když vytvoříte aplikaci function app, je nutné nakonfigurovat plán hostování pro funkce, které obsahuje aplikaci. V obou režimech instanci *Azure Functions hostitele* provádí funkce. Typ plánu ovládacích prvků:

* Jak jsou hostiteli instance škálovat na více systémů.
* Prostředky, které jsou k dispozici na každém hostiteli.

V současné době je nutné vybrat typ hostování plán při vytváření aplikace funkce. Následně ho nelze změnit. 

Na plán služby App Service je možné škálovat mezi vrstvami přidělit jinou objem prostředků. Azure Functions plánu spotřeby automaticky zpracovává všechny přidělení prostředků.

## <a name="consumption-plan"></a>Plán Consumption

Pokud používáte plánu spotřeby, instance hostitele Azure Functions dynamicky přidat nebo odebrat závislosti na počtu příchozích událostí. Tento plán automaticky přizpůsobí a musíte platit za výpočetní prostředky jenom v případě, že funkce běží. V plánu spotřeby funkci můžete používat maximálně 10 minut. 

> [!NOTE]
> Výchozí hodnota časového limitu pro funkce na plánu spotřeby je 5 minut. Hodnota je možné zvýšit na 10 minut pro aplikaci funkce Změna vlastnosti `functionTimeout` v [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).

Fakturace je založena na počet spuštěních, čas spuštění a paměti. Fakturace je agregovat přes všechny funkce v rámci funkce aplikace. Další informace najdete v tématu [Azure Functions stránce s cenami].

Spotřeba plán je výchozí plán hostování a nabízí následující výhody:
- Platí se pouze až nebudou spuštěny funkcí.
- Automaticky škálovat, i během období vysoké zatížení.

## <a name="app-service-plan"></a>Plán služby App Service

V plánu služby App Service vaše funkce aplikace běží na vyhrazených virtuálních počítačích na Basic, Standard, Premium a izolované SKU, podobně jako webové aplikace, rozhraní API Apps a Mobile Apps. Vyhrazených virtuálních počítačích jsou přiděleny aplikace služby App Service, což znamená, že na hostiteli funkce vždy běží. Plány služby App Service podporovat Linux.

Vezměte v úvahu plán služby App Service v následujících případech:
- Máte existující, nedostatečně virtuálních počítačů, které jsou již spuštěny jiné instance aplikace služby.
- Funkce aplikací pro spuštění nepřetržitě nebo téměř nepřetržitě očekáváte. V takovém případě plán služby App Service může být cenově výhodnější.
- Budete potřebovat další možnosti procesoru nebo paměti, než je zadán plánu spotřeby.
- Budete muset spustit delší než maximální dobu spuštění povolena v plánu spotřeby (of 10 minut).
- Vyžadujete, aby funkce, které jsou dostupné jenom na plán služby App Service, například podporu služby App Service Environment, připojení virtuální sítě nebo virtuální privátní sítě a větší velikosti virtuálních počítačů. 
- Chcete spustit aplikaci funkce v systému Linux, nebo chcete poskytnout vlastní image, na které se mají spouštět funkce.

Virtuální počítač oddělí ze počet spuštěních, čas spuštění a paměti. V důsledku toho nebude platit víc než náklady na instanci virtuálního počítače, které můžete přidělit. Podrobnosti o tom, jak funguje plán služby App Service najdete v tématu [podrobný přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md). 

Plán služby App Service můžete ručně škálovat tak, že přidáte další instance virtuálních počítačů, nebo můžete povolit automatické škálování. Další informace najdete v tématu [ruční nebo automatické škálování počtu instancí](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json). Také můžete škálovat a vybrat jiný plán služby App Service. Další informace najdete v tématu [škálování aplikace v Azure](../app-service/web-sites-scale.md). 

Pokud plánujete spouštět funkce jazyka JavaScript na plán služby App Service, měli byste vybrat plán, který má méně Vcpu. Další informace najdete v tématu [zvolte plány služby App Service jednojádrový](functions-reference-node.md#considerations-for-javascript-functions).  

<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 
<a name="always-on"></a>
###Always On

Pokud spustíte na plán služby App Service, měli byste povolit **Always On** nastavení tak, aby vaše aplikace funkce běží správně. Na plán služby App Service bude přejděte functions runtime nečinnosti po několika minutách nečinnosti, takže pouze aktivace protokolu HTTP bude "probuzení" funkcí. Toto je podobná jak webové úlohy musí mít povolenou funkci Always On. 

Always On je k dispozici pouze na plán služby App Service. V plánu spotřeby platformou automaticky aktivuje funkce aplikace.

## <a name="storage-account-requirements"></a>Požadavky na účet úložiště

Spotřeba plánu nebo plán služby App Service vyžaduje aplikaci funkce obecné účtu úložiště Azure, který podporuje Azure Blob, fronty, soubory a tabulka úložiště. Azure Functions interně používá Azure Storage pro operace, jako je například Správa aktivační události a protokolování spuštěních funkce. Některé účty úložiště nepodporují fronty a tabulky, jako je například účty pouze objekt blob úložiště (včetně úložiště premium) a účty úložiště pro obecné účely s replikací zónově redundantní úložiště. Tyto účty jsou filtrovány z **účet úložiště** okno při vytváření aplikaci funkce.

Další informace o typech účtu úložiště najdete v tématu [Představení služby Azure Storage](../storage/common/storage-introduction.md#introducing-the-azure-storage-services).

## <a name="how-the-consumption-plan-works"></a>Jak funguje s plánem spotřeba

V plánu spotřeby řadičem škálování automaticky přizpůsobí prostředků procesoru a paměti přidáním další instance funkce hostitele, na základě počtu událostí, které jsou aktivována jeho funkce. Každá instance hostitele funkce je omezený na 1,5 GB paměti.

Při spotřeby hostování plán, jsou uloženy soubory kódu funkce na Azure sdílených složek v účtu úložiště hlavní funkce. Pokud odstraníte účet úložiště hlavní funkce aplikace, soubory kódu funkce jsou odstraněny a nelze jej obnovit.

> [!NOTE]
> Pokud používáte aktivační události objektu blob na plánu spotřeby, může být až 10 minut zpoždění při zpracování nové objekty BLOB, pokud aplikaci funkce přešel nečinnosti. Po aplikaci funkce běží, objekty BLOB jsou zpracovávány okamžitě. Abyste se vyhnuli Tato počáteční prodleva, zvažte jednu z následujících možností:
> - Hostování aplikace funkce na plán služby App Service s povolenou funkci Always On.
> - Použijte jiný mechanismus pro aktivaci objektu blob zpracování, např. zprávu fronty, který obsahuje název objektu blob. Příklad, naleznete v části [skriptu jazyka C# a JavaScript příklady pro tento objekt blob vstup a výstup vazby](functions-bindings-storage-blob.md#input--output---example).

### <a name="runtime-scaling"></a>Modul runtime škálování

Azure Functions využívá komponenty s názvem *škálování řadiče* ke sledování frekvence událostí a určení, zda horizontální navýšení kapacity nebo škálování v. Škálování řadiče pomocí heuristické metody pro každý typ aktivační události. Například pokud používáte aktivační procedury fronty Azure storage, škáluje na základě délka fronty a stáří nejstarší zprávy ve frontě.

Jednotka škálování je funkce aplikace. Když je funkce aplikace škálovat na více systémů, další prostředky jsou přiděleny spustit víc instancí služby Azure Functions hostitele. Naopak jako výpočetní, že se snižuje vyžádání, řadičem škálování odebere funkce hostitele instance. Počet instancí se nakonec měřítko nula. Pokud žádná funkce běží v rámci funkce aplikace.

![Škálování řadič sledování událostí a vytváření instancí](./media/functions-scale/central-listener.png)

### <a name="billing-model"></a>Model fakturace

Fakturace plánu spotřeby je podrobně popsán na [Azure Functions stránce s cenami]. Využití je agregován na úrovni aplikace funkce a počty jenom na čas, který se spustí kód funkce. Tady jsou jednotky pro fakturaci: 
* **Spotřeba prostředků v GB sekundách (v GB-s)**. Vypočtená jako kombinace velikost paměti a dobu spuštění pro všechny funkce v rámci funkce aplikace. 
* **Spuštěních**. Počítají pokaždé, když provedení funkce v reakci na události.

[Azure Functions stránce s cenami]: https://azure.microsoft.com/pricing/details/functions
