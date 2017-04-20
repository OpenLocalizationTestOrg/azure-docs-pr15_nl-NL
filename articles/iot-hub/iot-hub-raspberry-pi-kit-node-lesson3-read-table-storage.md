<properties
 pageTitle="Gelezen berichten behouden in Azure Storage | Microsoft Azure"
 description="Het apparaat naar cloud-berichten worden gecontroleerd terwijl ze naar uw Azure-tabelopslag zijn geschreven."
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="33-read-messages-persisted-in-azure-storage"></a>3.3 gelezen berichten behouden in Azure Storage

## <a name="331-what-will-you-do"></a>3.3.1 Wat doet u

Het apparaat naar cloud-berichten die wordt verzonden vanuit uw frambozen Pi 3 op uw IoT-hub, zoals de berichten worden naar uw Azure-tabelopslag geschreven worden gecontroleerd. Als u problemen ondervindt overeenkomen, oplossingen in het [oplossen van de pagina](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="332-what-will-you-learn"></a>3.3.2 Wat leert u

- Het gebruik van de taak van de bericht lezen gulp om berichten te lezen doorgevoerd in uw Azure-tabelopslag.

## <a name="333-what-do-you-need"></a>3.3.3 wat hebt u nodig

- U moet zijn voltooid de vorige sectie [uitvoeren van de toepassing van de steekproef Azure knipperen op uw frambozen Pi 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).

## <a name="334-read-new-messages-from-your-storage-account"></a>3.3.4 nieuwe berichten van uw account opslag lezen

In de vorige sectie, kunt u een van voorbeeldtoepassing op uw Pi uitgevoerd. De steekproef toepassing verzonden berichten op uw Azure IoT-hub. De berichten op uw hub IoT worden opgeslagen in uw Azure-tabelopslag via de functie Azure-app. Moet u de verbindingsreeks Azure Storage om berichten te lezen uit uw Azure-tabelopslag.

Als u wilt lezen berichten die zijn opgeslagen in uw Azure-tabelopslag, als volgt te werk:

1. De verbindingsreeks ophalen door de volgende opdrachten uit te voeren:

    ```bash
    az storage account list -g iot-sample --query [].name
    az storage account show-connection-string -g iot-sample -n {storage name}
    ```

    De eerste opdracht haalt de `storage name` die wordt gebruikt in de tweede opdracht aan de verbindingsreeks. `iot-sample`is de standaardwaarde van `{resource group name}` als u de waarde in les 2 niet wijzigen.

2. Open het configuratiebestand `config-raspberrypi.json` bestand in Visual Studio-Code door de volgende opdracht uit te voeren:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Vervang `[Azure Storage connection string]` met de verbindingsreeks die u in stap 1 hebt ontvangen.
4. Sla de `config-raspberrypi.json` bestand.
5. Berichten opnieuw verzenden vanaf lezen en ze uw Azure-tabelopslag door de volgende opdracht uit te voeren:

    ```bash
    gulp run --read-storage
    ```

    De logica voor het lezen van Azure-tabelopslag is in de `azure-table.js` bestand.

    ![gulp uitgevoerd--gelezen-opslag](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="335-summary"></a>3.3.5 overzicht van

U hebt lukt om via uw Pi verbonden met uw hub IoT in de cloud en gebruikt de toepassing van de steekproef knipperen apparaat naar cloud berichten te verzenden. U kunt ook de app Azure functie gebruikt voor de opslag van binnenkomende berichten van IoT hub naar uw Azure-tabelopslag. U kunt op verplaatsen naar de volgende les over het verzenden van berichten cloud-naar-apparaat van uw IoT hub naar uw Pi.

## <a name="next-steps"></a>Volgende stappen

[Les 4: Cloud-to-device berichten verzenden](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)
