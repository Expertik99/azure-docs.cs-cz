---
title: Správa programů a ovládacích prvků pro službu Azure AD kontrol přístupu | Dokumentace Microsoftu
description: Ve vaší organizaci shromažďování a uspořádání využitím kontrol přístupu Azure Active Directory jako ovládací prvky můžete vytvořit další programy pro každou zásad správného řízení, řízení rizik a dodržování předpisů iniciativy.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: a1a08ac29605e66dc14ac5e32d21e00dad8cb655
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622282"
---
# <a name="manage-programs-and-their-controls"></a>Správa programů a jejich ovládacích prvků 

Azure Active Directory (Azure AD) obsahuje kontrol přístupu členů skupiny a přístup k aplikacím. Příklady ovládacích prvků zajistit dohledu, pro který má přístup k vaší organizaci členství ve skupinách a aplikace. Organizace můžou použít tyto ovládací prvky efektivně vyřešit jejich zásad správného řízení, řízení rizik a požadavky na dodržování předpisů.

## <a name="create-and-manage-programs-and-their-controls"></a>Vytvoření a Správa programů a jejich ovládacích prvků
Jak sledovat a shromažďovat kontroly přístupu pro různé účely jejich uspořádáním do programů může zjednodušit. Každý kontroly přístupu lze propojit do programu. Potom při přípravě sestavy pro auditor můžete soustředit na kontroly přístupu v oboru pro konkrétní iniciativy.  Programy a výsledky kontroly přístupu jsou viditelné pro uživatele v roli globálního správce, správce uživatelských účtů, správce zabezpečení nebo Čtenář zabezpečení.

Pokud chcete zobrazit seznam aplikací, přejděte na [stránku kontrol přístupu](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/) a vyberte **programy**.

**Výchozí Program** je vždy k dispozici. Pokud jste globální správce nebo role uživatele účet správce, můžete vytvořit další programy. Například můžete mít jeden program pro každou dodržování předpisů iniciativy nebo obchodní cíle.

Pokud už nepotřebujete programu a nemá žádné ovládací prvky na sebe, můžete ho odstranit.

## <a name="next-steps"></a>Další postup

- [Vytváření kontroly přístupu pro členy skupiny nebo přístupu k aplikaci](active-directory-azure-ad-controls-create-access-review.md)
- [Načíst výsledky kontroly přístupu](active-directory-azure-ad-controls-retrieve-access-review.md)
