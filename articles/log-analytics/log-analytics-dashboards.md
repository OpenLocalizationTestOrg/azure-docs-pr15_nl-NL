<properties
    pageTitle="Maak ik een aangepaste dashboard in Log Analytics | Microsoft Azure"
    description="Deze handleiding helpt u begrijpen hoe Log Analytics Dashboards kunt visualiseren alle zoekopdrachten opgeslagen logboek, zodat u een enkel lens om weer te geven van uw omgeving."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="create-a-custom-dashboard-in-log-analytics"></a>Maak ik een aangepaste dashboard in Log Analytics

Deze handleiding helpt u begrijpen hoe Log Analytics dashboards kunnen visualiseren alle zoekopdrachten opgeslagen logboek, zodat u een enkel lens om weer te geven van uw omgeving.

![Voorbeelddashboard](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

De aangepaste dashboards die u in de portal OMS maakt zijn ook beschikbaar in de OMS Mobile-App. Zie de volgende pagina's voor meer informatie over de apps.

- [De mobiele app OMS vanuit de Microsoft Store](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
- [De mobiele app OMS van Apple iTunes](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobiele dashboard](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Hoe maak ik mijn dashboard?

Als u wilt beginnen, gaat u naar de pagina overzicht van de OMS. Aan de linkerkant ziet u de tegel **Mijn Dashboard** . Klikt u op inzoomen op uw dashboard.

![Overzicht](./media/log-analytics-dashboards/oms-dashboards-overview.png)


## <a name="adding-a-tile"></a>Een tegel toevoegen

Tegels zijn in dashboards, mogelijk gemaakt door de opgeslagen logboek zoekopdrachten. OMS wordt geleverd met veel en-klare opgeslagen logboek zoekopdrachten, zodat u meteen kunt gaan. Gebruik de volgende stappen die een overzicht van hoe u begint.

In de mijn Dashboard weergave, klikt u **aanpassen** om in te voeren modus aanpassen.

![Afbeeldingen](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 Het deelvenster dat wordt geopend aan de rechterkant van de pagina ziet u alle van van uw werkruimte opgeslagen logboek zoekopdrachten. Als u wilt een zoekopdracht opgeslagen logboek als een tegel visualiseren, plaats de muisaanwijzer op een opgeslagen zoekactie en klik op het **plus** -symbool.

![Tegels 1 toevoegen](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Wanneer u op het **plus** -symbool klikt, wordt een nieuwe tegel weergegeven in de weergave Mijn Dashboard.

![Tegels 2 toevoegen](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)


## <a name="edit-a-tile"></a>Een tegel bewerken

In de mijn Dashboard weergave, klikt u **aanpassen** om in te voeren modus aanpassen. Klik op de tegel die u wilt bewerken. De wijzigingen rechterdeelvenster om te bewerken, en biedt een aantal opties:

![Tegel bewerken](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Tegel bewerken](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Tegel visualisaties#
Er zijn drie soorten tegel visualisaties waaruit u kunt kiezen:

|grafiektype|Beschrijving|
|---|---|
|![Staafdiagram](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png)|Geeft als een staafdiagram of een lijst met resultaten door een veld afhankelijk van een tijdlijn met uw opgeslagen logboek zoekresultaten als uw zoekopdracht log resultaten door een veld is samengevoegd of niet.
|![metrisch](./media/log-analytics-dashboards/oms-dashboards-metric.png)|Hiermee wordt de totale log zoeken resultaat treffers weergegeven als een getal in een tegel. Metrische tegels toestaan voor het configureren van een drempel die de tegel worden gemarkeerd wanneer de drempel is bereikt.|
|![lijn](./media/log-analytics-dashboards/oms-dashboards-line.png)|Hiermee wordt een tijdlijn van uw opgeslagen logboek zoeken resultaat treffers met waarden weergegeven als een lijndiagram bevat.|

### <a name="threshold"></a>Drempelwaarde
U kunt een drempelwaarde op een tegel met de metrische visualisatie maken. Selecteer op de drempelwaarde maken op de tegel. Kies of u de tegel markeren als de waarde boven of onder de door u gekozen drempel en dan de drempelwaarde onderstaande instellen.

## <a name="organizing-the-dashboard"></a>Het dashboard ordenen
Als u wilt organiseren van uw dashboard, gaat u naar de weergave Mijn Dashboard en klikt u op aanpassen **aanpassen** om in te voeren modus. Klik en sleep de tegel die u wilt verplaatsen en verplaatst u deze naar de gewenste positie voor de tegel om te worden.

![Uw Dashboard ordenen](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Een tegel verwijderen
Als u wilt verwijderen van een tegel, gaat u naar de weergave Mijn Dashboard en klikt u op aanpassen **aanpassen** om in te voeren modus. Selecteer de tegel die u wilt verwijderen en selecteer **Verwijderen-tegel**in het rechterdeelvenster.

![Een tegel verwijderen](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Volgende stappen

- [Waarschuwingen](log-analytics-alerts.md) maken in Log Analytics om meldingen te genereren en moet worden problemen hersteld.
