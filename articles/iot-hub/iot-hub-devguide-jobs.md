---
title: Vysvětlení úloh služby Azure IoT Hub | Dokumentace Microsoftu
description: Příručka pro vývojáře – plánování spouštění úloh v různých zařízeních připojené ke službě IoT hub. Úlohy můžete aktualizovat značky a požadované vlastnosti a vyvolání přímých metod na více zařízeních.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: e2beec1308b9664d35ccd9d355403b7076567f2f
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/23/2018
ms.locfileid: "42745840"
---
# <a name="schedule-jobs-on-multiple-devices"></a>Plánování úloh na několika zařízeních

Azure IoT Hub umožňuje pomocí stavebních bloků, jako je číslo [vlastnosti dvojčat zařízení a značky] [ lnk-twin-devguide] a [přímé metody][lnk-dev-methods]. Back endové aplikace obvykle povolit zařízení správci a operátoři, aktualizovat a komunikovat se zařízeními IoT hromadně a v naplánovaném čase.  Úlohy spustit aktualizace dvojčat zařízení nebo přímých metod pro skupiny zařízení v naplánovaném čase.  Operátor byste například použili back endové aplikace, který iniciuje a sleduje úlohu restartujte sadu zařízení v budově 43 a podlaží 3 v čase, který nebude působit rušivě pro operace vytvoření.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Zvažte použití úloh když budete potřebovat k plánování a sledování průběhu některý z následujících aktivit na skupiny zařízení:

* Aktualizace požadovaných vlastností
* Aktualizace značky
* Vyvolání přímých metod

## <a name="job-lifecycle"></a>Životní cyklus úlohy
Úlohy jsou iniciovaných back-end řešení a spravuje pomocí služby IoT Hub.  Můžete spustit úlohu prostřednictvím služby přístupem k identifikátoru URI (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) a dotazování na průběh provádění úlohy prostřednictvím služby přístupem k identifikátoru URI (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`). K aktualizaci stavu spuštěných úloh po zahájení úlohy, spusťte dotaz úlohy.

> [!NOTE]
> Když spustíte úlohu, názvy a hodnoty vlastností může obsahovat pouze US-ASCII tisknutelný alfanumerické znaky, s výjimkou těch v následující sadě: `$ ( ) < > @ , ; : \ " / [ ] ? = { } SP HT`.

## <a name="jobs-to-execute-direct-methods"></a>Úlohy spouštějí přímé metody
Následující fragment kódu ukazuje podrobnosti o žádosti HTTPS 1.1 pro spouštění [přímá metoda] [ lnk-dev-methods] na sadu zařízení pomocí úlohy:

    PUT /jobs/v2/<jobId>?api-version=2016-11-14

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            responseTimeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }

Podmínka dotazu může být také na jedno zařízení ID nebo na seznam zařízení ID, jak je znázorněno v následujícím příkladu:

```
queryCondition = "deviceId = 'MyDevice1'"
queryCondition = "deviceId IN ['MyDevice1','MyDevice2']"
queryCondition = "deviceId IN ['MyDevice1']
```
[Dotazovací jazyk IoT Hub] [ lnk-query] pokrývá dotazovací jazyk služby IoT Hub v dalších podrobností.

## <a name="jobs-to-update-device-twin-properties"></a>Úlohy se aktualizovat vlastnosti dvojčat zařízení
Následující fragment kódu ukazuje podrobnosti o žádosti HTTPS 1.1 pro aktualizaci vlastnosti dvojčat zařízení pomocí úlohy:

    PUT /jobs/v2/<jobId>?api-version=2016-11-14
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }

## <a name="querying-for-progress-on-jobs"></a>Dotazování na průběh úlohy
Následující fragment kódu ukazuje podrobnosti o žádosti HTTPS 1.1 pro dotazování pro úlohy:

    GET /jobs/v2/query?api-version=2016-11-14[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

Token continuationToken je k dispozici z odpovědi.  

Můžete zadat dotaz na stav provádění úlohy na každé zařízení pomocí [dotazovací jazyk služby IoT Hub pro dvojčata zařízení, úlohy a směrování zpráv][lnk-query].

## <a name="jobs-properties"></a>Vlastnosti úlohy
Následující seznam obsahuje vlastnosti a odpovídající popisy, které je možné použít při dotazování pro úlohy nebo výsledky úlohy.

| Vlastnost | Popis |
| --- | --- |
| **ID úlohy** |Aplikace uvedené ID pro úlohu. |
| **startTime** |Aplikace zadaný počáteční čas (ISO-8601) pro konkrétní úlohu. |
| **endTime** |IoT Hub poskytuje data (ISO-8601) pro dokončení úlohy. Platné až po úloze dosáhne stavu "dokončení". |
| **type** |Typy úloh: |
| | **scheduledUpdateTwin**: úloha používá k aktualizaci sadu požadovaných vlastností nebo značky. |
| | **scheduledDeviceMethod**: úloha použít k vyvolání metody zařízení na sadu dvojčata zařízení. |
| **Stav** |Aktuální stav úlohy. Možné hodnoty pro stav: |
| | **Čekající**: naplánované a čeká, aby ji mohla služba úloh. |
| | **naplánované**: naplánováno na dobu v budoucnu. |
| | **spuštění**: aktuálně aktivní úloha. |
| | **bylo zrušeno**: Úloha byla zrušena. |
| | **nepovedlo**: zpracování úlohy se nezdařilo. |
| | **dokončení**: Úloha byla dokončena. |
| **deviceJobStatistics** |Statistika týkající se spuštění úlohy. |
| | **deviceJobStatistics** vlastnosti: |
| | **deviceJobStatistics.deviceCount**: počet zařízení v rámci úlohy. |
| | **deviceJobStatistics.failedCount**: počet zařízení, kde úloha se nezdařila. |
| | **deviceJobStatistics.succeededCount**: počet zařízení, ve kterém úloha úspěšně. |
| | **deviceJobStatistics.runningCount**: počet zařízení, které jsou aktuálně spuštěné úlohy. |
| | **deviceJobStatistics.pendingCount**: počet zařízení, která čekají na spuštění úlohy. |

### <a name="additional-reference-material"></a>Další referenční materiál
Další referenční témata v příručce pro vývojáře IoT Hub patří:

* [Koncové body IoT Hubu] [ lnk-endpoints] popisuje různé koncové body, které každý IoT hub zpřístupní pro operace za běhu a správy.
* [Omezování a kvótách] [ lnk-quotas] popisuje kvóty, které platí pro službu IoT Hub a omezování chování očekávat, když používáte službu.
* [Azure IoT zařízení a služby sady SDK] [ lnk-sdks] uvádí různé jazykové sady SDK můžete použít při vývoji aplikace s zařízení i služby, které pracují s centrem IoT.
* [Dotazovací jazyk služby IoT Hub pro dvojčata zařízení, úlohy a směrování zpráv] [ lnk-query] popisuje dotazovací jazyk služby IoT Hub. Použijte tento dotazovací jazyk k načtení informací ze služby IoT Hub o dvojčata zařízení a úlohy.
* [Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokolu MQTT.

## <a name="next-steps"></a>Další postup
Vyzkoušet si některé koncepty popsané v tomto článku, najdete v následujícím kurzu služby IoT Hub:

* [Úlohy vysílání a plánování][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-node-node-schedule-jobs.md
[lnk-c2d-methods]: quickstart-control-device-node.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
