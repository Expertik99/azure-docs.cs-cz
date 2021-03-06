---
title: Dotazování na Avro data pomocí Azure Data Lake Analytics | Dokumentace Microsoftu
description: Pomocí vlastnosti text zprávy směrovat do úložiště objektů Blob v telemetrii zařízení a zadávat dotazy na data formátu Avro, která jsou zapsána do úložiště objektů Blob.
author: ash2017
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: asrastog
ms.openlocfilehash: a17df39c55b5c02c83e3f0b74a91d7109ddb4d3d
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2018
ms.locfileid: "43188940"
---
# <a name="query-avro-data-by-using-azure-data-lake-analytics"></a>Dotazování na Avro data pomocí Azure Data Lake Analytics

Tento článek popisuje, jak dotazovat data Avro můžete efektivně směrovat zprávy ze služby Azure IoT Hub ke službám Azure. Jak jsme oznámili v blogovém příspěvku [Směrování zpráv v Azure IoT Hub: nyní se směrováním zprávy], IoT Hub podporuje směrování na vlastnosti nebo zprávy. Další informace najdete v tématu [směrování zpráv][Routing on message bodies]. 

Před obrovskou výzvou – dochází po, který Azure IoT Hub provádí směrování zpráv do služby Azure Blob storage, služby IoT Hub zapíše obsah ve formátu Avro, který má do zprávy vlastnost text i vlastnost zprávy. IoT Hub pouze v datovém formátu Avro podporuje zápis dat do úložiště objektů Blob a tento formát se používá pro všechny ostatní koncové body. Další informace najdete v tématu [při použití služby Azure Storage kontejnery][When using Azure storage containers]. Ačkoli formát Avro se skvěle hodí pro zachování dat a zpráva, představuje výzvu ji používat k dotazování na data. Porovnání je mnohem jednodušší pro dotazování na data ve formátu JSON nebo CSV.

Adres nerelačních potřebám velkých objemů dat a formátů a tento problém vyřešili, můžete použít mnoho vzorků velkých objemů dat pro transformaci a škálování data. Jednomu ze vzorů, "platit podle počtu dotazů," je Azure Data Lake Analytics, který je hlavním cílem tohoto článku. I když v Hadoop nebo jiná řešení můžete snadno spustit dotaz, Data Lake Analytics je často vhodnější pro tento přístup "platba za dotaz". 

Není k dispozici "Extraktor" pro Avro v U-SQL. Další informace najdete v tématu [Příklad Avro U-SQL].

## <a name="query-and-export-avro-data-to-a-csv-file"></a>Dotazování a Avro data exportovat do souboru CSV
V této části dotazování na Avro data a exportujte ho do souboru CSV v úložišti objektů Blob v Azure, i když může snadno umístit data v jiných úložištích nebo dat úložiště.

1. Nastavení služby Azure IoT Hub pro data trasy pro koncový bod úložiště objektů Blob v Azure pomocí vlastnosti v textu zprávy k výběru zprávy.

    ![V části "Vlastní koncové body"][img-query-avro-data-1a]

    ![Příkaz trasy][img-query-avro-data-1b]

2. Ujistěte se, že vaše zařízení má kódování, typu obsahu a potřebná data ve vlastnosti nebo zprávy, jak je uvedeno v dokumentaci k produktu. Při zobrazení těchto atributů v Device Explorer, jak je znázorněno zde můžete ověřit, že jsou nastavené správně.

    ![V podokně Data centra událostí][img-query-avro-data-2]

3. Nastavte instanci Azure Data Lake Store a Data Lake Analytics instance. Azure IoT Hub nesměruje do Data Lake Store instance, ale instance Data Lake Analytics vyžaduje.

    ![Data Lake Store a Data Lake Analytics][img-query-avro-data-3]

4. V Data Lake Analytics nakonfigurujte jako další úložiště, stejné úložiště objektů Blob, který směruje Azure IoT Hub dat do úložiště objektů Blob v Azure.

    ![V podokně "Zdroje dat"][img-query-avro-data-4]
 
5. Jak je popsáno v [Příklad Avro U-SQL], budete potřebovat čtyři knihovny DLL. Tyto soubory nahrajte do umístění ve vaší instanci Data Lake Store.

    ![Čtyři nahraných souborů knihovny DLL][img-query-avro-data-5] 

6. V sadě Visual Studio vytvořte projekt U-SQL.
 
    ![Vytvořte projekt U-SQL][img-query-avro-data-6]

7. Vložte obsah následujícího skriptu do nově vytvořený soubor. Upravit tři zvýrazněné sekce: účtu Data Lake Analytics, cesty k souborům přidružené knihovny DLL a správnou cestu k účtu úložiště.
    
    ![Tři části má být upraven][img-query-avro-data-7a]

    Skutečné skript U-SQL pro jednoduché výstup do souboru CSV:
    
    ```sql
        DROP ASSEMBLY IF EXISTS [Avro];
        CREATE ASSEMBLY [Avro] FROM @"/Assemblies/Avro/Avro.dll";
        DROP ASSEMBLY IF EXISTS [Microsoft.Analytics.Samples.Formats];
        CREATE ASSEMBLY [Microsoft.Analytics.Samples.Formats] FROM @"/Assemblies/Avro/Microsoft.Analytics.Samples.Formats.dll";
        DROP ASSEMBLY IF EXISTS [Newtonsoft.Json];
        CREATE ASSEMBLY [Newtonsoft.Json] FROM @"/Assemblies/Avro/Newtonsoft.Json.dll";
        DROP ASSEMBLY IF EXISTS [log4net];
        CREATE ASSEMBLY [log4net] FROM @"/Assemblies/Avro/log4net.dll";

        REFERENCE ASSEMBLY [Newtonsoft.Json];
        REFERENCE ASSEMBLY [log4net];
        REFERENCE ASSEMBLY [Avro];
        REFERENCE ASSEMBLY [Microsoft.Analytics.Samples.Formats];

        // Blob container storage account filenames, with any path
        DECLARE @input_file string = @"wasb://hottubrawdata@kevinsayazstorage/kevinsayIoT/{*}/{*}/{*}/{*}/{*}/{*}";
        DECLARE @output_file string = @"/output/output.csv";

        @rs =
        EXTRACT
        EnqueuedTimeUtc string,
        Body byte[]
        FROM @input_file

        USING new Microsoft.Analytics.Samples.Formats.ApacheAvro.AvroExtractor(@"
        {
        ""type"":""record"",
        ""name"":""Message"",
        ""namespace"":""Microsoft.Azure.Devices"",
        ""fields"":[{
        ""name"":""EnqueuedTimeUtc"",
        ""type"":""string""
        },
        {
        ""name"":""Properties"",
        ""type"":{
        ""type"":""map"",
        ""values"":""string""
        }
        },
        {
        ""name"":""SystemProperties"",
        ""type"":{
        ""type"":""map"",
        ""values"":""string""
        }
        },
        {
        ""name"":""Body"",
        ""type"":[""null"",""bytes""]
        }
        ]
        }");

        @cnt =
        SELECT EnqueuedTimeUtc AS time, Encoding.UTF8.GetString(Body) AS jsonmessage
        FROM @rs;

        OUTPUT @cnt TO @output_file USING Outputters.Text(); 
    ```    

    Data Lake Analytics, kterou trvalo 5 minut, spusťte následující skript, která byla omezena na 10 jednotek analýzy a zpracování 177 soubory. Výsledek se zobrazí ve výstupu souboru CSV, který se zobrazí na následujícím obrázku:
    
    ![Výsledky výstupu do souboru CSV][img-query-avro-data-7b]

    ![Výstup převodu do souboru CSV][img-query-avro-data-7c]

    Parsovat JSON, pokračujte krokem 8.
    
8. Většina IoT zprávy jsou ve formátu JSON. Přidejte následující řádky, je možné analyzovat zprávu do souboru JSON, která vám umožní přidat klauzule WHERE a výstup pouze potřebná data.

    ```sql
       @jsonify = SELECT Microsoft.Analytics.Samples.Formats.Json.JsonFunctions.JsonTuple(Encoding.UTF8.GetString(Body)) AS message FROM @rs;
    
        /*
        @cnt =
            SELECT EnqueuedTimeUtc AS time, Encoding.UTF8.GetString(Body) AS jsonmessage
            FROM @rs;
        
        OUTPUT @cnt TO @output_file USING Outputters.Text();
        */
        
        @cnt =
            SELECT message["message"] AS iotmessage,
                   message["event"] AS msgevent,
                   message["object"] AS msgobject,
                   message["status"] AS msgstatus,
                   message["host"] AS msghost
            FROM @jsonify;
            
        OUTPUT @cnt TO @output_file USING Outputters.Text();
    ```

    Výstup zobrazuje sloupec pro každou položku v `SELECT` příkazu. 
    
    ![Výstup zobrazuje sloupec pro každou položku][img-query-avro-data-8]

## <a name="next-steps"></a>Další postup
V tomto kurzu jste zjistili, jak zadávat dotazy na data Avro můžete efektivně směrovat zprávy ze služby Azure IoT Hub ke službám Azure.

Příklady kompletní řešení začátku do konce, které používají služby IoT Hub, najdete v článku [akcelerátoru řešení vzdáleného monitorování Azure IoT][lnk-iot-sa-land].

Další informace o vývoji řešení s využitím služby IoT Hub, najdete v článku [Příručka vývojáře pro IoT Hub].

Další informace o směrování zpráv ve službě IoT Hub, najdete v článku [odesílání a příjem zpráv pomocí služby IoT Hub][lnk-devguide-messaging].

<!-- Images -->
[img-query-avro-data-1a]: ./media/iot-hub-query-avro-data/query-avro-data-1a.png
[img-query-avro-data-1b]: ./media/iot-hub-query-avro-data/query-avro-data-1b.png
[img-query-avro-data-2]: ./media/iot-hub-query-avro-data/query-avro-data-2.png
[img-query-avro-data-3]: ./media/iot-hub-query-avro-data/query-avro-data-3.png
[img-query-avro-data-4]: ./media/iot-hub-query-avro-data/query-avro-data-4.png
[img-query-avro-data-5]: ./media/iot-hub-query-avro-data/query-avro-data-5.png
[img-query-avro-data-6]: ./media/iot-hub-query-avro-data/query-avro-data-6.png
[img-query-avro-data-7a]: ./media/iot-hub-query-avro-data/query-avro-data-7a.png
[img-query-avro-data-7b]: ./media/iot-hub-query-avro-data/query-avro-data-7b.png
[img-query-avro-data-7c]: ./media/iot-hub-query-avro-data/query-avro-data-7c.png
[img-query-avro-data-8]: ./media/iot-hub-query-avro-data/query-avro-data-8.png

<!-- Links -->
[Směrování zpráv v Azure IoT Hub: nyní se směrováním zprávy]: https://azure.microsoft.com/blog/iot-hub-message-routing-now-with-routing-on-message-body/

[Routing on message bodies]: iot-hub-devguide-query-language.md#routing-on-message-bodies
[When using Azure storage containers]:iot-hub-devguide-endpoints.md#when-using-azure-storage-containers

[Příklad Avro U-SQL]:https://github.com/Azure/usql/tree/master/Examples/AvroExamples

[lnk-iot-sa-land]: ../iot-accelerators/index.yml
[Příručka vývojáře pro IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
