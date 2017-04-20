<properties
   pageTitle="Maken, start of een toepassingsgateway verwijderen met behulp van Azure resourcemanager | Microsoft Azure"
   description="Deze pagina bevat instructies voor het maken, configureren, starten en verwijderen van een toepassingsgateway Azure-via Azure resourcemanager"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Maken, start of een toepassingsgateway verwijderen met behulp van Azure resourcemanager

> [AZURE.SELECTOR]
- [Azure-Portal](application-gateway-create-gateway-portal.md)
- [Azure resourcemanager PowerShell](application-gateway-create-gateway-arm.md)
- [Azure klassieke PowerShell](application-gateway-create-gateway.md)
- [Azure resourcemanager-sjabloon](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure-toepassingsgateway is een laag tot en met 7 taakverdeling. Deze biedt failover, performance-routing HTTP-aanvragen tussen verschillende servers, ongeacht of deze op de cloud of on-premises implementatie zijn. Toepassingsgateway bevat veel toepassing bezorging Controller (ADC) functies zoals HTTP van taakverdeling, cookie gebaseerde sessie affiniteit, Secure Sockets Layer (SSL) offload, aangepaste status sondes, ondersteuning voor multi-site en dergelijke. Ga voor een volledige lijst met ondersteunde functies naar [Toepassing Gateway-overzicht](application-gateway-introduction.md)

In dit artikel begeleidt u bij de stappen voor het maken, configureren, starten en verwijderen van een toepassingsgateway.

>[AZURE.IMPORTANT] Voordat u met Azure resources werkt, is het belangrijk om te begrijpen dat Azure momenteel twee implementatiemodellen heeft: resourcemanager en klassiek. Zorg ervoor dat u [implementatiemodellen en hulpprogramma's voor](../azure-classic-rm.md) voordat het werken met een Azure bron begrijpt. U kunt de documentatie van verschillende hulpmiddelen weergeven door te klikken op de tabbladen aan de bovenkant van dit artikel. In dit document behandelt een toepassingsgateway maken met behulp van Azure resourcemanager. Als u wilt de lightversie gebruiken, gaat u naar [een gateway klassieke distributie via PowerShell maken](application-gateway-create-gateway.md).


## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de meest recente versie van de Azure PowerShell-cmdlets met behulp van het installatieprogramma van de Web-Platform. U kunt downloaden en installeren van de meest recente versie van het **Windows PowerShell** -gedeelte van de [pagina Downloads](https://azure.microsoft.com/downloads/).
2. Als u een bestaand virtuele netwerk hebt, selecteert u een bestaand leeg subnet of een subnet maken in uw bestaande virtuele netwerk uitsluitend voor gebruik door de toepassingsgateway. U kunt de toepassingsgateway bij naar een ander virtuele netwerk dan de bronnen die u wilt implementeren achter de toepassingsgateway niet implementeren.
3. De servers die u configureren voor het gebruik van de toepassingsgateway moeten bestaan of de eindpunten ervan hebt gemaakt in het virtuele netwerk of met een openbare IP-/ VIP hebt toegewezen.

## <a name="what-is-required-to-create-an-application-gateway"></a>Wat is vereist voor het maken van een toepassingsgateway?

- **Back-enddatabase server-groep:** De lijst met IP-adressen van de back-end-servers. De IP-adressen vermeld ofwel moeten behoren tot de virtuele netwerk subnet of een openbare IP/VIP moeten worden.
- **Back-enddatabase groep serverinstellingen:** Elke groep heeft instellingen, zoals poort, protocol en cookie gebaseerde affiniteit. Deze instellingen zijn gekoppeld aan een groep en worden toegepast op alle servers in de groep.
- **Front poort:** Deze poort is de openbare poort dat is geopend in de toepassingsgateway. Verkeer raakt deze poort en vervolgens wordt omgeleid naar een van de back-end-servers.
- **Luisteraar ervan af:** De luisteraar ervan af heeft een front poort, een protocol (Http of Https, deze waarden zijn hoofdlettergevoelig), en de SSL-certificaat-naam (als SSL configureren offload).
- **Regel:** De regel de luisteraar ervan af, de back-enddatabase server-groep gekoppeld en wordt gedefinieerd welke back-end-server-groep het verkeer moet worden doorgestuurd naar wanneer deze een bepaalde luisteraar ervan af.

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Het verschil tussen het gebruik van Classic Azure en Azure Resource Manager is de volgorde waarin u de toepassingsgateway en de items die moeten worden geconfigureerd maken.

Met resourcemanager, alle items die deel uitmaken van een toepassingsgateway zijn geconfigureerd afzonderlijk en zet vervolgens samen om te maken van de gateway-resource van toepassing.

Hier volgen de stappen die nodig zijn voor het maken van een toepassingsgateway.

## <a name="create-a-resource-group-for-resource-manager"></a>Een resourcegroep maken voor bronbeheer

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

    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="step-4"></a>Stap 4

Maak een resourcegroep (overslaan dit stap als u een bestaande resourcegroep gebruikt).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure resourcemanager is vereist dat alle resourcegroepen Geef een locatie. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Zorg ervoor dat alle opdrachten voor het maken van een toepassingsgateway gebruikmaakt van dezelfde resourcegroep.

In het voorbeeld hierboven, we hebben gemaakt een resourcegroep 'appgw-RG' en de locatie 'West ons' genoemd.

>[AZURE.NOTE] Als u nodig hebt voor het configureren van een aangepaste test voor uw toepassingsgateway, raadpleegt u [een toepassingsgateway met aangepaste sondes via PowerShell maken](application-gateway-create-probe-ps.md). Bekijk de [aangepaste sondes en de statuscontrole](application-gateway-probe-overview.md) voor meer informatie.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en een subnet voor de toepassingsgateway maken

Het volgende voorbeeld ziet u hoe u een virtueel netwerk maken met behulp van bronbeheer.

### <a name="step-1"></a>Stap 1

Het adres bereik 10.0.0.0/24 toewijzen aan de variabele subnet moet worden gebruikt om te maken van een virtueel netwerk.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Stap 2

Maak een virtueel netwerk met de naam 'appgwvnet' in resource groep "appgw-rg' voor een West Amerikaans gebied met het voorvoegsel 10.0.0.0/16 met subnet 10.0.0.0/24.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="step-3"></a>Stap 3

Toewijzen aan een variabele subnet voor de volgende stappen die een toepassingsgateway maken.

    $subnet=$vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Maken van een openbare IP-adres van de front-configuratie

Een openbare IP-bron 'publicIP01' in de groep "appgw-rg van de resource" voor de regio West Amerikaans maken.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object"></a>Een application gateway configuratie-object maken

U moet alle configuratie-items instellen voordat u de toepassingsgateway maakt. De volgende stappen uit maken de configuratie-items die u nodig voor een resource van toepassing gateway hebt.

### <a name="step-1"></a>Stap 1

Maak een toepassing gateway IP-configuratie met de naam 'gatewayIP01'. Wanneer de Gateway-toepassing wordt gestart, een IP-adres van het subnet geconfigureerd wordt opgehaald en stuurt netwerk verkeer door naar het IP-adressen in de groep back-enddatabase IP. Houd er rekening mee dat elk exemplaar één IP-adres duurt.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

### <a name="step-2"></a>Stap 2

De groep back-enddatabase IP-adressen met de naam "pool01" met IP-adressen configureren ' 134.170.185.46, 134.170.188.221,134.170.185.50 ". Deze IP-adressen zijn de IP-adressen die het netwerkverkeer die afkomstig zijn uit het front IP-eindpunt ontvangt. U vervangen de voorgaande IP-adressen als u wilt toevoegen van uw eigen toepassing IP-adres eindpunten.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

### <a name="step-3"></a>Stap 3

Toepassing gateway instelling "poolsetting01" configureren voor het netwerkverkeer van taakverdeling in de groep back-enddatabase.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Stap 4

Configureer de front-IP-poort met de naam 'frontendport01' voor het openbare IP-eindpunt.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

### <a name="step-5"></a>Stap 5

Maak de front-IP-configuratie met de naam 'fipconfig01' en het openbare IP-adres koppelen aan de front-IP-configuratie.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-6"></a>Stap 6

Maak de luisteraar ervan af naam 'listener01' en de front-poort naar de front-IP-configuratie koppelen.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-7"></a>Stap 7

Maak de laden verdeling routeren regel met de naam 'rule01' die het gedrag van de verdeling van laden configureert.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-8"></a>Stap 8

De grootte van het exemplaar van de toepassingsgateway configureren.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE]  De standaardwaarde voor *InstanceCount* is 2, met een maximumwaarde van 10. De standaardwaarde voor *GatewaySize* is normaal. U kunt kiezen tussen Standard_Small, Standard_Medium en Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Een toepassingsgateway maken met behulp van nieuw-AzureRmApplicationGateway

Een toepassingsgateway maken met alle items van de configuratie van de voorgaande stappen. In dit voorbeeld wordt de toepassingsgateway 'appgwtest' genoemd.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

### <a name="step-9"></a>Stap 9

DNS- en VIP details van de toepassingsgateway ophalen met de openbare IP-resource is gekoppeld aan de toepassingsgateway.

    Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  

## <a name="delete-an-application-gateway"></a>Verwijderen van een toepassingsgateway

Als u wilt verwijderen van een toepassingsgateway, als volgt te werk:

### <a name="step-1"></a>Stap 1

De toepassing gateway-object en deze te koppelen aan een variabele "$getgw".

    $getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Stap 2

Gebruik **Stoppen-AzureRmApplicationGateway** om te stoppen de toepassingsgateway.

    Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

Als u de toepassingsgateway hebt gestopt status, gebruikt u de cmdlet **Verwijderen-AzureRmApplicationGateway** om de service te verwijderen.

    Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force

>[AZURE.NOTE] De **-afdwingen** veranderen kan worden gebruikt om het bevestigingsbericht verwijderen onderdrukken.

Om te bevestigen dat de service is verwijderd, kunt u de cmdlet **Get-AzureRmApplicationGateway** . Deze stap is niet vereist.

    Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

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

Als u configureren SSL offload wilt, raadpleegt u [configureren de toepassingsgateway van een voor SSL offload](application-gateway-ssl.md).

Als u configureren van een toepassingsgateway wilt voor gebruik met een interne taakverdeling, raadpleegt u [een toepassingsgateway met een interne taakverdeling (ILB) maken](application-gateway-ilb.md).

Als u meer informatie over het laden taakverdeling opties in het algemeen wilt, Zie:

- [Azure taakverdeling](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure verkeer Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
