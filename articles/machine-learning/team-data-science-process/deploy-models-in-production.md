---
title: Nasazení modely v produkčním prostředí - Azure Machine Learning | Microsoft Docs
description: Postup nasazení modelů do produkčního prostředí tím jim umožníte hraje aktivní roli při vytvoření obchodních rozhodnutí.
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2017
ms.author: deguhath
ms.openlocfilehash: 0505d58261ec32015f8847b710791249f87f049b
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/07/2018
ms.locfileid: "34838288"
---
# <a name="deploy-models-in-production"></a>Nasazení modelů v produkčním prostředí

Produkční nasazení umožňuje modelu pro hraje aktivní roli v podniku. Predikce z nasazené modelu lze použít pro obchodních rozhodnutí.

## <a name="production-platforms"></a>Produkční platformy
Existují různé přístupy a platformy pro modely uvedení do produkčního prostředí. Tady je několik možností:


- [Model nasazení v nástroji Azure Machine Learning](../desktop-workbench/model-management-overview.md)
- [Nasazení modelu v systému SQL server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)


>[!NOTE]
>Před nasazením jeden má zajistit, že je latence modelu vyhodnocování nízké používat v produkčním prostředí.
>


>[!NOTE]
>Nasazení pomocí Azure Machine Learning Studio, najdete v tématu [nasazení webové služby Azure Machine Learning](../studio/publish-a-machine-learning-web-service.md).
>

## <a name="ab-testing"></a>A / B testování
Když více modelů v produkčním prostředí, může být užitečné k provedení [A / B testování](https://en.wikipedia.org/wiki/A/B_testing) k porovnání výkonu modelů. 

 
## <a name="next-steps"></a>Další postup

Návody, které ukazují všechny kroky v procesu pro **konkrétních scénářů** jsou také uvedeny. Jsou uvedena v seznamu a propojené s miniatur popisy v [příklad návody](walkthroughs.md) článku. Se ukazují, jak kombinovat cloud, místní nástroje a služby do pracovního postupu nebo kanálu vytvoření inteligentního aplikace. 
 


