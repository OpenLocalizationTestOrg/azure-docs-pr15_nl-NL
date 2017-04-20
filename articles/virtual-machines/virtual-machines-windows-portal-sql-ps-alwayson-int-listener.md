<properties
   pageTitle="Altijd op beschikbaarheid van de groep Listeners – Microsoft Azure configureren"
   description="Beschikbaarheid van de groep Listeners configureren voor het model Azure resourcemanager, gebruikt een interne taakverdeling met een of meer IP-adressen."
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="10/20/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-one-or-more-always-on-availability-group-listeners---resource-manager"></a>Een of meer altijd op beschikbaarheid van de groep Listeners - resourcemanager configureren 

Dit onderwerp wordt uitgelegd hoe u twee dingen doen:

- Maak een interne taakverdeling voor SQL Server beschikbaarheidsgroepen met PowerShell-cmdlets.

- Extra IP-adressen toevoegen aan een taakverdeling ter ondersteuning van meer dan één SQL Server beschikbaarheid van de groep. 

In SQL Server wordt een groep beschikbaarheid van de naam van een virtueel netwerk die clients verbinding maken met toegang tot een database in de primaire of secundaire replica. Klik op Azure virtuele machines bevat een taakverdeling het IP-adres voor de luisteraar ervan af. De taakverdeling stuurt verkeer door naar het exemplaar van SQL Server die op de poort test luistert. Een beschikbaarheidsgroep gebruikt in de meeste gevallen een interne taakverdeling. Een Azure interne taakverdeling kunt een of meer IP-adressen hosten. Elk IP-adres gebruikt een specifieke test-poort. In dit document ziet hoe u PowerShell gebruiken om te maken van een nieuwe taakverdeling of IP-adressen toevoegen aan een bestaande taakverdeling voor SQL Server beschikbaarheid van de groepen. 

De mogelijkheid meerdere IP-adressen toewijzen aan een interne taakverdeling is nieuw in Azure en is alleen beschikbaar in resourcemanager model. Als u wilt deze taak uitvoert, moet u beschikken over een SQL Server beschikbaarheid van de groep geïmplementeerd op Azure virtuele machines in resourcemanager model. Beide SQL Server virtuele machines moet behoren tot dezelfde beschikbaarheid set. De [Microsoft-sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) kunt u de van de beschikbaarheidsgroep automatisch in Azure resourcemanager hebt gemaakt. Deze sjabloon wordt automatisch de van de beschikbaarheidsgroep, inclusief de interne taakverdeling voor u gemaakt. Als u liever, kunt u [een groep AlwaysOn beschikbaarheid handmatig configureren](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

In dit onderwerp is vereist dat uw beschikbaarheidsgroepen al zijn geconfigureerd.  

Verwante onderwerpen zijn:

- [AlwaysOn beschikbaarheid van groepen in Azure VM (grafische gebruikersinterface) configureren](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   

- [Een verbinding VNet-naar-VNet configureren met behulp van Azure resourcemanager en PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-vm-powershell.md)]

## <a name="configure-the-windows-firewall"></a>Windows Firewall configureren

Configureer de Windows-Firewall voor SQL Server-toegang. U moet de firewall voor TCP-verbindingen met het gebruik van de poorten door de SQL Server-instantie, evenals de poort die wordt gebruikt door de luisteraar ervan af-test configureren. Zie voor gedetailleerde instructies [configureren voor toegang tot de Database-Engine Windows Firewall](http://msdn.microsoft.com/library/ms175043.aspx#Anchor_1). Maak een binnenkomende regel voor de poort SQL Server en voor de poort test.

## <a name="example-script-create-an-internal-load-balancer-with-powershell"></a>Voorbeeldscript: Maak een interne taakverdeling met PowerShell

> [AZURE.NOTE] Als u de van de beschikbaarheidsgroep hebt gemaakt met de [Microsoft-sjabloon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) worden het selectievakje laden hoeft u niet aan deze stap uitvoeren. 

Het volgende PowerShell-script maakt een interne taakverdeling, configureert de regels van taakverdeling en stelt u een IP-adres voor de taakverdeling. U kunt het script uitvoeren open Windows filter wissen en het script in het deelvenster Script plakken. Gebruik `Login-AzureRMAccount` om aan te melden met PowerShell. Als u meerdere Azure abonnementen hebt, gebruikt u `Select-AzureRmSubscription ` voor het instellen van het abonnement. 

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<Resource Group Name>" # Resource group name
$VNetName = "<Virtual Network Name>"         # Virtual network name
$SubnetName = "<Subnet Name>"                # Subnet name
$ILBName = "<Load Balancer Name>"            # ILB name
$Location = "<Azure Region>"                 # Azure location
$VMNames = "<VM1>","<VM2>"                   # Virtual machine names

$ILBIP = "<n.n.n.n>"                         # IP address
[int]$ListenerPort = "<nnnn>"                # AG listener port
[int]$ProbePort = "<nnnn>"                   # Probe port

$LBProbeName ="ILBPROBE_$ListenerPort"       # The Load balancer Probe Object Name              
$LBConfigRuleName = "ILBCR_$ListenerPort"    # The Load Balancer Rule Object Name

$FrontEndConfigurationName = "FE_SQLAGILB_1" # Object name for the Front End configuration 
$BackEndConfigurationName ="BE_SQLAGILB_1"   # Object name for the Back End configuration

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 

$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName 

$FEConfig = New-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.id

$BEConfig = New-AzureRMLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName 

$SQLHealthProbe = New-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -Protocol tcp -Port $ProbePort -IntervalInSeconds 15 -ProbeCount 2

$ILBRule = New-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP 

$ILB= New-AzureRmLoadBalancer -Location $Location -Name $ILBName -ResourceGroupName $ResourceGroupName -FrontendIpConfiguration $FEConfig -BackendAddressPool $BEConfig -LoadBalancingRule $ILBRule -Probe $SQLHealthProbe 

$bepool = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $BackEndConfigurationName -LoadBalancer $ILB 

foreach($VMName in $VMNames)
    {
        $VM = Get-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VMName 
        $NICName = ($VM.NetworkInterfaceIDs[0].Split('/') | select -last 1)
        $NIC = Get-AzureRmNetworkInterface -name $NICName -ResourceGroupName $ResourceGroupName
        $NIC.IpConfigurations[0].LoadBalancerBackendAddressPools = $BEPool
        Set-AzureRmNetworkInterface -NetworkInterface $NIC
        start-AzureRmVM -ResourceGroupName $ResourceGroupName -Name $VM.Name 
    }
```

## <a name="example-script-add-an-ip-address-to-an-existing-load-balancer-with-powershell"></a>Voorbeeldscript: een IP-adres toevoegen aan een bestaande taakverdeling met PowerShell

Om meer dan één van de beschikbaarheidsgroep PowerShell aanvullend IP-adres toevoegen aan een bestaande taakverdeling te gebruiken. Elk IP-adres is een eigen taakverdeling regel, test poort en voorste poort vereist.

De front-end wordt de poort die toepassingen met verbinding maken met de SQL Server-instantie. IP-adressen voor de van de verschillende beschikbaarheidsgroepen kunnen dezelfde front-end poort gebruiken.

>[AZURE.NOTE] Elk IP-adres vereist voor SQL Server beschikbaarheidsgroepen, een specifieke test-poort. Als één IP-adres op een taakverdeling test poort 59999 gebruikt, kunnen geen andere IP-adressen op die taakverdeling bijvoorbeeld test poort 59999 gebruiken. 

- Limieten Zie voor informatie over taakverdeling **privé voorgrond beëindigen IP per taakverdeling** onder [Netwerkproblemen limieten - resourcemanager Azure](../azure-subscription-service-limits.md#azure-resource-manager-virtual-networking-limits).

- Groep limieten Zie voor informatie over de beschikbaarheid van de [beperkingen (beschikbaarheidsgroepen)](http://msdn.microsoft.com/library/ff878487.aspx#RestrictionsAG).

Het volgende script wordt een nieuwe IP-adres toegevoegd aan een bestaande taakverdeling. Werk de variabelen voor uw omgeving. De ILB de luisteraar ervan af poort voor de belasting taakverdeling front-end poort wordt gebruikt. Deze poort kan de poort die SQL Server luistert zijn. Voor de standaardexemplaren van SQL Server is dit poort 1433. De regel voor een beschikbaarheidsgroep van taakverdeling vereist een zwevende IP-adres (directe server afzender), zodat de back-end-poort hetzelfde als de front-end van poort is.

```powershell
# Login-AzureRmAccount
# Select-AzureRmSubscription -SubscriptionId <xxxxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx>

$ResourceGroupName = "<ResourceGroup>"          # Resource group name
$VNetName = "<VirtualNetwork>"                  # Virtual network name
$SubnetName = "<Subnet>"                        # Subnet name
$ILBName = "<ILBName>"                          # ILB name                      

$ILBIP = "<n.n.n.n>"                            # IP address
[int]$ListenerPort = "<nnnn>"                   # AG listener port
[int]$ProbePort = "<nnnnn>"                     # Probe port 

$ILB = Get-AzureRmLoadBalancer -Name $ILBName -ResourceGroupName $ResourceGroupName 

$count = $ILB.FrontendIpConfigurations.Count+1
$FrontEndConfigurationName ="FE_SQLAGILB_$count"  

$LBProbeName = "ILBPROBE_$count"
$LBConfigrulename = "ILBCR_$count"

$VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName 
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $VNet -Name $SubnetName

$ILB | Add-AzureRmLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -PrivateIpAddress $ILBIP -SubnetId $Subnet.Id 

$ILB | Add-AzureRmLoadBalancerProbeConfig -Name $LBProbeName  -Protocol Tcp -Port $Probeport -ProbeCount 2 -IntervalInSeconds 15  | Set-AzureRmLoadBalancer 

$ILB = Get-AzureRmLoadBalancer -Name $ILBname -ResourceGroupName $ResourceGroupName

$FEConfig = get-AzureRMLoadBalancerFrontendIpConfig -Name $FrontEndConfigurationName -LoadBalancer $ILB

$SQLHealthProbe  = Get-AzureRmLoadBalancerProbeConfig -Name $LBProbeName -LoadBalancer $ILB

$BEConfig = Get-AzureRmLoadBalancerBackendAddressPoolConfig -Name $ILB.BackendAddressPools[0].Name -LoadBalancer $ILB 

$ILB | Add-AzureRmLoadBalancerRuleConfig -Name $LBConfigRuleName -FrontendIpConfiguration $FEConfig  -BackendAddressPool $BEConfig -Probe $SQLHealthProbe -Protocol tcp -FrontendPort  $ListenerPort -BackendPort $ListenerPort -LoadDistribution Default -EnableFloatingIP | Set-AzureRmLoadBalancer   
```



## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Configureer het cluster het laden de verdeling van IP-adres gebruiken 

De volgende stap is de luisteraar ervan af op het cluster configureren en online de luisteraar ervan af te brengen. U doet dit door het volgende doen: 

1. De beschikbaarheid van de groep luisteraar ervan af op het failovercluster maken  

2. Online de luisteraar ervan af brengen

## <a name="1-create-the-availability-group-listener-on-the-failover-cluster"></a>1. de beschikbaarheid van de groep luisteraar ervan af op het failovercluster maken

In deze stap kunt u een client-toegangspunt toevoegen aan het failovercluster met Failoverclusterbeheer, en vervolgens PowerShell gebruiken voor het configureren van de resource cluster de test-poort op. 

1. Met RDP verbinding maken met de Azure virtuele machine waarop de primaire replica. 

2. Open Failoverclusterbeheer.

3. Selecteer het knooppunt **netwerken** en noteer de naam van het cluster-netwerk. Gebruik deze naam in de `$ClusterNetworkName` variabele in de PowerShell-script.

4. Naam van het cluster uitvouwen en klik vervolgens op **rollen**.

5. Klik in het deelvenster **rollen** met de rechtermuisknop op de naam van de beschikbaarheid van de groep en selecteer vervolgens **Toevoegen Resource** > **Client-toegangspunt**.

6. In het vak **naam** een naam voor deze nieuwe luisteraar ervan af, maken en klik tweemaal op **volgende** en klik vervolgens op **Voltooien**. Worden niet weergegeven in de luisteraar ervan af of de resource online op dit punt.
 
    De naam voor de nieuwe luisteraar ervan af is de naam van het netwerk waarmee toepassingen kunt verbinding maken met databases in de groep van de beschikbaarheid van SQL Server.

7. Klik op het tabblad **Resources** en klik uitvouwen van de Client toegangspunt dat u zojuist hebt gemaakt. Met de rechtermuisknop op de IP-resource en klik op Eigenschappen. Noteer de naam van het IP-adres. U gebruikt deze naam in de `$IPResourceName` variabele in de PowerShell-script.

8. Klik op **Statische IP-adres** en het statische IP-adres naar hetzelfde adres die u gebruikt wanneer u het laden de verdeling van IP-adres in de portal van Azure instellen onder **IP-adres** . 

9. NetBIOS uitschakelen voor dit adres en klik op **OK**. Herhaal deze stap voor elke resource IP-als uw oplossing zich uitstrekt over meerdere Azure VNets. 

10. Maak de groep beschikbaarheid van SQL Server-bron afhankelijk van het IP-adres. Rechtermuisknop op de resource in cluster manager, dit is op het tabblad **Resources** onder **Andere bronnen**. 

11. Op het knooppunt waarop momenteel de primaire replica, opent u een verhoogde PowerShell ISE en plak de volgende opdrachten in een nieuw script. Klik op het tabblad **afhankelijkheden** op de naam van de luisteraar ervan af.
 
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
    $IPResourceName = "<IPResourceName>" # the IP Address resource name
    $ILBIP = “<n.n.n.n>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    [int]$ProbePort = <nnnnn>

    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```
 
6. Werk de variabelen en de PowerShell script uitvoeren om het IP-adres en poorten configureren voor de nieuwe luisteraar ervan af.

 >[AZURE.NOTE] Als uw SQL-Servers in verschillende regio's zijn, moet u de PowerShell-script tweemaal uitvoeren. De eerste keer gebruikt de naam van het cluster, cluster IP-resourcenaam, en het laden van de verdeling van IP-adres uit de eerste resourcegroep. De tweede keer gebruik naam van het cluster, cluster IP-resourcenaam, en het laden van de verdeling van IP-adres uit de tweede resourcegroep.

Het cluster bevat nu een resource beschikbaarheid van de groep luisteraar ervan af.

## <a name="2-bring-the-listener-online"></a>2. weer online de luisteraar ervan af

Met de beschikbaarheid van de groep luisteraar ervan af resource geconfigureerd, u kunt doen om de luisteraar ervan af online zodat toepassingen kunnen verbinden met databases in de beschikbaarheidsgroep met de luisteraar ervan af.

1. Ga terug naar Failoverclusterbeheer. Vouw **rollen** en selecteer vervolgens de beschikbaarheid van de groep. Met de rechtermuisknop op de naam van de luisteraar ervan af op het tabblad **Resources** en klikt u op **Eigenschappen**.

1. Klik op het tabblad **afhankelijkheden** . Als er meerdere resources weergegeven, controleert u of de IP-adressen of niet hebt en, afhankelijkheden. Klik op **OK**.

1. Met de rechtermuisknop op de naam van de luisteraar ervan af en klik op **Online brengen**.

1. Wanneer de luisteraar ervan af online, het tabblad **Resources is** , met de rechtermuisknop op de van de beschikbaarheidsgroep en klik op **Eigenschappen**.

1. Maak een afhankelijkheid op de luisteraar ervan af naam resource (niet de IP-adres resources naam). Klik op **OK**.

1. SQL Server Management Studio starten en verbinding maken met de primaire replica.

1. Navigeer naar **de beschikbaarheid van de AlwaysOn hoog** | **beschikbaarheidsgroepen** | **groep beschikbaarheid Listeners**. 

1. U ziet nu de naam van de luisteraar ervan af die u hebt gemaakt in Failoverclusterbeheer. Met de rechtermuisknop op de naam van de luisteraar ervan af en klik op **Eigenschappen**.

1. Geef in het vak **poort** het poortnummer in de beschikbaarheid van de groep luisteraar ervan af met behulp van de $EndpointPort u eerder hebt gebruikt (1433 was de standaardinstelling), klikt u op **OK**.

U hebt nu een groep van de beschikbaarheid van SQL Server in Azure virtuele machines u resourcemanager afspeelt. 

## <a name="test-the-connection-to-the-listener"></a>De verbinding met de luisteraar ervan af testen

De verbinding testen:

1. RDP met een SQL Server die is in hetzelfde virtuele netwerk, maar niet de eigenaar de replica. Dit is de andere SQL Server in het cluster.

2. Hulpprogramma **sqlcmd** gebruiken om de verbinding te testen. Bijvoorbeeld het volgende script een verbinding tot stand **sqlcmd** aan de primaire replica tot en met de luisteraar ervan af met Windows-verificatie:

    ```
    sqlmd -S <listenerName> -E
    ```

    Als een poort locatie dan de standaardlocatie wordt gebruikt door de luisteraar ervan af poort (1433), geef de poort in de verbindingstekenreeks. Bijvoorbeeld de volgende opdracht uit sqlcmd is verbonden met een luisteraar ervan af bij poort 1435: 
    
    ```
    sqlcmd -S <listenerName>,1435 -E
    ```

De verbinding SQLCMD wordt automatisch verbinding met het ongeacht exemplaar van SQL Server de primaire replica gehost. 

>[AZURE.NOTE] Zorg ervoor dat de poort die u opgeeft geopend op de firewall van beide SQL-Servers is. Beide servers vereisen een binnenkomende regel voor het TCP-poorten die u gebruikt. Zie [toevoegen of Firewall-regel bewerken](http://technet.microsoft.com/library/cc753558.aspx) voor meer informatie. 

## <a name="guidelines-and-limitations"></a>Richtlijnen en beperkingen

Houd rekening met de volgende richtlijnen op beschikbaarheid van de groep luisteraar ervan af in Azure wordt aangegeven met een interne belasting voor documentconversies:

- Met een interne taakverdeling u alleen toegang tot de luisteraar ervan af vanuit binnen hetzelfde virtuele netwerk.

## <a name="for-more-information"></a>Voor meer informatie

Zie [configureren altijd op beschikbaarheid groeperen in Azure VM handmatig](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)voor meer informatie.

### <a name="powershell-cmdlets"></a>PowerShell-cmdlets

Gebruik de volgende PowerShell-cmdlets om te maken van een interne taakverdeling voor Azure virtuele machines.

- `New-AzureRmLoadBalancer`Hiermee maakt u een taakverdeling. Zie [Nieuw AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt619450.aspx) voor meer informatie. 

- `New-AzureRMLoadBalancerFrontendIpConfig`Hiermee maakt u een front IP-configuratie voor een taakverdeling. Zie [Nieuw AzureRMLoadBalancerFrontendIpConfig](http://msdn.microsoft.com/library/mt603510.aspx) voor meer informatie.

- `New-AzureRmLoadBalancerRuleConfig`Hiermee maakt u de configuratie van een regel voor een taakverdeling. Zie [Nieuw AzureRmLoadBalancerRuleConfig](http://msdn.microsoft.com/library/mt619391.aspx) voor meer informatie. 

- `New-AzureRMLoadBalancerBackendAddressPoolConfig`Hiermee maakt u een back-end-mailadres van toepassingen configuratie voor een taakverdeling. Zie [Nieuw AzureRmLoadBalancerBackendAddressPoolConfig](http://msdn.microsoft.com/library/mt603791.aspx) voor meer informatie. 

- `New-AzureRmLoadBalancerProbeConfig`Hiermee maakt u een test-configuratie voor een taakverdeling. Zie [Nieuw AzureRmLoadBalancerProbeConfig](http://msdn.microsoft.com/library/mt603847.aspx) voor meer informatie.

Als u een taakverdeling verwijderen uit een Azure resourcegroep wilt, gebruikt u `Remove-AzureRmLoadBalancer`. Zie [Verwijderen-AzureRmLoadBalancer](http://msdn.microsoft.com/library/mt603862.aspx)voor meer informatie.
