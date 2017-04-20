<properties
    pageTitle="Beperkingen voor de Resource van de Azure SQL-Database"
    description="Deze pagina bevat enkele algemene resource-limieten voor Azure SQL-Database."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="10/13/2016"
    ms.author="carlrab" />


# <a name="azure-sql-database-resource-limits"></a>Limieten van de resource Azure SQL-Database

## <a name="overview"></a>Overzicht

Azure SQL-Database worden beheerd door de resources die beschikbaar zijn voor een database met behulp van twee verschillende regelingen: **Resources beheermodel** en **Afdwingen van limieten**. In dit onderwerp wordt uitgelegd dat deze twee hoofdgebieden van bronbeheer.

## <a name="resource-governance"></a>Resource-beheermodel
Een van de ontwerpdoelen van het van de niveaus van de service Basic, Standard en Premium is bedoeld voor Azure SQL-Database zich gedraagt als wanneer de database wordt uitgevoerd op de eigen computer, volledig geïsoleerd van andere databases. Resource-beheermodel emuleert dit probleem. Als het geaggregeerde Resourcegebruik bereikt het maximum aantal beschikbare CPU, geheugen, Log i/o- en gegevens i/o-resources die zijn toegewezen aan de database, wordt de resource beheermodel wachtrij van query's in uitvoering en de resources toewijzen aan de in de wachtrij query's terwijl ze vrij te maken.

Als op een speciale machine resulteert gebruik van alle beschikbare bronnen in een langere uitvoering van query's, wat leiden de opdracht-outs op de client tot kunnen dat momenteel wordt uitgevoerd. Toepassingen met agressieve opnieuw logica en toepassingen die uitvoeren van query's voor de database met een hoge frequentie kunnen foutberichten optreden als u wilt een nieuwe query's uitvoeren wanneer het maximum aantal gelijktijdige aanvragen is bereikt.

### <a name="recommendations"></a>Aanbevelingen:
Het Resourcegebruik, evenals de tijden gemiddelde antwoord van query's bewaken wanneer bijna ten het optimaal gebruik van een database. Wanneer zich voordoen tijdens hoger query vertragingstijden hebt u gewoonlijk drie opties:

1.  Verminder de hoeveelheid verzoeken voor oproepen naar de database om te voorkomen dat time-out en de stapel omhoog van aanvragen.

2.  Een hoger prestatieniveau toewijzen aan de database.

3.  Query's om te beperken het Resourcegebruik van elke query optimaliseren. Zie de sectie Query afstemmen/Hinting in het artikel Azure SQL Database prestaties richtlijnen voor meer informatie.

## <a name="enforcement-of-limits"></a>Afdwingen van limieten
Resources dan CPU, geheugen Log i/o- en i/o-gegevens worden afgedwongen door weigeren van nieuwe aanvragen wanneer limieten worden bereikt. Clients ontvangt een [foutbericht wordt weergegeven](sql-database-develop-error-messages.md) , afhankelijk van de limiet van die is bereikt.

Het aantal verbindingen met een SQL-database en het aantal gelijktijdige aanvragen dat kan worden verwerkt, zijn bijvoorbeeld beperkt. SQL-Database kunt het aantal verbindingen met de database moet groter zijn dan het aantal gelijktijdige aanvragen ter ondersteuning van groepsgewijze. Terwijl de hoeveelheid verbindingen die beschikbaar zijn kan eenvoudig worden beïnvloed door de toepassing, wordt de hoeveelheid parallelle aanvragen is vaak tijden moeilijker voor het schatten en om te bepalen. Met name tijdens piek laadtijd wanneer de toepassing hetzij stuurt te veel aanvragen of de database bereikt de resource bestandsgrootten en weer verdergaat stapel werknemer threads vanwege langer actief query's, kunnen fouten zich voordoen.

## <a name="service-tiers-and-performance-levels"></a>Service niveaus en prestaties

Er zijn Servicelagen en prestaties voor beide zelfstandige database en elastische van toepassingen.

### <a name="standalone-databases"></a>Zelfstandige databases

Voor een database zelfstandige, worden de grenzen van een database gedefinieerd door de database service laag en prestaties niveau. De volgende tabel beschrijft de kenmerken van eenvoudige, Standard en Premium databases op variërende prestaties.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

### <a name="elastic-pools"></a>Elastische van toepassingen

Bronnen in [elastische toepassingen](sql-database-elastic-pool.md) delen verschillende databases in de groep. De volgende tabel beschrijft de kenmerken van Basic, Standard en Premium elastische database-toepassingen.

[AZURE.INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Zie de beschrijvingen in [Service laag mogelijkheden en -limieten](sql-database-performance-guidance.md#service-tier-capabilities-and-limits)voor een uitgevouwen definitie van elke resource in de vorige tabellen weergegeven. Zie voor een overzicht van service niveaus, [Azure SQL Database Service niveaus en prestaties](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>Andere limieten SQL-Database

| Gebied | Limiet | Beschrijving |
|---|---|---|
| Databases met automatische exporteren per abonnement | 10 | Geautomatiseerde exporteren kunt u een aangepast schema voor het back-ups van uw SQL-databases maken. Zie voor meer informatie [SQL-Databases: ondersteuning voor SQL Database-uitvoer wordt automatisch](http://weblogs.asp.net/scottgu/windows-azure-july-updates-sql-database-traffic-manager-autoscale-virtual-machines).|
| Database per server | Maximaal 5000 | Maximaal 5000 databases zijn per server op V12-servers toegestaan. |  
| DTUs per server | 45000 | 45000 DTUs zijn beschikbaar per server op V12 servers voor inrichten databases, elastische pools en datawarehouses. |



## <a name="resources"></a>Resources

[Azure-abonnement en limieten van de Service, quota en beperkingen](../azure-subscription-service-limits.md)

[Azure SQL Database-Service niveaus en prestaties](sql-database-service-tiers.md)

[Foutberichten voor SQL-Database clientprogramma 's](sql-database-develop-error-messages.md)
