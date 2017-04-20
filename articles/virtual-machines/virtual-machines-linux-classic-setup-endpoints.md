<properties
    pageTitle="Instellen van de eindpunten van een klassieke Linux VM | Microsoft Azure"
    description="Leer hoe u eindpunten voor een VM Linux in de portal van Azure klassieke instellen voor communicatie toestaan met een Linux virtuele machine in Azure wordt aangegeven"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/13/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Het instellen van de eindpunten van een Linux klassieke virtuele machine in Azure wordt aangegeven

Alle Linux virtuele machines die u maakt in Azure wordt aangegeven met het implementatiemodel klassieke communiceren automatisch via een kanaal privé netwerk met andere virtuele machines in een virtuele netwerk of dezelfde cloudservice. Computers op Internet of andere virtuele netwerken vereisen echter eindpunten naar het verkeer door de binnenkomende netwerk met een virtuele machine. In dit artikel is ook beschikbaar voor [Windows virtuele machines](virtual-machines-windows-classic-setup-endpoints.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]In het model van de implementatie **Resourcemanager** eindpunten geconfigureerd met **Beveiligingsgroepen netwerk (NSGs)**. Zie voor meer informatie, [poorten openen en eindpunten] (virtual-machines-linux-nsg-quickstart.md).

Wanneer u een Linux virtuele machine in de portal van Azure klassieke maakt, is een eindpunt voor Secure Shell (SSH) meestal voor u automatisch gemaakt. U kunt extra eindpunten configureren bij het maken van de virtuele machine of achteraf naar wens.
 

[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Volgende stappen

* U kunt ook een VM-eindpunt maken met behulp van de [Interface van Azure-opdrachtregel](../virtual-machines-command-line-tools.md). Hiermee voert u de opdracht **azure vm eindpunt maken** .

* Als u een virtuele machine in het implementatiemodel resourcemanager hebt gemaakt, kunt u de CLI Azure in de modus resourcemanager [beveiligingsgroepen netwerk maken](../virtual-network/virtual-networks-create-nsg-arm-cli.md) op het besturingselement verkeer voor VM.