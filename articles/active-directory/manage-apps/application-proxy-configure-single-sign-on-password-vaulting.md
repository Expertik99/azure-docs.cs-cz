---
title: Jednotné přihlašování do aplikací pomocí Proxy aplikací Azure AD | Dokumentace Microsoftu
description: Zapnutí jedné přihlašování pro aplikace publikované v místním s Proxy aplikací Azure AD na webu Azure Portal.
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
ms.date: 07/20/2017
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: fa12dd5e9dbe25bad947abed5ab1c732d231b25c
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/31/2018
ms.locfileid: "39362818"
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a>Heslo vaulting pro jednotné přihlašování s Proxy aplikací

Azure Active Directory Application Proxy vám pomůže zlepšit produktivitu a publikujte místní aplikace tak, aby vzdálení zaměstnanci můžou bezpečně k nim přístup, příliš. Na webu Azure Portal můžete také nastavíte jednotné přihlašování (SSO) k těmto aplikacím. Uživatelé potřebují jenom k ověření ve službě Azure AD a bude moct své podnikové aplikace bez nutnosti znovu přihlásit.

Proxy aplikací podporuje několik [jednotného přihlašování režimy](application-proxy-single-sign-on.md). Přihlašování na základě heslo je určená pro aplikace, které používají kombinace uživatelského jména a hesla pro ověřování. Když konfigurujete přihlašování na základě heslo pro vaši aplikaci, pro přihlášení k místní aplikaci jednou mít uživatelé. Azure Active Directory, ukládá přihlašovací údaje a automaticky poskytuje k aplikaci při vaši uživatelé k němu přistupovat vzdáleně. 

By měl už máte publikování a testování aplikace s využitím Proxy aplikací. Pokud ne, postupujte podle kroků v [publikování aplikací pomocí Proxy aplikací Azure AD](application-proxy-publish-azure-portal.md) poté vraťte se sem. 

## <a name="set-up-password-vaulting-for-your-application"></a>Nastavit heslo vaulting pro vaši aplikaci

1. Přihlaste se na webu [Azure Portal](https://portal.azure.com) jako správce.
2. Vyberte **Azure Active Directory** > **podnikové aplikace** > **všechny aplikace**.
3. V seznamu vyberte aplikaci, kterou chcete nastavit jednotné přihlašování.  
4. Vyberte **jednotného přihlašování**.

   ![Vyberte jednotného přihlašování](./media/application-proxy-configure-single-sign-on-password-vaulting/select-sso.png)

5. Režim jednotného přihlašování, zvolte **přihlašování na základě heslo**.
6. Přihlašovací adresy URL zadejte adresu URL pro stránku, kde uživatelé zadat svoje uživatelské jméno a heslo pro přihlášení k aplikaci mimo podnikovou síť. To může být externí adresu URL, kterou jste vytvořili při publikování aplikace prostřednictvím Proxy aplikací. 

   ![Zvolte přihlašování na základě hesla a zadejte adresu URL](./media/application-proxy-configure-single-sign-on-password-vaulting/password-sso.png)

7. Vyberte **Uložit**.

<!-- Need to repro?
7. The page should tell you that a sign-in form was successfully detected at the provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow the instructions to point out where the sign-in credentials go. 
-->

## <a name="test-your-app"></a>Testování aplikace

Přejděte na externí adresu URL, kterou jste nakonfigurovali pro vzdálený přístup do vaší aplikace. Přihlaste se pomocí svých přihlašovacích údajů pro tuto aplikaci (nebo přihlašovací údaje pro účet testu, které jste nastavili přístup). Po přihlášení úspěšné, byste měli opustit aplikaci a vraťte bez nutnosti zadávat svoje přihlašovací údaje znovu. 

## <a name="next-steps"></a>Další postup

- Přečtěte si informace o další možnosti, jak implementovat [jednotné přihlašování s Proxy aplikací](application-proxy-single-sign-on.md)
- Další informace o [aspekty zabezpečení pro přístup k aplikacím s využitím Azure AD Application Proxy](application-proxy-security.md)