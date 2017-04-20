<properties
   pageTitle="Een upgrade naar de Azure zoeken .NET SDK versie 1.1 | Microsoft Azure | De zoekservice gehoste cloud"
   description="Een upgrade naar de Azure zoeken .NET SDK versie 1.1"
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/15/2016"
   ms.author="brjohnst"/>

# <a name="upgrading-to-the-azure-search-net-sdk-version-20-preview"></a>Een upgrade naar het Azure zoeken .NET SDK versie 2.0-voorbeeld

Als u versie 1.1 of oudere van de [Azure zoeken .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)gebruikt, kunt in dit artikel u uw toepassing voor het gebruik van de volgende primaire versie 2.0-preview upgraden.

Zie voor meer algemene Stapsgewijze instructies voor de SDK, inclusief voorbeelden, [het gebruik van Azure zoeken vanuit een .NET-toepassing](search-howto-dotnet-sdk.md).

Versie 2.0-voorbeeld van de Azure zoeken .NET SDK bevat enkele wijzigingen uit eerdere versies. Dit zijn meestal secundaire, zodat alleen minimale inspanning wijzigen van uw code moet vereisen. Zie de [stappen voor het upgraden](#UpgradeSteps) voor instructies over het wijzigen van uw code aan de nieuwe versie van de SDK.

> [AZURE.NOTE] Als u versie 0,13-voorbeeld of oudere gebruikt, moet u een upgrade uitvoeren naar versie 1.1 Voornaam en klikt u vervolgens een upgrade uitvoeren naar 2.0-preview. Zie [bijlage: stappen voor het upgraden naar versie 1.1](#UpgradeStepsV1) voor instructies.

<a name="WhatsNew"></a>
## <a name="whats-new-in-version-20-preview"></a>Wat is er nieuw in versie 2.0-voorbeeld

Versie 2.0-voorbeeld is de eerste versie van de Azure zoeken .NET SDK een preview-versie van de Azure zoeken REST API, specifiek 2015-02-28-preview afstemmen. Hierdoor kunnen veel functies van de Preview-versie van Azure zoeken vanuit een .NET-toepassing, waaronder de volgende gebruiken:

- [Aangepaste analyzers](https://aka.ms/customanalyzers)
- Ondersteuning voor de indexering [Azure-blobopslag](search-howto-indexing-azure-blob-storage.md) en [Azure-tabelopslag](search-howto-indexing-azure-tables.md)
- Indexering aanpassing via [veldtoewijzingen](search-indexer-field-mappings.md)
- ETags ondersteuning voor veilige gelijktijdige bijwerken van index definities, Indexeerfuncties en gegevensbronnen inschakelen
- Ondersteuning voor .NET Core en .NET draagbare profiel 111

<a name="UpgradeSteps"></a>
## <a name="steps-to-upgrade"></a>Stappen voor het upgraden

Eerst bijwerken NuGet referentie voor `Microsoft.Azure.Search` de NuGet Package Manager-console of door met de rechtermuisknop op uw projectverwijzingen en "Beheren NuGet-pakketten..." selecteren in Visual Studio. Controleer of u kunt u een voorlopige versie-pakketten inschakelen via 'Voorlopige versie opnemen' in de NuGet "Beheren-pakketten" venster in Visual Studio of met behulp van de `-IncludePrerelease` schakelen als u gebruikmaakt van NuGet Package Manager-Console.

Zodra NuGet is gedownload, de nieuwe pakketten en bijbehorende afhankelijkheden, bouw die tabellen opnieuw uw project. Afhankelijk van hoe uw code is gestructureerd, deze mogelijk opnieuw opbouwen met succes. Zo ja, bent u klaar!

Als uw opbouwen mislukt, ziet u een fout opbouwen als volgt uit:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

De volgende stap is deze fout opbouwen op te lossen. Zie [wijzigingen in versie 2.0-preview verbreken](#ListOfChanges) voor meer informatie over wat de fout veroorzaakt en hoe u kunt doen.

U ziet mogelijk extra opbouwwaarschuwingen verouderde methoden en eigenschappen. De waarschuwingen bevat instructies over wat u wilt gebruiken in plaats van de functie vervallen. Als u gebruikmaakt van uw toepassing bijvoorbeeld de `SearchRequestOptions.RequestId` eigenschap, moet u er een waarschuwing weergegeven met de mededeling dat`"This property is deprecated. Please use ClientRequestId instead."`

Nadat u eventuele opbouwfouten hebt gecorrigeerd, kunt u wijzigingen aanbrengen in uw toepassing om te profiteren van nieuwe functionaliteit als u wilt. Nieuwe functies in de SDK zijn gedetailleerde in [Wat is er nieuw in versie 2.0-preview](#WhatsNew).

<a name="ListOfChanges"></a>
## <a name="breaking-changes-in-version-20-preview"></a>Wijzigingen afbreken in versie 2.0-voorbeeld

Er is slechts één recente wijziging in versie 2.0-preview die worden gewijzigd van de code moeten mogelijk naast uw toepassing opnieuw te maken.

### <a name="indexesgetclient-return-type"></a>Het retourtype Indexes.GetClient

De `Indexes.GetClient` methode heeft een nieuwe retourtype. Eerder, deze geretourneerd `SearchIndexClient`, maar dit is gewijzigd in `ISearchIndexClient` in versie 2.0-preview. Dit is ter ondersteuning van klanten die willen model de `GetClient` methode voor eenheidstests door te retourneren van een model implementatie van `ISearchIndexClient`.

#### <a name="example"></a>Voorbeeld

Als uw code ziet er zo uit:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

U kunt dit wijzigen als volgt een opbouwfouten op te lossen:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

## <a name="conclusion"></a>Sluiten
Als u meer informatie over het gebruik van de Azure zoeken .NET SDK nodig hebt, raadpleegt u onze onlangs bijgewerkte [Stapsgewijze procedures](search-howto-dotnet-sdk.md).

We Welkom uw feedback over de SDK. Als u problemen ondervindt, altijd ons op het [Azure zoeken MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuresearch)om hulp vragen. Als u een fout hebt gevonden, kunt u een probleem in de [bibliotheek van Azure .NET SDK GitHub](https://github.com/Azure/azure-sdk-for-net/issues)bestand. Zorg ervoor dat de titel van uw probleem met het voorvoegsel "zoeken SDK:".

Bedankt voor het gebruik van Azure zoeken.

<a name="UpgradeStepsV1"></a>
## <a name="appendix-steps-to-upgrade-to-version-11"></a>Bijlage: Stappen voor het upgraden naar versie 1.1 

> [AZURE.NOTE] In deze sectie geldt alleen voor gebruikers van de versie van Azure zoeken .NET SDK 0,13 preview en eerder.

Eerst bijwerken NuGet referentie voor `Microsoft.Azure.Search` de NuGet Package Manager-console of door met de rechtermuisknop op uw projectverwijzingen en "Beheren NuGet-pakketten..." selecteren in Visual Studio.

Zodra NuGet is gedownload, de nieuwe pakketten en bijbehorende afhankelijkheden, bouw die tabellen opnieuw uw project.

Als u eerder versie 1.0.0-preview, 1.0.1-preview of 1.0.2-preview gebruikte, moet volgen op het bouwen en u bent klaar!

Als u eerder versie 0.13.0-preview gebruikte of eerder, ziet u bouwen fouten als volgt te werk:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

De volgende stap is het opbouwfouten één voor één op te lossen. Meestal moeten worden sommige klasse en de methode namen die in de SDK hebt gekregen wijzigen. [Lijst met wijzigingen in versie 1.1 verbreken](#ListOfChangesV1) bevat een lijst met deze naamwijzigingen.

Als u aangepaste klassen gebruikt voor het modelleren van uw documenten en die klassen eigenschappen van niet-nullable primitieve typen hebben (bijvoorbeeld `int` of `bool` in C#), is er een oplossing fout in de versie 1.1 van de SDK van de plaats waar u rekening moet houden. Zie [correcties in versie 1.1](#BugFixesV1) voor meer informatie.

Nadat u eventuele opbouwfouten hebt gecorrigeerd, kunt u tot slot wijzigingen aanbrengen in uw toepassing om te profiteren van nieuwe functionaliteit als u wilt.

<a name="ListOfChangesV1"></a>
### <a name="list-of-breaking-changes-in-version-11"></a>Lijst met wijzigingen in versie 1.1 verbreken

De volgende lijst is gesorteerd door de kans dat de wijziging van invloed is op uw toepassingscode.

#### <a name="indexbatch-and-indexaction-changes"></a>Wijzigingen in de IndexBatch en IndexAction

`IndexBatch.Create`is gewijzigd in `IndexBatch.New` en niet meer heeft een `params` argument. U kunt `IndexBatch.New` voor batches dat mengt u verschillende soorten acties (wordt samengevoegd, verwijderen, enzovoort). Daarnaast er zijn nieuwe statische methoden voor het maken van batches waar alle acties hetzelfde zijn: `Delete`, `Merge`, `MergeOrUpload`, en `Upload`.

`IndexAction`niet meer heeft openbare constructors en de eigenschappen zijn nu onveranderlijke. Moet u de nieuwe statische methoden voor het maken van acties voor verschillende doeleinden: `Delete`, `Merge`, `MergeOrUpload`, en `Upload`. `IndexAction.Create`is verwijderd. Als u de overbelasting waarmee alleen een document hebt gebruikt, moet u `Upload` in plaats daarvan.

##### <a name="example"></a>Voorbeeld

Als uw code ziet er zo uit:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

U kunt dit wijzigen als volgt een opbouwfouten op te lossen:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Als u wilt, kunt u deze verder als volgt vereenvoudigen:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>IndexBatchException wijzigingen

De `IndexBatchException.IndexResponse` eigenschap is gewijzigd in `IndexingResults`, en het type is nu `IList<IndexingResult>`.

##### <a name="example"></a>Voorbeeld

Als uw code ziet er zo uit:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

U kunt dit wijzigen als volgt een opbouwfouten op te lossen:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>
#### <a name="operation-method-changes"></a>Bewerking methode wijzigingen

Elke bewerking in de SDK van Azure zoeken .NET worden weergegeven als een set methode overbelastingen voor synchrone en asynchrone bellers. De handtekeningen en waarbij van deze methode overbelastingen is in versie 1.1 gewijzigd.

De bewerking "Krijgen indexstatistieken" in oudere versies van de SDK blootgesteld bijvoorbeeld deze handtekeningen:

In `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

In `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

De methode handtekeningen voor dezelfde bewerking in versie 1.1 er zo uit:

In `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

In `IndexesOperationsExtensions`:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

Beginnen met versie 1.1, organiseert de Azure zoeken .NET SDK bewerking methoden anders:

 - Optionele parameters worden nu gebaseerd als standaard parameters liever dan extra methode overbelastingen. Hierdoor het aantal methode overbelastingen, soms aanzienlijk.
 - De methoden extensie verbergen nu een groot aantal de overbodige details van HTTP van de beller. Bijvoorbeeld: oudere versies van de SDK geretourneerd voor een antwoordobject met een HTTP-statuscode, die u vaak niet controleren wilt omdat de bewerking methoden genereren `CloudException` voor elke statuscode die duidt op een fout. De nieuwe extensie methoden retourneren alleen modelobjecten, zodat u het niet meer hoeft te pakken ze in uw code.
 - De core interfaces daarentegen nu expose methoden u meer controle op het niveau van HTTP zodat als u deze nodig hebt. U kunt nu doorgeven in aangepaste HTTP-headers moeten worden opgenomen in aanvragen en de nieuwe `AzureOperationResponse<T>` retourtype biedt u direct toegang tot de `HttpRequestMessage` en `HttpResponseMessage` voor de bewerking. `AzureOperationResponse`is gedefinieerd de `Microsoft.Rest.Azure` naamruimte en Hiermee vervangt u `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>ScoringParameters wijzigingen

Een nieuwe klasse met de naam `ScoringParameter` is toegevoegd in de meest recente SDK zich eenvoudiger kunt parameters profielen scoren in een zoekopdracht opgeven. Eerder de `ScoringProfiles` eigenschap van het `SearchParameters` klasse is getypt als `IList<string>`; Nu deze wordt ingevoerd als `IList<ScoringParameter>`.

##### <a name="example"></a>Voorbeeld

Als uw code ziet er zo uit:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

U kunt dit wijzigen als volgt een opbouwfouten op te lossen: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Wijzigingen in het objectmodel class

Vanwege de handtekening verandert omschreven in de [bewerkingsmethode wijzigt](#OperationMethodChanges), veel klassen in de `Microsoft.Azure.Search.Models` naamruimte zijn verwijderd of hernoemd verwijderd. Bijvoorbeeld:

 - `IndexDefinitionResponse`is vervangen door`AzureOperationResponse<Index>`
 - `DocumentSearchResponse`is gewijzigd in`DocumentSearchResult`
 - `IndexResult`is gewijzigd in`IndexingResult`
 - `Documents.Count()`nu geeft als resultaat een `long` met het aantal documenten in plaats van een`DocumentCountResponse`
 - `IndexGetStatisticsResponse`is gewijzigd in`IndexGetStatisticsResult`
 - `IndexListResponse`is gewijzigd in`IndexListResult`

Om samen te vatten, `OperationResponse`-afgeleide klassen dat bestaan alleen laten teruglopen een object model zijn verwijderd. De resterende klassen hebben gehad hun achtervoegsel gewijzigd van `Response` naar `Result`.

##### <a name="example"></a>Voorbeeld

Als uw code ziet er zo uit:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

U kunt dit wijzigen als volgt een opbouwfouten op te lossen:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

##### <a name="response-classes-and-ienumerable"></a>Antwoord klassen en IEnumerable

Een extra wijziging die invloed kan zijn op uw code is dat antwoord klassen waarin verzamelingen niet langer implementeren `IEnumerable<T>`. In plaats daarvan kunt u de eigenschap siteverzameling rechtstreeks openen. Stel dat uw code ziet er zo uit:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

U kunt dit wijzigen als volgt een opbouwfouten op te lossen:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="important-note-for-web-applications"></a>Belangrijke opmerking voor webtoepassingen

Als er een webtoepassing die serialiseert `DocumentSearchResponse` direct om zoekresultaten te versturen naar de browser, moet u uw code wijzigen of de resultaten niet correct worden geconverteerd. Stel dat uw code ziet er zo uit:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

U kunt deze wijzigen door de `.Results` eigenschap van het antwoord zoeken om op te lossen zoeken resultaat weergave:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

Er moet worden gezocht die gevallen in uw code zelf; **De compileerprogramma ontvangt u geen waarschuwing** omdat `JsonResult.Data` van het type `object`.

#### <a name="cloudexception-changes"></a>CloudException wijzigingen

De `CloudException` klasse is verplaatst van de `Hyak.Common` naamruimte naar de `Microsoft.Rest.Azure` naamruimte. Bovendien de `Error` eigenschap is gewijzigd in `Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>Wijzigingen in de SearchServiceClient en SearchIndexClient

Het type van de `Credentials` eigenschap is gewijzigd van `SearchCredentials` aan de basis klasse `ServiceClientCredentials`. Als u nodig hebt voor toegang tot de `SearchCredentials` van een `SearchIndexClient` of `SearchServiceClient`, gebruik de nieuwe `SearchCredentials` eigenschap.

In oudere versies van de SDK, `SearchServiceClient` en `SearchIndexClient` constructors die u hebt gemaakt, hebt u er een `HttpClient` parameter. Deze zijn vervangen door constructors die een `HttpClientHandler` en een matrix van `DelegatingHandler` objecten. Dit vergemakkelijkt het installeren van aangepaste handlers vooraf HTTP om aanvragen te verwerken indien nodig.

Ten slotte de constructors die hebt gemaakt, een `Uri` en `SearchCredentials` zijn gewijzigd. Bijvoorbeeld als er code die er zo uitziet:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

U kunt dit wijzigen als volgt een opbouwfouten op te lossen:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Ook de notitie die het type van de parameter referenties heeft gewijzigd in `ServiceClientCredentials`. Dit is waarschijnlijk niet van invloed zijn op uw code sinds `SearchCredentials` wordt afgeleid van `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>Doorgeven van een aanvraag-ID

In oudere versies van de SDK, kunt u een aanvraag-ID instellen op de `SearchServiceClient` of `SearchIndexClient` en deze zou worden opgenomen in elke aanvraag voor de REST API. Dit is handig voor het oplossen van problemen met uw zoekservice als u wilt contact opnemen met ondersteuning. Het is echter handiger om in te stellen van unieke ID van voor elke bewerking in plaats van dezelfde-ID gebruiken voor alle bewerkingen. Daarom de `SetClientRequestId` methoden voor het `SearchServiceClient` en `SearchIndexClient` zijn verwijderd. U kunt een aanvraag-ID in plaats daarvan doorgeven aan elke bewerkingsmethode via het optionele `SearchRequestOptions` parameter.

> [AZURE.NOTE] In toekomstige versie van de SDK, wordt we een nieuwe om voor het instellen van een aanvraag-ID globaal op de client objecten die consistent is met de methode die wordt gebruikt door andere Azure SDK's toevoegen.

#### <a name="example"></a>Voorbeeld

Als u de code die er zo uitziet:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

U kunt dit wijzigen als volgt een opbouwfouten op te lossen:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Wijzigingen in de naam van de gebruikersinterface

De namen van de gebruikersinterface van de bewerking groep hebt gewijzigd zodat deze consistent zijn met de bijbehorende eigenschapnamen:

 - Het type `ISearchServiceClient.Indexes` is gewijzigd van `IIndexOperations` naar `IIndexesOperations`.
 - Het type `ISearchServiceClient.Indexers` is gewijzigd van `IIndexerOperations` naar `IIndexersOperations`.
 - Het type `ISearchServiceClient.DataSources` is gewijzigd van `IDataSourceOperations` naar `IDataSourcesOperations`.
 - Het type `ISearchIndexClient.Documents` is gewijzigd van `IDocumentOperations` naar `IDocumentsOperations`.

Deze wijziging is waarschijnlijk niet van invloed zijn op uw code tenzij u mocks van deze interfaces voor testdoeleinden gemaakt.

<a name="BugFixesV1"></a>
### <a name="bug-fixes-in-version-11"></a>Correcties in versie 1.1

Er is een fout in oudere versies van de Azure zoeken .NET SDK met betrekking tot serialisatie van aangepaste model klassen. De fout kan optreden als u een aangepaste model klasse met een eigenschap van een waardetype niet null gemaakt.

#### <a name="steps-to-reproduce"></a>Stappen om te reproduceren

Een aangepaste model-klasse maken met een eigenschap van het waardetype niet null. Bijvoorbeeld toevoegen een openbare `UnitCount` eigenschap van het type `int` in plaats van `int?`.

Als u een document met de standaardwaarde van dit type indexeert (bijvoorbeeld 0 voor `int`), het veld worden null in Azure zoeken. Als u later die zoeken de `Search` gesprek in de weergave van `JsonSerializationException` klagen die niet met de conversie `null` naar `int`.

Ook filters werkt mogelijk niet zoals verwacht aangezien null is geschreven naar de index in plaats van de opgegeven waarde.

#### <a name="fix-details"></a>Details oplossen

We hebben dit probleem is opgelost in versie 1.1 van de SDK. Nu, hebt u een klasse model als volgt:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

en u instellen `IntValue` 0, die waarde wordt nu correct omgezet als 0 op de kabel en die zijn opgeslagen als 0 in de index. Ophalen ook werkt zoals verwacht.

Er is een probleem met de mogelijke houden met deze methode: als u een modeltype met een niet-nullable eigenschap gebruikt, hebt u om ervoor te **zorgen** dat er geen documenten in uw index een null-waarde voor het bijbehorende veld bevatten. De SDK noch de Azure zoeken REST API kunt u dit afdwingen.

Dit is niet alleen een hypothetische probleem: Stel een scenario waarbij u een nieuw veld toevoegen aan een bestaande index die van het type `Edm.Int32`. Nadat de definitie van de index wordt bijgewerkt, worden alle documenten een null-waarde voor het nieuwe veld hebt (Aangezien alle typen nullable in Azure zoeken). Als u vervolgens een klasse model met een niet-nullable gebruikt `int` eigenschap voor dat veld, krijgt u een `JsonSerializationException` als volgt uitziet als u probeert om op te halen documenten:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Daarom wordt nog steeds aangeraden nullable typen te gebruiken in uw model klassen aangeraden.

Voor meer informatie over deze fout en de correctie, raadpleegt u [Dit probleem op GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).
