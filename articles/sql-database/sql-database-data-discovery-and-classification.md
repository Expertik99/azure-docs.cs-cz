---
title: Azure SQL Database Data zjišťování a klasifikace | Dokumentace Microsoftu
description: Azure SQL Database Data zjišťování a klasifikace
services: sql-database
author: giladmit
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: giladm
ms.openlocfilehash: 6ef9a701f3a228e4c40da94f83310ef2884a3f59
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/17/2018
ms.locfileid: "42061300"
---
# <a name="azure-sql-database-data-discovery-and-classification"></a>Azure SQL Database Data zjišťování a klasifikace
Data zjišťování a klasifikace (aktuálně ve verzi preview) nabízí rozšířené možnosti, které jsou integrované do Azure SQL Database pro **zjišťování**, **klasifikace**, **označování**  &  **ochrana** citlivá data ve vašich databázích.
Zjišťování a klasifikace vaše nejcitlivější data (business, finanční, zdravotní péče, identifikovatelné osobní údaje, atd.) můžete přehrát velmi důležitou roli v zásadní roli. ochrany vaší organizace. Může sloužit jako infrastruktura pro:
* Pomáhá splnit požadavky na dodržování legislativních předpisů a data standardy ochrany osobních údajů.
* Různé scénáře zabezpečení, jako je například monitorování (auditování) a upozorňuje na neobvyklé přístup k citlivým datům.
* Řízení přístupu k a posílení zabezpečení databáze, které obsahují hodně citlivá data.

Zjišťování a klasifikace dat je součástí [rozšířené ochrany před internetovými útoky SQL](sql-advanced-threat-protection.md) nabídky (ATP), která je jednotný balíček pro pokročilé funkce zabezpečení SQL. Zjišťování a klasifikace dat je možné získat přístup a spravovat prostřednictvím portálu pro centrální ochrany ATP v programu SQL.

> [!NOTE]
> Tento dokument se týká pouze ke službě Azure SQL Database. SQL Server (v místním prostředí), najdete v části [SQL Data zjišťování a klasifikace](https://go.microsoft.com/fwlink/?linkid=866999).

## <a id="subheading-1"></a>Co je Data zjišťování a klasifikace?
Zjišťování a klasifikace dat představuje sadu pokročilých služeb a nové funkce SQL vytváříme nové paradigma SQL Information Protection zaměřených na ochranu dat, ne jenom databáze:
* **Zjišťování a doporučení k** – modul klasifikace skenuje databáze a identifikuje sloupce obsahující potenciálně citlivá data. To pak umožňuje snadný způsob, jak zkontrolovat a použít příslušnou klasifikací doporučení prostřednictvím webu Azure portal.
* **Označování** – popisky klasifikace citlivosti můžete trvale označené na sloupce pomocí nové atributy metadat na klasifikaci zavedeny do modulu SQL. Tato metadata můžete využít potom pro pokročilé na základě citlivosti auditování a ochranu scénáře.
* **Výsledek dotazu nastavit citlivost** – citlivosti sady výsledků dotazu se počítá v reálném čase pro účely auditování.
* **Viditelnost** – stavu databáze klasifikace lze zobrazit v podrobné řídicí panel portálu. Kromě toho si můžete stáhnout sestavu (ve formátu aplikace Excel) má být použit pro dodržování předpisů a auditování, jakož i další požadavky.

## <a id="subheading-2"></a>Zjistit, klasifikovat & Popisek citlivé sloupce
Následující část popisuje kroky pro zjišťování, klasifikace a označování popisky sloupců obsahujících citlivá data v databázi, jakož i zobrazení aktuální stav zařazení vaší databáze a export sestav.

Klasifikace obsahuje dva atributy metadat:
* Popisky – atributy hlavní klasifikace se používá k definování úroveň citlivosti dat uložených ve sloupci.  
* Typy informací – poskytují další členitosti do typ dat uložených ve sloupci.

## <a name="classify-your-sql-database"></a>Klasifikace vaše databáze SQL

1. Přejděte na [Azure Portal](https://portal.azure.com).

2. Přejděte do **Advanced Threat Protection** v záhlaví zabezpečení v podokně pro Azure SQL Database. Klikněte na tlačítko se povolit rozšířenou ochranu před internetovými útoky a potom klikněte na **Data zjišťování a klasifikace (preview)** karty.

   ![Kontrola databáze](./media/sql-data-discovery-and-classification/data_classification.png)

3. **Přehled** karta obsahuje shrnutí aktuální stav databáze, včetně podrobný seznam všech klasifikované sloupce, které můžete také filtrovat, chcete-li zobrazit pouze určité schéma částí typy informací a popisky klasifikace. Pokud ještě nebyly žádné sloupce jsou klasifikovány [přejděte ke kroku 5](#step-5).

   ![Souhrnné informace o aktuálním stavu klasifikace](./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png)

4. Pokud chcete stáhnout sestavu ve formátu aplikace Excel, klikněte na **exportovat** možnosti v horní nabídce okna.

   ![Exportovat do Excelu](./media/sql-data-discovery-and-classification/3_data_classification_export_report.png)

5.  <a id="step-5"></a>Pokud chcete začít, klasifikace dat, klikněte na **klasifikace kartu** v horní části okna.

    ![Klasifikace dat](./media/sql-data-discovery-and-classification/4_data_classification_classification_tab_click.png)

6. Klasifikační modul prohledává vaše databáze pro sloupce obsahující potenciálně citlivých data a poskytuje seznam **doporučená klasifikace sloupců**. Zobrazení a použití doporučení klasifikace:

    * Chcete-li zobrazit seznam klasifikací sloupci, klikněte na panel doporučení v dolní části okna:

      ![Klasifikace dat](./media/sql-data-discovery-and-classification/5_data_classification_recommendations_panel.png)

    * Projděte si seznam doporučení – přijímala doporučení pro konkrétní sloupce, zaškrtněte políčko v levém sloupci relevantní řádek. Můžete také označit *všechna doporučení* jako přijatý zaškrtnutím políčka v záhlaví tabulky doporučení.

       ![Zkontrolujte seznam doporučení](./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png)

    * Jak používat vybrané doporučení, klikněte na modrý **přijmout vybraná doporučení** tlačítko.

      ![Použití doporučení](./media/sql-data-discovery-and-classification/7_data_classification_accept_selected_recommendations.png)

7. Můžete také **klasifikovat ručně** sloupců jako alternativu nebo navíc na základě doporučení klasifikace:

    * Klikněte na **přidat klasifikaci** v horní nabídce okna.

      ![Ručně přidat klasifikaci](./media/sql-data-discovery-and-classification/8_data_classification_add_classification_button.png)

    * V okně kontextu, které se otevře, vyberte schéma > tabulky > sloupec, který chcete klasifikovat a informace o typu a citlivosti popisek. Potom klikněte na modrý **přidat klasifikaci** tlačítko v dolní části okna kontextu.

      ![Vyberte sloupec pro klasifikaci](./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png)

8. K dokončení klasifikace a trvale sloupce (značka) databáze s novými metadaty klasifikaci, klikněte na **Uložit** v horní nabídce okna.

   ![Uložení](./media/sql-data-discovery-and-classification/10_data_classification_save.png)

## <a id="subheading-3"></a>Auditování přístupu k citlivým datům

Důležitou součástí ochrany paradigma informace je schopnost sledovat přístup k citlivým datům. [Auditování Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) má vylepšené a zahrnují nové pole do protokolu auditu volá *data_sensitivity_information*, které protokoly klasifikace citlivosti (popisky) skutečných dat, který byl vrácen dotaz.

![Protokol auditu](./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png)

## <a id="subheading-4"></a>Automatizované/programátorské klasifikace

T-SQL můžete použít k přidání nebo odebrání klasifikace sloupců, jakož i načíst všechny klasifikace pro celou databázi.

> [!NOTE]
> Při použití jazyka T-SQL ke správě popisků, neexistuje žádné ověření, která popisky přidané k sloupec existuje v zásadě ochrany informací v organizaci (sadu popisků, které se zobrazují v portálu doporučení). Je pro vás, abyste to ověřit.

* Přidat nebo aktualizovat klasifikaci jeden nebo více sloupců: [přidat klasifikace utajení](https://docs.microsoft.com/en-us/sql/t-sql/statements/add-sensitivity-classification-transact-sql)
* Odebrání klasifikace z jednoho nebo více sloupců: [VYŘADIT klasifikace utajení](https://docs.microsoft.com/en-us/sql/t-sql/statements/drop-sensitivity-classification-transact-sql)
* Zobrazit všechny klasifikace pro databázi: [sys.sensitivity_classifications](https://docs.microsoft.com/en-us/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql)

Můžete také použít rozhraní REST API můžete programově spravovat klasifikace. Publikovaná rozhraní REST API podporují tyto operace:
* [Vytvoření nebo aktualizace](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/createorupdate) – vytvoří nebo aktualizuje označení citlivosti daného sloupce
* [Odstranit](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/delete) -odstraní označení citlivosti daného sloupce
* [Získat](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/get) -získá označení citlivosti daného sloupce
* [Seznam podle databáze](https://docs.microsoft.com/rest/api/sql/sensitivitylabels/listbydatabase) -získá označení citlivosti na danou databázi


## <a id="subheading-5"></a>Další kroky

- Další informace o [SQL Advanced Threat Protection](sql-advanced-threat-protection.md).
- Zvažte možnost nakonfigurovat [auditování služby Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) pro sledování a auditování přístupu k vaší klasifikované citlivá data.

<!--Anchors-->
[SQL Data Discovery & Classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Auditing access to sensitive data]: #subheading-3
[Automated/Programmatic classification]: #subheading-4
[Next Steps]: #subheading-5
