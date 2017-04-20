<properties
   pageTitle="Groeperen op opties in SQL Data Warehouse | Microsoft Azure"
   description="Tips voor het implementeren van de groep door opties in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="group-by-options-in-sql-data-warehouse"></a>Groeperen op opties in SQL Data Warehouse

De [GROUP BY][] -component wordt gebruikt voor statistische gegevens aan een samenvatting aantal rijen. Er wordt ook een paar opties die de functionaliteit die moeten worden gewerkt rond als deze niet rechtstreeks worden ondersteund door Azure SQL Data Warehouse uitbreiden.

Deze opties zijn
- GROUP BY met ROLLUP
- GROEPERING SETS
- GROUP BY met kubus

## <a name="rollup-and-grouping-sets-options"></a>Hiermee stelt u opties Rollup en groeperen
De eenvoudigste optie hier is via `UNION ALL` in plaats daarvan de rollup uitvoeren in plaats van te vertrouwen op de syntaxis van de expliciete. Het resultaat is precies hetzelfde

Hieronder volgt een voorbeeld van een groep met behulp van de instructie de `ROLLUP` optie:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

Met behulp van ROLLUP hebben we de volgende aggregaties gevraagd:
- Land- en regio
- Land
- Eindtotaal

Voor het vervangen van dit moet u gebruiken `UNION ALL`; precisie van de aggregaties vereist expliciet om terug te keren dezelfde resultaten:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

Voor GROEPEREN SETS alle we moet doen bij het gebruik van dezelfde principal maar alleen maken UNION ALL secties voor aggregatie niveaus die we moeten worden weergegeven

## <a name="cube-options"></a>Kubusopties
Is het mogelijk te maken van een groep door met kubus met de UNION ALL aanpak. Het probleem is dat de code snel omslachtig en lastig worden kunt. U kunt deze meer geavanceerde methode gebruiken om te beperken Hiermee.

We gaan gebruiken met het bovenstaande voorbeeld.

De eerste stap is het definiëren van de 'kubus' waarin alle niveaus van de aggregatie die we wilt maken. Het is belangrijk te nemen van de CROSS JOIN van de twee afgeleide tabellen. Hiermee wordt gegenereerd alle niveaus voor ons. De rest van de code is echt er voor het opmaken van.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

De resultaten van de CTAS ziet u hieronder:

![][1]

De tweede stap is het een doeltabel om op te slaan tussentijdse resultaten opgeven:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

De derde stap is onze kubus van kolommen uitvoering van de aggregatie doorlopen. De query wordt uitgevoerd eenmaal voor elke rij in de tijdelijke #Cube-tabel en de resultaten op te slaan in de tijdelijke #Results-tabel

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Ten slotte kunnen we de resultaten worden geretourneerd door gewoon lezen van de tijdelijke #Results-tabel

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Door de code in secties splitsen en u genereert een herhaling constructie wordt de code beter worden beheerd en aanbrengen.


## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor de ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[GROEPEREN OP]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
