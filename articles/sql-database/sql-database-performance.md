<properties 
   pageTitle="Azure SQL Database-prestaties inzicht | Microsoft Azure" 
   description="De Azure SQL-Database biedt prestaties hulpmiddelen om te identificeren gebieden die u kunnen de huidige queryprestaties verbeteren." 
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
   ms.date="07/19/2016"
   ms.author="sstein"/>

# <a name="sql-database-performance-insight"></a>SQL-Database prestaties inzicht

Azure SQL-Database biedt prestaties hulpmiddelen waarmee u kunt identificeren en de prestaties van uw databases verbeteren door intelligente afstemmen acties en aanbevelingen te geven. 

1. Blader naar de database in de [Portal van Azure](http://portal.azure.com) en klik op **alle instellingen** > **prestaties **  >  **Overzicht** om de pagina van de **prestaties** te openen. 


2. Klik op **aanbevelingen** als u wilt openen van de [SQL-Database Advisor](#sql-database-advisor)en **query's** om te openen [Query prestaties inzicht](#query-performance-insight)op.

    ![Prestaties weergeven](./media/sql-database-performance/entries.png)



## <a name="performance-overview"></a>Prestatieoverzicht

Klikken op **Overzicht** of op de tegel **prestaties** , gaat u naar het Prestatiedashboard voor uw database. Deze weergave bevat een overzicht van de databaseprestaties van uw en kunt u met de prestaties optimaliseren en problemen oplossen. 

![Prestaties](./media/sql-database-performance/performance.png)

- De tegel **aanbevelingen** biedt een overzicht van het optimaliseren van aanbevelingen voor de database (top 3 aanbevelingen worden weergegeven als er meer). Te klikken op deze tegel Hiermee, gaat u naar **Advisor voor SQL-Database**. 
- De tegel **afstemmen activiteit** biedt een overzicht van de lopende en voltooide optimaliseren van acties voor de database, waarin u een snelle weergave in de geschiedenis van de activiteit optimaliseren. Te klikken op deze tegel gaat u naar de volledige afstemmen geschiedenisweergave voor de database.
- De tegel **Auto-aanpassing** ziet u de configuratie auto-aanpassing voor de database (welke afstemmen acties worden geconfigureerd om te worden automatisch toegepast op uw database). Te klikken op deze tegel, opent het dialoogvenster van de configuratie automatisering.
- De tegel **databasequery's** ziet u het overzicht van de prestaties van de query's voor de database (algehele DTU gebruik en top resource door andere query's). Te klikken op deze tegel Hiermee, gaat u naar **Query prestaties inzicht**.



## <a name="sql-database-advisor"></a>SQL-Database Advisor


[SQL-Database Advisor](sql-database-advisor.md) bevat intelligente afstemmen aanbevelingen die de prestaties van uw database kunt verbeteren. 

- Aanbevelingen over welke indexeert te maken of weg (en een optie om toe te passen index aanbevelingen automatisch zonder tussenkomst van de gebruiker en automatisch terugdraaien aanbevelingen die een negatieve invloed op prestaties hebben).
- Onder de aanbevelingen wanneer schema problemen worden aangegeven in de database.
- Onder de aanbevelingen wanneer de query's kunnen profiteren van parameterquery's.




## <a name="query-performance-insight"></a>Query prestaties inzicht

[Query prestaties inzicht](sql-database-query-performance.md) kunt u minder tijd hebt probleemoplossing prestaties van de database door op te geven:

- Meer inzicht in uw verbruik van databases (DTU). 
- De bovenste CPU door andere query's die mogelijk worden geconfigureerd voor verbeterde prestaties. 
- De mogelijkheid om Inzoomen op de details van een query. 


## <a name="additional-resources"></a>Aanvullende informatie

- [Azure SQL-Database prestaties richtlijnen voor één databases](sql-database-performance-guidance.md)
- [Wanneer moet een elastische database-toepassingen worden gebruikt?](sql-database-elastic-pool-guidance.md)