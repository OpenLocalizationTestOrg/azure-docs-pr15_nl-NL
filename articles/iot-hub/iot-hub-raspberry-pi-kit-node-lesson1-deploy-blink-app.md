<properties
 pageTitle="Maak en implementeer de toepassing knipperen | Microsoft Azure"
 description="De toepassing van de steekproef Node.js uit Github klonen en gulp als u wilt implementeren van deze toepassing naar uw bord frambozen Pi 3. In dit voorbeeld-toepassing knipperen de LED verbonden met het bord elke twee seconden."
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

# <a name="13-create-and-deploy-the-blink-application"></a>1.3 maken en implementeren van de toepassing knipperen

## <a name="131-what-you-will-do"></a>1.3.1 wat u doet

De toepassing van de steekproef Node.js uit Github klonen en het hulpmiddel gulp gebruiken om te implementeren van de steekproef-toepassing naar uw frambozen Pi 3. De toepassing van de steekproef knipperen de LED verbonden met het bord elke twee seconden. Als u problemen ondervindt overeenkomen, oplossingen in het [oplossen van de pagina](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="132-what-you-will-learn"></a>1.3.2 wat u leert

- Het gebruik van de `device-discover-cli` hulpmiddel om op te halen netwerken informatie over uw Pi.
- Hoe kunt implementeren en uitvoeren van de steekproef-toepassing op uw Pi.
- Implementatie en foutopsporing toepassingen op afstand op uw Pi.

## <a name="133-what-you-need"></a>1.3.3 wat u nodig hebt

U moet hebt voltooid in de secties volgen in les 1:

- [Uw apparaat configureren](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)
- [De hulpmiddelen voor krijgen](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

## <a name="134-obtain-the-ip-address-and-host-name-of-your-pi"></a>1.3.4 het IP-adres en host de naam van uw Pi verkrijgen

Open een opdrachtprompt in Windows of een terminal-venster in Mac OS of Ubuntu en voer de volgende opdracht uit:

```bash
devdisco list --eth
```

Hier ziet u uitvoer weergegeven die er ongeveer als volgt uit:

![opsporen](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

Let op de `IP address` en `hostname` van uw Pi. Moet u deze informatie verderop in dit gedeelte.

> [AZURE.NOTE] Zorg dat uw Pi is verbonden met hetzelfde netwerk als uw computer. Bijvoorbeeld: als uw computer is verbonden met een draadloos netwerk terwijl uw Pi is verbonden met een bekabeld netwerk, ziet u mogelijk niet het IP-adres in de uitvoer devdisco.

## <a name="135-clone-the-sample-application"></a>1.3.5 klonen de voorbeeldtoepassing

Als u wilt openen in de steekproef-code, als volgt te werk:

1. De steekproef opslagplaats uit Github klonen door de volgende opdracht uit te voeren:

    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started.git
    ```

2. Open de voorbeeldtoepassing in Visual Studio-Code door de volgende opdrachten uit te voeren:

    ```bash
    cd iot-hub-node-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Cessies‑retrocessies-structuur](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-mac.png)

De `app.js` bestand de `app` submap is de belangrijkste bronbestand dat de code aan een besturingselement voor de LED bevat.

### <a name="136-install-application-dependencies"></a>1.3.6 afhankelijkheden voor toepassingen installeren

Installeer de bibliotheken en andere modules die u nodig voor de steekproef-toepassing hebt door de volgende opdracht uit te voeren:

```bash
npm install
```

## <a name="137-configure-the-device-connection"></a>1.3.7 de apparaatverbinding configureren

Het configureren van de apparaatverbinding, als volgt te werk:

1. Het configuratiebestand apparaat genereren door de volgende opdracht uit te voeren:

    ```bash
    gulp init
    ```

    Het configuratiebestand `config-raspberrypi.json` bevat de referenties van de gebruiker die u kunt uw Pi aanmelden. Als u wilt voorkomen dat de lekkage van gebruikersreferenties, het configuratiebestand wordt gegenereerd in de submap `.iot-hub-getting-started` van de basismap op uw computer.

2. Open het apparaat configuratie-bestand in Visual Studio-Code door de volgende opdracht uit te voeren:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Vervang de tijdelijke aanduiding `[device hostname or IP address]` met het IP-adres of de hostnaam die u in de sectie 1.3.4.

    ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

Gefeliciteerd! U hebt de eerste steekproef-toepassing voor uw Pi gemaakt.

## <a name="138-deploy-and-run-the-sample-application"></a>1.3.8 implementeren en uitvoeren van de steekproef-toepassing

### <a name="1381-install-nodejs-and-npm-on-your-pi"></a>1.3.8.1 Node.js en NPM installeren op uw Pi

Node.js en NPM installeren op uw Pi door de volgende opdracht uit te voeren:

```bash
gulp install-tools
```

Het duurt tien minuten om te voltooien van de eerste keer dat u deze taak uitvoert.

### <a name="1382-deploy-and-run-the-sample-app"></a>1.3.8.2 implementeren en uitvoeren van de steekproef-app

Implementeren en uitvoeren van de steekproef-toepassing op door de volgende opdracht uit te voeren:

```bash
gulp deploy && gulp run
```

### <a name="1383-verify-the-app-works"></a>1.3.8.3 controleren of de app werkt

U ziet nu de LED op uw Pi knipperende elke twee seconden.  Als u niet het LED knipperende, raadpleegt u de [gids voor probleemoplossing](iot-hub-raspberry-pi-kit-node-troubleshooting.md) voor oplossingen voor veel voorkomende problemen.
![LED knipperende](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)

> [AZURE.NOTE] Gebruik `Ctrl + C` om de toepassing te beëindigen.

## <a name="139-summary"></a>1.3.9 overzicht van

U hebt de vereiste hulpmiddelen om te werken met uw Pi geïnstalleerd en een voorbeeld-toepassing naar uw Pi knipperen de LED geïmplementeerd. U kunt nu gaat u naar de volgende les maken, implementeren en uitvoeren van een ander voorbeeldtoepassing dat verbinding maakt met uw Pi Azure IoT Hub naar berichten verzenden en ontvangen.

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om te beginnen les 2 die begint met het [ophalen van de Azure hulpmiddelen](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
