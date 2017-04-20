<properties 
    pageTitle="Veilig verbinding maakt met BackEnd Resources in een App Service-omgeving" 
    description="Meer informatie over het veilig koppelen aan backend resources uit een App-Service-omgeving." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="stefsch"/>   

# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a>Veilig verbinding maakt met Backend Resources in een App Service-omgeving #

## <a name="overview"></a>Overzicht ##
Aangezien een App-Service-omgeving altijd is gemaakt in **een van beide** een virtueel resourcemanager Azure-netwerk, **of** een klassieke implementatie model [virtueel netwerk][virtualnetwork], uitgaande verbindingen vanuit een App-Service-omgeving naar andere bronnen backend kunnen flow uitsluitend via het virtuele netwerk.  Met een recente wijziging in juni 2016, kan ASEs ook worden geïmplementeerd in virtuele netwerken die openbare-adresbereiken of RFC1918 adres spaties (dat wil zeggen gebruiken persoonlijke adressen).  

Bijvoorbeeld, kunnen er een SQL Server op een cluster van virtuele machines met poort 1433 vergrendeld.  Het is mogelijk dat het eindpunt ACLd om alleen toegang te krijgen van andere bronnen op hetzelfde virtuele netwerk heeft.  

Een ander voorbeeld gevoelige eindpunten on-premises implementatie kan worden uitgevoerd en niet worden verbonden met Azure via beide [Naar website] [ SiteToSite] of [Azure ExpressRoute] [ ExpressRoute] verbindingen.  Hierdoor kunnen worden alleen de resources in de virtuele netwerken die zijn verbonden met de Site-naar-Site of ExpressRoute tunnels voor toegang tot on-premises implementatie eindpunten.

Voor alle van deze scenario's kunnen apps uitgevoerd op een App-Service-omgeving veilig verbinding maken met de verschillende servers en resources.  Uitgaand verkeer van apps uitgevoerd in een App-Service-omgeving naar privé eindpunten in hetzelfde virtuele netwerk (of verbonden met hetzelfde virtuele netwerk), wordt alleen mailstroom via het virtuele netwerk.  Uitgaand verkeer naar privé eindpunten doorloopt niet via de openbare Internet.

Enige belemmering geldt voor uitgaand verkeer uit een App-Service-omgeving wilt eindpunten binnen een virtueel netwerk.  App Service omgevingen bereiken niet eindpunten van virtuele machines bevinden in **hetzelfde** subnet als de App-Service-omgeving.  Dit moet gewoonlijk geen een probleem zo lang maken als App serviceomgevingen zijn geïmplementeerd in een subnet gereserveerd voor exclusief gebruik door alleen de omgeving in de App-Service.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="outbound-connectivity-and-dns-requirements"></a>Uitgaande connectiviteit en DNS-vereisten ##
Voor een App-Service-omgeving goed, vereist deze uitgaande toegang tot verschillende eindpunten. Er wordt een volledige lijst met de externe eindpunten die worden gebruikt door een ASE in de sectie 'Netwerkconnectiviteit vereist' van het artikel [Netwerkconfiguratie voor ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) .

App Service omgevingen vragen om een geldige DNS-infrastructuur is geconfigureerd voor het virtuele netwerk.  Als de DNS-configuratie voor welke reden dan ook is gewijzigd nadat een App-Service-omgeving is gemaakt, kunnen ontwikkelaars een App-Service-omgeving volgt te werk om de nieuwe DNS-configuratie afdwingen.  Een lopende omgeving opnieuw opstarten met het pictogram "Opnieuw starten" boven aan het App-omgeving management blad in de portal activeert, wordt de omgeving volgt te werk om de nieuwe DNS-configuratie.

Het is ook aanbevolen dat een aangepaste DNS-servers op de vnet setup tijd vóór het maken van een App-Service-omgeving vooraf zijn.  Als een virtueel netwerk DNS-configuratie wordt gewijzigd terwijl een App-Service-omgeving wordt gemaakt, die leidt mislukken proces maken van de App-omgeving.  In een soortgelijke vein, als er een aangepaste DNS-server op het andere uiteinde van een VPN-gateway bestaat en de DNS-server niet bereikbaar is of niet beschikbaar is, is het proces voor het maken van App-omgeving, ook mislukt.

## <a name="connecting-to-a-sql-server"></a>Verbinding maken met een SQL Server
Een veelgebruikte SQL Server-configuratie heeft een eindpunt luisteren op poort 1433:

![SQL Server-eindpunt][SqlServerEndpoint]

Er zijn twee methoden voor het beperken van verkeer naar dit eindpunt:


- [Netwerk toegangsbeheerlijsten] [ NetworkAccessControlLists] (netwerk ACL's)

- [Beveiligingsgroepen netwerk][NetworkSecurityGroups]


## <a name="restricting-access-with-a-network-acl"></a>Beperken van toegang met een netwerk ACL

Poort 1433 kan worden beveiligd met een netwerk toegangsbeheerlijst.  Het voorbeeld hieronder whitelists client adressen die afkomstig zijn van binnen een virtueel netwerk en toegang tot alle andere clients geblokkeerd.

![Voorbeeld van een besturingselement Access netwerk][NetworkAccessControlListExample]

Alle toepassingen uitvoeren in de App-omgeving in hetzelfde virtuele netwerk als de SQL Server is mogelijk verbinding maken met de SQL Server-instantie met behulp van het **interne VNet** IP-adres voor de SQL Server virtuele machine.  

De verbindingsreeks voorbeeld wordt verwezen naar de SQL Server het privé IP-adres.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Hoewel de virtuele machine ook een openbare eindpunt heeft, worden verbindingspogingen via het openbare IP-adres niet geaccepteerd vanwege het netwerk ACL. 

## <a name="restricting-access-with-a-network-security-group"></a>Beperken van toegang met een netwerk-beveiligingsgroep
Er is een alternatieve methode voor het beveiligen van access met een netwerk-beveiligingsgroep.  Netwerk beveiligingsgroepen kunnen worden toegepast op afzonderlijke virtuele machines of op een subnet met virtuele machines.

Een beveiligingsgroep netwerk moet eerst worden gemaakt:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

De toegang tot alleen VNet interne verkeer beperken is heel eenvoudig met een netwerk-beveiligingsgroep.  De standaardregels in een netwerk-beveiligingsgroep wordt alleen toegang vanuit andere netwerkclients in hetzelfde virtuele netwerk toestaan.

Hierdoor vergrendelen toegang tot SQL Server is net zo eenvoudig als een netwerk-beveiligingsgroep met de standaard-regels toepassen op een van beide de virtuele machines met SQL Server of het subnet met de virtuele machines.

Het onderstaande voorbeeld van een beveiligingsgroep netwerk de subnet, met toepassing:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'
    
Het eindresultaat is een set regels voor het blokkeren van externe toegang terwijl u de interne toegang VNet:

![Standaard netwerk beveiligingsregels][DefaultNetworkSecurityRules]


## <a name="getting-started"></a>Aan de slag
Alle artikelen en hoe-bevindt zich voor de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor omgevingen met toepassingen Service](../app-service/app-service-app-service-environments-readme.md)aan.

Als u wilt beginnen met App-serviceomgevingen, raadpleegt u [Inleiding tot de App-omgeving][IntroToAppServiceEnvironment]

Zie voor meer rond het beheren van binnenkomende verkeer naar uw App Service-omgeving [bepalen binnenkomende verkeer naar een App-Service-omgeving][ControlInboundASE]

Zie voor meer informatie over het platform Azure App-Service, [Azure App-Service][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
