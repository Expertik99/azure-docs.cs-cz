---
title: Co je základní ochranu podmíněného přístupu Azure Active Directory? -preview | Dokumentace Microsoftu
description: Zjistěte, jak základní ochranu zajistí, že budete mít aspoň základní úroveň zabezpečení povolené ve vašem prostředí Azure Active Directory.
services: active-directory
keywords: conditional access to apps, conditional access with Azure AD, secure access to company resources, conditional access policies
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/08/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 57fef112186834ead76f6223e32cb358e4d6d053
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "44024069"
---
# <a name="what-is-baseline-protection-preview"></a>Co je základní ochranu (preview)?  

V posledním roce útoky na identitu vzrůst 300 %. Pro ochranu před útoky stále se zvětšujícím prostředí, Azure Active Directory (Azure AD) představuje novou funkci volat základní ochranu. Základní ochranu je sada předdefinovaných [zásady podmíněného přístupu](../active-directory-conditional-access-azure-portal.md). Cílem těchto zásad je zajistit, že budete mít minimálně úroveň standardních hodnot zabezpečení povolené ve všech edicích služby Azure AD. 

Tento článek poskytuje přehled o základní ochranu ve službě Azure Active Directory.


 
## <a name="require-mfa-for-admins"></a>Vyžadovat vícefaktorové ověřování pro správce

Uživatelé s přístupem k privilegovaným účtům mají neomezený přístup k prostředí. Kvůli výkonu, které nemají tyto účty by měly zpracovávat s zvláštní pozornost. Běžným způsobem zlepšit ochranu privilegovaných účtů je tak, aby vyžadovala silnější formu ověření účtu, když se používají k registraci. Ve službě Azure Active Directory získáte silnější ověření účtu tak, že vyžaduje vícefaktorové ověřování (MFA).  

**Vyžadovat vícefaktorové ověřování pro správce** je základní zásady, které vyžadují vícefaktorové ověřování pro následující role adresáře: 

- Globální správce  

- Správce SharePointu  

- Správce Exchange  

- Správce podmíněného přístupu  

- Správce zabezpečení  


![Azure Active Directory](./media/baseline-protection/01.png)

Tyto zásady na směrný plán vám poskytuje možnost vyloučit skupiny a uživatele. Můžete chtít vyloučit jeden *[nouzovou přístup pro správu účtu](../users-groups-roles/directory-emergency-access.md)* zajistit zablokován přístup tenanta.


## <a name="enable-a-baseline-policy"></a>Povolit zásady směrný plán 

Zásady na směrný plán jsou ve verzi preview, ale jsou ve výchozím nastavení není aktivovaný. Musíte ručně povolit zásadu, pokud chcete aktivovat. Poté, co tato funkce dosáhla obecné dostupnosti, tyto zásady jsou ve výchozím nastavení aktivuje. Změna plánované chování je důvod, proč mají navíc aktivovat a deaktivovat třetí možnost – nastavení stavu zásad: **povolit zásady automaticky v budoucnu**. Výběrem této možnosti umožníte Microsoftu při rozhodování, kdy chcete aktivovat zásady.      


**Pokud chcete povolit zásady směrný plán:**  

1. Přihlaste se k [webu Azure portal](https://portal.azure.com) jako globální správce, správce zabezpečení nebo správce podmíněného přístupu.

2. V **webu Azure portal**, v levém navigačním panelu klikněte na **Azure Active Directory**.

    ![Azure Active Directory](./media/baseline-protection/02.png)

3. Na **Azure Active Directory** stránku, **zabezpečení** klikněte na tlačítko **podmíněného přístupu**.

    ![Podmíněný přístup](./media/baseline-protection/05.png)

4. V seznamu zásad, klikněte na zásadu, která začíná **základní zásady:**. 

5. Povolit příslušné zásady, klikněte na tlačítko **použít zásady okamžitě**.

6. Klikněte na **Uložit**. 
 
  
 

## <a name="what-you-should-know"></a>Co byste měli vědět 

Při správě vlastní zásady podmíněného přístupu vyžaduje licenci Azure AD Premium, standardní hodnoty zásady jsou k dispozici ve všech edicích služby Azure AD.     

Role adresáře, které jsou součástí základní zásady jsou nejvíce privilegované role Azure AD. 

Pokud jste privilegovaný účty, které se používají ve skriptech, byste měli vyměnit pomocí [Identity spravované služby (MSI)](../managed-identities-azure-resources/overview.md) nebo [instanční s certifikáty](../../azure-resource-manager/resource-group-authenticate-service-principal.md). Jako dočasné řešení můžete vyloučit konkrétní uživatelské účty z základní zásady. 

Základní zásady platí pro toky starší verze ověřování jako POP, IMAP, starší klientském počítači Office. 




## <a name="next-steps"></a>Další postup

Další informace naleznete v tématu:

- [Zabezpečení vaší infrastruktury identit v pěti krocích](https://docs.microsoft.com/azure/security/azure-ad-secure-steps)

- [Co je podmíněný přístup v Azure Active Directory?](overview.md) 

