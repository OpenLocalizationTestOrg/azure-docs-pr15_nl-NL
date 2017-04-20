<properties
   pageTitle="Maken, start of verwijderen van een toepassingsgateway | Microsoft Azure"
   description="Deze pagina bevat instructies voor het maken, configureren, starten en verwijderen van een toepassingsgateway Azure"
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

# <a name="create-start-or-delete-an-application-gateway"></a>Maken, start of een toepassingsgateway verwijderen

> [AZURE.SELECTOR]
- [Azure-Portal](application-gateway-create-gateway-portal.md)
- [Azure resourcemanager PowerShell](application-gateway-create-gateway-arm.md)
- [Azure klassieke PowerShell](application-gateway-create-gateway.md)
- [Azure resourcemanager-sjabloon](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure-toepassingsgateway is een laag tot en met 7 taakverdeling. Deze biedt failover, performance-routing HTTP-aanvragen tussen verschillende servers, ongeacht of deze op de cloud of on-premises implementatie zijn. Toepassingsgateway bevat veel toepassing bezorging Controller (ADC) functies zoals HTTP van taakverdeling, cookie gebaseerde sessie affiniteit, Secure Sockets Layer (SSL) offload, aangepaste status sondes, ondersteuning voor multi-site en dergelijke. Ga voor een volledige lijst met ondersteunde functies naar [Toepassing Gateway-overzicht](application-gateway-introduction.md)

In dit artikel begeleidt u bij de stappen voor het maken, configureren, starten en verwijderen van een toepassingsgateway.

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de meest recente versie van de Azure PowerShell-cmdlets met behulp van het installatieprogramma van de Web-Platform. U kunt downloaden en installeren van de meest recente versie van het **Windows PowerShell** -gedeelte van de [pagina Downloads](https://azure.microsoft.com/downloads/).
2. Als u een bestaand virtuele netwerk hebt, selecteert u een bestaand lege subnet of een nieuwe subnet maken in uw bestaande virtuele netwerk uitsluitend voor gebruik door de toepassingsgateway. U kunt de toepassingsgateway bij naar een ander virtuele netwerk dan de bronnen die u implementeren achter de toepassingsgateway wilt tenzij vnet peering wordt gebruikt niet implementeren. Ga voor meer informatie naar [Vnet Peering](../virtual-network/virtual-network-peering-overview.md)
3. Controleer of u een virtueel netwerk werkt met een geldige subnet. Zorg ervoor dat er geen virtuele machines of cloud-implementaties het subnet worden gebruikt. De toepassingsgateway moet op zichzelf in een virtueel netwerk subnet.
3. De servers die u configureren voor het gebruik van de toepassingsgateway moeten bestaan of de eindpunten ervan hebt gemaakt in het virtuele netwerk of met een openbare IP-/ VIP hebt toegewezen.

## <a name="what-is-required-to-create-an-application-gateway"></a>Wat is vereist voor het maken van een toepassingsgateway?

Wanneer u de opdracht **Nieuw AzureApplicationGateway** gebruikt voor het maken van de toepassingsgateway, geen configuratie is ingesteld op op dit punt en de zojuist gemaakte bron zijn geconfigureerd met XML- of een configuratieobject.

De waarden zijn:

- **Back-enddatabase server-groep:** De lijst met IP-adressen van de back-end-servers. De IP-adressen vermeld ofwel moeten behoren tot de virtuele netwerk subnet of een openbare IP/VIP moeten worden.
- **Back-enddatabase groep serverinstellingen:** Elke groep heeft instellingen, zoals poort, protocol en cookie gebaseerde affiniteit. Deze instellingen zijn gekoppeld aan een groep en worden toegepast op alle servers in de groep.
- **Front poort:** Deze poort is de openbare poort dat is geopend in de toepassingsgateway. Verkeer raakt deze poort en vervolgens wordt omgeleid naar een van de back-end-servers.
- **Luisteraar ervan af:** De luisteraar ervan af heeft een front poort, een protocol (Http of Https, deze waarden zijn hoofdlettergevoelig), en de SSL-certificaat-naam (als SSL configureren offload).
- **Regel:** De regel de luisteraar ervan af en de back-enddatabase server-groep gekoppeld en wordt gedefinieerd welke back-end-server-groep het verkeer moet worden doorgestuurd naar wanneer deze een bepaalde luisteraar ervan af.

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Een toepassingsgateway maken:

1. Hiermee maakt u een bron van de gateway toepassing.
2. Maak een XML-bestand of een configuratieobject.
3. De configuratie aan iedere resource van de gateway nieuwe toepassing doorvoeren.

>[AZURE.NOTE] Als u nodig hebt voor het configureren van een aangepaste test voor uw toepassingsgateway, raadpleegt u [een toepassingsgateway met aangepaste sondes via PowerShell maken](application-gateway-create-probe-classic-ps.md). Bekijk de [aangepaste sondes en de statuscontrole](application-gateway-probe-overview.md) voor meer informatie.

![Scenario-voorbeeld][scenario]

### <a name="create-an-application-gateway-resource"></a>Een bron van de gateway toepassing maken

Als u wilt de gateway hebt gemaakt, gebruikt u de cmdlet **New-AzureApplicationGateway** , de waarden vervangen door uw eigen. Facturering voor de gateway niet op dit moment kan worden gestart. Facturering wordt gestart in een latere stap, wanneer de gateway is gestart.

Het volgende voorbeeld wordt een application gateway gemaakt met behulp van een virtueel netwerk genoemd 'testvnet1' en een subnet 'subnet-1' genoemd.


    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 *Beschrijving*, *InstanceCount*en *GatewaySize* zijn optionele parameters.

Om te valideren dat de gateway is gemaakt, kunt u de cmdlet **Get-AzureApplicationGateway** .

    Get-AzureApplicationGateway AppGwTest
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Stopped
    VirtualIPs    : {}
    DnsName       :

>[AZURE.NOTE]  De standaardwaarde voor *InstanceCount* is 2, met een maximumwaarde van 10. De standaardwaarde voor *GatewaySize* is normaal. U kunt kiezen tussen klein, gemiddeld en groot.

*VirtualIPs* en *DNS-naam* worden weergegeven als lege omdat de gateway nog niet is gestart. Deze worden gemaakt wanneer de gateway actief is.

## <a name="configure-the-application-gateway"></a>De toepassingsgateway configureren

U kunt de toepassingsgateway configureren met behulp van XML- of een configuratieobject.

## <a name="configure-the-application-gateway-by-using-xml"></a>De toepassingsgateway configureren met behulp van XML

In het volgende voorbeeld, kunt u een XML-bestand gebruiken voor alle toepassing gateway-instellingen configureren en ze aan iedere resource van toepassing gateway hebt toegewezen.  

### <a name="step-1"></a>Stap 1  

Kopieer de volgende tekst naar Kladblok.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>(name-of-your-frontend-port)</Name>
                <Port>(port number)</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>(name-of-your-backend-pool)</Name>
                <IPAddresses>
                    <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                    <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>(backend-setting-name-to-configure-rule)</Name>
                <Port>80</Port>
                <Protocol>[Http|Https]</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>(name-of-the-listener)</Name>
                <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
                <Protocol>[Http|Https]</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>(name-of-load-balancing-rule)</Name>
                <Type>basic</Type>
                <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
                <Listener>(name-of-the-listener)</Listener>
                <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>

Bewerk de waarden tussen de haakjes voor de configuratie-items. Sla het bestand met de extensie .xml.

>[AZURE.IMPORTANT] Het item protocol Http of Https is hoofdlettergevoelig.

Het volgende voorbeeld ziet u hoe u een configuratiebestand gebruiken voor het instellen van de toepassingsgateway. De belasting voorbeeld saldi HTTP-verkeer is toegestaan op openbare poort 80 en netwerkverkeer verzendt naar de back-enddatabase poort 80 tussen twee IP-adressen.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>80</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>BackendPool1</Name>
                <IPAddresses>
                    <IPAddress>10.0.0.1</IPAddress>
                    <IPAddress>10.0.0.2</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>BackendSetting1</Name>
                <Port>80</Port>
                <Protocol>Http</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>HTTPListener1</Name>
                <FrontendPort>FrontendPort1</FrontendPort>
                <Protocol>Http</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>HttpLBRule1</Name>
                <Type>basic</Type>
                <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
                <Listener>HTTPListener1</Listener>
                <BackendAddressPool>BackendPool1</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


### <a name="step-2"></a>Stap 2

Vervolgens stelt u de toepassingsgateway. Gebruik de cmdlet **Set-AzureApplicationGatewayConfig** met een XML-bestand.


    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="configure-the-application-gateway-by-using-a-configuration-object"></a>De toepassingsgateway configureren met behulp van een configuratieobject

Het volgende voorbeeld ziet u hoe u de toepassingsgateway configureert met behulp van de van configuratieobjecten. Alle configuratie-items moeten worden afzonderlijk geconfigureerd en vervolgens worden toegevoegd aan een toepassing gateway configuratie-object. Na het maken van het configuratieobject, kunt u de opdracht **Set-AzureApplicationGateway** gebruiken om door te voeren van de configuratie aan iedere resource van de gateway eerder gemaakte toepassing.

>[AZURE.NOTE] Voordat u een waarde toewijst aan elk configuratieobject, moet u welk type object PowerShell gebruikt voor opslag declareren. De eerste regel te maken van de afzonderlijke items wordt gedefinieerd welke Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model (objectnaam) worden gebruikt.

### <a name="step-1"></a>Stap 1

Maak alle afzonderlijke configuratie-items.

De front IP maken zoals weergegeven in het volgende voorbeeld.

    $fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
    $fip.Name = "fip1"
    $fip.Type = "Private"
    $fip.StaticIPAddress = "10.0.0.5"

De front-poort maken zoals weergegeven in het volgende voorbeeld.

    $fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
    $fep.Name = "fep1"
    $fep.Port = 80

Maak de back-enddatabase server-groep.

 Definieer het IP-adressen die worden toegevoegd aan de back-end-server-toepassingen zoals wordt weergegeven in het volgende voorbeeld.


    $servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
    $servers.Add("10.0.0.1")
    $servers.Add("10.0.0.2")

 Gebruik het object $server de waarden toevoegen aan het back-enddatabase groep object ($pool).

    $pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
    $pool.BackendServers = $servers
    $pool.Name = "pool1"

Maak de instelling van de groep back-end-server.

    $setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
    $setting.Name = "setting1"
    $setting.CookieBasedAffinity = "enabled"
    $setting.Port = 80
    $setting.Protocol = "http"

Maak de luisteraar ervan af.

    $listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
    $listener.Name = "listener1"
    $listener.FrontendPort = "fep1"
    $listener.FrontendIP = "fip1"
    $listener.Protocol = "http"
    $listener.SslCert = ""

Maak de regel.

    $rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
    $rule.Name = "rule1"
    $rule.Type = "basic"
    $rule.BackendHttpSettings = "setting1"
    $rule.Listener = "listener1"
    $rule.BackendAddressPool = "pool1"

### <a name="step-2"></a>Stap 2

Alle items voor afzonderlijke configuratie toewijzen aan een toepassing gateway configuratieobject ($appgwconfig).

De front IP toevoegen aan de configuratie.

    $appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
    $appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
    $appgwconfig.FrontendIPConfigurations.Add($fip)

De front-poort toevoegen in de configuratie.

    $appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
    $appgwconfig.FrontendPorts.Add($fep)

De back-enddatabase server-groep toevoegen aan de configuratie.

    $appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
    $appgwconfig.BackendAddressPools.Add($pool)

De instelling van de back-enddatabase groep toevoegen aan de configuratie.

    $appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
    $appgwconfig.BackendHttpSettingsList.Add($setting)

De luisteraar ervan af toevoegen aan de configuratie.

    $appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
    $appgwconfig.HttpListeners.Add($listener)

De regel toevoegen aan de configuratie.

    $appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
    $appgwconfig.HttpLoadBalancingRules.Add($rule)

### <a name="step-3"></a>Stap 3

De configuratieobject aan iedere resource van toepassing gateway doorvoeren met behulp van de **Set-AzureApplicationGatewayConfig**.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## <a name="start-the-gateway"></a>De gateway starten

Wanneer de gateway is geconfigureerd, gebruikt u de cmdlet **Start-AzureApplicationGateway** starten van de gateway. Facturering voor de toepassingsgateway van een gestart nadat de gateway is gestart.

> [AZURE.NOTE] De **Begin-AzureApplicationGateway** -cmdlet duurt maximaal 15-20 minuten om te voltooien.

    Start-AzureApplicationGateway AppGwTest

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Controleer of de status van gateway

Gebruik de cmdlet **Get-AzureApplicationGateway** om de status van de gateway te controleren. Als **Begindatum-AzureApplicationGateway** is voltooid in de vorige stap, *staat* moet worden uitgevoerd en *Vip* en *DNS-naam* geldige waarden nodig hebt.

Het volgende voorbeeld ziet u een application gateway die wordt afgerond, uitgevoerd, en bij de hand te nemen verkeer bestemd voor `http://<generated-dns-name>.cloudapp.net`.

    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    Vip           : 138.91.170.26
    DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## <a name="delete-an-application-gateway"></a>Verwijderen van een toepassingsgateway

Een toepassingsgateway verwijderen:

1. Gebruik de cmdlet **Stop-AzureApplicationGateway** om te stoppen de gateway.
2. Gebruik de cmdlet **Verwijderen-AzureApplicationGateway** om het verwijderen van de gateway.
3. Controleer of dat de gateway is verwijderd door met de cmdlet **Get-AzureApplicationGateway** .

Het volgende voorbeeld ziet de cmdlet **Stop-AzureApplicationGateway** op de eerste regel, gevolgd door de uitvoer.

    Stop-AzureApplicationGateway AppGwTest

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Als u de toepassingsgateway hebt gestopt status, gebruikt u de cmdlet **Verwijderen-AzureApplicationGateway** om de service te verwijderen.


    Remove-AzureApplicationGateway AppGwTest

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

Om te bevestigen dat de service is verwijderd, kunt u de cmdlet **Get-AzureApplicationGateway** . Deze stap is niet vereist.


    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Volgende stappen

Als u configureren SSL offload wilt, raadpleegt u [configureren de toepassingsgateway van een voor SSL offload](application-gateway-ssl.md).

Als u configureren van een toepassingsgateway wilt voor gebruik met een interne taakverdeling, raadpleegt u [een toepassingsgateway met een interne taakverdeling (ILB) maken](application-gateway-ilb.md).

Als u meer informatie over het laden taakverdeling opties in het algemeen wilt, Zie:

- [Azure taakverdeling](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure verkeer Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png