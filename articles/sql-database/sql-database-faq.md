<properties 
   pageTitle="Azure SQL-Database Veelgestelde vragen" 
   description="Antwoorden op veelvoorkomende vragen klanten Stel een vraag over cloud-databases en Azure SQL-Database, Microsofts relationele database management systeem (RDBMS) en database als een service die u in de cloud." 
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
   ms.workload="data-management" 
   ms.date="08/16/2016"
   ms.author="sashan;carlrab"/>

# <a name="sql-database-faq"></a>SQL-Database Veelgestelde vragen

## <a name="how-does-the-usage-of-sql-database-show-up-on-my-bill"></a>Hoe wordt het gebruik van de SQL-Database weergegeven op mijn factuur? 
Facturen van de SQL-Database op een overzichtelijk per uur rente op basis van beide de servicelaag + prestatieniveau voor één-databases of eDTUs per elastische database-toepassingen. Werkelijke gebruik is berekend en rato per uur, zodat uw factuur, breuken van een uur kan weergeven. Als er een database voor 12 uur in een maand bestaat, bevat uw factuur bijvoorbeeld gebruik van 0,5 dagen. Daarnaast kunnen zijn Servicelagen + prestatieniveau en eDTUs per toepassingen opgedeeld in de factuur die u gemakkelijker om het aantal database dagen die hebt gebruikt voor elk label in één maand weer te geven.

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>Wat gebeurt er als één database actief is voor minder dan een uur of een hogere servicelaag voor minder dan een uur wordt gebruikt?
Afschrijving voor elk uur bestaat een database met de hoogste servicelaag + prestatieniveau die u hebt toegepast tijdens dat uur, ongeacht Taakgebruik of of de database actief is voor minder dan een uur. Als u één database maakt en verwijdert u deze later vijf minuten weerspiegelt uw factuur bijvoorbeeld een boete voor één database uur. 

Voorbeelden
    
- Als u een eenvoudige database maken en vervolgens direct naar standaard S1 upgraden, wordt u standaard S1 snelheid geheven voor het eerste uur.

- Als u een database vanuit Basic naar Premium om 10:00 upgraden en upgrade is voltooid om 1:35 uur Klik op de volgende dag, wordt geheven snelheid Premium beginnend bij 1:00 a.m. 

- Als u een database van Premium voor eenvoudige om 11:00 downgraden en om 2:15 p.m. is voltooid, wordt de database in rekening wordt gebracht met de Premium-snelheid tot 3:00 uur, waarna het bij de eenvoudige tarieven wordt opgeladen.

## <a name="how-does-elastic-database-pool-usage-show-up-on-my-bill-and-what-happens-when-i-change-edtus-per-pool"></a>Hoe wordt elastische database toepassingen gebruik weergegeven op mijn factuur en wat gebeurt er wanneer ik wijzigen eDTUs per toepassingen?
Elastische database groep kosten weergegeven op uw factuur als elastische DTUs (eDTUs) in de stappen onder eDTUs per toepassingen op [de prijzen pagina](https://azure.microsoft.com/pricing/details/sql-database/)weergegeven. Er zijn geen kosten per database voor elastische database-toepassingen. Afschrijving voor elk uur die een resourcegroep die bestaat op het hoogste eDTU, ongeacht Taakgebruik of opgeven of het gebied voor minder dan een uur actief is. 

Voorbeelden

- Als u een resourcegroep die standaard elastische database met 200 eDTUs om 11:18 uur maakt, wordt vijf databases toevoegen aan de groep, geheven voor 200 eDTUs voor het hele uur, beginnen bij 11: 00 via de rest van de dag.
- Op dag 2, om 5:05 uur, Database 1 begonnen met het gebruik van 50 eDTUs en bevat constante tot en met de dag. Databases 2 tot en met 5 variëren tussen 0 en 80 eDTUs. Tijdens de dag, moet u vijf andere databases die verschillende eDTUs hele dag in beslag nemen toevoegen. Dag 2 is een hele dag op 200 eDTU gefactureerd. 
- Klik op dag 3, om 5 uur u toevoegen een andere 15 databases. Gebruik van de database wordt verhoogd hele dag op het punt waar u besluit om uit te breiden eDTUs voor de toepassingen van 200 tot 400 om 8:05 uur Kosten op het niveau van 200 eDTU van kracht zijn tot en met 8 pm en verhogen naar 400 eDTUs voor de resterende vier uur. 

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-database-pool-show-up-on-my-bill"></a>Hoe wordt het gebruik van actieve geografische-herhaling in een toepassingen elastische database weergegeven op mijn factuur?
In tegenstelling tot één databases beïnvloeden [Actieve geografische-replicatie](sql-database-geo-replication-overview.md) gebruikt met elastische databases niet rechtstreeks facturering.  Alleen wordt geheven voor de eDTUs deze is ingericht voor elk van de groepen (groep met primaire en secundaire van toepassingen)

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>Wat heeft het gebruik van de beleidsfunctie invloed op mijn factuur? 
Controle deel uitmaakt van de service SQL-Database zonder extra kosten en is beschikbaar voor Basic, Standard en Premium-databases. Echter om op te slaan de controlelogboeken, de controle functie doeleinden die een Azure Storage-account en eenheidstarieven voor tabellen en wachtrijen in Azure Storage toepassen op basis van de grootte van het controlelogboek.

## <a name="how-do-i-find-the-right-service-tier-and-performance-level-for-single-databases-and-elastic-database-pools"></a>Hoe vind ik het juiste service laag en prestaties niveau voor één databases en elastische database van toepassingen? 
Er zijn een paar extra beschikbaar zijn voor u. 

- Voor on-premises implementatie-databases, gebruikt u de [DTU hoekformaatgreep advisor](http://dtucalculator.azurewebsites.net/) de databases en DTUs vereist en meerdere databases voor elastische database toepassingen evalueren.
- Als in aanmerking komende één database worden in een groep raadt intelligente van Azure-engine een toepassingen elastische database als er een historische gebruikspatroon dat deze garandeert. Zie [beeldscherm en beheren van een elastische database-toepassingen met de portal van Azure](sql-database-elastic-pool-manage-portal.md). Zie voor meer informatie over hoe u de wiskundige uzelf, [Aandachtspunten voor de prijs en prestaties voor een elastische database-toepassingen](sql-database-elastic-pool-guidance.md)
- Zie [prestaties richtlijnen voor één databases](sql-database-performance-guidance.md)of u moet bellen van één database omhoog of omlaag.

## <a name="how-often-can-i-change-the-service-tier-or-performance-level-of-a-single-database"></a>Hoe vaak kan ik het niveau van service laag of prestaties van één database wijzigen? 
Met V12-databases, kunt u de servicelaag (tussen Basic, Standard en Premium) of het prestatieniveau binnen een servicelaag (bijvoorbeeld S1 op S2) zo vaak als u wilt. Voor databases uit eerdere versies, kunt u het niveau van laag of prestaties service viermaal drukken in een periode van 24 uur in totaal wijzigen.

##<a name="how-often-can-i-adjust-the-edtus-per-pool"></a>Hoe vaak kan ik de eDTUs per toepassingen aanpassen? 
Zo vaak als u wilt.

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-database-pool"></a>Hoe lang duurt het wijzigen van het niveau van service laag of prestaties van één database of een database verplaatsen en afmelden bij een elastische database-toepassingen? 
Wijzigen van de servicelaag van een database te verplaatsen en afmelden bij een resourcegroep moet de database moet worden gekopieerd op het platform als achtergrond. Wijzigen van de servicelaag kunt uitvoeren vanuit een paar minuten tot enkele uren afhankelijk van de grootte van de databases. In beide gevallen blijven de databases online en beschikbaar tijdens het verplaatsen. Zie voor meer informatie over het wijzigen van één databases [wijzigen de servicelaag van een database](sql-database-scale-up.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Wanneer moet ik één database versus elastische databases gebruiken? 
In het algemeen, elastische database-toepassingen zijn gemaakt voor een normale [software-als-een-service (SaaS)-toepassing patroon](sql-database-design-patterns-multi-tenancy-saas-applications.md), indien er één database per klant of tenant. Kopen van afzonderlijke databases en overprovisioning om te voldoen aan de variabele en pieksnelheden aanvraag voor elke database is vaak niet kosten efficiënt. U beheert de collectieve prestaties van de groep van toepassingen, en de databases schaal automatisch omhoog en omlaag. 

Intelligente van Azure-engine wordt aanbevolen van een groep voor databases wanneer een gebruikspatroon deze garandeert. Zie de [SQL-Database prijzen laag aanbevelingen](sql-database-service-tier-advisor.md)voor meer informatie. Zie voor gedetailleerde informatie over het kiezen tussen enkele of elastische databases, [Aandachtspunten voor de prijs en prestaties voor elastische database-toepassingen](sql-database-elastic-pool-guidance.md).

## <a name="what-does-it-mean-to-have-up-to-200-of-your-maximum-provisioned-database-storage-for-backup-storage"></a>Wat betekent dit dat maximaal 200% van uw maximale ingerichte database-opslag voor back-up opslaan? 
Back-opslag is de opslag die is gekoppeld aan uw geautomatiseerde database back-ups die worden gebruikt voor [punt-In-tijd-herstellen](sql-database-recovery-using-backups.md#-point-in-time-restore) en deze [Geografische herstellen](sql-database-recovery-using-backups.md#geo-restore). Microsoft Azure SQL Database biedt maximaal 200% van uw maximale ingerichte database-opslag van back-up opslaan zonder extra kosten. Als u een exemplaar van de standaard DB met een ingerichte DB grootte van 250 GB hebt, kunt u worden bijvoorbeeld voor gegeven met 500 GB back-opslagruimte zonder bijkomende kosten. Als uw database groter is dan de meegeleverde back-up opslaan, kunt u kiezen de bewaarperiode beperken door contact opneemt met Azure ondersteuning of voor de extra back-opslag gefactureerd standaardtarief leestoegang geografisch redundante opslag (AB-GRS) van betalen. Zie voor meer informatie over AB-GRS facturering, opslag prijzen Details.

## <a name="im-moving-from-webbusiness-to-the-new-service-tiers-what-do-i-need-to-know"></a>Ik ben vanuit Web/bedrijven verplaatsen naar de nieuwe Servicelagen, wat moet ik weten?
Azure SQL-Web en bedrijfsdatabases nu teruggetrokken. De lagen Basic, Standard, Premium en elastische Vervang de retiring Web- en -databases. We hebt extra veelgestelde vragen die in deze overgangsperiode uitkomst mogelijk. [Web en Business Edition zonsondergang Veelgestelde vragen](sql-database-web-business-sunset-faq.md)

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>Wat is een vertraging verwachte herhaling wanneer geografische repliceren een database tussen twee delen binnen de dezelfde Azure Geografie?  
We zijn momenteel een vrijgegeven Productieorder van vijf seconden ondersteunende en de vertraging replicatie is kleiner dan dat wanneer de geografische-secundaire wordt gehost in het Azure wordt aangegeven gepaarde regio aanbevolen en klik in het hetzelfde niveau van de service.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>Wat is een vertraging van de verwachte herhaling als geografische en secundaire wordt gemaakt in hetzelfde gebied, als de primaire database?  
Op basis van empirische gegevens, is er niet te veel verschil tussen binnen-regio en tussen regio herhaling vertragings wanneer de Azure aanbevolen gepaarde regio wordt gebruikt. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Als er een netwerkfout tussen twee delen, hoe de logica opnieuw werkt als geografische-replicatie is ingesteld?  
Als er een verbinding verbreken, probeer we elke 10 seconden als u wilt herstellen op verbindingen.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>Wat kan ik doen om te garanderen dat een kritieke wijzigen op de primaire database is gerepliceerd?
De geografische-secundaire is een replica asynchrone en probeer we niet te behouden in volledige synchroniseren met de primaire. Maar wij bieden een methode als u wilt dat de synchronisatie om ervoor te zorgen de replicatie van kritieke wijzigingen (bijvoorbeeld wachtwoord updates). Afgedwongen synchronisatie van invloed op prestaties omdat de thread worden geblokkeerd totdat alle vastgelegde transacties worden gerepliceerd. Zie [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx)voor meer informatie. 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Functies zijn beschikbaar voor het controleren van de vertraging replicatie tussen de primaire database en de geografische en secundaire?
We de vertraging realtime replicatie tussen de primaire database en de geografische en secundaire via een DMV laten zien. Zie [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx)voor meer informatie.
