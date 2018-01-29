---
title: "Spravovat výpočetní výkon v Azure SQL Data Warehouse (portál Azure) | Microsoft Docs"
description: "Azure úkoly portálu pro správu výpočetního výkonu. Škálovat výpočetní prostředky úpravou Dwu. Nebo, pozastavení a obnovení výpočetní prostředky, abyste ušetřili náklady."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 233b0da5-4abd-4d1d-9586-4ccc5f50b071
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: f56e62576cae0c594f26bcddf44528032bd5ea69
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/29/2018
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-azure-portal"></a>Spravovat výpočetní výkon v Azure SQL Data Warehouse (portál Azure)
Škálovat výpočetní prostředky v Azure SQL Data Warehouse pomocí portálu Azure.

## <a name="scale-compute-power"></a>Škálování výpočetní výkon
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Chcete-li změnit výpočetní prostředky:

1. Otevřete [portál Azure][Azure portal], otevřete databázi a klikněte na tlačítko **škálování**.

    ![Klikněte na škálování][1]
2. V okně škálování nastavte posuvník doleva nebo doprava, chcete-li změnit nastavení DWU.

    ![Posuvník][2]
3. Klikněte na **Uložit**. Zobrazí potvrzovací zpráva. Klikněte na tlačítko **Ano** potvrďte nebo **žádné** zrušit.

    ![Kliknutí na Uložit][3]


## <a name="next-steps"></a>Další postup
Další informace najdete v tématu [přehled správy][Management overview].

<!--Image references-->
[1]: ./media/sql-data-warehouse-manage-compute-portal/click-scale.png
[2]: ./media/sql-data-warehouse-manage-compute-portal/move-slider.png
[3]: ./media/sql-data-warehouse-manage-compute-portal/click-save.png


<!--Article references-->
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
