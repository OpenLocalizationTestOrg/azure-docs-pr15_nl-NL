<properties
 pageTitle="Problemen oplossen | Microsoft Azure"
 description="De pagina voor probleemoplossing voor frambozen Pi Node.js ervaring"
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

# <a name="troubleshooting"></a>Problemen oplossen

## <a name="hardware-issues"></a>Hardwareproblemen

### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a>De toepassing ook wordt uitgevoerd, maar de LED is niet knipperende

Dit probleem is altijd gekoppeld aan de hardware circuitlijnen connectiviteit. Gebruik de volgende stappen om problemen te identificeren.

1. Controleer als u de juiste **GPIO** op uw bord kiezen. De twee poorten in deze les moet **GPIO GND (pincode 6)** en **GPIO 04 (pincode 7)**.
2. Controleren of de polariteit van uw LED juist is. De langer arm aangeven de **positieve**, gebruikte pincode.
3. Gebruik de **3,3 v vastmaken** en **GND pincode** op uw Raspberry Pi 3. Uw Pi als de domeincontroller macht behandelen. Controleer of het LED goed werkt.

![LED-specificatie](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a>Andere hardwareproblemen

Voor informatie over het oplossen van veelvoorkomende problemen op de frambozen Pi 3, raadpleegt u de [pagina voor officiële voor probleemoplossing](http://elinux.org/R-Pi_Troubleshooting).

## <a name="nodejs-package-issues"></a>Node.js pakket problemen

### <a name="no-response-during-gulp-tasks"></a>Geen antwoord tijdens gulp taken

Als u gulp taken uitvoeren problemen, kunt u toevoegen de `--verbose` optie voor foutopsporing. Probeert te beëindigen huidige gulp taken met `Ctrl + C` en voer de volgende in het consolevenster opdracht om te zien fouten opsporen in berichten. Hier ziet u gedetailleerde foutberichten worden weergegeven in uw console-uitvoer afgedrukt. 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a>Apparaat discovery problemen

Voor hulp bij het oplossen van veelvoorkomende problemen met de `devdisco` opdracht, schakelt u het [Leesmij-bestand](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).

### <a name="other-npm-issues"></a>Andere problemen NPM

Probeer of u kunt uw NPM-pakket bijwerken met de volgende opdracht uit:

```bash
npm install -g npm
```

Als het probleem blijft optreden, laat uw opmerkingen aan het einde van dit artikel of een probleem met de Github in ons [Voorbeeld opslagplaats](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started) maken

## <a name="remote-debugging"></a>Foutopsporing op afstand

### <a name="run-the-sample-application-in-debug-mode"></a>Het uitvoeren van de steekproef-toepassing in de foutopsporingsmodus voor

```bash
gulp run --debug
```

Wanneer de engine foutopsporing klaar is, ziet u ```Debugger listening on port 5858``` in de console-uitvoer.

### <a name="configure-vs-code-to-connect-to-the-remote-device"></a>TEGENOVER Code verbinding maken met het externe apparaat configureren

Open het paneel **fouten opsporen in** aan de linkerkant.

Klik op de groene knop voor **Foutopsporing starten** (F5). TEGENOVER Code wordt geopend een **launch.json** -bestand dat u wilt bijwerken.

Het bestand **launch.json** bijgewerkt met de volgende inhoud. Vervang `[device hostname or IP address]` met de werkelijke apparaat IP-adres of hostnaam.   

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}
```

![Externe foutopsporing configuratie](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a>Als bijlage toevoegen aan de externe toepassing

Klik op de groene knop **Starten foutopsporing** (F5) foutopsporing te starten. 

Lees [JavaScript in VS-Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) voor meer informatie over foutopsporing.

![Remote foutopsporing interactieve](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a>Azure CLI problemen

Azure CLI is een preview build. U kunt ook verwijzen naar [Preview installeren handleiding](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) voor het zoeken van oplossingen.

Als u fouten met het hulpmiddel, het bestand een [probleem](https://github.com/Azure/azure-cli/issues) in de sectie **problemen** van de cessies‑retrocessies GitHub.

Voor hulp bij het oplossen van veelvoorkomende problemen, raadpleegt u het [Leesmij-bestand](https://github.com/Azure/azure-cli/blob/master/README.rst).

## <a name="python-installation-issues"></a>Python installatieproblemen

### <a name="legacy-installation-issues-macos"></a>Oudere installatieproblemen (Mac OS)

Tijdens de installatie van **pip**, wordt een machtigingsfout wordt gegenereerd wanneer er oudere pakketten die worden geïnstalleerd met **so** machtigingen. Dit komt omdat de eerdere installatie van Python met brew (Mac OS) is niet volledig verwijderd. Sommige **pip** -pakketten van een eerdere installatie zijn gemaakt door hoofdsite, waardoor de machtigingsfout. De oplossing is om te verwijderen die pakketten geïnstalleerd door de hoofdmap. Gebruik de volgende stappen uit om deze taak te voltooien:

1. Ga naar /usr/local/lib/python2.7/site-packages
2. Lijst-pakketten maken door de hoofdmap:`ls -l | grep root`
3. Pakketten verwijderen uit stap 2:`sudo rm -rf {package name}`
4. Python opnieuw te installeren.

## <a name="azure-iot-hub-issues"></a>Azure IoT hub problemen

Als u hebt is ingericht Azure IoT hub met `azure-cli`, en moet u een venster voor het beheren van de apparaten die verbinding maken met uw IoT-hub, probeer de volgende hulpprogramma's:

### <a name="device-explorer"></a>Apparaat Explorer

[Apparaat Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) op uw lokale computer met Windows wordt uitgevoerd en maakt verbinding met uw hub IoT in Azure wordt aangegeven. Communiceren met de volgende [IoT Hub eindpunten](iot-hub-devguide.md):

- *Identiteit Apparaatbeheer* inrichten en beheren van apparaten die bij uw hub IoT zijn geregistreerd.
- *Ontvangen apparaat-naar-cloud* zodat u kunt berichten van uw apparaat naar uw hub IoT verzonden worden gecontroleerd.
- *Cloud-naar-apparaat verzenden* zodat u kunt berichten verzenden naar uw apparaten van uw hub IoT.

Configureer uw `IoT hub connection string` binnen dit hulpmiddel gebruik van de mogelijkheden ervan.

### <a name="iot-hub-explorer"></a>IoT hub Explorer

[IoT hub Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/iothub-explorer/readme.md) is een voorbeeld van meerdere platforms CLI hulpprogramma voor het beheren van apparaat-clients. Het hulpmiddel kunt u de apparaten beheren in het register identiteit, apparaat-naar-cloud berichten worden gecontroleerd en cloud-to-device opdrachten verzenden.

De laatste versie wilt installeren (voorlopige versie) van het hulpprogramma iothub explorer, voert u de volgende opdracht in uw omgeving opdrachtregel:

```
npm install -g iothub-explorer@latest
```

U kunt de volgende opdracht uit om aanvullende help-informatie over alle iothub-explorer-opdrachten en parameters gebruiken:

```bash
iothub-explorer help
```

### <a name="use-azure-portal-to-manage-your-resources"></a>Azure-portal gebruiken voor het beheren van uw resources

In deze lessen krijgt u een volledige CLI-ervaring maken en beheren van uw Azure resources. U kunt ook met de [portal van Azure](../azure-portal-overview.md) help inrichten, beheren en fouten opsporen in uw Azure resources.

## <a name="azure-storage-issues"></a>Problemen met Azure opslag

[Microsoft Azure opslag Explorer (Preview)](http://storageexplorer.com) is een app zelfstandige versie van Microsoft waarmee u kunt werken met gegevens van Azure-opslag in Windows, Mac OS en Linux. Dit hulpmiddel kunt u verbinding maken met uw tabel en ziet u de gegevens erin. Dit hulpmiddel kunt u problemen met uw Azure-opslag.
