<properties
    pageTitle="Technische handleiding aan de sjabloon Cortana Intelligence oplossing voor de blog onderhoud in ruimtevaart en andere bedrijven | Microsoft Azure"
    description="Een technische handleiding aan de sjabloon oplossing met Microsoft Cortana Intelligence voor blog onderhoud in ruimtevaart, hulpprogramma's en transport."
    services="cortana-analytics"
    documentationCenter=""
    authors="fboylu"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="fboylu" />

# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Technische handleiding aan de sjabloon Cortana Intelligence oplossing voor de blog onderhoud in ruimtevaart en andere bedrijven

## <a name="acknowledgements"></a>**Bevestigingen**
In dit artikel is geschreven door gegevens wetenschappers Yan Zhang, Gauher Shaheen, Fidan Boylu Uz en software engineeren Dan Grecoe bij Microsoft.

## <a name="overview"></a>**Overzicht**

Oplossing sjablonen zijn ontworpen om het proces van het samenstellen van een demo E2E boven op Cortana Intelligence Suite versnellen. Een uitgevouwen sjabloon wordt inrichten van uw abonnement met de benodigde Cortana Intelligence-onderdelen en hun onderlinge relaties maken. Deze van de pijplijn gegevens ook zaden met voorbeeldgegevens die zijn gegenereerd op basis van een toepassing voor het genereren van gegevens die u kunt downloaden en installeren op uw lokale computer nadat u de sjabloon oplossing implementeert. De gegevens die zijn gegenereerd door het aggregaat wordt hydrate de gegevens verkooppijplijn en start u genereert machine learning voorspellingen waarin kunnen u vervolgens op het Power BI-dashboard visualiseren. Het implementatieproces begeleidt u bij de verschillende stappen voor het instellen van uw referenties oplossing. Zorg ervoor dat u deze referenties zoals de oplossingsnaam van de, gebruikersnaam en wachtwoord dat u tijdens de implementatie opgeeft opnemen.  

Het doel van dit document is uitleg over de verwijzing architectuur en de verschillende onderdelen ingericht in uw abonnement als onderdeel van deze sjabloon voor een oplossing. Het document is ook moment spreekt over hoe u de voorbeeldgegevens vervangen door werkelijke gegevens van uw eigen moeten kunnen zien inzichten en voorspellingen van uw eigen gegevens. Bovendien het document wordt beschreven hoe de onderdelen van de oplossing-sjabloon die worden gewijzigd moet als u wilt aanpassen van de oplossing met uw eigen gegevens. Instructies voor het maken van het Power BI-dashboard voor deze sjabloon voor een oplossing worden gegeven aan het einde.

>[AZURE.TIP] U kunt downloaden en afdrukken van een [PDF-versie van dit document](http://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).

## <a name="big-picture"></a>**Totaalbeeld**

![](media/cortana-analytics-technical-guide-predictive-maintenance\predictive-maintenance-architecture.png)

Wanneer de oplossing wordt geïmplementeerd, verschillende Azure services binnen Cortana Analytics-Suite geactiveerd (*dat wil zeggen* gebeurtenis Hub Stream analyses, HDInsight, Data Factory, Machine Learning, *enzovoort*). De van bovenstaande Architectuurdiagram wordt weergegeven, op hoog niveau, hoe de blog onderhoud voor B.v. oplossing sjabloon is gebaseerd op end-to-end. U bent kunt deze services in de portal van azure onderzoeken door erop te klikken op de oplossing sjabloon diagram gemaakt met de implementatie van de oplossing met uitzondering van HDInsight als deze service is ingericht op aanvraag wanneer de activiteiten gerelateerde verkooppijplijn vereist voor uitvoeren en verwijderde achteraf zijn.
U kunt een [volledige versie van het diagram](http://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png)downloaden.

De volgende secties worden elk tekstgedeelte.

## <a name="data-source-and-ingestion"></a>**Gegevensbron en opname**

### <a name="synthetic-data-source"></a>Synthetische gegevensbron

Voor deze sjabloon voor de gegevensbron die wordt gebruikt vanuit een bureaublad-toepassing die u downloaden en uitvoeren lokaal na een geslaagde implementatie gegenereerd. U vindt u de instructies om te downloaden en installeren van deze toepassing op de Eigenschappenbalk wanneer u het eerste knooppunt blog onderhoud gegevens genereren genoemd in het diagram van de sjabloon oplossing selecteert. Deze toepassing-feeds van de [Azure gebeurtenis Hub](#azure-event-hub) -service met gegevenspunten of gebeurtenissen, die wordt gebruikt in de rest van de stroom van de oplossing. Deze gegevensbron is omvat of afgeleid van openbaar gegevens uit de [bibliotheek van NASA gegevens](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) met behulp van de [gegevensverzameling voor turbofanmotoren Engine verslechtering van simulatie](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan).

Een toepassing voor het genereren van de gebeurtenis vult de Hub van de gebeurtenis Azure alleen terwijl deze wordt uitgevoerd op uw computer.

### <a name="azure-event-hub"></a>Azure gebeurtenis hub

De [Gebeurtenis-Hub Azure](https://azure.microsoft.com/services/event-hubs/) -service is de geadresseerde van de invoer verstrekt door de synthetische gegevensbron hierboven is beschreven.

## <a name="data-preparation-and-analysis"></a>**Gegevensvoorbereiding van en analyse**


### <a name="azure-stream-analytics"></a>Azure Stream Analytics

De [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -service wordt gebruikt om te leveren in de buurt van realtime analytics voor de invoer stroom van de [Azure gebeurtenis Hub](#azure-event-hub) -service en publiceren van resultaten op een [Power BI](https://powerbi.microsoft.com) -dashboard, evenals alle onbewerkte binnenkomende gebeurtenissen bij de [Opslag van Azure](https://azure.microsoft.com/services/storage/) -service voor latere verwerking door de service [Factory van Azure-gegevens](https://azure.microsoft.com/documentation/services/data-factory/) archiveren.

### <a name="hd-insights-custom-aggregation"></a>Aangepaste aggregatie HD inzichten

De HD-inzicht Azure-service wordt gebruikt om uit te voeren [component](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) scripts (ingedeeld door Azure gegevens Factory) aggregaties beschikken over de onbewerkte gebeurtenissen die zijn gearchiveerd met de Azure Stream Analytics-service.

### <a name="azure-machine-learning"></a>Azure Machine Learning

De [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) -service wordt gebruikt (ingedeeld door Azure gegevens Factory) om voorspellingen op de resterende levensduur (RUL) van een bepaalde vliegtuig engine gegeven van de invoer ontvangen.

## <a name="data-publishing"></a>**Publicatie van gegevens**


### <a name="azure-sql-database-service"></a>Azure SQL Database-Service

De [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) -service wordt gebruikt om op te slaan (beheerd door Azure gegevens Factory) de voorspellingen ontvangen door de Azure Machine Learning-service die wordt verbruikt in het [Power BI](https://powerbi.microsoft.com) -dashboard.

## <a name="data-consumption"></a>**Gebruik van gegevens**

### <a name="power-bi"></a>Power BI

De [Power BI](https://powerbi.microsoft.com) -service wordt gebruikt om weer te geven van een dashboard met aggregaties en waarschuwingen gekregen van de [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -service, evenals RUL-voorspellingen die zijn opgeslagen in [Azure SQL-Database](https://azure.microsoft.com/services/sql-database/) die zijn gemaakt met de [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) -service. Voor instructies over het maken van het Power BI-dashboard voor deze oplossing-sjabloon, raadpleegt u de sectie hieronder.

## <a name="how-to-bring-in-your-own-data"></a>**Hoe u uw eigen gegevens importeren in**

In deze sectie wordt beschreven hoe om uw eigen gegevens gebruikt om te Azure weer te geven en welke gebieden zou moeten worden gewijzigd voor de gegevens die u naar deze architectuur overbrengen.

Het is waarschijnlijk dat een gegevensset u zou overeenkomen met de gegevensset gebruikt door de [turbofanmotoren Engine verslechtering van simulatie-gegevensset](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) gebruikt voor deze sjabloon voor een oplossing. Informatie over uw gegevens en de vereisten zijn essentieel in hoe u deze sjabloon om te werken met uw eigen gegevens wijzigen. Als dit uw eerste blootgesteld aan de Azure Machine Learning-service is, kunt u een inleiding over het verkrijgen met behulp van het voorbeeld in [het maken van uw eerste experiment](machine-learning-create-experiment.md).

De volgende secties wordt uitgelegd hoe de secties van de sjabloon waarvoor wijzigingen wordt uitgevoerd wanneer een nieuwe gegevensset is geïntroduceerd.

### <a name="azure-event-hub"></a>Azure gebeurtenis Hub

De gebeurtenis-Hub Azure-service is erg algemeen, zodat de gegevens in de hub in de CSV- of JSON kunt wordt geplaatst. Er is geen speciale verwerking wordt weergegeven in de Hub van de gebeurtenis Azure, maar het is belangrijk dat u weet dat de gegevens die wordt ingevoerd.

In dit document niet wordt beschreven hoe u het nemen van uw gegevens, maar u kunt eenvoudig verzenden gebeurtenissen of gegevens naar een Hub Azure-gebeurtenis de gebeurtenis Hub-API gebruiken.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics

De Azure Stream Analytics-service wordt gebruikt om aan te bieden bij realtime analytics door het lezen van gegevensstromen en uitvoeren van gegevens naar een willekeurig aantal bronnen.

Voor het blog onderhoud voor B.v. oplossing sjabloon, wordt de query Azure Stream Analytics bestaat uit vier subquery's, elke in beslag nemen gebeurtenissen van de Azure gebeurtenis Hub-service en hoeven uitvoer van vier verschillende locaties. Deze uitvoer bestaat uit drie Power BI-gegevenssets en een opslaglocatie van Azure.

De query Azure Stream Analytics kunt u vinden door:

-   Aanmelden bij de portal van Azure

-   Zoeken naar de stream analytics-taken ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-stream-analytics.png) die zijn gegenereerd tijdens de oplossing is geïmplementeerd (*bijvoorbeeld*, **maintenancesa02asapbi** en **maintenancesa02asablob** voor de blog onderhoud-oplossing)

-   Selecteren

    -   ***Ingangen*** om weer te geven van de invoer van de query

    -   ***QUERY*** om weer te geven van de query zelf

    -   ***Hiermee kunt u*** de uitvoer van de verschillende weergeven

Informatie over het samenstellen van Azure Stream Analytics-query vindt u in de [Stream Analytics Query verwijzing](https://msdn.microsoft.com/library/azure/dn834998.aspx) op MSDN.

De query's uitvoeren in deze oplossing, drie gegevenssets met bijna realtime analytics-informatie over de binnenkomende gegevensstroom op een Power BI-dashboard dat opgegeven als onderdeel van deze sjabloon voor een oplossing. Omdat er impliciete kennis van de binnenkomende gegevensindeling, moeten deze query's worden gewijzigd op basis van de gegevensopmaak van de.

De query in de tweede stream analytics taak- **maintenancesa02asablob** gewoon Hiermee kunt u alle [Gebeurtenis Hub](https://azure.microsoft.com/services/event-hubs/) -gebeurtenissen met [Azure Storage](https://azure.microsoft.com/services/storage/) en dus geen wijziging ongeacht de gegevensindeling van uw vereist, zoals de volledige gebeurtenisinformatie streaming naar opslag.

### <a name="azure-data-factory"></a>Azure gegevens Factory

De service [Azure gegevens Factory](https://azure.microsoft.com/documentation/services/data-factory/) orchestrates de verplaatsing en de verwerking van gegevens. In de blog onderhoud voor B.v. oplossing sjabloon de fabriek gegevens bestaat uit drie [pijpleidingen](../data-factory/data-factory-create-pipelines.md) die verplaatsen en verwerken van de gegevens met verschillende technologieën.  U kunt uw gegevens factory openen via het het knooppunt Data Factory onderaan in het diagram van de sjabloon oplossing die zijn gemaakt met de implementatie van de oplossing. Hiermee gaat u naar de fabriek gegevens op uw Azure-portal. Als u fouten onder uw gegevenssets ziet, kunt u die negeren als gevolg van gegevens factory wordt geïmplementeerd voordat het genereren van gegevens is gestart. Die fouten Voorkom dat uw gegevens factory niet werkt.

![](media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

In deze sectie wordt beschreven hoe de benodigde [pijpleidingen](../data-factory/data-factory-create-pipelines.md) en [activiteiten](../data-factory/data-factory-create-pipelines.md) in de [Factory van Azure-gegevens](https://azure.microsoft.com/documentation/services/data-factory/). Hieronder vindt u de diagramweergave van de oplossing.

![](media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Twee van de gasbuizen met deze factory bevatten [component](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) scripts die worden gebruikt voor het partitioneren en de gegevens. Wanneer wordt vermeld, wordt de scripts zich bevinden in de [Opslag van Azure](https://azure.microsoft.com/services/storage/) -account gemaakt tijdens de installatie. De locatie worden: maintenancesascript\\\\script\\\\component\\ \\ (of https://[Your oplossing name].blob.core.windows.net/maintenancesascript).

Vergelijkbaar met de [Azure Stream Analytics](#azure-stream-analytics-1) -query's, de scripts [component](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) impliciete kennis van de binnenkomende gegevensindeling, deze query's zou moeten worden gewijzigd op basis van uw gegevens opmaken en de [functie technische](machine-learning-feature-selection-and-engineering.md) vereisten.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*

Deze [verkooppijplijn](../data-factory/data-factory-create-pipelines.md) bevat één activiteit - een [HDInsightHive](../data-factory/data-factory-hive-activity.md) activiteit met een [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) die wordt uitgevoerd als een script [component](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) partitioneren van de gegevens die tijdens de [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -taak in [Azure opslagruimte](https://azure.microsoft.com/services/storage/) hebt opgeslagen.

Het script [component](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) voor deze partities taak is ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*

Deze [verkooppijplijn](../data-factory/data-factory-create-pipelines.md) bevat verschillende activiteiten en waarvan eindresultaat is dat de scored voorspellingen van de [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimenteren die is gekoppeld aan deze sjabloon voor een oplossing.

De activiteiten die zijn opgenomen in deze zijn:

-   [HDInsightHive](../data-factory/data-factory-hive-activity.md) activiteit met een [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) die wordt uitgevoerd als een script [component](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) als aggregaties en functie engineering nodig zijn voor het [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) -experiment wilt uitvoeren.
    Het script [component](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) voor deze partities taak is ***PrepareMLInput.hql***.

-   [Kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit die verplaatst de resultaten van de activiteit [HDInsightHive](../data-factory/data-factory-hive-activity.md) naar een één [Opslag van Azure](https://azure.microsoft.com/services/storage/) blob die toegang tot de activiteit [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) kan zijn.

-   [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) activiteit dat oproepen van de [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimenteren wat resulteert in de resultaten worden opgeslagen in een enkel [Opslag van Azure](https://azure.microsoft.com/services/storage/) blob.

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*

Deze [verkooppijplijn](../data-factory/data-factory-create-pipelines.md) bevat één activiteit - een [kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) activiteit dat de resultaten van de [Azure Machine Learning](#azure-machine-learning) experimenteren verplaatst van de ***MLScoringPipeline*** naar de [Azure SQL-Database](https://azure.microsoft.com/services/sql-database/) die als onderdeel van de installatie van de sjabloon oplossing is ingericht.

### <a name="azure-machine-learning"></a>Azure Machine Learning

Het [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) -experiment gebruikt voor deze sjabloon voor een oplossing bevat de resterende handig leven (RUL) van een vliegtuig-engine. Het experiment specifiek is voor de gegevensgroep verbruikt en dus moet u wijziging of vervangende die specifiek zijn voor de gegevens die opgenomen in.

Zie voor meer informatie over hoe het Azure Machine Learning-experiment is gemaakt, [blog onderhoud: stap 1 van 3, gegevens voorbereiden en functie engineering](http://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>**Voortgang bewaken**
 Wanneer het apparaat gegevens wordt gestart, de pijplijn begint te krijgen gehydrateerd en de verschillende onderdelen van uw oplossing beginnen bij de start in de volgende manieren te werk actie de opdrachten door de fabriek gegevens uitgegeven. Er zijn twee manieren kunt u de verkooppijplijn controleren.

1. Een van de Stream Analytics-taak schrijft de onbewerkte binnenkomende gegevens naar blobopslag. Als u op Blob Storage onderdeel van de oplossing in het scherm u kunt de oplossing geïmplementeerd en klik vervolgens op openen in het rechterdeelvenster, deze gaat u naar de [beheerportal](https://portal.azure.com/). Wanneer er, klikt u op op BLOB's. In het volgende deelvenster ziet u een lijst met Containers. Klik op **maintenancesadata**. In het volgende deelvenster ziet u de map **Nieuwrap:** . In de map Nieuwrap:, ziet u mappen met namen zoals uur = 17, uur = 18 enzovoort. Als u deze mappen ziet, betekent dit dat de onbewerkte gegevens succes is wordt gegenereerd op uw computer en die zijn opgeslagen in blobopslag. Hier ziet u CSV-bestanden die eindige grootte in MB in deze mappen nodig hebt.

2. De laatste stap van de pijplijn is bij het wegschrijven van gegevens (bijvoorbeeld voorspellingen van machine learning) in SQL-Database. U moet mogelijk wachten maximaal drie uur duren voordat de gegevens worden weergegeven in de SQL-Database. Er is een manier om te controleren hoe veel gegevens zijn beschikbaar in uw SQL-Database via [azure portal](https://manage.windowsazure.com/). Zoek op het linker deelvenster SQL-DATABASES ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-SQL-databases.png) en klikt u erop. Zoek uw database **pmaintenancedb** vervolgens en klik erop. Klik op de volgende pagina onderaan op aan beheren

    ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-manage.png).

    Hier kunt u op nieuwe Query en de query voor het aantal rijen (bijvoorbeeld select count(*) uit PMResult). Als uw database in omvang groeit, wordt het aantal rijen in de tabel moet verhogen.


## <a name="power-bi-dashboard"></a>**Power BI-Dashboard**

### <a name="overview"></a>Overzicht

In deze sectie wordt beschreven hoe u Power BI-dashboard kunt configureren op uw realtime gegevens van Azure stream analyseprogramma (warm pad), visualiseren en de resultaten van de batch tekstvoorspelling van Azure machine learning-(koudwatersystemen pad).

### <a name="setup-cold-path-dashboard"></a>Setup koudwatersystemen pad dashboard

In de pijplijn koudwatersystemen pad gegevens is het essentiële doel om de blog RUL van elke vliegtuig-engine (resterende levensduur) zodra deze is voltooid met een vlucht (cyclus). Het resultaat tekstvoorspelling wordt om 3 uur voor het voorspellen van de vliegtuig-engines die klaar bent met een vlucht tijdens de afgelopen 3 uur bijgewerkt.

Power BI maakt verbinding met een Azure SQL-database als gegevensbron, waar de voorspelling resultaten worden opgeslagen. Opmerking: 1) bij uw-oplossing implementeert, een reële voorspelling wordt weergegeven in de database binnen 3 uur.
Het pbix-bestand bij het downloaden van genereren bevat enkele gegevens zodat u mogelijk de Power BI-dashboard direct maken. 2) in deze stap is het vereiste downloaden en installeren van de gratis software [Power BI-bureaublad](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/).

De volgende stappen helpen u op hoe u het bestand pbix verbinden met de SQL-Database die is of omhoog op het moment van de implementatie van oplossingen met gegevens (*bijvoorbeeld*. resultaten van de voorspelling) voor de visualisatie.

1.  De databasereferenties krijgen.

    U moet **de naam van de database-server, de databasenaam van de, gebruikersnaam en wachtwoord** voordat u verdergaat met volgende stappen. Hier vindt u de stappen om u te helpen u hoe u ze vinden.

    -   Wanneer **' Azure SQL Database'** op uw oplossing sjabloon diagram groen wordt, klikt u erop en klik vervolgens op **'Openen'**.

    -   Hier ziet u een nieuw tabblad/browservenster waarin de Azure portalpagina. Klik op **'Resourcegroepen'** op het linker deelvenster.

    -   Selecteer het abonnement dat u gebruikt voor het implementeren van de oplossing en selecteer **' YourSolutionName\_ResourceGroup'**.

    -   In de nieuwe pop-out Configuratiescherm, klikt u op de ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-sql.png) pictogram toegang heeft tot uw database. De databasenaam van de zich naast het pictogram (*bijvoorbeeld*, **'pmaintenancedb'**) en de **naam van de database-server** wordt weergegeven onder de Server-naameigenschap en moet er ongeveer **YourSoutionName.database.windows.net**.

    -   Uw **gebruikersnaam** en **wachtwoord** zijn hetzelfde als de gebruikersnaam en wachtwoord eerder opgenomen tijdens de implementatie van de oplossing.

2.  De gegevensbron voor het rapport koudwatersystemen pad bijwerken met Power BI Desktop.

    -   Dubbelklik in de map op uw PC waar u hebt gedownload en bestanden van het bestand genereren, op de **PowerBI\\PredictiveMaintenanceAerospace.pbix** bestand. Als u eventuele waarschuwingsberichten ziet wanneer u het bestand opent, negeert u deze. Boven aan het bestand, klikt u op **Query's bewerken**.

        ![](media\cortana-analytics-technical-guide-predictive-maintenance\edit-queries.png)

    -   Hier ziet u twee tabellen, **RemainingUsefulLife** en **PMResult**. Selecteer de eerste tabel en klikt u op ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-query-settings.png) naast **'Bron'** onder **' TOEGEPASTE stappen '** in het rechterdeelvenster van **Query-instellingen** . Negeer eventuele waarschuwingsberichten die worden weergegeven.

    -   In het pop-out venster, **'Server'** en **'Database'** vervangen door uw eigen server en de databasenamen en klik vervolgens op **"OK"**. Zorg dat u de poort 1433 (**YourSoutionName.database.windows.net, 1433**) opgeven voor servernaam. Laat het databaseveld als **pmaintenancedb**. Negeer de waarschuwingsberichten die worden weergegeven op het scherm.

    -   In de volgende pop-out venster ziet u twee opties in het linkerdeelvenster (**Windows** en **Database**). Klik op **'Database'**, vul uw **'Gebruikersnaam'** en **'Wachtwoord'** (dit is de gebruikersnaam en wachtwoord die u hebt ingevoerd wanneer u eerst de oplossing geïmplementeerd en een Azure SQL-database gemaakt). Schakel in het ***selecteren van welk niveau deze instellingen toepassen***, niveau databaseoptie. Klik op **'Verbinding maken'**.

    -   Klikt u op de tweede tabel **PMResult** ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-navigation.png) naast **'Bron'** onder **' TOEGEPASTE stappen '** in het rechterdeelvenster van **Query-instellingen** en de namen van de server en database zoals in de bovenstaande stappen bijwerken en klik op OK.

    -   Nadat u bent begeleid terug naar de vorige pagina, sluit u het venster. Een bericht wordt pop-out - Klik op **toepassen**. Ten slotte, klikt u op de knop **Opslaan** als de wijzigingen wilt opslaan. Uw Power BI-bestand is nu tot stand gebracht verbinding met de server. Als uw visualisaties leeg zijn, zorg er dan voor dat u de selecties op de visualisaties kunt alle gegevens visualiseren door te klikken op het pictogram gum in de rechterbovenhoek van de legenda's wissen. Gebruik de knop Vernieuwen op de nieuwe gegevens na te denken over visualisaties. In eerste instantie ziet u alleen de gegevens op uw visualisaties terwijl de fabriek gegevens te vernieuwen om 3 uur is gepland. Na 3 uur, worden er nieuwe voorspellingen doorgevoerd in uw visualisaties Wanneer u de gegevens vernieuwt.

3.  (Optioneel) Het dashboard koudwatersystemen pad publiceren naar [Power BI online](http://www.powerbi.com/). Houd er rekening mee dat deze stap projectoverzicht aan een Power BI-account (of Office 365-account).

    -   Klik op **'Publiceren'** en enkele seconden later een venster wordt weergegeven met "Publiceren naar Power BI succes!" met een groen vinkje. Klik op de koppeling onder 'Openen PredictiveMaintenanceAerospace.pbix in Power BI'. Zie [publiceren van Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop)gedetailleerde instructies.

    -   Maak een nieuwe dashboard: klik op de **+** -teken naast de sectie **Dashboards** in het linkerdeelvenster. Voer de naam "Blog onderhoud Demo" voor deze nieuwe dashboard.

    -   Wanneer u het rapport hebt geopend, klikt u op ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-pin.png) om een opslaglocatie van alle visualisaties aan uw dashboard te. Gedetailleerde instructies vinden? Zie [pincode een tegel aan een Power BI-dashboard uit een rapport](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
    Ga naar de dashboardpagina en de grootte en de locatie van uw visualisaties aanpassen en de titel bewerken. Ga voor gedetailleerde instructies voor het bewerken van de tegels, raadpleegt u [bewerken een tegel--formaat wijzigen, verplaatsen, naam wijzigen, pincode, verwijderen, hyperlink toevoegen](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Hier volgt een voorbeelddashboard met enkele koudwatersystemen pad visualisaties vastgemaakt aan deze.  Afhankelijk van hoe lang u uw gegevens genereren uitvoert, enigszins de nummers op de visualisaties afwijken.
    <br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\final-view.png)
<br/>
    -   Naar het vernieuwen van de planning van de gegevens, houd u de muisaanwijzer boven de gegevensset **PredictiveMaintenanceAerospace** , klikt u op ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-elipsis.png) en kies vervolgens **Vernieuwen plannen**.
<br/>
        **Notitie:** Als er een waarschuwing massage, klik op **Referenties bewerken** en zorg ervoor dat zijn uw databasereferenties overeenkomen met de beschrijving bij stap 1.
<br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\schedule-refresh.png)
<br/>
    -   Vouw de sectie **Vernieuwen plannen** . Inschakelen "dat uw gegevens bijgewerkt blijft".
    <br/>
    -   Plannen het vernieuwen op basis van uw behoeften voldoet. Ga voor meer informatie raadpleegt u de [gegevens vernieuwen in Power BI](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>Setup warm pad dashboard

De volgende stappen begeleidt u hoe u realtime gegevensuitvoer van Stream Analytics-taken die zijn gegenereerd op het moment van oplossing implementatie visualiseren. Een [Power BI online](http://www.powerbi.com/) -account is vereist voor de volgende stappen uitvoeren. Als u geen account hebt, kunt u [een account maakt](https://powerbi.microsoft.com/pricing).

1.  Power BI-uitvoer in Azure Stream Analytics (ASA) toevoegen.

    -  Moet u de instructies in [Azure Stream Analytics en Power BI: een dashboard realtime analytics voor realtime zichtbaarheid van gegevensstromen](stream-analytics-power-bi-dashboard.md) voor het instellen van de uitvoer van uw taak Azure Stream Analytics als uw Power BI-dashboard.
    - De query ASA heeft drie uitvoer die **aircraftmonitor**, **aircraftalert**en **flightsbyhour zijn**. U kunt de query weergeven door te klikken op het querytabblad. Overeenkomt met elk van deze tabellen, moet u een uitvoer toevoegen aan ASA. Controleer of dat de **Uitvoeralias**, de **Naam van de gegevensset** en de **Naam van de tabel** worden de dezelfde (**aircraftmonitor**) bij u de eerste uitvoer (*bijvoorbeeld* **aircraftmonitor**) toevoegen. Herhaal de stappen om de uitvoer voor **aircraftalert**en **flightsbyhour**toevoegen. Zodra u hebt toegevoegd alle drie tabellen weergeven en de taak ASA gestart, ontvangt u een bevestigingsbericht weergegeven (*bijvoorbeeld*, 'Starten stream analytics taak maintenancesa02asapbi is geslaagd').

2. Meld u aan bij [Power BI online](http://www.powerbi.com)

    -   Klik op het linker deelvenster gegevenssets sectie in mijn werkruimte de ***GEGEVENSSET*** namen **aircraftmonitor**, **aircraftalert**en **flightsbyhour** moeten worden weergegeven. Dit is de streaming gegevens die u van Azure Stream analyseprogramma in de vorige stap hebt gedrukt. De gegevensset **flightsbyhour** mogelijk niet weergegeven op hetzelfde moment als de andere twee gegevenssets vanwege de aard van de SQL-query erachter. Echter deze moet worden weergegeven na een uur.
    -   Controleer of het deelvenster ***Visualisaties*** is geopend en wordt weergegeven aan de rechterkant van het scherm.

3. Nadat u de gegevens die doorloopt in Power BI hebt, kunt u beginnen de streaming gegevens visualiseren. Hieronder wordt een voorbeelddashboard met enkele populaire pad visualisaties vastgemaakt aan deze. U kunt andere dashboardtegels op basis van de juiste gegevenssets maken. Afhankelijk van hoe lang u uw gegevens genereren uitvoert, enigszins de nummers op de visualisaties afwijken.


    ![](media\cortana-analytics-technical-guide-predictive-maintenance\dashboard-view.png)

4. Hier volgen enkele stappen voor het maken van een van de bovenstaande tegels – de 'vloot weergave van Sensor 11 versus drempel 48,26"tegel gemarkeerd:

    -   Klik op gegevensset **aircraftmonitor** op het linker deelvenster gegevenssets sectie.

    -   Klik op het pictogram **Lijndiagram** .

    -   Klik in het deelvenster **velden** **verwerkte** zodat deze wordt weergegeven onder 'As' in het deelvenster **Visualisaties** .

    -   Klik op "s11" en "s11\_melding" zodat ze beide worden weergegeven onder 'Waarden'. Klik op de kleine pijl naast **s11** en **s11\_melding**, "Som" "Gemiddelde" wijzigen.

    -   Klik op **Opslaan** in het begin en het rapport een naam 'aircraftmonitor'. Het rapport met de naam 'aircraftmonitor' wordt weergegeven in het gedeelte **rapporten** in het deelvenster **Navigator** aan de linkerkant.

    -   Klik op het pictogram van de **Pincode visuele** in de rechterbovenhoek van dit lijndiagram. Een venster 'Pincode naar Dashboard' u kunt kiezen van een dashboard mogelijk weergegeven. Selecteer "Blog onderhoud Demo" en klik op "Pincode".

    -   Plaats de muisaanwijzer op deze tegel op het dashboard, klikt u op het pictogram 'bewerken' in de rechterbovenhoek om de titel aan "Vloot weergave van Sensor 11 versus drempelwaarde 48,26" en ondertitel "Gemiddelde over vloot na verloop van tijd" te wijzigen.

## <a name="how-to-delete-your-solution"></a>**Het verwijderen van uw oplossing**
Zorg ervoor dat u het genereren van de gegevens stopt bij gebruik van de oplossing niet actief terwijl het apparaat gegevens uitgevoerd wordt kosten hoger. Verwijder de oplossing als u deze niet gebruikt. Uw oplossing verwijdert, worden alle onderdelen ingericht in uw abonnement wanneer u de oplossing geïmplementeerd. Als u wilt verwijderen klikt u op de oplossing op de oplossingsnaam van uw in het linkerdeelvenster van de oplossing sjabloon en klik op verwijderen.

## <a name="cost-estimation-tools"></a>**Schatting extra kosten**

De volgende twee hulpprogramma's zijn beschikbaar waarmee u kunt beter begrip van de totale kosten voor het uitvoeren van de blog onderhoud voor B.v. oplossing-sjabloon van uw abonnement:

-   [Microsoft Azure kosten schatting hulpmiddel (online)](https://azure.microsoft.com/pricing/calculator/)

-   [Microsoft Azure kosten schatting hulpmiddel (bureaublad)](http://www.microsoft.com/download/details.aspx?id=43376)
