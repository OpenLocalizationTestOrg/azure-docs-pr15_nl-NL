<properties
   pageTitle="Bedrijfscontinuïteit cloud - herstel - SQL-Database database | Microsoft Azure"
   description="Leer hoe Azure SQL-Database ondersteunt cloud bedrijfscontinuïteit en herstellen van databases en kunt u belangrijke cloud-toepassingen die worden uitgevoerd."
   keywords="bedrijfscontinuïteit, cloud bedrijfscontinuïteit, database-herstel, database herstellen"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Overzicht van bedrijfscontinuïteit met Azure SQL-Database

Dit overzicht beschrijft de mogelijkheden die Azure SQL-Database voor bedrijfscontinuïteit en herstel. Deze biedt opties, aanbevelingen en zelfstudies voor het herstellen van storend gebeurtenissen die kunnen leiden tot gegevensverlies of ertoe leiden dat uw database en de toepassing niet meer beschikbaar. De discussie bevat wat u moet doen wanneer een gebruiker of fout van toepassing is van invloed op de integriteit van gegevens, een Azure gebied heeft een storing of uw toepassing vereist onderhoud. 

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>SQL-Database-functies die u gebruiken kunt om te leveren bedrijfscontinuïteit

SQL-Database voorziet in diverse bedrijven bedrijfscontinuïteit-functies, inclusief automatische back-ups en optioneel databasereplicatie. Elk heeft andere kenmerken voor geschatte hersteltijd (invoegen) en potentieel gegevensverlies voor recente transacties. Als u meer informatie over deze opties, kunt u kiezen tussen uw ze - en, in de meeste gevallen ze samen gebruiken voor verschillende scenario's. Als u uw plan voor bedrijfscontinuïteit ontwikkelt, moet u meer informatie over de maximale tijd die acceptabel voordat de toepassing volledig hersteld na de storend gebeurtenis: dit uw herstel tijd doelstelling (RTO is). Moet u ook voor meer informatie over de maximale hoeveelheid recente Gegevensupdates (tijdsinterval) de toepassing mogelijk zonder verliezen bij het herstellen na de storend gebeurtenis - herstel punt doel (vrijgegeven Productieorder). 

De volgende tabel worden de invoegen en vrijgegeven Productieorder voor de drie meest voorkomende scenario's vergeleken.

| De mogelijkheid |  Eenvoudige laag | Standaard laag  | Premium laag |
|---|---|---|---|
| Wijs in het tijd herstellen uit back-up | Een punt binnen zeven dagen herstellen   | Een punt in 35 dagen herstellen  | Een punt in 35 dagen herstellen |
Geografische-terugzetten vanuit geografische gerepliceerd back-ups | < 12 uur, vrijgegeven Productieorder < 1U invoegen   | < 12 uur, vrijgegeven Productieorder < 1U invoegen   | < 12 uur, vrijgegeven Productieorder < 1U invoegen |
|Actieve geografische-herhaling | < 30s, vrijgegeven Productieorder < 5s invoegen   | < 30s, vrijgegeven Productieorder < 5s invoegen | < 30s, vrijgegeven Productieorder < 5s invoegen |


### <a name="use-database-backups-to-recover-a-database"></a>Back-ups gebruiken voor het herstellen van een database

SQL-Database automatisch uitgevoerd een combinatie van volledige back-ups wekelijks differentiële database van back-ups per uur en transactie log back-ups om de vijf minuten aan uw bedrijf beschermen tegen verlies van gegevens. Deze back-ups zijn opgeslagen in lokaal redundante opslag voor 35 dagen voor databases in de service Standard en Premium lagen en zeven dagen voor databases in de servicelaag eenvoudige - [Servicelagen](sql-database-service-tiers.md) voor meer informatie op Servicelagen te zien. Als de bewaarperiode voor de servicelaag van uw niet aan uw zakelijke behoeften voldoet, kunt u de bewaarperiode vergroten door het [wijzigen van de servicelaag](sql-database-scale-up.md). De volledige en differentiële database back-ups ook worden gerepliceerd naar een [gepaarde Datacenter](../best-practices-availability-paired-regions.md) voor de bescherming tegen een data center storing - Zie [Automatische back-ups](sql-database-automated-backups.md) voor meer informatie.

Een database herstellen uit verschillende storend gebeurtenissen, binnen uw datacenter en een ander datacenter voor kunt u deze automatische back-ups. Automatische back-ups gebruikt, de geschatte tijd van herstel is afhankelijk van de volgende factoren zoals het totale aantal databases herstel in dezelfde regio op hetzelfde moment, de grootte van de database, de grootte van het logboek transactie en netwerkbandbreedte. In de meeste gevallen wordt de hersteltijd minder dan 12 uur. Bij het herstellen naar een ander gegevensgebied, is de potentieel gegevensverlies beperkt tot 1 uur door de geografische-redundante opslag van per uur differentiële back-ups. 

> [AZURE.IMPORTANT] Als u wilt herstellen met automatische back-ups, moet u een lid zijn van de rol Inzender van SQL Server of de eigenaar van het abonnement - Zie [RBAC: ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md). U kunt herstellen met de portal van Azure, PowerShell of voor de REST API. U kunt geen Transact-SQL.

Automatische back-ups gebruiken als uw bedrijf bedrijfscontinuïteit en herstelbestanden om, als uw toepassing:

- Geen opdracht als kritiek beschouwd.
- Heeft geen een binding SLA de downtime van 24 uur of meer dus niet in financiële aansprakelijkheid resulteert.
- Een laag tarief van gegevens wijzigen (lage transacties per uur) heeft en verliezen tot een uur na wijziging is een aanvaardbaar gegevensverlies. 
- Zijn de kosten gevoelige. 

Als u sneller herstel nodig hebt, gebruikt u [Actieve geografische-herhaling](sql-database-geo-replication-overview.md) (volgende besproken). Als u kunnen gegevens terughalen uit een periode die ouder zijn dan 35 dagen wilt, houd rekening met het archiveren van uw database regelmatig naar een Bacpac-bestand opgeslagen (een gecomprimeerde bestand met uw databaseschema en de bijbehorende gegevens) in Azure-blobopslag of in een andere locatie van uw keuze. Zie [een databasekopie maken](sql-database-copy.md) en [de databasekopie exporteren](sql-database-export.md)voor meer informatie over het maken van een archief transactioneel consistente database. 

### <a name="use-active-geo-replication-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Actieve geografische-replicatie gebruikt om te verkorten herstel en verlies van gegevens die zijn gekoppeld aan een herstel beperken

Naast back-ups voor databaseherstel in het geval van een business-onderbreking, kunt u [Actieve geografische-herhaling](sql-database-geo-replication-overview.md) gebruiken voor het configureren van een database om maximaal vier leesbare secundaire databases in de regio's van uw keuze. Deze secundaire databases worden gesynchroniseerd met de hoofddatabase van met een om asynchroon herhaling. Deze functie wordt gebruikt om te beveiligen tegen bedrijven verstoringen in het geval van een storing gegevens-beheercentrum of tijdens de upgrade van een toepassing. Actieve geografische-herhaling kan ook worden gebruikt op te geven betere prestaties van query's voor alleen-lezenquery's naar geografisch verspreid gebruikers.

Als de primaire database onverwacht offline gaat of moet u deze offline voor onderhoudsactiviteiten maakt, kunt u snel een secundair om te worden de primaire (ook wel een failover genoemd) en configureren van toepassingen verbinding maken met de zojuist gepromoveerde primaire promoveren. Met een geplande failover is er geen gegevens verloren gaan. Met een niet-geplande failover, is het mogelijk dat er een enkele kleine hoeveelheid verlies van gegevens voor zeer recent transacties vanwege de aard van asynchrone herhaling. Na een failover kunt u later failback - op basis van een abonnement of wanneer de Datacenter weer online gaat. In alle gevallen gebruikers krijgen een kleine hoeveelheid downtime en moeten opnieuw verbinding maakt. 

> [AZURE.IMPORTANT] Als u wilt gebruiken actieve geografische-replicatie, moet u de eigenaar van het abonnement of beheerdersmachtigingen hebben in SQL Server. U kunt configureren en overname bij storing met de portal van Azure, PowerShell of voor de REST API machtigingen op het abonnement of Transact-SQL gebruiken met machtigingen vanuit SQL Server.

Gebruik actieve geografische-herhaling als uw toepassing voldoet aan een van deze criteria voldoet:

- Opdracht is kritieke.
- Een service level agreement (SLA) die niet zijn toegestaan met 24 uur of meer van de downtime heeft.
- Downtime treden financiële aansprakelijkheid.
- Heeft een hoge frequentie van gegevens wijzigen hoog is en een uur gegevens kwijt niet acceptabel is.
- De extra kosten van actieve geografische-replicatie is lager dan de mogelijke financiële aansprakelijkheid en de bijbehorende verlies van bedrijven.

## <a name="recover-a-database-after-a-user-or-application-error"></a>Een database herstellen nadat een gebruiker of toepassing-fout

* Niemand is perfect! Een gebruiker mogelijk enkele gegevens per ongeluk verwijdert, per ongeluk een belangrijke tabel verwijderen of zelfs een volledige database weghalen. Of een toepassing mogelijk per ongeluk goede gegevens overschrijven met ongeldige gegevens vanwege een gebrek toepassing. 

In dit scenario zijn dit uw herstelopties.

### <a name="perform-a-point-in-time-restore"></a>Een punt-in-tijd herstellen uitvoeren

Voorwaarde dat moment binnen de bewaarperiode database is, kunt u de automatische back-ups herstellen van een kopie van uw database aan een bekende goede punt in de tijd. Nadat de database is hersteld, kunt u de oorspronkelijke database vervangen door de herstelde database of de benodigde gegevens van de herstelde gegevens kopiëren in de oorspronkelijke database. Als de database actief geografische-replicatie gebruikt, wordt u aangeraden de vereiste gegevens kopiëren van de herstelde kopie in de oorspronkelijke database. Als u de oorspronkelijke database door de herstelde database vervangt, moet u deze wordt opnieuw configureren en synchronisatie van Active geografische-replicatie (dat kan lang duren voor een grote database). 

Voor meer informatie Zie voor gedetailleerde stappen voor het herstellen van een database op een punt in tijd met behulp van de Azure portal of via PowerShell, en [punt-in-tijd herstellen](sql-database-recovery-using-backups.md#point-in-time-restore). U herstellen niet met Transact-SQL.

### <a name="restore-a-deleted-database"></a>Een verwijderde database terugzetten

Als de database wordt verwijderd, maar de logische server niet is verwijderd, kunt u de verwijderde database terugzet op het punt waarop deze is verwijderd. Een back-up van de database wordt teruggezet naar dezelfde logische SQL server waaruit deze is verwijderd. U kunt herstellen met de oorspronkelijke naam of een andere naam of de herstelde database bieden.

Voor meer informatie en voor gedetailleerde stappen voor het herstellen van een verwijderde database raadpleegt met behulp van de Azure portal of via PowerShell, u [een verwijderde database terugzetten](sql-database-recovery-using-backups.md#deleted-database-restore). U kunt geen herstellen met Transact-SQL.

> [AZURE.IMPORTANT] Als de logische server wordt verwijderd, kunt u een verwijderde database niet herstellen. 

### <a name="import-from-a-database-archive"></a>Importeren uit een database archiveren

Als het verlies van gegevens is opgetreden buiten de huidige bewaarperiode voor automatische back-ups en u de database is archiveren, kunt u [een gearchiveerd Bacpac-bestand importeren](sql-database-import.md) naar een nieuwe database. Nu kunt u de oorspronkelijke database vervangen door de geïmporteerde database of de benodigde gegevens van de geïmporteerde gegevens kopiëren in de oorspronkelijke database. 

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Een database herstellen naar een ander gebied van een storing Azure regionale gegevens-beheercentrum

<!-- Explain this scenario -->

Hoewel niet vaak voorkomen, kan een Azure Datacenter een storing hebben. Wanneer een storing plaatsvindt, wordt deze door een verstoring van de bedrijven die mogelijk alleen een paar minuten achternamen of uur kan duren. 

- Eén optie is moet wachten om door uw database voor het weer online komt wanneer de gegevens center storing verstreken is. Dit geldt voor toepassingen die de database offline hebt vast. Bijvoorbeeld een ontwikkeling van het project of de gratis proefversie u niet nodig hebt om aan te werken voortdurend. Wanneer een datacenter een storing bevat, weet u niet hoe lang duurt het storing, zodat deze optie alleen werkt als u niet de database nodig voor de tijd hebt.
- Als u actieve geografische-herhaling of de herstellen met geografische-redundante back-ups (geografische-herstellen) gebruikt, is er een andere optie beide foutherstel naar een ander gegevensgebied. Failover gaat maar een paar seconden, terwijl het herstellen van back-ups uur duurt.

Wanneer u de actie ondernemen, hoe lang duurt het herstellen van en hoeveel u in het geval van een storing gegevens-beheercentrum oplopen verlies van gegevens is afhankelijk van hoe u besluit het gebruik van de bedrijfscontinuïteit voorzieningen hierboven worden besproken in uw toepassing. U kunt wel een combinatie van back-ups en actieve geografische-replicatie, afhankelijk van uw toepassingsvereisten voor de gebruiken. Zie [ontwerp een toepassing voor de cloud-herstel](sql-database-designing-cloud-solutions-for-disaster-recovery.md) en [elastische groep noodgevallen herstelstrategieën](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)voor een discussie van toepassing ontwerpoverwegingen voor zelfstandige databases en voor gebruik van deze bedrijven bedrijfscontinuïteit functies van elastische toepassingen.

De secties hierna wordt een overzicht gegeven van de stappen om te herstellen met behulp van back-ups of actieve geografische-herhaling. Zie [een SQL-Database van een storing herstellen](sql-database-disaster-recovery.md)voor meer informatie inclusief planning vereisten, bericht herstel stappen en informatie over hoe u deze de vorm van een storing als u wilt uitvoeren van een noodgevallen herstel meer details.

### <a name="prepare-for-an-outage"></a>Voorbereiden voor een storing

Ongeacht de bedrijfscontinuïteit bedrijven functie die u gebruikt, moet u het volgende doen:

- Identificeren en de doelserver, inclusief serverniveau firewallregels, aanmeldingen en machtigingen voor het siteverzamelingsniveau van hoofddatabase voorbereiden.
- Bepalen hoe clients en de nieuwe server-clienttoepassingen doorsturen
- Document andere afhankelijkheden, zoals de controle-instellingen en waarschuwingen 
 
Als u niet van plan bent en goed om uw toepassingen online voorbereiden nadat een failover of een herstel aanvullende duurt en waarschijnlijk ook vragen om oplossen op een moment van belasting - een ongeldige combinatie.

### <a name="failover-to-a-geo-replicated-secondary-database"></a>Failover met een secundaire geografische gerepliceerd-database 

Als u actieve geografische-replicatie als uw om herstel, [afdwingen van een failover naar een secundair geografische gerepliceerd gebruikt](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database). In enkele seconden, de secundaire wordt gepromoveerd tot de nieuwe primaire en gereed is voor de nieuwe transacties vastleggen en reageren op query's - met slechts een paar seconden van verlies van gegevens voor de gegevens die u hebt u er nog niet zijn gerepliceerd. Zie voor informatie over het failoverproces automatiseren, [ontwerp van een toepassing voor herstel van de cloud](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [AZURE.NOTE] Wanneer u het datacenter weer online hebt, kunt u failback naar de oorspronkelijke primaire (al dan niet).

### <a name="perform-a-geo-restore"></a>Een geografische-herstellen uitvoeren 

Als u automatische back-ups met geografische-redundante opslag herhaling als uw om herstel, [starten van een databaseherstel met geografische herstellen](sql-database-disaster-recovery.md#recover-using-geo-restore). Herstel meestal plaatsvindt binnen 12 uur - met gegevensverlies van maximaal één uur bepaald door wanneer de laatste per uur differentiële back-up met genomen en gerepliceerde. Totdat het herstelproces is voltooid, wordt de database kan niet alle transacties opnemen of reageren op query's. 

> [AZURE.NOTE] Als het datacenter wordt geleverd weer online voordat u uw toepassing met de herstelde database overzet, kunt u gewoon het herstelproces is annuleren.  

### <a name="perform-post-failover--recovery-tasks"></a>Posten failover uitvoeren / Herstel taken 

Na het herstellen van beide waarin, moet u de volgende aanvullende taken uitvoeren voordat u uw gebruikers en toepassingen zijn opnieuw gebruiksklaar:

- Omleidings-clients en clienttoepassingen naar de nieuwe server en herstelde database
- Ervoor zorgen dat juiste serverniveau firewallregels in plaats wanneer gebruikers verbinding maken (of gebruik [firewalls op gebruikersniveau database](sql-database-firewall-configure.md#creating-database-level-firewall-rules))
- Ervoor zorgen dat juiste aanmeldingen en machtigingen voor het siteverzamelingsniveau van hoofddatabase ter plaatse (of gebruik [opgenomen gebruikers](https://msdn.microsoft.com/library/ff929188.aspx))
- Controle, passende configureren
- Meldingen, zo nodig configureren

## <a name="upgrade-an-application-with-minimal-downtime"></a>Upgraden van een toepassing met minimale uitvaltijd

Soms moet een toepassing worden ondernomen offline vanwege gepland onderhoud zoals een upgrade van toepassing. [Beheren toepassingsupgrades](sql-database-manage-application-rolling-upgrade.md) wordt beschreven hoe actieve geografische-herhaling gebruiken om te schakelen van lopende upgrades van de toepassing cloud uitvaltijd bij upgrades en geef het herstelpad in het geval er iets mis gaat. In dit artikel kijkt naar twee verschillende methoden voor het upgradeproces orchestrating en de voordelen en de voor-en nadelen van elke optie.

## <a name="next-steps"></a>Volgende stappen

Zie [ontwerp van een toepassing voor de cloud-herstel](sql-database-designing-cloud-solutions-for-disaster-recovery.md) en [elastische groep noodgevallen herstelstrategieën](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)voor een discussie van toepassing ontwerpoverwegingen voor zelfstandige databases en voor elastische toepassingen.






