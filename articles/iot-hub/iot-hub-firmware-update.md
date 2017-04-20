<properties
 pageTitle="Hoe u een firmware-update | Microsoft Azure"
 description="Deze zelfstudie wordt getoond hoe u een firmware-update"
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

# <a name="tutorial-how-to-do-a-firmware-update-preview"></a>Zelfstudie: Hoe u een firmware bijwerken (preview)

## <a name="introduction"></a>Inleiding
In de [aan de slag met Apparaatbeheer] [ lnk-dm-getstarted] zelfstudie, u hebt gezien het gebruik van het [apparaat dubbele] [ lnk-devtwin] en [cloud-naar-apparaat (C2D) methoden] [ lnk-c2dmethod] primitieven extern opnieuw opstarten een apparaat. Deze zelfstudie gebruikt de dezelfde IoT Hub primitieven en bevat richtlijnen en ziet u hoe u een update end-to-end gesimuleerd firmware.  Dit patroon wordt gebruikt in de uitvoering van de update firmware voor de steekproef Intel Edison-apparaat.

Deze zelfstudie wordt getoond hoe naar:

- Een consoletoepassing die de directe methode firmwareUpdate-op het apparaat gesimuleerd via uw hub IoT oproepen maken.
- Maak een gesimuleerd apparaat, dat een firmwareUpdate directe methode die tot en met een proces met meerdere fasen die wacht leidt met het downloaden van de afbeelding firmware, de afbeelding firmware gedownload implementeert en ten slotte op alle th firmware afbeelding.  Voortgang van de overal in het uitvoeren van elke fase het apparaat gebruikt de dubbele gerapporteerd eigenschappen die u kunt bijwerken.

Aan het einde van deze zelfstudie hebt u twee Node.js console-toepassingen:

**dmpatterns_fwupdate_service.js**, die een directe methode-op het apparaat dat gesimuleerd oproepen, het antwoord wordt weergegeven en regelmatig (elke 500ms) wordt weergegeven de bijgewerkte apparaat dubbele gerapporteerd eigenschappen.

**dmpatterns_fwupdate_device.js**, dat is gekoppeld aan uw IoT hub voor de identiteit van het apparaat eerder hebt gemaakt, ontvangt u een firmwareUpdate directe methode, wordt uitgevoerd door een meerdere proces simuleren een firmware update inclusief: wachten op de afbeelding downloaden, downloaden van de nieuwe afbeelding en ten slotte het toepassen van de afbeelding.


Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

Node.js versie 0.12.x of later <br/>  [Voorbereiden van uw ontwikkelomgeving] [ lnk-dev-setup] wordt uitgelegd hoe u bij het installeren van Node.js voor deze zelfstudie in Windows of Linux.

Een actieve Azure-account. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.)

Volg het artikel [aan de slag met Apparaatbeheer](iot-hub-device-management-get-started.md) voor het maken van uw hub IoT en uw verbindingsreeks ophalen.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Een app gesimuleerd apparaat maken

In deze sectie, een Node.js console app maakt die u moet reageren op een directe methode aangeroepen door de cloud, die een update van de firmware gesimuleerd apparaat veroorzaakt en gebruik de dubbele apparaat gerapporteerd eigenschappen die u kunt het inschakelen van apparaat dubbele query's naar welke apparaten en wanneer ze laatst opnieuw gestart.

1. Maak een nieuwe lege map **manageddevice**.  Klik in de map **manageddevice** door een package.json-bestand met de volgende opdracht in bij de opdrachtprompt te maken.  Alle standaardwaarden accepteren:

    ```
    npm init
    ```
    
2. Bij de opdrachtprompt in de map **manageddevice** , voer de volgende opdracht om te installeren de **azure-iot-device@dtpreview** apparaat SDK-pakket en **azure-iot-device-mqtt@dtpreview** pakket:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Met een teksteditor, maakt u een nieuw **dmpatterns_fwupdate_device.js** -bestand in de map **manageddevice** .

4. Toevoegen aan de volgende 'vereist' instructies aan het begin van het bestand **dmpatterns_fwupdate_device.js** :

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Voeg een variabele **connectionString** en deze gebruiken om u te maken van een apparaat-client.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Voeg de volgende functie die wordt gebruikt voor het bijwerken van dubbele gerapporteerd eigenschappen

    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };

      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported')
      });
    };
    ```
    
7. Voeg de volgende functies waarmee u simuleren van het downloaden en toepassen van de afbeelding firmware.

    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
      
      console.log("Downloading image from " + imageUrl);
      
      callback(error, image);
    }

    var simulateApplyImage = function(imageData, callback) {
      var error = null;
      
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
      
      callback(error);
    }
    ```

8. Voeg de volgende functie die worden bijgewerkt via de bijwerkstatus firmware tot en met de dubbele gerapporteerd eigenschappen om te wachten om te downloaden.  Meestal apparaten zijn op de hoogte van een update beschikbaar en een beheerder gedefinieerd beleid oorzaken van het apparaat te downloaden en toepassen van de update te starten.  Dit is waar de logica om in te schakelen dat beleid wilt uitvoeren.  Volgt de procedure waarmee bent we vertragen voor vier seconden en product te downloaden van de afbeelding firmware. 

    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
      
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```
    
9. Voeg de volgende functie die worden bijgewerkt via de bijwerkstatus firmware tot en met de dubbele gerapporteerd eigenschappen om te downloaden van de afbeelding firmware.  Er volgt omhoog met het downloaden van een firmware simuleren en ten slotte de bijwerkstatus firmware op de hoogte brengt van een downloaden slagen of een fout wordt bijgewerkt.

    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
      
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
          
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
            
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
        
      }, 4000);
    }
    ```
    
10. Voeg de volgende functie die worden bijgewerkt via de bijwerkstatus firmware tot en met de dubbele eigenschappen gemeld bij het toepassen van de afbeelding firmware.  Er volgt omhoog met een toepassen van de afbeelding firmware simuleren en ten slotte de bijwerkstatus firmware op de hoogte brengt van een succes toepassen of een fout wordt bijgewerkt.

    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
      
      setTimeout(function() {
        
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
            
          }
        });
        
        setTimeout(callback, 4000);
        
      }, 4000);
    }
    ```

11. Voeg de volgende functoin waarin de methode firmwareUpdate en start het updateproces met meerdere fasen firmware.

    ```
    var onFirmwareUpdate = function(request, response) {
            
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });

      // Get the parameter from the body of the method request
      var fwPackageUri = JSON.parse(request.payload).fwPackageUri;

      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
          
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });

        }
      });
    }
    ```
    
12. Ten slotte de volgende code dat is gekoppeld aan IoT hub als een apparaat toevoegen 

    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
      
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate(request, response));
    });
    ```

> [AZURE.NOTE] Als u wilt houden dingen eenvoudige, wordt deze zelfstudie opnieuw beleid niet geïmplementeerd. In productiecode, moet u opnieuw beleid (zoals een exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke foutenstructuuranalyse afhandelen]implementeren[lnk-transient-faults].

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Een externe firmware-update op het apparaat met behulp van een directe methode activeren 

In dit gedeelte maakt u een Node.js console-app die start van een externe firmware-update op een apparaat met een directe methode en wordt apparaat dubbele query's gebruikt om regelmatig de status van de actieve firmware-update op dat apparaat.


1. Maak een nieuwe lege map **triggerfwupdateondevice**.  Klik in de map **triggerfwupdateondevice** door een package.json-bestand met de volgende opdracht in bij de opdrachtprompt te maken.  Alle standaardwaarden accepteren:

    ```
    npm init
    ```
    
2. Bij de opdrachtprompt in de map **triggerfwupdateondevice** , voer de volgende opdracht voor het installeren van de **azure-iothub** apparaat SDK het pakket en **azure-iot-apparaat-mqtt** :

    ```
    npm install azure-iot-hub@dtpreview --save
    ```
    
3. Met een teksteditor, maakt u een nieuw **dmpatterns_getstarted_service.js** -bestand in de map **triggerfwupdateondevice** .

4. Toevoegen aan de volgende 'vereist' instructies aan het begin van het bestand **dmpatterns_getstarted_service.js** :

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. De volgende variabele declaraties toevoegen en de waarden van de tijdelijke aanduiding vervangen:

    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
    
6. Voeg de volgende functie om te zoeken en de waarde van de firmwareUpdate weer te geven eigenschap gerapporteerd.

    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```

7. Voeg de volgende functie om aan te roepen de methode firmwareUpdate om het apparaat opnieuw te starten:

    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
      
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
      
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
      
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```

8. Tot slot gerapporteerd toevoegen de volgende functie om code voor het starten van de firmware sequentie bijwerken en start regelmatig met de dubbele eigenschappen:

    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
    
9. Opslaan en sluit het bestand **dmpatterns_fwupdate_service.js** .

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren de toepassingen.

1. Bij de opdrachtprompt in de map **manageddevice** , voer de volgende opdracht om te beginnen met gebruikers zonder licentie voor de directe methode opnieuw opstarten.

    ```
    node dmpatterns_fwupdate_device.js
    ```

2. Voer de volgende opdracht aan trigger de extern opnieuw opstarten en de query voor het zoeken van de laatste apparaat opnieuw tijd opstarten bij de opdrachtprompt in de map **triggerfwupdateondevice** .

    ```
    node dmpatterns_fwupdate_service.js
    ```

3. Ziet u de reageren op de directe methode door het afdrukken van het bericht

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt u een directe methode voor het starten van een externe firmware-update op een apparaat en de dubbele gerapporteerd regelmatig gebruikt eigenschappen voor meer informatie over de voortgang van de firmware bijwerken proces.  

Als u wilt weten hoe u uw oplossing en planning methode-op meerdere apparaten oproepen IoT uitbreiden, raadpleegt u de [planning en uitzenden taken] [ lnk-tutorial-jobs] zelfstudie.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
