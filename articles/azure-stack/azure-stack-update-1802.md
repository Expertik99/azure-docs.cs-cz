---
title: Aktualizace zásobníku Azure 1802 | Microsoft Docs
description: Další informace o tom, co je v aktualizaci 1802 pro Azure zásobníku integrované systémy, známé problémy a kde se stáhnout aktualizaci.
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
ms.date: 05/30/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: af65ffc088c2beadf415b72ec284ef77f3e4f6d4
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757253"
---
# <a name="azure-stack-1802-update"></a>Azure aktualizace 1802 zásobníku

*Platí pro: Azure zásobníku integrované systémy*

Tento článek popisuje vylepšení a opravy v balíčku aktualizace 1802, známé problémy pro tuto verzi a toho, kde se stáhnout aktualizaci. Známé problémy se dál dělí na problémy, které jsou přímo souvisí s proces aktualizace a problémy s sestavení (po instalaci).

> [!IMPORTANT]        
> Tento balíček aktualizace je pouze pro Azure zásobníku integrované systémy. Tento balíček aktualizace nevztahují na Azure zásobníku Development Kit.

## <a name="build-reference"></a>Odkaz na sestavení    
Číslo sestavení aktualizace 1802 zásobník Azure je **20180302.1**.  


## <a name="before-you-begin"></a>Než začnete    
> [!IMPORTANT]    
> Nepokoušejte se vytvořit virtuální počítače při instalaci této aktualizace. Další informace o správě aktualizací najdete v tématu [správě aktualizací v přehledu Azure zásobníku](azure-stack-updates.md#plan-for-updates).


### <a name="prerequisites"></a>Požadavky
- Nainstalovat Azure zásobníku [aktualizovat 1712](azure-stack-update-1712.md) před instalací aktualizace 1802 zásobník Azure.    

- Nainstalujte **opravu AzS Hotfix – 1.0.180312.1-sestavení 20180222.2** před instalací aktualizace 1802 zásobník Azure. Tato oprava hotfix aktualizace programu Windows Defender a je k dispozici při stahování aktualizací pro Azure zásobníku.

  Chcete-li nainstalovat opravu hotfix, postupujte podle běžné postupy pro [instalace aktualizací pro Azure zásobníku](azure-stack-apply-updates.md). Název aktualizace zobrazí jako **opravu AzS Hotfix – 1.0.180312.1**a obsahuje následující soubory: 
    - PUPackageHotFix_20180222.2-1.exe
    - PUPackageHotFix_20180222.2-1.bin
    - Metadata.xml

  Po nahrávání těchto souborů do účtu úložiště a kontejneru, spusťte instalaci z dlaždice aktualizace v portálu pro správu. 
  
  Na rozdíl od aktualizace zásobníku Azure instalací této aktualizace se nemění verzi Azure zásobníku.  Pokud chcete potvrdit, instalaci této aktualizace, zobrazí se seznam **nainstalované aktualizace**.
 


### <a name="post-update-steps"></a>Postup po aktualizaci
Po instalaci 1802 nainstalujte všechny použitelné opravy hotfix. Další informace naleznete v následujících článcích znalostní báze knowledge base, a také naše [obsluhy zásad](azure-stack-servicing-policy.md). 
- Oprava hotfix Azure zásobníku **1.0.180302.4**. [KB 4131152 - existující sady škálování virtuálního počítače se může stát nepoužitelným]( https://support.microsoft.com/help/4131152) 

  Tato oprava také řeší problémy popsané v [KB 4103348 – rozhraní API funkce síťový adaptér služby dojde k chybě při pokusu o instalaci aktualizace zásobníku Azure](https://support.microsoft.com/help/4103348).


### <a name="new-features-and-fixes"></a>Nové funkce a opravy
Tato aktualizace zahrnuje následující vylepšení a opravy pro Azure zásobníku.

- **Následující verze rozhraní API služby Azure Storage je přidána podpora**:
    - 2017-04-17 
    - 2016-05-31 
    - 2015-12-11 
    - 2015-07-08 
    
    Další informace najdete v tématu [zásobník úložiště Azure: rozdíly a aspekty](/azure/azure-stack/user/azure-stack-acs-differences).

- **Podporu pro větší [objekty BLOB bloku](azure-stack-acs-differences.md)**:
    - Blok maximální povolená velikost je zvýšena ze 4 MB volného místa na 100 MB.
    - Velikost objektu blob maximální vzroste z 195 GB 4.75 TB.  

- **Infrastruktura zálohování** nyní se zobrazí na dlaždici zprostředkovatelé prostředků a výstrahy pro zálohování jsou povolené. Další informace o službě zálohování infrastruktury najdete v tématu [zálohování a data obnovení pro zásobník Azure pomocí služby Backup infrastruktury](azure-stack-backup-infrastructure-backup.md).

- **Aktualizovat *Test AzureStack* rutiny** ke zlepšení diagnostiku pro úložiště. Další informace o této rutině najdete v tématu [ověření pro Azure zásobníku](azure-stack-diagnostic-test.md).

- **Na základě rolí vylepšení řízení přístupu (RBAC)** – teď můžete RBAC delegovat oprávnění pro univerzální skupiny uživatelů po nasazení Azure zásobníku se službou AD FS. Další informace o RBAC najdete v tématu [spravovat RBAC](azure-stack-manage-permissions.md).

- **Byla přidána podpora několika domén selhání**.  Další informace najdete v tématu [vysoká dostupnost pro Azure zásobníku](azure-stack-key-features.md#high-availability-for-azure-stack).

- **Podpora pro upgrade fyzické paměti** – teď můžete rozšířit kapacitu paměti systému Azure zásobníku integrované po počátečním nasazení. Další informace najdete v tématu [spravovat kapacita fyzické paměti pro Azure zásobníku](azure-stack-manage-storage-physical-memory-capacity.md).

- **Různé opravy** pro výkon, stabilitu, zabezpečení a operační systém, který je používán zásobník Azure.

<!--
#### New features
-->


<!--
#### Fixes
-->


### <a name="known-issues-with-the-update-process"></a>Známé problémy s proces aktualizace    
*Nejsou žádné známé problémy pro instalaci aktualizace 1802.*


### <a name="known-issues-post-installation"></a>Známé problémy (po instalaci)
Toto jsou známé problémy po instalaci pro sestavení **20180302.1**

#### <a name="portal"></a>Portál
- <!-- 2332636 - IS -->  When you use AD FS for your Azure Stack identity system and update to this version of Azure Stack, the default owner of the default provider subscription is reset to the built-in **CloudAdmin** user.  
  Alternativní řešení: Chcete-li tento problém vyřešit, po instalaci této aktualizace, použijte krok 3 ze [automation aktivační události. ke konfiguraci vztahu důvěryhodnosti zprostředkovatele v zásobníku Azure deklarací](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) postup resetovat vlastníka předplatného výchozího zprostředkovatele.   

- Možnost [otevřete novou žádost o podporu z rozevíracího seznamu](azure-stack-manage-portals.md#quick-access-to-help-and-support) z v rámci správce portálu není k dispozici. Místo toho použijte následující odkaz:     
    - Pro Azure zásobníku integrované systémy, používat https://aka.ms/newsupportrequest.

- <!-- 2050709 --> In the admin portal, it is not possible to edit storage metrics for Blob service, Table service, or Queue service. When you go to Storage, and then select the blob, table, or queue service tile, a new blade opens that displays a metrics chart for that service. If you then select Edit from the top of the metrics chart tile, the Edit Chart blade opens but does not display options to edit metrics.

- Nemusí být možné zobrazit úložiště nebo výpočetní prostředky na portálu správce. Příčinou tohoto problému je k chybě při instalaci aktualizace, která způsobí, že aktualizace, která se nesprávně hlásit jako úspěšné. Pokud chcete tento problém opakuje, obraťte se na technickou podporu společnosti Microsoft pro pomoc.

- Může se zobrazit prázdný řídicí panel portálu. Chcete-li obnovit řídicí panel, vyberte ikonu ozubené kolečko v pravém horním rohu portálu a pak vyberte **obnovit výchozí nastavení**.

- Odstranění odběrů uživatele za následek osamocené prostředky. Jako alternativní řešení nejprve odstraňte prostředky uživatele nebo skupinu celý prostředků a potom odstraňte odběry uživatele.

- Nelze zobrazit oprávnění k vašemu předplatnému používání portálů zásobník Azure. Jako alternativní řešení ověřte oprávnění pomocí prostředí PowerShell.

- V řídicím panelu portálu pro správu se nezdaří aktualizace dlaždice zobrazíte informace o aktualizacích. Chcete-li vyřešit tento problém, klikněte na dlaždici jej aktualizovat.

-   V portálu pro správu může se zobrazit Kritická výstraha pro součást Microsoft.Update.Admin. Název výstrahy, popis a nápravu všech zobrazit jako:  
    - *Chyba: šablonu pro FaultType ResourceProviderTimeout nebyl nalezen.*

    Tuto výstrahu můžete ignorovat. 

- <!-- 2253274 --> In the admin and user portals, the Settings blade for vNet Subnets fails to load. As a workaround, use PowerShell and the [Get-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig?view=azurermps-5.5.0) cmdlet to view and  manage this information.

- V portálu pro správu i v portálu user portal, v okně Přehled nepodaří načíst, když vyberete v okně Přehled pro účty úložiště, které byly vytvořeny ve starší verzi rozhraní API (Příklad: 2015-06-15). To zahrnuje účty úložiště systému jako **updateadminaccount** používané během opravy a aktualizace. 

  Jako alternativní řešení, pomocí prostředí PowerShell spustit **Start-ResourceSynchronization.ps1** skript k obnovení přístup podrobnosti o účtu úložiště. [Skript je k dispozici z Githubu]( https://github.com/Azure/AzureStack-Tools/tree/master/Support/scripts)a musí být spuštěna s přihlašovací údaje Správce služby pro privilegované koncový bod. 

- **Stav služby** okno nepodaří načíst. Když otevřete okno Stav služby je správce nebo uživatel portálu Azure zásobníku zobrazí chybu a nenačte informace. Toto je očekávané chování. I když můžete vybrat a otevřete stav služby, tato funkce ještě není k dispozici, ale bude implementované v budoucí verzi systému Azure zásobníku.


#### <a name="health-and-monitoring"></a>Sledování stavu a
- <!-- 1264761 - IS ASDK -->  You might see alerts for the *Health controller* component that have the following details:  

  Výstrahy #1:
   - Název: Infrastruktury role není v pořádku
   - ZÁVAŽNOST: upozornění
   - SOUČÁSTÍ: Řízení stavu
   - Popis: Řadičem stavu prezenčního signálu skener není k dispozici. To může ovlivnit sestavy o stavu a metrik.  

  Výstrahy #2:
   - Název: Infrastruktury role není v pořádku
   - ZÁVAŽNOST: upozornění
   - SOUČÁSTÍ: Řízení stavu
   - Popis: Řadičem stavu skener chyby není k dispozici. To může ovlivnit sestavy o stavu a metrik.

  Obě výstrahy můžete bezpečně ignorovat. Se automaticky zavře v čase.  


#### <a name="marketplace"></a>Marketplace
- Uživatelé mohou procházet na kompletní nabídku marketplace bez předplatného a uvidí položky pro správu jako plány a nabízí. Tyto položky jsou pro uživatele funkční.

#### <a name="compute"></a>Compute
- Nastavení škálování pro sady škálování virtuálního počítače nejsou k dispozici na portálu. Jako alternativní řešení, můžete použít [prostředí Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Z důvodu rozdílů verze prostředí PowerShell, je nutné použít `-Name` parametr místo `-VMScaleSetName`.

- <!-- 2290877  --> You cannot scale up a virtual machine scale set (VMSS) that was created when using Azure Stack prior to version 1802. This is due to the change in support for using availability sets with virtual machine scale sets. This support was added with version 1802.  When you attempt to add additional instances to scale a VMSS that was created prior to this support being added, the action fails with the message *Provisioning state failed*. 

  Tento problém se vyřeší ve verzi 1803. Chcete-li vyřešit tento problém pro verzi 1802, nainstalujte opravu hotfix zásobník Azure **1.0.180302.4**. Další informace najdete v tématu [KB 4131152: existující sady škálování virtuálního počítače se může stát nepoužitelným]( https://support.microsoft.com/help/4131152). 

- Azure zásobníku podporuje používání pouze pevný typ virtuální pevné disky. Některé bitové kopie nabízeným přes marketplace v zásobníku Azure používají dynamické virtuální pevné disky, ale těch, které byly odebrány. Změna velikosti virtuálního počítače (VM) s dynamický disk, který je připojený ponechá virtuální počítač ve stavu selhání.

  Ke zmírnění tohoto problému, odstraňte virtuální počítač bez odstranění disku Virtuálního počítače, objekt blob souboru VHD v účtu úložiště. Potom převést virtuální pevný disk na dynamický disk na pevný disk a potom je znovu vytvořit virtuální počítač.

- Když vytvoříte sadu na portálu přejděte na dostupnosti **nový** > **výpočetní** > **sadu dostupnosti**, můžete vytvořit pouze skupiny dostupnosti s doména selhání a aktualizace domény 1. Jako řešení, při vytváření nového virtuálního počítače, vytvořte skupinu dostupnosti pomocí prostředí PowerShell, rozhraní příkazového řádku, nebo v portálu.

- Když vytvoříte virtuální počítače na portálu pro uživatele Azure zásobníku, portál zobrazí nesprávný počet datových disků, které můžete připojit do série D virtuálních počítačů. Všechny podporované řady D virtuální počítače zvládne tolik datových disků jako konfiguraci Azure.

- Když image virtuálního počítače se nepodaří vytvořit, položku se nezdařilo, nelze odstranit, mohou být přidány do okna výpočetní Image virtuálního počítače.

  Jako alternativní řešení, vytvořte novou bitovou kopii virtuálního počítače s fiktivní virtuální pevný disk vytvořený pomocí technologie Hyper-V (nový virtuální pevný disk-C:\dummy.vhd cesta-Fixed – SizeBytes 1 GB). Tento proces by měla potíže vyřešit, která zabraňuje odstranění položky se nezdařilo. Potom 15 minut po vytvoření fiktivní bitovou kopii, chcete-li úspěšně odstranit.

  Potom můžete zkusit redownload image virtuálního počítače, který předtím selhal.

-  Pokud zřizování rozšíření na nasazení virtuálního počítače trvá příliš dlouho, uživatelé měli nechat zřizování časový limit namísto pokusu o zastavení procesu navrácení nebo odstranění virtuálního počítače.  

- <!-- 1662991 --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings.  




#### <a name="networking"></a>Sítě
- Po vytvoření virtuálního počítače a související s veřejnou IP adresu, nelze zrušit přidružení tohoto virtuálního počítače z této IP adresy. Zrušení pravděpodobně fungovat, ale pořád přidruženy původní virtuální počítač dříve přiřazenou veřejnou IP adresu.

  V současné době používejte pouze nové veřejné IP adresy pro nové virtuální počítače, které vytvoříte.

  K tomuto chování dochází i v případě, že je přiřadit IP adresu na nový virtuální počítač (obvykle označuje jako *prohození*). Všechny budoucí pokusy o připojení přes tuto IP adresu povede připojení do původně přidružený virtuální počítač a ne do nového.

- Interní Vyrovnávání zatížení (ILB) nesprávně zpracovává adresy MAC u virtuálních počítačů v back-end, což způsobí, že ILB pro přerušení při použití instance systému Linux v síti Back-End.  ILB funguje bez problémů s instancí systému Windows na Back-endovou síť.

-   Funkce předávání IP adres se zobrazí na portálu, ale povolení předávání IP adres nemá žádný vliv. Tato funkce ještě není podporována.

- Azure zásobníku podporuje jediný *brány místní sítě* podle IP adresy. To platí ve všech předplatných klienta. Po vytvoření první místní síťové brány připojení, další pokusy o vytvoření prostředku brány místní sítě se stejnou adresou IP jsou blokovány.

- Ve virtuální síti, který byl vytvořen s nastavením serveru DNS *automatické*, změna na vlastní selhání serveru DNS. Aktualizované nastavení nejsou nabídnutých do virtuálních počítačů v této virtuální sítě.

- Azure zásobník nepodporuje přidávání dalších síťových rozhraní do instance virtuálního počítače po nasazení virtuálního počítače. Pokud virtuální počítač vyžaduje více než jedno síťové rozhraní, musí být definován v době nasazení.

-   <!-- 2096388 --> You cannot use the admin portal to update rules for a network security group. 

    Alternativní řešení pro službu App Service: Pokud potřebujete vzdálenou plochu instance řadiče, můžete upravit pravidla zabezpečení v rámci skupiny zabezpečení sítě pomocí prostředí PowerShell.  Následují příklady *povolit*a potom obnovte konfiguraci *Odepřít*:  
    
    - *Povolte:*
 
      ```powershell    
      Login-AzureRMAccount -EnvironmentName AzureStackAdmin
      
      $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"
      
      $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"
      
      ##This doesn’t work. Need to set properties again even in case of edit
      
      #Set-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389" -NetworkSecurityGroup $nsg -Access Allow  
      
      Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
        -Name $RuleConfig_Inbound_Rdp_3389.Name `
        -Description "Inbound_Rdp_3389" `
        -Access Allow `
        -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
        -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
        -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
        -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
        -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
        -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
        -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange
      
      # Commit the changes back to NSG
      Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
      ```

    - *Odepřít:*

        ```powershell
        
        Login-AzureRMAccount -EnvironmentName AzureStackAdmin
        
        $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"
        
        $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"
        
        ##This doesn’t work. Need to set properties again even in case of edit
    
        #Set-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389" -NetworkSecurityGroup $nsg -Access Allow  
    
        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
          -Name $RuleConfig_Inbound_Rdp_3389.Name `
          -Description "Inbound_Rdp_3389" `
          -Access Deny `
          -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
          -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
          -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
          -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
          -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
          -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
          -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange
          
        # Commit the changes back to NSG
        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg 
        ```





#### <a name="sql-and-mysql"></a>SQL a MySQL
 - Než budete pokračovat, zkontrolujte důležité upozornění v [před zahájením](#before-you-begin) téměř začátek tyto poznámky k verzi.
- Může trvat až jednu hodinu, než uživatelé mohou vytvářet databází v nové nasazení SQL nebo MySQL.

- Vytvoření položek na serverech, že hostitel SQL nebo MySQL je podporována pouze poskytovatele prostředků. Položky vytvořené na hostitelském serveru, které nebyly vytvořeny pomocí poskytovatele prostředků může mít za následek neodpovídající stavu.  

- <!-- IS, ASDK --> Special characters, including spaces and periods, are not supported in the **Family** name when you create a SKU for the SQL and MySQL resource providers.

> [!NOTE]  
> Po provedení aktualizace na 1802 zásobník Azure, můžete nadále používat zprostředkovatele prostředků SQL a MySQL, které jste předtím nasadili.  Doporučujeme, abyste že aktualizujete SQL a MySQL, když je dostupná nová verze. Zásobník Azure, jako je aktualizace pro poskytovatele prostředků SQL a MySQL postupně.  Pokud používáte verzi 1710, nejprve použít verzi 1711 pak 1712 a potom aktualizovat 1802.      
>   
> Instalace aktualizace 1802 nemá vliv na aktuální použití funkce SQL nebo MySQL poskytovatelů prostředků, které vaši uživatelé.
> Bez ohledu na verzi poskytovateli prostředků, které používáte vaše data uživatelů v své databáze není dotyku a zůstávají dostupné.    


#### <a name="app-service"></a>App Service
- Uživatelé musí registrovat poskytovatele prostředků úložiště, dřív, než vytvoří jejich první funkce Azure v rámci předplatného.

- Aby bylo možné škálovat infrastrukturu (pracovních procesů, správy, front-end rolí), musíte použít PowerShell, jak je popsáno v poznámkách k verzi pro výpočet.

<!--
#### Identity
-->



#### <a name="downloading-azure-stack-tools-from-github"></a>Stažení nástroje Azure zásobníku z Githubu
- Při použití *vyvolání webrequest* rutiny prostředí PowerShell ke stažení protokolů Azure nástroje z Githubu, obdržíte chybu:     
    -  *vyvolání webrequest: požadavek byl přerušen: Nelze vytvořit zabezpečený kanál SSL/TLS.*     

  K této chybě dochází z důvodu poslední zastaralá podpora Githubu Tlsv1 a Tlsv1.1 kryptografických standardů (výchozí nastavení pro prostředí PowerShell). Další informace najdete v tématu [slabé kryptografické standardy odebrání oznámení](https://githubengineering.com/crypto-removal-notice/).

  Chcete-li vyřešit tento problém, přidejte `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` do horní části skript, který chcete vynutit konzole PowerShell pro stahování z úložiště Githubu TLSv1.2.


## <a name="download-the-update"></a>Stažení aktualizace
Můžete si stáhnout balíček aktualizace 1802 zásobník Azure z [zde](https://aka.ms/azurestackupdatedownload).


## <a name="more-information"></a>Další informace
Společnost Microsoft poskytuje způsob, jak sledovat a pokračovat v použití privilegovaných koncový bod období s 1710 aktualizace nainstalovat aktualizace.

- Najdete v článku [monitorování aktualizací v zásobníku Azure pomocí dokumentace privilegované koncový bod](https://docs.microsoft.com/azure/azure-stack/azure-stack-monitor-update).

## <a name="see-also"></a>Další informace najdete v tématech

- Přehled správy aktualizací v zásobníku Azure najdete v tématu [správě aktualizací v přehledu Azure zásobníku](azure-stack-updates.md).
- Další informace o tom, jak použít aktualizace se zásobníkem Azure najdete v tématu [aktualizace v zásobníku Azure](azure-stack-apply-updates.md).
