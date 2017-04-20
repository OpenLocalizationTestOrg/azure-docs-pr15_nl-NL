<properties
    pageTitle="SQL-Database: Wat is een DTU? | Microsoft Azure"
    description="Informatie over wat een Azure SQL-Database is transactie-eenheid."
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
    ms.workload="NA"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="explaining-database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Waarin u uitlegt Database transactie-eenheden (DTUs) en elastische Database transactie-eenheden (eDTUs)

In dit artikel wordt uitgelegd Database transactie-eenheden (DTUs) en elastische Database transactie-eenheden (eDTUs) en wat gebeurt er wanneer u de maximale DTUs of eDTUs raken.  

## <a name="what-are-database-transaction-units-dtus"></a>Wat zijn Database transactie-eenheden (DTUs)

Een DTU is een maateenheid voor de resources die zijn gegarandeerd beschikbaar voor een zelfstandige versie van Azure SQL-database op een specifiek prestatieniveau binnen een [zelfstandige database servicelaag](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels). Een DTU is een gemengde maateenheid van processor en geheugen gegevens i/o- en transactie log i/o-in een bepaald door een OLTP vergelijkende werkbelasting ontworpen naar echte OLTP werkbelasting breedteverhouding. De DTUs wordt verdubbeld doordat het prestatieniveau van een database is gelijk aan de set beschikbaar voor die database resource wordt verdubbeld. Een database maken Premium P11 met 1750 DTUs bevat bijvoorbeeld 350 x meer DTU rekenkracht dan een eenvoudige database maken met 5 DTUs. Als u wilt weten over de ontwikkelingsmethodologie achter de OLTP vergelijkende werklast gebruikt om te bepalen de blend DTU, Zie [overzicht van de SQL-Database-vergelijkende](sql-database-benchmark-overview.md).

![Inleiding tot SQL-Database: één database DTUs door laag en niveau](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

U kunt [wijzigen Servicelagen](sql-database-scale-up.md) op elk gewenst moment met minimale uitvaltijd in uw toepassing (doorgaans het gemiddelde berekenen van onder vier seconden). Voor veel en apps voor bedrijven is kunnen databases maken en databaseprestaties van één omhoog of omlaag bellen op verzoek voldoende, vooral als gebruikspatronen relatief overzichtelijk. Hebt u onverwachte gebruikspatronen, dit kunt u maar deze vaste kosten en uw zakelijke model beheren. In dit scenario kunt u een elastische van toepassingen gebruiken met een bepaald aantal eDTUs.

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Wat zijn elastische Database transactie-eenheden (eDTUs)

Een eDTU is een maateenheid voor de reeks resources (DTUs) die tussen een set van databases op een server Azure SQL - met de naam van een [elastische toepassingen](sql-database-elastic-pool.png)kunnen worden gedeeld. Elastische groepen bieden een eenvoudige kosten effectieve oplossing voor het beheren van de prestatiedoelstellingen voor meerdere databases met variërende breed en onverwachte gebruikspatronen. Zie [elastische pools en Servicelagen](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) voor meer informatie.

![Inleiding tot SQL-Database: eDTUs door laag en niveau](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Een groep wordt een bepaald aantal eDTUs, voor een vastgestelde prijs uitgedrukt. In de groep, afzonderlijke databases, krijgen de flexibiliteit auto-schaalfactor binnen parameters instellen. Onder dik laden, kan een database meer eDTUs om te voldoen aan de vraag in beslag nemen. Databases onder light laadtijd verbruikt minder en databases onder geen laden geen eDTUs gebruiken. Resources voor de hele groep in plaats van voor één databases inrichting, wordt uw beheertaken vereenvoudigd. Plus er een overzichtelijk budget voor de toepassingen.

Aanvullende eDTUs kan worden toegevoegd aan een bestaande groep met geen downtime database of geen invloed op de databases in de groep elastische. Op dezelfde manier als extra eDTUs niet meer nodig zijn kunnen ze worden verwijderd uit een bestaande groep op een willekeurige plaats in de tijd. U kunt optellen of aftrekken van databases naar de groep. Als u een database is volgens plan onder-gebruik van resources, verplaatst u deze af.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Hoe kan ik het aantal DTUs die nodig zijn voor mijn werkbelasting vaststellen?

Als u op zoek bent voor het migreren van een bestaande on-premises implementatie of SQL Server VM werkbelasting met Azure SQL-Database, kunt u de [DTU Rekenmachine](http://dtucalculator.azurewebsites.net/) gebruiken voor het benaderen van het aantal DTUs nodig. Voor een bestaande Azure SQL Database-werkbelasting, kunt u [SQL Database Query prestaties inzicht](sql-database-query-performance.md) voor meer informatie over verbruik (DTUs) om meer inzicht in het optimaliseren van de werklast van uw database. U kunt ook de [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV gebruiken om de gegevens van de resource verbruik voor het laatste één uur. U kunt ook kunnen de catalogus weergave [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) ook worden opgezocht krijgen van dezelfde gegevens voor de afgelopen 14 dagen, hoewel bij een lagere beeldkwaliteit van vijf minuten gemiddelden.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Hoe weet ik als ik van een elastische resourcegroep profiteren kan?

Groepen zijn geschikt voor een groot aantal databases met specifieke gebruik patronen. Voor een bepaalde database, wordt dit patroon gekenmerkt door lage Gemiddeld gebruik met relatief onregelmatig gebruik pieken. SQL-Database automatisch evalueert de historische Resourcegebruik van databases in een bestaande SQL-databaseserver en wordt aanbevolen de configuratie van de juiste groep in de portal van Azure. Zie voor meer informatie [Wanneer moet een elastische database-toepassingen worden gebruikt?](sql-database-elastic-pool-guidance.md)

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Wat gebeurt er wanneer ik mijn maximale DTUs raken

Prestaties zijn gekalibreerd en moet voldoen aan zodat de benodigde resources om uit te voeren van uw database werkbelasting snel aan de maximale limieten toegestaan voor de laag-/ prestatieniveau van uw geselecteerde service. Als uw werkzaamheden is kunt u door de limieten op een van de processor/gegevens IO/Log IO limieten, blijft de bronnen aan het maximale aantal toegestane niveau, maar u ziet er waarschijnlijk meer vertragingstijden voor query's. Deze limieten resulteren niet in eventuele fouten, maar in plaats daarvan een vertraging van de werklast, tenzij de vertraging zodat slechte verandert dat query's tijdsinstellingen beginnen. Als u beperkingen voor maximale toegestane gelijktijdige sessies/aanvragen van gebruikers (werknemer threads) zijn raken, ziet u expliciete fouten. Zie [Azure SQL-Database resource limieten](sql-database-resource-limits.md) voor meer informatie over de limiet voor bronnen dan CPU, geheugen, gegevens I/O en transactie log i/o.

## <a name="next-steps"></a>Volgende stappen

- Zie [servicelaag](sql-database-service-tiers.md) voor informatie over de DTUs en eDTUs beschikbaar voor zelfstandige databases en voor elastische toepassingen.
- Zie [Azure SQL-Database resource limieten](sql-database-resource-limits.md) voor meer informatie over de limiet voor bronnen dan CPU, geheugen, gegevens I/O en transactie log i/o.
- Zie [SQL Database Query prestaties inzicht](sql-database-query-performance.md) voor meer informatie over uw verbruik (DTUs).
- Zie [overzicht van de vergelijkende SQL-Database](sql-database-benchmark-overview.md) voor meer informatie over de ontwikkelingsmethodologie achter de OLTP vergelijkende werklast gebruikt om te bepalen de blend DTU.