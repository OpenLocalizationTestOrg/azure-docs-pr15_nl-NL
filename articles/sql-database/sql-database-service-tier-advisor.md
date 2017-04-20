<properties 
   pageTitle="Prijzen van laag aanbevelingen voor Azure SQL-Database" 
   description="Bij het wijzigen van de prijzen zijn niveaus in de Azure-portal prijzen van laag aanbevelingen mits aanbevolen de laag die het meest geschikt is geschikt zijn voor het uitvoeren van een bestaande Azure SQL Database de werklast. Prijzen lagen beschrijven de service laag en prestaties het niveau van een SQL-database." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="08/08/2016"
   ms.author="sstein"/>

# <a name="sql-database-pricing-tier-recommendations"></a>SQL-Database prijzen laag aanbevelingen

 Prijzen laag aanbevelingen suggestie voor de service laag en prestaties niveau dat is het meest geschikt is voor het uitvoeren van de werklast van een bestaande Azure SQL-database.

> [AZURE.NOTE] Prijzen laag aanbevelingen zijn alleen beschikbaar voor het Web en bedrijfsdatabases en elastische database pools-- en is alleen beschikbaar in de [portal van Azure](https://portal.azure.com/).


Krijg laag aanbevelingen prijzen tijdens de volgende taken uitvoeren:

- [De service laag en prestaties niveau wijzigen (prijzen laag) van een SQL-database](sql-database-scale-up.md)
- [Azure SQL server upgraden naar V12](sql-database-upgrade-server-portal.md)
- Blader naar de server V12. Zie [SQL-Database prijzen laag aanbevelingen](sql-database-service-tier-advisor.md).
- [Een toepassingen elastische database maken](sql-database-elastic-pool.md#elastic-database-pool-pricing-tier-recommendations)





## <a name="overview"></a>Overzicht

De SQL-Database-service analyseert de huidige prestaties en functievereisten door vast te stellen historische Resourcegebruik voor een SQL-database. Bovendien wordt de servicelaag minimale aanvaardbaar bepaald op basis van de grootte van de database en ingeschakeld [bedrijfscontinuïteit](sql-database-business-continuity.md) functies. 

Deze gegevens worden geanalyseerd en de service laag en prestaties niveau dat het meest geschikt is geschikt zijn voor het uitvoeren van de normale werklast van de database en onderhouden van dat het huidige functieset is wordt aanbevolen.

- De service wordt de vorige 15 tot 30 dagen van historische gegevens (Resourcegebruik, databasegrootte en activiteit in een database) en een vergelijking tussen de hoeveelheid verbruikte resources en de werkelijke beperkingen van de momenteel beschikbare Servicelagen en de prestaties.
- Gegevens worden geanalyseerd in 15 tweede intervallen en van elk interval resultaatset is ingedeeld in de bestaande servicelaag en prestatieniveau die het meest geschikt is geschikt zijn voor het afhandelen van die resultaatset werkbelasting.
- Deze 15 seconden voorbeelden worden vervolgens samengevoegd in de grotere 15-30 onderneming dag-analyse en het service-laag en prestaties niveau dat kunt optimale verwerking van 95% van de historische werklast wordt aanbevolen.

### <a name="recommendations"></a>Aanbevelingen

Op basis van gebruik van uw database, zijn momenteel 2 categorieën onderverdeeld aanbevelingen die kunnen zich voordoen:


| Aanbeveling | Beschrijving |
| :--- | :--- |
| Upgrade | Upgrade uitvoeren naar een nieuwe laag. |
| Niet beschikbaar | Een database is een minimale werkbelasting of ongeveer 35 dagen van de activiteit vereist. Er zijn niet voldoende gegevens op te geven van een geldige aanbeveling. |

## <a name="getting-pricing-tier-recommendations"></a>Aan de prijzen van laag aanbevelingen

Krijg prijzen van laag aanbevelingen door te selecteren van een bestaande database voor de Web- of en grote ondernemingen, klikt u op **alle instellingen**en klik op van **prijzen laag (schaal DTUs)**. (Prijzen laag aanbevelingen zijn ook beschikbaar wanneer u een [Upgrade van Azure SQL-server naar V12](sql-database-upgrade-server-portal.md).)

1. Meld u aan bij de [portal van Azure](https://portal.azure.com/).
2. Klik op **Bladeren** > **SQL-databases**.
4. Klik in het blad **SQL-databases** op de database die u wilt zien van een aanbeveling voor:

    ![Selecteer database][1]

5. Klik op het blad database, selecteert u **alle instellingen** en selecteer vervolgens **prijzen laag (schaal DTUs)**.


7. De **aanbevolen prijzen van lagen** open kunt u de voorgestelde laag klikt u op en klik vervolgens op de knop **selecteren** om te wijzigen in die laag.

    ![Registreer u voor de Preview-versie][4]

8. (Optioneel) Klik op **details van gebruik weergeven** als u wilt openen van het blad **Prijzen laag aanbeveling Details** is waar u de aanbevolen laag voor de database, een vergelijking van functies tussen huidige en aanbevolen lagen, en een grafiek van de gebruiksanalyse historische resource kunt bekijken.

    ![Registreer u voor de Preview-versie][5]



## <a name="summary"></a>Overzicht

Prijzen laag aanbevelingen bieden een geautomatiseerde ervaring voor het verzamelen van telemetriegegevens voor elke SQL-database en de beste service laag/prestaties niveau combinatie op basis van de behoeften van de werkelijke prestaties en de functievereisten van een database aanbevelen. Klik op het blad instellingen **prijzen laag (schaal DTUs)** voor laag aanbevelingen voor alle databases Web- en prijzen.



## <a name="next-steps"></a>Volgende stappen

Afhankelijk van de details van uw specifieke database plaatsvindt meestal uitvoeren van een upgrade of downgrade niet onmiddellijk. De portal, vindt u meldingen als de database overgangen naar de nieuwe laag of u kunt de upgradestatus controleren bij het controleren van de weergave [sys.dm_operation_status (Azure SQL-Database)](https://msdn.microsoft.com/library/dn270022.aspx) in de SQL-databaseserver hoofddatabase.


<!--Image references-->
[1]: ./media/sql-database-service-tier-advisor/select-database.png
[4]: ./media/sql-database-service-tier-advisor/choose-pricing-tier.png
[5]: ./media/sql-database-service-tier-advisor/usage-details.png


 
