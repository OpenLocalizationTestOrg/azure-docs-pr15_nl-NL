<properties 
    pageTitle="Azure wachtrijen en Service Bus wachtrijen - vergeleken en in afgezet tegen | Microsoft Azure"
    description="Analyseert verschillen en overeenkomsten tussen twee soorten wachtrijen door Azure worden aangeboden."
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
    ms.workload="tbd"
    ms.date="09/23/2016"
    ms.author="sethm" />

# <a name="azure-queues-and-service-bus-queues---compared-and-contrasted"></a>Azure wachtrijen en Service Bus wachtrijen - worden vergeleken en in afgezet tegen

In dit artikel worden geanalyseerd de verschillen en overeenkomsten tussen de twee soorten wachtrijen aangeboden door Microsoft Azure vandaag: Azure wachtrijen en Service Bus wachtrijen. Deze informatie wordt gebruikt, kunt u vergelijken en contrast van de desbetreffende technologieën en kunnen maken een betere beslissing over welke oplossing het beste aan uw wensen voldoet.

## <a name="introduction"></a>Inleiding

Microsoft Azure ondersteunt twee soorten wachtrij regelingen: **Azure wachtrijen** en **Service Bus wachtrijen**.

**Azure wachtrijen**, die deel uitmaken van de [Azure opslag](https://azure.microsoft.com/services/storage/) -infrastructuur, aanbevelen een eenvoudige REST gebaseerde Get-opslag/korte weergave-interface, leveren betrouwbare, permanente messaging binnen en tussen services.

**Service Bus wachtrijen** maken deel uit van een uitgebreidere [Azure messaging](https://azure.microsoft.com/services/service-bus/) infrastructuur die ondersteuning biedt voor queuing evenals publiceren/abonneren, Web service externe en integratie patronen. Zie voor meer informatie over de Service Bus wachtrijen, onderwerpen/abonnementen en relais, het [overzicht van Service Bus messaging](service-bus-messaging-overview.md).

Hoewel beide wachtrij plaatsen technologieën gelijktijdig bestaat, zijn eerst als specifieke wachtrij opslag om de opslagservices Azure gebouwd Azure wachtrijen geïntroduceerd. Service Bus wachtrijen zijn ingebouwd boven aan de bredere 'brokered messaging' infrastructuur ontworpen voor het integreren van toepassingen of toepassingsonderdelen die meerdere communicatieprotocollen gegevenscontracten, kunnen beslaan domeinen vertrouwt, en/of omgevingen.

In dit artikel worden de twee wachtrij technologieën door Azure worden aangeboden door de verschillen tussen de gedrag en de uitvoering van de functies die door elk bespreekt vergeleken. Het artikel bevat ook richtlijnen voor het kiezen van welke functies mogelijk beste aansluiten op uw behoeften van de ontwikkeling toepassing.

## <a name="technology-selection-considerations"></a>Aandachtspunten voor de selectie technologie

Azure wachtrijen zowel Service Bus wachtrijen zijn implementaties van het bericht Queuing-service op Microsoft Azure die momenteel wordt aangeboden. Elk heeft een iets anders functieset, wat betekent dat u kunt kiezen een of beide worden weergegeven, afhankelijk van de behoeften van uw bepaalde-oplossing of bedrijven/technische probleem u het oplossen van.

Wanneer om te bepalen welke wachtrij plaatsen technologie aansluit het doel voor elke oplossing, moet u de onderstaande aanbevelingen met oplossing versneld overwegen. Zie de volgende sectie voor meer informatie.

Als een oplossing ontwerper/ontwikkelaar, **moet u overwegen Azure wachtrijen** wanneer:

- Uw toepassing moet meer dan 80 GB van berichten opslaan in een wachtrij, waar de berichten een minder dan zeven dagen levensduur hebben.

- Uw toepassing wil voortgang voor het verwerken van een bericht in de wachtrij bijhouden. Dit is handig als de werknemer verwerken van een bericht loopt vast. Een latere werknemer kunt vervolgens deze informatie gebruiken om door te gaan vanaf waar de voorgaande werknemer gebleven was.

- U vereist logboeken aan de zijde van alle transacties uitgevoerd voor uw wachtrijen server.

Als een oplossing ontwerper/ontwikkelaar, **moet u overwegen Service Bus wachtrijen** wanneer:

- Uw oplossing moet worden ontvangen van berichten zonder dat u moet een poll uitvoeren onder de wachtrij. Met de Service Bus, u kunt dit doen met behulp van de lange-peiling bewerkingen op basis van de TCP-protocollen die ondersteuning biedt voor Service Bus ontvangen.

- Uw oplossing is vereist voor de wachtrij op te geven van een gegarandeerd eerste-in-first-out (FIFO) hebt besteld bezorging.

- Wilt u een symmetric ervaring in Azure en klik op Windows Server (privé cloud). Zie [Service Bus voor Windows Server](https://msdn.microsoft.com/library/dn282144.aspx)voor meer informatie.

- Uw oplossing moet kunnen automatische detectie van dubbele ondersteunen.

- U wilt dat uw toepassing op berichten als parallelle langdurige streams (berichten zijn gekoppeld aan een gegevensstroom met de [sessie-id](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) -eigenschap op het bericht). In dit model, wordt elk knooppunt in de toepassing in beslag nemen concurreert voor gegevensstromen, in plaats van berichten. Wanneer een stream wordt uitgedrukt tot in beslag nemen knooppunt, kan het knooppunt de status van de status van de toepassing-stream met transacties kunt bekijken.

- Uw oplossing is vereist voor atomiciteit wanneer verzenden of ontvangen van meerdere berichten uit een wachtrij en transacties gedrag.

- Het kenmerk voor time to live (TTL) van de werklast toepassingsspecifieke kan de 7 dagen overschrijden.

- Uw toepassing omgaat met berichten die langer zijn dan 64 KB, maar niet waarschijnlijk methode de 256 KB wordt beperkt.

- U werkt met een vereiste is om een model op basis van een rol aan het wachtrijen en andere rights/machtigingen voor afzenders en ontvangers.

- De wachtrijgrootte van uw groeit niet groter zijn dan 80 GB.

- U wilt gebruiken het AMQP 1.0 gestandaardiseerde SMS-protocol. Zie voor meer informatie over AMQP, [Service Bus AMQP overzicht](./service-bus-amqp-overview.md).

- U kunt een eventuele migratie van point-to-point wachtrij gebaseerde communicatie naar een bericht exchange patroon waarmee naadloze integratie van extra ontvangers (abonnees), die elk onafhankelijke kopieën van sommige of alle berichten die zijn verzonden naar de wachtrij ontvangt voor ogen hebt. Deze verwijst naar de mogelijkheid publiceren/abonneren op de oorspronkelijke programma is verstrekt door de Service Bus.

- Uw beheeroplossing moet kunnen ter ondersteuning van de "Meest-eens" bezorging garantie zonder dat u de onderdelen van de aanvullende infrastructuur maken.

- U wilt kunnen publiceren en gebruiken van batches van berichten.

- U vereist volledige integratie met de Windows Communication Foundation (WCF) communicatie stapel in .NET Framework.

## <a name="comparing-azure-queues-and-service-bus-queues"></a>Vergelijking van Azure wachtrijen en Service Bus wachtrijen

De tabellen in de volgende secties bieden een logische groepering van wachtrij functies en laat u vergelijken, in één oogopslag de mogelijkheden die beschikbaar zijn in zowel Azure wachtrijen en Service Bus wachtrijen.

## <a name="foundational-capabilities"></a>Moeten beschikken over mogelijkheden

In deze sectie worden enkele van de fundamentele wachtrij plaatsen mogelijkheden van Azure wachtrijen en Service Bus wachtrijen vergeleken.

|Vergelijkingscriteria|Azure wachtrijen|Service Bus wachtrijen|
|---|---|---|
|Ordening van garantie|**Nee** <br/><br>Zie de eerste opmerking in de sectie 'Aanvullende informatie' voor meer informatie.</br>|**Ja - First In First Out (FIFO)**<br/><br>(met behulp van messaging sessies)|
|Bezorging garantie|**Het kleinste-eens**|**Het kleinste-eens**<br/><br/>**De meeste-eens**|
|Ondersteuning voor atomaire bewerking|**Nee**|**Ja**<br/><br/>|
|Gedrag ontvangen|**Niet-blokkeren**<br/><br/>(is voltooid direct als er geen nieuwe bericht wordt gevonden)|**Met/zonder time-out blokkeren**<br/><br/>(aanbiedingen lang peiling of de ["Komeet methode"](http://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**Niet-blokkeren**<br/><br/>(met behulp van .NET beheerde API van alleen)|
|Push-stijl-API|**Nee**|**Ja**<br/><br/>[OnMessage](https://msdn.microsoft.com/library/azure/jj908682.aspx) en **OnMessage** sessies .NET-API.|
|Modus ontvangen|**Korte weergave & Lease**|**Korte weergave & vergrendelen**<br/><br/>**Ontvangen en verwijderen**|
|Exclusieve toegangsmodus|**Lease gebaseerde**|**Op basis van vergrendelen**|
|Duur lease/vergrendelen|**30 seconden (standaard)**<br/><br/>**7 dagen (maximaal)** (U kunt verlengen of los van de lease van een bericht met de [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API.)|**60 seconden (standaard)**<br/><br/>U kunt een bericht vergrendelen met de [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) API verlengen.|
|Lease/vergrendelen precisie van formules|**Berichtniveau**<br/><br/>(elk bericht kan hebben een andere time-outwaarde, die u vervolgens indien nodig bijwerken kunt tijdens het verwerken van het bericht met behulp van de [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API)|**Wachtrijniveau**<br/><br/>(elke wachtrij heeft een precisie lock is toegepast op alle bijbehorende berichten, maar u kunt de vergrendeling met de [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) API verlengen.)|
|Batch verwerkt ontvangen|**Ja**<br/><br/>(expliciet aantal berichten bij het ophalen van berichten, tot maximaal 32 berichten aan)|**Ja**<br/><br/>(er wordt impliciet een eigenschap oude ophalen inschakelen of expliciet via transacties)|
|Gegroepeerde verzenden|**Nee**|**Ja**<br/><br/>(met behulp van transacties of aan de clientzijde batchen)|

### <a name="additional-information"></a>Aanvullende informatie

- Berichten in Azure wachtrijen zijn meestal eerste-in-first-out, maar soms dit steekt nogal volgorde; bijvoorbeeld wanneer de duur van de time-out voor zichtbaarheid van een bericht verloopt (bijvoorbeeld als gevolg van een clienttoepassing vast tijdens het verwerken van). Wanneer de zichtbaarheid time-out is verstreken, wordt het bericht opnieuw in de wachtrij voor een andere werknemer naar deze in wachtrij zichtbaar. Het zojuist zichtbaar bericht mogelijk op dat moment in de wachtrij (om te worden uit wachtrij opnieuw geplaatst) achter een bericht dat in wachtrij oorspronkelijk daarachter worden geplaatst.

- Als u al Azure opslag BLOB's gebruikt of tabellen en u ingebruikname wachtrijen, kunt u 99,9% beschikbaarheid zijn gegarandeerd. Als u BLOB's of tabellen met Service Bus wachtrijen gebruikt, hebt u de beschikbaarheid van de onderste.

- Het patroon dat gegarandeerd FIFO in Service Bus wachtrijen is vereist voor het gebruik van messaging-sessies. In het geval dat de toepassing loopt vast tijdens het verwerken van een bericht ontvangen in de modus **korte weergave & vergrendelen** , wordt de volgende keer dat een ontvanger wachtrij een SMS-sessie accepteert gestart met het bericht is mislukt als de periode time to live (TTL) is verlopen.

- Azure wachtrijen ter ondersteuning van standaard wachtrij plaatsen scenario's, zoals toepassingsonderdelen ontkoppeling om uit te breiden schaalbaarheid en tolerantie voor niet-werkende, laden effenen, en het bouwen van proces werkstromen.

- Service Bus wachtrijen ondersteuning voor de *Kleinste-eens* bezorging garantie. Bovendien kunnen de *Meeste-eens* semantic worden ondersteund met behulp van de status van de sessie voor het opslaan van de status van de toepassing en met behulp van transacties atomically berichten en status van de sessie bijwerken.

- Azure wachtrijen bieden een uniform en consistente programming model verkoopeenheden wachtrijen, tabellen en BLOB's – zowel voor ontwikkelaars en voor bewerkingen teams.

- Service Bus wachtrijen bieden ondersteuning voor lokale transacties in de context van één wachtrij.

- De **ontvangen en verwijder** modus wordt ondersteund door de Service Bus biedt de mogelijkheid om te verkleinen van de SMS-berichten bewerking tellen (en de bijbehorende kosten) tegen verlaagde bezorging assurance.

- Azure wachtrijen voorzien lease van de mogelijkheid om uit te breiden de lease voor berichten. Hierdoor wordt de werknemers te onderhouden korte lease op berichten. Dus als een werknemer vastloopt, kunt het bericht snel opnieuw worden verwerkt door een andere werknemer. Een werknemer kan bovendien uitbreiden met de lease op een bericht dat deze langer dan de huidige leasetijd moeten worden verwerkt.

- Azure wachtrijen bieden een zichtbaarheid time-out die u kunt instellen bij de enqueueing of waarbij van een bericht. U kunt ook bijwerken van een bericht met verschillende lease waarden tijdens de uitvoering en verschillende waarden op berichten in dezelfde wachtrij worden bijgewerkt. Service Bus vergrendelen time-out zijn gedefinieerd in de wachtrij-metagegevens. u kunt echter de vergrendeling verlengen door te bellen van de methode [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) .

- De maximale time-out is voor een blokkeren ontvangstbewerking in Service Bus wachtrijen 24 dagen. Op basis van een REST-outs hebben echter een maximumwaarde van 55 seconden.

- Aan de clientzijde batchen verstrekt door de Service Bus kan wachtrij clients naar de batch meerdere berichten in een bewerking één verzenden. Batchen is alleen beschikbaar voor bewerkingen asynchroon verzenden.

- Functies zoals het 200 TB maximum van Azure wachtrijen (meer wanneer u accounts virtueel) en onbeperkt wachtrijen kunnen u een ideaal platform voor SaaS providers.

- Azure wachtrijen bieden een flexibele en zodat gedelegeerde toegangsbeveiliging.

## <a name="advanced-capabilities"></a>Geavanceerde mogelijkheden

In deze sectie worden geavanceerde mogelijkheden van Azure wachtrijen en Service Bus wachtrijen vergeleken.

|Vergelijkingscriteria|Azure wachtrijen|Service Bus wachtrijen|
|---|---|---|
|Geplande bezorging|**Ja**|**Ja**|
|Automatische dode letters|**Nee**|**Ja**|
|Beteken wachtrij time to live|**Ja**<br/><br/>(via de update van de ter plaatse van zichtbaarheid time-out)|**Ja**<br/><br/>(verstrekt via een speciale API-functie)|
|Ondersteuning voor message verontreinigen|**Ja**|**Ja**|
|In-place bijwerken|**Ja**|**Ja**|
|Aan de clientzijde transactielogboek|**Ja**|**Nee**|
|Metrische opslaggegevens|**Ja**<br/><br/>**Minuten aan de doelstellingen**: biedt realtime aan de doelstellingen voor beschikbaarheid, TPS, API bellen telt, fout telt en meer in realtime (samengevoegd per minuut en gerapporteerd binnen enkele minuten uit wat alleen is er gebeurd in productie. Zie voor meer informatie [Over Analytics opslageenheden](https://msdn.microsoft.com/library/azure/hh343258.aspx).|**Ja**<br/><br/>(bulksgewijs query's door te bellen [GetQueues](https://msdn.microsoft.com/library/azure/hh293128.aspx))|
|Provinciale management|**Nee**|**Ja**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx)|
|Automatisch doorsturen van berichten|**Nee**|**Ja**|
|Afbeelding van de functie wachtrij|**Ja**|**Nee**|
|Bericht groepen|**Nee**|**Ja**<br/><br/>(met behulp van messaging sessies)|
|De status van de toepassing per groep bericht|**Nee**|**Ja**|
|Dubbele detectie|**Nee**|**Ja**<br/><br/>(te configureren op kant van de afzender)|
|WCF-integratie|**Nee**|**Ja**<br/><br/>(biedt out-van-het-box WCF bindingen)|
|WF-integratie|**Aangepaste**<br/><br/>(het bouwen van een activiteit in een aangepaste WF vereist)|**Systeemeigen**<br/><br/>(biedt out-van-het-box WF activiteiten)|
|Browsen op het bericht groepen|**Nee**|**Ja**|
|Bericht sessies op ID ophalen|**Nee**|**Ja**|

### <a name="additional-information"></a>Aanvullende informatie

- Beide wachtrij plaatsen technologieën kunnen een bericht moet worden gepland voor de bezorging van op een later tijdstip.

- Wachtrij auto-doorschakelen stelt duizenden wachtrijen naar hun berichten naar een enkele wachtrij, waaruit de ontvangst-toepassing het bericht verbruikt automatisch doorsturen. U kunt deze methode gebruiken als u wilt bereiken beveiliging, stroom bepalen en isoleren opslag tussen elk bericht publisher.

- Azure wachtrijen bieden ondersteuning voor het bijwerken van de inhoud van het bericht. U kunt deze functionaliteit gebruiken voor informatie over de status van persistent maken en incrementele voortgangsupdates in het bericht zodat deze kan worden verwerkt in het laatste bekende controlepunt, in plaats van een draaigrafiekrapport maken. Met de Service Bus wachtrijen, kunt u hetzelfde scenario met behulp van de bericht-sessies inschakelen. Sessies kunnen u opslaan en de status van de toepassing verwerking (via [SetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx) en [GetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx)) op te halen.

- [Dode letters](service-bus-dead-letter-queues.md)wordt alleen ondersteund door de Service Bus wachtrijen, zijn handig voor berichten die niet goed worden verwerkt door de ontvangst-toepassing, of wanneer berichten hun bestemming vanwege een eigenschap verlopen time to live (TTL) kunnen bereiken isoleren. De TTL-waarde geeft hoelang een bericht blijft in de wachtrij. Met de Service Bus, wordt het bericht worden verplaatst naar een speciale wachtrij $DeadLetterQueue genoemd wanneer de TTL-verstreken.

- Controleert de eigenschap **[DequeueCount](https://msdn.microsoft.com/library/azure/dd179474.aspx)** van het bericht "poison" om berichten te zoeken in Azure wachtrijen, wanneer een bericht waarbij de toepassing. Als **DequeueCount** dan een bepaalde drempel is, de toepassing het bericht wordt verplaatst naar een wachtrij toepassingsspecifieke "onbestelbare".

- Azure wachtrijen kunnen u voor een gedetailleerd logboek van alle transacties uitgevoerd voor de wachtrij, als u ook als geaggregeerde aan de doelstellingen. Beide opties zijn handig voor foutopsporing en informatie over het gebruik van Azure wachtrijen in uw toepassing. Deze zijn ook handig voor uw toepassing prestaties optimaliseren en de kosten van het gebruik van wachtrijen.

- Het concept van het 'bericht sessies"wordt ondersteund door de Service Bus kunt berichten die deel uitmaakt van een bepaalde logische groep moet worden gekoppeld aan een bepaald ontvanger, die op zijn beurt Hiermee maakt u een sessie-achtige affiniteit tussen berichten en hun bijbehorende ontvangers. Met de eigenschap [sessie-id](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) van een bericht, kunt u deze geavanceerde functies in Service Bus inschakelen. Ontvangers kunnen vervolgens luisteren op een specifieke sessie-ID en ontvangen van berichten die de opgegeven sessie-id.

- De functionaliteit voor het detectie van dubbele items worden ondersteund door de Service Bus wachtrijen automatisch verwijdert dubbele berichten die zijn verzonden naar een wachtrij of onderwerp, op basis van de waarde van de eigenschap [MessageID](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) .

## <a name="capacity-and-quotas"></a>Capaciteit en quota

In deze sectie worden vergeleken Azure wachtrijen en Service Bus wachtrijen vanuit het perspectief van [capaciteit en quota](service-bus-quotas.md) die mogelijk van toepassing.

|Vergelijkingscriteria|Azure wachtrijen|Service Bus wachtrijen|
|---|---|---|
|Maximale grootte|**200 TB**<br/><br/>(beperkt tot een account van één opslagcapaciteit)|**1 GB 80 GB**<br/><br/>(gedefinieerd na het maken van een wachtrij en [inschakelen van partitioneren](service-bus-partitioning.md) : Zie de sectie "Aanvullende informatie")|
|Maximale grootte van berichten|**64 KB**<br/><br/>(48 KB bij gebruik van **Base64** -codering)<br/><br/>Azure ondersteunt grote berichten door wachtrijen en BLOB's – op dat moment u plaatsen kunt te combineren tot 200GB voor één item.|**256 KB** of **1 MB**<br/><br/>(inclusief zowel de koptekst en de berichttekst, maximum koptekst grootte: 64 KB).<br/><br/>Is afhankelijk van de [servicelaag](service-bus-premium-messaging.md).|
|Maximale bericht TTL|**7 dagen**|**`TimeSpan.Max`**|
|Maximum aantal wachtrijen|**Onbeperkt**|**10.000**<br/><br/>(per naamruimte van de service, kan worden verhoogd)|
|Maximum aantal gelijktijdige clients|**Onbeperkt**|**Onbeperkt**<br/><br/>(100 gelijktijdige verbindingslimiet geldt alleen voor TCP-protocol gebaseerde communicatie)|

### <a name="additional-information"></a>Aanvullende informatie

- Service Bus zorgt ervoor dat de maximale grootte wachtrij. De maximale grootte is opgegeven bij het maken van de wachtrij en een waarde tussen 1 en 80 GB kan hebben. Als de waarde voor de wachtrij instellen voor het maken van de wachtrij is bereikt, aanvullende binnenkomende berichten geweigerd en een uitzondering wordt ontvangen door de bellen code. Zie voor meer informatie over quota's in de Service Bus, [Service Bus quota](service-bus-quotas.md).

- U kunt Service Bus wachtrijen maken in 1, 2, 3, 4 of 5 GB formaten (de standaardinstelling is 1 GB). Met partitioneren ingeschakeld (dit is de standaardinstelling), Service Bus wordt 16 partities gemaakt voor elke GB die u opgeeft. Als zodanig als u een wachtrij die is 5 GB groot maakt, is door de 16 partities de maximale grootte (5 * 16) = 80 GB. U kunt de maximale grootte van uw gepartitioneerde wachtrij of onderwerp weergeven door bekijken via de vermelding van de [Azure-portal][].

- Met Azure wachtrijen, als de inhoud van het bericht niet geschikt XML is, zich moet deze **Base64** codering. Als u **Base64**-coderen van het bericht, de nettolading gebruiker kan maximaal 48 KB, in plaats van 64 KB zijn.

- Met Service Bus wachtrijen, elk bericht dat is opgeslagen in een wachtrij bestaat uit twee delen: een kop- en een hoofdtekst. De totale grootte van het bericht niet langer zijn dan de maximale grootte van berichten worden ondersteund door de servicelaag.

- Wanneer clients met Service Bus wachtrijen via het TCP-protocol communiceren, is het maximum aantal gelijktijdige verbindingen met één Service Bus wachtrij is beperkt tot 100. Dit nummer worden tussen verzenders en ontvangers gedeeld. Als dit quotum is bereikt, wordt de volgende aanvragen voor extra verbindingen geweigerd en wordt een uitzondering wordt ontvangen door de bellen code. Deze beperking niet ingesteld op clients verbinding maken met de wachtrijen op basis van een REST API gebruiken.

- Als u meer dan 10.000 wachtrijen in één Service Bus naamruimte vereist, kunt u contact opnemen met het ondersteuningsteam van Azure en een grotere aanvragen. Als u wilt schalen voorbij 10.000 wachtrijen met Service Bus, kunt u ook extra naamruimten met behulp van de [Azure-portal][]maken.

## <a name="management-and-operations"></a>Management en bedrijfsactiviteiten

In dit gedeelte worden de functies management van Azure wachtrijen en Service Bus wachtrijen vergeleken.

|Vergelijkingscriteria|Azure wachtrijen|Service Bus wachtrijen|
|---|---|---|
|Management protocol|**Op HTTP-/ HTTPS PLAATST**|**Op HTTPS PLAATST**|
|Runtime protocol|**Op HTTP-/ HTTPS PLAATST**|**Op HTTPS PLAATST**<br/><br/>**AMQP 1.0 standaard (TCP met TLS)**|
|Beheerde API van .NET|**Ja**<br/><br/>(.NET managed API voor opslag-Client)|**Ja**<br/><br/>(Beheerde brokered messaging API)|
|Systeemeigen C++|**Ja**|**Nee**|
|Java-API|**Ja**|**Ja**|
|PHP-API|**Ja**|**Ja**|
|Node.js API|**Ja**|**Ja**|
|Willekeurige metagegevens ondersteuning|**Ja**|**Nee**|
|Regels voor naamgeving van wachtrij|**Maximaal 63 tekens lang**<br/><br/>(Letters in de naam van een wachtrij moet kleine letters.)|**Maximaal 260 tekens bevatten**<br/><br/>(Wachtrij paden en namen zijn niet hoofdlettergevoelig.)|
|De functie lengte wachtrij ophalen|**Ja**<br/><br/>(Niet-geheel exacte waarde als berichten voorbij de TTL verlopen zonder wordt verwijderd.)|**Ja**<br/><br/>(Door de exacte, point-in-time waarde).|
|Korte weergave, functie|**Ja**|**Ja**|

### <a name="additional-information"></a>Aanvullende informatie

- Azure wachtrijen bieden ondersteuning voor willekeurige kenmerken die kunnen worden toegepast op de omschrijving van de wachtrij, in de vorm van naam/waardeparen.

- Beide technologieën wachtrij bieden de mogelijkheid om te kijken in een bericht zonder te vergrendelen, waarin het is soms handig bij het implementeren van een wachtrij explorer/browser hulpmiddel.

- De Service Bus .NET brokered SMS API's middelen volledige duplex TCP-verbindingen voor verbeterde prestaties in vergelijking met REST via HTTP en ondersteunen het standaard AMQP 1.0-protocol.

- Namen van Azure wachtrijen kan 3-63 tekens lang, kleine letters, cijfers en afbreekstreepjes kan bevatten. Zie [Naming wachtrijen en metagegevens](https://msdn.microsoft.com/library/azure/dd179349.aspx)voor meer informatie.

- Service Bus wachtrijnamen kunnen 260 tekens lang zijn en minder beperkend naming regels. Service Bus wachtrijnamen kunnen bevatten letters, cijfers, perioden, streepjes en onderstrepingstekens.

## <a name="authentication-and-authorization"></a>Verificatie en machtiging

In deze sectie wordt beschreven hoe de verificatie en machtiging-functies worden ondersteund door Azure wachtrijen en Service Bus wachtrijen.

|Vergelijkingscriteria|Azure wachtrijen|Service Bus wachtrijen|
|---|---|---|
|Verificatie|**Symmetric-toets**|**Symmetric-toets**|
|Beveiligingsmodel|Gedelegeerd toegang via SA's tokens.|SA 'S|
|Identiteitsfederatie provider|**Nee**|**Ja**|

### <a name="additional-information"></a>Aanvullende informatie

- Elke aanvraag naar een van de wachtrij plaatsen technologieën moet worden geverifieerd. Openbare wachtrijen met anonieme toegang worden niet ondersteund. [SA's](service-bus-sas-overview.md)gebruikt, kunt u dit scenario beantwoorden door te publiceren van een alleen-schrijven SA's, alleen-lezen SA's of zelfs een volledige toegang SA's.

- De verificatiemethode die is verstrekt door Azure wachtrijen heeft betrekking op het gebruik van symmetric toets heeft, dat wil een hash gebaseerde bericht verificatie Code (HMAC) zeggen, met de algoritme van de SHA-256 berekend en gecodeerd als een **Base64** -tekenreeks. Zie voor meer informatie over het desbetreffende protocol, [verificatie voor de opslag Azure Services](https://msdn.microsoft.com/library/azure/dd179428.aspx). Service Bus wachtrijen ondersteuning voor een soortgelijke model symmetric te gebruiken. Zie [Access handtekening-verificatie gedeeld met Service Bus](service-bus-shared-access-signature-authentication.md)voor meer informatie.

## <a name="cost"></a>Kosten

In deze sectie worden vergeleken Azure wachtrijen en Service Bus wachtrijen kosten vanuit het perspectief van.

|Vergelijkingscriteria|Azure wachtrijen|Service Bus wachtrijen|
|---|---|---|
|Wachtrij transactiekosten|**$0.0036**<br/><br/>(per 100.000 transacties)|**Eenvoudige laag**: **$0,05**<br/><br/>(per miljoen bewerkingen)|
|Factureerbare bewerkingen|**Alle**|**Verzenden/ontvangen alleen**<br/><br/>(gratis voor andere bewerkingen)|
|Niet-actieve transacties|**Factureerbare**<br/><br/>(Query's uitvoeren een lege wachtrij telt als een factureerbare transactie.)|**Factureerbare**<br/><br/>(Een ontvangen ten opzichte van een lege wachtrij wordt beschouwd als een bericht factureerbare.)|
|Opslag kosten|**$0,07**<br/><br/>(per GB/maand)|**$0,00**|
|Uitgaande gegevens overbrengen kosten|**$0,12 - $0.19**<br/><br/>(Afhankelijk van Geografie.)|**$0,12 - $0.19**<br/><br/>(Afhankelijk van Geografie.)|

### <a name="additional-information"></a>Aanvullende informatie

- Gegevens over te schrijven in rekening worden gebracht op basis van de totale hoeveelheid gegevens die de Azure datacenters via internet in een bepaald factureringsperiode verlaat.

- Gegevens over te schrijven tussen Azure services bevinden binnen dezelfde regio zijn niet gelden kosten.

- Schrijven van dit, alle inkomende gegevens overdrachten niet kunnen worden boete.

- De ondersteuning voor lange polling gegeven, kan Service Bus wachtrijen gebruik zijn kosten effectieve in situaties waarin lage latentie bezorging vereist is.

>[AZURE.NOTE] Alle kosten kunnen worden gewijzigd. In deze tabel weerspiegelt huidige prijzen en is niet inbegrepen bij elke aanbiedingen die momenteel weergegeven worden. Zie voor actuele informatie over Azure prijzen, de pagina [Azure prijzen](https://azure.microsoft.com/pricing/) . Zie [Service Bus prijzen](https://azure.microsoft.com/pricing/details/service-bus/)voor meer informatie over het Service Bus prijzen.

## <a name="conclusion"></a>Sluiten

Door een beter begrip van de twee technologieën, is mogelijk meer op de hoogte bepalen op welke wachtrij technologie wilt gebruiken en wanneer. De beschikking over het gebruik van Azure wachtrijen of Service Bus wachtrijen is duidelijk is afhankelijk van een aantal factoren. Deze factoren mogelijk intensief hangt af van de afzonderlijke behoeften van uw toepassing en de architectuur. Als uw toepassing al gebruikmaakt van de core mogelijkheden van Microsoft Azure, wilt u mogelijk Azure wachtrijen, kies met name als u eenvoudige communicatie en messaging tussen services of nodig wachtrijen die groter zijn dan 80 GB groot vereist.

Omdat Service Bus wachtrijen een aantal geavanceerde functies, zoals sessies, transacties biedt, dupliceren detectie, automatische dode-letters en duurzame publiceren/abonneren-functies, ze kunnen een uitstekende keuze als u een toepassing voor hybride bouwt of als uw toepassing anders deze functies vereist.

## <a name="next-steps"></a>Volgende stappen

De volgende artikelen vindt u meer hulp en informatie over het gebruik van Azure wachtrijen of Service Bus wachtrijen.

- [Het gebruik van de Service Bus wachtrijen](service-bus-dotnet-get-started-with-queues.md)
- [Het gebruik van de wachtrij Storage-Service](../storage/storage-dotnet-how-to-use-queues.md)
- [Aanbevolen procedures voor het prestatieverbeteringen Service Bus gebruikt brokered messaging](service-bus-performance-improvements.md)
- [Inleiding tot wachtrijen en onderwerpen in Azure Service Bus](http://www.code-magazine.com/article.aspx?quickid=1112041)
- [De handleiding voor ontwikkelaars Service Bus](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
- [Gebruik van de wachtrij plaatsen Service in Azure wordt aangegeven](http://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)
- [Wat Azure opslag facturering – bandbreedte, transacties en capaciteit](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)

[Azure-portal]: https://portal.azure.com
 
