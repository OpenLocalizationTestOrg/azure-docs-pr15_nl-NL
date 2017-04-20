<properties
    pageTitle="Compatibiliteit effenen, hoe u Beoordeel | Microsoft Azure"
    description="Stappen en hulpprogramma's voor om te bepalen welke compatibiliteitsniveau meest geschikt is voor uw database op Azure SQL-Database of Microsoft SQL Server"
    services="sql-database"
    documentationCenter=""
    authors="alainlissoir"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.devlang="NA"
    ms.tgt_pltfrm="NA"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="alainl"/>


# <a name="improved-query-performance-with-compatibility-level-130-in-azure-sql-database"></a>Verbeterde prestaties van query's met compatibiliteit niveau 130 in Azure SQL-Database


Azure SQL-Database wordt uitgevoerd transparant honderden duizenden databases op veel verschillende compatibiliteitsniveaus, behouden en de compatibiliteit met eerdere versies naar de bijbehorende versie van Microsoft SQL Server te garanderen voor alle klanten!

Daarom niets voorkomt u dat klanten die geen bestaande databases op het niveau van de meest recente compatibiliteit wijzigen geen gebruikmaken van de nieuwe query optimizer en query processorfuncties. Als een overzicht van de geschiedenis, de uitlijning van de SQL-versies naar standaard compatibiliteitsniveaus zijn als volgt uit:

- 100: in SQL Server 2008 en Azure SQL Database V11:.
- 110: in SQL Server 2012 en Azure SQL Database V11:.
- 120: in SQL Server-2014 en Azure SQL Database V12.
- 130: in SQL Server-2016 en Azure SQL Database V12.


> [AZURE.IMPORTANT] Beginnen in **mid juni 2016**in Azure SQL-Database, is het niveau van de standaard-compatibiliteit 130 in plaats van 120 voor **nieuwe** databases.
> 
> Databases die zijn gemaakt voordat mid juni 2016 wordt *niet* worden beïnvloed en blijft de huidige compatibiliteitsniveau (100, 110 of 120). Databases die migreren van Azure SQL Database-versie dat V11: naar V12 hebben geen hun daadwerkelijke compatibiliteit gewijzigd.


In dit artikel besproken voor de voordelen van het niveau van de compatibiliteit 130 en hoe u gebruikmaken van deze voordelen. We adres van de mogelijke bijwerkingen op de prestaties van de query's voor de bestaande SQL-toepassingen.


## <a name="about-compatibility-level-130"></a>Over de compatibiliteit effenen 130


Als u weten het niveau van de huidige compatibiliteit van uw database wilt, Voer eerst de volgende Transact-SQL-instructie.


```
SELECT compatibility_level
    FROM sys.databases
    WHERE name = '<YOUR DATABASE_NAME>’;
```


Voordat u deze wijziging naar een niveau 130 zijn de gevolgen voor de **zojuist** gemaakt-databases, laten we bekijken welke deze wijziging wordt gevormd door alle over tot en met enkele voorbeelden van zeer eenvoudige query en kijk hoe iedereen kunt profiteren van deze.

Queryverwerking in relationele databases zeer complex kan zijn en kan leiden naar een groot aantal computer wetenschap en wiskunde voor meer informatie over de inherent ontwerpkeuzen en het gedrag. In dit document, is de inhoud met opzet vereenvoudigd om ervoor te zorgen dat iedereen met minimale technische achtergrondinformatie kunt begrijpen de gevolgen van de wijzigingen in het compatibiliteit wat en bepalen hoe deze toepassingen kunt profiteren.

Laten we eens kort kijken wat het niveau van de compatibiliteit 130 kunt u de tabel.  U vindt meer informatie op [ALTER DATABASE compatibiliteitsniveau (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx), maar hier volgt een kort overzicht:

- De werking van het invoegen van een invoegen-select-instructie kan meerdere threads toestaan of kunt u een parallelle abonnement hebt terwijl voordat deze bewerking één thread is.
- Geheugen optimaliseren tabel en tabel variabelen query's kunnen beschikken nu over parallelle plannen, terwijl voordat deze bewerking is ook enkele thread.
- Statistieken voor geheugen geoptimaliseerd tabel kan nu worden partijen en worden automatisch bijgewerkt. Zie [Wat is er nieuw in de Database Engine: In het geheugen OLTP](https://msdn.microsoft.com/library/bb510411.aspx#InMemory) voor meer informatie.
- Batch modus v/s rij modus wordt met kolom Store indexen
  - Krabbels op een tabel met een index kolom Store zijn nu in de batchmodus.
  - Windowing aggregaties werken nu in de batchmodus zoals TSQL VERTRAGINGS/OVERLAPPINGS-instructies.
  - Query's op kolom Store tabellen met meerdere afzonderlijke componenten werken in de Batch-modus.
  - Query uitvoeren onder DOP = 1 of met een seriële plan uitvoeren ook in de Batch-modus.
- Laatst, verbeteringen in de verhouding raming werkelijk afkomstig zijn met het niveau van compatibiliteit 120, maar voor mensen met een lager niveau compatibiliteit (dat wil zeggen 100 of 110), de verplaatsen naar compatibiliteit niveau 130 ook brengt deze verbeteringen en deze profiteert ook de prestaties van de query's van uw toepassingen.


## <a name="practicing-compatibility-level-130"></a>Compatibiliteitsniveau 130 oefent


Eerste we gaan sommige tabellen, indexen en willekeurige gegevens hebt gemaakt als u wilt oefenen met deze nieuwe functies. Voorbeelden van de TSQL-scripts kunnen worden uitgevoerd onder SQL Server-2016 of onder Azure SQL-Database. Echter wanneer u een Azure SQL-database maakt, moet dat u ten minste een P2 database kiezen omdat u ten minste een aantal cores toestaan multithreading en kunnen daarom profiteren van deze functies nodig hebt.


```
-- Create a Premium P2 Database in Azure SQL Database

CREATE DATABASE MyTestDB
    (EDITION=’Premium’, SERVICE_OBJECTIVE=’P2′);
GO

-- Create 2 tables with a column store index on
-- the second one (only available on Premium databases)

CREATE TABLE T_source
    (Color varchar(10), c1 bigint, c2 bigint);

CREATE TABLE T_target
    (c1 bigint, c2 bigint);

CREATE CLUSTERED COLUMNSTORE INDEX CCI ON T_target;
GO

-- Insert few rows.

INSERT T_source VALUES
    (‘Blue’, RAND() * 100000, RAND() * 100000),
    (‘Yellow’, RAND() * 100000, RAND() * 100000),
    (‘Red’, RAND() * 100000, RAND() * 100000),
    (‘Green’, RAND() * 100000, RAND() * 100000),
    (‘Black’, RAND() * 100000, RAND() * 100000);

GO 200

INSERT T_source SELECT * FROM T_source;

GO 10
```


Nu, laten we een uiterlijk betreft als enkele van de verwerking van Query-functies die afkomstig zijn met het niveau van compatibiliteit 130.


## <a name="parallel-insert"></a>Parallelle invoegen


De onderstaande TSQL-instructies uitvoeren, voert de bewerking invoegen onder compatibiliteitsniveau 120 en 130, waarmee respectievelijk de bewerking invoegen in één discussielijnen model (120), en in een met meerdere threads model (130).


```
-- Parallel INSERT … SELECT … in heap or CCI
-- is available under 130 only

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO 

-- The INSERT part is in serial

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130
GO

-- The INSERT part is in parallel

INSERT t_target WITH (tablock)
    SELECT C1, COUNT(C2) * 10 * RAND()
        FROM T_source
        GROUP BY C1
    OPTION (RECOMPILE);

SET STATISTICS XML OFF;
```


Door te vragen de werkelijke klikken query bekijken via de grafische voorstelling of de XML-inhoud, kunt u bepalen welke verhouding schatting functie op afspelen is. De abonnementen naast elkaar ziet op afbeelding 1, we duidelijk dat de uitvoering kolom Store invoegen uit seriële in 120 naar parallel in 130 gaat. Bedenk ook, dat de wijziging van het pictogram herhaler in het 130 abonnement met twee pijlen naar parallelle, wat aangeeft dat nu de uitvoering herhaler daadwerkelijk parallelle is. Als er grote invoegbewerkingen om uit te voeren, worden de parallelle uitvoering, dat is gekoppeld aan het aantal core u over beschikt voor de database, sneller; maximaal 100 keer geen sneller afhankelijk van uw situatie!


*Afbeelding 1: Invoegbewerking gewijzigd van seriële in parallel compatibiliteit met niveau 130.*


![Afbeelding 1](./media/sql-database-compatibility-level-query-performance-130/figure-1.jpg)


## <a name="serial-batch-mode"></a>SERIËLE batchmodus


Verplaatsen naar compatibiliteitsniveau 130 tijdens het verwerken van rijen met gegevens kunt op dezelfde manier batchbestand modus. Bewerkingen van de modus batch zijn eerst alleen beschikbaar wanneer u een kolomindex store geplaatst hebt. Tweede, een batch meestal vertegenwoordigt ~ 900 rijen, en gebruikmaakt van een code logica geoptimaliseerd voor multicore CPU, hoger geheugengebruik vereist en rechtstreeks worden gebruikt in de gecomprimeerde gegevens van de kolom Store indien mogelijk. Deze voorwaarden wordt voldaan, kan in één keer ~ 900 rijen verwerken met SQL Server-2016 in plaats van 1 rij op het moment, en als gevolg hiervan, de algemene algemene kosten van de bewerking nu wordt gedeeld door de hele batch, verkleinen van de totale kosten per rij. Deze gedeelde hoeveelheid bewerkingen gecombineerd met de kolom store compressie in principe Hiermee reduceert u de latentie verbindt waarop een SELECT batch modus betrekking heeft. U kunt meer informatie vinden over de kolom store en batch-modus met [Columnstore indexen handleiding](https://msdn.microsoft.com/library/gg492088.aspx).


```
-- Serial batch mode execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT (C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO 

– The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Als zichtbaar onder, door de naleving van de query abonnementen naast elkaar op figuur 2, kunnen wij is Zie dat de verwerkingsmodus is gewijzigd met niveau van de compatibiliteit, en als gevolg hiervan, bij het uitvoeren van de query's in het niveau van beide compatibiliteit helemaal, u dat het grootste deel van de verwerkingstijd ziet kunt besteed in de rij-modus (86%) vergeleken met de batchmodus (14%), waar 2 batches zijn verwerkt. De gegevensset vergroten, het voordeel, neemt.


*Afbeelding 2: Selecteer bewerking wordt gewijzigd in seriële batch-modus met compatibiliteitsniveau 130.*


![Afbeelding 2](./media/sql-database-compatibility-level-query-performance-130/figure-2.jpg)


## <a name="batch-mode-on-sort-execution"></a>Batchmodus voor op sorteren worden uitgevoerd


Vergelijkbaar met het bovenstaande, maar toegepast op een sorteerbewerking, de overgang van rij-modus (compatibiliteitsniveau 120) naar de batchmodus (compatibiliteitsniveau 130) verbetert de prestaties van de sorteerbewerking om dezelfde redenen.


```
-- Batch mode on sort execution

SET STATISTICS XML ON;

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 120;
GO

-- The scan and aggregate are in row mode

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

-- The scan and aggregate are in batch mode,
-- and force MAXDOP to 1 to show that batch mode
-- also now works in serial mode.

SELECT C1, COUNT(C2)
    FROM T_target
    GROUP BY C1
    ORDER BY C1
    OPTION (MAXDOP 1, RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Zichtbaar naast elkaar op figuur 3, kunnen we zien dat de sorteerbewerking in de rij modus 81% van de kosten, betekent en de batchmodus alleen 19% van de kosten (respectievelijk 81% en 56% van het sorteren zelf).


*Afbeelding 3: Sorteerbewerking verandert rij batchmodus met compatibiliteitsniveau 130.*


![Afbeelding 3](./media/sql-database-compatibility-level-query-performance-130/figure-3.png)


Duidelijk, deze voorbeelden alleen bevatten tientallen duizenden rijen, namelijk niets wanneer u de gegevens die beschikbaar zijn in de meeste SQL-Servers bekijkt deze dagen. Deze tegen miljoenen rijen in plaats daarvan alleen project en dit in enkele minuten van elke dag in behandeling de aard van uw werkzaamheden gespaard uitvoering kan vertalen.


## <a name="cardinality-estimation-ce-improvements"></a>Verbeteringen in de verhouding schatting (CE)


Kennis met SQL Server-2014, elke database uitgevoerd op een compatibiliteitsniveau 120 of boven maakt gebruik van de nieuwe verhouding schatting functionaliteit. Verhouding schatting is in feite de logica gebruikt om te bepalen hoe SQL server een query op basis van de geschatte kosten wordt uitgevoerd. De schatting wordt berekend met behulp van de invoer van statistieken die is gekoppeld aan objecten betrokken bij deze query. Vrijwel, op een hoog niveau, verhouding schatting functies zijn rij tellen maakt een schatting samen met informatie over de verdeling van de waarden, het aantal unieke waarden, en de dubbele tellingen komen uit de tabellen en objecten waarnaar wordt verwezen in de query. Deze maakt een schatting verkeerde krijgt, kan leiden naar onnodige schijf I/O vanwege onvoldoende geheugen verleent (dat wil zeggen TempDB morsen), of een selectie van een seriële abonnement kan worden uitgevoerd via de uitvoering van een parallelle plannen, om een paar te noemen. Sluiting, onjuiste maakt een schatting kunnen leiden tot een algehele systeemprestaties van de queryuitvoering. Aan de andere kant leidt betere schattingen, nauwkeuriger maakt een schatting, naar betere query executions!

Zoals eerder query optimalisaties en maakt een schatting een complexe belangrijk zijn, maar als u meer informatie over het queryplannen en schatting van de verhouding wilt, kunt u verwijzen naar het document aan [Uw Query-abonnement met de SQL Server 2014 verhouding schatting optimaliseren](https://msdn.microsoft.com/library/dn673537.aspx) voor een grondigere kennismaking.


## <a name="which-cardinality-estimation-do-you-currently-use"></a>Welke schatting verhouding u momenteel gebruik?


Om te bepalen welke verhouding schatting uw query's worden uitgevoerd, laten we gewoon gebruik onder de onderstaande voorbeelden query. Houd er rekening mee dat het eerste voorbeeld wordt uitgevoerd onder compatibiliteitsniveau 110,: de overdracht van het gebruik van de oude verhouding schatting-functies.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 110;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Nadat de uitvoering is voltooid, klikt u op de koppeling XML- en kijkt u naar de eigenschappen van de eerste herhaler zoals hieronder wordt weergegeven. Houd rekening met de naam van de eigenschap die momenteel zijn ingesteld op 70 CardinalityEstimationModelVersion genoemd. Dit betekent niet dat het niveau van de database-compatibiliteit is ingesteld op de versie van de SQL Server 7.0 (dit is ingesteld op 110 als zichtbaar zijn in de bovenstaande TSQL-instructies), maar de waarde 70 gewoon de functionaliteit van de beschikbare sinds SQL Server 7.0, die geen primaire revisies tot SQL Server-2014 hadden (die wordt geleverd met een compatibiliteitsniveau van 120) oudere verhouding schatting vertegenwoordigt.


*Afbeelding 4: De CardinalityEstimationModelVersion is ingesteld op 70 bij gebruik van een compatibiliteitsniveau van 110 of onder.*


![Afbeelding 4](./media/sql-database-compatibility-level-query-performance-130/figure-4.png)


U kunt ook het compatibiliteitsniveau te 130 wijzigen en het gebruik van de nieuwe functie van de verhouding schatting uitschakelen met behulp van de LEGACY_CARDINALITY_ESTIMATION stelt in op aan met de [Configuratie van DATABASE beperkt wijzigen](https://msdn.microsoft.com/library/mt629158.aspx). Dit is precies hetzelfde als met 110 uit een schatting van de verhouding functie oogpunt, bij het gebruik van de meest recente queryverwerking compatibiliteitsniveau. Als u dit doet, kunt u profiteren van de nieuwe functies binnenkort met niveau van de meest recente compatibiliteit (dat wil zeggen batchmodus) voor queryverwerking, maar nog steeds zijn afhankelijk van de oude verhouding schatting functionaliteit indien nodig.


```
-- Old CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


Verplaatsen naar het niveau van de compatibiliteit 120 of 130, schakelt de nieuwe functie van de verhouding schatting. In dat geval de standaard CardinalityEstimationModelVersion ingesteld dienovereenkomstig gewijzigd 120 of 130 als daaronder weergegeven.


```
-- New CE

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT [c1]
    FROM [dbo].[T_target]
    WHERE [c1] > 20000;
GO

SET STATISTICS XML OFF;
```


*Afbeelding 5: De CardinalityEstimationModelVersion ingesteld op 130 wanneer u een compatibiliteitsniveau van 130 gebruikt.*


![Afbeelding 5](./media/sql-database-compatibility-level-query-performance-130/figure-5.jpg)


## <a name="witnessing-the-cardinality-estimation-differences"></a>Den met de verhouding schatting verschillen


Nu kunnen we uitvoeren iets meer complexe query waarbij een INNER JOIN met een WHERE-component met enkele predicaten en eens kijken naar de rij telling schatting van de oude verhouding schatting functie eerst.


```
-- Old CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = ON;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Effectief uitvoeren van deze query geeft als resultaat 200.704 rijen, terwijl de schatting van de rij met de functionaliteit voor oude verhouding schatting 194,284 rijen vorderingen. Duidelijk, zoals gezegd voordat, deze rij aantal resultaten is ook afhankelijk van hoe vaak u hebt uitgevoerd in de vorige voorbeelden waarin wordt gevuld van de steekproef tabellen steeds opnieuw aan elke uitvoeren. Duidelijk, de predicaten in uw query wordt ook van invloed zijn op de werkelijke schatting afgezien van de shape van de tabel, gegevens inhoud en hoe deze gegevens daadwerkelijk relateren met elkaar.


*Afbeelding 6: De rij tellen schatting is 194,284 of 6.000 rijen uit van de 200.704 rijen verwacht.*


![Afbeelding 6](./media/sql-database-compatibility-level-query-performance-130/figure-6.jpg)


Op dezelfde manier, laten we nu de dezelfde query uitgevoerd met de nieuwe functionaliteit voor de verhouding schatting.


```
-- New CE row estimate with INNER JOIN and WHERE clause

ALTER DATABASE MyTestDB
    SET COMPATIBILITY_LEVEL = 130;
GO

ALTER DATABASE
    SCOPED CONFIGURATION
    SET LEGACY_CARDINALITY_ESTIMATION = OFF;
GO

SET STATISTICS XML ON;

SELECT T.[c2]
    FROM
                   [dbo].[T_source] S
        INNER JOIN [dbo].[T_target] T  ON T.c1=S.c1
    WHERE
        S.[Color] = ‘Red’  AND
        S.[c2] > 2000  AND
        T.[c2] > 2000
    OPTION (RECOMPILE);
GO

SET STATISTICS XML OFF;
```


Bekijken via de hieronder nu zien we dat de schatting van de rij 202,877, of veel nabij en hoger zijn dan de oude verhouding schatting is.

*Afbeelding 7: De rij tellen raming is nu 202,877, in plaats van 194,284.*


![Afbeelding 7](./media/sql-database-compatibility-level-query-performance-130/figure-7.jpg)


In feite de resultatenset is 200.704 rijen (maar alles is afhankelijk van hoe vaak u de query's van de vorige voorbeelden hebt uitgevoerd, maar belangrijker omdat de TSQL gebruikmaakt van de instructie ASELECT(), de werkelijke waarden die worden geretourneerd kunnen afwijken van één uitvoeren naar de volgende). In dit voorbeeld wordt heeft de nieuwe verhouding schatting daarom een betere taak op het aantal rijen schatting omdat 202,877 veel dichter bij 200,704, dan 194,284 is! Achternaam, als u wijzigt de WHERE-component predicaten naar gelijke (en niet ">" bijvoorbeeld), kan dit de maakt een schatting tussen de oude maken en nieuwe verhouding functie nog andere, afhankelijk van hoeveel overeenkomt met u kan bereiken.

Duidelijk, in dit geval wordt ~ 6000 rijen uit van de werkelijke aantal staat niet voor een groot aantal gegevens in sommige gevallen. Hiermee kunt u miljoenen rijen over verschillende tabellen en meer complexe query's en soms de geschatte kan worden uitgeschakeld door miljoenen rijen en daarom de kans op pesterijen kiezen-ups van het verkeerde execution-abonnement of aanvragen van een onvoldoende geheugen verleent TempDB morsen en om er meer i/o-, waardoor transponeren zijn nu veel hoger.

Als u de verkoopkans, oefening hebt deze vergelijking met de meest voorkomende query's en gegevenssets, en ontdek zelf door hoeveel enkele van de oude en nieuwe maakt een schatting worden beïnvloed, terwijl deel meer uit van de realiteit of enkele anderen alleen gewoon dichter naar de werkelijke rij alleen kan worden geteld werkelijk geretourneerde in de resultatensets. Alles hangt af van de vorm van uw query's, de Azure SQL database-eigenschappen, de aard en de grootte van uw gegevenssets en de beschikbare over hen statistieken. Als u uw exemplaar van de Azure SQL-Database net hebt gemaakt, moet de optimizer query maken van de knowledge maken in plaats van hergebruiken statistieken gemaakt van de vorige query wordt uitgevoerd. Zo is, zijn de maakt een schatting zeer contextuele en bijna specifiek zijn voor elke situatie server en -toepassing. Dit is een belangrijk aspect in gedachten moet houden!


## <a name="some-considerations-to-take-into-account"></a>Enkele punten waarmee u rekening te houden


Hoewel de meeste werkbelasting wilt profiteren van de niveau van de compatibiliteit 130, voordat u gesteld door het niveau van de compatibiliteit voor uw productieomgeving, hebt u in feite 3 opties:

1. U verplaatsen naar een compatibiliteitsniveau 130 en Zie hoe dingen uitvoeren. Geval u enkele behalen ziet klikt, wordt u gewoon items instellen de compatibiliteit niveau terug naar de oorspronkelijke niveau of 130 behouden en wordt alleen omgekeerde de verhouding schatting terug naar de oudere modus (zoals hierboven vermeld, dit alleen kan betrekking hebben op het probleem).
2. U uw bestaande toepassingen onder vergelijkbare productie laden uitvoerig te testen, verfijnen en de prestaties voordat u overschakelt naar productie valideren. In geval van problemen, dezelfde als hierboven, u kunt altijd terug te gaan naar het niveau van de oorspronkelijke compatibiliteit, of gewoon de verhouding schatting omkeren terug naar de oudere modus.
3. Als laatste optie en de meest recente manier om het adres van deze vragen, is om te profiteren van de Query-Store. Dat is de aanbevolen optie vandaag! Om u te helpen de analyse van uw query's onder compatibiliteit 120 effenen of onder versus 130, kunnen niet aangeraden genoeg is om te gebruiken Query Store. Query Store is beschikbaar met de nieuwste versie van Azure SQL Database V12 en ontworpen om u te helpen bij het oplossen van query-prestaties. Als het ware de Store Query een recorder flight gegevens voor de database verzamelen en gedetailleerde historische informatie over alle query's presenteren. Dit maakt prestaties forensics enorm eenvoudiger door de tijd opsporen en oplossen van problemen te verkorten. U vindt meer informatie aan [Query Store: een recorder flight gegevens voor de database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).


AT overzichtsweergave, als u al een set van databases op compatibiliteitsniveau 120 of onder uitgevoerd, en wilt enkele van deze verplaatsen naar 130 of omdat uw werkzaamheden automatisch inrichten van nieuwe databases die binnenkort worden standaard ingesteld op 130, houd rekening met de volgende elementen:

- Inschakelen voordat u deze wijzigt naar het niveau van de nieuwe compatibiliteit in productie, Query Store. U kunt [de compatibiliteitsmodus Database wijzigen](https://msdn.microsoft.com/library/bb895281.aspx) en gebruiken van de Query-Store verwijzen voor meer informatie.
- Test vervolgens alle essentiële werkbelasting met vertegenwoordiger gegevens en query's van een productie-achtige-omgeving en de prestaties ervaring vergelijken en als gerapporteerde door Query Store. Als u bepaalde behalen ondervindt, kunt u de regressed query's in de Query-Store identificeren en gebruiken van het abonnement dat optie in de Query Store (ook bekend abonnement losmaken). In dat geval u definitief blijven met het niveau van de compatibiliteit 130 en het abonnement voormalige query gebruiken als voorgestelde door de Query-winkel.
- Als u wilt om te profiteren van nieuwe functies en mogelijkheden van Azure SQL-Database (die wordt uitgevoerd SQL Server-2016), maar gevoelige wijzigingen gebracht met de compatibiliteitsniveau 130, noodgevallen zijn, kunt u ook dat niveau van de compatibiliteit terug naar het niveau dat past bij uw werkzaamheden met behulp van een DATABASE wijzigen-instructie. Maar eerst moet u zich realiseren dat het Query-Store-abonnement losmaken van de optie de beste optie is omdat er geen gebruikmaakt van 130 is in principe blijven op het functionaliteitsniveau van de van een oudere versie van SQL Server.
- Als er multitenant toepassingen die meerdere databases in beslag nemen, dit kan het nodig zijn bij het inrichten bedrijfslogica van uw databases om ervoor te zorgen het niveau van een consistente compatibiliteit over alle databases. oude en nieuwe ingerichte bestanden. De prestaties van de werklast van uw toepassing kan zijn afhankelijk van het feit dat sommige databases op met verschillende compatibiliteitsniveaus worden uitgevoerd en niveau consistentie van de compatibiliteit tussen elke database kan dus vereist alleen worden aangeboden als de dezelfde ervaring naar uw klanten alles over het bord. Houd er rekening mee dat het is niet verplicht, dat deze echt afhankelijk hoe uw toepassing wordt beïnvloed door het niveau van de compatibiliteit.
- Laatst, met betrekking tot de verhouding schatting en net als het wijzigen van het niveau van de compatibiliteit, voordat u verder gaat in productie, verdient Test uw werkzaamheden productie onder de nieuwe voorwaarden om te bepalen als uw toepassing van de verbeteringen verhouding schatting gebruikmaakt.


## <a name="conclusion"></a>Sluiten


Gebruik van Azure SQL-Database om te profiteren van alle verbeteringen van SQL Server-2016, kunt u uw query executions duidelijk verbeteren. Net zoals-is! Natuurlijk, zoals een nieuwe functie, moet een juiste evaluatie doen om te bepalen de exacte voorwaarden, waaronder de databasewerklast van uw het beste werkt. Ervaring ziet u dat de meeste werkbelasting zijn moet ten minste uitvoeren transparant onder compatibiliteitsniveau 130, terwijl u gebruikmaken van nieuwe queryverwerking functies en nieuwe verhouding schatting. Die gezegd uit praktisch oogpunt, er zijn altijd enkele uitzonderingen en beginletters einddatum doen toewijding inzetten is een belangrijk beoordeling om te bepalen hoeveel u kunt profiteren van deze verbeteringen. En klik nogmaals de Store Query kan zijn een uitstekende help in dit werk doen van!

U kunt een compatibiliteitsniveau 140 zoals later in SQL Azure wordt aangegeven in de toekomst verwachten. Wanneer het tijd nodig is, wordt begin bespreken van wat dit niveau compatibiliteit in de toekomst 140 brengt, net zoals kort besproken hier welk compatibiliteitsniveau 130 brengt vandaag.

Nu we niet vergeten, beginnend vanaf juni 2016, Azure SQL-Database wordt gewijzigd het niveau van de standaard-compatibiliteit van 120 in 130 voor nieuwe databases. Let op!


## <a name="references"></a>Verwijzingen


- [Wat is er nieuw in de Database Engine](https://msdn.microsoft.com/library/bb510411.aspx#InMemory)

- [Blog: Query Store: een recorder flight gegevens voor de database, door Borko Novakovic, juni 8 2016](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/)

- [ALTER DATABASE compatibiliteitsniveau (Transact-SQL)](https://msdn.microsoft.com/library/bb510680.aspx)

- [ALTER DATABASE BEREIK CONFIGURATIE](https://msdn.microsoft.com/library/mt629158.aspx)

- [Compatibiliteitsniveau 130 voor Azure SQL Database V12](https://azure.microsoft.com/updates/compatibility-level-130-for-azure-sql-database-v12/)

- [Met de SQL Server-schatting van de verhouding 2014 optimaliseren van uw Query abonnementen](https://msdn.microsoft.com/library/dn673537.aspx)

- [Columnstore indexen handleiding](https://msdn.microsoft.com/library/gg492088.aspx)

- [Blog: Verbeterde queryprestaties compatibiliteitsniveau 130 in Azure SQL-Database, door aan Alain Lissoir, mei 6 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/)



<!--
Improved Query Performance with Compatibility Level 130 in Azure SQL Database

May 6, 2016 by Alain Lissoir (AlainL), on GitHub 'alainlissoir'.

https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/05/06/improved-query-performance-with-compatibility-level-130-in-azure-sql-database/

..... Now, above.
....................
..... Soon, below?

CAPS / MSDN ideally, but instead on ACom:
.. # Assess effects of latest compatibility level on query performance, how to

sql-database-compatibility-level-query-performance-130.md

genemi = MightyPen , 2016-05-20  Friday  17:00pm
-->
