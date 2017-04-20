<properties
   pageTitle="Een aangepaste test voor de Gateway-toepassing maken met behulp van PowerShell in het implementatiemodel klassieke | Microsoft Azure"
   description="Informatie over het maken van een aangepaste test voor de Gateway-toepassing via PowerShell in het implementatiemodel klassieke"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Een aangepaste test voor maken Azure-toepassingsgateway (klassieke) via PowerShell

> [AZURE.SELECTOR]
- [Azure-portal](application-gateway-create-probe-portal.md)
- [Azure resourcemanager PowerShell](application-gateway-create-probe-ps.md)
- [Azure klassieke PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Leer hoe [u deze stappen uitvoert met het model resourcemanager](application-gateway-create-probe-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Een toepassingsgateway maken:

1. Hiermee maakt u een bron van de gateway toepassing.
2. Maak een XML-bestand of een configuratieobject.
3. De configuratie aan iedere resource van de gateway nieuwe toepassing doorvoeren.

### <a name="create-an-application-gateway-resource"></a>Een bron van de gateway toepassing maken

Als u wilt de gateway hebt gemaakt, gebruikt u de cmdlet **New-AzureApplicationGateway** , de waarden vervangen door uw eigen. Facturering voor de gateway niet op dit moment kan worden gestart. Facturering wordt gestart in een latere stap, wanneer de gateway is gestart.

Het volgende voorbeeld wordt een application gateway gemaakt met behulp van een virtueel netwerk genoemd 'testvnet1' en een subnet 'subnet-1' genoemd.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Om te valideren dat de gateway is gemaakt, kunt u de cmdlet **Get-AzureApplicationGateway** .

    Get-AzureApplicationGateway AppGwTest

>[AZURE.NOTE]  De standaardwaarde voor *InstanceCount* is 2, met een maximumwaarde van 10. De standaardwaarde voor *GatewaySize* is normaal. U kunt kiezen tussen klein, normaal en groot.

 *VirtualIPs* en *DNS-naam* worden weergegeven als lege omdat de gateway nog niet is gestart. Deze worden gemaakt wanneer de gateway actief is.

## <a name="configure-an-application-gateway"></a>Een toepassingsgateway configureren

U kunt de toepassingsgateway configureren met behulp van XML- of een configuratieobject.

## <a name="configure-an-application-gateway-by-using-xml"></a>Een toepassingsgateway configureren met behulp van XML

In het volgende voorbeeld, kunt u een XML-bestand gebruiken voor alle toepassing gateway-instellingen configureren en ze aan iedere resource van toepassing gateway hebt toegewezen.  

### <a name="step-1"></a>Stap 1

Kopieer de volgende tekst naar Kladblok.

    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name>
            <Type>Private</Type>
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>    
    <FrontendPorts>
        <FrontendPort>
            <Name>port1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
      </Probes>
     <BackendAddressPools>
        <BackendAddressPool>
            <Name>pool1</Name>
            <IPAddresses>
                <IPAddress>1.1.1.1</IPAddress>
                <IPAddress>2.2.2.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>listener1</Name>
            <FrontendIP>fip1</FrontendIP>
        <FrontendPort>port1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>lbrule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>setting1</BackendHttpSettings>
            <Listener>listener1</Listener>
            <BackendAddressPool>pool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


Bewerk de waarden tussen de haakjes voor de configuratie-items. Sla het bestand met de extensie .xml.

Het volgende voorbeeld ziet u hoe u een configuratiebestand gebruiken voor het instellen van de toepassingsgateway te verdelen HTTP-verkeer op openbare poort 80 en netwerkverkeer naar back-enddatabase poort 80 tussen twee IP-adressen met behulp van een aangepaste test.

>[AZURE.IMPORTANT] Het item protocol Http of Https is hoofdlettergevoelig.

Een nieuw item van de configuratie \<onderzoeken\> wordt toegevoegd aan de aangepaste sondes configureren.

De configuratieparameters zijn:

- **Naam** - naam voor de aangepaste test.
- **Protocol** - Protocol dat wordt gebruikt (mogelijke waarden zijn HTTP of HTTPS).
- **Host** en het **pad** - volledige URL-pad dat wordt aangeroepen door de toepassingsgateway om te bepalen de status van het exemplaar. Bijvoorbeeld, hebt u een website http://contoso.com/, worden kan de aangepaste test geconfigureerd voor "http://contoso.com/path/custompath.htm" voor test controles moet een positief HTTP-antwoord.
- **Interval** - configureert test interval controles in seconden.
- **Time-out** - Hiermee definieert u de test time-out voor een selectievakje HTTP-antwoord.
- **UnhealthyThreshold** - het aantal mislukte HTTP-antwoorden nodig moeten worden gemarkeerd het exemplaar van de back-enddatabase als *beschadigd*.

De naam van de test wordt verwezen in de <BackendHttpSettings> configuratie toewijzen welke back-end-toepassingen worden gebruikt in aangepaste test-instellingen.

## <a name="add-a-custom-probe-configuration-to-an-existing-application-gateway"></a>Een aangepaste test-configuratie toevoegen aan een bestaande application gateway

Drie stappen voor het wijzigen van de huidige configuratie van een toepassingsgateway vereist: krijgen van het huidige XML-configuratiebestand en de toepassingsgateway configureren met de nieuwe XML-instellingen wijzigen om een aangepaste test.

### <a name="step-1"></a>Stap 1

Het XML-bestand met behulp van get-AzureApplicationGatewayConfig krijgen. Hiermee wordt de configuratie XML in als u wilt toevoegen van een instelling test worden gewijzigd.

    Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"


### <a name="step-2"></a>Stap 2

Open het XML-bestand in een teksteditor. Toevoegen een `<probe>` sectie na `<frontendport>`.

    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
    </Probes>

Klik in de sectie backendHttpSettings van de XML-toevoegen de naam van de test zoals wordt weergegeven in het volgende voorbeeld:

        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>

Sla het XML-bestand.

### <a name="step-3"></a>Stap 3

De configuratie van de toepassing gateway bijwerken met het nieuwe XML-bestand met behulp van de **Set-AzureApplicationGatewayConfig**. Hiermee wordt de toepassingsgateway bijgewerkt met de nieuwe configuratie.

    Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"


## <a name="next-steps"></a>Volgende stappen

Als u wilt configureren offload Secure Sockets Layer (SSL), raadpleegt u [configureren de toepassingsgateway van een voor SSL offload](application-gateway-ssl.md).

Als u configureren van een toepassingsgateway wilt voor gebruik met een interne taakverdeling, raadpleegt u [een toepassingsgateway met een interne taakverdeling (ILB) maken](application-gateway-ilb.md).
