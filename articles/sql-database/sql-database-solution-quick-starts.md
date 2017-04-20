<properties
   pageTitle="Azure SQL Database-oplossing snel aan de slag | Microsoft Azure"
   description="Meer informatie over Azure SQL Database-oplossingen"
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
   ms.workload="sqldb-quickstart"
   ms.date="09/06/2016"
   ms.author="carlrab"/>

# <a name="explore-azure-sql-database-solution-quick-starts"></a>Kennismaken met Azure SQL Database-oplossing snel aan de slag

In dit artikel bevat een overzicht van de Azure SQL Database-oplossing snelle wordt gestart. Deze snelle wordt gestart, bevinden zich in de bibliotheek van de voorbeelden GitHub SQL Server en het gebruik van de SQL-Database in geen volledige oplossing op basis van de echte scenario's demonstreren. Zie [Azure SQL-Database verkennen zelfstudies](sql-database-explore-tutorials.md)voor eenvoudige zelfstudies met stapsgewijze instructies die het gebruik van een bepaalde functie van de SQL-Database te illustreren.

## <a name="try-the-wingtiptickets-demo-and-hands-on-lab"></a>Probeer de WingTipTickets demo en praktische testomgeving

De [Azure SQL Database WingTipTickets](https://github.com/microsoft/wingtiptickets) demo en praktische testomgeving demonstreren een Azure SQL-Database en de steekproef Azure op zoekopdrachten gebaseerde-toepassing die wordt gebruikt om te verkopen overleg tickets.


## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools"></a>Verzamelen en bijhouden van gegevens over zoekgebruik resource over meerdere van toepassingen

[Oplossing snel aan de slag: elastische groep telemetrielogboek via PowerShell](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) biedt een oplossing voor het verzamelen en bijhouden van SQL-Database Resourcegebruik over meerdere toepassingen in een abonnement. Wanneer u een groot aantal databases in een abonnement hebt, is het lastig zijn om de elke elastische toepassingen afzonderlijk te houden.

U lost dit probleem, kunt u SQL-Database PowerShell-cmdlets en T-SQL-query's voor het verzamelen van gegevens over zoekgebruik resource uit meerdere toepassingen en hun databases combineren. Hiermee kunt u controleren en Resourcegebruik efficiënter te analyseren.

Deze aan de slag biedt een set PowerShell-scripts en T-SQL-query's samen met documentatie aan de werking van de oplossing en hoe u deze implementeert houden.

## <a name="get-started-with-elastic-database-in-an-saas-scenario"></a>Aan de slag met elastische Database in een scenario SaaS

 [Oplossing snel aan de slag: elastische groep aangepaste dashboard voor SaaS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) biedt een oplossing voor een Software-als-een-oplossing (SaaS)-scenario waarbij de maakt gebruik van de functie elastische Database van SQL-Database een efficiënt en scalable databaseback-end verstrekken voor een toepassing SaaS.

In deze oplossing vindt u informatie over de uitvoering van een web-app. Deze WebApp kunt u het selectievakje laden dat wordt gemaakt op een elastische-database op een aggregaat laden die gebruikmaakt van een aangepaste dashboard aanvulling op de portal van Azure visualiseren.

Deze aan de slag, vindt u een laden genereren en controleren in de browser samen met de documentatie over de werking van de app en hoe u dit gebruikt.

## <a name="create-an-azure-sql-database-by-using-code-first-development-and-the-entity-framework"></a>Een Azure SQL-database maken met behulp van eerste Code ontwikkelen en entiteit Framework

De video en het voorbeeld in [Eerste Code naar een nieuwe Database](https://msdn.microsoft.com/data/jj193542.aspx) bevat een inleiding tot het ontwikkelen van eerste Code die is bedoeld voor een nieuwe database. Dit scenario is bedoeld voor een database die niet bestaat, maar die met Code eerste worden gemaakt. U kunt ook het scenario Hiermee maakt u een nieuwe, lege database waarnaar Code eerste nieuwe tabellen voegt.

Code eerst kunt u uw model definiëren met behulp van door C# of Visual Basic .NET klassen. U kunt optioneel aanvullende configuratie uitvoeren met behulp van de kenmerken van uw lessen en eigenschappen of met behulp van een fluent API.

## <a name="integrate-elastic-database-tools-into-an-entity-framework-application"></a>Hulpmiddelen voor databases elastische integreren in een toepassing entiteit Framework

De steekproef [elastische Database client-bibliotheek met entiteit Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) ziet u de wijzigingen die u wilt aanbrengen in een toepassing entiteit Framework integreren met [Hulpmiddelen voor databases elastische](sql-database-elastic-scale-get-started.md). De focus heeft [shard toewijzen management](sql-database-elastic-scale-shard-map-management.md) en [gegevens-afhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) met de entiteit Framework Code eerste methode voor het bericht.

De [Eerste Code aan een nieuwe database steekproef voor EF](http://msdn.microsoft.com/data/jj193542.aspx) fungeert als onze actieve voorbeeld overal in dit voorbeeld. De voorbeeldcode bij dit document maakt deel uit van de set elastische Database hulpmiddelen voor voorbeelden in de voorbeelden van de Visual Studio-code.

## <a name="integrate-elastic-database-tools-with-row-level-security"></a>Hulpmiddelen voor databases elastische integreren met beveiliging op gebruikersniveau rij

[Multitenant toepassingen met hulpmiddelen voor databases elastische en beveiliging op gebruikersniveau rij](sql-database-elastic-tools-multi-tenant-row-level-security.md) ziet u de wijzigingen die u wilt aanbrengen in een toepassing entiteit Framework [elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md) integreren met [beveiliging op gebruikersniveau rij](https://msdn.microsoft.com/library/dn765131). In dit voorbeeld ziet u hoe deze technologieën samen gebruiken om een toepassing met een gegevenslaag ten zeerste scalable die ondersteuning biedt voor multitenant shards te maken.

U doen dit met behulp van ADO.NET SqlClient of entiteit Framework. In dit voorbeeld wordt de [elastische Database client-bibliotheek met entiteit Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) uitgebreid met ondersteuning voor multitenant shard databases.
Hiermee maakt u een eenvoudige console-programma voor het maken van blogs en berichten, met vier tenants en twee multitenant shard-databases.

## <a name="create-online-surveys-with-the-tailspin-surveys-application"></a>Online enquêtes maken met de toepassing de enquêtes

In dit [voorbeeld van de enquêtes toepassing](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md) is een multitenant webtoepassing, genaamd enquêtes, waarmee gebruikers kunnen online enquêtes maken. Enkele belangrijke vragen over het beheren van de identiteit van de gebruikers in een multitenant toepassing, waaronder aanmelding, verificatie, autorisatie en app rollen-adressen voor de steekproef.

## <a name="learn-about-the-latest-security-features-of-sql-database-with-the-contoso-clinic-demo-application"></a>Meer informatie over de nieuwste beveiligingsfuncties van SQL-Database met de Contoso kliniek Demo-toepassing

Deze [Contoso kliniek Demo toepassing](https://github.com/Microsoft/azure-sql-security-sample) gepresenteerd de nieuwste beveiligingsfuncties van SQL-Database.

## <a name="next-steps"></a>Volgende stappen

[Azure SQL Database-zelfstudies verkennen](sql-database-explore-tutorials.md)
