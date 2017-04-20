<properties 
pageTitle="Indexering bewerkingen (Azure zoeken Service REST API: 2015-02-28-Preview) | Voorbeeld van Azure Search API" 
description="Indexering bewerkingen (Azure zoeken Service REST API: 2015-02-28-Preview)" 
services="search" 
documentationCenter="" 
authors="chaosrealm" 
manager="pablocas"
editor="" />

<tags 
ms.service="search" 
ms.devlang="rest-api" 
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na" 
ms.date="09/07/2016" 
ms.author="eugenesh" />

#<a name="indexer-operations-azure-search-service-rest-api-2015-02-28-preview"></a>Indexering bewerkingen (Azure zoeken Service REST API: 2015-02-28-Preview)#

> [AZURE.NOTE] In dit artikel wordt beschreven indexeerfuncties in de [2015-02-28-Preview REST API](search-api-2015-02-28-preview.md). Deze API-versie wordt de preview-versies van Azure-blobopslag indexering met de extractie van het document en Azure Table Storage indexering, plus andere verbeteringen toegevoegd. De API ondersteunt ook algemeen beschikbaar (GA) indexeerfuncties, inclusief indexeerfuncties voor Azure SQL-Database, SQL Server op Azure VMs en Azure DocumentDB.

## <a name="overview"></a>Overzicht ##

Azure zoeken kunt integreren rechtstreeks met enkele veelvoorkomende gegevensbronnen, hoeft te maken van de code voor het indexeren van uw gegevens verwijderen. Als u dit precies wilt, kunt u de API voor het zoeken van Azure als u wilt maken en beheren van **indexeerfuncties** en **gegevensbronnen**bellen. 

Een **indexering** is een resource dat verbinding maakt met gegevensbronnen met doel zoekindexen. Een indexering wordt gebruikt in de volgende manieren: 

- Een momentopname van de gegevens voor het vullen van een index uitvoeren.
- Een index met wijzigingen in de gegevensbron volgens een schema synchroniseren. De planning maakt deel uit van de definitie van indexering.
- Aanroepen op aanvraag indexen bijwerken naar wens. 

Een **indexering** is handig als u wilt dat de normale updates indexen. U kunt een inline-planning instellen als onderdeel van de definitie van een indexering, of op aanvraag [Indexering uitvoeren](#RunIndexer)met worden uitgevoerd. 

Een **gegevensbron** geeft aan welke gegevens moeten worden geïndexeerd, referenties voor toegang tot de gegevens en beleid voor het inschakelen van Azure zoeken efficiënt identificeren wijzigingen in de gegevens (zoals gewijzigd of rijen in een databasetabel verwijderd). Deze gedefinieerd als een onafhankelijke bron zodat deze kan worden gebruikt door meerdere indexeerfuncties.

De volgende gegevensbronnen worden momenteel ondersteund:

- **Azure SQL-Database** en **SQL Server Azure VMs**. Zie voor een overzicht gerichte, [in dit artikel](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md). 
- **Azure DocumentDB**. Zie voor een overzicht gerichte, [in dit artikel](../documentdb/documentdb-search-indexer.md). 
- **Azure-blobopslag**, inclusief de opmaak van de volgende documenten: PDF-bestand, Microsoft Office (DOCX-document, XSLX/XLS, PPTX/PPT, MSG), HTML, XML, POSTCODES en tekst zonder opmaak bestanden (inclusief JSON). Zie voor een overzicht gerichte, [in dit artikel](search-howto-indexing-azure-blob-storage.md).
- **Azure-tabelopslag**. Zie voor een overzicht gerichte, [in dit artikel](search-howto-indexing-azure-tables.md).
     
We bezig bent met het toevoegen van ondersteuning voor aanvullende gegevensbronnen in de toekomst. Om ons te helpen prioriteit voorzien, zodat deze beslissingen, Geef uw feedback op het [forum van Azure zoeken feedback](http://feedback.azure.com/forums/263029-azure-search).

Zie [Service limieten](search-limits-quotas-capacity.md) voor maximumlimieten die betrekking hebben op indexering en gegevens bron resources.

## <a name="typical-usage-flow"></a>Veelvoorkomend gebruik stroom ##

U kunt maken en beheren van Indexeerfuncties en gegevensbronnen via eenvoudige HTTP aanvragen (POST, ophalen, wordt opgeslagen, DELETE) ten opzichte van een bepaald `data source` of `indexer` resource.

Het instellen van automatische indexeren is meestal een vier stappen:

1. Geef de gegevensbron met de gegevens die moeten worden geïndexeerd. Houd er rekening mee dat Azure zoeken mogelijk geen ondersteuning voor alle gegevenstypen aanwezig zijn in uw gegevensbron. Zie [ondersteunde gegevenstypen](https://msdn.microsoft.com/library/azure/dn798938.aspx) voor de lijst.

2. Maak een Azure zoekindex waarvan het schema compatibel met de gegevensbron is.
  
3. Maak een gegevensbron van Azure zoeken zoals is beschreven in de [Gegevensbron maken](#CreateDataSource).
  
4. Maak een indexering Azure zoeken als beschreven [Indexering maken](#CreateIndexer).

Het is raadzaam over het maken van één indexering voor elke combinatie van target index en gegevens bron. U kunt meerdere indexeerfuncties in dezelfde index schrijft hebben en u dezelfde gegevensbron voor meerdere indexeerfuncties opnieuw kunt gebruiken. Echter een indexering slechts één gegevensbron tegelijk nodig hebben en kan alleen schrijven naar een enkele index. 

Na het maken van een indexering, kunt u de status ervan kan worden uitgevoerd met de bewerking [Indexering Status ophalen](#GetIndexerStatus) ophalen. U kunt ook een indexering uitvoeren op elk gewenst moment (in plaats van of naast het uitvoeren van deze regelmatig volgens een schema) met de bewerking [Uitvoeren voor indexering](#RunIndexer) .

<!-- MSDN has 2 art files plus a API topic link list -->


## <a name="create-data-source"></a>Gegevensbron maken ##

Een gegevensbron wordt in Azure zoeken gebruikt met indexeerfuncties, de verbindingsgegevens voor ad hoc of geplande gegevensvernieuwing van een doel-index leveren. U kunt een nieuwe gegevensbron binnen een Azure-zoekservice met een HTTP POST-aanvraag maken.
    
    POST https://[service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

U kunt ook opslag gebruiken en geef de naam van de gegevensbron op de URI. Als de gegevensbron niet bestaat, kunt u het wordt gemaakt.

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]

**Opmerking**: het maximum aantal gegevensbronnen toegestaan is afhankelijk van de prijzen van laag. De gratis service kunnen maximaal 3 gegevensbronnen. Standaard service kunt 50 gegevensbronnen. Zie [Service limieten](search-limits-quotas-capacity.md) voor meer informatie.

**Aanvragen**

HTTPS is vereist voor alle serviceaanvragen. Het verzoek **Gegevensbron maken** kan worden samengesteld met behulp van een bericht of de opslag methode. Wanneer u een bericht gebruikt, kunt u de naam van een gegevensbron in het hoofdgedeelte van de aanvraag samen met de definitie van de gegevensbron moet opgeven. Met opgeslagen is de naam onderdeel van de URL. Als de gegevensbron niet bestaat, wordt deze is gemaakt. Als dit al bestaat, wordt deze bijgewerkt naar de nieuwe definitie. 

Naam van de gegevensbron moet worden kleine letters, beginnen met een letter of een getal, hebben geen streepjes of stippellijnen en minder dan 128 tekens bevatten. De rest van de naam kunt na de naam van de gegevensbron begint met een letter of een getal, opnemen een letter, getal en streepjes, zolang de streepjes geen opeenvolgende zijn. Zie [Naming regels](https://msdn.microsoft.com/library/azure/dn857353.aspx) voor meer informatie.

De `api-version` is vereist. De huidige versie niet wordt `2015-02-28`.

**Kopteksten aanvragen**

De volgende lijst bevat de vereiste en optionele verzoek koppen. 

- `Content-Type`: Vereist. Deze worden ingesteld`application/json`
- `api-key`: Vereist. De `api-key` wordt gebruikt voor verificatie van de aanvraag voor de zoekservice. Dit is een tekenreekswaarde, uniek zijn voor uw service. Het verzoek **Gegevensbron maken** moet bevatten een `api-key` koptekst ingesteld op uw sleutel beheerder (in plaats van een sleutel query). 
 
U moet ook de naam van de service om samen te stellen van de aanvraag-URL. U kunt zowel de servicenaam verkrijgen en `api-key` vanuit uw servicedashboard in de [Portal van Azure](https://portal.azure.com/). Zie [maken een Search-service in de portal](search-create-service-portal.md) voor paginanavigatie help.

<a name="CreateDataSourceRequestSyntax"></a>
**De syntaxis van de hoofdtekst aanvragen**

De hoofdtekst van de aanvraag bevat een definitie van de gegevensbron, waaronder de gegevensbron, type referenties om de gegevens, evenals een optioneel-detectie wijzigen en gegevens verwijdering detectie beleidsregels die worden gebruikt voor het efficiënt identificeren gewijzigd of verwijderd van gegevens in de gegevensbron vooraf worden gebruikt in combinatie met een regelmatig geplande indexering te lezen. 

De syntaxis voor het structureren van de aanvraag nettolading is als volgt. Een voorbeeld van een aanvraag is opgegeven op verder in dit onderwerp.

    { 
        "name" : "Required for POST, optional for PUT. The name of the data source",
        "description" : "Optional. Anything you want, or nothing at all",
        "type" : "Required. Must be one of 'azuresql', 'documentdb', 'azureblob', or 'azuretable'",
        "credentials" : { "connectionString" : "Required. Connection string for your data source" },
        "container" : { "name" : "Required. The name of the table, collection, or blob container you wish to index" },
        "dataChangeDetectionPolicy" : { Optional. See below for details }, 
        "dataDeletionDetectionPolicy" : { Optional. See below for details }
    }

Aanvraag bevat de volgende eigenschappen: 

- `name`: Vereist. De naam van de gegevensbron. Naam van een gegevensbron moet alleen kleine letters, cijfers of streepjes bevatten, kunnen niet beginnen of eindigen met koppeltekens en is beperkt tot 128 tekens.
- `description`: Een optionele beschrijving. 
- `type`: Vereist. Moet een van de ondersteunde typen gegevensbronnen:
    - `azuresql`-Azure SQL-Database of SQL Server Azure VMs
    - `documentdb`-Azure DocumentDB
    - `azureblob`-Azure-blobopslag
    - `azuretable`-Azure-tabelopslag
- `credentials`:
    - De vereiste `connectionString` eigenschap geeft de verbindingsreeks voor de gegevensbron. De opmaak van de verbindingsreeks, is afhankelijk van het gegevensbrontype: 
        - Voor SQL Azure is dit de gebruikelijke SQL Server-verbindingsreeks. Als u de portal van Azure gebruikt om op te halen de verbindingsreeks, gebruikt u de `ADO.NET connection string` optie.
        - DocumentDB, moet de verbindingsreeks in de volgende indeling: `"AccountEndpoint=https://[your account name].documents.azure.com;AccountKey=[your account key];Database=[your database id]"`. Alle waarden zijn vereist. U vindt deze in de [portal van Azure](https://portal.azure.com/).  
        - Voor Azure Blob en Table Storage is dit de verbindingsreeks van de opslag-account. Beschrijving van de notatie [hier](https://azure.microsoft.com/documentation/articles/storage-configure-connection-string/). HTTPS-protocol voor eindpunt is vereist.  
- `container`, vereist: de gegevens die u wilt indexeren met de `name` en `query` eigenschappen: 
    - `name`Vereist:
        - Azure SQL: Hiermee geeft u de tabel of weergave. U kunt schema gekwalificeerde namen, zoals `[dbo].[mytable]`.
        - DocumentDB: Hiermee geeft u de verzameling. 
        - Azure-blobopslag: Hiermee geeft u de container opslag.
        - Azure Table Storage: Hiermee geeft u de naam van de tabel. 
    - `query`, optioneel:
        - DocumentDB: kunt u een query die de indeling van een willekeurige JSON-document in een platte schema die Azure zoeken kunt indexeren effent opgeven.  
        - Azure-blobopslag: kunt u een virtuele map in de container blob opgeven. Bijvoorbeeld: voor blob pad `mycontainer/documents/blob.pdf`, `documents` kunnen worden gebruikt als de virtuele map.
        - Azure Table Storage: kunt u opgeven van een query die de set te worden geïmporteerd rijen worden gefilterd.
        - Azure SQL: query wordt niet ondersteund. Als u deze functionaliteit nodig hebt, neemt u stemmen op [deze suggestie](https://feedback.azure.com/forums/263029-azure-search/suggestions/9893490-support-user-provided-query-in-sql-indexer)
- Het optionele `dataChangeDetectionPolicy` en `dataDeletionDetectionPolicy` eigenschappen worden hieronder beschreven.

<a name="DataChangeDetectionPolicies"></a>
**Beleidsregels voor detectie van gegevens wijzigen**

Het doel van een beleid voor detectie wijzigen is efficiënt gewijzigde gegevens om items te identificeren. Ondersteunde beleidsregels, hangen af van het gegevensbrontype. De volgende secties wordt beschreven elk beleid. 

***Bovengrens wijzigen detectie beleid*** 

Gebruik dit beleid als de gegevensbron bevat een kolom of de eigenschap die voldoet aan de volgende criteria voldoet:
 
- Alle wordt ingevoegd, Geef een waarde voor de kolom. 
- Alle updates voor een item wordt ook de waarde van de kolom wijzigen. 
- Hiermee verhoogt u de waarde van de kolom met elke wijziging.
- Query's met een van de volgende strekking filtercomponent `WHERE [High Water Mark Column] > [Current High Water Mark Value]` efficiënt kunnen worden uitgevoerd.

Bij gebruik van Azure SQL-gegevensbronnen, een geïndexeerde bijvoorbeeld `rowversion` kolom is de ideale candidate voor gebruik met met het beleid hoog water markeren. 

Dit beleid kunt als volgt opgeven:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
    } 

> [AZURE.NOTE] Wanneer u een DocumentDB gegevensbronnen gebruikt, moet u de `_ts` eigenschap verstrekt door DocumentDB. 

> [AZURE.NOTE] Bij gebruik van Azure Blob gegevensbronnen, Azure zoeken automatisch gebruik een bovengrens detectie van beleid wijzigen op basis van een blob van laatste wijziging tijdstempel; u hoeft niet te zo'n beleid zelf opgeeft.   

***SQL geïntegreerd wijzigen detectie beleid***

Als uw SQL-database is geconfigureerd voor [het bijhouden van wijzigingen](https://msdn.microsoft.com/library/bb933875.aspx), wordt u aangeraden SQL geïntegreerde wijzigen bijhouden beleid gebruiken. Dit beleid mogelijk maakt de efficiëntste wijzigingen bijhouden en Azure zoeken om aan te geven verwijderde rijen zonder dat u dat er een kolom expliciete "vloeiende verwijderen' in uw schema kunt.

Geïntegreerde wijzigingen bijhouden wordt ondersteund beginnen met de volgende versies van SQL Server-database: 
- SQL Server 2008 R2, als u SQL Server op Azure VMs gebruikt.
- Azure SQL Database V12, als u gebruikmaakt van Azure SQL-Database.  

Wanneer gebruik van de SQL-geïntegreerde wijzigingen bijhouden beleid, een beleid voor gegevens apart verwijdering detectie geen opgeeft - dit beleid heeft ingebouwde ondersteuning voor identificeren verwijderd rijen. 

Dit beleid kan alleen worden gebruikt met tabellen. Dit kan niet worden gebruikt met weergaven. U moet voor de tabel die u gebruikt voordat u dit beleid kunt het bijhouden van wijzigingen inschakelen. Zie [Wijzigingen bijhouden uitschakelen en inschakelen](https://msdn.microsoft.com/library/bb964713.aspx) voor instructies.    
 
Wanneer het verzoek van de **Gegevensbron maken** , SQL geïntegreerd wijzigen bijhouden beleid structureren kan worden opgegeven als volgt:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
    }

<a name="DataDeletionDetectionPolicies"></a>
**Beleid voor de detectie van de gegevens verwijderen**

Het doel van een beleid verwijdering detectie is efficiënt verwijderde gegevens om items te identificeren. De enige ondersteunde beleid is momenteel de `Soft Delete` beleid, waarmee identificeren van verwijderde items op basis van de waarde van een `soft delete` kolom of eigenschap in de gegevensbron. Dit beleid kunt als volgt opgeven:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the column that specifies whether a row was deleted", 
        "softDeleteMarkerValue" : "the value that identifies a row as deleted" 
    }

**NOTITIE:** Alleen kolommen met tekenreeks, geheel getal of Booleaanse waarden worden ondersteund. De waarde die wordt gebruikt als `softDeleteMarkerValue` moet een tekenreeks, zelfs als de bijbehorende kolom gehele getallen of Booleaanse waarden bevat. Bijvoorbeeld, gebruikt u als de waarde die wordt weergegeven in de gegevensbron 1 is, `"1"` als de `softDeleteMarkerValue`.    

<a name="CreateDataSourceRequestExamples"></a>
**Voorbeelden van de hoofdtekst aanvragen**

Als u van plan bent voor het gebruik van de gegevensbron met een indexering die wordt uitgevoerd op een planning, ziet in dit voorbeeld u hoe wijzigen en verwijderen detectie beleidsregels opgeven: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy", "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }

Als u alleen gebruiken van de gegevensbron voor eenmalige kopie van de gegevens wilt, kunnen het beleid worden weggelaten:

    { 
        "name" : "asqldatasource",
        "description" : "anything you want, or nothing at all",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" }
    } 

**Antwoord**

Voor een succesvolle aanvraag: 201 gemaakt. 

<a name="UpdateDataSource"></a>
## <a name="update-data-source"></a>Gegevensbron bijwerken ##

U kunt een bestaande gegevensbron met een verzoek HTTP plaatsen bijwerken. U opgeven de naam van de gegevensbron wilt bijwerken op de aanvraag URI:

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

De `api-version` is vereist. De huidige versie niet wordt `2015-02-28`. [Azure zoeken versiebeheer](https://msdn.microsoft.com/library/azure/dn864560.aspx) heeft details en meer informatie over alternatieve versies.

De `api-key` moet een beheerder-sleutel (in plaats van een sleutel query). Raadpleeg de sectie verificatie in [Zoeken Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over toetsen. [Maken een Search-service in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de URL van de service en belangrijke eigenschappen in het verzoek gebruikt.

**Aanvragen**

De syntaxis van de hoofdtekst van de aanvraag is hetzelfde als voor het [aanvragen van de gegevensbron maken](#CreateDataSourceRequestSyntax).

> [AZURE.NOTE] Bepaalde eigenschappen kunnen niet worden bijgewerkt op een bestaande gegevensbron. Bijvoorbeeld, kunt u het type van een bestaande gegevensbron niet wijzigen.  

> [AZURE.NOTE] Als u niet dat de verbindingsreeks voor een bestaande gegevensbron wijzigen wilt, kunt u de letterlijke `<unchanged>` voor de verbindingsreeks. Dit is handig in situaties waarin u moet een bijwerkquery gegevensbron, maar niet gemakkelijk toegang hebt tot de verbindingsreeks omdat het beveiliging van gevoelige gegevens.

**Antwoord**

Voor een succesvolle aanvraag: 201 gemaakt als een nieuwe gegevensbron is gemaakt en 204 geen inhoud als een bestaande gegevensbron werd bijgewerkt.

<a name="ListDataSource"></a>
## <a name="list-data-sources"></a>Lijst met gegevensbronnen ##

De **Lijst met gegevensbronnen** bewerking geeft als resultaat een lijst van de gegevensbronnen in uw Azure Search-service. 

    GET https://[service name].search.windows.net/datasources?api-version=[api-version]
    api-key: [admin key]

De `api-version` is vereist. De huidige versie niet wordt `2015-02-28`. 

De `api-key` moet een beheerder-sleutel (in plaats van een sleutel query). Raadpleeg de sectie verificatie in [Zoeken Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over toetsen. [Maken een Search-service in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de URL van de service en belangrijke eigenschappen in het verzoek gebruikt.

**Antwoord**

Voor een succesvolle aanvraag: 200 OK.

Hier volgt een voorbeeld antwoord hoofdtekst:

    {
      "value" : [
        {
          "name": "datasource1",
          "type": "azuresql",
          ... other data source properties
        }]
    }

Houd er rekening mee dat u het antwoord naar beneden af op alleen de eigenschappen waarin u geïnteresseerd bent kunt filteren. Bijvoorbeeld als u wilt dat alleen een lijst met namen van gegevensbronnen, gebruikt u de OData `$select` optie query:

    GET /datasources?api-version=205-02-28&$select=name

In dit geval er de reactie van het bovenstaande voorbeeld als volgt: 

    {
      "value" : [ { "name": "datasource1" }, ... ]
    }

Dit is een handige techniek om op te slaan bandbreedte als er een groot aantal gegevensbronnen in uw Search-service.

<a name="GetDataSource"></a>
## <a name="get-data-source"></a>Gegevensbron ophalen ##

De bewerking **Gegevensbron ophalen** krijgt de definitie van de gegevensbron van Azure zoekresultaten.

    GET https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

De `api-version` is vereist. De huidige versie niet wordt `2015-02-28`. 

De `api-key` moet een beheerder-sleutel (in plaats van een sleutel query). Raadpleeg de sectie verificatie in [Zoeken Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over toetsen. [Maken een Search-service in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de URL van de service en belangrijke eigenschappen in het verzoek gebruikt.

**Antwoord**

Statuscode: 200 OK geretourneerd voor een succesvolle reactie.

Het antwoord is vergelijkbaar met de voorbeelden in de [gegevensbron maken voorbeeld aanvragen](#CreateDataSourceRequestExamples): 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted", 
            "softDeleteMarkerValue" : "true" }
    }

> [AZURE.NOTE] Stel niet de `Accept` verzoek kop naar `application/json;odata.metadata=none` wanneer deze API als u dit doet bellen veroorzaakt `@odata.type` kenmerk moeten worden weggelaten uit het antwoord en u niet mogelijk onderscheid maken tussen gegevens wijzigen en gegevens verwijdering detectie beleid van verschillende typen. 

<a name="DeleteDataSource"></a>
## <a name="delete-data-source"></a>Gegevensbron verwijderen ##

Een gegevensbron verwijdert met de bewerking **Gegevensbron verwijderen** uit uw Azure Search-service.

    DELETE https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Als een indexeerfuncties verwijst naar de gegevensbron die u wilt verwijderen, wordt de verwijderbewerking nog steeds Ga verder. Echter deze indexeerfuncties gewijzigd in een foutstatus na de volgende keer wordt uitgevoerd.  

De `api-version` is vereist. De huidige versie niet wordt `2015-02-28`. 

De `api-key` moet een beheerder-sleutel (in plaats van een sleutel query). Raadpleeg de sectie verificatie in [Zoeken Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over toetsen. [Maken een Search-service in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de URL van de service en belangrijke eigenschappen in het verzoek gebruikt.

**Antwoord**

Statuscode: 204 geen inhoud geretourneerd voor een succesvolle reactie.

<a name="CreateIndexer"></a>
## <a name="create-indexer"></a>Indexering maken ##

U kunt een nieuwe indexering binnen een Azure-zoekservice met een HTTP POST-aanvraag maken.
    
    POST https://[service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

U kunt ook opslag gebruiken en geef de naam van de gegevensbron op de URI. Als de gegevensbron niet bestaat, kunt u het wordt gemaakt.

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]

> [AZURE.NOTE] Het maximale aantal toegestane indexeerfuncties varieert door de prijzen van laag. De gratis service kunnen maximaal 3 indexeerfuncties. Standaard service kunt 50 indexeerfuncties. Zie [Service limieten](search-limits-quotas-capacity.md) voor meer informatie.

De `api-version` is vereist. De huidige versie niet wordt `2015-02-28`. 

De `api-key` moet een beheerder-sleutel (in plaats van een sleutel query). Raadpleeg de sectie verificatie in [Zoeken Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over toetsen. [Maken een Search-service in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de URL van de service en belangrijke eigenschappen in het verzoek gebruikt.


<a name="CreateIndexerRequestSyntax"></a>
**De syntaxis van de hoofdtekst aanvragen**

De hoofdtekst van de aanvraag bevat de definitie van een indexering, waarmee de gegevensbron en de doel-index voor indexeren, evenals optioneel indexing planning en parameters. 


De syntaxis voor het structureren van de aanvraag nettolading is als volgt. Een voorbeeld van een aanvraag is opgegeven op verder in dit onderwerp.

    { 
        "name" : "Required for POST, optional for PUT. The name of the indexer",
        "description" : "Optional. Anything you want, or null",
        "dataSourceName" : "Required. The name of an existing data source",
        "targetIndexName" : "Required. The name of an existing index",
        "schedule" : { Optional. See Indexing Schedule below. },
        "parameters" : { Optional. See Indexing Parameters below. },
        "fieldMappings" : { Optional. See Field Mappings below. },
        "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.  
    }

**Indexering planning**

Een indexering kunt u desgewenst een schema opgeven. Als een planning aanwezig is, wordt de indexering regelmatig aan de hand van planning uitvoeren. Planning heeft de volgende kenmerken:

- `interval`: Vereist. Een duurwaarde waarmee een interval of punt voor indexering wordt uitgevoerd. Het kleinste toegestane interval is 5 minuten; de langste is één dag. Deze moet worden opgemaakt als een XSD "dayTimeDuration" waarde (een beperkte subset van de waarde van een [ISO-8601 duur](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) ). Het patroon hiervoor is: `"P[nD][T[nH][nM]]"`. Voorbeelden: `PT15M` voor elke 15 minuten, `PT2H` voor elke 2 uur. 

- `startTime`: Vereist. Een UTC-datetime wanneer de indexering moet beginnen met. 

**Indexering Parameters**

Een indexering kunt eventueel verschillende parameters die betrekking hebben op het gedrag opgeven. Alle parameters zijn optioneel.  

- `maxFailedItems`: Het aantal items die moeten worden geïndexeerd voordat het uitvoeren van een indexering wordt beschouwd als een fout kan mislukken. Standaard is aan 0. Informatie over mislukte items wordt geretourneerd door de bewerking [Indexering Status ophalen](#GetIndexerStatus) . 

- `maxFailedItemsPerBatch`: Het aantal items die moeten worden geïndexeerd in elke batch voordat het uitvoeren van een indexering wordt beschouwd als een fout kan mislukken. Standaard is aan 0.

- `base64EncodeKeys`: Geeft aan of document toetsen base-64-codering. Azure zoeken legt beperkingen voor de tekens die kunnen voorkomen in een documentsleutel. De waarden in de brongegevens kunnen echter ongeldige tekens bevatten. Zo nodig indexeren van dergelijke waarden als het document toetsen, deze vlag kan worden ingesteld op waar. Standaard is `false`.

- `batchSize`: Hiermee geeft u het aantal items die zijn lezen uit de gegevensbron en geïndexeerde als één batch zodat de prestaties verbeteren. De standaardinstelling is afhankelijk van het gegevensbrontype: dit is 1000 voor Azure SQL- en DocumentDB en 10 voor Azure-blobopslag.

**Veldtoewijzingen**

Veldtoewijzingen kunt u de naam van een veld in de gegevensbron toewijzen aan een andere veldnaam in de doel-index. Stel dat u een brontabel met een veld `_id`. Azure zoeken niet is toegestaan een veldnaam begint met een onderstrepingsteken, zodat de naam van het veld moet worden gewijzigd. U kunt dit doen met de `fieldMappings` eigenschap van de indexering als volgt: 
    
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 

U kunt meerdere veldtoewijzingen opgeven: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "_id", "targetFieldName" : "id" },
        { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" },
     ]

Zowel bronsite en doelsites veldnamen zijn niet hoofdlettergevoelig.

<a name="FieldMappingFunctions"></a>
***Veld toewijzing functies***

Veldtoewijzingen kunnen ook worden gebruikt naar het veld uit de bronwaarden *toewijzing*functies te transformeren.

Slechts één van deze functie is momenteel ondersteund: `jsonArrayToStringCollection`. Een veld met een tekenreeks die is opgemaakt als een matrix JSON in een veld Collection(Edm.String) in de doel-index worden geparseerd. Dit is bedoeld voor gebruik met Azure SQL-indexering met name omdat SQL geen een gegevenstype systeemeigen siteverzameling. Dit kan als volgt worden gebruikt: 

    "fieldMappings" : [ { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } } ] 

Als het bronveld bevat de tekenreeks bijvoorbeeld `["red", "white", "blue"]`, klikt u vervolgens het veld als doel van het type `Collection(Edm.String)` wordt gevuld met de drie waarden `"red"`, `"white"` en `"blue"`.

Houd er rekening mee dat het `targetFieldName` eigenschap is optioneel. Als u bent vergeten, de `sourceFieldName` waarde wordt gebruikt. 

<a name="CreateIndexerRequestExamples"></a>
**Voorbeelden van de hoofdtekst aanvragen**

Het volgende voorbeeld wordt een indexering die gegevens opgehaald uit de tabel waarnaar wordt verwezen door de `ordersds` gegevensbron aan de `orders` index volgens een schema dat begint op 1 januari 2015 UTC en per uur is uitgevoerd. Elke aanroep indexering wordt voltooid als u niet meer dan 5 items moeten worden geïndexeerd in elke batch mislukt en niet meer dan 10 items moeten worden geïndexeerd in totaal mislukt. 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }

**Antwoord**

201 gemaakt voor een succesvolle aanvraag.


<a name="UpdateIndexer"></a>
## <a name="update-indexer"></a>Indexering bijwerken ##

U kunt een bestaande indexering met een verzoek HTTP plaatsen bijwerken. U opgeven de naam van de indexering bijwerken op de aanvraag URI:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

De `api-version` is vereist. De huidige versie niet wordt `2015-02-28`. 

De `api-key` moet een beheerder-sleutel (in plaats van een sleutel query). Raadpleeg de sectie verificatie in [Zoeken Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over toetsen. [Maken een Search-service in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de URL van de service en belangrijke eigenschappen in het verzoek gebruikt.

**Aanvragen**

De syntaxis van de hoofdtekst van de aanvraag is hetzelfde als voor het [maken van indexering aanvragen](#CreateIndexerRequestSyntax).

**Antwoord**

Voor een succesvolle aanvraag: 201 gemaakt als een nieuwe indexering is gemaakt en 204 geen inhoud als een bestaande indexering werd bijgewerkt.


<a name="ListIndexers"></a>
## <a name="list-indexers"></a>Lijst indexeerfuncties ##

De bewerking **Lijst indexeerfuncties** retourneert de lijst met indexeerfuncties in uw Azure Search-service. 

    GET https://[service name].search.windows.net/indexers?api-version=[api-version]
    api-key: [admin key]


De `api-version` is vereist. De preview-versie is `2015-02-28-Preview`. [Azure zoeken versiebeheer](https://msdn.microsoft.com/library/azure/dn864560.aspx) heeft details en meer informatie over alternatieve versies.

De `api-key` moet een beheerder-sleutel (in plaats van een sleutel query). Raadpleeg de sectie verificatie in [Zoeken Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over toetsen. [Maken een Search-service in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de URL van de service en belangrijke eigenschappen in het verzoek gebruikt.

**Antwoord**

Voor een succesvolle aanvraag: 200 OK.

Hier volgt een voorbeeld antwoord hoofdtekst:

    {
      "value" : [
      {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        ... other indexer properties
      }]
    }

Houd er rekening mee dat u het antwoord naar beneden af op alleen de eigenschappen waarin u geïnteresseerd bent kunt filteren. Desgewenst kunt u alleen een lijst met namen van indexering Gebruik bijvoorbeeld de OData `$select` optie query:

    GET /indexers?api-version=2014-10-20-Preview&$select=name

In dit geval er de reactie van het bovenstaande voorbeeld als volgt: 

    {
      "value" : [ { "name": "myindexer" } ]
    }

Dit is een handige techniek om op te slaan bandbreedte als er een groot aantal indexeerfuncties in uw Search-service.


<a name="GetIndexer"></a>
## <a name="get-indexer"></a>Indexering ophalen ##

De bewerking **Ophalen indexering** krijgt de definitie indexering van Azure zoekresultaten.

    GET https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

De `api-version` is vereist. De preview-versie is `2015-02-28-Preview`. 

De `api-key` moet een beheerder-sleutel (in plaats van een sleutel query). Raadpleeg de sectie verificatie in [Zoeken Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over toetsen. [Maken een Search-service in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de URL van de service en belangrijke eigenschappen in het verzoek gebruikt.

**Antwoord**

Statuscode: 200 OK geretourneerd voor een succesvolle reactie.

Het antwoord is vergelijkbaar met de voorbeelden in [indexering maken voorbeeld aanvragen](#CreateIndexerRequestExamples): 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }


<a name="DeleteIndexer"></a>
## <a name="delete-indexer"></a>Indexering verwijderen ##

De bewerking **Indexering verwijderen** verwijdert een indexering uit uw Azure Search-service.

    DELETE https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Wanneer een indexering wordt verwijderd, wordt de indexering executions Bezig op dat moment tot voltooiing wordt uitgevoerd, maar geen verdere executions gepland. Een niet-bestaande indexering gebruiken wordt resulteren in HTTP-statuscode 404 niet gevonden. 
 
De `api-version` is vereist. De preview-versie is `2015-02-28-Preview`. 

De `api-key` moet een beheerder-sleutel (in plaats van een sleutel query). Raadpleeg de sectie verificatie in [Zoeken Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over toetsen. [Maken een Search-service in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de URL van de service en belangrijke eigenschappen in het verzoek gebruikt.

**Antwoord**

Statuscode: 204 geen inhoud geretourneerd voor een succesvolle reactie.

<a name="RunIndexer"></a>
## <a name="run-indexer"></a>Indexering uitvoeren ##

Naast het regelmatig op een schema uitgevoerd, kan een indexering ook worden geactiveerd op aanvraag via de bewerking **Uitvoeren voor indexering** : 

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [admin key]

De `api-version` is vereist. De preview-versie is `2015-02-28-Preview`. 

De `api-key` moet een beheerder-sleutel (in plaats van een sleutel query). Raadpleeg de sectie verificatie in [Zoeken Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over toetsen. [Maken een Search-service in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de URL van de service en belangrijke eigenschappen in het verzoek gebruikt.

**Antwoord**

Statuscode: 202 geaccepteerde geretourneerd voor een succesvolle reactie.

<a name="GetIndexerStatus"></a>
## <a name="get-indexer-status"></a>Indexering Status ophalen ##

De bewerking **Indexering Status ophalen** haalt de huidige status en de uitvoering van de geschiedenis van een indexering: 

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [admin key]


De `api-version` is vereist. De preview-versie is `2015-02-28-Preview`. 

De `api-key` moet een beheerder-sleutel (in plaats van een sleutel query). Raadpleeg de sectie verificatie in [Zoeken Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over toetsen. [Maken een Search-service in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de URL van de service en belangrijke eigenschappen in het verzoek gebruikt.

**Antwoord**

Statuscode: 200 OK voor een succesvolle reactie.

De hoofdtekst van het antwoord bevat informatie over de algehele indexering systeemstatus status, de laatste indexering aanroep, evenals de geschiedenis van recente indexering aanroepen (indien aanwezig). 

De hoofdtekst van een steekproef antwoord ziet er zo uit: 

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

**Indexering Status**

Indexering status zijn van de volgende waarden:

- `running`Geeft aan dat de indexering normaal wordt uitgevoerd. Houd er rekening mee dat enkele van de executions indexering nog steeds is beschadigd, zodat het is een goed idee om te controleren het `lastResult` ook eigenschap. 

- `error`Geeft aan dat de indexering er is een fout zonder menselijke tussenkomst te corrigeren. Bijvoorbeeld de referenties voor gegevensbronnen is mogelijk verlopen, of het schema van de gegevensbron of van de doel-index is gewijzigd in een recente manier. 

**Indexering Execution resultaat**

Een resultaat van de uitvoering van indexering bevat informatie over de uitvoering van een enkel indexering. De meest recente resultaat is opgehaald als de `lastResult` eigenschap van de status van indexering. Andere recente resultaten, als deze aanwezig zijn, worden geretourneerd als de `executionHistory` eigenschap van de status van indexering. 

Indexering execution resultaat heeft de volgende eigenschappen: 

- `status`: de status van de uitvoering. Zie [Indexering Execution Status](#IndexerExecutionStatus) hieronder voor meer informatie. 

- `errorMessage`: foutbericht wordt weergegeven voor een mislukte worden uitgevoerd. 

- `startTime`: de tijd in UTC bij het opstarten van deze kan worden uitgevoerd.

- `endTime`: de tijd in UTC wanneer deze worden uitgevoerd beëindigd. Deze waarde is niet ingesteld als de uitvoering nog steeds bezig is.

- `errors`: een matrix van itemniveau fouten, indien van toepassing. Elk item bevat een documentsleutel (`key` eigenschap) en een foutbericht wordt weergegeven (`errorMessage` eigenschap). 

- `itemsProcessed`: het aantal items (bijvoorbeeld tabelrijen) die de indexering heeft geprobeerd indexeren tijdens de uitvoering van deze van de gegevensbron. 

- `itemsFailed`: het aantal items dat is mislukt tijdens deze kan worden uitgevoerd.  
 
- `initialTrackingState`: altijd `null` de eerste indexering wordt uitgevoerd, of als de gegevens bijhouden beleid wijzigen is niet ingeschakeld voor de gegevensbron die is gebruikt. Als dit beleid is ingeschakeld, in de volgende uitvoering deze waarde geeft de eerste (laagste) het bijhouden van wijzigingen waarde verwerkt door deze kan worden uitgevoerd. 

- `finalTrackingState`: altijd `null` als de gegevens bijhouden beleid wijzigen is niet ingeschakeld voor de gegevensbron die is gebruikt. Anders geeft aan dat de meest recente (hoogste) het bijhouden van wijzigingen waarde verwerkt door deze kan worden uitgevoerd. 

<a name="IndexerExecutionStatus"></a>
**Indexering Execution Status**

Indexering execution status wordt de status van de uitvoering van een enkel indexering vastgelegd. Er kunnen de volgende waarden:

- `success`Geeft aan dat de indexering uitvoering is voltooid.

- `inProgress`Geeft aan dat de indexering uitvoering uitgevoerd wordt. 

- `transientFailure`Geeft aan dat de indexering uitvoering is mislukt. Zie `errorMessage` eigenschap voor meer informatie. De fout mogelijk of mogelijk niet menselijke tussenkomst om op te lossen: bijvoorbeeld een schema-incompatibiliteit tussen de gegevensbron en de doel-index op te lossen vereist gebruikersactie, maar niet door een tijdelijke gegevensbron downtime. Indexering aanroepen blijft per schema, indien aanwezig. 

- `persistentFailure`Geeft aan dat de indexering op een manier die moeten worden menselijke tussenkomst is mislukt. Geplande indexering executions stoppen. Gebruik na het adressering van het probleem, API voor indexering opnieuw instellen om de geplande executions opnieuw te starten. 

- `reset`Geeft aan dat de indexering opnieuw is ingesteld door een aanroep opnieuw indexering API (Zie hieronder). 

<a name="ResetIndexer"></a>
## <a name="reset-indexer"></a>Indexering opnieuw instellen ##

De bewerking **Indexering opnieuw** Hiermee stelt u het bijhouden van wijzigingen staat dat is gekoppeld aan de indexering opnieuw. Hiermee kunt u naar het activeren van geheel nieuwe opnieuw indexeren (bijvoorbeeld als het schema van de gegevensbron is gewijzigd), of om de gegevens wijzigen detectie beleid voor een gegevensbron die is gekoppeld aan de indexering te wijzigen.   

    POST https://[service name].search.windows.net/indexers/[indexer name]/reset?api-version=[api-version]
    api-key: [admin key]

De `api-version` is vereist. De preview-versie is `2015-02-28-Preview`. 

De `api-key` moet een beheerder-sleutel (in plaats van een sleutel query). Raadpleeg de sectie verificatie in [Zoeken Service REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) voor meer informatie over toetsen. [Maken een Search-service in de portal](search-create-service-portal.md) wordt uitgelegd hoe u de URL van de service en belangrijke eigenschappen in het verzoek gebruikt.

**Antwoord**

Statuscode: 204 geen inhoud voor een succesvolle reactie.

## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Toewijzing tussen SQL-gegevenstypen en gegevenstypen Azure zoeken ##

<table style="font-size:12">
<tr>
<td>SQL-gegevenstype</td>  
<td>Target index veldtypen toegestaan</td>
<td>Notities</td>
</tr>
<tr>
<td>bits</td>
<td>Edm.Boolean, Edm.String</td>
<td></td>
</tr>
<tr>
<td>int, DateTime, tinyint</td>
<td>Edm.Int32, Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>bigint</td>
<td>Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>reële, zweven</td>
<td>Edm.Double, Edm.String</td>
<td></td>
</tr>
<tr>
<td>smallmoney, geld<br>decimaal<br>numerieke
</td>
<td>Edm.String</td>
<td>Azure zoeken biedt geen ondersteuning voor het converteren van een decimaal typen in Edm.Double, omdat dit precisie van formules kwijtraken
</td>
</tr>
<tr>
<td>teken, nchar, varchar, nvarchar</td>
<td>Edm.String<br/>Collection(EDM.String)</td>
<td>Zie [Veld toewijzing functies](#FieldMappingFunctions) in dit document voor meer informatie over hoe u een kolom tekenreeks omzetten in een Collection(Edm.String)</td>
</tr>
<tr>
<td>smalldatetime, datetime, datetime2, datum, datetimeoffset</td>
<td>Edm.DateTimeOffset, Edm.String</td>
<td></td>
</tr>
<tr>
<td>uniqueidentifer</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>Geografie</td>
<td>Edm.GeographyPoint</td>
<td>Alleen Geografie exemplaren van het type punt met SRID 4326 (dit is de standaardinstelling) worden ondersteund</td>
</tr>
<tr>
<td>ROWVERSION</td>
<td>N/B</td>
<td>Rij-versie kolommen kunnen niet worden opgeslagen in de zoekindex, maar ze kunnen worden gebruikt voor het bijhouden van wijzigingen</td>
</tr>
<tr>
<td>tijd, tijdspanne<br>de binaire, varbinary, afbeelding,<br>XML, geometrie, CLR-gegevenstypen</td>
<td>N/B</td>
<td>Niet ondersteund</td>
</tr>
</table>

## <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>Toewijzing tussen JSON-gegevenstypen en gegevenstypen Azure zoeken ##

<table style="font-size:12">
<tr>
<td>JSON-gegevenstype</td> 
<td>Target index veldtypen toegestaan</td>
<td>Notities</td>
</tr>
<tr>
<td>BOOL</td>
<td>Edm.Boolean, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Integraal getallen</td>
<td>Edm.Int32, Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Getallen met drijvende komma</td>
<td>Edm.Double, Edm.String</td>
<td></td>
</tr>
<tr>
<td>tekenreeks</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>matrices van primitieve typen, bijvoorbeeld ["a", "b", "c"]</td>
<td>Collection(EDM.String)</td>
<td></td>
</tr>
<tr>
<td>Tekenreeksen die zijn opgemaakt als datums</td>
<td>Edm.DateTimeOffset, Edm.String</td>
<td></td>
</tr>
<tr>
<td>GeoJSON point-objecten</td>
<td>Edm.GeographyPoint</td>
<td>GeoJSON punten zijn JSON-objecten in de volgende indeling: {"type": "Punt", "coördinaten": [lang, lat]} </td>
</tr>
<tr>
<td>Andere JSON-objecten</td>
<td>N/B</td>
<td>Niet ondersteund. Azure zoeken worden momenteel ondersteund alleen primitieve typen en tekenreeks verzamelingen</td>
</tr>
</table>