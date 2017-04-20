<properties 
    pageTitle="XEvent bellen Buffer code voor SQL-Database | Microsoft Azure" 
    description="Biedt een steekproef van Transact-SQL-code die gemakkelijk en snel door gebruik van het doel bellen Buffer in Azure SQL-Database is gemaakt." 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jhubbard" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="genemi"/>


# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>Bellen Buffer doel code voor uitgebreide gebeurtenissen in SQL-Database

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Wilt u een voorbeeld van de volledige code voor het gemakkelijkst snelle aan vastleggen en rapport informatie voor een uitgebreide gebeurtenis tijdens een test. De eenvoudigste doel voor uitgebreide gebeurtenisgegevens is het [doel van de bellen Buffer](http://msdn.microsoft.com/library/ff878182.aspx).


In dit onderwerp vindt een steekproef Transact-SQL-code die:


1. Hiermee maakt u een tabel met gegevens om te laten zien met.

2. Hiermee maakt u een sessie voor een bestaande gebeurtenis voor uitgebreide, namelijk **sqlserver.sql_statement_starting**.
    - De gebeurtenis is beperkt tot SQL-instructies met een bepaalde tekenreeks van de Update: **instructie als '% UPDATE tabEmployee %'**.
    - De uitvoer van de gebeurtenis verzenden naar een doel van het type bellen Buffer, namelijk **package0.ring_buffer**kiest.

3. Hiermee start u de gebeurtenis-sessie.

4. Problemen met een aantal eenvoudige UPDATE van de SQL-instructies.

5. Problemen met een SQL-selecteren om op te halen gebeurtenis uitvoer uit de Buffer bellen.
    - **sys.dm_xe_database_session_targets** en andere weergaven dynamische management (DMVs) zijn gekoppeld.

6. De gebeurtenis-sessie stopt.

7. Het doel van de bellen Buffer, om de bronnen vrij worden.

8. Worden de sessie gebeurtenis en de tabel demo.


## <a name="prerequisites"></a>Vereisten voor


- Een Azure-account en abonnementen. U kunt zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).


- Een database die kunt u een tabel in.
 - Desgewenst kunt u [een **AdventureWorksLT** demonstratie-database maken](sql-database-get-started.md) in minuten.


- SQL Server Management Studio (ssms.exe), in het ideale geval de als meest recente maandelijkse update versie. U kunt de meest recente ssms.exe uit downloaden:
 - Onderwerp van [SQL Server Management Studio downloaden](http://msdn.microsoft.com/library/mt238290.aspx).
 - [Een directe koppeling naar het downloaden.](http://go.microsoft.com/fwlink/?linkid=616025)


## <a name="code-sample"></a>Codevoorbeeld


Met zeer kleine wijziging, kan het volgende voorbeeld bellen Buffer worden uitgevoerd op Azure SQL-Database of Microsoft SQL Server. Het verschil is de aanwezigheid van het knooppunt '_database' naam sommige dynamische management weergaven (DMVs), gebruikt in de FROM-component in stap 5. Bijvoorbeeld:

- sys.dm_xe**_database**_session_targets
- sys.dm_xe_session_targets


&nbsp;


```
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;


## <a name="ring-buffer-contents"></a>Bellen Buffer inhoud


We ssms.exe gebruikt om uit te voeren in het voorbeeld.


Als u wilt de resultaten bekijken, klik op we de cel onder de kop van kolom **target_data_XML**.

Klik in het resultatendeelvenster geklikt we de cel onder de kop van kolom **target_data_XML**. Klikt u op een ander tabblad van het bestand in ssms.exe waarin de inhoud van de resultaatcel werd weergegeven, als XML hebt gemaakt.


De uitvoer wordt weergegeven in het volgende blok. Het lijkt erop lang, maar het is slechts twee **<event>** elementen.


&nbsp;


```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a>Release-bronnen die door uw Buffer bellen


Wanneer u klaar bent met uw Buffer bellen, kunt u deze verwijderen en los van de uitgifte een **wijzigen** zoals de volgende bronnen:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


De definitie van de gebeurtenis-sessie is bijgewerkt, maar niet verwijderd. U kunt een ander exemplaar van de bellen Buffer later toevoegen aan de sessie gebeurtenis:


```
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a>Meer informatie


Het primaire-onderwerp voor uitgebreide evenementen Azure SQL-Database is:


- [Overwegingen voor uitgebreide gebeurtenissen in SQL-Database](sql-database-xevent-db-diff-from-svr.md), die verschilt van bepaalde aspecten van uitgebreide gebeurtenissen die tussen Azure SQL-Database versus Microsoft SQL Server verschillen.


Andere onderwerpen van de steekproef code voor uitgebreide gebeurtenissen zijn beschikbaar via de volgende koppelingen. U moet een voorbeeld om te zien of Microsoft SQL Server versus Azure SQL-Database is bedoeld voor de steekproef echter regelmatig controleren. Vervolgens kunt u bepalen of kleine wijzigingen nodig zijn om uit te voeren in het voorbeeld.


- Voorbeeld van de code voor Azure SQL-Database: [gebeurtenisbestand doel code voor uitgebreide gebeurtenissen in SQL-Database](sql-database-xevent-code-event-file.md)


<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
