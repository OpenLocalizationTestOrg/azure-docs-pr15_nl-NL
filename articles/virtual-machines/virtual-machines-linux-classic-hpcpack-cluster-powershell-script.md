<properties
   pageTitle="PowerShell-script om te implementeren Linux HPC cluster | Microsoft Azure"
   description="Een PowerShell script uitvoeren om een cluster Linux HPC Pack in Azure virtuele machines implementeren"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Een Linux-krachtige computing (HPC) cluster met het implementatiescript HPC Pack IaaS maken

De implementatie HPC Pack IaaS PowerShell-script om te implementeren van een volledige HPC-cluster voor Linux werkbelasting in Azure virtuele machines uitvoeren. Het cluster bestaat uit een met Windows Server en Microsoft HPC Pack Active Directory-die zijn gekoppeld hoofd knooppunt en berekeningscluster knooppunten die een van de Linux onderzoeken worden ondersteund door HPC Pack worden uitgevoerd. Als u implementeren van een cluster HPC Pack in Windows Azure voor Windows-werkbelastingen wilt, raadpleegt u [een Windows-HPC cluster met het implementatiescript HPC Pack IaaS maken](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). U kunt ook een resourcemanager Azure-sjabloon gebruiken om te implementeren van een cluster HPC Pack. Zie [maken een HPC-cluster met Linux knooppunten berekenen](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)voor een voorbeeld.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a>Voorbeeldconfiguratiebestand

Het volgende configuratiebestand maakt een nieuwe domeincontroller en een domein en een HPC Pack cluster met 1 hoofd knooppunt met lokale databases en 10 Linux berekeningscluster knooppunten implementeren. De cloudservices worden rechtstreeks in de locatie van Oost-Azië gemaakt. De Linux berekeningscluster knooppunten zijn gemaakt in 2 cloudservices en 2 opslag-accounts (dat wil zeggen _MyLnxCN-0001_ naar _MyLnxCN-0005_ in _MyLnxCNService01_ en _mylnxstorage01_) en _MyLnxCN-0006_ naar _MyLnxCN-0010_ in _MyLnxCNService02_ en _mylnxstorage02_. De knooppunten berekeningscluster worden gemaakt van een afbeelding OpenLogic CentOS versie 7.0 Linux. 

Vervangt door uw eigen waarden voor de naam van uw abonnement en de namen van de account en -service.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
    </DomainController>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a>Problemen oplossen

* **Fout 'VNet niet bestaat'** - als u de HPC Pack IaaS implementatiescript uitvoeren om meerdere clusters in Azure gelijktijdig onder één abonnement implementeren, een of meer implementaties kunnen mislukken met de fout ' VNet *VNet\_naam* bestaat niet ".
Als deze fout treedt op, voert u opnieuw het script voor de implementatie is mislukt.

* **Probleem toegang tot Internet uit het Azure virtuele netwerk** - als u een cluster HPC Pack met een nieuwe domeincontroller maken met behulp van de implementatiescript of als u handmatig een hoofd knooppunten VM naar domeincontroller verhogen, treden problemen op de VMs in het Azure virtuele netwerk verbinding met Internet. Dit kan gebeuren als een DNS-doorstuurservers server automatisch geconfigureerd op de domeincontroller en deze DNS-doorstuurservers server niet correct is opgelost.

    Dit probleem, meld u aan bij de domeincontroller en een van beide verwijderen de instelling van de configuratie doorstuurservers omzeilen of een geldige doorstuurservers DNS-server configureren. Klik hiertoe in Serverbeheer Klik op **Extra** >
    **DNS** te openen van DNS-beheer en dubbelklik vervolgens op **doorstuurservers**.
    
## <a name="next-steps"></a>Volgende stappen

* Zie [aan de slag met Linux berekeningscluster knooppunten in een cluster HPC Pack in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster.md) voor informatie over ondersteunde Linux onderzoeken, gegevens te verplaatsen en het versturen van taken aan een HPC Pack cluster met Linux knooppunten berekenen.
* Voor zelfstudies waarmee het script een cluster maken en uitvoeren van een werkbelasting Linux HPC, raadpleegt u:
    * [NAMD uitvoeren met Microsoft HPC Pack op Linux berekeningscluster knooppunten in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
    * [OpenFOAM uitvoeren met Microsoft HPC Pack op Linux berekeningscluster knooppunten in Azure wordt aangegeven](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)
    * [STER uitvoeren-CCM + met Microsoft HPC Pack op Linux knooppunten in Azure berekenen](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)
