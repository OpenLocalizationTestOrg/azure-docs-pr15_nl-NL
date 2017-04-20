<properties 
    pageTitle="Een failover of niet gepland initiëren voor Azure SQL-Database met Transact-SQL | Microsoft Azure" 
    description="Een failover of niet gepland initiëren voor Azure SQL-Database met Transact-SQL" 
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
    ms.date="08/29/2016"
    ms.author="carlrab"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-transact-sql"></a>Een failover of niet gepland initiëren voor Azure SQL-Database met Transact-SQL


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


In dit artikel leest u hoe failover met een secundaire SQL-Database wordt gestart met Transact-SQL. Zie [Geografische-replicatie configureren voor Azure SQL-Database](sql-database-geo-replication-transact-sql.md)configureren geografische-herhaling.



Als u wilt starten failover, hebt u het volgende nodig:

- Een aanmelding die DBManager op de primaire, db_ownership van de lokale database dat u geografische-repliceren, zult hebben en DBManager op de partner (s) waaraan u geografische-replicatie wordt configureren.
- SQL Server Management Studio (SSMS)


> [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de nieuwste versie van Management Studio gebruiken om u te blijven synchroon met updates voor Microsoft Azure en SQL-Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).




## <a name="initiate-a-planned-failover-promoting-a-secondary-database-to-become-the-new-primary"></a>Een geplande failover-promoveren van een secundaire database als u wilt worden de nieuwe primaire

U kunt de instructie **ALTER DATABASE** verhogen van een secundaire database als u wilt de nieuwe primaire database op een geplande wijze, worden de bestaande primaire als u wilt een secundair worden degraderen. Deze instructie wordt uitgevoerd op de hoofddatabase op de logische Azure SQL Database-server waarin de op geografische gerepliceerd secundaire database die is gepromoveerd zich bevindt. Deze functionaliteit is ontworpen voor geplande failover, zoals tijdens de oefeningen DR en vereist dat de hoofddatabase beschikbaar zijn.

De opdracht voert de werkstroom voor het volgende:

1. Schakelt u tijdelijk herhaling naar synchrone modus, veroorzaakt door alle openstaande transacties moeten worden opgeschoond aan de secundaire en alle nieuwe transacties; blokkeren

2. Schakelopties voor de rollen van de twee databases in de geografische-replicatie-verbinding.  

Deze reeks zorgt ervoor dat de twee databases zijn gesynchroniseerd voordat de rollen veranderen en dus geen gegevensverlies zal plaatsvinden. Er is een korte termijn waarin beide databases niet beschikbaar (volgorde van 0 tot en met 25 seconden zijn) terwijl de rollen worden veranderd. Als de primaire database meerdere secundaire databases bevat, wordt de andere ontvangen verbinding maken met de nieuwe primaire automatisch configureren, de opdracht.  De gehele bewerking moet minder dan een minuut wilt uitvoeren onder normale omstandigheden nemen. Zie [DATABASE wijzigen (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) en [Servicelagen](sql-database-service-tiers.md)voor meer informatie.


Gebruik de volgende stappen aan een geplande failover.

1. In Management Studio verbinding met de logische Azure SQL Database-server waarin een secundaire geografische gerepliceerd-database zich bevindt.

2. Open de map Databases, vouwt u de map **System Databases** , met de rechtermuisknop op de **basispagina**en klik vervolgens op **Nieuwe Query**.

3. Gebruik de volgende instructie **ALTER DATABASE** om de secundaire database gaan naar de primaire rol.

        ALTER DATABASE <MyDB> FAILOVER;

4. Klik op **uitvoeren** als de query wilt uitvoeren.

>[AZURE.NOTE] In sommige gevallen is het mogelijk dat de bewerking niet voltooien en zitten wordt mogelijk weergegeven. In dit geval kan de gebruiker de dwingen failover-opdracht uitvoeren en gegevensverlies accepteren.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Een niet-geplande failover uit de primaire database met de secundaire database initiëren

U kunt de instructie **ALTER DATABASE** verhogen een secundaire database als u wilt de nieuwe primaire database op een niet-geplande wijze, worden automatisch de degraderen van de bestaande primaire te worden van een secundair tegelijk wanneer de primaire database niet langer beschikbaar is. Deze instructie wordt uitgevoerd op de hoofddatabase op de logische Azure SQL Database-server waarin de op geografische gerepliceerd secundaire database die is gepromoveerd zich bevindt.

Deze functionaliteit is bedoeld voor herstel wanneer beschikbaarheid van de database terugzetten kritieke is en gegevensverlies acceptabel is. Wanneer afgedwongen failover wordt gestart, wordt de opgegeven secundaire database onmiddellijk wordt de hoofddatabase en begonnen met het accepteren van transacties schrijven. Zodra de hoofddatabase van de oorspronkelijke opnieuw verbinding maakt met deze nieuwe primaire database wordt, een incrementele back-up is gemaakt met de hoofddatabase van de oorspronkelijke en de oude primaire database is gemaakt in een secundaire database voor de nieuwe primaire database. het is slechts een synchroniseren replica van de nieuwe primaire.

Echter omdat punt In tijd herstellen wordt niet ondersteund op de secundaire-databases, als de gebruiker wil gegevens vastgelegd voor de oude primaire database die niet had zijn gerepliceerd naar de nieuwe primaire database voordat de afgedwongen plaatsgevonden herstellen, moet de gebruiker verrichten ondersteuning herstellen van deze gegevens verloren gegaan.

Als de primaire database meerdere secundaire databases bevat, wordt de andere ontvangen verbinding maken met de nieuwe primaire automatisch configureren, de opdracht.

Gebruik de volgende stappen uit om te starten een niet-geplande failover.

1. In Management Studio verbinding met de logische Azure SQL Database-server waarin een secundaire geografische gerepliceerd-database zich bevindt.

2. Open de map Databases, vouwt u de map **System Databases** , met de rechtermuisknop op de **basispagina**en klik vervolgens op **Nieuwe Query**.

3. Gebruik de volgende instructie **ALTER DATABASE** om de secundaire database gaan naar de primaire rol.

        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;

4. Klik op **uitvoeren** als de query wilt uitvoeren.

>[AZURE.NOTE] Als de opdracht wordt gegeven wanneer de primaire en secundaire vraag are online de oude primaire komen de nieuwe secundaire onmiddellijk zonder gegevenssynchronisatie. Als de primaire kunnen transacties doorgevoerd wanneer u de opdracht verzendt gegevensverlies optreden.



## <a name="next-steps"></a>Volgende stappen   

- Controleer dat de verificatievereisten voor de server en database zijn geconfigureerd op de nieuwe primaire na een failover. Zie [beveiliging van de SQL-Database na herstel](sql-database-geo-replication-security-config.md)voor meer informatie.
- Zie voor meer herstellen na een noodgevallen met actieve geografische-replicatie, met inbegrip van de pre en procedure posten en uitvoeren van een detailanalyse noodgevallen herstel, [Herstel](sql-database-disaster-recovery.md)
- Zie voor een blogbericht Sasha Nosov over actieve geografische-herhaling [Spotlight op nieuwe geografische herhaling mogelijkheden](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Voor informatie over het ontwerpen van cloud-toepassingen actieve geografische-replicatie gebruikt, raadpleegt u [ontwerpen cloud toepassingen voor bedrijfscontinuïteit met geografische-herhaling](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Zie voor informatie over het gebruik van de actieve geografische-replicatie met elastische database pools [elastische groep noodgevallen herstelstrategieën](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Zie [Overzicht van de bedrijven bedrijfscontinuïteit](sql-database-business-continuity.md) voor een overzicht van business continurity,
