|    | **VPN Gateway doorvoer (1)** | **Max IPsec VPN Gateway tunnels (2)** | **ExpressRoute Gateway doorvoer** | **VPN Gateway en ExpressRoute worden geïnstalleerd**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **Eenvoudige SKU (3)(5)**              |  100 Mbps | 10                         |  500 Mbps                           | Nee   |
| **Standaard SKU (4)(5)**           |  100 Mbps | 10                         | 1000 Mbps                           | Ja  |
| **Krachtige SKU (4)**   | 200 Mbps  | 30                         | 2000 Mbps                           | Ja  |

- (1) de doorvoer VPN is een ruw raming op basis van de afmetingen tussen VNets in dezelfde Azure regio. Het is niet een gegarandeerd doorvoer voor cross-premises verbindingen via Internet. De meting maximumdoorvoer mogelijk is.
- (2) het nummer tunnel raadpleegt u RouteBased VPN's. Een VPN-PolicyBased kan slechts één naar website VPN tunnel ondersteunen.
- (3) BGP wordt niet ondersteund voor de eenvoudige SKU.
- (4) PolicyBased VPN's worden niet ondersteund voor de SKU. Deze worden ondersteund voor de eenvoudige SKU alleen.
- (5) actieve S2S VPN Gateway verbindingen worden niet ondersteund voor de SKU. Actieve wordt ondersteund in de HighPerformance SKU alleen.