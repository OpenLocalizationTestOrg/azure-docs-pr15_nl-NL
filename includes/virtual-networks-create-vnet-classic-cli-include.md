## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Het maken van een klassieke VNet met Azure CLI

U kunt de CLI Azure gebruiken voor het beheren van uw Azure resources uit de opdrachtprompt vanaf elke computer met Windows, Linux of OSX. Als u wilt een VNet maken met behulp van de Azure CLI, de onderstaande stappen uit te voeren.

1. Als u nooit Azure CLI gebruikt nog, Zie [installeren en configureren van de Azure CLI](../articles/xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en een abonnement selecteren.
2. Zoals hieronder wordt weergegeven, voert u de opdracht **azure netwerk vnet maken** om een VNet en een subnet, te maken. De lijst die wordt weergegeven na de uitvoer wordt uitgelegd van de parameters die worden gebruikt.

            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    
    Verwachte output:

            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    - **--vnet**. Naam van de VNet moet worden gemaakt. In ons scenario *TestVNet*
    - **-e (of--adresruimte)**. VNet-adresruimte. In ons scenario *192.168.0.0*
    - **-i (of - cidr)**. Netwerkmasker in CIDR indeling. In ons scenario *16*.
    - **- n (of--subnet-naam**). Naam van het eerste subnet. In ons scenario *FrontEnd*.
    - **-p (of--subnet-begin-IP-)**. Eerste IP-adres voor subnet of subnet-adresruimte. In ons scenario *192.168.1.0*.
    - **-r (of--subnet-cidr)**. Netwerkmasker in CIDR-indeling voor subnet. In ons scenario *24*.
    - **-l (of--locatie)**. Azure regio waar de VNet worden gemaakt. In ons scenario *Central US*.

3. Hiermee voert u de opdracht **azure netwerk vnet subnet maken** om te maken van een subnet, zoals hieronder wordt weergegeven. De lijst die wordt weergegeven na de uitvoer wordt uitgelegd van de parameters die worden gebruikt.

            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    
    Hier ziet u de verwachte uitvoer voor de bovenstaande opdracht:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    - **-t (of--vnet-naam**. Naam van de VNet waar het subnet worden gemaakt. In ons scenario *TestVNet*.
    - **-n (of--naam)**. De naam van het nieuwe subnet. In ons scenario *BackEnd*.
    - **-een (of--adres-voorvoegsel)**. Subnet CIDR blokkeren. Vier onze scenario, *192.168.2.0/24*.

4. Voer de opdracht **azure netwerk vnet weergeven** om weer te geven van de eigenschappen van het nieuwe vnet, zoals hieronder wordt weergegeven.

            azure network vnet show

    Hier ziet u de verwachte uitvoer voor de bovenstaande opdracht:

            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK
