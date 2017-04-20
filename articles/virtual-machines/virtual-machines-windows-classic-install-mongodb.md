<properties
    pageTitle="MongoDB installeren op een Windows-VM | Microsoft Azure"
    description="Leer hoe u MongoDB installeert op een Azure VM die zijn gemaakt met het klassieke implementatiemodel met Windows Server."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

#<a name="install-mongodb-on-a-windows-vm"></a>MongoDB installeren op een Windows-VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Als u wilt installeren en configureren met het implementatiemodel resourcemanager MongoDB, Zie [dit artikel](virtual-machines-windows-classic-install-mongodb.md).

[MongoDB] [ MongoDB] is een populaire open source, krachtige NoSQL database. In dit artikel helpt u bij het maken van een Windows Server virtuele machine (VM) met behulp van de [Azure klassieke portal][AzurePortal]. U vervolgens maakt en een gegevensschijf aan de VM voordat installeren en configureren van MongoDB toegevoegd. Als u een bestaande VM in Azure wordt aangegeven dat u wilt gebruiken hebt, kunt u rechtstreeks naar [installeren en configureren van MongoDB](#install-and-run-mongodb-on-the-virtual-machine)gaan.


## <a name="create-a-virtual-machine-running-windows-server"></a>Een virtuele machine met Windows Server maken

Volg deze instructies voor het maken van een virtuele machine.

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

> [AZURE.NOTE] U kunt een eindpunt voor MongoDB toevoegen tijdens het maken van de virtuele machine en configureert u deze als volgt: noem deze als **Mongo**, **TCP** gebruiken als het protocol en de openbare en persoonlijke poorten ingesteld op **27017**.

## <a name="attach-a-data-disk"></a>Een gegevensschijf koppelen
Opslagruimte voor de virtuele machine, een gegevensschijf hebt toegevoegd en geïnitialiseerd zodat Windows deze kunt gebruiken. Als u al een gegevensschijf hebt, kunt u de bestaande schijf koppelen of u een lege schijf kunt koppelen.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

Zie voor instructies over het initialisatie van de schijf, "hoe: een nieuwe gegevensschijf in Windows Server geïnitialiseerd ' in [het toevoegen van een gegevensschijf aan een virtuele Windows-computer](virtual-machines-windows-classic-attach-disk.md).

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Installeren en uitvoeren van MongoDB op de virtuele machine

[AZURE.INCLUDE [install-and-run-mongo-on-win2k8-vm](../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Overzicht
In deze zelfstudie hebt u geleerd hoe u een virtuele machine met Windows Server maken, extern verbinden met dit, en een gegevensschijf hebt toegevoegd.  U hebt geleerd ook installeren en configureren van MongoDB op de Windows-virtuele machine. U kunt nu toegang tot MongoDB op de Windows gebaseerde virtuele machine, volgens de geavanceerde onderwerpen in de [documentatie van MongoDB][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: http://manage.windowsazure.com
