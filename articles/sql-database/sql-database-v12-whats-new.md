<properties
    pageTitle="Wat is er nieuw in SQL-Database V12 | Microsoft Azure"
    description="Beschrijft waarom business-systemen die Azure SQL-Database in de cloud gebruikt door een upgrade naar versie V12 nu profiteren."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="genemi"/>


# <a name="whats-new-in-sql-database-v12"></a>Wat is er nieuw in SQL-Database V12


In dit onderwerp worden de vele voordelen die de nieuwe V12-versie van Azure SQL-Database over versie V11 heeft:.


We blijven functies toevoegen aan V12. Dus is het raadzaam te bezoeken van onze webpagina Service-Updates voor Azure en het gebruik van de filters:


- Gefilterd op de [service van de SQL-Database](https://azure.microsoft.com/updates/?service=sql-database).
- Gefilterd op algemene beschikbaarheid [(GA) aankondigingen](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) voor functies van de SQL-Database.


De meest recente informatie over limieten voor de resource voor SQL-Database wordt beschreven bij:<br/>[Beperkingen voor de Resource van de azure SQL-Database](sql-database-resource-limits.md).


## <a name="increased-application-compatibility-with-sql-server"></a>Een grotere compatibiliteit met SQL Server


Een belangrijke doelstelling voor SQL-Database V12 is om te verbeteren de compatibiliteit met Microsoft SQL Server-2014, en voor de compatibiliteit als nieuwe versies van SQL Server zijn vrijgegeven. Onder andere gebieden realiseert V12 overeenkomst met SQL Server in het belangrijke gebied van programmeerfuncties. Bijvoorbeeld:

- [Ingebouwde JSON-ondersteuning](https://msdn.microsoft.com/library/dn921897.aspx)

- [Venster functies](http://msdn.microsoft.com/library/ms189798.aspx), met [via](http://msdn.microsoft.com/library/ms189461.aspx)

- [XML-indexen](http://msdn.microsoft.com/library/bb934097.aspx) en [Selectief XML-indexen](http://msdn.microsoft.com/library/jj670104.aspx)

- [Het bijhouden van wijzigingen](http://msdn.microsoft.com/library/bb933875.aspx)

- [SELECTEREN... IN](http://msdn.microsoft.com/library/ms188029.aspx)

- [Zoeken in volledige tekst](http://msdn.microsoft.com/library/ms142571.aspx)

- [ALTER DATABASE binnen bereik van configuratie (Transact-SQL)](http://msdn.microsoft.com/library/mt629158.aspx)

Zie [hier](sql-database-transact-sql-information.md) op de kleine set functies die nog niet worden ondersteund in SQL-Database.


### <a name="compatibility-level-130"></a>Compatibiliteitsniveau 130


> [AZURE.IMPORTANT] Het niveau van hun compatibiliteit beginnen bij 130, wat overeenkomt met Microsoft SQL Server 2016 GA. hebben *nieuwe* databases op Azure SQL Database V12 gemaakt vanaf **juni 2016**,
> 
> U kunt `ALTER DATABASE YourDatabase SET COMPATIBILITY_LEVEL = 120` als u liever.
> 
> Databases die zijn gemaakt voordat juni 2016 hebben geen het niveau van hun compatibiliteit gewijzigd door deze wijziging van standaard. En niet het niveau van een database gewijzigd door een upgrade uitvoert voor V11: naar V12 is.



Voor meer informatie over hoe u uw belangrijkste query's tussen de meest recente versus het niveau van de vorige compatibiliteit kunt vergelijken, raadpleegt u:

- [Verbeterde queryprestaties compatibiliteitsniveau 130 in Azure SQL-Database](sql-database-compatibility-level-query-performance-130.md)



## <a name="more-premium-performance-new-performance-levels"></a>Meer premium prestaties, nieuwe prestaties


In V12 verhoogd we de Database transactie-eenheden (DTUs) toegewezen aan alle Premium prestatieniveaus met 25% gratis. Nog meer prestatieverbeteringen kunnen worden bereikt met nieuwe functies beschikbaar, zoals:


- Ondersteuning voor in het geheugen [columnstore indexen](http://msdn.microsoft.com/library/gg492153.aspx).
- [Tabel partitioneren door rijen](http://msdn.microsoft.com/library/ms187802.aspx) met gerelateerde verbeteringen in de [Tabel afkappen](http://msdn.microsoft.com/library/ms177570.aspx).
- De beschikbaarheid van dynamische management weergaven [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx) om te controleren en afstemmen van prestaties.


### <a name="reliable-performance"></a>Betrouwbare prestaties


Als uw clientprogramma verbinding maakt met SQL-Database V12 terwijl de klant wordt uitgevoerd op een Azure virtuele machine (VM), moet u de volgende bereikwaarden op de VM openen:

- 11000-11999
- 14000-14999


Klik [hier](sql-database-develop-direct-route-ports-adonet-v12.md) voor meer informatie over de poorten voor SQL-Database V12. Verbeterde prestaties in SQL-Database V12 de poorten nodig.


## <a name="better-support-for-cloud-saas-vendors"></a>Betere ondersteuning voor cloud SaaS leveranciers


Alleen in V12 uitgebracht we het nieuwe standaard prestatieniveau S3 en de openbare preview van [elastische database-toepassingen](sql-database-elastic-pool.md). Elastische database van toepassingen is een oplossing die zijn ontworpen voor cloud SaaS leveranciers.  Met elastische database van toepassingen, kunt u het volgende doen:


- DTUs delen tussen databases kosten voor grote aantallen databases te verlagen.
- [Elastische database taken](sql-database-elastic-jobs-overview.md) voor het beheren van databases bij het op schaal uitvoeren.


## <a name="security-enhancements"></a>Verbeterde beveiliging


Beveiliging is een belangrijk probleem voor iedereen die wordt uitgevoerd van hun bedrijf in de cloud. De meest recente beveiligingsfuncties uitgebracht in V12 opnemen:


- [Beveiliging op gebruikersniveau rij](http://msdn.microsoft.com/library/dn765131.aspx) (RLS)
- [Dynamische gegevens Masking](sql-database-dynamic-data-masking-get-started.md)
- [Container databases](http://msdn.microsoft.com/library/ff929188.aspx)
- [Rollen toepassing](http://msdn.microsoft.com/library/ms190998.aspx) beheerd met verlenen, weigeren, INTREKKEN
- [Transparante gegevensversleuteling](http://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) (TDE)
- [Verbinding maken met SQL-Database met Azure Active Directory-verificatie](sql-database-aad-authentication.md)
 - SQL-Database ondersteunt nu de verificatie van de Azure Active Directory, een om verbinding te maken met SQL-Database met behulp van identiteiten in Azure Active Directory (Azure AD). U kunt de identiteit van de databasegebruikers en andere Microsoft-services op één centrale locatie centraal beheren met Azure Active Directory-verificatie.
- [Altijd versleuteld](https://msdn.microsoft.com/library/mt163865.aspx) (in preview) maakt versleuteling transparante naar toepassingen en maakt clients gevoelige gegevens in clienttoepassingen zonder de toetsen versleuteling delen met SQL-Database versleutelen.


## <a name="increased-business-continuity-when-recovery-is-needed"></a>Bedrijfscontinuïteit verhoogd als herstel noodzakelijk is


V12 biedt verbeterde herstel punt doelstellingen (productoutput) en geschatte herstel tijden (ERTs):


| Functie van Business bedrijfscontinuïteit | Eerdere versie | V12 |
| :-- | :-- | :-- |
| Geografische herstellen | • Vrijgegeven Productieorder < 24 uur.<br/>• Invoegen < 12 uur. | • Vrijgegeven Productieorder < 1 uur staan.<br/>• Invoegen < 12 uur. |
| Actieve geografische-herhaling | • Vrijgegeven Productieorder < 5 minuten.<br/>• Invoegen < 1 uur staan. | • Vrijgegeven Productieorder < 5 seconden.<br/>• Invoegen < 30 seconden. |


Zie [bedrijfscontinuïteit SQL-Database](sql-database-business-continuity.md) voor meer informatie.


## <a name="more-reasons-to-upgrade-now"></a>Meer redenen dat u nu bijwerken


Zijn er veel goede redenen waarom klanten upgraden nu naar Azure SQL Database V12 vanuit V11 moeten::


- SQL-Database V12 heeft een lijst met functies dan de functies van V11: lang.
- We gaat u verder met de nieuwe functies toevoegen aan V12, maar geen nieuwe functies, worden toegevoegd aan V11:.
- De meeste nieuwe functies zijn uitgebracht op SQL-Database V12 voordat ze zijn vrijgegeven voor Microsoft SQL Server.


## <a name="are-you-using-v12-already"></a>Gebruikt u V12 al?


Een eenvoudige manier om te bekijken of er een database of logische server waarop een eerdere versie van de SQL-Database-service is het volgende doen:


1. Ga naar de [Azure-Portal](https://portal.azure.com/).
2. Klik op **Bladeren**.
3. Klik op **SQL-Servers**.
4. Het pictogram naast de server of database Hiermee wordt aan het verhaal:
 - ![Pictogram voor een server v12](./media/sql-database-v12-whats-new/v12_icon.png) **V12 logische server**
 - ![Pictogram voor de eerdere versie server](./media/sql-database-v12-whats-new/earlier_icon.png) **eerdere versie logische server**


Een andere methode om na te gaan van de versie wordt uitgevoerd de `SELECT @@version;` instructie in uw database, en de resultaten die vergelijkbaar is met bekijken:


- **12**.0.2000.10 &nbsp; *(versie V12)*
- **11**.0.9228.18 &nbsp; *(versie V11:)*


Een database V12 kan worden gehost op een logische V12-server. En een server V12 kunt hosten alleen V12 databases.


Als u nog niet op V12 uitvoert, kunt u uw logische server bijwerken door de stappen in het [upgraden naar SQL Database V12 op hun plaats staan](sql-database-v12-plan-prepare-upgrade.md).


## <a name="V12AzureSqlDbPreviewGaTable"></a>Algemene beschikbaarheid regio 's


- Vóór 31 juli 2015, had alle regio's gepromoveerd naar algemene beschikbaarheid (GA).
- V12 is uitgebracht in December 2014, maar alleen op de status van de Preview-versie.

[Aanvullende gebruiksrechtovereenkomst voor Microsoft Azure voorbeelden](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
