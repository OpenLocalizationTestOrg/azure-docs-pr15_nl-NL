<properties
    pageTitle="Geografische-replicatie configureren voor Azure SQL-Database met Transact-SQL | Microsoft Azure"
    description="Geografische-replicatie configureren voor Azure SQL-Database met Transact-SQL"
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

# <a name="configure-geo-replication-for-azure-sql-database-with-transact-sql"></a>Geografische-replicatie configureren voor Azure SQL-Database met Transact-SQL

> [AZURE.SELECTOR]
- [Overzicht](sql-database-geo-replication-overview.md)
- [Azure-Portal](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

In dit artikel leest u het actieve geografische-replicatie configureren voor een Azure SQL-Database met Transact-SQL.

Als u wilt starten met Transact-SQL failover, raadpleegt u [een failover- of niet gepland voor Azure SQL-Database met Transact-SQL](sql-database-geo-replication-failover-transact-sql.md).

>[AZURE.NOTE] Actieve geografische-herhaling (leesbare ontvangen) is nu beschikbaar voor alle databases in alle Servicelagen. In April 2017 de niet-leesbare secundaire type wordt ingetrokken en bestaande niet-leesbare databases automatisch worden bijgewerkt naar leesbare secundaire servers.

Actieve geografische-replicatie configureren via Transact-SQL, moet u de volgende handelingen uit:

- Een Azure-abonnement.
- Een logische Azure SQL Database-server <MyLocalServer> en een SQL-database <MyDB> -de primaire database die u wilt repliceren.
- Een of meer logische Azure SQL Database-servers < MySecondaryServer(n) > - de logische servers die de partnerservers waarin u secundaire databases wilt maken.
- Een aanmelding die DBManager op de primaire, db_ownership van de lokale database dat u geografische-repliceren, zult hebben en DBManager op de partner (s) waaraan u geografische-replicatie wordt configureren.
- SQL Server Management Studio (SSMS)

> [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de nieuwste versie van Management Studio gebruiken om u te blijven synchroon met updates voor Microsoft Azure en SQL-Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="add-secondary-database"></a>Secundaire database toevoegen

U kunt de instructie **ALTER DATABASE** gebruiken om te maken van een secundaire geografische gerepliceerd-database op een partnerserver. U uitvoeren deze instructie op de hoofddatabase van de server met de database moet worden gerepliceerd. De database geografische gerepliceerd (de "primaire database") heeft dezelfde naam als de database worden gerepliceerd en, al dan niet standaard, heeft hetzelfde serviceniveau als de hoofddatabase van. De secundaire database kunt werken met leesbare of niet-leesbare en kunt werken met één database of een elastische databbase. Zie [DATABASE wijzigen (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) en [Servicelagen](sql-database-service-tiers.md)voor meer informatie.
Nadat de secundaire database is gemaakt en agarvoedingsbodem, begint gegevens repliceren asynchroon uit de hoofddatabase. De onderstaande stappen wordt uitgelegd hoe u geografische-replicatie configureren met Management Studio. Stappen voor het maken van niet-leesbare en leesbaar ontvangen, met één database of een elastische-database, worden gegeven.

> [AZURE.NOTE] Als er een database op de partnerserver opgegeven met dezelfde naam als de hoofddatabase van die de opdracht bestaat, mislukt.


### <a name="add-non-readable-secondary-single-database"></a>Niet-leesbare secundair (één database) toevoegen

Gebruik de volgende stappen uit om te maken van een niet-leesbare secundair als één database.

1. Met versie 13.0.600.65 of hoger van SQL Server Management Studio.

     > [AZURE.IMPORTANT] Download de [nieuwste](https://msdn.microsoft.com/library/mt238290.aspx) versie van SQL Server Management Studio. Het wordt aanbevolen dat u altijd de nieuwste versie van Management Studio gebruiken om u te blijven synchroon met updates van de Azure-portal.


2. Open de map Databases, vouwt u de map **System Databases** , met de rechtermuisknop op de **basispagina**en klik vervolgens op **Nieuwe Query**.

3. Gebruik de volgende instructie **ALTER DATABASE** om een lokale database naar een geografische-herhaling primaire met een niet-leesbare secundaire database op MySecondaryServer1 waar MySecondaryServer1 is voor de beschrijvende servernaam.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer1> WITH (ALLOW_CONNECTIONS = NO);

4. Klik op **uitvoeren** als de query wilt uitvoeren.


### <a name="add-readable-secondary-single-database"></a>Leesbare secundair (één database) toevoegen
Gebruik de volgende stappen uit om te maken van een leesbare secundair als één database.

1. In Management Studio verbinding met uw logische Azure SQL Database-server.

2. Open de map Databases, vouwt u de map **System Databases** , met de rechtermuisknop op de **basispagina**en klik vervolgens op **Nieuwe Query**.

3. Gebruik de volgende instructie **ALTER DATABASE** om een lokale database naar een geografische-herhaling primaire met een leesbare secundaire database op een secundaire server.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);

4. Klik op **uitvoeren** als de query wilt uitvoeren.



### <a name="add-non-readable-secondary-elastic-database"></a>Niet-leesbare secundair (elastische database) toevoegen

Gebruik de volgende stappen uit om te maken van een niet-leesbare secundair als een elastische-database.

1. In Management Studio verbinding met uw logische Azure SQL Database-server.

2. Open de map Databases, vouwt u de map **System Databases** , met de rechtermuisknop op de **basispagina**en klik vervolgens op **Nieuwe Query**.

3. Gebruik de volgende instructie **ALTER DATABASE** om een lokale database naar een geografische-herhaling primaire met een niet-leesbare secundaire database op een secundaire server in een elastische toepassingen.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer3> WITH (ALLOW_CONNECTIONS = NO
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool1));

4. Klik op **uitvoeren** als de query wilt uitvoeren.



### <a name="add-readable-secondary-elastic-database"></a>Leesbare secundair (elastische database) toevoegen
Gebruik de volgende stappen uit om te maken van een leesbare secundair als een elastische-database.

1. In Management Studio verbinding met uw logische Azure SQL Database-server.

2. Open de map Databases, vouwt u de map **System Databases** , met de rechtermuisknop op de **basispagina**en klik vervolgens op **Nieuwe Query**.

3. Gebruik de volgende instructie **ALTER DATABASE** om een lokale database naar een geografische-herhaling primaire met een leesbare secundaire database op een secundaire server in een elastische toepassingen.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));

4. Klik op **uitvoeren** als de query wilt uitvoeren.



## <a name="remove-secondary-database"></a>Secundaire database verwijderen

U kunt de instructie **ALTER DATABASE** permanent beëindigen de replicatiesamenwerking tussen een secundaire database en de primaire. Deze instructie wordt uitgevoerd op de hoofddatabase waarop de primaire database zich bevindt. Na de beëindiging van de relatie wordt de secundaire database een normale alleen-lezen-database. Als de connectiviteit met secundaire database niet meer werkt de opdracht is geslaagd, maar de secundaire worden alleen-lezen-schrijven nadat verbinding is hersteld. Zie [DATABASE wijzigen (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) en [Servicelagen](sql-database-service-tiers.md)voor meer informatie.

Gebruik de volgende stappen geografische gerepliceerd secundair verwijderen uit een geografische-replicatie-verbinding.

1. In Management Studio verbinding met uw logische Azure SQL Database-server.

2. Open de map Databases, vouwt u de map **System Databases** , met de rechtermuisknop op de **basispagina**en klik vervolgens op **Nieuwe Query**.

3. Gebruik de volgende instructie **ALTER DATABASE** te verwijderen van een secundair geografische gerepliceerd.

        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;

4. Klik op **uitvoeren** als de query wilt uitvoeren.

## <a name="monitor-geo-replication-configuration-and-health"></a>Geografische herhaling configuratie en servicestatus bewaken

Controle taken opnemen van de geografische-replicatie-configuratie voor controle en gegevens herhaling servicestatus bewaken.  U kunt de weergave van de dynamische beheer **sys.dm_geo_replication_links** gebruiken in de hoofddatabase om terug te keren informatie over alle Forms herhaling koppelingen voor elke database op de logische Azure SQL Database-server. Deze weergave bevat een rij voor elk van de koppeling herhaling tussen primaire en secundaire databases. U kunt de weergave van de dynamische beheer **sys.dm_replication_link_status** gebruiken om te retourneren van een rij voor elke Azure SQL-Database die momenteel een herhaling replicatie-koppeling. Dit geldt ook voor primaire en secundaire databases. Als meer dan één continue herhaling koppeling voor de hoofddatabase van een opgegeven bestaat, wordt in deze tabel een rij voor elk van de relaties bevat. De weergave is gemaakt in alle databases, met inbegrip van het logische model. Query's uitvoeren in deze weergave in de hoofdlijst logische retourneert echter een lege set. U kunt de weergave van de dynamische beheer **sys.dm_operation_status** gebruiken om weer te geven van de status van alle databasebewerkingen, waaronder de status van de koppelingen herhaling. Zie [sys.geo_replication_links (Azure SQL-Database)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm_geo_replication_link_status (Azure SQL-Database)](https://msdn.microsoft.com/library/mt575504.aspx)en [sys.dm_operation_status (Azure SQL-Database)](https://msdn.microsoft.com/library/dn270022.aspx)voor meer informatie.

Gebruik de volgende stappen uit om een verbinding geografische herhaling de te houden.

1. In Management Studio verbinding met uw logische Azure SQL Database-server.

2. Open de map Databases, vouwt u de map **System Databases** , met de rechtermuisknop op de **basispagina**en klik vervolgens op **Nieuwe Query**.

3. Gebruik de volgende instructie te geven van alle databases met geografische herhaling koppelingen.

        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM [sys].geo_replication_links;

4. Klik op **uitvoeren** als de query wilt uitvoeren.
5. Open de map Databases, vouwt u de map **System Databases** , met de rechtermuisknop op **mijndb**en klik vervolgens op **Nieuwe Query**.
6. De volgende instructie gebruiken om weer te geven van de herhaling vertragingen bij het berekenen en de laatste replicatie tijd van mijn secundaire databases van mijndb.

        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status

7. Klik op **uitvoeren** als de query wilt uitvoeren.
8. Gebruik de volgende instructie te geven van de meest recente geografische-replicatie-bewerkingen die zijn gekoppeld aan de database mijndb.

        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC

9. Klik op **uitvoeren** als de query wilt uitvoeren.

## <a name="upgrade-a-non-readable-secondary-to-readable"></a>Een niet-leesbare secundair upgraden naar leesbare

In April 2017 de niet-leesbare secundaire type wordt ingetrokken en bestaande niet-leesbare databases automatisch worden bijgewerkt naar leesbare secundaire servers. Als u niet-leesbare secundaire servers vandaag gebruikt en u wilt upgraden, zodat ze worden gelezen, kunt u de volgende eenvoudige stappen voor elke secundair.

> [AZURE.IMPORTANT] Er is geen ter plaatse een upgrade van een niet-leesbare secundair naar leesbare selfservice-methode. Als u uw alleen secundaire plaatst, klikt u vervolgens blijft de hoofddatabase onbeveiligde totdat de nieuwe secundaire volledig is gesynchroniseerd. Als van uw toepassing SLA vereist dat de primaire altijd is beveiligd, kunt u overwegen een parallelle secundair maken in een andere server voordat de upgrade bovenstaande stappen is toegepast. Houd er rekening mee dat elke primair kan maximaal 4 secundaire databases hebben.


1. Eerst verbinding maken met de *secundaire* server en de niet-leesbare secundaire database weghalen:  
        
        DROP DATABASE <MyNonReadableSecondaryDB>;

2. Nu verbinding maken met de *primaire* server en een nieuwe leesbare secundair toevoegen

        ALTER DATABASE <MyDB>
            ADD SECONDARY ON SERVER <MySecondaryServer> WITH (ALLOW_CONNECTIONS = ALL);




## <a name="next-steps"></a>Volgende stappen

- Zie meer informatie over actieve geografische-replicatie, - [Actieve geografische-herhaling](sql-database-geo-replication-overview.md)
- Zie [overzicht van Business-bedrijfscontinuïteit](sql-database-business-continuity.md) voor een overzicht van de bedrijfscontinuïteit bedrijven en scenario's,
