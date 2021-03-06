---
title: S použitím AppAuth do aplikace pro iOS v Azure Active Directory B2C | Dokumentace Microsoftu
description: V tomto článku se dozvíte, jak vytvořit aplikaci pro iOS, která používá AppAuth s Azure Active Directory B2C ke správě identit uživatelů a ověřování uživatelů.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/07/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 5f95b71497b59eafff09d4add2b4bb1c20656592
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/31/2018
ms.locfileid: "43339354"
---
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a>Azure AD B2C: Aplikace pro iOS pomocí přihlášení

Platforma Microsoft identity používá otevřené standardy, jako je například OAuth2 nebo OpenID Connect. Pomocí otevřeného protokolu nabízí více možností pro vývojáře, při výběru knihovnu integrovat s našimi službami. Líbí se na podporu vývojářů při psaní aplikace, které se připojují k platformě Microsoft Identity poskytujeme Tento názorný postup a další. Většinu knihoven, které implementují [specifikace RFC6749 oauth2](https://tools.ietf.org/html/rfc6749) se připojit k platformě Microsoft Identity.

> [!WARNING]
> Společnost Microsoft neposkytuje oprav pro knihovny třetích stran a přezkoumání těchto knihoven, jako jednička. Tato ukázka používá knihovnu třetí strany s názvem AppAuth, který byl testován pro kompatibilitu s Azure AD B2C v základní scénáře. Problémy a žádosti o funkce směrovat do projektu open source knihovny. Další informace najdete v [tomto článku](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).
>
>

Pokud jste nepracovali s OAuth2 nebo OpenID Connect, velkou část tahle vzorová konfigurace možná moc smysl pro vás. Doporučujeme prohlédnout si stručný [přehled protokolu, který uvádíme tady](active-directory-b2c-reference-protocols.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Získání adresáře služby Azure AD B2C
Před použitím Azure AD B2C musíte vytvořit adresář, nebo klienta. Adresář je kontejner pro všechny uživatele, aplikace, skupiny a další. Pokud ho ještě nemáte, [vytvořte adresář B2C](active-directory-b2c-get-started.md) předtím, než budete pokračovat.

## <a name="create-an-application"></a>Vytvoření aplikace
Dále musíte vytvořit aplikaci v adresáři B2C. Registrace aplikací poskytuje Azure AD informace potřebné k bezpečné komunikaci s vaší aplikací. Při vytváření mobilních aplikací, postupujte podle [tyto pokyny](active-directory-b2c-app-registration.md). Ujistěte se, že:

* Zahrnout **nativního klienta** v aplikaci.
* Poznamenejte si **ID aplikace** přiřazené vaší aplikaci. Budete potřebovat později tento identifikátor GUID.
* Nastavení **identifikátor URI pro přesměrování** s vlastním schématem (například com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect). Budete později potřebovat pomocí tohoto identifikátoru URI.

## <a name="create-your-policies"></a>Vytvořte svoje zásady
V Azure AD B2C je každé uživatelské rozhraní definováno [zásadou](active-directory-b2c-reference-policies.md). Tato aplikace obsahuje možnosti pro jednu identitu: kombinované přihlášení a registraci. Vytvořte tuto zásadu, jak je popsáno v [článku o zásadách](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). Při vytváření zásady nezapomeňte na následující:

* V části **atributy registrace**, vyberte atribut **zobrazovaný název**.  Můžete vybrat i jiné atributy.
* V části **deklarace identit aplikace**, vyberte deklarace identity **zobrazovaný název** a **ID objektu uživatele**. Můžete vybrat i další deklarace identity.
* Po vytvoření každé zásady si poznamenejte její **Název**. Je název vaší zásady předponu `b2c_1_` při ukládání zásad.  Název zásady budete potřebovat později.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Po vytvoření zásad jste připraveni k sestavení aplikace.

## <a name="download-the-sample-code"></a>Stáhněte si ukázkový kód
Poskytujeme ukázku práci, která používá AppAuth s Azure AD B2C [na Githubu](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Můžete stáhnout kód a spustíme ji. Použití vašeho vlastního tenanta Azure AD B2C, postupujte podle pokynů [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).

Tato ukázka byla vytvořena podle následujících pokynů README podle [iOS AppAuth projektu na Githubu](https://github.com/openid/AppAuth-iOS). Podrobné informace o fungování vzorku a knihovny odkaz README AppAuth na Githubu.

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a>Úprava aplikace pro použití Azure AD B2C s AppAuth

> [!NOTE]
> AppAuth podporuje iOS 7 a vyšší.  Ale pro podporu přihlašování přes sociální sítě na Googlu, je potřeba SFSafariViewController vyžadujícího iOS 9 nebo vyšší.
>

### <a name="configuration"></a>Konfigurace

Je-li nakonfigurovat komunikaci s Azure AD B2C, zadáte koncový bod autorizace a koncový bod tokenu identifikátory URI.  Ke generování těchto identifikátorů URI, budete potřebovat následující informace:
* ID tenanta (například contoso.onmicrosoft.com).
* Název zásad (například B2C\_1\_SignUpIn)

Koncový bod tokenu identifikátor URI je vygenerovat tak, že nahradíte Tenanta\_ID a zásad\_názvu v následující adrese URL:

```objc
static NSString *const tokenEndpoint = @"https://<Tenant_name>.b2clogin.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

Koncový bod autorizace identifikátoru URI je vygenerovat tak, že nahradíte Tenanta\_ID a zásad\_názvu v následující adrese URL:

```objc
static NSString *const authorizationEndpoint = @"https://<Tenant_name>.b2clogin.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

Spusťte následující kód k vytvoření objektu AuthorizationServiceConfiguration:

```objc
OIDServiceConfiguration *configuration = 
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready to perform the auth request...
```

### <a name="authorizing"></a>Probíhá autorizace.

Po konfiguraci nebo načítání konfigurace povolení služby, lze sestavit žádost o autorizaci. Pokud chcete vytvořit žádost, budete potřebovat následující informace:  
* ID klienta (například 00000000-0000-0000-0000-000000000000)
* Identifikátor URI přesměrování s vlastním schématem (například com.onmicrosoft.fabrikamb2c.exampleapp://oauth/redirect)

Obě položky by se uložily. Pokud byste byli [zaregistrujete aplikaci](#create-an-application).

```objc
OIDAuthorizationRequest *request = 
    [[OIDAuthorizationRequest alloc] initWithConfiguration:configuration
                                                  clientId:kClientId
                                                    scopes:@[OIDScopeOpenID, OIDScopeProfile]
                                               redirectURL:[NSURL URLWithString:kRedirectUri]
                                              responseType:OIDResponseTypeCode
                                      additionalParameters:nil];

AppDelegate *appDelegate = (AppDelegate *)[UIApplication sharedApplication].delegate;
appDelegate.currentAuthorizationFlow = 
    [OIDAuthState authStateByPresentingAuthorizationRequest:request
                                   presentingViewController:self
                                                   callback:^(OIDAuthState *_Nullable authState, NSError *_Nullable error) {
        if (authState) {
            NSLog(@"Got authorization tokens. Access token: %@", authState.lastTokenResponse.accessToken);
            [self setAuthState:authState];
        } else {
            NSLog(@"Authorization error: %@", [error localizedDescription]);
            [self setAuthState:nil];
        }
    }];
```

K nastavení aplikace pro zpracování přesměrování na identifikátor URI s vlastním schématem, je potřeba aktualizovat seznam "schémata adres URL v souboru Info.pList:
* Otevřete soubor Info.pList.
* Řádek kódu sady prostředků operačního systému typu ukazatel myši a klikněte na tlačítko \+ symbol.
* Přejmenujte na novém řádku "Typy adres URL".
* Klikněte na šipku nalevo od "typy adres URL' Chcete-li otevřít stromu.
* Klikněte na šipku nalevo od "položky 0' otevřete stromu.
* Přejmenujte první položky pod položku 0 "Schémata URL".
* Klikněte na šipku nalevo od adresy URL schémata otevřete stromu.
* Ve sloupci 'Value' je prázdné pole nalevo od "Položky 0" pod "Schémata URL".  Nastavte hodnotu na jedinečné schéma vaší aplikace.  Hodnota musí odpovídat používané v redirectURL při vytvoření objektu OIDAuthorizationRequest schéma.  V ukázce se používá schéma "com.onmicrosoft.fabrikamb2c.exampleapp".

Odkazovat [AppAuth průvodce](https://openid.github.io/AppAuth-iOS/) o tom, k dokončení procesu. Pokud chcete rychle začít s funkční aplikaci, přečtěte si [vzorku](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Postupujte podle pokynů [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) zadat konfiguraci Azure AD B2C.

Máme vždy otevřít na názory a návrhy! Pokud nemají nějaké problémy s tímto článkem, nebo máte doporučení k vylepšení tohoto obsahu, uvítáme vaše zpětná vazba v dolní části stránky. Pro žádosti o funkce, přidejte je do [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
