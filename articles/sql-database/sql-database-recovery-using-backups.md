<properties
   pageTitle="Bedrijfscontinuïteit cloud - een verwijderde database - SQL-Database terugzetten | Microsoft Azure"
   description="Meer informatie over Point-in-Time herstellen, waarmee u terugkeren een Azure SQL-Database naar een vorige punt in tijd (maximaal 35 dagen)."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/01/2016"
   ms.author="sstein"/>

# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Een automatische back-ups met Azure SQL-database herstellen

SQL-Database biedt drie opties voor het herstellen van databases met [dat SQL-Database automatische back-ups](sql-database-automated-backups.md). In hun [bewaarperiode](sql-database-service-tiers.md) tot, kunt u een database terugzetten vanaf de back-ups van de service is gestart:

- Een nieuwe database op dezelfde logische server herstellen naar een opgegeven punt in de tijd binnen de bewaarperiode. 
- Een database op dezelfde logische server naar de verwijdering tijd voor een verwijderde database hersteld.
- Een nieuwe database op een willekeurige logische server in elke regio herstellen naar de meest recente dagelijkse back-ups in geografische gerepliceerd blobopslag (AB-GRS).

U kunt ook een [SQL-Database automatische back-ups](sql-database-automated-backups.md) gebruiken om te maken van een [database kopiëren](sql-database-copy.md) op elke logische server in elke regio transactioneel consistent is met de huidige SQL-Database. U kunt database kopiëren en [exporteren naar een BACPAC](sql-database-export.md) gebruiken om een transactioneel consistente kopie van een database langdurige opslag buiten uw bewaarperiode of een kopie van uw database overbrengen naar een on-premises of Azure VM exemplaar van SQL Server.

## <a name="recovery-time"></a>Hersteltijd

De hersteltijd herstellen van een database met behulp van automatische back-ups wordt beïnvloed door een aantal factoren: 
 - De grootte van de database
 - Het prestatieniveau van de database
 - Het aantal betrokken transactielogboeken
 - De hoeveelheid activiteit dat moet worden cookies als u wilt herstellen naar de komma herstellen
 - De netwerkbandbreedte als het herstellen naar een ander gebied is 
 - Het aantal gelijktijdige herstellen-aanvragen worden verwerkt in de doelregio. 
 
 Voor een database zeer grote en/of actieve kan de terugzetten enkele uren duren. Als er langdurige storing in een gebied, is het mogelijk dat er worden grote hoeveelheden geografische herstellen aanvragen wordt verwerkt door andere regio's. Als er een groot aantal aanvragen dat hierdoor het tijdstip herstel voor databases in dat gebied kan toenemen. De meeste database herstelt voltooid binnen 12 uur.

 Er is geen ingebouwde functionaliteit om te bulksgewijs te herstellen. De [Azure SQL-Database: volledig Server herstel](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) script is een voorbeeld van één manier uitvoeren van deze taak.

> [AZURE.IMPORTANT] Als u wilt herstellen met automatische back-ups, moet u een lid van de rol Inzender van SQL Server in het abonnement dat zijn of de eigenaar van het abonnement. U kunt herstellen met behulp van de Azure portal, PowerShell of de REST API. U kunt geen Transact-SQL. 

## <a name="point-in-time-restore"></a>Point-In-Time herstellen

Point-In-Time herstellen, kunt u een bestaande database herstellen als een nieuwe database op een eerder in tijd op dezelfde logische server met [dat SQL-Database automatische back-ups](sql-database-automated-backups.md). U kunt de bestaande database niet overschrijven. U kunt op een eerder herstellen met behulp van de [Azure-portal](sql-database-point-in-time-restore-portal.md), [PowerShell](sql-database-point-in-time-restore-powershell.md) of de [REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Point-In-Time herstellen: Azure portal](sql-database-point-in-time-restore-portal.md)
- [Point-In-Time herstellen: PowerShell](sql-database-point-in-time-restore-powershell.md)

De database op een prestatieniveau of elastische kan worden hersteld toepassingen. Moet u zorgen dat er een voldoende DTU quotum op de logische server of elastische toepassingen. Houd er rekening mee dat de terugzetten Hiermee maakt u een nieuwe database en dat de service laag en prestaties het niveau van de herstelde database is mogelijk anders dan de huidige status van de actieve database. Als voltooid, is de herstelde database een normale volledig toegankelijk onlinedatabase tegen normale tarieven op basis van de laag en prestaties van een dienstverlening in rekening gebracht. U kan niet in rekening gebracht kosten totdat de database herstellen voltooid is.

U in het algemeen terugzetten een database naar een punt earler voor herstellen. Wanneer als u dit doet, kunt u de herstelde database behandelen als een vervanging van de oorspronkelijke database of gebruiken voor het ophalen van gegevens uit en werkt u vervolgens de oorspronkelijke database. 

- ***Database vervangende:*** Als de herstelde database is bedoeld als een vervanging van de oorspronkelijke database, moet u controleren of het prestatieniveau van de en/of servicelaag van toepassing zijn en schaal van de database indien nodig. U kunt de naam van de oorspronkelijke database wijzigen en geef de herstelde database de oorspronkelijke naam met de opdracht DATABASE wijzigen in T-SQL. 
- ***Herstel van gegevens:*** Als u van plan bent de gegevens worden opgehaald uit de herstelde database herstellen uit een fout gebruiker of toepassing, moet u afzonderlijk om te schrijven en welke gegevens herstel-scripts die u wilt extraheren van gegevens uit de herstelde database met de oorspronkelijke database uitvoeren. Hoewel het terugzetten een lange tijd in beslag nemen mogelijk, zijn de database terugzetten zichtbaar zijn in de databaselijst overal in. Als u de database tijdens de terugzetten verwijdert, wordt deze de bewerking te annuleren en u niet moet betalen voor de database die niet de herstellen is voltooid. 

Zie voor gedetailleerde informatie over het gebruik van Point-in-Time herstellen herstellen uit de gebruiker en toepassingsfouten [Point-in-Time herstellen](sql-database-recovery-using-backups.md#point-in-time-restore)

## <a name="deleted-database-restore"></a>Verwijderde database terugzetten

Verwijderde database terugzetten kunt u een verwijderde database terugzetten naar de verwijdering tijd voor een verwijderde database op dezelfde logische server met [dat SQL-Database automatische back-ups](sql-database-automated-backups.md). 

> [AZURE.IMPORTANT] Als u een exemplaar van de server Azure SQL-Database verwijdert, worden alle bijbehorende databases worden ook verwijderd en kan niet worden hersteld. Er is geen ondersteuning voor het herstellen van een verwijderde server op dit moment.

U kunt hetzelfde of een nieuwe databasenaam voor de herstelde database gebruiken. U kunt de [portal van Azure](sql-database-restore-deleted-database-portal.md) [PowerShell](sql-database-restore-deleted-database-powershell.md) gebruiken of de [REST (createMode = terugzetten)](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [AZURE.SELECTOR]
- [Verwijderde database terugzetten: Azure-portal](sql-database-restore-deleted-database-portal.md)
- [Verwijderde database terugzetten: PowerShell](sql-database-restore-deleted-database-powershell.md)

## <a name="geo-restore"></a>Geografische herstellen

Geografische herstellen, kunt u een SQL-database op een server in een Azure regio van de meest recente door de geografische gerepliceerd [geautomatiseerde dagelijkse back-up](sql-database-automated-backups.md)herstellen. Geografische herstellen een geografische-redundante back-up als gegevensbron gebruikt en kan worden gebruikt om een database herstellen, zelfs als de database of datacenter niet toegankelijk vanwege een storing is. U kunt de [portal van Azure](sql-database-geo-restore-portal.md) [PowerShell](sql-database-geo-restore-powershell.md), of de [REST (createMode = herstel)](https://msdn.microsoft.com/library/azure/mt163685.aspx) 

> [AZURE.SELECTOR]
- [Geografische-herstellen: Azure portal](sql-database-geo-restore-portal.md)
- [Geografische herstellen: PowerShell](sql-database-geo-restore-powershell.md)

Geografische herstellen is de standaardoptie voor herstel wanneer de database is niet beschikbaar vanwege een incident in de regio waar de database wordt gehost. Als een incident grote schaal in de resultaten van een regio in niet beschikbaar van de databasetoepassing, kunt u geografische terugzetten naar een database terugzetten uit de meest recente back-up op een server in een andere regio. Alle back-ups zijn geografische gerepliceerd en kan hebben een vertraging op tussen wanneer de back-up genomen en geografische gerepliceerd naar de Azure is blob in een andere regio. Deze vertraging mag maximaal een uur, zodat een storing kunnen er omhoog tot 1 uur gegevensverlies, dat wil zeggen vrijgegeven Productieorder maximaal 1 uur. Hieronder ziet u het herstellen van de database uit het laatste dagelijkse back-up.

![geografische herstellen](./media/sql-database-geo-restore/geo-restore-2.png)

Zie [herstellen van een storing](sql-database-disaster-recovery.md) voor gedetailleerde informatie over het gebruik van geografische herstellen herstellen uit een storing,

> [AZURE.IMPORTANT] Geografische herstellen is beschikbaar met alle Servicelagen, is maar de belangrijkste beschikbaar in SQL-Database met de langste geschatte herstel tijd (invoegen) en vrijgegeven Productieorder noodgevallen herstel oplossingen. Voor eenvoudige databases met de maximale grootte van 2 GB geografische-herstellen biedt een redelijk DR-oplossing met een invoegen van 12 uur. Voor grotere standaard of Premium-databases, als aanzienlijk korter herstel tijden gewenst zijn of als u wilt de kans van gegevensverlies moet u overwegen actieve geografische-herhaling. Actieve geografische-herhaling biedt een dalen vrijgegeven Productieorder en invoegen als u alleen hoeft een failover-aan een continu gerepliceerde secundair. Zie [Active geografische-replicatie](sql-database-geo-replication-overview.md)voor meer informatie.

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Via een programma uitvoering herstel met automatische back-ups

Zie hierboven, in addiition bij de portal Azure kan databaseherstel worden uitgevoerd wordt Azure PowerShell en de REST API gebruiken. De onderstaande tabellen beschrijven de reeks opdrachten die beschikbaar zijn.

### <a name="powershell"></a>PowerShell

|Cmdlet|Beschrijving|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Krijgt een of meer databases.|
|[Get-AzureRMSqlDeletedDatabaseBackup](https://msdn.microsoft.com/en-us/library/azure/mt693387.aspx)|Krijgt een verwijderde database die u kunt terugzetten.|
|[Get-AzureRmSqlDatabaseGeoBackup](https://msdn.microsoft.com/library/azure/mt693388.aspx)|Krijgt een geografische-redundante back-up van een database.|
|[Herstellen-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390.aspx)|Hiermee herstelt u een SQL-database.|
||||

### <a name="rest-api"></a>REST API

|API|Beschrijving|
|---|-----------|
|[REST (createMode = herstel)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Hiermee herstelt u een database|
|[Get maken of de databasestatus van de bijwerken](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Geeft als resultaat de status tijdens een bewerking herstellen|
||||



## <a name="summary"></a>Overzicht

Automatische back-ups Bescherm uw databases in de gebruiker en toepassingsfouten, ongeluk database verwijderen en langdurige bijvoorbeeld. Deze oplossing nul-kosten nul-beheerder is beschikbaar met alle SQL-databases. 

## <a name="next-steps"></a>Volgende stappen

- Zie [overzicht van Business-bedrijfscontinuïteit](sql-database-business-continuity.md) voor een overzicht van de bedrijfscontinuïteit bedrijven en scenario's,
- Meer informatie over Azure SQL-Database wordt automatisch back-ups, raadpleegt u [SQL-Database automatische back-ups](sql-database-automated-backups.md)
- Zie voor meer informatie over sneller herstelopties, [Actief-geografische-replicatie](sql-database-geo-replication-overview.md)  
- Voor meer informatie over het gebruik van automatische back-ups voor het archiveren, raadpleegt u [database kopiëren](sql-database-copy.md)
