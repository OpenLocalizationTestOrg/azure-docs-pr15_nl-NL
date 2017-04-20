<properties
   pageTitle="Overzicht van maximaal beschikbare configuraties met Azure VPN Gateways | Microsoft Azure"
   description="Dit artikel bevat een overzicht van maximaal beschikbare configuratieopties met Azure VPN Gateways."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/24/2016"
   ms.author="yushwang"/>

# <a name="highly-available-cross-premises-and-vnet-to-vnet-connectivity"></a>Maximaal beschikbare Cross-Premises en VNet-naar-VNet-connectiviteit

Dit artikel bevat een overzicht van maximaal beschikbare configuratieopties voor uw cross-premises en VNet-naar-VNet connectiviteit met Azure VPN gateways.

## <a name = "activestandby"></a>Over Azure VPN gateway redundantie

Elke gateway Azure VPN bestaat uit twee instanties in een actief stand-by-configuratie. Gepland onderhoud of niet-geplande onderbreking die gebeurt er met de actieve instantie, zou het exemplaar van de stand-by (uitvalbeveiliging) automatisch overnemen en hervatten de S2S VPN of een RAS-VNet-naar-VNet. De schakeloptie via treedt een kort onderbroken. Voor gepland onderhoud, moet de connectiviteit binnen 10 tot 15 seconden worden hersteld. Voor niet-geplande problemen wordt het herstelproces is verbinding niet langer, over 1 minuut met 1 en een halve minuten in het ongunstigste geval. De verbindingen P2S wordt verbroken en de gebruikers moeten opnieuw uit de clientcomputers verbinding te maken voor P2S VPN-clientverbindingen met de gateway.

![Actief stand-by-](./media/vpn-gateway-highlyavailable/active-standby.png)

## <a name="highly-available-cross-premises-connectivity"></a>Maximaal beschikbare Cross-Premises-connectiviteit

Betere beschikbaarheid bieden voor uw cross lokale verbindingen, zijn een aantal opties beschikbaar:

- Meerdere apparaten van de on-premises implementatie VPN
- Actieve Azure VPN gateway
- Combinatie van beide

### <a name = "activeactiveonprem"></a>Meerdere apparaten van de on-premises implementatie VPN

Kunt u meerdere VPN-apparaten van uw on-premises netwerk verbinding maken met uw gateway Azure VPN, zoals wordt weergegeven in het volgende diagram:

![Meerdere On-Premises VPN](./media/vpn-gateway-highlyavailable/multiple-onprem-vpns.png)

Deze configuratie biedt meerdere actieve tunnels van de gateway bij dezelfde Azure VPN naar uw on-premises implementatie-apparaten op dezelfde locatie. Er zijn enkele vereisten en voorwaarden:

1. U moet meerdere S2S VPN-verbindingen maken van uw apparaten VPN naar Azure. Wanneer u meerdere VPN-apparaten van de dezelfde on-premises netwerk met Azure verbindt, moet u één lokale netwerkgateway voor elk apparaat VPN en één verbinding maken via uw Azure VPN gateway bij naar de gateway lokale netwerk.

2. Het lokale netwerkgateways overeenkomt met uw VPN-apparaten moeten unieke openbare IP-adressen in de eigenschap "GatewayIpAddress".

3. BGP is vereist voor deze configuratie. Elke lokale netwerkgateway dat staat voor een VPN-apparaat moet een unieke IP-adres voor BGP peer opgegeven in de eigenschap "BgpPeerIpAddress".

4. Het veld van de eigenschap AddressPrefix in elke gateway lokale netwerk moet niet overlappen. U moet de "BgpPeerIpAddress" opgeeft in /32 CIDR-indeling in het veld AddressPrefix, bijvoorbeeld 10.200.200.254/32.

5. Reclame maken voor de dezelfde voorvoegsels voor eenheden van hetzelfde on-premises netwerk voorvoegsels voor eenheden tot uw gateway Azure VPN, en het verkeer worden doorgestuurd door deze tunnels tegelijk moet u BGP.

6. Elke verbinding wordt geteld ten opzichte van het maximum aantal tunnel voor uw gateway Azure VPN, 10 voor Basic en Standard SKU's, en 30 voor HighPerformance SKU. 

In deze configuratie wordt is de gateway Azure VPN nog steeds in actief stand-by-modus, zodat de overname bij dezelfde en kort onderbroken nog steeds er als beschreven [hierboven gebeurt](#activestandby). Maar deze instelling bewaakt tegen fouten of onderbrekingen op uw on-premises netwerk en een VPN-apparaten.
 
### <a name="active-active-azure-vpn-gateway"></a>Actieve Azure VPN gateway

U kunt nu een gateway Azure VPN maken in een actieve configuratie, waarbij beide exemplaren van de gateway-VMs S2S VPN tunnels naar uw on-premises implementatie VPN apparaat, stellen zoals wordt weergegeven in het volgende diagram:

![Actieve](./media/vpn-gateway-highlyavailable/active-active.png)

In deze configuratie wordt elk exemplaar Azure gateway heeft een unieke openbare IP-adres en elk maken met een tunnel IPsec/IKE S2S VPN naar uw on-premises implementatie VPN apparaat is opgegeven in uw lokale netwerkgateway en de verbinding. Houd er rekening mee dat beide tunnels VPN daadwerkelijk deel van de verbinding uitmaken. Nog steeds moet u voor het configureren van uw apparaat van de VPN on-premises implementatie als wilt accepteren of twee S2S VPN tunnels naar deze twee Azure VPN gateway openbare IP-adressen definiëren.

Omdat de Azure gateway-exemplaren in actieve configuratie, worden het verkeer van uw Azure virtuele netwerk naar uw on-premises netwerk gerouteerd via beide tunnels tegelijk, zelfs als uw apparaat van de VPN on-premises implementatie mogelijk voorkeur voor één tunnel elkaar. Hoewel de dezelfde TCP of UDP-stroom altijd dezelfde tunnel of pad bladeren wordt, opmerking tenzij een onderhoudsgebeurtenis gebeurt er op een van de exemplaren.

Als een gepland onderhoud of niet-geplande gebeurtenis op één gateway exemplaar gebeurt, kan de IPSec-tunnel uit die sessie aan uw apparaat van de VPN on-premises implementatie moet worden beëindigd. De bijbehorende routes op uw apparaten VPN moeten worden verwijderd of automatisch worden teruggetrokken, zodat het verkeer naar de andere actieve IPSec-tunnel via wordt worden veranderd. Klik aan de Azure de schakeloptie via gebeurt er automatisch vanuit het desbetreffende exemplaar met het actieve exemplaar.

### <a name="dual-redundancy-active-active-vpn-gateways-for-both-azure-and-on-premises-networks"></a>Twee-redundantie: actieve VPN gateways voor zowel Azure en on-premises-netwerken te gebruiken

De meest betrouwbare optie is om te combineren van de gateways actieve op uw netwerk- en Azure, zoals wordt weergegeven in het onderstaande diagram.

![Schermafbeelding van twee redundantie](./media/vpn-gateway-highlyavailable/dual-redundancy.png)

Hier u maken en instellen van de gateway Azure VPN in een actieve-configuratie en u maakt twee lokale netwerkgateways en twee verbindingen voor uw twee on-premises VPN-apparaten zoals hierboven is beschreven. Het resultaat is een volledige NET connectivity 4 IPsec tunnel tussen uw Azure virtuele netwerk en uw on-premises netwerk.

Alle gateways en tunnels zijn actief van de Azure kant, zodat het verkeer worden verdeeld tussen alle 4 tunnels tegelijk, hoewel de stroom van elke TCP of UDP opnieuw de dezelfde tunnel of het pad van de Azure-kant opvolgt. Hoewel door het verkeer spreiden, u iets hogere doorvoersnelheid via de IPSec-tunnels ziet mogelijk, wordt het primaire doel van deze configuratie voor maximale beschikbaarheid is. En vanwege de statistische aard van de verspreiding, het is moeilijk te bieden de maateenheden van verschillende voorwaarden van toepassing verkeer is van invloed op de geaggregeerde doorvoer.

Deze topologie moeten worden twee lokale netwerkgateways en twee verbindingen met de ondersteuning voor de combinatie van on-premises implementatie VPN-apparaten en BGP is verplicht voor het toestaan van de twee verbindingen met de dezelfde on-premises netwerk. Deze vereisten zijn hetzelfde als de [hierboven](#activeactiveonprem). 

## <a name="highly-available-vnet-to-vnet-connectivity-through-azure-vpn-gateways"></a>Maximaal beschikbare VNet-naar-VNet verbinding via Azure VPN Gateways

Dezelfde actieve configuratie kan ook toegepast op Azure VNet-naar-VNet verbindingen. U kunt actieve VPN gateways voor beide virtuele netwerken maken en ze met elkaar verbinden aan formulier de dezelfde volledige NET-connectiviteit 4 tunnel tussen de twee VNets, zoals wordt weergegeven in het onderstaande diagram:

![VNet-naar-VNet](./media/vpn-gateway-highlyavailable/vnet-to-vnet.png)

Dit zorgt ervoor dat er altijd een paar tunnel tussen de twee virtuele netwerken op gepland onderhoud gebeurtenissen, nog betere beschikbaarheid leveren. Hoewel de dezelfde topologie voor cross-premises connectivity vereist twee verbindingen, moet de VNet-naar-VNet topologie hierboven slechts één verbinding voor elke gateway. Bovendien is BGP optioneel tenzij overdracht routering via de VNet-naar-VNet-verbinding vereist is.


## <a name="next-steps"></a>Volgende stappen

Zie [Actieve VPN Gateways voor Cross-Premises en VNet-naar-VNet verbindingen configureren](vpn-gateway-activeactive-rm-powershell.md) voor stapsgewijze instructies voor het configureren van actieve cross-premises en VNet-naar-VNet verbindingen.
