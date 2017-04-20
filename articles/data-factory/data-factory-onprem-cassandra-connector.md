<properties 
    pageTitle="Verplaats gegevens van Cassandra met gegevens Factory | Microsoft Azure" 
    description="Meer informatie over het verplaatsen van gegevens uit een on-premises implementatie Cassandra-database met Azure gegevens Factory." 
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
    ms.date="09/07/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-an-on-premises-cassandra-database-using-azure-data-factory"></a>Verplaats gegevens van een on-premises implementatie Cassandra-database met behulp van Azure gegevens Factory
In dit artikel wordt beschreven hoe u de activiteit kopiëren in een fabriek Azure gegevens kunt gebruiken om gegevens te kopiëren vanuit een on-premises implementatie Cassandra-database naar een gegevensopslag vermeld onder Sink kolom in de sectie [bronnen ondersteund en Sinks](data-factory-data-movement-activities.md#supported-data-stores) . In dit artikel is gebaseerd op het artikel [gegevens verkeer activiteiten](data-factory-data-movement-activities.md) , waarbij een een algemeen overzicht van de verplaatsing van gegevens met kopie activiteit en ondersteunde store combinaties wordt weergegeven.

Gegevens factory ondersteunt momenteel alleen zwevend gegevens uit een database Cassandra [sink gegevens winkels ondersteund](data-factory-data-movement-activities.md#supported-data-stores), maar niet zwevend gegevens uit andere winkels gegevens met een Cassandra-database.

## <a name="prerequisites"></a>Vereisten voor
Voor de service Factory van Azure-gegevens kunnen verbinding maken met uw on-premises implementatie Cassandra database, moet u het volgende: 

- Data Management Gateway 2.0 of hoger op dezelfde computer waarop de database of op een aparte computer om te voorkomen dat voor resources met de database. Data Management Gateway is een software die on-premises gegevensbronnen met cloudservices in een veilige en beheerde manier verbindt. Zie [gegevens tussen on-premises en cloud verplaatsen](data-factory-move-data-between-onprem-and-cloud.md) artikel voor meer informatie over Data Management Gateway.
  
    Wanneer u de gateway installeert, installeert deze automatisch een Microsoft Cassandra ODBC-stuurprogramma verbinding maken met Cassandra database gebruikt. 

> [AZURE.NOTE] Zie [problemen oplossen met de gateway problemen](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) voor tips over het oplossen van verbinding/gateway gerelateerde problemen. 

## <a name="copy-data-wizard"></a>Wizard gegevens kopiëren
De eenvoudigste manier om te maken van een pijplijn waarmee gegevens uit een database Cassandra gekopieerd naar een van de ondersteunde sink gegevensarchieven is het gebruik van de wizard van de gegevens kopiëren. Zie [Zelfstudie: een pijplijn met de Wizard kopie maken](data-factory-copy-data-wizard-tutorial.md) voor snelle stapsgewijze instructies over het maken van een pijplijn met de wizard kopiëren. 

Het volgende voorbeeld bevat de definities van een steekproef JSON waarmee u kunt een pijplijn maken met behulp van [Azure-portal](data-factory-copy-activity-tutorial-using-azure-portal.md) of [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) of [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Het kopiëren van gegevens uit een database Cassandra naar Azure-blobopslag worden ze weergegeven. Gegevens kunnen echter worden gekopieerd naar een van de sinks vermelde [hier](data-factory-data-movement-activities.md#supported-data-stores) gebruik van de activiteit kopiëren in Factory van Azure-gegevens.   


## <a name="sample-copy-data-from-cassandra-to-blob"></a>Voorbeeld: Gegevens uit Cassandra naar Blob kopiëren
De steekproef kopieert gegevens uit een database Cassandra naar een Azure blob per uur. De JSON-eigenschappen in deze voorbeelden gebruikt, worden beschreven in de secties in de voorbeelden te volgen. Gegevens kunnen worden gekopieerd rechtstreeks naar een van de gootstenen vermeld in het artikel [Gegevens verkeer activiteiten](data-factory-data-movement-activities.md#supported-data-stores) met behulp van de activiteit kopiëren in fabriek van Azure-gegevens. 

- Een gekoppelde service van het type [OnPremisesCassandra](#onpremisescassandra-linked-service-properties).
- Een gekoppelde service van het type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Een invoer [dataset](data-factory-create-datasets.md) van het type [CassandraTable](#cassandratable-properties).
- Een uitvoer [dataset](data-factory-create-datasets.md) van het type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- Een [verkooppijplijn](data-factory-create-pipelines.md) met kopie activiteit die wordt gebruikt [CassandraSource](#cassandrasource-type-properties) en [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

**Cassandra gekoppeld-service**

Dit voorbeeld wordt de service **Cassandra** gekoppeld. Zie sectie [Cassandra gekoppelde service](#onpremisescassandra-linked-service-properties) voor de eigenschappen die worden ondersteund door deze gekoppelde service.  

    {
        "name": "CassandraLinkedService",
        "properties":
        {
            "type": "OnPremisesCassandra",
            "typeProperties":
            {
                "authenticationType": "Basic",
                "host": "mycassandraserver",
                "port": 9042,
                "username": "user",
                "password": "password",
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

**Cassandra invoer gegevensset**

    {
        "name": "CassandraInput",
        "properties": {
            "linkedServiceName": "CassandraLinkedService",
            "type": "CassandraTable",
            "typeProperties": {
                "tableName": "mytable",
                "keySpace": "mykeyspace" 
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

Als u **externe** op **true** , wordt de Data Factory-service geïnformeerd dat de gegevensset de fabriek gegevens buiten en is niet geproduceerd door een activiteit in de fabriek gegevens.

**Azure Blob uitvoer gegevensset**

Gegevens worden geschreven naar een nieuwe blob per uur (frequentie: uur, interval: 1). 

    {
        "name": "AzureBlobOutput",
        "properties":
        {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties":
            {
                "folderPath": "adfgetstarted/fromcassandra"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Pijplijn met activiteit kopiëren**

De pijplijn bevat de activiteit in een kopie die is geconfigureerd voor gebruik van de invoer- en uitvoerbereik gegevenssets en per uur is gepland. In de pijplijn JSON definitie, het type **bron** is ingesteld op **CassandraSource** en **sink** type is ingesteld op **BlobSink**. 

Zie [RelationalSource type eigenschappen](#cassandrasource-type-properties) voor de lijst met eigenschappen die worden ondersteund door de RelationalSource. 
    
    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2016-06-01T18:00:00",
            "end":"2016-06-01T19:00:00",
            "description":"pipeline with copy activity",
            "activities":[  
            {
                "name": "CassandraToAzureBlob",
                "description": "Copy from Cassandra to an Azure blob",
                "type": "Copy",
                "inputs": [
                {
                    "name": "CassandraInput"
                }
                ],
                "outputs": [
                {
                    "name": "AzureBlobOutput"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "CassandraSource",
                        "query": "select id, firstname, lastname from mykeyspace.mytable"
        
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "OldestFirst",
                    "retry": 0,
                    "timeout": "01:00:00"
                }
            }
            ]   
        }
    }
## <a name="onpremisescassandra-linked-service-properties"></a>Eigenschappen van de service OnPremisesCassandra die zijn gekoppeld

De volgende tabel bevat een beschrijving voor de JSON-elementen die specifiek zijn voor de service Cassandra die zijn gekoppeld.

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- | 
| type | De eigenschap type moet zijn ingesteld op: **OnPremisesCassandra** | Ja |
| host | Een of meer IP-adressen of hostnamen van Cassandra-servers.<br/><br/>Geef een door komma's gescheiden lijst met IP-adressen of hostnamen gelijktijdig verbinding maakt met alle servers. | Ja | 
| poort | De TCP-poorten waarop de server Cassandra luistert voor clientverbindingen. | Nee, standaardwaarde: 9042 |
| authenticationType | Eenvoudige of anonieme | Ja |
| gebruikersnaam |Geef de gebruikersnaam voor de gebruikersaccount. | Ja, als authenticationType is ingesteld op Basic. |
| wachtwoord | Wachtwoord voor het gebruikersaccount opgeven.  | Ja, als authenticationType is ingesteld op Basic. |
| gatewayName | De naam van de gateway die wordt gebruikt om te verbinden met de on-premises implementatie Cassandra-database. | Ja |
| encryptedCredential | Referentie versleuteld door de gateway. | Nee | 

## <a name="cassandratable-properties"></a>CassandraTable eigenschappen

Zie het artikel [gegevenssets maken](data-factory-create-datasets.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van gegevenssets. Secties zoals structuur, beschikbaarheid en beleid van een gegevensset JSON lijken voor alle gegevensset typen (Azure SQL Azure blob, Azure tabel, enz.).

De sectie **typeProperties** verschilt voor elk type gegevensset en vindt u informatie over de locatie van de gegevens in de gegevensopslag. De sectie typeProperties voor dataset van het type **CassandraTable** heeft de volgende eigenschappen

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| keyspace | De naam van de keyspace of schema in Cassandra-database. | Ja (als de **query** voor **CassandraSource** wordt niet gedefinieerd). |
| Tabelnaam | Naam van de tabel in Cassandra database. | Ja (als de **query** voor **CassandraSource** wordt niet gedefinieerd). |


## <a name="cassandrasource-type-properties"></a>CassandraSource type eigenschappen
Zie het artikel [Pijpleidingen maken](data-factory-create-pipelines.md) voor een volledige lijst met secties en eigenschappen die beschikbaar zijn voor het definiëren van activiteiten. Eigenschappen zoals naam, beschrijving, invoer en uitvoer tabellen en beleid zijn beschikbaar voor alle typen activiteiten. 

Eigenschappen die beschikbaar zijn in de sectie typeProperties van de activiteit variëren echter met elk activiteitstype. Ze varieert afhankelijk van de soorten bronnen en sinks voor een activiteit kopiëren.

Als bron van het type **CassandraSource**, is de volgende eigenschappen zijn beschikbaar in de sectie typeProperties:

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------------- | -------- |
| query | De aangepaste query gebruiken om gegevens te lezen. | SQL-92 query of CQL query. Raadpleeg [referentie voor CQL](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/cqlReferenceTOC.html). <br/><br/>Wanneer u met SQL-query, geef **keyspace name.table naam** voor de tabel die u wilt opvragen. | Nee (als de tabelnaam en keyspace op gegevensset zijn gedefinieerd).  |
| consistencyLevel | Het niveau van de consistentie geeft aan hoeveel replica's moeten reageren op een leesbevestiging om gegevens terug te gaan met de clienttoepassing. Cassandra wordt het opgegeven aantal replica's voor gegevens om te voldoen aan de gelezen aanvraag gecontroleerd. | EEN, TWEE, DRIE, QUORUM, ALLE, LOCAL_QUORUM, EACH_QUORUM, LOCAL_ONE. Zie [consistentie van de gegevens configureren](http://docs.datastax.com/en//cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) voor meer informatie. | Nee. Standaardwaarde is een. |  


### <a name="type-mapping-for-cassandra"></a>De toewijzing van het type voor Cassandra
Cassandra Type | .NET op basis van Type
--------------- | ---------------
ASCII | Tekenreeks 
BIGINT | Int64
BLOB | Byte]
BOOLEAANSE WAARDE | Booleaanse waarde
DECIMAAL | Decimaal
DUBBELTIK | Dubbeltik
LATEN ZWEVEN | Eén
INET | Tekenreeks
INT | Int32
TEKST | Tekenreeks
TIJDSTEMPEL | Datum /
TIMEUUID | GUID
UUID | GUID
VARCHAR | Tekenreeks
VARINT | Decimaal

> [AZURE.NOTE]  
> Voor de siteverzameling raadpleegt u typen (kaart, instellen, lijst, enzovoort), [werken met Cassandra siteverzameling typen met behulp van virtuele tabel](#work-with-collections-using-virtual-table) sectie. 
> 
> De gebruiker gedefinieerde gegevenstypen worden niet ondersteund.
> 
> De lengte van binaire kolommen en tekenreeks met lengte berekend mogen niet groter is dan 4000. 

## <a name="work-with-collections-using-virtual-table"></a>Werken met collecties met behulp van virtuele tabel
Azure gegevens Factory gebruikt een ingebouwde ODBC-stuurprogramma verbinding maken met en gegevens van uw database Cassandra kopiëren. Voor siteverzameling typen inclusief kaart, instellen en lijst, Normaliseert het stuurprogramma voor de gegevens opnieuw naar de bijbehorende virtuele tabellen. Als een tabel de kolommen van een siteverzameling bevat, genereert het stuurprogramma voor specifieke taken in de volgende virtuele tabellen:

-   Een **basistabel**, waarin dezelfde gegevens als de echte tafel, behalve voor de kolommen van de siteverzameling. De basistabel gebruikt dezelfde naam als de echte tafel die deze weergeeft.
-   Een **virtuele tabel** voor elke kolom collectie, die wordt uitgebreid met de geneste gegevens. De virtuele tabellen die verzamelingen vertegenwoordigen zijn benoemde met de naam van de echte tafel, een scheidingsteken '_vt_' en de naam van de kolom.

Virtuele tabellen raadpleegt u de gegevens in de echte tafel, waardoor het stuurprogramma voor toegang tot de Gedenormaliseerde gegevens. Zie de sectie Voorbeeld voor meer informatie. U kunt de inhoud van Cassandra collecties openen door query's uitvoeren en deelnemen aan de virtuele tabellen.

U kunt gebruikmaken van de [Wizard kopiëren](data-factory-data-movement-activities.md#data-factory-copy-wizard) om weer te geven intuïtief de lijst met tabellen in Cassandra database, inclusief de virtuele tabellen en de gegevens in een voorbeeld bekijken. U kunt ook een query in de Wizard kopie maken en valideren van het resultaat wilt weergeven.

### <a name="example"></a>Voorbeeld
De volgende 'ExampleTable' is bijvoorbeeld een databasetabel Cassandra met een geheel getal primaire-sleutelkolom met de naam 'pk_int', een tekstkolom naamwaarde een lijstkolom, een kolom kaart en een set kolom (ook wel 'StringSet' genoemd).

pk_int | Waarde | Lijst | Plattegrond |   StringSet
------ | ----- | ---- | --- | --------
1 | "Voorbeeldwaarde 1" | ["1", "2", "3"]  | {"V1": 'A","v2': "b"} | {"A", "B", "C"}
3 | "Voorbeeldwaarde 3" | ["100", "101", "102", "105"] | {"V1": "t"} | {"A", "E"}

Het stuurprogramma zou meerdere virtuele tabellen bevatten deze één tabel te genereren. De refererende sleutel kolommen in de virtuele tabellen verwijzen naar de primaire sleutel kolommen in de echte tafel en aangeven welke echte tafel rij de virtuele tabelrij komt met overeen. 

De eerste virtuele tabel is de tabel met de naam 'ExampleTable' wordt weergegeven in de volgende tabel. De basistabel bevat dezelfde gegevens als de oorspronkelijke databasetabel, behalve voor de collecties, die zijn weggelaten uit deze tabel en uitgevouwen in andere virtuele tabellen.

pk_int | Waarde
------ | -----
1 | "Voorbeeldwaarde 1"
3 | "Voorbeeldwaarde 3"

De volgende tabellen bevatten de virtuele tabellen die de gegevens uit de lijst, Map en StringSet kolommen renormalize. De kolommen met namen die met "_index" of "_key eindigen" geven de positie van de gegevens in de oorspronkelijke lijst of map. De kolommen met namen die met "_Waarde eindigen" bevatten de uitgevouwen gegevens van de siteverzameling.

#### <a name="table-exampletablevtlist"></a>Tabel 'ExampleTable_vt_List':
pk_int | List_index | List_value
------ | ---------- | ----------
1 | 0 | 1
1 | 1 | 2
1 | 2 | 3
3 | 0 | 100
3 | 1 | 101
3 | 2 | 102
3 | 3 | 103

#### <a name="table-exampletablevtmap"></a>Tabel 'ExampleTable_vt_Map':
pk_int | Map_key | Map_value
------ | ------- | ---------
1 | S1 | A
1 | S2 | b
3 | S1 | t

#### <a name="table-exampletablevtstringset"></a>Tabel 'ExampleTable_vt_StringSet':
pk_int | StringSet_value
------ | ---------------
1 | A
1 | B
1 | C
3 | A
3 | E





[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]
[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Prestaties en optimaliseren  
Zie [kopie activiteit prestaties & handleiding optimaliseren](data-factory-copy-activity-performance.md) voor meer informatie over belangrijke factoren die invloed prestaties van gegevens verplaatsen (kopie activiteit) in Factory van Azure-gegevens en verschillende manieren optimaliseren.
