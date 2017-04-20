<properties
    pageTitle="Waarschuwingen instellen voor query's in Stream Analytics | Microsoft Azure"
    description="Lidmaatschap Stream Analytics waarschuwingen"
    keywords="waarschuwingen instellen"
    services="stream-analytics"
    documentationCenter=""
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


# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Waarschuwingen voor Azure Stream Analytics taken instellen

## <a name="introduction-monitor-page"></a>Inleiding: Monitor pagina

U kunt waarschuwingen instellen voor het starten van een melding wanneer een meting bereikt een voorwaarde die u opgeeft.

Bijvoorbeeld: "als uitvoer gebeurtenissen voor de laatste 15 minuten is < 100, e-mailbericht verzenden naar e-id: xyz@company.com”.

Regels kunnen worden ingesteld op bijhouden via de portal of geconfigureerde [via programmacode](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) kunnen worden over bewerking Logboeken gegevens.

## <a name="set-up-alerts-through-the-azure-classic-portal"></a>Instellen van waarschuwingen via de Portal van Azure-klassiek

Er zijn twee manieren voor het instellen van waarschuwingen in de klassieke Azure-portal:  

1.  Het tabblad **Beeldscherm** van de Stream Analytics-uptaak  
2.  Het logboek bewerkingen in de Management-services  

## <a name="set-up-alert-through-the-monitor-tab-of-the-job-in-the-portal"></a>Waarschuwing via het tabblad beeldscherm van de taak in de portal instellen

1.  De meetwaarde selecteren in het tabblad beeldscherm en klik op de knop **Regel toevoegen** in de linkerbenedenhoek van het dashboard en configuratie van uw regels.  

    ![Dashboard](./media/stream-analytics-set-up-alerts/01-stream-analytics-set-up-alerts.png)  

2.  De naam en beschrijving van de waarschuwing definiëren  

    ![Regel maken](./media/stream-analytics-set-up-alerts/02-stream-analytics-set-up-alerts.png)  

3.  Voer de drempels, waarschuwing evaluatie-venster en de acties voor de melding  

    ![Voorwaarden definiëren](./media/stream-analytics-set-up-alerts/03-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-through-the-operations-logs"></a>Instellen van waarschuwingen tot en met de bewerkingen-Logboeken

1.  Ga naar het tabblad **waarschuwingen** in Management-Services in de [Klassieke Azure-Portal](https://manage.windowsazure.com).  
2.  Klik op **regel toevoegen**  

    ![Criteria](./media/stream-analytics-set-up-alerts/04-stream-analytics-set-up-alerts.png)  

3.  De naam en beschrijving van de waarschuwing definiëren. Selecteer 'Stream Analytics' als Type Service en de naam van de taak als de naam van de Service.  

    ![Waarschuwing definiëren](./media/stream-analytics-set-up-alerts/05-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-in-the-azure-portal"></a>Instellen van waarschuwingen in de portal van Azure ##

Klik in de portal Azure Blader naar de Stream Analytics-taak waarin u geïnteresseerd waarschuwen bent bij en klik op de sectie **controle** .  Klik in het blad **Metrisch** dat wordt geopend, klikt u op de opdracht **melding toevoegen** .

  ![Azure portal-instellingen](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

U kunt de waarschuwing regel een naam en kies een beschrijving die wordt weergegeven in het e-mailbericht voor de melding.

Wanneer u aan de doelstellingen selecteert kiest u een voorwaarde en een drempel waarde voor de meetwaarde.

  ![Azure-portal select meetwaarde](./media/stream-analytics-set-up-alerts/07-stream-analytics-set-up-alerts.png)  

Zie voor meer informatie over het configureren van waarschuwingen in de portal van Azure, [waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  

## <a name="get-help"></a>U kunt hulp krijgen
Voor hulp, probeert u onze [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Stream Analytics](stream-analytics-introduction.md)
- [Aan de slag met Azure Stream Analytics](stream-analytics-get-started.md)
- [Schaal Azure Stream Analytics taken](stream-analytics-scale-jobs.md)
- [Naslaggids voor Azure Stream Analytics-Query](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API verwijzing](https://msdn.microsoft.com/library/azure/dn835031.aspx)
