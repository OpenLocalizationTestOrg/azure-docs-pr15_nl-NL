<properties
pageTitle="CSV-BLOB's met zoekresultaten Azure blob indexering indexeren | Microsoft Azure"
description="Meer informatie over het CSV-BLOB's met Azure Search indexeren"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="07/12/2016"
ms.author="eugenesh" />

# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>CSV-BLOB's met zoekresultaten Azure blob indexering indexeren 

Standaard [Zoeken Azure blob indexering](search-howto-indexing-azure-blob-storage.md) parseert gescheiden tekst BLOB's als een hoeveelheid één tekst. Echter met BLOB's met de CSV-gegevens, wilt u vaak naar elke regel behandelen in de blob als een afzonderlijk document. Bijvoorbeeld de volgende scheidingstekens tekst gegeven: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

wilt u mogelijk deze parseren in 2 documenten, elk die "-id", "datePublished" en "codes" velden bevat.

In dit artikel leert u hoe u het CSV-BLOB's met een zoekopdracht Azure blob indexering parseren. 

> [AZURE.IMPORTANT] Deze functie is momenteel in de proefversie. Dit is alleen beschikbaar in de REST API gebruik versie **2015-02-28-Preview**. Neem onthouden, voorbeeld API's zijn bedoeld voor het testen en evaluatie en mag niet worden gebruikt in productieomgevingen. 

## <a name="setting-up-csv-indexing"></a>Bij het instellen van het CSV-indexeren

CSV-BLOB's indexeren, maken of bijwerken van de definitie van een indexering met de `delimitedText` parseren van modus:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Voor meer informatie over de API voor indexering maken, raadpleegt u [Indexering maken](search-api-indexers-2015-02-28-preview.md#create-indexer).

`firstLineContainsHeaders`Geeft aan dat de eerste regel (niet-lege) van elke blob kopteksten bevat.
Als BLOB's niet een eerste koptekstregel bevatten, kan de koppen moeten worden opgegeven in de configuratie indexering: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Op dit moment wordt alleen de UTF-8-codering ondersteund. Ook alleen de komma `','` teken wordt ondersteund als scheidingsteken. Als u ondersteuning nodig voor andere coderingen of scheidingstekens, laat het ons weten op [onze UserVoice-site](https://feedback.azure.com/forums/263029-azure-search).

> [AZURE.IMPORTANT] Wanneer u de tekst met scheidingstekens parseren van modus gebruikt, Azure zoeken wordt ervan uitgegaan dat alle BLOB's in uw gegevensbron zal CSV. Als u nodig hebt voor de ondersteuning van een combinatie van CSV- en niet-CSV-BLOB's in dezelfde gegevensbron, laat het ons weten op [onze UserVoice-site](https://feedback.azure.com/forums/263029-azure-search).

## <a name="request-examples"></a>Voorbeelden van de aanvraag

Dit alle plaatsen samen en hier ziet u de volledige nettolading voorbeelden. 

Gegevensbron: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexering:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Help ons te verbeteren Azure zoeken

Als u functieverzoeken verzenden of ideeën voor de verbeteringen hebt, neem contact maken met ons op onze [UserVoice-site](https://feedback.azure.com/forums/263029-azure-search/).