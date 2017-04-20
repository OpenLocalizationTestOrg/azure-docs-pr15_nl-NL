<properties
    pageTitle="Indexen Azure zoeken met de SDK .NET maken | Microsoft Azure | De zoekservice gehoste cloud"
    description="Een index maken in code met behulp van de Azure zoeken .NET SDK."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="create-an-azure-search-index-using-the-net-sdk"></a>Indexen Azure zoeken met de SDK .NET maken
> [AZURE.SELECTOR]
- [Overzicht](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)


In dit artikel wordt u begeleid bij het proces van het maken van een Azure zoeken [index](https://msdn.microsoft.com/library/azure/dn798941.aspx) met behulp van de [Azure zoeken .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx).

Voordat u deze handleiding volgen en een index maken, hebt u al [gemaakt van een Azure-zoekservice](search-create-service-portal.md).

Houd er rekening mee dat alle steekproef-code in dit artikel is geschreven in C#. U vindt de volledige bron code [op GitHub](http://aka.ms/search-dotnet-howto).

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Ik. Uw Azure zoekservice van beheerder api-sleutel identificeren
Nu u hebt een Azure-zoekservice ingericht, u bent bijna klaar om te verzoeken ten opzichte van uw service-eindpunt met de .NET-SDK. Eerst moet u deze een van de beheerder api-toetsen die is gegenereerd voor de zoekservice die u deze is ingericht. De .NET SDK stuurt op elk verzoek deze api-sleutel uit uw service. Met een geldige sleutel tot stand brengt vertrouwen, op basis per verzoek tussen het verzenden van de aanvraag en de service die omgaat met deze toepassing.

1. Als u wilt zoeken van uw service api-toetsen u moet zich aanmelden bij de [Portal van Azure](https://portal.azure.com/)
2. Ga naar uw Azure zoekservice van blade
3. Klik op het pictogram "Toetsen"

Uw service heeft *beheerder* en *query sleutels*.

  - Uw primaire en secundaire *beheerder toetsen* verlenen volledige rechten voor alle bewerkingen, waaronder de mogelijkheid te beheren van de service, maken en verwijderen van indexen, Indexeerfuncties en gegevensbronnen. Er zijn twee sleutels zodat u de secundaire sleutel gebruiken blijven kunt als u besluit om opnieuw te genereren van de primaire sleutel en vice versa.
  - Uw *query toetsen* alleen-lezen toegang verlenen tot indexen en documenten en meestal zijn verdeeld over clienttoepassingen die zoeken verzoeken.

Voor de toepassing van een index maken, gebruikt u een uw sleutel primaire of secundaire beheerder.

<a name="CreateSearchServiceClient"></a>
## <a name="ii-create-an-instance-of-the-searchserviceclient-class"></a>II. Een exemplaar van de klas SearchServiceClient maken
Als u wilt gaan gebruiken van de Azure zoeken .NET SDK, moet u maken van een exemplaar van de `SearchServiceClient` class. Deze klasse heeft verschillende constructors. Gewenste wordt uw naam van de service zoeken en een `SearchCredentials` object als parameters. `SearchCredentials`uw api-sleutel terugloopt.

De onderstaande code wordt een nieuw `SearchServiceClient` met waarden voor de servicenaam zoekopdracht en api-sleutel die zijn opgeslagen in van de toepassing config-bestand (`app.config` of `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`SearchServiceClient`heeft een `Indexes` eigenschap. Deze eigenschap bevat alle methoden die u maken wilt, lijst, bijwerken of verwijderen van Azure zoeken indexen.

> [AZURE.NOTE] De `SearchServiceClient` klasse beheert verbindingen met de zoekservice. Om te voorkomen dat te veel verbindingen, moet u probeert te delen van één exemplaar van `SearchServiceClient` in uw toepassing indien mogelijk. De methoden zijn thread-veilige dergelijke delen inschakelen.

<a name="DefineIndex"></a>
## <a name="iii-define-your-azure-search-index-using-the-index-class"></a>III. Definieer uw Azure-zoekopdracht index met de `Index` class
Één aanroep van de `Indexes.Create` methode maakt de Zijtabs met letter. Deze methode heeft als een parameter een `Index` object dat de zoekindex van Azure definieert. U moet maken een `Index` object en geïnitialiseerd als volgt:

1. Stel de `Name` eigenschap van het `Index` object op de naam van de Zijtabs met letter.
2. Stel de `Fields` eigenschap van het `Index` object naar een matrix van `Field` objecten. Elk van de `Field` objecten definieert het gedrag van een veld in de index. U kunt de naam van het veld toe aan de constructor, samen met het gegevenstype (of analyzer voor tekenreeksvelden) opgeeft. U kunt ook andere eigenschappen zoals instellen `IsSearchable`, `IsFilterable`, enz.

Het is belangrijk dat u uw zoekopdracht gebruiker ervaring en zakelijke behoeften in gedachten houden bij het ontwerpen van de Zijtabs met letter als elk `Field` de [gewenste eigenschappen](https://msdn.microsoft.com/library/azure/dn798941.aspx)moeten worden toegewezen. Deze eigenschappen bepalen welke functies (filteren, faceting, sorteren zoeken in volledige tekst, enzovoort) zoeken van toepassing op welke velden. Voor een eigenschap die u niet expliciet instellen wilt, de `Field` klasse standaardwaarden uitschakelen van de corresponderende zoekfunctie, tenzij u de map instellen.

In ons voorbeeld hebt we onze index "hotels" benoemde en onze velden als volgt gedefinieerd:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = new[]
    {
        new Field("hotelId", DataType.String)                       { IsKey = true, IsFilterable = true },
        new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("description", DataType.String)                   { IsSearchable = true },
        new Field("description_fr", AnalyzerName.FrLucene),
        new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true, IsSortable = true },
        new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
        new Field("smokingAllowed", DataType.Boolean)               { IsFilterable = true, IsFacetable = true },
        new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
    }
};
```

We de eigenschapswaarden zorgvuldig hebt gekozen voor elk `Field` op basis van hoe we dat ze worden gebruikt in een toepassing. Bijvoorbeeld, is de kans groot dat personen zoeken naar hotels zal geïnteresseerd trefwoord overeenkomsten op de `description` veld, zodat we zoeken in volledige tekst voor dat veld door in te stellen inschakelen `IsSearchable` naar `true`.

Houd er rekening mee dat precies één veld in de index van het type `DataType.String` moet de aangewezen als _het sleutelveld_ door in te stellen `IsKey` naar `true` (Zie `hotelId` in het voorbeeld hierboven).

De definitie van de bovenstaande index gebruikt een analyzer aangepaste taal voor de `description_fr` veld omdat deze is bedoeld voor de opslag van Franse tekst. Zie [het onderwerp op MSDN ondersteunen taal](https://msdn.microsoft.com/library/azure/dn879793.aspx) , evenals de bijbehorende [weblog posten](https://azure.microsoft.com/blog/language-support-in-azure-search/) voor meer informatie over taal analyzers.

> [AZURE.NOTE]  Merk op dat door het doorgeven van `AnalyzerName.FrLucene` in de constructor de `Field` worden automatisch van het type `DataType.String` en `IsSearchable` ingesteld op `true`.

## <a name="iv-create-the-index"></a>IV. De index maken
Nu dat u een geïnitialiseerde hebt `Index` object, kunt u de index maken door te bellen `Indexes.Create` op uw `SearchServiceClient` object:

```csharp
serviceClient.Indexes.Create(definition);
```

Voor een succesvolle aanvraag retourneert de methode normaal. Als er een probleem met het verzoek zoals een ongeldige parameter, de weergave van de methode `CloudException`.

Wanneer u klaar bent met een index en u wilt verwijderen, net bellen de `Indexes.Delete` methode op uw `SearchServiceClient`. Dit is bijvoorbeeld hoe we de index 'hotels' wilt verwijderen:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [AZURE.NOTE] De voorbeeldcode in dit artikel worden de synchroon methoden van de Azure zoeken .NET SDK voor eenvoudig gebruikt. Het is raadzaam dat u de asynchrone methoden in uw eigen toepassingen gebruiken om ze te houden scalable en heeft gereageerd. Bijvoorbeeld, in de voorbeelden hierboven kunt u `CreateAsync` en `DeleteAsync` in plaats van `Create` en `Delete`.

## <a name="next"></a>Volgende
Na het maken van een Azure zoekindex, bent u klaar uw inhoud in de index te [uploaden](search-what-is-data-import.md) zodat u kunt beginnen met het zoeken van uw gegevens.
