<properties
    pageTitle="Een gegevensschijf toevoegen aan een VM Linux | Microsoft Azure"
    description="Het nieuwe of bestaande gegevensschijf toevoegen aan een VM Linux in de Azure-portal met behulp van het implementatiemodel resourcemanager."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cynthn"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-vm-in-the-azure-portal"></a>Hoe u een gegevensschijf koppelt aan een VM Linux in de portal van Azure

In dit artikel leest u hoe u nieuwe en bestaande schijven koppelt aan een virtuele Linux machine via de portal van Azure. U kunt ook [een gegevensschijf voor een Windows-VM in de portal van Azure hebt toegevoegd](virtual-machines-windows-attach-disk-portal.md). Voordat u dit doet, raadpleegt u deze tips:

- De grootte van de virtuele machine besturingselementen hoeveel gegevensschijven u kunt koppelen. Zie [grootten voor virtuele machines](virtual-machines-linux-sizes.md)voor meer informatie.
- Als u wilt gebruiken Premium opslag, moet u een VM DS-reeks of GS-reeks. Met deze virtuele machines kunt u schijven van zowel Premium en standaard opslag-accounts. Premium opslag is beschikbaar in bepaalde regio's. Zie voor meer informatie [Premium opslag: High-Performance-opslag voor Azure virtuele machines werkbelasting](../storage/storage-premium-storage.md).
- Schijven die zijn bijgevoegd bij virtuele machines zijn daadwerkelijk VHD-bestanden in een Azure opslag-account. Zie voor meer informatie [over schijven en VHD's voor virtuele machines](virtual-machines-linux-about-disks-vhds.md).
- Voor een nieuwe schijf moet u niet eerst maken omdat Azure gemaakt wanneer u deze koppelen.
- Voor een bestaande schijf moet het VHD-bestand beschikbaar zijn in een Azure opslag-account. U kunt een VHD die al is er, als deze niet gekoppeld aan een andere VM of upload uw eigen VHD van het bestand in de opslagruimte-account.


[AZURE.INCLUDE [virtual-machines-common-attach-disk-portal](../../includes/virtual-machines-common-attach-disk-portal.md)]

## <a name="next-steps"></a>Volgende stappen

Nadat de schijf is toegevoegd, moet u deze voorbereiden voor gebruik. Zien in de virtuele machine-besturingssysteem: "het: een nieuwe gegevensschijf in Linux geïnitialiseerd ' in dit [artikel](virtual-machines-linux-classic-attach-disk.md#how-to-initialize-a-new-data-disk-in-linux).
