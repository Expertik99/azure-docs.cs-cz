---
title: Řešení potíží s Enterprise State Roaming nastavení v Azure Active Directory | Dokumentace Microsoftu
description: Poskytuje odpovědi na některé dotazy, které správce IT může mít informace o nastavení a synchronizace dat aplikace.
services: active-directory
keywords: Enterprise stavu cestovní nastavení cloudu systému windows, nejčastější dotazy k enterprise stav roamingu
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.component: devices
ms.assetid: f45d0515-99f7-42ad-94d8-307bc0d07be5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: markvi
ms.reviewer: tanning
ms.custom: it-pro
ms.openlocfilehash: c7a2428e4e5e3b5af0e9e01514ba433707e6a3c8
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "44022794"
---
# <a name="troubleshooting-enterprise-state-roaming-settings-in-azure-active-directory"></a>Řešení potíží s Enterprise State Roaming nastavení v Azure Active Directory

Toto téma obsahuje informace o tom, jak řešení potíží a diagnostiku problémů s Enterprise State Roaming a poskytuje seznam známých problémů.

## <a name="preliminary-steps-for-troubleshooting"></a>Přípravné kroky pro řešení potíží 
Předtím, než se pustíte do odstraňování, ověřte, že uživatele a zařízení správně nakonfigurován a že jsou splněny všechny požadavky sady Enterprise State Roaming podle zařízení a uživatele. 

1. Windows 10 s nejnovější aktualizací a minimální verze 1511 (sestavení operačního systému 10586 nebo novější) je nainstalována v zařízení. 
2. Azure AD je zařízení připojené, nebo připojené k hybridní službě Azure AD. Další informace najdete v tématu [získání zařízení pod kontrolu služby Azure AD](device-management-introduction.md).
3. Ujistěte se, že **Enterprise State Roaming** je povolená pro příslušného tenanta Azure AD jak je popsáno v [povolit Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-enable.md). Můžete povolit roaming pro všechny uživatele nebo pouze vybrané skupiny uživatelů.
4. Uživatel musí již být přiřazenou licenci Azure Active Directory Premium.  
25. Po restartování zařízení a uživatele musíte se přihlásit znovu pro přístup k funkcím Enterprise State Roaming.

## <a name="information-to-include-when-you-need-help"></a>Informace, které byste měli zahrnout při nápovědy
Pokud nelze vyřešit vaše potíže s níže uvedeném doprovodném materiálu, můžete kontaktovat našimi techniky podpory. Pokud je kontaktovat, uveďte následující informace:

* **Obecný popis chyby**: existují chybové zprávy pro uživatele viditelný? Pokud se žádná chybová zpráva, popište neočekávané chování, které jste si všimli, podrobně. Jaké funkce jsou povolené pro synchronizaci a co je uživatel očekává synchronizovat? Nejsou synchronizaci více funkcí nebo je izolovaná na jednu?
* **Ovlivnění uživatelé** – je synchronizace pracovní/selhání pro jeden nebo více uživateli? Kolik zařízení se podílejí na uživatele? Všechny z nich nesynchronizuje, nebo některé z nich synchronizace a některé nesynchronizuje?
* **Informace o uživateli** – co je identita uživatele pomocí pro přihlášení k zařízení? Jak uživatel přihlašuje k zařízení? Jsou součástí vybrané skupiny zabezpečení můžou synchronizovat? 
* **Informace o zařízení** – je toto zařízení Azure AD připojen nebo připojené k doméně? Jaké sestavení je na zařízení? Co jsou nejnovější aktualizace?
- **Datum / čas / časové pásmo** – co byl přesné datum a čas jste viděli chybu (včetně časové pásmo)?

Tyto informace včetně pomáhá nám tak rychle vyřešit problém.

## <a name="troubleshooting-and-diagnosing-issues"></a>Řešení potíží a diagnostika problémů
Tato část poskytuje návrhy na řešení potíží a Diagnostikujte problémy související s Enterprise State Roaming.

## <a name="verify-sync-and-the-sync-your-settings-settings-page"></a>Ověření synchronizace a nastavení stránky "Synchronizovat nastavení" 

1. Po připojení k doméně, která je nakonfigurovaná k povolení Enterprise State Roaming svůj počítač s Windows 10, přihlaste se pomocí svého pracovního účtu. Přejděte na **nastavení** > **účty** > **nastavení synchronizace** a ověřte, zda jsou synchronizace a individuální nastavení a že horní části Stránka nastavení znamená, že se synchronizují pomocí svého pracovního účtu. Potvrzení o stejný účet slouží také jako účet přihlášení v **nastavení** > **účty** > **vaše informace**. 
2. Ověřte, že synchronizace funguje napříč více počítačů tím, že některé změny na původní počítač, jako je například přesun na hlavním panelu k pravému nebo hornímu okraji obrazovky. Podívejte se změny rozšíří do druhého počítače do pěti minut. 
  * Zamknutí a odemknutí obrazovky (Win + L) může pomoct aktivovat synchronizaci.
  * Jak Enterprise State Roaming je vázán na uživatelský účet a účet počítače musí být přihlásit na obou počítačích pro synchronizaci pro práci – stejný účet.

**Potenciální problém**: Pokud ovládací prvky **nastavení** stránky nejsou k dispozici, a zobrazí se zpráva "některé funkce Windows jsou k dispozici pouze pokud používáte účet Microsoft nebo pracovní účet." Tento problém může vzniknout pro zařízení, která jsou nastavena na být připojená k doméně a zaregistrované u služby Azure AD, ale zařízení nebyl dosud úspěšně ověřil do služby Azure AD. Možnou příčinou je, že zásady zařízení musí být použity, ale tato aplikace probíhá asynchronně a může zpozdit několik hodin. 

### <a name="verify-the-device-registration-status"></a>Ověřte stav registrace zařízení
Enterprise State Roaming vyžaduje zařízení k registraci v Azure AD. I když nejsou specifické pro Enterprise State Roaming, postupujte podle pokynů níže může pomoct potvrdit, že klienta pro Windows 10 je zaregistrován a potvrdit stav kryptografický otisk, adresa URL nastavení služby Azure AD, NGC a další informace.

1.  Otevřete příkazový řádek, který je pozastavené. Pokud chcete udělat ve Windows, otevřete Spouštěč spustit (Win + R) a zadejte "cmd" otevřete.
2.  Jakmile příkazový řádek se otevře, zadejte "*/status dsregcmd.exe*".
3.  Pro očekávaný výstup **AzureAdJoined** pole hodnota by měla být "Ano", **WamDefaultSet** pole hodnota by měla být "Ano" a **WamDefaultGUID** pole hodnota by měla být identifikátor GUID s "(Azure AD)" na konci.

**Potenciální problém**: **WamDefaultSet** a **AzureAdJoined** "Ne" mají hodnoty pole, zařízení je připojené k doméně a registrované v Azure AD i ne synchronizaci zařízení. Pokud je to zobrazeno, zařízení mohou muset počkat, než zásady a použít nebo ověřování pro zařízení se nezdařila při připojování ke službě Azure AD. Uživatel může mít počkat několik hodin pro zásadu použít. Při řešení potíží může zahrnovat odhlásit a znovu v opakování pokusu o automatickou registraci, nebo spouštění úloh v Plánovači úloh. V některých případech se systémem "*dsregcmd.exe /leave*" v okně příkazového řádku se zvýšenými oprávněními, restartování a opakovaným pokusem o registraci může pomoct s tímto problémem.


**Potenciální problém**: pole pro **SettingsUrl** je prázdný a není synchronizovaná zařízení. Uživatel může mít naposledy přihlásil k zařízení než Enterprise State Roaming bylo povoleno na portálu Azure Active Directory. Restartujte zařízení a jste přihlášení uživatele. Volitelně můžete na portálu, zkuste s správci IT zakázat a znovu povolit uživatele může synchronizovat nastavení a Data podnikových aplikací. Jednou obnovená, restartujte zařízení a jste přihlášení uživatele. Pokud to problém nevyřeší **SettingsUrl** může být prázdný, v případě certifikát chybná zařízení. V takovém případě spuštění "*dsregcmd.exe /leave*" v okně příkazového řádku se zvýšenými oprávněními, restartování a opakovaným pokusem o registraci může pomoct s tímto problémem.

## <a name="enterprise-state-roaming-and-multi-factor-authentication"></a>Enterprise State Roaming a ověřování službou Multi-Factor Authentication 
Za určitých podmínek Enterprise State Roaming může selhat synchronizaci dat, pokud je nakonfigurované ověřování Azure Multi-Factor Authentication. Další podrobnosti o těchto příznaků, naleznete v dokumentu podporu [KB3193683](https://support.microsoft.com/kb/3193683). 

**Potenciální problém**: Pokud zařízení nakonfigurován tak, aby ověřování službou Multi-Factor Authentication na portálu Azure Active Directory, může selhat pro synchronizaci nastavení při přihlašování k zařízení s Windows 10 pomocí hesla. Tento typ konfigurace ověřování službou Multi-Factor Authentication je určen k ochraně účet správce služby Azure. Správci mohou nadále moci synchronizovat po přihlášení k zařízení s Windows 10 s jejich Microsoft Passport pro pracovní PIN kód nebo pomocí služby Multi-Factor Authentication při přístupu k dalšími službami Azure, jako je Office 365.

**Potenciální problém**: synchronizace mohou selhat, pokud správce nakonfiguruje zásady podmíněného přístupu ověřování Active Directory Federation Services službou Multi-Factor Authentication a vyprší platnost přístupového tokenu v zařízení. Zajistěte, aby přihlášení a odhlášení pomocí Microsoft Passport pro Work PIN nebo dokončí ověření služby Multi-Factor Authentication při přístupu k dalšími službami Azure, jako je Office 365.

### <a name="event-viewer"></a>Prohlížeč událostí
Případě pokročilého odstraňování problémů, prohlížeče událostí umožňuje najít konkrétní chyby. Ty jsou popsány v následující tabulce. Události lze nalézt v prohlížeči událostí > protokoly aplikací a služeb > **Microsoft** > **Windows** > **SettingSync Azure** a souvisejícím s identitou problémy se synchronizací **Microsoft** > **Windows** > **AAD**.


## <a name="known-issues"></a>Známé problémy

### <a name="sync-does-not-work-on-devices-that-have-apps-side-loaded-using-mdm-software"></a>Synchronizaci nelze použít u zařízení, která mají aplikace zkušebně načtených pomocí příslušného softwaru MDM.

Má vliv na zařízení se systémem Windows 10 Anniversary Update (verze 1607). V prohlížeči událostí pod protokoly SettingSync Azure je často viděli 6013 ID události s chybou 80070259.

**Doporučená akce**  
Ujistěte se, že má klient Windows 10 v1607 23. srpna 2016 kumulativní aktualizace ([KB3176934](https://support.microsoft.com/kb/3176934) 14393.82 sestavení operačního systému). 

---

### <a name="internet-explorer-favorites-do-not-sync"></a>Oblíbené aplikace Internet Explorer se nesynchronizují

Má vliv na zařízení se systémem Windows 10. listopadu Update (verze 1511).

**Doporučená akce**  
Ujistěte se, že má klient v1511 Windows 10. července 2016 kumulativní aktualizace ([KB3172985](https://support.microsoft.com/kb/3172985) 10586.494 sestavení operačního systému).

---

### <a name="theme-is-not-syncing-as-well-as-data-protected-with-windows-information-protection"></a>Motiv nesynchronizuje, a také data chráněných službou Windows Information Protection 

Aby se zabránilo úniku dat, data, která je pak chráněn rozhraním [Windows Information Protection](https://technet.microsoft.com/itpro/windows/keep-secure/protect-enterprise-data-using-wip) nebude synchronizovat prostřednictvím Enterprise State Roaming pro zařízení s Windows 10 Anniversary Update.



**Doporučená akce**  
Žádné. Tento problém může vyřešit budoucí aktualizace Windows.

---

### <a name="date-time-and-region-settings-do-not-sync-on-domain-joined-device"></a>Nastavení data, času a oblasti se nesynchronizují na zařízení připojeném k doméně 
  
Zařízení, která jsou připojená k doméně nedojde k synchronizaci nastavení datum, čas a oblasti: Automatický čas. Pomocí automatického času může přepsat další nastavení datum, čas a oblast a tato nastavení nejsou pro synchronizaci. 

**Doporučená akce**  
Žádné. 

---

### <a name="uac-prompts-when-syncing-passwords"></a>Nástroj Řízení uživatelských účtů vyzve při synchronizaci hesel

Má vliv na zařízení se systémem Windows 10. listopadu Update (verze 1511) s bezdrátové síťové rozhraní, který je nakonfigurovaný k synchronizaci hesel.

**Doporučená akce**  
Ujistěte se, že má klient Windows 10 v1511 kumulativní aktualizace ([KB3140743](https://support.microsoft.com/kb/3140743) 10586.494 sestavení operačního systému).

---

### <a name="sync-does-not-work-on-devices-that-use-smart-card-for-login"></a>Synchronizaci nelze použít u zařízení, která používají čipové karty pro přihlášení
Pokud se pokusíte přihlásit k zařízení Windows pomocí čipové karty nebo virtuální čipovou kartu, synchronizovat nastavení přestanou fungovat.     

**Doporučená akce**  
Žádné. Tento problém může vyřešit budoucí aktualizace Windows.

---

### <a name="domain-joined-device-is-not-syncing-after-leaving-corporate-network"></a>Zařízení připojená k doméně nesynchronizuje po opuštění podnikové sítě     
Zařízení připojených k doméně zaregistrovaný do služby Azure AD může docházet selhání synchronizace, pokud není zařízení připojené mimo pracoviště pro dlouhou dobu, a nemůže dokončit ověřování domény.

**Doporučená akce**  
Připojte zařízení k podnikové síti, aby synchronizace lze obnovit.

---

 ### <a name="azure-ad-joined-device-is-not-syncing-and-the-user-has-a-mixed-case-user-principal-name"></a>Azure AD připojeno zařízení nesynchronizuje a uživatel má smíšených velikosti písmen názvů hlavní název uživatele.
 Pokud je uživatel na zařízení připojeno k Azure AD, který se má upgradovat z Windows 10 sestavení 10586 na 14393 smíšené znakové hlavní název uživatele (například uživatelské jméno namísto uživatelského jména) má uživatel, zařízení uživatele může selhat pro synchronizaci. 

**Doporučená akce**  
Uživatel bude muset odebrat a znovu se připojit zařízení ke cloudu. Chcete-li to provést, přihlaste se jako místní správce a připojilo tak, že přejdete do **nastavení** > **systému** > **o** a vybrání možnosti "spravovat nebo odpojit se od práce nebo do školy ". Vyčistit zařízení znovu v níže uvedených souborů a potom připojení ke službě Azure AD **nastavení** > **systému** > **o** a výběrem možnosti "připojit k práci nebo Školní". Pokračujte v připojení k pracovišti ke službě Azure Active Directory a dokončete tok.

V kroku vyčištění, čištění následující soubory:
- Settings.dat v `C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\Settings\`
- Všechny soubory ve složce `C:\Users\<Username>\AppData\Local\Packages\Microsoft.AAD.BrokerPlugin_cw5n1h2txyewy\AC\TokenBroker\Account`

---

### <a name="event-id-6065-80070533-this-user-cant-sign-in-because-this-account-is-currently-disabled"></a>ID události 6065:80070533, které tento uživatel nemůže přihlásit, protože tento účet je aktuálně zakázaný.  
V prohlížeči událostí pod protokoly SettingSync/Debug může tato chyba zobrazí, pokud vypršela platnost přihlašovacích údajů uživatele. Kromě toho může dojít, když klient automaticky neměl AzureRMS zřízené. 

**Doporučená akce**  
V prvním případě požádejte uživatele, aktualizujte svoje přihlašovací údaje a přihlaste se na zařízení s novými přihlašovacími údaji. AzureRMS problém vyřešit, postupujte podle kroků uvedených v [KB3193791](https://support.microsoft.com/kb/3193791). 

---

### <a name="event-id-1098-error-0xcaa5001c-token-broker-operation-failed"></a>ID události. 1098: Chyba: 0xCAA5001C Token zprostředkovatele operace se nezdařila  
V prohlížeči událostí pod protokoly AAD/Operational tato chyba může zobrazit s událostí 1104: Asie a Tichomoří cloudu AAD modulu plug-in volání Get token vrátil chybu: 0xC000005F. K tomuto problému dochází, pokud jsou chybějící oprávnění nebo vlastnictví atributy.  

**Doporučená akce**  
Postupujte podle kroků uvedených [KB3196528](https://support.microsoft.com/kb/3196528).  

## <a name="related-topics"></a>Související témata
* [Cestovní přehled stavu Enterprise](active-directory-windows-enterprise-state-roaming-overview.md)
* [Povolit enterprise state roaming v Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md)
* [Nastavení a datovému roamingu – nejčastější dotazy](active-directory-windows-enterprise-state-roaming-faqs.md)
* [Skupina zásad a nastavení MDM pro nastavení synchronizace](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
* [Informace o cestovní nastavení Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
