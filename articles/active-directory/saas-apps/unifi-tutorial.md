---
title: 'Kurz: Integrace Azure Active Directory se službou UNIFI | Dokumentace Microsoftu'
description: Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a UNIFI.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e1f49ee4-d2d4-4a82-9baf-0587ca1f20f6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 35b1b9492b7bcd09c79cb5bd2509a6cfea205ae9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/02/2018
ms.locfileid: "39445462"
---
# <a name="tutorial-azure-active-directory-integration-with-unifi"></a>Kurz: Integrace Azure Active Directory se službou UNIFI

V tomto kurzu se dozvíte, jak integrovat UNIFI s Azure Active Directory (Azure AD).

UNIFI integraci se službou Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k UNIFI
- Můžete povolit uživatelům, aby automaticky získat přihlášení k UNIFI (Single Sign-On) s jejich účty Azure AD
- Můžete spravovat své účty na jediném místě – na webu Azure portal

Pokud chcete zjistit další podrobnosti o integraci aplikací SaaS v Azure AD, přečtěte si téma [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s UNIFI, potřebujete následující položky:

- S předplatným služby Azure AD
- UNIFI jednotného přihlašování povolená předplatného

> [!NOTE]
> Pokud chcete vyzkoušet kroky v tomto kurzu, nedoporučujeme použití produkční prostředí.

Pokud chcete vyzkoušet kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte produkčním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verzi Azure AD, můžete získat měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu je otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénář popsaný v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání UNIFI z Galerie
1. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-unifi-from-the-gallery"></a>Přidání UNIFI z Galerie
Konfigurace integrace UNIFI do služby Azure AD, budete muset přidat UNIFI z Galerie na váš seznam spravovaných aplikací SaaS.

**Chcete-li přidat UNIFI z galerie, postupujte následovně:**

1. V  **[webu Azure portal](https://portal.azure.com)**, v levém navigačním panelu klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

1. Přejděte do **podnikové aplikace**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
1. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko v horní části dialogového okna.

    ![Aplikace][3]

1. Do vyhledávacího pole zadejte **UNIFI**.

    ![Vytváří se testovací uživatele služby Azure AD](./media/unifi-tutorial/tutorial_unifi_search.png)

1. Na panelu výsledků vyberte **UNIFI**a potom klikněte na tlačítko **přidat** tlačítko pro přidání aplikace.

    ![Vytváří se testovací uživatele služby Azure AD](./media/unifi-tutorial/tutorial_unifi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části Konfigurace a testování Azure AD jednotné přihlašování pomocí UNIFI podle testovacího uživatele nazývá "Britta Simon".

Pro jednotné přihlašování pro práci služba Azure AD potřebuje vědět, co uživatel protějšky v UNIFI je pro uživatele ve službě Azure AD. Jinými slovy vztah odkazu mezi uživatele služby Azure AD a související uživatelské v UNIFI potřeba navázat.

V UNIFI, přiřaďte hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** a tím vytvoří vztah odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s UNIFI, které potřebujete k dokončení následujících stavebních bloků:

1. **[Konfigurace Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  – Pokud chcete, aby uživatelé mohli tuto funkci používat.
1. **[Vytváří se testovací uživatele služby Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
1. **[Vytváření UNIFI testovací uživatele](#creating-a-unifi-test-user)**  – Pokud chcete mít protějšek Britta Simon UNIFI, který je propojený s Azure AD reprezentace uživatele.
1. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotného přihlašování.
1. **[Testování Single Sign-On](#testing-single-sign-on)**  – Pokud chcete ověřit, jestli funguje v konfiguraci.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace služby Azure AD jednotného přihlašování

V této části Povolení služby Azure AD jednotného přihlašování na portálu Azure portal a konfigurace jednotného přihlašování v aplikaci UNIFI.

**Ke konfiguraci Azure AD jednotné přihlašování s UNIFI, proveďte následující kroky:**

1. Na webu Azure Portal na **UNIFI** integrace stránka aplikace, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace jednotného přihlašování][4]

1. Na **jednotného přihlašování** dialogového okna, vyberte **režimu** jako **přihlašování na základě SAML** povolit jednotné přihlašování.
 
    ![Konfigurace jednotného přihlašování](./media/unifi-tutorial/tutorial_unifi_samlbase.png)

1. Na **UNIFI domény a adresy URL** části, pokud chcete nakonfigurovat aplikace v **IDP** iniciované režimu:

    ![Konfigurace jednotného přihlašování](./media/unifi-tutorial/tutorial_unifi_url1.png)

    V **identifikátor** textového pole zadejte hodnotu: `INVIEWlabs` 

1. Zkontrolujte **zobrazit pokročilé nastavení URL**, pokud chcete nakonfigurovat aplikace v **SP** iniciované režimu:

    ![Konfigurace jednotného přihlašování](./media/unifi-tutorial/tutorial_unifi_url2.png)

    V **přihlašovací adresa URL** textového pole zadejte adresu URL: `https://app.discoverunifi.com/login`

1. Na **podpisový certifikát SAML** klikněte na tlačítko **Certificate(Base64)** a uložte soubor certifikátu v počítači.

    ![Konfigurace jednotného přihlašování](./media/unifi-tutorial/tutorial_unifi_certificate.png) 

1. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurace jednotného přihlašování](./media/unifi-tutorial/tutorial_general_400.png)
    
1. Na **UNIFI konfigurace** klikněte na tlačítko **konfigurace UNIFI** otevřete **nakonfigurovat přihlašování** okna. Kopírovat **SAML jednotné přihlašování – adresa URL služby** z **Stručná referenční příručka oddílu.**

    ![Konfigurace jednotného přihlašování](./media/unifi-tutorial/tutorial_unifi_configure.png)

1. V okně jiné webové prohlížeče, přihlaste se k vaší **UNIFI** společnosti serveru jako správce.

1. Klikněte na **uživatelé**.

    ![Konfigurace jednotného přihlašování](./media/unifi-tutorial/app1.png) 

1. Klikněte na **přidat nového zprostředkovatele Identity**.

    ![Konfigurace jednotného přihlašování](./media/unifi-tutorial/app2.png)

1. V **přidat zprostředkovatele Identity** části, proveďte následující kroky:    

    ![Konfigurace jednotného přihlašování](./media/unifi-tutorial/app3.png) 

    a. V **název zprostředkovatele** textového pole zadejte název zprostředkovatele Identity...

    b. V **adresa URL zprostředkovatele** vložení textového pole **SAML jednotné přihlašování – adresa URL služby** hodnotu, kterou jste zkopírovali z portálu Azure portal.

    c. Otevřete certifikát, že jste si stáhli z portálu Azure v programu Poznámkový blok, odeberte **---BEGIN CERTIFICATE---** a **---END CERTIFICATE---** značky a pak vložte zbývající obsah **Certifikát** textového pole.

    d. Vyberte **je výchozí zprostředkovatel** zaškrtávací políčko.

> [!TIP]
> Teď si můžete přečíst stručné verzi těchto pokynů uvnitř [webu Azure portal](https://portal.azure.com), zatímco jsou nastavení aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace** části, stačí kliknout **Single Sign-On** kartu a přístup k vložené dokumentaci prostřednictvím  **Konfigurace** oblast v dolní části. Další informace o funkci vložená dokumentace: [dokumentace ke službě Azure AD embedded]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváří se testovací uživatele služby Azure AD
Cílem této části je vytvoření zkušebního uživatele na webu Azure Portal volá Britta Simon.

![Vytvoření uživatele Azure AD][100]

**Chcete-li vytvořit testovacího uživatele ve službě Azure AD, postupujte následovně:**

1. V **webu Azure portal**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváří se testovací uživatele služby Azure AD](./media/unifi-tutorial/create_aaduser_01.png) 

1. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváří se testovací uživatele služby Azure AD](./media/unifi-tutorial/create_aaduser_02.png) 

1. Chcete-li otevřít **uživatele** dialogového okna, klikněte na tlačítko **přidat** horní části dialogového okna.
 
    ![Vytváří se testovací uživatele služby Azure AD](./media/unifi-tutorial/create_aaduser_03.png) 

1. Na **uživatele** dialogového okna stránky, proveďte následující kroky:
 
    ![Vytváří se testovací uživatele služby Azure AD](./media/unifi-tutorial/create_aaduser_04.png) 

    a. V **název** textové pole, typ **BrittaSimon**.

    b. V **uživatelské jméno** textové pole, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit heslo** a zapište si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-a-unifi-test-user"></a>Vytvoření zkušebního uživatele UNIFI

V této části vytvořte uživatele Britta Simon. **UNIFI** podporuje automatické zřizování uživatelů, nejsou požadované žádné ruční kroky. Uživatelé jsou automaticky vytvořeny po úspěšném ověření ze služby Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části je povolit Britta Simon k udělení přístupu k UNIFI použití Azure jednotného přihlašování.

![Přiřadit uživatele][200] 

**Přiřadit UNIFI Britta Simon, proveďte následující kroky:**

1. Na webu Azure Portal, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

1. V seznamu aplikací vyberte **UNIFI**.

    ![Konfigurace jednotného přihlašování](./media/unifi-tutorial/tutorial_unifi_app.png) 

1. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

1. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogového okna.

    ![Přiřadit uživatele][203]

1. Na **uživatelů a skupin** dialogového okna, vyberte **Britta Simon** v seznamu uživatelů.

1. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogového okna.

1. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogového okna.
    
### <a name="testing-single-sign-on"></a>Testování jednotného přihlašování

V této části Testování služby Azure AD jednotné přihlašování – konfigurace pomocí přístupového panelu.

Po kliknutí na dlaždici UNIFI na přístupovém panelu, vám by měl získat automaticky přihlášení k aplikaci UNIFI.
Další informace o přístupovém panelu, naleznete v tématu [Úvod k přístupovému panelu](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje informací:

* [Seznam kurzů o integraci aplikací SaaS pomocí Azure Active Directory](tutorial-list.md)
* [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/unifi-tutorial/tutorial_general_01.png
[2]: ./media/unifi-tutorial/tutorial_general_02.png
[3]: ./media/unifi-tutorial/tutorial_general_03.png
[4]: ./media/unifi-tutorial/tutorial_general_04.png

[100]: ./media/unifi-tutorial/tutorial_general_100.png

[200]: ./media/unifi-tutorial/tutorial_general_200.png
[201]: ./media/unifi-tutorial/tutorial_general_201.png
[202]: ./media/unifi-tutorial/tutorial_general_202.png
[203]: ./media/unifi-tutorial/tutorial_general_203.png

