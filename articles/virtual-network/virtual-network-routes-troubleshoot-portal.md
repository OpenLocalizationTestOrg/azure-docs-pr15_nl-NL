<properties 
   pageTitle="Problemen met routes - Portal | Microsoft Azure"
   description="Informatie over het oplossen van routes in het resourcemanager Azure implementatiemodel met behulp van de Azure-Portal."
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

# <a name="troubleshoot-routes-using-the-azure-portal"></a>Problemen met routes met behulp van de Azure-Portal

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

1. Meld u aan bij van de Azure-portal op https://portal.azure.com.
2. Klik op **meer services**en klik op **virtuele machines** in de lijst die wordt weergegeven.
3. Selecteer een VM om op te lossen in de lijst die wordt weergegeven en een blade VM met opties wordt weergegeven.
4. Klik op **Diagnose & oplossen van problemen** en selecteer vervolgens een veelvoorkomend probleem. In dit voorbeeld is **ik kan geen verbinding maken met mijn Windows-VM** geselecteerd. 

    ![](./media/virtual-network-routes-troubleshoot-portal/image1.png)

5. Stappen die wordt weergegeven onder het probleem, zoals wordt weergegeven in de volgende afbeelding: 

    ![](./media/virtual-network-routes-troubleshoot-portal/image2.png)

    Klik op *effectieve routes* in de lijst met aanbevolen stappen.

6. Het blad **effectieve routes** wordt weergegeven, zoals wordt weergegeven in de volgende afbeelding:

    ![](./media/virtual-network-routes-troubleshoot-portal/image3.png)

    Als uw VM slechts één NIC heeft, wordt het al dan niet standaard geselecteerd. Als u meer dan één NIC hebt, selecteert u de NIC waarvoor u wilt de effectieve routes weergeven.

    >[AZURE.NOTE] Als de VM die is gekoppeld aan de NIC niet actief is, wordt niet effectieve routes weergegeven. Alleen de eerste 200 effectieve routes worden weergegeven in de portal. Voor de volledige lijst, klikt u op **downloaden**. Verder kunt u filteren op de resultaten van het gedownloade CSV-bestand.

    Let op de volgende handelingen uit in de uitvoer:
    - **Bron**: aangegeven welk type doorsturen. Systeem routes worden weergegeven als *standaard*, UDRs worden weergegeven als *gebruiker* en de gateway routes (statische of BGP) worden weergegeven als *VPNGateway*.
    - **De staat**: status van de effectieve route aangeeft. Mogelijke waarden zijn *actief* of *Ongeldige*.
    - **AddressPrefixes**: Hiermee geeft u het adresvoorvoegsel van de effectieve route in CIDR-notatie. 
    - **nextHopType**: geeft aan de volgende hop voor de opgegeven route. Mogelijke waarden zijn *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*of *Null*. Een waarde *Null* voor **nextHopType** in een UDR kan duiden op een ongeldige route. Als **nextHopType** *VirtualAppliance* en het virtuele netwerk is is toestel VM bijvoorbeeld niet in een deze is ingericht/voorlopig staat. Als **nextHopType** *VPNGateway* is en er geen gateway deze is ingericht is/uitvoeren in de opgegeven VNet, wordt de route ongeldig.
    
7. Er is geen route vermeld de *WestUS-VNET3* VNet (voorvoegsel 10.10.0.0/16) van de *WestUS-VNet1* (voorvoegsel 10.9.0.0/16) in de afbeelding in de vorige stap. In de volgende afbeelding worden de peering koppeling in de stand *verbroken* :
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    De koppeling bidirectionele voor de peering verbroken is, waarin wordt uitgelegd waarom VM1 kan geen verbinding maken met VM3 in de VNet *WestUS-VNet3* .

8. De volgende afbeelding ziet de routes na de bidirectionele peering koppeling tot stand brengen:

    ![](./media/virtual-network-routes-troubleshoot-portal/image5.png)

Lees voor meer voor probleemoplossing scenario's voor u wordt gedwongen-tunneling en doorsturen van evaluatie in de sectie [overwegingen](virtual-network-routes-troubleshoot-portal.md#Considerations) van dit artikel.

### <a name="view-effective-routes-for-a-network-interface"></a>De effectieve routes weergeven voor een netwerkinterface

Als het netwerkverkeer wordt beïnvloed voor een bepaald netwerk-interface (NIC), kunt u een volledige lijst met effectieve routes rechtstreeks op een NIC weergeven. Als u wilt zien van de statistische routes die zijn toegepast op een NIC, moet u de volgende stappen uitvoeren:

1. Meld u aan bij van de Azure-portal op https://portal.azure.com.
2. Klik op **meer services**en klik op **netwerk-interfaces**
3. Zoek in de lijst voor de naam van een NIC of selecteert u deze in de lijst die wordt weergegeven. In dit voorbeeld is **VM1-NIC1** ingeschakeld.
4. Selecteer in het blad **netwerkinterface** **effectieve routes** zoals wordt weergegeven in de volgende afbeelding:
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image6.png)

    Het **bereik** standaard ingesteld op het netwerkinterface geselecteerd.

    ![](./media/virtual-network-routes-troubleshoot-portal/image7.png)


### <a name="view-effective-routes-for-a-route-table"></a>De effectieve routes weergeven voor een tabel

Als u de gebruiker gedefinieerde routes (UDRs) wijzigt in een tabel, is het raadzaam wilt zien wat de invloed van de routes op een bepaalde VM wordt toegevoegd. Een tabel kan worden gekoppeld aan een willekeurig aantal subnetten. Nu kunt u de effectieve routes weergeven voor alle NIC's die een bepaalde route-tabel is toegepast, zonder dat u moet schakelen van de context van het blad bepaalde route-tabel.

In dit voorbeeld wordt een UDR (*UDRoute*) opgegeven in een tabel (*UDRouteTable*). Deze route verzendt alle internetverkeer van *Subnet1* in de VNet *WestUS-VNet1* via een netwerk virtuele toestel (NVA), in *Subnet2* van de dezelfde VNet. In de volgende afbeelding ziet u de route:

![](./media/virtual-network-routes-troubleshoot-portal/image8.png)

Als u wilt zien van de statistische routes voor een tabel, kunt u de volgende stappen uitvoeren:

1. Meld u aan bij van de Azure-portal op https://portal.azure.com.
2. Klik op **meer services**en klik op **Route-tabellen**
3. Zoek in de lijst voor de routetabel die u wilt zien van statistische routes voor en selecteer deze. In dit voorbeeld is **UDRouteTable** ingeschakeld. Een blade voor de geselecteerde route-tabel wordt weergegeven, zoals wordt weergegeven in de volgende afbeelding:

    ![](./media/virtual-network-routes-troubleshoot-portal/image9.png)

4. Selecteer **Effectieve Routes** in het blad **Route-tabel** . Het **bereik** is ingesteld op de routetabel die u hebt geselecteerd.
5. Een tabel kan worden toegepast op meerdere subnetten. Selecteer het **Subnet** dat u wilt bekijken in de lijst. In dit voorbeeld is **Subnet1** ingeschakeld.
6. Selecteer een **netwerkinterface**. Alle NIC's die zijn verbonden met het geselecteerde subnet worden vermeld. In dit voorbeeld is **VM1-NIC1** ingeschakeld.

    ![](./media/virtual-network-routes-troubleshoot-portal/image10.png)

    >[AZURE.NOTE] Als de NIC niet gekoppeld aan een actieve VM is, worden geen effectieve routes weergegeven.

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
    - Regels voor netwerk-beveiligingsgroep (NSG) het mogelijk worden die invloed hebben op de verkeersstromen. Zie het artikel [Problemen met netwerk beveiligingsgroepen](virtual-network-nsg-troubleshoot-portal.md) voor meer informatie.
