<properties
   pageTitle="Gegevens uit SQL Server laadt in Azure SQL Data Warehouse (bcp) | Microsoft Azure"
   description="Voor een kleine gegevensgrootte, gebruikt bcp gegevens exporteren vanuit SQL Server naar platte bestanden en de gegevens rechtstreeks in Azure SQL Data Warehouse importeren."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>Gegevens uit SQL Server laadt in Azure SQL Data Warehouse (platte bestanden)

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)

Voor kleine gegevenssets, kunt u het hulpprogramma bcp gegevens exporteren vanuit SQL Server en deze vervolgens rechtstreeks naar Azure SQL Data Warehouse te laden.

In deze zelfstudie gebruikt u bcp naar:

- Een tabel uit exporteren vanuit SQL Server met behulp van de bcp opdracht (of maken van een eenvoudige voorbeeldbestand)
- De tabel uit een plat bestand importeren naar SQL Data Warehouse.
- Statistieken maken voor de geladen gegevens.

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## <a name="before-you-begin"></a>Voordat u begint

### <a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie stapsgewijs, hebt u het volgende nodig:

- Een SQL Data Warehouse-database
- Het bcp hulpprogramma is geïnstalleerd
- De opdrachtregel hulpprogramma sqlcmd geïnstalleerd

U kunt de hulpprogramma's bcp en sqlcmd downloaden van het [Microsoft Downloadcentrum][].

### <a name="data-in-ascii-or-utf-16-format"></a>Gegevens in ASCII- of UTF-16-indeling

Als u deze zelfstudie met uw eigen gegevens probeert, moet uw gegevens de ASCII- of UTF-16-codering gebruiken omdat bcp UTF-8 niet ondersteunt. 

PolyBase UTF-8 ondersteunt, maar nog biedt geen ondersteuning voor UTF-16. Houd er rekening mee dat als u wilt combineren bcp met PolyBase u de gegevens in UTF-8 transformeren moeten nadat deze is geëxporteerd vanuit SQL Server. 


## <a name="1-create-a-destination-table"></a>1. een tabel bestemming maken

Een tabel in SQL Data Warehouse die de doeltabel voor het selectievakje laden definiëren. De kolommen in de tabel moeten overeenkomen met de gegevens in elke rij van het gegevensbestand.

Een tabel maken, open een opdrachtprompt met behulp van sqlcmd.exe en voer de volgende opdracht:


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


## <a name="2-create-a-source-data-file"></a>2. een bron-gegevensbestand maken

Open Kladblok en kopieer de volgende regels van gegevens naar een nieuw tekstbestand en sla dit bestand in de lokale tijdelijke map, C:\Temp\DimDate2.txt. Deze gegevens zijn in ASCII-indeling.

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

(Optioneel) Uw eigen gegevens exporteren uit een SQL Server-database, open een opdrachtprompt en voer de volgende opdracht. Tabelnaam, servernaam databasenaam, gebruikersnaam en wachtwoord vervangen door uw eigen gegevens.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-the-data"></a>3. de gegevens te laden
Als u wilt de gegevens hebt geladen, open een opdrachtprompt en voer de volgende opdracht, de waarden voor de naam van de Server, Database naam, gebruikersnaam en wachtwoord vervangen door uw eigen gegevens.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Gebruik deze opdracht om te controleren of dat de gegevens correct is geladen

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

De resultaten er als volgt:

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

## <a name="4-create-statistics"></a>4. statistieken maken

SQL Data Warehouse biedt nog geen ondersteuning automatisch maken of statistieken automatisch bijwerken. Als u de beste queryprestaties, is het belangrijk dat het statistieken maken voor alle kolommen van alle tabellen na de eerste laden of nadat belangrijke wijzigingen in de gegevens optreden. Voor een gedetailleerde uitleg van statistieken, [statistische gegevens][]te bekijken. 

Voer de volgende opdracht naar statistieken maken voor uw nieuwe geladen tabel.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5. gegevens exporteren vanuit SQL Data Warehouse
Voor een goede, kunt u de gegevens die u zojuist hebt geladen terug uit SQL Data Warehouse exporteren.  De opdracht exporteren is precies hetzelfde als het exporteren van SQL Server.

Er is echter een verschil in de zoekresultaten. Aangezien de gegevens worden opgeslagen op verdeelde locaties in SQL Data Warehouse, wanneer u gegevens exporteert schrijft elk knooppunt berekeningscluster u deze gegevens naar het uitvoerbestand. De volgorde van de gegevens in het uitvoerbestand is waarschijnlijk afwijken van de volgorde van de gegevens in het bestand dat invoer.

### <a name="export-a-table-and-compare-exported-results"></a>Een tabel exporteren en geëxporteerde resultaten vergelijken

Zie de geëxporteerde gegevens, open een opdrachtprompt en uitvoeren van deze opdracht met uw eigen parameters. Servernaam is de naam van uw Azure logische SQL Server.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
U kunt controleren of dat de gegevens correct zijn geëxporteerd door het nieuwe bestand te openen. De gegevens in het bestand moet overeenkomen met de onderstaande tekst, maar waarschijnlijk worden gesorteerd in een andere volgorde:

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

### <a name="export-the-results-of-a-query"></a>De resultaten van een query exporteren

U kunt de functie **queryout** van bcp exporteren de resultaten van een query in plaats van de hele tabel exporteren. 

## <a name="next-steps"></a>Volgende stappen
Zie voor een overzicht van laden, [gegevens laden in SQL Data Warehouse][].
Zie voor meer tips voor de ontwikkeling, [SQL Data Warehouse ontwikkelen-overzicht][].
Zie [Overzicht van de tabel][] of [de syntaxis van de tabel maken][] voor meer informatie over het maken van een tabel op SQL Data Warehouse.

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
