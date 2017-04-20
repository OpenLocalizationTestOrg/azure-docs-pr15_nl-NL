<properties
   pageTitle="Een toepassingsgateway voor SSL offload geconfigureerd met behulp van klassieke implementatie | Microsoft Azure"
   description="Dit artikel bevat instructies voor het maken van een toepassingsgateway met SSL offload met behulp van het model Azure klassieke-implementatie."
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

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>Een toepassingsgateway voor SSL offload geconfigureerd met behulp van het implementatiemodel klassieke

> [AZURE.SELECTOR]
-[Azure-portal](application-gateway-ssl-portal.md)
-[Azure resourcemanager PowerShell](application-gateway-ssl-arm.md)
-[Azure klassieke PowerShell](application-gateway-ssl.md)

Azure-toepassingsgateway kan worden geconfigureerd om de sessie Secure Sockets Layer (SSL) op de gateway om te voorkomen dure SSL decoderen taken om optreden bij de webfarm te beëindigen. SSL-offload eenvoudiger ook de installatie van front-server en het beheer van de webtoepassing.

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de meest recente versie van de Azure PowerShell-cmdlets met behulp van het installatieprogramma van de Web-Platform. U kunt downloaden en installeren van de meest recente versie van het **Windows PowerShell** -gedeelte van de [pagina Downloads](https://azure.microsoft.com/downloads/).
2. Controleer of u een virtueel netwerk werkt met een geldige subnet. Zorg ervoor dat er geen virtuele machines of cloud-implementaties het subnet worden gebruikt. De toepassingsgateway moet op zichzelf in een virtueel netwerk subnet.
3. De servers die u configureren voor het gebruik van de toepassingsgateway moeten bestaan of de eindpunten ervan hebt gemaakt in het virtuele netwerk of met een openbare IP-/ VIP hebt toegewezen.

Ga als de volgende stappen in de aangegeven volgorde om SSL offload configureren op een toepassingsgateway:

1. [Een toepassingsgateway maken](#create-an-application-gateway)
2. [SSL-certificaten uploaden](#upload-ssl-certificates)
3. [De gateway configureren](#configure-the-gateway)
4. [De gatewayconfiguratie instellen](#set-the-gateway-configuration)
5. [De gateway starten](#start-the-gateway)
6. [Controleer of de status van gateway](#verify-the-gateway-status)


## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Als u wilt de gateway hebt gemaakt, gebruikt u de cmdlet **New-AzureApplicationGateway** , de waarden vervangen door uw eigen. Facturering voor de gateway niet op dit moment kan worden gestart. Facturering wordt gestart in een latere stap, wanneer de gateway is gestart.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Om te valideren dat de gateway is gemaakt, kunt u de cmdlet **Get-AzureApplicationGateway** .

In het voorbeeld, *Beschrijving*, *InstanceCount*en *GatewaySize* zijn optionele parameters. De standaardwaarde voor *InstanceCount* is 2, met een maximumwaarde van 10. De standaardwaarde voor *GatewaySize* is normaal. Kleine en grote andere beschikbare waarden zijn. *VirtualIPs* - en *DNS-naam* worden weergegeven als lege omdat de gateway nog niet is gestart. Deze waarden worden gemaakt wanneer de gateway actief is.

    Get-AzureApplicationGateway AppGwTest

## <a name="upload-ssl-certificates"></a>SSL-certificaten uploaden

Gebruik **Toevoegen-AzureApplicationGatewaySslCertificate** om het certificaat van de server in *pfx* -indeling te uploaden naar de toepassingsgateway. De certificaatnaam van de is de naam van een gebruiker zijn gekozen en moet uniek zijn binnen de toepassingsgateway. Dit certificaat wordt verwezen door deze naam in alle certificaat management bewerkingen op de toepassingsgateway.

Dit volgende voorbeeld ziet u de cmdlet, de waarden in de steekproef vervangen door uw eigen.

    Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>

Vervolgens valideren de certificaat-upload. Gebruik de cmdlet **Get-AzureApplicationGatewayCertificate** .

Dit voorbeeld ziet u de cmdlet op de eerste regel, gevolgd door de uitvoer.

    Get-AzureApplicationGatewaySslCertificate AppGwTest

    VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
    VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
    Name           : SslCert
    SubjectName    : CN=gwcert.app.test.contoso.com
    Thumbprint     : AF5ADD77E160A01A6......EE48D1A
    ThumbprintAlgo : sha1RSA
    State..........: Provisioned

>[AZURE.NOTE] Het certificaatwachtwoord heeft tussen 4 t/m 12 tekens, letters of cijfers. Speciale tekens worden niet geaccepteerd.

## <a name="configure-the-gateway"></a>De gateway configureren

Een toepassing gateway-configuratie bestaat uit meerdere waarden. De waarden kunnen worden gekoppeld samen om de configuratie te maken.

De waarden zijn:

- **Back-enddatabase server-groep:** De lijst met IP-adressen van de back-end-servers. De IP-adressen vermeld ofwel moeten behoren tot de virtuele netwerk subnet of een openbare IP/VIP moeten worden.
- **Back-enddatabase groep serverinstellingen:** Elke groep heeft instellingen, zoals poort, protocol en cookie gebaseerde affiniteit. Deze instellingen zijn gekoppeld aan een groep en worden toegepast op alle servers in de groep.
- **Front poort:** Deze poort is de openbare poort dat is geopend in de toepassingsgateway. Verkeer raakt deze poort en vervolgens wordt omgeleid naar een van de back-end-servers.
- **Luisteraar ervan af:** De luisteraar ervan af heeft een front poort, een protocol (Http of Https, deze waarden zijn hoofdlettergevoelig), en de SSL-certificaat-naam (als SSL configureren offload).
- **Regel:** De regel de luisteraar ervan af en de back-enddatabase server-groep gekoppeld en wordt gedefinieerd welke back-end-server-groep het verkeer moet worden doorgestuurd naar wanneer deze een bepaalde luisteraar ervan af. Op dit moment wordt alleen de *eenvoudige* regel ondersteund. De *eenvoudige* regel is round robin laden verdeling.

**Aanvullende configuratie notities**

Voor de configuratie van SSL-certificaten, moet het protocol in **HttpListener** wijzigen naar *Https* (hoofdlettergevoelig). Het element **SslCert** wordt toegevoegd aan **HttpListener** met de waarde die is ingesteld op dezelfde naam als gebruikt voor het uploaden van voorafgaand aan de sectie voor SSL-certificaten. De front poort moet worden bijgewerkt naar 443.

**Om in te schakelen affiniteit op basis van een cookie**: een toepassingsgateway kan worden geconfigureerd om ervoor te zorgen dat een verzoek om van een clientsessie altijd naar de dezelfde VM in de webfarm wordt gestuurd. Dit scenario wordt uitgevoerd door webweergave van een sessiecookie waarmee de gateway bij naar de verkeer correct. Stel om in te schakelen affiniteit cookie gebaseerde, **CookieBasedAffinity** op *ingeschakeld* in het element **BackendHttpSettings** .



U kunt uw configuratie maken door een configuratieobject te maken of met behulp van een XML-bestand.
Als wilt samenstellen uw configuratie met behulp van een XML-configuratiebestand, gebruiken in het onderstaande voorbeeld:

**Configuratie van XML-voorbeeld**

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendIPConfigurations />
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>443</Port>
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
                <Protocol>Https</Protocol>
                <SslCert>GWCert</SslCert>
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


## <a name="set-the-gateway-configuration"></a>De gatewayconfiguratie instellen

Vervolgens kunt u de toepassingsgateway instellen. U kunt de cmdlet **Set-AzureApplicationGatewayConfig** gebruiken met een configuratieobject of met een XML-bestand.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

## <a name="start-the-gateway"></a>De gateway starten

Wanneer de gateway is geconfigureerd, gebruikt u de cmdlet **Start-AzureApplicationGateway** starten van de gateway. Facturering voor de toepassingsgateway van een gestart nadat de gateway is gestart.


**Notitie:** De **Begin-AzureApplicationGateway** -cmdlet duurt maximaal 15-20 minuten om te voltooien.

    Start-AzureApplicationGateway AppGwTest

## <a name="verify-the-gateway-status"></a>Controleer of de status van gateway

Gebruik de cmdlet **Get-AzureApplicationGateway** om de status van de gateway te controleren. Als **Begindatum-AzureApplicationGateway** is voltooid in de vorige stap, *staat* moet worden uitgevoerd en *VirtualIPs* en *DNS-naam* geldige waarden nodig hebt.

In dit voorbeeld ziet u een application gateway die omhoog, is uitgevoerd, en is klaar om verkeer door te nemen.

    Get-AzureApplicationGateway AppGwTest

    Name          : AppGwTest2
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {23.96.22.241}
    DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net


## <a name="next-steps"></a>Volgende stappen


Als u meer informatie over het laden taakverdeling opties in het algemeen wilt, Zie:

- [Azure taakverdeling](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure verkeer Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)