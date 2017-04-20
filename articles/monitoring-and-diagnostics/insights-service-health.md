<properties
    pageTitle="Azure servicestatus met Azure Monitor activiteitenlogboeken bijhouden | Microsoft Azure"
    description="Meer informatie over wanneer Azure prestaties verslechtering van of service onderbrekingen is opgetreden. "
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="robb"/>

# <a name="track-azure-service-health-using-azure-monitor-activity-logs"></a>Azure servicestatus met Azure Monitor activiteitenlogboeken bijhouden

Azure publicizes telkens wanneer er een service onderbroken of prestaties verslechtering van is. U kunt deze gebeurtenissen in de portal van Azure bladeren en u kunt ook de [REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx) of [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) gebruiken voor toegang via programmacode tot de volledige reeks gebeurtenissen.

## <a name="browse-the-service-health-logs-for-your-subscription"></a>Bladeren in de service-systeemstatus-logboeken voor uw abonnement

1. Meld u aan bij de [Portal van Azure](https://portal.azure.com/).

2. Klik op **Start** ziet u een tegel **Servicestatus**genoemd. Klik erop.

    ![Start](./media/insights-service-health/Insights_Home.png)

3. U ziet een lijst met alle regio's in Azure wordt aangegeven. Klik op elk gebied om de activiteit Log-query waarin de service-incidenten die u hebt een van uw abonnementen in de afgelopen 24 uur worden beïnvloed weer te geven.

    ![Servicestatus van activiteit Log abonnement](./media/insights-service-health/AzureActivityLogServiceHealth3.png)

4. U kunt de details van een afzonderlijke incident zien door te klikken op die gebeurtenis in de tabel.

5. Wijzig de **tijdspanne** als u wilt zien van een langere periode.

## <a name="next-steps"></a>Volgende stappen

* [Beschikbaarheid van de monitor en serverreactie van een webpagina](../application-insights/app-insights-monitor-web-app-availability.md) met de toepassing inzichten zodat u als de pagina vinden kan is niet beschikbaar.
