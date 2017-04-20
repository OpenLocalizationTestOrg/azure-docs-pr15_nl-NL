<properties
   pageTitle="Uitvoering van een zeer beschikbaar hybride netwerkarchitectuur | Microsoft Azure"
   description="Het implementeren van een beveiligde site-naar-site netwerkarchitectuur die betrekking hebben op een Azure virtuele netwerk en een on-premises netwerk verbonden door middel van ExpressRoute met VPN gateway failover."
   services="guidance,virtual-network,vpn-gateway,expressroute"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-highly-available-hybrid-network-architecture"></a>Een netwerkarchitectuur ten zeerste beschikbaar hybride implementeren

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

In dit artikel wordt beschreven aanbevolen procedures voor het verbinden met een on-premises netwerk virtuele netwerken op Azure via ExpressRoute, met een site-naar-site VPN (VPN) als een failover-verbinding. De verkeersstromen tussen de on-premises netwerk en een Azure virtuele netwerk (VNet) tot en met een ExpressRoute-verbinding.  Als er een verlies van connectiviteit in de circuitlijnen ExpressRoute, wordt verkeer worden gerouteerd via een VPN-IPSec-tunnel.

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen: [Resourcemanager] [ resource-manager-overview] en klassieke. Deze blauwdruk gebruikt resourcemanager, waarin u wordt aangeraden voor nieuwe implementaties.

Veelvoorkomend gebruik dozen voor deze architectuur opnemen:

- Hybride toepassingen waarbij de werkbelasting tussen een on-premises netwerk en Azure zijn verdeeld.

- Applicaties grootschalige, essentiële werkbelasting waarvoor een hoge mate van schaalbaarheid.

- Grootschalige back-up en herstellen voorzieningen voor de gegevens die offline werkt moet worden opgeslagen.

- Afhandeling Big Data werkbelasting.

- Azure als een site herstel gebruiken.

Houd er rekening mee dat als de circuitlijnen ExpressRoute niet beschikbaar is, de route VPN slechts persoonlijke peering verbindingen verwerken wordt. Openbare peering en Microsoft peering verbindingen worden doorgegeven via Internet.

## <a name="architecture-diagram"></a>Architectuurdiagram

>[AZURE.NOTE] De [Azure VPN Gateway-service] [ azure-vpn-gateway] implementeert twee soorten virtueel netwerkgateways; VPN virtueel netwerkgateways en ExpressRoute virtueel netwerkgateways. In dit document, zijn de term *VPN Gateway* verwijst naar de Azure-service, terwijl de zinnen *VPN virtueel netwerkgateway* en *ExpressRoute virtueel netwerkgateway* gebruikt om te verwijzen naar de VPN en ExpressRoute implementaties van de gateway respectievelijk.

In het volgende diagram wordt de belangrijke onderdelen in deze architectuur gemarkeerd:

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft Downloadcentrum][visio-download]. Dit diagram is op het 'hybride netwerk - ER VPN' pagina.

![[0]][0]

- **Azure virtuele netwerken (VNets).** Elke VNet bevindt zich in een enkel Azure regio en meerdere lagen kunt hosten. Lagen kunnen worden gesegmenteerd subnetten gebruiken in elke VNet en/of netwerk beveiligingsgroepen (NSGs).

- **On-premises implementatie-bedrijfsnetwerk.** Dit is een netwerk van computers en apparaten, verbonden via een privé LAN-netwerk uitgevoerd binnen een organisatie.

- **VPN toestel.** Dit is een apparaat of service waarmee met een externe connectiviteit met de on-premises netwerk. Het toestel VPN mogelijk een apparaat kan, of dit een softwareoplossing zoals de Routering en Remote Access Service (RRAS) in Windows Server 2012.

> [AZURE.NOTE] Voor een lijst met ondersteunde VPN-apparaten en informatie over het configureren van de geselecteerde VPN-apparaten om verbinding te maken met een VPN-Gateway Azure, raadpleegt u de instructies voor het gewenste apparaat in de [lijst met VPN apparaten worden ondersteund door Azure][vpn-appliance].

- **VPN virtueel netwerkgateway.** De gateway van VPN virtueel netwerk kunt de VNet verbinding maken met het toestel VPN in de on-premises netwerk. De VPN virtueel netwerkgateway is geconfigureerd voor het aanvragen van de on-premises netwerk alleen via het toestel VPN accepteren. Zie voor meer informatie [verbinding maken met een on-premises netwerk met een Microsoft Azure virtual netwerk][connect-to-an-Azure-vnet].

- **ExpressRoute virtueel netwerkgateway.** De gateway van ExpressRoute virtueel netwerk kunt de VNet verbinding maken met de ExpressRoute circuitlijnen gebruikt voor verbinding met uw on-premises netwerk.

- **Gateway-subnet.** Virtuele netwerkgateways worden gehouden in hetzelfde subnet.

- **VPN-verbinding.** De verbinding heeft eigenschappen die het verbindingstype (IPSec) en de toets met het on-premises implementatie VPN toestel gedeeld te versleutelen verkeer.

- **ExpressRoute circuitlijnen.** Dit is een laag 2 of layer 3 circuitlijnen verstrekt door de provider connectivity dat joins het lokale netwerk met Azure via de routers rand. De circuitlijnen maakt gebruik van de hardware-infrastructuur beheerd door de provider connectivity.

- **N lagen cloud-toepassing.** Dit is de toepassing die wordt gehost in Azure wordt aangegeven. Deze kan meerdere niveaus, met meerdere subnetten die zijn verbonden via Azure netwerktaakverdelers bevatten. Het verkeer in elk subnet kan er kosten regels die zijn gedefinieerd door middel van [Azure netwerk beveiligingsgroepen]worden[azure-network-security-group](NSGs). Zie voor meer informatie [aan de slag met Microsoft Azure-beveiliging][getting-started-with-azure-security].

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en resourcetypen, zodat deze architectuur verwijzing kan worden deze is ingericht veel verschillende manieren. We hebben een resourcemanager Azure-sjabloon om te kunnen installeren van de architectuur van de verwijzing die deze aanbevelingen volgt opgegeven. Als u wilt maken van de architectuur van uw eigen verwijzing moet u deze aanbevelingen volgt, tenzij u specifieke vereisten die een aanbeveling niet past hebt.

### <a name="vnet-and-gatewaysubnet"></a>VNet en GatewaySubnet

ExpressRoute virtueel netwerkgateway en de VPN virtueel netwerkgateway maken in de dezelfde VNet. Dit betekent dat ze hetzelfde subnet **GatewaySubnet** met de naam moeten delen

Als de VNet een benoemde **GatewaySubnet**subnet bevat, zorg ervoor dat er een /27 of grotere adresruimte. Als het bestaande subnet te klein is, verwijdert u deze als volgt en een nieuw account te maken, zoals wordt weergegeven in het volgende opsommingsteken:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Remove-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet
```

Als de VNet bevat geen een benoemde **GatewaySubnet**subnet, maakt u een nieuwe record als volgt:

```powershell
$vnet = Get-AzureRmVirtualNetworkGateway -Name <yourvnetname> -ResourceGroupName <yourresourcegroup>
Add-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.200.255.224/27"
$vnet = Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="vpn-and-expressroute-gateways"></a>VPN en ExpressRoute gateways

Controleer of uw organisatie voldoet aan de [vereisten van ExpressRoute] [ expressroute-prereq] voor de verbinding met Azure.

Als u al een VPN-virtueel netwerkgateway in uw VNet Azure, verwijderen, zoals hieronder wordt weergegeven.

```powershell
Remove-AzureRmVirtualNetworkGateway -Name <yourgatewayname> -ResourceGroupName <yourresourcegroup>
```

Volg de instructies in de [uitvoering van een hybride netwerkarchitectuur met Azure ExpressRoute] [ implementing-expressroute] uw ExpressRoute-verbinding tot stand brengen.

Volg de instructies in de [uitvoering van een hybride netwerkarchitectuur met Azure en On-premises VPN] [ implementing-vpn] tot stand brengen van de VPN virtuele gateway netwerkverbinding.

Nadat u de virtuele netwerkverbindingen voor de gateway hebt ingesteld, test u de omgeving als volgt te werk:

1. Zorg ervoor dat u vanaf uw on-premises netwerk kunt verbinden met uw VNet Azure.

2. Neem contact op met uw provider als u wilt stoppen connectivity ExpressRoute voor het testen.

3. Controleer of de kunt u nog steeds koppelen vanuit uw on-premises netwerk naar uw Azure VNet gebruik van de VPN virtuele gateway netwerkverbinding.

4. Neem contact op met uw provider naar reestabilish ExpressRoute connectivity.

## <a name="considerations"></a>Overwegingen

Zie de [uitvoering van een hybride netwerkarchitectuur met Azure ExpressRoute] voor ExpressRoute overwegingen,[ guidance-expressroute] richtlijnen.

Zie de [uitvoering van een hybride netwerkarchitectuur met Azure en On-premises VPN] voor Site-naar-Site VPN overwegingen,[ guidance-vpn] richtlijnen.

Zie [Microsoft cloudservices en beveiliging van het netwerk]voor algemene Azure beveiligingsoverwegingen,[best-practices-security].

## <a name="solution-deployment"></a>Implementatie van oplossingen

Als u een bestaande on-premises implementatie-infrastructuur reeds geconfigureerd met een toestel geschikt netwerk hebt, kunt u de architectuur verwijzing kunt implementeren door deze stappen uit:

1. Klik met de rechtermuisknop op de knop onderstaande en selecteer een van beide 'koppeling openen in nieuw tabblad"of 'Koppeling openen in nieuw venster':  
[![Implementeren naar Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy.json)

2. Wachten op een koppeling moet worden geopend in de portal van Azure en voert u de volgende stappen uit: 
  - De naam van de **resourcegroep** is al gedefinieerd in het parameterbestand, dus **Nieuw** te selecteren en voer `ra-hybrid-vpn-er-rg` in het tekstvak.
  - Selecteer de regio in de vervolgkeuzelijst **locatie** .
  - Bewerk de **Sjabloon hoofdsite Uri** of de **Parameter hoofdsite Uri** tekstvakken niet.
  - Controleer de bepalingen en voorwaarden en klik op het selectievakje **dat ik ga akkoord met de bovenstaande voorwaarden** .
  - Klik op de knop **aanschaffen** .

3. Wacht totdat de implementatie om te voltooien.

4. Klik met de rechtermuisknop op de knop onderstaande en selecteer een van beide 'koppeling openen in nieuw tabblad"of 'Koppeling openen in nieuw venster':  
[![Implementeren naar Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn-er%2Fazuredeploy-expressRouteCircuit.json)

5. Wacht totdat de koppeling naar openen in de portal Azure, voert u vervolgens als volgt te werk: 
  - Selecteer **bestaande gebruiken** in de sectie **resourcegroep** en voer `ra-hybrid-vpn-er-rg` in het tekstvak.
  - Selecteer de regio in de vervolgkeuzelijst **locatie** .
  - Bewerk de **Sjabloon hoofdsite Uri** of de **Parameter hoofdsite Uri** tekstvakken niet.
  - Controleer de bepalingen en voorwaarden en klik op het selectievakje **dat ik ga akkoord met de bovenstaande voorwaarden** .
  - Klik op de knop **aanschaffen** .

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[azure-network-security-group]: ../virtual-network/virtual-networks-nsg.md
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[expressroute-prereq]: ../expressroute/expressroute-prerequisites.md
[implementing-expressroute]: ./guidance-hybrid-network-expressroute.md#implementing-this-architecture
[implementing-vpn]: ./guidance-hybrid-network-vpn.md#implementing-this-architecture
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn]: ./guidance-hybrid-network-vpn.md
[best-practices-security]: ../best-practices-network-security.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-vpn-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-vpn.parameters.json
[virtualnetworkgateway-expressroute-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/virtualNetworkGateway-expressRoute.parameters.json
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[naming conventions]: ./guidance-naming-conventions.md
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/hybrid-network-expressroute-vpn-failover.png "Architectuur van een zeer beschikbaar hybride netwerkarchitectuur ExpressRoute en VPN gateway gebruiken"
[ARM-Templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
