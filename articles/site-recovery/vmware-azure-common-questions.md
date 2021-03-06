---
title: Časté otázky – replikace z VMware do Azure pomocí Azure Site Recovery | Dokumentace Microsoftu
description: Tento článek shrnuje běžné otázky při replikaci místních virtuálních počítačů VMware do Azure pomocí Azure Site Recovery
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.date: 07/19/2018
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: fe20cae4c316462e3af3f0a5e7e6052f6ba5719d
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344419"
---
# <a name="common-questions---vmware-to-azure-replication"></a>Časté otázky – VMware pro replikaci Azure

Tento článek obsahuje odpovědi na běžné dotazy, které můžeme vidět při replikaci místních virtuálních počítačů VMware do Azure. Pokud po přečtení tohoto článku máte dotazy, zveřejněte na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Obecné
### <a name="how-is-site-recovery-priced"></a>Jak se účtuje Site Recovery?
Kontrola [ceny za Azure Site Recovery](https://azure.microsoft.com/en-in/pricing/details/site-recovery/) podrobnosti.

### <a name="how-do-i-pay-for-azure-vms"></a>Jak zaplatím za virtuální počítače Azure?
Během replikace data se replikují do služby Azure storage a neplatíte žádné změny virtuálního počítače. Při spuštění převzetí služeb při selhání do Azure Site Recovery automaticky vytvoří virtuální počítače Azure IaaS. Následně se vám budeme fakturovat výpočetní prostředky, které využívají v Azure.

### <a name="what-can-i-do-with-vmware-to-azure-replication"></a>Co je možné? s VMware pro replikaci Azure
- **Zotavení po havárii**: můžete nastavit úplné havárii. V tomto scénáři replikaci místních virtuálních počítačů VMware do Azure storage. Potom Pokud není k dispozici v místní infrastruktuře, můžete převzít služby do Azure. Při převzetí služeb při selhání, se vytvoří virtuální počítače Azure pomocí replikovaná data. Aplikace a úlohy na virtuálních počítačích Azure, mají přístup až do vašeho místního datového centra je opět k dispozici. Pak můžete předat zpět z Azure do místní lokality.
- **Migrace**: Site Recovery můžete použít k migraci místních virtuálních počítačů VMware do Azure. V tomto scénáři replikaci místních virtuálních počítačů VMware do Azure storage. Pak převezme služby při selhání z místního Azure. Po převzetí služeb při selhání jsou vaše aplikace a úlohy dostupné a spuštěné na virtuálních počítačích Azure.



## <a name="azure"></a>Azure
### <a name="what-do-i-need-in-azure"></a>Co musím udělat v Azure?
Budete potřebovat předplatné Azure, trezor služby Recovery Services, účet úložiště a virtuální síť. Trezor, účet úložiště a síť musí být ve stejné oblasti.

### <a name="what-azure-storage-account-do-i-need"></a>Který účet úložiště Azure budu potřebovat?
Budete potřebovat účet úložiště LRS nebo GRS. Doporučujeme účet úložiště GRS, aby byla zajištěna odolnost dat v případě oblastního výpadku nebo pokud není možné obnovit primární oblast. Premium storage podporuje.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Oprávnění k vytvoření virtuálních počítačů musí Můj účet Azure?
Pokud si nejste správce předplatného, máte oprávnění replikace, které potřebujete. Pokud si nejste, budete potřebovat oprávnění k vytvoření virtuálního počítače Azure ve skupině prostředků a virtuální síť, kterou zadáte při konfiguraci Site Recovery a oprávnění k zápisu do vybraného účtu úložiště. [Další informace](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines).



## <a name="on-premises"></a>Lokálně

### <a name="what-do-i-need-on-premises"></a>K čemu potřebuji místní?
Na místním potřebujete součásti Site Recovery nainstalovaná na jeden virtuální počítač VMware. Budete potřebovat infrastrukturu VMware s minimálně jednoho hostitele ESXi, a doporučujeme použít vCenter server. Kromě toho budete potřebovat jeden nebo více virtuálních počítačů VMware pro replikaci. [Další informace](vmware-azure-architecture.md) o replikaci z VMware do Azure architektury.

Místní konfigurační server je možné nasadit v jednom z následujících způsobů

1. Nasazení pomocí šablony virtuálního počítače, který má předinstalované konfigurační server. [Tady najdete další](vmware-azure-tutorial.md#download-the-vm-template).
2. Nasaďte ji pomocí instalace na počítač s Windows serverem 2016 podle vašeho výběru. [Tady najdete další](physical-azure-disaster-recovery.md#set-up-the-source-environment).

Chcete-li zjistit Začínáme kroky nasazení konfiguračního serveru na vlastním počítači s Windows serverem v cíle ochrany povolit ochranu, zvolte **do Azure > nevirtualizované/jiné**.

### <a name="where-do-on-premises-vms-replicate-to"></a>Kde replikace místních virtuálních počítačů do?
Data se replikují do úložiště Azure. Při spuštění převzetí služeb při selhání Site Recovery automaticky vytvoří virtuální počítače Azure z účtu úložiště.

### <a name="what-apps-can-i-replicate"></a>Jaké aplikace můžou replikovat?
Můžete replikovat jakoukoli aplikaci nebo úlohu spuštěnou na virtuálním počítači VMware, který splňuje [požadavky na replikaci](vmware-physical-azure-support-matrix.md##replicated-machines). Site Recovery poskytuje podporu pro replikaci se sledováním aplikací, tak, aby aplikace, můžete převzetí služeb při selhání a zpět do použitelného stavu se nezdařilo. Site Recovery se integruje s aplikacemi Microsoftu, například SharePoint, Exchange, Dynamics, SQL Server a Active Directory a úzce spolupracuje s předními dodavateli včetně Oracle, SAP, IBM a Red Hat. [Další informace](site-recovery-workload.md) o ochraně úloh.

### <a name="can-i-replicate-to-azure-with-a-site-to-site-vpn"></a>Můžete replikovat do Azure pomocí VPN typu site-to-site?
Site Recovery replikuje data z místních do úložiště Azure přes veřejný koncový bod, nebo pomocí veřejného partnerského vztahu ExpressRoute. Replikace přes síť VPN typu site-to-site se nepodporuje.

### <a name="can-i-replicate-to-azure-with-expressroute"></a>Můžete replikovat do Azure pomocí ExpressRoute?
Ano, je možné replikovat virtuální počítače Azure ExpressRoute. Site Recovery replikuje data do účtu úložiště Azure přes veřejný koncový bod a je potřeba nastavit [veřejného partnerského vztahu](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) replikace Site Recovery. Po převzetí služeb virtuálních počítačů do služby Azure virtual network, k nim můžete přistupovat pomocí [soukromého partnerského vztahu](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).


### <a name="why-cant-i-replicate-over-vpn"></a>Proč se můžu replikovat přes síť VPN?

Při replikaci do Azure, provoz replikace dosáhne veřejné koncové body účtu úložiště Azure, proto vám můžou replikovat jenom přes veřejný internet s ExpressRoute (veřejný partnerský vztah) a VPN nebude fungovat.



## <a name="what-are-the-replicated-vm-requirements"></a>Jaké jsou požadavky na replikovaných virtuálních počítačů?

Pro účely replikace virtuálního počítače VMware musí běžet podporovaný operační systém. Kromě toho virtuální počítač musí splňovat požadavky pro virtuální počítače Azure. [Další informace](vmware-physical-azure-support-matrix.md##replicated-machines) v matici podpory.

## <a name="how-often-can-i-replicate-to-azure"></a>Jak často můžete replikovat do Azure?
Při replikaci virtuálních počítačů VMware do Azure je průběžné replikace.

## <a name="can-i-extend-replication"></a>Je možné rozšířit replikaci?
Rozšířená nebo zřetězená replikace není podporována. Žádost o tuto funkci v [fóru pro zpětnou vazbu](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication).

## <a name="can-i-do-an-offline-initial-replication"></a>Můžete provést offline počáteční replikaci?
Toto není podporováno. Žádost o tuto funkci [fóru pro zpětnou vazbu](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-disks"></a>Vyloučení disků
Ano, můžete vyloučit disky z replikace.

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>Můžete replikovat virtuální počítače s dynamickými disky?
Dynamické disky se dají replikovat. Disk s operačním systémem musí být základní disk.

### <a name="if-i-use-replication-groups-for-multi-vm-consistency-can-i-add-a-new-vm-to-an-existing-replication-group"></a>Pokud budu používat skupiny replikace konzistenci, můžu přidat nový virtuální počítač do existující skupiny replikace?
Ano, můžete přidat nové virtuální počítače do existující skupiny replikace, když povolíte replikaci pro ně. Virtuální počítač nelze přidat do existující skupiny replikace po replikaci se zahájí, a nelze vytvořit replikační skupinu pro stávající virtuální počítače.

### <a name="can-i-modify-vms-that-are-replicating-by-adding-or-resizing-disks"></a>Můžete upravit virtuální počítače, které se replikují přidáním nebo změnou velikosti disků?

Pro replikaci VMware do Azure můžete upravit velikost disku. Pokud chcete přidat nové disky, které potřebujete přidat disk a znovu povolit ochranu virtuálního počítače.

## <a name="configuration-server"></a>Konfigurační server

### <a name="what-does-the-configuration-server-do"></a>Co dělá konfiguračního serveru?
Konfigurační server běží v místním součásti Site Recovery, včetně:
- Konfigurační server koordinuje komunikaci mezi místním a Azure a spravuje replikaci dat.
- Procesový server, který funguje jako replikační brána. Přijímá data replikace; optimalizuje je pomocí ukládání do mezipaměti, komprese a šifrování. a odešle ho do úložiště Azure., že procesový server na virtuálních počítačích, které chcete replikovat a provádí automatické zjišťování virtuálních počítačů VMware v místním také nainstaluje službu Mobility.
- Hlavní cílový server zpracovává replikační data během navrácení služeb po obnovení z Azure.

### <a name="where-do-i-set-up-the-configuration-server"></a>Kde můžu nastavit konfigurační server?
Potřebujete jeden vysoce dostupný místní VMware virtuálního počítače pro konfigurační server.

### <a name="what-are-the-requirements-for-the-configuration-server"></a>Jaké jsou požadavky pro konfigurační server?

Zkontrolujte [požadavky](vmware-azure-deploy-configuration-server.md#prerequisites).

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>Můžete ručně nastavit konfigurační server, namísto použití šablony?
Doporučujeme používat nejnovější verzi šablony OVF pro [vytvoření virtuálního počítače konfiguračního serveru](vmware-azure-deploy-configuration-server.md). Pokud z nějakého důvodu nemůžete, například nemáte přístup k serveru VMware, můžete si [stáhnout soubor sjednocené instalace](physical-azure-set-up-source.md) z portálu, a spusťte na virtuálním počítači.

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>Konfigurační server možná replikace do více než jedné oblasti?
Ne. K tomu potřeba nastavit konfigurační server v jednotlivých oblastech.

### <a name="can-i-host-a-configuration-server-in-azure"></a>Můžete hostovat konfigurační server v Azure?
Při nejbližším virtuálnímu počítači Azure s konfiguračního serveru potřebuje komunikovat s virtuálními počítači a na místní infrastrukturu VMware. Může přidat latenci a mít vliv na probíhající replikaci.


### <a name="where-can-i-get-the-latest-version-of-the-configuration-server-template"></a>Kde lze získat nejnovější verzi šablona konfiguračního serveru?
Stáhněte si nejnovější verzi z [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

### <a name="how-do-i-update-the-configuration-server"></a>Jak můžu aktualizovat konfigurační server?
Instalaci kumulativní aktualizace. Můžete najít informace o nejnovější aktualizaci v [aktualizace wikistránka](https://social.technet.microsoft.com/wiki/contents/articles/38544.azure-site-recovery-service-updates.aspx).

### <a name="should-i-backup-the-deployed-configuration-server"></a>Můžu zálohovat nasazené konfiguračního serveru?
Doporučujeme pravidelných naplánovaných záloh konfiguračního serveru. Úspěšné navrácení služeb po obnovení se při navrácení služeb obnoví virtuální počítač musí existovat v databázi konfigurací serveru a konfiguračního serveru musí být spuštěn a v připojeném stavu. Další informace o běžných úloh správy serveru konfigurace [tady](vmware-azure-manage-configuration-server.md).

## <a name="mobility-service"></a>Služba Mobility

### <a name="where-can-i-find-the-mobility-service-installers"></a>Kde najdu instalace služby Mobility?
Instalační programy jsou uloženy v **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository** složky na konfiguračním serveru.

## <a name="how-do-i-install-the-mobility-service"></a>Jak nainstalovat službu Mobility?
Nainstalujete na každý virtuální počítač, který chcete replikovat, použití [nabízená instalace](vmware-azure-install-mobility-service.md#install-mobility-service-by-push-installation-from-azure-site-recovery), nebo ruční instalace z [rozhraní](vmware-azure-install-mobility-service.md#install-mobility-service-manually-by-using-the-gui), nebo [pomocí prostředí PowerShell](vmware-azure-install-mobility-service.md#install-mobility-service-manually-at-a-command-prompt). Alternativně můžete nasadit pomocí nástroje nasazení, jako [System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md), nebo [Azure Automation a DSC](vmware-azure-mobility-deploy-automation-dsc.md).



## <a name="security"></a>Zabezpečení

### <a name="what-access-does-site-recovery-need-to-vmware-servers"></a>Jaký přístup Site Recovery potřebuje servery VMware?
Site Recovery potřebuje přístup k serverům VMware z těchto důvodů:

- Nastavení virtuálního počítače VMware s konfiguračním serverem a další součásti Site Recovery v místním. [Další informace](vmware-azure-deploy-configuration-server.md)
- Automatické zjišťování virtuálních počítačů pro replikaci. Další informace o přípravě účtu pro automatické zjišťování. [Další informace](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery)


### <a name="what-access-does-site-recovery-need-to-vmware-vms"></a>Jaký přístup Site Recovery potřebuje virtuální počítače VMware?

- Pokud chcete replikovat, musí mít Virtuálním počítači VMware službu Site Recovery Mobility nainstalovaná a spuštěná. Můžete nasadit nástroj ručně, nebo zadat Site Recovery proveďte nabízenou instalaci služby při povolení replikace pro virtuální počítač. Site Recovery pro nabízenou instalaci, potřebuje účet, který můžete použít k instalaci součásti service. [Další informace](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-mobility-service-installation). Procesový server (spuštěné ve výchozím nastavení na konfiguračním serveru) provádí tuto instalaci. [Další informace](vmware-azure-install-mobility-service.md) o instalace služby Mobility.
- Během replikace virtuálních počítačů komunikovat s využitím Site Recovery následujícím způsobem:
    - Virtuální počítače komunikovat s konfiguračním serverem na port HTTPS 443 pro správu replikace.
    - Virtuální počítače odesílají data do procesového serveru na příchozím portu HTTPS 9443 (je možné upravit).
    - Pokud povolíte konzistenci pro více virtuálních počítačů, virtuálních počítačů vzájemně komunikovat přes port 20004.


### <a name="is-replication-data-sent-to-site-recovery"></a>Replikační data odeslaná do Site Recovery?
Ne, Site Recovery nezachycuje replikovaná data se nepodporuje a nemá žádné informace o co běží na virtuálních počítačích. Replikovaná data se přemisťují mezi hypervisory VMware a Azure storage. Site Recovery nemá schopnost tato data zachytit. Do služby Site Recovery se odesílají jen metadata, která jsou nezbytná k orchestraci replikace a převzetí služeb při selhání.  

Site Recovery je ISO 27001: 2013, 27018, HIPAA, DPA certifikace a probíhá SOC2 a FedRAMP JAB.

### <a name="can-we-keep-on-premises-metadata-within-a-geographic-regions"></a>Jsme zachovat místní metadata v rámci zeměpisné oblasti?
Ano. Když vytvoříte trezor služby v oblasti, zajišťujeme, že všechna metadata používat Site Recovery zůstane v rámci dané oblasti geografické hranice.

### <a name="does-site-recovery-encrypt-replication"></a>Šifruje Site Recovery replikaci?
Ano, jak šifrování během přenosu a [šifrování v Azure](https://docs.microsoft.com/azure/storage/storage-service-encryption) jsou podporovány.


## <a name="failover-and-failback"></a>Převzetí služeb při selhání a navrácení služeb po obnovení
### <a name="how-far-back-can-i-recover"></a>Jak daleko mohou obnovit?
Nejstarší bod obnovení, které můžete použít pro replikaci z VMware do Azure je 72 hodin.

### <a name="how-do-i-access-azure-vms-after-failover"></a>Jak získám přístup virtuálních počítačů Azure po převzetí služeb při selhání?
Po převzetí služeb při selhání můžete přístup k virtuálním počítačům Azure přes zabezpečené internetové připojení, přes síť site-to-site VPN nebo přes Azure ExpressRoute. Budete muset připravit řada věcí, abyste se mohli připojit. [Další informace](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### <a name="is-failed-over-data-resilient"></a>Se převzaly při selhání data odolná?
Služba Azure je pro odolnost navržena. Site Recovery je navržena pro převzetí služeb při selhání do sekundárního datacentra Azure, v souladu s smlouvy SLA pro Azure. Pokud dojde k převzetí služeb při selhání, zajišťujeme, že metadata a trezory zůstávají ve stejné geografické oblasti, kterou jste zvolili pro svůj trezor.

### <a name="is-failover-automatic"></a>Je převzetí služeb při selhání automatické?
[Převzetí služeb při selhání](site-recovery-failover.md) neprobíhá automaticky. Zahájení převzetí služeb při selhání s jedním kliknutím na portálu, nebo můžete použít [ Powershellu](/powershell/module/azurerm.siterecovery) k aktivaci převzetí služeb při selhání.

### <a name="can-i-fail-back-to-a-different-location"></a>Můžu navrátit služby zpět do jiného umístění?
Ano, pokud převzetí služeb při selhání do Azure, můžete navrátit služby zpět do jiného umístění Pokud původní není k dispozici. [Další informace](concepts-types-of-failback.md#alternate-location-recovery-alr).

### <a name="why-do-i-need-a-vpn-or-expressroute-to-fail-back"></a>Proč potřebuji síť VPN nebo ExpressRoute k navrácení služeb po obnovení?

Když převezmete služby zpět z Azure, se kopírují data z Azure zpět na vaše místní virtuální počítač a soukromý přístup je povinný.



## <a name="automation-and-scripting"></a>Automatizací a skriptováním

### <a name="can-i-set-up-replication-with-scripting"></a>Můžete nastavit replikaci pomocí skriptování?
Ano. Můžete automatizovat postupy workflow Site Recovery pomocí rozhraní Rest API, Powershellu nebo sady Azure SDK. [Další](vmware-azure-disaster-recovery-powershell.md).

## <a name="performance-and-capacity"></a>Výkon a kapacita
### <a name="can-i-throttle-replication-bandwidth"></a>Je možné omezovat šířku pásma replikace?
Ano. [Další informace](site-recovery-plan-capacity-vmware.md).


## <a name="next-steps"></a>Další postup
* [Kontrola](vmware-physical-azure-support-matrix.md) požadavky na podporu.
* [Nastavit](vmware-azure-tutorial.md) replikaci z VMware do Azure replikace.
