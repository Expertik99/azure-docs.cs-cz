---
title: Zřizuje se špatná sada uživatelů k aplikaci Galerie Azure AD | Dokumentace Microsoftu
description: Zjistěte, jak zjistit, proč jinou sadu uživatelé se nezřizují k aplikaci než ty, které jste očekávali
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
ms.openlocfilehash: 8893c6a45ecbe0c697fe60483bbd2d9856d3b599
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366575"
---
# <a name="wrong-set-of-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Zřizuje se špatná sada uživatelů k aplikaci Galerie Azure AD

Které uživatelé jsou přiřazeni k aplikaci primárně doprovází kteří uživatelé a skupiny byly **přiřazené** do aplikace.

Zjistěte, jak zkontrolovat, kteří uživatelé a skupiny byly přiřazeny k aplikaci v Azure Active Directory pomocí následující prostředky.

## <a name="assign-a-user-directly-as-an-administrator"></a>Přiřadit uživatele přímo jako správce

Jeden nebo více uživatelů přiřadit přímo k aplikaci, postupujte takto:

1.  Otevřít [ **webu Azure portal** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřít **rozšíření Azure Active Directory** kliknutím **všechny služby** v horní části hlavní navigační nabídce vlevo.

3.  Zadejte **"Azure Active Directory**" do vyhledávacího pole filtrovat a vybrat **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace** levé navigační nabídce Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** zobrazíte seznam všech aplikací.

  * Pokud nevidíte aplikaci, kterou má zobrazit tady, použijte **filtr** ovládacího prvku v horní části **seznam všech aplikací** a nastavit **zobrazit** umožňuje **všechny Aplikace.**

6.  Vyberte aplikaci, kterou chcete přiřadit uživatele ze seznamu.

7.  Po načtení aplikace, klikněte na tlačítko **uživatelů a skupin** levé navigační nabídce aplikace.

8.  Otevřete **přidat přiřazení** podokně klikněte na tlačítko **přidat** tlačítko nahoře **uživatelů a skupin** seznamu.

9.  Klikněte na tlačítko **uživatelů a skupin** selektor z **přidat přiřazení** podokně.

10. Zadejte **celý název** nebo **e-mailová adresa** uživatele zájem o přiřazení do **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.

11. Najeďte myší **uživatele** v seznamu zobrazíte **zaškrtávací políčko**. Klikněte na zaškrtávací políčko vedle profilové fotky uživatele nebo logo, které chcete přidat uživatele **vybrané** seznamu.

12. **Volitelné:** Pokud byste chtěli **přidat více než jeden uživatel**, typ v jiném **celý název** nebo **e-mailová adresa** do **hledat podle názvu nebo e-mailová adresa** vyhledávací pole a klikněte na zaškrtávací políčko a přidáním tohoto uživatele do **vybrané** seznamu.

13. Po dokončení výběru uživatelů, klikněte na tlačítko **vyberte** tlačítko pro přidání do seznamu uživatelů a skupin pro přiřazení k aplikaci.

14. **Volitelné:** klikněte na tlačítko **vybrat roli** oblasti pro výběr **přidat přiřazení** podokně vyberte roli, kterou chcete přiřadit uživatelům, které jste vybrali.

15. Klikněte na tlačítko **přiřadit** tlačítko přiřadit aplikaci do vybraného uživatele.

Pokud zřizování je nakonfigurována a spuštěna pro aplikaci, by mělo proběhnout zřízení nových uživatelů, k aplikaci v přibližně 10 minut. Zkontrolujte, **protokoly auditu** podrobnosti.

## <a name="assign-a-group-directly-to-an-application-as-an-administrator"></a>Přiřazení skupiny přímo k aplikaci jako správce

Jeden nebo více skupin přiřadit přímo k aplikaci, postupujte takto:

1.  Otevřít [ **webu Azure portal** ](https://portal.azure.com/) a přihlaste se jako **globálního správce.**

2.  Otevřít **rozšíření Azure Active Directory** kliknutím **všechny služby** v horní části hlavní navigační nabídce vlevo.

3.  Zadejte **"Azure Active Directory**" do vyhledávacího pole filtrovat a vybrat **Azure Active Directory** položky.

4.  Klikněte na tlačítko **podnikové aplikace** levé navigační nabídce Azure Active Directory.

5.  Klikněte na tlačítko **všechny aplikace** zobrazíte seznam všech aplikací.

  * Pokud nevidíte aplikaci, kterou má zobrazit tady, použijte **filtr** ovládacího prvku v horní části **seznam všech aplikací** a nastavit **zobrazit** umožňuje **všechny Aplikace.**

6.  Vyberte aplikaci, kterou chcete přiřadit uživatele ze seznamu.

7.  Po načtení aplikace, klikněte na tlačítko **uživatelů a skupin** levé navigační nabídce aplikace.

8.  Otevřete **přidat přiřazení** podokně klikněte na tlačítko **přidat** tlačítko nahoře **uživatelů a skupin** seznamu.

9.  Klikněte na tlačítko **uživatelů a skupin** selektor z **přidat přiřazení** podokně.

10. Zadejte **název celé skupiny** zájem o přiřazení do skupiny **hledat podle jména nebo e-mailové adresy** vyhledávacího pole.

11. Najeďte myší **skupiny** v seznamu zobrazíte **zaškrtávací políčko**. Klikněte na zaškrtávací políčko vedle profilové fotky nebo logo, které chcete přidat uživatele do skupiny **vybrané** seznamu.

12. **Volitelné:** Pokud byste chtěli **přidat více než jednu skupinu**, typ v jiném **název celé skupiny** do **hledat podle jména nebo e-mailové adresy** vyhledávacího pole a Klikněte na zaškrtávací políčko k přidání do této skupiny **vybrané** seznamu.

13. Když jste hotovi s výběrem skupin, klikněte na tlačítko **vyberte** tlačítko pro přidání do seznamu uživatelů a skupin pro přiřazení k aplikaci.

14. **Volitelné:** klikněte na tlačítko **vybrat roli** oblasti pro výběr **přidat přiřazení** podokně vyberte roli, kterou chcete přiřadit ke zvoleným skupinám, které jste vybrali.

15. Klikněte na tlačítko **přiřadit** tlačítko a přiřazení aplikace k vybraným skupinám.

Pokud zřizování je nakonfigurována a spuštěna pro aplikace, noví uživatelé, obsažené ve skupině by mělo proběhnout zřízení k aplikaci v přibližně 10 minut. Zkontrolujte, **protokoly auditu** podrobnosti.

>[!IMPORTANT]
>Zřizování název skupiny a podrobnosti o skupině, kromě členů, pokud se podporuje u některých aplikací. Můžete povolit nebo zakázat tuto funkci povolením nebo zakázáním **mapování** pro objekty skupiny je znázorněno **zřizování** kartu. 
>
>

Pokud je zapnutá zřizování skupin, nezapomeňte zkontrolovat mapování atributů k zajištění, že se že příslušné pole se používá pro "Odpovídající ID". To může být zobrazovaný název ani e-mailu, jak skupině a jejích členů nesmí být zřízené, pokud je odpovídající vlastnost prázdná, nebo ne naplnit pro skupinu ve službě Azure AD.

## <a name="next-steps"></a>Další postup
[Automatizace zřizování uživatelů a jeho rušení pro aplikace SaaS ve službě Azure Active Directory](active-directory-saas-app-provisioning.md)
