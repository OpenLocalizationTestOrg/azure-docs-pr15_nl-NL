<properties
    pageTitle="Voorbeelden van NoSQL Node.js voor DocumentDB | Microsoft Azure"
    description="Zoeken NoSQL Node.js voorbeelden op github voor algemene taken in DocumentDB, CRUD-bewerkingen voor JSON-documenten op te nemen in NoSQL databases."
    keywords="Voorbeelden van node.js"
    services="documentdb"
    authors="moderakh"
    manager="jhubbard"
    editor="monicar"
    documentationCenter="nodejs"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="moderakh"/>


# <a name="documentdb-nodejs-examples"></a>Voorbeelden van DocumentDB Node.js

> [AZURE.SELECTOR]
- [.NET-voorbeelden](documentdb-dotnet-samples.md)
- [Voorbeelden van node.js](documentdb-nodejs-samples.md)
- [Voorbeelden van Python](documentdb-python-samples.md)
- [Galerie met voorbeelden van Azure-Code](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Voorbeeldoplossingen die CRUD-bewerkingen en andere algemene bewerkingen op Azure DocumentDB resources uitvoeren worden opgenomen in de bibliotheek van [azure-documentdb-nodejs](https://github.com/Azure/azure-documentdb-node/tree/master/samples) GitHub. In dit artikel bevat:

- Koppelingen naar de taken in elk van de Node.js voorbeeld project-bestanden.
- Koppelingen naar de gerelateerde API verwijzingen maken naar inhoud.

**Vereisten voor**

1. U moet een Azure-account in deze voorbeelden Node.js gebruiken:
    - U kunt [een Azure-account gratis openen](https://azure.microsoft.com/pricing/free-trial/): U tegoeden krijgt u kunt uitproberen betaalde Azure services en zelfs nadat ze gebruikt afgerond kunt u het account en gebruik vrij te geven Azure services, zoals Websites. Uw creditcard nooit brengt, tenzij u expliciet uw instellingen wijzigen en vragen om te betalen.
   - U kunt [Visual Studio abonnee voordelen activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): uw Visual Studio abonnement geeft u tegoeden elke maand die u voor betaalde Azure-services gebruiken kunt.
2. Moet u ook de [Node.js SDK](documentdb-sdk-node.md).

    > [AZURE.NOTE] Elk voorbeeld is zelfstandig, zelf ingesteld en opgeschoond. In de voorbeelden actie als zodanig meerdere oproepen naar [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection). Telkens wanneer dit u uw abonnement doet wordt gefactureerd voor 1 uur gebruik per de laag prestaties van de siteverzameling wordt gemaakt.

## <a name="database-examples"></a>Voorbeelden van de database

Het bestand [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DatabaseManagement/app.js) van het project [DatabaseManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DatabaseManagement) ziet hoe u de volgende taken uitvoeren.

Taak | API-verwijzing
--- | ---
[Een database maken](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L121-L131) | [DocumentClient.createDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDatabase)
[Query een account voor een database](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L146-L171) | [DocumentClient.queryDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDatabases)
[Een database op Id lezen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L89-L99) | [DocumentClient.readDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase)
[Lijst-databases voor een account](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L111-L119) | [DocumentClient.readDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase)
[Een database verwijderen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L133-L144) | [DocumentClient.deleteDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDatabase)

## <a name="collection-examples"></a>Voorbeelden van de siteverzameling

Het bestand [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/CollectionManagement/app.js) van het project [CollectionManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/CollectionManagement) ziet hoe u de volgende taken uitvoeren.

Taak | API-verwijzing
--- | ---
[Een siteverzameling maken](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L97-L118) | [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)
[Lees een lijst met alle siteverzamelingen in een database](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L120-L130) | [DocumentClient.readCollections](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollections)
[Een siteverzameling kunt krijgen door te _self](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L132-L141) | [DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection)
[Een verzameling op Id ophalen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L143-L156) | [DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection)
[Ophalen van de laag prestaties van een siteverzameling](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L158-L186) | [DocumentQueryable.queryOffers](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryOffers)
[Prestaties laag van een siteverzameling wijzigen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L188-L202) | [DocumentClient.replaceOffer](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceOffer)
[Een siteverzameling verwijderen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L204-L215) | [DocumentClient.deleteCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteCollection)

## <a name="document-examples"></a>Voorbeelden van document

Het bestand [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DocumentManagement/app.js) van het project [DocumentManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DocumentManagement) ziet hoe u de volgende taken uitvoeren.

Taak | API-verwijzing
--- | ---
[Documenten maken](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L153-L177) | [DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDocument)
[Lezen van het document feed voor een siteverzameling](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L179-L189) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)
[Een document op ID lezen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L191-L201) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)
[Document lezen alleen als het document is gewijzigd](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L79-L107) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Query voor documenten](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L82-L110) | [DocumentClient.queryDocuments](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocuments)
[Een document vervangen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L112-L119) |  [DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)
[Document vervangen door voorwaardelijke ETag controleren](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L147-L164) |  [DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Een document verwijderen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L122-L133) | [DocumentClient.deleteDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDocument)

## <a name="indexing-examples"></a>Voorbeelden indexeren

Het bestand [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/IndexManagement/app.js) van het project [IndexManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/IndexManagement) ziet hoe u de volgende taken uitvoeren.

Taak | API-verwijzing
--- | ---
Maken van een siteverzameling met standaard indexeren | [DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html)
[Handmatig een specifiek document index](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L185-L238) | [indexingDirective: 'Voeg'](http://azure.github.io/azure-documentdb-node/global.html#indexingDirective)
[Handmatig een specifiek document uitsluiten van de index](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L120-L183) | [RequestOptions.indexingDirective](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Gebruik fikse indexeren voor bulksgewijs importeren of dik verzamelingen lezen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L240-L269) | [IndexingMode.Lazy](http://azure.github.io/azure-documentdb-node/global.html#IndexingMode)
[Specifieke paden van een document opnemen in het indexeren](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L433-L444) | [IndexingPolicy.IncludedPaths](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy)
[Bepaalde paden uitsluiten van het indexeren](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L427-L450) | [ExcludedPath](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy)
[Een scan toestaan op het pad van een tekenreeks tijdens een bereik](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L271-L347)| [ExcludedPath.EnableScanInQuery](http://azure.github.io/azure-documentdb-node/global.html#FeedOptions)
[Een bereik-index maken voor het pad van een tekenreeks](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L349-L425) | [DocumentClient.queryDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocument)
[Een siteverzameling met standaard indexPolicy maken en dit online bijwerken](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L519-L614) | [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)<br> [DocumentClient.replaceCollection#replaceCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html)

Zie voor meer informatie over het indexeren, [DocumentDB indexing beleidsregels](documentdb-indexing-policies.md).

## <a name="server-side-programming-examples"></a>Aan de clientzijde programming voorbeelden

Het bestand [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/ServerSideScripts/app.js) van het project [ServerSideScripts](https://github.com/Azure/azure-documentdb-node/tree/master/samples/ServerSideScripts) ziet hoe u de volgende taken uitvoeren.

Taak | API-verwijzing
--- | ---
[Een opgeslagen procedure maken](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L44-L71) | [DocumentClient.createStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createStoredProcedure)
[Een opgeslagen procedure uitvoeren](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L73-L90) | [DocumentClient.executeStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#executeStoredProcedure)

Zie voor meer informatie over servers hoeft te programmeren, [DocumentDB serverzijde programmeren: opgeslagen procedures, databasetriggers en UDF's](documentdb-programming.md).

## <a name="partitioning-examples"></a>Voorbeelden partitioneren

Het bestand [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/Partitioning/app.js) van het project [partitionering](https://github.com/Azure/azure-documentdb-node/tree/master/samples/Partitioning) ziet hoe u de volgende taken uitvoeren.

Taak | API-verwijzing
--- | ---
[Een HashPartitionResolver gebruiken](https://github.com/Azure/azure-documentdb-node/blob/ce0fc3c4e70b0279091a1e03620a668d93a14fc2/samples/Partitioning/app.js#L53-L103) | [HashPartitionResolver](http://azure.github.io/azure-documentdb-node/HashPartitionResolver.html)

Zie voor meer informatie over gegevens in DocumentDB partitioneren, [gegevens in DocumentDB Partition en schaal](documentdb-partition-data.md).
