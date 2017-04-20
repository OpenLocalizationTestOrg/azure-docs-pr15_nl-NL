<properties 
    pageTitle="Demonstratie: Microsoft Dynamics CRM met toepassing inzichten controleren" 
    description="Telemetrielogboek ophalen uit Microsoft Dynamics CRM Online toepassing inzichten gebruik. Stapsgewijze instructies voor installatie aan gegevens, visualisatie en exporteren." 
    services="application-insights" 
    documentationCenter=""
    authors="mazharmicrosoft" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="awills"/>
 
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Demonstratie: Telemetrielogboek inschakelen voor Microsoft Dynamics CRM Online toepassing inzichten gebruik

Dit artikel leest u hoe u het telemetrielogboek gegevens ophalen uit [Microsoft Dynamics CRM Online](https://www.dynamics.com/) [Visual Studio toepassing inzichten](https://azure.microsoft.com/services/application-insights/)gebruik. We wordt begeleiden bij het voltooien van de toepassing inzichten script toevoegen aan uw toepassing, gegevens en gegevens vastleggen.

>[AZURE.NOTE] [Bladeren in de steekproef-oplossing](https://dynamicsandappinsights.codeplex.com/).

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Toepassing inzichten aan nieuwe of bestaande CRM Online exemplaar toevoegen 

Als u wilt controleren van uw toepassing, kunt u een toepassing inzichten SDK toevoegen aan uw toepassing. De SDK verzendt telemetrielogboek naar de [toepassing inzichten-portal](https://portal.azure.com), kunt u onze krachtige analyses en diagnostische hulpprogramma's, of de gegevens exporteren naar opslag.

### <a name="create-an-application-insights-resource-in-azure"></a>Maken van een resource van toepassing inzichten in Azure wordt aangegeven

1. [Een account in Microsoft Azure](http://azure.com/pricing)aanvragen. 
2. Meld u aan bij de [portal van Azure](https://portal.azure.com) en een nieuwe resource van toepassing inzichten toevoegen. Dit is waar uw gegevens worden verwerkt en wordt weergegeven.

    ![Klik op +, ontwikkelaars Services, toepassing inzichten.](./media/app-insights-sample-mscrm/01.png)

    Kies ASP.NET als het toepassingstype.

3. Open het tabblad Quick Start en open het script code.

    ![](./media/app-insights-sample-mscrm/03.png)

**De codetabel geopend houden** terwijl u de volgende stap in een ander browservenster. U moet de code binnenkort. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Een resource JavaScript-web maken in Microsoft Dynamics CRM

1. Open uw exemplaar van CRM Online en u aanmelden met beheerdersbevoegdheden.
2. Open Microsoft Dynamics CRM-instellingen aanpassingen, past u het systeem

    ![](./media/app-insights-sample-mscrm/04.png)
    
    ![](./media/app-insights-sample-mscrm/05.png)


    ![](./media/app-insights-sample-mscrm/06.png)

3. Maak een JavaScript-resource.

    ![](./media/app-insights-sample-mscrm/07.png)

    Een naam geven, selecteert u **Script (JScript)** en open de teksteditor.

    ![](./media/app-insights-sample-mscrm/08.png)
    
4. Kopieer de code uit de toepassing inzichten. Zorg dat u script-tags negeren tijdens het kopiëren. Raadpleeg onder schermafbeelding:

    ![](./media/app-insights-sample-mscrm/09.png)

    De code bevat de instrumentation-sleutel die de bron van uw toepassing inzichten aangeeft.

5. Opslaan en publiceren.

    ![](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Instrument formulieren

1. Open het accountformulier in Microsoft CRM Online

    ![](./media/app-insights-sample-mscrm/11.png)

2. Open het formulier Eigenschappen

    ![](./media/app-insights-sample-mscrm/12.png)

3. De JavaScript-web-resource die u hebt gemaakt toevoegen

    ![](./media/app-insights-sample-mscrm/13.png)

    ![](./media/app-insights-sample-mscrm/14.png)

4. Opslaan en publiceren van uw aanpassingen formulier.


## <a name="metrics-captured"></a>Aan de doelstellingen vastgelegd

U hebt nu telemetrielogboek-opname voor het formulier instellen. Wanneer deze wordt gebruikt, worden de gegevens worden verzonden naar uw toepassing inzichten resource.

Hier vindt u voorbeelden van de gegevens die u te zien krijgt.

#### <a name="application-health"></a>Toepassingsstatus

![](./media/app-insights-sample-mscrm/15.png)

![](./media/app-insights-sample-mscrm/16.png)

Browser uitzonderingen:

![](./media/app-insights-sample-mscrm/17.png)

Klik op de grafiek als u meer informatie:

![](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Gebruik

![](./media/app-insights-sample-mscrm/19.png)

![](./media/app-insights-sample-mscrm/20.png)

![](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Browsers

![](./media/app-insights-sample-mscrm/22.png)

![](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>Geolocatie

![](./media/app-insights-sample-mscrm/24.png)

![](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Aanvraag voor Inside pagina weergeven

![](./media/app-insights-sample-mscrm/26.png)

![](./media/app-insights-sample-mscrm/27.png)

![](./media/app-insights-sample-mscrm/28.png)

![](./media/app-insights-sample-mscrm/29.png)

![](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Voorbeeld van een code

[Bladeren in de steekproef-code](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI

U kunt zelfs grondigere analyse kunt doen als u [de gegevens exporteren naar Microsoft Power BI](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Voorbeeld van Microsoft Dynamics CRM-oplossing

[Als volgt de voorbeeld-oplossing geïmplementeerd in Microsoft Dynamics CRM] (https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Meer informatie

* [Wat is een toepassing inzichten?](app-insights-overview.md)
* [Toepassing inzichten voor webpagina 's](app-insights-javascript.md)
* [Meer voorbeelden en scenario 's](app-insights-code-samples.md)

 
