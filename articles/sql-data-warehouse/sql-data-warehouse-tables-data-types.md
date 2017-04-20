<properties
   pageTitle="Gegevenstypen voor tabellen in SQL Data Warehouse | Microsoft Azure"
   description="Aan de slag met gegevenstypen voor Azure SQL Data Warehouse tabellen."
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
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="data-types-for-tables-in-sql-data-warehouse"></a>Gegevenstypen voor tabellen in SQL Data Warehouse

> [AZURE.SELECTOR]
- [Overzicht][]
- [Gegevenstypen][]
- [Distribueren][]
- [Index][]
- [Partition][]
- [Statistieken][]
- [Tijdelijke][]

Gegevenstypen wordt het meest worden gebruikt door SQL Data Warehouse ondersteunt.  Hieronder volgt een lijst met de gegevenstypen die worden ondersteund door SQL Data Warehouse.  Voor meer informatie over het gegevenstype ondersteuning: Zie [tabel maken][].

|**Ondersteunde gegevenstypen**|||
|---|---|---|
[bigint][]|[decimaal][]|[DateTime][]|
[binair getal][]|[toegestane achterstand][]|[smallmoney][]|
[bits][]|[int][]|[Sysname][]|
[teken][]|[geld][]|[tijd][]|
[datum][]|[nchar][]|[tinyint][]|
[datum /][]|[nvarchar][]|[unieke id][]|
[datetime2][]|[reële][]|[varbinary][]|
[DateTimeOffset][]|[smalldatetime][]|[varchar][]|


## <a name="data-type-best-practices"></a>Aanbevolen procedures voor het gegevenstype

 Wanneer u uw kolomtypen definieert, met het kleinste gegevenstype die ondersteuning voor uw gegevens bieden te verbeteren queryprestaties. Dit is vooral belangrijk is voor de kolommen teken en VARCHAR. Als de langste waarde in een kolom 25 tekens is, definieer vervolgens uw kolom als VARCHAR(25). Voorkomen dat alle tekenkolommen aan een grote standaardlengte definiëren. Bovendien kolommen definiëren als VARCHAR wanneer dat wordt gevormd door alle die nodig is, in plaats van [NVARCHAR][]gebruiken.  Gebruik NVARCHAR(4000) of VARCHAR(8000) indien mogelijk in plaats van NVARCHAR(MAX) of VARCHAR(MAX).

## <a name="polybase-limitation"></a>Polybase beperking

Als u Polybase gebruikt om te laden van tabellen, uw tabellen definiëren zodat de maximale mogelijke rijgrootte, inclusief de volledige lengte van kolommen met variabele lengte, niet meer dan 32.767 bytes.  Terwijl u een rij met variabele lengtegegevens die kunnen groter is dan deze breedte en rijen met BCP laden definiëren kunt, is het niet mogelijk Polybase gebruiken om deze gegevens te laden.  Polybase ondersteuning voor breed rijen wordt binnenkort toegevoegd.

## <a name="unsupported-data-types"></a>Niet-ondersteunde gegevenstypen

Als u de database van een ander SQL-platform zoals Azure SQL-Database, migreert zoals u migreren, kunt u sommige gegevenstypen die niet worden ondersteund op SQL Data Warehouse ondervinden.  Hieronder vindt u niet-ondersteunde gegevenstypen, evenals enkele alternatieven die u in plaats van niet-ondersteunde gegevenstypen gebruiken kunt.

|Gegevenstype|Tijdelijke oplossing|
|---|---|
|[geometrie][]|[varbinary][]|
|[Geografie][]|[varbinary][]|
|[activiteitknooppunt][]|[nvarchar][] (4000)|
|[afbeelding][ntext,text,image]|[varbinary][]|
|[tekst][ntext,text,image]|[varchar][]|
|[ntext][ntext,text,image]|[nvarchar][]|
|[sql_variant][]|Kolom splitsen in meerdere ten zeerste getypte kolommen.|
|[tabel][]|Converteren naar tijdelijke tabellen.|
|[tijdstempel][]|Herschrijf code voor het gebruik van [datetime2][] en `CURRENT_TIMESTAMP` functie.  Alleen constanten worden ondersteund als de standaardprogramma daarom current_timestamp kan niet worden gedefinieerd als een standaardbeperking. Als u wilt migreren van rijwaarden voor de versie van een tijdstempel getypte kolom gebruikt u [binaire][](8) of [VARBINARY][binaire](8) voor niet-null-waarden of null-waarden rij Versiewaarden.|
|[XML][]|[varchar][]|
|[door de gebruiker gedefinieerde typen][]|terug converteren naar hun systeemeigen typen waar mogelijk|
|standaardwaarden|standaardwaarden ondersteund letterlijke waarden en constanten alleen.  Niet-deterministisch expressies of functies, zoals `GETDATE()` of `CURRENT_TIMESTAMP`, worden niet ondersteund.|

De onderstaande SQL kan worden uitgevoerd op uw huidige SQL-database aan te geven kolommen die niet worden ondersteund door Azure SQL Data Warehouse:

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="next-steps"></a>Volgende stappen

Zie de artikelen op [Tabel overzicht][Overzicht], [een tabel distribueren][verdelen], [indexeren van een tabel][Index], [partitioneren van een tabel][Partition], [Statistische tabelgegevens onderhouden][Statistieken] en[tijdelijke] [Tijdelijke tabellen]voor meer informatie.  Zie voor meer informatie over aanbevolen werkwijzen voor [SQL Data Warehouse aanbevolen procedures][].

<!--Image references-->

<!--Article references-->
[Overzicht]: ./sql-data-warehouse-tables-overview.md
[Gegevenstypen]: ./sql-data-warehouse-tables-data-types.md
[Distribueren]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md
[Tijdelijke]: ./sql-data-warehouse-tables-temporary.md
[Aanbevolen procedures voor Warehouse SQL-gegevens]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
[tabel maken]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[binair getal]: https://msdn.microsoft.com/library/ms188362.aspx
[bits]: https://msdn.microsoft.com/library/ms177603.aspx
[teken]: https://msdn.microsoft.com/library/ms176089.aspx
[datum]: https://msdn.microsoft.com/library/bb630352.aspx
[datum /]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[DateTimeOffset]: https://msdn.microsoft.com/library/bb630289.aspx
[decimaal]: https://msdn.microsoft.com/library/ms187746.aspx
[toegestane achterstand]: https://msdn.microsoft.com/library/ms173773.aspx
[geometrie]: https://msdn.microsoft.com/library/cc280487.aspx
[Geografie]: https://msdn.microsoft.com/library/cc280766.aspx
[activiteitknooppunt]: https://msdn.microsoft.com/library/bb677290.aspx
[int]: https://msdn.microsoft.com/library/ms187745.aspx
[geld]: https://msdn.microsoft.com/library/ms179882.aspx
[nchar]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[reële]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[DateTime]: https://msdn.microsoft.com/library/ms187745.aspx
[smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[sql_variant]: https://msdn.microsoft.com/library/ms173829.aspx
[Sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[tabel]: https://msdn.microsoft.com/library/ms175010.aspx
[tijd]: https://msdn.microsoft.com/library/bb677243.aspx
[tijdstempel]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[unieke id]: https://msdn.microsoft.com/library/ms187942.aspx
[varbinary]: https://msdn.microsoft.com/library/ms188362.aspx
[varchar]: https://msdn.microsoft.com/library/ms186939.aspx
[XML]: https://msdn.microsoft.com/library/ms187339.aspx
[door de gebruiker gedefinieerde typen]: https://msdn.microsoft.com/library/ms131694.aspx
