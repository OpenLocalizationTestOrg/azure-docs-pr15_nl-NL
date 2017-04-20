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

Aan het einde van deze zelfstudie hebt u twee Node.js console-toepassingen:

* **AddTagsAndQuery.js**, een Node.js app deel uit moeten worden uitgevoerd vanuit de back-end, die wordt toegevoegd tags en apparaat twins-query's.
* **TwinSimulatedDevice.js**, een Node.js-app dat overeenkomt met een apparaat dat verbinding maakt met uw IoT hub voor de identiteit van het apparaat eerder hebt gemaakt en de voorwaarde connectivity-rapporten.

> [AZURE.NOTE] Het artikel [IoT Hub SDK's] [ lnk-hub-sdks] vindt u informatie over de verschillende SDK's waarmee u kunt u zowel apparaat en back-end-toepassingen kunt maken.

Deze zelfstudie hebt u het volgende nodig:

+ Node.js versie 0.10.x of hoger.

+ Een actieve Azure-account. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>De service-app maken

In dit gedeelte maakt u een Node.js console-app die locatie metagegevens aan de dubbele die is gekoppeld aan **myDeviceId**worden toegevoegd. Vervolgens de twins die zijn opgeslagen in de hub selecteren de apparaten zich bevindt in de Verenigde Staten en klik vervolgens op degene die het rapporteren van een mobiele verbinding-query's.

1. Maak een nieuwe lege map **addtagsandqueryapp**. Klik in de map **addtagsandqueryapp** door een nieuwe package.json-bestand met de volgende opdracht in bij de opdrachtprompt te maken. Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **addtagsandqueryapp** , voert u de volgende opdracht uit het pakket **azure-iothub** installeren:

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Met een teksteditor, maakt u een nieuw **AddTagsAndQuery.js** -bestand in de map **addtagsandqueryapp** .

4. De volgende code toevoegen aan het bestand **AddTagsAndQuery.js** te vervangen door de **{verbindingsreeks service}** tijdelijke aanduiding met de verbindingsreeks die u hebt gekopieerd wanneer u uw hub gemaakt:

        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{service hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);

        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };
             
                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });

    De **register** -object bevat alle methoden die zijn vereist voor interactie met apparaat twins van de service. De vorige code eerst geïnitialiseerd de **register** -object, en vervolgens de dubbele voor **myDeviceId**zijn opgehaald en ten slotte de labels bijgewerkt met de gewenste locatie-gegevens.

    Na het bijwerken van de desbetreffende tags roept de functie **queryTwins** .

7. Voeg de volgende code toe aan het einde van **AddTagsAndQuery.js** willen implementeren van de functie **queryTwins** :

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
            
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };

    De vorige code wordt uitgevoerd twee query's: de eerste Hiermee selecteert u alleen het apparaat twins van apparaten die zich bevindt in de **Redmond43** plant en de tweede verfijning Selecteer alleen de apparaten die ook via mobiel netwerk verbonden bent de query.

    Houd er rekening mee dat de vorige code, wanneer dat wordt gemaakt van de **query** -object, geeft u een maximum aantal geretourneerde documenten. Het **query** -object bevat een **hasMoreResults** Booleaanse eigenschap die u kunt methoden **nextAsTwin** meerdere keren om alle resultaten te halen. Een methode met de **volgende** naam is beschikbaar voor de resultaten die niet apparaat twins bijvoorbeeld de resultaten van aggregatie query's zijn.

8. Voer de toepassing met:

        node AddTagsAndQuery.js

    Hier ziet u een apparaat in de zoekresultaten voor de query vragen voor alle apparaten zich bevindt in **Redmond43** en geen voor de query die de resultaten tot apparaten die gebruikmaken van een mobiel netwerk wordt beperkt.

    ![][1]

In het volgende gedeelte maakt u een apparaat-app die de gegevens connectivity-rapporten en wijzigt u het resultaat van de query in de vorige sectie.

## <a name="create-the-device-app"></a>Het apparaat-app maken

In dit gedeelte maakt u een Node.js console-app die is verbonden met uw hub als **myDeviceId**en vervolgens wordt bijgewerkt van de twee gerapporteerd eigenschappen die u kunt de gegevens dat is verbonden met een mobiel netwerk bevatten.

> [AZURE.NOTE] Op dit moment kan zijn apparaat twins alleen toegankelijk via apparaten die verbinding maken met IoT Hub met het MQTT-protocol. Raadpleeg de [ondersteuning voor MQTT] [ lnk-devguide-mqtt] artikel voor instructies voor het converteren van bestaande apparaat app MQTT gebruiken.

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

6. Nu dat de informatie connectivity gerapporteerd, moet deze worden weergegeven in beide query's. Ga terug in de map **addtagsandqueryapp** en de query's opnieuw uitvoeren:

        node AddTagsAndQuery.js

    Deze tijd **myDeviceId** moet worden weergegeven in de resultaten van beide query.

    ![][3]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie kunt u een nieuwe IoT hub geconfigureerd in de portal van Azure, en vervolgens de identiteit van een apparaat in de hub identiteit register gemaakt. U apparaat metagegevens toegevoegd als labels uit een back-enddatabase-toepassing en een app gesimuleerd apparaat geschreven naar rapport apparaat connectivity informatie in de dubbele apparaat. U weet hoe u ook deze informatie toe aan de IoT Hub SQL-achtige querytaal doorzoeken.

Gebruik van de volgende bronnen voor meer informatie over hoe u:

- telemetrielogboek verzenden met apparaten waarop de [aan de slag met IoT Hub] [ lnk-iothub-getstarted] zelfstudie
- apparaten configureren met de gewenste eigenschappen van de twee met het [gebruik van de gewenste eigenschappen als u wilt configureren apparaten] [ lnk-twin-how-to-configure] zelfstudie
- apparaten interactief (zoals inschakelen wanneer u een ventilator via een app door de gebruiker ingestelde), met de [rechtstreekse methoden gebruiken] bepalen[ lnk-methods-tutorial] zelfstudie.

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md