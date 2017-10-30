---
title: "Poznámky k verzi agenta Azure File Sync | Dokumentace Microsoftu"
description: "Poznámky k verzi Azure File Sync"
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: tamram
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 1db3fef17e24022bff59665558f4a354f8c37d1d
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/20/2017
---
# <a name="azure-file-sync-agent-release-notes"></a>Poznámky k verzi agenta Azure File Sync
Azure File Sync (Preview) umožňuje centralizovat sdílené složky organizace ve službě Soubory Azure bez ztráty flexibility, výkonu a kompatibility místního souborového serveru. Dělá to pomocí transformace serverů Windows na rychlou mezipaměť sdílené složky Azure. Pro místní přístup k datům můžete použít jakýkoli protokol dostupný ve Windows Serveru (včetně SMB, NFS a FTPS) a můžete mít libovolný počet mezipamětí po celém světě.

Tento článek obsahuje poznámky k podporovaným verzím agenta Azure File Sync.

## <a name="supported-versions"></a>Podporované verze
Azure File Sync podporuje následující verze:

| Číslo verze agenta | Datum vydání | Platnost do |
|----------------------|--------------|------------------|
| 1.1.0.0 | 26. 9. 2017 | Aktuální verze |

### <a name="azure-file-sync-agent-update-policy"></a>Zásady aktualizace agenta Azure File Sync
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-1100"></a>Agent verze 1.1.0.0
Následující poznámky k verzi platí pro agenta verze 1.1.0.0 vydaného 9. září 2017. Tato verze označuje počáteční verzi Preview služby Azure File Sync.

### <a name="agent-installation-and-server-configuration"></a>Instalace agenta a konfigurace serveru
Další informace o instalaci a konfiguraci agenta Azure File Sync s použitím Windows Serveru najdete v tématech [Plánování nasazení služby Azure File Sync (Preview)](storage-sync-files-planning.md) a [Nasazení služby Azure File Sync (Preview)](storage-sync-files-deployment-guide.md).

- Instalační balíček agenta (MSI) se musí nainstalovat se zvýšenými oprávněními (správce).
- Nepodporuje se v možnostech nasazení Windows Server Core nebo Nano Server.
- Podporuje se pouze ve Windows Serveru 2016 a 2012 R2.
- Agent vyžaduje alespoň 2 GB fyzické paměti.

### <a name="interoperability"></a>Vzájemná funkční spolupráce
- Antivirové, zálohovací a další aplikace s přístupem k vrstveným souborům můžou způsobit nežádoucí odvolání, pokud nerespektují atribut offline a přeskočí čtení obsahu těchto souborů. Další informace najdete v tématu [Řešení potíží se službou Azure File Sync (Preview)](storage-sync-files-troubleshoot.md).
- Nepoužívejte blokování souborů služby Správce prostředků souborového serveru: blokování souborů můžou způsobit chyby nekonečné synchronizace, pokud jsou kvůli němu soubory blokované.
- Duplikace registrovaného serveru (včetně klonování virtuálního počítače) může vést k neočekávaným výsledkům (konkrétně se synchronizace nemusí nikdy sloučit).
- Odstranění duplicit dat a vrstvení cloudu se nepodporuje na stejném svazku.
 
### <a name="sync-limitations"></a>Omezení synchronizace
Následující položky se nesynchronizují, ale zbytek systému bude fungovat normálně dál:
- Cesty delší než 2 048 znaků
- Část DACL popisovače zabezpečení, pokud je větší než 2 kB (to je problém pouze v případě, že pro jednu položku máte více než přibližně 40 položek řízení přístupu)
- Část SACL popisovače zabezpečení (používá se pro auditování)
- Rozšířené atributy
- Alternativní datové proudy
- Body rozboru
- Pevné odkazy
- Komprese (pokud je nastavená pro soubor na serveru) se nezachová při synchronizaci změn do tohoto souboru z ostatních koncových bodů
- Všechny soubory šifrované pomocí systému souborů EFS (nebo jiného šifrování v uživatelském režimu), který brání naší službě ve čtení těchto dat 
    
    > [!Note]  
    > Azure File Sync vždy šifruje přenášená data a neaktivní uložená data se můžou šifrovat v Azure.
 
### <a name="server-endpoints"></a>Koncové body serveru
- Koncový bod serveru je možné vytvořit pouze na svazku NTFS. Azure File Sync v současné době nepodporuje systémy souborů ReFS, FAT, FAT32 a jiné.
- Koncový bod serveru nemůže být na systémovém svazku (například C:\moje_složka není přijatelná cesta, pokud C:\moje_složka není bod připojení).
- Clustering s podporou převzetí služeb při selhání se podporuje pouze s clusterovanými disky, nikoli se sdílenými svazky clusteru (CSV).
- Koncový bod serveru nemůže být vnořený, ale může existovat paralelně na stejném svazku jako ostatní.
- Odstranění velkého počtu adresářů ze serveru najednou (více než 10 000) může způsobit chyby synchronizace – odstraňujte adresáře v dávkách po méně než 10 000 a před odstraněním další dávky se ujistěte, že se operace odstranění úspěšně synchronizovaly.
- Nepodporuje se v kořenovém adresáři svazku.
- Neukládejte na koncový bod serveru stránkovací soubor operačního systému nebo aplikace.
 
### <a name="cloud-tiering"></a>Vrstvení cloudu
- Za účelem zajištění možnosti správného odvolání souborů systém nemusí provést automatické vrstvení nových nebo změněných souborů po dobu až 32 hodin, což zahrnuje i první vrstvení po konfiguraci nového koncového bodu serveru – nabízíme rutinu PowerShellu pro vrstvení na vyžádání, abyste mohli vrstvení vyhodnotit efektivněji bez čekání na proces na pozadí.
- Pokud se vrstvený soubor zkopíruje pomocí příkazu Robocopy do nového umístění, výsledný soubor nebude vrstvený, ale může mít nastavený atribut offline, protože příkaz Robocopy nesprávně zahrnuje tento atribut do operací kopírování.
- Při zobrazení vlastností souboru v klientovi SMB se může zobrazit nesprávně nastavený atribut offline. Důvodem je ukládání metadat souboru do mezipaměti protokolem SMB.