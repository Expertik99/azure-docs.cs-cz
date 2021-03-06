---
title: Plány nasazení Azure Active Directory | Microsoft Docs
description: Tento článek poskytuje kompletní pokyny pro nasazení funkcí Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: overview
ms.date: 08/03/2018
ms.author: lizross
ms.openlocfilehash: 07d3915fd007c0827b885b0603eb176b9e408576
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2018
ms.locfileid: "39508358"
---
# <a name="azure-active-directory-deployment-plans"></a>Plány nasazení Azure Active Directory
Hledáte kompletní pokyny pro nasazení některých funkcí Azure Active Directory (Azure AD)? Následující plány nasazení popisují obchodní hodnotu, důležité informace o plánování, návrh a provozní postupy potřebné k úspěšnému zavedení několika běžnějších funkcí Azure AD. 

V dokumentech najdete šablony e-mailů, diagramy architektur systémů, běžné testovací případy a další. 

Velmi bychom uvítali váš názor na tyto dokumenty. Vyplňte tento krátký [dotazník](https://aka.ms/deploymentplanfeedback) o vašich zkušenostech s dokumenty. 

|Scénář |Popis |
|-|-|
|[Jednotné přihlašování](https://aka.ms/SSODPDownload)|Jednotné přihlašování pomáhá získat přístup ke všem aplikacím a prostředkům, které potřebujete ke své obchodní činnosti, prostřednictvím jediného přihlášení s použitím jednoho uživatelského účtu. Po přihlášení můžete přecházet například mezi Microsoft Office, SalesForce a Boxem, aniž by se po vás vyžadovalo další ověření (například zadáním hesla).|
|[Zřizování uživatelů](https://aka.ms/UserProvisioningDPDownload)|Azure AD pomáhá automatizovat vytváření, údržbu a odebírání identit uživatelů v cloudových aplikacích (SaaS), jako jsou Dropbox, Salesforce, ServiceNow a další.|
|[Multi-Factor Authentication](https://aka.ms/MFADPDownload)|Azure Multi-Factor Authentication (MFA) je řešení dvoustupňového ověřování od Microsoftu. Použitím metod ověřování schválených správcem pomáhá Azure MFA chránit přístup k datům a aplikacím a současně splňuje požadavky na jednoduchý proces přihlašování.|
|[Podmíněný přístup](https://aka.ms/CADPDownload)|S využitím podmíněného přístupu můžete implementovat automatizované rozhodování ohledně řízení přístupu a na základě podmínek určit, kdo bude mít přístup k vašim cloudovým aplikacím.|
|[ADFS pro předávací ověřování](https://aka.ms/ADFSTOPTADPDownload)|Předávací ověřování Azure AD pomáhá uživatelům přihlašovat se k místním i cloudovým aplikacím s použitím stejných hesel. Tato funkce zlepšuje zkušenosti uživatelů, protože si nemusejí pamatovat další heslo, a snižuje náklady na helpdesk IT, protože se snižuje pravděpodobnost, že uživatelé zapomenou, jak se přihlásit. Když se uživatelé přihlásí pomocí Azure AD, tato funkce ověří jejich hesla přímo v místní službě Active Directory.|
|[ADFS pro synchronizaci hodnot hash hesel](https://aka.ms/ADFSTOPHSDPDownload)|Synchronizace hodnot hash hesel zajišťuje synchronizaci hodnot hash hesel uživatelů z místní služby Active Directory do Azure AD a umožňuje službě Azure AD ověřovat uživatele bez jakékoli interakce s místní službou Active Directory.|
|[Bezproblémové jednotné přihlašování](https://aka.ms/SeamlessSSODPDownload)|Bezproblémové jednotné přihlašování Azure Active Directory (bezproblémové jednotné přihlašování Azure AD) automaticky přihlašuje uživatele, kteří zrovna používají svá podniková zařízení připojená k vaší podnikové síti. Po zapnutí této funkce uživatelé nebudou muset kvůli přihlášení do Azure AD zadávat heslo a obvykle nebudou muset zadávat ani své uživatelské jméno. Tato funkce poskytuje uživatelům snadný přístup k vašim cloudovým aplikacím bez potřeby dalších místních komponent.|
|[Samoobslužné resetování hesla](https://aka.ms/SSPRDPDownload)|Samoobslužné resetování hesla pomáhá uživatelům tím, že si kdykoli a kdekoli můžou v případě potřeby resetovat heslo bez zásahu správce.|
|[Proxy aplikací služby Azure AD](https://aka.ms/AppProxyDPDownload)|Dnešní zaměstnanci chtějí být produktivní, ať jsou kdekoli, a to neustále a na jakémkoli zařízení. Chtějí pracovat na vlastních zařízeních, ať už se jedná o tablety, telefony nebo přenosné počítače. Zaměstnanci navíc očekávají, že budou mít přístup ke všem svým aplikacím, až už jde o aplikace SaaS v cloudu nebo místní podnikové aplikace. Poskytnutí přístupu k místním aplikacím tradičně zahrnovalo virtuální privátní sítě (VPN) nebo demilitarizované zóny (DMZ). Tato řešení jsou nejen složitá a obtížně zabezpečitelná, ale jsou také nákladná na nastavení a správu. Existuje však lepší způsob. – Proxy aplikací služby Azure AD|
