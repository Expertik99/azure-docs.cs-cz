---
title: Údržba zásad služby Azure Stack | Dokumentace Microsoftu
description: Další informace o službě Azure Stack údržby, zásad a tom, jak zajistit integrovaný systém v podporovaném stavu.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: caac3d2f-11cc-4ff2-82d6-52b58fee4c39
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: brenduns
ms.reviewer: harik
ms.openlocfilehash: f74a4ad0507f1c1f029befff88d733ffa719a763
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "44023507"
---
# <a name="azure-stack-servicing-policy"></a>Údržba zásad služby Azure Stack
Tento článek popisuje údržby zásady pro integrované systémy Azure Stack, a co musíte udělat, aby byl váš systém v podporovaném stavu. 

## <a name="update-package-types"></a>Typy aktualizace balíčků

Existují dva typy balíčků aktualizací pro integrované systémy: 

- **Aktualizace softwaru Microsoft**. Společnost Microsoft zodpovídá za začátku do konce životní cyklus údržby pro balíčky aktualizací softwaru společnosti Microsoft. Tyto balíčky můžou zahrnovat nejnovější aktualizace zabezpečení Windows serveru, které se netýkají zabezpečení aktualizace a aktualizace funkcí služby Azure Stack. Balíčky těchto aktualizací můžete stáhnout přímo od Microsoftu.

- **Výrobce OEM aktualizací poskytnutých dodavatelem hardwaru**. Partneři hardware Azure Stack je zodpovědná za začátku do konce údržby životní cyklus (včetně pokynů) pro související s hardwarem firmware a aktualizace balíčků ovladačů. Kromě toho partneři hardware Azure Stack vlastní a Udržovat pokyny pro veškerý software a hardware na hostitelský hardware životního cyklu. Výrobce OEM dodavatele hardwaru hostitelem tyto balíčky na vlastní web pro stahování aktualizací.


## <a name="update-package-release-cadence"></a>Aktualizace balíčku vydávání verzí
Microsoft se očekává, že verze balíčků aktualizací softwaru v měsíčním tempo. Je však možné mít více nebo žádná verze aktualizace v daném měsíci. Výrobce OEM výrobci hardwaru vydávat jejich aktualizace na základě podle potřeby. 

Vyhledejte si dokumentaci na tom, jak naplánovat a spravovat aktualizace a jak určit vaší aktuální verzí v [spravovat aktualizace přehled](azure-stack-updates.md). Informace o konkrétní aktualizaci včetně si ho stáhnout, naleznete v tématu poznámky k verzi pro, které aktualizace: 
- [Aktualizace služby Azure Stack. 1808](azure-stack-update-1808.md)
- [Aktualizace služby Azure Stack 1807](azure-stack-update-1807.md)
- [Aktualizace služby Azure Stack 1805](azure-stack-update-1805.md)


## <a name="hotfixes"></a>Opravy hotfix
V některých případech společnost Microsoft poskytuje opravy hotfix pro Azure Stack, které řeší konkrétní problém, který je často preventivní nebo časovým počitadlem.  Každý opravy hotfix jsou vydány s odpovídající článek znalostní báze Microsoft s podrobnostmi o problému, příčině a řešení. 

Opravy hotfix se stahují a instalují stejně jako regulární úplnou aktualizaci balíčků pro službu Azure Stack. Ale na rozdíl od úplné aktualizace, opravy hotfix můžete nainstalovat během několika minut. Doporučujeme, abyste že operátorům Azure stacku nastavení časového období údržby při instalaci oprav hotfix. Opravy hotfix aktualizujte verzi cloudu služby Azure Stack, můžete snadno zjistit, pokud byl použit opravu hotfix. Samostatné opravy hotfix se poskytuje pro každou verzi služby Azure Stack, která se stále podpory. Jednotlivé opravy pro konkrétní iteraci je kumulativní a obsahuje předchozí aktualizace pro stejnou verzi. Další informace o použitelnosti konkrétního oprava hotfix v opravy odpovídající znalostní báze Knowledge Base article.  


## <a name="keep-your-system-under-support"></a>Zachovat systému v rámci podpory
Pokračujte k získání podpory, je nutné zachovat vašeho nasazení Azure stacku aktuální. Zásady odložení aktualizace je: pro nasazení Azure Stack tak si zachováte podporu, musí spustit nedávno vydané verze aktualizace nebo spusťte jeden z dvě předchozí verze aktualizace. Opravy hotfix, se nepovažují aktualizace hlavní verze. Pokud cloudu služby Azure Stack je za *více než dvě aktualizace*, je to považovat za nedodržení a musíte aktualizovat alespoň minimální podporovaná verze získání podpory. 

Například pokud nejvíce dostupného aktualizovanou verzi je 1805 a předchozí dva balíčky aktualizace byly verze 1804 a 1803, 1803 a 1804 zůstanou na podporu. Je však 1802 bez podpory. Zásady platí, pokud neexistuje žádná verze pro měsíc nebo dvě. Například pokud v aktuální verzi je 1805 a nebyly žádné verzi 1804, předchozí dva balíčky aktualizací 1803 a 1802 zůstanou na podporu.

Balíčky aktualizací softwaru společnosti Microsoft jsou mimo kumulativní a vyžadují předchozí aktualizace jako předpoklad. Pokud se rozhodnete odložení jeden nebo více aktualizací, zvažte celkové modulu runtime, pokud chcete získat nejnovější verzi. 

## <a name="get-support"></a>Získat podporu
Azure Stack se řídí podporu stejně jako Azure. Podnikoví zákazníci můžou postupujte podle procesu popsaného v [postupy vytvoření žádosti o podporu Azure](/azure/azure-supportability/how-to-create-azure-support-request). Pokud jste zákazník z Cloud Service Provider (CSP), požádejte o podporu poskytovatel cloudových služeb.  Další informace najdete v tématu [nejčastější dotazy k podpoře Azure](https://azure.microsoft.com/support/faq/). 


## <a name="next-steps"></a>Další postup

- [Správa aktualizací ve službě Azure Stack](azure-stack-updates.md)


