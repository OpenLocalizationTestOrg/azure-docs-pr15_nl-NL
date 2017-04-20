<properties
    pageTitle="SQL-Database elastische groep prijs en prestaties"
    description="Specifiek voor elastische database pools prijsinformatie."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="05/27/2016"
    ms.author="srinia"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="elastic-database-pool-billing-and-pricing-information"></a>Elastische database groep facturering en prijsinformatie

Elastische database-toepassingen zijn gefactureerd per de volgende kenmerken:

- Een elastische toepassingen gefactureerd na het is gemaakt, zelfs wanneer er geen databases in de groep.
- Een elastische toepassingen wordt per uur gefactureerd. Dit is dezelfde meting frequentie als voor het niveau van de prestaties van één databases.
- Als u het formaat van een elastische toepassingen wordt aangepast aan een nieuwe hoeveelheid eDTUs, klikt u vervolgens de groep niet gefactureerd op basis van de nieuwe hoeveelheid eDTUS totdat de formaat bewerking is voltooid. Dit komt overeen met hetzelfde patroon als het prestatieniveau van zelfstandige databases wijzigen.


- De prijs van een resourcegroep die elastisch is gebaseerd op het aantal eDTUs van de groep. De prijs van een resourcegroep die elastisch is afhankelijk van het aantal en het gebruik van de elastische databases erin.
- Prijs wordt berekend door (aantal groep eDTUs) x (eenheidsprijs per eDTU).

De prijs per eenheid eDTU voor een elastische toepassingen is hoger zijn dan de prijs per eenheid DTU voor een zelfstandig product-database in het hetzelfde niveau van de service. Zie de [prijzen van de SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/)voor meer informatie. 


Als u wilt weten over de lagen eDTUs en -service, raadpleegt u [Opties voor de SQL-Database en prestaties](sql-database-service-tiers.md).

## <a name="next-steps"></a>Volgende stappen

- [Een toepassingen elastische database maken](sql-database-elastic-pool-create-portal.md)
- [Controleren, beheren en de grootte van een elastische database-toepassingen](sql-database-elastic-pool-manage-portal.md)
- [Opties voor de SQL-Database en prestaties: begrijpen wat is beschikbaar in elke servicelaag](sql-database-service-tiers.md)
- [PowerShell-script om aan te duiden databases die geschikt zijn voor een elastische database-toepassingen](sql-database-elastic-pool-database-assessment-powershell.md)
