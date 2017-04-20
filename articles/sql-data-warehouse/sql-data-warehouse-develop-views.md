<properties
   pageTitle="Weergaven in SQL datawarehouse | Microsoft Azure"
   description="Tips voor het gebruik van Transact-SQL-weergaven in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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
   ms.date="07/01/2016"
   ms.author="jrj;barbkess;sonyama"/>


# <a name="views-in-sql-data-warehouse"></a>Weergaven in SQL datawarehouse

Weergaven zijn met name handig in SQL Data Warehouse. Ze kunnen worden gebruikt in een aantal verschillende manieren om de kwaliteit van de oplossing te verbeteren.  In dit artikel worden enkele voorbeelden van hoe u uw oplossing met weergaven, evenals de beperkingen die moeten worden beschouwd als Verrijk gemarkeerd.

> [AZURE.NOTE] Syntaxis voor `CREATE VIEW` niet in dit artikel wordt beschreven. Raadpleeg het artikel [Weergave maken][] op MSDN voor deze informatie.

## <a name="architectural-abstraction"></a>Architectuur abstraction
Een veelgebruikte toepassing patroon is om tabellen met de maken tabel als selecteren (CTAS) gevolgd door een object dat de naam van patroon wijzigen tijdens het laden van de gegevens opnieuw te maken.

In het onderstaande voorbeeld toevoegen nieuwe datum records aan een datumdimensie Houd er rekening mee hoe een nieuwe tabble, DimDate_New, is het eerst wordt gemaakt en wordt de naam gewijzigd als u wilt vervangen van de oorspronkelijke versie van de tabel.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Deze methode kan echter resulteren in tabellen die wordt weergegeven en verdwijnen uit de weergave van een gebruiker, evenals foutberichten "tabel bestaat niet". Weergaven kunnen worden gebruikt om gebruikers te bieden een consistente presentatie laag terwijl de namen van de onderliggende objecten worden gewijzigd. Doordat gebruikers toegang tot de gegevens door een weergaven, betekent dat gebruikers hoeven zichtbaarheid van de onderliggende tabellen. Hierdoor wordt een consistente gebruikersinterface terwijl ervoor te zorgen dat het datawarehouse ontwerpers kan het gegevensmodel ontwikkelen en prestaties met behulp van CTAS tijdens het laden van proces van gegevens.    

## <a name="performance-optimization"></a>Optimalisatie van prestaties
Weergaven kunnen ook worden gebruikt om af te dwingen prestaties geoptimaliseerd joins tussen tabellen. Een weergave kunt bijvoorbeeld een toets overtollige verdeling opnemen als onderdeel van de gekoppelde criteria te minimaliseren ten verplaatsing van gegevens.  Een ander voordeel van een weergave kan worden afdwingen van een specifieke query of gekoppelde tip. Weergaven gebruiken op deze manier zorgt ervoor dat joins altijd worden uitgevoerd op optimale wijze voorkomen dat gebruikers de juiste constructie voor hun joins onthouden.

## <a name="limitations"></a>Beperkingen
Weergaven in SQL Data Warehouse zijn alleen-metagegevens.  De volgende opties zijn dus niet beschikbaar:

-   Er is geen schema binding-optie
-   Tabellen kunnen niet worden bijgewerkt via de weergave
-   Weergaven kunnen niet worden gemaakt via tijdelijke tabellen
-   Er is geen ondersteuning voor de UITVOUWEN / NOEXPAND hints
-   Er zijn geen geïndexeerde weergaven in SQL Data Warehouse


## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor de ontwikkeling, [SQL Data Warehouse ontwikkelen-overzicht][].
Voor `CREATE VIEW` syntaxis Raadpleeg [Weergave maken][].

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse ontwikkelen-overzicht]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[WEERGAVE MAKEN]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
