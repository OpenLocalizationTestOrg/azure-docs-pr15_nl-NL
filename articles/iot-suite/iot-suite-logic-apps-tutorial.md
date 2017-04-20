<properties
  pageTitle="Azure IoT-Suite en logica Apps | Microsoft Azure"
  description="Een zelfstudie over de logica Apps Azure IoT Suite koppelen voor bedrijfsproces."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="08/16/2016"
  ms.author="araguila"/>
  
# <a name="tutorial-connect-logic-app-to-your-azure-iot-suite-remote-monitoring-preconfigured-solution"></a>Zelfstudie: Verbinden met logica App uw oplossing Azure IoT Suite externe controle vooraf geconfigureerde

De [Microsoft Azure IoT Suite] [ lnk-internetofthings] externe controle vooraf geconfigureerde oplossing is een uitstekende manier om snel aan de slag met een end-to-end-functieset die voorbeeld van een oplossing IoT de voordelen. Deze zelfstudie leert u hoe logica App toevoegen aan uw externe vooraf geconfigureerde oplossing controle van Microsoft Azure IoT Suite. Deze stappen wordt beschreven hoe u uw oplossing IoT nog verder door deze verbinding maakt met een bedrijfsproces kunt uitvoeren.

_Als u op zoek bent naar stapsgewijze instructies voor het inrichten een externe controle vooraf geconfigureerde oplossing, raadpleegt u [Zelfstudie: aan de slag met de oplossingen IoT vooraf geconfigureerde][lnk-getstarted]._

Voordat u deze zelfstudie begint, moet u:

- Inrichten van de externe controle vooraf geconfigureerde oplossing in uw Azure-abonnement.

- Maak een account SendGrid zodat u kunt het verzenden van een e-mailbericht die uw bedrijfsproces activeert. U kunt zich aanmeldt voor een gratis proefabonnement account voor [SendGrid](https://sendgrid.com/) door te klikken op **gratis proberen**. Nadat u hebt geregistreerd voor een gratis proefabonnement account, moet u een [API-sleutel](https://sendgrid.com/docs/User_Guide/Settings/api_keys.html) maken in SendGrid die worden toegewezen machtigingen om e-mail te verzenden. Verderop in deze zelfstudie moet u deze API-sleutel.

Ervan uitgaande dat u hebt al deze is ingericht uw externe controle vooraf geconfigureerde oplossing, gaat u naar de resourcegroep voor deze oplossing in de [portal van Azure][lnk-azureportal]. De resourcegroep heeft dezelfde naam als de naam van de oplossing gekozen wanneer u deze is ingericht uw externe controle oplossing. In de resourcegroep ziet u de ingerichte Azure resources voor uw oplossing, behalve voor de Azure Active Directory-toepassing die u in de klassieke Azure-Portal vinden kunt. De volgende schermafbeelding ziet u een voorbeeld **resourcegroep** blade voor een externe controle vooraf geconfigureerde oplossing:

![](media/iot-suite-logic-apps-tutorial/resourcegroup.png)

Als u wilt beginnen, de logica app instellen voor gebruik met de vooraf geconfigureerde oplossing.

## <a name="set-up-the-logic-app"></a>De logica App instellen

1. Klik op __toevoegen__ boven aan de resource groep blade in de portal van Azure.

2. Zoeken naar __Logica App__, selecteert u deze en klik op **maken**.

3. Vul de __naam__ en gebruik de hetzelfde **abonnement** en **resourcegroep** die u gebruikt wanneer u uw externe controle oplossing deze is ingericht. Klik op __maken__.

    ![](media/iot-suite-logic-apps-tutorial/createlogicapp.png)

4. Als uw implementatie is voltooid, ziet u dat de App logica is vermeld als een resource in de resourcegroep.

5. Klik op de App logica om te navigeren naar het blad logica App, selecteert u de sjabloon **Lege logica App** om de **Logica Apps Designer**te openen.

    ![](media/iot-suite-logic-apps-tutorial/logicappsdesigner.png)

6. Selecteer __aanvragen__. Deze actie geeft aan dat een binnenkomende HTTP-aanvraag met een specifieke JSON nettolading handelingen als een trigger opgemaakt.

7. Plak de volgende handelingen uit in het Schema hoofdtekst JSON aanvragen:

    ```
    {
      "$schema": "http://json-schema.org/draft-04/schema#",
      "id": "/",
      "properties": {
        "DeviceId": {
          "id": "DeviceId",
          "type": "string"
        },
        "measuredValue": {
          "id": "measuredValue",
          "type": "integer"
        },
        "measurementName": {
          "id": "measurementName",
          "type": "string"
        }
      },
      "required": [
        "DeviceId",
        "measurementName",
        "measuredValue"
      ],
      "type": "object"
    }
    ```
    
    > [AZURE.NOTE] Nadat u de app logica opslaan, maar eerst moet u een actie toevoegen, kunt u de URL voor de HTTP-post kopiëren.

8. Klik onder uw handmatige trigger op __+ nieuwe stap__ . Klik vervolgens op **een actie toevoegen**.

    ![](media/iot-suite-logic-apps-tutorial/logicappcode.png)

9. Zoeken naar **SendGrid - e-mailbericht verzenden** en klikt u erop.

    ![](media/iot-suite-logic-apps-tutorial/logicappaction.png)

10. Voer een naam voor de verbinding, zoals **SendGridConnection**, voert u de **SendGrid API-sleutel** die u hebt gemaakt wanneer u uw account SendGrid instellen en klik op **maken**.

    ![](media/iot-suite-logic-apps-tutorial/sendgridconnection.png)

11. E-mailadressen die u eigenaar bent van zowel de velden **van** en **naar** toevoegen. **Externe controle melding [DeviceId]** naar het veld **onderwerp** toevoegen. In het veld **E-mailbericht hoofdtekst** toevoegen **apparaat [DeviceId] [measurementName] met de waarde [measuredValue] heeft gerapporteerd.** U kunt **[DeviceId]**, **[measurementName]**en **[measuredValue]** toevoegen door te klikken in de sectie **kunt u gegevens uit de vorige stappen invoegen** .

    ![](media/iot-suite-logic-apps-tutorial/sendgridaction.png)

12. Klik op __Opslaan__ in het bovenste menu.

13. Klik op de trigger **aanvragen** en de waarde __Http posten naar deze URL__ kopiëren. Deze URL moet u verderop in deze zelfstudie.

> [AZURE.NOTE] Logica Apps kunt u [veel verschillende soorten actie] uitvoeren[ lnk-logic-apps-actions] acties op te nemen in Office 365. 

## <a name="set-up-the-eventprocessor-web-job"></a>De taak van de Web EventProcessor instellen

In deze sectie, kunt u uw vooraf geconfigureerde oplossing verbinding met de logica-App die u hebt gemaakt. Als u wilt deze taak uitvoert, moet u de URL als zo activeren dat de logica-App op de actie die wordt uitgevoerd wanneer een apparaat sensor waarde een drempelwaarde overschrijdt toevoegen.

1. Gebruik van de klant cijfer kopie van de nieuwste versie van de [azure-iot-remote-cmdlets voor controle github opslagplaats][lnk-rmgithub]. Bijvoorbeeld:

    ```
    git clone https://github.com/Azure/azure-iot-remote-monitoring.git
    ```

2. Open in Visual Studio, de __RemoteMonitoring.sln__ uit de lokale kopie van de bibliotheek.

3. Open het bestand __ActionRepository.cs__ in de **infrastructuur\\opslagplaats** map.

4. De woordenlijst **actionIds** bijwerken met het __Http-posten naar deze URL__ die u als volgt uit uw logica-App hebt genoteerd:

    ```
    private Dictionary<string,string> actionIds = new Dictionary<string, string>()
    {
        { "Send Message", "<Http Post to this UR>" },
        { "Raise Alarm", "<Http Post to this UR> }
    };
    ```

5. Sla de wijzigingen in de oplossing en Visual Studio afsluiten.

## <a name="deploy-from-the-command-line"></a>Vanaf de opdrachtregel implementeren

In deze sectie, kunt u de bijgewerkte versie van de externe controle oplossing voor het vervangen van de versie die momenteel uitgevoerd in Azure implementeren.

1. Na de [installatie van de ontwikkelaar] [ lnk-devsetup] instructies voor het instellen van uw omgeving voor implementatie.

2.  Als u wilt implementeren lokaal, volgt u de [lokale implementatie] [ lnk-localdeploy] instructies.

3.  Als u wilt implementeren naar de cloud en bijwerken van uw bestaande cloud-implementatie, volgt u de [cloud-implementatie] [ lnk-clouddeploy] instructies. Gebruik de naam van uw oorspronkelijke implementatie als de naam van de implementatie. Voor voorbeeld als u de oorspronkelijke implementatie heet **demologicapp**, gebruikt u de volgende opdracht uit:

    ``
    build.cmd cloud release demologicapp
    ``
    
    Wanneer het opbouwscript wordt uitgevoerd, moet u de dezelfde Azure-account, abonnement, regio en Active Directory-exemplaar die u hebt gebruikt bij het inrichten van de oplossing gebruiken.

## <a name="see-your-logic-app-in-action"></a>Uw App logica in actie zien

De externe controle vooraf geconfigureerde oplossing heeft twee regels al dan niet standaard instellen wanneer u een oplossing inrichten. Beide regels zijn op het apparaat **SampleDevice001** :

* Temperatuur > 38.00
* Vochtigheid > 48,00

De regel temperatuur gebeurtenis wordt geactiveerd **Waarschuwing verhogen** en de regel vochtigheid gebeurtenis wordt geactiveerd **SendMessage** . Als dat u dezelfde URL hebt gebruikt voor beide acties worden de klasse **ActionRepository** , uw app logica voor beide regel wordt geactiveerd. Beide regels gebruik SendGrid een e-mail te verzenden naar het adres **naar** met details van de waarschuwing.

> [AZURE.NOTE] De App logica nog altijd de drempelwaarde is voldaan activeren. Als u wilt voorkomen dat onnodige e-mailberichten, kunt u de regels in de portal van uw oplossing uitschakelen of uitschakelen van de logica-App in de [portal van Azure][lnk-azureportal].

Naast het ontvangen van e-mailberichten, kunt u ook zien wanneer de logica-App wordt uitgevoerd in de portal:

![](media/iot-suite-logic-apps-tutorial/logicapprun.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat u een App logica de vooraf geconfigureerde oplossing verbinden met een bedrijfsproces gebruikt hebt, kunt u meer leren over de opties voor het aanpassen van de vooraf geconfigureerde oplossingen:

- [Dynamische telemetrielogboek gebruiken met de externe controle vooraf geconfigureerde oplossing][lnk-dynamic]
- [Apparaat informatie metagegevens in de externe controle vooraf geconfigureerde oplossing][lnk-devinfo]

[lnk-dynamic]: iot-suite-dynamic-telemetry.md
[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[lnk-internetofthings]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-getstarted]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-azureportal]: https://portal.azure.com
[lnk-logic-apps-actions]: ../connectors/apis-list.md
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devsetup]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/dev-setup.md
[lnk-localdeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/local-deployment.md
[lnk-clouddeploy]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
