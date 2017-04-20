<properties
 pageTitle="Azure IoT vooraf geconfigureerde oplossingen | Microsoft Azure"
 description="Een beschrijving van de Azure IoT vooraf geconfigureerde oplossingen en hun architectuur met koppelingen naar aanvullende bronnen."
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

# <a name="what-are-the-azure-iot-suite-preconfigured-solutions"></a>Wat zijn de oplossingen Azure IoT Suite vooraf geconfigureerde?

De oplossingen Azure IoT Suite vooraf geconfigureerde zijn implementaties van algemene IoT oplossing patronen die u kunt implementeren naar Azure uw abonnement te gebruiken. U kunt de vooraf geconfigureerde oplossingen gebruiken:

- Als uitgangspunt voor uw eigen IoT-oplossingen.
- Voor meer informatie over algemene patronen in het ontwerp van de oplossing IoT en ontwikkeling.

Elke vooraf geconfigureerde oplossing is een volledige, end-to-end-implementatie waarbij gesimuleerd apparaten wordt gebruikt om te genereren telemetrielogboek.

Naast het implementeren en uitvoeren van de oplossingen in Azure wordt aangegeven, kunt u de volledige broncode downloaden en vervolgens aanpassen en de oplossing om te voldoen aan uw specifieke vereisten voor IoT uitbreiden.

> [AZURE.NOTE] Als u wilt implementeren op een van de vooraf geconfigureerde oplossingen, gaat u naar [Microsoft Azure IoT Suite][lnk-azureiotsuite]. Het artikel [aan de slag met de oplossingen IoT vooraf geconfigureerde] [ lnk-getstarted-preconfigured] vindt u meer informatie over het implementeren en voer een van de oplossingen.

De volgende tabel ziet u hoe de oplossingen toewijzen aan specifieke IoT functies:

| Oplossing | Opname van gegevens | Apparaat-identiteit | Besturingselement voor opdrachten en parameters | Regels en acties | Bekijk analyses |
|------------------------|-----|-----|-----|-----|-----|
| [Externe controle][lnk-getstarted-preconfigured] | Ja | Ja | Ja | Ja | -   |
| [Bekijk onderhoud][lnk-predictive-maintenance] | Ja | Ja | Ja | Ja | Ja |

- *Opname van gegevens*: Ingress van gegevens bij het op schaal in de cloud.
- *Apparaat-id*: unieke identiteiten van elke verbonden apparaat beheren.
- *Besturingselement voor opdrachten en parameters*: berichten verzenden naar een apparaat vanuit de cloud oorzaak van het apparaat dat sommige actie te ondernemen.
- *Regels en acties*: de oplossing back-end gebruikt regels om te reageren op specifieke gegevens van het apparaat naar cloud.
- *Bekijk gebruiksanalyses*: de oplossing back-end is van toepassing analyseert apparaat naar cloud gegevens om te voorspellen wanneer bepaalde acties moeten plaatsvinden. Bijvoorbeeld het vliegtuig engine telemetrielogboek om te bepalen wanneer onderhoud engine vervalt analyseren.

## <a name="remote-monitoring-preconfigured-solution-overview"></a>Overzicht van externe controle vooraf geconfigureerde oplossingen

We hebt gekozen voor de externe controle vooraf geconfigureerde oplossing in dit artikel bespreken omdat deze ziet u verschillende veelgebruikte ontwerpelementen die delen van de andere oplossingen.

In het volgende diagram ziet u de belangrijkste elementen van de externe controle oplossing. De onderstaande secties bevatten meer informatie over deze elementen.

![Externe controle vooraf geconfigureerde oplossingsarchitectuur][img-remote-monitoring-arch]

## <a name="devices"></a>Apparaten

Wanneer u de externe controle vooraf geconfigureerde oplossing implementeert, zijn vier gesimuleerd apparaten vooraf ingerichte in de oplossing aan die een koelingsapparaat simuleren. Deze gesimuleerd apparaten hebben een ingebouwde temperatuur en vochtigheid model dat telemetrielogboek genereert. Deze gesimuleerd apparaten zijn opgenomen om te laten zien de stroom end-to-end van gegevens via de oplossing en op te geven van een handige bron van telemetrielogboek en een doel voor opdrachten als u een back-enddatabase ontwikkelaar met behulp van de oplossing als uitgangspunt voor een aangepaste implementatie.

Wanneer een apparaat is eerst verbinding met IoT Hub in de externe controle vooraf geconfigureerde oplossing maakt, wordt de lijst met opdrachten die het apparaat kan reageren op door het apparaat informatiebericht verzonden naar de hub IoT opgesomd. In de externe controle vooraf geconfigureerde oplossing zijn de opdrachten: 

- *Ping apparaat*: het apparaat moet reageren op deze opdracht met een bevestiging. Dit is handig om te controleren of het apparaat dat nog steeds actieve en luistert is.
- *Start Telemetrielogboek*: Hiermee geeft u het apparaat om te beginnen met het verzenden van telemetrielogboek.
- *Telemetrielogboek stoppen*: Hiermee geeft u het apparaat om te stoppen met het verzenden van telemetrielogboek.
- *Wijziging Set punt temperatuur*: besturingselementen voor de gesimuleerd temperatuur telemetrielogboek waarden het apparaat wordt verzonden. Dit is handig voor het testen van back-enddatabase logica.
- *Diagnostische Telemetrielogboek*: besturingselementen als het apparaat dat de externe temperatuur moet worden verzonden als telemetrielogboek.
- *Status van het apparaat wijzigen*.: Hiermee stelt u de eigenschap apparaat state metagegevens die het apparaat-rapporten. Dit is handig voor het testen van back-enddatabase logica.

U kunt meer gesimuleerd apparaten toevoegen aan de oplossing aan die de dezelfde telemetrielogboek verzenden en beantwoorden dezelfde opdrachten. 

## <a name="iot-hub"></a>IoT Hub

In deze vooraf geconfigureerde oplossing het exemplaar IoT Hub komt overeen met de *Cloud Gateway* in een normale [IoT oplossingsarchitectuur][lnk-what-is-azure-iot].

Een hub IoT ontvangt telemetrielogboek van de apparaten op een enkelvoudig eindpunt. Een hub IoT onderhoudt ook apparaat specifieke eindpunten zodat de opdrachten die zijn verzonden naar deze op elke apparaten kunnen ophalen.

De hub IoT kunt u de ontvangen telemetrielogboek beschikbaar via de service aan de clientzijde telemetrielogboek eindpunt lezen.

## <a name="azure-stream-analytics"></a>Azure Stream Analytics

De vooraf geconfigureerde oplossing maakt gebruik van drie [Azure Stream Analytics] [ lnk-asa] (ASA) taken voor het filteren van de stream telemetrielogboek vanaf de apparaten:


- *DeviceInfo taak* - voert gegevens op een gebeurtenis-hub waarmee apparaat registratie specifieke berichten, wanneer een apparaat voor het eerst verbinding maakt of in antwoord op een opdracht **wijzigen Apparaatstatus** verzonden naar de oplossing apparaat register (een DocumentDB-database). 
- *Telemetrielogboek taak* - alle onbewerkte telemetrielogboek verzendt naar Azure-blobopslag voor verkoudheid opslag en berekent telemetrielogboek aggregaties die worden weergegeven in het dashboard oplossing.
- *Regels taak* - filters de stream telemetrielogboek voor waarden die groter zijn dan een regel drempelwaarden en voert de gegevens op een hub voor de gebeurtenis. Wanneer een regel wordt gestart, wordt de weergave van de portal dashboard oplossing geeft deze gebeurtenis als een nieuwe rij in de waarschuwing geschiedenistabel en een actie op basis van de instellingen die zijn gedefinieerd in de regels en acties weergaven in de portal oplossing activeert.

In deze vooraf geconfigureerde oplossing, de taken ASA deel uitmaken van de **back-end IoT oplossing** in een normale [IoT oplossingsarchitectuur][lnk-what-is-azure-iot].

## <a name="event-processor"></a>Gebeurtenis-processor

In deze vooraf geconfigureerde oplossing, de gebeurtenis processor deel uitmaakt van de **back-end IoT oplossing** in een normale [IoT oplossingsarchitectuur][lnk-what-is-azure-iot].

De taken **DeviceInfo** en **regels** ASA stuurt hun uitvoer naar gebeurtenis hubs voor de bezorging van andere back-end-services. De oplossing maakt gebruik van een [EventPocessorHost] [ lnk-event-processor] exemplaar, dat wordt uitgevoerd in een [WebJob][lnk-web-job], de berichten lezen van deze gebeurtenis hubs. De **EventProcessorHost** de **DeviceInfo** gegevens gebruikt om te werken van de apparaatgegevens in de database DocumentDB en gebruikt de gegevens van de **regels** aan te roepen de logica-app en de meldingen in de portal van de oplossing weergeven bijwerken.

## <a name="device-identity-registry-and-documentdb"></a>Apparaat identiteit register en DocumentDB

Elke hub IoT bevat een [apparaat identiteit register] [ lnk-identity-registry] die apparaat toetsen worden opgeslagen. IoT Hub gebruikt deze gegevens apparaten verifiëren: een apparaat moet worden geregistreerd en u hebt een geldig sleutel voordat deze verbinding met de hub maken kunt.

Deze oplossing wordt opgeslagen als u meer informatie over apparaten zoals hun status, de opdrachten die worden ondersteund en andere metagegevens. De oplossing beschikt over een DocumentDB-database voor de opslag van deze apparaatgegevens oplossing / regiospecifieke en de oplossing-portal haalt gegevens op vanuit deze database DocumentDB voor het weergeven en bewerken.

De oplossing moet ook de informatie in het apparaat identiteit register gesynchroniseerd met de inhoud van de database DocumentDB behouden. De **EventProcessorHost** gebruikt de gegevens uit **DeviceInfo** stream analytics taak voor het beheren van de synchronisatie.

## <a name="solution-portal"></a>Oplossing-portal

![Oplossing dashboard][img-dashboard]

De oplossing-portal is een web gebaseerde gebruikersinterface die wordt geïmplementeerd in de cloud als onderdeel van de vooraf geconfigureerde oplossing. U kunt:

- Telemetrielogboek en waarschuwing geschiedenis weergeven in een dashboard.
- Nieuwe apparaten inrichten.
- Beheren en controleren van apparaten.
- Opdrachten sturen naar bepaalde apparaten.
- Regels en acties beheren.

In deze vooraf geconfigureerde oplossing, de portal oplossing formulieren deel van de **oplossing IoT back-end** en een deel van de **verwerking en business connectivity** in de normale [IoT oplossingsarchitectuur][lnk-what-is-azure-iot].

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over IoT oplossing architecturen, [services van Microsoft Azure IoT: verwijzing architectuur][lnk-refarch].

Nu u wat een vooraf geconfigureerde oplossing weet is, u kunt aan de slag met de *externe controle* vooraf geconfigureerde-oplossing implementeert: [aan de slag met de vooraf geconfigureerde oplossingen][lnk-getstarted-preconfigured].

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-getstarted-preconfigured]: iot-suite-getstarted-preconfigured-solutions.md