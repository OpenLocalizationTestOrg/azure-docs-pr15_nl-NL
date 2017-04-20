Wanneer u een virtueel netwerkgateway maakt, moet u opgeven van de gateway SKU die u wilt gebruiken. Wanneer u een hogere gateway SKU selecteert, meer CPU's en netwerkbandbreedte worden toegewezen aan de gateway en daardoor de gateway sneller netwerk worden verwerkt bij het virtuele netwerk kunt ondersteunen.

VPN Gateway de beschikking over de volgende SKU's:

- Eenvoudige
- Standaard
- HighPerformance

Als u een SKU selecteert, kunt u het volgende overwegen:

- Als u een type PolicyBased VPN gebruiken wilt, moet u de eenvoudige SKU gebruiken. PolicyBased VPN's (voorheen statische routering) worden niet ondersteund op een andere SKU.
- BGP wordt niet ondersteund in de eenvoudige SKU.
- ExpressRoute-VPN Gateway naast configuraties worden niet ondersteund in de eenvoudige SKU.
- Actieve S2S VPN Gateway verbindingen kunnen worden geconfigureerd in de HighPerformance SKU alleen.
