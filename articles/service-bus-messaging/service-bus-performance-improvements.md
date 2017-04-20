<properties 
    pageTitle="Aanbevolen procedures voor het verbeteren van prestaties Service Bus gebruikt | Microsoft Azure"
    description="Het gebruik van Azure Service Bus optimaliseren wanneer uitwisselen brokered berichten wordt beschreven."
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
    ms.date="10/25/2016"
    ms.author="sethm" />

# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Aanbevolen procedures voor het gebruik van de Service Bus Messaging prestatieverbeteringen

In dit onderwerp wordt beschreven hoe voor gebruik met Azure Service Bus Messaging optimaliseren wanneer uitwisselen brokered berichten. Het eerste deel van dit onderwerp worden de verschillende regelingen die worden aangeboden om te helpen verbeteren. Het tweede onderdeel bevat richtlijnen over het gebruik van Service Bus op een manier die de beste prestaties in een bepaalde scenario bieden kunt.

In dit onderwerp wordt de term "client" verwijst naar een entiteit die toegang heeft tot de Service Bus. Een client, kan de rol van een afzender of een ontvanger nemen. De term "afzender" wordt gebruikt voor een Service Bus wachtrij onderwerp-client of die berichten naar een Service Bus wachtrij of onderwerp. De term "ontvanger" verwijst naar een Service Bus wachtrij abonnement-client of dat berichten van een abonnement of Service Bus wachtrij ontvangt.

Deze secties introduceren verschillende concepten die Service Bus wordt gebruikt om te helpen Verbeter de prestaties.

## <a name="protocols"></a>Protocollen

Service Bus kunnen clients verzenden en ontvangen via drie protocollen

1. Geavanceerde berichtenwachtrij-Protocol (AMQP)

2. Service Bus Messaging Protocol (SBMP)

3. HTTP

Zowel AMQP en SBMP zijn efficiënter, omdat ze voor het behoud van de verbinding met de Service Bus zolang de SMS-berichten fabriek bestaat. Ook wordt geïmplementeerd batchen en veelgevraagde. Tenzij expliciet is vermeld, is alle inhoud in dit onderwerp wordt ervan uitgegaan het gebruik van AMQP of SBMP.

## <a name="reusing-factories-and-clients"></a>Hergebruiken factory's en -clients

Service Bus client-objecten, zoals [QueueClient][] of [MessageSender][], worden via een object [MessagingFactory][] , waarmee ook interne beheer van verbindingen gemaakt. U moet SMS-factory's of wachtrij, onderwerp en Abonnementclients niet sluiten nadat u een bericht verzenden en klik vervolgens opnieuw te maken wanneer u het volgende bericht verzendt. Een SMS-berichten factory sluiten Hiermee verwijdert u de verbinding met de service-Service Bus en een nieuwe verbinding wordt gemaakt wanneer de fabriek taakvensterapp. Een verbinding tot stand is een dure bewerking die u voorkomen kunt door de dezelfde fabriek en de clientobjecten voor meerdere bewerkingen opnieuw te gebruiken. U kunt het object [QueueClient][] veilig gebruiken voor het verzenden van berichten van gelijktijdige asynchrone bewerkingen en meerdere threads. 

## <a name="concurrent-operations"></a>Gelijktijdige bewerkingen

Een bewerking (verzenden, ontvangen, verwijderen, enz.) duurt enige tijd. Ditmaal bevat de verwerking van de bewerking door de service-Service Bus naast de latentie van de aanvraag en het antwoord. Als u wilt het aantal bewerkingen per keer vergroten, moeten gelijktijdig bewerkingen uitvoeren. U kunt dit op verschillende manieren doen:

-   **Asynchrone bewerkingen**: de client plant bewerkingen door asynchrone bewerkingen te voeren. De volgende aanvraag is gestart voordat de vorige aanvraag is voltooid. Hier volgt een voorbeeld van een verzendbewerking asynchroon:

    ```
    BrokeredMessage m1 = new BrokeredMessage(body);
    BrokeredMessage m2 = new BrokeredMessage(body);
    
    Task send1 = queueClient.SendAsync(m1).ContinueWith((t) => 
      {
        Console.WriteLine("Sent message #1");
      });
    Task send2 = queueClient.SendAsync(m2).ContinueWith((t) => 
      {
        Console.WriteLine("Sent message #2");
      });
    Task.WaitAll(send1, send2);
    Console.WriteLine("All messages sent");
    ```

    Dit is een voorbeeld van een asynchrone ontvangstbewerking:
    
    ```
    Task receive1 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
    Task receive2 = queueClient.ReceiveAsync().ContinueWith(ProcessReceivedMessage);
    
    Task.WaitAll(receive1, receive2);
    Console.WriteLine("All messages received");
    
    async void ProcessReceivedMessage(Task<BrokeredMessage> t)
    {
      BrokeredMessage m = t.Result;
      Console.WriteLine("{0} received", m.Label);
      await m.CompleteAsync();
      Console.WriteLine("{0} complete", m.Label);
    }
    ```

-   **Meerdere factory's**: alle clients (afzenders naast ontvangers) die zijn gemaakt door dezelfde fabriek één TCP-verbinding delen. De maximale bericht doorvoer wordt beperkt door het aantal bewerkingen die via deze TCP-verbinding kunt gaan. De doorvoer die kan worden verkregen met dezelfde fabriek varieert enorm met TCP inclusief de tijd en de grootte van het bericht. Als u hogere doorvoersnelheden, moet u meerdere SMS factory's.

## <a name="receive-mode"></a>Modus ontvangen

Wanneer u een abonnement of wachtrij-client maakt, kunt u een modus ontvangen: *korte weergave-lock* of *ontvangen en verwijderen*. De standaardinstelling ontvangen modus is [PeekLock][]. Als u uitvoert in deze modus, verzendt de client een verzoek om door de Service Bus een bericht. Nadat de client heeft ontvangen het bericht, stuurt een verzoek om het bericht te voltooien.

Als u de modus ontvangen op [ReceiveAndDelete][], worden beide stappen worden gecombineerd in één aanvraag. Dit het totale aantal bewerkingen voorkomt, en de totale bericht doorvoersnelheid kunt verbeteren. Deze prestatieverbetering wordt geleverd risico van berichten verliezen.

Service Bus biedt geen ondersteuning voor transacties voor bewerkingen ontvangen en verwijderen. Daarnaast zijn de korte weergave-lock semantiek vereist voor de scenario's waarin de client wil uitstellen of [onbestelbare](service-bus-dead-letter-queues.md) een bericht.

## <a name="client-side-batching"></a>Aan de clientzijde batchen

Aan de clientzijde batchen kan wachtrij of onderwerp clients wachttijd het verzenden van een bericht voor een bepaalde periode. Als de client extra berichten tijdens deze periode verzendt, verzonden berichten in één batch. Aan de clientzijde batchen veroorzaakt ook een client wachtrij/abonnement naar de batch meerdere **voltooid** aanvragen in een enkel verzoek. Batchen is alleen beschikbaar voor asynchrone bewerkingen voor **verzenden** en **voltooid** . Synchrone bewerkingen worden onmiddellijk naar de service-Service Bus verzonden. Niet batchen optreden voor de korte weergave of ontvangstbewerkingen, noch batchen over clients optreedt.

Als de batch de maximale grootte van berichten overschrijdt, het laatste bericht uit de batch is verwijderd en de client verzendt direct de batch. Het laatste bericht wordt het eerste bericht van de volgende batch. Standaard wordt met een client een tijdsinterval batch 20 MS gebruikt. U kunt het interval batch wijzigen met de eigenschap [BatchFlushInterval][] voordat u de SMS-berichten fabriek maakt. Deze instelling is van invloed op alle clients die door deze factory worden gemaakt.

Als wilt uitschakelen batchen, stelt u de eigenschap [BatchFlushInterval][] aan **TimeSpan.Zero**. Bijvoorbeeld:

```
MessagingFactorySettings mfs = new MessagingFactorySettings();
mfs.TokenProvider = tokenProvider;
mfs.NetMessagingTransportSettings.BatchFlushInterval = TimeSpan.FromSeconds(0.05);
MessagingFactory messagingFactory = MessagingFactory.Create(namespaceUri, mfs);
```

Batchen heeft geen invloed op het aantal factureerbare SMS bewerkingen en is alleen beschikbaar voor de Service Bus client-protocol. Het HTTP-protocol biedt geen ondersteuning voor batchen.

## <a name="batching-store-access"></a>Batchen store access

Als u wilt vergroten de doorvoer van een wachtrij/onderwerp/abonnement, batches Service Bus meerdere berichten wanneer deze gegevens worden geschreven naar het interne archief. Als op een wachtrij of onderwerp ingeschakeld, batch schrijven van berichten in het archief worden verwerkt. Als op een abonnement of wachtrij ingeschakeld, berichten verwijderen uit de store wordt batch worden geplaatst. Als de batch store toegang is ingeschakeld voor een entiteit, vertraagt Service Bus een schrijven bewerking store door maximaal 20 MS aangaande die entiteit. Aanvullende store bewerkingen die tijdens dit interval optreden worden toegevoegd aan de batch. Gegroepeerde store access geldt alleen voor **verzenden** en **voltooid** bewerkingen; ontvangen bewerkingen niet worden beïnvloed. Gegroepeerde store toegang is een eigenschap van een entiteit. Batchen doet zich voor op alle entiteiten die batch store toegang inschakelen.

Wanneer u maakt een nieuwe wachtrij, onderwerp of -abonnement, is de batch store access standaard ingeschakeld. Als u wilt batch store toegang uitschakelt, moet u de eigenschap [EnableBatchedOperations][] instellen op **false** voordat u de entiteit maakt. Bijvoorbeeld:

```
QueueDescription qd = new QueueDescription();
qd.EnableBatchedOperations = false;
Queue q = namespaceManager.CreateQueue(qd);
```

Gegroepeerde store access heeft geen invloed op het aantal factureerbare SMS bewerkingen en is een eigenschap van een wachtrij, onderwerp of -abonnement. Dit is afhankelijk van de modus ontvangen en het protocol dat is gebruikt tussen een client en de Service Bus-service.

## <a name="prefetching"></a>Veelgevraagde

Veelgevraagde, kunt de wachtrij of abonnement client extra berichten laden van de service wanneer deze een ontvangen uitvoert. De client worden deze berichten in een lokale cache opgeslagen. De grootte van de cache wordt bepaald door de eigenschappen [QueueClient.PrefetchCount][] of [SubscriptionClient.PrefetchCount][] . Elke client waarmee veelgevraagde onderhoudt een eigen cache. Een cache in clients niet worden gedeeld. Als de client een ontvangstbewerking Start en de cache leeg is, wordt door de service een reeks berichten verzendt. De grootte van de batch is gelijk aan de grootte van de cache of 256KB, indien dat kleiner is. Als de client een ontvangstbewerking Start en de cache een bericht bevat, wordt het bericht wordt opgehaald uit de cache.

Wanneer een bericht is tabelruimte, vergrendelen als u de service het prefetched bericht. Dit doet, kan niet het prefetched bericht worden ontvangen door een andere ontvanger. Als de ontvanger kan het bericht niet kunt voltooien voordat de vergrendeling verloopt, wordt het bericht beschikbaar voor andere ontvangers. De prefetched kopie van het bericht blijft in de cache. De ontvanger die een kopie van de verlopen cache verbruikt ontvangt een uitzondering wanneer wordt geprobeerd om uit te voeren dat bericht. Standaard wordt het bericht vergrendelen verloopt na 60 seconden. Deze waarde kan worden verlengd op 5 minuten. Om te voorkomen dat het verbruik van verlopen berichten, moet de cachegrootte altijd kleiner zijn dan het aantal berichten die kan worden gebruikt door een client binnen het time-outinterval van vergrendelen.

Wanneer u met de standaard vergrendelen verlooptijd van 60 seconden, is een goede waarde voor [SubscriptionClient.PrefetchCount][] 20 maal het maximum aantal verwerking tarieven van alle ontvangers van de fabriek. Bijvoorbeeld 3 ontvangers Hiermee maakt u een factory en elke ontvanger kan maximaal 10 berichten per seconde verwerken. Het aantal prefetch mag niet meer dan 20\*3\*10 = 600. [QueueClient.PrefetchCount][] is standaard ingesteld op 0, wat betekent dat er geen berichten meer van de service worden opgehaald.

Berichten veelgevraagde Hiermee verhoogt u de totale doorvoersnelheid voor een abonnement of wachtrij omdat het daardoor het totale aantal bericht bewerkingen of interactie. Het eerste bericht ophalen, echter duurt langer (vanwege de verbeterde berichtgrootte). Ontvangen van berichten tabelruimte worden sneller omdat deze berichten al zijn gedownload door de client.

De eigenschap voor time to live (TTL) van een bericht is ingeschakeld door de server op het moment dat de server het bericht naar de klant verzendt. De client controleert niet van het bericht TTL eigenschap wanneer het bericht is ontvangen. In plaats daarvan kan het bericht worden ontvangen evenzeer van het bericht TTL is verstreken terwijl het bericht in de cache is door de client.

Veelgevraagde heeft geen invloed op het aantal factureerbare SMS bewerkingen en is alleen beschikbaar voor de Service Bus client-protocol. Het HTTP-protocol biedt geen ondersteuning voor veelgevraagde. Veelgevraagde is beschikbaar voor synchrone en asynchrone ontvangstbewerkingen.

## <a name="express-queues-and-topics"></a>Express wachtrijen en onderwerpen

Express entiteiten hoge doorvoer inschakelen en beperkte latentie scenario's. Met express entiteiten, als een bericht wordt verzonden naar een wachtrij of onderwerp, het bericht is niet direct opgeslagen in de SMS-winkel. In plaats daarvan is deze in het cachegeheugen opgeslagen. Als een bericht in de wachtrij voor meer dan een paar seconden blijft, wordt deze automatisch naar stabiele opslaglocatie, dus beschermd tegen verlies vanwege een storing geschreven. Het bericht schrijft in de geheugencache vergroot doorvoercapaciteit en Hiermee reduceert u latentie omdat er geen toegang tot stabiele opslaglocatie op het moment dat het bericht is verzonden. Berichten die worden verbruikt binnen een paar seconden zijn niet naar de SMS-berichten store geschreven. Het volgende voorbeeld wordt een express onderwerp.

```
TopicDescription td = new TopicDescription(TopicName);
td.EnableExpress = true;
namespaceManager.CreateTopic(td);
```

Als een bericht met cruciale informatie die mag geen verloren wordt verzonden naar een express entiteit, kan de afzender Service Bus onmiddellijk persistent het bericht naar een stabiele opslaglocatie met de eigenschap [ForcePersistence][] op **true**afdwingen.

## <a name="use-of-partitioned-queues-or-topics"></a>Gebruik van gepartitioneerde wachtrijen of onderwerpen

Intern Service Bus beschikt over hetzelfde knooppunt en messaging opslaan voor verwerken en bewaren van alle berichten voor een SMS-berichten entiteit (wachtrij of onderwerp). Een gepartitioneerde wachtrij of onderwerp aan de andere kant is verdeeld over meerdere knooppunten en SMS winkels. Gepartitioneerde wachtrijen en onderwerpen niet alleen een sneller worden verwerkt dan gewone wachtrijen en onderwerpen rendement, ze ook de beschikbaarheid van de bovenliggende vertonen. Als u een gepartitioneerde entiteit, stelt u de eigenschap [EnablePartitioning][] op **waar**, zoals wordt weergegeven in het volgende voorbeeld. Zie voor meer informatie over gepartitioneerde entiteiten, [SMS entiteiten partitioneren][].

```
// Create partitioned queue.
QueueDescription qd = new QueueDescription(QueueName);
qd.EnablePartitioning = true;
namespaceManager.CreateQueue(qd);
```

## <a name="use-of-multiple-queues"></a>Gebruik van meerdere wachtrijen

Als het is niet mogelijk om een gepartitioneerde wachtrij of onderwerp te gebruiken of de verwachte belasting kan niet worden afgehandeld door een enkele gepartitioneerde wachtrij of onderwerp, kunt u meerdere SMS entiteiten moet gebruiken. Als u meerdere entiteiten gebruikt, kunt u een speciale client voor elke entiteit, in plaats van dezelfde client gebruiken voor alle entiteiten maken.

## <a name="development--testing-features"></a>Ontwikkelen en testen functies

Service Bus heeft een functie die wordt gebruikt voor het ontwikkelen welke **nooit in productie configuraties moet worden gebruikt**.

[TopicDescription.EnableFilteringMessagesBeforePublishing](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablefilteringmessagesbeforepublishing.aspx)
- Wanneer nieuwe regels of Filters worden toegevoegd aan het onderwerp, kan EnableFilteringMessagesBeforePublishing worden gebruikt om te bevestigen dat de nieuwe filterexpressie werkt zoals verwacht.

## <a name="scenarios"></a>Scenario 's

De volgende secties worden typische SMS-scenario's en overzicht van de voorkeur Bus Service-instellingen. Doorvoersnelheden (minder dan 1 bericht per seconde) zo klein worden geclassificeerd, optreden als moderator (1 bericht/tweede of groter maar kleiner dan 100 berichten/tweede) en hoog (100 berichten/tweede of groter). Het aantal clients worden geclassificeerd als kleine (5 of minder), Gemiddeld (meer dan 5 en lager is dan of gelijk is aan 20), en groot (meer dan 20).

### <a name="high-throughput-queue"></a>Hoge gegevensdoorvoer wachtrij

Doel: De doorvoer van één wachtrij maximaliseren Het aantal afzenders en ontvangers is klein.

-   Gebruik een gepartitioneerde wachtrij voor betere prestaties en beschikbaarheid.

-   Als u wilt vergroten de verzendsnelheid in de wachtrij, gebruikt u meerdere bericht factory's afzenders maken. Voor elke afzender, gebruikt u asynchrone bewerkingen of meerdere threads.

-   Als u wilt vergroten de snelheid ontvangen uit de wachtrij, gebruikt u meerdere bericht factory's ontvangers maken.

-   Asynchrone bewerkingen gebruiken om te profiteren van aan de clientzijde batchen.

-   Stel het batchen interval op 50ms om het aantal Service Bus client protocol verzendingen te verlagen. Als meerdere afzenders worden gebruikt, vergroot u de batchen interval naar 100ms.

-   Laat batch store toegang is ingeschakeld. Dit Hiermee verhoogt u de snelheid waarmee berichten in de wachtrij kunnen worden opgeslagen.

-   Stel het aantal prefetch op 20 maal het maximum aantal verwerking tarieven van alle ontvangers van een fabriek. Hierdoor wordt het aantal Service Bus client protocol verzendingen.

### <a name="multiple-high-throughput-queues"></a>Meerdere hoge gegevensdoorvoer wachtrijen

Doel: Algemene doorvoer van meerdere wachtrijen maximaliseren. De doorvoer van een afzonderlijke wachtrij is regelmatige of hoog.

Gebruik de instellingen voor de doorvoer van één wachtrij optimale overzicht om maximumdoorvoer over meerdere wachtrijen. Bovendien kunt andere bedrijven maken clients die verzenden of ontvangen van verschillende wachtrijen.

### <a name="low-latency-queue"></a>Lage latentie wachtrij

Doel: Minimaliseren end-to-end latentie van een wachtrij of onderwerp. Het aantal afzenders en ontvangers is klein. De doorvoer van de wachtrij is kleine of gemiddelde.

-   Gebruik een gepartitioneerde wachtrij voor betere beschikbaarheid.

-   Aan de clientzijde batchen uitschakelen De client wordt onmiddellijk een bericht verzendt.

-   Gegroepeerde store toegang uitschakelen. De service wordt onmiddellijk het bericht schrijft naar de store.

-   Als een enkele client gebruikt, moet u het aantal prefetch ingesteld op 20 tijden de verwerking rentabiliteit van de ontvanger. Als u meerdere berichten komen terecht in de wachtrij op hetzelfde moment, verzendt het protocol van de client Service Bus deze allemaal tegelijk. Wanneer de client het volgende bericht ontvangt, is dat bericht al in de lokale cache. De cache moet klein.

-   Als meerdere klanten gebruikt, moet u het aantal prefetch ingesteld op 0. Dit doet, kan de tweede client het tweede bericht ontvangen terwijl de eerste client steeds het eerste bericht verwerkt nog.

### <a name="queue-with-a-large-number-of-senders"></a>Wachtrij met een groot aantal afzenders

Doel: Doorvoer van een wachtrij of onderwerp met een groot aantal afzenders maximaliseren. Elke afzender wordt verzonden berichten met een gemiddelde tarief. Het aantal ontvangers is klein.

Service Bus kunnen maximaal 1000 gelijktijdige verbindingen met een SMS-berichten entiteit (of 5000 met AMQP). Deze limiet wordt afgedwongen op het niveau van de naamruimte en wachtrijen/onderwerpen/abonnementen door de limiet van verbindingen per naamruimte worden afgetopt. Voor wachtrijen, is dit nummer door afzenders en ontvangers worden gedeeld. Als alle 1000 verbindingen vereist voor afzenders zijn, moet u de wachtrij vervangen door een onderwerp en een één abonnement. Een onderwerp accepteert tot 1000 gelijktijdige verbindingen van afzenders worden geaccepteerd, terwijl het abonnement dat een extra 1000 gelijktijdige verbindingen van ontvangers accepteert. Als meer dan 1000 gelijktijdige afzenders vereist zijn, moeten de afzenders berichten verzenden naar het protocol Service Bus via HTTP.

Ga als volgt te werk als u wilt doorvoer maximaliseren:

-   Gebruik een gepartitioneerde wachtrij voor betere prestaties en beschikbaarheid.

-   Als u elke afzender zich bevindt in een ander proces, gebruikt u alleen een enkele factory per proces.

-   Asynchrone bewerkingen gebruiken om te profiteren van aan de clientzijde batchen.

-   Gebruik de standaardwaarde batchen tijdsinterval 20 MS om Beperk het aantal Service Bus client protocol verzendingen.

-   Laat batch store toegang is ingeschakeld. Dit Hiermee verhoogt u de snelheid waarmee berichten in de wachtrij of onderwerp kunnen worden opgeslagen.

-   Stel het aantal prefetch op 20 maal het maximum aantal verwerking tarieven van alle ontvangers van een fabriek. Hierdoor wordt het aantal Service Bus client protocol verzendingen.

### <a name="queue-with-a-large-number-of-receivers"></a>Wachtrij met een groot aantal ontvangers

Doel: Het tarief weer dat ontvangen van een abonnement met een groot aantal ontvangers of wachtrij maximaliseren Elke ontvanger ontvangt berichten met een gemiddelde snelheid. Het aantal afzenders is klein.

Service Bus kunnen maximaal 1000 gelijktijdige verbindingen met een entiteit. Als een wachtrij nodig meer dan 1000 ontvangers heeft, moet u de wachtrij vervangen door een onderwerp en meerdere abonnementen. Elk abonnement kan maximaal 1000 gelijktijdige verbindingen ondersteunen. U kunt ook ontvangers toegang tot de wachtrij via het HTTP-protocol.

Ga als volgt te werk als u wilt doorvoer maximaliseren:

-   Gebruik een gepartitioneerde wachtrij voor betere prestaties en beschikbaarheid.

-   Als u elke ontvanger zich bevindt in een ander proces, gebruikt u alleen een enkele factory per proces.

-   Ontvangers kunnen bewerkingen synchroon of asynchroon gebruiken. Het tarief weer dat regelmatige ontvangen van een afzonderlijke ontvanger gegeven, aan de clientzijde batchen van een volledige aanvraag heeft geen invloed op ontvanger doorvoer.

-   Laat batch store toegang is ingeschakeld. Hierdoor wordt de totale belasting van de entiteit. Het vermindert ook de snelheid waarmee berichten in de wachtrij of onderwerp kunnen worden opgeslagen.

-   Stel de prefetch telling op de kleine waarde (bijvoorbeeld PrefetchCount = 10). Hiermee voorkomt u dat ontvangers inactiviteit terwijl andere ontvangers grote aantallen van berichten die zijn opgeslagen in de cache hebt.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Onderwerp met een klein aantal abonnementen

Doel: De doorvoer van een onderwerp met een klein aantal abonnementen maximaliseren. Een bericht is ontvangen door veel abonnementen, wat betekent dat het tarief weer dat gecombineerde ontvangen over alle abonnementen groter is dan het tarief weer dat verzenden. Het aantal afzenders is klein. Het aantal ontvangers per abonnement is klein.

Ga als volgt te werk als u wilt doorvoer maximaliseren:

-   Gebruik een gepartitioneerde onderwerp voor betere prestaties en beschikbaarheid.

-   Als u wilt vergroten de verzendsnelheid in het onderwerp, gebruikt u meerdere bericht factory's afzenders maken. Voor elke afzender, gebruikt u asynchrone bewerkingen of meerdere threads.

-   Als u wilt vergroten de snelheid ontvangen van een abonnement, gebruikt u meerdere bericht factory's ontvangers maken. Voor elke ontvanger, gebruikt u asynchrone bewerkingen of meerdere threads.

-   Asynchrone bewerkingen gebruiken om te profiteren van aan de clientzijde batchen.

-   Gebruik de standaardwaarde batchen tijdsinterval 20 MS om Beperk het aantal Service Bus client protocol verzendingen.

-   Laat batch store toegang is ingeschakeld. Dit Hiermee verhoogt u de snelheid waarmee berichten in het onderwerp kunnen worden opgeslagen.

-   Stel het aantal prefetch op 20 maal het maximum aantal verwerking tarieven van alle ontvangers van een fabriek. Hierdoor wordt het aantal Service Bus client protocol verzendingen.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Onderwerp met een groot aantal abonnementen

Doel: De doorvoer van een onderwerp met een groot aantal abonnementen maximaliseren. Een bericht is ontvangen door veel abonnementen, hetgeen betekent dat het tarief weer dat gecombineerde ontvangen over alle abonnementen veel groter is dan het tarief weer dat verzenden. Het aantal afzenders is klein. Het aantal ontvangers per abonnement is klein.

Een lage algehele doorvoersnelheid bieden onderwerpen met een groot aantal abonnementen algemeen als alle berichten worden doorgestuurd naar alle abonnementen. Dit wordt veroorzaakt door het feit dat elk bericht wordt ontvangen vaak en alle berichten die zijn opgenomen in een onderwerp en alle bijbehorende abonnementen zijn opgeslagen op dezelfde locatie. Ervan wordt uitgegaan dat het aantal afzenders en het aantal ontvangers per abonnement kleine is. Service Bus ondersteunt maximaal 2000 abonnementen per onderwerp.

Ga als volgt te werk als u wilt doorvoer maximaliseren:

-   Gebruik een gepartitioneerde onderwerp voor betere prestaties en beschikbaarheid.

-   Asynchrone bewerkingen gebruiken om te profiteren van aan de clientzijde batchen.

-   Gebruik de standaardwaarde batchen tijdsinterval 20 MS om Beperk het aantal Service Bus client protocol verzendingen.

-   Laat batch store toegang is ingeschakeld. Dit Hiermee verhoogt u de snelheid waarmee berichten in het onderwerp kunnen worden opgeslagen.

-   Stel het aantal prefetch op 20 maal het verwachte ontvangen tarief in seconden. Hierdoor wordt het aantal Service Bus client protocol verzendingen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het optimaliseren van de Service Bus prestaties, Zie [Partitioned messaging entiteiten][].

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [MessageSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [PeekLock]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx
  [ReceiveAndDelete]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx
  [BatchFlushInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.netmessagingtransportsettings.batchflushinterval.aspx
  [EnableBatchedOperations]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablebatchedoperations.aspx
  [QueueClient.PrefetchCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.prefetchcount.aspx
  [SubscriptionClient.PrefetchCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.prefetchcount.aspx
  [ForcePersistence]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.forcepersistence.aspx
  [EnablePartitioning]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx
  [Gepartitioneerde SMS entiteiten]: service-bus-partitioning.md
  