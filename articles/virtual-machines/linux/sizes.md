---
title: Velikosti virtuálních počítačů s Linuxem v Azure | Dokumentace Microsoftu
description: Obsahuje seznam různých velikostí, které jsou k dispozici pro virtuální počítače s Linuxem v Azure.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/06/2018
ms.author: jonbeck
ms.openlocfilehash: 5ee3d29ceada238c5e4a633b501f63ed307ba76e
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900863"
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Velikosti virtuálních počítačů s Linuxem v Azure
Tento článek popisuje dostupné velikosti a možnosti pro virtuální počítače Azure, které lze použít ke spouštění Linuxových aplikací a úloh. Také poskytuje důležité informace o nasazení je potřeba vědět při plánování použití těchto prostředků. Tento článek je také k dispozici pro [Windows virtual machines](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


| Typ                     | Velikosti           |    Popis       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Obecné účely](sizes-general.md)          | B, Dsv3, Dv3, DSv2, řada Dv2, Av2  | Vyvážený poměr procesorů k paměti. Ideální pro testování a vývoj, malé a střední databáze a webové servery s nízkým a středním provozem. |
| [Optimalizované z hlediska výpočetních služeb](sizes-compute.md)        | Fsv2 služby Fs, F             | Vysoký poměr procesorů k paměti. Vhodné pro webové servery se středním provozem, síťová zařízení, dávkové procesy a aplikační servery.        |
| [Optimalizované z hlediska paměti](sizes-memory.md)         | Esv3 Ev3, M, GS, G, DSv2, řada Dv2  | Vysoký poměr paměti na procesor. Velmi vhodné pro servery relačních databází, střední a velké mezipaměti a analýzu v paměti.                 |
| [Optimalizované z hlediska úložiště](sizes-storage.md)        | Ls                | Vysoká propustnost disku a V/V. Ideální pro databáze NoSQL, SQL a velké objemy dat.                                                         |
| [GPU](sizes-gpu.md)            | NV, síťového adaptéru, NCv2, NCv3, ND.            | Specializované virtuální počítače určené pro náročné vykreslování grafiky a úpravy videa, jakož i model trénování a odvozování (ND) s obsáhlým learningem. K dispozici s jedním nebo několika grafickými procesory.       |
| [Vysokovýkonné výpočetní prostředí](sizes-hpc.md) | H       | Naše nejrychlejší a procesorově nejvýkonnější virtuální počítače s volitelnými síťovými rozhraními s vysokou propustností (RDMA). 

<br>

- Informace o různých velikostí cenách najdete v tématu [ceny Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux). 
- Dostupnost velikostí virtuálních počítačů v oblasti Azure, najdete v části [dostupné produkty v jednotlivých oblastech](https://azure.microsoft.com/regions/services/).
- Obecná omezení na virtuálních počítačích Azure najdete v tématu [předplatného Azure a limity, kvóty a omezení](../../azure-subscription-service-limits.md).
- Další informace o tom [Azure výpočetních jednotek (ACU)](acu.md) můžete porovnat výpočetní výkon jednotlivých SKU v Azure.


## <a name="rest-api"></a>REST API

Informace o použití rozhraní REST API k dotazování pro velikosti virtuálních počítačů naleznete v následujících tématech:

- [Seznam dostupných velikostí virtuálních počítačů pro změnu velikosti](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes)
- [Seznam dostupných velikostí virtuálních počítačů pro předplatné](https://docs.microsoft.com/rest/api/compute/virtualmachines/listall)
- [Seznam dostupných velikostí virtuálních počítačů ve skupině dostupnosti](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes)

## <a name="acu"></a>ACU

Další informace o tom [Azure výpočetních jednotek (ACU)](acu.md) můžete porovnat výpočetní výkon jednotlivých SKU v Azure.

## <a name="benchmark-scores"></a>Výsledky srovnávacích testů

Přečtěte si další informace o výpočetní výkon pro virtuální počítače s Linuxem pomocí [CoreMark srovnávacích](compute-benchmark-scores.md).

## <a name="next-steps"></a>Další postup

Další informace o různých velikostech virtuálních počítačů, které jsou k dispozici:
- [Obecné účely](sizes-general.md)
- [Optimalizované z hlediska výpočetních služeb](sizes-compute.md)
- [Optimalizované z hlediska paměti](sizes-memory.md)
- [Optimalizované z hlediska úložiště](sizes-storage.md)
- [GPU](sizes-gpu.md)
- [Vysokovýkonné výpočetní prostředí](sizes-hpc.md)
- Zkontrolujte, [předchozí generace](sizes-previous-gen.md) stránky A Standard, Dv1 (D1-4 a D11-14 v1) a A8-A11-series



