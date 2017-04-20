<properties 
    pageTitle="Partitioneren wachtrijen en onderwerpen | Microsoft Azure"
    description="Wordt beschreven hoe Service Bus wachtrijen en onderwerpen partitioneren met behulp van meerdere bericht makelaars."
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
    ms.date="09/02/2016"
    ms.author="sethm;hillaryc" />

# <a name="partitioned-queues-and-topics"></a>Gepartitioneerde wachtrijen en onderwerpen

Azure-Service Bus maakt gebruik van meerdere bericht makelaars om berichten te verwerken en meerdere SMS winkels voor het opslaan van berichten. Een gebruikelijke wachtrij of onderwerp is verwerkt door een enkel bericht makelaar en die zijn opgeslagen in een SMS-berichten store. Service Bus kunt ook wachtrijen of onderwerpen kunt partitioneren op meerdere bericht makelaars en SMS winkels. Dit betekent dat de algehele doorvoer van een gepartitioneerde wachtrij of onderwerp niet meer wordt beperkt door de prestaties van een enkel bericht makelaar of SMS-winkel. Daarnaast weer een tijdelijke storing van een SMS-archief niet een gepartitioneerde wachtrij of onderwerp niet beschikbaar. Gepartitioneerde wachtrijen en onderwerpen kunnen alle geavanceerde functies van de Service Bus, zoals ondersteuning voor transacties en sessies bevatten.

Zie het onderwerp van de [Service-busarchitectuur][] voor meer informatie over de Service Bus interne.

## <a name="how-it-works"></a>Werkwijze

Elke gepartitioneerde wachtrij of onderwerp bestaat uit meerdere fragmenten. Elke fragment is opgeslagen in een andere SMS winkel en verwerkt door een ander bericht makelaar. Wanneer een bericht wordt verzonden naar een gepartitioneerde wachtrij of onderwerp, Service Bus wordt toegewezen van het bericht naar een van de fragmenten. De selectie klaar is willekeurig Service Bus of via een partitiesleutel die de afzender kunt opgeven.

Wanneer een client wil ontvangen een bericht van een gepartitioneerde wachtrij, of van een abonnement van een gepartitioneerde onderwerp, Service Bus query's alle fragmenten voor berichten, wordt als resultaat het eerste bericht dat wordt opgehaald uit een van de SMS-berichten archieven naar de ontvanger. Service Bus cache de andere berichten en retourneert deze wanneer deze ontvangt extra ontvangt aanvragen. Een ontvangst-client is niet op de hoogte van de partities; het gedrag van de client-omlaag van een gepartitioneerde wachtrij of onderwerp (bijvoorbeeld lezen, voltooien, uitstellen, onbestelbare, veelgevraagde) is gelijk aan het gedrag van een gewone entiteit.

Er is geen extra kosten wanneer een bericht te verzenden of ontvangen van een bericht van een gepartitioneerde wachtrij of onderwerp.

## <a name="enable-partitioning"></a>Partitioneren inschakelen

Als u gepartitioneerde wachtrijen en onderwerpen met Azure Service Bus, gebruik van de Azure SDK versie 2.2 of hoger of opgeven `api-version=2013-10` in uw HTTP aanvraagt.

U kunt Service Bus wachtrijen en onderwerpen maken in 1, 2, 3, 4 of 5 GB formaten (de standaardinstelling is 1 GB). Met partitioneren ingeschakeld, wordt Service Bus 16 partities gemaakt voor elke GB die u opgeeft. Als zodanig als u een wachtrij die is 5 GB groot maakt, is door de 16 partities de maximale grootte (5 \* 16) = 80 GB. U kunt de maximale grootte van uw gepartitioneerde wachtrij of onderwerp weergeven door bekijken via de vermelding van de [Azure-portal][].

Er zijn verschillende manieren om een gepartitioneerde wachtrij of onderwerp te maken. Wanneer u de wachtrij of het onderwerp van uw toepassing maakt, kunt u inschakelen voor de wachtrij of onderwerp partitioneren door respectievelijk de eigenschap [QueueDescription.EnablePartitioning][] of [TopicDescription.EnablePartitioning][] op **true**. Deze eigenschappen moeten zijn ingesteld op het moment de wachtrij of onderwerp is gemaakt. Het is niet mogelijk om te wijzigen van deze eigenschappen op een bestaande wachtrij of onderwerp. Bijvoorbeeld:

```
// Create partitioned topic
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

U kunt ook een gepartitioneerde wachtrij of onderwerp maken in Visual Studio of in de [portal van Azure][]. Wanneer u een nieuwe wachtrij of onderwerp in de portal maakt, stelt u de optie **Inschakelen partitioneren** in het blad **algemene instellingen** van de wachtrij of onderwerp **Instellingen** -venster op **true**. Klik op het selectievakje **Partitioneren inschakelen** in het dialoogvenster **Nieuwe wachtrij** of **Nieuw onderwerp** in Visual Studio.

## <a name="use-of-partition-keys"></a>Gebruik van partitiesleutels

Wanneer een bericht in wachtrij naar een gepartitioneerde wachtrij of onderwerp is, Service Bus Hiermee wordt gecontroleerd op de aanwezigheid van een partitiesleutel. Als er een, selecteren het fragment op basis van die toets. Als een partitiesleutel niet wordt gevonden, selecteren het fragment op basis van een interne algoritme.

### <a name="using-a-partition-key"></a>Door een partition te gebruiken

Sommige scenario's, zoals sessies of transacties, vereisen berichten moeten worden opgeslagen in een specifieke fragment. Al deze scenario's is het gebruik van een partitiesleutel vereist. Alle berichten die dezelfde partitiesleutel gebruiken zijn toegewezen aan het dezelfde fragment. Als het fragment tijdelijk niet beschikbaar is is, retourneert Service Bus een fout.

Afhankelijk van het scenario worden de eigenschappen van een ander bericht als een partitiesleutel gebruikt:

**Sessie-id**: als een bericht is de eigenschap [BrokeredMessage.SessionId][] is ingesteld, wordt de Service Bus deze eigenschap wordt gebruikt als de partitiesleutel. Op deze manier worden alle berichten die deel uitmaakt van dezelfde sessie verwerkt door de hetzelfde bericht makelaar. Hiermee worden Service Bus te garanderen bericht ordening evenals de consistentie van de status van sessies.

**PartitionKey**: als een bericht is de eigenschap [BrokeredMessage.PartitionKey][] , maar niet de [BrokeredMessage.SessionId][] -eigenschap die zijn ingesteld, wordt de Service Bus de eigenschap [PartitionKey][] als de partitiesleutel gebruikt. Als het bericht zowel de [sessie-id][] en de [PartitionKey][] eigenschappen is ingesteld heeft, zijn beide eigenschappen identiek. Als de eigenschap [PartitionKey][] is ingesteld op een andere waarde dan de eigenschap [sessie-id][] , Service Bus een uitzondering **InvalidOperationException** n/b als resultaat. De eigenschap [PartitionKey][] moet worden gebruikt als een afzender niet-sessie hoogte transacties berichten stuurt. De toets partition zorgt ervoor dat alle berichten die binnen een transactie worden verzonden door de dezelfde SMS makelaar worden afgehandeld.

**MessageId**: als de wachtrij of onderwerp de eigenschap [QueueDescription.RequiresDuplicateDetection][] is ingesteld op **waar heeft** en de eigenschappen [BrokeredMessage.SessionId][] of [BrokeredMessage.PartitionKey][] niet is ingesteld, wordt de eigenschap [BrokeredMessage.MessageId][] als de partitiesleutel fungeert. (Houd er rekening mee dat de bibliotheken Microsoft .NET en AMQP een bericht-ID automatisch toewijzen als de verzendende toepassing niet.) In dit geval worden alle exemplaren van hetzelfde bericht verwerkt door de hetzelfde bericht makelaar. Hierdoor Service Bus detecteren en verwijderen van dubbele berichten. Als de eigenschap [QueueDescription.RequiresDuplicateDetection][] is niet ingesteld op **waar**, rekening Service Bus geen met de eigenschap [MessageId][] als partitiesleutel.

### <a name="not-using-a-partition-key"></a>Geen gebruikmaakt van een partitiesleutel

Service Bus distribueert bij afwezigheid van een partitiesleutel, berichten op een wijze round robin op alle fragmenten van gepartitioneerde wachtrij of onderwerp. Als het door u gekozen fragment niet beschikbaar is, wordt het bericht door Service Bus toegewezen aan een ander fragment. Op deze manier slaagt de verzendbewerking ondanks de tijdelijk niet beschikbaar een SMS-berichten store.

Om te geven van de Service Bus moet voldoende tijd in de wachtrij plaatsen het bericht naar een ander fragment, de [MessagingFactorySettings.OperationTimeout][] -waarde die is opgegeven door de client die het bericht groter dan 15 seconden. Het wordt aanbevolen dat u de eigenschap [OperationTimeout][] ingesteld op de standaardwaarde van 60 seconden.

Houd er rekening mee dat een partitiesleutel "" een bericht naar een specifieke fragment kledingwinkelketen. Als de SMS-berichten store waarin dit fragment niet beschikbaar is, retourneert Service Bus een fout. In de afwezigheid van een partitiesleutel, Service Bus een verschillende fragment kunt kiezen en de bewerking is geslaagd. Het wordt daarom aanbevolen dat u geen een partitiesleutel opgeeft tenzij dit vereist is.

## <a name="advanced-topics-use-transactions-with-partitioned-entities"></a>Geavanceerde onderwerpen: transacties met gepartitioneerde entiteiten gebruiken

Berichten die worden verzonden als onderdeel van een transactie moeten een partitiesleutel opgeven. Dit is een van de volgende eigenschappen: [BrokeredMessage.SessionId][], [BrokeredMessage.PartitionKey][]of [BrokeredMessage.MessageId][]. Alle berichten die worden verzonden als onderdeel van dezelfde transactie moeten dezelfde partitiesleutel opgeven. Als u probeert een bericht zonder partitiesleutel binnen een transactie verzenden, Service Bus een uitzondering **InvalidOperationException** n/b als resultaat. Als u probeert te verzenden van meerdere berichten binnen dezelfde transactie met verschillende partition toetsen, Service Bus een uitzondering **InvalidOperationException** n/b als resultaat. Bijvoorbeeld:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.PartitionKey = "myPartitionKey";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

Als een van de eigenschappen die als een partitiesleutel fungeren zijn ingesteld, kledingwinkelketen Service Bus het bericht naar een specifieke fragment. Dit probleem doet zich al dan niet een transactie wordt gebruikt. Het wordt aanbevolen dat u geen een partitiesleutel opgeeft als het niet nodig is.

## <a name="using-sessions-with-partitioned-entities"></a>Sessies met gepartitioneerde entiteiten gebruiken

Als u wilt een transacties bericht verzenden naar een sessie-hoogte onderwerp of de wachtrij, moet het bericht de eigenschap [BrokeredMessage.SessionId][] is ingesteld. Als de eigenschap [BrokeredMessage.PartitionKey][] ook is opgegeven, is deze moet gelijk aan de [sessie-id][] -eigenschap. Als ze verschillen, geeft Service Bus een uitzondering **InvalidOperationException** n/b als resultaat.

In tegenstelling tot de normale (niet-partities) wachtrijen of onderwerpen is het niet mogelijk is met een enkele transactie meerdere berichten verzenden naar verschillende sessies. Als geprobeerd is, retourneert Service Bus een uitzondering **InvalidOperationException **. Bijvoorbeeld:

```
CommittableTransaction committableTransaction = new CommittableTransaction();
using (TransactionScope ts = new TransactionScope(committableTransaction))
{
    BrokeredMessage msg = new BrokeredMessage("This is a message");
    msg.SessionId = "mySession";
    messageSender.Send(msg); 
    ts.Complete();
}
committableTransaction.Commit();
```

## <a name="automatic-message-forwarding-with-partitioned-entities"></a>Automatische bericht doorsturen met partities entiteiten

Azure-Service Bus ondersteunt automatische bericht doorsturen van, aan of tussen gepartitioneerde entiteiten. Als u wilt inschakelen automatisch doorsturen van berichten, stelt u de eigenschap [QueueDescription.ForwardTo][] op de bronwachtrij of -abonnement. Als het bericht bevat een partitiesleutel ([sessie-id][], [PartitionKey][]of [MessageId][]), wordt die toets partition wordt gebruikt voor de bestemming entiteit.

## <a name="considerations-and-guidelines"></a>Overwegingen en richtlijnen

- **Hoge consistentie functies**: als een entiteit functies zoals sessies, dubbele detectie of expliciete controle over partitioneren sleutel worden gebruikt, zal de SMS-bewerkingen altijd worden doorgestuurd naar specifieke fragmenten. Als een van de fragmenten hoog verkeer ondervindt of de onderliggende store beschadigd is, wordt deze bewerkingen mislukken en beschikbaarheid wordt beperkt. Algemene, de consistentie is nog steeds veel hoger dan niet-partities entiteiten; alleen een subset van verkeer is met problemen, in tegenstelling tot alle verkeer te maken.
- **Management**: bewerkingen zoals maken, bijwerken en verwijderen moeten worden uitgevoerd op alle fragmenten van de entiteit. Als een fragment beschadigd is kan dit resulteren in fouten voor deze bewerkingen. Voor de bewerking Get moet informatie, zoals bericht telt worden samengevoegd uit alle fragmenten. Als een fragment beschadigd is, wordt de status van de beschikbaarheid van entiteit wordt vermeld als beperkt.
- **Lage volume bericht scenario's**: voor deze scenario's, met name wanneer het HTTP-protocol, u mogelijk moet u meerdere bewerkingen voor het verkrijgen van alle berichten ontvangen. Voor ontvangstaanvragen voor front-end van de alle fragmenten uitvoeren met een ontvangen en in de cache opgeslagen alle antwoorden hebt ontvangen. Een aanvraag latere ontvangen op de verbinding wilt profiteren van deze caching en ontvangen vertragingstijden, lager zal zijn. Echter als u meerdere verbindingen hebt of HTTP, die een nieuwe verbinding maakt voor elk verzoek. Er is dus niet zeggen dat deze op hetzelfde knooppunt zou terechtkomt. Als alle bestaande berichten worden vergrendeld en in cache in een andere front-end opgeslagen, retourneert de ontvangstbewerking **null**. Berichten uiteindelijk verlopen en u deze opnieuw kunt ontvangen. Permanente HTTP wordt aanbevolen.
- **Bladeren/korte weergave berichten**: PeekBatch het aantal berichten die zijn opgegeven in de [eigenschap MessageCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.messagecount.aspx)niet altijd terugkeren. Zijn er twee veelvoorkomende oorzaken hiervoor. Een reden is dat de geaggregeerde grootte van de verzameling berichten klikt de maximale grootte van 256KB overschrijdt. Een andere reden dan ook is dat als de wachtrij of onderwerp de [eigenschap EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) ingesteld op **waar bevat**, een partition mogelijk niet voldoende berichten naar het gewenste aantal berichten te voltooien. In het algemeen, als een toepassing wil ontvangen van een bepaald aantal berichten, moet deze aanroepen [PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) herhaaldelijk drukken totdat deze dat aantal berichten ontvangt of er zijn geen berichten meer om te kijken. Zie voor meer informatie, inclusief voorbeelden van de code, [QueueClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peekbatch.aspx) of [SubscriptionClient.PeekBatch](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.peekbatch.aspx).

## <a name="latest-added-features"></a>Nieuwste extra functies

- Toevoegen of verwijderen regel nu met gepartitioneerde entiteiten wordt ondersteund. Verschillende van niet-partities entiteiten, deze bewerkingen worden niet ondersteund onder transacties. 
- AMQP wordt nu ondersteund voor verzenden en ontvangen van berichten van en van een gepartitioneerde entiteit.
- AMQP wordt nu ondersteund voor de volgende bewerkingen: [Batch verzenden](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.sendbatch.aspx), [Batch ontvangen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.receivebatch.aspx), [ontvangen door volgnummer](https://msdn.microsoft.com/library/azure/hh330765.aspx), [korte weergave](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.peek.aspx), [Verlengen vergrendelen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.renewmessagelock.aspx), [Planning bericht](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.schedulemessageasync.aspx), [Gepland bericht annuleren](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.cancelscheduledmessageasync.aspx), [Regel toevoegen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Regel verwijderen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.ruledescription.aspx), [Sessie verlengen vergrendelen](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.renewlock.aspx), [Set Session State](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx), [Get Session State](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx), [Korte weergave sessie berichten](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.peek.aspx)en [Sessies opsommen](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.getmessagesessionsasync.aspx).

## <a name="partitioned-entities-limitations"></a>Gepartitioneerde entiteiten beperkingen

Momenteel Service Bus gelden de volgende beperkingen van gepartitioneerde wachtrijen en onderwerpen:

-   Gepartitioneerde wachtrijen en onderwerpen bieden geen ondersteuning voor het verzenden van berichten die deel uitmaakt van verschillende sessies in één transactie.
-   Service Bus kunnen momenteel maximaal 100 gepartitioneerde wachtrijen of onderwerpen per naamruimte. Elke gepartitioneerde wachtrij of onderwerp telt richting van het quotum van 10.000 entiteiten per naamruimte (geldt niet voor Premium laag).

## <a name="next-steps"></a>Volgende stappen

Zie het onderwerp van het [AMQP 1.0-ondersteuning Service partitioneren wachtrijen en onderwerpen][] voor meer informatie over SMS entiteiten partitioneren. 

  [Service-busarchitectuur]: service-bus-architecture.md
  [Azure-portal]: https://portal.azure.com
  [QueueDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [TopicDescription.EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx
  [BrokeredMessage.SessionId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [BrokeredMessage.PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [Sessie-id]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx
  [PartitionKey]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.partitionkey.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [QueueDescription.RequiresDuplicateDetection]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.requiresduplicatedetection.aspx
  [MessagingFactorySettings.OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
  [QueueDescription.ForwardTo]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.forwardto.aspx
  [AMQP 1.0-ondersteuning Service partitioneren wachtrijen en onderwerpen]: service-bus-partitioned-queues-and-topics-amqp-overview.md
