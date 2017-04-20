<properties
 pageTitle="Maken van een functie Azure-app en opslag van Azure-account | Microsoft Azure"
 description="De app Azure functie luistert naar Azure IoT hub gebeurtenissen, binnenkomende berichten verwerkt en deze gegevens worden geschreven naar Azure-tabelopslag."
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

# <a name="31-create-an-azure-function-app-and-azure-storage-account"></a>3.1 een Azure functie-app en opslag van Azure-account maken

[Azure-functies](../../articles/azure-functions/functions-overview.md) is een oplossing voor het uitvoeren van eenvoudig kleine stukjes code, genaamd 'functies', in de cloud. Een app Azure functie host de uitvoering van de functies in Azure wordt aangegeven.

## <a name="311-what-will-you-do"></a>3.1.1 Wat doet u

Een sjabloon Azure Resource Manager gebruiken om een app Azure functie en een Azure Storage-account te maken. De app Azure functie luistert naar Azure IoT hub gebeurtenissen, binnenkomende berichten verwerkt en deze gegevens worden geschreven naar Azure-tabelopslag. Als u problemen ondervindt overeenkomen, oplossingen in het [oplossen van de pagina](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="312-what-will-you-learn"></a>3.1.2 Wat leert u

- Hoe u [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) gebruiken voor het implementeren van Azure resources.
- Het gebruik van een app Azure functie IoT hub voor berichten en schrijft u ze aan een tabel in Azure-tabelopslag.

## <a name="313-what-do-you-need"></a>3.1.3 wat hebt u nodig

- U moet hebt voltooid de vorige lessen: [aan de slag met uw frambozen Pi 3](iot-hub-raspberry-pi-kit-node-get-started.md) en [maken uw Azure IoT hub](iot-hub-raspberry-pi-kit-node-get-started.md).

## <a name="314-open-the-sample-app"></a>3.1.4 opent u de voorbeeld-app

Open het voorbeeldproject in Visual Studio-Code door de volgende opdrachten uit te voeren:

```bash
cd Lesson3
code .
```

![Cessies‑retrocessies-structuur](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

- De `app.js` bestand de `app` submap is de belangrijkste bronbestand. Dit bronbestand bevat de code te 20 keer van een bericht verzenden naar uw hub IoT knipperen de LED voor elk bericht die wordt verzonden.
- De `arm-template.json` het bestand is van de Azure resourcemanager-sjabloon met een functie Azure-app en een Azure Storage-account.
- De `arm-template-param.json` bestand is het configuratiebestand gebruikt door de resourcemanager Azure-sjabloon.
- De `ReceiveDeviceMessages` submap bevat de code Node.js voor de functie Azure.

## <a name="315-configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>3.1.5 configureren Azure resourcemanager sjablonen en resources in Azure maken

Update van de `arm-template-param.json` bestand in Visual Studio-Code.

![Parameters van Azure resourcemanager-sjabloon](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

- **[Naam van uw IoT Hub]** vervangen door **{Mijn hubnaam}** die u hebt opgegeven in [Les 2](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).
- **[Voorvoegseltekenreeks voor nieuwe resources]** vervangen door een voorvoegsel dat u wilt. Het voorvoegsel zorgt ervoor dat de naam van de resource is uniek conflicten te vermijden. Gebruik geen streepje of nummer eerste in het voorvoegsel.

> [AZURE.NOTE] U hoeft niet `azure_storage_connection_string` in deze sectie. Houd deze zo is.

Na het bijwerken van de `arm-template-param.json` bestand, de resources implementeren naar Azure door de volgende opdracht uit te voeren:

```bash
az resource group deployment create --template-file-path arm-template.json --parameters-file-path arm-template-param.json -g iot-sample -n mydeployment
```

Het duurt ongeveer 5 minuten om te maken van deze resources. Tijdens het maken van de resource uitgevoerd wordt, kunt u op verplaatsen naar de volgende sectie.

## <a name="316-summary"></a>3.1.6 overzicht van

U kunt uw app Azure functie te verwerken IoT hub berichten en een Azure Storage-account om op te slaan deze berichten hebt gemaakt. U kunt op verplaatsen naar de volgende sectie kunt implementeren en uitvoeren van de steekproef apparaat naar cloud om berichten te verzenden met uw Pi.

## <a name="next-steps"></a>Volgende stappen

[3,2 uitvoeren steekproef toepassing apparaat naar cloud om berichten te verzenden op uw frambozen Pi-3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

