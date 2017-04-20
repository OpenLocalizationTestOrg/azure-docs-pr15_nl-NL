<properties
 pageTitle="Over computerintensieve VMs met Linux | Microsoft Azure"
 description="Achtergrondinformatie en overwegingen bij het gebruik van de grootte van de computerintensieve H-reeks en A8, A9 A10 en A11 voor Linux VMs"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Over H-reeks en computerintensieve A-reeks VMs 

Hier ziet u achtergrondinformatie en enkele aandachtspunten voor het gebruik van de nieuwere Azure H-reeks en de eerdere A8, A9 A10 en A11 grootte, ook bekend als *computerintensieve* exemplaren. In dit artikel bevat informatie over het gebruik van deze formaten voor Linux VMs. In dit artikel is ook beschikbaar voor [Windows VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md).




[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Toegang tot het netwerk RDMA

U kunt clusters van RDMA-ondersteuning Linux VMs dat uitvoeren een van de volgende ondersteunde Linux HPC onderzoeken en een ondersteunde MPI-implementatie om te profiteren van het netwerk Azure RDMA maken. Zie [een cluster Linux RDMA MPI toepassingen uit te voeren instelt](virtual-machines-linux-classic-rdma-cluster.md) voor distributieopties en steekproef configuratiestappen uit.

* **Onderzoeken** - moet u VMs implementeren van RDMA-ondersteuning SUSE Linux Enterprise Server (SLES) of OpenLogic CentOS gebaseerde HPC afbeeldingen in de Azure Marketplace. Alleen de volgende Marketplace afbeeldingen ondersteund de benodigde Linux RDMA-stuurprogramma's:

    * SLES 12 SP1 voor HPC SLES 12 SP1 voor HPC (Premium)
    
    * SLES 12 voor HPC SLES 12 voor HPC (Premium)
    
    * 7.1 HPC centOS gebaseerde
    
    * 6.5 HPC centOS gebaseerde
    
    >[AZURE.NOTE]Voor H-reeks VMs raden op een SLES 12 SP1 voor HPC afbeelding of afbeeldingen 7.1 HPC CentOS op.
    >
    >Klik op de afbeeldingen CentOS gebaseerde HPC kernel-updates in het configuratiebestand **yum** uitgeschakeld. Dit komt doordat de Linux RDMA-stuurprogramma's zijn verspreid als een pakket RPM en stuurprogramma updates niet werkt mogelijk als de kernel wordt bijgewerkt.

* **MPI** - Intel MPI bibliotheek 5.x

    Afhankelijk van de afbeelding Marketplace u kiest, afzonderlijke licentievoorwaarden, installatie, of de configuratie van Intel MPI mogelijk zijn er nodig, als volgt: 
    
    * **SLES 12 SP1 voor HPC afbeelding** - installatie het Intel MPI-pakketten verdeeld over de VM door de volgende opdracht uit te voeren:
    
            sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

    * **SLES 12 voor HPC afbeelding** - moet u afzonderlijk registreren als u wilt downloaden en installeren van Intel MPI. Zie de [installatiehandleiding voor Intel MPI bibliotheek](https://software.intel.com/sites/default/files/managed/7c/2c/intelmpi-2017-installguide-linux.pdf).
    
    * **Afbeeldingen centOS gebaseerde HPC** - Intel MPI 5.1 is al geïnstalleerd.  

    Aanvullende systeemconfiguratie is MPI taken uitvoeren op gegroepeerde VMs nodig. Klik op een cluster van VMs moet u bijvoorbeeld vertrouwensrelatie tussen de knooppunten berekeningscluster tot stand brengen. Zie [een cluster Linux RDMA MPI toepassingen uit te voeren instelt](virtual-machines-linux-classic-rdma-cluster.md)voor standaardinstellingen.


## <a name="considerations-for-hpc-pack-and-linux"></a>Overwegingen voor HPC Pack en Linux

[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft gratis HPC cluster en beheeroplossing voor taak, biedt één optie voor het gebruik van de exemplaren computerintensieve met Linux. De laatste releases van HPC Pack 2012 R2 ondersteuning verschillende Linux onderzoeken uitvoeren op berekenen knooppunten geïmplementeerd in Azure VMs, beheerd door een hoofd knooppunt van Windows Server. Met RDMA-ondersteuning Linux berekeningscluster knooppunten Intel MPI uitgevoerd, kunt HPC Pack plannen en uitvoeren Linux MPI toepassingen die toegang het netwerk RDMA tot. Zie [aan de slag met Linux berekeningscluster knooppunten in een cluster HPC Pack in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster.md)om te beginnen.

## <a name="network-topology-considerations"></a>Aandachtspunten bij het topologie van netwerk

* Klik op RDMA ingeschakelde Linux VMs in Azure wordt aangegeven, is Eth1 gereserveerd voor RDMA netwerkverkeer. Wijzig de instellingen van een Eth1 of gegevens in het configuratiebestand verwijzen naar dit netwerk niet. Eth0 is gereserveerd voor gewone Azure netwerkverkeer.

* IP via InfiniBand (i) wordt in Azure wordt aangegeven, niet ondersteund. Alleen RDMA via i wordt ondersteund.

## <a name="rdma-driver-updates-for-sles-12"></a>RDMA stuurprogramma updates voor SLES 12

Nadat u een VM op basis van een afbeelding SLES 12 HPC hebt gemaakt, moet u mogelijk de RDMA-stuurprogramma's op de VMs voor netwerkconnectiviteit RDMA bijwerken. 

>[AZURE.IMPORTANT]Deze stap is **vereist** voor SLES 12 voor HPC VM implementaties in alle Azure regio's. 
>Deze stap is niet nodig als u een SLES 12 SP1 voor HPC, 7.1 HPC CentOS gebaseerde of CentOS gebaseerde 6.5 HPC VM implementeert. 

Voordat u de stuurprogramma's bijwerken, stopt u alle **zypper** processen of processen die de SUSE cessies‑retrocessies-databases op de VM vergrendelen. Anders wordt de stuurprogramma's mogelijk niet correct bijgewerkt.  

Als u de Linux RDMA stuurprogramma's op elke VM bijwerken, voert u een van de volgende logicaverzamelingen van Azure CLI opdrachten uit vanaf de clientcomputer.

**SLES 12 voor HPC VM deze is ingericht in het implementatiemodel klassieke**

```
azure config mode asm

azure vm extension set <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

**SLES 12 voor HPC VM deze is ingericht in het implementatiemodel resourcemanager**

```
azure config mode arm

azure vm extension set <resource-group> <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

>[AZURE.NOTE]Het duurt enige tijd voor het installeren van de stuurprogramma's en de opdracht retourneert zonder uitvoer. Nadat de update, uw VM wordt opnieuw gestart en gereed voor gebruik in enkele minuten.

### <a name="sample-script-for-driver-updates"></a>Voorbeeldscript voor een bijgewerkte versie

Als u een cluster van SLES 12 voor HPC VMs hebt, kunt u de stuurprogrammaupdate script op alle knooppunten in uw cluster. Het volgende script worden bijvoorbeeld de stuurprogramma's in een cluster met 8 knooppunten bijgewerkt.

```

#!/bin/bash -x

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Plug in appropriate numbers in the for loop below.

for (( i=11; i<19; i++ )); do

# For VMs created in the classic deployment model, use the following command in your script.

azure vm extension set $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

# For VMs created in the Resource Manager deployment model, use the following command in your script.

# azure vm extension set <resource-group> $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

done

```


## <a name="next-steps"></a>Volgende stappen

* Zie [virtuele Machines prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux)voor meer informatie over de beschikbaarheid en prijzen van de grootte computerintensieve.

* Zie [grootten voor virtuele machines](virtual-machines-linux-sizes.md)voor opslagcapaciteit en schijf details.

* Zie [een cluster Linux RDMA MPI toepassingen uit te voeren](virtual-machines-linux-classic-rdma-cluster.md)om te beginnen implementatie en het gebruik van computerintensieve formaten met RDMA op Linux.


