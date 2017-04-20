<properties 
   pageTitle="Maken en configureren van een toepassingsgateway met interne taakverdeling (ILB) in een virtueel netwerk | Microsoft Azure"
   description="Deze pagina bevat instructies voor het configureren van een Gateway Azure-toepassing met een eindpunt interne laden verdeeld"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="jdial"
   editor="tysonn"/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/19/2016"
   ms.author="gwallace"/>

# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>Een toepassingsgateway maken met een interne taakverdeling (ILB)

> [AZURE.SELECTOR]
- [Azure klassieke stappen](application-gateway-ilb.md)
- [Resourcemanager Powershell-stappen](application-gateway-ilb-arm.md)

De toepassingsgateway kan worden geconfigureerd met een internetverbinding virtuele IP of met een interne fungeert niet blootgesteld aan internet, ook bekend als interne laden verdeling (ILB) eindpunt. De gateway configureren met een ILB is handig voor interne LOB-toepassingen niet blootgesteld aan internet. Het is ook handig voor services/lagen van een toepassing met meerdere niveaus, die wordt weergegeven in een beveiligd gebied niet blootgesteld aan internet, maar nog steeds vereisen round robin laden distributie, sessie kleverigheid of SSL-beëindiging. In dit artikel begeleidt u bij de stappen voor het configureren van een toepassingsgateway met een ILB.

## <a name="before-you-begin"></a>Voordat u begint

1. Installeer de meest recente versie van de Azure PowerShell-cmdlets met het installatieprogramma van de Web-Platform. U kunt downloaden en installeren van de meest recente versie van het **Windows PowerShell** -gedeelte van de [downloadpagina](https://azure.microsoft.com/downloads/).
2. Controleer of u een virtueel netwerk werkt met geldige subnet.
3. Controleer of u backend-servers in het virtuele netwerk of met een openbare IP-/ VIP die zijn toegewezen.


Als u wilt maken van een toepassingsgateway, moet u de volgende stappen uitvoeren in de aangegeven volgorde. 

1. [Een toepassingsgateway maken](#create-a-new-application-gateway)
2. [De gateway configureren](#configure-the-gateway)
3. [De gatewayconfiguratie instellen](#set-the-gateway-configuration)
4. [De gateway starten](#start-the-gateway)
4. [Controleer of de gateway](#verify-the-gateway-status)



## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken:

**De gateway te maken**, gebruiken de `New-AzureApplicationGateway` cmdlet, de waarden vervangen door uw eigen. Houd er rekening mee dat facturering voor de gateway niet op dit moment wordt gestart. Facturering wordt gestart in een latere stap, wanneer de gateway is gestart.

    PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399

**Voor het valideren van** dat de gateway is gemaakt, kunt u de `Get-AzureApplicationGateway` cmdlet. 

In het voorbeeld, *Beschrijving*, *InstanceCount*en *GatewaySize* zijn optionele parameters. De standaardwaarde voor *InstanceCount* is 2, met een maximumwaarde van 10. De standaardwaarde voor *GatewaySize* is normaal. Kleine en grote andere beschikbare waarden zijn. *VIP* en *DNS-naam* worden weergegeven als lege omdat de gateway nog niet is gestart. Deze worden gemaakt wanneer de gateway actief is. 

    PS C:\> Get-AzureApplicationGateway AppGwTest

    VERBOSE: 4:39:39 PM - Begin Operation:
    Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
    Operation: Get-AzureApplicationGateway
    Name: AppGwTest 
    Description: 
    VnetName: testvnet1 
    Subnets: {Subnet-1} 
    InstanceCount: 2 
    GatewaySize: Medium 
    State: Stopped 
    VirtualIPs: 
    DnsName:


## <a name="configure-the-gateway"></a>De gateway configureren

Een toepassing gateway-configuratie bestaat uit meerdere waarden. De waarden kunnen worden gekoppeld samen om de configuratie te maken.
 
De waarden zijn:

- **Backend server-groep:** De lijst met IP-adressen van de backend-servers. De IP-adressen vermeld moeten al deel uitmaakt van de VNet subnet of een openbare IP/VIP moeten worden. 
- **Backend-serverinstellingen van toepassingen:** Elke groep heeft instellingen, zoals poort, protocol en cookie gebaseerde affiniteit. Deze instellingen zijn gekoppeld aan een groep en worden toegepast op alle servers in de groep.
- **Frontend poort:** Deze poort is de openbare poort geopend op de toepassingsgateway. Verkeer deze poort treffers en wordt vervolgens omgeleid naar een van de backend-servers.
- **Luisteraar ervan af:** De luisteraar ervan af heeft een frontend poort, een protocol (Http of Https, dit zijn hoofdlettergevoelig), en de SSL-certificaat-naam (als SSL configureren offload). 
- **Regel:** De regel de luisteraar ervan af en de backend-server-groep gekoppeld en wordt gedefinieerd welke backend-server-groep het verkeer moet worden doorgestuurd naar wanneer deze een bepaalde luisteraar ervan af. Op dit moment wordt alleen de *eenvoudige* regel ondersteund. De *eenvoudige* regel is round robin laden verdeling.

U kunt uw configuratie maken door het maken van een configuratieobject of met behulp van een XML-bestand. Als wilt samenstellen uw configuratie met behulp van een XML-configuratiebestand, gebruiken in het onderstaande voorbeeld.



Houd rekening met het volgende:


- Het element *FrontendIPConfigurations* beschrijving van de details ILB relevant zijn voor het configureren van de Gateway-toepassing met een ILB. 

- Het Frontend IP- *Type* moet worden ingesteld op 'Privé'

- De *StaticIPAddress* moet worden ingesteld op de gewenste interne IP waarop de gateway verkeer ontvangt. Houd er rekening mee dat het element *StaticIPAddress* optioneel is. Als dit niet is ingesteld, een beschikbaar interne IP uit het geïmplementeerd subnet wordt gekozen. 

- De waarde van het opgegeven in *FrontendIPConfiguration* *naam* element moet worden gebruikt in van de HTTPListener *FrontendIP* element te verwijzen naar de FrontendIPConfiguration.

 **Configuratie van XML-voorbeeld**

 

        <?xml version="1.0" encoding="utf-8"?>
        <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
            <FrontendIPConfigurations>
                <FrontendIPConfiguration>
                    <Name>fip1</Name> 
                    <Type>Private</Type> 
                    <StaticIPAddress>10.0.0.10</StaticIPAddress> 
                </FrontendIPConfiguration>
            </FrontendIPConfigurations>
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
                    <FrontendIP>fip1</FrontendIP>
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
    


## <a name="set-the-gateway-configuration"></a>De gatewayconfiguratie instellen

Vervolgens moet u de toepassingsgateway instellen. U kunt de `Set-AzureApplicationGatewayConfig` cmdlet met een configuratieobject of met een XML-bestand. 

    PS C:\> Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="start-the-gateway"></a>De gateway starten

Wanneer de gateway is geconfigureerd, gebruikt u de `Start-AzureApplicationGateway` cmdlet starten van de gateway. Facturering voor de toepassingsgateway van een gestart nadat de gateway is gestart. 


> [AZURE.NOTE] De `Start-AzureApplicationGateway` cmdlet kan maximaal 15-20 minuten duren. 
   
    PS C:\> Start-AzureApplicationGateway AppGwTest 

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Controleer of de status van gateway

Gebruik de `Get-AzureApplicationGateway` Controleer de status van gateway-cmdlet. Als *Begindatum-AzureApplicationGateway* is voltooid in de vorige stap, de status moet worden *uitgevoerd*en de Vip en de DNS-naam geldige waarden nodig hebt. Dit voorbeeld ziet u de cmdlet op de eerste regel, gevolgd door de uitvoer. In dit voorbeeld wordt de gateway wordt uitgevoerd en gereed is voor verkeer. 

> [AZURE.NOTE] De toepassingsgateway is geconfigureerd voor het accepteren van verkeer op het eindpunt van de geconfigureerde ILB van 10.0.0.10 in dit voorbeeld.

    PS C:\> Get-AzureApplicationGateway AppGwTest 

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway 
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   : 
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {10.0.0.10}
    DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net

## <a name="next-steps"></a>Volgende stappen


Als u meer informatie over het laden taakverdeling opties in het algemeen wilt, Zie:

- [Azure taakverdeling](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure verkeer Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
