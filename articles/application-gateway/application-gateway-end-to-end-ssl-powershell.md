<properties
    pageTitle="SSL-beleid en complete SSL configureren met toepassingsgateway | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe complete SSL configureren met toepassingsgateway via Azure resourcemanager PowerShell"
    services="application-gateway"
    documentationCenter="na"
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

# <a name="configure-ssl-policy-and-end-to-end-ssl-with-application-gateway-using-powershell"></a>SSL-beleid en complete SSL configureren met toepassingsgateway via PowerShell

## <a name="overview"></a>Overzicht

Toepassingsgateway ondersteunt end-to-end-versleuteling verkeer. Toepassingsgateway wordt de verbinding wordt verbroken SSL in de toepassingsgateway. De gateway vervolgens regels voor de routering is van toepassing op het verkeer, opnieuw worden gecodeerd het pakket en de doorgestuurd naar de juiste backend op basis van de regels voor de routering gedefinieerd. Geen antwoord van de webserver doorloopt hetzelfde proces terug naar de eindgebruiker.

Een andere functie die toepassingsgateway ondersteunt schakelt bepaalde versies SSL-protocol. Toepassingsgateway ondersteunt de versie van het volgende protocol; uitschakelen TLSv1.0, TLSv1.1 en TLSv1.2.

> [AZURE.NOTE] SSL 2.0 en SSL 3.0 zijn standaard uitgeschakeld en kan niet worden ingeschakeld. Ze worden beschouwd als onbeveiligde en kunnen niet worden gebruikt met de Gateway-toepassing

![scenario-afbeelding][scenario]

## <a name="scenario"></a>Scenario

In dit scenario leert u hoe u een toepassingsgateway via complete SSL via PowerShell maakt.

Dit scenario wordt:

- Een resourcegroep met de naam "appgw-rg" maken
- Maak een virtueel netwerk met de naam "appgwvnet" met een gereserveerde CIDR tekstblok 10.0.0.0/16.
- Maak twee subnetten 'appgwsubnet' en 'appsubnet' genoemd.
- Maak een kleine toepassingsgateway ondersteunende complete SSL-versleuteling die bepaalde SSL-protocollen worden uitgeschakeld.

## <a name="before-you-begin"></a>Voordat u begint

Als u wilt configureren complete SSL met een application gateway, een certificaat is vereist voor de gateway en certificaten zijn vereist voor de backend-servers. Het gatewaycertificaat wordt gebruikt om te versleutelen en ontsleutelen het verkeer naar deze via SSL verzonden. Het gatewaycertificaat moet in de indeling Personal Information Exchange (pfx). Deze bestandsindeling kunt voor de persoonlijke sleutel wilt exporteren die door de toepassingsgateway is vereist voor het coderen en decoderen verkeer uitvoeren.

Voor complete ssl-versleuteling moet de backend whitelisted met toepassingsgateway. Hiermee wordt uitgevoerd door de openbare certificaat van de backends uploaden naar de toepassingsgateway. Dit zorgt ervoor dat de toepassingsgateway alleen met bekende backend exemplaren communiceert. Hiermee beveiligt verder de end-to-end-communicatie.

Dit proces wordt beschreven in de volgende stappen uit:

## <a name="create-the-resource-group"></a>De resourcegroep maken

In dit gedeelte begeleidt u bij het maken van een resourcegroep, waarin de toepassingsgateway.

### <a name="step-1"></a>Stap 1

Meld u aan bij uw Azure-Account.

    Login-AzureRmAccount

### <a name="step-2"></a>Stap 2

Selecteer het abonnement wilt gebruiken voor dit scenario.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Stap 3

Maak een resourcegroep (overslaan deze stap als u een bestaande resourcegroep gebruikt).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en een subnet voor de toepassingsgateway maken

Het volgende voorbeeld wordt een virtueel netwerk en twee subnetten. Één subnet wordt gebruikt om de toepassingsgateway. Het andere subnet wordt gebruikt voor het hosten van de webtoepassing backends.

### <a name="step-1"></a>Stap 1

Een adresbereiken toewijzen voor het subnet worden gebruikt voor de Gateway-toepassing zelf.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Subnetten die zijn geconfigureerd voor de toepassingsgateway moeten behoren formaat worden gewijzigd. Een toepassingsgateway kan worden geconfigureerd voor maximaal 10 exemplaren. Elk exemplaar gaat 1 IP-adres van het subnet. Te klein van een subnet, kan de schaal van een toepassingsgateway nadelig beïnvloeden.

### <a name="step-2"></a>Stap 2

Een adresbereik moet worden gebruikt voor de groep Backend-adressen toewijzen.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-3"></a>Stap 3

Maak een virtueel netwerk met de subnetten die is gedefinieerd in de voorgaande stappen.

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-4"></a>Stap 4

Ophalen van de resource van virtueel netwerk en subnetresources moet worden gebruikt in de volgende stappen:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Maken van een openbare IP-adres van de front-configuratie

Een openbare IP-bron moet worden gebruikt voor de toepassingsgateway maken. Dit openbare IP-adres wordt gebruikt een volgende stap.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Toepassingsgateway biedt geen ondersteuning voor het gebruik van een openbare IP-adres dat is gemaakt met een domeinlabel gedefinieerd. Alleen een openbare IP-adres met een domeinlabel dynamisch gemaakte wordt ondersteund. Als u een beschrijvende DNS-naam voor de toepassingsgateway vereist, is het raadzaam te gebruiken van een cname-record als een alias.

## <a name="create-an-application-gateway-configuration-object"></a>Een application gateway configuratie-object maken

U moet alle configuratie-items instellen voordat u de toepassingsgateway maakt. De volgende stappen uit maken de configuratie-items die u nodig voor een resource van toepassing gateway hebt.

### <a name="step-1"></a>Stap 1

Maken van een toepassing gateway IP-configuratie, deze instelling configureert welke subnet de toepassingsgateway wordt gebruikt. Wanneer toepassingsgateway wordt gestart, een IP-adres van het subnet geconfigureerd wordt opgehaald en stuurt netwerk verkeer door naar het IP-adressen in de groep van het IP-back-enddatabase. Houd er rekening mee dat elk exemplaar één IP-adres duurt.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-2"></a>Stap 2

Maken van een front IP-configuratie, deze instelling wordt een persoonlijke of openbare IP-adres toegewezen aan de front-end van de toepassingsgateway. De volgende stap het openbare IP-adres in de vorige stap gekoppeld aan de front-IP-configuratie.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-3"></a>Stap 3

De back-enddatabase IP-adresgroep configureren met het IP-adressen van de backend-endwebservers. Deze IP-adressen zijn de IP-adressen die het netwerkverkeer die afkomstig zijn uit het front IP-eindpunt ontvangt. U vervangen de volgende IP-adressen als u wilt toevoegen van uw eigen toepassing IP-adres eindpunten.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

> [AZURE.NOTE] Een volledig gekwalificeerde domeinnaam (FQDN) is ook een ongeldige waarde in plaats van een IP-adres voor de backend-servers met de schakeloptie - BackendFqdns.

### <a name="step-4"></a>Stap 4

Configureer de front-IP-poort voor het openbare IP-eindpunt. Dit wordt de poort die eindgebruikers verbinding met maken.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-5"></a>Stap 5

Het certificaat voor de toepassingsgateway configureren. Dit certificaat is gebruikt u ontsleutelt en het verkeer op de toepassingsgateway opnieuw te versleutelen.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

> [AZURE.NOTE] In dit voorbeeld configureert het certificaat voor SSL-verbinding. Het certificaat moet in .pfx-indeling en het wachtwoord moet tussen tekens met 4 t/m 12.

### <a name="step-6"></a>Stap 6

Maak de HTTP luisteraar ervan af voor de toepassingsgateway. Toewijzen aan de front-IP-configuratie, poorten en ssl-certificaat wilt gebruiken.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-7"></a>Stap 7

Upload het certificaat moet worden gebruikt op de resources in de backend ssl is ingeschakeld.

> [AZURE.NOTE] De standaard-test ontvangt van de openbare sleutel van de **standaard** -SSL-binding op IP-adres van de back-enddatabase en verschilt van de openbare sleutelwaarde die deze naar de openbare sleutelwaarde die u hier opgeeft, ontvangt. De opgehaalde openbare sleutel is mogelijk niet per se de gewenste site waarop verkeer doorloopt **als** u host kop- en SNI gebruikt in de back-enddatabase. Als u twijfelt, bezoekt u https://127.0.0.1/ op de back-uiteinden om te bevestigen welke certificaat wordt gebruikt om de **standaard** -SSL-binding. Gebruik de openbare sleutel aangevraagde die in deze sectie. Als u host-kop- en SNI op HTTPS bindingen gebruikt en niet u een reactie en het certificaat van een aanvraag voor een handmatige browser naar https://127.0.0.1/ op de back-einden ontvangt, moet u een standaard-SSL-binding op de back-einden instellen. Als u niet doet dit, worden sondes, mislukt en wordt het back-enddatabasebestand niet whitelisted.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer

> [AZURE.NOTE] Het certificaat dat is opgegeven in deze stap moet de openbare sleutel van het certificaat pfx op de backend aanwezig zijn. Exporteer het certificaat (niet de hoofdmap certificaat) geïnstalleerd op de backend-server in. CER opmaken en in deze stap te gebruiken. Deze stap whitelists de backend met de toepassingsgateway.

### <a name="step-8"></a>Stap 8

De toepassing gateway back-enddatabase http-instellingen configureren. Toewijzen aan het certificaat in de vorige stap in de http-instellingen hebt geüpload.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-9"></a>Stap 9

Een laden verdeling routeren regel maken die het gedrag van de verdeling van laden configureert. In dit voorbeeld wordt een eenvoudige round robin-regel gemaakt.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-10"></a>Stap 10

De grootte van het exemplaar van de toepassingsgateway configureren.  De beschikbare formaten zijn **standaard\_kleine**, **standaard\_gemiddeld**, en **standaard\_groot**.  De beschikbare waarden zijn voor capaciteit, 1 tot en met 10.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE] Een aantal exemplaar van 1 u kunt kiezen voor testdoeleinden. Het is belangrijk te weten dat een exemplaar tellen onder twee instanties valt buiten de SLA en worden daarom niet aanbevolen. Kleine gateways moeten worden gebruikt voor ontwikkelaar test en niet voor productiedoeleinden.

### <a name="step-11"></a>Stap 11

Configureer het beleid SSL in de Gateway-toepassing moet worden gebruikt. Toepassingsgateway ondersteunt de mogelijkheid voor het uitschakelen van bepaalde versies SSL-protocol.

De volgende waarden zijn in een lijst met protocol-versies die kunnen worden uitgeschakeld.

- **TLSv1_0**
- **TLSv1_1**
- **TLSv1_2**

Het volgende voorbeeld wordt uitgeschakeld TLSv1\_0.

    $sslPolicy = New-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0

## <a name="create-the-application-gateway"></a>De toepassingsgateway maken

Met de voorgaande stappen, maakt u de Gateway-toepassing. Het maken van de gateway is een langdurige proces.

    $appgw = New-AzureRmApplicationGateway -Name appgateway -SslCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslPolicy $sslPolicy -AuthenticationCertificates $authcert -Verbose

## <a name="disable-ssl-protocol-versions-on-an-existing-application-gateway"></a>SSL-protocol versies op een bestaande Application Gateway uitschakelen

De voorgaande stappen gaat u door een toepassing maken met complete ssl- en uitschakelen van bepaalde versies SSL-protocol. Het volgende voorbeeld wordt de beleidsregels van bepaalde SSL op een bestaande application gateway.

### <a name="step-1"></a>Stap 1

Hiermee kunt u de toepassingsgateway bijwerken ophalen.

    $gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG

### <a name="step-2"></a>Stap 2

Een SSL-beleid definiëren. In het volgende voorbeeld, zijn TLSv1.0 en TLSv1.1 uitgeschakeld.

    Set-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0, TLSv1_1 -ApplicationGateway $gw

### <a name="step-3"></a>Stap 3

Tot slot werk de gateway. Het is belangrijk te weten dat deze laatste stap een langdurige taak is. Wanneer deze klaar is, wordt complete ssl is geconfigureerd op de toepassingsgateway.

    $gw | Set-AzureRmApplicationGateway

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

Meer informatie over de beveiliging van uw webtoepassingen met Web Application Firewall via toepassingsgateway versterken via [Webtoepassingen Firewall-overzicht](application-gateway-webapplicationfirewall-overview.md)

[scenario]: ./media/application-gateway-end-to-end-ssl-powershell/scenario.png
