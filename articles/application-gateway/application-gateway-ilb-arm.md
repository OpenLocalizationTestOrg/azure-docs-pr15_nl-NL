<properties
   pageTitle="Maken en configureren van een toepassingsgateway met een interne taakverdeling (ILB) met behulp van Azure resourcemanager | Microsoft Azure"
   description="Deze pagina bevat instructies voor het maken, configureren, starten en verwijderen van een toepassingsgateway Azure-met interne taakverdeling (ILB) voor Azure resourcemanager"
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
   ms.date="08/19/2016"
   ms.author="gwallace"/>


# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a>Een toepassingsgateway met een interne taakverdeling (ILB) maken met behulp van Azure resourcemanager

> [AZURE.SELECTOR]
- [Azure klassieke stappen](application-gateway-ilb.md)
- [Resourcemanager PowerShell-stappen](application-gateway-ilb-arm.md)

Azure-toepassingsgateway kan worden geconfigureerd met een internetverbinding VIP of met een interne eindpunt dat wordt niet blootgesteld aan Internet, ook bekend als een interne laden verdeling (ILB) eindpunt. De gateway configureren met een ILB is handig voor interne LOB-toepassingen die niet beschikbaar zijn met Internet. Het is ook handig voor services en lagen van een toepassing met meerdere niveaus die gaan zitten in een beveiligd gebied die wordt niet blootgesteld aan Internet, maar nog steeds vereisen round robin laden verdeling, sessie kleverigheid of beëindiging van de Secure Sockets Layer (SSL).

In dit artikel begeleidt u bij de stappen voor het configureren van een toepassingsgateway met een ILB.

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de meest recente versie van de Azure PowerShell-cmdlets met behulp van het installatieprogramma van de Web-Platform. U kunt downloaden en installeren van de meest recente versie van het **Windows PowerShell** -gedeelte van de [pagina Downloads](https://azure.microsoft.com/downloads/).
2. U maakt een virtueel netwerk en een subnet voor de Gateway-toepassing. Zorg ervoor dat er geen virtuele machines of cloud-implementaties het subnet worden gebruikt. Toepassingsgateway moet zich op zichzelf in een virtueel netwerk subnet.
3. De servers die u configureren voor het gebruik van de toepassingsgateway moeten bestaan of de eindpunten ervan hebt gemaakt in het virtuele netwerk of met een openbare IP-/ VIP hebt toegewezen.

## <a name="what-is-required-to-create-an-application-gateway"></a>Wat is vereist voor het maken van een toepassingsgateway?


- **Back-enddatabase server-groep:** De lijst met IP-adressen van de back-end-servers. De IP-adressen vermeld ofwel moeten behoren bij het virtuele netwerk, maar in een ander subnet voor de toepassingsgateway of een openbare IP/VIP moeten worden.
- **Back-enddatabase groep serverinstellingen:** Elke groep heeft instellingen, zoals poort, protocol en cookie gebaseerde affiniteit. Deze instellingen zijn gekoppeld aan een groep en worden toegepast op alle servers in de groep.
- **Front poort:** Deze poort is de openbare poort dat is geopend in de toepassingsgateway. Verkeer raakt deze poort en vervolgens wordt omgeleid naar een van de back-end-servers.
- **Luisteraar ervan af:** De luisteraar ervan af heeft een front poort, een protocol (Http of Https, dit zijn hoofdlettergevoelig), en de SSL-certificaat-naam (als SSL configureren offload).
- **Regel:** De regel de luisteraar ervan af en de back-enddatabase server-groep gekoppeld en wordt gedefinieerd welke back-end-server-groep het verkeer moet worden doorgestuurd naar wanneer deze een bepaalde luisteraar ervan af. Op dit moment wordt alleen de *eenvoudige* regel ondersteund. De *eenvoudige* regel is round robin laden verdeling.



## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Het verschil tussen het gebruik van Azure klassieke en Azure Resource Manager is de volgorde waarin u de toepassingsgateway en de items die moeten worden geconfigureerd maken.
Met resourcemanager, alle items die deel uitmaken van een toepassingsgateway is geconfigureerd afzonderlijk en zet vervolgens samen om te maken van de gateway-resource van toepassing.


Hier volgen de stappen die nodig zijn voor het maken van een toepassingsgateway:

1. Een resourcegroep maken voor bronbeheer
2. Een virtueel netwerk en een subnet voor de toepassingsgateway maken
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

Maak een nieuwe resourcegroep (overslaan deze stap als u een bestaande resourcegroep gebruikt).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure resourcemanager is vereist dat alle resourcegroepen Geef een locatie. Dit wordt gebruikt als de standaardlocatie voor resources in die resourcegroep. Zorg ervoor dat alle opdrachten voor het maken van een toepassingsgateway gebruikmaakt van dezelfde resourcegroep.

In het voorbeeld hierboven, we hebben gemaakt een resourcegroep 'appgw-rg' en de locatie 'West ons' genoemd.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Een virtueel netwerk en een subnet voor de toepassingsgateway maken

Het volgende voorbeeld ziet hoe u een virtueel netwerk maken met behulp van resourcemanager:

### <a name="step-1"></a>Stap 1

    $subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

Hiermee wordt het adres bereik 10.0.0.0/24 toegewezen aan een variabele subnet moet worden gebruikt om te maken van een virtueel netwerk.

### <a name="step-2"></a>Stap 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig

Hiermee maakt u een virtueel netwerk met de naam 'appgwvnet' in resource groep "appgw-rg' voor een West Amerikaans gebied met het voorvoegsel 10.0.0.0/16 met subnet 10.0.0.0/24.

### <a name="step-3"></a>Stap 3

    $subnet = $vnet.subnets[0]

Hiermee wordt het subnetobject toegewezen aan variabele $subnet voor de volgende stappen.

## <a name="create-an-application-gateway-configuration-object"></a>Een application gateway configuratie-object maken

### <a name="step-1"></a>Stap 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Hiermee maakt u een toepassing gateway IP-configuratie met de naam 'gatewayIP01'. Wanneer toepassingsgateway wordt gestart, een IP-adres van het subnet geconfigureerd wordt opgehaald en netwerkverkeer doorsturen naar het IP-adressen in de groep van het IP-back-enddatabase. Houd er rekening mee dat elk exemplaar één IP-adres duurt.


### <a name="step-2"></a>Stap 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Hiermee wordt de back-enddatabase IP-adres van toepassingen met de naam "pool01" met IP-adressen "134.170.185.46, 134.170.188.221,134.170.185.50". Dit zijn de IP-adressen die het netwerkverkeer die afkomstig zijn uit het front IP-eindpunt ontvangt. U vervangen de bovenstaande om toe te voegen van uw eigen toepassing IP-adres eindpunten IP-adressen.

### <a name="step-3"></a>Stap 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

Hiermee wordt toepassing gateway instelling "poolsetting01" voor het selectievakje laden verdeeld netwerkverkeer in de groep back-enddatabase.

### <a name="step-4"></a>Stap 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

Hiermee wordt de front-IP-poort met de naam 'frontendport01' voor de ILB.

### <a name="step-5"></a>Stap 5

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet

Hiermee maakt de front-IP-configuratie 'fipconfig01' genoemd en koppelt aan een privé IP-adres van de huidige virtuele netwerk subnet.

### <a name="step-6"></a>Stap 6

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

Hiermee wordt gemaakt van de luisteraar ervan af 'listener01' genoemd en de front-poort naar de front-IP-configuratie koppelt.

### <a name="step-7"></a>Stap 7

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Hiermee maakt u het laden verdeling doorstuurregel genoemd 'rule01' die het gedrag van de verdeling van laden configureert.

### <a name="step-8"></a>Stap 8

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Hiermee wordt de grootte van het exemplaar van de toepassingsgateway.

>[AZURE.NOTE]  De standaardwaarde voor *InstanceCount* is 2, met een maximumwaarde van 10. De standaardwaarde voor *GatewaySize* is normaal. U kunt kiezen tussen Standard_Small, Standard_Medium en Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Een toepassingsgateway maken met behulp van nieuw-AzureApplicationGateway

Hiermee maakt u een toepassingsgateway met alle items van de configuratie van de bovenstaande stappen. In dit voorbeeld wordt de toepassingsgateway 'appgwtest' genoemd.


    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

Hiermee wordt een application gateway gemaakt met alle items van de configuratie van de bovenstaande stappen. In het voorbeeld wordt de toepassingsgateway 'appgwtest' genoemd.


## <a name="delete-an-application-gateway"></a>Verwijderen van een toepassingsgateway

Als u wilt verwijderen van een toepassingsgateway, moet u in de volgorde het volgende doen:

1. Gebruik de cmdlet **Stop-AzureRmApplicationGateway** om te stoppen de gateway.
2. Gebruik de cmdlet **Verwijderen-AzureRmApplicationGateway** om het verwijderen van de gateway.
3. Controleer of dat de gateway is verwijderd door met de cmdlet **Get-AzureApplicationGateway** .


### <a name="step-1"></a>Stap 1

De toepassing gateway-object en deze te koppelen aan een variabele "$getgw".

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Stap 2

Gebruik **Stoppen-AzureRmApplicationGateway** om te stoppen de toepassingsgateway. Dit voorbeeld ziet u de cmdlet **Stop-AzureRmApplicationGateway** op de eerste regel, gevolgd door de uitvoer.

    PS C:\> Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Als u de toepassingsgateway hebt gestopt status, gebruikt u de cmdlet **Verwijderen-AzureRmApplicationGateway** om de service te verwijderen.


    PS C:\> Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

>[AZURE.NOTE] De **-afdwingen** veranderen kan worden gebruikt om het bevestigingsbericht verwijderen onderdrukken.


Om te bevestigen dat de service is verwijderd, kunt u de cmdlet **Get-AzureRmApplicationGateway** . Deze stap is niet vereist.


    PS C:\>Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Volgende stappen

Als u configureren SSL offload wilt, raadpleegt u [configureren de toepassingsgateway van een voor SSL offload](application-gateway-ssl.md).

Als u configureren van een toepassingsgateway wilt voor gebruik met een ILB, raadpleegt u [een toepassingsgateway met een interne taakverdeling (ILB) maken](application-gateway-ilb.md).

Als u meer informatie over het laden taakverdeling opties in het algemeen wilt, Zie:

- [Azure taakverdeling](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure verkeer Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
