---
title: Nahrání souborů ze zařízení do služby Azure IoT Hub pomocí Javy | Dokumentace Microsoftu
description: Postup nahrání souborů ze zařízení do cloudu pomocí zařízení Azure IoT SDK pro Javu. Nahrané soubory se ukládají v kontejneru objektů blob v Azure storage.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: java
ms.topic: conceptual
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: 7eb18a623683fb8a3662297825e90234644ead39
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/21/2018
ms.locfileid: "42060125"
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>Nahrání souborů ze zařízení do cloudu pomocí služby IoT Hub

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

V tomto kurzu vychází z kódu v [odesílat zprávy typu Cloud-zařízení pomocí služby IoT Hub](iot-hub-java-java-c2d.md) kurzu se dozvíte, jak používat [soubor nahrát možnosti služby IoT Hub](iot-hub-devguide-file-upload.md) nahrát soubor do [objektů blob v Azure úložiště](../storage/index.yml). V tomto kurzu získáte informace o následujících postupech:

- Zabezpečeně dodávají zařízení s Azure blob identifikátorů URI pro nahrání souboru.
- Oznámení o nahrávání souborů služby IoT Hub použijte k aktivaci zpracování souboru v back-endu aplikace.

[Začínáme se službou IoT Hub](quickstart-send-telemetry-java.md) a [odesílat zprávy typu Cloud-zařízení pomocí služby IoT Hub](iot-hub-java-java-c2d.md) kurzy vám ukážou základní funkce typu zařízení cloud a cloud zařízení zasílání zpráv služby IoT Hub. [Zprávy procesu zařízení-Cloud](tutorial-routing.md) kurz popisuje způsob, jak spolehlivě ukládat zprávy typu zařízení cloud ve službě Azure blob storage. Nicméně v některých scénářích nelze mapovat snadno data, která vaše zařízení odesílají do poměrně málo početnému zpráv typu zařízení cloud, které služby IoT Hub přijímá. Příklad:

* Velké soubory, které obsahují obrázky
* Videa
* Data pronikavost odebírána data v vysoká frekvence
* Určitou formu předzpracovaná data.

Tyto soubory jsou obvykle dávkově zpracovány v cloudu pomocí nástrojů, jako [Azure Data Factory](../data-factory/introduction.md) nebo [Hadoop](../hdinsight/index.yml) zásobníku. Když budete potřebovat hornatých souborů ze zařízení, můžete stále použít zabezpečení a spolehlivost služby IoT Hub.

Na konci tohoto kurzu spustíte dvě konzolové aplikace Java:

* **simulated-device**, upravenou verzi aplikaci vytvořenou v kurzu [zpráv odesílání typu Cloud-zařízení pomocí služby IoT Hub]. Tato aplikace nahraje soubor do služby storage pomocí SAS URI poskytované služby IoT hub.
* **čtení souboru odesílání oznámení**, který obdrží oznámení o nahrávání souborů ze služby IoT hub.

> [!NOTE]
> IoT Hub podporuje mnoho platforem zařízení a jazyků (včetně C, .NET a Javascript) prostřednictvím sady SDK pro zařízení Azure IoT. Odkazovat [centrum pro vývojáře Azure IoT] podrobné pokyny o tom, jak připojit zařízení ke službě Azure IoT Hub.

Pro absolvování tohoto kurzu potřebujete:

* Nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Aktivní účet Azure. (Pokud účet nemáte, můžete vytvořit [bezplatný účet](http://azure.microsoft.com/pricing/free-trial/) během několika minut.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Nahrajte soubor z aplikace pro zařízení

V této části upravíte zařízení aplikaci, kterou jste vytvořili v [odesílat zprávy typu Cloud-zařízení pomocí služby IoT Hub](iot-hub-java-java-c2d.md) k nahrání souboru do služby IoT hub.

1. Kopírování souboru obrázku, který `simulated-device` složku a přejmenujte jej `myimage.png`.

1. Pomocí textového editoru, otevřete `simulated-device\src\main\java\com\mycompany\app\App.java` souboru.

1. Přidat deklaraci proměnných na **aplikace** třídy:

    ```java
    private static String fileName = "myimage.png";
    ```

1. Ke zpracování zprávy zpětné volání stav nahrávání souborů, přidáním následující vnořené třídy **aplikace** třídy:

    ```java
    // Define a callback method to print status codes from IoT Hub.
    protected static class FileUploadStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to file upload for " + fileName
            + " operation with status " + status.name());
      }
    }
    ```

1. Můžete nahrávat obrázky do služby IoT Hub, přidejte následující metodu do **aplikace** třídy k nahrání Image do služby IoT Hub:

    ```java
    // Use IoT Hub to upload a file asynchronously to Azure blob storage.
    private static void uploadFile(String fullFileName) throws FileNotFoundException, IOException
    {
      File file = new File(fullFileName);
      InputStream inputStream = new FileInputStream(file);
      long streamLength = file.length();

      client.uploadToBlobAsync(fileName, inputStream, streamLength, new FileUploadStatusCallBack(), null);
    }
    ```

1. Upravit **hlavní** metoda se má volat **uploadFile** způsob, jak je znázorněno v následujícím fragmentu kódu:

    ```java
    client.open();

    try
    {
      // Get the filename and start the upload.
      String fullFileName = System.getProperty("user.dir") + File.separator + fileName;
      uploadFile(fullFileName);
      System.out.println("File upload started with success");
    }
    catch (Exception e)
    {
      System.out.println("Exception uploading file: " + e.getCause() + " \nERROR: " + e.getMessage());
    }

    MessageSender sender = new MessageSender();
    ```

1. Použijte následující příkaz k sestavení **simulated-device** aplikace a zkontrolujte chyby:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="receive-a-file-upload-notification"></a>Přijímat oznámení o nahrání souborů

V této části vytvoříte konzolovou aplikaci Java, která bude přijímat zprávy oznámení nahrávání souborů ze služby IoT Hub.

Je nutné **iothubowner** připojovací řetězec služby IoT hub k dokončení této části. Připojovací řetězec můžete najít [webu Azure portal](https://portal.azure.com/) na **zásady sdíleného přístupu** okno.

1. Vytvořte projekt Maven s názvem **čtení souboru odesílání oznámení** pomocí následujícího příkazu na příkazovém řádku. Všimněte si, že tento příkaz je jeden dlouhý příkaz:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-file-upload-notification -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

1. Na příkazovém řádku přejděte do nové `read-file-upload-notification` složky.

1. Pomocí textového editoru otevřete `pom.xml` soubor `read-file-upload-notification` složky a přidejte následující závislost **závislosti** uzlu. Přidat závislost vám umožní použít **iothub-java-service-client** balíčku v aplikaci komunikovat s vaší službou IoT hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > Můžete vyhledat nejnovější verzi **iot-service-client** pomocí [vyhledávání Maven](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Uložte a zavřete `pom.xml` souboru.

1. Pomocí textového editoru, otevřete `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` souboru.

1. Do souboru přidejte následující příkazy pro **import**:

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    ```

1. Přidejte následující proměnné na úrovni třídy pro **aplikace** třídy:

    ```java
    private static final String connectionString = "{Your IoT Hub connection string}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    private static FileUploadNotificationReceiver fileUploadNotificationReceiver = null;
    ```

1. Můžete vytisknout tak informace o nahrávání souborů do konzoly, přidáním následující vnořené třídy **aplikace** třídy:

    ```java
    // Create a thread to receive file upload notifications.
    private static class ShowFileUploadNotifications implements Runnable {
      public void run() {
        try {
          while (true) {
            System.out.println("Recieve file upload notifications...");
            FileUploadNotification fileUploadNotification = fileUploadNotificationReceiver.receive();
            if (fileUploadNotification != null) {
              System.out.println("File Upload notification received");
              System.out.println("Device Id : " + fileUploadNotification.getDeviceId());
              System.out.println("Blob Uri: " + fileUploadNotification.getBlobUri());
              System.out.println("Blob Name: " + fileUploadNotification.getBlobName());
              System.out.println("Last Updated : " + fileUploadNotification.getLastUpdatedTimeDate());
              System.out.println("Blob Size (Bytes): " + fileUploadNotification.getBlobSizeInBytes());
              System.out.println("Enqueued Time: " + fileUploadNotification.getEnqueuedTimeUtcDate());
            }
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

1. Spustit vlákno, který naslouchá oznámení o nahrávání souborů, přidejte následující kód, který **hlavní** metody:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(connectionString, protocol);

      if (serviceClient != null) {
        serviceClient.open();

        // Get a file upload notification receiver from the ServiceClient.
        fileUploadNotificationReceiver = serviceClient.getFileUploadNotificationReceiver();
        fileUploadNotificationReceiver.open();

        // Start the thread to receive file upload notifications.
        ShowFileUploadNotifications showFileUploadNotifications = new ShowFileUploadNotifications();
        ExecutorService executor = Executors.newFixedThreadPool(1);
        executor.execute(showFileUploadNotifications);

        System.out.println("Press ENTER to exit.");
        System.in.read();
        executor.shutdownNow();
        System.out.println("Shutting down sample...");
        fileUploadNotificationReceiver.close();
        serviceClient.close();
      }
    }
    ```

1. Uložte a zavřete `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` souboru.

1. Použijte následující příkaz k sestavení **čtení souboru odesílání oznámení** aplikace a zkontrolujte chyby:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>Spuštění aplikací

Nyní můžete spustit aplikace.

Na příkazovém řádku v `read-file-upload-notification` složky, spusťte následující příkaz:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

Na příkazovém řádku v `simulated-device` složky, spusťte následující příkaz:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

Následující snímek obrazovky ukazuje výstup z **simulated-device** aplikace:

![Výstup z aplikace simulovaného zařízení](media/iot-hub-java-java-upload/simulated-device.png)

Následující snímek obrazovky ukazuje výstup z **čtení souboru odesílání oznámení** aplikace:

![Výstup z aplikace pro čtení souboru odeslání oznámení](media/iot-hub-java-java-upload/read-file-upload-notification.png)

Na portálu můžete použít k zobrazení nahraných souborů v kontejneru úložiště, které jste nakonfigurovali:

![Nahraný soubor](media/iot-hub-java-java-upload/uploaded-file.png)

## <a name="next-steps"></a>Další postup

V tomto kurzu jste zjistili, jak zjednodušit nahrávání souborů ze zařízení pomocí možnosti nahrávání souborů služby IoT Hub. Můžete pokračovat k prozkoumání funkcí služby IoT hub a scénáře najdete v následujících článcích:

* [Vytvoření centra IoT prostřednictvím kódu programu][lnk-create-hub]
* [Seznámení s C SDK][lnk-c-sdk]
* [Sady Azure IoT SDK][lnk-sdks]

Podrobněji prozkoumat možnosti služby IoT Hub, najdete v tématech:

* [Simulace zařízení pomocí IoT Edge][lnk-iotedge]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png
[3]: ./media/iot-hub-csharp-csharp-file-upload/enable-file-notifications.png

<!-- Links -->



[Centrum pro vývojáře Azure IoT]: http://azure.microsoft.com/develop/iot

[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure Storage]:../storage/common/storage-quickstart-create-account.md
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT service SDK NuGet package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md


