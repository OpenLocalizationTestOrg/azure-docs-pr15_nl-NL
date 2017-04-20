<properties
   pageTitle="Overzicht van de Service stof betrouwbare Service programming model | Microsoft Azure"
   description="Meer informatie over de Service stof betrouwbare Service programming model en aan de slag voor het schrijven van uw eigen services."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor="vturecek; mani-ramaswamy"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/25/2016"
   ms.author="masnider;vturecek"/>

# <a name="reliable-services-overview"></a>Betrouwbare Services-overzicht
Azure-Service stof eenvoudiger schrijven en stateless en statuscontrole betrouwbare Services beheren. In dit document wordt nader bekeken:

- Het model voor programming betrouwbare Services voor stateless en statuscontrole services.
- De opties die u hebt om bij het schrijven van een betrouwbare Service.
- Sommige scenario's en voorbeelden van betrouwbare Services gebruiken en hoe deze worden geschreven.

Betrouwbare Services is een van de programming modellen beschikbaar op stof Service. Voor meer informatie over het programmeren model betrouwbare betrokkenen, Zie [Inleiding tot Service stof betrouwbare betrokkenen](service-fabric-reliable-actors-introduction.md).

In Service stof, een service bestaat uit de configuratie, toepassingscode, en bepaalt (optioneel).

Service stof beheert de levensduur van services, vanaf inrichting en de implementatie via upgrade en verwijderen via [Service stof Toepassingsbeheer](service-fabric-deploy-remove-applications.md).

## <a name="what-are-reliable-services"></a>Wat zijn betrouwbare Services?
Betrouwbare Services kunt u een eenvoudige, krachtige, op het hoogste niveau programming model kunt express wat belangrijk voor uw toepassing is. Met de betrouwbare Services programming model, krijgt u:

- Voor statuscontrole services kunt het programmeren model betrouwbare Services u consistente en betrouwbaar uw rechts staat binnen uw service opslaan met behulp van betrouwbare verzamelingen. Dit is een eenvoudige reeks ten zeerste beschikbaar siteverzameling klassen die vertrouwd zijn voor iedereen die C#-verzamelingen heeft gebruikt. Services vereist traditioneel externe systemen voor betrouwbare staat management. Met betrouwbare siteverzamelingen, kunt u uw status naast uw berekeningscluster opslaan met de dezelfde hoge beschikbaarheid en betrouwbaarheid die u hebt zou verwachten van externe winkels ten zeerste beschikbaar, en de extra latentie verbeteringen waarmee de berekeningscluster en staat samen te zoeken.

- Een eenvoudige model voor het uitvoeren van uw eigen code die eruitziet programming modellen die u vaak gebruikt. Uw code heeft een duidelijke ingangspunt en de levenscyclus van de eenvoudig te beheren.

- Een communicatiemodel pluggable. Gebruik het transport van uw keuze, zoals HTTP met [Web API](service-fabric-reliable-services-communication-webapi.md), WebSockets, aangepaste TCP-protocollen, enzovoort. Betrouwbare Services bieden bepaalde uitstekende out-van-het-box-opties die u kunt gebruiken of u kunt uw eigen.

## <a name="what-makes-reliable-services-different"></a>Wat kunt u verschillende betrouwbare Services?
Betrouwbare Services in Service stof verschilt van de services die u hebt gemaakt voordat. Service stof biedt betrouwbaarheid, beschikbaarheid van de consistentie en schaalbaarheid.  

- **Betrouwbaarheid**--uw service blijven zelfs in onbetrouwbare omgevingen waar uw machines mogelijk mislukt of druk op netwerkproblemen.

- **Beschikbaarheid**--uw service zijn bereikbaar en heeft gereageerd. (Dit betekent niet dat er geen services die niet kunnen worden gevonden of kan worden bereikt vanaf de buiten.)

- **Schaalbaarheid**--Services zijn ontkoppelde van specifieke hardware en ze kunnen vergroten of verkleinen desgewenst tot en met het toevoegen of verwijderen van hardware of virtuele bronnen. Services zijn eenvoudig partitioneren (met name in de statuscontrole hoofdletters/kleine letters) om ervoor te zorgen dat onafhankelijke gedeelten van de service kunnen schaal en fouten onafhankelijk beantwoorden. Tot slot aanmoedigt Service stof services worden lightweight doordat u duizenden services worden ingericht in één proces, kunt in plaats van vereisen of hele OS exemplaren uitsluitend naar een enkel exemplaar van een bepaalde werkbelasting.

- **Consistentie**--de gegevens op deze service kan worden gegarandeerd consistent (geldt alleen voor statuscontrole services: meer informatie over dit later)

## <a name="service-lifecycle"></a>Levenscyclus van de service
Of uw service statuscontrole of stateless, bieden betrouwbare Services de levensduur van een eenvoudige, waarmee u snel uw code sluit en aan de slag.  Zijn er maar één of twee methoden die u nodig hebt om uw service bedrijf te implementeren.

- **CreateServiceReplicaListeners/CreateServiceInstanceListeners** - dit is waar de communicatie-stack die moeten worden gebruikt door de service wordt gedefinieerd. De stapel communicatie, zoals [Web API](service-fabric-reliable-services-communication-webapi.md), is wat het luisteren eindpunt of de eindpunten voor de service (hoe clients, komt dit) definieert. Ook wordt gedefinieerd hoe de berichten die worden weergegeven uiteindelijk interactief werken met de rest van de servicecode.

- **RunAsync** - dit is waar de bedrijfslogica in uw service wordt uitgevoerd. De annulering token die is opgegeven, is een signaal voor wanneer die kunnen worden geopend moet worden beëindigd. Bijvoorbeeld, hebt u een service die moet worden voortdurend berichten afmelden bij een betrouwbare wachtrij halen en deze te verwerken, is dit waar die kunnen worden geopend gebeurt.

### <a name="service-startup"></a>Service is gestart

De belangrijkste momenten in de levenscyclus van een betrouwbare Service zijn:

1. De serviceobject (het object dat is afgeleid van de stateless service of statuscontrole service) wordt opgesteld.

2. De `CreateServiceReplicaListeners` / `CreateServiceInstanceListeners` methode wordt aangeroepen, waarin de service om terug te keren communicatie listeners van een of meer van de keuze.
  - Houd er rekening mee dat dit optioneel, is Hoewel de meeste services sommige eindpunt rechtstreeks worden.

3. Nadat u de communicatie listeners hebt gemaakt, wordt het geopend.
  - Communicatie listeners hebben een methode genoemd `OpenAsync()`, die op dit moment wordt genoemd, en die resulteert in het luisteren adres voor de service. Als een van de ingebouwde ICommunicationListeners uw betrouwbare Service wordt gebruikt, is dit voor u verwerkt.

4. Als u de communicatie luisteraar ervan af hebt geopend, de `RunAsync()` methode voor de belangrijkste service wordt genoemd.
  - Houd er rekening mee dat `RunAsync()` is optioneel. Als de service alle bijbehorende werk rechtstreeks in antwoord op de gebruiker alleen telefoongesprekken is, is niet nodig voor deze willen implementeren `RunAsync()`.

### <a name="service-shutdown"></a>Service afsluiten

Wanneer de service wordt afgesloten (om te worden verwijderd, bijgewerkte of verplaatste) de volgorde van de oproep wordt gespiegeld weergegeven: eerst de annulering token waarover `RunAsync()` is geannuleerd. vervolgens `CloseAsync()` voor de communicatie listeners wordt aangeroepen.

Er zijn enkele belangrijke punten onthouden over afsluiten voor statuscontrole services:

- Service stof wordt niet een andere replica van uw service aan primaire statusdatum tot promoveren `CloseAsync` en `RunAsync` hebt geretourneerd. Als u een ingebouwde communicatie luisteraar ervan af, gebruikt de `CloseAsync` methode voor u is verwerkt.

- Hoewel er geen tijdslimiet op uit de volgende manieren retourneren, kunt u direct kwijtraakt de mogelijkheid om naar betrouwbaar verzamelingen te schrijven en kunnen daarom niet voltooien van een werkelijke hoeveelheid werk. Het wordt aanbevolen dat u hiernaar zo snel mogelijk terugkeert bij ontvangst van de aanvraag is geannuleerd.

## <a name="example-services"></a>Voorbeeld services
U weet dit programming model, eens een kort overzicht van de verschillende services van de twee om te zien hoe deze stukken elkaar passen.

### <a name="stateless-reliable-services"></a>Stateless betrouwbare Services
Een stateless service is waar u één letterlijk geen provincie is onderhouden binnen de service of de provincie waarvan aanwezig is geheel beschikbare is en geen nodig synchronisatie, herhaling, permanente of beschikbaarheid.

Stel een rekenmachine die geen geheugen heeft en alle voorwaarden en bewerkingen uitvoeren in één keer ontvangt.

In dit geval kan de RunAsync() van de service niet leeg zijn, omdat er geen achtergrond taak-bewerkingen die de service moet doen. Wanneer de Rekenmachine-service is gemaakt, wordt een ICommunicationListener (bijvoorbeeld [Web API](service-fabric-reliable-services-communication-webapi.md)) dat wordt geopend een luisteren eindpunt op sommige poort geretourneerd. Dit luisteren eindpunt wordt verbinden met de verschillende methoden (voorbeeld: "Toevoegen (n1, n2)") die de berekening openbare API definiëren.

Wanneer een oproep wordt uitgevoerd van een client, de gewenste methode is geactiveerd en de service Rekenmachine de bewerkingen van de gegevens die worden uitgevoerd en geeft als resultaat. Elke staat worden niet opgeslagen.

Niet opslaat een interne staat, kunt u in dit voorbeeld Rekenmachine heel eenvoudig. Maar de meeste services zijn niet echt stateless. In plaats daarvan extern ze hun status naar enkele andere store. (Bijvoorbeeld een WebApp die afhankelijk zijn van om te houden sessie in een back-ups store of cache is niet volledig stateless.)

Een voorbeeld van hoe stateless services worden gebruikt de Service stof is als een front dat toegang biedt tot de API openbare voor een webtoepassing. De front-service vervolgens gesprekken voert statuscontrole services om een gebruikersaanvraag te voltooien. In dit geval worden aanroepen van clients doorgestuurd naar een bekende poort, zoals 80, waar de stateless service luistert. Deze stateless service de oproep ontvangt, en bepaalt u of het gesprek van een vertrouwde partij is, als u ook als welke service bestemd voor.  Vervolgens de stateless service stuurt de oproep door naar de juiste partition van de statuscontrole service en wachten op antwoord. Wanneer de stateless service een antwoord ontvangt, wordt deze antwoorden terug naar de oorspronkelijke client.

### <a name="stateful-reliable-services"></a>Statuscontrole betrouwbare Services
Een statuscontrole service is een waarde die een gedeelte van staat consistente en presenteren in volgorde voor de service-functie bewaard moet. Houd rekening met een service die voortdurend berekent een huidig gemiddelde van een waarde op basis van deze ontvangt updates. Hiervoor moet de huidige set verzoeken voor oproepen die proces, evenals de huidige gemiddelde moet hebben. Elke service die zijn opgehaald en worden gegevens opgeslagen in een externe store (zoals een Azure blob of tabel store vandaag) verwerkt is statuscontrole. Het zojuist zorgt ervoor dat de status in de winkel externe staat.

De meeste services opslaan vandaag hun status extern, omdat het externe archief wat betrouwbaarheid, beschikbaarheid, schaalbaarheid en consistentie voor die staat biedt. In de Service stof, niet zijn statuscontrole services vereist voor de opslag van hun status extern; Service stof zorgt voor deze vereisten voor zowel de servicecode en status van de service.

Stel dat we wilt schrijven van een service waarmee verzoeken om een reeks conversies die moeten worden uitgevoerd op een afbeelding en de afbeelding die moet worden geconverteerd.  Voor deze service, levert deze een communicatie luisteraar ervan af (Stel Web API) dat wordt geopend een communicatiepoort en kan bijdragen via een API zoals `ConvertImage(Image i, IList<Conversion> conversions)`. In deze API, kan de service de gegevens en de aanvraag opslaat in een betrouwbare wachtrij en sommige token Ga terug naar de client zodat deze kan bijhouden de aanvraag (Aangezien de aanvragen enige tijd duren kunnen).

In deze service mogelijk RunAsync complexere. De service kan een lus binnen de RunAsync die aanvragen afmelden bij IReliableQueue ophaalt, voert de conversies vermeld en slaat de resultaten in IReliableDictionary, hebben, zodat wanneer de client terugkomt, verkregen hun geconverteerde afbeeldingen worden kan. Om ervoor te zorgen dat zelfs als iets niet aan de afbeelding niet verloren, deze betrouwbare Service wilt afmelden bij de wachtrij halen, de conversies uitvoeren en het resultaat opslaan in een transactie. In dit geval het bericht dat is wel alleen uit de wachtrij wordt verwijderd en de resultaten worden opgeslagen in de woordenlijst resultaat wanneer de conversies voltooid zijn. Als iets mislukt in het midden (zoals de computer die dit exemplaar van de code wordt uitgevoerd op), wordt het verzoek blijft in de wachtrij moeten opnieuw worden verwerkt.

Wat te weten over deze service is dat deze als een normale .NET-service klinkt. De enige verschil is dat de gegevensstructuren die wordt gebruikt (IReliableQueue en IReliableDictionary) worden verstrekt door de Service stof en daarom worden aangebracht ten zeerste betrouwbare, beschikbare en consistente.

## <a name="when-to-use-reliable-services-apis"></a>Wanneer gebruikt u betrouwbare Services-API 's
Als een van de volgende uw service applicaties beschrijving, moet u betrouwbare Services-API's overwegen:

- U moet toepassing gedrag over meerdere van status (bijvoorbeeld orders en lijnartikelen volgorde).

- De status van uw toepassing kunt natuurlijk worden gezien als betrouwbare woordenlijsten en wachtrijen.

- Uw status moet ten zeerste beschikbaar met lage latentie toegang.

- Uw toepassing moet om te bepalen de gelijktijdigheid of granulatie van uitgevoerde bewerkingen in een of meer betrouwbare siteverzamelingen.

- U wilt bepalen van de partities kleurenschema voor uw service of via de communicatie beheren.

- Uw code moet een vrij te geven thread-runtimeomgeving.

- Uw toepassing moet dynamisch maken of betrouwbare woordenlijsten of wachtrijen destroy gedurende runtime.

- U moet via programmacode bepalen geleverde Service stof back-up en herstellen van functies voor van uw service staat *.

- Uw toepassing moet voor het behoud van het wijzigingsoverzicht voor de eenheden van staat *.

- U wilt ontwikkelen of gebruiken derde partij ontwikkeld, aangepaste staat providers *.

> [AZURE.NOTE] * Functies die beschikbaar zijn op de algemene beschikbaarheid SDK.


## <a name="next-steps"></a>Volgende stappen
+ [Betrouwbare Services snel aan de slag](service-fabric-reliable-services-quick-start.md)
+ [Betrouwbare Services Geavanceerd gebruik](service-fabric-reliable-services-advanced-usage.md)
+ [Het programmeren betrouwbare betrokkenen-model](service-fabric-reliable-actors-introduction.md)
