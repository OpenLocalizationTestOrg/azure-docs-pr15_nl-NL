> [AZURE.SELECTOR]
- [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
- [C#](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)

## <a name="introduction"></a>Inleiding

Apparaat twins zijn JSON-documenten die apparaat staat informatie (metagegevens, configuraties en voorwaarden) opslaan. IoT Hub zich blijft voordoen een dubbele apparaat voor elk apparaat dat u verbinding met IoT Hub maakt.

Apparaat twins om te gebruiken:

* Sla apparaat metagegevens uit uw back-end.
* Informatie over de huidige status zoals beschikbare mogelijkheden en voorwaarden (bijvoorbeeld de connectivity-methode gebruikt) rapporteren vanuit de app van uw apparaat.
* De status van werkstromen langdurige (zoals firmware en configuratie updates) tussen apparaat-app en back-end synchroniseren.
* Query uitvoeren op uw apparaat metagegevens, configuratie of staat.

> [AZURE.NOTE] Apparaat twins zijn ontworpen voor synchronisatie en voor query's uitvoeren apparaatconfiguraties en voorwaarden. [Apparaat-naar-cloud berichten] gebruiken[ lnk-d2c] voor reeksen voorzien van een tijdstempel gebeurtenissen (zoals telemetrielogboek streams van op tijd gebaseerde sensorgegevens) en [cloud-naar-apparaat methoden] [ lnk-methods] voor interactieve besturingselement apparaten, zoals het inschakelen van een ventilator via een app door de gebruiker.

Apparaat twins zijn opgeslagen in een hub IoT en bevatten:

* *labels*, apparaat metagegevens alleen toegankelijk zijn voor de back-end;
* *gewenste eigenschappen*, JSON objecten kan worden gewijzigd door de back-end en heeft met de app apparaat; en
* *gerapporteerde eigenschappen*, JSON objecten kan worden gewijzigd door de app apparaat en gelezen door de achtergrond beëindigen. Labels en eigenschappen matrices mogen geen bevatten, maar objecten kunnen worden genest.

![][img-twin]

De app back-end kunt bovendien apparaat twins op basis van de bovenstaande gegevens te zoeken.
Verwijzen naar [informatie over apparaat twins] [ lnk-twins] voor meer informatie over het apparaat twins en naar de [querytaal IoT Hub] [ lnk-query] overzicht van query's uitvoeren.

> [AZURE.NOTE] Op dit moment kan zijn apparaat twins alleen toegankelijk via apparaten die verbinding maken met IoT Hub met het MQTT-protocol. Raadpleeg de [ondersteuning voor MQTT] [ lnk-devguide-mqtt] artikel voor instructies voor het converteren van bestaande apparaat app MQTT gebruiken.

Deze zelfstudie wordt getoond hoe naar:

- Een back-enddatabase app maakt die worden *labels* toegevoegd aan een dubbele apparaat en een gesimuleerd apparaat dat het kanaal connectivity als een *eigenschap gerapporteerd* -rapporten op de dubbele apparaat.
- Query apparaten uit uw back-end-app filters met op de labels en de eigenschappen die eerder hebt gemaakt.


<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md