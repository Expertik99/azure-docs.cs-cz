---
title: Co je co-li nástroj podmíněného přístupu Azure Active Directory?
description: Zjistěte, jak můžete porozumět dopadu zásad podmíněného přístupu ve vašem prostředí.
services: active-directory
keywords: podmíněný přístup pro aplikace, podmíněný přístup s Azure AD, zabezpečený přístup k prostředkům společnosti, zásady podmíněného přístupu
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: d9bdc35e732a84920800424a260610fd6f068c94
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/08/2018
ms.locfileid: "39626086"
---
# <a name="what-is-the-what-if-tool-in-azure-active-directory-conditional-access"></a>Co je co-li nástroj podmíněného přístupu Azure Active Directory?

[Podmíněný přístup](../active-directory-conditional-access-azure-portal.md) je funkce služby Azure Active Directory (Azure AD), která umožňuje řídit jak Autorizovaní uživatelé přistupovat ke vašim cloudovým aplikacím. Jak víte, co můžete očekávat formuláře zásady podmíněného přístupu ve vašem prostředí? Na tuto otázku odpovědět, můžete použít **podmíněný přístup jak postupovat, pokud nástroj**.

Tento článek vysvětluje, jak můžete tento nástroj pro testování zásad podmíněného přístupu.

## <a name="what-it-is"></a>Co to je

**Podmíněný přístup citlivostní nástroj zásad** umožňuje na vědomí následky zásad podmíněného přístupu ve vašem prostředí. Místo test řízení zásad ručně provedením více přihlášení tento nástroj umožňuje vyhodnotit Simulovaná přihlášení uživatele. Simulace odhad dopadu tomto přihlášení na zásady a generuje sestavy simulace. Sestava neobsahuje pouze seznam použitých zásad správy zásad přístupu ale také [klasické zásady](policy-migration.md#classic-policies) Pokud existují.    

Co Pokud nástroje také poskytuje způsob, jak rychle určit zásady, které platí pro konkrétní uživatele. Informace, můžete použít například, pokud je potřeba vyřešit nějaký problém.  

## <a name="how-it-works"></a>Jak to funguje

V **podmíněný přístup jak postupovat, pokud nástroj**, je nutné nejprve nakonfigurovat nastavení chcete simulovat scénáře přihlášení. Tato nastavení zahrnují:

- Uživatel, kterého chcete testovat 

- Cloudové aplikace, které uživatel se pokusil získat přístup

- Podmínky, za které přístup k nakonfiguruje cloudové aplikace se provádí
     
V dalším kroku můžete zahájit spuštění simulace, které vyhodnotí nastavení. Pouze zásady, které jsou povoleny jsou součástí spustit zkušební verzi.


Po dokončení hodnocení, nástroj vygeneruje sestavu ovlivněné zásady.


> [!NOTE]
> V současné době co Pokud nástroj nepodporuje vnořené skupiny. Pokud je uživatel ve skupině a v této skupině je členem jiné skupiny, který se používá v zásadách podmíněného přístupu, pak bude postupovat v případě nástroj nezobrazuje správně vliv těchto zásad pro uživatele. 


## <a name="running-the-tool"></a>Spuštění nástroje

Můžete najít **co když** nástroj **[podmíněného přístupu – zásady](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies)** stránky na webu Azure Portal.

Chcete-li spustit nástroj, na panelu nástrojů nad seznamem zásad, klikněte na tlačítko **co když**.

![Co když](./media/what-if-tool/01.png)

Než budete moct spustit zkušební verzi, musíte nakonfigurovat nastavení.

## <a name="settings"></a>Nastavení

Tato část obsahuje informace o nastavení spuštění simulace.

![Co když](./media/what-if-tool/02.png)


### <a name="user"></a>Uživatel

Můžete vybrat pouze jednoho uživatele. Toto je jediné povinné pole.

### <a name="cloud-apps"></a>Cloudové aplikace

Výchozí nastavení pro toto nastavení je **všechny cloudové aplikace**. Ve výchozím nastavení provádí vyhodnocení všech dostupných zásad ve vašem prostředí. Můžete zúžit obor zásad by to mělo dopad konkrétních cloudových aplikací.


### <a name="ip-address"></a>IP adresa

IP adresa je jedna adresa IPv4 tak, aby napodoboval [podmínka umístění](location-condition.md). Adresa představuje internetových adres zařízení uživatele k přihlášení. Můžete ověřit IP adresa zařízení, například, že přejdete na [jaká je Moje IP adresa](https://whatismyipaddress.com).    

### <a name="device-platforms"></a>Platformy zařízení

Toto nastavení napodobuje [podmínku platformy zařízení](conditions.md#device-platforms) a představuje ekvivalent **všechny platformy (včetně nepodporovaných)**. 
### <a name="client-apps"></a>Klientské aplikace

Toto nastavení napodobuje [klientské aplikace podmínku](conditions.md#client-apps).
Ve výchozím nastavení, toto nastavení způsobí, že zkušební verzi všech zásad s **prohlížeče** nebo **mobilní aplikace a desktopoví klienti** buď jednotlivě nebo oba zaškrtnuto. Zjistí také zásady, které vynucují **protokolu Exchange ActiveSync (EAS)**. Toto nastavení můžete zúžit tak, že vyberete:

- **Prohlížeč** vyhodnotit všechny zásady s nejméně **prohlížeče** vybrané. 

- **Mobilní aplikace a desktopoví klienti** vyhodnotit všechny zásady s nejméně **mobilní aplikace a desktopoví klienti** vybrané. 


### <a name="sign-in-risk"></a>Riziko přihlášení

Toto nastavení napodobuje [rizika přihlašování podmínku](conditions.md#sign-in-risk).   


## <a name="evaluation"></a>Vyhodnocení 

Spustit zkušební verzi kliknutím **co když**. Výsledek vyhodnocení poskytuje sestavu, která se skládá ze: 

![Co když](./media/what-if-tool/03.png)

- Indikátor zda klasické zásady existují ve vašem prostředí
- Zásady, které platí pro vaše uživatele
- Zásady, které se nevztahují na vaše uživatele


Pokud [klasické zásady](policy-migration.md#classic-policies) neexistuje pro aplikace pro vybraný cloud, se zobrazí indikátor. Kliknutím na indikátor, budete přesměrováni na stránku pro klasické zásady. Na stránce klasické zásady můžete migrovat klasické zásady nebo ho jenom zakázat. Můžete se vrátit k vaší výsledek vyhodnocení podle zavřením tohoto dialogového okna.

V seznamu zásad, které se vztahují k vybranému uživateli, můžete také najít seznam [udělení ovládacích prvků](controls.md#grant-controls) a [relace](controls.md#session-controls) ovládací prvky, musí splňovat vaše uživatele.

V seznamu zásad, které se nevztahují na vaše uživatele můžete a také zjistit důvody, proč se tyto zásady nevztahují. U každé uvedené zásady z důvodu představuje první podmínku, která nebyla splněna. Možný důvod pro zásadu, která se nepoužije je zakázané zásady, protože dále se nevyhodnocují.   



## <a name="next-steps"></a>Další postup

- Pokud chcete vědět, jak nakonfigurovat zásady podmíněného přístupu najdete v tématu [vyžadovat vícefaktorové ověřování pro konkrétní aplikace s podmíněným přístupem Azure Active Directory](app-based-mfa.md).

- Pokud jste připraveni ke konfiguraci zásad podmíněného přístupu pro vaše prostředí, najdete v článku [osvědčené postupy pro podmíněný přístup ve službě Azure Active Directory](best-practices.md). 

- Pokud chcete migrovat klasické zásady, najdete v článku [migrace klasických zásad na webu Azure Portal](policy-migration.md)  
