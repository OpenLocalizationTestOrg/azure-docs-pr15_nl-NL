<properties
 pageTitle="Aan de slag met Apparaatbeheer | Microsoft Azure"
 description="Deze zelfstudie leert u hoe u het aan de slag met Apparaatbeheer op Azure IoT Hub"
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

# <a name="tutorial-get-started-with-device-management-preview"></a>Zelfstudie: Aan de slag met Apparaatbeheer (preview)

## <a name="introduction"></a>Inleiding
IoT cloud-toepassingen kunnen primitieven in Azure IoT-Hub, namelijk de dubbele apparaat en directe methoden gebruiken, extern starten en bewaken apparaat management acties op apparaten.  Dit artikel bevat instructies en code voor een IoT cloud-toepassing en een apparaat werking om te starten en een externe apparaat opnieuw opstarten met IoT Hub controleren.

Gebruiken om te extern starten en apparaat management handelingen op uw apparaten via een cloudgebaseerde, back-enddatabase app controleren, Azure IoT Hub primitieven zoals [apparaat dubbele] [ lnk-devtwin] en [cloud-naar-apparaat (C2D) methoden][lnk-c2dmethod]. Deze zelfstudie ziet u hoe een back-enddatabase-app en een apparaat samenwerken kunnen om te kunnen starten en externe apparaat opnieuw opstarten van IoT Hub controleren.

U gebruikt een methode C2D om apparaat management handelingen (zoals opnieuw opstarten, factory opnieuw instellen en firmware-update) uit een back-enddatabase-app in de cloud te starten. Het apparaat is verantwoordelijk voor:

- De methode aanvraag verzonden vanuit IoT Hub voor de verwerking.
- De bijbehorende apparaat specifieke actie op het apparaat wordt gestart.
- Eigenschappen statusupdates tot en met de dubbele apparaat leveren aan IoT Hub worden gemeld.

U kunt een back-enddatabase-app in de cloud apparaat dubbele query's rapporteren van de voortgang van uw apparaat management acties uitvoeren.

Deze zelfstudie wordt getoond hoe naar:

- Gebruik de Azure-portal maken van een IoT Hub en een apparaat-identiteit maken in de hub IoT.
- Maak een gesimuleerd apparaat met een directe methode waarmee opnieuw opstarten die kan worden aangeroepen door de cloud.
- Een consoletoepassing die de directe methode opnieuw opstarten-op het apparaat gesimuleerd via uw hub IoT oproepen maken.

Aan het einde van deze zelfstudie hebt u twee Node.js console-toepassingen:

**dmpatterns_getstarted_device.js**, dat is gekoppeld aan uw IoT hub voor de identiteit van het apparaat eerder hebt gemaakt, ontvangt een directe methode opnieuw opstarten, simuleert fysiek opnieuw opstarten en de tijd voor de laatste keer opnieuw opstarten-rapporten.

**dmpatterns_getstarted_service.js**, die een directe methode-oproepen op het apparaat dat gesimuleerd, het antwoord weergegeven en ziet u de bijgewerkte apparaat dubbele gerapporteerd eigenschappen.

Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

Node.js versie 0.12.x of later <br/>  [Voorbereiden van uw ontwikkelomgeving] [ lnk-dev-setup] wordt uitgelegd hoe u bij het installeren van Node.js voor deze zelfstudie in Windows of Linux.

Een actieve Azure-account. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Een app gesimuleerd apparaat maken

In deze sectie, een Node.js console app maakt die u moet reageren op een directe methode aangeroepen door de cloud, wat inhoudt dat geactiveerd gesimuleerd apparaat opnieuw opstarten en gebruik de dubbele apparaat gerapporteerd eigenschappen die u kunt het inschakelen van apparaat dubbele query's naar welke apparaten en wanneer ze laatst opnieuw gestart.

1. Maak een nieuwe lege map **manageddevice**.  Klik in de map **manageddevice** door een package.json-bestand met de volgende opdracht in bij de opdrachtprompt te maken.  Alle standaardwaarden accepteren:

    ```
    npm init
    ```
    
2. Bij de opdrachtprompt in de map **manageddevice** , voer de volgende opdracht om te installeren de **azure-iot-device@dtpreview** apparaat SDK-pakket en **azure-iot-device-mqtt@dtpreview** pakket:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Met een teksteditor, maakt u een nieuw **dmpatterns_getstarted_device.js** -bestand in de map **manageddevice** .

4. Toevoegen aan de volgende 'vereist' instructies aan het begin van het bestand **dmpatterns_getstarted_device.js** :

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Voeg een variabele **connectionString** en deze gebruiken om u te maken van een apparaat-client.  Vervang de verbindingsreeks door de verbindingsreeks van uw apparaat.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Voeg de volgende functie implementatie van de directe methode op het apparaat

    ```
    var onReboot = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };

        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
        
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```

7. De verbinding met uw hub IoT openen en beginnen met de directe methode luisteraar ervan af:

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
    
8. Opslaan en sluit het bestand **dmpatterns_getstarted_device.js** .

 [AZURE.NOTE] Als u wilt houden dingen eenvoudige, wordt deze zelfstudie opnieuw beleid niet geïmplementeerd. In productiecode, moet u opnieuw beleid (zoals een exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke foutenstructuuranalyse afhandelen]implementeren[lnk-transient-faults].

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Een extern opnieuw opstarten op het apparaat met behulp van een directe methode activeren 

In deze sectie, kunt u een Node.js console-app die een extern opnieuw opstarten op een apparaat met een directe methode Start en het apparaat dubbele query's gebruikt om te zoeken van de laatste keer dat opnieuw opstarten voor dat apparaat maken.

1. Maak een nieuwe lege map **triggerrebootondevice**.  Klik in de map **triggerrebootondevice** door een package.json-bestand met de volgende opdracht in bij de opdrachtprompt te maken.  Alle standaardwaarden accepteren:

    ```
    npm init
    ```
    
2. Bij de opdrachtprompt in de map **triggerrebootondevice** , voer de volgende opdracht om te installeren de **azure-iothub@dtpreview** apparaat SDK-pakket en **azure-iot-device-mqtt@dtpreview** pakket:

    ```
    npm install azure-iothub@dtpreview --save
    ```
    
3. Met een teksteditor, maakt u een nieuw **dmpatterns_getstarted_service.js** -bestand in de map **triggerrebootondevice** .

4. Toevoegen aan de volgende 'vereist' instructies aan het begin van het bestand **dmpatterns_getstarted_service.js** :

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. De volgende variabele declaraties toevoegen en de waarden van de tijdelijke aanduiding vervangen:

    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
    
6. Voeg de volgende functie om aan te roepen de methode apparaat om het apparaat opnieuw te starten:

    ```
    var startRebootDevice = function(twin) {

        var methodName = "reboot";
        
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
        
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```

7. Voeg de volgende functie om te zoeken op het apparaat en de laatste keer dat opnieuw opstarten:

    ```
    var queryTwinLastReboot = function() {

        registry.getTwin(deviceToReboot, function(err, twin){

            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
    
8. Voeg de volgende code om te bellen van de functies waarmee de directe methode opnieuw opstarten en de query voor de laatste keer dat opnieuw opstarten wordt geactiveerd:

    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
    
9. Opslaan en sluit het bestand **dmpatterns_getstarted_service.js** .

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren de toepassingen.

1. Bij de opdrachtprompt in de map **manageddevice** , voer de volgende opdracht om te beginnen met gebruikers zonder licentie voor de directe methode opnieuw opstarten.

    ```
    node dmpatterns_getstarted_device.js
    ```

2. Voer de volgende opdracht aan trigger de extern opnieuw opstarten en de query voor het zoeken van de laatste apparaat opnieuw tijd opstarten bij de opdrachtprompt in de map **triggerrebootondevice** .

    ```
    node dmpatterns_getstarted_service.js
    ```

3. Ziet u de reageren op de directe methode door het afdrukken van het bericht

## <a name="customize-and-extend-the-device-management-actions"></a>Aanpassen en het apparaat management acties uitbreiden

Uw oplossingen IoT kunnen uitvouwen de gedefinieerde set apparaat management patronen of aangepaste patronen door middel van het apparaat dubbele en C2D methode primitieven inschakelen. Andere voorbeelden van apparaat management acties zijn factory opnieuw instellen, firmware-update, software-update, power management, netwerk- en -connectiviteit management en codering van gegevens.

## <a name="device-maintenance-windows"></a>Apparaat onderhoud windows

Meestal configureert u apparaten bewerkingen uit te voeren op een moment dat wordt geminimaliseerd onderbrekingen en downtime.  Apparaat onderhoud windows zijn een veelgebruikte patroon definiëren van de tijd wanneer de configuratie door een apparaat moet worden bijgewerkt. Uw back-end-oplossingen kunnen de gewenste eigenschappen van de twee apparaat gebruiken om te definiëren en een beleid op uw apparaat waarmee een onderhoudsvenster activeren. Wanneer een apparaat het onderhoud venster beleid ontvangt, kunt deze de gerapporteerde eigenschap van het apparaat dubbele gebruiken om de status van het beleid te rapporteren. De app back-enddatabase kunt apparaat dubbele query's behaald na een naleving van apparaten en elk beleid.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt u een directe methode voor het starten van een extern opnieuw opstarten op een apparaat, gebruikt de dubbele apparaat gerapporteerd eigenschappen die u kunt het rapporteren van de laatste keer dat opnieuw opstarten van het apparaat en een query wordt uitgevoerd voor het apparaat te vinden van de laatste keer dat opnieuw opstarten van het apparaat vanuit de cloud.

Als u wilt doorgaan met aan de slag met IoT Hub en apparaat management patronen zoals remote via de air firmware-update, raadpleegt u:

[Zelfstudie: Hoe u een firmware-update][lnk-fwupdate]

Als u wilt weten hoe u uw oplossing en planning methode-op meerdere apparaten oproepen IoT uitbreiden, raadpleegt u de [planning en uitzenden taken] [ lnk-tutorial-jobs] zelfstudie.

Als u wilt doorgaan met aan de slag met IoT-Hub, raadpleegt u [aan de slag met de SDK van de Gateway IoT][lnk-gateway-SDK].


<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-fwupdate]: iot-hub-firmware-update.md
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx