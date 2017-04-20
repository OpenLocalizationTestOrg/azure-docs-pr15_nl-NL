<properties 
   pageTitle="Beheren via PowerShell in resourcemanager NSGs | Microsoft Azure"
   description="Informatie over het beheren van de huidige NSGs via PowerShell in resourcemanager"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-powershell"></a>NSGs via PowerShell beheren

[AZURE.INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a>Gegevens ophalen

U kunt bekijken van uw bestaande NSGs, regels ophalen voor een bestaande NSG en ontdek wat resources een NSG is gekoppeld aan.

### <a name="view-existing-nsgs"></a>Bestaande NSGs weergeven
Uitvoeren om weer te geven alle bestaande NSGs in een abonnement, de `Get-AzureRmNetworkSecurityGroup` cmdlet zoals hieronder wordt weergegeven.

    Get-AzureRmNetworkSecurityGroup

Verwacht resultaat:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
                            
    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


Uitvoeren om de lijst met NSGs weergeven in een bepaalde resourcegroep, de `Get-AzureRmNetworkSecurityGroup` cmdlet zoals hieronder wordt weergegeven. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG

Verwachte output:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
         
### <a name="list-all-rules-for-an-nsg"></a>Alle regels voor een NSG weergeven

Uitvoeren om weer te geven van de regels van een NSG **NSG-FrontEnd**met de naam, de `Get-AzureRmNetworkSecurityGroup` cmdlet zoals hieronder wordt weergegeven. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules

Verwachte output:
    
    Name                     : rdp-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound
    
    Name                     : web-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

>[AZURE.NOTE] U kunt ook `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` voor een overzicht van de standaardregels uit de **NSG-FrontEnd** NSG.

### <a name="view-nsgs-associations"></a>NSGs koppelingen weergeven

Als u wilt weergeven wat de NSG **NSG-FrontEnd is** resources koppelen aan, voer de `Get-AzureRmNetworkSecurityGroup` cmdlet zoals hieronder wordt weergegeven.

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd

Zoekt u de eigenschappen **NetworkInterfaces** en **subnetten** zoals hieronder wordt weergegeven:

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

In het bovenstaande voorbeeld de NSG is niet gekoppeld aan een netwerkinterfaces (NIC's) en is gekoppeld aan een benoemde **FrontEnd**subnet.

## <a name="manage-rules"></a>Regels beheren

U kunt regels toevoegen aan een bestaande NSG, bestaande regels bewerken en verwijderen van regels.

### <a name="add-a-rule"></a>Een regel toevoegen

Als u wilt toevoegen van een regel voor **binnenkomende** verkeer vanaf een willekeurige computer naar de **NSG-FrontEnd** NSG mogen poort **443** , de onderstaande stappen uit te voeren.

1. Voer de `Get-AzureRmNetworkSecurityGroup` cmdlet kunt ophalen van de bestaande NSG en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Voer de `Add-AzureRmNetworkSecurityRuleConfig` cmdlet, zoals hieronder wordt weergegeven.

        Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange * `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. De wijzigingen hebt aangebracht in de NSG op te slaan uitvoeren de `Set-AzureRmNetworkSecurityGroup` cmdlet zoals hieronder wordt weergegeven.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Verwachte output met alleen de beveiligingsregels:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a>Een regel wijzigen

Als u de regel toe te staan dat binnenkomende verkeer van **Internet** alleen hiervoor hebt gemaakt, voert u de onderstaande stappen.

1. Voer de `Get-AzureRmNetworkSecurityGroup` cmdlet kunt ophalen van de bestaande NSG en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Voer de `Set-AzureRmNetworkSecurityRuleConfig` cmdlet, zoals hieronder wordt weergegeven.

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange Internet `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. De wijzigingen hebt aangebracht in de NSG op te slaan uitvoeren de `Set-AzureRmNetworkSecurityGroup` cmdlet zoals hieronder wordt weergegeven.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Verwachte output met alleen de beveiligingsregels:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a>Een regel verwijderen

1. Voer de `Get-AzureRmNetworkSecurityGroup` cmdlet kunt ophalen van de bestaande NSG en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Voer de `Remove-AzureRmNetworkSecurityRuleConfig` cmdlet, zoals hieronder wordt weergegeven.

        Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule

3. De wijzigingen hebt aangebracht in de NSG op te slaan uitvoeren de `Set-AzureRmNetworkSecurityGroup` cmdlet zoals hieronder wordt weergegeven.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Verwachte output met alleen de beveiligingsregels, kennisgeving de **https-regel** niet meer wordt vermeld:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a>Koppelingen beheren

U kunt een NSG naar subnetten en NIC's koppelen. U kunt ook een NSG uit een willekeurige deze gekoppeld aan bron koppeling.

### <a name="associate-an-nsg-to-a-nic"></a>Een NSG naar een NIC koppelen

Als u wilt koppelen het **NSG-FrontEnd** NSG naar de **TestNICWeb1** NIC, de onderstaande stappen uit te voeren.

1. Voer de `Get-AzureRmNetworkSecurityGroup` cmdlet kunt ophalen van de bestaande NSG en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Voer de `Get-AzureRmNetworkInterface` cmdlet kunt ophalen van de bestaande NIC en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. Stel de eigenschap **NetworkSecurityGroup** van de variabele **NIC** aan de waarde van de variabele **NSG** , zoals hieronder wordt weergegeven.

        $nic.NetworkSecurityGroup = $nsg

4. De wijzigingen hebt aangebracht in de NIC op te slaan uitvoeren de `Set-AzureRmNetworkInterface` cmdlet zoals hieronder wordt weergegeven.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Verwachte output met alleen de eigenschap **NetworkSecurityGroup** :

        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a>Koppeling van een NSG uit een NIC

Als u wilt de **NSG-FrontEnd** NSG uit de **TestNICWeb1** NIC koppeling, de onderstaande stappen uit te voeren.

1. Voer de `Get-AzureRmNetworkSecurityGroup` cmdlet kunt ophalen van de bestaande NSG en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Voer de `Get-AzureRmNetworkInterface` cmdlet kunt ophalen van de bestaande NIC en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. Stel de eigenschap **NetworkSecurityGroup** van de variabele **NIC** op **$null**, zoals hieronder wordt weergegeven.

        $nic.NetworkSecurityGroup = $null

4. De wijzigingen hebt aangebracht in de NIC op te slaan uitvoeren de `Set-AzureRmNetworkInterface` cmdlet zoals hieronder wordt weergegeven.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Verwachte output met alleen de eigenschap **NetworkSecurityGroup** :

        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a>Koppeling van een NSG vanuit een subnet

Als u wilt de **NSG-FrontEnd** NSG koppeling uit het **FrontEnd** subnet, de onderstaande stappen uit te voeren.

1. Voer de `Get-AzureRmVirtualNetwork` cmdlet kunt ophalen van de bestaande VNet en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Voer de `Get-AzureRmVirtualNetworkSubnetConfig` cmdlet kunt ophalen van het subnet **FrontEnd** en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

3. Stel de eigenschap **NetworkSecurityGroup** van de variabele **subnet** op **$null**, zoals hieronder wordt weergegeven.

        $subnet.NetworkSecurityGroup = $null

4. De wijzigingen hebt aangebracht in het subnet op te slaan uitvoeren de `Set-AzureRmVirtualNetwork` cmdlet zoals hieronder wordt weergegeven.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Verwachte uitvoer met alleen de eigenschappen van het subnet **FrontEnd** . Zoals u ziet, dat is er niet een eigenschap voor **NetworkSecurityGroup**:

            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-to-a-subnet"></a>Een NSG aan een subnet koppelen

Als u wilt koppelen het **NSG-FrontEnd** NSG opnieuw de subnet, **FronEnd** , de onderstaande stappen uit te voeren.

1. Voer de `Get-AzureRmVirtualNetwork` cmdlet kunt ophalen van de bestaande VNet en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Voer de `Get-AzureRmVirtualNetworkSubnetConfig` cmdlet kunt ophalen van het subnet **FrontEnd** en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

1. Voer de `Get-AzureRmNetworkSecurityGroup` cmdlet kunt ophalen van de bestaande NSG en opslaan in een variabele, zoals hieronder wordt weergegeven.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

3. Stel de eigenschap **NetworkSecurityGroup** van de variabele **subnet** op **$null**, zoals hieronder wordt weergegeven.

        $subnet.NetworkSecurityGroup = $nsg

4. De wijzigingen hebt aangebracht in het subnet op te slaan uitvoeren de `Set-AzureRmVirtualNetwork` cmdlet zoals hieronder wordt weergegeven.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Verwacht uitvoer met alleen de eigenschap van het subnet **FrontEnd** **NetworkSecurityGroup** :

        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a>Een NSG verwijderen

Als dit niet is gekoppeld aan een resource, kunt u alleen een NSG verwijderen. Als u wilt verwijderen van een NSG, de onderstaande stappen uit te voeren.

1. Als u wilt controleren of de resources die zijn gekoppeld aan een NSG, voer de `azure network nsg show` zoals wordt weergegeven in de [weergave NSGs koppelingen](#View-NSGs-associations).
2. Als de NSG gekoppeld aan een NIC is, voert u de `azure network nic set` zoals in [Dissociate een NSG uit een NIC](#Dissociate-an-NSG-from-a-NIC) voor elke NIC 
3. Als de NSG gekoppeld aan een subnet is, voert u de `azure network vnet subnet set` zoals wordt weergegeven in [een NSG vanuit een subnet Dissociate](#Dissociate-an-NSG-from-a-subnet) voor elk subnet.
4. Als u wilt verwijderen de NSG, voer de `Remove-AzureRmNetworkSecurityGroup` cmdlet zoals hieronder wordt weergegeven.

        Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force

    >[AZURE.NOTE] De **-dwingen** parameter zorgt ervoor dat u niet nodig hebt om de verwijdering te bevestigen.

## <a name="next-steps"></a>Volgende stappen

- [Logboekregistratie inschakelen](virtual-network-nsg-manage-log.md) voor NSGs.