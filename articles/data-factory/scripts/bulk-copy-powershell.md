---
title: "Skript prostředí PowerShell: kopírování dat hromadně pomocí Azure Data Factory | Microsoft Docs"
description: "Tento skript prostředí PowerShell ukazuje, jak používat Azure Data Factory ke zkopírování dat ze zdrojového úložiště dat do cílového úložiště dat hromadně."
services: data-factory
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: jingwang
ms.openlocfilehash: f8a66f3daab9b57723f67a60fa195e53333f3b8d
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="powershell-script---copy-multiple-tables-in-bulk-by-using-azure-data-factory"></a>Prostředí PowerShell script – kopírovat více tabulek hromadné pomocí Azure Data Factory

Tento ukázkový skript prostředí PowerShell kopíruje data z více tabulek v databázi Azure SQL do služby Azure SQL data warehouse.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

V tématu [kurz: hromadné kopírování](../tutorial-bulk-copy.md#prerequisites) pro požadavky na spuštění této ukázce.

## <a name="sample-script"></a>Ukázkový skript

> [!IMPORTANT]
> Tento skript vytvoří soubory JSON, které definují entit služby Data Factory (propojené služby, datové sady a kanál) na vašem pevném disku ve složce c:\.

[!code-powershell[main](../../../powershell_scripts/data-factory/bulk-copy-from-sql-databse-to-sql-data-warehouse/bulk-copy-from-sql-database-to-sql-data-warehouse.ps1 "Bulk copy from Azure SQL Database => Azure SQL Data Warehouse")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění ukázkový skript můžete odebrat skupinu prostředků a všechny prostředky, které jsou s ním spojená následující příkaz:

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourceGroupName
```
Chcete-li odebrat objekt pro vytváření dat ze skupiny prostředků, spusťte následující příkaz: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy: 

| Příkaz | Poznámky |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Vytvoří skupinu prostředků, ve které se ukládají všechny prostředky. |
| [Set-AzureRmDataFactoryV2](/powershell/module/azurerm.datafactoryv2/set-azurermdatafactoryv2) | Vytvoření datové továrny |
| [Set-AzureRmDataFactoryV2LinkedService](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2linkedservice) | V datové továrně vytvoří propojené služby. Propojená služba odkazuje na objekt pro vytváření dat v úložišti dat nebo výpočetní. |
| [Set-AzureRmDataFactoryV2Dataset](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactoryv2dataset) | Vytvoří datovou sadu v datové továrně. Datové sady představuje vstupu a výstupu pro aktivitu v kanálu. | 
| [Set-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Set-azurermdatafactorv2ypipeline) | Vytvoří v objektu pro vytváření dat kanál. Kanál obsahuje jeden nebo více aktivit, které provádí určité operaci. U tohoto kanálu aktivity kopírování kopíruje data z jednoho umístění do jiného umístění v Azure Blob Storage. |
| [Invoke-AzureRmDataFactoryV2Pipeline](/powershell/module/azurerm.datafactoryv2/Invoke-azurermdatafactoryv2pipelinerun) | Vytvoří spustit pro kanál. Jinými slovy spouští kanálu. |
| [Get-AzureRmDataFactoryV2ActivityRun](/powershell/module/azurerm.datafactoryv2/get-azurermdatafactoryv2activityrun) | Získá informace o spouštění aktivity (aktivity při spuštění) v kanálu. 
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Odstraní skupinu prostředků včetně všech vnořených prostředků. |
|||

## <a name="next-steps"></a>Další postup

Další informace o Azure PowerShellu najdete v [dokumentaci k Azure PowerShellu](https://docs.microsoft.com/powershell/).

Další ukázky skriptu prostředí PowerShell objekt pro vytváření dat Azure lze nalézt v [skriptů prostředí PowerShell objekt pro vytváření dat Azure](../samples-powershell.md).