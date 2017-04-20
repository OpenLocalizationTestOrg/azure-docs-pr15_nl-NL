<properties
pageTitle="Indexering JSON BLOB's met zoekresultaten Azure blob indexering"
description="Indexering JSON BLOB's met zoekresultaten Azure blob indexering"
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
ms.date="07/26/2016"
ms.author="eugenesh" />

# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indexering JSON BLOB's met zoekresultaten Azure blob indexering 

In dit artikel leest het configureren van zoeken Azure blob indexering gestructureerde inhoud extraheren uit BLOB's die JSON bevatten.

## <a name="scenarios"></a>Scenario 's

Standaard parseert [Zoeken Azure blob indexering](search-howto-indexing-azure-blob-storage.md) JSON BLOB's als een hoeveelheid één tekst. Vaak, wilt u de structuur van uw documenten JSON behouden. Bijvoorbeeld, gegeven de JSON-document 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

het is raadzaam deze in een document Azure zoeken met "tekst", "datePublished" en "codes" velden parseren.

Wanneer uw BLOB's een **matrix van JSON-objecten bevatten**, kunt u ook elk element van de matrix te worden van een afzonderlijk document van Azure zoeken. Bijvoorbeeld, gegeven een blob met deze JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

u kunt uw Azure zoekindex met 3 afzonderlijke documenten, elk voorzien van een "-id" en "tekst" velden vullen. 

> [AZURE.IMPORTANT] Deze functie is momenteel in de proefversie. Dit is alleen beschikbaar in de REST API gebruik versie **2015-02-28-Preview**. Neem onthouden, voorbeeld API's zijn bedoeld voor het testen en evaluatie en mag niet worden gebruikt in productieomgevingen. 

## <a name="setting-up-json-indexing"></a>Bij het instellen van JSON indexeren

Index JSON BLOB's, stelt u de `parsingMode` configuratieparameter voor `json` (u kunt elke blob als één document index) of `jsonArray` (als uw BLOB's een JSON-matrix bevatten): 

    {
      "name" : "my-json-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "json" | "jsonArray" } }
    }

Gebruik indien nodig **veldtoewijzingen** als volgt te werk de eigenschappen van het brondocument JSON waarmee de zoekindex doel wordt gevuld.  Dit is hieronder uitvoerig beschreven. 

> [AZURE.IMPORTANT] Wanneer u gebruikt `json` of `jsonArray` parseren van modus, Azure zoeken wordt ervan uitgegaan dat alle BLOB's in uw gegevensbron JSON zal zijn. Als u nodig hebt voor de ondersteuning van een combinatie van JSON en niet-JSON BLOB's in dezelfde gegevensbron, laat het ons weten op [onze UserVoice-site](https://feedback.azure.com/forums/263029-azure-search).

## <a name="using-field-mappings-to-build-search-documents"></a>Veldtoewijzingen gebruiken om u te maken van documenten zoeken 

Op dit moment Azure zoeken kan niet worden geïndexeerd willekeurige JSON documenten rechtstreeks, omdat dit alleen primitieve gegevenstypen, tekenreeksmatrices en GeoJSON wordt verwezen ondersteunt. U kunt echter **veldtoewijzingen** naar kiest u gedeelten van uw document JSON en "til' deze in op het hoogste niveau velden van het document doorzoeken. Voor meer informatie over de basistaken van veld toewijzingen, Zie [Azure zoeken indexering veldtoewijzingen brug de verschillen tussen gegevensbronnen en zoekindexen](search-indexer-field-mappings.md).

Terugkomen in ons voorbeeld JSON document: 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Stel dat u hebt een zoekindex met de volgende velden: `text` van het type Edm.String, `date` van het type Edm.DateTimeOffset, en `tags` van het type Collection(Edm.String). Als u wilt uw JSON toewijzen in de gewenste vorm, gebruikt u de volgende veldtoewijzingen: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
    ]

De bron-veldnamen in de toewijzingen zijn opgegeven met de [Aanwijzer JSON](http://tools.ietf.org/html/rfc6901) -notatie. U start met een slash om te verwijzen naar de hoofdsite van uw document JSON, en inzoomen op de gewenste eigenschap (op een willekeurige niveau geneste) met behulp van doorsturen slash gescheiden pad. 

U kunt ook verwijzen naar afzonderlijke matrixelementen met behulp van een op nul gebaseerde index. Bijvoorbeeld als u het eerste element van de matrix "codes" uit het voorgaande voorbeeld, gebruikt u een veldtoewijzing als volgt:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [AZURE.NOTE] Als de naam van een bron-veld in een veld toewijzing pad naar een eigenschap die niet in JSON voorkomt verwijst, wordt deze toewijzing overgeslagen zonder een fout. Dit gebeurt zodat we documenten met een ander schema ondersteunt (dit is een algemene use-case). Omdat er geen validatie, moet u typefouten in het veld toewijzing specificatie vermijden kunt verrichten. 

Als uw documenten JSON alleen eenvoudige op het hoogste niveau eigenschappen bevatten, moet u mogelijk niet veldtoewijzingen helemaal. Bijvoorbeeld als uw JSON ziet er zo uit, wordt de eigenschappen van het hoogste niveau "tekst", "datePublished" en "codes" rechtstreeks toewijzen aan de corresponderende velden in de zoekindex: 
 
    { 
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13" 
       "tags" : [ "search", "storage", "howto" ]    
    }

## <a name="indexing-nested-json-arrays"></a>Geneste JSON matrices indexeren

Wat gebeurt er als u wilt indexeren van een matrix van JSON-objecten, maar deze matrix ergens is genest binnen het document? U kunt kiezen welke eigenschap bevat het gebruik van de matrix de `documentRoot` configuratie-eigenschap. Stel dat uw BLOB's er als volgt uit: 

    { 
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" }, 
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    } 

Gebruik deze configuratie voor het indexeren van de matrix in de eigenschap "2": 

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }


## <a name="request-examples"></a>Voorbeelden van de aanvraag

Dit alle plaatsen samen en hier ziet u de volledige nettoladingen voorbeelden. 

Gegevensbron: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Indexering:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "useJsonParser" : true } }, 
      "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]
    }

## <a name="help-us-make-azure-search-better"></a>Help ons te verbeteren Azure zoeken

Als u functieverzoeken verzenden of ideeën voor de verbeteringen hebt, neem contact maken met ons op onze [UserVoice-site](https://feedback.azure.com/forums/263029-azure-search/).