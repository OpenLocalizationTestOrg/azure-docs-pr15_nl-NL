<properties
   pageTitle="Meerdere IP-adressen voor virtuele machines - PowerShell | Microsoft Azure"
   description="Leer hoe u meerdere IP-adressen toewijzen aan een virtuele machine via Azure PowerShell."
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
   ms.date="10/05/2016"
   ms.author="jdial;annahar" />


# <a name="assign-multiple-ip-addresses-to-virtual-machines"></a>Meerdere IP-adressen toewijzen aan virtuele machines

Een Azure Virtual Machine (VM) kan een of meer netwerkinterfaces (NIC) die zijn bijgevoegd bij deze hebben. Een NIC kan een of meer openbaar of privé IP-adressen die zijn toegewezen aan deze hebben. Als u niet bekend met IP-adressen in Azure wordt aangegeven bent, leest u het [IP-adressen in Azure](virtual-network-ip-addresses-overview-arm.md) -artikel voor meer informatie over deze. In dit artikel wordt uitgelegd hoe u meerdere IP-adressen toewijzen aan een VM in het implementatiemodel van Azure resourcemanager met Azure PowerShell.

Het toewijzen van meerdere IP-adressen voor een VM, kunt de volgende mogelijkheden:

- Meerdere websites of services met verschillende IP-adressen en SSL-certificaten op één server hosten.
- Fungeren als een netwerk virtuele toestel, zoals een firewall of de belasting voor documentconversies.
- De mogelijkheid om een van de privé IP-adressen voor een van de NIC's toevoegen aan een back-enddatabase van taakverdeling Azure toepassingen. In het verleden, kan alleen het primaire IP-adres voor de primaire NIC worden toegevoegd aan een back-enddatabase-groep.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

Om u te registreren voor de Preview-versie, kunt u een e-mail verzenden naar [Meerdere IP's](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) met uw abonnements-ID en gebruik bedoeld.

## <a name="scenario"></a>Scenario

In dit artikel wilt u drie IP-configuraties aan een netwerkinterface koppelen.
Het volgende voorbeeldconfiguraties worden gemaakt en toegewezen aan een NIC die heeft drie privé IP-adressen en één openbare IP-adres toegewezen:

- IPConfig-1: Een dynamische privé IP-adres (standaard) en een openbare IP-adres uit het openbare IP-adres resource met de naam PIP1.
- IPConfig-2: Een statische privé IP-adres en geen openbare IP-adres.
- IPConfig-3: Een dynamische privé IP-adres en geen openbare IP-adres.

    ![Van de ALT-afbeeldingstekst](./media/virtual-network-multiple-ip-addresses-powershell/OneNIC-3IP.png)

Dit scenario wordt ervan uitgegaan dat u hebt een resourcegroep *RG1* waarbinnen er is een VNet *VNet1* genoemd en een subnet genaamd *Subnet1*genoemd. Bovendien wordt ervan uitgegaan u hebt een VM *VM1*genoemd, een netwerkinterface *VM1-NIC1* de naam die is gekoppeld aan en een openbare IP-adres genoemd *PIP1*.

[In dit artikel](./virtual-machines/virtual-machines-windows-ps-create.md ) begeleidt bij het maken van de resources die hierboven genoemde geval u ze voordat u niet hebt gemaakt.

## <a name = "create"></a>Een VM met meerdere IP-adressen maken

1. Open een opdrachtprompt PowerShell en vul de overige stappen in deze sectie binnen een enkel PowerShell-sessie. Als u nog niet PowerShell geïnstalleerd en geconfigureerd, voert u de stappen in het artikel [installeren en configureren van Azure PowerShell](../powershell-install-configure.md) .

2. Wijzig de "waarden" van de volgende $Variables naar de Azure [locatie](https://azure.microsoft.com/regions) die uw virtuele netwerk is in de naam van uw [resourcegroep](../azure-resource-manager/resource-group-overview.md#resource-groups), de VNet binnen de resourcegroep, het subnet die u wilt de NIC aan verbinden en de naam van de NIC Voltooi de stappen om meerdere IP-adressen toevoegen aan een NIC die zijn bijgevoegd bij een VM, als u nodig hebt.

        $Location = "westcentralus"
        $RgName   = "RG1"
        $VNetName   = "VNet1"
        $SubnetName = "Subnet1"
        $NicName     = "VM1-NIC1"
        $PIP = "PIP"

    Als u de naam van een bestaande Azure locatie of de resourcegroep niet weet, typt u de volgende opdrachten:

        Get-AzureRmLocation      | Format-Table Location
        Get-AzureRmResourceGroup | Format-Table ResourceGroupName

    <a name="subnet"></a>De NIC moet worden verbonden met een subnet binnen een bestaand Azure virtuele netwerk (VNet). De drie onderdelen: NIC, subnet en VNet, moet al bestaan in de regio en het [abonnement](../azure-glossary-cloud-terminology.md#subscription).  Als u niet bekend met VNets bent, leest u het artikel [virtuele netwerk overzicht](virtual-networks-overview.md) voor meer informatie over deze of lees het artikel [een VNet maken](virtual-networks-create-vnet-arm-ps.md) om te leren hoe u een account maakt.

    Als u de naam van een bestaande VNet niet weet, voer de volgende opdracht en *VNet1* in de vorige variabele vervangen door de naam van een VNet:

        Get-AzureRmVirtualNetwork | Format-Table Name

    Als de lijst als resultaat gegeven leeg is, moet u een VNet maken. Voor meer informatie over hoe, lees het artikel [een virtueel netwerk maken](virtual-networks-create-vnet-arm-ps.md) .

    Typ de volgende opdrachten voor het ophalen van de naam van de bestaande subnetten binnen de VNet en *Subnet1* hierboven vervangen door de naam van een subnet:

        $VNet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RgName
        $VNet.Subnets | Format-Table Name, AddressPrefix

4. Voer de volgende opdracht voor het ophalen van het subnet en toewijzen aan een variabele.

        $Subnet = $VNet.Subnets | Where-Object { $_.Name -eq $SubnetName }

5. <a name="ipconfigs"></a>De IP-configuraties die u wilt toewijzen aan de NIC definiëren Elke configuratie kunt beschikken over een statische of dynamische privé IP-adres en een bron van het openbare IP-adres dat is gekoppeld aan een statische of dynamische adres.

    Toevoegen of verwijderen niet het getal van de configuraties die volgt afhankelijk van hoeveel IP-adressen die u wilt koppelen aan de NIC en de instellingen die u wilt configureren.

    **IPConfig-1**

    Wijzig de waarde *PIP1* in de naam van een bestaande openbare IP-adres resource die zich in de locatie die u maakt de NIC in en die niet is gekoppeld aan een andere NIC Er bestaat een wijziging *RG1* op de naam van de resourcegroep de bron van het openbare IP-adres in. Wijzig *IPConfig-1* in de naam die u wilt geven tot de eerste IP-configuratie. Voer de volgende opdrachten:

        $PIP1 = Get-AzureRmPublicIPAddress -Name "PIP1" -ResourceGroupName $RgName

        $IpConfigName1 = "IPConfig-1"
        $IPConfig1     = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName1 -Subnet $Subnet -PublicIpAddress $PIP1 -Primary

    Opmerking de *-primaire* schakelen. Wanneer u meerdere IP-configuraties aan een NIC toewijzen, moet u één configuratie als de *primaire*toegewezen. Als u de naam van een bestaande bron van openbare IP-adres niet weet, voert u de volgende opdracht uit: Get-AzureRMPublicIPAddress | De naam van de tabel opmaken, locatie, IP-adres, IpConfiguration

    Als de kolom **IPConfiguration** geen waarde in de uitvoer geretourneerd heeft, wordt de bron van het openbare IP-adres is niet gekoppeld aan een bestaande NIC en kan worden gebruikt. Als de lijst leeg is, of er geen resources beschikbaar openbare IP-adres zijn, kunt u een met de opdracht **Nieuw AzureRmPublicIPAddress** maken.

    >[AZURE.NOTE] Openbare IP-adressen hebben een nominale kosten. Lees meer informatie over het IP-adres prijzen, de pagina [IP-adres prijzen](https://azure.microsoft.com/pricing/details/ip-addresses) .

    **IPConfig-2**

    *IPConfig-2* op de naam die u wilt geven tot de tweede IP-configuratie en *10.0.0.5* wijzigen in een niet-gebruikte geldige IP-adres voor het subnet die u de NIC om te toewijst wijzigen. Voer de volgende opdrachten:

        $IPConfigName2 = "IPConfig-2"
        $IPAddress = 10.0.0.5

        $IPConfig2 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName2 -Subnet $Subnet -PrivateIpAddress $IPAddress

    Voer de volgende opdracht, als u niet weet wie de IP bereik die zijn toegewezen aan het subnet adresseren:

        $VNet.Subnets | Format-Table Name, AddressPrefix

    **IPConfig-3**

    *IPConfig-3* wijzigen op de naam die u wilt geven tot de derde IP-configuratie en voer de volgende opdrachten:

        $IPConfigName3 = "IPConfig-3"
        $IPConfig3 = New-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName3 -Subnet $Subnet

    >[AZURE.NOTE] U kunt maximaal 250 privé IP-adres toewijzen aan een NIC Er is een beperking voor het aantal openbare IP-adressen die binnen een abonnement kan worden gebruikt. Lees meer informatie het artikel [Azure beperkt](../azure-subscription-service-limits.md#networking-limits---azure-resource-manager) .

6. Maak de NIC met het IP-configuraties gedefinieerd in de vorige stap.

        $nic = New-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName -Location $Location -IpConfiguration $IpConfig1,$IpConfig2,$IpConfig3

7. De NIC bijvoegen bij het maken van een VM volgens de stappen in het artikel [een VM maken](../virtual-machines/virtual-machines-windows-ps-create.md) . Hoewel het artikel een met Windows Server VM maakt, zijn de stappen in hetzelfde is voor een VM Linux, dan een ander besturingssysteem selecteren. Voer stap 1-3 in dit artikel. Stap 4 en 5 en vervolgens volledige stap 6 in het maken van een artikel VM overslaan.

    >[AZURE.WARNING] Stap 6 in het maken van een artikel VM, mislukt als u de variabele $nic naar iets anders in stap 6 van dit artikel met de naam gewijzigd of de vorige stappen van dit artikel niet zijn voltooid.

8. Weergave de privé IP-adressen die Azure-DHCP die zijn toegewezen aan de NIC en het openbare IP-adres resource toegewezen aan de NIC door de volgende opdracht uit te voeren:

        $nic.IpConfigurations | Format-Table Name, PrivateIPAddress, PublicIPAddress, Primary

9. <a name="os"></a>Handmatig alle privé IP-adressen (inclusief de primaire) toevoegen aan de TCP/IP-configuratie in het besturingssysteem. 

**Windows**

1. Vanaf een opdrachtprompt, typ *ipconfig/alle*.  U ziet alleen het *primaire* privé IP-adres (via DHCP).
2. Typ vervolgens *ncpa.cpl* in het opdrachtpromptvenster. Hiermee wordt een nieuw venster openen.
3. Open de eigenschappen voor **Lokale netwerkverbinding**.
4. Dubbelklikt u op Internet Protocol versie 4 (IPv4)
5. Selecteer **de volgende IP-adres gebruiken** en voer de volgende waarden:
    - **IP-adres**: Voer het *primaire* privé IP-adres
    - **Subnetmasker**: instellen op basis van uw subnet. Als het subnet is een /24 bijvoorbeeld subnet vervolgens het subnetmasker is 255.255.255.0.
    - **Standaard-gateway**: het eerste IP-adres in het subnet. Als uw subnet 10.0.0.0/24 is, is het IP-adres van de gateway 10.0.0.1.
    - Klik op **de adressen van de volgende DNS-server gebruiken** en voer de volgende waarden:
        - **Voorkeur DNS-server:** Voer 168.63.129.16 als u niet werkt met uw eigen DNS-server.  Als u bent, voert u het IP-adres voor uw DNS-server.
    - Klik op de knop **Geavanceerd** en toevoegen van extra IP-adressen. Toevoegen aan elk van de secundaire privé IP-adressen weergegeven in stap 8 met de NIC met hetzelfde subnet opgegeven voor het primaire IP-adres.
    - Klik op **OK** om te sluiten van de TCP/IP-instellingen en klik vervolgens op **OK** om te sluiten van instellingen voor de adapter. Hiermee wordt vervolgens uw RDP-verbinding hersteld.

6. Vanaf een opdrachtprompt, typ *ipconfig/alle*. Alle IP-adressen die u hebt toegevoegd, worden weergegeven en DHCP is uitgeschakeld.


**Linux (Ubuntu)**

1. Open een terminal-venster.
2. Zorg ervoor dat u de hoofdmap-gebruiker bent. Als u niet bent, kunt u dit doen met behulp van de volgende opdracht uit:

            sudo -i
3. Werk het configuratiebestand van het netwerk (ervan uitgaande dat er 'eth0').
    - Houd het bestaande item van de regel voor dhcp. Hiermee wordt het primaire IP-adres configureren, zoals het moet eerder hebt gebruikt.
    - Een configuratie voor een aanvullende statische IP-adres met de volgende opdrachten toevoegen:

                cd /etc/network/interfaces.d/
                ls

        Hier ziet u een bestand cfg.
4. Open het bestand: vi *bestandsnaam*.

    Hier ziet u de volgende regels aan het einde van het bestand:

            auto eth0
            iface eth0 inet dhcp
5. Voeg de volgende regels na de regels die in dit bestand aanwezig:

            iface eth0 inet static
            address <your private IP address here>
6. Sla het bestand met behulp van de volgende opdracht uit:

            :wq
7.  De netwerkinterface met de volgende opdracht uit het opnieuw instellen:

            sudo ifdown eth0 && sudo ifup eth0

    >[AZURE.IMPORTANT] Uitgevoerd ifdown zowel ifup in dezelfde regel als een verbinding met extern gebruikt.
8. Controleer of dat het IP-adres wordt toegevoegd aan het netwerkinterface met de volgende opdracht uit:

            ip addr list eth0

    Hier ziet u het IP-adres dat u hebt toegevoegd als onderdeel van de lijst.

**Linux (Redhat, CentOS en anderen)**

1. Open een terminal-venster.
2. Zorg ervoor dat u de hoofdmap-gebruiker bent. Als u niet bent, kunt u dit doen met behulp van de volgende opdracht uit:

            sudo -i
3. Voer uw wachtwoord in en volg de instructies als u hierom wordt gevraagd. Als u de hoofdmap-gebruiker bent, kunt u navigeren naar de netwerkmap scripts met de volgende opdracht uit:

            cd /etc/sysconfig/network-scripts
4. Een lijst met de gerelateerde ifcfg-bestanden met de volgende opdracht:

            ls ifcfg-*

    Als een van de bestanden ziet u *ifcfg-eth0* .
5. Kopieer het bestand *ifcfg-eth0* en noem deze *ifcfg-eth0:0* met de volgende opdracht uit:

            cp ifcfg-eth0 ifcfg-eth0:0
6. Bewerken de *ifcfg-eth0:0* bestand met de volgende opdracht uit:

            vi ifcfg-eth1
7. Wijzigen van het apparaat naar de gewenste naam in het bestand. *eth0:0* in dit geval met de volgende opdracht uit:

            DEVICE=eth0:0
8. Wijzigen de *IPADDR YourPrivateIPAddress =* regel aan het IP-adres.
9. Sla het bestand met de volgende opdracht uit:

            :wq
10. Start de netwerkservices opnieuw en zorg ervoor dat de wijzigingen die zijn geslaagd door de volgende opdrachten uit te voeren:

            /etc/init.d/network restart
            Ipconfig

    Hier ziet u het IP-adres dat u hebt toegevoegd, *eth0:0*, in de lijst als resultaat gegeven.

## <a name="add"></a>IP-adressen toevoegen aan een bestaande VM

Voer de volgende stappen uit om extra IP-adressen toevoegen aan een bestaande NIC:

1. Open een opdrachtprompt PowerShell en vul de overige stappen in deze sectie binnen een enkel PowerShell-sessie. Als u nog niet PowerShell geïnstalleerd en geconfigureerd, voert u de stappen in het artikel [installeren en configureren van Azure PowerShell](../powershell-install-configure.md) .

2. Wijzig de "waarden" van de volgende $Variables op de naam van de NIC die u wilt toevoegen van IP-adressen en de resourcegroep en de locatie die de NIC bestaat in:

        $NicName     = "RG1-VM1-NIC1"
        $RgName   = "RG1"
        $NicLocation = "westcentralus"

    Als u de naam van de NIC die u wilt wijzigen, voert u de volgende opdrachten niet weet, klikt u vervolgens de waarden van de vorige variabelen te wijzigen:

        Get-AzureRmNetworkInterface | Format-Table Name, ResourceGroupName, Location

3. Een variabele maken en stel deze in op de bestaande NIC door te typen van de volgende opdracht uit:

        $nic = Get-AzureRmNetworkInterface -Name $NicName -ResourceGroupName $RgName

4. De subnet-ID die de NIC is verbonden met door te vullen [stap 3](#subnet) van het maken van een VM met meerdere IP-adressen sectie in dit artikel worden opgehaald.

5. Maak de IP-configuraties die u toevoegen aan het netwerk wilt volgens de instructies in [stap 5](#ipconfigs) van de maken een VM met meerdere IP-adressen sectie in dit artikel.

6. *$IPConfigName4* wijzigen op de naam van de IP-configuratie die u in de vorige stap hebt gemaakt. Als u wilt toevoegen de configuratie, voert u de volgende opdracht uit:

        Add-AzureRmNetworkInterfaceIpConfig -Name $IPConfigName4 -NetworkInterface $nic -Subnet $Subnet1

7. Als u wilt instellen dat de NIC met het IP-, voert u de volgende opdracht uit:

        Set-AzureRmNetworkInterface -NetworkInterface $nic

8. De IP-adressen die u hebt toegevoegd aan de Cirkels aan het besturingssysteem VM volgens de instructies in [stap 9](#os) van het maken van een VM met meerdere IP-adressen sectie in dit artikel toevoegen.
