---
title: "Můj první runbook Python ve službě Azure Automation | Microsoft Docs"
description: "Kurz vás provede vytvořením, otestováním a publikováním jednoduchého runbooku Python."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: gwallace
ms.openlocfilehash: 0399719e5f80869f0131691b89a9dc48b3fc48d7
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/02/2018
---
# <a name="my-first-python-runbook"></a>Můj první runbook Python

> [!div class="op_single_selector"]
> * [Grafický](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [Pracovní postup PowerShellu](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)
> 
> 

Tento kurz vás provede procesem vytvoření [Python runbook](automation-runbook-types.md#python-runbooks) ve službě Azure Automation. Začneme s jednoduchým runbookem, který společně otestujeme a publikujeme a současně vám vysvětlíme, jak sledovat stav úlohy runbooku. Potom runbook upravíme, aby skutečně spravoval prostředky Azure, v tomto případě virtuální počítač Azure. Nakonec runbook uděláme robustnější přidáním parametrů.

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

* Předplatné Azure. Pokud ještě žádné nemáte, můžete si [aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) nebo si zaregistrovat [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Účet Automation](automation-offering-get-started.md), abyste si mohli runbook podržet a mohli ověřovat prostředky Azure.  Tento účet musí mít oprávnění ke spuštění a zastavení virtuálního počítače.
* Virtuální počítač Azure. Počítač zastavíme a spustíme, proto to nesmí být produkční virtuální počítač.

## <a name="create-a-new-runbook"></a>Vytvořit nový runbook

Začneme vytvořením jednoduchého runbooku, který zobrazí text *Hello World*.

1. Na webu Azure Portal otevřete účet Automation.
   Stránka účtu Automation nabízí rychlý přehled prostředků v tomto účtu. Už byste tam měli mít nějaké prostředky. Většina z nich jsou moduly, které jsou automaticky obsažené v novém účtu Automation. Také potřebujete asset přihlašovacích údajů, který je uvedený v [požadavcích](#prerequisites).
1. Kliknutím na dlaždici **Runbooky** otevřete seznam runbooků.
   ![RunbooksControl](media/automation-first-runbook-textual-python/runbooks-control-tiles.png)
1. Kliknutím na tlačítko **Přidat runbook** a potom na **Vytvořit nový runbook** vytvořte nový runbook.
1. Dejte runbooku název *MyFirstRunbook-Python*.
1. V tomto případě vytvoříme k vytvoření [Python runbook](automation-runbook-types.md#python-runbooks) tak vyberte **Python 2** pro **typ Runbooku**.
1. Kliknutím na **Vytvořit** vytvoříte runbook a otevřete textový editor.

## <a name="add-code-to-the-runbook"></a>Přidání kódu do runbooku

Teď přidáme jednoduchý příkaz Tisknout text "Hello World":

```python
print("Hello World!")
```

Klikněte na tlačítko **Uložit** uložit sady runbook.

## <a name="test-the-runbook"></a>Otestování sady runbook

Před publikováním runbooku, které ho zpřístupní v produkčním prostředí, chceme runbook otestovat a ujistit se, že bude fungovat správně. Když runbook testujete, spustíte jeho  verzi **Koncept** a interaktivně zobrazíte jeho výsledek.

1. Kliknutím na **Testovací podokno** otevřete testovací podokno.
   ![Testovací podokno](media/automation-first-runbook-textual-python/automation-runbook-edit-controls-test.png)
1. Kliknutím na **Spustit** spustíte test. Měla by to být jediná povolená možnost.
1. Vytvoří se [úloha runbooku](automation-runbook-execution.md) a její stav se zobrazí.
   Počáteční stav úlohy bude *Zařazeno ve frontě*. To označuje, že čekáte na zpřístupnění pracovního procesu runbooku v cloudu. Když pracovní proces úlohu přijme, změní se stav na *Spouštění*, a když se runbook skutečně spustí, změní se na *Spuštěno*.
1. Po dokončení úlohy runbooku se zobrazí jeho výstup. V našem případě bychom měli vidět text *Hello World*.
1. Zavřete testovací podokno a vraťte se na plátno.

## <a name="publish-and-start-the-runbook"></a>Publikování a spuštění sady runbook

Sada runbook, který jsme vytvořili je stále v režimu konceptu. Před jeho spuštěním v produkčním prostředí ho musíme publikovat.
Když runbook publikujete, přepíšete existující publikované verzi verzí v režimu konceptu.
V našem případě nemáme publikovanou verzi ještě protože jsme runbook teprve vytvořili.

1. Kliknutím na **Publikovat** runbook publikujte a po zobrazení výzvy klikněte na **Ano**.
   ![Tlačítko Publikovat](media/automation-first-runbook-textual-python/automation-runbook-edit-controls-publish.png)
1. Pokud se posunete doleva, abyste runbook viděli v podokně **Runbooky**, zobrazí se **Stav vytváření**, který má hodnotu **Publikováno**.
1. Posuňte se zpět doprava, abyste viděli podokno **MyFirstRunbook-Python**.
   Možnosti v horní části nám umožňují spuštění runbooku, zobrazení runbooku, naplánování jeho spuštění někdy v budoucnu nebo vytvoření [webhooku](automation-webhooks.md), který umožní spuštění prostřednictvím volání protokolu HTTP.
1. My chceme runbook spustit, proto klikněte na **Spustit** a po otevření okna Spuštění runbooku klikněte na **OK**.
1. Podokno úlohy runbooku, který jsme vytvořili, je otevřené. Toto podokno můžeme zavřít, ale v tomto případě ho necháme otevřené, abychom mohli sledovat průběh úlohy.
1. Stav úlohy se zobrazuje v **souhrnu úlohy** a odpovídá stavům, které jsme viděli při testování runbooku.
1. Když se jako stav runbooku zobrazí *Dokončeno*, klikněte na **Výstup**. Otevře se podokno Výstup a my uvidíme text *Hello World*.
1. Zavřete podokno Výstup.
1. Klikněte na **Všechny protokoly** a otevřete podokno Datové proudy, které patří k úloze runbooku. Ve výstupu datového proudu byste měli vidět jenom text *Hello World*, ale můžou se zobrazit i jiné datové proudy z úlohy runbooku, například Podrobný nebo Chyba, pokud do nich runbook zapisuje.
1. Zavřete podokno datové proudy a podokno úloha a vraťte se do podokna MyFirstRunbook-Python.
1. Kliknutím na **Úlohy** otevřete podokno Úlohy, které patří k tomuto runbooku. Vypíšou se všechny úlohy, které tento runbook vytvořil. Tady bychom měli vidět jenom jednu úlohu, protože jsme ji spustili jenom jednou.
1. Na tuto úlohu můžete kliknout a otevřít podokno Úloha, které jsme zobrazili při spuštění runbooku. Pomocí této možnosti se můžete vrátit v čase a zobrazit si podrobnosti libovolné úlohy, která byla pro konkrétní runbook vytvořena.

## <a name="add-authentication-to-manage-azure-resources"></a>Přidání ověřování ke správě prostředků Azure

Runbook jsme otestovali a publikovali, ale zatím nedělá nic užitečného. Chceme po něm, aby spravoval prostředky Azure.
Ke správě prostředků Azure, skript má k ověření pomocí přihlašovacích údajů z vaší [účet Automation](automation-offering-get-started.md).

> [!NOTE]
> Účet Automation musí být vytvořen s hlavní funkce služby pro zde být certifikát runas.
> Pokud váš účet automation nebyl vytvořen pomocí objektu služby, můžete ověřovat pomocí metody popsané v [ověřit pomocí správy knihovny Azure pro jazyk Python](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate).

1. Kliknutím otevřete textový editor **upravit** v podokně MyFirstRunbook-Python.
1. Přidejte následující kód k ověření do Azure:
   ```python
   import os
   from azure.mgmt.compute import ComputeManagementClient
   import azure.mgmt.resource
   import automationassets

   def get_automation_runas_credential(runas_connection):
       from OpenSSL import crypto
       import binascii
       from msrestazure import azure_active_directory
       import adal

       # Get the Azure Automation RunAs service principal certificate
       cert = automationassets.get_automation_certificate("AzureRunAsCertificate")
       pks12_cert = crypto.load_pkcs12(cert)
       pem_pkey = crypto.dump_privatekey(crypto.FILETYPE_PEM,pks12_cert.get_privatekey())

       # Get run as connection information for the Azure Automation service principal
       application_id = runas_connection["ApplicationId"]
       thumbprint = runas_connection["CertificateThumbprint"]
       tenant_id = runas_connection["TenantId"]

       # Authenticate with service principal certificate
       resource ="https://management.core.windows.net/"
       authority_url = ("https://login.microsoftonline.com/"+tenant_id)
       context = adal.AuthenticationContext(authority_url)
       return azure_active_directory.AdalAuthentication(
       lambda: context.acquire_token_with_client_certificate(
               resource,
               application_id,
               pem_pkey,
               thumbprint)
       )

   # Authenticate to Azure using the Azure Automation RunAs service principal
   runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
   azure_credential = get_automation_runas_credential(runas_connection)
   ```

## <a name="add-code-to-create-python-compute-client-and-start-the-vm"></a>Přidejte kód pro vytvoření klienta Python výpočetní a spuštění virtuálního počítače

Pro práci s virtuálními počítači Azure, vytvořte instanci [Azure Compute klienta pro jazyk Python](https://docs.microsoft.com/python/api/azure.mgmt.compute.computemanagementclient?view=azure-python).

Pomocí klienta výpočetní spusťte virtuální počítač. Přidejte následující kód do sady runbook:

```python
# Initialize the compute management client with the RunAs credential and specify the subscription to work against.
compute_client = ComputeManagementClient(
azure_credential,
  str(runas_connection["SubscriptionId"])
)


print('\nStart VM')
async_vm_start = compute_client.virtual_machines.start("MyResourceGroup", "TestVM")
async_vm_start.wait()
```

Kde _MyResourceGroup_ je název skupiny prostředků, která obsahuje virtuální počítač, a _TestVM_ je název virtuálního počítače, který chcete spustit. 

Testování a spuštění sady runbook zjistíte, že spuštění virtuálního počítače.

## <a name="use-input-parameters"></a>Používání vstupních parametrů

Sada runbook aktuálně používá pevně hodnoty pro názvy skupiny prostředků a virtuální počítač.
Nyní Pojďme přidat kód, který získá tyto hodnoty ze vstupní parametry.

Používáme `sys.argv` proměnnou s cílem získat hodnoty parametrů.
Přidejte následující kód v sadě runbook hned po druhé `import` příkazy:

```python
import sys

resource_group_name = str(sys.argv[1])
vm_name = str(sys.argv[2])
```

Imports – to `sys` modul a vytvoří dvě proměnné, do kterých název skupiny prostředků a virtuálních počítačů.
Všimněte si, že element seznamu argumentů `sys.argv[0]`, je název skriptu a není uživatelský vstup.

Nyní jsme poslední dva řádky sady runbook, chcete-li hodnoty vstupní parametr místo pevně definovaných hodnot změnit:

```python
async_vm_start = compute_client.virtual_machines.start(resource_group_name, vm_name)
async_vm_start.wait()
```

Při spuštění sady runbook Python (buď na **Test** okno nebo jako publikované sady runbook), můžete zadat hodnoty pro parametry v **spustit sadu Runbook** okno pod **parametry**.

![Parametr Hodnota pole](media/automation-first-runbook-textual-python/runbook-python-param-highlight.png)

Po spuštění zadáním hodnoty v prvním poli, druhý se zobrazí a tak dále, tak, aby podle potřeby můžete zadat libovolný počet hodnot parametrů.

Hodnoty jsou k dispozici do skriptu jako `sys.argv` pole jako kód, který jsme právě přidali.

Zadejte název vaší skupiny prostředků jako hodnotu pro první parametr a název virtuálního počítače zahájíte jako hodnotu druhý parametr.

![Zadejte hodnoty parametrů](media/automation-first-runbook-textual-python/runbook-python-params.png)

Klikněte na tlačítko **OK** pro spuštění sady runbook. Sada runbook spuštěna a spouští se virtuální počítač, který jste zadali.

## <a name="next-steps"></a>Další postup

* První kroky s powershellovými runbooky najdete v článku [Můj první powershellový runbook](automation-first-runbook-textual-powershell.md).
* První kroky s grafickými runbooky najdete v článku [Můj první grafický runbook](automation-first-runbook-graphical.md).
* První kroky s runbooky pracovních postupů PowerShellu najdete v článku [Můj první runbook pracovního postupu PowerShellu](automation-first-runbook-textual.md).
* Další informace o typech runbooků, jejich výhodách a omezeních najdete v článku [Typy runbooků ve službě Azure Automation](automation-runbook-types.md).
* Další informace o vývoji pro Azure s Python najdete v tématu [Azure pro vývojáře v Pythonu](https://docs.microsoft.com/python/azure/?view=azure-python).
* Ukázkové sady runbook Python 2 najdete v tématu [Azure Automation Githubu](https://docs.microsoft.com/python/azure/?view=azure-python).
