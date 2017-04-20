<properties
   pageTitle="I-SQL-scripts met Data Lake Tools voor Visual Studio ontwikkelen | Azure"
   description="Informatie over het installeren van Data Lake Tools voor Visual Studio, het ontwikkelen en U SQL-testscripts. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-u-sql-language"></a>Zelfstudie: Aan de slag met Azure gegevens Lake Analytics U SQL-taal

I-SQL is een taal die worden waarbij de voordelen van het SQL met de duidelijke kracht van uw eigen code om alle gegevens met een schaal te verwerken. De mogelijkheid van de I-SQL-scalable gedistribueerde query kunt u efficiënt om gegevens te analyseren in de store en over relationele winkels zoals Azure SQL-Database.  U kunt naar proces ongestructureerde gegevens volgens schema op lezen, aangepaste logica en van UDF invoegen en uitbreiden om in te schakelen prima korrelig controle over het uitvoeren van bij het op schaal bevat. Meer informatie over het ontwerpfilosofie achter I-SQL, Raadpleeg dit [Visual Studio weblog posten](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Er zijn enkele verschillen van ANSI SQL- of T-SQL. De trefwoorden, zoals Selecteer moeten bijvoorbeeld zich in hoofdletters.

Dit type systeem en expressie taal in de select-component, waar predicaten enzovoort zich in C# bevinden is.
Dit betekent dat de gegevenstypen zijn de C#-typen en de gegevenstypen C# NULL semantiek en volgt u de vergelijkingsbewerkingen binnen een predikaat C#-syntaxis (bijvoorbeeld een == "foo").  Dit ook betekent dat de waarden zijn volledige .NET-objecten, zodat u kunt eenvoudig een methode gebruiken om te werken op het object (bijvoorbeeld "f o o o". Split(' ')).

Voor meer informatie raadpleegt [U SQL-verwijzing](http://go.microsoft.com/fwlink/p/?LinkId=691348).

###<a name="prerequisites"></a>Vereisten voor

U moet uitvoeren [Zelfstudie: I-SQL-scripts met Data Lake Tools voor Visual Studio ontwikkelen](data-lake-analytics-data-lake-tools-get-started.md).

In deze zelfstudie, kunt u een taak gegevens Lake Analytics uitgevoerd met het volgende I-SQL-script:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();

Dit script heeft geen transformatie stappen. Deze worden gelezen uit het bronbestand **SearchLog.tsv**genoemd, schematizes deze en Hiermee kunt u de rijenset weer aan een bestand met de naam van **SearchLog-eerste-i-sql.csv**.

Zoals u ziet het vraagteken naast het gegevenstype van het veld Duur. Dat betekent dat het veld duur kan niet null zijn.

Sommige concepten en trefwoorden die in het script:

- **Rijenset variabelen**: elke query-expressie die een rijenset oplevert kan worden toegewezen aan een variabele. I-SQL volgt de T-SQL variabele naming notatie, bijvoorbeeld **@searchlog** in het script.
    Houd rekening met dat de toewijzing hoeft niet kan worden uitgevoerd. Deze alleen namen van de expressie en geeft u de mogelijkheid om te worden opgebouwd meer complexe expressies.
- **EXTRAHEREN** , kunt u de mogelijkheid om te definiëren van een schema op lezen. Het schema is opgegeven door een kolomnaam en C# type naam paar gegevenspunten per kolom. Een zogenaamde **Extractor**, bijvoorbeeld **Extractors.Tsv()** tsv uitpakken wordt gebruikt. U kunt aangepaste extractie ontwikkelen.
- **Uitvoer** een rijenset duurt en serialiseert deze. De Outputters.Csv() uitvoer van een CSV-bestand in de opgegeven locatie. U kunt ook aangepaste Outputters ontwikkelen.
- U ziet de twee paden relatieve paden. U kunt ook absolute paden gebruiken.  Bijvoorbeeld

        adl://<ADLStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    U moet absolute pad gebruiken voor toegang tot de bestanden in de gekoppelde opslag-accounts.  De syntaxis voor bestanden die zijn opgeslagen in de gekoppelde opslag van Azure-account is:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure Blob container met openbare BLOB's of openbare containers toegangsmachtigingen worden momenteel niet ondersteund.

## <a name="use-scalar-variables"></a>Scalaire variabelen gebruiken

U kunt ook scalaire variabelen gebruiken om uw script onderhoud te vereenvoudigen. Het vorige I-SQL-script kan ook worden geschreven als volgt te werk:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Rijensets transformeren

Gebruik de **selecteren** om te transformeren rijensets:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

De WHERE-component [C# Booleaanse expressie](https://msdn.microsoft.com/library/6a71f45d.aspx)wordt gebruikt. U kunt de C#-expressietaal moet uw eigen expressies en functies. U kunt zelfs uitvoeren complexere filteren ze te combineren met logische voegwoorden (en) en disjunctions (ORs).

Het volgende script wordt de methode DateTime.Parse() en een voegwoord.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datatime.csv"
        USING Outputters.Csv();

Zoals u ziet dat de tweede query op het resultaat van de eerste rijenset werkt en het resultaat dus een samenstelling van de twee filters is. U kunt ook opnieuw gebruiken met de naam van een variabelen en de namen lexically zijn beperkt.

## <a name="aggregate-rowsets"></a>Statistische rijensets

I-SQL biedt de vertrouwde **ORDER BY**, **GROUP BY** en aggregaties.

De volgende query vindt u de totale duur per regio en vervolgens Hiermee kunt u de bovenkant 5 duur in volgorde.

I-SQL rijensets blijven niet behouden in de volgorde voor de volgende query. Als u wilt een uitvoer bestel, moet u dus ORDER BY toevoegen aan de instructie uitvoer zoals hieronder wordt weergegeven:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();
    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

I-SQL ORDER BY-component moet worden gecombineerd met de component ophalen in een SELECT-expressie.

ONDERVINDT U SQL-component kan worden gebruikt om de uitvoer beperken tot groepen die voldoen aan de HAVING-voorwaarde:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

## <a name="join-data"></a>Deelnemen aan gegevens

I-SQL biedt algemene join-operatoren zoals INNER JOIN, links/rechts/volledige OUTER JOIN, SEMI-JOIN, deel te nemen aan niet alleen tabellen, maar elke rijensets (ook die geproduceerd uit bestanden).

Het volgende script deelneemt aan de searchlog met een advertentie indruk achter te laten log en ontstaat de advertenties voor de queryreeks voor een bepaalde datum.

    @adlog =
        EXTRACT UserId int,
                Ad string,
                Clicked int
        FROM "/Samples/Data/AdsLog.tsv"
        USING Extractors.Tsv();

    @join =
        SELECT a.Ad, s.Query, s.Start AS Date
        FROM @adlog AS a JOIN <insert your DB name>.dbo.SearchLog1 AS s
                        ON a.UserId == s.UserId
        WHERE a.Clicked == 1;

    OUTPUT @join   
        TO "/output/Searchlog-join.csv"
        USING Outputters.Csv();


I-SQL biedt alleen ondersteuning voor de syntaxis van de ANSI-compatibele join: Rowset1 JOIN Rowset2 aan predikaat. De syntaxis van de oude van uit Rowset1, waar Rowset2 predikaat wordt niet ondersteund.
Het predikaat in een JOIN bevat een gelijkheidstype: join en geen expressie. Als u een expressie gebruiken wilt, kunt u deze naar een vorige rijenset select-component toevoegen. Als u doen van een andere vergelijking wilt, kunt u deze kunt verplaatsen in de WHERE-component.


## <a name="create-databases-table-valued-functions-views-and-tables"></a>Databases, tabelwaardefuncties, weergaven en tabellen maken

I-SQL kunt u gegevens in de context van een database en schemabestanden gebruiken. U moet dus niet altijd lezen of schrijven naar bestanden.

Elke I-SQL-script wordt uitgevoerd met een standaard-database (basis) en de standaardschema (DBO) als de standaardcontext. U kunt uw eigen database en/of schema maken. Als u wilt wijzigen van de context, gebruikt u de instructie **gebruiken** om te wijzigen van de context.


### <a name="create-a-table-valued-function-tvf"></a>Een tabelwaardefunctie (TVF) maken

In het vorige I-SQL-script herhaald u uitpakken lezen van het bronbestand dezelfde gebruiken. I-SQL tabelwaardefunctie kunt u de gegevens voor toekomstig gebruik onderbrengen.   

Het volgende script Hiermee maakt u een TVF *Searchlog()* in de standaarddatabase en het schema genoemd:

    DROP FUNCTION IF EXISTS Searchlog;

    CREATE FUNCTION Searchlog()
    RETURNS @searchlog TABLE
    (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
    )
    AS BEGIN
    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();
    RETURN;
    END;

Het volgende script ziet u het gebruik van de TVF gedefinieerd in het vorige script:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM Searchlog() AS S
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/SerachLog-use-tvf.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-views"></a>Weergaven maken

Als u slechts één queryexpressie die u wilt abstracte en niet wilt voorzien deze hebt, kunt u een weergave in plaats van een tabelwaardefunctie maken.

Het volgende script Hiermee maakt u een weergave genaamd *SearchlogView* in de standaarddatabase en het schema:

    DROP VIEW IF EXISTS SearchlogView;

    CREATE VIEW SearchlogView AS  
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

Het volgende script ziet met de gedefinieerde weergave:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchlogView
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-use-view.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-tables"></a>Tabellen maken

I-SQL is vergelijkbaar met relationele databasetabel, kunt u een tabel maken met een vooraf gedefinieerde schema of maak een tabel en het schema van de query die de tabel (ook wel bekend als tabel-AS SELECT maken of CTAS) worden afgeleid.

Een database en twee tabellen, wordt het volgende script maken:

    DROP DATABASE IF EXISTS SearchLogDb;
    CREATE DATABASE SeachLogDb
    USE DATABASE SearchLogDb;

    DROP TABLE IF EXISTS SearchLog1;
    DROP TABLE IF EXISTS SearchLog2;

    CREATE TABLE SearchLog1 (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string,

                INDEX sl_idx CLUSTERED (UserId ASC)
                    PARTITIONED BY HASH (UserId)
    );

    INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

    CREATE TABLE SearchLog2(
        INDEX sl_idx CLUSTERED (UserId ASC)
                PARTITIONED BY HASH (UserId)
    ) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here


### <a name="query-tables"></a>Querytabellen

U kunt de tabellen (gemaakt in het vorige script) op dezelfde manier als u query zoeken via de gegevensbestanden. In plaats van het maken van een rijenset met uitpakken, nu kunt u alleen ook verwijzen naar de tabelnaam van de.

Het transformeren script die u eerder hebt gebruikt is gewijzigd als u wilt lezen uit de tabellen:

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchLogDb.dbo.SearchLog2
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @res
        TO "/output/Searchlog-query-table.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Houd er rekening mee dat u momenteel niet uitvoeren een selecteren op een tabel in hetzelfde script als het script waar u die tabel maken.


##<a name="conclusion"></a>Sluiten

Wat is beschreven in de zelfstudie is slechts een klein deel van de I-SQL. Vanwege de reikwijdte van deze zelfstudie, kan geen ze alles, zoals omvatten:

- Gebruik CROSS gelden voor het delen van tekenreeksen, matrices en kaarten uitpakken in rijen.
- Werken gepartitioneerde sets met gegevens (bestandssets en gepartitioneerde tabellen).
- Ontwikkel door de gebruiker gedefinieerde operatoren zoals extractie, outputters, processors, lezers door gebruiker gedefinieerd in C#.
- I-SQL windowing functies gebruiken.
- I-SQL-code met weergaven, tabelwaardefuncties en opgeslagen procedures beheren.
- Willekeurige aangepaste code uitvoeren op uw knooppunten verwerking.
- Verbinding maken met Azure SQL-Databases en federate query's in uw gegevens I-SQL en Azure gegevens Lake en bieden.

## <a name="see-also"></a>Zie ook

- [Overzicht van Microsoft Azure-gegevens Lake Analytics](data-lake-analytics-overview.md)
- [I-SQL-scripts met Data Lake Tools voor Visual Studio ontwikkelen](data-lake-analytics-data-lake-tools-get-started.md)
- [Met behulp van I-SQL-venster-functies voor Azure gegevens Lake Analytics-taken](data-lake-analytics-use-window-functions.md)
- [Controleren en problemen met Azure gegevens Lake Analytics-taken met behulp van Azure-Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

## <a name="let-us-know-what-you-think"></a>Laat het ons weten wat u ervan vindt

- [Kunt u een functie indienen](http://aka.ms/adlafeedback)
- [U kunt hulp krijgen in de-forums](http://aka.ms/adlaforums)
- [Feedback geven over I-SQL](http://aka.ms/usqldiscuss)
