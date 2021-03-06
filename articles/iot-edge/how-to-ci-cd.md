---
title: Azure IoT Edge průběžnou integraci a průběžné nasazování | Microsoft Docs
description: Přehled průběžnou integraci a průběžné nasazování pro Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 62d8d770f6b4c3a62a2395eb8c1505dbc3835c28
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/27/2018
ms.locfileid: "37047451"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Průběžnou integraci a průběžné nasazování okraj Azure IoT

Tento článek ukazuje, jak můžete použít průběžnou integraci a funkcí průběžného nasazování Visual Studio Team Services (VSTS) a Microsoft Team Foundation Server (TFS) pro vytvoření, testování a nasazení aplikace rychle a efektivně do vaší Azure Okraj IoT. 

V tomto článku se dozvíte, jak:
* Vytvoření a zkontrolujte v ukázce jednotku obsahující řešení IoT Edge testy.
* Nainstalujte rozšíření Azure IoT Edge pro vaše služby VSTS.
* Nakonfigurujte průběžnou integraci (CI) vytvářet řešení a spouštění testování částí.
* Nakonfigurujte průběžné nasazování (CD) k nasazení řešení a zobrazování odpovědí.

To bude trvat 30 minut na dokončení kroků v tomto článku.

![Položky konfigurace a disku CD](./media/how-to-ci-cd/cd.png)

## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Vytvoření řešení Azure IoT Edge ukázka pomocí Visual Studio Code

V této části vytvoříte ukázkové IoT Edge řešení obsahujícího testy jednotek, které můžete provést v rámci procesu sestavení. Než následující pokyny v této části, proveďte kroky v [vyvíjet IoT řešení s více modulů v aplikaci Visual Studio Code](tutorial-multiple-modules-in-vscode.md).

1. V příkazu palety VS Code, zadejte a spusťte příkaz **Edge: nové IoT řešení**. Pak vyberte pracovní prostor složce, zadejte název řešení (výchozí název je **EdgeSolution**) a vytvořte modul C# (**FilterModule**) jako první modul uživatele v tomto řešení. Bude také nutné zadat úložiště imagí Dockeru pro první modul. Výchozí úložiště bitové kopie je založena na místním registru Docker (`localhost:5000/filtermodule`). Budete muset změnit do registru kontejner Azure (`<your container registry address>/filtermodule`) nebo úložiště Docker Hub další nepřetržité integrace.

    ![Instalační program ACR](./media/how-to-ci-cd/acr.png)

2. V okně VS kód načte pracovního prostoru řešení IoT okraj. Volitelně zadejte a spusťte **Edge: Přidání okraj IoT modulu** přidat další moduly. Je `modules` složku, `.vscode` složku a soubor manifestu šablony nasazení v kořenové složce. Všechny kódy modulu uživatele bude podsložky ve složce `modules`. `deployment.template.json` Je manifestu šablony nasazení. Některé parametry v tomto souboru se získá analýzou z `module.json`, které existuje ve složce každý modul.

3. Ukázky řešení IoT je nyní připraven. Výchozí C# modul funguje jako modul zpráva kanálu. V `deployment.template.json`, zobrazí se, toto řešení obsahuje dva moduly. Zpráva se budou generovat z `tempSensor` modul a bude přímo přesměruje prostřednictvím `FilterModule`, pak se odešle do služby IoT hub. Nahraďte celou **Program.cs** soubor s níže obsah. Další informace o tento fragment kódu najdete [vytvoření projektu modulu IoT Edge C#](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module#create-an-iot-edge-module-project).

    ```csharp
    namespace FilterModule
    {
        using System;
        using System.IO;
        using System.Runtime.InteropServices;
        using System.Runtime.Loader;
        using System.Security.Cryptography.X509Certificates;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Client.Transport.Mqtt;
        using System.Collections.Generic;     // for KeyValuePair<>
        using Microsoft.Azure.Devices.Shared; // for TwinCollection
        using Newtonsoft.Json;                // for JsonConvert

        public class MessageBody
        {
            public Machine machine { get; set; }
            public Ambient ambient { get; set; }
            public string timeCreated { get; set; }
        }
        public class Machine
        {
            public double temperature { get; set; }
            public double pressure { get; set; }
        }
        public class Ambient
        {
            public double temperature { get; set; }
            public int humidity { get; set; }
        }

        public class Program
        {
            static int counter;
            static int temperatureThreshold { get; set; } = 25;

            static void Main(string[] args)
            {
                Init().Wait();

                // Wait until the app unloads or is cancelled
                var cts = new CancellationTokenSource();
                AssemblyLoadContext.Default.Unloading += (ctx) => cts.Cancel();
                Console.CancelKeyPress += (sender, cpe) => cts.Cancel();
                WhenCancelled(cts.Token).Wait();
            }

            /// <summary>
            /// Handles cleanup operations when app is cancelled or unloads
            /// </summary>
            public static Task WhenCancelled(CancellationToken cancellationToken)
            {
                var tcs = new TaskCompletionSource<bool>();
                cancellationToken.Register(s => ((TaskCompletionSource<bool>)s).SetResult(true), tcs);
                return tcs.Task;
            }

            /// <summary>
            /// Initializes the ModuleClient and sets up the callback to receive
            /// messages containing temperature information
            /// </summary>
            static async Task Init()
            {
                MqttTransportSettings mqttSetting = new MqttTransportSettings(TransportType.Mqtt_Tcp_Only);
                ITransportSettings[] settings = { mqttSetting };

                // Open a connection to the Edge runtime
                ModuleClient ioTHubModuleClient = await ModuleClient.CreateFromEnvironmentAsync(settings);
                await ioTHubModuleClient.OpenAsync();
                Console.WriteLine("IoT Hub module client initialized.");

                // Register callback to be called when a message is received by the module
                await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessage, ioTHubModuleClient);
            }

            /// <summary>
            /// This method is called whenever the module is sent a message from the EdgeHub. 
            /// It just pipe the messages without any change.
            /// It prints all the incoming messages.
            /// </summary>
            static async Task<MessageResponse> FilterMessage(Message message, object userContext)
            {
                int counterValue = Interlocked.Increment(ref counter);

                var moduleClient = userContext as ModuleClient;
                if (moduleClient == null)
                {
                    throw new InvalidOperationException("UserContext doesn't contain " + "expected values");
                }

                byte[] messageBytes = message.GetBytes();
                string messageString = Encoding.UTF8.GetString(messageBytes);
                Console.WriteLine($"Received message: {counterValue}, Body: [{messageString}]");

                var filteredMessage = filter(message);

                if (filteredMessage != null && !string.IsNullOrEmpty(messageString))
                {
                    var pipeMessage = new Message(messageBytes);
                    foreach (var prop in message.Properties)
                    {
                        pipeMessage.Properties.Add(prop.Key, prop.Value);
                    }
                    await moduleClient.SendEventAsync("output1", pipeMessage);
                    Console.WriteLine("Received message sent");
                }
                return MessageResponse.Completed;
            }

            public static Message filter(Message message)
            {
                var counterValue = Interlocked.Increment(ref counter);

                var messageBytes = message.GetBytes();
                var messageString = Encoding.UTF8.GetString(messageBytes);
                Console.WriteLine($"Received message {counterValue}: [{messageString}]");

                // Get message body
                var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

                if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
                {
                    Console.WriteLine($"Machine temperature {messageBody.machine.temperature} " +
                        $"exceeds threshold {temperatureThreshold}");
                    var filteredMessage = new Message(messageBytes);
                    foreach (KeyValuePair<string, string> prop in message.Properties)
                    {
                        filteredMessage.Properties.Add(prop.Key, prop.Value);
                    }

                    filteredMessage.Properties.Add("MessageType", "Alert");
                    return filteredMessage;
                }
                return null;
            }
        }
    }
    ```

4. Vytvoření .net Core projektu testování částí. V Průzkumníku souborů VS Code, vytvořte novou složku **tests\FilterModuleTest** v pracovním prostoru. Potom v terminálu integrované VS Code (**Ctrl + '**) spusťte následující příkazy a vytvoření projektu testování xunit a přidat odkaz na **FilterModule** projektu.

    ```cmd
    cd tests\FilterModuleTest
    dotnet new xunit
    dotnet add reference ../../modules/FilterModule/FilterModule.csproj
    ```

    ![Struktura složek](./media/how-to-ci-cd/add-test-project.png)

5. V **FilterModuleTest** složky, aktualizujte název souboru **UnitTest1.cs** k **FilterModuleTest.cs**. Vyberte a otevřete **FilterModuleTest.cs**, nahraďte celý kód s následující fragment kódu, který obsahuje testování částí proti FilterModule projektu.

    ```csharp
    using Xunit;
    using FilterModule;
    using Newtonsoft.Json;
    using System;
    using System.IO;
    using System.Runtime.InteropServices;
    using System.Runtime.Loader;
    using System.Security.Cryptography.X509Certificates;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Client.Transport.Mqtt;

    namespace FilterModuleTest
    {
        public class FilterModuleTest
        {
            [Fact]
            public void filterLessThanThresholdTest()
            {
                var source = createMessage(25 - 1);
                var result = Program.filter(source);
                Assert.True(result == null);
            }

            [Fact]
            public void filterMoreThanThresholdAlertPropertyTest()
            {
                var source = createMessage(25 + 1);
                var result = Program.filter(source);
                Assert.True(result.Properties["MessageType"] == "Alert");
            }

            [Fact]
            public void filterMoreThanThresholdCopyPropertyTest()
            {
                var source = createMessage(25 + 1);
                source.Properties.Add("customTestKey", "customTestValue");
                var result = Program.filter(source);
                Assert.True(result.Properties["customTestKey"] == "customTestValue");
            }

            private Message createMessage(int temperature)
            {
                var messageBody = createMessageBody(temperature);
                var messageString = JsonConvert.SerializeObject(messageBody);
                var messageBytes = Encoding.UTF8.GetBytes(messageString);
                return new Message(messageBytes);
            }

            private MessageBody createMessageBody(int temperature)
            {
                var messageBody = new MessageBody
                {
                    machine = new Machine
                    {
                        temperature = temperature,
                        pressure = 0
                    },
                    ambient = new Ambient
                    {
                        temperature = 0,
                        humidity = 0
                    },
                    timeCreated = string.Format("{0:O}", DateTime.Now)
                };

                return messageBody;
            }
        }
    }
    ```

6. V integrovaném terminálu můžete zadat následující příkazy ke spuštění testů jednotek místně. 
    ```cmd
    dotnet test
    ```

    ![Testování částí](./media/how-to-ci-cd/unit-test.png)

7. Uložte tyto projekty a poté zkontrolujte ho do úložiště služby VSTS nebo TFS.
    

> [!NOTE]
> Další informace o používání služby VSTS kódu úložiště naleznete v tématu [sdílet kódu s Visual Studio a služby VSTS Git](https://docs.microsoft.com/vsts/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-continuous-integration"></a>Konfigurace nepřetržité integrace
V této části vytvoříte definice buildu, která je nakonfigurována na automatické spuštění při se změnami všechny změny ukázkové řešení IoT a automaticky spustí testy jednotek, které obsahuje.

1. Přihlaste se k účtu služby VSTS (**https://**_váš účet_**. visualstudio.com**) a otevřete projekt, kde můžete zkontrolovat v ukázkové aplikace.

    ![Vrácení se změnami kódu](./media/how-to-ci-cd/init-project.png)

1. Navštivte [Azure IoT hranu služby VSTS](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) na služby VSTS Marketplace. Klikněte na tlačítko **získat volné** a postupujte podle pokynů Průvodce toto rozšíření nainstalovat do vašeho účtu služby VSTS a stažení do vaší sady TFS.

    ![Instalace rozšíření](./media/how-to-ci-cd/install-extension.png)

1. Ve vaší služby VSTS, otevřete **sestavení &amp; verze** rozbočovače a v **sestavení** , zvolte **+ novou definici**. Nebo, pokud již máte definice sestavení, vyberte **+ nový** tlačítko. 

    ![Nové sestavení](./media/how-to-ci-cd/add-new-build.png)

1. Pokud se zobrazí výzva, vyberte **Git služby VSTS** typ zdroje; vyberte projekt, úložiště a firemní pobočky, kde se nachází váš kód. Zvolte **pokračovat**.

    ![Vyberte služby VSTS git](./media/how-to-ci-cd/select-vsts-git.png)

1. V **vyberte šablonu** okně zvolte **začít s prázdnou proces**.

    ![Prázdný](./media/how-to-ci-cd/start-with-empty.png)

1. Klikněte na tlačítko **+** na pravé straně **fáze 1** přidat úloha do fáze. Pak vyhledejte a vyberte **.Net Core**a klikněte na tlačítko **přidat** přidat tuto úlohu na fázi.

    ![Test DotNet.](./media/how-to-ci-cd/add-dot-net-core.png)

1. Aktualizace **zobrazovaný název** k **dotnet test**a v **příkaz** rozevíracího seznamu vyberte **testování**. Přidat následující cestu k **cestu k projekty**.

    ```
    tests/FilterModuleTest/*.csproj
    ```

    ![Konfigurace testovací dotnet.](./media/how-to-ci-cd/dotnet-test.png)

1. Klikněte na tlačítko **+** na pravé straně **fáze 1** přidat úloha do fáze. Pak vyhledejte a vyberte **Azure IoT Edge**a klikněte na tlačítko **přidat** tlačítko **dvakrát** přidat tyto úlohy do fáze.

    ![IoT Edge](./media/how-to-ci-cd/add-azure-iot-edge.png)

1. V první úloze Azure IoT Edge, aktualizovat **zobrazovaný název** k **modul sestavení a nabízených**a v **akce** rozevíracího seznamu vyberte **sestavení a Push**. V **Module.json souboru** textovému poli, do něj přidejte následující cesty. Zvolte **typ kontejneru registru**, ujistěte se, nakonfigurujte a vyberte stejný registru do vašeho kódu. Tato úloha bude sestavovat a push všechny moduly v řešení a publikovat do registru kontejneru, který jste zadali. Pokud moduly se vloží do různých registrech, můžete mít více **modul sestavení a nabízených** úlohy.

    ```
    **/module.json
    ```

    ![Sestavení modulu a posílejte nabízená oznámení](./media/how-to-ci-cd/module-build-push.png)

1. V druhé úloze Azure IoT Edge aktualizace **zobrazovaný název** k **nasadit do zařízení IoT Edge**a v **akce** rozevíracího seznamu vyberte **nasadit do hraniční IoT zařízení**. Vyberte předplatné Azure a zadejte název vaší služby IoT Hub. Můžete zadat ID IoT Edge nasazení a prioritu pro nasazení. Můžete také nasadit do jedné nebo více zařízení. Pokud provádíte nasazení na více zařízení, je třeba zadat cílovou podmínku zařízení. Například pokud chcete používat značky zařízení jako podmínka, budete muset aktualizovat zařízení odpovídající značky před nasazení. 

    ![Nasazení na okraj](./media/how-to-ci-cd/deploy-to-edge.png)

1. Klikněte na tlačítko **proces** a zajistěte, aby vaše **fronty agenta** je **Preview Linux hostované**.

    ![Konfigurace](./media/how-to-ci-cd/configure-env.png)

1. Otevřete **aktivační události** kartě a zapněte **průběžnou integraci** aktivační události. Zajistěte, aby větev obsahující kódu je součástí.

    ![Trigger](./media/how-to-ci-cd/configure-trigger.png)

1. Uložte novou definici sestavení a fronty nové sestavení. Klikněte **Uložit & fronty** tlačítko.

1. Vyberte na panelu zpráv, který se zobrazí odkaz na sestavení. Nebo na vytvoření definice zobrazíte nejnovější zařazených do fronty sestavení úlohy.

    ![Sestavení](./media/how-to-ci-cd/build-def.png)

1. Po dokončení sestavení se zobrazí souhrnné informace pro každý úkol a výsledky v souboru protokolu za provozu. 
    
    ![Dokončit](./media/how-to-ci-cd/complete.png)

1. Můžete přejít zpět do VS Code a zkontrolujte explorer zařízení IoT Hub. Hraniční zařízení s modulem by se měl spustit systémem (zajistěte, aby registru pověření jste přidali do hraniční runtime).

    ![Hraniční systémem](./media/how-to-ci-cd/edge-running.png)

## <a name="continuous-deployment-to-iot-edge-devices"></a>Průběžné nasazování do zařízení IoT Edge

Pokud chcete povolit průběžné nasazování, v podstatě budete muset nastavit CI úlohy s správné IoT hraniční zařízení, povolení **aktivační události** pro větvích ve vašem projektu. V classic DevOps praxi projekt obsahuje dvě hlavní větve. Hlavní větve by měla být stabilní verzi kódu a vývoj větve obsahuje nejnovější změny kódu. Každý vývojář v týmu měli rozvětvit vývoj větev svůj nebo svůj vlastní funkce větve při spuštění aktualizace kódu, což znamená, že všechny potvrdí se odehrává na funkci větví vypnout vývoj větev. A každý stisknutí potvrzení by měl být testována prostřednictvím systému CI. Po plně otestovat kód místně, by měly být větev funkce sloučeny do větve vývoj prostřednictvím žádost o přijetí změn. Při testování kód na vývojáře větve prostřednictvím systému položek konfigurace, můžete sloučit do hlavní větve prostřednictvím žádost o přijetí změn.

Ano při nasazování do zařízení IoT Edge, existují tři hlavní prostředí.
- Na funkci větve můžete použít simulované zařízení IoT Edge na vývojovém počítači nebo nasadit do fyzického zařízení IoT okraj.
- Na vývoj větve, měli byste nasadit do fyzického zařízení IoT okraj.
- Na hlavní větve musí být cílová zařízení IoT hraniční zařízení produkční.

## <a name="next-steps"></a>Další postup

* Pochopení IoT Edge nasazení v [pochopit IoT Edge nasazení jednoho zařízení nebo ve velkém měřítku](module-deployment-monitoring.md)
* Provede kroky k vytvoření, aktualizace nebo odstranění nasazení v [nasadit a monitorovat moduly IoT Edge ve velkém měřítku](how-to-deploy-monitor.md).
