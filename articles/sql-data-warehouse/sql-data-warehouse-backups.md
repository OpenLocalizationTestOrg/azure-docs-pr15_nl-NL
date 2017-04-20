<properties
   pageTitle="Back-ups van SQL Data Warehouse | Microsoft Azure"
   description="Lees meer over SQL Data Warehouse ingebouwde back-ups waarmee u kunt een Azure SQL Data Warehouse terugzetten op een punt herstellen of een ander geografisch gebied."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lakshmi1812"
   manager="barbkess"
   editor="monicar"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="lakshmir;barbkess"/>

# <a name="sql-data-warehouse-backups"></a>Back-ups van SQL Data Warehouse

SQL Data Warehouse biedt geografische zowel lokale back-ups als onderdeel van de back-mogelijkheden om gegevens BTW-records. Hierbij Azure opslag Blob momentopnamen en geografische-redundante opslag. Gebruik warehouse back-ups van gegevens naar uw BTW-records voor de gegevens in een punt terugzetten in de primaire regio herstellen of terugzetten naar een ander geografisch gebied. In dit artikel wordt uitgelegd dat de details van back-ups in SQL Data Warehouse.

## <a name="what-is-a-data-warehouse-backup"></a>Wat is een back-up warehouse gegevens?

Een back-up BTW-records van gegevens is de gegevens die u gebruiken kunt om te zetten van een data-warehouse naar een bepaalde tijd.  Aangezien SQL Data Warehouse een gedistribueerd systeem is, wordt een back-up van gegevens warehouse bestaat uit veel bestanden die zijn opgeslagen in Azure BLOB's. 

Back-ups zijn een essentieel onderdeel van een strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel, omdat ze uw gegevens tegen ongeluk beschadigde bestanden of verwijdering beschermen. Zie [Business bedrijfscontinuïteit overzicht](../sql-database/sql-database-business-continuity.md)voor meer informatie.

## <a name="data-redundancy"></a>Overbodige

SQL Data Warehouse beveiligt uw gegevens door uw gegevens opslaat in een lokaal overtollige (LRS) Azure Premium opslag. Deze functie Azure Storage slaat meerdere synchroon kopieën van de gegevens in het midden van de lokale gegevens te garanderen transparante gegevensbescherming als er op gelokaliseerde fouten. Overbodige zorgt ervoor dat uw gegevens kunt ook na infrastructuur problemen zoals schijfproblemen. Overbodige zorgt ervoor dat bedrijfscontinuïteit met een fouttolerantie en de infrastructuur van maximaal beschikbaar.

Voor meer informatie over:

- Azure Premium-opslag, raadpleegt u [Inleiding tot Azure Premium opslag](../storage/storage-premium-storage.md).
- Lokaal overtollige opslag, Zie [Azure Storage replicatie](../storage/storage-redundancy.md#locally-redundant-storage).


## <a name="azure-storage-blob-snapshots"></a>Azure Blob Storage momentopnamen

Een voordeel van het gebruik van Azure Premium Storage SQL Data Warehouse worden met Azure opslag Blob momentopnamen back-up maken datawarehouse lokaal. U kunt een data-warehouse terugzetten naar een momentopname herstellen. Momentopnamen start een minimum aan elke acht uur en beschikbaar zijn voor zeven dagen.  

Voor meer informatie over:

- Azure blob momentopnamen, raadpleegt u [een momentopname blob maken](../storage/storage-blob-snapshots.md).


## <a name="geo-redundant-backups"></a>Geografische-redundante back-ups

Elke 24 uur slaat SQL Data Warehouse het volledige datawarehouse in Standard opslag. Het volledige datawarehouse wordt gemaakt zodat deze overeenkomen met de tijd van de laatste momentopname. De standaard opslag hoort bij een account geografische-redundante opslagruimte met leestoegang (AB-GRS). De functie Azure opslag AB-GRS wordt overgenomen door de back-upbestanden een [gepaarde Datacenter](../best-practices-availability-paired-regions.md). Deze geografische-replicatie zorgt ervoor dat u kunt datawarehouse herstellen als u geen toegang de momentopnamen in uw primaire regio tot. 

Deze functie is standaard. Als u niet dat geografische-redundante back-ups gebruiken wilt, kunt u opt-out. 

>[AZURE.NOTE] In Azure opslag, wordt de term *herhaling* verwijst naar het kopiëren van bestanden van de ene locatie naar een andere. De SQL- *databasereplicatie* verwijst naar het behouden tot meerdere secundaire databases die zijn gesynchroniseerd met een primaire-database. 

Voor meer informatie over:
- Geografische-redundante opslag, Zie [Azure Storage replicatie](../storage/storage-redundancy.md).
- AB-GRS opslag, Zie [leestoegang geografische-redundante opslag](../storage/storage-redundancy.md#read-access-geo-redundant-storage).

## <a name="data-warehouse-backup-schedule-and-retention-period"></a>Data warehouse back-ups plannen en bewaarperiode

SQL Data Warehouse momentopnamen maakt op uw online datawarehouses om vier tot acht uur en elke momentopname blijft gedurende zeven dagen. U kunt de onlinedatabase terugzetten in een van de herstellen wordt verwezen in de afgelopen zeven dagen. 

Om te zien krijgen wanneer de laatste momentopname starten, moet u deze query uitvoeren op uw online SQL Data Warehouse. 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Als u bewaren van een momentopname langer dan zeven dagen wilt, kunt u een punt herstellen terugzetten naar een nieuwe data-warehouse. Nadat de herstellen is voltooid, begint SQL Data Warehouse momentopnamen maken voor het nieuwe datawarehouse. Als u geen wijzigingen in de nieuwe gegevens BTW-records aanbrengt, de momentopnamen blijven leeg en daarom de kosten momentopname minimale. U kunt ook de database om te voorkomen dat SQL Data Warehouse maken van momentopnamen onderbreken.


### <a name="what-happens-to-my-backup-retention-while-my-data-warehouse-is-paused"></a>Wat gebeurt er met mijn back-up bewaarbeleid terwijl Mijn datawarehouse is onderbroken?

SQL Data Warehouse maakt geen momentopnamen en verloopt niet momentopnamen terwijl een data-warehouse is onderbroken. De leeftijd momentopname niet wordt gewijzigd terwijl het datawarehouse is onderbroken. Momentopname bewaarbeleid is gebaseerd op het aantal dagen die datawarehouse online, niet kalenderdagen is.

Als een momentopname 1 oktober om 4 uur begint en datawarehouse is onderbroken oktober 3 om 16: 00, is de momentopname bijvoorbeeld twee dagen geleden. Wanneer het datawarehouse weer online komt is de momentopname twee dagen geleden. Als het datawarehouse afkomstig online oktober 5 om 16: 00 is, wordt de momentopname twee dagen oud en blijft vijf meer dagen.

Wanneer het datawarehouse weer online komt, wordt SQL Data Warehouse cv's nieuwe momentopnamen en momentopnamen verloopt wanneer ze meer dan zeven dagen gegevens.

### <a name="how-long-is-the-retention-period-for-a-dropped-data-warehouse"></a>Hoe lang is de bewaarperiode voor een decoratieve data-warehouse?
Wanneer een data-warehouse wordt verbroken, zijn datawarehouse en de momentopnamen opgeslagen gedurende zeven dagen en vervolgens verwijderd. U kunt datawarehouse terugzetten in een van de opgeslagen herstellen wordt verwezen.

> [AZURE.IMPORTANT] Als u een logische SQL server-instantie verwijdert, worden alle databases die deel uitmaakt van het exemplaar worden ook verwijderd en kunnen niet worden hersteld. U kunt een verwijderde server niet herstellen.

## <a name="data-warehouse-backup-costs"></a>Data warehouse back-kosten

De totale kosten voor uw primaire datawarehouse en zeven dagen van Azure Blob momentopnamen afgerond op het dichtstbijzijnde TB. Als uw gegevenswarehouse 1,5 TB en de momentopnamen 100 GB gebruiken, kunt u zijn bijvoorbeeld voor gefactureerd voor 2 TB aan gegevens van Azure Premium Storage tarieven. 

>[AZURE.NOTE] Elke momentopname in eerste instantie leeg is, en wanneer u wijzigingen in de primaire datawarehouse aanbrengt in omvang groeit. Alle momentopnamen zijn als de gegevens BTW-records worden gewijzigd in omvang toeneemt. De kosten voor de opslag voor momentopnamen groeien daarom op basis van het aantal wijzigen.

Als u geografische-redundante opslag gebruikt, ontvangt u een boete gescheiden op te slaan. De geografische-redundante opslag gefactureerd bij het standaardtarief van leestoegang geografisch redundante opslag (AB-GRS).

Zie [SQL Data Warehouse prijzen](https://azure.microsoft.com/pricing/details/sql-data-warehouse/)voor meer informatie over SQL Data Warehouse prijzen.

## <a name="using-database-backups"></a>Back-ups gebruiken

Het primaire gebruik SQL data warehouse back-ups is om te zetten datawarehouse naar een van de punten herstellen binnen de bewaarperiode.  

- Als u wilt herstellen een SQL-data-warehouse, raadpleegt u [een SQL-data-warehouse herstellen](sql-data-warehouse-restore-database-overview.md).


## <a name="related-topics"></a>Verwante onderwerpen

### <a name="scenarios"></a>Scenario 's

- Zie [overzicht van de bedrijven bedrijfscontinuïteit](../sql-database/sql-database-business-continuity.md) voor een overzicht van de bedrijfscontinuïteit bedrijven


<!-- ### Tasks -->

- Als u een data-warehouse herstellen, raadpleegt u [een SQL-data-warehouse herstellen](sql-data-warehouse-restore-database-overview.md)

<!-- ### Tutorials -->

