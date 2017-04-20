<properties
   pageTitle="Voorbeeldgegevens laden in SQL Data Warehouse | Microsoft Azure"
   description="Voorbeeldgegevens in SQL Data Warehouse laden"
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
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="load-sample-data-into-sql-data-warehouse"></a>Voorbeeldgegevens in SQL Data Warehouse laden

Volg deze eenvoudige stappen als u wilt laden en de voorbeelddatabase Adventure Works query. Deze scripts gebruikt u eerst sqlcmd om uit te voeren SQL waarin tabellen en weergaven maakt. Zodra tabellen zijn gemaakt, zal de scripts bcp gebruiken om gegevens te laden.  Als u nog niet sqlcmd en bcp is geïnstalleerd, volgt u deze koppelingen [bcp installeren][] en [sqlcmd installeren][].

##<a name="load-sample-data"></a>Voorbeeldgegevens laden

1. Download het [Adventure Works-voorbeeldscripts voor SQL Data Warehouse][] zip-bestand.

2. De bestanden ophalen uit gedownloade zip naar een map op uw lokale computer.

3. De opgehaalde bestand aw_create.bat bewerken en instellen van de volgende variabelen gevonden boven aan het bestand.  Zorg ervoor dat u geen witruimte tussen laat de "=" en de parameter.  Hieronder ziet u voorbeelden van hoe de bewerkingen uitzien.

    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```

4. Uitvoeren vanaf een Windows cmd-prompt de bewerkte aw_create.bat.  Zorg ervoor dat u in de map waar u uw bewerkte versie van aw_create.bat hebt opgeslagen.
Dit script wordt...
    * Adventure Works tabellen of weergaven die zich al in uw database weghalen
    * Maak de Adventure Works-tabellen en weergaven
    * Elke Adventure Works-tabel met bcp laden
    * Valideer de Rijtellingen voor elke tabel Adventure Works
    * Statistische gegevens verzamelen in elke kolom voor elke tabel Adventure Works


##<a name="query-sample-data"></a>Voorbeeldgegevens voor query

Wanneer u enkele voorbeeldgegevens in uw SQL Data Warehouse hebt geladen, kunt u snel een paar query's uitvoeren.  Als u wilt een query uitvoert, verbinding maken met uw gemaakte Adventure Works-database in Azure SQL-DW Gebruik Visual Studio en SSDT, zoals beschreven in de [query met Visual Studio][] -document.

Voorbeeld van eenvoudige select-instructie om alle informatie van de werknemers:

```sql
SELECT * FROM DimEmployee;
```

Voorbeeld van een meer complexe query constructies, zoals GROUP BY gebruiken om te zoeken op het totaalbedrag voor alle verkoop op elke dag:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Voorbeeld van een selectie met een WHERE-component uitfilteren orders uit voor een bepaalde datum:

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

SQL Data Warehouse biedt ondersteuning voor bijna alle T-SQL-constructies die ondersteuning biedt voor SQL Server.  Eventuele verschillen worden beschreven in onze documentatie [code migreren][] .

## <a name="next-steps"></a>Volgende stappen
Nu dat u uitproberen sommige query's met voorbeeldgegevens hebt, controleert u informatie over het [ontwikkelen][], [laden][]of [migreren][] naar SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[migreren]: sql-data-warehouse-overview-migrate.md
[ontwikkelen]: sql-data-warehouse-overview-develop.md
[laden]: sql-data-warehouse-overview-load.md
[query met Visual Studio]: sql-data-warehouse-query-visual-studio.md
[code migreren]: sql-data-warehouse-migrate-code.md
[bcp installeren]: sql-data-warehouse-load-with-bcp.md
[sqlcmd installeren]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works-voorbeeldscripts voor SQL datawarehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
