<properties 
   pageTitle="Routering bepalen en het gebruik van virtuele apparaten met behulp van de Azure CLI in het implementatiemodel klassieke | Microsoft Azure"
   description="Meer informatie over het om te bepalen in VNets de CLI Azure gebruiken in het implementatiemodel klassieke-mailroutering"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-the-azure-cli"></a>Bepalen Routering en gebruiken met de CLI Azure virtuele toestellen (klassieke)

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel heeft betrekking op het implementatiemodel klassieke. U kunt ook [besturingselement Routering en virtuele toestellen in het implementatiemodel resourcemanager gebruiken](virtual-network-create-udr-arm-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Het onderstaande voorbeeld Azure CLI opdrachten verwachten een eenvoudige omgeving al gemaakt op basis van het bovenstaande scenario. Als u uitvoeren van de opdrachten wilt, zoals deze worden weergegeven in dit document, maakt u de omgeving wordt weergegeven in [een VNet (klassieke) gebruik van de Azure CLI maken](virtual-networks-create-vnet-classic-cli.md).

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>De UDR voor de front-end van subnet maken
Als u wilt maken van de tabel en de route nodig voor de front-end van subnet op basis van de bovenstaande scenario, de onderstaande stappen uit te voeren.

1. Voer de **`azure config mode`** overschakelen naar klassieke modus.

        azure config mode asm

    Uitvoer:

        info:    New mode is asm

3. Voer de **`azure network route-table create`** opdracht om een routetabel voor het subnet front-end te maken.

        azure network route-table create -n UDR-FrontEnd -l uswest

    Uitvoer:

        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK

    Parameters:
    - **-l (of--locatie)**. Azure regio waar de nieuwe NSG worden gemaakt. In ons scenario *westus*.
    - **-n (of--naam)**. De naam voor de nieuwe NSG. In ons scenario *NSG-FrontEnd*.

4. Voer de **`azure network route-table route set`** opdracht een route wilt maken in de routetabel gemaakt boven aan het verzenden van al het verkeer bestemd is voor het back-end-subnet (192.168.2.0/24) naar de **FW1** VM (192.168.0.4).

        azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4

    Uitvoer:

        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK

    Parameters:
    - **-r (of--route tabelnaam)**. Naam van de tabel waar de route wordt toegevoegd. In ons scenario *UDR-FrontEnd*.
    - **-een (of--adres-voorvoegsel)**. Adresvoorvoegsel voor het subnet waar pakketten naar bestemd zijn. In ons scenario *192.168.2.0/24*.
    - **-t (of--volgende-hop-type)**. Type object verkeer worden om te verzonden. Mogelijke waarden zijn *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*of *geen*.
    - **-p (of--volgende-hop-ip-adres**). IP-adres voor de volgende hop. In ons scenario *192.168.0.4*.

5. Voer de **`azure network vnet subnet route-table add`** opdracht om te koppelen van de tabel die hierboven zijn gemaakt met het subnet **FrontEnd** .

        azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd

    Uitvoer:

        info:    Executing command network vnet subnet route-table add
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK 

    Parameters:
    - **-t (of--vnet-naam)**. Naam van de VNet waarin het subnet zich bevindt. In ons scenario *TestVNet*.
    - **- n (of--subnet-naam**. De naam van het subnet dat de routetabel, worden toegevoegd aan. In ons scenario *FrontEnd*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>De UDR voor het back-end-subnet maken
Als u wilt maken van de tabel en de route die u nodig hebt voor het back-end-subnet op basis van de bovenstaande scenario, de onderstaande stappen uit te voeren.

3. Voer de **`azure network route-table create`** opdracht om een routetabel voor het back-end-subnet te maken.

        azure network route-table create -n UDR-BackEnd -l uswest

4. Voer de **`azure network route-table route set`** opdracht een route wilt maken in de routetabel gemaakt boven aan het verzenden van al het verkeer bestemd de front-end van subnet (192.168.1.0/24) de **FW1** VM (192.168.0.4).

        azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4

5. Voer de **`azure network vnet subnet route-table add`** opdracht om te koppelen van de tabel die hierboven zijn gemaakt met de **BackEnd** -subnet.

        azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd

