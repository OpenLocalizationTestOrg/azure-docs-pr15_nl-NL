<properties
   pageTitle="PolyBase in SQL Data Warehouse zelfstudie | Microsoft Azure"
   description="Lees wat PolyBase is en hoe u deze gebruiken voor gegevensopslag scenario's."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-with-polybase-in-sql-data-warehouse"></a>Gegevens met PolyBase in SQL Data Warehouse laden

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Gegevens Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)

Deze zelfstudie leert hoe u de gegevens laadt in SQL Data Warehouse met behulp van AzCopy en PolyBase. Wanneer u klaar bent, weet u hoe u:

- AzCopy gebruiken om gegevens te kopiëren naar Azure-blobopslag
- Databaseobjecten om te definiëren van de gegevens maken
- Een T-SQL-query om de gegevens te laden uitvoeren

>[AZURE.VIDEO loading-data-with-polybase-in-azure-sql-data-warehouse]

## <a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie stapsgewijs, die u nodig hebt

- Een database van SQL Data Warehouse.
- Een account Azure opslag van het type standaard lokaal redundante opslag (standaard-LRS), standaard geografische redundante opslag (standaard-GRS) of standaard leestoegang geografische redundante opslag (standaard-RAGRS).
- AzCopy te gebruiken. Download en installeer de [meest recente versie van AzCopy][] die is geïnstalleerd met Microsoft Azure opslag's.

    ![Hulpmiddelen voor de opslag van Azure](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)


## <a name="step-1-add-sample-data-to-azure-blob-storage"></a>Stap 1: Voorbeeldgegevens toevoegen aan Azure-blobopslag

Om te kunnen gegevens hebt geladen, moeten we enkele voorbeeldgegevens in een Azure-blobopslag plaatsen. In deze stap vullen we een opslag van Azure blob met voorbeeldgegevens. We gebruiken later PolyBase deze voorbeeldgegevens in uw database SQL Data Warehouse laden.

### <a name="a-prepare-a-sample-text-file"></a>A. Een tekstbestand steekproef voorbereiden

Voor het voorbereiden van een steekproef tekstbestand:

1. Open Kladblok en kopieer de volgende regels van gegevens naar een nieuw bestand. Dit opslaan in de lokale tijdelijke map als % temp%\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="b-find-your-blob-service-endpoint"></a>B TE DRUKKEN. Uw service-eindpunt blob zoeken

Uw service-eindpunt blob zoeken:

1. Selecteer **Bladeren**in de Portal Azure > **Opslag-Accounts**.
2. Klik op de opslag-account dat u wilt gebruiken.
3. Klik in het blad opslag-account op BLOB 's

    ![Klik op BLOB 's](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)

1. De URL van uw blob service-eindpunt opslaan voor later.

    ![BLOB service-eindpunt](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### <a name="c-find-your-azure-storage-key"></a>C. Azure opslag vinden

Uw sleutel Azure opslag zoeken:

1. Selecteer **Bladeren**in de Portal Azure > **Opslag-Accounts**.
2. Klik op de opslag-account dat u wilt gebruiken.
3. Selecteer **alle instellingen** > **toegangstoetsen**.
4. Klik in het vak kopiëren als u wilt een van uw toegangstoetsen naar het Klembord kopiëren.

    ![Kopieer Azure opslag-toets](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### <a name="d-copy-the-sample-file-to-azure-blob-storage"></a>D TE DRUKKEN. Het voorbeeldbestand kopiëren naar Azure-blobopslag

Uw gegevens kopiëren naar Azure-blobopslag:

1. Open een opdrachtprompt en mappen wijzigen in de map AzCopy installeren. Deze opdracht verandert in de standaardmap voor de installatie op een 64-bits Windows-client.

    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```

1. Voer de volgende opdracht uit om het bestand te uploaden. Geef uw blob service eindpunt-URL voor <blob service endpoint URL> en uw accountsleutel Azure opslag voor < azure_storage_account_key >.

    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

Zie ook [aan de slag met het hulpprogramma AzCopy][].

### <a name="e-explore-your-blob-storage-container"></a>E. Uw blob storage container verkennen

Als u wilt zien van het bestand dat u hebt geüpload naar blobopslag:

1. Ga terug naar uw blade Blob-service.
2. Dubbelklik onder Containers, op **datacontainer**.
3. Als u wilt het pad naar uw gegevens verkennen, klik op de map **datedimension** en ziet u uw geüploade **DimDate2.txt**-bestand.
4. Als eigenschappen wilt bekijken, klikt u op **DimDate2.txt**.
5. Houd er rekening mee dat in het blad Blob eigenschappen, kunt u downloaden of verwijder het bestand.

    ![Weergave opslagruimte Azure blob](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)


## <a name="step-2-create-an-external-table-for-the-sample-data"></a>Stap 2: Een externe tabel voor de voorbeeldgegevens maken

In deze sectie maken we een externe tabel waarin de voorbeeldgegevens.

Externe tabellen PolyBase gebruikt voor toegang tot gegevens in Azure-blobopslag. Aangezien de gegevens niet zijn opgeslagen in SQL Data Warehouse, verwerkt PolyBase verificatie met de externe gegevens met behulp van een referentie binnen bereik van de database.

Het voorbeeld in deze stap wordt deze Transact-SQL-instructies om een externe tabel te maken.

- [Basispagina sleutel maken (Transact-SQL)][] om het geheim van uw database versleutelen beperkt referentie.
- [Maak Database beperkt referentie (Transact-SQL)][] om op te geven verificatie-informatie voor uw account Azure opslag.
- [Externe gegevensbron maken (Transact-SQL)][] om op te geven van de locatie van uw Azure-blobopslag.
- [Maak externe bestandsindeling (Transact-SQL)][] om op te geven van de indeling van uw gegevens.
- [Externe tabel maken (Transact-SQL)][] de tabeldefinitie en de locatie van de gegevens te geven.

Deze query voor uw database SQL Data Warehouse uitgevoerd. Er wordt een externe tabel met de naam DimDate2External in het schema dbo die naar de DimDate2.txt voorbeeldgegevens in de Azure-blobopslag verwijst gemaakt.


```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);


-- D: Create an external file format
-- FORMAT_TYPE: Type of file format in Azure storage (supported: DELIMITEDTEXT, RCFILE, ORC, PARQUET).
-- FORMAT_OPTIONS: Specify field terminator, string delimiter, date format etc. for delimited text files.
-- Specify DATA_COMPRESSION method if data is compressed.

CREATE EXTERNAL FILE FORMAT TextFile
WITH (
    FORMAT_TYPE = DelimitedText,
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create the external table
-- Specify column names and data types. This needs to match the data in the sample file.
-- LOCATION: Specify path to file or directory that contains the data (relative to the blob container).
-- To point to all files under the blob container, use LOCATION='.'

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL,
    CalendarQuarter TINYINT NOT NULL,
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='/datedimension/',
    DATA_SOURCE=AzureStorage,
    FILE_FORMAT=TextFile
);


-- Run a query on the external table

SELECT count(*) FROM dbo.DimDate2External;

```


In SQL Server-Object Explorer in Visual Studio ziet u de externe bestandsindeling, externe gegevensbron en de tabel DimDate2External.

![Weergave externe tabel](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## <a name="step-3-load-data-into-sql-data-warehouse"></a>Stap 3: Gegevens laadt in SQL Data Warehouse

Nadat de externe tabel is gemaakt, kunt u de gegevens laadt in een nieuwe tabel of invoegen in een bestaande tabel.

- Als u wilt de gegevens in een nieuwe tabel hebt geladen, voer de instructie [CREATE tabel AS SELECT (Transact-SQL)][] . De nieuwe tabel heeft de kolommen met de naam in de query. De gegevenstypen van de kolommen komen overeen met de gegevenstypen in de definitie van de externe tabel.
- Gebruik de [Invoegen als u wilt de gegevens laadt in een bestaande tabel... Selecteer (Transact-SQL)][] instructie.

```sql
-- Load the data from Azure blob storage to SQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT * FROM [dbo].[DimDate2External];
```

## <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Stap 4: Statistieken maken voor uw nieuwe geladen gegevens

SQL Data Warehouse niet automatisch maken of statistieken automatisch bijwerken. Als u wilt bereiken hoog queryprestaties, daarom belangrijk voor statistieken maken voor elke kolom van elke tabel na de eerste keer worden geladen. Het is ook belangrijke statistieken bijwerken nadat belangrijke wijzigingen in de gegevens.

In dit voorbeeld wordt gemaakt van één kolom statistieken op de nieuwe DimDate2-tabel.

```sql
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
CREATE STATISTICS [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

Meer informatie, [statistische gegevens][]te bekijken.  


## <a name="next-steps"></a>Volgende stappen
Zie de [PolyBase handleiding][] voor meer informatie die u weten moet als u een oplossing die wordt gebruikt PolyBase ontwikkelen.

<!--Image references-->


<!--Article references-->
[PolyBase in SQL Data Warehouse Tutorial]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md
[PolyBase handleiding]: ./sql-data-warehouse-load-polybase-guide.md
[Aan de slag met het hulpprogramma AzCopy]: ../storage/storage-use-azcopy.md
[meest recente versie van AzCopy]: ../storage/storage-use-azcopy.md

<!--External references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx


[MAKEN van de externe gegevensbron (Transact-SQL)]:https://msdn.microsoft.com/library/dn935022.aspx
[EXTERNE BESTANDSINDELING (Transact-SQL) maken]:https://msdn.microsoft.com/library/dn935026.aspx
[EXTERNE tabel (Transact-SQL) maken]:https://msdn.microsoft.com/library/dn935021.aspx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]:https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]:https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]:https://msdn.microsoft.com/library/mt130698.aspx

[TABEL als selecteren (Transact-SQL) maken]:https://msdn.microsoft.com/library/mt204041.aspx
[INVOEGEN... Selecteer (Transact-SQL)]:https://msdn.microsoft.com/library/ms174335.aspx
[BASISPAGINA sleutel (Transact-SQL) maken]:https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189522.aspx
[MAKEN van DATABASE binnen bereik van referentie (Transact-SQL)]:https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]:https://msdn.microsoft.com/library/ms189450.aspx
