<properties
   pageTitle="Uitvoering van een hybride netwerkarchitectuur met Azure ExpressRoute | Overzicht van de architectuur | Microsoft Azure"
   description="Het implementeren van een beveiligde site-naar-site netwerkarchitectuur die betrekking hebben op een Azure virtuele netwerk en een on-premises netwerk verbonden door middel van Azure ExpressRoute."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-expressroute"></a>Een hybride netwerkarchitectuur met Azure ExpressRoute implementeren

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

In dit artikel wordt beschreven aanbevolen procedures voor het verbinden met een on-premises netwerk virtuele netwerken op Azure met behulp van ExpressRoute. ExpressRoute verbindingen worden gemaakt met behulp van een privé speciale verbinding via een derde partij connectivity-provider. De particuliere verbinding breidt de mogelijkheden van uw on-premises netwerk in Azure toegang bieden tot de infrastructuur van uw eigen IaaS in Azure, openbare eindpunten in PaaS-services en Office365 SaaS services gebruikt. In dit document bevat informatie over het gebruik van ExpressRoute verbinding maken met één Azure virtuele netwerk (VNet) met zogenaamd privé peering.

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen: [Resourcemanager] [ resource-manager-overview] en klassieke. Deze blauwdruk gebruikt resourcemanager, waarin u wordt aangeraden voor nieuwe implementaties.

Veelvoorkomend gebruik dozen voor deze architectuur opnemen:

- Hybride toepassingen waarbij de werkbelasting tussen een on-premises netwerk en Azure zijn verdeeld.

- Applicaties grootschalige, essentiële werkbelasting waarvoor een hoge mate van schaalbaarheid.

- Grootschalige back-up en herstellen voorzieningen voor de gegevens die offline werkt moet worden opgeslagen.

- Afhandeling Big Data werkbelasting.

- Azure als een site herstel gebruiken.

> [AZURE.NOTE] De [ExpressRoute technisch overzicht] [ expressroute-technical-overview] bevat een inleiding tot ExpressRoute.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram wordt de belangrijke onderdelen in deze architectuur gemarkeerd:

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft Downloadcentrum][visio-download]. Dit diagram is op de pagina 'Hybride netwerk - ER'.

![[0]][0]

- **Azure virtuele netwerken (VNets).** Elke VNet bevindt zich in een enkel Azure regio en meerdere lagen kunt hosten. Lagen kunnen worden gesegmenteerd subnetten gebruiken in elke VNet en/of netwerk beveiligingsgroepen (NSGs). 

- **Azure openbare services.** Hierna ziet u Azure services die kunnen worden gebruikt in een hybride-toepassing. Deze services zijn ook beschikbaar via de openbare Internet, maar ze toegang kunt krijgen via een circuitlijnen ExpressRoute biedt lage latentie en bieden prestaties, aangezien verkeer niet verder via Internet. Verbindingen worden uitgevoerd met behulp van **openbare peering**, met de adressen die worden eigendom van uw organisatie of door de provider van de connectivity gegeven. 

- **Office 365-services.** Dit zijn de openbare Office 365-toepassingen en services van Microsoft. Verbindingen worden uitgevoerd via **Microsoft peering**, met de adressen die worden eigendom van uw organisatie of door de provider van de connectivity gegeven.

>[AZURE.NOTE] U kunt ook rechtstreeks met Microsoft CRM Online via een Microsoft-peering verbinden.

- **On-premises implementatie-bedrijfsnetwerk.** Dit is een netwerk van computers en apparaten, verbonden via een privé LAN-netwerk uitgevoerd binnen een organisatie.

- **Lokale rand routers.** Dit zijn de on-premises netwerk verbinden met de circuitlijnen beheerd door de provider. Afhankelijk van hoe de verbinding wordt ingericht, moet u het openbare IP-adressen die worden gebruikt door de routers. 

- **Microsoft edge routers.** Hierna ziet u twee routers in een actieve ten zeerste beschikbaar configuratie. Deze routers inschakelen een provider connectivity hun circuits rechtstreeks verbinden met hun datacenter. Afhankelijk van hoe de verbinding wordt ingericht, moet u het openbare IP-adressen die worden gebruikt door de routers.

- **ExpressRoute circuitlijnen.** Dit is een laag 2 of layer 3 circuitlijnen verstrekt door de provider connectivity dat joins het lokale netwerk met Azure via de routers rand. De circuitlijnen maakt gebruik van de hardware-infrastructuur beheerd door de provider connectivity.

- **Connectiviteit providers.** Hierna ziet u een bedrijven die een verbinding opgeven dat een van beide gebruiken laag 2 of layer 3-connectiviteit tussen uw datacenter en een Azure datacenter.

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en resourcetypen, zodat deze architectuur verwijzing kan worden deze is ingericht veel verschillende manieren. We hebben een resourcemanager Azure-sjabloon om te kunnen installeren van de architectuur van de verwijzing die deze aanbevelingen volgt opgegeven. Als u wilt maken van de architectuur van uw eigen verwijzing moet u deze aanbevelingen volgt, tenzij u specifieke vereisten die een aanbeveling niet past hebt.

### <a name="connectivity-providers"></a>Connectiviteit providers

Selecteer een geschikte ExpressRoute connectivity-provider voor uw locatie. Als u een lijst met connectivity-providers die beschikbaar zijn op uw locatie, gebruikt u de volgende Azure PowerShell-opdracht:

```powershell
Get-AzureRmExpressRouteServiceProvider
```

ExpressRoute connectivity providers verbinden met uw datacenter Microsoft in de volgende manieren:

- **Samen aan een cloud-exchange zich bevindt.** Als u samen op een locatie met een exchange cloud bevindt zich, kunt u virtuele cross-verbindingen met het Microsoft cloud tot en met de collega locatie-provider Ethernet exchange kunt ordenen. Collega locatie providers kunnen bieden op laag 2 cross-verbindingen of beheerde Layer 3 cross-verbindingen tussen de infrastructuur van uw collega locatie functie en de Microsoft cloud...

- **Point-to-Point Ethernet-verbindingen.** U kunt uw on-premises implementatie datacenters/kantoren koppelen aan de Microsoft cloud via point-to-point Ethernet koppelingen. Point-to-Point Ethernet providers Layer 2-verbindingen kunnen bieden of beheerde Layer 3-verbindingen tussen uw site en de Microsoft-cloud.

- **De toepassing te (IPVPN)-netwerken.** U kunt uw WAN integreren met de Microsoft-cloud. IPVPN providers (meestal MPLS VPN) bieden de toepassing te connectiviteit tussen uw filialen en datacenters. De Microsoft cloud kunt onderling verbonden aan uw WAN om het uiterlijk naar net als andere kantoor. WAN-providers bieden meestal beheerde Layer 3 connectivity.

Zie voor meer informatie over connectivity providers, [ExpressRoute circuits en routeren domeinen][connectivity-providers].

### <a name="expressroute-circuit"></a>ExpressRoute circuitlijnen

Zorg ervoor dat uw organisatie voldoet aan de [vereisten van ExpressRoute] [ expressroute-prereqs] voor de verbinding met Azure.

Als u dit nog niet hebt gedaan, voegt u een benoemde subnet `GatewaySubnet` naar uw VNet Azure en een ExpressRoute virtueel netwerkgateway met de Azure VPN Gateway-service te maken. Zie voor meer informatie over dit proces, [ExpressRoute werkstromen voor de inrichting van circuitlijnen en circuitlijnen Staten][ExpressRoute-provisioning].

Hiermee maakt u een circuitlijnen ExpressRoute als volgt:

1. Voer de volgende PowerShell-opdracht:

    ```powershell
    New-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>> -Location <<location>> -SkuTier <<sku-tier>> -SkuFamily <<sku-family>> -ServiceProviderName <<service-provider-name>> -PeeringLocation <<peering-location>> -BandwidthInMbps <<bandwidth-in-mbps>>
    ```

2. Verzend de `ServiceKey` voor de nieuwe circuitlijnen de service-provider.

3. Wacht totdat de provider voor het inrichten van de circuitlijnen. U kunt de inrichten status van een circuitlijnen controleren met behulp van de volgende PowerShell-opdracht:

    ```powershell
    Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
    ```

De `Provisioning state` veld in de `Service Provider` gedeelte van de uitvoer wordt gewijzigd van `NotProvisioned` naar `Provisioned` wanneer de circuitlijnen kan worden.

>[AZURE.NOTE]Als u een layer 3-verbinding gebruikt, te de provider configureren en beheren omleiding voor u; u opgeeft de gegevens die nodig zijn om in te schakelen van de provider voor de uitvoering van de juiste routes.

Als u een 2-laagverbinding gebruikt, volgt u de volgende stappen uit:

1. Twee reserveren/30 subnetten op basis van een geldige openbare IP-adressen voor elk type van peering die u wilt implementeren. Deze/30 subnetten wordt gebruikt voor het bieden van IP-adressen voor de routers gebruikt voor de circuitlijnen. Als u privé, openbare en Microsoft peering implementeert, moet u 6 /30 subnetten met geldige openbare IP-adressen.     

2. Configureer omleiding voor de circuitlijnen ExpressRoute. Voor het uitvoeren van de opdracht onder voor elk type van peering die u wilt configureren (privé, openbare en Microsoft).

    >[AZURE.NOTE]Zie [maken en wijzigen omleiding voor een circuitlijnen ExpressRoute] [ configure-expressroute-routing] voor meer informatie. De volgende PowerShell-opdrachten gebruiken om toe te voegen een netwerk peering voor het routeren van verkeer:

    ```powershell
    Set-AzureRmExpressRouteCircuitPeeringConfig -Name <<peering-name>> -Circuit <<circuit-name>> -PeeringType <<peering-type>> -PeerASN <<peer-asn>> -PrimaryPeerAddressPrefix <<primary-peer-address-prefix>> -SecondaryPeerAddressPrefix <<secondary-peer-address-prefix>> -VlanId <<vlan-id>>

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit <<circuit-name>>
    ```

3. Reserveer een andere groep geldige openbare IP-adressen wilt gebruiken voor NAT voor openbare en Microsoft peering. Het wordt aanbevolen dat een andere groep voor elke peering. Geef de groep met uw provider connectivity, zodat ze BGP advertenties voor die bereiken kunnen configureren.

[Uw persoonlijke VNet(s) in de cloud koppelen aan de circuitlijnen ExpressRoute][link-vnet-to-expressroute]. De volgende PowerShell-opdrachten gebruiken:

```
$circuit = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$gw = Get-AzureRmVirtualNetworkGateway -Name <<gateway-name>> -ResourceGroupName <<resource-group>>
New-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> -ResourceGroupName <<resource-group>> -Location <<location> -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute
```

Houd rekening met de volgende punten:

- ExpressRoute gebruikt de rand Gateway Protocol (BGP) voor de uitwisseling van informatie over de routering tussen uw netwerk en Azure.

- U kunt meerdere VNets zich in verschillende regio's naar de dezelfde ExpressRoute circuitlijnen zo lang maken als alle VNets en de circuitlijnen ExpressRoute zich bevinden binnen dezelfde geopolitieke regio koppelen.

## <a name="scalability-considerations"></a>Aandachtspunten voor de schaalbaarheid

ExpressRoute circuits bieden een hoge bandbreedte pad tussen netwerken te gebruiken. In het algemeen wordt hoe hoger de bandbreedte hoe hoger de kosten. Hoewel bij sommige providers u uw bandbreedte wijzigen kunt, zorg er dan voor dat u een eerste bandbreedte dat uw behoeften wordt overschreden en biedt ruimte voor groei kiezen. Als u opnieuw vergroten bandbreedte in de toekomst wilt, wordt u links met twee opties.

Vergroot de bandbreedte. Houd er rekening mee dat niet alle providers kunt u daarvoor dynamisch. En u moet voorkomen dat dit zo veel mogelijk doen. Maar indien nodig, nadat u hebt gecontroleerd bij uw provider, voert u de onderstaande opdrachten.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
$ckt.ServiceProviderProperties.BandwidthInMbps = <<bandwidth-in-mbps>>
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

U kunt de bandbreedte zonder kwaliteitsverlies connectivity verhogen. De bandbreedte Downgrade treden verstoringen in connectivity. U moet de circuitlijnen verwijderen en opnieuw te maken met de nieuwe configuratie.

Als u de standaard SKU gebruikt voor ExpressRoute en dat u een upgrade uitvoeren naar Premium of ander u uw prijzen bent plannen (datalimiet of onbeperkt), voert u de onderstaande opdrachten. De `Sku.Tier` eigenschap kan worden `Standard` of `Premium`; de `Sku.Name` eigenschap kan worden `MeteredData` of `UnlimitedData`.

```powershell
$ckt = Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>

$ckt.Sku.Tier = "Premium"
$ckt.Sku.Family = "MeteredData"
$ckt.Sku.Name = "Premium_MeteredData"

 Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
```

>[AZURE.IMPORTANT] Zorg ervoor dat de `Sku.Name` eigenschap overeenkomsten de `Sku.Tier` en `Sku.Family`. Als u de familie en laag, maar niet de naam verandert, kunt u de verbinding wordt uitgeschakeld.

U kunt de SKU zonder verstoringen bijwerken, maar u niet overschakelen van het abonnement dat onbeperkte prijzen voor netwerken met een datalimiet. Wanneer de SKU Downgrade, moet uw bandbreedtegebruik binnen de standaardbeperking van de standaard SKU blijven.

> [AZURE.NOTE] ExpressRoute biedt twee prijzen abonnementen klanten, op basis van meting of onbeperkt gegevens. Zie [ExpressRoute prijzen] [ expressroute-pricing] voor meer informatie. Kosten variëren op basis van de circuitlijnen bandbreedte. Beschikbare bandbreedte varieert waarschijnlijk provider provider. Gebruik de `Get-AzureRmExpressRouteServiceProvider` cmdlet om de providers die beschikbaar zijn in uw regio en de bandbreedte die ze aanbieden weer te geven.

Een enkel ExpressRoute circuitlijnen kan een aantal peerings en VNet koppelingen ondersteunen. Zie [beperkingen ExpressRoute] [ expressroute-limits] voor meer informatie.

Voor een extra kosten bevat ExpressRoute Premium invoegtoepassing:

- Maximaal 10.000 routes per circuitlijnen. Zonder ExpressRoute Premium invoegtoepassing is de limiet van 4000 routes per circuitlijnen.

- Globale connectiviteit inschakelen van een ExpressRoute circuitlijnen bevindt ergens in de wereld voor toegang tot bronnen in elke regio in plaats van alleen gebieden in het dezelfde continent.

- Een toename in het aantal VNet koppelingen per circuitlijnen van 10 voor een grotere, afhankelijk van de bandbreedte van de circuitlijnen.

ExpressRoute circuits zijn ontworpen om tijdelijke netwerk bursts maximaal twee keer de Bandbreedtelimiet die u voor gratis hebt aangeschaft. Dit is bereikt met behulp van overtollige koppelingen. Echter ondersteuning niet alle connectivity providers voor deze functie. Controleer of dat uw provider connectivity schakelt u deze functie voordat afhankelijk van het.

## <a name="availability-considerations"></a>Aandachtspunten voor de beschikbaarheid

U kunt de beschikbaarheid voor uw Azure verbinding configureren op verschillende manieren, afhankelijk van het type provider die u gebruikt en het aantal ExpressRoute circuits en virtuele gateway netwerkverbindingen die u bereid bent te configureren. De volgende punten samenvatten beschikbaarheidopties:

- ExpressRoute biedt geen ondersteuning voor router redundantie protocollen zoals HSRP en VRRP willen implementeren beschikbaarheid. Wordt gebruikt in plaats daarvan een overtollige paar BGP-sessies per peering. Om te vergemakkelijken uiterst beschikbaar verbindingen met uw netwerk, bepalingen Microsoft Azure u met twee overtollige poorten op twee routers (onderdeel van de Microsoft edge) in een actieve configuratie.

- Als u een 2-laagverbinding gebruikt, implementeren redundante routers in uw on-premises netwerk in een actieve configuratie. De primaire circuitlijnen verbinden met één router en de secundaire circuitlijnen naar de andere. Hiermee krijgt u een hoge beschikbaarheid verbinding aan beide uiteinden van de verbinding. Dit is nodig als u de SLA ExpressRoute vereist. Zie [SLA voor Azure ExpressRoute] [ sla-for-expressroute] voor meer informatie.

Het volgende diagram ziet u een configuratie met een lokale overtollige router verbonden met de primaire en secundaire circuits. Elke circuitlijnen omgaat met het verkeer naar een openbare peering en een privé peering (een paar /30 elke peering is opgegeven adres spaties, zoals beschreven in de vorige sectie).

![[1]][1]

Als u een layer 3-verbinding gebruikt, controleert u of hieraan overtollige BGP sessies die beschikbaarheid voor u verwerken.

Virtuele netwerken kunnen niet worden verbonden met meerdere ExpressRoute circuits en elke circuits kunnen worden geleverd door verschillende serviceproviders. Deze strategie biedt aanvullende hoge beschikbaarheid en noodgevallen herstelmogelijkheden.

Een VPN-verbinding voor de Site-naar-Site configureren als een pad failover voor ExpressRoute. Dit geldt alleen voor persoonlijke peering. Internet is voor Azure en Office 365-services, het pad alleen failover.

Standaard gebruik BGP sessies de waarde van een inactiviteit van 60 seconden. Als u een sessie hebt time-out 3 keer, de route is gemarkeerd als niet beschikbaar en al het verkeer omgeleid naar de resterende router. Het is mogelijk dat deze time-out 180 seconden te lang is voor kritieke toepassingen. In dit geval is, kunt u uw BGP time-outinstellingen op de router on-premises implementatie een kleinere waarde.

## <a name="manageability-considerations"></a>Aandachtspunten voor de beheerbaarheid

U kunt de [Azure Connectivity Toolkit (AzureCT)] [ azurect] om de connectiviteit tussen uw on-premises implementatie datacenter en Azure te houden.

## <a name="security-considerations"></a>Veiligheidsoverwegingen

U kunt beveiligingsopties configureren voor uw Azure verbinding op verschillende manieren, afhankelijk van uw beveiliging en naleving behoeften. De volgende punten samenvatting maken van uw beveiligingsopties.

ExpressRoute werkt in layer 3. Threats in de laag toepassing kunnen worden voorkomen met behulp van een netwerk beveiliging toestel waarop verkeer wordt beperkt tot legitieme resources. ExpressRoute verbindingen met openbare peering kunnen Daarnaast kunnen alleen worden geïnitieerd vanaf on-premises implementatie. Hiermee voorkomt u dat een service rogue toegang krijgen tot en verlies van on-premises gegevens van de openbare Internet.

Voeg beveiliging netwerkapparaten tussen de on-premises netwerk en de provider rand routers voor optimale beveiliging. Hierdoor kunt u de oplossingen van ongeoorloofde verhinderen dat de VNet:

![[2]][2]

Voor controle of naleving doeleinden, kan het nodig is om te voorkomen dat directe toegang onderdelen die in de VNet worden uitgevoerd met Internet en implementeren [gedwongen tunneling]zijn[forced-tuneling]. In dit geval wordt internetverkeer omgeleid achterwaarts door een proxy met on-premises implementatie waar het kan worden gecontroleerd. De proxy kan worden geconfigureerd als u wilt blokkeren ongeoorloofde die doorloopt af en mogelijk schadelijke binnenkomende verkeer filteren.

![[3]][3]

Standaard worden Azure VMs eindpunten gebruikt voor de aanmeldingstoegang voor beheer - RDP en externe Powershell voor Windows VMs en SSH voor Linux gebaseerde VMs wanneer geïmplementeerd via de portal van Azure. Voor optimale beveiliging niet inschakelen van een openbare IP-adres voor uw VMs en NSGs gebruiken om ervoor te zorgen dat deze VMs zijn niet toegankelijk voor het publiek. VMs moeten alleen beschikbaar via het interne IP-adres zijn. Deze adressen kunnen toegankelijk is via het netwerk ExpressRoute, die aan de on-premises implementatie DevOps personeelsleden om uit te voeren van alle benodigde configuratie en onderhoud worden aangebracht.

Als u moet de eindpunten management voor VMs met een extern netwerk, gebruikt u NSGs en/of toegangsbeheerlijsten de zichtbaarheid van deze poorten beperken tot een "witte" lijst IP-adressen of netwerken.

## <a name="troubleshooting-considerations"></a>Informatie over probleemoplossing

Als een eerder functioneel ExpressRoute circuitlijnen nu geen verbinding kan maken, bij afwezigheid van eventuele configuratie wijzigingen on-premises implementatie of binnen uw privé VNet, moet u mogelijk contact opnemen met de connectivity-provider en ermee werken om het probleem te verhelpen. U kunt ook de volgende Azure PowerShell-opdrachten gebruiken om te worden gecontroleerd sommige beperkte en vast te stellen waar de problemen mogelijk liggen:

- Controleer of de circuitlijnen is ingericht:

```powershell
Get-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

De uitvoer van deze opdracht ziet u verschillende eigenschappen voor uw circuitlijnen, inclusief `ProvisioningState`, `CircuitProvisioningState`, en `ServiceProviderProvisioningState` zoals hieronder wordt weergegeven.

```
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : NotProvisioned
```

Als de `ProvisioningState` niet is ingesteld op `Succeeded` nadat u probeert te maken van een nieuwe circuitlijnen, de circuitlijnen verwijderen met behulp van de volgende opdracht en probeer het opnieuw maken.

```powershell
Remove-AzureRmExpressRouteCircuit -Name <<circuit-name>> -ResourceGroupName <<resource-group>>
```

Als uw provider, hebt u er al deze is ingericht de circuitlijnen, en de `ProvisioningState` is ingesteld op `Failed`, of de `CircuitProvisioningState` is niet `Enabled`, neem contact op met uw provider voor hulp.

## <a name="solution-deployment"></a>Implementatie van oplossingen

Als u een bestaande on-premises implementatie-infrastructuur reeds geconfigureerd met een toestel geschikt netwerk hebt, kunt u de architectuur verwijzing kunt implementeren door deze stappen uit:

Als u een bestaande on-premises implementatie-infrastructuur reeds geconfigureerd met een toestel VPN hebt, kunt u de architectuur verwijzing kunt implementeren door deze stappen uit:

1. Klik met de rechtermuisknop op de knop onderstaande en selecteer een van beide 'koppeling openen in nieuw tabblad"of 'Koppeling openen in nieuw venster':  
[![Implementeren naar Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy.json)

2. Wachten op een koppeling moet worden geopend in de portal van Azure en voert u de volgende stappen uit: 
  - De naam van de **resourcegroep** is al gedefinieerd in het parameterbestand, dus **Nieuw** te selecteren en voer `ra-hybrid-er-rg` in het tekstvak.
  - Selecteer de regio in de vervolgkeuzelijst **locatie** .
  - Bewerk de **Sjabloon hoofdsite Uri** of de **Parameter hoofdsite Uri** tekstvakken niet.
  - Controleer de bepalingen en voorwaarden en klik op het selectievakje **dat ik ga akkoord met de bovenstaande voorwaarden** .
  - Klik op de knop **aanschaffen** .

3. Wacht totdat de implementatie om te voltooien.

4. Klik met de rechtermuisknop op de knop onderstaande en selecteer een van beide 'koppeling openen in nieuw tabblad"of 'Koppeling openen in nieuw venster':  
[![Implementeren naar Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-er%2Fazuredeploy-expressRouteCircuit.json)

5. Wachten op een koppeling moet worden geopend in de portal van Azure en voert u de volgende stappen uit: 
  - Selecteer **bestaande gebruiken** in de sectie **resourcegroep** en voer `ra-hybrid-er-rg` in het tekstvak.
  - Selecteer de regio in de vervolgkeuzelijst **locatie** .
  - Bewerk de **Sjabloon hoofdsite Uri** of de **Parameter hoofdsite Uri** tekstvakken niet.
  - Controleer de bepalingen en voorwaarden en klik op het selectievakje **dat ik ga akkoord met de bovenstaande voorwaarden** .
  - Klik op de knop **aanschaffen** .

6. Wacht totdat de implementatie om te voltooien.

## <a name="next-steps"></a>Volgende stappen

- Zie [een netwerkarchitectuur ten zeerste beschikbaar hybride implementeren] [ highly-available-network-architecture] voor informatie over de beschikbaarheid van een hybride netwerk op basis van ExpressRoute worden niet via een VPN-verbinding.

<!-- links -->
[expressroute-technical-overview]: ../expressroute/expressroute-introduction.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[expressroute-prereqs]: ../expressroute/expressroute-prerequisites.md
[configure-expressroute-routing]: ../expressroute/expressroute-howto-routing-arm.md
[sla-for-expressroute]: https://azure.microsoft.com/support/legal/sla/expressroute/v1_0/
[link-vnet-to-expressroute]: ../expressroute/expressroute-howto-linkvnet-arm.md
[ExpressRoute-provisioning]: ../expressroute/expressroute-workflows.md
[expressroute-pricing]: https://azure.microsoft.com/pricing/details/expressroute/
[expressroute-limits]: ../azure-subscription-service-limits.md#networking-limits
[sample-script]: #sample-solution-script
[connectivity-providers]: ../expressroute/expressroute-introduction.md#how-can-i-connect-my-network-to-microsoft-using-expressroute
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[forced-tuneling]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[highly-available-network-architecture]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[arm-templates]: ../resource-group-authoring-templates.md
[naming-conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/deploy-reference-architecture.sh
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetwork.parameters.json
[virtualnetworkgateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/virtualNetworkGateway.parameters.json
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[er-circuit-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-er/parameters/expressRouteCircuit.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/guidance-hybrid-network-expressroute/figure1.png "Hybride netwerkarchitectuur Azure ExpressRoute gebruiken"
[1]: ./media/guidance-hybrid-network-expressroute/figure2.png "Gebruik van redundante routers met ExpressRoute primaire en secundaire circuits"
[2]: ./media/guidance-hybrid-network-expressroute/figure3.png "Beveiligingsapparaten toevoegen aan de on-premises netwerk"
[3]: ./media/guidance-hybrid-network-expressroute/figure4.png "Gebruik gedwongen om te controleren Internet verkeer tunneling"
[4]: ./media/guidance-hybrid-network-expressroute/figure5.png "Zoeken naar de ServiceKey van een circuitlijnen ExpressRoute"
