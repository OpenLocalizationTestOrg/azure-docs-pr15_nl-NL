<properties
   pageTitle="SQL Data Warehouse herstellen | Microsoft Azure"
   description="Overzicht van de opties voor het herstellen van database voor het herstellen van een database in Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/29/2016"
   ms.author="lakshmir;barbkess;sonyama"/>


# <a name="sql-data-warehouse-restore"></a>SQL Data Warehouse herstellen

> [AZURE.SELECTOR]
- [Overzicht][]
- [Portal][]
- [PowerShell][]
- [REST][]

SQL Data Warehouse biedt zowel lokale als geografische Hiermee herstelt als onderdeel van de warehouse gegevensverlies herstelmogelijkheden. Back-ups van gegevens warehouse gebruikt om te zetten van uw BTW-records voor de gegevens naar een punt terugzetten in de primaire regio of geografische-redundante back-ups om te zetten naar een ander geografisch gebied. In dit artikel wordt uitgelegd dat de details van een data-warehouse herstellen.

## <a name="what-is-a-data-warehouse-restore"></a>Wat is een data warehouse herstellen?

Een data warehouse terugzetten is een nieuwe datawarehouse die is gemaakt van een back-up van een bestaande of verwijderde gegevens BTW-records. Back-ups van datawarehouse herstelde datawarehouse opnieuw gemaakt op een bepaald tijdstip. Aangezien SQL Data Warehouse een gedistribueerd systeem is, wordt een data warehouse herstellen wordt gemaakt van veel back-bestanden die zijn opgeslagen in Azure BLOB's. 

Database terugzetten is een essentieel onderdeel van een strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel omdat deze opnieuw uw gegevens na ongeluk beschadigde bestanden of verwijdering wordt gemaakt.

Zie voor meer informatie:

-  [Back-ups van SQL Data Warehouse](sql-data-warehouse-backups.md)
-  [Bedrijfscontinuïteit voor bedrijfsoverzichten](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a>Gegevenspunten BTW-records herstellen

Een voordeel van het gebruik van Azure Premium Storage SQL Data Warehouse worden met Azure opslag Blob momentopnamen back-up maken van het primaire datawarehouse. Elke momentopname heeft een punt herstellen die de tijd die het momentopname starten vertegenwoordigt. Als u een data-warehouse herstellen, kiest u een punt herstellen en een opdracht herstellen.  

SQL Data Warehouse herstelt u altijd de back-up naar een nieuwe data-warehouse. U kunt het herstelde datawarehouse en de huidige rij houden, of een van deze verwijderen. Als u het huidige datawarehouse vervangen door het herstelde datawarehouse wilt, kunt u deze kunt wijzigen.

Als u een verwijderde of onderbroken data-warehouse herstellen moet, kunt u [een ondersteuningsticket maken](sql-data-warehouse-get-started-create-support-ticket.md). 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore the last available restore point.

Yes, for the next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps the data warehouse and its snapshots for seven days just in case you need the data. After seven days, you won't be able to restore to any of the restore points. -->

## <a name="geo-redundant-restore"></a>Geografische-redundante herstellen

Als u de geografische-redundante opslag gebruikt, kunt u het datawarehouse terugzetten naar uw [gepaarde Datacenter](../best-practices-availability-paired-regions.md) in een ander geografisch gebied. Datawarehouse is uit de laatste dagelijkse back-up hersteld. 

## <a name="restore-timeline"></a>Tijdlijn herstellen

In de afgelopen zeven dagen, kunt u een database terugzetten in een willekeurige plaats beschikbaar herstellen. Momentopnamen starten om de vier tot acht uur en beschikbaar zijn voor zeven dagen. Wanneer een momentopname ouder dan zeven dagen is, het verloopt en de punt herstellen is niet langer beschikbaar.

## <a name="restore-costs"></a>Kosten herstellen

De kosten van de opslagruimte voor herstelde datawarehouse gefactureerd Azure Premium Storage snelheid. 

Als u een herstelde data-warehouse onderbreekt, wordt geheven voor opslagruimte met het tarief weer dat Azure Premium opslag. Het voordeel van onderbreking is dat u geen btw geheven voor de DWU computing resources.

Zie [SQL Data Warehouse prijzen](https://azure.microsoft.com/pricing/details/sql-data-warehouse/)voor meer informatie over SQL Data Warehouse prijzen.

## <a name="uses-for-restore"></a>Herstellen kunnen worden gebruikt.

Het primaire gebruik voor data warehouse herstellen is om gegevens te herstellen na verlies van gegevens per ongeluk of beschadigde bestanden.

U kunt ook data warehouse herstellen gebruiken om te bewaren van een back-up langer dan zeven dagen. Als de back-up is hersteld, kunt u het datawarehouse online hebt en kunt onderbreken voor onbepaalde tijd om op te slaan berekeningscluster kosten. De onderbroken database bijhoudt opslag kosten Azure Premium Storage snelheid. 

## <a name="related-topics"></a>Verwante onderwerpen

### <a name="scenarios"></a>Scenario 's

- Zie [overzicht van de bedrijven bedrijfscontinuïteit](../sql-database/sql-database-business-continuity.md) voor een overzicht van de bedrijfscontinuïteit bedrijven


<!-- ### Tasks -->

Als u een data warehouse herstellen, herstellen gebruiken:

- Azure-portal, raadpleegt u [een data-warehouse met behulp van de Azure portal herstellen](sql-data-warehouse-restore-database-portal.md)
- PowerShell-cmdlets, raadpleegt u [een data-warehouse met PowerShell-cmdlets herstellen](sql-data-warehouse-restore-database-powershell.md)
- API's PLAATST, raadpleegt u [een data-warehouse met de REST API's herstellen](sql-data-warehouse-restore-database-rest-api.md)

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overzicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
