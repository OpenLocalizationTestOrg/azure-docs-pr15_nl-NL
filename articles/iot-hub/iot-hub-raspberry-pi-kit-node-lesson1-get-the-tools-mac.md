<properties
 pageTitle="De's (Mac OS 10.10) | Microsoft Azure"
 description="Download en installeer de benodigde hulpprogramma's en software voor de eerste steekproef-toepassing voor uw Pi op Mac OS."
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

# <a name="12-get-the-tools-macos-1010"></a>1.2 de hulpmiddelen voor (Mac OS 10.10) kunt krijgen

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-ubuntu.md)
- [Mac OS 10.10](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-mac.md)

## <a name="121-what-you-will-do"></a>1.2.1 wat u doet

Download de ontwikkeling-hulpmiddelen en de software voor de eerste steekproef-toepassing voor uw frambozen Pi 3. Als u problemen ondervindt overeenkomen, oplossingen in het [oplossen van de pagina](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="122-what-you-will-learn"></a>1.2.2 wat u leert
In dit gedeelte wordt uitgelegd:

- Cijfer en Node.js installeren
    - [Cijfer](https://git-scm.com) is een bron openen distributed versiebeheer. De voorbeeldtoepassing voor deze les is opgeslagen op een cijfer.
    - [Node.js](https://nodejs.org/en/) is een JavaScript-runtime met een uitgebreide pakket-systeem.
- Het gebruik van NPM extra Node.js ontwikkeling's installeren.
  - De minimaal vereiste versie van Node.js is 4,5 LTS.
  - [NPM](https://www.npmjs.com) is een van de managers pakket voor Node.js.

## <a name="123-what-you-need"></a>1.2.3 wat u nodig hebt

- Een internetverbinding om de hulpmiddelen voor de ontwikkeling en de software te downloaden
- Een Mac met Mac OS Yosemite (10.10) of hoger

## <a name="124-install-git-and-nodejs"></a>1.2.4 cijfer en Node.js installeren

Wilt installeren cijfer en Node.js, gebruikt u het hulpprogramma [Homebrew](http://brew.sh) pakket management door deze stappen uit:

1. Installeer Homebrew. Als u Homebrew al hebt geïnstalleerd, gaat u naar stap 2.
  1. Druk op `Cmd + Space` en voer `Terminal` om een terminal venster te openen.
  2. Voer de volgende opdracht:

    ```bash
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    ```
2. Cijfer en Node.js installeren door de volgende opdracht uit te voeren:

    ```bash
    brew install node git
    ```

## <a name="125-install-additional-nodejs-development-tools"></a>1.2.5 extra Node.js ontwikkeling's installeren

U [gulp.js](http://gulpjs.com) gebruiken om automatisch de implementatie van de steekproef-toepassing naar uw Pi. U kunt ook de [apparaat-discovery-cli](https://github.com/Azure/device-discovery-cli) gebruiken om op te halen netwerkinformatie over uw IoT-apparaten.

Installeren `gulp` en `device-discovery-cli` door de volgende opdracht uit te voeren in het venster Terminal:

```bash
sudo npm install -g device-discovery-cli gulp
```

Als u problemen hebt met het Node.js en deze aanvullende ontwikkeling hulpprogramma's voor de installatie op Mac OS, raadpleegt u de [gids voor probleemoplossing](iot-hub-raspberry-pi-kit-node-troubleshooting.md) voor oplossingen voor veel voorkomende problemen.

## <a name="126-install-visual-studio-code"></a>1.2.6 installeren Visual Studio-Code

[Download](https://code.visualstudio.com/docs/setup/osx) en installeer Visual Studio-Code. Visual Studio-Code is een lightweight maar krachtige broncode-editor voor Windows, Linux en Mac OS. U kunt deze editor verderop in deze zelfstudie steekproef code wilt bewerken.

## <a name="127-summary"></a>1.2.7 overzicht van

U kunt de vereiste ontwikkeling hulpprogramma's en software voor de eerste steekproef-toepassing hebt geïnstalleerd. In de volgende sectie, u maakt, implementeren, en de toepassing van de steekproef worden uitgevoerd op uw Pi.

## <a name="next-steps"></a>Volgende stappen

[1.3 maken en implementeren van de toepassing van de steekproef knipperen](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)
