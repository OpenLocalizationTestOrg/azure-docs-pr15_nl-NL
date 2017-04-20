### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>Is BGP ondersteund op alle Azure VPN Gateway-SKU's?

Nee, BGP wordt ondersteund op Azure **standaard** - en **HighPerformance** VPN gateways. **Eenvoudige** SKU wordt niet ondersteund.

### <a name="can-i-use-bgp-with-azure-policy-based-vpn-gateways"></a>Kan ik BGP met Azure Policy-Based VPN gateways gebruiken?

Nee, BGP wordt ondersteund op alleen Route gebaseerde VPN gateways.

### <a name="can-i-use-private-asns-autonomous-system-numbers"></a>Kan ik privé ASN's (zelfstandige systeem getallen) gebruiken?

Ja, u kunt uw eigen openbare ASN's of privé ASN's gebruiken voor uw on-premises implementatie-netwerken te gebruiken en de Azure virtuele netwerken.

#### <a name="are-there-asns-reserved-by-azure"></a>Zijn er ASN's gereserveerd door Azure?

Ja, zijn de volgende ASN's gereserveerd door Azure voor interne en externe peerings:

- Openbare ASN's: 8075, 8076, 12076
- Privé ASN's: 65515, 65517, 65518, 65519, 65520

U kunt deze ASN's opgeven voor uw aan premises VPN apparaten wanneer u verbinding maakt met Azure VPN gateways.

### <a name="can-i-use-the-same-asn-for-both-on-premises-vpn-networks-and-azure-vnets"></a>Kan ik de dezelfde ASN voor zowel on-premises implementatie VPN netwerken en Azure VNets gebruiken?

Nee, moet u verschillende ASN's toewijzen tussen uw on-premises implementatie-netwerken te gebruiken en uw Azure-VNets als u deze verbinding samen met BGP. Azure VPN Gateways, wordt er een standaardwaarde ASN van 65515 die zijn toegewezen, hebben of BGP is ingeschakeld voor niet voor uw cross-premises connectivity. U kunt deze standaardinstellingen overschrijven door het toewijzen van een andere ASN bij het maken van de gateway VPN of de ASN wijzigen nadat de gateway is gemaakt. U moet uw on-premises implementatie ASN's toewijzen aan de bijbehorende Azure lokale netwerkgateways.

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-to-me"></a>Welke adresvoorvoegsels zal Azure VPN gateways gebruiken voor mij?

Azure VPN gateway zal de volgende routes naar uw on-premises implementatie BGP apparaten gebruiken:

- Uw VNet adres voorvoegsels voor eenheden
- Adresvoorvoegsels voor elke lokale netwerkgateways verbonden met de gateway Azure VPN
- Routes die van andere BGP peering sessies verbonden met de gateway Azure VPN, **behalve standaardroute of routes overlapt met een voorvoegsel VNet**.

#### <a name="can-i-advertise-default-route-00000-to-azure-vpn-gateways"></a>Kan ik standaardroute (0.0.0.0/0) naar Azure VPN gateways reclame maken?

Niet op dit moment.

#### <a name="can-i-advertise-the-exact-prefixes-as-my-virtual-network-prefixes"></a>Kan ik de exacte voorvoegsels gebruiken als mijn Virtual Network voorvoegsels voor eenheden?

Nee, de dezelfde voorvoegsels als een van uw Virtual Network reclame adres voorvoegsels voor eenheden wordt geblokkeerd of gefilterd door het Azure platform. U kunt echter een voorvoegsel dat is een uitbreiding van wat u hebt het virtuele netwerk weergeven. Bijvoorbeeld, uw virtuele netwerk kunt doen met het adres ruimte 10.10.0.0/16 en u 10.0.0.0/8 kan weergeven.

### <a name="can-i-use-bgp-with-my-vnet-to-vnet-connections"></a>Kan ik met Mijn verbindingen VNet-naar-VNet BGP gebruiken?

Ja, kunt u BGP voor zowel lokale meerdere verbindingen en VNet-naar-VNet verbindingen.

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>Kan ik BGP meng met niet-BGP verbindingen voor mijn gateways Azure VPN?

Ja, kunt u zowel BGP als niet-BGP verbindingen voor dezelfde Azure VPN gateway combineren.

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Betekent Azure VPN gateway ondersteuning BGP overdracht routering?

Ja, BGP overdracht routering wordt ondersteund, met uitzondering dat Azure VPN gateways is **niet** bekend standaardroutes naar andere BGP collega's. Als u wilt inschakelen overdracht routeren tussen meerdere Azure VPN gateways, moet u BGP inschakelen op alle tussenliggende VNet-naar-VNet verbindingen.

### <a name="can-i-have-more-than-one-tunnels-between-azure-vpn-gateway-and-my-on-premises-network"></a>Kan ik meer dan één tunnels tussen Azure VPN gateway en mijn on-premises netwerk hebben?

Ja, u kunt tot stand brengen meer dan één S2S VPN tunnels tussen een gateway Azure VPN en uw on-premises netwerk. Houd er rekening mee dat alle deze tunnels ten opzichte van het totale aantal tunnel voor uw Azure VPN gateways worden geteld. Hebt u twee overtollige tunnels tussen uw gateway Azure VPN en een van uw on-premises netwerk, worden ze bijvoorbeeld 2 tunnels afmelden bij het totale quotum voor de gateway Azure VPN (10 voor standaard) en 30 voor HighPerformance gebruiken.

### <a name="can-i-have-multiple-tunnels-between-two-azure-vnets-with-bgp"></a>Kan ik meerdere tunnels tussen twee Azure VNets met BGP hebben?

Nee, overtollige tunnels tussen twee virtuele netwerken worden niet ondersteund.

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-expressroutes2s-vpn-co-existence-configuration"></a>Kan ik BGP voor S2S VPN in een VPN-ExpressRoute/S2S naast elkaar bestaan configuratie gebruiken?

Niet op dit moment.

### <a name="what-address-does-azure-vpn-gateway-use-for-bgp-peer-ip"></a>Welk adres gebruikt Azure VPN gateway voor BGP Peer IP?

De gateway Azure VPN wordt één IP-adres van het bereik GatewaySubnet is gedefinieerd voor het virtuele netwerk toewijzen. Standaard is dit het tweede laatste adres van het bereik. Bijvoorbeeld, als uw GatewaySubnet 10.12.255.0/27 is, zijn die variëren van 10.12.255.0 naar 10.12.255.31, klikt u vervolgens het BGP Peer-IP-adres van de gateway Azure VPN 10.12.255.30. U vindt deze informatie wanneer u de gegevens van de gateway Azure VPN weer te geven.

### <a name="what-are-the-requirements-for-the-bgp-peer-ip-addresses-on-my-vpn-device"></a>Wat zijn de vereisten voor de BGP Peer-IP-adressen op mijn apparaat VPN?

Uw on-premises implementatie BGP adres van peer **Mogen niet** hetzelfde zijn als het openbare IP-adres van uw apparaat VPN. Een ander IP-adres op het apparaat VPN gebruiken voor uw BGP Peer-IP. Een adres dat is toegewezen aan de loopback-interface op het apparaat kunt werken. Hiermee geeft u dit adres in de bijbehorende lokale netwerkgateway voor de locatie.

### <a name="what-should-i-specify-as-my-address-prefixes-for-the-local-network-gateway-when-i-use-bgp"></a>Wat moet ik opgeven als mijn adresvoorvoegsels voor de lokale netwerkgateway wanneer ik BGP gebruiken?

Azure lokale netwerkgateway Hiermee geeft u de oorspronkelijke adresvoorvoegsels voor de on-premises netwerk. Met BGP, moet u het voorvoegsel host toewijzen (/ 32 voorvoegsel) van uw BGP Peer-IP-adres als de adresruimte voor die on-premises netwerk. Als uw BGP Peer-IP 10.52.255.254 is, moet u '10.52.255.254/32' als de localNetworkAddressSpace van de lokale netwerkgateway dat staat voor deze on-premises netwerk. Dit is om ervoor te zorgen dat de gateway Azure VPN tot stand de sessie BGP via de tunnel S2S VPN brengt.

### <a name="what-should-i-add-to-my-on-premises-vpn-device-for-the-bgp-peering-session"></a>Wat moet ik toevoegen aan mijn apparaat van de VPN on-premises implementatie van de BGP peering sessie?

U moet een hostroute van het Azure BGP Peer IP-adres op uw apparaat VPN is die wijst naar de IPsec S2S VPN-tunnel toevoegen. Als de Azure VPN Peer-IP '10.12.255.30' is, moet u de hostroute van een voor "10.12.255.30" met een interface nexthop van de overeenkomende IPsec tunnelinterface bijvoorbeeld op uw apparaat VPN toevoegen.
