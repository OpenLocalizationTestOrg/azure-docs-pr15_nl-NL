<properties
    pageTitle="Azure SQL-Database met behulp van de Azure-Portal beheren | Microsoft Azure"
    description="Informatie over het gebruik van de Azure-Portal voor het beheren van een relationele database in de cloud met behulp van de Azure-Portal."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.date="09/19/2016"
    ms.author="sstein"/>


# <a name="managing-azure-sql-databases-using-the-azure-portal"></a>Azure SQL-Databases met behulp van de Azure portal beheren


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-manage-powershell.md)

De [portal van Azure](https://portal.azure.com/) kunt maken, controleren en beheren van Azure SQL-databases en servers. In dit artikel vindt u een snelle beschrijving en koppelingen naar de details van de meest voorkomende taken.

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Uw Azure SQL-databases, servers en groepen weergeven

Als u wilt bekijken van de beschikbare services voor SQL-Database, klikt u op **meer services**en typt u **SQL** in het zoekvak:

![SQL-Database](./media/sql-database-manage-portal/sql-services.png)


## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Hoe ik maken of SQL Azure-databases weergeven?

**SQL-databases**, klikt u op als u wilt openen van het blad **SQL-databases** , en klik op de database die u werken wilt met, of klik op **+ toevoegen** als een SQL-database wilt maken. Zie [een SQL-database in minuten met behulp van de Azure-portal maken](sql-database-get-started.md)voor meer informatie.


![SQL-databases](./media/sql-database-manage-portal/sql-databases.png)


## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Hoe ik maken of Azure SQL-servers weergeven?

**SQL-servers**, klikt u op om het **SQL-servers** blad hebt geopend, en klik op de server die u werken wilt met, of klik op **+ toevoegen** als u wilt maken van een SQL server. Zie [een SQL-database in minuten met behulp van de Azure-portal maken](sql-database-get-started.md)voor meer informatie.

![SQL-servers](./media/sql-database-manage-portal/sql-servers.png)


## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>Hoe ik maken of SQL elastische groepen weergeven?

**Elastische SQL-toepassingen**, klikt u op om te openen op het blad **elastische SQL-toepassingen** , en klik op de toepassingen die u werken wilt met, of klik op **+ toevoegen** als u wilt maken van een groep. Zie [een elastische database van toepassingen met de portal van Azure maken](sql-database-elastic-pool-create-portal.md)voor meer informatie.

![Elastische SQL-toepassingen](./media/sql-database-manage-portal/elastic-pools.png)



## <a name="how-do-i-update-or-view-sql-database-settings"></a>Hoe ik bijwerken of SQL database-instellingen weergeven?

Als u wilt bekijken of uw database-instellingen bijwerken, klikt u op de gewenste instelling op het blad SQL-database:


![Instellingen voor SQL-database](./media/sql-database-manage-portal/settings.png)


## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Hoe vind ik de naam van een SQL-databases volledig gekwalificeerde server?

Als u wilt weergeven op de naam van uw databases-server, klikt u op het blad **SQL-database** op **Overview** en noteer de servernaam:


![Instellingen voor SQL-database](./media/sql-database-manage-portal/server-name.png)


## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>Hoe beheer ik firewallregels toegang tot mijn SQL server en database te beheren?

Als u wilt weergeven, maken of bijwerken firewallregels, klik op het blad **SQL-database** op **een firewall instellen** . Zie [een regel voor het niveau van de server firewall van Azure SQL-Database met behulp van de Azure portal configureren](sql-database-configure-firewall-settings.md)voor meer informatie.


![firewallregels](./media/sql-database-manage-portal/sql-database-firewall.png)


## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>Hoe wijzig ik mijn SQL database-service laag of prestaties niveau?


Als u het niveau van service laag of prestaties van een SQL-database bijwerken, klikt u op de **prijzen laag (schaal DTUs)** op het blad **SQL-database** . Zie [de service laag en prestaties niveau wijzigen (prijzen laag) van een SQL-database](sql-database-scale-up.md)voor meer informatie.


![prijzen van lagen](./media/sql-database-manage-portal/pricing-tier.png)


## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Hoe configureer ik controle en bedreiging detectie voor een SQL-database?

Als u wilt controleren en bedreiging detectie voor een SQL-database configureren, klik op **controle en bedreiging-detectie** op het blad **SQL-database** . Zie [aan de slag met SQL-database controle](sql-database-auditing-get-started.md)en [aan de slag met SQL Database bedreiging detectie](sql-database-threat-detection-get-started.md)voor meer informatie.


## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Hoe configureer ik dynamische gegevens masking voor een SQL-database?

Als u wilt configureren dynamische gegevens masking voor een SQL-database, klikt u op **dynamische gegevens masking** in het blad **SQL-database** . Zie voor meer informatie [aan de slag met SQL Database dynamische gegevens-Masking](sql-database-dynamic-data-masking-get-started.md).


## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Hoe configureer ik transparante gegevensversleuteling (TDE) voor een SQL-database?

Als u wilt configureren transparante gegevensversleuteling voor een SQL-database, klikt u op **transparante gegevensversleuteling** in het blad **SQL-database** . Zie [TDE inschakelen op een database met behulp van de portal](https://msdn.microsoft.com/library/dn948096#Anchor_1)voor meer informatie.

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Hoe ik bekijken of wijzigen van de maximale grootte van een SQL-database?

Als u wilt weergeven of wijzigen van de grootte van een SQL-database, klikt u op de **omvang van Database** op het blad **SQL-database** . De maximale grootte van een database bijwerken door het wijzigen van de laag of prestaties niveau van de service. Zie [de service laag en prestaties niveau wijzigen (prijzen laag) van een SQL-database](sql-database-scale-up.md)voor meer informatie.

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Hoe ik bewaken en verbeteren de prestaties van een SQL-database?

Als u wilt controleren en prestatie-eigenschappen van een SQL-database te verbeteren, klikt u op de **prestaties-overview** op het blad **SQL-database** . Zie [SQL Database-prestaties inzicht](sql-database-performance.md)voor meer informatie.


## <a name="how-do-i-configure-geo-replication"></a>Hoe configureer ik geografische herhaling?

Klik op **Geografische-replicatie** op het blad **SQL-database** geografische herhaling instellen voor een SQL-database gaat. Zie [Geografische-replicatie configureren voor Azure SQL-Database in de portal van Azure](sql-database-geo-replication-portal.md)voor meer informatie.


## <a name="how-do-i-failover-to-a-geo-replicated-sql-database"></a>Hoe meld ik failover naar een geografische gerepliceerd SQL-database?

Foutherstel naar een secundair geografische gerepliceerd, **Geografische-replicatie** op het blad **SQL-database** , klik op **Failover**. Zie [een failover- of niet gepland voor Azure SQL-Database in de portal van Azure](sql-database-geo-replication-failover-portal.md)voor meer informatie.


## <a name="how-do-i-copy-a-sql-database"></a>Hoe kopieer ik een SQL-database?

Als u wilt kopiëren van een SQL-database, klik op **kopiëren** op het blad **SQL-database** . Zie [een Azure SQL-database met behulp van de Azure portal kopiëren](sql-database-copy-portal.md)voor meer informatie.


![Instellingen voor SQL-database](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Hoe moet ik een Azure SQL-database naar een bestand Bacpac-archiveren?

Als u wilt een BACPAC van een SQL-database maken, klikt u op **exporteren** in het blad **SQL-database** . Zie [archief een Azure SQL-database naar een Bacpac-bestand met behulp van de Azure portal](sql-database-export.md)voor meer informatie.


![SQL-database exporteren](./media/sql-database-manage-portal/sql-database-export.png)



## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Hoe zet ik een SQL-database naar een vorig punt in tijd?

Als u wilt herstellen een SQL-database, klik op het blad **SQL-database** op **herstellen** . Zie [een Azure SQL-Database naar een vorige tijdstip in de portal van Azure herstellen](sql-database-point-in-time-restore-portal.md)voor meer informatie.


![Instellingen voor SQL-database](./media/sql-database-manage-portal/sql-database-restore.png)


## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Hoe maak ik een Azure SQL-database uit een Bacpac-bestand?

Als u wilt een SQL-database maken vanuit een Bacpac-bestand, klikt u op **database importeren** in het **SQL server** -blad. Zie [Bacpac-bestand maken van een Azure SQL-database importeren](sql-database-import.md)voor meer informatie.


![SQL server](./media/sql-database-manage-portal/server-commands.png)


## <a name="how-do-i-restore-a-deleted-sql-database"></a>Hoe kan ik een verwijderde SQL-database herstellen?

Als u een verwijderde SQL-database herstellen, klikt u op **databases verwijderd** in het **SQL server** -blad (de SQL-server die deel uitmaakt van de database die is verwijderd). Zie voor meer informatie [een verwijderde Azure SQL-database met behulp van de Azure portal terugzetten](sql-database-restore-deleted-database-portal.md).

## <a name="how-do-i-delete-a-sql-database"></a>Hoe verwijder ik een SQL-database?

Als u wilt verwijderen van een SQL-database, klikt u op **verwijderen** in het blad **SQL-database** . 

![Instellingen voor SQL-database](./media/sql-database-manage-portal/sql-database-delete.png)



## <a name="additional-resources"></a>Aanvullende informatie

- [SQL-Database](sql-database-technical-overview.md)
- [Controleren en een elastische database-toepassingen met de Azure-portal beheren](sql-database-elastic-pool-manage-portal.md)
