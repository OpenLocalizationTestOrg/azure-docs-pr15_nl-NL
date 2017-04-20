<properties 
   pageTitle="Problemen met netwerk beveiligingsgroepen - PowerShell | Microsoft Azure"
   description="Informatie over het oplossen van netwerk beveiligingsgroepen in het resourcemanager Azure implementatiemodel via Azure PowerShell."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-network-security-groups-using-azure-powershell"></a>Problemen met het netwerk beveiligingsgroepen via Azure PowerShell oplossen

> [AZURE.SELECTOR]
- [Azure-Portal](virtual-network-nsg-troubleshoot-portal.md)
- [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)

Als u beveiligingsgroepen netwerk (NSGs) voor uw VM (VM) hebt geconfigureerd en VM verbindingsproblemen zich voordoet, overzicht in dit artikel van mogelijkheden voor diagnostische hulpprogramma's voor NSGs om te helpen oplossen verder.

NSGs kunt u bepalen welke typen verkeer die mailstroom en afmelden bij uw virtuele machines (VMs). NSGs kunnen worden toegepast op subnetten die in een Azure Virtual Network (VNet), netwerkinterfaces (NIC) of beide. De effectieve regels toegepast op een NIC zijn een aggregatie van de regels die in de NSGs toegepast op een NIC aanwezig en het is verbonden met subnet. Regels op deze NSGs kunnen soms een conflict veroorzaken met elkaar en van een VM netwerkconnectiviteit van invloed zijn op.  

NIC's van uw VM toegepast, kunt u alle beveiligingsregels voor het effectieve van uw NSGs, weergeven. In dit artikel ziet hoe u problemen met VM verbindingen met behulp van deze regels in het implementatiemodel resourcemanager Azure. Als u niet bekend met VNet en NSG concepten bent, leest u de [virtuele netwerk](virtual-networks-overview.md) - en [netwerk beveiligingsgroepen](virtual-networks-nsg.md) overzicht artikelen.

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Effectieve beveiligingsregels gebruiken bij het oplossen van VM verkeersstroom

Het scenario die volgt is een voorbeeld van een algemene verbindingsprobleem:

Een benoemde *VM1* VM maakt deel uit van een subnet met *Subnet1* binnen een VNet *WestUS-VNet1*met de naam de naam. Een poging tot het verbinding maken met de VM RDP met TCP-poort 3389 gebruiken is mislukt. NSGs worden toegepast op zowel de NIC *VM1-NIC1* als het subnet *Subnet1*. TCP-poort 3389-verkeer is toegestaan in de NSG die is gekoppeld aan het netwerkinterface *VM1-NIC1*, maar TCP ping naar VM1 van poort 3389 mislukt.

Terwijl dit voorbeeld wordt TCP-poort 3389 gebruikt, kunnen de volgende stappen uit om te bepalen van binnenkomende en uitgaande verbindingsfouten via een poort worden gebruikt.

## <a name="detailed-troubleshooting-steps"></a>Gedetailleerde stappen voor probleemoplossing
Voer de volgende stappen uit om op te lossen NSGs voor een VM:

1. Start een Azure PowerShell-sessie en meld u aan bij Azure. Als u niet bekend bent met via Azure PowerShell, leest u het artikel [installeren en configureren van Azure PowerShell](../powershell-install-configure.md) .

2. Voer de volgende opdracht uit om terug te keren alle NSG regels toegepast op een NIC met de naam *VM1-NIC1* in de resourcegroep *RG1*:

        Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Als u de naam van een NIC niet weet, voert u de volgende opdracht uit om op te halen van de namen van alle NIC's in een resourcegroep: 
    
    >`Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name`

    De volgende tekst is een voorbeeld van de effectieve regels uitvoer geretourneerd voor de *VM1-NIC1* NIC:

        NetworkSecurityGroup   : {
                                   "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/VM1-NIC1-NSG"
                                 }
        Association            : {
                                   "NetworkInterface": {
                                     "Id": "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkInterfaces/VM1-NIC1"
                                   }
                                 }
        EffectiveSecurityRules : [
                                 {
                                 "Name": "securityRules/allowRDP",
                                 "Protocol": "Tcp",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "3389-3389",
                                 "SourceAddressPrefix": "Internet",
                                 "DestinationAddressPrefix": "0.0.0.0/0",
                                 "ExpandedSourceAddressPrefix": [… ],
                                 "ExpandedDestinationAddressPrefix": [],
                                 "Access": "Allow",
                                 "Priority": 1000,
                                 "Direction": "Inbound"
                                 },
                                 {
                                 "Name": "defaultSecurityRules/AllowVnetInBound",
                                 "Protocol": "All",
                                 "SourcePortRange": "0-65535",
                                 "DestinationPortRange": "0-65535",
                                 "SourceAddressPrefix": "VirtualNetwork",
                                 "DestinationAddressPrefix": "VirtualNetwork",
                                 "ExpandedSourceAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                 "ExpandedDestinationAddressPrefix": [
                                  "10.9.0.0/16",
                                  "168.63.129.16/32",
                                  "10.0.0.0/16",
                                  "10.1.0.0/16"
                                  ],
                                  "Access": "Allow",
                                  "Priority": 65000,
                                  "Direction": "Inbound"
                                  },…
                         ]
        
        NetworkSecurityGroup   : {
                                   "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/networkSecurityGroups/Subnet1-NSG"
                                 }
        Association            : {
                                   "Subnet": {
                                     "Id": 
                                 "/subscriptions/[Subscription ID]/resourceGroups/RG1/providers/Microsoft.Network/virtualNetworks/WestUS-VNet1/subnets/Subnet1"
                                 }
                                 }
        EffectiveSecurityRules : [
                                 {
                                "Name": "securityRules/denyRDP",
                                "Protocol": "Tcp",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "3389-3389",
                                "SourceAddressPrefix": "Internet",
                                "DestinationAddressPrefix": "0.0.0.0/0",
                                "ExpandedSourceAddressPrefix": [
                                   ... ],
                                "ExpandedDestinationAddressPrefix": [],
                                "Access": "Deny",
                                "Priority": 1000,
                                "Direction": "Inbound"
                                },
                                {
                                "Name": "defaultSecurityRules/AllowVnetInBound",
                                "Protocol": "All",
                                "SourcePortRange": "0-65535",
                                "DestinationPortRange": "0-65535",
                                "SourceAddressPrefix": "VirtualNetwork",
                                "DestinationAddressPrefix": "VirtualNetwork",
                                "ExpandedSourceAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "ExpandedDestinationAddressPrefix": [
                                "10.9.0.0/16",
                                "168.63.129.16/32",
                                "10.0.0.0/16",
                                "10.1.0.0/16"
                                ],
                                "Access": "Allow",
                                "Priority": 65000,
                                "Direction": "Inbound"
                                },...
                                ]

    Houd rekening met de volgende gegevens in de uitvoer:

    - Er zijn twee **NetworkSecurityGroup** secties: een is gekoppeld aan een subnet (*Subnet1*) en een is gekoppeld aan een NIC (*VM1-NIC1*). In dit voorbeeld is een NSG toegepast op elk.
    - **Koppeling** ziet u de resource (subnet of NIC) een bepaald NSG is gekoppeld. Als de resource NSG verplaatst/ontkoppeld is onmiddellijk voordat u deze opdracht, moet u mogelijk Wacht een paar seconden voor de wijziging doorgevoerd in de uitvoer van de opdracht. 
    - De namen van de regel die worden voorafgegaan door *defaultSecurityRules*: wanneer een NSG wordt gemaakt, worden diverse standaard beveiligingsregels gemaakt erin. Standaardregels kunnen niet worden verwijderd, maar ze kunnen worden overschreven met hogere prioriteitregels.
     Lees het artikel [NSG overzicht](virtual-networks-nsg.md#default-rules) voor meer informatie over NSG standaard beveiligingsregels.
    - **ExpandedAddressPrefix** wordt uitgebreid met de adresvoorvoegsels voor NSG standaardlabels. Markeringen bevatten meerdere adres voorvoegsels voor eenheden. Uitbreiding van de desbetreffende tags is soms handig bij het oplossen van VM connectivity specifieke adresvoorvoegsels/naar. Bijvoorbeeld, met VNET peering, VIRTUAL_NETWORK tag uitgebreid peered VNet voorvoegsels voor eenheden weergeven in de vorige uitvoer.

        >[AZURE.NOTE] De opdracht alleen toont effectieve regels als een NSG gekoppeld aan een subnet, een NIC, of beide is. Een VM mogelijk meerdere NIC's met verschillende NSGs toegepast. Als u wilt oplossen, voert u de opdracht voor elke NIC
        
3. Om te vereenvoudigen via een groot aantal NSG regels filteren, voert u de volgende opdrachten om op te lossen verder: 

        $NSGs = Get-AzureRmEffectiveNetworkSecurityGroup -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1
        $NSGs.EffectiveSecurityRules | Sort-Object Direction, Access, Priority | Out-GridView

    Een filter voor RDP-verkeer (TCP-poort 3389), wordt toegepast op de rasterweergave, zoals wordt weergegeven in de volgende afbeelding:

    ![Lijst met regels](./media/virtual-network-nsg-troubleshoot-powershell/rules.png)
    
4. Zoals u in de rasterweergave ziet, zijn er beide toestaan en regels voor RDP weigeren. De uitvoer uit stap 2 ziet u dat de regel *DenyRDP* zich in de NSG toegepast op het subnet. Voor binnenkomende regels worden toegepast op het subnet NSGs eerst verwerkt. Als een overeenkomst wordt gevonden, wordt de NSG toegepast op het netwerkinterface wordt niet verwerkt. In dit geval voorkomen dat de regel *DenyRDP* uit het subnet RDP voor VM (**VM1**).

    >[AZURE.NOTE] Een VM mogelijk meerdere NIC's die zijn gekoppeld. Elk kan niet worden verbonden met een ander subnet. Aangezien de opdrachten in de vorige stappen voor een NIC worden uitgevoerd, is het is belangrijk om ervoor te zorgen dat u opgeeft dat de NIC die u de niet-connectiviteit hebt. Als u niet precies weet, kunt u altijd de opdrachten voor elke NIC die zijn bijgevoegd bij de VM uitgevoerd.

5. Wijzig de regel *Weigeren RDP (3389)* naar *RDP(3389) toestaan* in het **Subnet1-NSG** NSG naar RDP in VM1. Controleer of TCP-poort 3389 door een RDP-verbinding met de VM te openen of het hulpprogramma voor het PsPing is. Vindt u meer informatie over PsPing door te lezen de [downloadpagina van PsPing](https://technet.microsoft.com/sysinternals/psping.aspx)

    U kunt of regels verwijderen uit een NSG met behulp van de gegevens in de uitvoer van de volgende opdracht uit:

        Get-Help *-AzureRmNetworkSecurityRuleConfig
        

## <a name="considerations"></a>Overwegingen

Houd rekening met de volgende punten bij het oplossen van problemen met de verbinding:

- Standaard NSG regels worden binnenkomende toegang van internet blokkeren en alleen toestaan VNet binnenkomend verkeer. Regels moeten expliciet worden toegevoegd aan de binnenkomende toegang vanaf Internet, zoals vereist toestaan.
- Als er geen NSG beveiligingsregels veroorzaakt door van een VM netwerkconnectiviteit mislukt, wordt het probleem mogelijk vanwege:
    - Firewallsoftware in het besturingssysteem van de VM draaien
    - Routes geconfigureerd voor virtual toestellen of on-premises implementatie-verkeer is toegestaan. Internetverkeer kan worden omgeleid naar de on-premises implementatie via gedwongen-tunneling. Een verbinding RDP/SSH van Internet voor uw VM werkt niet met deze instelling, afhankelijk van hoe de netwerkhardware on-premises implementatie dit verkeer verwerkt. Lees het artikel [Problemen oplossen Routes](virtual-network-routes-troubleshoot-powershell.md) om te leren hoe route diagnosticeren die mogelijk worden belemmeren de stroom van verkeer en afmelden bij de VM. 
- Als u hebt VNets, al dan niet standaard peered, wordt automatisch de tag VIRTUAL_NETWORK uitgebreid met voorvoegsels voor peered VNets. U kunt deze voorvoegsels weergeven in de lijst **ExpandedAddressPrefix** eventuele problemen met betrekking tot VNet peering connectivity. 
- Effectieve beveiligingsregels worden alleen weergegeven als er een NSG die is gekoppeld aan van de VM NIC en of subnet is. 
- Als er geen NSGs die is gekoppeld aan de NIC zijn of subnet en u een openbare IP-adres dat is toegewezen aan uw VM, worden alle poorten open voor binnenkomende en uitgaande toegang. Als de VM een openbare IP-adres heeft, wordt NSGs toepassen op de NIC of subnet aanbevolen.  
