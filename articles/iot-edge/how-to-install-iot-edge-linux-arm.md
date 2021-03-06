---
title: Instalace Azure IoT Edge v Linuxu ARM32 | Dokumentace Microsoftu
description: Azure IoT Edge pokyny k instalaci v Linuxu na ARM32 zařízeních, jako je Raspberry PI
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: kgremban
ms.openlocfilehash: 3f4e914f12feab3c36fca604c1bb37ab1a61b66f
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/28/2018
ms.locfileid: "43127221"
---
# <a name="install-azure-iot-edge-runtime-on-linux-arm32v7armhf"></a>Instalace modulu runtime Azure IoT Edge v Linuxu (ARM32v7/armhf)

Modul runtime Azure IoT Edge je co se změní na zařízení do zařízení IoT Edge. Modul runtime můžete nasadit na zařízení jako Raspberry Pi malé nebo velké průmyslové serveru. Jakmile je zařízení nakonfigurovaná s modulem runtime IoT Edge, můžete začít nasazovat obchodní logiky k němu z cloudu. 

Další informace o fungování modul runtime IoT Edge a jaké součásti jsou zahrnuty, naleznete v tématu [pochopení runtime Azure IoT Edge a jeho architektura](iot-edge-runtime.md).

Tento článek uvádí postup instalace modulu runtime Azure IoT Edge v Linuxu ARM32v7/armhf hraničním zařízení. Například tyto kroky by fungovalo pro Raspberry Pi zařízení. Odkazovat na [podpory Azure IoT Edge](support.md#operating-systems) seznam ARM32 operační systémy, které jsou aktuálně podporovány. 

>[!NOTE]
>Balíčky v úložiště softwaru Linux jsou souladu s licenčními podmínkami v jednotlivých balíčků (/ usr/sdílet/doc/*název balíčku*). Přečtěte si licenční podmínky před použitím balíčku. Instalace a použití balíčku se považuje za svůj souhlas s těmito podmínkami. Pokud s licenčními podmínkami nesouhlasíte, nepoužívejte balíček.

## <a name="install-the-container-runtime"></a>Nainstalovat modul runtime kontejneru

Azure IoT Edge se může spolehnout [OCI kompatibilní] [ lnk-oci] kontejner modulu runtime. Pro produkční scénáře, důrazně doporučujeme vám použít [na základě Moby] [ lnk-moby] níže uvedený modul. Je modul pouze container oficiálně podporované službou Azure IoT Edge. Image kontejnerů dockeru CE/EE jsou kompatibilní s modulem runtime na základě Moby.

Následujících příkazů nainstalujte základě Moby modul a rozhraní příkazového řádku (CLI). Rozhraní příkazového řádku je užitečné pro vývoj, ale volitelný pro nasazení v produkčním prostředí.

```bash

# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the moby-engine
curl -L https://aka.ms/moby-engine-armhf-latest -o moby_engine.deb && sudo dpkg -i ./moby_engine.deb

# Download and install the moby-cli
curl -L https://aka.ms/moby-cli-armhf-latest -o moby_cli.deb && sudo dpkg -i ./moby_cli.deb

# Run apt-get fix
sudo apt-get install -f

```

## <a name="install-the-iot-edge-security-daemon"></a>Instalace démona zabezpečení IoT Edge

**Démon zabezpečení IoT Edge** poskytuje a udržuje standardy zabezpečení na hraniční zařízení. Proces démon spustí při každé spuštění a bootstraps zařízení spuštěním rest modul runtime IoT Edge. 


```bash
# You can copy the entire text from this code block and 
# paste in terminal. The comment lines will be ignored.

# Download and install the standard libiothsm implementation
curl -L https://aka.ms/libiothsm-std-linux-armhf-latest -o libiothsm-std.deb && sudo dpkg -i ./libiothsm-std.deb

# Download and install the IoT Edge Security Daemon
curl -L https://aka.ms/iotedged-linux-armhf-latest -o iotedge.deb && sudo dpkg -i ./iotedge.deb

# Run apt-get fix
sudo apt-get install -f
```

## <a name="connect-your-device-to-an-iot-hub"></a>Připojení zařízení k IoT hubu 

Konfigurace modulu runtime IoT Edge k propojení vaší fyzické zařízení s identitou zařízení, která existuje v Azure IoT hub. 

Proces démon lze konfigurovat pomocí konfiguračního souboru na `/etc/iotedge/config.yaml`. Soubor je chráněn proti zápisu ve výchozím nastavení, bude pravděpodobně nutné zvýšenou úroveň oprávnění a upravte ji.

Jedno zařízení IoT Edge se dá zřídit ručně pomocí řetězce připojení zařízení k dispozici ve službě IoT Hub. Nebo můžete do služby Device Provisioning k automatickému zřízení zařízení, což je užitečné, když máte velký počet zařízení ke zřízení. Podle svého výběru: zřizování zvolte příslušný instalační skript. 

### <a name="option-1-manual-provisioning"></a>Možnost 1: Ruční zřizování

Ruční zřizování zařízení, budete muset zadat ho [připojovací řetězec zařízení] [ lnk-dcs] , můžete vytvořit tak, že zaregistrujete nového zařízení ve službě IoT hub.


Otevřete konfigurační soubor. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Vyhledejte část souboru zřizování a zrušte komentář **ruční** režim zřizování. Aktualizujte hodnotu **device_connection_string** připojovacím řetězcem z vašeho zařízení IoT Edge.

   ```yaml
   provisioning:
     source: "manual"
     device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   # provisioning: 
   #   source: "dps"
   #   global_endpoint: "https://global.azure-devices-provisioning.net"
   #   scope_id: "{scope_id}"
   #   registration_id: "{registration_id}"
   ```

Uložte soubor a zavřete ho. 

   `CTRL + X`, `Y`, `Enter`

Jakmile zadáte informace o zřizování v konfiguračním souboru, restartujte démona:

```bash
sudo systemctl restart iotedge
```

### <a name="option-2-automatic-provisioning"></a>Možnost 2: Automatické zřizování

K automatickému zřízení zařízení, [nastavení služby Device Provisioning a načtení ID registrace zařízení][lnk-dps]. Automatické zřizování funguje jenom u zařízení, která mají čipu Trusted Platform Module (TPM). Například zařízení Raspberry Pi nezahrnují TPM ve výchozím nastavení. 

Otevřete konfigurační soubor. 

```bash
sudo nano /etc/iotedge/config.yaml
```

Vyhledejte část souboru zřizování a zrušte komentář **dps** režim zřizování. Aktualizujte hodnoty **scope_id** a **registration_id** hodnotami ze služby IoT Hub Device Provisioning a zařízení IoT Edge s čipem TPM. 

   ```yaml
   # provisioning:
   #   source: "manual"
   #   device_connection_string: "<ADD DEVICE CONNECTION STRING HERE>"
  
   provisioning: 
     source: "dps"
     global_endpoint: "https://global.azure-devices-provisioning.net"
     scope_id: "{scope_id}"
     registration_id: "{registration_id}"
   ```

Uložte soubor a zavřete ho. 

   `CTRL + X`, `Y`, `Enter`

Jakmile zadáte informace o zřizování v konfiguračním souboru, restartujte démona:

```bash
sudo systemctl restart iotedge
```


## <a name="verify-successful-installation"></a>Ověření úspěšné instalace

Pokud jste použili **ruční konfigurace** kroků v předchozí části, modul runtime IoT Edge by měla být úspěšně zřízený a spuštěný ve vašem zařízení. Pokud jste použili **automatickou konfiguraci** kroky, pak budete muset provést některé další kroky, tak, aby modul runtime můžete registraci zařízení ve službě IoT hub vaším jménem. Další pokyny najdete v článku [vytvoření a zřízení simulovaného zařízení TPM Edge na virtuální počítač s Linuxem](how-to-auto-provision-simulated-device-linux.md#give-iot-edge-access-to-the-tpm).

Můžete zkontrolovat stav použití IoT Edge démon:

```bash
systemctl status iotedge
```

Zkontrolujte protokoly démonu pomocí:

```bash
journalctl -u iotedge --no-pager --no-full
```

A seznam s moduly:

```bash
sudo iotedge list
```

## <a name="tips-and-suggestions"></a>Tipy a návrhy

Ke spouštění příkazů `iotedge` potřebujete zvýšená oprávnění. Po instalaci modulu runtime, odhlaste se z vašeho počítače a znovu se přihlaste automaticky aktualizovat vaše oprávnění. Dokud to neuděláte, použijte **sudo** před všechny `iotedge` příkazy.

Na zařízeních prostředků omezené, důrazně doporučujeme, abyste nastavili *OptimizeForPerformance* proměnnou prostředí, aby *false* podle pokynů v [Průvodce odstraňováním potíží ][lnk-trouble].

## <a name="next-steps"></a>Další postup

Pokud máte problémy s modulu runtime Edge instalaci správně, podívejte se na [řešení potíží s] [ lnk-trouble] stránky.

<!-- Links -->
[lnk-dcs]: how-to-register-device-portal.md
[lnk-dps]: how-to-auto-provision-simulated-device-linux.md
[lnk-trouble]: https://docs.microsoft.com/azure/iot-edge/troubleshoot#stability-issues-on-resource-constrained-devices
[lnk-oci]: https://www.opencontainers.org/
[lnk-moby]: https://mobyproject.org/
