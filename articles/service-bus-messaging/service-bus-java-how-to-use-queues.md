<properties
    pageTitle="Het gebruik van de Service Bus wachtrijen met Java | Microsoft Azure"
    description="Informatie over het gebruik van de Service Bus wachtrijen in Azure wordt aangegeven. Voorbeelden van de code is geschreven in Java."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Het gebruik van de Service Bus wachtrijen

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

In dit artikel wordt beschreven hoe Service Bus wachtrijen gebruiken. In de voorbeelden in Java zijn geschreven en gebruik van de [Azure SDK for Java][]. De scenario's waarvoor zijn **maken van wachtrijen** **verzenden en ontvangen van berichten**en **wachtrijen verwijderen**.

## <a name="what-are-service-bus-queues"></a>Wat zijn Service Bus wachtrijen?

Service Bus wachtrijen ondersteuning voor een **brokered messaging** communicatie-model. Wanneer u met wachtrijen, communiceren onderdelen van een gedistribueerde toepassing niet rechtstreeks met elkaar. in plaats daarvan wisselen ze berichten via een wachtrij bemiddelt (makelaar). Een bericht producent (afzender) uit een bericht naar de wachtrij handen en vervolgens de verwerking.
Asynchroon, een bericht consumenten (ontvanger) ophaalt van het bericht uit de wachtrij en verwerkt. De producent beschikt niet over moet wachten om een antwoord van de consument om verder te kunnen verwerken en verder berichten verzenden. Wachtrijen bieden de **eerste keer hebt aangemeld, eerste Out (FIFO)** berichtbezorging naar een of meer concurrerende consumenten. Berichten zijn dat wil zeggen, meestal ontvangen en verwerkt door de ontvangers in de volgorde waarin ze zijn toegevoegd aan de wachtrij en elk bericht is ontvangen en verwerkt door consumenten slechts één bericht.

![QueueConcepts](./media/service-bus-java-how-to-use-queues/sb-queues-08.png)

Service Bus wachtrijen zijn een algemene technologie die kan worden gebruikt voor een groot aantal scenario's:

- De communicatie tussen web en werknemer rollen in een meerlagige Azure-toepassing.
- Communicatie tussen on-premises implementatie-apps en Azure gehost apps in een hybride-oplossing.
- De communicatie tussen onderdelen van een gedistribueerde toepassing on-premises implementatie uitgevoerd in andere organisaties of afdelingen van een organisatie.

Met wachtrijen kunt u eenvoudiger uw toepassingen schaal en tolerantie in uw architectuur inschakelen.

## <a name="create-a-service-namespace"></a>Een Servicenaamruimte maken

Als u wilt gebruiken Service Bus wachtrijen in Azure wordt aangegeven, moet u eerst een naamruimte maken. Een naamruimte bevat een scope container voor de adressering van Service Bus bronnen binnen uw toepassing.

Een naamruimte maken:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing Service Bus configureren

Zorg ervoor dat u hebt de [Azure SDK for Java][] geïnstalleerd voordat u dit voorbeeld samenstellen. Als u Eclips gebruikt, kunt u de [Azure Toolkit voor Eclips][] waarin de Azure SDK for Java installeren. Vervolgens kunt u de **Microsoft Azure bibliotheken voor Java** toevoegen aan uw project:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Voeg de volgende `import` instructies naar het begin van het Java-bestand:

```
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Een wachtrij maken

Beheertaken uit te voeren voor Service Bus wachtrijen kunnen worden uitgevoerd via de klasse **ServiceBusContract** . Een object **ServiceBusContract** is gemaakt met een juiste configuratie die door het token SA's met machtigingen voor het beheren van dit omvat en de **ServiceBusContract** -klasse is het enige communicatie met Azure.

De klasse **ServiceBusService** bevat methoden voor het maken, opsommen en wachtrijen verwijderen. Het volgende voorbeeld ziet u hoe een object **ServiceBusService** kan worden gebruikt om een wachtrij met de naam 'TestQueue', met een naamruimte met de naam "HowToSample" te maken:

```
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Er zijn methoden op **QueueInfo** waarmee de eigenschappen van de wachtrij worden ingesteld (bijvoorbeeld: om in te stellen om de standaardwaarde time to live (TTL) moeten worden toegepast op berichten die zijn verzonden naar de wachtrij). Het volgende voorbeeld ziet u het maken van een wachtrij met de naam `TestQueue` met een maximale grootte van 5GB:

````
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Houd er rekening mee dat u de methode **listQueues** op **ServiceBusContract** objecten gebruiken kunt om te controleren of een wachtrij met een opgegeven naam al in een Servicenaamruimte bestaat.

## <a name="send-messages-to-a-queue"></a>Berichten verzenden naar een wachtrij

Als u wilt een bericht verzenden naar een Service Bus wachtrij, wordt een object **ServiceBusContract** opgehaald door uw toepassing. De volgende code ziet u hoe u een bericht verzendt voor de `TestQueue` wachtrij eerder hebt gemaakt de `HowToSample` naamruimte:

```
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Berichten naar verzonden en ontvangen van Service Bus wachtrijen zijn exemplaren van de klasse [BrokeredMessage][] . [BrokeredMessage][] objecten hebben een reeks standaardeigenschappen (zoals [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) en [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), een woordenlijst die wordt gebruikt om specifieke eigenschappen maatwerktoepassing en een willekeurige toepassing gegevens. Een toepassing de hoofdtekst van het bericht kunt instellen door een willekeurig serialiseerbaar object aan de constructor van de [BrokeredMessage][]en de bijbehorende serialisatiefunctie serialiseren van het object vervolgens worden gebruikt. U kunt ook een java **opgeeft. IO. InputStream** object.

Het volgende voorbeeld ziet u hoe u verzendt vijf berichten om te testen de `TestQueue` **MessageSender** we in het vorige codefragment verkregen:

```
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

Service Bus wachtrijen ondersteuning voor een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De kop, die de standaardkleuren of aangepaste toepassingseigenschappen bevat, kan een maximale grootte van 64 KB hebben. Er is geen limiet voor het aantal berichten in een wachtrij gezet, maar er is een initiaal van de totale grootte van de berichten die door een wachtrij gehouden. De wachtrijgrootte van deze is opgegeven bij het maken, met een maximum van 5 GB.

## <a name="receive-messages-from-a-queue"></a>Berichten ontvangt van een wachtrij

De primaire manier om berichten ontvangt van een wachtrij is om met een object **ServiceBusContract** . Ontvangen berichten kunnen werken in twee verschillende modi: **ReceiveAndDelete** en **PeekLock**.

Wanneer de **ReceiveAndDelete** -modus met ontvangt is een bewerking foto - dat wil zeggen wanneer Service Bus een leesbevestiging voor een bericht in een wachtrij ontvangt, wordt Hiermee markeert u het bericht als wordt verbruikt en naar de toepassing wordt geretourneerd. **ReceiveAndDelete** -modus (dit is de standaardmodus) is de eenvoudigste model en werkt het best voor scenario's waarin een toepassing mogelijk zonder niet verwerken van een bericht in het geval van een fout. Als u wilt weten over dit, kunt u een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze wordt verwerkt.
Omdat Service Bus wordt het bericht als wordt verbruikt hebt gemarkeerd, wordt klikt u vervolgens wanneer de toepassing opnieuw is opgestart en begint door nogmaals, andere berichten deze heb gemist het bericht verbruikte vóór het vastlopen.

In de modus **PeekLock** , ontvangen een bewerking twee fase, die het mogelijk maakt naar ondersteuningstoepassingen die u kunnen geen ontbrekende berichten zonder verandert. Als Service Bus een aanvraag ontvangt, deze vindt u het volgende bericht worden verbruikt, vergrendelen om te voorkomen dat andere consumenten ontvangen en vervolgens naar de toepassing wordt geretourneerd. Nadat de toepassing verwerking van het bericht (of betrouwbaar voor toekomstige verwerking opgeslagen), is de tweede fase van het proces ontvangen door de ondersteuning voor **verwijderen** op het ontvangen bericht voltooid. Wanneer Service Bus ziet het gesprek **verwijderen** , wordt deze markeert u het bericht worden gebruikt en deze te verwijderen uit de wachtrij.

Het volgende voorbeeld wordt gedemonstreerd hoe berichten kunnen worden ontvangen en verwerkt met behulp van **PeekLock** -modus (niet de standaardmodus). Het volgende voorbeeld wordt een lus en berichten verwerkt wanneer deze in onze "TestQueue binnenkomen":

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the queue message.
            System.out.print("From queue: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Hoe u omgaat met toepassing loopt en gelezen berichten

Service Bus biedt een functie waarmee u fouten in uw toepassing of problemen verwerken van een bericht zonder problemen herstellen. Als een toepassing voor de ontvanger kan niet worden verwerkt van het bericht om een bepaalde reden is, kan deze de methode **unlockMessage** aanroepen voor het ontvangen bericht (in plaats van de methode **deleteMessage** ). Hierdoor wordt de Service Bus u kunt het bericht in de wachtrij ontgrendelen en deze beschikbaar zijn voor het opnieuw worden ontvangen door de dezelfde in beslag nemen toepassing of door een andere in beslag nemen toepassing.

Er is ook een time-out die is gekoppeld aan een bericht in de wachtrij vergrendeld en als de toepassing mislukt verwerkingstijd van het bericht voordat u de vergrendelen time-out is verstreken (bijvoorbeeld als de toepassing loopt vast), en vervolgens Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen.

In het geval dat de toepassing loopt vast na het verwerken van het bericht, maar voordat het verzoek **deleteMessage** is uitgegeven, wordt klikt u vervolgens het bericht worden geleverd met de toepassing opnieuw wordt gestart. Heet dit vaak **Tenminste eenmaal Processing**, dat wil zeggen elk bericht wordt ten minste eenmaal worden verwerkt maar in sommige gevallen hetzelfde bericht kan worden geleverd. Als u het scenario kan geen dubbele verwerking zonder, kunnen softwareontwikkelaars moeten extra logica toevoegen aan de toepassing u omgaat met dubbele bericht is bezorgd. Dit is vaak bereikt met de methode **getMessageId** van het bericht, constant over bezorging pogingen blijft.

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Service Bus wachtrijen hebt geleerd, raadpleegt u [wachtrijen, onderwerpen en abonnementen][] voor meer informatie.

Zie het [Java Developer Center](/develop/java/)voor meer informatie.


  [Azure SDK for Java]: http://azure.microsoft.com/develop/java/
  [Azure Toolkit voor Eclips]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

