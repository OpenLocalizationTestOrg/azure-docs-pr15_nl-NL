<properties
   pageTitle="Schema's door gebruiker gedefinieerd in SQL Data Warehouse | Microsoft Azure"
   description="Tips voor het gebruik van schema's Transact-SQL in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
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

# <a name="user-defined-schemas-in-sql-data-warehouse"></a>Schema's door gebruiker gedefinieerd in SQL Data Warehouse

Afzonderlijke databases traditionele datawarehouses vaak gebruiken om te maken van de toepassingsgrenzen op basis van een werkbelasting, domein- of beveiliging. Een traditionele SQL Server data-warehouse kan bijvoorbeeld een tijdelijk opslaan database, een database met data warehouse en sommige gegevens mart-databases. In deze topologie wordt elke database werkt als een werkbelasting en de rand van de beveiliging in de architectuur.

Daarentegen in SQL Data Warehouse de werklast van de hele data warehouse binnen één database wordt uitgevoerd. Cross database zijn joins niet toegestaan. SQL Data Warehouse verwacht daarom alle tabellen wordt gebruikt door de BTW-records voor het in de één database zijn opgeslagen.

> [AZURE.NOTE] SQL Data Warehouse biedt geen ondersteuning voor cross databasequery's van elk type. Daarom moet data warehouse implementaties die gebruikmaken van deze patroon mogelijk worden gecorrigeerd.

## <a name="recommendations"></a>Aanbevelingen

Hierna ziet u aanbevelingen voor werkbelasting, beveiliging, domein en functionele grenzen consolideren met behulp van de gebruiker gedefinieerde schema's maken

1. Een database van SQL Data Warehouse gebruiken voor het uitvoeren van uw hele data warehouse werkbelasting
2. Uw bestaande gegevens warehouse-omgeving als u wilt gebruiken, één SQL Data Warehouse database consolideren
3. Gebruikmaken van **de gebruiker gedefinieerde schema's** die u wilt de grens eerder geïmplementeerd met behulp van databases bieden.

Als de gebruiker gedefinieerde schema's zijn niet eerder is gebruikt, hebt u een compleet leeg project. Gebruik de naam van de oude database gewoon als basis voor de gebruiker gedefinieerde schema's maken in de database van SQL Data Warehouse.

Als het schema's al zijn gebruikt hebt u een paar opties:

1. De schemanamen van de oudere verwijderen en opnieuw beginnen
2. De schemanamen van oudere door vooraf in behandeling de naam van het oude schema aan de naam van de tabel behouden
3. Behouden de schemanamen van de oudere door te implementeren van weergaven op de tabel in een extra schema om de structuur van het oude schema opnieuw te maken.

> [AZURE.NOTE] Klik op de eerste inspectie lijkt optie 3 de meest aantrekkelijke optie. De duivelse is echter in de details. Weergaven worden opgelezen alleen in SQL Data Warehouse. Gegevens of tabelopmaak wijzigingen moeten worden uitgevoerd met de basistabel. Optie 3 introduceert ook een laag van weergaven in uw systeem. U kunt dit even nadenken extra als u weergaven in uw architectuur al.


### <a name="examples"></a>Voorbeelden:

Door gebruiker gedefinieerd schema's op basis van databasenamen implementeren

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Behouden oudere schemanamen door vooraf in behandeling ze aan de naam van de tabel. Schema's maken voor de rand van de werklast gebruiken.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Behouden oudere schemanamen weergaven gebruiken

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [AZURE.NOTE] Een wijziging in het schema strategie moet een beoordeling van het beveiligingsmodel voor de database. In veel gevallen kunt u mogelijk het beveiligingsmodel vereenvoudigen door het toewijzen van machtigingen op het niveau van schema's. Als u meer gedetailleerde machtigingen zijn vereist kunt u databaserollen gebruiken.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer tips voor de ontwikkeling, [ontwikkelen-overzicht][].

<!--Image references-->

<!--Article references-->
[ontwikkelen-overzicht]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
