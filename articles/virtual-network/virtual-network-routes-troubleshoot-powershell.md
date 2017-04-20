<properties 
   pageTitle="Problemen met routes - PowerShell | Microsoft Azure"
   description="Informatie over het oplossen van routes in het resourcemanager Azure implementatiemodel via Azure PowerShell."
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

# <a name="troubleshoot-routes-using-azure-powershell"></a>Problemen met routes via Azure PowerShell

> [AZURE.SELECTOR]
- [Azure-Portal](virtual-network-routes-troubleshoot-portal.md)
- [PowerShell](virtual-network-routes-troubleshoot-powershell.md)

Als er problemen met de netwerkverbinding naar of uit uw Azure Virtual Machine (VM), routes mogelijk worden die invloed hebben op uw verkeersstromen VM. Dit artikel bevat een overzicht van mogelijkheden voor diagnostische hulpprogramma's voor routes om te helpen oplossen verder.

Routetabellen zijn gekoppeld aan subnetten en effectieve op alle netwerkinterfaces (NIC) in dat subnet. De volgende soorten routes kunnen worden toegepast op elke netwerkinterface:

- **Systeem routes:** Standaard heeft elke subnetten die is gemaakt in een Azure Virtual Network (VNet) systeem routetabellen die lokaal VNet verkeer, on-premises implementatie verkeer via VPN gateways en internetverkeer toestaan. Er zijn ook systeem routes voor peered VNets.
- **BGP routes:** Doorgegeven aan netwerkinterfaces via ExpressRoute of site-naar-site VPN-verbindingen. Meer informatie over BGP routering Lees de artikelen [BGP met VPN gateways](../vpn-gateway/vpn-gateway-bgp-overview.md) en het [overzicht van de ExpressRoute](../expressroute/expressroute-introduction.md) .
- **Door gebruiker gedefinieerd routes (UDR):** Als u virtuele netwerkapparaten of worden gebruikt u wordt gedwongen-tunneling verkeer naar een on-premises netwerk via een VPN-verbinding voor de site-naar-site, hebt u wellicht een gebruiker gedefinieerde routes (UDRs) die zijn gekoppeld aan uw subnet route-tabel. Als u niet bekend met UDRs bent, leest u het artikel [de gebruiker gedefinieerde routes](virtual-networks-udr-overview.md#user-defined-routes) .

Met de verschillende routes die kunnen worden toegepast op een netwerkinterface, kan het lastig zijn om te bepalen welke statistische routes effectieve. Voor het oplossen VM netwerkconnectiviteit, kunt u de effectieve routes voor de netwerkinterface van een weergeven in het implementatiemodel resourcemanager Azure.

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a>Effectieve Routes gebruiken bij het oplossen van VM verkeersstroom

In dit artikel wordt het volgende scenario als voorbeeld gebruikt om te laten zien hoe u problemen met de effectieve routes voor de netwerkinterface van een:

Een VM (*VM1*) die zijn verbonden met de VNet (*VNet1*, voorvoegsel: 10.9.0.0/16) geen verbinding met een VM(VM3) in een nieuw peered VNet (*VNet3*, voorvoegsel 10.10.0.0/16). Er zijn geen UDRs of BGP stuurt toegepast op VM1-NIC1 netwerkinterface verbonden met de VM, alleen de routes van het systeem zijn toegepast.

In dit artikel wordt uitgelegd hoe de oorzaak van het verbindingsfout effectieve routes mogelijkheid gebruiken in Azure Resource beheermodel implementatie.
Terwijl het voorbeeld wordt alleen de routes van het systeem gebruikt, kunnen de dezelfde stappen om te bepalen van binnenkomende en uitgaande verbindingsfouten op elk gewenst routetype worden gebruikt.

>[AZURE.NOTE] Als uw VM meer dan één NIC die zijn gekoppeld heeft, schakelt u effectieve routes voor elk van de NIC's naar een diagnose stellen bij problemen met de netwerkverbindingen en naar een VM.

### <a name="view-effective-routes-for-a-virtual-machine"></a>De effectieve routes weergeven voor een virtuele machine

Als u wilt zien van de statistische routes die zijn toegepast op een VM, moet u de volgende stappen uitvoeren:

### <a name="view-effective-routes-for-a-network-interface"></a>De effectieve routes weergeven voor een netwerkinterface

Als u wilt zien van de statistische routes die zijn toegepast op een netwerkinterface, moet u de volgende stappen uitvoeren:

1.  Start een Azure PowerShell-sessie en meld u aan bij Azure. Als u niet bekend met Azure PowerShell bent, leest u het artikel [installeren en configureren van Azure PowerShell](../powershell-install-configure.md) .

2.  De volgende opdracht retourneert alle routes die zijn toegepast op een netwerkinterface met de naam *VM1-NIC1* in de resourcegroep *RG1*.

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1

    >[AZURE.TIP] Als u de naam van een netwerkinterface niet weet, typt u de volgende opdracht uit om op te halen van de namen van alle netwerkinterfaces in een resource group.*

        Get-AzureRmNetworkInterface -ResourceGroupName RG1 | Format-Table Name

    De volgende uitvoer ziet er ongeveer als de uitvoer voor elke route toegepast op het subnet dat de NIC is verbonden met:

        Name :
        State : Active
        AddressPrefix : {10.9.0.0/16}
        NextHopType : VNetLocal
        NextHopIpAddress : {}

        Name :
        State : Active
        AddressPrefix : {0.0.0.0/16}
        NextHopType : Internet
        NextHopIpAddress : {}

    Let op de volgende handelingen uit in de uitvoer:
    - **Naam**: naam van de effectieve route mogelijk leeg, tenzij expliciet is opgegeven voor de gebruiker gedefinieerde routes. 
    - **De staat**: status van de effectieve route aangeeft. Mogelijke waarden zijn 'Actief' of "Ongeldige"
    - **AddressPrefixes**: Hiermee geeft u het adresvoorvoegsel van de effectieve route in CIDR-notatie. 
    - **nextHopType**: geeft aan de volgende hop voor de opgegeven route. Mogelijke waarden zijn *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*of *Null*. Een waarde *Null* voor **nextHopType** in een UDR kan duiden op een ongeldige route. Als **nextHopType** *VirtualAppliance* en het virtuele netwerk is is toestel VM bijvoorbeeld niet in een deze is ingericht/voorlopig staat. Als **nextHopType** *VPNGateway* is en er geen gateway deze is ingericht is/uitvoeren in de opgegeven VNet, wordt de route ongeldig.
    - **NextHopIpAddress**: het IP-adres van de volgende hop van de effectieve route.
    
    De volgende opdracht geeft als resultaat de routes in een gemakkelijker om weer te geven van de tabel:

        Get-AzureRmEffectiveRouteTable -NetworkInterfaceName VM1-NIC1 -ResourceGroupName RG1 | Format-Table

    De volgende uitvoer is enkele van de uitvoer ontvangen voor het scenario die eerder is beschreven:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {0.0.0.0/0} Internet {}
    

3. Er is geen route vermeld op de *WestUS-VNet3* VNet (voorvoegsel 10.10.0.0/16)* * uit *WestUS-VNet1* (voorvoegsel 10.9.0.0/16) in de uitvoer van de vorige stap. Zoals u in de volgende afbeelding, is de VNet peering koppeling met de VNet *WestUS-VNet3* in de stand *verbroken* .
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    De koppeling bidirectionele voor de peering verbroken is, waarin wordt uitgelegd waarom VM1 kan geen verbinding maken met VM3 in de VNet *WestUS-VNet3* . Een bidirectionele VNet peering koppeling opnieuw instellen voor *WestUS-VNet1* en *WestUS-VNet3* VNets. Voert u de uitvoer geretourneerd nadat de VNet peering koppeling juist is gemaakt:

        Name State AddressPrefix NextHopType NextHopIpAddress
        ---- ----- ------------- ----------- ----------------
        Active {10.9.0.0/16} VnetLocal {}
        Active {10.10.0.0/16} VNetPeering {}
        Active {0.0.0.0/0} Internet {}
        
    Nadat u het probleem vastgesteld, kunt u toevoegen, verwijderen, of routes wijzigen en doorsturen van tabellen. Typ de volgende opdracht uit voor een overzicht van de opdrachten kunt doen:

        Get-Help *-AzureRmRouteConfig

## <a name="considerations"></a>Overwegingen

Een paar dingen in gedachten moet houden bij het controleren van de lijst met routes geretourneerd:

- Routering is gebaseerd op langste voorvoegsel vergelijken (LPM) tussen UDRs, routes BGP en systeem. Als er meer dan één route met de dezelfde LPM overeenkomen, is klikt u vervolgens een route geselecteerd op basis van het beginpunt in de volgende volgorde:
    - De gebruiker gedefinieerde doorsturen
    - BGP doorsturen
    - Route systeem (standaard)

    Met effectieve routes, kunt u alleen effectieve routes LPM vergelijken op basis van alle er routes zien. Door te laten zien hoe de routes daadwerkelijk worden geëvalueerd voor een bepaald NIC, hierdoor veel eenvoudiger om op te lossen specifieke routes die mogelijk worden die invloed hebben op connectivity uw VM/naar.

- Als u UDRs en verkeer naar een netwerk virtuele toestel (NVA), met *VirtualAppliance* als **nextHopType**verzendt, moet u controleren of IP-doorsturen is ingeschakeld op de NVA ontvangen het verkeer of pakketten verloren gaan. 
- Als geforceerde tunnel is ingeschakeld, worden alle uitgaande internetverkeer worden doorgestuurd naar de on-premises implementatie. RDP/SSH van Internet voor uw VM werkt niet met deze instelling, afhankelijk van hoe dit verkeer in de on-premises implementatie worden verwerkt. 
  U wordt gedwongen-tunneling kan worden ingeschakeld:
    - Als site-naar-site VPN, gebruikt door een gebruiker gedefinieerde route (UDR) met nextHopType als VPN Gateway
    - Als u een standaard-route wordt aangekondigd via BGP
- Voor VNet peering verkeer naar correct werkt, moet de route van een systeem met **nextHopType** *VNetPeering* voor de peered VNet voorvoegsel bereik bestaan. Als deze een route niet bestaat en de peering VNet-koppeling wordt gezocht OK:
    - Wacht een paar seconden en probeer het opnieuw als u een nieuw waarop deze is opgezet peering koppeling. Soms duurt langer routes naar alle netwerkinterfaces in een subnet doorgeven.
    - Regels voor netwerk-beveiligingsgroep (NSG) het mogelijk worden die invloed hebben op de verkeersstromen. Zie het artikel [Problemen met netwerk beveiligingsgroepen](virtual-network-nsg-troubleshoot-powershell.md) voor meer informatie.
