---
title: 'Kurz: Integrace Azure Active Directory se službou TimeOffManager | Dokumentace Microsoftu'
description: Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a TimeOffManager.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: e7f0f6bb778dedeea61b74b5ca0c2edbadd5279b
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430283"
---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Kurz: Integrace Azure Active Directory se službou TimeOffManager

V tomto kurzu se dozvíte, jak integrovat TimeOffManager s Azure Active Directory (Azure AD).

TimeOffManager integraci se službou Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k TimeOffManager
- Můžete povolit uživatelům, aby automaticky získat přihlášení k TimeOffManager (Single Sign-On) s jejich účty Azure AD
- Můžete spravovat své účty na jediném místě – na webu Azure portal

Pokud chcete zjistit další podrobnosti o integraci aplikací SaaS v Azure AD, přečtěte si téma [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s TimeOffManager, potřebujete následující položky:

- S předplatným služby Azure AD
- TimeOffManager jednotného přihlašování povolená předplatného

> [!NOTE]
> Pokud chcete vyzkoušet kroky v tomto kurzu, nedoporučujeme použití produkční prostředí.

Pokud chcete vyzkoušet kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte produkčním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verzi Azure AD, můžete si [získat měsíční zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu je otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénář popsaný v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání TimeOffManager z Galerie
1. Konfigurace a otestování služby Azure AD jednotného přihlašování

## <a name="add-timeoffmanager-from-the-gallery"></a>Přidání TimeOffManager z Galerie
Konfigurace integrace TimeOffManager do služby Azure AD, budete muset přidat TimeOffManager z Galerie na váš seznam spravovaných aplikací SaaS.

**Chcete-li přidat TimeOffManager z galerie, postupujte následovně:**

1. V  **[webu Azure portal](https://portal.azure.com)**, v levém navigačním panelu klikněte na **Azure Active Directory** ikonu. 

    ![Active Directory][1]

1. Přejděte do **podnikové aplikace**. Pak přejděte na **všechny aplikace**.

    ![Aplikace][2]
    
1. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko v horní části dialogového okna.

    ![Aplikace][3]

1. Do vyhledávacího pole zadejte **TimeOffManager**vyberte **TimeOffManager** panel výsledek a pak klikněte na **přidat** tlačítko pro přidání aplikace.

    ![Přidat z Galerie](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování služby Azure AD jednotného přihlašování
V této části Konfigurace a testování Azure AD jednotné přihlašování pomocí TimeOffManager podle testovacího uživatele nazývá "Britta Simon".

Pro jednotné přihlašování pro práci služba Azure AD potřebuje vědět, co uživatel protějšky v TimeOffManager je pro uživatele ve službě Azure AD. Jinými slovy vztah odkazu mezi uživatele služby Azure AD a související uživatelské v TimeOffManager potřeba navázat.

V TimeOffManager, přiřaďte hodnotu **uživatelské jméno** ve službě Azure AD jako hodnotu **uživatelské jméno** a tím vytvoří vztah odkazu.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s TimeOffManager, které potřebujete k dokončení následujících stavebních bloků:

1. **[Konfigurovat Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  – Pokud chcete, aby uživatelé mohli tuto funkci používat.
1. **[Vytvořit testovacího uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
1. **[Vytvoření zkušebního uživatele TimeOffManager](#create-a-timeoffmanager-test-user)**  – Pokud chcete mít protějšek Britta Simon TimeOffManager, který je propojený s Azure AD reprezentace uživatele.
1. **[Přiřadit uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotného přihlašování.
1. **[Otestovat jednotné přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, jestli funguje v konfiguraci.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurace služby Azure AD jednotného přihlašování

V této části Povolení služby Azure AD jednotného přihlašování na portálu Azure portal a konfigurace jednotného přihlašování v aplikaci TimeOffManager.

**Ke konfiguraci Azure AD jednotné přihlašování s TimeOffManager, proveďte následující kroky:**

1. Na webu Azure Portal na **TimeOffManager** integrace stránka aplikace, klikněte na tlačítko **jednotného přihlašování**.

    ![Konfigurace jednotného přihlašování][4]

1. Na **jednotného přihlašování** dialogového okna, vyberte **režimu** jako **přihlašování na základě SAML** povolit jednotné přihlašování.
 
    ![Přihlašování založené na SAML](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_samlbase.png)

1. Na **TimeOffManager domény a adresy URL** části, proveďte následující kroky:

     ![Část TimeOffManager domény a adresy URL](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_url.png)

    V **adresy URL odpovědi** textového pole zadejte adresu URL pomocí následujícímu vzoru: `https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`

    > [!NOTE] 
    > Tato hodnota není skutečný. Aktualizujte tuto hodnotu Skutečná adresa URL odpovědi. Tuto hodnotu z lze získat **jednotného přihlašování na stránce nastavení** je vysvětleno dále v kurzu nebo kontaktujte [tým podpory TimeOffManager](https://www.purelyhr.com/contact-us).
 
1. Na **podpisový certifikát SAML** klikněte na tlačítko **certifikát (Base64)** a uložte soubor certifikátu v počítači.

    ![Části podpisový certifikát SAML](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_certificate.png) 

1. Cílem této části se popisují, jak povolit uživatelům ověření pro TimeOffManger pomocí svého účtu ve službě Azure AD využívající federaci založené na protokolu SAML.
    
    Vaše aplikace TimeOffManger očekává, že kontrolní výrazy SAML v určitém formátu, který je potřeba přidat vlastní atribut mapování konfigurace atributy tokenu SAML. Následující snímek obrazovky ukazuje příklad pro tuto.

    ![atributy tokenu SAML](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "atributy tokenu saml")
    
    | Název atributu | Hodnota atributu |
    | --- | --- |
    | jméno |User.givenName |
    | Příjmení |User.Surname |
    | Email |User.Mail |
    
    a.  Pro každý řádek dat v tabulce výše, klikněte na tlačítko **přidat atribut uživatele**.
    
    ![atributy tokenu SAML](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "atributy tokenu saml")
    
    ![atributy tokenu SAML](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "atributy tokenu saml")
    
    b.  V **název atributu** textového pole zadejte název atributu, který je zobrazený pro tento řádek.
    
    c.  V **hodnota atributu** textové pole, vyberte hodnotu atributu zobrazený pro tento řádek.
    
    d.  Klikněte na tlačítko **OK**.
    
1. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurace jednotného přihlašování](./media/timeoffmanager-tutorial/tutorial_general_400.png)

1. Na **TimeOffManager konfigurace** klikněte na tlačítko **nakonfigurovat TimeOffManager** otevřete **nakonfigurovat přihlašování** okna. Kopírovat **URL odhlašování SAML Entity ID a SAML jednotné přihlašování – adresa URL služby** z **Stručná referenční příručka oddílu.**

    ![Oddíl konfigurace TimeOffManager](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_configure.png) 

1. V okně jiné webové prohlížeče přihlaste jako správce serveru vaší společnosti TimeOffManager.

1. Přejděte na **účet \> možnosti účtu \> jednotné přihlašování – nastavení**.
   
   ![Jednotné přihlašování – nastavení](./media/timeoffmanager-tutorial/ic795917.png "jednotné přihlašování – nastavení")
1. V **nastavení jednotného přihlašování** části, proveďte následující kroky:
   
   ![Jednotné přihlašování – nastavení](./media/timeoffmanager-tutorial/ic795918.png "jednotné přihlašování – nastavení")
   
   a. Otevřete váš certifikát base-64 kódovaných v poznámkovém bloku, zkopírujte obsah ho do schránky a vložte celý certifikát do **certifikát X.509** textového pole.
   
   b. V **Vystavitel zprostředkovatele identity** textového pole vložte hodnotu **SAML Entity ID** zkopírovanou z webu Azure portal.
   
   c. V **adresu URL koncového bodu IdP** textového pole vložte hodnotu **SAML jednotné přihlašování – adresa URL služby** zkopírovanou z webu Azure portal.
   
   d. Jako **vynutit SAML**vyberte **ne**.
   
   e. Jako **uživatele automaticky vytvářet**vyberte **Ano**.
   
   f. V **odhlašovací adresa URL** textového pole vložte hodnotu **odhlašování URL** zkopírovanou z webu Azure portal.
   
   g. Klikněte na tlačítko **uložit změny**.

1. V **nastavení jednotného přihlašování ve** stránky, zkopírujte hodnotu **adresa URL služby Assertion příjemce** a vložte ji **adresy URL odpovědi** textového pole pod **TimeOffManager Domény a adresy URL** části webu Azure Portal. 

      ![Jednotné přihlašování – nastavení](./media/timeoffmanager-tutorial/ic795915.png "jednotné přihlašování – nastavení")

> [!TIP]
> Teď si můžete přečíst stručné verzi těchto pokynů uvnitř [webu Azure portal](https://portal.azure.com), zatímco jsou nastavení aplikace!  Po přidání této aplikace z **služby Active Directory > podnikové aplikace** části, stačí kliknout **Single Sign-On** kartu a přístup k vložené dokumentaci prostřednictvím  **Konfigurace** oblast v dolní části. Další informace o funkci vložená dokumentace: [dokumentace ke službě Azure AD embedded]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovacího uživatele Azure AD
Cílem této části je vytvoření zkušebního uživatele na webu Azure Portal volá Britta Simon.

![Vytvoření uživatele Azure AD][100]

**Chcete-li vytvořit testovacího uživatele ve službě Azure AD, postupujte následovně:**

1. V **webu Azure portal**, v levém navigačním podokně klikněte na tlačítko **Azure Active Directory** ikonu.

    ![Vytváří se testovací uživatele služby Azure AD](./media/timeoffmanager-tutorial/create_aaduser_01.png) 

1. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin** a klikněte na tlačítko **všichni uživatelé**.
    
    ![Uživatelé a skupiny--> všechny uživatele](./media/timeoffmanager-tutorial/create_aaduser_02.png) 

1. Chcete-li otevřít **uživatele** dialogového okna, klikněte na tlačítko **přidat** horní části dialogového okna.
 
    ![Přidání tlačítka](./media/timeoffmanager-tutorial/create_aaduser_03.png) 

1. Na **uživatele** dialogového okna stránky, proveďte následující kroky:
 
    ![Dialogové okno stránky uživatele](./media/timeoffmanager-tutorial/create_aaduser_04.png) 

    a. V **název** textové pole, typ **BrittaSimon**.

    b. V **uživatelské jméno** textové pole, typ **e-mailová adresa** z BrittaSimon.

    c. Vyberte **zobrazit heslo** a zapište si hodnotu **heslo**.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-timeoffmanager-test-user"></a>Vytvoření zkušebního uživatele TimeOffManager

Chcete-li povolit uživatele Azure AD k přihlášení do TimeOffManager, musí být poskytnuty k TimeOffManager.  

TimeOffManager podporuje jenom v době zřizování uživatelů. Neexistuje žádná položka akce za vás.  

Uživatelé se přidají automaticky při prvním přihlášení pomocí jednotného přihlašování na.

>[!NOTE]
>Můžete použít jakékoli jiné TimeOffManager uživatelského účtu nástrojů pro vytváření nebo rozhraní API poskytovaných TimeOffManager zřízení uživatelských účtů služby Azure AD.
> 

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit uživatele Azure AD

V této části je povolit Britta Simon k udělení přístupu k TimeOffManager použití Azure jednotného přihlašování.

![Přiřadit uživatele][200] 

**Přiřadit TimeOffManager Britta Simon, proveďte následující kroky:**

1. Na webu Azure Portal, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

1. V seznamu aplikací vyberte **TimeOffManager**.

    ![TimeOffManager v seznamu aplikací](./media/timeoffmanager-tutorial/tutorial_timeoffmanager_app.png) 

1. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Přiřadit uživatele][202] 

1. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogového okna.

    ![Přiřadit uživatele][203]

1. Na **uživatelů a skupin** dialogového okna, vyberte **Britta Simon** v seznamu uživatelů.

1. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogového okna.

1. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogového okna.
    
### <a name="test-single-sign-on"></a>Otestovat jednotné přihlašování

V této části Testování služby Azure AD jednotné přihlašování – konfigurace pomocí přístupového panelu.

Po kliknutí na dlaždici TimeOffManager na přístupovém panelu, vám by měl získat automaticky přihlášení k aplikaci TimeOffManager. Další informace o přístupovém panelu, naleznete v tématu [Úvod k přístupovému panelu](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Další zdroje informací:

* [Seznam kurzů o integraci aplikací SaaS pomocí Azure Active Directory](tutorial-list.md)
* [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/timeoffmanager-tutorial/tutorial_general_01.png
[2]: ./media/timeoffmanager-tutorial/tutorial_general_02.png
[3]: ./media/timeoffmanager-tutorial/tutorial_general_03.png
[4]: ./media/timeoffmanager-tutorial/tutorial_general_04.png

[100]: ./media/timeoffmanager-tutorial/tutorial_general_100.png

[200]: ./media/timeoffmanager-tutorial/tutorial_general_200.png
[201]: ./media/timeoffmanager-tutorial/tutorial_general_201.png
[202]: ./media/timeoffmanager-tutorial/tutorial_general_202.png
[203]: ./media/timeoffmanager-tutorial/tutorial_general_203.png

