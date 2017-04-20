<properties
   pageTitle="Uw oplossing migreren naar SQL Data Warehouse | Microsoft Azure"
   description="De richtlijnen van de migratie voor uw oplossing te brengen naar Azure SQL Data Warehouse-platform."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="barbkess;jrj;sonyama"/>

# <a name="migrate-your-solution-to-sql-data-warehouse"></a>Uw oplossing migreren naar SQL Data Warehouse

SQL Data Warehouse is een gedistribueerde database-systeem dat elastically wordt aangepast aan uw wensen. Als u wilt behouden prestaties en schaal, worden niet alle functies binnen SQL Server geïmplementeerd in SQL Data Warehouse. De volgende onderwerpen voor de migratie Raak in enkele van de basiselementen voor het migreren van uw oplossing naar SQL Data Warehouse. Ontwerpen datawarehouses voor schaal maakt u kennis met verschillende Ontwerpeffecten patronen en dus traditionele methoden niet altijd de beste. Daarom kan het gebeuren dat aanpassing van uw bestaande oplossing zorgt ervoor dat u optimaal profiteren van de verdeelde platform verstrekt door SQL Data Warehouse.

Het is ook belangrijk te weten dat SQL Data Warehouse een op basis in Microsoft Azure platform. Deel van de migratie kan dus ook omvat overzetten van uw gegevens in de cloud. Overdracht van gegevens is een onderwerp in een eigen rechts en voorzichtig; met name als volumes vergroten. Overdracht van gegevens en gegevens laden zijn afzonderlijke onderwerpen.

## <a name="migration-guidance"></a>Richtlijnen voor migratie

Zorg ervoor dat u hebt gelezen door deze artikelen om ervoor te zorgen dat u enkele product verschillen en basisconcepten kennen voordat u de migratie.

- [Migreren van uw schema][]
- [Migreren van uw gegevens][]
- [Migreren van uw code][]

## <a name="next-steps"></a>Volgende stappen

De kat (klant advies Team), heeft ook enkele uitstekende SQL Data Warehouse richtlijnen die ze via blogs publiceren.  Bekijk hun [gegevens migreren naar Azure SQL Data Warehouse in Word Web App][] voor meer informatie over het migratie-artikel.

<!--Image references-->

<!--Article references-->
[Migreren van uw schema]: sql-data-warehouse-migrate-schema.md
[Migreren van uw gegevens]: sql-data-warehouse-migrate-data.md
[Migreren van uw code]: sql-data-warehouse-migrate-code.md


<!--MSDN references-->


<!--Other Web references-->
[Gegevens migreert naar Azure SQL Data Warehouse in Word Web App]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
