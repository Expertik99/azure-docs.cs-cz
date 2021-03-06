---
title: Přizpůsobení jazyka v Azure Active Directory B2C | Dokumentace Microsoftu
description: Další informace o přizpůsobení prostředí jazyka.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: affd52352dcc745557dd66c61ccfa1e7a99dcdb7
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37442317"
---
# <a name="language-customization-in-azure-active-directory-b2c"></a>Přizpůsobení jazyka v Azure Active Directory B2C

Přizpůsobení jazyka v Azure Active Directory B2C (Azure AD B2C) umožňuje zásady tak, aby vyhovovaly různých jazyků tak, aby odpovídala vašim potřebám zákazníků.  Společnost Microsoft poskytuje překlady pro [36 jazyků](#supported-languages), ale můžete také poskytnout vlastní překlady pro libovolný jazyk. I v případě, že prostředí je k dispozici pro jeden jazyk, můžete přizpůsobit text na stránkách.  

## <a name="how-language-customization-works"></a>Jak funguje přizpůsobení jazyka
Pomocí přizpůsobení jazyka vyberte jazyky, které vaše cesta uživatele není k dispozici v. Po povolení funkce parametru řetězce dotazu, můžete zadat `ui_locales`, z vaší aplikace. Při volání do Azure AD B2C, vaše stránka je přeložen na národní prostředí, které jste určili. Tento typ konfigurace vám plnou kontrolu nad jazyky na vaší cestě uživatele a ignoruje nastavení jazyka prohlížeče zákazníka. 

Nemusíte tuto úroveň kontroly nad jaké jazyky se zobrazí vaše zákazníky. Pokud nezadáte `ui_locales` parametr, závisí uživatelské prostředí nastavení svého prohlížeče.  Můžete řídit jazyky, které převádějí na vaší cesty uživatele tak, že ho přidáte jako podporovaný jazyk. Pokud prohlížeč zákazníka je nastavena na Zobrazit jazyk, který nechcete podporu, zobrazí se místo toho jazyk, který jste vybrali jako výchozí v podporované jazykové verze.

- **Zadaný jazyk uživatelského rozhraní národní prostředí**: Po povolení přizpůsobení jazyka vaší cesty uživatele se přeloží na jazyk, který je zde uveden.
- **Prohlížeč požadovaný jazyk**: Pokud žádná `ui_locales` byl zadán parametr, vaše cesty uživatele se přeloží na jazyk prohlížeče požadovaný *Pokud daný jazyk podporován*.
- **Výchozí jazyk zásad**: Pokud v prohlížeči neurčuje jazyk nebo Určuje jednu, která není podporována, cesty uživatele se překládá na výchozí jazyk zásad.

>[!NOTE]
>Pokud používáte vlastní uživatelské atributy, budete muset zadat svoje vlastní překlady. Další informace najdete v tématu [přizpůsobení vašich řetězců](#customize-your-strings).
>

## <a name="support-requested-languages-for-uilocales"></a>Podpora požadované jazyky pro národní prostředí uživatelského rozhraní 
Zásady, které byly vytvořeny před všeobecnou dostupnost přizpůsobení jazyka je nutné nejprve povolit tuto funkci. Zásady, které byly vytvořeny po mají povolené ve výchozím nastavení přizpůsobení jazyka. 

Po povolení přizpůsobení jazyka v zásadách můžete nastavit jazyk cesty uživatele tak, že přidáte `ui_locales` parametru.
1. [Přejděte na stránku funkce B2C na webu Azure portal](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings).
2. Přejděte na zásady, které chcete povolit pro překlady.
3. Vyberte **přizpůsobení jazyka**.  
4. Vyberte **, povolte přizpůsobení jazyka**.
5. Přečtěte si informace v dialogovém okně a vyberte **Ano**.

## <a name="select-which-languages-in-your-user-journey-are-enabled"></a>Vyberte jazyky na vaší cestě uživatele jsou povolené. 
Povolit sadu jazyků pro vaši cestu uživatele mají být převedeny na požádání v prohlížeči bez `ui_locales` parametru.
1. Zajistěte, aby vaše zásady přizpůsobení jazyka povolená z předchozích kroků.
2. Z **upravit zásadu** stránce **přizpůsobení jazyka**.
3. Vyberte jazyk, který chcete podporovat.
4. V podokně vlastností změnit **povoleno** k **Ano**.  
5. Vyberte **Uložit** v horní části podokna Vlastnosti.

>[!NOTE]
>Pokud `ui_locales` parametr není zadán, stránky se přeloží na jazyk prohlížeče zákazníka pouze v případě, že je povolená.
>

## <a name="customize-your-strings"></a>Přizpůsobení vašich řetězců
Přizpůsobení jazyka umožňuje přizpůsobit libovolný řetězec na vaší cestě uživatele.
1. Zajistěte, aby vaše zásady přizpůsobení jazyka povolená z předchozích kroků.
2. Z **upravit zásadu** stránce **přizpůsobení jazyka**.
3. Vyberte jazyk, který chcete přizpůsobit.
4. Vyberte stránku, kterou chcete upravit.
5. Vyberte **výchozí hodnoty stahování** (nebo **přepisy stahování** Pokud jste upravili dříve tento jazyk). 

Tyto kroky vám soubor JSON, který vám pomůže začít upravovat vaše řetězce.

### <a name="change-any-string-on-the-page"></a>Změnit na stránce libovolný řetězec
1. Otevřete soubor JSON stažené z předchozích kroků v editoru JSON.
2. Najděte prvek, který chcete změnit.  Můžete najít `StringId` pro řetězec, který hledáte, nebo vyhledejte `Value` atribut, který chcete změnit.
3. Aktualizace `Value` atribut s co chcete zobrazit.
4. Pro každý řetězec, který chcete změnit, změňte `Override` k `true`.
5. Soubor uložte a odešlete své změny. (Ovládací prvek pro uložení můžete najít na stejném místě jako, které jste stáhli soubor JSON.) 

>[!IMPORTANT]
>Pokud je potřeba přepsat řetězec, nezapomeňte nastavit `Override` hodnota, která se `true`.  Pokud hodnota se nezmění, tato položka je ignorována. 
>

### <a name="change-extension-attributes"></a>Změnit atributy rozšíření
Pokud chcete změnit řetězec pro vlastní uživatelský atribut, nebo chcete přidejte jej do formátu JSON, je v následujícím formátu:
```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": true,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```

Nahraďte `<ExtensionAttribute>` s názvem atributu vlastní uživatele.  

Nahraďte `<ExtensionAttributeValue>` s nový řetězec, který se má zobrazit.

### <a name="provide-a-list-of-values-by-using-localizedcollections"></a>Zadejte seznam hodnot pomocí LocalizedCollections
Pokud chcete zadat seznam hodnot pro odpovědi, je potřeba vytvořit `LocalizedCollections` atribut.  `LocalizedCollections` je pole `Name` a `Value` dvojice. Pořadí položek bude pořadí, ve kterém jsou zobrazeny.  Chcete-li přidat `LocalizedCollections`, použijte následující formát:

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType", 
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": true,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```

* `ElementId` je, že tento atribut uživatele `LocalizedCollections` atribut je odpověď.
* `Name` je hodnota, která se zobrazí uživateli.
* `Value` je, co bude vráceno v deklaraci identity, pokud je vybraná tato možnost.

### <a name="upload-your-changes"></a>Odeslat změny
1. Po dokončení změny do souboru JSON, vraťte se na svého klienta B2C.
2. Z **upravit zásadu** stránce **přizpůsobení jazyka**.
3. Vyberte jazyk, ve kterém chcete přeložit na.
4. Vyberte stránku, kde chcete poskytnout překlady.
5. Vyberte ikonu složky a vyberte soubor JSON má nahrát.
 
Změny se automaticky uloží do zásady.

## <a name="customize-the-page-ui-by-using-language-customization"></a>Přizpůsobit stránce uživatelského rozhraní pomocí přizpůsobení jazyka

Existují dva způsoby, jak lokalizovat váš obsah ve formátu HTML. Jedním ze způsobů je zapnout [přizpůsobení jazyka](active-directory-b2c-reference-language-customization.md). Když tuto funkci povolí, Azure AD B2C k předávání parametru Open ID Connect `ui-locales`, do koncového bodu.  Vaše servery obsahu můžete použít tento parametr k poskytování přizpůsobených stránky HTML, které jsou specifické pro konkrétní jazyk.

Alternativně si můžete vyžádat obsah z různých míst na základě národního prostředí, který se používá. Váš koncový bod s povoleným CORS můžete nastavit strukturu složek jako hostitele obsahu pro konkrétní jazyky. Ten správný budete volat, pokud použijete zástupné hodnoty `{Culture:RFC5646}`.  Předpokládejme například, že se jedná o identifikátor URI pro vlastní stránky:

```
https://wingtiptoysb2c.blob.core.windows.net/{Culture:RFC5646}/wingtip/unified.html
```
Můžete načíst stránku v `fr`. Pokud na stránce si vyžádá obsah HTML a CSS, je přebírání z:
```
https://wingtiptoysb2c.blob.core.windows.net/fr/wingtip/unified.html
```

## <a name="add-custom-languages"></a>Přidat vlastní jazyky

Můžete také přidat jazyky, které společnost Microsoft aktuálně neposkytuje překlady. Bude potřeba poskytnout překlad pro všechny řetězce v zásadách.  Kódy jazyce a národním prostředí jsou omezené na ty ve standardu ISO 639-1. 

1. Z **upravit zásadu** stránce **přizpůsobení jazyka**.
2. Vyberte **přidat vlastní jazyk** z horní části stránky.
3. V kontextovém podokně, které se otevře Identifikujte jazyk, který zadáváte překlady pro tak, že zadáte kód platný národního prostředí.
4. Pro každou stránku můžete stáhnout sadu přepsání pro angličtinu a pracovat na překlady.
5. Až budete hotovi s soubory JSON, můžete je načíst pro každou stránku.
6. Vyberte **povolit**, a zásady teď můžete zobrazit tento jazyk pro vaše uživatele.
7. Uložte jazyk.

>[!IMPORTANT]
>Budete muset povolit vlastní jazyky nebo nahrát přepisy pro něj před uložením.
>

## <a name="additional-information"></a>Další informace

### <a name="page-ui-customization-labels-as-overrides"></a>Přizpůsobení popisků uživatelského rozhraní stránky jako přepsání
Po povolení přizpůsobení jazyka jsou trvalé předchozích úprav pro popisky pomocí přizpůsobení uživatelského rozhraní stránky v souboru JSON pro angličtinu (cs). Chcete-li změnit popisky a jiných řetězců tak, že nahrajete prostředků jazyka v přizpůsobení jazyka můžete pokračovat.
### <a name="up-to-date-translations"></a>Aktuální překlady
Společnost Microsoft se zavazuje poskytovat nejaktuálnější překlady pro použití. Microsoft neustále zlepšuje překlady a zajišťuje jejich dodržování předpisů pro vás. Společnost Microsoft bude identifikovat chyby a změny v globálních terminologie a zpřístupněte aktualizace, které budou fungovat bez problémů na vaší cestě uživatele.
### <a name="support-for-right-to-left-languages"></a>Podpora jazyků zprava doleva
Microsoft aktuálně neposkytuje podporu pro jazyky zprava doleva. Můžete to provést pomocí vlastní národní prostředí a pomocí šablon stylů CSS změnit způsob, jakým jsou zobrazeny řetězce.  Pokud potřebujete tuto funkci, prosím Hlasujte pro ni [zpětné vazby Azure](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Překlady zprostředkovatele sociálních identit
Společnost Microsoft poskytuje `ui_locales` OIDC parametr přihlašování přes sociální sítě. Ale někteří poskytovatelé sociálních identit, včetně Facebooku nebo Googlu, není případném dalším sdílení dodržovat je. 
### <a name="browser-behavior"></a>Chování prohlížeče
Chrome a Firefox, které obě žádosti pro své jazykové sady. Pokud je podporovaný jazyk, zobrazí se před výchozí. Edge aktuálně neuvede v požadavku jazyk a přejde přímo na výchozí jazyk.

### <a name="supported-languages"></a>Podporované jazyky

| Jazyk              | Kód jazyka |
|-----------------------|---------------|
| Bengálština                | Bn            |
| Čeština                 | cs            |
| dánština                | da            |
| Němčina                | de            |
| Řečtina                 | el            |
| Angličtina               | en            |
| Španělština               | es            |
| Finština               | fi            |
| Francouzština                | fr            |
| Gudžarátština              | gu            |
| Hindština                 | Ahoj            |
| Chorvatština              | h            |
| Maďarština             | hu            |
| italština               | it            |
| Japonština              | ja            |
| Kannadština               | KN            |
| Korejština                | ko            |
| Malajálamština             | ml            |
| Maráthština               | MR            |
| Malajština                 | ms            |
| Norština (Nynorsk)      | nb            |
| Holandština                 | nl            |
| Paňdžábština               | pa            |
| polština                | pl            |
| Portugalština – Brazílie   | pt-br         |
| Portugalština – Portugalsko | pt-pt         |
| Rumunština              | ro            |
| ruština               | ru            |
| Slovenština                | Sk            |
| švédština               | sv            |
| Tamilština                 | ta            |
| Telugština                | te            |
| Thajština                  | .            |
| turečtina               | tr            |
| Čínština (zjednodušená)  | zh-hans       |
| Čínština (tradiční) | zh-hant       |
