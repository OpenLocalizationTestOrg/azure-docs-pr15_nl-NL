<properties
    pageTitle="Oplossing in Log Analytics het bijhouden van wijzigingen | Microsoft Azure"
    description="U kunt de oplossing configuratie wijzigingen bijhouden in Log Analytics beter kunt herkennen eenvoudig software en Services van Windows die worden gewijzigd in uw omgeving, deze configuratiewijzigingen identificeren, kunt u pinpoint operationele problemen."
    services="operations-management-suite"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="operations-management-suite"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="change-tracking-solution-in-log-analytics"></a>Oplossing voor het bijhouden in Log Analytics wijzigen


U kunt de oplossing configuratie wijzigingen bijhouden in Log Analytics beter kunt herkennen eenvoudig software en Services van Windows wijzigingen of Linux daemon die in uw omgeving optreden, deze configuratiewijzigingen identificeren, kunt u pinpoint operationele problemen.

U de oplossing als u wilt bijwerken van het type agent die u hebt geïnstalleerd. Wijzigingen in de software geïnstalleerd en Windows-services op de gecontroleerde servers worden opgelezen en klikt u vervolgens de gegevens wordt verzonden naar de Log Analytics-service in de cloud voor verwerking. Logica wordt toegepast op de ontvangen gegevens en de gegevens voor de cloudservice-records. Wanneer wijzigingen worden gevonden, wordt servers met wijzigingen worden weergegeven in het dashboard wijzigingen bijhouden. Met behulp van de informatie op het dashboard wijzigingen bijhouden, kunt u gemakkelijk de wijzigingen die zijn aangebracht in de serverinfrastructuur van uw zien.

## <a name="installing-and-configuring-the-solution"></a>Installeren en configureren van de oplossing
Gebruik de volgende informatie voor het installeren en configureren van de oplossing.

- Operations Manager is vereist voor de oplossing wijzigingen bijhouden.
- U moet een agent Windows of Operations Manager op elke computer waarop om wijzigingen te bewaken.
- De oplossing wijzigingen bijhouden toevoegen aan uw OMS werkruimte met behulp van het proces wordt beschreven in [de oplossingen Log Analytics toevoegen vanuit de galerie met oplossingen](log-analytics-add-solutions.md).  Er is geen verdere configuratie vereist.


## <a name="change-tracking-data-collection-details"></a>Wijzigen van de siteverzameling detailgegevens bijhouden

Het bijhouden van wijzigingen verzamelt software voorraad en Windows-Service metagegevens met de agenten die u hebt ingeschakeld.

De volgende tabel ziet u methoden voor het verzamelen van gegevens en andere informatie over hoe gegevens worden verzameld voor wijzigingen bijhouden.

| platform | Directe Agent | SCOM agent | Azure-opslag | SCOM vereist? | SCOM agent gegevens die per management groep worden verzonden | frequentie van de siteverzameling |
|---|---|---|---|---|---|---|
|Windows|![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Nee](./media/log-analytics-change-tracking/oms-bullet-red.png)|            ![Nee](./media/log-analytics-change-tracking/oms-bullet-red.png)|![Ja](./media/log-analytics-change-tracking/oms-bullet-green.png)| per uur|

## <a name="use-change-tracking"></a>Wijzigingen bijhouden gebruiken

Nadat de oplossing is geïnstalleerd, kunt u het overzicht van wijzigingen voor uw gecontroleerde servers weergeven met behulp van de tegel **Wijzigingen bijhouden** op de pagina **Overzicht** in OMS.

![afbeelding van de tegel voor wijzigingen bijhouden](./media/log-analytics-change-tracking/oms-changetracking-tile.png)

U kunt wijzigingen in uw infrastructuur en klik vervolgens op details meer details in voor de volgende categorieën bekijken:

- Wijzigingen per configuratie voor software en services voor Windows
- Software wordt gewijzigd in toepassingen en updates voor afzonderlijke servers
- Totaal aantal softwarewijzigingen voor elke toepassing
- Windows-servicewijzigingen voor afzonderlijke servers

![afbeelding van de dashboard wijzigingen bijhouden](./media/log-analytics-change-tracking/oms-changetracking01.png)

![afbeelding van de dashboard wijzigingen bijhouden](./media/log-analytics-change-tracking/oms-changetracking02.png)

### <a name="to-view-changes-for-any-change-type"></a>Als u wilt bekijken wijzigen wijzigingen voor een type

1. Klik op de tegel **Wijzigingen bijhouden** op de pagina **Overzicht** .
2. Op het dashboard **Wijzigen bijhouden** , Controleer de samenvatting van de gegevens in een van de bladen van de type wijzigen en klik op een om gedetailleerde informatie over deze op de pagina **log** weer te geven.
3. Klik op alle pagina's log zoeken, kunt u de resultaten bekijken door de tijd, gedetailleerde resultaten en de geschiedenis van log zoeken. U kunt ook filteren op aspecten en de resultaten te beperken.

## <a name="next-steps"></a>Volgende stappen

- Gebruik [Log zoekopdrachten in Log Analytics](log-analytics-log-searches.md) gedetailleerde wijzigingen bijhouden van gegevens weergeven.
