<properties
    pageTitle="Maken van een Azure zoekindex | Microsoft Azure | De zoekservice gehoste cloud"
    description="Wat is een index in Azure zoeken en hoe wordt het gebruikt?"
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index"></a>Azure zoeken indexen maken
> [AZURE.SELECTOR]
- [Overzicht](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)

## <a name="what-is-an-index"></a>Wat is een index?

Een *index* is een permanente winkel van *documenten* en andere constructies die worden gebruikt door een Azure-zoekservice. Een document is één exemplaar van doorzoekbare gegevens in de index. Een e-commerce-winkel mogelijk hebt u een document voor elk item dat zij verkopen en bijvoorbeeld een organisatie nieuws mogelijk hebt u een document voor elk artikel, enzovoort. Deze concepten toewijzen aan meer bekende equivalenten voor de database: een *index* is in essentie gelijk is aan een *tabel*en *documenten* zijn ongeveer gelijk zijn aan de *rijen* in een tabel.

Wanneer u documenten toevoegen/uploaden en zoekopdrachten gaat naar Azure zoeken uitvoeren, moet u uw aanvragen voor een specifieke index in uw zoekservice indienen.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Veldtypen en kenmerken in een zoekindex van Azure

Als u uw schema definieert, moet u de naam, het type en de kenmerken van elk veld in de index. Het veldtype classificeert de gegevens die zijn opgeslagen in dat veld. Kenmerken zijn ingesteld voor afzonderlijke velden kunt u opgeven hoe het veld wordt gebruikt. De volgende tabellen opsommen de typen en de kenmerken die u kunt opgeven.


### <a name="field-types"></a>Veldtypen
|Type|Beschrijving|
|------------|-----------|
|*Edm.String*|Tekst die u kunt desgewenst worden ge? exeerd voor volledige tekst zoeken (woordafbreking, als gevolg, enzovoort).|
|*Collection(EDM.String)*|Een lijst van reeksen die u kunt desgewenst worden ge? exeerd voor zoeken in volledige tekst. Er is geen theoretische bovengrens van het aantal items in een verzameling, maar de 16 MB bovengrens op pakketgrootte is van toepassing op siteverzamelingen.|
|*Edm.Boolean*|Waar/onwaar waarden bevat.|
|*Edm.Int32*|waarden van de 32-bits geheel getal.|
|*Edm.Int64*|waarden van de 64-bits geheel getal.|
|*Edm.Double*|Dubbele precisie numerieke gegevens.|
|*Edm.DateTimeOffset*|Tijd datumwaarden weergegeven in de notatie OData V4 (bijvoorbeeld `yyyy-MM-ddTHH:mm:ss.fffZ` of `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`).|
|*Edm.GeographyPoint*|Een punt dat staat voor een geografische locatie op de wereldbol.|

U vindt meer informatie over van Azure zoekactie [ondersteunde gegevenstypen op MSDN](https://msdn.microsoft.com/library/azure/dn798938.aspx).



### <a name="field-attributes"></a>Veldkenmerken
|Kenmerk|Beschrijving|
|------------|-----------|
|*Toets*|Een tekenreeks die de unieke ID van elk document, gebruikt voor het opzoeken van document bevat. Elke index moet één sleutel hebben. Slechts één veld is de sleutel en het type moet zijn ingesteld op Edm.String.|
|*Opvraagbare*|Hiermee geeft u of een veld in een zoekresultaat kan worden geretourneerd.|
|*Filterbare*|Hiermee kunt u het veld moet worden gebruikt in een query filteren.|
|*Sorteerbaar*|Hiermee kunt een query toevoegen aan lijst met zoekresultaten in dit veld sorteren.|
|*Facetable*|Hiermee kunt u een veld moet worden gebruikt in een [beperkte navigatie](search-faceted-navigation.md) -structuur voor de gebruiker gericht filteren. Meestal werken velden met meest herhaalde waarden die u kunt meerdere documenten groeperen (bijvoorbeeld meerdere documenten die onder een enkel huisstijl of servicecategorie vallen) het best als aspecten.|
|*Doorzoekbare*|Hiermee markeert het veld als volledige tekst gezocht.|

U vindt meer informatie over van Azure zoekactie [indexkenmerken op MSDN](https://msdn.microsoft.com/library/azure/dn798941.aspx).



## <a name="guidance-for-defining-an-index-schema"></a>Richtlijnen voor het definiëren van een schema index

U ontwerpt de Zijtabs met letter, nemen uw tijd in de planning fase om na te denken tot en met elk beslissing. Het is belangrijk dat u uw zoekopdracht gebruiker ervaring en zakelijke behoeften in gedachten behouden bij het ontwerpen van de Zijtabs met letter, zoals de [kenmerken juist zijn ingesteld](https://msdn.microsoft.com/library/azure/dn798941.aspx)door elk veld moet worden toegewezen. Een index wijzigen nadat deze is geïmplementeerd heeft betrekking op opnieuw te bouwen en het laden van de gegevens.


Als voor gegevensopslag wijzigen, kunt u verhogen of verlagen van capaciteit met toevoegen of verwijderen van de partities. Zie [uw zoekservice in Azure beheren](search-manage.md) of [Service limieten](search-limits-quotas-capacity.md)voor meer informatie.
