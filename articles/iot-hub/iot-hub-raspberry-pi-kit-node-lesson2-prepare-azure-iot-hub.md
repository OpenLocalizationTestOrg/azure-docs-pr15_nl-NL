<properties
 pageTitle="Uw hub IoT maken en registreren van uw frambozen Pi 3 | Microsoft Azure"
 description="Een resourcegroep maken, een hub Azure IoT maken en uw Pi registreren in de hub Azure IoT uitvoeren met de CLI Azure."
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

# <a name="22-create-your-iot-hub-and-register-your-raspberry-pi-3"></a>2.2 uw hub IoT maken en uw frambozen Pi 3 registreren

## <a name="221-what-you-will-do"></a>2.2.1 wat u doet

- Een resourcegroep maken.
- Maak uw hub Azure IoT in de resourcegroep.
- Uw frambozen Pi 3 toevoegen aan de Azure IoT hub voor het gebruik van de Azure CLI.

Wanneer u de CLI Azure uw Pi toevoegen aan uw IoT-hub gebruikt, wordt een sleutel voor uw Pi om te verifiëren met de service gegenereerd door de service. Als u problemen ondervindt overeenkomen, oplossingen in het [oplossen van de pagina](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="222-what-you-will-learn"></a>2.2.2 wat u leert

- Het gebruik van de Azure CLI maken van een Azure IoT Hub.
- Hoe u een apparaat-identiteit maken voor uw Pi in uw Azure IoT Hub.

## <a name="223-what-you-need"></a>2.2.3 wat u nodig hebt

- Een Azure-account
- Een Mac of een Windows-computer met de CLI Azure is geïnstalleerd

## <a name="224-create-your-azure-iot-hub"></a>2.2.4 uw hub Azure IoT maken

Azure IoT Hub kunt u verbinding maken, controleren en miljoenen IoT activa beheren. Als u wilt uw hub Azure IoT hebt gemaakt, de volgende stappen uit:

1. Meld u aan bij uw Azure-account door de volgende opdracht uit te voeren:

    ```bash
    az login
    ```

    Alle beschikbare Azure abonnementen worden weergegeven na een geslaagde aanmelding.

2. Stel de standaard Azure abonnement dat u gebruiken wilt door de volgende opdracht uit te voeren:

    ```bash
    az account set -n {subscription id or name}
    ```

    De naam van de abonnements-ID of vindt u in de uitvoer van `az login`.

3. De provider registreren door de volgende opdracht uit te voeren:

    ```bash
    az resource provider register -n "Microsoft.Devices"
    ```

    Voordat u de Azure resource vindt u de provider kunt implementeren, moet u de provider registreren.

    > [AZURE.NOTE] De meeste providers zijn geregistreerde automatisch door de Azure-portal of de Azure CLI die u gebruikt, maar niet alle. Zie voor meer informatie over de provider, [problemen oplossen met veelvoorkomende Azure-implementatiefouten met Azure Resource Manager](../resource-manager-common-deployment-errors.md)

4. Maak een resourcegroep iot gepaarde steekproeven in de regio West Amerikaans benoemde door de volgende opdracht uit te voeren:

    ```bash
    az resource group create --name iot-sample --location westus
    ```

5. Een hub IoT maken in de groep iot gepaarde steekproeven resource door de volgende opdracht uit te voeren:

    ```bash
    az iot hub create --name {my hub name} --resource-group iot-sample
    ```

    De standaard-editie van de IoT hub die u maakt is **F1**, **gratis**. Zie [Azure IoT Hub prijzen](https://azure.microsoft.com/pricing/details/iot-hub/)voor meer informatie.

> [AZURE.NOTE] De naam van uw hub IoT moet uniek zijn.
>
> U kunt slechts één **F1** -editie van Azure IoT Hub maken onder uw Azure-abonnement.

## <a name="225-register-your-pi-in-your-iot-hub"></a>2.2.5 uw Pi registreren in de hub IoT

Elk apparaat dat berichten van uw hub Azure IoT verzenden/ontvangen moet zijn geregistreerd bij een unieke ID.

Uw Pi registreren in de hub Azure IoT met actieve volgende opdracht uit:

```bash
az iot device create --device-id myraspberrypi --hub {my hub name} --resource-group iot-sample
```

## <a name="226-summary"></a>2.2.6 overzicht van

U hebt gemaakt van een hub Azure IoT en uw Pi geregistreerd voor een identiteit apparaat in de hub Azure IoT. U wilt gaan naar de volgende les voor meer informatie over het verzenden van berichten vanuit uw Pi op uw hub IoT gaan.

## <a name="next-steps"></a>Volgende stappen

U bent nu klaar om te beginnen les 3 die begint met het [maken van een app Azure functie en een Azure Storage-account om te verwerken en IoT hub berichten opslaan](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).