---
title: Zabezpečené jednostránkové aplikace pomocí Azure AD v2.0 implicitním tokem | Dokumentace Microsoftu
description: Vytváření webových aplikací pomocí služby Azure AD v2.0 provádění implicitní tok pro jednostránkové aplikace.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 3605931f-dc24-4910-bb50-5375defec6a8
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/22/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 7954a4eebaf25e2a22e5a39721098ac8db1ed8dd
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/06/2018
ms.locfileid: "39581205"
---
# <a name="v20-protocols---spas-using-the-implicit-flow"></a>verze 2.0 protokolů – SPA pomocí implicitní tok
S koncovým bodem v2.0 se můžete přihlásit uživatele do vaší jednostránkové aplikace s osobní i pracovní nebo školní účty od Microsoftu. Jediné stránce a dalších aplikací jazyka JavaScript, které poběží především v prohlížeči plochy pár zajímavých vyzve při rozhodování o ověřování:

* Vlastnosti zabezpečení těchto aplikací se značně liší od tradiční serverových webových aplikací.
* Mnoho autorizace servery a zprostředkovatelů identity požadavků CORS nepodporují.
* Od aplikace stát zejména invazivní uživatelské prostředí pro přesměrování prohlížeče celou stránku.

Pro tyto aplikace (myslíte: AngularJS, Ember.js, React.js, etc) Azure AD podporuje toku implicitní Grant OAuth 2.0. Implicitní tok je popsána v [specifikaci OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-4.2). Jeho primární výhodou je, že umožňuje aplikaci získat tokeny ze služby Azure AD bez provedení back-end serveru výměnou přihlašovacích údajů. To umožňuje aplikaci pro uživatele, Udržovat relaci a získat tokeny do dalších webových rozhraní API vše v rámci klienta kódu jazyka JavaScript. Existuje několik důležité informace o zabezpečení vzít v úvahu při použití implicitní tok – konkrétně přibližně [klienta](http://tools.ietf.org/html/rfc6749#section-10.3) a [zosobnění uživatele](http://tools.ietf.org/html/rfc6749#section-10.3).

Pokud chcete použít implicitní tok a Azure AD k přidání ověřování do aplikace jazyka JavaScript, doporučujeme použít naše open source knihovna jazyka JavaScript, [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js). Nejsou k dispozici několik AngularJS kurzy [tady](active-directory-appmodel-v2-overview.md#getting-started) k vám pomůžou začít. 

Ale pokud chcete raději použít knihovnu v jednostránkové aplikaci a pošlete sami sobě zprávy protokolu, postupujte podle obecných kroků.

> [!NOTE]
> Ne všechny scénáře Azure Active Directory a funkce jsou podporovány bodem v2.0. Pokud chcete zjistit, pokud je vhodné použít koncový bod verze 2.0, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
> 
> 

## <a name="protocol-diagram"></a>Diagram protokolu
Celý implicitní přihlášení tok by měl vypadat asi takto – všechny kroky jsou podrobně popsány v níže.

![OpenId Connect plaveckých drah](./media/v2-oauth2-implicit-grant-flow/convergence_scenarios_implicit.png)

## <a name="send-the-sign-in-request"></a>Odeslat žádost o přihlášení
Pro počáteční přihlášení uživatele do vaší aplikace, můžete odeslat [OpenID Connect](v2-protocols-oidc.md) žádost o autorizaci a získat `id_token` z koncového bodu v2.0:

> [!IMPORTANT]
> Aby úspěšně požádat o token ID registrace aplikace v [portál pro registraci](https://apps.dev.microsoft.com) musí mít **[implicitní grant](v2-oauth2-implicit-grant-flow.md)** pro webového klienta povolena. Pokud není povolen, `unsupported_response` se vrátí Chyba: "pro vstupní parametr 'typ odpovědi' zadaná hodnota není povolena pro tohoto klienta. Očekávaná hodnota je "kód" "

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [!TIP]
> Klikněte na odkaz níže k provedení tohoto požadavku. Po přihlášení by měl prohlížeč přesměrován na `https://localhost/myapp/` s `id_token` do adresního řádku.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

| Parametr |  | Popis |
| --- | --- | --- |
| tenant |povinné |`{tenant}` Hodnota v cestě požadavku je možné řídit, kdo se můžete přihlásit do aplikace. Povolené hodnoty jsou `common`, `organizations`, `consumers`a identifikátorů klienta. Další podrobnosti najdete v části [protokol Základy](active-directory-v2-protocols.md#endpoints). |
| client_id |povinné |Id aplikace, která na portál pro registraci ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) přiřazené vaší aplikaci. |
| response_type |povinné |Musí zahrnovat `id_token` pro přihlášení OpenID Connect. Může také zahrnovat typ odpovědi `token`. Pomocí `token` tady vám umožní vaši aplikaci pro příjem přístupový token z koncového bodu authorize okamžitě bez nutnosti provádět druhou žádost do koncového bodu authorize. Pokud používáte `token` typ odpovědi, `scope` parametr musí obsahovat obor určující, který prostředek se má token vydat. |
| redirect_uri |Doporučené |Redirect_uri vaší aplikace, kde můžete odesílat a přijímat aplikací pro žádosti o ověření. Musí odpovídat přesně jeden z redirect_uris, které jste zaregistrovali na portálu, s tím rozdílem, musí být kódování url. |
| scope |povinné |Seznam oborů oddělených mezerami. Pro OpenID Connect, musí zahrnovat obor `openid`, který se přeloží na "Přihlášení" oprávnění v souhlasu uživatelského rozhraní. Volitelně můžete také chtít zahrnout `email` nebo `profile` [obory](v2-permissions-and-consent.md) pro získání přístupu k datům uživatele. V této žádosti pro vyžádání souhlasu k různým prostředkům mohou zahrnovat také další obory. |
| response_mode |Doporučené |Určuje metodu, která se má použít k odeslání výsledný token zpátky do vaší aplikace. By měl být `fragment` pro implicitní tok. |
| state |Doporučené |Hodnota v požadavku, která se také vrátit v odpovědi tokenu. Může být řetězec jakéhokoli obsahu, který chcete. Náhodně generované jedinečná hodnota se obvykle používá pro [prevence útoků proti padělání žádosti více webů](http://tools.ietf.org/html/rfc6749#section-10.12). Stav se také používá ke kódování informace o stavu uživatele v aplikaci předtím, než požadavek na ověření došlo k chybě, například stránky nebo zobrazení, které byly na. |
| Hodnota Nonce |povinné |Hodnota v požadavku, generovaný aplikací, který bude obsahovat výsledný id_token jako deklarace identity. Aplikace pak můžete ověřit tuto hodnotu a zmírnění útoků opětovného přehrání tokenu. Hodnota je obvykle náhodnou jedinečného řetězce, který můžete použít k identifikaci původcem požadavku. |
| řádek |nepovinné |Určuje typ interakce s uživatelem, který je požadován. Jedinými platnými hodnotami v tuto chvíli jsou "přihlášení", "žádný" a "souhlas". `prompt=login` Vynutí uživatele k zadání přihlašovacích údajů tohoto požadavku negace jednotného přihlašování. `prompt=none` je opakem – zajistí, že uživatel se nezobrazí se žádné interaktivní výzvu jakýmkoli způsobem. Žádost nejde dokončit tiše prostřednictvím jednotného přihlašování, koncový bod verze 2.0 vrátí chybu. `prompt=consent` Dialogové okno souhlasu OAuth se aktivuje, až se uživatel přihlásí, s výzvou uživateli udělit oprávnění k aplikaci. |
| login_hint |nepovinné |Umožňuje předem vyplnit pole uživatelské jméno nebo e-mailová adresa přihlašovací stránka pro uživatele, pokud znáte svoje uživatelské jméno předem. Často aplikace bude používat tento parametr během opětovné ověření uživatelského jména s již extrahovat z předchozí přihlášení pomocí `preferred_username` deklarací identity. |
| domain_hint |nepovinné |Může být jedna z `consumers` nebo `organizations`. Pokud zahrnutý, budou přeskočeny procesu zjišťování na základě e-mailu tento uživatel prochází na v2.0 přihlašovací stránku, což vede k poněkud jednodušší činnost koncového uživatele. Často aplikace bude používat tento parametr během opětovné ověření extrahováním `tid` deklarace identity z požadavku id_token. Pokud `tid` deklarace identity, je hodnota `9188040d-6c67-4c5b-b112-36a304b66dad` (Account Microsoft tenanta příjemce), měli byste použít `domain_hint=consumers`. Jinak použijte `domain_hint=organizations`. |


V tomto okamžiku uživatele vyzve k zadání přihlašovacích údajů a bylo možné ověření dokončit. Koncový bod v2.0 také pomohou zajistit, aby uživatel vyjádřil souhlas se oprávnění uvedená v `scope` parametr dotazu. Pokud uživatele. nevyjádřil souhlas některý z těchto oprávnění, požádá o souhlas požadovaná oprávnění. Podrobnosti o [zde uvedené aplikace s více tenanty, oprávnění a souhlas](v2-permissions-and-consent.md).

Jakmile se uživatel ověří a udělí svůj souhlas, koncový bod verze 2.0 vrátí odpověď na vaši aplikaci na označený `redirect_uri`, pomocí metody popsané v `response_mode` parametru.

#### <a name="successful-response"></a>Úspěšné odpovědi
Úspěšné odpovědi pomocí `response_mode=fragment` a `response_type=id_token+token` vypadá podobně jako následující okno, kde konců řádků pro čitelnost:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parametr | Popis |
| --- | --- |
| access_token |Pokud zahrnutý `response_type` zahrnuje `token`. Přístupový token, který aplikace žádá, v tomto případě pro Microsoft Graph. Přístupový token by neměly být dekódovat nebo jinak zkontroloval, lze považovat za neprůhledný řetězec. |
| token_type |Pokud zahrnutý `response_type` zahrnuje `token`. Bude vždy `Bearer`. |
| expires_in |Pokud zahrnutý `response_type` zahrnuje `token`. Určuje počet sekund, po které je token platný, pro účely ukládání do mezipaměti. |
| scope |Pokud zahrnutý `response_type` zahrnuje `token`. Označuje počet rozsahů:, pro které bude platit access_token. |
| id_token |Id_token požadovanou aplikaci. Požadavku id_token slouží k ověření identity uživatele a zahájit relaci s uživatelem. Další podrobnosti o id_tokens a jejich obsah je součástí [token reference koncového bodu v2.0](v2-id-and-access-tokens.md). |
| state |Pokud parametr stavu je zahrnutý v požadavku, by se zobrazit stejnou hodnotu v odpovědi. Aplikace by měl ověřit, že jsou identické hodnoty stavu v požadavku a odpovědi. |

#### <a name="error-response"></a>Odpověď na chybu
Chybové odpovědi může také být odeslán `redirect_uri` tak aplikaci můžete odpovídajícím způsobem zpracovat:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, která slouží ke klasifikaci typy chyb, ke kterým dochází a je možné reagovat na chyby. |
| error_description |Určité chybové zprávě, který vám pomůže vývojář zjistit původní příčinu chyby ověřování. |

## <a name="validate-the-idtoken"></a>Ověření požadavku id_token
Pouhého získání tokentu id_token není dostatečná k ověření uživatele. je nutné ověřit podpis požadavku id_token a ověřte, deklarace identity v tokenu podle požadavků vaší aplikace. Koncový bod verze 2.0 používá [webové tokeny JSON (Jwt)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) a kryptografii využívající veřejného klíče pro podepisování tokenů a ověřte, že jsou platné.

Můžete také ověřit `id_token` v klientovi kód, ale běžnou praxí je odeslat `id_token` back-end server a provést ověření existuje. Jakmile ověříte podpisu požadavku id_token, existují několik deklarací identity, které budete muset ověřit. Najdete v článku [odkaz tokenu v2.0](v2-id-and-access-tokens.md) Další informace, včetně [ověřování tokenů](v2-id-and-access-tokens.md#validating-tokens) a [důležité informace o podpisový klíč výměny](v2-id-and-access-tokens.md#validating-tokens). Doporučujeme, abyste využívající knihovnu k analýze a ověřování tokenů: k dispozici aspoň jeden k dispozici pro většinu jazyky a platformy.
<!--TODO: Improve the information on this-->

Také můžete chtít ověřit další deklarace identity v závislosti na vašem scénáři. Některé běžné ověření patří:

* Zajištění uživatele nebo organizaci zaregistroval k aplikaci.
* Zajistit, že uživatel má správnou autorizaci/oprávnění
* Zajištění sílu ověřování došlo, jako je ověřování službou Multi-Factor Authentication.

Další informace o deklarace identity v tokentu id_token, najdete v článku [token reference koncového bodu v2.0](v2-id-and-access-tokens.md).

Jakmile ověříte zcela požadavku id_token, můžete zahájit relaci s uživatelem a používat deklarace identity v požadavku id_token k získání informací o uživateli ve vaší aplikaci. Tyto informace můžete použít pro zobrazení záznamů, autorizace, atd.

## <a name="get-access-tokens"></a>Získání přístupových tokenů
Teď, když uživatel přihlásíte vaši jednostránkovou aplikaci, můžete získat přístupové tokeny pro volání webových rozhraní API zabezpečené pomocí Azure AD, jako [Microsoft Graphu](https://graph.microsoft.io). I v případě, že jste už dostali tokenu pomocí `token` typ odpovědi, tuto metodu můžete použít k získání tokenů na další zdroje bez nutnosti přesměrovat uživatele se znovu přihlásit.

V normálním toku OpenID Connect a OAuth, provedli byste to provedením požadavku v2.0 `/token` koncového bodu. Koncový bod v2.0 však nepodporuje požadavků CORS, tak volání jazyka AJAX k získání a obnovovacích tokenů je mimo dotaz. Místo můžete použít implicitní tok v skryté iframe získat nové tokeny pro jiné webové rozhraní API: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [!TIP]
> Zkuste kopírování a vkládání nižší než požadavek na kartě prohlížeče! (Nezapomeňte nahradit `domain_hint` a `login_hint` správné hodnoty pro vaše uživatele)
> 
> 

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| Parametr |  | Popis |
| --- | --- | --- |
| tenant |povinné |`{tenant}` Hodnota v cestě požadavku je možné řídit, kdo se můžete přihlásit do aplikace. Povolené hodnoty jsou `common`, `organizations`, `consumers`a identifikátorů klienta. Další podrobnosti najdete v části [protokol Základy](active-directory-v2-protocols.md#endpoints). |
| client_id |povinné |ID aplikace, které na portál pro registraci ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) přiřazené vaší aplikaci. |
| response_type |povinné |Musí zahrnovat `id_token` pro přihlášení OpenID Connect. Může také zahrnovat další response_types, jako například `code`. |
| redirect_uri |Doporučené |Redirect_uri vaší aplikace, kde můžete odesílat a přijímat aplikací pro žádosti o ověření. Musí odpovídat přesně jeden z redirect_uris, které jste zaregistrovali na portálu, s tím rozdílem, musí být kódování url. |
| scope |povinné |Seznam oborů oddělených mezerami. Jak získat tokeny, zahrnují všechny [obory](v2-permissions-and-consent.md) vyžadujete pro prostředek, které vás zajímají. |
| response_mode |Doporučené |Určuje metodu, která se má použít k odeslání výsledný token zpátky do vaší aplikace. Může být jedna z `query`, `form_post`, nebo `fragment`. |
| state |Doporučené |Hodnota v požadavku, která se také vrátit v odpovědi tokenu. Může být řetězec jakéhokoli obsahu, který chcete. Náhodně generované jedinečná hodnota se obvykle používá pro prevenci útoků proti padělání žádosti více webů. Stav se také používá ke kódování informace o stavu uživatele v aplikaci předtím, než požadavek na ověření došlo k chybě, například stránky nebo zobrazení, které byly na. |
| Hodnota Nonce |povinné |Hodnota v požadavku, generovaný aplikací, který bude obsahovat výsledný id_token jako deklarace identity. Aplikace pak můžete ověřit tuto hodnotu a zmírnění útoků opětovného přehrání tokenu. Hodnota je obvykle náhodnou jedinečného řetězce, který můžete použít k identifikaci původcem požadavku. |
| řádek |povinné |Pro obnovení a získávání tokenů v skrytý element iframe, byste měli použít `prompt=none` zajistíte, že iframe není vydržte v2.0 přihlašovací stránku a vrátí hodnotu okamžitě. |
| login_hint |povinné |Pro aktualizace a získání tokenů v skrytý element iframe, musí obsahovat uživatelské jméno uživatele, v tento pomocný parametr, aby bylo možné rozlišovat mezi více relací, které uživatel může mít k danému bodu v čase. Uživatelské jméno může extrahovat z předchozí přihlášení pomocí `preferred_username` deklarací identity. |
| domain_hint |povinné |Může být jedna z `consumers` nebo `organizations`. Pro obnovení a získávání tokenů v skrytý element iframe, uvést domain_hint v požadavku. By měl extrahujete `tid` deklarace identity z id_token z u předchozího přihlášení rozhodnout, jaká hodnota se má použít. Pokud `tid` deklarace identity, je hodnota `9188040d-6c67-4c5b-b112-36a304b66dad`, měli byste použít `domain_hint=consumers`. Jinak použijte `domain_hint=organizations`. |

K `prompt=none` parametr, tento požadavek buď úspěšné nebo selže okamžitě a vraťte se do vaší aplikace. Úspěšné odpovědi se pošle do vaší aplikace na adrese označený `redirect_uri`, pomocí metody popsané v `response_mode` parametru.

#### <a name="successful-response"></a>Úspěšné odpovědi
Úspěšné odpovědi pomocí `response_mode=fragment` vypadá jako:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Parametr | Popis |
| --- | --- |
| access_token |Token, který požadované aplikace. |
| token_type |Bude vždy `Bearer`. |
| state |Pokud parametr stavu je zahrnutý v požadavku, by se zobrazit stejnou hodnotu v odpovědi. Aplikace by měl ověřit, že jsou identické hodnoty stavu v požadavku a odpovědi. |
| expires_in |Jak dlouho je přístupový token platný (v sekundách). |
| scope |Obory, které je přístupový token platný pro. |

#### <a name="error-response"></a>Odpověď na chybu
Chybové odpovědi může také být odeslán `redirect_uri` tak aplikaci můžete odpovídajícím způsobem zpracovat. V případě třídy `prompt=none`, budou mít očekávanou chybou:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parametr | Popis |
| --- | --- |
| error |Řetězec kódu chyby, která slouží ke klasifikaci typy chyb, ke kterým dochází a je možné reagovat na chyby. |
| error_description |Určité chybové zprávě, který vám pomůže vývojář zjistit původní příčinu chyby ověřování. |

Pokud tato chyba se zobrazí v žádosti o prvku iframe, uživatel musí interaktivně Přihlaste se znovu získat nový token. Můžete zpracovávat tento případ v dle svého dává smysl pro vaši aplikaci.

## <a name="validating-access-tokens"></a>Ověřování tokenů přístupu

Jakmile obdržíte access_token, ujistěte se, že k ověření podpisu tokenu, jakož i následující deklarace identity. Můžete také ověřit další deklarace identity založené na váš scénář. 

* **Cílová skupina** deklarace, ujistěte se, že token, který měla předávat do aplikace
* **Vystavitel** deklarace identity, chcete-li ověřit, zda byl token vydán do vaší aplikace koncovým bodem v2.0
* **Ne dříve než** a **čas vypršení platnosti** deklarací, chcete-li ověřit, že nevypršela platnost tokenu

Další informace o deklarace identity v tokenu přístupu k dispozici, najdete v článku [token reference koncového bodu v2.0](v2-id-and-access-tokens.md)

## <a name="refreshing-tokens"></a>Aktualizace tokeny
Implicitní grant neposkytuje obnovovací tokeny. Obě `id_token`s a `access_token`s vyprší po krátké době čas, takže vaše aplikace musí být připravené k aktualizaci těchto tokenů pravidelně. Pokud chcete aktualizovat buď typ tokenu, můžete provést stejný požadavek skryté iframe ze pomocí `prompt=none` parametr pro řízení chování Azure AD. Pokud chcete dostávat nové `id_token`, nezapomeňte použít `response_type=id_token` a `scope=openid`, a také `nonce` parametru.

## <a name="send-a-sign-out-request"></a>Odeslat odhlášení žádosti
OpenIdConnect `end_session_endpoint` umožňuje vaší aplikaci odesílat požadavek na koncový bod verze 2.0 k ukončení relace uživatele a zrušte zaškrtnutí nastavte koncovým bodem v2.0 soubory cookie. Plně podepsat uživatele z webové aplikace, aplikace by měla ukončit svou vlastní relaci s uživatelem (obvykle zaškrtnutím nebo zrušením tokenu mezipaměti odstranit soubory cookie) a pak přesměrovat prohlížeč, aby:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/logout?post_logout_redirect_uri=https://localhost/myapp/
```

| Parametr |  | Popis |
| --- | --- | --- |
| tenant |povinné |`{tenant}` Hodnota v cestě požadavku je možné řídit, kdo se můžete přihlásit do aplikace. Povolené hodnoty jsou `common`, `organizations`, `consumers`a identifikátorů klienta. Další podrobnosti najdete v části [protokol Základy](active-directory-v2-protocols.md#endpoints). |
| post_logout_redirect_uri | Doporučené | Adresa URL, která uživatel má být vrácen do po dokončení odhlášení. Tato hodnota musí odpovídat jedné z přesměrování, na které registrovaný kód URIs pro aplikaci. Pokud nejsou zahrnuty, se uživateli zobrazí obecná zpráva koncovým bodem v2.0. |
