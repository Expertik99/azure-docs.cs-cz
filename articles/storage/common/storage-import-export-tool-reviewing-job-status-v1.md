---
title: Kontrola stavu úlohy Azure Import/Export - v1 | Dokumentace Microsoftu
description: Další informace o použití soubory protokolů vytvořené při spuštění úlohy importu nebo exportu a zjistit stav úlohu importu/exportu.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/26/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: c5b9d1993c9e90411c7b05d9874721a159275f22
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/06/2018
ms.locfileid: "44021824"
---
# <a name="reviewing-azure-importexport-job-status-with-copy-log-files"></a>Kontrola stavu úlohy Azure Import/Export s použitím kopií souborů protokolu
Když služba Microsoft Azure Import/Export zpracovává disky přidružené k importu nebo exportu úloze, zapíše kopírování souborů protokolu do účtu úložiště do nebo ze kterého jsou importu nebo exportu objektů BLOB. Soubor protokolu obsahuje podrobný stav o jednotlivých souborech, které se importovaná nebo exportovaná. Když odešlete dotaz na stav dokončené úlohy; vrátí se adresa URL pro každý soubor protokolu kopírování Zobrazit [Get Job](https://docs.microsoft.com/rest/api/storageimportexport/Jobs/Get) Další informace.  

## <a name="example-urls"></a>Příklad adresy URL

Následují příklady adres URL pro kopií souborů protokolu pro úlohu importu se dvěma jednotkami:  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  
  
 Zobrazit [služby Import/Export formát souboru protokolu](../storage-import-export-file-format-log.md) pro formát protokoly kopírování a úplný seznam stavových kódů.  
  
## <a name="next-steps"></a>Další postup
 
 * [Nastavení nástroje Azure Import/Export](storage-import-export-tool-setup-v1.md)   
 * [Příprava pevných disků pro úlohu importu](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 * [Oprava úlohy importu](../storage-import-export-tool-repairing-an-import-job-v1.md)   
 * [Oprava úlohy exportu](../storage-import-export-tool-repairing-an-export-job-v1.md)   
 * [Řešení potíží s nástrojem Azure pro import/export](storage-import-export-tool-troubleshooting-v1.md)
