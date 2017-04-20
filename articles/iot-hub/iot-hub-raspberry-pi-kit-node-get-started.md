<properties
 pageTitle="Aan de slag met Raspberry Pi 3 | Microsoft Azure"
 description="Aan de slag met frambozen Pi 3, uw Azure IoT Hub maken en uw Pi verbinden met de hub IoT"
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

# <a name="get-started-with-raspberry-pi-3"></a>Aan de slag met Raspberry Pi 3

In deze zelfstudie begint u door de basisbeginselen van het werken met frambozen Pi 3 die actieve Raspbian leermateriaal. Vervolgens leert u hoe u uw apparaten naadloos verbinding te maken met de cloud met [Azure IoT Hub](iot-hub-what-is-iot-hub.md). Voor Windows 10 IoT Core steekproeven, gaat u naar [windowsondevices.com](http://www.windowsondevices.com/).

## <a name="lesson-1-configure-your-device"></a>Les 1: Uw apparaat configureren

![Les 1 E2E-Diagram](media/iot-hub-raspberry-pi-lessons/e2e-lesson1.png)

In deze les u uw frambozen Pi 3-apparaat configureren met een besturingssysteem werkt, uw ontwikkelomgeving instellen en implementeren van een toepassing naar de Pi.

### <a name="configure-your-device"></a>Uw apparaat configureren

Configureer uw frambozen Pi 3 voor de eerste keer gebruiken en installeren van het besturingssysteem Raspbian, een gratis besturingssysteem wordt uitgevoerd die is geoptimaliseerd voor de hardware frambozen Pi.

*Geschatte tijd in beslag: 30 minuten* 

[Ga voor het configureren van uw apparaat](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)

### <a name="get-the-tools"></a>De hulpmiddelen voor krijgen
Download het hulpprogramma's en software voor bouwen en implementeren van uw eerste toepassing voor de frambozen Pi 3.

*Geschatte tijd in beslag: 20 minuten* 

[Ga naar 'Ophalen van de hulpmiddelen voor'](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

### <a name="create-and-deploy-the-blink-application"></a>Maak en implementeer de toepassing knipperen

De toepassing van de steekproef Node.js uit Github klonen en gulp als u wilt implementeren van deze toepassing naar uw bord frambozen Pi 3. In dit voorbeeld-toepassing knipperen de LED verbonden met het bord elke twee seconden.

*Geschatte tijd in beslag: 5 minuten* 

[Ga naar ' maken en implementeren van de toepassing knipperen '](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

## <a name="lesson-2-create-your-iot-hub"></a>Les 2: Uw hub IoT maken

![Les 2 E2E-Diagram](media/iot-hub-raspberry-pi-lessons/e2e-lesson2.png)

In deze les maken van uw gratis Azure-account, inrichten van de hub Azure IoT en maken van uw eerste apparaat in Azure IoT hub.

Les 1 voltooien voordat u deze les begint.

### <a name="get-the-azure-tools"></a>De Azure 's

Installeren van Azure opdrachtregel (Azure CLI).

*Geschatte tijd in beslag: 10 minuten* 

[Ga naar 'Krijgen Azure extra'](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)

### <a name="create-your-iot-hub-and-register-your-raspberry-pi-3"></a>Uw hub IoT maken en registreren van uw frambozen Pi-3

De resourcegroep maken en uw eerste apparaat toevoegen aan de Azure IoT Hub met Azure CLI inrichten van uw eerste Azure IoT hub. 

*Geschatte tijd in beslag: 10 minuten* 

[Ga naar "Uw hub IoT maken en registreren van uw frambozen Pi 3"](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)


## <a name="lesson-3-send-device-to-cloud-messages"></a>Les 3: Apparaat-naar-cloud-berichten verzenden

![Les 3 E2E-Diagram](media/iot-hub-raspberry-pi-lessons/e2e-lesson3.png)

In deze les verzenden u berichten uit uw Pi naar uw hub IoT. Ook een Azure functie app maakt die u binnenkomende berichten van uw hub IoT wordt opgehaald en deze gegevens worden geschreven naar Azure-tabelopslag.

Lessen 1 en 2 voltooien voordat u deze les begint.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Een functie Azure-app en opslag van Azure-account maken

Een sjabloon Azure Resource Manager gebruiken om een app Azure functie en een Azure Storage-account te maken.

*Geschatte tijd in beslag: 10 minuten* 

[Ga naar 'Maak een Azure functie-app en opslag van Azure-account'](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)

### <a name="run-sample-application-to-send-device-to-cloud-messages"></a>Uitvoeren van toepassing van de steekproef apparaat naar cloud berichten te verzenden

Implementeren en uitvoeren van een steekproef-toepassing op uw apparaat frambozen Pi 3 die berichten naar IoT hub.

*Geschatte tijd in beslag: 10 minuten* 

[Ga naar 'Uitvoeren van toepassing van de steekproef apparaat naar cloud berichten te verzenden'](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

### <a name="read-messages-persisted-in-azure-storage"></a>Gelezen berichten behouden in Azure Storage
Het apparaat naar cloud-berichten worden gecontroleerd terwijl ze naar uw Azure-opslag zijn geschreven.

*Geschatte tijd in beslag: 5 minuten* 

[Ga naar 'gelezen berichten behouden in Azure Storage'](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)


## <a name="lesson-4-send-cloud-to-device-messages"></a>Les 4: Cloud-to-device berichten verzenden

![Les 4 E2E-Diagram](media/iot-hub-raspberry-pi-lessons/e2e-lesson4.png)

In deze les demo's voor het verzenden van berichten vanaf uw Azure IoT hub naar uw frambozen Pi 3. De berichten bepalen aan en uit het gedrag van de LED die is gekoppeld aan uw Pi. Een toepassing voor de steekproef is voorbereid kunt bereiken van deze taak.

Voltooi lessen 1, 2 en 3 voordat u deze les begint.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Voer de toepassing van de steekproef cloud-to-device berichten ontvangen

De toepassing van de steekproef in les 4 wordt uitgevoerd op uw Pi en bewaakt de binnenkomende berichten van uw hub IoT. Een nieuwe taak in de gulp verzendt berichten naar uw Pi vanaf uw hub IoT het LED knipperen.

*Geschatte tijd in beslag: 10 minuten* 

[Ga naar 'Uitvoeren van de toepassing van de steekproef cloud-to-device berichten ontvangen'](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>Optionele sectie: wijzigen aan en uit het gedrag van de LED

De berichten om te wijzigen van het LED's in- en uitschakelen gedrag aanpassen.

*Geschatte tijd in beslag: 10 minuten* 

[Ga naar ' optionele sectie: wijzigen aan en uit het gedrag van de LED'](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)


## <a name="troubleshooting"></a>Problemen oplossen

Als u een dreigen aan tijdens de lessen, kunt u proberen oplossingen op deze pagina.

[Ga naar 'Probleemoplossing'](iot-hub-raspberry-pi-kit-node-troubleshooting.md)
