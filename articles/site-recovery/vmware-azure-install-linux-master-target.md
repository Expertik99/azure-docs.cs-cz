---
title: Instalovat hlavní cílový server Linux převzetí služeb při selhání z Azure do místní | Microsoft Docs
description: Před opětovnou ochranu virtuální počítač s Linuxem, potřebujete hlavní cílový server Linux. Zjistěte, jak k jeho instalaci.
services: site-recovery
documentationcenter: ''
author: nsoneji
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 05/08/2018
ms.author: nisoneji
ms.openlocfilehash: a18bc242d10c9eb287d0f3645490acb9ca9fec2a
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/11/2018
---
# <a name="install-a-linux-master-target-server"></a>Instalovat hlavní cílový server Linux
Po selhání virtuálních počítačů do Azure, můžete můžete navrácení služeb po obnovení virtuálních počítačů k místní lokalitě. Chcete-li navrácení služeb po obnovení, je potřeba znovu nastavte ochranu virtuálního počítače z Azure do místní lokality. Pro tento proces budete potřebovat místní hlavní cílový server příjem provozu. 

Pokud chráněný virtuální počítač je virtuálního počítače s Windows, musíte Windows hlavní cíl. Pro virtuální počítač s Linuxem budete potřebovat hlavního cíle Linuxu. Přečtěte si následující kroky a zjistěte, jak vytvořit a nainstalovat hlavního cíle Linuxu.

> [!IMPORTANT]
> Od verze 9.10.0 hlavní cílový server, nejnovější hlavní cílový server můžete nainstalovat jenom na Ubuntu 16.04 server. Nové instalace nejsou povoleny u CentOS6.6 servery. Však můžete nadále upgradu vaší starého hlavního cílové servery pomocí 9.10.0 verze.

## <a name="overview"></a>Přehled
Tento článek obsahuje pokyny k instalaci hlavního cíle Linuxu.

POST dotazy nebo připomínky můžete na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="prerequisites"></a>Požadavky

* Zvolte hostitele, do které chcete nasadit na hlavním cíli, určete, pokud navrácení služeb po obnovení bude do existující virtuální počítač místně nebo do nového virtuálního počítače. 
    * Pro existující virtuální počítač hostitele hlavního cíle mají mít přístup k úložišti dat virtuálního počítače.
    * Pokud na místním virtuálním počítači (v případě obnovení do alternativního umístění) neexistuje, je navrácení služeb po obnovení virtuálního počítače vytvořit na stejném hostiteli jako hlavní cíl. Můžete vybrat libovolného hostitele ESXi k instalaci na hlavním cíli.
* Hlavní cíl musí být v síti, který může komunikovat s procesovým serverem a konfigurační server.
* Verze hlavního cíle musí být rovna nebo starší než verze procesového serveru a konfigurační server. Například pokud je verze konfigurace serveru 9.4, verze hlavního cíle může být 9.4 nebo 9.3, ale není 9.5.
* Na hlavním cíli lze pouze virtuální počítač VMware, nikoli na fyzický server.

## <a name="sizing-guidelines-for-creating-master-target-server"></a>Změna velikosti pokyny pro vytvoření hlavního cílového serveru

Vytvoření hlavního cíle podle následujících pokynů pro změnu velikosti:
- **Paměť RAM**: 6 GB nebo více
- **Velikost disku operačního systému**: 100 GB nebo více (pro instalaci operačního systému)
- **Velikost disku Další jednotka pro uchování**: 1 TB
- **Jader procesoru**: 4 jádra nebo více

Jsou podporovány následující podporované Ubuntu jádra.


|Řada jádra  |Podporovat až  |
|---------|---------|
|4.4      |4.4.0-81-Generic         |
|4.8      |4.8.0-56-Generic         |
|4.10     |4.10.0-24-Generic        |


## <a name="deploy-the-master-target-server"></a>Nasazení hlavního cílového serveru

### <a name="install-ubuntu-16042-minimal"></a>Nainstalujte Ubuntu 16.04.2 minimální

Proveďte následující kroky pro instalaci Ubuntu 16.04.2 64bitový operační systém.

1.   Přejděte na [stáhnout odkaz](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64), vyberte nejbližší anddownload zrcadlení soubor ISO Ubuntu 16.04.2 minimální 64-bit.
Ponechte soubor ISO Ubuntu 16.04.2 minimální 64-bit do jednotky DVD a spuštění systému.

1.  Vyberte **Angličtina** jako upřednostňovaný jazyk a potom vyberte **Enter**.
    
    ![Výběr jazyka](./media/vmware-azure-install-linux-master-target/image1.png)
1. Vyberte **nainstalovat Ubuntu Server**a potom vyberte **Enter**.

    ![Vyberte možnost instalace Ubuntu Server](./media/vmware-azure-install-linux-master-target/image2.png)

1.  Vyberte **Angličtina** jako upřednostňovaný jazyk a potom vyberte **Enter**.

    ![Vyberte angličtina jako upřednostňovaný jazyk](./media/vmware-azure-install-linux-master-target/image3.png)

1. Vyberte příslušnou možnost **časové pásmo** seznam možností a potom vyberte **Enter**.

    ![Vyberte správné časové pásmo](./media/vmware-azure-install-linux-master-target/image4.png)

1. Vyberte **ne** (výchozí možnost) a potom vyberte **Enter**.

     ![Konfigurace klávesnice](./media/vmware-azure-install-linux-master-target/image5.png)
1. Vyberte **angličtinu (US)** jako země původu klávesnice, a potom vyberte **Enter**.

1. Vyberte **angličtinu (US)** rozložení klávesnice a pak vyberte **Enter**.

1. Zadejte název hostitele pro server v **Hostname** a pak vyberte **pokračovat**.

1. Pokud chcete vytvořit uživatelský účet, zadejte uživatelské jméno a potom vyberte **pokračovat**.

      ![Vytvoření uživatelského účtu](./media/vmware-azure-install-linux-master-target/image9.png)

1. Zadejte heslo pro nový uživatelský účet a potom vyberte **pokračovat**.

1.  Potvrďte heslo pro nového uživatele a pak vyberte **pokračovat**.

    ![Potvrzení hesla](./media/vmware-azure-install-linux-master-target/image11.png)

1.  V další výběr šifrování domovský adresář, vyberte **ne** (výchozí možnost) a potom vyberte **Enter**.

1. Pokud se časové pásmo, které se zobrazí správný, vyberte **Ano** (výchozí možnost) a potom vyberte **Enter**. Chcete-li překonfigurovat časové pásmo, vyberte **ne**.

1. Možnosti dělicí metody, vyberte **na základě - použít celý disk**a potom vyberte **Enter**.

     ![Vyberte možnost použít pro dělicí metody](./media/vmware-azure-install-linux-master-target/image14.png)

1.  Vyberte příslušný disk z **vyberte disk do oddílu** možnosti a pak vyberte **Enter**.

    ![Vyberte disk](./media/vmware-azure-install-linux-master-target/image15.png)

1.  Vyberte **Ano** k zápisu změn na disk a potom vyberte **Enter**.

    ![Vyberte možnost výchozí](./media/vmware-azure-install-linux-master-target/image16-ubuntu.png)

1.  Ve výběru konfigurace proxy serveru, vyberte možnost výchozí, vyberte **pokračovat**a potom vyberte **Enter**.
     
     ![Vyberte, jak spravovat upgrady](./media/vmware-azure-install-linux-master-target/image17-ubuntu.png)

1.  Vyberte **žádné automatické aktualizace** a pak vyberte možnost ve výběru pro správu upgrady systému **Enter**.

     ![Vyberte, jak spravovat upgrady](./media/vmware-azure-install-linux-master-target/image18-ubuntu.png)

    > [!WARNING]
    > Protože Azure Site Recovery hlavní cílový server vyžaduje velmi konkrétní verzi Ubuntu, musíte zajistit, aby jádra zakázali upgrady pro virtuální počítač. Pokud se povolí, nějaké regulární upgrady způsobit hlavní cílový server fungovat správně. Zkontrolujte, zda jste vybrali **žádné automatické aktualizace** možnost.

1.  Vyberte výchozí možnosti. Pokud chcete openSSH pro připojení SSH, vyberte **OpenSSH server** a pak vyberte možnost **pokračovat**.

    ![Vyberte software](./media/vmware-azure-install-linux-master-target/image19-ubuntu.png)

1. V selction pro instalaci GRUB spouštěcí zavaděč, vyberte **Ano**a potom vyberte **Enter**.
     
    ![KONTROLE spuštění instalačního programu](./media/vmware-azure-install-linux-master-target/image20.png)


1. Vyberte odpovídající zařízení, pro instalaci spouštěcí zavaděč (pokud možno **/dev/sda**) a potom vyberte **Enter**.
     
    ![Vyberte příslušné zařízení](./media/vmware-azure-install-linux-master-target/image21.png)

1. Vyberte **pokračovat**a potom vyberte **Enter** k dokončení instalace.

    ![Dokončení instalace](./media/vmware-azure-install-linux-master-target/image22.png)

1. Po dokončení instalace, přihlaste se k virtuálnímu počítači s novými pověřeními uživatele. (Odkazovat na **krok 10** Další informace.)

1. Pomocí kroků popsaných v následující snímek obrazovky nastavení KOŘENOVÉ heslo uživatele. Přihlaste se jako KOŘENOVÉ uživatele.

    ![Nastavení KOŘENOVÉ heslo uživatele](./media/vmware-azure-install-linux-master-target/image23.png)


### <a name="configure-the-machine-as-a-master-target-server"></a>Nakonfigurujte počítač jako hlavní cílový server

Chcete-li získat ID pro každý SCSI pevný disk ve virtuálním počítači Linux, **disku. EnableUUID = TRUE** parametr musí být povolena. Chcete-li tento parametr, proveďte následující kroky:

1. Vypněte virtuální počítač.

2. Pravým tlačítkem na položku pro virtuální počítač v levém podokně a pak vyberte **upravit nastavení**.

3. Vyberte **možnosti** kartě.

4. V levém podokně vyberte **Upřesnit** > **Obecné**a pak vyberte **parametry konfigurace** tlačítko v pravé dolní části obrazovky.

    ![Otevřete konfigurační parametr](./media/vmware-azure-install-linux-master-target/image24-ubuntu.png) 

    **Parametry konfigurace** možnost není k dispozici, když je počítač spuštěn. Chcete-li na této kartě aktivní, vypněte virtuální počítač.

5. Zda řádek s **disku. EnableUUID** již existuje.

    - Pokud hodnota existuje a je nastavená na **False**, změňte hodnotu na **True**. (Hodnoty nejsou malá a velká písmena.)

    - Pokud hodnota existuje a je nastavená na **True**, vyberte **zrušit**.

    - Pokud hodnota neexistuje, vyberte **přidat řádek**.

    - Ve sloupci Název přidat **disku. EnableUUID**a pak nastavte hodnotu na **TRUE**.

    ![Kontroluje, zda disk. EnableUUID již existuje.](./media/vmware-azure-install-linux-master-target/image25.png)

#### <a name="disable-kernel-upgrades"></a>Zakázat upgrady jádra

Azure Site Recovery hlavní cílový server vyžaduje konkrétní verzi Ubuntu, zkontrolujte, zda jsou pro virtuální počítač vypnutá upgrady jádra. Pokud jsou povolené jádra upgrady, může to způsobit hlavní cílový server fungovat správně.

#### <a name="download-and-install-additional-packages"></a>Stáhněte a nainstalujte další balíčky

> [!NOTE]
> Ujistěte se, že máte připojení k Internetu stáhněte a nainstalujte další balíčky. Pokud nemáte připojení k Internetu, budete muset ručně najít tyto balíčky ot. / min a nainstalovat je.

 `apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx`

### <a name="get-the-installer-for-setup"></a>Získání Instalační program pro instalaci

Pokud hlavní cíl má připojení k Internetu, můžete použít následující kroky stáhnout instalační program. Jinak můžete zkopírujte instalační službu z procesového serveru a nainstalujte ji.

#### <a name="download-the-master-target-installation-packages"></a>Stáhněte si instalační balíčky hlavní cíl

[Stáhněte si nejnovější bits instalace hlavní cíl Linux](https://aka.ms/latestlinuxmobsvc).

Chcete-li stáhnout pomocí Linux, zadejte:

`wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz`

> [!WARNING]
> Ujistěte se, stáhněte a rozbalte instalační program v domovském adresáři. Pokud rozbalte k **/usr/místní**, instalace selže.


#### <a name="access-the-installer-from-the-process-server"></a>Přístup k Instalační program z procesového serveru

1. Na procesní server, přejděte na **C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

2. Instalační program vyžaduje soubor zkopírovat z procesového serveru a uložte ho jako **latestlinuxmobsvc.tar.gz** v domovském adresáři.


### <a name="apply-custom-configuration-changes"></a>Změny vlastní konfigurace

Chcete-li použít změny v vlastní konfigurace, použijte následující kroky:


1. Spusťte následující příkaz, který untar binárního souboru.

    `tar -zxvf latestlinuxmobsvc.tar.gz`

    ![Snímek obrazovky příkaz ke spuštění](./media/vmware-azure-install-linux-master-target/image16.png)

2. Spusťte následující příkaz, který udělit oprávnění.

    `chmod 755 ./ApplyCustomChanges.sh`


3. Spusťte následující příkaz pro spuštění skriptu.
    
    `./ApplyCustomChanges.sh`

> [!NOTE]
> Spusťte skript jenom jednou na serveru. Potom vypněte server. Po přidání disku, jak je popsáno v další části, restartujte server.

### <a name="add-a-retention-disk-to-the-linux-master-target-virtual-machine"></a>Přidejte uchování disk k virtuálnímu počítači hlavního cíle Linuxu

Pomocí následujících kroků můžete vytvořit disku pro uchování:

1. Připojit nový disk 1 TB k virtuálnímu počítači hlavního cíle Linuxu a potom počítač spustit.

2. Použití **vícenásobný -udou** příkaz Další funkce multipath ID disku pro uchování: **vícenásobný -le**

    ![Vícenásobný ID](./media/vmware-azure-install-linux-master-target/image22.png)

3. Formátování disku a pak vytvořit systém souborů na nový disk: **mkfs.ext4 /dev/mapper/< vícenásobný id uchování disku >**.
    
    ![Systém souborů](./media/vmware-azure-install-linux-master-target/image23-centos.png)

4. Po vytvoření systému souborů, připojte disk uchovávání informací.

    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```

5. Vytvořte **fstab** položka připojit jednotka pro uchování pokaždé, když bude systém při spuštění.
    
    `vi /etc/fstab`
    
    Vyberte **vložit** zahájíte úpravy souboru. Vytvořte nový řádek a potom vložte následující text. Upravte vícenásobný ID disku na základě Identifikátoru zvýrazněná více cest z předchozí příkaz.

    **/dev/mapper/ <Retention disks multipath id> /mnt/uchování ext4 rw 0 0**

    Vyberte **Esc**a pak zadejte **: QW** (zápisu a ukončení) zavřete okno editor.

### <a name="install-the-master-target"></a>Nainstalujte na hlavním cíli

> [!IMPORTANT]
> Verze hlavního cílového serveru musí být rovna nebo starší než verze procesového serveru a konfigurační server. Pokud není tato podmínka splněná, opětovné ochrany úspěšná, ale replikace selže.


> [!NOTE]
> Než nainstalujete hlavní cílový server, zkontrolujte, zda **/etc/hosts** souboru na virtuální počítač obsahuje položky, které mapují názvem místního hostitele na IP adresy, které jsou přidruženy všechny síťové adaptéry.

1. Kopírovat heslo z **C:\ProgramData\Microsoft Azure lokality Recovery\private\connection.passphrase** na konfiguračním serveru. Potom uložte ho jako **passphrase.txt** ve stejném adresáři místní spuštěním následujícího příkazu:

    `echo <passphrase> >passphrase.txt`

    Příklad: 

       `echo itUx70I47uxDuUVY >passphrase.txt`
    

2. Poznamenejte si IP adresu konfiguračního serveru. Spusťte následující příkaz k instalaci hlavní cílový server a registrace serveru u konfigurační server.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    Příklad: 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

Počkejte na dokončení skriptu. Pokud se hlavní cíl zaregistruje úspěšně, se hlavní cíl je uvedený na **infrastruktura Site Recovery** na portálu.


#### <a name="install-the-master-target-by-using-interactive-installation"></a>Instalaci se hlavní cíl pomocí interaktivní instalace

1. Spusťte následující příkaz k instalaci na hlavním cíli. Pro roli agenta, vyberte **hlavní cíl**.

    ```
    ./install
    ```

2. Vyberte výchozí umístění pro instalaci a potom vyberte **Enter** pokračujte.

    ![Volba výchozí umístění pro instalaci hlavní cíl](./media/vmware-azure-install-linux-master-target/image17.png)

Po dokončení instalace zaregistrujte konfigurační server pomocí příkazového řádku.

1. Všimněte si IP adresu konfiguračního serveru. Budete ho potřebovat v dalším kroku.

2. Spusťte následující příkaz k instalaci hlavní cílový server a registrace serveru u konfigurační server.

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    Příklad: 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

     Počkejte na dokončení skriptu. Pokud se hlavní cíl se úspěšně registrována v instalaci, se hlavní cíl je uvedený na **infrastruktura Site Recovery** na portálu.


### <a name="install-vmware-tools--open-vm-tools-on-the-master-target-server"></a>Instalace nástroje VMware / otevřete nástroje virtuálního počítače na hlavním cílovém serveru

Musíte nainstalovat nástroje VMware nebo nástroje pro otevření virtuálního počítače na hlavním cíli, aby ho můžete zjistit úložiště data. Pokud nejsou nainstalovány nástroje, není v úložištích dat, uvedené na obrazovce opětovné ochrany. Po instalaci nástroje VMware je potřeba restartovat.

### <a name="upgrade-the-master-target-server"></a>Upgrade hlavní cílový server

Spusťte instalační program. Automaticky zjišťuje, zda je agent nainstalovaný na hlavním cíli. Pokud chcete upgradovat, vyberte **Y**.  Po dokončení instalace, zkontrolujte verzi hlavního cíle nainstalován pomocí následujícího příkazu:

`cat /usr/local/.vx_version`


Zobrazí se **verze** pole obsahuje číslo verze se hlavní cíl.

## <a name="common-issues"></a>Běžné problémy

* Ujistěte se, že jste nezapínejte řešení Storage vMotion na žádné součásti správy, jako je hlavní cíl. Pokud se hlavní cíl přesune po úspěšné opětovné ochrany, disků virtuálního počítače (VMDKs) nelze odpojit. V takovém případě navrácení služeb po obnovení selže.

* Hlavní cíl by neměl mít všechny snímky na virtuálním počítači. Pokud existují snímky, navrácení služeb po obnovení se nezdaří.

* Z důvodu některých vlastních konfigurací seskupování síťové rozhraní je zakázané během spouštění a nemůže inicializovat agenta hlavní cíl. Ujistěte se, že následující vlastnosti jsou správně nastaveny. Zkontrolujte tyto vlastnosti v Ethernet karty souboru /etc/sysconfig/network-scripts/ifcfg-eth *.
    * BOOTPROTO=dhcp
    * ONBOOT = Ano


## <a name="next-steps"></a>Další postup
Po dokončení instalace a registrace hlavního cíle, zobrazí se hlavní cíl se zobrazují v **hlavní cíl** kapitoly **infrastruktura Site Recovery**, v části Konfigurace Přehled serveru.

Teď můžete pokračovat s [vytvoření](vmware-azure-reprotect.md), za nímž následují navrácení služeb po obnovení.

