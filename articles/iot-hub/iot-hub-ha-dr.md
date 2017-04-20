<properties
 pageTitle="IoT Hub HA en DR | Microsoft Azure"
 description="Beschrijving van functies die kunnen maximaal beschikbare IoT oplossingen bouwen met noodgevallen herstelmogelijkheden."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="02/03/2016"
 ms.author="elioda"/>

# <a name="iot-hub-high-availability-and-disaster-recovery"></a>IoT Hub hoge beschikbaarheid herstel

Als een Azure-service biedt IoT Hub beschikbaarheid (HA) met redundanties op het regioniveau van de Azure, zonder extra werk vereist door de oplossing. Daarnaast biedt Azure een aantal functies die bij het maken van oplossingen met mogelijkheden voor noodgevallen herstel (DR) of cross-regio beschikbaarheid indien nodig. U moet ontwerpen en het voorbereiden van uw oplossingen om te profiteren van deze DR-functies als u de globale wilt opgeven, de beschikbaarheid van cross-regio voor apparaten of gebruikers. Het artikel [Azure bedrijven bedrijfscontinuïteit technische richtlijnen](../resiliency/resiliency-technical-guidance.md) beschrijving van de ingebouwde functies in Azure wordt aangegeven voor bedrijfscontinuïteit en DR. Het papier [herstel en beschikbaarheid van Azure-toepassingen][] biedt architectuur richtlijnen aan strategieën voor het Azure-toepassingen om HA en DR te bereiken.

## <a name="azure-iot-hub-dr"></a>Azure IoT Hub DR
Naast het binnen-regio HA implementeert IoT Hub failover regelingen voor herstel waarvoor geen tussenkomst van de gebruiker. IoT Hub DR zelf wordt gestart en heeft een herstel tijd doelstelling (RTO) van 2-26 uren en de volgende herstel punt doelstellingen (productoutput).

| Functionaliteit | VRIJGEGEVEN PRODUCTIEORDER |
| ------------- | --- |
| Beschikbare service voor register en communicatie bewerkingen | Mogelijke CName verlies |
| Id-gegevens in apparaat identiteit register | 0 tot en met 5 minuten gegevensverlies |
| Apparaat-naar-cloud-berichten | Alle ongelezen berichten gaan verloren |
| Bewerkingen berichten controleren | Alle ongelezen berichten gaan verloren |
| Berichten van de cloud-naar-apparaat | 0 tot en met 5 minuten gegevensverlies |
| Cloud-to-device feedback wachtrij | Alle ongelezen berichten gaan verloren |

## <a name="regional-failover-with-iot-hub"></a>Regionale failover met IoT Hub

Een volledige behandeling van implementatietopologieën in IoT oplossingen zich buiten de reikwijdte van dit artikel, maar voor het beschikbaarheid en herstel zullen we het model van de implementatie *regionale failover* overwegen.

In een model regionale failover de oplossing back-end hoofdzakelijk op één locatie van het datacenter wordt uitgevoerd, maar een extra IoT hub en back-end zijn geïmplementeerd in een andere datacenter van de locatie voor failover doeleinden, geval de hub IoT in het primaire datacenter een storing ondergaat of de netwerkverbinding van het apparaat naar het primaire datacenter wordt is onderbroken. Apparaten gebruiken een secundaire service-eindpunt zodra de primaire gateway kan niet worden bereikt. Met een cross-regio uitvalbeveiligingsmogelijkheden, kan de beschikbaarheid van de oplossing voorbij de beschikbaarheid van één regio worden verbeterd.

Op hoog niveau, implementatie van een model regionale failover met IoT-Hub, moet u het volgende.

* **Een secundaire IoT hub en een apparaat mailroutering logica**: in het geval van een service-onderbreking in uw primaire regio, apparaten moeten beginnen met het verbinding maken met uw secundaire regio. De staat-hoogte aard van de meeste services betrokken gegeven, is het algemene voor beheerders van de oplossing voor het starten van het failoverproces tussen regio. De beste manier om te communiceren van het nieuwe endpoint naar apparaten, terwijl de besturing van het proces, is deze regelmatig controleren een *concierge* -service voor het huidige actieve eindpunt zijn. De service concierge kan een eenvoudige webtoepassing dat wordt gerepliceerd en bewaard bereikbaar zijn met DNS-omleiding technieken (bijvoorbeeld met [Azure verkeer Manager][]).
* **Identiteit register replicatie** - om te kunnen worden gebruikt, de secundaire IoT hub alle apparaat identiteiten die kunnen worden verbonden met de oplossing moet bevatten. De oplossing moet houden geografische gerepliceerd back-ups van apparaat identiteiten en uploadt u ze naar de secundaire IoT hub voor wisselen van het actieve eindpunt voor de apparaten. Het apparaat identiteit exporteren functionaliteit van de IoT Hub is handig in deze context. Zie [IoT Hub Developer Guide - identiteit register][]voor meer informatie.
* **Logica samenvoegen** - wanneer de primaire regio weer beschikbaar, alle de provincie en de gegevens die zijn gemaakt in de secundaire site moeten worden gemigreerd terug naar de primaire regio. Dit is voornamelijk heeft betrekking op apparaat identiteiten en toepassing metagegevens, die moeten worden samengevoegd met de primaire IoT hub en eventuele andere toepassingsspecifieke winkels in de primaire regio. Om te vereenvoudigen deze stap, is het meestal aanbevolen idempotency is ingeschakeld bewerkingen te gebruiken. Hiermee wordt geminimaliseerd bijwerkingen niet alleen van eventuele consistente distributie van gebeurtenissen, maar ook van dubbele waarden of out volgorde aflevering van gebeurtenissen. Daarnaast moet de toepassingslogica zijn ontworpen voor gemiddeld mogelijke inconsistenties of 'enigszins' afmelden bij de datum statuswaarden. Dit is vanwege de extra tijd die het duurt voordat het systeem "retoucheren" op basis van herstel punt doelstellingen (vrijgegeven Productieorder).

## <a name="next-steps"></a>Volgende stappen

Klik op deze koppelingen voor meer informatie over Azure IoT Hub:

- [Aan de slag met IoT Hubs (zelfstudie)][lnk-get-started]
- [Wat is Azure IoT Hub?][]

[Herstel en beschikbaarheid van Azure-toepassingen]: ../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md
[Azure Business Continuity Technical Guidance]: https://azure.microsoft.com/documentation/articles/resiliency-technical-guidance/
[Azure verkeer Manager]: https://azure.microsoft.com/documentation/services/traffic-manager/
[IoT Hub-handleiding voor ontwikkelaars - identiteit register]: iot-hub-devguide-identity-registry.md

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[Wat is Azure IoT Hub?]: iot-hub-what-is-iot-hub.md
