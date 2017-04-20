<properties
   pageTitle="Gegevens uit CSV-bestand laadt in Azure SQL-Databaase (bcp) | Microsoft Azure"
   description="Voor een kleine gegevensgrootte, wordt de bcp gebruikt om gegevens in Azure SQL-Database importeren."
   services="sql-database"
   documentationCenter="NA"
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/13/2016"
   ms.author="carlrab"/>


# <a name="load-data-from-csv-into-azure-sql-data-warehouse-flat-files"></a>Gegevens uit CSV laadt in Azure SQL Data Warehouse (platte bestanden)

U kunt het hulpprogramma bcp gebruiken om gegevens te importeren uit een CSV-bestand naar Azure SQL-Database.

## <a name="before-you-begin"></a>Voordat u begint

### <a name="prerequisites"></a>Vereisten voor

Als u wilt deze zelfstudie stapsgewijs, hebt u het volgende nodig:

- Een logische Azure SQL Database-server en database
- Het bcp hulpprogramma is geïnstalleerd
- De opdrachtregel hulpprogramma sqlcmd geïnstalleerd

U kunt de hulpprogramma's bcp en sqlcmd downloaden van het [Microsoft Downloadcentrum][].

### <a name="data-in-ascii-or-utf-16-format"></a>Gegevens in ASCII- of UTF-16-indeling

Als u deze zelfstudie met uw eigen gegevens probeert, moet uw gegevens de ASCII- of UTF-16-codering gebruiken omdat bcp UTF-8 niet ondersteunt. 

## <a name="1-create-a-destination-table"></a>1. een tabel bestemming maken

Een tabel in SQL-Database als de doeltabel definiëren. De kolommen in de tabel moeten overeenkomen met de gegevens in elke rij van het gegevensbestand.

Een tabel maken, open een opdrachtprompt met behulp van sqlcmd.exe en voer de volgende opdracht:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
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


## <a name="next-steps"></a>Volgende stappen

Als u wilt migreren van een SQL Server-database, raadpleegt u [SQL Server-databasemigratie](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
