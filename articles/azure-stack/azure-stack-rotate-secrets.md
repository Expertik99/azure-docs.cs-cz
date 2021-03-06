---
title: Otočit tajné kódy ve službě Azure Stack | Dokumentace Microsoftu
description: Zjistěte, jak otočit vaše tajné kódy ve službě Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: dacfa738a99eb2d580d825957d09b2b1a3111e93
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/07/2018
ms.locfileid: "44051387"
---
# <a name="rotate-secrets-in-azure-stack"></a>Otočit tajné kódy ve službě Azure Stack

*Tyto pokyny platí jenom pro Azure Stack integrované systémy verze 1803 a novější. Nepokoušejte tajných kódů otočení na verze pre-1802 Azure Stack*

Azure Stack používá různé tajných kódů k údržbě zabezpečenou komunikaci mezi prostředky infrastruktury Azure stacku a služeb.

- **Interní tajných kódů**  
Všechny certifikáty, hesla, zabezpečené řetězce a klíčů používaných infrastruktury služby Azure Stack bez zásahu operátor Azure stacku. 

- **Externích tajných klíčů**  
Certifikáty služby infrastruktury externích služeb, které jsou k dispozici v operátor Azure stacku. To zahrnuje certifikáty pro následující služby: 
    - Portál správce 
    - Veřejný portál 
    - Správce Azure Resource Manageru 
    - Globální Azure Resource Manageru 
    - Správce služby Keyvault 
    - KeyVault 
    - Služby ACS (včetně objektů blob, table a queue storage) 
    - ADFS<sup>*</sup>
    - Graf<sup>*</sup>

   <sup>*</sup> Platí pouze pokud je zprostředkovatel identity prostředí Active Directory Federated Services (AD FS).

> [!NOTE]
> Všechny ostatní zabezpečení klíčů a řetězce, včetně BMC a přepněte hesla, správce ručně aktualizovat hesla k účtům uživatelů a správců. 

Aby bylo možné udržovat tak integritu infrastruktury Azure stacku, operátory nemusí pravidelně obměňovat svoji infrastrukturu tajných kódů v frekvence, které jsou v souladu s požadavky na zabezpečení organizace.

### <a name="rotating-secrets-with-external-certificates-from-a-new-certificate-authority"></a>Otáčení tajných kódů pomocí externí certifikáty od certifikační autority nový

Azure Stack podporuje v následujících kontextů tajných kódů otočení s externí certifikáty od nového certifikátu autority (CA):

|Nainstalovaný certifikát certifikační Autority|Otočit na certifikační Autority.|Podporováno|Podporované verze Azure Stack|
|-----|-----|-----|-----|
|Z podepsaného svým držitelem|Do firemní sítě|Nepodporuje se||
|Z podepsaného svým držitelem|Pro podepsaný svým držitelem|Nepodporuje se||
|Z podepsaného svým držitelem|Na veřejnou<sup>*</sup>|Podporováno|1803 & později|
|Z organizace|Do firemní sítě|Podporované tak dlouho, dokud zákazníci využívat stejné podnikové certifikační Autority, jak použít při nasazení|1803 & později|
|Z organizace|Pro podepsaný svým držitelem|Nepodporuje se||
|Z organizace|Na veřejnou<sup>*</sup>|Podporováno|1803 & později|
|Z veřejné<sup>*</sup>|Do firemní sítě|Nepodporuje se|1803 & později|
|Z veřejné<sup>*</sup>|Pro podepsaný svým držitelem|Nepodporuje se||
|Z veřejné<sup>*</sup>|Na veřejnou<sup>*</sup>|Podporováno|1803 & později|

<sup>*</sup> Tady veřejné certifikační autority jsou ty, které jsou součástí programu Windows důvěryhodné kořenové. Úplný seznam najdete [Microsoft Trusted Root Certificate Program: účastníky (od 27. června 2017)](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca).

## <a name="alert-remediation"></a>Oprava výstrahy

Po do 30 dnů od vypršení platnosti tajných kódů tyto výstrahy jsou generovány v portálu správce: 

- Vypršení platnosti hesla účtu čekající služby 
- Vypršení platnosti vnitřní certifikát čekající na vyřízení 
- Vypršení platnosti externí certifikát čekající na vyřízení 

Spuštění tajných kódů otočení pomocí následujícího postupu se opravit tyto výstrahy.

## <a name="pre-steps-for-secret-rotation"></a>Kroky prováděné před zpracováním pro rotaci tajného kódu

   > [!IMPORTANT]  
   > Zajistěte, aby tajných kódů rotace nebyla provedena úspěšně ve vašem prostředí. Pokud již byla provedena tajných kódů otočení aktualizaci na verzi 1807 nebo novější, ještě před spuštěním otočení tajných kódů služby Azure Stack. 
1.  Upozorněte uživatele na jakékoli operace údržby. Naplánujte běžné časová období údržby, co je to možné, během pracovní doby. Operace údržby může ovlivnit uživatelské úlohy a operací na portálu.
    > [!note]  
    > Další kroky se projeví pouze v případě otáčení externích tajných klíčů Azure Stack.
3. Přidejte novou sadu nahrazení externí certifikáty. Nová sada odpovídá certifikát požadavky uvedené v [požadavky na certifikát Azure Stack PKI](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs).
4.  Store zálohováním do certifikátů používaných pro rotaci do zabezpečeného umístění. Pokud vaše otočení spustí a pak se nezdaří, nahraďte certifikáty ve sdílené složce záložní kopie předtím, než znovu spustíte otočení. Mějte na paměti, uchovávání záložních kopií v zabezpečené umístění zálohy.
5.  Vytvoření sdílené složky, ke kterým přistupujete z ERCS virtuálních počítačů. Sdílené složky musí být čtení a zápisu pro **CloudAdmin** identity.
6.  Otevřete konzoly ISE Powershellu na počítači, kde máte přístup ke sdílení souborů. Přejděte na vaši sdílenou složku. 
7.  Spustit **[CertDirectoryMaker.ps1](http://www.aka.ms/azssecretrotationhelper)** k vytvoření požadované adresáře pro externí certifikáty.

## <a name="rotating-external-and-internal-secrets"></a>Otáčení externí a interní tajných kódů

Obměna externí interní tajný kód:

1. V rámci nově vytvořené **/certifikáty** adresář vytvořený v kroky prováděné před zpracováním, umístěte novou sadu nahrazení externí certifikáty do struktury adresářů podle formátu uvedených v části povinné certifikáty nástroje [požadavky na certifikát Azure Stack PKI](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs#mandatory-certificates).
2. Vytvořte relaci Powershellu s [privilegovaných koncový bod](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint) pomocí **CloudAdmin** účtu a uložit tyto relace jako proměnnou. Tato proměnná bude používat jako parametr v dalším kroku.

    > [!IMPORTANT]  
    > Nezadávejte relace, relace uložit jako proměnnou.
    
3. Spustit  **[invoke-command](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-5.1)**. Předání jako proměnnou relace Powershellu privilegovaných koncový bod **relace** parametru. 
4. Spustit **Start SecretRotation** s následujícími parametry:
    - **PfxFilesPath**  
    Zadejte síťovou cestu k adresáři certifikáty vytvořili dříve.  
    - **PathAccessCredential**  
    Objekt PSCredential pro přihlašovací údaje ke sdílené složce. 
    - **CertificatePassword**  
    Zabezpečený řetězec heslo použité pro všechny vytvořené soubory certifikátů pfx.
4. Počkejte, dokud otočit tajných klíčů.  
Po úspěšném dokončení otočení tajného kódu se zobrazí konzola **celkový stav akce: Úspěch**. 
5. Po úspěšném dokončení tajných kódů otáčení odebrat certifikáty ze sdílené složky vytvořené v kroku předběžné a uložit je do jejich zabezpečené umístění zálohy. 

## <a name="walkthrough-of-secret-rotation"></a>Návod k otočení tajného kódu

Následující příklad PowerShell ukazuje, rutiny a parametry k otočení tajných klíčů.

```powershell
#Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IPofERCSMachine>"}'
$PEPCreds = Get-Credential 
$PEPsession = New-PSSession -computername <IPofERCSMachine> -Credential $PEPCreds -ConfigurationName PrivilegedEndpoint 

#Run Secret Rotation
$CertPassword = ConvertTo-SecureString "Certpasswordhere" -AsPlainText -Force
$CertShareCred = Get-Credential 
$CertSharePath = "<NetworkPathofCertShare>"
Invoke-Command -session $PEPsession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword }
Remove-PSSession -Session $PEPSession
```
## <a name="rotating-only-internal-secrets"></a>Otáčení pouze interní tajných kódů

Obměna pouze interní tajných kódů služby Azure Stack:

1. Vytvořte relaci Powershellu s [privilegovaných koncový bod](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint).
2. V relaci privilegovaných koncový bod spustit **Start SecretRotation** bez argumentů.

## <a name="start-secretrotation-reference"></a>Odkaz na začátku SecretRotation

Otočí tajných kódů systému Azure Stack. Provést pouze proti koncovému bodu privilegovaných pro Azure Stack.

### <a name="syntax"></a>Syntaxe

Cesta (výchozí)

```powershell
Start-SecretRotation [-PfxFilesPath <string>] [-PathAccessCredential] <PSCredential> [-CertificatePassword <SecureString>]  
```

### <a name="description"></a>Popis

Rutina Start-SecretRotation otočí infrastruktury tajemství systémech pro Azure Stack. Ve výchozím nastavení otočí všech tajných kódů, které jsou vystaveny interní infrastrukturu sítě s uživatelským vstupem ho také otočí certifikáty všechny koncové body externí síťové infrastruktury. Při obměně koncové body externí síťové infrastruktury, Start-SecretRotation by měl provést prostřednictvím blok skriptu Invoke-Command s prostředím Azure Stack privileged koncový bod relace předaného jako parametr relace.
 
### <a name="parameters"></a>Parametry

| Parametr | Typ | Požaduje se | Umístění | Výchozí | Popis |
| -- | -- | -- | -- | -- | -- |
| PfxFilesPath | Řetězec  | False  | s názvem  | Žádný  | Cesta sdílení souborů na **\Certificates** síťové adresáře, který obsahuje všechny externí certifikáty koncový bod. Povinné pouze v případě otáčení externích tajných klíčů nebo všech tajných kódů. Adresář end musí být **\Certificates**. |
| CertificatePassword | SecureString | False  | s názvem  | Žádný  | Heslo pro všechny certifikáty v PfXFilesPath – k dispozici. Požadovaná hodnota, pokud PfxFilesPath víceklientského interních i externích tajných kódů se otočí. |
| PathAccessCredential | PSCredential | False  | s názvem  | Žádný  | Přihlašovací údaje prostředí PowerShell pro sdílení souborů systému **\Certificates** síťové adresáře, který obsahuje všechny externí certifikáty koncový bod. Povinné pouze v případě otáčení externích tajných klíčů nebo všech tajných kódů.  |
| Spustit znovu | SwitchParameter | False  | s názvem  | Žádný  | Opětovné spuštění se musí použít kdykoli po neúspěšném pokusu se znovu pokus o tajný obměnit. |

### <a name="examples"></a>Příklady
 
**Otočit pouze interní infrastruktury tajných kódů**

```powershell  
PS C:\> Start-SecretRotation  
```

Tento příkaz otočí všechny tajné klíče infrastruktury vystavený k interní síti Azure Stack. Start-SecretRotation otočí všech tajných kódů generovaných zásobníku, ale protože nejsou žádné zadané certifikáty, nebudou otočený o certifikáty externí koncový bod.  

**Otočit infrastruktury interních a externích tajných klíčů**
  
```powershell
PS C:\> Invoke-Command -session $PEPSession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword } 
Remove-PSSession -Session $PEPSession
```

Tento příkaz otočí všechny tajné klíče infrastruktury vystavený k interní síti Azure Stack, stejně jako certifikáty protokolu TLS pro koncové body služby Azure Stack externí síťové infrastruktury. Start-SecretRotation otočí všech tajných kódů generovaných zásobníku a vzhledem k tomu, že jsou k dispozici certifikáty, bude také otočen certifikáty externí koncový bod.  


## <a name="update-the-baseboard-management-controller-bmc-password"></a>Aktualizovat heslo pro řadič pro správu základní desky

Řadič pro správu základní desky (BMC) monitoruje fyzickému stavu vašich serverů. Požadavky a pokyny pro aktualizaci hesla BMC se liší v závislosti na dodavatele hardwaru, výrobce OEM (OEM). Měli byste aktualizovat hesla pro Azure Stack součásti v pravidelných tempo.

1. Aktualizujte správu základní desky na fyzických serverech Azure Stack podle pokynů vaší výrobce OEM. Heslo pro každou BMC ve vašem prostředí musí být stejné.
2. Otevřete koncový bod privilegovaných v Azure stacku relace. Pokyny naleznete v tématu [pomocí privilegovaných koncového bodu ve službě Azure Stack](azure-stack-privileged-endpoint.md).
3. Po Powershellu řádek byl změněn na **[adresa IP nebo virtuální počítač ERCS name]: PS >** nebo **[azs-ercs01]: PS >**, v závislosti na prostředí, spusťte `Set-BmcPassword` spuštěním `invoke-command`. Vaše proměnná privilegovaných koncový bod relace předáte jako parametr. Příklad:

    ```powershell
    # Interactive Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PECred = Get-Credential "<Domain>\CloudAdmin" -Message "PE Credentials" 
    $NewBMCpwd = Read-Host -Prompt "Enter New BMC password" -AsSecureString 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    Remove-PSSession -Session $PEPSession
    ```
    
    Statické verze prostředí PowerShell můžete použít také s hesla jako řádky kódu:
    
    ```powershell
    # Static Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEUser = "<Privileged Endpoint user for exmaple Domain\CloudAdmin>"
    $PEpwd = ConvertTo-SecureString "<Privileged Endpoint Password>" -AsPlainText -Force
    $PECred = New-Object System.Management.Automation.PSCredential ($PEUser, $PEpwd) 
    $NewBMCpwd = ConvertTo-SecureString "<New BMC Password>" -AsPlainText -Force 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    Remove-PSSession -Session $PEPSession
    ```

## <a name="next-steps"></a>Další postup

[Další informace o zabezpečení Azure stacku](azure-stack-security-foundations.md)
