---
title: Offline migrace SQL Serveru do služby Azure SQL Database pomocí služby Azure Database Migration Service | Microsoft Docs
description: Zjistěte, jak pomocí služby Azure Database Migration Service provést offline migraci z místního SQL Serveru do služby Azure SQL Database.
services: dms
author: edmacauley
ms.author: jtoland
manager: craigg
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 08/24/2018
ms.openlocfilehash: 0cb4a5169036fc0a24a5fc5c86d232bb587a6684
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/24/2018
ms.locfileid: "42886289"
---
# <a name="migrate-sql-server-to-azure-sql-database-offline-using-dms"></a>Offline migrace SQL Serveru do služby Azure SQL Database pomocí DMS
Pomocí služby Azure Database Migration Service můžete migrovat databáze z místní instance SQL Serveru do služby [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/). V tomto kurzu pomocí služby Azure Database Migration Service provedete migraci databáze **Adventureworks2012** obnovené do místní instance SQL Serveru 2016 (nebo novější) do služby Azure SQL Database.

V tomto kurzu se naučíte:
> [!div class="checklist"]
> * Posouzení místní databáze pomocí nástroje Data Migration Assistant
> * Migrace ukázkového schématu pomocí nástroje Data Migration Assistant
> * Vytvoření instance služby Azure Database Migration Service
> * Vytvoření projektu migrace pomocí služby Azure Database Migration Service
> * Spuštění migrace
> * Monitorování migrace
> * Stažení sestavy migrace

## <a name="prerequisites"></a>Požadavky
Pro absolvování tohoto kurzu je potřeba provést následující:

- Stáhněte a nainstalujte [SQL Server 2016 nebo novější](https://www.microsoft.com/sql-server/sql-server-downloads) (jakoukoli edici).
- Povolte protokol TCP/IP, který se ve výchozím nastavení zakáže během instalace SQL Serveru Express, a to podle pokynů v článku [Povolení nebo zakázání síťového protokolu serveru](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Vytvořte instanci služby Azure SQL Database podle podrobných pokynů v článku [Vytvoření databáze SQL Azure na webu Azure Portal](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- Stáhněte a nainstalujte nástroj [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) verze 3.3 nebo novější.
- Vytvořte pro službu Azure Database Migration Service virtuální síť s použitím modelu nasazení Azure Resource Manager, který poskytuje možnosti připojení typu Site-to-Site k místním zdrojovým serverům prostřednictvím [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) nebo sítě [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Zajistěte, aby pravidla skupiny zabezpečení virtuální sítě Azure neblokovala následující komunikační porty: 443, 53, 9354, 445 a 12000. Další podrobnosti o filtrování provozu pomocí skupiny zabezpečení virtuální sítě Azure najdete v článku [Filtrování provozu sítě s použitím skupin zabezpečení sítě](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Nakonfigurujte bránu [Windows Firewall pro přístup k databázovému stroji](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Otevřete bránu Windows Firewall a povolte službě Azure Database Migration Service přístup ke zdrojovému SQL Serveru, který ve výchozím nastavení probíhá přes port TCP 1433.
- Pokud provozujete několik pojmenovaných instancí SQL Serveru s využitím dynamických portů, možná budete chtít povolit službu SQL Browser a přístup k portu UDP 1434 přes vaše brány firewall, aby se služba Azure Database Migration Service mohla připojit k pojmenované instanci na vašem zdrojovém serveru.
- Pokud před zdrojovými databázemi používáte zařízení brány firewall, možná bude potřeba přidat pravidla brány firewall, která službě Azure Database Migration Service povolí přístup ke zdrojovým databázím za účelem migrace.
- Vytvořte pro server služby Azure SQL Database [pravidlo brány firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) na úrovni serveru, které službě Azure Database Migration Service povolí přístup k cílovým databázím. Zadejte rozsah podsítí virtuální sítě použité pro službu Azure Database Migration Service.
- Ujistěte se, že přihlašovací údaje použité pro připojení ke zdrojové instanci SQL Serveru mají oprávnění [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql).
- Ujistěte se, že přihlašovací údaje použité pro připojení k cílové instanci služby Azure SQL Database mají oprávnění CONTROL DATABASE k cílovým databázím SQL Azure.

## <a name="assess-your-on-premises-database"></a>Posouzení místní databáze
Před migrací dat z místní instance SQL Serveru do služby Azure SQL Database je potřeba databázi SQL Serveru posoudit z hlediska případných blokujících problémů, které by migraci mohly bránit. Proveďte posouzení místní databáze pomocí nástroje Data Migration Assistant verze 3.3 nebo novější a postupujte při tom podle kroků popsaných v článku [Posuzování migrace SQL Serveru](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem). Následuje souhrn požadovaných kroků:
1.  V nástroji Data Migration Assistant vyberte ikonu Nový (+) a pak vyberte typ projektu **Posouzení**.
2.  Zadejte název projektu, v textovém poli **Typ zdrojového serveru** vyberte **SQL Server**, v textovém poli **Typ cílového serveru** vyberte **Azure SQL Database** a pak vyberte **Vytvořit** a vytvořte projekt.

    Pokud posuzujete migraci zdrojové databáze SQL Serveru do služby Azure SQL Database, můžete zvolit jeden nebo oba z následujících typů sestavy posouzení:
    - Kontrola kompatibility databáze
    - Kontrola parity funkcí

    Ve výchozím nastavení jsou vybrané oba typy sestavy.

3.  V nástroji Data Migration Assistant na obrazovce **Možnosti** vyberte **Další**.
4.  Na obrazovce **Vybrat zdroje** v dialogovém okně **Připojit k serveru** zadejte podrobnosti o připojení k vašemu SQL Serveru a pak vyberte **Připojit**.
5.  V dialogovém okně **Přidat zdroje** vyberte **AdventureWorks2012**, pak **Přidat** a pak vyberte **Spustit posouzení**.

    Po dokončení posouzení se zobrazí výsledky, jak je znázorněno na následujícím obrázku:

    ![Posouzení migrace dat](media\tutorial-sql-server-to-azure-sql\dma-assessments.png)

    V případě služby Azure SQL Database posouzení identifikují problémy s paritou funkcí a problémy blokující migraci.

    - Kategorie **Parita funkcí SQL Serveru** poskytuje komplexní sadu doporučení, alternativní postupy, které jsou v Azure k dispozici, a postupy pro zmírnění problémů, které vám pomůžou naplánovat náročnost projektů migrace.
    - Kategorie **Problémy s kompatibilitou** identifikuje částečně podporované nebo nepodporované funkce, které odrážejí problémy, které můžou blokovat migraci místních databází SQL Serveru do služby Azure SQL Database. K dispozici jsou také doporučení, která vám pomůžou tyto problémy vyřešit.

6.  Výběrem konkrétních možností zkontrolujte výsledky posouzení z hlediska problémů blokujících migraci a problémů s paritou funkcí.

## <a name="migrate-the-sample-schema"></a>Migrace ukázkového schématu
Jakmile budete spokojeni s posouzením a vybranou databází jako vhodným kandidátem pro migraci do služby Azure SQL Database, použijte nástroj Data Migration Assistant k migraci schématu do služby Azure SQL Database.

> [!NOTE]
> Před vytvořením projektu migrace v nástroji Data Migration Assistant se ujistěte, že už máte zřízenou databázi SQL Azure, jak je uvedeno v požadavcích. Pro účely tohoto kurzu se předpokládá, že je název služby Azure SQL Database **AdventureWorksAzure**, ale můžete zadat libovolný název.

Pokud chcete migrovat schéma **AdventureWorks2012** do služby Azure SQL Database, proveďte následující kroky:

1.  V nástroji Data Migration Assistant vyberte ikonu Nový (+) a pak v části **Typ projektu** vyberte **Migrace**.
2.  Zadejte název projektu, v textovém poli **Typ zdrojového serveru** vyberte **SQL Server** a pak v textovém poli **Typ cílového serveru** vyberte **Azure SQL Database**.
3.  V části **Rozsah migrace** vyberte **Pouze schéma**.

    Po provedení předchozích kroků by se mělo zobrazit rozhraní nástroje Data Migration Assistant, jak je znázorněno na následujícím obrázku:
    
    ![Vytvoření projektu nástroje Data Migration Assistant](media\tutorial-sql-server-to-azure-sql\dma-create-project.png)

4.  Vyberte **Vytvořit** a vytvořte projekt.
5.  V nástroji Data Migration Assistant zadejte podrobnosti o připojení ke zdrojovému SQL Serveru, vyberte **Připojit** a pak vyberte databázi **AdventureWorks2012**.

    ![Podrobnosti o připojení ke zdroji v nástroji Data Migration Assistant](media\tutorial-sql-server-to-azure-sql\dma-source-connect.png)

6.  Vyberte **Další**, v části **Připojit k cílovému serveru** zadejte podrobnosti o připojení k cílové databázi SQL Azure, vyberte **Připojit** a pak vyberte databázi **AdventureWorksAzure**, kterou jste předtím zřídili v databázi SQL Azure.

    ![Podrobnosti o připojení k cíli v nástroji Data Migration Assistant](media\tutorial-sql-server-to-azure-sql\dma-target-connect.png)

7.  Vyberte **Další** a přejděte na obrazovku **Vybrat objekty**, na které můžete určit objekty schématu v databázi **AdventureWorks2012**, které je potřeba nasadit do služby Azure SQL Database.

    Ve výchozím nastavení jsou vybrané všechny objekty.

    ![Generování skriptů SQL](media\tutorial-sql-server-to-azure-sql\dma-assessment-source.png)

8.  Výběrem možnosti **Vygenerovat skript SQL** vytvořte skripty SQL a pak zkontrolujte, jestli skripty neobsahují chyby.

    ![Skript schématu](media\tutorial-sql-server-to-azure-sql\dma-schema-script.png)

9.  Vyberte **Nasadit schéma** a nasaďte schéma do služby Azure SQL Database. Po nasazení schématu zkontrolujte případné anomálie na cílovém serveru.

    ![Nasazení schématu](media\tutorial-sql-server-to-azure-sql\dma-schema-deploy.png)

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registrace poskytovatele prostředků Microsoft.DataMigration
1. Přihlaste se k webu Azure Portal, vyberte **Všechny služby** a pak vyberte **Předplatná**.
 
   ![Zobrazení předplatných na portálu](media\tutorial-sql-server-to-azure-sql\portal-select-subscription1.png)
       
2. Vyberte předplatné, ve kterém chcete vytvořit instanci služby Azure Database Migration Service, a pak vyberte **Poskytovatelé prostředků**.
 
    ![Zobrazení poskytovatelů prostředků](media\tutorial-sql-server-to-azure-sql\portal-select-resource-provider.png)
    
3.  Vyhledejte „migration“ a pak napravo od **Microsoft.DataMigration** vyberte **Zaregistrovat**.
 
    ![Registrace poskytovatele prostředků](media\tutorial-sql-server-to-azure-sql\portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Vytvoření instance
1.  Na webu Azure Portal vyberte **+ Vytvořit prostředek**, vyhledejte „Azure Database Migration Service“ a pak v rozevíracím seznamu vyberte **Azure Database Migration Service**.

    ![Azure Marketplace](media\tutorial-sql-server-to-azure-sql\portal-marketplace.png)

2.  Na obrazovce **Azure Database Migration Service** vyberte **Vytvořit**.
 
    ![Vytvoření instance služby Azure Database Migration Service](media\tutorial-sql-server-to-azure-sql\dms-create1.png)
  
3.  Na obrazovce **Vytvořit službu Migration Service** zadejte název služby, předplatné a novou nebo existující skupinu prostředků.

4. Vyberte umístění, ve kterém chcete vytvořit instanci služby Azure Database Migration Service. 

5. Vyberte existující virtuální síť nebo vytvořte novou.

    Virtuální síť poskytuje službě Azure Database Migration Service přístup ke zdrojovému SQL Serveru a cílové instanci služby Azure SQL Database.

    Další informace o vytvoření virtuální sítě na webu Azure Portal najdete v článku [Vytvoření virtuální sítě pomocí webu Azure Portal](https://aka.ms/DMSVnet).

6. Vyberte cenovou úroveň.

    Další informace o nákladech a cenových úrovních najdete na [stránce s cenami](https://aka.ms/dms-pricing).

    Pokud potřebujete pomoc s výběrem správné úrovně služby Azure Database Migration Service, projděte si doporučení v [tomto](https://go.microsoft.com/fwlink/?linkid=861067) příspěvku.  

     ![Konfigurace nastavení instance služby Azure Database Migration Service](media\tutorial-sql-server-to-azure-sql\dms-settings2.png)

7.  Vyberte **Vytvořit** a vytvořte službu.

## <a name="create-a-migration-project"></a>Vytvoření projektu migrace
Po vytvoření služby ji vyhledejte na webu Azure Portal, otevřete ji a pak vytvořte nový projekt migrace.

1. Na webu Azure Portal vyberte **Všechny služby**, vyhledejte „Azure Database Migration Service“ a pak vyberte **Služby Azure Database Migration Service**.
 
      ![Vyhledání všech instancí služby Azure Database Migration Service](media\tutorial-sql-server-to-azure-sql\dms-search.png)

2. Na obrazovce **Služby Azure Database Migration Service** vyhledejte název instance služby Azure Database Migration Service, kterou jste vytvořili, a vyberte ji.
 
     ![Vyhledání instance služby Azure Database Migration Service](media\tutorial-sql-server-to-azure-sql\dms-instance-search.png)
 
3. Vyberte **+ Nový projekt migrace**.
4. Na obrazovce **Nový projekt migrace** zadejte název projektu, v textovém poli **Typ zdrojového serveru** vyberte **SQL Server**, v textovém poli **Typ cílového serveru** vyberte **Azure SQL Database** a pak v části **Zvolte typ aktivity** vyberte **Offline migrace dat**. 

    ![Vytvoření projektu Database Migration Service](media\tutorial-sql-server-to-azure-sql\dms-create-project2.png)

5.  Vyberte **Vytvořit a spustit aktivitu** a vytvořte projekt a spusťte aktivitu migrace.

## <a name="specify-source-details"></a>Zadání podrobností o zdroji
1. Na obrazovce **Podrobnosti zdroje migrace** zadejte podrobnosti o připojení ke zdrojové instanci SQL Serveru.
 
    Jako název zdrojové instance SQL Serveru nezapomeňte použít plně kvalifikovaný název domény. V situacích, kdy není možný překlad názvů DNS, můžete použít také IP adresu.

2. Pokud jste na svém zdrojovém serveru nenainstalovali důvěryhodný certifikát, zaškrtněte políčko **Důvěřovat certifikátu serveru**.

    Pokud není nainstalovaný důvěryhodný certifikát, SQL Server při spuštění instance vygeneruje certifikát podepsaný svým držitelem. Tento certifikát slouží k šifrování přihlašovacích údajů pro připojení klientů.

    > [!CAUTION]
    > Připojení SSL šifrovaná pomocí certifikátu podepsaného svým držitelem neposkytují silné zabezpečení. Jsou náchylná na útoky, kdy se útočníci vydávají za prostředníky. Na připojení SSL s použitím certifikátů podepsaných svým držitelem byste neměli spoléhat v produkčním prostředí ani na serverech připojených k internetu.

   ![Podrobnosti zdroje](media\tutorial-sql-server-to-azure-sql\dms-source-details2.png)

## <a name="specify-target-details"></a>Zadání podrobností o cíli
1. Vyberte **Uložit** a pak na obrazovce **Podrobnosti cíle migrace** zadejte podrobnosti o připojení k cílovému serveru služby Azure SQL Database. Tím je předem zřízená služba Azure SQL Database, do které se pomocí nástroje Data Migration Assistant nasadilo schéma **AdventureWorks2012**.

    ![Výběr cíle](media\tutorial-sql-server-to-azure-sql\dms-select-target2.png)

2. Vyberte **Uložit** a pak na obrazovce **Mapovat na cílové databáze** namapujte zdrojovou a cílovou databázi pro migraci.

    Pokud cílová databáze obsahuje stejný název databáze jako zdrojová databáze, služba Azure Database Migration Service ve výchozím nastavení vybere cílovou databázi.

    ![Mapování na cílové databáze](media\tutorial-sql-server-to-azure-sql\dms-map-targets-activity2.png)

3. Vyberte **Uložit**, na obrazovce **Vybrat tabulky** rozbalte seznam tabulek a zkontrolujte seznam ovlivněných polí.

    Všimněte si, že služba Azure Database Migration Service automaticky vybere všechny prázdné zdrojové tabulky, které existují v cílové instanci služby Azure SQL Database. Pokud chcete znovu migrovat tabulky, které již obsahují data, musíte tabulky explicitně vybrat v tomto okně.

    ![Výběr tabulek](media\tutorial-sql-server-to-azure-sql\dms-configure-setting-activity2.png)

4.  Vyberte **Uložit** a na obrazovce **Shrnutí migrace** do textového pole **Název aktivity** zadejte název aktivity migrace.

5. Rozbalením části **Možnost ověřování** zobrazte obrazovku **Vybrat možnost ověřování** a pak určete, jestli chcete u migrovaných databází ověřovat **Porovnání schématu**, **Konzistenci dat** nebo **Správnost dotazu**.
    
    ![Výběr možnosti ověřování](media\tutorial-sql-server-to-azure-sql\dms-configuration2.png)

6.  Vyberte **Uložit**, zkontrolujte souhrnné informace a ujistěte se, že podrobnosti zdroje a cíle odpovídají dříve zadaným informacím.

    ![Shrnutí migrace](media\tutorial-sql-server-to-azure-sql\dms-run-migration2.png)

## <a name="run-the-migration"></a>Spuštění migrace
- Vyberte **Spustit migraci**.

    Zobrazí se okno aktivity migrace a **Stav** aktivity bude **Probíhající**.

    ![Stav aktivity](media\tutorial-sql-server-to-azure-sql\dms-activity-status1.png)

## <a name="monitor-the-migration"></a>Monitorování migrace
1. Na obrazovce aktivity migrace vyberte **Aktualizovat** a aktualizujte zobrazení, dokud se **Stav** migrace nezmění na **Dokončeno**.

    ![Stav aktivity – Dokončeno](media\tutorial-sql-server-to-azure-sql\dms-completed-activity1.png)

2. Po dokončení migrace vyberte **Stáhnout sestavu** a získejte sestavu s výpisem podrobností souvisejících s procesem migrace.

3. Zkontrolujte cílové databáze na cílovém serveru služby Azure SQL Database.

### <a name="additional-resources"></a>Další zdroje

 * Praktické cvičení [Migrace SQL pomocí služby Azure Database Migration Service (DMS)](https://www.microsoft.com/handsonlabs/SelfPacedLabs/?storyGuid=3b671509-c3cd-4495-8e8f-354acfa09587)