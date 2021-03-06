---
title: Externí sdílení Office 365 a Azure Active Directory s B2B spolupráce | Dokumentace Microsoftu
description: Tento článek popisuje prostředky pro sdílení obsahu s externími partnery pomocí spolupráce O365 a Azure Active Directory s B2B.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/24/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 7e0a65e4a5807cb9a6b39feecfd2d5b2643ea4a9
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/27/2018
ms.locfileid: "39330838"
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a>Externí sdílení Office 365 a spolupráce Azure Active Directory s B2B

Externí sdílení Office 365 (OneDrive, SharePoint Online, sjednocené skupiny atd.) a spolupráce B2B ve službě Azure Active Directory (Azure AD) je technicky stejnou věc. Všechny externí sdílení (kromě Onedrivu nebo Sharepointu Online), včetně hostů ve skupinách Office 365, již použije Pozvánka spolupráce B2B ve službě Azure AD rozhraní API pro sdílení.

## <a name="how-does-azure-ad-b2b-differ-from-external-sharing-in-sharepoint-online"></a>Jak Azure AD B2B se liší od externí sdílení v Sharepointu Online?

Onedrivu nebo Sharepointu Online má správce samostatné pozvánku. Podpora pro externí sdílení v Onedrivu nebo Sharepointu Online spuštěné před jeho podporu vyvinuté Azure AD. V průběhu času Onedrivu nebo Sharepointu Online externí sdílení rozlišeny několik funkcí a mnoha milionů uživatelů, kteří používají produktu je integrované sdílení vzor. Existují však některé drobné rozdíly mezi Onedrivu nebo Sharepointu Online externí sdílení jak funguje a jak spolupráce B2B ve službě Azure AD funguje. Další informace o Onedrivu nebo Sharepointu Online externímu sdílení ve [externí sdílení přehled](https://docs.microsoft.com/sharepoint/external-sharing-overview). Proces obecně se liší od Azure AD B2B následujícími způsoby:

- Onedrivu nebo Sharepointu Online přidá uživatele do adresáře po uživatelé mají uplatnit své pozvánky. Ano před uplatnění, se nezobrazí uživatele na portálu Azure AD. Pokud jiný web vyzývá uživatel do té doby, vygeneruje se nová pozvánka. Ale když použijete spolupráce B2B ve službě Azure AD, uživatelé jsou přidáni okamžitě na pozvánku tak, aby zobrazí všude.

- Možnosti uplatnění v Onedrivu nebo Sharepointu Online vypadá jinak než prostředí v Azure AD B2B collaboration. Jakmile uživatel uplatňuje pozvánku, prostředí vypadají podobně.

- Azure AD s B2B spolupráce pozváni uživatelé mohou být zachyceny z Onedrivu nebo Sharepointu Online, dialogová okna pro sdílení obsahu. Onedrivu nebo Sharepointu Online pozváni uživatelé se také zobrazí ve službě Azure AD po jejich uplatnit své pozvánky.

- Licenční požadavky se liší. Pro každý placený licence Azure AD, můžete nechat až pro 5 uživatelů typu Host přístup vašich placených funkcí Azure AD. Další informace o licencování najdete v tématu [licencování Azure AD B2B](https://docs.microsoft.com/azure/active-directory/b2b/licensing-guidance) a ["Jaký je externího uživatele?" v Sharepointu Online externí sdílení přehled](https://docs.microsoft.com/sharepoint/external-sharing-overview#what-is-an-external-user).

Správa externích sdílení v Onedrivu nebo Sharepointu Online se spoluprací Azure AD B2B, nastavte Onedrivu nebo Sharepointu Online externí sdílení nastavení **povolit sdílení pouze s externími uživateli, které již existují ve vaší organizaci adresář**. Uživatelům můžete přejít do externě sdílené weby a můžete si vybrat z externí spolupracovníci, které Správce přidal. Správce můžete přidat externí spolupracovníci prostřednictvím pozvánku spolupráce B2B rozhraní API.


![Onedrivu nebo Sharepointu Online externí sdílení nastavení](media/o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a>Další postup

* [Co je spolupráce B2B ve službě Azure AD?](what-is-b2b.md)
* [Přidání uživatele spolupráce B2B do role](add-guest-to-role.md)
* [Delegování pozvánek spolupráce B2B](delegate-invitations.md)
* [Dynamické skupiny a spolupráci B2B](use-dynamic-groups.md)