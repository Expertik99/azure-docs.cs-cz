---
title: 'Kurz: Integrace Azure Active Directory se službou SAP NetWeaver | Dokumentace Microsoftu'
description: Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a SAP NetWeaver.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 1b9e59e3-e7ae-4e74-b16c-8c1a7ccfdef3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: b773380a21e0c47a1e1519e592aa0ddd5e6388fa
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421946"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-netweaver"></a>Kurz: Integrace Azure Active Directory se službou SAP NetWeaver

V tomto kurzu se dozvíte, jak integrovat SAP NetWeaver pomocí Azure Active Directory (Azure AD).

SAP NetWeaver integraci se službou Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k SAP NetWeaver
- Uživatele, aby automaticky získat přihlášení k SAP NetWeaver (Single Sign-On) můžete povolit pomocí jejich účtů služby Azure AD
- Můžete spravovat své účty na jediném místě – na webu Azure portal

Pokud chcete zjistit další podrobnosti o integraci aplikací SaaS v Azure AD, přečtěte si téma [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](../manage-apps/what-is-single-sign-on.md).


## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s SAP NetWeaver, potřebujete následující položky:

- S předplatným služby Azure AD
- SAP NetWeaver jednotného přihlašování povolená předplatného

> [!NOTE]
> Pokud chcete vyzkoušet kroky v tomto kurzu, nedoporučujeme použití produkční prostředí.

Pokud chcete vyzkoušet kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte produkčním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verzi Azure AD, můžete získat měsíční zkušební [tady](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu je otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénář popsaný v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání SAP NetWeaver v galerii
1. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-sap-netweaver-from-the-gallery"></a>Přidání SAP NetWeaver v galerii
Konfigurace integrace SAP NetWeaver do služby Azure AD, budete muset přidat SAP NetWeaver na váš seznam spravovaných aplikací SaaS z galerie.

**SAP NetWeaver přidat z galerie, postupujte následovně:**

1. V  **[webu Azure Portal](https://portal.azure.com)**, v levém navigačním panelu klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

1. Přejděte do **podnikové aplikace**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
1. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko v horní části dialogového okna.

    ![Aplikace][3]

1. Do vyhledávacího pole zadejte **SAP NetWeaver**.

    ![Vytváří se testovací uživatele služby Azure AD](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_search.png)

1. Na panelu výsledků vyberte **SAP NetWeaver**a potom klikněte na tlačítko **přidat** tlačítko pro přidání aplikace.

    ![Vytváří se testovací uživatele služby Azure AD](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurace a testování Azure AD jednotného přihlašování
V této části nakonfigurovat a otestovat Azure AD jednotného přihlašování se SAP Netweaverem podle testovacího uživatele nazývá "Britta Simon."

Pro jednotné přihlašování pro práci služba Azure AD potřebuje vědět, co uživatel protějšky v SAP NetWeaver je pro uživatele ve službě Azure AD. Jinými slovy vztah odkazu mezi uživatele služby Azure AD a souvisejících uživatelem v systému SAP NetWeaver potřeba navázat.

Tento odkaz vztah navázaný přiřazením hodnoty **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** v SAP Netweaveru.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování pomocí SAP NetWeaver, které potřebujete k dokončení následujících stavebních bloků:

1. **[Konfigurace Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  – Pokud chcete, aby uživatelé mohli tuto funkci používat.
1. **[Vytváří se testovací uživatele služby Azure AD](#creating-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
1. **[Vytváří se testovací uživatel aplikace SAP NetWeaver](#creating-an-sap-netweaver-test-user)**  – Pokud chcete mít protějšek Britta Simon v SAP NetWeaver, který je propojený s Azure AD reprezentace uživatele.
1. **[Přiřazení testovacího uživatele Azure AD](#assigning-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotného přihlašování.
1. **[Testování Single Sign-On](#testing-single-sign-on)**  – Pokud chcete ověřit, jestli funguje v konfiguraci.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurace služby Azure AD jednotného přihlašování

V této části Povolení služby Azure AD jednotného přihlašování na portálu Azure portal a konfigurace jednotného přihlašování v aplikaci SAP NetWeaver.

**Ke konfiguraci Azure AD jednotného přihlašování se SAP Netweaverem, proveďte následující kroky:**

1. Na webu Azure Portal na **SAP NetWeaver** integrace stránka aplikace, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace jednotného přihlašování][4]

1. Na **jednotného přihlašování** dialogového okna, vyberte **režimu** jako **přihlašování na základě SAML** povolit jednotné přihlašování.
 
    ![Konfigurace jednotného přihlašování](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_samlbase.png)

1. Na **SAP NetWeaver domény a adresy URL** části, proveďte následující kroky:

    ![Konfigurace jednotného přihlašování](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_url.png)

    a. V **přihlašovací adresa URL** textového pole zadejte adresu URL pomocí následujícímu vzoru: `https://<your company instance of SAP NetWeaver>`

    b. V **identifikátor** textového pole zadejte adresu URL pomocí následujícímu vzoru: `https://<your company instance of SAP NetWeaver>`

    c. V **adresy URL odpovědi** textového pole zadejte adresu URL pomocí následujícímu vzoru: `https://<your company instance of SAP NetWeaver>/sap/saml2/sp/acs/100`
     
    > [!NOTE] 
    > Tyto hodnoty nejsou reálné. Aktualizujte tyto hodnoty se skutečné identifikátorem a adresa URL odpovědi a přihlašovací adresa URL. Tady doporučujeme používat jedinečnou hodnotu řetězce v identifikátoru. Kontakt [tým podpory SAP NetWeaver klienta](https://www.sap.com/support.html) k získání těchto hodnot. 

1. Na **podpisový certifikát SAML** klikněte na tlačítko **soubor XML s metadaty** a uložte soubor XML ve vašem počítači.

    ![Konfigurace jednotného přihlašování](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_certificate.png) 

1. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurace jednotného přihlašování](./media/sap-netweaver-tutorial/tutorial_general_400.png)
    
1. Na **konfigurace SAP NetWeaver** klikněte na tlačítko **konfigurace SAP NetWeaver** otevřete **nakonfigurovat přihlašování** okna. Kopírovat **SAML Entity ID** z **Stručná referenční příručka oddílu.**

    ![Konfigurace jednotného přihlašování](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_configure.png) 

1. Ke konfiguraci jednotného přihlašování na **SAP NetWeaver** straně, je nutné odeslat na stažený **soubor XML s metadaty** a **SAML Entity ID** k [podpory SAP NetWeaver](https://www.sap.com/support.html). 

> [!TIP]
> Teď si můžete přečíst stručné verzi těchto pokynů uvnitř [webu Azure portal](https://portal.azure.com), zatímco jsou nastavení aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace** části, stačí kliknout **Single Sign-On** kartu a přístup k vložené dokumentaci prostřednictvím  **Konfigurace** oblast v dolní části. Další informace o funkci vložená dokumentace: [dokumentace ke službě Azure AD embedded]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Vytváří se testovací uživatele služby Azure AD
Cílem této části je vytvoření zkušebního uživatele na webu Azure Portal volá Britta Simon.

![Vytvoření uživatele Azure AD][100]

**Chcete-li vytvořit testovacího uživatele ve službě Azure AD, postupujte následovně:**

1. V **webu Azure portal**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváří se testovací uživatele služby Azure AD](./media/sap-netweaver-tutorial/create_aaduser_01.png) 

1. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Vytváří se testovací uživatele služby Azure AD](./media/sap-netweaver-tutorial/create_aaduser_02.png) 

1. Chcete-li otevřít **uživatele** dialogového okna, klikněte na tlačítko **přidat** horní části dialogového okna.
 
    ![Vytváří se testovací uživatele služby Azure AD](./media/sap-netweaver-tutorial/create_aaduser_03.png) 

1. Na **uživatele** dialogového okna stránky, proveďte následující kroky:
 
    ![Vytváří se testovací uživatele služby Azure AD](./media/sap-netweaver-tutorial/create_aaduser_04.png) 

    a. V **název** textové pole, typ **Britta Simon**.

    b. V **uživatelské jméno** textové pole, typ **e-mailová adresa** Britta Simon.

    c. Vyberte **zobrazit heslo** a zapište si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="creating-an-sap-netweaver-test-user"></a>Vytvoření SAP NetWeaver testovacího uživatele

V této části vytvoříte uživatele v SAP NetWeaver jako Britta Simon. Práce s vaší [podpory SAP NetWeaver](https://www.sap.com/support.html) přidat uživatele na platformě SAP Netweaveru.

### <a name="assigning-the-azure-ad-test-user"></a>Přiřazení testovacího uživatele Azure AD

V této části je povolit Britta Simon používat jednotné přihlašování Azure tím, že udělíte přístup k SAP NetWeaver.

![Přiřadit uživatele][200] 

**Britta Simon přiřadit SAP NetWeaver, proveďte následující kroky:**

1. Na webu Azure Portal, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

1. V seznamu aplikací vyberte **SAP NetWeaver**.

    ![Konfigurace jednotného přihlašování](./media/sap-netweaver-tutorial/tutorial_sapnetweaver_app.png) 

1. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

1. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogového okna.

    ![Přiřadit uživatele][203]

1. Na **uživatelů a skupin** dialogového okna, vyberte **Britta Simon** v seznamu uživatelů.

1. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogového okna.

1. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogového okna.
    
### <a name="testing-single-sign-on"></a>Testování jednotného přihlašování

V této části Testování služby Azure AD jednotné přihlašování – konfigurace pomocí přístupového panelu.

Po kliknutí na dlaždici SAP NetWeaver na přístupovém panelu, vám by měl získat automaticky přihlášení k aplikaci SAP NetWeaver.

## <a name="additional-resources"></a>Další zdroje informací:

* [Seznam kurzů o integraci aplikací SaaS pomocí Azure Active Directory](tutorial-list.md)
* [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/sap-netweaver-tutorial/tutorial_general_01.png
[2]: ./media/sap-netweaver-tutorial/tutorial_general_02.png
[3]: ./media/sap-netweaver-tutorial/tutorial_general_03.png
[4]: ./media/sap-netweaver-tutorial/tutorial_general_04.png

[100]: ./media/sap-netweaver-tutorial/tutorial_general_100.png

[200]: ./media/sap-netweaver-tutorial/tutorial_general_200.png
[201]: ./media/sap-netweaver-tutorial/tutorial_general_201.png
[202]: ./media/sap-netweaver-tutorial/tutorial_general_202.png
[203]: ./media/sap-netweaver-tutorial/tutorial_general_203.png

