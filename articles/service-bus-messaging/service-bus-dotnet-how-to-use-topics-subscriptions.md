<properties
    pageTitle="Service Bus onderwerpen gebruiken met .NET | Microsoft Azure"
    description="Informatie over het gebruik van de Service Bus onderwerpen en abonnementen met .NET in Azure wordt aangegeven. Voorbeelden van code worden geschreven voor .NET-toepassingen."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Het gebruik van de Service Bus onderwerpen en abonnementen

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

In dit artikel wordt beschreven hoe Service Bus onderwerpen en abonnementen gebruiken. In de voorbeelden zijn geschreven in C# en gebruikt u de .NET-API's. De scenario's waarvoor opnemen maken van onderwerpen en -abonnement, abonnement filters maken, verzenden van berichten naar een onderwerp, ontvangen van berichten van een abonnement en verwijderen van onderwerpen en abonnementen. Zie voor meer informatie over onderwerpen en abonnementen, de sectie van de [volgende stappen](#next-steps) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="configure-the-application-to-use-service-bus"></a>De toepassing configureren voor gebruik van de Service Bus

Wanneer u een toepassing die gebruikmaakt van Service Bus maakt, moet u een verwijzing naar de Service Bus constructie toevoegen en de bijbehorende naamruimten opnemen. De eenvoudigste manier hiervoor is om te downloaden van het juiste [NuGet](https://www.nuget.org) -pakket.

## <a name="get-the-service-bus-nuget-package"></a>Het pakket Service Bus NuGet ophalen

De [Service Bus NuGet-pakket](https://www.nuget.org/packages/WindowsAzure.ServiceBus) is de eenvoudigste manier om de Service Bus API en voor het configureren van uw toepassing met alle benodigde Service Bus afhankelijkheden. Ga als volgt te werk als u wilt installeren van de Service Bus NuGet-pakket in uw project:

1.  Klik in Solution Explorer met de rechtermuisknop op **verwijzingen**en klik **NuGet-pakketten beheren**.
2.  Zoeken naar 'Service Bus' en selecteer het item **Microsoft Azure Service Bus** . Klik op **installeren** om de installatie te voltooien en sluit het volgende dialoogvenster:

    ![][7]

U bent nu klaar om te schrijven code voor Service Bus.

## <a name="create-a-service-bus-connection-string"></a>Een verbindingsreeks Service Bus maken

Een verbindingsreeks Service Bus gebruikt om op te slaan eindpunten en referenties. U kunt de verbindingsreeks in een configuratiebestand, in plaats van harde codering deze plaatsen:

- Wanneer u een Azure services gebruikt, wordt het wordt aanbevolen de verbindingsreeks met de configuratie-mailsysteem van Azure-service (.csdef en .cscfg bestanden) op te slaan.
- Wanneer u met Azure websites of Azure virtuele Machines, wordt het wordt aanbevolen de verbindingsreeks met het .NET-configuratie-systeem (bijvoorbeeld het Web.config-bestand) op te slaan.

In beide gevallen kunt u ophalen uw verbinding tekenreeks gebruiken de `CloudConfigurationManager.GetSetting` methode, zoals verderop in dit artikel.

### <a name="configure-your-connection-string"></a>De verbindingsreeks configureren

De service configuratie om kunt u dynamisch configuratie-instellingen van de [Azure portal][] wijzigen zonder de toepassing opnieuw te distribueren. Bijvoorbeeld toevoegen een `Setting` label naar uw servicebestand definitie (**.csdef**), zoals wordt weergegeven in het volgende voorbeeld.

```
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

U kunt vervolgens waarden opgeven in het configuratiebestand van service (.cscfg).

```
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Gebruik de naam van gedeelde Access handtekening (SA's) en belangrijke waarden opgehaald uit de portal zoals eerder is beschreven.

### <a name="configure-your-connection-string-when-using-azure-websites-or-azure-virtual-machines"></a>De verbindingsreeks configureren bij gebruik van Azure websites of Azure virtuele Machines

Wanneer u websites of virtuele Machines, is het aanbevolen dat u het .NET-configuratie-systeem (bijvoorbeeld Web.config) gebruiken. Slaat u de verbinding tekenreeks gebruiken de `<appSettings>` element.

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Gebruik de naam van de SA's en -sleutelwaarden die u hebt opgehaald van de [Azure-portal][], zoals eerder is beschreven.

## <a name="create-a-topic"></a>Een onderwerp maken

U kunt management bewerkingen voor Service Bus onderwerpen en abonnementen die gebruikmaken van de klas [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) uitvoeren. Deze klasse biedt methoden voor het maken en verwijderen van onderwerpen opsommen.

Het volgende voorbeeld wordt een `NamespaceManager` object met het gebruik van de Azure `CloudConfigurationManager` klasse met een verbindingsreeks die bestaat uit het basisadres van een Service Bus naamruimte en de juiste SA's referenties met machtigingen om te beheren. Deze verbindingsreeks heeft de volgende vorm:

```
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

Gebruik het volgende voorbeeld wordt de configuratie-instellingen in de vorige sectie gegeven.

```
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

Er zijn overbelastingen van de methode [CreateTopic](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createtopic.aspx) waarmee u kunt de eigenschappen van het onderwerp; instellen bijvoorbeeld: de standaardinstelling time to live (TTL) de waarde om te worden toegepast op berichten die zijn verzonden naar het onderwerp. Deze instellingen worden toegepast met behulp van de klasse [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) . Het volgende voorbeeld ziet u hoe u een onderwerp met de naam **TestTopic** met een maximale grootte van 5 GB en een standaardbericht TTL van 1 minuut maakt.

```
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [AZURE.NOTE] U kunt de methode [TopicExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.topicexists.aspx) op [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) objecten gebruiken om te controleren of een onderwerp met een opgegeven naam al in een naamruimte bestaat.

## <a name="create-a-subscription"></a>Een abonnement maken

U kunt ook onderwerp-abonnementen die gebruikmaken van de klas [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) maken. Abonnementen zijn met de naam en een optionele filter waarmee de verzameling berichten doorgegeven aan de virtuele wachtrij van het abonnement wordt beperkt kunnen hebben.

> [AZURE.IMPORTANT] In de volgorde voor berichten om te worden ontvangen door een abonnement, moet u die abonnement voor alle berichten verzenden naar het onderwerp. Als er geen abonnementen op een onderwerp, wordt in het onderwerp die berichten verwijdert.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Een abonnement maken met het standaardfilter (MatchAll)

Als geen filter is opgegeven als een nieuw abonnement wordt gemaakt, is het filter **MatchAll** het standaardfilter die wordt gebruikt. Wanneer u het filter **MatchAll** gebruikt, worden alle berichten die zijn gepubliceerd naar het onderwerp van het abonnement virtuele wachtrij geplaatst. Het volgende voorbeeld wordt gemaakt van een abonnement met de naam 'AllMessages' en het standaardfilter voor **MatchAll** gebruikt.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### <a name="create-subscriptions-with-filters"></a>Abonnementen met filters maken

U kunt ook filters waarmee u kunt opgeven welke berichten u verzonden naar een onderwerp moeten instellen worden weergegeven binnen een bepaald onderwerp-abonnement.

Het meest flexibele type filter die worden ondersteund in abonnementen is de klasse [SqlFilter][] , waarmee een subset van SQL92-standaard worden geïmplementeerd. SQL-filters worden uitgevoerd op de eigenschappen van de berichten die zijn gepubliceerd naar het onderwerp. Zie voor meer informatie over de expressies die kunnen worden gebruikt met een SQL-filter, de syntaxis van de [SqlFilter.SqlExpression][] .

Het volgende voorbeeld wordt een abonnement met de naam **HighMessages** met een [SqlFilter][] -object dat u selecteert alleen berichten met een aangepaste **MessageNumber** eigenschap die groter is dan 3.

```
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Het volgende voorbeeld wordt ook een abonnement met de naam **LowMessages** met een [SqlFilter][] die selecteert alleen berichten met een **MessageNumber** eigenschap kleiner is dan of gelijk aan 3.

```
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

Nu wanneer een bericht wordt verzonden naar `TestTopic`, wordt altijd bezorgd bij ontvangers geabonneerd op het abonnement dat **AllMessages** onderwerp en selectief bezorgd bij ontvangers geabonneerd op de **HighMessages** en **LowMessages** onderwerp abonnementen (afhankelijk van de inhoud van het bericht).

## <a name="send-messages-to-a-topic"></a>Berichten verzenden naar een onderwerp

Als u wilt een bericht verzenden naar een Service Bus onderwerp, maakt de toepassing een [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) -object met de verbindingsreeks.

De volgende code ziet het maken van een object [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) voor het **TestTopic** onderwerp gemaakt eerder met behulp van de [`CreateFromConnectionString`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.createfromconnectionstring.aspx) API-oproep.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

Berichten die zijn verzonden naar Service Bus onderwerpen zijn exemplaren van de klasse [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) . [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) objecten hebben een reeks standaardeigenschappen (zoals [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) en [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)), een woordenlijst die wordt gebruikt om aangepaste toepassingsspecifieke eigenschappen en een willekeurige toepassing gegevens. Een toepassing de hoofdtekst van het bericht met een willekeurig object serialiseerbaar doorgeven aan de constructor van het object [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) kan worden ingesteld, en de juiste **DataContractSerializer** vervolgens serialiseren van het object wordt gebruikt. U kunt ook kan een **System.IO.Stream** worden verstrekt.

Het volgende voorbeeld laat zien hoe vijf testberichten naar het **TestTopic** [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) object verkregen in het vorige voorbeeld te verzenden. Opmerking die de waarde van de eigenschap [MessageNumber](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.properties.aspx) van elk bericht afhankelijk van de herhaling van de lus varieert (Hiermee bepaalt u welke abonnementen ontvangen).

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageNumber"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Service Bus onderwerpen ondersteuning voor een maximale grootte van 256 KB in de [standaard laag](service-bus-premium-messaging.md) en 1 MB in de [Premium-laag](service-bus-premium-messaging.md). De kop, die de standaardkleuren of aangepaste toepassingseigenschappen bevat, kan een maximale grootte van 64 KB hebben. Er is geen limiet voor het aantal berichten in een onderwerp gehouden, maar er is een initiaal van de totale grootte van de berichten die door een onderwerp. De grootte van dit onderwerp wordt opgegeven bij het maken, met een maximum van 5 GB. Als partitioneren is ingeschakeld, is de bovengrens hoger. Zie [Partitioned messaging entiteiten](service-bus-partitioning.md)voor meer informatie.

## <a name="how-to-receive-messages-from-a-subscription"></a>Hoe u berichten ontvangt van een abonnement

De aanbevolen wijze voor het ontvangen van berichten van een abonnement is om met een object [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) . [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) objecten kunnen werken in twee verschillende modi: [ *ReceiveAndDelete* en *PeekLock*](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

Wanneer de **ReceiveAndDelete** -modus met ontvangt is een foto-bewerking. dat wil zeggen Service Bus ontvangt een leesbevestiging voor een bericht in een abonnement, deze markeert u het bericht worden gebruikt als naar de toepassing wordt geretourneerd. **ReceiveAndDelete** -modus is de eenvoudigste model en werkt het best voor scenario's waarin een toepassing mogelijk zonder niet verwerken van een bericht in het geval van een fout. Als u wilt weten over dit, kunt u een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze wordt verwerkt. Omdat Service Bus kan het bericht heeft gemarkeerd als verbruikt, wanneer de toepassing opnieuw is opgestart en begint berichten opnieuw verbruikt, wordt heb dit het bericht verbruikte vóór het vastlopen gemist.

In de modus **PeekLock** (dit is de standaardmodus), het proces ontvangen verandert in een bewerking van twee fasen, die het mogelijk maakt naar ondersteuningstoepassingen die niet kunnen zonder ontbrekende berichten. Als Service Bus een aanvraag ontvangt, deze vindt u het volgende bericht worden verbruikt, vergrendelen om te voorkomen dat andere consumenten ontvangen en vervolgens naar de toepassing wordt geretourneerd. Nadat de toepassing verwerking van het bericht (of betrouwbaar voor toekomstige verwerking opgeslagen), is de tweede fase van het proces ontvangen door de ondersteuning voor [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) op het ontvangen bericht voltooid. Wanneer Service Bus de **voltooid** bellen, markeert u het bericht worden gebruikt en wordt deze verwijderd van het abonnement.

Het volgende voorbeeld wordt gedemonstreerd hoe berichten kunnen worden ontvangen en verwerkt met behulp van de standaardmodus **PeekLock** . Als u een andere [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) -waarde, kunt u een andere overbelasting voor [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.createfromconnectionstring.aspx). Dit voorbeeld wordt de terugbellen [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) proces berichten wanneer deze in het **HighMessages** -abonnement binnenkomen.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

In dit voorbeeld wordt de [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) terugbellen met behulp van een object [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx) geconfigureerd. [Automatisch aanvullen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) is ingesteld op **Onwaar** handmatig beheer van het bellen van [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) op het ontvangen bericht op inschakelen. [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) is ingesteld op 1 minuut, waardoor de client moet wachten om door een minuut duren voordat het beëindigen van de functie voor automatisch verlengen en maakt een nieuw gesprek wilt controleren op berichten van de client. Waarde van deze eigenschap Hiermee reduceert u het aantal keren maakt de client factureerbare oproepen die berichten kunnen niet worden opgehaald.

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Hoe u omgaat met toepassing loopt en gelezen berichten

Service Bus biedt een functie waarmee u fouten in uw toepassing of problemen verwerken van een bericht zonder problemen herstellen. Als een toepassing voor de ontvangst kan niet worden verwerkt van het bericht om een bepaalde reden is, kan deze de methode [verlaten](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) aanroepen voor het ontvangen bericht (in plaats van de methode [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) ). Hierdoor Service Bus u kunt het bericht binnen het abonnement te ontgrendelen en deze beschikbaar zijn voor het opnieuw worden ontvangen door de dezelfde in beslag nemen toepassing of door een andere in beslag nemen toepassing.

Er is ook een time-out die is gekoppeld aan een bericht vergrendeld binnen het abonnement en als de toepassing mislukt verwerkingstijd van het bericht voordat u de time-out vergrendelen verloopt (bijvoorbeeld als de toepassing loopt vast), en vervolgens op de Service wordt het bericht automatisch ontgrendeld en voor het opnieuw worden ontvangen beschikbaar stelt.

In het geval dat de toepassing loopt vast na het verwerken van het bericht, maar voordat de aanvraag [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) is uitgegeven, wordt het bericht worden geleverd met de toepassing opnieuw wordt gestart. Dit wordt vaak genoemd *tenminste eenmaal processing*; dat wil zeggen elk bericht ten minste eenmaal worden verwerkt, maar in sommige gevallen hetzelfde bericht kan worden geleverd. Als u het scenario kan geen dubbele verwerking zonder, kunnen softwareontwikkelaars moeten extra logica toevoegen aan de toepassing u omgaat met dubbele bericht is bezorgd. Dit is vaak bereikt met de eigenschap [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) van het bericht ongewijzigd over bezorging pogingen.

## <a name="delete-topics-and-subscriptions"></a>Onderwerpen en abonnementen verwijderen

Het volgende voorbeeld ziet u hoe u het onderwerp **TestTopic** uit de naamruimte van de service **HowToSample** verwijdert.

```
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

Abonnementen die zijn geregistreerd met het onderwerp een onderwerp verwijderen ook worden verwijderd. Abonnementen kunnen ook afzonderlijk worden verwijderd. De volgende code ziet hoe u een abonnement met **HighMessages** de naam van het onderwerp **TestTopic** verwijdert.

```
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Service Bus onderwerpen en abonnementen hebt geleerd, volgt u deze koppelingen voor meer informatie.

-   [Wachtrijen, onderwerpen en abonnementen][].
-   [Onderwerp filters steekproef][]
-   API-Naslaggids voor [SqlFilter][].
-   Maken van een werken-toepassing die u verzendt en ontvangt berichten van en naar een Service Bus wachtrij: [Service Bus brokered SMS .NET zelfstudie][].
-   Voorbeelden van de service Bus: Download van [Azure voorbeelden][] of [Overzicht](service-bus-samples.md).

  [Azure-portal]: https://portal.azure.com

  [7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

  [Wachtrijen, onderwerpen en abonnementen]: service-bus-queues-topics-subscriptions.md
  [Onderwerp filters steekproef]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Service Bus brokered SMS .NET zelfstudie]: service-bus-brokered-tutorial-dotnet.md
  [Voorbeelden van Azure]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
