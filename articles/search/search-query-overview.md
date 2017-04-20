<properties
    pageTitle="Uw Azure zoekindex query | Microsoft Azure | De zoekservice gehoste cloud"
    description="Maken van een zoekopdracht in Azure zoeken en gebruiken van zoekparameters zoekresultaten filteren en sorteren."
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

# <a name="query-your-azure-search-index"></a>Query de zoekindex van Azure
> [AZURE.SELECTOR]
- [Overzicht](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [REST](search-query-rest-api.md)

Bij het verzenden van search-aanvragen aan Azure zoeken, zijn er een aantal parameters die kunnen worden opgegeven samen met de werkelijke woorden die in het zoekvak van uw toepassing worden getypt. Deze queryparameters kunnen u sommige grondigere besturing van de volledige tekst zoekervaring bereiken.

Hieronder volgt een lijst die een korte beschrijving van gangbare toepassingen van de queryparameters in Azure zoeken. Voor volledige uitvoering van queryparameters en hun gedrag, raadpleegt u de gedetailleerde's voor de [REST API](https://msdn.microsoft.com/library/azure/dn798927.aspx) en [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.searchparameters_properties.aspx).

## <a name="types-of-queries"></a>Typen query 's

Azure zoeken biedt veel opties voor het maken van zeer krachtige query's. De twee typen query die u wilt gebruiken zijn `search` en `filter`. A `search` query zoekt u naar een of meer voorwaarden in alle _doorzoekbare_ velden in de index en werkt u de manier waarop u een zoekmachine zoals Google of Bing werken zou verwachten. A `filter` query evalueert een Booleaanse expressie over alles _filterbare_ velden in een index. In tegenstelling tot `search` query's, `filter` query's overeenkomen met de exacte inhoud van een veld, wat betekent dat ze zijn hoofdlettergevoelig voor tekenreeksvelden.

U kunt samen of afzonderlijk zoekopdrachten en filters gebruiken. Als u deze samen gebruiken, het filter eerst wordt toegepast op de volledige index en vervolgens de zoekopdracht wordt uitgevoerd op de resultaten van het filter. Filters daarom is een handige techniek queryprestaties verbeteren, aangezien ze gevolgen voor het instellen van documenten die de query moet worden verwerkt.

De syntaxis voor filterexpressies is een subset van de [OData-filter taal](https://msdn.microsoft.com/library/azure/dn798921.aspx). Voor zoekquery's kunt u de [syntaxis van de vereenvoudigd](https://msdn.microsoft.com/library/azure/dn798920.aspx) of de [syntaxis van de query Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) die hieronder worden besproken.

### <a name="simple-query-syntax"></a>De syntaxis van de eenvoudige query
De [syntaxis van de eenvoudige query](https://msdn.microsoft.com/library/azure/dn798920.aspx) is de standaardtaal van de query die wordt gebruikt in Azure zoeken. De syntaxis van de eenvoudige query ondersteunt een aantal veelvoorkomende zoeken operatoren en, inclusief of, niet, woordgroep achtervoegsel en de prioriteit van regels operatoren.

### <a name="lucene-query-syntax"></a>De syntaxis van de query Lucene
De [syntaxis van de query Lucene](https://msdn.microsoft.com/library/azure/mt589323.aspx) kunt u de sterk uiteen vastgesteld en duidelijke querytaal ontwikkeld als onderdeel van [Apache Lucene](https://lucene.apache.org/core/4_10_2/queryparser/org/apache/lucene/queryparser/classic/package-summary.html)gebruiken.

De querysyntaxis van deze gebruikt, kunt u eenvoudig bereiken van de volgende mogelijkheden: [veld beperkt query's](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fields), [bij benadering zoeken](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_fuzzy), [nabijheid zoeken](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_proximity), [waarbij de term](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_termboost), [reguliere expressies zoeken](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_regex), [zoeken met jokertekens](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_wildcard), [syntaxis over grondbeginselen](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_syntax)en [Booleaans operatoren voor gebruik van query's](https://msdn.microsoft.com/library/azure/mt589323.aspx#bkmk_boolean).



## <a name="ordering-results"></a>Ordening van resultaten
Wanneer u de resultaten van een query ontvangt, kunt u aanvragen dat Azure zoeken de resultaten door waarden in een bepaald veld hebt besteld fungeert. Standaard orders Azure zoeken de lijst met zoekresultaten op basis van de rang van elk document zoeken score zijn gebaseerd, die wordt afgeleid van [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf).

Als u wilt dat Azure zoeken om terug te keren uw resultaten hebt besteld door een andere waarde dan de score zoeken, kunt u de `orderby` parameter zoeken. Kunt u de waarde van de `orderby` parameter met veldnamen en oproepen naar de [ `geo.distance()` functie](https://msdn.microsoft.com/library/azure/dn798921.aspx) voor georuimtelijke waarden. Elke expressie kan worden gevolgd door `asc` om aan te geven dat de resultaten worden gevraagd in oplopende volgorde, en `desc` om aan te geven dat de resultaten in aflopende volgorde wordt gevraagd. De standaard-classificatie oplopende volgorde.

## <a name="paging"></a>Paginering
Azure zoeken kunt u gemakkelijk willen implementeren paginering van lijst met zoekresultaten. Met behulp van de `top` en `skip` parameters, kunt u soepel search-aanvragen waarmee u kunt de totale set zoekresultaten ontvangen in beheerbare, geordende subsets waarmee eenvoudig goede zoeken UI procedures uitgeven. Wanneer u ontvangt deze kleinere subsets van resultaten, kunt u het aantal documenten kunt ook de totale set zoekresultaten ontvangen.

U kunt meer informatie over het pagineren van zoekresultaten in het artikel [hoe u pagina zoekresultaten in Azure zoeken](search-pagination-page-layout.md).


## <a name="hit-highlighting"></a>Klik op markeren
In Azure zoeken, de exacte deel van de zoekresultaten die overeenkomen met de zoekopdracht benadrukken is in een handomdraai met behulp van de `highlight`, `highlightPreTag`, en `highlightPostTag` parameters. U kunt opgeven welke _doorzoekbare_ velden moet hebben hun overeenkomende tekst de nadruk geven door het opgeven van de exacte tekenreeks tags toe te voegen aan het begin en einde van de overeenkomende tekst die zoeken Azure geeft als resultaat.
