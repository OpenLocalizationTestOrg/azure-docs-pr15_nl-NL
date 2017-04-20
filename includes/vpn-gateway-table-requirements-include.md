De volgende tabel bevat de vereisten voor PolicyBased en RouteBased VPN gateways. In deze tabel geldt voor de resourcemanager en de klassieke implementatiemodellen. Het model klassieke PolicyBased VPN gateways zijn hetzelfde als de statische gateways en gateways Route gebaseerde zijn hetzelfde als dynamische gateways.


|   | **PolicyBased Basic VPN Gateway** | **RouteBased Basic VPN Gateway** | **RouteBased standaard VPN Gateway**   | **RouteBased High Performance VPN Gateway** |
|---|---------------------------------------|---------------------------------------|----------------------------|----------------------------------|
|    **Site-naar-Site connectivity (S2S)**  | PolicyBased VPN configuratie        | RouteBased VPN configuratie  | RouteBased VPN configuratie     | RouteBased VPN configuratie    |
| **Punt-naar-Site connectivity (P2S**)      | Niet ondersteund   | Ondersteund (kunnen naast S2S)  | Ondersteund (kunnen naast S2S)  | Ondersteund (kunnen naast S2S) |
| **Verificatiemethode**                 |    Vooraf gedeelde sleutel  | Vooraf gedeelde sleutel voor S2S connectivity, certificaten voor P2S connectivity | Vooraf gedeelde sleutel voor S2S connectivity, certificaten voor P2S-connectiviteit | Vooraf gedeelde sleutel voor S2S connectivity, certificaten voor P2S-connectiviteit |
| **Maximum aantal S2S verbindingen**       | 1                              | 10                                                                    | 10                                | 30                               |
| **Maximum aantal P2S verbindingen**       | Niet ondersteund                  | 128                                                                   | 128                               | 128                              |
|**Ondersteuning voor actieve routering (BGP)**           | Niet ondersteund                  | Niet ondersteund                                                         | Ondersteund                     | Ondersteund                   |
 
