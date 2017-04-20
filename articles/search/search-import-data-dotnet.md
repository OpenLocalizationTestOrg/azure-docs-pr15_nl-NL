<properties
    pageTitle="Gegevens uploaden in Azure zoekprogramma's via de .NET SDK | Microsoft Azure | De zoekservice gehoste cloud"
    description="Informatie over het uploaden van gegevens naar een index in Azure zoekprogramma's via de .NET SDK."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="upload-data-to-azure-search-using-the-net-sdk"></a>Gegevens uploaden naar Azure zoekprogramma's via de .NET SDK
> [AZURE.SELECTOR]
- [Overzicht](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [REST](search-import-data-rest-api.md)

In dit artikel wordt uitgelegd hoe u het gebruik van de [Azure zoeken .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx) gegevens wilt importeren in een Azure-zoekindex.

Voordat u deze procedure begint, moet u al hebt [gemaakt van een Azure-zoekindex](search-what-is-an-index.md). In dit artikel ook wordt ervan uitgegaan dat u al hebt gemaakt een `SearchServiceClient` object, zoals aangegeven in [een Azure zoekindex met de SDK .NET maken](search-create-index-dotnet.md#CreateSearchServiceClient).

Houd er rekening mee dat alle steekproef-code in dit artikel is geschreven in C#. U vindt de volledige bron code [op GitHub](http://aka.ms/search-dotnet-howto).

Om te kunnen push documenten in uw index met de SDK .NET, moet u naar:

  1. Maak een `SearchIndexClient` object verbinding maken met de zoekindex.
  2. Maak een `IndexBatch` met de documenten wordt toegevoegd, gewijzigd of verwijderd.
  3. Bellen de `Documents.Index` methode van uw `SearchIndexClient` verzenden de `IndexBatch` aan de zoekindex.

## <a name="i-create-an-instance-of-the-searchindexclient-class"></a>Ik. Een exemplaar van de klas SearchIndexClient maken
Om gegevens te importeren in uw index met behulp van de Azure zoeken .NET SDK, moet u maken van een exemplaar van de `SearchIndexClient` class. U kunt samenstellen dit exemplaar uzelf, maar de gemakkelijker als u al een `SearchServiceClient` exemplaar bellen de `Indexes.GetClient` methode. Hier is bijvoorbeeld hoe u zou krijgen een `SearchIndexClient` voor de index met de naam "hotels" uit een `SearchServiceClient` met de naam `serviceClient`:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [AZURE.NOTE] Index management en populatie in een normale zoektoepassing wordt uitgevoerd door een afzonderlijk onderdeel van zoekopdrachten. `Indexes.GetClient`is handig voor het invullen van een index omdat deze u de problemen bespaart aan te bieden een andere `SearchCredentials`. Dit doet u dit door de beheerder-sleutel die u gebruikt de `SearchServiceClient` naar het nieuwe `SearchIndexClient`. In het gedeelte van de toepassing die wordt uitgevoerd van query's, het is echter beter maken de `SearchIndexClient` rechtstreeks zodat u kunt in een query gebruiken in plaats van een sleutel beheerder doorgeven. Dit is consistent is met het [principe van het laagste bevoegdheidsniveau](https://en.wikipedia.org/wiki/Principle_of_least_privilege) en kunt u uw toepassing veiliger. U vindt meer informatie over de beheerder en sleutels van de query in de [verwijzing Azure zoeken REST API op MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx).

`SearchIndexClient`heeft een `Documents` eigenschap. Deze eigenschap bevat alle methoden die u wilt toevoegen, wijzigen, verwijderen of query van documenten in de index.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Bepalen welke indexing actie moet worden gebruikt
Als u wilt importeren van gegevens met behulp van de .NET SDK, moet u als pakket inpakken up van uw gegevens in een `IndexBatch` object. Een `IndexBatch` omvat een verzameling `IndexAction` objecten, die elk bevat een document en een eigenschap Azure zoeken welke actie moet uitvoeren op dat document (uploaden, samenvoegen, verwijderen, enzovoort). Afhankelijk van welke van de onder acties u kiest, alleen bepaalde velden opgenomen voor elk document worden moeten:

Actie | Beschrijving | Vereiste velden voor elk document | Notities
--- | --- | --- | ---
`Upload` | Een `Upload` actie is vergelijkbaar met een "upsert" waar het document worden ingevoegd als deze nieuwe en bijgewerkt/vervangen indien aanwezig. | toets, plus eventuele andere velden die u wilt definiëren | Bij een bestaand document bijwerken/vervanging, een veld dat niet is opgegeven in de uitnodiging heeft het veld is ingesteld op `null`. Dit gebeurt zelfs wanneer het veld eerder is ingesteld op een niet-null-waarde.
`Merge` | Een bestaand document wordt bijgewerkt met de opgegeven velden. Als het document niet in de index bestaat, mislukt de samenvoeging. | toets, plus eventuele andere velden die u wilt definiëren | Elk veld dat u in een samenvoegbewerking opgeeft wordt vervangen door het bestaande veld in het document. Dit geldt ook voor velden van het type `DataType.Collection(DataType.String)`. Als het document bevat een veld bijvoorbeeld `tags` met waarde `["budget"]` en u een samenvoegbewerking met waarde uitvoeren `["economy", "pool"]` voor `tags`, de laatste waarde van de `tags` veld `["economy", "pool"]`. Worden niet `["budget", "economy", "pool"]`.
`MergeOrUpload` | Deze actie lijkt veel op `Merge` als een document met de opgegeven sleutel al in de index bestaat. Als het document niet bestaat nog, lijkt veel op `Upload` met een nieuw document. | toets, plus eventuele andere velden die u wilt definiëren | -
`Delete` | Hiermee verwijdert het opgegeven document uit de index. | alleen sleutel | Velden die u opgeeft andere dan het veld sleutel worden genegeerd. Als u een afzonderlijk veld verwijderen uit een document wilt, gebruikt u `Merge` in plaats daarvan en gewoon het veld expliciet ingesteld op null.

U kunt opgeven welke actie die u wilt gebruiken met de verschillende statische methoden van de `IndexBatch` en `IndexAction` klassen, zoals wordt weergegeven in het volgende gedeelte.

## <a name="iii-construct-your-indexbatch"></a>III. Uw IndexBatch maken
Nu u weet welke acties aan uw documenten, bent u gereed om samen te stellen de `IndexBatch`. Het volgende voorbeeld ziet hoe u een batch maken met een paar verschillende acties. Ons voorbeeld gebruikt een aangepaste klasse genoemd `Hotel` die wordt toegewezen aan een document in de index "hotels".

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                BaseRate = 199.0,
                Description = "Best hotel in town",
                DescriptionFr = "Meilleur hôtel en ville",
                HotelName = "Fancy Stay",
                Category = "Luxury",
                Tags = new[] { "pool", "view", "wifi", "concierge" },
                ParkingIncluded = false,
                SmokingAllowed = false,
                LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                Rating = 5,
                Location = GeographyPoint.Create(47.678581, -122.131577)
            }),
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                BaseRate = 79.99,
                Description = "Cheapest hotel in town",
                DescriptionFr = "Hôtel le moins cher en ville",
                HotelName = "Roach Motel",
                Category = "Budget",
                Tags = new[] { "motel", "budget" },
                ParkingIncluded = true,
                SmokingAllowed = true,
                LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                Rating = 1,
                Location = GeographyPoint.Create(49.678581, -122.131577)
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close to town hall and the river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

In dit geval gebruiken we `Upload`, `MergeOrUpload`, en `Delete` als onze acties zoeken, zoals aangegeven door de methoden met de naam op de `IndexAction` class.

Wordt ervan uitgegaan dat in dit voorbeeld "hotels" index al wordt gevuld met een aantal documenten. Houd er rekening mee hoe we geen om op te geven van alle mogelijke documentvelden bij gebruik van `MergeOrUpload` en hoe we de document-toets alleen opgegeven (`HotelId`) bij gebruik van `Delete`.

Ook rekening mee dat u kunt alleen tot 1000 documenten in een enkele indexing aanvraag.

> [AZURE.NOTE] In dit voorbeeld zijn er verschillende acties toepassen op verschillende documenten. Als u wilt dezelfde acties uitvoeren in alle documenten in de batch, in plaats van de beller `IndexBatch.New`, kunt u de andere methoden voor het statische `IndexBatch`. U kunt bijvoorbeeld batches maken door te bellen `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, of `IndexBatch.Delete`. Deze methoden gebruikmaken van een verzameling documenten (objecten van het type `Hotel` in dit voorbeeld) in plaats van `IndexAction` objecten.

## <a name="iv-import-data-to-the-index"></a>IV. Gegevens importeren in de index
Nu dat u een geïnitialiseerde hebt `IndexBatch` object aan, kunt u deze verzenden aan de index door te bellen `Documents.Index` op uw `SearchIndexClient` object. Het volgende voorbeeld wordt getoond hoe bellen `Index`, evenals een aantal extra stappen die u moet uitvoeren:

```csharp
try
{
    indexClient.Documents.Index(batch);
}
catch (IndexBatchException e)
{
    // Sometimes when your Search service is under load, indexing will fail for some of the documents in
    // the batch. Depending on your application, you can take compensating actions like delaying and
    // retrying. For this simple demo, we just log the failed document keys and continue.
    Console.WriteLine(
        "Failed to index some of the documents: {0}",
        String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
}

Console.WriteLine("Waiting for documents to be indexed...\n");
Thread.Sleep(2000);
```

Opmerking de `try` / `catch` rond de oproep door naar de `Index` methode. De blokkering variabel omgaat met een belangrijke fout hoofdletters/kleine letters voor het indexeren. Als uw Azure-zoekservice mislukt indexeren enkele van de documenten in de batch, een `IndexBatchException` wordt gegenereerd door `Documents.Index`. Dit kan gebeuren als u documenten zijn indexeren terwijl uw service druk bezet is. **Het is raadzaam deze zaak in code expliciet voor de verwerking.** U kunt uitstellen en probeer het opnieuw indexeren van de documenten die is mislukt, of u kunt zich aanmelden en doorgaan zoals in het voorbeeld bevat, of u iets anders is afhankelijk van uw toepassing gegevens consistentie vereisten kunt doen.

Ten slotte de code in het voorbeeld hierboven vertragingen twee seconden ingedrukt. Indexering gebeurt asynchroon op in uw Azure Search-service, zodat de voorbeeldtoepassing wacht even om ervoor te zorgen moet dat de documenten beschikbaar voor het zoeken zijn. Vertragingen als volgt zijn meestal alleen nodig in demo's, tests en voorbeeldtoepassingen.

<a name="HotelClass"></a>
### <a name="how-the-net-sdk-handles-documents"></a>De verwerking van de .NET SDK op documenten

U mogelijk worden wilt u weten hoe de SDK van Azure zoeken .NET is kunt uploaden exemplaren van een gebruiker gedefinieerde klasse zoals `Hotel` aan de index. Om u te helpen voorkomen dat vraag beantwoorden, eens kijken naar de `Hotel` klasse, die aan het schema index is gedefinieerd in [maken indexen Azure zoeken met de SDK .NET](search-create-index-dotnet.md#DefineIndex)toegewezen:

```csharp
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    public string HotelId { get; set; }

    public double? BaseRate { get; set; }

    public string Description { get; set; }

    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    public string HotelName { get; set; }

    public string Category { get; set; }

    public string[] Tags { get; set; }

    public bool? ParkingIncluded { get; set; }

    public bool? SmokingAllowed { get; set; }

    public DateTimeOffset? LastRenovationDate { get; set; }

    public int? Rating { get; set; }

    public GeographyPoint Location { get; set; }

    // ToString() method omitted for brevity...
}
```

Het eerste dat opvalt is dat elke openbare eigenschap van `Hotel` overeenkomt met een veld in de definitie van de index, maar één essentieel verschil: de naam van elk veld begint met een kleine letter ("kamelen letters'), terwijl de naam van elke openbare eigenschap van `Hotel` begint met een hoofdletter ("Pascal hoofdletters/kleine letters'). Dit is een algemene scenario in .NET-toepassingen die gegevens-binding uitvoeren waar het doelschema buiten de controle van de toepassingsontwikkelaar is. In plaats van dat u hoeft te handelen in strijd met de .NET richtlijnen naming doordat eigenschap namen kamelen-hoofdletters/kleine letters, onderscheidt u de SDK de eigenschapnamen toewijzen aan kamelen-hoofdlettergebruik automatisch met de `[SerializePropertyNamesAsCamelCase]` kenmerk.

> [AZURE.NOTE] De SDK van Azure zoeken .NET wordt de bibliotheek [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) serialiseren en terugconverteren van uw aangepaste modelobjecten en naar JSON gebruikt. U kunt deze serialisatie kunt aanpassen, indien nodig. U vindt meer informatie in [upgraden naar de Azure zoeken .NET SDK versie 1.1](search-dotnet-sdk-migration.md#WhatsNew). Een voorbeeld van dit is het gebruik van de `[JsonProperty]` kenmerk op de `DescriptionFr` eigenschap in de bovenstaande voorbeeldcode.

De tweede belangrijk punt bij het `Hotel` class zijn de gegevenstypen van de openbare eigenschappen. De typen .NET deze eigenschappen toewijzen aan hun equivalente veldtypen in de definitie van de index. Bijvoorbeeld de `Category` tekenreeks van de eigenschap wordt toegewezen aan de `category` veld van het type `DataType.String`. Er zijn vergelijkbaar type toewijzingen tussen `bool?` en `DataType.Boolean`, `DateTimeOffset?` en `DataType.DateTimeOffset`, enz. De specifieke regels voor de toewijzing van het type worden beschreven met de `Documents.Get` methode op [MSDN](https://msdn.microsoft.com/library/azure/dn931291.aspx).

Deze mogelijkheid om te gebruiken van uw eigen klassen als documenten werkt in beide richtingen; U kunt ook ophalen van zoekresultaten en de SDK automatisch terugconverteren ze op het type van uw keuze, zoals wordt weergegeven in het [volgende artikel](search-query-dotnet.md).

> [AZURE.NOTE] De SDK van Azure zoeken .NET ondersteunen ook dynamisch getypt documenten met de `Document` class, dat wil een toewijzing sleutelwaarde/met veldnamen naar waarden zeggen. Dit is handig in scenario's waarin u het schema index tijdens de ontwerpfase niet weet, of als deze kunt koppelen aan specifieke model klassen zou zijn. Alle methoden in de SDK die betrekking op documenten hebben hebt overbelastingen die met werken de `Document` klasse, evenals ten zeerste getypt overbelastingen waarop een algemene typeparameter maken. Alleen de laatste worden in de steekproef-code in dit artikel gebruikt. U vindt meer informatie over de `Document` klasse [op MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.document.aspx).

**Een belangrijke opmerking over gegevenstypen**

Bij het ontwerpen van uw eigen model klassen toewijzen aan een Azure zoekindex, het is raadzaam te declareren eigenschappen van waardetypen, zoals `bool` en `int` moeten nullable (bijvoorbeeld `bool?` in plaats van `bool`). Als u een niet-nullable eigenschap gebruikt, hebt u om ervoor te **zorgen** dat er geen documenten in uw index een null-waarde voor het bijbehorende veld bevatten. De SDK noch de zoekservice van Azure kunt u dit afdwingen.

Dit is niet alleen een hypothetische probleem: Stel een scenario waarbij u een nieuw veld toevoegen aan een bestaande index die van het type `DataType.Int32`. Nadat de definitie van de index wordt bijgewerkt, worden alle documenten een null-waarde voor het nieuwe veld hebt (Aangezien alle typen nullable in Azure zoeken). Als u vervolgens een klasse model met een niet-nullable gebruikt `int` eigenschap voor dat veld, krijgt u een `JsonSerializationException` als volgt uitziet als u probeert om op te halen documenten:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Om die reden is het raadzaam dat u nullable typen in uw model klassen aangeraden gebruikt.

## <a name="next"></a>Volgende
Na het vullen van de zoekindex van Azure, bent u klaar om te beginnen met het verlenen van query's om te zoeken naar documenten. Zie [Uw Azure-zoekindex Query](search-query-overview.md) voor meer informatie.
