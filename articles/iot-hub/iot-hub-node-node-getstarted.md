<properties
    pageTitle="Azure IoT Hub voor Node.js aan de slag | Microsoft Azure"
    description="Azure IoT Hub met Node.js aan de slag zelfstudie. Azure IoT Hub en Node.js met de Microsoft Azure IoT SDK's gebruiken om een oplossing Internet van zaken te implementeren."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-nodejs"></a>Aan de slag met Azure IoT Hub voor Node.js

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Aan het einde van deze zelfstudie hebt u drie Node.js console-toepassingen:

* **CreateDeviceIdentity.js**, die Hiermee maakt u een apparaat-identiteit en de bijbehorende beveiliging-toets om te koppelen van een gesimuleerd apparaat.
* **ReadDeviceToCloudMessages.js**, waarin de telemetrielogboek door uw gesimuleerd apparaat verzonden.
* **SimulatedDevice.js**, die is verbonden met uw IoT hub voor de identiteit van het apparaat eerder hebt gemaakt, en verzendt een bericht telemetrielogboek elke tweede gebruiken het AMQP-protocol.

> [AZURE.NOTE] Het artikel [IoT Hub SDK's] [ lnk-hub-sdks] vindt u informatie over de verschillende SDK's die u gebruiken kunt om beide toepassingen om uit te voeren naar apparaten en uw oplossing back-end te maken.

Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

+ Node.js versie 0.10.x of hoger.

+ Een actieve Azure-account. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

U hebt nu uw hub IoT gemaakt. U hebt de hostnaam IoT Hub en de verbindingsreeks IoT Hub die u nodig hebt voor de rest van deze zelfstudie.

## <a name="create-a-device-identity"></a>Een identiteit apparaat maken

In deze sectie, een Node.js console app maakt die u Hiermee maakt u een apparaat-id in het register identiteit in de hub IoT. Een apparaat verbinding geen met IoT-hub, tenzij er een vermelding in het register van de identiteit apparaat. Raadpleeg de sectie **Apparaat identiteit register** van de [Handleiding voor ontwikkelaars van IoT Hub] [ lnk-devguide-identity] voor meer informatie. Wanneer u deze consoletoepassing uitvoert, wordt een unieke apparaat-ID en de sleutel die uw apparaat gebruiken kunt om zichzelf te identificeren bij het apparaat naar cloud-berichten op IoT Hub verzenden gegenereerd.

1. Maak een nieuwe lege map **createdeviceidentity**. Klik in de map **createdeviceidentity** door een package.json-bestand met de volgende opdracht in bij de opdrachtprompt te maken. Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **createdeviceidentity** , voert u de volgende opdracht uit het pakket van **azure-iothub** Service SDK installeren:

    ```
    npm install azure-iothub --save
    ```

3. Met een teksteditor, maakt u een bestand **CreateDeviceIdentity.js** in de map **createdeviceidentity** .

4. Voeg de volgende `require` instructie aan het begin van het bestand **CreateDeviceIdentity.js** :

    ```
    'use strict';
    
    var iothub = require('azure-iothub');
    ```

5. De volgende code toevoegen aan het bestand **CreateDeviceIdentity.js** en vervang de waarde van de tijdelijke aanduiding voor door de verbindingsreeks voor de IoT hub die u in de vorige sectie hebt gemaakt: 

    ```
    var connectionString = '{iothub connection string}';
    
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. Voeg de volgende code als u wilt maken van een apparaat-definitie in het register van de identiteit apparaat in de hub IoT. Deze code maakt u een apparaat als het apparaat-id bestaat niet in het register hebben, anders wordt de sleutel van de bestaande apparaat:

    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });

    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device id: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.SymmetricKey.primaryKey);
      }
    }
    ```

7. Opslaan en sluit **CreateDeviceIdentity.js** -bestand.

8. Als u wilt de toepassing **createdeviceidentity** uitvoert, de volgende opdracht uit bij de opdrachtprompt worden uitgevoerd in de map createdeviceidentity:

    ```
    node CreateDeviceIdentity.js 
    ```

9. Noteer de **apparaat-id** en het **apparaat-toets**. U moet deze waarden later bij het maken van een toepassing die is verbonden met IoT Hub als een apparaat.

> [AZURE.NOTE] Er worden alleen het IoT Hub identiteit register opgeslagen apparaat identiteiten voor beveiligde toegang tot de hub. Apparaat-id's en sleutels wilt gebruiken als beveiligingsreferenties en een vlag ingeschakeld/uitgeschakeld, die u gebruiken kunt voor het uitschakelen van toegang voor een individuele apparaat wordt opgeslagen. Als de toepassing voor de opslag van andere metagegevens apparaat-specifieke, moet deze een winkel toepassingsspecifieke gebruiken. Verwijzen naar de [Handleiding voor ontwikkelaars van IoT Hub] [ lnk-devguide-identity] voor meer informatie.

## <a name="receive-device-to-cloud-messages"></a>Apparaat-naar-cloud berichten ontvangt

In deze sectie, een Node.js console app maakt die u apparaat-naar-cloud berichten van IoT Hub leest. Een hub IoT beschrijft een [Gebeurtenis Hubs][lnk-event-hubs-overview]-compatibele eindpunt zodat u kunt het apparaat naar cloud berichten lezen. Als u wilt houden dingen eenvoudige, deze zelfstudie Hiermee maakt u een eenvoudige lezer die niet geschikt is voor een hoge doorvoer-implementatie. Het [proces apparaat naar cloud berichten] [ lnk-process-d2c-tutorial] zelfstudie leert u hoe u het apparaat naar cloud-berichten bij het op schaal verwerkt. De [Aan de slag met gebeurtenis Hubs] [ lnk-eventhubs-tutorial] zelfstudie biedt meer informatie over het proces berichten van gebeurtenis Hubs en is van toepassing op de eindpunten IoT Hub gebeurtenis Hub-compatibele.

> [AZURE.NOTE] Het eindpunt van de gebeurtenis Hub-compatibele voor het apparaat naar cloud berichten altijd leest gebruikt het AMQP-protocol.

1. Maak een nieuwe lege map **readdevicetocloudmessages**. Klik in de map **readdevicetocloudmessages** door een package.json-bestand met de volgende opdracht in bij de opdrachtprompt te maken. Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **readdevicetocloudmessages** , voert u de volgende opdracht uit het pakket **azure-gebeurtenis-hubs** installeren:

    ```
    npm install azure-event-hubs --save
    ```

3. Met een teksteditor, maakt u een bestand **ReadDeviceToCloudMessages.js** in de map **readdevicetocloudmessages** .

4. Voeg de volgende `require` instructies aan het begin van het bestand **ReadDeviceToCloudMessages.js** :

    ```
    'use strict';

    var EventHubClient = require('azure-event-hubs').Client;
    ```

5. De volgende variabelen declareren toevoegen en vervang de waarde van de tijdelijke aanduiding voor door de verbindingsreeks voor uw hub IoT:

    ```
    var connectionString = '{iothub connection string}';
    ```

6. De volgende twee functies die uitvoer naar de console afdrukken toevoegen:

    ```
    var printError = function (err) {
      console.log(err.message);
    };

    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```

7. Voeg de volgende code om de **EventHubClient**maken, opent u de verbinding met uw IoT Hub en een ontvanger voor elke partition maken. Deze toepassing wordt een filter gebruikt bij het maken van een ontvanger zodat de ontvanger alleen wordt gelezen berichten die zijn verzonden naar IoT Hub nadat de ontvanger is gestart. Dit filter is handig in een testomgeving zodat u alleen de huidige set van berichten bekijken. In een productieomgeving, uw code moet ervoor zorgen dat alle berichten - worden verwerkt raadpleegt u [het verwerken van IoT Hub apparaat naar cloud berichten] [ lnk-process-d2c-tutorial] zelfstudie voor meer informatie:

    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```

8. Opslaan en sluit het bestand **ReadDeviceToCloudMessages.js** .

## <a name="create-a-simulated-device-app"></a>Een app gesimuleerd apparaat maken

In deze sectie, een Node.js console app maakt die u een apparaat dat apparaat naar cloud berichten naar een hub IoT verzendt simuleert.

1. Maak een nieuwe lege map **simulateddevice**. Klik in de map **simulateddevice** door een package.json-bestand met de volgende opdracht in bij de opdrachtprompt te maken. Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **simulateddevice** , voer de volgende opdracht voor het installeren van de **azure-iot-apparaat** apparaat SDK het pakket en **azure-iot-apparaat-amqp** :

    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```

3. Met een teksteditor, maakt u een nieuw **SimulatedDevice.js** -bestand in de map **simulateddevice** .

4. Voeg de volgende `require` instructies aan het begin van het bestand **SimulatedDevice.js** :

    ```
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

5. Voeg een variabele **connectionString** en deze gebruiken om u te maken van een apparaat-client. **{Youriothostname}** vervangen door de naam van de hub IoT u de sectie *maken een IoT Hub* hebt gemaakt. **{Yourdevicekey}** vervangen door de apparaat-sleutelwaarde die u hebt gegenereerd in de sectie *maken een identiteit apparaat* :

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
    
    var client = clientFromConnectionString(connectionString);
    ```

6. Voeg de volgende functie om uitvoer van de toepassing weer te geven:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Een terugbellen maken en gebruiken van de functie **setInterval** naar een nieuw bericht verzenden naar uw hub IoT elke tweede:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');

        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```

8. De verbinding met uw IoT Hub openen en beginnen met het verzenden van berichten:

    ```
    client.open(connectCallback);
    ```

9. Opslaan en sluit het bestand **SimulatedDevice.js** .

> [AZURE.NOTE] Als u wilt houden dingen eenvoudige, wordt deze zelfstudie opnieuw beleid niet geïmplementeerd. In productiecode, moet u opnieuw beleid (zoals een exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke foutenstructuuranalyse afhandelen]implementeren[lnk-transient-faults].


## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren de toepassingen.

1. Voer de volgende opdracht naar begint met het controleren van de hub IoT-opdrachtprompt in de map **readdevicetocloudmessages** :

    ```
    node ReadDeviceToCloudMessages.js 
    ```

    ![Node.js IoT Hub service-clienttoepassing voor het apparaat naar cloud berichten worden gecontroleerd][7]

2. Voer de volgende opdracht om te beginnen met het verzenden van telemetriegegevens op uw IoT-hub-opdrachtprompt in de map **simulateddevice** :

    ```
    node SimulatedDevice.js
    ```

    ![Node.js IoT Hub device-clienttoepassing apparaat naar cloud berichten te verzenden][8]

3. De tegel voor **Gebruik** in de [portal van Azure] [ lnk-portal] bevat het nummer van berichten die zijn verzonden naar de hub:

    ![Azure portal gebruik tegel met aantal aan IoT Hub verzonden berichten][43]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie kunt u een nieuwe IoT hub geconfigureerd in de portal van Azure, en vervolgens de identiteit van een apparaat in de hub identiteit register gemaakt. U deze identiteit apparaat gebruikt om de app gesimuleerd apparaat apparaat naar cloud berichten naar de hub te verzenden. U ook een app waarin de berichten ontvangen door de hub hebt gemaakt. 

Zie kunt doorgaan met het aan de slag met IoT Hub en verkennen van andere IoT scenario's:

- [Verbinding maken van uw apparaat][lnk-connect-device]
- [Aan de slag met Apparaatbeheer][lnk-device-management]
- [Aan de slag met de SDK van de Gateway IoT][lnk-gateway-SDK]

Als u wilt weten hoe u uw oplossing IoT uitbreiden en apparaat-naar-cloud-berichten bij het op schaal verwerkt, raadpleegt u het [proces apparaat naar cloud berichten] [ lnk-process-d2c-tutorial] zelfstudie.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
