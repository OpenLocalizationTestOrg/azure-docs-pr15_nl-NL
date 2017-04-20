<properties
   pageTitle="Azure SQL Database Web Business Edition zonsondergang Veelgestelde vragen over en | Microsoft Azure"
   description="Meer informatie over wanneer de Azure SQL-Web- en -databases wordt ingetrokken en leer meer over de functies en functionaliteit van de nieuwe service niveaus."
   services="sql-database"
   documentationCenter="na"
   authors="stevestein"
   manager="jhubbard"
   editor="monicar" />
<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/08/2016"
   ms.author="sstein" />

# <a name="web-and-business-edition-sunset-faq"></a>Web en Business Edition zonsondergang Veelgestelde vragen

Azure SQL-Web en bedrijfsdatabases nu teruggetrokken. De lagen Basic, Standard, Premium en elastische Vervang de retiring Web- en -databases.

Om u te helpen met het upgraden van Web en bedrijfsdatabases, is de SQL-Database-service wordt aanbevolen een juiste service laag en prestaties niveau (prijzen laag) voor elke database op basis van de historische werklast.

**Om toegang te krijgen prijzen laag aanbevelingen:**

- [Upgrade naar SQL-Database V12 met behulp van de Azure portal](sql-database-upgrade-server-portal.md)
- [Upgrade naar SQL-Database V12 via PowerShell](sql-database-upgrade-server-powershell.md)
- [Wijzigen van de prijzen laag van een database Web of en grote ondernemingen](sql-database-service-tier-advisor.md)



## <a name="why-does-the-azure-portal-show-my-web-and-business-edition-databases-as-retired"></a>Waarom wordt mijn Web en edition bedrijfsdatabases zoals buiten gebruik gesteld door de Azure portal worden weergegeven?

Omdat Web en edition bedrijfsdatabases niet meer beschikbaar na September 2015, ziet u de portal Web en bedrijfsdatabases zoals teruggetrokken. Het teruggetrokken label bevat ook een herinnering dat een Web en bedrijfsdatabases moeten worden bijgewerkt naar standaard, Basic of Premium. Zie [upgraden naar Azure SQL Database V12](sql-database-upgrade-server-portal.md)voor gedetailleerde informatie over het bijwerken van bestaande Web of en grote ondernemingen-databases naar de nieuwe Servicelagen.

## <a name="which-new-service-tier-is-the-best-choice-to-upgrade-my-existing-web-or-business-database-to"></a>Welke nieuwe servicelaag is de beste keuze mijn bestaande Web of en grote ondernemingen-database naar upgrade uit te voeren?

Een hoger nieuwe service laag en prestaties niveau voor uw bestaande Web of en grote ondernemingen-database te selecteren, is afhankelijk van de specifieke vereisten voor de functie en prestaties voor uw toepassing.

Gebruik de prijzen laag aanbevelingen beschreven boven of voor gedetailleerde informatie voor u helpen bij het selecteren van een juiste nieuwe servicelaag, raadpleegt u [Upgrade naar Azure SQL Database V12](sql-database-upgrade-server-portal.md).

## <a name="why-is-microsoft-introducing-new-service-tiers"></a>Waarom is Microsoft introductie van nieuwe Servicelagen?

Op basis van feedback van klanten, introduceert Azure SQL-Database nieuwe Servicelagen zodat klanten meer eenvoudig ondersteunen relationele database werkbelasting. De nieuwe lagen zijn ontworpen voor overzichtelijk prestaties over een breed scala van zeven niveaus voor lichte Donker symbool: transacties toepassing aanvragen genereert. De nieuwe lagen bieden bovendien een bereik van zakelijke bedrijfscontinuïteit functies, een sterker beschikbaarheid SLA, grotere database voor minder geld en een verbeterde ervaring voor de facturering.

## <a name="where-can-i-learn-more-about-the-new-service-tiers"></a>Waar vind ik meer informatie over de nieuwe Servicelagen?

Zie voor gedetailleerde informatie over de nieuwe Servicelagen en prestaties model, [Servicelagen](sql-database-service-tiers.md). Voor gedetailleerde informatie voor de nieuwe Servicelagen prijzen, raadpleegt u de [prijzen van de SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="what-features-or-functionality-will-not-be-available-in-basic-standard-and-premium"></a>Welke functies of functionaliteit is alleen beschikbaar in Basic, Standard en Premium?

De functie overkoepelende organisaties wordt ingetrokken met Web en bedrijven edities. Klanten die hun databases schalen moeten wordt aangeraden in plaats daarvan gebruiken of migreren naar [Hulpmiddelen voor databases elastische](sql-database-elastic-scale-get-started.md) voor [Azure SQL-Database](sql-database-elastic-scale-get-started.md), waardoor eenvoudiger maken en beheren van een toepassing die gebruikmaakt van sharding. Een bibliotheek van de client .NET geeft toepassingen bepalen hoe de gegevens op shards en routes OLTP-aanvragen voor het juiste databases zijn toegewezen. Ter ondersteuning van management bewerkingen die opnieuw configureren hoe gegevens worden verdeeld over shards, een sjabloon voor Azure cloud-service niet wordt vermeld dat u van uw eigen Azure-abonnement hosten kunt. Microsoft blijft naast [elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md), maken en publiceren van [aangepaste sharding patronen en procedures richtlijnen](https://msdn.microsoft.com/library/azure/dn764977.aspx) op basis van geleerde lessen van uitgebreide klant afspraken. Nieuwe klanten die nodig schaal out functionaliteit moeten [elastische Databasehulpmiddelen](sql-database-elastic-scale-get-started.md) uitchecken en/of neem contact op met Microsoft Support evalueren van hun opties.

Microsoft ook de database kopiëren ervaring met Premium databases wordt gewijzigd. Eerder premium database quotum is beperkt, maakt u een DATABASE... T-SQL gemaakt als een kopie van in een geschorst Premium-database zonder gereserveerde bronnen die werden dezelfde snelheid als een database. Als het quotum voor premium nu meer vrij beschikbaar is, wordt geschorst Premium niet meer ondersteund. Kopieën van de database wordt gemaakt met de dezelfde editie en prestatieniveau als de bron en hiervan wordt gefactureerd. Klanten kunnen kiezen om het gekopieerde databases naar een andere service laag of prestaties niveau verkleinen van hun kosten desgewenst downgraden. Bestaande geschorst Premium-databases wordt, geconverteerd naar Business edition als onderdeel van deze release. Verwacht wordt dat de back-ups van databases nodig zorgt ervoor dat de introductie van het [Punt-In-Time herstellen](sql-database-recovery-using-backups.md#point-in-time-restore) .

## <a name="how-does-basic-standard-and-premium-improve-my-billing-experience"></a>Hoe Basic, Standard en Premium verbeteren mijn facturering?

Eenvoudige, standaard, Premium Azure SQL-databases worden gefactureerd door het uur en u hebt de mogelijkheid om te schalen dat elke database omhoog of omlaag. U zijn gefactureerd met een vaste snelheid op basis van het hoogste service laag en prestaties niveau dat u voor elk uur kiest. Daarnaast kunnen prestaties (voorbeeld: Basic, S1 en P2) zijn opgedeeld in de factuur die u gemakkelijker om het aantal database dagen/uur u in één maand voor elk prestatieniveau weergegeven weer te geven. Web en bedrijfsdatabases gaat u verder met het gebruik van de Database-eenheden op basis van de grootte van de database worden gefactureerd. Ga naar de [SQL-Database prijzen pagina](https://azure.microsoft.com/pricing/details/sql-database/) voor meer informatie over de prijzen en de verschillen tussen de nieuwe Servicelagen.


## <a name="see-also"></a>Zie ook

[Azure SQL-Database](https://azure.microsoft.com/documentation/services/sql-database/)

[Web en bedrijven prijzen](https://azure.microsoft.com/pricing/details/sql-database/web-business/)

[Servicelagen](sql-database-service-tiers.md)

[Een upgrade uitvoeren naar Azure SQL Database V12](sql-database-upgrade-server-portal.md)
