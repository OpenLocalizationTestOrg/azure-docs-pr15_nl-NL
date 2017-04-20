<properties
   pageTitle="Tabellen in SQL Data Warehouse indexeren | Microsoft Azure"
   description="Aan de slag met tabel indexering in Azure SQL Data Warehouse."
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
   ms.date="07/12/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="indexing-tables-in-sql-data-warehouse"></a>Tabellen in SQL Data Warehouse indexeren

> [AZURE.SELECTOR]
- [Overzicht][]
- [Gegevenstypen][]
- [Distribueren][]
- [Index][]
- [Partition][]
- [Statistieken][]
- [Tijdelijke][]

SQL Data Warehouse biedt diverse indexeringsopties, met inbegrip van [gegroepeerde columnstore indexen][], [gegroepeerde indexen en niet-gegroepeerde indexen][].  Daarnaast biedt het ook een de indexoptie geen ook bekend als [opslagruimte][].  In dit artikel worden de voordelen van elk indextype, evenals tips om te krijgen van de meeste prestaties afmelden bij uw indexen. Zie de [tabelsyntaxis van de te maken][] voor meer informatie over het maken van een tabel in SQL Data Warehouse.

## <a name="clustered-columnstore-indexes"></a>Gegroepeerd columnstore indexen

Standaard wordt in SQL Data Warehouse als u een index gegroepeerd columnstore gemaakt wanneer geen indexopties zijn opgegeven in een tabel. Gegroepeerd columnstore tabellen bieden zowel het hoogste niveau van gegevenscompressie als de beste queryprestaties.  Gegroepeerd columnstore tabellen worden gegroepeerd index of opslagruimte tabellen in het algemeen beter en zijn meestal de beste keuze voor grote tabellen.  Voor deze redenen is gegroepeerd columnstore het beste beginpunt wanneer u niet hoe weet u uw tabel indexeren.  

Een gegroepeerd columnstore als tabel wilt maken, gewoon GECLUSTERDE COLUMNSTORE INDEX in de WITH-clausule opgeven of de WITH-clausule uitgeschakeld laat:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );
```

Er zijn een paar scenario's die zijn gegroepeerd columnstore mogelijk waar niet in een goede optie:

- Columnstore tabellen ondersteunen geen secundaire niet-gegroepeerde indexen.  Houd rekening met opslagruimte of gegroepeerde index tabellen in plaats daarvan.
- Columnstore tabellen ondersteunen geen varchar(max), nvarchar(max) en varbinary(max).  In plaats daarvan kunt u opslagruimte of gegroepeerde index.
- Het is mogelijk dat Columnstore tabellen minder efficiënt voor tijdelijke gegevens.  Houd rekening met opslagruimte en misschien zelfs tijdelijke tabellen.
- Kleine tabellen met minder dan 100 miljoen rijen.  Houd rekening met opslagruimte tabellen.

## <a name="heap-tables"></a>Opslagruimte tabellen

Wanneer u gegevens tijdelijk op SQL Data Warehouse lossen zijn, kan het gebeuren dat de algehele proces sneller als u een met behulp van een tabel opslagruimte maken zal.  Dit komt doordat laadtijd naar heaps zijn sneller dan naar index tabellen en klik in sommige gevallen die het lezen uit cache kan worden uitgevoerd.  Als u gegevens alleen als u wilt deze voordat u meer transformaties van fase laadt, het laden van de tabel aan opslagruimte tabel niet veel sneller dan het laden van de gegevens naar een tabel gegroepeerd columnstore. Daarnaast wordt laden van gegevens naar een [tijdelijke tabel][tijdelijke] ook veel sneller geladen dan het laden van een tabel met permanente storage.  

Voor kleine opzoektabellen, minder dan 100 miljoen rijen, logisch vaak opslagruimte tabellen.  Cluster columnstore tabellen beginnen met het optimale compressie bereiken nadat er meer dan 100 miljoen rijen is.

Als u wilt een tabel opslagruimte maakt, moet u gewoon opslagruimte opgeven in de WITH-clausule:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( HEAP );
```

## <a name="clustered-and-nonclustered-indexes"></a>Gegroepeerde en niet-gegroepeerde indexen

Gegroepeerde indexen mogelijk gegroepeerd columnstore tabellen beter als één rij moet snel worden opgehaald.  Voor query's waarin één of weinig rij opzoeken verplicht voor het prestaties met extreme snelheid is, kunt u een clusterindex of niet-geclusterde secundaire index.  Het nadeel van een gegroepeerde index is dat alleen de query's die een zeer selectief filter op de gegroepeerde indexkolom gebruiken profiteren.  Als u wilt filteren op andere kolommen verbeteren kunt geclusterde index worden toegevoegd aan andere kolommen.  Echter wordt elke index die wordt toegevoegd aan een tabel ruimte en de verwerkingstijd toevoegen aan de laadtijd voor.

Als u wilt een geclusterde indextabel maakt, moet u gewoon GECLUSTERDE INDEX opgeven in de WITH-clausule:

```SQL
CREATE TABLE myTable   
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED INDEX (id) );
```

Als u wilt toevoegen als u een niet-gegroepeerde index voor een tabel, moet u gewoon de volgende syntaxis gebruiken:

```SQL
CREATE INDEX zipCodeIndex ON t1 (zipCode);
```

> [AZURE.NOTE] Als u een niet-gegroepeerde index wordt standaard gemaakt wanneer CREATE INDEX wordt gebruikt. Als u een niet-gegroepeerde index is bovendien alleen toegestaan voor een rij opslag tabel (opslagruimte of GEGROEPEERDE INDEX). Niet-gegroepeerde indexen boven aan een GEGROEPEERDE COLUMNSTORE INDEX is niet toegestaan op dit moment.


## <a name="optimizing-clustered-columnstore-indexes"></a>Gegroepeerd columnstore indexen optimaliseren

Gegroepeerd columnstore tabellen worden ingedeeld in gegevens in segmenten.  Met hoge segment kwaliteit is belangrijk dat het optimale queryprestaties op een tabel columnstore bereiken.  Segment kwaliteit kan worden gemeten door het aantal rijen in een rijgroep gecomprimeerde.  Segment kwaliteit is meest optimaal wanneer er minimaal 100K rijen per gecomprimeerde rij groeperen en krijgen prestaties als het aantal rijen per rij groep aanpak 1.048.576 rijen, namelijk de meeste rijen die een rijgroep kan bevatten.

De onder weergave kan worden gemaakt en gebruikt op uw systeem te berekenen van de gemiddelde rijen per rij groeperen en sub optimale cluster columnstore indexen identificeren.  De laatste kolom op deze weergave wordt gegenereerd als SQL-instructie die kan worden gebruikt om uw indexen opnieuw opbouwen.

```sql
CREATE VIEW dbo.vColumnstoreDensity
AS
SELECT
        GETDATE()                                                               AS [execution_date]
,       DB_Name()                                                               AS [database_name]
,       s.name                                                                  AS [schema_name]
,       t.name                                                                  AS [table_name]
,   COUNT(DISTINCT rg.[partition_number])                   AS [table_partition_count]
,       SUM(rg.[total_rows])                                                    AS [row_count_total]
,       SUM(rg.[total_rows])/COUNT(DISTINCT rg.[distribution_id])               AS [row_count_per_distribution_MAX]
,   CEILING ((SUM(rg.[total_rows])*1.0/COUNT(DISTINCT rg.[distribution_id]))/1048576) AS [rowgroup_per_distribution_MAX]
,       SUM(CASE WHEN rg.[State] = 0 THEN 1                   ELSE 0    END)    AS [INVISIBLE_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE 0    END)    AS [INVISIBLE_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 0 THEN rg.[total_rows]     ELSE NULL END)    AS [INVISIBLE_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 1 THEN 1                   ELSE 0    END)    AS [OPEN_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE 0    END)    AS [OPEN_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 1 THEN rg.[total_rows]     ELSE NULL END)    AS [OPEN_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 2 THEN 1                   ELSE 0    END)    AS [CLOSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE 0    END)    AS [CLOSED_rowgroup_rows]
,       MIN(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 2 THEN rg.[total_rows]     ELSE NULL END)    AS [CLOSED_rowgroup_rows_AVG]
,       SUM(CASE WHEN rg.[State] = 3 THEN 1                   ELSE 0    END)    AS [COMPRESSED_rowgroup_count]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE 0    END)    AS [COMPRESSED_rowgroup_rows]
,       SUM(CASE WHEN rg.[State] = 3 THEN rg.[deleted_rows]   ELSE 0    END)    AS [COMPRESSED_rowgroup_rows_DELETED]
,       MIN(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MIN]
,       MAX(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_MAX]
,       AVG(CASE WHEN rg.[State] = 3 THEN rg.[total_rows]     ELSE NULL END)    AS [COMPRESSED_rowgroup_rows_AVG]
,       'ALTER INDEX ALL ON ' + s.name + '.' + t.NAME + ' REBUILD;'             AS [Rebuild_Index_SQL]
FROM    sys.[pdw_nodes_column_store_row_groups] rg
JOIN    sys.[pdw_nodes_tables] nt                   ON  rg.[object_id]          = nt.[object_id]
                                                    AND rg.[pdw_node_id]        = nt.[pdw_node_id]
                                                    AND rg.[distribution_id]    = nt.[distribution_id]
JOIN    sys.[pdw_table_mappings] mp                 ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[tables] t                              ON  mp.[object_id]          = t.[object_id]
JOIN    sys.[schemas] s                             ON t.[schema_id]            = s.[schema_id]
GROUP BY
        s.[name]
,       t.[name]
;
```

Nu dat u de weergave hebt gemaakt, kunt u deze query om te identificeren tabellen met Rijgroepen met minder dan 100K rijen uitvoeren.  U wilt natuurlijk Verhoog de drempel van 100 kB als u meer optimale segment kwaliteit zoekt. 

```sql
SELECT  *
FROM    [dbo].[vColumnstoreDensity]
WHERE   COMPRESSED_rowgroup_rows_AVG < 100000
        OR INVISIBLE_rowgroup_rows_AVG < 100000
```

Nadat u de query die u beginnen kunt met de gegevens wilt bekijken en analyseren van uw resultaten hebt uitgevoerd. In deze tabel wordt uitgelegd wat u moet worden gezocht in uw analyse van de groep rij.


| Kolom                             | Het gebruik van deze gegevens                                                                                                                                                                      |
| ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [table_partition_count]            | Als de tabel is partitioneren, mogelijk om te zien hoger openen rijgroep telt u verwacht. Elke partition in de verdeling wellicht in theorie een open rijgroep worden gekoppeld. Rekening houden met factoren dit uw analyse. Een kleine tabel die heeft zijn partities kan worden geoptimaliseerd doordat de partities helemaal zoals dit compressie wilt verbeteren.                                                                        |
| [row_count_total]                  | Rij Totaal aantal voor de tabel. Bijvoorbeeld, kunt u deze waarde om percentage van de rijen in de gecomprimeerde stand te berekenen.                                                                      |
| [row_count_per_distribution_MAX]   | Als u alle rijen gelijkmatig zijn verdeeld deze waarde wordt het doel aantal rijen per verdeling. Vergelijk deze waarde met de compressed_rowgroup_count.                                 |
| [COMPRESSED_rowgroup_rows]         | Totaal aantal rijen in de indeling columnstore voor de tabel.                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_AVG]     | Als het gemiddelde aantal rijen aanzienlijk kleiner is dan het maximum aantal rijen voor een rijgroep is, klikt u vervolgens kunt u overwegen CTAS of wijzigen voor de INDEX opnieuw OPBOUWEN aan de gegevens opnieuw te comprimeren                     |
| [COMPRESSED_rowgroup_count]        | Het aantal Rijgroepen in columnstore-indeling. Als dit nummer zeer hoog ten opzichte van de tabel is is een aanduiding dat de dichtheid columnstore laag is.                                  |
| [COMPRESSED_rowgroup_rows_DELETED] | Rijen worden logisch verwijderd in columnstore-indeling. Als het getal hoog ten opzichte van de tabelgrootte is, kunt u opnieuw te maken de partition of de index opnieuw maken, zoals Hiermee verwijdert u deze fysiek. |
| [COMPRESSED_rowgroup_rows_MIN]     | Gebruik deze in combinatie met de kolommen gemiddelde en MAX voor meer informatie over het bereik van waarden voor de Rijgroepen in uw columnstore. Een laag getal de laden overschrijdt (102,400 per partition uitgelijnd verdeling) stappen worden voorgesteld dat optimalisaties beschikbaar in de gegevens zijn geladen zijn                                                                                                                                                 |
| [COMPRESSED_rowgroup_rows_MAX]     | Hierboven                                                                                                                                                                                  |
| [OPEN_rowgroup_count]              | Open Rijgroepen zijn normale. Een zou verwachten één OPEN rijgroep per tabel verdeling (60). Overtollige getallen voorgesteld gegevens laden meerdere partities. Controleer de partities strategie om te controleren of het is geluid                                                                                                                                                                                                |
| [OPEN_rowgroup_rows]               | Elke rijgroep kan erin als maximaal 1.048.576 rijen hebben. Gebruikt u deze waarde om te zien hoe volledige de Rijgroepen openen zijn momenteel                                                                 |
| [OPEN_rowgroup_rows_MIN]           | Open groepen wordt aangegeven dat gegevens op nieuwe wordt geladen in de tabel of dat de vorige laden overgeslagen resterende rijen naar deze rijgroep. De MIN en MAX gebruiken, Gem kolommen om te zien hoeveel gegevens in die is geopend is sat rij groepen. Voor kleine tabellen mogelijk 100% van alle gegevens! In dat geval ALTER INDEX opnieuw OPBOUWEN om te zorgen dat de gegevens naar columnstore.                                                                       |
| [OPEN_rowgroup_rows_MAX]           | Hierboven                                                                                                                                                                                  |
| [OPEN_rowgroup_rows_AVG]           | Hierboven                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows]             | Bekijk de rijen van de groep gesloten rij als een controle.                                                                                                                                       |
| [CLOSED_rowgroup_count]            | Het aantal gesloten Rijgroepen moet lage als een helemaal zijn zichtbaar. Gesloten Rijgroepen kunnen worden geconverteerd naar gecomprimeerde rowg roups met de INDEX wijzigen... Opdracht SANEREN. Dit is echter niet normaal vereist. Gesloten groepen worden automatisch geconverteerd naar columnstore Rijgroepen door de achtergrond "tupel verplaatsen" proces.                                                                                               |
| [CLOSED_rowgroup_rows_MIN]         | Gesloten Rijgroepen moeten een tarief zeer hoog opvulling hebben. Als het tarief weer dat opvulling voor een rijgroep gesloten laag is, klikt u vervolgens is verdere analyse van de columnstore vereist.                                   |
| [CLOSED_rowgroup_rows_MAX]         | Hierboven                                                                                                                                                                                  |
| [CLOSED_rowgroup_rows_AVG]         | Hierboven                                                                                                                                                                                  |
| [Rebuild_Index_SQL]         | SQL om opnieuw columnstore index voor een tabel te maken                                                                                                                                                     |

## <a name="causes-of-poor-columnstore-index-quality"></a>Oorzaken van slechte columnstore index kwaliteit

Als u tabellen hebt vastgesteld met slechte segment kwaliteit, wilt u de onderliggende oorzaak identificeren.  Hieronder vindt u enkele andere veelvoorkomende oorzaken van slechte segment quaility:

1. Geheugendruk wanneer index is gebouwd
2. Groot aantal DML-bewerkingen
3. Kleine of doorsijpelen laden bewerkingen
4. Te veel partities

Deze factoren kunnen leiden tot een index columnstore aanzienlijk heeft die kleiner is dan de optimale 1 miljoen rijen per rijgroep.  Ze kunnen ook leiden tot rijen om te gaan naar de groep van de rij delta in plaats van een rijgroep gecomprimeerde. 

### <a name="memory-pressure-when-index-was-built"></a>Geheugendruk wanneer index is gebouwd

Het aantal rijen per gecomprimeerde rijgroep zijn rechtstreeks gerelateerd aan de breedte van de rij en de hoeveelheid geheugen beschikbaar om te verwerken van de rijgroep.  Wanneer rijen worden geschreven naar columnstore tabellen onder geheugendruk, kan worden beïnvloed door columnstore segment kwaliteit.  Het wordt aanbevolen dus geven de sessie aan die naar uw columnstore index tabellen toegang tot zo veel mogelijk geheugen schrijft.  Omdat er een verhouding tussen geheugen en gelijktijdigheid, de richtlijnen voor het juiste geheugentoewijzing, is afhankelijk van de gegevens in elke rij van de tabel, de hoeveelheid DWU die u hebt toegewezen aan uw systeem en de hoeveelheid gelijktijdigheid sleuven die u kunt geven tot de sessie waarin gegevens naar de tabel schrijft.  Als een goede gewoonte, is het raadzaam te beginnen met xlargerc als u DW300 gebruikt of minder, largerc als u DW400 DW600 en mediumrc gebruikt als u DW1000 gebruikt en hoger.

### <a name="high-volume-of-dml-operations"></a>Groot aantal DML-bewerkingen

Een groot aantal DML-bewerkingen die bijwerken en verwijderen van rijen kunt ondoelmatigheid in de columnstore introduceren. Dit geldt met name als het grootste deel van de rijen in een rijgroep worden gewijzigd.

- De rij voor het verwijderen van een rij uit een rijgroep gecomprimeerde is het alleen logisch wordt gemarkeerd als verwijderd. De rij blijft in de rijgroep gecomprimeerde totdat het partition of de tabel wordt opnieuw gemaakt.
- Invoegen van een rij, wordt de rij toegevoegd aan een interne rowstore tabel met de naam van een groep van de rij delta. De ingevoegde rij wordt niet geconverteerd naar columnstore totdat de rij-groep delta vol is en is gemarkeerd als gesloten. Rijgroepen zijn gesloten nadat ze de maximale capaciteit van 1.048.576 rijen hebt bereikt. 
- Bijwerken van een rij in de indeling columnstore wordt verwerkt als een logische verwijderen en klik vervolgens op een invoegen. De ingevoegde rij kan worden opgeslagen in de winkel delta.

Batch verwerkt bijwerken en Voeg bewerkingen die groter is dan de drempelwaarde voor bulksgewijs van 102.400 rijen per partition uitgelijnde verdeling rechtstreeks naar de columnstore-indeling worden geschreven. Echter, ervan uitgaande dat er een even verdeling, moet u meer dan 6.144 miljoen rijen in één bewerking hiervoor wijzigt. Als het aantal rijen voor een bepaald partition uitgelijnd is verdeling minder dan 102,400 de rijen worden doorgestuurd naar de store delta en blijven er totdat voldoende rijen zijn ingevoegd of gewijzigd om te sluiten van de rijgroep of de index is gewijzigd.

### <a name="small-or-trickle-load-operations"></a>Kleine of doorsijpelen laden bewerkingen

Kleine wordt geladen dat stroom in SQL Data Warehouse soms ook bekend als doorsijpelen laadtijd. Meestal staan voor een dichtbijzijnd constant gegevensstroom wordt binnenkrijgen aan het systeem. Als deze stroom bijna continue is is het volume van rijen echter niet bijzonder grote. De gegevens zijn vaak aanzienlijk onder de drempelwaarde voor vereist voor een directe laden naar columnstore-indeling.

In deze situaties is het vaak beter om de gegevens eerst terechtkomt in Azure-blobopslag en laat deze vóór het laden. Deze methode is vaak bekend als *micro batchen*.

### <a name="too-many-partitions"></a>Te veel partities

Iets anders u rekening moet houden is de invloed van de partities op uw gegroepeerd columnstore-tabellen.  Voordat u partitioneren verdeelt SQL Data Warehouse al uw gegevens in 60 databases.  Uw gegevens partitioneren verder worden opgedeeld.  Als u uw gegevens staan, wordt u rekening moet houden dat **elke** partition moet ten minste 1 miljoen rijen om te profiteren van een gegroepeerd columnstore index hebt u wilt.  Als u de tabel in 100 partities partitioneren, moet uw tabel ten minste 6 miljard rijen om te profiteren van een gegroepeerd columnstore index (60 onderzoeken *100 partities* 1 miljoen rijen) bevatten. Als uw partitietabel 100 geen 6 miljard rijen heeft, Verminder het aantal partities of kunt u overwegen een tabel opslagruimte in plaats daarvan.

Wanneer uw tabellen met enkele gegevens zijn geladen, voert u de onderstaande stappen te volgen herkennen en tabellen met sub optimale cluster columnstore indexen opnieuw maken.

## <a name="rebuilding-indexes-to-improve-segment-quality"></a>Opnieuw samenstellen van indexen segment kwaliteitsverbetering

### <a name="step-1-identify-or-create-user-which-uses-the-right-resource-class"></a>Stap 1: Zoek of maak gebruiker waarin de juiste resource klasse

Een snelle manier onmiddellijk segment kwaliteitsverbetering is de index opnieuw opbouwen.  De SQL geretourneerd door de bovenstaande weergave retourneert een ALTER INDEX opnieuw OPBOUWEN-instructie die kan worden gebruikt om uw indexen opnieuw opbouwen.  Wanneer opnieuw met het samenstellen van uw indexen, moet u dat u onvoldoende geheugen aan de sessie waarop de index wordt opnieuw toewijzen.  Klik hiertoe vergroot u de klas resource van een gebruiker met machtigingen om de index in deze tabel naar de aanbevolen minimum opnieuw te maken.  De resource klas van de database-eigenaar-gebruiker kan niet worden gewijzigd, dus als u een gebruiker niet op het systeem hebt gemaakt, moet u eerst te doen.  Het minimum dat wordt aangeraden is xlargerc als u DW300 gebruikt of kleiner, largerc als u DW400 DW600 en mediumrc gebruikt als u DW1000 gebruikt en hoger.

Hieronder volgt een voorbeeld van hoe meer geheugen aan een gebruiker toewijzen door te verhogen van hun class resource.  Voor meer informatie over resource klassen en hoe u een nieuwe gebruiker maakt vindt u in het [gelijktijdigheid en werkbelasting management] [ Concurrency] artikel.

```sql
EXEC sp_addrolemember 'xlargerc', 'LoadUser'
```

### <a name="step-2-rebuild-clustered-columnstore-indexes-with-higher-resource-class-user"></a>Stap 2: Gegroepeerd columnstore-indexen opnieuw opbouwen met hogere resource class gebruiker
Meld u aan als de gebruiker in stap 1 (bijvoorbeeld LoadUser), die is nu met behulp van een hogere resource-klasse en voer de instructies ALTER INDEX.  Zorg ervoor dat deze gebruiker ALTER machtiging aan de tabellen waarbij de index wordt opnieuw opgebouwd.  Deze voorbeelden wordt getoond hoe u de hele columnstore-index opnieuw maken of hoe om een enkele partition opnieuw te maken. Bij grote tabellen is het meer praktische tot bouw die tabellen opnieuw een enkel partition indexeert tegelijk.

In plaats van de index opnieuw, kunt u ook de tabel kopiëren naar een nieuwe tabel met [CTAS][].  Welke het beste is? Voor grote hoeveelheden gegevens is [CTAS][] meestal sneller dan [ALTER INDEX][]. [ALTER INDEX][] is eenvoudig te gebruiken en won't, moet u de tabel verwisselt voor kleinere hoeveelheden gegevens.  Zie **Rebuilding indexen met CTAS en partition overstappen** hieronder voor meer informatie over hoe u indexen met CTAS opnieuw opbouwen.

```sql
-- Rebuild the entire clustered index
ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD
```

```sql
-- Rebuild a single partition
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5
```

```sql
-- Rebuild a single partition with archival compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE_ARCHIVE)
```

```sql
-- Rebuild a single partition with columnstore compression
ALTER INDEX ALL ON [dbo].[FactInternetSales] REBUILD Partition = 5 WITH (DATA_COMPRESSION = COLUMNSTORE)
```

Een index in SQL Data Warehouse opnieuw is een offline bewerking.  Zie de sectie wijzigen voor de INDEX opnieuw OPBOUWEN in [Columnstore indexen defragmentatie][] en het onderwerp syntaxis [ALTER INDEX][]voor meer informatie over het opnieuw indexen maken.
 
### <a name="step-3-verify-clustered-columnstore-segment-quality-has-improved"></a>Stap 3: Controleren of gegroepeerd columnstore segment kwaliteit is verbeterd
Opnieuw uitvoeren van de query welke geïdentificeerd tabel met slecht segmenteren kwaliteit en controleer of segment kwaliteit verbeterd.  Als het segment kwaliteit hebt niet verbeteren, kan het zijn dat de rijen in de tabel extra breed zijn.  Kunt u overwegen een hogere resource class of DWU bij het opnieuw opbouwen uw indexen.

 
## <a name="rebuilding-indexes-with-ctas-and-partition-switching"></a>Opnieuw samenstellen van indexen met CTAS en partition over te schakelen

In dit voorbeeld worden [CTAS][] en partition overstappen op te bouwen van een tabel partition. 

```sql
-- Step 1: Select the partition of data and write it out to a new table using CTAS
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

-- Step 2: Create a SWITCH out table
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE   1=2 -- Note this table will be empty

-- Step 3: Switch OUT the data 
ALTER TABLE [dbo].[FactInternetSales] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales_20000101] PARTITION 2;

-- Step 4: Switch IN the rebuilt data
ALTER TABLE [dbo].[FactInternetSales_20000101_20010101] SWITCH PARTITION 2 TO  [dbo].[FactInternetSales] PARTITION 2;
```

Voor meer informatie over het opnieuw te maken met partities `CTAS`, raadpleegt u het artikel [Partition][] .

## <a name="next-steps"></a>Volgende stappen

Zie de artikelen op [Tabel overzicht][Overzicht], [Tabel gegevenstypen][Gegevenstypen], [een tabel distribueren][verdelen], [partitioneren van een tabel][Partition], [Statistische tabelgegevens onderhouden][Statistieken] en[tijdelijke] [Tijdelijke tabellen]voor meer informatie.  Zie meer informatie over aanbevolen werkwijzen voor [SQL Data Warehouse aanbevolen procedures][].

<!--Image references-->

<!--Article references-->
[Overzicht]: ./sql-data-warehouse-tables-overview.md
[Gegevenstypen]: ./sql-data-warehouse-tables-data-types.md
[Distribueren]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistieken]: ./sql-data-warehouse-tables-statistics.md
[Tijdelijke]: ./sql-data-warehouse-tables-temporary.md
[Concurrency]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Aanbevolen procedures voor Warehouse SQL-gegevens]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[ALTER INDEX]: https://msdn.microsoft.com/library/ms188388.aspx
[opslagruimte]: https://msdn.microsoft.com/library/hh213609.aspx
[gegroepeerde indexen en niet-gegroepeerde indexen]: https://msdn.microsoft.com/library/ms190457.aspx
[de tabelsyntaxis van de maken]: https://msdn.microsoft.com/library/mt203953.aspx
[Columnstore indexen defragmentatie]: https://msdn.microsoft.com/library/dn935013.aspx#Anchor_1
[gegroepeerd columnstore indexen]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
