<properties
 pageTitle="Linux HPC Pack cluster opties in de cloud | Microsoft Azure"
 description="Meer informatie over de opties met Microsoft HPC Pack maken en beheren van een Linux-krachtige computing (HPC) cluster in de cloud Azure"
 services="virtual-machines-linux,cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>Opties met HPC Pack maken en beheren van een HPC-cluster in Azure wordt aangegeven voor Linux werkbelasting

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

In dit artikel ligt de nadruk op Opties voor de HPC Pack gebruiken om uit te voeren Linux werkbelasting. Er zijn ook opties voor het uitvoeren van [Windows HPC werkbelasting met HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md).

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Een cluster HPC Pack uitvoeren in Azure VMs

### <a name="azure-templates"></a>Azure-sjablonen


* (Marketplace) [Cluster HPC Pack voor Linux werkbelasting](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)

* (Quickstart) [Een HPC cluster met Linux maken knooppunten berekenen](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)


### <a name="powershell-deployment-script"></a>PowerShell-script voor implementatie

* [Een Linux HPC-cluster met het implementatiescript HPC Pack IaaS maken](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Zelfstudies

* [Zelfstudie: Aan de slag met Linux berekeningscluster knooppunten in een cluster HPC Pack in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster.md)

* [Zelfstudie: Uitvoeren NAMD met Microsoft HPC Pack op Linux berekenen knooppunten in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Zelfstudie: Uitvoeren OpenFOAM met Microsoft HPC Pack op een cluster Linux RDMA in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Zelfstudie: Uitvoeren sterretje-CCM + met Microsoft HPC Pack op een RDMA Linux cluster in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)

### <a name="cluster-management"></a>Cluster management

* [Taken aan een cluster HPC Pack in Azure wordt aangegeven](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [Taakbeheer in HPC taalpakket](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="create-rdma-clusters-for-mpi-workloads"></a>RDMA clusters voor MPI werkbelasting maken

* [Zelfstudie: Uitvoeren OpenFOAM met Microsoft HPC Pack op een cluster Linux RDMA in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Instellen van een cluster Linux RDMA MPI toepassingen uit te voeren](virtual-machines-linux-classic-rdma-cluster.md)

