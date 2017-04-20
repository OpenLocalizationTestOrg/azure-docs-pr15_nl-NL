<properties
 pageTitle="Bekijk onderhoud scenario | Microsoft Azure"
 description="Stapsgewijze instructies voor de blog onderhoud van Azure IoT vooraf geconfigureerde oplossing."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="aguilaaj"
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

# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Bekijk onderhoud vooraf geconfigureerde oplossing Stapsgewijze instructies

## <a name="introduction"></a>Inleiding

De IoT Suite blog onderhoud vooraf geconfigureerde oplossing is een end-to-end-oplossing voor een bedrijfsscenario die voorspellen = de punt als fout kan worden kunnen optreden. U kunt deze vooraf geconfigureerde oplossing proactief te gebruiken voor activiteiten zoals het optimaliseren van onderhoud. De oplossing combineert belangrijke Azure IoT Suite services, met inbegrip van een [Azure Machine Learning] [ lnk_machine_learning] werkruimte. Deze werkruimte bevat experimenten, op basis van een gegevensverzameling openbare voorbeeld, worden de resterende handig leven (RUL) van een vliegtuig-engine voorspeld. De oplossing implementeert volledig het bedrijfsscenario IoT als uitgangspunt voor het plannen en implementeren van een oplossing die voldoet aan de vereisten van uw eigen specifieke zakelijke.

## <a name="logical-architecture"></a>Logische architectuur

In het volgende diagram bevat een overzicht van de logische onderdelen van de vooraf geconfigureerde oplossing:

![][img-architecture]

De blauwe items zijn Azure services die op de locatie die u selecteert wanneer u de vooraf geconfigureerde oplossing inrichten is ingericht. U kunt de vooraf geconfigureerde oplossing in de VS Oost, Noord Europe of Oost-Azië regio inrichten.

Enkele bronnen zijn niet beschikbaar in de regio's waar u de vooraf geconfigureerde oplossing inrichten. De oranje items in het diagram zijn de Azure services deze is ingericht in de eerstvolgende beschikbaar regio (Zuid centraal ons, Europe West of Zuidoost Azië) gegeven van de geselecteerde regio.

Het groene item is een gesimuleerd apparaat met een vliegtuig-engine. U kunt meer informatie over deze gesimuleerd apparaten in de volgende sectie.

De grijze items zijn de onderdelen die *apparaat* beheermogelijkheden implementeren. De huidige versie van de oplossing blog onderhoud vooraf geconfigureerde wordt niet inrichten van deze resources. Meer informatie over het beheer van het apparaat, raadpleegt u de [externe controle vooraf geconfigureerde oplossing][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Gesimuleerd apparaten

In de vooraf geconfigureerde oplossing vertegenwoordigt een gesimuleerd apparaat een vliegtuig-engine. De oplossing is met twee engines die naar een enkel vliegtuig verwijzen ingericht. Elke engine genereert vier typen telemetrielogboek: Sensor 9, 11 Sensor Sensor 14 en 15 Sensor bieden u de gegevens die nodig zijn voor het model Machine Learning voor het berekenen van de resterende handig leven (RUL) voor de engine. Elk gesimuleerd apparaat verzendt de volgende telemetrielogboek berichten naar IoT Hub:

*Cyclus tellen*. Een cyclus vertegenwoordigt een voltooide vlucht van variabele lengte tussen 2-10 uur waarin telemetriegegevens elk half uur tijdens de vlucht wordt vastgelegd.

*Telemetrielogboek*. Er zijn vier sensoren die kenmerken van de engine vertegenwoordigen. De sensors generieke Sensor 9, 11 Sensor Sensor 14 en 15 Sensor bevatten. Deze 4 sensoren vertegenwoordigen telemetrielogboek voldoende voor nuttige resultaten uit het model Machine Learning voor RUL. Dit model wordt gemaakt van een openbare gegevensverzameling die reële engine sensorgegevens bevat. Zie voor meer informatie over de manier waarop het model van de oorspronkelijke gegevensset is gemaakt, de [Cortana Intelligence galerie blog onderhoud sjabloon][lnk-cortana-analytics].

De volgende opdrachten verzonden vanuit een hub IoT kunnen worden verwerkt door de gesimuleerd apparaten:

| Opdracht | Beschrijving |
|---------|-------------|
| StartTelemetry | Bepaalt de status van de simulatie.<br/>Hiermee start u het apparaat dat u verzendt telemetrielogboek     |
| StopTelemetry  | Bepaalt de status van de simulatie.<br/>Het apparaat dat u verzendt telemetrielogboek stopt |

IoT Hub biedt apparaat opdracht bevestiging.

## <a name="azure-stream-analytics-job"></a>Azure Stream Analytics-taak

**Taak: Telemetrielogboek** werkt op de binnenkomende apparaat telemetrielogboek stream met twee instructies. De eerste selecteert van alle telemetrielogboek vanaf de apparaten en verzendt deze gegevens naar blob storage vanaf waar deze worden weergegeven in de WebApp. De tweede instructie wordt berekend gemiddelde sensor waarden over een schuifregelaar venster twee minuten en verzendt deze gegevens via de gebeurtenis hub naar een **gebeurtenis processor**.

## <a name="event-processor"></a>Gebeurtenis-processor

De **gebeurtenis processor** gaat de gemiddelde sensor waarden voor een voltooide cyclus. Deze de stadia deze waarden naar een API die de Machine Learning beschrijft-training model voor het berekenen van de RUL voor een engine.

## <a name="azure-machine-learning"></a>Azure Machine Learning

Zie voor meer informatie over de manier waarop het model van de oorspronkelijke gegevensset is gemaakt, de [Cortana Intelligence galerie blog onderhoud sjabloon][lnk-cortana-analytics].

## <a name="lets-start-walking"></a>We gaan lopen

In deze sectie begeleidt u bij de onderdelen van de oplossing, de beoogde use-case beschreven en worden voorbeelden gegeven.

### <a name="predictive-maintenance-dashboard"></a>Bekijk onderhoud Dashboard

Deze pagina in de webtoepassing gebruikt PowerBI JavaScript-besturingselementen (Zie de [PowerBI-visuele elementen opslagplaats][lnk-powerbi]) om te visualiseren:

- De uitvoergegevens van de Stream Analytics-taken in-blobopslag.
- Het aantal RUL en cyclus per vliegtuig engine.

### <a name="observing-the-behavior-of-the-cloud-solution"></a>Het gedrag van de oplossing cloud naleving

Navigeer naar de resourcegroep met de naam van de oplossing die u hebt gekozen om weer te geven van uw ingerichte resources in de portal Azure.

![][img-resource-group]

Wanneer u de vooraf geconfigureerde oplossing inrichten, ontvangt u een e-mailbericht met een koppeling naar de Machine Learning-werkruimte. U kunt ook naar de werkruimte Machine Learning navigeren vanaf de [azureiotsuite.com] [ lnk-azureiotsuite] pagina voor uw ingerichte oplossing wanneer deze in de status **Gereed** .

![][img-machine-learning]

Klik in de portal oplossing kunt u zien dat de steekproef met vier gesimuleerd apparaten om aan te geven van twee vliegtuig met twee engines per vliegtuig, elk met vier sensoren is ingericht. Als u eerst naar de portal oplossing gaat, wordt de simulatie is gestopt.

![][img-simulation-stopped]

Klik op **Start simulatie** om te beginnen met de simulatie waarin u de geschiedenis sensor, RUL, maal, ziet en RUL geschiedenis invullen in het dashboard.

![][img-simulation-running]

Wanneer RUL is minder dan 160 (een willekeurige drempel voorbeeld), wordt er in de portal oplossing geeft een waarschuwingssymbool naast de weergave RUL en de engine vliegtuig geel gemarkeerd. Zoals u ziet hoe de waarden RUL een algemene neerwaartse trend algehele hebt, maar meestal Stuiteren omhoog en omlaag. Dit probleem wordt veroorzaakt door de verschillende cyclus lengte berekend en de nauwkeurigheid model.

![][img-simulation-warning]

De volledige simulatie duurt ongeveer 35 minuten om uit te voeren 148 maal. De drempelwaarde voor 160 RUL is voldaan voor de eerste keer bij ongeveer 5 minuten en beide engines druk op de drempelwaarde voor ongeveer 8 minuten.

De simulatie wordt uitgevoerd door de volledige gegevensset voor 148 maal en betaalt op definitief RUL en cyclus waarden.

U kunt de simulatie op elk gewenst moment stoppen, maar opnieuw de simulatie vanaf het begin van de gegevensset te klikken op **Start simulatie** weergegeven.

## <a name="next-steps"></a>Volgende stappen

Nu u de blog onderhoud vooraf geconfigureerde oplossing die u wilt mogelijk wijzigen, raadpleegt u [richtlijnen voor het aanpassen van de vooraf geconfigureerde oplossingen]hebt uitgevoerd[lnk-customize].

Het blogbericht [IoT Suite - onder de geavanceerde instellingen weergeven - blog onderhoud](http://social.technet.microsoft.com/wiki/contents/articles/33527.iot-suite-under-the-hood-predictive-maintenance.aspx) TechNet vindt u aanvullende informatie over de blog onderhoud vooraf geconfigureerde-oplossing.

U kunt ook enkele van de andere functies en mogelijkheden van de oplossingen IoT Suite vooraf geconfigureerde verkennen:

- [Veelgestelde vragen over IoT Suite][lnk-faq]
- [IoT beveiliging helemaal omhoog][lnk-security-groundup]


[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-suite-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-suite-predictive-walkthrough/machine-learning.png
[img-simulation-stopped]: media/iot-suite-predictive-walkthrough/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-walkthrough/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-walkthrough/simulation-warning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
