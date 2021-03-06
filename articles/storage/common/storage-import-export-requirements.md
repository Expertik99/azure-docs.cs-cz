---
title: Požadavky pro službu Azure Import/Export | Dokumentace Microsoftu
description: Vysvětlení softwarové a hardwarové požadavky pro službu Azure Import/Export.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 07/19/2018
ms.author: alkohli
ms.component: common
ms.openlocfilehash: 10e8fb6ac5bcce278de3924ebd3a0d9f90392217
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/06/2018
ms.locfileid: "39528050"
---
# <a name="azure-importexport-system-requirements"></a>Požadavky na systém Azure Import/Export

Tento článek popisuje důležité požadavky pro vaši službu Azure Import/Export. Doporučujeme, abyste si informace o pečlivě před používat službu Import/Export a poté vraťte se k němu podle potřeby během operace.

## <a name="supported-operating-systems"></a>Podporované operační systémy

Pro přípravu pevných disků pomocí nástroje WAImportExport následující **64bitová verze operačního systému, které podporují nástroj BitLocker Drive Encryption** jsou podporovány.


|Platforma |Verze |
|---------|---------|
|Windows     | Windows 7 Enterprise, Windows 7 Ultimate <br> Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise <br> Windows 10        |
|Windows Server     |Windows Server 2008 R2 <br> Windows Server 2012, Windows Server 2012 R2         |


## <a name="supported-storage-accounts"></a>Účty úložiště podporuje

Služba Import/Export Azure podporuje následující [účty služby Azure storage](storage-account-options.md).
- Obecné účely v1 úložiště účtů (nasazení Classic nebo Azure Resource Manager)
- Účty služby Blob Storage
- Účty storage v2 pro obecné účely

Každá úloha slouží k přenosu dat do nebo z pouze jeden účet úložiště. Jinými slovy úlohu importu/exportu jedné nemůžou zahrnovat napříč několika účty úložiště. Informace o vytvoření nového účtu úložiště najdete v tématu [způsob vytvoření účtu úložiště](storage-create-storage-account.md#create-a-storage-account).

> [!IMPORTANT] 
> Služba Azure Import Export nepodporuje účty úložiště ve kterém [koncové body služeb virtuální sítě](../../virtual-network/virtual-network-service-endpoints-overview.md) se povolila funkce. 

## <a name="supported-storage-types"></a>Typy podporovaných úložišť

Následující seznam typů úložiště podporuje služba Azure Import/Export.


|Úloha  |Služba Storage |Podporováno  |Nepodporuje se  |
|---------|---------|---------|---------|
|Import     |  Azure Blob Storage <br><br> Azure File storage       | Objekty BLOB bloku a stránku objekty BLOB podporována <br><br> Podporované soubory          |
|Export     |   Azure Blob Storage       | Objekty BLOB bloku, objekty BLOB stránky a doplňovací objekty BLOB, nepodporuje         | Nepodporuje soubory Azure


## <a name="supported-hardware"></a>Podporovaný hardware 

Pro službu Azure Import/Export potřebujete podporované disky kopírovat data.

### <a name="supported-disks"></a>Podporované disky

Následující seznam disků se podporuje pro použití se službou Import/Export.


|Typ disku  |Velikost  |Podporováno |Nepodporuje se  |
|---------|---------|---------|---------|
|SSD    |   2,5"      |         |         |
|HDD     |  2,5"<br>3,5"       |II SATA, SATA III         |Externí pevný disk s integrovanou adaptéru USB <br> Disk v použití malých a velkých externí pevný disk         |


Úloha importu/exportu jedné může mít:
- Maximálně 10 pevný disk nebo disky SSD.
- Kombinace pevný disk nebo SSD libovolné velikosti.

Velký počet jednotek, lze distribuovat mezi několika úlohami a neexistuje žádné omezení počtu úloh, které je možné vytvořit. Pro úlohy importu se zpracuje pouze první datový svazek na disku. Objem dat musí být naformátovaná za použití systému souborů NTFS.

Když příprava pevných disků a kopírování dat pomocí nástroje WAImportExport, můžete použít externí adaptéry USB. Většina předem připravená USB 3.0 nebo novější adaptéry by měla fungovat. 


## <a name="next-steps"></a>Další postup

* [Nastavení nástroje WAImportExport](storage-import-export-tool-how-to.md)
* [Přenos dat pomocí nástroje příkazového řádku AzCopy](storage-use-azcopy.md)
* [Ukázkový Import exportovat rozhraní REST API služby Azure](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)

