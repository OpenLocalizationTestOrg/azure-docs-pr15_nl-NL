<properties
    pageTitle="Aanpassen van vooraf geconfigureerde oplossingen | Microsoft Azure"
    description="Bevat richtlijnen voor het aanpassen van de Azure IoT Suite vooraf geconfigureerde-oplossingen."
    services=""
    suite="iot-suite"
    documentationCenter=".net"
    authors="aguilaaj"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/11/2016"
     ms.author="aguilaaj"/>

# <a name="customize-a-preconfigured-solution"></a>Een vooraf geconfigureerde oplossing aanpassen

De vooraf geconfigureerde oplossingen voorzien van de Azure IoT Suite illustreren de services binnen de suite die samenwerken om een end-to-end-oplossing. Vanaf begin nu zijn diverse locaties waarin u kunt uitbreiden en aanpassen van de oplossing voor specifieke scenario's. De volgende secties worden deze algemene aanpassing wordt verwezen.

## <a name="finding-the-source-code"></a>Zoeken naar de broncode

De broncode van de vooraf geconfigureerde oplossingen is beschikbaar op GitHub in de volgende opslagplaatsen:

- Externe controle: [https://www.github.com/Azure/azure-iot-remote-monitoring](https://github.com/Azure/azure-iot-remote-monitoring)
- Bekijk onderhoud: [https://github.com/Azure/azure-iot-predictive-maintenance](https://github.com/Azure/azure-iot-predictive-maintenance)

De broncode van de vooraf geconfigureerde oplossingen is opgegeven om te laten zien van de patronen en procedures die worden gebruikt om de functionaliteit voor end-to-end van een IoT-oplossing met Azure IoT Suite te implementeren. U vindt meer informatie over het bouwen en implementeren van de oplossingen in de opslagplaatsen GitHub.

## <a name="changing-the-preconfigured-rules"></a>De vooraf geconfigureerde regels wijzigen

De externe controle oplossing bevat drie [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) taken implementatie van apparaat informatie, telemetrielogboek en regels logica weergegeven voor de oplossing.

De drie stream analytics-taken en hun syntaxis wordt beschreven in diepteas in de [externe controle vooraf geconfigureerde oplossing Stapsgewijze instructies](iot-suite-remote-monitoring-sample-walkthrough.md). 

U kunt deze taken rechtstreeks om te wijzigen van de logica bewerken of specifieke logica toevoegen aan uw scenario. U kunt de Stream Analytics-taken als volgt vinden:
 
1. Ga naar [Azure-portal](https://portal.azure.com).
2. Navigeer naar de resourcegroep met dezelfde naam als uw oplossing IoT. 
3. Selecteer de Azure Stream Analytics-taak die u wilt wijzigen. 
4. Stop de taak door te **stoppen**in de lijst met opdrachten te selecteren. 
5. De invoer, de query en de uitvoer bewerken.

    Een eenvoudige wijziging is om te wijzigen van de query voor de taak **regels** gebruik van een **"<"** in plaats van een **">"**. De oplossing-portal worden nog steeds **">"** wanneer u een regel bewerken, maar ziet u het gedrag wordt gespiegeld vanwege de wijziging in de onderliggende taak.

6. De taak starten

> [AZURE.NOTE] De externe controle dashboard, is afhankelijk van wat u specifieke gegevens, zodat de wijziging van de taken, kan het dashboard mislukt veroorzaken.

## <a name="adding-your-own-rules"></a>Uw eigen regels toevoegen

Naast het wijzigen van de vooraf geconfigureerde Azure Stream Analytics-taken, kunt u nieuwe taken toevoegen of nieuwe query's toevoegen aan bestaande taken met de portal van Azure.

## <a name="customizing-devices"></a>Apparaten aanpassen

Een van de meest voorkomende extensie activiteiten werkt samen met apparaten die specifiek zijn voor uw scenario. Er zijn verschillende methoden voor het werken met apparaten. Deze methoden omvatten een gesimuleerd apparaat zodat deze overeenkomen met uw scenario wijzigen of de [IoT apparaat SDK][] via uw apparaat fysiek verbinding met de oplossing.

Zie voor een stapsgewijze handleiding voor het toevoegen van apparaten met de externe controle vooraf geconfigureerde oplossing [Iot Suite verbinding apparaten](iot-suite-connecting-devices.md) en de [Externe controle C SDK steekproef](https://github.com/Azure/azure-iot-sdks/tree/master/c/serializer/samples/remote_monitoring) die is ontworpen voor gebruik met de externe controle vooraf geconfigureerde oplossing.

### <a name="creating-your-own-simulated-device"></a>Maken van uw eigen gesimuleerd apparaat

Opgenomen in de externe controle oplossing broncode (hierboven vermelde), is een .NET-simulator. Deze simulator is een deze is ingericht als onderdeel van de oplossing en andere metagegevens, telemetrielogboek verzenden of beantwoorden verschillende opdrachten kan worden gewijzigd.

De vooraf geconfigureerde simulator in de externe controle vooraf geconfigureerde oplossing is een lagere apparaat dat temperatuur en vochtigheid telemetrielogboek, genereert u kunt de simulator in het project [Simulator.WebJob](https://github.com/Azure/azure-iot-remote-monitoring/tree/master/Simulator/Simulator.WebJob) wijzigen wanneer u de bibliotheek GitHub hebt forked.

### <a name="available-locations-for-simulated-devices"></a>Beschikbare locaties voor gesimuleerd apparaten

De standaardset met locaties is in Seattle/Redmond, Washington, Verenigde Staten. Kunt u deze locaties in [SampleDeviceFactory.cs][lnk-sample-device-factory].


### <a name="building-and-using-your-own-physical-device"></a>Samenstellen van en werken met uw eigen (fysiek) apparaat

De [Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks) bieden bibliotheken om verbinding te maken groot apparaat grafiektypen (talen en besturingssystemen voor servers) in IoT oplossingen.

## <a name="modifying-dashboard-limits"></a>Limieten voor wijzigen dashboard

### <a name="number-of-devices-displayed-in-dashboard-dropdown"></a>Aantal apparaten die worden weergegeven in de vervolgkeuzelijst dashboard

De standaardinstelling is 200. Kunt u dit nummer in [DashboardController.cs][lnk-dashboard-controller].

### <a name="number-of-pins-to-display-in-bing-map-control"></a>Aantal pennen wilt weergeven in Bing-kaart besturingselement

De standaardinstelling is 200. Kunt u dit nummer in [TelemetryApiController.cs][lnk-telemetry-api-controller-01].

### <a name="time-period-of-telemetry-graph"></a>Periode van telemetrielogboek graph

De standaardinstelling is 10 minuten. U kunt dit wijzigen in [TelmetryApiController.cs][lnk-telemetry-api-controller-02].

## <a name="manually-setting-up-application-roles"></a>Handmatig instellen van de toepassingsrollen

De volgende procedure wordt beschreven hoe de rollen voor **beheerders** als **alleen-lezen** toepassing toevoegen aan een vooraf geconfigureerde oplossing. Rekening mee dat vooraf geconfigureerde oplossingen deze is ingericht van de site azureiotsuite.com al de rollen **beheerder** en **alleen-lezen** .

Leden van de **alleen-lezen** -rol het dashboard en de lijst met apparaten kunnen zien, maar zijn niet toegestaan apparaten toevoegen, wijzigen apparaat kenmerken of opdrachten verzenden.  Leden van **de beheerdersrol** hebben volledige toegang tot alle functies in de oplossing.

1. Ga naar de [portal van Azure klassieke][lnk-classic-portal].

2. Selecteer **Active Directory**.

3. Klik op de naam van de AAD-tenant die u gebruikt wanneer u uw oplossing deze is ingericht.

4. Klik op **toepassingen**.

5. Klik op de naam van de toepassing die overeenkomt met de oplossingsnaam van de vooraf geconfigureerde. Als u uw toepassing in de lijst niet ziet, wordt Selecteer **Mijn bedrijf eigenaar is van toepassingen** in de **weergeven** vervolgkeuzelijst en klik op het vinkje.

6.  Onderaan op de pagina, klikt u op **Beheren bestandenlijst** en klik vervolgens op **Downloaden bestandenlijst**.

7. Dit is een .json-bestand gedownload naar uw lokale computer.  In dit bestand openen voor bewerking in een teksteditor van uw keuze.

8. Op de derde regel van het bestand .json vindt u:

  ```
  "appRoles" : [],
  ```
  Dit vervangen door het volgende:

  ```
  "appRoles": [
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Administrator access to the application",
  "displayName": "Admin",
  "id": "a400a00b-f67c-42b7-ba9a-f73d8c67e433",
  "isEnabled": true,
  "value": "Admin"
  },
  {
  "allowedMemberTypes": [
  "User"
  ],
  "description": "Read only access to device information",
  "displayName": "Read Only",
  "id": "e5bbd0f5-128e-4362-9dd1-8f253c6082d7",
  "isEnabled": true,
  "value": "ReadOnly"
  } ],
  ```

9. Sla het bijgewerkte .json-bestand (u kunt het bestaande bestand overschrijven).

10.  Selecteer in de beheerportal Azure, onder aan de pagina **Beheren Manifest** Selecteer **Uploaden Manifest** het .json-bestand dat u hebt opgeslagen in de vorige stap te uploaden.

11. U hebt nu de rollen **beheerder** en **alleen-lezen** toegevoegd aan uw toepassing.

12. Als u wilt een van deze rollen toewijzen aan een gebruiker in uw adreslijst, raadpleegt u [machtigingen op de site azureiotsuite.com][lnk-permissions].

## <a name="feedback"></a>Feedback

Hebt u een aanpassing u zou willen zien gedekte in dit document? Voeg functie suggesties voor [Voicemail van de gebruiker](https://feedback.azure.com/forums/321918-azure-iot)of opmerking bij in dit artikel hieronder. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de opties voor het aanpassen van de vooraf geconfigureerde oplossingen, raadpleegt u:

- [Logica App verbinden met uw oplossing Azure IoT Suite externe controle vooraf geconfigureerde][lnk-logicapp]
- [Dynamische telemetrielogboek gebruiken met de externe controle vooraf geconfigureerde oplossing][lnk-dynamic]
- [Apparaat informatie metagegevens in de externe controle vooraf geconfigureerde oplossing][lnk-devinfo]

[lnk-logicapp]: iot-suite-logic-apps-tutorial.md
[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[IoT apparaat SDK]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-permissions]: iot-suite-permissions.md
[lnk-dashboard-controller]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/Controllers/DashboardController.cs#L27
[lnk-telemetry-api-controller-01]: https://github.com/Azure/azure-iot-remote-monitoring/blob/3fd43b8a9f7e0f2774d73f3569439063705cebe4/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L27
[lnk-telemetry-api-controller-02]: https://github.com/Azure/azure-iot-remote-monitoring/blob/e7003339f73e21d3930f71ceba1e74fb5c0d9ea0/DeviceAdministration/Web/WebApiControllers/TelemetryApiController.cs#L25 
[lnk-sample-device-factory]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Common/Factory/SampleDeviceFactory.cs#L40
[lnk-classic-portal]: https://manage.windowsazure.com
