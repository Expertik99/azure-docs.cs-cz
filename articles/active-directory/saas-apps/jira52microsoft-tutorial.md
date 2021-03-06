---
title: 'Kurz: Integrace Azure Active Directory s jednotným Přihlašováním SAML JIRA Microsoft (V5.2) | Dokumentace Microsoftu'
description: Zjistěte, jak nakonfigurovat jednotné přihlašování mezi Azure Active Directory a jednotné přihlašování SAML JIRA Microsoft (V5.2).
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d0c00408-f9b8-4a79-bccc-c346a7331845
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: jeedes
ms.openlocfilehash: d7f53efd4b473f36aa03628da4992d1c4c2fb04b
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2018
ms.locfileid: "42058081"
---
# <a name="tutorial-azure-active-directory-integration-with-jira-saml-sso-by-microsoft-v52"></a>Kurz: Integrace Azure Active Directory s jednotným Přihlašováním SAML JIRA Microsoft (V5.2)

V tomto kurzu se dozvíte, jak integrovat JIRA SAML SSO Microsoft (V5.2) se službou Azure Active Directory (Azure AD).

Integrace jednotného přihlašování SAML JIRA Microsoft (V5.2) s Azure AD poskytuje následující výhody:

- Můžete řídit ve službě Azure AD, který má přístup k systému JIRA SAML SSO Microsoft (V5.2).
- Uživatele, aby automaticky získat podepsaný ve službě JIRA SAML SSO ve společnosti Microsoft (verze 5.2) (jednotné přihlašování) můžete povolit pomocí jejich účtů služby Azure AD.
- Můžete spravovat své účty na jediném místě – na webu Azure portal.

Pokud chcete zjistit další podrobnosti o integraci aplikací SaaS v Azure AD, přečtěte si téma [co je přístup k aplikaci a jednotné přihlašování s Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="description"></a>Popis

Povolit jednotné přihlašování pomocí účtu Microsoft Azure Active Directory serverem Atlassian JIRA. Tímto způsobem všichni uživatelé vaší organizace můžete použít přihlašovací údaje služby Azure AD k přihlášení do aplikace systému JIRA. Tento modul plug-in používá protokol SAML 2.0 pro federaci.

## <a name="prerequisites"></a>Požadavky

Konfigurace integrace Azure AD s jednotným Přihlašováním SAML JIRA Microsoft (V5.2), potřebujete následující položky:

- Předplatné Azure AD
- Základní JIRA a 5.2 softwaru by měl nainstalovaná a nakonfigurovaná na verzi Windows 64-bit
- JIRA server je povolen protokol HTTPS
- Všimněte si, že podporované verze pro modul plug-in JIRA jsou uvedeny v následující části.
- JIRA server je dostupný na Internetu zejména na stránku pro přihlášení k Azure AD pro ověřování a měli byste může přijímat token ze služby Azure AD
- Přihlašovací údaje správce jsou nastavené ve službě JIRA
- WebSudo je zakázaný ve službě JIRA
- Testovací uživatel v serveru aplikaci JIRA

> [!NOTE]
> Pokud chcete vyzkoušet kroky v tomto kurzu, nedoporučujeme použití produkčního prostředí JIRA. Nejdřív otestovat integraci v vývojové nebo testovací prostředí aplikace a pak používat produkčním prostředí.

Pokud chcete vyzkoušet kroky v tomto kurzu, postupujte podle těchto doporučení:

- Nepoužívejte produkčním prostředí, pokud to není nutné.
- Pokud nemáte prostředí zkušební verzi Azure AD, můžete si [získat měsíční zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).

**Podporované verze:**

*   Základní JIRA a Software: 5.2
*   JIRA podporuje taky verze 6.0 a 7.8. Další podrobnosti získáte kliknutím [JIRA SAML SSO společností Microsoft](jiramicrosoft-tutorial.md)

## <a name="scenario-description"></a>Popis scénáře
V tomto kurzu je otestovat Azure AD jednotné přihlašování v testovacím prostředí.
Scénář popsaný v tomto kurzu se skládá ze dvou hlavních stavebních bloků:

1. Přidání jednotného přihlašování SAML JIRA Microsoft (V5.2) z Galerie
2. Konfigurace a testování Azure AD jednotného přihlašování

## <a name="adding-jira-saml-sso-by-microsoft-v52-from-the-gallery"></a>Přidání jednotného přihlašování SAML JIRA Microsoft (V5.2) z Galerie
Pokud chcete nakonfigurovat integraci jednotného přihlašování SAML JIRA Microsoft (V5.2) do služby Azure AD, budete muset přidat JIRA SAML SSO Microsoft (V5.2) z Galerie na váš seznam spravovaných aplikací SaaS.

**Chcete-li přidat JIRA SAML SSO Microsoft (V5.2) z galerie, postupujte následovně:**

1. V **[webu Azure portal](https://portal.azure.com)**, v levém navigačním panelu klikněte na **Azure Active Directory** ikonu. 

    ![Tlačítko Azure Active Directory][1]

2. Přejděte do **podnikové aplikace**. Pak přejděte na **všechny aplikace**.

    ![V okně podnikové aplikace][2]

3. Chcete-li přidat novou aplikaci, klikněte na tlačítko **novou aplikaci** tlačítko v horní části dialogového okna.

    ![Tlačítko nové aplikace][3]

4. Do vyhledávacího pole zadejte **JIRA SAML SSO Microsoft (V5.2)** vyberte **JIRA SAML SSO Microsoft (V5.2)** z panelu výsledků klikněte **přidat** tlačítko pro přidání aplikace.

    ![JIRA SAML SSO ve společnosti Microsoft (verze 5.2) v seznamu výsledků](./media/jira52microsoft-tutorial/tutorial_singlesign-onforjira5.2_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurace a otestování služby Azure AD jednotného přihlašování

V této části nakonfigurovat a otestovat Azure AD jednotné přihlašování s jednotným Přihlašováním SAML JIRA Microsoft (V5.2) na základě testovací uživatele nazývá "Britta Simon".

Pro jednotné přihlašování pro práci služba Azure AD potřebuje vědět, co uživatel protějšky v systému JIRA SAML SSO Microsoft (V5.2) je pro uživatele ve službě Azure AD. Jinými slovy vztah odkazu mezi uživatele služby Azure AD a souvisejících uživatelem v systému JIRA SAML SSO Microsoft (V5.2) je potřeba navázat.

Nakonfigurovat a otestovat Azure AD jednotné přihlašování s jednotným Přihlašováním SAML JIRA Microsoft (V5.2), které potřebujete k dokončení následujících stavebních bloků:

1. **[Konfigurovat Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  – Pokud chcete, aby uživatelé mohli tuto funkci používat.
2. **[Vytvořit testovacího uživatele Azure AD](#create-an-azure-ad-test-user)**  – Pokud chcete otestovat Azure AD jednotné přihlašování s Britta Simon.
3. **[Vytvoření JIRA SAML SSO ve společnosti Microsoft (verze 5.2) testovacího uživatele](#create-a-jira-saml-sso-by-microsoft-v52-test-user)**  – Pokud chcete mít protějšek Britta Simon v systému JIRA SAML SSO ve společnosti Microsoft (verze 5.2), který je propojený s Azure AD reprezentace uživatele.
4. **[Přiřadit uživatele Azure AD](#assign-the-azure-ad-test-user)**  – Pokud chcete povolit Britta Simon používat Azure AD jednotného přihlašování.
5. **[Otestovat jednotné přihlašování](#test-single-sign-on)**  – Pokud chcete ověřit, jestli funguje v konfiguraci.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurace služby Azure AD jednotného přihlašování

V této části Povolení služby Azure AD jednotného přihlašování na portálu Azure portal a konfigurace jednotného přihlašování ve vašich JIRA SAML SSO aplikací společnosti Microsoft (verze 5.2).

**Ke konfiguraci Azure AD jednotné přihlašování s jednotným Přihlašováním SAML JIRA Microsoft (V5.2), proveďte následující kroky:**

1. Na webu Azure Portal na **JIRA SAML SSO Microsoft (V5.2)** integrace stránka aplikace, klikněte na tlačítko **jednotného přihlašování**.

    ![Nakonfigurovat jednotné přihlašování – odkaz][4]

2. Na **jednotného přihlašování** dialogového okna, vyberte **režimu** jako **přihlašování na základě SAML** povolit jednotné přihlašování.

    ![Jednotné přihlašování – dialogové okno](./media/jira52microsoft-tutorial/tutorial_singlesign-onforjira5.2_samlbase.png)

3. Na **JIRA SAML SSO Microsoft Domain a adresy URL** části, proveďte následující kroky:

    ![JIRA SAML SSO Microsoft Domain a adresy URL jednotné přihlašování – informace](./media/jira52microsoft-tutorial/tutorial_singlesign-onforjira5.2_url.png)

    a. V **přihlašovací adresa URL** textového pole zadejte adresu URL pomocí následujícímu vzoru: `https://<domain:port>/plugins/servlet/saml/auth`

    b. V **identifikátor** textového pole zadejte adresu URL pomocí následujícímu vzoru: `https://<domain:port>/`

    c. V **adresy URL odpovědi** textového pole zadejte adresu URL pomocí následujícímu vzoru: `https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE]
    > Tyto hodnoty nejsou skutečný. Tyto hodnoty aktualizujte skutečné identifikátor, adresa URL odpovědi a přihlašovací adresa URL. Port je volitelný, v případě, že je pojmenovaný URL. Tyto hodnoty jsou přijímány během konfigurace modulu plug-in Jira, který je vysvětlen později v tomto kurzu.

4. Na **podpisový certifikát SAML** klikněte na tlačítko Kopírovat zkopírujte **adresa Url federačních metadat aplikace** a vložte ho do poznámkového bloku.

    ![Konfigurace jednotného přihlašování](./media/jira52microsoft-tutorial/tutorial_metadataurl.png)

5. Klikněte na tlačítko **Uložit** tlačítko.

    ![Konfigurace jednotného přihlašování](./media/jira52microsoft-tutorial/tutorial_general_400.png)

6. V okně jiné webové prohlížeče Přihlaste se k vaší instanci JIRA jako správce.

7. Najeďte myší na ikonu a klikněte na tlačítko **doplňky**.

    ![Konfigurace jednotného přihlašování](./media/jira52microsoft-tutorial/addon1.png)

8. Karta části doplňků, klikněte na tlačítko **spravovat doplňky**.

    ![Konfigurace jednotného přihlašování](./media/jira52microsoft-tutorial/addon7.png)

9. Stáhněte si modul plug-in z [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=56521). Ručně nahrát modul plug-in poskytl pomocí Microsoft **nahrát doplněk** nabídky. Stáhnout modul plug-in se vztahuje smlouva [servisní smlouvou aplikace Microsoft](https://www.microsoft.com/servicesagreement/).

    ![Konfigurace jednotného přihlašování](./media/jira52microsoft-tutorial/addon12.png)

10. Když je nainstalovaný modul plug-in, zobrazí se v **uživatel nainstaloval** části doplňky. Klikněte na tlačítko **konfigurovat** konfigurace nového modulu plug-in.

    ![Konfigurace jednotného přihlašování](./media/jira52microsoft-tutorial/addon13.png)

11. Proveďte následující kroky na stránce konfigurace:

    ![Konfigurace jednotného přihlašování](./media/jira52microsoft-tutorial/addon52.png)

    > [!TIP]
    > Ujistěte se, že je pouze jeden certifikát mapují na aplikaci tak, aby se nezobrazí žádná chyba překladu metadat. Pokud existuje víc certifikátů, po vyřešení metadata a získá správce k chybě.

    a. V **adresa URL metadat** vložit do textového pole **adresa Url federačních metadat aplikace** hodnotu, která jste zkopírovali z webu Azure portal a klikněte **vyřešit** tlačítko. Načte adresu URL metadat zprostředkovatele identity a naplní všechny informace o pole.

    b. Kopírovat **identifikátor, adresa URL odpovědi a adresa URL přihlašování** hodnoty a vložit je do **identifikátor, adresa URL odpovědi a adresa URL přihlašování** textová pole v uvedeném pořadí **JIRA SAML SSO ve společnosti Microsoft (verze 5.2) domény a adresy URL**  části na webu Azure portal.

    c. V **přihlašovací jméno tlačítko** zadejte název tlačítka, které vaše organizace chce, aby se uživatelům zobrazí na obrazovce přihlášení.

    d. V **SAML uživatelské ID umístění** vyberte buď **ID uživatele není v elementu NameIdentifier příkazu subjektu** nebo **ID uživatele není v elementu atribut**.  Toto ID musí být id uživatele JIRA. Pokud neodpovídá id uživatele, systém nedovolí uživatelům přihlášení.

    > [!Note]
    > Výchozí umístění SAML ID uživatele je název identifikátoru. Můžete to změnit atribut možnost a zadejte odpovídající název.

    e. Pokud vyberete **ID uživatele není v elementu atribut** možnost, pak v **název atributu** textového pole zadejte název atributu, kde se očekává Id uživatele. 

    f. Pokud používáte federovanou doménu (například služby AD FS atd.) pomocí služby Azure AD, potom klikněte na **povolit zjišťování domovské sféry** řádku a nakonfigurovat **název domény**.

    g. V **název domény** zadejte název domény zde v případě přihlášení na základě služby AD FS.

    h. Zkontrolujte **povolit jednotné přihlašování,** Pokud se chcete odhlásit z Azure AD, když uživatel odhlásí ze systému JIRA. 

    i. Klikněte na tlačítko **Uložit** uložte nastavení tlačítkem.

    > [!NOTE]
    > Další informace o instalaci a řešení potíží s [příručky pro správce konektoru MS JIRA jednotného přihlašování](../ms-confluence-jira-plugin-adminguide.md) a k dispozici je také [nejčastější dotazy k](../ms-confluence-jira-plugin-faq.md) vaši pomoc

### <a name="create-an-azure-ad-test-user"></a>Vytvořit testovacího uživatele Azure AD

Cílem této části je vytvoření zkušebního uživatele na webu Azure Portal volá Britta Simon.

   ![Vytvořit testovacího uživatele Azure AD][100]

**Chcete-li vytvořit testovacího uživatele ve službě Azure AD, postupujte následovně:**

1. Na webu Azure Portal, v levém podokně klikněte na tlačítko **Azure Active Directory** tlačítko.

    ![Tlačítko Azure Active Directory](./media/jira52microsoft-tutorial/create_aaduser_01.png)

2. Chcete-li zobrazit seznam uživatelů, přejděte na **uživatelů a skupin**a potom klikněte na tlačítko **všichni uživatelé**.

    !["Uživatele a skupiny" a "Všechny uživatele" odkazy](./media/jira52microsoft-tutorial/create_aaduser_02.png)

3. Chcete-li otevřít **uživatele** dialogové okno, klikněte na tlačítko **přidat** v horní části **všichni uživatelé** dialogové okno.

    ![Tlačítko Přidat](./media/jira52microsoft-tutorial/create_aaduser_03.png)

4. V **uživatele** dialogové okno pole, proveďte následující kroky:

    ![Dialogové okno uživatele](./media/jira52microsoft-tutorial/create_aaduser_04.png)

    a. V **název** zadejte **BrittaSimon**.

    b. V **uživatelské jméno** zadejte e-mailovou adresu uživatele Britta Simon.

    c. Vyberte **zobrazit heslo** zaškrtněte políčko a zapište si hodnotu, která se zobrazí **heslo** pole.

    d. Klikněte na možnost **Vytvořit**.

### <a name="create-a-jira-saml-sso-by-microsoft-v52-test-user"></a>Vytvoření JIRA SAML SSO ve společnosti Microsoft (verze 5.2) testovacího uživatele

Povolit uživatele Azure AD pro přihlášení k systému JIRA na místním serveru, musí být poskytnuty do JIRA na místním serveru.

**K poskytnutí uživatelského účtu, postupujte následovně:**

1. Přihlaste se k serveru v místním systému JIRA jako správce.

2. Najeďte myší na ikonu a klikněte na tlačítko **Správa uživatelů**.

    ![Přidat zaměstnance](./media/jira52microsoft-tutorial/user1.png)

3. Budete přesměrováni na stránku přístup správce k zadání **heslo** a klikněte na tlačítko **potvrdit** tlačítko.

    ![Přidat zaměstnance](./media/jira52microsoft-tutorial/user2.png)

4. V části **Správa uživatelů** kartě oddíl, klikněte na tlačítko **vytvořit uživatele**.

    ![Přidat zaměstnance](./media/jira52microsoft-tutorial/user3.png) 

5. Na **"Vytvořit nový uživatel"** dialogového okna stránky, proveďte následující kroky:

    ![Přidat zaměstnance](./media/jira52microsoft-tutorial/user4.png)

    a. V **e-mailová adresa** , jako je textové pole, typ e-mailovou adresu uživatele Brittasimon@contoso.com.

    b. V **jméno a příjmení** textového pole zadejte celé jméno uživatele jako Britta Simon.

    c. V **uživatelské jméno** , jako je textové pole, typ e-mailu uživatele Brittasimon@contoso.com.

    d. V **heslo** textového pole zadejte heslo uživatele.

    e. Klikněte na tlačítko **vytvořit uživatele**.

### <a name="assign-the-azure-ad-test-user"></a>Přiřadit uživatele Azure AD

V této části je povolit Britta Simon používat jednotné přihlašování Azure díky udělení přístupu k systému JIRA SAML SSO Microsoft (V5.2).

![Přiřazení role uživatele][200]

**Pokud chcete přiřadit Britta Simon JIRA SAML SSO Microsoft (V5.2), proveďte následující kroky:**

1. Na webu Azure Portal, otevřete zobrazení aplikací a pak přejděte do zobrazení adresáře a přejděte na **podnikové aplikace** klikněte **všechny aplikace**.

    ![Přiřadit uživatele][201]

2. V seznamu aplikací vyberte **JIRA SAML SSO Microsoft (V5.2)**.

    ![Jednotné přihlašování SAML JIRA podle propojení Microsoft (verze 5.2) v seznamu aplikací](./media/jira52microsoft-tutorial/tutorial_singlesign-onforjira5.2_app.png)

3. V nabídce na levé straně klikněte na tlačítko **uživatelů a skupin**.

    ![Odkaz "Uživatele a skupiny"][202]

4. Klikněte na tlačítko **přidat** tlačítko. Potom vyberte **uživatelů a skupin** na **přidat přiřazení** dialogového okna.

    ![Podokno Přidat přiřazení][203]

5. Na **uživatelů a skupin** dialogového okna, vyberte **Britta Simon** v seznamu uživatelů.

6. Klikněte na tlačítko **vyberte** tlačítko **uživatelů a skupin** dialogového okna.

7. Klikněte na tlačítko **přiřadit** tlačítko **přidat přiřazení** dialogového okna.

### <a name="test-single-sign-on"></a>Test jednotného přihlašování

V této části Testování služby Azure AD jednotné přihlašování – konfigurace pomocí přístupového panelu.

Po kliknutí na JIRA SAML SSO ve společnosti Microsoft (verze 5.2) dlaždici na přístupovém panelu, vám by měl získat automaticky přihlášení k vaší JIRA SAML SSO aplikací společnosti Microsoft (verze 5.2).
Další informace o přístupovém panelu, naleznete v tématu [Úvod k přístupovému panelu](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Další zdroje informací:

* [Seznam kurzů o integraci aplikací SaaS pomocí Azure Active Directory](tutorial-list.md)
* [Jak ve službě Azure Active Directory probíhá přístup k aplikacím a jednotné přihlašování?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/msaadssojira5.2-tutorial/tutorial_general_01.png
[2]: ./media/msaadssojira5.2-tutorial/tutorial_general_02.png
[3]: ./media/msaadssojira5.2-tutorial/tutorial_general_03.png
[4]: ./media/msaadssojira5.2-tutorial/tutorial_general_04.png

[100]: ./media/msaadssojira5.2-tutorial/tutorial_general_100.png

[200]: ./media/msaadssojira5.2-tutorial/tutorial_general_200.png
[201]: ./media/msaadssojira5.2-tutorial/tutorial_general_201.png
[202]: ./media/msaadssojira5.2-tutorial/tutorial_general_202.png
[203]: ./media/msaadssojira5.2-tutorial/tutorial_general_203.png