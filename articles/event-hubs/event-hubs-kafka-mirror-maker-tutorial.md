---
title: Pomocí nástroje MirrorMaker Apache Kafka s Azure Event Hubs pro ekosystém Kafka | Dokumentace Microsoftu
description: Pomocí nástroje MirrorMaker Kafka zrcadlí cluster Kafka ve službě Event Hubs.
services: event-hubs
documentationcenter: .net
author: basilhariri
manager: timlt
ms.service: event-hubs
ms.topic: mirror-maker
ms.custom: mvc
ms.date: 08/07/2018
ms.author: bahariri
ms.openlocfilehash: f3881d4448f44d44515ddb25072401d775d69b90
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/23/2018
ms.locfileid: "42747184"
---
# <a name="use-kafka-mirrormaker-with-event-hubs-for-apache-kafka"></a>Pomocí nástroje MirrorMaker Kafka s Event Hubs pro Apache Kafka

Tento kurz ukazuje postupy při zrcadlení zprostředkovatele Kafka v rozbočovači povolená událost Kafka pomocí nástroje MirrorMaker Kafka.

   ![Nástroje MirrorMaker Kafka s Event Hubs](./media/event-hubs-kafka-mirror-maker-tutorial/evnent-hubs-mirror-maker1.png)

> [!NOTE]
> Tato ukázka je k dispozici na [GitHubu](https://github.com/Azure/azure-event-hubs).


V tomto kurzu se naučíte:
> [!div class="checklist"]
> * Vytvoření oboru názvů služby Event Hubs
> * Klonování projektu z příkladu
> * Nastavení clusteru Kafka
> * Konfigurace nástroje MirrorMaker Kafka
> * Spuštění nástroje MirrorMaker Kafka

## <a name="introduction"></a>Úvod
Jedním z hlavních faktorů pro moderní cloudové aplikace se možnost aktualizace, vylepšení a změna infrastruktury bez přerušení služby. Tento kurz ukazuje, jak povolené Kafka eventhub a nástroje MirrorMaker Kafka můžete integrovat existující kanál Kafka do Azure pomocí "zrcadlení" vstupního datového proudu Kafka ve službě Event Hubs. 

Koncový bod Azure Event Hubs Kafka umožňuje připojení k Azure Event Hubs pomocí protokolu Kafka (to znamená, že klienti Kafka). Tím, že minimální změny do aplikace Kafka, můžete připojit k Azure Event Hubs a mohli využívat výhody ekosystému Azure. Kafka povolena Služba Event Hubs aktuálně podporuje Kafka verze 1.0 nebo novější.

## <a name="prerequisites"></a>Požadavky

Abyste mohli absolvovat tento kurz, ujistěte se, že máte následující:

* Přečtěte si [Event Hubs pro Apache Kafka](event-hubs-for-kafka-ecosystem-overview.md) článku. 
* Předplatné Azure. Pokud ho nemáte, než začnete, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    * Na Ubuntu nainstalujte sadu JDK spuštěním příkazu `apt-get install default-jdk`.
    * Nezapomeňte nastavit proměnnou prostředí JAVA_HOME tak, aby odkazovala na složku, ve které je sada JDK nainstalovaná.
* [Stáhněte si](http://maven.apache.org/download.cgi) a [nainstalovat](http://maven.apache.org/install.html) binární archiv Maven
    * Na Ubuntu můžete Maven nainstalovat spuštěním příkazu `apt-get install maven`.
* [Git](https://www.git-scm.com/downloads)
    * Na Ubuntu můžete Git nainstalovat spuštěním příkazu `sudo apt-get install git`.

## <a name="create-an-event-hubs-namespace"></a>Vytvoření oboru názvů služby Event Hubs

Obor názvů služby Event Hubs je potřeba odesílat a přijímat z jakékoli služby Event Hubs. Zobrazit [vytváření Kafka povolené Centrum událostí](event-hubs-create.md) postup získání koncového bodu Event Hubs Kafka. Ujistěte se, že zkopírujte připojovací řetězec služby Event Hubs pro pozdější použití.

## <a name="clone-the-example-project"></a>Klonování projektu z příkladu

Teď, když máte Kafka povolené připojovací řetězec služby Event Hubs, naklonujte si úložiště služby Azure Event Hubs a přejděte `mirror-maker` podsložky:

```shell
git clone https://github.com/Azure/azure-event-hubs.git
cd azure-event-hubs/samples/kafka/mirror-maker
```

## <a name="set-up-a-kafka-cluster"></a>Nastavení clusteru Kafka

Použít [příručky rychlý start Kafka](https://kafka.apache.org/quickstart) nastavení clusteru s požadovaným nastavením (nebo použijte existujícího clusteru Kafka).

## <a name="configure-kafka-mirrormaker"></a>Konfigurace nástroje MirrorMaker Kafka

Nástroje MirrorMaker Kafka umožňuje "zrcadlení" datového proudu. Zadaný zdrojový a cílový clustery Kafka, nástroje MirrorMaker zajišťuje všechny zprávy odeslané na zdrojovém clusteru jsou přijímány zdrojový i cílový clustery. Tento příklad ukazuje, jak pro zdrojový cluster Kafka s centrem událostí podporující Kafka cíl zrcadlení. Tento scénář lze použít k odesílání dat do služby Event Hubs bez přerušení tok dat z existujícího kanálu Kafka. 

Podrobnější informace o Kafka nástroje MirrorMaker najdete v článku [Kafka zrcadlení nebo nástroje MirrorMaker průvodce](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330).

Ke konfiguraci nástroje MirrorMaker Kafka, pojmenujte ji clusteru Kafka jako příjemce/zdroj a Centrum povolené Kafka událostí jako svůj producenta/cíl.

#### <a name="consumer-configuration"></a>Konfigurace uživatelů

Aktualizovat konfigurační soubor příjemce `source-kafka.config`, který dává pokyn nástroje MirrorMaker vlastnosti zdroje clusteru Kafka.

##### <a name="source-kafkaconfig"></a>Zdroj kafka.config

```xml
bootstrap.servers={SOURCE.KAFKA.IP.ADDRESS1}:{SOURCE.KAFKA.PORT1},{SOURCE.KAFKA.IP.ADDRESS2}:{SOURCE.KAFKA.PORT2},etc
group.id=example-mirrormaker-group
exclude.internal.topics=true
client.id=mirror_maker_consumer
```

#### <a name="producer-configuration"></a>Konfigurace výrobce

Nyní aktualizovat konfigurační soubor producent `mirror-eventhub.config`, který dává pokyn nástroje MirrorMaker k odesílání dat duplicitní (nebo "zrcadlených") do služby Event Hubs. Konkrétně změnit `bootstrap.servers` a `sasl.jaas.config` tak, aby odkazoval na koncový bod služby Event Hubs Kafka. Služba Event Hubs vyžaduje zabezpečené komunikace (SASL), který je dosaženo pomocí nastavení poslední tři vlastnosti v následující konfiguraci: 

##### <a name="mirror-eventhubconfig"></a>eventhub.config zrcadlový svazek

```xml
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
client.id=mirror_maker_producer

#Required for Event Hubs
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

## <a name="run-kafka-mirrormaker"></a>Spuštění nástroje MirrorMaker Kafka

Spusťte skript nástroje MirrorMaker Kafka z kořenového adresáře Kafka pomocí nově aktualizovaná konfigurační soubory. Ujistěte se, že buď zkopírujte konfigurační soubory do kořenového adresáře Kafka ani aktualizovat svoje cesty v následujícím příkazu.

```shell
bin/kafka-mirror-maker.sh --consumer.config source-kafka.config --num.streams 1 --producer.config mirror-eventhub.config --whitelist=".*"
```

Pokud chcete ověřit, že události jsou tam dostupné pro Centrum událostí Kafka s podporou, zobrazit statistiku příchozího přenosu dat v [webu Azure portal](https://azure.microsoft.com/features/azure-portal/), nebo spusťte konzumenta proti centra událostí.

Při spuštění nástroje MirrorMaker žádné události odeslané do clusteru Kafka zdroje jsou přijímány clusteru Kafka a zrcadlených Kafka povolena služby Azure event hub. Pomocí nástroje MirrorMaker a koncový bod Event Hubs Kafka můžete migrovat existující kanál Kafka do spravované služby Azure Event Hubs bez změny stávajícího clusteru nebo by bylo třeba přerušit všechny probíhající datový tok.

## <a name="next-steps"></a>Další postup

V tomto kurzu se naučíte:
> [!div class="checklist"]
> * Vytvoření oboru názvů služby Event Hubs
> * Klonování projektu z příkladu
> * Nastavení clusteru Kafka
> * Konfigurace nástroje MirrorMaker Kafka
> * Spuštění nástroje MirrorMaker Kafka

Přejděte k dalším článku se dozvíte víc o službě Event Hubs pro Apache Kafka:

> [!div class="nextstepaction"]
> [Použití Apache Flink s Azure Event Hubs pro systém Kafka](event-hubs-kafka-flink-tutorial.md)