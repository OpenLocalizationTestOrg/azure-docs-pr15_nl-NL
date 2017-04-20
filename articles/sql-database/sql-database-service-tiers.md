<properties
    pageTitle="SQL-Database prestaties & opties: Service-lagen | Microsoft Azure"
    description="Vergelijk SQL prestaties en bedrijven bedrijfscontinuïteit databasefuncties van de Servicelagen om te verdelen kosten en de mogelijkheid terwijl u de schaal."
    keywords="Opties voor de database, prestaties van de database"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/10/2016"
    ms.author="carlrab"/>

# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>Opties voor de SQL-Database en prestaties: begrijpen wat is beschikbaar in elke servicelaag

[Azure SQL-Database](sql-database-technical-overview.md) biedt drie Servicelagen met meerdere prestatieniveaus u omgaat met verschillende werkbelastingen. Elk prestatieniveau biedt een toeneemt set hulpmiddelen die zijn ontworpen om uit te voeren steeds sneller worden verwerkt. U kunt elke database in een eigen [servicelaag](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) met een eigen prestatieniveau beheren. U kunt ook meerdere databases in een [elastische toepassingen](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) met een gedeelde set bronnen beheren. De bronnen die beschikbaar zijn voor zelfstandige databases worden uitgedrukt Database transactie-eenheden (DTUs) en voor elastische toepassingen elastische DTUs of eDTUs. Zie [Wat is een DTU](sql-database-what-is-a-dtu.md)voor meer informatie over DTUs en eDTUs. 

In beide gevallen zijn de Servicelagen **eenvoudige**, **Standard**en **Premium**. Opties voor de database in deze lagen lijken voor zelfstandige databases en elastische van toepassingen, maar er zijn extra overwegingen voor elastische toepassingen. In dit artikel geeft details van service niveaus voor zelfstandige databases en elastische toepassingen.

## <a name="service-tiers-and-database-options"></a>Service niveaus en opties voor de database
Eenvoudige, standaard, en Premium Servicelagen hebben allemaal een beschikbaarheid SLA van 99,99% en bieden overzichtelijk prestaties, flexibele bedrijfscontinuïteit opties, beveiligingsfuncties en per uur facturering. De volgende tabel bevat voorbeelden van de beste lagen die geschikt zijn voor andere toepassing werkbelasting.

| Servicelaag | TARGET werkbelasting |
|---|---|
| **Eenvoudige** | Meest geschikt zijn voor een kleine database, meestal één actieve bewerking op een bepaald moment ondersteunende. Voorbeelden hiervan zijn databases die wordt gebruikt voor de ontwikkeling of testen of kleine zelden gebruikt toepassingen. |
| **Standaard** | De optie Ga naar voor de meeste cloud-toepassingen, ondersteunende meerdere gelijktijdige query's. Voorbeelden hiervan zijn werkgroep of web-toepassingen. |
| **Premium** | Ontworpen voor hoge transacties volume, ondersteunende van veel gebruikers tegelijkertijd en vereisen van het hoogste niveau van voorzieningen voor business bedrijfscontinuïteit. Voorbeelden zijn databases ondersteunende bedrijfskritieke toepassingen. |

>[AZURE.NOTE] Edities van web en bedrijven teruggetrokken. Lees de [Veelgestelde vragen over zonsondergang](https://azure.microsoft.com/pricing/details/sql-database/web-business/) als u van plan bent om door te gaan met Web en bedrijven edities.

## <a name="standalone-database-service-tiers-and-performance-levels"></a>Zelfstandige database service niveaus en prestaties
Voor zelfstandige-databases, zijn er meerdere prestatieniveaus binnen elke servicelaag. U hebt de flexibiliteit om te kiezen het niveau die het beste aan de behoeften van uw werkzaamheden. Als u schalen dat omhoog of omlaag wilt, kunt u gemakkelijk de niveaus van uw database te wijzigen. Zie [wijzigen Database Servicelagen en prestaties](sql-database-scale-up.md) voor meer informatie.

Prestatie-eigenschappen hier vermeld zijn van toepassing op databases die zijn gemaakt met behulp van [V12 voor SQL-Database](sql-database-v12-whats-new.md). Ongeacht het aantal databases die worden gehost, uw database wordt een gegarandeerd reeks resources en de verwachte prestatie-eigenschappen van uw database niet worden beïnvloed.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

>[AZURE.NOTE] Zie [Service laag mogelijkheden en beperkingen](sql-database-performance-guidance.md#service-tier-capabilities-and-limits)voor een gedetailleerde uitleg van alle rijen in deze tabel van de lagen service.

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Elastische groep Servicelagen en prestaties in eDTUs
Daarnaast kunnen maken en schaalbaarheid van een database zelfstandige, hebt u ook de optie van het beheren van meerdere databases binnen een [elastische toepassingen](sql-database-elastic-pool.md). Alle databases in een elastische toepassingen delen een gemeenschappelijke set met resources. De prestatie-eigenschappen worden gemeten door *Elastische Database transactie-eenheden* (eDTUs). Wanneer met zelfstandige-databases, pools zich in drie Servicelagen voordoen: **eenvoudige**, **Standard**en **Premium**. Voor toepassingen definiëren deze drie Servicelagen nog steeds de limieten voor de algehele prestaties en verschillende functies.

Groepen kunt databases om te delen en DTU resources gebruiken zonder dat een specifiek prestatieniveau toewijzen aan elke database in de groep nodig hebt. Bijvoorbeeld kunt een database zelfstandige in een standaard groep gaan vanuit met 0 eDTUs naar de maximale eDTU die u instellen wanneer u de groep configureren. Groepen kunt u meerdere databases met variërende werkbelasting efficiënt gebruik eDTU bronnen die beschikbaar zijn aan de hele groep. Zie [Aandachtspunten voor de prijs en prestaties voor een elastische toepassingen](sql-database-elastic-pool-guidance.md) voor meer informatie.

De volgende tabel beschrijft de kenmerken van toepassingen service niveaus.

[AZURE.INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Elke database binnen een groep wordt ook voldoet aan de kenmerken van de database zelfstandige voor die laag. Bijvoorbeeld: de eenvoudige toepassingen geldt een limiet voor aantal sessies per groep 4800-28800, maar een afzonderlijke database binnen een eenvoudige resourcegroep geldt een limiet van de database van 300 sessies.

## <a name="choosing-a-service-tier"></a>Een servicelaag kiezen

Om te bepalen op een servicelaag, starten door te bepalen of de database moet worden van een database zelfstandige of als onderdeel van een elastische toepassingen. 

### <a name="choosing-a-service-tier-for-a-standalone-database"></a>Een servicelaag voor een zelfstandig product-database kiezen

Om te bepalen op een servicelaag voor een database zelfstandige, begint u met het bepalen van de databasefuncties die u nodig hebt om te kiezen uw edition SQL-Database:

- Databasegrootte (maximaal 2 GB voor Basic, 250 GB maximale voor standaard en 500 GB tot de maximaal 1 TB voor Premium - afhankelijk van het prestatieniveau)
- Back-up bewaarperiode database (7 dagen voor Basic, 35 dagen voor standaard en 35 dagen voor Premium)

Als u de SQL-Database-editie hebt vastgesteld, bent u gereed om te bepalen de prestaties hebben voor de database (het aantal DTUs). U kunt raden en kiest u vervolgens [schaal omhoog of omlaag dynamisch](sql-database-scale-up.md) op basis van de werkelijke ervaring. U kunt ook de [DTU Rekenmachine](http://dtucalculator.azurewebsites.net/) gebruiken voor het benaderen van het aantal DTUs nodig. 

### <a name="choosing-a-service-tier-for-an-elastic-database-pool"></a>Een servicelaag voor een toepassingen elastische database te kiezen.

Om te bepalen op de servicelaag voor een elastische database-toepassingen, start u door het bepalen van de databasefuncties die u nodig hebt om te kiezen de servicelaag voor uw groep.

- Omvang van database (2 GB voor Basic, 250 GB voor standaard en 500 GB voor Premium)
- Back-up bewaarperiode database (7 dagen voor Basic, 35 dagen voor standaard en 35 dagen voor Premium)
- Aantal databases per toepassingen (400 voor Basic, 400 voor standaard en 50 voor Premium)
- Maximale opslagcapaciteit per toepassingen (117 GB voor Basic, 1200 voor standaard, en 750 voor Premium)

Als u de servicelaag hebt vastgesteld voor uw groep, bent u gereed om te bepalen de prestaties hebben voor de groep (eDTUs). U kunt raden en vervolgens [schaal omhoog of de schaal omlaag dynamisch](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) op basis van de werkelijke ervaring. U kunt de [DTU Rekenmachine](http://dtucalculator.azurewebsites.net/) gebruiken voor het benaderen van het aantal DTUs nodig voor elke database in een groep om vast te stellen van de bovengrens voor de groep.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de prijzen voor deze lagen op [Prijzen van de SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/).
- Informatie over de details van [elastische pools](sql-database-elastic-pool-guidance.md) en [Aandachtspunten voor de prijs en prestaties voor elastische toepassingen](sql-database-elastic-pool-guidance.md).
- Leer hoe u [Monitor, beheren, en groter of elastische van toepassingen](sql-database-elastic-pool-manage-portal.md) en [Monitor de prestaties van zelfstandige databases](sql-database-single-database-monitor.md).
- Nu u over de lagen SQL-Database weet, ze uitprobeert met een [gratis account](https://azure.microsoft.com/pricing/free-trial/) en informatie over [het maken van uw eerste SQL-database](sql-database-get-started.md).

## <a name="additional-resources"></a>Aanvullende informatie

- [Patronen voor meerdere tenant SaaS toepassingen met Azure SQL-Database ontwerpen](sql-database-design-patterns-multi-tenancy-saas-applications.md)
- [Microsoft Virtual Academy videocursus op elastische database-mogelijkheden in Azure SQL-Database](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
