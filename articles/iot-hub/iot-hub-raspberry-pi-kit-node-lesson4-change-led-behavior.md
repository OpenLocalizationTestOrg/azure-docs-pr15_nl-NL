<properties
 pageTitle="Optionele sectie - wijzigen aan en uit het gedrag van de LED | Microsoft Azure"
 description="De berichten om te wijzigen van het LED's in- en uitschakelen gedrag aanpassen."
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

# <a name="42-optional-section-change-the-on-and-off-behavior-of-the-led"></a>4.2 optionele sectie: wijzigen aan en uit het gedrag van de LED

## <a name="421-what-you-will-do"></a>4.2.1 wat u doet

De berichten om te wijzigen van het LED's in- en uitschakelen gedrag aanpassen. Als u problemen ondervindt overeenkomen, oplossingen in het [oplossen van de pagina](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="422-what-you-will-learn"></a>4.2.2 wat u leert

Gebruik aanvullende Node.js functies om te wijzigen van het LED's in- en uitschakelen gedrag.

## <a name="423-what-you-need"></a>4.2.3 wat u nodig hebt

U moet zijn voltooid [4.1 uitvoeren van een steekproef-toepassing op uw Pi frambozen cloud apparaat berichten ontvangen](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).

## <a name="424-add-nodejs-functions"></a>4.2.4 Node.js functies toevoegen

1. Open de voorbeeldtoepassing in Visual Studio-code door de volgende opdrachten uit te voeren:

    ```bash
    cd Lesson4
    code .
    ```

2. Open de `app.js` bestand en vervolgens de volgende functies toevoegen aan het einde:

    ```javascript
    function turnOnLED() {
      wpi.digitalWrite(CONFIG_PIN, 1);
    }

    function turnOffLED() {
      wpi.digitalWrite(CONFIG_PIN, 0);
    }
    ```

    ![app.js bijwerken](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)

3. Toevoegen van de volgende voorwaarden voordat de standaard een in de blokkering van de schakeloptie-hoofdletters/kleine letters van de `receiveMessageCallback` functie:

    ```javascript
    case 'on':
      turnOnLED();
      break;
    case 'off':
      turnOffLED();
      break;
    ```

    Nu hebt u de voorbeeldtoepassing om te reageren op meer instructies tot en met berichten geconfigureerd. De instructie 'in' Hiermee schakelt u het LED en schakelt u de instructie 'uit' uit het LED.

4. Open het bestand gulpfile.js en voegt u een nieuwe functie voordat u de functie `sendMessage`:

    ```javascript
    var buildCustomMessage = function (messageId) {
      if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
        return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
      } else if (messageId < MAX_MESSAGE_COUNT) {
        return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
      } else {
        return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
      }
    }
    ```

    ![gulpfile bijwerken](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile.png)

5. In de `sendMessage` , functie, vervangen van de regel `var message = buildMessage(sentMessageCount);` met de nieuwe regel weergegeven in het volgende fragment:

    ```javascript
    var message = buildCustomMessage(sentMessageCount);
    ```

6. Alle wijzigingen opslaan.

### <a name="425-deploy-and-run-the-sample-application"></a>4.2.5 implementeren en uitvoeren van de steekproef-toepassing

Implementeren en uitvoeren van de steekproef-toepassing op uw Pi door de volgende opdracht uit te voeren:

```bash
gulp
```

Hier ziet u de LED ingeschakeld voor twee seconden ingedrukt en klikt u vervolgens uitgeschakeld voor een andere twee seconden ingedrukt. Het laatste bericht met 'niet meer' stopt de voorbeeldtoepassing niet uitgevoerd.

![in- of uitschakelen](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

Gefeliciteerd! U hebt de berichten die zijn verzonden naar de Pi vanaf uw hub IoT is aangepast.

### <a name="427-summary"></a>4.2.7 overzicht van

Deze optionele sectie demo's voor het aanpassen van de berichten, zodat de voorbeeldtoepassing op een andere manier aan en uit het gedrag van de LED kan bepalen.

