<properties
    pageTitle="Database op de server is momenteel niet beschikbaar, verbinding te maken met SQL-Database | Microsoft Azure"
    description="Problemen met de database op server is niet momenteel beschikbaar fout wanneer een toepassing maakt verbinding met SQL-Database."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""
    keywords="database op de server is momenteel niet beschikbaar, verbinding te maken met sql-database"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="daleche"/>

# <a name="error-database-on-server-is-not-currently-available-when-connecting-to-sql-database"></a>Fout 'Database op de server is momenteel beschikbaar' wanneer u verbinding maakt met sql-database
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

Wanneer een toepassing verbinding met een Azure SQL-database maakt, wordt het volgende foutbericht weergegeven:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [AZURE.NOTE] Dit foutbericht wordt weergegeven, is meestal tijdelijk (tijdelijk).

Deze fout treedt op wanneer de Azure-database wordt verplaatst (of opnieuw geconfigureerd) en uw toepassing de verbinding met de SQL-database wordt verbroken. SQL-database opnieuw configureren gebeurtenissen wordt veroorzaakt door een geplande gebeurtenis (bijvoorbeeld een upgrade software) of een ongeplande gebeurtenis (bijvoorbeeld een proces vastlopen of taakverdeling). De meeste nieuwe configuratie gebeurtenissen zijn meestal tijdelijk en moeten worden uitgevoerd in minder dan 60 seconden maximaal. Echter kunnen deze gebeurtenissen af en toe langer duren om te voltooien, zoals wanneer een grote transactie zorgt ervoor een herstel langdurige dat.

## <a name="steps-to-resolve-transient-connectivity-issues"></a>Stappen voor het oplossen van problemen met de tijdelijke serververbinding
1.  Controleer het [Microsoft Azure-Service Dashboard](https://azure.microsoft.com/status) voor alle bekende bijvoorbeeld die zijn opgetreden tijdens de periode waarin de fouten zijn gerapporteerd door de toepassing.
2. Toepassingen die verbinding met een cloudservice maken zoals Azure SQL-Database moet periodiek nieuwe configuratie gebeurtenissen verwachten en implementeren opnieuw logica voor de afhandeling van deze fouten in plaats van deze als toepassingsfouten naar klasnotitieblokken weergegeven aan gebruikers. Bekijk de sectie [tijdelijke fouten](sql-database-connectivity-issues.md) en de aanbevolen procedures en richtlijnen bij het [Ontwikkelen-overzicht voor SQL-Database](sql-database-develop-overview.md) voor meer informatie en algemene strategieën opnieuw ontwerpen. Lees daarna voorbeelden van de code bij [Verbinding bibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md) voor specifieke informatie over.
3.  Als een database de grenzen van de resource nadert, kan het lijken te zijn van een tijdelijke verbindingsprobleem. Zie [het oplossen van prestatieproblemen](sql-database-troubleshoot-performance.md).
4.  Als verbindingsproblemen blijft, of als de duur waarvoor uw toepassing de fout zich voordoet groter is dan 60 seconden of als u meerdere exemplaren van de fout wordt weergegeven in een bepaalde datum, bestand een aanvraag Azure ondersteuning door in te schakelen voor **Ondersteuning krijgen** op de site [Azure ondersteunen](https://azure.microsoft.com/support/options) .

## <a name="next-steps"></a>Volgende stappen
- Als een ander foutbericht wordt weergegeven, als resultaat het [foutbericht wordt weergegeven](sql-database-develop-error-messages.md) voor aanwijzingen over de oorzaak.
- Als het probleem permanente is, bezoekt u de instructies in het [artikel problemen oplossen met betrekking tot een veelgebruikte verbinding met Azure SQL-Database](sql-database-troubleshoot-common-connection-issues.md).
