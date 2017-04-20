<properties 
    pageTitle="Service Bus gepaarde t-toets naamruimten | Microsoft Azure"
    description="Gepaarde naamruimte implementatiedetails en kosten"
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
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Gepaarde t-toets naamruimte implementatiedetails en consequenties van kosten

De methode [PairNamespaceAsync][] , met een exemplaar [SendAvailabilityPairedNamespaceOptions][] , kunt u zichtbaar taken uitvoeren namens u. Omdat er zijn kosten overwegingen wanneer u de functie gebruikt, is het handig voor meer informatie over deze taken, zodat u het gedrag verwachten als dit gebeurt. De API alleen mogelijk het volgende automatische gedrag namens:

-   Maken van achterstallig werk wachtrijen.
-   Het maken van een [MessageSender][] -object dat gesprekken wachtrijen of onderwerpen voert.
-   Wanneer een SMS-berichten entiteit niet beschikbaar is, ping verzendt berichten naar de entity in een poging om te bepalen wanneer die entiteit weer beschikbaar.
-   (Optioneel) wordt gemaakt van een set "bericht pompen" die berichten uit de wachtrijen achterstallig werk aan de primaire wachtrijen verplaatsen.
-   Coördinaten haakje/vastgelopen van de primaire en secundaire [MessagingFactory][] exemplaren.

Op hoog niveau, werkt de functie als volgt: als de primaire entiteit in orde is, geen wijzigingen in gedrag optreden. Wanneer de duur [FailoverInterval][] is verstreken en de primaire entiteit niet slaagt ziet verzendt na een tijdelijke [MessagingException][] of een [TimeoutException][], gebeurt het volgende:

1.  Verzenden naar de primaire entiteit metagegevens en het systeem Hiermee wordt de primaire entiteit totdat ping-opdrachten kunnen worden afgeleverd.

2.  Een willekeurig achterstallig werk wachtrij is geselecteerd.

3.  [BrokeredMessage][] objecten worden doorgestuurd naar de door u gekozen achterstallig werk wachtrij.

1.  Als een verzendbewerking naar de door u gekozen achterstallig werk wachtrij mislukt, die wachtrij opgehaald uit de draaiing en een nieuwe wachtrij is geselecteerd. Alle afzenders op het exemplaar [MessagingFactory][] meer van de fout.

De volgende cijfers weer de reeks. Eerst worden berichten verzonden door de afzender.

![Gepaarde naamruimten][0]

Bij een fout te verzenden naar de primaire wachtrij, begonnen de afzender met het verzenden van berichten naar een willekeurig door u gekozen achterstallig werk wachtrij. Tegelijk, wordt er een taak ping gestart.

![Gepaarde naamruimten][1]

Nu de berichten zijn nog steeds in de secundaire wachtrij en zijn niet aan de primaire wachtrij geleverd. Zodra de primaire wachtrij opnieuw in orde is, moet ten minste één proces de syphon worden uitgevoerd. De syphon biedt de berichten van de verschillende achterstallig werk wachtrijen aan de juiste bestemming entiteiten (wachtrijen en onderwerpen).

![Gepaarde naamruimten][2]

De rest van dit onderwerp wordt beschreven hoe de specifieke details van de werking van deze onderdelen.

## <a name="creation-of-backlog-queues"></a>Maken van achterstallig werk wachtrijen

Het [SendAvailabilityPairedNamespaceOptions][] -object doorgegeven aan de methode [PairNamespaceAsync][] geeft het aantal achterstallig werk wachtrijen die u wilt gebruiken. Elke wachtrij achterstallig werk wordt vervolgens gemaakt met de volgende eigenschappen expliciet instellen (alle andere waarden zijn ingesteld op de standaardwaarden [QueueDescription][] ):

| Pad                                   | [primaire naamruimte] / x-servicebus-doorverbinden / [index] waarbij [index] is een waarde in [0, BacklogQueueCount) |
|----------------------------------------|------------------------------------------------------------------------------------------------------|
| MaxSizeInMegabytes                     | 5120                                                                                                 |
| MaxDeliveryCount                       | int zijn. MaxValue                                                                                         |
| DefaultMessageTimeToLive               | TimeSpan.MaxValue                                                                                    |
| AutoDeleteOnIdle                       | TimeSpan.MaxValue                                                                                    |
| LockDuration                           | 1 minuut                                                                                             |
| EnableDeadLetteringOnMessageExpiration | waar                                                                                                 |
| EnableBatchedOperations                | waar                                                                                                 |

Bijvoorbeeld de eerste achterstallig werk wachtrij gemaakt voor naamruimte **contoso** heet `contoso/x-servicebus-transfer/0`.

Wanneer u maakt de wachtrijen, controleert de code eerst te controleren of deze wachtrij bestaat. Als de wachtrij niet bestaat, wordt de wachtrij gemaakt. De code niet opschonen "extra" achterstallig werk wachtrijen. Met name als de toepassing met de primaire naamruimte **contoso** aanvraagt vijf wachtrijen maar een wachtrij achterstallig werk door het pad reserve `contoso/x-servicebus-transfer/7` bestaat, die extra achterstallig werk wachtrij is nog steeds aanwezig, maar niet wordt gebruikt. Expliciet kunt extra achterstallig werk wachtrijen voorkomt dat niet wordt gebruikt. Als de eigenaar van de naamruimte bent u verantwoordelijk voor het opschonen van niet-gebruikte/ongewenste achterstallig werk wachtrijen. De reden voor deze beschikking is dat Service Bus kan niet weet welk doel voor de wachtrijen in uw naamruimte bestaan. Bovendien als een wachtrij met de opgegeven naam bestaat, maar niet aan de aangenomen dat [QueueDescription voldoet][], klikt u vervolgens uw redenen zijn uw eigen voor het wijzigen van het standaardgedrag. Geen garanties worden gemaakt voor wijzigingen in de wachtrijen achterstallig werk door uw code. Zorg ervoor dat uw wijzigingen zorgvuldig test.

## <a name="custom-messagesender"></a>Aangepaste MessageSender

Wanneer u verzendt, leest u alle berichten over een interne [MessageSender][] -object dat zich gedraagt normaal wanneer alles werkt en wordt omgeleid naar de achterstallig werk wachtrijen wanneer dingen 'verbreekt." Een tijdelijke fout wordt ontvangen, een timer wordt gestart. Na een [tijdspanne][] periode die bestaat uit de waarde van de [FailoverInterval][] -eigenschap waarin geen succesvolle berichten worden verzonden, de overname betrokken blijft. De volgende zaken aan bod optreden nu voor elke entiteit:

- Een taak ping wordt uitgevoerd voor elke [PingPrimaryInterval][] om te controleren of de entiteit beschikbaar. Nadat u deze taak is uitgevoerd, wordt alle clientcode die gebruikmaakt van de entiteit direct gestart nieuwe berichten verzenden naar de primaire naamruimte.

- Toekomstige aanvragen voor verzenden naar die dezelfde entiteit van eventuele andere afzenders worden geaccepteerd resulteert in de [BrokeredMessage][] wilt wijzigen dat wordt verzonden naar gaan zitten in de wachtrij achterstallig werk. De wijziging van een bepaalde eigenschappen verwijdert uit het object [BrokeredMessage][] en elders is opgeslagen. De volgende eigenschappen zijn uitgeschakeld en onder een nieuwe alias, zodat Service Bus en de SDK om berichten te verwerken dat gelijkmatig toegevoegd:

| Oude naam van eigenschap       | Naam van de nieuwe eigenschap |
|-------------------------|-------------------|
| Sessie-id               | x-ms-sessie-id    |
| TimeToLive              | x-ms-timetolive   |
| ScheduledEnqueueTimeUtc | x-ms-pad         |

Het oorspronkelijke bestemmingspad is als een eigenschap met de naam x-ms-pad ook in het bericht opgeslagen. Dit ontwerp kunt berichten voor veel entiteiten naast in een wachtrij één achterstallig werk te. De eigenschappen worden omgezet terug door de syphon.

Het aangepaste [MessageSender][] -object kan problemen ondervinden wanneer berichten benadering van de limiet van 256 KB en failover is ingeschakeld. Het aangepaste [MessageSender][] -object worden berichten voor alle wachtrijen en onderwerpen samen opgeslagen in de wachtrijen achterstallig werk. Dit object gemengd berichten van veel primaire samen binnen de wachtrijen achterstallig werk. Voor het verwerken van taakverdeling tussen veel clients die elkaar niet weet, neemt de SDK willekeurig één achterstallig werk wachtrij voor elke [QueueClient][] of [TopicClient][] u in code maakt.

## <a name="pings"></a>Ping-opdrachten

Een ping-bericht is een lege [BrokeredMessage][] met de eigenschap [ContentType][] is ingesteld op application/vnd.ms-servicebus-ping en een waarde [TimeToLive][] van 1 seconde. Deze ping één speciale kenmerk heeft in de Service Bus: de server biedt nooit een ping wanneer een beller een [BrokeredMessage][]aanvraagt. U moet dus nooit de informatie over het ontvangen en deze berichten negeren. Elke entiteit (unieke wachtrij of onderwerp) per [MessagingFactory][] exemplaar per client wordt worden gepingd wanneer ze worden beschouwd als niet beschikbaar is. Dit gebeurt eenmaal per minuut standaard. Ping-berichten worden beschouwd als gewone Service Bus berichten en kunnen resulteren in kosten voor bandbreedte en berichten. Zodra de clients wordt gedetecteerd dat het systeem beschikbaar is, wordt de berichten stoppen.

## <a name="the-syphon"></a>De syphon

Ten minste één programma in de toepassing moet actief zijn de syphon. De syphon voert een lang poll ontvangt die 15 minuten duurt. Wanneer alle entiteiten beschikbaar zijn en u 10 achterstallig werk wachtrijen hebt, belt de toepassing die de syphon host de ontvangstbewerking 40 keer per uur, 960 keer per dag en 28800 tijden in 30 dagen. Wanneer de syphon actief berichten van achterstallig werk aan de primaire wachtrij verplaatsen is, ervaringen elk bericht in de volgende kosten (standaard kosten voor de grootte van berichten en bandbreedte toepassing in alle stadia):

1.  Verzenden naar achterstallig werk.

2.  Ontvangen van achterstallig werk.

3.  Verzenden naar de primaire.

4.  Ontvangen van de primaire.

## <a name="closefault-behavior"></a>Sluiten/foutenstructuuranalyse gedrag

Binnen een toepassing die zich bevindt de syphon, één keer de primaire of secundaire [MessagingFactory][] fouten of zonder de partner is gesloten, wordt ook mislukt/gesloten en de syphon deze status wordt de handelingen syphon vastgesteld. Als de andere [MessagingFactory][] niet binnen vijf seconden wordt gesloten, wordt de syphon fout het nog steeds openen [MessagingFactory][]optreden.

## <a name="next-steps"></a>Volgende stappen

Zie [asynchroon SMS patronen en betere beschikbaarheid][] voor gedetailleerde informatie over de Service Bus asynchrone messaging. 

  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [SendAvailabilityPairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [MessageSender]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesender.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [FailoverInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.failoverinterval.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [QueueDescription]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx
  [Tijdspanne]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
  [PingPrimaryInterval]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.pingprimaryinterval.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [ContentType]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx
  [TimeToLive]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx
  [Asynchroon SMS patronen en betere beschikbaarheid]: service-bus-async-messaging.md
  [0]: ./media/service-bus-paired-namespaces/IC673405.png
  [1]: ./media/service-bus-paired-namespaces/IC673406.png
  [2]: ./media/service-bus-paired-namespaces/IC673407.png
