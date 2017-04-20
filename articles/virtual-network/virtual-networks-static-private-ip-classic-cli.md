<properties 
   pageTitle="Het instellen van een statische privé IP-adres in de klassieke modus ausing de CLI | Microsoft Azure"
   description="Informatie over statische privé IP-adressen (Spanningsdips) en hoe u ze kunt beheren in de klassieke modus de CLI gebruiken"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-azure-cli"></a>Hoe u een statische privé IP-adres (klassieke) in Azure CLI instellen

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]In dit artikel heeft betrekking op het implementatiemodel klassieke. U kunt ook [een statische privé IP-adres in het implementatiemodel resourcemanager-beheren](virtual-networks-static-private-ip-arm-cli.md).

Het onderstaande voorbeeld Azure CLI opdrachten verwachten een eenvoudige omgeving al hebt gemaakt. Als u uitvoeren van de opdrachten wilt, zoals deze worden weergegeven in dit document, moet u eerst de testomgeving wordt beschreven in het [maken van een vnet](virtual-networks-create-vnet-classic-cli.md)maken.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Een statische privé IP-adres opgeven bij het maken van een VM
Als u wilt maken van een nieuwe VM met de naam *DNS01* in een nieuwe cloudservice *TestService* op basis van de bovenstaande scenario met de naam, de volgende stappen uit:

1. Als u nooit Azure CLI gebruikt nog, Zie [installeren en configureren van de Azure CLI](../xplat-cli-install.md) en volg de instructies tot aan het punt waar u uw Azure-account en een abonnement selecteren.
1. Hiermee voert u de opdracht **azure-service maken** om de cloudservice te maken.

        azure service create TestService --location uscentral

    Verwachte output:

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
    
2. De opdracht **azure vm maken** om te maken van de VM uitvoeren. Let op de waarde voor een statische privé IP-adres. De lijst die wordt weergegeven na de uitvoer wordt uitgelegd van de parameters die worden gebruikt.

        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd

    Verwachte output:

        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK

    - **-l (of--locatie)**. Azure regio waar de VM worden gemaakt. In ons scenario *centralus*.
    - **-n (of--vm-naam)**. Naam van de VM moet worden gemaakt.
    - **-w (of--virtuele-netwerknaam)**. Naam van de VNet waar de VM worden gemaakt. 
    - **-S (of--statische ip-)**. Statische privé IP-adres van de VM.
    - **TestService**. Naam van de cloudservice waar de VM worden gemaakt.
    - **bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**. Afbeelding gebruikt om te maken van de VM.
    - **adminuser**. Lokale beheerder voor de Windows-VM.
    - **AdminP@ssw0rd**. Lokale beheerderswachtwoord voor de Windows-VM.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Het statische IP-adres privégegevens voor een VM ophalen
Om weer te geven de statische privé IP-adresgegevens voor VM die zijn gemaakt met de bovenstaande script, voer de volgende opdracht uit Azure CLI en houd rekening met de waarde voor *Netwerk-StaticIP*:

    azure vm static-ip show DNS01

Verwachte output:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Een statische privé IP-adres uit een VM verwijderen
Toegevoegd aan de VM in het bovenstaande, script uitvoeren van de volgende opdracht uit Azure CLI het statische privé IP-adres verwijderen:
    
    azure vm static-ip remove DNS01

Verwachte output:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a>Hoe u een statische privé IP toevoegt aan een bestaande VM
Een statische toevoegen privé IP-adres voor VM gemaakt met behulp van het script bovenstaande runt hij-opdracht:

    azure vm static-ip set DNS01 192.168.1.101

Verwachte output:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [gereserveerde openbare IP-](virtual-networks-reserved-public-ip.md) adressen.
- Meer informatie over [openbare IP-(ILPIP) op gebruikersniveau exemplaar](virtual-networks-instance-level-public-ip.md) -adressen.
- Raadpleeg de [gereserveerde IP-REST API's](https://msdn.microsoft.com/library/azure/dn722420.aspx).
