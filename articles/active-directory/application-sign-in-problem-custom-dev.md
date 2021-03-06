---
title: Potíže při přihlašování k aplikaci vyvinuté | Dokumentace Microsoftu
description: Běžné rrors, které by mohlo být příčinou je nebudou moci přihlásit k aplikaci vytvořili pomocí Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 68ab777efee3af67cfcdddf1744a36aca0f09571
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366698"
---
# <a name="problems-signing-in-to-an-custom-developed-application"></a>Potíže při přihlašování k vlastní vyvinuté aplikaci

Existuje několik chyb, které by mohlo být příčinou je nebudou moci přihlásit do aplikace. Největší důvod uživatelé setkávají, které tento problém je špatně nakonfigurovaný. aplikace.

## <a name="errors-related-to--misconfigured-apps"></a>Chyby související s nesprávné konfigurace aplikace

* Ověřte, že konfigurace na portálu odpovídat, co máte ve vaší aplikaci. Konkrétně porovnejte ID klienta nebo aplikace, adresy URL odpovědí, klienta tajných kódů a klíčů a identifikátor URI ID aplikace.

* Prostředek se vyžádání přístupu ke službě v kódu s nakonfigurovanou oprávnění v porovnání **požadované prostředky** kartu a ujistěte se, že jste pouze požadavky na prostředky, které jste nakonfigurovali.

* Zobrazit [Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory) všechny podobné chyby nebo problémy.

## <a name="next-steps"></a>Další postup

[Příručka pro vývojáře Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[Vyjádření souhlasu a integraci aplikací do služby Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications>)<br>

[Videí k Azure AD v2.0 a vyjádření souhlasu konvergované aplikace](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory>)
