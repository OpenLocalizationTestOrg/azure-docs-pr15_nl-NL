<properties 
    pageTitle="Gesimuleerd hybride cloud-testomgeving | Microsoft Azure" 
    description="Maken van een wolk gesimuleerd hybride omgeving voor IT pro- of ontwikkeling testen, met twee Azure virtuele netwerken en een verbinding VNet-naar-VNet." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-simulated-hybrid-cloud-environment-for-testing"></a>Een cloud gesimuleerd hybride omgeving voor het testen van instellen

In dit artikel wordt u stapsgewijs begeleid bij het maken van een wolk gesimuleerd hybride omgeving met Microsoft Azure door twee Azure virtuele netwerken te gebruiken. Hier ziet u de resulterende configuratie.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)

Hiermee wordt een hybride cloud-productieomgeving gesimuleerd en bestaat uit:

- Een gesimuleerd en vereenvoudigd on-premises netwerk die worden gehost op een Azure virtuele netwerk (het virtuele TestLab-netwerk).
- Een gesimuleerd cross-premises virtuele netwerk die worden gehost in Azure wordt aangegeven (TestVNET).
- Een VNet-naar-VNet verbinding tussen de twee virtuele netwerken.
- Een secundaire domeincontroller in de virtuele TestVNET-netwerk.

Dit biedt dat een basis- en algemene beginnend gegevenspunten uit die u kunt:

- Toepassingen ontwikkelen en testen in een cloud gesimuleerd hybride omgeving.
- Test-configuraties van computers, enkele binnen het virtuele netwerk TestLab en enkele binnen het TestVNET virtuele netwerk, kunt u simuleren hybride cloudgebaseerde IT werkbelasting maken.

Er zijn vier belangrijke fasen voor het instellen van deze hybride cloud-testomgeving:

1.  Het virtuele netwerk TestLab configureren.
2.  Maak het virtuele cross-premises-netwerk.
3.  De VNet-naar-VNet VPN-verbinding maken.
4.  DC2 configureren. 

Deze configuratie is vereist een Azure-abonnement. Als u een MSDN- of Visual Studio-abonnement hebt, raadpleegt u [Azure maandelijkse creditnota voor abonnees met Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

>[AZURE.NOTE] Virtuele machines en virtueel netwerkgateways in Azure wordt aangegeven kosten in rekening gebracht een lopend monetaire wanneer ze worden uitgevoerd. Deze kosten wordt gefactureerd ten opzichte van uw MSDN of dat is betaald abonnement. Een gateway Azure VPN wordt geïmplementeerd als een set van twee Azure virtuele machines. Als u wilt minimaliseren de kosten, de testomgeving maken en uitvoeren van de benodigde testen en demonstratie zo snel mogelijk.

## <a name="phase-1-configure-the-testlab-virtual-network"></a>Fase 1: Het virtuele netwerk TestLab configureren

Volg de instructies in het onderwerp [Base configuratie testomgeving](https://technet.microsoft.com/library/mt771177.aspx) de DC1, 1 en CLIENT1 computers in het Azure virtuele netwerk met de naam TestLab configureren. 

Start vervolgens een Azure PowerShell-prompt.

> [AZURE.NOTE] De volgende opdracht stelt gebruik Azure PowerShell 1.0 en hoger.

Meld u aan bij uw account.

    Login-AzureRMAccount

De naam van uw abonnement met de volgende opdracht krijgen.

    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName

Stel uw Azure-abonnement. Gebruik hetzelfde abonnement waarmee u uw grondtal configuratie in fase 1. Vervang alles in de aanhalingstekens, inclusief de < en > tekens op, met de juiste naam.

    $subscr="<subscription name>"
    Get-AzureRmSubscription –SubscriptionName $subscr | Select-AzureRmSubscription

Voeg een gateway-subnet vervolgens toe aan het TestLab virtuele netwerk van uw grondtal configuratie, die worden gebruikt voor het hosten van de Azure gateway.

    $rgName="<name of your resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $vnet=Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name TestLab
    Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 10.255.255.248/29 -VirtualNetwork $vnet
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

Vervolgens vraagt u een openbare IP-adres toewijzen aan de gateway voor het virtuele TestLab-netwerk.

    $gwpip=New-AzureRmPublicIpAddress -Name TestLab_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic

Maak vervolgens uw gateway.

    $vnet=Get-AzureRmVirtualNetwork -Name TestLab -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name TestLab_GWConfig -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 
    New-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Houd er rekening mee dat nieuwe gateways duurt 20 minuten of langer maken.

In de portal Azure op uw lokale computer verbinden met DC1 met de referenties CORP\User1. De CORP om domein te configureren zodat computers en gebruikers hun lokale domeincontroller voor verificatie gebruiken, moet u deze opdrachten uitvoeren met een beheerdersniveau Windows PowerShell-prompt op DC1.

    New-ADReplicationSite -Name "TestLab" 
    New-ADReplicationSite -Name "TestVNET"
    New-ADReplicationSubnet -Name "10.0.0.0/8" -Site "TestLab"
    New-ADReplicationSubnet -Name "192.168.0.0/16" -Site "TestVNET"

Dit is uw huidige configuratie.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph1.png)
 
## <a name="phase-2-create-the-testvnet-virtual-network"></a>Fase 2: Het virtuele netwerk TestVNET maken

Eerst het TestVNET virtuele netwerk maken en beveiligen met een netwerk-beveiligingsgroep.

    $rgName="<name of the resource group that you used for your TestLab virtual network>"
    $locName="<Azure location name where you placed the TestLab virtual network, such as West US>"
    $locShortName="<Azure location name from $locName in all lowercase letters with spaces removed. Example:  westus>"
    $testSubnet=New-AzureRMVirtualNetworkSubnetConfig -Name "TestSubnet" -AddressPrefix 192.168.0.0/24
    $gatewaySubnet=New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix 192.168.255.248/29
    New-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName -Location $locName -AddressPrefix 192.168.0.0/16 -Subnet $testSubnet,$gatewaySubnet –DNSServer 10.0.0.4
    $rule1=New-AzureRMNetworkSecurityRuleConfig -Name "RDPTraffic" -Description "Allow RDP to all VMs on the subnet" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389
    New-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName -Location $locShortName -SecurityRules $rule1
    $vnet=Get-AzureRMVirtualNetwork -ResourceGroupName $rgName -Name TestVNET
    $nsg=Get-AzureRMNetworkSecurityGroup -Name "TestSubnet" -ResourceGroupName $rgName
    Set-AzureRMVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet" -AddressPrefix 192.168.0.0/24 -NetworkSecurityGroup $nsg

Vervolgens vraagt u een openbare IP-adres worden toegewezen aan de gateway voor het virtuele netwerk TestVNET en de gateway te maken.

    $gwpip=New-AzureRmPublicIpAddress -Name TestVNET_pip -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $vnet=Get-AzureRmVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    $gwipconfig=New-AzureRmVirtualNetworkGatewayIpConfig -Name "TestVNET_GWConfig" -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id
    New-AzureRmVirtualNetworkGateway -Name "TestVNET_GW" -ResourceGroupName $rgName -Location $locName -IpConfigurations $gwipconfig -GatewayType Vpn -VpnType RouteBased

Dit is uw huidige configuratie.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph2.png)
 
##<a name="phase-3-create-the-vnet-to-vnet-connection"></a>Fase 3: De VNet-naar-VNet-verbinding maken

Eerst moet u een willekeurig, versleutelen sterke, 32 tekens vooraf gedeelde sleutel van de beheerder van uw netwerk of beveiliging. Gebruik u kunt ook de informatie op [een willekeurige tekenreeks voor een vooraf gedeelde sleutel IPsec maken](http://social.technet.microsoft.com/wiki/contents/articles/32330.create-a-random-string-for-an-ipsec-preshared-key.aspx) voor een vooraf gedeelde sleutel.

Gebruik deze opdrachten vervolgens de VNet-naar-VNet VPN-verbinding, die het enige tijd duren kan om uit te voeren maken.

    $sharedKey="<pre-shared key value>"
    $gwTestLab=Get-AzureRmVirtualNetworkGateway -Name TestLab_GW -ResourceGroupName $rgName
    $gwTestVNET=Get-AzureRmVirtualNetworkGateway -Name TestVNET_GW -ResourceGroupName $rgName
    New-AzureRmVirtualNetworkGatewayConnection -Name TestLab_to_TestVNET -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestLab -VirtualNetworkGateway2 $gwTestVNET -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey
    New-AzureRmVirtualNetworkGatewayConnection -Name TestVNET_to_TestLab -ResourceGroupName $rgName -VirtualNetworkGateway1 $gwTestVNET -VirtualNetworkGateway2 $gwTestLab -Location $locName -ConnectionType Vnet2Vnet -SharedKey $sharedKey

Na een paar minuten, moet de verbinding worden gemaakt.

Dit is uw huidige configuratie.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph3.png)
 
## <a name="phase-4-configure-dc2"></a>Fase 4: DC2 configureren

Maak eerst een virtuele machine voor DC2. Deze opdrachten bij de opdrachtprompt Azure PowerShell uitvoeren op uw lokale computer.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<the storage account name for the base configuration>"
    $vnet=Get-AzureRMVirtualNetwork -Name TestVNET -ResourceGroupName $rgName
    $pip=New-AzureRMPublicIpAddress -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name DC2-NIC -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -PrivateIpAddress 192.168.0.4
    $vm=New-AzureRMVMConfig -VMName DC2 -VMSize Standard_A1
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestVNET-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name ADDS-Data -DiskSizeInGB 20 -VhdUri $vhdURI  -CreateOption empty
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for DC2."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName DC2 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/DC2-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name DC2-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Vervolgens verbinding maken met de nieuwe DC2 virtuele machine vanuit de Azure-portal.

Een regel Windows Firewall dat verkeer is toegestaan voor eenvoudige connectiviteit testen vervolgens configureren. Vanaf een beheerdersniveau Windows PowerShell opdrachtprompt op DC2, moet u deze opdrachten uitvoeren.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

De opdracht ping moet leiden tot vier succesvolle antwoorden van IP-adres adres 10.0.0.4. Dit is een test verkeer via de verbinding VNet-naar-VNet.

De gegevensschijf extra op DC2 vervolgens toevoegen als een nieuw volume met de letter F:.

1.  In het linkerdeelvenster van Serverbeheer, klik op **bestand en Storage-Services**en klik vervolgens op **schijven**.
2.  Klik in het deelvenster inhoud in de groep **schijven** **schijf 2** (met de **Partition** ingesteld op **Onbekend**).
3.  Klik op **taken**en klik vervolgens op **NieuwVolume**.
4.  Klik op het vak voor pagina van de Wizard Nieuw Volume begint, klik op **volgende**.
5.  Klik op de pagina selecteren de server en schijf **schijf 2**op en klik vervolgens op **volgende**. Wanneer u wordt gevraagd, klikt u op **OK**.
6.  Klik op **volgende**op het opgeven van de grootte van de volume-pagina.
7.  Voer op de toewijzen aan een station letter of de map pagina, klikt u op **volgende**.
8.  Klik op **volgende**op de pagina selecteert u bestand systeem-instellingen.
9.  Klik op **maken**op de pagina bevestig selecties.
10. Als alles compleet is, klikt u op **sluiten**.

Configureer DC2 als een replicadomeincontroller voor het domein corp.contoso.com. Deze opdrachten uitvoeren met de Windows PowerShell-opdrachtprompt op DC2.

    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -Credential (Get-Credential CORP\User1) -DomainName "corp.contoso.com" -InstallDns:$true -DatabasePath "F:\NTDS" -LogPath "F:\Logs" -SysvolPath "F:\SYSVOL"

Houd er rekening mee dat u wordt gevraagd het wachtwoord CORP\User1 zowel een Directory Services herstellen modus wachtwoord opgeeft, en DC2 opgestart.

Nu dat het virtuele TestVNET-netwerk heeft een eigen DNS-server (DC2), moet u het virtuele netwerk TestVNET als u wilt deze DNS-server configureren.

1.  In het linkerdeelvenster van de Azure-portal, klik op het pictogram virtuele netwerken en klik vervolgens op **TestVNET**.
2.  Klik op het tabblad **Instellingen** op **DNS-servers**.
3.  Typ **192.168.0.4** als u wilt vervangen adres 10.0.0.4 in **primaire DNS-server**.
4.  Klik op **Opslaan**.

Dit is uw huidige configuratie. 

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sim/virtual-machines-windows-ps-hybrid-cloud-test-env-sim-ph4.png)
 
Uw gesimuleerd hybride omgeving in cloud is nu wilt testen.

## <a name="next-step"></a>Volgende stap

- Een [regel op het web, bedrijfstoepassing](virtual-machines-windows-ps-hybrid-cloud-test-env-lob.md) in deze omgeving instellen.
