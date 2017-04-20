<properties
    pageTitle="Verbinding maken met SQL-Database via Java met JDBC op Windows | Microsoft Azure"
    description="Geeft het voorbeeld van een Java-code dat kunt u verbinding maakt met Azure SQL-Database. In dit voorbeeld worden JDBC en deze op een Windows-clientcomputer wordt uitgevoerd."
    services="sql-database"
    documentationCenter=""
    authors="LuisBosquez"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="lbosq"/>


# <a name="connect-to-sql-database-by-using-java-with-jdbc-on-windows"></a>Verbinding maken met SQL-Database via Java met JDBC in Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


In dit onderwerp vindt een voorbeeld van de Java code die u kunt verbinding maken met Azure SQL-Database. De steekproef Java is gebaseerd op de Java Development Kit (JDK) versie 1.8. De steekproef maakt verbinding met een Azure SQL-Database met behulp van het stuurprogramma voor JDBC.

## <a name="step-1--configure-development-environment"></a>Stap 1: Ontwikkelomgeving configureren

[Ontwikkelomgeving voor Java ontwikkeling configureren](https://msdn.microsoft.com/library/mt720658.aspx)

## <a name="step-2-create-a-sql-database"></a>Stap 2: Een SQL-database maken

Zie de [pagina aan de slag](sql-database-get-started.md) wilt weten hoe u een database maken.  

## <a name="step-3-get-connection-string"></a>Stap 3: Een verbindingsreeks ophalen

[AZURE.INCLUDE [sql-database-include-connection-string-jdbc-20-portalshots](../../includes/sql-database-include-connection-string-jdbc-20-portalshots.md)]

> [AZURE.NOTE] Als u het stuurprogramma JTDS JDBC gebruikt en u moet toevoegen ' ssl = vereisen ' naar de URL van de verbinding tekenreeks en u moet de volgende optie voor het JVM instellen "-Djsse.enableCBCProtection=false". Deze optie JVM een oplossing voor een beveiligingsprobleem uitschakelt, dus controleer dat u weet welke risico betrokken is voordat u deze optie instelt.

## <a name="step-4-run-sample-code"></a>Stap 4: Voorbeeldcode uitvoeren

* [Aankoopbewijs concept verbinden met SQL Java gebruiken](https://msdn.microsoft.com/library/mt720656.aspx)

## <a name="next-steps"></a>Volgende stappen

* Bezoek het [Java Developer Center](/develop/java/).
* Bekijk het [SQL-Database ontwikkelen-overzicht](sql-database-develop-overview.md)
* Meer informatie over het [Microsoft JDBC stuurprogramma voor SQL Server](https://msdn.microsoft.com/library/mt484311.aspx)

## <a name="additional-resources"></a>Aanvullende informatie 

* [Ontwerppatronen voor meerdere tenant SaaS toepassingen met Azure SQL-Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* Kennismaken met alle [functies van de SQL-Database](https://azure.microsoft.com/services/sql-database/)
