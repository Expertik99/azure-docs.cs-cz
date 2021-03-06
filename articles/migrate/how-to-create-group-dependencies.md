---
title: Upravit skupinu posouzení pomocí mapování závislostí skupin ve službě Azure Migrate | Dokumentace Microsoftu
description: Popisuje, jak Upřesnit hodnocení využitím mapování závislostí skupiny ve službě Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 37c4ce8638c8f0481151449317d6cd387b61b256
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622894"
---
# <a name="refine-a-group-using-group-dependency-mapping"></a>Upřesnění skupiny s využitím mapování závislostí skupin

Tento článek popisuje, jak upřesnění skupiny s vizualizací závislostí všech počítačů ve skupině. Obvykle použijete tuto metodu, když budete chtít Upřesnit členství pro stávající skupinu, kontrola křížové skupiny závislostmi, před spuštěním posouzení. Upřesnění skupiny pomocí vizualizace závislostí může usnadní vám to efektivně naplánovat vaši migraci do Azure.You můžete zjišťovat všechny vzájemně závislých systémů, které je potřeba migrovat společně. To vám pomůže zajistit, že nic se zachovají a překvapením, že není dojde k výpadku při migraci do Azure. 


> [!NOTE]
> Skupiny, pro které chcete vizualizace závislostí nesmí obsahovat více než 10 počítačů. Pokud máte více než 10 počítačů ve skupině, doporučujeme ho rozdělte do menších skupin využívat funkce vizualizace závislostí.


# <a name="prepare-the-group-for-dependency-visualization"></a>Příprava skupině vizualizace závislostí
Chcete-li zobrazit závislosti skupiny, budete muset stáhnout a nainstalovat agenty na každém v místním počítači, který je součástí skupiny. Kromě toho, pokud máte počítače bez připojení k Internetu, musíte stáhnout a nainstalovat [bránu OMS](../log-analytics/log-analytics-oms-gateway.md) na ně.

### <a name="download-and-install-the-vm-agents"></a>Stažení a instalace agentů virtuálního počítače
1. V **přehled**, klikněte na tlačítko **spravovat** > **skupiny**, přejděte do požadované skupiny.
2. V seznamu počítačů v **agenta závislostí** sloupce, klikněte na tlačítko **vyžaduje instalaci** zobrazíte pokyny ohledně toho, jak stáhnout a nainstalovat agenty.
3. Na **závislosti** stránce, stáhněte a nainstalujte Microsoft Monitoring Agent (MMA) a agenta závislostí na každém virtuálním počítači, který je součástí skupiny.
4. Zkopírujte ID a klíč pracovního prostoru. Budete je potřebovat při instalaci agenta MMA na místních počítačích.

### <a name="install-the-mma"></a>Instalace agenta MMA

Instalace agenta na počítači s Windows:

1. Dvakrát klikněte na staženého agenta.
2. Na **úvodní** stránce klikněte na **Další**. Na stránce **Licenční podmínky** kliknutím na **Souhlasím** přijměte licenci.
3. V **cílovou složku**, udržovat nebo změnit výchozí instalační složku > **Další**. 
4. V **možnosti instalace agenta**vyberte **Azure Log Analytics** > **Další**. 
5. Klikněte na tlačítko **přidat** přidáte nový pracovní prostor Log Analytics. Vložte ID pracovního prostoru a klíč, který jste zkopírovali z portálu. Klikněte na **Další**.


Instalace agenta na počítači s Linuxem:

1. Přeneste příslušný balíček (x86 nebo x64) na počítači s Linuxem pomocí příkazu scp/sftp.
2. Instalaci sady pomocí argumentu--install.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```


### <a name="install-the-dependency-agent"></a>Instalace agenta závislostí
1. Instalace agenta závislostí na počítači s Windows, klikněte dvakrát na instalační soubor a postupujte podle pokynů průvodce.
2. Pokud chcete nainstalovat agenta závislostí na počítači s Linuxem, nainstalujte jako uživatel root pomocí následujícího příkazu:

    ```sh InstallDependencyAgent-Linux64.bin```

Další informace o podpoře agenta závislostí [Windows](../monitoring/monitoring-service-map-configure.md#supported-windows-operating-systems) a [Linux](../monitoring/monitoring-service-map-configure.md#supported-linux-operating-systems) operačních systémů.

## <a name="refine-the-group-based-on-dependency-visualization"></a>Upřesnění skupiny založené na vizualizace závislostí
Po instalaci agentů na všech počítačích skupiny můžete vizualizace závislostí skupiny a zpřesnit jej pomocí následujícího níže uvedených pokynů.

1. Ve službě Azure Migrate projektu, v části **spravovat**, klikněte na tlačítko **skupiny**a vyberte skupinu.
2. Na stránce skupiny, klikněte na tlačítko **zobrazení závislostí**, chcete-li spustit nástroj Mapa závislostí skupiny.
3. Mapa závislostí pro skupinu zobrazí následující podrobnosti:
    - Odchozí připojení (servery) TCP do a ze všech počítačů, které jsou součástí skupiny a příchozí (klientů)
        - Závislé počítače, které nemají nainstalovaného agenta MMA a závislosti jsou seskupené podle čísla portů
        - Dependenct počítače, které mají agenta MMA a instalaci agenta závislosti jsou uvedeny v samostatných polích. 
    - Spuštěné procesy v rámci počítače, můžete rozbalit každé pole počítače zobrazit procesy
    - Vlastnosti, jako je plně kvalifikovaný název domény, operačního systému, atd. adresu MAC každého počítače, můžete kliknout na každé pole počítače zobrazit tyto podrobnosti

     ![Zobrazení skupinových závislostí](./media/how-to-create-group-dependencies/view-group-dependencies.png)

3. Chcete-li zobrazit podrobnější závislosti, klikněte na tlačítko časový rozsah jej upravit. Ve výchozím nastavení rozsah je jedna hodina. Můžete upravit časový rozsah, nebo zadat počáteční a koncové datum a dobu trvání.
4. Ověření závislých počítačů, proces spuštěný v každém počítači a identifikaci počítačů, které mají přidat nebo odebrat ze skupiny.
5. Vyberte počítače, na mapě, které chcete přidat nebo odebrat ze skupiny pomocí kombinace kláves Ctrl + kliknutí.
    - Mohli byste přidávat pouze počítače, které byly zjištěny.
    - Přidání a odebrání počítače ze skupiny zruší platnost po posouzení pro něj.
    - Volitelně můžete vytvořit nové posouzení při úpravě skupině.
5. Klikněte na tlačítko **OK** uložte skupinu.

    ![Přidání nebo odebrání počítačů](./media/how-to-create-group-dependencies/add-remove.png)

Pokud chcete zkontrolovat závislosti pro konkrétní počítač, který se zobrazí na mapě závislostí skupiny [nastavit mapování závislosti počítačů](how-to-create-group-machine-dependencies.md).


## <a name="next-steps"></a>Další postup

[Přečtěte si další informace](concepts-assessment-calculation.md) o tom, jak se v rámci posouzení počítají náklady.
