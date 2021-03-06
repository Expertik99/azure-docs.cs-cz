---
title: 'Synchronizace Azure AD Connect: rozšíření adresáře | Dokumentace Microsoftu'
description: Toto téma popisuje funkce rozšíření adresáře ve službě Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: a19fa124396bf9c9db4a60591b74f5425ce9670b
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2018
ms.locfileid: "42058079"
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Synchronizace Azure AD Connect: rozšíření adresáře
Rozšíření adresáře můžete použít k rozšíření schématu do služby Azure Active Directory (Azure AD) pomocí vlastních atributů z místní služby Active Directory. Tato funkce umožňuje vytváření obchodních aplikací prostřednictvím atributy, které budete nadále spravovat místní. Tyto atributy mohou být spotřebovány prostřednictvím [rozšíření adresáře Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) nebo [Microsoft Graphu](https://graph.microsoft.io/). Dostupné atributy lze zobrazit pomocí [Azure AD Graph Explorer](https://graphexplorer.azurewebsites.net/) a [Microsoft Graph Exploreru](https://developer.microsoft.com/graph/graph-explorer)v uvedeném pořadí.

V současné době žádné úlohy Office 365 využívá tyto atributy.

Můžete nakonfigurovat další atributy, které se mají synchronizovat v cestě vlastní nastavení v Průvodci instalací.

![Průvodce rozšířením schématu](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)  

Instalace zobrazuje následující atributy, které jsou platné kandidáty:

* Typy uživatelů a skupin objektů
* Jednohodnotové atributy: řetězec, logická hodnota, celé číslo, binární soubor
* Více jednohodnotových atributů: řetězec, binární soubor


>[!NOTE]
> Azure AD Connect podporuje synchronizaci více Vážíme si toho atributy služby Active Directory k Azure AD jako rozšíření adresáře více Vážíme si toho. Ale žádné funkce v Azure AD momentálně nepodporují více Vážíme si toho adresář rozšíření.

Seznam atributů, které načítají z mezipaměti schémat, která je vytvořena během instalace služby Azure AD Connect. Pokud jste rozšířili schéma služby Active Directory s další atributy, je nutné [aktualizovat schéma](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) předtím, než tyto nové atributy jsou viditelné.

Objekt ve službě Azure AD může mít až 100 atributů rozšíření adresáře. Maximální délka je 250 znaků. Pokud hodnota atributu je delší, synchronizační modul oříznut.

Během instalace služby Azure AD Connect je registrována aplikace, kde tyto atributy jsou k dispozici. Zobrazí se tato aplikace na webu Azure Portal.

![Schéma rozšíření aplikace](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

Atributy mají předponu rozšíření \_{AppClientId}\_. AppClientId má stejnou hodnotu pro všechny atributy ve vašem tenantovi Azure AD.

Tyto atributy jsou teď k dispozici prostřednictvím Azure AD Graph API. Dotazování těchto pomocí [Azure AD Graph Explorer](https://graphexplorer.azurewebsites.net/).

![Azure AD Graph Exploreru](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Nebo můžete zadat dotaz na atributy prostřednictvím rozhraní Microsoft Graph API s využitím [Microsoft Graph Exploreru](https://developer.microsoft.com/graph/graph-explorer#).

>[!NOTE]
> Budete muset požádat o atributy, které mají být vráceny. Explicitně vybrat atributy takto: https://graph.microsoft.com/beta/users/abbie.spencer@fabrikamonline.com?$select = extension_9d98ed114c4840d298fad781915f27e4_employeeID extension_9d98ed114c4840d298fad781915f27e4_division. 
>
> Další informace najdete v tématu [Microsoft Graph: použijte parametry dotazu](https://developer.microsoft.com/graph/docs/concepts/query_parameters#select-parameter).

## <a name="next-steps"></a>Další postup
Další informace o [synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md) konfigurace.

Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
