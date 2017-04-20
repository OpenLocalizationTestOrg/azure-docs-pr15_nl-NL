<properties 
    pageTitle="Service Bus prijzen en facturering | Microsoft Azure"
    description="Overzicht van de Service Bus structuur prijzen."
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
    ms.date="10/06/2016"
    ms.author="sethm" />

# <a name="service-bus-pricing-and-billing"></a>Service Bus prijzen en facturering

Service Bus wordt aangeboden in Basic, Standard en [Premium](service-bus-premium-messaging.md) lagen. U kunt een servicelaag voor elke Service Bus Servicenaamruimte die u maakt en deze selectie laag past in alle entiteiten in die naamruimte gemaakt.

>[AZURE.NOTE] Zie de [Azure Service Bus prijzen pagina](https://azure.microsoft.com/pricing/details/service-bus/)en de [Service Bus Veelgestelde vragen](service-bus-faq.md#service-bus-pricing)voor gedetailleerde informatie over de huidige Service Bus prijzen.

Service Bus gebruikt de volgende twee meter voor wachtrijen en onderwerpen/abonnementen:

1. **Bewerkingen Messaging**: gedefinieerd als API oproepen ten opzichte van de wachtrij of onderwerp/abonnement service-eindpunten. Deze meter wordt vervangen door berichten verzonden of ontvangen als de primaire eenheid factureerbare gebruik voor wachtrijen en onderwerpen/abonnementen.

2. **Brokered verbindingen**: gedefinieerd als het maximum aantal permanente verbindingen tegen wachtrijen, onderwerpen of abonnementen een bepaald één uur steekproeven periode open. Deze meter is alleen van toepassing in de standaard laag, waarin u aanvullende verbindingen kunt openen (voorheen verbindingen zijn beperkt tot 100 per wachtrij/onderwerp/abonnement) voor de kosten van een nominale per verbinding.

De **standaard** laag maakt u kennis met geleidelijke prijzen voor bewerkingen die zijn uitgevoerd met wachtrijen en onderwerpen/abonnementen, waardoor kortingen op basis van het volume van maximaal 80% op het hoogste niveau van gebruik. Er zijn ook een standaard laag grondtal kosten van €10 per maand, waarmee u maximaal 12,5 miljoen bewerkingen per maand gratis uit te voeren.

De **Premium** -laag biedt resource moeten worden geïsoleerd bij de laag van processor en geheugen zodat de werklast van elke klant op moeten worden geïsoleerd wordt uitgevoerd. Deze resource container heet een *messaging per eenheid*. Elke naamruimte premium is toegewezen aan ten minste één eenheid voor SMS-berichten. U kunt 1, 2 of 4 SMS eenheden voor elke Service Bus Premium naamruimte kopen. Een enkel werkbelasting of entiteit kan beslaan meerdere SMS eenheden en het aantal eenheden SMS-berichten kan worden gewijzigd op wordt Hoewel facturering in de 24-uurs- of dagelijkse bijdragen is. Het resultaat is overzichtelijk en herhaald prestaties voor uw oplossing op basis van een Service Bus. Niet alleen deze prestaties is meer overzichtelijk en beschikbaar, maar het is ook sneller. Azure-Service Bus Premium messaging is gebaseerd op de opslag-engine geïntroduceerd in Azure gebeurtenis Hubs. Piek prestaties is met Premium messaging, veel sneller dan de standaard laag.

Houd er rekening mee dat het standaard grondtal kosten in rekening wordt gebracht slechts één keer per maand per Azure-abonnement. Dit betekent dat nadat u een enkel-standaard of Premium laag Service Bus naamruimte hebt gemaakt, is mogelijk zo veel aanvullende standaard of Premium laag naamruimten als u onder die hetzelfde Azure-abonnement, zonder extra grondtal kosten wilt maken.

Alle bestaande Service Bus naamruimten worden gemaakt vóór 1 November 2014 zijn automatisch in de standaard laag geplaatst. Dit zorgt ervoor dat u nog steeds toegang tot alle functies die momenteel beschikbaar met Service Bus hebt. Vervolgens kunt u de [portal van Azure klassieke][] downgraden naar de eenvoudige laag desgewenst.

De volgende tabel worden de functionele verschillen tussen de Basic- en standaard/Premium lagen.

|De mogelijkheid|Eenvoudige|Standaard/Premium|
|---|---|---|
|Wachtrijen|Ja|Ja|
|Geplande berichten|Ja|Ja|
|Onderwerpen/abonnementen|Nee|Ja|
|Relais|Nee|Ja|
|Transacties|Nee|Ja|
|Verval dupliceren|Nee|Ja|
|Sessies|Nee|Ja|
|Grote berichten|Nee|Ja|
|ForwardTo|Nee|Ja|
|SendVia|Nee|Ja|
|Brokered verbindingen (inbegrepen)|100 per Service Bus naamruimte|1.000 per Azure abonnement|
|Brokered verbindingen (overdosering toegestaan)|Nee|Ja (factureerbare)|

## <a name="messaging-operations"></a>SMS-bewerkingen

Als onderdeel van het nieuwe prijzen model, is facturering voor wachtrijen en onderwerpen/abonnementen gewijzigd. Deze entiteiten van functie bent veranderd van facturering per bericht facturering per bewerking. Een "bewerking" verwijst naar een API-aanroep ten opzichte van een wachtrij of onderwerp/abonnement service-eindpunt. Dit geldt ook voor beheer, verzenden/ontvangen en sessie staat bewerkingen.

|Bewerkingstype|Beschrijving|
|---|---|
|Projectmanagement|Maken, lezen, bijwerken, verwijderen (CRUD) tegen wachtrijen of onderwerpen/abonnementen.|
|Messaging|Berichten verzenden en ontvangen met wachtrijen of onderwerpen/abonnementen.|
|Status van de sessie|Ophalen of sessie staat op een wachtrij of onderwerp/abonnement instellen.|

De volgende prijzen zijn effectieve 1 November 2014 starten:

|Eenvoudige|Kosten|
|---|---|
|Bewerkingen|$0,05 per miljoen bewerkingen|

|Standaard|Kosten|
|---|---|
|Boete voor basis|$10/ maand|
|Eerst 12,5 miljoen bewerkingen/maand|Opgenomen|
|12,5-100 miljoen bewerkingen/maand|$0,80 per miljoen bewerkingen|
|100 miljoen - 2500 miljoen bewerkingen/maand|$0,50 per miljoen bewerkingen|
|Meer dan 2500 miljoen bewerkingen/maand|$0,20 per miljoen bewerkingen|

|Premium|Kosten|
|---|---|
|Dagelijkse|$11.13 vast rentetarief per eenheid van bericht|

## <a name="brokered-connections"></a>Brokered verbindingen

*Brokered verbindingen* geschikt voor klant gebruikspatronen waarbij u gebruikmaakt van een groot aantal "persistent verbonden" afzenders/ontvangers tegen wachtrijen, onderwerpen of abonnementen. Persistent verbonden afzenders/ontvangers zijn die verbinding maken met AMQP of HTTP met een niet-nul time-out (bijvoorbeeld HTTP lange polling) ontvangen. HTTP-afzenders en ontvangers met een time-out voor direct genereren geen brokered verbindingen.

Wachtrijen en onderwerpen/abonnementen eerder een limiet van 100 verbindingen per URL. Het huidige billing schema Hiermee verwijdert u de limiet per-URL voor wachtrijen en onderwerpen/abonnementen en quota en meting op brokered verbindingen op het niveau van de Azure-abonnement en Service Bus naamruimte geïmplementeerd.

De eenvoudige laag bevat en is strikt beperkt tot, 100 brokered verbindingen per Service Bus naamruimte. Verbindingen boven dit nummer gaan verloren in de eenvoudige laag. De standaard laag Hiermee verwijdert u de limiet per naamruimte en statistische brokered verbinding gebruik telt over het Azure abonnement. In de standaard laag 1000 brokered verbindingen per Azure abonnement kunnen geen extra kosten (buiten de basis kosten). Het gebruik van meer dan 1000 brokered verbindingen in totaal op standaard laag Service Bus wordt naamruimten in een Azure-abonnement gefactureerd volgens een geleidelijk schema, zoals wordt weergegeven in de volgende tabel.

|Brokered verbindingen (standaard laag)|Kosten|
|---|---|
|Eerste 1000/maand|Boete voor grondtal is inbegrepen|
|1.000-100.000/maand|$0,03 per verbinding/maand|
|100.000-500.000/maand|$0,025 per verbinding/maand|
|Via 500.000/maand|$0.015 per verbinding/maand|

>[AZURE.NOTE] 1000 brokered verbindingen zijn opgenomen in de standaard SMS laag (via de basis boete) en kunnen worden gedeeld met alle wachtrijen, onderwerpen en abonnementen in het bijbehorende Azure-abonnement.

<br />

>[AZURE.NOTE] Facturering is gebaseerd op het maximum aantal verbindingen en wordt naar rato berekend per uur op basis van 744 uren per maand.

|Premium laag
|---|
|Brokered verbindingen worden geen btw geheven in de Premium-laag.|

Zie het gedeelte [Veelgestelde vragen](#faq) verderop in dit onderwerp voor meer informatie over brokered verbindingen.

## <a name="relay"></a>Relay

Relais zijn alleen beschikbaar in Standard laag naamruimten. Anders blijven prijzen en verbinding quota voor relais ongewijzigd. Dit betekent dat relais blijft in rekening worden gebracht van het aantal berichten (niet bewerkingen), en door te sturen uur.

|Doorsturen prijzen|Kosten|
|---|---|
|Relay uur|$0,10 voor elke uur 100 relay|
|Berichten|$0,01 voor elke 10.000 e-mails|

## <a name="faq"></a>FAQ

### <a name="how-is-the-relay-hours-meter-calculated"></a>Hoe wordt de meter Relay uur berekend?

Raadpleeg [dit onderwerp](service-bus-faq.md#how-is-the-relay-hours-meter-calculated).

### <a name="what-are-brokered-connections-and-how-do-i-get-charged-for-them"></a>Wat zijn brokered verbindingen en hoe ik krijg betalen voor deze?

Een brokered verbinding wordt gedefinieerd als een van de volgende opties:

1. Een AMQP verbinding van een client naar een onderwerp/abonnement of Service Bus wachtrij.

2. Een HTTP-oproep ontvangt van een bericht van een Service Bus onderwerp of de wachtrij waarop een time-outwaarde voor ontvangen groter is dan nul.

Service Bus kosten voor het maximum aantal brokered verbindingen die groter zijn dan de opgenomen hoeveelheid (1000 in de standaard laag). Pieken worden gemeten per uur worden berekend, naar rato berekend worden gedeeld door 744 uren in een maand en via de maandelijkse betalingsperiode opgeteld. De opgenomen hoeveelheid (1000 brokered verbindingen per maand) wordt aan het einde van de betalingsperiode tegen de som van de naar rato per uur pieken toegepast.

Bijvoorbeeld:

1. Elk van 10.000 apparaten verbinding maakt via een enkele AMQP verbinding en opdrachten van een onderwerp Service Bus ontvangt. De apparaten verzenden telemetrielogboek gebeurtenissen op de Hub van een gebeurtenis. Als alle apparaten verbinding voor 12 uur per dag maakt, de volgende verbinding gesprekskosten zijn van toepassing (naast eventuele andere Service Bus onderwerp kosten): 10.000 verbindingen *12 uur* 31 dagen / 744 = 5.000 brokered verbindingen. Na de maandelijkse correctie van 1000 brokered verbindingen, zou u btw geheven voor 4000 brokered verbindingen, op het tarief weer $0,03 per brokered verbinding voor een totaal van $120.

2. 10.000 apparaten ontvangen berichten van een Service Bus wachtrij via HTTP, geven een time-out voor niet-nul. Als alle apparaten verbinding voor 12 uur per dag maakt, ziet u de volgende verbinding kosten (naast eventuele andere Service Bus-kosten): 10.000 HTTP ontvangen verbindingen *12 uur per dag* 31 dagen / 744 uur = 5.000 brokered verbindingen.

### <a name="do-brokered-connection-charges-apply-to-queues-and-topicssubscriptions"></a>Zijn brokered verbinding gesprekskosten zijn van toepassing op wachtrijen en onderwerpen/abonnementen?

Ja. Er zijn geen verbinding kosten voor het verzenden van gebeurtenissen met behulp van HTTP, ongeacht het aantal verzonden systemen of apparaten. Gebeurtenissen ontvangen met HTTP op basis van een groter is dan nul, ook wel genoemd 'lang polling,' time-out genereert brokered verbinding kosten. AMQP verbindingen genereren brokered verbinding kosten ongeacht of de verbindingen worden gebruikt om te verzenden of ontvangen. Houd er rekening mee dat 100 brokered verbindingen gratis in een eenvoudige naamruimte zijn toegestaan. Dit is ook het maximum aantal brokered verbindingen toegestaan voor het abonnement dat Azure. De eerste 1000 brokered-verbindingen in alle standaard naamruimten in een Azure-abonnement vindt u hier gratis (buiten de basis kosten). Omdat deze vergoedingen voldoende zijn voor veel SMS scenario's voor een service-naar-service, brokered verbinding kosten gewoonlijk alleen moeten worden relevante als u van plan bent AMQP of HTTP lang-peiling gebruiken met een groot aantal clients; Als u bijvoorbeeld wilt bereiken efficiënter gebeurtenis streaming of bidirectionele communicatie met veel apparaten of toepassingsexemplaren.

## <a name="next-steps"></a>Volgende stappen

- Zie de [Azure Service Bus prijzen pagina](https://azure.microsoft.com/pricing/details/service-bus/)voor meer informatie over het Service Bus prijzen.

- Raadpleeg de [Veelgestelde vragen over het Service Bus](service-bus-faq.md#service-bus-pricing) voor enkele veelgebruikte: veelgestelde vragen rond Service bus prijzen en facturering.

[Azure klassieke portal]: http://manage.windowsazure.com