---
title: Ukázkový skript Azure PowerShellu – Otevření portu aplikace v nástroji pro vyrovnávání zatížení | Microsoft Docs
description: Ukázkový skript Azure PowerShellu – Otevření portu pro aplikaci Service Fabric v nástroji pro vyrovnávání zatížení Azure
services: service-fabric
documentationcenter: ''
author: rwike77
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 05/18/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 0549f5f2b5b0f8fdfc18b8c091c1065d6137b8c6
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/20/2018
ms.locfileid: "34366167"
---
# <a name="open-an-application-port-in-the-azure-load-balancer"></a>Otevření portu aplikace v nástroji pro vyrovnávání zatížení Azure

Aplikace Service Fabric spuštěná v Azure se nachází za nástrojem pro vyrovnávání zatížení Azure. Tento ukázkový skript otevře v nástroji pro vyrovnávání zatížení port a tím umožní aplikaci Service Fabric komunikovat s externími klienty. Podle potřeby upravte parametry. Pokud je váš cluster ve skupině zabezpečení sítě, [přidejte také příchozí pravidlo skupiny zabezpečení sítě](service-fabric-powershell-add-nsg-rule.md), které povolí příchozí provoz.

V případě potřeby nainstalujte modul PowerShellu pro Service Fabric se sadou [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Ukázkový skript

[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in the load balancer")]

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazuje na příslušnou část dokumentace.

| Příkaz | Poznámky |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Získá prostředek Azure.  |
| [Get-AzureRmLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer) | Získá nástroj pro vyrovnávání zatížení Azure. |
| [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) | Přidá do nástroje pro vyrovnávání zatížení konfiguraci sondy.|
| [Get-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/get-azurermloadbalancerprobeconfig) | Získá konfiguraci sondy pro nástroj pro vyrovnávání zatížení. |
| [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) | Přidá do nástroje pro vyrovnávání zatížení konfiguraci pravidla. |
| [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer) | Nastaví cílový stav pro nástroj pro vyrovnávání zatížení. |

## <a name="next-steps"></a>Další kroky

Další informace o modulu Azure PowerShellu najdete v [dokumentaci k Azure PowerShellu](/powershell/azure/overview).

Další ukázky PowerShellu pro Azure Service Fabric najdete v [ukázkách Azure PowerShellu](../service-fabric-powershell-samples.md).
