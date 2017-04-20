<properties 
   pageTitle="Het instellen van een statische privé IP-adres in de klassieke modus via PowerShell | Microsoft Azure"
   description="Lidmaatschap statische privé IP-adressen (Spanningsdips) en hoe u ze kunt beheren in de klassieke modus en PowerShell"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-powershell"></a>Het instellen van een statische privé IP-adres (klassieke) in PowerShell

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel heeft betrekking op het implementatiemodel klassieke. U kunt ook [een statische privé IP-adres in het implementatiemodel resourcemanager-beheren](virtual-networks-static-private-ip-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Het onderstaande voorbeeld PowerShell opdrachten verwachten een eenvoudige omgeving al hebt gemaakt. Als u uitvoeren van de opdrachten wilt, zoals deze worden weergegeven in dit document, moet u eerst de testomgeving beschreven in [Create een VNet](virtual-networks-create-vnet-classic-netcfg-ps.md)maken.

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Controleren of een specifiek IP-adres beschikbaar is
Voer de volgende PowerShell-opdracht om te controleren of de IP-adres *192.168.1.101* beschikbaar in een VNet *TestVNet*met de naam is, en controleer of de waarde voor *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 192.168.1.101 

Verwachte output:

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Een statische privé IP-adres opgeven bij het maken van een VM
De onderstaande PowerShell-script Hiermee maakt u een nieuwe cloudservice *TestService*, met de naam en vervolgens opgehaald van een afbeelding uit Azure, een VM met de naam *DNS01* in de nieuwe cloudservice met de opgehaalde afbeeldingen maakt, de VM in een subnet *FrontEnd*benoemde sets en *192.168.1.7* ingesteld als een statische privé IP-adres voor de VM:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage | where {$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name DNS01 -InstanceSize Small -ImageName $image.ImageName |
      Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! |
      Set-AzureSubnet –SubnetNames FrontEnd |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      New-AzureVM -ServiceName TestService –VNetName TestVNet

Verwachte output:

    WARNING: No deployment found in service: 'TestService'.
    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    New-AzureService     fcf705f1-d902-011c-95c7-b690735e7412 Succeeded      
    New-AzureVM          3b99a86d-84f8-04e5-888e-b6fc3c73c4b9 Succeeded  

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Het statische IP-adres privégegevens voor een VM ophalen
Voer de volgende PowerShell-opdracht om weer te geven de statische privé IP-adresgegevens voor VM die zijn gemaakt met de bovenstaande script, en houd rekening met de waarden voor *IP-adres*:

    Get-AzureVM -Name DNS01 -ServiceName TestService

Verwachte output:

    DeploymentName              : TestService
    Name                        : DNS01
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 192.168.1.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : DNS01
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Een statische privé IP-adres uit een VM verwijderen
Toegevoegd aan de VM in het script bovenstaande, voer de volgende PowerShell-opdracht het statische privé IP-adres verwijderen:
    
    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Remove-AzureStaticVNetIP |
      Update-AzureVM

Verwachte output:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       052fa6f6-1483-0ede-a7bf-14f91f805483 Succeeded

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Een statische privé IP-adres toevoegen aan een bestaande VM
Een statische toevoegen privé IP-adres voor VM gemaakt met behulp van het script bovenstaande runt hij-opdracht:

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      Update-AzureVM

Verwachte output:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       77d8cae2-87e6-0ead-9738-7c7dae9810cb Succeeded 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [gereserveerde openbare IP-](virtual-networks-reserved-public-ip.md) adressen.
- Meer informatie over [openbare IP-(ILPIP) op gebruikersniveau exemplaar](virtual-networks-instance-level-public-ip.md) -adressen.
- Raadpleeg de [gereserveerde IP-REST API's](https://msdn.microsoft.com/library/azure/dn722420.aspx).
