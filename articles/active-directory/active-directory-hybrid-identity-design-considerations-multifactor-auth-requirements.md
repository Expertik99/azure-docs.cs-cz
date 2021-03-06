---
title: Hybridní identita návrh – požadavky na vícefaktorové ověřování Azure | Dokumentace Microsoftu
description: Pomocí řízení podmíněného přístupu Azure Active Directory ověří zvláštní podmínky, kterou vyberete při ověřování uživatele a před povolením přístupu k aplikaci. Po splnění těchto podmínek, uživatel ověří a povolí se přístup k aplikaci.
documentationcenter: ''
services: active-directory
author: billmath
manager: billmath
editor: ''
ms.assetid: 9c59fda9-47d0-4c7e-b3e7-3575c29beabe
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.component: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 0c4d3edabbef6fe5626ce85c753cc7775ff2f1b9
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2018
ms.locfileid: "37915397"
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Určení požadavků na ověření službou Multi-Factor Authentication pro vaše řešení hybridní identity
V tomto světě nastavení mobilních zařízení s uživateli přístup k datům a aplikacím v cloudu a z jakéhokoli zařízení a zabezpečení těchto informací se stal prvořadá.  Každý den se nový nadpis o porušení zabezpečení.  I když není zaručeno před takové porušením, ověřování službou Multi-Factor Authentication poskytuje další úroveň zabezpečení, které pomáhají zabránit těchto porušení.
Začněte tím, že vaše rozhodnutí vyzkoušet požadavky organizace na ověřování službou Multi-Factor Authentication. To znamená co je organizace pokoušíte zabezpečit.  Toto testování je důležité definovat technické požadavky pro nastavení a povolení uživatelé organizace pro ověřování službou Multi-Factor Authentication.

> [!NOTE]
> Pokud nejste obeznámeni s vícefaktorovým Ověřováním a co to dělá, důrazně doporučujeme, abyste si přečetli článek [co je Azure Multi-Factor Authentication?](authentication/multi-factor-authentication.md) předchozí pokračujte ve čtení v této části.
> 
> 

Ujistěte se, že odpovědět následující:

* Je vaší společnosti pokoušíte zabezpečit aplikace od Microsoftu? 
* Jak tyto aplikace jsou publikovány?
* Poskytuje společnosti vzdálený přístup k zaměstnancům přístup k místním aplikacím?

Pokud ano, jaký typ vzdáleného přístupu? Také musíte vyhodnotit, kde budou umístěné uživatele, kteří přistupují k těmto aplikacím. Toto hodnocení se jiný důležitým krokem k definování strategie pro správné ověřování službou Multi-Factor Authentication. Ujistěte se, že odpovědět na následující otázky:

* Pokud budou uživatelé budou umístěné?
* Můžou se být umístěná kdekoli?
* Vaše společnost chce vytvořit omezení podle umístění uživatele?

Jakmile pochopíte tyto požadavky je potřeba také měli zvážit požadavky na uživatele Multi-Factor Authentication. Toto testování je důležité, protože ho budou definovat požadavky na použití služby Multi-Factor authentication. Ujistěte se, že odpovědět na následující otázky:

* Jsou uživatelé obeznámeni s ověřováním Multi-Factor Authentication?
* Využijete, se bude vyžadovat další ověřování?  
  * Pokud ano, vždy, když pocházející z externí sítě nebo přístup k určité aplikace, nebo v jiných podmínkách?
* Uživatelé budou vyžadovat školení o tom, jak nastavit a zaveďte vícefaktorové ověřování?
* Jaké jsou klíčové scénáře, které chce vaše společnost povolení služby Multi-Factor authentication pro uživatele?

Po zvolení odpovědi na otázky předchozí, bude moci porozumět tomu při ověřování službou Multi-Factor Authentication již implementováno místní. Toto testování je důležité definovat technické požadavky pro nastavení a povolení uživatelé organizace pro ověřování službou Multi-Factor Authentication. Ujistěte se, že odpovědět na následující otázky:

* Potřebuje vaše společnost pro ochranu privilegovaných účtů s MFA?
* Potřebuje vaše společnost jak zapnout MFA pro určité aplikace kvůli dodržování předpisů?
* Potřebuje vaše společnost povolte MFA pro všichni oprávnění uživatelé tyto aplikace, nebo jenom správci?
* Třeba máte vždy povolená služba MFA nebo pouze při přihlášení uživatele mimo vaši firemní síť?

## <a name="next-steps"></a>Další postup
[Definování strategie přijetí hybridní identity](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a>Další informace najdete v tématech
[Přehled aspektů návrhu](active-directory-hybrid-identity-design-considerations-overview.md)

