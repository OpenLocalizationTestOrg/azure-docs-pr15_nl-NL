<properties
    pageTitle="Alle onderwerpen voor Data Factory-service | Microsoft Azure"
    description="Tabel met alle onderwerpen voor de Azure-service met gegevens Factory die aanwezig op http://azure.microsoft.com/documentation/articles/, titel en beschrijving de naam."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="data-factory"
    ms.workload="data-factory"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="spelluru"/>


# <a name="all-topics-for-azure-data-factory-service"></a>Alle onderwerpen voor Azure gegevens Factory-service

In dit onderwerp bevat elke onderwerp die van toepassing rechtstreeks naar de **Data Factory** -service van Azure is. U kunt dit webpagina voor trefwoorden zoeken met behulp van **Ctrl + F**, om de onderwerpen huidige belangrijke te zoeken.




## <a name="new"></a>Nieuw

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 1 | [Verplaats gegevens van Amazon Redshift, met Azure gegevens Factory](data-factory-amazon-redshift-connector.md) | Meer informatie over het verplaatsen van gegevens van Amazon Redshift met Azure gegevens Factory. |
| 2 | [Verplaats gegevens van Amazon eenvoudige Storage-Service via Azure gegevens Factory](data-factory-amazon-simple-storage-service-connector.md) | Meer informatie over het verplaatsen van gegevens uit de Amazon eenvoudige Storage-Service (S3) met behulp van Azure gegevens Factory. |
| 3 | [Azure gegevens Factory - Wizard kopiëren](data-factory-azure-copy-wizard.md) | Meer informatie over het gebruik van de Wizard gegevens Factory Azure kopiëren gegevens kopiëren van ondersteunde gegevensbronnen naar sinks. |
| 4 | [Zelfstudie: Uw eerste Azure gegevens factory met gegevens Factory REST API maken](data-factory-build-your-first-pipeline-using-rest-api.md) | In deze zelfstudie maakt u een voorbeeld Azure gegevens Factory pijplijn gegevens Factory REST API gebruiken. |
| 5 | [Zelfstudie: Een pijplijn maken met kopie activiteit .NET-API gebruiken](data-factory-copy-activity-tutorial-using-dotnet-api.md) | In deze zelfstudie maakt u een verkooppijplijn Factory van Azure-gegevens met de activiteit in een kopie door .NET-API gebruiken. |
| 6 | [Zelfstudie: Een pijplijn maken met kopie activiteit REST API gebruiken](data-factory-copy-activity-tutorial-using-rest-api.md) | In deze zelfstudie maakt u een verkooppijplijn Factory van Azure-gegevens met de activiteit in een kopie met behulp van de REST API. |
| 7 | [Wizard gegevens Factory-kopiëren](data-factory-copy-wizard.md) | Meer informatie over het gebruik van de Wizard Factory kopie gegevens kopiëren van ondersteunde gegevensbronnen naar sinks. |
| 8 | [Data Management Gateway](data-factory-data-management-gateway.md) | Een data gateway instellen om gegevens te verplaatsen tussen de on-premises en de cloud. Data Management Gateway in fabriek van Azure-gegevens gebruiken om uw gegevens te verplaatsen. |
| 9 | [Verplaats gegevens van een on-premises implementatie Cassandra-database met behulp van Azure gegevens Factory](data-factory-onprem-cassandra-connector.md) | Meer informatie over het verplaatsen van gegevens uit een on-premises implementatie Cassandra-database met Azure gegevens Factory. |
| 10 | [Gegevens uit MongoDB met Azure gegevens Factory verplaatsen](data-factory-on-premises-mongodb-connector.md) | Meer informatie over het verplaatsen van gegevens uit een MongoDB-database via van Azure gegevens Factory. |
| 11 | [Verplaats gegevens van Salesforce met behulp van Azure gegevens Factory](data-factory-salesforce-connector.md) | Meer informatie over het verplaatsen van gegevens van Salesforce met behulp van Azure gegevens Factory. |


## <a name="updated-articles-data-factory"></a>Bijgewerkte artikelen, Data Factory

In deze sectie worden artikelen die onlangs zijn bijgewerkt, waar het bijwerken is groot of aanzienlijk. Voor elke bijgewerkte-artikel een ruw fragment van tekst bij de toegevoegde korting weergegeven. De artikelen zijn bijgewerkt binnen het datumbereik van **2016-08-22** in **2016-10-05**.

| &nbsp; | Artikel | Bijgewerkte tekst, fragment | Wanneer worden bijgewerkt |
| --: | :-- | :-- | :-- |
| 12 | [Azure gegevens Factory - wijzigingenlogboek voor .NET-API](data-factory-api-change-log.md) | In dit artikel vindt u informatie over wijzigingen in een specifieke versie Azure gegevens Factory SDK. U vindt de meest recente NuGet-pakket voor Azure gegevens Factory hier (https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) **versie 4.11.0** functie toevoegingen: / de volgende typen gekoppelde services zijn toegevoegd: - OnPremisesMongoDbLinkedService (https://msdn.microsoft.com/library/mt765129.aspx) - AmazonRedshiftLinkedService (https://msdn.microsoft.com/library/mt765121.aspx) - AwsAccessKeyLinkedService (https://msdn.microsoft.com/library/mt765144.aspx) / de volgende typen van de gegevensset zijn toegevoegd: - MongoDbCollectionDataset (https://msdn.microsoft.com/library/mt765145.aspx) - AmazonS3Dataset (https://msdn.microsoft.com/library/mt765112.aspx) / de volgende typen van de kopie-bron zijn toegevoegd :-MongoDbSource (https://msdn.microsoft.com/en-US/library/mt765123.aspx) **versie 4.10.0** / de volgende optionele eigenschappen zijn toegevoegd aan de tekstopmaak:-ski's | 2016-09-22 |
| 13 | [Gegevens verplaatsen naar en vanuit Azure Blob Azure gegevens Factory gebruiken](data-factory-azure-blob-connector.md) |  / copyBehavior / definieert het gedrag van kopiëren als de bron BlobSource of bestandssysteem is.  / **PreserveHierarchy:** gewoon overgenomen in de bestandshiërarchie in de doelmap. Het relatieve pad van het bronbestand naar bronmap is gelijk aan het relatieve pad van het doelbestand naar doelmap... br /... br /. **FlattenHierarchy:** alle bestanden uit de bronmap zijn opgeslagen in het eerste niveau van de doelmap. De doelbestanden hebben automatisch gegenereerde naam. .BR /... br /. **MergeFiles: (standaard)** samengevoegd alle bestanden uit de bronmap naar een bestand. Als de naam van het bestand/Blob is opgegeven, zou de samengevoegde bestandsnaam de naam van de opgegeven; anders zou zijn automatisch gegenereerde bestandsnaam.  / Niet / **BlobSource** ondersteunt ook deze twee eigenschappen voor compatibiliteit met eerdere versies. / **treatEmptyAsNull**: Hiermee bepaalt u of lege tekenreeks behandelen als null-waarde. / **skipHeaderLineCount** - Hiermee geeft u het aantal regels moeten worden overgeslagen. Dit geldt alleen wanneer invoer gegevensset gebruikt tekstopmaak. Daarnaast ondersteunt **BlobSink** th | 2016-09-28 |
| 14 | [Bekijk pijpleidingen met Azure Machine Learning-activiteiten maken](data-factory-azure-ml-batch-execution-activity.md) | **Meerdere ingangen is vereist** Als de webservice meerdere ingangen duurt, gebruikt u de eigenschap **webServiceInputs** in plaats van **webServiceInput**. Gegevenssets waarnaar wordt verwezen door de **webServiceInputs** moeten ook worden opgenomen in de activiteit **invoeritems**. In uw experiment Azure ML hebben web service invoer en uitvoerpoorten en globale parameters standaardnamen ("input1", "input2") die u kunt aanpassen. De namen die u voor webServiceInputs, webServiceOutputs en globalParameters instellingen gebruikt moeten precies overeenkomen met de namen in de experimenten. U kunt de steekproef verzoek nettolading bekijken op de pagina Batch Execution Help voor uw eindpunt Azure ML om te controleren of de verwachte toewijzing.    {"naam": "PredictivePipeline", "eigenschappen": {"Beschrijving": 'Gebruik AzureML model', 'activiteiten': {"naam": "MLActivity", "type": "AzureMLBatchExecution", "Beschrijving": "tekstvoorspelling analyse op batch input", "ingangen": {"naam": "inputDataset1"}, {"naam": "inputDatase | 2016-09-13 |
| 15 | [Activiteit prestaties en afstemmen handleiding kopiëren](data-factory-copy-activity-performance.md) | 1. **een basislijn maken**. Tijdens de ontwikkelingsfase, test u uw verkooppijplijn met behulp van kopie activiteit ten opzichte van een steekproef vertegenwoordiger gegevens. U kunt de gegevens fabriek segmenteren model (gegevens-factory-planning-en-execution.md time-series-datasets-and-data-slices) gebruiken om de hoeveelheid gegevens waarmee die u werkt te beperken.  Tijd en prestatiekenmerken verzamelen met behulp van de **controle- en de App Management**. Kies **Monitor & beheren** op uw gegevens Factory-startpagina. Kies de **uitvoer gegevensset**in de boomstructuur. Kies in de lijst **Activiteit Windows** de kopie activiteit uitvoeren. **Activiteit Windows** bevat de kopie activiteitsduur en de grootte van de gegevens die wordt gekopieerd. De doorvoer wordt weergegeven in de **Activiteit venster Verkenner**. Meer informatie over de app, Zie Monitor en Azure gegevens Factory pijpleidingen beheren met behulp van de controle- en Management App (gegevens-factory-monitor-beheren-app.md).  ! Activiteit details (./media/data-factory-copy-activity-performance/mmapp-activity-run-details.pn uitvoeren | 2016-09-27 |
| 16 | [Zelfstudie: Een pijplijn maken met kopie activiteit gebruik van Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) |   Houd rekening met de volgende punten:-gegevensset **type** is ingesteld op **AzureBlob**.     - **linkedServiceName** is ingesteld op **AzureStorageLinkedService**. U kunt deze gekoppelde service gemaakt in stap 2.     - **mappad** is ingesteld op de container **adftutorial** . U kunt ook de naam van een blob in de map met de eigenschap **bestandsnaam** opgeven. Aangezien u niet de naam van de label opgeeft, worden gegevens uit alle BLOB's in de container wordt beschouwd als een invoergegevens.    -indeling, **type** is ingesteld op **tekstopmaak** - er twee velden in de tekst bestand ΓÇô **Voornaam** en **Achternaam** ΓÇô gescheiden door een kommateken (**columnDelimiter**) - de **beschikbaarheid van** is ingesteld op **elk uur** (**frequentie** is ingesteld op **uur** en **interval** is ingesteld op **1**). Daarom Data Factory Hiermee wordt gezocht naar invoergegevens per uur in de hoofdmap van blob container (**adftutorial**) dat u hebt opgegeven.  Als u een **bestandsnaam** voor een gegevensset **invoer** niet opgeeft, zijn alle bestanden/BLOB's uit de invoer map (**mappad**) Zorg | 2016-09-29 |
| 17 | [Maken, controleren en beheren van Azure gegevens factory's met gegevens Factory .NET SDK](data-factory-create-data-factories-programmatically.md) | Houd rekening met de toepassings-ID en het wachtwoord (geheim client) en deze gebruiken in de stapsgewijze instructies. **Tenant id's en krijgen Azure-abonnement** Als u beschikt niet over de nieuwste versie van PowerShell op uw computer zijn geïnstalleerd, volgt u de instructies in het installeren en configureren van Azure PowerShell (.. / powershell-installatie-configure.md) artikel om het te installeren. 1. Azure PowerShell Start en voer de volgende opdracht 2. Voer de volgende opdracht en voer de gebruikersnaam en wachtwoord waarmee u zich aanmelden bij de portal van Azure.         Login-AzureRmAccount als die u slechts één Azure abonnement dat is gekoppeld aan dit account hebt, hoeft u niet de volgende twee stappen uitvoeren. 3. Voer de volgende opdracht om te bekijken van alle abonnementen voor dit account.       Get-AzureRmSubscription 4. Voer de volgende opdracht om te selecteren van het abonnement waaraan u wilt werken. **NameOfAzureSubscription** vervangen door de naam van uw Azure-abonnement.       Get-AzureRmSubscription - SubscriptionName NameOfAzureSubscription / Set-AzureRmCo | 2016-09-14 |
| 18 | [Pijpleidingen en activiteiten in Azure gegevens fabriek](data-factory-create-pipelines.md) |       "starten": "2016-07-12T00:00:00Z", "einde": "2016-07-13T00:00:00Z"}} Houd rekening met de volgende punten: / In de sectie activiteiten is slechts één activiteit waarvan het **type** is ingesteld op **kopiëren**. / Invoer voor de activiteit is ingesteld op **InputDataset** en uitvoer voor de activiteit is ingesteld op **OutputDataset**. / In de sectie **typeProperties** **BlobSource** is opgegeven als het brontype en **SqlSink** is opgegeven als het type sink. Zie voor een volledige stapsgewijze instructies voor het maken van deze verkooppijplijn, zelfstudie: gegevens uit Blob Storage kopiëren met SQL-Database (data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). **Voorbeeld transformatie verkooppijplijn** In het volgende voorbeeld pijplijn is er een activiteit van het type **HDInsightHive** in de sectie **activiteiten** . In dit voorbeeld wordt met de activiteit HDInsight-component (gegevens-factory-component-activity.md) gegevens in een Azure-blobopslag omgezet door een component script-bestand in een cluster Azure HDInsight Hadoop uit te voeren.  {"naam": "TransformPipeline", "p | 2016-09-27 |
| 19 | [Transformeer gegevens in fabriek van Azure-gegevens](data-factory-data-transformation-activities.md) | Gegevens Factory ondersteunt de volgende gegevens transformatie activiteiten die kunnen worden toegevoegd aan pijpleidingen (gegevens-factory-maken-pipelines.md) ofwel afzonderlijk of reeksen met een andere activiteit. .  AZURE. Zie maken een pijplijn met component-transformatie (data-factory-build-your-first-pipeline.md) artikel NOTITIE voor stapsgewijze instructies met stapsgewijze instructies. **HDInsight component activiteit** De HDInsight component activiteit in een pijplijn Data Factory voert component query's op uw eigen of op aanvraag op basis van Windows/Linux HDInsight cluster. Zie Component activiteit (gegevens-factory-component-activity.md)-artikel voor meer informatie over deze activiteit. **HDInsight varken activiteit** De HDInsight varken activiteit in een pijplijn Data Factory voert varken query's op uw eigen of op aanvraag op basis van Windows/Linux HDInsight cluster. Zie varken activiteit (gegevens-factory-varken-activity.md)-artikel voor meer informatie over deze activiteit. **HDInsight MapReduce activiteit** De HDInsight MapReduce activiteit in een pijplijn Data Factory MapReduce-programma's op y wordt uitgevoerd | 2016-09-26 |
| 20 | [Gegevens Factory plannen en uitvoeren](data-factory-scheduling-and-execution.md) | CopyActivity2 gebeurt alleen als de CopyActivity1 met succes is uitgevoerd en Dataset2 beschikbaar is. Hier ziet u de steekproef pijplijn JSON: {"naam": "ChainActivities", "eigenschappen": {"Beschrijving": 'Activiteiten uitvoeren in de reeks', 'activiteiten': {"type": "Kopiëren", "typeProperties": {"bron": {"type": "BlobSource"}, 'sink': {"type": "BlobSink", "copyBehavior": "PreserveHierarchy", "writeBatchSize": 0, "writeBatchTimeout": "00: 00:00"}}, "invoer": {"naam": "Dataset1"}, "uitvoer": {"naam": "Dataset2"}, "beleid": {"time-out": "01: 00:00"}, "scheduler": {"frequentie": 'Uur', 'interval': 1}, "naam": "CopyFromBlob1ToBlob2", "Beschrijving": "Gegevens uit een blob kopiëren naar een andere"}, {"type": "Kopiëren", "typeProperties": {"bron": {"type": "BlobSource"}, "sink" : {"type": "BlobSink", "writeBatchSize": 0, "writeBatchTimeout": "00: 00:00"}}, "in | 2016-09-28 |





## <a name="tutorials"></a>Zelfstudies

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 21 | [Zelfstudie: Uw eerste verkooppijplijn om gegevens te verwerken met Hadoop cluster maken](data-factory-build-your-first-pipeline.md) | Deze zelfstudie Azure gegevens Factory ziet u hoe u maken en plannen van een fabriek gegevens waarmee gegevens met behulp van component script op een Hadoop-cluster worden verwerkt. |
| 22 | [Zelfstudie: Uw eerste Azure gegevens factory met resourcemanager Azure-sjabloon maken](data-factory-build-your-first-pipeline-using-arm.md) | In deze zelfstudie kunt u een voorbeeld Azure gegevens Factory pijplijn met een sjabloon van Azure resourcemanager maken. |
| 23 | [Zelfstudie: Uw eerste Azure gegevens factory met behulp van Azure portal maken](data-factory-build-your-first-pipeline-using-editor.md) | In deze zelfstudie maakt u een voorbeeld Azure gegevens Factory pijplijn met gegevens Factory-Editor in de portal van Azure. |
| 24 | [Zelfstudie: Uw eerste Azure gegevens factory via Azure PowerShell maken](data-factory-build-your-first-pipeline-using-powershell.md) | In deze zelfstudie maakt u een voorbeeld Azure gegevens Factory pijplijn via Azure PowerShell. |
| 25 | [Zelfstudie: Uw eerste Azure opbouwen gegevens factory met Microsoft Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) | In deze zelfstudie maakt u een voorbeeld Azure gegevens Factory pijplijn gebruik van Visual Studio. |
| 26 | [Zelfstudie: Een pijplijn maken met kopie activiteit met behulp van Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) | In deze zelfstudie maakt u een verkooppijplijn Factory van Azure-gegevens met de activiteit in een kopie met behulp van de gegevens Factory-Editor in de portal van Azure. |
| 27 | [Zelfstudie: Een pijplijn maken met kopie activiteit via Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) | In deze zelfstudie maakt u een verkooppijplijn Factory van Azure-gegevens met de activiteit in een kopie met behulp van Azure PowerShell. |
| 28 | [Zelfstudie: Een pijplijn maken met kopie activiteit gebruik van Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) | In deze zelfstudie maakt u een verkooppijplijn Factory van Azure-gegevens met de activiteit in een kopie met behulp van Visual Studio. |
| 29 | [Gegevens uit Blob Storage met SQL-Database met Data Factory kopiëren](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | Deze zelfstudie ziet u hoe kopie activiteit in een pijplijn Azure gegevens Factory gebruiken om gegevens te kopiëren uit Blob storage met SQL-database. |
| 30 | [Zelfstudie: Een pijplijn maken met kopie activiteit met gegevens Factory kopie Wizard](data-factory-copy-data-wizard-tutorial.md) | In deze zelfstudie maken u een verkooppijplijn Factory van Azure-gegevens met een activiteit kopiëren met behulp van de Wizard kopiëren wordt ondersteund door gegevens Factory |



## <a name="data-movement"></a>Gegevens verplaatsen

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 31 | [Gegevens verplaatsen naar en vanuit Azure Blob Azure gegevens Factory gebruiken](data-factory-azure-blob-connector.md) | Leer hoe u blob-gegevens kopiëren in fabriek van Azure-gegevens. Kunt u ons voorbeeld gebruiken: gegevens kopiëren naar en vanuit Azure-blobopslag en Azure SQL-Database. |
| 32 | [Gegevens verplaatsen naar en vanuit Azure Lake gegevensopslag Azure gegevens Factory gebruiken](data-factory-azure-datalake-connector.md) | Informatie over het verplaatsen van gegevens van Azure Lake gegevensopslag Azure gegevens Factory gebruiken |
| 33 | [Gegevens verplaatsen naar en vanuit DocumentDB Azure gegevens Factory gebruiken](data-factory-azure-documentdb-connector.md) | Leer hoe gegevens verplaatsen aan/uit Azure DocumentDB verzameling Azure gegevens Factory gebruiken |
| 34 | [Gegevens verplaatsen naar en vanuit Azure SQL-Database met Azure gegevens Factory](data-factory-azure-sql-connector.md) | Leer hoe u de gegevens van Azure SQL-Database met Azure gegevens Factory verplaatsen. |
| 35 | [Gegevens verplaatsen naar en vanuit Azure SQL Data Warehouse Azure gegevens Factory gebruiken](data-factory-azure-sql-data-warehouse-connector.md) | Informatie over het verplaatsen van gegevens van Azure SQL Data Warehouse Azure gegevens Factory gebruiken |
| 36 | [Gegevens verplaatsen naar en vanuit Azure-tabel met Azure gegevens Factory](data-factory-azure-table-connector.md) | Leer hoe u de gegevens van Azure Table Storage met Azure gegevens Factory verplaatsen. |
| 37 | [Activiteit prestaties en afstemmen handleiding kopiëren](data-factory-copy-activity-performance.md) | Meer informatie over belangrijke factoren die van invloed zijn op de prestaties van de verplaatsing van de gegevens in Azure gegevens fabriek wanneer u een kopie activiteit gebruikt. |
| 38 | [Gegevens verplaatsen met behulp van de activiteit kopiëren](data-factory-data-movement-activities.md) | Meer informatie over het verplaatsen van de gegevens Data Factory pijpleidingen: gegevensmigratie tussen cloud winkels, en tussen een on-premises implementatie store en een cloud-winkel. Gebruik kopie activiteit. |
| 39 | [Releaseopmerkingen voor Data Management Gateway](data-factory-gateway-release-notes.md) | Data Management Gateway-loop releaseopmerkingen |
| 40 | [Verplaats gegevens van de on-premises implementatie HDFS Azure gegevens Factory gebruiken](data-factory-hdfs-connector.md) | Meer informatie over het verplaatsen van gegevens van de on-premises implementatie HDFS met Azure gegevens Factory. |
| 41 | [Bewaken en Azure gegevens Factory pijpleidingen met nieuwe controle en beheer App beheren](data-factory-monitor-manage-app.md) | Informatie over het gebruiken van controle en beheer App wilt controleren en Azure gegevens factory's en pijpleidingen beheren. |
| 42 | [Gegevens verplaatsen tussen on-premises bronnen en de cloud met Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) | Een data gateway instellen om gegevens te verplaatsen tussen de on-premises en de cloud. Data Management Gateway in fabriek van Azure-gegevens gebruiken om uw gegevens te verplaatsen. |
| 43 | [Gegevens uit een OData-bron met Azure gegevens Factory verplaatsen](data-factory-odata-connector.md) | Meer informatie over het verplaatsen van gegevens uit OData-bronnen met Azure gegevens Factory. |
| 44 | [Gegevens uit ODBC-gegevens winkels met Azure gegevens Factory verplaatsen](data-factory-odbc-connector.md) | Meer informatie over het verplaatsen van gegevens van ODBC-gegevens winkels met Azure gegevens Factory. |
| 45 | [Gegevens uit DB2 met Azure gegevens Factory verplaatsen](data-factory-onprem-db2-connector.md) | Meer informatie over het verplaatsen van gegevens uit een DB2-Database via van Azure gegevens Factory |
| 46 | [Gegevens verplaatsen naar en vanuit een on-premises implementatie-bestandssysteem met behulp van Azure gegevens Factory](data-factory-onprem-file-system-connector.md) | Leer hoe u gegevens naar en vanuit een on-premises implementatie-bestandssysteem verplaatsen met behulp van Azure gegevens Factory. |
| 47 | [Gegevens uit MySQL-met Azure gegevens Factory verplaatsen](data-factory-onprem-mysql-connector.md) | Meer informatie over het verplaatsen van gegevens uit een MySQL-database via van Azure gegevens Factory. |
| 48 | [Verplaats gegevens van on-premises implementatie Oracle Azure gegevens Factory gebruiken](data-factory-onprem-oracle-connector.md) | Informatie over het verplaatsen van gegevens uit Oracle-database die is on-premises implementatie Azure gegevens Factory gebruiken. |
| 49 | [Verplaats gegevens van Azure gegevens Factory met PostgreSQL](data-factory-onprem-postgresql-connector.md) | Meer informatie over het verplaatsen van gegevens uit een PostgreSQL-Database via van Azure gegevens Factory. |
| 50 | [Verplaats gegevens van Azure gegevens Factory met Sybase](data-factory-onprem-sybase-connector.md) | Meer informatie over het verplaatsen van gegevens uit een Sybase-Database via van Azure gegevens Factory. |
| 51 | [Verplaats gegevens van Azure gegevens Factory met Teradata](data-factory-onprem-teradata-connector.md) | Meer informatie over Teradata-Connector voor de Data Factory-service waarmee u gegevens uit Teradata-Database verplaatsen |
| 52 | [Gegevens naar en vanuit SQL Server on-premises of op IaaS (Azure VM) met behulp van Azure gegevens Factory verplaatsen](data-factory-sqlserver-connector.md) | Meer informatie over het verplaatsen van gegevens uit SQL Server-database on-premises of in een Azure VM met Azure gegevens Factory. |
| 53 | [Verplaats gegevens van een Web tabelbron Azure gegevens Factory gebruiken](data-factory-web-table-connector.md) | Meer informatie over het verplaatsen van gegevens vanuit on-premises een tabel in een webpagina met Azure gegevens Factory. |



## <a name="data-transformation"></a>Gegevenstransformatie

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 54 | [Bekijk pijpleidingen met Azure Machine Learning-activiteiten maken](data-factory-azure-ml-batch-execution-activity.md) | Wordt beschreven hoe u blog pijpleidingen met Azure gegevens Factory en Azure Machine Learning maken |
| 55 | [Gekoppelde Services berekenen](data-factory-compute-linked-services.md) | Meer informatie over berekeningscluster enviornments waarmee u Azure gegevens Factory pijpleidingen Transformeren kunt/proces gegevens. |
| 56 | [Verwerken grootschalige gegevenssets Data Factory en Batch gebruiken](data-factory-data-processing-using-batch.md) | Beschreven hoe u grote hoeveelheden gegevens in een pijplijn Factory van Azure-gegevens met behulp van parallelle verwerking videomogelijkheden van Azure Batch verwerken. |
| 57 | [Transformeer gegevens in fabriek van Azure-gegevens](data-factory-data-transformation-activities.md) | Leer hoe u de gegevens of procesgegevens in Azure gegevens fabriek met Hadoop Machine Learning of Azure gegevens Lake Analytics transformeren. |
| 58 | [Hadoop Streaming activiteit](data-factory-hadoop-streaming-activity.md) | Leer hoe u kunt de Hadoop Streaming activiteit in een fabriek Azure gegevens Hadoop Streaming programma's uitvoert op een op aanvraag/uw eigen HDInsight cluster. |
| 59 | [Activiteit component](data-factory-hive-activity.md) | Leer hoe u kunt de component activiteit in een fabriek Azure gegevens component query's uitvoeren op een op aanvraag/uw eigen HDInsight cluster. |
| 60 | [Roepen MapReduce programma's van gegevens Factory](data-factory-map-reduce.md) | Informatie over het verwerken van gegevens door MapReduce-programma's uit te voeren op een Azure HDInsight cluster van een fabriek Azure-gegevens. |
| 61 | [Varken activiteit](data-factory-pig-activity.md) | Leer hoe u kunt de varken activiteit in een fabriek Azure gegevens varken scripts worden uitgevoerd op een op aanvraag/uw eigen HDInsight cluster. |
| 62 | [Een programma's van gegevens Factory roepen](data-factory-spark.md) | Leer hoe u een programma's van een Azure gegevens factory gebruik van de activiteit MapReduce roepen. |
| 63 | [Opgeslagen Procedure activiteit van SQL Server](data-factory-stored-proc-activity.md) | Leer hoe u de opgeslagen Procedure activiteit van SQL Server kunt gebruiken om aan te roepen van een opgeslagen procedure in een Azure SQL-Database of Azure SQL Data Warehouse uit een pijplijn Data Factory. |
| 64 | [Aangepaste activiteiten in een pijplijn Azure gegevens Factory gebruiken](data-factory-use-custom-activities.md) | Leer hoe u aangepaste activiteiten maken en gebruiken in een verkooppijplijn Factory van Azure-gegevens. |
| 65 | [I-SQL-script uitvoeren op Azure gegevens Lake analyses van Azure gegevens Factory](data-factory-usql-activity.md) | Informatie over het verwerken van gegevens door U-SQL-scripts op Azure gegevens Lake Analytics berekeningscluster-service uit te voeren. |



## <a name="samples"></a>Voorbeelden

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 66 | [Azure gegevens Factory - voorbeelden](data-factory-samples.md) | Meer informatie over voorbeelden die worden geleverd biedt met de Azure gegevens Factory-service. |



## <a name="use-cases"></a>Zaken gebruiken

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 67 | [Klant-casestudy](data-factory-customer-case-studies.md) | Meer informatie over hoe onze klanten aantal Azure gegevens Factory hebt gebruikt. |
| 68 | [Use-Case - klant profielen](data-factory-customer-profiling-usecase.md) | Leer hoe Azure gegevens Factory wordt gebruikt voor het maken van een werkstroom gegevensgestuurde (verkooppijplijn) om te spellen klanten profiel. |
| 69 | [Use-Case - aanbevelingen](data-factory-product-reco-usecase.md) | Meer informatie over een use-case geïmplementeerd via Azure gegevens Factory samen met andere services. |



## <a name="monitor-and-manage"></a>Bewaken en beheren

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 70 | [Bewaken en Azure gegevens Factory pijpleidingen beheren](data-factory-monitor-manage-pipelines.md) | Leer hoe u Azure-Portal en Azure PowerShell gebruiken om te controleren en beheren van Azure gegevens factory's en pijpleidingen die u hebt gemaakt. |



## <a name="sdk"></a>SDK

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 71 | [Azure gegevens Factory - wijzigingenlogboek voor .NET-API](data-factory-api-change-log.md) | Een beschrijving van de recente wijzigingen, functie toevoegingen, correcties enzovoort... in een specifieke versie van .NET-API voor de fabriek Azure-gegevens. |
| 72 | [Maken, controleren en beheren van Azure gegevens factory's met gegevens Factory .NET SDK](data-factory-create-data-factories-programmatically.md) | Leer hoe u via programmacode maken, controleren en Azure gegevens factory's beheren met behulp van Data Factory SDK. |
| 73 | [Naslaginformatie voor ontwikkelaars van Azure gegevens Factory](data-factory-sdks.md) | Meer informatie over de verschillende manieren om te maken, controleren en Azure gegevens factory's beheren |



## <a name="miscellaneous"></a>Diverse

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 74 | [Azure gegevens Factory - Veelgestelde vragen](data-factory-faq.md) | Veelgestelde vragen over Factory van Azure-gegevens. |
| 75 | [Azure gegevens Factory - functies en variabelen](data-factory-functions-variables.md) | Overzicht van Azure gegevens Factory-functies en variabelen |
| 76 | [Azure gegevens Factory - naamgeving](data-factory-naming-rules.md) | Beschrijft naming regels voor gegevens Factory entiteiten. |
| 77 | [Problemen met gegevens Factory oplossen](data-factory-troubleshoot.md) | Informatie over het oplossen van problemen met het gebruik van Azure gegevens Factory. |

