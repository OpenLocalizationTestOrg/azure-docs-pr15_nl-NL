<properties 
    pageTitle="Inleiding tot DocumentDB, een database JSON | Microsoft Azure" 
    description="Meer informatie over Azure DocumentDB, een NoSQL JSON-database. In dit document-database is geoptimaliseerd voor groot gegevens, elastische schaalbaarheid en beschikbaarheid." 
    keywords="JSON-database, document-database"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="09/13/2016" 
    ms.author="mimig"/>

# <a name="introduction-to-documentdb-a-nosql-json-database"></a>Inleiding tot DocumentDB: een NoSQL JSON-Database

##<a name="what-is-documentdb"></a>Wat is DocumentDB?

DocumentDB is een volledig beheerde database-service voor NoSQL is geoptimaliseerd voor snelle en overzichtelijk prestaties, beschikbaarheid elastische schaalbaarheid, distributielijsten en gebruiksgemak ontwikkeling. Een database NoSQL schema-vrije biedt DocumentDB uitgebreide en vertrouwde SQL querymogelijkheden met consistente lage vertragingstijden op gegevens van de JSON - ervoor te zorgen dat 99% van uw leest bereikbaar zijn minder dan 10 milliseconden en 99% van uw schrijft onder 15 milliseconden worden aangeboden. Deze unieke voordelen tabelmaakquery DocumentDB een geweldige passend maken voor het web, mobiele, spellen, en IoT en vele andere toepassingen waarmee naadloze schaal en globale herhaling nodig.

## <a name="how-can-i-learn-about-documentdb"></a>Hoe vind ik meer informatie over DocumentDB? 

Er is een snelle manier om meer informatie over DocumentDB en in actie zien en deze drie stappen: 

1. Bekijk de twee minuut [Wat is DocumentDB?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/) video, waarin maakt u kennis met de voordelen van het gebruik van DocumentDB.
2. Bekijk de drie minuut [DocumentDB maken op Azure](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) video, wat op hoe u aan de slag met DocumentDB met behulp van de Azure-Portal.
3. Ga naar de [Query Speelplaats](http://www.documentdb.com/sql/demo), waar u verschillende activiteiten voor meer informatie over de uitgebreide functionaliteit voor het opvragen beschikbaar in DocumentDB kunt doorlopen. Vervolgens onderweg via naar het tabblad Sandbox en uw eigen aangepaste SQL-query's uitvoeren en experimenteren met DocumentDB.

Vervolgens terug naar dit artikel, waarbij we wordt de afdruk.  

## <a name="what-capabilities-and-key-features-does-documentdb-offer"></a>Welke functies en belangrijke functies DocumentDB biedt?  

Azure DocumentDB biedt de volgende belangrijke mogelijkheden en voordelen:

-   **Elastically scalable doorvoer en opslag:** Eenvoudig vergroten of verkleinen van de database DocumentDB JSON aan uw behoeften van toepassing. Uw gegevens worden opgeslagen op effen staat schijven (SSD) voor de laag overzichtelijk vertragingstijden. DocumentDB ondersteunt containers voor het opslaan van JSON gegevens collecties die kunnen worden aangepast aan vrijwel onbeperkte opslaggrootte en de ingerichte doorvoer genoemd. U kunt elastically schalen DocumentDB met overzichtelijk prestaties naadloos als uw toepassing in omvang groeit. 

-   **Herhaling voor meerdere landen/regio:** DocumentDB transparant wordt overgenomen door uw gegevens alle regio's die u hebt gekoppeld aan uw account DocumentDB, zodat u kunt het ontwikkelen van toepassingen waarvoor globale toegang tot gegevens terwijl compromissen nodig zijn tussen consistentie, beschikbaarheid en prestaties, alle bijbehorende garanties. DocumentDB biedt regionale overname met meerdere homing API's en de mogelijkheid om te elastically schaal doorvoer en opslag overal ter wereld. Lees meer in [gegevens met DocumentDB globaal distribueren](documentdb-distribute-data-globally.md).

-   **Ad hoc-query's met vertrouwde SQL-syntaxis:** Heterogene JSON documenten binnen DocumentDB opslaan en deze documenten door een vertrouwde SQL-syntaxis query. DocumentDB maakt gebruik van een zeer gelijktijdige, slot gratis, log gestructureerde indexing technologie automatisch alle documentinhoud indexeren. Hiermee worden uitgebreide realtime-query's zonder dat u een schema hints, secundaire of weergaven wilt opgeven. Meer informatie in de [Query DocumentDB](documentdb-sql-query.md). 

-   **JavaScript worden uitgevoerd in de database:** Druk toepassingslogica opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies (UDF) JavaScript-standaard gebruiken. Hiermee kunt uw toepassingslogica plaatsvinden via gegevens zonder dat de verkeerde combinaties tussen de toepassing en het databaseschema. DocumentDB biedt volledige transacties uitvoering van JavaScript-toepassingslogica direct in de database-engine. De naadloze integratie van JavaScript kunt de uitvoering van invoegen, vervangen, verwijderen en selecteer bewerkingen uit vanuit een JavaScript-programma als een geïsoleerd transactie. Meer informatie in [DocumentDB serverzijde hoeft te programmeren](documentdb-programming.md).

-   **Instelbare consistentie niveaus:** Selecteer uit vier goed gedefinieerd consistentie niveaus tot optimale verhouding tussen de consistentie en prestaties bereiken. Voor query's en meer bewerkingen, biedt DocumentDB vier distinct consistentie niveaus: sterke gebonden-staleness, sessie, en eventuele. Deze niveaus gedetailleerde, duidelijke consistentie kunnen u geluid voor-en nadelen tussen consistentie, beschikbaarheid en latentie. Meer informatie bij het [gebruik van de consistentie niveaus als u wilt maximaliseren beschikbaarheid en prestaties in DocumentDB](documentdb-consistency-levels.md).

-   **Volledig beheerd:** Hoeft database en machine bronnen beheren. Als een Microsoft Azure volledig beheerde service, u niet hoeft te beheren virtuele machines, implementeren en configureren van de software, beheren schaalbaarheid, of complexe gegevens laag upgrades handelen. Elke database is automatisch back-up gemaakt en beveiligd tegen regionale fouten. U kunt eenvoudig een DocumentDB-account toevoegen en inrichten van capaciteit, zoals u deze nodig hebt, zodat u kunt richten op uw toepassing in plaats van besturingssysteem en beheren van uw database. 

-   **Openen door te ontwerpen:** Snel aan de slag met behulp van bestaande vaardigheden en hulpprogramma's. Hoeft te programmeren tegen DocumentDB eenvoudige, toegankelijk is en hoeft u vast te stellen van nieuwe hulpmiddelen of voldoen aan de aangepaste uitbreidingen van JSON of JavaScript niet. U kunt toegang tot de database-functionaliteit inclusief CRUD, query en JavaScript processing via een eenvoudige RESTful HTTP-interface. DocumentDB bestrijkt bestaande opmaak, talen en standaarden terwijl aanbod hoge waarde mogelijkheden boven aan deze database.

-   **Automatische indexering:** Standaard DocumentDB [indexeert automatisch](documentdb-indexing.md) alle documenten in de database en niet verwacht of een schema of de aanmaakdatum van secundaire indexen vereisen. Wilt u zich niet alles indexeren? Geen probleem, kunt u [Afmelden bij paden in uw JSON-bestanden](documentdb-indexing-policies.md) te.

##<a name="data-management"></a>Hoe DocumentDB gegevens beheren?

Azure DocumentDB JSON-gegevens door een duidelijke database resources worden beheerd. Deze resources worden gerepliceerd voor maximale beschikbaarheid en uniek toegankelijk zijn voor hun logische URI zijn. DocumentDB biedt dat een eenvoudige HTTP op basis van RESTful programming model voor alle resources. 

Het account van de database DocumentDB is een unieke naamruimte waarmee u toegang hebt tot Azure DocumentDB. Voordat u een databaseaccount maken kunt, moet u een Azure-abonnement, waarmee u toegang krijgt tot tal van Azure services hebben. 

Alle bronnen binnen DocumentDB zijn gebaseerd en opgeslagen als JSON-documenten. Resources worden beheerd als items, dat de JSON worden documenten met metagegevens en als-feeds die verzamelingen items zijn. Sets met items die zijn opgenomen in de desbetreffende feeds.

De onderstaande afbeelding ziet u de relaties tussen de DocumentDB bronnen:

![De hiërarchische relatie tussen bronnen in DocumentDB, een NoSQL JSON-database][1] 

Een databaseaccount bestaat uit een reeks-databases, elk met meerdere siteverzamelingen, die elk opgeslagen procedures, triggers UDF's, documenten en gerelateerde bijlagen kan bevatten. Een database is ook gebruikers, elk voorzien van een set machtigingen voor toegang tot verschillende andere siteverzamelingen, opgeslagen procedures, triggers, UDF's, documenten of bijlagen gekoppeld. Terwijl databases, gebruikers, machtigingen en verzamelingen systeem gedefinieerde resources met bekende schema's zijn-documenten, opgeslagen procedures, triggers, UDF's en bijlagen willekeurige bevatten, door gebruiker gedefinieerde JSON-inhoud.  

##<a name="develop"></a>Hoe kan ik apps gebruiken met DocumentDB ontwikkelen?

Azure DocumentDB beschrijft bronnen via een REST API die kan worden aangeroepen door een staat HTTP/HTTPS-verzoeken taal. Bovendien biedt DocumentDB programming bibliotheken voor verschillende populaire talen. Deze bibliotheken vereenvoudigen veel aspecten van het werken met Azure DocumentDB door het afhandelen van details zoals adres caching, uitzondering management, automatische nieuwe pogingen, enzovoort. Bibliotheken zijn momenteel beschikbaar zijn voor de volgende talen en platforms:  

Downloaden | Documentatie
--- | ---
[.NET SDK](http://go.microsoft.com/fwlink/?LinkID=402989) | [.NET-bibliotheek](https://msdn.microsoft.com/library/azure/dn948556.aspx)
[Node.js SDK](http://go.microsoft.com/fwlink/?LinkID=402990) | [Node.js-bibliotheek](http://azure.github.io/azure-documentdb-node/)
[Java SDK](http://go.microsoft.com/fwlink/?LinkID=402380) | [Java-bibliotheek](http://azure.github.io/azure-documentdb-java/)
[JavaScript-SDK](http://go.microsoft.com/fwlink/?LinkID=402991) | [JavaScript-bibliotheek](http://azure.github.io/azure-documentdb-js/)
n/b | [Aan de clientzijde JavaScript SDK](http://azure.github.io/azure-documentdb-js-server/)
[Python SDK](https://pypi.python.org/pypi/pydocumentdb) | [Python bibliotheek](http://azure.github.io/azure-documentdb-python/)

Meer geavanceerde mogelijkheden eenvoudige maken, lezen, bijwerken en verwijderen van de bewerkingen, DocumentDB biedt een uitgebreide SQL-query-interface voor het ophalen van JSON-documenten en serverzijde ondersteuning voor transacties uitvoering van JavaScript-toepassingslogica. De query en script execution-interfaces zijn beschikbaar via alle bibliotheken die platform, evenals de REST API's. 

### <a name="sql-query"></a>SQL-query
Azure DocumentDB ondersteunt bij het controleren van documenten met een SQL-taal, die basis in het systeem JavaScript en expressies met ondersteuning voor relationele, hiërarchische en ruimte query's heeft zijn. De taal van de query DocumentDB is een eenvoudige, maar krachtige interface voor query JSON documenten. De taal ondersteunt een subset van ANSI SQL-grammatica en naadloze integratie van JavaScript-object, matrices, objectconstructie en functie aanroep toegevoegd. De query-model zonder een expliciete schema of hints voor de indexing verschaft DocumentDB van de ontwikkelaar.

Door de gebruiker gedefinieerde functies (UDF) kan worden geregistreerd bij DocumentDB en waarnaar wordt verwezen als onderdeel van een SQL-query, waardoor de grammatica ter ondersteuning van maatwerktoepassing logica uitbreiden. Deze UDF's zijn geschreven als JavaScript-programma's en worden uitgevoerd in de database. 

Voor ontwikkelaars van .NET biedt DocumentDB ook een provider van de query LINQ als onderdeel van de [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx). 

### <a name="transactions-and-javascript-execution"></a>Transacties en JavaScript uitvoeren
DocumentDB kunt u het schrijven van toepassing als benoemde programma's die zijn geschreven volledig in JavaScript. Deze programma's voor een siteverzameling zijn geregistreerd en databasebewerkingen op de documenten in een bepaalde siteverzameling kunnen worden verleend. JavaScript kan worden geregistreerd voor uitvoering als een trigger, de opgeslagen procedure of de gebruiker gedefinieerde functie. Triggers en opgeslagen procedures kunnen maken, lezen, bijwerken en verwijderen van documenten, terwijl de gebruiker gedefinieerde functies als onderdeel van de logica van de query worden uitgevoerd zonder schrijftoegang tot de verzameling uitvoeren.

JavaScript kan worden uitgevoerd binnen DocumentDB lijkt sterk op de concepten die worden ondersteund door relationele database-systemen, met JavaScript als een modern vervanging voor Transact-SQL. Alle JavaScript-logica wordt uitgevoerd in een omgeving ZURE transactie met overgeslagen. In de loop van de uitvoering, als de JavaScript een uitzondering genereert, klikt u vervolgens de gehele transactie wordt afgebroken.

## <a name="next-steps"></a>Volgende stappen
Hebt u al een Azure-account? Vervolgens u kunt aan de slag met DocumentDB in de [Portal van Azure](https://portal.azure.com/#gallery/Microsoft.DocumentDB) doordat [een account van de database DocumentDB](documentdb-create-account.md).

Geen Azure-account? U kunt:

- Registreren voor een [gratis proefversie Azure](https://azure.microsoft.com/free/), waarmee u 30 dagen en $200 om te proberen de Azure services. 
- Als u een MSDN-abonnement hebt, bent u in aanmerking komen voor [$150 in gratis Azure tegoeden per maand](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) gebruiken op een Azure-service. 

Klik wanneer u klaar voor meer informatie bent, gaat u naar onze [leerpad](https://azure.microsoft.com/documentation/learning-paths/documentdb/) als u wilt gaan alle trainingsmaterialen die beschikbaar zijn voor u. 


[1]: ./media/documentdb-introduction/json-database-resources1.png
 
