---
title: Správa relací jednotné přihlašování pomocí vlastních zásad v Azure Active Directory B2C | Dokumentace Microsoftu
description: Zjistěte, jak spravovat relace jednotného přihlašování pomocí vlastních zásad v Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/20/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 351b48f2e2766b4974a5a41b5e95acfbd63dbfc9
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443218"
---
# <a name="azure-ad-b2c-single-sign-on-sso-session-management"></a>Azure AD B2C: Jednotné přihlašování (SSO) Správa relací

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure AD B2C umožňuje správcům řídit, jak Azure AD B2C komunikuje s konkrétním uživatelem po uživatel už provedl ověření. To se provádí prostřednictvím správy relace jednotného přihlašování. Správce může například řídit, určuje, zda je zobrazena škály zprostředkovatelů identity, nebo zda místní účet podrobnosti nutné znovu zadat. Tento článek popisuje, jak konfigurovat nastavení jednotného přihlašování pro Azure AD B2C.

## <a name="overview"></a>Přehled

Správa relací jednotného přihlašování má dvě části. První se zabývá interakce uživatele přímo s Azure AD B2C a jiné obchody s interakce uživatele s externími stranami, jako je Facebook. Azure AD B2C není přepsat nebo obejít relace jednotného přihlašování, které se můžou uchovávat externí strany. Místo toho směrovat přes Azure AD B2C se dostat k externí strany, která je "zapamatovaných", odpadá nutnost reprompt uživateli vybrat zprostředkovatele identity jejich podniku nebo sociální sítě. Ultimate rozhodnutí jednotného přihlašování zůstává třetí strana.

## <a name="how-does-it-work"></a>Jak to funguje?

Správa relací jednotného přihlašování používá stejnou sémantiku jako ostatní technický profil ve vlastních zásad. Pokud je spuštěn na krok Orchestrace, technický profil přidruženého k tomuto kroku se dotazují pro `UseTechnicalProfileForSessionManagement` odkaz. Pokud takové existuje, odkazovaná poskytovatel relace jednotného přihlašování se pak zkontroluje, aby zjistit, zda uživatel je účastník relace. Pokud tedy poskytovatel relace jednotného přihlašování se používá k znovu vytvořit relaci. Podobně po dokončení provádění na krok Orchestrace zprostředkovatele slouží k ukládání informací v relaci, pokud byl zadán poskytovatele relace jednotného přihlašování.

Azure AD B2C má definovaný počet zprostředkovatelé relace jednotného přihlašování, které je možné:

* NoopSSOSessionProvider
* DefaultSSOSessionProvider
* ExternalLoginSSOSessionProvider
* SamlSSOSessionProvider

Jednotné přihlašování správu třídy určeny pomocí `<UseTechnicalProfileForSessionManagement ReferenceId=“{ID}" />` element technického profilu.

### <a name="noopssosessionprovider"></a>NoopSSOSessionProvider

Jak určuje název tohoto zprostředkovatele nemá žádný účinek. Tento zprostředkovatel je možné pro potlačení chování jednotného přihlašování pro konkrétní technický profil.

### <a name="defaultssosessionprovider"></a>DefaultSSOSessionProvider

Tento zprostředkovatel je možné pro ukládání deklarací identity v relaci. Tento zprostředkovatel je obvykle odkazuje v technickém profilu, který používá ke správě místních účtů. 

> [!NOTE]
> Při použití DefaultSSOSessionProvider k uložení deklarací identity v relaci, je potřeba zajistit, že všechny deklarace, které je potřeba vrátit do aplikace nebo používají předběžné podmínky v dalších krocích, jsou uloženy v relaci nebo rozšířen o čtení z profilu uživatele v adresář. Tím se zajistí, že svou cestu ověřování nebudou na chybějící deklarací identity.

```XML
<TechnicalProfile Id="SM-AAD">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.DefaultSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="objectId" />
        <PersistedClaim ClaimTypeReferenceId="newUser" />
        <PersistedClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" />
    </PersistedClaims>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="objectIdFromSession" DefaultValue="true" />
    </OutputClaims>
</TechnicalProfile>
```

K přidávání deklarací identit v relaci, použijte `<PersistedClaims>` element technického profilu. Když poskytovatele umožňuje znovu vytvořit relaci, trvalé deklarace identity se nepřidají do kontejneru deklarací identity. `<OutputClaims>` slouží k načítání deklarace identity z relace.

### <a name="externalloginssosessionprovider"></a>ExternalLoginSSOSessionProvider

Tento zprostředkovatel umožňuje potlačit na obrazovce "Zvolte zprostředkovatele identity". Obvykle odkazuje v technickém profilu nakonfigurovaný pro externího zprostředkovatele identity, jako je Facebook. 

```XML
<TechnicalProfile Id="SM-SocialLogin">
    <DisplayName>Session Mananagement Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.ExternalLoginSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
</TechnicalProfile>
```

### <a name="samlssosessionprovider"></a>SamlSSOSessionProvider

Tento zprostředkovatel se používá pro správu Azure AD B2C SAML relací mezi aplikací, jakož i externí zprostředkovatele identity SAML.

```XML
<TechnicalProfile Id="SM-Reflector-SAML">
    <DisplayName>Session Management Provider</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.SSO.SamlSSOSessionProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
        <Item Key="IncludeSessionIndex">false</Item>
        <Item Key="RegisterServiceProviders">false</Item>
    </Metadata>
</TechnicalProfile>
```

Existují dvě položky metadat v technickém profilu:

| Položka | Výchozí hodnota | Možné hodnoty | Popis
| --- | --- | --- | --- |
| IncludeSessionIndex | true (pravda) | True nebo false | K poskytovateli označuje, že index relace by měla být uložena. |
| RegisterServiceProviders | true (pravda) | True nebo false | Označuje, že poskytovatel by měl registrace všech poskytovatelů služeb SAML, které byly vydány kontrolní výraz. |

Při použití zprostředkovatele pro ukládání relace SAML zprostředkovatele identity, výše uvedené položky by měl být false. Při použití zprostředkovatele pro ukládání relace B2C SAML, výše uvedené položky musí být true nebo není uveden jako výchozí hodnoty jsou true.

>[!NOTE]
> Odhlášení relace SAML vyžaduje `SessionIndex` a `NameID` dokončete.

## <a name="next-steps"></a>Další postup

Jsme rádi, názory a návrhy! Pokud máte případné potíže s tímto tématem, zveřejněte na webu Stack Overflow pomocí značky [azure-ad-b2c](https://stackoverflow.com/questions/tagged/azure-ad-b2c). Pro žádosti o funkce, Hlasujte pro ně v našich [fóru pro zpětnou vazbu](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

