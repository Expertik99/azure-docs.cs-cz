---
title: Azure Active Directory Graph API | Dokumentace Microsoftu
description: Přehled a rychlý start Průvodce pro Azure AD Graph API, která umožňuje programový přístup ke službě Azure AD prostřednictvím koncových bodů rozhraní REST API.
services: active-directory
documentationcenter: ''
author: mtillman
manager: mtillman
editor: mbaldwin
ms.assetid: 5471ad74-20b3-44df-a2b5-43cde2c0a045
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/02/2018
ms.author: mtillman
ms.custom: aaddev
ms.openlocfilehash: 3ec6bbcb9250626f8d6ef75330991c5ebac053a7
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2018
ms.locfileid: "39504713"
---
# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API
> [!IMPORTANT]
> Důrazně doporučujeme pro přístup k prostředkům Azure Active Directory použít [Microsoft Graph](https://graph.microsoft.io/) místo Azure AD Graph API. Náš vývojový program se nyní soustředí na Microsoft Graph a pro Azure AD Graph API nejsou plánovaná žádná další vylepšení. Existuje velmi omezený počet scénářů, pro které může být Azure AD Graph API stále vhodné. Další informace najdete v příspěvku [Microsoft Graph nebo Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) blogu na webu Office Dev Center.
> 
> 

Azure Active Directory Graph API poskytuje programový přístup ke službě Azure AD prostřednictvím koncových bodů rozhraní REST API. Aplikace můžete použít Azure AD Graph API k provedení vytvoření, čtení, aktualizace a odstranění (CRUD) operací objekty a daty adresáře. Například Azure AD Graph API podporuje následující běžných operací pro objekt uživatele:

* Vytvoření nového uživatele v adresáři
* Získejte podrobné vlastnosti uživatele, například jejich skupiny
* Aktualizovat vlastnosti uživatele, jako je například jejich umístění a telefonní číslo, nebo změnit heslo
* Kontrola členství ve skupině uživatele pro přístup na základě rolí
* Zakázat účet uživatele, nebo zcela odstranit

Kromě toho můžete provádět operace podobně jako v jiných objektech, jako jsou skupiny nebo aplikace. Kontaktování Azure AD Graph API v adresáři vaší aplikace musí být zaregistrovaná s Azure AD. Aplikace musí také být udělen přístup k Azure AD Graph API. Tento přístup se obvykle dosahuje prostřednictvím tok vyjádření souhlasu uživatele nebo správce.

Chcete-li začít používat službu Azure Active Directory Graph API, přečtěte si téma [Azure AD Graph API úvodní příručky](active-directory-graph-api-quickstart.md), nebo se podívejte [interaktivní referenční dokumentace rozhraní Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="features"></a>Funkce
Azure AD Graph API poskytuje následující funkce:

* **Koncové body rozhraní API REST**: Azure AD Graph API je služba RESTful sestává z koncových bodů, které jsou přístupné pomocí standardní požadavky HTTP. Azure AD Graph API podporuje typy obsahu XML nebo Javascript Object Notation (JSON) pro požadavky a odpovědi. Další informace najdete v tématu [Azure AD Graph API Reference k rozhraní REST](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* **Ověřování pomocí Azure AD**: každý požadavek do Azure AD Graph API se musí ověřit připojením JSON Web Token (JWT) v autorizační hlavičce požadavku. Tento token se získávají pomocí požadavku na koncový bod tokenu Azure AD a použijte platné přihlašovací údaje. Můžete použít tok přihlašovacích údajů klienta OAuth 2.0 nebo tok k získání tokenu pro volání Graphu poskytování autorizačních kódů. Další informace najdete [OAuth 2.0 ve službě Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* **Na základě rolí autorizace (RBAC)**: skupiny zabezpečení jsou používány k provádění RBAC v Azure AD Graph API. Například, pokud chcete zjistit, zda má uživatel přístup ke konkrétnímu prostředku, může zavolat [zkontrolovat členství ve skupině (přenosné)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/functions-and-actions#checkMemberGroups) operaci, která vrátí hodnotu true nebo false.
* **Rozdílový dotaz**: rozdílový dotaz umožňuje sledovat změny v adresáři mezi dvěma časová období bez nutnosti provádět časté dotazy k Azure AD Graph API. Tento typ požadavku vrátí pouze změny provedené mezi předchozí požadavek rozdílový dotaz a aktuální požadavek. Další informace najdete v tématu [Azure AD Graph API rozdílový dotaz](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).
* **Rozšíření adresáře**: Přidání vlastních vlastností do objektů adresáře bez nutnosti externím úložišti. Například pokud vaše aplikace vyžaduje vlastnost Skypové pro každého uživatele, můžete zaregistrovat nové vlastnosti v adresáři a bude k dispozici pro použití u každého uživatelského objektu. Další informace najdete v tématu [Azure AD Graph API adresáře schématu rozšíření](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **Zabezpečuje obory oprávnění**: Azure AD Graph API zveřejňuje obory oprávnění, které umožňují zabezpečený přístup k datům Azure AD pomocí OAuth 2.0. Podporuje širokou škálu typů aplikací klienta, včetně:
  
  * uživatelská rozhraní, která jsou uvedena Delegovaný přístup k datům prostřednictvím autorizace od přihlášeného uživatele (delegovaný)
  * aplikace služby/démona, pracovat na pozadí bez přihlášeného uživatele byla vyžadována jeho přítomnost, které používají řízení přístupu na základě rolí definované aplikací
    
    Obě delegovaný a oprávnění aplikace představují oprávnění vystavené Azure AD Graph API a můžete požadovat u klientských aplikací pomocí funkce oprávnění registrace aplikace v [webu Azure portal](https://portal.azure.com). [Azure AD Graph API oprávnění obory](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes) poskytuje informace o tom, co je k dispozici pro použití klientské aplikace.

## <a name="scenarios"></a>Scénáře
Azure AD Graph API umožňuje mnoho scénářů aplikace. Nejčastěji používané jsou následující scénáře:

* **Obchodní (jeden Tenant) aplikace**: V tomto scénáři funguje podnikový vývojář v organizaci, která má předplatné Office 365. Vývojář je vytváření webové aplikace, která komunikuje se službou Azure AD k provádění úloh, jako je například přiřazení licence k uživateli. Tato úloha vyžaduje přístup k Azure AD Graph API, takže vývojář zaregistruje ve službě Azure AD jednoho tenanta aplikaci a nakonfiguruje oprávnění čtení a zápisu pro Azure AD Graph API. Potom aplikace je nakonfigurována používat svoje vlastní přihlašovací údaje nebo jsou právě přihlášení uživatele k získání tokenu pro volání rozhraní Azure AD Graph API.
* **Software jako služba aplikace (s více Tenanty)**: V tomto scénáři je nezávislý výrobce softwaru (ISV) vývoj prostředí více tenantů webovou aplikaci, která poskytuje funkce pro správu uživatelů pro jiných organizací používajících službu Azure AD. Tyto funkce vyžadují přístup k adresářových objektů, takže aplikace musí volat rozhraní Azure AD Graph API. Vývojář aplikace registruje ve službě Azure AD, nakonfiguruje ho, aby se vyžadují pro čtení a zápis pro Azure AD Graph API a pak povolí externího přístupu tak, aby další organizace můžou udělit souhlas k používání aplikace v jejich adresáře. Při ověření uživatele v jiné organizaci k aplikaci poprvé, zobrazí se dialogové okno souhlasu s oprávnění, která požaduje aplikace. Udělení souhlasu bude dejte aplikaci jsou požadovaná oprávnění k Azure AD Graph API v adresáři uživatele. Další informace o rozhraní pro udělování souhlasu najdete v tématu [přehled architektury souhlas](quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="see-also"></a>Viz také
[Příručky rychlý start Azure AD Graph API](active-directory-graph-api-quickstart.md)

[Dokumentace ke službě Azure AD Graph REST](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Příručka pro vývojáře pro službu Azure Active Directory](azure-ad-developers-guide.md)

