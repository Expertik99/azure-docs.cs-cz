---
title: 'Kurz: Integrace Azure Active Directory s Jamf Pro | Dokumentace Microsoftu'
description: Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a Jamf Pro.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2018
ms.author: jeedes
ms.openlocfilehash: 94b8b935728110cd5dd07b2066e8320274e3b082
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/02/2018
ms.locfileid: "39428413"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>Kurz: Integrace Azure Active Directory s Jamf Pro

V tomto kurzu se dozvíte, jak integrace Jamf Pro s Azure Active Directory (Azure AD).

Integrace Jamf Pro s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k Jamf Pro.
- Uživatele, aby automaticky získat přihlášení k Jamf Pro (Single Sign-On) můžete povolit pomocí jejich účtů služby Azure AD.
- Můžete spravovat své účty na jediném místě – na webu Azure portal.

Pokud chcete zjistit další podrobnosti o integraci aplikací SaaS v Azure AD, přečtěte si téma [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s Jamf Pro, potřebujete následující položky:

- S předplatným služby Azure AD
- Jamf Pro jedno přihlášení povolený předplatné

> [!NOTE]
> Pokud chcete vyzkoušet kroky v tomto kurzu, nedoporučujeme použití produkční prostředí.

Pokud chcete vyzkoušet kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte produkčním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verzi Azure AD, můžete si [získat měsíční zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu je otestovat Azure AD jednotné přihlašování v testovacím prostředí. Scénář popsaný v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Jamf Pro přidání z Galerie
1. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-jamf-pro-from-the-gallery"></a>Jamf Pro přidání z Galerie
Konfigurace integrace Jamf Pro do služby Azure AD, budete muset přidat Jamf Pro do seznamu spravovaných aplikací SaaS z galerie.

**Chcete-li přidat Jamf Pro z galerie, postupujte následovně:**

1. V  **[webu Azure portal](https://portal.azure.com)**, v levém navigačním panelu klikněte na **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

1. Přejděte do **podnikové aplikace**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]
    
1. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko v horní části dialogového okna.

    ![Tlačítko nové aplikace][3]

1. Do vyhledávacího pole zadejte **Jamf Pro**vyberte **Jamf Pro** z panelu výsledků klikněte **přidat** tlačítko pro přidání aplikace.

    ![V seznamu výsledků Jamf Pro](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování služby Azure AD jednotného přihlašování

V této části nakonfigurujete a test Azure AD jednotné přihlašování s Jamf Pro podle testovacího uživatele nazývá "Britta Simon".

Pro jednotné přihlašování pro práci služba Azure AD potřebuje vědět, co uživatel protějšky v Jamf Pro je pro uživatele ve službě Azure AD. Jinými slovy vztah odkazu mezi uživatele služby Azure AD a související uživatelské v Jamf Pro je potřeba navázat.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s Jamf Pro, které potřebujete k dokončení následujících stavebních bloků:

1. **[Konfigurovat Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  – Pokud chcete, aby uživatelé mohli tuto funkci používat.
1. **[Vytvořit testovacího uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
1. **[Vytvoření zkušebního uživatele Jamf Pro](#create-a-jamf-pro-test-user)**  – Pokud chcete mít protějšek Britta Simon v Jamf Pro, který je propojený s Azure AD reprezentace uživatele.
1. **[Přiřadit uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotného přihlašování.
1. **[Otestovat jednotné přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, jestli funguje v konfiguraci.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurace služby Azure AD jednotného přihlašování

V této části Povolení služby Azure AD jednotného přihlašování na portálu Azure portal a konfigurace jednotného přihlašování v aplikaci Jamf Pro.

**Ke konfiguraci Azure AD jednotné přihlašování s Jamf Pro, proveďte následující kroky:**

1. Na webu Azure Portal na **Jamf Pro** integrace stránka aplikace, klikněte na tlačítko **jednotného přihlašování**.

    ![Nakonfigurovat jednotné přihlašování – odkaz][4]

1. Na **jednotného přihlašování** dialogového okna, vyberte **režimu** jako **přihlašování na základě SAML** povolit jednotné přihlašování.
 
    ![Jednotné přihlašování – dialogové okno](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_samlbase.png)

1. Na **Jamf Pro domény a adresy URL** části, proveďte následující kroky, pokud chcete nakonfigurovat aplikace v **IDP** iniciované režimu:

    ![Jamf Pro domény a adresy URL jednotného přihlašování – informace](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url.png)

    a. V **identifikátor (Entity ID)** textového pole zadejte adresu URL pomocí následujícímu vzoru: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. V **adresy URL odpovědi** textového pole zadejte adresu URL pomocí následujícímu vzoru: `https://<subdomain>.jamfcloud.com/saml/SSO`

1. Zkontrolujte **zobrazit pokročilé nastavení URL** a provést následující krok, pokud chcete nakonfigurovat aplikace v **SP** iniciované režimu:

    ![Jamf Pro domény a adresy URL jednotného přihlašování – informace](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url1.png)

    V **přihlašovací adresa URL** textového pole zadejte adresu URL pomocí následujícímu vzoru: `https://<subdomain>.jamfcloud.com`
     
    > [!NOTE]
    > Tyto hodnoty nejsou skutečný. Tyto hodnoty aktualizujte skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL. Zobrazí se skutečná hodnota identifikátoru z **Single Sign-On** části portálu Jamf Pro, který je vysvětlen později v tomto kurzu. Můžete rozbalit skutečnou **subdoménu** hodnoty z hodnoty identifikátor, který budete používat **subdoménu** informace v přihlašovací adresu URL a adresy URL odpovědi.

1. Na **podpisový certifikát SAML** klikněte na tlačítko Kopírovat zkopírujte **adresa Url federačních metadat aplikace** a vložte ho do poznámkového bloku.

    ![Odkaz ke stažení certifikátu](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_certificate.png) 

1. Klikněte na tlačítko **Uložit** tlačítko.

    ![Nakonfigurovat jednotné přihlašování uložit tlačítko](./media/jamfprosamlconnector-tutorial/tutorial_general_400.png)
    
1. V okně jiné webové prohlížeče přihlaste jako správce serveru vaší společnosti Jamf Pro.

1. Klikněte na **ikona nastavení** v pravém horním rohu stránky.

    ![Konfigurace Jamf Pro](./media/jamfprosamlconnector-tutorial/configure1.png)

1. Klikněte na **jednotné přihlašování**.

    ![Konfigurace Jamf Pro](./media/jamfprosamlconnector-tutorial/configure2.png)

1. Na **Single Sign-On** stránku, proveďte následující kroky:

    ![Jeden Jamf Pro](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

    a. Vyberte **Jamf Pro Server** umožňuje jednotné přihlašování přístup.

    b. Výběrem **povolit jednorázové přihlášení pro všechny uživatele** uživatelé nebudou přesměrováni na stránku pro přihlášení zprostředkovatele Identity pro ověřování, ale můžete přihlásit k Jamf Pro přímo místo. Když se uživatel pokusí o přístup k Jamf Pro pomocí zprostředkovatele Identity, dojde k autorizaci a ověřování jednotného přihlašování zahájené pomocí IdP.

    c. Vyberte **NameID** možnost **mapování uživatelů: SAML**. Ve výchozím nastavení, toto nastavení nastavené na **NameID** ale můžete definovat vlastní atribut.

    d. Vyberte **e-mailu** pro **mapování uživatelů: JAMF PRO**. Jamf Pro mapování atributů SAML odeslán ve zprostředkovateli identity následujícími způsoby: uživatelé a skupiny. Když se uživatel pokusí o přístup k Jamf Pro, ve výchozím nastavení Jamf Pro získá informace o uživateli od zprostředkovatele Identity a shoduje s Jamf Pro uživatelské účty. Pokud příchozí uživatelský účet v Jamf Pro neexistuje, pak odpovídající název skupiny vyvolá.

    e. Vložte hodnotu `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` v **název ATRIBUTU skupiny** textového pole.
 
1. Na stejné stránce procházejte až **zprostředkovatele IDENTITY** pod **Single Sign-On** části a proveďte následující kroky:

    ![Konfigurace Jamf Pro](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Vyberte **jiných** jako možnost z **zprostředkovatele IDENTITY** rozevíracího seznamu.

    b. V **ostatní poskytovatele** textového pole zadejte **Azure AD**.

    c. Vyberte **adresa URL metadat** jako možnost z **zdroj METADAT zprostředkovatele IDENTITY** rozevíracího seznamu a v následujícím textovém poli, vložte **adresa Url federačních metadat aplikace** hodnotu, která jste zkopírovali z portálu Azure portal.

    d. Kopírovat **Entity ID** hodnotu a vložte ho do **identifikátor (Entity ID)** textového pole v **Jamf Pro domény a adresy URL** části na webu Azure portal.

    >[!NOTE]
    > Tady rozmazaný hodnota je součástí subdomény. K dokončení přihlašovací adresu URL a adresy URL odpovědi v použít tuto hodnotu **Jamf Pro domény a adresy URL** části na webu Azure portal.

    e. Klikněte na **Uložit**.

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovacího uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na webu Azure Portal volá Britta Simon.

   ![Vytvořit testovacího uživatele Azure AD][100]

**Chcete-li vytvořit testovacího uživatele ve službě Azure AD, postupujte následovně:**

1. Na webu Azure Portal, v levém podokně klikněte na tlačítko **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/jamfprosamlconnector-tutorial/create_aaduser_01.png)

1. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na tlačítko **všichni uživatelé**.

    !["Uživatele a skupiny" a "Všechny uživatele" odkazy](./media/jamfprosamlconnector-tutorial/create_aaduser_02.png)

1. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/jamfprosamlconnector-tutorial/create_aaduser_03.png)

1. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/jamfprosamlconnector-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit heslo** zaškrtněte políčko a zapište si hodnotu, která se zobrazí **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.
 
### <a name="create-a-jamf-pro-test-user"></a>Vytvoření zkušebního uživatele Jamf Pro

Přihlaste se k Jamf Pro Azure AD uživatelům umožnit, musí být poskytnuty do Jamf Pro. V případě Jamf Pro zřizování je ruční úloha.

**K poskytnutí uživatelského účtu, postupujte následovně:**

1. Přihlaste se na web společnosti Jamf Pro jako správce.

1. Klikněte na **ikona nastavení** v pravém horním rohu stránky.

    ![Přidat zaměstnance](./media/jamfprosamlconnector-tutorial/configure1.png)

1. Klikněte na **Jamf Pro uživatelské účty a skupiny**.

    ![Přidat zaměstnance](./media/jamfprosamlconnector-tutorial/user1.png)

1. Klikněte na možnost **Nové**.

    ![Přidat zaměstnance](./media/jamfprosamlconnector-tutorial/user2.png)

1. Vyberte **vytvořit standardní účet**.

    ![Přidat zaměstnance](./media/jamfprosamlconnector-tutorial/user3.png)

1. Na **nový účet** dailog, proveďte následující kroky:

    ![Přidat zaměstnance](./media/jamfprosamlconnector-tutorial/user4.png)

    a. V **uživatelské jméno** textového pole zadejte úplný název BrittaSimon.

    b. Vyberte příslušné možnosti podle vaší organizaci pro **úroveň přístupu**, **oprávnění NASTAVENA**a pro **stav přístupu**.
    
    c. V **jméno a příjmení** textového pole zadejte úplný název Britta Simon.

    d. V **e-MAILOVOU adresu** textového pole zadejte e-mailovou adresu účtu Britta Simon.

    e. V **heslo** textového pole zadejte heslo uživatele.

    f. V **OVĚŘTE heslo** textového pole zadejte heslo uživatele.

    g. Klikněte na **Uložit**.

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit uživatele Azure AD

V této části je povolit Britta Simon používat jednotné přihlašování Azure díky udělení přístupu k Jamf Pro.

![Přiřazení role uživatele][200] 

**Pokud chcete přiřadit Britta Simon k Jamf Pro, proveďte následující kroky:**

1. Na webu Azure Portal, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201] 

1. V seznamu aplikací vyberte **Jamf Pro**.

    ![Jamf Pro odkaz v seznamu aplikací](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_app.png)  

1. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatele a skupiny"][202]

1. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogového okna.

    ![Podokno Přidat přiřazení][203]

1. Na **uživatelů a skupin** dialogového okna, vyberte **Britta Simon** v seznamu uživatelů.

1. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogového okna.

1. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogového okna.
    
### <a name="test-single-sign-on"></a>Otestovat jednotné přihlašování

V této části Testování služby Azure AD jednotné přihlašování – konfigurace pomocí přístupového panelu.

Když kliknete na dlaždici Jamf Pro na přístupovém panelu, vám by měl získat automaticky přihlášení k aplikaci Jamf Pro.
Další informace o přístupovém panelu, naleznete v tématu [Úvod k přístupovému panelu](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje informací:

* [Seznam kurzů o integraci aplikací SaaS pomocí Azure Active Directory](tutorial-list.md)
* [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/jamfprosamlconnector-tutorial/tutorial_general_01.png
[2]: ./media/jamfprosamlconnector-tutorial/tutorial_general_02.png
[3]: ./media/jamfprosamlconnector-tutorial/tutorial_general_03.png
[4]: ./media/jamfprosamlconnector-tutorial/tutorial_general_04.png

[100]: ./media/jamfprosamlconnector-tutorial/tutorial_general_100.png

[200]: ./media/jamfprosamlconnector-tutorial/tutorial_general_200.png
[201]: ./media/jamfprosamlconnector-tutorial/tutorial_general_201.png
[202]: ./media/jamfprosamlconnector-tutorial/tutorial_general_202.png
[203]: ./media/jamfprosamlconnector-tutorial/tutorial_general_203.png

