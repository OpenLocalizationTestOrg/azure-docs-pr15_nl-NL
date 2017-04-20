<properties
   pageTitle="Een toepassingsgateway voor SSL offload geconfigureerd met behulp van Azure resourcemanager | Microsoft Azure"
   description="Deze pagina bevat instructies voor het maken van een toepassingsgateway met SSL offload met behulp van Azure resourcemanager"
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
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Een toepassingsgateway voor SSL offload geconfigureerd met behulp van Azure bronbeheer

> [AZURE.SELECTOR]
-[Azure-Portal](application-gateway-ssl-portal.md)
-[Azure resourcemanager PowerShell](application-gateway-ssl-arm.md)
-[Azure klassieke PowerShell](application-gateway-ssl.md)

 Azure-toepassingsgateway kan worden geconfigureerd om de sessie Secure Sockets Layer (SSL) op de gateway om te voorkomen dure SSL decoderen taken als u wilt uitvoeren op de webfarm te beëindigen. SSL-offload eenvoudiger ook de installatie van front-server en het beheer van de webtoepassing.

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de meest recente versie van de Azure PowerShell-cmdlets met behulp van het installatieprogramma van de Web-Platform. U kunt downloaden en installeren van de meest recente versie van het **Windows PowerShell** -gedeelte van de [pagina Downloads](https://azure.microsoft.com/downloads/).
2. U maakt een virtueel netwerk en een subnet voor de toepassingsgateway. Zorg ervoor dat er geen virtuele machines of cloud-implementaties het subnet worden gebruikt. Toepassingsgateway moet zich op zichzelf in een virtueel netwerk subnet.
3. De servers die u configureren voor het gebruik van de toepassingsgateway moeten bestaan of de eindpunten ervan hebt gemaakt in het virtuele netwerk of met een openbare IP-/ VIP hebt toegewezen.

## <a name="what-is-required-to-create-an-application-gateway"></a>Wat is vereist voor het maken van een toepassingsgateway?


- **Back-enddatabase server-groep:** De lijst met IP-adressen van de back-end-servers. De IP-adressen vermeld ofwel moeten behoren tot de virtuele netwerk subnet of een openbare IP/VIP moeten worden.
- **Back-enddatabase groep serverinstellingen:** Elke groep heeft instellingen, zoals poort, protocol en cookie gebaseerde affiniteit. Deze instellingen zijn gekoppeld aan een groep en worden toegepast op alle servers in de groep.
- **Front poort:** Deze poort is de openbare poort dat is geopend in de toepassingsgateway. Verkeer raakt deze poort en vervolgens wordt omgeleid naar een van de back-end-servers.
- **Luisteraar ervan af:** De luisteraar ervan af heeft een front poort, een protocol (Http of Https, deze instellingen zijn hoofdlettergevoelig), en de SSL-certificaat-naam (als SSL configureren offload).
- **Regel:** De regel de luisteraar ervan af en de back-enddatabase server-groep gekoppeld en wordt gedefinieerd welke back-end-server-groep het verkeer moet worden doorgestuurd naar wanneer deze een bepaalde luisteraar ervan af. Op dit moment wordt alleen de *eenvoudige* regel ondersteund. De *eenvoudige* regel is round robin laden verdeling.

**Aanvullende configuratie notities**

Voor SSL-certificaten configuratie, moet het protocol in **HttpListener** wijzigen in *Https* (hoofdlettergevoelig). Het element **SslCertificate** wordt toegevoegd aan **HttpListener** met de waarde van de variabele geconfigureerd voor het SSL-certificaat. De front poort moet worden bijgewerkt naar 443.

**Om in te schakelen affiniteit op basis van een cookie**: een toepassingsgateway kan worden geconfigureerd om ervoor te zorgen dat een verzoek om van een clientsessie altijd naar de dezelfde VM in de webfarm wordt gestuurd. Dit scenario wordt uitgevoerd door webweergave van een sessiecookie waarmee de gateway bij naar de verkeer correct. Stel om in te schakelen affiniteit cookie gebaseerde, **CookieBasedAffinity** op *ingeschakeld* in het element **BackendHttpSettings** .


## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Het verschil tussen het gebruik van de Azure klassieke implementatiemodel en Azure Resource Manager is de volgorde die u maakt een toepassingsgateway en de items die moeten worden geconfigureerd.

Met resourcemanager, alle onderdelen van een toepassingsgateway zijn geconfigureerd afzonderlijk en zet vervolgens samen om te maken van een gateway-resource van toepassing.


Hier volgen de vereiste stappen voor het maken van een toepassingsgateway:

1. Een resourcegroep maken voor bronbeheer
2. Virtuele netwerk, subnet en openbare IP-voor de toepassingsgateway maken
3. Een application gateway configuratie-object maken
4. Een bron van de gateway toepassing maken


## <a name="create-a-resource-group-for-resource-manager"></a>Een resourcegroep maken voor bronbeheer

Zorg ervoor dat u PowerShell-modus voor het gebruik van de cmdlets Azure resourcemanager overstappen. Meer informatie is beschikbaar op [Windows PowerShell gebruiken met bronbeheer](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Stap 1

    Login-AzureRmAccount

### <a name="step-2"></a>Stap 2

Controleer de abonnementen voor het account.

    Get-AzureRmSubscription

U wordt gevraagd om te verifiëren met uw referenties.<BR>

### <a name="step-3"></a>Stap 3

Kies welke van uw Azure-abonnementen te gebruiken. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Stap 4

Maak een resourcegroep (overslaan deze stap als u een bestaande resourcegroep gebruikt).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure resourcemanager is vereist dat alle resourcegroepen Geef een locatie. Deze instelling wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Zorg ervoor dat alle opdrachten voor het maken van een toepassingsgateway gebruikmaakt van dezelfde resourcegroep.

In het voorbeeld hierboven, we hebben gemaakt een resourcegroep 'appgw-RG' en de locatie 'West ons' genoemd.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en een subnet voor de toepassingsgateway maken

Het volgende voorbeeld ziet hoe u een virtueel netwerk maken met behulp van resourcemanager:

### <a name="step-1"></a>Stap 1

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

In dit voorbeeld wordt het adres bereik 10.0.0.0/24 toegewezen aan een variabele subnet moet worden gebruikt om te maken van een virtueel netwerk.

### <a name="step-2"></a>Stap 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

In dit voorbeeld wordt een virtueel netwerk met de naam 'appgwvnet' in resource groep "appgw-rg' voor een West Amerikaans gebied met het voorvoegsel 10.0.0.0/16 met subnet 10.0.0.0/24.

### <a name="step-3"></a>Stap 3

    $subnet = $vnet.Subnets[0]

In dit voorbeeld wordt het subnetobject toegewezen aan variabele $subnet voor de volgende stappen.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Maken van een openbare IP-adres van de front-configuratie

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

In dit voorbeeld wordt een openbare IP-resource 'publicIP01' in de groep "appgw-rg van de resource" voor de regio West VS.


## <a name="create-an-application-gateway-configuration-object"></a>Een application gateway configuratie-object maken

### <a name="step-1"></a>Stap 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

In dit voorbeeld wordt een toepassing gateway IP-configuratie met de naam 'gatewayIP01'. Wanneer toepassingsgateway wordt gestart, een IP-adres van het subnet geconfigureerd wordt opgehaald en netwerkverkeer doorsturen naar het IP-adressen in de groep van het IP-back-enddatabase. Houd er rekening mee dat elk exemplaar één IP-adres duurt.

### <a name="step-2"></a>Stap 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

In dit voorbeeld de back-enddatabase IP-adres van toepassingen met de naam "pool01" met IP-adressen configureert "134.170.185.46, 134.170.188.221,134.170.185.50." Deze waarden zijn de IP-adressen die het netwerkverkeer die afkomstig zijn uit het front IP-eindpunt ontvangt. Vervang de IP-adressen van het voorgaande voorbeeld door het IP-adressen van de eindpunten van de web-toepassingen.

### <a name="step-3"></a>Stap 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

In dit voorbeeld configureert toepassing gateway instelling "poolsetting01" op netwerkverkeer taakverdeling in de groep back-enddatabase.

### <a name="step-4"></a>Stap 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443

In dit voorbeeld Hiermee configureert u de front-IP-poort met de naam 'frontendport01' voor het openbare IP-eindpunt.

### <a name="step-5"></a>Stap 5

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password ‘<password>’

In dit voorbeeld configureert het certificaat voor SSL-verbinding. Het certificaat moet in .pfx-indeling en het wachtwoord moet tussen tekens met 4 t/m 12.

### <a name="step-6"></a>Stap 6

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

In dit voorbeeld wordt gemaakt van de front-IP-configuratie met de naam 'fipconfig01' en het openbare IP-adres worden gekoppeld aan de front-IP-configuratie.

### <a name="step-7"></a>Stap 7

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert


In dit voorbeeld wordt gemaakt van de luisteraar ervan af naam 'listener01' en de front-poort naar de front-IP-configuratie en het certificaat koppelen.

### <a name="step-8"></a>Stap 8

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

In dit voorbeeld wordt de laden verdeling doorstuurregel met de naam 'rule01' die het gedrag van de verdeling van laden configureert.

### <a name="step-9"></a>Stap 9

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

In dit voorbeeld wordt de grootte van het exemplaar van de toepassingsgateway geconfigureerd.

>[AZURE.NOTE]  De standaardwaarde voor *InstanceCount* is 2, met een maximumwaarde van 10. De standaardwaarde voor *GatewaySize* is normaal. U kunt kiezen tussen Standard_Small, Standard_Medium en Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Een toepassingsgateway maken met behulp van nieuw-AzureApplicationGateway

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert

In dit voorbeeld wordt een application gateway met alle items van de configuratie van de voorgaande stappen. In het voorbeeld wordt de toepassingsgateway 'appgwtest' genoemd.

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

Als u wilt configureren van een toepassingsgateway voor gebruik met een interne taakverdeling (ILB), raadpleegt u [een toepassingsgateway met een interne taakverdeling (ILB) maken](application-gateway-ilb.md).

Als u meer informatie over het laden taakverdeling opties in het algemeen wilt, Zie:

- [Azure taakverdeling](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure verkeer Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
