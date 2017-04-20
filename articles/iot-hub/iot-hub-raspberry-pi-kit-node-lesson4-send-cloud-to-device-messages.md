<properties
 pageTitle="Voer de voorbeeldtoepassing voor het ontvangen van berichten van de cloud-naar-apparaat | Microsoft Azure"
 description="De toepassing van de steekproef in les 4 wordt uitgevoerd op uw Pi en bewaakt de binnenkomende berichten van uw hub IoT. Een nieuwe taak in de gulp verzendt berichten naar uw Pi vanaf uw hub IoT het LED knipperen."
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

# <a name="41-run-the-sample-application-to-receive-cloud-to-device-messages"></a>4.1 uitvoeren van de steekproef-toepassing voor het ontvangen van berichten van de cloud-naar-apparaat

In deze sectie, kunt u een voorbeeld-toepassing implementeren op uw frambozen Pi 3. De toepassing van de steekproef bewaakt binnenkomende berichten van uw hub IoT. U kunt ook een gulp taak uitvoeren op uw computer om berichten te verzenden naar uw Pi vanaf uw hub IoT. Nadat de berichten ontvangen, knipperen de voorbeeldtoepassing de LED. Als u problemen ondervindt overeenkomen, oplossingen in het [oplossen van de pagina](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="411-what-you-will-do"></a>4.1.1 wat u doet

- Verbinding met het maken van de steekproef-toepassing op uw hub IoT.
- Implementeren en uitvoeren van de steekproef-toepassing.
- Berichten verzenden vanuit uw IoT hub naar uw Pi het LED knipperen.

## <a name="412-what-you-will-learn"></a>4.1.2 wat u leert

- Hoe u binnenkomende berichten van uw hub IoT worden gecontroleerd.
- Hoe cloud-naar-apparaat om berichten te verzenden vanaf uw IoT hub naar uw Pi. 

## <a name="413-what-do-you-need"></a>4.1.3 wat hebt u nodig

- Een frambozen Pi 3 die is geconfigureerd voor gebruik. Zie informatie over het instellen van uw Pi, [Les 1: aan de slag met uw apparaat frambozen Pi 3](iot-hub-raspberry-pi-kit-node-get-started.md)
- Een hub IoT die is gemaakt in uw Azure-abonnement. Zie informatie over het maken van uw Azure IoT-Hub, [Les 2: uw Azure IoT Hub maken](iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="414-connect-the-sample-application-to-your-iot-hub"></a>4.1.4 verbinding maken met de voorbeeldtoepassing op uw hub IoT

1. Controleer of u bent in de map cessies‑retrocessies `iot-hub-node-raspberrypi-getting-started`. Open de voorbeeldtoepassing in Visual Studio-Code door de volgende opdrachten uit te voeren:

    ```bash
    cd Lesson4
    code .
    ```

    Kennisgeving de `app.js` bestand de `app` submap. De `app.js` bestand is de belangrijkste bronbestand met de code om de binnenkomende berichten van IoT Hub te houden. De `blinkLED` functie knipperen de LED.

    ![Cessies‑retrocessies-structuur](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure.png)

2. Het configuratiebestand met de volgende opdrachten geïnitialiseerd:

    ```bash
    npm install
    gulp init
    ```

    Als u klaar bent met les 3 op deze computer, worden alle configuraties zodat u verder met stap 4.1.5 overgenomen. Als u les 3 op een andere computer uitgevoerd, moet u voor het vervangen van de tijdelijke aanduidingen in het `config-raspberrypi.json` bestand. De `config-raspberrypi.json` staat in de submap van de basismap.

    ![Zoekconfiguratie](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

- **[Apparaat hostname of IP-adres]** vervangen door het IP-adres of de hostname die u met de opdracht van uw Pi`devdisco list --eth`
- **[Apparaat-verbindingsreeks IoT]** vervangen door de verbindingsreeks van apparaat die u met de opdracht `az iot hub show-connection-string --name {my hub name} --resource-group {resource group name}`.
- **[Hub-verbindingsreeks IoT]** vervangen door de IoT hub-verbindingsreeks die u met de opdracht `az iot device show-connection-string --hub {my hub name} --device-id {device id} --resource-group {resource group name}`.

## <a name="415-deploy-and-run-the-sample-application"></a>4.1.5 implementeren en uitvoeren van de steekproef-toepassing

Implementeren en uitvoeren van de steekproef-toepassing op uw Pi door de volgende opdrachten uit te voeren:
  
```
gulp
```

De opdracht gulp wordt de installatie-hulpmiddelen voor taak eerst uitgevoerd. Deze implementeert vervolgens de voorbeeldtoepassing op uw Pi. Ten slotte, wordt de toepassing op uw Pi en afzonderlijke taak op uw hostcomputer 20 knipperen berichten verzenden naar uw Pi vanaf uw hub IoT uitgevoerd.

Zodra de voorbeeldtoepassing wordt uitgevoerd, begint met gebruikers zonder licentie berichten van uw hub IoT. Ondertussen de taak gulp worden verscheidene "knipperen" berichten van uw IoT-Hub naar uw Pi. Voor elk van het bericht dat de knipperen, roept de voorbeeldtoepassing de functie blinkLED als u wilt de LED knipperen.

Hier ziet u de LED knipperende elke twee seconden ingedrukt terwijl u de taak gulp 20 berichten worden verzonden vanuit uw IoT hub naar uw Pi. Het laatste nummer is een 'niet meer'-bericht waarin staat de toepassing dat beëindigen.

![Gulp](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink.png)

## <a name="416-summary"></a>4.1.6 overzicht van

U hebt verzonden berichten van uw IoT hub naar uw Pi het LED knipperen. Volgende gedeelte is een optionele sectie om te hoe zien wijzigen aan en uit het gedrag van de LED.

## <a name="next-steps"></a>Volgende stappen

[Optionele sectie: wijzigen aan en uit het gedrag van de LED](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)