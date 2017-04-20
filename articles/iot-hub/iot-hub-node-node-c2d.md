<properties
    pageTitle="Verzenden van berichten van de cloud-naar-apparaat met IoT Hub | Microsoft Azure"
    description="Volg deze zelfstudie meer informatie over het gebruik van Azure IoT Hub met Java cloud-to-device-berichten te verzenden."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/23/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-nodejs"></a>Zelfstudie: Het verzenden van berichten van de cloud-naar-apparaat met IoT Hub en Node.js

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Inleiding

Azure IoT Hub heeft een volledig beheerde service waarmee inschakelen betrouwbare en veilige bidirectionele communicatie tussen miljoenen IoT apparaten en weer een toepassing beëindigen. De zelfstudie [aan de slag met IoT Hub] ziet hoe u een hub IoT maken, inrichten van een identiteit apparaat erin en code een gesimuleerd apparaat, dat wordt verzonden berichten apparaat-naar-cloud.

Deze zelfstudie dat is gebaseerd op het [aan de slag met IoT Hub]. U ziet hoe naar:

- Vanuit uw toepassing cloud back-end, door cloud-to-device berichten te verzenden naar een enkel apparaat via IoT Hub.
- Cloud-to-device ontvangen op een apparaat.
- Een back-end, bezorging bevestiging (*feedback*) aanvragen voor berichten die worden verzonden naar een apparaat IoT Hub vanuit de cloud toepassing.

U vindt meer informatie over cloud-to-device berichten in de [Handleiding voor ontwikkelaars van IoT Hub][IoT Hub Developer Guide - C2D].

Aan het einde van deze zelfstudie, moet u twee Node.js console-toepassingen uitvoert:

* **SimulatedDevice**, een gewijzigde versie van de app die is gemaakt in [aan de slag met IoT-Hub], die is verbonden met uw hub IoT en ontvangt berichten cloud-naar-apparaat.
* **SendCloudToDeviceMessage**, stuurt een bericht cloud-naar-apparaat naar het gesimuleerd apparaat via IoT-Hub, en klikt u vervolgens de bezorging-bevestiging ontvangt.

> [AZURE.NOTE] IoT Hub heeft SDK ondersteuning voor veel talen (inclusief C, Java en Javascript) tot en met Azure IoT apparaat SDK's en platforms voor apparaten. Zie voor stapsgewijze instructies voor het koppelen van uw apparaat van deze zelfstudie code en over het algemeen op Azure IoT-Hub, het [Azure IoT Developer Center].

Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

+ Node.js versie 0.10.x of hoger.

+ Een actieve Azure-account. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.)

## <a name="receive-messages-on-the-simulated-device"></a>Ontvangen van berichten op het apparaat dat gesimuleerd

In dit gedeelte vindt u de apparaattoepassing gesimuleerd wijzigen u hebt gemaakt in [aan de slag met IoT Hub] naar cloud-to-device berichten ontvangt van de hub IoT.

1. Met een teksteditor, open het bestand SimulatedDevice.js.

2. De functie **connectCallback** voor het verwerken van verzonden berichten van IoT Hub wijzigen. In dit voorbeeld wordt het apparaat altijd de **volledige** functie IoT Hub melding dat het bericht is worden verwerkt. Uw nieuwe versie van de functie **connectCallback** ziet er zo uit:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
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

    > [AZURE.NOTE] Als u HTTP in plaats van MQTT of AMQP als het transport gebruikt, het exemplaar **DeviceClient** Hiermee wordt gecontroleerd op berichten van IoT Hub zelden (minder dan elke 25 minuten). Zie voor meer informatie over de verschillen tussen MQTT, AMQP en HTTP-ondersteuning en het beperken van IoT-Hub, de [Handleiding voor ontwikkelaars van IoT Hub][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Een bericht cloud naar apparaat verzenden

In deze sectie, een Node.js console app maakt die u cloud-to-device berichten naar de app gesimuleerd apparaat verzendt. Moet u het apparaat-Id van het apparaat dat u in de [aan de slag met IoT Hub] zelfstudie hebt toegevoegd. U moet ook de verbindingsreeks voor uw IoT hub die u in de [portal van Azure]kunt vinden.

1. Maak een lege map **sendcloudtodevicemessage**. Klik in de map **sendcloudtodevicemessage** door een package.json-bestand met de volgende opdracht in bij de opdrachtprompt te maken. Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **sendcloudtodevicemessage** , voert u de volgende opdracht uit het pakket **azure-iothub** installeren:

    ```
    npm install azure-iothub --save
    ```

3. Met een teksteditor, maakt u een nieuw **SendCloudToDeviceMessage.js** -bestand in de map **sendcloudtodevicemessage** .

4. Voeg de volgende `require` instructies aan het begin van het bestand **SendCloudToDeviceMessage.js** :

    ```
    'use strict';
    
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```

5. Voeg de volgende code naar **SendCloudToDeviceMessage.js** -bestand. Vervang tijdelijke aanduiding voor tekenreekswaarde van de verbinding met de verbindingsreeks voor de IoT hub die u hebt gemaakt in de zelfstudie [aan de slag met IoT Hub] . De doel-apparaat tijdelijke aanduiding vervangen door het apparaat-id van het apparaat dat u in de [aan de slag met IoT Hub] zelfstudie hebt toegevoegd:

    ```
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';

    var serviceClient = Client.fromConnectionString(connectionString);
    ```

6. De volgende functie als u wilt afdrukken van de Bewerkingsresultaten van de wilt de console toevoegen:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. De volgende functie als u wilt afdrukken bezorging Feedbackberichten aan de console toevoegen:

    ```
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```

8. De volgende code als u wilt een bericht verzenden naar uw apparaat en het Feedbackbericht afgehandeld wanneer het apparaat dat het bericht cloud-to-device bevestigt hebt toegevoegd:

    ```
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```

7. Opslaan en sluit **SendCloudToDeviceMessage.js** -bestand.

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren de toepassingen.

1. Voer de volgende opdracht telemetrielogboek op IoT Hub verzenden en luisteren naar cloud-to-device berichten bij de opdrachtprompt in de map **simulateddevice** :

    ```
    node SimulatedDevice.js 
    ```

    ![De gesimuleerd apparaat-app uitvoeren][img-simulated-device]

2. Voer de volgende opdracht een cloud-to-device-bericht wilt verzenden en wacht totdat de bevestiging feedback-opdrachtprompt in de map **sendcloudtodevicemessage** :

    ```
    node SendCloudToDeviceMessage.js 
    ```

    ![De app als u wilt verzenden, de opdracht c2d uitvoeren][img-send-command]

    > [AZURE.NOTE] Voor de eenvoudig mogelijk te houden, wordt deze zelfstudie opnieuw beleid niet geïmplementeerd. In productiecode, moet u opnieuw beleid (zoals exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke foutenstructuuranalyse afhandelen]implementeren.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe cloud-to-device berichten verzenden en ontvangen. 

Voorbeelden van volledige end-to-end-oplossingen die IoT Hub gebruikt, raadpleegt u [Azure IoT Suite].

Meer informatie over het ontwikkelen van oplossingen met IoT-Hub, raadpleegt u de [Handleiding voor ontwikkelaars van IoT Hub].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[Aan de slag met IoT Hub]: iot-hub-node-node-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[Handleiding voor ontwikkelaars van IoT Hub]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[Tijdelijke foutenstructuuranalyse afhandeling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-portal]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/