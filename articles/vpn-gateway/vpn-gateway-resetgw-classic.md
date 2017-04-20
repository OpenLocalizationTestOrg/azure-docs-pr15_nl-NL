<properties
   pageTitle="Opnieuw instellen van een Gateway Azure VPN | Microsoft Azure"
   description="In dit artikel begeleidt u bij het opnieuw instellen van uw Azure VPN Gateway. Het artikel is van toepassing op VPN gateways in zowel het klassieke en de resourcemanager implementatiemodellen."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc"/>

# <a name="reset-an-azure-vpn-gateway-using-powershell"></a>Opnieuw een Azure VPN Gateway via PowerShell


In dit artikel begeleidt u bij het opnieuw instellen van uw Azure VPN Gateway met PowerShell-cmdlets. Deze instructies zijn zowel het implementatiemodel klassieke en het implementatiemodel resourcemanager.

Opnieuw instellen van de gateway Azure VPN is het handig als u meerdere lokale VPN-verbinding op een of meer S2S VPN tunnels kwijtraakt. In dit geval uw on-premises implementatie VPN apparaten alles goed werken, maar kunnen niet tot stand brengen van IPSec-tunnels met Azure VPN gateways. 

Elke gateway Azure VPN bestaat uit twee VM exemplaren die actief zijn in een actief stand-by-configuratie. Wanneer u de PowerShell-cmdlet opnieuw instellen van de gateway hebt gebruikt, deze opnieuw is opgestart de gateway, en klik vervolgens opnieuw de configuraties cross-premises ernaar te toegepast. De gateway zorgt ervoor dat het openbare IP-adres dat al heeft. Dit betekent dat u niet nodig bij de configuratie van de router VPN met een nieuwe openbare IP-adres van Azure VPN gateway.  

Nadat de opdracht wordt gegeven, het huidige actieve exemplaar van de gateway Azure VPN opnieuw is opgestart onmiddellijk. Er is een kort tussenruimte tijdens een de overname van het actieve exemplaar (wordt opnieuw is opgestart), in het exemplaar van de stand-by. De tussenruimte moet minder dan een minuut.

Als de verbinding niet na de eerste keer opnieuw opstarten is hersteld, moet u dezelfde opdracht opnieuw te starten van het tweede VM-exemplaar (de nieuwe actieve gateway). Als de twee opnieuw is opgestart gelijktijdige worden opgevraagd, wordt er een iets langere periode waarin opnieuw beide VM-exemplaren (active en stand-by) zijn wordt gestart. Hierdoor wordt een langere tussenruimte op de VPN-verbinding tot maximaal 2 voor 4 minuten voor VMs om te voltooien van de computer opnieuw hebt opgestart.

Nadat u hebt twee opnieuw opstarten, als u steeds cross-premises connectiviteitsproblemen ondervindt nog, open een verzoek voor ondersteuning van de Azure-portal.

## <a name="before-you-begin"></a>Voordat u begint

Voordat u uw gateway opnieuw hebt ingesteld, controleert u de belangrijkste punten hieronder ziet u voor elke tunnel IPsec naar website (S2S) VPN. Er treden eventuele komen niet overeen in de items in de verbinding is verbroken S2S VPN tunnel. Controleren en corrigeren van de configuraties voor uw on-premises en Azure VPN gateways, bespaart u uit onnodige opnieuw opstarten en onderbrekingen voor de andere werken verbindingen op gateways.

Controleer of u de volgende items voordat uw gateway:

- De Internet IP-adressen (VIP's) voor de gateway Azure VPN zowel de on-premises implementatie VPN gateway zijn juist geconfigureerd in zowel de Azure en het beleid van de VPN on-premises implementatie.
- De vooraf gedeelde sleutel moet hetzelfde op zowel Azure en on-premises VPN gateways.
- Als u specifieke IPsec/IKE configuratie, toepast zoals codering, hashing algoritmen en optie (perfecte doorsturen geheimhouding), zorgt ervoor zowel de Azure en on-premises VPN gateways de dezelfde configuraties.

## <a name="reset-a-vpn-gateway-using-the-resource-management-deployment-model"></a>Opnieuw een VPN-Gateway met het model van de implementatie resourcebeheer

De resourcemanager PowerShell-cmdlet voor het opnieuw instellen van de gateway is `Reset-AzureRmVirtualNetworkGateway`. Het volgende voorbeeld wordt de Azure VPN gateway, 'VNet1GW', in de resourcegroep "TestRG1".

    $gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
    Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw

## <a name="reset-a-vpn-gateway-using-the-classic-deployment-model"></a>Opnieuw een VPN-Gateway met het implementatiemodel klassieke

De PowerShell-cmdlet voor het opnieuw instellen van Azure VPN gateway is `Reset-AzureVNetGateway`. Het volgende voorbeeld wordt de gateway Azure VPN voor het virtuele netwerk 'ContosoVNet' genoemd.
 
    Reset-AzureVNetGateway –VnetName “ContosoVNet” 

Resultaat:

    Error          :
    HttpStatusCode : OK
    Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
    Status         : Successful
    RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
    StatusCode     : OK


## <a name="next-steps"></a>Volgende stappen
    
Zie de [verwijzing naar servicebeheer PowerShell-cmdlets](https://msdn.microsoft.com/library/azure/mt617104.aspx) en de [verwijzing naar resourcemanager PowerShell-cmdlets](http://go.microsoft.com/fwlink/?LinkId=828732) voor meer informatie.






