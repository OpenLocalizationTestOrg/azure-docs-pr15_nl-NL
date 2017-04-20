<properties
    pageTitle="Alle onderwerpen voor SQL-Database service | Microsoft Azure"
    description="Tabel met alle onderwerpen voor de Azure-service met SQL-Database die zich op http://azure.microsoft.com/documentation/articles/, titel en beschrijving de naam."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="genemi"/>


# <a name="all-topics-for-azure-sql-database-service"></a>Alle onderwerpen voor Azure SQL Database-service

In dit onderwerp bevat elke onderwerp die van toepassing rechtstreeks naar de **SQL-Database** -service van Azure is. U kunt dit webpagina voor trefwoorden zoeken met behulp van **Ctrl + F**, om de onderwerpen huidige belangrijke te zoeken.




## <a name="new"></a>Nieuw

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 1 | [Daxko/CSI Azure gebruikt de ontwikkelingscyclus en en de klantenservices en de prestaties verbeteren](sql-database-implementation-daxko.md) | Meer informatie over hoe Daxko/CSI SQL-Database gebruikt de ontwikkelingscyclus en en de klantenservices en de prestaties verbeteren |
| 2 | [Azure biedt GEP wereldwijde bereik en efficiënter](sql-database-implementation-gep.md) | Meer informatie over hoe GEP SQL-Database gebruikt om meer globale klanten bereiken en efficiëntie vergroot |
| 3 | [Met Azure is SnelStart snel de business-services met een snelheid van 1000 nieuwe Azure SQL-Databases per maand uitgevouwen](sql-database-implementation-snelstart.md) | Meer informatie over hoe SnelStart SQL-Database gebruikt om snel uitgevouwen de business-services met een snelheid van 1000 nieuwe Azure SQL-Databases per maand |
| 4 | [Umbraco gebruikmaakt van Azure SQL-Database naar snel inrichten en schaal van services voor duizenden tenants in de cloud](sql-database-implementation-umbraco.md) | Meer informatie over het gebruik van SQL-Database voor het inrichten van snel in Umbraco en schaal services voor duizenden tenants in de cloud |
| 5 | [Azure SQL-Database met PowerShell beheren](sql-database-manage-powershell.md) | Beheer van de Azure SQL-Database met PowerShell. |
| 6 | [SSMS ondersteuning voor Azure AD MFA met SQL-Database en SQL Data Warehouse](sql-database-ssms-mfa-authentication.md) | Gebruik meerdere meegenomen verificatie met SSMS voor SQL-Database en SQL datawarehouse. |
| 7 | [Waarin u uitlegt Database transactie-eenheden (DTUs) en elastische Database transactie-eenheden (eDTUs)](sql-database-what-is-a-dtu.md) | Informatie over wat een Azure SQL-Database is transactie-eenheid. |


## <a name="updated-articles-sql-database"></a>Bijgewerkte artikelen, SQL-Database

In deze sectie worden artikelen die onlangs zijn bijgewerkt, waar het bijwerken is groot of aanzienlijk. Voor elke bijgewerkte-artikel een ruw fragment van tekst bij de toegevoegde korting weergegeven. De artikelen zijn bijgewerkt binnen het datumbereik van **2016-08-22** in **2016-10-05**.

| &nbsp; | Artikel | Bijgewerkte tekst, fragment | Wanneer worden bijgewerkt |
| --: | :-- | :-- | :-- |
| 8 | [Azure SQL-Database met PowerShell beheren](sql-database-command-line-tools.md) | Een resourcegroep maken voor onze SQL-Database en verwante Azure resources met de cmdlet New-AzureRmResourceGroup (https://msdn.microsoft.com/library/azure/mt759837.aspx). *$resourceGroupName = "resourcegroup1" $resourceGroupLocation = "northcentralus" nieuw-AzureRmResourceGroup-naam $resourceGroupName-locatie $resourceGroupLocation* Zie voor meer informatie Azure PowerShell gebruiken met Azure resourcemanager (.. / powershell-azure-resource-manager.md.). Zie voor een voorbeeldscript maken een SQL-database PowerShell-script (sql-database-get-gestart-powershell.md create-a-sql-database-powershell-script). **Een SQL-databaseserver maken** Maak een SQL-databaseserver met de cmdlet New-AzureRmSqlServer (https://msdn.microsoft.com/library/azure/mt603715.aspx). *Server1* vervangen door de naam van uw mailserver. Namen van servers voor moet uniek zijn voor alle Azure SQL Database-servers. Als de servernaam al gebruikt wordt, krijgt u een fout. Deze opdracht kan enkele minuten duren om te voltooien. De resou | 2016-09-14 |
| 9 | [Kopieer een Azure SQL-database via PowerShell](sql-database-copy-powershell.md) |   SQL-databasebron (de bestaande database als u wilt kopiëren)--$sourceDbName = "BW1" $sourceDbServerName = "server1" $sourceDbResourceGroupName = "rg1" SQL-database kopiëren (de nieuwe database wilt maken)--$copyDbName = "db1_copy" $copyDbServerName "server2" = $copyDbResourceGroupName = "rg2" kopiëren een database met de server--nieuw AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - servernaam $sourceDbServerName - databasenaam $sourceDbName - CopyDatabaseName $copyDbName een database kopiëren naar een andere server--nieuw AzureRmSqlDatabaseCopy - ResourceGroupName $sourceDbResourceGroupName - servernaam $sourceDbServerName - databasenaam $sourceDbName - CopyResourceGroupName $copyDbResourceGroupName - CopyServerName $copyDbServerName - CopyDatabaseName $copyDbName een database kopiëren naar een groep elastische database--$poolName = "pool1" nieuw-AzureRmSqlDatabaseCopy - ResourceGroupName $ sourceDbResourceGroupName - servernaam $sourceDbServerName - databasenaam $sourceDbName - CopyReso | 2016-09-08 |
| 10 | [Een toepassingen elastische database maken met C#](sql-database-elastic-pool-create-csharp.md) |   Console.WriteLine ("Server firewall...");  FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule (_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);  Console.WriteLine ("Server firewall:" + fwr. FirewallRule.Id);  Console.WriteLine ("elastische groep...').  ElasticPoolCreateOrUpdateResponse epr (eindpuntreferentie) = CreateOrUpdateElasticDatabasePool (_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);  Console.WriteLine ("elastische toepassingen:" + epr (eindpuntreferentie). ElasticPool.Id);  Console.WriteLine("Database...");  DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase (_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);  Console.WriteLine ("Database:" + dbr. Database.Id);  Console.WriteLine ("Druk op een toets om door te gaan...");  Console.ReadKey();  } statische ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupNa | 2016-09-14 |
| 11 | [PowerShell-script om aan te duiden databases die geschikt zijn voor een elastische database-toepassingen](sql-database-elastic-pool-database-assessment-powershell.md) | Vreemde valuta **voor elastische database-groep, die we uitsluiten modelweergave en alle databases die zich al in een groep. U kunt meer databases in de onderstaande lijst uitgesloten naar wens toevoegen** $ListOfDBs = $servername Get-AzureRmSqlDatabase - servernaam. Split('.') 0 - ResourceGroupName $ResourceGroupName / waar-Object {$_. Databasenaam - notin ("de hoofdlijst") - en $_. CurrentServiceLevelObjectiveName - notin ("ElasticPool")- en $_. CurrentServiceObjectiveName - notin ("ElasticPool")} $outputConnectionString = "gegevensbron = $outputServerName; geïntegreerde beveiliging = false; initiële catalogus = $outputdatabaseName; Gebruikers-Id = $outputDBUsername; Wachtwoord $outputDBpassword = "$destinationTableName ="resource_stats_output" **maken van een tabel in de Uitvoerdatabase voor de doelstellingen verzameling** $sql =" als NOT EXISTS (Selecteer * uit sys.objects waar object_id OBJECT_ID(N'$($destinationTableName)') = te typen en (N'U')) BEGIN maken tabel $($destinationTableName) (databasenaam varchar(128), trage varchar(20), end_time datetime, avg_cpu toegestane achterstand, Gem_ | 2016-09-29 |
| 12 | [SQL-Database zelfstudie: een SQL-database maken in minuten met behulp van de Azure-portal](sql-database-get-started.md) |   ! Nieuwe sql-database 1 (. / media/sql-database-get-started/sql-database-new-database-1.png) 3. Klik op **SQL-Database** om te openen van het blad SQL-Database. De inhoud op deze blade varieert afhankelijk van het nummer van uw abonnementen en uw bestaande objecten (zoals bestaande servers).  ! Nieuwe sql-database 2 (. / media/sql-database-get-started/sql-database-new-database-2.png) 4. Geef een naam voor de eerste database - zoals 'mijn-database' in het tekstvak **naam van de Database** . Een groen vinkje geeft aan dat u de naam van een geldige hebt opgegeven.  ! Nieuwe sql-database 3 (. / media/sql-database-get-started/sql-database-new-database-3.png) 5. Als u meerdere abonnementen hebt, selecteert u een abonnement. 6. Klik onder **resourcegroep**, klik op **Nieuw** en geef een naam voor uw eerste resourcegroep - zoals "Mijn--resourcegroep". Een groen vinkje geeft aan dat u de naam van een geldige hebt opgegeven.  ! Nieuwe sql-database 4 (. / media/sql-database-get-started/sql-database-new-database-4.png) 7. Klik onder **bron selecteren* | 2016-09-08 |
| 13 | [Probeer SQL-Database: Gebruik C# naar een SQL-database maken met de SQL-Database-bibliotheek voor .NET](sql-database-get-started-csharp.md) | **Een service hoofdsom voor toegang tot resources maken** Het volgende PowerShell-script Hiermee maakt u de toepassing AD (Active Directory) en de service-hoofdsom die we nodig hebt om te verifiëren van onze C-app. De waarden die we nodig voor de voorgaande C steekproef uitgevoerd. Zie Azure PowerShell gebruiken om te maken van een service principal voor toegang tot bronnen voor gedetailleerde informatie (.. / resource-groep-verifiëren-service-principal.md.).  Meld u aan bij Azure.  Toevoegen AzureRmAccount als er meerdere abonnementen, verwijder de opmerkingen bij en stel op het abonnement dat u wilt werken.  $subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}" Set-AzureRmContext - SubscriptionId $subscriptionId bieden deze waarden voor de nieuwe AAD-app.  $appName is de weergavenaam voor de app, moet uniek zijn in uw adreslijst.  $uri hoeft niet te worden van een reële uri.  $secret is een wachtwoord die u maakt.  $appName = "{app-naam}" $uri = "http://{app-name}" $secret = "{-appwachtwoord}" maken een AAD-app $azureAdApplication = nieuw AzureRmADApp | 2016-09-23 |
| 14 | [Azure SQL-Databases met behulp van de Azure portal beheren](sql-database-manage-portal.md) | Als u wilt bekijken of uw database-instellingen bijwerken, klikt u op de gewenste instelling op het blad SQL-database:! Instellingen voor SQL-database (. / media/sql-database-manage-portal/settings.png) **Hoe vind ik de volledig gekwalificeerde servernaam van een SQL-databases?** Als u wilt weergeven op de naam van uw databases-server, klikt u op het blad **SQL-database** op **Overview** en noteer de servernaam:! Instellingen voor SQL-database (. / media/sql-database-manage-portal/server-name.png) **Hoe beheer ik firewallregels toegang tot mijn SQL server en database te beheren?** Als u wilt weergeven, maken of bijwerken firewallregels, klik op het blad **SQL-database** op **een firewall instellen** . Zie configureren een regel voor het niveau van de server firewall van Azure SQL-Database met behulp van de Azure portal (sql-database-configureren-firewall-settings.md) voor meer informatie. ! regels van Firewall (. / media/sql-database-manage-portal/sql-database-firewall.png) **hoe wijzig ik mijn SQL database-service laag of prestaties niveau?** Als u het niveau van service laag of prestaties van een SQL-database bijwerken, klikt u op ** prijzen laag (s | 2016-09-20 |





## <a name="get-started"></a>Aan de slag

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 15 | [Meerdere tenant-Apps gebruiken met Azure SQL-Database moeten worden geïsoleerd en efficiënt genereert](sql-database-build-multi-tenant-apps.md) | Leer hoe SQL-Database meerdere tenant apps genereert |
| 16 | [Verbinding maken met SQL-Database met SQL Server Management Studio en uitvoeren van een steekproef T-SQL-query](sql-database-connect-query-ssms.md) | Leer hoe u verbinding maken met SQL-Database op Azure met behulp van SQL Server Management Studio (SSMS). Voer een voorbeeldquery met Transact-SQL (T-SQL). |
| 17 | [Verbinding maken met SQL-Database via .NET (C#)](sql-database-develop-dotnet-simple.md) | Gebruik de voorbeeldcode in deze snel starten voor het maken van een toepassing voor de moderne met C# and ondersteund door een krachtige relationele database in de cloud met Azure SQL-Database. |
| 18 | [Een nieuwe resourcegroep die elastische database maken met de portal van Azure](sql-database-elastic-pool-create-portal.md) | Het toevoegen van een resourcegroep die scalable elastische database aan de configuratie van de SQL-database voor snellere beheer en resources delen met veel databases. |
| 19 | [Azure SQL Database-zelfstudies verkennen](sql-database-explore-tutorials.md) | Meer informatie over de SQL-Database-functies en mogelijkheden |
| 20 | [SQL-Database zelfstudie: een SQL-database maken in minuten met behulp van de Azure-portal](sql-database-get-started.md) | Informatie over het instellen van een logische SQL Database-server, server firewallregel, SQL-database en voorbeeldgegevens. Ook informatie over het verbinding maken met clienthulpprogramma's, gebruikers configureren en een regel van de firewall database instellen. |
| 21 | [Azure SQL-Database beveiligt en beschermen](sql-database-helps-secures-and-protects.md) | Lees hoe SQL-Database beveiligen helpt en beveiligen |
| 22 | [Azure SQL-Database leert &amp; past](sql-database-learn-and-adapt.md) | Leer hoe SQL-Database leert en past |
| 23 | [Kies een wolk SQL Server-optie: (PaaS) van Azure SQL-Database of SQL Server Azure VMs (IaaS)](sql-database-paas-vs-sql-server-iaas.md) | Informatie over welke cloud SQL Server-optie past uw toepassing: (PaaS) van Azure SQL-Database of SQL Server in de cloud op Azure virtuele Machines. |
| 24 | [Azure SQL Database-schaal in de browser](sql-database-scale-on-the-fly.md) | Leer hoe SQL-Database in de browser wordt aangepast |
| 25 | [Kennismaken met Azure SQL Database-oplossing snel aan de slag](sql-database-solution-quick-starts.md) | Meer informatie over Azure SQL Database-oplossingen |
| 26 | [Azure SQL-Database werkt in uw omgeving](sql-database-works-in-your-environment.md) | Leer hoe SQL-Database helpt, beveiligt en beschermen |



## <a name="build-an-app"></a>Een app maken

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 27 | [Problemen met, een diagnose stellen bij en SQL-verbindingsfouten en tijdelijke fouten te voorkomen voor SQL-Database](sql-database-connectivity-issues.md) | Leer hoe u problemen met, een diagnose stellen bij en voorkomen dat een SQL-verbinding of een tijdelijke fout in Azure SQL-Database.  |
| 28 | [Verbinding maken met een SQL-Database met Visual Studio](sql-database-connect-query.md) | Schrijf een programma in C# query en verbinding maken met SQL-database. Informatie over het IP-adressen, verbindingstekenreeksen secure login en gratis Visual Studio. |
| 29 | [Behalve 1433 voor ADO.NET 4.5 en SQL-Database V12](sql-database-develop-direct-route-ports-adonet-v12.md) | Clientverbindingen van ADO.NET met Azure SQL Database V12 soms de proxy overslaan en rechtstreeks ermee aan de database. Poorten dan 1433 worden belangrijke. |
| 30 | [SQL-foutcodes voor SQL-Database-clienttoepassingen: Database verbindingsfout en andere problemen](sql-database-develop-error-messages.md) | Meer informatie over foutcodes SQL voor SQL-Database client-toepassingen, zoals veelvoorkomende fouten in database-verbinding, problemen met de database kopiëren en algemene fouten.  |
| 31 | [Verbinding maken met SQL-Database via PHP in Windows](sql-database-develop-php-simple.md) | Geeft een voorbeeld PHP-programma dat vanuit een Windows-client verbinding maakt met Azure SQL-Database en koppelingen naar de benodigde software-onderdelen die nodig zijn voor de client. |
| 32 | [Probeer SQL-Database: Gebruik C# naar een SQL-database maken met de SQL-Database-bibliotheek voor .NET](sql-database-get-started-csharp.md) | SQL-Database voor het ontwikkelen van SQL- en C#-apps probeert en een Azure SQL-Database maken met C# met behulp van de SQL-Database-bibliotheek voor .NET. |
| 33 | [Aan de slag met JSON-functies in Azure SQL-Database](sql-database-json-features.md) | Azure SQL-Database kunt u parseren, query en opmaak van gegevens in de notatie voor JavaScript-Object (JSON) notatie weer. |
| 34 | [Problemen met verbinding oplossen met Azure SQL-Database](sql-database-troubleshoot-common-connection-issues.md) | Stappen om te identificeren en oplossen van veelvoorkomende verbindingsfouten in voor Azure SQL-Database. |
| 35 | [Fout 'Database op de server is momenteel beschikbaar' wanneer u verbinding maakt met sql-database](sql-database-troubleshoot-connection.md) | Problemen met de database op server is niet momenteel beschikbaar fout wanneer een toepassing maakt verbinding met SQL-Database. |



## <a name="develop"></a>Ontwikkelen

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 36 | [De vereiste waarden ophalen voor het verifiëren van een toepassing voor toegang tot SQL-Database vanaf code](sql-database-client-id-keys.md) | Maak de hoofdsom van een service voor het werken met SQL-Database van code. |
| 37 | [Verbinding maken met SQL-Database via Java met JDBC in Windows](sql-database-develop-java-simple.md) | Geeft het voorbeeld van een Java-code dat kunt u verbinding maakt met Azure SQL-Database. In dit voorbeeld worden JDBC en deze op een Windows-clientcomputer wordt uitgevoerd. |
| 38 | [Verbinding maken met SQL-Database via Node.js](sql-database-develop-nodejs-simple.md) | Geeft een Node.js voorbeeld die kunt u verbinding maakt met Azure SQL-Database. |
| 39 | [SQL-Database ontwikkelen-overzicht](sql-database-develop-overview.md) | Meer informatie over de beschikbare connectivity bibliotheken en aanbevolen procedures voor toepassingen verbinden met SQL-Database. |
| 40 | [Verbinding maken met SQL-Database via Python](sql-database-develop-python-simple.md) | Geeft een Python voorbeeld die kunt u verbinding maakt met Azure SQL-Database. |
| 41 | [Verbinding maken met SQL-Database via Ruby](sql-database-develop-ruby-simple.md) | Geef een Ruby code steekproef die u uitvoeren kunt om te verbinden met Azure SQL-Database. |
| 42 | [Aan de slag met tijdelijke tabellen in Azure SQL-Database](sql-database-temporal-tables.md) | Leer hoe u aan de slag met tijdelijke tabellen gebruiken in Azure SQL-Database. |



## <a name="performance-and-scale"></a>Prestaties en schaal

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 43 | [SQL-Database Advisor](sql-database-advisor.md) | De Azure SQL Database-Advisor bevat aanbevelingen voor uw bestaande SQL-Databases die u kunt de huidige queryprestaties verbeteren. |
| 44 | [SQL-Database Advisor met behulp van de Azure portal](sql-database-advisor-portal.md) | U kunt de Advisor Azure SQL-Database in de portal van Azure gebruiken om te controleren en implementeren van aanbevelingen voor uw bestaande SQL-Databases die u kunt de huidige queryprestaties verbeteren. |
| 45 | [Overzicht van de vergelijkende Azure SQL-Database](sql-database-benchmark-overview.md) | In dit onderwerp wordt het Azure SQL Database-criterium gebruikt in de prestaties van Azure SQL-Database meten beschreven. |
| 46 | [Wanneer moet een elastische database-toepassingen worden gebruikt?](sql-database-elastic-pool-guidance.md) | Een groep elastische database is een verzameling van beschikbare resources die zijn gedeeld door een groep elastische databases. In dit document bevat richtlijnen beoordelen van de geschiktheid van het gebruik van een elastische database-toepassingen voor een groep van databases. |
| 47 | [Aan de slag met elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md) | Eenvoudige uitleg van de functie in hulpmiddelen voor elastische webdatabase van Azure SQL-Database, inclusief eenvoudig voorbeeld app uitvoeren. |
| 48 | [SQL-Database Veelgestelde vragen](sql-database-faq.md) | Antwoorden op veelvoorkomende vragen klanten Stel een vraag over cloud-databases en Azure SQL-Database, Microsofts relationele database management systeem (RDBMS) en database als een service die u in de cloud. |
| 49 | [Aan de slag met In het geheugen (Preview) in SQL-Database](sql-database-in-memory.md) | SQL In het geheugen technologieën verbeteren aanzienlijk de prestaties van transacties en analyses werkbelasting. Leer hoe u deze mogelijkheden. |
| 50 | [Gebruik In het geheugen OLTP (preview) om te verbeteren de toepassingsprestaties van uw in SQL-Database](sql-database-in-memory-oltp-migration.md) | Hanteer In het geheugen OLTP om te verbeteren de prestaties voor transacties in een bestaande SQL-database. |
| 51 | [Monitor met een In het geheugen OLTP opslag](sql-database-in-memory-oltp-monitoring.md) | De geschatte en monitor XTP geheugenopslag gebruikt, capaciteit; capaciteit fout 41823 oplossen |
| 52 | [De Query winkel in Azure SQL-Database](sql-database-operate-query-store.md) | Informatie over het werken de Store Query in Azure SQL-Database |
| 53 | [SQL-Database prestaties inzicht](sql-database-performance.md) | De Azure SQL-Database biedt prestaties hulpmiddelen om te identificeren gebieden die u kunnen de huidige queryprestaties verbeteren. |
| 54 | [Azure SQL-Database en prestaties voor één databases](sql-database-performance-guidance.md) | In dit artikel kunt u bepalen welke servicelaag kiezen voor uw toepassing. Dit wordt ook aanbevolen manieren om uw toepassing optimaal te profiteren van Azure SQL-Database af te stellen. |
| 55 | [Azure SQL Database Query prestaties inzicht](sql-database-query-performance.md) | Query prestatiecontroles, geeft u de meeste CPU-gebruik query's voor een Azure SQL-Database. |
| 56 | [De service laag en prestaties niveau wijzigen (prijzen laag) van een SQL-database](sql-database-scale-up.md) | Wijzig de servicelaag en prestatieniveau van een Azure SQL-database ziet u hoe u de schaal van de SQL-database omhoog of omlaag. Het wijzigen van de prijzen laag van een Azure SQL-database. |
| 57 | [Databaseprestaties in Azure SQL-Database controleren](sql-database-single-database-monitor.md) | Meer informatie over de opties voor het controleren van de database met Azure hulpprogramma's en dynamische management weergaven. |
| 58 | [SQL-Database prestaties afstemmen tips](sql-database-troubleshoot-performance.md) | Tips voor het prestaties optimaliseren in Azure SQL-Database via evaluatie en verbetering. |
| 59 | [Het gebruik van batchen SQL-Database toepassingsprestaties te verbeteren](sql-database-use-batching-to-improve-performance.md) | Het onderwerp bevat bewijs die batchen databasebewerkingen enorm imroves de snelheid en schaalbaarheid van uw Azure SQL Database-toepassingen. Hoewel deze batchen technieken voor een SQL Server-database werken, wordt de nadruk in dit artikel ligt op Azure. |



## <a name="tools-for-scaling-out"></a>Hulpprogramma's voor schalen

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 60 | [Patronen voor multitenant SaaS-toepassingen en Azure SQL-Database ontwerpen](sql-database-design-patterns-multi-tenancy-saas-applications.md) | In dit artikel wordt beschreven hoe de vereisten en algemene gegevens architectuur patronen van multitenant database toepassingen die worden uitgevoerd in een cloud-omgeving op moet letten en de verschillende compromissen nodig zijn die is gekoppeld aan deze patronen. Ook wordt uitgelegd hoe Azure SQL-Database, met de elastische van toepassingen en hulpmiddelen voor elastische te herstellen op een wijze zonder deze vereisten is voldaan. |
| 61 | [Bestaande databases naar schalen migreren](sql-database-elastic-convert-to-use-elastic-tools.md) | Een laptopgeheugen databases als u wilt gebruiken elastische Databasehulpmiddelen door te maken van een shard kaart manager converteren |
| 62 | [Samenstellen van scalable cloud-databases](sql-database-elastic-database-client-library.md) | Scalable .NET-database apps gebruiken met de bibliotheek van de client elastische database maken |
| 63 | [Prestatie-items voor shard kaart manager](sql-database-elastic-database-perf-counters.md) | ShardMapManager klasse- en afhankelijke routeren prestatiemeteritems |
| 64 | [Een shard met hulpmiddelen voor databases elastische toevoegen](sql-database-elastic-scale-add-a-shard.md) | Het gebruik van elastische schaal-API's nieuwe shards toevoegen aan een shard instellen. |
| 65 | [Een gesplitste samenvoegen service implementeren](sql-database-elastic-scale-configure-deploy-split-and-merge.md) | Splitsen en samenvoegen met hulpmiddelen voor databases elastische |
| 66 | [Afhankelijke omleiding van gegevens](sql-database-elastic-scale-data-dependent-routing.md) | Het gebruik van de klas ShardMapManager in .NET-apps voor gegevens-afhankelijke circulatielijst voor een functie van elastische databases voor Azure SQL-Database |
| 67 | [Elastische Databasehulpmiddelen Veelgestelde vragen](sql-database-elastic-scale-faq.md) | Veelgestelde vragen over elastische schaal van Azure SQL-Database. |
| 68 | [Woordenlijst van de hulpmiddelen voor elastische Database](sql-database-elastic-scale-glossary.md) | Uitleg van de voorwaarden die wordt gebruikt voor hulpmiddelen voor databases elastische |
| 69 | [Schalen met Azure SQL-Database](sql-database-elastic-scale-introduction.md) | Software als een Service (SaaS)-ontwikkelaars eenvoudig elastische, scalable databases kan maken in de cloud met deze hulpmiddelen |
| 70 | [Referenties gebruikt voor toegang tot de bibliotheek van de client elastische Database](sql-database-elastic-scale-manage-credentials.md) | Het instellen van het juiste niveau van de referenties van beheerder om alleen-lezen, voor elastische database-apps |
| 71 | [Meerdere shard query's uitvoeren](sql-database-elastic-scale-multishard-querying.md) | Query's uitvoeren in shards met de bibliotheek van de client elastische database. |
| 72 | [Gegevens verplaatsen tussen schaal-out cloud-databases](sql-database-elastic-scale-overview-split-and-merge.md) | Wordt uitgelegd hoe manipuleren shards en verplaats gegevens via een selfservice gehoste service elastische database API's gebruikt. |
| 73 | [De schaal van databases met de manager van de map shard aanpassen](sql-database-elastic-scale-shard-map-management.md) | Het gebruik van de ShardMapManager, elastische database client-bibliotheek |
| 74 | [Beveiliging van de gesplitste samenvoegen](sql-database-elastic-scale-split-merge-security-configuration.md) | Instellen van x409 certificaten voor versleuteling |
| 75 | [Een app voor het gebruik van de meest recente elastische database clientbibliotheek upgraden](sql-database-elastic-scale-upgrade-client-library.md) | Upgrade-apps en -bibliotheken met Nuget |
| 76 | [Elastische Database client-bibliotheek met entiteit Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) | Elastische Database clientbibliotheek en entiteit Framework gebruiken voor het coderen van databases |
| 77 | [Gebruik van elastische database client-bibliotheek met Dapper](sql-database-elastic-scale-working-with-dapper.md) | Gebruik elastische database client-bibliotheek met Dapper. |
| 78 | [Meerdere tenant-toepassingen met hulpmiddelen voor databases elastische en beveiliging op gebruikersniveau rij](sql-database-elastic-tools-multi-tenant-row-level-security.md) | Leer hoe u Elastische Databasehulpmiddelen samen met de beveiliging op gebruikersniveau rij gebruiken om te maken van een toepassing met een gegevenslaag ten zeerste scalable op Azure SQL-Database die ondersteuning biedt voor meerdere tenant shards. |



## <a name="elastic-jobs"></a>Elastische taken

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 79 | [Maken en beheren scaled out Azure SQL-Databases met behulp van elastische taken (preview)](sql-database-elastic-jobs-create-and-manage.md) | Doorloop maken en beheren van een taak elastische database. |
| 80 | [Aan de slag met elastische Database taken](sql-database-elastic-jobs-getting-started.md) | het gebruik van elastische database taken |
| 81 | [Uitgebreide cloud databases beheren](sql-database-elastic-jobs-overview.md) | Ziet u de service van de taak elastische database |
| 82 | [Maken en beheren van een SQL-Database elastische database taken via PowerShell (preview)](sql-database-elastic-jobs-powershell.md) | PowerShell gebruikt voor het beheren van Azure SQL Database-toepassingen |
| 83 | [Overzicht van de taken installatie elastische Database](sql-database-elastic-jobs-service-installation.md) | Installatie van de functie elastische taak doorlopen. |
| 84 | [Onderdelen van de taken elastische Database verwijderen](sql-database-elastic-jobs-uninstall.md) | Het databasehulpmiddel voor elastische taken verwijderen |



## <a name="elastic-pools"></a>Elastische van toepassingen

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 85 | [Wat is een Azure elastische toepassingen?](sql-database-elastic-pool.md) | Het beheren van honderden of duizenden databases met een groep. Één prijs voor een reeks performance-eenheden kan worden verdeeld over de groep. Verplaats op databases in- of uitzoomen op wordt. |
| 86 | [Een toepassingen elastische database maken met C#](sql-database-elastic-pool-create-csharp.md) | Gebruik C#-database-technieken voor het maken van een resourcegroep die scalable elastische database in Azure SQL-Database, zodat u resources met andere veel databases delen kunt. |
| 87 | [Een nieuwe elastische database-toepassingen maken met PowerShell](sql-database-elastic-pool-create-powershell.md) | Leer hoe u PowerShell gebruiken om te schalen Azure SQL-Database resources door te maken van een resourcegroep die scalable elastische database voor het beheren van meerdere databases. |
| 88 | [PowerShell-script om aan te duiden databases die geschikt zijn voor een elastische database-toepassingen](sql-database-elastic-pool-database-assessment-powershell.md) | Een groep elastische database is een verzameling van beschikbare resources die zijn gedeeld door een groep elastische databases. Dit document bevat een Powershell-script om te beoordelen van de geschiktheid van het gebruik van een elastische database-toepassingen voor een groep van databases. |
| 89 | [Bewaken en beheren van een toepassingen elastische database met C#](sql-database-elastic-pool-manage-csharp.md) | Gebruik C#-database-technieken voor het beheren van een Azure SQL-Database elastische database-toepassingen. |
| 90 | [Controleren en een elastische database-toepassingen met de Azure-portal beheren](sql-database-elastic-pool-manage-portal.md) | Informatie over het gebruik van de Azure-portal en ingebouwde intelligence SQL-Database om te beheren, bewaken en breedte een resourcegroep die scalable elastische database databaseprestaties optimaliseren en beheren van kosten. |
| 91 | [Bewaken en beheren van een toepassingen elastische database met PowerShell](sql-database-elastic-pool-manage-powershell.md) | Leer hoe u PowerShell gebruiken voor het beheren van een elastische database-toepassingen. |
| 92 | [Bewaken en beheren van een toepassingen elastische database met Transact-SQL](sql-database-elastic-pool-manage-tsql.md) | T-SQL Azure SQL-database maken in een elastische van toepassingen gebruiken. Of de datbase verplaatsen en afmelden bij de groepen met T-SQL. |
| 93 | [Elastische database groep facturering en prijsinformatie](sql-database-elastic-pool-price.md) | Specifiek voor elastische database pools prijsinformatie. |



## <a name="elastic-query"></a>Elastische query

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 94 | [Rapport over schaal-out cloud-databases (preview)](sql-database-elastic-query-getting-started.md) | het gebruik van cross database databasequery 's |
| 95 | [Aan de slag met meerdere databases query's (verticale partities) (preview)](sql-database-elastic-query-getting-started-vertical.md) | het gebruik van elastische databasequery's met verticaal partitioneren databases |
| 96 | [Rapportage van binnen schaal-out cloud-databases (preview)](sql-database-elastic-query-horizontal-partitioning.md) | het instellen van elastische query's via horizontale partities |
| 97 | [Azure SQL-Database elastische database-queryoverzicht (preview)](sql-database-elastic-query-overview.md) | Overzicht van de functie elastische query |
| 98 | [Query met betrekking tot cloud-databases met verschillende schema's (preview)](sql-database-elastic-query-vertical-partitioning.md) | cross-databasequery's instellen op verticale partities |



## <a name="elastic-transaction"></a>Elastische transactie

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 99 | [Gedistribueerde transacties in de cloud-databases](sql-database-elastic-transactions-overview.md) | Overzicht van elastische databasetransacties met Azure SQL-Database |



## <a name="backup-and-recovery"></a>Back-up en herstellen

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 100 | [Back-ups van SQL-Database](sql-database-automated-backups.md) | Lees meer over SQL-Database ingebouwde back-ups die u voor gebruik in staat stellen een Azure SQL-Database terug naar een vorige punt in tijd of een database kopiëren naar een nieuwe database in een geografische gebied (maximaal 35 dagen). |
| 101 | [Overzicht van bedrijfscontinuïteit met Azure SQL-Database](sql-database-business-continuity.md) | Leer hoe Azure SQL-Database ondersteunt cloud bedrijfscontinuïteit en herstellen van databases en kunt u belangrijke cloud-toepassingen die worden uitgevoerd. |
| 102 | [Het herstellen van één tabel van een back-up van Azure SQL-Database](sql-database-cloud-migrate-restore-single-table-azure-backup.md) | Informatie over het herstellen van één tabel van back-up van Azure SQL-Database. |
| 103 | [Een toepassing voor de cloud-herstel met actieve geografische-herhaling in SQL-Database ontwerpen](sql-database-designing-cloud-solutions-for-disaster-recovery.md) | Informatie over het ontwerpen van cloud noodgevallen hersteloplossingen voor bedrijven bedrijfscontinuïteit planning geografische-replicatie met voor app gegevens back-up met Azure SQL-Database. |
| 104 | [Een Azure SQL-Database of failover in een secundair herstellen](sql-database-disaster-recovery.md) | Informatie over het herstellen van een database van een storing regionale datacenter of mislukt met de Azure SQL Database actieve geografische-replicatie en mogelijkheden voor geografische herstellen. |
| 105 | [Uitvoering van noodgevallen herstel meer details](sql-database-disaster-recovery-drills.md) | Meer informatie vinden en aanbevolen procedures voor het gebruik van Azure SQL-Database om uit te voeren noodgevallen herstel oefeningen die helpt behouden de opdracht kritieke bedrijfstoepassingen robuuste fouten en bijvoorbeeld. |
| 106 | [Strategieën voor noodgevallen voor toepassingen met SQL-Database elastische toepassingen](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) | Informatie over het ontwerpen van uw oplossing cloud voor herstel door het juiste failover patroon te kiezen. |
| 107 | [Gebruik van de klasse RecoveryManager shard kaart problemen oplossen](sql-database-elastic-database-recovery-manager.md) | Gebruik de klasse RecoveryManager voor het oplossen van problemen met shard-kaarten |
| 108 | [Een Bacpac-bestand als u wilt maken van een Azure SQL-database importeren](sql-database-import.md) | Een Azure SQL-database maken door een bestaande Bacpac-bestand te importeren. |
| 109 | [Een Azure SQL-Database herstellen naar een vorig punt in tijd in de Portal van Azure](sql-database-point-in-time-restore-portal.md) | Herstel een Azure SQL-Database naar een vorig punt in tijd. |
| 110 | [Een Azure SQL-Database naar een vorig punt in tijd met PowerShell herstellen](sql-database-point-in-time-restore-powershell.md) | Een Azure SQL-Database naar een vorig punt in tijd herstellen |
| 112 | [Een automatische back-ups met Azure SQL-database herstellen](sql-database-recovery-using-backups.md) | Meer informatie over Point-in-Time herstellen, waarmee u terugkeren een Azure SQL-Database naar een vorige punt in tijd (maximaal 35 dagen). |
| 113 | [Een verwijderde Azure SQL-database met behulp van de Portal Azure terugzetten](sql-database-restore-deleted-database-portal.md) | Herstel een verwijderde Azure SQL-database (Azure Portal). |
| 114 | [Een verwijderde Azure SQL-Database herstellen via PowerShell](sql-database-restore-deleted-database-powershell.md) | Herstel een verwijderde SQL Azure-Database (PowerShell). |
| 115 | [Een database terugzetten naar een vorig punt in tijd, een verwijderde database terugzetten of herstellen van een storing gegevens-beheercentrum](sql-database-troubleshoot-backup-and-restore.md) | Informatie over het herstellen van een database cloud van fouten en bijvoorbeeld back-ups en replica's gebruiken in Azure SQL-Database. |



## <a name="migrate"></a>Migreren

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 116 | [Een SQL Server-database exporteren naar een Bacpac-bestand met SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md) | Microsoft Azure SQL-Database, databasemigratie database exporteren, Bacpac-bestand, sqlpackage exporteren |
| 117 | [Een SQL Server-database exporteren naar een Bacpac-bestand met SQL Server Management Studio](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) | Microsoft Azure SQL-Database, databasemigratie database exporteren, Bacpac-bestand, exporteren gegevens laag toepassing wizard Exporteren |
| 118 | [Met SQL-Database importeren uit een Bacpac-bestand met SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md) | Microsoft Azure SQL-Database, databasemigratie-database importeren, Bacpac-bestand, sqlpackage importeren |
| 119 | [Importeren uit BACPAC met behulp van SSMS SQL-Database](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) | Microsoft Azure SQL-Database, database implementeert, databasemigratie, database importeren, exporteren database, wizard migreren |
| 120 | [SQL Server-database migreren met SQL-Database met Database implementeren naar de Wizard van Microsoft Azure-Database](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) | Microsoft Azure SQL-Database, databasemigratie Wizard van Microsoft Azure-Database |
| 121 | [SQL Server-database migreren met Azure SQL-Database replicatietransacties](sql-database-cloud-migrate-compatible-using-transactional-replication.md) | Microsoft Azure SQL-Database, databasemigratie, database, importeren transacties herhaling |
| 122 | [SQL-Database compatibiliteit met SqlPackage.exe bepalen](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md) | Microsoft Azure SQL Database, databasemigratie, compatibiliteit van de SQL-Database, SqlPackage |
| 123 | [Gebruik van SQL Server Management Studio om te bepalen van de SQL-Database compatibiliteit voor de migratie met Azure SQL-Database](sql-database-cloud-migrate-determine-compatibility-ssms.md) | Microsoft Azure SQL Database, databasemigratie, compatibiliteit met SQL-Database gegevens laag toepassing Wizard exporteren |
| 124 | [Wizard SQL Azure-migratie om oplossen SQL Server-database compatibiliteitsproblemen vóór de migratie met Azure SQL-Database te gebruiken](sql-database-cloud-migrate-fix-compatibility-issues.md) | Microsoft Azure SQL Database, databasemigratie, compatibiliteit, de Wizard SQL Azure-migratie |
| 125 | [Een SQL Server-Database migreren met Azure SQL-Database met behulp van SQL Server Data Tools voor Visual Studio](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md) | Microsoft Azure SQL Database, databasemigratie, compatibiliteit, de Wizard in de SQL Azure-migratie, SSDT |
| 126 | [SQL Server-database compatibiliteitsproblemen met behulp van SQL Server Management Studio voor de migratie met SQL-Database oplossen](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md) | Microsoft Azure SQL Database, databasemigratie, compatibiliteit, de Wizard SQL Azure-migratie |
| 127 | [Een Azure SQL-database naar een bestand Bacpac-archiveren via PowerShell](sql-database-export-powershell.md) | Een Azure SQL-database naar een bestand Bacpac-archiveren via PowerShell |
| 128 | [Een bestand Bacpac-een Azure SQL-database maken met behulp van PowerShell importeren](sql-database-import-powershell.md) | Een bestand Bacpac-een Azure SQL-database maken met behulp van PowerShell importeren |
| 129 | [Gegevens uit CSV laadt in Azure SQL Data Warehouse (platte bestanden)](sql-database-load-from-csv-with-bcp.md) | Voor een kleine gegevensgrootte, wordt de bcp gebruikt om gegevens in Azure SQL-Database importeren. |
| 130 | [Databases verplaatsen tussen servers, tussen abonnementen en en afmelden bij Azure](sql-database-troubleshoot-moving-data.md) | Snelle stappen om te kopiëren, verplaatsen en migreren van gegevens en databases in Azure SQL-Database. |



## <a name="move-data-in-and-out"></a>Gegevens in-en uitfaden verplaatsen

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 131 | [SQL Server-database migreren met SQL-Database in de cloud](sql-database-cloud-migrate.md) | Meer informatie over hoe on-premises implementatie SQL Server-database migreren met Azure SQL-Database in de cloud. Database migratiehulpprogramma's gebruiken om te testen compatibiliteit vóór de databasemigratie. |
| 132 | [Kopieer een Azure SQL-Database](sql-database-copy.md) | Een kopie van een Azure SQL-database maken |
| 133 | [Een Azure SQL-database naar een Bacpac-bestand met behulp van de Portal Azure archiveren](sql-database-export.md) | Een Azure SQL-database naar een Bacpac-bestand met behulp van de Portal Azure archiveren |



## <a name="security"></a>Beveiliging

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 134 | [Verbinding maken met SQL-Database of SQL datawarehouse met Azure Active Directory-verificatie](sql-database-aad-authentication.md) | Leer hoe u verbinding maken met SQL-Database via Azure Active Directory-verificatie. |
| 135 | [Altijd versleuteld: Gevoelige gegevens in een SQL-Database beveiligen en bewaren van uw versleutelingssleutels in het archief van de Windows-certificaat](sql-database-always-encrypted.md) | Vertrouwelijke gegevens in uw SQL-database beveiligen in minuten. |
| 136 | [Altijd versleuteld: Gevoelige gegevens in een SQL-Database beveiligen en bewaren van uw versleutelingssleutels in Azure-toets kluis](sql-database-always-encrypted-azure-key-vault.md) | Vertrouwelijke gegevens in uw SQL-database beveiligen in minuten. |
| 137 | [SQL-Database - ondersteuning voor Downlevel-clients en IP-eindpunt wijzigingen voor controle](sql-database-auditing-and-dynamic-data-masking-downlevel-clients.md) | Meer informatie over ondersteuning voor SQL-Database voor downlevel clients en IP-eindpunt wijzigingen voor controle. |
| 138 | [Azure SQL-Database serverniveau firewallregels configureren via PowerShell](sql-database-configure-firewall-settings-powershell.md) | Informatie over het configureren van de firewall voor IP-adressen die toegang de SQL Azure-databases tot. |
| 139 | [Azure SQL-Database serverniveau firewallregels configureren met de REST API](sql-database-configure-firewall-settings-rest.md) | Informatie over het configureren van de firewall voor IP-adressen die toegang de SQL Azure-databases tot. |
| 140 | [Azure SQL Database-server en database-niveau firewallregels met T-SQL configureren](sql-database-configure-firewall-settings-tsql.md) | Informatie over het configureren van de firewall voor IP-adressen die toegang de SQL Azure-databases tot. |
| 141 | [Aan de slag met SQL-Database dynamische gegevens Masking (Azure Portal)](sql-database-dynamic-data-masking-get-started.md) | Hoe u aan de slag met SQL-Database dynamische gegevens Masking in de Portal van Azure |
| 142 | [Aan de slag met SQL-Database dynamische gegevens Masking (Azure klassieke Portal)](sql-database-dynamic-data-masking-get-started-portal.md) | Hoe u aan de slag met SQL-Database dynamische gegevens Masking in de klassieke Azure-Portal |
| 143 | [Azure SQL Database-firewallregels configureren \- overzicht](sql-database-firewall-configure.md) | Informatie over het configureren van een firewall SQL-database met de server en database-niveau firewallregels voor het beheren van access. |
| 144 | [SQL-Database zelfstudie: gebruikersaccounts kunnen openen en beheren van een database van SQL-database maken](sql-database-get-started-security.md) | Leer hoe u gebruikersaccounts wilt openen en voor het beheren van een database maken. |
| 145 | [SQL-databaseverificatie en machtiging: toegang verlenen](sql-database-manage-logins.md) | Meer informatie over het beheer van de beveiliging SQL-Database, specifiek het beheren van access en login databasebeveiliging via de hoofdsom niveau van de server-account. |
| 146 | [Richtlijnen voor beveiliging van Azure SQL-Database en beperkingen](sql-database-security-guidelines.md) | Meer informatie over Microsoft Azure SQL Database-richtlijnen en beperkingen ten aanzien van beveiliging. |
| 147 | [Aan de slag met SQL-Database bedreiging detectie](sql-database-threat-detection-get-started.md) | Hoe u aan de slag met SQL-Database bedreiging detectie in de Portal van Azure |
| 148 | [Het uitvoeren van veelvoorkomende beheertaken zoals opnieuw instellen van beheerderswachtwoord in Azure SQL-Database](sql-database-troubleshoot-permissions.md) | Beschrijving van het uitvoeren van veelvoorkomende beheertaken in SQL-Database. Bijvoorbeeld beheerderswachtwoord opnieuw in te stellen, toewijzen en verwijderen van access. |



## <a name="manage-your-database"></a>Beheer de database

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 149 | [Aan de slag met SQL-database controleren](sql-database-auditing-get-started.md) | Aan de slag met SQL-database controleren |
| 150 | [Een regel voor het niveau van de server firewall van Azure SQL-Database met behulp van de Portal Azure configureren](sql-database-configure-firewall-settings.md) | Informatie over het configureren van de firewall voor IP-adressen die toegang Azure SQL server tot. |
| 151 | [Kopieer een Azure SQL-Database met behulp van de Azure portal](sql-database-copy-portal.md) | Een kopie van een Azure SQL-database maken |
| 152 | [Azure SQL-Databases met Azure automatisering beheren](sql-database-manage-automation.md) | Meer informatie over hoe de automatisering Azure-service kan worden gebruikt voor het beheren van Azure SQL-databases op schaal. |
| 153 | [Overzicht: beheerprogramma's voor SQL-Database](sql-database-manage-overview.md) | Hulpprogramma's en opties voor het beheren van Azure SQL-Database wordt vergeleken |
| 154 | [Cmdlets voor controle Azure SQL-Database dynamische management weergaven gebruiken](sql-database-monitoring-with-dmvs.md) | Leer hoe u prestatieproblemen opsporen en onderzoeken algemene met behulp van dynamische management weergaven om de Microsoft Azure SQL-Database te houden. |
| 155 | [Beveiliging van uw SQL-Database](sql-database-security.md) | Meer informatie over de beveiliging van Azure SQL-Database en SQL Server, met inbegrip van de verschillen tussen de cloud en SQL Server on-premises als het gaat om verificatie, autorisatie, beveiliging van verbindingen, codering en naleving. |



## <a name="extended-events"></a>Uitgebreide gebeurtenissen

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 156 | [Bestand doel code voor uitgebreide gebeurtenissen in SQL-Database](sql-database-xevent-code-event-file.md) | Voor een steekproef in twee fasen code waarop u het doel van de gebeurtenisbestand in een uitgebreide gebeurtenis op Azure SQL-Database, vindt u PowerShell en Transact-SQL. Azure opslag is een vereist onderdeel van dit scenario. |
| 157 | [Bellen Buffer doel code voor uitgebreide gebeurtenissen in SQL-Database](sql-database-xevent-code-ring-buffer.md) | Biedt een steekproef van Transact-SQL-code die gemakkelijk en snel door gebruik van het doel bellen Buffer in Azure SQL-Database is gemaakt. |
| 158 | [Uitgebreide gebeurtenissen in SQL-Database](sql-database-xevent-db-diff-from-svr.md) | Beschreven uitgebreide gebeurtenissen (XEvents) in Azure SQL-Database, en hoe gebeurtenis sessies enigszins afwijken van de gebeurtenis-sessies in Microsoft SQL Server. |



## <a name="geo-replication"></a>Geografische herhaling

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 159 | [Een failover of niet gepland initiëren voor Azure SQL-Database met de portal van Azure](sql-database-geo-replication-failover-portal.md) | Een failover of niet gepland voor Azure SQL-Database met behulp van de Azure portal initiëren |
| 160 | [Een failover of niet gepland initiëren voor Azure SQL-Database met PowerShell](sql-database-geo-replication-failover-powershell.md) | Een failover of niet gepland initiëren voor Azure SQL-Database via PowerShell |
| 161 | [Een failover of niet gepland initiëren voor Azure SQL-Database met Transact-SQL](sql-database-geo-replication-failover-transact-sql.md) | Een failover of niet gepland initiëren voor Azure SQL-Database met Transact-SQL |
| 162 | [Overzicht: SQL actieve geografische-databasereplicatie](sql-database-geo-replication-overview.md) | Actieve geografische-herhaling kunt u setup-4 kopieën van uw database in een van de Azure datacenters. |
| 163 | [Geografische-replicatie configureren voor Azure SQL-Database in de portal van Azure](sql-database-geo-replication-portal.md) | Geografische-replicatie configureren voor Azure SQL-Database met behulp van de Azure portal |
| 164 | [Geografische-replicatie configureren voor Azure SQL-Database met PowerShell](sql-database-geo-replication-powershell.md) | Actieve geografische-replicatie configureren voor Azure SQL-Database via PowerShell |
| 165 | [Beveiliging van Azure SQL-Database beheren na herstel](sql-database-geo-replication-security-config.md) | In dit onderwerp wordt uitgelegd beveiligingsoverwegingen voor beveiliging beheren na een database terugzetten of een failover. |
| 166 | [Geografische-replicatie configureren voor Azure SQL-Database met Transact-SQL](sql-database-geo-replication-transact-sql.md) | Geografische-replicatie configureren voor Azure SQL-Database met Transact-SQL |
| 167 | [Een Azure SQL-Database uit een geografische-redundante back-up met behulp van de Portal Azure geografische-herstellen](sql-database-geo-restore-portal.md) | Geografische herstellen een Azure SQL-Database uit een geografische-redundante back-up (Azure Portal). |
| 168 | [Een Azure SQL-Database herstellen vanuit een geografische-redundante back-up via PowerShell](sql-database-geo-restore-powershell.md) | Een Azure SQL-Database naar een nieuwe server uit een geografische-redundante back-up herstellen |



## <a name="upgrade"></a>Upgrade

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 169 | [Verbeterde prestaties van query's met compatibiliteit niveau 130 in Azure SQL-Database](sql-database-compatibility-level-query-performance-130.md) | Stappen en hulpprogramma's voor om te bepalen welke compatibiliteitsniveau meest geschikt is voor uw database op Azure SQL-Database of Microsoft SQL Server |
| 170 | [Lopende upgrades van SQL actieve geografische-databasereplicatie met cloud-toepassingen beheren](sql-database-manage-application-rolling-upgrade.md) | Informatie over het gebruik van Azure SQL-Database geografische-herhaling ter ondersteuning van online upgrades van uw cloud-toepassing. |
| 171 | [Upgrade naar Azure SQL Database V12 met behulp van de Azure portal](sql-database-upgrade-server-portal.md) | Wordt uitgelegd hoe u een upgrade uitvoeren naar Azure SQL Database V12 met inbegrip van het Web en bedrijfsdatabases upgraden en hoe u een V11: server zijn databases migreren rechtstreeks in een elastische database-toepassingen met behulp van de Azure portal bijwerken. |
| 172 | [Upgrade naar Azure SQL Database V12 via PowerShell](sql-database-upgrade-server-powershell.md) | Wordt uitgelegd hoe u een upgrade uitvoeren naar Azure SQL Database V12 waaronder het Web en bedrijfsdatabases upgraden en het upgraden van een V11: server zijn databases migreren rechtstreeks in een elastische database-toepassingen via PowerShell. |
| 173 | [Plannen en voorbereiden voor het bijwerken naar SQL-Database V12](sql-database-v12-plan-prepare-upgrade.md) | Worden de voorbereidingen en beperkingen voor het upgraden naar versie V12 van Azure SQL-Database. |
| 174 | [Wat is er nieuw in SQL-Database V12](sql-database-v12-whats-new.md) | Beschrijft waarom business-systemen die Azure SQL-Database in de cloud gebruikt door een upgrade naar versie V12 nu profiteren. |
| 175 | [Web en Business Edition zonsondergang Veelgestelde vragen](sql-database-web-business-sunset-faq.md) | Meer informatie over wanneer de Azure SQL-Web- en -databases wordt ingetrokken en leer meer over de functies en functionaliteit van de nieuwe service niveaus. |



## <a name="tools"></a>Hulpmiddelen voor

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 176 | [Azure SQL-Database met PowerShell beheren](sql-database-command-line-tools.md) | Beheer van de Azure SQL-Database met PowerShell. |
| 177 | [SQL-Database zelfstudie: Excel verbinden met een Azure SQL-database en een rapport maken](sql-database-connect-excel.md) | Leer hoe u Microsoft Excel verbinden met Azure SQL-database in de cloud. Gegevens importeren in Excel voor rapportage en gegevens verkennen. |
| 178 | [Een SQL-database maken en uitvoeren van algemene database configuratietaken met PowerShell-cmdlets](sql-database-get-started-powershell.md) | Lees meer over het maken van een SQL-database met PowerShell. Algemene database-configuratietaken kunnen worden beheerd via PowerShell-cmdlets. |
| 179 | [Aan de slag met de synchronisatie van Azure SQL-gegevens (Preview)](sql-database-get-started-sql-data-sync.md) | Deze zelfstudie kunt u aan de slag met het synchroniseren in de Azure SQL-gegevens (Preview). |
| 180 | [Azure SQL-Database met SQL Server Management Studio beheren](sql-database-manage-azure-ssms.md) | Informatie over het gebruik van SQL Server Management Studio SQL-Database servers en databases beheren. |
| 181 | [Azure SQL-Databases met behulp van de Azure portal beheren](sql-database-manage-portal.md) | Informatie over het gebruik van de Azure-Portal voor het beheren van een relationele database in de cloud met behulp van de Azure-Portal. |
| 182 | [De service laag en prestaties niveau wijzigen (prijzen laag) van een SQL-database met PowerShell](sql-database-scale-up-powershell.md) | Wijzig de servicelaag en prestatieniveau van een Azure SQL-database ziet u hoe u de schaal van de SQL-database omhoog of omlaag met PowerShell. De prijzen laag van een Azure SQL-database met PowerShell wijzigen. |
| 183 | [SQL Server Management Studio in Azure RemoteApp met verbinding maken met SQL-Database](sql-database-ssms-remoteapp.md) | Gebruik van deze zelfstudie om te leren hoe u SQL Server Management Studio in Azure RemoteApp voor beveiliging en prestaties wordt gebruikt wanneer u verbinding maakt met SQL-Database |



## <a name="reference"></a>Verwijzing

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 184 | [Bibliotheken van de verbinding voor SQL-Database en SQL Server](sql-database-libraries.md) | Bevat de minimale versienummer voor elk stuurprogramma dat clientprogramma's kunt verbinding maken met Azure SQL-Database of Microsoft SQL Server. Een koppeling is opgegeven voor de versie-informatie over het stuurprogramma's die zijn vrijgegeven per de community in plaats van Microsoft. |
| 185 | [Limieten van de resource Azure SQL-Database](sql-database-resource-limits.md) | Deze pagina bevat enkele algemene resource-limieten voor Azure SQL-Database. |
| 186 | [Azure SQL Database Transact-SQL-verschillen](sql-database-transact-sql-information.md) | Transact-SQL-instructies die minder dan volledig worden ondersteund in Azure SQL-Database |



## <a name="miscellaneous"></a>Diverse

| &nbsp; | Titel | Beschrijving |
| --: | :-- | :-- |
| 187 | [Kopieer een Azure SQL-database via PowerShell](sql-database-copy-powershell.md) | Kopie van een Azure SQL-database via PowerShell maken |
| 188 | [Kopieer een met Transact-SQL Azure SQL-database](sql-database-copy-transact-sql.md) | Kopie van een met Transact-SQL Azure SQL-database maken |
| 189 | [Azure SQL Database-algemeen beperkingen en richtlijnen](sql-database-general-limitations.md) | Enkele algemene beperkingen voor Azure SQL-Database, evenals de gebieden van interoperabiliteit en ondersteuning voor deze pagina wordt beschreven. |
| 190 | [SQL-Database prijzen laag aanbevelingen](sql-database-service-tier-advisor.md) | Bij het wijzigen van de prijzen zijn niveaus in de Azure-portal prijzen van laag aanbevelingen mits aanbevolen de laag die het meest geschikt is geschikt zijn voor het uitvoeren van een bestaande Azure SQL Database de werklast. Prijzen lagen beschrijven de service laag en prestaties het niveau van een SQL-database. |


&nbsp;

<hr/>

<a name="extras_append"></a>

## <a name="extras"></a>Extra 's

<a name="extra_related_articles"></a>

### <a name="related-articles-from-other-azure-services"></a>Verwante artikelen vanuit andere Azure-services

- [Back-up van uw app Azure App Services in Azure wordt aangegeven](../app-service-web/web-sites-backup.md)

<a name="extra_documentation_tools"></a>

### <a name="documentation-tools"></a>Hulpmiddelen voor documentatie

- [Updates aangekondigd voor Azure SQL-Database](http://azure.microsoft.com/updates/?service=sql-database)

- [Zoeken in de documentatie typen van Microsoft Azure waarop de filters](http://azure.microsoft.com/search/documentation/)

<a name="extra_learning_path_graphics"></a>

### <a name="learning-path-graphics"></a>Learning pad afbeeldingen

- [Leren werken met afbeeldingen pad voor alle Microsoft Azure-services](http://azure.microsoft.com/documentation/learning-paths/)

- [Leerpad: Leer Azure SQL-Database](http://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/)

- [Leerpad: SQL-Database elastische schaal](http://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/)


<!--
AzIxAA.ExtraAppend.sql-database.txt
C:\_MainW\VS15\AzureIndexAllArticles\AzureIndexAllArticles\In-Out\In\

This bullet link is improperly disallowed by publishing automation due to presence of string 'docuXXmentation/arXXticles':
- [Search SQL Database documentation, with filters](http://azure.microsoft.com/docuXXmentation/arXXticles/?service=sql-database)
-->


