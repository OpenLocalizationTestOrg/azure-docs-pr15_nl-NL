<properties
 pageTitle="Uw apparaat configureren | Microsoft Azure"
 description="Configureer uw frambozen Pi 3 voor de eerste keer gebruiken en installeren van het besturingssysteem Raspbian, een gratis besturingssysteem wordt uitgevoerd die is geoptimaliseerd voor de hardware frambozen Pi."
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

# <a name="11-configure-your-device"></a>1.1 uw apparaat configureren

## <a name="111-what-you-will-do"></a>1.1.1 wat u doet

Configureer uw Pi voor de eerste keer gebruiken en installeer het besturingssysteem van Raspbian, een gratis besturingssysteem wordt uitgevoerd die is geoptimaliseerd voor de hardware frambozen Pi. Als u problemen ondervindt overeenkomen, oplossingen in het [oplossen van de pagina](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="112-what-you-will-learn"></a>1.1.2 wat u leert

In dit gedeelte wordt uitgelegd:

- Hoe u Raspbian installeert op uw Pi
- Hoe tot de macht van uw Pi met behulp van een USB-kabel
- Hoe u verbinding maakt van uw Pi bij het gebruik van een Ethernet-kabel of Wi-Fi-netwerk
- Hoe u een LED toevoegen aan de breadboard en maak verbinding met uw Pi

## <a name="113-what-you-need"></a>1.1.3 wat u nodig hebt

Als u wilt deze sectie hebt voltooid, moet u de volgende onderdelen van uw frambozen Pi 3 Starter Kit:

- Het bord frambozen Pi 3
- De 16GB MicroSD-kaart
- De macht van 5 v 2A opgeven met de zes mond micro USB-kabel
- De breadboard
- Connector draden
- Een weerstand 560 Ohm
- Een diffuse 10mm LED
- De Ethernet-kabel

![Dingen in uw starterskit](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

Moet u ook:

- Een bekabelde of draadloze verbinding voor uw Pi verbinding maken met
- Een USB-SD adapter of mini-SD kaartje branden van de afbeelding OS aan op de kaart MicroSD.
- Een computer met Windows, Mac of Linux. De computer wordt gebruikt voor het installeren van Raspbian op de kaart MicroSD.
- Een internetverbinding met de benodigde hulpprogramma's en software downloaden

## <a name="114-install-raspbian-on-the-microsd-card"></a>1.1.4 Raspbian installeren op de kaart MicroSD

Bereid het kaartje MicroSD naar de afbeelding Raspbian wilt schrijven.

1. Download Raspbian.
  1. [Download](https://www.raspberrypi.org/downloads/raspbian/) het zipbestand voor Raspbian Jessie met Pixel.
  2. Pak de Raspbian-afbeelding in een andere map op uw computer.
2. Installeer Raspbian naar het kaartje MicroSD.
  1. [Download](https://www.etcher.io) en installeer het hulpprogramma Etcher SD kaart-brander.
  2. Etcher uitvoeren en selecteer de Raspbian-afbeelding die u hebt uitgepakt in stap 1.
  3. Selecteer het station van de kaart MicroSD.
    Opmerking: Etcher hebt mogelijk al geselecteerd in het juiste station.
  4. Klik op Flash om te kunnen installeren Raspbian naar het kaartje MicroSD.
  5. Verwijder de kaart MicroSD vanaf uw computer als voltooid.
    Opmerking: Dit is het kaartje MicroSD rechtstreeks verwijderen omdat Etcher automatisch uitwerpen of de kaart MicroSD na voltooiing ontkoppelt veilige.
  6. De kaart MicroSD invoegt in uw Pi.

![De SD-kaart invoegen](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="115-power-on-your-pi"></a>1.1.5 power op uw Pi

Zet uw Pi met behulp van de micro USB-kabel en de voeding.

![Power op](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [AZURE.NOTE] Het is belangrijk dat de voeding gebruiken in de kit die ten minste 2A om ervoor te zorgen dat uw frambozen met voldoende goed wordt ingevoerd.

## <a name="116-connect-your-raspberry-pi-3-to-the-network"></a>1.1.6 uw frambozen Pi 3 verbinding met het netwerk

U kunt uw Pi verbinding maken met een bekabeld netwerk of een draadloos netwerk. Zorg dat uw Pi is verbonden met hetzelfde netwerk als uw computer. U kunt bijvoorbeeld uw Pi verbinding met de dezelfde schakeloptie waarmee uw computer is verbonden met.

### <a name="1161-connect-to-a-wired-network"></a>1.1.6.1 verbinding maken met een bekabeld netwerk

Gebruik de Ethernet-kabel aan uw Pi verbinden met uw bekabeld netwerk. De twee LED's op uw Pi inschakelen als de verbinding tot stand is gebracht.

![Verbinding maken met Ethernet-kabel](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="1162-connect-to-a-wireless-network"></a>1.1.6.2 verbinding maken met een draadloos netwerk

Volg de [instructies](https://www.raspberrypi.org/learning/software-guide/wifi/) uit het fundament frambozen Pi uw Pi verbinden met uw draadloos netwerk. Deze instructies moeten u eerst verbinding maken een monitor en een toetsenbord met uw Pi.

## <a name="117-connect-the-led-to-your-pi"></a>1.1.7 verbinden met de LED uw Pi

Als u wilt deze taak uitvoert, de [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), de verbindingslijn draden, het LED en de weerstand te gebruiken. U verbinding deze met de poorten [algemene invoer/uitvoer](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO) van uw Pi. 

![Breadboard, LED en weerstand](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. De kortere zijde van de LED verbinden met **GPIO GND (pincode 6)**.
2. De langere zijde van de LED verbinden met één zijde van de weerstand.
3. De andere zijde van de weerstand verbinden met **GPIO 4 (pincode 7)**.

Houd er rekening mee dat het LED-polariteit belangrijk is. Deze instelling polariteit algemeen bekend als actieve laag.

![Pin-out](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

Gefeliciteerd! U hebt uw Pi geconfigureerd.

## <a name="118-summary"></a>1.1.8 overzicht van

In deze sectie, hebt u geleerd hoe uw Pi configureren door Raspbian installeren, uw Pi verbinden met een netwerk en een LED verbinden met uw Pi. Houd er rekening mee dat het LED nog niet omhoog licht. In de volgende sectie installeert u de benodigde hulpprogramma's en software in voorbereiden voor het uitvoeren van een toepassing voor de steekproef op uw Pi.

![Hardware is gereed](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a>Volgende stappen

[1.2 de hulpmiddelen voor krijgen](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)
