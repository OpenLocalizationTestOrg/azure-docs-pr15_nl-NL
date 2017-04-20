<properties
    pageTitle="Meld u Analytics-ontwerpfunctie voor weergaven | Microsoft Azure"
    description="Ontwerpfunctie voor weergaven in Log Analytics kunt u in de OMS-console met verschillende visualisaties van gegevens in de bibliotheek OMS aangepaste weergaven maken. In dit artikel bevat een overzicht van de ontwerpfunctie voor weergaven en procedures voor het maken en aangepaste weergaven bewerken."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer"></a>Ontwerpfunctie voor weergaven log-analyses
De ontwerpfunctie voor weergaven in Log Analytics kunt u in de OMS-console met verschillende visualisaties van gegevens in de bibliotheek OMS aangepaste weergaven maken. In dit artikel bevat een overzicht van de ontwerpfunctie voor weergaven en procedures voor het maken en aangepaste weergaven bewerken.

Andere artikelen die beschikbaar zijn voor de ontwerpfunctie voor weergaven zijn:

- [Overzicht van de tegel](log-analytics-view-designer-tiles.md) - overzicht van de instellingen voor elk van de tegels beschikbaar voor gebruik in uw aangepaste weergaven. 
- [Visualisatie deel verwijzing](log-analytics-view-designer-parts.md) - overzicht van de instellingen voor elk van de tegels beschikbaar voor gebruik in uw aangepaste weergaven. 


## <a name="concepts"></a>Concepten
Weergaven die zijn gemaakt met de ontwerpfunctie voor weergaven bevatten de elementen in de volgende tabel.

| Deel | Beschrijving |
|:--|:--|
| Tegel | Weergegeven op het belangrijkste Log Analytics overzicht dashboard.  Bevat een visuele samenvatten van de gegevens in de aangepaste weergave.  Verschillende typen van de tegel bieden verschillende visualisaties met records in de bibliotheek OMS.  Klik op de tegel om de aangepaste weergave te openen. |
| Aangepaste weergave | Wanneer de gebruiker op de tegel weergegeven.  Bevat een of meer visualisatie delen. |
| Onderdelen van de visualisatie | Visualisatie van gegevens in de OMS-bibliotheek op basis van een of meer [log zoekopdrachten](log-analytics-log-searches.md).  De meeste onderdelen bevat een koptekst waarmee een hoog niveau visualisatie en een lijst van de beste resultaten.  Verschillende typen bieden verschillende visualisaties met records in de bibliotheek OMS.  Klik op elementen in het onderdeel wilt doorzoeken log gedetailleerde records leveren. |

![Designer overzicht weergeven](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-to-your-workspace"></a>Ontwerpfunctie voor weergaven aan uw werkruimte toevoegen
Ontwerpfunctie voor weergaven is in de proefversie, moet u deze toevoegen aan uw werkruimte door te selecteren van **Functies** in de sectie **Instellingen** van de portal OMS.

![Voorbeeld inschakelen](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Maken en bewerken van weergaven

### <a name="create-a-new-view"></a>Een nieuwe weergave maken
Open een nieuwe weergave in de **Ontwerpfunctie voor weergaven** door te klikken op de tegel van de ontwerpfunctie voor weergaven in het hoofdvenster OMS dashboard.

![Designer venster weergeven](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Een bestaande weergave bewerken
Als u wilt bewerken in een bestaande weergave in de ontwerpfunctie voor weergaven, de weergave te openen door te klikken op de tegel in het hoofdvenster OMS dashboard.  Klik vervolgens op de knop **bewerken** om de weergave in de ontwerpfunctie voor weergaven.

![Een weergave bewerken](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Een bestaande weergave klonen
Wanneer u een weergave klonen, wordt een nieuwe weergave gemaakt en deze in de ontwerpfunctie voor weergaven worden geopend.  De nieuwe weergave heeft dezelfde naam als de oorspronkelijke met 'kopiëren' toegevoegde naar het einde van deze.  Een weergave klonen, opent u de bestaande weergave door te klikken op de tegel in het hoofdvenster OMS dashboard.  Klik vervolgens op de knop **klonen** om de weergave in de ontwerpfunctie voor weergaven.

![Een weergave klonen](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Een bestaande weergave verwijderen
Als u wilt verwijderen van een bestaande weergave, de weergave te openen door te klikken op de tegel in het hoofdvenster OMS dashboard.  Vervolgens klikt u op de knop **bewerken** om de weergave in de ontwerpfunctie voor weergaven openen en klik op **Weergave verwijderen**.

![Een weergave verwijderen](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Een bestaande weergave exporteren
U kunt een weergave exporteren naar een JSON-bestand dat u kunt importeren in een andere werkruimte of in een [Azure resourcemanager sjabloon](../resource-group-authoring-templates.md)gebruiken.  Als u wilt exporteren van een bestaande weergave, de weergave te openen door te klikken op de tegel in het hoofdvenster OMS dashboard.  Klik vervolgens op de knop **exporteren** om u te maken van een bestand in de downloadmap van de browser.  De naam van het bestand is de naam van de weergave met de extensie *omsview*.

![Een weergave exporteren](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Een bestaande weergave importeren
U kunt een *omsview* -bestand dat u hebt geïmporteerd uit een andere management groep kunt importeren.  Als u wilt importeren in een bestaande weergave, moet u eerst een nieuwe weergave maken.  Klik op de knop **importeren** en selecteer het bestand *omsview* .  De configuratie in het bestand wordt gekopieerd naar de bestaande weergave.

![Een weergave exporteren](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Werken met de ontwerpfunctie voor weergaven
De ontwerpfunctie voor weergaven bestaat uit drie deelvensters.  Het deelvenster **ontwerp** staat voor de aangepaste weergave.  Wanneer u tegels en onderdelen vanuit het deelvenster **besturingselement** aan het deelvenster **ontwerp toevoegen** worden ze toegevoegd aan de weergave.  Het deelvenster **Eigenschappen** worden de eigenschappen voor de tegel of het geselecteerde gedeelte weergegeven.

![Ontwerpfunctie voor weergaven](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Tegels view configureren
Een aangepaste weergave kan slechts één tegel hebben.  Selecteer het tabblad **-tegel** in het deelvenster **besturingselement** om te bekijken van de huidige tegel of een alternatieve te selecteren.  Het deelvenster **Eigenschappen** worden de eigenschappen voor de huidige tegel weergegeven.  De eigenschappen van de tegel op basis van de gedetailleerde informatie in de [Tegel verwijzing](log-analytics-view-designer-tiles.md) configureren en klik op **toepassen** om de wijzigingen op te slaan.

### <a name="configure-visualization-parts"></a>Onderdelen van de visualisatie configureren
Een weergave kan een willekeurig aantal visualisatie onderdelen bevatten.  Selecteer het tabblad **Beeld** en selecteer een visualisatie-onderdeel toevoegen aan de weergave.  Het deelvenster **Eigenschappen** worden de eigenschappen voor de geselecteerde sectie weergegeven.  De weergave-eigenschappen op basis van de gedetailleerde informatie in de [visualisatie deel verwijzing](log-analytics-view-designer-parts.md) configureren en klik op **toepassen** om de wijzigingen op te slaan.

### <a name="delete-a-visualization-part"></a>Een visualisatie-onderdeel verwijderen
U kunt een deel van de visualisatie vanuit de weergave verwijderen door te klikken op de knop **X** in de rechterbovenhoek van het webonderdeel formulier.

### <a name="rearrange-visualization-parts"></a>Onderdelen van de visualisatie rangschikken
Weergaven hoeft slechts één rij van onderdelen van de visualisatie.  Bestaande onderdelen in een weergave anders rangschikken door te klikken en deze naar een nieuwe locatie te slepen.


## <a name="next-steps"></a>Volgende stappen

- [Tegels](log-analytics-view-designer-tiles.md) toevoegen aan de aangepaste weergave.
- [Onderdelen van de visualisatie](log-analytics-view-designer-parts.md) toevoegen aan uw aangepaste weergave.
