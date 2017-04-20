<properties 
    pageTitle="Het maken van een taak gegevens analytics verwerking van Stream analysegegevens | Microsoft Azure" 
    description="Maken van een taak gegevens analytics verwerking van Stream analysegegevens | leren padsegment."
    keywords="analyseverwerking gegevens"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/> 

# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Het maken van een taak gegevens analytics verwerking van Stream analysegegevens

De resource op het hoogste niveau in Azure Stream Analytics is een Stream Analytics-taak.  Bestaat uit een of meer invoergegevens bronnen, een query die de gegevenstransformatie uitdrukken en een of meer uitvoer doelen die resultaten naar worden geschreven. Samen dat deze de gebruiker kan gegevens analyses processing voor streaming gegevens scenario's uitvoeren.

Als u wilt gaan gebruiken Stream analyses, gaat u eerst een nieuwe taak van de Stream Analytics maakt.  Houd er rekening mee dat deze actie geen facturering gevolgen heeft totdat de taak is gestart.

1.  Meld u aan op de online [Azure klassieke portal](http://manage.windowsazure.com) of de [Azure-portal](https://portal.azure.com/).
2.  Klik in de portal: **Klik op Nieuw**, klik vervolgens op **Gegevensservices** of **Analyses van gegevens** , afhankelijk van de portal en klik op ** **Azure Stream Analytics** of Stream analyses** en vervolgens **Snelle maken**.

    ![Wizard van de taak analyseverwerking gegevens](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  

    ![Gegevens analytics verwerking van taak maken](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  

3.  Geef de gewenste configuratie voor de taak Stream Analytics.
    - Voer een naam voor de Stream Analytics-taak in het vak **Taaknaam** . Wanneer de **Taaknaam** is gevalideerd, wordt een groen vinkje weergegeven in het vak taaknaam. De **Taaknaam** mogelijk alleen alfanumerieke tekens bevatten en de '-' karakter en moet liggen tussen 3 en 63 tekens.
    - Gebruik **gebied** in de portal van Azure of **locatie** in de portal van Azure om op te geven van de geografische locatie waar u de taak uit te voeren.
    - Als de portal van Azure gebruikt, selecteert u of een opslag-account wilt gebruiken als het **Regionale Monitoring opslag-Account**maken. Dit account opslag wordt gebruikt voor de opslag van controlegegevens voor alle Stream Analytics-taken in dit gebied wordt uitgevoerd.
    - Als de portal van Azure gebruikt, geeft u een nieuwe of bestaande **Resourcegroep** waarin verwante resources voor uw toepassing.

4.  Nadat de nieuwe opties voor de Stream Analytics-taak zijn geconfigureerd, klikt u op **Stream Analytics-taak maken**. Het kan enkele minuten duren voordat de Stream Analytics-taak moet worden gemaakt. U kunt u de status controleren door de voortgang in de hub meldingen controleren.

    ![Gegevens analytics verwerking taak notfications hub](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  

    ![Azure portal gegevens analytics verwerking van taak maken taak](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  

5.  De nieuwe taak wordt weergegeven met de status van **gemaakt**. Zoals u ziet dat **de startknop** is uitgeschakeld. U moet de invoer van de taak, de query en de uitvoer configureren voordat u kunt de taak.

    ![Gegevens analyseverwerking taak taak Status](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  

    ![Azure portal gegevens analytics processing taak taakstatus](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>U kunt hulp krijgen
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
