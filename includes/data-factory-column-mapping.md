## <a name="column-mapping-with-translator-rules"></a>Kolommen toewijzen met translator regels
Kolomtoewijzing kan worden gebruikt om op te geven hoe kolommen opgegeven in de "structuur" van bron tabel map naar kolommen opgegeven in de "structuur" van sink tabel. De eigenschap **columnMapping** is beschikbaar in de sectie **typeProperties** van de activiteit kopiëren.

Kolommen toewijzen ondersteunt de volgende scenario's:

- Alle kolommen in de brontabel "structuur" zijn toegewezen aan alle kolommen in de tabel sink "structuur".
- Een subset van de kolommen in de brontabel "structuur" zijn toegewezen aan alle kolommen in de tabel sink "structuur".

De volgende fouten zijn en leidt tot een uitzondering:

- Minder kolommen of meer kolommen in de "structuur" van sink tabel dan opgegeven in de toewijzing.
- Dubbele toewijzing.
- Resultaat van de SQL-query heeft geen een kolomnaam die is opgegeven in de toewijzing.

## <a name="column-mapping-samples"></a>Voorbeelden van kolom toewijzing
> [AZURE.NOTE] In de onderstaande voorbeelden zijn van Azure SQL- en Azure Blob maar zijn van toepassing op alle gegevensopslag die ondersteuning biedt voor rechthoekige gegevenssets. U moet gegevensset en servicedefinities van de gekoppelde in de voorbeelden hieronder aanpassen zodat deze verwijzen naar gegevens in de relevante gegevensbron. 

### <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>Voorbeeld 1 – kolom vanuit SQL Azure aan Azure blob toewijzen
In dit voorbeeld wordt de invoertabel heeft een structuur en deze verwijst naar een SQL-tabel in een Azure SQL-database.

    {
        "name": "AzureSQLInput",
        "properties": {
            "structure": 
             [
               { "name": "userid"},
               { "name": "name"},
               { "name": "group"}
             ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
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

In dit voorbeeld wordt de uitvoertabel heeft een structuur en deze verwijst naar een blob in een Azure-blobopslag.

    {
        "name": "AzureBlobOutput",
        "properties":
        {
             "structure": 
              [
                    { "name": "myuserid"},
                    { "name": "myname" },
                    { "name": "mygroup"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

De JSON voor de activiteit wordt hieronder weergegeven. De kolommen uit de bron die zijn toegewezen aan kolommen in sink (**columnMappings**) met behulp van de eigenschap **Minivertaler** .

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "Copy",
        "inputs":  [ { "name": "AzureSQLInput"  } ],
        "outputs":  [ { "name": "AzureBlobOutput" } ],
        "typeProperties":    {
            "source":
            {
                "type": "SqlSource"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
            }
        },
       "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

**Stroom van de toewijzing kolom:**

![Kolom toewijzing stroom](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow.png)

### <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>Voorbeeld 2 – kolom toewijzing met SQL-query uit SQL Azure naar Azure blob
In dit voorbeeld wordt een SQL-query gebruikt voor het extraheren van gegevens uit Azure SQL in plaats van te geven de tabelnaam van de en de kolomnamen in de sectie "structuur". 

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "CopyActivity",
        "inputs":  [ { "name": " AzureSQLInput"  } ],
        "outputs":  [ { "name": " AzureBlobOutput" } ],
        "typeProperties":
        {
            "source":
            {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "Translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
            }
        },
        "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

In dit geval zijn de resultaten van de query eerst toegewezen aan kolommen die zijn opgegeven in "structuur" van de bron. Vervolgens zijn de kolommen uit de bron "structuur" toegewezen aan kolommen in sink "structuur" met regels die zijn opgegeven in columnMappings.  Stel dat de query retourneert 5 kolommen, twee extra kolommen en die zijn opgegeven in de "structuur" van de bron.

**Kolom toewijzing stroom**

![Kolom toewijzing stroom-2](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow-2.png)







