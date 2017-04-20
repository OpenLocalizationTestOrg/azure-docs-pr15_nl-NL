<properties
   pageTitle="Web Application Firewall configureren op nieuwe of bestaande toepassingsgateway | Microsoft Azure"
   description="In dit artikel bevat instructies voor het web application firewall gebruiken op een bestaand of nieuw application gateway."
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>

# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Web Application Firewall configureren op een nieuw of bestaand toepassingsgateway

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-web-application-firewall-portal.md)
- [Azure resourcemanager PowerShell](application-gateway-web-application-firewall-powershell.md)

De firewall-toepassing (WAF) in het vak Azure-toepassingsgateway voorkomt webtoepassingen algemene aanvallen zoals SQL webweergave, uitvoeren van scripts op meerdere sites-aanvallen en sessie hijacks.

Azure-toepassingsgateway is een laag tot en met 7 taakverdeling. Deze biedt failover, performance-routing HTTP-aanvragen tussen verschillende servers, ongeacht of deze op de cloud of on-premises implementatie zijn. Toepassing bevat veel toepassing bezorging Controller (ADC) functies zoals HTTP van taakverdeling, cookie gebaseerde sessie affiniteit, Secure Sockets Layer (SSL) offload, aangepaste status sondes, ondersteuning voor multi-site en dergelijke. Ga voor een volledige lijst met ondersteunde functies naar overzicht van de Gateway-toepassing

Het volgende artikel ziet u hoe u [web application firewall aan een bestaande application gateway toevoegen](#add-web-application-firewall-to-an-existing-application-gateway) en [maken van een toepassingsgateway die gebruikmaakt van web application firewall](#create-an-application-gateway-with-web-application-firewall).

![scenario-afbeelding][scenario]

## <a name="waf-configuration-differences"></a>WAF configuratie verschillen

Als u [een toepassingsgateway met PowerShell maken](application-gateway-create-gateway-arm.md)hebt gelezen, kunt u de SKU-instellingen configureren bij het maken van een toepassingsgateway begrijpen. WAF biedt aanvullende instellingen op te geven tijdens het configureren van de SKU op een toepassingsgateway. Er zijn geen extra wijzigingen die u in de toepassingsgateway zelf aanbrengt.

**SKU** - een gateway normale toepassing zonder WAF ondersteunt **standaard\_kleine**, **standaard\_gemiddeld**, en **standaard\_groot** grootte. Door de introductie van WAF, zijn er twee extra SKU's, **WAF\_gemiddeld** en **WAF\_groot**. WAF wordt niet ondersteund op kleine Toepassingsgateways.

**Laag** - de beschikbare waarden zijn **standaard** - of **WAF**. Wanneer u een Web-toepassing Firewall gebruikt, moet **WAF** worden gekozen.

**Modus** - met deze instelling is de modus van WAF. toegestane waarden zijn **detecteren** en **tegenhouden**. Wanneer WAF in de modus voor detectie is ingesteld, worden alle threats worden opgeslagen in een logboekbestand. Gebeurtenissen nog steeds worden vastgelegd in de modus voor preventie, maar de hacker een 403 onbevoegde reactie van de toepassingsgateway krijgt.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Web application firewall toevoegen aan een bestaande application gateway

Zorg ervoor dat u de nieuwste versie van Azure PowerShell gebruikt. Meer informatie is beschikbaar op [Windows PowerShell gebruiken met bronbeheer](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Stap 1

Meld u aan bij uw Azure-Account.

    Login-AzureRmAccount

### <a name="step-2"></a>Stap 2

Selecteer het abonnement wilt gebruiken voor dit scenario.

    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Stap 3

De gateway die u Web Application Firewall om te toevoegt worden opgehaald.

    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"


### <a name="step-4"></a>Stap 4

Configureer de web-toepassing firewall sku. De beschikbare formaten zijn **WAF\_groot** en **WAF\_gemiddeld**. Wanneer web application firewall wordt gebruikt moet de laag **WAF**, de capaciteit moet worden bevestigd bij het instellen van de sku.

    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2

### <a name="step-5"></a>Stap 5

De WAF-instellingen configureren zoals gedefinieerd in het volgende voorbeeld:

Voor de instelling **WafMode** zijn de beschikbare waarden voorkomen en detectie.

    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention

### <a name="step-6"></a>Stap 6

Werk de toepassingsgateway met de instellingen in de vorige stap.

    Set-AzureRmApplicationGateway -ApplicationGateway $gw

Deze opdracht bijwerkt de toepassingsgateway met Web Application Firewall. Het wordt aanbevolen om weer te geven van de [Gateway diagnostische hulpprogramma's van toepassing](application-gateway-diagnostics.md) als u wilt weten hoe u kunt Logboeken voor uw toepassingsgateway weergeven. Vanwege de beveiliging aard van WAF moeten u Logboeken regelmatig begrip van de houding beveiliging van uw webtoepassingen worden beoordeeld.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Een toepassingsgateway maken met Web Application Firewall

De volgende stappen gaat u door het hele proces van begin tot eind voor het maken van een Gateway-toepassing met Web Application Firewall.

Zorg ervoor dat u de nieuwste versie van Azure PowerShell gebruikt. Meer informatie is beschikbaar op [Windows PowerShell gebruiken met bronbeheer](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Stap 1

Meld u aan bij Azure

    Login-AzureRmAccount

U wordt gevraagd om te verifiëren met uw referenties.

### <a name="step-2"></a>Stap 2

Controleer de abonnementen voor het account.

    Get-AzureRmSubscription

### <a name="step-3"></a>Stap 3

Kies welke van uw Azure-abonnementen te gebruiken.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-4"></a>Stap 4

Maak een resourcegroep (overslaan deze stap als u een bestaande resourcegroep gebruikt).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure resourcemanager is vereist dat alle resourcegroepen Geef een locatie. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Zorg ervoor dat alle opdrachten voor het maken van een toepassingsgateway gebruikmaakt van dezelfde resourcegroep.

In het voorgaande voorbeeld, maar we hebben gemaakt een resourcegroep 'appgw-RG' en de locatie 'West ons' genoemd.

>[AZURE.NOTE] Als u nodig hebt voor het configureren van een aangepaste test voor uw toepassingsgateway, raadpleegt u [een toepassingsgateway met aangepaste sondes via PowerShell maken](application-gateway-create-probe-ps.md). Bekijk de [aangepaste sondes en de statuscontrole](application-gateway-probe-overview.md) voor meer informatie.

### <a name="step-5"></a>Stap 5

Een adresbereiken toewijzen voor het subnet worden gebruikt voor de Gateway-toepassing zelf.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Een subnet voor een toepassing moet minimaal van 28 maskerbits. Deze waarde verlaat 10 beschikbaar adressen in het subnet voor toepassingsgateway-exemplaren. Met een kleinere subnet, u mogelijk niet meer exemplaar van de toepassingsgateway in de toekomst toevoegen.

### <a name="step-6"></a>Stap 6

Een adresbereik moet worden gebruikt voor de groep Backend-adressen toewijzen.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-7"></a>Stap 7

Maken, een virtueel netwerk met de voorgaande subnetten die in de resourcegroep is gemaakt in stap: [de Resource-groep maken](#create-the-resource-group)

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-8"></a>Stap 8

Ophalen van de resource van virtueel netwerk en subnetresources moet worden gebruikt in de volgende stappen:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

### <a name="step-9"></a>Stap 9

Een openbare IP-bron moet worden gebruikt voor de toepassingsgateway maken. Dit openbare IP-adres wordt gebruikt in een van de volgende stappen uit:

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Toepassingsgateway biedt geen ondersteuning voor het gebruik van een openbare IP-adres dat is gemaakt met een domeinlabel gedefinieerd. Alleen een openbare IP-adres met een domeinlabel dynamisch gemaakte wordt ondersteund. Als u een beschrijvende DNS-naam voor de toepassingsgateway vereist, is het raadzaam te gebruiken van een cname-record als een alias.

### <a name="step-10"></a>Stap 10

U moet alle configuratie-items instellen voordat u de toepassingsgateway maakt. De volgende stappen uit maken de configuratie-items die u nodig voor een resource van toepassing gateway hebt.

Maken van een toepassing gateway IP-configuratie, Hiermee configureert u welke subnet gebruikmaakt van de Gateway-toepassing. Wanneer de Gateway-toepassing wordt gestart, een IP-adres van het subnet geconfigureerd wordt opgehaald en stuurt netwerk verkeer door naar het IP-adressen in de groep back-enddatabase IP. Houd er rekening mee dat elk exemplaar één IP-adres duurt.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-11"></a>Stap 11

De back-enddatabase IP-adresgroep configureren met het IP-adressen van de backend-endwebservers. Deze IP-adressen zijn de IP-adressen die het netwerkverkeer die afkomstig zijn uit het front IP-eindpunt ontvangt. U vervangen de volgende IP-adressen als u wilt toevoegen van uw eigen toepassing IP-adres eindpunten.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

### <a name="step-12"></a>Stap 12

Upload het certificaat moet worden gebruikt op de resources in de backend ssl is ingeschakeld.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

### <a name="step-13"></a>Stap 13

De toepassing gateway back-enddatabase http-instellingen configureren. Toewijzen aan het certificaat in de vorige stap in de http-instellingen hebt geüpload.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-14"></a>Stap 14

Configureer de front-IP-poort voor het openbare IP-eindpunt. Dit wordt de poort die eindgebruikers verbinding met maken.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-15"></a>Stap 15

Maken van een front IP-configuratie, deze instelling wordt een persoonlijke of openbare IP-adres toegewezen aan de front-end van de toepassingsgateway. De volgende stap het openbare IP-adres in de vorige stap gekoppeld aan de front-IP-configuratie.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-16"></a>Stap 16

Het certificaat voor de toepassingsgateway configureren. Dit certificaat is gebruikt u ontsleutelt en het verkeer op de toepassingsgateway opnieuw te versleutelen.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

### <a name="step-17"></a>Stap 17

Maak de HTTP luisteraar ervan af voor de toepassingsgateway. Toewijzen aan de front-IP-configuratie, poorten en ssl-certificaat wilt gebruiken.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-18"></a>Stap 18

Een laden verdeling routeren regel maken die het gedrag van de verdeling van laden configureert. In dit voorbeeld wordt een eenvoudige round robin-regel gemaakt.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   
### <a name="step-19"></a>Stap 19

De grootte van het exemplaar van de toepassingsgateway configureren.

    $sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

>[AZURE.NOTE]  U kunt kiezen tussen **WAF\_gemiddeld** en **WAF\_groot**, de laag wanneer het gebruik van WAF altijd **WAF is**. Capaciteit is een willekeurig getal tussen 1 en 10.

### <a name="step-20"></a>Stap 20

Het configureren van de modus voor WAF, de waarden die zijn **voorkomen** en **detectie**.

    $config = New-AzureRmApplicationGatewayWafConfig -Enabled $true -WafMode "Prevention"

### <a name="step-21"></a>Stap 21

Een toepassingsgateway maken met alle items van de configuratie van de voorgaande stappen. In dit voorbeeld wordt de toepassingsgateway 'appgwtest' genoemd.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert

## <a name="get-application-gateway-dns-name"></a>Naam van de DNS-toepassing gateway ophalen

Nadat de gateway is gemaakt, is de volgende stap voor het configureren van de front-end voor communicatie. Wanneer u een openbare IP-adres, geeft toepassingsgateway een dynamisch toegewezen DNS-naam, dat wil niet geschikt zeggen is vereist. Om ervoor te zorgen eindgebruikers druk de toepassingsgateway een CNAME-record kunnen worden gebruikt zodat deze verwijzen naar de openbare eindpunt van de toepassingsgateway. [Het configureren van een aangepaste domeinnaam voor in Azure wordt aangegeven](../cloud-services/cloud-services-custom-domain-name-portal.md). Klik hiertoe ophalen details van de toepassingsgateway en de bijbehorende IP-/ DNS-naam met het PublicIPAddress-element dat is gekoppeld aan de toepassingsgateway. De van de toepassingsgateway DNS-naam moet worden gebruikt voor het maken van een CNAME-record, die wijst van de twee webtoepassingen naar deze DNS-naam. Het gebruik van de A-records wordt niet aanbevolen, omdat de VIP op opnieuw starten van toepassingsgateway kan veranderen.
    
    Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
        
    Name                     : publicIP01
    ResourceGroupName        : appgw-RG
    Location                 : westus
    Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
    Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
    ResourceGuid             : 00000000-0000-0000-0000-000000000000
    ProvisioningState        : Succeeded
    Tags                     : 
    PublicIpAllocationMethod : Dynamic
    IpAddress                : xx.xx.xxx.xx
    PublicIpAddressVersion   : IPv4
    IdleTimeoutInMinutes     : 4
    IpConfiguration          : {
                                 "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                               Configurations/frontend1"
                               }
    DnsSettings              : {
                                 "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                               }

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het configureren van diagnostische gegevens vastleggen, als u wilt de gebeurtenissen die zijn gevonden of is vanwege Meld met Web Application Firewall via de [Toepassing Gateway diagnostische gegevens](application-gateway-diagnostics.md)


[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png