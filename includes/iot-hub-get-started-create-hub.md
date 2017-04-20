## <a name="create-an-iot-hub"></a>Een hub IoT maken

Maak een hub IoT voor uw gesimuleerd apparaat kunt verbinden. De volgende stappen hoe u deze taak uitvoert met behulp van de Azure-portal.

1. Meld u aan bij de [portal van Azure][lnk-portal].

2. Klik in de Jumpbar, op **Nieuw** > **Internet van zaken** > **Azure IoT Hub**.

    ![Azure-portal Jumpbar][1]

3. Kies in het blad **IoT hub** de configuratie voor uw hub IoT.

    ![IoT hub blade][2]

    * Voer in het vak **naam** een naam voor uw hub IoT. Als de **naam** geldig en beschikbaar is, wordt een groen vinkje weergegeven in het vak **naam** .
    * Selecteer een [prijzen en schaal laag][lnk-pricing]. Deze zelfstudie is niet vereist voor een specifieke laag. Voor deze zelfstudie gebruikt u de gratis F1 laag.
    * Een nieuwe resourcegroep maken in **resourcegroep**of Selecteer een bestaande eigenschap. Voor meer informatie raadpleegt u [werken met resourcegroepen voor het beheren van uw Azure resources][lnk-resource-groups].
    * Selecteer in de **locatie**, de locatie voor het hosten van uw hub IoT. Kies de dichtstbijzijnde locatie voor deze zelfstudie.

4. Wanneer u uw hub IoT configuratieopties gekozen hebt, klikt u op **maken**.  Het kan enkele minuten duren voordat Azure uw hub IoT maken. U kunt u de status controleren door de voortgang in de Startboard of in het deelvenster meldingen controleren.

    ![Nieuwe IoT hub status][3]

5. Wanneer de hub IoT is gemaakt, klikt u op de nieuwe tegel voor uw hub IoT in de Azure-portal te openen van het blad voor de nieuwe IoT-hub. Noteer de **Hostname**, en klik vervolgens op **gedeeld-beleid**.

    ![Nieuwe IoT hub blade][4]

6. In het blad **clienttoegangsbeleid gedeeld** , klik op het beleid **iothubowner** en kopieer en noteer de verbindingsreeks in het blad **iothubowner** . Zie [toegangsbeheer] voor meer informatie[ lnk-access-control] in de "Azure IoT Hub developer guide."

    ![Gedeelde toegang beleidsregels blade][5]


<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-portal/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
