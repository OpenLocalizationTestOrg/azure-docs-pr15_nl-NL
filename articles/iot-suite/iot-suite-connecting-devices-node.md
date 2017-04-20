<properties
   pageTitle="Verbinding maken met een apparaat met Node.js | Microsoft Azure"
   description="Wordt beschreven hoe u verbinding maakt u een apparaat met de Azure IoT Suite vooraf geconfigureerde externe controle oplossing met gebruikmaking van een toepassing in Node.js geschreven."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Uw apparaat verbinden met de externe controle vooraf geconfigureerde oplossing (Node.js)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-nodejs-sample-solution"></a>Maken en uitvoeren van de oplossing van de steekproef node.js

1. Als u wilt de *SDK's Microsoft Azure IoT* GitHub opslagplaats klonen en installeer het *Microsoft Azure IoT apparaat SDK voor Node.js* in uw bureaublad-omgeving, volgt u de [voorbereiden uw ontwikkelomgeving] [ lnk-github-prepare] instructies.

2. Vanuit uw lokale kopie van de [azure-iot-SDK's] [ lnk-github-repo] bibliotheek, de volgende twee bestanden kopiëren uit de map knooppunt/apparaat/voorbeelden naar een map op uw apparaat:

  - Packages.JSON
  - remote_monitoring.js

3. Open het bestand remote_monitoring.js en zoek naar de volgende variabele:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

4. **[Apparaat-verbindingsreeks IoT Hub]** vervangen door de verbindingsreeks van uw apparaat. U vindt de waarden voor uw IoT Hub hostname, apparaat-id en de apparaatsleutel in het externe controle oplossing dashboard. Een verbindingsreeks apparaat heeft de volgende indeling:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Als uw hostname IoT Hub **contoso is** en uw apparaat-id **mydevice**is, wordt de verbindingsreeks ziet er zo:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

5. Sla het bestand. Voer de volgende opdrachten opdrachtprompt in de map waarin deze bestanden om de nodige pakketten installeren en vervolgens de toepassing van de steekproef uitvoeren:

    ```
    npm install --save
    node remote_monitoring.js
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdks
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md