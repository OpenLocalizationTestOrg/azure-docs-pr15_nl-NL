<properties
    pageTitle="Verplaats gegevens van een on-premises SQL Server naar SQL Azure wordt aangegeven met Azure gegevens Factory | Azure"
    description="Het instellen van een ADF verkooppijplijn die stelt het bericht op twee activiteiten van de gegevens migratie die samen gegevens dagelijks tussen databases on-premises en in de cloud verplaatsen."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />


# <a name="move-data-from-an-on-premise-sql-server-to-sql-azure-with-azure-data-factory"></a>Verplaats gegevens van een on-premises SQL server naar SQL Azure wordt aangegeven met Azure gegevens Factory

Dit onderwerp wordt uitgelegd hoe u gegevens uit een on-premises SQL Server-Database verplaatsen naar een SQL Azure-Database via Azure-blobopslag gebruik van de Azure gegevens Factory (ADF).

Het volgende **menu** koppelingen naar onderwerpen waarin wordt uitgelegd hoe u gegevens in de doel-omgevingen waar de gegevens kunnen worden opgeslagen en verwerkt tijdens het wetenschappelijke Team gegevens nemen.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="intro"></a>Inleiding: Wat is ADF en wanneer moet worden gebruikt om gegevens te migreren?

Azure gegevens Factory is een volledig beheerde cloudgebaseerde integratie gegevensservice die orchestrates en automatiseert de verplaatsing en transformatie van gegevens. Het belangrijkste concept in het model ADF is verkooppijplijn. Een pijplijn is een logische groepering van activiteiten, die elk de acties uitvoeren op de gegevens in gegevenssets definieert. Gekoppelde services worden gebruikt voor het definiëren van de gegevens die u nodig hebt voor gegevens Factory verbinding maken met de gegevensbronnen.

Met ADF, kunnen bestaande gegevensverwerking services bestaan in gegevens pijplijnen die zeer beschikbaar en beheerde in de cloud zijn. Deze gegevens pijpleidingen kunnen worden gepland kunt nemen, voorbereiden, transformeren, analyseren en publiceren van gegevens en ADF worden beheerd en de complexe gegevens en de verwerking van afhankelijkheden orchestrates. Oplossingen kunnen worden snel gemaakt en geïmplementeerd in de cloud, verbinding steeds meer on-premises en cloud gegevensbronnen.

U kunt ADF gebruiken:

- Wanneer de gegevens moet worden continu gemigreerd in een scenario voor hybride dat toegang zoekt tot zowel on-premises en cloud resources 
- Wanneer de gegevens is verwerkt of moet worden gewijzigd of bedrijfslogica die zijn toegevoegd aan deze wanneer het wordt gemigreerd. 

ADF kunt voor de planning en controle van taken met behulp van eenvoudige JSON-scripts die het verkeer van gegevens op basis van periodieke beheren. ADF, heeft ook andere mogelijkheden zoals ondersteuning voor complexe bewerkingen. Zie de documentatie op [Azure gegevens Factory (ADF)](https://azure.microsoft.com/services/data-factory/)voor meer informatie over ADF.


## <a name="scenario"></a>Het Scenario

We instellen een ADF verkooppijplijn die stelt het bericht op twee gegevens migratie activiteiten. Samen wordt gegevens dagelijks verplaatsen tussen een on-premises SQL-database en een Azure SQL-Database in de cloud. De twee activiteiten zijn:

* Kopieer gegevens uit een on-premises SQL Server-database aan een account Azure-blobopslag
* gegevens van het account Azure-blobopslag kopiëren naar een Azure SQL-Database.

>[AZURE.NOTE] De stappen die worden weergegeven hier zijn aangepast uit de meer gedetailleerde zelfstudie verstrekt door het team ADF: verwijzingen [gegevens tussen on-premises bronnen en cloud met Data Management Gateway verplaatsen](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) naar de betreffende secties van dat onderwerp worden verstrekt indien nodig.


## <a name="prereqs"></a>Vereisten voor
Deze zelfstudie wordt ervan uitgegaan dat u hebt:

* Een **Azure-abonnement**. Als u niet een abonnement hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* Een **account van Azure opslag**. U kunt een Azure opslag-account gebruiken voor het opslaan van de gegevens in deze zelfstudie. Als u niet een Azure opslag-account hebt, raadpleegt u het artikel [een opslag-account maken](storage-create-storage-account.md#create-a-storage-account) . Nadat u het opslag-account hebt gemaakt, moet u de accountsleutel gebruikt voor toegang tot de opslag verkrijgen. Zie de [weergave, kopiëren en opnieuw genereren opslag toegangstoetsen](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Toegang tot een **Azure SQL-Database**. Als u een Azure SQL-Database instelt moet, vindt u informatie met de tpoic [Aan de slag met Microsoft Azure SQL-Database](../sql-database/sql-database-get-started.md) voor het inrichten van een nieuw exemplaar van een Azure SQL-Database.
* Geïnstalleerd en geconfigureerd **Azure PowerShell** lokaal. Zie voor instructies voor het [installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

> [AZURE.NOTE] Deze procedure wordt gebruikt voor de [Azure-portal](https://portal.azure.com/).


##<a name="upload-data"></a>De gegevens uploaden naar uw on-premises SQL Server

We de [tevens Taxi gegevensset](http://chriswhong.com/open-data/foil_nyc_taxi/) gebruiken om te laten zien van het migratieproces. De gegevensset tevens Taxi is beschikbaar, zoals aangegeven in dat bericht, klikt u op Azure blob storage [Tevens Taxi gegevens](http://www.andresmh.com/nyctaxitrips/). De gegevens heeft twee bestanden, het trip_data.csv-bestand, met reis details, en het bestand trip_far.csv, met details van een klasse van elke reis dat is betaald. Een voorbeeld en een beschrijving van deze bestanden worden gegeven in het vak [Tevens Taxi reizen gegevensset beschrijving](machine-learning-data-science-process-sql-walkthrough.md#dataset).


U kunt aanpassen van de procedure hier tot een set met uw eigen gegevens gebruikt of volg de stappen beschreven met behulp van de gegevensset tevens Taxi. Als u wilt de gegevensset tevens Taxi uploaden naar uw on-premises SQL Server-database, volgt u de procedure in de [Gegevens van de bulksgewijs importeren in SQL Server-Database](machine-learning-data-science-process-sql-walkthrough.md#dbload). Deze instructies zijn bedoeld voor een SQL Server op een Azure virtuele machines, maar de procedure voor het uploaden naar de on-premises SQL Server is dezelfde.


##<a name="create-adf"></a>Maken van een fabriek Azure-gegevens

De instructies voor het maken van een nieuwe Factory van de Azure-gegevens en een resourcegroep in de [portal van Azure](https://portal.azure.com/) beschikbaar [maken van een fabriek Azure-gegevens](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#step-1-creating-the-data-factory). De naam van de nieuwe ADF exemplaar *adfdsp* en de naam van de resource groep gemaakt *adfdsprg*.


## <a name="install-and-configure-up-the-data-management-gateway"></a>Installeren en configureren van de Data Management Gateway

Als u wilt dat uw pijpleidingen in een fabriek Azure-gegevens voor gebruik met een on-premises SQL Server, moet u als een gekoppelde Service toevoegen aan de fabriek gegevens. Als u wilt een gekoppelde Service maken voor een on-premises SQL Server, moet u het volgende doen:

- Download en installeer Microsoft Data Management Gateway naar de on-premises computer. 
- Configureer de gekoppelde service voor de lokale gegevensbron voor het gebruik van de gateway. 

De Data Management Gateway serialiseert en deserializes de bron- en sink gegevens op de computer waarop die wordt gehost.

Zie [gegevens tussen on-premises bronnen en cloud met Data Management Gateway verplaatsen](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) voor installatie-instructies en informatie over Data Management Gateway


## <a name="adflinkedservices"></a>Gekoppelde services verbinding maken met de gegevensbronnen maken

Een gekoppelde service Hiermee definieert u de gegevens die u nodig hebt voor Azure gegevens Factory verbinding maken met een Gegevensresource. De stapsgewijze procedure voor het maken van gekoppelde services vindt u in het [maken van gekoppelde services](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-2-create-linked-services).

We hebben drie resources in dit scenario waarvoor u gekoppelde services nodig hebt.

1. [Gekoppelde service voor on-premises SQL Server](#adf-linked-service-onprem-sql)
2. [Gekoppelde service voor Azure-blobopslag](#adf-linked-service-blob-store)
3. [Gekoppelde service voor Azure SQL-database](#adf-linked-service-azure-sql)


###<a name="adf-linked-service-onprem-sql"></a>Gekoppelde service voor on-premises SQL Server-database

De gekoppelde service voor de on-premises SQL Server maken:

- Klik op de **Gegevensopslag** in de openingspagina ADF op klassieke Azure-Portal 
- Selecteer **SQL** en typ de *gebruikersnaam* en *wachtwoord* referenties voor de on-premises SQL Server. U moet de servernaam opgeven als een **volledig gekwalificeerd servernaam backslash exemplaarnaam (servernaam\exemplaarnaam)**. De naam van de gekoppelde service *adfonpremsql*.

###<a name="adf-linked-service-blob-store"></a>Gekoppelde service voor Blob

De gekoppelde service voor het account Azure-blobopslag maken:

- Klik op de **Gegevensopslag** in de openingspagina ADF op klassieke Azure-Portal
- Selecteer **Account van Azure-opslag** 
- Voer de Azure-blobopslag sleutel en container accountnaam. De naam van de gekoppelde Service *adfds*.

###<a name="adf-linked-service-azure-sql"></a>Gekoppelde service voor Azure SQL-database

De gekoppelde service voor het Azure SQL-Database maken:

- Klik op de **Gegevensopslag** in de openingspagina ADF op klassieke Azure-Portal
- Selecteer **SQL Azure** en typ de *gebruikersnaam* en *wachtwoord* referenties voor de Azure SQL-Database. De *gebruikersnaam* moet worden opgegeven als *user@servername*.   


##<a name="adf-tables"></a>Problemen definiëren en tabellen om op te geven hoe u toegang tot de gegevenssets maken

Tabellen die de structuur, de locatie en de beschikbaarheid van de gegevenssets met de volgende procedures op basis van een script opgeven maken. JSON-bestanden worden gebruikt voor het definiëren van de tabellen. Zie voor meer informatie over de structuur van deze bestanden, [gegevenssets](../data-factory/data-factory-create-datasets.md).

> [AZURE.NOTE]  U moet worden uitgevoerd de `Add-AzureAccount` cmdlet voordat de cmdlet [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) om te bevestigen dat het rechts Azure abonnement is geselecteerd voor de uitvoering van de opdracht wordt uitgevoerd. Zie [Toevoegen-AzureAccount](https://msdn.microsoft.com/library/azure/dn790372.aspx)voor documentatie van deze cmdlet.

De JSON gebaseerde definities in de tabellen de namen van de volgende gebruikt:

* de **naam van de tabel** in de on-premises SQL server is *nyctaxi_data*
* de **containernaam** in het account Azure-blobopslag is *containername*  

Drie tabeldefinities nodig zijn voor deze pijplijn ADF:

1. [SQL-on-premises-tabel](#adf-table-onprem-sql)
2. [BLOB-tabel](#adf-table-blob-store)
3. [SQL Azure-tabel](#adf-table-azure-sql)

> [AZURE.NOTE]  Deze procedures met Azure PowerShell kunt definiëren en de activiteiten ADF maken. Maar deze taken kunnen ook worden bereikt met behulp van de Azure portal. Zie [invoer maken en gegevenssets uitvoeren](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-3-create-input-and-output-datasets)voor meer informatie.

###<a name="adf-table-onprem-sql"></a>SQL-on-premises-tabel

De definitie van de tabel voor de on-premises SQL Server is opgegeven in het volgende JSON-bestand:

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }

                }
            }
        }

De kolomnamen zijn hier niet opgenomen. U kunt sub selecteren op de kolomnamen door ze hier (voor details controleren het onderwerp [ADF documentatie](../data-factory/data-factory-data-movement-activities.md ) .

Kopieer de JSON-definitie van de tabel naar een bestand met de naam van bestand *onpremtabledef.json* en sla deze op een bekende locatie (hier uitgegaan *C:\temp\onpremtabledef.json*). De tabel in ADF maken met de volgende Azure PowerShell-cmdlet:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


###<a name="adf-table-blob-store"></a>BLOB-tabel
Definitie voor de tabel voor de uitvoerlocatie blob is in de volgende (Hiermee worden de geconsumeerde gegevens van de on-premises met Azure blob):

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

Kopieer de JSON-definitie van de tabel naar een bestand met de naam van bestand *bloboutputtabledef.json* en sla deze op een bekende locatie (hier uitgegaan *C:\temp\bloboutputtabledef.json*). De tabel in ADF maken met de volgende Azure PowerShell-cmdlet:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

###<a name="adf-table-azure-sq"></a>SQL Azure-tabel
Definitie voor de tabel voor de SQL Azure-uitvoer is in de volgende (dit schema toegewezen de gegevens die afkomstig zijn uit de blob):

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

Kopieer de JSON-definitie van de tabel naar een bestand met de naam van bestand *AzureSqlTable.json* en sla deze op een bekende locatie (hier uitgegaan *C:\temp\AzureSqlTable.json*). De tabel in ADF maken met de volgende Azure PowerShell-cmdlet:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


##<a name="adf-pipeline"></a>Problemen definiëren en de pijplijn maken

Geef de activiteiten die deel uitmaakt van de pijplijn en de pijplijn met de volgende procedures op basis van een script maken. Een JSON-bestand wordt gebruikt om de eigenschappen van de verkooppijplijn te definiëren.

* Het script wordt ervan uitgegaan dat de **naam pijplijn** *AMLDSProcessPipeline*.
* Bedenk ook dat we de frequentie van de pijplijn worden uitgevoerd op dagelijks en gebruiken van de standaardtijd voor uitvoering voor de taak (12 am UTC) ingesteld.

> [AZURE.NOTE]De volgende procedures uit met Azure PowerShell kunt definiëren en maken van de pijplijn ADF. Maar deze taak kan ook worden uitgevoerd met behulp van de Azure portal. Zie [maken en uitvoeren een pijplijn](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-4-create-and-run-a-pipeline)voor meer informatie.

De tabeldefinities eerder hebt gebruikt, is de pijplijn voor de ADF opgegeven als volgt:

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premise SQL to Azure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premise SQL server to blob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       

                     },

                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data to Sql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",             
                            }           
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

Kopieer deze JSON-definitie van de pijplijn naar een bestand met de naam van bestand *pipelinedef.json* en sla deze op een bekende locatie (hier uitgegaan *C:\temp\pipelinedef.json*). Maak de pijplijn in ADF met de volgende Azure PowerShell-cmdlet:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

Bevestig dat u de pijplijn op de ADF in de klassieke Azure-Portal weergegeven als volgt te werk ziet (wanneer u klikt op het diagram)

![](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)


##<a name="adf-pipeline-start"></a>De pijplijn starten
De pijplijn kan nu worden uitgevoerd met de volgende opdracht uit:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

De *begindatum* en *einddatum* parameterwaarden moeten worden vervangen door de werkelijke datums waartussen u wilt dat de pijplijn om uit te voeren.

Zodra de pijplijn wordt uitgevoerd, moet u mogelijk zijn bepaalde gegevens worden weergegeven in de container geselecteerd voor de blob, één bestand per dag wilt bekijken.

Houd er rekening mee dat we de functionaliteit van ADF met recht streepje gegevens stapsgewijs niet hebt gebruikt. Zie voor meer informatie over hoe u dit en andere mogelijkheden die is verstrekt door ADF, de [ADF documentatie](https://azure.microsoft.com/services/data-factory/).
