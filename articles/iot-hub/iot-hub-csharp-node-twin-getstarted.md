<properties
    pageTitle="Aan de slag met twins | Microsoft Azure"
    description="Deze zelfstudie ziet u hoe u het twins gebruiken"
    services="iot-hub"
    documentationCenter="node"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="node"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="elioda"/>

# <a name="tutorial-get-started-with-device-twins-preview"></a>Zelfstudie: Aan de slag met apparaat twins (preview)

[AZURE.INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Aan het einde van deze zelfstudie hebt u een .NET en een Node.js console-toepassing:

* **AddTagsAndQuery.sln**, een .NET-app deel uit moeten worden uitgevoerd vanuit de back-end, die wordt toegevoegd tags en apparaat twins-query's.
* **TwinSimulatedDevice.js**, een Node.js-app dat overeenkomt met een apparaat dat verbinding maakt met uw IoT hub voor de identiteit van het apparaat eerder hebt gemaakt en de voorwaarde connectivity-rapporten.

> [AZURE.NOTE] Het artikel [IoT Hub SDK's] [ lnk-hub-sdks] vindt u informatie over de verschillende SDK's waarmee u kunt u zowel apparaat en back-end-toepassingen kunt maken.

Deze zelfstudie hebt u het volgende nodig:

+ Microsoft Visual Studio 2015.

+ Node.js versie 0.10.x of hoger.

+ Een actieve Azure-account. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>De service-app maken

In dit gedeelte maakt u een Node.js console-app die locatie metagegevens aan de dubbele die is gekoppeld aan **myDeviceId**worden toegevoegd. Vervolgens de twins die zijn opgeslagen in de hub selecteren de apparaten zich bevindt in de Verenigde Staten en klik vervolgens op degene die het rapporteren van een mobiele verbinding-query's.

1. Visual Studio, moet u een visuele C# klassiek Windows-bureaublad-project toevoegen naar de huidige oplossing met behulp van het project-sjabloon van **Console-toepassing** . De naam van het project **AddTagsAndQuery**.

    ![Nieuw visuele C# klassiek Windows-bureaublad-project][img-createapp]

2. In Solution Explorer met de rechtermuisknop op het project **AddTagsAndQuery** en klik vervolgens op **Nuget-pakketten beheren**.

3. Klik in het venster **Nuget Package Manager** controleert u of **opnemen voorlopige versie** is ingeschakeld, zoeken naar **microsoft.azure.devices**, selecteert u **installeren** om te installeren de de *voorlopige* versie van de **Microsoft.Azure.Devices** Inpakken en accepteer de gebruiksvoorwaarden. Deze procedure is gedownload, installaties en voegt u een verwijzing naar de [Microsoft Azure IoT Service SDK] [ lnk-nuget-service-sdk] Nuget-pakket en de bijbehorende afhankelijkheden.

    ![Nuget Package Manager-venster][img-servicenuget]

4. Voeg de volgende `using` instructies boven aan het bestand **Program.cs** :

        using Microsoft.Azure.Devices;

5. De volgende velden toevoegen aan de klas **programma** . Vervang de aanduidingswaarde van de tijdelijke met de verbindingsreeks voor de hub IoT die u in de vorige sectie hebt gemaakt.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. De volgende methode toevoegen aan de klas **programma** :

        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);

            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));

            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }

    De klasse **RegistryManager** bevat alle methoden die zijn vereist voor interactie met apparaat twins van de service. De vorige code eerst geïnitialiseerd het object **registryManager** , en vervolgens de dubbele voor **myDeviceId**zijn opgehaald en ten slotte de labels bijgewerkt met de gewenste locatie-gegevens.

    Nadat de zijn bijgewerkt, wordt deze uitgevoerd twee query's: de eerste Hiermee selecteert u alleen het apparaat twins van apparaten die zich bevindt in de **Redmond43** plant en de tweede verfijning Selecteer alleen de apparaten die ook via mobiel netwerk verbonden bent de query.

    Houd er rekening mee dat de vorige code, wanneer dat wordt gemaakt van de **query** -object, geeft u een maximum aantal geretourneerde documenten. Het **query** -object bevat een **HasMoreResults** Booleaanse eigenschap die u kunt methoden **GetNextAsTwinAsync** meerdere keren om alle resultaten te halen. Een methode met de naam van **GetNextAsJson** is beschikbaar voor de resultaten die niet apparaat twins bijvoorbeeld de resultaten van query's voor aggregatie zijn.

7. Tot slot toevoegen de volgende regels aan de methode **Main** :

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

8. Uitvoeren van deze toepassing en ziet u een apparaat in de zoekresultaten voor de query vragen voor alle apparaten zich bevindt in **Redmond43** en geen voor de query die de resultaten tot apparaten die gebruikmaken van een mobiel netwerk wordt beperkt.

    ![Queryresultaten in venster][img-addtagapp]

In het volgende gedeelte maakt u een apparaat-app die de gegevens connectivity-rapporten en wijzigt u het resultaat van de query in de vorige sectie.

## <a name="create-the-device-app"></a>Het apparaat-app maken

In dit gedeelte maakt u een Node.js console-app die is verbonden met uw hub als **myDeviceId**en vervolgens wordt bijgewerkt van de twee gerapporteerd eigenschappen die u kunt de gegevens dat is verbonden met een mobiel netwerk bevatten.

1. Maak een nieuwe lege map **reportconnectivity**. Klik in de map **reportconnectivity** door een nieuwe package.json-bestand met de volgende opdracht in bij de opdrachtprompt te maken. Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **reportconnectivity** , voer de volgende opdracht voor het installeren van de **azure-iot-apparaat**en **azure-iot-apparaat-mqtt** pakket:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Met een teksteditor, maakt u een nieuw **ReportConnectivity.js** -bestand in de map **reportconnectivity** .

4. De volgende code toevoegen aan het bestand **ReportConnectivity.js** te vervangen door de **{verbindingsreeks apparaat}** tijdelijke aanduiding met de verbindingsreeks die u hebt gekopieerd wanneer u de **myDeviceId** apparaat identiteit gemaakt:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');

            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };

                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });

    Het **Client** -object bevat alle methoden die u nodig hebt om te communiceren met apparaat twins van het apparaat. De vorige code, wanneer deze wordt geïnitialiseerd het object **Client** , de dubbele voor **myDeviceId** zijn opgehaald en de bijbehorende gerapporteerde eigenschap bijgewerkt met de verbindingsgegevens.

5. Het apparaat-app uitvoeren

        node ReportConnectivity.js

    Ziet u het bericht `twin state reported`.

6. Nu dat de informatie connectivity gerapporteerd, moet deze worden weergegeven in beide query's. De app .NET **AddTagsAndQuery** om uit te voeren van de query's opnieuw uitvoeren. Deze tijd **myDeviceId** moet worden weergegeven in de resultaten van beide query.

    ![][img-addtagapp2]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie kunt u een nieuwe IoT hub geconfigureerd in de portal van Azure, en vervolgens de identiteit van een apparaat in de hub identiteit register gemaakt. U apparaat metagegevens toegevoegd als labels uit een back-enddatabase-toepassing en een app gesimuleerd apparaat geschreven naar rapport apparaat connectivity informatie in de dubbele apparaat. U weet hoe u ook deze informatie toe aan de IoT Hub SQL-achtige querytaal doorzoeken.

Gebruik van de volgende bronnen voor meer informatie over hoe u:

- telemetrielogboek verzenden met apparaten waarop de [aan de slag met IoT Hub] [ lnk-iothub-getstarted] zelfstudie
- apparaten configureren met de gewenste eigenschappen van de twee met het [gebruik van de gewenste eigenschappen als u wilt configureren apparaten] [ lnk-twin-how-to-configure] zelfstudie
- apparaten interactief (zoals inschakelen wanneer u een ventilator via een app door de gebruiker ingestelde) bepalen met de [rechtstreekse methoden gebruiken] [ lnk-methods-tutorial] zelfstudie.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

