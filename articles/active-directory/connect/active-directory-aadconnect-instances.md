---
title: "Azure AD Connect: Instance služby synchronizace | Microsoft Docs"
description: "Tato stránka dokumenty zvláštní upozornění pro instancí služby Azure AD."
services: active-directory
documentationcenter: 
author: andkjell
manager: mtillman
editor: 
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: billmath
ms.openlocfilehash: 396a0ff94f380e194d4b1d45c3d8132a4265a131
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Zvláštní upozornění pro instance
Azure AD Connect se nejčastěji používá s celosvětové instance služby Azure AD a Office 365. Ale existují také další instance a ty mají různé požadavky na adresy URL a další důležité.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Germany
[Microsoft Cloud Německo](http://www.microsoft.de/cloud-deutschland) je svrchovaných Cloudová provozují zplnomocněnec němčině data.

| Adresy URL otevřete v proxy serveru |
| --- |
| \*. microsoftonline.de |
| \*.windows.net |
| + Seznamy odvolaných certifikátů |

Když se přihlásíte ke klientovi Azure AD, musíte použít účet v doméně onmicrosoft.de.

Funkce aktuálně nejsou k dispozici v Německu cloudu společnosti Microsoft:

* **Azure AD Connect Health** není k dispozici.
* **Funkce Automatické aktualizace** není k dispozici.
* **Zpětný zápis hesla** je k dispozici pro verzi preview verzí Azure AD Connect 1.1.570.0 a po.
* Nejsou k dispozici další služby Azure AD Premium.

## <a name="microsoft-azure-government-cloud"></a>Cloudu Microsoft Azure Government.
[Cloudu Microsoft Azure Government](https://azure.microsoft.com/features/gov/) je cloud pro US government.

Tento cloud byl podporován ve starších verzích nástroje DirSync. Ze sestavení 1.1.180 služby Azure AD Connect je podporována nové generace cloudu. Generování používá jen USA založené na koncových bodů a mít jiný seznam adres URL, otevřete v proxy serveru.

| Adresy URL otevřete v proxy serveru |
| --- |
| \*.microsoftonline.com |
| \*. microsoftonline.us |
| \*. windows.net (vyžaduje se pro automatické zjišťování klienta Azure AD government) |
| \*. gov.us.microsoftonline.com |
| + Seznamy odvolaných certifikátů |

> [!NOTE]
> Od verze AAD Connect verze 1.1.647.0 nastavení AzureInstance hodnoty v registru se už nevyžaduje za předpokladu, že *. windows.net je otevřený na vaše servery proxy.

Aktuálně nejsou k dispozici v cloudu Microsoft Azure Government funkce:

* **Azure AD Connect Health** není k dispozici.
* **Funkce Automatické aktualizace** není k dispozici.
* **Zpětný zápis hesla** je k dispozici pro verzi preview verzí Azure AD Connect 1.1.570.0 a po.
* Nejsou k dispozici další služby Azure AD Premium.

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
