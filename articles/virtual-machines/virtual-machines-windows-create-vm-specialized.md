<properties
    pageTitle="Een kopie van uw Windows-VM maken | Microsoft Azure"
    description="Leer hoe u een kopie van uw gespecialiseerde Azure VM met Windows, in het implementatiemodel resourcemanager maken."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-specialized-vhd"></a>Een VM maken op basis van een gespecialiseerde VHD

Maak een nieuwe VM door het koppelen van een gespecialiseerde VHD als de OS schijf via Powershell. Een gespecialiseerde VHD onderhoudt de gebruikersaccounts, toepassingen en andere statusgegevens van uw oorspronkelijke VM. 

Als u een VM maken op basis van een generalized VHD wilt, raadpleegt u [een VM uit een generalized VHD-afbeelding maken](virtual-machines-windows-create-vm-generalized.md).

## <a name="create-the-subnet-and-vnet"></a>De subNet en vNet maken

De vNet en subNet van het [virtuele netwerk](../virtual-network/virtual-networks-overview.md)maken.

1. De subNet maken. In dit voorbeeld wordt gemaakt van een subnet **mySubNet**, in de groep resource **myResourceGroup**, met de naam en Hiermee stelt u het subnetvoorvoegsel-adres op **10.0.0.0/24**.

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```

2. Maak de vNet. In het volgende voorbeeld wordt de naam van het virtuele netwerk moeten **myVnetName**, de locatie op **West US**en het adresvoorvoegsel voor de virtuele netwerk **10.0.0.0/16**. 

    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-nic"></a>Een openbare IP-adres en NIC maken

Als u wilt kunnen communiceren met de virtuele machine in het virtuele netwerk, moet u een [openbare IP-adres](../virtual-network/virtual-network-ip-addresses-overview-arm.md) en een netwerkinterface.

1. Maak het openbare IP. In dit voorbeeld worden de naam van het openbare IP-adres is ingesteld op **myIP**.

    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. De NIC maken In dit voorbeeld is de naam van de NIC ingesteld op **myNicName**.

    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>De beveiligingsgroep van netwerk en een RDP-regel maken

Als u wilt kunnen aanmelden bij uw VM RDP gebruiken, moet u beschikken over een regel voor RDP toegang op poort 3389. Omdat de VHD voor de nieuwe VM is gemaakt van een bestaande kunt gespecialiseerde VM, nadat de VM u maakt een bestaand account uit de bron virtuele machine die hadden aan te melden met RDP gebruiken.

In het volgende voorbeeld wordt de naam van de NSG op **myNsg** en de naam van de regel RDP naar **myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```

Zie voor meer informatie over de eindpunten en NSG regels, [poorten openen voor een VM in Azure wordt aangegeven via PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md).

## <a name="create-the-vm-configuration"></a>De configuratie VM maken

De configuratie VM instellen om de gekopieerde VHD als de VHD OS te koppelen.


```powershell
    # Set the URI for the VHD that you want to use. In this example, the VHD file named "myOsDisk.vhd" is kept in a storage account named "myStorageAccount" in a container named "myContainer".
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    
    #Set the VM name and size. This example sets the VM name to "myVM" and the VM size to "Standard_A2".
    $vmName = "myVM"
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"

    #Add the NIC
    $vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

    #Add the OS disk by using the URL of the copied OS VHD. In this example, when the OS disk is created, the term "osDisk" is appened to the VM name to create the OS disk name. This example also specifies that this Windows-based VHD should be attached to the VM as the OS disk.
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
```


Als u gegevensschijven die moeten worden gekoppeld aan de VM hebt, moet u ook het volgende toevoegen: 

```powershell
    # Optional: Add data disks by using the URLs of the copied data VHDs at the appropriate Logical Unit Number (Lun).
    $dataDiskName = $vmName + "dataDisk"
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 0 -CreateOption attach
```

De gegevens en besturingssysteem schijf-URL's er ongeveer zo uitzien: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. U kunt dit op de portal vinden door te bladeren naar de doelcontainer-opslag, te klikken op het besturingssysteem of gegevens VHD die is gekopieerd en vervolgens de inhoud van de URL te kopiëren.


## <a name="create-the-vm"></a>De VM maken

Maak de VM de configuraties die we zojuist hebt gemaakt.

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

Als deze opdracht geslaagd is, ziet u uitvoer als volgt:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
 
```
 
## <a name="verify-that-the-vm-was-created"></a>Controleer of de VM is gemaakt 
 
U moet de zojuist gemaakte VM ziet in de [portal van Azure](https://portal.azure.com)onder **Zoeken** > **virtuele machines**, of met behulp van de volgende PowerShell-opdrachten:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Volgende stappen

Om aan te melden bij uw nieuwe virtuele machine, blader naar de VM in de [portal](https://portal.azure.com), op **verbinding maken**en het externe bureaublad RDP-bestand openen. De referenties van uw oorspronkelijke virtuele machine te melden bij uw nieuwe virtuele machine gebruikt. Lees [hoe u contact kunt leggen en meld u aan bij een Azure virtuele machines waarop Windows wordt uitgevoerd](virtual-machines-windows-connect-logon.md)voor meer informatie.







