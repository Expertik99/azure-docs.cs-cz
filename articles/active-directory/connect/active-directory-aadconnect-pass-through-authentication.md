---
title: 'Služby Azure AD Connect: Předávací ověřování | Dokumentace Microsoftu'
description: Tento článek popisuje předávací ověřování služby Azure Active Directory (Azure AD) a jak umožňuje přihlášení Azure AD pomocí ověřování hesla uživatelů v místním Active Directory.
services: active-directory
keywords: Co je Azure AD Connect předávací ověřování, instalace služby Active Directory, požadované součásti pro službu Azure AD, jednotné přihlašování, jednotné přihlašování
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 12c825143f48b5558ea9b1d49ed8cea59d84f6af
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/06/2018
ms.locfileid: "39522777"
---
# <a name="user-sign-in-with-azure-active-directory-pass-through-authentication"></a>Přihlášení uživatele pomocí předávacího ověřování Azure Active Directory

## <a name="what-is-azure-active-directory-pass-through-authentication"></a>Co je Azure Active Directory předávací ověřování?

Předávací ověřování Azure Active Directory (Azure AD) umožňuje uživatelům se přihlásit přes místní aplikace založené na cloudu přihlašovali stejnými hesly. Tato funkce nabízí uživatelům lepší – jeden menší heslo mějte na paměti a snižuje náklady na IT helpdesk, protože jsou vaši uživatelé méně pravděpodobné, že zapomenete jak se přihlásit. Při přihlášení uživatele pomocí služby Azure AD, tato funkce ověřuje hesla uživatelů přímo proti místní Active Directory.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

Tato funkce se o alternativu k [synchronizaci hodnot Hash hesel služby Azure AD](active-directory-aadconnectsync-implement-password-hash-synchronization.md), která nabízí stejné výhody cloudové ověřování pro organizace. Některé organizace chtějí zajistit jejich zabezpečení místní služby Active Directory a zásad pro hesla, ale můžete místo toho použít předávací ověřování. Kontrola [Tato příručka](https://docs.microsoft.com/azure/security/azure-ad-choose-authn) porovnání různé služby Azure AD přihlášení metod a jak zvolit metodu přímo přihlášení pro vaši organizaci.

![Azure AD předávací ověřování](./media/active-directory-aadconnect-pass-through-authentication/pta1.png)

Můžete kombinovat předávacího ověřování s [bezproblémové jednotné přihlašování](active-directory-aadconnect-sso.md) funkce. Tímto způsobem, pokud vaši uživatelé získávají přístup k aplikacím na svých počítačích podnikové uvnitř firemní sítě, nepotřebujete k zadání hesla k přihlášení.

## <a name="key-benefits-of-using-azure-ad-pass-through-authentication"></a>Mezi klíčové výhody používání předávacího ověřování Azure AD

- *Skvělé uživatelské prostředí*
  - Uživatelé používat stejné hesla k přihlášení na místních a cloudových aplikací.
  - Uživatelé stráví méně času s IT helpdesk řešení související s hesly potíže.
  - Uživatelé můžou Dokončit [Samoobslužná správa hesel](../authentication/active-directory-passwords-overview.md) úloh v cloudu.
- *Snadné nasazení a Správa*
  - Není nutné pro místní nasazení nebo konfiguraci sítě.
  - Potřebuje pouze jednoduché agenta být nainstalovaný místní.
  - Bez režie na správu. Agent automaticky přijme vylepšení a oprav chyb.
- *Zabezpečení*
  - Místních hesel se nikdy neukládají v cloudu v libovolné formě.
  - Agent je pouze odchozí připojení z v rámci vaší sítě. Proto neexistuje žádný požadavek na instalaci agenta v hraniční síti, označované také jako DMZ.
  - Chrání vaše uživatelské účty tím, že funguje bez problémů s [zásady podmíněného přístupu Azure AD](../active-directory-conditional-access-azure-portal.md), včetně služby Multi-Factor Authentication (MFA), [blokování starší verze ověřování](../conditional-access/conditions.md) a [ filtrování útoky na hesla hrubou silou](../authentication/howto-password-smart-lockout.md).
- *S vysokou dostupností*
  - Další agenty lze nainstalovat na několik místních serverů pro zajištění vysoké dostupnosti žádostí o přihlášení.

## <a name="feature-highlights"></a>Stručný přehled funkcí

- Podporuje přihlášení uživatele do všech webových aplikací využívajících prohlížeč a do klientské aplikace Microsoft Office, které používají [moderní ověřování](https://aka.ms/modernauthga).
- Uživatelská jména přihlášení může být buď místní výchozí uživatelské jméno (`userPrincipalName`) nebo jiný atribut nakonfigurované ve službě Azure AD Connect (označované jako `Alternate ID`).
- Tato funkce funguje bez problémů s [podmíněného přístupu](../active-directory-conditional-access-azure-portal.md) funkce jako je například Multi-Factor Authentication (MFA) pro pomoc se zabezpečením vašich uživatelů.
- Integrované s cloudovými [Samoobslužná správa hesel](../authentication/active-directory-passwords-overview.md), včetně zpětný zápis hesla do místní služby Active Directory a ochrany heslem podle zakazovat běžně používaných hesel.
- Podporují se prostředí s více doménovými strukturami, pokud existují vztahy důvěryhodnosti doménové struktury mezi vaší doménové struktury AD a v případě směrování přípon názvů je správně nakonfigurovaný.
- Je funkci a není nutné žádné placené edice Azure AD, aby ho použít.
- Je možné povolit prostřednictvím [Azure AD Connect](active-directory-aadconnect.md).
- Používá jednoduché místní agent přijímá a reaguje na požadavky ověřování hesla.
- Instalaci víc agentů poskytuje vysokou dostupnost žádostí o přihlášení.
- To [chrání](../authentication/howto-password-smart-lockout.md) vaše účty místní vůči útoku hrubou vynutit útoky hesla v cloudu.

## <a name="next-steps"></a>Další postup

- [Rychlý Start](active-directory-aadconnect-pass-through-authentication-quick-start.md) – rychle zprovoznit a systémem předávacího ověřování Azure AD.
- [Migrace ze služby AD FS na předávací ověřování](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx) – podrobné pokyny k migraci ze služby AD FS (nebo jiné technologie federation) na předávací ověřování.
- [Inteligentní uzamčení](../authentication/howto-password-smart-lockout.md) – konfigurace inteligentním uzamčením funkce na tenantovi služby ochrany uživatelských účtů.
- [Aktuální omezení](active-directory-aadconnect-pass-through-authentication-current-limitations.md) – zjistěte, jaké postupy se podporují, a ty, které nejsou.
- [Podrobné technické informace](active-directory-aadconnect-pass-through-authentication-how-it-works.md) -pochopit, jak tato funkce funguje.
- [Nejčastější dotazy](active-directory-aadconnect-pass-through-authentication-faq.md) – odpovědi na nejčastější dotazy.
- [Řešení potíží s](active-directory-aadconnect-troubleshoot-pass-through-authentication.md) – zjistěte, jak vyřešit běžné problémy s funkcí.
- [Podrobné informace o zabezpečení](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md) – další podrobné technické informace o funkci.
- [Azure AD bezproblémového jednotného přihlašování k](active-directory-aadconnect-sso.md) – Další informace o této doplňkové funkce.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – k podání žádostí o nové funkce.
