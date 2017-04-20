<properties
 pageTitle="Overzicht van Azure IoT Hub | Microsoft Azure"
 description="Overzicht van Azure IoT Hub-service: Wat is iot-hub, apparaat connectivity, internet van zaken communicatiepatronen en service ondersteunde communicatiepatroon"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/25/2016"
 ms.author="dobett"/>

# <a name="what-is-azure-iot-hub"></a>Wat is Azure IoT Hub?

Welkom bij Azure IoT Hub. In dit artikel bevat een overzicht van Azure IoT Hub en wordt beschreven waarom u deze service moet gebruiken om een oplossing Internet van dingen (IoT) te implementeren. Azure IoT Hub heeft een volledig beheerde service waarmee betrouwbare en veilige bidirectionele communicatie tussen miljoenen IoT apparaten en oplossing back-enddatabase. Azure IoT Hub:

- Biedt betrouwbare apparaat naar cloud en cloud-to-device messaging bij het op schaal.
- Hiermee activeert beveiligde communicatie met beveiligingsreferenties per apparaat en toegangsbeheer.
- Biedt uitgebreide controle voor apparaat-connectiviteit en -apparaat identiteit management gebeurtenissen.
- Bevat apparaatbibliotheken voor de populairste talen en platforms.

Het artikel [vergelijking van IoT Hub en gebeurtenis Hubs] [ lnk-compare] worden de belangrijkste verschillen tussen deze twee services beschreven en worden de voordelen van het gebruik van de IoT Hub in IoT oplossingen gemarkeerd.

![Azure IoT Hub als cloud gateway in internet van zaken oplossing][img-architecture]

> [AZURE.NOTE] Zie voor een gedetailleerde beschrijving van IoT architectuur, [Microsoft Azure IoT verwijzing architectuur][lnk-refarch].

## <a name="iot-device-connectivity-challenges"></a>IoT apparaat-connectiviteit uitdagingen

IoT Hub en de apparaatbibliotheken kunnen u om te voldoen aan de uitdagingen van de manier waarop naar betrouwbaar en veilig apparaten aansluiten op de oplossing back-end. IoT apparaten:

- Zijn vaak ingesloten-systemen met geen menselijke operator.
- Externe locaties, waar fysiek toegang dure is mogelijk niet.
- Alleen mogelijk bereikbaar via de oplossing back-end.
- Mogelijk hebt beperkt power en verwerking van resources.
- Wellicht door onregelmatige, traag of dure netwerkconnectiviteit.
- Mogelijk moet u, bedrijfseigen, aangepaste of gespecialiseerde toepassingsprotocollen gebruiken.
- U kunt maken met een groot aantal populaire hardware en software-platforms.

Naast de bovenstaande voorwaarden, moet elke oplossing IoT ook geven schaal, beveiliging en betrouwbaarheid. De resulterende set vereisten voor connectiviteit is moeilijk en tijdrovende willen implementeren wanneer u traditionele technologieën, zoals web containers en SMS makelaars gebruikt.

## <a name="why-use-azure-iot-hub"></a>Het nut van Azure IoT Hub

Azure IoT Hub-adressen voor de uitdagingen apparaat-connectiviteit in de volgende manieren:

-   **Per apparaat verificatie en beveiligde verbindingen**. U kunt elk apparaat met een eigen [beveiliging toets] inrichten[ lnk-devguide-security] om het te verbinden met IoT Hub. Het [IoT Hub identiteit register] [ lnk-devguide-identityregistry] apparaat identiteiten en toetsen worden opgeslagen in een oplossing. Oplossing back-enddatabase kunt afzonderlijke apparaten wilt toestaan of weigeren van lijsten om in te schakelen van de volledige controle over toegang tot apparaten toevoegen.

-   **Controle van apparaat connectivity bewerkingen**. U kunt gedetailleerde bewerking logboeken over apparaat identiteit management bewerkingen en apparaat connectivity gebeurtenissen ontvangen. Deze controle mogelijkheid staat uw oplossing IoT identificeren verbindingsproblemen, zoals apparaten die proberen om te communiceren met de verkeerde referenties te vaak berichten verzenden of afwijzen van alle berichten van de cloud-naar-apparaat.

-   **Een uitgebreide reeks apparaatbibliotheken**. [Azure IoT apparaat SDK's] [ lnk-device-sdks] zijn alleen beschikbaar en ondersteunde voor verschillende talen en platforms--C voor veel Linux onderzoeken, Windows en realtime-besturingssystemen. Azure IoT apparaat SDK's ondersteunen ook beheerde talen, zoals C#, Java en JavaScript.

-   **IoT protocollen en uitbreiden**. Als uw oplossing niet kan de apparaatbibliotheken, beschrijft IoT Hub een openbare protocol waarmee apparaten native gebruik van de MQTT v3.1.1, HTTP 1.1 of AMQP 1.0 protocollen. U kunt ook IoT Hub om te bieden ondersteuning voor aangepaste protocollen door uitbreiden:

    - Een gateway veld maken met de [Azure IoT Gateway SDK] [ lnk-gateway-sdk] dat uw aangepaste protocol converteert naar een van de drie protocollen begrepen door IoT Hub. 
    - Het aanpassen van de [Azure IoT protocol gateway][protocol-gateway], een onderdeel bron openen die wordt uitgevoerd in de cloud.

-   **Schaal**. Er wordt een Azure IoT Hub aangepast aan miljoenen tegelijk verbonden apparaten en miljoenen gebeurtenissen per seconde.

Deze voordelen zijn algemene veel communicatie patronen. IoT Hub kunt momenteel u de volgende specifieke communicatiepatronen implementeren:

-   **Op basis van een gebeurtenis apparaat naar cloud opname.** IoT Hub kunnen betrouwbaar miljoenen gebeurtenissen per seconde ontvangen van uw apparaten. Dit kan ze vervolgens verwerken op uw warm pad met behulp van een gebeurtenis processor-engine. Dit kan ze ook opslaan op uw koudwatersystemen pad voor analyse. IoT Hub behoudt de gegevens van de gebeurtenis voor maximaal zeven dagen naar betrouwbaar verwerking garanderen en naar absorbeert pieken in het selectievakje laden.

-   * *Betrouwbaar cloud-to-device messaging (of *opdrachten*). ** de oplossing back-end IoT Hub kunt gebruiken om berichten met een garantie bezorging van het kleinste-eens aan afzonderlijke apparaten. Elk bericht heeft een afzonderlijke time to live-instelling en de back-end kunt ook zelf aanvragen bezorging en de vervaldatum bevestigingen. Deze bevestigingen zorgen volledig inzicht in de levenscyclus van een bericht cloud-naar-apparaat. Vervolgens kunt u bedrijfslogica bewerkingen die worden uitgevoerd op apparaten met implementeren.

-   **Bestanden en in de cache sensorgegevens te uploaden naar de cloud.** Uw apparaten kunnen bestanden uploaden naar Azure opslagruimte met SA's URI's voor u wordt beheerd door IoT Hub. IoT Hub kunnen meldingen genereren tijdens bestanden komen terecht in de cloud om in te schakelen van de back-end ze te verwerken.

## <a name="gateways"></a>Gateways

Een gateway in een oplossing IoT is meestal een [protocol gateway] [ lnk-gateway] die wordt geïmplementeerd in de cloud of een [veld gateway] [ lnk-field-gateway] die lokaal wordt geïmplementeerd met uw apparaten. Een gateway protocol kunt protocol vertaling, bijvoorbeeld MQTT naar AMQP uitvoeren. Een veld gateway kunt analyses uitvoeren op de rand, beslissingen nemen tijdgevoelig latentie verkleinen, apparaat management services bieden, afdwingen van beveiliging en privacy-beperkingen en ook uitvoeren protocol vertaling. Beide typen gateway fungeren als schakels tussen uw apparaten en uw hub IoT.

Een gateway veld verschilt van een eenvoudige verkeer routeren apparaat (zoals een apparaat network address translation of de firewall) omdat dit meestal een actieve rol bij het beheren van access en gegevens-mailstroom in de oplossing wordt uitgevoerd.

Een oplossing kan zowel protocol als het veld gateways bevatten.

## <a name="how-does-iot-hub-work"></a>Hoe werkt IoT Hub?

Azure IoT Hub implementeert [service ondersteunde communicatie] [ lnk-service-assisted-pattern] patroon naar de interactie tussen uw apparaten en uw oplossing back-end staan. Het doel van service ondersteunde communicatie is tot stand brengen van betrouwbare, bidirectionele communicatiepaden tussen een besturingselement systeem, zoals IoT Hub en speciale apparaten die zijn geïmplementeerd in niet-vertrouwde fysieke ruimte. Het patroon tot stand brengt van de volgende beginselen:

- Beveiliging heeft voorrang op alle andere mogelijkheden.
- Apparaten accepteren ongewenste Netwerkgegevens niet. Een apparaat tot stand brengt alle verbindingen en routes op alleen uitgaande wijze. Voor een apparaat voor het ontvangen van een opdracht uit de back-end, moet het apparaat regelmatig een verbinding met het controleren op alle opdrachten in behandeling verwerkingstijd starten.
- Apparaten moeten alleen verbinding maken met of routes naar bekende services die ze zijn peered, zoals IoT Hub definiëren.
- Het communicatiepad tussen apparaat en service of tussen apparaat en de gateway is bij het protocol toepassingslaag beveiligd.
- Systeem niveau autorisatie en verificatie op basis van identiteiten per apparaat. Zij aanbrengen-referenties en machtigingen vrijwel direct terughalen.
- Bidirectionele communicatie voor apparaten die verbinding af vanwege power- of connectiviteitsproblemen bezwaren wordt mogelijk gemaakt door het vasthouden van opdrachten en apparaatmeldingen totdat een apparaat verbinding maakt als u wilt ontvangen. IoT Hub onderhoudt apparaat-specifieke wachtrijen voor de opdrachten die te sturen.
- Nettolading van toepassing is afzonderlijk beveiligd voor beveiligde overdracht via gateways naar een bepaalde service.

De mobiele bedrijfstak heeft het communicatiepatroon service ondersteunde gebruikt met enorm schaal willen implementeren push notification services zoals [Windows Push Notification Services][lnk-wns], [Google Cloud Messaging][lnk-google-messaging], en de [Apple Push Notification-Service][lnk-apple-push].

IoT Hub wordt ondersteund via ExpressRoute van openbare peering pad.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe Azure IoT Hub gestandaardiseerde IoT device management voor u extern beheren, configureren en bijwerken van uw apparaten kunt, raadpleegt u [overzicht van Azure IoT Hub Apparaatbeheer][lnk-device-management].

Als u wilt implementeren clienttoepassingen op tal van apparaat hardwareplatforms en verschillende besturingssystemen, kunt u het apparaat IoT SDK's. Het apparaat IoT SDK's bevatten bibliotheken die verzendende telemetrielogboek in een hub IoT en ontvangen van cloud-to-device opdrachten te vergemakkelijken. Wanneer u de SDK's gebruikt, kunt u kiezen uit verschillende netwerkprotocollen om te communiceren met IoT Hub. Meer informatie raadpleegt u de [informatie over apparaat SDK's][lnk-device-sdks].

Als u wilt beginnen sommige code schrijft en enkele voorbeelden uitvoeren, raadpleegt u de [aan de slag met IoT Hub] [ lnk-get-started] zelfstudie.

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png


[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Service bijstaan communicatie, blogbericht door Clemens Vasters"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-gateway]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-device-management]: iot-hub-device-management-overview.md
