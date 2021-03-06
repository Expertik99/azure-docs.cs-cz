---
title: Funkce zabezpečení Azure používat s virtuálními počítači Azure | Dokumentace Microsoftu
description: Tento článek obsahuje základní informace o základní funkce zabezpečení Azure, které lze použít s Azure Virtual Machines.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 467b2c83-0352-4e9d-9788-c77fb400fe54
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: fb6a984ff838305b4ce411538465c0b9b5c152da
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/24/2018
ms.locfileid: "42886910"
---
# <a name="azure-virtual-machines-security-overview"></a>Přehled zabezpečení služby Azure Virtual Machines
Azure Virtual Machines můžete pružně nasadit širokou škálu výpočetních řešení. Služba podporuje Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP a Azure BizTalk Services. Proto můžete nasadit jakoukoli úlohu a v jakémkoli jazyce na téměř jakýkoli operační systém.

Virtuální počítač Azure vám nabídne flexibilitu virtualizace bez nutnosti zakoupení a údržby fyzického hardwaru, na kterém virtuální počítač běží. Můžete vytvářet a nasazovat aplikace s jistotou, že jsou vaše data chráněná a v bezpečí ve vysoce zabezpečených datacentrech.

S Azure můžete vytvářet s rozšířeným zabezpečením, který vyhovuje řešení, které:

* Ochrana virtuálních počítačů před viry a malwarem.
* Šifrovat citlivá data.
* Zabezpečení provozu sítě.
* Identifikace a detekce hrozeb.
* Splnění požadavků na dodržování předpisů.

Cílem tohoto článku je poskytnout přehled o základní funkce zabezpečení Azure, které můžete používat s virtuálními počítači. Odkazy na články poskytují podrobnosti o každé funkce tak další informace.  

## <a name="antimalware"></a>Antimalware
S Azure můžete použít antimalwarový software od dodavatelů zabezpečení, jako je Microsoft, Symantec, Trend Micro a Kaspersky. Tento software pomáhá chránit virtuální počítače před škodlivými soubory, adwarem a dalšími hrozbami.

Microsoft Antimalware pro Azure Cloud Services a Virtual Machines je funkce ochrany v reálném čase, který pomáhá zjistit a odebrat viry, spyware a jiný škodlivý software.  Microsoft Antimalware pro Azure poskytuje konfigurovatelných upozornění, když označuje, že škodlivý nebo nežádoucí software pokusí nainstalovat nebo spustit v Azure systémech.

Microsoft Antimalware pro Azure je řešení jednoho agenta pro aplikace a prostředí tenanta. Je určený ke spouštění na pozadí bez zásahu člověka. Je možné nasadit ochranu na základě potřeb vaší aplikace úlohy s využitím buď základní zabezpečení výchozím nebo Rozšířené vlastní konfigurace, včetně antimalwarový monitorování.

Při nasazení a povolit Microsoft Antimalware pro Azure, jsou k dispozici následující základní funkce:

* **Ochrana v reálném čase**: monitoruje aktivity ve službě Cloud Services a na virtuálních počítačích ke zjištění a blokování spuštění malwaru.
* **Naplánované prohledávání**: provádí pravidelné cílové skenování pro detekci malwaru, včetně aktivně spuštěné programy.
* **Malwarové nápravy**: automaticky převezme zjištěného malwaru, jako je například odstranění nebo umístění do karantény škodlivých souborů a čištění položky registru škodlivé akce.
* **Aktualizace signatur**: automaticky nainstaluje nejnovější signatury ochrany (definice virů) k zajištění, že je v frekvenci předem určené aktuální ochranu.
* **Aktualizace antimalwarového stroje**: automaticky aktualizuje Microsoft Antimalware pro Azure modul.
* **Antimalwarová platforma aktualizace**: automaticky aktualizuje Microsoft Antimalware pro platformu Azure.
* **Aktivní ochranu**: sestavy metadat telemetrických dat do Azure informace o zjištěných hrozeb a podezřelé prostředky k zajištění rychlé reakce. Umožňuje v reálném čase synchronní podpis doručování prostřednictvím sítě Microsoft Active Protection systému (MAPS).
* **Ukázky reporting**: poskytuje a sestavám ukázky Microsoft Antimalware pro Azure service vám pomůže vylepšit službu a umožňují řešit potíže.
* **Vyloučení**: umožňuje aplikaci a správců služeb ke konfiguraci určitých souborů a procesy a řídí Pokud chcete vyloučit z ochrany a kontrolu pro výkon a z jiných důvodů.
* **Shromažďování událostí Antimalwarové**: zaznamenává stav antimalwarové služby podezřelé aktivity a nápravné akce prováděné v protokolu událostí operačního systému a shromažďuje ve vašem účtu úložiště Azure.

Další informace o antimalwarový software k ochraně virtuálních počítačů:

* [Microsoft Antimalware pro Azure Cloud Services a Virtual Machines](azure-security-antimalware.md)
* [Nasazování antimalwarových řešení na virtuálních počítačích Azure](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Jak nainstalovat a nakonfigurovat Trend Micro Deep Security jako službu na virtuálním počítači s Windows](../virtual-machines/windows/classic/install-trend.md)
* [Jak nainstalovat a nakonfigurovat Symantec Endpoint Protection na virtuálním počítači s Windows](../virtual-machines/windows/classic/install-symantec.md)
* [Řešení zabezpečení v Azure Marketplace](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Modul hardwarového zabezpečení
Zlepšení zabezpečení klíče můžete vylepšit ochranu ověřování a šifrování. Správa a zabezpečení důležitých tajných kódů a klíčů můžete zjednodušit jejich uložením ve službě Azure Key Vault. 

Key Vault umožňuje ukládat klíče v modulech zabezpečení hardwaru (HSM) s certifikací podle standardů FIPS 140-2 úrovně 2. SQL Server šifrování klíče pro zálohování nebo [transparentní šifrování dat](https://msdn.microsoft.com/library/bb934049.aspx) můžete všechny uloženy ve službě Key Vault všechny klíče nebo tajné kódy z vašich aplikací. Oprávnění a přístup k těmto chráněným položkám se spravují přes [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

Další informace:

* [Co je Azure Key Vault?](../key-vault/key-vault-whatis.md)
* [Začínáme s Azure Key Vault](../key-vault/key-vault-get-started.md)
* [Blog o Azure Key Vault](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Šifrování disku virtuálního počítače
Azure Disk Encryption je nová funkce pro šifrování disků virtuálního počítače Windows a Linux. Azure Disk Encryption používá standard odvětví v oblasti [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funkce Windows a [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt) funkce Linux zajišťuje šifrování pro operační systém a datové disky.

Toto řešení je integrovaná s Azure Key Vault a pomáhá řídit a spravovat klíče pro šifrování disků a tajné kódy ve vašem předplatném služby key vault. Zajišťuje, že všechna data na discích virtuálních počítačů jsou zašifrovaná rest ve službě Azure Storage.

Další informace:

* [Azure Disk Encryption pro virtuální počítače IaaS](../security/azure-security-disk-encryption-overview.md)
* [Rychlý start: Šifrování IaaS virtuálního počítače s Windows pomocí Azure Powershellu](../security/quick-encrypt-vm-powershell.md)

## <a name="virtual-machine-backup"></a>Záloha virtuálního počítače
Azure Backup je škálovatelné řešení, která pomáhá chránit data vaší aplikace s nulovou kapitálovou investicí a minimálními provozními náklady. Chyby aplikací můžou poškodit vaše data a lidské omyly zase můžou způsobit chyby v aplikacích. Pomocí služby Azure Backup jsou chráněné virtuální počítače s Windows a Linux.

Další informace:

* [Co je Azure Backup?](../backup/backup-introduction-to-azure-backup.md)
* [Azure Backup výuky](https://azure.microsoft.com/documentation/learning-paths/backup/)
* [Nejčastějších dotazech ke službě Azure Backup](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Azure Site Recovery
Důležitou součástí strategie BCDR organizace je tím, jak udržovat firemní úlohy a aplikace, které běží při plánovaných nebo neočekávaných výpadcích dojít. Azure Site Recovery pomáhá Orchestrace replikace, převzetí služeb při selhání a obnovení úloh a aplikací tak, že jsou k dispozici ze sekundární lokality v případě, že primární lokalita ocitne mimo provoz.

Site Recovery:

* **Tato strategie zjednodušuje**: Site Recovery umožňuje snadno zpracovat replikace, převzetí služeb při selhání a obnovení několika podnikových úloh a aplikací z jednoho místa. Site Recovery orchestruje replikace a převzetí služeb při selhání, ale nebude nezachycuje data aplikací nebo nemá žádné informace o něm.
* **Poskytuje flexibilní replikace**: pomocí Site Recovery můžete replikovat úlohy běžící na virtuálních počítačích Hyper-V, virtuálních počítačů VMware a fyzické servery Windows nebo Linuxem.
* **Podporuje převzetí služeb při selhání a obnovení**: Site Recovery poskytuje testovací převzetí služeb při selhání pro podporu zotavení po havárii, aniž to ovlivní produkční prostředí. Pro očekávané výpadky je možné spouštět plánovaná převzetí služeb při selhání bez ztráty dat. V případě neočekávaných havárií pak mohou proběhnout neplánovaná převzetí služeb s minimálními ztrátami dat (podle četnosti replikací). Po převzetí služeb při selhání můžete navrátit služby zpět do primární lokality. Site Recovery poskytuje plány obnovení, které mohou obsahovat skripty a sešity automatizace Azure, což vám umožní přizpůsobit si přebírání služeb při selhání a obnovování vícevrstvých aplikací.
* **Eliminuje sekundárních datových center**: můžete replikovat do sekundární místní lokality nebo do Azure. Použití Azure jako cíle pro zotavení po havárii se eliminují náklady a složitost spojené s udržováním sekundární lokality. Replikovaná data jsou uložena ve službě Azure Storage.
* **Se integruje s existujícími technologiemi BCDR**: Site Recovery spolupracuje s funkcemi BCDR jiných aplikací. Site Recovery můžete například použít k ochraně back end systému SQL Server u firemních úloh. To zahrnuje nativní podporu pro SQL Server AlwaysOn pro správu převzetí služeb při selhání skupiny dostupnosti.

Další informace:

* [Co je Azure Site Recovery?](../site-recovery/site-recovery-overview.md)
* [Jak funguje Azure Site Recovery?](../site-recovery/site-recovery-components.md)
* [Jaké úlohy jsou chráněné službou Azure Site Recovery?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtuální síť
Virtuální počítače vyžadují připojení k síti. Splnění tohoto požadavku Azure vyžaduje virtuální počítače připojit ke službě Azure virtual network. 

Virtuální síť Azure je logická konstrukce postavené na Azure síťových prostředcích infrastruktury. Každé logické Azure virtual network je izolovaná od všech jiným virtuálním sítím Azure. Tato izolace pomáhá zajistit, že síťový provoz v nasazeních není dostupný ostatním zákazníkům Microsoft Azure.

Další informace:

* [Přehled zabezpečení sítě Azure](security-network-overview.md)
* [Přehled služby Virtual Network](../virtual-network/virtual-networks-overview.md)
* [Síťové funkce a partnerství pro podnikové scénáře](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Správa zásad zabezpečení a generování sestav
Azure Security Center pomáhá zabránit, detekci a reakce na hrozby. Poskytuje Security Center můžete zvýšit přehled a kontrolu nad zabezpečením vašich prostředků Azure. Poskytuje integrované bezpečnostní sledování a správu zásad ve vašich předplatných Azure. To pomáhá detekovat hrozby, které jinak nevšimli a spolupracuje s řadou řešení zabezpečení.

Security Center pomáhá optimalizaci a monitorování zabezpečení virtuálních počítačů podle:

* Poskytuje [doporučení zabezpečení](../security-center/security-center-recommendations.md) pro virtuální počítače. Příklad doporučení zahrnují: použít aktualizace systému, nakonfigurujte seznamy ACL koncových bodů, zapnout Antimalware budete moct, povolit skupiny zabezpečení sítě a použít šifrování disku.
* Monitorování stavu virtuálních počítačů.

Další informace:

* [Úvod do Azure Security Center](../security-center/security-center-intro.md)
* [Azure Security Center – nejčastější dotazy](../security-center/security-center-faq.md)
* [Plánováním a provozem Azure Security Center](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Dodržování předpisů
Azure Virtual Machines jsou certifikované pro FISMA, FedRAMP, HIPAA, PCI DSS úrovně 1 a další klíčové programy dodržování předpisů. Tato certifikace usnadňuje pro vlastní Azure aplikace, které splňují požadavky na dodržování předpisů a vaší firmě snadnější plnění široké škály domácích a mezinárodních zákonné požadavky.

Další informace:

* [Centrum zabezpečení Microsoft: dodržování předpisů](https://www.microsoft.com/en-us/trustcenter/compliance)
* [Důvěryhodný Cloud: Zabezpečení Microsoft Azure, ochrana osobních údajů a dodržování předpisů](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
