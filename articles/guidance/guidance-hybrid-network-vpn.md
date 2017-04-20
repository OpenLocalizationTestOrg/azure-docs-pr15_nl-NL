<properties
   pageTitle="Uitvoering van een hybride netwerkarchitectuur met Azure en On-premises VPN | Microsoft Azure"
   description="Het implementeren van een beveiligde site-naar-site netwerkarchitectuur die betrekking hebben op een Azure virtuele netwerk en een on-premises netwerk verbonden via een VPN-verbinding."
   services=""
   documentationCenter="na"
   authors="RohitSharma-pnp"
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
   ms.author="roshar"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-and-on-premises-vpn"></a>Een hybride netwerkarchitectuur met Azure en On-premises VPN implementeren

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

In dit artikel vindt u een overzicht van een reeks procedures voor het uitbreiden van een on-premises netwerk naar gebruik van een site-naar-site VPN (VPN) Azure. De verkeersstromen tussen de on-premises netwerk en een Azure Virtual Network (VNet) via een VPN-IPSec-tunnel. Deze architectuur is geschikt voor hybride toepassingen met de volgende kenmerken:

- Onderdelen van de toepassing uitgevoerd on-premises implementatie terwijl anderen worden uitgevoerd in Azure wordt aangegeven.

- Het verkeer tussen on-premises implementatie hardware en de cloud is waarschijnlijk light of beter handel enigszins uitgebreide latentie voor de flexibiliteit en de verwerkingskracht van de cloud.

- De uitgebreide netwerk vormt een gesloten systeem. Er is geen directe pad van Internet naar de VNet Azure.

- Gebruikers verbinding maken met de on-premises netwerk gebruik van de services die worden gehost in Azure wordt aangegeven. De brug tussen de on-premises netwerk en de services die wordt uitgevoerd in Azure is transparant voor gebruikers.

Voorbeelden van scenario's die dit profiel past zijn:

- LOB-toepassingen gebruikt binnen een organisatie, waar deel van de functies zijn gemigreerd naar de cloud.

- Test-ontwikkeling/testomgeving werkbelasting.

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen: [Azure resourcemanager] [ resource-manager-overview] en klassieke. Deze blauwdruk gebruikt resourcemanager, waarin u wordt aangeraden voor nieuwe implementaties.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram worden de onderdelen in deze architectuur gemarkeerd:

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft Downloadcentrum][visio-download]. In dit diagram deel uitmaakt van de 'hybride netwerk - VPN' pagina.

![[0]][0]

- **On-premises netwerk.** Dit is een netwerk van computers en apparaten, verbonden via een privé LAN-netwerk uitgevoerd binnen een organisatie.

- **VPN toestel.** Dit is een apparaat of service waarmee met een externe connectiviteit met de on-premises netwerk. Het toestel VPN mogelijk een apparaat kan, of dit een softwareoplossing zoals de Routering en Remote Access Service (RRAS) in Windows Server 2012.

> [AZURE.NOTE] Voor een lijst met ondersteunde VPN-apparaten en informatie over het configureren van de geselecteerde VPN-apparaten om verbinding te maken met een VPN-Gateway Azure, raadpleegt u de instructies voor het gewenste apparaat in de [lijst met VPN apparaten worden ondersteund door Azure][vpn-appliance].

- **N lagen cloud-toepassing.** Dit is de toepassing die wordt gehost in Azure wordt aangegeven. Deze kan meerdere niveaus, met meerdere subnetten die zijn verbonden via Azure netwerktaakverdelers bevatten. Het verkeer in elk subnet kan er kosten regels die zijn gedefinieerd door middel van [Azure netwerk beveiligingsgroepen (NSGs)]worden[azure-network-security-group]. Zie voor meer informatie [aan de slag met Microsoft Azure-beveiliging][getting-started-with-azure-security].

> [AZURE.NOTE] In dit artikel worden de cloud-toepassing als een eenheid. Zie [een architectuur N lagen op Azure uitgevoerd] [ implementing-a-multi-tier-architecture-on-Azure] voor gedetailleerde informatie.

- **[Virtuele netwerk (VNet)][azure-virtual-network].** De cloud-toepassing en de onderdelen voor de Gateway van de VPN Azure bevinden zich in de dezelfde VNet.

- **[Azure VPN Gateway][azure-vpn-gateway].** De gateway-service VPN kunt u de VNet zijn verbonden met de on-premises netwerk via een VPN-toestel. Zie voor meer informatie [verbinding maken met een on-premises netwerk met een Microsoft Azure virtual netwerk][connect-to-an-Azure-vnet]. De Gateway VPN omvat de volgende elementen:

  - **Virtuele netwerkgateway.** Dit is een resource die een virtuele VPN toestel voor de VNet biedt. Dit is verantwoordelijk voor het routeren van verkeer van de on-premises netwerk naar de VNet.

  - **Lokale netwerkgateway.** Dit is een abstractie van het apparaat van de VPN on-premises implementatie. Netwerkverkeer vanuit de cloud-toepassing naar de on-premises netwerk is gerouteerd via deze gateway.

  - **Verbinding.** De verbinding heeft eigenschappen die het verbindingstype (IPSec) en de toets met het on-premises implementatie VPN toestel gedeeld te versleutelen verkeer.

  - **Gateway-subnet.** De netwerkgateway virtual wordt in een eigen subnet, dat wil gelden er verschillende vereisten, zeggen zoals wordt beschreven in de sectie aanbevelingen hieronder bewaard.

- **Interne taakverdeling.** Netwerkverkeer van de Gateway VPN wordt doorgestuurd naar de cloud-toepassing via een interne taakverdeling. De taakverdeling bevindt zich in het front subnet van de toepassing.

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en resourcetypen, zodat deze architectuur verwijzing kan worden deze is ingericht veel verschillende manieren. We hebben een resourcemanager Azure-sjabloon om te kunnen installeren van de architectuur van de verwijzing die deze aanbevelingen volgt opgegeven. Als u wilt maken van de architectuur van uw eigen verwijzing moet u deze aanbevelingen volgt, tenzij u specifieke vereisten die een aanbeveling niet past hebt.

### <a name="vnet-and-gateway-subnet"></a>VNet en de Gateway subnet

Maak een Azure-VNet voor het vasthouden van de onderdelen in de cloud. De adresruimte van de Azure-VNet moet groot genoeg voor de adressen die door de VMs en subnetten in de VNet. Zorg ervoor dat de adresruimte VNet voldoende ruimte voor groei heeft als extra VMs waarschijnlijk in de toekomst nodig is. De adresruimte van de VNet moet niet overlappen met het on-premises netwerk. Het bovenstaande diagram wordt bijvoorbeeld het adres ruimte 10.20.0.0/16 gebruikt voor de VNet.

Een benoemde _GatewaySubnet_, met een adresbereiken van /27 subnet maken. Dit subnet is vereist door de gateway virtueel netwerk en het toewijzen van 32 adressen naar dit subnet helpt om te voorkomen dat u in de toekomst zich verhouden tot de mogelijke gateway groottebeperkingen uitgevoerd. Plaats dit subnet in het midden van de adresruimte. Er is een goede gewoonte om in te stellen van de adresruimte voor de gateway-subnet aan de bovenkant van de VNet-adresruimte. Het voorbeeld wordt in het diagram wordt 10.20.255.224/27 gebruikt.  Een snelle procedure voor het berekenen van de CIDR is als volgt:

1. Stel de variabele bits in de adresruimte van de VNet 1, tot aan de bits wordt gebruikt door de gateway-subnet, worden de overige bits ingesteld op 0.

2. De resulterende bits converteren naar een decimaal getal en deze als een adresruimte met het voorvoegsel lengte instellen voor de grootte van de gateway-subnet uit te drukken.

Voor een VNet met een IP-adresbereiken van 10.20.0.0/16 wordt toepassen stap #1 hierboven bijvoorbeeld 10.20.0b11111111.0b11100000.  Die converteren naar een decimaal getal en deze uit te drukken terwijl een adresruimte 10.20.255.224/27 oplevert

> [AZURE.WARNING] Implementeer niet andere virtuele machines of rol exemplaren subnet, de gateway. Ook geen toewijst een NSG aan dit subnet, omdat de gateway niet meer werkt.

### <a name="virtual-network-gateway"></a>Virtuele netwerkgateway

Een openbare IP-adres voor de gateway virtueel netwerk toewijzen.

De gateway virtueel netwerk maken in het subnet Gateway en het nieuw toegewezen openbare IP-adres toewijzen. Gebruik van de gateway-type die het meest overeenkomt met uw vereisten en die door uw toestel VPN is ingeschakeld:

Een [gateway - beleid] maken[ policy-based-routing] als u moet nauwkeurig bepalen hoe de routering van aanvragen. op basis van beleid criteria zoals adres voorvoegsels voor eenheden. Beleid gebaseerde gateways gebruik statische routering en alleen werken met site-naar-site verbindingen.

Maken van een [gateway route gebaseerde] [ route-based-routing] als u verbinding met de on-premises netwerk met RRAS maakt, ondersteuning voor multi-site of cross-regio-verbindingen of VNet-naar-VNet verbindingen (inclusief routes die meerdere VNets bladeren) implementeert. Route gebaseerde doelgateway gebruiken dynamische zoekresultaten omleiden naar directe verkeer tussen netwerken te gebruiken. Ze kunnen fouten in het netwerkpad beter dan statische routes, zonder omdat ze alternatieve routes kunnen proberen. Route gebaseerde gateways kunnen ook de realiseren management verkleinen omdat routes niet handmatig worden bijgewerkt moeten mogelijk wanneer netwerkadressen wijzigt.

Zie voor een lijst met ondersteunde VPN-apparaten, [over VPN apparaten voor Site-naar-Site VPN Gateway verbindingen][vpn-appliances].

> [AZURE.NOTE] Nadat de gateway is gemaakt, kunt u niet wijzigen tussen gateway typen zonder het verwijderen en opnieuw de gateway te maken.

Selecteer de Azure VPN Gateway SKU die het meest overeenkomt met uw vereisten voor doorvoer. Azure VPN Gateway is beschikbaar in drie SKU's weergegeven in de volgende tabel. Elke SKU vindt u verschillende doorvoer, [kosten worden geheven] [ azure-gateway-charges] op basis van de hoeveelheid tijd die de gateway is ingericht en beschikbaar.

| SKU | VPN doorvoer | Max IPSec-Tunnels|
|-----|----------------|------------------|
| Eenvoudige | 100 Mbps | 10 |
| Standaard | 100 Mbps | 10 |
| Krachtige | 200 Mbps | 30 |

> [AZURE.NOTE] De eenvoudige SKU is niet compatibel met Azure ExpressRoute. U kunt [wijzigen welke SKU] [ changing-SKUs] nadat de gateway is gemaakt.

Routeren regels maken voor de gateway-subnetten die directe binnenkomende toepassing verkeer van de gateway bij naar de interne taakverdeling in plaats van aanvragen om rechtstreeks naar de VMs die implementeren van de toepassing te toestaan.

### <a name="on-premises-network-connection"></a>On-premises implementatie netwerkverbinding

Een gateway lokale netwerk maken. Geef het openbare IP-adres van de on-premises implementatie VPN toestel en de adresruimte van de on-premises netwerk. Houd er rekening mee dat het on-premises implementatie VPN toestel een openbare IP-adres zijn dat kan worden gebruikt door de gateway lokale netwerk in Azure VPN Gateway moet hebben. Het VPN-apparaat wordt niet gevonden achter een NAT-apparaat.

Een site op site-verbinding voor de gateway virtueel netwerk en de gateway lokale netwerk maken. Selecteer het type van de verbinding aan website (IPSec) en de gedeelde sleutel opgeven. Naar website versleuteling met de Azure VPN Gateway is gebaseerd op het IPSec-protocol, met behulp van vooraf gedeelde sleutels voor verificatie. U kunt de sleutel opgeven bij het maken van de Azure VPN Gateway. U kunt het VPN toestel on-premises implementatie uitgevoerd met dezelfde sleutel moet configureren. Andere verificatiemethoden worden momenteel niet ondersteund.

Zorg ervoor dat de on-premises implementatie infrastructuur voor de routering is geconfigureerd voor het aanvragen van doorsturen bedoeld voor de adressen in de VNet Azure bij het apparaat VPN.

Open een poorten vereist door de cloud-toepassing in de on-premises netwerk.

Test de verbinding om te controleren:

- Het on-premises implementatie VPN toestel stuurt correct verkeer door naar de cloud-toepassing via de Azure VPN Gateway.

- De VNet stuurt verkeer door correct terug naar de on-premises netwerk.

- Verboden verkeer in beide richtingen is juist geblokkeerd.

## <a name="scalability-considerations"></a>Aandachtspunten voor de schaalbaarheid

U kunt beperkte verticale schaalbaarheid bereiken door te verplaatsen tussen de Basic en Standard VPN Gateway-SKU's naar de hoge prestaties VPN SKU.

Voor VNets dat een grote hoeveelheid VPN-verkeer verwacht, kunt u bovendien de verschillende werkbelastingen in afzonderlijke kleinere VNets distribueren en configureren van een Gateway VPN voor elk van deze.

U kunt de VNet partitioneren horizontaal of verticaal. Als u wilt horizontaal partitioneren, door sommige VM exemplaren van elke laag over subnetten van de nieuwe VNet te verplaatsen. Het resultaat is dat elke VNet dezelfde structuur en functionaliteit heeft. Als u wilt partitioneren verticaal, het ontwerp van elke laag om de functionaliteit voor verdeeld over verschillende logische gebieden (zoals orders voor de verwerking, facturering, accountbeheer klant, enzovoort). Elk functioneel gebied kan vervolgens worden geplaatst in een eigen VNet.

Een lokale Active Directory-domeincontroller in de VNet repliceren en implementeren van DNS in de VNet, kunnen helpen te verkleinen enkele van de beveiligings- en administratieve verkeer die doorloopt vanuit on-premises in de cloud.

## <a name="availability-considerations"></a>Aandachtspunten voor de beschikbaarheid

Als u nodig hebt om ervoor te zorgen dat de on-premises netwerk beschikbaar voor de Gateway van de VPN Azure blijft, moet u een failovercluster implementeren voor de on-premises implementatie VPN Gateway.

Als uw organisatie meerdere on-premises implementatie-sites heeft, maakt u [meerdere site verbindingen] [ vpn-gateway-multi-site] naar een of meer Azure VNets. Deze methode is vereist dynamische (route-basis) routering, dus zorg ervoor dat deze functie wordt ondersteund door de on-premises implementatie VPN Gateway.

Zie [SLA voor VPN Gateway] [ sla-for-vpn-gateway] voor de details van de VPN Gateway SLA.

## <a name="manageability-considerations"></a>Aandachtspunten voor de beheerbaarheid

Diagnostische gegevens uit lokale VPN toestellen bewaken. Dit proces, is afhankelijk van de functies van de VPN toestel. Bijvoorbeeld als u de Routering en Remote Access Service op Windows Server 2012 gebruikt, moet u inschakelen [RRAS logboekregistratie][rras-logging].

Gebruik het [Diagnostisch hulpprogramma Azure VPN Gateway] [ gateway-diagnostic-logs] om vast te leggen informatie over verbindingsproblemen. Deze logboeken kunnen worden gebruikt voor het bijhouden van gegevens, zoals de bron- en bestemmingen van aanvragen, verbinding welke protocol is gebruikt, en hoe de verbinding is gemaakt (of waarom het mislukt).

De operationele logboeken van de Azure VPN Gateway controleren met behulp van de controlelogboeken bijhouden die beschikbaar zijn in de portal van Azure. Afzonderlijke logboeken zijn beschikbaar voor de gateway lokale netwerk, de netwerkgateway Azure en de verbinding. Deze informatie kan worden gebruikt voor het bijhouden van wijzigingen aan de gateway en is handig als u een eerder functioneel gateway werkt niet voor andere reden.

![[2]][2]

Connectiviteit bewaken en connectivity mislukte gebeurtenissen bijhouden. Kunt u een controle pakket zoals [Nagios] [ nagios] om te leggen en deze informatie rapporteren.

## <a name="security-considerations"></a>Veiligheidsoverwegingen

Een andere gedeelde sleutel voor elke gateway VPN genereren. Gebruik een sterke gedeelde sleutel voor hulp gewelddadige aanvallen bestand.

> [AZURE.NOTE] Op dit moment niet u Azure-toets kluis gebruiken om vooraf-gedeelde sleutels Azure VPN gateway.

Zorg ervoor dat het on-premises implementatie VPN toestel gebruikmaakt van een versleutelingsmethode die [compatibel]is met de Gateway van de VPN Azure[vpn-appliance-ipsec]. De Gateway van de VPN Azure ondersteuning voor de versleuteling AES256, AES128 en 3DES algoritmen beleid-e-mailroutering. Route gebaseerde gateways ondersteunen AES256 en 3DES.

Als uw on-premises implementatie VPN toestel in een netwerk met een omtrek met een firewall tussen de omtrek en-netwerk en Internet, moet u mogelijk [Extra firewallregels] configureren[ additional-firewall-rules] toe te staan dat de site-naar-site VPN-verbinding.

Als de toepassing in het VNet gegevens met Internet verzendt, Overweeg de [implementatie van afgedwongen tunneling] [ forced-tunneling] om te leiden van al het verkeer Internet gebonden via de on-premises netwerk. Deze methode kunt u uitgaande aanvragen die zijn aangebracht door de toepassing van de on-premises implementatie-infrastructuur controleren.

> [AZURE.NOTE] Afgedwongen tunneling kan van invloed zijn op connectiviteit met Azure services (de Storage-Service, bijvoorbeeld) en Windows Licentiebeheer.

## <a name="troubleshooting-considerations"></a>Informatie over probleemoplossing

> [AZURE.NOTE] Ga naar de Routering en externe toegang-Blog voor algemene informatie over [het oplossen van veelvoorkomende fouten in VPN-gerelateerde][troubleshooting-vpn-errors].

Als verkeer niet kan doorlopen van de VPN-verbinding, controleert u het volgende:

### <a name="on-premises-vpn-appliance"></a>On-premises implementatie VPN toestel:

**Werkt het on-premises implementatie VPN toestel goed?**

Selectievakje alle gegenereerd door het toestel VPN voor fouten en fouten logboekbestanden. De locatie van deze gegevens zijn afhankelijk van uw toestel. Als u RRAS op Windows Server 2012 gebruikt, kunt u bijvoorbeeld de volgende PowerShell-opdracht gebruiken om weer te geven van de gebeurtenis foutgegevens voor de RRAS-service:

```
Get-EventLog -LogName System -EntryType Error -Source RemoteAccess | Format-List -Property *
```

De eigenschap _bericht_ van elk item vindt u een beschrijving van de fout. Algemene voorbeelden zijn:

- Verbinding, mogelijk vanwege een onjuiste IP-adres dat is opgegeven voor de Gateway Azure VPN in de interface van RRAS VPN netwerkconfiguratie meer:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {41, 3, 0, 0}
Index              : 14231
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: The network connection between your computer and
                     the VPN server could not be established because the remote server is not responding. This could
                     be because one of the network devices (e.g, firewalls, NAT, routers, etc) between your computer
                     and the remote server is not configured to allow VPN connections. Please contact your
                     Administrator or your service provider to determine which device may be causing the problem.
Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, The network connection between
                     your computer and the VPN server could not be established because the remote server is not
                     responding. This could be because one of the network devices (e.g, firewalls, NAT, routers, etc)
                     between your computer and the remote server is not configured to allow VPN connections. Please
                     contact your Administrator or your service provider to determine which device may be causing the
                     problem.}
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:26:02 PM
TimeWritten        : 3/18/2016 1:26:02 PM
UserName           :
Site               :
Container          :
```

- De verkeerde gedeelde sleutel wordt opgegeven in de interface van RRAS VPN netwerkconfiguratie:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {233, 53, 0, 0}
Index              : 14245
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: IKE authentication credentials are unacceptable

Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, IKE authentication credentials are
                     unacceptable
                     }
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:34:22 PM
TimeWritten        : 3/18/2016 1:34:22 PM
UserName           :
Site               :
Container          :
```

U kunt ook aanvragen gebeurtenislogboek informatie over pogingen via de RRAS-service verbinding maken met behulp van de volgende PowerShell-opdracht:

```
Get-EventLog -LogName Application -Source RasClient | Format-List -Property *
```

In het geval van een kan geen verbinding maken, bevat dit logboek fouten die er ongeveer als volgt uit:

```
EventID            : 20227
MachineName        : on-prem-vm
Data               : {}
Index              : 4203
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : CoId={B4000371-A67F-452F-AA4C-3125AA9CFC78}: The user SYSTEM dialed a connection named
                     AzureGateway which has failed. The error code returned on failure is 809.
Source             : RasClient
ReplacementStrings : {{B4000371-A67F-452F-AA4C-3125AA9CFC78}, SYSTEM, AzureGateway, 809}
InstanceId         : 20227
TimeGenerated      : 3/18/2016 1:29:21 PM
TimeWritten        : 3/18/2016 1:29:21 PM
UserName           :
Site               :
Container          :
```

**Is het VPN toestel correct routeren van verkeer via de Gateway van de VPN Azure?**

Gebruik een hulpprogramma zoals [PsPing] [ psping] om te controleren of connectiviteit en routeren tussen de gateway VPN. Bijvoorbeeld, voer de volgende opdracht connectiviteit vanaf een lokale computer tot een webserver die zich op het VNet testen (vervangen `<<web-server-address>>` met het adres van de webserver):

```
PsPing -t <<web-server-address>>:80
```

Als de lokale computer verkeer naar de webserver doorsturen kunt, worden er ongeveer als volgt uit:

```
D:\PSTools>psping -t 10.20.0.5:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.0.5:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.0.5:80 (warmup): 6.21ms
Connecting to 10.20.0.5:80: 3.79ms
Connecting to 10.20.0.5:80: 3.44ms
Connecting to 10.20.0.5:80: 4.81ms

  Sent = 3, Received = 3, Lost = 0 (0% loss),
  Minimum = 3.44ms, Maximum = 4.81ms, Average = 4.01ms
```

Als de lokale computer niet kunt met de opgegeven bestemming communiceren, ziet u berichten als volgt:

```
D:\PSTools>psping -t 10.20.1.6:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.1.6:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.1.6:80 (warmup): This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80:
  Sent = 3, Received = 0, Lost = 3 (100% loss),
  Minimum = 0.00ms, Maximum = 0.00ms, Average = 0.00ms
```

**De firewall on-premises implementatie zijn toegestaan VPN verkeer door? De juiste poorten zijn geopend?**

Verifiëren dat het on-premises implementatie VPN toestel gebruikmaakt van een versleutelingsmethode die [compatibel]is met de Gateway van de VPN Azure[vpn-appliance].

De Gateway van de VPN Azure ondersteuning voor de versleuteling AES256, AES128 en 3DES algoritmen beleid-e-mailroutering. Route gebaseerde gateways ondersteunen AES256 en 3DES.

### <a name="azure-vnet-gateway"></a>Azure VNet Gateway:

[Diagnostische logboeken Azure VPN Gateway] onderzoeken[ gateway-diagnostic-logs] voor mogelijke problemen.

**Zijn de Azure VPN Gateway en on-premises implementatie VPN toestel is geconfigureerd met dezelfde gedeelde verificatiesleutel?**

U kunt de gedeelde sleutel die is opgeslagen door de Gateway van de VPN Azure met behulp van de volgende opdracht uit Azure CLI weergeven:

```
azure network vpn-connection shared-key show <<resource-group>> <<vpn-connection-name>>
```

Gebruik de opdracht geschikt te maken voor uw on-premises implementatie VPN toestel te geven van de gedeelde sleutel die is geconfigureerd voor dat toestel.

Controleer of het _GatewaySubnet_ subnet vasthouden van dat de Azure VPN Gateway is niet gekoppeld aan een NSG.

U kunt de details subnet weergeven met behulp van de volgende opdracht uit Azure CLI:

```
azure network vnet subnet show -g <<resource-group>> -e <<vnet-name>> -n GatewaySubnet
```

Controleer of er is geen gegevensveld benoemde _netwerk beveiliging groeps-id_. Het volgende voorbeeld ziet u de resultaten voor exemplaar van de _GatewaySubnet_ waarop een toegewezen NSG (_VPN-Gateway-groep_). Dit kan leiden tot de gateway niet goed werkt als er alle regels die is gedefinieerd voor deze NSG zijn voorkomen:

```
C:\>azure network vnet subnet show -g profx-prod-rg -e profx-vnet -n GatewaySubnet
    info:    Executing command network vnet subnet show
    + Looking up virtual network "profx-vnet"
    + Looking up the subnet "GatewaySubnet"
    data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/virtualNetworks/profx-vnet/subnets/GatewaySubnet
    data:    Name                            : GatewaySubnet
    data:    Provisioning state              : Succeeded
    data:    Address prefix                  : 10.20.3.0/27
    data:    Network Security Group id       : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/networkSecurityGroups/VPN-Gateway-Group
    info:    network vnet subnet show command OK
```

**De virtuele machines in de VNet Azure zodanig zijn geconfigureerd dat verkeer afkomstig van buiten de VNet? Controleer de NSG regels die zijn gekoppeld aan subnetten die deze virtuele machines bevat.**

U kunt alle regels voor NSG weergeven met behulp van de volgende opdracht uit Azure CLI:

```
azure network nsg show -g <<resource-group>> -n <<nsg-name>>
```

**Is de Gateway van de VPN Azure verbroken?**

U kunt de volgende opdracht uit Azure PowerShell gebruiken om te controleren van de huidige status van de Azure VPN-verbinding. De `<<connection-name>>` parameter is de naam van de Azure VPN-verbinding die de gateway virtueel netwerk en de lokale gateway koppelt.

```
Get-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> - ResourceGroupName <<resource-group>>
```

De volgende knipsels Markeer de uitvoer is gegenereerd als de gateway is verbonden (het eerste voorbeeld) en de verbinding verbroken (het tweede voorbeeld):

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : Connected
EgressBytesTransferred     : 55254803
IngressBytesTransferred    : 32227221
ProvisioningState          : Succeeded
...
```

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection2 -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : NotConnected
EgressBytesTransferred     : 0
IngressBytesTransferred    : 0
ProvisioningState          : Succeeded
...
```

### <a name="host-vm-configuration-network-bandwidth-utilization-and-application-performance"></a>VM hostconfiguratie, netwerk-bandbreedtegebruik en toepassingsprestaties:

Controleer of de firewall in het gastbesturingssysteem op de VMs Azure in het subnet correct zijn geconfigureerd voor het toegestane verkeer van de on-premises implementatie IP-bereiken.

**Is het volume van verkeer dicht bij de limiet van de bandbreedte beschikbaar voor de Gateway van de VPN Azure?**

Tooling, is afhankelijk van de installaties beschikbaar zijn voor uw VPN toestel on-premises implementatie uitgevoerd. Bijvoorbeeld als u RRAS op Windows Server 2012 gebruikt, kunt u prestaties Monitor voor het bijhouden van de hoeveelheid gegevens die worden ontvangen en verzonden via de VPN-verbinding; met het _Totale RAS_ -object, selecteert u de items _Ontvangen Bytes per seconde_ en _Verzonden Bytes per seconde_ :

![[3]][3]

U moet de resultaten vergelijken met de bandbreedte die beschikbaar zijn voor de gateway VPN (100 voor de Basic en Standard SKU's, en 200 Mbps voor de hoge prestaties SKU):

![[4]][4]

**Een van de virtuele machines in de VNet Azure langzaam worden uitgevoerd? Ze overbelast zijn, zijn er genoeg deze u omgaat met het selectievakje laden, alle netwerktaakverdelers geconfigureerd zijn?**

Hiervoor is vereist [vastleggen en diagnostische gegevens analyseren][azure-vm-diagnostics]. U kunt de resultaten met behulp van de Azure portal bekijken, maar veel hulpprogramma's van derden zijn ook beschikbaar kan die gedetailleerde inzicht krijgen in de prestatiegegevens bieden.

**Is de toepassing efficiënter cloud resources gebruiken?**

Instrument toepassingscode die wordt uitgevoerd op elke VM om te bepalen of toepassingen optimaal gebruik van resources stapt. U kunt hulpprogramma's zoals [Toepassing inzichten] [ application-insights] zodat deze taken uitvoeren.

## <a name="solution-deployment"></a>Implementatie van oplossingen

Als u een bestaande on-premises implementatie-infrastructuur reeds geconfigureerd met een toestel VPN hebt, kunt u de architectuur verwijzing kunt implementeren door deze stappen uit:

1. Klik met de rechtermuisknop op de knop onderstaande en selecteer een van beide 'koppeling openen in nieuw tabblad"of 'Koppeling openen in nieuw venster':  
[![Implementeren naar Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn%2Fazuredeploy.json)

2. Wachten op een koppeling moet worden geopend in de portal van Azure en voert u de volgende stappen uit: 
    - De naam van de **resourcegroep** is al gedefinieerd in het parameterbestand, dus **Nieuw** te selecteren en voer `ra-hybrid-vpn-rg` in het tekstvak.
    - Selecteer de regio in de vervolgkeuzelijst **locatie** .
    - Bewerk de **Sjabloon hoofdsite Uri** of de **Parameter hoofdsite Uri** tekstvakken niet.
    - Controleer de bepalingen en voorwaarden en klik op het selectievakje **dat ik ga akkoord met de bovenstaande voorwaarden** .
    - Klik op de knop **aanschaffen** .

3. Wacht totdat de implementatie om te voltooien.

## <a name="next-steps"></a>Volgende stappen

- [Installatie van controller voor een on-premises Active Directory-domein VMs gebruiken in een Azure-VNet][installing-ad].

- [Richtlijnen voor het implementeren van Windows Server Active Directory op Azure VMs][deploying-ad].

- [Een DNS-server maken in een VNet][creating-dns].

- [Configureren en beheren van DNS in een VNet][configuring-dns].

- [Gebruik van de on-premises implementatie Stormshield netwerkapparaten beveiliging via een VPN][stormshield].

- [Uitvoering van een hybride netwerkarchitectuur met Azure ExpressRoute][expressroute].

<!-- links -->

[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[arm-templates]: ../resource-group-authoring-templates.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-portal]: ../azure-portal/resource-group-portal.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[azure-virtual-network]: ../virtual-network/virtual-networks-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: https://azure.microsoft.com/services/vpn-gateway/
[azure-gateway-charges]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[azure-network-security-group]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[vpn-gateway-multi-site]: ../vpn-gateway/vpn-gateway-multi-site.md
[policy-based-routing]: https://en.wikipedia.org/wiki/Policy-based_routing
[route-based-routing]: https://en.wikipedia.org/wiki/Static_routing
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[sla-for-vpn-gateway]: https://azure.microsoft.com/support/legal/sla/vpn-gateway/v1_0/
[additional-firewall-rules]: https://technet.microsoft.com/library/dn786406.aspx#firewall
[nagios]: https://www.nagios.org/
[azure-vpn-gateway-diagnostics]: http://blogs.technet.com/b/keithmayer/archive/2014/12/18/diagnose-azure-virtual-network-vpn-connectivity-issues-with-powershell.aspx
[ping]: https://technet.microsoft.com/library/ff961503.aspx
[tracert]: https://technet.microsoft.com/library/ff961507.aspx
[psping]: http://technet.microsoft.com/sysinternals/jj729731.aspx
[nmap]: http://nmap.org
[changing-SKUs]: https://azure.microsoft.com/blog/azure-virtual-network-gateway-improvements/
[gateway-diagnostic-logs]: http://blogs.technet.com/b/keithmayer/archive/2015/12/07/step-by-step-capturing-azure-resource-manager-arm-vnet-gateway-diagnostic-logs.aspx
[troubleshooting-vpn-errors]: https://blogs.technet.microsoft.com/rrasblog/2009/08/12/troubleshooting-common-vpn-related-errors/
[rras-logging]: https://www.petri.com/enable-diagnostic-logging-in-windows-server-2012-r2-routing-and-remote-access
[create-on-prem-network]: https://technet.microsoft.com/library/dn786406.aspx#routing
[create-azure-vnet]: ../virtual-network/virtual-networks-create-vnet-classic-cli.md
[azure-vm-diagnostics]: https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/
[application-insights]: ../application-insights/app-insights-overview-usage.md
[forced-tunneling]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-forced-tunneling/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[vpn-appliances]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[installing-ad]: ../active-directory/active-directory-install-replica-active-directory-domain-controller.md
[deploying-ad]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[creating-dns]: https://blogs.msdn.microsoft.com/mcsuksoldev/2014/03/04/creating-a-dns-server-in-azure-iaas/
[configuring-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[stormshield]: https://azure.microsoft.com/marketplace/partners/stormshield/stormshield-network-security-for-cloud/
[vpn-appliance-ipsec]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec-parameters
[expressroute]: ./guidance-hybrid-network-expressroute.md
[naming conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/deploy-reference-architecture.sh
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetwork.parameters.json
[virtualNetworkGateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetworkGateway.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/blueprints/hybrid-network-vpn.png "Structuur van een hybride netwerk die de on-premises implementatie in beslag nemen en cloud infrastructuur"
[1]: ./media/guidance-hybrid-network-vpn/partitioned-vpn.png "Een VNet om te verbeteren schaalbaarheid partitioneren"
[2]: ./media/guidance-hybrid-network-vpn/audit-logs.png "Controlelogboeken in de portal van Azure"
[3]: ./media/guidance-hybrid-network-vpn/RRAS-perf-counters.png "Prestatie-items voor een VPN-netwerkverkeer controleren"
[4]: ./media/guidance-hybrid-network-vpn/RRAS-perf-graph.png "Voorbeeld VPN netwerk prestaties graph"