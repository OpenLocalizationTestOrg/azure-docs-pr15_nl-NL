<properties 
    pageTitle="Een failover of niet gepland initiëren voor Azure SQL-Database met PowerShell | Microsoft Azure" 
    description="Een failover of niet gepland initiëren voor Azure SQL-Database via PowerShell" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-powershell"></a>Een failover of niet gepland initiëren voor Azure SQL-Database met PowerShell



> [AZURE.SELECTOR]
- [Azure-portal](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


In dit artikel leest u hoe een failover of niet gepland voor SQL-Database met PowerShell wordt gestart. Zie [Geografische-replicatie configureren voor Azure SQL-Database](sql-database-geo-replication-powershell.md)configureren geografische-herhaling.



## <a name="initiate-a-planned-failover"></a>Een geplande failover-

Gebruik de cmdlet **Set-AzureRmSqlDatabaseSecondary** met de parameter **- Failover** te bevorderen een secundaire database de nieuwe primaire database, de bestaande primaire als u wilt een secundair worden degraderen tot. Deze functionaliteit is bedoeld voor een geplande failover, zoals tijdens noodgevallen herstel boren, en is vereist dat de hoofddatabase beschikbaar zijn.

De opdracht voert de werkstroom voor het volgende:

1. Tijdelijk overschakelen herhaling op synchrone modus. Hierdoor wordt alle openstaande transacties aan de secundaire worden gewist.

2. Overschakelen van de rollen van de twee databases in de geografische-replicatie-verbinding.  

Deze reeks zorgt ervoor dat de twee databases zijn gesynchroniseerd voordat de rollen veranderen en dus geen gegevensverlies zal plaatsvinden. Er is een korte termijn waarin beide databases niet beschikbaar (volgorde van 0 tot en met 25 seconden zijn) terwijl de rollen worden veranderd. De gehele bewerking moet minder dan een minuut wilt uitvoeren onder normale omstandigheden nemen. Zie [Set-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt619393.aspx)voor meer informatie.




Deze cmdlet retourneert wanneer het proces voor het overschakelen van de secundaire database naar primaire is voltooid.

De volgende opdracht schakelt u de rollen van de database met de naam 'mijndb' op de server "srv2" onder de resource-groep "rg2" naar primaire. De oorspronkelijke primaire waaraan "db2" is verbonden met wordt overgeschakeld naar secundaire nadat de twee databases volledig zijn gesynchroniseerd.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary -Failover


> [AZURE.NOTE] In sommige gevallen is het mogelijk dat de bewerking niet voltooien en niet reageert mogelijk weergegeven. In dit geval kan de gebruiker de dwingen failover-opdracht (niet-geplande failover) bellen en gegevensverlies accepteren.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Een niet-geplande failover uit de primaire database met de secundaire database initiëren


U kunt de cmdlet **Set-AzureRmSqlDatabaseSecondary** met **– Failover** en **-AllowDataLoss** parameters promoveren een secundaire database als u wilt de nieuwe primaire database op een niet-geplande wijze, worden automatisch de degraderen van de bestaande primaire te worden van een secundair tegelijk wanneer de hoofddatabase niet langer beschikbaar is.

Deze functionaliteit is bedoeld voor herstel wanneer beschikbaarheid van de database terugzetten kritieke is en gegevensverlies acceptabel is. Wanneer afgedwongen failover wordt gestart, wordt de opgegeven secundaire database onmiddellijk wordt de hoofddatabase en begonnen met het accepteren van transacties schrijven. Zodra de hoofddatabase van de oorspronkelijke opnieuw verbinding maakt met de hoofddatabase van deze nieuwe na de bewerking afgedwongen failover wordt, een incrementele back-up is gemaakt met de hoofddatabase van de oorspronkelijke en de oude primaire database is gemaakt in een secundaire database voor de nieuwe primaire database. het is slechts een replica van de nieuwe primaire.

Maar omdat punt In tijd herstellen wordt niet ondersteund op secundaire-databases, als u wilt herstellen gegevens vastgelegd voor de oude primaire database waarin had niet gerepliceerd naar de nieuwe primaire database, moet u CSS aan een database terugzetten naar de bekende back-up van deelnemen.

> [AZURE.NOTE] Als de opdracht wordt gegeven wanneer de primaire en secundaire vraag are online de oude primaire komen de nieuwe secundaire onmiddellijk zonder gegevenssynchronisatie. Als de primaire kunnen transacties doorgevoerd wanneer u de opdracht verzendt gegevensverlies optreden.


Als de primaire database meerdere secundaire servers heeft slagen de opdracht gedeeltelijk. De secundaire waarop de opdracht is uitgevoerd worden primaire. De oude primaire echter, blijven de primaire, dat wil zeggen de twee primaire uiteindelijk inconsistente status en verbonden door een koppeling geschorst herhaling. De gebruiker moet deze configuratie een "verwijderen secundaire"-API gebruiken op een van deze primaire databases handmatig te herstellen.


De volgende opdracht schakelt u de rollen van de database met de naam 'mijndb' naar primaire als de primaire niet beschikbaar is. De oorspronkelijke primaire waaraan 'mijndb' is verbonden met wordt overgeschakeld naar secundaire nadat deze weer online is. Op dat moment de synchronisatie kan dit leiden tot verlies van gegevens.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary –Failover -AllowDataLoss




## <a name="next-steps"></a>Volgende stappen   

- Controleer dat de verificatievereisten voor de server en database zijn geconfigureerd op de nieuwe primaire na een failover. Zie [beveiliging van de SQL-Database na herstel](sql-database-geo-replication-security-config.md)voor meer informatie.
- Zie voor meer herstellen na een noodgevallen met actieve geografische-replicatie, met inbegrip van de pre en procedure posten en uitvoeren van een detailanalyse noodgevallen herstel, [Noodgevallen herstel boren](sql-database-disaster-recovery.md)
- Zie voor een blogbericht Sasha Nosov over actieve geografische-herhaling [Spotlight op nieuwe geografische herhaling mogelijkheden](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Voor informatie over het ontwerpen van cloud-toepassingen actieve geografische-replicatie gebruikt, raadpleegt u [ontwerpen cloud toepassingen voor bedrijfscontinuïteit met geografische-herhaling](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Zie voor informatie over het gebruik van de actieve geografische-replicatie met elastische database pools [elastische groep noodgevallen herstelstrategieën](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Zie [Overzicht van de bedrijven bedrijfscontinuïteit](sql-database-business-continuity.md) voor een overzicht van business continurity,
