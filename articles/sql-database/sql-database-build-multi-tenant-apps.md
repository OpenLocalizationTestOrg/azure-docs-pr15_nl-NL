<properties
   pageTitle="Azure SQL-Database meerdere Tenant-Apps gebruiken met moeten worden geïsoleerd en efficiëntie genereert"
   description="Leer hoe SQL-Database meerdere tenant apps genereert"
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
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="builds-multi-tenant-apps-with-azure-sql-database-with-isolation-and-efficiency"></a>Meerdere tenant-Apps gebruiken met Azure SQL-Database moeten worden geïsoleerd en efficiënt genereert

## <a name="leverage-elastic-pools-and-build-more-efficient-multi-tenant-apps"></a>Gebruikmaken van elastische van toepassingen en efficiënter meerdere tenant-apps maken

Als u een ontwikkelaar SaaS schrijven van een app meerdere tenant en veel klanten afhandelen, u vaak compromissen nodig zijn in klant prestaties, beheer en beveiliging. Met Azure SQL Database elastische Database van toepassingen moet u niet langer maken die compromissen. Deze groepen te beheren en meerdere tenant apps controleren en moeten worden geïsoleerd voordelen van een klant per database. Zie [ontwerppatronen voor meerdere tenant SaaS toepassingen met Azure SQL-Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

![opbouwen-multi-tenant-apps](./media/sql-database-build-multi-tenant-apps/sql-database-build-multi-tenant-apps.png)

## <a name="auto-scaling-you-control"></a>Automatisch schalen u bepalen

Pools schaal automatisch prestaties en opslagcapaciteit voor elastische databases in de browser. U kunt bepalen van de prestaties die zijn toegewezen aan een groep, toevoegen of verwijderen elastische databases op aanvraag en prestaties van elastische databases handhaven de totale kosten van de groep definiëren. Dit betekent dat u geen zorgen over het beheren van het gebruik van afzonderlijke databases te maken.

[Lees de documentatie](sql-database-elastic-pool.md)

## <a name="intelligent-management-of-your-environment"></a>Intelligente beheer van uw omgeving

Ingebouwde hoekformaatgreep aanbevelingen identificeren proactief databases die u van toepassingen profiteren wilt. Deze aanbevelingen toestaan 'what-if'-analyse voor snelle optimalisatie om te voldoen aan de prestatiedoelstellingen van uw. RTF-prestaties bewaken en probleemoplossing van dashboards kunt u historische toepassingen gebruik visualiseren.

[Lees de documentatie](sql-database-elastic-pool-guidance.md)

## <a name="performance-and-price-to-meet-your-needs"></a>Prestaties en prijs aan uw behoeften

Eenvoudige, standaard, en Premium-groepen bieden u een brede breed scala van prestaties, opslag en opties prijzen. Toepassingen kunnen maximaal 400 elastische databases bevatten. Elastische databases kunnen automatisch-schalen tot 1000 elastische database transactie-eenheden (eDTU).

[Lees de documentatie](https://azure.microsoft.com/pricing/details/sql-database/?b=16.50)

## <a name="elastic-tools"></a>Elastische hulpmiddelen

Naast elastische van toepassingen, zijn er functies van de SQL-Database om te helpen beheren operationele activiteiten binnen meerdere databases:

**Cross-databasequery's en rapportage uitvoeren.**  
[Elastische databasequery's](sql-database-elastic-query-overview.md) kunt u uitvoeren van query's of rapporten over databases in de groep elastische en toegang tot externe gegevens die zijn opgeslagen in verschillende databases van uw groep in één keer.

**Cross databasetransacties uitvoeren.**  
[Elastische databasetransacties](sql-database-elastic-transactions-overview.md) kunt u uitvoeren van transacties die meerdere databases in de SQL-Databases's beslaan en bewerkingen uitvoeren (dat wil zeggen als de verwerking van financiële transacties via databases of bij het bijwerken van voorraad in een database en orders).

**Bewerkingen uitvoeren op meerdere databases worden uitgevoerd.**  
[Elastische database taken](sql-database-elastic-jobs-overview.md) uitvoeren beheerdersbewerkingen zoals PowerPoint indexen opnieuw moet maken of bijwerken van schema's door elke database in uw elastische-groep.

Ga naar de homepage om te zien wat anders SQL-Database te bieden heeft.
[Het uitchecken](https://azure.microsoft.com/services/sql-database/) 

## <a name="next-steps"></a>Volgende stappen

Krijg een [gratis Azure abonnement](https://azure.microsoft.com/get-started/) en [uw eerste Azure SQL-Database maken](sql-database-get-started.md).

## <a name="additional-resources"></a>Aanvullende informatie

Kennismaken met alle [functies van de SQL-Database](https://azure.microsoft.com/services/sql-database/).
 
Bekijk de [technisch overzicht van de SQL-Database](sql-database-technical-overview.md).  
