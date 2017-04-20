<properties
    pageTitle="Rapportage van binnen schaal-out cloud databases | Microsoft Azure"
    description="het instellen van elastische query's via horizontale partities"    
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/27/2016"
    ms.author="torsteng" />

# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Rapportage van binnen schaal-out cloud-databases (preview)

![Query met betrekking tot shards][1]

Een laptopgeheugen databases rijen verdelen over een scaled out gegevens laag. Het schema is identiek zijn op alle deelnemende databases, ook bekend als horizontaal partitioneren. Een elastische query gebruikt, kunt u rapporten die alle databases in een database een laptopgeheugen's beslaan.

Zie voor een snel aan de slag, [rapportage via schaal-out cloud-databases](sql-database-elastic-query-getting-started.md).

Zie voor een niet-laptopgeheugen-databases, [Query in de cloud-databases met verschillende schema's](sql-database-elastic-query-vertical-partitioning.md). 

 
## <a name="prerequisites"></a>Vereisten voor

* Maak een shard-map met de bibliotheek van de client elastische database. Zie [Shard management toewijzen](sql-database-elastic-scale-shard-map-management.md). Of gebruik de voorbeeld-app in [aan de slag met elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md).
* U kunt ook zien [migreren bestaande databases op schaal-out databases](sql-database-elastic-convert-to-use-elastic-tools.md).
* De gebruiker moet beschikken over machtigingen voor de externe gegevensbron wijzigen. Met deze machtiging is opgenomen in de machtiging DATABASE wijzigen.
* EEN externe gegevensbron wijzigen machtigingen nodig zijn om te verwijzen naar de onderliggende gegevensbron.

## <a name="overview"></a>Overzicht

De weergave van de metagegevens van uw een laptopgeheugen gegevenslaag maken deze instructies in de querydatabase elastische. 


1. [BASISPAGINA SLEUTEL MAKEN](https://msdn.microsoft.com/library/ms174382.aspx)
2. [MAKEN VAN DATABASE BEPERKT REFERENTIE](https://msdn.microsoft.com/library/mt270260.aspx)
3. [EXTERNE GEGEVENSBRON MAKEN](https://msdn.microsoft.com/library/dn935022.aspx)
4. [EXTERNE TABEL MAKEN](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a>1.1 database beperkt outmodel sleutel en referenties maken 

De referentie wordt gebruikt door de elastische query verbinding maakt met uw externe databases.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
>[AZURE.NOTE] Zorg ervoor dat de *"\<gebruikersnaam\>"* omvat geen *"@servername"* achtervoegsel. 

## <a name="12-create-external-data-sources"></a>1.2 externe gegevensbronnen maken

Syntaxis:

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                            
            (TYPE = SHARD_MAP_MANAGER,
                    LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a>Voorbeeld 

    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );
 
De lijst met huidige externe gegevensbronnen ophalen: 

    select * from sys.external_data_sources; 

De externe gegevensbron wordt verwezen naar de plattegrond shard. Een elastische query vervolgens wordt de externe gegevensbron en de onderliggende shard-kaart de databases die deel uitmaken van de gegevenslaag opsommen. Dezelfde referenties worden gebruikt om te lezen van de kaart shard en voor toegang tot de gegevens op de shards tijdens het verwerken van een elastische query. 

## <a name="13-create-external-tables"></a>1.3 externe tabellen maken 
 
Syntaxis:  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  
    
    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Voorbeeld**

    CREATE EXTERNAL TABLE [dbo].[order_line]( 
         [ol_o_id] int NOT NULL, 
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL, 
         [ol_number] tinyint NOT NULL, 
         [ol_i_id] int NOT NULL, 
         [ol_delivery_d] datetime NOT NULL, 
         [ol_amount] smallmoney NOT NULL, 
         [ol_supply_w_id] int NOT NULL, 
         [ol_quantity] smallint NOT NULL, 
         [ol_dist_info] char(24) NOT NULL 
    ) 
    
    WITH 
    ( 
        DATA_SOURCE = MyExtSrc, 
        SCHEMA_NAME = 'orders', 
        OBJECT_NAME = 'order_details', 
        DISTRIBUTION=SHARDED(ol_w_id)
    ); 

De lijst met externe tabellen ophalen van de huidige database: 

    SELECT * from sys.external_tables; 

Tabellen wilt laten vervallen externe:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Opmerkingen

De gegevens\_bron-component definieert de externe gegevensbron (een kaart shard) die wordt gebruikt voor de externe tabel.  

Het SCHEMA\_naam en een OBJECT\_naam componenten definitie van de externe tabel toewijzen aan een tabel in een ander schema. Indien weggelaten, wordt uitgegaan van het schema van het externe object "dbo" en wordt uitgegaan van de naam ervan niet identiek zijn aan de naam van de externe tabel wordt gedefinieerd. Dit is handig als de naam van uw externe tabel wordt al gebruikt in de database waar u de externe tabel maken. Zoals u wilt een externe tabel een statistische weergave van de catalogusweergaven definiëren of DMVs op uw scaled out gegevens trapsgewijs. Aangezien catalogusweergaven en DMVs lokaal bestaat, kunt u hun namen voor de definitie van de externe tabel niet gebruiken. In plaats daarvan gebruik een andere naam en het gebruik van de catalogusweergave of van de DMV naam in het SCHEMA\_naam en/of OBJECT\_naam componenten. (Zie onderstaand voorbeeld). 

De component verdeling bepaalt hoe de gegevens die worden gebruikt voor deze tabel. De queryprocessor wordt gebruikgemaakt van de informatie in de component verdeling maken van de efficiëntste query-abonnementen.  

1. **SHARDED** betekent dat er gegevens is horizontaal partitioneren over de databases. De partities-toets voor de verdeling van gegevens is de parameter **< sharding_column_name >** .
2. **GEREPLICEERDE** betekent of identieke kopieën van de tabel op elke database aanwezig zijn. Het is uw verantwoordelijkheid om ervoor te zorgen dat de replica's identiek op de databases zijn.
3. **Afronden\_ROBIN** betekent dat de tabel horizontaal is partities met een verdelingsmethode afhankelijk zijn van toepassing-. 

**Gegevens trapsgewijs verwijzing**: de externe tabel DDL naar een externe gegevensbron verwijst. De externe gegevensbron Hiermee geeft u een shard-kaart waarmee de externe tabel met de gegevens die nodig zijn om te zoeken van alle databases in de gegevenslaag van uw. 


### <a name="security-considerations"></a>Veiligheidsoverwegingen 

Gebruikers met toegang tot de externe tabel toegang automatisch tot de onderliggende externe tabellen onder de referenties die zijn opgegeven in de definitie van de externe gegevensbron. Voorkom ongewenste misbruik van bevoegdheden tot en met de referenties van de externe gegevensbron. Gebruik verlenen of INTREKKEN voor een externe tabel net alsof het een normale tabel.  

Als u de externe gegevensbron en uw externe tabellen hebt gedefinieerd, kunt u nu volledige T-SQL via externe tabellen gebruiken.

## <a name="example-querying-horizontal-partitioned-databases"></a>Voorbeeld: query's uitvoeren horizontale gepartitioneerde databases 

De volgende query een join drie manieren tussen magazijnen, orders en regels uitvoert en worden verschillende samentellingen en een selectief filter. Er wordt aangenomen (1) horizontale partities (sharding) en (2) dat magazijnen, orders en regels een laptopgeheugen door de kolomid BTW-records zijn en dat de elastische query kan samen Zoek de verbindingen in de shards en het dure deel van de query op de shards parallel verwerken. 

    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount, 
         min(ol_delivery_d) as min_deliv_date
    from warehouse 
    join orders 
    on w_id = o_w_id
    join order_line 
    on o_id = ol_o_id and o_w_id = ol_w_id 
    where w_id > 100 and w_id < 200 
    group by w_id, o_c_id 
 
## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Opgeslagen procedure voor de uitvoering van externe T-SQL: sp\_execute_remote

Elastische query maakt u kennis met ook een opgeslagen procedure waarmee directe toegang tot de shards. De opgeslagen procedure heet [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714) en externe opgeslagen procedures of T-SQL-code uitvoeren op de externe database kan worden gebruikt. Het kost de volgende parameters: 

* Naam van de gegevensbron (nvarchar): de naam van de externe gegevensbron van het type RDBMS. 
* Query (nvarchar): de T-SQL-query worden uitgevoerd op elke shard. 
* Parameterdeclaratie (nvarchar) - optioneel: tekenreeks met definities voor afhankelijkheidstype gegevens voor de parameters in de queryparameter (zoals sp_executesql) gebruikt. 
* Parameter lijst met waarden - optioneel: door komma's gescheiden lijst met parameterwaarden (zoals sp_executesql).

De sp\_uitvoeren\_remote gebruikmaakt van de externe gegevensbron die beschikbaar zijn in de Aanroepparameters voor het uitvoeren van de gegeven T-SQL-instructie voor de externe databases. De referenties van de externe gegevensbron wordt gebruikt verbinding maken met de shardmap manager-database en de externe database.  

Voorbeeld: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a>Connectiviteit voor hulpmiddelen voor  

Normale SQL Server verbindingstekenreeksen gebruiken om verbinding maken met uw-toepassing, de hulpmiddelen voor uw BI en gegevens-integratie met de database met de tabeldefinities van uw externe. Zorg ervoor dat de SQL Server als gegevensbron voor een hulpmiddel in uw wordt ondersteund. Vervolgens verwijzing de querydatabase elastische zoals elke andere database van SQL Server verbonden met het hulpmiddel en externe tabellen gebruiken uit uw hulpmiddel of een toepassing alsof het lokale tabellen. 

## <a name="best-practices"></a>Aanbevolen procedures 

* Zorg ervoor dat de elastische query eindpunt database heeft toegang is verleend tot de database shardmap en alle shards tot en met de SQL-DB firewalls.  

* Valideer de of de gegevens verdeling gedefinieerd door de externe tabel afdwingen. Als u de verdeling van de werkelijke gegevens verschilt van de verdeling die is opgegeven in de tabeldefinitie van uw, kunnen uw query's onverwachte resultaten opleveren. 

* Elastische query momenteel geen uitgevoerd shard schrappen wanneer predicaten via de sleutel sharding wilt toestaan dat het veilig bepaalde shards uitsluiten van de verwerking.

* Elastische query werkt het best voor query's waar de meeste van de berekening op de shards kan worden uitgevoerd. U ontvangt meestal de beste queryprestaties met selectief filter predicaten die kan worden geëvalueerd op de shards of joins via de partities toetsen die kunnen worden uitgevoerd op een manier partition uitgelijnd op alle shards. Andere patronen query mogelijk moet u het laden van grote hoeveelheden gegevens uit de shards naar het hoofd knooppunt en goed

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
