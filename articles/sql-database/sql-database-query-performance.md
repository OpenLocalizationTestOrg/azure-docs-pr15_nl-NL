<properties 
   pageTitle="Azure SQL Database Query prestaties inzicht" 
   description="Query prestatiecontroles, geeft u de meeste CPU-gebruik query's voor een Azure SQL-Database." 
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
   ms.date="08/09/2016"
   ms.author="sstein"/>

# <a name="azure-sql-database-query-performance-insight"></a>Azure SQL Database Query prestaties inzicht


Beheren en optimaliseren van de prestaties van relationele databases is een uitdaging waarvoor aanzienlijk expertise en tijd investering. Query prestaties inzicht kunt u minder tijd probleemoplossing databaseprestaties biedt de volgende nodig:

- Meer inzicht in uw verbruik van databases (DTU). 
- De meest voorkomende query's door aantal keer CPU/duur/uitgevoerd, die mogelijk worden geconfigureerd voor verbeterde prestaties.
- De mogelijkheid om te zoomen op de details van een query, de tekst en de geschiedenis van Resourcegebruik weergeven. 
- Prestaties optimaliseren aantekeningen die acties uitgevoerd door de [SQL Azure-Database Advisor](sql-database-advisor.md) weergeven  



## <a name="prerequisites"></a>Vereisten voor

- Query prestaties inzicht is alleen beschikbaar met Azure SQL Database V12.
- Query prestaties inzicht is vereist dat [Query Store](https://msdn.microsoft.com/library/dn817826.aspx) op uw database actief is. Als Query Store niet actief is, wordt de portal vraagt u deze inschakelen.

 
## <a name="permissions"></a>Machtigingen

De volgende [Rolgebaseerd toegangsbeheer](../active-directory/role-based-access-control-configure.md) -machtigingen zijn vereist voor het gebruik van de Query prestaties inzicht: 

- **Lezer**, **eigenaar**, **Inzender**, **SQL DB Inzender**of **SQL Server Inzender** machtigingen zijn vereist voor het weergeven van de bovenste resource in beslag nemen query's en grafieken. 
- **Eigenaar**, **Inzender**, **SQL DB Inzender**of **SQL Server Inzender** machtigingen zijn vereist voor het weergeven van de tekst van de query.



## <a name="using-query-performance-insight"></a>Gebruik van de Query prestaties inzicht

Query prestaties inzicht is eenvoudig te gebruiken:

- Open [Azure-portal](https://portal.azure.com/) en database die u wilt onderzoeken welke gevolgen vinden. 
  - Selecteer in menu van de linkerkant, onder ondersteuning en probleemoplossing, "Query prestaties inzicht".
- Klik op het eerste tabblad raadpleegt u de lijst met bovenste resource gebruiken query's.
- Selecteer een afzonderlijke query om de berichtdetails te bekijken.
- Open [SQL Azure-Database Advisor](sql-database-advisor.md) en controleren of eventuele aanbevelingen beschikbaar zijn.
- Gebruik de schuifregelaars of pictogrammen voor het wijzigen van waargenomen interval in-en uitzoomen.

    ![van Prestatiedashboard](./media/sql-database-query-performance/performance.png)

> [AZURE.NOTE] Een paar uur van gegevens moet worden vastgelegd door Query Store voor SQL-Database te leveren query prestaties inzichten. Als de database geen activiteit heeft of Query Store tijdens een bepaalde periode niet actief is, wordt de grafieken leeg zijn deze periode moeten worden weergegeven. U kunt Query Store op elk gewenst moment inschakelen als deze niet wordt uitgevoerd.   



## <a name="review-top-cpu-consuming-queries"></a>Bovenste Processorsnelheid door andere query's bekijken

Ga als volgt te werk in de [portal](http://portal.azure.com) :

1. Blader naar een SQL-database en klik op **alle instellingen** > **ondersteuning + probleemoplossing** > **Query prestaties inzicht**. 

    ![Query prestaties inzicht][1]

    De weergave meest voorkomende query's wordt geopend en de bovenste CPU in beslag nemen query's worden weergegeven.

1. Klik op rond de grafiek voor meer informatie.<br>De bovenste regel ziet algehele DTU % voor de database, terwijl de balken CPU weergeven % tijdens het geselecteerde interval worden gebruikt door de geselecteerde query's (bijvoorbeeld als **laatste week** is geselecteerd elke staaf vertegenwoordigt één dag).

    ![meest voorkomende query 's][2]

    Het raster onder vertegenwoordigt geaggregeerde gegevens voor de zichtbare query's.

  - Query-ID: de unieke id van de query in de database.
  - CPU per query tijdens waarneembare interval (afhangt van aggregatiefunctie).
  - Duur per query (afhangt van aggregatiefunctie).
  - Totaal aantal executions voor een bepaalde query.

    Schakel individuele query's wilt opnemen of uitsluiten ze uit de grafiek met selectievakjes in of uit.

1. Als uw gegevens verlopen is, klikt u op de knop **vernieuwen** .
1. U kunt gebruik schuifregelaars en knoppen om te wijzigen van waarnemingen interval en pieken onderzoeken in-en uitzoomen:  ![instellingen](./media/sql-database-query-performance/zoom.png)
1. (Optioneel) als u een andere weergave wilt, kunt u Selecteer tabblad **aangepast** en stel:
  
  - Metrisch (CPU, duur, aantal keer uitgevoerd)
  - Tijdsinterval (afgelopen 24 uur, vorige week, afgelopen maand). 
  - Het aantal query's.
  - Aggregatiefunctie.

    ![Instellingen](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Bekijken welke Querydetails zijn afzonderlijke

Bekijken welke Querydetails zijn:

1. Klik op een query in de lijst met meest voorkomende query's.

    ![meer informatie](./media/sql-database-query-performance/details.png)

1. De detailweergave wordt geopend en het aantal verbruik/duur/uitvoeren van query's processor werkt niet goed omlaag na verloop van tijd.
1. Klik op rond de grafiek voor meer informatie.
  - Grafiek met bovenste regel met algehele database DTU % wordt weergegeven en de balken zijn CPU % dat door de geselecteerde query.
  - Tweede diagram ziet u totale duur door de geselecteerde query.
  - Klik onder grafiek bevat de totale aantal executions door de geselecteerde query.
    
    ![welke Querydetails zijn][3]

1. (Optioneel) gebruik schuifregelaars, zoomknoppen of klik op **Instellingen** om aan te passen hoe querygegevens wordt weergegeven, of moet kiezen van een andere tijdsperiode.

## <a name="review-top-queries-per-duration"></a>Bekijk de meest voorkomende query's per duur

In de recente update van de Query prestaties inzicht, we twee nieuwe doelstellingen waarmee u mogelijke sporen kunt geïntroduceerd: duur en de uitvoering tellen.<br>

Langdurige query's hebben de grootste mogelijkheden voor resources langer vergrendelen, andere gebruikers blokkeren en schaalbaarheid beperken. Deze zijn ook de aanbevolen kandidaten voor optimalisatie.<br>

Om aan te geven langdurige query's:

1. Tabblad **aangepast** in Query prestaties inzicht voor geselecteerde database opent
1. Aan de doelstellingen moeten **duur** wijzigen
1. Selecteer aantal query's en waarnemingen interval
1. Selecteer aggregatiefunctie
  - **Som** is de som van alle verwerkingstijd van de query tijdens de hele waarnemingen interval.
  - **Max** vindt u query's met hele waarnemingen tijdsinterval is welke tijd execution maximum.
  - **Avg** gemiddelde tijd voor uitvoeren van alle query executions vindt en leert u de bovenkant afmelden bij deze gemiddelden. 

    ![de duur van de query][4]

## <a name="review-top-queries-per-execution-count"></a>Bekijk de meest voorkomende query's per aantal keer uitgevoerd

Hoog aantal executions mogelijk niet beïnvloeden database zelf en resources gebruik laag kan zijn, maar algehele toepassing kan worden traag.

In sommige gevallen zeer hoog execution telling kan leiden tot het vergroten van netwerk interactie. Interactie aanzienlijk trager. Ze zijn onderhevig aan netwerklatentie en naar de volgende server latentie. 

Zo veel gegevensgestuurde websites intensief toegang heeft tot de database voor elke gebruikersaanvraag. Terwijl de verbinding kan helpen, de verbeterde netwerkverkeer groepsgewijze en belasting op de databaseserver verwerken kan de prestaties nadelig beïnvloeden.  Algemeen advies is interactie tot een absolute minimum te beperken.

Als u wilt identificeren vaak uitgevoerd query's ("chatty") query's:

1. Tabblad **aangepast** in Query prestaties inzicht voor geselecteerde database opent
1. Aan de doelstellingen moeten **worden uitgevoerd telling** wijzigen
1. Selecteer aantal query's en waarnemingen interval

    ![aantal-query's worden uitgevoerd][5]

## <a name="understanding-performance-tuning-annotations"></a>Informatie over prestaties afstemmen aantekeningen 

Tijdens het verkennen van uw werkzaamheden in Query prestaties inzicht, ervaart u mogelijk pictogrammen met verticale lijn boven aan de grafiek.<br>

Deze pictogrammen zijn aantekeningen; prestaties dat dit gevolgen heeft acties uitgevoerd door de [SQL Azure-Database Advisor](sql-database-advisor.md)staan. Door zwevende aantekening krijgt u basisinformatie over de actie:

![query aantekeningen][6]

Als u wilt meer informatie of advisor aanbeveling toepassen, klikt u op het pictogram. Details van actie wordt geopend. Als dit een actieve aanbeveling is kunt u deze meteen via de opdracht kunt toepassen.

![welke Querydetails zijn aantekeningen][7]

### <a name="multiple-annotations"></a>Meerdere aantekeningen. ###

Het is mogelijk dat vanwege zoomniveau, aantekeningen die dicht bij elkaar zijn worden krijgen samengevoegd tot een. Hiermee worden weergegeven door een pictogram voor speciale, erop te klikken wordt geopend nieuwe blade waar de lijst met aantekeningen gegroepeerd worden weergegeven.
Query's en prestaties optimaliseren acties correleren kunt uw werkzaamheden beter te begrijpen. 


##  <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a>De configuratie van de Query Store voor Query prestaties inzicht optimaliseren

U kunt de volgende Query Store-berichten kunt tegenkomen tijdens het gebruik van de Query prestaties inzicht:

- "Query Store niet correct is geconfigureerd op deze database. Klik hier voor meer informatie."
- "Query Store niet correct is geconfigureerd op deze database. Klik hier om instellingen te wijzigen.' 

Deze berichten worden meestal weergegeven als Query opslaan is niet mogen nieuwe gegevens verzamelen. 

Eerste hoofdletters/kleine letters gebeurt wanneer Query Store alleen-lezen status is en parameters optimaal worden ingesteld. U kunt dit probleem oplossen door het vergroten van Query Store of Store Query uit te schakelen.

![knop qds][8]

Tweede hoofdletters/kleine letters gebeurt wanneer Query Store uitgeschakeld is of parameters optimaal niet worden ingesteld. <br>U kunt wijzigen van het beleid voor bewaren en vastleggen en inschakelen van Query Store door het uitvoeren van opdrachten onderstaande of rechtstreeks vanuit de portal:

![knop qds][9]

### <a name="recommended-retention-and-capture-policy"></a>Aanbevolen beleid voor bewaren en vastleggen

Er zijn twee soorten bewaarbeleid:

- Grootte gebaseerd – als ingesteld op automatisch deze wordt schonen gegevens automatisch wanneer dicht bij de maximale grootte is bereikt.
- Tijd op basis - al dan niet standaard die we dit wordt ingesteld op 30 dagen, wat betekent dat, als Query Store geen ruimte meer uitvoert, worden deze querygegevens ouder zijn dan 30 dagen verwijderd

Vastleggen beleid kan worden ingesteld op:

- **Alle** – wordt vastgelegd alle query's.
- **Automatisch** – onregelmatig query's met onbelangrijke compilatie en uitvoering duur worden genegeerd. Drempelwaarden voor de uitvoering tellen, compileren en runtime duur worden intern bepaald. Dit is de standaardoptie.
- **Geen** – Query Store stopt vastleggen van nieuwe query's, maar runtime stat voor al vastgelegde query's nog steeds verzameld worden.
    
Het is raadzaam om alle beleidsregels automatisch en schoon beleid tot 30 dagen instellen:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);
        
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));
    
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

De tekengrootte van de Query Store. Dit kan worden uitgevoerd door de verbinding maakt met een database en uitgifte volgende query:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Deze instellingen toepassen, uiteindelijk kunt u Query Store verzamelt nieuwe query's, maar als u niet wilt wachten kunt u Query Store wissen. 
> [AZURE.NOTE] Uitvoeren van de volgende query wordt alle actuele gegevens in de winkel Query verwijderen. 


    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Overzicht

Query prestaties inzicht vindt u meer informatie over de impact van uw werkzaamheden query en hoe deze zich verhoudt tot verbruik van database. Met deze functie wordt u meer informatie over de bovenkant door andere query's en herkennen van de bestanden op te lossen voordat ze een probleem geworden.




## <a name="next-steps"></a>Volgende stappen

Klik op [aanbevelingen](sql-database-advisor.md) over het blad **Query prestaties inzicht** voor extra aanbevelingen over het verbeteren van de SQL-database.

![Prestaties Advisor](./media/sql-database-query-performance/ia.png)


<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

