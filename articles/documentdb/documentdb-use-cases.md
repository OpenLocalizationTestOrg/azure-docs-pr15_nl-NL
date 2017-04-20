<properties 
    pageTitle="Algemene DocumentDB gebruiken gevallen | Microsoft Azure" 
    description="Meer informatie over de top vijf gevallen gebruiken voor DocumentDB: de gebruiker worden gegenereerd inhoud, -logboekregistratie, catalogusgegevens, voorkeuren gebruikersgegevens en Internet van dingen (IoT)." 
    services="documentdb" 
    authors="h0n" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="hawong"/>

# <a name="common-documentdb-use-cases"></a>Veelvoorkomende DocumentDB gebruik gevallen
Dit artikel bevat een overzicht van verschillende veelvoorkomende gebruik gevallen voor DocumentDB.  De aanbevelingen in dit artikel fungeren als uitgangspunt tijdens het ontwikkelen van uw toepassing met DocumentDB.   

Lees dit artikel en kunt u wel de volgende vragen beantwoorden: 
 
- Wat zijn de algemene gebruik dozen voor DocumentDB?
- Wat zijn de voordelen van het gebruik van DocumentDB als een gebruiker gegenereerd inhoud store?
- Wat zijn de voordelen van het gebruik van DocumentDB als een catalogusgegevens opslaan?
- Wat zijn de voordelen van het gebruik van DocumentDB als een gebeurtenislogboek opslaan?
- Wat zijn de voordelen van het gebruik van DocumentDB als een gebruiker voorkeuren-gegevens opslaan?
- Wat zijn de voordelen van het gebruik van DocumentDB als een gegevensopslag voor Internet van dingen (IoT) systems?

## <a name="common-use-cases-for-documentdb"></a>Algemene gebruik dozen voor DocumentDB
Azure DocumentDB is een algemene NoSQL-database die wordt gebruikt in een breed scala van toepassingen en gebruik dozen. Dit is een goede keuze voor alle toepassingen die lage volgorde van milliseconden antwoord tijden nodig en moet snel schaal. Hier volgen enkele kenmerken van DocumentDB waardoor deze zeer geschikt voor krachtige toepassingen.

- DocumentDB partities native van uw gegevens voor beschikbaarheid en schaalbaarheid.
- De DocumentDB heeft SSD back-opslag met lage latentie volgorde van milliseconden antwoord tijden.
- De DocumentDB ondersteunt consistentie niveaus zoals eventuele, sessie en gebonden staleness voor lage kosten naar prestaties-breedteverhouding. 
- DocumentDB heeft een flexibele prijzen model voor gegevens-vriendelijke, dat meter opslagruimte en doorvoer onafhankelijk.
- DocumentDB van gereserveerde doorvoer model, kunt u structuren op basis van aantal leest/schrijft in plaats van CPU/geheugen/IO's / s van de onderliggende hardware.
- DocumentDB van ontwerpen kunt die u de schaal van enorme verzoek volume in de volgorde van miljard van aanvragen per dag.

Deze kenmerken zijn met name handig wanneer u naar web, mobiele, gaat spellen en IoT toepassingen die nodig lage antwoord tijden en moeten worden afgehandeld enorme hoeveelheid lezen en schrijven. 

## <a name="user-generated-content"></a>Gebruikersinhoud die zijn gegenereerd
Een algemene use-case voor DocumentDB is om op te slaan en query gebruiker gegenereerd inhoud (UGC) voor het web en mobiele toepassingen, met name sociale media-toepassingen.  Enkele voorbeelden van UGC zijn chatsessies, tweets blogberichten, classificaties en opmerkingen.  De UGC in sociale media-toepassingen is vaak een combinatie van vrije vorm tekst, eigenschappen, labels en relaties die niet beperkt harde structuur tot zijn.   

Inhoud zoals chats, opmerkingen en berichten kunnen worden opgeslagen in DocumentDB zonder transformaties of complexe object aan relationele gegevens lagen.  Eigenschappen van gegevens kunnen worden toegevoegd of gewijzigd eenvoudig om te voldoen aan vereisten zoals ontwikkelaars de toepassingscode doorlopen, dus bevorderen van snelle ontwikkeling.  

Toepassingen die zijn geïntegreerd met verschillende sociale netwerken moeten reageren op schema's wijzigen van deze netwerken.  Als gegevens wordt automatisch geïndexeerd al dan niet standaard in DocumentDB, kan gegevens moeten worden opgevraagd op elk gewenst moment.  Daarom hebt deze toepassingen de flexibiliteit om op te halen prognoses aan de hand van de desbetreffende behoeften.       

Veel van de sociale toepassingen uitvoeren op wereldwijde schaal en onverwachte gebruikspatronen kunnen vertonen.  Flexibiliteit bij schaal van de gegevensopslag is essentiële tijdens de toepassingslaag schalen zodat deze overeenkomt met de aanvraag gebruik.  U kunt schalen af door het toevoegen van extra gegevenspartities onder een DocumentDB-account.  Bovendien kunt u ook extra DocumentDB-accounts maken tussen meerdere regio's. Zie [Azure regio's](https://azure.microsoft.com/regions/#services)voor de beschikbaarheid van een DocumentDB service regio.   

## <a name="catalog-data"></a>Gegevens in de catalogus
Scenario's voor het gebruik van gegevenscatalogus betrekking hebben op Opslaan en query's uitvoeren een verzameling kenmerken voor entiteiten zoals personen, plaatsen en producten.  Enkele voorbeelden van gegevens in de catalogus zijn gebruikersaccounts, catalogi, apparaat registervermeldingen voor IoT en bol. van materialen systemen.  Kenmerken voor deze gegevens kunnen variëren en na verloop van tijd om te voldoen aan de toepassingsvereisten kunnen wijzigen.  

Bekijk een voorbeeld van een productcatalogus voor een auto-onderdelen leverancier. Elk onderdeel mogelijk een eigen kenmerken naast de algemene kenmerken die alle onderdelen delen.  Kenmerken voor een bepaald deel kunnen bovendien het volgende jaar wanneer een nieuw model is uitgebracht wijzigen.  Als een document opslaan van JSON, DocumentDB ondersteunt flexibele schema's en u kunt gegevens met geneste eigenschappen weergeven en het is dus geschikt voor het opslaan van gegevens in de productcatalogus.       

## <a name="logging-and-time-series-data"></a>Logboekregistratie en tijdreeks gegevens
Logboekregistratie van toepassing is vaak weergegeven in grote hoeveelheden en mogelijk hebben verschillende kenmerken op basis van de gedistribueerde toepassing-versie of de gebeurtenissen die onderdeel logboekregistratie.  Logboekgegevens is niet beperkt tot complexe relaties aanbrengt of harde structuren. Logboekgegevens worden steeds, behouden in de indeling van JSON omdat JSON licht en eenvoudig voor mensen te lezen.
   
Er zijn meestal twee belangrijkste gebruik gevallen betrekking hebben op gebeurtenislogboekgegevens.  De eerste use-case is ad-hoc-query's uitvoeren op een subset van gegevens voor probleemoplossing.  Tijdens het oplossen van problemen is een subset van gegevens eerst opgehaald uit de logboeken, meestal door tijdreeks.  Vervolgens wordt een Inzoomoptie uitgevoerd door het filteren van de gegevensset met foutniveaus of foutberichten. Dit is hier gebeurtenislogboeken in DocumentDB opslaan en is een voordeel. Logboekgegevens die zijn opgeslagen in DocumentDB wordt automatisch geïndexeerd al dan niet standaard en kan dus de werkmap is gereed moeten worden opgevraagd op elk gewenst moment. Bovendien kunt logboekgegevens meerdere gegevenspartities worden vastgelegd als een tijdreeks. Oudere logboeken kunnen worden geïmplementeerd op verkoudheid opslagruimte per uw bewaarbeleid.          

De tweede use-case heeft betrekking op langdurige gegevens analytics taken offline via een grote hoeveelheid logboekgegevens worden uitgevoerd.  Voorbeelden van deze use-case zijn server beschikbaarheid analyse, toepassing fout analyse en clickstream gegevensanalyse.  Hadoop wordt meestal gebruikt deze typen analyses uitvoeren.  Met de Hadoop-Connector voor DocumentDB werken DocumentDB databases als gegevensbronnen en sinks voor varken, component en kaart/verkleinen taken. Zie [een taak Hadoop met DocumentDB en HDInsight uitvoeren](documentdb-run-hadoop-with-hdinsight.md)voor meer informatie over de Hadoop-Connector voor DocumentDB.      

## <a name="gaming"></a>Spellen
De databaselaag is een belangrijk onderdeel van spellen-toepassingen. Moderne spellen grafische bewerkingen uitvoeren op mobile/console-clients, maar zijn afhankelijk van de cloud voor het leveren van aangepaste en gepersonaliseerde inhoud, zoals in het spel Stat, integratie van sociale media en high-score scoreborden. Spellen erg laag vertragingstijden vereisen voor lezen en schrijven op te geven met een aantrekkelijke spel ervaring en de databaselaag moet de beste en slechtste in verzoek tarieven tijdens nieuwe spel gestart en functie-updates afhandelen.

DocumentDB wordt gebruikt door enorme schaal spellen zoals [het lopen dode: geen Man van Land](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/) door [Volgende spellen](http://www.nextgames.com/), en [Halo 5: voogden](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Gebruik in beide gevallen, de belangrijkste voordelen van DocumentDB zijn de volgende:

- DocumentDB kunt prestaties om te worden aangepast omhoog of omlaag elastically. Hierdoor spellen worden afgehandeld bijwerken profiel en stat uit tientallen miljoenen tegelijk gamers door één API bellen.
- DocumentDB ondersteunt milliseconden gelezen en geschreven om u te helpen vermijden eventuele vertragingen bij het berekenen tijdens het spel.
- Automatische indexering van DocumentDB kan voor filteren op meerdere verschillende eigenschappen in realtime, bijvoorbeeld Zoek spelers door hun interne speler-id's of hun GameCenter, Facebook, Google-id's of query die zijn gebaseerd op speler lidmaatschap van een vereniging. Dit is mogelijk zonder het samenstellen van complexe indexeren of sharding infrastructuur.
- Sociale functies zoals chatberichten spel, speler vereniging lidmaatschappen, uitdagingen voltooid, hoog-score scoreborden en sociale grafieken zijn gemakkelijker te implementeren met een flexibele schema.
- DocumentDB als een beheerde platform-als-een-service (PaaS) vereist minimale instelling management werken om toe te staan voor snelle iteratie en market verminderen.


## <a name="user-preferences-data"></a>Voorkeuren gebruikersgegevens
Tegenwoordig meeste moderne web en mobiele toepassingen worden geleverd met complexe weergaven en ervaringen. Deze weergaven en ervaringen zijn meestal dynamisch, catering gebruikersvoorkeuren of stemming en huisstijl behoeften.  Daarom moeten toepassingen kunnen om op te halen persoonlijke instellingen effectief om gebruikersinterface-elementen en ervaringen snel weergegeven. 

JSON is een effectieve indeling om UI indeling gegevens te stellen, zoals het is niet alleen licht, maar ook kan eenvoudig door worden geïnterpreteerd JavaScript.  DocumentDB biedt instelbare consistentie niveaus die snel gelezen met lage latentie schrijft toestaan. UI-indeling gegevens persoonlijke instellingen als JSON-documenten op te nemen in DocumentDB opslaat is daarom een effectieve methode voor het ophalen van deze gegevens via de verbinding.

## <a name="iot-and-device-sensor-data"></a>IoT en apparaat sensorgegevens
IoT gebruik gevallen delen meest sommige patronen in hoe ze nemen, verwerken en opslag van gegevens.  Deze systemen Sta eerst gegevens opname die gegevens van apparaat sensoren van verschillende landinstellingen lichtflitsen kunt nemen.  Vervolgens deze systemen verwerken en streaming gegevens als u wilt afleiden realtime inzichten analyseren. En laatste maar niet minste meest als dat niet alle gegevens wordt uiteindelijk terechtkomt in een gegevensopslag voor ad hoc-query- en offline analytics.    

Microsoft Azure biedt uitgebreide services die kunnen worden gebruikt voor IoT gevallen gebruiken.  Azure IoT services zijn een reeks services met inbegrip van Azure gebeurtenis Hubs, Azure DocumentDB Azure Stream analyses, Azure melding Hub, Azure Machine Learning, Azure HDInsight en PowerBI. 

Lichtflitsen gegevens kunnen worden binnenkrijgen Azure gebeurtenis Hubs als deze optie veel doorvoer gegevens opname met lage latentie biedt. Gegevens ingenomen die moet worden verwerkt voor realtime inzicht kan worden softwareproducten naar Azure Stream analyses van realtime analysegegevens. Gegevens kan in DocumentDB voor ad hoc query's worden geladen. Wanneer de gegevens in DocumentDB is geladen, kan de gegevens moeten worden opgevraagd.  De gegevens in DocumentDB kan worden gebruikt als de verwijzingsgegevens als onderdeel van realtime analytics. Bovendien kunnen gegevens verder worden verfijnd en verwerkt door DocumentDB gegevens verbinden met HDInsight voor taken varken, component of kaart/verkleinen.  Verfijnd gegevens wordt voor rapportage vervolgens terug naar DocumentDB geladen.   

Zie voor een steekproef IoT oplossing met DocumentDB, EventHubs en Storm, de [hdinsight-storm-voorbeelden bibliotheek op GitHub](https://github.com/hdinsight/hdinsight-storm-examples/).

Zie voor meer informatie over Azure aanbiedingen voor IoT, [maken de Internet van uw zaken aan bod](http://www.microsoft.com/en-us/server-cloud/internet-of-things.aspx).

## <a name="next-steps"></a>Volgende stappen
 
Als u wilt beginnen met DocumentDB, kunt u een [account](https://azure.microsoft.com/pricing/free-trial/) maken en volg vervolgens onze [leerpad](https://azure.microsoft.com/documentation/learning-paths/documentdb/) om meer informatie over DocumentDB en de informatie die u nodig hebt. 

Of, als u wilt meer informatie over klanten die DocumentDB, de volgende artikelen voor de klant zijn beschikbaar:

- [Volgende spellen](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/). De dode wandel: Man van Land spel soars # 1 wordt ondersteund door Azure DocumentDB.
- [Halo](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/). Hoe Halo 5 sociale spelen met Azure DocumentDB geïmplementeerd.
- [Cortana Analytics-galerie](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/). Cortana Analytics galerie - een scalable communitysite is gebouwd op Azure DocumentDB.
- [Gemakkelijk](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602). Integrator voorloop krijgt meertalige ondernemingen globale inzicht in minuten met flexibele Cloud-technologieën.
- [Nieuws Republiek](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639). Bedrijfsinformatie toevoegen aan het nieuws informatie te verstrekken met doel voor ingeschakeld burgers. 
- [SGS internationale](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653). Voor consistente kleur overal ter wereld, zijn de belangrijkste merken Ga naar SGS. En SGS verandert in Azure.
- [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608). Globale opvulteken Telenor wordt de cloud gebruikt om te gaan met de snelheid van een opstarten. 
- [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667). Het archief van de toekomst wordt uitgevoerd op de snelle zoeken en de eenvoudige stroom van gegevens.
