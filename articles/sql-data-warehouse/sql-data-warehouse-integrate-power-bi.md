<properties
   pageTitle="Power BI gebruiken met SQL datawarehouse | Microsoft Azure"
   description="Tips voor het gebruik van Power BI met Azure SQL Data Warehouse voor het ontwikkelen van oplossingen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/31/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="use-power-bi-with-sql-data-warehouse"></a>Power BI met SQL datawarehouse gebruiken
Als met Azure SQL-Database kan SQL Data Warehouse Direct Connect gebruiker om te profiteren van krachtige logische naar beneden duwen samen met de analysefuncties van Power BI.  Query's met directe verbinding kunt maken, terug naar uw Azure SQL Data Warehouse realtime verzonden als u de gegevens verkennen.  Deze, gecombineerd met de schaal van SQL Data Warehouse, kunnen gebruikers dynamische rapporten maakt in minuten tegen TB aan gegevens.  Bovendien kan de introductie van de knop openen in Power BI gebruikers rechtstreeks verbinden met Power BI hun SQL Data Warehouse zonder het verzamelen van informatie uit andere delen van Azure.

Bij gebruik van Direct Connect neemt notitie:

+ Geef de volledig gekwalificeerde servernaam wanneer u verbinding maakt (Zie hieronder voor meer informatie)
+ Controleer firewallregels voor de database zijn geconfigureerd om 'Toestaan toegang tot Azure services'.
+ Elke actie zoals het selecteren van een kolom of het toevoegen van een filter wordt rechtstreeks datawarehouse query
+ Tegels worden vernieuwd ongeveer elke 15 minuten (vernieuwen niet hoeft te worden gepland)
+ Q & A is niet beschikbaar voor gegevenssets Direct Connect
+ Wijzigingen in het schema zijn niet opgenomen automatisch
+ Alle Direct Connect-query's na 2 minuten

Deze beperkingen en notities kunnen wijzigen, zoals we gaat u verder met de ervaringen te verbeteren. De stappen om verbinding te worden hieronder beschreven.  

## <a name="using-the-open-in-power-bi-button"></a>Werken met de knop 'Openen in Power BI'
De eenvoudigste manier om te schakelen tussen uw SQL Data Warehouse en de Power BI is met de knop openen in Power BI. Deze knop kunt u naadloos begint met het maken van nieuwe dashboards in Power BI.  

1.  Aan de slag gaat u naar uw exemplaar van SQL Data Warehouse in de klassieke Azure-Portal.
2.  Klik op de knop 'Openen in Power BI'.
3.  Als we niet kunt u rechtstreeks aanmelden, of als u geen een Power BI-account hebt, moet u moeten aanmelden.  
4.  U wordt doorgestuurd naar de pagina SQL Data Warehouse verbinding met de gegevens uit uw SQL-Data Warehouse vooraf is ingevuld.
5.  Na het invoeren van uw referenties wordt u volledig verbonden met uw SQL Data Warehouse.

## <a name="connecting-through-the-power-bi-portal"></a>Een verbinding via de Power BI-portal
Naast de knop openen in Power BI, kunnen gebruikers ook verbinding maakt met hun SQL Data Warehouse via het Power BI-Portal.

1.  Klik op gegevens ophalen onderaan in het navigatiedeelvenster.
2.  Selecteer 'Databases'.
3.  Selecteer één keer op de pagina Databases 'Azure SQL Data Warehouse' en klik op 'Verbinden'.
4.  Voer de benodigde verbindingsinformatie.  Uw servernaam en de naam van de database vindt u in de Portal Azure.
5.  U wordt doorgestuurd terug naar de hoofdpagina van Power BI en nadat de verbinding wordt gemaakt van een nieuwe vermelding onder 'Gegevenssets' wordt weergegeven met de naam van uw exemplaar.  
6.   U kunt klikken op de nieuwe gegevensset om te verkennen van alle tabellen en weergaven in uw database. Een kolom selecteren om een query terug naar de bron, dynamisch maken van uw visual te verzenden. Deze visuele elementen kunnen worden opgeslagen in een nieuw rapport en vastgemaakt weer aan uw dashboard.

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->
