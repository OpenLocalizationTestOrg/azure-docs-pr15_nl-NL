<properties
    pageTitle="Verzenden van berichten van de cloud-naar-apparaat met IoT Hub | Microsoft Azure"
    description="Volg deze zelfstudie meer informatie over het gebruik van Azure IoT Hub met C# cloud-to-device-berichten te verzenden."
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/23/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-net"></a>Zelfstudie: Het verzenden van berichten van de cloud-naar-apparaat met IoT Hub en .net

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Inleiding

Azure IoT Hub heeft een volledig beheerde service waarmee inschakelen betrouwbare en veilige bidirectionele communicatie tussen miljoenen IoT apparaten en weer een toepassing beëindigen. De zelfstudie [aan de slag met IoT Hub] ziet hoe u een hub IoT maken, inrichten van een identiteit apparaat erin en code een gesimuleerd apparaat, dat wordt verzonden berichten apparaat-naar-cloud.

Deze zelfstudie dat is gebaseerd op het [aan de slag met IoT Hub]. U ziet hoe naar:

- Vanuit uw toepassing cloud back-end, door cloud-to-device berichten te verzenden naar een enkel apparaat via IoT Hub.
- Cloud-to-device ontvangen op een apparaat.
- Een back-end, bezorging bevestiging (*feedback*) aanvragen voor berichten die worden verzonden naar een apparaat IoT Hub vanuit de cloud toepassing.

U vindt meer informatie over cloud-to-device berichten in de [Handleiding voor ontwikkelaars van IoT Hub][IoT Hub Developer Guide - C2D].

Aan het einde van deze zelfstudie, wordt u twee Windows console-toepassingen uitvoeren:

* **SimulatedDevice**, een gewijzigde versie van de app die is gemaakt in [aan de slag met IoT-Hub], die is verbonden met uw hub IoT en ontvangt berichten cloud-naar-apparaat.
* **SendCloudToDevice**, stuurt een bericht cloud-naar-apparaat naar het gesimuleerd apparaat via IoT-Hub, en klikt u vervolgens de bezorging-bevestiging ontvangt.

> [AZURE.NOTE] IoT Hub heeft SDK ondersteuning voor veel talen (inclusief C, Java en Javascript) tot en met Azure IoT apparaat SDK's en platforms voor apparaten. Zie voor stapsgewijze instructies voor het koppelen van uw apparaat van deze zelfstudie code en over het algemeen op Azure IoT-Hub, het [Azure IoT Developer Center].

Als u wilt deze zelfstudie hebt voltooid, hebt u het volgende nodig:

+ Microsoft Visual Studio 2015

+ Een actieve Azure-account. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.)

## <a name="receive-messages-on-the-simulated-device"></a>Ontvangen van berichten op het apparaat dat gesimuleerd

In deze sectie, moet u de apparaattoepassing gesimuleerd wijzigen u hebt gemaakt in [aan de slag met IoT Hub] naar cloud-to-device berichten ontvangt van de hub IoT.

1. Visual Studio, in het project **SimulatedDevice** toevoegen de volgende methode aan de klas **programma** .

        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();

                await deviceClient.CompleteAsync(receivedMessage);
            }
        }

    De `ReceiveAsync` methode van asynchroon geeft als resultaat het ontvangen bericht op het moment dat deze is ontvangen door het apparaat. Het resultaat *leeg* na een periode specifiable time-out (in dit geval de standaardwaarde van één minuut wordt gebruikt). Als dit gebeurt, moet de code wachten tot nieuwe berichten. Dit is de reden voor de `if (receivedMessage == null) continue` lijn.

    De oproep door naar `CompleteAsync()` IoT Hub krijgt dat het bericht is verwerkt. Het bericht kan veilig worden verwijderd uit de apparaatwachtrij. Als er iets is er gebeurd waardoor de app apparaat voltooiing van de verwerking van het bericht, bezorgd IoT Hub opnieuw. Het is vervolgens belangrijk dat bericht verwerking logica in de app apparaat *idempotency is ingeschakeld*, zodat hetzelfde bericht ontvangen meerdere keren hetzelfde resultaat oplevert. Een toepassing kunt een bericht, waardoor IoT hub behoud van het bericht in de wachtrij voor toekomstige verbruik ook tijdelijk verlaten. Of de toepassing een bericht, waarmee wordt het bericht worden blijvend verwijderd uit de wachtrij kunt negeren. Zie voor meer informatie over de levenscyclus van de cloud-to-device bericht, de [Handleiding voor ontwikkelaars van IoT Hub][IoT Hub Developer Guide - C2D].

    > [AZURE.NOTE] Wanneer u een HTTP gebruikt in plaats van MQTT of AMQP als transport, de `ReceiveAsync` methode onmiddellijk als resultaat. De ondersteunde patroon cloud-to-device berichten met HTTP is tijdelijk verbonden apparaten te controleren voor berichten zelden (minder dan elke 25 minuten). Meer HTTP uitgifte ontvangt resultaten in IoT Hub beperken de aanvragen. Zie voor meer informatie over de verschillen tussen MQTT, AMQP en HTTP-ondersteuning en het beperken van IoT-Hub, de [Handleiding voor ontwikkelaars van IoT Hub][IoT Hub Developer Guide - C2D].

2. De volgende methode in de methode **Main** toevoegen rechts voordat de `Console.ReadLine()` lijn:

        ReceiveC2dAsync();

> [AZURE.NOTE] Voor de eenvoudig mogelijk te houden, wordt deze zelfstudie opnieuw beleid niet geïmplementeerd. In productiecode, moet u opnieuw beleid (zoals exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke foutenstructuuranalyse afhandelen]implementeren.

## <a name="send-a-cloud-to-device-message"></a>Een bericht cloud naar apparaat verzenden

In deze sectie, hebt u een Windows-console-app die cloud-to-device berichten naar de app gesimuleerd apparaat schrijven.

1. In de huidige Visual Studio-oplossing, door een nieuw visueel C# bureaublad-App-project te maken met behulp van het project-sjabloon van **Console-toepassing** . De naam van het project **SendCloudToDevice**.

    ![Nieuw project in Visual Studio][20]

2. In Solution Explorer met de rechtermuisknop op de oplossing en klik vervolgens op **NuGet-pakketten beheren voor oplossing...**. 

    Hiermee opent u het venster **NuGet-pakketten beheren** .

3. Zoeken naar `Microsoft Azure Devices`, klikt u op **installeren**en accepteer de gebruiksvoorwaarden. 

    Dit is gedownload, installaties en voegt een verwijzing naar de [IoT van Azure - Service SDK NuGet-pakket].

4. Voeg de volgende `using` instructie boven aan het bestand **Program.cs** :

        using Microsoft.Azure.Devices;

5. De volgende velden toevoegen aan de klas **programma** . Vervang de aanduidingswaarde van de tijdelijke met de verbindingsreeks van IoT hub van het [aan de slag met IoT Hub]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";

6. De volgende methode toevoegen aan de klas **programma** :

        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }

    Deze methode verzendt u een nieuw bericht in de cloud-naar-apparaat aan op het apparaat met de-ID, `myFirstDevice`. Wijzig deze parameter dienovereenkomstig gewijzigd, in het geval u deze vanaf het account waarmee in [aan de slag met IoT Hub]hebt gewijzigd.

7. Tot slot toevoegen de volgende regels aan de methode **Main** :

        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);

        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();

8. Uit met de rechtermuisknop op de oplossing in Visual Studio, en selecteer **opstarten instellen projecten...**. Selecteer **meerdere opstarten projecten**en vervolgens de actie **starten** voor **ProcessDeviceToCloudMessages**, **SimulatedDevice**en **SendCloudToDevice**.

9.  Druk op **F5**. Alle drie de toepassingen moeten beginnen. Selecteer de vensters **SendCloudToDevice** en druk op **Enter**. Hier ziet u het bericht wordt ontvangen door de gesimuleerd app.

    ![App ontvangen bericht][21]

## <a name="receive-delivery-feedback"></a>Bezorging feedback ontvangen
Het is mogelijk naar verzoek bezorging (of verlooptijd) bevestigingen van IoT Hub voor elk bericht cloud-naar-apparaat. Hiermee worden de cloud-back-end om te informeren eenvoudig opnieuw of vergoedingen logica. Zie voor meer informatie over cloud-to-device feedback, de [Handleiding voor ontwikkelaars van IoT Hub][IoT Hub Developer Guide - C2D].

In deze sectie wijzigt u de **SendCloudToDevice** -app om feedback aanvragen en u ontvangt deze van IoT Hub.

1. Visual Studio, in het project **SendCloudToDevice** toevoegen de volgende methode aan de klas **programma** .
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();

            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();

                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }

    Houd er rekening mee dat het patroon ontvangen het hetzelfde account waarmee naar cloud-to-device berichten ontvangt van de app apparaat is.

2. Voegt u de volgende methode in de methode **Main** direct na de `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` lijn:

        ReceiveFeedbackAsync();

3. Als u wilt ontvangen feedback voor de bezorging van uw bericht cloud-naar-apparaat, moet u een eigenschap in de methode **SendCloudToDeviceMessageAsync** opgeven. Voeg de volgende regel, direct na de `var commandMessage = new Message(...);` lijn:

        commandMessage.Ack = DeliveryAcknowledgement.Full;

4.  De apps uitvoeren door op **F5**te drukken. Hier ziet u alle drie de toepassingen starten. Selecteer de vensters **SendCloudToDevice** en druk op **Enter**. Hier ziet u het bericht wordt ontvangen door de gesimuleerd app en na een paar seconden de Feedbackbericht door uw toepassing **SendCloudToDevice** wordt ontvangen.

    ![App ontvangen bericht][22]

> [AZURE.NOTE] Voor de eenvoudig mogelijk te houden, wordt deze zelfstudie opnieuw beleid niet geïmplementeerd. In productiecode, moet u opnieuw beleid (zoals exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke foutenstructuuranalyse afhandelen]implementeren.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe cloud-to-device berichten verzenden en ontvangen. 

Voorbeelden van volledige end-to-end-oplossingen die IoT Hub gebruikt, raadpleegt u [Azure IoT Suite].

Meer informatie over het ontwikkelen van oplossingen met IoT-Hub, raadpleegt u de [Handleiding voor ontwikkelaars van IoT Hub].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IoT - Service SDK NuGet pakket]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Tijdelijke foutenstructuuranalyse afhandeling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md

[Handleiding voor ontwikkelaars van IoT Hub]: iot-hub-devguide.md
[Aan de slag met IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/