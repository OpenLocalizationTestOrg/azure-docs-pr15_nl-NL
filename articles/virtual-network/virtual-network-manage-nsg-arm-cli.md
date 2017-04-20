<properties 
   pageTitle="Beheren met behulp van de Azure CLI in resourcemanager NSGs | Microsoft Azure"
   description="Informatie over het beheren van bestaande NSGs met de CLI Azure in resourcemanager"
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

# <a name="manage-nsgs-using-the-azure-cli"></a>Gebruik van de Azure CLI NSGs beheren

[AZURE.INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klassieke implementatiemodel.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="retrieve-information"></a>Gegevens ophalen

U kunt bekijken van uw bestaande NSGs, regels ophalen voor een bestaande NSG en ontdek wat resources een NSG is gekoppeld aan.

### <a name="view-existing-nsgs"></a>Bestaande NSGs weergeven

Uitvoeren om de lijst met NSGs weergeven in een bepaalde resourcegroep, de `azure network nsg list` opdracht, zoals hieronder wordt weergegeven. 

    azure network nsg list --resource-group RG-NSG

Verwachte output:

    info:    Executing command network nsg list
    + Getting the network security groups
    data:    Name          Location
    data:    ------------  --------
    data:    NSG-BackEnd   westus
    data:    NSG-FrontEnd  westus
    info:    network nsg list command OK
         
### <a name="list-all-rules-for-an-nsg"></a>Alle regels voor een NSG weergeven

Uitvoeren om weer te geven van de regels van een NSG **NSG-FrontEnd**met de naam, de `azure network nsg show` opdracht, zoals hieronder wordt weergegeven. 

    azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd

Verwachte output:
    
    info:    Executing command network nsg show
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Name                            : NSG-FrontEnd
    data:    Type                            : Microsoft.Network/networkSecurityGroups
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    Tags                            : displayName=NSG - Front End
    data:    Security group rules:
    data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
    data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
    data:    rdp-rule                       Internet           *            *               3389              Tcp       Inbound    Allow   100
    data:    web-rule                       Internet           *            *               80                Tcp       Inbound    Allow   101
    data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000
    data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001
    data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500
    data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000
    data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001
    data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500
    info:    network nsg show command OK

>[AZURE.NOTE] U kunt ook `azure network nsg rule list --resource-group RG-NSG --nsg-name NSG-FrontEnd` voor een overzicht van de regels van de **NSG-FrontEnd** NSG.

### <a name="view-nsg-associations"></a>Weergave NSG koppelingen

Als u wilt weergeven wat de NSG **NSG-FrontEnd is** resources koppelen aan, voer de `azure network nsg show` opdracht, zoals hieronder wordt weergegeven. Zoals u ziet dat het enige verschil het gebruik van de parameter **--json is** .

    azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json

Let op de eigenschappen **networkInterfaces** en **subnetten** zoals hieronder wordt weergegeven:

    "networkInterfaces": [],
    ...
    "subnets": [
        {
            "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
        }
    ],
    ...

In het bovenstaande voorbeeld de NSG is niet gekoppeld aan een netwerkinterfaces (NIC's) en is gekoppeld aan een benoemde **FrontEnd**subnet.

## <a name="manage-rules"></a>Regels beheren

U kunt regels toevoegen aan een bestaande NSG, bestaande regels bewerken en verwijderen van regels.

### <a name="add-a-rule"></a>Een regel toevoegen

Als u wilt toevoegen van een regel voor **binnenkomende** verkeer vanaf een willekeurige computer naar de **NSG-FrontEnd** NSG mogen poort **443** , uitvoeren de `azure network nsg rule create` opdracht, zoals hieronder wordt weergegeven.

    azure network nsg rule create --resource-group RG-NSG \
        --nsg-name NSG-FrontEnd \
        --name allow-https \
        --description "Allow access to port 443 for HTTPS" \
        --protocol Tcp \
        --source-address-prefix * \
        --source-port-range * \
        --destination-address-prefix * \
        --destination-port-range 443 \
        --access Allow \
        --priority 102 \
        --direction Inbound     

Verwachte output:

    info:    Executing command network nsg rule create
    + Looking up the network security rule "allow-https"
    + Creating a network security rule "allow-https"
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access to port 443 for HTTPS
    data:    Source IP                       : *
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule create command OK

### <a name="change-a-rule"></a>Een regel wijzigen

Als u wilt wijzigen van de regel toe te staan dat binnenkomende verkeer van **Internet** alleen hiervoor hebt gemaakt, worden uitgevoerd de `azure network nsg rule set` opdracht, zoals hieronder wordt weergegeven.

    azure network nsg rule set --resource-group RG-NSG \
        --nsg-name NSG-FrontEnd \
        --name allow-https \
        --source-address-prefix Internet

Verwachte output:

    info:    Executing command network nsg rule set
    + Looking up the network security group "NSG-FrontEnd"
    + Setting a network security rule "allow-https"
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access to port 443 for HTTPS
    data:    Source IP                       : Internet
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule set command OK

### <a name="delete-a-rule"></a>Een regel verwijderen

Als u wilt verwijderen van de regel die hierboven hebt gemaakt, worden uitgevoerd de `azure network nsg rule delete` opdracht, zoals hieronder wordt weergegeven.

    azure network nsg rule delete --resource-group RG-NSG \
        --nsg-name NSG-FrontEnd \
        --name allow-https \
        --quiet

>[AZURE.NOTE] De **--stille** parameter zorgt ervoor dat u niet nodig hebt om de verwijdering te bevestigen.

Verwachte output:

    info:    Executing command network nsg rule delete
    + Looking up the network security group "NSG-FrontEnd"
    + Deleting network security rule "allow-https"
    info:    network nsg rule delete command OK

## <a name="manage-associations"></a>Koppelingen beheren

U kunt een NSG naar subnetten en NIC's koppelen. U kunt ook een NSG uit een willekeurige deze gekoppeld aan bron koppeling.

### <a name="associate-an-nsg-to-a-nic"></a>Een NSG naar een NIC koppelen

Uitvoeren om te koppelen het **NSG-FrontEnd** NSG naar de **TestNICWeb1** NIC, de `azure network nic set` opdracht, zoals hieronder wordt weergegeven.

    azure network nic set --resource-group RG-NSG \
        --name TestNICWeb1 \
        --network-security-group-name NSG-FrontEnd

Verwachte output:

    info:    Executing command network nic set
    + Looking up the network interface "TestNICWeb1"
    + Looking up the network security group "NSG-FrontEnd"
    + Updating network interface "TestNICWeb1"
    + Looking up the network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Network security group          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Virtual machine                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-nic"></a>Koppeling van een NSG uit een NIC

Als u wilt de **NSG-FrontEnd** NSG uit de **TestNICWeb1** NIC koppeling, voer de `azure network nic set` opdracht, zoals hieronder wordt weergegeven.

    azure network nic set --resource-group RG-NSG --name TestNICWeb1 --network-security-group-id ""

>[AZURE.NOTE] Kennisgeving de "" (lege) waarde voor de parameter **netwerk-beveiliging-groep-id** . Dat is hoe u een koppeling naar een NSG verwijderen. Niet hetzelfde zijn als de parameter **netwerk-beveiliging-groep-name** .

Verwacht resultaat:

    info:    Executing command network nic set
    + Looking up the network interface "TestNICWeb1"
    + Updating network interface "TestNICWeb1"
    + Looking up the network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Virtual machine                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-subnet"></a>Koppeling van een NSG vanuit een subnet

Als u wilt de **NSG-FrontEnd** NSG koppeling uit het **FrontEnd** subnet, uitvoeren de `azure network vnet subnet set` opdracht, zoals hieronder wordt weergegeven.

    azure network vnet subnet set --resource-group RG-NSG \
        --vnet-name TestVNet \
        --name FrontEnd \
        --network-security-group-id ""

Verwachte output:

    info:    Executing command network vnet subnet set
    + Looking up the subnet "FrontEnd"
    + Setting subnet "FrontEnd"
    + Looking up the subnet "FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:    Type                            : Microsoft.Network/virtualNetworks/subnets
    data:    ProvisioningState               : Succeeded
    data:    Name                            : FrontEnd
    data:    Address prefix                  : 192.168.1.0/24
    data:    IP configurations:
    data:      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
    data:      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
    data:
    info:    network vnet subnet set command OK

### <a name="associate-an-nsg-to-a-subnet"></a>Een NSG aan een subnet koppelen

Uitvoeren om te koppelen het **NSG-FrontEnd** NSG opnieuw naar de **FronEnd** subnet, de `azure network vnet subnet set` opdracht, zoals hieronder wordt weergegeven.

    azure network vnet subnet set --resource-group RG-NSG \
        --vnet-name TestVNet \
        --name FrontEnd \
        --network-security-group-name NSG-FronEnd

>[AZURE.NOTE] De opdracht hierboven werkt alleen omdat de NSG **NSG-FrontEnd** in dezelfde resourcegroep als het virtuele netwerk **TestVNet**. Als de NSG zich in een groep van verschillende bronnen, moet u gebruiken de **--netwerk-beveiliging-groep-id** parameter in plaats daarvan en geef de volledige-id voor de NSG. U kunt de id ophalen door te voeren **azure netwerk nsg weergeven--resourcegroep RG-NSG--naam NSG-FrontEnd--json** en zoekt u de eigenschap **id** . 

Verwachte output:

        info:    Executing command network vnet subnet set
        + Looking up the subnet "FrontEnd"
        + Looking up the network security group "NSG-FrontEnd"
        + Setting subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
        data:      /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
        data:
        info:    network vnet subnet set command OK

## <a name="delete-an-nsg"></a>Een NSG verwijderen

Als dit niet is gekoppeld aan een resource, kunt u alleen een NSG verwijderen. Als u wilt verwijderen van een NSG, de onderstaande stappen uit te voeren.

1. Als u wilt controleren of de resources die zijn gekoppeld aan een NSG, voer de `azure network nsg show` zoals wordt weergegeven in de [weergave NSGs koppelingen](#View-NSGs-associations).
2. Als de NSG gekoppeld aan een NIC is, voert u de `azure network nic set` zoals in [Dissociate een NSG uit een NIC](#Dissociate-an-NSG-from-a-NIC) voor elke NIC 
3. Als de NSG gekoppeld aan een subnet is, voert u de `azure network vnet subnet set` zoals wordt weergegeven in [een NSG vanuit een subnet Dissociate](#Dissociate-an-NSG-from-a-subnet) voor elk subnet.
4. Als u wilt verwijderen de NSG, voer de `azure network nsg delete` opdracht, zoals hieronder wordt weergegeven.

        azure network nsg delete --resource-group RG-NSG --name NSG-FrontEnd --quiet

    Verwachte output:

        info:    Executing command network nsg delete
        + Looking up the network security group "NSG-FrontEnd"
        + Deleting network security group "NSG-FrontEnd"
        info:    network nsg delete command OK

## <a name="next-steps"></a>Volgende stappen

- [Logboekregistratie inschakelen](virtual-network-nsg-manage-log.md) voor NSGs.