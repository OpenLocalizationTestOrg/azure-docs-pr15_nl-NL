<properties 
    pageTitle="AMQP 1.0 in Azure Service Bus en gebeurtenis Hubs protocol handleiding | Microsoft Azure" 
    description="Protocol handleiding voor expressies en beschrijving van AMQP 1,0 in Azure Service Bus en gebeurtenis Hubs" 
    services="service-bus,event-hubs" 
    documentationCenter=".net" 
    authors="clemensv" 
    manager="timlt" 
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na" 
    ms.date="07/01/2016"
    ms.author="clemensv;jotaub;hillaryc;sethm"/>

# <a name="amqp-10-in-azure-service-bus-and-event-hubs-protocol-guide"></a>AMQP 1.0 in Azure Service Bus en gebeurtenis Hubs protocol-handleiding

De geavanceerde bericht wachtrij Protocol 1.0 is een gestandaardiseerde kadrering en transfer protocol voor de overdracht van berichten tussen twee partijen asynchroon, veilig en betrouwbaar. Dit is het primaire protocol van Azure Service Bus Messaging en Azure gebeurtenis Hubs. HTTPS wordt ook ondersteund door beide services. De eigen SBMP protocol waarmee wordt ook ondersteund is wordt geleidelijk vervangen door AMQP.

AMQP 1.0 is het resultaat van globaal industriële samenwerking die middleware leveranciers, zoals Microsoft en rood rol, met veel berichten middleware gebruikers zoals JP Morgan Chase dat staat voor de bedrijfstak van de financiële services worden samengebracht. Het technische standaardisatie-forum voor de AMQP protocol en een toestelnummer specificaties is OASIS en deze formele goedkeuring als internationale standaard als ISO/IEC 19494 heeft bereikt.

## <a name="goals"></a>Doelstellingen

In dit artikel bevat een overzicht van de belangrijkste begrippen van de AMQP 1.0-specificatie samen met een kleine set concept extensie specificaties die momenteel nog in de technische Commissie OASIS AMQP gewerkt wordt messaging kort en wordt uitgelegd hoe Azure-Service wordt geïmplementeerd en is gebaseerd op deze specificaties.

Het doel is voor eventuele ontwikkelaars met een bestaande AMQP 1.0-client stapel op elk platform kunnen werken met Azure Service Bus via AMQP 1.0.

Algemene algemene AMQP 1.0 stapels, zoals Apache Proton of AMQP.NET Lite, implementeer al alle core AMQP 1.0 bewegingen. Deze moeten beschikken over bewegingen worden soms verpakt met een hoger niveau API; Apache Proton biedt zelfs twee, de dwingende Messenger-API en de reactieve Reactor-API.

In de volgende bespreking wordt ervan uitgegaan dat het beheer van AMQP verbindingen, sessies, en koppelingen en de verwerking van frame overdrachten en besturing worden verwerkt door de desbetreffende stack (zoals Apache Proton-C) en hoeven niet veel als minimaal een specifieke aandacht van toepassingsontwikkelaars. We zullen abstract wordt ervan uitgegaan dat de aanwezigheid van een paar API primitieven zoals de mogelijkheid om verbinding te en andere vorm van *verzenders* en *ontvangers* abstraction om objecten te maken, hebt die vervolgens enkele vorm van `send()` en `receive()` bewerkingen, respectievelijk.

Bij het bespreken van geavanceerde mogelijkheden van Azure Service Bus, zoals browsen op het bericht of het beheer van sessies, worden die worden beschreven in AMQP termen, maar ook als een gelaagde pseudo implementatie boven aan deze aangenomen dat API abstraction.

## <a name="what-is-amqp"></a>Wat is AMQP?

AMQP is een kadrering en transfer protocol. Kadrering betekent dat deze structuur biedt voor binaire gegevensstromen die flow in de gewenste richting van een netwerkverbinding. De structuur biedt begrenzing voor afzonderlijke blokken met gegevens – frames – te wisselen tussen de verbonden partijen. De mogelijkheden van de bestandsoverdracht Zorg ervoor dat beide communiceren partijen een gedeelde lidmaatschap over wanneer frames worden overgedragen en wanneer overdrachten worden beschouwd voltooid kunnen definiëren.

In tegenstelling tot eerder verlopen conceptversies geproduceerd door de werkgroep AMQP die nog steeds gebruikt door een paar bericht makelaars worden, schrijft de aanwezigheid van een bericht makelaar of een bepaalde topologie voor entiteiten in een bericht makelaar niet voor door de werkgroep definitief en gestandaardiseerde AMQP 1.0-protocol.

Het protocol kan worden gebruikt voor symmetric peer-to-peer-communicatie, voor interactie met bericht makelaars die ondersteuning bieden voor wachtrijen en publiceren/abonneren entiteiten, evenals van Azure Service Bus. Dit kan ook worden gebruikt voor interactie met SMS infrastructuur waar de interactie patronen zijn anders uit dan gewone wachtrijen, zoals het geval met Azure gebeurtenis Hubs. Een gebeurtenis Hub fungeert als een wachtrij wanneer gebeurtenissen naartoe worden verzonden, maar meer fungeert als een seriële opslagservice wanneer gebeurtenissen worden gelezen Deze enigszins lijkt op een tapestation. De client een verschuiving haalt in de gegevensstroom beschikbaar en wordt vervolgens alle gebeurtenissen uit die verschuiving naar de meest recente waarover bediend.

Het AMQP 1.0-protocol is bedoeld om te kunnen worden verlengd, waardoor verdere specificaties voor het verbeteren van de mogelijkheden. De drie extensie specificaties we in dit document bespreken illustreren dit. Voor communicatie via bestaande HTTPS/WebSockets infrastructuur waar de systeemeigen AMQP TCP-poorten configureren kan lastig zijn, wordt een specificatie gedefinieerd hoe de lagen AMQP via WebSockets. De specificatie AMQP Management definieert voor de communicatie met de SMS-infrastructuur op aanvraag en respons wijze voor beheer of geavanceerde functionaliteit, de vereiste eenvoudige interactie primitieven. De AMQP claims-beveiliging op basis van-specificatie definieert voor federatieve autorisatie model-integratie, hoe koppelen en autorisatie tokens die is gekoppeld aan koppelingen verlengen.

## <a name="basic-amqp-scenarios"></a>Eenvoudige AMQP-scenario 's

In deze sectie wordt uitgelegd basisgebruik van AMQP 1.0 met Azure Service Bus, waaronder verbindingen, sessies en koppelingen maken en het overbrengen van berichten van en naar de Service Bus entiteiten zoals wachtrijen, onderwerpen en abonnementen.

De meest gezaghebbende bron voor meer informatie over de werking van AMQP is de AMQP 1.0-specificatie, maar de specificatie is geschreven naar exact begeleiden implementatie en niet als u wilt leren het protocol. In deze sectie bevat informatie op Kennismaking met zoveel terminologie zo nodig voor de beschrijving van het gebruik van Service Bus AMQP 1.0. Voor een uitgebreidere Inleiding tot AMQP, evenals een uitgebreidere discussie van AMQP 1.0, kunt u [deze cursus video][]bekijken.

### <a name="connections-and-sessions"></a>Verbindingen en sessies

![][1]

AMQP belt de communiceren programma's *containers*; de bevatten *knooppunten*, die de communiceren entiteiten binnen deze containers zijn. Een wachtrij kan zich bijvoorbeeld een knooppunt. AMQP kunnen multiplexing, zodat een enkele verbinding kan worden gebruikt voor veel communicatiepaden tussen knooppunten; de toepassingsclient van een kan bijvoorbeeld gelijktijdig ontvangen van de ene wachtrij en naar een andere wachtrij verzenden via dezelfde netwerkverbinding.

De netwerkverbinding is dus verankerd op de container. Dit is gestart door de container in de rol van de client maken van een uitgaande TCP-socketverbinding met een container in de rol van de ontvanger die luistert naar en binnenkomende TCP-verbindingen accepteert. De verbinding handshake bevat de versie van het protocol, declareren of onderhandelingen over het gebruik van Transport Level Security (TLS/SSL) en een handshake verificatie voor het verbindingsbereik die is gebaseerd op SASL onderhandelingen.

Azure-Service Bus is vereist voor het gebruik van TLS allen tijde. Dit ondersteunt verbindingen via TCP-poorten 5671, waarbij de TCP-verbinding met TLS eerst wat is voordat u de AMQP protocol handshake invoert en ondersteunt ook verbindingen via TCP-poorten 5672 waarbij de server direct biedt een verplicht upgrade verbinding TLS met het model AMQP beschreven. De binding AMQP WebSockets Hiermee maakt u een tunnel via TCP-poort 443 die vervolgens gelijk is aan AMQP 5671 verbindingen.

Nadat u de verbinding en TLS ingesteld, biedt Service Bus twee SASL om opties:

-   SASL zonder opmaak wordt gebruikt voor het doorgeven van gebruikersnaam en wachtwoord referenties op een server. Service Bus heeft geen accounts, maar met de naam voor [beveiliging in Access gedeeld regels](service-bus-shared-access-signature-authentication.md)welke rechten verlenen en zijn gekoppeld aan een sleutel. De naam van een regel wordt gebruikt als de gebruikersnaam van de en de toets (zoals base64 gecodeerd tekst) wordt gebruikt als het wachtwoord. De bewerkingen die is toegestaan op de verbinding wordt bepaald door de rechten die is gekoppeld aan de door u gekozen regel.

-   ANONIEME SASL wordt gebruikt voor SASL autorisatie overslaan wanneer de client wil gebruiken de claims-gebaseerd-(CBS) beveiligingsmodel dat later worden beschreven. Met deze optie wordt een clientverbinding kan worden gemaakt anoniem gedurende een beperkte periode waarin de client alleen met het eindpunt CBS kunt werken en de handshake CBS moet uitvoeren.

Nadat de transportverbinding tot stand is gebracht, de containers formaat van het maximum aantal frame declareren ze zijn bereid worden verwerkt, en de na welke inactiviteit ze wordt eenzijdig verbinding verbreken als er geen activiteit op de verbinding is.

Ze declareren ook hoeveel gelijktijdige kanalen worden ondersteund. Een kanaal is een pad één richting, uitgaande, virtuele transfer boven aan de verbinding. Een sessie vindt een kanaal uit elk van de onderling verbonden containers om een pad bidirectionele communicatie.

Sessies hebben een venster-e-mailstroom besturingselement model; Als een sessie is gemaakt, wordt elke partij gedeclareerd hoeveel frames is willen accepteren in het venster ontvangen. Als de partijen exchange-frames overgedragen frames opvulling die-venster en overdrachten stoppen wanneer het venster vol is en totdat het venster wordt opnieuw instellen of uitgevouwen gebruiken van de *flow performative* (*performative* is de term AMQP voor protocol niveau bewegingen uitwisselen tussen de twee partijen).

Dit model op basis van het venster is ongeveer gelijk is aan het TCP-concept van venster-e-mailstroom besturingselement, maar op het niveau van de sessie binnen de socket. Concept van het protocol voor meerdere gelijktijdige sessies toestaan ervoor te zorgen dat hoge prioriteit verkeer kan worden meest eerdere vertraagd normale verkeer, zoals op een express baan weg.

Azure-Service Bus precies één sessie wordt momenteel wordt gebruikt voor elke verbinding. De Service Bus frame-maximumgrootte is 262.144 bytes (256K bytes) voor Service Bus Standard en gebeurtenis Hubs. 1.048.576 (1 MB) voor Service Bus Premium is. Service Bus leggen geen een bepaald niveau sessie bandbreedteregeling windows, maar stelt regelmatig opnieuw in het venster als onderdeel van de koppeling niveau besturing (Zie [de volgende sectie](#links)).

Verbindingen, kanalen en sessies zijn kortstondige. Als de onderliggende verbinding wordt samengevouwen, verbindingen, TLS tunnel, SASL autorisatiecontext en sessies moeten worden opnieuw ingesteld.

### <a name="links"></a>Koppelingen

![][2]

AMQP brengt berichten via verbindingen. Een koppeling is een communicatiepad gemaakt via een sessie waarmee overdragen berichten in één richting; de overdracht status-onderhandelingen is op de koppeling en bidirectionele tussen de verbonden partijen.

Koppelingen kunnen worden gemaakt door een van beide container op elk gewenst moment en na verloop van een bestaande sessie, waardoor u AMQP verschillen van de vele andere protocollen, inclusief HTTP en MQTT, waar de inleiding van overdrachten en doorverbinden pad is een exclusieve bevoegdheid van degene die de socketverbinding te maken.

De container initiëren met een koppeling wordt gevraagd om de tegenovergestelde container een koppeling te accepteren en een rol van de afzender of ontvanger worden gekozen. Daarom op container kunt starten met het maken van één richting of bidirectionele communicatiepaden, met de laatste gezien als paren van koppelingen.

Koppelingen zijn met de naam en dat is gekoppeld aan knooppunten. De wijze beschreven in het begin, zijn knooppunten de communiceren entiteiten in een container.

In Azure Service Bus is het knooppunt rechtstreeks gelijk is aan een wachtrij, een onderwerp, een abonnement of een onderliggend wachtrij van een abonnement of wachtrij. De naam van het knooppunt gebruikt in AMQP daarom de relatieve naam van de entiteit binnen de naamruimte Service Bus. Als een wachtrij heet **DezeWachtrij**, is dat ook de naam van het knooppunt AMQP. Een abonnement onderwerp volgt de HTTP-API-overeenkomst met die in een siteverzameling van de resource 'abonnementen' en dus ook een abonnement **sub** geordend of een onderwerp **mytopic** heeft de AMQP knooppunt naam **mytopic/abonnementen/sub**.

De client is ook vereist voor het gebruik van een lokale knooppunt-naam voor het maken van koppelingen; Service Bus is niet beschrijvende over knooppuntnamen en wordt niet interpreteren. AMQP 1.0-client stapels doorgaans gebruik een schema te verzekeren die deze knooppuntnamen kortstondige uniek in het bereik van de client zijn.

### <a name="transfers"></a>Overdrachten

![][3]

Nadat u een koppeling tot stand is gebracht, kunnen berichten worden overgedragen die koppeling. In AMQP, is een uitgevoerd met een beweging expliciete protocol (de *overdracht* performative) die een bericht van afzender naar de ontvanger op een koppeling verplaatst. Een overdracht is voltooid wanneer deze wordt 'vereffend', wat betekent dat beide partijen een gedeelde begrip van het resultaat van deze doorverbinden tot stand hebt gebracht.

De afzender kunt verzenden van berichten 'vooraf vereffend', wat betekent dat de client niet geïnteresseerd in het resultaat is en de ontvanger feedback over de uitkomst van de bewerking niet krijgt in het geval eenvoudigste. Deze modus wordt ondersteund door Azure Service Bus op het niveau van de protocol AMQP, maar niet beschikbaar zijn in een van de client-API.

Het normale hoofdlettergebruik is dat berichten worden verzonden niet-vereffende, en de ontvanger wordt vervolgens acceptatie of afkeuring de performative *voor bestemming* gebruiken. Weigering treedt op wanneer de ontvanger het bericht niet om een reden accepteren en het bericht afkeuring bevat informatie over de reden, dat wil de structuur van een fout gedefinieerd door AMQP zeggen. Als berichten worden geweigerd vanwege interne fouten binnen Azure Service Bus, retourneert de service extra gegevens binnen een die structuur die kan worden gebruikt voor het leveren van hints voor de diagnostische hulpprogramma's om ondersteuningspersoneel als u aanvragen voor ondersteuning indient. Leert u meer informatie over fouten later.

Een speciale vorm van geweigerde is de status *uitgebracht* , waarmee wordt aangegeven dat de ontvanger heeft geen technische bezwaren met de overdracht, maar ook niet geïnteresseerd bent in de overdracht vereffenen. Dat hoofdletters/kleine letters bestaat, bijvoorbeeld wanneer een bericht is bezorgd bij een Service Bus-client en de client wil het bericht 'verlaten"omdat deze niet kan worden uitgevoerd het werk die resulteert uit het verwerken van het bericht terwijl het bericht is bezorgd zelf niet beschadigd is. Een variatie van die staat is de status *gewijzigd* , zodat wijzigingen in het bericht, zoals deze is uitgebracht. Die staat, wordt op dit moment niet door Service Bus gebruikt.

De AMQP 1.0-specificatie definieert een verdere voor bestemming staat *ontvangen* die specifiek helpt bij het afhandelen van koppeling herstel. Koppeling herstel kunt opnieuw samenstellen op basis de status van een koppeling en alle wachtende leveringen boven aan een nieuwe verbinding en een sessie, wanneer de eerdere verbinding en de sessie verloren gegaan zijn.

Azure-Service Bus biedt geen ondersteuning voor koppeling herstel; Als de client de verbinding met de Service Bus met een niet-vereffende bericht overbrengen in behandeling, de overdracht van dat bericht is verbroken en de client moet opnieuw verbinding maakt, de koppeling herstellen en probeer de overdracht.

Als zodanig ondersteunen Azure Service Bus en gebeurtenis Hubs "ten minste eenmaal" overdrachten waar de afzender kan worden gewaarborgd voor het bericht hebt opgeslagen en geaccepteerd, maar worden niet ondersteund "exact eenmaal" overdrachten op het niveau van AMQP, waar het systeem probeert te herstellen van de koppeling en gaat u verder met het Onderhandel over de bezorging staat om te voorkomen dat dupliceren van het bericht is verzonden.

Ter voor mogelijk dubbele record stuurt, Azure Service Bus ondersteunt dubbele detectie als een optionele functie van wachtrijen en onderwerpen. Records van de detectie van het bericht-id's van alle inkomende berichten tijdens een door de gebruiker gedefinieerde tijdvenster dupliceren en stilte neerzetten alle berichten die worden verzonden met de hetzelfde bericht-id's tijdens hetzelfde venster.

### <a name="flow-control"></a>Stroom-besturingselement

![][4]

Naast het model van de sessie niveau stroom besturingselement die eerder is beschreven, heeft elke koppeling een eigen model stroom-besturingselement. Sessie niveau besturing voorkomt dat de container ondervindt u omgaat met te veel frames aan eenmaal in, koppeling niveau stroom besturingselement plaatst de toepassing verantwoordelijk hoeveel berichten wil verwerken vanuit een koppeling en wanneer.

Op een koppeling, kunnen alleen overdrachten gebeuren wanneer de afzender heeft onvoldoende "koppeling krediet". Koppeling krediet is een item instellen door de ontvanger met de *stroom* van performative, die is beperkt tot een koppeling. Wanneer en terwijl de afzender koppeling krediet is toegewezen, wordt deze probeert te gebruiken van die krediet bezorgen van berichten. Elke bericht bezorging verlaagt de resterende koppeling creditering door een. Wanneer de koppeling lof omhoog wordt gebruikt, niet meer leveringen.

Wanneer Service Bus zich in de rol van de ontvanger bevindt wordt deze onmiddellijk voorzien van de afzender voldoende koppeling creditcard, zodat berichten direct kunnen worden verzonden. Zoals koppeling creditcard wordt gebruikt, stuurt Service Bus af en toe een *stroom* performative naar de afzender het saldo koppeling bijwerken.

In de rol van de afzender wordt stuurt Service Bus enthousiast berichten van ieder krediet uitstaande koppeling gebruiken.

Een oproep 'ontvangen' op het niveau van de API omgezet in een *stroom* performative naar Service-Bus is verzonden door de client en Service Bus wordt die creditcard gebruiken door het eerste beschikbaar, ontgrendeld bericht uit de wachtrij duurt, door deze te vergrendelen en overdragen. Als er geen bericht beschikken voor de bezorging van, een uitstaande krediet door een koppeling tot stand gebracht met dat bepaalde entiteit de opgenomen in de volgorde van ontvangst blijven, en berichten worden vergrendeld en worden overgedragen zodra deze beschikbaar voor gebruik van een uitstaande krediet.

De vergrendeling van een bericht wordt uitgebracht wanneer de overdracht wordt vereffend in een van de terminal Staten *geaccepteerd*, *geweigerd*of *uitgebracht*. Het bericht wordt verwijderd van Service Bus wanneer de status van de terminal *geaccepteerd is*. Er blijft aanwezig in Service Bus en worden bezorgd bij de volgende ontvanger wanneer de overdracht van de andere Staten bereikt. Service Bus wordt automatisch het bericht verplaatsen naar de wachtrij van de entiteit wanneer de bezorging van het maximum aantal toegestaan voor de entiteit vanwege herhaalde afwijzingen of versies worden bereikt.

Hoewel de officiële Service Bus-API's niet rechtstreeks zoals een optie vandaag worden, kunt een lager niveau AMQP protocol client het model koppeling creditcard gebruiken om te schakelen van de interactie 'halen-stijl' als u één eenheid van krediet voor elk ontvangen verzoek om uit te geven in een model 'push-stijl' door een groot aantal koppeling tegoeden en vervolgens ontvangen zodra deze beschikbaar zonder tussenkomst verdere. Push wordt ondersteund door de instellingen van de eigenschap [MessagingFactory.PrefetchCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.prefetchcount.aspx) of [MessageReceiver.PrefetchCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.prefetchcount.aspx) . Wanneer ze dan nul zijn, wordt deze door de AMQP-client gebruikt als de koppeling lof.

In deze context is het belangrijk om te begrijpen dat de klok voor het verloop van de vergrendeling van het bericht in de entiteit wordt gestart wanneer het bericht wordt gemaakt van de entiteit en niet wanneer het bericht worden op het netwerk wordt geplaatst. Wanneer de client wordt aangegeven dat de gereedheid van berichten ontvangen via de koppeling krediet uitgifte, daarom wordt verwacht om te worden berichten actief trekken via het netwerk en klaar is om deze. Anders de bericht-lock is mogelijk verlopen voordat het bericht is zelfs bezorgd. Het gebruik van koppeling-krediet stroom besturingselement moet direct overeenkomen met de gereedheid van de direct te handelen beschikbare berichten die zijn verzonden naar de ontvanger.

In het overzicht vindt de volgende secties u een schema overzicht van de performative stroom tijdens verschillende API interacties. Elke sectie Beschrijving van een andere logische bewerking. Een deel van deze interacties mogelijk "fikse,' wat betekent dat ze kunnen alleen worden uitgevoerd eenmaal vereist. Maken van de afzender van een bericht mogelijk niet een netwerk wordt herhaald totdat het eerste bericht wordt verzonden of aangevraagd.

De pijlen geven de stroomrichting performative.

#### <a name="create-message-receiver"></a>Bericht ontvanger maken

| Client                                                                                                                                                | Service Bus                                                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------------------------------------    |--------------------------------------------------------------------------------------------------------------------------------------------   |
| --> bijvoegen ()<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke greep},<br/>rol =**ontvanger**,<br/>bron = {entiteitsnaam},<br/>TARGET = {client koppeling id}<br/>)         | Client die is gekoppeld aan entiteit als ontvanger                                                                                                         |
| Service Bus antwoorden einde van de koppeling toevoegen                                                                                                     | <--(als bijlage toevoegen<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke greep},<br/>rol =**afzender**,<br/>bron = {entiteitsnaam},<br/>TARGET = {client koppeling id}<br/>)       |

#### <a name="create-message-sender"></a>Afzender maken

| Client                                                                                                            | Service Bus                                                                                                           |
|------------------------------------------------------------------------------------------------------------------ |--------------------------------------------------------------------------------------------------------------------   |
| --> bijvoegen ()<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke greep},<br/>rol =**afzender**,<br/>bron = {client koppeling id},<br/>TARGET = {entiteitsnaam}<br/>)   | Geen actie                                                                                                                     |
| Geen actie                                                                                                                 | <--(als bijlage toevoegen<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke greep},<br/>rol =**ontvanger**,<br/>bron = {client koppeling id},<br/>TARGET = {entiteitsnaam}<br/>)     |

#### <a name="create-message-sender-error"></a>Afzender (fout) maken

| Client                                                                                                            | Service Bus                                                           |
|------------------------------------------------------------------------------------------------------------------ |---------------------------------------------------------------------  |
| --> bijvoegen ()<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke greep},<br/>rol =**afzender**,<br/>bron = {client koppeling id},<br/>TARGET = {entiteitsnaam}<br/>)   | Geen actie                                                                     |
| Geen actie                                                                                                                 | <--(als bijlage toevoegen<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke greep},<br/>rol =**ontvanger**,<br/>bron = null,<br/>TARGET = null-waarden<br/>)<br/><br/><--loskoppelen ()<br/>verwerken = {numerieke greep},<br/>gesloten =**waar**,<br/>Fout = {foutgegevens}<br/>)  |

#### <a name="close-message-receiversender"></a>Bericht sluiten ontvanger/afzender

| Client                                            | Service Bus                                       |
|-------------------------------------------------  |-------------------------------------------------  |
| --> loskoppelen ()<br/>verwerken = {numerieke greep},<br/>gesloten =**true**<br/>)    | Geen actie                                                 |
| Geen actie                                                 | <--loskoppelen ()<br/>verwerken = {numerieke greep},<br/>gesloten =**true**<br/>)    |

#### <a name="send-success"></a>Verzenden (Success)

| Client                                                                                                                        | Service Bus                                                                                           |
|------------------------------------------------------------------------------------------------------------------------------ |------------------------------------------------------------------------------------------------------ |
| --> doorverbinden (<br/>bezorging-id = {numerieke greep},<br/>bezorging-tag = {binaire greep},<br/>vereffend =**false**,, meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>)   | Geen actie                                                                                                     |
| Geen actie                                                                                                                             | <--() voor bestemming<br/>rol = ontvanger,<br/>eerst = {bezorging id},<br/>laatst = {bezorging id},<br/>vereffend =**waar**,<br/>status =**geaccepteerd**<br/>)   |

#### <a name="send-error"></a>Verzenden (fout)

| Client                                                                                                                        | Service Bus                                                                                                                   |
|------------------------------------------------------------------------------------------------------------------------------ |-----------------------------------------------------------------------------------------------------------------------------  |
| --> doorverbinden (<br/>bezorging-id = {numerieke greep},<br/>bezorging-tag = {binaire greep},<br/>vereffend =**false**,, meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>)   | Geen actie                                                                                                                             |
| Geen actie                                                                                                                             | <--() voor bestemming<br/>rol = ontvanger,<br/>eerst = {bezorging id},<br/>laatst = {bezorging id},<br/>vereffend =**waar**,<br/>status = (**geweigerd**<br/>Fout = {foutgegevens}<br/>)<br/>)     |

#### <a name="receive"></a>Ontvangen

| Client                                                                                                | Service Bus                                                                                                                   |
|------------------------------------------------------------------------------------------------------ |------------------------------------------------------------------------------------------------------------------------------ |
| --> stroom (<br/>koppeling-krediet = 1<br/>)                                                                                 | Geen actie                                                                                                                             |
| Geen actie                                                                                                     | < doorverbinden ()<br/>bezorging-id = {numerieke greep},<br/>bezorging-tag = {binaire greep},<br/>vereffend =**false**,<br/>meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>)     |
| --> () voor bestemming<br/>rol =**ontvanger**,<br/>eerst = {bezorging id},<br/>laatst = {bezorging id},<br/>vereffend =**waar**,<br/>status =**geaccepteerd**<br/>)   | Geen actie                                                                                                                             |

#### <a name="multi-message-receive"></a>Met meerdere berichten ontvangen

| Client                                                                                                    | Service Bus                                                                                                                       |
|--------------------------------------------------------------------------------------------------------   |--------------------------------------------------------------------------------------------------------------------------------   |
| --> stroom (<br/>koppeling-krediet = 3<br/>)                                                                                 | Geen actie                                                                                                                                 |
| Geen actie                                                                                                         | < doorverbinden ()<br/>bezorging-id = {numerieke greep},<br/>bezorging-tag = {binaire greep},<br/>vereffend =**false**,<br/>meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>)     |
| Geen actie                                                                                                         | < doorverbinden ()<br/>bezorging-id = {numerieke greep + 1},<br/>bezorging-tag = {binaire greep},<br/>vereffend =**false**,<br/>meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>)   |
| Geen actie                                                                                                         | < doorverbinden ()<br/>bezorging-id = {numerieke greep + 2},<br/>bezorging-tag = {binaire greep},<br/>vereffend =**false**,<br/>meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>)   |
| --> () voor bestemming<br/>rol = ontvanger,<br/>eerst = {bezorging id},<br/>laatst = {bezorging id + 2},<br/>vereffend =**waar**,<br/>status =**geaccepteerd**<br/>)     | Geen actie                                                                                                                                 |

### <a name="messages"></a>Berichten

De volgende secties wordt uitgelegd welke eigenschappen van de standaard AMQP bericht secties worden gebruikt door de Service Bus en hoe ze de officiële Service Bus-API's toewijzen.

#### <a name="header"></a>koptekst

| Veldnaam        | Gebruik                             | De naam van de API          |
|----------------   |-------------------------------    |---------------    |
| duurzame           | -                                 | -                 |
| prioriteit          | -                                 | -                 |
| TTL               | TTL voor dit bericht     | [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)     |
| eerste verwerver    | -                                 | -                 |
| bezorging tellen    | -                                 | [DeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.deliverycount.aspx)   |

#### <a name="properties"></a>Eigenschappen

| Veldnaam            | Gebruik                                                                                                                             | De naam van de API                                      |
|---------------------- |---------------------------------------------------------------------------------------------------------------------------------  |--------------------------------------------   |
| bericht-id            | Toepassingsspecifieke, vrije vorm-id voor dit bericht. Wordt gebruikt voor dubbele detectie.                                         | [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx)                                   |
| gebruikers-id               | Toepassingsspecifieke gebruikers-id, niet geïnterpreteerd door de Service Bus.                                                              | Niet toegankelijk via de Service Bus-API.   |
| Aan                    | Toepassingsspecifieke bestemming identificatie, niet geïnterpreteerd door de Service Bus.                                                       | [Aan](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.to.aspx)                                             |
| Onderwerp               | Toepassingsspecifieke bericht doel-id, niet geïnterpreteerd door de Service Bus.                                                   | [Label](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx)                                       |
| Antwoordadres              | Toepassingsspecifieke antwoord-pad indicator, niet geïnterpreteerd door de Service Bus.                                                         | [ReplyTo](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.replyto.aspx)                                       |
| correlatie-id        | Toepassingsspecifieke correlatie-id, niet geïnterpreteerd door de Service Bus.                                                       | [CorrelationId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.correlationid.aspx)                               |
| inhoudstype          | Toepassingsspecifieke inhoudstype indicator voor de hoofdtekst, niet geïnterpreteerd door de Service Bus.                                          | [ContentType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx)                                   |
| inhoud-codering      | Toepassingsspecifieke indicator coderen van inhoud voor de hoofdtekst, niet geïnterpreteerd door de Service Bus.                                      | Niet toegankelijk via de Service Bus-API.   |
| absolute verlooptijd  | Gedeclareerd aan welke absolute chatberichten het bericht verloopt. Genegeerd op invoer (kop ttl is viering), gezaghebbende op uitvoer.   | [ExpiresAtUtc](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.expiresatutc.aspx)                                 |
| Aanmaaktijd         | Gedeclareerd waarop het bericht is gemaakt. Niet door de Service Bus gebruikt                                                           | Niet toegankelijk via de Service Bus-API.   |
| groep-id              | Toepassingsspecifieke id voor een gerelateerde set van berichten. Wordt gebruikt voor sessies met Service Bus.                                      | [Sessie-id](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx)                                   |
| volgorde van de groep        | De teller identificerende het relatieve volgnummer van het bericht in een sessie. Door de Service Bus genegeerd.                         | Niet toegankelijk via de Service Bus-API.   |
| antwoord-naar-groep-id     | -                                                                                                                                 | [ReplyToSessionId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.replytosessionid.aspx)                             |

## <a name="advanced-service-bus-capabilities"></a>Geavanceerde mogelijkheden van de Service Bus

Deze sectie bevat geavanceerde mogelijkheden van Azure Service Bus die zijn gebaseerd op concept extensies voor AMQP die momenteel worden ontwikkeld in de technische Commissie OASIS AMQP. Azure-Service de meest recente status van deze concepten implementeert en wordt bij het gebruik van wijzigingen Als deze concepten standaard status hebt bereikt.

> [AZURE.NOTE] Een patroon aanvraag en respons Service Bus Messaging geavanceerde bewerkingen worden ondersteund worden beschouwd. De details van deze bewerkingen in het document worden beschreven [AMQP 1,0 in Service Bus: aanvraag/antwoord-bewerkingen](https://msdn.microsoft.com/library/azure/mt727956.aspx).

### <a name="amqp-management"></a>AMQP management

De specificatie AMQP Management is de eerste van de tekst concept-extensies die we hier bespreken. Deze specificatie van definieert een reeks protocol-bewegingen gelaagde boven aan het protocol AMQP waarmee management interacties met de SMS-infrastructuur via AMQP. De specificatie definieert algemene bewerkingen zoals *maken*, *lezen*, *bijwerken*en *verwijderen* voor het beheren van entiteiten in een SMS-infrastructuur en een reeks querybewerkingen.

Alle deze bewegingen vereisen een aanvraag en respons interactie tussen de client en de SMS-infrastructuur en kunnen daarom de specificatie wordt gedefinieerd hoe de interactie patroon boven aan de AMQP model: de client verbinding maakt met de SMS-infrastructuur, een sessie start en maakt vervolgens een paar koppelingen. Op een koppeling, de client fungeert als afzender en klik op de andere deze als ontvanger, waardoor een combinatie van koppelingen die als een kanaal bidirectionele kunnen fungeren.

| Logische bewerking            | Client                      | Service Bus                 |
|------------------------------|-----------------------------|-----------------------------|
| Aanvraag antwoord pad maken | --> bijvoegen ()<br/>naam = {*de koppelingsnaam van de*},<br/>verwerken = {*numerieke greep*},<br/>rol =**afzender**,<br/>bron =**null**,<br/>TARGET = "myentity / $management"<br/>)                            |Geen actie                             |
|Aanvraag antwoord pad maken                              |Geen actie                             | \<--(als bijlage toevoegen<br/>naam = {*de koppelingsnaam van de*},<br/>verwerken = {*numerieke greep*},<br/>rol =**ontvanger**,<br/>bron = null,<br/>TARGET = "myentity"<br/>)                            |
|Aanvraag antwoord pad maken                              | --> bijvoegen ()<br/>naam = {*de koppelingsnaam van de*},<br/>verwerken = {*numerieke greep*},<br/>rol =**ontvanger**,<br/>bron = "myentity / $management,"<br/>TARGET = "myclient$-id"<br/>)                            |                             |Geen actie
|Aanvraag antwoord pad maken                              |Geen actie                             | \<--(als bijlage toevoegen<br/>naam = {*de koppelingsnaam van de*},<br/>verwerken = {*numerieke greep*},<br/>rol =**afzender**,<br/>bron = "myentity",<br/>TARGET = "myclient$-id"<br/>)                            |

Lukt die combinatie van koppelingen op hun plaats staan, de aanvraag en respons-implementatie is duidelijke: een verzoek om een bericht verzonden naar een entiteit binnen de SMS-infrastructuur die dit patroon begrijpt is. Klik in het verzoek-bericht is het veld *antwoord* in de sectie *Eigenschappen* aan de *doel* -id voor de koppeling waarop u voor het leveren van het antwoord ingesteld. De afhandeling entity wordt verwerking van de aanvraag en klikt u vervolgens het antwoord geven op de koppeling waarvan *doel* -id overeenkomt met de opgegeven *antwoordadressen* -id.

Het patroon is duidelijk vereist dat de container client en de client gegenereerde id voor de bestemming antwoord uniek zijn op alle clients en, veiligheidsredenen worden ook moeilijk te voorspellen.

De uitwisseling van het bericht gebruikt voor het protocol management en voor alle andere protocollen die hetzelfde patroon gebruiken plaatsvinden op het toepassingsniveau van de; ze definieert geen nieuwe AMQP protocol niveau bewegingen. Dat is opzettelijk zodat toepassingen kunnen profiteren onmiddellijke van deze extensies met compatibele AMQP 1.0 stapels.

Azure-Service Bus momenteel implementeert een van de gewenste basisfuncties van de specificatie management, maar de aanvraag en respons patroon gedefinieerd door de specificatie management is moeten beschikken over voor de functie claims-beveiliging op basis van- en voor bijna alle de geavanceerde mogelijkheden die wordt in de volgende secties wordt besproken.

### <a name="claims-based-authorization"></a>Op claims gebaseerde verificatie

Het concept van de specificatie AMQP Claims-gebaseerd-verificatie (CBS) is gebaseerd op de specificatie van de aanvraag en respons patroon en wordt een algemene model voor het gebruik van federatieve beveiligingstokens met AMQP beschreven.

De standaard-beveiligingsmodel van AMQP besproken in de inleiding is gebaseerd op SASL en werkt naadloos samen met de AMQP verbinding handshake. Gebruik van SASL heeft het voordeel deze vindt u een extensible model waarvoor een set van regelingen van elk protocol dat formeel leans op SASL kan profiteren zijn gedefinieerd. Onder deze regelingen zijn 'PLATTE' voor de overdracht van gebruikersnamen en wachtwoorden, "Externe" binding maken met beveiliging op gebruikersniveau TLS, van 'Anoniem' Express gebrek aan expliciete verificatie/autorisatie en tal van aanvullende regelingen waarmee verificatie en/of autorisatiereferenties of tokens doorgeven.

Integratie van de AMQP SASL heeft twee nadelen:

-   Alle referenties en tokens zijn beperkt tot de verbinding. Een SMS-infrastructuur kunt gesplitste toegangsbeheer op basis van de per entiteit bieden. Bijvoorbeeld, zodat de drager van een token om te verzenden naar wachtrij A, maar niet aan de wachtrij b te drukken. Met de autorisatiecontext op de verbinding is gekoppeld, is het niet mogelijk slechts één verbinding en nog gebruiken verschillende access tokens voor wachtrij A en wachtrij b te drukken.

-   Access tokens zijn meestal alleen geldig gedurende een beperkte periode. Hiermee wordt de gebruiker om regelmatig terug tokens te gedwongen en biedt de mogelijkheid op token degene te weigeren uitgifte een vers token als de machtigingen van de gebruiker zijn gewijzigd. AMQP verbindingen mogelijk duurt erg lang perioden. Het model SASL alleen recht biedt een mogelijkheid om het instellen van een token verbinding keer, wat dat de SMS-infrastructuur is de client verbreken betekent wanneer de token is verlopen of deze moeten de kans op pesterijen toestaan voortdurende communicatie met een client accepteren wie heeft toegangsrechten is ingetrokken in de tussentijd.

De specificatie AMQP CBS geïmplementeerd door Bus van Azure-Service, kan een elegante tijdelijke oplossing voor beide van deze problemen: dit kan een client access tokens koppelen aan elk knooppunt en deze tokens bijwerken voordat ze verlopen, zonder te onderbreken de berichtenstroom.

CBS definieert beheer van virtuele knooppunt, met de naam *$cbs*, door de SMS-infrastructuur te verstrekken. Het knooppunt beheer accepteert tokens namens een andere knooppunten in de SMS-infrastructuur.

Het protocol beweging is een aanvraag/beantwoorden exchange zoals gedefinieerd door de specificatie management. Dat betekent dat de client tot stand brengt een paar koppelingen met het knooppunt *$cbs* en klikt u vervolgens een verzoek om te worden doorgegeven naar de koppeling voor uitgaande en wacht tot het antwoord naar de koppeling voor binnenkomende.

Het bericht heeft de volgende toepassingseigenschappen:

| Toets        | Optionele | Waardetype | De inhoud van waarde                             |
|------------|----------|------------|--------------------------------------------|
| bewerking  | Nee       | tekenreeks     | **opslag-token**                                |
| type       | Nee       | tekenreeks     | Het type van het token worden opgeslagen.            |
| naam       | Nee       | tekenreeks     | De "doelgroep", waarop de token van toepassing is. |
| vervaldatum | Ja      | tijdstempel  | De verlooptijd van het token.              |

De eigenschap *naam* geeft de entiteit waarmee het token gekoppeld worden moet. In de Service Bus is het pad naar de wachtrij of onderwerp/abonnement. De eigenschap *type* geeft aan wat het token type:

| Type token                      | Token beschrijving      | Hoofdtekst           | Notities                                                    |
|---------------------------------|------------------------|---------------------|----------------------------------------------------------|
| amqp:jwt                        | JSON Web Token (JWT)   | AMQP waarde (tekenreeks) | Nog niet beschikbaar.  |
| amqp:SWT                        | Eenvoudige Web Token (SWT) | AMQP waarde (tekenreeks) | Alleen ondersteund voor SWT tokens uitgegeven door AAD/ACS          |
| servicebus.Windows.NET:sastoken | Service Bus SA's Token  | AMQP waarde (tekenreeks) | -                                                        |

Tokens verlenen rechten. Service Bus weet drie fundamentele rechten: 'Verzenden' kunnen verzendt, "Beluisteren" kunnen ontvangen en "Beheren" entiteiten bewerken. SWT tokens uitgegeven door AAD/ACS expliciet opnemen die rechten als claims. Service Bus SA's tokens raadpleegt u regels voor de naamruimte of entiteit hebt geconfigureerd en deze regels worden geconfigureerd met rechten. Het token aanmeldt met de toets die is gekoppeld aan die regel dus zorgt ervoor dat de token express de desbetreffende rechten. Het token dat is gekoppeld aan een entiteit *opslag -* token gebruikt, wordt de verbonden client om te communiceren met de entiteit per token machtigen toestaan. Een koppeling waarop de client de rol van de *afzender neemt* is vereist de "verzenden" rechts op de rol van de *ontvanger* zijn vereist rechts "Beluisteren".

Het antwoordbericht heeft de volgende waarden in de *Eigenschappen van de servicetoepassing*

| Toets                | Optionele | Waardetype | De inhoud van waarde                    |
|--------------------|----------|------------|-----------------------------------|
| status-code        | Nee       | int        | HTTP-antwoordcode **[RFC2616]**. |
| Beschrijving van de status | Ja      | tekenreeks     | Beschrijving van de status.        |

De client kunt bellen *opslag-token* herhaaldelijk en voor elke entiteit in de SMS-infrastructuur. De tokens zijn beperkt tot de huidige-client en verankerd op de huidige verbinding, wat betekent dat de server worden eventuele behouden tokens neerzetten wanneer de verbinding uitvalt.

De huidige Service Bus-implementatie kunt CBS alleen in combinatie met de methode SASL 'Anoniem'. Een SSL/TLS-verbinding moet altijd bestaan voordat u de handshake SASL.

De anonieme om moet daarom worden ondersteund door de door u gekozen AMQP 1.0-client. Anonieme toegang betekent dat de eerste verbinding tot stand handshake, zoals voor het maken van de eerste sessie gebeurt zonder Service Bus u weet wie de verbinding maakt.

Nadat de verbinding en de sessie tot stand is gebracht, vermelding van de koppelingen bij de *$cbs* knooppunt en verzenden van de aanvraag *opslag-token* zijn de enige bewerkingen toegestaan. Een geldige token moet worden ingesteld met succes met behulp van een aanvraag *opslag-token* voor sommige entity-knooppunt binnen 20 seconden nadat de verbinding tot stand is gebracht, anders eenzijdig de verbinding wordt verbroken door Service Bus.

De client is vervolgens verantwoordelijk voor het bijhouden van token verlooptijd. Als een token verloopt, neerzetten Service Bus onmiddellijk alle koppelingen op de verbinding met de desbetreffende entiteit. U kunt dit voorkomen, kunt de client het token voor het knooppunt vervangen door een nieuwe record op elk gewenst moment via het virtuele *$cbs* management knooppunt met de dezelfde *opslag-token* beweging en zonder weg staat het verkeer nettolading die op verschillende koppelingen doorloopt ophalen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over AMQP, raadpleegt u de volgende koppelingen:

- [Overzicht van de service Bus AMQP]
- [AMQP 1.0-ondersteuning Service partitioneren wachtrijen en onderwerpen]
- [AMQP in Service Bus voor Windows Server]

[Deze video cursus]: https://www.youtube.com/playlist?list=PLmE4bZU0qx-wAP02i0I7PJWvDWoCytEjD
[1]: ./media/service-bus-amqp/amqp1.png
[2]: ./media/service-bus-amqp/amqp2.png
[3]: ./media/service-bus-amqp/amqp3.png
[4]: ./media/service-bus-amqp/amqp4.png

[Overzicht van de service Bus AMQP]: service-bus-amqp-overview.md
[AMQP 1.0-ondersteuning Service partitioneren wachtrijen en onderwerpen]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP in Service Bus voor Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx