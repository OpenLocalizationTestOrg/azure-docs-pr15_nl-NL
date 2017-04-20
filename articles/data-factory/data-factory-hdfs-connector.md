<properties 
    pageTitle="Verplaats gegevens van de on-premises implementatie HDFS | Azure gegevens Factory" 
    description="Meer informatie over het verplaatsen van gegevens van de on-premises implementatie HDFS met Azure gegevens Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Verplaats gegevens van de on-premises implementatie HDFS Azure gegevens Factory gebruiken
In dit artikel wordt beschreven hoe u de activiteit kopiëren in een fabriek Azure gegevens kunt gebruiken om gegevens te verplaatsen vanaf een on-premises implementatie HDFS naar een andere gegevensopslag. In dit artikel is gebaseerd op het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) waarin een algemeen overzicht van de verplaatsing van gegevens met kopie activiteit en ondersteunde store combinaties.

Gegevens factory ondersteunt momenteel alleen zwevend gegevens uit een on-premises implementatie HDFS naar andere winkels gegevens, maar niet voor het verplaatsen van gegevens uit andere winkels gegevens naar een on-premises implementatie HDFS.


## <a name="enabling-connectivity"></a>Connectiviteit inschakelen
Factory-gegevensservice ondersteunt verbindingen met on-premises implementatie HDFS met de Data Management Gateway. Zie [zwevend gegevens tussen on-premises implementatie locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) -artikel voor meer informatie over Data Management Gateway en stapsgewijze instructies voor het instellen van de gateway. Met de gateway verbinding maken met HDFS, zelfs als die wordt gehost in een Azure IaaS VM. 

Terwijl u de gateway op dezelfde lokale computer of de VM Azure als de HDFS installeren kunt, wordt u aangeraden de gateway te installeren op een aparte machine/Azure IaaS VM. Gateway hebt op een aparte computer Hiermee reduceert u bronnen wordt geminimaliseerd en verbeterde prestaties. Wanneer u de gateway op een aparte computer installeert, kunnen de computer toegang tot het systeem met de HDFS moet mogelijk. 


## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om te maken van een pijplijn die gegevens opgehaald uit on-premises implementatie HDFS, is het gebruik van de wizard van de gegevens kopiëren. Zie [Zelfstudie: een pijplijn met de Wizard kopie maken](data-factory-copy-data-wizard-tutorial.md) voor snelle stapsgewijze instructies over het maken van een pijplijn met de wizard kopiëren. 

De volgende voorbeelden bieden definities van een steekproef JSON waarmee u kunt een pijplijn maken met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Gegevens uit een on-premises implementatie HDFS kopiëren naar een Azure-blobopslag worden ze weergegeven. Gegevens kunnen echter worden gekopieerd naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Factory van Azure-gegevens.

## <a name="sample-copy-data-from-on-premises-hdfs-to-azure-blob"></a>Voorbeeld: Gegevens uit lokale HDFS naar Azure Blob kopiëren

In dit voorbeeld ziet hoe u gegevens uit een on-premises implementatie HDFS naar Azure-blobopslag kopiëren. Gegevens kan echter gekopieerde **rechtstreeks** naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Azure gegevens Factory zijn.  
 
Het voorbeeld heeft de volgende gegevens factory entiteiten:

1.  Een gekoppelde service van het type [OnPremisesHdfs](#hdfs-linked-service-properties).
2.  Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Een invoer [dataset](data-factory-create-datasets.md) van het type [bestandsshare](#hdfs-dataset-type-properties).
4.  Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Een [verkooppijplijn](data-factory-create-pipelines.md) met kopie activiteit die wordt gebruikt [FileSystemSource](#hdfs-copy-activity-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

De steekproef kopieert gegevens uit een on-premises implementatie HDFS naar een Azure blob per uur. De JSON-eigenschappen in deze voorbeelden gebruikt, worden beschreven in de secties in de voorbeelden te volgen. 

Als eerste stap van de data management gateway instellen. De instructies in het artikel [zwevend gegevens tussen on-premises implementatie locaties en cloud](data-factory-move-data-between-onprem-and-cloud.md) . 

**HDFS gekoppeld service** Dit voorbeeld wordt de Windows-verificatie. Zie sectie [HDFS gekoppelde service](#hdfs-linked-service-properties) voor verschillende soorten verificatie die u kunt gebruiken. 

    {
        "name": "HDFSLinkedService",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }

**Azure gekoppeld opslagservice**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**HDFS input gegevensset** Deze dataset verwijst naar de map HDFS DataTransfer/UnitTest /. De pijplijn voor het kopiëren van alle bestanden in deze map op de bestemming. 

Instellen "externe": "true" melding aan de Data Factory-service dat de gegevensset de fabriek gegevens buiten en is niet geproduceerd door een activiteit in de fabriek gegevens.
    
    {
        "name": "InputDataset",
        "properties": {
            "type": "FileShare",
            "linkedServiceName": "HDFSLinkedService",
            "typeProperties": {
                "folderPath": "DataTransfer/UnitTest/"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval":  1
            }
        }
    }




**Azure Blob uitvoer gegevensset**

Gegevens worden geschreven naar een nieuwe blob per uur (frequentie: uur, interval: 1). Het pad naar de blob geëvalueerd dynamisch op basis van de begintijd van het segment die wordt verwerkt. Pad naar de map wordt gebruikt voor jaar, maand, dag en uur onderdelen van de begintijd.

    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Pijplijn met activiteit kopiëren**

De pijplijn bevat de activiteit in een kopie die is geconfigureerd voor gebruik van deze invoer- en uitvoerbereik gegevenssets en per uur is gepland. In de pijplijn JSON definitie, het type **bron** is ingesteld op **FileSystemSource** en **sink** type is ingesteld op **BlobSink**. De SQL-query die is opgegeven voor de eigenschap **query** selecteert u de gegevens in de afgelopen uur om te kopiëren.
    
    {
        "name": "pipeline",
        "properties":
        {
            "activities":
            [
                {
                    "name": "HdfsToBlobCopy",
                    "inputs": [ {"name": "InputDataset"} ],
                    "outputs": [ {"name": "OutputDataset"} ],
                    "type": "Copy",
                    "typeProperties":
                    {
                        "source":
                        {
                            "type": "FileSystemSource"
                        },
                        "sink":
                        {
                            "type": "BlobSink"
                        }
                    },
                    "policy":
                    {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "00:05:00"
                    }
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="hdfs-linked-service-properties"></a>Eigenschappen van de gekoppelde HDFS-Service

De volgende tabel bevat de omschrijving voor JSON-elementen die specifiek zijn voor HDFS service die is gekoppeld.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- | 
| type | De eigenschap type moet zijn ingesteld op: **Hdfs** | Ja | 
| URL | URL voor de HDFS | Ja |
| encryptedCredential | [Nieuw-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) -uitvoer van de access-referentie. | Nee |
| Gebruikersnaam | Gebruikersnaam voor Windows-verificatie. | Ja (voor Windows-verificatie)
| wachtwoord | Wachtwoord voor Windows-verificatie. | Ja (voor Windows-verificatie)
| authenticationType | Windows, of anonieme. | Ja |
| gatewayName | De naam van de gateway die de gegevens Factory-service verbinding maken met de HDFS moet gebruiken. | Ja |   

Zie [instelling referenties en beveiliging](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) voor meer informatie over het instellen van referenties voor on-premises implementatie HDFS.

### <a name="using-anonymous-authentication"></a>Anonieme verificatie gebruikt

    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Anonymous",
                "userName": "hadoop",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }


### <a name="using-windows-authentication"></a>Windows-verificatie
    
    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }
 


## <a name="hdfs-dataset-type-properties"></a>Eigenschappen van HDFS gegevensset

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON lijken voor alle gegevensset typen (Azure SQL Azure blob, Azure tabel, enz.).

De sectie **typeProperties** verschilt voor elk type gegevensset en vindt u informatie over de locatie van de gegevens in de gegevensopslag. De sectie typeProperties voor dataset van het type **bestandsshare** (waaronder HDFS gegevensset) heeft de volgende eigenschappen

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
Mappad | Pad naar de map. Voorbeeld:`myfolder`<br/><br/>Gebruik escape-teken ' \ ' voor speciale tekens in de tekenreeks. Bijvoorbeeld: map opgeeft voor folder\subfolder,\\\\submap en voor d:\samplefolder, geef d:\\\\VoorbldMap.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** moet paden op basis van segment begin/einde-datums en tijden. | Ja
fileName | Geef de naam van het bestand in de **mappad** desgewenst kunt u de tabel om te verwijzen naar een specifieke bestand in de map. Als u elke waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Wanneer de bestandsnaam niet wordt opgegeven voor een gegevensset uitvoer, wordt de naam van het gegenereerde bestand zou in de volgende indeling: <br/><br/>Gegevens. <Guid>.txt (bijvoorbeeld:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | Nee
partitionedBy | partitionedBy kan worden gebruikt om op te geven van een dynamische mappad, bestandsnaam voor time reeksgegevens. Voorbeeld: mappad parameters voor elk uur van gegevens. | Nee
fileFilter | Geef een filter moet worden gebruikt om een subset van bestanden in de mappad in plaats van alle bestanden te selecteren. <br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (willekeurig teken dat).<br/><br/>Voorbeeld 1:`"fileFilter": "*.log"`<br/>Voorbeeld 2:`"fileFilter": 2014-1-?.txt"`<br/><br/>**Opmerking**: fileFilter geldt voor een invoer bestandsshare gegevensset | Nee
| indeling | De volgende indelingstypen worden ondersteund: **tekstopmaak**, **AvroFormat** **JsonFormat**, **OrcFormat**en **ParquetFormat**. Stel de eigenschap **type** onder Opmaak op een van deze waarden. Zie [Tekstopmaak precisie](#specifying-textformat), [AvroFormat precisie](#specifying-avroformat) [Precisie JsonFormat](#specifying-jsonformat), [OrcFormat opgeven](#specifying-orcformat)en [Precisie ParquetFormat](#specifying-parquetformat) secties voor meer informatie. Als u bestanden wilt kopiëren als-is tussen bestand gebaseerde winkels (binaire kopie), kunt u de sectie indeling in de definities van beide invoer- en uitvoerbereik gegevensset overslaan. | Nee 
| compressie | Geef het type en compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, en **BZip2** en ondersteunde niveaus zijn: **optimale** en **snelst**. Compressie-instellingen worden momenteel niet ondersteund voor gegevens in **AvroFormat** of **OrcFormat**. Voor meer informatie, Zie de sectie [compressie-ondersteuning](#compression-support) .  | Nee |



> [AZURE.NOTE] bestandsnaam en fileFilter kunnen niet tegelijkertijd worden gebruikt.


### <a name="using-partionedby-property"></a>De eigenschap partionedBy

Zoals vermeld in de vorige sectie, kunt u een dynamische mappad, filename voor reeks tijdgegevens met partitionedBy opgeven. U kunt dit doen met de gegevens Factory-macro's en de systeemvariabele SliceStart, SliceEnd die de logische periode voor een bepaalde gegevens segment aangeven. 

Meer informatie over tijd reeks gegevenssets, raadpleegt plannen en segmenten, u [Gegevenssets maken](data-factory-create-datasets.md), [planning en uitvoering](data-factory-scheduling-and-execution.md)en [Pijpleidingen maken](data-factory-create-pipelines.md) artikelen. 

#### <a name="sample-1"></a>Voorbeeld 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

In dit voorbeeld {segment} wordt vervangen door de waarde van gegevens Factory systeemvariabele SliceStart in de indeling (YYYYMMDDHH) opgegeven. De SliceStart verwijst om de begintijd van het segment. De mappad verschilt voor elk segment. Bijvoorbeeld: wikidatagateway/wikisampledataout/2014100103 of wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Voorbeeld 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

In dit voorbeeld worden jaar, maand, dag en tijd van SliceStart uitgepakt in afzonderlijke variabelen die worden gebruikt door eigenschappen mappad en de bestandsnaam.

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="hdfs-copy-activity-type-properties"></a>HDFS kopie activiteit type eigenschappen

Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en beleidsregels zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren echter met elk activiteitstype. Ze varieert afhankelijk van de soorten bronnen en sinks voor een activiteit kopiëren.

Voor een activiteit kopiëren als bron van het type **FileSystemSource** zijn de volgende eigenschappen beschikbaar in de sectie typeProperties:

**FileSystemSource** ondersteunt de volgende eigenschappen:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------------- | -------- |
| recursieve | Geeft aan of de gegevens of wordt gelezen recursief uit de submappen alleen uit de opgegeven map. | Waar, ONWAAR (standaard)| Nee |

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Prestaties en optimaliseren  
Zie [kopie activiteit prestaties & handleiding optimaliseren](data-factory-copy-activity-performance.md) voor meer informatie over belangrijke factoren die invloed prestaties van gegevens verplaatsen (kopie activiteit) in Factory van Azure-gegevens en verschillende manieren optimaliseren.

