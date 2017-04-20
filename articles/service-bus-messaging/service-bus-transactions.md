<properties 
    pageTitle="Service Bus transacties | Microsoft Azure" 
    description="Overzicht van Azure Service Bus atomische transacties en verzenden via" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="10/04/2016"
    ms.author="clemensv;sethm"/>

# <a name="overview-of-service-bus-transaction-processing"></a>Overzicht van de Service Bus transactieverwerking

In dit artikel wordt beschreven hoe de transactiemogelijkheden van Azure Service Bus. Veel van de discussie wordt geïllustreerd door de [Atomische transacties met Service Bus steekproef](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions). In dit artikel is beperkt tot een overzicht van de verwerking van transacties en de functie *via verzenden* in Service Bus, terwijl de steekproef atomische transacties uitgebreidere en meer complexe bereik wordt.

## <a name="transactions-in-service-bus"></a>Transacties in de Service Bus

Twee of meer bewerkingen samen in een *bereik van de uitvoering*van een [transactie](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions#what-are-transactions) groepen. Een dergelijke transactie moet ervoor zorgen dat alle bewerkingen die deel uitmaken van een bepaalde groep bewerkingen slagen of gezamenlijk mislukken aard. In dit verband fungeren transacties als een eenheid, die is vaak *atomiciteit*genoemd. 

Service is een transacties bericht makelaar en zorgt ervoor dat transacties integriteit voor alle interne bewerkingen ten opzichte van de bericht-winkels. Alle overdrachten van berichten binnen Service Bus, zoals het verplaatsen van berichten naar een [Global](service-bus-dead-letter-queues.md) of [Automatische doorsturen](service-bus-auto-forwarding.md) van berichten tussen entiteiten, zijn transacties. Als zodanig als Service Bus een bericht accepteert, is al opgeslagen en met een volgnummer het label. Daarna de overdracht van elk bericht binnen Service Bus gecoördineerde bewerkingen over entiteiten zijn en zal geen van beide leiden tot verlies (bron is geslaagd en doel mislukt) of dupliceren (gegevensbron is mislukt en doel is geslaagd) van het bericht.

Service Bus ondersteunt bewerkingen voor het groeperen op een enkele SMS entiteit (wachtrij, onderwerp, abonnement) in het bereik van een transactie. Bijvoorbeeld: u kunt verschillende berichten verzenden naar één wachtrij uit binnen een transactiebereik en de berichten worden alleen vastgelegd in van de wachtrij log wanneer de transactie met succes is voltooid.

## <a name="operations-within-a-transaction-scope"></a>Bewerkingen binnen een transactiebereik 

De bewerkingen die kunnen worden uitgevoerd binnen een transactiebereik zijn als volgt:

- ** [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx), [MessageSender](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx), [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx)**: verzenden, SendAsync, SendBatch, SendBatchAsync 

- **[BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)**: voltooid, CompleteAsync, oefening, AbandonAsync, onbestelbare, DeadletterAsync, uitstellen, DeferAsync, RenewLock, RenewLockAsync 

Ontvangen bewerkingen niet zijn opgenomen, omdat wordt aangenomen dat de toepassing berichten krijgt de [ReceiveMode.PeekLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) -modus met binnen enkele lus hebt ontvangen of met een [OnMessage](https://msdn.microsoft.com/library/azure/dn369601.aspx) terugbellen en vervolgens pas een transactiebereik voor het verwerken van het bericht opent.

Het verwijderen van het bericht (klaar is, stelt u oefening, onbestelbare, uit) wordt plaatsvindt binnen het bereik van, en afhankelijke op de algehele resultaat van de transactie.

## <a name="transfers-and-send-via"></a>Overdrachten en 'verzenden '

Als u wilt inschakelen transacties overdracht van gegevens uit een wachtrij aan een processor en vervolgens naar een andere wachtrij, ondersteunt Service Bus *overdrachten*. Een afzender eerst stuurt een bericht naar een 'doorverbinden wachtrij"een bestandsoverdracht betrekking heeft, en de wachtrij doorverbinden direct het bericht wordt verplaatst naar de opgegeven bestemmingswachtrij met de dezelfde robuuste doorverbinden uitvoering die afhankelijk zijn van het automatisch doorsturen van berichten. Het bericht wil nooit van de bestandsoverdracht wachtrij log zodanig dat deze zichtbaar is voor de overdracht van de wachtrij consumenten.

De kracht van transacties Hierdoor wordt zichtbaar wanneer de overdracht wachtrij zelf fungeert als bron voor de invoer berichten van de afzender. Met andere woorden, Service Bus kunt overbrengen het bericht naar de bestemmingswachtrij "via' de wachtrij doorverbinden tijdens het uitvoeren van een voltooid (of uitstellen, of onbestelbare) betrekking heeft op het invoerbericht, alles in één atomaire bewerking. 

### <a name="see-it-in-code"></a>Zie het in code

Als u wilt deze overdracht hebt ingesteld, kunt u de afzender van een bericht dat is bedoeld voor de bestemmingswachtrij via de wachtrij doorverbinden maken. U hebt ook een ontvanger waarmee berichten van die dezelfde wachtrij opgehaald. Bijvoorbeeld:

```
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Een eenvoudige transactie vervolgens deze elementen, zoals in het volgende voorbeeld gebruikt:

```
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over de Service Bus wachtrijen:

- [Service Bus entiteiten met doorverbinden automatisch koppelen](service-bus-auto-forwarding.md)
- [Voorbeeld van automatisch doorsturen](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AutoForward)
- [Atomische transacties met Service Bus steekproef](https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/AtomicTransactions)
- [Azure wachtrijen en Service Bus wachtrijen vergeleken](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Het gebruik van de Service Bus wachtrijen](service-bus-dotnet-get-started-with-queues.md)