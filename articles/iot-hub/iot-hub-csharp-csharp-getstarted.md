<properties
    pageTitle="Azure IoT Hub voor C# aan de slag | Microsoft Azure"
    description="Azure IoT Hub met C# aan de slag zelfstudie. Azure IoT Hub en C# met de Microsoft Azure IoT SDK's gebruiken om een oplossing Internet van zaken te implementeren."
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-net"></a>Aan de slag met Azure IoT Hub voor .NET

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Aan het einde van deze zelfstudie hebt u drie Windows console-toepassingen:

* **CreateDeviceIdentity**, die Hiermee maakt u een apparaat-identiteit en de bijbehorende beveiliging-toets om te koppelen van een gesimuleerd apparaat.
* **ReadDeviceToCloudMessages**, waarin de telemetrielogboek door uw gesimuleerd apparaat verzonden.
* **SimulatedDevice**, maakt verbinding met uw IoT hub voor de identiteit van het apparaat eerder hebt gemaakt, en een bericht telemetrielogboek elke tweede met behulp van het AMQP-protocol.

> [AZURE.NOTE] Zie voor informatie over de verschillende SDK's die u gebruiken kunt om te maken van beide toepassingen uitvoeren op apparaten, en uw oplossing back-end [IoT Hub SDK's][lnk-hub-sdks].

Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

+ Microsoft Visual Studio 2015.

+ Een actieve Azure-account. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

U hebt nu uw hub IoT gemaakt en u de tekenreeks hostname en de verbinding die u nodig hebt voor de rest van deze zelfstudie hebt.

## <a name="create-a-device-identity"></a>Een identiteit apparaat maken

In dit gedeelte maakt u een console-app voor Windows die Hiermee maakt u een apparaat-id in het register identiteit in de hub IoT. Een apparaat verbinding geen met IoT-hub, tenzij er een vermelding in het register van de identiteit apparaat. Zie voor meer informatie de sectie "Apparaat identiteit register" van de [Handleiding voor ontwikkelaars van IoT Hub][lnk-devguide-identity]. Wanneer u deze console-app uitvoert, wordt een unieke apparaat-ID en de sleutel die uw apparaat gebruiken kunt om zichzelf te identificeren bij het apparaat naar cloud-berichten op IoT Hub verzenden gegenereerd.

1. Visual Studio, moet u een visuele C# klassiek Windows-bureaublad-project toevoegen naar de huidige oplossing met behulp van het project-sjabloon van **Console-toepassing** . Zorg ervoor dat de .NET Framework-versie is 4.5.1 of hoger. De naam van het project **CreateDeviceIdentity**.

    ![Nieuw visuele C# klassiek Windows-bureaublad-project][10]

2. In Solution Explorer met de rechtermuisknop op het project **CreateDeviceIdentity** en klik vervolgens op **Nuget-pakketten beheren**.

3. Klik in het venster **Nuget Package Manager** Selecteer **Bladeren**, zoekt **microsoft.azure.devices**, **installeren** voor het installeren van het pakket **Microsoft.Azure.Devices** en accepteer de gebruiksvoorwaarden. Deze procedure is gedownload, installaties en voegt u een verwijzing naar de [Microsoft Azure IoT Service SDK] [ lnk-nuget-service-sdk] Nuget-pakket en de bijbehorende afhankelijkheden.

    ![Nuget Package Manager-venster][11]

4. Voeg de volgende `using` instructies boven aan het bestand **Program.cs** :

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. De volgende velden toevoegen aan de klas **programma** . Vervang de aanduidingswaarde van de tijdelijke met de verbindingsreeks voor de hub IoT die u in de vorige sectie hebt gemaakt.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. De volgende methode toevoegen aan de klas **programma** :

        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }

    Deze methode maakt de identiteit van een apparaat met ID **myFirstDevice**. (Als dat apparaat-ID al in het register bestaat, de code gewoon haalt de bestaande apparaatinformatie.) De app geeft de primaire sleutel voor die identiteit. Gebruik deze sleutel in de gesimuleerd apparaat verbinding maken met uw hub IoT.

7. Tot slot toevoegen de volgende regels aan de methode **Main** :

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. Uitvoeren van deze toepassing en noteer de apparaat-toets.

    ![Apparaat-toets gegenereerd door de toepassing][12]

> [AZURE.NOTE] Er worden alleen het IoT Hub identiteit register opgeslagen apparaat identiteiten voor beveiligde toegang tot de hub. Apparaat-id's en sleutels wilt gebruiken als beveiligingsreferenties en een vlag ingeschakeld/uitgeschakeld, die u kunt gebruiken voor het uitschakelen van toegang voor een individuele apparaat wordt opgeslagen. Als de toepassing voor de opslag van andere metagegevens apparaat-specifieke, moet deze een winkel toepassingsspecifieke gebruiken. Zie voor meer informatie [IoT Hub Developer Guide][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Apparaat-naar-cloud berichten ontvangt

In deze sectie, kunt u een Windows-console-app die apparaat-naar-cloud berichten van IoT Hub leest maken. Een hub IoT beschrijft een [Azure gebeurtenis Hubs][lnk-event-hubs-overview]-compatibele eindpunt zodat u kunt het apparaat naar cloud berichten lezen. Als u wilt houden dingen eenvoudige, deze zelfstudie Hiermee maakt u een eenvoudige lezer die niet geschikt is voor een hoge doorvoer-implementatie. Als u wilt weten hoe u het apparaat naar cloud-berichten bij het op schaal verwerkt, raadpleegt u het [proces apparaat naar cloud berichten] [ lnk-process-d2c-tutorial] zelfstudie. Zie voor meer informatie over het proces berichten van gebeurtenis Hubs, de [Aan de slag met gebeurtenis Hubs] [ lnk-eventhubs-tutorial] zelfstudie. (Deze zelfstudie is van toepassing op de eindpunten IoT Hub gebeurtenis Hub-compatibele.)

> [AZURE.NOTE] Het eindpunt van de gebeurtenis Hub-compatibele voor het apparaat naar cloud berichten altijd leest gebruikt het AMQP-protocol.

1. Visual Studio, moet u een visuele C# klassiek Windows-bureaublad-project toevoegen naar de huidige oplossing, met behulp van de projectsjabloon **Console-toepassing** . Zorg ervoor dat de .NET Framework-versie is 4.5.1 of hoger. De naam van het project **ReadDeviceToCloudMessages**.

    ![Nieuw visuele C# klassiek Windows-bureaublad-project][10]

2. In Solution Explorer met de rechtermuisknop op het project **ReadDeviceToCloudMessages** en klik vervolgens op **Nuget-pakketten beheren**.

3. Klik in het venster **Nuget Package Manager** **WindowsAzure.ServiceBus**zoekt, selecteert u **installeren**en accepteer de gebruiksvoorwaarden. Deze procedure is gedownload, installaties en voegt u een verwijzing naar [Azure Service Bus][lnk-servicebus-nuget], met alle bijbehorende afhankelijkheden. Dit pakket kan de toepassing verbinding maken met het eindpunt van de gebeurtenis Hub-compatibele op uw hub IoT.

4. Voeg de volgende `using` instructies boven aan het bestand **Program.cs** :

        using Microsoft.ServiceBus.Messaging;
        using System.Threading;

5. De volgende velden toevoegen aan de klas **programma** . Vervang de aanduidingswaarde van de tijdelijke met de verbindingsreeks voor de IoT hub die u hebt gemaakt in de sectie "Maken een hub IoT".

        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;

6. De volgende methode toevoegen aan de klas **programma** :

        private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
        {
            var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
            while (true)
            {
                if (ct.IsCancellationRequested) break;
                EventData eventData = await eventHubReceiver.ReceiveAsync();
                if (eventData == null) continue;

                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }

    Deze methode wordt een exemplaar **EventHubReceiver** gebruikt voor het ontvangen van berichten van alle de IoT hub apparaat-naar-cloud partities ontvangen. Zoals u ziet hoe u doorgeven een `DateTime.Now` parameter bij het maken van het object **EventHubReceiver** , zodat deze alleen nadat deze is gestart verzonden berichten ontvangt. Dit filter is handig in een testomgeving zodat u de huidige set van berichten kunt zien. In een productieomgeving moet uw code zorgen dat alle berichten worden verwerkt. Zie voor meer informatie, [het verwerken van IoT Hub apparaat naar cloud berichten] [ lnk-process-d2c-tutorial] zelfstudie.

7. Tot slot toevoegen de volgende regels aan de methode **Main** :

        Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

        CancellationTokenSource cts = new CancellationTokenSource();

        System.Console.CancelKeyPress += (s, e) =>
        {
          e.Cancel = true;
          cts.Cancel();
          Console.WriteLine("Exiting...");
        };

        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
           tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }  
        Task.WaitAll(tasks.ToArray());

## <a name="create-a-simulated-device-app"></a>Een app gesimuleerd apparaat maken

In dit gedeelte maakt u een console-app voor Windows die een apparaat dat apparaat naar cloud berichten naar een hub IoT verzendt simuleert.

1. Visual Studio, moet u een visuele C# klassiek Windows-bureaublad-project toevoegen naar de huidige oplossing, met behulp van het project-sjabloon van **Console-toepassing** . Zorg ervoor dat de .NET Framework-versie is 4.5.1 of hoger. De naam van het project **SimulatedDevice**.

    ![Nieuw visuele C# klassiek Windows-bureaublad-project][10]

2. In Solution Explorer met de rechtermuisknop op het project **SimulatedDevice** en klik vervolgens op **Nuget-pakketten beheren**.

3. Klik in het venster **Nuget Package Manager** Selecteer **Bladeren**, zoekt **Microsoft.Azure.Devices.Client**, **installeren** voor het installeren van het pakket **Microsoft.Azure.Devices.Client** en accepteer de gebruiksvoorwaarden. Deze procedure is gedownload, installaties en voegt u een verwijzing naar de [Azure IoT - apparaat SDK Nuget pakket] [ lnk-device-nuget] en de bijbehorende afhankelijkheden.

4. Voeg de volgende `using` instructie boven aan het bestand **Program.cs** :

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;

5. De volgende velden toevoegen aan de klas **programma** . Vervang de tijdelijke aanduiding voor waarden door de IoT hub hostname die u hebt opgehaald in de sectie 'Een hub IoT maken' en de toets apparaat in de sectie 'Een apparaat-identiteit maken' opgehaald.

        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. De volgende methode toevoegen aan de klas **programma** :

        private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();

            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;

                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));

                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

                Task.Delay(1000).Wait();
            }
        }

    Deze methode wordt een nieuw apparaat naar cloud-mailbericht van elke tweede stuurt. Het bericht bevat een object JSON serienummer, met de apparaat-ID en een getal dat willekeurig wordt gegenereerd, kunt u een wind snelheid sensor simuleren.

7. Tot slot toevoegen de volgende regels aan de methode **Main** :

        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();

  De methode **Create** maakt standaard een exemplaar van **DeviceClient** die het protocol AMQP om te communiceren met IoT Hub gebruikt. Om het HTTP-protocol het opheffen van de methode **maken** waarmee u kunt opgeven van het protocol te gebruiken. Als u het HTTP-protocol gebruikt, moet u ook het **Microsoft.AspNet.WebApi.Client** Nuget pakket toevoegen aan uw project voor het opnemen van de naamruimte **System.Net.Http.Formatting** .

Deze zelfstudie gaat u door de stappen voor het maken van een client IoT Hub-apparaat. U kunt ook de [Service voor Azure IoT Hub verbonden] [ lnk-connected-service] Visual Studio-extensie voor de benodigde code hebt toegevoegd met de clienttoepassing van uw apparaat.


> [AZURE.NOTE] Als u wilt houden dingen eenvoudige, wordt deze zelfstudie opnieuw beleid niet geïmplementeerd. In productiecode, moet u opnieuw beleid (zoals een exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke foutenstructuuranalyse afhandelen]implementeren[lnk-transient-faults].

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren de toepassingen.

1.  Met de rechtermuisknop op de oplossing in Visual Studio, in Solution Explorer en klik op **instellen opstarten projecten**. Selecteer **meerdere opstarten projecten**en selecteer vervolgens **starten** als de actie voor de **ReadDeviceToCloudMessages** en de **SimulatedDevice** projecten.

    ![Opstartopties voor project][41]

2.  Druk op **F5** om te beginnen beide apps uitgevoerd. De uitvoer console vanuit de app **SimulatedDevice** geeft de berichten die uw gesimuleerd apparaat wordt verzonden naar uw hub IoT. De uitvoer console vanuit de app **ReadDeviceToCloudMessages** geeft de berichten die uw hub IoT ontvangt.

    ![Console-uitvoer van apps][42]

3. De tegel voor **Gebruik** in de [portal van Azure] [ lnk-portal] bevat het nummer van berichten die zijn verzonden naar de hub:

    ![Azure portal gebruik-tegel][43]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie kunt u een hub IoT geconfigureerd in de portal van Azure, en vervolgens de identiteit van een apparaat in de hub identiteit register gemaakt. U deze identiteit apparaat gebruikt om de app gesimuleerd apparaat apparaat naar cloud berichten naar de hub te verzenden. U ook een app waarin de berichten ontvangen door de hub hebt gemaakt. 

Zie kunt doorgaan met het aan de slag met IoT Hub en verkennen van andere IoT scenario's:

- [Verbinding maken van uw apparaat][lnk-connect-device]
- [Aan de slag met Apparaatbeheer][lnk-device-management]
- [Aan de slag met de SDK van de Gateway IoT][lnk-gateway-SDK]

Als u wilt weten hoe u uw oplossing IoT uitbreiden en apparaat-naar-cloud-berichten bij het op schaal verwerkt, raadpleegt u het [proces apparaat naar cloud berichten] [ lnk-process-d2c-tutorial] zelfstudie.

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
