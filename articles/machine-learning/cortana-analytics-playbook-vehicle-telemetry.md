<properties 
    pageTitle="Voertuig telemetrielogboek analytics oplossing playbook | Microsoft Azure" 
    description="De mogelijkheden van Cortana Intelligence gebruiken om te krijgen van realtime en bekijk inzichten in een voertuig gezondheid en sturende voorkeur werkwijze." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Voertuig telemetrielogboek analytics oplossing playbook

In dit **menu** koppelingen naar de hoofdstukken in deze playbook. 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Overzicht
Super computers afmelden bij de testomgeving bent aangekomen en zijn nu geparkeerd in onze garage! Deze geavanceerde auto bevatten een groot aantal sensoren, zodat deze kunnen bijhouden en bewaken miljoenen gebeurtenissen per seconde. We verwachten dat in 2020, meestal deze auto's wordt hebt is verbonden met internet. Stel toegang te krijgen tot deze enorme hoeveelheid gegevens op te geven aanbevolen in class veiligheid, betrouwbaarheid en achter! Microsoft heeft gesteld dat dit een reality met Cortana Intelligence aan het einde.

Microsoft Cortana Intelligence is een volledig beheerde big data en geavanceerde analyses-suite waarmee u kunt uw gegevens transformeren in intelligente actie. Willen we bieden een inleiding tot de Cortana Intelligence voertuig Telemetrielogboek Analytics oplossing sjabloon. Deze oplossing wordt gedemonstreerd hoe auto dealerbedrijven, auto fabrikanten en ondernemingen de beschikking over de mogelijkheden van Cortana Intelligence krijgen realtime en bekijk inzichten in een voertuig gezondheid en sturende achterhalen. 

De oplossing wordt geïmplementeerd als een [lambda architectuur patroon](https://en.wikipedia.org/wiki/Lambda_architecture) met het volledige mogelijke van het Cortana Intelligence-platform voor realtime en batchbestand. De oplossing: 

- biedt een simulator voertuig telematica
- maakt gebruik van de gebeurtenis Hubs voor miljoenen gesimuleerd voertuig telemetrielogboek gebeurtenissen in Azure ingesting 
- Stream Analytics gebruikt om te krijgen van realtime inzichten over voertuig status
-  zich blijft voordoen de gegevens in de lange termijn opslag van uitgebreidere batch analysegegevens. 
- maakt gebruik van Machine Learning voor detectie van de afwijking in realtime en batch verwerking blog inzicht krijgen.
- maakt gebruik van HDInsight om gegevens bij het op schaal en gegevens Factory worden afgehandeld situatie, planning, resourcebeheer en controleren van de pijplijn batchbestand transformeren 
- Deze oplossing biedt een uitgebreide dashboard voor realtimegegevens en bekijk analyses visualisaties met behulp van Power BI

## <a name="architecture"></a>Architectuur

![](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Afbeelding 1: een voertuig Telemetrielogboek Analytics-oplossingsarchitectuur*

Deze oplossing omvat de volgende **Cortana Intelligence-onderdelen** en gepresenteerd hun end-to-end-integratie


- **Gebeurtenis Hubs** voor ingesting miljoenen voertuig telemetrielogboek gebeurtenissen in Azure.
- **Stream Analytics** voor realtime inzichten over voertuig status en zich blijft voordoen die gegevens in de lange termijn opslag van uitgebreidere batch analysegegevens.
- **Machine Learning** voor detectie van de afwijking in realtime en batchbestand naar blog inzicht krijgen.
- **HDInsight** wordt gebruikt om gegevens bij het op schaal transformeren
- **Gegevens Factory** verwerkt situatie, planning, resourcebeheer en het monitoren van de pijplijn batchbestand.
- **Power BI** krijgt deze oplossing een uitgebreide dashboard voor realtimegegevens en bekijk analyses visualisaties.

Deze oplossing toegang heeft tot twee verschillende **gegevensbronnen**: 

- **Gesimuleerd voertuig signalen en diagnostische hulpprogramma's**: een voertuig telematica simulator genereert diagnostische informatie en signalen die overeenkomen met de status van het voertuig en het een patroon op een bepaald moment in tijd. 
- **Voertuig catalogus**: een verwijzing gegevensset met een Chassisnummer model toewijzen aan.
