---
title: Příklad PowerShellu – Zkopírování databáze SQL Azure na nový server | Microsoft Docs
description: Ukázkový skript Azure PowerShellu pro zkopírování databáze SQL na nový server
services: sql-database
documentationcenter: sql-database
author: CarlRabeler
manager: craigg
editor: carlrab
tags: azure-service-management
ms.assetid: ''
ms.service: sql-database
ms.custom: load & move data, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 446bf6e3f73fc703dcb02a2dad530b791a5a51d1
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/20/2018
ms.locfileid: "34366133"
---
# <a name="use-powershell-to-copy-a-sql-database-to-a-new-server"></a>Zkopírování databáze SQL na nový server pomocí PowerShellu

Tento ukázkový skript PowerShellu vytvoří kopii stávající databáze na novém serveru. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="copy-a-database-to-a-new-server"></a>Zkopírování databáze na nový server

[!code-powershell[main](../../../powershell_scripts/sql-database/copy-database-to-new-server/copy-database-to-new-server.ps1?highlight=18-21 "Copy database to new server")]

## <a name="clean-up-deployment"></a>Vyčištění nasazení

Po spuštění ukázkového skriptu můžete pomocí následujícího příkazu odebrat skupinu prostředků a všechny k ní přidružené prostředky.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $sourceresourcegroupname
Remove-AzureRmResourceGroup -ResourceGroupName $targetresourcegroupname
```

## <a name="script-explanation"></a>Vysvětlení skriptu

Tento skript používá následující příkazy. Každý příkaz v tabulce odkazuje na příslušnou část dokumentace.

| Příkaz | Poznámky |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Vytvoří skupinu prostředků, ve které se ukládají všechny prostředky. |
| [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Vytvoří logický server, který je hostitelem databáze nebo elastického fondu. |
| [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Vytvoří databázi na logickém serveru jako izolovanou databázi nebo databázi ve fondu. |
| [New-AzureRmSqlDatabaseCopy](/powershell/module/azurerm.sql/new-azurermsqldatabasecopy) | Vytvoří kopii databáze s použitím snímku v aktuálním čase. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Odstraní skupinu prostředků včetně všech vnořených prostředků. |
|||

## <a name="next-steps"></a>Další kroky

Další informace o Azure PowerShellu najdete v [dokumentaci k Azure PowerShellu](/powershell/azure/overview).

Další ukázkové skripty PowerShellu pro službu SQL Database najdete v tématu [Skripty PowerShellu pro službu Azure SQL Database](../sql-database-powershell-samples.md).
