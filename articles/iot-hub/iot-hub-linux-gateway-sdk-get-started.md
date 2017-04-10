---
title: "Začínáme se sadou SDK Azure IoT Gateway (Linux) | Dokumentace Microsoftu"
description: "Postup vytvoření brány na počítači s Linuxem a popis klíčových konceptů v sadě SDK Azure IoT Gateway, jako jsou moduly a konfigurační soubory JSON."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: cf537bdd-2352-4bb1-96cd-a283fcd3d6cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 856ffeeeb8f9d8296ba972a9e070686171f7fde8
ms.lasthandoff: 03/31/2017


---
# <a name="explore-the-iot-gateway-sdk-architecture-on-linux"></a>Zkoumání architektury IoT Gateway SDK v Linuxu

[!INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Postup pro sestavení ukázky

Než začnete, musíte [připravit vývojové prostředí][lnk-setupdevbox] pro práci se sadou SDK v systému Linux.

1. Otevřete prostředí.
1. Přejděte do kořenové složky místní kopie úložiště **azure-iot-gateway-sdk**.
1. Spusťte skript **tools/build.sh**. Tento skript vytvoří v kořenové složce místní kopie úložiště **azure-iot-gateway-sdk** pomocí nástroje **cmake** složku s názvem **build** a vygeneruje soubor pravidel. Skript potom sestaví řešení a přeskočí při tom testy jednotek a celkové testy. Pokud chcete sestavit a spustit testy jednotek, přidejte parametr **--run-unittests**. Pokud chcete sestavit a spustit celkové testy, přidejte parametr **--run-e2e-tests**.

> [!NOTE]
> Pokaždé když spustíte skript **build.sh**, odstraní a potom znovu vytvoří složku **build** v kořenové složce místní kopie úložiště **azure-iot-gateway-sdk**.

## <a name="how-to-run-the-sample"></a>Spuštění ukázky

1. Skript **build.sh** generuje výstup ve složce **build** v místní kopii úložiště **azure-iot-gateway-sdk**. Tento výstup zahrnuje dva moduly používané v tomto příkladu.

    Skript sestavení umístí modul **liblogger.so** do složky **build/modules/logger/** a modul **libhello\_world.so** do složky **build/modules/hello_world/**. Tyto cesty použijte jako hodnotu **module path**, jak vidíte v následujícím ukázkovém souboru nastavení JSON.
1. Proces hello\_world\_sample přijímá jako argument příkazového řádku cestu ke konfiguračnímu souboru JSON. Následující ukázkový soubor JSON je součástí úložiště sady SDK v umístění **samples/hello\_world/src/hello\_world\_lin.json**. Tento konfigurační soubor bude fungovat, pokud jste neupravili skript sestavení, aby umístil moduly nebo ukázkové spustitelné soubory do jiných než výchozích umístění.

   > [!NOTE]
   > Cesty k modulům jsou relativní vzhledem k aktuálnímu pracovnímu adresáři, ze kterého je spuštěn spustitelný soubor hello\_world\_sample, ne k adresáři, ve kterém je spustitelný soubor umístěn. Ukázkový konfigurační soubor JSON ve výchozím nastavení zapíše do aktuálního pracovního adresáře soubor log.txt.

    ```json
    {
        "modules" :
        [
            {
              "name" : "logger",
              "loader": {
                "name": "native",
                "entrypoint": {
                  "module.path": "./modules/logger/liblogger.so"
                }
              },
              "args" : {"filename":"log.txt"}
            },
            {
                "name" : "hello_world",
              "loader": {
                "name": "native",
                "entrypoint": {
                  "module.path": "./modules/hello_world/libhello_world.so"
                }
              },
                "args" : null
            }
        ],
        "links":
        [
            {
                "source": "hello_world",
                "sink": "logger"
            }
        ]
    }
    ```
1. Přejděte do složky **build**.
1. Spusťte následující příkaz:

   `./samples/hello_world/hello_world_sample ./../samples/hello_world/src/hello_world_lin.json`

[!INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md

