<properties
    pageTitle="Eigenschappen van de twee gebruiken | Microsoft Azure"
    description="Deze zelfstudie wordt getoond hoe dubbele eigenschappen kunt gebruiken"
    services="iot-hub"
    documentationCenter=".net"
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

# <a name="tutorial-use-desired-properties-to-configure-devices-preview"></a>Zelfstudie: De gewenste eigenschappen gebruiken voor het configureren van apparaten (preview)

[AZURE.INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Aan het einde van deze zelfstudie hebt u twee Node.js console-toepassingen:

* **SimulateDeviceConfiguration.js**, een gesimuleerd apparaat-app die een update van de gewenste configuratie wacht en de status van een updateproces gesimuleerd configuratie-rapporten.
* **SetDesiredConfigurationAndQuery.js**, een Node.js app deel uit moeten worden uitgevoerd vanuit de back-end, waarin de gewenste configuratie instellen op een apparaat en het updateproces configuratie-query's.

> [AZURE.NOTE] Het artikel [IoT Hub SDK's] [ lnk-hub-sdks] vindt u informatie over de verschillende SDK's waarmee u kunt u zowel apparaat en back-end-toepassingen kunt maken.

Deze zelfstudie hebt u het volgende nodig:

+ Node.js versie 0.10.x of hoger.

+ Een actieve Azure-account. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.)

Als u de [aan de slag met apparaat twins] gevolgd[ lnk-twin-tutorial] zelfstudie hebt u al een apparaat management ingeschakeld hub en een apparaat identiteit genoemd **myDeviceId**; en gaat u verder met het [maken van de app gesimuleerd apparaat] [ lnk-how-to-configure-createapp] sectie.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-simulated-device-app"></a>De app gesimuleerd apparaat maken

In dit gedeelte maakt u een Node.js console-app die is verbonden met uw hub als **myDeviceId**, wacht op een gewenste configuratie-update en vervolgens rapporten updates op het updateproces gesimuleerd configuratie.

1. Maak een nieuwe lege map **simulatedeviceconfiguration**. Klik in de map **simulatedeviceconfiguration** door een nieuwe package.json-bestand met de volgende opdracht in bij de opdrachtprompt te maken. Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **simulatedeviceconfiguration** , voer de volgende opdracht voor het installeren van de **azure-iot-apparaat**en **azure-iot-apparaat-mqtt** pakket:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Met een teksteditor, maakt u een nieuw **SimulateDeviceConfiguration.js** -bestand in de map **simulatedeviceconfiguration** .

4. De volgende code toevoegen aan het bestand **SimulateDeviceConfiguration.js** te vervangen door de **{verbindingsreeks apparaat}** tijdelijke aanduiding met de verbindingsreeks die u hebt gekopieerd wanneer u de **myDeviceId** apparaat identiteit gemaakt:

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });

    De **Client** -object bevat alle methoden die zijn vereist voor interactie met apparaat twins van het apparaat. De vorige code, wanneer deze wordt het object **Client** geïnitialiseerd haalt de dubbele voor **myDeviceId**en gevoegd een handler voor het bijwerken op de gewenste eigenschappen. De handler wordt gecontroleerd of er een wijzigingsaanvraag werkelijke configuratie door de configIds vergelijken en dan roept een methode waarmee wijzigen van de configuratie kan worden gestart.

    Houd er rekening mee dat omdat dit eenvoudiger, de voorgaande code gebruikt een standaard vastgelegde voor de eerste configuratie. Een reële app eenWeergave waarschijnlijk die configuratie van een lokale opslag.
    
    > [AZURE.IMPORTANT] Gewenste eigenschap wijzigingsgebeurtenissen worden altijd eenmaal dat bij de apparaatverbinding, zorgt u ervoor dat u controleert of er een werkelijke wijziging in de gewenste eigenschappen voordat u een actie uitvoert.

5. Toevoegen van de volgende methoden voordat u de `client.open()` aanroep:

        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";

            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }

        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
            
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;

            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };

    De methode **initConfigChange** gerapporteerde eigenschappen op het object lokale dubbele bijwerkt met het verzoek van de update config en de status wordt ingesteld als **in behandeling**en de dubbele apparaat op de service wordt bijgewerkt. Nadat de dubbele succes zijn bijgewerkt, wordt een langdurige proces dat wordt beëindigd als de uitvoering van **completeConfigChange**gesimuleerd. Deze methode werkt het lokale dubbele gerapporteerde eigenschappen de status is ingesteld op **Success** en verwijderen van het object **pendingConfig** . Er vervolgens de dubbele over de service wordt bijgewerkt.

    Houd er rekening mee dat, om op te slaan bandbreedte, gerapporteerd eigenschappen worden bijgewerkt door het opgeven van alleen de eigenschappen voor (benoemde **patch** in de bovenstaande code), in plaats van het vervangen van het hele document worden gewijzigd.

    > [AZURE.NOTE] Deze zelfstudie bevat niet alle gedrag voor gelijktijdige configuratie updates simuleren. Sommige updateprocessen configuratie mogelijk zijn voor wijzigingen van doelconfiguratie terwijl de update wordt uitgevoerd, anderen mogelijk moet u deze in de wachtrij en anderen met een fout kunnen weigeren. Controleer of het gewenste gedrag voor uw specifieke configuratieproces aandachtspunten en de juiste logica toevoegen voordat u begint wijzigen van de configuratie.

6. De app apparaat uitgevoerd:

        node SimulateDeviceConfiguration.js

    Ziet u het bericht `retrieved device twin`. Houd de app actief.

## <a name="create-the-service-app"></a>De service-app maken

In dit gedeelte maakt u een Node.js console-app die de *gewenste eigenschappen* op de dubbele die is gekoppeld aan **myDeviceId** met een nieuwe telemetrielogboek configuratie-object wordt bijgewerkt. Vervolgens query's van het apparaat twins die zijn opgeslagen in de hub en bevat het verschil tussen de gewenste en gerapporteerde configuraties van het apparaat.

1. Maak een nieuwe lege map **setdesiredandqueryapp**. Klik in de map **setdesiredandqueryapp** door een nieuwe package.json-bestand met de volgende opdracht in bij de opdrachtprompt te maken. Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **setdesiredandqueryapp** , voert u de volgende opdracht uit het pakket **azure-iothub** installeren:

    ```
    npm install azure-iothub@dtpreview node-uuid --save
    ```

3. Met een teksteditor, maakt u een nieuw **SetDesiredAndQuery.js** -bestand in de map **addtagsandqueryapp** .

4. De volgende code toevoegen aan het bestand **SetDesiredAndQuery.js** te vervangen door de **{verbindingsreeks service}** tijdelijke aanduiding met de verbindingsreeks die u hebt gekopieerd wanneer u uw hub gemaakt:

        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{service connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
         
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });
            

    De **register** -object bevat alle methoden die zijn vereist voor interactie met apparaat twins van de service. De vorige code, wanneer deze wordt de **register** -object, geïnitialiseerd haalt de dubbele voor **myDeviceId**en de gewenste eigenschappen bijwerkt met een nieuw telemetrielogboek configuratie-object. Daarna wordt aangeroepen de gebeurtenis van de functie **queryTwins** 10 seconden.

    > [AZURE.IMPORTANT] Deze toepassing query IoT Hub elke 10 seconden ter illustratie. Met query's op gebruikerspagina rapporten genereren over veel apparaten, en niet op het detecteren van wijzigingen. Als uw oplossing moet realtime meldingen over apparaatgebeurtenissen met de [apparaat-naar-cloud berichten][lnk-d2c].

5. Toevoegen van de volgende code rechts voordat de `registry.getDeviceTwin()` aanroep voor de uitvoering van de functie **queryTwins** :

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };

    De vorige code query's de twins die zijn opgeslagen in de hub en op de gewenste en gerapporteerde telemetrielogboek configuraties wordt afgedrukt. Verwijzen naar de [querytaal IoT Hub] [ lnk-query] wilt weten hoe u uitgebreide rapporten genereren op al uw apparaten.


8. Met **SimulateDeviceConfiguration.js** actief is, voert u de toepassing met:

        node SetDesiredAndQuery.js 5m

    U ziet nu de gerapporteerde configuratie veranderd ten opzichte van **Success** naar **in behandeling** in **Success** nogmaals met de nieuwe actieve verzenden frequentie van vijf minuten in plaats van 24 uur.

    > [AZURE.IMPORTANT] Er treedt een vertraging van maximaal minuten tussen het apparaat rapport betrekking heeft en het queryresultaat. Hiermee wordt de query-infrastructuur voor gebruik met zeer hoog schaal inschakelen. Om op te halen consistente weergaven van een één dubbele de methode **getDeviceTwin** gebruiken in de klas **register** .

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie kunt u een gewenste configuratie als de *gewenste eigenschappen* instellen vanuit een back-enddatabase-toepassing, en een app gesimuleerd apparaat om op te sporen die wijzigen en een meerdere stappen updateproces rapporteren van de status ervan als *gerapporteerd eigenschappen* aan de dubbele simuleren hebt geschreven.

Gebruik van de volgende bronnen voor meer informatie over hoe u:

- telemetrielogboek verzenden met apparaten waarop de [aan de slag met IoT Hub] [ lnk-iothub-getstarted] zelfstudie
- plannen of bewerkingen uitvoeren op grote sets met apparaten Zie de [Gebruik taken voor het plannen en uitzenden apparaatbewerkingen] [ lnk-schedule-jobs] zelfstudie.
- apparaten interactief (zoals inschakelen wanneer u een ventilator via een app door de gebruiker ingestelde), met de [rechtstreekse methoden gebruiken] bepalen[ lnk-methods-tutorial] zelfstudie.


<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
