---
title: zahrnout soubor
description: zahrnout soubor
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 5702c6e9c9d75c6cccb82f1c57684ef7b9898c34
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/01/2018
ms.locfileid: "34665996"
---
## <a name="view-device-telemetry"></a>Zobrazení telemetrie zařízení

Můžete zobrazit telemetrická data odesílaná ze zařízení **zařízení** stránky v řešení.

1. Vyberte zařízení, které jsou zřízené v seznamu zařízení na **zařízení** stránky. Panelu se zobrazí informace o vašem zařízení, včetně vykreslení telemetrie zařízení:

    ![Najdete v části Podrobnosti o zařízení](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Zvolte **přetížení** ke změně zobrazení telemetrie:

    ![Zobrazení telemetrie přetížení](media/iot-suite-visualize-connecting/devicespressure.png)

1. Chcete-li zobrazit diagnostické informace o vašem zařízení, přejděte dolů na **diagnostiky**:

    ![Zobrazení zařízení diagnostiky](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Fungovat na zařízení

Chcete-li volat metody na zařízení, použijte **zařízení** stránky v řešení vzdáleného monitorování. Například v řešení vzdáleného monitorování **chladič** zařízení implementovat **FirmwareUpdate** metoda.

1. Zvolte **zařízení** přejděte na **zařízení** stránky v řešení.

1. Vyberte zařízení, které jsou zřízené v seznamu zařízení na **zařízení** stránky:

    ![Vyberte fyzické zařízení](media/iot-suite-visualize-connecting/devicesselect.png)

1. Chcete-li zobrazit seznam metod, můžete volat na vašem zařízení, zvolte **úlohy**, pak **Run – metoda**. Naplánování úlohy ke spuštění na několika zařízeních, můžete vybrat více zařízení v seznamu. **Úlohy** panelu zobrazí požadované typy metoda společné pro všechny vámi vybraná zařízení.

1. Zvolte **FirmwareUpdate**, nastavte název úlohy na **UpdatePhysicalChiller**. Nastavit **verzi firmwaru** k **2.0.0**, nastavte **Firmware URI** k **http://contoso.com/updates/firmware.bin**a potom zvolte **použít**:

    ![Plán aktualizace firmwaru](media/iot-suite-visualize-connecting/deviceschedule.png)

1. Posloupnost zprávy zobrazí v konzole zařízení kód spuštěný, zatímco metoda zpracovává simulované zařízení.

1. Po dokončení aktualizace na novou verzi firmwaru zobrazí na **zařízení** stránky:

    ![Aktualizace byla dokončena](media/iot-suite-visualize-connecting/complete.png)

> [!NOTE]
> Chcete-li sledovat stav úlohy v řešení, zvolte **zobrazení**.

## <a name="next-steps"></a>Další postup

Článek [přizpůsobit akcelerátoru řešení vzdáleného monitorování](../articles/iot-accelerators/iot-accelerators-remote-monitoring-customize.md) popisuje několik způsobů, jak přizpůsobit akcelerátoru řešení.