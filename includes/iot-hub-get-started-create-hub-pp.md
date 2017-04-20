## <a name="create-a-device-management-enabled-iot-hub"></a>Maken van een apparaat management IoT Hub ingeschakeld

Omdat IoT Hub Apparaatbeheer zich in de Preview-versie, moet u een apparaat management ingeschakeld IoT hub maken. Wanneer de Apparaatbeheer IoT Hub algemene beschikbaarheid bereikt, zijn deze zelfstudie bijgewerkt. De volgende stappen hoe u deze taak met behulp van de Azure portal uitvoert.

1.  Meld u aan bij de [portal van Azure].
2.  In de Jumpbar, klik op **Nieuw**en vervolgens op **Internet van zaken**en klik vervolgens op **Azure IoT Hub**.

    ![][img-new-hub]

3.  Kies in het blad **IoT Hub** de configuratie voor uw IoT Hub.

    ![][img-configure-hub]

  -   Voer in het vak **naam** een naam voor uw IoT Hub. Als de **naam** geldig en beschikbaar is, wordt een groen vinkje weergegeven in het vak **naam** .
  -   Selecteer een **prijzen en schaal laag**. Deze zelfstudie is niet vereist voor een specifieke laag.
  -   Een nieuwe resourcegroep maken in **resourcegroep**of Selecteer een bestaande eigenschap. Zie [werken met resourcegroepen uw Azure bronnen beheren]voor meer informatie.
  -   Schakel het selectievakje naar **Inschakelen Apparaatbeheer - PREVIEW**.
  -   Selecteer in de **locatie**, de locatie voor het hosten van uw IoT Hub. Apparaatbeheer IoT Hub is alleen beschikbaar in Oost-VS, Noord Europe en Oost-Azië in openbare voorbeeldweergave.

    > [AZURE.NOTE]Als u het selectievakje voor het **Inschakelen van Apparaatbeheer**niet inschakelt, werken niet in de voorbeelden.<br/>Door te schakelen **Apparaatbeheer inschakelen**, moet u een voorbeeld IoT Hub alleen ondersteund in Oost-VS, Noord Europe en Oost-Azië en niet is bedoeld voor productie scenario's maken. U kunt geen apparaten migreren in- en afmelden bij apparaat management ingeschakeld hubs.

4.  Wanneer u uw IoT Hub configuratieopties gekozen hebt, klikt u op **maken**. Het kan enkele minuten duren voordat Azure uw IoT Hub maken. U kunt u de status controleren door de voortgang in de **Startboard** of in het deelvenster **meldingen** controleren.

    ![][img-monitor]

5.  Wanneer de IoT Hub is gemaakt, wordt het blad voor de hub automatisch geopend. Noteer de **Hostname**, en klik vervolgens op **gedeeld-beleid**.

    ![][img-keys]

6.  Klik op het beleid **iothubowner** en vervolgens kopiëren en noteer de verbindingsreeks in het blad **iothubowner** . Kopieer deze naar een locatie die u later gebruiken kunt omdat u deze om te voltooien van deze zelfstudie nodig hebt.

    > [AZURE.NOTE] Zorg dat u geen de **iothubowner** -referenties gebruiken in productie scenario's.

    ![][img-connection]

U hebt nu gemaakt met een apparaat management IoT Hub ingeschakeld. Moet u de verbindingsreeks om te voltooien van deze zelfstudie.

## <a name="create-a-device-identity"></a>Een identiteit apparaat maken

In deze sectie, gebruikt u een knooppunt hulpmiddel [IoT Hub Explorer] [ iot-hub-explorer] te maken van een identiteit apparaat voor deze zelfstudie.

1. Voer de volgende in de opdrachtregel omgeving:

    npm installatie -giothub-explorer@latest

2. Voer de volgende opdracht naar Meld u aan bij uw hub vooruit te vervangen `{service connection string}` met de IoT Hub-verbindingsreeks die u eerder hebt gekopieerd:

    iothub-explorer login "{service verbindingsreeks}"

3. Ten slotte maakt een nieuwe apparaat identiteit genoemd `myDeviceId` met de opdracht:

    iothub-explorer maken myDeviceId--verbindingsreeks

Noteer de verbindingsreeks apparaat van het resultaat. Deze verbindingsreeks wordt gebruikt door de app apparaat verbinding maken met uw IoT Hub als een apparaat.

![][img-identity]

Verwijzen naar [aan de slag met IoT Hub] [ lnk-getstarted] voor een manier om het apparaat identiteiten via programmacode maken.

<!-- images and links -->
[img-new-hub]: media/iot-hub-get-started-create-hub-pp/image1.png
[img-configure-hub]: media/iot-hub-get-started-create-hub-pp/image2.png
[img-monitor]: media/iot-hub-get-started-create-hub-pp/image3.png
[img-keys]: media/iot-hub-get-started-create-hub-pp/image4.png
[img-connection]: media/iot-hub-get-started-create-hub-pp/image5.png
[img-identity]: media/iot-hub-get-started-create-hub-pp/devidentity.png

[Azure-portal]: https://portal.azure.com/
[iot-hub-explorer]: https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
[Resourcegroepen gebruiken voor het beheren van uw Azure resources]: ../articles/azure-portal/resource-group-portal.md
