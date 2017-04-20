<properties
   pageTitle="Een VM met meerdere NIC's maken"
   description="Meer informatie over het maken en configureren van vms met meerdere NIC 's"
   services="virtual-network, virtual-machines"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management,azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="create-a-vm-with-multiple-nics"></a>Een VM met meerdere NIC's maken

U kunt maken van virtuele machines (VMs) in Azure wordt aangegeven en meerdere netwerkinterfaces (NIC's) toevoegen aan elk van uw VMs. Multi NIC is vereist voor veel netwerk virtuele toestellen, zoals de bezorging van de toepassing en WAN-optimalisatieoplossingen. Multi NIC ook vindt u meer netwerkverkeer management functionaliteit, inclusief moeten worden geïsoleerd van verkeer tussen een voorgrond NIC beëindigen en back-end NIC of scheiding gegevens vlak verkeer uit management vlak-verkeer is toegestaan.

![Meerdere NIC voor VM](./media/virtual-networks-multiple-nics/IC757773.png)

De bovenstaande afbeelding ziet u een VM met drie NIC's dat elk verbonden met een ander subnet.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]

- Internetgerichte VIP (klassieke implementaties) wordt alleen ondersteund op de NIC "standaard" Er is slechts één VIP op het IP-adres van de standaard-NIC
- Op dit moment kan worden exemplaar niveau openbare IP-(LPIP)-adressen (klassieke implementaties) worden niet ondersteund voor meerdere NIC VMs.
- De volgorde van de NIC's uit binnen de VM wordt willekeurig, en ook tussen Azure-infrastructuur updates kan wijzigen. Echter de IP-adressen en de bijbehorende ethernet MAC-adressen is ongewijzigd. Stel **dat eth1** heeft IP-adres 10.1.0.100 en MAC-adres 00-0D-3A-B0-39-0D; na een infrastructuurupdate van Azure-en opnieuw opstarten, deze kan worden gewijzigd naar **Eth2**, maar het IP- en MAC koppelen, ongewijzigd blijft. Wanneer een herstart handeling van de klant is, ongewijzigd de volgorde NIC blijft.
- Het adres van elke NIC in elke VM moet zich bevinden in een subnet, meerdere netwerkadapters op een enkele VM kunnen elk worden toegewezen adressen in hetzelfde subnet.
- De grootte VM bepaalt het aantal NIC's die u voor een VM maken kunt. Verwijzing de [Windows Server](../virtual-machines/virtual-machines-windows-sizes.md) en [Linux](../virtual-machines/virtual-machines-linux-sizes.md) VM grootten artikelen om te bepalen hoeveel NIC's wordt de grootte van elke VM ondersteunt. 

## <a name="network-security-groups-nsgs"></a>Beveiligingsgroepen netwerk (NSGs)
In een implementatie resourcemanager mogelijk een NIC in een VM gekoppeld met een netwerk beveiliging groep (NSG), inclusief eventuele netwerkadapters op een VM met meerdere NIC's die zijn ingeschakeld. Als een NIC een adres binnen een subnet waar het subnet gekoppeld aan een NSG is is toegewezen, klikt u vervolgens de regels in van het subnet NSG ook toegepast op die NIC Naast het koppelen van subnetten met NSGs, kunt u ook een NIC koppelen aan een NSG.

Als een subnet is gekoppeld aan een NSG en een NIC binnen dat subnet afzonderlijk gekoppeld aan een NSG, de bijbehorende NSG regels worden toegepast in **stroom volgorde** op basis van de richting van het verkeer wordt doorgegeven in- of uitzoomen op de NIC:

- **Inkomende verkeer** waarvan bestemming de betreffende NIC is doorloopt eerst het subnet, activeert van het subnet NSG regels voor het doorgeven van in de NIC en vervolgens van de NIC NSG regels activeert.
- **Uitgaande verkeer** waarvan de gegevensbron de betreffende NIC is loopt eerst af van de NIC, activeert de NIC's NSG regels, voordat u via het subnet en vervolgens van het subnet NSG regels activeert.

Meer informatie over het [Netwerk beveiligingsgroepen](virtual-networks-nsg.md) en hoe deze worden toegepast op basis van koppelingen naar subnetten, VMs en NIC..

## <a name="how-to-configure-a-multi-nic-vm-in-a-classic-deployment"></a>Een meerdere NIC VM configureren in een klassieke implementatie

De onderstaande instructies kunt u een meerdere NIC VM met 3 NIC maken: een standaard NIC en twee extra netwerkadapters. De volgende configuratiestappen uit maakt een VM die wordt geconfigureerd op basis van de onderstaande van service configuratie bestandsfragment:

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over the remainder section …
    </VirtualNetworkSite>


Moet u de volgende vereisten voordat u probeert de PowerShell-opdrachten uitvoeren in het voorbeeld.

- Een Azure-abonnement.
- Een geconfigureerde virtueel netwerk. Zie [Overzicht van virtuele netwerk](virtual-networks-overview.md) voor meer informatie over VNets.
- De nieuwste versie van Azure PowerShell gedownload en geïnstalleerd. Lees [hoe u installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

Als u wilt een VM maken met meerdere NIC's, de volgende stappen uit te voeren:

1. Selecteer de afbeelding van een VM uit de afbeeldingengalerie Azure VM. Houd er rekening mee dat afbeeldingen vaak veranderen en beschikbaar per regio zijn. De afbeelding die is opgegeven in het onderstaande voorbeeld mogelijk wijzigen of mogelijk niet in uw regio, daarom moet u opgeven van de afbeelding die u nodig hebt.

        $image = Get-AzureVMImage `
            -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"

1. Maak een VM-configuratie.

        $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
            -Image $image.ImageName –AvailabilitySetName "MyAVSet"

1. Maak de standaard-beheerder login.

        Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
            -Password "<YourAdminPassword>"

1. Extra NIC's toevoegen aan de VM-configuratie.

        Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
            -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
        Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
            -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm

1. Geef de subnet en IP-adres om de standaard-NIC

        Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
        Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm

1. Maak de VM in uw virtuele netwerk.

        New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm

>[AZURE.NOTE] De VNet die u hier opgeeft moet al bestaan (zoals die worden genoemd in de vereisten). Een virtueel netwerk met de naam **MultiNIC-VNet**Hiermee geeft u het volgende voorbeeld.

## <a name="limitations"></a>Beperkingen

De volgende beperkingen zijn van toepassing wanneer u de functie van de multi-NIC gebruikt:

- Meerdere NIC VMs moet worden gemaakt in Azure virtuele netwerken (VNets). Niet-VNet VMs kan niet worden geconfigureerd met meerdere NIC's.
- Alle VMs in een set beschikbaarheid moeten gebruiken op meerdere NIC of één NIC Er kunnen niet een combinatie van meerdere NIC VMs en één NIC VMs binnen een set beschikbaarheid. Er gelden dezelfde regels voor VMs in een cloudservice.
- Een VM met één NIC kan niet worden geconfigureerd met meerdere NIC's (en vice versa) zodra deze is geïmplementeerd, zonder te verwijderen en opnieuw te maken.


## <a name="secondary-nics-access-to-other-subnets"></a>Secundaire NIC toegang tot andere subnetten

Standaard worden secundaire NIC's niet geconfigureerd met een standaard-gateway, vanwege waarin de verkeersstroom op de secundaire NIC's wordt worden beperkt zodat deze binnen hetzelfde subnet. Als de gebruikers inschakelen secundaire NIC's om te communiceren buiten hun eigen subnet wilt, moeten deze een fragment toevoegen in de tabel voor het configureren van de gateway, zoals hieronder beschreven.

>[AZURE.NOTE] VMs die zijn gemaakt voordat juli 2015 een standaard-gateway geconfigureerd voor alle NIC's wellicht. De standaard-gateway voor secundaire NIC niet verwijderd totdat deze VMs opnieuw worden gestart. Internetconnectiviteit kunt in besturingssystemen die gebruikmaken van de zwakke host routeren model, zoals Linux, verbreken als het verkeer ingress- en egress verschillende NIC's gebruikt.

### <a name="configure-windows-vms"></a>Windows VMs configureren

Stel dat er een Windows-VM met twee NIC's als volgt:

- Primaire NIC IP-adres: 192.168.1.4
- Secundaire NIC IP-adres: 192.168.2.5

De tabel voor het routeren van IPv4 voor deze VM er als volgt uit:

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

Zoals u ziet dat de standaard-route (0.0.0.0) alleen beschikbaar voor de primaire NIC is Niet is mogelijk toegang krijgen tot bronnen buiten het subnet voor de secundaire NIC, zoals hieronder wordt weergegeven:

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

Als u wilt een standaard-route toevoegen op de secundaire NIC, de volgende stappen uit te voeren:

1. Vanaf een opdrachtprompt, voert u de onderstaande opdracht om het nummer voor de secundaire NIC:

        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================

2. Zoals u ziet het tweede item in de tabel met een index 27 (in dit voorbeeld).
3. Vanaf de opdrachtprompt, voert u de opdracht **route toevoegen** zoals hieronder wordt weergegeven. In dit voorbeeld geeft u 192.168.2.1 als standaard-gateway voor de secundaire NIC:

        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27

4. Connectiviteit testen, gaat u terug naar de opdrachtprompt en wil ping een ander subnet van de secundaire NIC als weergegeven int eh voorbeeld hieronder:

        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128

5. U kunt ook de routetabel als u wilt controleren van de toegevoegde route, controleren, zoals hieronder wordt weergegeven:

        C:\Users\Administrator>route print

        ...

        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a>Linux VMs configureren

Voor Linux VMs, aangezien het standaardgedrag zwakke host-mailroutering gebruikt wordt aangeraden dat de secundaire NIC's beperkt tot verkeersstromen alleen binnen hetzelfde subnet zijn. Echter als bepaalde scenario's connectivity buiten het subnet, gebruikers moeten inschakelen beleid gebaseerd routering om ervoor te zorgen dat het verkeer ingress- en egress de dezelfde NIC gebruikt

## <a name="next-steps"></a>Volgende stappen

- Implementeren [MultiNIC VMs in een scenario 2 niveaus toepassing in een resourcemanager-implementatie](virtual-network-deploy-multinic-arm-template.md).
- Implementeren [MultiNIC VMs in een scenario 2 niveaus toepassing in een klassieke implementatie](virtual-network-deploy-multinic-classic-ps.md).
