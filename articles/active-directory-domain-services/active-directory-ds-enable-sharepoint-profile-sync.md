---
title: 'Azure Active Directory Domain Services: Povolení podpory pro službu SharePoint uživatelský profil | Dokumentace Microsoftu'
description: Konfigurace spravovaných domén Azure Active Directory Domain Services pro podporu synchronizace profil pro SharePoint Server
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: maheshu
ms.openlocfilehash: c25fca2f3645a0397a999cec7552de15f20fb6be
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502071"
---
# <a name="configure-a-managed-domain-to-support-profile-synchronization-for-sharepoint-server"></a>Nakonfigurujte spravované domény pro podporu synchronizace profil pro SharePoint Server
Server služby SharePoint obsahuje služba profilu uživatele, který je používán k synchronizaci profilu uživatele. Nastavení služby profilů uživatelů, příslušná oprávnění muset být udělen v doméně služby Active Directory. Další informace najdete v tématu [Active Directory Domain Services oprávnění k synchronizaci profilu v SharePoint serveru 2013](https://technet.microsoft.com/library/hh296982.aspx).

Tento článek vysvětluje, jak nakonfigurovat spravované domény služby Azure AD Domain Services pro nasazení služby synchronizace profilu uživatele serveru SharePoint.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="the-aad-dc-service-accounts-group"></a>Skupina účty AAD DC služby
Skupina zabezpečení s názvem "**účty služby AAD DC**" je k dispozici v rámci 'Uživatele' organizační jednotky ve vaší spravované doméně. Zobrazí se v této skupině **Active Directory Users and Computers** modul snap-in konzoly MMC ve vaší spravované doméně.

![Skupiny zabezpečení účtů služby AAD DC](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

Členové této skupiny zabezpečení jsou delegovanými oprávněními následující:
- Replikace změn adresáře oprávnění na kořenovým položkám DSE spravované domény.
- Replikace změn adresáře oprávnění v názvovém kontextu konfigurace (cn = configuration kontejneru) spravované domény.

Tato skupina zabezpečení je také členem předdefinované skupiny **Pre-Windows 2000 Compatible Access**.

![Skupiny zabezpečení účtů služby AAD DC](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-to-support-sharepoint-server-user-profile-sync"></a>Povolit spravované domény pro podporu synchronizace profilu uživatele serveru SharePoint
Můžete přidat účet služby použitý k synchronizaci profilu uživatele Sharepointu k **účty služby AAD DC** skupiny. V důsledku toho synchronizační účet získá odpovídající oprávnění k replikaci změn do adresáře. Tento krok konfigurace umožňuje serveru SharePoint Server uživatelského profilu synchronizace fungovat správně.

![Účty služby AAD DC – přidání členů](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![Účty služby AAD DC – přidání členů](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a>Související obsah
* [Technické Reference - udělení Active Directory Domain Services oprávnění k synchronizaci profilu v SharePoint serveru 2013](https://technet.microsoft.com/library/hh296982.aspx)
