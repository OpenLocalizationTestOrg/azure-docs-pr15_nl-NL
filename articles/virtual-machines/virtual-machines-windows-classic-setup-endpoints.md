<properties
    pageTitle="Instellen van de eindpunten van een klassieke Windows VM | Microsoft Azure"
    description="Leer hoe u eindpunten voor een Windows-VM in de portal van Azure klassieke instellen voor communicatie toestaan met een virtuele Windows-computer in Azure wordt aangegeven."
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

# <a name="how-to-set-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Het instellen van de eindpunten van een klassieke virtuele Windows-computer in Azure wordt aangegeven


Alle vensters van virtuele machines die u maakt in Azure wordt aangegeven met het implementatiemodel klassieke automatisch kunt communiceren via een privé netwerk kanaal met andere virtuele machines in een virtuele netwerk of dezelfde cloudservice. Computers op Internet of andere virtuele netwerken vereisen echter eindpunten naar het verkeer door de binnenkomende netwerk met een virtuele machine. In dit artikel is ook beschikbaar voor [Linux virtuele machines](virtual-machines-linux-classic-setup-endpoints.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]In het model van de implementatie **Resourcemanager** eindpunten geconfigureerd met **Beveiligingsgroepen netwerk (NSGs)**. Zie voor meer informatie, [toestaan externe toegang tot uw VM met behulp van de Portal Azure] (virtual-machines-windows-nsg-quickstart-portal.md).

Wanneer u een virtuele Windows-computer in de portal van Azure klassieke maakt, zijn algemene eindpunten zoals referenties voor extern bureaublad- en Windows PowerShell externe meestal automatisch voor u gemaakt. U kunt extra eindpunten configureren bij het maken van de virtuele machine of achteraf naar wens.



[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Volgende stappen

* Zie [Toevoegen-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx)voor informatie over het gebruik van een Azure PowerShell-cmdlet voor het instellen van een VM-eindpunt.

* Zie voor informatie over het gebruik van een Azure PowerShell-cmdlet voor het beheren van een ACL op een eindpunt [beheren toegangsbeheerlijsten (ACL's) voor eindpunten via PowerShell](../virtual-network/virtual-networks-acl-powershell.md).

* Als u een virtuele machine in het implementatiemodel resourcemanager hebt gemaakt, kunt u Azure PowerShell [beveiligingsgroepen netwerk maken](../virtual-network/virtual-networks-create-nsg-arm-ps.md) op het besturingselement verkeer voor VM.
