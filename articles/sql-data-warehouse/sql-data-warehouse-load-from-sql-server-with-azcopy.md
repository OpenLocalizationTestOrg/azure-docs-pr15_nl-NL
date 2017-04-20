<properties
   pageTitle="Gegevens uit SQL Server laadt in Azure SQL Data Warehouse (PolyBase) | Microsoft Azure"
   description="Bcp exporteren van gegevens uit SQL Server naar platte bestanden, AZCopy importeren van gegevens met Azure-blobopslag en PolyBase naar het nemen van de gegevens in Azure SQL Data Warehouse gebruikt."
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
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-azcopy"></a>Gegevens uit SQL Server laadt in Azure SQL Data Warehouse (AZCopy)

Gebruik bcp en AZCopy hulpprogramma gegevens laden vanuit SQL Server naar Azure-blobopslag. Gebruik PolyBase of Factory van Azure-gegevens naar de gegevens laadt in Azure SQL Data Warehouse. 


## <a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie stapsgewijs, hebt u het volgende nodig:

- Een SQL Data Warehouse-database
- Het hulpprogramma voor bcp-opdrachtregel is geïnstalleerd
- Het hulpprogramma voor het opdrachtregel van SQLCMD geïnstalleerd

>[AZURE.NOTE] U kunt de hulpprogramma's bcp en sqlcmd downloaden van het [Microsoft Downloadcentrum][].

## <a name="import-data-into-sql-data-warehouse"></a>Gegevens importeren in SQL Data Warehouse

In deze zelfstudie u een tabel maken in Azure SQL Data Warehouse en gegevens importeren in de tabel.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Stap 1: Een tabel maken in Azure SQL Data Warehouse

Gebruik sqlcmd naar de volgende query om te maken van een tabel in uw exemplaar van een opdrachtprompt:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```

>[AZURE.NOTE] Zie [Overzicht van de tabel][] of [de syntaxis van de tabel maken][] voor meer informatie over het maken van een tabel op SQL Data Warehouse en de opties die beschikbaar zijn in de WITH-clausule.

### <a name="step-2-create-a-source-data-file"></a>Stap 2: Een bron-gegevensbestand maken

Open Kladblok en kopieer de volgende regels van gegevens naar een nieuw tekstbestand en sla dit bestand in de lokale tijdelijke map, C:\Temp\DimDate2.txt.

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

> [AZURE.NOTE] Het is belangrijk te onthouden dat die bcp.exe biedt geen ondersteuning voor het coderen van UTF-8 bestand. Gebruik ASCII-bestanden of UTF-16 gecodeerde bestanden wanneer u een bcp.exe gebruikt.

### <a name="step-3-connect-and-import-the-data"></a>Stap 3: Verbinding maken en de gegevens te importeren
Bcp gebruikt, kunt u verbinding maken en de gegevens importeren via de volgende opdracht uit de gewenste waarden vervangen:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

U kunt controleren of dat de gegevens is geladen door de volgende query met sqlcmd uit te voeren:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Dit moet de volgende resultaten worden geretourneerd:

DateId |Kalenderkwartaal |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Stap 4: Statistieken maken voor uw nieuwe geladen gegevens

Azure SQL Data Warehouse biedt nog geen ondersteuning auto maken of bijwerken statistieken jaarlijks automatisch. Om te krijgen de beste prestaties van uw query's, het is belangrijk dat statistieken worden gemaakt voor alle kolommen van alle tabellen na de eerste laden of belangrijke wijzigingen optreden in de gegevens. Zie het onderwerp [Statistieken][] in de groep ontwikkelen van onderwerpen voor een gedetailleerde uitleg van statistieken. Hieronder ziet u een snel voorbeeld van het maken van statistieken over de tabellen bevatten geladen in dit voorbeeld

Voer de volgende instructies voor de statistieken maken vanaf een prompt sqlcmd:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Gegevens exporteren vanuit SQL Data Warehouse
In deze zelfstudie maakt u een gegevensbestand uit een tabel in SQL Data Warehouse. We worden de gegevens die u hebt gemaakt boven exporteren naar een nieuw gegevensbestand DimDate2_export.txt genoemd.

### <a name="step-1-export-the-data"></a>Stap 1: De gegevens exporteren

Het hulpprogramma bcp gebruikt, kunt u verbinden en exporteren van gegevens met behulp van de volgende opdracht uit de gewenste waarden vervangen:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
U kunt controleren of dat de gegevens correct zijn geëxporteerd door het nieuwe bestand te openen. De gegevens in het bestand moet overeenkomen met de onderstaande tekst:

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

>[AZURE.NOTE] Vanwege de aard van gedistribueerde systemen, de gegevensvolgorde mogelijk niet hetzelfde over SQL Data Warehouse databases. Een andere optie is de functie **queryout** van bcp gebruiken om een query uitpakken schrijven in plaats van de hele tabel exporteren.

## <a name="next-steps"></a>Volgende stappen
Zie voor een overzicht van laden, [gegevens laden in SQL Data Warehouse][].
Zie voor meer tips voor de ontwikkeling, [SQL Data Warehouse ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->

[Gegevens laadt in SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse ontwikkelen-overzicht]: ./sql-data-warehouse-overview-develop.md
[Overzicht van de tabel]: ./sql-data-warehouse-tables-overview.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[De syntaxis van de tabel maken]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
