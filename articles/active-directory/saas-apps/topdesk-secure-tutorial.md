---
title: 'Kurz: Integrace Azure Active Directory s TOPdesk – zabezpečení | Dokumentace Microsoftu'
description: Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a TOPdesk – zabezpečené.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8e06ee33-18f9-4c05-9168-e6b162079d88
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2018
ms.author: jeedes
ms.openlocfilehash: 8529dfda5ee4a7fc3360f91163b7f5f5bbf6c6ff
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/09/2018
ms.locfileid: "42060734"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Kurz: Integrace Azure Active Directory s TOPdesk – zabezpečení

V tomto kurzu se dozvíte, jak integrovat TOPdesk – zabezpečení pomocí služby Azure Active Directory (Azure AD).

Integrace TOPdesk – zabezpečené pomocí Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k TOPdesk – zabezpečené.
- Můžete povolit uživatelům, aby automaticky získat přihlášení k TOPdesk – zabezpečené (Single Sign-On) s jejich účty Azure AD.
- Můžete spravovat své účty na jediném místě – na webu Azure portal.

Pokud chcete zjistit další podrobnosti o integraci aplikací SaaS v Azure AD, přečtěte si téma [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s TOPdesk – zabezpečené, potřebujete následující položky:

- Předplatné Azure AD
- A TOPdesk – zabezpečené jednotné přihlašování povolená předplatného

> [!NOTE]
> Pokud chcete vyzkoušet kroky v tomto kurzu, nedoporučujeme použití produkční prostředí.

Pokud chcete vyzkoušet kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte produkčním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verzi Azure AD, můžete si [získat měsíční zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře

V tomto kurzu je otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénář popsaný v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání TOPdesk – zabezpečení z Galerie
1. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-topdesk---secure-from-the-gallery"></a>Přidání TOPdesk – zabezpečení z Galerie

Konfigurace integrace TOPdesk – zabezpečení do služby Azure AD, budete muset přidat TOPdesk – zabezpečení z Galerie do seznamu spravovaných SaaS aplikací.

**Chcete-li přidat TOPdesk – zabezpečení z galerie, proveďte následující kroky:**

1. V **[webu Azure portal](https://portal.azure.com)**, v levém navigačním panelu klikněte na **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte do **podnikové aplikace**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]

3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko v horní části dialogového okna.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **TOPdesk – zabezpečené**vyberte **TOPdesk – zabezpečené** z panelu výsledků klikněte **přidat** tlačítko pro přidání aplikace.

    ![TOPdesk – zabezpečení v seznamu výsledků](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování služby Azure AD jednotného přihlašování

V této části můžete nakonfigurovat a otestovat Azure AD jednotné přihlašování s TOPdesk – zabezpečené podle testovacího uživatele nazývá "Britta Simon".

Pro jednotné přihlašování pro práci služba Azure AD potřebuje vědět, co uživatel protějšky v TOPdesk – zabezpečení je pro uživatele ve službě Azure AD. Jinými slovy musí navázat vztah odkazu mezi uživatele služby Azure AD a související uživatelské v TOPdesk – zabezpečené.

V TOPdesk – zabezpečení, přiřadit hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** a tím vytvoří vztah odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s TOPdesk – zabezpečení, které potřebujete k dokončení následujících stavebních bloků:

1. **[Konfigurovat Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  – Pokud chcete, aby uživatelé mohli tuto funkci používat.
2. **[Vytvořit testovacího uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření TOPdesk – zabezpečené testovacího uživatele](#create-a-topdesk---secure-test-user)**  – Pokud chcete mít protějšek Britta Simon v TOPdesk – zabezpečení, který je propojený s Azure AD reprezentace uživatele.
4. **[Přiřadit uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotného přihlašování.
5. **[Otestovat jednotné přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, jestli funguje v konfiguraci.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurace služby Azure AD jednotného přihlašování

V této části Povolení služby Azure AD jednotného přihlašování na portálu Azure portal a konfigurace jednotného přihlašování ve vašich TOPdesk - zabezpečenou webovou aplikaci.

**Ke konfiguraci Azure AD jednotné přihlašování s TOPdesk – zabezpečení, proveďte následující kroky:**

1. Na webu Azure Portal na **TOPdesk – zabezpečené** integrace stránka aplikace, klikněte na tlačítko **jednotného přihlašování**.

    ![Nakonfigurovat jednotné přihlašování – odkaz][4]

2. Na **jednotného přihlašování** dialogového okna, vyberte **režimu** jako **přihlašování na základě SAML** povolit jednotné přihlašování.

    ![Jednotné přihlašování – dialogové okno](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_samlbase.png)

3. Na **TOPdesk – zabezpečení domény a adresy URL** části, proveďte následující kroky:

    ![TOPdesk – zabezpečení domény a adresy URL jednotného přihlašování – informace](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_url.png)

    a. V **přihlašovací adresa URL** textového pole zadejte adresu URL pomocí následujícímu vzoru: `https://<companyname>.topdesk.net`

    b. V **identifikátor** textového pole zadejte adresu URL pomocí následujícímu vzoru: `https://<companyname>.topdesk.net/tas/secure/login/verify`

    c. V **adresy URL odpovědi** textového pole zadejte adresu URL pomocí následujícímu vzoru: `https://<companyname>.topdesk.net/tas/public/login/saml`

    > [!NOTE]
    > Tyto hodnoty nejsou skutečný. Tyto hodnoty aktualizujte s skutečné přihlašovací adresu URL a identifikátorem. Adresa URL odpovědi je vysvětlen později v kurzu. Kontakt [TOPdesk - tým podpory zabezpečení klienta](http://www.topdesk.com/us/support) k získání těchto hodnot. 

4. Na **podpisový certifikát SAML** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor metadat ve vašem počítači.

    ![Odkaz ke stažení certifikátu](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_certificate.png) 

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurovat jednotné přihlašování uložit tlačítko](./media/topdesk-secure-tutorial/tutorial_general_400.png)

6. Na **TOPdesk – konfigurace zabezpečení** klikněte na tlačítko **TOPdesk konfigurace – zabezpečené** otevřete **nakonfigurovat přihlašování** okna. Kopírovat **URL odhlašování SAML Entity ID a SAML jednotné přihlašování – adresa URL služby** z **Stručná referenční příručka oddílu.**

    ![TOPdesk - konfiguraci zabezpečení](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_configure.png)

7. Přihlaste se k vaší **TOPdesk – zabezpečené** společnosti serveru jako správce.

8. V **TOPdesk** nabídky, klikněte na tlačítko **nastavení**.

    ![Nastavení](./media/topdesk-secure-tutorial/ic790598.png "nastavení")

9. Klikněte na tlačítko **nastavení přihlášení**.

    ![Nastavení přihlášení](./media/topdesk-secure-tutorial/ic790599.png "nastavení přihlášení")

10. Rozbalte **nastavení přihlášení** nabídky a pak klikněte na tlačítko **Obecné**.

    ![Obecné](./media/topdesk-secure-tutorial/ic790600.png "obecné")

11. V **Secure** část **SAML přihlášení** konfigurace části, proveďte následující kroky:

    ![Technické nastavení](./media/topdesk-secure-tutorial/ic790855.png "technické nastavení")

    a. Klikněte na tlačítko **Stáhnout** ke stažení souboru metadat veřejné a pak ho uložte místně ve vašem počítači.

    b. Otevřete soubor metadat a najděte **AssertionConsumerService** uzlu.

    ![Assertion Consumer Service](./media/topdesk-secure-tutorial/ic790856.png "Assertion Consumer Service")

    c. Kopírovat **AssertionConsumerService** hodnota a vložte tuto hodnotu v textovém poli Adresa URL pro odpověď v **TOPdesk – zabezpečení domény a adresy URL** oddílu.

12. Chcete-li vytvořit soubor certifikátu, proveďte následující kroky:

    ![Certifikát](./media/topdesk-secure-tutorial/ic790606.png "certifikátu")

    a. Otevřete soubor stažený metadat z webu Azure portal.

    b. Rozbalte **RoleDescriptor** uzel, který má **xsi: type** z **dodáni: ApplicationServiceType**.

    c. Zkopírujte hodnotu **certifikátu x 509** uzlu.

    d. Uložit zkopírovaný **certifikátu x 509** hodnotu místně na vašem počítači v souboru.

13. V **veřejné** klikněte na tlačítko **přidat**.

    ![Přidat](./media/topdesk-secure-tutorial/ic790607.png "přidat")

14. Na **pomocníka s nastavením konfigurace SAML** dialogového okna stránky, proveďte následující kroky:

    ![Pomocníka s nastavením konfigurace SAML](./media/topdesk-secure-tutorial/ic790608.png "Pomocníka s nastavením konfigurace SAML")

    a. Chcete-li nahrát soubor metadat stažené z webu Azure portal v části **federačních metadat**, klikněte na tlačítko **Procházet**.

    b. K nahrání souboru certifikátu v části **certifikátu (RSA)**, klikněte na tlačítko **Procházet**.

    c. Pro **privátní klíč (RSA, PKCS8, kódování DER)**, můžete nahrát vlastní privátní klíč, nebo můžete kontaktovat [TOPdesk - tým podpory zabezpečení klienta](http://www.topdesk.com/us/support) získat soukromý klíč.

    d. Nahrát soubor loga jste získali v části z na tým podpory TOPdesk **ikona loga**, klikněte na tlačítko **Procházet**.

    e. V **atribut uživatelského jména** textové pole, typ `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    f. V **zobrazovaný název** textového pole zadejte název pro vaši konfiguraci.

    g. Klikněte na **Uložit**.

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovacího uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na webu Azure Portal volá Britta Simon.

   ![Vytvořit testovacího uživatele Azure AD][100]

**Chcete-li vytvořit testovacího uživatele ve službě Azure AD, postupujte následovně:**

1. Na webu Azure Portal, v levém podokně klikněte na tlačítko **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/topdesk-secure-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na tlačítko **všichni uživatelé**.

    !["Uživatele a skupiny" a "Všechny uživatele" odkazy](./media/topdesk-secure-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/topdesk-secure-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/topdesk-secure-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit heslo** zaškrtněte políčko a zapište si hodnotu, která se zobrazí **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.

### <a name="create-a-topdesk---secure-test-user"></a>Vytvoření TOPdesk – zabezpečené testovacího uživatele

Chcete-li povolit uživatele Azure AD k přihlášení do TOPdesk – zabezpečené, se musí být poskytnuty do TOPdesk – zabezpečené.  
V případě TOPdesk – zabezpečené, zřizování je ruční úloha.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Konfigurace zřizování uživatelů, proveďte následující kroky:

1. Přihlaste se k vaší **TOPdesk – zabezpečené** společnosti serveru jako správce.

2. V nabídce v horní části klikněte na tlačítko **TOPdesk \> nový \> podpůrné soubory \> operátor**.

    ![Operátor](./media/topdesk-secure-tutorial/ic790610.png "– operátor")

3. Na **operátor New** dialogového okna, proveďte následující kroky:

    ![Operátor new](./media/topdesk-secure-tutorial/ic790611.png "New – operátor")

    a. Klikněte na tlačítko **Obecné** kartu.

    b. V **příjmení** , jako je textové pole, zadejte příjmení uživatele **Simon**.

    c. Vyberte **lokality** pro tento účet v **umístění** oddílu.

    d. V **přihlašovací jméno** textové pole z **TOPdesk přihlášení** části, zadejte přihlašovací jméno uživatele.

    e. Klikněte na **Uložit**.

> [!NOTE]
> Můžete použít jakékoli jiné TOPdesk – nástroje pro tvorbu zabezpečené uživatelského účtu nebo rozhraní API poskytovaných TOPdesk – zabezpečené ke zřízení účtů služby AAD uživatele.

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit uživatele Azure AD

V této části je povolit Britta Simon používat jednotné přihlašování Azure díky udělení přístupu k TOPdesk – zabezpečené.

![Přiřazení role uživatele][200] 

**Přiřadit Britta Simon TOPdesk – zabezpečení, proveďte následující kroky:**

1. Na webu Azure Portal, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201]

2. V seznamu aplikací vyberte **TOPdesk – zabezpečené**.

    ![TOPdesk – zabezpečené připojení v seznamu aplikací](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_app.png)  

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatele a skupiny"][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogového okna.

    ![Podokno Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogového okna, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogového okna.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogového okna.

### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části Testování služby Azure AD jednotné přihlašování – konfigurace pomocí přístupového panelu.

Po kliknutí na TOPdesk – zabezpečené dlaždici na přístupovém panelu, vám by měl získat automaticky přihlášení k vaší TOPdesk - zabezpečenou webovou aplikaci.
Další informace o přístupovém panelu, naleznete v tématu [Úvod k přístupovému panelu](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje informací:

* [Seznam kurzů o integraci aplikací SaaS pomocí Azure Active Directory](tutorial-list.md)
* [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/topdesk-secure-tutorial/tutorial_general_01.png
[2]: ./media/topdesk-secure-tutorial/tutorial_general_02.png
[3]: ./media/topdesk-secure-tutorial/tutorial_general_03.png
[4]: ./media/topdesk-secure-tutorial/tutorial_general_04.png

[100]: ./media/topdesk-secure-tutorial/tutorial_general_100.png

[200]: ./media/topdesk-secure-tutorial/tutorial_general_200.png
[201]: ./media/topdesk-secure-tutorial/tutorial_general_201.png
[202]: ./media/topdesk-secure-tutorial/tutorial_general_202.png
[203]: ./media/topdesk-secure-tutorial/tutorial_general_203.png