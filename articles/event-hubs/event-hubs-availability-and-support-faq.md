<properties 
    pageTitle="Veelgestelde vragen (FAQ) gebeurtenis Hubs | Microsoft Azure"
    description="Gebeurtenis Hubs Veelgestelde vragen."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/01/2016"
    ms.author="sethm" />

# <a name="event-hubs-faq"></a>Gebeurtenis Hubs Veelgestelde vragen

Gebeurtenis Hubs biedt grootschalige opname, permanente en verwerking van Gegevensgebeurtenissen uit hoge gegevensdoorvoer gegevensbronnen en/of miljoenen apparaten. Als u gepaard met Service Bus wachtrijen en onderwerpen, kunt gebeurtenis Hubs permanente voor opdrachten en parameters besturingselement implementaties voor [Internet van dingen (IoT)](https://azure.microsoft.com/services/iot-hub/) scenario's.

In dit artikel wordt beschreven hoe prijsgegevens en vindt u antwoorden op enkele veelgestelde vragen over gebeurtenis Hubs:

## <a name="pricing-information"></a>Prijsinformatie

Zie de [gebeurtenis Hubs prijsgegevens](https://azure.microsoft.com/pricing/details/event-hubs/)voor volledige informatie over gebeurtenis Hubs prijzen.

## <a name="how-are-event-hubs-ingress-events-calculated"></a>Hoe worden gebeurtenis Hubs ingress gebeurtenissen berekend?

Elke gebeurtenis verzonden naar een Hub gebeurtenis telt als een factureerbare bericht. Een *gebeurtenis ingress* wordt gedefinieerd als een eenheid van de gegevens die kleiner is dan of gelijk is aan 64 KB. Een willekeurige gebeurtenis die kleiner is dan of gelijk is aan 64 KB grootte wordt beschouwd als één factureerbare gebeurtenis. Als de gebeurtenis groter dan 64 KB is, wordt het aantal factureerbare gebeurtenissen wordt berekend op basis van de grootte van de gebeurtenis, in veelvouden van 64 KB. Bijvoorbeeld een 8 KB-gebeurtenis verzonden naar de Hub gebeurtenis is gefactureerd als een gebeurtenis, maar een 96 KB-bericht verzonden naar de Hub gebeurtenis is gefactureerd als twee gebeurtenissen.

Gebeurtenissen verbruikte van een gebeurtenis-Hub, evenals beheertaken uit te voeren en oproepen zoals controlepunten bepalen, worden niet meegeteld als factureerbare ingress gebeurtenissen, maar toenemen tot aan de afschrijving van de eenheid doorvoer.

## <a name="what-are-event-hubs-throughput-units"></a>Wat zijn gebeurtenis Hubs doorvoer eenheden?

U selecteren expliciet gebeurtenis Hubs doorvoer eenheden, via de portal van Azure of gebeurtenis Hubs resource manager sjablonen. Doorvoer eenheden toepassen op alle gebeurtenis Hubs in een gebeurtenis Hubs naamruimte en elke eenheid doorvoer u hebt recht de naamruimte naar de volgende mogelijkheden:

- Omhoog tot 1 MB per seconde van ingress gebeurtenissen (gebeurtenissen verzonden op een gebeurtenis Hub), maar niet meer dan 1000 ingress gebeurtenissen, beheertaken uit te voeren of besturingselementen oproepen API per seconde.

- Maximaal 2 MB per seconde egress gebeurtenissen (gebeurtenissen verbruikt vanuit een Hub gebeurtenis).

- Maximaal 84 GB opslagruimte van gebeurtenis (voldoende voor de 24-uurs-bewaarperiode standaard).

Gebeurtenis Hubs doorvoer eenheden worden gefactureerd per uur, op basis van het maximum aantal eenheden dat is geselecteerd tijdens het opgegeven uur.

## <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Hoe worden de beperkingen voor de eenheid van de gebeurtenis Hubs doorvoer afgedwongen?

Als de totale ingress doorvoer of het percentage van de gebeurtenis totale ingress over alle gebeurtenissen Hubs in een naamruimte de toegestane van de eenheid geaggregeerde doorvoer overschrijdt, wordt afzenders wordt de snelheid en ontvangen van fouten die aangeeft dat de ingress is overschreden.

Als de totale egress doorvoer of het tarief weer dat egress totale gebeurtenis over alle gebeurtenissen Hubs in een naamruimte de toegestane van de eenheid geaggregeerde doorvoer overschrijdt, kunnen ontvangers worden vertraagd en ontvangen van fouten die aangeeft dat de egress is overschreden. Ingress- en egress quota gelden afzonderlijk, zodat er geen afzender leiden gebeurtenis verbruik tot kan langzamer te gaan, noch kan een ontvanger voorkomen dat gebeurtenissen in de Hub van een gebeurtenis is verzonden.

Houd er rekening mee dat de selectie van de eenheid doorvoer onafhankelijk van het aantal gebeurtenis Hubs partities is. Elke partition biedt een maximale capaciteit van 1 MB per tweede ingress (met een maximum van 1000 gebeurtenissen per seconde) en 2 MB per tweede egress, maar er is geen vaste kosten voor de partities zelf. De kosten zijn voor de eenheden geaggregeerde doorvoer op alle gebeurtenis Hubs in een naamruimte gebeurtenis Hubs. Met dit patroon, kunt u voldoende partities ter ondersteuning van de verwachte maximumbelasting voor hun systemen, zonder een doorvoer eenheid kosten totdat de gebeurtenis belasting op het systeem dat is wel hogere doorvoer getallen vereist en zonder te wijzigen van de structuur en de architectuur van uw systemen als de belasting van de computer verhogen.

## <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Geldt er een beperking van het aantal doorvoer eenheden dat kan worden geselecteerd?

Er is een standaardquotum van 20 doorvoer-eenheden per naamruimte. U kunt een grotere quotum doorvoer eenheden vragen door het indienen van een ondersteuningsticket. Naast de limiet van de eenheid 20 doorvoer zijn pakketten beschikbaar in 20-100 doorvoer eenheden. Houd er rekening mee dat met meer dan 20 doorvoer eenheden Hiermee verwijdert u de mogelijkheid om te wijzigen van het aantal eenheden doorvoer zonder het opbergen van een ondersteuningsticket.

## <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Is er een kosten voor het behoud van gebeurtenis Hubs gebeurtenissen voor meer dan 24 uur?

De gebeurtenis Hubs standaard laag zijn toegestaan met bericht bewaarbeleid perioden van meer dan 24 uur per dag, maximaal 30 dagen. Als u de grootte van het totale aantal opgeslagen gebeurtenissen groter is dan de afschrijving opslag voor het aantal geselecteerde doorvoer eenheden (84 GB per eenheid doorvoer), wordt de grootte groter is dan de afschrijving btw geheven snelheid gepubliceerde Azure Blob storage. De afschrijving opslagruimte in elke eenheid doorvoer behandelt alle opslagkosten voor bewaarperiode van 24 uur (de standaardinstelling) zelfs als de eenheid doorvoer omhoog wordt gebruikt om de maximale ingress afschrijving.

## <a name="what-is-the-maximum-retention-period"></a>Wat is de maximale bewaarperiode?

Gebeurtenis Hubs standaard laag ondersteunt momenteel een maximum bewaarperiode van 7 dagen. Houd er rekening mee dat gebeurtenis Hubs niet als een permanente gegevensopslag zijn bedoeld. Bewaarbeleid perioden groter is dan 24 uur zijn bedoeld voor scenario's waarin is het handig op opnieuw afspelen van de stream van een gebeurtenis in de dezelfde systemen; Als u bijvoorbeeld trainen of controleren van een nieuwe machine learning-model op bestaande gegevens.

## <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Hoe is de maximale grootte van het evenement Hubs berekend en betalen?

De totale grootte van alle opgeslagen gebeurtenissen, inclusief eventuele interne realiseren voor gebeurtenis kopteksten of op schijf opslagruimte structuren in alle gebeurtenis Hubs, wordt gemeten hele dag. Aan het einde van de dag, wordt de maximale grootte van het piek berekend. De dagelijkse opslag afschrijving wordt berekend op basis van het minimum aantal doorvoer eenheden die zijn geselecteerd tijdens de dag (per eenheid doorvoer biedt een correctie van 84 GB). Als de totale grootte groter is dan de berekende dagelijkse opslag afschrijving, wordt de overtollige opslag gefactureerd met Azure Blob storage tarieven (aan het tarief weer dat **Lokaal redundante opslag** ).

## <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-event-hubs-and-service-bus-queuestopics"></a>Kan ik een enkele AMQP verbinding verzenden en ontvangen van gebeurtenis Hubs en Service Bus wachtrijen/onderwerpen gebruiken?

Ja, mits de gebeurtenis Hubs, wachtrijen en onderwerpen in hetzelfde naamvak zijn. U kunt zo bidirectionele, brokered connectiviteit met veel apparaten, met subsecond vertragingstijden, implementeren in een efficiënt en ten zeerste scalable manier.

## <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Brokered verbinding gesprekskosten zijn van toepassing op gebeurtenis Hubs?

Voor afzenders verbinding gesprekskosten zijn van toepassing is alleen als het protocol AMQP wordt gebruikt is. Er zijn geen verbinding kosten voor het verzenden van gebeurtenissen met behulp van HTTP, ongeacht het aantal verzonden systemen of apparaten. Als u wilt gaan gebruiken AMQP (bijvoorbeeld bereiken efficiënter gebeurtenis streaming of bidirectionele communicatie in IoT scenario's voor opdrachten en parameters besturingselement inschakelen), raadpleegt u naar de pagina [Service Bus prijsinformatie](https://azure.microsoft.com/pricing/details/service-bus/) voor informatie over wat een brokered verbinding vormt, en hoe ze worden gemeten.

## <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Wat is het verschil tussen de gebeurtenis Hubs basisfilters en standaard lagen?

Gebeurtenis Hubs standaard laag biedt functies dan beschikbaar in gebeurtenis Hubs eenvoudige, en sommige Concurrentieanalyse systemen is. Deze functies omvatten bewaarperiode van meer dan 24 uur en de mogelijkheid één AMQP verbinding gebruiken om opdrachten te verzenden naar een groot aantal apparaten met subsecond vertragingstijden, evenals telemetrielogboek op deze apparaten in gebeurtenis Hubs verzenden. Zie de [gebeurtenis Hubs prijsgegevens](https://azure.microsoft.com/pricing/details/event-hubs/)voor de lijst met functies.

## <a name="geographic-availability"></a>Geografische beschikbaarheid

Gebeurtenis Hubs is beschikbaar in de volgende regio's:

|Geografische|Regio 's|
|---|---|
|Verenigde Staten|Central US, Oost Amerikaans, Oost Amerikaans 2, Zuid-Central US, West VS|
|Europa|Noord-Europa, West Europa|
|Azië en stille|Oost-Aziatische, Zuidoost-Azië|
|Japan|Japanse Oost West Japan|
|Brazilië|Brazilië zuidelijk|
|Australië|Australië Oost, Australië Zuidoost|

## <a name="support-and-sla"></a>Ondersteuning en SLA

Technische ondersteuning voor gebeurtenis Hubs is beschikbaar via de [communityforums](https://social.msdn.microsoft.com/forums/azure/home). Ondersteuning voor facturering en abonnementen management wordt gratis aangeboden.

Meer informatie over onze SLA, raadpleegt u de pagina [Serviceovereenkomsten niveau](https://azure.microsoft.com/support/legal/sla/) .

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de gebeurtenis Hubs, raadpleegt u de volgende artikelen:

- [Overzicht van de gebeurtenis Hubs][].
- Een volledige [steekproef-toepassing die gebruikmaakt van gebeurtenis Hubs][].

[Overzicht van de Hubs gebeurtenissen]: event-hubs-overview.md
[voorbeeldtoepassing die gebruikmaakt van gebeurtenis Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
