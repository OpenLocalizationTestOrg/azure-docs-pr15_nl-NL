<properties
   pageTitle="Historische gegevens beheren in tijdelijke tabellen met bewaarbeleid | Microsoft Azure"
   description="Leer hoe u tijdelijke bewaarbeleid gebruiken om de historische gegevens onder uw beheer."
   services="sql-database"
   documentationCenter=""
   authors="bonova"
   manager="drasumic"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sql-database"
   ms.date="10/12/2016"
   ms.author="bonova"/>

#<a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Historische gegevens in tijdelijke tabellen met bewaarbeleid beheren

Tijdelijke tabellen kunnen vergroten databasegrootte meer dan gewone tabellen, vooral als u een langere periode historische gegevens bewaren. Bewaarbeleid voor historische gegevens is daarom een belangrijk aspect plannen en beheren van de levenscyclus van elke tijdelijke tabel. Tijdelijke tabellen in Azure SQL-Database hoort eenvoudig-en-klare bewaarbeleid om waarmee u uitvoeren van deze taak.

Tijdelijke geschiedenis bewaarbeleid kan worden geconfigureerd op het tabelniveau van afzonderlijke, zodat gebruikers kunnen maken van flexibele verouderende beleid. Tijdelijke bewaarbeleid toepassen is heel eenvoudig: er slechts één parameter worden ingesteld tijdens tabel maken of schema wijzigen.

Nadat u bewaarbeleid definiëren, start Azure SQL-Database regelmatig te controleren of er historische rijen die in aanmerking voor automatische gegevens wissen komen. Overeenkomende rijen kunt herkennen en hun verwijderen uit de geschiedenistabel optreden transparant, in de achtergrondtaak die is gepland en macro's starten met het systeem. Leeftijdsvoorwaarde voor de tabelrijen geschiedenis wordt gecontroleerd op basis van de kolom dat staat voor het einde van SYSTEM_TIME termijn. Als bewaarperiode, bijvoorbeeld is ingesteld op zes maanden, geeft tabelrijen in aanmerking komen voor opruimen voldoen aan de volgende voorwaarde:

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

In het bovenstaande voorbeeld uitgegaan we dat **ValidTo** kolom bij het einde van SYSTEM_TIME termijn hoort.

##<a name="how-to-configure-retention-policy"></a>Het bewaarbeleid configureren?

Voordat u een bewaarbeleid verplicht te maken voor een tijdelijke tabel configureert, Controleer eerst of tijdelijke historische bewaarbeleid ingeschakeld *op het databaseniveau van de is*.

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

Database vlag **is_temporal_history_retention_enabled** al dan niet standaard is ingesteld op aan, maar gebruikers deze kunnen wijzigen met de instructie ALTER DATABASE. Er is ook automatisch uit ingesteld na bewerking [wijst u in de tijd herstellen](sql-database-point-in-time-restore-portal.md) . Als u wilt inschakelen tijdelijke geschiedenis bewaarbeleid opruimen voor uw database, voer de volgende instructie:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [AZURE.IMPORTANT] U kunt een bewaarbeleid voor tijdelijke tabellen configureren evenzeer **is_temporal_history_retention_enabled** uitgeschakeld is, maar wordt automatisch opschonen voor verouderde rijen niet in dat geval wordt geactiveerd.


Bewaarbeleid is geconfigureerd tijdens het maken van de tabel door de waarde voor de parameter HISTORY_RETENTION_PERIOD te geven:

````
CREATE TABLE dbo.WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH
 (
     SYSTEM_VERSIONING = ON
     (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
     )
 );
````

Azure SQL-Database, kunt u opgeven dat bewaarperiode met verschillende tijdseenheden: dagen, weken, maanden en jaren. Als u HISTORY_RETENTION_PERIOD weglaat, wordt uitgegaan van ONEINDIG bewaarbeleid. U kunt ook ONEINDIG trefwoord expliciet gebruiken.

In sommige gevallen kunt u bewaarbeleid configureren na het maken van de tabel of als u wilt wijzigen, eerder geconfigureerde waarde. In dit geval Gebruik ALTER TABLE, instructie:

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [AZURE.IMPORTANT]  Instelling SYSTEM_VERSIONING op periode waarde bewaarbeleid *blijven niet behouden* uit. SYSTEM_VERSIONING instellen in op aan zonder HISTORY_RETENTION_PERIOD expliciet resultaten in de ONEINDIG bewaarperiode opgegeven.

Als u wilt bekijken huidige status van het bewaarbeleid definiëren, gebruikt u de volgende query die aan elkaar tijdelijke bewaarbeleid activering vlag op het databaseniveau van de met bewaarbeleid perioden voor afzonderlijke tabellen koppelt:

````
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName,  SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
````


##<a name="how-sql-database-deletes-aged-rows"></a>Hoe SQL-Database worden verwijderd van rijen jaar?

Het opruimproces is afhankelijk van de indexindeling van de geschiedenistabel. Het is belangrijk opvalt die *alleen geschiedenis tabellen met een gegroepeerde index (B-structuur of columnstore) eindige bewaarbeleid geconfigureerd kunnen hebben*. Een achtergrondtaak wordt gemaakt als u wilt opruimen verouderde gegevens uitvoeren voor alle tijdelijke tabellen met eindige bewaarperiode.
Opruimen logica voor de geclusterde index rowstore (B-structuur) Hiermee verwijdert u verouderde rij in kleinere stukken (maximaal 10 K) druk op de database-logboeken en IO-subsysteem minimaliseren. Hoewel opruimen logica vereiste B-structuur index, volgorde van verwijderingen voor de rijen die ouder zijn gebruikt dan bewaarperiode goed kan niet worden gegarandeerd. Daarom *hebben geen eventuele afhankelijkheid van de volgorde opruimen in uw toepassingen*.

De taak opruimen voor de gegroepeerde columnstore verwijdert u volledige [Rijgroepen](https://msdn.microsoft.com/library/gg492088.aspx) in één keer (meestal bevatten 1 miljoen van rijen, elke), namelijk zeer efficiënt, met name wanneer historische gegevens met een hoge snelheid wordt gegenereerd.

![Gegroepeerd columnstore bewaarbeleid](./media/sql-database-temporal-tables-retention-policy/cciretention.png)


Bronnen met uitstekende compressie en efficiënt bewaarbeleid opruimen wordt gegroepeerde columnstore index een perfecte keuze voor scenario's wanneer uw werkzaamheden snel grote hoeveelheden historische gegevens genereert. Patroon is typische voor intensief [transacties verwerking werkbelasting die tijdelijke tabellen gebruiken](https://msdn.microsoft.com/library/mt631669.aspx) voor het bijhouden en controle, trendanalyses of IoT gegevens opname.

##<a name="index-considerations"></a>Aandachtspunten voor de index

De taak opruimen voor tabellen met rowstore geclusterde index vereist index beginnen met de kolom met vinkjes het einde van SYSTEM_TIME termijn. Als de index niet bestaat, niet mogelijk eindige bewaarperiode configureren:

*Msg 13765, niveau 16, staat 1 <br> </br> op systeem versienummer tijdelijke tabel 'temporalstagetestdb.dbo.WebsiteUserInfo' eindige bewaarperiode instellen mislukt, omdat de geschiedenistabel 'temporalstagetestdb.dbo.WebsiteUserInfoHistory' bevat geen vereiste geclusterde index. Houd rekening met het maken van een gegroepeerd columnstore of B-structuur index beginnen met de kolom die overeenkomt met einde van SYSTEM_TIME periode, klik op de geschiedenistabel.*

Dit is belangrijk dat u ziet dat de tabel die standaard geschiedenis gemaakt door Azure SQL-Database al index, dat wil voldoen voor bewaarbeleid zeggen heeft gegroepeerde. Als u probeert te verwijderen die index voor een tabel met eindige bewaarperiode, mislukt de bewerking met het volgende foutbericht weergegeven:

*Msg 13766, niveau 16, staat 1 <br> </br> kan de geclusterde index 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' niet verwijderen omdat deze wordt gebruikt voor automatische opschoning van verouderde gegevens. U kunt instellen HISTORY_RETENTION_PERIOD naar ONEINDIG op de bijbehorende systeem versienummer tijdelijke tabel als u wilt neerzetten deze index.*

Opruimen op de index gegroepeerd columnstore werkt optimaal als historische rijen worden ingevoegd in de oplopende volgorde (geordend aan het einde van de kolom periode), dat wil altijd de hoofdletters/kleine letters zeggen wanneer de geschiedenistabel wordt gevuld uitsluitend op de manier SYSTEM_VERSIONIOING. Als u rijen in de geschiedenistabel niet gesorteerd op door het einde van de periode kolom (die mogelijk de hoofdletters/kleine letters als u bestaande historische gegevens gemigreerd), moet u opnieuw gegroepeerd columnstore index boven op B-structuur rowstore index goed om te bereiken optimale prestaties bestelde maken.

Vermijden bij het opnieuw opbouwen gegroepeerd columnstore index in de geschiedenistabel met de eindige bewaarperiode, omdat dit kan veranderen ordening in de Rijgroepen natuurlijk ingesteld door de systeem-versiebeheer-bewerking. Als u nodig hebt om een gegroepeerd columnstore index in de geschiedenistabel opnieuw te maken, doet u dat door opnieuw te maken boven op compatibele B-structuur index, het bevriezen van bestellen in de rowgroups die nodig zijn voor gewone gegevens wissen. Dezelfde aanpak moet worden genomen als u tijdelijke tabel maken met bestaande geschiedenistabel die is gegroepeerd kolomindex zonder gegarandeerd gegevensvolgorde:

````
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

Als eindige bewaarperiode is geconfigureerd voor de geschiedenistabel met de index gegroepeerd columnstore, kunt u geen extra niet-gegroepeerde B-structuur indexen maken van deze tabel:

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

Een poging om uit te voeren hierboven instructie, mislukt met de volgende fout:

*Msg 13772, niveau 16, staat 1 <br> </br> kan geen niet-gegroepeerde index maken voor een geschiedenistabel tijdelijke 'WebsiteUserInfoHistory' omdat er eindige bewaarperiode en gegroepeerd columnstore index die zijn gedefinieerd.*

##<a name="querying-tables-with-retention-policy"></a>Query's uitvoeren tabellen met bewaarbeleid

Alle query's op de tijdelijke tabel uitfilteren automatisch historische rijen overeenkomende eindige bewaarbeleid definiëren en om te voorkomen van onbekende en inconsistente resultaten, aangezien verouderde rijen kunnen worden verwijderd door de taak opruimen, klikt u *op een willekeurige plaats in de tijd en in willekeurige volgorde*.

De volgende afbeelding ziet de query-plannen voor een eenvoudige query:

````
SELECT * FROM dbo.WebsiteUserInfo FROM SYSTEM_TIME ALL;
````

De query-abonnement bevat extra filter is toegepast naar het einde van de kolom (ValidTo) van de periode in de operator 'Index scannen gegroepeerde' in de geschiedenistabel (gemarkeerd). In dit voorbeeld wordt ervan uitgegaan dat 1 maand bewaarperiode is ingesteld op WebsiteUserInfo tabel.

![Bewaarbeleid query filteren](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Echter als u de geschiedenistabel rechtstreeks query uitvoert, ziet u mogelijk rijen die ouder dan bewaarbeleid opgegeven periode, maar zonder een garantie voor herhaald queryresultaten zijn. De volgende afbeelding ziet abonnement van de query worden uitgevoerd voor de query in de geschiedenistabel zonder extra filters zijn toegepast:

![Query's uitvoeren geschiedenis zonder bewaarbeleid filter](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

Niet te vertrouwen uw bedrijfslogica op geschiedenistabel voorbij bewaarperiode lezen, zoals u inconsistent of onverwachte resultaten krijgt mogelijk. Het wordt aanbevolen tijdelijke query's met FOR SYSTEM_TIME-component gebruiken voor het analyseren van gegevens in tijdelijke tabellen.

##<a name="point-in-time-restore-considerations"></a>Wijs in de tijd herstellen overwegingen

Wanneer u een nieuwe database door het [herstellen van bestaande database aan een specifiek punt in tijd maken](sql-database-point-in-time-restore-portal.md), heeft dit tijdelijke bewaarbeleid op het databaseniveau van de is uitgeschakeld. (**is_temporal_history_retention_enabled** vlag ingesteld op OFF). Deze functie kunt u alle historische rijen na herstellen, zonder dat dit verouderde rijen zijn verwijderd voordat u toegang hebt tot deze query te bekijken. U kunt deze *historische gegevens buiten geconfigureerde bewaarperiode controleren*.

Stel dat een tijdelijke tabel een maand bewaarbeleid opgegeven periode heeft. Als de database is gemaakt in Premium Service laag, zou u kunnen maken database kopiëren met de status van de database omhoog naar 35 dagen terug in het verleden. Effectief zou waarmee u kunt analyseren historische rijen die maximaal 65 dagen oud rechtstreeks een query uitgevoerd op de geschiedenistabel.

Als u opruimen tijdelijke bewaarbeleid activeren wilt, voert u de volgende Transact-SQL-instructie na de komma in tijd herstellen:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

##<a name="next-steps"></a>Volgende stappen

Meer informatie over het gebruik van uitchecken tijdelijke tabellen in uw toepassingen [Aan de slag met tijdelijke tabellen in Azure SQL-Database](sql-database-temporal-tables.md).

Ga naar kanaal 9 om te horen van een [bestaande klanten tijdelijke implementatie Succesverhaal](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) en bekijk een [live tijdelijke demonstratie](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

Bekijk [MSDN-documentatie](https://msdn.microsoft.com/library/dn935015.aspx)voor gedetailleerde informatie over tijdelijke tabellen.
