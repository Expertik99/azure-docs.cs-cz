---
title: Řešení potíží s Azure File Sync | Dokumentace Microsoftu
description: Řešení běžných potíží s Azure File Sync.
services: storage
author: jeffpatt24
ms.service: storage
ms.topic: article
ms.date: 09/06/2018
ms.author: jeffpatt
ms.component: files
ms.openlocfilehash: 88c73b3c9fd3ffc0c323b9971e245e6f6d9695a0
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44095534"
---
# <a name="troubleshoot-azure-file-sync"></a>Řešení problémů se Synchronizací souborů Azure
Azure File Sync umožňuje centralizovat sdílené složky organizace ve službě soubory Azure, při zachování flexibility, výkonu a kompatibility s místními souborového serveru. Azure File Sync transformuje serveru systému Windows na rychlou mezipaměť sdílené složky Azure. Můžete použít jakýkoli protokol dostupný ve Windows serveru pro přístup k datům místně, včetně SMB, NFS a FTPS. Můžete mít libovolný počet mezipamětí po celém světě potřebujete.

Tento článek je určen můžete odstraňovat potíže a řešit problémy, které se můžete setkat s nasazením Azure File Sync. Také zjistíte, jak shromažďovat důležité protokoly ze systému, pokud se vyžaduje hlubší zkoumání problému. Pokud nevidíte odpověď na svoji otázku, kontaktujte nás prostřednictvím následujících kanálů (v neustále rostoucích pořadí):

1. [Fórum služby Azure Storage](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).
2. [Azure Files UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files).
3. podporu Microsoftu. Chcete-li vytvořit novou žádost o podporu, na webu Azure Portal, na **pomáhají** kartu, vyberte možnost **Nápověda a podpora** tlačítko a pak vyberte **nová žádost o podporu**.

## <a name="im-having-an-issue-with-azure-file-sync-on-my-server-sync-cloud-tiering-etc-should-i-remove-and-recreate-my-server-endpoint"></a>Mám potíže s Azure File Sync na serveru (synchronizace, cloud vrstvení atd.). By měla odebrat a znovu vytvořte koncový bod pro tento server?
[!INCLUDE [storage-sync-files-remove-server-endpoint](../../../includes/storage-sync-files-remove-server-endpoint.md)]

## <a name="agent-installation-and-server-registration"></a>Instalace a server registrace agenta
<a id="agent-installation-failures"></a>**Řešení potíží se selháním instalace agenta**  
V případě selhání instalace agenta Azure File Sync v příkazovém řádku se zvýšenými oprávněními spusťte následující příkaz, který zapnout protokolování během instalace agenta:

```
StorageSyncAgent.msi /l*v AFSInstaller.log
```

Projděte si installer.log a zjistěte příčinu selhání instalace.

<a id="agent-installation-on-DC"></a>**Instalace agenta nepodaří na řadič domény služby Active Directory**  
Pokud se pokusíte nainstalovat agenta synchronizace na řadič domény služby Active Directory, kde je vlastníkem této role primárního řadiče domény na Windows Server 2008 R2 nebo nižší než verze operačního systému, můžete narazit na problém, kde se nepodaří nainstalovat agenta synchronizace.

Pokud chcete vyřešit, přenos role primárního řadiče domény do jiné domény kontroleru spuštěný Windows Server 2012R2 nebo novější, potom nainstalovat synchronizace.

<a id="server-registration-missing"></a>**Server není uveden v části registrované servery na portálu Azure portal**  
Pokud server není uvedený v seznamu **registrované servery** pro službu synchronizace úložiště:
1. Přihlaste se k serveru, který chcete zaregistrovat.
2. Otevřete Průzkumníka souborů a potom přejděte na instalační adresář agenta synchronizace úložiště (výchozí umístění je C:\Program Files\Azure\StorageSyncAgent). 
3. Spusťte ServerRegistration.exe a dokončete průvodce a registrace serveru u služby synchronizace úložiště.

<a id="server-already-registered"></a>**Registrace serveru během instalace agenta Azure File Sync se zobrazí následující zpráva: "Tento server je už zaregistrovaný"** 

![Snímek obrazovky registrace serveru dialogové okno s chybou "serveru je již registrováno" zpráva](media/storage-sync-files-troubleshoot/server-registration-1.png)

Tato zpráva se zobrazí, pokud server byl dříve zaregistrován u služby synchronizace úložiště. Pokud chcete zrušit registraci serveru aktuální služby synchronizace úložiště a potom zaregistrovat nové služby synchronizace úložiště, proveďte kroky, které jsou popsány v [zrušení registrace serveru pomocí služby Azure File Sync](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

Pokud server není uveden v části **registrované servery** v služba synchronizace úložiště na serveru, na kterém chcete zrušit registraci, spusťte následující příkazy Powershellu:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Reset-StorageSyncServer
```

> [!Note]  
> Pokud je server součástí clusteru, můžete použít volitelný *Reset StorageSyncServer - CleanClusterRegistration* parametr také odebrat registraci clusteru.

<a id="web-site-not-trusted"></a>**Mohu zaregistrovat server, zobrazila mnoho odpovědí "webový server není důvěryhodný.". Proč?**  
Tomuto problému dochází, když **rozšířeného zabezpečení aplikace Internet Explorer** zásady jsou povolené při registraci serveru. Další informace o tom, jak správně zakázat **rozšířeného zabezpečení aplikace Internet Explorer** zásady, najdete v článku [připravit systém Windows Server pro použití s Azure File Sync](storage-sync-files-deployment-guide.md#prepare-windows-server-to-use-with-azure-file-sync) a [jak nasadit Azure File Synchronizace](storage-sync-files-deployment-guide.md).

## <a name="sync-group-management"></a>Správa skupin synchronizace
<a id="cloud-endpoint-using-share"></a>**Vytvoření koncového bodu cloudu selže s touto chybou: "Zadaná sdílená složka Azure je již používán jinou CloudEndpoint"**  
K tomuto problému dochází, pokud sdílená složka Azure se už používá jiný koncový bod cloudu. 

Pokud se zobrazí tato zpráva a sdílené složky Azure aktuálně není používán koncového bodu cloudu, proveďte následující kroky zrušte Azure File Sync metadat na sdílené složky Azure:

> [!Warning]  
> Odstranění metadat na sdílenou složku Azure, který je aktuálně používán koncového bodu cloudu způsobí selhání operací Azure File Sync. 

1. Na webu Azure Portal přejděte do sdílené složky Azure.  
2. Klikněte pravým tlačítkem na sdílenou složku Azure a pak vyberte **upravit metadata**.
3. Klikněte pravým tlačítkem na **SyncService**a pak vyberte **odstranit**.

<a id="cloud-endpoint-authfailed"></a>**Vytvoření koncového bodu cloudu selže s touto chybou: "AuthorizationFailed"**  
K tomuto problému dochází, pokud váš uživatelský účet nemá dostatečná práva k vytvoření koncového bodu cloudu. 

Vytvoření koncového bodu cloudu, váš uživatelský účet musí mít následující oprávnění Authorization společnosti Microsoft:  
* Čtení: Získat definice role
* Zápis: Vytvořit nebo aktualizovat vlastní definici role
* Čtení: Získat přiřazení role
* Zápis: Vytvořit přiřazení role

Následující předdefinované role mají všechna oprávnění vyžadovaná Authorization společnosti Microsoft:  
* Vlastník
* Správce přístupu uživatelů

Chcete-li zjistit, zda vaše uživatelská role účet má potřebná oprávnění:  
1. Na webu Azure Portal, vyberte **skupiny prostředků**.
2. Vyberte skupinu prostředků, které je umístěný účet úložiště a pak vyberte **řízení přístupu (IAM)**.
3. Vyberte **role** (například vlastníka nebo přispěvatele) pro váš uživatelský účet.
4. V **poskytovatele prostředků** seznamu vyberte **Microsoft Authorization**. 
    * **Přiřazení role** by měl mít **čtení** a **zápisu** oprávnění.
    * **Definice role** by měl mít **čtení** a **zápisu** oprávnění.

<a id="server-endpoint-createjobfailed"></a>**Vytvoření koncového bodu serveru selže s touto chybou: "MgmtServerJobFailed" (kód chyby:-2134375898)**  
K tomuto problému dochází, pokud je cesta na koncový bod serveru na systémovém svazku a cloud ovládání datových vrstev je povolená. Cloud nepodporuje ovládání datových vrstev na systémovém svazku. Pokud chcete vytvořit koncový bod serveru na systémovém svazku, zakažte vrstvení cloudu po vytvoření koncového bodu serveru.

<a id="server-endpoint-deletejobexpired"></a>**Odstraňuje se koncový bod serveru selže s touto chybou: "MgmtServerJobExpired"**                
K tomuto problému dochází, pokud se server je offline nebo nemá připojení k síti. Pokud již není k dispozici na serveru, zrušení registrace serveru na portálu, který se odstraní koncové body serveru. Pokud chcete odstranit koncové body serveru, postupujte podle kroků, které jsou popsány v [zrušení registrace serveru pomocí služby Azure File Sync](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

<a id="server-endpoint-provisioningfailed"></a>**Nelze otevřít stránku Vlastnosti koncového bodu serveru nebo aktualizujete zásady pro vrstvení cloudu**  
Tomuto problému může dojít v případě selhání operace správy na koncovém bodu serveru. Pokud na stránce Vlastnosti koncového bodu serveru nelze otevřít na webu Azure Portal, aktualizuje se koncový bod serveru pomocí příkazů Powershellu ze serveru může tento problém vyřešit. 

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"
# Get the server endpoint id based on the server endpoint DisplayName property
Get-AzureRmStorageSyncServerEndpoint `
    -SubscriptionId mysubguid `
    -ResourceGroupName myrgname `
    -StorageSyncServiceName storagesvcname `
    -SyncGroupName mysyncgroup

# Update the free space percent policy for the server endpoint
Set-AzureRmStorageSyncServerEndpoint `
    -Id serverendpointid `
    -CloudTiering true `
    -VolumeFreeSpacePercent 60
```
<a id="server-endpoint-noactivity"></a>**Koncový bod serveru je ve stavu stavu "Žádná aktivita" nebo "Čeká na vyřízení" a stavu serveru v okně registrované servery je "Se zobrazí v režimu offline"**  

Tomuto problému může dojít, pokud proces synchronizace monitorování úložiště není spuštěna nebo není schopen komunikovat se službou Azure File Sync kvůli proxy nebo brány firewall na serveru.

Chcete-li vyřešit tento problém, proveďte následující kroky:

1. Otevřete Správce úloh na serveru a ověřte, zda že je spuštěn proces synchronizace monitorování úložiště (AzureStorageSyncMonitor.exe). Pokud proces není spuštěn, nejprve zkuste restartovat server. Pokud restartování serveru problém nevyřeší, odinstalujte a znovu nainstalujte agenta Azure File Sync (Poznámka: nastavení serveru se zachovají i po odinstalování a přeinstalování agenta).
2. Ověřte, zda je správně nakonfigurované nastavení brány Firewall a proxy serveru:
    - Pokud je server za bránou firewall, ověřte, že je povolené odchozím portu 443. Pokud brána firewall omezuje provoz na konkrétní domény, zkontrolujte domén uvedené v bráně Firewall [dokumentaci](https://docs.microsoft.com/en-us/azure/storage/files/storage-sync-files-firewall-and-proxy#firewall) jsou k dispozici.
    - Pokud je za proxy server, nakonfigurujte nastavení proxy celý počítač nebo konkrétní aplikace podle postupu v proxy serveru [dokumentaci](https://docs.microsoft.com/en-us/azure/storage/files/storage-sync-files-firewall-and-proxy#proxy).

## <a name="sync"></a>Sync
<a id="afs-change-detection"></a>**Když jsem vytvořil soubor přímo v mé sdílené složky Azure přes protokol SMB nebo prostřednictvím portálu, jak dlouho trvá synchronizaci pro servery ve skupině synchronizace souboru?**  
[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="serverendpoint-pending"></a>**Stav koncového bodu serveru je ve stavu čekání po dobu několika hodin**  
Tento problém se očekává, je-li vytvořit koncový bod cloudu a používat sdílené složky Azure, který obsahuje data. Mezi koncové body cloudu a serveru můžou synchronizovat soubory musíte dokončit úlohu výčtu změn, která hledá změny ve sdílené složce Azure file. Čas k dokončení úlohy se závisí na velikosti oboru názvů do sdílené složky Azure. Po dokončení úlohy výčtu změn by měl aktualizovat stav koncového bodu serveru.

### <a id="broken-sync"></a>Jak můžu monitorovat stav synchronizace?
# <a name="portaltabportal1"></a>[Azure Portal](#tab/portal1)
V rámci jednotlivých skupin synchronizace můžete procházet hierarchii do jeho koncových bodů jednotlivých serverů a zjistit stav poslední relace dokončení synchronizace. Zelená sloupce stavu a soubory nesynchronizuje hodnoty 0 označují, že synchronizace funguje podle očekávání. Pokud to není tento případ, níže naleznete seznam běžných chyb synchronizace a ke zpracování souborů, která se synchronizují. 

![Snímek obrazovky webu Azure portal](media/storage-sync-files-troubleshoot/portal-sync-health.png)

# <a name="servertabserver"></a>[Server](#tab/server)
Přejděte na protokoly telemetrických dat serveru, které můžete najít v události prohlížeč na `Applications and Services Logs\Microsoft\FileSync\Agent\Telemetry`. Událost 9102 odpovídá dokončené synchronizační relaci. Vyhledejte nejnovější událost s ID 9102 pro nejnovější stav synchronizace. SyncDirection informuje, pokud tuto relaci bylo odeslání nebo stažení. Pokud je hodnota HResult 0, relace synchronizace byla úspěšná. Nenulové HResult znamená, že během synchronizace; došlo k chybě Níže naleznete seznam běžných chyb. Pokud PerItemErrorCount je větší než 0, znamená to, že některé soubory nebo složky nesynchronizovaly správně. Je možné mít HResult 0 ale PerItemErrorCount, který je větší než 0.

Níže je příklad úspěšně nahrávaly. Pro účely jako stručný výtah jenom některé z hodnoty obsažené v každé 9102 události jsou uvedeny níže. 

```
Replica Sync session completed.
SyncDirection: Upload,
HResult: 0, 
SyncFileCount: 2, SyncDirectoryCount: 0,
AppliedFileCount: 2, AppliedDirCount: 0, AppliedTombstoneCount 0, AppliedSizeBytes: 0.
PerItemErrorCount: 0,
TransferredFiles: 2, TransferredBytes: 0, FailedToTransferFiles: 0, FailedToTransferBytes: 0.
```

Naopak neúspěšné nahrávání může vypadat takto:

```
Replica Sync session completed.
SyncDirection: Upload,
HResult: -2134364065,
SyncFileCount: 0, SyncDirectoryCount: 0, 
AppliedFileCount: 0, AppliedDirCount: 0, AppliedTombstoneCount 0, AppliedSizeBytes: 0.
PerItemErrorCount: 0, 
TransferredFiles: 0, TransferredBytes: 0, FailedToTransferFiles: 0, FailedToTransferBytes: 0.
```

Někdy relace synchronizace nezdaří, celkový nebo mít nenulovou PerItemErrorCount ale stále provádět postup směrem vpřed, se některé soubory úspěšně synchronizovat. To můžete vidět použito * polí (AppliedFileCount AppliedDirCount, AppliedTombstoneCount a AppliedSizeBytes), které zjistíte, jak velká část relace je úspěšné. Pokud se zobrazí více relací synchronizace za sebou, které se nedaří, ale mají rostoucí počet použito *, je třeba přiřadit čas synchronizace pokusit znovu před otevřením lístku podpory.

---

### <a name="how-do-i-monitor-the-progress-of-a-current-sync-session"></a>Jak můžu monitorovat průběh aktuální relace synchronizace?
# <a name="portaltabportal1"></a>[Azure Portal](#tab/portal1)
V rámci skupiny pro synchronizaci přejděte na koncový bod serveru dotyčný a podívejte se na část aktivity synchronizace a zobrazit počet soubory nahrávat nebo stahovat v aktuální relaci synchronizace. Všimněte si, že tento stav bude zpozdit o přibližně 5 minut, a pokud se vaše relace synchronizace je dostatečně malá, aby dokončit během tohoto období, nemusí být hlášena na portálu. 

# <a name="servertabserver"></a>[Server](#tab/server)
Hledejte na poslední 9302 v telemetrická data do protokolu událostí na serveru (v prohlížeči událostí přejděte k aplikacím a službám Logs\Microsoft\FileSync\Agent\Telemetry). Tato událost ukazuje na stavu relace synchronizace. TotalItemCount označuje, kolik souborů je možné synchronizovat AppliedItemCount počet souborů, které byly synchronizovány zatím a PerItemErrorCount počet souborů, které se nedaří synchronizace (viz níže jak zacházet s tímto).

```
Replica Sync Progress. 
ServerEndpointName: <CI>sename</CI>, SyncGroupName: <CI>sgname</CI>, ReplicaName: <CI>rname</CI>, 
SyncDirection: Upload, CorrelationId: {AB4BA07D-5B5C-461D-AAE6-4ED724762B65}. 
AppliedItemCount: 172473, TotalItemCount: 624196. AppliedBytes: 51473711577, 
TotalBytes: 293363829906. 
AreTotalCountsFinal: true. 
PerItemErrorCount: 1006.
```
---

### <a name="how-do-i-know-if-my-servers-are-in-sync-with-each-other"></a>Jak poznám, že pokud jsou servery mezi sebou synchronizované?
# <a name="portaltabportal1"></a>[Azure Portal](#tab/portal1)
Pro každý server ve skupině dané synchronizace Ujistěte se, že:
- Jsou poslední časové razítko pro poslední pokus o synchronizaci pro nahrávání a stahování.
- Stav je zelená pro nahrávání a stahování.
- Synchronizace pole zobrazuje velmi málo nebo žádná zbývajících pro synchronizaci souborů.
- Pole souborů nesynchronizuje hodnotu 0 pro nahrávání a stahování.

# <a name="servertabserver"></a>[Server](#tab/server)
Podívejte se na dokončení synchronizace relací, které jsou označené nástrojem 9102 události v protokolu událostí telemetrie pro každý server (v prohlížeči událostí, přejděte na `Applications and Services Logs\Microsoft\FileSync\Agent\Telemetry`). 

1. Na daném serveru budete chtít Ujistěte se, že nejnovější nahrávání a stahování relace byla úspěšně dokončena. Chcete-li to provést, zkontrolujte, zda HResult a PerItemErrorCount 0 pro nahrávání a stahování (SyncDirection pole určuje, zda je dané relace nahrávání a stahování relace). Všimněte si, že pokud nevidíte relaci synchronizace nedávno dokončené, je pravděpodobné, že relace synchronizace je aktuálně v průběhu, který se dá očekávat, pokud právě přidá nebo upraví velké množství dat.
2. Když server je plně aktuální s cloudem a nemá žádné změny k synchronizaci v obou směrech, zobrazí se prázdný synchronizační relace. Tyto jsou označeny nahrání a stažení událostí, ve které všechny synchronizaci * pole (SyncFileCount SyncDirCount, SyncTombstoneCount a SyncSizeBytes) jsou nula, což znamená, že se nic k synchronizaci. Všimněte si, že tyto relace synchronizace prázdný nedojde na serverech s vysokou četností změn, jako je vždycky něco nového synchronizace. Pokud není žádná synchronizační aktivita odehrávalo každých 30 minut. 
3. Pokud všechny servery jsou aktuální s cloudem, což znamená jejich poslední nahrávání a stahování relace jsou relace synchronizace prázdný, můžete napsat s dostatečnou jistotou, že systém jako celek se synchronizuje. 
    
Všimněte si, že pokud jste provedli změny přímo do sdílené složky Azure, Azure File Sync nerozpozná tuto změnu do spuštění výčtu změn, která se stane každých 24 hodin. Je možné, že server bude sdělení, že se že jedná aktuální s cloudem, když ve skutečnosti neobsahuje nejnovější změny provedené přímo do sdílené složky Azure. 

---

### <a name="how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing"></a>Jak můžu zobrazit, pokud existují určité soubory nebo složky, která se synchronizují?
Pokud vaše PerItemErrorCount na serveru nebo soubory nesynchronizuje počet na portálu jsou větší než 0 pro všechny relace dané synchronizace, to znamená, že některé položky se nedaří synchronizovat. Soubory a složky může mít vlastnosti, které jim zabránit v synchronizaci. Tyto charakteristiky může být trvalý a vyžaduje explicitní akci obnovíte synchronizaci, například odebere nepodporované znaky z názvu souboru nebo složky. Také může být přechodná, což znamená, soubor nebo složku se automaticky obnoví synchronizace; například soubory s otevřenými popisovači se automaticky obnoví synchronizace při zavření souboru. Když modul Azure File Sync zjistí tento problém, je vytvořen protokol chyb, který může být analyzován na seznam položek aktuálně nesynchronizuje správně.

Pokud chcete zobrazit tyto chyby, spusťte **FileSyncErrorsReport.ps1** skript prostředí PowerShell (umístěný v instalační adresář agenta agenta Azure File Sync) identifikovat soubory, které se nepovedlo synchronizovat z důvodu otevřených popisovačů, nepodporované znaky, nebo jiné problémy. Položka ItemPath pole obsahuje umístění souboru ve vztahu k do kořenového adresáře synchronizace. Zobrazit seznam běžných chyb synchronizace níže k nápravným krokům.

#### <a name="troubleshooting-per-filedirectory-sync-errors"></a>Řešení potíží s za chyby synchronizace souborů či složek
**Protokol ItemResults – chyby za položek synchronizace**  
| HODNOTA HRESULT | HRESULT (decimální) | Text chyby | Problém | Náprava |
|---------|-------------------|--------------|-------|-------------|
| 0x80c80065 | -2134376347 | ECS_E_DATA_TRANSFER_BLOCKED | Soubor má došlo k trvalé chybě při synchronizaci a proto bude pouze proveden pokus o synchronizaci jednou za den. Základní chyba najdete v dřívějších protokolu událostí. | V produktu agents R2 (2.0) a výše, se zobrazí původní chyby spíše než ten. Upgradujte na nejnovější verzi agenta zobrazí podkladová chyba nebo podívejte se na starší protokoly událostí k nalezení příčiny původní chyby. |
| 0x7B | 123 | ERROR_INVALID_NAME | Název souboru nebo adresáře je neplatný. | Přejmenujte soubor nebo adresář nejistá. Zobrazit [pokyny pro pojmenování souborů Azure](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names) a ze seznamu níže nepodporované znaky. |
| 0x8007007b | -2147024773 | STIERR_INVALID_DEVICE_NAME | Název souboru nebo adresáře je neplatný. | Přejmenujte soubor nebo adresář nejistá. Zobrazit [pokyny pro pojmenování souborů Azure](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata#directory-and-file-names) a ze seznamu níže nepodporované znaky. |
| 0x80c8031d | -2134375651 | ECS_E_CONCURRENCY_CHECK_FAILED | Soubor byl změněn, ale změny ještě nezjistil synchronizace. Synchronizace se obnoví po této změně se detekuje. | Není vyžadována žádná akce. |
| 0x80c80018 | -2134376424 | ECS_E_SYNC_FILE_IN_USE | Soubor nelze synchronizovat, protože je používán. Soubor bude synchronizován, až se už používá. | Není vyžadována žádná akce. Azure File Sync vytvoří dočasné snímek služby VSS jednou za den na serveru, aby synchronizovat soubory, které mají otevřených popisovačů. |
| 0x20 | 32 | ERROR_SHARING_VIOLATION | Soubor nelze synchronizovat, protože je používán. Soubor bude synchronizován, až se už používá. | Není vyžadována žádná akce. |
| 0x80c80207 | -2134375929 | ECS_E_SYNC_CONSTRAINT_CONFLICT | Změnu souboru nebo adresáře nejde zatím synchronizovat, protože ještě není synchronizovaná závislá složka. Tato položka se synchronizuje po závislých změn. | Není vyžadována žádná akce. |
| 0x80c80017 | -2134376425 | ECS_E_SYNC_OPLOCK_BROKEN | Nějaký soubor se během synchronizace změnil, takže je nutné ho synchronizovat znovu. | Není vyžadována žádná akce. |

#### <a name="handling-unsupported-characters"></a>Zpracování nepodporované znaky.
Pokud **FileSyncErrorsReport.ps1** skript prostředí PowerShell ukazuje selhání kvůli nepodporované znaky (kódy chyb 0x7b a 0x8007007b), odstranit nebo přejmenovat znaky na selhání z příslušných souborů. Prostředí PowerShell pravděpodobně vytiskne tyto znaky jako otazník nebo prázdný obdélníky, protože většina z těchto znaků mít žádné standardní vizuálního kódování.

Následující tabulka obsahuje všechny znaky unicode, které Azure File Sync zatím nepodporuje.

| Znaková sada | Počet znaků |
|---------------|-----------------|
| <ul><li>0x0000009D (příkaz osc operačního systému)</li><li>0x00000090 (řetězec řízení zařízení řadiče domény)</li><li>0x0000008F (jeden shift ss3 tři)</li><li>0x00000081 (vysoká octet předvolbu)</li><li>0x0000007F (odstraňte del)</li><li>0x0000008D (ri reverzní řádek kanál)</li></ul> | 6 |
| <ul><li>0x0000200F (Značka zprava doleva)</li><li>0x0000200E (značka zleva doprava)</li><li>0x0000202E (přepsání zprava doleva)</li><li>0x0000202D (přepsání zleva doprava)</li><li>0x0000202C (pop směrové formátování)</li><li>0x0000202B (vkládání zprava doleva)</li><li>0x0000202A (zleva doprava vložení)</li></ul> | 7 |
| 0x0000FDD0 - 0x0000FDEF (arabštiny forms-a) | 32 |
| 0x0000FFF0 - 0x0000FFFF (nabídky) | 16 |
| <ul><li>0x0001FFFE - 0x0001FFFF = 2 (neznakové)</li><li>0x0002FFFE - 0x0002FFFF = 2 (neznakové)</li><li>0x0003FFFE - 0x0003FFFF = 2 (neznakové)</li><li>0x0004FFFE - 0x0004FFFF = 2 (neznakové)</li><li>0x0005FFFE - 0x0005FFFF = 2 (neznakové)</li><li>0x0006FFFE - 0x0006FFFF = 2 (neznakové)</li><li>0x0007FFFE - 0x0007FFFF = 2 (neznakové)</li><li>0x0008FFFE - 0x0008FFFF = 2 (neznakové)</li><li>0x0009FFFE - 0x0009FFFF = 2 (neznakové)</li><li>0x000AFFFE - 0x000AFFFF = 2 (neznakové)</li><li>0x000BFFFE - 0x000BFFFF = 2 (neznakové)</li><li>0x000CFFFE - 0x000CFFFF = 2 (neznakové)</li><li>0x000DFFFE - 0x000DFFFF = 2 (neznakové)</li><li>0x000EFFFE - 0x000EFFFF = 2 (Nedefinováno)</li><li>0x000FFFFE - 0x000FFFFF = 2 (doplňující soukromé použití oblast)</li></ul> | 30 |
| 0x0010FFFE, 0x0010FFFF | 2 |

### <a name="common-sync-errors"></a>Běžné chyby synchronizace
<a id="-2147023673"></a>**Relace synchronizace byla zrušena.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x800704c7 |
| **HRESULT (decimální)** | -2147023673 | 
| **Text chyby** | ERROR_CANCELLED |
| **Požadována náprava** | Ne |

Relace synchronizace mohou selhat z různých důvodů, včetně server se restartuje nebo aktualizaci snímků služby VSS, atd. I když se tato chyba pravděpodobně vyžaduje následnou akci, je bezpečné tuto chybu ignorovat, pokud se opakuje po dobu několika hodin.

<a id="-2147012889"></a>**Nelze navázat připojení ke službě.**    
| | |
|-|-|
| **HODNOTA HRESULT** | 0x80072EE7 |
| **HRESULT (decimální)** | -2147012889 | 
| **Text chyby** | WININET_E_NAME_NOT_RESOLVED |
| **Požadována náprava** | Ano |

[!INCLUDE [storage-sync-files-bad-connection](../../../includes/storage-sync-files-bad-connection.md)]

<a id="-2134376372"></a>**Požadavek uživatele byla omezena systémem služby.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x80c8004c |
| **HRESULT (decimální)** | -2134376372 |
| **Text chyby** | ECS_E_USER_REQUEST_THROTTLED |
| **Požadována náprava** | Ne |

Nemusíte nic dělat; server to zkusí znovu. Pokud tento problém potrvá déle než několik hodin, vytvořte žádost o podporu.

<a id="-2134364065"></a>**Synchronizaci nelze přistoupit, sdílené složky Azure zadaný koncový bod cloudu.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x80c8305f |
| **HRESULT (decimální)** | -2134364065 |
| **Text chyby** | ECS_E_CANNOT_ACCESS_EXTERNAL_STORAGE_ACCOUNT |
| **Požadována náprava** | Ano |

K této chybě dochází, protože agenta Azure File Sync nemá přístup ke sdílené složky Azure, který může být, že sdílené složky Azure nebo účtu úložiště hostování už existuje. Odstranění této chyby z práce prostřednictvím následujících kroků:

1. [Ověřte, že účet úložiště existuje.](#troubleshoot-storage-account)
2. [Zkontrolujte, ujistěte se, že účet úložiště neobsahuje žádná pravidla sítě.](#troubleshoot-network-rules)
3. [Ujistěte se, že existuje sdílená složka Azure.](#troubleshoot-azure-file-share)
4. [Ujistěte se, že má přístup k účtu úložiště Azure File Sync.](#troubleshoot-rbac)

<a id="-2134364064"></a><a id="cannot-resolve-storage"></a>**Nelze přeložit název účtu úložiště.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x80C83060 |
| **HRESULT (decimální)** | -2134364064 |
| **Text chyby** | ECS_E_STORAGE_ACCOUNT_NAME_UNRESOLVED |
| **Požadována náprava** | Ano |

1. Zkontrolujte, zda lze přeložit název DNS úložiště ze serveru.

    ```PowerShell
    Test-NetConnection -ComputerName <storage-account-name>.file.core.windows.net -Port 443
    ```
2. [Ověřte, že účet úložiště existuje.](#troubleshoot-storage-account)
3. [Zkontrolujte, ujistěte se, že účet úložiště neobsahuje žádná pravidla sítě.](#troubleshoot-network-rules)

<a id="-1906441138"></a>**Synchronizace se nezdařila z důvodu problému s databází synchronizace.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x8e5e044e |
| **HRESULT (decimální)** | -1906441138 |
| **Text chyby** | JET_errWriteConflict |
| **Požadována náprava** | Ano |

Tato chyba nastane, pokud dojde k nějakému problému s interní databází používá Azure File Sync. Pokud k tomuto problému dochází, vytvořit žádost o podporu a pošleme vám pomůžou při řešení tohoto problému.

<a id="-2134351810"></a>**Dosáhli jste limitu úložiště sdílené složky Azure file.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x80c8603e |
| **HRESULT (decimální)** | -2134351810 |
| **Text chyby** | ECS_E_AZURE_STORAGE_SHARE_SIZE_LIMIT_REACHED |
| **Požadována náprava** | Ano |

K této chybě dochází, když limit úložiště sdílené složky Azure file se dosáhne, které může dojít, pokud se použije kvótu pro sdílenou složku Azure, nebo pokud využití překročí limity pro sdílené složky Azure. Další informace najdete v tématu [současných limitů pro sdílené složky Azure](storage-files-scale-targets.md).

1. Přejděte do skupiny synchronizace v rámci služby synchronizace úložiště.
2. Vyberte koncový bod cloudu v rámci skupiny synchronizace.
3. Poznamenejte si název sdílené složky Azure file v podokně otevřené.
4. Vyberte propojeném účtu úložiště. Pokud se tento odkaz se nepodaří, účet úložiště odkazované se odebrala.

    ![Snímek obrazovky ukazující na panelu informací o koncový bod cloudu s odkazem na účet úložiště.](media/storage-sync-files-troubleshoot/file-share-inaccessible-1.png)

5. Vyberte **soubory** Chcete-li zobrazit seznam sdílených složek.
6. Klikněte na tři tečky na konci řádku pro sdílené složky Azure odkazovat na koncový bod cloudu.
7. Ověřte, že **využití** je nižší než **kvóty**. Poznámka: alternativní kvóty byla určena, bude odpovídat kvóty [maximální velikost sdílené složky Azure](storage-files-scale-targets.md).

    ![Snímek obrazovky vlastností sdílené složky Azure file.](media/storage-sync-files-troubleshoot/file-share-limit-reached-1.png)

Pokud sdílená složka je plná a kvóty není nastavená, je jeden možný způsob, jak tento problém vyřešíme, aby všechny podsložky v aktuální koncový bod serveru na vlastní koncový bod serveru v samostatné synchronizační skupin. Tímto způsobem bude každá podsložka synchronizovat do jednotlivých sdílených složek Azure.

<a id="-2134351824"></a>**Sdílená složka Azure se nenašel.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x80c86030 |
| **HRESULT (decimální)** | -2134351824 |
| **Text chyby** | ECS_E_AZURE_FILE_SHARE_NOT_FOUND |
| **Požadována náprava** | Ano |

Tato chyba nastane, pokud sdílená složka Azure není dostupný. Řešení:

1. [Ověřte, že účet úložiště existuje.](#troubleshoot-storage-account)
2. [Ujistěte se, že existuje sdílená složka Azure.](#troubleshoot-azure-file-share)

Pokud byl odstraněn sdílenou složku Azure, musíte vytvořit nové sdílené složky a potom znovu vytvořit skupinu synchronizace. 

<a id="-2134364042"></a>**Synchronizace je pozastavená tohoto předplatného Azure je pozastaven.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x80C83076 |
| **HRESULT (decimální)** | -2134364042 |
| **Text chyby** | ECS_E_SYNC_BLOCKED_ON_SUSPENDED_SUBSCRIPTION |
| **Požadována náprava** | Ano |

Tato chyba nastane, pokud je předplatné Azure pozastavené. Synchronizace se opětovně povolena, když se obnoví předplatné Azure. Zobrazit [je proč Moje předplatné Azure aktivní a jak ho aktivovat?](../../billing/billing-subscription-become-disable.md) Další informace.

<a id="-2134364052"></a>**Účet úložiště obsahuje bránu firewall nebo virtuální sítě nakonfigurovaná.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x80c8306c |
| **HRESULT (decimální)** | -2134364052 |
| **Text chyby** | ECS_E_MGMT_STORAGEACLSNOTSUPPORTED |
| **Požadována náprava** | Ano |

K této chybě dochází, pokud sdílená složka Azure je nepřístupný z důvodu bránu firewall pro účet úložiště, nebo protože účet úložiště patří do virtuální sítě. Azure File Sync ještě nemá podporu pro tuto funkci. Řešení:

1. [Ověřte, že účet úložiště existuje.](#troubleshoot-storage-account)
2. [Zkontrolujte, ujistěte se, že účet úložiště neobsahuje žádná pravidla sítě.](#troubleshoot-network-rules)

Odeberte tato pravidla, chcete-li vyřešit tento problém. 

<a id="-2134375911"></a>**Synchronizace se nezdařila z důvodu problému s databází synchronizace.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x80c80219 |
| **HRESULT (decimální)** | -2134375911 |
| **Text chyby** | ECS_E_SYNC_METADATA_WRITE_LOCK_TIMEOUT |
| **Požadována náprava** | Ne |

Tato chyba obvykle vyřeší sám a může dojít, pokud jsou:

* Vysoký počet změn souborů na serverech ve skupině synchronizace.
* Velký počet chyb u jednotlivých souborů a adresářů.

Pokud tento problém potrvá déle než několik hodin, vytvořte žádost o podporu a pošleme vám pomůžou při řešení tohoto problému.

<a id="-2146762487"></a>**Serveru se nepodařilo navázat zabezpečené připojení. Cloudová služba přijala neočekávané certifikátu.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x800b0109 |
| **HRESULT (decimální)** | -2146762487 |
| **Text chyby** | CERT_E_UNTRUSTEDROOT |
| **Požadována náprava** | Ano |

K této chybě může dojít, pokud vaše organizace používá SSL proxy ukončující nebo pokud škodlivý entity brání přenosů mezi serverem a službu Azure File Sync. Pokud jste si jisti, že to je očekáváno, (protože vaše organizace používá proxy se ukončuje SSL), můžete přeskočit ověření certifikátů se přepsání registru.

1. Vytvořte hodnotu registru SkipVerifyingPinnedRootCertificate.

    ```PowerShell
    New-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Azure\StorageSync -Name SkipVerifyingPinnedRootCertificate -PropertyType DWORD -Value 1
    ```

2. Restartujte službu synchronizace na registrovaného serveru.

    ```PowerShell
    Restart-Service -Name FileSyncSvc -Force
    ```

Když nastavíte tuto hodnotu registru, agent služby Azure File Sync přijme při přenosu dat mezi serverem a cloudovou službou jakýkoli místně důvěryhodný certifikát protokolu SSL.

<a id="-2147012894"></a>**Nelze navázat připojení ke službě.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x80072EE2 |
| **HRESULT (decimální)** | -2147012894 |
| **Text chyby** | WININET_E_TIMEOUT |
| **Požadována náprava** | Ano |

[!INCLUDE [storage-sync-files-bad-connection](../../../includes/storage-sync-files-bad-connection.md)]

<a id="-2134375680"></a>**Synchronizace se nezdařila z důvodu problému s ověřováním.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x80c80300 |
| **HRESULT (decimální)** | -2134375680 |
| **Text chyby** | ECS_E_SERVER_CREDENTIAL_NEEDED |
| **Požadována náprava** | Ano |

K této chybě obvykle dochází, protože čas serveru je nesprávný nebo vypršela platnost certifikátu používaného pro ověřování. Pokud je serveru správná, proveďte následující kroky se odstranit certifikát s prošlou platností (pokud vypršela platnost) a obnovit stav registrace serveru:

1. Otevřete modul snap-in Certifikáty konzoly MMC, zvolte účet počítače a přejděte do \Personal\Certificates certifikáty (místní počítač).
2. Odstranit certifikát pro ověřování klientů, pokud vypršela platnost a zavřete modul snap-in Certifikáty konzoly MMC.
3. Otevřete Regedit a odstranění ServerSetting klíče v registru: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure\StorageSync\ServerSetting
4. Na webu Azure Portal přejděte do části registrovaných serverů služby synchronizace úložiště. Klikněte pravým tlačítkem na server s certifikát s prošlou platností a klikněte na tlačítko "Zrušit registraci serveru."
5. Spusťte následující příkazy Powershellu na serveru:

    ```PowerShell
    Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
    Reset-StorageSyncServer
    ```

6. Opětovná registrace serveru spuštěním ServerRegistration.exe (výchozí umístění je C:\Program Files\Azure\StorageSyncAgent).

<a id="-1906441711"></a><a id="-2134375654"></a><a id="doesnt-have-enough-free-space"></a>**Svazku, kde je umístěn koncový bod serveru je málo místa na disku.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x8e5e0211 |
| **HRESULT (decimální)** | -1906441711 |
| **Text chyby** | JET_errLogDiskFull |
| **Požadována náprava** | Ano |
| | |
| **HODNOTA HRESULT** | 0x80c8031a |
| **HRESULT (decimální)** | -2134375654 |
| **Text chyby** | ECS_E_NOT_ENOUGH_LOCAL_STORAGE |
| **Požadována náprava** | Ano |

K této chybě dochází, protože má svazek zaplní. K této chybě obvykle dochází, protože soubory mimo koncový bod serveru používají místo na svazku. Uvolněte místo na svazku přidáním dalšího serveru pro koncové body, přesouvání souborů na jiný svazek nebo zvětšení velikosti svazku koncový bod serveru zapnutá.

<a id="-2134364145"></a><a id="replica-not-ready"></a>**Služba ještě není připravené na synchronizaci s Tento koncový bod serveru.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x80c8300f |
| **HRESULT (decimální)** | -2134364145 |
| **Text chyby** | ECS_E_REPLICA_NOT_READY |
| **Požadována náprava** | Ne |

K této chybě dochází, protože došlo ke změnám na sdílené složky Azure přímo a probíhá detekce změn. Synchronizace začnou po dokončení detekce změn.

[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="-2134375877"></a><a id="-2134375908"></a><a id="-2134375853"></a>**Synchronizace se nezdařila z důvodu problémů s velkým množstvím jednotlivých souborů.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x80c8023b |
| **HRESULT (decimální)** | -2134364145 |
| **Text chyby** | ECS_E_SYNC_METADATA_KNOWLEDGE_SOFT_LIMIT_REACHED |
| **Požadována náprava** | Ano |
| | |
| **HODNOTA HRESULT** | 0x80c8021c |
| **HRESULT (decimální)** | -2134375908 |
| **Text chyby** | ECS_E_SYNC_METADATA_KNOWLEGE_LIMIT_REACHED |
| **Požadována náprava** | Ano |
| | |
| **HODNOTA HRESULT** | 0x80c80253 |
| **HRESULT (decimální)** | -2134375853 |
| **Text chyby** | ECS_E_TOO_MANY_PER_ITEM_ERRORS |
| **Požadována náprava** | Ano |

V případech, kdy existuje mnoho za chyby synchronizace souborů, může relace synchronizace začnou být neúspěšné. Řešení potíží s tímto stavem, naleznete v tématu [řešení potíží s za chyby synchronizace souborů či složek](#troubleshooting-per-file-directory-sync-errors).

> [!NOTE]
> Azure File Sync vytvoří dočasné snímek služby VSS jednou za den na serveru, aby synchronizovat soubory, které mají otevřených popisovačů.

<a id="-2134376423"></a>**Synchronizace se nezdařila z důvodu problému s cestou koncový bod serveru.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x80c80019 |
| **HRESULT (decimální)** | -2134376423 |
| **Text chyby** | ECS_E_SYNC_INVALID_PATH |
| **Požadována náprava** | Ano |

Ujistěte se, že cesta existuje, je na místním svazku NTFS a není to spojovací bod nebo existující koncový bod serveru.

<a id="-2134376373"></a>**Služba je momentálně není k dispozici.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x80c8004b |
| **HRESULT (decimální)** | -2134376373 |
| **Text chyby** | ECS_E_SERVICE_UNAVAILABLE |
| **Požadována náprava** | Ne |

K této chybě dochází, protože služba Azure File Sync není k dispozici. Tato chyba bude automaticky vyřešit při služby Azure File Sync vzhledem k tomu, k dispozici znovu.

<a id="-2134375922"></a>**Synchronizace se nezdařila z důvodu o přechodný problém s databáze sync.**  
| | |
|-|-|
| **HODNOTA HRESULT** | 0x80c8020e |
| **HRESULT (decimální)** | -2134375922 |
| **Text chyby** | ECS_E_SYNC_METADATA_WRITE_LEASE_LOST |
| **Požadována náprava** | Ne |

K této chybě dochází z důvodu vnitřní problém s databáze sync. Tato chyba bude automaticky vyřešit při Azure File Sync při opakování synchronizace. Pokud tato chyba přetrvává pro určité časové období prodloužit, vytvořte žádost o podporu a pošleme vám pomůžou při řešení tohoto problému.

### <a name="common-troubleshooting-steps"></a>Běžné kroky odstraňování potíží
<a id="troubleshoot-storage-account"></a>**Ověřte, že účet úložiště existuje.**  
# <a name="portaltabportal"></a>[Azure Portal](#tab/portal)
1. Přejděte do skupiny synchronizace v rámci služby synchronizace úložiště.
2. Vyberte koncový bod cloudu v rámci skupiny synchronizace.
3. Poznamenejte si název sdílené složky Azure file v podokně otevřené.
4. Vyberte propojeném účtu úložiště. Pokud se tento odkaz se nepodaří, účet úložiště odkazované se odebrala.
    ![Snímek obrazovky ukazující na panelu informací o koncový bod cloudu s odkazem na účet úložiště.](media/storage-sync-files-troubleshoot/file-share-inaccessible-1.png)

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
```PowerShell
# Variables for you to populate based on your configuration
$agentPath = "C:\Program Files\Azure\StorageSyncAgent"
$region = "<Az_Region>"
$resourceGroup = "<RG_Name>"
$syncService = "<storage-sync-service>"
$syncGroup = "<sync-group>"

# Import the Azure File Sync management cmdlets
Import-Module "$agentPath\StorageSync.Management.PowerShell.Cmdlets.dll"

# Log into the Azure account and put the returned account information
# in a reference variable.
$acctInfo = Connect-AzureRmAccount

# this variable stores your subscription ID 
# get the subscription ID by logging onto the Azure portal
$subID = $acctInfo.Context.Subscription.Id

# this variable holds your Azure Active Directory tenant ID
# use Login-AzureRMAccount to get the ID from that context
$tenantID = $acctInfo.Context.Tenant.Id

# Check to ensure Azure File Sync is available in the selected Azure
# region.
$regions = [System.String[]]@()
Get-AzureRmLocation | ForEach-Object { 
    if ($_.Providers -contains "Microsoft.StorageSync") { 
        $regions += $_.Location 
    } 
}

if ($regions -notcontains $region) {
    throw [System.Exception]::new("Azure File Sync is either not available in the " + `
        " selected Azure Region or the region is mistyped.")
}

# Check to ensure resource group exists and create it if doesn't
$resourceGroups = [System.String[]]@()
Get-AzureRmResourceGroup | ForEach-Object { 
    $resourceGroups += $_.ResourceGroupName 
}

if ($resourceGroups -notcontains $resourceGroup) {
    throw [System.Exception]::new("The provided resource group $resourceGroup does not exist.")
}

# the following command creates an AFS context 
# it enables subsequent AFS cmdlets to be executed with minimal 
# repetition of parameters or separate authentication 
Login-AzureRmStorageSync `
    –SubscriptionId $subID `
    -ResourceGroupName $resourceGroup `
    -TenantId $tenantID `
    -Location $region

# Check to make sure the provided Storage Sync Service
# exists.
$syncServices = [System.String[]]@()

Get-AzureRmStorageSyncService -ResourceGroupName $resourceGroup | ForEach-Object {
    $syncServices += $_.DisplayName
}

if ($storageSyncServices -notcontains $syncService) {
    throw [System.Exception]::new("The provided Storage Sync Service $syncService does not exist.")
}

# Check to make sure the provided Sync Group exists
$syncGroups = [System.String[]]@()

Get-AzureRmStorageSyncGroup -ResourceGroupName $resourceGroup -StorageSyncServiceName $syncService | ForEach-Object {
    $syncGroups += $_.DisplayName
}

if ($syncGroups -notcontains $syncGroup) {
    throw [System.Exception]::new("The provided sync group $syncGroup does not exist.")
}

# Get reference to cloud endpoint
$cloudEndpoint = Get-AzureRmStorageSyncCloudEndpoint `
    -ResourceGroupName $resourceGroup `
    -StorageSyncServiceName $storageSyncService `
    -SyncGroupName $syncGroup

# Get reference to storage account
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroup | Where-Object { 
    $_.Id -eq $cloudEndpoint.StorageAccountResourceId
}

if ($storageAccount -eq $null) {
    Write-Host "The storage account referenced in the cloud endpoint does not exist."
}
```
---

<a id="troubleshoot-network-rules"></a>**Zkontrolujte, ujistěte se, že účet úložiště neobsahuje žádná pravidla sítě.**  
# <a name="portaltabportal"></a>[Azure Portal](#tab/portal)
1. Jednou v účtu úložiště vyberte **virtuální sítí a bran firewall** na levé straně účtu úložiště.
2. V účtu úložiště **povolit přístup ze všech sítí** přepínač by měl být vybrán.
    ![Snímek obrazovky zobrazující úložiště účtu sítě a brány firewall pravidla zakázán.](media/storage-sync-files-troubleshoot/file-share-inaccessible-2.png)

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
```PowerShell
if ($storageAccount.NetworkRuleSet.DefaultAction -ne 
    [Microsoft.Azure.Commands.Management.Storage.Models.PSNetWorkRuleDefaultActionEnum]::Allow) {
    Write-Host ("The storage account referenced contains network " + `
        "rules which are not currently supported by Azure File Sync.")
}
```
---

<a id="troubleshoot-azure-file-share"></a>**Ujistěte se, že existuje sdílená složka Azure.**  
# <a name="portaltabportal"></a>[Azure Portal](#tab/portal)
1. Klikněte na tlačítko **přehled** v levé tabulce obsah se vrátíte na stránku hlavního úložiště účtu.
2. Vyberte **soubory** Chcete-li zobrazit seznam sdílených složek.
3. Ověřte sdílenou odkazuje koncového bodu cloudu se zobrazí v seznamu sdílených složek (měli jste zaznamenali to v kroku 1 výše).

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
```PowerShell
$fileShare = Get-AzureStorageShare -Context $storageAccount.Context | Where-Object {
    $_.Name -eq $cloudEndpoint.StorageAccountShareName -and
    $_.IsSnapshot -eq $false
}

if ($fileShare -eq $null) {
    Write-Host "The Azure file share referenced by the cloud endpoint does not exist"
}
```
---

<a id="troubleshoot-rbac"></a>**Ujistěte se, že má přístup k účtu úložiště Azure File Sync.**  
# <a name="portaltabportal"></a>[Azure Portal](#tab/portal)
1. Klikněte na tlačítko **řízení přístupu (IAM)** v levé tabulce obsah přejít na seznam uživatelů a aplikací (*instanční*) která mají přístup k vašemu účtu úložiště.
2. Ověřte **hybridní služby File Sync** se zobrazí v seznamu **Čtenář a přístup k datům** role. 

    ![Snímek obrazovky instančnímu objektu služby File Sync hybridní v kartě pro řízení přístupu k účtu úložiště](media/storage-sync-files-troubleshoot/file-share-inaccessible-3.png)

    Pokud **hybridní služby File Sync** nezobrazí v seznamu, proveďte následující kroky:

    - Klikněte na tlačítko **Add** (Přidat).
    - V **Role** pole, vyberte **Čtenář a přístup k datům**.
    - V **vyberte** zadejte **hybridní služby File Sync**, vyberte roli a klikněte na tlačítko **Uložit**.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)
```PowerShell    
$foundSyncPrincipal = $false
Get-AzureRmRoleAssignment -Scope $storageAccount.Id | ForEach-Object { 
    if ($_.DisplayName -eq "Hybrid File Sync Service") {
        $foundSyncPrincipal = $true
        if ($_.RoleDefinitionName -ne "Reader and Data Access") {
            Write-Host ("The storage account has the Azure File Sync " + `
                "service principal authorized to do something other than access the data " + `
                "within the referenced Azure file share.")
        }

        break
    }
}

if (!$foundSyncPrincipal) {
    Write-Host ("The storage account does not have the Azure File Sync " + `
                "service principal authorized to access the data within the " + ` 
                "referenced Azure file share.")
}
```
---

### <a name="how-do-i-prevent-users-from-creating-files-containing-unsupported-characters-on-the-server"></a>Jak zabránit uživatelům ve vytváření souborů obsahujících nepodporované znaky na serveru?
Můžete použít [blokování souborů prostředků správce soubor serveru (FSRM)](https://docs.microsoft.com/windows-server/storage/fsrm/file-screening-management) do bloku souborů pomocí nepodporované znaky v názvu z vytváří na serveru. Budete muset provést pomocí prostředí PowerShell jako většina nepodporované znaky nejsou tisknutelný a budete muset nejprve přetypování jejich šestnáctkové vyjádření jako znaky.

Nejprve vytvořte skupinu souborů správce prostředků souborového serveru pomocí [rutiny New-FsrmFileGroup](https://docs.microsoft.com/powershell/module/fileserverresourcemanager/new-fsrmfilegroup). Tento příklad definuje skupiny tak, aby obsahovala pouze dvě z nepodporované znaky, ale může obsahovat libovolný počet znaků podle potřeby ve skupině souborů.

```PowerShell
New-FsrmFileGroup -Name "Unsupported characters" -IncludePattern @(("*"+[char]0x00000090+"*"),("*"+[char]0x0000008F+"*"))
```

Po definování skupiny souborů služby Správce prostředků souborového serveru, můžete vytvořit blokování souborů správce prostředků souborového serveru pomocí rutiny New-FsrmFileScreen.

```PowerShell
New-FsrmFileScreen -Path "E:\AFSdataset" -Description "Filter unsupported characters" -IncludeGroup "Unsupported characters"
```

> [!Important]  
> Všimněte si, že blokování souborů by měla sloužit pouze k blokování vytváření znaky nejsou podporovány službou Azure File Sync. Pokud blokování souborů se používají v jiných scénářích, synchronizace se průběžně pokusí stáhnout soubory ze sdílené složky Azure k serveru a budou blokovány z důvodu blokování souborů, což vede k odchozí přenos dat vysoké. 

## <a name="cloud-tiering"></a>Vrstvení cloudu 
Existují dvě cesty pro selhání v cloudu ovládání datových vrstev na:

- Soubory může selhat na úroveň, což znamená, že Azure File Sync neúspěšných pokusů na úroveň souboru do služby soubory Azure.
- Soubory může selhat Vzpomínáte, což znamená, že filtr systému souborů Azure File Sync (StorageSync.sys) nepodaří stáhnout data, když se uživatel pokusí o přístup k souboru, který zřízeny vrstvené.

Existují dva hlavní třídy chyb, které můžou probíhat přes buď cesta selhání:

- Chyby cloudové úložiště
    - *Problémy s dostupností služby přechodným úložištěm*. Další informace najdete v tématu [smlouva o úrovni služeb (SLA) pro službu Azure Storage](https://azure.microsoft.com/support/legal/sla/storage/v1_2/).
    - *Nedostupné sdílené složky Azure*. Této chybě obvykle dochází v případě odstranit sdílenou složku Azure, když je stále koncového bodu cloudu ve skupině synchronizace.
    - *Účet úložiště nedostupný*. Této chybě obvykle dochází, když odstraníte účet úložiště, pokud má stále do sdílené složky Azure file, což je koncový bod cloudu ve skupině synchronizace. 
- Selhání na serveru 
    - *Azure File Sync filtru systému souborů (StorageSync.sys) není načten*. Aby bylo možné reagovat na odvolání/ovládání datových vrstev na požadavky, musí být načten filtru systému souborů Azure File Sync. Filtr není načten může dojít z několika důvodů, ale nejběžnějším důvodem je, že správce uvolnit ji ručně. Azure File Sync filtru systému souborů musí být načtena na celou dobu pro Azure File Sync správně fungovat.
    - *Chybí, je poškozený nebo jinak přerušeno spojovacím bodem*. Bod opakování analýzy je speciální datové struktury v souboru, který se skládá ze dvou částí:
        1. Značky rozboru, který určuje v operačním systému, že filtr systému souborů Azure File Sync (StorageSync.sys) možná muset provést některé akce na vstupu nebo výstupu do souboru. 
        2. Zpracování dat, který označuje filtru systému souborů identifikátoru URI souboru na koncovém bodu přidruženého cloudu (sdílená složka Azure). 
        
        Nejběžnější způsob, jakým může dojít k poškození spojovacím bodem je, pokud se správce pokusí upravit značky nebo jeho data. 
    - *Problém s připojením*. Aby bylo možné vrstvy nebo odvolat soubor, server musí mít připojení k Internetu.

Následující části uvádějí, jak potíží vrstvení cloudu a určení, zda je problém problém úložiště cloudu nebo chybu serveru.

<a id="monitor-tiering-activity"></a>**Jak monitorovat vrstvení aktivity na serveru**  
K monitorování vrstvení aktivity na serveru, použijte 9002 ID události, 9003, události 9016 a 9029 v protokolu událostí Telemetrie (nachází se v umístění Applications and Services\Microsoft\FileSync\Agent v prohlížeči událostí).

- ID události 9002 poskytuje zástupných položek statistiky pro koncový bod serveru. Například TotalGhostedFileCount, SpaceReclaimedMB, atd.

- ID události 9003 poskytuje distribucí chyb pro koncový bod serveru. Celkový počet chyb, kód chyby, apod. Všimněte si, že se jedna událost je protokolována za kód chyby.

- ID události 9016 poskytuje zástupných položek výsledky pro svazek. Například volného místa, které zbývá, počet souborů, které vytvořena zástupná položka v relaci, počet souborů se nezdařilo. Chcete-li ghost, atd.

- ID události 9029 poskytuje informace o relaci zástupných položek. Třeba počet souborů v této relaci, počet souborů, které se pokusil vrstvami v této relaci, počet souborů, které již vrstvené atd.

<a id="monitor-recall-activity"></a>**Jak monitorovat aktivity spojené s vracením na serveru**  
Monitorování aktivity spojené s vracením na serveru, použijte událost ID 9005 9006, 9007 v protokolu událostí Telemetrie (nachází se v umístění Applications and Services\Microsoft\FileSync\Agent v prohlížeči událostí). Všimněte si, že se tyto události jsou protokolovány po hodinách.

- ID události 9005 poskytuje spojené s vracením spolehlivost pro koncový bod serveru. Například celkový počet jedinečných souborů přístup, celkový počet jedinečných souborů s neúspěšných přístupů atd.

- ID události 9006 poskytuje distribuci chyby spojené s vracením pro koncový bod serveru. Celkový počet neúspěšných požadavků, kód chyby, apod. Všimněte si, že se jedna událost je protokolována za kód chyby.

- ID události 9007 poskytuje spojené s vracením výkon pro koncový bod serveru. Například TotalRecallIOSize, TotalRecallTimeTaken, atd.

<a id="files-fail-tiering"></a>**Řešení problémů se soubory, které se nepodařilo vrstvy**  
Pokud soubory vrstvy do služby soubory Azure:

1. V prohlížeči událostí zkontrolujte telemetrická data, provozní a diagnostické protokoly událostí, nachází v umístění Applications and Services\Microsoft\FileSync\Agent. 
    1. Ověřte, že soubory existují v sdílené složky Azure.

    > [!NOTE]
    > Soubor musí synchronizovat do sdílené složky Azure, než může být rozvrstvena.

    2. Ověřte, že server má připojení k Internetu. 
    3. Ověřte, zda že běží ovladače filtru Azure File Sync (StorageSync.sys a StorageSyncGuard.sys):
        - Na příkazovém řádku se zvýšenými oprávněními spusťte `fltmc`. Ověřte, že jsou uvedeny StorageSync.sys a StorageSyncGuard.sys filtr ovladače systému souborů.

> [!NOTE]
> 9003 ID události je zaznamenána jednou za hodinu v protokolu událostí Telemetrie, pokud se soubor nepodaří vrstvy (jednu událost je protokolována za kód chyby:). Diagnostické protokoly událostí a provozní by měla sloužit pokud vyžadují další informace pro diagnostiku problému.

<a id="files-fail-recall"></a>**Řešení problémů se soubory, které se nepodařilo zrušit**  
Pokud třeba připomenout soubory:
1. V prohlížeči událostí zkontrolujte telemetrická data, provozní a diagnostické protokoly událostí, nachází v umístění Applications and Services\Microsoft\FileSync\Agent.
    1. Ověřte, že soubory existují v sdílené složky Azure.
    2. Ověřte, že server má připojení k Internetu. 
    3. Otevřete modul snap-in Services MMC a ověřte, zda že je spuštěna Služba agenta synchronizace úložiště (FileSyncSvc).
    4. Ověřte, zda že běží ovladače filtru Azure File Sync (StorageSync.sys a StorageSyncGuard.sys):
        - Na příkazovém řádku se zvýšenými oprávněními spusťte `fltmc`. Ověřte, že jsou uvedeny StorageSync.sys a StorageSyncGuard.sys filtr ovladače systému souborů.

> [!NOTE]
> 9006 ID události je zaznamenána jednou za hodinu v protokolu událostí Telemetrie, pokud soubor se nepodařilo odvolat (jednu událost je protokolována za kód chyby:). Diagnostické protokoly událostí a provozní by měla sloužit pokud vyžadují další informace pro diagnostiku problému.

<a id="files-unexpectedly-recalled"></a>**Řešení problémů se soubory na serveru se neočekávaně připomenout**  
Antivirová ochrana v programu, zálohování a další aplikace, které načítají velké množství souborů způsobit nežádoucí navrácení, pokud nerespektují atribut offline přeskočit a přeskočí čtení obsahu těchto souborů. Přeskočení offline souborů pro produkty, které podporují tato možnost pomáhá zabránit neúmyslnému navrácení během operace, třeba antivirovou kontrolu nebo úlohy zálohování.

Poraďte s dodavatelem softwaru se naučíte konfigurovat jejich řešení Chcete-li přeskočit čtení offline soubory.

Nežádoucí navrácení také může dojít u dalších scénářů, jako jsou při prohlížení souborů v Průzkumníku souborů. Otevřete složku, která má cloud vrstvené soubory v Průzkumníku souborů na serveru může způsobit nežádoucí navrácení. To je ještě více pravděpodobné, pokud je na serveru zapnutá antivirová řešení.

## <a name="general-troubleshooting"></a>Obecné řešení potíží
Pokud narazíte na problémy s Azure File Sync na serveru, spusťte následující kroky:
1. V prohlížeči událostí zkontrolujte telemetrická data, provozní a diagnostické protokoly událostí.
    - Synchronizovat, ovládání datových vrstev, a problémy spojené s vracením přihlášeni telemetrie, diagnostických a provozní protokoly událostí v umístění Applications and Services\Microsoft\FileSync\Agent.
    - Problémy související se správou serveru (například nastavení konfigurace) jsou protokolovány v provozních a diagnostické protokoly událostí v umístění Applications and Services\Microsoft\FileSync\Management.
2. Ověřte, zda že na serveru běží služba Azure File Sync:
    - Otevřete modul snap-in Services MMC a ověřte, zda je spuštěna Služba agenta synchronizace úložiště (FileSyncSvc).
3. Ověřte, zda že běží ovladače filtru Azure File Sync (StorageSync.sys a StorageSyncGuard.sys):
    - Na příkazovém řádku se zvýšenými oprávněními spusťte `fltmc`. Ověřte, že jsou uvedeny StorageSync.sys a StorageSyncGuard.sys filtr ovladače systému souborů.

Pokud není problém vyřešen, spusťte nástroj AFSDiag:
1. Vytvoření adresáře, ve kterém se uloží výstupní AFSDiag (například C:\Output).
2. Otevřete okno Powershellu se zvýšenými oprávněními a spusťte následující příkazy (po každém z nich stiskněte klávesu Enter):

    ```PowerShell
    cd "c:\Program Files\Azure\StorageSyncAgent"
    Import-Module .\afsdiag.ps1
    Debug-Afs c:\output # Note: Use the path created in step 1.
    ```

3. Pro úroveň trasování Azure File Sync jádra režimu, zadejte **1** (Pokud není uvedeno jinak, chcete-li vytvořit podrobnější trasování), a potom stiskněte klávesu Enter.
4. Pro úroveň trasování Azure File Sync uživatelského režimu, zadejte **1** (Pokud není uvedeno jinak, chcete-li vytvořit podrobnější trasování), a potom stiskněte klávesu Enter.
5. Reprodukujte problém. Jakmile budete hotovi, zadejte **D**.
6. Soubor .zip, který obsahuje protokolů a trasovací soubory se uloží do výstupního adresáře, který jste zadali.

## <a name="see-also"></a>Další informace najdete v tématech
- [Služba soubory Azure – nejčastější dotazy](storage-files-faq.md)
- [Řešení potíží s Azure Files problémy ve Windows](storage-troubleshoot-windows-file-connection-problems.md)
- [Řešení potíží s Azure Files v Linuxu](storage-troubleshoot-linux-file-connection-problems.md)
