<properties 
    pageTitle="DocumentDB Database vragen - Veelgestelde vragen | Microsoft Azure" 
    description="Antwoorden op veelgestelde vragen over Azure DocumentDB een NoSQL document database-service voor JSON. Database vragen over capaciteit, prestaties en schaalbaarheid beantwoorden." 
    keywords="Database vragen, veelgestelde vragen, documentdb, azure, Microsoft azure"
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
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="mimig"/>


#<a name="frequently-asked-questions-about-documentdb"></a>Veelgestelde vragen over DocumentDB

## <a name="database-questions-about-microsoft-azure-documentdb-fundamentals"></a>Database vragen over de grondbeginselen van Microsoft Azure DocumentDB

### <a name="what-is-microsoft-azure-documentdb"></a>Wat is Microsoft Azure DocumentDB? 
Microsoft Azure DocumentDB is een wereld-schaal NoSQL document database-als-een-service die biedt uitgebreide query over gegevens schema-vrij te geven, kan de prestaties kunnen worden geconfigureerd en betrouwbare en snelle ontwikkeling, alles door een beheerde platform ondersteund door de kracht in- en bereiken van Microsoft Azure van geweldige snelle. DocumentDB is de juiste oplossing voor het web, mobiele, spellen en IoT toepassingen wanneer overzichtelijk doorvoer, beschikbaarheid, lage latentie en een schema-vrije gegevensmodel zijn belangrijke vereisten. DocumentDB biedt schema flexibiliteit en RTF indexeren via een systeemeigen JSON-gegevensmodel en ondersteuning van transacties met meerdere documenten met geïntegreerde JavaScript bevat.  
  
Zie voor meer database vragen, antwoorden en instructies voor implementatie en het gebruik van deze service, de [DocumentDB documentatiepagina](https://azure.microsoft.com/documentation/services/documentdb/).

### <a name="what-kind-of-database-is-documentdb"></a>Welk soort database is DocumentDB?
DocumentDB is een NoSQL document oriënteren-database waarin gegevens in de indeling van JSON opgeslagen.  DocumentDB ondersteunt geneste, contained gegevens structuren die kunnen worden gezocht een uitgebreide DocumentDB [SQL-query grammatica](documentdb-sql-query.md). DocumentDB biedt krachtige transacties verwerking van serverzijde JavaScript tot en met [opgeslagen procedures, triggers, en de gebruiker gedefinieerde functies](documentdb-programming.md). De database ondersteunt ook ontwikkelaars instelbare consistentie niveaus met bijbehorende [prestaties](documentdb-performance-levels.md).
 
### <a name="do-documentdb-databases-have-tables-like-a-relational-database-rdbms"></a>Heb DocumentDB databases tabellen zoals een relationele database (RDBMS)?
Nee, worden de gegevens van DocumentDB opgeslagen in verzamelingen van JSON-documenten.  Zie voor informatie over DocumentDB bronnen [DocumentDB resource model en concepten](documentdb-resources.md). Zie voor meer informatie over hoe NoSQL oplossingen zoals DocumentDB van relationele oplossingen verschillen, [NoSQL tegenover SQL](documentdb-nosql-vs-sql.md).

### <a name="do-documentdb-databases-support-schema-free-data"></a>Ondersteunen DocumentDB databases schema gegevens?
Ja, geeft DocumentDB toepassingen willekeurige JSON documenten zonder schemadefinitie of hints opslaan. Gegevens zijn onmiddellijk beschikbaar voor de query wordt uitgevoerd via de DocumentDB SQL-query-interface.   

### <a name="does-documentdb-support-acid-transactions"></a>Ondersteunt DocumentDB ZURE transacties?
Ja, ondersteunt DocumentDB cross-document transacties uitgedrukt als JavaScript opgeslagen procedures en triggers. Transacties zijn beperkt tot een enkel partition binnen elke verzameling en worden uitgevoerd met ZURE semantiek alles of niets geïsoleerd van andere aanvragen in gelijktijdig uitvoering code en gebruiker.  Als de uitzonderingen zijn gegenereerd door de uitvoering van de server kant JavaScript-code van toepassing, de hele transactie hersteld. Zie voor meer informatie over transacties, [databasetransacties programma](documentdb-programming.md#database-program-transactions).

### <a name="what-are-the-typical-use-cases-for-documentdb"></a>Wat zijn de veelvoorkomend gebruik dozen voor DocumentDB?  
DocumentDB is een goede keuze voor nieuwe web, mobiele, spellen en IoT toepassingen waar Automatische schaal overzichtelijk prestaties volgorde van milliseconden antwoord tijden en de mogelijkheid om query snel over schema-vrije gegevens belangrijk is. DocumentDB gepaard met snelle ontwikkeling en ondersteunende de continue iteratie van gegevensmodellen van toepassing. Toepassingen die gegenereerd gebruikersinhoud en gegevens beheren zijn [algemene gebruik dozen voor DocumentDB](documentdb-use-cases.md).  

### <a name="how-does-documentdb-offer-predictable-performance"></a>Hoe biedt DocumentDB overzichtelijk prestaties?
Een [aanvraag eenheid](documentdb-request-units.md) is de maateenheid van doorvoersnelheid in DocumentDB. 1 RU overeenkomt met de doorvoer van het ophalen van een document 1KB. Elke bewerking in DocumentDB, inclusief lezen, schrijven, SQL-query's en opgeslagen procedure executions heeft een deterministic RU-waarde op basis van de doorvoer vereist om de bewerking te voltooien. In plaats van na te denken over CPU, IO en geheugen en hoe ze elke van invloed zijn op uw toepassing doorvoer, kunt u denkt dat met een enkel RU eenheid.

Elke verzameling DocumentDB kan worden gereserveerd met ingerichte doorvoer met RUs doorvoer per seconde. Voor toepassingen van een schaal, kunt u afzonderlijke aanvragen voor het meten van hun RU waarden en inrichten collecties met het verwerken van het totaal van de aanvraag eenheden voor alle aanvragen gebruikelijke. U kunt ook vergroten of verkleinen van de verzameling doorvoer als de behoeften van uw toepassing evolve. Voor meer informatie over het verzoek eenheden en voor hulp bij het nodig heeft bepalen van de verzameling, Lees [beheren prestaties en capaciteit](documentdb-manage.md) en probeert u de [doorvoer Rekenmachine](https://www.documentdb.com/capacityplanner). 

### <a name="is-documentdb-hipaa-compliant"></a>Compatibel is met DocumentDB HIPAA?
Ja, is DocumentDB HIPAA voldoen. HIPAA tot stand brengt vereisten voor het gebruik, uitlekt, en beveiligen van geïdentificeerd systeemstatus gegevens. Zie het [Microsoft-Vertrouwenscentrum](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA)voor meer informatie.

### <a name="what-are-the-storage-limits-of-documentdb"></a>Wat zijn de opslaglimieten van DocumentDB? 
Er is geen theoretische limiet voor de totale hoeveelheid gegevens die een siteverzameling kunt opslaan in DocumentDB. Als u meer dan 250 GB van gegevens binnen één collectie opslaan wilt, kunt u [contact opnemen met ondersteuning](documentdb-increase-limits.md) om de quota voor uw account verhoogd. 

### <a name="what-are-the-throughput-limits-of-documentdb"></a>Wat zijn de beperkingen doorvoer van DocumentDB? 
Er is geen theoretische limiet voor de totale hoeveelheid doorvoer die een siteverzameling kunt ondersteuning in DocumentDB, als uw werkbelasting kunt worden verdeeld ongeveer gelijkmatig over een voldoende groot aantal partition toetsen. Als u 250.000 verzoek eenheden/tweede per siteverzameling of account overschrijden wilt, moet u [contact opnemen met ondersteuning](documentdb-increase-limits.md) naar de quota voor uw account verhoogd hebt. 

### <a name="how-much-does-microsoft-azure-documentdb-cost"></a>Hoeveel kost Microsoft Azure DocumentDB?
Raadpleeg de detailpagina van [DocumentDB prijzen](https://azure.microsoft.com/pricing/details/documentdb/) voor meer informatie. Gebruikskosten DocumentDB wordt bepaald door het aantal siteverzamelingen in gebruik, het aantal uren de verzamelingen online zijn, en de verbruikte opslag en de ingerichte doorvoer voor elke siteverzameling. 

### <a name="is-there-a-free-account-available"></a>Is er een gratis account beschikbaar?
Als u eerder met Azure, kunt u zich kunt aanmelden voor een [gratis Azure-account](https://azure.microsoft.com/free/), waarmee u 30 dagen en $200 om te proberen de Azure services. Of, als u een Visual Studio-abonnement hebt, u bent in aanmerking komen voor [$150 in gratis Azure tegoeden per maand](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) om te gebruiken op een Azure-service.  

### <a name="how-can-i-get-additional-help-with-documentdb"></a>Hoe kan ik extra hulp bij DocumentDB verkrijgen?
Als u een hulp nodig hebt, contact maken met ons op de [Stapel overloop](http://stackoverflow.com/questions/tagged/azure-documentdb), de [Azure DocumentDB MSDN Ontwikkelaarsforums](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB), of een [1:1 chatten met het technische team DocumentDB](http://www.askdocdb.com/)plannen. Als u wilt blijven op de hoogte van de laatste DocumentDB nieuws en functies, volgt u ons op het [Twitter](https://twitter.com/DocumentDB).

## <a name="set-up-microsoft-azure-documentdb"></a>Microsoft Azure DocumentDB instellen

### <a name="how-do-i-sign-up-for-microsoft-azure-documentdb"></a>Hoe meld ik aanmelding voor Microsoft Azure DocumentDB?
Microsoft Azure DocumentDB is beschikbaar in de [Portal van Azure][azure-portal].  Eerst moet u aanmelden voor een Microsoft Azure-abonnement.  Nadat u zich aanmeldt voor een Microsoft Azure-abonnement, kunt u een DocumentDB-account toevoegen aan uw Azure-abonnement. Zie voor instructies over het toevoegen van een account DocumentDB, [een account van de database DocumentDB maken](documentdb-create-account.md).   

### <a name="what-is-a-master-key"></a>Wat is een diamodel-sleutel?
Een basispagina-sleutel is een beveiligingstoken voor toegang tot alle resources voor een account. Personen met de toets hebt gelezen en schrijftoegang tot de alle resources in de databaseaccount. Wees voorzichtig bij outmodel toetsen distribueren. De primaire basispagina en secundaire outmodel sleutel zijn beschikbaar in het blad **toetsen **van de [Portal van Azure][azure-portal]. Zie voor meer informatie over sleutels [weergave, kopiëren en opnieuw genereren toegangstoetsen](documentdb-manage-account.md#keys).

### <a name="how-do-i-create-a-database"></a>Hoe maak ik een database?
U kunt databases met behulp van de [Azure-Portal]() , zoals beschreven in [een database DocumentDB maken](documentdb-create-database.md), een van de [DocumentDB SDK's](documentdb-sdk-dotnet.md)of via de [REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx)maken.  

### <a name="what-is-a-collection"></a>Wat is een siteverzameling?
Een verzameling is een container van JSON-documenten en de bijbehorende logica voor JavaScript-toepassing. Een verzameling is een factureerbare entiteit, waarbij de [kosten](documentdb-performance-levels.md) wordt bepaald door de doorvoer en storaged gebruikt. Verzamelingen kunnen een of meer partities/servers omvatten en kunnen schalen om af te handelen vrijwel onbeperkte hoeveelheden opslag of doorvoer.

Verzamelingen zijn ook de facturering entiteiten voor DocumentDB. Elke verzameling gefactureerd per uur op basis van de ingerichte doorvoer en de opslagruimte gebruikt. Zie [DocumentDB prijzen](https://azure.microsoft.com/pricing/details/documentdb/)voor meer informatie.  

### <a name="how-do-i-set-up-users-and-permissions"></a>Hoe stel ik gebruikers en machtigingen?
U kunt gebruikers en machtigingen met een van de [DocumentDB SDK's](documentdb-sdk-dotnet.md) of via de [REST API's](https://msdn.microsoft.com/library/azure/dn781481.aspx)maken.   

## <a name="database-questions-about-developing-against-microsoft-azure-documentdb"></a>Database vragen over het ontwikkelen van ten opzichte van Microsoft Azure DocumentDB

### <a name="how-to-do-i-start-developing-against-documentdb"></a>Hoe moet ik starten ontwikkelen tegen DocumentDB?
[SDK's](documentdb-sdk-dotnet.md) zijn beschikbaar voor .NET, Python Node.js, JavaScript en Java.  Ontwikkelaars kunnen ook gebruikmaken van de [RESTful HTTP-API's](https://msdn.microsoft.com/library/azure/dn781481.aspx) om te communiceren met DocumentDB resources uit allerlei platforms en verschillende talen. 

Voorbeelden voor het DocumentDB [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md)en [Python](documentdb-python-samples.md) SDK's zijn beschikbaar op GitHub.

### <a name="does-documentdb-support-sql"></a>Ondersteunt DocumentDB SQL?
De taal DocumentDB SQL-query is een uitgebreide subset van de queryfunctionaliteit die worden ondersteund door SQL. De taal DocumentDB SQL-query biedt uitgebreide, hiërarchische en relationele operatoren en uitbreiden via JavaScript op basis van gebruiker gedefinieerde functies (UDF). JSON grammatica kan modeling JSON-documenten als appelbomen met labels als de structuurknooppunten, die wordt gebruikt door zowel de DocumentDB automatische indexing-technieken als de SQL-query-dialect van DocumentDB.  Voor meer informatie over het gebruik van de SQL-grammatica, raadpleegt u de [Query DocumentDB] [ query] artikel.

### <a name="what-are-the-data-types-supported-by-documentdb"></a>Wat zijn de gegevenstypen die worden ondersteund door DocumentDB?
De primitieve gegevenstypen die worden ondersteund in DocumentDB zijn hetzelfde als JSON. JSON heeft een eenvoudige typesysteem die uit tekenreeksen, getallen (IEEE754 dubbele precisie) en Booleaanse waarden - waar, onwaar en null-waarden bestaat.  Meer complexe gegevenstypen zoals DateTime, Guid, Int64 en geometrie kunnen worden weergegeven in JSON en DocumentDB tot en met het maken van geneste objecten met de operator {} en matrices met de operator []. 

### <a name="how-does-documentdb-provide-concurrency"></a>Hoe biedt DocumentDB gelijktijdigheid?
DocumentDB ondersteunt optimistische gelijktijdigheid besturingselement (OCC) tot en met HTTP entiteit labels of etags. Elke resource DocumentDB heeft een etag en de etag is ingesteld op de server telkens wanneer een document wordt bijgewerkt. De kop etag en de huidige waarde worden in alle antwoordberichten opgenomen. Etags kan worden gebruikt met het If-Match koptekst toe te staan dat de server te bepalen als een resource moet worden bijgewerkt. De waarde van het If-Match is de waarde etag gecontroleerd. Als de waarde etag overeenkomt met de server etag-waarde, kunt u de resource wordt bijgewerkt. Als de etag niet langer huidige is, de server weigert de bewerking met een "HTTP 412 voorwaarde is mislukt" antwoordcode. De client moet vervolgens opnieuw ophalen van de resource voor de huidige etag-waarde voor de resource. Bovendien kan etags met If-None-Match koptekst worden gebruikt om te bepalen of een opnieuw ophalen van een resource nodig is. 

Om de optimistische gelijktijdigheid in .NET gebruiken de klasse [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) . Zie voor een steekproef .NET, [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) in de steekproef DocumentManagement op github.

### <a name="how-do-i-perform-transactions-in-documentdb"></a>Hoe ik transacties uitvoeren in DocumentDB?
DocumentDB ondersteunt taal geïntegreerde transacties via JavaScript opgeslagen procedures en triggers. Alle databasebewerkingen in scripts worden uitgevoerd binnen bereik van de verzameling als dit een verzameling één partition is, of met dezelfde partition sleutelwaarde binnen een verzameling documenten, als de verzameling partitioneren is overgeslagen. Een momentopname van de versies van een document (ETags) is die u aan het begin van de transactie hebt gemaakt en vastgelegde alleen als het script is geslaagd. Als de JavaScript een fout genereert, de transactie hersteld. Zie [DocumentDB serverzijde programming](documentdb-programming.md) voor meer informatie.

### <a name="how-can-i-bulk-insert-documents-into-documentdb"></a>Hoe kan ik documenten invoegen in DocumentDB bulksgewijs? 
Er zijn drie manieren bulksgewijs invoegen documenten in DocumentDB:

- De gegevens migratieprogramma, zoals wordt beschreven in [de gegevens importeren naar DocumentDB](documentdb-import-data.md).
- Document Explorer in de Portal van Azure, zoals wordt beschreven in een [groot aantal documenten met Document Explorer toevoegen](documentdb-view-json-document-explorer.md#BulkAdd).
- Opgeslagen procedures uit, zoals beschreven in [DocumentDB serverzijde programming](documentdb-programming.md).

### <a name="does-documentdb-support-resource-link-caching"></a>DocumentDB ondersteunt caching van resource-koppeling?
Ja, omdat DocumentDB een RESTful service is, resource-koppelingen zijn onveranderlijke en kunnen worden opgeslagen. DocumentDB clients kunnen een header 'If-None-Match' opgeven voor leest ten opzichte van een bron, zoals document of de siteverzameling en de update van hun lokale gekopieerd alleen wanneer de serverversie heeft wijzigen. 

### <a name="is-a-local-instance-of-documentdb-available"></a>Een lokaal exemplaar van DocumentDB beschikbaar is?
Een lokaal exemplaar van DocumentDB is op dit moment niet beschikbaar. U kunt de status van een lokale emulator en stem voor deze op het [Feedbackforum](https://feedback.azure.com/forums/263030-documentdb/suggestions/6328798-standalone-local-instance)kunt bijhouden.


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
 
