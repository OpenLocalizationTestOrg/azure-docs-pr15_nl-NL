<properties
   pageTitle="Open poorten voor een VM via PowerShell | Microsoft Azure"
   description="Leer hoe u een poort openen / maken van een eindpunt voor uw Windows-VM gebruik van de modus voor implementatie van Azure resource manager en Azure PowerShell"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Het openen van poorten en eindpunten naar een VM in Azure wordt aangegeven via PowerShell
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Snelle opdrachten
Als een netwerk-beveiligingsgroep en ACL regels wilt maken, moet u [de nieuwste versie van Azure PowerShell is geïnstalleerd](../powershell-install-configure.md). U kunt ook de [volgende stappen uit met behulp van de Azure portal uitvoeren](virtual-machines-windows-nsg-quickstart-portal.md).

Meld u aan bij uw Azure-account:

```powershell
Login-AzureRmAccount
```

In de volgende voorbeelden, kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen `myResourceGroup`, `myNetworkSecurityGroup`, en `myVnet`.

Een regel maken. Het volgende voorbeeld wordt een regel met de naam `myNetworkSecurityGroupRule` toe te staan dat TCP-verkeer is toegestaan op poort 80:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" -Access "Allow" -Protocol "Tcp" -Direction "Inbound" `
    -Priority "100" -SourceAddressPrefix "Internet" -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 80
```

Vervolgens uw netwerk-beveiligingsgroep maken en toewijzen van de HTTP-regel die u zojuist hebt gemaakt, als volgt. Het volgende voorbeeld wordt de beveiligingsgroep van een netwerk met de naam `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNetworkSecurityGroup" -SecurityRules $httprule
```

Nu we de beveiligingsgroep van uw netwerk toewijzen aan een subnet. Het volgende voorbeeld wordt een bestaand virtuele netwerk met de naam `myVnet` naar de variabele `$vnet`:

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Uw netwerk-beveiligingsgroep koppelen aan uw subnet. Het volgende voorbeeld wordt het subnet met de naam `mySubnet` met uw netwerk-beveiligingsgroep:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Tot slot bijwerken uw virtual netwerk in om uw wijzigingen zijn doorgevoerd:

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Meer informatie over het netwerk beveiligingsgroepen
De snelle opdrachten hier kunnen u aan de slag met verkeer die doorloopt naar uw VM. Beveiligingsgroepen netwerk bieden veel handige functies en granulatie voor toegang tot uw resources beheren. U kunt meer informatie over het [maken van een beveiligingsgroep netwerk en hier ACL regels](../virtual-network/virtual-networks-create-nsg-arm-ps.md).

U kunt netwerk-beveiligingsgroepen en ACL regels definiëren als onderdeel van Azure resourcemanager sjablonen. Lees meer over het [maken van netwerk beveiligingsgroepen met sjablonen](../virtual-network/virtual-networks-create-nsg-arm-template.md).

Als u gebruiken poort doorverbinden wilt naar een unieke externe poort toewijzen aan een interne poort op uw VM, gebruikt u een taakverdeling en regels voor vertaling (Network Address Translation). U wilt bijvoorbeeld TCP poort 8080 extern en hebben verkeer naar TCP-poort 80 op een VM wordt gestuurd. U kunt meer informatie over het [maken van een verdeling van de belasting internetgerichte](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld kunt u een eenvoudige regel als u wilt dat HTTP-verkeer is toegestaan gemaakt. Hier vindt u informatie over het maken van meer gedetailleerde omgevingen in de volgende artikelen:

- [Azure resourcemanager-overzicht](../azure-resource-manager/resource-group-overview.md)
- [Wat is een netwerk beveiliging groep (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure resourcemanager overzicht voor netwerktaakverdelers](../load-balancer/load-balancer-arm.md)