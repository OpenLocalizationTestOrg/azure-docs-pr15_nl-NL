<properties
    pageTitle="Databaseprestaties in Azure SQL-Database controleren | Microsoft Azure"
    description="Meer informatie over de opties voor het controleren van de database met Azure hulpprogramma's en dynamische management weergaven."
    keywords="database cloud databaseprestaties, controleren"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/27/2016"
    ms.author="carlrab"/>

# <a name="monitoring-database-performance-in-azure-sql-database"></a>Databaseprestaties in Azure SQL-Database controleren
Controleren van de prestaties van een SQL-database in Azure begint met het Resourcegebruik ten opzichte van het niveau van de prestaties van de database is die u kiest voor controle. Cmdlets voor controle kunt die u bepalen of uw database overtollige capaciteit heeft of problemen ondervindt omdat resources maximum zijn is overschreden en bepaal vervolgens of het is tijd om het prestatieniveau en [servicelaag](sql-database-service-tiers.md) van uw database te passen. U kunt uw database grafische hulpprogramma's in de [portal van Azure](https://portal.azure.com) of gebruiken van de SQL- [dynamische management weergaven](https://msdn.microsoft.com/library/ms188754.aspx)controleren.

## <a name="monitor-databases-using-the-azure-portal"></a>Monitor databases met behulp van de Azure portal

U kunt in de [portal van Azure](https://portal.azure.com/)van één database gebruik controleren door de database te selecteren en te klikken op de grafiek **controle** . Hiermee opent u een **meetwaarde** -venster dat u wijzigen kunt door te klikken op de knop **grafiek bewerken** . De doelstellingen van de volgende toevoegen:

- CPU-percentage
- DTU percentage
- Gegevens IO percentage
- Database grootte percentage

Nadat u deze gegevens hebt toegevoegd, kunt u blijven deze weer in het diagram **controleren** met meer informatie over het **Metrisch** -venster. Alle vier de doelstellingen geven het percentage Gemiddeld gebruik ten opzichte van de **DTU** van uw database. Zie het artikel [Servicelagen](sql-database-service-tiers.md) voor meer informatie over DTUs.

![Laag controle van de database basisprestaties van service.](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

U kunt ook waarschuwingen configureren op de prestatiegegevens. Klik op de knop **toevoegen een melding** in het venster **Metrisch** . Voer de wizard om te configureren van de waarschuwing. Als de doelstellingen een bepaalde drempel overschrijdt of als de meetwaarde kleiner is dan een bepaalde drempelwaarde hebt u de optie een melding.

Bijvoorbeeld als u de werklast op uw database om te laten groeien verwachten, kunt u kiezen voor het configureren van een e-mailwaarschuwing zodra uw database 80% op een van de prestatiegegevens bereikt. U kunt dit gebruiken als een vroegtijdige waarschuwing om te bepalen wanneer u moet mogelijk tot overschakelen naar de volgende hoger prestatieniveau.

De prestatiegegevens kunt u bepalen of u zich kunt downgraden naar een lager prestatieniveau. Wordt ervan uitgegaan dat u een database standaard S2 gebruikt en alle prestatiegegevens weergeven die de database op gemiddelde niet meer dan 10% op een bepaald moment gebruikt. Is het mogelijk dat de database ook in de standaard S1 blijven werken. Echter Let werkbelasting die oploopt of variëren voordat u de besluit om naar een lager prestatieniveau te gaan.

## <a name="monitor-databases-using-dmvs"></a>Monitor databases met DMVs

Dezelfde parameters die beschikbaar zijn in de portal zijn ook beschikbaar via het systeemweergaven: [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) in de logische **basispagina** database van uw server en [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) in de database. Gebruik **sys.resource_stats** als u nodig hebt om de minder gedetailleerde gegevens over een langere periode te houden. Gebruik **sys.dm_db_resource_stats** als u nodig hebt om meer gedetailleerde gegevens in een kleinere bepaald tijdsbestek te controleren. Zie [Azure SQL Database prestaties richtlijnen](sql-database-performance-guidance.md#monitoring-resource-use-with-sysresourcestats)voor meer informatie.

>[AZURE.NOTE] **sys.dm_db_resource_stats** geeft als resultaat een lege resultatenset wanneer in Web en edition bedrijfsdatabases, waarin teruggetrokken gebruikt.

Voor elastische database van toepassingen, kunt u afzonderlijke databases in de groep controleren met de technieken die in deze sectie worden beschreven. Maar u kunt ook de groep als geheel controleren. Zie voor informatie [beeldscherm en een elastische database-toepassingen beheren](sql-database-elastic-pool-manage-portal.md).
