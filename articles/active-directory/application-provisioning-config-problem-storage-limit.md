---
title: Problém s uložením přihlašovacích údajů správce při konfiguraci zřizování uživatelů pro aplikaci Galerie Azure AD | Dokumentace Microsoftu
description: Jak řešit běžné problémy, kterým čelí při konfiguraci zřizování uživatelů pro aplikace již uvedená v galerii aplikací Azure AD
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
ms.date: 02/21/2018
ms.author: barbkess
ms.reviewer: asmalser
ms.openlocfilehash: b31e4a9a15f4ed9bfb51e26252a00c749ef333ce
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366906"
---
# <a name="problem-saving-administrator-credentials-while-configuring-user-provisioning-to-an-azure-active-directory-gallery-application"></a>Potíže s uložením přihlašovacích údajů správce při konfiguraci zřizování uživatelů pro aplikaci Galerie Azure Active Directory 

Při použití na webu Azure portal ke konfiguraci [automatické zřizování uživatelů](active-directory-saas-app-provisioning.md) pro podnikové aplikace, mohou nastat situace, kde:

* **Přihlašovacích údajů správce** zadaných pro aplikaci jsou platné a **Test připojení** tlačítko funguje. Ale přihlašovací údaje nelze uložit, a vrátí obecnou chybovou zprávu na webu Azure portal.

Pokud založené na SAML jednotného přihlašování je také nakonfigurováno pro stejnou aplikaci, nejpravděpodobnější příčinou chyby je limit podle aplikace, interní úložiště této Azure AD pro certifikáty a přihlašovacích údajů došlo k překročení.

Azure AD momentálně má maximální úložnou kapacitu jednoho kB pro všechny certifikáty, tajných kódů tokenů, přihlašovacích údajů a související konfigurační data související s jednou instancí aplikace (také označované jako hlavní záznam služby ve službě Azure AD).

Pokud založené na SAML jednotného přihlašování je nakonfigurovaná, certifikát používaný k podepisování tokenů SAML je zde uloženy a často využívá více než 50 % prostoru.

Všechny tajné tokeny, identifikátory URI, oznámení e-mailové adresy, uživatelská jména a hesla, které získat zadaný během nastavení zřizování uživatelů může způsobit překročení limitu úložiště.

## <a name="how-to-work-around-this-issue"></a>Jak chcete tento problém vyřešit 

Existují dva možné způsoby, jak tento problém obejít ještě dnes:

1. **Použijte Galerii dvě instance aplikace, jeden pro jednotné přihlašování a jeden pro zřizování uživatelů** – přepnutí aplikace Galerie [rozvoji Linkedinem](saas-apps/linkedinelevate-tutorial.md) jako příklad, můžete přidat rozvoji Linkedinem z galerie a konfigurace je pro jednotné přihlašování. Pro zřizování, přidejte další instanci rozvoji Linkedinem z Galerie aplikací Azure AD a pojmenujte ho "LinkedIn zvýšení (zřizování)." U této druhé instance nakonfigurovat [zřizování](saas-apps/linkedinelevate-provisioning-tutorial.md), ale ne jednotného přihlašování. Při použití tohoto řešení stejného uživatele a skupiny musí být [přiřazené](manage-apps/assign-user-or-group-access-portal.md) pro obě aplikace. 

2. **Snížit objem uložených dat konfigurace** – všechny údaje zadávané do [přihlašovacích údajů správce](active-directory-saas-app-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) oddíl kartě zřizování je uložen na stejném místě jako certifikát SAML. Nemusí být možné zkrátit délku všechna tato data, jako jsou některá pole volitelná konfigurace **e-mailové oznámení** je možné odebrat.

## <a name="next-steps"></a>Další postup
[Konfigurace uživatele zřizování a zrušení zřizování pro aplikace SaaS](active-directory-saas-app-provisioning.md)
