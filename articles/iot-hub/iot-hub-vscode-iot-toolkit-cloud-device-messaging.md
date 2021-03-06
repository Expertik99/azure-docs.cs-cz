---
title: Správa zařízení Azure IoT Hub cloudové zasílání zpráv pomocí rozšíření Azure IoT Toolkit pro Visual Studio Code | Dokumentace Microsoftu
description: Další informace o použití rozšíření Azure IoT Toolkit pro Visual Studio Code k monitorování zařízení, aby zprávy typu cloud a cloud posílat zprávy zařízení ve službě Azure IoT Hub.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 7/20/2018
ms.author: junhan
ms.openlocfilehash: 7bcb6eebdb6ceba6b07aeb19c1a74309fd4227d0
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/23/2018
ms.locfileid: "39206098"
---
# <a name="use-azure-iot-toolkit-extension-for-visual-studio-code-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Použití rozšíření Azure IoT Toolkit pro Visual Studio Code k odesílání a příjem zpráv mezi zařízením a centrem IoT

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/2.png)

[Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) je užitečná rozšíření Visual Studio Code, který usnadňuje správy ve službě IoT Hub. Tento článek se zaměřuje na tom, jak používat rozšíření Azure IoT Toolkit pro Visual Studio Code k odesílání a příjem zpráv mezi zařízením a centrem IoT.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="what-you-will-learn"></a>Co se dozvíte

Zjistíte, jak pomocí rozšíření Azure IoT Toolkit pro Visual Studio Code pro monitorování zpráv typu zařízení cloud a odesílat zprávy typu cloud zařízení. Zprávy typu zařízení cloud může být data ze senzorů, který shromažďuje vaše zařízení a pak odešle do služby IoT hub. Zprávy typu cloud zařízení může být příkazy, které služby IoT hub odesílá do vašeho zařízení blikání kontrolku LED, která je připojená k zařízení.

## <a name="what-you-will-do"></a>Co budete dělat

- Pomocí rozšíření Azure IoT Toolkit pro Visual Studio Code pro monitorování zpráv typu zařízení cloud.
- Pomocí rozšíření Azure IoT Toolkit pro Visual Studio Code pro odesílání zpráv typu cloud zařízení.

## <a name="what-you-need"></a>Co potřebujete

- Aktivní předplatné Azure.
- Azure IoT hub v rámci vašeho předplatného.
- [Visual Studio Code](https://code.visualstudio.com/)
- [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

## <a name="sign-in-to-access-your-iot-hub"></a>Přihlaste se ke službě IoT hub

1. V **Explorer** VS Code, rozbalte **zařízení Azure IoT Hub** části v levém dolním rohu.
1. Klikněte na tlačítko **vyberte centrum IoT** v kontextové nabídce.
1. Automaticky otevírané okno se zobrazí v pravém dolním rohu umožňuje při prvním přihlášení k Azure.
1. Po přihlášení se zobrazí seznam vašich předplatných Azure a potom vyberte předplatné Azure a IoT Hub.
1. Zobrazí se seznam zařízení v **zařízení Azure IoT Hub** kartu za pár sekund.

   > [!Note]
   > Umožňuje také dokončení sady nahoru výběrem **nastavit připojovací řetězec centra IoT**. Zadejte připojovací řetězec služby IoT hub, který připojí zařízení IoT k v místním okně.
   
## <a name="monitor-device-to-cloud-messages"></a>Monitorování zpráv typu zařízení cloud

Pokud chcete monitorovat zprávy odeslané ze zařízení do služby IoT hub, postupujte takto:

1. Klikněte pravým tlačítkem na zařízení a vyberte **spustit monitorování zpráv D2C**.
1. Sledované zprávy zobrazí ve **výstup** > **Azure IoT Toolkit** zobrazení.
1. Pokud chcete zastavit monitorování, klikněte pravým tlačítkem **výstup** zobrazit a vybrat **zastavit monitorování zpráv D2C**.

## <a name="send-cloud-to-device-messages"></a>Odesílání zpráv z cloudu do zařízení

Odeslat zprávu na vaše zařízení ze služby IoT hub, postupujte podle těchto kroků:

1. Klikněte pravým tlačítkem na zařízení a vyberte **C2D odeslat zprávu do zařízení**. 
1. Zadejte zprávu vstupního pole.
1. Výsledky se zobrazí v **výstup** > **Azure IoT Toolkit** zobrazení.

## <a name="next-steps"></a>Další postup

Jste zjistili, jak pro monitorování zpráv typu zařízení cloud a odesílání zpráv typu cloud zařízení mezi zařízení IoT a Azure IoT Hub.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
