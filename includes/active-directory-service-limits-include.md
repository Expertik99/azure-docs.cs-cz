---
title: zahrnout soubor
description: zahrnout soubor
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 07/30/2018
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: a2055c3f00306fbfdad3028a16bb49d919e1e251
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/31/2018
ms.locfileid: "39361460"
---
Zde jsou uvedena omezení využití a další omezení služby pro službu Azure Active Directory (Azure AD).

| Kategorie | Omezení |
| --- | --- |
| Adresáře | Jeden uživatel může patřit do maximálně 500 adresářů služby Azure AD jako člen nebo Host.<br/>Jenom jednoho konkrétního uživatele můžete vytvořit maximálně 20 adresářů. |
| Domény | Můžete přidat více než 900 spravovaných názvech domén. Pokud nastavujete všechny domén pro federaci s místní služby Active Directory, můžete přidat více než 450 názvy domén v každém adresáři. |
| Objekty |<ul><li>Maximálně 500 000 objektů lze vytvořit v jednom adresáři uživatelé edice Free služby Azure Active Directory.</li><li>Uživatelé, kteří nejsou správci, můžou vytvořit maximálně 250 objektů.</li></ul> |
| Rozšíření schématu |<ul><li>Rozšíření řetězcového typu (String) můžou mít maximálně 256 znaků. </li><li>Rozšíření binárního typu (Binary) jsou omezena 256 bajty.</li><li>K libovolnému objektu je možné zapsat 100 hodnot rozšíření (v rámci VŠECH typů a VŠECH aplikací).</li><li>Pouze entity User, Group, TenantDetail, Device, Application a ServicePrincipal je možné rozšířit pomocí atributů typu String nebo Binary s jednou hodnotou.</li><li>Rozšíření schématu jsou k dispozici pouze v rozhraní Graph API verze 1.21 Preview. Pro registraci rozšíření musí aplikace mít přístup k zápisu.</li></ul> |
| Aplikace |Maximálně 100 uživatelů může být vlastníkem jedné aplikace. |
| Skupiny |<ul><li>Maximálně 100 uživatelů může být vlastníkem jedné skupiny.</li><li>Členem jedné skupiny ve službě Azure Active Directory může být libovolný počet objektů.</li><li>Počet členů ve skupině, které můžete synchronizovat z místní Active Directory do Azure Active Directory pomocí služby Azure AD Connect je omezen na 50 tis.</li></ul> |
| Přístupový panel |<ul><li>Pro koncové uživatele a pro uživatele s přiřazenými licencemi služby Azure AD Premium nebo Enterprise Mobility Suite neexistuje žádné omezení počtu aplikací, které můžou zobrazit na přístupovém panelu.</li><li>Jednotliví koncoví uživatelé s přiřazenými licencemi edice Free nebo Basic služby Azure Active Directory můžou na přístupovém panelu zobrazit maximálně 10 dlaždic aplikací (například: Box, Salesforce nebo Dropbox). Toto omezení se nevztahuje na účty správců.</li></ul> |
| Reports | V jednotlivých sestavách je možné zobrazit nebo stáhnout maximálně 1 000 řádků. Veškerá další data se oříznou. |
| Jednotky pro správu | Objekt může být členem maximálně 30 jednotek pro správu. |
