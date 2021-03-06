---
title: Zpráva k vydání verze Microsoft Azure Stack Development Kit | Dokumentace Microsoftu
description: Vylepšení, oprav a známých problémů pro Azure Stack Development Kit.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
git ms.author: brenduns
ms.reviewer: misainat
ms.openlocfilehash: fe6be5a041b87af2323c7978c5371e326b3cd3d6
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44051268"
---
# <a name="azure-stack-development-kit-release-notes"></a>Zpráva k vydání verze Azure Stack Development Kit  
Tento článek obsahuje informace o vylepšení, oprav a známé problémy v Azure Stack Development Kit. Pokud si nejste jistí, kterou verzi používáte, můžete si [použití portálu ke kontrole](.\.\azure-stack-updates.md#determine-the-current-version).

> Udržujte si s tím, co se přihlásíte k odběru je novinkou ASDK [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [kanálu](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-11808097"></a>Sestavení 1.1808.0.97

### <a name="new-features"></a>Nové funkce
Toto sestavení obsahuje následující vylepšení a oprav pro Azure Stack.  

- <!-- 2682594   | ASDK  -->   **Všechna prostředí Azure Stack teď použijte formát časového pásma koordinovaný univerzální čas (UTC).**  Všechny protokoly dat a související informace nyní zobrazuje ve formátu UTC. 

- <!-- 2437250  | IS  ASDK --> **Spravované disky se podporují.** Teď můžete použít Managed Disks v Azure stacku virtuální počítače a škálovací sady virtuálních počítačů. Další informace najdete v tématu [Azure Stack Managed Disks: rozdíly a aspekty](/azure/azure-stack/user/azure-stack-managed-disk-considerations).
 
- <!-- 2563799  | IS  ASDK -->  **Azure Monitor**. Jako je Azure Monitor v Azure Azure Monitor ve službě Azure Stack poskytuje základní infrastruktura metriky a protokoly pro většina služeb. Další informace najdete v tématu [Azure Monitor ve službě Azure Stack](/azure/azure-stack/user/azure-stack-metrics-azure-data).

- <!-- ASDK --> **Položky galerie pro Škálovací sady virtuálních počítačů jsou teď integrované**.  Škálovací sada virtuálních počítačů položky galerie jsou teď k dispozici na portálech uživatele a správce bez nutnosti si je stáhnout. 

- <!-- IS, ASDK --> **Škálovací sada virtuálních počítačů škálování**.  Můžete použít portál pro [škálování Škálovací sady virtuálních počítačů](/azure/azure-stack/azure-stack-compute-add-scalesets.md#scale-a-virtual-machine-scale-set) (VMSS).   

- <!-- 2489570 | IS ASDK--> **Podpora pro vlastní konfigurace zásad IPSec/IKE** pro [bran VPN Gateway ve službě Azure Stack](/azure/azure-stack/azure-stack-vpn-gateway-about-vpn-gateways).


### <a name="fixed-issues"></a>Opravené problémy
- <!-- IS ASDK--> Opravili jsme problém pro vytvoření dostupnosti na portálu, což vedlo sada domény selhání a aktualizační doména 1.

- <!-- IS ASDK --> Nastavení škálování škálovací sady virtuálních počítačů jsou teď dostupné na portálu.  

- <!-- 2494144- IS, ASDK --> Nyní je vyřešen problém, který zabránil některé velikosti virtuálních počítačů řady F-series povolí při výběru velikosti virtuálního počítače pro nasazení. 

- <!-- IS, ASDK --> Vylepšení výkonu při vytváření virtuálních počítačů a další optimalizované pomocí základního úložiště.

- **Různé opravy** výkonu, stability, zabezpečení a operační systém, který se používá ve službě Azure Stack


### <a name="changes"></a>Změny
- <!-- 1697698  | IS, ASDK --> *Kurzy rychlý Start* v na řídicím panelu portálu teď odkaz na související články v online dokumentaci k Azure Stack.

- <!-- 2515955   | IS ,ASDK--> *Všechny služby* nahradí *další služby* na portálech správce a uživatele Azure stacku. Teď můžete použít *všechny služby* jako alternativu k navigaci na portálech Azure Stack stejným způsobem jako v Azure Portal.

- <!--  TBD – IS, ASDK --> *Základní A* velikostí virtuálních počítačů byly ukončeny pro [vytvoření škálovací sady virtuálních počítačů](.\.\azure-stack-compute-add-scalesets.md) (VMSS) prostřednictvím portálu. Pokud chcete vytvořit VMSS se tato velikost, pomocí Powershellu nebo šablony. 

### <a name="known-issues"></a>Známé problémy

#### <a name="portal"></a>Portál  
-  <!--  2873083 - IS ASDK --> Při použití na portálu vytvořit škálovací sadu virtuálních počítačů (VMSS), nastavte *velikost instance* rozevírací seznam nenačte správně při použití aplikace Internet Explorer. Chcete-li tento problém vyřešit, použijte jiný prohlížeč při použití portálu k vytvoření VMSS.  

- <!-- TBD  ASDK --> Výchozí časové pásmo pro všechna nasazení Azure Stack jsou nyní nastavení koordinovaný univerzální čas (UTC). Časové pásmo můžete vybrat při instalaci Azure Stack, ale automaticky přejde na čas UTC jako výchozí během instalace.

- <!-- 2931230 – IS  ASDK --> Plány, které jsou přidány na předplatné uživatele jako doplňkový plán nelze odstranit, i když odebrat plán ze předplatné uživatele. Plán zůstane, dokud se také odstraní předplatné, které odkazují na doplňkový plán. 

- <!--2760466 – IS  ASDK --> Při instalaci nového prostředí Azure Stack, na kterém běží tato verze, upozornění, která informuje o *vyžadována aktivace* se nemusí zobrazit. [Aktivace](.\.\azure-stack-registration.md) se vyžaduje, abyste mohli používat marketplace syndikace. 

- <!-- TBD - IS ASDK --> Dva typy pro správu předplatného, které byly [představený poprvé ve verzi 1804](.\.\azure-stack-update-1804.md#new-features) by se neměly. Typy předplatného jsou **měření předplatné**, a **využití předplatného**. Tyto typy předplatného jsou **měření předplatné**, a **využití předplatného**. Tyto typy předplatného jsou viditelné v novým prostředím Azure Stack od verze 1804, ale ještě nejsou připravené k použití. By měla dál používat **předplatné poskytovatele. výchozí** typu.

- <!-- TBD -  IS ASDK --> Odstraňuje se předplatné uživatele za následek osamocené prostředky. Jako alternativní řešení nejprve odstranit prostředky uživatele nebo celou skupinu prostředků a pak odstraňte předplatná uživatelů.

- <!-- TBD -  IS ASDK --> Nelze zobrazit oprávnění k předplatnému pomocí na portálech Azure Stack. Jako alternativní řešení pomocí prostředí PowerShell mohl ověřit oprávnění.



#### <a name="health-and-monitoring"></a>Monitorování stavu a
- <!-- 1264761 - IS ASDK -->  Může se zobrazit upozornění *stavu řadiče* komponenta, která mají následující podrobnosti:  

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

  Obě výstrahy můžete bezpečně ignorovat a bude automaticky ukončena v čase.  

- <!-- 2368581 - IS. ASDK --> Operátory Azure stacku, pokud se zobrazí upozornění na nedostatek paměti a virtuální počítače klientů selhání nasazení *Chyba při vytváření virtuálních počítačů Fabric*, je možné, že toto razítko Azure Stack je nedostatek dostupné paměti. Použití [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) nejlépe pochopit dostupné kapacity pro vaše úlohy.


#### <a name="compute"></a>Compute  
- <!-- 2869209 – IS, ASDK --> Při použití [ **přidat AzsPlatformImage** rutiny](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), je nutné použít **- OsUri** parametr jako identifikátor URI, kde je odeslána na disk účtu úložiště. Pokud používáte místní cesta na disku, rutina selže s následující chybou: *dlouho běžící operace se nezdařila se stavem "Failed"*. 

- <!--  2966665 – IS, ASDK --> Připojení SSD spravovaných disků úrovně premium velikosti spravovaného disku virtuální počítače (DS, DSv2, Fs, Fs_V2) selže s chybou: *nepovedlo se aktualizovat disky pro virtuální počítač 'vmname' Chyba: Požadovaná operace nejde provést, protože typ účtu úložiště. Premium_LRS' není podporována pro velikost virtuálního počítače "Standard_DS/Ds_V2/FS/Fs_v2)*

   Chcete-li tento problém obejít, použijte *Standard_LRS* datových disků místo *Premium_LRS disky*. Použití *Standard_LRS* datových disků nedojde ke změně vstupně-výstupních operací nebo fakturační náklady.  

- <!--  2795678 – IS, ASDK --> Při použití portálu k vytvoření virtuálních počítačů (VM) o velikosti virtuálních počítačů úrovně premium (DS, Ds_v2, služby FS, FSv2) virtuální počítač se vytvoří v účtu úložiště úrovně standard. Vytvoření účtu úložiště úrovně standard neovlivňuje funkčně vstupně-výstupních operací, nebo fakturace. 

   Můžete bezpečně ignorovat upozornění, že: *jste se rozhodli použít standardní disk o velikosti, která podporuje prémiové disky. To může mít vliv na výkon operačního systému a nedoporučuje se používat. Zvažte raději použití storage úrovně premium (SSD).*

- <!-- 2967447 - IS, ASDK --> Škálovací sada virtuálních počítačů (VMSS) vytvořte prostředí založené na CentOS 7.2 poskytuje jako možnost pro nasazení. Vzhledem k tomu, že obrázek není k dispozici ve službě Azure Stack, vyberte jiný operační systém pro vaše nasazení nebo pomocí šablony ARM zadáním jiné image CentOS, který byl stažen před jejich nasazením na Marketplace operátorem.

- <!-- TBD -  IS ASDK --> Nastavení škálování pro škálovací sady virtuálních počítačů nejsou k dispozici na portálu. Jako alternativní řešení můžete použít [prostředí Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Z důvodu rozdíly mezi verzemi prostředí PowerShell, je nutné použít `-Name` parametr namísto `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Při vytváření virtuálních počítačů na portálu user portal Azure Stack na portálu zobrazuje nesprávný počet datových disků, které můžete připojit virtuální počítač řady D series. Všechny podporované řady D series virtuálních počítačů může obsahovat libovolný počet datových disků jako konfiguraci Azure.

- <!-- TBD -  IS ASDK --> Selhání vytvoření image virtuálního počítače neúspěšné položky, který se nedá odstranit, mohou být přidány do okna výpočetní Image virtuálního počítače.

  Jako alternativní řešení vytvořte nové image virtuálního počítače s fiktivní virtuálního pevného disku, který je možné vytvářet přes Hyper-V (nový virtuální pevný disk-C:\dummy.vhd cesta-oprava SizeBytes – 1 GB). Tento proces by měla vyřešit problém, který zabraňuje odstranění položky. Pak 15 minut po vytvoření fiktivní image můžete úspěšně ho odstranit.

  Potom můžete opakovat stahování image virtuálního počítače, která původně selhala.

- <!-- TBD -  IS ASDK --> Pokud zřizování rozšíření na nasazení virtuálního počítače trvá příliš dlouho, uživatelé měli nechat zřizování vypršení časového limitu namísto pokusu o zastavení procesu uvolnění nebo odstranění virtuálního počítače.  

- <!-- 1662991 - IS ASDK --> Diagnostika virtuálních počítačů Linux není podporována ve službě Azure Stack. Při nasazení virtuálního počítače s Linuxem s povolenou diagnostikou virtuálního počítače, nasazení se nezdaří. Nasazení se také nezdaří, pokud povolíte základní metriky virtuálního počítače s Linuxem prostřednictvím nastavení diagnostiky.

- <!-- 2724961- IS ASDK --> Když se zaregistrujete **Microsoft.Insight** poskytovatele prostředků v nastavení předplatného a vytvoření virtuálního počítače s Windows pomocí hostovaného operačního systému diagnostických povolena, graf procento využití procesoru na stránce Přehled virtuálního počítače nebudou moci zobrazit data metrik.
 
  Graf procento využití procesoru pro virtuální počítač, najdete **metriky** blade a zobrazit všechny podporované Windows virtuálního počítače hosta metriky.

 

#### <a name="networking"></a>Sítě
- <!-- 1766332 - IS, ASDK --> V části **sítě**, vyberete-li **vytvořit bránu VPN** nastavit připojení k síti VPN, **založenou na zásadách** je uveden jako typ sítě VPN. Nevybírejte tuto možnost. Pouze **trasy na základě** možnost je podporovaná ve službě Azure Stack.

- <!-- 1902460 -  IS ASDK --> Azure Stack podporuje jediný *bránu místní sítě* na IP adresu. To platí ve všech předplatných tenanta. Po vytvoření první místní síťové brány připojení, následné pokusech o vytvoření prostředku brány místní sítě pomocí stejné IP adresy jsou zablokované.

- <!-- 16309153 -  IS ASDK --> Ve virtuální síti, který byl vytvořen s nastavením serveru DNS *automatické*, změna vlastní selhání serveru DNS. Aktualizované nastavení nenasdílí do virtuálních počítačů v dané virtuální síti.

- <!-- 2702741 -  IS ASDK --> Chcete-li být zachována po zastavte a Navraťte vydání není zaručena veřejné IP adresy, které jsou nasazeny pomocí metody dynamického přidělení.

- <!-- 2529607 - IS ASDK --> Během Azure Stack *tajný klíč otočení*, je období, ve kterém veřejné IP adresy nedostupné přibližně 2 až 5 minut.

-   <!-- 2664148 - IS ASDK --> Ve scénářích, kde tenanta je přístup ke své virtuální počítače s použitím tunelu S2S VPN kterými se mohou setkat scénář, kde pokusy o připojení selhat, pokud místní podsítě byl přidán do místní síťové brány po již vytvořit bránu. 


<!--  #### SQL and MySQL  -->


#### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Uživatelé musí registrovat poskytovatele prostředků úložiště, dřív, než vytvoří jejich první funkce Azure v rámci předplatného.

- <!-- TBD - IS ASDK --> Aby bylo možné horizontální navýšení kapacity infrastruktury (pracovních procesů, správy, role front-endu), musíte použít PowerShell, jak je popsáno v poznámkách k verzi pro výpočetní prostředky.  


#### <a name="usage"></a>Využití  
- <!-- TBD -  IS ASDK --> Data o využití veřejné IP adresy využití měřičů zobrazuje stejné *EventDateTime* hodnotu pro každý záznam místo *TimeDate* razítko, které ukazuje, kdy se záznam vytvořil. V současné době nelze pomocí těchto dat pro monitorování přesné použití veřejné IP adresy.

<!-- #### Identity -->




## <a name="build-11807076"></a>Sestavení 1.1807.0.76

### <a name="new-features"></a>Nové funkce
Toto sestavení obsahuje následující vylepšení a oprav pro Azure Stack.  

- <!-- 1658937 | ASDK, IS --> **Spustit zálohování podle předem daného plánu** – jako zařízení, Azure Stack můžete teď automaticky aktivovat zálohování infrastruktury pravidelně a Eliminujte zásahu člověka. Azure Stack se také automaticky vyčištění z externí sdílené složky pro zálohy, které jsou starší než doba uchování definované. Další informace najdete v tématu [povolit zálohování pro Azure Stack s prostředím PowerShell](.\.\azure-stack-backup-enable-backup-powershell.md).

- <!-- 2496385 | ASDK, IS -->  **Čas do celkový čas zálohování za přenosy dat přidaný.** Další informace najdete v tématu [povolit zálohování pro Azure Stack s prostředím PowerShell](.\.\azure-stack-backup-enable-backup-powershell.md).

-   <!-- 1702130 | ASDK, IS -->  **Záložní kapacita externí nyní zobrazuje správné kapacity z externí sdílené složky.** (Dříve to bylo pevně zakódovat až 10 GB.) Další informace najdete v tématu [povolit zálohování pro Azure Stack s prostředím PowerShell](.\.\azure-stack-backup-enable-backup-powershell.md).
 
- <!-- 2753130 |  IS, ASDK   -->  **Šablony Azure Resource Manageru teď podporují elementu podmínku** – teď můžete nasadit prostředků v šabloně Azure Resource Manageru pomocí podmínku. Můžete navrhnout šablonu k nasazení prostředků na základě podmínky, jako jsou hodnocení, pokud hodnota parametru je k dispozici. Informace o použití šablony jako podmínku, naleznete v tématu [podmíněné nasazení prostředku](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/conditional-deploy) a [části proměnných šablon Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-templates-variables) v dokumentaci k Azure. 

   Můžete také použít šablony, které [nasadit prostředky do více než jedno předplatné nebo skupinu prostředků](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-cross-resource-group-deployment).  

- <!--2753073 | IS, ASDK -->  **Podpora verzí prostředků Microsoft.Network rozhraní API se aktualizovala** zahrnující podporu pro rozhraní API verze 2017-10-01 z 2015-06-15 pro Azure Stack síťovým prostředkům.  Podpora verzí prostředků mezi 2017-10-01 a 2015-06-15 není součástí této verze, ale budou zahrnuty v budoucí verzi.  Najdete [důležité informace týkající se služby Azure Stack sítě](.\.\user\azure-stack-network-differences.md) pro rozdíly funkcí.

- <!-- 2272116 | IS, ASDK   -->  **Azure Stack má přidanou podporu pro zpětné vyhledávání DNS pro externí koncové koncové body infrastruktury Azure stacku** (to je pro portál, adminportal, správu a adminmanagement). To umožňuje službě Azure Stack je externí koncový bod názvů přeložit IP adresu.

- <!-- 2780899 |  IS, ASDK   --> **Azure Stack teď podporuje přidání další síťové rozhraní existujícímu virtuálnímu počítači.**  Tato funkce je dostupná prostřednictvím portálu, Powershellu a rozhraní příkazového řádku. Další informace najdete v tématu [přidání nebo odebrání síťových rozhraní](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-vm) v dokumentaci k Azure. 

- <!-- 2222444 | IS, ASDK   -->  **Zlepšení přesnosti a odolnost proti chybám se provedly sítě měřiče využití**. Měření využití sítě jsou nyní přesnější a rozdělení předplatná účet pozastavit, výpadek období a konflikty časování.

- <!-- 2297790 | IS, ASDK -->  **Vylepšení klienta služby Azure Stack syslog (funkce preview)**. Tento klient umožňuje předávání auditu a protokolů spojených s infrastrukturou Azure Stack na syslog serveru nebo zabezpečení informací a událostí (SIEM) software pro správu externí do služby Azure Stack. Klient syslog nyní podporuje protokol TCP pomocí prostého textu nebo šifrování TLS 1.2, druhá možnost je výchozí konfigurace. Připojení TLS lze nakonfigurovat buď jen pro server, nebo vzájemné ověřování.

  Nakonfigurujte syslog server jak komunikuje klienta syslog (jako je protokol, šifrování a ověřování), použijte rutinu Set-SyslogServer. Tato rutina je k dispozici z privileged koncového bodu (období). 

  Přidání certifikátu na straně klienta pro vzájemné ověřování klienta TLS 1.2 syslog, použijte rutinu Set-SyslogClient v období.

  V této verzi preview zobrazí se mnohem většího počtu audity a upozornění. 

  Protože tato funkce je stále ve verzi preview, není v produkčním prostředí závisí na něm.

  Další informace najdete v tématu [předávání syslog Azure Stack](.\.\azure-stack-integrate-security.md).

- <!-- ####### | IS, ASDK -->  **Azure Resource Manageru obsahuje název oblasti.** V této verzi načtené z Azure Resource Manageru objekty teď bude zahrnovat atribut názvu oblasti. Pokud stávající skript prostředí PowerShell přímo předá objekt jiná rutina, skript může vést k chybě a selhání. To je chování kompatibilní s Azure Resource Manageru a vyžaduje odečítané atribut oblasti volajícího klienta. Další informace o Azure Resource Manageru najdete v tématu [dokumentaci k Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/).

- <!-- TBD | IS, ASDK -->  **Přesun předplatných mezi delegované poskytovatele.** Mezi nové nebo existující delegované poskytovatele odběry, které patří do stejného tenanta adresáře nyní se můžete přesunout předplatné. Předplatná, která patří do předplatného výchozí zprostředkovatel můžete také přesunout do delegovaný předplatná poskytovatele ve stejném tenantu Active Directory. Další informace najdete v části [delegování nabídek v Azure stacku](.\.\azure-stack-delegated-provider.md).
 
- <!-- 2536808 IS ASDK --> **Vylepšené čas vytvoření virtuálního počítače** pro virtuální počítače, které jsou vytvořené pomocí Image stáhnout z webu Azure marketplace.

### <a name="fixed-issues"></a>Opravené problémy

- <!-- TBD | ASDK, IS --> Různá vylepšení provedly se proces aktualizace provádět spolehlivější. Kromě toho byly provedeny opravy základní infrastruktury, což zvyšuje vyprázdnění uzlu, a tím minimalizovat potenciální výpadek pro úlohy během aktualizace.

-   <!--2292271 | ASDK, IS --> Opravili jsme problém kde upravené kvótu neuplatnilo ke stávajícím předplatným.  Teď když plán přidružené předplatné uživatele, zvýšit kvótu pro síťový prostředek, který je součástí nabídky nové omezení platí pro již existujících předplatných, jakož i nová předplatná.

- <!-- 2448955 | IS ASDK --> Teď můžete úspěšně dotazovat protokolů aktivit pro systémy, které jsou nasazené v časovém pásmu UTC + N.    

- <!-- 2319627 |  ASDK, IS --> Předběžné kontroly zálohování konfiguračních parametrů (cesta/uživatelské jméno/heslo/šifrovací klíč) už nastaví nesprávné nastavení pro konfiguraci zálohování. (Dříve, nesprávné nastavení byly nastaveny do zálohy a zálohy by dojde k selhání při aktivaci.)

- <!-- 2215948 |  ASDK, IS --> Zálohování seznamu nyní aktualizuje, když je ručně odstranit zálohy z externí sdílené složky.

- <!-- 2360715 |  ASDK, IS -->  Při nastavování integrace datových center už přístup k souboru metadat služby AD FS ze sdílené složky. Další informace najdete v tématu [nastavení integrace služby AD FS tím, že poskytuje soubor metadat federace](.\.\azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

- <!-- 2388980 | ASDK, IS --> Opravili jsme problém, že mu uživatelům přiřadit stávající veřejnou IP adresu, která byla dříve přidělili síťové rozhraní nebo nástroj pro vyrovnávání zatížení na nové síťové rozhraní nebo nástroj pro vyrovnávání zatížení.  

- <!-- 2551834 - IS, ASDK --> Když vyberete *přehled* pro účet úložiště na portálech na správce nebo uživatele *Essentials* podokně správně teď zobrazuje všechny požadované informace. 

- <!-- 2551834 - IS, ASDK --> Když vyberete *značky* pro účet úložiště na portálech na správce nebo uživatele, zobrazí informace o správně.

- <!-- TBD - IS ASDK --> Tato verze služby Azure Stack řeší problém, která bránila použití aktualizací ovladače z balíčků rozšíření výrobce OEM.

-   <!-- 2055809- IS ASDK --> Opravili jsme problém, který zabránil můžete odstraňovat virtuální počítače v okně výpočty, když virtuální počítač se nepodařilo vytvořit.  

- <!--  2643962 IS ASDK -->  Upozornění pro *nízká kapacita paměti* již nezobrazuje správně.

- <!--  TBD ASDK --> Virtuální počítač, který je hostitelem koncového bodu oprávnění (období) bylo zvýšeno na 4GB. V ASDK je tento virtuální počítač s názvem AzS-ERCS01.

- **Různé opravy** výkonu, stability, zabezpečení a operační systém, který se používá ve službě Azure Stack


<!-- ### Changes  -->
<!--   ### Additional releases timed with this update  -->


### <a name="known-issues"></a>Známé problémy

#### <a name="portal"></a>Portál  
- <!-- 2931230 – IS  ASDK --> Plány, které jsou přidány na předplatné uživatele jako doplňkový plán nelze odstranit, i když odebrat plán ze předplatné uživatele. Plán zůstane, dokud se také odstraní předplatné, které odkazují na doplňkový plán. 

- <!--2760466 – IS  ASDK --> Při instalaci nového prostředí Azure Stack, na kterém běží tato verze, upozornění, která informuje o *vyžadována aktivace* se nemusí zobrazit. [Aktivace](.\.\azure-stack-registration.md) se vyžaduje, abyste mohli používat marketplace syndikace. 

- <!-- TBD - IS ASDK --> Dva typy pro správu předplatného, které byly [představený poprvé ve verzi 1804](.\.\azure-stack-update-1804.md#new-features) by se neměly. Typy předplatného jsou **měření předplatné**, a **využití předplatného**. Tyto typy předplatného jsou **měření předplatné**, a **využití předplatného**. Tyto typy předplatného jsou viditelné v novým prostředím Azure Stack od verze 1804, ale ještě nejsou připravené k použití. By měla dál používat **předplatné poskytovatele. výchozí** typu.

- <!-- 2403291 - IS ASDK --> Nemusí mít užívání vodorovný posuvník v dolní části portály správce a uživatele. Pokud nemůžete použít vodorovný posuvník, left pomocí popisu cesty přejít na předchozí okno na portálu tak, že vyberete název okna je chcete zobrazit v seznamu s popisem cesty v horní části portálu.
  ![Navigace s popisem cesty](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Odstraňuje se předplatné uživatele za následek osamocené prostředky. Jako alternativní řešení nejprve odstranit prostředky uživatele nebo celou skupinu prostředků a pak odstraňte předplatná uživatelů.

- <!-- TBD -  IS ASDK --> Nelze zobrazit oprávnění k předplatnému pomocí na portálech Azure Stack. Jako alternativní řešení pomocí prostředí PowerShell mohl ověřit oprávnění.

- <!--  TBD | ASDK -->  Výchozí časové pásmo pro vaše nasazení Azure Stack získáte nastaví na čas UTC. Časové pásmo můžete vybrat při instalaci Azure Stack, ale to se automaticky vrátí k UTC jako výchozí během instalace.

#### <a name="health-and-monitoring"></a>Monitorování stavu a
- <!-- 1264761 - IS ASDK -->  Může se zobrazit upozornění *stavu řadiče* komponenta, která mají následující podrobnosti:  

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

  Obě výstrahy můžete bezpečně ignorovat a bude automaticky ukončena v čase.  

- <!-- 2368581 - IS. ASDK --> Operátory Azure stacku, pokud se zobrazí upozornění na nedostatek paměti a virtuální počítače klientů selhání nasazení *Chyba při vytváření virtuálních počítačů Fabric*, je možné, že toto razítko Azure Stack je nedostatek dostupné paměti. Použití [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) nejlépe pochopit dostupné kapacity pro vaše úlohy.

- <!-- TBD - IS. ASDK --> Při spuštění rutiny Test-AzureStack v koncovém bodě oprávnění (období), testu vygeneruje zprávu UPOZORNIT/selhání pro virtuální počítač ERCS. Můžete nadále používat ASDK.

#### <a name="compute"></a>Compute
- <!-- 2494144 - IS, ASDK --> Když vyberete velikost virtuálního počítače pro nasazení virtuálního počítače, některé velikosti virtuálních počítačů řady F-Series se nezobrazí jako součást modulu pro výběr velikost při vytváření virtuálního počítače. Tyto velikosti virtuálních počítačů se nezobrazí v selektoru: *F8s_v2*, *F16s_v2*, *F32s_v2*, a *F64s_v2*.  
  Jako alternativní řešení použijte jednu z následujících metod nasazení virtuálního počítače. V každé metodě je třeba zadat velikost virtuálního počítače, které chcete použít.

  - **Šablona Azure Resource Manageru:** při použití šablony, nastavte *vmSize* v šabloně na velikost virtuálního počítače, který chcete použít. Například následující položka se používá k nasazení virtuálního počítače, který používá *F32s_v2* velikost:  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **Azure CLI:** můžete použít [az vm vytvořit](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) příkaz a zadejte velikost virtuálního počítače jako parametr, podobně jako `--size "Standard_F32s_v2"`.

  - **Prostředí PowerShell:** pomocí prostředí PowerShell můžete použít [New-AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) s parametrem, který určuje velikost virtuálního počítače, podobně jako `-VMSize "Standard_F32s_v2"`.


- <!-- TBD -  IS ASDK --> Nastavení škálování pro škálovací sady virtuálních počítačů nejsou k dispozici na portálu. Jako alternativní řešení můžete použít [prostředí Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Z důvodu rozdíly mezi verzemi prostředí PowerShell, je nutné použít `-Name` parametr namísto `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Při vytváření virtuálních počítačů na portálu user portal Azure Stack na portálu zobrazuje nesprávný počet datových disků, které můžete připojit virtuální počítač řady D series. Všechny podporované řady D series virtuálních počítačů může obsahovat libovolný počet datových disků jako konfiguraci Azure.

- <!-- TBD -  IS ASDK --> Selhání vytvoření image virtuálního počítače neúspěšné položky, který se nedá odstranit, mohou být přidány do okna výpočetní Image virtuálního počítače.

  Jako alternativní řešení vytvořte nové image virtuálního počítače s fiktivní virtuálního pevného disku, který je možné vytvářet přes Hyper-V (nový virtuální pevný disk-C:\dummy.vhd cesta-oprava SizeBytes – 1 GB). Tento proces by měla vyřešit problém, který zabraňuje odstranění položky. Pak 15 minut po vytvoření fiktivní image můžete úspěšně ho odstranit.

  Potom můžete opakovat stahování image virtuálního počítače, která původně selhala.

- <!-- TBD -  IS ASDK --> Pokud zřizování rozšíření na nasazení virtuálního počítače trvá příliš dlouho, uživatelé měli nechat zřizování vypršení časového limitu namísto pokusu o zastavení procesu uvolnění nebo odstranění virtuálního počítače.  

- <!-- 1662991 - IS ASDK --> Diagnostika virtuálních počítačů Linux není podporována ve službě Azure Stack. Při nasazení virtuálního počítače s Linuxem s povolenou diagnostikou virtuálního počítače, nasazení se nezdaří. Nasazení se také nezdaří, pokud povolíte základní metriky virtuálního počítače s Linuxem prostřednictvím nastavení diagnostiky.

- <!-- 2724961- IS ASDK --> Když se zaregistrujete **Microsoft.Insight** poskytovatele prostředků v nastavení předplatného a vytvoření virtuálního počítače s Windows pomocí hostovaného operačního systému diagnostických povolena, nezobrazí stránka s přehledem virtuálních počítačů typu data metrik. 

   Data metrik, jako je graf procento využití procesoru pro virtuální počítač, najdete **metriky** blade a zobrazit všechny podporované Windows virtuálního počítače hosta metriky.

#### <a name="networking"></a>Sítě
- <!-- 1766332 - IS, ASDK --> V části **sítě**, vyberete-li **vytvořit bránu VPN** nastavit připojení k síti VPN, **založenou na zásadách** je uveden jako typ sítě VPN. Nevybírejte tuto možnost. Pouze **trasy na základě** možnost je podporovaná ve službě Azure Stack.

- <!-- 1902460 -  IS ASDK --> Azure Stack podporuje jediný *bránu místní sítě* na IP adresu. To platí ve všech předplatných tenanta. Po vytvoření první místní síťové brány připojení, následné pokusech o vytvoření prostředku brány místní sítě pomocí stejné IP adresy jsou zablokované.

- <!-- 16309153 -  IS ASDK --> Ve virtuální síti, který byl vytvořen s nastavením serveru DNS *automatické*, změna vlastní selhání serveru DNS. Aktualizované nastavení nenasdílí do virtuálních počítačů v dané virtuální síti.

- <!-- 2702741 -  IS ASDK --> Chcete-li být zachována po zastavte a Navraťte vydání není zaručena veřejné IP adresy, které jsou nasazeny pomocí metody dynamického přidělení.

- <!-- 2529607 - IS ASDK --> Během Azure Stack *tajný klíč otočení*, je období, ve kterém veřejné IP adresy nedostupné přibližně 2 až 5 minut.

-   <!-- 2664148 - IS ASDK --> Ve scénářích, kde tenanta je přístup ke své virtuální počítače s použitím tunelu S2S VPN kterými se mohou setkat scénář, kde pokusy o připojení selhat, pokud místní podsítě byl přidán do místní síťové brány po již vytvořit bránu. 


#### <a name="sql-and-mysql"></a>SQL a MySQL
- <!-- TBD - ASDK --> Databáze hostování servery musí být vyhrazená pro použití podle prostředku zprostředkovatele a uživatelského zatížení. Nelze použít instanci, která se používá v jakékoli další příjemce, včetně služeb App Services.

- <!-- IS, ASDK --> Speciální znaky, mezery a tečky, včetně nepodporuje **řady** název při vytváření SKU pro poskytovatele prostředků SQL nebo MySQL.

#### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Uživatelé musí registrovat poskytovatele prostředků úložiště, dřív, než vytvoří jejich první funkce Azure v rámci předplatného.

- <!-- TBD - IS ASDK --> Aby bylo možné horizontální navýšení kapacity infrastruktury (pracovních procesů, správy, role front-endu), musíte použít PowerShell, jak je popsáno v poznámkách k verzi pro výpočetní prostředky.  

- <!-- TBD - IS ASDK --> App Service můžete nasadit jenom do *předplatné poskytovatele. výchozí* v tuto chvíli. V budoucí aktualizaci, bude nasazení služby App Service do nové *měření předplatné* , která byla zavedena ve verzi 1804 Azure Stack. Při použití podporuje měření, všechna existující nasazení budou migrovány na tento nový typ předplatného.

#### <a name="usage"></a>Využití  
- <!-- TBD -  IS ASDK --> Data o využití veřejné IP adresy využití měřičů zobrazuje stejné *EventDateTime* hodnotu pro každý záznam místo *TimeDate* razítko, které ukazuje, kdy se záznam vytvořil. V současné době nelze pomocí těchto dat pro monitorování přesné použití veřejné IP adresy.

<!-- #### Identity -->






## <a name="build-11805147"></a>Sestavení 1.1805.1.47

> [!TIP]  
> Na základě názorů zákazníků, dojde k aktualizaci verze schématu používá pro Microsoft Azure Stack. Od této aktualizace 1805, nové schéma lépe představuje aktuální cloudovou verzi.  
>
> Verze schématu je nyní *Version.YearYearMonthMonth.MinorVersion.BuildNumber* kde označují sad druhý a třetí verzi a verzi. Například 1805.1 představuje *verze pro výrobu* 1805 verzi (RTM).  


### <a name="new-features"></a>Nové funkce
Toto sestavení obsahuje následující vylepšení a oprav pro Azure Stack.  

- <!-- 2297790 - IS, ASDK --> **Azure Stack je teď zahrnuje *Syslog* klienta** jako *funkce ve verzi preview*. Tento klient umožňuje předávání protokolů auditu a zabezpečení spojených s infrastrukturou Azure Stack na Syslog serveru nebo zabezpečení událostí (SIEM) software pro správu, která je externí do služby Azure Stack. Klient protokolu Syslog v současné době podporuje pouze neověřenými připojeními UDP přes výchozí port 514. Datová část každé zprávy Syslog formátována v Common Event Format (CEF).

  Chcete-li nakonfigurovat klienta Syslog, použijte **Set-SyslogServer** rutinu na privilegované koncový bod.

  V této verzi preview může se zobrazit následující tři výstrahy. Při zápisu ve službě Azure Stack, tyto výstrahy obsahují *popisy* a *nápravy* pokyny.
  - Název: Integrita kódu. vypnuto  
  - TITLE: Integrita kódu v režimu auditování
  - TITLE: Vytvořeného účtu uživatele

  Tato funkce je ve verzi preview a je byste se neměli spoléhat v produkčním prostředí.   


### <a name="fixed-issues"></a>Opravené problémy
- Opravili jsme problém, který blokovaný [otevřete novou žádost o podporu z rozevíracího seznamu](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) z portálu pro správu. Tato možnost teď funguje očekávaným způsobem.

- <!--  TBD ASDK --> Virtuální počítač, který je hostitelem koncového bodu oprávnění (období) bylo zvýšeno na 4GB. V ASDK je tento virtuální počítač s názvem AzS-ERCS01.

- **Různé opravy** výkonu, stability, zabezpečení a operační systém, který se používá ve službě Azure Stack


<!-- ### Changes  -->


<!--   ### Additional releases timed with this update  -->


### <a name="known-issues"></a>Známé problémy

#### <a name="portal"></a>Portál
- <!-- 2931230 – IS  ASDK --> Plány, které jsou přidány na předplatné uživatele jako doplňkový plán nelze odstranit, i když odebrat plán ze předplatné uživatele. Plán zůstane, dokud se také odstraní předplatné, které odkazují na doplňkový plán. 

- <!-- 2551834 - IS, ASDK --> Když vyberete **přehled** portálech na správce nebo uživatele, informace z účtu úložiště *Essentials* podokně nezobrazí.  V podokně Essentials se zobrazí informace o účtu jako jeho *skupiny prostředků*, *umístění*, a *ID předplatného*.  Další možnosti pro přehled jsou přístupné, jako je *služby* a *monitorování*, stejně jako možnosti *otevřít v Průzkumníkovi* nebo *odstranit účet úložiště* .  

  Chcete-li zobrazit není k dispozici informace, použijte [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) rutiny Powershellu.

- <!-- 2551834 - IS, ASDK --> Když vyberete **značky** pro účet úložiště na portálech na správce nebo uživatele, informace se nepodaří načíst a nejsou zobrazeny.  

  Chcete-li zobrazit není k dispozici informace, použijte [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) rutiny Powershellu.

- <!-- TBD - IS ASDK --> Nepoužívejte nové typy pro správu předplatného *měření předplatné*, a *využití předplatného*. Tyto nové typy předplatného byly představený poprvé ve verzi 1804, ale ještě nejsou připravené k použití. By měla dál používat *výchozí zprostředkovatel* typu předplatného.  
- <!-- TBD - IS ASDK --> Aktualizace ovladačů nelze použít s použitím balíčku výrobce OEM rozšíření s touto verzí služby Azure Stack.  Neexistuje žádné alternativní řešení tohoto problému.
 
- <!-- TBD - IS ASDK --> Možnost [otevřete novou žádost o podporu z rozevíracího seznamu](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) z v rámci správce portálu není k dispozici. Místo toho použijte následující odkaz:     
    - Pro Azure Stack Development Kit, použijte https://aka.ms/azurestackforum.    

- <!-- 2403291 - IS ASDK --> Nemusí mít užívání vodorovný posuvník v dolní části portály správce a uživatele. Pokud nemůžete použít vodorovný posuvník, left pomocí popisu cesty přejít na předchozí okno na portálu tak, že vyberete název okna je chcete zobrazit v seznamu s popisem cesty v horní části portálu.
  ![Navigace s popisem cesty](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> Odstraňuje se předplatné uživatele za následek osamocené prostředky. Jako alternativní řešení nejprve odstranit prostředky uživatele nebo celou skupinu prostředků a pak odstraňte předplatná uživatelů.

- <!-- TBD -  IS ASDK --> Nelze zobrazit oprávnění k předplatnému pomocí na portálech Azure Stack. Jako alternativní řešení pomocí prostředí PowerShell mohl ověřit oprávnění.


#### <a name="health-and-monitoring"></a>Monitorování stavu a
- <!-- 1264761 - IS ASDK -->  Může se zobrazit upozornění *stavu řadiče* komponenta, která mají následující podrobnosti:  

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

    Obě výstrahy #1 a #2 můžete bezpečně ignorovat a bude automaticky zavřít v čase. 

  Můžete si také prohlédnout následující výstrahu k *kapacity*. Pro toto upozornění se může lišit procento paměti k dispozici identifikované v popisu:  

  Upozornění #3:
   - Název: Nedostatek paměti kapacity
   - ZÁVAŽNOST: kritické
   - KOMPONENTA: kapacity
   - Popis: Region spotřeboval více než 80,00 % dostupné paměti. Vytváření virtuálních počítačů s velkými objemy paměti může selhat.  

  V této verzi služby Azure Stack můžete toto upozornění aktivovat nesprávně. Pokud nasazení úspěšně tenantské virtuální počítače, můžete ignorovat toto upozornění. 
  
  Automaticky zavřít upozornění #3. Pokud zavřít tuto výstrahu vytvoří Azure Stack stejná výstraha do 15 minut.  

- <!-- 2368581 - IS. ASDK --> Operátory Azure stacku, pokud se zobrazí upozornění na nedostatek paměti a virtuální počítače klientů selhání nasazení *Chyba při vytváření virtuálních počítačů Fabric*, je možné, že toto razítko Azure Stack je nedostatek dostupné paměti. Použití [Azure Stack Capacity Planner](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) nejlépe pochopit dostupné kapacity pro vaše úlohy.

- <!-- TBD - IS. ASDK --> Při spuštění rutiny Test-AzureStack v koncovém bodě oprávnění (období), vygeneruje zprávu UPOZORNIT ERCS virtuálního počítače. Můžete nadále používat ASDK. 


#### <a name="compute"></a>Compute
- <!-- TBD - IS, ASDK --> Když vyberete velikost virtuálního počítače pro nasazení virtuálního počítače, některé velikosti virtuálních počítačů řady F-Series se nezobrazí jako součást modulu pro výběr velikost při vytváření virtuálního počítače. Tyto velikosti virtuálních počítačů se nezobrazí v selektoru: *F8s_v2*, *F16s_v2*, *F32s_v2*, a *F64s_v2*.  
  Jako alternativní řešení použijte jednu z následujících metod nasazení virtuálního počítače. V každé metodě je třeba zadat velikost virtuálního počítače, které chcete použít.

  - **Šablona Azure Resource Manageru:** při použití šablony, nastavte *vmSize* v šabloně na velikost virtuálního počítače, který chcete použít. Například následující položka se používá k nasazení virtuálního počítače, který používá *F32s_v2* velikost:  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **Azure CLI:** můžete použít [az vm vytvořit](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) příkaz a zadejte velikost virtuálního počítače jako parametr, podobně jako `--size "Standard_F32s_v2"`.

  - **Prostředí PowerShell:** pomocí prostředí PowerShell můžete použít [New-AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) s parametrem, který určuje velikost virtuálního počítače, podobně jako `-VMSize "Standard_F32s_v2"`.


- <!-- TBD -  IS ASDK --> Nastavení škálování pro škálovací sady virtuálních počítačů nejsou k dispozici na portálu. Jako alternativní řešení můžete použít [prostředí Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Z důvodu rozdíly mezi verzemi prostředí PowerShell, je nutné použít `-Name` parametr namísto `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Při vytváření virtuálních počítačů na portálu user portal Azure Stack na portálu zobrazuje nesprávný počet datových disků, které můžete připojit virtuální počítač řady D series. Všechny podporované řady D series virtuálních počítačů může obsahovat libovolný počet datových disků jako konfiguraci Azure.

- <!-- TBD -  IS ASDK --> Selhání vytvoření image virtuálního počítače neúspěšné položky, který se nedá odstranit, mohou být přidány do okna výpočetní Image virtuálního počítače.

  Jako alternativní řešení vytvořte nové image virtuálního počítače s fiktivní virtuálního pevného disku, který je možné vytvářet přes Hyper-V (nový virtuální pevný disk-C:\dummy.vhd cesta-oprava SizeBytes – 1 GB). Tento proces by měla vyřešit problém, který zabraňuje odstranění položky. Pak 15 minut po vytvoření fiktivní image můžete úspěšně ho odstranit.

  Potom můžete opakovat stahování image virtuálního počítače, která původně selhala.

- <!-- TBD -  IS ASDK --> Pokud zřizování rozšíření na nasazení virtuálního počítače trvá příliš dlouho, uživatelé měli nechat zřizování vypršení časového limitu namísto pokusu o zastavení procesu uvolnění nebo odstranění virtuálního počítače.  

- <!-- 1662991 - IS ASDK --> Diagnostika virtuálních počítačů Linux není podporována ve službě Azure Stack. Při nasazení virtuálního počítače s Linuxem s povolenou diagnostikou virtuálního počítače, nasazení se nezdaří. Nasazení se také nezdaří, pokud povolíte základní metriky virtuálního počítače s Linuxem prostřednictvím nastavení diagnostiky.

#### <a name="networking"></a>Sítě
- <!-- TBD - IS ASDK --> Trasy definované uživatelem nelze vytvořit buď v portálu pro správu nebo uživatele. Jako alternativní řešení použít [prostředí Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell).

- <!-- 1766332 - IS, ASDK --> V části **sítě**, vyberete-li **vytvořit bránu VPN** nastavit připojení k síti VPN, **založenou na zásadách** je uveden jako typ sítě VPN. Nevybírejte tuto možnost. Pouze **trasy na základě** možnost je podporovaná ve službě Azure Stack.

- <!-- 2388980 -  IS ASDK --> Po vytvoření virtuálního počítače a přidružené k veřejné IP adresy, nelze zrušit přidružení tohoto virtuálního počítače z této IP adresy. Zrušení přidružení zdánlivě fungovat, ale zůstane spojená s původní virtuální počítač dříve přiřazenou veřejnou IP adresu.

  V současné době používaly pouze nové veřejné IP adresy pro nové virtuální počítače, které vytvoříte.

  K tomuto chování dochází i v případě, že znovu přiřadit IP adresu do nového virtuálního počítače (obvykle označované jako *prohození virtuálních IP adres*). Všechny budoucí pokusy o připojení přes tuto IP adresu výsledkem připojení původního virtuálního počítače a ne do nové.

- <!-- 2292271 - IS ASDK --> Při zvýšení maximální kvóty pro sítě prostředek, který je součástí nabídky a plán, který je přidružený k předplatnému klienta služby, nové omezení neplatí pro toto předplatné. Nový limit se však nevztahují na nových předplatných, které jsou vytvořeny po zvýšení této kvóty.

  Chcete-li tento problém vyřešit, použijte doplňkový plán na zvýšení této kvóty sítě při v plánu je už přidružený k předplatnému. Další informace najdete v tématu Jak [zpřístupnit doplňkový plán](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> Nelze odstranit odběr, který má prostředky zóny DNS nebo prostředky směrovací tabulku s ním spojená. Chcete-li úspěšně odstranit předplatné, musíte nejprve odstranit zónu DNS a směrovací tabulky prostředků z předplatného tenanta.


- <!-- 1902460 -  IS ASDK --> Azure Stack podporuje jediný *bránu místní sítě* na IP adresu. To platí ve všech předplatných tenanta. Po vytvoření první místní síťové brány připojení, následné pokusech o vytvoření prostředku brány místní sítě pomocí stejné IP adresy jsou zablokované.

- <!-- 16309153 -  IS ASDK --> Ve virtuální síti, který byl vytvořen s nastavením serveru DNS *automatické*, změna vlastní selhání serveru DNS. Aktualizované nastavení nenasdílí do virtuálních počítačů v dané virtuální síti.

- <!-- TBD -  IS ASDK --> Azure Stack nepodporuje přidávání další síťová rozhraní k instanci virtuálního počítače po nasazení virtuálního počítače. Pokud virtuální počítač vyžaduje více než jedno síťové rozhraní, musí být definován v době nasazení.


#### <a name="sql-and-mysql"></a>SQL a MySQL
- <!-- TBD - ASDK --> Databáze hostování servery musí být vyhrazená pro použití podle prostředku zprostředkovatele a uživatelského zatížení. Nelze použít instanci, která se používá v jakékoli další příjemce, včetně služeb App Services.

- <!-- IS, ASDK --> Speciální znaky, mezery a tečky, včetně nepodporuje **řady** název při vytváření SKU pro poskytovatele prostředků SQL nebo MySQL.

#### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Uživatelé musí registrovat poskytovatele prostředků úložiště, dřív, než vytvoří jejich první funkce Azure v rámci předplatného.

- <!-- TBD - IS ASDK --> Aby bylo možné horizontální navýšení kapacity infrastruktury (pracovních procesů, správy, role front-endu), musíte použít PowerShell, jak je popsáno v poznámkách k verzi pro výpočetní prostředky.  

- <!-- TBD - IS ASDK --> App Service můžete nasadit jenom do *předplatné poskytovatele. výchozí* v tuto chvíli. <!-- In a future update, App Service will deploy into the new *Metering subscription* that was introduced in Azure Stack 1804. When Metering is supported for use, all existing deployments will be migrated to this new subscription type. -->

#### <a name="usage"></a>Využití  
- <!-- TBD -  IS ASDK --> Data o využití veřejné IP adresy využití měřičů zobrazuje stejné *EventDateTime* hodnotu pro každý záznam místo *TimeDate* razítko, které ukazuje, kdy se záznam vytvořil. V současné době nelze pomocí těchto dat pro monitorování přesné použití veřejné IP adresy.

<!-- #### Identity -->

