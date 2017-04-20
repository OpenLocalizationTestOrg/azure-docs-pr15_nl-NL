<properties
    pageTitle="Hoe u het modelleren van complexe gegevenstypen in Azure zoeken | Microsoft Azure zoeken"
    description="Geneste of hiërarchische gegevensstructuren in een Azure zoekindex met afgeplatte rijenset en verzamelingen gegevenstype kunnen worden gebaseerd."
    services="search"
    documentationCenter=""
    authors="LiamCa"
    manager="pablocas"
    editor=""
    tags="complex data types; compound data types; aggregate data types"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="09/07/2016"
    ms.author="liamca"
/>

# <a name="how-to-model-complex-data-types-in-azure-search"></a>Het model complexe gegevenstypen in Azure zoeken

Externe gegevenssets waarmee een Azure zoekindex soms gevuld zijn hiërarchische of geneste substructuren die volgt niet Splits op netjes in een tabelvormige rijenset. Voorbeelden van dergelijke structuren mogelijk krijg ik meerdere locaties en telefoonnummers voor één klant, meerdere kleuren en tekengrootten voor een enkele SKU, meerdere auteurs van één boek, enzovoort. In het modeling termen, ziet u mogelijk deze structuren genoemd *complexe gegevenstypen*, *samengestelde gegevenstypen*, *samengestelde gegevenstypen*of *gegevenstypen aggregeren*, als u wilt een paar te noemen.

Complexe gegevenstypen worden niet ondersteund in Azure zoeken, maar een beproefde tijdelijke oplossing bevat een proces van het samenvoegen van de structuur en vervolgens opnieuw samenstellen de structuur van de binnenkant met het gegevenstype van een **siteverzameling** . Als u de methode beschreven in dit artikel kunt u de inhoud moet worden gezocht, beperkte, gefilterd en gesorteerd.

## <a name="example-of-a-complex-data-structure"></a>Voorbeeld van een complexe gegevensstructuur

De betreffende gegevens bevindt zich doorgaans voor als een set van JSON of XML-documenten of items in een winkel NoSQL zoals DocumentDB. Structuur, de uitdaging komen voort niet kan meerdere onderliggende items die moeten worden doorzocht en worden gefilterd.  Als uitgangspunt voor het illustreren van de tijdelijke oplossing, voert u het volgende JSON-document waarin een reeks contactpersonen als voorbeeld:

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

Terwijl de velden met de naam 'id', 'naam' en 'bedrijf' kunnen eenvoudig worden toegewezen-op-een als velden binnen een Azure zoekindex, het veld 'locaties' bevat een matrix van locaties, die beide een reeks locatie-id's, evenals de beschrijvingen van de locatie. Gezien het feit dat Azure zoeken geen een gegevenstype dat hiervoor ondersteuning is geconfigureerd, moeten we een andere manier om dit te modelleren in Azure zoeken. 

> [AZURE.NOTE] Deze techniek wordt ook beschreven door Kirk Evans in een blogbericht [Indexeren DocumentDB met Azure zoeken](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), welke ziet u een methode genoemd "samenvoegen de gegevens", waarbij u een veld met de naam wilt hebben `locationsID` en `locationsDescription` die beide zijn [verzamelingen](https://msdn.microsoft.com/library/azure/dn798938.aspx) (of een matrix van tekenreeksen).   

## <a name="part-1-flatten-the-array-into-individual-fields"></a>Deel 1: Het samenvoegen van de matrix in afzonderlijke velden

Als u wilt een Azure zoekindex die geschikt is voor deze dataset hebt gemaakt, maakt u afzonderlijke velden voor de geneste substructure: `locationsID` en `locationsDescription` met het gegevenstype van [verzamelingen](https://msdn.microsoft.com/library/azure/dn798938.aspx) (of een matrix van tekenreeksen). In deze velden wilt u de waarden '1' en '2' index naar de `locationsID` veld voor John Smith en de waarden '3' en '4' in de `locationsID` voor Jen Campbell-veld.  

Uw gegevens binnen Azure zoeken er als volgt uit: 

![voorbeeldgegevens, 2 rijen](./media/search-howto-complex-data-types/sample-data.png)


## <a name="part-2-add-a-collection-field-in-the-index-definition"></a>Deel 2: Een verzameling veld toevoegen in de definitie van de index

In het schema index uitzien de velddefinities zoals in dit voorbeeld.

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## <a name="validate-search-behaviors-and-optionally-extend-the-index"></a>Valideer de zoekopdracht gedrag en de index (optioneel) uitbreiden

Als u de index gemaakt en de gegevens geladen, kunt u nu testen de oplossing om te controleren of de uitvoering van de zoekopdracht query ten opzichte van de gegevensset. Elke **siteverzameling** het veld moet **doorzoekbare**, **filterbare** en **facetable**. U moet kunnen uitvoeren van query's zoals:

* Zoeken naar alle personen die in het Adventureworks hoofdkantoor werken.
* Krijg een telling van het aantal personen die in een start Office werken.  
* Weergeven van de personen die bij een start Office werken, welke andere kantoren ze samen met een telling van de personen die op elke locatie werken.  

Wanneer u moet een zoekopdracht die worden gecombineerd zowel de id van de locatie, evenals de Locatiebeschrijving van de waar deze techniek valt uit elkaar trekken worden is. Bijvoorbeeld:

* Zoeken naar alle personen waarvoor ze een start Office hebben en heeft een locatie-ID van 4.  

Als u de oorspronkelijke inhoud van dit vroeger intrekken:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

Echter nu dat we de gegevens in afzonderlijke velden gescheiden hebben, zijn er geen manier omdat u weet dat als de Office start voor Jen Campbell zich tot verhoudt `locationsID 3` of `locationsID 4`.  

Als u wilt in dit geval een ander veld in de index die alle gegevens worden gecombineerd in één collectie te definiëren.  In ons voorbeeld we dit veld bellen `locationsCombined` en we wordt scheidt u de inhoud met een `||` Hoewel u kunt kiezen een scheidingsteken waarvan u denkt dat is een unieke set tekens voor uw inhoud. Bijvoorbeeld: 

![voorbeeldgegevens, 2 rijen met een scheidingsteken](./media/search-howto-complex-data-types/sample-data-2.png)

Met deze `locationsCombined` veld we nu geschikt voor nog meer query's, zoals:

* Een telling van werknemers binnen een 'Start kantoor' met locatie Id van '4' weergeven.  
* Zoeken naar personen die bij een start Office werken met locatie Id '4'. 

## <a name="limitations"></a>Beperkingen

Dit is handig voor een aantal scenario's, maar het is niet van toepassing in elk geval.  Bijvoorbeeld:

1. Als u geen een statische reeks velden in uw complexe gegevenstype en er geen manier is om alle mogelijke typen toewijzen aan één veld. 
2. Bijwerken van de geneste objecten is vereist voor sommige extra werk om te bepalen precies wat u moet zijn bijgewerkt in de zoekindex van Azure

## <a name="sample-code"></a>Voorbeeld van een code

Het indexeren van een complexe JSON-gegevensverzameling in Azure zoeken naar en een aantal query's uitvoeren via deze dataset bij deze [GitHub cessies‑retrocessies](https://github.com/liamca/AzureSearchComplexTypes)kunt u een voorbeeld bekijken.

## <a name="next-step"></a>Volgende stap

[Stem voor ondersteuning voor complexe gegevenstypen](https://feedback.azure.com/forums/263029-azure-search) op de Azure zoeken UserVoice pagina en eventuele aanvullende feedback die we aandachtspunten met betrekking tot uitvoering van de functie. U kunt ook contact maken voor mij rechtstreeks op Twitter bij @liamca.


 