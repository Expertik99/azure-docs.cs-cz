---
title: Užitečné operátorů v dotazech Azure Log Analytics | Dokumentace Microsoftu
description: Běžné funkce pro různé scénáře v dotazy Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: ce397b1ba8d77d2916caa2798c0161ba55f51dbb
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/22/2018
ms.locfileid: "42447499"
---
# <a name="useful-operators-in-log-analytics-queries"></a>Užitečné operátorů v dotazech Log Analytics

Následující tabulka obsahuje některé běžné funkce pro různé scénáře v dotazy Log Analytics.

## <a name="useful-operators"></a>Užitečné operátory

Kategorie                                |Relevantní Analytics – funkce
----------------------------------------|----------------------------------------
Výběr a ve sloupci aliasy            |`project`, `project-away`, `extend`
Dočasné tabulky a konstanty          |`let scalar_alias_name = …;` <br> `let table_alias_name = (){ … &#124; … &#124; … };`
Porovnání a operátory řetězce         |`startswith`, `!startswith`, `has`, `!has` <br> `contains`, `!contains`, `containscs` <br> `hasprefix`, `!hasprefix`, `hassuffix`, `!hassuffix`, `in`, `!in` <br> `matches regex` <br> `==`, `=~`, `!=`, `!~`
Běžné funkce řetězce                 |`strcat()`, `replace()`, `tolower()`, `toupper()`, `substring()`, `strlen()`
Běžné matematické funkce                   |`sqrt()`, `abs()` <br> `exp()`, `exp2()`, `exp10()`, `log()`, `log2()`, `log10()`, `pow()` <br> `gamma()`, `gammaln()`
Analýza textu                            |`extract()`, `extractjson()`, `parse`, `split()`
Omezení výstupu                         |`take`, `limit`, `top`, `sample`
Datové funkce                          |`now()`, `ago()` <br> `datetime()`, `datepart()`, `timespan` <br> `startofday()`, `startofweek()`, `startofmonth()`, `startofyear()` <br> `endofday()`, `endofweek()`, `endofmonth()`, `endofyear()` <br> `dayofweek()`, `dayofmonth()`, `dayofyear()` <br> `getmonth()`, `getyear()`, `weekofyear()`, `monthofyear()`
Seskupení a agregace                |`summarize by` <br> `max()`, `min()`, `count()`, `dcount()`, `avg()`, `sum()` <br> `stddev()`, `countif()`, `dcountif()`, `argmax()`, `argmin()` <br> `percentiles()`, `percentile_array()`
Spojení a sjednocení                        |`join kind=leftouter`, `inner`, `rightouter`, `fullouter`, `leftanti` <br> `union`
Pořadí řazení                             |`sort`, `order` 
Dynamických objektů (JSON a pole)         |`parsejson()` <br> `makeset()`, `makelist()` <br> `split()`, `arraylength()` <br> `zip()`, `pack()`
Logické operátory                       |`and`, `or`, `iff(condition, value_t, value_f)` <br> `binary_and()`, `binary_or()`, `binary_not()`, `binary_xor()`
Strojové učení                        |`evaluate autocluster`, `basket`, `diffpatterns`, `extractcolumns`


## <a name="next-steps"></a>Další postup

- Projděte si lekci [zápis dotazů v Log Analytics](get-started-queries.md).