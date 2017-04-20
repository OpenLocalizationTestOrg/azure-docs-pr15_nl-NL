<properties
 pageTitle="Directe methoden gebruiken | Microsoft Azure"
 description="Deze zelfstudie ziet u hoe u het rechtstreekse methoden gebruiken"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="nberdy"/>

# <a name="tutorial-use-direct-methods"></a>Zelfstudie: Directe methoden gebruiken

## <a name="introduction"></a>Inleiding

Azure IoT Hub heeft een volledig beheerde service betrouwbare waarmee en beveiligde bidirectionele communicatie tussen miljoenen IoT apparaten en een toepassing een back-end. Vorige zelfstudies ([aan de slag met IoT Hub] en [verzenden van berichten van de Cloud-naar-apparaat met IoT Hub]) illustreren de apparaat-naar-cloud en cloud-to-device SMS basisfunctionaliteit van IoT Hub. IoT Hub kunt u ook de mogelijkheid om aan te roepen van niet-duurzame methoden op apparaten vanuit de cloud. Methoden vertegenwoordigen een interactie aanvraag beantwoorden met een apparaat overeen met een HTTP-oproep in dat ze slagen of meteen (na een door de gebruiker opgegeven time-out) mislukken kan de gebruiker de status van het gesprek weten. [Directe methode op een apparaat] [ lnk-devguide-methods] worden methoden uitgebreider beschreven en biedt hulp over wanneer u de methoden versus berichten cloud-naar-apparaat gebruiken.

Deze zelfstudie wordt getoond hoe naar:

- Gebruik de Azure-portal maken van een hub IoT en een apparaat-identiteit maken in de hub IoT.
- Maak een gesimuleerd apparaat met een directe methode die kan worden aangeroepen door de cloud.
- Een consoletoepassing die een directe methode-op het apparaat gesimuleerd via uw hub IoT oproepen maken.

> [AZURE.NOTE] Op dit moment kan zijn rechtstreekse methoden alleen toegankelijk via apparaten die verbinding maken met IoT Hub met het MQTT-protocol. Raadpleeg de [ondersteuning voor MQTT] [ lnk-devguide-mqtt] artikel voor instructies voor het converteren van bestaande apparaat app MQTT gebruiken.

Aan het einde van deze zelfstudie hebt u twee Node.js console-toepassingen:

* **CallMethodOnDevice.js**, die een methode-oproepen op het apparaat dat gesimuleerd en wordt het antwoord wordt weergegeven.
* **SimulatedDevice.js**, die is verbonden met uw IoT hub voor de identiteit van het apparaat eerder hebt gemaakt en reageert op de methode aangeroepen door de cloud.

> [AZURE.NOTE] Het artikel [IoT Hub SDK's] [ lnk-hub-sdks] vindt u informatie over de verschillende SDK's die u gebruiken kunt om beide toepassingen om uit te voeren naar apparaten en uw oplossing back-end te maken.

Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

+ Node.js versie 0.10.x of hoger.

+ Een actieve Azure-account. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Een app gesimuleerd apparaat maken

In dit gedeelte maakt u een Node.js console-app die u moet op een methode aangeroepen door de cloud reageren.

1. Maak een nieuwe lege map **simulateddevice**. Klik in de map **simulateddevice** door een package.json-bestand met de volgende opdracht in bij de opdrachtprompt te maken. Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **simulateddevice** , voer de volgende opdracht voor het installeren van de **azure-iot-apparaat** apparaat SDK het pakket en **azure-iot-apparaat-mqtt** :

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Met een teksteditor, maakt u een nieuw **SimulatedDevice.js** -bestand in de map **simulateddevice** .

4. Voeg de volgende `require` instructies aan het begin van het bestand **SimulatedDevice.js** :

    ```
    'use strict';

    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```

5. Voeg een variabele **connectionString** en deze gebruiken om u te maken van een apparaat-client. **{Apparaat verbindingsreeks}** vervangen door de verbindingsreeks die u hebt gegenereerd in de sectie *maken een identiteit apparaat* :

    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```

6. Voeg de volgende functie implementatie van de methode op het apparaat:

    ```
    function onWriteLine(request, response) {
        console.log(request.payload);

        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

7. Open de verbinding met uw IoT hub en start geïnitialiseerd de methode luisteraar ervan af:

    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```

8. Opslaan en sluit het bestand **SimulatedDevice.js** .

> [AZURE.NOTE] Als u wilt houden dingen eenvoudige, wordt deze zelfstudie opnieuw beleid niet geïmplementeerd. In productiecode, moet u opnieuw beleid (zoals verbinding opnieuw), zoals aangegeven in het MSDN-artikel [Tijdelijke foutenstructuuranalyse afhandelen]implementeren[lnk-transient-faults].

## <a name="call-a-method-on-a-device"></a>Een methode bellen op een apparaat

In deze sectie, een Node.js console app maakt die u een methode-oproepen op het apparaat dat gesimuleerd en het antwoord selecteert, wordt weergegeven.

1. Maak een nieuwe lege map **callmethodondevice**. Klik in de map **callmethodondevice** door een package.json-bestand met de volgende opdracht in bij de opdrachtprompt te maken. Alle standaardwaarden accepteren:

    ```
    npm init
    ```

2. Bij de opdrachtprompt in de map **callmethodondevice** , voert u de volgende opdracht uit het pakket **azure-iothub** installeren:

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Met een teksteditor, maakt u een bestand **CallMethodOnDevice.js** in de map **callmethodondevice** .

4. Voeg de volgende `require` instructies aan het begin van het bestand **CallMethodOnDevice.js** :

    ```
    'use strict';

    var Client = require('azure-iothub').Client;
    ```

5. De volgende variabelen declareren toevoegen en vervang de waarde van de tijdelijke aanduiding voor door de verbindingsreeks voor uw hub IoT:

    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```

6. De client om te openen van de verbinding met uw hub IoT maken.

    ```
    var client = Client.fromConnectionString(connectionString);
    ```
    
7. De volgende functie roepen de methode apparaat en afdrukken van de reactie van het apparaat aan de console toevoegen:

    ```
    var methodParams = {
        methodName: methodName,
        payload: 'a line to be written',
        timeoutInSeconds: 30
    };

    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```

7. Opslaan en sluit het bestand **CallMethodOnDevice.js** .

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren de toepassingen.

1. Voer de volgende opdracht om te beginnen met gebruikers zonder licentie voor gesprekken met een methode uit uw IoT Hub-opdrachtprompt in de map **simulateddevice** :

    ```
    node SimulatedDevice.js
    ```

    ![][7]
    
2. Voer de volgende opdracht naar begint met het controleren van de hub IoT-opdrachtprompt in de map **callmethodondevice** :

    ```
    node CallMethodOnDevice.js 
    ```

    ![][8]
    
3. Hier ziet u het apparaat reageren op de methode door het afdrukken van het bericht en de toepassing waarin het antwoord van de methode weergave genaamd van het apparaat:

    ![][9]
    
## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie kunt u een nieuwe IoT hub geconfigureerd in de portal van Azure, en vervolgens de identiteit van een apparaat in de hub identiteit register gemaakt. U deze identiteit apparaat gebruikt om de app gesimuleerd apparaat om te reageren op methoden aangeroepen door de cloud. U ook een app dat roept methoden op het apparaat en wordt weergegeven het antwoord van het apparaat hebt gemaakt. 

Zie kunt doorgaan met het aan de slag met IoT Hub en verkennen van andere IoT scenario's:

- [Aan de slag met IoT Hub]
- [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Als u wilt weten hoe u uw oplossing en planning methode-op meerdere apparaten oproepen IoT uitbreiden, raadpleegt u de [planning en uitzenden taken] [ lnk-tutorial-jobs] zelfstudie.

<!-- Images. -->
[7]: ./media/iot-hub-c2d-methods/run-simulated-device.png
[8]: ./media/iot-hub-c2d-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-c2d-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Berichten van de Cloud-naar-apparaat met IoT Hub verzenden]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Aan de slag met IoT Hub]: iot-hub-node-node-getstarted.md
