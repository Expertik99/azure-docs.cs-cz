---
title: 'Kurz: Integrace Azure Active Directory se službou Dropbox for Business | Dokumentace Microsoftu'
description: Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Dropbox for Business.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/20/2018
ms.author: jeedes
ms.openlocfilehash: eadf6724891d348c2ea3654bcf19ef0d74078049
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/20/2018
ms.locfileid: "42060557"
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Kurz: Integrace Azure Active Directory se službou Dropbox for Business

V tomto kurzu se dozvíte, jak integrovat Dropboxu pro firmy s Azure Active Directory (Azure AD).

Dropbox for Business integraci se službou Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup na Dropbox for Business.
- Můžete povolit uživatelům, aby automaticky získat přihlášeného na Dropbox for Business (Single Sign-On) s jejich účty Azure AD.
- Můžete spravovat své účty na jediném místě – na webu Azure portal.

Pokud chcete zjistit další podrobnosti o integraci aplikací SaaS v Azure AD, přečtěte si téma [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Dropbox for Business, potřebujete následující položky:

- Předplatné Azure AD
- Dropbox pro obchodní jednotného přihlašování povolená předplatného

> [!NOTE]
> Pokud chcete vyzkoušet kroky v tomto kurzu, nedoporučujeme použití produkční prostředí.

Pokud chcete vyzkoušet kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte produkčním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verzi Azure AD, můžete si [získat měsíční zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu je otestovat Azure AD jednotné přihlašování v testovacím prostředí.
Scénář popsaný v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Dropbox for Business přidat z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-dropbox-for-business-from-the-gallery"></a>Dropbox for Business přidat z Galerie

Pokud chcete nakonfigurovat integraci Dropbox for Business do služby Azure AD, budete muset Dropbox for Business přidat z Galerie na váš seznam spravovaných aplikací SaaS.

**Dropbox for Business přidat z galerie, postupujte následovně:**

1. V **[webu Azure portal](https://portal.azure.com)**, v levém navigačním panelu klikněte na **Azure Active Directory** ikonu.

    ![Tlačítko Azure Active Directory][1]

2. Přejděte do **podnikové aplikace**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]

3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko v horní části dialogového okna.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **Dropbox for Business**vyberte **Dropbox for Business** z panelu výsledků klikněte **přidat** tlačítko pro přidání aplikace.

    ![Dropbox for Business v seznamu výsledků](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování služby Azure AD jednotného přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s Dropbox for Business podle testovacího uživatele nazývá "Britta Simon".

Pro jednotné přihlašování pro práci služba Azure AD potřebuje vědět, co uživatel protějšky v Dropboxu pro firmy je pro uživatele ve službě Azure AD. Jinými slovy musí navázat vztah odkazu mezi uživatele služby Azure AD a související uživatelské v Dropboxu pro firmy.

V Dropboxu pro firmy přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** a tím vytvoří vztah odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Dropbox for Business, které potřebujete k dokončení následujících stavebních bloků:

1. **[Konfigurovat Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  – Pokud chcete, aby uživatelé mohli tuto funkci používat.
2. **[Vytvořit testovacího uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření Dropboxu pro obchodní testovacího uživatele](#create-a-dropbox-for-business-test-user)**  – Pokud chcete mít protějšek Britta Simon v Dropboxu pro podniky, které souvisí s Azure AD reprezentace uživatele.
4. **[Přiřadit uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotného přihlašování.
5. **[Otestovat jednotné přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, jestli funguje v konfiguraci.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurace služby Azure AD jednotného přihlašování

V této části Povolení služby Azure AD jednotného přihlašování na portálu Azure portal a konfigurace jednotného přihlašování do vaší schránky pro obchodní aplikace.

**Ke konfiguraci Azure AD jednotné přihlašování s Dropbox for Business, proveďte následující kroky:**

1. Na webu Azure Portal na **Dropbox for Business** integrace stránka aplikace, klikněte na tlačítko **jednotného přihlašování**.

    ![Nakonfigurovat jednotné přihlašování – odkaz][4]

2. Na **jednotného přihlašování** dialogového okna, vyberte **režimu** jako **přihlašování na základě SAML** povolit jednotné přihlašování.
 
    ![Jednotné přihlašování – dialogové okno](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_samlbase.png)

3. Na **Dropboxu obchodní domény a adresy URL** části, proveďte následující kroky:

    ![Dropbox obchodní domény a adresy URL jednotné přihlašování – informace](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_url1.png)

    a. V **přihlašovací adresa URL** textového pole zadejte adresu URL pomocí následujícímu vzoru: `https://www.dropbox.com/sso/<id>`

    b. V **identifikátor** textového pole zadejte hodnotu: `Dropbox`

    > [!NOTE]
    > Předchozí hodnota přihlašovací adresa URL není skutečné hodnoty. Hodnota bude aktualizován skutečné přihlašovací adresa URL, který je vysvětlen později v tomto kurzu.

4. Na **podpisový certifikát SAML** klikněte na tlačítko **certifikát (Base64)** a uložte soubor certifikátu v počítači.

    ![Odkaz ke stažení certifikátu](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurovat jednotné přihlašování uložit tlačítko](./media/dropboxforbusiness-tutorial/tutorial_general_400.png)

6. Na **Dropboxu pro konfiguraci obchodního** klikněte na tlačítko **konfigurace Dropbox for Business** otevřete **nakonfigurovat přihlašování** okna. Kopírovat **SAML jednotné přihlašování – adresa URL služby** z **Stručná referenční příručka oddílu.**

    ![Konfigurace obchodní z Dropboxu](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_configure.png) 

7. Ke konfiguraci jednotného přihlašování na **Dropbox for Business** straně, přejděte na váš Dropbox pro obchodní tenanta a přihlaste do vaší schránky pro obchodní tenanta.

    ![Konfigurace jednotného přihlašování](./media/dropboxforbusiness-tutorial/ic769509.png "nakonfigurovat jednotné přihlašování")

8. Klikněte na **ikonu uživatele** a vyberte **nastavení** kartu.

    ![Konfigurace jednotného přihlašování](./media/dropboxforbusiness-tutorial/configure1.png "nakonfigurovat jednotné přihlašování")

9. V navigačním podokně na levé straně klikněte na tlačítko **konzoly pro správu**.

    ![Konfigurace jednotného přihlašování](./media/dropboxforbusiness-tutorial/configure2.png "nakonfigurovat jednotné přihlašování")

10. Na **konzoly pro správu**, klikněte na tlačítko **nastavení** v levém navigačním podokně.

    ![Konfigurace jednotného přihlašování](./media/dropboxforbusiness-tutorial/configure3.png "nakonfigurovat jednotné přihlašování")

11. Vyberte **jednotného přihlašování** v části **ověřování** oddílu.

    ![Konfigurace jednotného přihlašování](./media/dropboxforbusiness-tutorial/configure4.png "nakonfigurovat jednotné přihlašování")

12. V **jednotného přihlašování** části, proveďte následující kroky:  

    ![Konfigurace jednotného přihlašování](./media/dropboxforbusiness-tutorial/configure5.png "nakonfigurovat jednotné přihlašování")

    a. Vyberte **vyžaduje** jako možnost z rozevíracího seznamu **jednotného přihlašování**.

    b. Klikněte na **přidat přihlašovací adresu URL** a **přihlašovací adresa URL zprostředkovatele Identity** vložit do textového pole **SAML jednotné přihlašování – adresa URL služby** hodnotu, která jste zkopírovali z portálu Azure portal a pak vyberte **provádí**.

    ![Konfigurace jednotného přihlašování](./media/dropboxforbusiness-tutorial/configure6.png "nakonfigurovat jednotné přihlašování")

    c. Klikněte na tlačítko **nahrát certifikát**a potom přejděte k vaší **soubor certifikátu v kódování Base64** který jste si stáhli z portálu Azure portal.

    d. Klikněte na **Kopírovat odkaz** a vložte zkopírovaný hodnotu do **přihlašovací adresa URL** textové pole z **Dropboxu obchodní domény a adresy URL** části na webu Azure portal.

    e. Klikněte na **Uložit**.

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovacího uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na webu Azure Portal volá Britta Simon.

   ![Vytvořit testovacího uživatele Azure AD][100]

**Chcete-li vytvořit testovacího uživatele ve službě Azure AD, postupujte následovně:**

1. Na webu Azure Portal, v levém podokně klikněte na tlačítko **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/dropboxforbusiness-tutorial/create_aaduser_01.png)

1. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na tlačítko **všichni uživatelé**.

    !["Uživatele a skupiny" a "Všechny uživatele" odkazy](./media/dropboxforbusiness-tutorial/create_aaduser_02.png)

1. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/dropboxforbusiness-tutorial/create_aaduser_03.png)

1. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/dropboxforbusiness-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit heslo** zaškrtněte políčko a zapište si hodnotu, která se zobrazí **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-dropbox-for-business-test-user"></a>Vytvoření Dropboxu pro obchodní testovacího uživatele

V této části se vytvoří uživateli Britta Simon v Dropboxu pro firmy. Dropbox for Business podporuje just-in-time zřizování, který je ve výchozím nastavení povolené.

Neexistuje žádná položka akce pro vás v této části. Pokud uživatel ještě neexistuje v Dropboxu pro firmy, je vytvořen nový při pokusu o přístup k Dropboxu pro firmy.

>[!Note]
>Pokud je potřeba ručně vytvořit uživatele, obraťte se na [tým podpory obchodních klienta z Dropboxu](https://www.dropbox.com/business/contact) 

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit uživatele Azure AD

V této části je povolit Britta Simon používat jednotné přihlašování Azure díky udělení přístupu na Dropbox for Business.

![Přiřazení role uživatele][200] 

**Pokud chcete přiřadit Britta Simon na Dropbox for Business, postupujte následovně:**

1. Na webu Azure Portal, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

1. V seznamu aplikací vyberte **Dropbox for Business**.

    ![Dropbox pro obchodní odkaz v seznamu aplikací](./media/dropboxforbusiness-tutorial/tutorial_dropboxforbusiness_app.png)  

1. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatele a skupiny"][202]

1. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogového okna.

    ![Podokno Přidat přiřazení][203]

1. Na **uživatelů a skupin** dialogového okna, vyberte **Britta Simon** v seznamu uživatelů.

1. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogového okna.

1. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogového okna.
    
### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části Testování služby Azure AD jednotné přihlašování – konfigurace pomocí přístupového panelu.

Po kliknutí na Dropboxu pro obchodní dlaždici na přístupovém panelu, měli byste obdržet přihlašovací stránku vašeho dropboxu podnikových aplikací.
 

## <a name="additional-resources"></a>Další zdroje informací:

* [Seznam kurzů o integraci aplikací SaaS pomocí Azure Active Directory](tutorial-list.md)
* [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/dropboxforbusiness-tutorial/tutorial_general_01.png
[2]: ./media/dropboxforbusiness-tutorial/tutorial_general_02.png
[3]: ./media/dropboxforbusiness-tutorial/tutorial_general_03.png
[4]: ./media/dropboxforbusiness-tutorial/tutorial_general_04.png

[100]: ./media/dropboxforbusiness-tutorial/tutorial_general_100.png

[200]: ./media/dropboxforbusiness-tutorial/tutorial_general_200.png
[201]: ./media/dropboxforbusiness-tutorial/tutorial_general_201.png
[202]: ./media/dropboxforbusiness-tutorial/tutorial_general_202.png
[203]: ./media/dropboxforbusiness-tutorial/tutorial_general_203.png

