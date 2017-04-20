<properties
     pageTitle="De Azure-portal gebruiken om te maken van een IoT Hub | Microsoft Azure"
     description="Een overzicht over het maken en beheren van Azure IoT hubs via de portal van Azure"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-the-azure-portal"></a>Een IoT hub met behulp van de Azure portal maken

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]


## <a name="introduction"></a>Inleiding

In dit artikel wordt beschreven hoe u de IoT Hub om service te vinden in de portal van Azure en hoe maken en beheren van IoT hubs.

## <a name="where-to-find-iot-hubs"></a>Waar vind ik IoT hubs

Er zijn verschillende plaatsen waar u IoT hubs kunt vinden.

1. **+ Nieuw**: **Azure IoT Hub** is een service IoT en vindt u in de categorie **Internet van zaken**, onder **+ Nieuw**, vergelijkbaar met andere services.

2. IoT hubs zijn ook toegankelijk via de Marketplace als de service prominente onder **Internet van zaken**.

## <a name="create-an-iot-hub"></a>Een hub IoT maken

U kunt een IoT hub de volgende manieren maken:

- Maken van een hub IoT via de optie **+ Nieuw** leidt naar het blad dat wordt weergegeven in de volgende schermopname. De stappen voor het maken van de hub IoT tot en met deze methode en tot en met de marketplace zijn identiek.

- Maken van een hub IoT tot en met de Marketplace: te klikken op **maken** wordt geopend een blade die identiek is aan het vorige blad voor de **+ nieuwe** ervaring. De volgende secties worden de verschillende stappen voor het maken van een hub IoT in een lijst met.

### <a name="choose-the-name-of-the-iot-hub"></a>Kies de naam van de hub IoT

Als u wilt een hub IoT hebt gemaakt, moet u de hub naam. Deze naam moet uniek zijn in de hubs. Geen dubbele hubs is toegestaan op de back-end, zodat het wordt aanbevolen dat deze hub is een unieke naam hebben als mogelijk.

### <a name="choose-the-pricing-tier"></a>Kies de prijzen laag

U kunt kiezen uit vier niveaus: **gratis**, **Standard 1** en **standaard 2**en **Standaard S3**. De gratis laag kunt u alleen 500 apparaten worden verbonden aan de hub IoT plus maximaal 8000 berichten per dag.

**Standaard S1**: IoT Hubs S1 edition is ontworpen voor IoT-oplossingen die een groot aantal apparaten genereren relatief kleine hoeveelheden gegevens per apparaat. Elk exemplaar van de S1 edition kan maximaal 400.000 berichten per dag op alle verbonden apparaten.

**Standaard S2**: IoT Hub S2 edition is ontworpen voor IoT oplossingen waarin apparaten grote hoeveelheden gegevens genereren. Elk exemplaar van de S2 edition kan maximaal 6 miljoen berichten per dag tussen alle aangesloten apparaten.

**Standaard S3**: IoT Hub S3 edition is ontworpen voor IoT-oplossingen die grote hoeveelheden gegevens genereren. Elk exemplaar van de S3 edition kan maximaal 300 miljoen berichten per dag tussen alle aangesloten apparaten.

![][4]

> [AZURE.NOTE] Hub voor IoT beschikken slechts één gratis hub per Azure abonnement.

### <a name="iot-hub-units"></a>IoT hub eenheden

Een eenheid van de hub IoT bevat een bepaald aantal berichten per dag. Het totale aantal berichten die worden ondersteund voor deze hub is het aantal eenheden vermenigvuldigd met het aantal berichten per dag voor die laag. Als u wilt dat de hub IoT ter ondersteuning van ingress van 700.000 berichten, kiest u bijvoorbeeld twee S1 laag eenheden.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Apparaat cloud partities en resourcegroep

U kunt het aantal partities voor een hub IoT wijzigen. Standaardpartities zijn ingesteld op 4; u kunt echter een verschillend aantal partities kiezen uit een vervolgkeuzelijst.

Voor resourcegroepen hoeft u niet expliciet maakt u een lege resourcegroep. Wanneer u een resource maakt, kunt u een nieuwe resourcegroep maken of een bestaande resourcegroep wilt gebruiken.

![][5]

### <a name="choose-subscriptions"></a>Kies abonnementen

Azure IoT Hub ziet automatisch u de lijst met Azure abonnementen waarnaar u het gebruikersaccount is gekoppeld. U kunt een van de opties voor het koppelen van de hub IoT met Azure abonnement te kiezen.

### <a name="choose-the-location"></a>Kies de locatie

De optie voor locatie vindt u een lijst van de regio's waarin IoT Hub wordt aangeboden. IoT Hub is beschikbaar voor het implementeren in de volgende locaties: Australië Oost; Australië Zuidoost, Oost-Azië, Zuidoost Azië, Noord-Europa, Europe West; Japan Oost, Japan West ons Oost, ons West.

### <a name="create-the-iot-hub"></a>De hub IoT maken

Wanneer alle vorige stappen voltooid hebt, is de hub IoT klaar voor worden gemaakt. Klik op **maken** om het back-enddatabase proces van het maken van deze hub IoT met de specifieke opties te starten en naar het dashboard implementeren naar de opgegeven locatie.

Het kan enkele minuten duren voordat de hub IoT moet worden gemaakt terwijl het duurt voor de implementatie van de back-enddatabase om te voorkomen in de juiste locatie-servers.

## <a name="change-the-settings-of-the-iot-hub"></a>De instellingen van de hub IoT wijzigen

U kunt de instellingen van een bestaande IoT hub wijzigen nadat deze is gemaakt van het blad IoT Hub.

![][8]

**Gedeeld Clienttoegangsbeleid**: dit beleid definiëren de machtigingen voor apparaten en services verbinding maken met IoT Hub. U kunt dit beleid openen door te klikken op **Gedeeld Clienttoegangsbeleid** onder **Algemeen**. In dit blade, kunt u bestaande beleid wijzigen of een nieuw beleid toevoegen.

### <a name="create-a-policy"></a>Beleid maken

- Klik op **toevoegen** om te openen van een blade. Hier kunt u de naam van het nieuwe beleid en de machtigingen die u koppelen aan dit beleid, wilt zoals wordt weergegeven in de volgende afbeelding.

    Er zijn verschillende machtigingen die gekoppeld aan deze gedeelde beleid worden kunnen. De eerste twee beleid, **register lezen** en **schrijven register**, verlenen lees- en schrijftoegang machtigen voor het apparaat identiteit archief of in het register identiteit. De optie voor het schrijven automatisch kiezen kiest als u ook de gelezen optie.

    Het beleid **Service verbinding** geeft het recht voor toegang tot de eindpunten cloud aan de clientzijde zoals de groep consumenten voor services verbinding maken met de hub IoT. Het beleid **apparaat verbinding maken met** verleend machtigingen voor verzenden en ontvangen van berichten op de eindpunten apparaat aan de clientzijde van de hub IoT.

- Klik op **maken** als u wilt deze nieuwe beleid aan de bestaande lijst toevoegen.

![][10]

## <a name="messaging"></a>Messaging

Klik op **Messaging** om weer te geven van een lijst met messaging eigenschappen voor de hub IoT dat is gewijzigd. Er zijn twee soorten eigenschappen die u kunt wijzigen of kopiëren: **wolk apparaat** en het **apparaat naar Cloud**.

- **Cloud naar apparaat** instellingen: deze instelling heeft twee subsettings: **wolk apparaat TTL** (time to live) en **bewaarbeleid tijd** voor de berichten. Wanneer de hub IoT eerst wordt gemaakt, worden beide deze instellingen zijn gemaakt met een standaardwaarde van een uur. Als u deze waarden, gebruik de schuifregelaars of typt u de waarden.

- Apparaatinstellingen **naar Cloud** : met deze instelling heeft verschillende subsettings, waarvan sommige met de naam/toegewezen zijn wanneer de hub IoT wordt gemaakt en kan alleen worden gekopieerd naar andere subsettings die kunnen aangepast worden. Deze instellingen worden vermeld in het volgende gedeelte.

**Partities**: deze waarde wordt ingesteld wanneer de hub IoT wordt gemaakt en tot en met deze instelling kan worden gewijzigd.

**De naam van de gebeurtenis Hub-compatibele en eindpunt**: wanneer de IoT hub wordt gemaakt, wordt een gebeurtenis Hub gemaakt intern mogelijk moet u toegang tot onder bepaalde omstandigheden. Deze gebeurtenis Hub-compatibele naam en het eindpunt kan niet worden aangepast, maar is beschikbaar voor gebruik via de knop **kopiëren** .

**Bewaarbeleid tijd**: standaard ingesteld op één dag, maar kan zo worden aangepast aan de andere waarden met de vervolgkeuzelijst. Deze waarde is in dagen voor apparaat naar Cloud en niet in uren, evenals de dezelfde instelling voor wolk apparaat.

**Groepen consumenten**: consumenten groepen zijn een vergelijkbaar is met andere systemen voor SMS-berichten die kunnen worden gebruikt om gegevens te halen in specifieke manieren om verbinding maken met andere toepassingen of services op IoT Hub-instelling. Elke hub IoT wordt gemaakt met een standaardgroep voor consumenten. U kunt echter toevoegen of verwijderen van groepen consumenten naar uw hubs IoT.

> [AZURE.NOTE] De standaardgroep voor consumenten kan niet worden bewerkt of verwijderd.

![][11]

## <a name="pricing-and-scale"></a>Prijzen en schaal

De prijzen van een bestaande IoT hub kan worden gewijzigd via de instellingen van de **prijzen** , met de volgende uitzonderingen:

- In de huidige implementatie, een hub IoT met een gratis SKU lagen niet meer wijzigen in een van de betaalde SKU's, of vice versa.
- Er kan niet meer dan één gratis laag IoT hub van de Azure-abonnement.

![][12]

Verplaatsen van een hoger niveau (S2 of S3) naar rechtsonder laag (S1 of S2), mag alleen als het aantal berichten voor die dag zich niet in conflict. Bijvoorbeeld, als het aantal berichten per dag 400.000 overschrijdt, kan klikt u vervolgens de laag voor de hub IoT worden gewijzigd. Echter als u de laag S1 wijzigt is vervolgens de hub vertraagd voor die dag.

## <a name="delete-the-iot-hub"></a>De hub IoT verwijderen

U kunt Blader naar de IoT hub die u verwijderen wilt door te klikken op **Bladeren**, klikken en vervolgens de gewenste hub uitvoeren om te verwijderen. Klik op de knop **verwijderen** onder de hubnaam van de verwijderen van de hub.

## <a name="next-steps"></a>Volgende stappen

Klik op deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

- [Bulksgewijs IoT apparaten beheren][lnk-bulk]
- [Gebruik de doelstellingen][lnk-metrics]
- [Bewerkingen bewaken][lnk-monitor]

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK van de Gateway IoT simuleren][lnk-gateway]
- [Uw oplossing IoT helemaal Secure omhoog][lnk-securing]


  [4]: ./media/iot-hub-create-through-portal/create-iothub.png
  [5]: ./media/iot-hub-create-through-portal/location1.png
  [8]: ./media/iot-hub-create-through-portal/portal-settings.png
  [10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
  [11]: ./media/iot-hub-create-through-portal/messaging-settings.png
  [12]: ./media/iot-hub-create-through-portal/pricing-error.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md