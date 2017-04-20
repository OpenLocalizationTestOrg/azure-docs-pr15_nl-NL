<properties
     pageTitle="De Azure-portal gebruiken voor het configureren van bestandsupload | Microsoft Azure"
     description="Een overzicht van het bestand uploaden met behulp van de Azure portal configureren"
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

# <a name="configure-file-uploads-using-the-azure-portal"></a>Met behulp van de Azure portal bestandsuploads configureren

## <a name="file-upload"></a>Bestand uploaden

Gebruik van het [bestand uploaden functionaliteit in IoT Hub][lnk-upload], moet u eerst een account Azure Storage koppelen aan uw hub. Selecteer de instellingen **bestand uploaden** voor het weergeven van bestand uploaden eigenschappen voor de hub IoT dat is gewijzigd.

![][13]

**Opslag container**: de Azure-portal gebruiken om te selecteren van een container blob in een account Azure opslag van uw huidige Azure-abonnement om te koppelen aan uw IoT Hub. Zo nodig kunt u een account Azure opslagruimte op de **opslag accounts** blade en blob de container op het blad **Containers** . IoT Hub genereert automatisch SA's URI's met schrijfmachtigingen aan deze container blob voor apparaten om over te gebruiken wanneer ze bestanden uploaden.

![][14]

**Meldingen ontvangen voor geüploade bestanden**: in- of uitschakelen van meldingen over het uploaden van bestand via de wisselknop.

**SA's TTL**: deze instelling is de time to live van de SA's URI's door IoT Hub naar het apparaat wordt geretourneerd. Standaard ingesteld op 1 uur, maar kan zo worden aangepast aan de andere waarden met de schuifregelaar.

**Bestand melding instellingen default TTL**: de time to live van een bestand uploaden melding voordat deze is verlopen. Standaard ingesteld op één dag, maar kan zo worden aangepast aan de andere waarden met de schuifregelaar.

**Bestand melding bezorging van het maximum aantal**: het aantal keren de IoT Hub probeert een bestand uploaden melding. Standaard ingesteld op 10, maar kan zo worden aangepast aan de andere waarden met de schuifregelaar.

![][15]

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de functies voor het uploaden van bestanden van IoT-Hub, [uploaden van bestanden vanaf een apparaat] [ lnk-upload] in de handleiding voor ontwikkelaars.

Klik op deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

- [Bulksgewijs IoT apparaten beheren][lnk-bulk]
- [Gebruik de doelstellingen][lnk-metrics]
- [Bewerkingen bewaken][lnk-monitor]

Als u wilt verkennen verder de mogelijkheden van IoT-Hub, raadpleegt u:

- [Handleiding voor ontwikkelaars][lnk-devguide]
- [Een apparaat met de SDK van de Gateway IoT simuleren][lnk-gateway]
- [Uw oplossing IoT helemaal Secure omhoog][lnk-securing]


  [13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
  [14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
  [15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md