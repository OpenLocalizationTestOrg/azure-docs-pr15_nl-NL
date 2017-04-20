<properties
   pageTitle="VNet Peering met resourcemanager sjablonen maken | Microsoft Azure"
   description="Informatie over het maken van een virtueel netwerk peering met behulp van de sjablonen van de Resource-Manager."
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-vnet-peering-using-resource-manager-templates"></a>VNet Peering met resourcemanager sjablonen maken

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Volg de onderstaande stappen om een VNet peering met behulp van resourcemanager sjablonen hebt gemaakt:

1. Als u nooit Azure PowerShell gebruikt nog, raadpleegt u [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) en volg de instructies helemaal naar het einde aan te melden bij Azure en selecteert u uw abonnement.

    > [AZURE.NOTE] De PowerShell-cmdlet voor het beheren van VNet peering wordt geleverd bij [Azure PowerShell 1,6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. De tekst die hieronder ziet u de definitie van een VNet peering koppeling voor VNet1 naar VNet2, op basis van de bovenstaande scenario. Kopieer de onderstaande inhoud en sla deze op een bestand met de naam VNetPeeringVNet1.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }

3. Het gedeelte hierna ziet u de definitie van een VNet peering koppeling voor VNet2 naar VNet1, op basis van de bovenstaande scenario.  Kopieer de onderstaande inhoud en sla deze op een bestand met de naam VNetPeeringVNet2.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }

    Zoals u in de bovenstaande sjabloon, moet u er een paar configureerbare eigenschappen voor VNet peering zijn:

  	|Optie|Beschrijving|Standaard|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|De adresruimte van een peer-VNet is al dan niet deel uit van de tag virtual_network.|Ja|
  	|AllowForwardedTraffic|Opgeven of verkeer niet die afkomstig zijn van een peered VNet wordt geaccepteerd of verwijderd.|Nee|
  	|AllowGatewayTransit|Hiermee kunt u de peer VNet gebruik van de gateway VNet.|Nee|
  	|UseRemoteGateways|Gebruik van de peer VNet gateway. De peer VNet moet een gateway geconfigureerd en AllowGatewayTransit geselecteerd. U kunt deze optie niet gebruiken, hebt u een gateway geconfigureerd.|Nee|

    Elke koppeling in het VNet peering heeft de set met eigenschappen hierboven. U kunt bijvoorbeeld AllowVirtualNetworkAccess ingesteld op waar voor VNet peering koppeling VNet1 naar VNet2 en stel deze in op ONWAAR voor de VNet peering koppeling in de andere richting.


4. Als u wilt het sjabloonbestand implementeert, kunt u de cmdlet New-AzureRmResourceGroupDeployment maken of bijwerken van de implementatie uitvoeren. Raadpleeg voor meer informatie over het gebruik van resourcemanager sjablonen in dit [artikel](../resource-group-template-deploy.md).

        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all

    > [AZURE.NOTE] Vervang de groep naam en de sjabloon bronbestand zo nodig.

    Hieronder wordt een voorbeeld op basis van het bovenstaande scenario:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all

    Uitvoer ziet:

        DeploymentName      : VNetPeeringVNet1
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all

    Uitvoer ziet:

        DeploymentName      : VNetPeeringVNet2
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

5. Nadat de implementatie is voltooid, kunt u de onderstaande om weer te geven van de peering staat cmdlet uitvoeren:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2

    Uitvoer ziet:

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : VNet101
        VirtualNetworkName  : VNet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Nadat peering in dit scenario is gemaakt, moet u mogelijk zijn om te starten, de verbindingen vanuit een virtuele machine met een virtuele machine in beide VNets. Standaard AllowVirtualNetworkAccess waar is en de juiste ACL's als u wilt dat de communicatie tussen VNets VNet peering wordt inrichten. U kunt nog steeds netwerk beveiligingsregels groep (NSG) als u wilt blokkeren connectiviteit tussen specifieke subnetten of virtuele machines fijnmazige controle over access tussen twee virtuele netwerken toepassen.  Raadpleeg voor meer informatie voor het maken van regels voor NSG van dit [artikel](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Volg de onderstaande stappen om een VNet peering in abonnementen hebt gemaakt:

1. Aanmelden bij Azure met rechten gebruiker-A de van-account in abonnement-A en de volgende cmdlet uitvoeren:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

    Dit is niet een vereiste, peering kan worden vastgesteld evenzeer peering aanvragen gebruikers afzonderlijk voor de desbetreffende Vnets verhogen zo lang maken als de aanvragen overeenkomen. Een gebruiker met bevoegdheden van de andere VNet toevoegen als gebruikers in de lokale VNet gemakkelijker moet de configuratie.

2. Meld u aan bij Azure met bevoegdheden gebruiker-B van account voor abonnement-B en de volgende cmdlet uitvoeren:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. Gebruiker-A is in login sessie, deze cmdlet uitvoeren:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all

    Hier ziet u hoe de JSON-bestand wordt gedefinieerd.  

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }

4. In de gebruiker-B login sessie, kunt u de volgende cmdlet uitvoeren:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all

    Hier ziet u hoe de JSON-bestand wordt gedefinieerd:

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }

    Nadat peering in dit scenario is gemaakt, moet u mogelijk zijn om te starten de verbindingen vanaf een virtuele machine met een virtuele machine van beide VNets in verschillende abonnementen.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. In dit scenario kunt u de onderstaande tot stand brengen van de VNet peering voorbeeldsjabloon implementeren.  U moet de eigenschap AllowForwardedTraffic op True, waarmee het netwerk virtuele toestel in de peered VNet verzenden en ontvangen van verkeer instellen.

    Hier ziet u de sjabloon voor het maken van een VNet peering uit HubVNet naar VNet1. Houd er rekening mee dat AllowForwardedTraffic is ingesteld op onwaar.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }

2. Hier ziet u de sjabloon voor het maken van een VNet peering uit VNet1 naar HubVnet. Opmerking die AllowForwardedTraffic is ingesteld op waar.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }


3. Nadat peering is gemaakt, kunt u verwijzen naar dit [artikel](virtual-network-create-udr-arm-ps.md) voor de gebruiker gedefinieerde routes(UDR) om te leiden VNet1 verkeer door een virtueel toestel gebruik van de mogelijkheden definiëren. Als u het hopadres van de volgende in route opgeeft, stelt u deze naar het IP-adres van het virtuele toestel in de peer VNet HubVNet.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Als u wilt maken op een peering tussen virtuele netwerken van andere implementatiemodellen, de volgende stappen uit te voeren:
1. De onderstaande tekst wordt weergegeven van de definitie van een VNet peering koppeling voor VNET1 naar VNET2 in dit scenario. Slechts één koppeling is vereist voor een klassiek virtueel netwerk met een virtueel netwerk van Azure resource manager op hetzelfde niveau.

    Zorg ervoor dat u in uw abonnements-ID voor waarin de klassieke virtual network of VNET2 zich bevindt moeten worden geplaatst en wijzig MyResouceGroup in de naam van de desbetreffende resource-groep.

    {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0', 'parameters': {},"variabelen": {},"resources": [{"apiVersion":"2016-06-01","type":"Microsoft.Network/virtualNetworks/virtualNetworkPeerings","naam":" VNET1/LinkToVNET2","locatie":"[.location resourceGroup-()]","eigenschappen": {"allowVirtualNetworkAccess": waar,"allowForwardedTraffic": ONWAAR,"allowGatewayTransit": ONWAAR,"useRemoteGateways": ONWAAR,"remoteVirtualNetwork": {"-id":" [resourceId (' Microsoft.ClassicNetwork/virtualNetworks', 'VNET2')] "}}}]}

2. Als u wilt het sjabloonbestand implementeert, voert u de volgende cmdlet maken of bijwerken van de implementatie.

        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all

        Output shows:

        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

3. Nadat de implementatie is gemaakt, kunt u de volgende cmdlet om weer te geven van de peering staat kunt uitvoeren:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2

        Output shows:

        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

Nadat peering tussen een klassieke VNet en een resourcemanager VNet is gemaakt, moet u mogelijk zijn om te starten verbindingen vanuit een virtuele machine in VNET1 met een virtuele machine in VNET2 en vice versa.
