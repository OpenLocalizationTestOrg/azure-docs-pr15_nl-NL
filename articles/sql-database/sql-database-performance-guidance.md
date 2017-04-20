<properties
    pageTitle="Azure SQL-Database en prestaties voor één databases | Microsoft Azure"
    description="In dit artikel kunt u bepalen welke servicelaag kiezen voor uw toepassing. Dit wordt ook aanbevolen manieren om uw toepassing optimaal te profiteren van Azure SQL-Database af te stellen."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/13/2016"
    ms.author="carlrab" />

# <a name="azure-sql-database-and-performance-for-single-databases"></a>Azure SQL-Database en prestaties voor één databases

Azure SQL-Database biedt drie [Servicelagen](sql-database-service-tiers.md): Basic, Standard en Premium. Elke servicelaag geïsoleerd strikt de resources dat uw SQL-database gebruiken kunt en overzichtelijk prestaties voor die serviceniveau garandeert. In dit artikel bieden we richtlijnen die u kiest u de servicelaag voor uw toepassing kan helpen. Wordt ook dat u de toepassing van Azure SQL-Database voor een optimaal kunt afstemmen manieren besproken.

>[AZURE.NOTE] In dit artikel ligt de nadruk op prestaties richtlijnen voor één databases in Azure SQL-Database. Zie [Aandachtspunten voor de prijs en prestaties voor elastische database van toepassingen](sql-database-elastic-pool-guidance.md)voor prestaties instructies betrekking hebben op elastische database van toepassingen. Merk echter die u kunt veel van de afstelpagina aanbevelingen in dit artikel van toepassing op databases in een elastische database-toepassingen en vergelijkbare prestaties voordelen.

Hierna ziet u de drie Azure SQL Database-Servicelagen die u kunt kiezen uit (prestaties worden gemeten in database doorvoer eenheden of [DTUs](sql-database-what-is-a-dtu.md):

- **Eenvoudige**. De eenvoudige service laag aanbiedingen goede prestaties voorspelbaarheid voor elke database, uur in uur. In een eenvoudige database ondersteuning voldoende bronnen goede prestaties in een kleine database die geen meerdere gelijktijdige aanvragen.
- **Standaard**. De laag Standard service biedt verbeterde prestaties voorspelbaarheid en verheft de balk voor databases met meerdere gelijktijdige aanvragen, zoals werkgroep en webtoepassingen. Als u een database van de laag Standard service kiest, u kunt het formaat van de databasetoepassing op basis van overzichtelijk prestaties minuut in minuut.
- **Premium**. De Premium-servicelaag biedt overzichtelijk prestaties, tweede via Tweede, voor elke Premium-database. Als u de laag Premium-service kiest, kunt u het formaat van de databasetoepassing op basis van de belasting voor die database. Het abonnement wordt verwijderd gevallen welke prestaties afwijking leiden kleine query's tot kan kunt langer duren dan verwacht in latentie gevoelige bewerkingen. Dit model kunt u de ontwikkeling en product validatie maal voor toepassingen die u wilt aanbrengen sterke beweringen over piek resourcebehoeften, de variantie van de prestaties of Querylatentie enorm vereenvoudigen.

Op elke servicelaag instellen u het prestatieniveau voor, zodat u hebt de flexibiliteit om te betalen alleen voor de capaciteit die u nodig hebt. Als de wijzigingen van de werkbelasting kunt u [capaciteit aanpassen](sql-database-scale-up.md), omhoog of omlaag. Als uw database werkbelasting tijdens het winkelwagentje season terug naar school hoog is, kan u bijvoorbeeld het prestatieniveau voor de database vergroten voor een bepaald tijdstip, juli tot en met September. Als uw season piek eindigt, kunt u deze verkleinen. U kunt het minimaliseren wat u met het optimaliseren van uw omgeving cloud naar de periodieke variatie van uw bedrijf betaalt. Dit model werkt ook voor software product release maal. Een testteam mogelijk capaciteit toewijzen terwijl deze wordt uitgevoerd testen en laat deze capaciteit los wanneer ze klaar bent met het testen. In een model van de aanvraag capaciteit betalen u capaciteit als u deze nodig hebt en besparen op de toegewezen resources die u mogelijk zelden gebruikt.

## <a name="why-service-tiers"></a>Waarom lagen voor service?

Hoewel de databasewerklast van elke verschillen kan, is het doel van de Servicelagen te leveren prestaties voorspelbaarheid op verschillende prestatieniveaus. Klanten met resourcevereisten grootschalige database kunnen werken in een meer speciale omgeving.

### <a name="common-service-tier-use-cases"></a>Algemene servicelaag gevallen gebruiken

#### <a name="basic"></a>Eenvoudige

- **U gaat voor het eerst met Azure SQL-Database**. Toepassingen die in de ontwikkelingsfase bevindt vaak zijn hoeft geen krachtige niveaus. Eenvoudige databases zijn een ideale omgeving voor de databaseontwikkeling, tegen een lage prijs.
- **U hebt een database maken met een enkele gebruiker**. Toepassingen die voor het koppelen van één gebruiker met een database is het meestal hebben niet hoge gelijktijdigheid en prestaties vereisten. Deze toepassingen zijn kandidaten voor de servicelaag eenvoudige.

#### <a name="standard"></a>Standaard

- **De database heeft meerdere gelijktijdige aanvragen**. Toepassingen die meer dan één gebruiker service per keer meestal moeten hogere prestatieniveaus. Bijvoorbeeld zijn websites die u regelmatige verkeer of afdelings-toepassingen waarvoor meer bronnen goede kandidaten voor de laag Standard service.

#### <a name="premium"></a>Premium

Premium service laag gebruik meestal bestaan uit een of meer van deze kenmerken:

- **Hoge piek laden**. Een toepassing die is vereist een groot aantal CPU, geheugen of invoer/uitvoer (I/O) om de bewerkingen te voltooien is een speciale, krachtige niveau vereist. Een databasebewerking bekende verschillende CPU cores langere tijd in beslag neemt is bijvoorbeeld een candidate voor de laag Premium-service.
- **Veel gelijktijdige aanvragen**. Sommige databasetoepassingen serviceaanvragen veel gelijktijdige, bijvoorbeeld wanneer een intensief netwerkverkeer voor een website die bevat. Basisfilters en Standard Servicelagen Beperk het aantal gelijktijdige aanvragen per database. Toepassingen die meerdere verbindingen vereisen nodig hebt om de reserveringsgrootte van een juiste u omgaat met het maximum aantal benodigde aanvragen te kiezen.
- **Lage latentie**. Sommige toepassingen moeten een antwoord van de database te garanderen in minimale tijd. Als een specifieke opgeslagen procedure wordt aangeroepen als onderdeel van een uitgebreidere bewerking klanten, bent u mogelijk een vereiste dat een return van dat gesprek hebt in niet meer dan 20 milliseconden, 99 procent van de tijd. Dit soort toepassing voordelen van de Premium-servicelaag, om ervoor te zorgen dat de vereiste rekenkracht beschikbaar is.

Het serviceniveau van de die u nodig voor uw SQL-database hebt, is afhankelijk van de vereisten voor het laden van piek voor elke resourcedimensie. Sommige toepassingen een eenvoudig hoeveelheid één resource gebruiken, maar wel aanzienlijk vereisten voor andere bronnen.

## <a name="service-tier-capabilities-and-limits"></a>Service laag mogelijkheden en -limieten
Elk niveau van laag en prestaties service is gekoppeld aan andere limieten en prestatiekenmerken. Deze tabel worden deze kenmerken voor één database beschreven.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

De volgende secties bevatten meer informatie over het weergeven van gebruiken die zijn gerelateerd aan deze limieten.

### <a name="maximum-in-memory-oltp-storage"></a>Maximum aantal In het geheugen OLTP opslag

U kunt de weergave **sys.dm_db_resource_stats** gebruiken om de van het gebruik van de opslagruimte Azure In het geheugen te houden. Zie voor meer informatie over het controleren van [Beeldscherm In het geheugen OLTP opslag](sql-database-in-memory-oltp-monitoring.md).

>[AZURE.NOTE] Op dit moment wordt Azure In het geheugen online transactie processing (OLTP) preview alleen ondersteund voor één databases. U kunt deze niet gebruiken in databases in elastische database-toepassingen.

### <a name="maximum-concurrent-requests"></a>Maximum aantal gelijktijdige aanvragen

Als wilt zien van het aantal gelijktijdige aanvragen, voert u deze Transact-SQL-query in uw SQL-database:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Als u wilt analyseren de werklast van een lokale SQL Server-database, wijzig deze query om te filteren op het specifieke database die u wilt analyseren. Als u een lokale database met de naam MijnDatabase hebt, wordt deze Transact-SQL-query bijvoorbeeld het aantal gelijktijdige aanvragen in die database:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Dit is alleen een momentopname op één punt in tijd. Als u een beter begrip van de werkbelasting en gelijktijdige aanvraag vereisten, moet u voor het verzamelen van veel voorbeelden na verloop van tijd.

### <a name="maximum-concurrent-logins"></a>Maximale gelijktijdige aanmeldingen

U kunt uw gebruikers- en -toepassing patronen als u een idee van de frequentie van aanmeldingen wilt analyseren. U kunt ook echte laadtijd uitvoeren in een testomgeving om ervoor te zorgen dat u bent niet kunt u door dit of andere limieten die wordt besproken in dit artikel. Er is een enkele query of dynamische management weergave (DMV) die kan worden gevuld met u gelijktijdige login telt of geschiedenis niet.

Als meerdere klanten dezelfde verbindingsreeks gebruiken, wordt elke aanmelding geverifieerd door de service. Als u 10 gebruikers is tegelijkertijd verbinding maken met een database met behulp van dezelfde gebruikersnaam en wachtwoord, is er zou 10 gelijktijdige aanmeldingen. Deze beperking geldt alleen voor de duur van de aanmelding en verificatie. Als dezelfde 10 gebruikers verbinding met de database opeenvolging, zou het aantal gelijktijdige aanmeldingen nooit groter is dan 1.

>[AZURE.NOTE] Deze limiet is momenteel niet van toepassing op databases in elastische database-toepassingen.

### <a name="maximum-sessions"></a>Maximumaantal sessies

Als u wilt weten hoeveel huidige actieve sessies, deze Transact-SQL-query worden uitgevoerd op uw SQL-database:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Als u een lokale SQL Server-werkbelasting analyseren bent, wijzigt u de query kunt richten op een specifieke database. Deze query kunt u bepalen mogelijke sessie behoeften voor de database bent u van plan verplaatsen naar Azure SQL-Database.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Klik nogmaals deze query's geteld punt-in-tijd. Als u meerdere voorbeelden na verloop van tijd verzamelt, hebt u het beste lidmaatschap van uw sessie gebruiken.

U kunt de historische statistieken over sessies opvragen voor analyse van de SQL-Database. Query **sys.resource_stats**en gebruikt u de kolom **active_session_count** . Zie de volgende sectie voor meer informatie over het gebruik van deze weergave.

## <a name="monitor-resource-use"></a>Bronnengebruik
Twee weergaven kunt u resource wordt gebruikt voor een SQL-database ten opzichte van de servicelaag controleren:

- [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
- [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
U kunt de weergave [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) gebruiken in elke SQL-database. De weergave **sys.dm_db_resource_stats** ziet u recente gegevens ten opzichte van de servicelaag resource gebruiken. Gemiddelde percentages voor CPU, gegevens I/O log schrijft en geheugen elke 15 seconden worden vastgelegd en worden onderhouden voor 1 uur staan.

Omdat deze weergave een meer gedetailleerde beschrijving van de resource gebruiken bevat, gebruikt u **sys.dm_db_resource_stats** eerste voor een analyse van de huidige status of het oplossen van problemen. Deze query bevat bijvoorbeeld de gemiddelde- en maximumwaarden bronnengebruik voor de huidige database via het afgelopen uur:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data I/O in percent',
        MAX(avg_data_io_percent) AS 'Maximum data I/O in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Zie de voorbeelden in [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)voor andere query's.

### <a name="sysresourcestats"></a>sys.resource_stats

De weergave [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) in **de hoofddatabase** biedt extra informatie kunt u de prestaties van uw SQL-database op het niveau van laag en prestaties specifieke service controleren. De gegevens worden opgevraagd elke 5 minuten en ongeveer 35 dagen blijft behouden. Deze weergave is handig voor een langere historische analyse van het gebruik van resources in uw SQL-database.

Het volgende diagram ziet u de CPU bronnengebruik voor een Premium-database met het niveau van de prestaties P2 voor elk uur in een week. Deze grafiek wordt gestart op een maandag, zien 5 werkdagen en klikt u vervolgens een weekend, wanneer u veel minder gebeurt er in de toepassing.

![Bronnengebruik voor SQL-database](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Van de gegevens, heeft deze database momenteel een piek CPU-belasting van iets meer dan 50 procent CPU-gebruik ten opzichte van het prestatieniveau P2 (twaalf uur 's middags dinsdag). Als CPU de dominante factor in van de toepassing resource profiel is, klikt u vervolgens het misschien dat P2 de juiste prestatieniveau om ervoor te zorgen is dat de werklast altijd past. Als u een toepassing na verloop van tijd toenemen verwacht, is een goed idee om een extra bron buffer hebben zodat de toepassing niet ooit de limiet van prestatieniveau hebt bereikt. Als u het prestatieniveau van de vergroot, kunt u helpen vermijden klant-zichtbare fouten die optreden kunnen wanneer een database geen voldoende kracht en het verwerken van aanvragen effectief, met name in latentie gevoelige omgevingen. Een voorbeeld is een database die ondersteuning biedt voor een toepassing die Hiermee tekent u webpagina's op basis van de resultaten van de database-oproepen.

Houd er rekening mee dat andere toepassingstypen dezelfde grafiek anders mogelijk interpreteren. Bijvoorbeeld als een toepassing probeert te verwerken salarisgegevens elke dag en dezelfde grafiek heeft, kunt dit soort "batchtaak" model doen prima op een niveau van de prestaties P1. Het P1 prestatieniveau heeft 100 DTUs vergeleken met 200 DTUs op het niveau van de prestaties P2. Het P1 prestatieniveau biedt helft van de prestaties van het niveau van de prestaties P2. 50 procent van CPU-gebruik in P2 zo is, is gelijk aan 100 procent CPU-gebruik in P1. Als de toepassing geen time-out, maakt het niet uit als u een taak duurt 2 uur of 2,5 uur om te voltooien, als deze vandaag wordt uitgevoerd. Een toepassing in deze categorie kunt waarschijnlijk een P1 prestatieniveau gebruiken. U kunt profiteren van het feit dat er perioden overdag als bronnengebruik lager, is dat een 'groot piek"in een van de holten verderop in de dag passen mogelijk. Het P1 prestatieniveau mogelijk goed voor dit soort van toepassing (en geld besparen), zo lang maken als de taken kunnen worden voltooid op tijd elke dag.

Azure SQL-Database gegarandeerd verbruikt resourcegegevens voor elke actieve database in de weergave **sys.resource_stats** van **de hoofddatabase in elke server** . De gegevens in de tabel wordt samengevoegd voor 5 minuten. Met de lagen Basic, Standard en Premium-service kan de gegevens wordt weergegeven in de tabel, zodat deze gegevens gebruiksvriendelijker voor historische analyse in plaats van in de buurt van de realtime analyse zijn meer dan 5 minuten duren. Query **sys.resource_stats** de recente geschiedenis van een database bekijken en wilt controleren of de reservering die u hebt gekozen de prestaties afgeleverd u de gewenste weergave als dat nodig is.

>[AZURE.NOTE] U moet worden verbonden met **de hoofddatabase van de logische SQL database-server om query's in **sys.resource_stats** in de volgende voorbeelden** .

In dit voorbeeld ziet u hoe de gegevens in deze weergave worden weergegeven:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![De weergave van de catalogus sys.resource_stats](./media/sql-database-performance-guidance/sys_resource_stats.png)

Het volgende voorbeeld ziet u verschillende manieren waarmee u de weergave van de catalogus **sys.resource_stats kunt** voor informatie over het gebruik van resources in uw SQL-database:

1. De laatste week resource nagaan gebruiken voor de database userdb1, kunt u deze query uitvoeren:

        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;

2. Als u wilt evalueren hoe goed uw werkzaamheden past het prestatieniveau van de, moet u inzoomen op elke aspecten van de doelstellingen van de resource: CPU, lezen, schrijven, aantal werknemers en aantal sessies. Hier ziet u een gewijzigde query **sys.resource_stats** gebruiken om de gemiddelde en hoogste waarde van de doelstellingen van deze resource:

        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data I/O use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data I/O use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

3. Met deze informatie over de gemiddelde en hoogste waarde voor elke resource-meetwaarde, kunt u beoordelen hoe goed uw werkzaamheden past in het prestatieniveau die u hebt gekozen. Meestal, geef gemiddelden van **sys.resource_stats** u een goede basislijn gebruik ten opzichte van de doelgrootte. Uw primaire maateenheden golfclub moet worden. Voor een voorbeeld mogelijk gebruikt u de laag Standard service met S2 prestatieniveau. Het gemiddelde percentages gebruiken voor CPU en i/o-lezen en schrijven zijn onder 40 procent, het gemiddelde aantal werknemers lager is dan 50 en het gemiddelde aantal sessies lager is dan 200. Uw werkzaamheden mogelijk passen in het prestatieniveau S1. Het is eenvoudig om te zien of de database in de limieten werknemer en sessie past. Als u wilt zien of een database in een lager prestatieniveau met betrekking tot CPU past, leest en schrijft, het getal DTU van de onderste prestatieniveau door het nummer DTU van uw huidige prestatieniveau delen en het resultaat vervolgens vermenigvuldigd met 100:

    * *S1 DTU / S2 DTU* 100 = 20 / 50* 100 = 40 **

    Het resultaat is het verschil relatieve prestaties tussen de twee prestaties percentage. Als het gebruik van de resource niet groter is dan dit bedrag, worden uw werkzaamheden mogelijk passen in het onderste prestatieniveau. Echter, moet u kijkt u naar alle bereiken van de waarden voor het gebruik van resources en bepalen, met percentage, hoe vaak de databasewerklast van uw in het onderste prestatieniveau zou past. De volgende query uitvoer het passend maken percentage per resourcedimensie, op basis van de limiet van 40 procent die we in dit voorbeeld berekend:

        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Op basis van uw database service niveau doelstelling (TRAGE), kunt u bepalen of uw werkzaamheden in het onderste prestatieniveau past. Als uw database werkbelasting TRAGE 99,9 procent is en de voorgaande query waarden groter zijn dan 99,9 procent voor alle drie resourcedimensies geretourneerd, wordt uw werkzaamheden waarschijnlijk past in het onderste prestatieniveau.

    Bekijken via de passend maken percentage ook biedt u inzicht in of u verplaatsen naar het volgende hoger prestatieniveau om te voldoen aan uw TRAGE hebt. Userdb1 bevat bijvoorbeeld de volgende CPU-gebruik voor de laatste week:

  	| Gemiddelde CPU procent | Maximumpercentage CPU |
  	|---|---|
  	| 24,5 | 100,00 |

    De gemiddelde CPU gaat over een kwartaal van de limiet van het prestatieniveau, waardoor zijn voor het prestatieniveau van de database geschikt zou. Maar de maximumwaarde ziet u dat de database de limiet voor het prestatieniveau bereikt. Moet u verplaatsen naar de volgende hoger prestatieniveau? U moet kijken hoe vaak uw bereikt werkbelasting 100 procent en vergelijk deze met de databasewerklast van uw TRAGE.

        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent’
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data I/O fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());

    Als deze query geeft als resultaat een waarde wordt minder dan 99,9 procent voor een van de afmetingen drie resource zinvol beide te verplaatsen naar het volgende hoger prestatieniveau of technieken afstemming van toepassingen gebruiken om minder van de belasting van de SQL-database.

4. Deze oefening acht ook uw grotere verwachte werkbelasting in de toekomst.

## <a name="tune-your-application"></a>Uw toepassing afstemmen

In traditionele lokale SQL Server, worden het proces van het plannen van de eerste capaciteit vaak gescheiden van het proces van het uitvoeren van een toepassing in productie. Licenties hardware en product eerst worden gekocht en prestaties optimaliseren daarna is voltooid. Wanneer u Azure SQL-Database gebruikt, is een goed idee om het proces van een toepassing gestart en deze afstemmen interweave. Met het model van betaalt voor capaciteit op aanvraag, kunt u uw toepassing voor het gebruik van de minimale resources nodig nu in plaats van op hardware op basis van de suggesties van toekomstige groei-plannen voor een toepassing, die vaak onjuist zijn overprovisioning afstemmen. Sommige klanten mogelijk niet om een toepassing af te stellen, om te selecteren in plaats daarvan overprovision hardware resources. Deze methode mogelijk een goed idee als u niet wilt dat een belangrijke toepassing een bezet periode aan te passen. Maar optimaliseren van een toepassing kunt minimaliseren resourcevereisten en maandelijkse rekeningen te verlagen wanneer u de Servicelagen in Azure SQL-Database gebruikt.

### <a name="application-characteristics"></a>Kenmerken van toepassingen

Hoewel Azure SQL Database-Servicelagen zijn ontworpen voor het verbeteren van prestaties stabiliteit en voorspelbaarheid voor een toepassing, kunnen u uw toepassing beter profiteren van de bronnen op een prestatieniveau afstemmen door enkele aanbevolen procedures helpen. Hoewel veel toepassingen aanzienlijke prestatieverbeteringen kennen gewoon door over te schakelen naar een hoger prestatieniveau of servicelaag van de, sommige toepassingen nodig extra worden afgestemd om te profiteren van een hoger niveau van de service. Voor betere prestaties, kunt u bovendien aanvullende toepassing optimaliseren voor toepassingen die deze kenmerken hebben:

- **Toepassingen die vertragingen vanwege "chatty" gedrag hebben**. Chatty toepassingen maken veel gegevens access bewerkingen die gevoelige naar netwerklatentie zijn. Mogelijk moet u dit soort toepassingen Beperk het aantal gegevensbewerkingen van access met de SQL-database te wijzigen. U kunt bijvoorbeeld toepassingsprestaties verbeteren met behulp van technieken zoals batchen ad-hoc-query's of de query's verplaatsen naar opgeslagen procedures. Zie [Batch query's](#batch-queries)voor meer informatie.
- **Databases met een intensief werkbelasting die door een hele één computer kan niet worden ondersteund**. Databases die groter is dan de bronnen van het hoogste niveau van de Premium-prestaties nuttig schaalbaarheid de werklast. Zie [meerdere databases sharding](#cross-database-sharding) en [functionele partitioneren](#functional-partitioning)voor meer informatie.
- **Toepassingen die niet-optimale query's hebt**. Toepassingen, met name de in de data access-laag, die slecht query's afgestemd niet nuttig een hoger prestatieniveau. Dit geldt ook voor query's die u niet over een WHERE-component, indexen ontbrekende of verouderde statistieken. Deze toepassingen profiteren van de standaardquery prestaties optimaliseren technieken. Zie [ontbrekende indexen](#missing-indexes) en [Query optimaliseren en coderingstips](#query-tuning-and-hinting)voor meer informatie.
- **Toepassingen die niet-optimale gegevens hebben toegang tot ontwerpen**. Toepassingen die zijn er inherent gegevens access gelijktijdigheid problemen, bijvoorbeeld deadlocking, mogelijk niet goed een hoger prestatieniveau. Houd rekening met interactie ten opzichte van de Azure SQL-Database wordt afgetrokken van in de cache gegevens aan de clientzijde met de Caching van Azure-service of een andere in de cache technologie. Zie [caching van toepassing laag](#application-tier-caching).

## <a name="tuning-techniques"></a>Technieken optimaliseren
In dit gedeelte kijken we naar enkele technieken die u gebruiken kunt om af te stellen Azure SQL-Database te krijgen van de beste prestaties voor uw toepassing op de laagste mogelijke prestatieniveau worden uitgevoerd. Enkele van de volgende manieren traditionele SQL Server optimaliseren aanbevolen procedures overeenkomen, maar anderen zijn specifiek voor Azure SQL-Database. In sommige gevallen kunt u de verbruikte bronnen voor een database voor het zoeken van gebieden die u wilt verder af te stellen en traditioneel SQL Server-technieken om te werken in Azure SQL-Database uitbreiden bekijken.

### <a name="azure-portal-tools"></a>Azure portal hulpmiddelen
U vindt de twee hulpmiddelen in de portal van Azure waarmee u kunt analyseren en oplossen van prestatieproblemen met uw SQL-database:

- [Query prestaties inzicht](sql-database-query-performance.md)
- [SQL-Database Advisor](sql-database-advisor.md)

De Azure-portal bevat meer informatie over beide van deze hulpprogramma's en hoe ze worden gebruikt. Efficiënt opsporen en corrigeren van problemen, is het raadzaam dat u de eerste keer de hulpmiddelen in de portal van Azure. Het is raadzaam dat u de handleiding benaderingen die wordt besproken vervolgens ontbrekende indexen en query optimaliseren in speciale gevallen optimaliseren gebruiken.

### <a name="missing-indexes"></a>Ontbrekende indexen
Een veelvoorkomend probleem in de prestaties van de database OLTP verband houden met de de fysieke databaseontwerp. Vaak zijn databaseschema ontworpen en verzonden zonder te testen op schaal (hetzij in laden of in het gegevensvolume van de). De prestaties van een query-abonnement mogelijk Helaas kunt aanvaardbaar op kleine schaal maar afnemen aanzienlijk onder productieniveau gegevensvolumes. De meest voorkomende bron van dit probleem is het ontbreken van de juiste indexen om te voldoen aan filters of andere beperkingen in een query. Vaak ontbrekende indexen manifesten als een tabel wanneer een index seek kan voldoende scannen.

In dit voorbeeld wordt een scan het abonnement van de geselecteerde query gebruikt wanneer een seek zou voldoende:

    DROP TABLE dbo.missingindex;
    CREATE TABLE dbo.missingindex (col1 INT IDENTITY PRIMARY KEY, col2 INT);
    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO dbo.missingindex(col2) VALUES (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION;
    GO
    SELECT m1.col1
    FROM dbo.missingindex m1 INNER JOIN dbo.missingindex m2 ON(m1.col1=m2.col1)
    WHERE m1.col2 = 4;

![Een query-indeling met ontbrekende indexen](./media/sql-database-performance-guidance/query_plan_missing_indexes.png)

Azure SQL-Database kunt u zoeken en repareren algemene ontbreken voorwaarden indexeren. DMVs die zijn ingebouwd in Azure SQL-Database kijkt u naar querycompilaties waarin een index aanzienlijk sneller een de geschatte kosten als een query wilt uitvoeren. Tijdens de queryuitvoering, kunt u SQL-Database bijhouden hoe vaak elk queryplan wordt uitgevoerd en de geschatte tussenruimte tussen het abonnement waarop query uitvoeren en de imagined bijgehouden waar die index bestaan. U kunt deze DMVs snel raden welke wijzigingen in uw fysieke databaseontwerp algehele werkbelasting kosten voor een database en de werkelijke werkbelasting kunnen verbeteren.

U kunt deze query mogelijke ontbrekende indexen wordt berekend:

    SELECT CONVERT (varchar, getdate(), 126) AS runtime,
        mig.index_group_handle, mid.index_handle,
        CONVERT (decimal (28,1), migs.avg_total_user_cost * migs.avg_user_impact *
                (migs.user_seeks + migs.user_scans)) AS improvement_measure,
        'CREATE INDEX missing_index_' + CONVERT (varchar, mig.index_group_handle) + '_' +
                  CONVERT (varchar, mid.index_handle) + ' ON ' + mid.statement + '
                  (' + ISNULL (mid.equality_columns,'')
                  + CASE WHEN mid.equality_columns IS NOT NULL
                              AND mid.inequality_columns IS NOT NULL
                         THEN ',' ELSE '' END + ISNULL (mid.inequality_columns, '')
                  + ')'
                  + ISNULL (' INCLUDE (' + mid.included_columns + ')', '') AS create_index_statement,
        migs.*,
        mid.database_id,
        mid.[object_id]
    FROM sys.dm_db_missing_index_groups AS mig
    INNER JOIN sys.dm_db_missing_index_group_stats AS migs
        ON migs.group_handle = mig.index_group_handle
    INNER JOIN sys.dm_db_missing_index_details AS mid
        ON mig.index_handle = mid.index_handle
    ORDER BY migs.avg_total_user_cost * migs.avg_user_impact * (migs.user_seeks + migs.user_scans) DESC

In dit voorbeeld wordt de query geleid tot deze suggestie:

    CREATE INDEX missing_index_5006_5005 ON [dbo].[missingindex] ([col2])  

Nadat deze gemaakt, uitgelicht die dezelfde SELECT-instructie in een ander abonnement seek gebruikt in plaats van een gescande afbeelding, en vervolgens het abonnement efficiënter wordt uitgevoerd:

![Een query-abonnement met gecorrigeerde indexen](./media/sql-database-performance-guidance/query_plan_corrected_indexes.png)

De belangrijkste inzicht is dat de i/o-capaciteit van een gedeelde, rol systeem beperktere dan die van een speciale server-computer. Er is een premium op minimaliseren onnodige I/O om optimaal te profiteren van het systeem in het DTU van elk prestatieniveau van de niveaus van de service Azure SQL-Database. Ontwerp van de juiste fysieke database keuzen kunnen aanzienlijk verbeteren de latentie voor afzonderlijke query's, de doorvoer van gelijktijdige aanvragen per eenheid voor tijdschaal afgehandeld verbeteren en de kosten die nodig is om te voldoen aan de query te minimaliseren. Zie voor meer informatie over de ontbrekende index DMVs, [sys.dm_db_missing_index_details](https://msdn.microsoft.com/library/ms345434.aspx).

### <a name="query-tuning-and-hinting"></a>Query optimaliseren en coderingstips
De query optimizer in Azure SQL-Database is vergelijkbaar met de traditionele optimizer voor SQL Server-query. De meeste van de aanbevolen procedures voor het optimaliseren van query's en informatie over de motivering model beperkingen voor het optimaliseren van de query ook van toepassing op Azure SQL-Database. Als u query's in Azure SQL-Database afstemmen, krijgt u mogelijk de extra voordeel van het verkleinen van statistische systeembronnen. Uw toepassing mogelijk worden uitgevoerd op een lagere kosten dan een ruiseffect equivalent omdat deze afgespeeld op een lager prestatieniveau worden kan.

Een voorbeeld die algemene in SQL Server is en dat geldt ook voor Azure SQL-Database is hoe de query optimizer "bithandtekeningen" parameters. Tijdens de compilatie evalueert de query optimizer de huidige waarde van een parameter om te bepalen of deze een meer optimale query-abonnement kunt genereren. Hoewel deze strategie vaak tot een query-abonnement dat is aanzienlijk sneller dan een abonnement gecompileerd zonder bekende parameterwaarden leiden kan, deze werkt op dit moment dat beide in SQL Server en Azure SQL-Database. Soms wordt de parameter niet sniffed, en soms de parameter is sniffed maar het plan voor de gegenereerde optimale voor de volledige reeks parameterwaarden in een werkbelasting is. Microsoft bevat hints voor de query (richtlijnen), zodat u kunt meer opzettelijk bedoelingen opgeven en overschrijven van het standaardgedrag van de parameter bekijken. Als u hints voor de gebruikt, kunt u vaak gevallen waarin het standaardgedrag voor SQL Server- of Azure SQL-Database imperfecte voor een bepaalde klant werkbelasting is oplossen.

Het volgende voorbeeld wordt gedemonstreerd hoe de queryprocessor een abonnement dat is niet-optimale zowel voor prestaties en resourcevereisten kunt genereren. In dit voorbeeld ziet u ook als u een tip query gebruikt, u query uitvoeren tijd- en resourcekalenders vereisten voor uw SQL-database verkleinen kunt:

    DROP TABLE psptest1;
    CREATE TABLE psptest1(col1 int primary key identity, col2 int, col3 binary(200));

    DECLARE @a int = 0;
    SET NOCOUNT ON;
    BEGIN TRANSACTION
    WHILE @a < 20000
    BEGIN
        INSERT INTO psptest1(col2) values (1);
        INSERT INTO psptest1(col2) values (@a);
        SET @a += 1;
    END
    COMMIT TRANSACTION
    CREATE INDEX i1 on psptest1(col2);
    GO

    CREATE PROCEDURE psp1 (@param1 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1
        WHERE col2 = @param1
        ORDER BY col2;
    END
    GO

    CREATE PROCEDURE psp2 (@param2 int)
    AS
    BEGIN
        INSERT INTO t1 SELECT * FROM psptest1 WHERE col2 = @param2
        ORDER BY col2
        OPTION (OPTIMIZE FOR (@param2 UNKNOWN))
    END
    GO

    CREATE TABLE t1 (col1 int primary key, col2 int, col3 binary(200));
    GO

De Setupcode wordt gemaakt van een tabel die gegevens verdeling heeft vervormd. Het queryplan voor optimale verschilt op basis van welke parameter is geselecteerd. Helaas compileren niet het abonnement caching van gedrag altijd de query op basis van de meest voorkomende parameterwaarde. Zo is, is het mogelijk voor een optimale plan worden opgeslagen in de cache en gebruikt voor meerdere waarden, zelfs wanneer u een ander abonnement mogelijk een betere abonnement keuze gemiddeld. Het query-abonnement wordt vervolgens twee opgeslagen procedures die identiek zijn, behalve dat een een tip speciale query heeft gemaakt.

**Bijvoorbeeld: deel 1**

    -- Prime Procedure Cache with scan plan
    EXEC psp1 @param1=1;
    TRUNCATE TABLE t1;

    -- Iterate multiple times to show the performance difference
    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp1 @param1=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

**Bijvoorbeeld: deel 2**

(Raden u ten minste tien minuten voordat u deel 2 van het voorbeeld begint, wacht zodat de resultaten, onderling verschillen in de resulterende telemetriegegevens.)

    EXEC psp2 @param2=1;
    TRUNCATE TABLE t1;

    DECLARE @i int = 0;
    WHILE @i < 1000
    BEGIN
        EXEC psp2 @param2=2;
        TRUNCATE TABLE t1;
        SET @i += 1;
    END

Elk onderdeel van dit voorbeeld probeert een invoeginstructie met parameters 1000 keer uitvoeren (als u wilt een voldoende laden wilt gebruiken als een gegevensset test genereren). Bij het uitvoeren van opgeslagen procedures, worden de queryprocessor onderzocht, de waarde van de parameter die wordt doorgegeven aan de procedure tijdens de eerste gecompileerd (parameter "bekijken'). De processor slaat het resulterende plan en gebruikt voor later aanroepen, zelfs als de parameterwaarde anders is. Het optimale abonnement mogelijk niet worden gebruikt in alle gevallen. Soms moet u de optimizer om te kiezen een abonnement dat is nog beter voor de gemiddelde hoofdletters/kleine letters in plaats van het specifieke geval uit wanneer de query voor het eerst is gecompileerd gids. In dit voorbeeld wordt een 'scan'-abonnement dat alle rijen om te zoeken van elke waarde die overeenkomt met de parameter leest gegenereerd door het oorspronkelijke abonnement:

![Query met behulp van een abonnement scan optimaliseren](./media/sql-database-performance-guidance/query_tuning_1.png)

Omdat we de procedure met behulp van de waarde 1 hebt uitgevoerd, wordt het resulterende abonnement optimaal voor de waarde 1 is maar optimale voor alle andere waarden in de tabel is. Het resultaat waarschijnlijk zou niet uw bedoeling is als u kiest u elk plan willekeurig, omdat het abonnement langzamer en meer resources gebruikt.

Als u de test met uitvoert `SET STATISTICS IO` ingesteld op `ON`, het werk logische gescande afbeelding in dit voorbeeld is voltooid achter de schermen. U kunt zien dat er 1,148 leest uitgevoerd door het abonnement hebt (dat niet efficiënt verloopt, als de gemiddelde hoofdletters/kleine letters is om terug te keren slechts één rij):

![Query met behulp van een logische scan optimaliseren](./media/sql-database-performance-guidance/query_tuning_2.png)

Het tweede deel van het voorbeeld wordt de Tip van een query om de optimizer voor het gebruik van een bepaalde waarde tijdens het proces gecompileerd zien. In dit geval dit zorgt ervoor dat de queryprocessor de waarde die wordt doorgegeven als de parameter genegeerd en in plaats daarvan aan wordt ervan uitgegaan dat `UNKNOWN`. Dit heeft betrekking op een waarde die de gemiddelde frequentie in de tabel heeft (scheefheid worden genegeerd). Het plan voor de resulterende is een seek gebaseerde-abonnement dat is sneller en minder resources, Gemiddeld, dan het abonnement in deel 1 van dit voorbeeld gebruikt:

![Query met behulp van een query geheugensteun optimaliseren](./media/sql-database-performance-guidance/query_tuning_3.png)

Ziet u het effect in de tabel **sys.resource_stats** (er wordt een vertraging vanaf het moment dat u de test en wanneer de tabel in de gegevens worden ingevuld uitvoeren). Voor dit voorbeeld, deel 1 uitgevoerd tijdens het tijdvenster 22:25:00 en deel 2 uitgevoerd om 22:35:00. Houd er rekening mee dat het eerdere tijdvenster meer bronnen in die tijdvenster dan de hoger (vanwege abonnement efficiency verbeteringen gebruikt).

    SELECT TOP 1000 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![Query voorbeeldresultaten optimaliseren](./media/sql-database-performance-guidance/query_tuning_4.png)

>[AZURE.NOTE] Hoewel het volume in dit voorbeeld opzettelijk kleine is, kan het effect van niet-optimale parameters aanzienlijke, met name op grotere databases zijn. Het verschil in extreme omstandigheden kunnen kan tussen seconden voor snel situaties en uren voor traag gevallen zijn.

U kunt controleren **sys.resource_stats** om te bepalen of de resource voor een test meer of minder resources dan een ander test gebruikt. Wanneer u gegevens vergelijken, scheidt u de timing van tests zodat ze niet in hetzelfde venster 5 minuten in de weergave **sys.resource_stats** zijn. Het doel van de oefening is om te minimaliseren van het totaalbedrag van resources die worden gebruikt en niet op de resources piek minimaliseren. In het algemeen wordt minder een deel van een code voor latentie optimaliseren ook verbruik. Zorg ervoor dat de wijzigingen die u in een toepassing aanbrengt nodig zijn en de wijzigingen die niet een negatieve invloed op de gebruikerservaring voor iemand die hints voor de query in de toepassing gebruiken.

Als een werkbelasting een set herhalende van query's heeft, vaak, is het handig om te leggen en de optimalisatie van uw keuzen abonnement valideren omdat deze de minimale resource grootte eenheid die is vereist voor het hosten van de database de. Nadat u deze valideert, klikken af en toe de abonnementen om te zorgen dat ze hebben niet niet beschikbaar is weergegeven. U kunt meer informatie over [query hints (Transact-SQL)](https://msdn.microsoft.com/library/ms181714.aspx).

### <a name="cross-database-sharding"></a>Meerdere databases sharding
Omdat Azure SQL-Database op rol hardware wordt uitgevoerd, zijn de beperkingen van de capaciteit voor één database lager is dan voor een traditionele lokale SQL Server-installatie. Sommige klanten gebruiken sharding technieken om databasebewerkingen via meerdere databases verspreiden wanneer de bewerkingen niet binnen de de grenzen van één database in Azure SQL-Database. De meeste klanten die gebruikmaken van sharding technieken in Azure SQL-Database splitsen hun gegevens op één dimensie over meerdere databases. Voor deze benadering moet u begrijpen dat OLTP toepassingen vaak transacties die van toepassing zijn naar slechts één rij of een kleine groep rijen in het schema uitvoeren.

>[AZURE.NOTE] SQL-Database bevat nu een bibliotheek om u te helpen met sharding. Zie [overzicht van de bibliotheek elastische Database client](sql-database-elastic-database-client-library.md)voor meer informatie.

Bijvoorbeeld als een database bevat de klantnaam van de, volgorde en details van bestelling (zoals de voorbeelddatabase traditionele spellingregels voorbeeld die wordt geleverd met SQL Server), kan u splitsen deze gegevens in meerdere databases door een klant met de gerelateerde volgorde en gedetailleerde informatie over groeperen. U kunt garanderen dat de gegevens van de klant in één database blijft. De toepassing zou verschillende klanten over-databases, effectief spreiden het selectievakje laden over meerdere databases splitsen. Met sharding, klanten niet alleen de maximale limiet kunnen voorkomen, maar Azure SQL-Database ook werkbelasting die aanzienlijk groter dan de grenzen van de verschillende prestaties, zijn zo lang maken als elke afzonderlijke-database in de DTU past kan verwerken.

Hoewel de database sharding niet de capaciteit van de statistische resources voor een oplossing verkleinen, is het uiterst effectieve te ondersteunen zeer grote oplossingen die verdeeld over meerdere databases. Elke database kan worden uitgevoerd op een prestatieniveau verschillende ter ondersteuning van zeer grote, "effectieve" databases met hoge resourcevereisten.

### <a name="functional-partitioning"></a>Functionele partitioneren
SQL Server-gebruikers wordt vaak veel functies in één database combineren. Bijvoorbeeld als een toepassing logica heeft voor het beheren van voorraad voor een winkel, mogelijk die database logica die is gekoppeld aan Voorraadbeheer, inkooporders, opgeslagen procedures en geïndexeerde of gerealiseerde weergaven die het einde van de maand rapportage beheren. Op deze manier kunt eenvoudiger te beheren van de database voor bewerkingen, zoals back-up maken, maar ook moeten u om de grootte van de hardware voor het verwerken van de belasting over alle functies van een toepassing.

Als u een schaal architectuur in Azure SQL-Database gebruikt, is een goed idee om verschillende functies van een toepassing in andere databases splitsen. Met behulp van deze techniek, wordt elke toepassing onafhankelijk aangepast. Als een toepassing wordt uitgebreid veelgebruikte (en de belasting van de database verhogen), kan de beheerder kan onafhankelijke prestaties voor elke functie kiezen in de toepassing. Een toepassing kan zijn groter is dan een machine één product afhandelen kunt, omdat het selectievakje laden is verdeeld over meerdere computers aan de limiet, met deze architectuur.

### <a name="batch-queries"></a>Batch query 's
Voor toepassingen die toegang gegevens tot met behulp van grote hoeveelheden, veelgebruikte, ad hoc query wordt uitgevoerd, een aanzienlijke antwoord-tijdsduur is besteed aan netwerkcommunicatie tussen de toepassingslaag en de laag Azure SQL-Database. Zelfs wanneer de toepassing en de Azure SQL-Database in het midden met dezelfde gegevens, de netwerklatentie tussen de twee mogelijk worden vergroot met een groot aantal gegevens access bewerkingen. Verkleinen van het netwerk afronden reizen voor de data access-bewerkingen, kunt u overwegen de optie naar de batch op het ad hoc-query's of ze als opgeslagen procedures compileren. Als u de ad hoc-query's batch, kunt u meerdere query's verzenden als één grote batch in een enkel reis met Azure SQL-Database. Als u ad hoc-query's in een opgeslagen procedure compileren, krijgt u hetzelfde resultaat als wanneer u deze batch. Een opgeslagen procedure kunt u ook het voordeel van de kans van de query-abonnementen in Azure SQL-Database in het cachegeheugen zodat u de opgeslagen procedure opnieuw kunt gebruiken toeneemt.

Sommige-toepassingen zijn veel schrijven. Soms kunt u de totale i/o-belasting op een database verkleinen door te overwegen hoe u schrijft batch. Dit is vaak net zo eenvoudig als expliciete transacties gebruiken in plaats van transacties automatisch doorvoeren in opgeslagen procedures en ad-hoc batches. Zie voor een beoordeling van verschillende technieken die u kunt gebruiken, [Batching technieken voor SQL-Database-toepassingen in Azure wordt aangegeven](https://msdn.microsoft.com/library/windowsazure/dn132615.aspx). Experimenteren met uw eigen werkbelasting om het juiste model voor batchen. Zorg ervoor dat u begrijpen dat een model iets anders transacties consistentie garanties mogelijk hebben. Zoeken naar de juiste werklast die bronnengebruik minimaliseert is vereist voor het vinden van de juiste combinatie van consistentie en prestaties voor-en nadelen.

### <a name="application-tier-caching"></a>Toepassingslaag cache opslaan
Sommige databasetoepassingen hebt gelezen veel werkbelasting. Caching van lagen mogelijk Verminder de belasting van de database en mogelijk verkleint het prestatieniveau vereist ter ondersteuning van een database met behulp van Azure SQL-Database. Met [Azure bestand Vgx. Cache](https://azure.microsoft.com/services/cache/), hebt u een werkbelasting gelezen veel vindt u de gegevens eenmaal (of bijvoorbeeld eenmaal per toepassingslaag computer, afhankelijk van hoe deze is geconfigureerd), en die gegevens buiten uw SQL-database op te slaan. Dit is een manier om te verminderen database laden (CPU en gelezen I/O), maar er is een effect op transacties consistentie omdat de gegevens worden gelezen uit de cache mogelijk niet gesynchroniseerd met de gegevens in de database. Hoewel in veel toepassingen sommige niveau van inconsistenties acceptabel is, dat geldt niet voor alle werkbelasting in. U moet de toepassingsvereisten van een volledig kennen voordat u een toepassing niveaus in de cache strategie implementeren.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Servicelagen, [Opties voor de SQL-Database en prestaties](sql-database-service-tiers.md)
- Zie voor meer informatie over elastische database van toepassingen, [Wat is een Azure elastische database-toepassingen?](sql-database-elastic-pool.md)
- Zie voor informatie over prestaties en elastische database van toepassingen, [Wanneer u rekening moet houden een elastische database-toepassingen](sql-database-elastic-pool-guidance.md)
