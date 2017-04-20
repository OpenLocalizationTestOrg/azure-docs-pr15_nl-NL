<properties
    pageTitle="Een apparaat met de SDK van de Gateway IoT simuleren | Microsoft Azure"
    description="Azure IoT Gateway SDK scenario Windows gebruiken om te laten zien verzendende telemetrielogboek vanaf een gesimuleerd apparaat met de SDK van Azure IoT Gateway."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/29/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-simulated-device-using-windows"></a>Azure IoT Gateway SDK (beta) – apparaat naar cloud-berichten verzenden met een gesimuleerd apparaat met Windows

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Maken en uitvoeren van de steekproef

Voordat u begint, moet u het volgende doen:

- [Instellen van uw ontwikkelomgeving] [ lnk-setupdevbox] voor het werken met de SDK van Windows.
- [Maken van een hub IoT] [ lnk-create-hub] in uw Azure-abonnement, moet u de naam van uw hub om te voltooien van deze procedure. Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.
- Twee apparaten toevoegen aan uw hub IoT en noteer de id's en het apparaat sleutels. U kunt het [apparaat Explorer of iothub-explorer] [ lnk-explorer-tools] venster voor het toevoegen van uw apparaten aan de IoT hub die u in de vorige stap hebt gemaakt en hun toetsen op te halen.

De steekproef maken:

1. Open een **opdrachtprompt voor ontwikkelaars voor VS2015** opdrachtprompt.
2. Navigeer naar de hoofdmap in uw lokale kopie van de **azure-iot-gateway-sdk** -bibliotheek.
3. Voer de **Hulpmiddelen voor\\build.cmd** script. Dit script Hiermee maakt u een bestand van de oplossing Visual Studio, de oplossing maakt en wordt de tests wordt uitgevoerd. U vindt de Visual Studio-oplossing in de map **maken** in uw lokale kopie van de **azure-iot-gateway-sdk** -bibliotheek.

De steekproef uitvoeren:

Open het bestand in een teksteditor, **voorbeelden\\simulated_device_cloud_upload\\src\\simulated_device_cloud_upload_win.json** in uw lokale kopie van de **azure-iot-gateway-sdk** -bibliotheek. Dit bestand het configureren van de modules in de steekproef gateway:

- De module **IoTHub** maakt verbinding met uw hub IoT. U moet configureren om gegevens te verzenden naar uw hub IoT. Name beginwaarde voor de **IoTHubName** op de naam van uw hub IoT en stel de waarde **IoTHubSuffix** op **azure-devices.net**. Stel de waarde **Transport** op een van: "HTTP", "AMQP" of "MQTT". Houd er rekening mee dat op dit moment alleen "HTTP" wordt delen één TCP-verbinding voor alle berichten van het apparaat. Als u de waarde "AMQP" of "MQTT" instelt, blijft de gateway een afzonderlijke TCP-verbinding met IoT Hub voor elk apparaat.
- De module **toewijzing** worden de MAC-adressen van uw gesimuleerd apparaten aan uw IoT Hub apparaat-id toegewezen. Zorg ervoor dat **deviceId** waarden overeenkomen met de id's van de twee apparaten die u hebt toegevoegd aan uw hub IoT en dat de waarden **deviceKey** de toetsen van uw twee apparaten bevatten.
- De modules **BLE1** en **BLE2** zijn de gesimuleerd apparaten. Houd er rekening mee hoe het MAC-adres overeenkomen met die in de module **toewijzing** .
- De module **logboek** logboeken uw activiteit gateway naar een bestand.
- De waarden van de **module pad** hieronder wordt ervan uitgegaan dat u de opslagplaats IoT Gateway SDK in de hoofdmap van uw station **C:** gekopieerd. Als u deze naar een andere locatie hebt gedownload, moet u de waarden voor de **module pad** aanpassen.
- De matrix **koppelingen** onder aan het bestand JSON verbindt de modules **BLE1** en **BLE2** met de module **toewijzing** en de module **toewijzing** naar de module **IoTHub** . Ook zorgt u ervoor dat alle berichten worden geregistreerd door de module **logboek** .

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\iothub\\Debug\\iothub_hl.dll",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net",
                "Transport": "HTTP"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\identitymap\\Debug\\identity_map_hl.dll",
            "args" : 
            [
                {
                    "macAddress" : "01-01-01-01-01-01",
                    "deviceId"   : "{Device ID 1}",
                    "deviceKey"  : "{Device key 1}"
                },
                {
                    "macAddress" : "02-02-02-02-02-02",
                    "deviceId"   : "{Device ID 2}",
                    "deviceKey"  : "{Device key 2}"
                }
            ]
        },
        {
            "module name":"BLE1",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
            "args":
            {
                "filename":"C:\\azure-iot-gateway-sdk\\deviceCloudUploadGatewaylog.log"
            }
        }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IoTHub" }
    ]
}
```

Sla de wijzigingen die u naar het configuratiebestand hebt aangebracht.

De steekproef uitvoeren:

1. Navigeer naar de hoofdmap van uw lokale kopie van de **azure-iot-gateway-sdk** opslagplaats bij een opdrachtprompt.
2. Voer de volgende opdracht:
  
    ```
    build\samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```

3. U kunt het [apparaat Explorer of iothub-explorer] [ lnk-explorer-tools] venster voor het controleren van de berichten die IoT hub van de gateway ontvangt.


## <a name="next-steps"></a>Volgende stappen

Als u wilt krijgen een geavanceerdere begrip van de SDK van de Gateway IoT en experimenteren met codevoorbeelden, gaat u naar de volgende zelfstudies voor ontwikkelaars en bronnen:

- [Apparaat-naar-cloud-berichten verzenden vanaf een reële apparaat waarbij de SDK van de Gateway IoT][lnk-physical-device]
- [Azure IoT Gateway SDK][lnk-gateway-sdk]

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Handleiding voor ontwikkelaars][lnk-devguide]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 