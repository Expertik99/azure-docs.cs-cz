---
title: Zařízení s nižší úrovně připojená k řešení potíží s hybridní služby Azure Active Directory | Dokumentace Microsoftu
description: Řešení potíží s hybridní služby Azure Active Directory zařízení připojená k nižší úrovně
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 2c50ba1abfe3681a39b39bf52f127efd9d518aef
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/27/2018
ms.locfileid: "43041864"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Zařízení s nižší úrovně připojená k řešení potíží s hybridní služby Azure Active Directory 

V tomto článku se vztahuje pouze na následujících zařízeních: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Pro Windows 10 nebo Windows Server 2016, najdete v článku [zařízení s Windows 10 a Windows serveru 2016 připojená k hybridní Poradce při potížích s Azure Active Directory](troubleshoot-hybrid-join-windows-current.md).

Tento článek předpokládá, že máte [nakonfigurované hybridní služby Azure Active Directory zařízení připojená k](hybrid-azuread-join-plan.md) pro podporu následujících scénářů:

- Podmíněný přístup podle zařízení

- [Podnikový roaming nastavení](../active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello pro firmy](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification) 





Tento článek poskytuje pokyny o tom, jak vyřešit potenciální problémy při řešení potíží.  

**Co byste měli vědět:** 

- Maximální počet zařízení na uživatele je zaměřeného na zařízení. Například pokud *jdoe* a *jharnett* přihlášení k zařízení, samostatných registrací (DeviceID) se vytvoří pro každý z nich ve **uživatele** Karta informace.  

- Počáteční registraci / připojení k zařízení umožňují provést pokus o přihlášení nebo uzamčení nebo odemčení. Může dojít k 5 minut, než aktivované úloh služby Plánovač úloh. 

- Pro zařízení na kartě informace o uživateli z důvodu přeinstalaci operačního systému nebo ruční opětovná registrace můžete získat více položek. 

- Ujistěte se, že [KB4284842](https://support.microsoft.com/help/4284842) je nainstalována v případě Windows 7 SP1 nebo Windows Server 2008 R2 SP1. Tato aktualizace brání selhání budoucích ověřování kvůli ztrátám zákazníka přístup k chráněné klíče po změně hesla.

## <a name="step-1-retrieve-the-registration-status"></a>Krok 1: Načíst stav registrace 

**Pokud chcete ověřit stav registrace:**  

1. Přihlásit se pomocí uživatelského účtu, který se má provést připojení k Azure AD hybridní.

2. Otevřete příkazový řádek jako správce 

3. Typ `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe" /i`

Tento příkaz zobrazí dialogové okno, které vám poskytne další podrobnosti o stavu připojení.

![Připojení k pracovní ploše pro Windows](./media/troubleshoot-hybrid-join-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>Krok 2: Vyhodnoťte hybridní stav připojení k Azure AD 

Pokud připojení k hybridní službě Azure AD nebylo úspěšné, dialogové okno vám poskytuje podrobnosti o problému, který došlo k chybě.

**Nejčastější problémy jsou:**

- Nesprávně nakonfigurované služby AD FS nebo služby Azure AD

    ![Připojení k pracovní ploše pro Windows](./media/troubleshoot-hybrid-join-windows-legacy/02.png)

- Nejste přihlášení jako uživatel domény

    ![Připojení k pracovní ploše pro Windows](./media/troubleshoot-hybrid-join-windows-legacy/03.png)
    
    Existuje několik různých důvodů, proč tato situace může nastat:
    
    - Přihlášený uživatel není uživatelem domény (například místní uživatel). Hybridní připojení k Azure AD na zařízeních s nižší úrovně je podporována pouze pro uživatele domény.
    
    - Autoworkplace.exe nedokáže bezobslužné ověření pomocí Azure AD nebo AD FS. To může způsobovat odesílací vázané problém s připojením k adresám URL Azure AD. Také je možné, že vícefaktorové ověřování (MFA) je povolený a nakonfigurovaný pro tohoto uživatele a WIAORMUTLIAUTHN není nakonfigurovaná na federační server. Další možností je, že tuto stránku zjišťování domovské sféry čeká pro interakci s uživatelem, což zabrání **autoworkplace.exe** tiše vyžádat token.
    
    - Vaše organizace používá Azure AD bezproblémové jednotné přihlašování, `https://autologon.microsoftazuread-sso.com` nebo `https://aadg.windows.net.nsatc.net` nejsou k dispozici v nastavení aplikace Internet Explorer intranetu zařízení, a **povolit aktualizace stavového řádku prostřednictvím skriptu** není povolená pro zónu intranetu.

- Bylo dosaženo kvóty

    ![Připojení k pracovní ploše pro Windows](./media/troubleshoot-hybrid-join-windows-legacy/04.png)

- Služba nereaguje 

    ![Připojení k pracovní ploše pro Windows](./media/troubleshoot-hybrid-join-windows-legacy/05.png)

Informace o stavu můžete také najít v protokolu událostí v: **aplikací a služeb Log\Microsoft-Workplace Join**
  
**Nejběžnější příčiny selhání hybridní službě Azure AD join jsou:** 

- Počítač není připojený k interní síti vaší organizace nebo k síti VPN s připojením k místní řadič domény služby AD.

- Jste přihlášeni k počítači pomocí účtu místního počítače. 

- Problémy s konfigurací služby: 

  - Federační server nakonfigurovaný pro podporu **WIAORMULTIAUTHN**. 

  - Doménová struktura počítače nemá žádný bod připojení služby objekt, který odkazuje na název ověřené domény ve službě Azure AD 

  - Uživatel dosáhl limitu počtu zařízení. 

## <a name="next-steps"></a>Další postup

Dotazy, najdete v článku [nejčastější dotazy ke správě zařízení](faq.md)  
