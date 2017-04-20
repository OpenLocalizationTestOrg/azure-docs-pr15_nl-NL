<properties 
    pageTitle="Verplaats gegevens aan/uit DocumentDB | Microsoft Azure" 
    description="Leer hoe gegevens verplaatsen aan/uit Azure DocumentDB verzameling Azure gegevens Factory gebruiken" 
    services="data-factory, documentdb" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="multiple" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-documentdb-using-azure-data-factory"></a>Gegevens verplaatsen naar en vanuit DocumentDB Azure gegevens Factory gebruiken

In dit artikel wordt beschreven hoe u de kopie activiteit in een fabriek Azure gegevens kunt gebruiken om gegevens uit een andere gegevens Azure DocumentDB opslaan en verplaats gegevens van DocumentDB naar een andere gegevensopslag te verplaatsen. In dit artikel is gebaseerd op het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , waarbij een een algemeen overzicht van de verplaatsing van gegevens met kopie activiteit en ondersteunde store combinaties wordt weergegeven.

In de volgende voorbeelden laten zien hoe u gegevens kopieert en naar Azure DocumentDB en Azure-blobopslag. Gegevens kan echter gekopieerde **rechtstreeks** vanaf een van de bronnen aan de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Azure gegevens Factory zijn.  

> [AZURE.NOTE] Kopiëren van gegevens uit aan-premises/Azure IaaS gegevens winkels aan Azure DocumentDB en omgekeerd worden ondersteund met Data Management Gateway versie 2.1 en hoger.

## <a name="sample-copy-data-from-documentdb-to-azure-blob"></a>Voorbeeld: Gegevens uit DocumentDB naar Azure Blob kopiëren

In het onderstaande voorbeeld ziet:

1. Een gekoppelde service van het type [DocumentDb](#azure-documentdb-linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Een invoer [dataset](data-factory-create-datasets.md) van het type [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Een [verkooppijplijn](data-factory-create-pipelines.md) met kopie activiteit die wordt gebruikt [DocumentDbCollectionSource](#azure-documentdb-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

De steekproef worden gegevens van Azure DocumentDB gekopieerd naar Azure Blob. De JSON-eigenschappen in deze voorbeelden gebruikt, worden beschreven in de secties in de voorbeelden te volgen.

**Azure DocumentDB gekoppeld service:**

    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure-blobopslag gekoppeld service:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure Document DB invoer gegevensset:**

De steekproef wordt ervan uitgegaan dat u hebt een siteverzameling met de **persoon die** naam in een DocumentDB Azure-database.
 
Instellen "externe": "true" en externalData beleidsgegevens de service Factory van Azure-gegevens die de tabel de fabriek gegevens buiten en niet worden geproduceerd door een activiteit in de fabriek gegevens op te geven.

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }


**Azure Blob uitvoer gegevensset:**

Gegevens wordt gekopieerd naar een nieuwe blob per uur door het pad voor het weergeven van de specifieke datetime met uur granulatie blob.

    {
      "name": "PersonBlobTableOut",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Voorbeeld JSON-document in de siteverzameling van de persoon in een database DocumentDB: 

    {
      "PersonId": 2,
      "Name": {
        "First": "Jane",
        "Middle": "",
        "Last": "Doe"
      }
    }

DocumentDB ondersteunt opvragen documenten met een SQL zoals syntaxis via hiërarchische JSON-documenten. 

Voorbeeld: Selecteer Person.PersonId, Person.Name.First als voornaam, Person.Name.Middle als afzonderlijk, Person.Name.Last als achternaam van persoon

De volgende pijplijn, gegevens van de siteverzameling van de persoon in de database DocumentDB naar een Azure blob kopiëren. Als onderdeel van de activiteit kopiëren de invoer en uitvoer zijn gegevenssets opgegeven.  
    
    {
      "name": "DocDbToBlobPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "DocumentDbCollectionSource",
                "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                "nestingSeparator": "."
              },
              "sink": {
                "type": "BlobSink",
                "blobWriterAddHeader": true,
                "writeBatchSize": 1000,
                "writeBatchTimeout": "00:00:59"
              }
            },
            "inputs": [
              {
                "name": "PersonDocumentDbTable"
              }
            ],
            "outputs": [
              {
                "name": "PersonBlobTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromDocDbToBlob"
          }
        ],
        "start": "2015-04-01T00:00:00Z",
        "end": "2015-04-02T00:00:00Z"
      }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-documentdb"></a>Voorbeeld: Gegevens van Azure Blob naar Azure DocumentDB kopiëren

In het onderstaande voorbeeld ziet:

1. Een gekoppelde service van het type [DocumentDb](#azure-documentdb-linked-service-properties).
2. Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3. Een invoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Een uitvoer [dataset](data-factory-create-datasets.md) van het type [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. Een [verkooppijplijn](data-factory-create-pipelines.md) met kopie activiteit die wordt gebruikt [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) en [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).


De steekproef kopieert gegevens van Azure blob naar Azure DocumentDB. De JSON-eigenschappen in deze voorbeelden gebruikt, worden beschreven in de secties in de voorbeelden te volgen.

**Azure-blobopslag gekoppeld service:**
    
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure DocumentDB gekoppeld service:**
    
    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure Blob invoer gegevensset:**

    {
      "name": "PersonBlobTableIn",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "MiddleName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
          }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "fileName": "input.csv",
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Azure DocumentDB uitvoer gegevensset:**

De steekproef worden gegevens naar een siteverzameling met de naam 'Contactpersoon' gekopieerd.

    {
      "name": "PersonDocumentDbTableOut",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "Name.First",
            "type": "String"
          },
          {
            "name": "Name.Middle",
            "type": "String"
          },
          {
            "name": "Name.Last",
            "type": "String"
          }
        ],
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

De volgende pijplijn kopieert gegevens van Azure Blob aan de collectie persoon in de DocumentDB. Als onderdeel van de activiteit kopiëren de invoer en uitvoer zijn gegevenssets opgegeven. 
    
    {
      "name": "BlobToDocDbPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "DocumentDbCollectionSink",
                "nestingSeparator": ".",
                "writeBatchSize": 2,
                "writeBatchTimeout": "00:00:00"
              }
              "translator": {
                  "type": "TabularTranslator",
                  "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
              }
            },
            "inputs": [
              {
                "name": "PersonBlobTableIn"
              }
            ],
            "outputs": [
              {
                "name": "PersonDocumentDbTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromBlobToDocDb"
          }
        ],
        "start": "2015-04-14T00:00:00Z",
        "end": "2015-04-15T00:00:00Z"
      }
    }
 
Als de steekproef blob invoer als 

    1,John,,Doe

De uitvoer JSON in DocumentDB worden vervolgens als:

    {
      "Id": 1,
      "Name": {
        "First": "John",
        "Middle": null,
        "Last": "Doe"
      },
      "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
    }
    
DocumentDB is een NoSQL store voor JSON-documenten, waar geneste structuren zijn toegestaan. Azure gegevens Factory kan de gebruiker om aan te geven van de hiërarchie via **nestingSeparator**, dat wil zeggen "." in dit voorbeeld. Met het scheidingsteken, de activiteit kopie genereert het object "Naam" met drie onderliggende elementen eerst tweede voornaam en achternaam, op basis van de 'Name.First', 'Name.Middle' en 'Name.Last' in de definitie van de tabel.

## <a name="azure-documentdb-linked-service-properties"></a>Azure DocumentDB gekoppelde Service-eigenschappen

De volgende tabel bevat een beschrijving voor de JSON-elementen die specifiek zijn voor de service Azure DocumentDB gekoppeld. 

| **Eigenschap** | **Beschrijving** | **Vereist** |
| -------- | ----------- | --------- |
| type | De eigenschap type moet zijn ingesteld op: **DocumentDb** | Ja |
| connectionString | Geef de gegevens die nodig zijn verbinding maken met DocumentDB Azure-database. | Ja |

## <a name="azure-documentdb-dataset-type-properties"></a>Azure DocumentDB Dataset type eigenschappen

Raadpleeg het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst met secties en eigenschappen beschikbaar voor de gegevenssets definiëren. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON lijken voor alle gegevensset typen (Azure SQL Azure blob, Azure tabel, enz.).
 
De sectie typeProperties verschilt voor elk type gegevensset en vindt u informatie over de locatie van de gegevens in de gegevensopslag. De sectie typeProperties voor de gegevensset van het type **DocumentDbCollection** heeft de volgende eigenschappen.

| **Eigenschap** | **Beschrijving** | **Vereist** |
| -------- | ----------- | -------- |
| NaamVerzameling | Naam van de verzameling DocumentDB-document. | Ja |


Voorbeeld:

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

### <a name="schema-by-data-factory"></a>Schema door gegevens Factory
Voor gegevens schema winkels zoals DocumentDB afleidt de gegevens Factory-service het schema in een van de volgende manieren:  

1.  Als u de structuur van gegevens met behulp van de eigenschap **structuur** in de definitie van de gegevensset opgeeft, wordt in de gegevens Factory-service deze structuur respecteert als het schema. In dit geval als een rij niet een waarde voor een kolom bevat, een null-waarde krijgt voor deze.
2.  Als u de structuur van gegevens met behulp van de eigenschap **structuur** in de definitie van de gegevensset niet opgeeft, wordt in de gegevens Factory-service het schema afleidt met behulp van de eerste rij in de gegevens. In dit geval als de eerste rij bevat geen het volledige schema, zijn aantal kolommen ontbreken in het resultaat kopiëren.

Daarom voor schema-vrije gegevensbronnen is de beste manier is om op te geven van de structuur van gegevens met behulp van de eigenschap **structuur** .

## <a name="azure-documentdb-copy-activity-type-properties"></a>Azure DocumentDB kopie activiteit type eigenschappen

Raadpleeg het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst van secties & eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en beleid zijn beschikbaar voor alle typen activiteiten.
 
**Notitie:** De activiteit kopie haalt slechts één invoer en genereert slechts één uitvoer.

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren echter met elk activiteitstype en voor het geval de kopie activiteit ze varieert afhankelijk van de soorten bronnen en sinks.

Voor het geval de kopie activiteit wanneer bron van het type **DocumentDbCollectionSource** zijn de volgende eigenschappen beschikbaar in de sectie **typeProperties** :

| **Eigenschap** | **Beschrijving** | **Toegestane waarden** | **Vereist** |
| ------------ | --------------- | ------------------ | ------------ |
| query | Geef de query om te lezen van gegevens. | Queryreeks wordt ondersteund door DocumentDB. <br/><br/>Voorbeeld:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` | Nee <br/><br/>Als u niet opgeeft, de SQL-instructie die wordt uitgevoerd:`select <columns defined in structure> from mycollection` 
| nestingSeparator | Speciaal teken om aan te geven dat het document is genest | Een teken. <br/><br/>DocumentDB is een NoSQL store voor JSON-documenten, waar geneste structuren zijn toegestaan. Azure gegevens Factory kan de gebruiker om aan te geven van de hiërarchie via nestingSeparator, dat wil zeggen "." in de bovenstaande voorbeelden. Met het scheidingsteken, de activiteit kopie genereert het object "Naam" met drie onderliggende elementen eerst tweede voornaam en achternaam, op basis van de 'Name.First', 'Name.Middle' en 'Name.Last' in de definitie van de tabel. | Nee

**DocumentDbCollectionSink** ondersteunt de volgende eigenschappen:

| **Eigenschap** | **Beschrijving** | **Toegestane waarden** | **Vereist** |
| -------- | ----------- | -------------- | -------- |
| nestingSeparator | Een speciaal teken in de kolomnaam van de bron om aan te geven dat geneste document nodig is. <br/><br/>Bijvoorbeeld hierboven: `Name.First` in de uitvoer tabel de volgende JSON-structuur in het document DocumentDB oplevert:<br/><br/>"Naam": {<br/>  "Eerste": "John"<br/>}, | Teken dat wordt gebruikt voor het scheiden van geneste niveaus.<br/><br/>Standaardwaarde is `.` (punt). | Teken dat wordt gebruikt voor het scheiden van geneste niveaus. <br/><br/>Standaardwaarde is `.` (punt). | Nee | 
| writeBatchSize | Het aantal parallelle aanvragen voor DocumentDB service om documenten te maken.<br/><br/>Als u gegevens uit DocumentDB met deze eigenschap, kunt u de prestaties verfijnen. U kunt een betere prestaties kunt verwachten als u writeBatchSize vergroten omdat meer parallelle aanvragen voor DocumentDB worden verzonden. Het foutbericht echter moet u voorkomen dat beperken kunnen genereren: "Aanvragen rente gelijk is aan grote".<br/><br/>Beperken wordt bepaald door een aantal factoren, met inbegrip van de grootte van documenten, aantal termen in documenten, indexeren beleid van target siteverzameling, enzovoort. Voor het kopiëren van, kunt u een betere collectie (bijvoorbeeld S3) gebruiken om optimaal doorvoer beschikbaar (2500 aanvragen eenheden/tweede). | Geheel getal | Niet (standaard: 10000) |
| writeBatchTimeout | Wacht tijd voor de bewerking is voltooid voordat er een optreedt time-out. | tijdspanne<br/><br/> Voorbeeld: "00: 30:00" (30 minuten). | Nee |
 
## <a name="appendix"></a>Bijlage
1. **Vraag:**  
    bevat de update van de ondersteuning kopie activiteit van bestaande records?

    **Answer:**  
    Nee.

2. **Vraag:**  
    records hoe al wordt een nieuwe poging van een kopie naar DocumentDB behandelen gekopieerd?

    **Answer:**  
    als records een veld "ID" bevatten en de kopieeropdracht wil invoegen van een record met dezelfde ID, de kopieeropdracht genereert een fout.  
 
3. **Vraag:** 
    Data Factory ondersteunt [bereik of de hash gebaseerde gegevens partitioneren](https://azure.microsoft.com/documentation/articles/documentdb-partition-data/)? 

    **Answer:** 
    Nee. 
4. **Vraag:** 
    kan ik meer dan één DocumentDB siteverzameling voor een tabel opgeven?
    
    **Answer:** 
    Nee. Slechts één siteverzameling kan worden opgegeven op dit moment.
     
## <a name="performance-and-tuning"></a>Prestaties en optimaliseren  
Zie [kopie activiteit prestaties & handleiding optimaliseren](data-factory-copy-activity-performance.md) voor meer informatie over belangrijke factoren die invloed prestaties van gegevens verplaatsen (kopie activiteit) in Factory van Azure-gegevens en verschillende manieren optimaliseren.
