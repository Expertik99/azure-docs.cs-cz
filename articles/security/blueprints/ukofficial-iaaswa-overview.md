---
title: Zabezpečení Azure a dodržování předpisů – IaaS třívrstvé webové aplikace pro UK OFFICIAL
description: Zabezpečení Azure a dodržování předpisů – IaaS třívrstvé webové aplikace pro UK OFFICIAL
services: security
author: jomolesk
ms.assetid: 9c32e836-0564-4906-9e15-f070d2707e63
ms.service: security
ms.topic: article
ms.date: 02/08/2018
ms.author: jomolesk
ms.openlocfilehash: d40e23a7cc113a9db297a7dbf00a2372063dfb52
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/14/2018
ms.locfileid: "39060562"
---
# <a name="azure-security-and-compliance-blueprint---three-tier-iaas-web-application-for-uk-official"></a>Zabezpečení Azure a dodržování předpisů – IaaS třívrstvé webové aplikace pro UK OFFICIAL

## <a name="overview"></a>Přehled

 Tento článek obsahuje pokyny a automatizace skripty k poskytování architektura třívrstvou webovou aplikaci Microsoft Azure, která je vhodná pro zpracování mnoho úloh, které jsou klasifikovány jako oficiální ve Spojeném království.

 Použitím infrastruktury jako kódu přístup sadu [Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) šablony nasadit prostředí, které odpovídá na Spojené království národní Kybernetických zabezpečení centrum (NCSC) 14 [principů zabezpečení cloudu](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) Centrum pro zabezpečení Internetu (CIS) a [kontrolních mechanismů pro zabezpečení důležitých](https://www.cisecurity.org/critical-controls.cfm).

 NCSC doporučujeme používat jejich principů zabezpečení cloudu zákazníky k vyhodnocení vlastností zabezpečení služby a pro lepší porozumění tomu dělení odpovědnost mezi zákazníkem a dodavateli. Připravili jsme vám informace o každé z těchto zásad, které vám pomůžou porozumět dělení zodpovědnosti.

 Tato architektura a odpovídající šablon Azure Resource Manageru podporují Microsoft – dokument White Paper, [14 ovládací prvky cloudové zabezpečení pro Spojené království cloudu pomocí Microsoft Azure](https://gallery.technet.microsoft.com/14-Cloud-Security-Controls-670292c1). Tento dokument katalozích Azure services bylo v souladu s Spojené království NCSC 14 principů zabezpečení cloudu, a umožňuje organizacím rychlého možnost pro splnění závazků dodržování předpisů pomocí cloudových služeb v Microsoft Azure globálně a ve Spojeném království cloud.

 Tato šablona nasadí infrastrukturu pro pracovní vytížení. Kód aplikace a podpůrných obchodní vrstvy a datové vrstvy softwaru musí být nainstalované a nakonfigurované. Podrobné pokyny jsou k dispozici [tady](https://aka.ms/ukwebappblueprintrepo).

 Pokud nemáte předplatné Azure, pak můžete se zaregistrovat rychle a snadno – [Začínáme s Azure](https://azure.microsoft.com/get-started/).

## <a name="architecture-diagram-and-components"></a>Diagram architektury a komponenty

 Šablony Azure poskytovat architektura pro třívrstvé webové aplikace v prostředí cloudu Azure, které podporuje UK-OFFICIAL úlohy. Tato architektura poskytuje zabezpečenou hybridní prostředí, které rozšiřuje místní síť do Azure umožňuje webové úlohy bezpečně přistupovat firemní uživatele nebo z Internetu.

![Třívrstvé IaaS webové aplikace pro UK-OFFICIAL diagram referenční architektury](images/ukofficial-iaaswa-architecture.png?raw=true "třívrstvé IaaS webové aplikace pro UK-OFFICIAL diagram referenční architektury")

 Toto řešení používá následující služby Azure. Podrobnosti o architektura nasazení jsou umístěné v [architektura nasazení](#deployment-architecture) oddílu.

(((1) /16 virtuální sítě – provozní síti
- (3) /24 podsítě – úroveň 3 (Web, Biz, Data)
- (1) velikost/27 přidá podsíť –
- podsíť (1) velikost/27 – podsíť brány
- podsíť (1) minimální velikostí/29 – podsítě služby Application Gateway
- Používá výchozí (poskytováno Azure) DNS
- Vytvoření partnerského vztahu povolena Správa virtuální sítě
- Skupiny zabezpečení sítě (NSG) pro správu toku provozu

(((1) /24 virtuální sítě – Správa virtuální sítě
- (((1) velikost/27 podsítě
- Přidá DNS používá (2) a (1) položky Azure DNS
- Vytvoření partnerského vztahu k provozní síti povolena
- Skupiny zabezpečení sítě (NSG) pro správu toku provozu

(1) application Gateway
- -Aktivovanou úrovní WAF
- Režim WAF – ochrany před únikem informací
- Sada pravidel, která: OWASP 3.0
- Naslouchací proces protokolu HTTP na portu 80
- Připojení/přenosů upraveno prostřednictvím skupiny zabezpečení sítě
- Koncový bod veřejné IP adresy definované (Azure)

(1) VPN – založené na směrování, tunelové propojení VPN typu Site-2-Site protokolu IPSec
- Koncový bod veřejné IP adresy definované (Azure)
- Připojení/přenosů upraveno prostřednictvím skupiny zabezpečení sítě
- (1) místní síťová brána (v místním koncového bodu)
- (1) bránu sítě azure (koncový bod Azure)

(9) nasazení virtuálních počítačů – všechny virtuální počítače s nastavením Azure IaaS Antimalwarové DSC

- (2) active Directory Domain Services řadiče domény (Windows Server 2012 R2)
  - (2) role serveru DNS - 1 na virtuální počítač
  - (2) síťové adaptéry připojené k provozní síti - 1 na virtuální počítač
  - Obě jsou připojené k doméně, které jsou definované v šabloně
    - Doména vytvořená jako součást svého nasazení


- (1) Správa virtuálních počítačů Jumpbox (Bastion Host)
  - 1 síťové karty na správu virtuální síť s veřejnou IP adresu
    - Skupina zabezpečení sítě se používá k omezení provozu (vstup/výstup) do konkrétního zdroje
  - Není připojený k doméně


- (2) virtuální počítače webové vrstvy
  - (2) role serveru služby IIS - 1 na virtuální počítač
  - (2) síťové adaptéry připojené k provozní síti - 1 na virtuální počítač
  - Není připojený k doméně


- (2) virtuální počítače úrovně biz
  - (2) síťové adaptéry připojené k provozní síti - 1 na virtuální počítač
  - Není připojený k doméně


- (2) data úrovně virtuálních počítačů
  - (2) síťové adaptéry připojené k provozní síti - 1 na virtuální počítač
  - Není připojený k doméně

Skupiny dostupnosti
- (1) active Directory virtuálního počítače řadiče domény nastavit - 2 virtuální počítače
- (1) webové vrstvy virtuálního počítače nastavené – virtuální počítače 2
- (1) virtuální počítač na úrovni biz nastavené – virtuální počítače 2
- (1) nastavte virtuální počítač na úrovni data – 2 virtuální počítače

Load Balancer
- (1) webové nástroje pro vyrovnávání zatížení vrstvy
- (1) nástroj pro vyrovnávání zatížení vrstvy biz
- (1) nástroj pro vyrovnávání zatížení vrstvy data

Úložiště
- (14) účty úložiště celkový počet
  - Skupinu dostupnosti řadiče domény služby Active Directory
    - (2) primární účtů místně redundantního úložiště (LRS) - 1 pro každý virtuální počítač  
    - (1) diagnostiky účet místně redundantního úložiště (LRS) pro přidá skupinu dostupnosti
  - Jumpbox správy virtuálního počítače
    - (1) pro virtuální počítač Jumpbox účet primární místně redundantní úložiště (LRS)
    - (1) diagnostiky účet místně redundantního úložiště (LRS) pro virtuální počítač Jumpbox
  - Virtuální počítače webové vrstvy
    - (2) primární účtů místně redundantního úložiště (LRS) - 1 pro každý virtuální počítač  
    - (1) diagnostiky účet místně redundantního úložiště (LRS) pro Web úroveň dostupnosti
  - Virtuální počítače úrovně Biz
    - (2) primární účtů místně redundantního úložiště (LRS) - 1 pro každý virtuální počítač  
    - (1) diagnostiky účet místně redundantního úložiště (LRS) pro Biz úroveň dostupnosti
  - Virtuální počítače datové vrstvy
    - (2) primární účtů místně redundantního úložiště (LRS) - 1 pro každý virtuální počítač  
    - (1) diagnostiky účet místně redundantního úložiště (LRS) pro Data úroveň dostupnosti

### <a name="deployment-architecture"></a>Architektura nasazení:

**V místní síti**: privátní místní síť implementovaná v organizaci.

**Virtuální síť výroby**: produkční [VNet](https://docs.microsoft.com/azure/Virtual-Network/virtual-networks-overview) (virtuální sítě) je hostitelem aplikace a jiných provozní prostředky spuštěné v Azure. Každá virtuální síť může obsahovat několik podsítí, které se používají pro izolování a správu síťového provozu.

**Webová vrstva**: zpracovává příchozí požadavky HTTP. Odpovědi se vrátí přes tuto úroveň.

**Obchodní vrstva**: implementuje obchodní procesy a další logiku funkčnosti systému.

**Databáze úrovně**: poskytuje úložiště trvalých dat pomocí [SQL Server skupin dostupnosti Always On](https://msdn.microsoft.com/library/hh510230.aspx) pro zajištění vysoké dostupnosti. Zákazníci můžou využít [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview) jako alternativu PaaS.

**Brána**: [VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) poskytuje připojení mezi směrovači v místní síti a produkční virtuální sítě.

**Internetové brány a veřejné IP adresy**: Internetová brána zpřístupňuje aplikačními službami, abyste uživatelům prostřednictvím Internetu. Přístup k těmto službám provoz je zabezpečený pomocí [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) nabízí směrování vrstvy 7 a funkce ochrany webové brány firewall (WAF) služby Vyrovnávání zatížení.

**Správa virtuální sítě**: to [VNet](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) obsahuje prostředky, které implementují správu a monitorování pro úlohy běžící v produkčním prostředí virtuální sítě.

**Jumpbox**: zkratka [bastion host](https://en.wikipedia.org/wiki/Bastion_host), což je zabezpečený virtuální počítač v síti, který správci používají pro připojení k virtuálním počítačům v produkčním prostředí virtuální sítě. Jumpbox má skupinu NSG, která umožňuje vzdálenou komunikaci pouze z jedné veřejné IP adresy na seznamu bezpečných adres. Zdroje přenosů pro povolení provozu vzdálené plochy (RDP), musí být definován v této skupině. Správa produkčních prostředků je přes protokol RDP pomocí zabezpečeného virtuálního počítače s Jumpbox.

**Trasy definované uživatelem**: [trasy definované uživatelem](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) se používají k definování tok přenosů IP ve virtuálních sítích Azure.

**Sítě v partnerském vztahu virtuálních sítí**: produkčním prostředí a správu virtuálních sítí jsou propojeny pomocí [VNet peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).
Tyto virtuální sítě jsou i nadále spravovány jako samostatné prostředky, ale pro tyto virtuální počítače všechny účely připojení jeví jako jedna. Tyto sítě spolu komunikovat přímo pomocí privátních IP adres. Služba VNet peering je v souladu s virtuálními sítěmi ve stejné oblasti Azure.

**Skupiny zabezpečení sítě**: [skupiny Nsg](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) obsahují seznamy řízení přístupu, která povolují nebo zakazují provoz ve virtuální síti. Skupiny zabezpečení sítě slouží k zabezpečení provozu na úrovni jednotlivých virtuálních počítačů nebo podsítě.

**Active Directory Domain Services (AD DS)**: Tato architektura poskytuje vyhrazený [Active Directory Domain Services](https://technet.microsoft.com/library/hh831484.aspx) nasazení.

**Protokolování a auditování**: [protokolu aktivit Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) zachycuje operace s prostředky ve vašem předplatném, jako je například kdo inicioval operaci, při operaci došlo k chybě, stav operace a hodnoty Další vlastnosti, které vám mohou pomoci při zkoumání operace. Protokol aktivit v Azure je služba platformy Azure, který explicitně zaznamenává všechny akce v rámci předplatného. Protokoly můžete archivovat nebo exportovat podle potřeby.

**Monitorování a upozorňování sítě**: [Azure Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) je služba platformy poskytuje zachytávání paketů sítě, protokolování toků, topologie nástroje a diagnostiky pro traffics sítě v rámci virtuální sítě.

## <a name="guidance-and-recommendations"></a>Pokyny a doporučení

### <a name="business-continuity"></a>Kontinuita podnikových procesů

**Vysoká dostupnost**: Server úlohy jsou seskupené ve [dostupnosti](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) k zajištění vysoké dostupnosti virtuálních počítačů v Azure. Tato konfigurace pomáhá zajistit, že během plánované i neplánované údržby alespoň jeden virtuální počítač bude mít k dispozici a musí splňovat 99,95 % Azure SLA.

### <a name="logging-and-audit"></a>Protokolování a auditování

**Monitorování**: [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) je služba platformy, která poskytuje jeden zdroj pro monitorování protokolu aktivit, metriky a diagnostické protokoly ze všech vašich prostředků Azure. Azure Monitor můžete nastavit vizualizace, dotazy, směrovat, archivace a jednat na základě metriky a protokoly pocházející z prostředků v Azure. Doporučuje se, že řízení přístupu na základě prostředku se používá k zabezpečení záznam pro audit, abychom zajistili, že uživatelé nemají k dispozici je možnost Upravit protokoly.

**Protokoly aktivit**: konfigurace [protokolů aktivit Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) k poskytování přehledů o operacích provedených na prostředky ve vašem předplatném.

**Diagnostické protokoly**: [diagnostické protokoly](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) jsou všechny protokoly generované prostředek. Tyto protokoly mohou zahrnovat protokoly událostí systému Windows, objektů blob, tabulky a fronty protokoly.

**Brána firewall protokoly**: Application Gateway poskytuje úplnou diagnostiku a protokoly přístupů. Protokoly brány firewall jsou dostupné pro prostředky služby Application Gateway, které mají povolený Firewall webových aplikací.

**Archivace protokolu**: protokol úložiště dat je možné nakonfigurovat pro zápis do centralizované úložiště Azure pro účet, archivace a období definované uchovávání informací. Protokoly je možné zpracovávat pomocí Azure Log Analytics nebo systémy jiných výrobců SIEM.

### <a name="identity"></a>Identita

**Active Directory Domain Services**: Tato architektura přináší nasazení služby Active Directory Domain Services v Azure. Konkrétní doporučení ohledně implementace Active Directory v Azure najdete v následujících článcích:

[Rozšíření služby Active Directory Domain Services (AD DS) do Azure](https://docs.microsoft.com/azure/guidance/guidance-identity-adds-extend-domain).

[Pokyny pro nasazení systému Windows Server Active Directory na virtuálních počítačích Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx).

**Integrace služby Active Directory**: jako alternativu k vyhrazené architektury služby AD DS, mohou zákazníci chtít používat [Azure Active Directory](https://docs.microsoft.com/azure/guidance/guidance-ra-identity#using-azure-active-directory) integrace nebo [služby Active Directory v Azure připojené k místní doménová struktura](https://docs.microsoft.com/azure/guidance/guidance-ra-identity#using-active-directory-in-azure-joined-to-an-on-premises-forest).

### <a name="security"></a>Zabezpečení

**Správa zabezpečení**: podrobný plán umožňuje správcům připojit k rozhraní pro správu virtuálních sítí a Jumpbox pomocí protokolu RDP z důvěryhodného zdroje. Přenos v síti pro virtuální síť pro správu je řízen pomocí skupin zabezpečení sítě. Přístup k portu 3389 je omezen na provoz z důvěryhodných IP rozsahu, s přístupem k podsíť obsahující Jumpbox.

Zákazníci mohou také zvážit použití [zvýšené zabezpečení pro správu modelu](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/securing-privileged-access) pro zabezpečení prostředí při připojování ke službě pro správu virtuálních sítí a Jumpbox. Doporučuje se, že pro zvýšení zabezpečení zákazníci používat [Privileged Access pracovní stanice](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations#what-is-a-privileged-access-workstation-paw) a konfigurace RDGateway. Použití síťových virtuálních zařízení a veřejného/soukromého zóny DMZ nabídne další vylepšení zabezpečení.

**Zabezpečení sítě**: [skupiny zabezpečení sítě](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (Nsg) doporučují pro každou podsíť pro druhou úroveň ochrany u příchozích přenosů obejít nesprávně nakonfigurované nebo zakázané brány. Příklad – [šablony Resource Manageru pro nasazení skupiny NSG](https://github.com/mspnp/template-building-blocks/tree/v1.0.0/templates/buildingBlocks/networkSecurityGroups).

**Zabezpečení koncových bodů veřejných**: Internetová brána zpřístupňuje aplikačními službami, abyste uživatelům prostřednictvím Internetu. Provoz přístup k těmto službám je zabezpečený pomocí [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction), který poskytuje protokol správy brány Firewall webových aplikací a HTTPS.

**Rozsahy IP adres**: rozsahy IP adres v architektuře, která jsou navrhované rozsahy. Zákazníkům doporučujeme zvážit svoje vlastní prostředí a použijte příslušné rozsahy.

**Hybridní připojení**: cloudové úlohy, které jsou připojeny k místním datovým centrem pomocí protokolu IPSEC VPN s použitím Azure VPN Gateway. Zákazníci by měl zajistit, že používají příslušné brány sítě VPN pro připojení k Azure. Příklad – [VPN Gateway Resource Manageru šablony](https://github.com/mspnp/template-building-blocks/tree/v1.0.0/templates/buildingBlocks/vpn-gateway-vpn-connection). Zákazníky, kteří používají ve velkém měřítku, nejdůležitější úlohy s velkými objemy dat požadavky chtít zvážit použití architektury hybridní sítě [ExpressRoute](https://docs.microsoft.com/azure/guidance/guidance-hybrid-network-expressroute) privátní síťové připojení k Microsoft cloud services.

**Oddělení oblastí zájmu**: Tato referenční architektura oddělí virtuální sítě pro operace správy a obchodních operací. Samostatné virtuální sítě a podsítě povolit správu provozu, včetně omezení pro příchozí a odchozí provoz, s použitím skupin zabezpečení sítě mezi síťové segmenty následující [zabezpečení cloudové služby a sítě Microsoft](https://docs.microsoft.com/azure/best-practices-network-security) osvědčené postupy.

**Správa prostředků**: spravuje prostředky Azure, jako jsou virtuální počítače, virtuální sítě a nástroje pro vyrovnávání zatížení seskupí společně do [skupin prostředků Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groupsresource). Řízení přístupu na základě role prostředků pak můžete přiřadit ke každé skupině prostředků k omezení přístupu jenom na autorizované uživatele.

**Přístup k omezení ovládacích prvků**: použití [řízení přístupu na základě Role](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) (RBAC) ke správě prostředků v aplikaci pomocí [vlastní role](https://docs.microsoft.com/azure/role-based-access-control/custom-roles) RBAC slouží k omezení operací, které DevOps můžete provádět na jednotlivých úrovních. Při udělování oprávnění používat [principu nejnižších možných oprávnění](https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1). Protokolujte všechny operace správy a provádějte pravidelné audity, abyste měli jistotu, že byly všechny změny konfigurace plánované.

**Přístup k Internetu**: Tato referenční architektura využívá [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) jako internetové brány a load balancer. Někteří zákazníci mohou také zvážit použití síťových virtuálních zařízení jiných výrobců pro další vrstvy zabezpečení jako alternativu k připojení sítě [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction).

**Azure Security Center**: [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) poskytuje centralizovaný pohled na stav zabezpečení prostředků v předplatném a poskytuje doporučení, která pomáhá zabránit ohrožených prostředků. To také umožňuje povolit podrobnější zásady. Zásady můžete například použít na určitých skupinách prostředků, které mohou přizpůsobit jeho stav k riziku. Doporučujeme, aby zákazníci s Azure Security Center v rámci svého předplatného Azure.

## <a name="ncsc-cloud-security-principles-compliance-documentation"></a>Dokumentace k NCSC cloudové zabezpečení zásady dodržování předpisů

Komerční služby koruna (subjekt, který lze použít ke zlepšení obchodních a zajišťování aktivity vláda) obnovit klasifikace cloudovým službám Microsoftu enterprise v oboru pro v6 G-Cloud, pokrývá všechny nabídky na úrovni OFFICIAL. Podrobnosti o službě Azure a G-Cloud najdete v [souhrn vyhodnocení zabezpečení Azure UK G-Cloud](https://www.microsoft.com/en-us/trustcenter/compliance/uk-g-cloud).

Tento podrobný plán zarovná zásadám 14 cloudové zabezpečení, které jsou popsané v NCSC [principů zabezpečení cloudu](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) k zajištění prostředí, které podporuje úlohy, které jsou klasifikovány jako UK-OFFICIAL.

[Matici zodpovědnosti zákazníků](https://aka.ms/ukofficial-crm) (Excelový sešit) obsahuje seznam všech principů zabezpečení cloudu 14 a matice označuje, pro každou zásadu (nebo její hlavní komponenty), zda implementace Princip zodpovídá za Microsoft, Zákazník, nebo sdílené mezi nimi.

[Princip implementace matice](https://aka.ms/ukofficial-iaaswa-pim) (Excelový sešit) zobrazí všechny zásady zabezpečení cloudu 14 a matice označuje, pro každou zásadu (nebo její hlavní komponenty), který je určen odpovědnosti zákazníka v zákazníka (1) Pokud podrobného plánu automation implementuje princip a (2) popis, jak implementace v souladu s principem požadavků: matici zodpovědnosti.

Kromě toho Cloud Security Alliance (CSA) publikované ovládací prvek matice Cloud podporu pro zákazníky v pořadí vyhodnocování poskytovatelů cloudových služeb a identifikovat dotazy, které by měl být zodpovědět, než přechod ke cloudovým službám. V odpovědi, Microsoft Azure odpovědi iniciativy dotazník pro posouzení CSA Caiq ([CSA CAIQ](https://www.microsoft.com/en-us/TrustCenter/Compliance/CSA)), která popisuje, jak Microsoft přistupuje k navrhované zásady.

## <a name="deploy-the-solution"></a>Nasazení řešení

Existují dvě metody, které nasazení uživatelé mohou využít k nasazení tohoto podrobného plánu automation. První metoda používá skript prostředí PowerShell, zatímco druhý způsob využívá Azure portal pro tuto referenční architekturu nasadit. Podrobné pokyny jsou k dispozici [tady](https://aka.ms/ukofficial-iaaswa-repo).

## <a name="disclaimer"></a>Právní omezení

 - Tento dokument slouží pouze k informačním účelům. MICROSOFT NEPOSKYTUJE ŽÁDNÉ ZÁRUKY, VÝSLOVNÝCH, ODVOZENÝCH NEBO ZÁKONNÝCH, INFORMACE V TOMTO DOKUMENTU. Tento dokument se poskytuje "jako-je." Informace a názory vyjádřené v tomto dokumentu včetně adres URL a jiných odkazů na internetové weby, mohou změnit bez předchozího upozornění. Tento dokument, zákazníci nese riziko jeho použití.
 - Tento dokument neposkytuje žádná zákonná práva na duševní vlastnictví v libovolném produkt společnosti Microsoft nebo řešení zákazníkům.
 - Zákazníci mohou kopírovat a používat tento dokument pro interní referenční účely.
 - Některá doporučení v tomto dokumentu může vést k vyšší dat, sítě nebo výpočetní využití prostředků v Azure a můžou zvýšit náklady Azure licenci předplatného zákazníka.
 - Tato architektura má sloužit jako základ pro zákazníky, chcete-li upravit jejich konkrétním požadavkům by se neměl používat jako-je v produkčním prostředí.
 - Tento dokument je mimo jiné vyvinuto odkaz a neměl by se definovat všechny prostředky, které zákazník dokáže splnit požadavky na konkrétní dodržování předpisů a nařízení. Zákazníci by měl hledání právní podporu – od organizace na implementace schválené zákazníků.
