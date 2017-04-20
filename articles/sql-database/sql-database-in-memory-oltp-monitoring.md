<properties
    pageTitle="Bewaak XTP geheugenopslag | Microsoft Azure"
    description="De geschatte en monitor XTP geheugenopslag gebruikt, capaciteit; capaciteit fout 41823 oplossen"
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="monitor-in-memory-oltp-storage"></a>Monitor met een In het geheugen OLTP opslag

Wanneer u [In het geheugen OLTP](sql-database-in-memory.md), wordt gegevens in tabellen geheugen geoptimaliseerde en tabelvariabelen bevindt zich in de opslagruimte In het geheugen OLTP. Elke laag Premium-service heeft een maximum aantal In het geheugen OLTP opslaggrootte, die wordt beschreven in het [artikel SQL Database-Servicelagen](sql-database-service-tiers.md#service-tiers-for-single-databases). Als deze limiet is overschreden, invoegen en bijwerken bewerkingen mogelijk starten verbroken (met fout 41823). Op dat moment wordt nodig naar een gegevens om geheugen vrij te verwijderen, of upgraden van de laag prestaties van uw database.

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a>Bepalen of gegevens binnen de initiaal in het geheugen opslag past

Bepalen van de initiaal opslag: Raadpleeg het [artikel SQL Database-Servicelagen](sql-database-service-tiers.md#service-tiers-for-single-databases) voor de initialen van de opslag van de andere niveaus van de Premium-service.

Schatting geheugen nodig voor een tabel geheugen geoptimaliseerde werkt net voor SQL Server als dit wordt in Azure SQL-Database. Een paar minuten moet worden gereviseerd van dat onderwerp op [MSDN](https://msdn.microsoft.com/library/dn282389.aspx)duren.

Houd er rekening mee dat de tabel en variabele tabelrijen, evenals indexen, naar de grootte van de gegevens max gebruiker tellen. ALTER TABLE moet bovendien voldoende ruimte om u te maken van een nieuwe versie van de hele tabel en de bijbehorende indexen.

## <a name="monitoring-and-alerting"></a>Cmdlets voor controle en waarschuwingen

U kunt volgen in het geheugen opslag gebruiken als een percentage van de [opslagruimte voor uw prestaties laag hoofdletters](sql-database-service-tiers.md#service-tiers-for-single-databases) in de Azure [portal](https://portal.azure.com/): 

- Klik op het blad Database, zoek het gebruik van Resource en klik op bewerken.
- Selecteer de meetwaarde `In-Memory OLTP Storage percentage`.
- Als u wilt een waarschuwing toevoegen, klikt u op het vak Resourcegebruik het metrische blad openen en klik vervolgens op op melding toevoegen.

Of gebruikt u de volgende query om weer te geven van het gebruik van de opslagruimte in het geheugen:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Onvoldoende geheugen situaties - fout 41823 corrigeren

Actief onvoldoende geheugen resultaten in invoegen, bijwerken, en maak bewerkingen verbroken 41823 een foutbericht wordt weergegeven.

Foutbericht 41823 geeft aan dat de tabellen geheugen geoptimaliseerde en de tabelvariabelen de maximale grootte hebben overschreden.

Deze fout, ofwel oplossen:


- Gegevens verwijderen uit de tabellen geheugen geoptimaliseerde, potentieel offloading de gegevens naar traditionele, op de schijf tabellen. of,
- De servicelaag een upgrade uitvoeren naar een met voldoende opslagruimte in het geheugen voor de gegevens die u wilt behouden in het geheugen geoptimaliseerde tabellen.

## <a name="next-steps"></a>Volgende stappen
Aanvullende informatie over over het [gebruik van Azure SQL-Database Monitoring dynamische management weergaven](sql-database-monitoring-with-dmvs.md)
