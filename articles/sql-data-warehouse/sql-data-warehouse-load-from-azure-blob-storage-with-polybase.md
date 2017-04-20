<properties
   pageTitle="Gegevens van Azure-blobopslag laadt in SQL Data Warehouse (PolyBase) | Microsoft Azure"
   description="Informatie over het gebruik van PolyBase gegevens van Azure-blobopslag in SQL Data Warehouse laden. Een paar tabellen van openbare gegevens laden in het schema Contoso detailhandel Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-azure-blob-storage-into-sql-data-warehouse-polybase"></a>Gegevens van Azure-blobopslag laadt in SQL Data Warehouse (PolyBase)

> [AZURE.SELECTOR]
- [Gegevens Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
- [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)

Gegevens van Azure-blobopslag in Azure SQL Data Warehouse laden, gebruikt u PolyBase en T-SQL-opdrachten. 

Deze zelfstudie wordt in het schema Contoso detailhandel Data Warehouse zodat deze eenvoudige, de twee tabellen uit een openbare Azure opslag Blob geladen. Als u wilt de volledige gegevensset hebt geladen, het [laden van de volledige Contoso detailhandel Data Warehouse][] voorbeeld uit de bibliotheek Microsoft SQL Server Samples worden uitgevoerd.

In deze zelfstudie zal u het volgende doen:

1. PolyBase laden van Azure-blobopslag configureren
2. Openbare gegevens laadt in uw database
3. Optimalisaties uitgevoerd nadat het selectievakje laden is voltooid.


## <a name="before-you-begin"></a>Voordat u begint
Als u wilt uitvoeren van deze zelfstudie, moet u een Azure account al met een SQL Data Warehouse-database. Als u dit nog geen hebt, raadpleegt u [een SQL-Data Warehouse maken][].

## <a name="1-configure-the-data-source"></a>1. de gegevensbron configureren

T-SQL externe objecten PolyBase gebruikt om de locatie en de kenmerken van de externe gegevens te definiëren. De externe objectdefinities zijn opgeslagen in SQL Data Warehouse. De gegevens zelf wordt extern opgeslagen.

### <a name="11-create-a-credential"></a>1.1. Een referentie maken

**Deze stap overslaan** als u de Contoso in openbare gegevens worden geladen. U hoeft geen beveiligde toegang tot de openbare gegevens sinds deze nog toegankelijk voor iedereen.

**Deze stap niet overslaan** als u deze zelfstudie als een sjabloon gebruikt voor het laden van uw eigen gegevens. Gebruik het volgende script maken van een database beperkt referentie voor toegang tot gegevens via een referentie, en deze vervolgens gebruiken bij het definiëren van de locatie van de gegevensbron.


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
```

Ga naar stap 2.

### <a name="12-create-the-external-data-source"></a>1.2. De externe gegevensbron maken

Gebruik deze opdracht [Externe gegevensbron maken][] voor de opslag van de locatie van de gegevens en het type gegevens. 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [AZURE.IMPORTANT] Als u besluit om uw azure blob storage containers openbaar maken, moet u dat als de eigenaar van de gegevens u voor gegevens egress kosten betalen moet wanneer gegevens de Datacenter verlaat. 

## <a name="2-configure-data-format"></a>2. gegevensindeling configureren

De gegevens worden opgeslagen in tekstbestanden in Azure-blobopslag en elk veld is gescheiden met een scheidingsteken. Deze opdracht [Externe BESTANDSINDELING maken][] om op te geven van de indeling van de gegevens in de tekstbestanden uitvoeren. De Contoso-gegevens is niet gecomprimeerd en recht streepje als scheidingsteken.

```sql
CREATE EXTERNAL FILE FORMAT TextFileFormat 
WITH 
(   FORMAT_TYPE = DELIMITEDTEXT
,   FORMAT_OPTIONS  (   FIELD_TERMINATOR = '|'
                    ,   STRING_DELIMITER = ''
                    ,   DATE_FORMAT      = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,   USE_TYPE_DEFAULT = FALSE 
                    )
);
``` 

## <a name="3-create-the-external-tables"></a>3. de externe tabellen maken

Nu u de gegevens bron en de bestandsindeling hebt opgegeven, bent u gereed om de externe tabellen te maken. 

### <a name="31-create-a-schema-for-the-data"></a>3.1. Maak een schema voor de gegevens. 

Als u wilt een locatie voor de opslag van de Contoso-gegevens in uw database hebt gemaakt, kunt u een schema maken.

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-the-external-tables"></a>3,2. De externe tabellen maken. 

In dit script uitvoeren om de DimProduct en FactOnlineSales externe tabellen maken. Alle we hier presteren gegevenstypen en kolomnamen definiëren en binden ze naar de locatie en de opmaak van de Azure blob storage-bestanden. De definitie is opgeslagen in SQL Data Warehouse en de gegevens zich nog in de Blob Azure-opslag.

De parameter **locatie** is de map onder de hoofdmap in de Blob Azure-opslag. Elke tabel is in een andere map.


```sql

--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
 
--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-the-data"></a>4. de gegevens te laden
Er is verschillende manieren voor toegang tot externe gegevens.  U kunt een query gegevens rechtstreeks uit de externe tabel, de gegevens laadt in nieuwe databasetabellen, of externe gegevens toevoegen aan bestaande databasetabellen.  


### <a name="41-create-a-new-schema"></a>4.1. Een nieuw schema maken

CTAS Hiermee maakt u een nieuwe tabel met gegevens.  Maak eerst een schema voor de contoso-gegevens.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-the-data-into-new-tables"></a>4.2. De gegevens laadt in nieuwe tabellen

Als u wilt gegevens van Azure-blobopslag laden en opslaat in een tabel in uw database, gebruikt u de instructie [CREATE tabel AS SELECT (Transact-SQL)][] . Laden met CTAS maakt gebruik van de ten zeerste getypte externe tabellen die u zojuist hebt gemaakt. Als u wilt de gegevens laadt in nieuwe tabellen, gebruikt u één [CTAS][] overzicht per tabel. 

CTAS wordt een nieuwe tabel gemaakt en gevuld met de resultaten van een select-instructie. CTAS Hiermee definieert u de nieuwe tabel als u wilt dat de gegevenstypen en dezelfde kolommen als de resultaten van de select-instructie. Als u alle kolommen uit een externe tabel selecteren, zijn de nieuwe tabel een replica van de kolommen en gegevenstypen in de externe tabel.

In dit voorbeeld maken we als u de dimensie en de feitentabel zoals hash-verdeelde tabellen. 


```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-the-load-progress"></a>4,3 de voortgang van het laden van bijhouden

U kunt de voortgang van uw laden met dynamische management weergaven (DMVs) bijhouden. 

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r;
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a>5. columnstore compressie optimaliseren

Standaard worden in SQL Data Warehouse de tabel als u een index gegroepeerd columnstore opgeslagen. Nadat een laden is voltooid, mogelijk enkele van de rijen met gegevens niet in de columnstore worden gecomprimeerd.  Er is een aantal redenen waarom dit kan gebeuren. Meer informatie raadpleegt u [columnstore indexen beheren][].

Als u wilt optimaliseren queryprestaties en columnstore compressie na een laden, bouw die tabellen opnieuw in de tabel als u wilt dat de index columnstore voor het comprimeren van alle rijen. 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Zie het artikel [columnstore indexen beheren][] voor meer informatie over het onderhouden van columnstore indexen.

## <a name="6-optimize-statistics"></a>6. statistieken optimaliseren

Het is raadzaam om te maken van één kolom statistieken direct achter een laden. Er zijn verschillende opties om de statistieken. Bijvoorbeeld als u één kolom statistieken voor elke kolom maken duurt het erg lang om alle statistieken opnieuw te maken. Als u weet dat bepaalde kolommen in de query predicaten kunnen niet worden verzonden, kunt u maken statistieken op deze kolommen overslaan.

Als u één kolom statistieken maken voor elke kolom van elke tabel, kunt u de opgeslagen procedure code steekproef `prc_sqldw_create_stats` in het artikel [Statistieken][] .

Het volgende voorbeeld is een goed beginpunt voor het maken van statistieken. Statistieken van één kolom wordt gemaakt van elke kolom in de dimensietabel en elke gekoppelde kolom in de feitentabellen. U kunt altijd enkele of meerdere kolommen statistieken naar andere tabelkolommen faculteit later toevoegen.


```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a>Bereiken ontgrendelen!

Openbare gegevens is in Azure SQL Data Warehouse geladen. Uitstekende taak!

U kunt nu starten bij het controleren van de tabellen met query's als volgt uit:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>Volgende stappen
De volledige Contoso detailhandel Data Warehouse om gegevens te laden, het script in gebruiken voor meer tips voor de ontwikkeling, Zie [overzicht van de ontwikkeling SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Een SQL-datawarehouse maken]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[SQL Data Warehouse ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md
[columnstore indexen beheren]: sql-data-warehouse-tables-index.md
[Statistieken]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[EXTERNE GEGEVENSBRON MAKEN]: https://msdn.microsoft.com/en-us/library/dn935022.aspx
[EXTERNE BESTANDSINDELING MAKEN]: https://msdn.microsoft.com/en-us/library/dn935026.aspx
[TABEL als selecteren (Transact-SQL) maken]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[De volledige Contoso detailhandel Data Warehouse laden]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
