<properties
    pageTitle="Azure IoT Hub voor Java aan de slag | Microsoft Azure"
    description="Azure IoT Hub met Java aan de slag zelfstudie. Azure IoT Hub en Java met de Microsoft Azure IoT SDK's gebruiken om een oplossing Internet van zaken te implementeren."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/11/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-java"></a>Aan de slag met Azure IoT Hub voor Java

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Aan het einde van deze zelfstudie hebt u drie Java-console-toepassingen:

* **Maak-apparaat-identiteit**, die Hiermee maakt u een apparaat-identiteit en de bijbehorende beveiliging-toets om te koppelen van een gesimuleerd apparaat.
* **alleen-d2c-berichten**, waarin de telemetrielogboek door uw gesimuleerd apparaat verzonden.
* **gesimuleerd-apparaat**, dat is verbonden met uw IoT hub voor de identiteit van het apparaat eerder hebt gemaakt, en stuurt een bericht telemetrielogboek elke tweede gebruiken het AMQP-protocol.

> [AZURE.NOTE] Het artikel [IoT Hub SDK's] [ lnk-hub-sdks] vindt u informatie over de verschillende SDK's die u gebruiken kunt om beide toepassingen om uit te voeren naar apparaten en uw oplossing back-end te maken.

Als u wilt deze zelfstudie hebt voltooid, moet u het volgende:

+ Java SE 8. <br/> [Voorbereiden van uw ontwikkelomgeving] [ lnk-dev-setup] wordt uitgelegd hoe u bij het installeren van Java voor deze zelfstudie in Windows of Linux.

+ Maven 3.  <br/> [Voorbereiden van uw ontwikkelomgeving] [ lnk-dev-setup] wordt uitgelegd hoe u bij het installeren van Maven voor deze zelfstudie in Windows of Linux.

+ Een actieve Azure-account. (Als u geen account hebt, kunt u een [gratis account] [ lnk-free-trial] in een paar minuten.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Een laatste stap, noteert u de waarde van de **primaire sleutel** en klik op **Messaging**. Klik op het blad **Messaging** Noteer de **naam van de gebeurtenis Hub-compatibele** en het **eindpunt van de gebeurtenis Hub-compatibele**. U moet deze drie waarden bij het maken van uw toepassing **gelezen-d2c-berichten** .

![Azure portal IoT Hub Messaging blade][6]

U hebt nu uw hub IoT gemaakt en u de IoT Hub hostname, IoT Hub-verbindingsreeks IoT Hub primaire sleutel, de naam van de gebeurtenis Hub-compatibele en gebeurtenis Hub-compatibele eindpunt moet u deze zelfstudie hebt.

## <a name="create-a-device-identity"></a>Een identiteit apparaat maken

In deze sectie, een Java-console app maakt die u een nieuwe apparaat-id in het register identiteit in de hub IoT maakt. Een apparaat verbinding geen met IoT-hub, tenzij er een vermelding in het register van de identiteit apparaat. Raadpleeg de sectie **Apparaat identiteit register** van de [Handleiding voor ontwikkelaars van IoT Hub] [ lnk-devguide-identity] voor meer informatie. Wanneer u deze consoletoepassing uitvoert, wordt een unieke apparaat-ID en de sleutel die uw apparaat gebruiken kunt om zichzelf te identificeren bij het apparaat naar cloud-berichten op IoT Hub verzenden gegenereerd.

1. Maak een nieuwe lege map iot-java-get-gestart. Klik in de map iot-java-get-gestart door een nieuw Maven-project genoemd **maken-apparaat-identiteit** , met de volgende opdracht in bij de opdrachtprompt te maken. Houd rekening met dat dit is een enkel, lang-opdracht:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigeer naar de nieuwe maken-apparaat-ID-map bij de opdrachtprompt.

3. Met een teksteditor, open het bestand pom.xml in de map maken-apparaat-identiteit en de volgende afhankelijkheid toevoegen aan het knooppunt **afhankelijkheden** . Hiermee kunt u het pakket iothub-service-sdk gebruiken in uw toepassing:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.9</version>
    </dependency>
    ```
    
4. Opslaan en sluit het bestand pom.xml.

5. Met een teksteditor, open het bestand create-device-identity\src\main\java\com\mycompany\app\App.java.

6. De volgende instructies voor het **importeren** naar het bestand toevoegen:

    ```
    import com.microsoft.azure.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.iot.service.sdk.Device;
    import com.microsoft.azure.iot.service.sdk.RegistryManager;

    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. De volgende class niveau variabelen toevoegen aan de **App** class, kunt vervangen **{yourhubconnectionstring}** de waarde de vermelde eerder:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    
    ```
    
8. De handtekening van de **belangrijkste** methode om op te nemen de uitzonderingen als volgt wijzigen:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```
    
9. Voeg de volgende code toe als de hoofdtekst van de **belangrijkste** methode. Deze code maakt u een zogeheten *javadevice* in het register van de identiteit IoT Hub als nog niet bestaat. Vervolgens wordt weergegeven het apparaat-id en de sleutel die u later nodig hebt:

    ```
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }
    System.out.println("Device id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Opslaan en sluit het bestand App.java.

11. Maak de **maken-apparaat-id** -toepassing met Maven door uitvoeren bij de opdrachtprompt de volgende opdracht uit in de map maken-apparaat-id:

    ```
    mvn clean package -DskipTests
    ```

12. Als u wilt de **maken-apparaat-id** -toepassing met Maven uitvoert, Voer u de volgende opdracht bij de opdrachtprompt in de map maken-apparaat-id:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Noteer de **apparaat-id** en het **apparaat-toets**. Moet u deze later wanneer u een toepassing die is verbonden met IoT Hub als een apparaat maakt.

> [AZURE.NOTE] Er worden alleen het IoT Hub identiteit register opgeslagen apparaat identiteiten voor beveiligde toegang tot de hub. Apparaat-id's en sleutels wilt gebruiken als beveiligingsreferenties en een vlag ingeschakeld/uitgeschakeld, die u gebruiken kunt voor het uitschakelen van toegang voor een individuele apparaat wordt opgeslagen. Als de toepassing voor de opslag van andere metagegevens apparaat-specifieke, moet deze een winkel toepassingsspecifieke gebruiken. Verwijzen naar de [Handleiding voor ontwikkelaars van IoT Hub] [ lnk-devguide-identity] voor meer informatie.

## <a name="receive-device-to-cloud-messages"></a>Apparaat-naar-cloud berichten ontvangt

In deze sectie, een Java-console app maakt die u apparaat-naar-cloud berichten van IoT Hub leest. Een hub IoT beschrijft een [Gebeurtenis Hub][lnk-event-hubs-overview]-compatibele eindpunt zodat u kunt het apparaat naar cloud berichten lezen. Als u wilt houden dingen eenvoudige, deze zelfstudie Hiermee maakt u een eenvoudige lezer die niet geschikt is voor een hoge doorvoer-implementatie. Het [proces apparaat naar cloud berichten] [ lnk-process-d2c-tutorial] zelfstudie leert u hoe u het apparaat naar cloud-berichten bij het op schaal verwerkt. De [Aan de slag met gebeurtenis Hubs] [ lnk-eventhubs-tutorial] zelfstudie biedt meer informatie over het proces berichten van gebeurtenis Hubs en is van toepassing op de eindpunten IoT Hub gebeurtenis Hub-compatibele.

> [AZURE.NOTE] Het eindpunt van de gebeurtenis Hub-compatibele voor het apparaat naar cloud berichten altijd leest gebruikt het AMQP-protocol.

1. In de map iot java-get-started die u in de sectie *maken een identiteit apparaat* hebt gemaakt, kunt u een nieuw Maven-project dat is **gelezen-d2c-berichten** met de volgende opdracht in bij de opdrachtprompt genoemd maken. Houd rekening met dat dit is een enkel, lang-opdracht:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigeer naar de nieuwe map voor de alleen-d2c-berichten bij de opdrachtprompt.

3. Met een teksteditor, open het bestand pom.xml in de map alleen-d2c-berichten en de volgende afhankelijkheid toevoegen aan het knooppunt **afhankelijkheden** . Hiermee kunt u het pakket eventhubs-client in uw toepassing gebruiken om te lezen van het eindpunt van de gebeurtenis Hub-compatibele:

    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.8.2</version> 
    </dependency>
    ```

4. Opslaan en sluit het bestand pom.xml.

5. Met een teksteditor, open het bestand read-d2c-messages\src\main\java\com\mycompany\app\App.java.

6. De volgende instructies voor het **importeren** naar het bestand toevoegen:

    ```
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;
    
    import java.io.IOException;
    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.Collection;
    import java.util.concurrent.ExecutionException;
    import java.util.function.*;
    import java.util.logging.*;
    ```

7. De volgende class niveau variabelen toevoegen aan de **App** class. **{Youriothubkey}**, **{youreventhubcompatibleendpoint}**en **{youreventhubcompatiblename}** vervangen door de waarden die u eerder hebt genoteerd:

    ```
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. De volgende **receiveMessages** methode toevoegen aan de **App** class. Deze methode maakt een exemplaar **EventHubClient** verbinding maken met het eindpunt van de gebeurtenis Hub-compatibele en maakt u een exemplaar **PartitionReceiver** lezen van een gebeurtenis Hub partition asynchroon. Er continu lussen en de berichtdetails van het wordt afgedrukt, totdat de toepassing wordt beëindigd.

    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      }
      catch(Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        client.createReceiver( 
          EventHubClient.DEFAULT_CONSUMER_GROUP_NAME,  
          partitionId,  
          Instant.now()).thenAccept(new Consumer<PartitionReceiver>()
        {
          public void accept(PartitionReceiver receiver)
          {
            System.out.println("** Created receiver on partition " + partitionId);
            try {
              while (true) {
                Iterable<EventData> receivedEvents = receiver.receive(100).get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getProperties().get("iothub-connection-device-id")));
                    System.out.println(String.format("| Message Payload: %s", new String(receivedEvent.getBody(),
                      Charset.defaultCharset())));
                    batchSize++;
                  }
                }
                System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId,batchSize));
              }
            }
            catch (Exception e)
            {
              System.out.println("Failed to receive messages: " + e.getMessage());
            }
          }
        });
      }
      catch (Exception e)
      {
        System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

    > [AZURE.NOTE] Deze methode wordt een filter gebruikt bij het maken van de ontvanger zodat de ontvanger alleen wordt gelezen berichten die zijn verzonden naar IoT Hub nadat de ontvanger is gestart. Dit is handig in een testomgeving zodat u de huidige set van berichten kunt zien. In een productieomgeving uw code moet ervoor zorgen dat deze verwerkt alle berichten - [het verwerken van IoT Hub apparaat naar cloud berichten] Zie[ lnk-process-d2c-tutorial] zelfstudie voor meer informatie.

9. De handtekening van de **belangrijkste** methode om op te nemen de uitzondering als volgt wijzigen:

    ```
    public static void main( String[] args ) throws IOException
    ```

10. De volgende code hebt toegevoegd aan de **belangrijkste** methode in de klas **App** . Deze code wordt gemaakt van de twee instanties van **EventHubClient** en **PartitionReceiver** en kunt u de toepassing sluiten wanneer u klaar bent met het verwerken van berichten:

    ```
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try
    {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    }
    catch (ServiceBusException sbe)
    {
      System.exit(1);
    }
    ```

    > [AZURE.NOTE] Deze code wordt ervan uitgegaan dat u hebt uw hub IoT gemaakt in de F1 (gratis) laag. Een gratis IoT hub heeft twee partities met de naam '0' en '1'.

11. Opslaan en sluit het bestand App.java.

12. Maak de **alleen-d2c-berichten** -toepassing met Maven door de volgende opdracht uit bij de opdrachtprompt worden uitgevoerd in de map alleen-d2c-berichten:

    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-simulated-device-app"></a>Een app gesimuleerd apparaat maken

In deze sectie, een Java-console app maakt die u een apparaat dat apparaat naar cloud berichten naar een hub IoT verzendt simuleert.

1. In de map iot java-get-started die u hebt gemaakt in de sectie *maken een apparaat-identiteit* , maken een nieuw Maven project **gesimuleerd-apparaat** met de volgende opdracht in bij de opdrachtprompt genoemd. Houd rekening met dat dit is een enkel, lang-opdracht:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigeer naar de nieuwe map voor gesimuleerd apparaat bij de opdrachtprompt.

3. Met een teksteditor, open het bestand pom.xml in de map gesimuleerd apparaat en de volgende afhankelijkheden toevoegen aan het knooppunt **afhankelijkheden** . Hiermee kunt u het pakket iothub-java-client gebruiken in uw toepassing om te communiceren met uw hub IoT en Java-objecten naar JSON serialiseren:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.14</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

4. Opslaan en sluit het bestand pom.xml.

5. Met een teksteditor, open het bestand simulated-device\src\main\java\com\mycompany\app\App.java.

6. De volgende instructies voor het **importeren** naar het bestand toevoegen:

    ```
    import com.microsoft.azure.iothub.DeviceClient;
    import com.microsoft.azure.iothub.IotHubClientProtocol;
    import com.microsoft.azure.iothub.Message;
    import com.microsoft.azure.iothub.IotHubStatusCode;
    import com.microsoft.azure.iothub.IotHubEventCallback;
    import com.microsoft.azure.iothub.IotHubMessageResult;
    import com.google.gson.Gson;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. De volgende class niveau variabelen toevoegen aan de **App** class, **{youriothubname}** vervangen door uw IoT hub en **{yourdevicekey}** met de apparaat-sleutelwaarde die u hebt gegenereerd in de sectie *een apparaat-identiteit maken* :

    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.AMQP;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```

    In dit voorbeeld-toepassing wordt de variabele **protocol** gebruikt wanneer deze een **DeviceClient** -object wordt. U kunt de HTTP- of AMQP protocol gebruiken om te communiceren met IoT Hub.

8. Toevoegen aan dat de volgende geneste **TelemetryDataPoint** class binnen de **App** -klasse om op te geven van de telemetriegegevens die uw apparaat wordt verzonden naar uw hub IoT:

    ```
    private static class TelemetryDataPoint {
      public String deviceId;
      public double windSpeed;
        
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```

9. Voeg de volgende geneste **EventCallback** klasse binnen de **App** -klasse om weer te geven van de status bevestiging dat de hub IoT retourneert wanneer een bericht van het apparaat dat gesimuleerd worden verwerkt. Deze methode ook de belangrijkste thread in de toepassing een melding wanneer het bericht is verwerkt:

    ```
    private static class EventCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
      
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. De volgende geneste **MessageSender** klasse binnen de **App** -klasse toevoegen. De methode **run** in deze klasse telemetrielogboek voorbeeldgegevens te verzenden naar uw hub IoT genereert en wacht een bevestiging voor het volgende bericht verzenden:

    ```
    private static class MessageSender implements Runnable {
      public volatile boolean stopThread = false;
      
      public void run()  {
        try {
          double avgWindSpeed = 10; // m/s
          Random rand = new Random();
          
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.windSpeed = currentWindSpeed;
            
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            System.out.println("Sending: " + msgStr);
            
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
            
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```

    Deze methode wordt een nieuw apparaat naar cloud-mailbericht van één seconde nadat de hub IoT bevestigt het vorige bericht stuurt. Het bericht bevat een object JSON serienummer met apparaat-id en een willekeurig wordt gegenereerd getal tot een wind snelheid sensor simuleren.

11. De **belangrijkste** methode vervangen door de volgende code die wordt gemaakt van een thread apparaat naar cloud om berichten te verzenden naar uw hub IoT:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();

      MessageSender sender = new MessageSender();

      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);

      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.close();
    }
    ```

12. Opslaan en sluit het bestand App.java.

13. Maak de **gesimuleerd apparaat** -toepassing met Maven door uitvoeren bij de opdrachtprompt de volgende opdracht uit in de map gesimuleerd apparaat:

    ```
    mvn clean package -DskipTests
    ```

> [AZURE.NOTE] Als u wilt houden dingen eenvoudige, wordt deze zelfstudie opnieuw beleid niet geïmplementeerd. In productiecode, moet u opnieuw beleid (zoals een exponentiële backoff), zoals aangegeven in het MSDN-artikel [Tijdelijke foutenstructuuranalyse afhandelen]implementeren[lnk-transient-faults].

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren de toepassingen.

1. Voer de volgende opdracht naar begint met het controleren van de eerste partition in de hub IoT-opdrachtprompt in de map lezen d2c:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java IoT Hub service-clienttoepassing voor het apparaat naar cloud berichten worden gecontroleerd][7]

2. Opdrachtprompt in de map gesimuleerd-apparaat, voert u de volgende opdracht uit om te beginnen met het verzenden van telemetriegegevens op uw hub IoT:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Java IoT Hub device-clienttoepassing apparaat naar cloud berichten te verzenden][8]

3. De tegel voor **Gebruik** in de [portal van Azure] [ lnk-portal] bevat het nummer van berichten die zijn verzonden naar de hub:

    ![Azure portal gebruik tegel met aantal aan IoT Hub verzonden berichten][43]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie kunt u een nieuwe IoT hub geconfigureerd in de portal van Azure, en vervolgens de identiteit van een apparaat in de hub identiteit register gemaakt. U deze identiteit apparaat gebruikt om de app gesimuleerd apparaat apparaat naar cloud berichten naar de hub te verzenden. U ook een app waarin de berichten ontvangen door de hub hebt gemaakt. 

Andere IoT scenario's Zie gaat u verder aan de slag met IoT Hub en verkennen:

- [Verbinding maken van uw apparaat][lnk-connect-device]
- [Aan de slag met Apparaatbeheer][lnk-device-management]
- [Aan de slag met de SDK van de Gateway IoT][lnk-gateway-SDK]

Als u wilt weten hoe u uw oplossing IoT uitbreiden en apparaat-naar-cloud-berichten bij het op schaal verwerkt, raadpleegt u het [proces apparaat naar cloud berichten] [ lnk-process-d2c-tutorial] zelfstudie.

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/