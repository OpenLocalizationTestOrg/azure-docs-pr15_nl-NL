<properties
pageTitle="Azure-tabelopslag met Azure Search indexeren"
description="Leer hoe u de gegevens die zijn opgeslagen in tabellen Azure met Azure Search indexeren"
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
ms.date="08/16/2016"
ms.author="eugenesh" />

# <a name="indexing-azure-table-storage-with-azure-search"></a>Azure-tabelopslag met Azure Search indexeren

In dit artikel leest hoe u Azure zoeken gebruiken om gegevens te indexeren die zijn opgeslagen in Azure Table Storage. De nieuwe tabel indexering voor Azure zoeken kunt u dit proces snel en naadloze. 

> [AZURE.IMPORTANT] Deze functie is momenteel in de proefversie. Dit is alleen beschikbaar in de REST API gebruik versie **2015-02-28-Preview** en in versie 2.0-voorbeeld van de .NET SDK. Neem onthouden, voorbeeld API's zijn bedoeld voor het testen en evaluatie en mag niet worden gebruikt in productieomgevingen.

## <a name="setting-up-azure-table-indexing"></a>Bij het instellen van Azure tabel indexeren

Als u wilt instellen en configureren van een indexering Azure tabel, kunt u de Azure zoeken REST API maken en beheren van **indexeerfuncties** en **gegevensbronnen** , zoals wordt beschreven in [indexering bewerkingen](https://msdn.microsoft.com/library/azure/dn946891.aspx). U kunt ook de [Preview-versie 2.0-versie](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) van de .NET SDK gebruiken. In de toekomst ondersteuning voor indexering van de tabel, worden toegevoegd aan de Azure-Portal.

Welke gegevens worden indexeert, referenties die nodig zijn voor toegang tot de gegevens en beleidsregels waarmee Azure zoeken efficiënt identificeren wijzigingen in de gegevens (nieuwe, gewijzigd of verwijderd rijen) Hiermee geeft u een gegevensbron.

Een indexering gegevens ophaalt uit een gegevensbron en laadt deze in de zoekindex van een doel.

Voor het instellen van de tabel indexeren:

1. Een gegevensbron maken
    - Stel de `type` -parameter voor`azuretable`
    - In de verbindingsreeks van uw opslag-account als doorgeven de `credentials.connectionString` parameter
    - Geef de gebruik van de tabel de naam van de `container.name` parameter
    - Geef desgewenst een query met de `container.query` parameter. Gebruik waar mogelijk een filter op PartitionKey voor de beste prestaties; er een andere query resulteert in een scannen van de volledige tabel, wat leiden slechte prestaties voor grote tabellen tot kan.
2. Maak een zoekindex met het schema die overeenkomt met de kolommen in de tabel die u wilt indexeren. 
3. De indexering door uw gegevensbron aan de zoekindex verbinding te maken.

### <a name="create-data-source"></a>Gegevensbron maken

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Zie voor meer informatie over de gegevensbron maken API [Gegevensbron maken](search-api-indexers-2015-02-28-preview.md#create-data-source).

### <a name="create-index"></a>Indexen maken 

    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-target-index",
        "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
        ]
    }

Zie voor meer informatie over de Index maken API [Index maken](https://msdn.microsoft.com/library/dn798941.aspx)

### <a name="create-indexer"></a>Indexering maken 

Ten slotte de indexering dat verwijst naar de gegevensbron en de doel-index maken. Bijvoorbeeld:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Voor meer informatie over de API voor indexering maken, raadpleegt u [Indexering maken](search-api-indexers-2015-02-28-preview.md#create-indexer).

Die wordt gevormd door alle klaar is Kees - tevreden indexeren!

## <a name="dealing-with-different-field-names"></a>Omgaan met verschillende veldnamen

Vaak zullen de veldnamen in de bestaande index afwijken van de namen van de eigenschappen in de tabel. **Veldtoewijzingen** kunt u de namen van de eigenschappen van de tabel naar de veldnamen in de zoekindex toewijzen. Meer informatie over het toewijzen van een veld, Zie [Azure zoeken indexering veldtoewijzingen brug de verschillen tussen gegevensbronnen en zoekindexen](search-indexer-field-mappings.md).

## <a name="handling-document-keys"></a>Verwerking van document toetsen

In Azure zoeken aanduiding de toets document een unieke voor een document. Elke zoekindex moet precies één sleutelveld van het type `Edm.String`. Het veld sleutel is vereist voor elk document dat wordt toegevoegd aan de index (in feite dit is het enige vereist veld).

Aangezien tabelrijen een samengestelde sleutel hebben, Azure zoeken genereert een synthetische veld `Key` die is een samenvoeging van partition sleutel en rij sleutelwaarden. Als een rij van het PartitionKey is bijvoorbeeld `PK1` en RowKey is `RK1`, klikt u vervolgens `Key` veldwaarde worden `PK1RK1`. 

> [AZURE.NOTE] De `Key` waarde tekens die ongeldig in document toetsen, zoals streepjes zijn kan bevatten. U kunt ongeldige tekens verwerken met behulp van de `base64Encode` [veld toewijzing functie](search-indexer-field-mappings.md#base64EncodeFunction). Als u dit doet, moet u ook gebruiken URL geschikt Base64-codering wanneer het doorgeven van document toetsen in API zoals opzoeken belt.

## <a name="incremental-indexing-and-deletion-detection"></a>Incrementele detectie van indexering en verwijderen
 
Wanneer u een tabel indexering ingesteld om uit te voeren op een planning, deze alleen nieuwe of bijgewerkte rijen, afhankelijk van de van een rij reindexes `Timestamp` waarde. U hoeft niet te geven van een wijziging detectie beleid: incrementele indexeren automatisch voor u is ingeschakeld. 

Om aan te geven dat bepaalde documenten moeten worden verwijderd uit de index, kunt u een strategie vloeiende verwijderen – in plaats van een rij verwijderen, moet u een eigenschap om aan te geven dat deze wordt verwijderd en een vloeiende verwijdering detectie beleid op de gegevensbron instellen toevoegen. Het beleid hieronder wordt bijvoorbeeld dat een rij is verwijderd als er een eigenschap `IsDeleted` met de waarde `"true"`: 

    PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]
    
    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "query" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   


## <a name="help-us-make-azure-search-better"></a>Help ons te verbeteren Azure zoeken

Als u functieverzoeken verzenden of ideeën voor de verbeteringen hebt, neem contact maken met ons op onze [UserVoice-site](https://feedback.azure.com/forums/263029-azure-search/).