<properties 
   pageTitle="Hoe verplaats ik een exemplaar van de rol van VM of naar een ander subnet"
   description="Meer informatie over het VMs en rol exemplaren verplaatsen naar een ander subnet"
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

# <a name="how-to-move-a-vm-or-role-instance-to-a-different-subnet"></a>Hoe verplaats ik een exemplaar van de rol van VM of naar een ander subnet

U kunt PowerShell gebruiken om te gaan van uw VMs uit één subnet naar een andere in hetzelfde virtuele netwerk (VNet). Exemplaren van de rol kunnen worden verplaatst door de CSCFG bewerken, in plaats van via PowerShell.

>[AZURE.NOTE] In dit artikel bevat informatie die is ten opzichte van Azure klassieke implementaties alleen.

Waarom VMs verplaatsen naar een ander subnet? Subnet migratie is nuttig wanneer het oudere subnet te klein is en kan niet worden uitgevouwen vanwege bestaande actieve VMs in dat subnet. In dat geval u kunt een nieuwe, grotere subnet maken en de VMs migreren naar het nieuwe subnet en nadat de migratie is voltooid, kunt u het oude lege subnet verwijderen.

## <a name="how-to-move-a-vm-to-another-subnet"></a>Hoe verplaats ik een VM naar een ander subnet

Als u wilt verplaatsen een VM, voert u de Set-AzureSubnet PowerShell-cmdlet, volgens het voorbeeld hieronder als een sjabloon. In het onderstaande voorbeeld wordt verplaatst TestVM uit een presenteren subnet, Subnet-2. Zorg ervoor dat u wilt bewerken in het voorbeeld zodat uw omgeving. Houd er rekening mee dat wanneer u de Update-AzureVM-cmdlet als onderdeel van een procedure uitvoeren, deze opnieuw wordt opgestart uw VM als onderdeel van het updateproces.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
  	| Set-AzureSubnet –SubnetNames Subnet-2 `
  	| Update-AzureVM

Als u een statische interne privé IP-adres voor uw VM hebt opgegeven, moet u deze instelling wissen voordat u de VM naar een nieuwe subnet verplaatsen kunt. Gebruik in dat geval het volgende:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Set-AzureSubnet -SubnetNames Subnet-2 `
  	| Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>Een exemplaar van de rol verplaatsen naar een ander subnet

Een exemplaar van de rol verplaatsen, het CSCFG-bestand te bewerken. In het onderstaande voorbeeld wordt verplaatst "Role0" in virtuele netwerk *VNETName* uit een presenteren subnet *Subnet-2*. Omdat het exemplaar van de rol is al geïmplementeerd, moet u alleen de naam van de Subnet wijzigen = Subnet-2. Zorg ervoor dat u wilt bewerken in het voorbeeld zodat uw omgeving.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
