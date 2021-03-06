---
title: Pokročilé možnosti v tokenu SAML pro předběžně integrované aplikace v Azure Active Directory podepisování certifikátů | Dokumentace Microsoftu
description: Zjistěte, jak použít pokročilý certifikát Podepisování možnosti v tokenu SAML pro předběžně integrované aplikace v Azure Active Directory
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/07/2017
ms.author: barbkess
ms.reviewer: jeedes
ms.custom: aaddev
ms.openlocfilehash: e86c7e82203e2ecd571d5ce5e450999704218e33
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/06/2018
ms.locfileid: "39579159"
---
# <a name="advanced-certificate-signing-options-in-the-saml-token-for-gallery-apps-in-azure-active-directory"></a>Rozšířené možnosti v tokenu SAML pro aplikace z Galerie v Azure Active Directory podepsání certifikátu
Azure Active Directory (Azure AD) ještě dnes podporuje tisíce předem integrovaných aplikací v galerii aplikací Azure Active Directory. Toto číslo zahrnuje více než 500 aplikace, které podporují jednotné přihlašování s použitím protokolu SAML 2.0. Když se uživatel přihlásí k aplikaci prostřednictvím služby Azure AD pomocí SAML, Azure AD odešle token aplikaci (přes HTTP POST). Aplikace pak ověří a použije token pro přihlášení uživatele místo vás vyzve k zadání uživatelského jména a hesla. Tyto tokeny SAML jsou podepsány pomocí jedinečný certifikát, který je generován ve službě Azure AD a konkrétní standardních algoritmů.

Azure AD používá některé výchozí nastavení pro aplikace Galerie. Výchozí hodnoty nastavení jsou podle požadavků vaší aplikace.

Azure AD podporuje upřesňující nastavení podepisování certifikátu. Pokud chcete vybrat tyto možnosti, vyberte nejdřív **zobrazit pokročilé nastavení podepisování certifikátu** zaškrtávací políčko:

![Zobrazit upřesňující nastavení podepisování certifikátu](./media/certificate-signing-options/saml-advance-certificate.png)

Po výběru tohoto zaškrtávacího políčka můžete nastavit certifikát možnosti podpisu a podpisový algoritmus.

## <a name="certificate-signing-options"></a>Možnosti podpisu certifikátu

Azure AD podporuje tři možnosti podpisový certifikát:

* **Podepsat kontrolní výraz SAML**. Toto výchozí nastavení je pro většinu z Galerie aplikací. Pokud je vybraná tato možnost, Azure AD jako identity podepíše kontrolní výraz SAML a certifikát se X509 certifikát aplikace. Kromě toho používá podpisový algoritmus, který je vybrán v **podpisový algoritmus** rozevíracího seznamu.

* **Podepsat odpověď SAML**. Pokud je vybraná tato možnost, Azure AD jako identity podepíše odpověď SAML s X509 certifikát aplikace. Kromě toho používá podpisový algoritmus, který je vybrán v **podpisový algoritmus** rozevíracího seznamu.

* **Podepsat odpověď SAML a kontrolní výraz**. Pokud je vybraná tato možnost, Azure AD jako identity podepíše celý token SAML s X509 certifikát aplikace. Kromě toho používá podpisový algoritmus, který je vybrán v **podpisový algoritmus** rozevíracího seznamu.

    ![Možnosti podpisu certifikátu](./media/certificate-signing-options/saml-signing-options.png)

## <a name="certificate-signing-algorithms"></a>Algoritmy podepsání certifikátu

Azure AD podporuje dva podpisové algoritmy podepsat odpověď SAML:

* **SHA-256**. Azure AD používá tuto výchozí algoritmus podepsat odpověď SAML. Je nejnovější algoritmus a je považován za více bezpečné než SHA-1. Většina aplikací podporuje algoritmus SHA-256. Pokud aplikace podporuje pouze SHA-1 jako podpisový algoritmus, můžete ji změnit. Jinak doporučujeme použít algoritmus SHA-256 k podepisování odpověď SAML.

    ![SHA-256 certifikát podpisový algoritmus](./media/certificate-signing-options/saml-signing-algo-sha256.png)

* **SHA-1**. To je algoritmus starší a je považován za méně bezpečné než SH-256. Pokud aplikace podporuje pouze tento podpisový algoritmus, můžete vybrat tuto možnost v **podpisový algoritmus** rozevíracího seznamu. Azure AD pak podepíše odpověď SAML pomocí algoritmu SHA-1.

    ![SHA-1 certifikátu podpisový algoritmus](./media/certificate-signing-options/saml-signing-algo-sha1.png)

## <a name="next-steps"></a>Další postup
* [Rejstřík článků o správě aplikací ve službě Azure Active Directory](../active-directory-apps-index.md)
* [Konfigurace jednotného přihlašování k aplikacím, které nejsou v galerii aplikací Azure Active Directory](../application-config-sso-how-to-configure-federated-sso-non-gallery.md)
* [Řešení potíží s založené na SAML jednotného přihlašování](../develop/howto-v1-debug-saml-sso-issues.md)


