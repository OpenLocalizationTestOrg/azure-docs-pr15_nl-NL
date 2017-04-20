<properties
    pageTitle="Alle onderwerpen voor SQL Data Warehouse service | Microsoft Azure"
    description="Tabel met alle onderwerpen voor de Azure-service met SQL Data Warehouse die aanwezig op http://azure.microsoft.com/documentation/articles/, titel en beschrijving de naam."
    services="sql-data-warehouse"
    documentationCenter=""
    authors="barbkess"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-data-warehouse"
    ms.workload="sql-data-warehouse"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="barbkess"/>


# <a name="all-topics-for-azure-sql-data-warehouse-service"></a>Alle onderwerpen voor Azure SQL Data Warehouse-service

In dit onderwerp bevat elke onderwerp die van toepassing rechtstreeks naar de service **SQL Data Warehouse** van Azure is. U kunt dit webpagina voor trefwoorden zoeken met behulp van **Ctrl + F**, om de onderwerpen huidige belangrijke te zoeken.




## <a name="new"></a>Nieuw

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 1 | [Back-ups van SQL Data Warehouse](sql-data-warehouse-backups.md) | Lees meer over SQL Data Warehouse ingebouwde back-ups waarmee u kunt een Azure SQL Data Warehouse terugzetten op een punt herstellen of een ander geografisch gebied. |


## <a name="updated-articles-sql-data-warehouse"></a>Bijgewerkte artikelen, SQL Data Warehouse

In deze sectie worden artikelen die onlangs zijn bijgewerkt, waar het bijwerken is groot of aanzienlijk. Voor elke bijgewerkte-artikel een ruw fragment van tekst bij de toegevoegde korting weergegeven. De artikelen zijn bijgewerkt binnen het datumbereik van **2016-08-22** in **2016-10-05**.

| &nbsp; | Artikel | Bijgewerkte tekst, fragment | Wanneer worden bijgewerkt |
| --: | :-- | :-- | :-- |
| 2 | [Gegevens van Azure-blobopslag laadt in SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | /-Om bij te houden van de bytes en bestanden selecteren r.command, s.request_id, r.status, count (distinct input_name) als nbr_files, optellen (s.bytes_processed) / 1024/1024 als gb_processed van sys.dm_pdw_exec_requests r inner join sys.dm_pdw_dms_external_work s op r.request_id s.request_id = waar r. label = ' CTAS: laden cso. DimProduct ' of r. label = ' CTAS: laden cso. De FactOnlineSales GROUP BY r.command, s.request_id, r.status ORDER BY nbr_files desc, gb_processed desc;  | 2016-09-07 |
| 3 | [SQL Data Warehouse herstellen](sql-data-warehouse-restore-database-overview.md) | **Kan ik een onderbroken data-warehouse herstellen?** Als u een data-warehouse die is onderbroken herstellen, moet u eerst terug online brengen. Nadat het datawarehouse weer online is, hebt u zeven dagen van herstellen punten waaruit u kunt kiezen. **Naar een geografische-redundante gebied herstellen** Als u de geografische-redundante opslag gebruikt, kunt u het datawarehouse terugzetten naar uw gepaarde datacenter in een ander geografisch gebied. Datawarehouse is uit de laatste dagelijkse back-up hersteld. **Tijdlijn herstellen** In de afgelopen zeven dagen, kunt u een database terugzetten in elk gewenst moment herstellen. Momentopnamen starten om de vier tot acht uur en beschikbaar zijn voor zeven dagen. Wanneer een momentopname ouder dan zeven dagen is, het verloopt en de punt herstellen is niet langer beschikbaar. **Kosten herstellen** De kosten van de opslagruimte voor herstelde datawarehouse gefactureerd Azure Premium Storage snelheid. Als u een herstelde data-warehouse onderbreekt, wordt geheven voor opslagruimte met het tarief weer dat Azure Premium opslag. Het voordeel van onderbreking is dat u bent niet boete | 2016-09-29 |





## <a name="get-started"></a>Aan de slag

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 4 | [Verificatie van Azure SQL datawarehouse](sql-data-warehouse-authentication.md) | Azure Active Directory (AAD) en SQL Server-verificatie naar Azure SQL Data Warehouse. |
| 5 | [Aanbevolen procedures voor het Azure SQL Data Warehouse](sql-data-warehouse-best-practices.md) | Aanbevelingen en aanbevolen procedures die u als u weten moet ontwikkelen van oplossingen voor Azure SQL Data Warehouse. Deze helpen u lukt. |
| 6 | [Stuurprogramma's voor SQL Azure datawarehouse](sql-data-warehouse-connection-strings.md) | Verbindingstekenreeksen met de en stuurprogramma's voor SQL Data Warehouse |
| 7 | [Verbinding maken met SQL Azure datawarehouse](sql-data-warehouse-connect-overview.md) | Hoe u vindt de server en de naam van de verbindingstekenreeks voor uw naar Azure SQL Data Warehouse |
| 8 | [Gegevens analyseren met Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md) | Gebruik Azure Machine Learning maken van een blog machine learning-model op basis van gegevens die zijn opgeslagen in Azure SQL Data Warehouse. |
| 9 | [Query Azure SQL Data Warehouse (sqlcmd)](sql-data-warehouse-get-started-connect-sqlcmd.md) | Query's uitvoeren Azure SQL Data Warehouse met de opdrachtregel hulpprogramma sqlcmd. |
| 10 | [Een SQL Data Warehouse-database maken met behulp van Transact-SQL (TSQL)](sql-data-warehouse-get-started-create-database-tsql.md) | Informatie over het maken van een Azure SQL Data Warehouse met TSQL |
| 11 | [Het maken van een ondersteuningsticket voor SQL Data Warehouse](sql-data-warehouse-get-started-create-support-ticket.md) | Het maken van een ondersteuningsticket in Azure SQL Data Warehouse. |
| 12 | [Gegevens met Azure gegevens Factory laden](sql-data-warehouse-get-started-load-with-azure-data-factory.md) | Leer hoe u gegevens met Azure gegevens Factory laden |
| 13 | [Gegevens met PolyBase in SQL Data Warehouse laden](sql-data-warehouse-get-started-load-with-polybase.md) | Lees wat PolyBase is en hoe u deze gebruiken voor gegevensopslag scenario's. |
| 14 | [Een Azure SQL datawarehouse maken](sql-data-warehouse-get-started-provision.md) | Informatie over het maken van een Azure SQL Data Warehouse in de portal van Azure |
| 15 | [SQL Data Warehouse via PowerShell maken](sql-data-warehouse-get-started-provision-powershell.md) | SQL Data Warehouse maken met behulp van PowerShell |
| 16 | [Gegevens met Power BI visualiseren](sql-data-warehouse-get-started-visualize-with-power-bi.md) | SQL Data Warehouse gegevens met Power BI visualiseren |
| 17 | [Query Azure SQL datawarehouse (Visual Studio)](sql-data-warehouse-query-visual-studio.md) | Query SQL datawarehouse met Visual Studio. |



## <a name="develop"></a>Ontwikkelen

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 18 | [Transacties voor SQL Data Warehouse optimaliseren](sql-data-warehouse-develop-best-practices-transactions.md) | Aanbevolen oefening richtlijnen voor het schrijven van efficiënt transactie-updates in Azure SQL Data Warehouse |
| 19 | [Gelijktijdigheid en werkbelasting management in SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md) | Inzicht in Azure SQL Data Warehouse gelijktijdigheid en werkbelasting beheer voor het ontwikkelen van oplossingen. |
| 20 | [Tabel maken als selecteren (CTAS) in SQL datawarehouse](sql-data-warehouse-develop-ctas.md) | Tips voor het coderen van de tabel maken als de select-instructie (CTAS) in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 21 | [Dynamische SQL in SQL datawarehouse](sql-data-warehouse-develop-dynamic-sql.md) | Tips voor het gebruik van dynamische SQL in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 22 | [Groeperen op opties in SQL Data Warehouse](sql-data-warehouse-develop-group-by-options.md) | Tips voor het implementeren van de groep door opties in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 23 | [Labels tot instrument query's in de SQL Data Warehouse gebruiken](sql-data-warehouse-develop-label.md) | Tips voor het gebruik van de etiketten tot instrument query's in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 24 | [Lussen in SQL datawarehouse](sql-data-warehouse-develop-loops.md) | Tips voor het Transact-SQL lussen en vervangt cursors in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 25 | [Opgeslagen procedures in SQL Data Warehouse](sql-data-warehouse-develop-stored-procedures.md) | Tips voor de uitvoering van opgeslagen procedures in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 26 | [Transacties in SQL datawarehouse](sql-data-warehouse-develop-transactions.md) | Tips voor het implementeren van transacties in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 27 | [Schema's door gebruiker gedefinieerd in SQL Data Warehouse](sql-data-warehouse-develop-user-defined-schemas.md) | Tips voor het gebruik van schema's Transact-SQL in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 28 | [Variabelen in SQL Data Warehouse toewijzen](sql-data-warehouse-develop-variable-assignment.md) | Tips voor het toewijzen van Transact-SQL-variabelen in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 29 | [Weergaven in SQL datawarehouse](sql-data-warehouse-develop-views.md) | Tips voor het gebruik van Transact-SQL-weergaven in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 30 | [Beslissingen en code technieken voor SQL Data Warehouse ontwerpen](sql-data-warehouse-overview-develop.md) | Ontwikkeling concepten, ontwerp, aanbevelingen en code technieken voor SQL Data Warehouse. |



## <a name="manage"></a>Beheren

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 31 | [Beheren rekenkracht in Azure SQL Data Warehouse (overzicht)](sql-data-warehouse-manage-compute-overview.md) | De schaal van de prestaties meer mogelijkheden in Azure SQL Data Warehouse. Met aanpassen DWUs wilt verkleinen of onderbreken en hervatten berekeningscluster resources om op te slaan kosten. |
| 32 | [Rekenkracht in Azure SQL Data Warehouse (Azure portal) beheren](sql-data-warehouse-manage-compute-portal.md) | Azure portal taken voor het beheren van rekenkracht. Schaal berekenen resources DWUs aanpassen. Of, onderbreken en hervatten resources om op te slaan kosten berekenen. |
| 33 | [Rekenkracht in Azure SQL Data Warehouse (PowerShell) beheren](sql-data-warehouse-manage-compute-powershell.md) | PowerShell-taken voor het beheren van rekenkracht. Schaal berekenen resources DWUs aanpassen. Of, onderbreken en hervatten resources om op te slaan kosten berekenen. |
| 34 | [Rekenkracht in Azure SQL Data Warehouse (REST) beheren](sql-data-warehouse-manage-compute-rest-api.md) | PowerShell-taken voor het beheren van rekenkracht. Schaal berekenen resources DWUs aanpassen. Of, onderbreken en hervatten resources om op te slaan kosten berekenen. |
| 35 | [Beheren rekenkracht in Azure SQL Data Warehouse (T-SQL)](sql-data-warehouse-manage-compute-tsql.md) | (T-SQL) Transact-SQL-taken aan schalen prestaties door DWUs aan te passen. Bespaar kosten door schaalbaarheid terug tijdens de niet-tijden. |
| 36 | [Uw werkzaamheden met DMVs controleren](sql-data-warehouse-manage-monitor.md) | Leer hoe u uw werkzaamheden met DMVs controleren. |
| 37 | [Databases in Azure SQL Data Warehouse beheren](sql-data-warehouse-overview-manage.md) | Overzicht van de SQL Data Warehouse databases beheren. Hulpmiddelen voor projectbeheer, DWUs en schalen prestaties, probleemoplossing queryprestaties, goede beveiligingsbeleid voor apparaten tot stand te brengen en een database herstellen vanuit beschadiging of een regionale storing bevat. |
| 38 | [Monitor met een gebruiker query's in Azure SQL Data Warehouse](sql-data-warehouse-overview-manage-user-queries.md) | Overzicht van de overwegingen, aanbevolen procedures en taken voor het controleren van de gebruiker query's in Azure SQL Data Warehouse |
| 39 | [SQL Data Warehouse herstellen](sql-data-warehouse-restore-database-overview.md) | Overzicht van de opties voor het herstellen van database voor het herstellen van een database in Azure SQL Data Warehouse. |
| 40 | [Een SQL Azure datawarehouse (Portal) herstellen](sql-data-warehouse-restore-database-portal.md) | Azure portal taken voor het herstellen van een Azure SQL Data Warehouse. |
| 41 | [Een SQL Azure datawarehouse (PowerShell) herstellen](sql-data-warehouse-restore-database-powershell.md) | PowerShell taken voor het herstellen van een Azure SQL Data Warehouse. |
| 42 | [Een SQL Azure datawarehouse (REST API) herstellen](sql-data-warehouse-restore-database-rest-api.md) | REST API taken voor het herstellen van een Azure SQL Data Warehouse. |



## <a name="tables-and-indexes"></a>Tabellen en indexen

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 43 | [Gegevenstypen voor tabellen in SQL Data Warehouse](sql-data-warehouse-tables-data-types.md) | Aan de slag met gegevenstypen voor Azure SQL Data Warehouse tabellen. |
| 44 | [Tabellen in SQL Data Warehouse distribueren](sql-data-warehouse-tables-distribute.md) | Aan de slag met tabellen in Azure SQL Data Warehouse distribueren. |
| 45 | [Tabellen in SQL Data Warehouse indexeren](sql-data-warehouse-tables-index.md) | Aan de slag met tabel indexering in Azure SQL Data Warehouse. |
| 46 | [Overzicht van tabellen in SQL Data Warehouse](sql-data-warehouse-tables-overview.md) | Aan de slag met Azure SQL-gegevenstabellen BTW-records. |
| 47 | [Tabellen in SQL Data Warehouse partitioneren](sql-data-warehouse-tables-partition.md) | Aan de slag met de tabel partitioneren in Azure SQL Data Warehouse. |
| 48 | [Statistieken over tabellen in SQL Data Warehouse beheren](sql-data-warehouse-tables-statistics.md) | Aan de slag met statistieken over tabellen in Azure SQL Data Warehouse. |
| 49 | [Tijdelijke tabellen in SQL Data Warehouse](sql-data-warehouse-tables-temporary.md) | Aan de slag met tijdelijke tabellen in Azure SQL Data Warehouse. |



## <a name="integrate"></a>Integreren

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 50 | [Azure gegevens Factory gebruiken met SQL datawarehouse](sql-data-warehouse-integrate-azure-data-factory.md) | Tips voor het gebruik van Azure gegevens Factory (ADF) met Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 51 | [Azure Machine Learning gebruiken met SQL datawarehouse](sql-data-warehouse-integrate-azure-machine-learning.md) | Zelfstudie voor het gebruik van Azure Machine Learning met Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 52 | [Azure Stream Analytics gebruiken met SQL datawarehouse](sql-data-warehouse-integrate-azure-stream-analytics.md) | Tips voor het gebruik van Azure Stream analyses met Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 53 | [Power BI met SQL datawarehouse gebruiken](sql-data-warehouse-integrate-power-bi.md) | Tips voor het gebruik van Power BI met Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 54 | [Gebruikmaken van andere services met SQL Data Warehouse](sql-data-warehouse-overview-integrate.md) | Hulpprogramma's en partners met oplossingen die zijn geïntegreerd met SQL Data Warehouse.  |



## <a name="load"></a>Laden

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 55 | [Gegevens van Azure-blobopslag laadt in Azure SQL Data Warehouse (Azure Data Factory genoemd)](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md) | Leer hoe u gegevens met Azure gegevens Factory laden |
| 56 | [Gegevens van Azure-blobopslag laadt in SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | Informatie over het gebruik van PolyBase gegevens van Azure-blobopslag in SQL Data Warehouse laden. Een paar tabellen van openbare gegevens laden in het schema Contoso detailhandel Data Warehouse. |
| 57 | [Gegevens uit SQL Server laadt in Azure SQL Data Warehouse (AZCopy)](sql-data-warehouse-load-from-sql-server-with-azcopy.md) | Bcp exporteren van gegevens uit SQL Server naar platte bestanden, AZCopy importeren van gegevens met Azure-blobopslag en PolyBase naar het nemen van de gegevens in Azure SQL Data Warehouse gebruikt. |
| 58 | [Gegevens uit SQL Server laadt in Azure SQL Data Warehouse (platte bestanden)](sql-data-warehouse-load-from-sql-server-with-bcp.md) | Voor een kleine gegevensgrootte, gebruikt bcp gegevens exporteren vanuit SQL Server naar platte bestanden en de gegevens rechtstreeks in Azure SQL Data Warehouse importeren. |
| 59 | [Gegevens laden uit SQL Server in Azure SQL Data Warehouse (SSIS)](sql-data-warehouse-load-from-sql-server-with-integration-services.md) | Ziet u hoe u een SQL Server Integration Services (SSIS)-pakket om gegevens van een groot aantal gegevensbronnen te SQL Data Warehouse verplaatsen maken. |
| 60 | [Gegevens met PolyBase in SQL Data Warehouse laden](sql-data-warehouse-load-from-sql-server-with-polybase.md) | Bcp exporteren van gegevens uit SQL Server naar platte bestanden, AZCopy importeren van gegevens met Azure-blobopslag en PolyBase naar het nemen van de gegevens in Azure SQL Data Warehouse gebruikt. |
| 61 | [Handleiding voor het gebruik van PolyBase in SQL Data Warehouse](sql-data-warehouse-load-polybase-guide.md) | Richtlijnen en aanbevelingen voor het gebruik van PolyBase in SQL Data Warehouse scenario's. |
| 62 | [Voorbeeldgegevens in SQL Data Warehouse laden](sql-data-warehouse-load-sample-databases.md) | Voorbeeldgegevens in SQL Data Warehouse laden |
| 63 | [Gegevens met bcp laden](sql-data-warehouse-load-with-bcp.md) | Leer welke bcp is en hoe u deze gebruiken voor gegevensopslag scenario's. |
| 64 | [Gegevens laadt in Azure SQL Data Warehouse](sql-data-warehouse-overview-load.md) | Informatie over de gebruikelijke scenario's voor gegevens laden in SQL Data Warehouse. Hierbij met PolyBase, Azure-blobopslag, bestanden en schijf verzending. U kunt ook hulpprogramma's van derden gebruiken. |



## <a name="migrate"></a>Migreren

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 65 | [Uw SQL-code migreren naar SQL Data Warehouse](sql-data-warehouse-migrate-code.md) | Tips voor het migreren van uw SQL-code naar Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 66 | [Migreren van uw gegevens](sql-data-warehouse-migrate-data.md) | Tips voor het migreren van uw gegevens naar Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 67 | [Data Warehouse migratiehulpprogramma (Preview)](sql-data-warehouse-migrate-migration-utility.md) | Migreren naar SQL datawarehouse. |
| 68 | [Migreren van uw schema naar SQL Data Warehouse](sql-data-warehouse-migrate-schema.md) | Tips voor het migreren van uw schema naar Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |
| 69 | [Uw oplossing migreren naar SQL Data Warehouse](sql-data-warehouse-overview-migrate.md) | De richtlijnen van de migratie voor uw oplossing te brengen naar Azure SQL Data Warehouse-platform. |



## <a name="partners"></a>Partners

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 70 | [SQL Data Warehouse business intelligence partners](sql-data-warehouse-partner-business-intelligence.md) | Lijsten van derden business intelligence partners met oplossingen die ondersteuning bieden voor SQL Data Warehouse. |
| 71 | [SQL Data Warehouse gegevens integratie van partners](sql-data-warehouse-partner-data-integration.md) | Lijsten met externe partners met oplossingen die ondersteuning bieden voor Azure SQL Data Warehouse voor integratie van gegevens. |
| 72 | [SQL Data Warehouse data management partners](sql-data-warehouse-partner-data-management.md) | Lijsten met gegevens van derden management partners met oplossingen die ondersteuning bieden voor SQL Data Warehouse. |



## <a name="reference"></a>Verwijzing

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 73 | [Naslaginformatie voor SQL Data Warehouse](sql-data-warehouse-overview-reference.md) | Inhoud referentiekoppelingen voor SQL Data Warehouse. |
| 74 | [PowerShell-cmdlets en REST API's voor SQL Data Warehouse](sql-data-warehouse-reference-powershell-cmdlets.md) | De bovenste PowerShell-cmdlets voor Azure SQL Data Warehouse waaronder over het onderbreken en hervatten van een database te vinden. |
| 75 | [Elementen van de taal](sql-data-warehouse-reference-tsql-language-elements.md) | Lijst met koppelingen naar naslaginformatie voor de Transact-SQL-taal-elementen die wordt gebruikt voor SQL Data Warehouse. |
| 76 | [Transact-SQL-onderwerpen](sql-data-warehouse-reference-tsql-statements.md) | Koppelingen naar naslaginformatie voor de Transact-SQL-onderwerpen die worden gebruikt door SQL Data Warehouse. |
| 77 | [Systeemweergaven](sql-data-warehouse-reference-tsql-system-views.md) | Koppelingen naar systeem weergaven inhoud voor SQL Data Warehouse. |



## <a name="security"></a>Beveiliging

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 78 | [SQL datawarehouse - Downlevel-clients ondersteuning voor controle en dynamische gegevens Masking](sql-data-warehouse-auditing-downlevel-clients.md) | Meer informatie over SQL Data Warehouse downlevel clients ondersteuning voor het controleren van gegevens |
| 79 | [Controle in SQL Azure datawarehouse](sql-data-warehouse-auditing-overview.md) | Aan de slag met Azure SQL Data Warehouse controle |
| 80 | [Aan de slag met transparante gegevens versleuteling (TDE) in SQL Data Warehouse](sql-data-warehouse-encryption-tde.md) | Transparante gegevensversleuteling (TDE) in SQL datawarehouse |
| 81 | [Aan de slag met transparante gegevens versleuteling (TDE)](sql-data-warehouse-encryption-tde-tsql.md) | Transparante gegevensversleuteling (TDE) in SQL datawarehouse (T-SQL) |
| 82 | [Een database in SQL Data Warehouse beveiligen](sql-data-warehouse-overview-manage-security.md) | Tips voor het beveiligen van een database in Azure SQL Data Warehouse voor het ontwikkelen van oplossingen. |



## <a name="miscellaneous"></a>Diverse

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 83 | [Visual Studio-2015 en SSDT voor SQL datawarehouse installeren](sql-data-warehouse-install-visual-studio.md) | Visual Studio en SQL Server Development Tools (SSDT) voor SQL Azure datawarehouse installeren |
| 84 | [Migratie naar Premium opslag Details](sql-data-warehouse-migrate-to-premium-storage.md) | Instructies voor het migreren van een bestaande SQL Data Warehouse naar premium-opslag |
| 85 | [Aan de slag met bedreiging detectie](sql-data-warehouse-security-threat-detection.md) | Hoe u aan de slag met bedreiging detectie |
| 86 | [SQL Data Warehouse Capaciteitslimieten](sql-data-warehouse-service-capacity-limits.md) | Maximumwaarden voor verbindingen, databases, tabellen en query's voor SQL Data Warehouse. |
| 87 | [Azure SQL datawarehouse probleemoplossing](sql-data-warehouse-troubleshoot.md) | Voor probleemoplossing Azure SQL datawarehouse. |

