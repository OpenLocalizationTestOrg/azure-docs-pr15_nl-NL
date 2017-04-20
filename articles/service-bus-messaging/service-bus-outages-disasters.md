<properties 
    pageTitle="Service Bus toepassingen tegen bijvoorbeeld en systeemfouten isolerend | Microsoft Azure"
    description="U gebruiken kunt om te beveiligen toepassingen ten opzichte van een mogelijke Service Bus storing technieken wordt beschreven."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/02/2016"
    ms.author="sethm" />

# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Aanbevolen procedures voor het isolerend toepassingen tegen Service Bus bijvoorbeeld en systeemfouten

Belangrijke toepassingen hanteert continu, ook in aanwezigheid van niet-geplande bijvoorbeeld of systeemfouten. In dit onderwerp wordt beschreven technieken die u gebruiken kunt om te beveiligen Service Bus toepassingen ten opzichte van een mogelijke servicestoring of noodgevallen.

Een storing wordt gedefinieerd als de tijdelijk niet beschikbaar Azure Service Bus. De storing invloed kan zijn op bepaalde onderdelen van de Service Bus, zoals een SMS-berichten store of zelfs het hele datacenter. Nadat het probleem is opgelost, weer Service Bus beschikbaar is. Een storing wordt meestal niet verlies van berichten of andere gegevens. Een voorbeeld van een storing onderdeel is het ontbreken van een bepaalde SMS winkel. Een voorbeeld van een storing datacenter hele is een fout power van het datacenter of een schakeloptie voor het netwerk van defect datacenter. Een storing kunt laatste uit een paar minuten tot enkele dagen.

Een noodgevallen wordt gedefinieerd als permanent verlies van een eenheid voor tijdschaal Service Bus of datacenter. Het datacenter mogelijk of mogelijk niet weer beschikbaar. Meestal verloren een noodgevallen sommige of alle berichten of andere gegevens. Voorbeelden van systeemfouten zijn brand, overbelasting of aardbeving.

## <a name="current-architecture"></a>Huidige architectuur

Meerdere berichten winkels Service Bus gebruikt voor het opslaan van berichten die zijn verzonden naar wachtrijen of onderwerpen. Een niet-partities wachtrij of onderwerp is toegewezen aan één SMS winkel. Als dit SMS archief niet beschikbaar is, worden alle bewerkingen voor de wachtrij of onderwerp, mislukt.

Alle Service Bus SMS entiteiten (wachtrijen, onderwerpen, relais) bevinden zich in een Servicenaamruimte, die is gekoppeld aan een datacenter. Service Bus wordt niet ingeschakeld voor automatische geografische-replicatie van gegevens, en ook niet toestaan een naamruimte naar meerdere datacenters's beslaan.

## <a name="protecting-against-acs-outages"></a>Bescherming tegen ACS bijvoorbeeld

Als u ACS referenties gebruikt en ACS niet meer beschikbaar, kunnen clients niet langer tokens ophalen. Clients die een token hebben op het moment dat ACS uitvalt kunnen blijven gebruiken Service Bus totdat de tokens verloopt. De levensduur van tokens standaard is 3 uur.

Als u wilt beveiligen tegen ACS bijvoorbeeld, gebruikt u tokens gedeeld Access handtekening (SA's). In dit geval verifieert de client rechtstreeks met Service Bus door een selfservice minted token aanmeldt met een geheime sleutel. Oproepen naar ACS zijn niet meer vereist. Zie voor meer informatie over SA's tokens, [Service Bus verificatie][].

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Wachtrijen en onderwerpen tegen messaging store fouten beveiligen

Een niet-partities wachtrij of onderwerp is toegewezen aan één SMS winkel. Als dit SMS archief niet beschikbaar is, worden alle bewerkingen voor de wachtrij of onderwerp, mislukt. Een gepartitioneerde wachtrij, wordt aan de andere kant bestaat uit meerdere fragmenten. Elke fragment wordt opgeslagen in een andere SMS winkel. Wanneer een bericht wordt verzonden naar een gepartitioneerde wachtrij of onderwerp, Service Bus wordt toegewezen van het bericht naar een van de fragmenten. Als het bijbehorende SMS-archief niet beschikbaar is, Service Bus gegevens worden geschreven het bericht naar een ander fragment, indien mogelijk. Zie voor meer informatie over gepartitioneerde entiteiten, [SMS entiteiten partitioneren][].

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Bescherming tegen datacenter bijvoorbeeld of systeemfouten

Als u wilt toestaan voor een failover tussen twee datacenters, kunt u de naamruimte van een Service Bus-service in elke datacenter. Bijvoorbeeld: de Service Bus service naamruimte **contosoPrimary.servicebus.windows.net** kunnen staan in de regio Noord Verenigde Staten/centraal en **contosoSecondary.servicebus.windows.net** in het gebied ons Zuid/centraal kunnen staan. Als een Service Bus messaging entiteit toegankelijk in de aanwezigheid van een storing datacenter blijft, kunt u die entiteit in beide naamruimten maken.

Zie de sectie "Mislukken van Service Bus binnen een Azure datacenter" in [asynchroon SMS patronen en betere beschikbaarheid][]voor meer informatie.

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Relay eindpunten tegen datacenter bijvoorbeeld of systeemfouten beveiligen

Geografische-replicatie van relay eindpunten kunt een service die een eindpunt relay om bereikbaar in de aanwezigheid van Service Bus bijvoorbeeld beschrijft. Als u wilt bereiken geografische-replicatie, moet de service relay eindpunten maken in verschillende naamruimten. De naamruimten moet zich bevinden in verschillende datacenters en de eindpunten moeten verschillende namen hebben. Een primaire-eindpunt kan bijvoorbeeld worden bereikt onder **contosoPrimary.servicebus.windows.net/myPrimaryService**, terwijl de secundaire tegenhanger onder **contosoSecondary.servicebus.windows.net/mySecondaryService**kan worden bereikt.

De service luistert vervolgens op beide eindpunten en een client de service via op de eindpunten kunt activeren. Een clienttoepassing willekeurig haalt een van de relais als het primaire eindpunt, en wordt de aanvraag verzonden naar de actieve eindpunt. Als de bewerking met een foutcode mislukt, wordt deze fout wordt aangegeven dat het eindpunt relay niet beschikbaar is. De toepassing een kanaal geopend naar de back-eindpunt en databasebron moet worden gestuurd het verzoek. Het actieve en de back-eindpunten overschakelen op dat moment rollen: de clienttoepassing acht het oude actieve eindpunt moeten de nieuwe back-eindpunt en de oude back-eindpunt moeten de nieuwe actieve eindpunt. Als beide mislukken verzendt, wordt de rollen van de twee entiteiten blijven ongewijzigd en wordt een fout geretourneerd.

De [geografische-replicatie met Service Bus doorgegeven berichten][] voorbeeld wordt gedemonstreerd hoe relais repliceren.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Wachtrijen en onderwerpen tegen datacenter bijvoorbeeld of systeemfouten beveiligen

Als u wilt bereiken flexibiliteit tegen datacenter bijvoorbeeld wanneer brokered gebruiken messaging, Service Bus ondersteunt twee methoden: *actieve* en *passieve* herhaling. Voor elke methode, als een bepaalde wachtrij of onderwerp toegankelijk bij de aanwezigheid van een storing in datacenter blijft, kunt u deze in beide naamruimten. Beide entiteiten kunnen dezelfde naam hebben. Een primaire wachtrij kan bijvoorbeeld worden bereikt onder **contosoPrimary.servicebus.windows.net/myQueue**, terwijl de secundaire tegenhanger onder **contosoSecondary.servicebus.windows.net/myQueue**kan worden bereikt.

Als de toepassing niet voor permanente afzender-naar-ontvanger communicatie vereist is, kan de toepassing een duurzame wachtrij aan de clientzijde om te voorkomen dat bericht verlies en om te beschermen van de afzender van tijdelijke Service Bus fouten implementeren.

## <a name="active-replication"></a>Actieve herhaling

Actieve herhaling gebruikt entiteiten in beide naamruimten voor elke bewerking. Een client die een bericht stuurt verzendt twee kopieën van hetzelfde bericht. Het eerste exemplaar wordt verzonden naar de primaire entiteit (bijvoorbeeld **contosoPrimary.servicebus.windows.net/sales**) en de tweede kopie van het bericht wordt verzonden naar de secundaire entiteit (bijvoorbeeld **contosoSecondary.servicebus.windows.net/sales**).

Een client ontvangt berichten uit beide wachtrijen. De ontvanger verwerkt het eerste exemplaar van een bericht en de tweede exemplaar wordt onderdrukt. Als u wilt dubbele onderdrukken, moet de afzender melding geven voor elk bericht met een unieke id. Beide versies van het bericht moeten zijn gemarkeerd met dezelfde identificatie. U kunt de eigenschappen [BrokeredMessage.MessageId][] of [BrokeredMessage.Label][] , of aangepaste eigenschap van een melding geven voor het bericht. Moet de ontvanger voor het behoud van een lijst met berichten die al is ontvangen.

Het voorbeeld [geografische-replicatie met Service Bus Brokered berichten][] worden actieve herhaling van entiteiten messaging.

> [AZURE.NOTE] De methode actieve replicatie wordt het aantal bewerkingen verdubbeld, dus deze methode kan leiden tot hogere kosten.

## <a name="passive-replication"></a>Passieve herhaling

In het geval is foutenstructuuranalyse-vrij te geven gebruikt passieve herhaling slechts één van de twee SMS entiteiten. Een client verzendt het bericht naar de actieve entiteit. Als het betrekking heeft op de actieve entiteit met een foutcode die aangeeft van het datacenter waarop de actieve entity mogelijk niet beschikbaar mislukt, stuurt de client een kopie van het bericht naar de back-entiteit. Het actieve en de back-entiteiten overschakelen op dat moment rollen: de verzendende client acht de oude actieve entiteit moeten de nieuwe back-entiteit, en de oude back-entiteit is de nieuwe actieve entiteit. Als beide mislukken verzendt, wordt de rollen van de twee entiteiten blijven ongewijzigd en wordt een fout geretourneerd.

Een client ontvangt berichten uit beide wachtrijen. Omdat er een kans dat de ontvanger twee kopieën van hetzelfde bericht ontvangt, moet de ontvanger dubbele berichten onderdrukt. U kunt dubbele waarden onderdrukken op dezelfde manier, zoals beschreven voor actieve herhaling.

Passieve replicatie is in het algemeen goedkoper dan actieve herhaling omdat in de meeste gevallen slechts één bewerking wordt uitgevoerd. Latentie, doorvoer en monetaire kosten zijn gelijk aan de niet-gerepliceerde scenario.

Wanneer u passieve herhaling, in de volgende scenario's kunnen berichten worden verbroken of tweemaal ontvangen:

-   **Bericht vertraging of verlies**: Stel dat een bericht m1 de afzender is verzonden naar de primaire wachtrij en klikt u vervolgens de wachtrij niet meer beschikbaar voordat de ontvanger m1 ontvangt. De afzender verzendt een m2 volgende bericht naar de secundaire wachtrij. Als de primaire wachtrij tijdelijk niet beschikbaar is is, wordt er in de ontvanger m1 krijgt nadat de wachtrij weer beschikbaar. Voor het geval een voordoet, kan de ontvanger nooit m1 ontvangen.

-   **Ontvangst dupliceren**: wordt ervan uitgegaan dat de afzender een bericht m verzendt naar de primaire wachtrij. Service Bus is verwerkt m maar mislukt om een antwoord te sturen. Nadat de verzendbewerking treedt er een time-out, verzendt de afzender een identieke kopie van m naar de secundaire wachtrij. Als de ontvanger ontvangen van het eerste exemplaar van m is voordat de primaire wachtrij niet meer beschikbaar, ontvangt de ontvanger beide kopieën van m op nagenoeg hetzelfde moment. Als de ontvanger ontvangen van het eerste exemplaar van m voordat de primaire wachtrij niet meer beschikbaar is, wordt de ontvanger in eerste instantie ontvangt alleen het tweede exemplaar van m, maar klikt u vervolgens een tweede exemplaar van m ontvangt wanneer de primaire wachtrij beschikbaar.

Het voorbeeld [geografische-replicatie met Service Bus Brokered berichten][] worden passieve herhaling van entiteiten messaging.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over herstel, raadpleegt u deze artikelen:

- [Bedrijfscontinuïteit voor Azure SQL-Database][]
- [Technische ondersteuning van Azure tolerantie][]

  [Service Bus verificatie]: service-bus-authentication-and-authorization.md
  [Gepartitioneerde SMS entiteiten]: service-bus-partitioning.md
  [Asynchroon SMS patronen en betere beschikbaarheid]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
  [Geografische-replicatie met Service Bus doorgegeven berichten]: http://code.msdn.microsoft.com/Geo-replication-with-16dbfecd
  [BrokeredMessage.MessageId]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx
  [BrokeredMessage.Label]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx
  [Geografische-replicatie met Service Bus Brokered berichten]: http://code.msdn.microsoft.com/Geo-replication-with-f5688664
  [Bedrijfscontinuïteit voor Azure SQL-Database]: ../sql-database/sql-database-business-continuity.md
  [Technische ondersteuning van Azure tolerantie]: ../resiliency/resiliency-technical-guidance.md
