## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>De sjabloon ARM met behulp van de Azure CLI implementeren

Als u wilt de ARM sjabloon die u hebt gedownload met behulp van Azure CLI implementeert, de onderstaande stappen uit te voeren.

1. Als u nooit Azure CLI gebruikt nog, Zie [installeren en configureren van de Azure CLI](../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en een abonnement selecteren.
2. Voer de **`azure config mode`** opdracht om te schakelen naar resourcemanager-modus, zoals hieronder wordt weergegeven.

        azure config mode arm

    Hier ziet u de verwachte uitvoer voor de bovenstaande opdracht:

        info:    New mode is arm

3. Voer indien nodig de **`azure group create`** maken van een nieuwe resourcegroep, zoals hieronder wordt weergegeven. Zoals u ziet de uitvoer van de opdracht. De lijst die wordt weergegeven na de uitvoer wordt uitgelegd van de parameters die worden gebruikt. Voor meer informatie over resourcegroepen, gaat u naar [Azure resourcemanager overzicht](../articles/resource-group-overview.md).

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

4. Voer de **`azure group deployment create`** implementeren van de nieuwe VNet met behulp van de sjabloon en de parameter-cmdlet bestanden u hebt gedownload en gewijzigd hierboven. De lijst die wordt weergegeven na de uitvoer wordt uitgelegd van de parameters die worden gebruikt.

        azure group deployment create -g TestRG -n TestVNetDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

    Hier ziet u de verwachte uitvoer voor de bovenstaande opdracht:

        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestVNetDeployment"
        + Registering providers
        info:    Registering provider microsoft.network
        + Waiting for deployment to complete
        data:    DeploymentName     : TestVNetDeployment
        data:    ResourceGroupName  : TestRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-08-14T21:56:11.152759Z
        data:    Mode               : Incremental
        data:    Name           Type    Value
        data:    -------------  ------  --------------
        data:    location       String  Central US
        data:    vnetName       String  TestVNet
        data:    addressPrefix  String  192.168.0.0/16
        data:    subnet1Prefix  String  192.168.1.0/24
        data:    subnet1Name    String  FrontEnd
        data:    subnet2Prefix  String  192.168.2.0/24
        data:    subnet2Name    String  BackEnd
        info:    group deployment create command OK

    - **-g (of--resourceveld groep)**. Naam van het resourceveld groep het nieuwe VNet wordt gemaakt.
    - **-f (of--sjabloon-bestand)**. Pad naar het sjabloonbestand ARM.
    - **-e (of--parameters-bestand)**. Pad naar uw ARM parameters-bestand.

5. Voer de **`azure network vnet show`** opdracht om weer te geven van de eigenschappen van het nieuwe vnet, zoals hieronder wordt weergegeven.

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
