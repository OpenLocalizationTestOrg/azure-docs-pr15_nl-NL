<properties 
    pageTitle="AMQP 1.0-ondersteuning Service partitioneren wachtrijen en onderwerpen | Microsoft Azure" 
    description="Meer informatie over het gebruik van de geavanceerde bericht Queuing-Protocol (AMQP) 1.0 met Service Bus partitioneren wachtrijen en onderwerpen." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="hillaryc" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="hillaryc;sethm"/>

# <a name="amqp-10-support-for-service-bus-partitioned-queues-and-topics"></a>AMQP 1.0-ondersteuning Service partitioneren wachtrijen en onderwerpen 

Azure-Service Bus ondersteunt nu het geavanceerde bericht Queuing-Protocol (**AMQP**) 1.0 voor Service Bus **partitioneren wachtrijen en onderwerpen.**

**AMQP** is een open-standaard bericht wachtrij plaatsen protocol waarmee u kunt meerdere platforms ontwikkelen met behulp van verschillende talen. Voor algemene informatie over AMQP ondersteuning in Service Bus: Zie [AMQP 1.0 ondersteuning in Service Bus](service-bus-amqp-overview.md).

**Partitioned wachtrijen en onderwerpen**, ook bekend als *gepartitioneerde entiteiten*bieden hogere beschikbaarheid, betrouwbaarheid en doorvoersnelheid dan gebruikelijke niet-partities wachtrijen en onderwerpen. Zie voor meer informatie over gepartitioneerde entiteiten [Gepartitioneerde Messaging entiteiten](service-bus-partitioning.md).

De toevoeging van AMQP 1.0 als protocol voor communicatie met gepartitioneerde wachtrijen en onderwerpen kunt u interoperabele toepassingen die u kunnen profiteren van de betere beschikbaarheid betrouwbaarheid, en de overal in aangeboden door Service Bus partitioneren entiteiten maken.

Zie de [AMQP 1,0 in Azure Service en gebeurtenis Hubs protocol handleiding](service-bus-amqp-protocol-guide.md)voor een gedetailleerde kabel niveau AMQP 1.0 protocol handleiding, waarin wordt uitgelegd hoe Service wordt geïmplementeerd en is gebaseerd op de OASIS AMQP technische specificatie.    

## <a name="use-amqp-with-partitioned-queues"></a>Gebruik AMQP met gepartitioneerde wachtrijen

Wachtrijen zijn handig voor scenario's waarvoor tijdelijke ontkoppeling, laden effenen taakverdeling en losse koppeling. Met een wachtrij, uitgevers berichten naar de wachtrij verzenden en ontvangen van consumenten uit de wachtrij in situaties waarin een bericht kan alleen worden ontvangen eenmaal. Een goede voorbeeld hiervan is een voorraadsysteem waarin verkooppunt overstapstations gegevens naar een wachtrij in plaats van rechtstreeks naar het voorraad management system publiceren. Het voorraad management system verbruikt vervolgens de gegevens op elk gewenst moment voor het beheren van de onderdelen aanvullen. Dit heeft diverse voordelen, waaronder het voorraad management systeem dat u niet hoeft te allen tijde online zijn. Zie voor meer informatie over de Service Bus wachtrijen [maken toepassingen die Service Bus wachtrijen gebruiken](service-bus-create-queues.md). 

Een gepartitioneerde wachtrij verder vergroot de beschikbaarheid van, en doorvoer voor toepassingen, zoals deze wachtrijen zijn partities op meerdere bericht makelaars en SMS winkels.     

### <a name="create-partitioned-queues"></a>Gepartitioneerde wachtrijen maken

U kunt een gepartitioneerde wachtrij gebruik van de [Azure klassieke portal][] en de Service Bus SDK maken. Als u een gepartitioneerde wachtrij, stelt u de eigenschap [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) op **waar** in het exemplaar [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx) . De volgende code ziet hoe u een gepartitioneerde wachtrij met de Service Bus SDK maken. 
 
```
// Create partitioned queue
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var queueDescription = new QueueDescription("myQueue");
queueDescription.EnablePartitioning = true;
nm.CreateQueue(queueDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Berichten met AMQP verzenden en ontvangen

U kunt berichten te verzenden en ontvangen van berichten uit een gepartitioneerde wachtrij AMQP gebruiken als protocol, met de eigenschap [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) naar [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx) zoals wordt weergegeven in de volgende code.  

```
// Sending and receiving messages to and from a queue
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
var queueClient = QueueClient.CreateFromConnectionString(amqpConnectionString, "myQueue");

BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
queueClient.Send(message);

var receivedMessage = queueClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="use-amqp-with-partitioned-topics"></a>Gebruik AMQP met gepartitioneerde onderwerpen

Onderwerpen zijn gelijksoortige naar wachtrijen, maar onderwerpen een kopie van hetzelfde bericht doorsturen naar meerdere *abonnementen*. Uitgevers berichten verzenden naar het onderwerp in een onderwerp en consumenten berichten ontvangt van abonnementen. In de voorraad systeem verkooppunt scenario publiceren overstapstations gegevens naar het onderwerp. Berichten ontvangt het voorraad management system vervolgens van een abonnement. Bovendien kan een systeem van toezicht hetzelfde bericht ontvangen uit een ander abonnement. Zie voor meer informatie over het Service Bus onderwerpen en abonnementen [maken toepassingen die gebruikmaken van Service Bus onderwerpen en abonnementen](service-bus-create-topics-subscriptions.md). 

Als u met de wachtrijen, vergroot verder gepartitioneerde onderwerpen de beschikbaarheid, betrouwbaarheid en doorvoer voor toepassingen, zoals deze onderwerpen en hun abonnementen zijn partities op meerdere bericht makelaars en SMS winkels. 

### <a name="create-partitioned-topics"></a>Gepartitioneerde onderwerpen maken

U kunt een gepartitioneerde onderwerp met behulp van de [Azure klassieke portal][] en de Service Bus SDK maken. Als u een gepartitioneerde onderwerp, stelt u de eigenschap [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx) op **waar** in het exemplaar [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) . De volgende code ziet u hoe u een gepartitioneerde onderwerp met de Service Bus SDK maakt.
    
```
// Create partitioned topic
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var topicDescription = new TopicDescription("myTopic");
topicDescription.EnablePartitioning = true;
nm.CreateTopic(topicDescription);

var subscriptionDescription = new SubscriptionDescription("myTopic", "mySubscription");
nm.CreateSubscription(subscriptionDescription);
```

### <a name="send-and-receive-messages-using-amqp"></a>Berichten met AMQP verzenden en ontvangen

U kunt berichten te verzenden en ontvangen van berichten van een gepartitioneerde onderwerp abonnement AMQP gebruiken als een protocol, met de eigenschap [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) naar [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx), zoals wordt weergegeven in de volgende code.  

```
// Sending and receiving messages to a topic and from a subscription
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
    
var topicClient = TopicClient.CreateFromConnectionString(amqpConnectionString, "myTopic");
BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
topicClient.Send(message);
    
var subcriptionClient = SubscriptionClient.CreateFromConnectionString(amqpConnectionString, "myTopic", "mySubscription");
var receivedMessage = subcriptionClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende aanvullende informatie voor meer informatie over gepartitioneerde SMS entiteiten, evenals AMQP.

*    [Gepartitioneerde SMS entiteiten](service-bus-partitioning.md)
*    [OASIS geavanceerde Message Queuing-Protocol (AMQP) versie 1.0](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)
*    [AMQP 1.0-ondersteuning in Service Bus](service-bus-amqp-overview.md)
*    [AMQP 1.0 in Azure Service Bus en gebeurtenis Hubs protocol-handleiding](service-bus-amqp-protocol-guide.md)
*    [Het gebruik van de Java bericht Service (JMS) API met Service Bus en AMQP 1.0](service-bus-java-how-to-use-jms-api-amqp.md)
*    [Hoe u AMQP 1.0 met de Service Bus .NET-API](service-bus-dotnet-advanced-message-queuing.md)

[Azure klassieke portal]: http://manage.windowsazure.com
