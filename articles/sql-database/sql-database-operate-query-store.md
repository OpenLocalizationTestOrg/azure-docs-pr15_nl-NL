<properties
   pageTitle="Query winkel in Azure SQL-Database"
   description="Informatie over het werken de Store Query in Azure SQL-Database"
   keywords=""
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="sqldb-performance"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="operating-the-query-store-in-azure-sql-database"></a>De Query winkel in Azure SQL-Database 

Query Store in Azure is een volledig beheerde database-functie continu worden verzameld en geeft weer gedetailleerde historische informatie over alle query's. U kunt Query Store nadenken als strekking van een vliegtuig flight gegevens recorder die aanzienlijk queryprestaties probleemoplossing voor cloud vereenvoudigd en on-premises-klanten. In dit artikel wordt uitgelegd dat bepaalde aspecten van de Query winkel in Azure wordt aangegeven. Deze querygegevens vooraf verzamelde gebruikt, kunt u snel te analyseren en dus besteden meer tijd aan hun bedrijf en oplossen van prestatieproblemen. 

Query Store is sinds [Algemeen beschikbaar](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) in Azure SQL-Database November 2015. Query Store is de basis voor prestatieanalyse en functies, zoals [SQL Database Advisor en Prestatiedashboard](https://azure.microsoft.com/updates/sqldatabaseadvisorga/)optimaliseren. Op het moment van het publiceren van dit artikel wordt wordt Store van de Query uitgevoerd in databases met meer dan 200.000 in Azure wordt aangegeven, voor het verzamelen van gegevens die betrekking hebben van een query om meerdere maanden, zonder onderbreking.

> [AZURE.IMPORTANT] Microsoft stelt activeert Query Store voor alle Azure SQL-databases (oude en nieuwe). 

## <a name="optimal-query-store-configuration"></a>Optimale Query Store configuratie

Dit onderwerp vindt optimale configuratie standaardwaarden die zijn ontworpen om ervoor te zorgen betrouwbare werking van de Query Store en afhankelijke functies, zoals [SQL Database Advisor en Prestatiedashboard](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Standaardconfiguratie is geoptimaliseerd voor doorlopende gegevens verzamelen, die minimale tijd besteed aan de Staten uit/alleen-lezen is.

| Configuratie | Beschrijving | Standaard | Opmerking |
| ------------- | ----------- | ------- | ------- |
| MAX_STORAGE_SIZE_MB | Hiermee geeft u de limiet voor de gegevensruimte in de die Query Store in klantendatabase z uitvoeren kunt | 100 | Afgedwongen voor nieuwe databases |
| INTERVAL_LENGTH_MINUTES | Hiermee definieert u de grootte van tijdvenster waarin verzamelde runtime statistieken voor abonnementen van de query worden samengevoegd en behouden. Elk abonnement actieve query heeft maximaal één rij voor een periode gedefinieerd met behulp van deze configuratie | 60   | Afgedwongen voor nieuwe databases |
| STALE_QUERY_THRESHOLD_DAYS | Op tijd gebaseerde opruimen beleid waarmee wordt bepaald van de bewaarperiode van permanente runtime statistieken en niet-actieve query 's | 30 | Afgedwongen voor nieuwe databases en databases met vorige standaard (367) |
| SIZE_BASED_CLEANUP_MODE | Hiermee geeft u op of automatische gegevens opschonen plaats vindt wanneer de Query Store gegevensgrootte nadert de limiet | Auto | Afgedwongen voor alle databases |
| QUERY_CAPTURE_MODE | Hiermee geeft u of alle query's of alleen een subset van query's worden bijgehouden | Auto | Afgedwongen voor alle databases |
| FLUSH_INTERVAL_SECONDS | Hiermee geeft u maximale periode waarin vastgelegd runtime statistieken in het geheugen worden bewaard voordat naar schijf | 900 | Afgedwongen voor nieuwe databases |
||||||

> [AZURE.IMPORTANT] Deze standaardinstellingen worden automatisch toegepast in de laatste fase van Query Store activering in alle Azure SQL-databases (Zie voorafgaand aan de belangrijke opmerking). Na deze light omhoog, won't op Azure SQL-Database, configuratiewaarden die door klanten, tenzij ze een negatieve invloed primaire werkbelasting of betrouwbare bewerkingen van de Query-archief hebben worden gewijzigd.

Als u blijven met uw aangepaste instellingen wilt, gebruikt u [DATABASE wijzigen met de opties van de Query Store](https://msdn.microsoft.com/library/bb522682.aspx) wilt terugdraaien configuratie in de vorige staat. Bekijk de [Aanbevolen procedures in de Query-Store](https://msdn.microsoft.com/library/mt604821.aspx) om te leren hoe boven hebt gekozen optimale configuratieparameters.

## <a name="next-steps"></a>Volgende stappen

[SQL-Database prestaties inzicht](sql-database-performance.md)

## <a name="additional-resources"></a>Aanvullende informatie

Voor meer informatie uitchecken in de volgende artikelen:

- [Een recorder flight gegevens voor de database](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database) 

- [Prestaties controleren met behulp van de Query-Store](https://msdn.microsoft.com/library/dn817826.aspx)

- [Scenario's voor het gebruik van query-Store](https://msdn.microsoft.com/library/mt614796.aspx)

- [Prestaties controleren met behulp van de Query-Store](https://msdn.microsoft.com/library/dn817826.aspx) 
