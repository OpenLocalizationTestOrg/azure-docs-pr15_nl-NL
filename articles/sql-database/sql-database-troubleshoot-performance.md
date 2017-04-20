<properties
    pageTitle="SQL-Database prestaties optimaliseren tips | Microsoft Azure"
    description="Tips voor het prestaties optimaliseren in Azure SQL-Database via evaluatie en verbetering."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    keywords="SQL-prestaties optimaliseren, databaseprestaties optimaliseren, sql-prestaties optimaliseren tips, sql database prestaties optimaliseren"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-database-performance-tuning-tips"></a>SQL-Database prestaties afstemmen tips
U kunt wijzigen van de [servicelaag](sql-database-service-tiers.md) van één database of het eDTUs van een resourcegroep elastische database vergroten op elk gewenst moment prestaties te verbeteren, maar u kunt identificeren verkoopkansen te verbeteren en queryprestaties eerst optimaliseren. Ontbrekende indexen en slecht geoptimaliseerde query's zijn veelvoorkomende oorzaken van slechte databaseprestaties. In dit artikel bevat richtlijnen voor prestaties optimaliseren in SQL-Database.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="steps-to-evaluate-and-tune-database-performance"></a>Prestaties van databases stappen evalueren en afstemmen
1.  Klik in de [Portal van Azure](https://portal.azure.com) **SQL-databases**op, selecteer de database en gebruik vervolgens de controle-grafiek om te zoeken voor resources hun maximum bijna is bereikt. DTU verbruik wordt standaard weergegeven. Klik op **bewerken** als u wilt wijzigen van het tijdsbereik en de waarden die worden weergegeven.
2.  [Query prestaties inzicht](sql-database-query-performance.md) gebruikt om te beoordelen van de query's met DTUs en gebruik [SQL Database Advisor](sql-database-advisor.md) om aanbevelingen voor het maken en daar neerzetten indexen, parameters voorzien van query's en het oplossen van problemen schema te bekijken.
3.  U kunt dynamische management weergaven (DMVs), uitgebreide gebeurtenissen (Xevents) en de Query Store in SSMS gebruiken om prestatieparameters in realtime. Zie het [onderwerp van prestaties richtlijnen](sql-database-performance-guidance.md) voor gedetailleerde controleren en afstemmen van tips.


    > [AZURE.IMPORTANT] Het wordt aanbevolen dat u altijd de nieuwste versie van Management Studio gebruiken om u te blijven synchroon met updates voor Microsoft Azure en SQL-Database. [SQL Server Management Studio bijwerken](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="steps-to-improve-database-performance-with-more-resources"></a>Stappen voor het verbeteren van de prestaties van de database met meer informatiebronnen
1.  Voor één-databases kunt u de [Servicelagen wijzigen](sql-database-scale-up.md) op aanvraag databaseprestaties te verbeteren.
2.  Voor meerdere databases, kunt u overwegen [elastische database pools](sql-database-elastic-pool-guidance.md) aan de resources automatisch nieuwe schaal.
