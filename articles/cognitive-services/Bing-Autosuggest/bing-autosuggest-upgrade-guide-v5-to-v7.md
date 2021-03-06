---
title: Rozhraní API verze 5 k v7 automatické návrhy v Bingu upgradu | Microsoft Docs
description: Identifikuje části aplikace, které je potřeba aktualizovat na použití verze 7.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 751EDCF0-0C8B-4C23-942C-FA06F5DAD3FD
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: article
ms.date: 01/12/2017
ms.author: scottwhi
ms.openlocfilehash: 5663a671711dba4f44c89e8221a729c6670ec8fc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/23/2018
ms.locfileid: "35343418"
---
# <a name="autosuggest-api-upgrade-guide"></a>Průvodce upgradem rozhraní API pro automatické návrhy

Tento průvodce upgradem identifikuje změny mezi 5 a verze 7 rozhraní API pro automatické návrhy v Bingu. Pomocí Tento průvodce vám pomůže identifikovat části aplikace, které je potřeba aktualizovat na použití verze 7.

## <a name="breaking-changes"></a>Změny způsobující chyby

### <a name="endpoints"></a>Koncové body

- Číslo verze pro koncový bod změněn z v5 na v7. Například https://api.cognitive.microsoft.com/bing/\ * \*v7.0 ** nebo návrhy.

### <a name="error-response-objects-and-error-codes"></a>Chyba odpovědi objekty a kódy chyb

- Teď by měla obsahovat všechny neúspěšné žádosti `ErrorResponse` objektu v textu odpovědi.

- Přidat následující pole do `Error` objektu.  
  - `subCode`&mdash;Oddíly. kód chyby do diskrétní sad, pokud je to možné
  - `moreDetails`&mdash;Další informace o této chybě podrobněji popsaná `message` pole

- Kódy chyb v5 nahradí následující možné `code` a `subCode` hodnoty.

|Kód|Dílčí|Popis
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Neimplementováno|Bing vrátí ServerError vždy, když nastane některá z podmínek dílčí kódu. Odpověď obsahuje tyto chyby, pokud je stavový kód protokolu HTTP 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Blokováno|Bing vrátí InvalidRequest vždy, když libovolná součást žádosti není platný. Například chybí povinný parametr nebo hodnota parametru není platný.<br/><br/>Pokud je chyba ParameterMissing nebo ParameterInvalidValue, je stavový kód HTTP 400.<br/><br/>Pokud je chyba HttpNotAllowed, stavový kód protokolu HTTP 410.
|RateLimitExceeded||Bing vrátí RateLimitExceeded vždy, když překročí dotazů za sekundu (QPS) nebo dotazů za měsíc (QPM) kvóty.<br/><br/>Bing vrátí stavový kód HTTP 429, pokud se překročí QPS a 403 překračování QPM.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing vrátí InvalidAuthorization, pokud Bing nemůže ověřit volající. Například `Ocp-Apim-Subscription-Key` chybí hlavička nebo klíč předplatného není platný.<br/><br/>Pokud zadáte více než jednu metodu ověřování, dojde k redundance.<br/><br/>Pokud je chyba InvalidAuthorization, je stavový kód HTTP 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing vrátí InsufficientAuthorization, pokud má volající nemá oprávnění pro přístup k prostředku. Tato situace může nastat, pokud klíč předplatného je zakázané nebo vypršela platnost. <br/><br/>Pokud je chyba InsufficientAuthorization, je stavový kód HTTP 403.

- Následující mapuje předchozí kódy chyb nové kódy. Pokud jste prováděné závislost na kódy chyb v5, aktualizujte kód odpovídajícím způsobem.

|Verze 5 kódu|Verze 7 code.subCode
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
> [Požadavky na použití a zobrazení](./UseAndDisplayRequirements.md)
