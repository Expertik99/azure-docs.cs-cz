---
title: Import a Export dat ve službě Azure Redis Cache | Dokumentace Microsoftu
description: Zjistěte, jak importovat a exportovat data do a z úložiště objektů blob s vaší instancí Azure Redis Cache premium
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 4a68ac38-87af-4075-adab-569d37d7cc9e
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: wesmc
ms.openlocfilehash: 6733891213f15e9ceaf08ef7fb50380db47a695f
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/26/2018
ms.locfileid: "39259190"
---
# <a name="import-and-export-data-in-azure-redis-cache"></a>Import a Export dat ve službě Azure Redis Cache
Import/Export je operace správy Azure Redis Cache dat, který umožňuje importovat data do Azure Redis Cache a export dat z Azure Redis Cache pomocí importu a exportu snímku databáze Redis Cache (RDB) z mezipaměti premium do objektu blob v Azure Účet úložiště. 

- **Export** – vaše Azure Redis Cache RDB snímky můžete exportovat do objektů Blob stránky.
- **Importovat** – vaše snímky Redis RDB do mezipaměti můžete importovat z objektů Blob stránky nebo objekt Blob bloku.

Import/Export umožňuje migraci mezi různými instancemi Azure Redis Cache nebo naplňte mezipaměť daty před použitím.

Tento článek poskytuje návod pro import a export dat pomocí Azure Redis Cache a poskytuje odpovědi na nejčastější dotazy.

> [!IMPORTANT]
> Import/Export je ve verzi preview a je dostupná jenom pro [úroveň premium](cache-premium-tier-intro.md) ukládá do mezipaměti.
>
>

## <a name="import"></a>Import
Import je možné uvést kompatibilní soubory RDB Redis z jakéhokoli serveru Redis spuštěná v jakémkoli cloudu nebo prostředí, včetně Redis běžící na Linuxu, Windows nebo libovolného poskytovatele cloudových služeb, jako je například Amazon Web Services a další. Import dat je snadný způsob, jak vytvořit mezipaměť s předem naplněných daty. Azure Redis Cache během procesu importu načte soubory RDB ze služby Azure storage do paměti a poté vloží klíčů do mezipaměti.

> [!NOTE]
> Před zahájením operace importu se ujistěte, že Redis databáze (RDB) souboru nebo souborům se nahrají do stránka nebo blok objektů BLOB v Azure storage, ve stejné oblasti a předplatném Azure jako vaše instance Azure Redis Cache. Další informace najdete v tématu [Začínáme s úložištěm objektů Blob v Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Pokud jste exportovali pomocí souboru RDB [Azure Redis Cache Export](#export) funkce souboru RDB je již uložen v objektu blob stránky a je připravený k importu.
>
>

1. K importu minimálně jeden objekt BLOB exportované mezipaměti, [přejděte na příslušnou mezipaměť](cache-configure.md#configure-redis-cache-settings) na webu Azure portal a klikněte na **importovat data** z **nabídce prostředků**.

    ![Import dat][cache-import-data]
2. Klikněte na tlačítko **zvolit objekty BLOB** a vyberte účet úložiště, který obsahuje data pro import.

    ![Výběr účtu úložiště][cache-import-choose-storage-account]
3. Klikněte na kontejner, který obsahuje data pro import.

    ![Vyberte kontejner][cache-import-choose-container]
4. Vyberte minimálně jeden objekt BLOB pro import kliknutím do oblasti vlevo od názvu objektu blob a pak klikněte na tlačítko **vyberte**.

    ![Vyberte objekty BLOB][cache-import-choose-blobs]
5. Klikněte na tlačítko **importovat** zahajte proces importu.

   > [!IMPORTANT]
   > Mezipaměti není přístupný pro klienty mezipaměti během procesu importu a odstranit všechna existující data v mezipaměti.
   >
   >

    ![Import][cache-import-blobs]

    Pomocí následujících oznámení z portálu Azure portal nebo pomocí zobrazení událostí v můžete sledovat průběh operace importu [protokolu auditu](../azure-resource-manager/resource-group-audit.md).

    ![Postup importu][cache-import-data-import-complete]

## <a name="export"></a>Export
Export umožňuje exportovat data uložená ve službě Azure Redis Cache k Redis kompatibilní soubory RDB. Tato funkce slouží k přesunu dat z jedné instance Azure Redis Cache do jiné nebo na jiný server Redis. Během procesu exportu dočasný soubor vytvořen na virtuálním počítači, který je hostitelem instance serveru služby Azure Redis Cache a nahrání souboru do účtu úložiště určený. Po dokončení operace exportu se stavem úspěch nebo neúspěch dočasný soubor bude odstraněn.

1. Exportovat aktuální obsah do mezipaměti do úložiště, [přejděte na příslušnou mezipaměť](cache-configure.md#configure-redis-cache-settings) na webu Azure portal a klikněte na **exportovat data** z **nabídce prostředků**.

    ![Zvolit kontejner úložiště][cache-export-data-choose-storage-container]
2. Klikněte na tlačítko **vyberte kontejner úložiště** a vyberte požadovaný účet úložiště. Účet úložiště musí být ve stejném předplatném a oblasti jako mezipaměť.

   > [!IMPORTANT]
   > Export funguje s objekty BLOB stránky, které jsou podporovány v modelu classic a účty úložiště Resource Manageru, ale nejsou podporovány [účty Blob storage](../storage/common/storage-account-options.md#blob-storage-accounts) v tuto chvíli.
   >
   >

    ![Účet úložiště][cache-export-data-choose-account]
3. Zvolte požadovanou objektů blob v kontejneru a klikněte na tlačítko **vyberte**. Chcete-li použít nový kontejner, klikněte na tlačítko **přidat kontejner** nejdříve přidejte a potom ho vyberte ze seznamu.

    ![Zvolit kontejner úložiště][cache-export-data-container]
4. Zadejte **předponu názvu objektu Blob** a klikněte na tlačítko **exportovat** zahájíte proces exportu. Předpona názvu objektu blob je používat jako předpona názvy souborů generovaných touto operací exportu.

    ![Export][cache-export-data]

    Pomocí následujících oznámení z portálu Azure portal nebo pomocí zobrazení událostí v můžete sledovat průběh operace exportu [protokolu auditu](../azure-resource-manager/resource-group-audit.md).

    ![Export dat dokončeno][cache-export-data-export-complete]

    Mezipaměti zůstávají dostupné k použití během procesu exportu.

## <a name="importexport-faq"></a>Nejčastější dotazy týkající se importu/exportu
Tento oddíl obsahuje nejčastější dotazy týkající se funkce importu a exportu.

* [Jaké cenové úrovně můžete použít Import/Export?](#what-pricing-tiers-can-use-importexport)
* [Můžete importovat data z jakéhokoli serveru Redis](#can-i-import-data-from-any-redis-server)
* [Jaké verze RDB můžete importovat?](#what-rdb-versions-can-i-import)
* [Je k dispozici mezipaměť během operace importu/exportu?](#is-my-cache-available-during-an-importexport-operation)
* [Můžete použít Import/Export s clusterem Redis](#can-i-use-importexport-with-redis-cluster)
* [Jak funguje Import/Export s vlastní databáze nastavení?](#how-does-importexport-work-with-a-custom-databases-setting)
* [Čím se liší od trvalosti Redis Import/Export](#how-is-importexport-different-from-redis-persistence)
* [Můžete automatizovat pomocí Powershellu, CLI nebo jiných klientů pro správu importu/exportu?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
* [Vypršení časového limitu zobrazila se mi během Moje operace importu/exportu. Co znamená?](#i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean)
* [Zobrazila se chyba při exportu dat do úložiště objektů Blob v Azure. Co se přihodilo?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened)

### <a name="what-pricing-tiers-can-use-importexport"></a>Jaké cenové úrovně můžete použít Import/Export?
Import/Export je k dispozici pouze v cenové úrovně premium.

### <a name="can-i-import-data-from-any-redis-server"></a>Můžete importovat data z jakéhokoli serveru Redis
Ano, kromě importovat data exportovaná z instance služby Azure Redis Cache, můžete importovat soubory RDB z jakéhokoli serveru Redis spuštěná v jakémkoli cloudu nebo prostředí, například Linux, Windows, nebo poskytovatelů, jako je například Amazon Web Services v cloudu. K tomuto účelu nahrání souboru RDB z požadovaného serveru Redis do stránku nebo objekt blob bloku v účtu služby Azure Storage a potom ho importujte do vaší instance Azure Redis Cache premium. Například můžete exportovat data z mezipaměti produkčního prostředí a importujte ho do mezipaměti používá jako součást testovací prostředí pro testování nebo pro migraci.

> [!IMPORTANT]
> Chcete-li úspěšně importovat data exportovaná z servery Redis kromě Azure Redis Cache, při použití objektů blob stránky, velikost objektu blob stránky musí být zarovnány na hranici 512 bajtů. Vzorový kód k provedení všech požadovaných bajtů odsazení, naleznete v tématu [odeslání objektu blob stránky ukázka](https://github.com/JimRoberts-MS/SamplePageBlobUpload).
> 
> 

### <a name="what-rdb-versions-can-i-import"></a>Jaké verze RDB můžete importovat?

Azure Redis Cache podporuje import RDB do až prostřednictvím RDB verze 7.

### <a name="is-my-cache-available-during-an-importexport-operation"></a>Je k dispozici mezipaměť během operace importu/exportu?
* **Export** – mezipamětí nadále k dispozici a můžete nadále používat mezipaměť během operace exportu.
* **Importovat** – mezipaměti k dispozici, když se spustí operace importu a budou k dispozici pro použití po dokončení operace importu.

### <a name="can-i-use-importexport-with-redis-cluster"></a>Můžete použít Import/Export s clusterem Redis
Ano, a je můžete import/export mezi Clusterové mezipaměti a neclusterovaných mezipaměť. Od Redis cluster [jen podporuje databáze 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), není-li importovat všechna data v databázích než 0. Když se importují data v clusteru mezipaměti, jsou klíče rozloží mezi horizontální oddíly clusteru.

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Jak funguje Import/Export s vlastní databáze nastavení?
Některé cenové úrovně mají odlišné [databáze omezení](cache-configure.md#databases), takže existují některé aspekty při importu, pokud jste nakonfigurovali vlastní hodnotu `databases` nastavení během vytváření mezipaměti.

* Při importu na cenovou úroveň s nižší `databases` limit než na úrovni, ze kterého jste exportovali:
  * Pokud používáte výchozí počet `databases`, což je 16 pro všechny cenové úrovně, se neztratila žádná data.
  * Pokud použijete vlastní číslo `databases` této spadá do omezení pro vrstvu, na který importujete, se neztratila žádná data.
  * Exportovaná data obsažená data v databázi, která překračuje omezení na nové úrovni, není k importu dat z těchto vyšší databází.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Čím se liší od trvalosti Redis Import/Export
Azure trvalost Redis Cache vám umožňuje zachovat data uložená v Redis do služby Azure Storage. Pokud je nakonfigurovaná trvalost, Azure Redis Cache uchovává snímek mezipaměti Redis v binárním formátu Redis na disk na základě konfigurovatelných četnosti zálohování. Případě katastrofické události, která zakáže primárním i replikovaném mezipaměti, data v mezipaměti obnoví automaticky pomocí nejnovější snímek. Další informace najdete v tématu [konfigurace trvalosti dat pro mezipaměti redis Cache Premium Azure](cache-how-to-premium-persistence.md).

Import / Export umožňuje přenést data do nebo ji exportujte ze služby Azure Redis Cache. Není konfigurace zálohování a obnovení pomocí trvalosti Redis.

### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Můžete automatizovat pomocí Powershellu, CLI nebo jiných klientů pro správu importu/exportu?
Ano, prostředí PowerShell pokyny naleznete v tématu [import mezipaměti redis cache](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) a [Export mezipaměti redis cache](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).

### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Vypršení časového limitu zobrazila se mi během Moje operace importu/exportu. Co znamená?
Pokud zůstanou na **importovat data** nebo **exportovat data** okno déle než 15 minut před inicializací operace, obdržíte chybu s chybovou zprávou podobně jako v následujícím příkladu:

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Chcete-li tento problém vyřešit, spusťte import nebo export fungování předtím, než 15 minut uplynul.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Zobrazila se chyba při exportu dat do úložiště objektů Blob v Azure. Co se stalo?
Export funguje jenom s RDB soubory uložené jako objekty BLOB stránky. Jiné typy objektů blob se aktuálně nepodporují, včetně účty blob storage s vrstvami hot a cool. Další informace najdete v tématu [účty Blob storage](../storage/common/storage-account-options.md#blob-storage-accounts).

## <a name="next-steps"></a>Další postup
Další informace o použití další prémiových funkcí mezipaměti.

* [Úvod do služby Azure Redis Cache na úrovni Premium](cache-premium-tier-intro.md)    

<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png
