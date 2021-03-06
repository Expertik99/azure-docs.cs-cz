---
title: 'Kurz: Integrace Azure Active Directory se službou SumTotalCentral | Dokumentace Microsoftu'
description: Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SumTotalCentral.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 93ad629a-f516-4cac-bfe2-a77257e3a797
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: jeedes
ms.openlocfilehash: 1c604eebb2c1c85de717217063333190ffa865f4
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426512"
---
# <a name="tutorial-azure-active-directory-integration-with-sumtotalcentral"></a>Kurz: Integrace Azure Active Directory se službou SumTotalCentral

V tomto kurzu se dozvíte, jak integrovat SumTotalCentral s Azure Active Directory (Azure AD).

SumTotalCentral integraci se službou Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k SumTotalCentral.
- Můžete povolit uživatelům, aby automaticky získat přihlášení k SumTotalCentral (Single Sign-On) s jejich účty Azure AD.
- Můžete spravovat své účty na jediném místě – na webu Azure portal.

Pokud chcete zjistit další podrobnosti o integraci aplikací SaaS v Azure AD, přečtěte si téma [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s SumTotalCentral, potřebujete následující položky:

- S předplatným služby Azure AD
- SumTotalCentral jednotného přihlašování povolená předplatného

> [!NOTE]
> Pokud chcete vyzkoušet kroky v tomto kurzu, nedoporučujeme použití produkční prostředí.

Pokud chcete vyzkoušet kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte produkčním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verzi Azure AD, můžete si [získat měsíční zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu je otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénář popsaný v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání SumTotalCentral z Galerie
1. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-sumtotalcentral-from-the-gallery"></a>Přidání SumTotalCentral z Galerie
Konfigurace integrace SumTotalCentral do služby Azure AD, budete muset přidat SumTotalCentral z Galerie na váš seznam spravovaných aplikací SaaS.

**Chcete-li přidat SumTotalCentral z galerie, postupujte následovně:**

1. V  **[webu Azure portal](https://portal.azure.com)**, v levém navigačním panelu klikněte na **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

1. Přejděte do **podnikové aplikace**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
1. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko v horní části dialogového okna.

    ![Tlačítko nové aplikace][3]

1. Do vyhledávacího pole zadejte **SumTotalCentral**vyberte **SumTotalCentral** z panelu výsledků klikněte **přidat** tlačítko pro přidání aplikace.

    ![SumTotalCentral v seznamu výsledků](./media/sumtotalcentral-tutorial/tutorial_sumtotalcentral_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování služby Azure AD jednotného přihlašování

V této části Konfigurace a testování Azure AD jednotné přihlašování pomocí SumTotalCentral podle testovacího uživatele nazývá "Britta Simon".

Pro jednotné přihlašování pro práci služba Azure AD potřebuje vědět, co uživatel protějšky v SumTotalCentral je pro uživatele ve službě Azure AD. Jinými slovy vztah odkazu mezi uživatele služby Azure AD a související uživatelské v SumTotalCentral potřeba navázat.

V SumTotalCentral, přiřaďte hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** a tím vytvoří vztah odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s SumTotalCentral, které potřebujete k dokončení následujících stavebních bloků:

1. **[Konfigurovat Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  – Pokud chcete, aby uživatelé mohli tuto funkci používat.
1. **[Vytvořit testovacího uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
1. **[Vytvoření zkušebního uživatele SumTotalCentral](#create-a-sumtotalcentral-test-user)**  – Pokud chcete mít protějšek Britta Simon SumTotalCentral, který je propojený s Azure AD reprezentace uživatele.
1. **[Přiřadit uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotného přihlašování.
1. **[Otestovat jednotné přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, jestli funguje v konfiguraci.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurace služby Azure AD jednotného přihlašování

V této části Povolení služby Azure AD jednotného přihlašování na portálu Azure portal a konfigurace jednotného přihlašování v aplikaci SumTotalCentral.

**Ke konfiguraci Azure AD jednotné přihlašování s SumTotalCentral, proveďte následující kroky:**

1. Na webu Azure Portal na **SumTotalCentral** integrace stránka aplikace, klikněte na tlačítko **jednotného přihlašování**.

    ![Nakonfigurovat jednotné přihlašování – odkaz][4]

1. Na **jednotného přihlašování** dialogového okna, vyberte **režimu** jako **přihlašování na základě SAML** povolit jednotné přihlašování.
 
    ![Jednotné přihlašování – dialogové okno](./media/sumtotalcentral-tutorial/tutorial_sumtotalcentral_samlbase.png)

1. Na **SumTotalCentral domény a adresy URL** části, proveďte následující kroky:

    ![SumTotalCentral domény a adresy URL jednotného přihlašování – informace](./media/sumtotalcentral-tutorial/tutorial_sumtotalcentral_url.png)

    a. V **přihlašovací adresa URL** textového pole zadejte adresu URL pomocí následujícímu vzoru: `https://<subdomain>.sumtotalsystems.com/sites/default`

    b. V **identifikátor** textového pole zadejte hodnotu: `SumTotalFederationGateway`

    > [!NOTE] 
    > Hodnota přihlašovací adresa URL není skutečný. Aktualizujte příslušnou hodnotu skutečné přihlašovací adresa URL. Kontakt [tým podpory SumTotalCentral klienta](http://www.sumtotalsystems.com/support/) má být získána hodnota. 
 
1. Na **podpisový certifikát SAML** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor metadat ve vašem počítači.

    ![Odkaz ke stažení certifikátu](./media/sumtotalcentral-tutorial/tutorial_sumtotalcentral_certificate.png) 

1. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurovat jednotné přihlašování uložit tlačítko](./media/sumtotalcentral-tutorial/tutorial_general_400.png)

1. Ke konfiguraci jednotného přihlašování na **SumTotalCentral** straně, je nutné odeslat na stažený **soubor XML s metadaty** k [tým podpory SumTotalCentral](http://www.sumtotalsystems.com/support/). Nastavují tohoto nastavení můžete mít správně nastavené na obou stranách připojení SAML SSO.

> [!TIP]
> Teď si můžete přečíst stručné verzi těchto pokynů uvnitř [webu Azure portal](https://portal.azure.com), zatímco jsou nastavení aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace** části, stačí kliknout **Single Sign-On** kartu a přístup k vložené dokumentaci prostřednictvím  **Konfigurace** oblast v dolní části. Další informace o funkci vložená dokumentace: [dokumentace ke službě Azure AD embedded]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovacího uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na webu Azure Portal volá Britta Simon.

   ![Vytvořit testovacího uživatele Azure AD][100]

**Chcete-li vytvořit testovacího uživatele ve službě Azure AD, postupujte následovně:**

1. Na webu Azure Portal, v levém podokně klikněte na tlačítko **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/sumtotalcentral-tutorial/create_aaduser_01.png)

1. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na tlačítko **všichni uživatelé**.

    !["Uživatele a skupiny" a "Všechny uživatele" odkazy](./media/sumtotalcentral-tutorial/create_aaduser_02.png)

1. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/sumtotalcentral-tutorial/create_aaduser_03.png)

1. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/sumtotalcentral-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit heslo** zaškrtněte políčko a zapište si hodnotu, která se zobrazí **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-sumtotalcentral-test-user"></a>Vytvoření zkušebního uživatele SumTotalCentral

V této části vytvoříte uživatele v SumTotalCentral jako Britta Simon. Práce s [tým podpory SumTotalCentral](http://www.sumtotalsystems.com/support/) přidat uživatele na platformě SumTotalCentral. Uživatelé musí vytvořit a aktivovat, než použití jednotného přihlašování.  

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit uživatele Azure AD

V této části je povolit Britta Simon k udělení přístupu k SumTotalCentral použití Azure jednotného přihlašování.

![Přiřazení role uživatele][200] 

**Přiřadit SumTotalCentral Britta Simon, proveďte následující kroky:**

1. Na webu Azure Portal, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

1. V seznamu aplikací vyberte **SumTotalCentral**.

    ![Odkaz SumTotalCentral v seznamu aplikací](./media/sumtotalcentral-tutorial/tutorial_sumtotalcentral_app.png)  

1. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatele a skupiny"][202]

1. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogového okna.

    ![Podokno Přidat přiřazení][203]

1. Na **uživatelů a skupin** dialogového okna, vyberte **Britta Simon** v seznamu uživatelů.

1. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogového okna.

1. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogového okna.
    
### <a name="test-single-sign-on"></a>Otestovat jednotné přihlašování

V této části Testování služby Azure AD jednotné přihlašování – konfigurace pomocí přístupového panelu.

Po kliknutí na dlaždici SumTotalCentral na přístupovém panelu, vám by měl získat automaticky přihlášení k aplikaci SumTotalCentral.
Další informace o přístupovém panelu, naleznete v tématu [Úvod k přístupovému panelu](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje informací:

* [Seznam kurzů o integraci aplikací SaaS pomocí Azure Active Directory](tutorial-list.md)
* [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sumtotalcentral-tutorial/tutorial_general_01.png
[2]: ./media/sumtotalcentral-tutorial/tutorial_general_02.png
[3]: ./media/sumtotalcentral-tutorial/tutorial_general_03.png
[4]: ./media/sumtotalcentral-tutorial/tutorial_general_04.png

[100]: ./media/sumtotalcentral-tutorial/tutorial_general_100.png

[200]: ./media/sumtotalcentral-tutorial/tutorial_general_200.png
[201]: ./media/sumtotalcentral-tutorial/tutorial_general_201.png
[202]: ./media/sumtotalcentral-tutorial/tutorial_general_202.png
[203]: ./media/sumtotalcentral-tutorial/tutorial_general_203.png

