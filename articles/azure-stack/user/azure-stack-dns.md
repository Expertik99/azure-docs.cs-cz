---
title: DNS v Azure stacku | Dokumentace Microsoftu
description: Pomocí služby DNS v Azure stacku
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.openlocfilehash: acb8b262256031ae8615180e0f55c98cb56b538d
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "42059408"
---
# <a name="using-dns-in-azure-stack"></a>Pomocí služby DNS v Azure stacku

*Platí pro: Azure Stack integrované systémy a Azure Stack Development Kit*

Azure Stack podporuje následující funkce systému DNS (Domain Name):

* Překlad názvů hostitelů DNS
* Vytváření a správa DNS zón a záznamů pomocí rozhraní API

## <a name="support-for-dns-hostname-resolution"></a>Podpora pro překlad názvů hostitelů DNS

Můžete zadat popisek názvu domény DNS pro prostředky veřejné IP adresy. Azure Stack používá *domainnamelabel.location*. cloudapp.azurestack.external pro název popisku a mapy na veřejné IP adresy ve službě Azure Stack spravované servery DNS.

Například, pokud vytvoříte prostředek s veřejnou IP **contoso** jako popisek názvu domény v umístění místní služby Azure Stack [plně kvalifikovaný název domény](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (plně kvalifikovaný název domény)  **contoso.Local.cloudapp.azurestack.external** přeloží na veřejnou IP adresu prostředku. Tento plně kvalifikovaný název domény můžete použít k vytvoření vlastní domény záznam CNAME, který odkazuje na veřejnou IP adresu ve službě Azure Stack.

Další informace o překladu, najdete v tématu [překlad názvů DNS](https://docs.microsoft.com/azure/dns/dns-for-azure-services?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) článku.

> [!IMPORTANT]
> Každý popisek názvu domény, které vytvoříte, musí být jedinečný v rámci jeho umístění služby Azure Stack.

Na další obrazovce zachycení ukazuje **vytvoření veřejné IP adresy** dialogové okno pro vytvoření veřejné IP adresy na portálu.

![Vytvoření veřejné IP adresy](media/azure-stack-whats-new-dns/image01.png)

**Příklad scénáře**

Je nutné nástroj pro vyrovnávání zatížení zpracování požadavků z webové aplikace. Nástroje pro vyrovnávání za zatížení je webový server běží na jeden nebo více virtuálních počítačů. Můžete přistupovat pomocí názvu DNS, místo IP adresy webové stránky s vyrovnáváním zatížení.

## <a name="create-and-manage-dns-zones-and-records-using-the-api"></a>Vytvoření a správa DNS zón a záznamů pomocí rozhraní API

Můžete vytvořit a spravovat zóny DNS a záznamy ve službě Azure Stack.

Azure Stack poskytuje službě DNS, jako je Azure, s využitím rozhraní API, která jsou konzistentní s rozhraními API pro Azure DNS.  Hostovat v Azure stacku DNS, můžete spravovat svoje záznamy DNS pomocí stejné přihlašovací údaje, rozhraní API a nástroje. Můžete také použít stejné fakturace a podpory jako u ostatních služeb Azure.

Infrastruktura Azure stacku DNS je kompaktnějším než Azure. Velikost a umístění nasazení Azure Stack bude mít vliv na DNS oboru, škálování a výkonu. Zároveň to znamená, výkonu, dostupnosti, globální distribuci a vysokou dostupnost se může lišit od nasazení k nasazení.

## <a name="comparison-with-azure-dns"></a>Porovnání s Azure DNS

Je podobný DNS v Azure DNS ve službě Azure Stack, ale existují hlavními výjimkami, které potřebujete znát.

* **Nepodporuje záznamů AAAA**

    Azure Stack záznamů AAAA nepodporuje, protože Azure Stack nepodporuje adresy IPv6.  Toto je klíčovým rozdílem mezi DNS v Azure a Azure Stack.
* **Není více tenantů**

    Služba DNS v Azure stacku není více tenantů. Každý tenant nelze vytvořit stejnou zónu DNS. Pouze první předplatné, pokusí se vytvořit zónu úspěšné a neúspěšné odeslání dalších žádostí.  Jde o známý problém a klíčovým rozdílem mezi Azure a Azure Stack DNS. Tento problém bude opraven v budoucí verzi.
* **Značky, metadata a značek etag**

    Existují malé rozdíly ve zpracování značky, metadat, značek etag a omezení v Azure Stack.

Další informace o Azure DNS najdete v tématu [záznamů a zón DNS](../../dns/dns-zones-records.md).

### <a name="tags-metadata-and-etags"></a>Značky, metadata a značek etag

**Značky**

Azure Stack DNS podporuje používání značky Azure Resource Manageru na prostředky zóny DNS. Nepodporuje značky na sady záznamů DNS, i když jako alternativu 'metadata' je podporován v sad záznamů DNS, jak je popsáno dále.

**Metadata**

Jako alternativu k značky pro sady záznamů Azure Stack DNS podporuje zadávání poznámek do sad záznamů pomocí 'metadat'. Podobně jako u značky se metadata vám umožní přidružit každá sada záznamů dvojice název hodnota. Například to může být užitečné k zaznamenání účel každá sada záznamů. Na rozdíl od značky metadata nelze použít k poskytování filtrované zobrazení vaší faktuře za Azure a nelze zadat v zásadách Azure Resource Manageru.

**Značek etag**

Předpokládejme, že dva lidé, případně dva procesy zkuste upravit záznam DNS ve stejnou dobu. Která z nich wins? A vítězem ví, že jste se přepsat změny, které vytvořil někdo jiný?

Azure Stack DNS bezpečně zpracování souběžných změn do stejného prostředku pomocí značek ETag. Značek etag jsou oddělené od Azure Resource Manageru 'Značky'. Každý prostředek DNS (zóny nebo sadu záznamů) má značku Etag s ním spojená. Pokaždé, když se načte prostředek, se také načte Etag. Při aktualizaci prostředku, můžete předat zpět Etag, Azure Stack DNS můžete ověřit, že značky Etag na server odpovídá. Protože každá aktualizace prostředku výsledkem Etag, znovu se generuje, neshoda značek Etag označuje, že došlo ke změně souběžných. Značek etag můžete také použít při vytváření nového prostředku k zajištění, že prostředek ještě neexistuje.

Ve výchozím nastavení používá Azure Stack DNS Powershellu značek entit k blokování souběžných změn zón a sad záznamů. Volitelný *-přepsat* přepínače lze potlačit kontroly Etag, v takovém případě všechny souběžných se přepíše změny, ke kterým došlo.

Na úrovni DNS REST API služby Azure Stack značek etag určeny pomocí hlavičky protokolu HTTP. Jejich chování je uveden v následující tabulce:

| Záhlaví | Chování|
|--------|---------|
| Žádný   | PUT vždy úspěšné (žádné kontroly Etag)|
| If-match| PUT úspěšná, pouze pokud prostředek existuje a Značka Etag odpovídá|
| If-match *| PUT úspěšná, pouze pokud existuje prostředek|
| If-none-match *| PUT úspěšná, pouze pokud prostředek neexistuje.|

### <a name="limits"></a>Omezení

Následující výchozí omezení platí při použití Azure Stack DNS:

| Prostředek| Výchozí omezení|
|---------|--------------|
| Zón v předplatném| 100|
| Sady záznamů za zónu| 5000|
| Záznamy na sadu záznamů| 20|

## <a name="next-steps"></a>Další postup

[Představení iDNS pro Azure Stack](azure-stack-understanding-dns.md)
