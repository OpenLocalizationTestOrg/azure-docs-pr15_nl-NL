<properties 
    pageTitle="Het starten van streaming taken in Stream Analytics | Microsoft Azure" 
    description="Hoe een streaming taak uitvoeren in Azure Stream Analytics | leren padsegment."
    keywords="Streaming taken"
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

# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a>Het uitvoeren van een taak streaming in Azure Stream Analytics

Wanneer een taak input, de query en de uitvoer zijn alle opgegeven dat kunt u de taak Stream Analytics starten.

Uw taak starten:

1.  Klik in de portal Azure klassieke vanuit het dashboard taak, klikt u op **starten** onder aan de pagina.

    ![De knop taak starten](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  

    Klik op **Start** boven aan de pagina van uw taak in de portal Azure.

    ![Azure portal starttaak knop](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  

2.  Geef een waarde voor **Uitvoer starten** om te bepalen wanneer deze taak wordt gestart uitvoer produceren. De standaardinstelling voor taken die eerder niet zijn gestart is **Taak begintijd**, wat inhoudt dat de taak verwerkingsgegevens onmiddellijk wordt gestart. U kunt ook een **aangepaste** tijd opgeven in het verleden (voor verbruikt historische gegevens) of de toekomst (naar verwerking tot een later tijdstip vertraagd). Voor situaties wanneer een taak is eerder gestart en gestopt, is de optie **Gestopt laatst** beschikbaar te hervatten van de taak van de laatste keer dat uitvoer en gegevensverlies voorkomen.  

    ![Taak tijd streaming starten](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  

    ![Azure portal Start streaming taak tijd](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  

3.  Bevestig uw selectie. De taakstatus wordt gewijzigd in *starten* en wordt kort naar *uitgevoerd* verplaatst wanneer de taak is begonnen. U kunt de voortgang van de bewerking **starten** in de **Melding Hub**controleren:

    ![Streaming taakvoortgang van](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  

    ![Azure-portal streaming taakvoortgang van](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>U kunt hulp krijgen
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
