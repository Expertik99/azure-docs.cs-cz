---
title: Konfigurace nastavení role adresáře Azure AD v PIM | Dokumentace Microsoftu
description: Zjistěte, jak nakonfigurovat nastavení role adresáře Azure AD v Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 08/27/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 2d7226f18eb922eaba3c8184656560c33202ef56
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/04/2018
ms.locfileid: "43665430"
---
# <a name="configure-azure-ad-directory-role-settings-in-pim"></a>Konfigurace nastavení role adresáře Azure AD v PIM

Správce privilegovaných rolí, odkud můžete přizpůsobit Azure AD Privileged Identity Management (PIM) ve své organizaci, včetně změnou prostředí pro uživatele, který je aktivaci přiřazení oprávněné role.

## <a name="open-role-settings"></a>Otevřete nastavení role

Postupujte podle těchto kroků a otevřete nastavení pro role adresáře Azure AD.

1. Otevřít **Azure AD Privileged Identity Management**.

1. Klikněte na tlačítko **role adresáře Azure AD**.

1. Klikněte na tlačítko **nastavení**.

    ![Role adresáře Azure AD – nastavení](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. Klikněte na tlačítko **role**.

1. Klikněte na roli, jehož nastavení chcete konfigurovat.

    ![Role adresáře Azure AD – nastavení role](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    Na stránce nastavení pro každou roli existuje několik nastavení, která můžete konfigurovat. Tato nastavení ovlivní jenom uživatele, kteří jsou **oprávněné** přiřazení, ne **trvalé** přiřazení.

## <a name="activations"></a>Aktivace

Použití **aktivací** posuvník nastavit maximální dobu v hodinách, že role zůstane aktivní, než vyprší její platnost. Tato hodnota může být v rozmezí 1 až 72 hodin.

## <a name="notifications"></a>Oznámení

Použití **oznámení** přepínač k určení, zda systém odesílá e-maily k potvrzení, že se aktivovat roli správce. To může být užitečné pro zjišťování nezákonných nebo neoprávněné aktivace.

## <a name="incidentrequest-ticket"></a>Lístek incidentu nebo žádosti

Použití **lístek incidentu nebo žádosti** přepínač k určení, jestli se vyžaduje oprávněné správce zahrnovat číslo lístku, pokud se aktivovat svoje role. To může být užitečné, když provádíte auditování přístupu k roli.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Použití **ověřování službou Multi-Factor Authentication** přepínač k určení, jestli se má vyžadovat, aby uživatelé ověřit svoji identitu pomocí MFA, před aktivací jejich role. Mají pouze k ověření tohoto jednou na relaci, není vždy, když se aktivovat roli. Existují dva tipy, jak vzít v úvahu při povolení vícefaktorového ověřování:

* Uživatelé, kteří mají účty Microsoft pro jejich e-mailové adresy (obvykle @outlook.com, ale ne vždy) nejde zaregistrovat pro Azure MFA. Pokud chcete přiřazení rolí uživatelům s účty Microsoft, by měl daly trvalých správců nebo zakázání MFA pro danou roli.
* Vícefaktorové ověřování pro vysoce privilegované role nelze zakázat pro službu Azure AD a Office 365. Toto je funkce bezpečný přístup z více, protože pracovníci v těchto rolích by měly být pečlivě chráněné:  
  
  * Správce aplikace
  * Správce serveru Proxy aplikací
  * Správce fakturace  
  * Správce dodržování předpisů  
  * Správce služby CRM
  * Schvalovatel přístupu Lockboxu zákazníka
  * Zapisovač adresáře  
  * Správce Exchange  
  * Globální správce
  * Správce služby Intune
  * Správce poštovní schránky  
  * Partnerská podpora úrovně 1  
  * Partnerská podpora úrovně 2  
  * Správce privilegovaných rolí
  * Správce zabezpečení  
  * Správce SharePointu  
  * Správce Skypu pro firmy  
  * Správce uživatelských účtů  

Další informace najdete v tématu [vícefaktorové ověřování (MFA) a PIM](pim-how-to-require-mfa.md).

## <a name="require-approval"></a>Vyžadovat schválení

Pokud chcete k aktivaci role vyžadovat schválení, postupujte podle těchto kroků.

1. Nastavte **vyžadovat schválení** přepnout na **povoleno**. V podokně rozšíří možnosti vybrat schvalovatele.

    ![Role adresáře Azure AD – nastavení – vyžadovat schválení](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    Pokud jste **neměňte** zadejte jakékoli schvalovatelé, výchozí schvalovatelé se správci privilegovaných rolí. Správce privilegovaných rolí by vyžadoval schválení **všechny** žádosti o aktivaci pro tuto roli.

1. Chcete-li zadat schvalovatele, klikněte na tlačítko **vybrat schvalovatele**.

    ![Role adresáře Azure AD – nastavení – vyžadovat schválení](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. Vyberte jednoho nebo více schvalovatelů a potom klikněte na tlačítko **vyberte**. Můžete vybrat uživatele nebo skupiny. Doporučuje se aspoň 2 schvalovatele. Schvalování sebe sama není povolený.

    Vyberte požadované možnosti se zobrazí v seznamu vybraných schvalovatelů.

1. Po zadání všech vašich nastavení role, klikněte na tlačítko **Uložit** uložte provedené změny.


<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

## <a name="next-steps"></a>Další postup

- [Přiřazení role adresáře Azure AD v PIM](pim-how-to-add-role-to-user.md)
- [Konfigurace výstrah zabezpečení pro role adresáře Azure AD v PIM](pim-how-to-configure-security-alerts.md)
