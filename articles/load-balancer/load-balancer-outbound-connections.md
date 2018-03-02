---
title: "Odchozí připojení v Azure | Microsoft Docs"
description: "Tento článek vysvětluje, jak Azure umožňuje virtuálním počítačům komunikovat s služby veřejného Internetu."
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
ms.assetid: 5f666f2a-3a63-405a-abcd-b2e34d40e001
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/05/2018
ms.author: kumud
ms.openlocfilehash: 1c776d94d217622186d880352c518ad5a34b0949
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/28/2018
---
# <a name="outbound-connections-in-azure"></a>Odchozí připojení v Azure

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]


Azure poskytuje odchozí připojení pro zákaznických nasazení pomocí několika různých mechanismů. Tento článek popisuje, co jsou scénáře, kdy se vztahují, jak pracují a jak spravovat.

Nasazení v Azure může komunikovat s koncovými body mimo Azure v rámci veřejný adresní prostor IP adres. Pokud instance zahájí odchozího toku do cílového umístění v veřejný adresní prostor IP adres, Azure dynamicky mapuje privátní IP adresu na veřejnou IP adresu. Po vytvoření tohoto mapování, návratový provoz pro tuto odchozí původu toku dosáhnout taky privátní IP adresu kde toku vytvořena.

Azure používá zdroj překládání adres (překládat pomocí SNAT) a provést tuto funkci. Pokud více privátních IP adres se vydávají za jednu veřejnou IP adresu, použije Azure [portu překlad adres (Jan)](#pat) k maskovat privátních IP adres. Dočasné porty se používají pro Jan a jsou [souhrnů ještě neumístěných](#preallocatedports) na základě velikosti fondu.

Existuje více [odchozí scénáře](#scenarios). Tyto scénáře můžete kombinovat podle potřeby. Přečtěte si je pečlivě a pochopit schopnosti, omezení a vzory, protože se vztahují na váš model nasazení a aplikační scénář. Přečtěte si pokyny pro [Správa těchto scénářů](#snatexhaust).

## <a name="scenarios"></a>Přehled scénáře

Azure má dva modely nasazení hlavní: Azure Resource Manager a Klasický model. Azure nástroj pro vyrovnávání zatížení a související prostředky jsou explicitně definován při použití [Azure Resource Manager](#arm). Nasazení Classic abstraktní koncept služby Vyrovnávání zatížení a express podobné funkce prostřednictvím definice koncové body [Cloudová služba](#classic). Příslušné [scénáře](#scenarios) pro vaše nasazení závisí na modelu nasazení použijete.

### <a name="arm"></a>Azure Resource Manager

Azure aktuálně poskytuje tři různých způsobů dosažení odchozí připojení pro prostředky Azure Resource Manager. [Classic](#classic) má podmnožinu těchto scénářích nasazení.

| Scénář | Metoda | Popis |
| --- | --- | --- |
| [1. Virtuální počítač s adresu veřejná IP adresa na úrovni Instance (s nebo bez nástroj pro vyrovnávání zatížení)](#ilpip) | Překládat pomocí SNAT, port vydávají nepoužívá |Azure používá veřejnou IP adresu, které jsou přiřazené ke konfiguraci IP adresy z instance síťový adaptér. Instance má všechny dočasné porty, které jsou k dispozici. |
| [2. Veřejné nástroj pro vyrovnávání zatížení, které jsou přidružené k virtuálnímu počítači (žádná Instance úroveň veřejná IP adresa v instanci)](#lb) | Překládat pomocí SNAT s port vydávají (PAT) pomocí frontends nástroj pro vyrovnávání zatížení |Azure sdílí veřejnou IP adresu veřejné frontends nástroj pro vyrovnávání zatížení s více privátních IP adres. Azure používá dočasné porty frontends k Jan. |
| [3. Samostatný virtuální počítač (žádná služba Vyrovnávání zatížení, žádná Instance úroveň veřejná IP adresa)](#defaultsnat) | Překládat pomocí SNAT s port vydávají (PAT) | Azure automaticky označí veřejnou IP adresu pro překládat pomocí SNAT, sdílí tato veřejná IP adresa s více privátních IP adres skupiny dostupnosti a používá dočasné porty tato veřejná IP adresa. Toto je záložní scénář pro uvedených scénářů. Pokud potřebujete viditelnost a kontrolu se nedoporučuje. |

Pokud nechcete, aby virtuální počítač ke komunikaci s koncovými body mimo Azure v rámci prostor veřejných IP adres, můžete použít skupiny zabezpečení sítě (Nsg) k blokování přístupu podle potřeby. V části [nelze navázat odchozí připojení](#preventoutbound) podrobněji popisuje skupiny Nsg. Pokyny k návrhu, implementaci a správu virtuálních sítí bez jakékoli odchozí přístup je mimo rámec tohoto článku.

### <a name="classic"></a>Classic (cloudové služby)

Scénáře, které jsou k dispozici pro nasazení classic jsou podmnožinou k dispozici pro scénáře [Azure Resource Manager](#arm) nasazení a základní nástroje pro vyrovnávání zatížení.

Klasické virtuální počítač má stejné tři základní scénáře, jak je popsáno pro prostředky Azure Resource Manager ([1](#ilpip), [2](#lb), [3](#defaultsnat)). Má jenom dva scénáře classic webové role pracovního procesu ([2](#lb), [3](#defaultsnat)). [Zmírnění dopadů strategie](#snatexhaust) mít také stejnou rozdíly.

Algoritmus používaný pro [předběžné přidělování dočasné porty](#ephemeralprots) Jan pro nasazení classic je stejné jako pro nasazení prostředků Azure Resource Manager.  

### <a name="ilpip"></a>Scénář 1: Počítač s adresou veřejná IP adresa na úrovni Instance

V tomto scénáři má virtuální počítač instanci úroveň veřejné IP splnění přiřazen. Co se týče odchozí připojení, nezáleží, jestli virtuální počítač je Vyrovnávané nebo ne. Tento scénář má přednost před ostatní. Pokud se používá splnění, virtuální počítač používá splnění pro všechny odchozí toky.  

Port podvržený (Jan) se nepoužívá a virtuální počítač má všechny dočasné porty, které jsou k dispozici pro použití.

Pokud vaše aplikace iniciuje mnoho odchozí toky a dochází k vyčerpání port překládat pomocí SNAT, vezměte v úvahu přiřazení [splnění zmírnit překládat pomocí SNAT omezení](#assignilpip). Zkontrolujte [Správa překládat pomocí SNAT vyčerpání](#snatexhaust) v celé jeho šíři.

### <a name="lb"></a>Scénář 2: Vyrovnávání zatížení sítě VM bez Instance úroveň veřejné IP adresy

V tomto scénáři je virtuální počítač součástí veřejné back-endový fond Vyrovnávání zatížení. Virtuální počítač nemá veřejnou IP adresu přiřazen. Vyrovnávání zatížení prostředků musí být nakonfigurované pravidlo Vyrovnávání zatížení pro vytvoření odkazu mezi veřejnou IP front-endovou s fondem back-end. 

Pokud neprovedete tohoto pravidla konfigurace, toto chování je, jak je popsáno v tento scénář pro [samostatný virtuální počítač s žádné Instance úroveň veřejnou IP adresu](#defaultsnat). Není nutné pro pravidlo tak, aby měl pracovní naslouchací proces ve fondu back-end pro test stavu úspěšné.

Při vyrovnávání zatížení sítě virtuálního počítače vytvoří odchozí tok, znamená, že Azure privátní zdrojovou IP adresu odchozího toku veřejnou IP adresu veřejnou front-endovou Vyrovnávání zatížení. Azure pomocí této funkce překládat pomocí SNAT. Azure také používá [PAT](#pat) k maskovat více privátních IP adres za veřejnou IP adresu. 

Dočasné porty nástroje pro vyrovnávání zatížení veřejnou IP adresu front-endovou slouží k rozlišení jednotlivých toky pochází ve virtuálním počítači. Dynamicky používá překládat pomocí SNAT [souhrnů ještě neumístěných dočasné porty](#preallocatedports) vytvoření odchozí toky. V tomto kontextu se nazývají dočasné porty používané pro překládat pomocí SNAT porty překládat pomocí SNAT.

Překládat pomocí SNAT porty jsou souhrnů ještě neumístěných jak je popsáno v [překládat pomocí SNAT principy a Jana](#snat) části. Konečný prostředek, který může dojít k vyčerpání jsou. Je důležité pochopit, jak jsou [spotřebované](#pat). Zjistit informace o návrhu pro tento spotřebu a zmírnit podle potřeby zkontrolujte [Správa překládat pomocí SNAT vyčerpání](#snatexhaust).

Když [více (veřejné) IP adresy, které jsou přidružené základní nástroje pro vyrovnávání zatížení](load-balancer-multivip-overview.md), jakékoliv tyto veřejné IP adresy jsou [kandidátem na odchozí toky](#multivipsnat), a jedna je vybrána.  

Chcete-li sledovat stav odchozí připojení s základní nástroje pro vyrovnávání zatížení, můžete použít [analýzy protokolů pro vyrovnávání zatížení](load-balancer-monitor-log.md) a [výstrahy protokoly událostí](load-balancer-monitor-log.md#alert-event-log) pro monitorování překládat pomocí SNAT port vyčerpání zprávy.

### <a name="defaultsnat"></a>Scénář 3: Samostatný virtuální počítač bez Instance úroveň veřejnou IP adresu

V tomto scénáři virtuální počítač není součástí fondu vyrovnávání zatížení Azure a nemá adresu splnění přiřazen. Když virtuální počítač se vytváří odchozího toku, překládá Azure privátní zdrojovou IP adresu odchozího toku veřejné Zdrojová IP adresa. Veřejnou IP adresu použitou pro tuto odchozího toku není Konfigurovatelný a nepočítá proti odběru limitu prostředků veřejné IP. 

Azure používá překládat pomocí SNAT s vydávají port ([PAT](#pat)) a provést tuto funkci. Tento scénář je podobná [scénář 2](#lb), s výjimkou je žádnou kontrolu nad adresa IP použitá. Toto je záložní scénář pro při scénáře 1 a 2 neexistují. Pokud chcete kontrolu nad adresu odchozí nedoporučujeme tento scénář.

Překládat pomocí SNAT porty jsou souhrnů ještě neumístěných jak je popsáno v [překládat pomocí SNAT principy a Jana](#snat) části. Konečný prostředek, který může dojít k vyčerpání jsou. Je důležité pochopit, jak jsou [spotřebované](#pat). Zjistit informace o návrhu pro tento spotřebu a zmírnit podle potřeby zkontrolujte [Správa překládat pomocí SNAT vyčerpání](#snatexhaust).

### <a name="combinations"></a>Více, kombinované scénářů

Popsané v předchozích částech k dosažení konkrétní výsledek scénáře můžete kombinovat. Pokud existuje více scénářů, se vztahují pořadí podle priority: [scénář 1](#ilpip) má přednost před [scénář 2](#lb) a [3](#defaultsnat) (pouze Azure Resource Manager). [Scénář 2](#lb) přepsání [scénář 3](#defaultsnat) (Azure Resource Manager a klasický).

Příkladem je nasazení služby Správce prostředků Azure, kde založena na odchozí připojení k omezenému počtu cílů aplikace, ale také přijme příchozí toky přes front-end pro vyrovnávání zatížení. V takovém případě můžete kombinovat scénáře 1 a 2 pro pomoc. Pro další vzory, zkontrolujte [Správa překládat pomocí SNAT vyčerpání](#snatexhaust).

### <a name="multivipsnat"></a> Více frontends pro odchozí toky

Základní nástroje pro vyrovnávání zatížení vybere jeden front-endu má být použit pro odchozí toky při [více (veřejné) frontends IP](load-balancer-multivip-overview.md) jsou kandidáty pro odchozí toky. Tento výběr není konfigurovatelné a měli byste zvážit algoritmus výběr být náhodné. Konkrétní IP adresu pro odchozí toky můžete určit, jak je popsáno v [několika kombinaci scénáře](#combinations).

## <a name="snat"></a>Princip překládat pomocí SNAT a Jan

### <a name="pat"></a>Port podvržený překládat pomocí SNAT (Jan)

Pokud prostředek veřejné nástroje pro vyrovnávání zatížení je spojen s instancí virtuálních počítačů, je přepsaná každý zdroj odchozí připojení. Zdroj je přepisuje z virtuální sítě privátní adresní prostor IP adres, aby front-endu veřejná IP adresa služby Vyrovnávání zatížení. V veřejný adresní prostor IP adres 5-n-tice toku (zdrojové IP adresy, zdrojového portu, přenosového protokolu IP, cílové IP adresy, cílový port) musí být jedinečný.  

Dočasné porty (porty překládat pomocí SNAT) se používají k dosáhnout po přepisování privátní zdrojovou IP adresu, protože více toků pocházejí z jedné veřejné IP adresy. 

Jeden port překládat pomocí SNAT obsazením za toku do jedné cílové IP adresy, portu a protokolu. Pro více toků na stejnou cílovou IP adresu, port a protokol využívá každý tok jediný port překládat pomocí SNAT. Zajistíte, že tyto toky jsou jedinečné, když budou pocházet ze stejné veřejnou IP adresu, přejděte na stejnou cílovou IP adresu, port a protokol. 

Více toků, každý na jinou cílovou IP adresu, port a protokol, sdílet jeden port překládat pomocí SNAT. Cílovou IP adresu, port a protokol zkontrolujte toky jedinečný bez nutnosti dalších zdrojových portů k rozlišení toků v veřejný adresní prostor IP adres.

Když vyčerpání prostředků překládat pomocí SNAT port, odchozí toky nezdaří, dokud existující toky verze porty překládat pomocí SNAT. Nástroj pro vyrovnávání zatížení získá překládat pomocí SNAT porty, když toku zavře a používá [časový limit nečinnosti 4minutové](#idletimeout) pro opětovného získání překládat pomocí SNAT porty z nečinnosti toků.

Vzory pro zmírnění podmínky, které běžně vést k vyčerpání port překládat pomocí SNAT, přečtěte si [Správa překládat pomocí SNAT](#snatexhaust) části.

### <a name="preallocatedports"></a>Předběžné přidělení dočasných portů pro port vydávají překládat pomocí SNAT (Jan)

Azure používá algoritmus určit počet souhrnů ještě neumístěných překládat pomocí SNAT porty, které jsou k dispozici na základě velikosti fondu back-end při použití portu podvržený překládat pomocí SNAT ([PAT](#pat)). Překládat pomocí SNAT porty jsou dočasné porty, které jsou k dispozici pro konkrétní veřejné zdrojové adresy IP.

Azure preallocates překládat pomocí SNAT porty ke konfiguraci IP adresy síťového adaptéru každý virtuální počítač. Při konfiguraci IP adres je přidán do fondu, jsou porty překládat pomocí SNAT souhrnů ještě neumístěných pro tuto konfiguraci IP adresy na základě velikosti fondu back-end. Pro classic webové role pracovního procesu je přidělení za role instance. Když se vytváří odchozí toky, [PAT](#pat) dynamicky využívá (až předběžně přidělené limit) a uvolní tyto porty toku zavře nebo [časové limity nečinnosti](#ideltimeout) dojít.

V následující tabulce jsou uvedeny preallocations port překládat pomocí SNAT pro úrovně velikosti fondu back-end:

| Velikost fondu (instance virtuálních počítačů) | Předběžně přidělené překládat pomocí SNAT porty na konfiguraci protokolu IP|
| --- | --- |
| 1-50 | 1,024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1,000 | 32 |

Mějte na paměti, že počet dostupných portů překládat pomocí SNAT nepřekládá přímo na toků. Jediný port překládat pomocí SNAT lze znovu použít pro více cílů jedinečný. Porty jsou využívat pouze v případě, že je nutné, aby toky jedinečný. Pokyny k návrhu a zmírnění dopadů, naleznete v části o [postup správy tento prostředek vyčerpatelným](#snatexhaust) a oddíl, který popisuje [PAT](#pat).

Změna velikosti fondu back-end může mít vliv na některé vaše zavedených toků. Pokud velikost fondu back-end zvyšuje a přejde do s další vrstvou, polovinu vaší předběžně přidělené překládat pomocí SNAT porty jsou uvolnit při přechodu do další vrstvou větší fond back-end. Toky, které jsou spojeny s regenerovaný portem překládat pomocí SNAT vypršení časového limitu, který se musí znovu navázat. Pokud dojde k pokusu o nové toku, bude toku okamžitě úspěšné, dokud předběžně přidělené porty jsou k dispozici.

Pokud velikost fondu back-end snižuje a přejde do nižší úrovně, zvyšuje počet dostupných portů překládat pomocí SNAT. V takovém případě existující přidělené překládat pomocí SNAT porty a jejich odpovídajících toky neovlivní.

## <a name="snatexhaust"></a>Správa vyčerpání port překládat pomocí SNAT (Jan)

[Dočasné porty](#preallocatedports) používá pro [PAT](#pat) jsou vyčerpatelným prostředků, jak je popsáno v [samostatný virtuální počítač bez Instance úroveň veřejnou IP adresu](#defaultsnat) a [Vyrovnávání zatížení sítě virtuálních počítačů bez Instance úroveň veřejnou IP adresu](#lb).

Pokud víte, že jste zahajování mnoho odchozí připojení TCP nebo UDP na stejnou cílovou IP adresu a port, a doporučujeme pomocí podpory, že jste vyčerpává porty překládat pomocí SNAT nebo sledovat selhání odchozí připojení (souhrnů ještě neumístěných [dočasných porty](#preallocatedports) používá [PAT](#pat)), máte několik možností obecné zmírnění dopadů. Přečtěte si tyto možnosti a rozhodnout, jaký je k dispozici a nejvhodnější pro váš scénář. Je možné, že jeden nebo více může pomoct spravovat tento scénář.

Pokud máte potíže seznámení s chováním odchozí připojení, můžete použít IP zásobník statistiky (netstat). Nebo může být užitečné, abyste mohli pozorovat chování připojení pomocí paketu zachycení. Můžete provádět tyto záznamy o paketů v hostovaný operační systém instanci služby nebo použít [sledovací proces sítě pro zachytávání paketů](../network-watcher/network-watcher-packet-capture-manage-portal.md).

### <a name="connectionreuse"></a>Upravit aplikaci znovu použít připojení 
Můžete snížit vyžádání pro dočasné porty, které se používají pro překládat pomocí SNAT opětovným použitím připojení ve vaší aplikaci. To platí hlavně pro protokoly jako HTTP/1.1, kde je výchozím nastavením, opakované použití připojení. A pak využívat jiné protokoly, které používají protokol HTTP jako způsob přepravy (například REST). 

Opakované použití je vždy lepší, než jednotlivé, atomic připojení TCP pro každou žádost. Znovu použít za následek další původce, velmi efektivní TCP transakce.

### <a name="connection pooling"></a>Upravit aplikaci, aby používala sdružování připojení
Můžete použít připojení sdružování schéma ve vaší aplikaci, kde jsou požadavky interně distribuovány na pevnou sadu připojení (každý opakovaného použití kde je to možné). Toto schéma omezí počet dočasné porty používá a vytvoří předvídatelnější prostředí. Toto schéma, můžete taky zvýšit propustnost žádostí tím, že více souběžných operací, když blokuje jednoho připojení na odpověď operace.  

Sdružování připojení možná již existuje v rámci, který používáte k vývoji vaší aplikace nebo nastavení konfigurace pro vaši aplikaci. Můžete kombinovat s opakované použití připojení sdružování připojení. Více požadavků pak využívat pevné, předvídatelný počet portů na stejnou cílovou IP adresu a port. Požadavky také těžit z efektivní využití TCP transakce snižuje latence a využití prostředků. Transakce UDP mohou také těžit, protože správa UDP toků můžete zase vyhnout výfukového podmínky a spravovat využití portu překládat pomocí SNAT.

### <a name="retry logic"></a>Upravit aplikaci, aby používala méně agresivní logika opakovaných pokusů
Když [souhrnů ještě neumístěných dočasné porty](#preallocatedports) používá pro [PAT](#pat) jsou vyčerpání nebo aplikace dojde k selhání, agresivní nebo hrubou silou opakuje bez decay a omezení rychlosti logiku způsobit vyčerpání dojít, nebo zachovat. Vyžádání pro dočasné porty můžete omezit použitím méně agresivní logika opakovaných pokusů. 

Dočasné porty mít 4 minut nečinnosti, po vypršení časového limitu (není možné nastavit). Pokud jsou moc agresivní opakované pokusy, má k vyčerpání nebyla příležitost vymazány svoje vlastní. Vzhledem k tomu, jak – a jak často – aplikace opakování transakce je proto důležitou součástí návrhu.

### <a name="assignilpip"></a>Přiřazení IP adresy Instance úrovně veřejné pro každý virtuální počítač
Přiřazení splnění změní váš scénář [Instance úroveň veřejnou IP adresu pro virtuální počítač](#ilpip). Všechny dočasné porty veřejné IP adresy, které se používají pro jednotlivé virtuální počítače jsou k dispozici pro virtuální počítač. (Na rozdíl od scénáře kde sdílené dočasné porty veřejnou IP adresu s všechny virtuální počítače přidružené k příslušné back-end fondu.) Existují kompromisy vzít v úvahu, například další náklady na veřejné IP adresy a potenciální dopad vytvoření seznamu povolených velký počet jednotlivé IP adresy.

>[!NOTE] 
>Tato možnost není k dispozici pro webové role pracovního procesu.

### <a name="idletimeout"></a>Použít udržování otevřených připojení k resetování odchozí časový limit nečinnosti

Odchozí připojení mít časový limit nečinnosti 4 minuty. Tento časový limit není upravit. Však můžete přenos (například TCP udržování otevřených připojení) nebo udržování otevřených připojení aplikační vrstvě aktualizujte tok nečinnosti a obnovit tento časový limit nečinnosti v případě potřeby.

## <a name="discoveroutbound"></a>Zjišťování veřejnou IP adresu, která používá virtuální počítač
Existuje mnoho způsobů, jak určit IP adresu zdrojového veřejné odchozí připojení. OpenDNS poskytuje službu, která můžete zobrazit veřejnou IP adresu vašeho virtuálního počítače. 

Pomocí příkazu nslookup můžete odeslat dotaz DNS pro název myip.opendns.com do překladače OpenDNS. Služba vrátí zdrojové IP adresy, který se používá k odeslání dotazu. Když spustíte následující dotaz z virtuálního počítače, je odpověď na veřejné IP adresy použít pro tento virtuální počítač:

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventoutbound"></a>Nelze navázat odchozí připojení
Někdy je žádoucí pro virtuální počítač povolit vytváření odchozího toku. Nebo může být požadavek na správu, které cíle je dostupný odchozí toky nebo které cíle zahájit příchozí toky. V takovém případě můžete použít [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md) ke správě cílů, které mohou být využity virtuálního počítače. Skupiny Nsg můžete použít také ke správě, který veřejné cílové můžete zahájit příchozí toky. 

Když použijete skupinu NSG k virtuálnímu počítači s vyrovnáváním zatížení, věnovat pozornost [výchozí značky](../virtual-network/virtual-networks-nsg.md#default-tags) a [výchozí pravidla](../virtual-network/virtual-networks-nsg.md#default-rules). Je nutné zajistit, že virtuální počítač může přijímat žádosti o kontrolu stavu z nástroje pro vyrovnávání zatížení Azure. 

Pokud skupina NSG blokuje žádostí o stav testu z výchozí značka AZURE_LOADBALANCER, selže test stavu vašeho virtuálního počítače a virtuálního počítače je označena. Nástroj pro vyrovnávání zatížení zastaví odesílání nové toky do tohoto virtuálního počítače.

## <a name="next-steps"></a>Další postup

- Další informace o [základní nástroje pro vyrovnávání zatížení](load-balancer-overview.md).
- Další informace o [skupin zabezpečení sítě](../virtual-network/virtual-networks-nsg.md).
- Přečtěte si o některých dalších klíče [sítě možnosti](../networking/networking-overview.md) v Azure.
