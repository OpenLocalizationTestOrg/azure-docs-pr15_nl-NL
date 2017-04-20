<properties
    pageTitle="Verzenden van berichten van de cloud-naar-apparaat met IoT Hub | Microsoft Azure"
    description="Volg deze zelfstudie meer informatie over het gebruik van Azure IoT Hub met Java cloud-to-device-berichten te verzenden."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-java"></a>Zelfstudie: Het verzenden van berichten van de cloud-naar-apparaat met IoT Hub en Java

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Inleiding

Azure IoT Hub heeft een volledig beheerde service waarmee inschakelen betrouwbare en veilige bidirectionele communicatie tussen miljoenen IoT apparaten en weer een toepassing beëindigen. De zelfstudie [aan de slag met IoT Hub] ziet hoe u een hub IoT maken, inrichten van een identiteit apparaat erin en code een gesimuleerd apparaat, dat wordt verzonden berichten apparaat-naar-cloud.

Deze zelfstudie dat is gebaseerd op het [aan de slag met IoT Hub]. U ziet hoe naar:

- Vanuit uw toepassing cloud back-end, door cloud-to-device berichten te verzenden naar een enkel apparaat via IoT Hub.
- Cloud-naar-apparaat ontvangen op een apparaat.
- Een back-end, bezorging bevestiging (*feedback*) aanvragen voor berichten die worden verzonden naar een apparaat IoT Hub vanuit de cloud toepassing.

U vindt meer informatie over cloud-to-device berichten in de [Handleiding voor ontwikkelaars van IoT Hub][IoT Hub Developer Guide - C2D].

Aan het einde van deze zelfstudie, moet u twee Java-console-toepassingen uitvoert:

* **gesimuleerd-apparaat**, een gewijzigde versie van de app die is gemaakt in [aan de slag met IoT-Hub], die is verbonden met uw hub IoT en cloud-naar-apparaat berichten ontvangt.
* **berichten van een verzenden c2d**, stuurt een bericht cloud-naar-apparaat naar het gesimuleerd apparaat via IoT-Hub, en klikt u vervolgens de bezorging-bevestiging ontvangt.

> [AZURE.NOTE] IoT Hub heeft SDK ondersteuning voor veel talen (inclusief C, Java en Javascript) tot en met Azure IoT apparaat SDK's en platforms voor apparaten. Zie voor stapsgewijze instructies voor het koppelen van uw apparaat van deze zelfstudie code en over het algemeen op Azure IoT-Hub, het [Azure IoT Developer Center].

Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

+ Java SE 8. <br/> [Voorbereiden van uw ontwikkelomgeving] [ lnk-dev-setup] wordt uitgelegd hoe u bij het installeren van Java voor deze zelfstudie in Windows of Linux.

+ Maven 3.  <br/> [Voorbereiden van uw ontwikkelomgeving] [ lnk-dev-setup] wordt uitgelegd hoe u bij het installeren van Maven voor deze zelfstudie in Windows of Linux.

+ Een actieve Azure-account. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.)

## <a name="receive-messages-on-the-simulated-device"></a>Ontvangen van berichten op het apparaat dat gesimuleerd

In dit gedeelte vindt u de apparaattoepassing gesimuleerd wijzigen u hebt gemaakt in [aan de slag met IoT Hub] naar cloud-to-device berichten ontvangt van de hub IoT.

1. Met een teksteditor, open het bestand simulated-device\src\main\java\com\mycompany\app\App.java.

2. Voeg de volgende **MessageCallback** klasse als een geneste klasse binnen de **App** -klasse. De methode **execute** wordt aangeroepen wanneer het apparaat dat een bericht van IoT Hub ontvangt. In dit voorbeeld krijgt het apparaat altijd de hub dat het bericht is voltooid:

    ```
    private static class MessageCallback implements
    com.microsoft.azure.iothub.MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));

        return IotHubMessageResult.COMPLETE;
      }
    }
    ```

3. De **belangrijkste** methode voor het maken van een exemplaar **MessageCallback** en de methode **setMessageCallback** aanroepen voordat deze wordt geopend de client als volgt wijzigen:

    ```
    client = new DeviceClient(connString, protocol);

    MessageCallback callback = new MessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [AZURE.NOTE] Als u HTTP in plaats van MQTT of AMQP als het transport gebruikt, het exemplaar **DeviceClient** Hiermee wordt gecontroleerd op berichten van IoT Hub zelden (minder dan elke 25 minuten). Zie voor meer informatie over de verschillen tussen MQTT, AMQP en HTTP-ondersteuning en het beperken van IoT-Hub, de [Handleiding voor ontwikkelaars van IoT Hub][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Een bericht cloud naar apparaat verzenden

In deze sectie, een Java-console app maakt die u cloud-to-device berichten naar de app gesimuleerd apparaat verzendt. Moet u het apparaat-Id van het apparaat dat u in de [aan de slag met IoT Hub] zelfstudie hebt toegevoegd. U moet ook de verbindingsreeks voor uw IoT hub die u in de [portal van Azure]kunt vinden.

1. Maak een Maven project **verzenden-c2d-berichten** met de volgende opdracht in bij de opdrachtprompt genoemd. Houd rekening met dat dit is een enkel, lang-opdracht:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigeer naar de nieuwe map voor berichten van een verzenden c2d bij de opdrachtprompt.

3. Met een teksteditor, open het bestand pom.xml in de map verzenden-c2d-berichten en de volgende afhankelijkheid toevoegen aan het knooppunt **afhankelijkheden** . De afhankelijkheid toevoegen, kunt u het pakket **iothub-java-service-client** in uw toepassing gebruiken om te communiceren met uw service van de hub IoT:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.7</version>
    </dependency>
    ```

4. Opslaan en sluit het bestand pom.xml.

5. Met een teksteditor, open het send-c2d-messages\src\main\java\com\mycompany\app\App.java-bestand.

6. De volgende instructies voor het **importeren** naar het bestand toevoegen:

    ```
    import com.microsoft.azure.iot.service.sdk.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. De volgende class niveau variabelen toevoegen aan de **App** class, en te vervangen **{yourhubconnectionstring}** **{yourdeviceid}** door de waarden de vermelde eerder:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQP;
    ```
    
8. De **belangrijkste** methode vervangen door de volgende code die is verbonden met uw IoT-hub, stuurt een bericht naar uw apparaat en vervolgens wacht tot een bevestiging dat het apparaat hebt ontvangen en het bericht verwerkt:

    ```
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
      
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver(deviceId);
        if (feedbackReceiver != null) feedbackReceiver.open();

        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);

        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");

        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }

        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [AZURE.NOTE] Voor de eenvoudig mogelijk te houden, wordt deze zelfstudie opnieuw beleid niet geïmplementeerd. In productiecode, moet u opnieuw beleid (zoals exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke foutenstructuuranalyse afhandelen]implementeren.

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren de toepassingen.

1. Opdrachtprompt in de map gesimuleerd-apparaat, voert u de volgende opdracht uit om te beginnen met het verzenden van telemetrielogboek op uw hub IoT en om te luisteren naar cloud-naar-apparaat verzonden berichten van de hub:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![De gesimuleerd apparaat-app uitvoeren][img-simulated-device]

2. Voer de volgende opdracht een cloud-to-device-bericht wilt verzenden en te wachten om een bevestiging feedback-opdrachtprompt in de map verzenden-c2d-berichten:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Voer de opdracht uit om de cloud-to-device-bericht te verzenden][img-send-command]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe cloud-to-device berichten verzenden en ontvangen. 

Voorbeelden van volledige end-to-end-oplossingen die IoT Hub gebruikt, raadpleegt u [Azure IoT Suite].

Meer informatie over het ontwikkelen van oplossingen met IoT-Hub, raadpleegt u de [Handleiding voor ontwikkelaars van IoT Hub].


<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[Aan de slag met IoT Hub]: iot-hub-java-java-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[Handleiding voor ontwikkelaars van IoT Hub]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[Tijdelijke foutenstructuuranalyse afhandeling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-portal]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/