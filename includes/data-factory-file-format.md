## <a name="specifying-formats"></a>Indelingen opgeven

Azure gegevens Factory ondersteunt de volgende notatietypen: 

- [Tekst opmaken](#specifying-textformat)
- [Indeling van JSON](#specifying-jsonformat)
- [Avro opmaken](#specifying-avroformat)
- [ORC opmaken](#specifying-orcformat)
- [Parketvloeren opmaken](#specifying-parquetformat)

### <a name="specifying-textformat"></a>Tekstopmaak opgeven

Als de notatie is ingesteld op **tekstopmaak**, kunt u de volgende **optionele** eigenschappen opgeven in de sectie **indeling** .

| Eigenschap | Beschrijving | Toegestane waarden | Vereist |
| -------- | ----------- | -------- | -------- | 
| columnDelimiter | Het teken dat wordt gebruikt om kolommen in een bestand te scheiden. | Slechts één teken is toegestaan. De standaardwaarde is door komma's (','). <br/><br/>Als u wilt gebruiken in een Unicode-teken, raadpleegt u [Unicode-tekens](https://en.wikipedia.org/wiki/List_of_Unicode_characters) om de bijbehorende code voor deze. U kunt bijvoorbeeld een uitzonderlijke niet-afdrukbare tekens voor welke kans bestaat niet in uw gegevens gebruiken: "\u0001" dat staat voor starten van kop (Statusverklaring) opgeven. | Nee |
| rowDelimiter | Het teken dat wordt gebruikt om rijen in een bestand te scheiden. | Slechts één teken is toegestaan. De standaardwaarde is een van de volgende waarden op lezen: ["\r\n", "\r", "\n"] en "\r\n" op schrijven. | Nee |
| escapeChar | Het speciale teken dat wordt gebruikt om een scheidingsteken voor kolommen in de inhoud van de invoer bestand. <br/><br/>U kunt geen escapeChar zowel quoteChar voor een tabel opgeven. | Slechts één teken is toegestaan. Geen standaardwaarde. <br/><br/>Voorbeeld: als er een komma (', ') als scheidingsteken voor de kolommen, maar u wilt dat het teken dat door komma's in de tekst (voorbeeld: "Hallo, wereld"), u kunt '$' als de escape-teken definiëren en gebruiken van de tekenreeks "$Hallo, wereld" in de bron. | Nee | 
| quoteChar | Het teken aan een tekenreekswaarde van offerte. De kolom en rij scheidingstekens binnen de offerte tekens zou worden beschouwd als onderdeel van de tekenreekswaarde. Deze eigenschap is van toepassing op invoer- en uitvoerbereik gegevenssets.<br/><br/>U kunt geen escapeChar zowel quoteChar voor een tabel opgeven. | Slechts één teken is toegestaan. Geen standaardwaarde. <br/><br/>Als u de komma hebben bijvoorbeeld (', ') als scheidingsteken voor de kolommen, maar u wilt laten kommateken in de tekst (voorbeeld: < Hallo, wereld >), kunt u "(dubbele aanhalingstekens) als de offerte teken en het gebruik van de tekenreeks"Hallo, wereld"in de bron. | Nee |
| nullValue | Een of meer tekens gebruikt voor een null-waarde. | Een of meer tekens. De standaardwaarden zijn "\N" en "NULL" op gelezen en "\N" op schrijven. | Nee |
| encodingName | Geef de naam van de codering. | Een geldig codering naam. Zie [Encoding.EncodingName eigenschap](https://msdn.microsoft.com/library/system.text.encoding.aspx). Voorbeeld: windows-1250 of shift_jis. De standaardwaarde is UTF-8. | Nee | 
| firstRowAsHeader | Geeft aan of houd rekening met de eerste rij als koptekst. Voor een gegevensset invoer leest gegevens Factory eerste rij als koptekst. Voor een gegevensset uitvoer schrijft gegevens Factory eerste rij als koptekst. <br/><br/>Zie [scenario's voor het gebruik van **firstRowAsHeader** en **skipLineCount** ](#scenarios-for-using-firstrowasheader-and-skiplinecount) voor voorbeeldscenario's. | Waar<br/>ONWAAR (standaard) | Nee |
| skipLineCount | Geeft het aantal rijen overgeslagen tijdens het lezen van gegevens uit bestanden met invoer. Als zowel skipLineCount en firstRowAsHeader zijn opgegeven, wordt de regels eerst worden overgeslagen en vervolgens de informatie over het toevoegen van bestand wordt gelezen. <br/><br/>Zie [scenario's voor het gebruik van firstRowAsHeader en skipLineCount](#scenarios-for-using-firstrowasheader-and-skiplinecount) voor voorbeeldscenario's. | Geheel getal | Nee | 
| treatEmptyAsNull | Geeft aan of lege tekenreeks behandelen als een null-waarde tijdens het lezen van gegevens uit een invoer-bestand. | True (standaard)<br/>ONWAAR | Nee |  

#### <a name="textformat-example"></a>Voorbeeld van de tekstopmaak
In het onderstaande voorbeeld ziet u enkele eigenschappen opmaken voor tekstopmaak.

    "typeProperties":
    {
        "folderPath": "mycontainer/myfolder",
        "fileName": "myblobname"
        "format":
        {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";",
            "quoteChar": "\"",
            "NullValue": "NaN"
            "firstRowAsHeader": true,
            "skipLineCount": 0,
            "treatEmptyAsNull": true
        }
    },

Als u wilt een escapeChar gebruiken in plaats van quoteChar, kunt u de regel vervangen door quoteChar met de volgende escapeChar:

    "escapeChar": "$",



### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>Scenario's voor het gebruik van firstRowAsHeader en skipLineCount

- U kopieert de bron van een niet-bestand naar een tekstbestand en wilt toevoegen van een kopregel die de schemametagegevens bevat (bijvoorbeeld: SQL schema). Geef **firstRowAsHeader** als waar in de uitvoer gegevensset in dit scenario. 
- U kopieert vanuit een tekstbestand met een kopregel naar een niet-bestandssink en wilt deze regel neerzetten. **FirstRowAsHeader** opgeven als waar in de invoer gegevensset.
- U kopieert vanuit een tekstbestand en wilt u een paar regels aan het begin die geen gegevens of kop informatie bevatten overslaan. Geef **skipLineCount** om aan te geven van het aantal regels worden overgeslagen. Als de rest van het bestand een kopregel bevat, kunt u ook **firstRowAsHeader**opgeven. Als zowel **skipLineCount** en **firstRowAsHeader** zijn opgegeven, de regels eerst worden overgeslagen en wordt de kopgegevens gelezen uit bestand

### <a name="specifying-avroformat"></a>AvroFormat opgeven
Als de notatie is ingesteld op AvroFormat, hoeft u niet kunt u de eigenschappen in de sectie indeling in de sectie typeProperties opgeven. Voorbeeld:

    "format":
    {
        "type": "AvroFormat",
    }

Als u wilt Avro opmaken in een tabel component gebruikt, kunt u verwijzen naar [de Apache component zelfstudie](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).

Houd rekening met de volgende punten:  

- [Complexe gegevenstypen](http://avro.apache.org/docs/current/spec.html#schema_complex) worden niet ondersteund (records, vaste teksten, matrices, kaarten, samenvoegingen en vaste)

### <a name="specifying-jsonformat"></a>JsonFormat opgeven

Als de notatie is ingesteld op **JsonFormat**, kunt u de volgende **optionele** eigenschappen opgeven in de sectie **indeling** .

| Eigenschap | Beschrijving | Vereist |
| -------- | ----------- | -------- |
| filePattern | Geven aan het patroon van gegevens die zijn opgeslagen in elk JSON-bestand. Toegestane waarden zijn: **setOfObjects** en **arrayOfObjects**. **De standaardwaarde** is: **setOfObjects**. Zie uit te voeren voor meer informatie over deze patronen.| Nee |
| encodingName | Geef de naam van de codering. Zie voor de lijst met geldige codering namen: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) eigenschap. Bijvoorbeeld: windows-1250 of shift_jis. **De standaardwaarde** is: **UTF-8**. | Nee | 
| nestingSeparator | Teken dat wordt gebruikt voor het scheiden van geneste niveaus. De standaardwaarde is '.' (punt). | Nee | 


#### <a name="setofobjects-file-pattern"></a>setOfObjects bestandspatroon

Elk bestand bevat één object of lijn-scheidingsteken/samengevoegd meerdere objecten. Wanneer deze optie is gekozen in een gegevensset uitvoer, levert kopie activiteit één JSON-bestand met elk object per regel (lijn is scheidingsteken).

**één object** 

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }

**lijn is scheidingsteken JSON** 

    {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
    {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
    {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"789037573","callingnum2":"789044691","switch1":"UK","switch2":"Australia"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789044691","switch1":"US","switch2":"Australia"}

**samengevoegde JSON**

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }
    {
        "time": "2015-04-29T07:13:21.0220000Z",
        "callingimsi": "466922202613463",
        "callingnum1": "123436380",
        "callingnum2": "789037573",
        "switch1": "US",
        "switch2": "UK"
    }
    {
        "time": "2015-04-29T07:13:21.4370000Z",
        "callingimsi": "466923101048691",
        "callingnum1": "678901578",
        "callingnum2": "345626404",
        "switch1": "Germany",
        "switch2": "UK"
    }


#### <a name="arrayofobjects-file-pattern"></a>arrayOfObjects bestandspatroon. 

Elk bestand bevat een matrix van objecten. 

    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "789037573",
            "callingnum2": "789044691",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789044691",
            "switch1": "US",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:24.2120000Z",
            "callingimsi": "466922201102759",
            "callingnum1": "345698602",
            "callingnum2": "789097900",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:25.6320000Z",
            "callingimsi": "466923300236137",
            "callingnum1": "567850552",
            "callingnum2": "789086133",
            "switch1": "China",
            "switch2": "Germany"
        }
    ]

### <a name="jsonformat-example"></a>Voorbeeld van JsonFormat

Als u een JSON-bestand met de volgende inhoud:  

    {
        "Id": 1,
        "Name": {
            "First": "John",
            "Last": "Doe"
        },
        "Tags": ["Data Factory”, "Azure"]
    }

en u wilt kopiëren naar een Azure SQL-tabel in de volgende indeling: 

ID  | Name.First | Name.Middle | Name.Last | Labels
--- | ---------- | ----------- | --------- | ----
1 | John | Null | Doe | ["Gegevens Factory", "Azure"]

De invoer gegevensset met JsonFormat type wordt als volgt gedefinieerd: (gedeeltelijke definitie met alleen de relevante onderdelen)

    "properties": {
        "structure": [
            {"name": "Id", "type": "Int"},
            {"name": "Name.First", "type": "String"},
            {"name": "Name.Middle", "type": "String"},
            {"name": "Name.Last", "type": "String"},
            {"name": "Tags", "type": "string"}
        ],
        "typeProperties":
        {
            "folderPath": "mycontainer/myfolder",
            "format":
            {
                "type": "JsonFormat",
                "filePattern": "setOfObjects",
                "encodingName": "UTF-8",
                "nestingSeparator": "."
            }
        }
    }

Als de structuur wordt niet gedefinieerd, wordt de activiteit kopie effent de structuur al dan niet standaard en elke ding kopiëren. 

#### <a name="supported-json-structure"></a>Ondersteunde JSON-structuur
Houd rekening met de volgende punten: 

- Elk object met een verzameling naam/waardeparen is toegewezen aan één rij met gegevens in een tabelvormige indeling. Objecten kunnen worden genest en kunt u hoe u de structuur in een gegevensset samenvoegen met het nesten scheidingsteken (.) al dan niet standaard. Zie het [voorbeeld JsonFormat](#jsonformat-example) voorafgaand aan de sectie voor een voorbeeld.  
- Als de structuur is niet gedefinieerd in de gegevensset Data Factory, wordt de kopie activiteit wordt gedetecteerd het schema van het eerste object en het hele object samenvoegen. 
- Als de JSON-invoer een matrix heeft, zet de activiteit kopiëren om de hele Matrixwaarde in een tekenreeks. U kunt deze overslaat met behulp van [kolommen toewijzen of filteren](#column-mapping-with-translator-rules).
- Als er dubbele namen op hetzelfde niveau zijn, wordt in de kopie activiteit laatste hervat.
- Eigenschapnamen zijn hoofdlettergevoelig. Twee eigenschappen met dezelfde naam, maar andere darmen worden behandeld als twee afzonderlijke eigenschappen. 

### <a name="specifying-orcformat"></a>OrcFormat opgeven
Als de notatie is ingesteld op OrcFormat, hoeft u niet kunt u de eigenschappen in de sectie indeling in de sectie typeProperties opgeven. Voorbeeld:

    "format":
    {
        "type": "OrcFormat"
    }

> [AZURE.IMPORTANT] Als u niet ORC bestanden kopieert **als-is** tussen on-premises en cloud gegevensopslag, moet u de JRE 8 (Java runtimeomgeving) installeren op de gatewaycomputer. Een gateway 64-bits vereist JRE 64-bits en 32-bits gateway 32-bits JRE is vereist. U kunt beide versies van [hier](http://go.microsoft.com/fwlink/?LinkId=808605)vinden. Kies het juiste bestand.

Houd rekening met de volgende punten:

-   Complexe gegevens typen niet zijn wordt ondersteund (structuur, MAP, lijst, Unie)
-   ORC bestand bevat drie [Opties voor compressie-gerelateerde](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): geen, ZLIB, SNAPPY. Gegevens Factory ondersteunt het lezen van gegevens uit ORC-bestand in een van deze gecomprimeerde indelingen. Site gebruikmaakt van de compressie codec is in de metagegevens om de gegevens te lezen. Echter bij het schrijven naar een bestand ORC, kiest Data Factory ZLIB, de standaardinstelling voor ORC. Er is momenteel geen optie om te dit probleem negeren. 

### <a name="specifying-parquetformat"></a>ParquetFormat opgeven
Als de notatie is ingesteld op ParquetFormat, hoeft u niet kunt u de eigenschappen in de sectie indeling in de sectie typeProperties opgeven. Voorbeeld:

    "format":
    {
        "type": "ParquetFormat"
    }

> [AZURE.IMPORTANT] Als u niet parketvloeren bestanden kopieert **als-is** tussen on-premises en cloud gegevensopslag, moet u de JRE 8 (Java runtimeomgeving) installeren op de gatewaycomputer. Een gateway 64-bits vereist JRE 64-bits en 32-bits gateway 32-bits JRE is vereist. U kunt beide versies van [hier](http://go.microsoft.com/fwlink/?LinkId=808605)vinden. Kies het juiste bestand.

Houd rekening met de volgende punten:

-   Complexe gegevens typen niet worden ondersteund (MAP, lijst)
-   Parketvloeren bestand heeft de volgende opties voor compressie-gerelateerde: geen, SNAPPY, GZIP en LZO. Gegevens Factory ondersteunt het lezen van gegevens uit ORC-bestand in een van deze gecomprimeerde indelingen. Deze de compressiecodec gebruikt in de metagegevens om de gegevens te lezen. Echter bij het schrijven naar een bestand parketvloeren, kiest Data Factory SNAPPY, de standaardinstelling voor parketvloeren opmaken. Er is momenteel geen optie om te dit probleem negeren. 
