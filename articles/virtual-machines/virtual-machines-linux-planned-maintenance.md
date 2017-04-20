<properties
    pageTitle="Gepland onderhoud voor Linux VMs | Microsoft Azure"
    description="Begrijpen welke Azure gepland onderhoud is en hoe dit van invloed is op uw Linux virtuele machines uitgevoerd in Azure wordt aangegeven"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="drewm"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="drewm"/>

# <a name="planned-maintenance-for-linux-virtual-machines-in-azure"></a>Gepland onderhoud voor Linux virtuele machines in Azure wordt aangegeven

Begrijpen welke Azure gepland onderhoud is en hoe deze invloed kan zijn op de beschikbaarheid van uw Linux virtuele machines. In dit artikel is ook beschikbaar voor [Windows virtuele machines](virtual-machines-windows-planned-maintenance.md). 

In dit artikel vindt een achtergrond als in het proces Azure gepland onderhoud. Als u bepalen willen zijn waarom uw VM opnieuw gestart, kunt u [lezen dit blog bericht met informatie over het weergeven van VM opnieuw logboeken](https://azure.microsoft.com/blog/viewing-vm-reboot-logs/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="why-azure-performs-planned-maintenance"></a>Waarom worden uitgevoerd Azure gepland onderhoud

Microsoft Azure voert regelmatig updates overal ter wereld om de betrouwbaarheid, prestaties en beveiliging van de host-infrastructuur waarop virtuele machines te verbeteren. Veel van deze updates worden uitgevoerd zonder eventuele gevolgen voor uw virtuele machines of Cloud Services, met inbegrip van updates geheugen behouden.

Sommige updates hebben echter opnieuw aan uw virtuele machines de vereiste updates toepassen op de infrastructuur nodig. De virtuele machines worden afgesloten terwijl we de infrastructuur patch, en vervolgens de virtuele machines opnieuw worden gestart.

Houd er rekening mee dat er zijn twee soorten onderhoud die van invloed kunnen de beschikbaarheid van uw virtuele machines: geplande en ongeplande. Deze pagina wordt beschreven hoe Microsoft Azure gepland onderhoud uitvoert. Zie [informatie over geplande versus niet gepland onderhoud](virtual-machines-linux-manage-availability.md)voor meer informatie over niet-gepland onderhoud.

[AZURE.INCLUDE [virtual-machines-common-planned-maintenance](../../includes/virtual-machines-common-planned-maintenance.md)]
