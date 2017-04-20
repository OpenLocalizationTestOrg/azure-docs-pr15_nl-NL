<properties 
    pageTitle="Azure SQL-Database verbinden met Azure zoekprogramma's via indexeerfuncties | Microsoft Azure | Indexeerfuncties" 
    description="Leer hoe u gegevens ophalen uit Azure SQL-Database aan een Azure zoekindex indexeerfuncties gebruiken." 
    services="search" 
    documentationCenter="" 
    authors="chaosrealm" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="05/26/2016" 
    ms.author="eugenesh"/>

#<a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Azure SQL-Database verbinden met Azure zoekprogramma's via indexeerfuncties

Azure Search-service is een gehoste cloud-zoekservice waarmee u gemakkelijk een geweldige zoekervaring te bieden. Voordat u zoeken kunt, moet u een index Azure zoeken met uw gegevens te vullen. Als de gegevens worden opgeslagen in een Azure SQL-database, kunt u de indexing proces met de nieuwe **Azure zoeken indexering voor Azure SQL-Database** (of **Azure SQL-indexering** voor korte) in Azure zoeken automatiseren. Dit betekent dat er minder code te schrijven en minder infrastructuur naar belangrijk vindt.

Indexeerfuncties werken op dit moment alleen met Azure SQL-Database, SQL Server op Azure VMs en [Azure DocumentDB](../documentdb/documentdb-search-indexer.md). In dit artikel richten we ons op indexeerfuncties die met Azure SQL-Database werken. Als u wilt zien ondersteuning voor aanvullende gegevensbronnen: Geef uw feedback op het [forum van Azure zoeken feedback](https://feedback.azure.com/forums/263029-azure-search/).

In dit artikel wordt uitgelegd hoe het mechanisme van het gebruik van indexeerfuncties, maar we wordt ook inzoomen op functies en effecten die zijn alleen beschikbaar met SQL-databases (bijvoorbeeld geïntegreerde wijzigingen bijhouden).

## <a name="indexers-and-data-sources"></a>Indexeerfuncties en gegevensbronnen

Als u wilt instellen en configureren van een Azure SQL-indexering, kunt u de [Azure zoeken REST API](http://go.microsoft.com/fwlink/p/?LinkID=528173) voor het maken en beheren van **indexeerfuncties** en **gegevensbronnen**bellen. 

U kunt ook de [indexering class](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.indexer.aspx) in het [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx), of de wizard gegevens importeren in de [portal van Azure](https://portal.azure.com) gebruiken om te maken en plannen van een indexering.

Een **gegevensbron** bepaalt welke gegevens worden indexeert, referenties die nodig zijn voor toegang tot de gegevens en beleidsregels waarmee Azure zoeken efficiënt identificeren wijzigingen in de gegevens (nieuwe, gewijzigd of verwijderd rijen). Deze gedefinieerd als een onafhankelijke bron zodat deze kan worden gebruikt door meerdere indexeerfuncties.

Een **indexering** is een resource dat verbinding maakt met gegevensbronnen met doel zoekindexen. Een indexering wordt gebruikt in de volgende manieren:
 
- Een momentopname van de gegevens voor het vullen van een index uitvoeren.
- Een index bijwerken met wijzigingen in de gegevensbron volgens een schema.
- Bellen op een index bijwerken naar wens worden uitgevoerd. 

## <a name="when-to-use-azure-sql-indexer"></a>Wanneer u SQL Azure indexering gebruiken

Afhankelijk van de volgende factoren met betrekking tot uw gegevens, het gebruik van Azure SQL-indexering mogelijk of niet. Als uw gegevens past de volgende vereisten, kunt u Azure SQL-indexering gebruiken: 

- Alle gegevens afkomstig zijn uit één tabel of weergave
    - Als de gegevens is verspreid over meerdere tabellen, kunt u een weergave maken en gebruiken die weergave met de indexering. Let echter dat als u een weergave gebruikt, niet mogelijk gebruik van de detectie van SQL Server geïntegreerd wijzigen. Zie hieronder voor meer informatie. 
- De gegevenstypen die worden gebruikt in de gegevensbron worden ondersteund door de indexering. De meeste, maar niet alle van de SQL gegevenstypen worden ondersteund. Zie de [toewijzing van gegevenstypen in Azure zoeken](http://go.microsoft.com/fwlink/p/?LinkID=528105)voor meer informatie. 
- U niet nodig hebt in de buurt van realtime updates voor de index wanneer een rij wordt gewijzigd. 
    - De indexering kan uw tabel opnieuw indexeren maximaal elke 5 minuten. Als uw gegevens regelmatig worden gewijzigd en de wijzigingen worden doorgevoerd in de index binnen enkele seconden of één minuten moeten, wordt u aangeraden [Azure zoeken Index API](https://msdn.microsoft.com/library/azure/dn798930.aspx) rechtstreeks gebruiken. 
- Als u een grote gegevensset plannen en uitvoeren de indexering volgens een schema, uw schema kunt ons efficiënt identificeren gewijzigd (en hebt verwijderd, indien van toepassing) rijen. Zie 'vastleggen gewijzigd en verwijderde rijen"hieronder voor meer informatie. 
- De grootte van geïndexeerde velden in een rij niet groter is dan de maximale grootte van een Azure-zoekopdracht indexeren verzoek, dat wil 16 MB zeggen. 

## <a name="create-and-use-an-azure-sql-indexer"></a>Maken en een Azure SQL-indexering gebruiken

Maak eerst de gegevensbron: 

    POST https://myservice.search.windows.net/datasources?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of the table or view that you want to index" }
    }


U kunt de verbindingsreeks ophalen uit het [Klassieke Azure-Portal](https://portal.azure.com); Gebruik de `ADO.NET connection string` optie.

Maak vervolgens de zoekindex van target Azure als u nog niet hebt. Vanaf de [portal UI](https://portal.azure.com) of met behulp van de [Index-API maken](https://msdn.microsoft.com/library/azure/dn798941.aspx), kunt u dit doen.  Zorg ervoor dat het schema van de doel-index compatibel met het schema van de brontabel is: Zie [toewijzing tussen SQL Azure zoeken gegevenstypen](#TypeMapping) voor meer informatie.

Ten slotte de indexering maken door een naam geven en te verwijzen naar de gegevens bronsite en doelsites index:

    POST https://myservice.search.windows.net/indexers?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }

Op deze manier hebt gemaakt voor een indexering heeft geen een planning. Deze automatisch wordt uitgevoerd eenmaal zodra deze gemaakt. U kunt deze opnieuw op elk gewenst moment een aanvraag voor het **uitvoeren van indexering** uitvoeren:

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2015-02-28 
    api-key: admin-key

U kunt verschillende aspecten van indexering gedrag, zoals de batchgrootte en hoeveel documenten worden overgeslagen voordat een uitvoering indexering mislukt aanpassen. Zie [Indexering API maken](https://msdn.microsoft.com/library/azure/dn946899.aspx)voor meer informatie.
 
Mogelijk moet u toestaan dat Azure services verbinding maken met uw database. Zie [Verbinding maken van Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) voor instructies voor hoe u dat doen.

Als u de status van de indexering en kan worden uitgevoerd u geschiedenis (aantal geïndexeerde items, fouten, enzovoort), een aanvraag **indexering status** : 

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2015-02-28 
    api-key: admin-key

Het antwoord ziet er ongeveer als volgt uit: 

    {
        "@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null 
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null 
            },
            ... earlier history items 
        ]
    }

Uitvoering geschiedenis bevat maximaal 50 van de meest recent voltooide executions, die zijn gesorteerd in omgekeerde chronologische volgorde (zodat de meest recente uitvoering zich het eerste in de reactie voordoet). Meer informatie over het antwoord vindt u in de [Status van indexering ophalen](http://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Indexeerfuncties worden uitgevoerd op een planning

U kunt ook de indexering periodiek wordt uitgevoerd volgens een schema rangschikken. Klik hiertoe toe te voegen de eigenschap **planning** bij het maken of bijwerken van de indexering. Het volgende voorbeeld ziet u een aanvraag opslag voor het bijwerken van de indexering:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key 

    { 
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

De parameter **interval** is vereist. Het interval verwijst naar de tijd tussen de begindatum van twee opeenvolgende indexering executions. Het kleinste toegestane interval is 5 minuten; de langste is één dag. Deze moet worden opgemaakt als een XSD "dayTimeDuration" waarde (een beperkte subset van de waarde van een [ISO-8601 duur](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Het patroon hiervoor is: `P(nD)(T(nH)(nM))`. Voorbeelden: `PT15M` voor elke 15 minuten, `PT2H` voor elke 2 uur.

Het optionele **Starttijd** wordt aangegeven wanneer de geplande executions worden moeten begonnen; Als dit wordt weggelaten, wordt de huidige UTC-tijd worden gebruikt. Dit moment kan zijn in het verleden – waarin hoofdletters/kleine letters de uitvoering van het eerste gepland als wanneer de indexering is actief continu vanaf de starttijd.  

Slechts één uitvoering van een bepaald indexering kan tegelijkertijd worden uitgevoerd. Als een indexering is al worden uitgevoerd wanneer u het volgende document moet beginnen, de uitvoering is overgeslagen en cv's met het volgende interval, ervan uitgaande dat er geen andere indexering taak wordt uitgevoerd.

Een voorbeeld Maak deze concrete laten we eens. Stel dat we de volgende per uur planning geconfigureerd: 

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Hier ziet u wat er gebeurt: 

1. De uitvoering van het eerste indexering begint op of rond 1 maart 2015 12:00 a.m. UTC.
1. Wordt ervan uitgegaan dat deze kan worden uitgevoerd duurt 20 minuten (of elk gewenst moment minder dan 1 uur).
1. De tweede uitvoering begint op of rond 1 maart 2015 1:00 a.m. 
1. Stel nu dat deze kan worden uitgevoerd meer dan een uur duurt (hiervoor een groot aantal documenten dat is wel hiervoor, maar het is een handige illustratie) – bijvoorbeeld 70 minuten – zodat deze oplossing 2:10 uur is voltooid
1. Het is nu 2:00 uur, de tijd voor de derde uitvoering om te beginnen. Echter, omdat de tweede uitvoering van 1 uur nog steeds wordt uitgevoerd, is de derde uitvoering is overgeslagen. Hiermee start u de uitvoering van het derde om 3 uur

U kunt toevoegen, wijzigen of verwijderen van een schema voor een bestaande indexering met behulp van een verzoek om te **ZETTEN van indexering** . 

## <a name="capturing-new-changed-and-deleted-rows"></a>Nieuwe, vastleggen gewijzigd en verwijderde rijen

Als u een schema gebruikt en uw tabel een niet-alledaagse aantal rijen bevat, moet u een beleid van de detectie in de gegevens wijzigen, zodat de indexering verbindingsbestand alleen de nieuwe of gewijzigde rijen zonder dat u moet het opnieuw indexeren van de hele tabel is ingesteld.

### <a name="sql-integrated-change-tracking-policy"></a>Beleid voor het bijhouden van wijzigingen zijn geïntegreerd met SQL

Als uw SQL-database is geconfigureerd voor [het bijhouden van wijzigingen](https://msdn.microsoft.com/library/bb933875.aspx), wordt u aangeraden **SQL geïntegreerde wijzigen bijhouden beleid**gebruiken. Met dit beleid kan de efficiëntste het bijhouden van wijzigingen, en ook kunt Azure zoeken verwijderde rijen zonder dat u een kolom expliciete "vloeiende verwijderen' toevoegen aan uw tabel hoeft te identificeren.

Geïntegreerde wijzigingen bijhouden wordt ondersteund beginnen met de volgende versies van SQL Server-database:
 
- SQL Server 2008 R2 en later, als u SQL Server op Azure VMs gebruikt. 
- Azure SQL Database V12, als u gebruikmaakt van Azure SQL-Database.

Wanneer gebruik SQL zijn geïntegreerd met bijhouden van wijzigingen beleid, geef geen een beleid voor gegevens apart verwijdering detectie - dit beleid heeft ingebouwde ondersteuning voor identificeren verwijderd rijen.

Dit beleid kan alleen worden gebruikt met tabellen. Dit kan niet worden gebruikt met weergaven. U moet voor de tabel die u gebruikt voordat u dit beleid kunt het bijhouden van wijzigingen inschakelen. Zie [Wijzigingen bijhouden uitschakelen en inschakelen](https://msdn.microsoft.com/library/bb964713.aspx) voor instructies. 

Als u wilt gebruiken dit beleid, maken of bijwerken van uw gegevensbron als volgt:
 
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
      }
    }

### <a name="high-water-mark-change-detection-policy"></a>Beleid voor detectie van hoog Water markeren wijzigen

Terwijl het beleid SQL geïntegreerde wijzigingen bijhouden wordt aanbevolen, niet mogelijk gebruiken als uw gegevens zich in een weergave of als u een oudere versie van Azure SQL-database gebruikt. In dat geval kunt u overwegen het beleid hoog water markeren. Dit beleid kan worden gebruikt als de tabel bevat een kolom die voldoet aan de volgende criteria voldoet:

- Alle wordt ingevoegd, Geef een waarde voor de kolom. 
- Alle updates voor een item wordt ook de waarde van de kolom wijzigen. 
- Hiermee verhoogt u de waarde van de kolom met elke wijziging.
- Query's die gebruikmaken van een `WHERE` component die vergelijkbaar is met `WHERE [High Water Mark Column] > [Current High Water Mark Value]` efficiënt kunnen worden uitgevoerd.

Een kolom voor geïndexeerde **rowversion** is bijvoorbeeld een ideaal candidate voor de kolom hoog water markeren. Als u wilt gebruiken dit beleid, maken of bijwerken van uw gegevensbron als volgt: 

    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
      }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Beleid voor vloeiende detectie van kolom verwijdering verwijderen

Wanneer de rijen zijn verwijderd uit de brontabel, wilt u waarschijnlijk die rijen uit de zoekindex ook verwijderen. Als u het beleid voor bijhouden van SQL geïntegreerd gebruikt, is dit afgehandeld voor u. Echter kunt het beleid voor bijhouden van hoog water markeren niet u met verwijderde rijen. Wat moet u doen? 

Als de rijen zijn fysiek verwijderd uit de tabel, u hebt geen succes: Er is geen manier om te leiden van de aanwezigheid van de records die niet langer bestaat.  U kunt echter de 'vloeiende verwijderen'-methode rijen als logisch verwijderde markeren zodat ze niet uit de tabel door een kolom toe te voegen en rijen te markeren als verwijderd met een markering-waarde in die kolom.

Wanneer u met de methode vloeiende verwijderen, kunt u de vloeiende beleid als volgt verwijderen bij het maken of bijwerken van de gegevensbron: 

    { 
        …, 
        "dataDeletionDetectionPolicy" : { 
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]", 
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]" 
        }
    }

Houd er rekening mee dat het **softDeleteMarkerValue** een tekenreeks moet – de tekenreeks van de werkelijke waarde gebruiken. Bijvoorbeeld, hebt u een gehele kolom waar verwijderde rijen zijn gemarkeerd met de waarde 1, gebruikt `"1"`; Als u een bits kolom waar verwijderde rijen zijn gemarkeerd met de Booleaanse waarde true hebt, gebruikt u `"True"`. 

<a name="TypeMapping"></a>
## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Koppeling tussen SQL-gegevenstypen en gegevenstypen Azure zoeken

|SQL-gegevenstype | Target index veldtypen toegestaan |Notities 
|------|-----|----|
|bits|Edm.Boolean, Edm.String| |
|int, DateTime, tinyint |Edm.Int32, Edm.Int64, Edm.String| |
| bigint | Edm.Int64, Edm.String | |
| reële, zweven |Edm.Double, Edm.String | |
| smallmoney, geld decimale numerieke | Edm.String| Azure zoeken biedt geen ondersteuning voor het converteren van een decimaal typen in Edm.Double, omdat dit precisie van formules kwijtraken |
| teken, nchar, varchar, nvarchar | Edm.String<br/>Collection(EDM.String)|Omzetten van een kolom van de tekenreeks in Collection(Edm.String) is vereist voor het gebruik van een voorbeeld API versie 2015-02-28-Preview. Zie [dit artikel](search-api-indexers-2015-02-28-preview.md#CreateIndexer) voor meer informatie| 
|smalldatetime, datetime, datetime2, datum, datetimeoffset |Edm.DateTimeOffset, Edm.String| |
|uniqueidentifer | Edm.String | |
|Geografie | Edm.GeographyPoint | Alleen Geografie exemplaren van het type punt met SRID 4326 (dit is de standaardinstelling) worden ondersteund | | 
|ROWVERSION| N/B |Rij-versie kolommen kunnen niet worden opgeslagen in de zoekindex, maar ze kunnen worden gebruikt voor het bijhouden van wijzigingen | |
| tijd, binair getal, varbinary, afbeelding, xml, geometrie, Stream CLR-gegevenstypen | N/B |Niet ondersteund |


## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Q:** Kan ik SQL Azure indexering gebruiken met SQL-databases op IaaS VMs in Azure uitgevoerd?

A: Ja. Echter, moet u toestaan dat uw zoekservice verbinding maken met uw database door de volgende twee dingen doen. Zie [een verbinding uit een indexering Azure zoeken met SQL Server op een VM Azure configureren](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md) voor meer informatie.

1. Mogelijk moet u de database met een vertrouwd certificaat zodanig configureren dat de zoekservice SSL-verbindingen met de database kan openen.
2. De firewall voor toegang tot het IP-adres van uw search-service configureren.

**Q:** Kan ik SQL Azure indexering gebruiken met SQL-databases on-premises implementatie uitgevoerd? 

A: we wordt niet aanbevolen noch ondersteunt, zoals hierdoor, moet u uw databases naar internetverkeer openen. 

**Q:** Kan ik SQL Azure indexering met databases dan SQL Server wordt uitgevoerd in IaaS op Azure gebruiken? 

A: wordt kunt dit scenario wordt niet ondersteund, omdat de indexering met alle databases dan SQL Server is niet getest.  

**Q:** Kan ik meerdere indexeerfuncties uitgevoerd op een planning maken? 

A: Ja. Echter kan slechts één indexering worden uitgevoerd op een van de knooppunten in één keer. Als u meerdere indexeerfuncties tegelijkertijd worden uitgevoerd, kunt u de schaal van de zoekservice aan meer dan één zoeken eenheid. 

**Q:** Invloed van een indexering uitgevoerd op de werklast van mijn query? 

A: Ja. Indexering wordt uitgevoerd op een van de knooppunten in uw zoekservice en van het knooppunt resources worden gedeeld door indexering en op queryverkeer en andere API-aanvragen. Als u intensief indexering en query werkbelasting uitvoert en een grote hoeveelheid 503 fouten of toeneemt antwoord tijden optreden, kunt u de schaal van de zoekservice.
