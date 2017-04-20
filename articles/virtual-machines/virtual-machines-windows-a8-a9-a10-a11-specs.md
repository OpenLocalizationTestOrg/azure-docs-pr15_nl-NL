<properties
 pageTitle="Over computerintensieve VMs met Windows | Microsoft Azure"
 description="Achtergrondinformatie en overwegingen bij het gebruik van de Azure H-reeks en A8, A9 A10 en A11 computerintensieve grootte voor Windows VMs en cloud services"
 services="virtual-machines-windows, cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Over H-reeks en computerintensieve A-reeks VMs

Hier ziet u achtergrondinformatie en enkele aandachtspunten voor het gebruik van de nieuwere Azure H-reeks en de eerdere A8, A9 A10 en A11 exemplaren, ook bekend als *computerintensieve* exemplaren. In dit artikel bevat informatie over het gebruik van deze processen voor Windows VMs. In dit artikel is ook beschikbaar voor [Linux VMs](virtual-machines-linux-a8-a9-a10-a11-specs.md).


[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>Toegang tot het netwerk RDMA

U kunt clusters van de exemplaren die geschikt zijn voor het RDMA Windows Server maken en implementeren van een van de ondersteunde MPI-implementaties om te profiteren van de Azure RDMA-netwerk. Dit netwerk lage latentie, hoge gegevensdoorvoer is gereserveerd voor alleen MPI-verkeer is toegestaan.

* **Besturingssysteem**
    * **Virtuele machines** - Windows Server 2012 R2, Windows Server 2012
    * **Cloudservices** - Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 Gast OS familie

* **MPI** - Microsoft MPI ([MS-MPI) 2012 R2 of hoger, Intel MPI bibliotheek 5.x

De Microsoft-netwerk directe interface ondersteunde MPI-implementaties gebruiken voor communicatie tussen exemplaren. Zie [een Windows-RDMA cluster met HPC Pack MPI toepassingen uit te voeren instellen](virtual-machines-windows-classic-hpcpack-rdma-cluster.md) en [gebruiken met meerdere exemplaar taken aan het bericht Passing Interface (MPI) applicaties in Azure Batch](../batch/batch-mpi.md) voor installatieopties en steekproef configuratiestappen uit.


>[AZURE.NOTE]Klik op RDMA-ondersteuning computerintensieve VMs, moet de extensie HpcVmDrivers worden toegevoegd aan de VMs voor het installeren van Windows netwerk-stuurprogramma die nodig zijn voor RDMA connectivity. In de meeste implementaties, wordt automatisch de extensie HpcVmDrivers toegevoegd. Als u toevoegen van de extensie uzelf wilt, raadpleegt u [extensies VM beheren](virtual-machines-windows-classic-manage-extensions.md).

## <a name="considerations-for-hpc-pack-and-windows"></a>Overwegingen voor HPC Pack en Windows

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoft gratis HPC cluster en beheeroplossing voor taak, is niet vereist voor u voor het gebruik van de exemplaren computerintensieve met Windows Server. Het is echter één optie voor het maken van een berekeningscluster in Azure MPI op basis van een Windows-toepassingen en andere werkbelasting HPC uitvoeren. HPC Pack 2012 R2 en nieuwere versies krijg ik een runtimeomgeving voor MS-MPI dat het netwerk Azure RDMA als geïmplementeerd op RDMA-ondersteuning VMs kunt gebruiken.

Voor meer informatie en controlelijsten voor het gebruik van de exemplaren computerintensieve met HPC Pack op Windows Server, Zie [instellen op een Windows-RDMA cluster met HPC Pack MPI toepassingen uit te voeren](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).




## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de beschikbaarheid en prijzen van de grootte computerintensieve [virtuele Machines prijzen](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows) en [prijzen van Cloudservices](https://azure.microsoft.com/pricing/details/cloud-services/).

* Zie [grootten voor virtuele machines](virtual-machines-linux-sizes.md)voor opslagcapaciteit en schijf details.

* Zie [een Windows-RDMA cluster met HPC Pack MPI toepassingen uit te voeren](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)om te beginnen implementatie en het gebruik van computerintensieve exemplaren met HPC Pack in Windows.

* Zie voor informatie over het gebruik van A8 en A9 exemplaren MPI toepassingen uitvoeren met Azure Batch [Gebruik meerdere exemplaren taken bericht Passing Interface (MPI) toepassingen in Azure Batch uit te voeren](../batch/batch-mpi.md).
