## <a name="invoking-stored-procedure-for-sql-sink"></a>Aanroepen van opgeslagen procedure voor het ontvangen van SQL

Als u gegevens in SQL Server of Azure SQL/SQL Server-Database, wordt een gebruiker opgegeven opgeslagen procedure kan worden geconfigureerd en aangeroepen met aanvullende parameters. 

Een opgeslagen procedure kan worden gebruikt wanneer ingebouwde kopie regelingen niet voor het doel dienen doen. Dit is meestal gebruikt bij het verwerken van extra (samenvoegen kolommen, opzoeken extra waarden, invoegen in meerdere tabellen...) moet worden uitgevoerd voordat het uiteindelijke invoegen van brongegevens in de doeltabel. 

Een opgeslagen procedure van keuze kan worden gestart. In het onderstaande voorbeeld ziet u hoe u met een opgeslagen procedure uitvoeren van een eenvoudige invoegen in een tabel in de database. 

**Uitvoer gegevensset**

In dit voorbeeld type is ingesteld op: SqlServerTable. Stel deze in op AzureSqlTable voor gebruik met een Azure SQL-database. 

    {
      "name": "SqlOutput",
      "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlLinkedService",
        "typeProperties": {
          "tableName": "Marketing"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    
De sectie SqlSink als volgt in kopie activiteit JSON definiëren. Als u wilt bellen een opgeslagen procedure tijdens invoegen gegevens, zijn zowel SqlWriterStoredProcedureName als SqlWriterTableType eigenschappen nodig.

    "sink":
    {
        "type": "SqlSink",
        "SqlWriterTableType": "MarketingType",
        "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
        "storedProcedureParameters":
                {
                    "stringData": 
                    {
                        "value": "str1"     
                    }
                }
    }

In uw database, definieert u de opgeslagen procedure met dezelfde naam als SqlWriterStoredProcedureName. Het verwerkt ingevoerde gegevens uit de opgegeven bron en invoegen in de uitvoertabel. Zoals u ziet dat de naam van de parameter van de opgeslagen procedure hetzelfde als de tabelnaam gedefinieerd in de tabel JSON-bestand zijn moet.

    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
    AS
    BEGIN
        DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
        INSERT [dbo].[Marketing](ProfileID, State)
        SELECT * FROM @Marketing
    END

In uw database, het tabeltype met dezelfde naam als SqlWriterTableType te definiëren. U ziet dat het schema van het tabeltype hetzelfde als het schema die het resultaat van de invoergegevens moet.

    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL
    )

De functie opgeslagen procedure maakt gebruik van [Table-Valued Parameters](https://msdn.microsoft.com/library/bb675163.aspx).