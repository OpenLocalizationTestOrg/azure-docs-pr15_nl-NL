<properties 
    pageTitle="Toepassing-kaart in de toepassing inzichten | Microsoft Azure" 
    description="Een visuele presentatie van de afhankelijkheden tussen de onderdelen van de app, met KPI's en waarschuwingen het label." 
    services="application-insights" 
    documentationCenter=""
    authors="SoubhagyaDash" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/15/2016" 
    ms.author="awills"/>
 
# <a name="application-map-in-application-insights"></a>Toepassing-kaart in de toepassing inzichten

Plattegrond van toepassing is in [Visual Studio toepassing inzichten](app-insights-overview.md), een visuele lay-out van de afhankelijkheidsrelaties van uw toepassingsonderdelen. Elk onderdeel toont KPI's zoals laden, prestaties, fouten en waarschuwingen, zodat u elk onderdeel veroorzaakt door een prestatieprobleem leiden of mislukt vinden. Door te klikken op via van elk onderdeel meer gedetailleerde diagnostische gegevens van toepassing inzichten, en, als uw app wordt gebruikt dat Azure services - Azure diagnostische gegevens, zoals de SQL-Database Advisor aanbevelingen.

Als andere grafieken, kunt u een toepassing met hyperlinks naar Azure dashboard, waarop de invoegtoepassing volledig functioneel is vastmaken. 

## <a name="open-the-application-map"></a>Open de toepassing-plattegrond

Open de plattegrond van het blad Overzicht voor uw toepassing:

![app-map openen](./media/app-insights-app-map/01.png)

![App-kaart](./media/app-insights-app-map/02.png)

De kaart wordt weergegeven:

* Beschikbaarheid van de tests
* Client-onderdeel (gecontroleerd met de JavaScript-SDK)
* Onderdeel van de server
* Afhankelijkheden van onderdelen van de client en server

U kunt groepen uitvouwen en samenvouwen afhankelijkheid koppeling:

![samenvouwen](./media/app-insights-app-map/03.png)
 
Als er een groot aantal afhankelijkheden van het ene type (SQL, HTTP enzovoort), kunnen deze worden weergegeven gegroepeerd. 


![gegroepeerde afhankelijkheden](./media/app-insights-app-map/03-2.png)
 
 
## <a name="spot-problems"></a>Plek problemen

Elk knooppunt heeft relevante prestatie-indicatoren, zoals het laden, prestaties en mislukt tarieven voor dat onderdeel. 

Waarschuwing pictogrammen Markeer mogelijke problemen. Een oranje waarschuwing betekent dat er fouten in aanvragen, paginaweergaven of afhankelijkheid oproepen. Rood betekent dat er een fout tarief 5% of hoger.


![Fout bij pictogrammen](./media/app-insights-app-map/04.png)

 
Meldingen van actief ook weergeven: 


![actieve waarschuwingen](./media/app-insights-app-map/05.png)
 
Als u SQL Azure gebruikt, is er een pictogram waarin wordt aangegeven wanneer er aanbevelingen over hoe u kunt de prestaties verbeteren. 


![Azure aanbeveling](./media/app-insights-app-map/06.png)

Klik op een pictogram als u meer informatie:


![Azure aanbeveling](./media/app-insights-app-map/07.png)
 
 
## <a name="diagnostic-click-through"></a>Diagnostische klik door

Elk van de knooppunten op de kaart biedt gerichte klik tot en met voor diagnostische gegevens. De opties variëren, afhankelijk van het type van het knooppunt.

![Serveropties](./media/app-insights-app-map/09.png)

 
De opties omvatten voor de onderdelen die worden gehost in Azure wordt aangegeven, directe koppelingen naar deze.


## <a name="filters-and-time-range"></a>Filters en tijdsbereik

Standaard ziet de kaart u alle gegevens die beschikbaar zijn voor het door u gekozen tijdsbereik. Maar u kunt deze zodanig dat alleen specifieke bewerking namen of afhankelijkheden filteren.

* De bewerkingsnaam van de: deze groep omvat paginaweergaven zowel servertypen kant verzoek. Met deze optie wordt ziet de kaart u de KPI op de server/client kant knooppunt voor alleen de geselecteerde bewerkingen. De afhankelijkheden genoemd in de context van die specifieke acties worden weergegeven.
* Afhankelijkheid naam: deze groep omvat de AJAX browser kant afhankelijkheden en afhankelijkheden op de server. Als u aangepaste afhankelijkheid telemetrielogboek met de API TrackDependency, wordt ze ook weergegeven hier. U kunt de afhankelijkheden om weer te geven op de kaart selecteren. Houd er rekening mee dat op dit moment kan dit niet met de server kant aanvragen of paginaweergaven op de client overeen.


![Filters instellen](./media/app-insights-app-map/11.png)

 
 
## <a name="save-filters"></a>Filters opslaan

Als u wilt opslaan in de filters die u hebt toegepast, vastmaken de gefilterde weergave op een [dashboard](app-insights-dashboards.md).


![Vastmaken aan het dashboard](./media/app-insights-app-map/12.png)
 


## <a name="feedback"></a>Feedback

Geef [feedback geven via de portal feedbackoptie](app-insights-get-dev-support.md).


![Afbeelding van de MapLink-1](./media/app-insights-app-map/13.png)


