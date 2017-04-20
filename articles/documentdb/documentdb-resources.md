<properties 
    pageTitle="DocumentDB hiërarchische resource model en concepten | Microsoft Azure" 
    description="Meer informatie over de DocumentDB hiërarchische model van databases, verzamelingen, door de gebruiker gedefinieerde functie (UDF), documenten, machtigingen voor het beheren van bronnen, en meer."
    keywords="Hiërarchische model, documentdb, azure, Microsoft azure"   
    services="documentdb" 
    documentationCenter="" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="anhoh"/>

# <a name="documentdb-hierarchical-resource-model-and-concepts"></a>Model met hiërarchische DocumentDB en concepten

De database-entiteiten die worden beheerd in DocumentDB zijn **resources**genoemd. Elke resource wordt uniek aangeduid met een logische URI. U kunt werken met de resources met standaard HTTP-woorden, de aanvraag en respons kop en de statuscodes. 

In dit artikel leest, kunt u wel de volgende vragen beantwoorden:

- Wat is de DocumentDB resource model?
- Wat zijn gedefinieerd resources in plaats van de gebruiker gedefinieerde resources?
- Hoe adresseer ik een resource?
- Hoe werk ik met verzamelingen?
- Hoe werk ik met opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies (UDF)?

## <a name="hierarchical-resource-model"></a>Hiërarchische resource model
Zoals u in het volgende diagram ziet, wordt het DocumentDB hiërarchische **resource model** bestaat uit sets met bronnen onder een databaseaccount, elk gebruikt via een logische en stabiele-URI. Een reeks resources zal worden verwezen als een **feed** in dit artikel. 

>[AZURE.NOTE] DocumentDB biedt een zeer efficiënte TCP-protocol dat ook RESTful in de communicatiemodel, beschikbaar via de [.NET-client SDK is](https://msdn.microsoft.com/library/azure/dn781482.aspx).

![DocumentDB hiërarchische resource model][1]  
**Hiërarchische resource model**   

Als u wilt werken met resources, moet u [een account van de database DocumentDB maken](documentdb-create-account.md) met uw Azure-abonnement. Een databaseaccount kan bestaan uit een set- **databases**, elk met meerdere **siteverzamelingen**, die elk weer bevatten **opgeslagen procedures, veroorzaakt, UDF's, documenten** en gerelateerde **bijlagen** (voorbeeldfunctie). Een database is ook **gebruikers**, elk voorzien van een set **machtigingen** voor toegang tot siteverzamelingen, opgeslagen procedures, triggers, UDF's, documenten of bijlagen gekoppeld. Hoewel databases, gebruikers, machtigingen en verzamelingen zijn systeem gedefinieerde resources met bekende schema's, documenten en bijlagen willekeurige bevatten, door de gebruiker gedefinieerde JSON-inhoud.  

|Resource   |Beschrijving
|-----------|-----------
|Databaseaccount   |Een databaseaccount is gekoppeld aan een set databases en een vaste hoeveelheid blobopslag voor bijlagen (voorbeeldfunctie). U kunt een of meer database accounts met uw Azure-abonnement kunt maken. Ga naar onze [pagina prijzen](https://azure.microsoft.com/pricing/details/documentdb/)voor meer informatie.
|Database   |Een database is een logische container document opslag partities over siteverzamelingen. Het is ook een gebruikerscontainer.
|Gebruiker   |De logische naamruimte voor een bereik van machtigingen instellen. 
|Machtiging |Een Autorisatietoken dat is gekoppeld aan een gebruiker voor toegang tot een specifieke bron.
|Siteverzameling |Een verzameling is een container van JSON-documenten en de bijbehorende logica voor JavaScript-toepassing. Een verzameling is een factureerbare entiteit, waarbij de [kosten](documentdb-performance-levels.md) worden bepaald door het prestatieniveau dat is gekoppeld aan de siteverzameling. Verzamelingen kunnen een of meer partities/servers omvatten en kunnen schalen om af te handelen vrijwel onbeperkte hoeveelheden opslag of doorvoer.
|Opgeslagen Procedure   |Toepassingslogica geschreven in JavaScript dat is geregistreerd bij een siteverzameling en transactioneel uitgevoerd in de database-engine.
|Trigger    |Toepassingslogica geschreven in JavaScript uitgevoerd voor of na een van beide invoegen, vervangen of verwijderen.
|UDF    |Toepassingslogica geschreven in JavaScript. UDF's kunnen u een aangepaste query-operator model en daarmee ook uitbreiden de core DocumentDB querytaal.
|Document   |Door gebruiker gedefinieerde (willekeurige) JSON-inhoud. Standaard geen schema, moet deze velden worden gedefinieerd en ook niet secundaire indexen hoeft te worden opgegeven voor alle documenten die zijn toegevoegd aan een siteverzameling.
|(Preview) Bijlage   |Een bijlage is een speciale-document met verwijzingen en de bijbehorende metagegevens voor externe blob/media. De ontwikkelaar kunt kiezen om de blob beheerd door DocumentDB hebt of opslaan met een externe blob-provider zoals OneDrive, Dropbox, enzovoort. 


## <a name="system-vs-user-defined-resources"></a>Systeem versus door de gebruiker gedefinieerde resources
Bronnen zoals database-accounts, databases, verzamelingen, gebruikers, machtigingen, opgeslagen procedures, triggers en UDF's - hebben allemaal een vaste schema en systeembronnen worden genoemd. In tegenstelling bronnen zoals documenten en bijlagen hebben geen beperkingen voor het schema en ziet u voorbeelden van de gebruiker gedefinieerde resources. In DocumentDB, zowel systeem en de gebruiker gedefinieerde resources worden weergegeven en beheerd als standaard-compatibele JSON. Alle resources, systeem of door de gebruiker gedefinieerde, hebben de volgende algemene eigenschappen.

> [AZURE.NOTE] Opmerking dat alle systeem eigenschappen gegenereerd in een resource worden voorafgegaan door een onderstrepingsteken (_) in de weergave van hun JSON.

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>Eigenschap</strong></p></td>
            <td valign="top"><p><strong>Gebruiker worden ingesteld of systeem gegenereerd?</strong></p></td>
            <td valign="top"><p><strong>Doel</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>Systeem gegenereerd</p></td>
            <td valign="top"><p>Systeem gegenereerd, unieke en hiërarchische id van de resource</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Systeem gegenereerd</p></td>
            <td valign="top"><p>ETag van de resource die is vereist voor optimistische gelijktijdigheid besturingselement</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Systeem gegenereerd</p></td>
            <td valign="top"><p>Laatste bijgewerkte tijdstempel van de resource</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>Systeem gegenereerd</p></td>
            <td valign="top"><p>Unieke gebruikt URI van de resource</p></td>
        </tr>
        <tr>
            <td valign="top"><p>ID</p></td>
            <td valign="top"><p>Systeem gegenereerd</p></td>
            <td valign="top"><p>Door gebruiker gedefinieerde unieke naam van de resource (met dezelfde partition sleutelwaarde). Als de gebruiker geen een id is opgegeven, wordt een id automatisch worden gegenereerd</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Weergave van de kabel van resources
DocumentDB schrijft een eigen uitbreidingen niet naar de JSON-coderingen voor standaard- of speciale; Dit werkt met standaarddocumenten compatibele JSON.  
 
### <a name="addressing-a-resource"></a>Adressering van een resource
Alle resources zijn URI gebruikt. De waarde van de eigenschap **_self** van een resource de relatieve URI vertegenwoordigt van de resource. De indeling van de URI bestaat uit de /\<feed\>/ {_rid} padsegmenten:  

|Waarde van de _self |Beschrijving
|-------------------|-----------
|/DBS   |Feed van databases onder het databaseaccount van een
|/DBS/ {dbName}  |Database maken met een id die overeenkomt met de waarde {dbName}
|/DBS/ {dbName} /colls/   |Feed van verzamelingen onder een database
|/colls/ /DBS/ {dbName} {collName} |Siteverzameling met een id die overeenkomt met de waarde {collName}
|/colls/ /DBS/ {dbName} {collName} / documenten    |Feed van documenten in een siteverzameling
|/DBS/ {dbName} /colls/ {collName} /docs/ {docId}    |Document met een id die overeenkomt met de waarde {doc}
|/users/ /DBS/ {dbName}   |Feed van gebruikers onder een database
|/users/ /DBS/ {dbName} {gebruikers-id}   |Gebruiker met een id die overeenkomt met de waarde {gebruiker}
|/users/ /DBS/ {dbName} {gebruikers-id} / machtigingen   |Feed van machtigingen onder een gebruiker
|/DBS/ {dbName} {gebruikers-id} in de map /gebruikers/ /permissions/ {permissionId}    |Machtiging met een id die overeenkomt met de waarde {machtiging}
  
Elke resource heeft een unieke door de gebruiker gedefinieerde naam weergegeven via de eigenschap id. Opmerking: voor documenten, als de gebruiker geen een id, onze ondersteunde SDK's wordt Genereer automatisch een unieke id voor het document. De id is een door de gebruiker gedefinieerde reeks, maximaal 256 tekens die uniek is binnen de context van een resource specifieke bovenliggende. 

Elke resource heeft ook een hiërarchische resource systeem gegenereerde id (ook wel een RID genoemd), die beschikbaar zijn via de eigenschap _rid is. De RID de gehele hiërarchie van een bepaalde bron worden gecodeerd en is een handige interne weergave kon u referentiële integriteit afdwingen in een gedistribueerde wijze. De RID is uniek zijn binnen een databaseaccount en wordt intern gebruikt door DocumentDB voor efficiënte routering zonder cross partition zoekacties. De waarden van de _self en de eigenschappen van het _rid zijn canonieke zowel alternatieve weergaven van een resource. 

De DocumentDB REST API's ondersteuning adressering van resources en routering van aanvragen door de-id en de eigenschappen van het _rid.

## <a name="database-accounts"></a>Database-accounts
U kunt een of meer DocumentDB database accounts met uw Azure-abonnement inrichten.

U kunt [maken en beheren van DocumentDB database accounts](documentdb-create-account.md) via de Portal Azure op [http://portal.azure.com/](https://portal.azure.com/). Maken en beheren van een databaseaccount vereist beheerderstoegang en kan alleen worden uitgevoerd onder uw Azure-abonnement. 

### <a name="database-account-properties"></a>Eigenschappen van de database-account
U kunt als onderdeel van de inrichting van en beheren van een databaseaccount configureren en lees de volgende eigenschappen:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Naam van eigenschap</strong></p></td>
            <td valign="top"><p><strong>Beschrijving</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Consistentie beleid</p></td>
            <td valign="top"><p>Deze eigenschap instellen op het niveau van de consistentie standaard voor alle collecties onder uw databaseaccount configureren. U kunt het niveau van de consistentie op basis per aanvraag voor het gebruik van de koptekst [x-ms-consistentie-niveau] verzoek negeren. <p><p>Houd er rekening mee dat deze eigenschap alleen van toepassing op de <i>door de gebruiker gedefinieerde resources</i>. Alle systeem gedefinieerde resources zijn geconfigureerd voor de ondersteuning gelezen/query's met sterke consistentie.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Autorisatie toetsen</p></td>
            <td valign="top"><p>Hierna ziet u de primaire en secundaire outmodel als alleen-lezen sleutels voor administratieve toegang tot alle bronnen onder de databaseaccount.</p></td>
        </tr>
    </tbody>
</table>

Houd er rekening mee dat naast is geïnstalleerd, configureren en beheren van uw databaseaccount in de Portal Azure u kunt ook via programmacode maken en DocumentDB database accounts beheren met behulp van de [Azure DocumentDB REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx) , evenals de [SDK van clienthulpprogramma's](https://msdn.microsoft.com/library/azure/dn781482.aspx).  

## <a name="databases"></a>Databases
Een database DocumentDB is een logische container van een of meer siteverzamelingen en gebruikers, zoals wordt weergegeven in het volgende diagram. U kunt een willekeurig aantal databases onder een account van de database DocumentDB aanbieding beperkt maken.  

![Account en verzamelingen hiërarchische databasemodel][2]  
**Een Database is een logische container van gebruikers en siteverzamelingen**

Een database kan vrijwel onbeperkte documentopslag partitioneren door de verzamelingen, die de domeinen van de documenten in deze de transactie vormen bevatten. 

### <a name="elastic-scale-of-a-documentdb-database"></a>Elastische schaal van een database DocumentDB
Een database DocumentDB is elastische al dan niet standaard – die variëren van een paar GB naar petabytes van opslag van SSD back-documenten en ingerichte doorvoer. 

Een database in DocumentDB is anders dan bij een database in traditionele RDBMS, niet beperkt tot één computer. Met DocumentDB, de schaal van de toepassing behoeften wilt laten groeien, kunt u meer verzamelingen, databases of beide. Daadwerkelijk, verschillende toepassingen in de eerste derden in Microsoft gebruikten DocumentDB op consumenten schaal erg groot DocumentDB-databases van elke met duizenden verzamelingen met TB aan opslagcapaciteit document maken. U kunt vergroten of verkleinen van een database toevoegen of verwijderen van verzamelingen om te voldoen aan vereisten voor schaal van de toepassing. 

U kunt een willekeurig aantal collecties in een database onderhevig aan de aanbieding maken. Elke siteverzameling heeft SSD back-opslag en doorvoer afhankelijk van de prestaties van de geselecteerde laag voor u is ingericht.

Een database DocumentDB is ook een container van gebruikers. Een gebruiker, in inschakelen, is een logische naamruimte voor een reeks machtigingen die beschikbaar fijnmazige autorisatie en toegang tot siteverzamelingen, documenten en bijlagen zijn.  
 
Als u met de andere bronnen in het model van de resource DocumentDB, databases kunnen worden gemaakt, vervangen, verwijderd, lees of opgesomde eenvoudig met [Azure DocumentDB REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx) of een van de [client SDK's](https://msdn.microsoft.com/library/azure/dn781482.aspx). DocumentDB garandeert sterke consistentie om te lezen of query's uitvoeren de metagegevens van een resource database. Als u een database verwijdert, wordt automatisch zorgt ervoor dat u geen toegang een van de verzamelingen of gebruikers die zich daarin tot.   

## <a name="collections"></a>Siteverzamelingen
Een verzameling DocumentDB is een container voor uw JSON-documenten. Een siteverzameling is ook een maateenheid schaal voor transacties en query's. 

### <a name="elastic-ssd-backed-document-storage"></a>Opslag van elastische SSD back-documenten
Een siteverzameling is intrinsiek elastische - deze automatisch in omvang groeit en verkleind u documenten toevoegen of verwijderen. Verzamelingen logische resources en kunnen een of meer fysieke partities of servers beslaan. Het aantal partities binnen een siteverzameling wordt bepaald door DocumentDB op basis van de opslaggrootte en de ingerichte doorvoer van uw siteverzameling. Elke partition in DocumentDB heeft een vast bedrag SSD back-opslag die zijn gekoppeld en wordt gerepliceerd voor maximale beschikbaarheid. Partition management volledig wordt beheerd door Azure DocumentDB en u hoeft niet te complexe programmacode schrijven of het beheren van uw partities. DocumentDB verzamelingen zijn **vrijwel onbeperkte** opslag-en doorvoer. 

### <a name="automatic-indexing-of-collections"></a>Automatische indexeren van siteverzamelingen
DocumentDB is een databasesysteem waar schema-vrije. Deze niet wordt ervan uitgegaan dat of het vereisen van een schema voor de JSON-documenten. Als u documenten aan een siteverzameling toevoegen, DocumentDB indexeert ze automatisch en ze beschikbaar zijn voor u om query's. Automatische indexeren van documenten zonder schema of secundaire indexen is een belangrijke mogelijkheid van DocumentDB en is ingeschakeld door schrijven geoptimaliseerde, vergrendelen-vrij te geven en log gestructureerde index onderhoud technieken. DocumentDB ondersteunt continue volume van zeer snel schrijven terwijl nog steeds consistente query's. Document- en index opslag worden gebruikt voor het berekenen van de opslag dat door elke siteverzameling. U kunt de opslagruimte en prestaties voor-en nadelen die is gekoppeld aan het indexeren door de indexing beleid configureert voor een siteverzameling bepalen. 

### <a name="configuring-the-indexing-policy-of-a-collection"></a>Het indexeren beleid van een siteverzameling configureren
Het indexeren beleid van elke siteverzameling kunt u prestaties en opslag voor-en nadelen die is gekoppeld aan het indexeren. De volgende opties zijn beschikbaar om u als onderdeel van de Indexeerstatus configuratie:  

-   Kies of de verzameling automatisch alle documenten of niet indexeert. Standaard worden alle documenten automatisch geïndexeerd. U kunt automatische indexering uitschakelen en selectief alleen bepaalde documenten toevoegen aan de index. U kunt daarentegen selectief alleen bepaalde documenten uitsluiten. Dit doet u door de automatische eigenschap moet waar of onwaar op de indexingPolicy van een siteverzameling instellen en het gebruik van de koptekst [x-ms-indexingdirective] verzoek tijdens het invoegen, vervangen of verwijderen van een document.  
-   Kies of u wilt opnemen of uitsluiten van specifieke paden of patronen in uw documenten in de index. Dit kunt u bereiken door de instelling includedPaths en excludedPaths op de indexingPolicy van een siteverzameling respectievelijk. U kunt ook de opslagcapaciteit en de prestaties voor-en nadelen voor bereik en hash-query's voor specifieke pad patronen configureren. 
-   Kiezen tussen synchroon (consistente) en asynchroon (fikse) index-updates. De index wordt standaard synchroon bijgewerkt bij elke invoegen, vervangen of verwijderen van een document aan de collectie. Hiermee worden de query's ingaan op hetzelfde niveau als het document leest consistentie. Terwijl DocumentDB geoptimaliseerd schrijven is en continue hoeveelheden document schrijft samen met synchroon index onderhoud en op consistente query's ondersteunt, kunt u bepaalde siteverzamelingen als u wilt bijwerken hun index lazily kunt configureren. Fikse indexeren de prestaties van schrijven verder en is ideaal voor het bulksgewijs opname scenario's voor hoofdzakelijk gelezen veel siteverzamelingen.

Het indexeren beleid kan worden gewijzigd door te voeren van een positie op de verzameling. U kunt dit doen via de [client SDK](https://msdn.microsoft.com/library/azure/dn781482.aspx), de [Portal van Azure](https://portal.azure.com) of de [Azure DocumentDB REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx).

### <a name="querying-a-collection"></a>Query's uitvoeren van een siteverzameling
De documenten in een siteverzameling kunnen bevatten willekeurige schema's en kunt u documenten binnen een siteverzameling zoeken zonder een schema of secundaire indexen voorhand. U kunt de verzameling met de [DocumentDB SQL-syntaxis](https://msdn.microsoft.com/library/azure/dn782250.aspx), die uitgebreide, hiërarchische, relationele biedt, en ruimte operatoren en uitbreiden via JavaScript gebaseerde UDF's zoeken. JSON grammatica kan modeling JSON-documenten als appelbomen met labels als de structuurknooppunten. Hiermee wordt zowel door DocumentDB van automatische indexing technieken, alsmede van DocumentDB SQL-dialect benut. De taal van de query DocumentDB bestaat uit drie hoofdkenmerken:   

1.  Een klein aantal querybewerkingen die natuurlijk toewijzen aan de boomstructuur inclusief hiërarchische query's en prognoses. 
2.  Een subset van relationele bewerkingen samenstelling, filteren, prognoses, aggregaties en self joins. 
3.  Puur JavaScript gebaseerd UDF's die met werken (1) en (2).  

Het model van de query DocumentDB probeert te vinden van een balans tussen functionaliteit, efficiency en eenvoudig. De database-engine DocumentDB native gecompileerd en worden de SQL-query-instructies. Een verzameling de [Azure DocumentDB REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx) of een van de [client SDK's](https://msdn.microsoft.com/library/azure/dn781482.aspx)gebruikt, kunt u zoeken. De .NET SDK wordt geleverd met een provider voor LINQ.

> [AZURE.TIP] U kunt uitproberen DocumentDB en SQL-query's voor onze gegevensset in de [Query Speelplaats](https://www.documentdb.com/sql/demo)uitgevoerd.

### <a name="multi-document-transactions"></a>Meerdere documenten transacties
Databasetransacties bieden een veilige en overzichtelijk programming model voor omgaan met gelijktijdige wijzigingen in de gegevens. In RDBMS is de gebruikelijke manier om te schrijven bedrijfslogica schrijven **opgeslagen procedures** en/of **triggers** en verzendt het naar de database-server voor transacties worden uitgevoerd. In RDBMS, is de aard vereist voor het verwerken van twee verschillende talen: 

- De (niet-transacties) toepassing programmeertaal (bijvoorbeeld JavaScript, Python, C#, Java, enz.)
- T-SQL, de transacties programmeertaal die standaard is uitgevoerd door de database

DocumentDB vindt u op grond van het uitgebreide gebied van JavaScript en JSON rechtstreeks vanuit de database-engine, een intuïtieve programming model in voor het uitvoeren van JavaScript op basis van toepassingslogica rechtstreeks op de collecties met opgeslagen procedures en triggers. In deze sectie geeft voor beide van de volgende opties:

- Efficiënt implementatie van gelijktijdigheid bepalen, herstel, automatische indexering van de grafieken JSON object rechtstreeks in de database engine
- Natuurlijk uitdrukken besturingsstroom, variabele een bereik instellen, toewijzing en integratie van primitieven met databasetransacties rechtstreeks in de programmeertaal JavaScript verwerking van uitzonderingen

De JavaScript-logica geregistreerd bij een niveau van de siteverzameling kunt databasebewerkingen op de documenten van de opgegeven verzameling verlenen. DocumentDB terugloopt impliciet de JavaScript gebaseerd opgeslagen procedures en triggers binnen een de ZURE transacties met overgeslagen in documenten binnen een siteverzameling. In de loop van de uitvoering, als de JavaScript een uitzondering genereert, klikt u vervolgens de gehele transactie wordt afgebroken. De resulterende programming model is een zeer eenvoudige maar krachtige. JavaScript-ontwikkelaars krijgen een "duurzame" programming model terwijl ze nog steeds hun vertrouwde taalconstructies en bibliotheek primitieven.   

De mogelijkheid om uit te voeren JavaScript rechtstreeks vanuit de database-engine in dezelfde adresruimte als de buffer van toepassingen kunt zodat-transacties uitvoering van de database activiteiten ten opzichte van de documenten van een siteverzameling. Bovendien DocumentDB database engine zorgt ervoor dat een uitgebreide resourcebeperkingen naar de JSON en JavaScript lost eventuele impedantie verkeerde combinaties tussen de type-mailsystemen van toepassing en de database.   

Na het maken van een siteverzameling, kunt u opgeslagen procedures, triggers en UDF's met een siteverzameling met behulp van de [Azure DocumentDB REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx) of een van de [client SDK's](https://msdn.microsoft.com/library/azure/dn781482.aspx)registreren. Na registratie kunt u verwijzen naar en uitvoeren. Houd rekening met de volgende opgeslagen procedure geschreven volledig in JavaScript, de onderstaande code twee argumenten (de naam van het adresboek en de auteursnaam van de) en een nieuw document maakt, wordt gevraagd om een document en vervolgens wordt dit – alle binnen een impliciete ZURE transactie bijgewerkt. Op elk gewenst moment tijdens de uitvoering, als een JavaScript-uitzondering wordt gegenereerd, de hele transactie wordt afgebroken.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()
            
        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);
                
                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);
                        
                        context.getResponse().setBody(matchingDocuments.length);
                       
                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

De client kunt "verzenden" de bovenstaande JavaScript-logica in de database voor transacties uit te voeren via HTTP POST. Zie voor meer informatie over het gebruik van HTTP-methoden [RESTful interacties met DocumentDB resources](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


U ziet dat omdat de database native JSON en JavaScript begrijpt, er geen typen systeem niet overeenkomen, geen 'OR toewijzing' of code generatie bijzonder vereist.   

Opgeslagen procedures en triggers werken met een verzameling en de documenten in een verzameling door een duidelijke objectmodel, waarmee de huidige context voor de siteverzameling.  

Siteverzamelingen in DocumentDB kunnen worden gemaakt, verwijderd, lees of opgesomde gemakkelijk met behulp van de [Azure DocumentDB REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx) of een van de [client SDK's](https://msdn.microsoft.com/library/azure/dn781482.aspx). DocumentDB biedt altijd sterke consistentie om te lezen of query's uitvoeren de metagegevens van een siteverzameling. Automatisch verwijderen van een siteverzameling zorgt ervoor dat u geen toegang de documenten, bijlagen, opgeslagen procedures, triggers tot en UDF's daarin.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Opgeslagen procedures, triggers en de gebruiker gedefinieerde functies (UDF)
Zoals beschreven in de vorige sectie, kunt u de toepassingslogica rechtstreeks vanuit een transactie binnen de database-engine schrijven. De toepassingslogica kan worden geschreven volledig in JavaScript en kan worden gezien als een opgeslagen procedure, trigger of een UDF. De JavaScript-code in een opgeslagen procedure of een trigger kunt invoegen, vervangen, verwijderen, lees- of query documenten binnen een siteverzameling. De JavaScript binnen een UDF kan niet aan de andere kant, invoegen, vervangen of documenten verwijderen. UDF's opsommen van de documenten van een queryresultatenset en een andere resultatenset produceren. Voor meerdere pachtadres afgedwongen DocumentDB een beheermodel van de resource strikte reserveren die zijn gebaseerd. Elk opgeslagen procedure, trigger of een UDF krijgt een vaste quantum van besturingssysteem resources uitvoeren. Bovendien opgeslagen procedures, triggers of UDF's kunnen geen koppeling maken met externe JavaScript-bibliotheken en zijn gebeurd als ze de budgetten resource is toegewezen aan deze overschrijdt. U kunt vastleggen, opgeslagen procedures, triggers of UDF's met een verzameling unregister met behulp van de REST API's.  Bij registratie een opgeslagen procedure, trigger of een UDF vooraf gecompileerd en opgeslagen als byte-code die later wordt uitgevoerd. De volgende sectie illustreren hoe u de DocumentDB JavaScript-SDK kunt gebruiken voor het registreren, uitvoeren en een opgeslagen procedure, trigger en een UDF unregister. De JavaScript-SDK is een eenvoudige Wikkel via de [DocumentDB REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx). 

### <a name="registering-a-stored-procedure"></a>Een opgeslagen procedure registreren
Registratie van een opgeslagen procedure Hiermee maakt u een nieuwe opgeslagen procedure resource aan een siteverzameling via HTTP POST.  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();
            
            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };
    
    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>Een opgeslagen procedure uitvoeren
Uitvoering van een opgeslagen procedure wordt uitgevoerd door een HTTP POST ten opzichte van een bestaande opgeslagen procedure resource door parameters doorgeven aan de procedure in het hoofdgedeelte van de aanvraag uitgeven.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>Afmelden van een opgeslagen procedure
Afmelden van een opgeslagen procedure is gewoon klaar door het verwijderen van een HTTP ten opzichte van een bestaande opgeslagen procedure bron.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Registratie van een oude trigger
Registratie van een trigger wordt uitgevoerd door een nieuwe trigger resource maken voor een siteverzameling via HTTP POST. U kunt opgeven als de trigger een pre is of een post-trigger en het type bewerking deze gekoppeld aan (bijvoorbeeld maken, vervangen, verwijderen of alle worden kan).   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }
    
    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>Een oude trigger uitvoeren
Uitvoering van een trigger doet u door de naam van een bestaande trigger op het moment van het bericht/opslag/verwijderen van de resource van een document via de kop van het verzoek verzenden.  
 
    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>Afmelden van een oude trigger
Afmelden van een trigger is gewoon klaar via het verwijderen van een HTTP ten opzichte van een bestaande trigger bron verlenen.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Een UDF registreren
Registratie van een UDF wordt uitgevoerd door een nieuwe UDF resource maken voor een siteverzameling via HTTP POST.  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-the-query"></a>Een UDF als onderdeel van de query wordt uitgevoerd
Een UDF kan worden opgegeven als onderdeel van de SQL-query en wordt gebruikt als een manier om uit te breiden de core [SQL-querytaal van DocumentDB](https://msdn.microsoft.com/library/azure/dn782250.aspx).

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>Afmelden van een UDF 
Afmelden van een UDF is gewoon klaar door het verwijderen van een HTTP ten opzichte van een bestaande UDF bron.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Hoewel de bovenstaande fragmenten de registratie (POST), verwijdering (opslag), (GET) lijst/lezen en uit te voeren (POST) via de [DocumentDB JavaScript SDK](https://github.com/Azure/azure-documentdb-js), kunt u ook de [REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx) of andere [client SDK's](https://msdn.microsoft.com/library/azure/dn781482.aspx)gebruiken. 

## <a name="documents"></a>Documenten
U kunt invoegen, vervangen, verwijderen, lezen, opsommen en willekeurige JSON-documenten in een verzameling opvragen. DocumentDB schrijft niet voor een schema en hoeft geen secundaire indexen ter ondersteuning van query's uitvoeren op documenten in een siteverzameling.   

Wordt een behoorlijk geopende database-service, DocumentDB wordt niet voorraad eventuele gespecialiseerde gegevenstypen (bijvoorbeeld een datum-tijd) of specifieke coderingen voor JSON-documenten. Houd er rekening mee dat DocumentDB niet vereist is een speciale JSON conventies samen te vatten de relaties tussen verschillende documenten; de SQL-syntaxis van de DocumentDB biedt krachtige query voor hiërarchische en relationele operatoren query en project documenten zonder speciale aantekeningen of nodig samen te vatten relaties tussen documenten met onderscheiden eigenschappen.  
 
Als met alle andere bronnen, kunnen documenten worden gemaakt, vervangen, verwijderde, gelezen, opgesomde en aangevraagde eenvoudig met REST API's of een van de [client SDK's](https://msdn.microsoft.com/library/azure/dn781482.aspx). Als u een document verwijdert, wordt onmiddellijk vrijgemaakt het quotum die overeenkomt met alle geneste bijlagen. Het niveau lezen consistentie van documenten volgt het beleid consistentie van de databaseaccount. Dit beleid kan worden overschreven op basis van per aanvraag afhankelijk van de gegevens consistentie vereisten van uw toepassing. Query's naar documenten, volgt de gelezen consistentie de indexing-modus instellen voor de siteverzameling. Voor "consistente" gevolgd dit beleid voor consistentie van het account. 

## <a name="attachments-and-media"></a>Bijlagen en media
>[AZURE.NOTE] Bijlage en media resources zijn functies.
 
DocumentDB, kunt u voor de opslag van binaire BLOB's / media met DocumentDB of naar uw eigen externe media-store. Bovendien kunt u om aan te geven van de metagegevens van een media in een speciale document met de naam van de bijlage. Een bijlage in een DocumentDB is een speciale (JSON)-document dat verwijst naar de media/blob die elders is opgeslagen. Een bijlage is eenvoudig een speciale document dat wordt vastgelegd de metagegevens van een opgeslagen in de opslagruimte van een externe media media (bijvoorbeeld locatie, auteur, enzovoort). 

Houd rekening met een toepassing van de sociale lezen die DocumentDB gebruikt voor het opslaan van aantekeningen in inkt en metagegevens opmerkingen, inclusief worden gemarkeerd, bladwijzers, classificaties, vind ik leuks/ervaringen die zijn gekoppeld voor een e-mailadresboek van een bepaalde gebruiker enzovoort.   

-   De inhoud van het adresboek zelf is opgeslagen in de opslagruimte media ofwel beschikbaar als onderdeel van DocumentDB database-account of een externe media-winkel. 
-   Een toepassing mogelijk van elke gebruiker metagegevens opslaan als een afzonderlijke document--bijvoorbeeld Jan van metagegevens voor Map1 is opgeslagen in een document waarnaar wordt verwezen door /colls/joe/docs/book1. 
-   Bijlagen die wijst naar de inhoudspagina's van een bepaald boek van een gebruiker worden opgeslagen onder het bijbehorende document bijvoorbeeld /colls/joe/docs/book1/chapter1, /colls/joe/docs/book1/chapter2 enzovoort. 

Houd er rekening mee dat de voorbeelden hierboven wordt beschrijvende ID's gebruikt overbrengen van de hiërarchie van de resource. Resources zijn toegankelijk via de REST API's tot en met unieke resource-id's. 

De media dat wordt beheerd door DocumentDB, wordt de eigenschap _media van de bijlage verwijst naar de media door de URI. DocumentDB zorgt ervoor dat naar ongewenste verzamelen de media wanneer alle van de uitstaande verwijzingen worden niet weergegeven. DocumentDB automatisch genereert van de bijlage als u de nieuwe media uploaden en vult het _media zodat deze verwijzen naar de zojuist toegevoegde media. Als u kiest voor de opslag van de media in een externe blob store beheerd door u (bijvoorbeeld OneDrive of Azure-opslag, DropBox enzovoort), kunt u bijlagen als u verwijst naar de media. In dit geval u zelf maakt u de bijlage en de eigenschap _media vullen.   

Net als met alle andere bronnen, bijlagen kunnen worden gemaakt, vervangen, verwijderd, lezen of opgesomde eenvoudig met REST API's of een van de client SDK's. Als u met de documenten, volgt het niveau lezen consistentie van bijlagen het beleid consistentie van de databaseaccount. Dit beleid kan worden overschreven op basis van per aanvraag afhankelijk van de gegevens consistentie vereisten van uw toepassing. Bij het opvragen van bijlagen, volgt de gelezen consistentie de indexing-modus instellen voor de siteverzameling. Voor "consistente" gevolgd dit beleid voor consistentie van het account. 
 
## <a name="users"></a>Gebruikers
Een gebruiker DocumentDB vertegenwoordigt een logische naamruimte voor het groeperen van machtigingen. Een gebruiker DocumentDB mogelijk komen overeen met een gebruiker in een identiteitsbeheersysteem of een vooraf gedefinieerde toepassingsrol. Voor DocumentDB vertegenwoordigt een gebruiker gewoon een abstractie om te groeperen van een set machtigingen onder een database.   

Voor de uitvoering van meerdere pachtadres in uw toepassing, kunt u gebruikers in DocumentDB die overeenkomt met uw werkelijke gebruikers of de tenants van uw toepassing maken. Vervolgens kunt u machtigingen voor een bepaalde gebruiker die met de access-controle over de verschillende siteverzamelingen, documenten, bijlagen, enzovoort overeenkomen.   

Als uw toepassingen schalen met de groei van de gebruiker wilt, kunt u uw gegevens kunt shard op verschillende manieren vast. U kunt een model van elk van uw gebruikers als volgt:   

-   Elke gebruiker wordt toegewezen aan een database.
-   Elke gebruiker wordt toegewezen aan een siteverzameling. 
-   Documenten die overeenkomt met meerdere gebruikers gaat u naar een specifieke verzameling. 
-   Documenten die overeenkomt met meerdere gebruikers gaat u naar een reeks siteverzamelingen.   

Ongeacht de specifieke sharding strategie die u kiest, kunt u uw werkelijke gebruikers modelleren als gebruikers in de database DocumentDB en prima korrelig machtigingen voor elke gebruiker koppelen.  

![Gebruiker verzamelingen][3]  
**Sharding strategieën en modellering gebruikers**

Net als alle andere bronnen, gebruikers in DocumentDB kunnen worden gemaakt, vervangen, verwijderd, lezen of opgesomde eenvoudig met REST API's of een van de client SDK's. DocumentDB biedt altijd sterke consistentie om te lezen of query's uitvoeren de metagegevens van een gebruiker een resource. Het verdient aanbeveling af te wijzen dat een gebruiker automatisch verwijderen zorgt ervoor dat u geen toegang de machtigingen van deze tot. Hoewel de DocumentDB haalt de limiet van de machtigingen als onderdeel van de verwijderde gebruiker op de achtergrond, de verwijderde machtigingen is beschikbaar direct opnieuw kunt gebruiken.  

## <a name="permissions"></a>Machtigingen
Vanuit het perspectief in een access-besturingselement, resources zoals database-accounts databases, gebruikers en machtigingen worden beschouwd als *administratieve* resources omdat deze beheerdersmachtigingen is vereist. Bronnen, waaronder de verzamelingen, documenten, bijlagen, opgeslagen procedures, triggers en UDF's zijn aan de andere kant, beperkt onder een opgegeven database en beschouwd als *toepassing resources*. Overeenkomt met de twee soorten resources en de rollen die toegang deze (namelijk de beheerder en de gebruiker tot), wordt het model autorisatie gedefinieerd twee soorten *toegangstoetsen*: *outmodel* en *resource-sleutel*. De toets model is een onderdeel van de databaseaccount en is opgegeven naar de ontwikkelaar (of beheerder) die de databaseaccount is ingericht. Deze toets outmodel heeft de beheerder semantiek, in dat deze kan worden gebruikt voor het toestaan van toegang tot administratieve en toepassing resources. Een resource-sleutel is daarentegen een gedetailleerde toegangstoets voor toegang tot een *specifieke* bron van de toepassing. Zo wordt de verhouding tussen de gebruiker van een database en de machtigingen die de gebruiker voor een specifieke resource (bijvoorbeeld siteverzameling, document, bijlage, opgeslagen procedure, trigger of UDF heeft) vastgelegd.   

De enige manier om een resource-sleutel is door te maken van een resource machtiging onder een bepaalde gebruiker. Houd er rekening mee dat om te kunnen maken of een machtiging ophaalt, een basispagina-sleutel moet worden gepresenteerd in de koptekst autorisatie. Een resource machtiging verbindt de resource, de toegang en de gebruiker. Na het maken van een resource machtiging, moet de gebruiker alleen om aan te bieden de bijbehorende resource-toets voor toegang tot de gewenste resource. Een resource-sleutel kan daarom worden weergegeven als een logische en compacte weergave van de resource machtiging.  

Net als met alle andere bronnen, machtigingen in DocumentDB kunnen worden gemaakt, vervangen, verwijderd, lees of opgesomde eenvoudig met REST API's of een van de client SDK's. DocumentDB biedt altijd sterke consistentie om te lezen of query's uitvoeren de metagegevens van een machtiging. 

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het werken met bronnen HTTP-opdrachten gebruiken in [RESTful interacties met DocumentDB resources](https://msdn.microsoft.com/library/azure/mt622086.aspx).


[1]: media/documentdb-resources/resources1.png
[2]: media/documentdb-resources/resources2.png
[3]: media/documentdb-resources/resources3.png

