---
title: Přidání nebo změna rolí správce Azure předplatné | Dokumentace Microsoftu
description: Popisuje, jak přidat nebo změnit Azure Spolusprávce, Správce služeb a správce účtu
services: ''
documentationcenter: ''
author: genlin
manager: jlian
editor: ''
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/14/2018
ms.author: genli
ms.openlocfilehash: b0e24e498acd823242b3613abb62df978466d56d
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/24/2018
ms.locfileid: "42918308"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Přidat nebo změnit správce předplatného Azure

Pokud chcete spravovat přístup k prostředkům Azure, potřebujete mít odpovídající roli správce. Tento článek popisuje postup přidání nebo změna rolí správce pro uživatele na úrovni předplatného.

> [!div class="nextstepaction"]
> [Pomozte nám vylepšit Azure fakturační dokumentace](https://go.microsoft.com/fwlink/p/?linkid=2010091)

## <a name="what-administrator-role-do-i-use"></a>Jakou roli správce můžu použít?

Azure nabízí několik různých rolí. Ke správě přístupu k prostředkům, můžete použít role správce klasickém modelu předplatného, jako je například Správce služeb a spolupracujícího správce nebo novější systém autorizace nazývá řízení přístupu na základě role (RBAC). Pro zajištění lepší kontroly a zjednodušit správu přístupu, doporučujeme použít RBAC pro všechny potřeby správy přístupu. Pokud je to možné doporučujeme změnit konfiguraci stávající zásady přístupu pomocí RBAC. Další informace najdete v tématu [co je řízení přístupu na základě rolí (RBAC)](../role-based-access-control/overview.md) a [pochopit různé role v Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-for-a-subscription-in-azure-portal"></a>Přidat vlastníka předplatného RBAC na webu Azure portal 

Pokud chcete do předplatného Azure někoho přidat jako správce, přiřaďte mu roli [Vlastník](../role-based-access-control/built-in-roles.md#owner) (role RBAS) v oboru předplatného. Role vlastníka umožňuje správu prostředků v předplatném, které jste přiřadili, a nemá oprávnění k přístupu k jiným předplatným.

1. Navštivte [ **předplatná** na webu Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Vyberte předplatné, ke kterému chcete udělit přístup.
3. Vyberte **Přidat**.
   (Pokud tlačítko Přidat chybí, nemáte oprávnění k přidání oprávnění.)
4. V seznamu vyberte **Řízení přístupu (IAM)**.
5. V poli **Role** vyberte **Vlastník**. 
6. V poli **Přiřadit přístup k** vyberte **Uživatel, skupina nebo aplikace Azure AD**. 
7. Do pole **Vybrat** zadejte e-mailovou adresu uživatele, kterého chcete přidat jako vlastníka. Vyberte uživatele a pak vyberte **Uložit**.

    ![Snímek obrazovky zobrazující vybrané role vlastníka](./media/billing-add-change-azure-subscription-administrator/add-role.png)

To poskytuje úplný přístup uživatele na všechny prostředky včetně práva na delegovat přístup ostatním uživatelům. Pokud chcete poskytnout přístup na jiného oboru, jako jsou skupiny prostředků, přejděte **řízení přístupu (IAM)** okna pro daný obor.

## <a name="add-or-change-co-administrator"></a>Přidat nebo změnit spolusprávce

Jako spolusprávce je možné přidat pouze [vlastníka](../role-based-access-control/built-in-roles.md#owner). Ostatní uživatele například s rolemi [Přispěvatel](../role-based-access-control/built-in-roles.md#contributor) nebo [Čtenář](../role-based-access-control/built-in-roles.md#reader) není možné přidat jako spolusprávce.

> [!TIP]
> Stačí přidat jako spolusprávce vlastníka, pokud uživatel potřebuje ke správě nasazení Azure classic. Doporučujeme, abyste pomocí RBAC pro jiné účely.

1. Pokud jste tak dosud neučinili, přidáte uživatele jako vlastníka postupujte podle pokynů výše.
2. **Klikněte pravým tlačítkem na** vlastník uživatele jste právě přidali a pak vyberte **přidat jako spolusprávce**. Pokud se nezobrazí **přidat jako spolusprávce** možnost, aktualizujte stránku nebo zkuste jiný internetového prohlížeče. 

    ![Snímek obrazovky, který přidá spolusprávce](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    Chcete-li odebrat oprávnění spolusprávce **klikněte pravým tlačítkem na** uživatele spolupracující správce a pak vyberte **odebrat spolusprávce**.

    ![Snímek obrazovky, který odebere spolusprávce](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Změna správce služby pro předplatné Azure

Pouze správce účtu může změnit správce služeb pro předplatné. Ve výchozím nastavení když se zaregistrujete, Správce služeb je stejný jako správce účtu. Pokud správce služby se změní na jiný uživatel, správce účtu ztratí přístup k webu Azure portal. Však správce účtu můžete vždy použít centra pro účty změnit správce služeb samy na sebe.

1. Ujistěte se, že váš scénář podporuje kontrolu [limity pro změnu Správci služeb](#limits).
1. Přihlaste se k [centra pro účty](https://account.windowsazure.com/subscriptions) jako správce účtu.
1. Vyberte předplatné.
1. Na pravé straně vyberte **upravit podrobnosti o předplatném**.

    ![Snímek obrazovky zobrazující tlačítko Upravit předplatné v centru účtů](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. V **Správce služeb** zadejte e-mailová adresa nového správce služby.

    ![Snímek obrazovky zobrazující pole, chcete-li změnit e-mailu Správce služby](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Omezení pro změnu Správci služeb

* Každé předplatné je přidružený k adresáři Azure AD. Pokud chcete najít adresář je přidružené předplatné, přejděte na [ **předplatná**](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade), pak vyberte předplatné zobrazíte adresáři.
* Pokud jste přihlášeni pomocí pracovní nebo školní účet, můžete přidat další účty ve vaší organizaci jako správce služeb. Například abby@contoso.com můžete přidat bob@contoso.com služby správce, ale nemůže přidat john@notcontoso.com Pokud john@notcontoso.com má přítomnost v adresáři contoso.com. Uživatelé přihlášení pomocí pracovní nebo školní účty mohou nadále přidat Account Microsoft uživatele jako správce služeb.

  | Metoda přihlašování | Přidání Account Microsoft uživatele jako správce služeb? | Přidat pracovní nebo školní účet v rámci stejné organizace jako správce služeb? | Přidat pracovní nebo školní účet v jiné organizaci jako správce služeb? |
  | --- | --- | --- | --- |
  |  Účet Microsoft |Ano |Ne |Ne |
  |  Pracovní nebo školní účet |Ano |Ano |Ne |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>Změnit správce účtu pro předplatné Azure

Účet správce je uživatel, který původně zaregistrovali předplatné Azure a je zodpovědný jako fakturace vlastníkem předplatného. Chcete-li změnit správce účtu spravujete předplatné [přenos vlastnictví předplatného Azure na jiný účet](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Nejste si jisti, který je správcem účtu?** Postupujte následovně:

1. Navštivte [ **předplatná** na webu Azure portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Vyberte předplatné, které chcete zkontrolovat a pak hledejte v části **nastavení**.
1. Vyberte **vlastnosti**. Správce účtu předplatného se zobrazí v **správce účtu** pole.  

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Další informace o řízení přístupu k prostředkům a služby Active Directory

* Další informace o RBAC najdete v tématu [co je řízení přístupu na základě rolí (RBAC)?](../role-based-access-control/overview.md)
* Další informace o všech rolí v Azure najdete v tématu [pochopit různé role v Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).
* Další informace o službě Azure Active Directory najdete v tématu [předplatné Azure propojeno se službou Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) a [přiřazení rolí správce v Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.

Pokud stále potřebujete pomoc, [obraťte se na podporu](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) pro rychlé vyřešení problému.
