---
title: S deklaracemi identity aplikace – Proxy aplikací Azure AD | Dokumentace Microsoftu
description: Jak publikovat místní aplikace ASP.NET, které přijímají deklarací služby AD FS pro zabezpečený vzdálený přístup vašich uživatelů.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: barbkess
ms.reviewer: harshja
ms.openlocfilehash: cf92b5b6ee3c6a529a43e7fa4cfeeb09954ad9ea
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365387"
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Práce s deklaracemi identity aplikace v Proxy aplikací
[S deklaracemi identity aplikace](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) provést přesměrování do tokenů zabezpečení služby (STS). Služba tokenů zabezpečení přihlašovacích údajů požádá uživatele výměnou za token a pak přesměruje uživatele k aplikaci. Existuje několik způsobů, jak povolit Proxy aplikace pro práci s těmito přesměrování. Tento článek slouží ke konfiguraci svého nasazení s deklaracemi identity aplikace. 

## <a name="prerequisites"></a>Požadavky
Ujistěte se, že je služba tokenů zabezpečení, které s deklaracemi identity aplikace přesměruje na mimo místní síť k dispozici. Můžete zpřístupnit služby STS zveřejnění prostřednictvím proxy serveru nebo tím, že externí připojení. 

## <a name="publish-your-application"></a>Publikování aplikace

1. Publikování aplikace podle pokynů v tématu [publikování aplikací pomocí Proxy aplikace](application-proxy-publish-azure-portal.md).
2. Přejděte na stránku aplikace v portálu a vyberte **jednotného přihlašování**.
3. Pokud jste zvolili **Azure Active Directory** jako vaše **metoda předběžného ověření**vyberte **Azure AD jednotné přihlašování zakázáno** jako vaše **interní Metoda ověřování**. Pokud jste zvolili **průchozí** jako vaše **metoda předběžného ověření**, nemusíte nic měnit.

## <a name="configure-adfs"></a>Konfigurace služby AD FS

Konfigurace služby AD FS pro deklaracemi identity aplikace v jednom ze dvou způsobů. První je pomocí vlastních domén. Druhou je použití WS-Federation. 

### <a name="option-1-custom-domains"></a>Možnost 1: Vlastní domény

Pokud všechny interní adresy URL pro vaše aplikace jsou plně kvalifikované názvy domény (FQDN), pak můžete nakonfigurovat [vlastních domén](application-proxy-configure-custom-domain.md) pro vaše aplikace. Použijte vlastní domény k vytvoření externí adresy URL, které jsou stejné jako interní adresy URL. Když externí adresy URL odpovídají interní adresy URL, přesměrování služby tokenů zabezpečení fungovat, jestli vaši uživatelé jsou místní nebo vzdálené. 

### <a name="option-2-ws-federation"></a>Možnost 2: WS-Federation

1. Otevřete správu služby AD FS.
2. Přejděte na **vztahy důvěryhodnosti předávající strany**, klikněte pravým tlačítkem na aplikaci, které publikujete pomocí Proxy aplikace a zvolte **vlastnosti**.  

   ![Vztahy důvěryhodnosti předávající strany, klikněte pravým tlačítkem na název aplikace – snímek obrazovky](./media/application-proxy-configure-for-claims-aware-applications/appproxyrelyingpartytrust.png)  

3. Na **koncové body** ve skupině **typ koncového bodu**vyberte **WS-Federation**.
4. V části **důvěryhodné adresy URL**, zadejte adresu URL, které jste zadali v Proxy aplikací v rámci **externí adresa URL** a klikněte na tlačítko **OK**.  

   ![Přidání koncového bodu - nastavit hodnotu adresy URL důvěryhodných – snímek obrazovky](./media/application-proxy-configure-for-claims-aware-applications/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a>Další postup
* [Povolení jednoduchého přihlášení na](application-proxy-single-sign-on.md) pro aplikace, které nejsou s deklaracemi identity
* [Povolit nativní klientské aplikace pro interakci s proxy aplikací](application-proxy-configure-native-client-application.md)


