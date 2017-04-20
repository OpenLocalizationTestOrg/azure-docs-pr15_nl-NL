<properties
   pageTitle="Gegevens van Azure-blobopslag laadt in Azure SQL Data Warehouse (Azure Data Factory genoemd) | Microsoft Azure"
   description="Leer hoe u gegevens met Azure gegevens Factory laden"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>
<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="load-data-from-azure-blob-storage-into-azure-sql-data-warehouse-azure-data-factory"></a>Gegevens van Azure-blobopslag laadt in Azure SQL Data Warehouse (Azure Data Factory genoemd)

> [AZURE.SELECTOR]
- [Gegevens Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
- [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

 Deze zelfstudie wordt getoond hoe u een pijplijn maakt in Azure gegevens Factory gegevens verplaatsen van Azure opslag Blob naar SQL Data Warehouse. U wordt met de volgende stappen uit:

+ Voorbeeldgegevens instellen in een Blob Azure-opslag.
+ Bronnen verbinden met Azure gegevens Factory.
+ Maak een pijplijn Verplaats gegevens van opslag BLOB's naar SQL Data Warehouse.

>[AZURE.VIDEO loading-azure-sql-data-warehouse-with-azure-data-factory]


## <a name="before-you-begin"></a>Voordat u begint

Als u wilt Maak uzelf vertrouwd met Azure gegevens Factory, raadpleegt u [Inleiding tot Factory van Azure-gegevens][].

### <a name="create-or-identify-resources"></a>Maak of resources identificeren

Voordat u deze zelfstudie begint, moet u beschikken over de volgende bronnen.

   + **Azure opslag Blob**: deze zelfstudie gebruikt Azure opslag Blob als de gegevensbron voor de pijplijn Factory van Azure-gegevens en dus moet u beschikken over een beschikbaar voor het opslaan van de voorbeeldgegevens. Als u niet een al hebt, leert u hoe u [een opslag-account maken][].

   + **SQL Data Warehouse**: de gegevens worden verplaatst in deze zelfstudie van Azure opslag Blob naar SQL Data Warehouse en zo nodig een data-warehouse online die samen met de voorbeeldgegevens AdventureWorksDW is geladen. Als u nog een data-warehouse, leert u hoe u voor het [inrichten van een][Create a SQL Data Warehouse]. Als u een data-warehouse hebt, maar deze niet met de voorbeeldgegevens inrichten, kunt u [deze handmatig laden][Load sample data into SQL Data Warehouse].

   + **Azure gegevens Factory**: Azure gegevens Factory voltooit de werkelijke laden en dus moet u beschikken over een waarmee u kunt u de gegevens verkeer verkooppijplijn kunt maken. Als u niet een al hebt, informatie over het maken van een in stap 1 van [aan de slag met Azure gegevens Factory (Factory-Editor, gegevens)][].

   + **AZCopy**: U AZCopy de voorbeeldgegevens kopiëren van uw lokale client naar uw Azure opslag Blob nodig hebt. Zie de [documentatie van AZCopy][]voor installatie-instructies.

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Stap 1: Voorbeeldgegevens naar Azure opslag Blob kopiëren

Nadat u alle van de onderdelen die gereed hebt, kunt u bent voorbeeldgegevens kopiëren naar uw Azure opslag Blob.

1. [Downloaden van voorbeeldgegevens][]. Deze gegevens wordt nog eens drie jaar met verkoopgegevens, toevoegen aan uw AdventureWorksDW-voorbeeldgegevens.

2. Gebruik deze opdracht AZCopy de drie jaar van gegevens kopiëren naar uw Azure opslag Blob.

````
AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
````


## <a name="step-2-connect-resources-to-azure-data-factory"></a>Stap 2: Resources koppelen aan Factory van Azure-gegevens

Nu de gegevens die zich op hun plaats staan kunnen we de pijplijn Azure gegevens fabriek om de gegevens van Azure-blobopslag verplaatsen naar SQL Data Warehouse maken.

Als u wilt beginnen, open de [portal van Azure][] en selecteer uw factory gegevens in de linkerkant menu.

### <a name="step-21-create-linked-service"></a>Stap 2.1: Gekoppelde Service maken

Uw opslag Azure-account en SQL Data Warehouse koppelen aan uw factory gegevens.  

1. Eerst het registratieproces beginnen door te klikken op de sectie 'Gekoppelde Services' van uw gegevens fabriek en klik op 'nieuwe gegevens opslaan.' Kies een naam voor het registreren van uw azure-opslag onder, selecteer Azure-opslag terwijl u typt en voer vervolgens uw accountnaam en Accountsleutel.

2. Als u wilt registreren Ga SQL Data Warehouse naar de "Auteur en Deploy" sectie, selecteer 'Nieuw gegevensopslag' en 'Azure SQL Data Warehouse'. Kopiëren en plakken in deze sjabloon en vul uw specifieke informatie.

```JSON
{
    "name": "<Linked Service Name>",
    "properties": {
        "description": "",
        "type": "AzureSqlDW",
        "typeProperties": {
             "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
         }
    }
}
```

### <a name="step-22-define-the-dataset"></a>Stap 2.2: Definieer de gegevensset

Na het maken van de gekoppelde services, moeten we de gegevensgroepen definiëren.  Hier betekent dit dat de structuur van de gegevens die uit uw opslag wordt verplaatst naar de data-warehouse definiëren.  U kunt meer informatie over het maken van

1. Dit proces starten door te gaan naar de sectie 'Auteur and Deploy' van uw gegevens factory.

2. Klik op 'Nieuwe gegevensset' en 'Azure-blobopslag' uw opslagruimte aan uw factory gegevens koppelen.  U kunt de onderstaande script Azure-blobopslag uw gegevens definiëren:

```JSON
{
    "name": "<Dataset Name>",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "<linked storage name>",
        "typeProperties": {
            "folderPath": "<containter name>",
            "fileName": "FactInternetSales.csv",
            "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```


3. Nu wordt ook definiëren we onze gegevensset voor SQL Data Warehouse.  We beginnen op dezelfde manier, door te klikken op 'Nieuwe gegevensset' en 'Azure SQL Data Warehouse'.

```JSON
{
    "name": "DWDataset",
    "properties": {
        "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "FactInternetSales"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

## <a name="step-3-create-and-run-your-pipeline"></a>Stap 3: Maken en uitvoeren van uw verkooppijplijn

Tot slot we wordt ingesteld en uitvoeren van de pijplijn in Factory van Azure-gegevens.  Dit is de bewerking die de verplaatsing van de werkelijke gegevens invullen.  U kunt een volledige weergave van de bewerkingen die u met SQL Data Warehouse en Azure gegevens Factory uitvoeren kunt vinden [hier][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].

Klik in de sectie 'Auteur and Deploy' op nu 'Meer opdrachten' en klik vervolgens op 'Nieuwe verkooppijplijn'.  Nadat u de verkooppijplijn hebt gemaakt, kunt u de onderstaande code wilt overbrengen van de gegevens naar uw gegevens BTW-records:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
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
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie, begint u met bekijken:

- [Azure gegevens Factory leerpad][].
- [Azure SQL-datawarehouse verbindingslijn][]. Dit is het onderwerp core naslaginformatie voor het gebruik van Azure gegevens Factory met Azure SQL Data Warehouse.


Deze onderwerpen bevatten gedetailleerde informatie over Factory van Azure-gegevens. Ze bespreken Azure SQL-Database of HDinsight, maar de informatie geldt ook voor Azure SQL Data Warehouse.

- [Zelfstudie: aan de slag met Azure gegevens Factory][] Dit is de zelfstudie core voor het verwerken van gegevens met Azure gegevens Factory. In deze zelfstudie wordt u uw eerste verkooppijplijn waarin HDInsight transformeren en analyseren weblogs maandelijks managementrapporten maken. U ziet, is geen activiteit kopiëren in deze zelfstudie.
- [Zelfstudie: gegevens van Azure opslag Blob kopiëren naar Azure SQL-Database][]. In deze zelfstudie maakt u een pijplijn in fabriek van Azure-gegevens kopiëren van gegevens van Azure opslag Blob naar Azure SQL-Database.


<!--Image references-->

<!--Article references-->
[AZCopy documentatie]: ../storage/storage-use-azcopy.md
[Azure SQL Data Warehouse Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Een opslag-account maken]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Aan de slag met Azure gegevens Factory (gegevens Factory-Editor)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Inleiding tot Azure gegevens Factory]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Zelfstudie: Gegevens van Azure opslag Blob kopiëren met Azure SQL-Database]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Zelfstudie: Aan de slag met Azure gegevens Factory]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory leerpad]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure-portal]: https://portal.azure.com
[Downloaden van voorbeeldgegevens]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv
