<properties
 pageTitle="Azure hulpmiddelen voor krijgen (Windows 7 +) | Microsoft Azure"
 description="Installeer Python en Azure opdrachtregel-Interface (Azure CLI) op Windows 7 en nieuwere versies."
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

# <a name="21-get-azure-tools-windows-7-"></a>2.1 Azure hulpmiddelen voor krijgen (Windows 7 +)

> [AZURE.SELECTOR]
- [Windows 7 +](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
- [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
- [Mac OS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="211-what-you-will-do"></a>2.1.1 wat u doet

Python installeren en de opdrachtregel Azure Interface (Azure CLI). Als u problemen ondervindt overeenkomen, oplossingen in het [oplossen van de pagina](iot-hub-raspberry-pi-kit-node-troubleshooting.md)zoeken.

## <a name="212-what-you-will-learn"></a>2.1.2 wat u leert

- Het installeren van Python.
- Het installeren van Azure CLI.

## <a name="213-what-you-need"></a>2.1.3 wat u nodig hebt

- Een Windows-computer met internetverbinding
- Een actieve Azure-abonnement. Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) maken in een paar minuten.

## <a name="214-install-python"></a>2.1.4 Python installeren

Python installeren op uw Windows-computer. Python 2.7, 3.4 of 3.5, kunt u installeren. Deze zelfstudie is gebaseerd op Python 2.7. Als u Python al hebt geïnstalleerd, gaat u naar sectie 2.1.5.

[Python voor Windows downloaden](https://www.python.org/downloads/)

Moet u ook het pad van de mappen waarop python.exe en pip.exe aan het systeem zijn geïnstalleerd toevoegen `PATH` omgevingsvariabele. Standaard python.exe is geïnstalleerd `C:\Python27` en pip.exe is geïnstalleerd `C:\Python27\Scripts`.

## <a name="215-install-the-azure-cli"></a>2.1.5 de Azure CLI installeren

De CLI Azure beschikt u over een meerdere platforms opdrachtregel ervaring voor Azure, zodat u kunt werken rechtstreeks vanaf de opdrachtregel inrichten en bronnen beheren.

Als u wilt installeren Azure CLI, als volgt te werk:

1. Open een opdrachtpromptvenster als beheerder.
2. Voer de volgende opdrachten:

    ```bash
    pip install azure-cli-core==0.1.0b7 azure-cli-vm==0.1.0b7 azure-cli-storage==0.1.0b7 azure-cli-role==0.1.0b7 azure-cli-resource==0.1.0b7 azure-cli-profile==0.1.0b7 azure-cli-network==0.1.0b7 azure-cli-iot==0.1.0b7 azure-cli-feedback==0.1.0b7 azure-cli-configure==0.1.0b7 azure-cli-component==0.1.0b7 azure-cli==0.1.0b7
    ```
3. Controleer of u de installatie door de volgende opdracht uit te voeren:

    ```bash
    az iot -h
    ```

U ziet het volgende resultaat als de installatie voltooid is.

![AZ iot -h](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="216-summary"></a>2.1.6 overzicht van

U kunt Azure CLI hebt geïnstalleerd. Gaat u verder met de volgende sectie voor het maken van uw Azure IoT Hub en apparaat-identiteit met behulp van de Azure CLI.

## <a name="next-steps"></a>Volgende stappen

[2.2 uw hub IoT maken en uw frambozen Pi 3 registreren](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)
