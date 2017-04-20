<properties
    pageTitle="Verbinding maken met Windows VMs in een cloudservice | Microsoft Azure"
    description="Verbinding maken met Windows virtuele machines die zijn gemaakt met het implementatiemodel klassieke aan een Azure cloudservice of virtueel netwerk."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="connect-windows-virtual-machines-created-with-the-classic-deployment-model-with-a-virtual-network-or-cloud-service"></a>Verbinding maken met Windows virtuele machines die zijn gemaakt met het implementatiemodel klassieke met een virtueel netwerk of cloud-service

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Windows virtuele machines die zijn gemaakt met het implementatiemodel klassieke worden altijd geplaatst in een cloudservice. De cloudservice fungeert als een container en biedt een unieke naam voor openbare DNS, een openbare IP-adres en een reeks eindpunten voor toegang tot de virtuele machine via Internet. De cloudservice soms in een virtueel netwerk, maar dat is niet vereist. U kunt ook een [verbinding maken met Linux virtuele machines met een virtueel netwerk of cloud-service](virtual-machines-linux-classic-connect-vms.md).

Als een cloudservice niet in een virtueel netwerk, is deze functie heet van een *zelfstandig product* -cloudservice. De virtuele machines in een cloudservice zelfstandige kunnen alleen communiceren met andere virtuele machines met behulp van de andere virtuele machines openbare DNS-namen en het verkeer overdracht via Internet. Als een cloudservice zich in een virtueel netwerk, de virtuele machines in dat cloudservice met alle andere virtuele machines in het virtuele netwerk communiceren kan zonder het verzenden van al het verkeer via Internet.

Als u uw virtuele machines in de dezelfde zelfstandige-cloudservice plaatst, kunt u nog steeds taakverdeling en beschikbaarheid sets gebruiken. Zie [virtuele machines van taakverdeling](virtual-machines-windows-load-balance.md) en [de beschikbaarheid van virtuele machines beheren](virtual-machines-windows-manage-availability.md)voor meer informatie. U niet kunt echter de virtuele machines op subnetten organiseren of een zelfstandige cloudservice verbinden met uw on-premises netwerk. Hier volgt een voorbeeld:

[AZURE.INCLUDE [virtual-machines-common-classic-connect-vms](../../includes/virtual-machines-common-classic-connect-vms.md)]

## <a name="next-steps"></a>Volgende stappen

Nadat u een virtuele machine hebt gemaakt, is het een goed idee om [een gegevensschijf toevoegen](virtual-machines-windows-classic-attach-disk.md) zodat uw services en de werkbelasting hebt een locatie voor de opslag van gegevens. 