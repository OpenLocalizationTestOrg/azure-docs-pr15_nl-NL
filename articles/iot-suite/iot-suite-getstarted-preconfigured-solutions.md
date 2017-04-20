<properties
    pageTitle="Aan de slag met vooraf geconfigureerde oplossingen | Microsoft Azure"
    description="Volg deze zelfstudie om te leren hoe een Azure IoT Suite vooraf geconfigureerde-oplossing implementeert."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Zelfstudie: Aan de slag met de vooraf geconfigureerde oplossingen

## <a name="introduction"></a>Inleiding

Azure IoT Suite [vooraf geconfigureerde oplossingen] [ lnk-preconfigured-solutions] combineren van meerdere Azure IoT services tot end-to-end-oplossingen die bedrijfsscenario's IoT implementeren. De oplossing *externe controle* vooraf geconfigureerde verbinding maakt en bewaakt de uw apparaten. U kunt de oplossing wilt analyseren de stroom van gegevens uit uw apparaten en bedrijven resultaten te verbeteren doordat processen die gegevensstroom automatisch beantwoorden.

Deze zelfstudie ziet u hoe u het inrichten van de externe controle vooraf geconfigureerde oplossing. Ook begeleidt u bij de basisfuncties van de externe controle oplossing. U kunt veel van deze functies openen via het dashboard oplossing die samen met de vooraf geconfigureerde oplossing implementeert:

![Externe controle vooraf geconfigureerde oplossing dashboard][img-dashboard]

Als u wilt deze zelfstudie hebt voltooid, moet u een actief Azure-abonnement.

> [AZURE.NOTE]  Als u geen account hebt, kunt u een gratis proefabonnement-account maken in een paar minuten. Zie voor meer informatie, [Gratis proefversie van Azure][lnk_free_trial].

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="view-the-solution-dashboard"></a>De oplossing dashboard bekijken

Het dashboard oplossing kunt u voor het beheren van de oplossing geïmplementeerd. U kunt bijvoorbeeld telemetrielogboek weergeven, apparaten toevoegen en configureren van regels.

1.  Wanneer de inrichting voltooid is en de tegel voor uw vooraf geconfigureerde oplossing wordt aangegeven **gaan**, klikt u op **starten** om uw externe controle oplossing-portal openen in een nieuw tabblad.

    ![De vooraf geconfigureerde oplossing starten][img-launch-solution]

2.  De oplossing-portal worden standaard weergegeven van het *dashboard van de oplossing*. U kunt andere weergaven met behulp van de linkerkant menu selecteren.

    ![Externe controle vooraf geconfigureerde oplossing dashboard][img-dashboard]

Het dashboard bevat de volgende gegevens:

- De kaart wordt de locatie van elk apparaat dat is verbonden met de oplossing. Als u de oplossing voor het eerst uitvoert, zijn er vier gesimuleerd apparaten. De gesimuleerd apparaten worden geïmplementeerd als Azure WebJobs en de oplossing maakt gebruik van de Bing Maps-API wilt uitzetten van informatie op de kaart.
- Het deelvenster **Telemetrielogboek geschiedenis** worden uitgezet vochtigheid en temperatuur telemetrielogboek vanaf een geselecteerde apparaat in bijna realtime en statistische gegevens wordt weergegeven zoals maximum, minimum aantal en gemiddelde vochtigheid.
- Het deelvenster **Waarschuwing geschiedenis** ziet u recente waarschuwing gebeurtenissen wanneer een waarde telemetrielogboek een drempel heeft overschreden. U kunt uw eigen alarmen naast de voorbeelden die zijn gemaakt door de vooraf geconfigureerde oplossing definiëren.

## <a name="view-the-device-list"></a>De lijst met apparaten weergeven

De lijst ziet u de geregistreerde apparaten in de oplossing. U weergeven en bewerken van metagegevens voor apparaten, toevoegen of verwijderen van apparaten en opdrachten verzenden naar apparaten.

1.  Klik op **apparaten** in het menu linkerkant om weer te geven van de *lijst met apparaten* voor deze oplossing.

    ![Lijst met apparaten in dashboard][img-devicelist]

2.  De lijst ziet u dat er vier gesimuleerd apparaten die zijn gemaakt door het inrichten zijn.

3.  Klik op een apparaat in de lijst met apparaten om de Apparaatdetails te geven.

    ![Details van het apparaat in dashboard][img-devicedetails]

Het deelvenster **Details apparaat** bevat drie secties:

- De sectie **Acties** worden de acties die u op het apparaat uitvoeren kunt. Als u het apparaat uitschakelt, is het niet meer toegestaan om telemetrielogboek verzenden of ontvangen van opdrachten. Als u een apparaat uitschakelt, klikt u vervolgens kunt u deze opnieuw. U kunt een die is gekoppeld aan het apparaat dat een waarschuwing activeert wanneer een waarde telemetrielogboek groter is dan de drempelwaarde voor een regel toevoegen. U kunt ook een opdracht verzenden naar een apparaat. Wanneer een apparaat eerst verbinding maakt, krijgt de oplossing de opdrachten die deze mogen beantwoorden.
- De sectie **Apparaateigenschappen** lijsten de Apparaatmetagegevens. Sommige van deze metagegevens afkomstig zijn van het apparaat zelf (zoals de fabrikant) en enkele wordt gegenereerd door de oplossing (zoals de gemaakte tijd). U kunt de Apparaatmetagegevens hier kunt bewerken.
- De sectie **Verificatie toetsen** hier de toetsen die het apparaat gebruiken kunt om te verifiëren met de oplossing.

## <a name="send-a-command-to-a-device"></a>Een opdracht verzenden naar een apparaat

Het apparaat detailvenster ziet u alle opdrachten die ondersteuning biedt voor een specifieke apparaat en kunt u verzenden naar een apparaat opdrachten. Wanneer een apparaat eerst wordt gestart, verzendt deze informatie over de opdrachten die wordt ondersteund naar de oplossing.

1.  Klik op **opdrachten** in het detailvenster apparaat voor het geselecteerde apparaat.

    ![Apparaatopdrachten in dashboard][img-devicecommands]

2.  Selecteer **PingDevice** in de opdrachtenlijst.

3.  Klik op de **opdracht verzenden**.

4.  Hier ziet u de status van de opdracht in de opdrachtgeschiedenis van de.

    ![Opdrachtstatus in dashboard][img-pingcommand]

De oplossing kunt u de status van elke opdracht verzendt bijhouden. Het resultaat is in eerste instantie **in behandeling**. Wanneer het apparaat meldt dat deze de opdracht uitgevoerd, wordt het resultaat is ingesteld op **Success**.

## <a name="add-a-new-simulated-device"></a>Een nieuw gesimuleerd apparaat toevoegen

Wanneer u de vooraf geconfigureerde oplossing implementeert, inrichten u automatisch de vier steekproef-apparaten die u in de lijst met apparaten zien kunt. Deze apparaten zijn *gesimuleerd apparaten* uitgevoerd in een WebJob Azure. Gesimuleerd apparaten kunnen u eenvoudig kunt experimenteren met de vooraf geconfigureerde oplossing hoeft te implementeren echte apparaten. Als u een reële apparaat verbinden met de oplossing wilt, raadpleegt u de [verbinding maken met uw apparaat naar de externe monitoring vooraf geconfigureerde oplossing] [ lnk-connect-rm] zelfstudie.

De volgende stappen uitgelegd hoe u een gesimuleerd apparaat toevoegen aan de oplossing:

1.  Ga terug naar de lijst met apparaten.

2.  Klik op **+ A-apparaat toevoegen** in de linkerbenedenhoek op een apparaat toevoegen.

    ![Een apparaat toevoegen aan de vooraf geconfigureerde oplossing][img-adddevice]

3.  Klik op de tegel **Gesimuleerd apparaat** op **Nieuwe toevoegen** .

    ![Details van nieuwe apparaat instellen in dashboard][img-addnew]
    
    U kunt ook een fysiek apparaat toevoegen naast het maken van een nieuw gesimuleerd apparaat, als u wilt maken van een **Aangepaste apparaat**. Meer informatie over het verbinden van fysieke apparaten met de oplossing Zie [verbinding maken met uw apparaat naar de externe controle vooraf geconfigureerde oplossing IoT-Suite][lnk-connect-rm].

4.  Selecteer **ik mijn eigen apparaat-ID definiëren**en voer de naam van een unieke apparaat-ID zoals **mydevice_01**.

5.  Klik op **maken**.

    ![Een nieuwe apparaat opslaan][img-definedevice]

6. In stap 3 van **een gesimuleerd apparaat toevoegen**, klikt u op **Gereed** om terug te keren naar de lijst met apparaten.

7. U kunt uw apparaat **uitgevoerd** in de lijst met apparaten weergeven.

    ![Nieuwe apparaat van de weergave in de lijst met apparaten][img-runningnew]

8. U kunt ook de gesimuleerd telemetrielogboek weergeven van uw nieuwe apparaat op het dashboard:

    ![Weergave-telemetrielogboek van nieuwe apparaat][img-runningnew-2]

## <a name="edit-the-device-metadata"></a>De Apparaatmetagegevens van het bewerken

Wanneer een apparaat is eerst verbinding met de oplossing maakt, stuurt de metagegevens naar de oplossing. Als u de Apparaatmetagegevens via het dashboard oplossing bewerkt, wordt de nieuwe metagegevenswaarden die zijn verzonden naar het apparaat en de nieuwe waarden worden opgeslagen in de oplossing DocumentDB-database. Zie voor meer informatie [apparaat identiteit register en DocumentDB][lnk-devicemetadata].

1.  Ga terug naar de lijst met apparaten.

2.  Selecteer uw nieuwe apparaat in de **Lijst met apparaten**en klik vervolgens op **bewerken** om de **Apparaateigenschappen**te bewerken:

    ![De Apparaatmetagegevens van het bewerken][img-editdevice]

3. Schuif omlaag en een wijziging aanbrengt in de breedte- en lengtegegevens vales. Klik vervolgens op **wijzigingen aan apparaat register op te slaan**.

    ![De Apparaatmetagegevens van het bewerken][img-editdevice2]

4. Ga terug naar het dashboard, de locatie van het apparaat is gewijzigd op de kaart:

    ![De Apparaatmetagegevens van het bewerken][img-editdevice3]

## <a name="add-a-rule-for-the-new-device"></a>Een regel voor het nieuwe apparaat toevoegen

Er zijn geen regels voor het nieuwe apparaat dat u zojuist hebt toegevoegd. In deze sectie voegt u een regel die een waarschuwing activeert wanneer de temperatuur gemeld door het nieuwe apparaat groter is dan 47 graden. Voordat u begint, zoals u ziet dat de geschiedenis telemetrielogboek voor het nieuwe apparaat op het dashboard ziet u dat de temperatuur apparaat overschrijden nooit 45 graden.

1.  Ga terug naar de lijst met apparaten.

2.  Selecteer uw nieuwe apparaat in de **Lijst met apparaten**en klik vervolgens op **regel toevoegen** als u wilt toevoegen van een regel voor het apparaat.

3. Een regel maken die gebruikmaakt van **temperatuur** als het gegevensveld en **AlarmTemp** gebruikt als de uitvoer wanneer de temperatuur groter is dan 47 graden:

    ![Een regel apparaat toevoegen][img-adddevicerule]

4. Klik op **Opslaan en regels weergeven** als uw wijzigingen wilt opslaan.

5.  Klik op **opdrachten** in het detailvenster apparaat voor het nieuwe apparaat.

    ![Een regel apparaat toevoegen][img-adddevicerule2]

6.  Selecteer **ChangeSetPointTemp** in de opdrachtenlijst en stel **SetPointTemp** op 45. Klik vervolgens op **De opdracht verzenden**:

    ![Een regel apparaat toevoegen][img-adddevicerule3]

7.  Ga terug naar het dashboard oplossing. Even ziet u een nieuwe vermelding in het deelvenster **Geschiedenis van een waarschuwing** wanneer de temperatuur gemeld door het nieuwe apparaat groter is dan de drempelwaarde voor 47 graden:

    ![Een regel apparaat toevoegen][img-adddevicerule4]

8. U kunt bekijken en bewerken van uw regels op de pagina **regels** van het dashboard:

    ![Regels voor lijst-apparaat][img-rules]

9. U kunt bekijken en bewerken van de acties die kunnen worden gegeven aan een regel op de pagina **Acties** van het dashboard:

    ![Lijstacties apparaat][img-actions]

> [AZURE.NOTE] Is het mogelijk te definiëren acties die u kunnen een e-mailbericht of SMS in antwoord op een regel verzenden of integreren met een LOB-systeem via een [Logica App][lnk-logic-apps]. Zie voor meer informatie de [logica App koppelen aan uw Azure IoT Suite externe controle vooraf geconfigureerde oplossing][lnk-logicapptutorial].

## <a name="other-features"></a>Andere functies

Met behulp van de oplossing portal die kunt u zoeken voor apparaten met specifieke kenmerken zoals het modelnummer van een:

![Zoeken naar een apparaat][img-search]

U kunt een apparaat uitschakelen en nadat deze is uitgeschakeld kunt u deze verwijderen:

![Uitschakelen en een apparaat verwijderen][img-disable]

## <a name="behind-the-scenes"></a>Achter de schermen

Wanneer u een vooraf geconfigureerde oplossing implementeert, maakt het implementatieproces meerdere resources in het Azure abonnement dat u hebt geselecteerd. U kunt deze resources weergeven in de [portal]-Azure[lnk-portal]. Een **resourcegroep** maakt implementatie met een naam die is gebaseerd op de naam die u voor uw vooraf geconfigureerde oplossing kiest:

![Vooraf geconfigureerde oplossing in de portal van Azure][img-portal]

U kunt de instellingen van elke resource weergeven door deze te selecteren in de lijst met bronnen in de resourcegroep.

U kunt ook de broncode van de vooraf geconfigureerde oplossing weergeven. De externe controle vooraf geconfigureerde oplossing-broncode is in de [azure-iot-remote-cmdlets voor controle] [ lnk-rmgithub] GitHub bibliotheek:

- De map **DeviceAdministration** bevat de broncode voor het dashboard.
- De map **Simulator** bevat de broncode voor het gesimuleerd apparaat.
- De map **EventProcessor** bevat de broncode voor het back-enddatabase-proces dat de binnenkomende telemetrielogboek verwerkt.

Wanneer u klaar bent, kunt u de vooraf geconfigureerde oplossing verwijderen uit uw Azure abonnement op de [azureiotsuite.com] [ lnk-azureiotsuite] site. Deze site kunt u eenvoudig verwijderen alle bronnen die heeft gekregen tijdens het maken van de vooraf geconfigureerde oplossing.

> [AZURE.NOTE] Om ervoor te zorgen dat u alles gerelateerd aan de vooraf geconfigureerde oplossing verwijdert, verwijdert u deze op de [azureiotsuite.com] [ lnk-azureiotsuite] site en gewoon Verwijder de resourcegroep in de portal.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt een vooraf geconfigureerde werkoplossing geïmplementeerd, kunt u doorgaan met het aan de slag met IoT Suite door te lezen van de volgende artikelen:

- [Externe controle vooraf geconfigureerde oplossing Stapsgewijze instructies][lnk-rm-walkthrough]
- [Uw apparaat verbinden met de externe controle vooraf geconfigureerde oplossing][lnk-connect-rm]
- [Machtigingen voor de site azureiotsuite.com][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-editdevice]: media/iot-suite-getstarted-preconfigured-solutions/editdevice.png
[img-editdevice2]: media/iot-suite-getstarted-preconfigured-solutions/editdevice2.png
[img-editdevice3]: media/iot-suite-getstarted-preconfigured-solutions/editdevice3.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-search]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_07.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
