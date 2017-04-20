<properties
    pageTitle="Een ILB luisteraar ervan af voor altijd configureren op beschikbaarheidsgroepen | Microsoft Azure"
    description="Deze zelfstudie resources die zijn gemaakt met het implementatiemodel klassieke gebruikt en maakt een altijd op beschikbaarheid van de groep luisteraar ervan af in Azure wordt aangegeven met een interne laden verdeling (ILB)."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Een ILB luisteraar ervan af voor altijd op beschikbaarheid van groepen in Azure configureren

> [AZURE.SELECTOR]
- [Interne luisteraar ervan af](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Externe luisteraar ervan af](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="overview"></a>Overzicht

In dit onderwerp ziet u hoe u een luisteraar ervan af voor een altijd op beschikbaarheid-groep configureren met behulp van een **Interne laden verdeling (ILB)**.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Zie voor meer informatie over het configureren van een ILB luisteraar ervan af voor een groep altijd op beschikbaarheid in resourcemanager model [configureren een interne taakverdeling voor een groep van beschikbaarheid altijd op in Azure wordt aangegeven](virtual-machines-windows-portal-sql-alwayson-int-listener.md).


De beschikbaarheid van de groep kunnen replica's on-premises implementatie alleen Azure alleen, bevatten of zowel on-premises en Azure bij hybride configuraties's beslaan. Azure replica's kunnen zich bevinden binnen dezelfde regio of tussen meerdere regio's met meerdere virtuele netwerken (VNets). De onderstaande stappen wordt ervan uitgegaan dat u hebt al [een beschikbaarheidsgroep geconfigureerd](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) , maar niet een luisteraar ervan af hebt geconfigureerd.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Richtlijnen en beperkingen voor interne listeners
Houd rekening met de volgende richtlijnen op de luisteraar ervan af groep beschikbaarheid in Azure wordt aangegeven met ILB:

- De beschikbaarheid van de groep luisteraar ervan af wordt op Windows Server 2008 R2, Windows Server 2012 en Windows Server 2012 R2 ondersteund.

- Slechts één interne beschikbaarheid groep luisteraar ervan af wordt ondersteund per cloudservice, omdat de luisteraar ervan af is geconfigureerd voor het ILB, en er slechts één ILB per cloudservice. Het is echter mogelijk te maken van meerdere externe listeners. Zie [een externe luisteraar ervan af voor altijd op beschikbaarheid van groepen in Azure configureren](virtual-machines-windows-classic-ps-sql-ext-listener.md)voor meer informatie.

- Deze wordt niet ondersteund als u wilt maken van een interne luisteraar ervan af in dezelfde cloudservice waar u een externe luisteraar ervan af met de cloudservice openbare VIP ook hebt.

## <a name="determine-the-accessibility-of-the-listener"></a>De toegankelijkheid van de luisteraar ervan af bepalen

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

In dit artikel bevat informatie over het maken van een luisteraar ervan af met een **Interne laden verdeling (ILB)**. Als u een openbare/extern luisteraar ervan af nodig hebt, raadpleegt u de versie van dit artikel vindt u stappen voor het instellen van een [externe luisteraar ervan af](virtual-machines-windows-classic-ps-sql-ext-listener.md)

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>VM eindpunten verdeeld met directe server afzender maken

Voor ILB, moet u eerst de interne taakverdeling maken. Dit is in het onderstaande script.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Voor **ILB**, moet u een statische IP-adres toewijzen. De huidige VNet-configuratie eerst controleren door de volgende opdracht uit te voeren:

        (Get-AzureVNetConfig).XMLConfiguration

1. Noteer de naam **Subnet** voor het subnet waarin de VMs die als de replica's host. Hiermee wordt gebruikt in de parameter **$SubnetName** in het script.

1. Noteer de naam van de **VirtualNetworkSite** en de eerste **AddressPrefix** voor het subnet waarin de VMs die als de replica's host. Zoek naar een beschikbaar IP-adres door beide waarden doorgeven aan de opdracht **Test-AzureStaticVNetIP** en de **AvailableAddresses**onderzoeken. Als de VNet is met de naam *MyVNet* en hebt u er een subnetadres bereik die de slag op *172.16.0.128*en bijvoorbeeld de volgende opdracht zou een lijst met beschikbare adressen:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses

1. Kies een van de beschikbare adressen en deze gebruiken in de **$ILBStaticIP** -parameter van de volgende script.

3. Het volgende PowerShell-script kopiëren in een teksteditor en stel de waarden van variabelen aanpassen aan uw omgeving (Let erop dat standaardwaarden zijn opgegeven voor sommige parameters). Houd er rekening mee dat bestaande implementaties met affiniteit groepen ILB geen toevoegen. Zie voor meer informatie over ILB vereisten, [Interne taakverdeling](../load-balancer/load-balancer-internal-overview.md). Ook als uw beschikbaarheidsgroep 's Azure regio's beslaat, moet u het script eenmaal uitgevoerd in elke datacenter voor de cloudservice en knooppunten die zich in die datacenter bevinden.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that the replicas use in the VNet
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for the ILB in the subnet
        $ILBName = "AGListenerLB" # customize the ILB name or use this default value

        # Create the ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load balanced endpoint for each node in $AGNodes using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

1. Als u de variabelen hebt ingesteld, kopieert u het script in de teksteditor in uw Azure PowerShell-sessie uit te voeren. Als de vraag nog steeds >>, typt u ENTER opnieuw om ervoor te zorgen dat het script wordt gestart. Opmerking

>[AZURE.NOTE] De portal van Azure klassieke biedt geen ondersteuning van de verdeling van de interne belasting op dit moment, zodat u geen de ILB of de eindpunten in de portal van Azure klassieke ziet. **Get-AzureEndpoint** retourneert echter een interne IP-adres als de taakverdeling wordt uitgevoerd op is geïnstalleerd. Anders is het resultaat leeg.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Controleer of KB2854082 zo nodig is geïnstalleerd

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Open de firewallpoorten in de beschikbaarheid van de groepsknooppunten

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>De beschikbaarheid van de groep luisteraar ervan af maken

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Voor ILB, moet u het IP-adres van de interne laden verdeling (ILB) eerder hebt gemaakt. Gebruik het volgende script om deze IP-adres in PowerShell te verkrijgen.

        # Define variables
        $ServiceName="<MyServiceName>" # the name of the cloud service that contains the AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

1. Klik op een van de VMs, de PowerShell-script voor uw besturingssysteem kopiëren in een teksteditor en stel de variabelen op de waarden die u eerder hebt genoteerd.

    Gebruik het volgende script of hoger voor Windows Server 2012:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
        
    Gebruik het volgende script voor Windows Server 2008 R2:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB)

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255
    

1. Eenmaal u hebt de variabelen, een verhoogde Windows PowerShell-venster te openen en vervolgens het script in de teksteditor kopiëren en plakken in uw Azure PowerShell-sessie uit te voeren. Als de vraag nog steeds >>, typt u ENTER opnieuw om ervoor te zorgen dat het script wordt gestart.

2. Herhaal dit op elk VM. Dit script voor het configureren van de resource IP-adres met het IP-adres van de cloudservice en andere parameters wordt ingesteld als de test-poort. Wanneer de bron IP-adres online gebracht is, kunt deze vervolgens reageren op de peiling op de test-poort van het eindpunt taakverdeling eerder in deze zelfstudie hebt gemaakt.

## <a name="bring-the-listener-online"></a>Online de luisteraar ervan af brengen

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Items voor opvolgen

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Test de luisteraar ervan af groep beschikbaarheid (binnen de dezelfde VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
