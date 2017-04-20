<properties 
    pageTitle="Geografische-replicatie configureren voor Azure SQL-Database in de portal van Azure | Microsoft Azure" 
    description="Geografische-replicatie configureren voor Azure SQL-Database met behulp van de Azure portal" 
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
    ms.workload="NA"
    ms.date="10/18/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-the-azure-portal"></a>Geografische-replicatie configureren voor Azure SQL-Database in de portal van Azure


> [AZURE.SELECTOR]
- [Overzicht](sql-database-geo-replication-overview.md)
- [Azure-portal](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

In dit artikel leest u hoe actieve geografische-replicatie configureren voor SQL-Database met de [portal van Azure](http://portal.azure.com).

Als u wilt starten failover met de portal van Azure, raadpleegt u [een failover- of niet gepland voor Azure SQL-Database in de portal van Azure](sql-database-geo-replication-failover-portal.md).

>[AZURE.NOTE] Actieve geografische-herhaling (leesbare ontvangen) is nu beschikbaar voor alle databases in alle Servicelagen. Klik in April 2017, wordt het niet-leesbare secundaire type ingetrokken en bestaande niet-leesbare databases automatisch worden bijgewerkt naar leesbare secundaire servers.

Als u wilt configureren met behulp van de Azure portal geografische-replicatie, moet u de volgende informatiebron:

- Een Azure SQL database - de primaire database die u wilt repliceren naar een ander geografisch gebied.

## <a name="add-secondary-database"></a>Secundaire database toevoegen

Een nieuwe secundaire database maken de volgende stappen in een samenwerkingsverband geografische-herhaling.  

Als u wilt een secundair hebt toegevoegd, moet u de eigenaar van het abonnement of een mede-eigenaar zijn. 

De secundaire database heeft dezelfde naam als de primaire database en, al dan niet standaard, heeft hetzelfde. De secundaire database kan zowel een enkele database of een elastische database. Zie [Servicelagen](sql-database-service-tiers.md)voor meer informatie.
Nadat de secundaire is gemaakt en agarvoedingsbodem, begint gegevens uit de hoofddatabase repliceren naar de nieuwe secundaire database. 

> [AZURE.NOTE] Als de database partner al bestaat (bijvoorbeeld - grond van een vorige geografische herhaling relatie beëindigen) wordt de opdracht mislukt.

### <a name="add-secondary"></a>Secundaire toevoegen

1. Blader naar de database die u wilt instellen voor geografische-herhaling in de [portal van Azure](http://portal.azure.com).
2. Klik op de pagina SQL-database **Geografische herhaling**selecteren en selecteer vervolgens de regio om de secundaire database te maken. U kunt elke regio dan het gebied waarop de primaire-database selecteren, maar de [gekoppelde regio](../best-practices-availability-paired-regions.md) wordt aanbevolen.

    ![Geografische-replicatie configureren](./media/sql-database-geo-replication-portal/configure-geo-replication.png)


4. Selecteer of de server en prijzen laag voor de secundaire database configureren.

    ![secundaire configureren](./media/sql-database-geo-replication-portal/create-secondary.png)

5. U kunt desgewenst een secundaire database toevoegen aan een elastische database-toepassingen:

 Als u wilt de secundaire database maakt in een groep, klik op **elastische database van toepassingen** en selecteer een groep op de doelserver. Een resourcegroep moet al bestaan op de doelserver terwijl deze werkstroom geen een groep maakt.

6. Klik op **maken** om toe te voegen van de secundaire.
 
6. De secundaire database is gemaakt en het seeding wordt gestart. 
 
    ![secundaire configureren](./media/sql-database-geo-replication-portal/seeding0.png)

7. Als het seeding proces voltooid is, wordt de status ervan weergegeven in de secundaire database.

    ![seeding voltooid](./media/sql-database-geo-replication-portal/seeding-complete.png)


## <a name="remove-secondary-database"></a>Secundaire database verwijderen

De bewerking beëindigt de replicatie met de secundaire database permanent en de rol van de secundaire wordt gewijzigd in een normale alleen-lezen-database. Als de verbinding met de secundaire database verbroken is, de opdracht is geslaagd, maar de secundaire hebben die niet worden alleen-lezen totdat na connectivity is hersteld.  

1. Blader naar de primaire database in de samenwerking geografische-herhaling in de [portal van Azure](http://portal.azure.com).
2. Selecteer op de pagina SQL-Database **Geografische-herhaling**.
3. Selecteer de database die u wilt verwijderen uit de samenwerking geografische-herhaling in de lijst **secundaire servers** .
4. Klik op **Replicatie stoppen**.

    ![secundaire verwijderen](./media/sql-database-geo-replication-portal/remove-secondary.png)

5. Op **Replicatie stoppen** , verschijnt er een bevestigingsscherm dus klikt u op **Ja** de database verwijderen uit de geografische-replicatie-verbinding (Stel deze in op een alleen-lezen-database geen deel uitmaakt van eventuele herhaling).


## <a name="next-steps"></a>Volgende stappen

- Zie meer informatie over actieve geografische-replicatie, - [Actieve geografische-herhaling](sql-database-geo-replication-overview.md)
- Zie [overzicht van Business-bedrijfscontinuïteit](sql-database-business-continuity.md) voor een overzicht van de bedrijfscontinuïteit bedrijven en scenario's,

