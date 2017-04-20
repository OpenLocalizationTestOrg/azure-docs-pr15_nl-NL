<properties
   pageTitle="Grote hoeveelheden gegevens uploaden naar Lake gegevensopslag met offline methoden | Microsoft Azure"
   description="AdlCopy-hulpprogramma met gegevens van Azure opslag BLOB's kopiëren naar Lake gegevensopslag"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/07/2016"
   ms.author="nitinme"/>

# <a name="use-azure-import-export-service-for-offline-copy-of-data-to-data-lake-store"></a>Azure-Service voor importeren exporteren gebruiken voor offlinekopie van gegevens naar Lake gegevensopslag

In dit artikel leert u over het kopiëren van grote gegevenssets (> 200GB) bij een Azure-gegevens Lake winkel, met behulp van offlinekopie methoden, zoals [Azure-Service voor importeren/exporteren](../storage/storage-import-export-service.md). Het bestand dat wordt gebruikt als voorbeeld in dit artikel is met name 339,420,860,416 bytes dat wil zeggen ongeveer 319GB op schijf. Laten we belt u dit bestand 319GB.tsv.

Azure importeren/exporteren Service kunt u grote hoeveelheden gegevens veilig overbrengen met Azure-blobopslag door vaste schijven een Azure Datacenter voor verzending.

## <a name="prerequisites"></a>Vereisten voor

Voordat u in dit artikel begint, hebt u het volgende:

- **Een Azure-abonnement**. Zie [Azure krijgen gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

- **Opslag van azure-account**.

- **Azure gegevens Lake Analytics-account (optioneel)** - Zie [aan de slag met Azure gegevens Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) voor instructies over het maken van een account voor gegevensopslag Lake.


## <a name="preparing-the-data"></a>De gegevens voorbereiden

Vóór het gebruik van de Service importeren/exporteren, moeten we het gegevensbestand om te worden overgedragen **naar exemplaren die minder dan 200 GB zijn** groot verbreken. Dit is omdat het hulpprogramma voor importeren niet met bestanden die groter is dan 200GB werkt. Om te voldoen, in deze zelfstudie we het bestand splitsen in stukken van 100GB bytes. U kunt dit doen [Cygwin](https://cygwin.com/install.html)eenvoudig te gebruiken. Cygwin ondersteunt Linux-opdracht waarmee de gebruikers eenvoudig hiervoor. We gebruiken de volgende opdracht voor onze hoofdletters/kleine letters.

    split -b 100m 319GB.tsv

De bewerking gesplitste bestanden gemaakt met de onderstaande namen.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Schijven voorbereiden met gegevens

Volg de instructies bij [Gebruik van Azure importeren/exporteren-Service](../storage/storage-import-export-service.md) (onder de sectie **voorbereiden van uw stations** ) als u wilt uw vaste schijven voorbereiden. Hier ziet u de algehele stroom over het voorbereiden van het station.

1. Een vaste schijf die voldoet aan de vereisten voor worden gebruikt voor Auzre importeren/exporteren Service aanschaffen.

2. Identificeren een Azure Storage-account waar de gegevens worden gekopieerd eenmaal si verzonden naar het beheercentrum Azure-gegevens.

3. Gebruik het [Hulpprogramma Azure-importeren/exporteren](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), een opdrachtregelhulpprogramma. Hier volgt een voorbeeld-fragment over het gebruik van het hulpmiddel.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    Zie [Azure importeren/exporteren-Service gebruiken](../storage/storage-import-export-service.md) voor meer steekproef fragmenten over het gebruik van het **Hulpprogramma Azure-importeren/exporteren**.

4. De bovenstaande opdracht maakt een logboek-bestand op de opgegeven locatie. U kunt dit bestand logboek wilt gebruiken voor het maken van een project importeren in de [Klassieke Azure-Portal](https://manage.windowsazure.com).

## <a name="create-an-import-job"></a>Maken van een project importeren

U kunt nu een importeren taak volgens de instructies bij [Gebruik van Azure importeren/exporteren-Service](../storage/storage-import-export-service.md) (onder de sectie **maken de taak importeren** ) maken. Voor deze taak importeren met andere details, Daarnaast bieden het logboek-bestand gemaakt bij het voorbereiden van de schijfstations.

## <a name="physically-ship-the-disks"></a>De schijven fysiek verzenden

U kunt nu de schijven een Azure Datacenter waar de gegevens wordt gekopieerd naar de BLOB van de Azure opslag's u tijdens het maken van de taak importeren hebt opgegeven voor fysiek verzenden. Ook tijdens het maken van de taak, wanneer u niet later informatie bijhouden u kunt nu teruggaan aan uw project importeren en de volgnummer bijgewerkt.

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-store"></a>Gegevens van Azure opslag BLOB's kopiëren naar Azure Lake gegevensopslag

Zodra de status van de taak importeren voltooide ziet, kunt u controleren of de gegevens zijn beschikbaar in de BLOB van de Azure opslag's u had opgegeven. Vervolgens kunt u verschillende methoden die gegevens verplaatsen van de Azure opslag BLOB's naar Azure Lake gegevensopslag. Zie [Ingesting gegevens in Lake gegevensopslag](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store)voor alle de beschikbare opties voor het uploaden van gegevens.

In deze sectie bieden we u met de JSON-definities die u gebruiken kunt om te maken van een fabriek van Azure-gegevens pijplijn voor het kopiëren van gegevens. U kunt deze JSON definities van de [Azure-portal](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md)gebruiken.

### <a name="source-linked-service-azure-storage-blob"></a>Bron gekoppeld Service (Azure opslag Blob)

````
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
````

### <a name="target-linked-service-azure-data-lake-store"></a>Doelgerichte gekoppelde Service (Azure gegevensopslag-Lake)

````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>Invoer gegevensverzameling
````
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
````
### <a name="output-data-set"></a>Uitvoer gegevensverzameling
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>Pijplijn (kopie activiteit)
````
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
````
Zie [gegevens van Azure opslag Blob naar Azure Lake gegevensopslag met Azure gegevens Factory verplaatsen](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)voor meer informatie over het gebruik van Azure gegevens Factory om gegevens te verplaatsen van Azure opslag Blob en Azure Lake gegevensopslag.

## <a name="reconstruct-the-data-files-in-azure-data-lake-store"></a>Opnieuw maken van de bestanden in Azure Lake gegevensopslag

We de slag met een bestand dat is 319GB grootte en moest worden deze in bestanden van kleinere zodat deze kan worden verplaatst met de Azure-Service voor importeren/exporteren. Nu dat de gegevens zich in de gegevensopslag Lake Azure, kunnen wij reconstrcut het bestand naar de oorspronkelijke grootte. U kunt de volgende Azure PowerShell-cmldts kunt doen.

````
# Login to our account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>Volgende stappen

- [Beveiliging van gegevens in Lake gegevensopslag](data-lake-store-secure-data.md)
- [Azure gegevens Lake Analytics gebruiken met Lake gegevensopslag](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight gebruiken met Lake gegevensopslag](data-lake-store-hdinsight-hadoop-use-portal.md)
