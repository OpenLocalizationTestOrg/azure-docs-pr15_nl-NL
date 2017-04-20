## <a name="how-to-create-a-vnet-using-the-azure-cli"></a>Het maken van een VNet gebruik van de Azure CLI

U kunt de CLI Azure gebruiken voor het beheren van uw Azure resources uit de opdrachtprompt vanaf elke computer met Windows, Linux of OSX. Als u wilt een VNet maken met behulp van de Azure CLI, de onderstaande stappen uit te voeren.

1. Als u de CLI Azure nooit hebt gebruikt, Zie [installeren en configureren van de Azure CLI](../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en een abonnement selecteren.
2. Voer de opdracht **azure config modus** om te schakelen naar resourcemanager-modus, zoals hieronder wordt weergegeven.

        azure config mode arm

    Hier ziet u de verwachte uitvoer voor de bovenstaande opdracht:

        info:    New mode is arm

3. Voer indien nodig de **azure groep maken** als u wilt maken van een nieuwe resourcegroep, zoals hieronder wordt weergegeven. Zoals u ziet de uitvoer van de opdracht. De lijst die wordt weergegeven na de uitvoer wordt uitgelegd van de parameters die worden gebruikt. Voor meer informatie over resourcegroepen, gaat u naar [Azure resourcemanager overzicht](../articles/virtual-network/resource-group-overview.md#resource-groups).

        azure group create -n TestRG -l centralus

    Hier ziet u de verwachte uitvoer voor de bovenstaande opdracht:

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **-n (of--naam)**. De naam voor de nieuwe resourcegroep. In ons scenario *TestRG*.
    - **-l (of--locatie)**. Azure regio waar de nieuwe resourcegroep worden gemaakt. In ons scenario *centralus*.

4. Zoals hieronder wordt weergegeven, voert u de opdracht **azure netwerk vnet maken** om een VNet en een subnet, te maken. 

        azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l centralus

    Hier ziet u de verwachte uitvoer voor de bovenstaande opdracht:

        info:    Executing command network vnet create
        + Looking up virtual network "TestVNet"
        + Creating virtual network "TestVNet"
        + Loading virtual network state
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet2
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK

    - **-g (of--resourceveld groep)**. De naam van de resourcegroep waar de VNet worden gemaakt. In ons scenario *TestRG*.
    - **-n (of--naam)**. Naam van de VNet moet worden gemaakt. In ons scenario *TestVNet*
    - **-een (of--adres-voorvoegsels voor eenheden)**. Lijst met CIDR blokken gebruikt voor de VNet-adresruimte. In ons scenario *192.168.0.0/16*
    - **-l (of--locatie)**. Azure regio waar de VNet worden gemaakt. In ons scenario *centralus*.

5. Hiermee voert u de opdracht **azure netwerk vnet subnet maken** om te maken van een subnet, zoals hieronder wordt weergegeven. Zoals u ziet de uitvoer van de opdracht. De lijst die wordt weergegeven na de uitvoer wordt uitgelegd van de parameters die worden gebruikt.

        azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24

    Hier ziet u de verwachte uitvoer voor de bovenstaande opdracht:

        info:    Executing command network vnet subnet create
        + Looking up the subnet "FrontEnd"
        + Creating subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK

    - **-e (of--vnet-naam**. Naam van de VNet waar het subnet worden gemaakt. In ons scenario *TestVNet*.
    - **-n (of--naam)**. De naam van het nieuwe subnet. In ons scenario *FrontEnd*.
    - **-een (of--adres-voorvoegsel)**. Subnet CIDR blokkeren. Vier onze scenario, *192.168.1.0/24*.

6. Herhaal stap 5 hierboven om u te maken van andere subnetten, indien nodig. In ons scenario, voert u de opdracht hieronder de *BackEnd* -subnet maken.

        azure network vnet subnet create -g TestRG -e TestVNet -n BackEnd -a 192.168.2.0/24

4. Voer de opdracht **azure netwerk vnet weergeven** om weer te geven van de eigenschappen van het nieuwe vnet, zoals hieronder wordt weergegeven.

        azure network vnet show -g TestRG -n TestVNet

    Hier ziet u de verwachte uitvoer voor de bovenstaande opdracht:

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK
