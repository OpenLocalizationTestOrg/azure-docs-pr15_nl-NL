<properties
    pageTitle="Azure SQL-Databases met Azure automatisering beheren | Microsoft Azure"
    description="Meer informatie over hoe de automatisering Azure-service kan worden gebruikt voor het beheren van Azure SQL-databases op schaal."
    services="sql-database, automation"
    documentationCenter=""
    authors="jodoglevy"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/26/2016"
    ms.author="jolevy"/>



#<a name="managing-azure-sql-databases-using-azure-automation"></a>Azure SQL-Databases met Azure automatisering beheren

Deze handleiding laat u kennismaken met de automatisering Azure-service en hoe deze kan worden gebruikt om beheer van uw Azure SQL-databases te vereenvoudigen.


## <a name="what-is-azure-automation"></a>Wat Azure automatisering is?

[Azure automatisering](https://azure.microsoft.com/services/automation/) is een Azure-service voor het beheer van de cloud met procesautomatisering vereenvoudigen. Azure-automatisering gebruikt, kunnen langdurige, handmatige lastige en vaak herhaalde taken worden geautomatiseerd om de betrouwbaarheid, efficiency en tijd aan de waarde voor uw organisatie te verbeteren.

Azure automatisering biedt een uiterst betrouwbare en ten zeerste beschikbaar werkstroom execution engine die wordt aangepast aan uw wensen als uw organisatie in omvang groeit. In Azure automatisering, processen volledig uitschakelen handmatig door 3e leveranciers of regelmatig zodat taken plaatsvinden precies wanneer nodig.

Verlaag operationele realiseren en vrijgemaakt IT / DevOps personeel kunt richten op werk die worden toegevoegd bedrijven waarde door de cloud-beheertaken automatisch worden uitgevoerd door Azure automatisering te bewegen.


## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Hoe kunt Azure automatisering Azure SQL-databases beheren?

Azure SQL-Database kunnen worden beheerd in Azure automatisering met behulp van de [Azure SQL Database PowerShell-cmdlets](https://msdn.microsoft.com/library/dn546723.aspx) die beschikbaar zijn in de [Hulpmiddelen voor Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx). Azure automatisering heeft deze beschikbaar Azure SQL Database PowerShell-cmdlets uit het vak, zodat u al uw taken voor het beheer van SQL DB binnen de service kunt uitvoeren. U kunt ook deze cmdlets in Azure automatisering met de cmdlets voor andere Azure-services, complexe om taken te automatiseren via Azure services en 3e partijen systemen koppelen.

Azure automatisering, heeft ook de mogelijkheid om te communiceren met SQL-servers rechtstreeks door SQL-opdrachten via PowerShell.

De [galerie met Azure automatisering runbook](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) bevat tal van de team- en -community runbooks product aan de slag beheer van Azure SQL-Databases, andere Azure services en 3e partijen systemen automatiseren. Galerie runbooks opnemen:

 * [SQL-query's uitvoeren op een SQL Server-database](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
 * [Verticaal schalen (omhoog of omlaag) een Azure SQL-Database op een planning](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
 * [Een SQL-tabel afkappen als de database de maximale grootte nadert](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
 * [Tabellen in een Azure SQL-Database indexeren als ze zijn sterk gefragmenteerd](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Volgende stappen

U kunt de basisbeginselen van Azure automatisering en hoe deze kan worden gebruikt voor het beheren van Azure SQL-databases hebt geleerd, volgt u deze koppelingen voor meer informatie over Azure automatisering.

- [Overzicht van de Azure automatisering](../automation/automation-intro.md)
- [Mijn eerste runbook](../automation/automation-first-runbook-graphical.md)
- [Azure automatisering learning-kaart](https://azure.microsoft.com/documentation/learning-paths/automation/)
- [Azure automatisering: Uw SQL-Agent in de Cloud](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 
 
