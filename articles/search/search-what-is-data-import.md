<properties
    pageTitle="Gegevens uploaden in Azure zoeken | Microsoft Azure | De zoekservice gehoste cloud"
    description="Informatie over het uploaden van gegevens naar een index in Azure zoeken."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search"></a>Gegevens uploaden naar Azure zoeken
> [AZURE.SELECTOR]
- [Overzicht](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [REST](search-import-data-rest-api.md)


## <a name="data-upload-models-in-azure-search"></a>Gegevens uploaden modellen in Azure zoeken
Er zijn twee manieren om te vullen de zoekindex van Azure met uw gegevens. De eerste optie is handmatig uw gegevens in de index met de Azure zoeken [REST API](search-import-data-rest-api.md) of [.NET SDK](search-import-data-dotnet.md)drukken. De tweede optie is [een ondersteunde gegevensbron wijst](search-indexer-overview.md) naar de zoekindex van Azure en laat Azure zoeken automatisch ophalen van uw gegevens in de zoekservice.

Deze handleiding komen alleen instructies over het gebruik van de push-model van gegevens uploaden (die wordt alleen ondersteund in de [REST API](search-import-data-rest-api.md) en [.NET SDK](search-import-data-dotnet.md)), maar u kunt nog meer informatie over het onderstaande halen-model.

### <a name="push-data-to-an-index"></a>Push-gegevens aan een index

Deze methode verwijst naar uw gegevens via programmacode worden verzonden naar Azure zoeken zodat deze beschikbaar is voor het zoeken. Voor toepassingen erg laag latentie vereisten hebt (bijvoorbeeld als u bewerkingen synchroon met dynamische voorraad databases nodig zoeken), het model push is de enige optie.

U kunt de [REST API](https://msdn.microsoft.com/library/azure/dn798930.aspx) of [.NET SDK](search-import-data-dotnet.md) push-gegevens aan een index. Er is momenteel geen ondersteuning hulpmiddel voor gegevens via de portal.

Deze methode is flexibeler dan het model halen omdat u documenten, afzonderlijk of in batches uploaden kunt (maximaal 1000 per batch of 16 MB, ongeacht limiet zich het eerste voordoet). Het model push kunt u documenten uploaden naar Azure zoeken ongeacht waar uw gegevens is.

### <a name="pull-data-into-an-index"></a>Gegevens in een index halen

Het model halen een ondersteunde gegevensbron te verkennen en uploadt de gegevens automatisch naar u Azure zoekindex voor u. Door het bijhouden van wijzigingen en verwijdert aan bestaande documenten naast herkent nieuwe documenten, verwijder indexeerfuncties dat u de gegevens in de index actief beheren.

In Azure zoeken, wordt deze mogelijkheid geïmplementeerd via *indexeerfuncties*, momenteel beschikbaar voor [blobopslag (preview)](search-howto-indexing-azure-blob-storage.md), [DocumentDB](http://aka.ms/documentdb-search-indexer), [Azure SQL-database, en SQL Server Azure VMs](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md).

De functionaliteit voor indexering worden weergegeven in de [Portal van Azure](search-import-data-portal.md) ook zoals in de [REST API](https://msdn.microsoft.com/library/azure/dn946891.aspx).
