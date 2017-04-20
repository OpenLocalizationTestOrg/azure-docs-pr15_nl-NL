<properties
    pageTitle="Overzicht van de doelstellingen in Microsoft Azure | Microsoft Azure"
    description="Informatie over het aanpassen van de controle grafieken in Azure wordt aangegeven."
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
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>Overzicht van de doelstellingen in Microsoft Azure wordt aangegeven

Alle Azure services volgen belangrijke waarmee u kunt de status, de prestaties, de beschikbaarheid en het gebruik van uw services. U kunt deze gegevens weergeven in de portal van Azure en u kunt ook de [REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx) of [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) gebruiken voor toegang via programmacode tot de volledige reeks cijfers.

Voor bepaalde services moet u mogelijk diagnostische hulpprogramma's inschakelen om te zien van de parameters. Voor anderen, zoals virtuele machines, krijgt u een eenvoudige reeks aan de doelstellingen, maar nodig hebt om in te schakelen van de volledige hoge frequentie aan de doelstellingen. Zie [cmdlets voor controle en hulpprogramma's voor diagnose inschakelen](insights-how-to-use-diagnostics.md) voor meer informatie.

## <a name="using-monitoring-charts"></a>Gebruik van grafieken controleren

U kunt een van de doelstellingen grafiek ze over een periode die u kiest.

1. Klik in de [Portal van Azure](https://portal.azure.com/)op **Bladeren**en klik vervolgens op een resource waarin u bent geïnteresseerd bewaken.

2. De **controle** -sectie bevat de doelstellingen van de belangrijkste voor elke Azure resource. Een web-app bevat bijvoorbeeld **aanvragen en fouten**, indien als een virtuele machine zou hebben **CPU percentage** en **schijf lezen en schrijven**:  ![controle lens](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)

3. Klikken op een grafiek ziet u het blad **Metrisch** . Klik op het blad, naast de grafiek, is een tabel waarin u aggregaties van de doelstellingen (zoals gemiddelde, minimum en maximum, via het tijdsbereik die u hebt gekozen). Hieronder die zijn de waarschuwingsregels voor de resource.
    ![Metrische blade](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)

4. Als u wilt aanpassen van de regels die worden weergegeven, klikt u op de knop **bewerken** op de grafiek of, de opdracht **grafiek bewerken** op het metrische blad.

5. Klik op het blad Query bewerken kunt u drie dingen doen:
    - Het tijdsbereik wijzigen
    - Overschakelen van het uiterlijk tussen balk en lijn
    - Kies andere metics ![Query bewerken](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)

6. Het tijdsbereik wijzigen is zo eenvoudig als het selecteren van een ander bereik (zoals **Afgelopen uur**) en te klikken op **Opslaan** onderaan in het blad. U kunt ook **aangepaste**waarmee u kunt een periode in de afgelopen twee weken te kiezen. Bijvoorbeeld, ziet u de hele twee weken of, NET 1 uur van gisteren. Typ in het tekstvak om in te voeren van een andere uur.
    ![Aangepaste tijdsbereik](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)

7. Onder het tijdsbereik kiest u kanaal een willekeurig aantal maatstelsel weergeven op de grafiek.

8. Wanneer u op Opslaan klikt worden, uw wijzigingen opgeslagen voor die resource. Bijvoorbeeld als u twee virtuele machines hebt en u een grafiek op een wijzigen, invloed dat geen op de andere.

## <a name="creating-side-by-side-charts"></a>Maken van grafieken naast elkaar

U kunt zo veel grafieken als u wilt toevoegen met de krachtige aanpassing in de portal.

1. Klik in het menu **…** aan de bovenkant van het blad op **tegels toevoegen**:  
    ![Menu toevoegen](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Klik, u kunt items een grafiek selecteren in de **Galerie** aan de rechterkant van het scherm:  ![galerie](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Als u de gewenste meetwaarde niet ziet, kunt u altijd toevoegen een van de vooraf ingestelde aan de doelstellingen en **Bewerk** de grafiek om weer te geven van de meetwaarde die u nodig hebt.

## <a name="monitoring-usage-quotas"></a>Gebruik quota's controleren

De meeste zien u trends na verloop van tijd, maar bepaalde gegevens, zoals quota, zijn point-in-time informatie aan een drempelwaarde.

U ziet ook quota voor gebruik op het blad voor resources die quota's hebt:

![Gebruik](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Als met de doelstellingen, kunt u de [REST API](https://msdn.microsoft.com/library/azure/dn931963.aspx) of [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) voor toegang via programmacode tot de volledige reeks quota voor gebruik.

## <a name="next-steps"></a>Volgende stappen

* [Meldingen ontvangen](insights-receive-alert-notifications.md) wanneer een meting een drempelwaarde kruist.
* [Schakel controle en diagnostische hulpprogramma's](insights-how-to-use-diagnostics.md) voor het verzamelen van gedetailleerde hoge frequentie afmetingen van uw service.
* [Automatisch schalen dat exemplaar tellen](insights-how-to-scale.md) om ervoor te zorgen dat uw service is alleen beschikbaar en heeft gereageerd.
* [Prestaties van de toepassing controleren](../application-insights/app-insights-azure-web-apps.md) als u wilt begrijpen precies hoe uw code wordt uitgevoerd in de cloud.
* Gebruik de [Toepassing inzichten voor JavaScript-apps en webpagina's](../application-insights/app-insights-web-track-usage.md) om client analytische gegevens over de browsers die naar een webpagina te gaan.
* [Beschikbaarheid van de monitor en serverreactie van een webpagina](../application-insights/app-insights-monitor-web-app-availability.md) met de toepassing inzichten zodat u als de pagina vinden kan is niet beschikbaar.
