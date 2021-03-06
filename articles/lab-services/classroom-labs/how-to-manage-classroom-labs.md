---
title: Správa testovacích prostředí v učebnách ve službě Azure Lab Services | Dokumentace Microsoftu
description: Zjistěte, jak vytvořit a nakonfigurovat prostředí v učebně, zobrazit všichni testovací prostředí v učebnách, sdílet registraci propojit s uživatelem testovacího prostředí nebo odstranění testovacího prostředí.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 48056d6e2988dd674351aca83526032175c355b6
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214388"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Správa testovacích prostředí v učebnách ve službě Azure Lab Services 
Tento článek popisuje postup vytvoření a konfigurace testovacího prostředí v učebně, zobrazení všech testovacích prostředí v učebnách nebo odstranění testovacího prostředí v učebně.

## <a name="prerequisites"></a>Požadavky
Pokud chcete nastavit testovací prostředí v učebně v účtu testovacího prostředí, musíte v účtu testovacího prostředí být členem role **Autor testovacího prostředí**. Účet, který jste použili k vytvoření účtu testovacího prostředí se automaticky přidá do této role. Vlastník testovacího prostředí můžete přidat další uživatele do role Tvůrce prostředí, pomocí kroků v následujícím článku: [přidání uživatele do role Tvůrce prostředí](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Vytvoření testovacího prostředí v učebně

1. Přejděte na [web Azure Lab Services](https://labs.azure.com).
2. Vyberte **Sign in** (Přihlásit se) a zadejte své přihlašovací údaje. Azure Lab Services podporuje účty a účty Microsoft.
3. V okně **New Lab** (Nové testovací prostředí) proveďte následující akce: 
    1. Zadejte **název** testovacího prostředí v učebně. 
    2. Vyberte **velikost** virtuálního počítače, který chcete v učebně použít.
    3. Vyberte **image**, která se má použít k vytvoření virtuálního počítače.
    4. Zadejte **výchozí pověření** pro protokolování do virtuálních počítačů v testovacím prostředí.
    7. Vyberte **Uložit**.

        ![Vytvoření testovacího prostředí v učebně](../media/how-to-manage-classroom-labs/new-lab-window.png)
1. Zobrazí se **řídicí panel** testovacího prostředí. 
    
    ![Řídicí panel testovacího prostředí v učebně](../media/how-to-manage-classroom-labs/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Konfigurace zásad používání

1. Vyberte **Usage policy** (Zásady používání). 
2. V nastavení **Usage policy** (Zásady používání) zadejte **počet uživatelů**, kteří mohou testovací prostředí používat.
3. Vyberte **Save** (Uložit). 

    ![Zásady používání](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="set-up-the-template"></a>Nastavení šablony
Šablona v testovacím prostředí je základní image virtuálního počítače, ze které se vytváří všechny virtuální počítače uživatelů. Nastavte virtuální počítač šablony tak, aby byl nakonfigurovaný přesně podle toho, co chcete uživatelům testovacího prostředí poskytnout. Můžete zadat název a popis šablony, které uvidí uživatelé testovacího prostředí. Publikujte šablonu, aby instance šablony virtuálního počítače byly dostupné všem uživatelům testovacího prostředí.  

### <a name="set-template-title-and-description"></a>Sada šablony nadpis a popis
1. V části **Template** (Šablona) vyberte **Edit** (Upravit) (ikona tužky) u šablony. 
2. V okně **User view** (Zobrazení uživatele) zadejte **název** šablony.
3. Zadejte **popis** šablony.
4. Vyberte **Save** (Uložit).

    ![Popis testovacího prostředí v učebně](../media/how-to-manage-classroom-labs/lab-description.png)

### <a name="set-up-the-template-vm"></a>Nastavení šablony virtuálního počítače
 Než šablonu virtuálního počítače zpřístupníte studentům, připojíte se k ní a nainstalujete na ní požadovaný software. 

1. Počkejte, až bude šablona virtuálního počítače připravená. Jakmile bude připravená, mělo by se aktivovat tlačítko **Start** (Spustit). Pokud chcete virtuální počítač spustit, vyberte **Start** (Spustit).

    ![Spuštění šablony virtuálního počítače](../media/tutorial-setup-classroom-lab/start-template-vm.png)
1. Pokud se chcete k virtuálnímu počítači připojit, vyberte **Connect** (Připojit) a postupujte podle pokynů. 

    ![Připojení k šabloně virtuálního počítače](../media/tutorial-setup-classroom-lab/connect-template-vm.png)
1. Nainstalujte software, který studenti v testovacím prostředí potřebují (například sadu Visual Studio, Průzkumníka služby Azure Storage atd.). 
2. Odpojte se od šablony virtuálního počítače (ukončete relaci vzdálené plochy). 
3. **Zastavte** šablonu virtuálního počítače výběrem **Stop** (Zastavit). 

    ![Zastavení šablony virtuálního počítače](../media/tutorial-setup-classroom-lab/stop-template-vm.png)


### <a name="publish-the-template"></a>Publikování šablony 
Jakmile publikujete šablonu, vytvoří služba Azure Lab Services pomocí této šablony virtuální počítače v testovacím prostředí. Počet virtuálních počítačů, které se v tomto procesu vytvoří, se rovná maximálnímu počtu uživatelů, kteří mohou k testovacímu prostředí přistupovat. Tento počet můžete nastavit v zásadách používání testovacího prostředí. Všechny virtuální počítače mají stejnou konfiguraci jako šablona. 

1. V části **Template** (Šablona) vyberte **Publish** (Publikovat). 

    ![Publikování šablony virtuálního počítače](../media/tutorial-setup-classroom-lab/public-access.png)
1. V okně **Publish** (Publikovat) vyberte možnost **Published** (Publikováno). 
2. Teď vyberte tlačítko **Publish** (Publikovat). Tento proces může chvíli trvat v závislosti na počtu vytvářených virtuálních počítačů, který je stejný jako počet uživatelů s povoleným přístupem k testovacímu prostředí.
    
    > [!IMPORTANT]
    > Jakmile je šablona publikovaná, nejde to vrátit. 
4. Přepněte na stránku **Virtual machines** (Virtuální počítače) a zkontrolujte, že se zobrazí virtuální počítače ve stavu **Unassigned** (Nepřiřazeno). Tyto virtuální počítače ještě nejsou přiřazené ke studentům. 

    ![Virtuální počítače](../media/tutorial-setup-classroom-lab/virtual-machines.png)
5. Počkejte na vytvoření virtuálních počítačů. Měly by být ve stavu **Stopped** (Zastaveno). Na této stránce můžete spustit studentský virtuální počítač, připojit se k němu, zastavit ho a odstranit ho. Virtuální počítače můžete spustit na této stránce nebo jejich spuštění můžete nechat na studentech. 

    ![Virtuální počítače v zastaveném stavu](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="send-registration-link-to-students"></a>Odeslání odkazu pro registraci studentům

1. Přepněte na zobrazení **Dashboard** (Řídicí panel). 
2. Vyberte dlaždici **User registration** (Registrace uživatelů).

    ![Odkaz pro registraci studenta](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. V dialogovém okně **User registration** (Registrace uživatelů) vyberte tlačítko **Copy** (Kopírovat). Odkaz se zkopíruje do schránky. Vložte ho do editoru e-mailů a e-mail odešlete studentovi. 

    ![Odkaz pro registraci studenta](../media/tutorial-setup-classroom-lab/registration-link.png)
2. V dialogovém okně **User registration** (Registrace uživatelů) vyberte **Close** (Zavřít). 

## <a name="view-all-classroom-labs"></a>Zobrazit všechny testovací prostředí v učebnách
1. Přejděte do [portálu Azure Lab Services](https://labs.azure.com).
2. Ověřte, že testovací prostředí v rámci účtu vybraného testovacího prostředí. 

    ![Všechny testovací prostředí](../media/how-to-manage-classroom-labs/all-labs.png)
3. Pomocí rozevíracího seznamu v horní části vyberte účet jiného testovacího prostředí. Zobrazí testovací prostředí v rámci účtu vybraného testovacího prostředí. 

## <a name="delete-a-classroom-lab"></a>Odstranění testovacího prostředí v učebně
1. Na dlaždici pro testovací prostředí vyberte tři tečky (...) v pravém rohu. 

    ![Výběr testovacího prostředí](../media/how-to-manage-classroom-labs/select-three-dots.png)
2. Vyberte **Odstranit**. 

    ![Tlačítko Odstranit](../media/how-to-manage-classroom-labs/delete-button.png)
3. Na **Delete lab** dialogu **odstranit**. 

    ![Dialogové okno Odstranit](../media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)
 
## <a name="manage-student-vms"></a>Správa virtuálních počítačů studenta
Jakmile studenty registru pomocí Azure Lab Services pomocí registrace propojení za předpokladu, uvidíte virtuální počítače přiřazené pro studenty na **virtuálních počítačů** kartu. 

![Virtuální počítače přiřazené pro studenty](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

Můžete provádět následující úlohy na studenta virtuálního počítače: 

- Zastavení virtuálního počítače, pokud je virtuální počítač spuštěný. 
- Spuštění virtuálního počítače, pokud je virtuální počítač zastavený. 
- Připojte se k virtuálnímu počítači. 
- Odstranění virtuálního počítače. 

## <a name="next-steps"></a>Další postup
Začínáme s nastavením testovacího prostředí pomocí Azure Lab Services:

- [Nastavení testovacího prostředí v učebně](how-to-manage-classroom-labs.md)
- [Nastavení testovacího prostředí](../tutorial-create-custom-lab.md)
