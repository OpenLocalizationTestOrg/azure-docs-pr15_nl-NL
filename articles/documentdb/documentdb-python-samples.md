<properties 
    pageTitle="Voorbeelden van NoSQL Python voor DocumentDB | Microsoft Azure" 
    description="Zoeken NoSQL Python voorbeelden op github voor algemene taken in DocumentDB, CRUD-bewerkingen voor JSON-documenten op te nemen in NoSQL databases." 
    keywords="Voorbeelden van Python"
    services="documentdb" 
    authors="moderakh" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter="python"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/18/2016" 
    ms.author="moderakh"/>


# <a name="documentdb-python-examples"></a>Voorbeelden van DocumentDB Python

> [AZURE.SELECTOR]
- [.NET-voorbeelden](documentdb-dotnet-samples.md)
- [Voorbeelden van node.js](documentdb-nodejs-samples.md)
- [Voorbeelden van Python](documentdb-python-samples.md)
- [Galerie met voorbeelden van Azure-Code](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Voorbeeldoplossingen die CRUD-bewerkingen en andere algemene bewerkingen op Azure DocumentDB resources uitvoeren worden opgenomen in de bibliotheek van [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) GitHub. In dit artikel bevat:

- Koppelingen naar de taken in elk van de Python voorbeeld project-bestanden. 
- Koppelingen naar de gerelateerde API verwijzingen maken naar inhoud.

**Vereisten voor**

1. U moet een Azure-account in deze voorbeelden Python gebruiken:
    - U kunt [een Azure-account gratis openen](https://azure.microsoft.com/pricing/free-trial/): U tegoeden krijgt u kunt uitproberen betaalde Azure services en zelfs nadat ze gebruikt afgerond kunt u het account en gebruik vrij te geven Azure services, zoals Websites. Uw creditcard nooit brengt, tenzij u expliciet uw instellingen wijzigen en vragen om te betalen.
   - U kunt [Visual Studio abonnee voordelen activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): uw Visual Studio abonnement geeft u tegoeden elke maand die u voor betaalde Azure-services gebruiken kunt.
2. Moet u ook de [Python SDK](documentdb-sdk-python.md). 

    > [AZURE.NOTE] Elk voorbeeld is zelfstandig, zelf ingesteld en opgeschoond. In de voorbeelden actie als zodanig meerdere oproepen naar [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Telkens wanneer dit u uw abonnement doet wordt gefactureerd voor 1 uur gebruik per de laag prestaties van de siteverzameling wordt gemaakt. 

## <a name="database-examples"></a>Voorbeelden van de database

Het bestand [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) van het project [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) ziet hoe u de volgende taken uitvoeren.

Taak | API-verwijzing
--- | ---
[Een database maken](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) | [document_client. CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Query een account voor een database](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) | [document_client. QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Een database op Id lezen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) | [document_client. ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Lijst-databases voor een account](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) | [document_client. ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Een database verwijderen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) | [document_client. DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)

## <a name="collection-examples"></a>Voorbeelden van de siteverzameling 

Het bestand [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) van het project [CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) ziet hoe u de volgende taken uitvoeren.

Taak | API-verwijzing
--- | ---
[Een siteverzameling maken](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) | [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Lees een lijst met alle siteverzamelingen in een database](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) | [document_client. ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Een verzameling op Id ophalen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) | [document_client. ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Ophalen van de laag prestaties van een siteverzameling](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) | [DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Prestaties laag van een siteverzameling wijzigen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) | [document_client. ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Een siteverzameling verwijderen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) | [document_client. DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
