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
    ms.date="10/27/2016" 
    ms.author="eugenesh"/>

#<a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Azure SQL-Database verbinden met Azure zoekprogramma's via indexeerfuncties

Azure Search-service is een gehoste cloud-zoekservice waarmee u gemakkelijk een geweldige zoekervaring te bieden. Voordat u zoeken kunt, moet u een index Azure zoeken met uw gegevens te vullen. Als de gegevens worden opgeslagen in een Azure SQL-database, kunt u de indexing proces met de nieuwe **Azure zoeken indexering voor Azure SQL-Database** (of **Azure SQL-indexering** voor korte) automatiseren. Dit betekent dat er minder code te schrijven en minder infrastructuur naar belangrijk vindt.

In dit artikel worden de breukmechanica van het gebruik van indexeerfuncties, maar deze ook de worden functies beschreven die zijn alleen beschikbaar met Azure SQL-databases (bijvoorbeeld geïntegreerde wijzigingen bijhouden). Azure zoeken ondersteunt ook andere gegevensbronnen, zoals DocumentDB Azure-blobopslag en tabelopslag. Als u wilt zien ondersteuning voor aanvullende gegevensbronnen: uw feedback geven over het [zoeken van Azure feedback-forum](https://feedback.azure.com/forums/263029-azure-search/).

## <a name="indexers-and-data-sources"></a>Indexeerfuncties en gegevensbronnen

U kunt instellen en configureren van een SQL Azure indexering gebruiken: 

- Wizard importeren in de [portal van Azure](https://portal.azure.com)
- Azure zoeken [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)
- Azure zoeken [REST API](http://go.microsoft.com/fwlink/p/?LinkID=528173)

In dit artikel gebruiken we de REST API om aan te geven hoe u **indexeerfuncties** en **gegevensbronnen**maken en beheren. 

Een **gegevensbron** bepaalt welke gegevens worden indexeert, referenties die nodig zijn voor toegang tot de gegevens en beleidsregels waarmee efficiënt identificeren wijzigingen in de gegevens (nieuwe, gewijzigde of verwijderde rijen). Deze gedefinieerd als een onafhankelijke bron zodat deze kan worden gebruikt door meerdere indexeerfuncties.

Een **indexering** is een resource dat verbinding maakt met gegevensbronnen met doel zoekindexen. Een indexering wordt gebruikt in de volgende manieren:
 
- Een momentopname van de gegevens voor het vullen van een index uitvoeren.
- Een index bijwerken met wijzigingen in de gegevensbron volgens een schema.
- Bellen op een index bijwerken naar wens worden uitgevoerd. 

## <a name="when-to-use-azure-sql-indexer"></a>Wanneer u SQL Azure indexering gebruiken

Afhankelijk van de volgende factoren met betrekking tot uw gegevens, het gebruik van Azure SQL-indexering mogelijk of niet. Als uw gegevens past de volgende vereisten, kunt u Azure SQL-indexering gebruiken: 

- Alle gegevens afkomstig zijn uit één tabel of weergave
    - Als de gegevens is verspreid over meerdere tabellen, kunt u een weergave maken en gebruiken die weergave met de indexering. Echter als u een weergave gebruikt, u kan niet worden gebruik van de detectie van SQL Server geïntegreerd wijzigen. Zie voor meer informatie [in deze sectie](#CaptureChangedRows). 
- De gegevenstypen die worden gebruikt in de gegevensbron worden ondersteund door de indexering. De meeste, maar niet alle de SQL-typen worden ondersteund. Zie de [toewijzing van gegevenstypen in Azure zoeken](http://go.microsoft.com/fwlink/p/?LinkID=528105)voor meer informatie. 
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

Maak vervolgens de zoekindex van target Azure als u nog niet hebt. U kunt een index via de [portal UI](https://portal.azure.com) of de [Index-API maken](https://msdn.microsoft.com/library/azure/dn798941.aspx)maken. Zorg ervoor dat het schema van de doel-index compatibel met het schema van de brontabel is: Zie [toewijzen tussen SQL Azure zoeken gegevenstypen](#TypeMapping).

Ten slotte de indexering maken door een naam geven en te verwijzen naar de gegevens bronsite en doelsites index:

    POST https://myservice.search.windows.net/indexers?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }

Op deze manier hebt gemaakt voor een indexering heeft geen een planning. Automatisch wordt uitgevoerd nadat wanneer deze gemaakt. U kunt deze opnieuw op elk gewenst moment een aanvraag voor het **uitvoeren van indexering** uitvoeren:

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

U kunt ook de indexering periodiek wordt uitgevoerd volgens een schema rangschikken. Hiervoor moet u de **planning** -eigenschap bij het maken of bijwerken van de indexering toevoegen. Het volgende voorbeeld ziet u een aanvraag opslag voor het bijwerken van de indexering:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key 

    { 
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

De parameter **interval** is vereist. Het interval verwijst naar de tijd tussen de begindatum van twee opeenvolgende indexering executions. Het kleinste toegestane interval is 5 minuten; de langste is één dag. Deze moet worden opgemaakt als een XSD "dayTimeDuration" waarde (een beperkte subset van de waarde van een [ISO-8601 duur](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Het patroon hiervoor is: `P(nD)(T(nH)(nM))`. Voorbeelden: `PT15M` voor elke 15 minuten, `PT2H` voor elke 2 uur.

Het optionele **Starttijd** wordt aangegeven wanneer de geplande executions moeten beginnen. Als dit wordt weggelaten, wordt de huidige UTC-tijd wordt gebruikt. Dit moment kan zijn in het verleden – waarin hoofdletters/kleine letters de uitvoering van het eerste is gepland als wanneer de indexering is actief continu vanaf de starttijd.  

Slechts één uitvoering van een indexering kan tegelijkertijd worden uitgevoerd. Als een indexering wordt uitgevoerd wanneer de uitvoering is gepland, is de uitvoering van de volgende geplande tijdstip uitgesteld.

Een voorbeeld Maak deze concrete laten we eens. Stel dat we de volgende per uur planning geconfigureerd: 

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Hier ziet u wat er gebeurt: 

1. De uitvoering van het eerste indexering begint op of rond 1 maart 2015 12:00 a.m. UTC.
1. Wordt ervan uitgegaan dat deze kan worden uitgevoerd duurt 20 minuten (of elk gewenst moment minder dan 1 uur).
1. De tweede uitvoering begint op of rond 1 maart 2015 1:00 a.m. 
1. Stel nu dat deze kan worden uitgevoerd meer dan een uur – bijvoorbeeld 70 minuten – duurt zodat deze oplossing 2:10 uur is voltooid
1. Het is nu 2:00 uur, de tijd voor de derde uitvoering om te beginnen. Echter, omdat de tweede uitvoering van 1 uur nog steeds wordt uitgevoerd, is de derde uitvoering is overgeslagen. Hiermee start u de uitvoering van het derde om 3 uur

U kunt toevoegen, wijzigen of verwijderen van een schema voor een bestaande indexering met behulp van een verzoek om te **ZETTEN van indexering** . 

<a name="CaptureChangedRows">, /a >
## <a name="capturing-new-changed-and-deleted-rows"></a>Vastleggen nieuwe, gewijzigd en verwijderde rijen

Als de tabel bevat een aantal rijen, moet u een beleid voor detectie wijzigen. Detectie wijzigen kunt een efficiënte voor het ophalen van alleen de nieuwe of gewijzigde rijen zonder dat u moet het opnieuw indexeren van de hele tabel.

### <a name="sql-integrated-change-tracking-policy"></a>Beleid voor het bijhouden van wijzigingen zijn geïntegreerd met SQL

Als uw SQL-database is geconfigureerd voor [het bijhouden van wijzigingen](https://msdn.microsoft.com/library/bb933875.aspx), wordt u aangeraden **SQL geïntegreerde wijzigen bijhouden beleid**gebruiken. Dit is de efficiëntste beleid. Bovendien kunnen Azure zoeken verwijderde rijen zonder dat u een kolom expliciete "vloeiende verwijderen' toevoegen aan uw tabel hoeft te identificeren.

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

<a name="HighWaterMarkPolicy"></a>
### <a name="high-water-mark-change-detection-policy"></a>Beleid voor detectie van hoog Water markeren wijzigen

Terwijl het beleid SQL geïntegreerde wijzigingen bijhouden wordt aanbevolen, kan dit alleen worden gebruikt met tabellen, niet weergaven. Als u een weergave gebruikt, kunt u overwegen het beleid hoog water markeren. Dit beleid kan worden gebruikt als uw tabel of weergave bevat een kolom die voldoet aan de volgende criteria voldoet:

- Alle wordt ingevoegd, Geef een waarde voor de kolom. 
- Alle updates voor een item wordt ook de waarde van de kolom wijzigen. 
- Hiermee verhoogt u de waarde van de kolom met elke wijziging.
- Query's met het volgende waar en ORDER BY componenten efficiënt kunnen worden uitgevoerd: `WHERE [High Water Mark Column] > [Current High Water Mark Value] ORDER BY [High Water Mark Column]`.

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

> [AZURE.WARNING] Als de brontabel geen indexen op de kolom hoog water is ingeschakeld heeft, kunnen een query's die worden gebruikt door de SQL-indexering time-out. Met name de `ORDER BY [High Water Mark Column]` component vereist een index wilt efficiënt uitvoeren als de tabel bevat een aantal rijen. 

Als u time-out fouten optreden, kunt u de `queryTimeout` indexering configuratie-instelling de querytime-out instellen op een waarde hoger dan de standaard 5 minuten-out. Bijvoorbeeld als u wilt de time-out instelt op 10 minuten, maken of de indexering bijwerken met de volgende configuratie: 

    {
      ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

U kunt ook uitschakelen de `ORDER BY [High Water Mark Column]` component. Dit is echter niet aanbevolen omdat als de uitvoering indexering wordt onderbroken door een fout, de indexering heeft opnieuw verwerkingstijd van alle rijen als u deze later - wordt uitgevoerd zelfs als de indexering heeft al vrijwel alle rijen worden verwerkt door de tijd die het is onderbroken. Uitschakelen de `ORDER BY` component, gebruik de `disableOrderByHighWaterMarkColumn` instellen in de definitie indexering:  

    {
     ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "disableOrderByHighWaterMarkColumn" : true } }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Beleid voor vloeiende detectie van kolom verwijdering verwijderen

Wanneer de rijen zijn verwijderd uit de brontabel, wilt u waarschijnlijk die rijen uit de zoekindex ook verwijderen. Als u het beleid voor bijhouden van SQL geïntegreerd gebruikt, is dit afgehandeld voor u. Echter kunt het beleid voor bijhouden van hoog water markeren niet u met verwijderde rijen. Wat moet u doen? 

Als de rijen worden fysiek verwijderd uit de tabel, heeft Azure zoeken geen manier om te leiden van de aanwezigheid van de records die niet langer bestaat.  U kunt echter de 'vloeiende verwijderen'-methode logisch rijen wilt verwijderen zonder deze te verwijderen uit de tabel. Voeg een kolom aan rijen in uw tabel of weergave en markeren als verwijderd met die kolom.

Wanneer u met de methode vloeiende verwijderen, kunt u de vloeiende beleid als volgt verwijderen bij het maken of bijwerken van de gegevensbron: 

    { 
        …, 
        "dataDeletionDetectionPolicy" : { 
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]", 
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]" 
        }
    }

De **softDeleteMarkerValue** moet een tekenreeks – de tekenreeks van de werkelijke waarde gebruiken. Bijvoorbeeld, hebt u een gehele kolom waar verwijderde rijen zijn gemarkeerd met de waarde 1, gebruikt `"1"`. Als u een bits kolom waar verwijderde rijen zijn gemarkeerd met de Booleaanse waarde true hebt, gebruikt u `"True"`. 

<a name="TypeMapping"></a>
## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Koppeling tussen SQL-gegevenstypen en gegevenstypen Azure zoeken

|SQL-gegevenstype | Target index veldtypen toegestaan |Notities 
|------|-----|----|
|bits|Edm.Boolean, Edm.String| |
|int, DateTime, tinyint |Edm.Int32, Edm.Int64, Edm.String| |
| bigint | Edm.Int64, Edm.String | |
| reële, zweven |Edm.Double, Edm.String | |
| smallmoney, geld decimale numerieke | Edm.String| Azure zoeken biedt geen ondersteuning voor het converteren van een decimaal typen in Edm.Double, omdat dit precisie van formules kwijtraken |
| teken, nchar, varchar, nvarchar | Edm.String<br/>Collection(EDM.String)|Een SQL-tekenreeks kan worden gebruikt om te vullen van een veld Collection(Edm.String) als de tekenreeks een matrix JSON van tekenreeksen vertegenwoordigt:`["red", "white", "blue"]` | 
|smalldatetime, datetime, datetime2, datum, datetimeoffset |Edm.DateTimeOffset, Edm.String| |
|uniqueidentifer | Edm.String | |
|Geografie | Edm.GeographyPoint | Alleen Geografie exemplaren van het type punt met SRID 4326 (dit is de standaardinstelling) worden ondersteund | | 
|ROWVERSION| N/B |Rij-versie kolommen kunnen niet worden opgeslagen in de zoekindex, maar ze kunnen worden gebruikt voor het bijhouden van wijzigingen | |
| tijd, binair getal, varbinary, afbeelding, xml, geometrie, Stream CLR-gegevenstypen | N/B |Niet ondersteund |

## <a name="configuration-settings"></a>Configuratie-instellingen

SQL-indexering beschrijft de verschillende configuratie-instellingen: 

|Instelling |Gegevenstype | Doel | Standaardwaarde |
|--------|----------|---------|---------------|
| queryTimeout | tekenreeks | Hiermee stelt u de time-out voor de uitvoering van de SQL-query | 5 minuten ("00: 05:00") |
| disableOrderByHighWaterMarkColumn | BOOL | Hiermee wordt de SQL-query wordt gebruikt door het beleid hoog water markeren voor de ORDER BY-component weglaat. Zie [beleid hoog Water markeren](#HighWaterMarkPolicy) | ONWAAR | 

Deze instellingen worden gebruikt de `parameters.configuration` object in de definitie van indexering. Zo wilt instellen de querytime-out op 10 minuten, maken of de indexering bijwerken met de volgende configuratie: 

    {
      ... other indexer definition properties
     "parameters" : { 
            "configuration" : { "queryTimeout" : "00:10:00" } }
    }

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Q:** Kan ik SQL Azure indexering gebruiken met SQL-databases op IaaS VMs in Azure uitgevoerd?

A: Ja. Echter, moet u toestaan dat uw zoekservice verbinding maken met uw database. Zie [een verbinding uit een indexering Azure zoeken met SQL Server op een VM Azure configureren](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md)voor meer informatie.


**Q:** Kan ik SQL Azure indexering gebruiken met SQL-databases on-premises implementatie uitgevoerd? 

A: we wordt niet aanbevolen noch ondersteunt, zoals hierdoor, moet u uw databases naar internetverkeer openen. 

**Q:** Kan ik SQL Azure indexering met databases dan SQL Server wordt uitgevoerd in IaaS op Azure gebruiken? 

A: wordt kunt dit scenario wordt niet ondersteund, omdat de indexering met alle databases dan SQL Server is niet getest.  

**Q:** Kan ik meerdere indexeerfuncties uitgevoerd op een planning maken? 

A: Ja. Echter kan slechts één indexering worden uitgevoerd op een van de knooppunten in één keer. Als u meerdere indexeerfuncties tegelijkertijd worden uitgevoerd, kunt u de schaal van de zoekservice aan meer dan één zoeken eenheid. 

**Q:** Invloed van een indexering uitgevoerd op de werklast van mijn query? 

A: Ja. Indexering wordt uitgevoerd op een van de knooppunten in uw zoekservice en van het knooppunt resources worden gedeeld door indexering en op queryverkeer en andere API-aanvragen. Als u intensief indexering en query werkbelasting uitvoert en een grote hoeveelheid 503 fouten of toeneemt antwoord tijden optreden, kunt u de schaal van de zoekservice.
