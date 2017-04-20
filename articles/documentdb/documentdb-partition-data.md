<properties 
    pageTitle="Partitioneren en schaal in Azure DocumentDB | Microsoft Azure"      
    description="Meer informatie over hoe partities werkt in Azure DocumentDB, hoe u configureert partitioneren en partitioneren toetsen en hoe u kiest u de juiste partition-toets voor uw toepassing."         
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/> 

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="arramac"/> 

# <a name="partitioning-and-scaling-in-azure-documentdb"></a>Partitioneren en schaal in Azure DocumentDB
[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) is bedoeld om u helpen snel en overzichtelijk prestaties en schaal naadloos samen met uw toepassing als deze in omvang groeit. In dit artikel bevat een overzicht van hoe partities werkt in DocumentDB en wordt beschreven hoe u DocumentDB siteverzamelingen als u uw toepassingen effectief wilt verkleinen kunt configureren.

Lees dit artikel en kunnen u de volgende vragen beantwoorden:   

- Hoe werkt partities in Azure DocumentDB?
- Hoe configureer ik partitioneren in DocumentDB
- Wat zijn de partition toetsen en hoe ik Kies de juiste partition-toets voor de toepassing?

Download het project uit [DocumentDB prestaties testen stuurprogramma voorbeeld](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark)om te beginnen met code. 

## <a name="partitioning-in-documentdb"></a>Partitioneren in DocumentDB

U kunt in DocumentDB, opslaan en schema minder JSON documenten met de volgorde van milliseconden antwoord tijden met een schaal opvragen. DocumentDB biedt containers voor het opslaan van gegevens **verzamelingen**genoemd. Verzamelingen logische resources en kunnen een of meer fysieke partities of servers beslaan. Het aantal partities wordt bepaald door DocumentDB op basis van de opslaggrootte en de ingerichte doorvoer van de siteverzameling. Elke partition in DocumentDB heeft een vast bedrag SSD back-opslag die zijn gekoppeld en wordt gerepliceerd voor maximale beschikbaarheid. Partition management volledig wordt beheerd door Azure DocumentDB en u hoeft niet te complexe programmacode schrijven of het beheren van uw partities. DocumentDB verzamelingen zijn **vrijwel onbeperkte** opslag-en doorvoer. 

Partitioneren is volledig doorzichtig in uw toepassing. DocumentDB ondersteunt snel lezen en schrijven, query's SQL en LINQ JavaScript op basis transacties logica, consistentie niveaus en fijnmazige toegangsbeheer via REST API oproepen naar een resource één siteverzameling. De service verwerkt distributie gegevens over partities en routering van queryaanvragen naar de juiste partition. 

Hoe werkt dit? Wanneer u een verzameling in DocumentDB maakt, ziet u dat er is een **belangrijke eigenschap partition** configuratie-waarde die u kunt opgeven. Dit is de JSON-eigenschap (of het pad) in uw documenten die kunnen worden gebruikt door DocumentDB distributie van uw gegevens tussen meerdere servers of partities. DocumentDB wordt de sleutelwaarde partition hash en het opgedeelde resultaat gebruiken om te bepalen de partition waarin de JSON-document wordt opgeslagen. Alle documenten met dezelfde partitiesleutel worden opgeslagen in de dezelfde partition. 

Stel een toepassing waarmee gegevens over werknemers en hun afdelingen in DocumentDB wordt opgeslagen. Laten we Kies `"department"` als de belangrijke eigenschap van partition, om te kunnen gegevens weggegooid met afdeling wilt verkleinen. Elk document in DocumentDB moet bevatten een verplichte `"id"` eigenschap aan die uniek zijn voor elk document met dezelfde partition sleutelwaarde, bijvoorbeeld moet `"Marketing`". Elk document dat is opgeslagen in een verzameling moet een unieke combinatie van partitiesleutel en -id, bijvoorbeeld hebben `{ "Department": "Marketing", "id": "0001" }`, `{ "Department": "Marketing", "id": "0002" }`, en `{ "Department": "Sales", "id": "0001" }`. Met andere woorden, de samengestelde eigenschap van (partition key, id) is de primaire sleutel voor uw siteverzameling.

### <a name="partition-keys"></a>Partition toetsen
De keuze van de partitiesleutel is een belangrijke beslissing die u moet maken tijdens de ontwerpfase. U moet de naam van een JSON-eigenschap die een groot aantal waarden bevat, waardoor waarschijnlijk hebben toegang patronen gelijkmatig verdeeld kiezen. De toets partition is opgegeven als een pad JSON, bijvoorbeeld `/department` vertegenwoordigt van de eigenschap-afdeling. 

De volgende tabel ziet u voorbeelden van partition belangrijke definities en de JSON-waarden die overeenkomen met elk.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Partition belangrijke pad</strong></p></td>
            <td valign="top"><p><strong>Beschrijving</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/Department</p></td>
            <td valign="top"><p>Komt overeen met de JSON-waarde van doc.department document waar het document is.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ Eigenschappen/naam</p></td>
            <td valign="top"><p>Komt overeen met de JSON-waarde van doc.properties.name waar document is voor het document (geneste eigenschap).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ID</p></td>
            <td valign="top"><p>Komt overeen met de JSON-waarde van doc.id (-id en partition toets zijn dezelfde eigenschap).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ "afdelingsnaam"</p></td>
            <td valign="top"><p>Komt overeen met de JSON-waarde van het document ["afdelingsnaam"] document waar het document is.</p></td>
        </tr>
    </tbody>
</table>

> [AZURE.NOTE] De syntaxis voor het pad van partition is vergelijkbaar met het opgegeven pad voor het indexeren beleid netwerkpaden door het belangrijkste verschil is dat het pad met de eigenschap in plaats van de waarde overeenkomt, dat wil zeggen de jokertekens aan het einde er is. Bijvoorbeeld, geeft u/afdeling /? Als u wilt de waarden onder afdeling indexeert, maar u /department als de sleutel Partitiedefinitie opgeven. Het pad van de partition impliciet wordt geïndexeerd en kan niet worden uitgesloten uit het indexeren indexing beleid negeren gebruiken.

Laten we eens kijken hoe de keuze van partitiesleutel invloed op de prestaties van uw toepassing.

### <a name="partitioning-and-provisioned-throughput"></a>Partities en ingerichte doorvoer
DocumentDB is bedoeld voor overzichtelijk prestaties. Wanneer u een siteverzameling maakt, kunt u reserveren doorvoer in ** [eenheden van de aanvraag](documentdb-request-units.md) (RU) per seconde**. Elk verzoek om een wordt een aanvraag eenheid boete die verhouding tot de hoeveelheid systeembronnen zoals CPU en IO verbruikt bij de bewerking is toegewezen. Het lezen van een document 1 kB met sessie consistentie verbruikt 1 verzoek eenheid. Lees is 1 RU ongeacht het aantal items die zijn opgeslagen of het aantal gelijktijdige aanvragen tegelijkertijd worden uitgevoerd. Grotere documenten vereisen hoger verzoek eenheden afhankelijk van de grootte. Als u de grootte van uw entiteiten en het aantal leest die u hoeft te ondersteunen voor uw toepassing kent, kunt u de exacte hoeveelheid doorvoer vereist is voor uw toepassing bevindt zich op de behoeften lezen inrichten. 

Wanneer DocumentDB worden documenten opgeslagen, deze Hiermee stelt u deze gelijkmatig tussen partities op basis van de sleutelwaarde partition. De doorvoer ook gelijkmatig wordt verdeeld over de beschikbare partities dat wil zeggen de doorvoer per partition = (totale doorvoer per siteverzameling) / (aantal partities). 

>[AZURE.NOTE] Om te bereiken van de volledige doorvoer van de siteverzameling, moet u een partitiesleutel waarmee u aanvragen tussen een aantal belangrijke waarden distinct partition gelijkmatig wordt verdeeld.

## <a name="single-partition-and-partitioned-collections"></a>Eén Partition en gepartitioneerde verzamelingen
DocumentDB ondersteunt het maken van één partition zowel gepartitioneerde collecties. 

- **Partitioned verzamelingen** kan meerdere partities's beslaan en zeer grote hoeveelheden opslagruimte en doorvoer ondersteunen. U moet een partitiesleutel voor de collectie opgeven.
- **Enkel-partition verzamelingen** hebben lagere prijs opties en de mogelijkheid om de query en transacties uitvoeren voor alle gegevens van de siteverzameling. Ze beschikken over de limieten schaalbaarheid en opslag van een enkel partition. U beschikt niet over een partitiesleutel voor deze collecties te geven. 

![Gepartitioneerde verzamelingen in DocumentDB][2] 

Voor scenario's die grote hoeveelheden opslag of doorvoer niet hoeven zijn verzamelingen van één partition thuishoort. Houd er rekening mee dat één partition verzamelingen de schaalbaarheid en opslaglimieten van een enkel partition, dat wil zeggen hebben maximaal 10 GB opslagruimte plus maximaal 10.000 verzoek eenheden per seconde. 

Gepartitioneerde verzamelingen kunnen zeer grote hoeveelheden opslagruimte en doorvoer ondersteunen. De standaardaanbiedingen zijn echter geconfigureerd voor het opslaan van maximaal 250 GB opslagruimte en schaal tot 250.000 verzoek eenheden per seconde. Als u nodig hoger opslag of doorvoer per siteverzameling hebt, neem contact op met [Azure ondersteuning](documentdb-increase-limits.md) als u wilt dat deze verbeterde voor uw account.

De volgende tabel bevat de verschillen in werken met een enkel-partition en gepartitioneerde siteverzamelingen:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Eén Partition siteverzameling</strong></p></td>
            <td valign="top"><p><strong>Gepartitioneerde siteverzameling</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Partition-toets</p></td>
            <td valign="top"><p>Geen</p></td>
            <td valign="top"><p>Vereist</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Primaire sleutel voor Document</p></td>
            <td valign="top"><p>"-id"</p></td>
            <td valign="top"><p>samengestelde sleutel &lt;partitiesleutel&gt; en "id"</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimale opslag</p></td>
            <td valign="top"><p>0 GB</p></td>
            <td valign="top"><p>0 GB</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximale opslag</p></td>
            <td valign="top"><p>10 GB</p></td>
            <td valign="top"><p>Onbeperkt (250 GB al dan niet standaard)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimumdoorvoer</p></td>
            <td valign="top"><p>400 verzoek eenheden per seconde</p></td>
            <td valign="top"><p>10.000 verzoek eenheden per seconde</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximumdoorvoer</p></td>
            <td valign="top"><p>10.000 verzoek eenheden per seconde</p></td>
            <td valign="top"><p>Onbeperkt (250.000 verzoek eenheden per seconde al dan niet standaard)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>API-versies</p></td>
            <td valign="top"><p>Alle</p></td>
            <td valign="top"><p>API-2015-12-16 en nieuwere</p></td>
        </tr>
    </tbody>
</table>

## <a name="working-with-the-sdks"></a>Werken met de SDK 's

Azure DocumentDB toegevoegd ondersteuning voor automatische partitioneren met [REST API versie 2015-12-16](https://msdn.microsoft.com/library/azure/dn781481.aspx). Om te gepartitioneerde siteverzamelingen maken, moet u SDK versies 1.6.0 downloaden of hoger op een van de ondersteunde platforms voor SDK (.NET, Node.js, Java, Python). 

### <a name="creating-partitioned-collections"></a>Gepartitioneerde verzamelingen maken

In het onderstaande voorbeeld ziet u een fragment .NET een verzameling om op te slaan apparaat telemetriegegevens 20.000 verzoek eenheden per seconde doorvoer maken. De SDK Hiermee stelt u de waarde OfferThroughput (die op zijn beurt Hiermee stelt u de `x-ms-offer-throughput` verzoek koptekst in de REST API). Hier we instellen de `/deviceId` als de partitiesleutel. De keuze van partitiesleutel wordt opgeslagen samen met de rest van de siteverzameling metagegevens, zoals naam en indexing beleid.

Dit voorbeeld wordt gekozen `deviceId` omdat we dat weten (a) aangezien er op een groot aantal apparaten, schrijft kunnen worden verdeeld over partities gelijkmatig en ons toestemming te schaal van de database voor het nemen van grote hoeveelheden gegevens en (b) veel van de aanvragen zoals het ophalen van de meest recente lezen voor een apparaat zijn beperkt tot een enkel deviceId en kunnen worden opgehaald uit een enkel partition.

    DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
    await client.CreateDatabaseAsync(new Database { Id = "db" });

    // Collection for device telemetry. Here the JSON property deviceId will be used as the partition key to 
    // spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
    // sorting against any number or string property.
    DocumentCollection myCollection = new DocumentCollection();
    myCollection.Id = "coll";
    myCollection.PartitionKey.Paths.Add("/deviceId");

    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri("db"),
        myCollection,
        new RequestOptions { OfferThroughput = 20000 });
        

> [AZURE.NOTE] Om te gepartitioneerde siteverzamelingen maken, moet u een doorvoercapaciteit > 10.000 verzoek eenheden per seconde. Aangezien doorvoer in veelvouden van 100, heeft dit tot het 10,100 of hoger.

Deze methode maakt een REST API bellen naar DocumentDB, terwijl de service wordt een aantal partities op basis van de gevraagde doorvoer inrichten. U kunt de doorvoer van een siteverzameling kunt wijzigen, zoals uw prestaties nodig is. Zie [Prestaties](documentdb-performance-levels.md) voor meer informatie.

### <a name="reading-and-writing-documents"></a>Lezen en schrijven van documenten

Nu kunnen we gegevens in DocumentDB invoegen. Hier volgt een voorbeeldklasse met een apparaat lezen, en een oproep aan CreateDocumentAsync om een nieuwe apparaat lezen in een verzameling invoegen.

    public class DeviceReading
    {
        [JsonProperty("id")]
        public string Id;

        [JsonProperty("deviceId")]
        public string DeviceId;

        [JsonConverter(typeof(IsoDateTimeConverter))]
        [JsonProperty("readingTime")]
        public DateTime ReadingTime;

        [JsonProperty("metricType")]
        public string MetricType;

        [JsonProperty("unit")]
        public string Unit;

        [JsonProperty("metricValue")]
        public double MetricValue;
      }

    // Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new DeviceReading
        {
            Id = "XMS-001-FE24C",
            DeviceId = "XMS-0001",
            MetricType = "Temperature",
            MetricValue = 105.00,
            Unit = "Fahrenheit",
            ReadingTime = DateTime.UtcNow
        });


Laten we lezen het document door de partitiesleutel en -id, moet u dit bijwerken, en als laatste stap, verwijdert u deze door partitiesleutel en -id. Opmerking die de leest een PartitionKey-waarde bevat (die overeenkomt met de `x-ms-documentdb-partitionkey` verzoek koptekst in de REST API).

    // Read document. Needs the partition key and the ID to be specified
    Document result = await client.ReadDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

    DeviceReading reading = (DeviceReading)(dynamic)result;

    // Update the document. Partition key is not required, again extracted from the document
    reading.MetricValue = 104;
    reading.ReadingTime = DateTime.UtcNow;

    await client.ReplaceDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      reading);

    // Delete document. Needs partition key
    await client.DeleteDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });



### <a name="querying-partitioned-collections"></a>Query's uitvoeren gepartitioneerde verzamelingen

Wanneer u gegevens in gepartitioneerde verzamelingen query uitvoert, stuurt DocumentDB automatisch de query aan de partities die overeenkomt met de partition-sleutelwaarden opgegeven in het filter (als er een). Deze query wordt bijvoorbeeld doorgestuurd naar alleen de partition met de partitiesleutel "XMS-0001".

    // Query using partition key
    IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"))
        .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");

De volgende query heeft geen een filter op de toets partition (DeviceId) en is fanned af tot alle partities waar deze ten opzichte van de partition index wordt uitgevoerd. Opmerking die u moet opgeven van de EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in de REST API) om de SDK voor het uitvoeren van een query meerdere partities.

    // Query across partition keys
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true })
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);

### <a name="parallel-query-execution"></a>Parallelle Query Execution

De DocumentDB SDK's 1.9.0 en hoger parallelle query execution ondersteuningsopties, waarmee u kunt het uitvoeren van lage latentie query's op gepartitioneerde verzamelingen, zelfs als ze nodig hebben met een groot aantal partities aanraakschermen. Bijvoorbeeld is de volgende query geconfigureerd om uit te voeren parallel meerdere partities.

    // Cross-partition Order By Queries
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
        .OrderBy(m => m.MetricValue);

U kunt parallelle query execution beheren door het optimaliseren van de volgende parameters:

- Door in te stellen `MaxDegreeOfParallelism`, kunt u de mate van parallellisme dat wil zeggen het maximum aantal gelijktijdige netwerkverbindingen aan de collectie partities bepalen. Als u dit instelt op -1, de mate van parallellisme wordt beheerd door de SDK. Als de `MaxDegreeOfParallelism` is niet opgegeven of ingestelde aan 0, dat wil de standaardwaarde zeggen, wordt er één netwerkverbinding aan de collectie partities.
- Door in te stellen `MaxBufferedItemCount`, kunt u zaken doet uitschakelen query latentie en client kant geheugengebruik. Als u deze parameter weglaat of dit instelt op -1, het aantal items buffer tijdens de queryuitvoering van parallelle wordt beheerd door de SDK.

Gegeven dezelfde staat van de siteverzameling, retourneert een parallelle query resultaten in dezelfde volgorde zoals in de seriële worden uitgevoerd. Bij het uitvoeren van een cross-partition-query waarin sorteren (ORDER BY en/of TOP), wordt de DocumentDB SDK problemen van de query parallel meerdere partities en samengevoegd gedeeltelijk gesorteerd resulteert in de client en globaal geordende resultaten te retourneren.

### <a name="executing-stored-procedures"></a>Opgeslagen procedures uitvoeren

U kunt ook atomische transacties ten opzichte van documenten met dezelfde apparaat-ID, bijvoorbeeld uitvoeren als u aggregaties of de meest recente status van een apparaat in één document onderhoudt. 

    await client.ExecuteStoredProcedureAsync<DeviceReading>(
        UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
        new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
        "XMS-001-FE24C");

We bekijken hoe u naar gepartitioneerde verzamelingen van één partition siteverzamelingen verplaatsen kunt in het volgende gedeelte.

<a name="migrating-from-single-partition"></a>
### <a name="migrating-from-single-partition-to-partitioned-collections"></a>Migreren van één partition naar gepartitioneerde verzamelingen
Wanneer een toepassing op basis van een siteverzameling één partition sneller worden verwerkt, moet (> 10.000 RU/s) of groter gegevensopslag (> 10GB), kunt u het [Hulpprogramma voor migratie van DocumentDB gegevens](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) om de gegevens van de één-verzameling naar een gepartitioneerde siteverzameling. 

Migreren van een siteverzameling één partition aan een gepartitioneerde verzameling

1. Gegevens exporteren vanuit de één-verzameling naar JSON. Zie [exporteren naar bestand JSON](documentdb-import-data.md#export-to-json-file) voor meer informatie.
2. De gegevens importeren in een gepartitioneerde verzameling die zijn gemaakt met een partition belangrijke definitie en meer dan 10.000 verzoek eenheden per tweede doorvoer, zoals wordt weergegeven in het onderstaande voorbeeld. Zie [DocumentDB importeren](documentdb-import-data.md#DocumentDBSeqTarget) voor meer informatie.

![Gegevens aan een verzameling Partitioned in DocumentDB migreren][3]  

>[AZURE.TIP] Voor importeren te versnellen, kunt u bovendien het nummer van parallelle aanvragen voor 100 of hoger om te profiteren van de sneller worden verwerkt beschikbaar voor gepartitioneerde collecties met groter wordende. 

Nu dat we de basisbeginselen hebt uitgevoerd, eens enkele belangrijke overwegingen wanneer u werkt met partition toetsen in DocumentDB.

## <a name="designing-for-partitioning"></a>Ontwerpen voor partitioneren
De keuze van de partitiesleutel is een belangrijke beslissing die u moet maken tijdens de ontwerpfase. In deze sectie worden enkele van de compromissen nodig zijn voor het selecteren van een partitiesleutel voor uw siteverzameling.

### <a name="partition-key-as-the-transaction-boundary"></a>Partitiesleutel als de transactiegrens
Uw keuze van partitiesleutel moet saldo vanaf dat u het gebruik van transacties ten opzichte van de vereiste naar uw entiteiten verdelen over meerdere partition toetsen om ervoor te zorgen een scalable oplossing inschakelen. Op één extreme, u kunt dezelfde partitiesleutel instellen voor al uw documenten, maar dit kan de schaalbaarheid van uw oplossing beperken. Aan de andere extreme, kunt u een unieke partitiesleutel voor elk document, dat zou ten zeerste scalable maar wilt voorkomen dat u gebruikmaakt van cross document transacties via opgeslagen procedures en triggers toewijzen. Een ideaal partitiesleutel is een waarmee u kunt het gebruiken van efficiënt query's en die voldoende verhouding om ervoor te zorgen dat uw oplossing bestaat uit scalable heeft. 

### <a name="avoiding-storage-and-performance-bottlenecks"></a>Opslag en prestaties knelpunten voorkomen 
Het is ook belangrijk voor het kiezen van een eigenschap die kan worden verdeeld over een aantal unieke waarden. Aanvragen voor dezelfde partitiesleutel niet langer zijn dan de doorvoer van een enkel partition en wordt de snelheid. Het is dus belangrijk om te kiezen van een partitiesleutel die niet in **"warm nét"** binnen de toepassing resulteert. De totale opslagcapaciteit voor documenten met dezelfde partitiesleutel kan ook niet groter is dan 10 GB in opslag. 

### <a name="examples-of-good-partition-keys"></a>Voorbeelden van goede partitiesleutels
Hier volgen enkele voorbeelden voor het kiezen van de toets partition voor uw toepassing:

* Als u een gebruikersprofiel backend implementeren bent, klikt u vervolgens is de gebruikers-ID een goede keuze voor partitiesleutel.
* Als u IoT gegevens, bijvoorbeeld de status van het apparaat hebt opgeslagen, is een apparaat-ID een goede keuze voor partitiesleutel.
* Als u DocumentDB voor het vastleggen van tijd-reeks gegevens gebruikt, is de ID van de hostname of proces een goede keuze voor partitiesleutel.
* Als er een tenant multi-architectuur, is de ID van de tenant een goede keuze voor partitiesleutel.

Houd er rekening mee dat in sommige gevallen gebruiken (zoals de IoT en gebruiker profielen hierboven beschreven) de toets partition mogelijk niet hetzelfde zijn als id (document toets). In anderen zoals de tijd reeksgegevens hebt u mogelijk een partitiesleutel die verschilt van de-id.

### <a name="partitioning-and-loggingtime-series-data"></a>Gegevens partities en logboekregistratie-tijdreeks
Een van de meest voorkomende gebruik gevallen van DocumentDB is bedoeld voor logboekregistratie en telemetrielogboek. Het is belangrijk om te kiezen een goede partitiesleutel aangezien u alleen-lezen moet mogelijk/schrijven grote hoeveelheden gegevens. De keuze wordt, hangt af van uw lezen en schrijven tarieven en soorten query's die u verwacht om uit te voeren. Hier volgen enkele tips over het kiezen van een goede partitiesleutel.

- Als er sprake is uw use-case een kleine tarief van schrijft acculumating gedurende een lange periode en hoeft te query door bereiken van tijdstempels en andere filters, klikt u vervolgens bijvoorbeeld met een combinatie van de tijdstempel datum als een partitiesleutel een goede manier is. Hiermee kunt u op query over alle gegevens voor een datum in een enkel partition. 
- Als uw werkzaamheden schrijven dik, dat wil doorgaans komt vaker is zeggen, moet u een partitiesleutel die niet gebaseerd op tijdstempel zodat DocumentDB gelijkmatig schrijft over een aantal partities verdelen kunt. Hier is een hostname, proces-ID, activiteits-ID of een andere eigenschap met hoge verhouding een goede keuze. 
- Een derde benadering is een hybride een waar u hebt meerdere siteverzamelingen, één voor elke dag/maand en de toets partition is een gedetailleerde eigenschap als hostname. Het voordeel die u kunt verschillende prestaties op basis van het tijdvenster instellen is, bijvoorbeeld de verzameling voor de huidige maand is ingericht met sneller worden verwerkt omdat deze fungeert lezen en schrijven, terwijl de vorige maanden met Verlaag doorvoer, aangezien ze alleen lezen fungeren.

### <a name="partitioning-and-multi-tenancy"></a>Partitioneren en meerdere pachtadres
Als u een DocumentDB met meerdere tenant-toepassing implementeert, zijn er twee belangrijkste patronen voor de uitvoering van pachtadres met DocumentDB – één partitiesleutel per pachter en één siteverzameling per pachter. Hier volgen de voor- en nadelen voor elk label:

* Één partitiesleutel per pachter: In dit model, tenants worden collocated binnen een enkele verzameling. Maar query's en wordt ingevoegd voor documenten binnen een enkel tenant kunnen worden uitgevoerd ten opzichte van een enkel partition. U kunt ook transacties logica implementeren in alle documenten binnen een tenant. Aangezien meerdere tenants een verzameling deelt, kunt u opslagruimte en doorvoer kosten besparen door resources voor tenants binnen een enkele verzameling groepsgewijze in plaats van extra ruimte voor elke tenant is geïnstalleerd. Het nadeel is dat u geen prestaties moeten worden geïsoleerd per pachter. Prestaties/doorvoer toename van toepassing op de hele siteverzameling tegenover gerichte toeneemt voor tenants.
* Een siteverzameling per pachter: elke tenant heeft een eigen siteverzameling. In dit model, kunt u de prestaties per pachter reserveren. Met de DocumentDB nieuwe verbruik op basis van prijzen model is dit model meest efficiënt voor meerdere tenant toepassingen met een klein aantal tenants.

U kunt ook een combinatie/doorverbonden aanpak die kleine tenants collocates en grotere tenants migreert naar hun eigen siteverzameling gebruiken.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt wordt beschreven hoe partities werkt in Azure DocumentDB, hoe u gepartitioneerde verzamelingen kunt maken en hoe u een goede partitiesleutel kunt kiezen voor uw toepassing. 

-   Schaal en prestaties testen met DocumentDB uitvoeren. Zie [prestaties en schaal testen met Azure DocumentDB](documentdb-performance-testing.md) voor een steekproef.
-   Aan de slag met de [SDK's](documentdb-sdk-dotnet.md) of de [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx) kleurcodering
-   Meer informatie over [ingerichte doorvoersnelheid in DocumentDB](documentdb-performance-levels.md)
-   Als u aanpassen hoe uw toepassing uitvoert partitioneren wilt, kunt u uw eigen aan de clientzijde partities implementatie aansluiten. Zie [aan de clientzijde partitioneren ondersteuning](documentdb-sharding.md).

[1]: ./media/documentdb-partition-data/partitioning.png
[2]: ./media/documentdb-partition-data/single-and-partitioned.png
[3]: ./media/documentdb-partition-data/documentdb-migration-partitioned-collection.png  

 
