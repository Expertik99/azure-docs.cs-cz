---
title: Přesun databáze Azure AD Connect z SQL Serveru Express na SQL Server. | Dokumenty Microsoft
description: Tento dokument popisuje, jak přesunout databázi Azure AD Connect z místního serveru SQL Server Express na vzdálený SQL Server.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: get-started-article
ms.date: 03/19/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 16b8cb843b7081c2489f1b8048d896858c5424c2
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231764"
---
# <a name="move-azure-ad-connect-database-from-sql-server-express-to-sql-server"></a>Přesun databáze Azure AD Connect z SQL Serveru Express na SQL Server 

Tento dokument popisuje, jak přesunout databázi Azure AD Connect z místního serveru SQL Server Express na vzdálený SQL Server.  K provedení této úlohy můžete použít následující postupy.

## <a name="about-this-scenario"></a>O tomto scénáři
Následuje několik stručných informací o tomto scénáři.  V tomto scénáři je na jednom řadiči domény s Windows Serverem 2016 nainstalovaný nástroj Azure AD Connect verze 1.1.819.0.  Pro svou databázi využívá integrovaný SQL Server 2012 Express Edition.  Databáze se přesune na server SQL Server 2017.

![](media/active-directory-aadconnect-move-db/move1.png)

## <a name="move-the-azure-ad-connect-database"></a>Přesun databáze Azure AD Connect
Pomocí následujících kroků přesuňte databázi Azure AD Connect na vzdálený SQL Server.

1.  Na serveru Azure AD Connect přejděte do části **Služby** a zastavte službu **Microsoft Azure AD Sync**.
2. Vyhledejte složku **%Program Files%\Microsoft Azure AD Sync/Data/** a zkopírujte soubory **ADSync.mdf** a **ADSync_log.mdf** na vzdálený SQL Server.
3. Na serveru Azure AD Connect restartujte službu **Microsoft Azure AD Sync**.
4. Odinstalujte Azure AD Connect tak, že přejdete do části Ovládací panely > Programy > Programy a funkce.  Vyberte Microsoft Azure AD Connect a v horní části klikněte na Odinstalovat.
5. Na vzdáleném SQL Serveru otevřete SQL Server Management Studio.
6. V části Databáze klikněte pravým tlačítkem a vyberte Připojit.
7. Na obrazovce **Připojit databáze** klikněte na **Přidat** a přejděte k souboru ADSync.mdf.  Klikněte na **OK**.
![](media/active-directory-aadconnect-move-db/move2.png)

8. Po připojení databáze se vraťte na server Azure AD Connect a nainstalujte Azure AD Connect.
9. Po dokončení instalace MSI se spustí průvodce Azure AD Connect v režimu expresní instalace. Zavřete obrazovku kliknutím na ikonu Ukončit.
![Uvítání](media/active-directory-aadconnect-existing-database/db1.png)
10. Spusťte nový příkazový řádek nebo novou relaci PowerShellu. Přejděte do složky <drive>\Program Files\Microsoft Azure AD Connect. Spuštěním příkazu .\AzureADConnect.exe /useexistingdatabase spusťte průvodce Azure AD Connect v režimu instalace Použít stávající databázi.
![PowerShell](media/active-directory-aadconnect-existing-database/db2.png)
11. Zobrazí se obrazovka Vítá vás Azure AD Connect. Jakmile odsouhlasíte licenční podmínky a oznámení o ochraně osobních údajů, klikněte na **Pokračovat**.
![Uvítání](media/active-directory-aadconnect-existing-database/db3.png)
12. Na obrazovce **Instalace požadovaných komponent** je povolená možnost **Použít existující SQL Server**. Zadejte název SQL Serveru, který je hostitelem databáze ADSync. Pokud instance stroje SQL použitá k hostování databáze ADSync není na SQL Serveru výchozí instancí, musíte zadat název instance stroje SQL. Dále, pokud není povolené procházení SQL, musíte zadat také číslo portu instance stroje SQL. Příklad:         
![Uvítání](media/active-directory-aadconnect-existing-database/db4.png)           

13. Na obrazovce **Připojení ke službě Azure AD** musíte zadat přihlašovací údaje globálního správce vašeho adresáře služby Azure AD. Je vhodné použít účet ve výchozí doméně onmicrosoft.com. Tento účet slouží jenom k vytvoření účtu služby v Azure AD, a po dokončení průvodce se už nepoužívá.
![Připojení](media/active-directory-aadconnect-existing-database/db5.png)
 
14. Na obrazovce **Připojení adresářů** je uvedená stávající doménová struktura AD nakonfigurovaná pro synchronizaci adresářů, vedle níž je ikona červeného křížku. K synchronizaci změn z místní doménové struktury AD se vyžaduje účet služby AD DS. Průvodce Azure AD Connect nemůže načíst přihlašovací údaje účtu služby AD DS uložené v databázi ADSync, protože jsou šifrované a může je dešifrovat pouze předchozí server Azure AD Connect. Klikněte na **Změnit přihlašovací údaje** a zadejte účet služby AD DS pro doménovou strukturu AD.
![Adresáře](media/active-directory-aadconnect-existing-database/db6.png)
 
 
15. V automaticky otevíraném dialogovém okně můžete buď (i) zadat přihlašovací údaje podnikového správce a nechat Azure AD Connect vytvořit účet služby AD DS za vás, nebo (ii) sami vytvořit účet služby AD DS a zadat jeho přihlašovací údaje do Azure AD Connect. Jakmile vyberete jednu z možností a zadáte potřebné přihlašovací údaje, kliknutím na **OK** zavřete automaticky otevírané dialogové okno.
![Uvítání](media/active-directory-aadconnect-existing-database/db7.png)
 
 
16. Po zadání přihlašovacích údajů se ikona červeného křížku změní na ikonu zeleného zaškrtnutí. Klikněte na **Další**.
![Uvítání](media/active-directory-aadconnect-existing-database/db8.png)
 
 
17. Na obrazovce **Připraveno ke konfiguraci** klikněte na **Nainstalovat**.
![Uvítání](media/active-directory-aadconnect-existing-database/db9.png)
 
 
18. Po dokončení instalace se na serveru Azure AD Connect automaticky zapne pracovní režim. Před vypnutím pracovního režimu se doporučuje zkontrolovat neočekávané změny v konfiguraci serveru a čekajících sestavách. 

## <a name="next-steps"></a>Další kroky

- Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
- [Instalace nástroje Azure AD Connect s využitím existující databáze ADSync](active-directory-aadconnect-existing-database.md)
- [Instalace nástroje Azure AD Connect pomocí oprávnění delegovaného správce SQL](active-directory-aadconnect-sql-delegation.md)

