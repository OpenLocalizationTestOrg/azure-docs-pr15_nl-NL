<properties
    pageTitle="Query over cloud-databases met een ander schema | Microsoft Azure"
    description="cross-databasequery's instellen op verticale partities"    
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

# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Query met betrekking tot cloud-databases met verschillende schema's (preview)

![Query met betrekking tot de tabellen in andere databases][1]

Verticaal partitioneren databases gebruiken verschillende groepen van tabellen in andere databases. Dat betekent dat het schema verschillende gegevens weergeven op verschillende databases. Alle tabellen voor voorraad zijn bijvoorbeeld op één database terwijl u alle financieel-gerelateerde tabellen op een tweede database. 

## <a name="prerequisites"></a>Vereisten voor

* De gebruiker moet beschikken over machtigingen voor de externe gegevensbron wijzigen. Met deze machtiging is opgenomen in de machtiging DATABASE wijzigen.
* EEN externe gegevensbron wijzigen machtigingen nodig zijn om te verwijzen naar de onderliggende gegevensbron.

## <a name="overview"></a>Overzicht

**Opmerking**: in tegenstelling tot met horizontaal partitioneren, deze DDL-instructies niet afhankelijk zijn over het definiëren van een gegevenslaag met een kaart shard tot en met de bibliotheek van de client elastische database.

1. [BASISPAGINA SLEUTEL MAKEN](https://msdn.microsoft.com/library/ms174382.aspx)
2. [MAKEN VAN DATABASE BEPERKT REFERENTIE](https://msdn.microsoft.com/library/mt270260.aspx)
3. [EXTERNE GEGEVENSBRON MAKEN](https://msdn.microsoft.com/library/dn935022.aspx)
4. [EXTERNE TABEL MAKEN](https://msdn.microsoft.com/library/dn935021.aspx) 


## <a name="create-database-scoped-master-key-and-credentials"></a>Database beperkt outmodel sleutel en referenties maken 

De referentie wordt gebruikt door de elastische query verbinding maakt met uw externe databases.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
**Opmerking**    Zorg ervoor dat de *<username>* omvat geen *"@servername"* achtervoegsel. 

## <a name="create-external-data-sources"></a>Externe gegevensbronnen maken

Syntaxis:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

**Belangrijke**   De parameter TYPE moet zijn ingesteld op **RDBMS**. 

### <a name="example"></a>Voorbeeld 

Het volgende voorbeeld ziet u het gebruik van de instructie CREATE voor externe gegevensbronnen. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 
 
De lijst met huidige externe gegevensbronnen ophalen: 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Externe tabellen 

Syntaxis:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 
    
    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Voorbeeld  

    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 

Het volgende voorbeeld ziet hoe u kunt de lijst met externe tabellen ophalen uit de huidige database: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Opmerkingen

De tabelsyntaxis van de bestaande externe om te definiëren externe tabellen met externe gegevensbronnen van het type RDBMS uitbreiden elastische query De tabeldefinitie van een externe voor verticale partities worden de volgende aspecten: 

* **Schema**: de externe tabel DDL definieert een schema dat uw query's kunnen gebruiken. Het schema die beschikbaar zijn in de tabeldefinitie van uw externe moet overeenkomen met het schema van de tabellen in de externe database waarin de werkelijke gegevens is opgeslagen. 

* **Externe databaseverwijzing**: de externe tabel DDL naar een externe gegevensbron verwijst. De externe gegevensbron geeft de logische servernaam en de naam van de database van de externe database waarin de werkelijke tabelgegevens is opgeslagen. 

Een externe gegevensbron gebruiken zoals beschreven in de vorige sectie, is de syntaxis van de externe tabellen te maken als volgt: 

De component DATA_SOURCE Hiermee definieert u de externe gegevensbron (dat wil zeggen de externe database voor het geval de verticale partities) die wordt gebruikt voor de externe tabel.  

De clausules SCHEMA_NAME en Ontwerpweergavesnelkoppelingen kunnen u de definitie van de externe tabel aan een tabel in een ander schema op de externe database of aan een tabel met een andere naam, respectievelijk toewijzen. Dit is handig als u wilt definiëren van een externe tabel naar de weergave van een catalogus of DMV op de externe database – of andere situaties waarin de naam van de externe tabel wordt al gebruikt lokaal.  

De volgende DDL-instructie worden de tabeldefinitie van een bestaande externe uit de lokale catalogus. Deze heeft geen gevolgen voor de externe database. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Machtigingen voor externe tabel maken/DECORATIEVE**: een externe gegevensbron wijzigen machtigingen nodig zijn voor externe tabel DDL dat ook nodig is om te verwijzen naar de onderliggende gegevensbron.  

## <a name="security-considerations"></a>Veiligheidsoverwegingen
Gebruikers met toegang tot de externe tabel toegang automatisch tot de onderliggende externe tabellen onder de referenties die zijn opgegeven in de definitie van de externe gegevensbron. U moet de toegang tot de externe tabel zorgvuldig beheren om te voorkomen ongewenste misbruik van bevoegdheden tot en met de referenties van de externe gegevensbron. Normale SQL-machtigingen kunnen worden gebruikt om te kennen of toegang tot een externe tabel INTREKKEN net alsof het een normale tabel.  


## <a name="example-querying-vertically-partitioned-databases"></a>Voorbeeld: query's uitvoeren verticaal databases partitioneren 

Een join drie manieren tussen de twee lokale tabellen voor orders en regels en de externe tabel kunt u de volgende query uitvoeren voor klanten. Dit is een voorbeeld van de verwijzing gegevens use-case voor elastische query: 

    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100


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

Normale tekenreeksen voor SQL Server kunt u de hulpmiddelen voor de integratie van BI en gegevens verbinden met databases in de SQL-databaseserver met elastische query ingeschakeld en externe tabellen die zijn gedefinieerd. Zorg ervoor dat de SQL Server als gegevensbron voor een hulpmiddel in uw wordt ondersteund. Vervolgens kunt u verwijzen naar de querydatabase elastische en de externe tabellen net zoals elke andere SQL Server-database die u met uw hulpmiddel verbinden wilt. 

## <a name="best-practices"></a>Aanbevolen procedures 
 
* Zorg ervoor dat de elastische query eindpunt database heeft toegang is verleend tot de externe database doordat toegang voor Azure Services in de configuratie van de firewall SQL DB. Ook voor zorgen dat de opgegeven in de definitie van externe bron referentie zich bij de externe database aanmelden kunt en de machtigingen voor toegang tot de externe tabel heeft.  

* Elastische query werkt het best voor query's waar de meeste van de berekening kan worden uitgevoerd op de externe database. U wordt meestal de beste queryprestaties met selectief filter predicaten die op de externe databases of joins die volledig kunnen worden uitgevoerd op de externe database kan worden geëvalueerd. Andere patronen query mogelijk moet u het laden van grote hoeveelheden gegevens uit de externe database en goed. 


## <a name="next-steps"></a>Volgende stappen

Als u wilt query horizontaal gepartitioneerde databases (ook wel bekend als een laptopgeheugen databases), raadpleegt u [query's via een laptopgeheugen cloud-databases (horizontaal partitioneren)](sql-database-elastic-query-horizontal-partitioning.md).

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
