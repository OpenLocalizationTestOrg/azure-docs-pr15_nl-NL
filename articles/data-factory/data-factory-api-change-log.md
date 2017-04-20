<properties 
    pageTitle="Gegevens Factory - .NET API wijzigingenlogboek | Microsoft Azure" 
    description="Een beschrijving van de recente wijzigingen, functie toevoegingen, correcties enzovoort... in een specifieke versie van .NET-API voor de fabriek Azure-gegevens." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="spelluru"/>

# <a name="azure-data-factory---net-api-change-log"></a>Azure gegevens Factory - wijzigingenlogboek voor .NET-API 
In dit artikel vindt u informatie over wijzigingen in een specifieke versie Azure gegevens Factory SDK. U vindt het meest recente NuGet-pakket voor Azure gegevens Factory [hier](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) 

## <a name="version-4110"></a>Versie 4.11.0
Functie toevoegingen:

- De volgende typen gekoppelde services zijn toegevoegd:
    - [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
    - [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
    - [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
- De volgende typen van de gegevensset zijn toegevoegd: 
    - [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
    - [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
- De volgende typen van de kopie-bron zijn toegevoegd:
    - [MongoDbSource](https://msdn.microsoft.com/en-US/library/mt765123.aspx)

## <a name="version-4100"></a>Versie 4.10.0
- De volgende optionele eigenschappen zijn toegevoegd aan de tekstopmaak:
    - [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
    - [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
    - [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
- De volgende typen gekoppelde services zijn toegevoegd:
    - [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
    - [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
- De volgende typen van de gegevensset zijn toegevoegd:
    - [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
- De volgende typen van de kopie-bron zijn toegevoegd:
    - [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
- [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) eigenschap toevoegen aan AzureMLBatchExecutionActivity 
    - Doorgeven van meerdere web service invoer voor een experiment Azure Machine Learning inschakelen


## <a name="version-491"></a>Versie 4.9.1

### <a name="bug-fix"></a>Fout fix

- Afschaffen WebApi gebaseerde verificatie voor [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx).

## <a name="version-490"></a>Versie 4.9.0

### <a name="feature-additions"></a>Functie toevoegingen

- [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) en [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) eigenschappen aan CopyActivity toevoegen. Zie [gefaseerde kopiëren](data-factory-copy-activity-performance.md#staged-copy) voor meer informatie over de functie.


### <a name="bug-fix"></a>Fout fix

- Een grote hoeveelheid [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) -methode, waarmee een exemplaar [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) introduceren.
- [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) en [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) markeren als optioneel in CopySink.

## <a name="version-480"></a>Versie 4.8.0

### <a name="feature-additions"></a>Functie toevoegingen
- De volgende optionele eigenschappen zijn toegevoegd aan het type van de activiteit kopiëren naar het inschakelen van kopie prestaties optimaliseren:
    - [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
    - [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Versie 4.7.0

### <a name="feature-additions"></a>Functie toevoegingen
* Nieuwe StorageFormat type [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) type om bestanden te kopiëren in kolomindeling (ORC) voor geoptimaliseerde rij toegevoegd.
* [AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx) en PolyBaseSettings eigenschappen aan SqlDWSink toevoegen.
    * Kunt u gebruikmaken van PolyBase kunt u gegevens kopiëren naar SQL Data Warehouse.

## <a name="version-461"></a>Versie 4.6.1

### <a name="bug-fixes"></a>Correcties
* HTTP-aanvraag voor het aanbieden van activiteit windows corrigeert.
    * Hiermee verwijdert de naam van de resource-groep en de naam van de factory gegevens uit de aanvraag nettolading.

## <a name="version-460"></a>Versie 4.6.0

### <a name="feature-additions"></a>Functie toevoegingen

- De volgende eigenschappen zijn toegevoegd aan [PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx):
    - [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
    - [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
    - [Gegevenssets](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
- De volgende eigenschappen zijn toegevoegd aan [PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx):
    - [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
- Extra nieuwe [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) Typ [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) type gegevenssets waarvan de gegevens zich in de indeling van JSON definiëren. 

## <a name="version-450"></a>Versie 4.5.0

### <a name="feature-additions"></a>Functie toevoegingen
* Toegevoegde [lijst bewerkingen voor activiteitvenster](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
    * Toegevoegde methoden voor het ophalen van windows activiteit waarop de filters op basis van de Entiteitstypen (dat wil zeggen gegevens factory's, gegevenssets pijpleidingen en activiteiten).
* De volgende typen gekoppelde services zijn toegevoegd: 
    * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* De volgende typen van de gegevensset zijn toegevoegd: 
    * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* De volgende typen van de kopie-bron zijn toegevoegd:  
    * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Versie 4.4.0

### <a name="feature-additions"></a>Functie toevoegingen

- Het volgende gekoppelde servicetype is toegevoegd als gegevensbronnen en sinks voor kopie activiteiten:
    - [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Zie [Azure SA's gekoppelde opslagservice](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) voor algemene informatie en voorbeelden. 

## <a name="version-430"></a>Versie 4.3.0

### <a name="feature-additions"></a>Functie toevoegingen

- De volgende gekoppelde service typen haven zijn toegevoegd als gegevensbronnen voor kopie activiteiten:
    - [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Zie [gegevens uit HDFS met gegevens Factory verplaatsen](data-factory-hdfs-connector.md) voor algemene informatie en voorbeelden. 
    - [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Zie [gegevens uit ODBC-gegevens worden opgeslagen met behulp van Azure gegevens Factory verplaatsen](data-factory-odbc-connector.md) voor algemene informatie en voorbeelden. 

## <a name="version-420"></a>Versie 4.2.0

### <a name="feature-additions"></a>Functie toevoegingen

- De volgende nieuwe activiteitstype is toegevoegd: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Zie voor meer informatie over de activiteit [bijwerken Azure ML modellen met de activiteit van de Resource bijwerken](data-factory-azure-ml-batch-execution-activity.md#updating-azure-ml-models-using-the-update-resource-activity).
- Een nieuwe optionele eigenschap [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) is toegevoegd aan de [AzureMLLinkedService class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx). 
- Eigenschappen [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) en [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) zijn toegevoegd aan de klasse [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) . 
- Toestaan dat de configuratie van de time-out voor client oproepen naar de Data Factory-service. 


## <a name="version-410"></a>Versie 4.1.0

### <a name="feature-additions"></a>Functie toevoegingen
* De volgende typen gekoppelde services zijn toegevoegd: 
    * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
    * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* De volgende activiteitstypen zijn toegevoegd: 
    * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* De volgende typen van de gegevensset zijn toegevoegd: 
    * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* De volgende typen voor de bron- en sink voor kopie activiteit zijn toegevoegd:
    * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
    * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)


## <a name="version-401"></a>Versie 4.0.1

### <a name="breaking-changes"></a>Wijzigingen verbreken
De volgende klassen hebt gekregen. De nieuwe namen zijn de oorspronkelijke namen van klassen voordat 4.0.0 loslaat. 
 
Geef een naam in 4.0.0 | Geef een naam in 4.0.1
:------------ | :------------ 
AzureSqlDataWarehouseDataset | [AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx)
AzureSqlDataset | [AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx)
AzureDataset | [AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx)
OracleDataset | [OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx)
RelationalDataset | [RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx)
SqlServerDataset | [SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx)


## <a name="version-400"></a>Versie 4.0.0

### <a name="breaking-changes"></a>Wijzigingen verbreken



- De volgende klassen/interfaces hebt gekregen.

| De naam van de oude | Nieuwe naam |
| :-------- | :-------- |
| ITableOperations | [IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |  
| Tabel | [Gegevensset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) | 
| TableProperties | [DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) | 
| TableTypeProprerties | [DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters | [DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) | 
| TableCreateOrUpdateResponse | [DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) | 
| TableGetResponse | [DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) | 
| TableListResponse | [DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters | [DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) | 
    

- De **lijst** methoden wordt nu verwisselbaar resultaten retourneren. Als het antwoord een niet-lege **NextLink** eigenschap bevat, moet de clienttoepassing gaan met het ophalen van de volgende pagina totdat alle pagina's worden geretourneerd.  Hier volgt een voorbeeld: 

        PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
        var pipelines = new List<Pipeline>(response.Pipelines);
    
        string nextLink = response.NextLink;
        while (string.IsNullOrEmpty(response.NextLink))
        {
            PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
            pipelines.AddRange(nextResponse.Pipelines);
    
            nextLink = nextResponse.NextLink;
        }
    
- **Lijst** verkooppijplijn API retourneert alleen het overzicht van een pijplijn in plaats van de volledige details. Activiteiten in het overzicht van verkooppijplijn bevat bijvoorbeeld alleen naam en type.

### <a name="feature-additions"></a>Functie toevoegingen
- De klasse [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) ondersteunt twee nieuwe eigenschappen, **SliceIdentifierColumnName** en **SqlWriterCleanupScript**, ter ondersteuning van idempotency is ingeschakeld kopiëren naar Azure SQL Data Warehouse. Zie het artikel [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md) , specifiek, de [om 1](data-factory-azure-sql-data-warehouse-connector.md#mechanism-1) en [2 om](data-factory-azure-sql-data-warehouse-connector.md#mechanism-2) secties, voor meer informatie over deze eigenschappen.

- We nu ondersteuning voor opgeslagen procedure ten opzichte van Azure SQL-Database en Azure SQL Data Warehouse bronnen uitgevoerd als onderdeel van de activiteit kopiëren. De klassen [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) en [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) hebben de volgende eigenschappen: **SqlReaderStoredProcedureName** en **StoredProcedureParameters**. Zie de artikelen [Azure SQL-Database](data-factory-azure-sql-connector.md#sqlsource) en [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) op Azure.com voor meer informatie over deze eigenschappen.  