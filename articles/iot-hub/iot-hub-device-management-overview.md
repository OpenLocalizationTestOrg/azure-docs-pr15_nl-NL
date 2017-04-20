<properties
 pageTitle="Overzicht van IoT Hub voor het beheer van apparaat | Microsoft Azure"
 description="In dit artikel bevat een overzicht van Apparaatbeheer in Azure IoT Hub: levenscyclus van de enterprise-apparaat, opnieuw opstarten, factory opnieuw instellen, firmware-update, configuratie, apparaat twins, query's, taken"
 services="iot-hub"
 documentationCenter=""
 authors="bzurcher"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/03/2016"
 ms.author="bzurcher"/>

# <a name="overview-of-azure-iot-hub-device-management-preview"></a>Overzicht van Azure IoT Hub Apparaatbeheer (preview)

## <a name="introduction"></a>Inleiding

Azure IoT Hub vindt u de functies en een uitbreiding model waarmee apparaat en back-enddatabase ontwikkelaars om krachtige IoT apparaat managementoplossingen te maken. IoT apparaten bereik van beperkte sensoren en één doel microcontrollers, krachtige gateways die communicatie voor groepen van apparaten routeren.  Bovendien verschillen de gebruik aanvragen en de vereisten voor IoT operatoren aanzienlijk over industrie.  Ondanks deze variatie biedt Azure IoT Hub Apparaatbeheer de mogelijkheden, patronen en codebibliotheken naar geschikt zijn voor een uitgebreide reeks apparaten en eindgebruikers.

Een essentieel deel van het maken van een succesvolle enterprise IoT oplossing te voorzien in een strategie hoe operatoren, het beheer van de siteverzameling van apparaten afhandelen. IoT operatoren vereisen eenvoudige en betrouwbare hulpprogramma's en toepassingen waarmee ze concentreren op de strategische aspecten van hun taken. In dit artikel bevat:

- Een beknopt overzicht van Azure IoT Hub aanpak Apparaatbeheer.
- Een beschrijving van de gemeenschappelijke apparaat management beginselen.
- Een beschrijving van de levenscyclus van het apparaat.
- Een overzicht van algemene apparaat management patronen.

## <a name="iot-device-management-principles"></a>IoT apparaat management beginselen

IoT brengt met deze een unieke set apparaat management uitdagingen en elke oplossing enterprise-klasse moet adres van de volgende beginselen:

![Azure IoT Hub apparaat management beginselen-afbeelding][img-dm_principles]

- **Schaal en automatisering**: IoT oplossingen vereisen eenvoudige hulpprogramma's waarmee u kunnen steeds terugkerende taken kunt automatiseren en inschakelen van een relatief kleine beheerders voor het beheren van miljoenen apparaten. Dagelijkse, operatoren verwachten bewerkingen apparaat op afstand, bulksgewijs en alleen worden gewaarschuwd als zich problemen voordoen die hun directe aandacht vereist.

- **Toegankelijkheid en compatibiliteit**: het IoT apparaat-systeem is uitzonderlijk diverse. Hulpmiddelen voor projectbeheer moeten worden aangepast zodat een groot aantal apparaatklassen, platforms en protocollen. Operatoren moet kunnen veel soorten apparaten, van de meest beperkte chips voor ingesloten enkel-proces, krachtige en volledig functioneel computers ondersteunen.

- **Context chatstatus**: IoT omgevingen zijn dynamische en voortdurend. Betrouwbaarheid van de service is grootste. Apparaat management bewerkingen nog SLA onderhoud windows, netwerk- en power Staten in gebruik voorwaarden en geolocatie apparaat om ervoor te zorgen dat downtime onderhoud niet van invloed zijn op kritieke bedrijfsactiviteiten of schadelijke voorwaarden maken.

- **Service-veel rollen**: ondersteuning voor de unieke werkstromen en processen van IoT bewerkingen rollen zijn essentieel. Beheerders moeten eenheid in de omschrijving werken met de opgegeven beperkingen van interne IT-afdelingen.  Duurzame manieren surface realtime apparaat bewerkingen informatie om te toezichthouders en andere zakelijke leidinggevende rollen, moeten ze ook vinden.

## <a name="iot-device-lifecycle"></a>Levenscyclus van IoT-apparaat

Er is een reeks algemene apparaat management fasen die voor alle IoT ondernemingsprojecten gelden. In Azure IoT zijn er vijf fasen binnen de levenscyclus van het apparaat IoT:

![De vijf Azure IoT apparaat levenscyclus fasen: plannen, inrichten, configureren, controleren, intrekken][img-device_lifecycle]

Er zijn verschillende vereisten van het apparaat operator die moeten worden voldaan om te bieden geen volledige oplossing in elk van deze vijf stadia:

- **Plannen**: operatoren om te maken van een apparaat metagegevens kleurenschema kunnen ze gemakkelijk en nauwkeurig naar zoeken en een doelgroep van apparaten voor management bulkbewerkingen inschakelen. U kunt de dubbele apparaat gebruiken voor het opslaan van deze Apparaatmetagegevens in de vorm van labels en eigenschappen.

    *Aanbevolen artikelen*: [aan de slag met apparaat twins][lnk-twins-getstarted], [informatie over apparaat twins][lnk-twins-devguide], [hoe dubbele eigenschappen kunt gebruiken][lnk-twin-properties]

- **Voorziening**: veilig inrichten van nieuwe apparaten op IoT Hub en operatoren om te ontdekken onmiddellijk apparaat mogelijkheden inschakelen.  Het register IoT Hub-apparaat gebruiken om flexibele apparaat identiteiten en referenties maken en uitvoeren van deze bewerking bulksgewijs met behulp van een taak. Apparaten om het rapporteren van de mogelijkheden en voorwaarden tot en met apparaateigenschappen in de dubbele apparaat maken.

    *Aanbevolen artikelen*: [beheren apparaat identiteiten][lnk-identity-registry], [bulksgewijs beheer van apparaat identiteiten][lnk-bulk-identity], [hoe dubbele eigenschappen kunt gebruiken][lnk-twin-properties]

- **Configureren**: bulksgewijs configuratiewijzigingen en firmware updates voor apparaten vergemakkelijken zowel gezondheid en beveiliging. Met behulp van de gewenste eigenschappen of met de rechtstreekse methoden en uitzenden taken, moet u deze apparaat management bewerkingen uitvoeren bulksgewijs.

    *Verder lezen*: [directe methoden gebruiken][lnk-c2d-methods], [roep een directe methode op een apparaat][lnk-methods-devguide], [het gebruik van dubbele eigenschappen][lnk-twin-properties], [planning en uitzenden taken][lnk-jobs], [taken plannen op meerdere apparaten][lnk-jobs-devguide]

- **Monitor**: algemene apparaat siteverzameling toestand, de status van lopende bewerkingen uitvoeren, kunt controleren en operatoren voor problemen die mogelijk hun aandacht zien.  De twee apparaat als apparaten rapport realtime operationele voorwaarden en status van de updatebewerkingen toepassen. Krachtige dashboardrapporten die het oppervlak van de meest directe problemen met behulp van apparaat dubbele query's maken.

    *Aanbevolen artikelen*: [het gebruik van dubbele eigenschappen][lnk-twin-properties], [querytaal voor twins en taken][lnk-query-language]

- **Intrekken**: vervangen of schaft apparaten na een fout, upgrade cyclus, of aan het einde van de levensduur van de service.  Gebruiken om de dubbele apparaat te onderhouden apparaat info als het apparaat wordt vervangen of gearchiveerd als worden teruggetrokken. Het register IoT Hub-apparaat gebruiken voor het veilig intrekken apparaat identiteiten en referenties.

    *Aanbevolen artikelen*: [het gebruik van dubbele eigenschappen][lnk-twin-properties], [apparaat-identiteiten beheren][lnk-identity-registry]

## <a name="iot-hub-device-management-patterns"></a>IoT Hub apparaat management patronen

IoT Hub kunt de volgende set apparaat management patronen.  Het [apparaat management zelfstudies] [ lnk-get-started] u uitgebreider zien hoe u deze patronen aanpassen aan uw exacte scenario uitbreidt en hoe bij het ontwerpen van nieuwe patronen op basis van deze core-sjablonen.

- **Opnieuw opstarten** - de back-enddatabase-toepassing wordt geïnformeerd het apparaat via een directe methode dat deze opnieuw heeft gestart.  Het apparaat wordt gebruikt voor het apparaat dubbele gerapporteerd eigenschappen die u kunt de status opnieuw opstarten van het apparaat bijwerken.

    ![Azure IoT Hub Apparaatbeheer patroon afbeelding opnieuw opstarten][img-reboot_pattern]

- **Opnieuw factory** - de back-enddatabase-toepassing wordt geïnformeerd het apparaat via een directe methode dat deze opnieuw instellen van een fabriek heeft gestart.  Het apparaat gebruikt de dubbele apparaat gerapporteerde eigenschappen die u kunt het bijwerken van de fabriek status van het apparaat opnieuw instellen.

    ![Azure IoT Hub apparaat management factory herstellen patroon afbeelding][img-facreset_pattern]

- **Configuratie** - de back-enddatabase-toepassing worden de eigenschappen van de twee gewenst apparaat configureren voor software die op het apparaat uitgevoerd.  Het apparaat wordt gebruikt voor het apparaat dubbele gerapporteerd eigenschappen die u kunt de configuratiestatus van het apparaat bijwerken.

    ![Azure IoT Hub apparaat management configuratie patroon afbeelding][img-config_pattern]

- **Firmware bijwerken** - de back-enddatabase toepassing wordt geïnformeerd het apparaat via een directe methode dat dit een firmware-update heeft gestart.  Het apparaat start een dergelijk complex proces om de firmware downloaden, het pakket firmware toepassen en ten slotte opnieuw verbinding maken met de IoT Hub-service.  Overal in het proces mult-stap, het apparaat gebruikmaakt van het apparaat dubbele gerapporteerd eigenschappen die u kunt de voortgang bijwerken en de status van het apparaat.

    ![Azure IoT Hub apparaat management firmware patroon afbeelding bijwerken][img-fwupdate_pattern]

- **Voortgang en status rapportage** - toepassing back-end apparaat dubbele query's is uitgevoerd voor een reeks apparaten, rapporteren van de status en voortgang van acties uitvoeren op de apparaten.

    ![Azure IoT Hub Apparaatbeheer rapportage voortgang en status patroon-afbeelding][img-report_progress_pattern]

## <a name="next-steps"></a>Volgende stappen

U kunt de mogelijkheden, patronen en codebibliotheken waarmee Azure IoT Hub Apparaatbeheer, u kunt IoT toepassingen die voldoen aan de vereisten enterprise IoT operator binnen in elk niveau van de levenscyclus van apparaat maken.

Als u wilt blijven leren over de beheerfuncties van de Azure IoT Hub-apparaat, raadpleegt u de [aan de slag met Apparaatbeheer Azure IoT Hub] [ lnk-get-started] zelfstudie.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md