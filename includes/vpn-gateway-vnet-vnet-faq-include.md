- De virtuele netwerken kunnen zijn in dezelfde of een andere Azure regio's (locaties).

- Een cloudservice of een eindpunt van taakverdeling kan geen bestrijkt virtuele netwerken, zelfs als ze met elkaar zijn verbonden.

- Meerdere Azure virtuele netwerken aan elkaar koppelen, geen vereist een on-premises implementatie VPN gateways tenzij cross-premises connectiviteit vereist is.

- VNet-naar-VNet ondersteunt verbindende virtuele netwerken. Dit ondersteunt verbindende virtuele machines of cloud services niet in een virtueel netwerk.

- VNet-naar-VNet vereist Azure VPN gateways met RouteBased (eerder genoemd dynamische routering) VPN typen. 

- Virtuele netwerkconnectiviteit tegelijk kan worden gebruikt met meerdere locaties VPN's, met een maximum van 10 (standaard/standaard Gateways) of 30 (hoge prestaties Gateways) VPN tunnels voor een virtueel netwerk VPN gateway verbinding maken met andere virtuele netwerken of on-premises-sites.

- De adresruimte van de virtuele netwerken en on-premises lokale netwerksites moeten niet overlappen. Overlappende adres spaties, zal het maken van verbindingen VNet-naar-VNet mislukt.

- Overtollige tunnels tussen twee virtuele netwerken worden niet ondersteund.

- Alle VPN tunnels van het virtuele netwerk delen de beschikbare bandbreedte op de gateway Azure VPN en de beschikbaarheid VPN op dezelfde gateway SLA in Azure wordt aangegeven.

- VNet-naar-VNet verkeer passeert af van het Microsoft-Network, niet Internet.

- VNet-naar-VNet verkeer binnen dezelfde regio is gratis voor beide richtingen; cross regio VNet-naar-VNet egress verkeer in rekening wordt gebracht met de uitgaande inter-VNet gegevens doorverbinden tarieven die op basis van de bron regio's. Raadpleeg de [prijzen van de pagina](https://azure.microsoft.com/pricing/details/vpn-gateway/) voor meer informatie.