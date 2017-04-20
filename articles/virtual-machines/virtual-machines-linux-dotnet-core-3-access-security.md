<properties
   pageTitle="Toegang en beveiliging in Azure resourcemanager sjablonen | Microsoft Azure" 
   description="Azure virtuele machines DotNet Core zelfstudie"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="access-and-security-in-azure-resource-manager-templates"></a>Toegang en beveiliging in resourcemanager Azure-sjablonen

Toepassingen die zijn ingesloten in een Azure waarschijnlijk moeten zijn van access via internet of een VPN-verbinding / Express Route-verbinding met Azure. Met de voorbeeld van de muziek Store-toepassing, wordt de website beschikbaar gemaakt op internet met een openbare IP-adres. Met toegang tot stand gebracht moeten verbindingen met de toepassing en toegang tot de VM resources zelf worden beveiligd. Deze beveiliging in access wordt geleverd bij een beveiligingsgroep netwerk. 

Dit document een gedetailleerd overzicht van hoe de muziek Store-servicetoepassing in de steekproef resourcemanager Azure-sjabloon is beveiligd. Alle afhankelijkheden en unieke configuraties zijn gemarkeerd. Voor een optimale ervaring, vooraf een exemplaar van de oplossing aan uw Azure-abonnement en werk samen met de sjabloon Azure resourcemanager implementeren. De volledige sjabloon vindt u hier: [Muziek Store-implementatie op Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).


## <a name="public-ip-address"></a>Openbare IP-adres

Als u wilt openbare toegang bieden tot een Azure resource, kan een openbare IP-adres resource worden gebruikt. Openbare IP-adres kan worden geconfigureerd met een statische of dynamische IP-adres. Als een dynamisch adres wordt gebruikt, en de virtuele machine wordt gestopt en opgeheven, wordt de adressen wordt verwijderd. Wanneer de computer opnieuw wordt gestart, is het mogelijk dat deze een andere openbare IP-adres worden toegewezen. Als u wilt voorkomen dat een IP-adres wijzigen, kan een gereserveerde IP-adres worden gebruikt. 

Een openbare IP-adres kunnen worden toegevoegd aan een resourcemanager Azure-sjabloon met de Visual Studio nieuwe Resource Wizard toevoegen of door een geldig JSON invoegen in een sjabloon. 

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [Openbare IP-adres](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L121).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicipaddressName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "public-ip-front"
  },
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('publicipaddressDnsName')]"
    }
  }
}
```

Een openbare IP-adres kan worden gekoppeld aan een virtuele netwerkadapter of een taakverdeling. In dit voorbeeld dat omdat de website van muziek Store verdeeld over de verschillende virtuele machines is, het openbare IP-adres is gekoppeld aan de verdeling van de laden.

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [openbare IP-adres koppeling met taakverdeling](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).

```none
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicipaddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Het openbare IP-adres als een van de Azure-portal. Zoals u ziet dat het openbare IP-adres gekoppeld aan een taakverdeling en niet een virtuele machine is. Netwerk netwerktaakverdelers worden aangegeven in het volgende document van deze reeks.

![Openbare IP-adres](./media/virtual-machines-linux-dotnet-core/pubip.png)

Zie voor meer informatie over het openbare IP-adressen Azure, [IP-adressen in Azure wordt aangegeven](../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Beveiligingsgroep netwerk

Zodra u toegang tot stand is gebracht naar Azure bronnen, worden deze toegang beperkt. Voor Azure virtuele machines, beveiligde toegang gebeurt met een netwerk-beveiligingsgroep. Met de voorbeeld van de muziek Store-toepassing, alle toegang tot de virtuele machine is beperkt met uitzondering van via poort 80 voor http-toegang en poort 22 voor SSH-toegang. Een beveiligingsgroep netwerk kunnen worden toegevoegd aan een resourcemanager Azure-sjabloon met de Visual Studio nieuwe Resource Wizard toevoegen of door een geldig JSON invoegen in een sjabloon.

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager – [Netwerk beveiligingsgroep](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L68).

```none
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('nsgfront')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "nsg-front"
  },
  "properties": {
    "securityRules": [
      {
        "name": "http",
        "properties": {
          "description": "http endpoint",
          "protocol": "Tcp",
          "sourcePortRange": "*",
          "destinationPortRange": "80",
          "sourceAddressPrefix": "*",
          "destinationAddressPrefix": "*",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
      ........<truncated> 
    ]
  }
}
```

In dit voorbeeld is de beveiligingsgroep van netwerk koppelen aan het subnetobject in de bron Virtual Network zijn gedeclareerd. 

Volg deze koppeling om te zien van de steekproef JSON binnen de sjabloon resourcemanager- [koppeling van de beveiligingsgroep van netwerk met Virtual Network](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L158).


```none
"subnets": [
  {
    "name": "[variables('subnetName')]",
    "properties": {
      "addressPrefix": "10.0.0.0/24",
      "networkSecurityGroup": {
        "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
      }
    }
  }
```

Hier ziet u hoe de beveiligingsgroep van het netwerk van de Azure portal eruitziet. U ziet dat een NSG kunt worden koppelen aan een interface subnet en / of netwerk. In dit geval is de NSG gekoppeld aan een subnet. In deze configuratie wordt toepassen de binnenkomende regels op alle virtuele machines verbonden met het subnet.

![Beveiligingsgroep netwerk](./media/virtual-machines-linux-dotnet-core/nsg.png)

Zie [Wat is een beveiligingsgroep netwerk]( https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/)voor uitgebreide informatie over het netwerk beveiligingsgroepen.

## <a name="next-step"></a>Volgende stap

<hr>

[Stap 3: beschikbaarheid en schaal in Azure resourcemanager-sjablonen](./virtual-machines-linux-dotnet-core-4-availability-scale.md)
