<properties 
   pageTitle="Virtual Network VPN Gateway Veelgestelde vragen over | Microsoft Azure"
   description="De Gateway VPN Veelgestelde vragen. Veelgestelde vragen over Microsoft Azure Virtual cross-premises netwerkverbindingen, hybride configuratie verbindingen en VPN Gateways"
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/10/2016"
   ms.author="yushwang" />

# <a name="vpn-gateway-faq"></a>VPN Gateway Veelgestelde vragen

## <a name="connecting-to-virtual-networks"></a>Verbinding maken met virtuele netwerken

### <a name="can-i-connect-virtual-networks-in-different-azure-regions"></a>Kan ik virtuele netwerken in verschillende Azure regio's verbinding maken?
Ja. Er is in feite geen beperking regio. Een virtueel netwerk kunt verbinding maken met een ander virtual netwerk in dezelfde regio of in een ander gebied van de Azure.

### <a name="can-i-connect-virtual-networks-in-different-subscriptions"></a>Kan ik virtuele netwerken in de verschillende abonnementen verbinding maken?
Ja.

### <a name="can-i-connect-to-multiple-sites-from-a-single-virtual-network"></a>Kan ik verbinding maken met meerdere sites uit één virtuele netwerk?

U kunt meerdere sites verbinden met behulp van Windows PowerShell en de Azure REST API's. Zie het gedeelte [met meerdere locaties en VNet-naar-VNet-connectiviteit](#multi-site-and-vnet-to-vnet-connectivity) Veelgestelde vragen.
## <a name="what-are-my-cross-premises-connection-options"></a>Wat zijn mijn verbindingsopties cross-premises?

De volgende cross-premises-verbindingen worden ondersteund:

- [Site-naar-Site](vpn-gateway-site-to-site-create.md) : VPN-verbinding via IPsec (IKE v1 en IKE v2). Dit type verbinding vereist een VPN-apparaat of RRAS.

- [Punt-naar-Site](vpn-gateway-point-to-site-create.md) – VPN-verbinding via SSTP (Secure Socket Tunneling Protocol). Deze verbinding hoeft niet een VPN-apparaat.

- [VNet-naar-VNet](virtual-networks-configure-vnet-to-vnet-connection.md) – dit type verbinding is hetzelfde als de configuratie van een Site-naar-Site. VNet naar VNet is een VPN-verbinding via IPsec (IKE v1 en IKE v2). Hoeft niet een VPN-apparaat.

- [Meerdere locaties](vpn-gateway-multi-site.md) : dit is een variatie op een Site op Site-configuratie waarmee u kunt meerdere on-premises implementatie sites verbinden door een virtueel netwerk.

- [ExpressRoute](../expressroute/expressroute-introduction.md) – ExpressRoute is een directe verbinding met Azure uit uw WAN, niet via de openbare Internet. Zie de [ExpressRoute technisch overzicht](../expressroute/expressroute-introduction.md) en de [Veelgestelde vragen over ExpressRoute](../expressroute/expressroute-faqs.md) voor meer informatie.

Zie voor meer informatie over verbindingen, [Over VPN Gateway](vpn-gateway-about-vpngateways.md).

### <a name="what-is-the-difference-between-a-site-to-site-connection-and-point-to-site"></a>Wat is het verschil tussen een Site-naar-Site verbinding en punt-naar-Site?

**Site-naar-Site** verbindingen kunnen u verbinding maken tussen een van de computers in uw lokale naar een virtuele machine of exemplaar van de rol binnen het virtuele netwerk, afhankelijk van hoe u wilt configureren routering. Er is een handige optie voor een altijd beschikbaar cross lokale verbinding en is zeer geschikt voor hybride configuraties. Dit type verbinding, is afhankelijk van een toestel voor IPsec VPN (hardware of vloeiende toestel), die moet worden geïmplementeerd op de rand van het netwerk. Als u wilt dit type verbinding hebt gemaakt, moet u de vereiste VPN hardware en een extern omlaag IPv4-adres.

**Punt-naar-Site** verbindingen kunnen u verbinding maken vanaf één computer vanaf elke locatie op een andere waarde in het virtuele netwerk bevindt. De Windows in het vak VPN-client wordt gebruikt. Als onderdeel van de configuratie punt-naar-Site installeert u een certificaat en een VPN-client configuratiepakket, waarin de instellingen die uw computer om verbinding met een virtuele machine of exemplaar van de rol binnen het virtuele netwerk toestaan. Dit is handig wanneer u verbinding maakt met een virtueel netwerk, maar u niet bevindt on-premises implementatie. Het is ook een goede optie als u geen toegang tot VPN hardware of een extern omlaag IPv4-adres, die beide vereist voor de verbinding van een Site-naar-Site zijn. 

U kunt uw virtuele netwerk gelijktijdig, gebruik van zowel Site-naar-Site en punt-naar-Site configureren, mits u de Site-naar-Site verbinding maken met een VPN-route gebaseerde type voor de gateway. Route gebaseerde VPN typen heten dynamische gateways in het implementatiemodel klassieke.

### <a name="what-is-expressroute"></a>Wat is ExpressRoute?

ExpressRoute kunt u persoonlijke verbindingen tussen Microsoft datacenters en dat op uw locatie of in een omgeving met anderen samenwerkt locatie-infrastructuur maken. U kunt met ExpressRoute, vergaderen met Microsoft cloudservices zoals Microsoft Azure en Office 365 bij een ExpressRoute partner samen locatie faciliteit of rechtstreeks verbinding te maken van uw bestaande WAN-netwerk (zoals een MPLS VPN verstrekt door een netwerkprovider).

ExpressRoute verbindingen bieden betere beveiliging, meer betrouwbaarheid hogere bandbreedte en lagere vertragingstijden dan de normale verbindingen via Internet. In sommige gevallen ExpressRoute verbindingen gebruiken om gegevens te brengen tussen uw on-premises netwerk en Azure kan ook worden verkregen aanzienlijk kostenvoordelen. Als u al een cross lokale verbinding uit uw on-premises netwerk naar Azure gemaakt hebt, kunt u migreren naar een verbinding ExpressRoute terwijl uw virtuele netwerk intact blijft.

Raadpleeg de [Veelgestelde vragen over ExpressRoute](../expressroute/expressroute-faqs.md) voor meer informatie.

## <a name="site-to-site-connections-and-vpn-devices"></a>Site-naar-Site verbindingen en VPN-apparaten

### <a name="what-should-i-consider-when-selecting-a-vpn-device"></a>Wat moet ik rekening houden bij het selecteren van een VPN-apparaat?

We hebben een reeks standaard naar website VPN apparaten in samenwerking met leveranciers van apparaat gevalideerd. Een lijst met bekende compatibele VPN apparaten, hun bijbehorende configuratie-instructies of voorbeelden en apparaat specificaties vindt u [hier](vpn-gateway-about-vpn-devices.md). Alle apparaten in het apparaat gezinnen vermeld als compatibel zijn met bekende moeten samenwerken met Virtual Network. Om u te helpen met het configureren van uw apparaat VPN, raadpleegt u het apparaat configuratie voorbeeld of de koppeling die met de juiste apparaat familie overeenkomt.

### <a name="what-do-i-do-if-i-have-a-vpn-device-that-isnt-in-the-known-compatible-device-list"></a>Wat moet ik doen als ik een VPN-apparaat, dat zich niet in de lijst met bekende compatibele apparaten heb?

Als uw apparaat vermeld als een bekende compatibele VPN apparaat niet wordt weergegeven en u wilt gebruiken voor uw VPN-verbinding, moet u controleren of deze aan de ondersteunde IPsec/IKE configuratie opties en parameters vermelden [hier](vpn-gateway-about-vpn-devices.md#devices-not-on-the-compatible-list)voldoet. Apparaten die voldoen aan de minimale vereisten moeten werken ook met VPN gateways. Contact opnemen met de apparaatfabrikant van uw voor extra ondersteuning en configuratie-instructies.

### <a name="why-does-my-policy-based-vpn-tunnel-go-down-when-traffic-is-idle"></a>Waarom Mijn VPN-beleid gebaseerde tunnel omlaag gaan als verkeer niet actief is?

Dit is normaal voor op basis van beleid (ook wel bekend als statische routering) VPN gateways. Als het verkeer via de tunnel gedurende meer dan 5 minuten niet actief is, wordt de tunnel worden verwijderd. Wanneer verkeer wordt gestart die doorloopt in beide richtingen, wordt de tunnel direct worden gebracht.

### <a name="can-i-use-software-vpns-to-connect-to-azure"></a>Kan ik verbinding maken met Azure software VPN's gebruiken?

We ondersteunen routering van Windows Server 2012 en Remote Access (RRAS)-servers voor Site-naar-Site cross-premises configuratie.

Andere oplossingen van de VPN software moeten samenwerken met onze gateway zo lang maken als ze aan industriële standaard IPSec-implementaties voldoen. Neem contact op met de leverancier van de software voor configuratie en ondersteuning voor instructies.

## <a name="point-to-site-connections"></a>Verbindingen punt-naar-Site

### <a name="what-operating-systems-can-i-use-with-point-to-site"></a>Welke besturingssystemen kan ik gebruiken met punt-naar-Site?

De volgende besturingssystemen worden ondersteund:

- Windows 7 (32-bits en 64-bits)

- Windows Server 2008 R2 (alleen 64-bits)

- Windows 8 (32-bits en 64-bits)

- Windows 8.1 (32-bits en 64-bits)

- Windows Server 2012 (alleen 64-bits)

- Windows Server 2012 R2 (alleen 64-bits)

- Windows 10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Kan ik een VPN-client voor software voor punt-naar-Site die ondersteuning biedt voor SSTP gebruiken?

Nee. Ondersteuning is beperkt tot het Windows-besturingssysteemversies hierboven wordt genoemd.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Hoeveel VPN-client eindpunten kan ik hebben in de configuratie van mijn punt-naar-Site?

Microsoft ondersteuning voor maximaal 128 VPN-clients de mogelijkheid verbinding maken met een virtueel netwerk op hetzelfde moment.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Kan ik mijn eigen interne PKI basiscertificeringsinstantie voor connectivity punt-naar-Site gebruiken?

Ja. Eerder, alleen zelfondertekende basiscertificaten kunnen worden gebruikt. U kunt nog steeds 20 basiscertificaten uploaden.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Kan ik bladeren proxy's en firewalls met de mogelijkheid punt-naar-Site?

Ja. We SSTP (Secure Socket Tunneling Protocol) gebruiken om via firewalls. Deze tunnel wordt weergegeven als een HTTPs-verbinding.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Als ik een clientcomputer die is geconfigureerd voor punt-naar-Site opnieuw opstart, wordt de VPN automatisch opnieuw verbinding maken?

Standaard wordt de clientcomputer niet hersteld de VPN-verbinding automatisch.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Wordt punt-naar-Site ondersteuning auto-opnieuw verbinding maakt en DDNS op de VPN-clients?

Automatisch opnieuw en DDNS worden momenteel niet ondersteund in VPN's punt-naar-Site.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Kan ik naar website hebben en punt-naar-Site configuraties naast elkaar gebruiken voor het dezelfde virtuele netwerk?

Ja. Als u een type RouteBased VPN voor uw gateway hebt werkt beide deze oplossingen. Voor het implementatiemodel klassieke moet u een dynamische gateway. We doen niet ondersteuning punt-naar-Site voor statische routeren VPN gateways of gateways - VpnType PolicyBased met.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Kan ik een punt-naar-Site-client verbinding maakt met meerdere virtuele netwerken op hetzelfde moment geconfigureerd?

Ja, is het mogelijk. Maar de virtuele netwerken kunnen geen bevatten overlappende IP-voorvoegsels voor eenheden en moeten de punt-naar-Site adres spaties tussen de virtuele netwerken elkaar niet overlappen.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Hoeveel doorvoer kan ik verwachten via verbindingen naar website of punt-naar-Site?

Het is moeilijk te voor het behoud van de exacte doorvoer de tunnel VPN. IPsec en SSTP zijn cryptografische veel VPN protocollen. Doorvoer wordt ook beperkt door de latentie en bandbreedte tussen uw lokale en Internet.

## <a name="gateways"></a>Gateways

### <a name="what-is-a-policy-based-static-routing-gateway"></a>Wat is een gateway-beleid (statische-omleiding)?

Beleid gebaseerde gateways implementeren beleid gebaseerde VPN's. Beleid gebaseerde VPN's versleutelen en pakketten via IPSec-tunnels op basis van de combinaties van adresvoorvoegsels tussen uw on-premises netwerk en het VNet Azure verwijzen. Het beleid (of verkeer Gegevenskiezer) wordt gewoonlijk gedefinieerd als een access-lijst in de configuratie van de VPN.

### <a name="what-is-a-route-based-dynamic-routing-gateway"></a>Wat is een gateway route gebaseerde (dynamic-omleiding)?

Route gebaseerde gateways Implementeer de route-VPN's. Route-VPN's 'routes' in de IP-doorsturen of omleiden tabel naar directe pakketten in hun bijbehorende tunnelinterfaces gebruiken De tunnelinterfaces vervolgens versleutelen of de pakketten en afmelden bij de tunnels ontsleutelen. Het beleid of verkeer Gegevenskiezer voor route op basis VPN's zijn geconfigureerd als de toepassing te (of de jokertekens).

### <a name="can-i-get-my-vpn-gateway-ip-address-before-i-create-it"></a>Kan ik mijn IP-adres van de VPN gateway krijgen voordat ik eraan?

Nee. U hoeft te maken van de gateway eerst om het IP-adres. Het IP-adres verandert als u verwijderen en opnieuw maken van de VPN-gateway.

### <a name="how-does-my-vpn-tunnel-get-authenticated"></a>Hoe mijn tunnel VPN krijgen geverifieerd?

Azure VPN wordt gebruikt voor verificatie van de vooraf gedeelde sleutel (vooraf-gedeelde sleutel). We een vooraf gedeelde sleutel (PSK) gegenereerd als we de VPN-tunnel maken. U kunt de automatisch gegenereerde vooraf gedeelde sleutel wijzigen in uw eigen met het instellen van vooraf toets PowerShell-cmdlet of REST API.

### <a name="can-i-use-the-set-pre-shared-key-api-to-configure-my-policy-based-static-routing-gateway-vpn"></a>Kan ik de API instellen vooraf-toets gebruiken voor het configureren van mijn-beleid (statische routering) gateway VPN?

Ja, kan de API voor vooraf toets instellen en PowerShell-cmdlet worden gebruikt voor het configureren van Azure-beleid (statische) VPN's zowel route-(dynamische) routeren VPN's.

### <a name="can-i-use-other-authentication-options"></a>Kan ik andere verificatieopties gebruiken?

We zijn beperkt tot met behulp van vooraf gedeelde sleutels (PSK) voor verificatie.

### <a name="what-is-the-gateway-subnet-and-why-is-it-needed"></a>Wat is het "gateway subnet" en waarom dit nodig is?

We hebben een gatewayservice die we uitvoeren om te cross-premises-connectiviteit inschakelen. 

U moet een gateway subnet maken voor uw VNet een VPN-gateway configureren. Alle gateway-subnetten moeten de naam GatewaySubnet werkt alleen naar behoren. Geen naam toewijzen aan uw subnet, gateway iets anders. En VMs of iets anders op het subnet gateway niet implementeren.

De gateway subnet minimale grootte is volledig afhankelijk van de configuratie die u wilt maken. Hoewel het is mogelijk te maken van een gateway-subnet kleinst /29 voor sommige configuraties, raden wij aan dat u een gateway-subnet van /28 of groter maken (/ 28, /27, /26 enz.). 

### <a name="can-i-deploy-virtual-machines-or-role-instances-to-my-gateway-subnet"></a>Kan ik virtuele Machines of rol exemplaren implementeren naar Mijn subnet gateway?

Nee.

### <a name="how-do-i-specify-which-traffic-goes-through-the-vpn-gateway"></a>Hoe geef ik waarop verkeer doorloopt de gateway VPN?

Als u de klassieke Azure-Portal gebruikt, kunt u elk bereik gewenste verzonden via de gateway voor uw virtuele netwerk op de pagina netwerken onder lokale netwerken toevoegen.

### <a name="can-i-configure-forced-tunneling"></a>Kan ik configureren gedwongen Tunneling?

Ja. Zie [configureren gedwongen tunneling](vpn-gateway-about-forced-tunneling.md).

### <a name="can-i-set-up-my-own-vpn-server-in-azure-and-use-it-to-connect-to-my-on-premises-network"></a>Kan ik mijn eigen VPN-server in Azure instellen en deze gebruiken om te verbinden met mijn on-premises netwerk?

Ja, kunt u uw eigen VPN gateways of servers in Azure wordt aangegeven van de Azure Marketplace of het maken van uw eigen routers VPN implementeren. U moet de gebruiker gedefinieerde routes in uw virtuele netwerk om ervoor te zorgen verkeer correct is doorgestuurd tussen uw on-premises implementatie-netwerken te gebruiken en uw virtuele subnetten configureren.

### <a name="why-are-certain-ports-opened-on-my-vpn-gateway"></a>Waarom worden bepaalde poorten geopend op mijn gateway VPN?

Ze zijn vereist voor Azure-infrastructuur communicatie. Ze zijn beveiligd (vergrendeld) door Azure certificaten. Zonder BEGINLETTERS certificaten kunnen externe entiteiten, inclusief de klanten van deze gateways, niet ertoe leiden dat geen invloed op deze eindpunten.

Een VPN-gateway is in feite een apparaat multi-homed met één NIC toegang te krijgen tot het customer network voor persoonlijke en één NIC die tegenover elkaar liggen in het openbare netwerk. Azure-infrastructuur entiteiten kunnen geen klant particuliere netwerken vanwege de naleving benutten zodat ze nodig hebben gebruikmaken van de openbare eindpunten voor infrastructuur communicatie. De openbare eindpunten worden periodiek gescand door Azure in te vullen.


### <a name="more-information-about-gateway-types-requirements-and-throughput"></a>Meer informatie over het typen van de gateway, vereisten en doorvoer

Zie voor meer informatie [Over VPN Gateway-instellingen](vpn-gateway-about-vpn-gateway-settings.md).

## <a name="multi-site-and-vnet-to-vnet-connectivity"></a>Meerdere locaties en VNet-naar-VNet-connectiviteit

### <a name="which-type-of-gateways-can-support-multi-site-and-vnet-to-vnet-connectivity"></a>Welk type gateways kunt ondersteuning voor meerdere locaties en VNet-naar-VNet connectivity?

Alleen op route gebaseerde VPN's (dynamische omleiding).

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>Kan ik een VNet verbinden met een VPN-Type RouteBased met een type PolicyBased VPN naar een andere VNet?

Nee, beide virtuele netwerken moeten gebruiken op basis van route (dynamische omleiding) VPN's.

### <a name="is-the-vnet-to-vnet-traffic-secure"></a>Is het verkeer VNet-naar-VNet veilig?

Ja, het is beveiligd met een IPsec/IKE-codering.

### <a name="does-vnet-to-vnet-traffic-travel-over-the-azure-backbone"></a>VNet-naar-VNet verkeer via de Azure backbone verzonden?

Ja.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Hoeveel lokale sites en virtuele netwerken kan een virtueel netwerk verbinding maken?

Max. 10 gecombineerd voor de Basic- en standaard dynamische routering gateways; 30 voor hoge prestaties VPN gateways.

### <a name="can-i-use-point-to-site-vpns-with-my-virtual-network-with-multiple-vpn-tunnels"></a>Kan ik gebruik punt-naar-Site VPN's met mijn virtuele netwerk met meerdere VPN tunnels?

Ja, kunnen het punt-naar-Site (P2S) VPN's worden gebruikt met gateways VPN-verbinding maakt met meerdere sites voor on-premises implementatie en andere virtuele netwerken.

### <a name="can-i-configure-multiple-tunnels-between-my-virtual-network-and-my-on-premises-site-using-multi-site-vpn"></a>Kan ik meerdere tunnels configureren tussen mijn virtuele netwerk en mijn on-premises implementatie-site via VPN meerdere site?

Nee, overtollige tunnels tussen een Azure virtuele netwerk en een on-premises site worden niet ondersteund.

### <a name="can-there-be-overlapping-address-spaces-among-the-connected-virtual-networks-and-on-premises-local-sites"></a>Kan er overlappende adres spaties tussen de verbonden virtuele netwerken en on-premises implementatie lokale sites?

Nee. Overlappende adres spaties, treedt het 'Virtuele netwerk maken' mislukt of netwerk configuratie bestand uploaden.

### <a name="do-i-get-more-bandwidth-with-more-site-to-site-vpns-than-for-a-single-virtual-network"></a>Vind ik meer bandbreedte met meer naar website VPN's dan voor één virtuele netwerk?

Nee, alle VPN tunnels punt-naar-Site VPN's, inclusief de dezelfde Azure VPN gateway en de beschikbare bandbreedte delen.

### <a name="can-i-use-azure-vpn-gateway-to-transit-traffic-between-my-on-premises-sites-or-to-another-virtual-network"></a>Kan ik via Azure VPN gateway verkeer douanevervoer tussen Mijn sites on-premises implementatie of naar een ander virtuele netwerk?

**Klassieke implementatiemodel**<br>
Overdracht verkeer via Azure VPN gateway is het mogelijk dat met het implementatiemodel klassieke, maar is afhankelijk van het statisch gedefinieerde adres spaties in het configuratiebestand netwerk. BGP is nog niet ondersteund met Azure virtuele netwerken en VPN gateways met het implementatiemodel klassieke. Zonder BGP is handmatig definiëren overdracht adres spaties zeer fout vatbaar en niet aanbevolen.<br>
**Resourcemanager implementatiemodel**<br>
Als u het implementatiemodel resourcemanager gebruikt, raadpleegt u de sectie [BGP](#bgp) voor meer informatie.

### <a name="does-azure-generate-the-same-ipsecike-pre-shared-key-for-all-my-vpn-connections-for-the-same-virtual-network"></a>Genereert Azure dezelfde IPsec/IKE vooraf gedeelde sleutel voor Mijn VPN-verbindingen voor hetzelfde virtuele netwerk?

Nee, genereert Azure al dan niet standaard verschillende vooraf-gedeelde sleutels voor verschillende VPN verbindingen. U kunt echter de instellen VPN Gateway-toets REST API of PowerShell-cmdlet voor het instellen van de gewenste sleutelwaarde. De sleutel moet alfanumerieke tekenreeks met lengte tussen 1 tot en met 128 tekens.

### <a name="does-azure-charge-for-traffic-between-virtual-networks"></a>Azure kosten in rekening voor verkeer tussen virtuele netwerken?

Voor verkeer tussen verschillende Azure virtuele netwerken kosten Azure alleen voor het verkeer doorlopen van de ene Azure regio naar de andere. Het tarief weer dat boete wordt vermeld op de pagina Azure [VPN Gateway prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway/) .


### <a name="can-i-connect-a-virtual-network-with-ipsec-vpns-to-my-expressroute-circuit"></a>Kan ik een virtueel netwerk verbinden met IPsec VPN's naar Mijn circuitlijnen ExpressRoute?

Ja, dit wordt ondersteund. Zie [ExpressRoute configureren en Site-naar-Site VPN verbindingen die naast elkaar gebruiken](../expressroute/expressroute-howto-coexist-classic.md)voor meer informatie.

## <a name="bgp"></a>BGP

[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 



## <a name="cross-premises-connectivity-and-vms"></a>Cross-premises-connectiviteit en VMs

### <a name="if-my-virtual-machine-is-in-a-virtual-network-and-i-have-a-cross-premises-connection-how-should-i-connect-to-the-vm"></a>Als mijn VM zich in een virtueel netwerk en ik heb een verbinding cross-premises, moet ik toegang krijgen tot de VM?

U hebt een paar opties. Als er RDP ingeschakeld en u een eindpunt hebt gemaakt, kunt u verbinding maken met uw virtuele machine met behulp van de VIP. In dat geval geeft u de VIP en de poort waarmee u verbinding wilt maken. U moet de poort configureren op uw VM voor het verkeer. U zou meestal, gaat u naar de klassieke Azure-Portal en de instellingen voor de RDP-verbinding met uw computer opslaan. De instellingen bevat de benodigde verbindingsinformatie.

Als u een virtueel netwerk met meerdere lokale connectivity geconfigureerd hebt, kunt u verbinding maken met uw VM via de interne DIP of persoonlijk IP-adres. U kunt ook verbinding maakt met uw VM door interne DIP vanaf een andere virtuele machine die in hetzelfde virtuele netwerk bevindt zich. U kunt RDP niet aan uw virtuele machine met behulp van de DIP als u verbinding van een locatie buiten het virtuele netwerk maakt. Als u een punt-naar-Site van een virtueel netwerk geconfigureerd hebt en u geen verbinding vanaf uw computer maken, kunt u geen bijvoorbeeld voor verbinding met de virtuele machine door DIP.

### <a name="if-my-virtual-machine-is-in-a-virtual-network-with-cross-premises-connectivity-does-all-the-traffic-from-my-vm-go-through-that-connection"></a>Als mijn VM zich in een virtueel netwerk met meerdere lokale connectivity, alle verkeer vanaf mijn VM doorlopen die verbinding?

Nee. Alleen het verkeer met een bestemming IP die deel uitmaakt van de virtuele netwerk lokale netwerk IP-adresbereiken die u hebt opgegeven gaat via de gateway virtueel netwerk. Verkeer heeft een bestemming IP bevinden binnen het virtuele netwerk binnen het virtuele netwerk blijft. Andere verkeer is verzonden via de taakverdeling naar openbare netwerken, of als afgedwongen tunneling wordt gebruikt, verzonden via de gateway Azure VPN. Als u oplossen wilt, is het belangrijk om ervoor te zorgen dat er alle bereik het lokale netwerk dat u wilt verzenden via de gateway. Controleer of dat de lokale netwerk adresbereiken elkaar niet met een van de adresbereiken in het virtuele netwerk overlappen. Ook wilt u controleren of dat de DNS-server die u gebruikt de naam wordt omgezet naar het juiste IP-adres.


## <a name="virtual-network-faq"></a>Virtual Network Veelgestelde vragen

U weergeven extra virtueel netwerkinformatie in de [Virtuele Veelgestelde vragen over het netwerk](../virtual-network/virtual-networks-faq.md).
 
