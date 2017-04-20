<properties
    pageTitle="Overzicht van Microsoft Azure IoT Suite | Microsoft Azure"
    description="Overzicht van hoe Azure IoT Suite biedt internet van de vooraf geconfigureerde oplossingen dingen te verzamelen, analyseren, en opslag van gegevens, bieden visualisaties en integreren met andere systemen."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/09/2016"
     ms.author="dobett"/>

# <a name="what-is-azure-iot-suite"></a>Wat is Azure IoT Suite?

Azure internet van dingen (IoT) services bieden een breed scala van mogelijkheden voor. Deze enterprise grade-services kunnen u naar:

- Gegevens verzamelen van apparaten
- Analyseren van gegevens streams in de beweging
- Opslaan en query grote gegevenssets
- Historische zowel realtime gegevens visualiseren
- Integreren met back-office systemen

Voor het leveren van deze mogelijkheden, Azure IoT Suite-pakketten samen meerdere Azure services met aangepaste extensies als *vooraf geconfigureerde oplossingen*. Deze vooraf geconfigureerde oplossingen zijn grondtal implementaties van algemene IoT oplossing patronen waarmee verkleinen van de tijd die u voor het leveren van uw IoT-oplossingen. Werken met de [SDK van IoT's][lnk-sdks], u kunt aanpassen en deze oplossingen om te voldoen aan uw eigen vereisten uitbreiden. U kunt ook deze oplossingen als voorbeelden of sjablonen gebruiken wanneer u nieuwe IoT oplossingen ontwikkelt.

De volgende video bevat een inleiding tot Azure IoT Suite:

> [AZURE.VIDEO azurecon-2015-introducing-the-microsoft-azure-iot-suite]

## <a name="azure-iot-services-in-azure-iot-suite"></a>Azure IoT services in Azure IoT-Suite

De vooraf geconfigureerde oplossingen meestal de volgende services gebruikt:

- Core Azure IoT Suite is de [Hub van Azure IoT] [ lnk-iot-hub] service. Deze service biedt het apparaat naar cloud en cloud-to-device SMS mogelijkheden en fungeert als de gateway bij naar de cloud en de andere belangrijke IoT Suite-services. De service kunt u berichten ontvangt van uw apparaten bij het op schaal en opdrachten verzenden naar uw apparaten.

- [Azure Stream Analytics] [ lnk-asa] gegevensanalyse in beweging bevat. IoT Suite maakt gebruik van deze service om binnenkomende telemetrielogboek proces, aggregatie uitvoeren en gebeurtenissen detecteren. Stream analytics de vooraf geconfigureerde oplossingen ook gebruiken om informatie berichten die gegevens zoals metagegevens of opdracht antwoorden van apparaten bevatten te verwerken. De oplossingen gebruik Stream Analytics verwerking van de berichten op uw apparaten en geven van die berichten naar andere services.

- [Azure opslag] [ lnk-azure-storage] en [Azure DocumentDB] [ lnk-document-db] de opslagruimte mogelijkheden om gegevens te bieden. De vooraf geconfigureerde oplossingen gebruik blobopslag om op te slaan telemetrielogboek en maak het beschikbaar voor analyse. De oplossingen gebruik DocumentDB voor het opslaan van metagegevens voor apparaten en inschakelen van de mogelijkheden voor het beheer van apparaat van de oplossingen.

- [Azure-WebApps] [ lnk-web-apps] en [Microsoft Power BI] [ lnk-power-bi] de mogelijkheden van de gegevensvisualisatie bieden. De flexibiliteit van Power BI kunt u uw eigen interactieve dashboards maken die gebruikmaken van IoT Suite gegevens snel kunt maken.

Zie [Microsoft Azure en de Internet van zaken aan bod (IoT)]voor een overzicht van de architectuur van een typisch IoT-oplossing,[iot-suite-what-is-azure-iot].

## <a name="preconfigured-solutions"></a>Vooraf geconfigureerde oplossingen

IoT Suite bevat vooraf geconfigureerde oplossingen waarmee u snel aan de slag met en algemene IoT scenario's, zoals *externe controle* verkennen en *Bekijk onderhoud*, die Azure IoT Suite mogelijk maakt. U kunt deze oplossingen implementeren naar uw Azure-abonnement en voer vervolgens een volledige, end-to-end IoT scenario.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een overzicht van wat IoT Suite kan doen en wat zijn de belangrijkste onderdelen hebt, kunt u meer informatie over de vooraf geconfigureerde oplossingen in IoT-Suite, raadpleegt u [Wat zijn de IoT Azure vooraf geconfigureerde oplossingen?][lnk-what-are-preconfig]

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
