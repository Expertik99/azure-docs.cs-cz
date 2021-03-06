---
title: Vizuální vytváření obsahu v Azure Data Factory | Dokumentace Microsoftu
description: Další informace o použití vizuálního vytváření ve službě Azure Data Factory
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/14/2018
ms.author: shlo
ms.openlocfilehash: b457d1ae01e523ac99c6171fa8d2123023ebcd2c
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/15/2018
ms.locfileid: "42060081"
---
# <a name="visual-authoring-in-azure-data-factory"></a>Vizuální vytváření obsahu v Azure Data Factory
Azure Data Factory uživatelské rozhraní rozhraní (UX) umožňuje vizuálně vytvoříte a nasadíte prostředky pro službu data factory bez nutnosti psát jakýkoli kód. Můžete přetáhnout aktivity na plátno kanálu, provádějte testovací běhy, využívejte iterativní ladění a nasadit a monitorovat spuštění kanálů. Existují dvě metody pro provádění vizuálního vytváření pomocí uživatelského rozhraní:

- Autor přímo ve službě Data Factory.
- Autor integrace Visual Studio Team Services (VSTS) Git pro spolupráci, správy zdrojového kódu nebo Správa verzí.

## <a name="author-directly-with-the-data-factory-service"></a>Autor přímo ve službě Data Factory
Vizuální vytváření obsahu pomocí služby Data Factory se liší od vizuálním vytváření s VSTS dvěma způsoby:

- Služba Data Factory neobsahuje úložiště pro ukládání entity JSON pro vaše změny.
- Služba Data Factory není optimalizovaná pro spolupráci a správu verzí.

![Konfigurace služby Data Factory ](media/author-visually/configure-data-factory.png)

Při použití uživatelského rozhraní **plátno pro vytváření obsahu** vytvořit přímo ve službě Data Factory, pouze **Publikovat vše** režim je k dispozici. Všechny změny, které provedete, se publikují přímo do služby Data Factory.

![Režim publikování](media/author-visually/data-factory-publish.png)

## <a name="author-with-vsts-git-integration"></a>Vytváření s využitím integrace VSTS Git
Pro práci na vaše kanály data factory vizuálním vytváření s využitím integrace VSTS Git podporuje správu zdrojového kódu a spolupráci. Datové továrny můžete přidružit účet úložiště VSTS Git pro správy zdrojového kódu, spolupráci, správu verzí a tak dále. Jeden účet úložiště VSTS Git můžete mít více úložišť, ale může být přidružený pouze jeden datový objekt pro vytváření úložiště VSTS Git. Pokud nemáte účet VSTS nebo úložišti, postupujte podle [tyto pokyny](https://docs.microsoft.com/vsts/accounts/create-account-msa-or-work-student) k vytvoření vašich prostředků.

> [!NOTE]
> Skript a datových souborů můžete uložit v úložišti Git služby VSTS. Ale budete muset ručně nahrání souborů do služby Azure Storage. Kanál služby Data Factory není automaticky odeslat soubory skriptu nebo data uložená v úložišti Git služby VSTS do služby Azure Storage.

### <a name="configure-a-vsts-git-repository-with-azure-data-factory"></a>Konfigurace úložiště VSTS Git s Azure Data Factory
Úložiště VSTS Git můžete nakonfigurovat pomocí služby data factory pomocí dvou metod.

#### <a name="method1"></a> Konfigurace metody 1 (úložiště VSTS Git): Stránka Začínáme

Ve službě Azure Data Factory, přejděte **pusťme se do práce** stránky. Vyberte **konfigurace úložiště kódu**:

![Konfigurace úložiště kódu VSTS](media/author-visually/configure-repo.png)

**Nastavení úložiště** otevře se podokno konfigurace:

![Konfigurovat nastavení úložiště kódu](media/author-visually/repo-settings.png)

V podokně se zobrazí nastavení úložiště VSTS následovně:

| Nastavení | Popis | Hodnota |
|:--- |:--- |:--- |
| **Typ úložiště** | Typ úložištěm kódu VSTS.<br/>**Poznámka:**: GitHub v tuto chvíli nepodporuje. | Visual Studio Team Services Git |
| **Azure Active Directory** | Název tenanta Azure AD. | <your tenant name> |
| **Účet služby Visual Studio Team Services** | Název účtu VSTS. Můžete najít název účtu VSTS na `https://{account name}.visualstudio.com`. Je možné [přihlásit ke svému účtu VSTS](https://www.visualstudio.com/team-services/git/) pro přístup k profilu Visual Studio a zobrazit projekty a úložiště. | <your account name> |
| **ProjectName** | Název projektu VSTS. Můžete najít na název projektu VSTS `https://{account name}.visualstudio.com/{project name}`. | <your VSTS project name> |
| **RepositoryName** | Název úložiště kódu VSTS. Úložiště Git pro správu zdrojového kódu, jak se projekt rozrůstá obsahují projekty VSTS. Můžete vytvořit nové úložiště nebo použít existující úložiště, který je již ve vašem projektu. | <your VSTS code repository name> |
| **Spolupráce větve** | Větvi VSTS spolupráci, které slouží k publikování. Ve výchozím nastavení je to `master`. Toto nastavení změňte, v případě, že chcete publikovat prostředky z jiné větve. | <your collaboration branch name> |
| **Kořenová složka** | Kořenové složky ve vaší větvi spolupráci VSTS. | <your root folder name> |
| **Importovat do úložiště stávající prostředky Data Factory** | Určuje, jestli se má naimportovat stávající prostředky data factory z uživatelského rozhraní **plátno pro vytváření obsahu** do úložiště VSTS Git. Vyberte pole pro import prostředky data factory do přidružené úložiště Git ve formátu JSON. Tato akce exportuje každého prostředku zvlášť (to znamená, propojené služby a datové sady se exportují do samostatných JSONs). Když toto políčko není zaškrtnuto, nenaimportují se existující prostředky. | Vybrané (výchozí) |

#### <a name="configuration-method-2--vsts-git-repo-ux-authoring-canvas"></a>Metoda konfigurace 2 (úložiště VSTS Git): UX plátno pro vytváření obsahu
V uživatelském prostředí Azure Data Factory **plátno pro vytváření obsahu**, vyhledejte svou datovou továrnu. Vyberte **služby Data Factory** rozevírací nabídky a pak vyberte **konfigurace úložiště kódu**.

Otevře se podokno konfigurace. Podrobnosti o nastavení konfigurace najdete v popisech v <a href="#method1">metody konfigurace 1</a>.

![Konfigurovat nastavení úložiště kódu pro vytváření uživatelského rozhraní](media/author-visually/configure-repo-2.png)

## <a name="use-a-different-azure-active-directory-tenant"></a>Použít na jiného tenanta Azure Active Directory

Úložiště VSTS Git můžete vytvořit v jiném tenantovi Azure Active Directory. Pokud chcete zadat jinou tenanta Azure AD, musíte mít oprávnění správce pro předplatné Azure, které používáte.

## <a name="switch-to-a-different-git-repo"></a>Přepnout na jiné úložiště Git

Přepnout na jiné úložiště Git, vyhledejte ikonu v pravém horním rohu na stránce Přehled služby Data Factory, jak je znázorněno na následujícím snímku obrazovky. Pokud nevidíte ikonu, vymažte mezipaměť místní prohlížeče. Vyberte ikonu a odebere přidružení aktuální úložiště.

Po odebrání přidružení k aktuální úložiště, můžete nakonfigurovat nastavení Git použít jiné úložiště. Pak můžete importovat stávající prostředky Data Factory do nového úložiště.

![Odebere přidružení aktuálního úložiště Git](media/author-visually/remove-repo.png)

## <a name="use-version-control"></a>Správa verzí
Systémy správy verzí (označované také jako _správy zdrojového kódu_) umožňují vývojářům spolupráce na kódu a sledování změn provedených na kód základní. Správy zdrojového kódu je to důležitý nástroj pro vývojáře více projekty.

Každé úložiště VSTS Git, který je spojen s data factory má větev spolupráci. (`master` je výchozím nastavení spolupráci větev). Uživatelé mohou také vytvářet větve funkcí kliknutím **+ novou větev** a vývoj v větve funkcí.

![Změnit kód synchronizace nebo publikování](media/author-visually/sync-publish.png)

Jakmile budete připraveni s vývojem pro funkce ve vaší větvi funkce, můžete kliknout na **vytvořit žádost o přijetí změn**. Tato akce se provede k VSTS Git, kde může vyvolat žádosti o přijetí změn, revizemi kódu a sloučit změny do větve spolupráci. (`master` je výchozí nastavení). Jsou povoleny pouze pro publikování do služby Data Factory ze své větve spolupráci. 

![Vytvořte novou žádost o přijetí změn](media/author-visually/create-pull-request.png)

## <a name="publish-code-changes"></a>Publikování změn kódu
Poté, co mají sloučit změny do větve spolupráce (`master` je výchozí nastavení), vyberte **publikovat** ručního publikování změn kódu v hlavní větvi do služby Data Factory.

![Publikujte změny do služby Data Factory](media/author-visually/publish-changes.png)

> [!IMPORTANT]
> Hlavní větev se nemusí shodovat s co je nasazená ve službě Data Factory. V hlavní větvi *musí* ručně publikovat do služby Data Factory.

## <a name="author-with-github-integration"></a>Autor integrace Githubu

Pro práci na vaše kanály data factory vizuálním vytváření s integraci Githubu podporuje správu zdrojového kódu a spolupráci. Datové továrny můžete přidružit účet úložiště GitHub pro správy zdrojového kódu, spolupráci, správu verzí. Jeden účet GitHub může mít více úložišť, ale úložiště GitHub může být přidružený pouze jeden datové továrny. Pokud nemáte účet GitHub nebo úložiště, postupujte podle [tyto pokyny](https://github.com/join) k vytvoření vašich prostředků. Integrace GitHub pomocí služby Data Factory podporuje jak veřejné GitHub tak Githubu Enterprise.

Konfigurace úložiště GitHub, musíte mít oprávnění správce pro předplatné Azure, které používáte.

Pro zavedení devět po minutách a ukázku této funkce z následujícího videa:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Azure-Data-Factory-visual-tools-now-integrated-with-GitHub/player]

### <a name="limitations"></a>Omezení

- Skript a datových souborů můžete uložit v úložišti GitHub. Ale budete muset ručně nahrání souborů do služby Azure Storage. Kanál služby Data Factory není automaticky odeslat soubory skriptu nebo data uložená v úložišti GitHub do služby Azure Storage.

- GitHub Enterprise s verzí starší než 2.14.0 nefunguje v prohlížeči Microsoft Edge.

- Integrace Githubu s nástroji visual pro vytváření dat faktor funguje jenom v obecně dostupné verzi Data Factory.

### <a name="configure-a-public-github-repository-with-azure-data-factory"></a>Konfigurace veřejného úložiště GitHub s Azure Data Factory

Úložiště GitHub pomocí služby data factory můžete nakonfigurovat pomocí dvou metod.

**Konfigurace metody 1 (veřejné úložiště): Stránka Začínáme**

Ve službě Azure Data Factory, přejděte **pusťme se do práce** stránky. Vyberte **konfigurace úložiště kódu**:

![Stránka Začínáme objekt pro vytváření dat](media/author-visually/github-integration-image1.png)

**Nastavení úložiště** otevře se podokno konfigurace:

![Nastavení úložiště GitHub](media/author-visually/github-integration-image2.png)

V podokně se zobrazí nastavení úložiště VSTS následovně:

| **Nastavení**                                              | **Popis**                                                                                                                                                                                                                                                                                                                                                                                                                   | **Hodnota**          |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| **Typ úložiště**                                      | Typ úložištěm kódu VSTS.                                                                                                                                                                                                                                                                                                                                                                                             | GitHubu             |
| **Účet GitHub**                                       | Název účtu GitHub. Tento název najdete z https://github.com/{account název} / {název úložiště}. Přejdete na tuto stránku vyzve k zadání přihlašovacích údajů Githubu OAuth ke svému účtu GitHub.                                                                                                                                                                                                                                               |                    |
| **RepositoryName**                                       | Název úložiště GitHub kódu. Účtů GitHub obsahovat úložiště Git pro správu zdrojového kódu. Můžete vytvořit nové úložiště nebo použít existující úložiště, který je již ve vašem účtu.                                                                                                                                                                                                                              |                    |
| **Spolupráce větve**                                 | Větvi Githubu spolupráci, které slouží k publikování. Ve výchozím nastavení je hlavní. Toto nastavení změňte, v případě, že chcete publikovat prostředky z jiné větve.                                                                                                                                                                                                                                                               |                    |
| **Kořenová složka**                                          | Kořenové složky ve vaší větvi spolupráci Githubu.                                                                                                                                                                                                                                                                                                                                                                             |                    |
| **Importovat do úložiště stávající prostředky Data Factory** | Určuje, jestli se má naimportovat stávající prostředky data factory z uživatelského rozhraní **plátno pro vytváření obsahu** do úložiště GitHub. Vyberte pole pro import prostředky data factory do přidružené úložiště Git ve formátu JSON. Tato akce exportuje každého prostředku zvlášť (to znamená, propojené služby a datové sady se exportují do samostatných JSONs). Když toto políčko není zaškrtnuto, nenaimportují se existující prostředky. | Vybrané (výchozí) |
| **Větev se má importovat prostředky do**                       | Určuje, do které větve se importují prostředky data factory (kanály, datové sady, propojených služeb atd.). Prostředky můžete importovat do jednoho z následujících větví:. Spolupráce b. Vytvořte nový c. Použít existující                                                                                                                                                                                                     |                    |

#### <a name="configuration-method-2-public-repo-ux-authoring-canvas"></a>Metoda konfigurace 2 (veřejné úložiště): UX plátno pro vytváření obsahu

V uživatelském prostředí Azure Data Factory **plátno pro vytváření obsahu**, vyhledejte svou datovou továrnu. Vyberte **služby Data Factory** rozevírací nabídky a pak vyberte **konfigurace úložiště kódu**.

Otevře se podokno konfigurace. Podrobnosti o nastavení konfigurace najdete v popisech v *metody konfigurace 1* výše.

### <a name="configure-a-github-enterprise-repository-with-azure-data-factory"></a>Nakonfigurujte úložiště Githubu Enterprise s Azure Data Factory

Úložiště GitHub Enterprise můžete nakonfigurovat pomocí služby data factory pomocí dvou metod.

 #### <a name="configuration-method-1-enterprise-repo-lets-get-started-page"></a>Konfigurace metody 1 (Enterprise úložiště): Stránka Začínáme

Ve službě Azure Data Factory, přejděte **pusťme se do práce** stránky. Vyberte **konfigurace úložiště kódu**:

![Stránka Začínáme objekt pro vytváření dat](media/author-visually/github-integration-image1.png)

**Nastavení úložiště** otevře se podokno konfigurace:

![Nastavení úložiště GitHub](media/author-visually/github-integration-image3.png)

V podokně se zobrazí nastavení úložiště VSTS následovně:

| **Nastavení**                                              | **Popis**                                                                                                                                                                                                                                                                                                                                                                                                                   | **Hodnota**          |
|----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------|
| **Typ úložiště**                                      | Typ úložištěm kódu VSTS.                                                                                                                                                                                                                                                                                                                                                                                             | GitHubu             |
| **Používání Githubu Enterprise**                                | Pokud chcete vybrat Githubu Enterprise                                                                                                                                                                                                                                                                                                                                                                                              |                    |
| **Adresa URL Githubu Enterprise**                                | Adresa URL kořenového Githubu Enterprise. Příklad: https://github.mydomain.com                                                                                                                                                                                                                                                                                                                                                          |                    |
| **Účet GitHub**                                       | Název účtu GitHub. Tento název najdete z https://github.com/{account název} / {název úložiště}. Přejdete na tuto stránku vyzve k zadání přihlašovacích údajů Githubu OAuth ke svému účtu GitHub.                                                                                                                                                                                                                                               |                    |
| **RepositoryName**                                       | Název úložiště GitHub kódu. Účtů GitHub obsahovat úložiště Git pro správu zdrojového kódu. Můžete vytvořit nové úložiště nebo použít existující úložiště, který je již ve vašem účtu.                                                                                                                                                                                                                              |                    |
| **Spolupráce větve**                                 | Větvi Githubu spolupráci, které slouží k publikování. Ve výchozím nastavení je hlavní. Toto nastavení změňte, v případě, že chcete publikovat prostředky z jiné větve.                                                                                                                                                                                                                                                               |                    |
| **Kořenová složka**                                          | Kořenové složky ve vaší větvi spolupráci Githubu.                                                                                                                                                                                                                                                                                                                                                                             |                    |
| **Importovat do úložiště stávající prostředky Data Factory** | Určuje, jestli se má naimportovat stávající prostředky data factory z uživatelského rozhraní **plátno pro vytváření obsahu** do úložiště GitHub. Vyberte pole pro import prostředky data factory do přidružené úložiště Git ve formátu JSON. Tato akce exportuje každého prostředku zvlášť (to znamená, propojené služby a datové sady se exportují do samostatných JSONs). Když toto políčko není zaškrtnuto, nenaimportují se existující prostředky. | Vybrané (výchozí) |
| **Větev se má importovat prostředky do**                       | Určuje, do které větve se importují prostředky data factory (kanály, datové sady, propojených služeb atd.). Prostředky můžete importovat do jednoho z následujících větví:. Spolupráce b. Vytvořte nový c. Použít existující                                                                                                                                                                                                     |                    |

#### <a name="configuration-method-2-enterprise-repo-ux-authoring-canvas"></a>Metoda konfigurace 2 (Enterprise úložiště): UX plátno pro vytváření obsahu

V uživatelském prostředí Azure Data Factory **plátno pro vytváření obsahu**, vyhledejte svou datovou továrnu. Vyberte **služby Data Factory** rozevírací nabídky a pak vyberte **konfigurace úložiště kódu**.

Otevře se podokno konfigurace. Podrobnosti o nastavení konfigurace najdete v popisech v *metody konfigurace 1* výše.

## <a name="use-the-expression-language"></a>Použijte jazyk výrazů
Výrazy pro hodnoty vlastnosti můžete určit pomocí výrazů jazyka, který je podporovaný službou Azure Data Factory.

Zadejte výrazy pro hodnoty vlastnosti tak, že vyberete **Přidání dynamického obsahu**:

![Použijte jazyk výrazů](media/author-visually/dynamic-content-1.png)

## <a name="use-functions-and-parameters"></a>Pomocí služby functions a parametry

Můžete použít funkce nebo zadejte parametry pro kanálů a datových sad v datové továrně **Tvůrce výrazů**:

Informace o podporovaných výrazů naleznete v tématu [výrazům a funkcím ve službě Azure Data Factory](control-flow-expression-language-functions.md).

![Přidat dynamický obsah](media/author-visually/dynamic-content-2.png)

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
Vyberte **zpětnou vazbu** komentovat funkcí nebo oznámení o problémech s nástrojem Microsoft:

![Váš názor](media/author-visually/provide-feedback.png)

## <a name="next-steps"></a>Další postup
Další informace o monitorování a Správa kanálů najdete v tématu [monitorování a Správa kanálů prostřednictvím kódu programu](monitor-programmatically.md).
