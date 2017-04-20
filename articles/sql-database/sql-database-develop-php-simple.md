<properties
    pageTitle="Verbinding maken met SQL-Database via PHP op Windows | Microsoft Azure"
    description="Geeft een voorbeeld PHP-programma dat vanuit een Windows-client verbinding maakt met Azure SQL-Database en koppelingen naar de benodigde software-onderdelen die nodig zijn voor de client."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-php-on-windows"></a>Verbinding maken met SQL-Database via PHP in Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


In dit onderwerp ziet u hoe u kunt verbinding maken met Azure SQL-Database van een clienttoepassing in PHP die wordt uitgevoerd op Windows geschreven.

## <a name="step-1--configure-development-environment"></a>Stap 1: Ontwikkelomgeving configureren

[Ontwikkelomgeving voor PHP ontwikkeling configureren](https://msdn.microsoft.com/library/mt720663.aspx)

## <a name="step-2-create-a-sql-database"></a>Stap 2: Een SQL-database maken

Zie de [pagina aan de slag](sql-database-get-started.md) voor meer informatie over het maken van een voorbeelddatabase.  Het is belangrijk dat u volgt de richtlijnen voor het maken van een **sjabloon voor AdventureWorks-database**. In de voorbeelden hieronder werken alleen met het **schema voor AdventureWorks**.


## <a name="step-3-get-connection-details"></a>Stap 3: Verbindingsgegevens ophalen

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]


## <a name="step-4-run-sample-code"></a>Stap 4: Voorbeeldcode uitvoeren

* [Aankoopbewijs concept verbinden met SQL met PHP](https://msdn.microsoft.com/library/mt720665.aspx)
* [Resiliently verbinden met SQL met PHP](https://msdn.microsoft.com/library/mt720667.aspx)


## <a name="next-steps"></a>Volgende stappen

* Bekijk het [SQL-Database ontwikkelen-overzicht](sql-database-develop-overview.md)
* Meer informatie over het [Microsoft PHP stuurprogramma voor SQL Server](https://msdn.microsoft.com/library/dn865013.aspx)
* Zie voor meer informatie aangaande PHP installatie en het gebruik, [SQL Server-Databases openen met PHP](http://social.technet.microsoft.com/wiki/contents/articles/1258.accessing-sql-server-databases-from-php.aspx).

## <a name="additional-resources"></a>Aanvullende informatie 

* [Ontwerppatronen voor meerdere tenant SaaS toepassingen met Azure SQL-Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Kennismaken met alle [functies van de SQL-Database](https://azure.microsoft.com/services/sql-database/)
