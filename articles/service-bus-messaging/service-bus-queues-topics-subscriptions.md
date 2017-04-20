<properties 
    pageTitle="Service Bus wachtrijen, onderwerpen en abonnementen | Microsoft Azure"
    description="Overzicht van de Service Bus entiteiten messaging."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/14/2016"
    ms.author="sethm" />

# <a name="service-bus-queues-topics-and-subscriptions"></a>Service Bus wachtrijen, onderwerpen en abonnementen

Microsoft Azure-Service Bus ondersteunt een set cloudgebaseerde, bericht-georiënteerd-middleware technologieën inclusief betrouwbare wachtrijen en duurzame publiceren/abonneren messaging. Deze functies voor brokered SMS-berichten kunnen worden beschouwd als losgekoppelde messaging functies die ondersteuning publicatie-abonnementen, tijdelijke ontkoppeling en scenario's met de Service-Bus stof messaging van taakverdeling. Losgekoppelde communicatie heeft vele voordelen; clients en servers kunnen bijvoorbeeld verbinding maken met zo nodig en hun bewerkingen uitvoeren op asynchrone wijze.

De SMS-berichten entiteiten die de kerninhoud van de mogelijkheden voor brokered SMS formulier in de Service Bus zijn wachtrijen, onderwerpen/abonnementen en regels/acties.

## <a name="queues"></a>Wachtrijen

Wachtrijen bieden eerste keer hebt aangemeld, berichtbezorging eerste Out (FIFO) naar een of meer concurrerende consumenten. Dat wil zeggen berichten meestal naar verwachting worden ontvangen en verwerkt door de ontvangers in de volgorde waarin ze zijn toegevoegd aan de wachtrij en elk bericht is ontvangen en verwerkt door consumenten slechts één bericht. Een belangrijk voordeel van het gebruik van wachtrijen is om te bereiken "tijdelijke ontkoppeling" van toepassingsonderdelen. Met andere woorden, hoeft de producenten (afzenders) en consumenten (ontvangers) niet te worden berichten verzenden en ontvangen op hetzelfde moment, omdat de berichten worden blijvend opgeslagen in de wachtrij. Bovendien bevat de producent geen moet wachten om een antwoord van de consument om verder te kunnen verwerken en berichten verzenden.

Een gerelateerde voordeel is 'laden effenen,"waarmee producenten en op consumenten verzenden en ontvangen van berichten op verschillende tarieven. In veel toepassingen varieert het systeem selectievakje laden na verloop van tijd; de verwerkingstijd die is vereist voor elk exemplaar van werk is echter meestal constante. Intermediating bericht producenten en consumenten met een wachtrij betekent dit dat de in beslag nemen toepassing alleen als u wilt kunnen worden afgehandeld gemiddelde laden in plaats van piek laden in te richten. De diepte van de wachtrij in omvang groeit en opdrachten terwijl de binnenkomende laden varieert. Hiermee slaat u rechtstreeks geld met betrekking tot de hoeveelheid infrastructuur die nodig zijn voor het laden van de toepassing. Als de belasting toeneemt, kunnen meer werknemer-processen worden toegevoegd aan in de wachtrij lezen. Elk bericht wordt verwerkt door één van de werknemer-processen. Bovendien kunt deze halen gebaseerde taakverdeling optimaal gebruik van de werknemer-computers zelfs als de werknemer-computers verschillen op het gebied power verwerking, terwijl ze worden berichten op hun eigen maximumsnelheid halen. Dit patroon wordt vaak het patroon 'dat consumenten' genoemd.

Tussenliggende tussen bericht producenten en consumenten wachtrijen beschikt u over een inherent losse koppeling tussen de onderdelen. Omdat producenten en consumenten niet op de hoogte van elkaar zijn, kan een consument zonder gevolgen voor de producent worden bijgewerkt.

Maken van een wachtrij is een met meerdere stappen. U bewerkingen management voor Service Bus messaging entiteiten (wachtrijen en onderwerpen) via de klasse [Microsoft.ServiceBus.NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) , die wordt samengesteld door het opgeven van het basisadres van de naamruimte Service Bus en de referenties van de gebruiker. [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) biedt methoden voor het maken, opsommen en SMS entiteiten verwijderen. Na het maken van een object [Microsoft.ServiceBus.TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) uit de naam van de SA's en -toets en een service naamruimte management object, kunt u de methode [Microsoft.ServiceBus.NamespaceManager.CreateQueue](https://msdn.microsoft.com/library/azure/hh293157.aspx) om de wachtrij te maken. Bijvoorbeeld:

```
// Create management credentials
TokenProvider credentials = TokenProvider. CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
// Create namespace client
NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials);
```

U kunt vervolgens een wachtrij-object en een SMS-berichten factory maken met de Service Bus URI als een argument. Bijvoorbeeld:

```
QueueDescription myQueue;
myQueue = namespaceClient.CreateQueue("TestQueue");
MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", ServiceNamespace, string.Empty), credentials); 
QueueClient myQueueClient = factory.CreateQueueClient("TestQueue");
```

Vervolgens kunt u berichten verzenden naar de wachtrij. Als er een lijst met brokered berichten genoemd bijvoorbeeld `MessageList`, de code lijkt op de volgende handelingen uit:

```
for (int count = 0; count < 6; count++)
{
    var issue = MessageList[count];
    issue.Label = issue.Properties["IssueTitle"].ToString();
    myQueueClient.Send(issue);
}
```

U kunt vervolgens berichten ontvangt van de wachtrij als volgt:

```
while ((message = myQueueClient.Receive(new TimeSpan(hours: 0, minutes: 0, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();

        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

In de modus [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) is de ontvangstbewerking foto; dat wil zeggen als Service Bus de aanvraag ontvangt, er markeert u het bericht worden gebruikt en naar de toepassing wordt geretourneerd. [ReceiveAndDelete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) -modus is de eenvoudigste model en werkt het best voor scenario's waarin de toepassing mogelijk zonder niet verwerken van een bericht in het geval van een fout. Als u wilt weten over dit, kunt u een scenario waarin de consument ontvangen verzoek verzendt en vervolgens loopt vast voordat deze wordt verwerkt. Omdat Service Bus markeert u het bericht als wordt verbruikt, wanneer de toepassing opnieuw is opgestart en begint berichten opnieuw verbruikt, wordt heb dit het bericht verbruikte vóór het vastlopen gemist.

In de modus voor [PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) wordt de ontvangstbewerking twee fasen, waardoor de naar ondersteuningstoepassingen die niet kunnen zonder ontbrekende berichten. Als Service Bus de aanvraag ontvangt, deze vindt u het volgende bericht worden verbruikt, vergrendelen om te voorkomen dat andere consumenten ontvangen deze en klikt u vervolgens naar de toepassing wordt geretourneerd. Nadat de toepassing verwerking van het bericht (of betrouwbaar voor toekomstige verwerking opgeslagen), is de tweede fase van het proces ontvangen door de ondersteuning voor [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) op het ontvangen bericht voltooid. Wanneer Service Bus de [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) bellen, wordt het bericht als wordt verbruikt gemarkeerd.

Als de toepassing kan niet worden verwerkt van het bericht om een bepaalde reden is, kan deze de methode [verlaten](https://msdn.microsoft.com/library/azure/hh181837.aspx) aanroepen voor het ontvangen bericht (in plaats van [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx)). Hiermee worden Service Bus ontgrendelen van het bericht en deze beschikbaar moeten opnieuw worden ontvangen door de dezelfde consument of door een andere concurrerende consumenten. Tweede plaats, is er een time-out die is gekoppeld aan de vergrendeling en als de toepassing mislukt verwerkingstijd van het bericht voordat u de vergrendeling time-out verloopt (bijvoorbeeld als de toepassing loopt vast) en vervolgens Service ontgrendelt van het bericht en wordt opnieuw worden ontvangen (in feite bewerking [verlaten](https://msdn.microsoft.com/library/azure/hh181837.aspx) al dan niet standaard).

Houd er rekening mee dat in het geval dat de toepassing loopt vast na het verwerken van het bericht, maar voordat de aanvraag [voltooid](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) is uitgegeven, het bericht wordt geleverd met de toepassing opnieuw wordt gestart. Vaak heet dit *Tenminste eenmaal *verwerking; elk bericht wordt verwerkt ten minste eenmaal dat wil zeggen. In bepaalde situaties mogelijk hetzelfde bericht worden geleverd. Als u het scenario kan geen dubbele verwerking, zonder en vervolgens extra logica is vereist in de toepassing te detecteren dubbele waarden die kunnen worden bereikt op basis van de eigenschap **MessageId** van het bericht ongewijzigd over bezorging pogingen. Dit wordt genoemd *Exact eenmaal* verwerken.

Zie de [Service Bus brokered messaging .NET zelfstudie](service-bus-brokered-tutorial-dotnet.md)voor meer informatie en een voorbeeld werken van het maken en verzenden van berichten aan en uit wachtrijen.

## <a name="topics-and-subscriptions"></a>Onderwerpen en abonnementen

Geef een een-op-veel-formulier van communicatie, in een patroon *publiceren/abonneren* in tegenstelling tot wachtrijen, waarin elk bericht wordt verwerkt door een enkel consument, *onderwerpen* en *abonnementen* . Handig voor het schaalbaarheid zeer groot aantal geadresseerden, elke gepubliceerde bericht beschikbaar wordt gemaakt voor elk abonnement dat is geregistreerd bij het onderwerp. Berichten worden verzonden naar een onderwerp en afgeleverd in een of meer bijbehorende abonnementen, afhankelijk van het filterregels die kunnen worden ingesteld op basis van de per-abonnement. De abonnementen kunnen extra filters gebruiken om te beperken de berichten die ze willen ontvangen. Berichten worden verzonden naar een onderwerp op dezelfde manier deze naar een wachtrij worden verzonden, maar berichten worden niet ontvangen van het onderwerp rechtstreeks. In plaats daarvan worden ze ontvangen van een abonnement. Een abonnement onderwerp lijkt op een virtuele wachtrij kopieën van de berichten die zijn verzonden naar het onderwerp. Berichten worden ontvangen van een abonnement dezelfde manier als die ze worden ontvangen uit een wachtrij.

Ter vergelijking, de functionaliteit voor het verzenden van bericht van een wachtrij toewijzingen rechtstreeks naar een onderwerp en de bericht-ontvangst-functionaliteit wordt toegewezen aan een abonnement. Onder andere dit betekent dat abonnementen dezelfde patronen eerder is beschreven in deze sectie met betrekking tot wachtrijen ondersteunt: concurrerende consumenten, tijdelijke ontkoppeling laden effenen en taakverdeling.

Maken van een onderwerp is vergelijkbaar met het maken van een wachtrij, zoals wordt weergegeven in het voorbeeld in de vorige sectie. Maak de service-URI en gebruik vervolgens de klas [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) maken van de naamruimte-client. Vervolgens kunt u een onderwerp met de methode [CreateTopic](https://msdn.microsoft.com/library/azure/hh293080.aspx) maken. Bijvoorbeeld:

```
TopicDescription dataCollectionTopic = namespaceClient.CreateTopic("DataCollectionTopic");
```

Voeg nu abonnementen desgewenst:

```
SubscriptionDescription myAgentSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Inventory");
SubscriptionDescription myAuditSubscription = namespaceClient.CreateSubscription(myTopic.Path, "Dashboard");
```

Vervolgens kunt u een onderwerp-client. Bijvoorbeeld:

```
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider);
TopicClient myTopicClient = factory.CreateTopicClient(myTopic.Path)
```

Met de afzender van het bericht, kunt u verzenden en ontvangen van berichten van en naar het onderwerp, zoals wordt weergegeven in de vorige sectie. Bijvoorbeeld:

```
foreach (BrokeredMessage message in messageList)
{
    myTopicClient.Send(message);
    Console.WriteLine(
    string.Format("Message sent: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

Net zoals bij wachtrijen, berichten worden ontvangen van een abonnement met behulp van een [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) -object in plaats van een object [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) . Maak de client van abonnement, de naam van het onderwerp, de naam van het abonnement en (optioneel) de modus ontvangen als parameters doorgeven. Bijvoorbeeld, met de **voorraad** -abonnement:

```
// Create the subscription client
MessagingFactory factory = MessagingFactory.Create(serviceUri, tokenProvider); 

SubscriptionClient agentSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Inventory", ReceiveMode.PeekLock);
SubscriptionClient auditSubscriptionClient = factory.CreateSubscriptionClient("IssueTrackingTopic", "Dashboard", ReceiveMode.ReceiveAndDelete); 

while ((message = agentSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Inventory...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
    message.Complete();
}          

// Create a receiver using ReceiveAndDelete mode
while ((message = auditSubscriptionClient.Receive(TimeSpan.FromSeconds(5))) != null)
{
    Console.WriteLine("\nReceiving message from Dashboard...");
    Console.WriteLine(string.Format("Message received: Id = {0}, Body = {1}", message.MessageId, message.GetBody<string>()));
}
```

### <a name="rules-and-actions"></a>Regels en acties

In veel gevallen moeten de berichten die specifieke kenmerken hebben op verschillende manieren worden verwerkt. U kunt abonnementen als u wilt zoeken naar berichten die u hebt de keuze eigenschappen en voer daarna bepaalde wijzigingen aanbrengen in deze eigenschappen configureren om dit mogelijk. Terwijl de Service Bus abonnementen ziet alle berichten die naar het onderwerp, kunt u alleen een subset van deze berichten kopiëren naar de wachtrij virtuele abonnement. Dit gebeurt abonnement filters gebruiken. Wijzigingen worden *filteracties*genoemd. Wanneer u een abonnement is gemaakt, kunt u opgeven met een filterexpressie die wordt toegepast op de eigenschappen van het bericht, de Systeemeigenschappen (bijvoorbeeld **Label**) en de maatwerktoepassing eigenschappen (bijvoorbeeld **winkelnaam**.) De SQL-filterexpressie is optioneel in dit geval; zonder een filterexpressie SQL wordt niets filter gedefinieerd op een abonnement uitgevoerd op alle berichten voor dit abonnement.

Het vorige voorbeeld, filter-berichten die afkomstig zijn alleen uit **Store1**, maakt u het abonnement Dashboard als volgt:

```
namespaceManager.CreateSubscription("IssueTrackingTopic", "Dashboard", new SqlFilter("StoreName = 'Store1'"));
```

Met dit filter abonnement op hun plaats staan, alleen berichten met de `StoreName` eigenschap ingesteld op `Store1` worden gekopieerd naar de virtuele wachtrij voor de `Dashboard` abonnement.

Zie de documentatie van de klassen [SqlFilter](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx) en [SqlRuleAction](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlruleaction.aspx) voor meer informatie over mogelijke filterwaarden. Zie ook de [Brokered Messaging: geavanceerde Filters](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) en [Onderwerp Filters](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters) voorbeelden.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende onderwerpen voor meer informatie en voorbeelden van het gebruik van de Service Bus brokered SMS entiteiten geavanceerde.

- [SMS-service Bus-overzicht](service-bus-messaging-overview.md)
- [Service Bus brokered SMS .NET zelfstudie](service-bus-brokered-tutorial-dotnet.md)
- [Service Bus brokered SMS REST zelfstudie](service-bus-brokered-tutorial-rest.md)
- [Onderwerp Filters steekproef](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters)
- [Brokered Messaging: Geavanceerde Filters steekproef](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)

