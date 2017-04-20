<properties
   pageTitle="PowerShell-script om te implementeren Windows HPC cluster | Microsoft Azure"
   description="Een PowerShell script uitvoeren om een Windows HPC Pack cluster in Azure virtuele machines implementeren"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Een Windows high performance computing (HPC) cluster met het implementatiescript HPC Pack IaaS maken

De implementatie HPC Pack IaaS PowerShell-script om te implementeren van een volledige HPC-cluster voor Windows-werkbelastingen in Azure virtuele machines uitvoeren. Het cluster bestaat uit een in Active Directory-die zijn gekoppeld hoofd-knooppunt, met Windows Server en Microsoft HPC Pack en extra vensters berekenen bronnen die u opgeeft. Als u implementeren van een cluster HPC Pack in Azure wordt aangegeven voor Linux-werkbelastingen wilt, raadpleegt u [een Linux HPC cluster met het implementatiescript HPC Pack IaaS maken](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). U kunt ook een resourcemanager Azure-sjabloon gebruiken om te implementeren van een cluster HPC Pack. Zie [een HPC cluster maken](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) en [maken een HPC-cluster met een aangepaste afbeelding van het knooppunt berekenen](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/)voor voorbeelden.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Voorbeeld configuratiebestanden

In de volgende voorbeelden wordt vervangen door uw eigen waarden voor uw abonnement Id of naam en de namen van de account en -service.

### <a name="example-1"></a>Voorbeeld 1

Het volgende configuratiebestand implementeert een HPC Pack-cluster met een hoofd knooppunt met lokale databases en vijf knooppunten waarop het besturingssysteem Windows Server 2012 R2 berekenen. De cloudservices worden rechtstreeks in de locatie West VS gemaakt. Het hoofd knooppunt fungeert als domeincontroller van het domein.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a>Voorbeeld 2

Het volgende configuratiebestand implementeert een HPC Pack cluster in een bestaand domein. Het cluster heeft 1 hoofd knooppunt met lokale databases en 12 knooppunten berekenen met de extensie BGInfo VM is toegepast.
Automatische installatie van Windows-updates is uitgeschakeld voor alle VMs in het domein. De cloudservices worden rechtstreeks in de locatie van Oost-Azië gemaakt. De knooppunten berekeningscluster zijn gemaakt in drie cloudservices en drie opslag-accounts: _MyHPCCN-0001_ naar _MyHPCCN-0005_ in _MyHPCCNService01_ en _mycnstorage01_; _MyHPCCN-0006_ naar _MyHPCCN0010_ in _MyHPCCNService02_ en _mycnstorage02_; (en _MyHPCCN-0011_ naar _MyHPCCN-0012_ in _MyHPCCNService03_ en _mycnstorage03_). De knooppunten berekeningscluster worden gemaakt van een bestaande persoonlijke afbeelding uit een berekeningscluster knooppunt vastgelegd. Het automatisch groter en kleiner maken-service is ingeschakeld met standaard groter en kleiner maken intervallen.

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
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a>Voorbeeld 3

Het volgende configuratiebestand implementeert een HPC Pack cluster in een bestaand domein. Het cluster bevat één hoofd knooppunt, één database-server op een dvd van de gegevens 500 GB 2 makelaar knooppunten waarop het besturingssysteem Windows Server 2012 R2 wordt uitgevoerd en vijf berekeningscluster knooppunten waarop het besturingssysteem Windows Server 2012 R2 wordt uitgevoerd. De cloudservice MyHPCCNService is gemaakt in de groep affiniteit *MyIBAffinityGroup*en de andere cloudservices zijn gemaakt in de groep affiniteit *MyAffinityGroup*. De HPC taak Scheduler REST API en HPC web-portal zijn op het hoofd knooppunt ingeschakeld.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a>Voorbeeld 4

Het volgende configuratiebestand implementeert een HPC Pack cluster in een bestaand domein. Het cluster heeft twee hoofd knooppunt met lokale databases, twee Azure knooppunt sjablonen zijn gemaakt en drie grootte gemiddeld Azure knooppunten worden gemaakt voor Azure knooppunt sjabloon _AzureTemplate1_. Een script-bestand wordt uitgevoerd op het hoofd knooppunt nadat het hoofd knooppunt is geconfigureerd.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a>Problemen oplossen


* **Fout 'VNet niet bestaat'** - als u het script uitvoeren om meerdere clusters in Azure gelijktijdig onder één abonnement implementeren, een of meer implementaties kunnen mislukken met de fout ' VNet *VNet\_naam* bestaat niet ".
Als deze fout treedt op, voert u het script opnieuw voor de implementatie is mislukt.

* **Probleem toegang tot Internet uit het Azure virtuele netwerk** - als u een cluster met een nieuwe domeincontroller maken met behulp van de implementatiescript of als u handmatig een hoofd knooppunten VM naar domeincontroller verhogen, treden problemen op de VMs verbinding met Internet. Dit probleem kan optreden als een DNS-doorstuurservers server automatisch geconfigureerd op de domeincontroller en deze DNS-doorstuurservers server niet correct is opgelost.

    Dit probleem, meld u aan bij de domeincontroller en een van beide verwijderen de instelling van de configuratie doorstuurservers omzeilen of een geldige doorstuurservers DNS-server configureren. Als u wilt deze instelling configureert, klik in Serverbeheer op **Extra** >
    **DNS** te openen van DNS-beheer en dubbelklik vervolgens op **doorstuurservers**.

* **Probleem toegang krijgen tot RDMA netwerk van computerintensieve VMs** - als u Windows Server berekeningscluster toevoegen of broker knooppunt VMs een RDMA geschikte grootte zoals A8 of A9 gebruiken, kunt u waarnemen deze VMs voor problemen met het netwerk van de toepassing RDMA. Een reden dat dit probleem doet zich is als de extensie HpcVmDrivers niet juist is geïnstalleerd wanneer de VMs worden toegevoegd aan het cluster. Bijvoorbeeld de extensie vast in de installatie staat.

    U kunt dit probleem omzeilen, leest u eerst de status van de extensie in de VMs. Als de extensie niet correct is geïnstalleerd, verwijdert u de knooppunten uit het cluster HPC en voeg de knooppunten vervolgens weer toe. U kunt bijvoorbeeld berekeningscluster knooppunt VMs toevoegen door het script toevoegen-HpcIaaSNode.ps1 op het hoofd knooppunt uit te voeren.
    
## <a name="next-steps"></a>Volgende stappen

* Voer de werklast van een toets op het cluster. Zie de HPC Pack- [handleiding aan de slag](https://technet.microsoft.com/library/jj884144)voor een voorbeeld.

* Zie [aan de slag met een HPC Pack cluster in Azure wordt aangegeven om uit te voeren, Excel en SOA werkbelasting](virtual-machines-windows-excel-cluster-hpcpack.md)voor een zelfstudie script van de cluster-implementatie en de werklast van een HPC uitvoeren.

* Probeer HPC Pack van hulpprogramma's om te starten, stoppen, toevoegen en verwijderen van berekeningscluster knooppunten uit een cluster die u maakt. Zie [beheren berekenen knooppunten in een cluster HPC Pack in Azure wordt aangegeven](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md).

* Als u wilt instellen om in te dienen taken aan het cluster vanaf een lokale computer, raadpleegt u [taken indienen HPC vanaf een lokale computer aan een cluster HPC Pack in Azure wordt aangegeven](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).
