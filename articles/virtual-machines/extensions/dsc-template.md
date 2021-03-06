---
title: Desired State Configuration rozšíření pomocí šablon Azure Resource Manageru
description: Další informace o definici šablony Resource Manageru pro rozšíření Desired State Configuration (DSC) v Azure.
services: virtual-machines-windows
documentationcenter: ''
author: DCtheGeek
manager: carmonm
editor: ''
tags: azure-resource-manager
keywords: DSC
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 05/02/2018
ms.author: dacoulte
ms.openlocfilehash: 1dcbc8e0221689a6ece7e061d4b1a2632986ae84
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224370"
---
# <a name="desired-state-configuration-extension-with-azure-resource-manager-templates"></a>Desired State Configuration rozšíření pomocí šablon Azure Resource Manageru

Tento článek popisuje šablony Azure Resource Manageru pro [obslužné rutiny rozšíření Desired State Configuration (DSC)](dsc-overview.md).

> [!NOTE]
> Můžete narazit na příklady mírně odlišné schéma. Došlo ke změně ve schématu ve vydané verzi. říjnu 2016. Podrobnosti najdete v tématu [aktualizace z předešlého formátu](#update-from-a-previous-format).

## <a name="template-example-for-a-windows-vm"></a>Příklad šablony pro virtuální počítač s Windows

Následující fragment kódu voliče **prostředků** část šablony.
Rozšíření DSC dědí vlastnosti rozšíření výchozí.
Další informace najdete v tématu [VirtualMachineExtension třídy](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.compute.models.virtualmachineextension?view=azure-dotnet.).

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "[concat(parameters('VMName'),'/Microsoft.Powershell.DSC')]",
    "apiVersion": "2017-12-01",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('VMName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Powershell",
        "type": "DSC",
        "typeHandlerVersion": "2.76",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "protectedSettings": {
                "Items": {
                    "registrationKeyPrivate": "registrationKey"
                }
            },
            "publicSettings": {
                "configurationArguments": [{
                        "Name": "RegistrationKey",
                        "Value": {
                            "UserName": "PLACEHOLDER_DONOTUSE",
                            "Password": "PrivateSettingsRef:registrationKeyPrivate"
                        }
                    },
                    {
                        "RegistrationUrl": "registrationUrl"
                    },
                    {
                        "NodeConfigurationName": "nodeConfigurationName"
                    }
                ]
            }
        }
    }
}
```

## <a name="template-example-for-windows-virtual-machine-scale-sets"></a>Nastaví příkladu šablony pro škálování virtuálního počítače Windows

Má uzel sady škálování virtuálního počítače **vlastnosti** oddíl, který má **VirtualMachineProfile extensionProfile** atribut.
V části **rozšíření**, přidejte podrobnosti o rozšíření DSC.

Rozšíření DSC dědí vlastnosti rozšíření výchozí.
Další informace najdete v tématu [VirtualMachineScaleSetExtension třídy](/dotnet/api/microsoft.azure.management.compute.models.virtualmachinescalesetextension?view=azure-dotnet).

```json
"extensionProfile": {
    "extensions": [{
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(parameters('VMName'),'/Microsoft.Powershell.DSC')]",
        "apiVersion": "2017-12-01",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', parameters('VMName'))]"
        ],
        "properties": {
            "publisher": "Microsoft.Powershell",
            "type": "DSC",
            "typeHandlerVersion": "2.76",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "protectedSettings": {
                    "Items": {
                        "registrationKeyPrivate": "registrationKey"
                    }
                },
                "publicSettings": {
                    "configurationArguments": [{
                            "Name": "RegistrationKey",
                            "Value": {
                                "UserName": "PLACEHOLDER_DONOTUSE",
                                "Password": "PrivateSettingsRef:registrationKeyPrivate"
                            },
                        },
                        {
                            "RegistrationUrl": "registrationUrl"
                        },
                        {
                            "NodeConfigurationName": "nodeConfigurationName"
                        }
                    ]
                }
            },
        }
    }]
}
```

## <a name="detailed-settings-information"></a>Informace o podrobné nastavení

Použít následující schéma v **nastavení** oddílu rozšíření DSC Azure v šabloně Resource Manageru.

Seznam argumentů, které jsou k dispozici pro výchozí konfigurační skript najdete v tématu [výchozí konfigurační skript](#default-configuration-script).

```json
"settings": {
    "wmfVersion": "latest",
    "configuration": {
        "url": "http://validURLToConfigLocation",
        "script": "ConfigurationScript.ps1",
        "function": "ConfigurationFunction"
    },
    "configurationArguments": {
        "argument1": "Value1",
        "argument2": "Value2"
    },
    "configurationData": {
        "url": "https://foo.psd1"
    },
    "privacy": {
        "dataCollection": "enable"
    },
    "advancedOptions": {
        "downloadMappings": {
            "customWmfLocation": "http://myWMFlocation"
        }
    }
},
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        },
        "parameterOfTypePSCredential2": {
            "userName": "UsernameValue2",
            "password": "PasswordValue2"
        }
    },
    "configurationUrlSasToken": "?g!bber1sht0k3n",
    "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}
```

## <a name="details"></a>Podrobnosti

| Název vlastnosti | Typ | Popis |
| --- | --- | --- |
| settings.wmfVersion |řetězec |Určuje verzi Windows Management Frameworku (WMF), který musí být nainstalován na váš virtuální počítač. Nastavení této vlastnosti na **nejnovější** nainstaluje nejnovější verzi WMF. V současné době pouze možné hodnoty této vlastnosti jsou **4.0**, **5.0**, **5.0PP**, a **nejnovější**. Tyto možné hodnoty jsou v souladu s aktualizací. Výchozí hodnota je **nejnovější**. |
| Settings.Configuration.URL |řetězec |Určuje adresu URL umístění, ze kterého chcete stáhnout soubor ZIP konfigurace DSC. Pokud zadaná adresa URL se vyžaduje SAS token pro přístup, nastavte **protectedSettings.configurationUrlSasToken** k hodnotě váš token SAS. Tato vlastnost je vyžadována, pokud **settings.configuration.script** nebo **settings.configuration.function** jsou definovány. -Li zadána žádná hodnota pro tyto vlastnosti, rozšíření volá výchozí konfigurační skript nastavení metadat umístění Configuration Manageru (LCM) a musí být zadán argument. |
| Settings.Configuration.Script |řetězec |Určuje název souboru skriptu, který obsahuje definici konfigurace DSC. Tento skript musí být v kořenové složce souboru .zip, který se stáhne z adresy URL pro určené **configuration.url** vlastnost. Tato vlastnost je vyžadována, pokud **settings.configuration.url** nebo **settings.configuration.script** jsou definovány. -Li zadána žádná hodnota pro tyto vlastnosti, rozšíření volá výchozí konfigurační skript nastavení LCM metadat a by měl být zadán argument. |
| Settings.Configuration.Function |řetězec |Určuje název konfigurace DSC. Konfigurace s názvem musí být součástí skriptu, který **configuration.script** definuje. Tato vlastnost je vyžadována, pokud **settings.configuration.url** nebo **settings.configuration.function** jsou definovány. -Li zadána žádná hodnota pro tyto vlastnosti, rozšíření volá výchozí konfigurační skript nastavení LCM metadat a by měl být zadán argument. |
| settings.configurationArguments |Kolekce |Definuje všechny parametry, které chcete předat do vaší konfigurace DSC. Tato vlastnost není zašifrován. |
| settings.configurationData.url |řetězec |Určuje adresu URL z nichž lze stáhnout soubor konfiguračních dat (.psd1) použít jako vstup pro konfiguraci DSC. Pokud zadaná adresa URL se vyžaduje SAS token pro přístup, nastavte **protectedSettings.configurationDataUrlSasToken** k hodnotě váš token SAS. |
| settings.privacy.dataEnabled |řetězec |Povolí nebo zakáže shromažďování telemetrie. Pouze možné hodnoty této vlastnosti jsou **povolit**, **zakázat**, **''**, nebo **$null**. Opuštění tato vlastnost prázdná nebo null umožňuje telemetrická data. Výchozí hodnota je **''**. Další informace najdete v tématu [shromažďování dat rozšíření DSC Azure](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/). |
| settings.advancedOptions.downloadMappings |Kolekce |Definuje alternativní umístění, ze kterých chcete stáhnout WMF. Další informace najdete v tématu [rozšíření DSC Azure 2.8 a jak namapovat na vlastní umístění stahování závislostí rozšíření](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx). |
| protectedSettings.configurationArguments |Kolekce |Definuje všechny parametry, které chcete předat do vaší konfigurace DSC. Tato vlastnost je šifrovaná. |
| protectedSettings.configurationUrlSasToken |řetězec |Určuje token SAS používat pro přístup k adresu URL, která **configuration.url** definuje. Tato vlastnost je šifrovaná. |
| protectedSettings.configurationDataUrlSasToken |řetězec |Určuje token SAS používat pro přístup k adresu URL, která **configurationData.url** definuje. Tato vlastnost je šifrovaná. |

## <a name="default-configuration-script"></a>Výchozí konfigurační skript

Další informace o těchto hodnot najdete v tématu [správce místní konfigurace základního nastavení](/powershell/dsc/metaconfig#basic-settings).
Skript pro konfiguraci výchozí rozšíření DSC můžete nakonfigurovat pouze LCM vlastnosti, které jsou uvedeny v následující tabulce.

| Název vlastnosti | Typ | Popis |
| --- | --- | --- |
| settings.configurationArguments.RegistrationKey |SecureString |Požadovaná vlastnost. Určuje klíč, který se používá pro uzel k registraci ve službě Azure Automation jako heslo objekt pověření prostředí PowerShell. Tuto hodnotu můžete automaticky zjistit pomocí **klíče listkey** metoda proti účtu Automation. Hodnota by měla být zabezpečena jako chráněné nastavení. |
| settings.configurationArguments.RegistrationUrl |řetězec |Požadovaná vlastnost. Určuje adresu URL koncového bodu služby Automation, kde se pokusí zaregistrovat uzlu. Tuto hodnotu můžete automaticky zjistit pomocí **odkaz** metoda proti účtu Automation. |
| settings.configurationArguments.NodeConfigurationName |řetězec |Požadovaná vlastnost. Určuje konfiguraci uzlu v účtu Automation přiřadit k uzlu. |
| settings.configurationArguments.ConfigurationMode |řetězec |Určuje režim pro LCM. Platné možnosti jsou **ApplyOnly**, **ApplyandMonitor**, a **ApplyandAutoCorrect**.  Výchozí hodnota je **ApplyandMonitor**. |
| settings.configurationArguments.RefreshFrequencyMins | UInt32 | Určuje, jak často se pokusí LCM obraťte se na účtu Automation pro aktualizace.  Výchozí hodnota je **30**.  Minimální hodnota je **15**. |
| settings.configurationArguments.ConfigurationModeFrequencyMins | UInt32 | Určuje, jak často LCM ověří aktuální konfiguraci. Výchozí hodnota je **15**. Minimální hodnota je **15**. |
| settings.configurationArguments.RebootNodeIfNeeded | Boolean | Určuje, zda uzel možné automaticky restartovat žádost o operaci DSC. Výchozí hodnota je **false**. |
| settings.configurationArguments.ActionAfterReboot | řetězec | Určuje, co se stane po restartování, při použití konfigurace. Platné možnosti jsou **ContinueConfiguration** a **StopConfiguration**. Výchozí hodnota je **ContinueConfiguration**. |
| settings.configurationArguments.AllowModuleOverwrite | Boolean | Určuje, zda LCM přepíše existující moduly na uzlu. Výchozí hodnota je **false**. |

## <a name="settings-vs-protectedsettings"></a>Nastavení vs. ProtectedSettings

Všechna nastavení jsou uložena v nastavení textového souboru ve virtuálním počítači.
Vlastnosti uvedené pod **nastavení** jsou veřejné vlastnosti.
Veřejné vlastnosti nejsou šifrovány v textovém souboru nastavení.
Vlastnosti uvedené pod **protectedSettings** jsou šifrované pomocí certifikátu a nejsou zobrazeny ve formátu prostého textu v souboru s nastaveními na virtuálním počítači.

Pokud konfiguraci potřebuje přihlašovací údaje, můžete zahrnout přihlašovací údaje v **protectedSettings**:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        }
    }
}
```

## <a name="example-configuration-script"></a>Příklad konfigurace skriptu

Následující příklad ukazuje výchozí chování pro rozšíření DSC, které je poskytovat LCM nastavení metadat a registrace ve službě Automation DSC.
Konfigurace argumenty jsou povinné.
Konfigurace argumenty jsou předány do výchozí konfigurační skript nastavení LCM metadat.

```json
"settings": {
    "configurationArguments": {
        {
            "Name": "RegistrationKey",
            "Value": {
                "UserName": "PLACEHOLDER_DONOTUSE",
                "Password": "PrivateSettingsRef:registrationKeyPrivate"
            },
        },
        "RegistrationUrl": "[parameters('registrationUrl1')]",
        "NodeConfigurationName": "nodeConfigurationNameValue1"
    }
},
"protectedSettings": {
    "Items": {
        "registrationKeyPrivate": "[parameters('registrationKey1')]"
    }
}
```

## <a name="example-using-the-configuration-script-in-azure-storage"></a>Příklad použití konfigurační skript ve službě Azure Storage

Následující příklad je z [přehled obslužné rutiny rozšíření DSC](dsc-overview.md).
Tento příklad používá k nasazení rozšíření šablon Resource Manageru místo rutiny.
Uložte konfiguraci IisInstall.ps1, umístěte ho do souboru ZIP a potom nahrajte soubor přístupný adresy URL.
Tento příklad využívá úložiště objektů Blob v Azure, ale soubory .zip si můžete stáhnout z jakéhokoli libovolného umístění.

V šabloně Resource Manageru následující kód nastaví virtuální počítač ke stažení souboru správné a spusťte odpovídající funkce prostředí PowerShell:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="update-from-a-previous-format"></a>Aktualizace z předešlého formátu

Všechna nastavení s předešlým formátem rozšíření (a které mají veřejné vlastnosti **ModulesUrl**, **ConfigurationFunction**, **SasToken**, nebo  **Vlastnosti**) automaticky přizpůsobí do aktuálního formátu rozšíření.
Spouštějí se stejně jako před nástupem prostředí.

Následující schéma ukazuje, co předchozí nastavení schématu vypadal:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties": {
        "ParameterToConfigurationFunction1": "Value1",
        "ParameterToConfigurationFunction2": "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1"
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": {
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Zde je, jak přizpůsobuje předešlým formátem do aktuálního formátu:

| Název vlastnosti | Ekvivalentní předchozí schématu |
| --- | --- |
| settings.wmfVersion |nastavení. WMFVersion |
| Settings.Configuration.URL |settings.ModulesUrl |
| Settings.Configuration.Script |První část nastavení. ConfigurationFunction (před \\ \\) |
| Settings.Configuration.Function |Druhá část nastavení. ConfigurationFunction (po \\ \\) |
| settings.configurationArguments |nastavení. Vlastnosti |
| settings.configurationData.url |protectedSettings.DataBlobUri (bez tokenu SAS) |
| settings.privacy.dataEnabled |settings.Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings |nastavení. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments |protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken |settings.SasToken |
| protectedSettings.configurationDataUrlSasToken |Token SAS z protectedSettings.DataBlobUri |

## <a name="troubleshooting---error-code-1100"></a>Řešení potíží – kód chyby: 1100

Kód chyby: 1100 znamená problém s uživatelským vstupem do rozšíření DSC.
Text tyto chyby se liší a můžou změnit.
Tady jsou některé, které můžete narazit na chyby, a jak je opravit.

### <a name="invalid-values"></a>Neplatné hodnoty

"Je Privacy.dataCollection"{0}".
Pouze možné hodnoty jsou ","Zapnout"a"Zakázat"".
"Je WmfVersion"{0}".
Pouze možné hodnoty jsou... a 'nejnovější' ".

**Problém**: Zadaná hodnota není povolena.

**Řešení**: Neplatná hodnota změňte na platnou hodnotu.
Další informace najdete v tabulce v [podrobnosti](#details).

### <a name="invalid-url"></a>Neplatná adresa URL

"Je ConfigurationData.url"{0}". Toto není platná adresa URL"" je DataBlobUri "{0}". Toto není platná adresa URL"" je Configuration.url "{0}". Toto není platná adresa URL."

**Problém**: k dispozici A adresa URL není platná.

**Řešení**: Zkontrolujte zadaná URL.
Ujistěte se, že všechny adresy URL překládat na platné umístění, že rozšíření se dostanete na vzdáleném počítači.

### <a name="invalid-configurationargument-type"></a>Neplatný typ ConfigurationArgument

"Neplatný configurationArguments typ {0}"

**Problém**: *ConfigurationArguments* vlastnost nelze rozpoznat **zatřiďovací tabulky** objektu.

**Řešení**: Ujistěte se, vaši *ConfigurationArguments* vlastnost **zatřiďovací tabulky**.
Postupujte podle formátu najdete v předchozím příkladu. Podívejte se pro nabídky, čárky a závorky.

### <a name="duplicate-configurationarguments"></a>Duplicitní ConfigurationArguments

"Nalezena duplicitní argumenty{0}" ve veřejné a chráněné configurationArguments "

**Problém**: *ConfigurationArguments* v nastavení veřejné a *ConfigurationArguments* v nastavení chráněné mají vlastnosti se stejným názvem.

**Řešení**: odeberte jeden z duplicitních vlastností.

### <a name="missing-properties"></a>Chybí vlastnosti

"Configuration.function vyžaduje, aby byl zadán configuration.url nebo configuration.module"

"Configuration.url vyžaduje, že je zadán tento configuration.script"

"Configuration.script vyžaduje, že je zadán tento configuration.url"

"Configuration.url vyžaduje, že je zadán tento configuration.function"

"ConfigurationUrlSasToken vyžaduje, že je zadán tento configuration.url"

"ConfigurationDataUrlSasToken vyžaduje, že je zadán tento configurationData.url"

**Problém**: definované vlastnosti potřebuje další vlastnost, která se nenašel.

**Řešení**:

- Zadejte vlastnost chybí.
- Odeberte vlastnost, která potřebuje chybí vlastnost.

## <a name="next-steps"></a>Další postup

- Další informace o [pomocí virtuálního počítače škálovací sady pomocí rozšíření DSC Azure](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md).
- Najít další podrobnosti o [DSC. Správa zabezpečení přihlašovacích údajů](dsc-credentials.md).
- Získat [Úvod do obslužné rutiny rozšíření DSC Azure](dsc-overview.md).
- Další informace o prostředí PowerShell DSC, přejděte [centrum dokumentace k Powershellu](/powershell/dsc/overview).
