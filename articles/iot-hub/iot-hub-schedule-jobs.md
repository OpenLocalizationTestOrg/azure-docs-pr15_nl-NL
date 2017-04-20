<properties
 pageTitle="Hoe u taken plant | Microsoft Azure"
 description="Deze zelfstudie leert u hoe u het taken plannen"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="tutorial-schedule-and-broadcast-jobs-preview"></a>Zelfstudie: Plannen en uitzenden van taken (preview)

## <a name="introduction"></a>Inleiding
Azure IoT Hub heeft een volledig beheerde service waarmee een back-end toepassing maken en bijhouden van taken die plannen en miljoenen apparaten bijwerken.  Taken kunnen worden gebruikt voor de volgende bewerkingen uit:

- Dubbele gewenst apparaateigenschappen bijwerken
- Apparaat dubbele labels bijwerken
- Methoden voor cloud-naar-apparaat

Een taak worden in dat geval terugloopt op een van deze acties en de voortgang van execution ten opzichte van een reeks apparaten, die is gedefinieerd door een dubbele query.  Bijvoorbeeld kunt behulp van een taak een toepassing back-end aanroepen een methode opnieuw opstarten op 10.000 apparaten, aangegeven door een dubbele query en gepland op een later tijdstip.  Van die toepassing kunt vervolgens voortgang bijhouden, zoals elk van deze apparaten, ontvangen en de methode opnieuw uitvoeren.

Meer informatie over elk van deze mogelijkheden in deze artikelen:

- Dubbele apparaat en eigenschappen: [aan de slag met dubbele] [ lnk-get-started-twin] en [Zelfstudie: het gebruik van dubbele eigenschappen][lnk-twin-props]
- Cloud-to-device methoden: [handleiding voor ontwikkelaars - directe methoden] [ lnk-dev-methods] en [Zelfstudie: C2D methoden][lnk-c2d-methods]

Deze zelfstudie wordt getoond hoe naar:

- Maak een gesimuleerd apparaat met een directe methode waarmee lockDoor die kan worden aangeroepen door de toepassing weer einde.
- Maak een consoletoepassing waarin de directe methode lockDoor-oproepen op het gesimuleerd apparaat behulp van een taak en updates van de eigenschappen van het gewenste dubbele behulp van een taak apparaat.

Aan het einde van deze zelfstudie hebt u twee Node.js console-toepassingen:

**simDevice.js**, die is verbonden met uw IoT hub voor de identiteit van het apparaat en ontvangt een lockDoor directe methode.

**scheduleJobService.js**, die een directe methode-oproepen op het apparaat dat gesimuleerd en bijwerken van de twee disired eigenschappen behulp van een taak.

Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

Node.js versie 0.12.x of later <br/>  [Voorbereiden van uw ontwikkelomgeving] [ lnk-dev-setup] wordt uitgelegd hoe u bij het installeren van Node.js voor deze zelfstudie in Windows of Linux.

Een actieve Azure-account. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Een app gesimuleerd apparaat maken

In deze sectie, een Node.js console app maakt die u moet reageren op een directe methode aangeroepen door de cloud, wat inhoudt dat geactiveerd gesimuleerd apparaat opnieuw opstarten en gebruik de dubbele apparaat gerapporteerd eigenschappen die u kunt het inschakelen van apparaat dubbele query's naar welke apparaten en wanneer ze laatst opnieuw gestart.

1. Maak een nieuwe lege map **simDevice**.  Klik in de map **simDevice** door een package.json-bestand met de volgende opdracht in bij de opdrachtprompt te maken.  Alle standaardwaarden accepteren:

    ```
    npm init
    ```
    
2. Bij de opdrachtprompt in de map **simDevice** , voer de volgende opdracht voor het installeren van de **azure-iot-apparaat** apparaat SDK het pakket en **azure-iot-apparaat-mqtt** :

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Met een teksteditor, maakt u een nieuw **simDevice.js** -bestand in de map **simDevice** .

4. Toevoegen aan de volgende 'vereist' instructies aan het begin van het bestand **simDevice.js** :

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Een variabele **connectionString** toevoegen en deze gebruiken om u te maken van een apparaat-client.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Voeg de volgende functie voor het verwerken van de methode lockDoor.

    ```
    var onLockDoor = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        console.log('Locking Door!');
    };
    ```

7. Voeg de volgende code als u wilt de handler voor de methode lockDoor registreren.

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub.  Waiting for reboot direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
    
8. Opslaan en sluit het bestand **simDevice.js** .

> [AZURE.NOTE] Als u wilt houden dingen eenvoudige, wordt deze zelfstudie opnieuw beleid niet geïmplementeerd. In productiecode, moet u opnieuw beleid (zoals een exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke foutenstructuuranalyse afhandelen]implementeren[lnk-transient-faults].

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-twins-properties"></a>Taken voor het bellen van een directe methode en bijwerken van een dubbele eigenschappen plannen

In deze sectie, die u kunt een Node.js console-app waarmee een externe lockDoor op een apparaat met een directe methode maken en bijwerken van de twee eigenschappen.

1. Maak een nieuwe lege map **scheduleJobService**.  Klik in de map **scheduleJobService** door een package.json-bestand met de volgende opdracht in bij de opdrachtprompt te maken.  Alle standaardwaarden accepteren:

    ```
    npm init
    ```
    
2. Bij de opdrachtprompt in de map **scheduleJobService** , voer de volgende opdracht voor het installeren van de **azure-iothub** apparaat SDK het pakket en **azure-iot-apparaat-mqtt** :

    ```
    npm install azure-iothub@dtpreview uuid --save
    ```
    
3. Met een teksteditor, maakt u een nieuw **scheduleJobService.js** -bestand in de map **scheduleJobService** .

4. Toevoegen aan de volgende 'vereist' instructies aan het begin van het bestand **dmpatterns_gscheduleJobServiceetstarted_service.js** :

    ```
    'use strict';

    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```

5. De volgende variabele declaraties toevoegen en de waarden van de tijdelijke aanduiding vervangen:

    ```
    var connectionString = '{iothubconnectionstring}';
    var deviceArray = ['myDeviceId'];
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
    
6. Voeg de volgende functie die wordt gebruikt om de uitvoering van de taak te houden:

    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```

7. De volgende code voor het plannen van de taak die de methode apparaat hebt toegevoegd:

    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        timeoutInSeconds: 45
    };

    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                deviceArray,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
        
8. De volgende code voor het plannen van de taak voor het bijwerken van de twee hebt toegevoegd:

    ```
    var twinPatch = {
        etag: '*',
        desired: {
            building: '43',
            floor: 3
        }
    };

    var twinJobId = uuid.v4();

    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                deviceArray,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
    
9. Opslaan en sluit het bestand **scheduleJobService.js** .

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren de toepassingen.

1. Bij de opdrachtprompt in de map **simDevice** , voer de volgende opdracht om te beginnen met gebruikers zonder licentie voor de directe methode opnieuw opstarten.

    ```
    node simDevice.js
    ```

2. Voer de volgende opdracht aan trigger de extern opnieuw opstarten en de query voor het zoeken van de laatste apparaat opnieuw tijd opstarten bij de opdrachtprompt in de map **scheduleJobService** .

    ```
    node scheduleJobService.js
    ```

3. Hier ziet u de uitvoer van apparaat en back-end-toepassingen.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt u een taak een directe methode aan een apparaat en de update van de eigenschappen van het apparaat dubbele plannen.

Als u wilt doorgaan met aan de slag met IoT Hub en apparaat management patronen zoals remote via de air firmware-update, raadpleegt u:

[Zelfstudie: Hoe u een firmware-update][lnk-fwupdate]

Als u wilt doorgaan met aan de slag met IoT-Hub, raadpleegt u [aan de slag met de SDK van de Gateway IoT][lnk-gateway-SDK].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-firmware-update.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx