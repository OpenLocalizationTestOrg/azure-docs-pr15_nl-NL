<properties 
    pageTitle="Een failover of niet gepland initiëren voor Azure SQL-Database met de portal van Azure | Microsoft Azure" 
    description="Een failover of niet gepland voor Azure SQL-Database met behulp van de Azure portal initiëren" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-the-azure-portal"></a>Een failover of niet gepland initiëren voor Azure SQL-Database met de portal van Azure


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-geo-replication-failover-portal.md)
- [PowerShell](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


In dit artikel leest u hoe failover met een secundaire SQL-Database met de [portal van Azure](http://portal.azure.com)wordt gestart. Zie [Geografische-replicatie configureren voor Azure SQL-Database](sql-database-geo-replication-portal.md)configureren geografische-herhaling.


## <a name="initiate-a-failover"></a>Een failover-

Als u wilt de primaire worden, kan de secundaire database worden veranderd.  

1. In de [portal van Azure](http://portal.azure.com) bladeren naar de primaire database in de geografische-replicatie-verbinding.
2. Klik op het blad SQL-Database, selecteert u **alle instellingen** > **Geografische-herhaling**.
3. Selecteer in de lijst **secundaire servers voor** de database die u wilt de nieuwe primaire en **Failover**op.

    ![failover][2]

4. Klik op **Ja** om te beginnen met de overname.

De opdracht wordt onmiddellijk de secundaire database overschakelen naar de primaire rol. 

Er is een korte termijn waarin beide databases niet beschikbaar (volgorde van 0 tot en met 25 seconden zijn) terwijl de rollen worden veranderd. Als de primaire database meerdere secundaire databases bevat, wordt de andere ontvangen verbinding maken met de nieuwe primaire automatisch configureren, de opdracht. De gehele bewerking moet minder dan een minuut wilt uitvoeren onder normale omstandigheden nemen. 

>[AZURE.NOTE] Als de primaire online is en doorvoeren van transacties wanneer u de opdracht verzendt gegevensverlies kan optreden.


## <a name="next-steps"></a>Volgende stappen   

- Controleer dat de verificatievereisten voor de server en database zijn geconfigureerd op de nieuwe primaire na een failover. Zie [beveiliging van de SQL-Database na herstel](sql-database-geo-replication-security-config.md)voor meer informatie.
- Zie voor meer herstellen na een noodgevallen met actieve geografische-replicatie, met inbegrip van de pre en procedure posten en uitvoeren van een detailanalyse noodgevallen herstel, [Noodgevallen herstel boren](sql-database-disaster-recovery.md)
- Zie voor een blogbericht Sasha Nosov over actieve geografische-herhaling [Spotlight op nieuwe geografische herhaling mogelijkheden](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Voor informatie over het ontwerpen van cloud-toepassingen actieve geografische-replicatie gebruikt, raadpleegt u [ontwerpen cloud toepassingen voor bedrijfscontinuïteit met geografische-herhaling](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Zie voor informatie over het gebruik van de actieve geografische-replicatie met elastische database pools [elastische groep noodgevallen herstelstrategieën](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Zie [Overzicht van de bedrijven bedrijfscontinuïteit](sql-database-business-continuity.md) voor een overzicht van business continurity,




<!--Image references-->
[1]: ./media/sql-database-geo-replication-failover-portal/failover.png
[2]: ./media/sql-database-geo-replication-failover-portal/secondaries.png
