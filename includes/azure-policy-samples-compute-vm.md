---
title: zahrnout soubor
description: zahrnout soubor
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: eedce1ef3a4e7d56cfaf3142a72e370c96f9f575
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664453"
---
### <a name="virtual-machines"></a>Virtuální počítače

|  |  |
|---------|---------|
| [Povolení vlastní image virtuálního počítače ze skupiny prostředků](../articles/azure-policy/scripts/allow-custom-vm-image.md) |  Vyžaduje, aby vlastní image pocházely ze schválené skupiny prostředků. Zadejte název schválené skupiny prostředků. |
| [Povolené skladové položky účtů úložišť a virtuálních počítačů](../articles/azure-policy/scripts/allowed-skus-storage.md) | Vyžaduje, aby účty úložišť a virtuální počítače používaly schválené skladové položky. K ověření schválených skladových položek slouží integrované zásady. Pole schválených skladových položek virtuálních počítačů a pole schválených skladových položek účtů úložišť zadáváte vy. |
| [Schválené image virtuálních počítačů](../articles/azure-policy/scripts/allowed-custom-images.md) | Vyžaduje, aby se ve vašem prostředí nasazovaly jenom schválené vlastní image. Zadejte pole ID schválených imagí. |
| [Provedení auditu, když neexistuje rozšíření](../articles/azure-policy/scripts/audit-ext-not-exist.md) | Provede audit, pokud není s virtuálním počítačem nasazené rozšíření. Zadáte vydavatele a typ rozšíření, u kterého chcete zkontrolovat, jestli je nasazené. |
| [Nepovolená rozšíření virtuálního počítače](../articles/azure-policy/scripts/not-allowed-vm-ext.md) | Zakáže použití zadaných rozšíření. Zadáte pole obsahující typy zakázaných rozšíření. |