<properties
 pageTitle="Bekijk onderhoud vooraf geconfigureerde oplossing | Microsoft Azure"
 description="Een beschrijving van de blog onderhoud van Azure IoT vooraf geconfigureerde oplossing."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="stevehob"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="araguila"/>

# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Overzicht van de blog onderhoud vooraf geconfigureerde-oplossingen

De oplossing *blog onderhoud* vooraf geconfigureerde is een van de [vooraf geconfigureerde oplossingen] [ lnk_preconfigured_solutions] uitgebracht als onderdeel van [Microsoft Azure IoT Suite][lnk_iot_suite]. Deze oplossing realtime apparaat telemetrielogboek siteverzameling geïntegreerd met een blog model dat is gemaakt met behulp van [Azure Machine Learning][lnk_machine_learning].


Met Azure IoT Suite kunnen ondernemingen snel en eenvoudig verbinding maken met activa bewaken en analyseren van gegevens in realtime. De blog onderhoud vooraf geconfigureerde-oplossing gaat die gegevens en werkt met uitgebreide dashboards en visualisaties bedrijven zodat nieuwe intelligence waarmee u kunt sturen efficiëntie en inkomsten streams verbeteren.

## <a name="the-scenario"></a>Het Scenario

Fabrikam is een regionale luchtvaartmaatschappij die is gericht op ervaringen van uw klanten tegen scherpe prijzen. Een oorzaak van vluchtvertragingen te zien zijn is onderhoudsproblemen en vliegtuig engine onderhoud is vooral uitdaging. Engine is mislukt tijdens vlucht moet vooral worden voorkomen zodat Fabrikam de engines regelmatig controleert en aan een programma gepland onderhoud voldoet. Vliegtuig-engines niet hetzelfde echter altijd kunnen dragen. Sommige onnodige onderhoud wordt uitgevoerd op engines. Belangrijker zich problemen voordoen waarin een vliegtuig kunt grond totdat onderhoud wordt uitgevoerd. Hierdoor dure vertragingen, met name als een vliegtuig is ingesteld op een locatie waar de juiste technici of onderdelen niet beschikbaar zijn.

De engines van van Fabrikam vliegtuig worden geïmplementeerd met sensoren die engine voorwaarden tijdens vlucht controleren. Azure IoT Suite Fabrikam gebruiken voor het verzamelen van de sensorgegevens tijdens de vlucht verzameld. Nadat u hebt opgebouwd jaren van engine operationele en mislukt gegevens, hebben van Fabrikam gegevens wetenschappers gebaseerd op een manier om te voorspellen van de resterende handig leven (RUL) van een vliegtuig-engine. Wat ze hebben geïdentificeerd is een relatie tussen de gegevens uit vier van de sensors engine met het gebruik van engine die naar eventuele is mislukt leidt. Terwijl Fabrikam regelmatig controle om ervoor te zorgen veiligheid uitvoeren blijft, kunt deze nu de modellen te berekenen van de RUL voor elke engine na elke flight met het telemetrielogboek die worden verzameld uit de engines tijdens de vlucht gebruiken. Fabrikam kan nu voorspellen toekomstige punten van is mislukt en onderhoud plannen en vooraf herstellen.

> [AZURE.NOTE] Werkelijke engine gebruik gegevens wordt gebruikt door het model oplossing.

Door de komma voorspellen wanneer onderhoud vereist is, Fabrikam kunt hun activiteiten optimaliseren voor verlagen. Onderhoud coördinatoren werken met planners plannen onderhoud combinatie met een vliegtuig stoppen op een bepaalde locatie en ervoor zorgen dat voldoende tijd vrij voor het vliegtuig moeten afmelden bij service zonder dat planning verstoringen. Fabrikam kan; technici plannen ervoor zorgen dat vliegtuig worden efficiënt zonder wachttijd verwerkt. Voorraad besturingselement managers ontvangen onderhoud-abonnementen, zodat ze kunnen hun nodig optimaliseren en vrije onderdelenvoorraad. Dit alles kunnen Fabrikam minimaliseren vliegtuig grond en verlagen en ervoor zorgen dat de veiligheid van personen en personeel.

Voor meer informatie over hoe [Azure IoT Suite] [ lnk_iot_suite] biedt de klanten mogelijkheden moeten zich realiseert dat de mogelijkheden van de blog onderhoud, Controleer dit [infographic][lnk_infographic].

## <a name="how-the-predictive-maintenance-solution-is-built"></a>Hoe de blog onderhoud-oplossing is gemaakt

Een bestaand Azure Machine Learning-model beschikbaar als een sjabloon voor het weergeven van deze functies werken vanuit uw apparaat telemetrielogboek die worden verzameld via IoT Suite services maakt gebruik van de oplossing. Microsoft heeft een [regressiemodel] ingebouwd[ lnk_regression_model] van een vliegtuig-engine en de volledige sjabloon, gegevens die zijn gepubliceerd<sup>\[1\]</sup>, en stapsgewijze instructies over het gebruik van het model.

De oplossing van de blog onderhoud vooraf geconfigureerde Azure IoT maakt gebruik van de regressiemodel dat is gemaakt op basis van deze sjabloon. Er is geïmplementeerd in uw Azure abonnement en weergegeven via een automatisch gegenereerde API. De oplossing bevat een subset van de tests gegevens dat staat voor 4 (van 100 totale)-engines en de 4 (van 21 totale) sensor gegevensstromen die een nauwkeurige resultaat uit het model ervaren bieden.

*\[1\] A. Saxena en K. Goebel (2008). 'Turbofanmotoren Engine verslechtering van simulatie gegevensverzameling', NASA Ames Prognostics gegevensopslagplaats (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett veld, CA*

## <a name="next-steps"></a>Volgende stappen

Lees voor meer informatie over hoe Azure IoT blog onderhoud scenario's kunt [vastleggen van de waarde van de Internet van zaken aan bod][lnk_capture_value].

Volg een [rondleiding] [ lnk-predictive-walkthrough] vooraf geconfigureerde oplossing van de blog onderhoud.

[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF

U kunt ook enkele van de andere functies en mogelijkheden van de oplossingen IoT Suite vooraf geconfigureerde verkennen:

- [Veelgestelde vragen over IoT Suite][lnk-faq]
- [IoT beveiliging helemaal omhoog][lnk-security-groundup]

[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
