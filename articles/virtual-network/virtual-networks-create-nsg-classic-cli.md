<properties
   pageTitle="Het maken van NSGs in de klassieke modus voor het gebruik van de Azure CLI | Microsoft Azure"
   description="Meer informatie over het maken en implementeren van NSGs in de klassieke modus voor het gebruik van de Azure CLI"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a>Het maken van NSGs (klassieke) in de CLI Azure

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel heeft betrekking op het implementatiemodel klassieke. U kunt ook [NSGs in het implementatiemodel resourcemanager maken](virtual-networks-create-nsg-arm-cli.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Het onderstaande voorbeeld Azure CLI opdrachten verwachten een eenvoudige omgeving al gemaakt op basis van het bovenstaande scenario. Als u uitvoeren van de opdrachten wilt, zoals deze worden weergegeven in dit document, moet u eerst de testomgeving maken door het [maken van een VNet](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Het maken van de NSG voor de front-end van subnet
Als u wilt maken van een NSG benoemde **NSG-FrontEnd** op basis van de bovenstaande scenario met de naam, de onderstaande stappen uit te voeren.

1. Als u nooit Azure CLI gebruikt nog, Zie [installeren en configureren van de Azure CLI](../xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en een abonnement selecteren.

2. Voer de **`azure config mode`** opdracht overschakelen naar klassieke modus, zoals hieronder wordt weergegeven.

        azure config mode asm

    Verwachte output:

        info:    New mode is asm

3. Voer de **`azure network nsg create`** opdracht om te maken van een NSG.

        azure network nsg create -l uswest -n NSG-FrontEnd

    Verwachte output:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Parameters:

    - **-l (of--locatie)**. Azure regio waar de nieuwe NSG worden gemaakt. In ons scenario *westus*.
    - **-n (of--naam)**. De naam voor de nieuwe NSG. In ons scenario *NSG-FrontEnd*.

4. Voer de **`azure network nsg rule create`** opdracht om een regel maken die toegang tot poort 3389 (RDP) met Internet is.

        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389

    Verwachte output:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

    Parameters:

    - **-een (of--nsg-naam)**. Naam van de NSG waarin de regel wordt gemaakt. In ons scenario *NSG-FrontEnd*.
    - **-n (of--naam)**. De naam voor de nieuwe regel. In ons scenario *rdp-regel*.
    - **-c (of--actie)**. Toegangsniveau voor de regel (weigeren of toestaan).
    - **-p (of--protocol)**. Protocol (Tcp, Udp, of *) voor de regel.
    - **-r (of--type)**. De richting van de verbinding (inkomend of uitgaand).
    - **-y (of--prioriteit)**. De prioriteit voor de regel.
    - **-f (of--bron-adres-voorvoegsel)**. Bron adresvoorvoegsel in CIDR of met standaardlabels.
    - **+ o (of--bronbereik-poort)**. Bronpoort of poortbereik.
    - **-e (of--doel-mailadres-voorvoegsel)**. Bestemming adresvoorvoegsel in CIDR of met standaardlabels.
    - **-u (of--bestemming-poort-bereik)**. Bestemmingspoort of poortbereik.

5. Voer de **`azure network nsg rule create`** opdracht om een regel maken die toegang tot poort 80 (HTTP) met Internet is.

        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80

    Verwachte putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Voer de **`azure network nsg subnet add`** opdracht de NSG koppelen aan de front-end van subnet.

        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd

    Verwachte output:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>Het maken van de NSG voor het back-end-subnet
Als u wilt maken van een NSG benoemde *NSG-BackEnd* op basis van de bovenstaande scenario met de naam, de onderstaande stappen uit te voeren.

3. Voer de **`azure network nsg create`** opdracht om te maken van een NSG.

        azure network nsg create -l uswest -n NSG-BackEnd

    Verwachte output:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Parameters:

    - **-l (of--locatie)**. Azure regio waar de nieuwe NSG worden gemaakt. In ons scenario *westus*.
    - **-n (of--naam)**. De naam voor de nieuwe NSG. In ons scenario *NSG-FrontEnd*.

4. Voer de **`azure network nsg rule create`** opdracht om een regel maken die toegang tot poort 1433 (SQL) met de front-end van subnet is.

        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433

    Verwachte output:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK


5. Voer de **`azure network nsg rule create`** opdracht om een regel maken die geen toegang met Internet.

        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80

    Verwachte putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Voer de **`azure network nsg subnet add`** opdracht de NSG koppelen aan het back-end-subnet.

        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd

    Verwachte output:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK
