---
title: Upgrade Bingu v5 rozhraní API pro kontrolu na v7 pravopisu | Dokumentace Microsoftu
description: Identifikuje části aplikace, které je potřeba aktualizovat na použití verze 7.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 7DC8FB29-4732-47D8-824B-CF2D7AEBA07B
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: article
ms.date: 06/21/2016
ms.author: scottwhi
ms.openlocfilehash: 305139e45ee93614eab17c5798cb1105e3e8f8cb
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/21/2018
ms.locfileid: "41987477"
---
# <a name="spell-check-api-upgrade-guide"></a>Průvodce upgradem rozhraní API pro kontrolu pravopisu

Tento průvodce upgradem identifikuje změny mezi verzí 5 a API kontrola pravopisu Bingu verze 7. Tento průvodce vám pomůže identifikovat části aplikace, které je potřeba aktualizovat na použití verze 7.

## <a name="breaking-changes"></a>Změny způsobující chyby

### <a name="endpoints"></a>Koncové body

- Číslo verze koncový bod se změní z v5 na v7. Například, `https://api.cognitive.microsoft.com/bing/v7.0/spellcheck`.

### <a name="error-response-objects-and-error-codes"></a>Objekty odpovědi chyby a chybové kódy

- Všechny neúspěšné žádosti by teď měl obsahovat `ErrorResponse` objektu v textu odpovědi.

- Přidat následující pole `Error` objektu.  
  - `subCode`&mdash;Oddíly kód chyby: do samostatných sad, tj. Pokud je to možné
  - `moreDetails`&mdash;Další informace o chybě popsaných v `message` pole
   

- Nahradí chybové kódy v5 následujícího `code` a `subCode` hodnoty.  
  
|Kód|Podřízeného|Popis
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Neimplementováno|Bing vrátí ServerError pokaždé, když dojde k některé z podmínek podřízeného. Odpověď obsahuje tyto chyby, pokud je stavový kód HTTP 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Blokováno|Bing vrátí InvalidRequest pokaždé, když libovolnou část žádosti není platný. Například povinný parametr chybí nebo není platná hodnota parametru.<br/><br/>Pokud je chyba ParameterMissing nebo ParameterInvalidValue, je stavový kód HTTP 400.<br/><br/>Pokud je chyba HttpNotAllowed, stavový kód HTTP 410.
|RateLimitExceeded||Bing vrátí RateLimitExceeded pokaždé, když překročíte dotazů za sekundu (QPS) nebo dotazů za měsíc (QPM) kvóty.<br/><br/>Bing vrátí stavový kód HTTP 429, pokud se překročí QPS a 403 překročení QPM.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing vrátí InvalidAuthorization při Bingu se nemůže ověřit volající. Například `Ocp-Apim-Subscription-Key` záhlaví chybí nebo není platný klíč předplatného.<br/><br/>Redundance nastane, pokud zadáte více než jednu metodu ověřování.<br/><br/>Pokud je chyba InvalidAuthorization, je stavový kód HTTP 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing vrátí InsufficientAuthorization, když volající nemá oprávnění k přístupu k prostředku. Tato situace může nastat, pokud klíč předplatného se zakázalo, nebo vypršela platnost. <br/><br/>Pokud je chyba InsufficientAuthorization, je stavový kód HTTP 403.

- Následující mapuje předchozích kódů chyb nové kódy. Pokud jste pořídili závislost na kódy chyb v5, aktualizujte svůj kód odpovídajícím způsobem.  
  
|Kód verze 5|Verze 7 code.subCode
|-|-
|RequestParameterMissing|InvalidRequest.ParameterMissing
RequestParameterInvalidValue|InvalidRequest.ParameterInvalidValue
ResourceAccessDenied|InsufficientAuthorization
ExceededVolume|RateLimitExceeded
ExceededQpsLimit|RateLimitExceeded
Zakázáno|InsufficientAuthorization.AuthorizationDisabled
UnexpectedError|ServerError.UnexpectedError
DataSourceErrors|ServerError.ResourceError
AuthorizationMissing|InvalidAuthorization.AuthorizationMissing
HttpNotAllowed|InvalidRequest.HttpNotAllowed
UserAgentMissing|InvalidRequest.ParameterMissing
Neimplementováno|ServerError.NotImplemented
InvalidAuthorization|InvalidAuthorization
InvalidAuthorizationMethod|InvalidAuthorization
MultipleAuthorizationMethod|InvalidAuthorization.AuthorizationRedundancy
ExpiredAuthorizationToken|InsufficientAuthorization.AuthorizationExpired
InsufficientScope|InsufficientAuthorization
Blokováno|InvalidRequest.Blocked

## <a name="next-steps"></a>Další postup

> [!div class="nextstepaction"]
> [Použít a zobrazit požadavky](./UseAndDisplayRequirements.md)
