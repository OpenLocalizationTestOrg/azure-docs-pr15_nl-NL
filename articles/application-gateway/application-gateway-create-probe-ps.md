<properties
   pageTitle="Een aangepaste test maken voor de Gateway-toepassing via PowerShell in resourcemanager | Microsoft Azure"
   description="Informatie over het maken van een aangepaste test voor de Gateway-toepassing via PowerShell in resourcemanager"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a>Een aangepaste test voor Azure-toepassingsgateway via PowerShell voor Azure resourcemanager maken

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-probe-portal.md)
- [Azure resourcemanager PowerShell](application-gateway-create-probe-ps.md)
- [Azure klassieke PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassieke implementatiemodel](application-gateway-create-probe-classic-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

### <a name="step-1"></a>Stap 1

Login-AzureRmAccount gebruiken om te verifiëren.

    Login-AzureRmAccount

### <a name="step-2"></a>Stap 2

Controleer de abonnementen voor het account.

    Get-AzureRmSubscription

### <a name="step-3"></a>Stap 3

Kies welke van uw Azure-abonnementen te gebruiken. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Stap 4

Maak een resourcegroep (overslaan deze stap als u een bestaande resourcegroep gebruikt).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure resourcemanager is vereist dat alle resourcegroepen Geef een locatie. Deze locatie wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Zorg ervoor dat alle opdrachten voor het maken van een toepassingsgateway dezelfde resourcegroep gebruiken.

In het voorbeeld hierboven, we hebben gemaakt een resourcegroep 'appgw-RG' en de locatie 'West ons' genoemd.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en een subnet voor de toepassingsgateway maken

De volgende stappen uit maken een virtueel netwerk en een subnet voor de toepassingsgateway.

### <a name="step-1"></a>Stap 1


Het adres bereik 10.0.0.0/24 toewijzen aan een variabele subnet moet worden gebruikt om te maken van een virtueel netwerk.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Stap 2

Maak een virtueel netwerk met de naam 'appgwvnet' in resource groep "appgw-rg' voor een West Amerikaans gebied met het voorvoegsel 10.0.0.0/16 met subnet 10.0.0.0/24.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### <a name="step-3"></a>Stap 3

Toewijzen aan een variabele subnet voor de volgende stappen die een toepassingsgateway maken.

    $subnet = $vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Maken van een openbare IP-adres van de front-configuratie


Een openbare IP-bron 'publicIP01' in de groep "appgw-rg van de resource" voor de regio West Amerikaans maken.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object-with-a-custom-probe"></a>Een application gateway configuratie-object maken met een aangepaste-test

U instellen alle items van de configuratie voordat u de toepassingsgateway maakt. De volgende stappen uit maken de configuratie-items die u nodig voor een resource van toepassing gateway hebt.

### <a name="step-1"></a>Stap 1

Maak een toepassing gateway IP-configuratie met de naam 'gatewayIP01'. Wanneer toepassingsgateway wordt gestart, een IP-adres van het subnet geconfigureerd wordt opgehaald en netwerkverkeer doorsturen naar het IP-adressen in de groep van het IP-back-enddatabase. Houd er rekening mee dat elk exemplaar één IP-adres duurt.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### <a name="step-2"></a>Stap 2


De groep back-enddatabase IP-adressen met de naam "pool01" met IP-adressen configureren ' 134.170.185.46, 134.170.188.221,134.170.185.50 ". Deze waarden zijn de IP-adressen die het netwerkverkeer die afkomstig zijn uit het front IP-eindpunt ontvangt. U vervangen de bovenstaande om toe te voegen van uw eigen toepassing IP-adres eindpunten IP-adressen.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50


### <a name="step-3"></a>Stap 3


De aangepaste-test is geconfigureerd in deze stap.

De parameters zijn:

- **Interval** - configureert test interval controles in seconden.
- **Time-out** - Hiermee definieert u de test time-out voor een selectievakje HTTP-antwoord.
- **Hostname- en pad** - volledige URL-pad dat wordt aangeroepen door de Gateway-toepassing om te bepalen de status van het exemplaar. Bijvoorbeeld, hebt u een website http://contoso.com/, worden kan de aangepaste test geconfigureerd voor "http://contoso.com/path/custompath.htm" voor test controles moet een positief HTTP-antwoord.
- **UnhealthyThreshold** - het aantal mislukte HTTP-antwoorden nodig moeten worden gemarkeerd het exemplaar van de back-enddatabase als *beschadigd*.

<BR>

    $probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/path.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8

### <a name="step-4"></a>Stap 4

Toepassing gateway instelling "poolsetting01" voor het verkeer configureren in de groep back-enddatabase. Deze stap, heeft ook een time-configuratie die is bedoeld voor het back-enddatabase groep antwoord op de opdracht van een toepassing gateway. Wanneer u een back-enddatabase antwoord raakt een time-out optreedt, annuleert toepassingsgateway het verzoek. Deze waarde verschilt van een test time-out die geldt alleen voor het back-enddatabase-antwoord op controles onderzoeken.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

### <a name="step-5"></a>Stap 5

Configureer de front-IP-poort met de naam 'frontendport01' voor het openbare IP-eindpunt.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

### <a name="step-6"></a>Stap 6

Maak de front-IP-configuratie met de naam 'fipconfig01' en het openbare IP-adres koppelen aan de front-IP-configuratie.


    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-7"></a>Stap 7

Maak de luisteraar ervan af naam 'listener01' en de front-poort naar de front-IP-configuratie koppelen.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-8"></a>Stap 8

Maak de laden verdeling routeren regel met de naam 'rule01' die het gedrag van de verdeling van laden configureert.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-9"></a>Stap 9

De grootte van het exemplaar van de toepassingsgateway configureren.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2


>[AZURE.NOTE]  De standaardwaarde voor *InstanceCount* is 2, met een maximumwaarde van 10. De standaardwaarde voor *GatewaySize* is normaal. U kunt kiezen tussen Standard_Small, Standard_Medium en Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Een toepassingsgateway maken met behulp van nieuw-AzureRmApplicationGateway

Een toepassingsgateway maken met alle items van de configuratie van de bovenstaande stappen. In dit voorbeeld wordt de toepassingsgateway 'appgwtest' genoemd.

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

## <a name="add-a-probe-to-an-existing-application-gateway"></a>Een test toevoegen aan een bestaande application gateway

Er zijn vier stappen voor het toevoegen van een aangepaste test aan een bestaande application gateway.

### <a name="step-1"></a>Stap 1

De bron van de gateway toepassing laden in een PowerShell-variabele met behulp van **Get-AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Stap 2

Een test toevoegen aan de bestaande gatewayconfiguratie van de.

    $getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/custompath.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8


In het voorbeeld worden de aangepaste-test is geconfigureerd voor het controleren op URL-pad contoso.com/path/custompath.htm elke 30 seconden. Een time-drempelwaarde voor 120 seconden is geconfigureerd met een maximum aantal 8 mislukte test aanvragen.

### <a name="step-3"></a>Stap 3

De test de configuratie van de back-enddatabase groep-instelling en time-out toevoegen met behulp van **-instellen-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

### <a name="step-4"></a>Stap 4

Sla de configuratie op de toepassingsgateway met behulp van de **Set-AzureRmApplicationGateway**.

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="remove-a-probe-from-an-existing-application-gateway"></a>Een test uit een bestaande application gateway verwijderen

Hier vindt u de stappen voor het verwijderen van een aangepaste test uit een bestaande application gateway.

### <a name="step-1"></a>Stap 1

De bron van de gateway toepassing laden in een PowerShell-variabele met behulp van **Get-AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg


### <a name="step-2"></a>Stap 2

De configuratie test verwijderen uit de toepassingsgateway met behulp van **Verwijderen-AzureRmApplicationGatewayProbeConfig**.

    $getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

### <a name="step-3"></a>Stap 3

De instelling van de back-enddatabase toepassingen verwijderen van de test en time-out-instelling bijwerken met behulp van **-instellen-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Stap 4

Sla de configuratie op de toepassingsgateway met behulp van de **Set-AzureRmApplicationGateway**. 

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

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

Leer hoe u SSL offloading door een bezoek [SSL Offload configureren](application-gateway-ssl-arm.md) te configureren

