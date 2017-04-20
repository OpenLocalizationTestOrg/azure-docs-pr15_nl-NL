<properties
    pageTitle="Upload bestanden op apparaten met IoT Hub | Microsoft Azure"
    description="Volg deze zelfstudie meer informatie over het uploaden van bestanden op apparaten met Azure IoT Hub met C#."
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
     ms.date="06/21/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-upload-files-from-devices-to-the-cloud-with-iot-hub"></a>Zelfstudie: Het uploaden van bestanden van apparaten in de cloud met IoT Hub

## <a name="introduction"></a>Inleiding

Azure IoT Hub heeft een volledig beheerde service betrouwbare waarmee en beveiligde bidirectionele communicatie tussen miljoenen IoT apparaten en een toepassing een back-end. Vorige zelfstudies ([aan de slag met IoT Hub] en [verzenden van berichten van de Cloud-naar-apparaat met IoT Hub]) illustreren de eenvoudige apparaat naar cloud en cloud-to-device SMS functionaliteit van IoT Hub en de zelfstudie [proces apparaat-naar-Cloud berichten] een manier naar betrouwbaar apparaat naar cloud berichten opslaan in Azure-blobopslag beschreven. In sommige gevallen kan niet u echter eenvoudig toewijzen de gegevens die uw apparaten verzenden naar de relatief klein apparaat naar cloud-berichten die IoT Hub heeft geaccepteerd. Voorbeelden hiervan zijn grote bestanden die afbeeldingen, video's, trillingen gegevens partijen met hoge frequentie bevatten of die een vorm van voorverwerkte gegevens bevatten. Deze bestanden zijn meestal batch in de cloud met hulpprogramma's zoals [Factory van Azure-gegevens] of de stapel [Hadoop] verwerkt. Wanneer bestandsuploads vanaf een apparaat met het verzenden van gebeurtenissen voorkeur bent, kunt u nog steeds IoT Hub beveiliging en betrouwbaarheid functionaliteit.

Deze zelfstudie is gebaseerd op de code in de [Cloud-to-Device berichten verzenden met IoT Hub] zelfstudie om aan te geven hoe u de functies voor het uploaden van bestanden van IoT Hub gebruikt. Deze ziet u hoe u het veilig bieden een apparaat met een Azure blob-URI voor het uploaden van een bestand en het gebruik van de meldingen IoT Hub bestand uploaden om te activeren verwerking van het bestand in uw app back-end.

Aan het einde van deze zelfstudie kunt u twee Windows-console-toepassingen uitvoeren:

* **SimulatedDevice**, een gewijzigde versie van de app is gemaakt in de [Cloud-naar-apparaat berichten verzenden met IoT Hub] zelfstudie waarin een bestand naar opslagruimte met een SAS-URI verstrekt door uw hub IoT geüpload.
* **ReadFileUploadNotification**, die ontvangt meldingen over het uploaden van bestand vanaf uw hub IoT.

> [AZURE.NOTE] IoT Hub ondersteunt veel platforms voor apparaten en talen (inclusief C, Java en Javascript) via Azure IoT apparaat SDK's. Raadpleeg het [Azure IoT Developer Center] voor stapsgewijze instructies voor het koppelen van uw apparaat naar de code die wordt weergegeven in deze zelfstudie, en in het algemeen op Azure IoT Hub.

Wilt u deze zelfstudie hebt voltooid, moet u het volgende:

+ Microsoft Visual Studio 2015 verlengt,

+ Een actieve Azure-account. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.)

## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Een account Azure opslagruimte op IoT Hub koppelen

Aangezien het gesimuleerd apparaat kunt u een bestand naar een blob geüpload, moet u een [Opslag van Azure] -account dat is gekoppeld aan IoT Hub hebben. Wanneer u een opslag van Azure-account aan een IoT-hub koppelen, kan de hub een SAS URI die een apparaat gebruiken kunt voor het veilig een bestand uploaden naar een container blob genereren. De service IoT Hub en de apparaat SDK's coördineren het proces dat de URI SAS genereert en beschikbaar zijn op een apparaat gebruiken om een bestand te uploaden.

Volg de instructies in [bestandsupload configureren met behulp van de Azure portal] [ lnk-configure-upload] aan een account Azure opslag op uw hub IoT koppelen.

## <a name="upload-a-file-from-a-simulated-device"></a>Een bestand vanaf een apparaat gesimuleerd uploaden

In dit gedeelte vindt u de apparaattoepassing gesimuleerd wijzigen u in de [Cloud-to-Device berichten met IoT Hub verzenden] naar de cloud-to-device berichten ontvangt van de hub IoT hebt gemaakt.

1. Visual Studio, met de rechtermuisknop op het project **SimulatedDevice** , klikt u op **toevoegen**en klik vervolgens op **Bestaand Item**. Ga naar het afbeeldingsbestand en opnemen in uw project. Deze zelfstudie wordt ervan uitgegaan dat de afbeelding heet `image.jpg`.

2. Met de rechtermuisknop op de afbeelding en klik vervolgens op **Eigenschappen**. Zorg dat **kopiëren naar uitvoer Directory** is ingesteld op **altijd kopiëren**.

    ![][1]

3. In het bestand **Program.cs** , moet u de volgende instructies toevoegen aan de bovenkant van het bestand:

        using System.IO;

4. De volgende methode toevoegen aan de klas **programma** :
         
        private static async void SendToBlobAsync()
        {
            string fileName = "image.jpg";
            Console.WriteLine("Uploading file: {0}", fileName);
            var watch = System.Diagnostics.Stopwatch.StartNew();

            using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
            {
                await deviceClient.UploadToBlobAsync(fileName, sourceData);
            }

            watch.Stop();
            Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
        }

    De `UploadToBlobAsync` methode worden in de naam en de stream bron van het bestand te uploaden en omgaat met het uploaden naar opslag. De consoletoepassing wordt weergegeven hoe lang die het duurt het bestand te uploaden.

5. De volgende methode in de methode **Main** toevoegen rechts voordat de `Console.ReadLine()` lijn:

        SendToBlobAsync();

> [AZURE.NOTE] Voor de eenvoudig mogelijk te houden, wordt deze zelfstudie opnieuw beleid niet geïmplementeerd. In productiecode, moet u opnieuw beleid (zoals exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke foutenstructuuranalyse afhandelen]implementeren.

## <a name="receive-a-file-upload-notification"></a>Ontvangen een melding van bestand uploaden

In deze sectie schrijft u een Windows-console-app dat bestand uploaden-meldingen van IoT Hub ontvangt.

1. In de huidige Visual Studio-oplossing, door een nieuw Visual C# Windows-project te maken met behulp van de projectsjabloon **Console-toepassing** . De naam van het project **ReadFileUploadNotification**.

    ![Nieuw project in Visual Studio][2]

2. In Solution Explorer met de rechtermuisknop op het project **ReadFileUploadNotification** en klik vervolgens op **NuGet-pakketten beheren**.

    Hiermee worden weergegeven met het venster NuGet-pakketten beheren.

2. Zoeken naar `Microsoft.Azure.Devices`, klikt u op **installeren**en accepteer de gebruiksvoorwaarden. 

    Dit is gedownload, installaties en voegt een verwijzing naar de [IoT van Azure - Service SDK NuGet pakket] in het project **ReadFileUploadNotification** .

3. In het bestand **Program.cs** , moet u de volgende instructies toevoegen aan de bovenkant van het bestand:

        using Microsoft.Azure.Devices;

4. De volgende velden toevoegen aan de klas **programma** . Vervang de aanduidingswaarde van de tijdelijke met de verbindingsreeks van IoT hub van het [aan de slag met IoT Hub]:

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
        
5. De volgende methode toevoegen aan de klas **programma** :
   
        private async static Task ReceiveFileUploadNotificationAsync()
        {
            var notificationReceiver = serviceClient.GetFileNotificationReceiver();

            Console.WriteLine("\nReceiving file upload notification from service");
            while (true)
            {
                var fileUploadNotification = await notificationReceiver.ReceiveAsync();
                if (fileUploadNotification == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
                Console.ResetColor();

                await notificationReceiver.CompleteAsync(fileUploadNotification);
            }
        }

    Houd er rekening mee dat het patroon ontvangen, dezelfde gebruikt is voor het ontvangen van berichten cloud-to-device vanuit de app apparaat.

6. Tot slot toevoegen de volgende regels aan de methode **Main** :

        Console.WriteLine("Receive file upload notifications\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        ReceiveFileUploadNotificationAsync().Wait();
        Console.ReadLine();

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren de toepassingen.

1. Visual Studio, met de rechtermuisknop op uw oplossing en selecteer **opstarten instellen projecten**. Selecteer **meerdere opstarten projecten**en vervolgens de actie **starten** voor **ReadFileUploadNotification** en **SimulatedDevice**.

2. Druk op **F5**. Beide toepassingen moeten beginnen. Hier ziet u de upload is voltooid in één console-app en het meldingsbericht voor de upload worden ontvangen door de andere console-app. U kunt de [Azure-portal] of Visual Studio Server Explorer gebruiken om te controleren op de aanwezigheid van de geüploade bestand in uw account Azure Storage.

  ![][50]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe om te profiteren van de mogelijkheden van de bestand uploaden van IoT Hub te vereenvoudigen bestandsuploads vanaf apparaten. U kunt doorgaan met het verkennen van IoT hub functies en scenario's met de volgende artikelen:

- [Een hub IoT via programmacode maken][lnk-create-hub]
- [Inleiding tot C SDK][lnk-c-sdk]
- [IoT Hub SDK 's][lnk-sdks]

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Een apparaat met de SDK van de Gateway IoT simuleren][lnk-gateway]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/create-identity-csharp1.png

<!-- Links -->

[Azure-portal]: https://portal.azure.com/

[Azure gegevens Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[Hadoop]: https://azure.microsoft.com/documentation/services/hdinsight/

[Berichten van de Cloud-naar-apparaat met IoT Hub verzenden]: iot-hub-csharp-csharp-c2d.md
[CMYK-apparaat in Cloud-berichten]: iot-hub-csharp-csharp-process-d2c.md
[Aan de slag met IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot

[Tijdelijke foutenstructuuranalyse afhandeling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-opslag]: ../storage/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT - Service SDK NuGet pakket]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md


