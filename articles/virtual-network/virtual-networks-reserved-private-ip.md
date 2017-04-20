<properties 
   pageTitle="Het instellen van een statische IP voor interne privé"
   description="Lidmaatschap statische interne IP-adressen (Spanningsdips) en hoe u ze kunt beheren"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/22/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a>Het instellen van een statische interne privé IP-adres via PowerShell (klassieke)
In de meeste gevallen nodig u niet om op te geven van een statische interne IP-adres van uw virtuele computer. VMs in een virtueel netwerk ontvangt automatisch een interne IP-adres van een bereik dat u opgeeft. Maar in sommige gevallen een statische IP-adres opgeven voor een bepaalde VM relevant is. Stel dat uw VM gaat u naar DNS uitvoert of wordt een domeincontroller. Een statische interne IP-adres blijft met de VM zelfs via een stoppen/identiteitsgegevens staat. 

> [AZURE.IMPORTANT] Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [resourcemanager en klassiek](../resource-manager-deployment-model.md). In dit artikel beschreven hoe u met het implementatiemodel klassieke. Microsoft raadt meest nieuwe implementaties het [resourcemanager implementatiemodel](virtual-networks-static-private-ip-arm-ps.md)gebruiken.

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Controleren of een specifiek IP-adres beschikbaar is
Voer de volgende PowerShell-opdracht om te controleren of de IP-adres *10.0.0.7* beschikbaar in een vnet *TestVnet*met de naam is, en controleer of de waarde voor *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

>[AZURE.NOTE] Als u testen van de opdracht boven in het volgen van een veilige omgeving de richtlijnen in [een virtueel netwerk maken](virtual-networks-create-vnet-classic-portal.md) om te maken van een vnet met de naam *TestVnet* en zorg ervoor dat de adresruimte *10.0.0.0/8* wordt gebruikt wilt.

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a>Een statische interne IP opgeven bij het maken van een VM
De onderstaande PowerShell-script Hiermee maakt u een nieuwe cloudservice *TestService*met de naam, vervolgens opgehaald van een afbeelding uit Azure, en vervolgens Hiermee maakt u een VM met de naam *TestVM* in de nieuwe cloudservice met de opgehaalde afbeeldingen, wordt de VM in een subnet met de naam *Subnet-1*, ingesteld en *10.0.0.7* als een statische interne IP voor VM:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzureSubnet –SubnetNames Subnet-1 `
  	| Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
  	| New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a>Het ophalen van statische interne IP-informatie voor een VM
Als u het statische interne IP-informatie voor de VM die zijn gemaakt met de bovenstaande script, voer de volgende PowerShell-opdracht en houd rekening met de waarden voor *IP-adres*:

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
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

## <a name="how-to-remove-a-static-internal-ip-from-a-vm"></a>Een statische interne IP-adres uit een VM verwijderen
Als u wilt de statische interne IP toegevoegd aan de VM in het bovenstaande script verwijderen, voert u de volgende PowerShell-opdracht:
    
    Get-AzureVM -ServiceName TestService -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a>Hoe u een statische interne IP toevoegt aan een bestaande VM
Als u wilt toevoegen van een interne statische IP voor VM gemaakt met het script bovenstaande runt hij-opdracht:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
  	| Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
  	| Update-AzureVM

## <a name="next-steps"></a>Volgende stappen

[Gereserveerde IP](virtual-networks-reserved-public-ip.md)

[Exemplaar niveau openbare IP (ILPIP)](virtual-networks-instance-level-public-ip.md)

[Gereserveerde IP-REST API 's](https://msdn.microsoft.com/library/azure/dn722420.aspx)
 
