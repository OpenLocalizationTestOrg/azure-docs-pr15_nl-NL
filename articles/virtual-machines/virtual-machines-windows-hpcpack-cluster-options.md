<properties
 pageTitle="Windows HPC Pack cluster opties in de cloud | Microsoft Azure"
 description="Meer informatie over de opties met Microsoft HPC Pack maken en beheren van een Windows-krachtige computing (HPC) cluster in de cloud Azure"
 services="virtual-machines-windows,cloud-services,batch"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-a-windows-hpc-cluster-in-azure"></a>Opties met HPC Pack maken en beheren van een Windows-HPC-cluster in Azure wordt aangegeven

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

In dit artikel ligt de nadruk op Opties voor het maken van HPC Pack clusters om uit te voeren Windows werkbelasting. Er zijn ook opties voor het maken van clusters [Linux HPC werkbelasting met HPC Pack](virtual-machines-linux-hpcpack-cluster-options.md)uitvoeren.


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Een cluster HPC Pack uitvoeren in Azure VMs

### <a name="azure-templates"></a>Azure-sjablonen

* (Marketplace) [Cluster HPC Pack voor Windows-werkbelastingen](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)

* (Marketplace) [Cluster HPC Pack voor Excel-werkbelastingen](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)

* (Quickstart) [Een HPC cluster maken](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)

* (Quickstart) [Een HPC cluster met aangepaste berekeningscluster knooppunt afbeelding maken](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Azure VM afbeeldingen

* [HPC Pack op Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)

* [HPC Pack berekeningscluster knooppunt op Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)

* [HPC Pack berekenen knooppunt met Excel voor Windows Server 2012 R2](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)



### <a name="powershell-deployment-script"></a>PowerShell-script voor implementatie

* [Een HPC-cluster met het implementatiescript HPC Pack IaaS maken](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Zelfstudies

* [Zelfstudie: Aan de slag met een HPC Pack cluster in Azure wordt aangegeven om uit te voeren, Excel en SOA werkbelasting](virtual-machines-windows-excel-cluster-hpcpack.md)



### <a name="manual-deployment-with-the-azure-portal"></a>Handmatige implementatie in de portal van Azure

* [Het hoofd knooppunt van een HPC Pack cluster in een VM Azure instellen](virtual-machines-windows-hpcpack-cluster-headnode.md)

### <a name="cluster-management"></a>Cluster management

* [Berekeningscluster knooppunten in een cluster HPC Pack in Azure beheren](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)

* [Groter en kleiner maken van Azure berekeningscluster resources in een cluster HPC Pack](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md)

* [Taken aan een cluster HPC Pack in Azure wordt aangegeven](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Taakbeheer in HPC taalpakket](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="add-worker-role-nodes-to-an-hpc-pack-cluster"></a>Werknemer rol knooppunten toevoegen aan een cluster HPC Pack


* [Burst in Azure werknemer exemplaren met HPC Pack](https://technet.microsoft.com/library/gg481749.aspx)

* [Zelfstudie: Een hybride cluster met HPC Pack in Azure instellen](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)

* [Azure "burst" knooppunten toevoegen aan een kop knooppunt HPC Pack in Azure wordt aangegeven](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md)


## <a name="integrate-with-azure-batch"></a>Integreren met Azure Batch 

* [Burst aan Azure Batch met HPC Pack](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>RDMA clusters voor MPI werkbelasting maken

* [Instellen van een Windows-RDMA cluster met HPC Pack MPI toepassingen uit te voeren](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)
