---
title: Přizpůsobení uživatelského rozhraní pomocí vlastních zásad v Azure Active Directory B2C | Dokumentace Microsoftu
description: Další informace o přizpůsobení uživatelského rozhraní (UI), zatímco použití vlastních zásad v Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/04/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 9908a7cf96c56e414e0a8d7faea0352b60214ea4
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37446159"
---
# <a name="azure-active-directory-b2c-configure-ui-customization-in-a-custom-policy"></a>Azure Active Directory B2C: Konfigurace ve vlastních zásadách pro přizpůsobení uživatelského rozhraní

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Po dokončení tohoto článku, budete mít vlastní zásady registrace a přihlášení pomocí značky a vzhled. S Azure Active Directory B2C (Azure AD B2C), získáte téměř úplnou kontrolu nad obsah HTML a CSS, která se zobrazí uživatelům. Při použití vlastních zásad konfigurace přizpůsobení uživatelského rozhraní v XML namísto použití ovládacích prvků na webu Azure Portal. 

## <a name="prerequisites"></a>Požadavky

Než začnete, dokončete [Začínáme s vlastními zásadami](active-directory-b2c-get-started-custom.md). Měli byste mít funkční vlastní zásady pro registraci a přihlaste se pomocí místních účtů.

## <a name="page-ui-customization"></a>Přizpůsobení uživatelského rozhraní stránky

Pomocí stránky funkce přizpůsobení uživatelského rozhraní můžete přizpůsobit vzhled a chování všech vlastních zásad. Můžete také spravovat značky a vizuální konzistenci mezi vaší aplikací a službou Azure AD B2C.

Zde je, jak to funguje: Azure AD B2C kód v prohlížeči vašeho zákazníka a využívá moderní přístup a volá [sdílení prostředků mezi zdroji (CORS)](http://www.w3.org/TR/cors/). Nejprve zadejte adresu URL ve vlastních zásadách přizpůsobený obsah ve formátu HTML. Azure AD B2C sloučí elementy uživatelského rozhraní pomocí obsahu HTML, který je načten z vaší adresy URL a pak zobrazí na stránce zákazníkovi.

## <a name="create-your-html5-content"></a>Vytvoření obsahu vaší HTML5

Vytvoření obsahu s názvem vašeho produktu značky HTML v názvu.

1. Zkopírujte následující fragment kódu HTML. Je ve správném formátu HTML5 se prázdný element volá *\<div id = "rozhraní api"\>\</div\>* v rámci *\<tělo\>* značky. Tento prvek určuje, kde má být vložen obsah Azure AD B2C.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Product Brand Name</title>
   </head>
   <body>
       <div id="api"></div>
   </body>
   </html>
   ```

   >[!NOTE]
   >Z bezpečnostních důvodů použití jazyka JavaScript a aktuálně je blokováno pro přizpůsobení.

2. Vložte zkopírovaný fragment kódu v textovém editoru a uložte soubor jako *přizpůsobit ui.html*.

## <a name="create-an-azure-blob-storage-account"></a>Vytvoření účtu úložiště objektů Blob v Azure

>[!NOTE]
> V tomto článku používáme úložiště objektů Blob v Azure k hostování náš obsah. Je možné k hostování obsahu na webovém serveru, ale je potřeba [povolení CORS na vašem webovém serveru](https://enable-cors.org/server.html).

Chcete-li hostovat tento obsah ve formátu HTML v úložišti objektů Blob, postupujte takto:

1. Přihlaste se k webu [Azure Portal](https://portal.azure.com).
2. Na **centra** nabídce vyberte možnost **nový** > **úložiště** > **účtu úložiště**.
3. Zadejte jedinečný **název** pro váš účet úložiště.
4. **Model nasazení** může zůstat **Resource Manageru**.
5. Změna **druh účtu** k **úložiště objektů Blob**.
6. **Výkon** může zůstat **standardní**.
7. **Replikace** může zůstat **RA-GRS**.
8. **Úroveň přístupu** může zůstat **Hot**.
9. **Šifrování služby Storage** může zůstat **zakázané**.
10. Vyberte **předplatné** pro váš účet úložiště.
11. Vytvoření **skupiny prostředků** nebo vyberte existující.
12. Vyberte **zeměpisná poloha** pro váš účet úložiště.
13. Vytvořte účet úložiště kliknutím na **Vytvořit**.  
    Po dokončení nasazení **účtu úložiště** se automaticky otevře okno.

## <a name="create-a-container"></a>Vytvoření kontejneru

Vytvoření veřejného kontejneru v úložišti objektů Blob, postupujte takto:

1. Klikněte na tlačítko **přehled** kartu.
2. Klikněte na tlačítko **kontejneru**.
3. Pro **název**, typ **$root**.
4. Nastavte **získat přístup k typu** k **Blob**.
5. Klikněte na tlačítko **$root** otevřete nový kontejner.
6. Klikněte na **Odeslat**.
7. Klikněte na ikonu složky vedle **vyberte soubor**.
8. Přejděte na **přizpůsobit ui.html**, který jste vytvořili dříve v [přizpůsobení uživatelského rozhraní stránky](#the-page-ui-customization-feature) oddílu.
9. Klikněte na **Odeslat**.
10. Vyberte vlastní ui.html objekt blob, který jste nahráli.
11. Vedle položky **URL**, klikněte na tlačítko **kopírování**.
12. V prohlížeči vložte zkopírovanou adresu URL a přejděte na web. Pokud web není přístupný, ujistěte se, že typ přístupu kontejneru je nastaven na **blob**.

## <a name="configure-cors"></a>Konfigurace CORS

Konfigurace úložiště objektů Blob pro sdílení prostředků různého původu následujícím způsobem:

>[!NOTE]
>Chcete si vyzkoušet si funkce přizpůsobení uživatelského rozhraní pomocí našich ukázkový kód HTML a CSS obsah? Vytvořili jsme [jednoduché pomocným nástrojem](active-directory-b2c-reference-ui-customization-helper-tool.md) , která nahraje a nakonfiguruje náš ukázkový obsah na vašem účtu úložiště objektů Blob. Pokud používáte nástroj, přeskočte k části [upravit vlastní zásady registrace / přihlášení](#modify-your-sign-up-or-sign-in-custom-policy).

1. Na **úložiště** okně v části **nastavení**, otevřete **CORS**.
2. Klikněte na tlačítko **Add** (Přidat).
3. Pro **povolené zdroje**, zadejte hvězdičku (\*).
4. V **povolených operací** rozevíracího seznamu, vyberte **získat** a **možnosti**.
5. Pro **povolené hlavičky**, zadejte hvězdičku (\*).
6. Pro **zveřejněné hlavičky**, zadejte hvězdičku (\*).
7. Pro **maximální stáří (sekundy)**, typ **200**.
8. Klikněte na tlačítko **Add** (Přidat).

## <a name="test-cors"></a>Test CORS

Ověřte, že budete připraveni, následujícím způsobem:

1. Přejděte [www.test-cors.org](http://www.test-cors.org/) webu a pak vložte adresu URL v **adresa URL vzdáleného úložiště** pole.
2. Klikněte na tlačítko **poslat žádost o**.  
    Pokud se zobrazí chybová zpráva, ujistěte se, že vaše [nastavení CORS](#configure-cors) jsou správné. Může také muset vymazat mezipaměť prohlížeče nebo otevřete relaci procházení v privátní stisknutím kombinace kláves Ctrl + Shift + P.

## <a name="modify-your-sign-up-or-sign-in-custom-policy"></a>Upravit vlastní zásady registrace / přihlášení

V části na nejvyšší úrovni *\<TrustFrameworkPolicy\>* označit, měli byste najít *\<BuildingBlocks\>* značky. V rámci *\<BuildingBlocks\>* značky, přidejte *\<ContentDefinitions\>* značky tak, že zkopírujete následující příklad. Nahraďte *your_storage_account* s názvem účtu úložiště.

  ```xml
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.idpselections">
        <LoadUri>https://{your_storage_account}.blob.core.windows.net/customize-ui.html</LoadUri>
        <DataUri>urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0</DataUri>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>
  ```

## <a name="upload-your-updated-custom-policy"></a>Nahrání aktualizované vlastní zásady

1. V [webu Azure portal](https://portal.azure.com), [přepnutí do kontextu tenanta Azure AD B2C](active-directory-b2c-navigate-to-b2c-context.md)a pak otevřete **Azure AD B2C** okno.
2. Klikněte na tlačítko **všechny zásady**.
3. Klikněte na tlačítko **nahrát zásady**.
4. Nahrát `SignUpOrSignin.xml` s *\<ContentDefinitions\>* značku, kterou jste přidali dříve.

## <a name="test-the-custom-policy-by-using-run-now"></a>Testování s použitím vlastní zásady **spustit nyní**

1. Na **Azure AD B2C** okno, přejděte na **všechny zásady**.
2. Vyberte vlastní zásady, které jste nahráli a klikněte na tlačítko **spustit nyní** tlačítko.
3. Byste měli být schopni zaregistrujte s použitím e-mailovou adresu.

## <a name="reference"></a>Referenční informace

Pro přizpůsobení uživatelského rozhraní tady najdete ukázkové šablony:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Složka sample_templates/wingtip obsahuje následující soubory HTML:

| Šablona HTML5 | Popis |
|----------------|-------------|
| *phonefactor.html* | Tento soubor můžete použijte jako šablonu pro stránku služby Multi-Factor authentication. |
| *resetpassword.html* | Použít jako šablonu pro tento soubor stránka zapomenuté heslo. |
| *selfasserted.html* | Tento soubor můžete použijte jako šablonu pro stránka registrace sociálního účtu, stránku pro přihlášení místním účtem nebo místní účet přihlašovací stránky. |
| *unified.html* | Tento soubor můžete použijte jako šablonu pro jednotné stránku registrace nebo přihlášení. |
| *updateprofile.html* | Tento soubor můžete použijte jako šablonu pro stránku aktualizace profilu. |

V [upravit vaše vlastní zásady registrace / přihlášení oddíl](#modify-your-sign-up-or-sign-in-custom-policy), nakonfigurovat definici obsahu pro `api.idpselections`. Kompletní obsah ID definice, které jsou rozpoznány modulem pro architekturu rozhraní identit Azure AD B2C a jejich popisy jsou v následující tabulce:

| ID obsahu definice | Popis | 
|-----------------------|-------------|
| *api.error* | **Chybová stránka**. Na této stránce se zobrazí, když došlo k výjimce nebo došlo k chybě. |
| *API.idpselections* | **Stránka výběru zprostředkovatele identit**. Tato stránka obsahuje seznam zprostředkovatelů identity, které může uživatel vybrat při přihlašování. Tyto možnosti jsou organizace zprostředkovatelů identity, zprostředkovatelé sociálních identit, jako je Facebook nebo Google + nebo místním účtům. |
| *api.idpselections.signup* | **Výběr zprostředkovatele identity pro registraci**. Tato stránka obsahuje seznam zprostředkovatelů identity, které může uživatel vybírat během registrace. Tyto možnosti jsou organizace zprostředkovatelů identity, zprostředkovatelé sociálních identit, jako je Facebook nebo Google + nebo místním účtům. |
| *api.localaccountpasswordreset* | **Stránka zapomenuté heslo**. Tato stránka obsahuje formulář, který uživatel musí dokončit k zahájení resetování hesla.  |
| *API.localaccountsignin* | **Místní účet přihlašovací stránku**. Tato stránka obsahuje přihlašovací formulář pro přihlašování pomocí místní účet, který je založen na e-mailovou adresu nebo jméno uživatele. Formulář může obsahovat pole textového zadání a pole pro zadání hesla. |
| *API.localaccountsignup* | **Stránku pro přihlášení místním účtem**. Tato stránka obsahuje formuláři pro registraci registrace pro místní účet, který je založen na e-mailovou adresu nebo jméno uživatele. Formulář může obsahovat různé vstupní ovládací prvky, jako je například pole textového zadání, pole pro zadání hesla, přepínač, jedním výběrem rozevírací seznamy a vyberte zaškrtávací políčka. |
| *api.phonefactor* | **Stránka služby Multi-Factor authentication**. Na této stránce si uživatelé mohli ověřit jejich telefonní čísla (pomocí textových nebo hlasových) během registrace nebo přihlášení. |
| *api.selfasserted* | **Stránka registrace sociálního účtu**. Tato stránka obsahuje registrace formulář, který uživatelé musí dokončit při registraci pomocí existujícího účtu ze zprostředkovatele sociální identity, jako je Facebook nebo Google +. Tato stránka je podobný předchozí stránka registrace sociálního účtu, s výjimkou pole pro zadání hesla. |
| *api.selfasserted.profileupdate* | **Stránka pro aktualizaci profilu**. Tato stránka obsahuje formulář, který mohou uživatelé aktualizovat svůj profil. Tato stránka je podobná stránka registrace sociálního účtu s výjimkou pole pro zadání hesla. |
| *api.signuporsignin* | **Jednotná stránka registrace nebo přihlašování**. Tato stránka zpracovává registrace i přihlášení uživatelů, kteří můžou využívat enterprise zprostředkovatelů identity, zprostředkovatelé sociálních identit, jako je Facebook nebo Google + nebo místním účtům.  |

## <a name="next-steps"></a>Další postup

Další informace o prvcích uživatelského rozhraní, které lze přizpůsobit, najdete v části [referenční příručka pro přizpůsobení uživatelského rozhraní pro předdefinované zásady](active-directory-b2c-reference-ui-customization.md).
