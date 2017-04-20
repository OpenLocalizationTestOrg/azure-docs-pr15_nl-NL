<properties 
    pageTitle="Gegevenssets maken in Azure gegevens Factory | Microsoft Azure" 
    description="Informatie over het maken van gegevenssets in Azure gegevens fabriek met voorbeelden waarin met eigenschappen zoals verschuiving en anchorDateTime."
    keywords="maken van de gegevensset, voorbeeld van de gegevensset, voorbeeld van de offset"
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="shlo"/>

# <a name="datasets-in-azure-data-factory"></a>Gegevenssets in Azure gegevens fabriek
In dit artikel gegevenssets in Azure gegevens fabriek beschrijving en voorbeelden zoals verschuiving, anchorDateTime en verschuiving/stijl databases.

Wanneer u een gegevensset maakt, maakt u een verwijzing naar de gegevens die u wilt verwerken. Gegevens worden verwerkt (invoer/uitvoer) in een activiteit en een activiteit in een pijplijn is opgenomen. Een invoer dataset staat voor de invoer voor een activiteit in de pijplijn en een gegevensset uitvoer voor de uitvoer voor de activiteit.

Gegevenssets welke gegevens vanuit verschillende winkels, zoals tabellen, bestanden, mappen en documenten. Nadat u een gegevensset hebt gemaakt, kunt u deze kunt gebruiken met activiteiten in een pijplijn. Een gegevensset kan bijvoorbeeld een gegevensset invoer/uitvoer van een kopie of een activiteit HDInsightHive. De portal van Azure biedt een visuele lay-out van al uw pijpleidingen en gegevens invoer en uitvoer. In één oogopslag, u kunt zien alle relaties en afhankelijkheden van uw pijpleidingen in alle uw bronnen zodat u altijd weet waar gegevens vandaan en waar deze naartoe gaat.

In fabriek van Azure-gegevens, kunt u gegevens ophalen uit een gegevensset met behulp van kopie activiteit in een pijplijn.

> [AZURE.NOTE] Als u eerder met Azure gegevens Factory, raadpleegt u [Inleiding tot Factory van Azure-gegevens](data-factory-introduction.md) voor een overzicht van Azure gegevens Factory-service. Zie [uw eerste gegevens factory maken](data-factory-build-your-first-pipeline.md) voor een zelfstudie om te maken van uw eerste gegevens factory. Deze twee artikelen vindt u achtergrondinformatie hebt u nodig hebt in dit artikel beter te begrijpen. 

## <a name="define-datasets"></a>Gegevenssets definiëren
Een gegevensset in Azure gegevens fabriek wordt als volgt gedefinieerd: 


    {
        "name": "<name of dataset>",
        "properties": {
            "type": "<type of dataset: AzureBlob, AzureSql etc...>",
            "external": <boolean flag to indicate external data. only for input datasets>,
            "linkedServiceName": "<Name of the linked service that refers to a data store.>",
            "structure": [
                {
                    "name": "<Name of the column>",
                    "type": "<Name of the type>"
                }
            ],
            "typeProperties": {
                "<type specific property>": "<value>",
                "<type specific property 2>": "<value 2>",
            },
            "availability": {
                "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
                "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
            },
           "policy": 
            {      
            }
        }
    }

De volgende tabel worden de eigenschappen in de bovenstaande JSON beschreven:   

| Eigenschap | Beschrijving | Vereist | Standaard |
| -------- | ----------- | -------- | ------- |
| naam | Naam van de gegevensset. Zie [Azure gegevens Factory - naamgeving](data-factory-naming-rules.md) voor naming regels. | Ja | NB |
| type | Type van de gegevensset. Geef een van de typen die worden ondersteund door Factory van Azure-gegevens (bijvoorbeeld: AzureBlob, AzureSqlTable). <br/><br/>Zie [Gegevensset Type](#Type) voor meer informatie. | Ja | NB |
| structuur | Schema van de gegevensset<br/><br/>Zie voor meer informatie [Gegevensset structuur](#Structure) sectie. | Nee. | NB |
| typeProperties | Eigenschappen voor het geselecteerde bestandstype. Zie de sectie van de [Gegevensset Type](#Type) voor meer informatie over de ondersteunde typen en hun eigenschappen. | Ja | NB |
| externe | Booleaanse vlag om op te geven of een gegevensset expliciet is geproduceerd door een gegevens factory pijplijn of niet.  | Nee | ONWAAR | 
| beschikbaarheid | Hiermee definieert u het venster verwerking of het segmenteringshulplijnen model voor de productie gegevensset. <br/><br/>Zie voor meer informatie [De beschikbaarheid van de gegevensset](#Availability) sectie. <br/><br/>Voor meer informatie over het model, Zie [planning en uitvoering](data-factory-scheduling-and-execution.md) artikel segmenteren gegevensset. | Ja | NB
| beleid | Hiermee definieert u de criteria of de voorwaarde die de gegevensset segmenten moeten voldoen. <br/><br/>Zie voor meer informatie [Gegevensset beleid](#Policy) sectie. | Nee | NB |

## <a name="dataset-example"></a>Voorbeeld van de gegevensset
In het volgende voorbeeld wordt staat de gegevensset voor een tabel met de naam **MyTable** in een **Azure SQL-database**. 

    {
        "name": "DatasetSample",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": 
            {
                "tableName": "MyTable"
            },
            "availability": 
            {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

Houd rekening met de volgende punten: 

- type is ingesteld op AzureSqlTable.
- Tabelnaam type eigenschap (specifiek voor AzureSqlTable type) is ingesteld op MyTable.
- linkedServiceName verwijst naar een gekoppelde service van het type AzureSqlDatabase. Zie de definitie van de volgende gekoppelde service. 
- beschikbaarheid van de frequentie is ingesteld op dag en interval is ingesteld op 1, wat betekent dat het segment dagelijks is geproduceerd.  

AzureSqlLinkedService wordt als volgt gedefinieerd:

    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "",
            "typeProperties": {
                "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
            }
        }
    }

In de bovenstaande JSON: 

- type is ingesteld op AzureSqlDatabase
- de eigenschap type connectionString geeft informatie verbinding maken met een Azure SQL-database.  


Zoals u ziet, de gekoppelde service Hiermee definieert u hoe u verbinding maakt met een Azure SQL-database. De gegevensset wordt gedefinieerd welke tabel wordt gebruikt als een invoer/uitvoer voor de activiteit in een pijplijn. De sectie activiteit in de [verkooppijplijn](data-factory-create-pipelines.md) JSON geeft aan of de gegevensset als een gegevensset invoer- en uitvoerbereik wordt gebruikt.


> [AZURE.IMPORTANT] Tenzij een gegevensset wordt geproduceerd door Azure gegevens Factory, moet deze worden gemarkeerd als **externe**. Deze instelling is algemeen van toepassing op input van eerste activiteit in een pijplijn.   

## <a name="Type"></a>Type van de gegevensset
Ondersteunde gegevensbronnen en -typen gegevensset worden uitgelijnd. Zie onderwerpen waarnaar wordt verwezen in het artikel [Gegevens verkeer activiteiten](data-factory-data-movement-activities.md#supported-data-stores) voor informatie over het typen en de configuratie van gegevenssets. Bijvoorbeeld als u gegevens uit een Azure SQL-database gebruikt, klikt u op Azure SQL-Database in de lijst met ondersteunde winkels om gedetailleerde informatie weer te geven.  

## <a name="Structure"></a>Structuur van de gegevensset
De sectie **structuur** bepaalt het schema van de gegevensset. Een verzameling namen en gegevenstypen van kolommen bevat.  In het volgende voorbeeld wordt de gegevensset bevat drie kolommen slicetimestamp, projectname en pageviews en ze zijn van het type: tekenreeks, tekenreeks en het decimaalteken respectievelijk.

    structure:  
    [ 
        { "name": "slicetimestamp", "type": "String"},
        { "name": "projectname", "type": "String"},
        { "name": "pageviews", "type": "Decimal"}
    ]

## <a name="Availability"></a>Beschikbaarheid van de gegevensset
De sectie **mogelijkheid om** in een gegevensset definieert het venster verwerking (per uur, dagelijks, wekelijks, enz.) of het segmenteringshulplijnen model voor de gegevensset. Zie [plannen en uitvoeren van](data-factory-scheduling-and-execution.md) een artikel voor meer informatie over het gegevensset segmenteren en afhankelijkheid model. 

De volgende sectie beschikbaarheid geeft aan dat de gegevensset uitvoer de geproduceerd per uur (of) invoer gegevensset per uur beschikbaar is:

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 1   
    }

De volgende tabel worden de eigenschappen die u in de sectie mogelijkheid gebruiken kunt beschreven: 

| Eigenschap | Beschrijving | Vereist | Standaard |
| -------- | ----------- | -------- | ------- |
| frequentie | Hiermee wordt de tijdseenheid voor gegevensset segment productie.<br/><br/>**Ondersteunde frequentie**: minuut, uur, dag, Week, maand | Ja | NB |
| interval | Een vermenigvuldigingsfactor voor frequentie<br/><br/>"X frequentie" bepaalt hoe vaak het segment is geproduceerd.<br/><br/>Als u nodig hebt voor de gegevensset om te worden gesneden per uur worden berekend, stelt u de **frequentie** in **uur**en **interval** **1**.<br/><br/>**Notitie:** Als u de frequentie als minuten opgeeft, wordt aangeraden dat u het interval op niet minder dan 15 instellen | Ja | NB |
| stijl | Hiermee geeft u op of het segment aan het begin/einde van het interval moet worden geproduceerd.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Als frequentie is ingesteld op maand en stijl is ingesteld op EndOfInterval, wordt het segment geproduceerd op de laatste dag van maand. Als u de stijl is ingesteld op StartOfInterval, wordt het segment geproduceerd op de eerste dag van maand.<br/><br/>Als frequentie is ingesteld op dag en stijl is ingesteld op EndOfInterval, wordt het segment geproduceerd in het laatste uur van de dag.<br/><br/>Als frequentie is ingesteld op uur en stijl is ingesteld op EndOfInterval, wordt het segment geproduceerd aan het einde van het uur. Voor een segment voor 1-2 uur periode, bijvoorbeeld het segment geproduceerd om 2 uur. | Nee | EndOfInterval |
| anchorDateTime | Hiermee definieert u de absolute positie in de tijd die wordt gebruikt door scheduler voor het berekenen van de gegevensset segmentgrenzen. <br/><br/>**Notitie:** Als de AnchorDateTime bevat de datumonderdelen die uitgebreider dan de frequentie zijn en vervolgens de uitgebreider onderdelen worden genegeerd. <br/><br/>Als het **interval** **uur** wordt bijvoorbeeld (frequentie: uur en interval: 1) en de **AnchorDateTime** **minuten en seconden**bevat, wordt de **minuten en seconden** onderdelen van de AnchorDateTime worden genegeerd. | Nee | 01/01/0001 |
| verschuiving | Tijdspanne waarmee het begin en einde van alle segmenten van de gegevensset verplaatst. <br/><br/>**Notitie:** Als zowel anchorDateTime en verschuiving zijn opgegeven, is het resultaat de gecombineerde shift. | Nee | NB |

### <a name="offset-example"></a>verschuiving voorbeeld

Dagelijks segmenten die beginnen bij 6 AM in plaats van de standaard-middernacht. 

    "availability":
    {
        "frequency": "Day",
        "interval": 1,
        "offset": "06:00:00"
    }

De **frequentie** is ingesteld op **dag** en **interval** is ingesteld op **1** (één keer per dag): als u wilt dat het segment moet worden geproduceerd om 6 uur in plaats van op het moment standaard: 12 AM. Vergeet niet dat dit moment een UTC-tijd. 

## <a name="anchordatetime-example"></a>Voorbeeld van anchorDateTime

**Voorbeeld:** 23 uur gegevensset segmenten die op 2007 beginnen-04-19T08:00:00

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 23, 
        "anchorDateTime":"2007-04-19T08:00:00"  
    }

## <a name="offsetstyle-example"></a>verschuiving/stijl voorbeeld

Als u nodig gegevensset op maandelijks op specifieke datum en tijd hebt (Stel op 3e van elke maand om 8:00 AM) Gebruik de tag **verschuiving** instellen van de datum en tijd deze moet worden uitgevoerd. 

    {
      "name": "MyDataset",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "availability": {
          "frequency": "Month",
          "interval": 1,
          "offset": "3.08:10:00",
          "style": "StartOfInterval"
        }
      }
    }


## <a name="Policy"></a>Beleid van de gegevensset

De sectie **beleid** in de definitie van de gegevensset Hiermee definieert u de criteria of de voorwaarde die de gegevensset segmenten moeten voldoen.

### <a name="validation-policies"></a>Beleidsregels voor gegevensvalidatie

| De beleidsnaam van het | Beschrijving | Toegepast op | Vereist | Standaard |
| ----------- | ----------- | ---------- | -------- | ------- |
| minimumSizeMB | Gevalideerd of de gegevens in een **Azure blob** voldoet aan de vereisten minimale grootte (in MB). | Azure Blob | Nee | NB |
|minimumRows | Gevalideerd dat de gegevens in een **Azure SQL-database** of een **Azure tabel** de minimale aantal rijen bevat. | <ul><li>Azure SQL-Database</li><li>Azure tabel</li></ul> | Nee | NB

#### <a name="examples"></a>Voorbeelden

**minimumSizeMB:**

    "policy":
    
    {
        "validation":
        {
            "minimumSizeMB": 10.0
        }
    }

**minimumRows**

    "policy":
    {
        "validation":
        {
            "minimumRows": 100
        }
    }

### <a name="external-datasets"></a>Externe gegevenssets

Externe gegevenssets zijn de velden die niet worden geproduceerd door een actieve pijplijn in de fabriek gegevens. Als de gegevensset is gemarkeerd als **externe**, kan het beleid **ExternalData** beïnvloeden van het gedrag van de gegevensset segment beschikbaarheid worden gedefinieerd. 

Tenzij een gegevensset wordt geproduceerd door Azure gegevens Factory, moet deze worden gemarkeerd als **externe**. Deze instelling in het algemeen is van toepassing op de invoer van de eerste activiteit in een pijplijn tenzij activiteit of de verkooppijplijn koppelen wordt gebruikt. 

| Naam | Beschrijving | Vereist | Standaardwaarde  |
| ---- | ----------- | -------- | -------------- |
| dataDelay | Tijd aan het controleren van de beschikbaarheid van de externe gegevens voor het opgegeven segment uitstellen. Als de gegevens elk uur beschikbaar zijn moet, kan er bijvoorbeeld de controleren om te zien van de externe gegevens beschikbaar is en de bijbehorende segment is gereed voor worden uitgesteld met behulp van dataDelay.<br/><br/>Alleen van toepassing op dit moment.  Bijvoorbeeld, als dit nu om 1:00 uur is en deze waarde 10 minuten wordt, begint de validatie om 1:10 uur.<br/><br/>Deze instelling heeft geen invloed op segmenten in het verleden (segmenten met segment eindtijd + dataDelay < nu) zonder eventuele vertraging worden verwerkt.<br/><br/>De tijd is groter is dan 23:59 uur moeten opgegeven met de opmaak day.hours:minutes:seconds. Als u wilt opgeven 24 uur, niet gebruik bijvoorbeeld 24:00:00; Gebruik in plaats daarvan 1.00:00:00. Als u 24:00:00 gebruikt, wordt deze verwerkt als 24 dagen (24.00:00:00). Geef bij 1:04:00:00 voor 1 dag en 4 uur. | Nee | 0 |
| retryInterval | De wachttijd tussen een fout en het volgende proberen poging. Van toepassing is op dit moment; Als de vorige is mislukt probeert, wacht we dit lang na het laatste uitproberen. <br/><br/>Als het nu om 1:00 pm, beginnen we in het eerste uitproberen. Als de duur van de eerste validatie controleren is 1 minuut en de bewerking is mislukt, wordt de volgende poging is aan 1:00 + 1 min (duur) + 1min (interval voor nieuwe pogingen) = 1:02 pm. <br/><br/>Er is geen vertraging voor segmenten in het verleden. De nieuwe poging gebeurt onmiddellijk. | Nee | 00:01:00 (1 minuut) | 
| retryTimeout | De time-out voor nieuwe pogingen.<br/><br/>Als u deze eigenschap is ingesteld op 10 minuten, moet de validatie binnen 10 minuten worden verricht. Als het duurt langer dan 10 minuten aan de gevalideerd, wordt de nieuwe poging time-out.<br/><br/>Als alle pogingen voor de validatie treedt er een time-out, wordt het segment is gemarkeerd als time-out. | Nee | 00:10:00 (10 minuten) |
| maximumRetry | Het aantal keren om de beschikbaarheid van de externe gegevens te controleren. De toegestane maximumwaarde is 10. | Nee | 3 | 

## <a name="scoped-datasets"></a>Bereik gegevenssets
U kunt gegevenssets die zijn beperkt tot een pijplijn met behulp van de eigenschap **gegevenssets** maken. Deze gegevenssets kan alleen worden gebruikt door activiteiten in deze verkooppijplijn, maar niet door activiteiten andere pijpleidingen. Het volgende voorbeeld wordt een pijplijn met twee gegevenssets - InputDataset-verbinding met extern bureaublad en OutputDataset-verbinding met extern bureaublad - moet worden gebruikt in de pijplijn gedefinieerd:  

> [AZURE.IMPORTANT] Bereik gegevenssets worden alleen voor gebruik met eenmalige pijpleidingen (**pipelineMode** ingesteld op **OneTime**) ondersteund. Zie [Onetime pijplijn](data-factory-scheduling-and-execution.md#onetime-pipeline) voor meer informatie.

    {
        "name": "CopyPipeline-rdc",
        "properties": {
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": false
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "InputDataset-rdc"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset-rdc"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1,
                        "style": "StartOfInterval"
                    },
                    "name": "CopyActivity-0"
                }
            ],
            "start": "2016-02-28T00:00:00Z",
            "end": "2016-02-28T00:00:00Z",
            "isPaused": false,
            "pipelineMode": "OneTime",
            "expirationTime": "15.00:00:00",
            "datasets": [
                {
                    "name": "InputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "InputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/input",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": true,
                        "policy": {}
                    }
                },
                {
                    "name": "OutputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "OutputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/output",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": false,
                        "policy": {}
                    }
                }
            ]
        }
    }