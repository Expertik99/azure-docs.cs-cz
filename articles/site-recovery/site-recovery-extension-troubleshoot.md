---
title: 'Řešení potíží s selhání agenta Azure Site Recovery: hosta stavu agenta není k dispozici | Dokumentace Microsoftu'
description: Příznaky, příčiny a řešení Azure Site Recovery chyby týkající se agenta a rozšíření
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 07/06/2018
ms.author: asgang
ms.openlocfilehash: c0429e87f6c58ef2b9c7a268bee596d769e95910
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/09/2018
ms.locfileid: "37919800"
---
# <a name="troubleshoot-azure-site-recovery-extension-failures-issues-with-the-agent-or-extension"></a>Řešení potíží s chybami rozšíření Azure Site Recovery: problémy s agentů nebo rozšíření

Tento článek popisuje postup řešení potíží, které vám může pomoct vyřešit Azure Site Recovery chyby související s agentem virtuálního počítače a rozšíření.


## <a name="azure-site-recovery-extension-time-out"></a>Azure Site Recovery rozšíření časový limit  

Chybová zpráva: "provádění úkolu vypršel během operace rozšíření spustit sledování"<br>
Kód chyby: "151076"

 Azure Site Recovery nainstalovat rozšíření virtuálního počítače jako součást úlohu zapnutí ochrany. Některé z následujících podmínek by mohly bránit ochranu před se aktivuje a selhání úlohy. Dokončení následujících kroků pro řešení problémů a opakujte operaci:

**1. příčina: [agent je nainstalovaný ve virtuálním počítači, ale jeho reagovat (pro virtuální počítače s Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**2. příčina: [agent nainstalovaný na virtuálním počítači je zastaralý (pro virtuální počítače s Linuxem)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3. příčina: [rozšíření služby Site Recovery se nepodařilo aktualizovat nebo načíst](#the-site-recovery-extension-fails-to-update-or-load)**  

Chybová zpráva: "operace rozšíření site recovery předchozí trvá déle, než se očekávalo."<br>
Kód chyby: "150066"<br>

**1. příčina: [agent je nainstalovaný ve virtuálním počítači, ale jeho reagovat (pro virtuální počítače s Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**2. příčina: [agent nainstalovaný na virtuálním počítači je zastaralý (pro virtuální počítače s Linuxem)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**3. příčina: [stav rozšíření služby Site Recovery je nesprávný](#the-site-recovery-extension-fails-to-update-or-load)**  

## <a name="protection-fails-because-the-vm-agent-is-unresponsive"></a>Ochrana se nezdaří, protože agent virtuálního počítače nereaguje

Chybová zpráva: "provádění úkolu vypršel při sledování operace rozšíření spustit."<br>
Kód chyby: "151099"<br>

K této chybě může dojít, pokud na virtuálním počítači agent hosta Azure není v připraveném stavu.
Můžete zkontrolovat stav agenta hosta Azure v [webu Azure portal](https://portal.azure.com/). Přejděte na virtuální počítač, který se pokoušíte chránit a zkontrolovat stav "virtuální počítač > Nastavení > Vlastnosti > Stav agenta". Ve většině případů se stav agenta po restartování virtuálního počítače připraví. Ale pokud Restart není možné nebo jsou stále k problému, proveďte následující kroky pro řešení potíží.

**1. příčina: [agent je nainstalovaný ve virtuálním počítači, ale jeho reagovat (pro virtuální počítače s Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**2. příčina: [agent nainstalovaný na virtuálním počítači je zastaralý (pro virtuální počítače s Linuxem)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  


Chybová zpráva: "provádění úkolu vypršel při sledování operace rozšíření spustit."<br>
Kód chyby: "151095"<br>

Tato situace nastat, když je stará verze agenta na počítači s Linuxem. Proveďte následující krok odstraňování problémů.<br>
  **1. příčina: [agent nainstalovaný na virtuálním počítači je zastaralý (pro virtuální počítače s Linuxem)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
## <a name="causes-and-solutions"></a>Příčiny a řešení

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Agent je nainstalovaný na virtuálním počítači, ale přestane reagovat (pro virtuální počítače s Windows)

#### <a name="solution"></a>Řešení
Agent virtuálního počítače může být poškozený nebo služba zastavila. Opětovné instalaci agenta virtuálního počítače pomáhá získat nejnovější verzi. Pomáhá také restartovat komunikaci se službou.

1. Určení, zda "Windows službu agenta hosta Azure" je spuštěný ve virtuálním počítači službami (services.msc). Restartujte "Windows službu agenta hosta Azure".    
2. Pokud nejsou viditelná ve službách, v Ovládacích panelech, službu agenta hosta Azure pro Windows, přejděte na **programy a funkce** k určení, zda je nainstalována služba Windows agenta hosta.
4. Pokud agenta hosta Azure Windows se zobrazí v **programy a funkce**, odinstalujte agenta hosta Windows.
5. Stáhněte a nainstalujte [nejnovější verzi MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Musíte mít oprávnění správce k dokončení instalace.
6. Ověřte, že služby agenta hosta Azure pro Windows se zobrazí v služeb.
7. Restartujte úlohu ochrany.

Kromě toho ověřte, že [je nainstalované rozhraní Microsoft .NET 4.5](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) ve virtuálním počítači. Rozhraní .NET 4.5 je vyžadován pro agenta virtuálního počítače ke komunikaci se službou.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Agent nainstalovaný na virtuálním počítači je zastaralý (pro virtuální počítače s Linuxem)

#### <a name="solution"></a>Řešení
Většina souvisejících s agenty nebo souvisejících s rozšířením selhání pro virtuální počítače s Linuxem jsou způsobeny problémy, které ovlivňují zastaralé agenta virtuálního počítače. Chcete-li tento problém vyřešit, postupujte podle následujících obecných pokynů:

1. Postupujte podle pokynů pro [aktualizace agenta virtuálního počítače s Linuxem](../virtual-machines/linux/update-agent.md).

 > [!NOTE]
 > Jsme *důrazně doporučujeme* , že aktualizujete agenta pouze prostřednictvím distribuce úložiště. Nedoporučujeme stažením kódu agenta přímo z Githubu a aktualizacích. Pokud nejnovější verzi agenta pro vaši distribuci není k dispozici, požádejte distribuce podporu pokyny o tom, jak ji nainstalovat. Vyhledat nejnovější agent, přejděte [Windows Azure Linux agent](https://github.com/Azure/WALinuxAgent/releases) stránky v úložišti GitHub.

2. Ujistěte se, že Azure agent běží na virtuálním počítači spuštěním následujícího příkazu: `ps -e`

 Pokud proces není spuštěn, můžete ji restartujte pomocí následujících příkazů:

 * Pro Ubuntu: `service walinuxagent start`
 * Pro jiné distribuce: `service waagent start`

3. [Konfigurace automatického restartování agenta](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Povolení ochrany virtuálního počítače.



### <a name="the-site-recovery-extension-fails-to-update-or-load"></a>Rozšíření služby Site Recovery se nepodařilo aktualizovat nebo načíst
Pokud je stav rozšíření "prázdný", "NotReady" nebo přechod.

#### <a name="solution"></a>Řešení

Odinstalujte rozšíření a znovu spusťte operaci znovu.

Chcete-li odinstalovat rozšíření:

1. V [webu Azure portal](https://portal.azure.com/), přejděte k virtuálnímu počítači, na kterém dochází k selhání zálohování.
2. Vyberte **nastavení**.
3. Vyberte **rozšíření**.
4. Vyberte **obnovení rozšíření webu**.
5. Vyberte **odinstalovat**.

Pro virtuální počítač s Linuxem, pokud rozšíření VMSnapshot není uveden na webu Azure Portal [aktualizace agenta Azure Linux](../virtual-machines/linux/update-agent.md), a pak spusťte ochranu. 

Dokončení tohoto postupu způsobí, že rozšíření nutné přeinstalovat během ochrany.


