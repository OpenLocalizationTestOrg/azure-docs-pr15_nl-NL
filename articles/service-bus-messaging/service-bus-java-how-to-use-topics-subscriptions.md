<properties
    pageTitle="Het gebruik van de Service Bus onderwerpen met Java | Microsoft Azure"
    description="Informatie over het gebruik van de Service Bus onderwerpen en abonnementen in Azure wordt aangegeven. Voorbeelden van code worden geschreven voor Java-toepassingen."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Het gebruik van de Service Bus onderwerpen en abonnementen

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Deze handleiding wordt beschreven hoe Service Bus onderwerpen en abonnementen gebruiken. In de voorbeelden in Java zijn geschreven en gebruik van de [Azure SDK for Java][]. De scenario's waarvoor zijn **maken van onderwerpen en abonnementen** **abonnement filters maken**, **berichten verzenden naar een onderwerp**, **ontvangen van berichten van een abonnement**en **verwijderen van onderwerpen en abonnementen**.

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Wat zijn Service Bus onderwerpen en abonnementen?

Ondersteund in een *publicatie/abonnementen* communicatiemodel messaging service Bus onderwerpen en abonnementen. Wanneer u onderwerpen en abonnementen, communiceren onderdelen van een gedistribueerde toepassing niet rechtstreeks met elkaar. in plaats daarvan uitwisselen deze berichten via een onderwerp bemiddelt.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Geef een 'een-op-veel'-vorm van communicatie, met een patroon publiceren/abonneren in tegenstelling tot de Service Bus wachtrijen, waarin elk bericht wordt verwerkt door een enkel consument, onderwerpen en abonnementen. Het is mogelijk meerdere abonnementen op een onderwerp registreren. Wanneer een bericht wordt verzonden naar een onderwerp, dit is vervolgens ter beschikking gesteld aan elk abonnement greep/afzonderlijk proces.

Een abonnement op een onderwerp lijkt op een virtuele wachtrij kopieën van de berichten die zijn verzonden naar het onderwerp. U kunt desgewenst filterregels voor een onderwerp op basis van een per abonnement, zodat u kunt het filter/beperken welke berichten u wilt een onderwerp worden ontvangen door welke abonnementen onderwerp registreren.

Service Bus onderwerpen en -abonnementen kunnen u aan de nieuwe schaal verwerkingstijd van een groot aantal berichten over een groot aantal gebruikers en toepassingen.

## <a name="create-a-service-namespace"></a>Een Servicenaamruimte maken

Als u wilt gebruiken Service Bus onderwerpen en abonnementen in Azure wordt aangegeven, moet u eerst de naamruimte van een service maken. Een naamruimte bevat een scope container voor de adressering van Service Bus bronnen binnen uw toepassing.

Een naamruimte maken:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Uw toepassing Service Bus configureren

Zorg ervoor dat u hebt de [Azure SDK for Java][] geïnstalleerd voordat u dit voorbeeld samenstellen. Als u Eclips gebruikt, kunt u de [Azure Toolkit voor Eclips][] waarin de Azure SDK for Java installeren. Vervolgens kunt u de **Microsoft Azure bibliotheken voor Java** toevoegen aan uw project:

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

De volgende importinstructies toevoegen aan het begin van het Java-bestand:

```
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

De bibliotheken Azure voor Java toevoegen aan uw pad opbouwen en opnemen in uw project implementatie constructie.

## <a name="create-a-topic"></a>Een onderwerp maken

Beheertaken uit te voeren voor Service Bus onderwerpen kunnen worden uitgevoerd via de klasse **ServiceBusContract** . Een object **ServiceBusContract** is gemaakt met een juiste configuratie die door het token SA's met machtigingen voor het beheren van dit omvat en de **ServiceBusContract** -klasse is het enige communicatie met Azure.

De klasse **ServiceBusService** bevat methoden voor het maken en verwijderen van onderwerpen opsommen. Het volgende voorbeeld ziet u hoe een object **ServiceBusService** kan worden gebruikt om te maken van een onderwerp met de naam `TestTopic`, met een naamruimte genoemd `HowToSample`:

    Configuration config =
        ServiceBusConfiguration.configureWithSASAuthentication(
          "HowToSample",
          "RootManageSharedAccessKey",
          "SAS_key_value",
          ".servicebus.windows.net"
          );

    ServiceBusContract service = ServiceBusService.create(config);
    TopicInfo topicInfo = new TopicInfo("TestTopic");
    try  
    {
        CreateTopicResult result = service.createTopic(topicInfo);
    }
    catch (ServiceException e) {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

Er zijn methoden op **TopicInfo** waarmee de eigenschappen van het onderwerp worden ingesteld (bijvoorbeeld: om in te stellen om de standaardwaarde time to live (TTL) moeten worden toegepast op berichten die zijn verzonden naar het onderwerp). Het volgende voorbeeld ziet u het maken van een onderwerp met de naam `TestTopic` met een maximale grootte van 5 GB:

    long maxSizeInMegabytes = 5120;  
    TopicInfo topicInfo = new TopicInfo("TestTopic");  
    topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
    CreateTopicResult result = service.createTopic(topicInfo);

Houd er rekening mee dat u de methode **listTopics** op **ServiceBusContract** objecten gebruiken kunt om te controleren of een onderwerp met een opgegeven naam al in een Servicenaamruimte bestaat.

## <a name="create-subscriptions"></a>Abonnementen maken

Abonnementen op onderwerpen worden ook de klas **ServiceBusService** gemaakt. Abonnementen zijn met de naam en een optionele filter waarmee de verzameling berichten doorgegeven aan de virtuele wachtrij van het abonnement wordt beperkt kunnen hebben.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement maken met het standaardfilter (MatchAll)

Het filter **MatchAll** is het standaardfilter die wordt gebruikt als er geen filter is opgegeven als een nieuw abonnement wordt gemaakt. Wanneer het **MatchAll** -filter wordt gebruikt, worden alle berichten die zijn gepubliceerd naar het onderwerp van het abonnement virtuele wachtrij geplaatst. Het volgende voorbeeld wordt gemaakt van een abonnement met de naam 'AllMessages' en het standaardfilter voor **MatchAll** gebruikt.

    SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken

U kunt ook filters waarmee u naar een bereik dat berichten die zijn verzonden naar een onderwerp dat moeten worden weergegeven binnen een bepaald onderwerp abonnement maken.

Het meest flexibele type filter die worden ondersteund in abonnementen is de [SqlFilter][], waarmee een subset van SQL92-standaard worden geïmplementeerd. SQL-filters worden uitgevoerd op de eigenschappen van de berichten die zijn gepubliceerd naar het onderwerp. Bekijk de syntaxis van de [SqlFilter.SqlExpression][] voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter.

Het volgende voorbeeld wordt een abonnement met de naam `HighMessages` met een [SqlFilter][] -object dat u selecteert alleen berichten met een aangepaste **MessageNumber** eigenschap die groter is dan 3:

```
// Create a "HighMessages" filtered subscription  
SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "HighMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "HighMessages", "$Default");
```

Ook het volgende voorbeeld wordt een abonnement met de naam `LowMessages` met een [SqlFilter][] -object dat u selecteert alleen berichten met een **MessageNumber** eigenschap kleiner is dan of gelijk aan 3:

```
// Create a "LowMessages" filtered subscription
SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
CreateSubscriptionResult result = service.createSubscription("TestTopic", subInfo);
RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
CreateRuleResult ruleResult = service.createRule("TestTopic", "LowMessages", ruleInfo);
// Delete the default rule, otherwise the new rule won't be invoked.
service.deleteRule("TestTopic", "LowMessages", "$Default");
```

Wanneer een bericht nu wordt verzonden naar `TestTopic`, deze altijd worden bezorgd bij ontvangers geabonneerd op de `AllMessages` -abonnement en selectief geleverde met ontvangers geabonneerd op de `HighMessages` en `LowMessages` abonnementen (afhankelijk van de inhoud van het bericht).

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp

Als u wilt een bericht verzenden naar een onderwerp Service Bus, wordt een object **ServiceBusContract** opgehaald door uw toepassing. De volgende code ziet u hoe u een bericht verzendt voor de `TestTopic` onderwerp eerder hebt gemaakt in de `HowToSample` naamruimte:

```
BrokeredMessage message = new BrokeredMessage("MyMessage");
service.sendTopicMessage("TestTopic", message);
```

Berichten die zijn verzonden naar Service Bus onderwerpen zijn exemplaren van de klasse [BrokeredMessage][] . [BrokeredMessage][] *objecten bevatten een set standaard methoden (zoals * *setLabel* * en * *TimeToLive**), een woordenlijst die wordt gebruikt om aangepaste toepassingsspecifieke eigenschappen en een willekeurige toepassing gegevens. Een toepassing de hoofdtekst van het bericht kunt instellen door een willekeurig serialiseerbaar object aan de constructor van de [BrokeredMessage][]en de juiste * *DataContractSerializer* * serialiseren van het object vervolgens worden gebruikt. U kunt ook een * *java.io.InputStream** kan worden verstrekt.

Het volgende voorbeeld ziet u hoe u verzendt vijf berichten om te testen de `TestTopic` **MessageSender** we in de bovenstaande codefragment verkregen.
Houd er rekening mee hoe de waarde van de eigenschap **MessageNumber** van elk bericht op de herhaling van de lus varieert (dit veld bepaalt welke abonnementen ontvangen):

```
for (int i=0; i<5; i++)  {
// Create message, passing a string message for the body
BrokeredMessage message = new BrokeredMessage("Test message " + i);
// Set some additional custom app-specific property
message.setProperty("MessageNumber", i);
// Send message to the topic
service.sendTopicMessage("TestTopic", message);
}
```

Service Bus onderwerpen ondersteuning voor een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De kop, die de standaardkleuren of aangepaste toepassingseigenschappen bevat, kan een maximale grootte van 64 KB hebben. Er is geen limiet voor het aantal berichten in een onderwerp gehouden, maar er is een initiaal van de totale grootte van de berichten die door een onderwerp. De grootte van dit onderwerp wordt opgegeven bij het maken, met een maximum van 5 GB.

## <a name="how-to-receive-messages-from-a-subscription"></a>Hoe u berichten ontvangt van een abonnement

Ontvangen berichten van een abonnement, gebruikt u een object **ServiceBusContract** . Ontvangen berichten kunnen werken in twee verschillende modi: **ReceiveAndDelete** en **PeekLock**.

Wanneer de **ReceiveAndDelete** -modus met ontvangt is een bewerking foto - dat wil zeggen wanneer Service Bus een leesbevestiging voor een bericht ontvangt, wordt Hiermee markeert u het bericht als wordt verbruikt en naar de toepassing wordt geretourneerd. **ReceiveAndDelete** -modus is de eenvoudigste model en werkt het best voor scenario's waarin een toepassing mogelijk zonder niet verwerken van een bericht in het geval van een fout. Als u wilt weten over dit, kunt u een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze wordt verwerkt. Omdat Service Bus wordt het bericht als wordt verbruikt hebt gemarkeerd, wordt klikt u vervolgens wanneer de toepassing opnieuw is opgestart en begint door nogmaals, andere berichten deze heb gemist het bericht verbruikte vóór het vastlopen.

In de modus **PeekLock** , ontvangen een bewerking twee fase, die het mogelijk maakt naar ondersteuningstoepassingen die u kunnen geen ontbrekende berichten zonder verandert. Als Service Bus een aanvraag ontvangt, deze vindt u het volgende bericht worden verbruikt, vergrendelen om te voorkomen dat andere consumenten ontvangen en vervolgens naar de toepassing wordt geretourneerd. Nadat de toepassing verwerking van het bericht (of betrouwbaar voor toekomstige verwerking opgeslagen), is de tweede fase van het proces ontvangen door de ondersteuning voor **verwijderen** op het ontvangen bericht voltooid. Wanneer Service Bus ziet het gesprek **verwijderen** , wordt deze markeert u het bericht worden gebruikt en deze te verwijderen uit het onderwerp.

Het volgende voorbeeld wordt gedemonstreerd hoe berichten kunnen worden ontvangen en verwerkt met behulp van **PeekLock** -modus (niet de standaardmodus). In het onderstaande voorbeeld kunt u een lus uitvoeren en de berichten in het abonnement "HighMessages" verwerkt en vervolgens afgesloten wanneer er geen berichten meer zijn (u kunt ook deze kan worden ingesteld op wachten voor nieuwe berichten).

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
        ReceiveSubscriptionMessageResult  resultSubMsg =
            service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
        BrokeredMessage message = resultSubMsg.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the topic message.
            System.out.print("From topic: ");
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
                message.getProperty("MessageNumber"));
            // Delete message.
            System.out.println("Deleting this message.");
            service.deleteMessage(message);
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

Service Bus biedt een functie waarmee u fouten in uw toepassing of problemen verwerken van een bericht zonder problemen herstellen. Als een toepassing voor de ontvanger kan niet worden verwerkt van het bericht om een bepaalde reden is, kan deze de methode **unlockMessage** aanroepen voor het ontvangen bericht (in plaats van de methode **deleteMessage** ). Hierdoor wordt de Service Bus u kunt het bericht in het onderwerp te ontgrendelen en deze beschikbaar zijn voor het opnieuw worden ontvangen door de dezelfde in beslag nemen toepassing of door een andere in beslag nemen toepassing.

Er is ook een time-out die is gekoppeld aan een bericht in het onderwerp is vergrendeld en als de toepassing mislukt verwerkingstijd van het bericht voordat u de vergrendelen time-out is verstreken (bijvoorbeeld als de toepassing loopt vast), en vervolgens Service Bus wordt het bericht automatisch ontgrendelen en beschikbaar stellen aan opnieuw worden ontvangen.

In het geval dat de toepassing loopt vast na het verwerken van het bericht, maar voordat het verzoek **deleteMessage** is uitgegeven, wordt klikt u vervolgens het bericht worden geleverd met de toepassing opnieuw wordt gestart. Heet dit vaak **Tenminste eenmaal Processing**, dat wil zeggen elk bericht wordt ten minste eenmaal worden verwerkt maar in sommige gevallen hetzelfde bericht kan worden geleverd. Als u het scenario kan geen dubbele verwerking zonder, kunnen softwareontwikkelaars moeten extra logica toevoegen aan de toepassing u omgaat met dubbele bericht is bezorgd. Dit is vaak bereikt met de methode **getMessageId** van het bericht, constant over bezorging pogingen blijft.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen

De primaire manier om het verwijderen van onderwerpen en abonnementen is om met een object **ServiceBusContract** . Een onderwerp worden ook verwijdert, alle abonnementen die zijn geregistreerd met het onderwerp. Abonnementen kunnen ook afzonderlijk worden verwijderd.

```
// Delete subscriptions
service.deleteSubscription("TestTopic", "AllMessages");
service.deleteSubscription("TestTopic", "HighMessages");
service.deleteSubscription("TestTopic", "LowMessages");

// Delete a topic
service.deleteTopic("TestTopic");
```

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Service Bus wachtrijen hebt geleerd, raadpleegt u de [Service Bus wachtrijen, onderwerpen en abonnementen][] voor meer informatie.

  [Azure SDK for Java]: http://azure.microsoft.com/develop/java/
  [Azure Toolkit voor Eclips]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Azure classic portal]: http://manage.windowsazure.com/
  [Service Bus wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx 
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  
  [0]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-13.png
  [2]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-04.png
  [3]: ./media/service-bus-java-how-to-use-topics-subscriptions/sb-queues-09.png
