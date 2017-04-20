<properties
   pageTitle="Aan de slag met tijdelijke tabellen in Azure SQL-Database | Microsoft Azure"
   description="Leer hoe u aan de slag met tijdelijke tabellen gebruiken in Azure SQL-Database."
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
   ms.workload="sql-database"
   ms.date="08/29/2016"
   ms.author="carlrab"/>

#<a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Aan de slag met tijdelijke tabellen in Azure SQL-Database

Tijdelijke tabellen zijn een nieuwe programmeerfuncties-functie van Azure SQL-Database waarmee u voor het bijhouden en de volledige geschiedenis van wijzigingen in uw gegevens, zonder dat u voor het coderen van aangepaste analyseren. Tijdelijke tabellen houden gegevens nauw met tijd context zodat opgeslagen feiten kunnen worden geïnterpreteerd als geldig alleen binnen de specifieke periode. Deze eigenschap van tijdelijke tabellen kan efficiënt analyse op basis van tijd en ophalen inzichten die gegevens ontwikkeling.

##<a name="temporal-scenario"></a>Tijdelijke Scenario

In dit artikel ziet u de stappen voor het gebruik van tijdelijke tabellen in een scenario voor de toepassing. Stel dat u wilt bijhouden gebruikersactiviteit op een nieuwe website die wordt ontwikkeld maken of op een bestaande website die u wilt uitbreiden met gebruiker activiteit analytics. In dit voorbeeld vereenvoudigd we wordt ervan uitgegaan dat het aantal bezochte webpagina's in een periode een aanduiding die moet worden vastgelegd en bewaakt in de database van de website die wordt gehost in Azure SQL-Database. Het doel van de historische analyse van gebruikers actief is om invoer voor het ontwerpen van de website en betere ervaring beschikken voor de bezoekers.

De databasemodel voor dit scenario is heel eenvoudig: gebruiker activiteit metrisch wordt aangeduid met een veld voor één geheel getal, **PageVisited**, en wordt vastgelegd samen met basisinformatie in het gebruikersprofiel. Daarnaast kunnen voor analyse van de tijd die zijn gebaseerd, zou u een reeks rijen voor elke gebruiker, waarbij elke rij vertegenwoordigt het aantal pagina's een bepaalde gebruiker binnen een bepaalde periode bezocht.

![Schema](./media/sql-database-temporal-tables/AzureTemporal1.png)

Gelukkig hoeft u niet moeten worden geplaatst eventuele hoeveelheid in uw app om deze activiteitsgegevens te beheren. Dit proces is met tijdelijke tabellen automatische - waardoor u volledige flexibiliteit tijdens het websiteontwerp en meer tijd kunt richten op de gegevensanalyse zelf. Het enige wat u moet doen om ervoor te zorgen dat **WebSiteInfo** tabel is geconfigureerd als is [tijdelijke systeem versienummer](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0). De exacte procedure voor het gebruik van tijdelijke tabellen in dit scenario worden hieronder beschreven.

##<a name="step-1-configure-tables-as-temporal"></a>Stap 1: Tabellen als tijdelijke configureren

Afhankelijk van of u het starten van de ontwikkeling van nieuwe of bestaande toepassing upgraden, wordt u tijdelijke tabellen maken of bestaande wijzigen door het tijdelijke kenmerken toe te voegen. In het algemeen hoofdletters/kleine letters, uw scenario is een combinatie van de volgende twee opties. Deze actie uitvoeren met gebruik [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS), [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) of een ander Transact-SQL development-programma.


> [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de nieuwste versie van Management Studio gebruiken om u te blijven synchroon met updates voor Microsoft Azure en SQL-Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).


###<a name="create-new-table"></a>Nieuwe tabel maken

Gebruik context menu-item "Nieuwe systeem versienummer tabel" in SSMS Object Explorer om de query-editor openen met een script van de sjabloon tijdelijke tabel en gebruikt u "Opgeven waarden voor Parameters sjabloon" (Ctrl + Shift + M) voor de sjabloon vullen:

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

Kiezen in SSDT, "tijdelijke (systeem versienummer)" tabelsjabloon wanneer nieuwe items toe te voegen aan het databaseproject. Die ontwerpfunctie voor tabellen te openen en kunt u gemakkelijk de tabelindeling opgeven:

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

U kunt ook een tijdelijke tabel maken door het opgeven van de Transact-SQL-instructies rechtstreeks, zoals wordt weergegeven in het onderstaande voorbeeld. Houd er rekening mee dat de verplicht elementen van elke tijdelijke tabel de definitie van de periode en de component SYSTEM_VERSIONING met een verwijzing naar een andere gebruikerstabel waarin wordt opgeslagen historische rij versies zijn:

````
CREATE TABLE WebsiteUserInfo 
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED 
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL 
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
````

Wanneer u systeem versienummer tijdelijke tabel maakt, wordt automatisch de bijbehorende geschiedenistabel met de standaardconfiguratie gemaakt. De standaard geschiedenistabel bevat een gegroepeerde index B-structuur van de periode kolommen (end, starten) met gebruik van pagina compressie. Deze configuratie is optimaal voor de meeste scenario's waarin tijdelijke tabellen worden gebruikt, met name voor [controle van gegevens](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0). 

In dit geval spijt op tijd gebaseerde trendanalyses uitvoeren via een langere gegevensgeschiedenis en met gegevensgroepen die groter te maken, zodat de keuze opslag voor de geschiedenistabel een gegroepeerd columnstore-index is. Een gegroepeerd columnstore biedt zeer goede compressie en prestaties voor analytische query's. Tijdelijke tabellen voor de flexibiliteit indexen volledig onafhankelijk configureren op de huidige en het tijdelijke tabellen. 

**Opmerking**: Columnstore indexen zijn alleen beschikbaar in de premium-servicelaag.

Het volgende script ziet u hoe de standaardindex voor geschiedenistabel kan worden gewijzigd in de gegroepeerde columnstore:

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

Tijdelijke tabellen worden weergegeven in de Verkenner Object met het specifieke pictogram zodat u gemakkelijker kunt herkennen, terwijl de geschiedenistabel wordt weergegeven als een onderliggend knooppunt.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

###<a name="alter-existing-table-to-temporal"></a>Bestaande tabel naar tijdelijke wijzigen

Laten we duidelijk het alternatieve scenario waarin de tabel WebsiteUserInfo al bestaat, maar niet is ontworpen voor het bijhouden van wijzigingen. U kunt de bestaande tabel te worden tijdelijke, zoals wordt weergegeven in het volgende voorbeeld in dit geval gewoon uitbreiden:

````
ALTER TABLE WebsiteUserInfo 
ADD 
    ValidFrom datetime2 (0) GENERATED ALWAYS AS ROW START HIDDEN  
        constraint DF_ValidFrom DEFAULT DATEADD(SECOND, -1, SYSUTCDATETIME())
    , ValidTo datetime2 (0)  GENERATED ALWAYS AS ROW END HIDDEN   
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo); 

ALTER TABLE WebsiteUserInfo  
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

##<a name="step-2-run-your-workload-regularly"></a>Stap 2: Uw werkzaamheden regelmatig wordt uitgevoerd

Het belangrijkste voordeel van tijdelijke tabellen is dat u wilt wijzigen of aanpassen van uw website op geen enkele manier om uit te voeren bijhouden. Zodra u hebt gemaakt, opgeslagen tijdelijke tabellen transparant eerdere versies van de rij telkens wanneer u wijzigingen op uw gegevens uitvoeren. 

Om te kunnen gebruikmaken van automatische dit bepaalde scenario voor het bijhouden van wijzigingen, laten we gewoon kolom bijwerken **PagesVisited** telkens wanneer de installatieserver sessie op de website voor het beëindigen van gebruiker:

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

Het is belangrijk dat u ziet dat de bijwerkquery geen hoeft te weten de exacte tijd als de werkelijke bewerking is opgetreden of hoe historische gegevens voor toekomstige analyse behouden blijven. Beide aspecten worden automatisch verwerkt door de Azure SQL-Database. In het volgende diagram ziet u hoe geschiedenisgegevens wordt gegenereerd bij elke update.

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

##<a name="step-3-perform-historical-data-analysis"></a>Stap 3: Historische gegevensanalyses uitvoeren

Als tijdelijke systeem-versiebeheer is ingeschakeld, is historische gegevensanalyse nu slechts één query u af. In dit artikel bieden we een paar voorbeelden die adres algemene analyse-scenario's - alle informatie, verschillende opties kennis met de component [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) verkennen.

Als u wilt zien van de bovenste 10 gebruikers gesorteerd op het aantal bezochte webpagina's vanaf een uur geleden, voert u deze query:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

U kunt deze query als u wilt analyseren de bezoekers van de site op een dag geleden, een maand geleden eenvoudig wijzigen of op een willekeurige plaats in het verleden die u wilt.

Eenvoudige statistische analyses uitvoeren voor de vorige dag, gebruikt u het volgende voorbeeld:

````
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
````

Als u wilt zoeken voor activiteiten van een specifieke gebruiker binnen een bepaalde periode, gebruik de component opgenomen:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

Afbeelding visualisatie is vooral handig voor tijdelijke query's als u trends en gebruikspatronen in een intuïtieve manier heel eenvoudig weergeven kunt:

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

##<a name="evolving-table-schema"></a>Tabelschema in ontwikkeling

Meestal, moet u het tabelschema tijdelijke wijzigt terwijl u mee bezig zijn ontwikkelen van apps. Daarvoor wordt uit te voeren normale ALTER TABLE-instructies en Azure SQL-Database correct wijzigingen doorgeven aan de geschiedenistabel. Het volgende script ziet u hoe u extra kenmerk voor voortgangscontrole kunt toevoegen:

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

Terwijl uw werkzaamheden actief is, kunt u ook de definitie van kolom wijzigen:

````
/*Increase the length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

Tot slot kunt u een kolom die u niet meer nodig hebt.

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````
    
U kunt ook via nieuwste [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) tijdelijke tabelschema wijzigen terwijl u met de database (onlinemodus) of als onderdeel van de databaseproject (offlinemodus verbonden bent).

##<a name="controlling-retention-of-historical-data"></a>Bewaren van historische gegevens beheren

De geschiedenistabel mogelijk met systeem versienummer tijdelijke tabellen vergroot de omvang van database van meer dan gewone tabellen. Een geschiedenistabel grote en groeiende kan worden omgezet in een probleem beide vanwege puur opslagkosten, evenals een prestaties opleggen belasting op het tijdelijke query's uitvoeren. Ontwikkeling van een bewaarbeleid voor gegevens voor het beheren van gegevens in de geschiedenistabel is daarom een belangrijk aspect plannen en beheren van de levenscyclus van elke tijdelijke tabel. Met Azure SQL-Database hebt u de volgende methoden voor het beheren van historische gegevens in de tijdelijke tabel:

- [Tabel partitioneren](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
- [Aangepaste opruimen Script](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

##<a name="next-steps"></a>Volgende stappen

Bekijk [MSDN-documentatie](https://msdn.microsoft.com/library/dn935015.aspx)voor gedetailleerde informatie over tijdelijke tabellen.
Ga naar kanaal 9 om te horen van een [bestaande klanten tijdelijke-implementatie Succesverhaal](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) en bekijk een [live tijdelijke demonstratie](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).
