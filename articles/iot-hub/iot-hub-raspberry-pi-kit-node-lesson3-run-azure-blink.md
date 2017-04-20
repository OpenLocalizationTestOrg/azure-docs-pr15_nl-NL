<properties
 pageTitle="Uitvoeren van toepassing van de steekproef apparaat naar cloud berichten te verzenden | Microsoft Azure"
 description="Implementeren en uitvoeren van een steekproef-toepassing naar uw frambozen Pi 3 die wordt verzonden berichten naar IoT hub en de LED knipperen."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="32-run-sample-application-to-send-device-to-cloud-messages"></a>3,2 uitvoeren van toepassing van de steekproef apparaat naar cloud berichten te verzenden

## <a name="321-what-you-will-do"></a>3.2.1 wat u doet

Implementeren en uitvoeren van een steekproef-toepassing op uw frambozen Pi 3 die berichten naar uw hub IoT. Als u problemen ondervindt overeenkomen, oplossingen in het [oplossen van de pagina](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="322-what-you-will-learn"></a>3.2.2 wat u leert

- Het gebruik van het hulpmiddel gulp kunt implementeren en uitvoeren van de steekproef Node.js-toepassing op uw Pi.

## <a name="323-what-you-need"></a>3.2.3 wat u nodig hebt

- U moet hebt voltooid de vorige sectie in deze les: [een app Azure functie en een Azure Storage-account om te verwerken en IoT hub berichten opslaan maken](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).

## <a name="324-get-your-iot-hub-and-device-connection-strings"></a>3.2.4 krijgen uw IoT hub en apparaat verbindingstekenreeksen

De verbindingsreeks van het apparaat wordt gebruikt voor de Pi verbinden met uw hub IoT. De verbindingsreeks van de hub IoT wordt gebruikt om te verbinden met uw hub IoT de apparaat-id die uw Pi in de hub IoT vertegenwoordigt.

- De verbindingsreeks van de IoT-hub ophalen door de volgende opdracht uit Azure CLI uit te voeren:

```bash
az iot hub show-connection-string --name {my hub name} --resource-group iot-sample
```

`{my hub name}`is de naam die u hebt opgegeven in les 2. Gebruik `iot-sample` als de waarde van `{resource group name}` als u de waarde in les 2 niet wijzigen.

- Het apparaat-verbindingsreeks ophalen door de volgende opdracht uit te voeren:

```bash
az iot device show-connection-string --hub {my hub name} --device-id myraspberrypi --resource-group iot-sample
```

`{my hub name}`Hiermee gaat hetzelfde resultaat als het account waarmee met de voorgaande opdracht. Gebruik `iot-sample` als de waarde van `{resource group name}` en gebruikt u `myraspberrypi` als de waarde van `{device id}` als u de waarde in les 2 niet wijzigen.

## <a name="325-configure-the-device-connection"></a>3.2.5 de apparaatverbinding configureren

1. Het configuratiebestand geïnitialiseerd door de volgende opdrachten uit te voeren:

    ```bash
    npm install
    gulp init
    ```

2. Open het bestand dat apparaat configuratie `config-raspberrypi.json` in Visual Studio-Code door de volgende opdracht uit te voeren:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

    ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)

3. Controleer het volgende vervanging in de `config-raspberrypi.json` bestand:

  - **[Apparaat hostname of IP-adres]** vervangen door het IP-adres of hostname die u hebt ontvangen van `device-discovery-cli` of met de waarde die is overgenomen van wat u in les 1 hebt geconfigureerd.
  - Vervang **[apparaat-verbindingsreeks IoT]** met de `device connection string` u verkregen.
  - Vervang **[hub-verbindingsreeks IoT]** met de `iot hub connection string` u verkregen.

U kunt bijwerken de `config-raspberrypi.json` bestand zodat u de toepassing van de steekproef vanaf uw computer kunt implementeren.

## <a name="326-deploy-and-run-the-sample-application"></a>3.2.6 implementeren en uitvoeren van de steekproef-toepassing

Implementeren en uitvoeren van de steekproef-toepassing op uw Pi door de volgende opdracht uit te voeren:

```bash
gulp
```

> [AZURE.NOTE] De standaard gulp taak wordt uitgevoerd `install-tools`, `deploy`, en `run` opeenvolging taken. In [Les 1](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md), u hebt uitgevoerd deze taken achter elkaar afzonderlijk.

## <a name="327-verify-the-sample-application-works"></a>3.2.7 controleren of de steekproef toepassing werkt

Hier ziet u de LED die is gekoppeld aan uw Pi knipperende elke twee seconden. Telkens wanneer de LED knipperen, wordt de voorbeeldtoepassing stuurt een bericht naar uw hub IoT en controleert dat het bericht zijn verzonden naar uw hub IoT. Daarnaast voor elk van het bericht is ontvangen door de hub IoT, het bericht wordt weergegeven in het consolevenster. De voorbeeldtoepassing wordt beëindigd automatisch na het verzenden van 20 berichten.

![](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="328-summary"></a>3.2.8 overzicht van

U hebt geïmplementeerd en de nieuwe knipperen steekproef toepassing worden uitgevoerd op uw Pi apparaat naar cloud berichten naar uw hub IoT te verzenden. U kunt naar de volgende sectie verplaatsen naar uw berichten controleren als deze naar de opslag van Azure-account zijn geschreven.

## <a name="next-steps"></a>Volgende stappen

[3.3 gelezen berichten behouden in Azure Storage](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)