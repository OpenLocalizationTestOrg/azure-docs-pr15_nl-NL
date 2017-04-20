<properties
   pageTitle="Lidmaatschap microservices | Microsoft Azure"
   description="Een overzicht van waarom cloud toepassingen maken met een methode microservices belangrijk is voor de ontwikkeling van moderne toepassingen en hoe stof van Azure-Service biedt een platform hiervoor"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="mfussell"/>

# <a name="why-a-microservices-approach-to-building-applications"></a>Waarom wordt een microservices benaderen tot het bouwen van toepassingen?
Als softwareontwikkelaars is er er nieuw in hoe we nadenken waarbij een toepassing in delen opsplitsen onderdeel niets. Dit is de centrale model object afdrukstand, software abstracties en componentization. Deze factoriseren vaak tegenwoordig kunnen de vorm van klassen en interfaces tussen gedeelde bibliotheken en technologie lagen. Meestal een gegevensbescherming is die u hebt gemaakt met een back-enddatabase store, middelste laag bedrijfslogica en een front UI. De laatste jaren wat *heeft* gewijzigd, is dat we, ontwikkelaars, gedistribueerde toepassingen voor de cloud samenstelt, als gevolg van het bedrijf.

De veranderende zakelijke behoeften zijn:

- Maken en gebruiken van een service bij het op schaal om te kunnen klanten in nieuwe geografische regio's (bijvoorbeeld) te bereiken.
- Sneller bezorging van functies en mogelijkheden kunnen reageren op vraag van klanten in een agile manier.
- Verbeterde Resourcegebruik kosten te verlagen.

Deze zakelijke behoeften zijn van invloed op *hoe* we toepassingen maken.

Lees voor meer informatie over van Azure aanpak microservices, [Microservices: een toepassing revolutie mogelijk gemaakt door de cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="monolithic-vs-microservice-design-approach"></a>Monolithische versus microservice ontwerp methode
Alle toepassingen ontwikkelen na verloop van tijd. Succesvolle toepassingen ontwikkelen doordat nuttig zijn voor personen. Niet geslaagd toepassingen niet doen ontwikkelen en uiteindelijk zijn afgeschaft. De volgende vraag: hoeveel u weet over uw vereisten voor vandaag en wat ze worden in de toekomst? Stel dat u maakt een rapportage-toepassing voor een afdeling. U weet zeker dat de toepassing binnen het bereik van uw bedrijf blijft en dat de rapporten tijdelijk. Uw keuze van aanpak is anders dan, bijvoorbeeld bouwen van een service om video-inhoud aan tientallen miljoenen klanten te bieden. Soms is iets de deur uit als aankoopbewijs concept aan de drijvende factor, met de kennis dat de toepassing later kan worden aangepast. Er is weinig punt in iets te weinig engineering die nooit wordt gebruikt. De gebruikelijke technische verhouding is. Wanneer bedrijven over building voor de cloud praten, is de verwachting aan de andere kant groei en het gebruik. Het probleem is dat groei en schaal onverwachte zijn. Willen we kunnen prototype snel terwijl u ook weten dat we op een pad zijn te handelen succes in de toekomst. Dit is de methode lean opstarten: maken, meten, leer, doorgaan met het ontwikkelen.

Tijdens de era client / server-doorgaans we richten op het bouwen van trapsgewijze toepassingen met behulp van specifieke technologieën in elke laag. De term "monolithische" toepassing geworden voor de volgende manieren. De interfaces doorgaans tussen de lagen en een meer nauw verbonden ontwerp gebruikt tussen onderdelen binnen elke laag. Ontwikkelaars ontworpen en meegenomen klassen gecompileerd in bibliotheken, aan elkaar zijn gekoppeld in een paar uitvoerbare bestanden en dll-bestanden. Zijn er voordelen, zoals een monolithisch ontwerp-methode. Dit is het meestal eenvoudiger te ontwerpen en sneller oproepen tussen onderdelen, omdat deze oproepen vaak overschreden IPC is heeft. Ook iedereen één product, doorgaans meer personen-resource efficiënt worden getest. Het nadeel is dat een contour koppeling tussen doorverbonden lagen resultaten en u kunt geen dat afzonderlijke onderdelen schalen. Als u uitvoeren wilt of upgrades van reparaties, er moet wachten om door anderen mogen klaar bent met hun testen. Is het moeilijker moeten agile.

Microservices adres deze nadelen en meer nauw uitlijnen met de voorgaande vereisten voor bedrijven, maar ze ook zowel upsides- en nadelen hebt. De voordelen van het microservices zijn dat elk meestal eenvoudiger business-functionaliteit, waardoor u schaal omhoog of omlaag, test, implementeren en afzonderlijk beheren omvat. Een belangrijk voordeel van een benadering microservice is dat teams nog veel meer met bedrijfsscenario's dan worden gestuurd door technologie, die de gegevensbescherming aanmoedigt. In de praktijk ontwikkel kleinere teams een microservice op basis van een klant-scenario, met behulp van een technologieën die zij kiezen. De organisatie hoeft geen met andere woorden, te normaliseren tech voor het behoud van monolithische toepassingen. Afzonderlijke teams die eigen services doen kunnen wat relevant is voor hen op basis van een team expertise of wat is het meest geschikt zijn voor het probleem moet worden opgelost. In Word Web App, dat ze een aanbevolen technologieën, zoals een bepaalde NoSQL store of web verdient application framework.

Het nadeel van microservices wordt geleverd in het verbeterde aantal afzonderlijke entiteiten beheren en omgaan met meer complexe implementaties en versiebeheer. Netwerkverkeer tussen de microservices Hiermee verhoogt u en de bijbehorende netwerkvertragingen. Een groot aantal chatty lukt, gedetailleerde services is een recepten voor een nachtmerrie prestaties. Zonder hulpmiddelen voor het help-weergave de doelcellen is moeilijk te "zien" het hele systeem. Standaarden zijn wat de methode microservice zorg werk door afspreken hoe om te communiceren en tolerantie voor alleen de dingen die u nodig met een service hebt wordt in plaats van harde contracten. Het is belangrijk op basis van deze contactpersonen kunt vooraf in het ontwerp, aangezien services onafhankelijk van elkaar bijwerken. Een andere beschrijving lang geleden bedacht voor het ontwerpen met een methode microservices is "fijnmazige SOA."


***De eenvoudigste is de benadering van de ontwerpen microservices over een losgekoppelde Federatie van services, met onafhankelijke wijzigingen aan elk en vooraf vastgestelde standaarden voor communicatie.***


Als u meer cloud-apps worden geproduceerd, ontdekken personen dat deze uitgevouwen van de algehele app in onafhankelijke, scenario's gebaseerde services een betere langere benadering is.

## <a name="comparison-between-application-development-approaches"></a>Aspecten van toepassing ontwikkeling methoden vergeleken

![De ontwikkeling van een service stof platform-toepassingen][Image1]

1. Een monolithisch app domein / regiospecifieke functionaliteit bevat en normaal wordt gedeeld door functionele lagen, zoals web, business en gegevens.

2. U Schalen een monolithisch app door deze op meerdere servers/VMs/containers klonen.

3. Een toepassing voor de microservice worden gescheiden door functionaliteit in afzonderlijke kleinere services.

4. Deze methode wordt aangepast af door te implementeren elke service belooft exemplaren van de volgende services over VMs-servers/containers maken.


Ontwerpen met een microservice benadering is niet een wondermiddel voor alle projecten, maar deze nauwer Hiermee wordt uitgelijnd met de bedrijfsdoelstellingen hierboven. Beginnen met een monolithisch aanpak mogelijk acceptabel als u weet dat later kunt u geen de mogelijkheid om de code in het ontwerp van een microservice Herschrijf indien nodig. Vaker voorkomt, begint met een monolithisch app en langzaam splits deze in fasen, beginnend met de functiegebieden die moeten worden meer scalable of agile.

Als u wilt samenvatten, is de methode microservice om op te stellen van de toepassing van veel kleinere services.  De services worden uitgevoerd in een cluster van machines geïmplementeerd containers. Kleinere teams ontwikkelen van een service die is gericht op een scenario onafhankelijk testen, versie, implementeren en de schaal van elke service aanpassen zodat de toepassing als geheel kunt ontwikkelen.

## <a name="what-is-a-microservice"></a>Wat is een microservice?

Er zijn verschillende definities van microservices en zoeken op Internet biedt veel goede bronnen die hun eigen opzicht en definities geven. Echter zijn grootste deel van de volgende kenmerken van microservices sterk uiteen akkoord gaat:

- Een scenario klant of onderbrengen. Wat is het probleem dat u het oplossen van?
- Ontwikkeld door een klein team voor technische.
- Geschreven in elke programmeertaal en eventuele framework gebruiken.
- Bestaan uit de code en (optioneel) provinciale, die beide zijn afzonderlijk bijgehouden, geïmplementeerd en schaal.
- Werken met andere microservices via goed gedefinieerde interfaces en protocollen.
- Hebt u een unieke naam (URL's) gebruikt voor het oplossen van hun locatie.
- Blijven consistente en beschikbaar zijn in de aanwezigheid van fouten.

U kunt deze kenmerken in samenvatten:

***Microservice toepassingen bestaan uit kleine, onafhankelijk versienummer en scalable klant gerichte services die met elkaar via standaard protocollen met duidelijke interfaces communiceren.***


We de eerste twee punten in de vorige sectie behandeld en nu we uitvouwen op en de andere verduidelijken.

### <a name="written-in-any-programming-language-and-use-any-framework"></a>Geschreven in elke programmeertaal en gebruiken van een framework
Ontwikkelaars moeten we gratis om te kiezen welke taal of framework we, afhankelijk van onze vaardigheden of de behoeften van de service horen. In sommige services, kunt u de prestatievoordelen van C++ boven alle nog meer waarde. In andere services zijn het gemak van beheerde ontwikkelen in C# of Java belangrijkst. In sommige gevallen moet u mogelijk een specifieke bibliotheek van derden, technologie voor gegevensopslag of betekent dat de service aan clients gebruiken.

Als u een technologie, u keert u naar de operationele of een beperkte levensduur management en schaalbaarheid van de service hebt gekozen.

### <a name="allows-code-and-state-to-be-independently-versioned-deployed-and-scaled"></a>Kunt u code en staat om te worden afzonderlijk bijgehouden, geïmplementeerd en schaal  

Echter u kiezen om te schrijven van uw microservices, de code en u kunt ook de status moeten onafhankelijk implementeren, upgraden en schaal. Dit is daadwerkelijk een van de moeilijker problemen op te lossen, aangezien het gaat omlaag om uw keuze van technologieën. Voor schaal, begrijpen hoe u partition (of shard) zowel de code als de status is lastige. Wanneer de code en gebruikt afzonderlijke technologieën (dat ze vaak vandaag doen), moeten de implementatie-scripts voor uw microservice kunnen omgaan met schaalbaarheid van beide. Dit is ook over flexibiliteit en flexibiliteit, zodat u enkele van de microservices bijwerken kunt zonder dat ze allemaal in één keer bijwerken.
Terugkeren naar de monolithische versus microservice aanpak even, ziet in het volgende diagram u de verschillen tussen de methode die u wilt opslaan status.

#### <a name="state-storage-between-application-styles"></a>Provinciale opslag tussen de stijlen van toepassing
![Service stof platform staat opslag][Image2]

***Is de monolithische wijze, met één database en lagen van specifieke technologieën aan de linkerkant.***

***Is de microservices benadering van de onderling verbonden microservices waar staat meestal is beperkt tot de microservice en verschillende technologieën worden gebruikt in een grafiek aan de rechterkant.***

In een monolithisch aanpak, meestal is één database door de toepassing gebruikt. Het voordeel is dat dit een één locatie is, zodat u gemakkelijk kunt implementeren. Elk onderdeel kan één tabel voor de opslag van de status hebben. Teams moeten worden strikte in staat, die een uitdaging scheiden. Er zijn onvermijdelijk temptations moet u een nieuwe kolom toevoegen aan een bestaande klantentabel, een join tussen tabellen doen en afhankelijkheden bij de opslaglaag maken. Na afloop niet kunt u afzonderlijke onderdelen schalen. In de methode microservices, elke service worden beheerd en wordt een eigen status opgeslagen. Elke service is verantwoordelijk voor schaalbaarheid van zowel de code als de status samen om te voldoen aan de vereisten van de service. Een nadeel is dat wanneer er een hoeft te maken van weergaven of query's, met de gegevens van uw toepassing moet u query met betrekking tot verschillende staat winkels. Meestal wordt dit opgelost doordat een aparte microservice die een weergave in een verzameling microservices maakt. Als u meerdere ad-hoc-query's uitvoeren op de gegevens moet, elke microservice rekening moet houden bij het schrijven van de gegevens in een service voor offline analytics voor gegevensopslag.

Versiebeheer specifiek is voor de gebruikte versie van een microservice dus die meerdere, verschillende versies implementeren en elkaar uitvoeren. Versiebeheer adressen scenario's waarin een nieuwere versie van een microservice mislukt tijdens de upgrade en moet terugkeren naar een eerdere versie. Het andere scenario voor versiebeheer wordt uitgevoerd A/B-stijl testen, waarbij verschillende gebruikers ondervindt met verschillende versies van de service. Dat is bijvoorbeeld gebruikelijk een microservice voor een specifieke reeks klanten om te testen van nieuwe functionaliteit alvorens meer sterk uiteen upgrade uit te voeren. Na het beheer van de levenscyclus van microservices kunt dit nu u ons communicatie ertussen.


### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>Samenwerkt met andere microservices via goed gedefinieerde interfaces en protocollen

In dit onderwerp aandacht weinig hier, omdat er uitgebreide documentatie op service gerichte architectuur die zijn gepubliceerd in de afgelopen 10 jaar waarin wordt beschreven communicatiepatronen. Service communicatie gebruikt in het algemeen wordt een aanpak REST met HTTP- en TCP protocollen en XML- of JSON als de serialisatie-indeling. Vanuit een perspectief interface wordt omvat onderdelen die de web-ontwerp-methode. Maar natuurlijk u binaire protocollen of uw eigen gegevensindelingen te gebruiken. Het voorbereid van personen een moeilijker tijd met behulp van uw microservices als deze openlijk beschikbaar zijn zijn.

### <a name="has-a-unique-name-url-used-to-resolve-its-location"></a>Heeft een unieke naam (URL) gebruikt voor het oplossen van de locatie

Onthouden hoe houden we zeggen dat de methode microservice lijkt op het web? Als het web moet uw microservice worden gebruikt, waar deze wordt uitgevoerd. Als u zijn nadenkt over machines en welke een bepaalde microservice wordt uitgevoerd, wordt alles snel ongeldige soepel. Op dezelfde manier dat er een bepaalde URL op DNS wordt omgezet in een bepaalde machine, moet uw microservice een unieke naam hebben zodat de huidige locatie gedetecteerd wordt. Microservices moet gebruikt namen die deze los van de infrastructuur waarop ze op te maken. Dit betekent dat er een interactie tussen hoe uw service wordt geïmplementeerd en hoe deze wordt ontdekt is, aangezien er moet een service-register. Even, wanneer een machine het register mislukt service moet ziet u waar de service nu wordt uitgevoerd. Hiermee ons naar het volgende onderwerp: flexibiliteit en consistentie.

### <a name="remains-consistent-and-available-in-the-presence-of-failures"></a>Consistente en beschikbaar zijn in de aanwezigheid van fouten blijft

Omgaan met onverwachte fouten is een van de moeilijkst problemen op te lossen, met name in een gedistribueerd systeem. Veel van de code die we als ontwikkelaars schrijven uitzonderingen verwerkt en dit is ook waar de meeste tijd is besteed aan testen. Maar het probleem dat is minder snel dan het schrijven van code voor het verwerken van fouten. Wat gebeurt er wanneer u de computer waarop de microservice is uitgevoerd mislukt? Niet alleen moet u het detecteren van deze fout microservice (een vaste probleem zelf op), maar moet u ook een ander nummer om uw microservice opnieuw te starten. Een microservice moet worden robuuste op fouten en opnieuw vaak op een andere computer om beschikbaarheid redenen. Dit wordt ook geleverd naar beneden af op welke staat namens de microservice is opgeslagen, waar kan deze herstellen deze status van en of u het is wel opnieuw te starten. Met andere woorden, moet er flexibiliteit in de berekeningscluster (het proces opnieuw opgestart), evenals flexibiliteit in het land of de gegevens (geen gegevens verloren gaan en de gegevens consistent blijft).

De problemen van tolerantie zijn samengestelde tijdens andere scenario's, zoals wanneer fouten tijdens de upgrade van een toepassing optreden. De microservice, werken met de implementatie-systeem, hoeft niet te herstellen. Dit moet ook Bepaal of u kunt doorgaan naar voren verplaatsen naar de nieuwere versie of in plaats daarvan terugkeren naar een vorige versie om een consistente status te behouden. Vragen, zoals of er voldoende machines die beschikbaar zijn voor het doorschakelen zwevend behouden en het herstellen van eerdere versies van de microservice nodig meegerekend. U moet hiervoor de microservice systeemstatus informatie om deze beslissingen te kunnen verzenden.

### <a name="reports-health-and-diagnostics"></a>Rapporten gezondheid en diagnostische gegevens

Het kan lijken duidelijke, en deze wordt vaak vergeten, maar het is essentiële dat een microservice de gezondheid en hulpprogramma's voor diagnose rapporten. Er is anders weinig inzicht vanuit een perspectief bewerkingen. Diagnostische gebeurtenissen voor een reeks onafhankelijke services correleren en omgaan met machine klok laat om inzicht te krijgen van de volgorde van de gebeurtenis is lastige. Op dezelfde manier als u ermee in een microservice via vooraf vastgestelde en gegevensindelingen, verschijnt er behoefte standaardisatie in het registreren van gezondheid en diagnostische gebeurtenissen die uiteindelijk is terechtkomen in een winkel gebeurtenis voor query's uitvoeren en weergeven. In een aanpak microservices, is deze toets dat de verschillende teams een notatie één logboekregistratie overeenkomen.  Er moet een consistente manier voor het weergeven van diagnostische gebeurtenissen in de toepassing als geheel.

Servicestatus verschilt van diagnostische gegevens. Servicestatus gaat over het microservice rapportage van de huidige status naar de juiste acties uitvoeren. Een goed voorbeeld werkt samen met de upgrade en implementatie regelingen beschikbaar. Een service mogelijk momenteel beschadigd vanwege proces vastlopen of van de computer opnieuw opstarten, maar nog steeds operationele. Het laatste wat dat u nodig hebt, is Maak deze slechter door een upgrade uitvoert. De beste manier is een onderzoek doet u als eerste, of om tijd voor de microservice herstellen toe. Servicestatus gebeurtenissen van een microservice kunnen we beslissingen nemen en, in feite te maken van zelfreparerende services.

## <a name="service-fabric-as-a-microservices-platform"></a>Service stof als een microservices-platform

Azure-Service stof ontstaan afmelden bij de overgang van Microsoft van het vak producten die meestal monolithische stijl zijn, aan het geven van de services te bieden. De ervaring van bouwen en besturingssysteem grote services, zoals SQL Azure-databases en DocumentDB, in de vorm van Service stof. Het platform die is voortgekomen na verloop van tijd meer services aangenomen deze. Is van belang dat Service stof had om uit te voeren op elke locatie: niet alleen in Azure wordt aangegeven, maar ook in de zelfstandige versie van Windows Server implementaties.

***Het doel van Service stof is voor het oplossen van de harde problemen van bouwen en uit te voeren van een service en infrastructuur bronnen efficiënt, gebruiken zodat teams zakelijke problemen bij het gebruik van een benadering microservices kunnen oplossen.***

Service stof biedt twee globaal gebieden te maken van toepassingen met een methode microservices:

- Een platform waarmee de systeemservices te implementeren, upgrade, detecteren en opnieuw starten is mislukt services, ontdekken locatie, staat beheren en servicestatus bewaken. Deze systeemservices kunnen van kracht ook veel van de kenmerken van microservices die eerder is beschreven.

-  API's hoeft te programmeren of kaders, waarmee u kunt maken van toepassingen als microservices: [betrouwbare betrokkenen en betrouwbare services](service-fabric-choose-framework.md). Daarnaast kunt u een code van uw keuze maken van uw microservice. Controleer deze API's van de taak eenvoudiger maar ze integreren met het platform op een grondigere niveau. Op deze manier kunt u gezondheid en diagnostische informatie ophalen bijvoorbeeld of u kunt profiteren van ingebouwde beschikbaarheid.

***Service stof agnostische op hoe u uw service samenstellen is en u kunt een technologie wilt gebruiken. Echter biedt ingebouwde programming API's die het gemakkelijker maken microservices.***

### <a name="are-microservices-right-for-my-application"></a>Microservices zijn geschikt voor mijn toepassing?

Negeren. Wat wordt er is veel van deze de voordelen van een benadering microservice-achtige het duurt mogelijk zoals meer teams in Microsoft Start is gegaan maken voor de cloud voor werk te doen, gerealiseerd. Bing, bijvoorbeeld heeft zijn de ontwikkeling van microservices in jaren zoeken. Voor andere teams is de methode microservices nieuwe. Ze vinden die er moeilijk problemen voor het oplossen van buiten hun core gebieden van kracht zijn. Daarom Service stof ervaring tractie als de technologie van keuze voor het samenstellen van services.

Het doel van de Service stof is verkleinen van de complexiteit van toepassingen maken met een methode microservice, zodat er geen verwerkt als redesigns veel dure. Kleine beginnen, schalen wanneer nodig hebt, afschaffen services, nieuwe toevoegen en tijdens het gebruik van de klant--die de methode is aangepast. We weten ook dat in feite er vele andere problemen voordoen tijdens nog moet worden opgelost zodat microservices meer toegankelijk voor de meeste ontwikkelaars. Containers en het programmeren acteur-model zijn voorbeelden van kleine stappen in deze richting en we zijn ervoor dat meer innovaties markt worden gebracht te vergemakkelijken.
 
<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

* Voor meer informatie:
    * [Overzicht van de service stof terminologie](service-fabric-technical-overview.md)
    * [Microservices: Een toepassing revolutie mogelijk gemaakt door de cloud](https://azure.microsoft.com/en-us/blog/microservices-an-application-revolution-powered-by-the-cloud/)


[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
