<properties
    pageTitle="SQL-Database ontwikkelen overzicht | Microsoft Azure"
    description="Meer informatie over de beschikbare connectivity bibliotheken en aanbevolen procedures voor toepassingen verbinden met SQL-Database."
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="annemill"/>

# <a name="sql-database-development-overview"></a>SQL-Database ontwikkelen-overzicht
In dit artikel worden doorlopen de eenvoudige overwegingen waarmee een ontwikkelaar moet houden bij het schrijven van code verbinding maken met Azure SQL-Database.

## <a name="language-and-platform"></a>Taal en platform
Er zijn voorbeelden van code beschikbaar voor verschillende programmeren talen en platforms. Hier vindt u koppelingen naar voorbeelden van de code op: 

* Meer informatie: [verbinding bibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md)

## <a name="resource-limitations"></a>Resource-beperkingen
Azure SQL-Database worden beheerd door de resources die beschikbaar zijn voor een database met behulp van twee verschillende regelingen: Resources beheermodel en afdwingen van limieten.

* Meer informatie: [Azure SQL-Database resource limieten](sql-database-resource-limits.md)

## <a name="security"></a>Beveiliging
Azure SQL-Database bevat bronnen voor het beperken van toegang, gegevens beveiligen en het monitoren, activiteiten op een SQL-Database.

* Meer informatie: [uw SQL-Database beveiligen](sql-database-security.md)

## <a name="authentication"></a>Verificatie
* Azure SQL-Database ondersteunt zowel SQL Server-verificatie-gebruikers en aanmeldingen, evenals [verificatie van Azure Active Directory](sql-database-aad-authentication.md) -gebruikers en aanmeldingen.
* U moet een bepaalde database, in plaats van *de hoofddatabase* de standaardinstelling opgeven.
* U kunt de instructie Transact-SQL **myDatabaseName; gebruiken** op SQL-Database niet gebruiken om te schakelen naar een andere database.
* Meer informatie: [SQL-Database beveiliging: toegang en login databasebeveiliging beheren](sql-database-manage-logins.md)

## <a name="resiliency"></a>Tolerantie
Als een tijdelijke fout optreedt tijdens het verbinding maken met SQL-Database, moet uw code het gesprek opnieuw.  Het is raadzaam dat logica gebruik backoff logica, probeer zodat deze de SQL-Database niet worden overspoeld met meerdere klanten tegelijk opnieuw.

* Codeweergave-voorbeelden: voorbeelden van de code die illustreren proberen logica, vindt u voorbeelden voor de taal van uw keuze op: [verbinding bibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md)
* Meer informatie: [foutberichten voor SQL-Database clientprogramma's](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Verbindingen beheren
* Overschrijven in uw client verbinding logica, de standaard-out zodanig 30 seconden.  De standaardwaarde van 15 seconden is te kort voor verbindingen die afhankelijk van internet zijn.
* Als u een [groep](http://msdn.microsoft.com/library/8xx3tyca.aspx)gebruikt, moet u de verbinding het chatgesprek uw programma wordt niet actief gebruikt en wordt niet voorbereid opnieuw sluiten.

## <a name="network-considerations"></a>Aandachtspunten bij netwerk
* Controleer op de computer waarop uw clientprogramma, of dat de firewall kunt uitgaande TCP-communicatie op poort 1433.  Meer informatie: [een firewall Azure SQL-Database configureren](sql-database-configure-firewall-settings.md)
* Als uw clientprogramma verbinding maakt met SQL-Database V12 terwijl de klant wordt uitgevoerd op een Azure virtuele machine (VM), moet u bepaalde poortbereiken op de VM openen. Meer informatie: [poorten voorbij 1433 voor ADO.NET 4.5 en V12 voor SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md)
* Clientverbindingen met Azure SQL Database V12 soms de proxy overslaan en rechtstreeks ermee aan de database. Poorten dan 1433 worden belangrijke. Meer informatie: [poorten voorbij 1433 voor ADO.NET 4.5 en V12 voor SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md)

## <a name="data-sharding-with-elastic-scale"></a>Gegevens Sharding met elastische schaal
Elastische schaal eenvoudiger schaalbaarheid af (en in). 

* [Ontwerppatronen voor meerdere tenant SaaS toepassingen met Azure SQL-Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Afhankelijke omleiding van gegevens](sql-database-elastic-scale-data-dependent-routing.md)
* [Aan de slag met Azure SQL Database elastische schaal Preview](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Volgende stappen

Kennismaken met alle [functies van de SQL-Database](https://azure.microsoft.com/services/sql-database/).
