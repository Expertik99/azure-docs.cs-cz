---
title: Aktualizace služby Azure Stack. 1808 | Dokumentace Microsoftu
description: Další informace o co je nového v aktualizaci. 1808 pro integrované systémy Azure Stack, včetně známých problémů a kde se stáhnout aktualizaci.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: f9973f2099ed5c6dda38c6569f42995b6903ef5e
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162406"
---
# <a name="azure-stack-1808-update"></a>Aktualizace služby Azure Stack. 1808

*Platí pro: integrované systémy Azure Stack*

Tento článek popisuje obsah balíčku. 1808 aktualizace. Balíček aktualizace zahrnuje vylepšení, oprav a známé problémy pro tuto verzi sady Azure Stack. Tento článek také obsahuje odkaz, takže si můžete stáhnout aktualizace. Známé problémy jsou rozděleny do problémy přímo souvisí s proces aktualizace a problémy se sestavením (po instalaci).

> [!IMPORTANT]  
> Tento balíček aktualizace je pouze pro integrované systémy Azure Stack. Tento balíček aktualizace nevztahují na Azure Stack Development Kit.

## <a name="build-reference"></a>Referenční informace o buildu

Je číslo sestavení aktualizace. Azure Stack 1808 **1.1808.0.97**.  

### <a name="new-features"></a>Nové funkce

Tato aktualizace zahrnuje následující vylepšení pro službu Azure Stack.

- <!--  2682594   | IS  -->  **Všechna prostředí Azure Stack teď použijte formát časového pásma koordinovaný univerzální čas (UTC).**  Všechny protokoly dat a související informace nyní zobrazuje ve formátu UTC. Při aktualizaci z předchozí verze, který nebyl nainstalován pomocí času UTC, vaše prostředí se aktualizuje a pomocí času UTC. 

- <!-- 2437250  | IS  ASDK --> **Spravované disky se podporují.** Teď můžete použít Managed Disks v Azure stacku virtuální počítače a škálovací sady virtuálních počítačů. Další informace najdete v tématu [Azure Stack Managed Disks: rozdíly a aspekty](/azure/azure-stack/user/azure-stack-managed-disk-considerations).

- <!-- 2563799  | IS  ASDK -->  **Azure Monitor**. Jako je Azure Monitor v Azure Azure Monitor ve službě Azure Stack poskytuje základní infrastruktura metriky a protokoly pro většina služeb. Další informace najdete v tématu [Azure Monitor ve službě Azure Stack](/azure/azure-stack/user/azure-stack-metrics-azure-data).

- <!-- 2487932| IS -->  **Příprava hostitelském rozšíření**. Hostitel rozšíření můžete použít k zabezpečení služby Azure Stack pomůže snížením počtu požadované porty TCP/IP. S aktualizací. 1808 můžete připravit, Příprava služby Azure Stack pro rozšíření hostitele. Další informace najdete v tématu [Příprava hostitele rozšíření pro službu Azure Stack](/azure/azure-stack/azure-stack-extension-host-prepare).

- <!-- IS --> **Položky galerie pro Škálovací sady virtuálních počítačů jsou teď integrované**.  Položka galerie Virtual Machine Scale Sets je teď k dispozici na portálech uživatele a správce bez nutnosti ho můžete stáhnout.  Pokud upgradujete na. 1808 je k dispozici po dokončení upgradu.  

- <!-- IS, ASDK --> **Škálovací sada virtuálních počítačů škálování**.  Můžete použít portál pro [škálování Škálovací sady virtuálních počítačů](azure-stack-compute-add-scalesets.md#scale-a-virtual-machine-scale-set) (VMSS).    

- <!-- 2489570 | IS ASDK--> **Podpora pro vlastní konfigurace zásad IPSec/IKE** pro [bran VPN Gateway ve službě Azure Stack](/azure/azure-stack/azure-stack-vpn-gateway-about-vpn-gateways).


 ### <a name="fixed-issues"></a>Opravené problémy
- <!-- IS ASDK--> Opravili jsme problém pro vytvoření dostupnosti na portálu, což vedlo sada domény selhání a aktualizační doména 1. 

- <!-- IS ASDK --> Nastavení škálování škálovací sady virtuálních počítačů jsou teď dostupné na portálu.  

- <!-- 2494144- IS, ASDK --> Nyní je vyřešen problém, který zabránil některé velikosti virtuálních počítačů řady F-series povolí při výběru velikosti virtuálního počítače pro nasazení. 

- <!-- IS, ASDK --> Vylepšení výkonu při vytváření virtuálních počítačů a další optimalizované pomocí základního úložiště.

- **Různé opravy** výkonu, stability, zabezpečení a operační systém, který se používá ve službě Azure Stack.


### <a name="changes"></a>Změny
- <!-- 1697698  | IS, ASDK --> *Kurzy rychlý Start* v na řídicím panelu portálu teď odkaz na související články v online dokumentaci k Azure Stack.

- <!-- 2515955   | IS ,ASDK--> *Všechny služby* nahradí *další služby* na portálech správce a uživatele Azure stacku. Teď můžete použít *všechny služby* jako alternativu k navigaci na portálech Azure Stack stejným způsobem jako v Azure Portal.

- <!--  TBD – IS, ASDK --> *Základní A* velikostí virtuálních počítačů byly ukončeny pro [vytvoření škálovací sady virtuálních počítačů](azure-stack-compute-add-scalesets.md) (VMSS) prostřednictvím portálu. Pokud chcete vytvořit VMSS se tato velikost, pomocí Powershellu nebo šablony.  

### <a name="common-vulnerabilities-and-exposures"></a>Common Vulnerabilities and Exposures

Tuto aktualizaci nainstaluje následující aktualizace:  

- [CVE-2018-0952](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-0952)
- [CVE-2018-8200](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8200)
- [CVE-2018-8204](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8204)
- [CVE-2018-8253](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8253)
- [CVE-2018-8339](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8339)
- [CVE-2018-8340](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8340)
- [CVE-2018-8341](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8341)
- [CVE-2018-8343](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8343)
- [CVE-2018-8344](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8344)
- [CVE-2018-8345](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8345)
- [CVE-2018-8347](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8347)
- [CVE-2018-8348](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8348)
- [CVE-2018-8349](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8349)
- [CVE-2018-8394](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8394)
- [CVE-2018-8398](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8398)
- [CVE-2018-8401](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8401)
- [CVE-2018-8404](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8404)
- [CVE-2018-8405](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8405)
- [CVE-2018-8406](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8406)  

Další informace o těchto ohrožení zabezpečení, klikněte na výše uvedené odkazy nebo článku znalostní báze Microsoft Knowledge základní [4343887](https://support.microsoft.com/help/4343887).

Tato aktualizace obsahuje také ke zmírnění chyby zabezpečení spekulativního spouštění na straně kanálu říká terminál selhání L1 (L1TF), je popsáno v [Microsoft Security Advisory ADV180018](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180018).  

### <a name="prerequisites"></a>Požadavky

- Instalace služby Azure Stack [1807 aktualizovat](azure-stack-update-1807.md) před instalací aktualizace. 1808 Azure Stack. 

  > [!TIP]  
  > Předplatit následující *RRS* nebo *Atom* kanály, držet krok s Azure Stack opravy hotfix:
  > - RRS: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss ... 
  > - Atom: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom ...


- Před instalací této aktualizace, spusťte [testovací AzureStack](azure-stack-diagnostic-test.md) s následujícími parametry do ověřte stav služby Azure Stack a vyřešte všechny provozní problémy zjištěné, včetně všech upozornění a chyby. Také aktivní výstrahy můžete zkontrolovat a vyřešit všechny, které vyžadují nějakou akci.  

  ```PowerShell
  Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
  ``` 

### <a name="known-issues-with-the-update-process"></a>Známé problémy s proces aktualizace

- <!-- 2468613 - IS --> Při instalaci této aktualizace, může se zobrazit upozornění s názvem *chyba – šablona pro typ FaultType UserAccounts.New chybí.*  Tyto výstrahy můžete bezpečně ignorovat. Tyto výstrahy se automaticky zavře po dokončení instalace této aktualizace.

- <!-- 2489559 - IS --> Nepokoušejte se vytvářet virtuální počítače při instalaci této aktualizace. Další informace o správě aktualizací najdete v tématu [správy aktualizací ve službě Azure Stack přehled](azure-stack-updates.md#plan-for-updates).

- <!-- 2830461 - IS --> V některých případech při aktualizaci vyžaduje pozornost, odpovídající nemusí být vygenerována výstraha. Přesné stavu se stále projeví na portálu a není ovlivněn.

### <a name="post-update-steps"></a>Postup po aktualizaci

*Nejsou žádné kroky po aktualizaci pro aktualizaci. 1808.*

<!-- After the installation of this update, install any applicable Hotfixes. For more information view the following knowledge base articles, as well as our [Servicing Policy](azure-stack-servicing-policy.md).  
 - [Link to KB]()  
 -->

## <a name="known-issues-post-installation"></a>Známé problémy (po instalaci)

Toto jsou známé problémy této verze sestavení po instalaci.

### <a name="portal"></a>Portál
-  <!--  2873083 - IS ASDK --> Při použití na portálu vytvořit škálovací sadu virtuálních počítačů (VMSS), nastavte *velikost instance* rozevírací seznam nenačte správně při použití aplikace Internet Explorer. Chcete-li tento problém vyřešit, použijte jiný prohlížeč při použití portálu k vytvoření VMSS.  

- <!-- 2931230 – IS  ASDK --> Plány, které jsou přidány na předplatné uživatele jako doplňkový plán nelze odstranit, i když odebrat plán ze předplatné uživatele. Plán zůstane, dokud se také odstraní předplatné, které odkazují na doplňkový plán. 

- <!--2760466 – IS  ASDK --> Při instalaci nového prostředí Azure Stack, na kterém běží tato verze, upozornění, která informuje o *vyžadována aktivace* se nemusí zobrazit. [Aktivace](azure-stack-registration.md) se vyžaduje, abyste mohli používat marketplace syndikace.  

- <!-- TBD - IS ASDK --> Dva typy pro správu předplatného, které byly [představený poprvé ve verzi 1804](azure-stack-update-1804.md#new-features) by se neměly. Typy předplatného jsou **měření předplatné**, a **využití předplatného**. Tyto typy předplatného jsou viditelné v novým prostředím Azure Stack od verze 1804, ale ještě nejsou připravené k použití. By měla dál používat **výchozí zprostředkovatel** typu předplatného.

- <!-- TBD - IS --> Nemusí být možné zobrazit na portálu správce prostředků výpočetního výkonu a úložiště. Příčinou tohoto problému je chyba při instalaci aktualizace, která způsobí, že aktualizace, která se správně hlášené jako úspěšně dokončený. Pokud k tomuto problému dochází, obraťte se na zákaznické podpory Microsoftu o pomoc.

- <!-- TBD - IS --> Může se zobrazit prázdný řídicí panel portálu. Obnovit řídicí panel, vyberte ikonu ozubeného kolečka v pravém horním rohu portálu a pak vyberte **obnovit výchozí nastavení**.

- <!-- TBD - IS ASDK --> Odstraňuje se předplatné uživatele za následek osamocené prostředky. Jako alternativní řešení nejprve odstranit prostředky uživatele nebo celou skupinu prostředků a pak odstraňte předplatná uživatelů.

- <!-- TBD - IS ASDK --> Nelze zobrazit oprávnění k předplatnému pomocí na portálech Azure Stack. Jako alternativní řešení pomocí prostředí PowerShell mohl ověřit oprávnění.


### <a name="health-and-monitoring"></a>Monitorování stavu a
- <!-- 1264761 - IS ASDK -->  Může se zobrazit upozornění **stavu řadiče** komponenta, která mají následující podrobnosti:  

   Upozornění #1:
   - Název: Infrastrukturu role není v pořádku
   - ZÁVAŽNOST: upozornění
   - KOMPONENTY: Kontroler stavu
   - Popis: Kontroler stavu prezenčního signálu skener není k dispozici. To může mít vliv sestav o stavu a metriky.  

  Upozornění #2:
   - Název: Infrastrukturu role není v pořádku
   - ZÁVAŽNOST: upozornění
   - KOMPONENTY: Kontroler stavu
   - Popis: Stav řadiče skener selhání není k dispozici. To může mít vliv sestav o stavu a metriky.

  Obě výstrahy můžete bezpečně ignorovat a bude automaticky zavřít v čase.  


- <!-- 2812138 | IS --> Může se zobrazit upozornění pro **úložiště** komponenta, která mají následující podrobnosti:

   - Název: Chyba interní komunikace se službou Storage  
   - ZÁVAŽNOST: kritické  
   - KOMPONENTA: úložiště  
   - Popis: Chyba interní komunikace se službou úložiště došlo k chybě při odesílání požadavků na tyto uzly.  

    Upozornění můžete ignorovat, ale budete muset ručně zavřete výstrahu.

- <!-- 2368581 - IS. ASDK --> Operátory Azure stacku, pokud se zobrazí upozornění na nedostatek paměti a virtuální počítače klientů selhání nasazení **Chyba při vytváření virtuálních počítačů Fabric**, je možné, že toto razítko Azure Stack je nedostatek dostupné paměti. Použití [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) nejlépe pochopit dostupné kapacity pro vaše úlohy.


### <a name="compute"></a>Compute
- <!-- 2869209 – IS, ASDK --> Při použití [ **přidat AzsPlatformImage** rutiny](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), je nutné použít **- OsUri** parametr jako identifikátor URI, kde je odeslána na disk účtu úložiště. Pokud používáte místní cesta na disku, rutina selže s následující chybou: *dlouho běžící operace se nezdařila se stavem "Failed"*. 

- <!--  2966665 – IS, ASDK --> Připojení SSD spravovaných disků úrovně premium velikosti spravovaného disku virtuální počítače (DS, DSv2, Fs, Fs_V2) selže s chybou: *nepovedlo se aktualizovat disky pro virtuální počítač 'vmname' Chyba: Požadovaná operace nejde provést, protože typ účtu úložiště. Premium_LRS' není podporována pro velikost virtuálního počítače "Standard_DS/Ds_V2/FS/Fs_v2)*

   Chcete-li tento problém obejít, použijte *Standard_LRS* datových disků místo *Premium_LRS disky*. Použití *Standard_LRS* datových disků nedojde ke změně vstupně-výstupních operací nebo fakturační náklady. 

- <!--  2795678 – IS, ASDK --> Při použití portálu k vytvoření virtuálních počítačů (VM) o velikosti virtuálních počítačů úrovně premium (DS, Ds_v2, služby FS, FSv2) virtuální počítač se vytvoří v účtu úložiště úrovně standard. Vytvoření účtu úložiště úrovně standard neovlivňuje funkčně vstupně-výstupních operací, nebo fakturace. 

   Můžete bezpečně ignorovat upozornění, že: *jste se rozhodli použít standardní disk o velikosti, která podporuje prémiové disky. To může mít vliv na výkon operačního systému a nedoporučuje se používat. Zvažte raději použití storage úrovně premium (SSD).*

- <!-- 2967447 - IS, ASDK --> Škálovací sada virtuálních počítačů (VMSS) vytvořte prostředí založené na CentOS 7.2 poskytuje jako možnost pro nasazení. Vzhledem k tomu, že obrázek není k dispozici ve službě Azure Stack, vyberte jiný operační systém pro vaše nasazení nebo pomocí šablony ARM zadáním jiné image CentOS, který byl stažen před jejich nasazením na Marketplace operátorem.  

- <!-- 2724873 - IS --> Při použití rutiny Powershellu **Start AzsScaleUnitNode** nebo **Stop-AzsScaleunitNode** ke správě jednotek škálování, první pokus o spuštění nebo zastavení jednotka škálování může selhat. Pokud se rutina nezdaří při prvním spuštění, spusťte rutinu znovu. Druhé spuštění by měl úspěšně dokončit operaci. 

- <!-- TBD - IS ASDK --> Při vytváření virtuálních počítačů na portálu user portal Azure Stack na portálu zobrazuje nesprávný počet datových disků, které můžete připojit k DS-series virtuálních počítačů. Virtuálních počítačů řady DS může obsahovat libovolný počet datových disků jako konfiguraci Azure.

- <!-- TBD - IS ASDK --> Pokud zřizování rozšíření na nasazení virtuálního počítače trvá příliš dlouho, uživatelé měli nechat zřizování vypršení časového limitu namísto pokusu o zastavení procesu uvolnění nebo odstranění virtuálního počítače.  

- <!-- 1662991 IS ASDK --> Diagnostika virtuálních počítačů Linux není podporována ve službě Azure Stack. Při nasazení virtuálního počítače s Linuxem s povolenou diagnostikou virtuálního počítače, nasazení se nezdaří. Nasazení se také nezdaří, pokud povolíte základní metriky virtuálního počítače s Linuxem prostřednictvím nastavení diagnostiky.  

- <!-- 2724961- IS ASDK --> Když se zaregistrujete **Microsoft.Insight** poskytovatele prostředků v nastavení předplatného a vytvoření virtuálního počítače s Windows pomocí hostovaného operačního systému diagnostických povolena, graf procento využití procesoru na stránce Přehled virtuálního počítače nebudou moci zobrazit data metrik.

   Graf procento využití procesoru pro virtuální počítač, najdete **metriky** blade a zobrazit všechny podporované Windows virtuálního počítače hosta metriky.



### <a name="networking"></a>Sítě  

- <!-- 1766332 - IS ASDK --> V části **sítě**, vyberete-li **vytvořit bránu VPN** nastavit připojení k síti VPN, **založenou na zásadách** je uveden jako typ sítě VPN. Nevybírejte tuto možnost. Pouze **trasy na základě** možnost je podporovaná ve službě Azure Stack.

- <!-- 1902460 - IS ASDK --> Azure Stack podporuje jediný *bránu místní sítě* na IP adresu. To platí ve všech předplatných tenanta. Po vytvoření první místní síťové brány připojení, následné pokusech o vytvoření prostředku brány místní sítě pomocí stejné IP adresy jsou zablokované.

- <!-- 16309153 - IS ASDK --> Ve virtuální síti, který byl vytvořen s nastavením serveru DNS *automatické*, změna vlastní selhání serveru DNS. Aktualizované nastavení nenasdílí do virtuálních počítačů v dané virtuální síti.

- <!-- 2702741 -  IS ASDK --> Chcete-li být zachována po zastavte a Navraťte vydání není zaručena veřejné IP adresy, které jsou nasazeny pomocí metody dynamického přidělení.

- <!-- 2529607 - IS ASDK --> Během Azure Stack *tajný klíč otočení*, je období, ve kterém veřejné IP adresy nedostupné přibližně 2 až 5 minut.

-   <!-- 2664148 - IS ASDK --> Ve scénářích, kde tenanta je přístup ke své virtuální počítače s použitím tunelu S2S VPN kterými se mohou setkat scénář, kde pokusy o připojení selhat, pokud místní podsítě byl přidán do místní síťové brány po již vytvořit bránu. 


<!-- ### SQL and MySQL-->


### <a name="app-service"></a>App Service

- <!-- 2352906 - IS ASDK --> Uživatelé musí registrovat poskytovatele prostředků úložiště, dřív, než vytvoří jejich první funkce Azure v rámci předplatného.

- <!-- 2489178 - IS ASDK --> Aby bylo možné horizontální navýšení kapacity infrastruktury (pracovních procesů, správy, role front-endu), musíte použít PowerShell, jak je popsáno v poznámkách k verzi pro výpočetní prostředky.



### <a name="usage"></a>Využití  
- <!-- TBD - IS ASDK --> Data o využití veřejné IP adresy využití měřičů zobrazuje stejné *EventDateTime* hodnotu pro každý záznam místo *TimeDate* razítko, které ukazuje, kdy se záznam vytvořil. V současné době nelze pomocí těchto dat pro monitorování přesné použití veřejné IP adresy.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>Stáhnout aktualizaci.
Můžete stáhnout aktualizace balíčku. Azure Stack 1808 z [tady](https://aka.ms/azurestackupdatedownload).
  

## <a name="next-steps"></a>Další postup
- Zásady údržby pro integrované systémy Azure Stack, a co musíte udělat, aby byl váš systém v podporovaném stavu najdete v tématu [Azure Stack zásady obsluhy](azure-stack-servicing-policy.md).  
- Privilegované koncový bod (období) použít ke sledování a obnovit aktualizace, najdete v článku [monitorování aktualizací ve službě Azure Stack pomocí privilegovaných koncového bodu](azure-stack-monitor-update.md).  
- Přehled správy aktualizací ve službě Azure Stack najdete v tématu [správy aktualizací ve službě Azure Stack přehled](azure-stack-updates.md).  
- Další informace o tom, jak použít aktualizace pomocí služby Azure Stack najdete v tématu [použití aktualizací ve službě Azure Stack](azure-stack-apply-updates.md).  
