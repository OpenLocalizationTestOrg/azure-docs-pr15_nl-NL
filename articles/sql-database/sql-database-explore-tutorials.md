<properties
   pageTitle="Kennismaken met Azure SQL Database-zelfstudies | Microsoft Azure"
   description="Meer informatie over de SQL-Database-functies en mogelijkheden"
   keywords=""
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
   ms.workload="data-management"
   ms.date="08/24/2016"
   ms.author="carlrab"/>
   
# <a name="explore-azure-sql-database-tutorials"></a>Azure SQL Database-zelfstudies verkennen

De onderstaande koppelingen gaat u naar een overzicht van elk functiegebied vermelden en een eenvoudige stap door de begin-zelfstudie voor elk gebied. Voor oplossing beperkt snel aan de slag die het gebruik van de SQL-Database in geen volledige oplossing op basis van de echte wereld scenario's demonstreren, raadpleegt u [Azure SQL Database oplossing snelle wordt gestart](sql-database-solution-quick-starts.md).

## <a name="using-sql-server-management-studio"></a>Gebruik van SQL Server Management Studio

In de volgende zelfstudies leert u over het gebruik van SQL Server Management Studio beheren en query Azure SQL-Database.


> [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de nieuwste versie van Management Studio gebruiken om u te blijven synchroon met updates voor Microsoft Azure en SQL-Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).


| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Verbinding maken met behulp van een principal aanmelding serverniveau van Azure SQL Database](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| In deze zelfstudie leert u hoe u verbinding maakt met Azure SQL-Database met een serverniveau principal aanmelding.|
| [Verbinding maken met Azure SQL-Database als een gebruiker](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | In deze zelfstudie leert u hoe u verbinding maakt met een Azure SQL-database met een database op gebruikersniveau gebruikersaccount.|
||||

## <a name="elastic-pools"></a>Elastische van toepassingen

In de volgende zelfstudies leert u over het gebruik van [elastische toepassingen](sql-database-elastic-pool.md) voor het beheren van de prestatiedoelstellingen voor meerdere databases die sterk uiteen verschillende en onverwachte gebruikspatronen hebben.

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Een elastische toepassingen maken](sql-database-elastic-pool-create-portal.md) | In deze zelfstudie leert u hoe u een scalable toepassingen van Azure SQL-databases te maken. |
| [Monitor met een elastische-database](sql-database-elastic-pool-manage-portal.md#elastic-database-monitoring) | In deze zelfstudie leert u hoe u een afzonderlijke elastische-database voor mogelijke problemen met het bewaken. |
| [Een waarschuwing toevoegen aan een resourceweergave van toepassingen](sql-database-elastic-pool-manage-portal.md#add-an-alert-to-a-pool-resource) | In deze zelfstudie leert u hoe u regels toevoegt aan resources die e-mail naar personen of waarschuwing tekenreeksen naar URL eindpunten verzenden wanneer de resource raakt een gebruik drempel die u hebt ingesteld. |
| [Een database verplaatsen naar een elastische toepassingen](sql-database-elastic-pool-manage-portal.md#move-a-database-into-an-elastic-pool) | In deze zelfstudie leert u hoe u een database verplaatsen naar een elastische toepassingen. |
| [Een database verplaatsen uit een elastische toepassingen](sql-database-elastic-pool-manage-portal.md#move-a-database-out-of-an-elastic-pool) | In deze zelfstudie leert u hoe u een database verplaatsen uit een elastische toepassingen. |
| [Instellingen van de prestaties van een groep wijzigen](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) | In deze zelfstudie leert u hoe u de prestaties en opslag limieten voor een groep aanpast. |
||||

## <a name="elastic-database-jobs"></a>Elastische database taken

In de volgende zelfstudies leert u over het gebruik van [elastische database taken](sql-database-elastic-jobs-overview.md).

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Aan de slag met elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md) | In deze zelfstudie leert u hoe u de mogelijkheden van elastische Databasehulpmiddelen met gebruikmaking van een eenvoudige een laptopgeheugen toepassing gebruikt. |
| [Aan de slag met Azure SQL-Database elastische taken](sql-database-elastic-jobs-getting-started.md)  | In deze zelfstudie leert u hoe aan het maken en beheren van taken die een groep van gerelateerde databases beheren.  | 
||||

## <a name="elastic-queries"></a>Elastische query 's

In de volgende zelfstudies, wordt u meer informatie over het [elastische query's](sql-database-elastic-query-overview.md)uitvoeren. 

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Query's uitvoeren door een horizontaal gepartitioneerde (een laptopgeheugen) database)](sql-database-elastic-query-getting-started.md) | In deze zelfstudie leert u hoe u rapporten maakt in alle databases in een horizontaal gepartitioneerde (een laptopgeheugen) database met behulp van [elastische query](sql-database-elastic-query-overview.md) |
| [Query's uitvoeren in een verticaal gepartitioneerde database)](sql-database-elastic-query-getting-started-vertical.md#create-database-objects) | In deze zelfstudie leert u hoe u rapporten maakt in alle databases in een verticaal database met behulp van [elastische query](sql-database-elastic-query-overview.md) |
| [Migreren van een bestaande database schalen](sql-database-elastic-convert-to-use-elastic-tools.md)| In deze zelfstudie leert u horizontaal schalen (shard) een SQL Azure-database. |
||||

## <a name="performance-optimization"></a>Optimalisatie van prestaties

In de volgende zelfstudies, wordt u meer informatie over het optimaliseren van de [prestaties van één-databases](sql-database-performance-guidance.md). Zie voor het optimaliseren van de prestaties van meerdere databases, [elastische toepassingen](#elastic-pools).

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Het serviceniveau laag en prestaties van uw database wijzigen](sql-database-scale-up.md#change-the-service-tier-and-performance-level-of-your-database) | In deze zelfstudie leert u hoe u vergroten of verkleinen van de prestaties van een Servicelagen met Azure SQL-database. |
| [SQL-Database Advisor Query prestaties inzicht](sql-database-performance.md#performance-overview) | In deze zelfstudie leert u hoe openen en gebruiken van de SQL-Database Advisor Query prestaties inzicht.|
| [SQL-Database Advisor prestaties aanbevelingen](sql-database-advisor.md#viewing-recommendations) | In deze zelfstudie leert u hoe bekijken en SQL-Database Advisor prestaties aanbevelingen toepassen. |
| [Bovenste CPU door andere query's bekijken](sql-database-query-performance.md#review-top-cpu-consuming-queries)| In deze zelfstudie leert u hoe u met SQL-Database Advisor Query prestaties inzicht bovenste CPU door andere query's bekijken.|
| [Bekijken welke Querydetails zijn afzonderlijke](sql-database-query-performance.md#viewing-individual-query-details)| In deze zelfstudie leert u hoe u met SQL-Database Advisor Query prestaties inzicht afzonderlijke query Prestatiedetails weergeven.|
||||

## <a name="sql-database-migration-and-archive"></a>SQL-databasemigratie en archiveren 

In de volgende zelfstudies, wordt u meer informatie over het [migreren van een bestaande SQL Server-database met Azure SQL-Database](sql-database-cloud-migrate.md).

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Compatibiliteitsproblemen met behulp van SQL Server Data Tools voor Visual Studio detecteren](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md#detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | In deze zelfstudie leert u hoe u SQL Server Data Tools voor Visual Studio gebruiken om te bepalen van Azure SQL Database-compatibiliteit. |
| [Compatibiliteitsproblemen met behulp van SQL Server Data Tools voor Visual Studio op te lossen](sql-database-cloud-migrate-fix-compatibility-issues-ssdt#fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio) | In deze zelfstudie leert u hoe u SQL Server Data Tools voor Visual Studio gebruiken om op te lossen compatibiliteitsproblemen Azure SQL-Database. |
| [SQL-Database compatibiliteit met SqlPackage.exe bepalen](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md#using-sqlpackageexe) | In deze zelfstudie leert u hoe u het hulpprogramma SQLPackage.exe gebruikt om te bepalen van Azure SQL Database-compatibiliteit.|
| [SQL-Database compatibiliteit met SSMS bepalen](sql-database-cloud-migrate-determine-compatibility-ssms.md#using-sql-server-management-studio) |In deze zelfstudie leert u hoe u SQL Server Management Studio gebruiken om te bepalen van Azure SQL Database-compatibiliteit.|
| [SQL Server-database migreren met SQL-Database met Database implementeren naar de Wizard van Microsoft Azure-Database](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md#use-the-deploy-database-to-microsoft-azure-database-wizard) | In deze zelfstudie leert u hoe u een compatibele SQL Server-database migreren met Azure SQL-Database met de Database implementeren naar de Wizard van Microsoft Azure-Database in SQL Server Management Studio.
| [Een SQL Server-database exporteren naar een Bacpac-bestand met SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | In deze zelfstudie leert u hoe u kunt een compatibele SQL Server-database exporteren naar een Bacpac-bestand met de Wizard exporteren laag toepassing in SQL Server Management Studio.|
| [Een SQL Server-database exporteren naar een Bacpac-bestand met SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | In deze zelfstudie leert u hoe u kunt een compatibele SQL Server-database exporteren naar een Bacpac-bestand met het hulpprogramma SQLPackage.exe.|
| [Een Bacpac-bestand importeren in Azure SQL-Database met SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | In deze zelfstudie leert u hoe u een database importeren in Azure SQL-Database uit een Bacpac-bestand met de Wizard exporteren laag toepassing in SQL Server Management Studio. |
| [Een Bacpac-bestand importeren in Azure SQL-Database met SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md#import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage) | In deze zelfstudie leert u hoe u een database importeren in Azure SQL-Database uit een Bacpac-bestand met het hulpprogramma SQLPackage. |
| [Een Bacpac-bestand importeren in Azure SQL-Database met behulp van de Azure portal](sql-database-import.md) | In deze zelfstudie leert u hoe u een database importeren in Azure SQL-Database uit een Bacpac-bestand dat is opgeslagen in een Azure blob met behulp van de Azure-Portal.|
| [Een Bacpac-bestand importeren in Azure SQL-Database via PowerShell](sql-database-import-powershell.md) | In deze zelfstudie leert u hoe u een database importeren in Azure SQL-Database uit een Bacpac-bestand via PowerShell.|
| [Een Azure SQL-database met behulp van de Azure portal archiveren](sql-database-export.md#export-your-database) | In deze zelfstudie leert u hoe u een Azure SQL-database naar een Bacpac-bestand met behulp van de Azure portal archiveren. |
| [Een Azure SQL-database via PowerShell archiveren](sql-database-export-powershell.md) | In deze zelfstudie leert u hoe u een Azure SQL-database naar een Bacpac-bestand via PowerShell archiveren. |
| [Kopieer een Azure SQL-database met behulp van de Azure portal](sql-database-copy.md#copy-your-sql-database) | In deze zelfstudie leert u hoe u een Azure SQL-database met behulp van de Azure portal kopiëren. |
| [Kopieer een Azure SQL-database via PowerShell](sql-database-copy-powershell#copy-your-sql-database) | In deze zelfstudie leert u hoe u een Azure SQL-database via PowerShell kopiëren. |
| [Kopieer een met Transact-SQL Azure SQL-database](sql-database-copy-transact-sql.md#copy-your-sql-database) | In deze zelfstudie leert u hoe u het kopiëren van een Azure SQL-database met Transact-SQL. |
||||

##<a name="develop"></a>Ontwikkelen

In de volgende zelfstudies leert u over het [Ontwikkelen van SQL-Database](sql-database-develop-overview.md) en gebruiken van [connectivity bibliotheken](sql-database-libraries.md).

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Verbinding maken met SQL-Database via .NET (C#)](sql-database-develop-dotnet-simple.md) | In deze zelfstudie leert u hoe u verbinding maakt met een Azure SQL-database met C#. |
| [Verbinding maken met SQL-Database via Java](sql-database-develop-java-simple.md) | In deze zelfstudie leert u hoe u verbinding maakt met een Java met Azure SQL-database. |
| [Verbinding maken met SQL-Database via Node.js](sql-database-develop-nodejs-simple.md) | In deze zelfstudie leert u hoe u verbinding maakt met een Node.js met Azure SQL-database. |
| [Verbinding maken met SQL-Database via PHP](sql-database-develop-php-simple.md) | In deze zelfstudie leert u hoe u verbinding maakt met een Azure SQL-database met PHP. |
| [Verbinding maken met SQL-Database via Python](sql-database-develop-python-simple.md) | In deze zelfstudie leert u hoe u verbinding maakt met een Python met Azure SQL-database. |
| [Verbinding maken met SQL-Database via Ruby](sql-database-develop-ruby-simple.md) | In deze zelfstudie leert u hoe u verbinding maakt met een Ruby met Azure SQL-database. |
||||
 
## <a name="database-access"></a>Toegang tot de database

In de volgende zelfstudies leert u over het [maken en aanmeldingen en gebruikers beheren](sql-database-manage-logins.md).

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Een regel voor het niveau van de server firewall van Azure SQL-Database met behulp van de Azure portal maken](sql-database-configure-firewall-settings.md)  | In deze zelfstudie leert u hoe u een SQL-Database serverniveau firewall met behulp van de Azure portal configureert.  |
| [Een database op gebruikersniveau firewallregel maken met Transact-SQL](sql-database-configure-firewall-settings-tsql.md#database-level-firewall-rules) | In deze zelfstudie leert u hoe u een database op gebruikersniveau firewallregel maken met Transact-SQL.|
| [Serverniveau firewallregels met Transact-SQL beheren](sql-database-configure-firewall-settings-tsql.md#manage-server-level-firewall-rules-through-transact-sql) | In deze zelfstudie leert u hoe u het beheren van een Azure SQL-Database serverniveau firewall met Transact-SQL.|
| [Serverniveau firewallregels via PowerShell beheren](sql-database-configure-firewall-settings-powershell.md#manage-firewall-rules-using-powershell) | In deze zelfstudie leert u hoe u een firewall voor het niveau van de server van Azure SQL Database via PowerShell beheert.|
| [Gebruik van de REST API serverniveau firewallregels beheren](sql-database-configure-firewall-settings-rest.md#manage-firewall-rules-using-the-service-management-rest-api) | In deze zelfstudie leert u hoe u het beheren van een Azure SQL-Database serverniveau firewall met de API opnieuw instellen.|
| [Verbinding maken met behulp van een principal aanmelding serverniveau van Azure SQL Database](sql-database-get-started-security.md#connect-to-azure-sql-database-using-a-server-level-principal-login)| In deze zelfstudie leert u hoe u verbinding maakt met Azure SQL-Database met een serverniveau principal aanmelding.|
| [Databasetoegang verlenen tot een aanmelding] (sql-database-manage-logins.md#granting-database-access-to-a-login() | In deze zelfstudie leert u hoe u het verlenen van toegang tot de database aan een aanmelding serverniveau.|
| [Nieuwe databasegebruiker met SSMS maken](sql-database-get-started-security.md#create-new-database-user-using-ssms) | In deze zelfstudie leert u hoe u een nieuwe databasegebruiker maakt in een bestaande database SSMS gebruiken.|
| [Nieuwe database db_owner gebruikersmachtigingen verlenen](sql-database-get-started-security.md#grant-new-database-user-dbowner-permissions) | In deze zelfstudie leert u hoe u een bestaande database db_owner gebruikersmachtigingen verlenen.|
| [Verbinding maken met Azure SQL-Database als een gebruiker](sql-database-get-started-security.md#connect-to-azure-sql-database-as-a-user) | In deze zelfstudie leert u hoe u verbinding maakt met een Azure SQL-database met een database op gebruikersniveau gebruikersaccount.|
||||


## <a name="data-security"></a>Beveiliging van gegevens

In de volgende zelfstudies, wordt u meer informatie over het [beveiligen van Azure SQL Database-gegevens](sql-database-security.md). 

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Bedreiging detectie voor de database met behulp van de Azure portal inschakelen](sql-database-threat-detection-get-started.md#set-up-threat-detection-for-your-database) | In deze zelfstudie leert u hoe bedreiging detectie in Azure portal voor uw database instellen.|
| [Beveiligen van vertrouwelijke gegevens uisng altijd versleuteld](sql-database-always-encrypted-azure-key-vault.md) | In deze zelfstudie gebruikt u de wizard altijd versleuteld om gevoelige gegevens in een Azure SQL-database te beveiligen.|
| [Secure laten gegevens transparante codering](https://msdn.microsoft.com/library/dn948096.aspx)| In deze zelfstudie leert u hoe transparante gegevens-versleuteling om laten gegevens te beveiligen.|
| [Een kolom met gegevens versleutelen](https://msdn.microsoft.com/library/ms179331.aspx)| In deze zelfstudie leert u hoe u een kolom met gegevens met behulp van Transact-SQL versleutelen.|
| [Dynamische gegevens masking instellen](sql-database-dynamic-data-masking-get-started.md#set-up-dynamic-data-masking-for-your-database-using-the-azure-portal)  | In deze zelfstudie leert u hoe u dynamische gegevens masking voor uw Azure SQL-database instelt. |
||||

## <a name="business-continuity-and-query-scale-out"></a>Bedrijfscontinuïteit en Query schalen

In de volgende zelfstudies, wordt u meer informatie over het [geografische herstellen en actieve geografische-herhaling](sql-database-business-continuity.md) gebruiken om te reccover van fouten, voor bedrijfscontinuïteit en voor de query schalen.

| Zelfstudie  | Beschrijving  |
|---|---|---|
| [Een Azure SQL-Database herstellen naar een vorig punt in tijd in de Portal van Azure](sql-database-point-in-time-restore-portal.md)| In deze zelfstudie leert u hoe u uw database op een eerder in tijd met behulp van de Azure portal herstelt.|
| [Een Azure SQL-Database naar een vorig punt in tijd met PowerShell herstellen](sql-database-point-in-time-restore-powershell.md) | In deze zelfstudie leert u hoe u uw database terugzetten op een eerder in tijd via PowerShell|
| [Een verwijderde Azure SQL-database met behulp van de Portal Azure terugzetten](sql-database-restore-deleted-database-portal.md) | In deze zelfstudie leert u hoe u een verwijderde database met behulp van de Azure portal herstelt.|
| [Een verwijderde Azure SQL-database via de PowerShell terugzetten](sql-database-restore-deleted-database-powershell.md) | In deze zelfstudie leert u hoe u herstelt een verwijderde database via PowerShell.|
| [Geografische-replicatie configureren voor Azure SQL-Database met behulp van de Azure portal](sql-database-geo-replication-portal.md)| In deze zelfstudie leert u hoe actieve geografische-replicatie met behulp van de Azure portal configureren.|
| [Geografische-replicatie configureren voor Azure SQL-Database via PowerShell](sql-database-geo-replication-powershell.md)| In deze zelfstudie leert u hoe u het actieve geografische-replicatie configureren via PowerShell.|
| [Geografische-replicatie configureren voor Azure SQL-Database met Transact-SQL](sql-database-geo-replication-transact-sql.md)| In deze zelfstudie leert u hoe u het actieve geografische-replicatie configureren met Transact-SQL.|
| [Een failover of niet gepland initiëren voor Azure SQL-Database met behulp van de Azure portal](sql-database-geo-replication-failover-portal.md) | In deze zelfstudie leert u hoe u foutherstel met een geografische gerepliceerd secundaire replica met behulp van de Azure portal.|
| [Een failover of niet gepland initiëren voor Azure SQL-Database via PowerShell](sql-database-geo-replication-failover-powershell.md) | In deze zelfstudie leert u hoe u foutherstel met een geografische gerepliceerd secundaire replica via PowerShell.|
| [Een failover of niet gepland initiëren voor Azure SQL-Database met Transact-SQL](sql-database-geo-replication-failover-transact-sql.md) | In deze zelfstudie leert u hoe u foutherstel naar een geografische gerepliceerd secundaire replica Transact-SQL.|
||||

## <a name="data-sync"></a>Gegevens synchroniseren

In deze zelfstudie leert u over [Gegevens synchroniseren](http://download.microsoft.com/download/4/E/3/4E394315-A4CB-4C59-9696-B25215A19CEF/SQL_Data_Sync_Preview.pdf).

| Zelfstudie  | Beschrijving  |
|---|---|---| 
| [Aan de slag met de synchronisatie van Azure SQL-gegevens (Preview)](sql-database-get-started-sql-data-sync.md)  | In deze zelfstudie leert u de grondbeginselen van Azure SQL gegevens synchroniseren met behulp van de Azure klassieke Portal. |
||||

## <a name="next-steps"></a>Volgende stappen

[Kennismaken met Azure SQL Database-oplossing snel aan de slag](sql-database-solution-quick-starts.md)
