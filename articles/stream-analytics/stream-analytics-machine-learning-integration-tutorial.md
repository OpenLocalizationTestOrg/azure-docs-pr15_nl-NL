<properties
    pageTitle="Sentiment analyse met behulp van Azure Stream analyses en Machine Learning van Azure | Microsoft Azure"
    description="Het gebruik van een gebruiker gedefinieerde functie en Machine Learning in een Stream Analytics-project"
    keywords=""
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>


<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/04/2016" 
    ms.author="jeffstok"
/>

# <a name="sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a>Sentiment analyse met behulp van Azure Stream analyses en Machine Learning van Azure #

In dit artikel is bedoeld om te helpen u snel een eenvoudige Azure Stream Analytics project, met Azure Machine Learning-integratie is ingesteld. We een sentiment analytics Machine Learning-model van de Cortana Intelligence-galerie gebruiken om streaming tekstgegevens te analyseren en sentiment score in realtime bepalen. De informatie in dit artikel kunt u scenario's zoals realtime sentiment-analyses op Twitter gegevensstromen begrijpen, vermeldingen van de klant chats met ondersteuningspersoneel analyseren en opmerkingen op de forums, blogs en video's, behalve de vele andere realtime, bekijk score scenario's evalueren.

In dit artikel biedt een CSV-voorbeeldbestand met tekst als invoer in Azure-blobopslag, weergegeven in de volgende afbeelding. De taak is van toepassing het sentiment analytics-model als de gebruiker gedefinieerde functie (UDF) op de voorbeeldgegevens voor tekst in de store blob. Het eindresultaat bevindt zich in dezelfde blob locatie in een ander CSV-bestand. 

![Stream Analytics Machine Learning](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

De volgende afbeelding ziet u deze configuratie. Voor een meer natuurlijke scenario, kunt u blobopslag vervangen door streaming Twitter-gegevens uit een Azure gebeurtenis Hubs invoer. Bovendien kunt u een realtime [Microsoft Power BI](https://powerbi.microsoft.com/) -visualisatie van de statistische sentiment maken.    

![Stream Analytics Machine Learning](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Vereisten voor

De vereisten voor het uitvoeren van de taken die zijn die in dit artikel zijn als volgt:

-   Een actieve Azure-abonnement.
-   Een CSV-bestand met enkele gegevens erin. U kunt het bestand wordt weergegeven in de afbeelding 1 van [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample Data/sampleinput.csv)downloaden of u kunt uw eigen bestand maken. Voor dit artikel, we wordt ervan uitgegaan dat u het account dat is opgegeven voor downloaden op GitHub.

Op hoog niveau, om te voltooien van de taken die in dit artikel, hebt u het volgende doen:

1.  De invoer CSV-bestand uploaden naar Azure-blobopslag.
2.  Een sentiment analytics-model vanuit de galerie met Cortana Intelligence toevoegen aan uw Azure Machine Learning-werkruimte.
3.  Dit model te implementeren als een webservice in de Machine Learning-werkruimte.
4.  Een taak Stream Analytics oproepen van deze webservice als een functie, om te bepalen sentiment voor de tekst input maken.
5.  Start de Stream Analytics-taak en de uitvoer toekijken.

## <a name="upload-the-csv-input-file-to-blob-storage"></a>CSV-bestand uploaden naar Blob storage

Voor deze stap kunt u een CSV-bestand, zoals een al is opgegeven als gedownload van GitHub. U kunt [Azure opslag Explorer](http://storageexplorer.com/) of Visual Studio gebruiken om het bestand te uploaden of kunt u aangepaste code. We gebruiken voorbeelden op basis van Visual Studio.

1.  Klik in Visual Studio, op **Azure** > **opslag** > **Externe opslag als bijlage toevoegen**. Voer een **accountnaam** en **Accountsleutel**.  

    ![Stream Analytics Machine Learning, Server Explorer](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-server-explorer.png)  

2.  Vouw de opslag die u in stap 1 hebt bijgevoegd op **Blob Container maken**en voer vervolgens een logische naam. Nadat u de container maken, openen om de inhoud ervan weer te geven. (Deze wordt niet leeg zijn op dit punt).  

    ![Streamen Analytics Machine Learning, blob maken](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-create-blob.png)  

3.  Naar het CSV-bestand uploaden, klikt u op **Blob uploaden**en klik vervolgens op **bestand vanaf de lokale schijf**.  

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a>Het sentiment analytics-model toevoegen vanuit de galerie met Cortana bedrijfsinformatie

1.  Download het [blog sentiment analytics model](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) vanuit de galerie met Cortana Intelligence.  
2.  Klik op **openen in Studio**in Machine Learning Studio.  

    ![Streamen Analytics Machine Learning, open Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3.  Meld u aan om te gaan naar de werkruimte. Selecteer de locatie die het beste aan uw eigen locatie.
4.  Klik op **uitvoeren** onder aan de pagina.  
5.  Nadat het proces is uitgevoerd, klikt u op **Webservice implementeren**.
6.  Het sentiment analytics-model is klaar voor gebruik. Om te valideren, klikt u op de knop **testen** en geef de tekstinvoer, zoals "Ik al Microsoft kent." De test moet worden geretourneerd als resultaat de volgende strekking:

`'Predictive Mini Twitter sentiment analysis Experiment' test returned ["4","0.715057671070099"]...`  

![Stream Analytics Machine Learning, analysegegevens](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-analysis-data.png)  

Klik op de koppeling voor **Excel 2010 of eerdere werkmap** om uw API-sleutel en de URL die u later moet voor het instellen van de Stream Analytics-taak in de kolom **Apps** . (Deze stap is alleen voor het gebruik van een Machine Learning-model van een andere werkruimte van Azure-account vereist. In dit artikel wordt ervan uitgegaan dat dit het geval is, om op te lossen dat scenario is.)  

Houd rekening met de URL en access sleutel voor de webservice uit het gedownloade bestand voor Excel, zoals hieronder wordt weergegeven:  

![Stream Analytics Machine Learning, snel aan te duiden](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  

## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a>Maken van een taak Stream Analytics dat het Machine Learning-objectmodel gebruikt

1.  Ga naar de [Azure-portal](https://manage.windowsazure.com).  
2.  Klik op **nieuwe** > **gegevensservices** > **Stream Analytics** > **snel tabellen maken**. Typ een naam voor uw taak in de **Taaknaam**, voert u de juiste regio voor de taak in de **regio**en selecteer vervolgens het account in **Regionale Monitoring opslag-Account**.    
3.  Klik op **toevoegen een invoer**nadat de taak is gemaakt, klik op het tabblad **invoeritems** .  

    ![Streamen Analytics Machine Learning, voegt u Machine Learning invoer](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-input-screen.png)  

4.  Op de eerste pagina van de wizard **Toevoegen** , klikt u op **gegevensstroom**en klik vervolgens op **volgende**. Klik op de volgende pagina op **Blob Storage** als invoer en klik vervolgens op **volgende**.  
5.  Geef op de pagina **Blob Storage instellingen** van de wizard de blob container opslagaccountnaam die u eerder hebt gedefinieerd wanneer u de gegevens hebt geüpload. Klik op **volgende**. Voor **Gebeurtenis serialisatie-indeling**, klikt u op **CSV**. Accepteer de standaardwaarden voor de rest van de pagina **serialisatie-instellingen** . Klik op **OK**.  
6.  Klik op **toevoegen een uitvoer**op het tabblad **uitvoer** .  

    ![Streamen Analytics Machine Learning, voegt u uitvoer](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-output-screen.png)  

7.  Klik op **Blob Storage**en voer dezelfde parameters, behalve voor de container. De waarde voor de **invoer** is geconfigureerd voor het lezen van de container met de naam "test" waar het **CSV-** bestand is geüpload. Voer voor **uitvoer**, "testoutput". Container namen mogen niet hetzelfde zijn. Controleer of deze container bestaat.     
8.  Klik op **volgende** om de uitvoer van **serialisatie instellingen**te configureren. Als u met de **invoer**, **CSV**op en klik vervolgens op de knop **OK** .
9.  Klik op het tabblad **functies** , op **toevoegen een Machine Learning-functie**.  

    ![Streamen Analytics Machine Learning, voeg Machine Learning-functie](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-add-ml-function.png)  

10. Zoek de werkruimte Machine Learning, webservice en standaardeindpunt op de pagina **Machine Learning Web Service-instellingen** . Pas de instellingen voor handmatig gebruikt om toegang te krijgen bekend zijn met het configureren van een webservice voor een werkruimte, zo lang maken als u de URL kent en de API-sleutel hebt voor dit artikel. Voer het eindpunt **URL** en de **API-sleutel**. Klik op **OK**.    

    ![Stream Analytics Machine Learning, Machine Learning-webservice](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-web-service.png)    

11. Wijzig op het tabblad **Query** op de query als volgt:    

 ```
    WITH subquery AS (  
        SELECT text, sentiment(text) as result from input  
    )  
 
    Select text, result.[Scored Labels]  
    Into output  
    From subquery  
 ```    
12. Klik op **Opslaan** als de query wilt opslaan.

## <a name="start-the-stream-analytics-job-and-observe-the-output"></a>Start de Stream Analytics-taak en de uitvoer toekijken

1.  Klik op **starten** onder aan de pagina van de taak.
2.  Klik in het **Dialoogvenster voor query's starten**, klik op **Aangepaste tijd**en selecteer een tijd voorafgaand aan wanneer u het CSV geüpload naar Blob storage. Klik op **OK**.  

    ![Stream Analytics Machine Learning, aangepaste tijd](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-custom-time.png)  

3.  Ga naar de blobopslag met behulp van het hulpmiddel dat u gebruikt het CSV-bestand, bijvoorbeeld Visual Studio te uploaden.
4.  Een paar minuten nadat de taak wordt gestart, de container uitvoer wordt gemaakt en een CSV-bestand wordt geüpload naar deze.  
5.  Open het bestand in de standaard CSV-editor. Iets de volgende strekking moet worden weergegeven:  

    ![Stream Analytics Machine Learning, CSV-weergave](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  

## <a name="conclusion"></a>Sluiten

In dit artikel wordt beschreven hoe een taak Stream Analytics die leest streaming tekstgegevens en sentiment analytics geldt voor de gegevens in realtime wilt maken. U kunt dit alles uitvoeren zonder dat de kneepjes van het samenstellen van een sentiment analytics-model bang nodig hebt. Dit is een van de voordelen van de Cortana Intelligence-Suite.

U kunt ook de functie-gerelateerde doelstellingen Azure Machine Learning weergeven. Klik op het tabblad **Beeldscherm** hiervoor. Drie aan de functie-gerelateerde doelstellingen worden weergegeven.  

- **Functie aanvragen** geeft het aantal aanvragen die naar een Machine Learning-webservice verzonden.  
- **Functie gebeurtenissen** geeft het aantal gebeurtenissen in het verzoek. Standaard bevat elk verzoek om bij een webservice Machine Learning tot 1000 gebeurtenissen.  
- **Mislukte functie aanvragen** geeft het aantal mislukte aanvragen voor de Machine Learning-webservice.  

    ![Stream Analytics Machine Learning, Machine Learning monitor weergeven](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-ml-monitor-view.png)  
