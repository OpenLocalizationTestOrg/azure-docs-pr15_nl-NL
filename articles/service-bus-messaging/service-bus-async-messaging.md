<properties 
    pageTitle="Asynchroon messaging service Bus | Microsoft Azure"
    description="Beschrijving van de Service Bus asynchrone messaging."
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

# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Asynchroon SMS patronen en betere beschikbaarheid

Asynchroon messaging kan worden geïmplementeerd in een groot aantal verschillende manieren. Met wachtrijen, onderwerpen en abonnementen ondersteunt Azure Service Bus asynchrony via een store en doorsturen om. Normale (synchroon) betrekking heeft, kunt u berichten verzenden naar wachtrijen en onderwerpen, en berichten ontvangt van wachtrijen en abonnementen. Toepassingen die u schrijft, hangt af van deze entiteiten wordt altijd beschikbaar. Wanneer de entiteit-status wordt gewijzigd, vanwege verschillende omstandigheden, moet u een oplossing voor een beperkte mogelijkheid entiteit die in de meeste behoeften voorzien kunt voorzien.

Asynchroon SMS patronen toepassingen meestal gebruiken om te schakelen van een aantal communicatie scenario's. U kunt maken op toepassingen waarin clients berichten naar services, verzenden kunnen zelfs als de service wordt niet uitgevoerd. Voor toepassingen die ervaring lichtflitsen communicatie, een wachtrij kunnen helpen het selectievakje laden effenen doordat locatie buffer communicatie. Tot slot kunt u een eenvoudige maar effectieve taakverdeling distributie van berichten op meerdere computers krijgen.

Om te kunnen de beschikbaarheid van een van deze entiteiten behouden, kunt u een aantal verschillende manieren waarin deze entiteiten niet beschikbaar voor een duurzaam berichtensysteem kunnen weergeven. In het algemeen, zien we de entity meer beschikbaar voor toepassingen die we in de volgende manieren schrijven:

- Kan geen berichten verzenden.

- Kan geen berichten ontvangen.

- Kan niet voor het beheren van entiteiten (maken, ophalen, bijwerken of verwijderen van entiteiten).

- Er kan geen verbinding maken met de service.

Voor elk van deze fouten bestaan verschillende mislukt modi waarmee een toepassing om door te gaan inzetten op sommige niveau van verminderde functionaliteit. Bijvoorbeeld een systeem die u kunt berichten verzenden maar deze niet meer ontvangen orders van klanten kan nog steeds ontvangen, maar kan niet worden verwerkt die orders. In dit onderwerp wordt beschreven hoe potentiële problemen die kunnen optreden en hoe deze problemen zijn beperkt. Service Bus heeft een aantal beperkingen die u moet kiezen in kennis en de regels voor het gebruik van deze oplossingen aanmelden in dit onderwerp ook worden besproken.

## <a name="reliability-in-service-bus"></a>Betrouwbaarheid in Service Bus

Er zijn verschillende manieren worden afgehandeld bericht en entiteit problemen en er zijn richtlijnen voor het juiste gebruik van deze oplossingen. Als u wilt weten over de richtlijnen, moet u eerst begrijpen wat in Service Bus kan mislukken. Vanwege het ontwerp van Azure systemen, al deze problemen zijn meestal tijdelijk. Op hoog niveau uitzien de verschillende oorzaken van niet beschikbaar als volgt:

-   Beperking van een extern systeem waarvan Service Bus afhankelijk is. Vóór interacties met de opslag en berekeningscluster bronnen plaatsvindt beperken.

-   Probleem voor een systeem waarvan Service Bus afhankelijk is. Een bepaald gedeelte van opslag kunt bijvoorbeeld problemen optreden.

-   Mislukken van Service Bus op één subsysteem. In dit geval een berekeningscluster knooppunt kunt ophalen voor een inconsistente status en opnieuw hebt gestart, veroorzaakt door alle entiteiten die deze fungeert voor het verdelen naar andere knooppunten. Dit kan op zijn beurt kort traag berichtverwerking veroorzaken.

-   Storing van Service Bus in een Azure datacenter. Dit is een 'volledige uitval"waarin het systeem niet bereikbaar voor het aantal minuten of een paar uur is.

> [AZURE.NOTE] De term **opslag** kan betekenen zowel Azure Storage en SQL Azure wordt aangegeven.

Service Bus bevat een aantal beperkingen voor deze problemen. De volgende secties worden de desbetreffende beperkingen en elke actie-items.

### <a name="throttling"></a>Beperken

Met de Service Bus kunt beperken gezamenlijke bericht rente management. Elk afzonderlijk Service Bus knooppunt bevinden zich veel entiteiten. Elk van deze entiteiten wordt vraag op het systeem met CPU, geheugen, opslag en andere aspecten. Wanneer een van deze aspecten gebruik vastgesteld dat groter is dan gedefinieerde drempelwaarden, Service Bus een bepaalde aanvraag kunt weigeren. De beller ontvangt een [ServerBusyException][] en pogingen na 10 seconden.

Als een risicobeperking, moet de code lezen van de fout en een nieuwe pogingen van het bericht voor ten minste tien seconden stoppen. Aangezien de fout over de onderdelen van de Klanttoepassing optreden kan, wordt naar verwachting dat elk tekstgedeelte onafhankelijk wordt uitgevoerd de logica opnieuw. De code kunt verkleinen van de kans van wordt vertraagd doordat partities op een wachtrij of onderwerp.

### <a name="issue-for-an-azure-dependency"></a>Probleem voor een Azure afhankelijkheid

Andere onderdelen binnen Azure kunnen serviceproblemen af en toe hebben. Bijvoorbeeld wanneer een systeem dat Service Bus wordt bijgewerkt, dat systeem kunt tijdelijk optreden verminderde mogelijkheden. U kunt deze soorten problemen omzeilen, Service Bus regelmatig gekeken naar en wordt geïmplementeerd beperkingen. Resultaat van deze beperkingen worden weergegeven. Om te verwerken tijdelijke problemen met opslag, implementeert Service Bus bijvoorbeeld een systeem waarmee bericht verzenden bewerkingen consistente werken. Vanwege de aard van de risicobeperking, kan een verzonden bericht worden weergegeven in de desbetreffende wachtrij of een abonnement en klaar voor een ontvangstbewerking maximaal 15 minuten duren. De meeste entiteiten ervaart in het algemeen niet dit probleem. Gezien het aantal entiteiten in Service Bus binnen Azure, is deze beperking echter soms vereist voor een klein aantal Service Bus klanten.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Fout bij Service Bus op een enkele subsysteem

Omstandigheden kunnen leiden tot een interne onderdeel van de Service Bus om vertrouwd te inconsistent met andere toepassingen. Wanneer Service Bus gedetecteerd, worden gegevens verzameld uit de toepassing als hulpmiddel bij het oplossen van wat er is gebeurd. Nadat de gegevens worden verzameld, wordt de toepassing opnieuw in een poging om terug te keren deze naar een consistente status wordt gestart. Dit proces vrij snel gebeurt, en resultaten in een entiteit zichtbaar zijn niet beschikbaar is voor maximaal een paar minuten, hoewel typische omlaag tijden veel korter.

De clienttoepassing genereert in dat geval een uitzondering [System.TimeoutException][] of [MessagingException][] . Service Bus bevat een Risicobeperking voor dit probleem in de vorm van geautomatiseerde client opnieuw logica. Zodra de periode is leeg en het bericht niet is afgeleverd, vindt u met andere functies zoals [gepaarde naamruimten][]. Gepaarde naamruimten hebt andere waarschuwingen die worden beschreven in dit artikel.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Storing van Service Bus in een Azure datacenter

De meest waarschijnlijke reden voor een fout in een datacenter van de Azure is een mislukte upgrade implementatie van Service Bus of een afhankelijke systeem. Als het platform is ontwikkeld, is de kans van dit type mislukt afgenomen. Een fout datacenter kan ook gebeuren om redenen die onder andere de volgende:

-   Elektrische storing (voeding en genereren power verdwijnen).
-   Connectiviteit (internet einde tussen uw clients en Azure).

In beide gevallen een natuurlijke of menselijke noodgevallen het probleem hebben veroorzaakt. Als dit probleem omzeilen en controleer of u kunt nog steeds berichten verzenden, kunt u [gepaarde naamruimten][] berichten naar een tweede locatie wordt verzonden, terwijl de primaire locatie bestaat uit de orde opnieuw inschakelen. Zie [Aanbevolen procedures voor het isolerende toepassingen tegen Service Bus bijvoorbeeld en systeemfouten][]voor meer informatie.

## <a name="paired-namespaces"></a>Gepaarde naamruimten

De functie [gepaarde naamruimten][] ondersteunt scenario's waarin een Service Bus entiteit of implementatie binnen een datacenter niet meer beschikbaar. Terwijl deze gebeurtenis plaatsvindt af en toe, moeten gedistribueerde systemen nog steeds worden bereid u omgaat met slechtste hoofdletters/kleine letters scenario's. Deze gebeurtenis gebeurt meestal omdat sommige element waarvan Service Bus afhankelijk is een korte probleem zich voordoet. Voor meer informatie over het behoud van de beschikbaarheid van de toepassing tijdens een storing kunnen gebruikers van de Service Bus twee afzonderlijke naamruimten, bij voorkeur in afzonderlijke datacenters, gebruiken voor het hosten van hun SMS entiteiten. De rest van deze sectie wordt de volgende terminologie gebruikt:

-   Primaire naamruimte: de naamruimte waarmee uw toepassing communiceert, voor verzenden en ontvangen van bewerkingen.

-   Secundaire naamruimte: de naamruimte die als een back-up aan de primaire naamruimte fungeert. Toepassingslogica vindt geen interactie met deze naamruimte.

-   Failover interval: de hoeveelheid tijd normale fouten accepteren voordat de toepassing van de primaire naamruimte naar de secundaire naamruimte overschakelt.

Gepaarde t-toets naamruimten ondersteuning *beschikbaarheid verzenden*. Stuur de mogelijkheid om berichten te verzenden en beschikbaarheid blijft behouden. Als u wilt verzenden beschikbaarheid gebruiken, moet uw toepassing voldoen aan de volgende vereisten:

1.  Berichten worden alleen ontvangen van de primaire naamruimte.

2.  Berichten naar een bepaalde wachtrij of onderwerp misschien een verkeerde volgorde binnenkomen.

3.  Als uw toepassing sessies gebruikt, kunnen berichten in een sessie verkeerde volgorde binnenkomen. Dit is een einde van de normale functionaliteit van sessies. Dit betekent dat uw toepassing sessies berichten logisch groeperen gebruikt. Status van de sessie worden alleen de primaire naamruimte bijgehouden.

4.  Berichten in een sessie misschien een verkeerde volgorde binnenkomen. Dit is een einde van de normale functionaliteit van sessies. Dit betekent dat uw toepassing sessies berichten logisch groeperen gebruikt.

5.  Status van de sessie worden alleen de primaire naamruimte bijgehouden.

6.  De primaire wachtrij kunt online komt en begin met het accepteren van berichten voordat de secundaire wachtrij voor alle berichten in de primaire wachtrij bevat.

De volgende secties worden de API's, hoe de API's worden geïmplementeerd en ziet u voorbeelden van code die de functie is gebruikt. Houd er rekening mee dat er billing consequenties die is gekoppeld aan deze functie.

### <a name="the-messagingfactorypairnamespaceasync-api"></a>De MessagingFactory.PairNamespaceAsync API

De functie gepaarde naamruimten bevat de methode [PairNamespaceAsync][] in de klas [Microsoft.ServiceBus.Messaging.MessagingFactory][] :

```
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Als de taak is voltooid, wordt ook de naamruimte koppeling is voltooid en klaar om te reageren op voor elke [MessageReceiver][], [QueueClient][], of [TopicClient][] die zijn gemaakt met de instantie [MessagingFactory][] . [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions][] is de basistoewijzing klasse voor de verschillende soorten koppelen die beschikbaar in een object [MessagingFactory zijn][] . De enige afgeleide klasse is momenteel een benoemde [SendAvailabilityPairedNamespaceOptions][], waarbij de vereisten van de beschikbaarheid verzenden. [SendAvailabilityPairedNamespaceOptions][] heeft een reeks constructors die zijn gebaseerd op elkaar. De constructor met de meeste parameters bekijkt, kunt u het gedrag van de andere constructors begrijpen.

```
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Deze parameters heeft de volgende betekenis:

-   *secondaryNamespaceManager*: een geïnitialiseerde [NamespaceManager][] exemplaar voor de secundaire naamruimte die de methode [PairNamespaceAsync][] gebruiken kunt voor het instellen van de secundaire naamruimte. De naamruimte manager worden gebruikt om de lijst met wachtrijen in de naamruimte ophalen en om ervoor te zorgen dat de vereiste achterstallig werk wachtrijen bestaan. Als deze wachtrijen niet aanwezig zijn, worden ze gemaakt. [NamespaceManager][] is vereist voor de mogelijkheid om te maken van een token met het claimen **beheren** .

-   *messagingFactory*: het exemplaar [MessagingFactory][] voor de secundaire naamruimte. Het object [MessagingFactory][] wordt gebruikt om te verzenden en, als de eigenschap [EnableSyphon][] is ingesteld op **waar**, berichten ontvangt van de wachtrijen achterstallig werk.

-   *backlogQueueCount*: het aantal achterstallig werk wachtrijen maken. Deze waarde moet ten minste 1. Wanneer u berichten naar achterstallig werk verzendt, wordt een van deze wachtrijen willekeurig gekozen. Als u de waarde ingesteld op 1, kunt wordt slechts één wachtrij ooit gebruikt. Wanneer dit gebeurt en de wachtrij één achterstallig werk fouten wordt gegenereerd, wordt de client is niet mogelijk om te proberen een wachtrij verschillende achterstallig werk en mislukt mogelijk het bericht te verzenden. Het is raadzaam deze waarde instellen voor sommige grotere waarde en standaard de waarde naar 10. U kunt dit wijzigen in een hogere of lagere waarde afhankelijk van hoeveel gegevens uw toepassing per dag stuurt. Elke wachtrij achterstallig werk kan maximaal 5 GB aan berichten bevatten.

-   *failoverInterval*: de hoeveelheid tijd waarin u de primaire naamruimte: fouten accepteren wilt voordat u een enkele entiteit overgaan naar de secundaire naamruimte. Failovers optreden op basis van de entiteit door entiteit. Entiteiten in één naamruimte live vaak in verschillende knooppunten binnen Service Bus. Een fout in één entiteit betekent niet een fout in een andere. U kunt deze waarde ingesteld op [System.TimeSpan.Zero][] foutherstel aan de secundaire direct na de eerste, tijdelijke is mislukt. Fouten die door de timer failover wordt geactiveerd, zijn alle [MessagingException][] waarin de eigenschap [IsTransient][] ONWAAR is of een [System.TimeoutException][]. Andere uitzonderingen, zoals [UnauthorizedAccessException][] opnieuw niet failover, want die geven de client is onjuist geconfigureerd. Een [ServerBusyException][] wordt niet failover omdat het juiste patroon wachten 10 seconden, klikt u vervolgens het bericht opnieuw verzenden.

-   *enableSyphon*: Hiermee wordt aangegeven dat dit bepaalde koppelen moet ook syphon berichten van de secundaire naamruimte terug naar de primaire naamruimte. Toepassingen die berichten verzenden moeten in het algemeen wordt deze waarde ingesteld op **Onwaar**. toepassingen die berichten ontvangt, moeten deze waarde ingesteld op **waar**. De reden hiervoor is dat vaak er minder bericht ontvangers dan afzenders van berichten. Afhankelijk van het aantal ontvangers kunt u een exemplaar van één toepassing verwerken van de rechten syphon hebt. Gebruik van de vele ontvangers heeft billing gevolgen voor elke wachtrij achterstallig werk.

Als u wilt gebruiken de code, een primaire [MessagingFactory][] exemplaar, een secundaire [MessagingFactory][] exemplaar, een secundaire [NamespaceManager][] exemplaar en een [SendAvailabilityPairedNamespaceOptions][] exemplaar te maken. De oproep zijn net zo eenvoudig als volgt te werk:

```
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Als de taak die is geretourneerd door de methode [PairNamespaceAsync][] is voltooid, is alles ingesteld en klaar voor gebruik. Voordat u de taak wordt geretourneerd, u mogelijk niet alle hebt uitgevoerd van het werk achtergrond nodig voor de koppeling naar rechts werken. U moet daardoor niet beginnen met het verzenden van berichten totdat de taak als resultaat. Als er fouten opgetreden, zoals ongeldige referenties of nalatigheid bij het maken van de wachtrijen achterstallig werk, wordt deze uitzonderingen worden gegenereerd zodra de taak is voltooid. Zodra de taak als resultaat geeft, controleert u of de wachtrijen zijn gevonden of gemaakt door de eigenschap [BacklogQueueCount][] op uw [SendAvailabilityPairedNamespaceOptions][] exemplaar te bekijken. Voor de voorgaande code, die betrekking heeft ziet er als volgt:

```
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van asynchrone messaging in Service Bus hebt geleerd, lees dan meer informatie over [gepaarde naamruimten][].

  [ServerBusyException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.serverbusyexception.aspx
  [System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [Aanbevolen procedures voor het isolerend toepassingen tegen Service Bus bijvoorbeeld en systeemfouten]: service-bus-outages-disasters.md
  [Microsoft.ServiceBus.Messaging.MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [MessageReceiver]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [SendAvailabilityPairedNamespaceOptions]:https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [EnableSyphon]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.enablesyphon.aspx
  [System.TimeSpan.Zero]: https://msdn.microsoft.com/library/azure/system.timespan.zero.aspx
  [IsTransient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.istransient.aspx
  [UnauthorizedAccessException]: https://msdn.microsoft.com/library/azure/system.unauthorizedaccessexception.aspx
  [BacklogQueueCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.backlogqueuecount.aspx
  [gepaarde naamruimten]: service-bus-paired-namespaces.md