<properties
    pageTitle="Wat is de SQL-Database? Inleiding tot SQL-Database | Microsoft Azure"
    description="Bekijk de inleiding voor SQL-Database: technische details en mogelijkheden van Microsoft relationele database management systeem (RDBMS) in de cloud."
    keywords="Inleiding tot sql, inleiding tot sql, wat is sql-database"
    services="sql-database"
    documentationCenter=""
    authors="shontnew"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="shkurhek"/>

# <a name="what-is-sql-database-introduction-to-sql-database"></a>Wat is de SQL-Database? Inleiding tot SQL-Database

SQL-Database is een relationele database-service in de cloud op basis van de toonaangevende Microsoft SQL Server-engine, met belangrijke mogelijkheden. SQL-Database biedt overzichtelijk prestaties, schaalbaarheid met geen downtime, bedrijfscontinuïteit en gegevensbescherming, alles met vlakbij nul beheer. U kunt zich richten op ontwikkelen van apps snelle en uw tijd versnellen naar market plaats virtuele machines en infrastructuur beheren. Omdat deze gebaseerd op de [SQL Server](https://msdn.microsoft.com/library/bb545450.aspx) -engine, ondersteunt SQL-Database bestaande SQL Server hulpprogramma's, bibliotheken en API's, waardoor u deze gemakkelijker kunt verplaatsen en uitbreiden in de cloud.

In dit artikel is een inleiding voor SQL-Database core concepten en functies die betrekking hebben op prestaties, schaalbaarheid en beheerbaarheid, met koppelingen naar het verkennen van details. Als u klaar bent om te springen, kunt u [uw eerste SQL-database maken](sql-database-get-started.md) of [een toepassingen elastische database maken](sql-database-elastic-pool-create-portal.md) in minuten. Als u een grondigere Kennismaking wilt, bekijk deze video 30 minuten.

> [AZURE.VIDEO azurecon-2015-get-started-with-azure-sql-database]

## <a name="adjust-performance-and-scale-without-downtime"></a>Prestaties en schaal zonder uitvaltijd aanpassen

SQL-databases is beschikbaar in Basic, Standard en Premium *Servicelagen*. Elke servicelaag biedt [verschillende niveaus van prestaties en mogelijkheden](sql-database-service-tiers.md) ter ondersteuning van lightweight naar heavyweight database werkbelasting. U kunt maken op uw eerste app op een kleine database voor een paar tegoed per maand, klikt u vervolgens [wijzigen de servicelaag](sql-database-scale-up.md) handmatig of via een programma op elk gewenst moment terwijl uw app virus wereldwijd, zonder uitvaltijd naar uw app of uw klanten gaat.

Voor veel en apps voor bedrijven is kunnen databases maken en databaseprestaties van één omhoog of omlaag bellen op verzoek voldoende, vooral als gebruikspatronen relatief overzichtelijk. Hebt u onverwachte gebruikspatronen, dit kunt u maar deze vaste kosten en uw zakelijke model beheren.

[Elastische pools](sql-database-elastic-pool.md) in SQL-Database oplossen dit probleem. Het concept is eenvoudig. U prestaties naar een groep toewijzen en betalen voor de collectieve prestaties van de groep in plaats van één databaseprestaties. U hoeft niet te bellen databaseprestaties omhoog of omlaag. De databases in de groep, genaamd *elastische databases*, wordt automatisch schalen omhoog en omlaag naar voldoen aan bepaalde aanvraag. Elastische databases gebruiken, maar niet langer zijn dan de grenzen van de groep, zodat uw kosten overzichtelijk blijft, zelfs als gebruik van de database niet. Bovendien kunt u [toevoegen aan of verwijderen van databases naar de groep](sql-database-elastic-pool-manage-portal.md), schaalbaarheid van uw app uit een klein aantal databases duizendtallen, alle binnen een budget waarmee u kunt u bepalen. Meer informatie over ontwerppatronen voor SaaS toepassingen elastische van toepassingen gebruiken, raadpleegt u [Ontwerppatronen voor meerdere tenant SaaS toepassingen met Azure SQL-Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

In beide gevallen die u gaat, enkele of elastische, niet op de plek. U kunt samenstellen van één databases met elastische database van toepassingen en de Servicelagen van één databases en groepen maken innovatieve ontwerpen wijzigen. Met de kracht en het bereik van Azure kunt u bovendien mix-and-match Azure services met SQL-Database aan uw wensen voldoet unieke moderne app-ontwerp en nieuwe zakelijke kansen ontgrendelen efficiëntie van kosten- en resourcekalenders station.

Maar hoe kunt u de relatieve prestaties van databases en database pools vergelijken? Hoe weet u het met de rechtermuisknop op stop wanneer u omhoog en omlaag bellen? Het antwoord is de Database transactie eenheid (DTU) voor één databases en de elastische DTU (eDTU) voor elastische databases en groepen van de database. Zie [SQL-Database-opties en prestaties: begrijpen wat is beschikbaar in elke servicelaag](sql-database-service-tiers.md) voor meer informatie.

## <a name="keep-your-app-and-business-running"></a>Uw app en bedrijf

Van Azure industrie toonaangevende 99,99% beschikbaarheid service level agreement [(SLA)](http://azure.microsoft.com/support/legal/sla/), mogelijk gemaakt door een wereldwijd netwerk van Microsoft beheerde datacenters, zorgt u ervoor dat uw 24 x 7-app. Met elke SQL-database, is u profiteren van de ingebouwde gegevensbescherming, fouttolerantie en gegevensbescherming die u anders moet ontwerpen, kopen, maken en beheren. Zelfs zo is, afhankelijk van de vereisten van uw bedrijf, kan u aanvullende lagen van de beveiliging van om ervoor te zorgen dat uw app en uw bedrijf snel in het geval van een noodgevallen, een fout of iets anders kunnen herstellen vraag. Elke servicelaag biedt een ander menu van functies die u gebruiken kunt om en uitvoeren en blijven op die manier met SQL-Database. U kunt point-in-time herstellen gebruiken om te retourneren van een database met een eerdere versie, tot 35 dagen. Bovendien als het hosten van uw databases datacenter een storing ervaringen, kunt u naar databasereplica in een andere regio. Of u kunt replica's gebruiken voor upgrades of verplaatsing voor verschillende regio's.

![SQL-Database geografische-herhaling](./media/sql-database-technical-overview/azure_sqldb_map.png)


Zie [Bedrijfscontinuïteit](sql-database-business-continuity.md) voor meer informatie over de verschillende zakelijke bedrijfscontinuïteit functies die beschikbaar zijn voor verschillende lagen.

## <a name="secure-your-data"></a>Uw gegevens beveiligen
SQL Server heeft een traditie van effen gegevensbeveiliging dat SQL-Database met de functies die toegang beperken, gegevens beveiligen en waarmee u kunt controleren activiteit beschermt. Zie [uw SQL-database beveiligen](sql-database-security.md) voor een beknopt overzicht van beveiligingsopties die u in SQL-Database hebt. Zie het [Beveiligingscentrum voor SQL Server-Database Engine en SQL-Database](https://msdn.microsoft.com/library/bb510589) voor een uitgebreidere weergave van beveiligingsfuncties. En Ga naar het [Vertrouwenscentrum Azure](https://azure.microsoft.com/support/trust-center/security/) voor informatie over van Azure platform beveiliging.

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt een introductie over het SQL-Database lezen en de vraag beantwoord "Wat is de SQL-Database?", dat u iets wilt:

- Zie de [pagina prijzen](https://azure.microsoft.com/pricing/details/sql-database/) voor één database en elastische database kosten vergelijkingen en rekenmachines.
- Meer informatie over [elastische toepassingen](sql-database-elastic-pool.md).
- Aan de slag door uw eerste database te [maken](sql-database-get-started.md).
- [Verbinding maken en de query met SSMS](sql-database-connect-query-ssms.md)
- Uw eerste app in C#, Java, Node.js, PHP, Python of Ruby maken: [verbinding bibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md)
- Zie een index van de titels en beschrijvingen van [alle onderwerpen voor de service Azure sql-database](sql-database-index-all-articles.md).
