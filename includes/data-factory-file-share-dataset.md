## <a name="fileshare-dataset-type-properties"></a>Eigenschappen van bestandsshare gegevensset

Zie het artikel [gegevenssets maken](../articles/data-factory/data-factory-create-datasets.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON lijken voor alle typen van de gegevensset. 

De sectie **typeProperties** verschilt voor elk type gegevensset. Deze vindt u informatie die specifiek is voor het type gegevensset. De sectie typeProperties voor een gegevensset van het type **bestandsshare** gegevensset heeft de volgende eigenschappen:

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
Mappad | Subpad naar de map. Gebruik escape-teken ' \ ' voor speciale tekens in de tekenreeks. Zie de [steekproef gekoppeld service en gegevensset definities](#sample-linked-service-and-dataset-definitions) voor voorbeelden.<br/><br/>U kunt deze eigenschap combineren met **partitionBy** moet paden op basis van segment begin/einde-datums en tijden. | Ja
fileName | Geef de naam van het bestand in de **mappad** desgewenst kunt u de tabel om te verwijzen naar een specifieke bestand in de map. Als u elke waarde voor deze eigenschap niet opgeeft, wordt de tabel verwijst naar alle bestanden in de map.<br/><br/>Wanneer de bestandsnaam niet wordt opgegeven voor een gegevensset uitvoer, wordt de naam van het gegenereerde bestand zou in de volgende indeling: <br/><br/>Gegevens. <Guid>.txt (voorbeeld: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | Nee
partitionedBy | partitionedBy kan worden gebruikt om op te geven van een dynamische mappad, bestandsnaam voor time reeksgegevens. Bijvoorbeeld mappad parameters voor elk uur van gegevens. | Nee
Indeling | De volgende indelingstypen worden ondersteund: **tekstopmaak**, **AvroFormat** **JsonFormat**en **OrcFormat**. Stel de eigenschap **type** onder Opmaak op een van deze waarden. Zie [Tekstopmaak precisie](#specifying-textformat), [Precisie AvroFormat](#specifying-avroformat) [JsonFormat precisie](#specifying-jsonformat)en [Precisie OrcFormat](#specifying-orcformat) secties voor meer informatie. Als u bestanden wilt kopiëren als-is tussen bestand gebaseerde winkels (binaire kopie), kunt u de sectie indeling in de definities van beide invoer- en uitvoerbereik gegevensset overslaan. | Nee
fileFilter | Geef een filter moet worden gebruikt om een subset van bestanden in de mappad in plaats van alle bestanden te selecteren.<br/><br/>Toegestane waarden zijn: `*` (meerdere tekens) en `?` (willekeurig teken dat).<br/><br/>Voorbeeld 1:`"fileFilter": "*.log"`<br/>Voorbeeld 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter is van toepassing voor een invoer bestandsshare gegevensset. Deze eigenschap wordt niet ondersteund met HDFS.  | Nee
| compressie | Geef het type en compressie van de gegevens. Ondersteunde typen zijn: **GZip**, **Deflate**, en **BZip2** en ondersteunde niveaus zijn: **optimale** en **snelst**. Compressie-instellingen worden momenteel niet ondersteund voor gegevens in **AvroFormat** of **OrcFormat**. Voor meer informatie, Zie de sectie [compressie-ondersteuning](#compression-support) .  | Nee |
| useBinaryTransfer | Opgeven of binaire doorverbinden-modus gebruiken. Waar voor binaire modus en ONWAAR ASCII. Standaardwaarde: True. Deze eigenschap kan alleen worden gebruikt wanneer gekoppeld gekoppelde servicetype type: FtpServer. | Nee | 
 

> [AZURE.NOTE] bestandsnaam en fileFilter kunnen niet tegelijkertijd worden gebruikt.

### <a name="using-partionedby-property"></a>De eigenschap partionedBy

Zoals vermeld in de vorige sectie, kunt u een dynamische mappad, bestandsnaam voor reeks tijdgegevens met partitionedBy opgeven. U kunt dit doen met de gegevens Factory-macro's en de systeemvariabele SliceStart, SliceEnd die de logische periode voor een bepaalde gegevens segment aangeven. 

Zie voor meer informatie over tijd reeks gegevenssets, plannen en segmenten, [Gegevenssets maken](../articles/data-factory/data-factory-create-datasets.md), [planning en uitvoering](../articles/data-factory/data-factory-scheduling-and-execution.md)en [Pijpleidingen maken](../articles/data-factory/data-factory-create-pipelines.md) artikelen. 

#### <a name="sample-1"></a>Voorbeeld 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

In dit voorbeeld {segment} wordt vervangen door de waarde van gegevens Factory systeemvariabele SliceStart in de indeling (YYYYMMDDHH) opgegeven. De SliceStart verwijst om de begintijd van het segment. De mappad verschilt voor elk segment. Voorbeeld: wikidatagateway/wikisampledataout/2014100103 of wikidatagateway/wikisampledataout/2014100104.

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
