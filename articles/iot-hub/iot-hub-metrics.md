<properties
 pageTitle="De diagnostische doelstellingen IoT Hub"
 description="Een overzicht van Azure IoT Hub aan de doelstellingen, zodat gebruikers kunnen de algemene status van de resource beoordelen"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-diagnostic-metrics"></a>Inleiding tot diagnostische gegevens aan de doelstellingen

Diagnostische gegevens aan de doelstellingen, krijgt u betere gegevens over de status van de Azure resources in uw Azure-abonnement. Aan de doelstellingen kunt u de algemene status van de service en de apparaten die zijn verbonden met het beoordelen. Gebruikerspagina statistieken zijn belangrijk omdat ze u zien wat er gebeurt met uw IoT hub help onderliggende oorzaak problemen met en kunnen zonder dat u contact kunt opnemen met Azure ondersteuning nodig hebt.

Diagnostische gegevens aan de doelstellingen van de Azure-portal, kunt u ook weer inschakelen.

## <a name="how-to-enable-diagnostic-metrics"></a>Het inschakelen van diagnostische gegevens aan de doelstellingen

1. Maak een hub IoT. U vindt instructies voor het maken van een hub IoT in de [Aan de slag] [ lnk-get-started] handleiding.

2. Open het blad van uw hub IoT. Klik op **diagnose**daar.

    ![][1]

3. Uw diagnostische hulpprogramma's configureren door de status is ingesteld op **op** en een account Azure-opslag voor de opslag van de gegevens diagnostische gegevens te selecteren. Controleer **de doelstellingen**en druk vervolgens op **Opslaan**. Houd er rekening mee dat het account Azure Storage tijd vooraf moet worden gemaakt en dat u in rekening worden gebracht afzonderlijk voor opslag. U kunt er ook voor kiezen uw gegevens diagnostische gegevens te verzenden naar een gebeurtenis Hubs-eindpunt.

    ![][2]

4. Nadat u de diagnostische hulpprogramma's hebt ingesteld, kunt u terug naar het **Overzicht** IoT hub blad. Aan de doelstellingen informatie is in de sectie **controle** van het blad gevuld. Het deelvenster aan de doelstellingen waar u kunt een overzicht van de gegevens aan de doelstellingen voor uw hub IoT weergeven en bewerken van de selectie van de doelstellingen weergegeven in de grafiek te klikken op de grafiek worden geopend. U kunt ook op basis van metrieke waarden waarschuwingen configureren.

    ![][3]

## <a name="metrics-and-how-to-use-them"></a>Aan de doelstellingen en hoe u ze kunt gebruiken

IoT Hub bevat verschillende maatstaven voor een overzicht gegeven van de status van uw hub en het totale aantal apparaten zijn aangesloten. U kunt gegevens uit meerdere maatstelsel tekenen van een afbeelding groter te maken van de status van de hub IoT combineren. De volgende tabel worden de maatstaven die elke hub IoT wordt bijgehouden en hoe elke metrisch zich verhoudt tot de algemene status van de hub IoT.

| Metrisch | Metrische beschrijving | Waar de meetwaarde voor wordt gebruikt |
| ---- | ---- | ---- |
| d2c.telemetry.ingress.allProtocol | Het aantal berichten op alle apparaten | Overzicht van gegevens op bericht verzendt |
| d2c.telemetry.ingress.Success | Het aantal van alle succesvolle berichten in de hub | Overzicht van het bericht is voltooid ingress in de hub |
| c2d.Commands.egress.complete.Success | Het aantal van alle opdracht berichten uitgevoerd door het apparaat dat ontvangen op alle apparaten | Samen met de doelstellingen op Oefening en negeren, kunt een overzicht van de algehele C2D opdracht success rente |
| c2d.Commands.egress.Abandon.Success | Het aantal van alle berichten met succes afgebroken door het apparaat dat ontvangen op alle apparaten | Potentiële problemen markeert als berichten worden vaker dan verwacht ophalen afgebroken. |
| c2d.Commands.egress.Reject.Success | Het aantal van alle berichten is geweigerd door het apparaat dat ontvangen op alle apparaten | Potentiële problemen markeert als berichten zijn afgewezen vaker dan verwacht |
| devices.totalDevices | Het gemiddelde, min en max van het aantal apparaten die zijn geregistreerd voor de hub IoT | Het aantal apparaten die zijn geregistreerd voor de hub |
| devices.connectedDevices.allProtocol | Het gemiddelde, min en max van het aantal gelijktijdige verbonden apparaten | Overzicht van het aantal apparaten die zijn verbonden met de hub |

## <a name="next-steps"></a>Volgende stappen

U kunt een overzicht van de doelstellingen van de diagnostische hebt gezien, volgt u deze koppeling voor meer informatie over het beheren van Azure IoT Hub:

- [Bewerkingen bewaken][lnk-monitor]

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK van de Gateway IoT simuleren][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-metrics/enable-metrics-1.png
[2]: media/iot-hub-metrics/enable-metrics-2.png
[3]: media/iot-hub-metrics/enable-metrics-3.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-operations-monitoring]: iot-hub-operations-monitoring.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
