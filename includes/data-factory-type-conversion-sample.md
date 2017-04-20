### <a name="type-conversion-sample"></a>Type conversie steekproef
In het onderstaande voorbeeld is voor het kopiëren van gegevens uit een Blob naar SQL Azure met typeconversies.

Stel dat u de Blob gegevensset in CSV-indeling en 3 kolommen bevat. Een van deze is een datetime-kolom met een aangepaste datetime-notatie met Franse afkortingen voor dag van de week.

U kunt de gegevensset Blob bron als volgt samen met de definities voor afhankelijkheidstype voor de kolommen wilt definiëren.

    {
        "name": "AzureBlobTypeSystemInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
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
            "external": true,
            "availability":
            {
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

Gegeven van de SQL zou type naar .NET toewijzingstabel hierboven u de Azure SQL-tabel met de volgende schema definiëren.

| Kolomnaam | SQL-Type |
| ----------- | -------- |
| gebruikers-id | bigint |
| naam | tekst |
| lastlogindate | datum / |

Vervolgens wordt u de SQL Azure-gegevensset als volgt definiëren. Opmerking: U niet hoeft te geven "structuur" sectie met gegevens van omdat de gegevens al is opgegeven in de onderliggende gegevensopslag.

    {
        "name": "AzureSQLOutput",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

In dit geval doet gegevens factory automatisch het type conversies waaronder het datum/tijd-veld met de aangepaste datum-/ met behulp van de cultuur Frans-Frans opmaken wanneer u gegevens uit Blob naar SQL Azure wordt verplaatst.


