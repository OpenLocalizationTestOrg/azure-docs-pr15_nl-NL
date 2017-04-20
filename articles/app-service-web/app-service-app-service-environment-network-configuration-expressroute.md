<properties 
    pageTitle="Gegevens in de netwerkconfiguratie voor het werken met Express doorsturen" 
    description="Gegevens in de netwerkconfiguratie voor het uitvoeren van App-serviceomgevingen in een virtuele netwerken verbonden met een Circuitlijnen ExpressRoute." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="stefsch"/>   

# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>Gegevens in de netwerkconfiguratie voor App Service omgevingen met ExpressRoute 

## <a name="overview"></a>Overzicht ##
Klanten verbinding kunnen maken van een [Azure ExpressRoute] [ ExpressRoute] circuitlijnen naar hun virtuele netwerkinfrastructuur, dus hun on-premises netwerk uitbreiden naar Azure.  Een App-Service-omgeving kan worden gemaakt in een subnet van deze [virtuele netwerk] [ virtualnetwork] infrastructuur.  Apps die worden uitgevoerd op de App-Service-omgeving kunnen vervolgens alleen via de ExpressRoute-verbinding tot stand brengen beveiligde verbindingen naar back-enddatabase bronnen toegankelijk.  

Een App-Service-omgeving kan worden gemaakt in **een van beide** een virtueel resourcemanager Azure-netwerk, **of** een klassieke implementatie model virtueel netwerk.  Met een recente wijziging in juni 2016, kunnen ASEs nu ook worden geïmplementeerd in virtuele netwerken die openbare-adresbereiken of RFC1918 adres spaties (dat wil zeggen gebruiken persoonlijke adressen). 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="required-network-connectivity"></a>Vereiste netwerkconnectiviteit ##
Zijn er vereisten voor netwerk-connectiviteit voor App-serviceomgevingen die niet in eerste instantie mogelijk worden voldaan in een virtueel netwerk is verbonden met een ExpressRoute.  App Service omgevingen vereisen alle van de volgende handelingen uit om te kunnen correct functioneren:


-  Uitgaande netwerkverbinding met Azure Storage eindpunten overal ter wereld op beide poort 80 en 443.  Dit geldt ook voor eindpunten zich op dezelfde regio bevinden als de App-omgeving, evenals de eindpunten van opslag in **andere** Azure regio's bevinden.  Azure opslag eindpunten oplossen onder de domeinen van de volgende DNS-: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* en *file.core.windows.net*.  
-  Uitgaande netwerkconnectiviteit met de service Azure-bestanden op poort 445.
-  Uitgaande netwerkconnectiviteit naar Sql DB eindpunten bevinden in hetzelfde gebied, als de App-Service-omgeving.  SQL-DB eindpunten onder het volgende domein oplossen: *database.windows.net*.  Hiervoor is vereist voor het openen van toegang tot poorten 1433, 11000-11999 en 14000-14999.  Zie voor meer informatie [in dit artikel over het gebruik van de Sql-Database V12 poort](../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
-  Uitgaande netwerkconnectiviteit op de eindpunten van Azure management vlak (ASM zowel ARM eindpunten).  Dit geldt ook voor uitgaande verbindingen met zowel *management.core.windows.net* en *management.azure.com*. 
-  Uitgaande netwerkconnectiviteit *ocsp.msocsp.com*, *mscrl.microsoft.com* en *crl.microsoft.com*.  Dit is nodig ter ondersteuning van SSL-functionaliteit.
-  De DNS-configuratie voor het virtuele netwerk moet mogelijk het oplossen van alle eindpunten en domeinen die worden genoemd in de eerdere wordt verwezen.  Als deze eindpunten kunnen niet worden omgezet, App-omgeving maken pogingen, mislukt en bestaande App serviceomgevingen wordt gemarkeerd als beschadigd.
-  Uitgaande toegang via poort 53 is vereist voor communicatie met DNS-servers.
-  Als u een aangepaste DNS-server op het andere uiteinde van een gateway VPN aanwezig is, kunnen de DNS-server uit het subnet met de App-Service-omgeving moet bereikbaar. 
-  De uitgaande netwerkpad langs interne zakelijke proxy's kan niet worden verzonden, die niet dwingen tunnel naar on-premises implementatie.  Het effectieve NAT-adres van uitgaand netwerkverkeer doet worden wijzigingen uit de App-Service-omgeving.  Het NAT-adres van een App-Service-omgeving uitgaand verkeer wijzigt, worden connectivity fouten op-veel van de bovenstaande eindpunten.  Dit levert mislukte pogingen van de App-omgeving maken, evenals eerder orde App serviceomgevingen wordt gemarkeerd als beschadigd.  
-  Netwerktoegang tot vereiste poorten inkomende voor App-serviceomgevingen moet worden toegestaan, zoals wordt beschreven in dit [artikel][requiredports].

De DNS-vereisten kunnen worden voldaan doordat een geldige DNS-infrastructuur is geconfigureerd en onderhouden voor het virtuele netwerk.  Als de DNS-configuratie voor welke reden dan ook is gewijzigd nadat een App-Service-omgeving is gemaakt, kunnen ontwikkelaars een App-Service-omgeving volgt te werk om de nieuwe DNS-configuratie afdwingen.  Aan de bovenkant van het App-omgeving management blad in de [portal van Azure] activeert lopende omgeving opnieuw opstarten met het pictogram "Opnieuw" gevestigd[ NewPortal] veroorzaakt de omgeving volgt te werk om de nieuwe DNS-configuratie.

De vereisten van binnenkomende netwerk toegang kunnen worden voldaan door het configureren van een [beveiligingsgroep netwerk] [ NetworkSecurityGroups] op de App-Service-omgeving subnet om de vereiste toegang te krijgen zoals is beschreven in dit [artikel][requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Uitgaande netwerkconnectiviteit inschakelen voor een App Service-omgeving##
Standaard wordt een nieuw gemaakte ExpressRoute circuitlijnen een standaard-route waarmee uitgaande verbinding met Internet.  Met deze configuratie kunnen een App-Service-omgeving verbinding maken met andere Azure eindpunten.

Een algemene klant-configuratie is echter hun eigen standaardroute (0.0.0.0/0) waardoor uitgaande internetverkeer naar de on-premises implementatie in plaats daarvan flow definiëren.  Deze verkeersstroom eindemarkeringen altijd App serviceomgevingen omdat de uitgaand verkeer een van beide geblokkeerde on-premises wordt of NAT naar een niet-herkenbaar reeks adressen die niet langer met verschillende Azure eindpunten werken zou doen.

De oplossing is het definiëren van een (of meer) door de gebruiker gedefinieerde routes (UDRs) op het subnet met de App-Service-omgeving.  Een UDR definieert subnet / regiospecifieke routes die wordt in plaats van de standaard-route van kracht.

Indien mogelijk wordt het gebruik van de volgende configuratie aanbevolen:

- De configuratie ExpressRoute promotie 0.0.0.0/0 van en al dan niet standaard tunnels dwingen alle uitgaand verkeer on-premises implementatie.
- De UDR toegepast op het subnet met de App-Service-omgeving definieert 0.0.0.0/0 met een volgende hop type Internet (een voorbeeld hiervan is verder naar beneden in dit artikel).

Het gecombineerde effect van deze stappen is dat het subnetniveau van de UDR heeft voorrang boven de ExpressRoute gedwongen tunneling, zodat uitgaande internettoegang uit de App-Service-omgeving.

> [AZURE.IMPORTANT] De routes gedefinieerd in een UDR **moet** worden specifieke voorrang op eventuele routes aangekondigd door de configuratie ExpressRoute.  In het onderstaande voorbeeld de adresbereiken globaal 0.0.0.0/0 gebruikt en als zodanig kan potentieel worden per ongeluk overschreven door route advertenties met meer informatie over-adresbereiken.
>
>App-serviceomgevingen worden niet ondersteund met ExpressRoute configuraties die **meerdere – reclame routes van de openbare peering pad naar het persoonlijk peering pad**.  ExpressRoute configuraties waarvoor openbare peering is geconfigureerd, ontvangt route advertenties om een groot aantal Microsoft Azure IP-adresbereiken van Microsoft.  Als deze adresbereiken cross aangekondigd op het privé peering pad, is het eindresultaat dat alle uitgaande netwerkpakketten van de App-Service-omgeving subnet zal dwingen tunnel tot netwerkinfrastructuur voor on-premises implementatie van een klant.  De stroom van dit netwerk is momenteel niet ondersteund met App-serviceomgevingen.  Een oplossing voor dit probleem is cross-reclame routes van de openbare peering pad naar het persoonlijk peering pad stoppen.

Achtergrondinformatie over de gebruiker gedefinieerde routes is beschikbaar in dit [Overzicht][UDROverview].  

Meer informatie over het maken en configureren van de gebruiker gedefinieerde routes is beschikbaar in deze [Instructies][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>Voorbeeld UDR configuratie voor een App Service-omgeving ##

**Minimumvereisten**

1. Azure Powershell installeren vanaf de [pagina Downloads van Azure] [ AzureDownloads] (gedateerd is juni 2015 of later).  Onder "Hulpmiddelen voor de opdrachtregel" vindt u een koppeling "Installeren" onder "Windows Powershell" waarmee u de meest recente Powershell-cmdlets installeert.

2. Het wordt aanbevolen dat een uniek subnet voor exclusief gebruik wordt gemaakt door een App-Service-omgeving.  Dit zorgt ervoor dat de UDRs toegepast op het subnet alleen uitgaand verkeer voor de App-Service-omgeving wordt geopend.
3. **Belangrijk**: implementeer de App-Service-omgeving niet tot **na** de volgende configuratiestappen worden gevolgd.  Dit zorgt ervoor dat uitgaande netwerkconnectiviteit beschikbaar is voordat u probeert te implementeren van een App-Service-omgeving.

**Stap 1: Een benoemde routetabel maken**

Het volgende codefragment maakt een tabel met 'DirectInternetRouteTable' genoemd in de regio West ons Azure:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**Stap 2: Een of meer routes in de routetabel maken**

U moet een of meer routes toevoegen aan de tabel om te kunnen inschakelen uitgaande internettoegang.  

De aanbevolen methode voor het configureren van uitgaande toegang tot Internet werkt het definiëren van een route voor 0.0.0.0/0 zoals hieronder wordt weergegeven.
  
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Denk eraan dat 0.0.0.0/0 is een adresbereik globaal, en als zodanig wordt overschreven door meer informatie over adresbereiken aangekondigd door de ExpressRoute.  Als u wilt herhalen opnieuw de eerdere aanbeveling, moet een UDR met de route van een 0.0.0.0/0 worden gebruikt in combinatie met een ExressRoute-configuratie die alleen ook 0.0.0.0/0 wordt. 

Als alternatief, kunt u een volledig en bijgewerkte lijst met CIDR-bereiken gebruikt door Azure downloaden.  De XML-bestand met alle van de Azure IP-adresbereiken vindt u in het [Microsoft Download Center][DownloadCenterAddressRanges].  

Merk op door deze bereiken wijzigen na verloop van tijd die reden periodiek handmatige updates voor de gebruiker gedefinieerde routes om synchroon te houden.  Ook, omdat er een standaard-bovengrens van 100 routes in een enkel UDR, moet u de Azure-IP-adresbereiken passend maken binnen de limiet van 100 route "Samenvatting" ervoor dat er rekening mee dat UDR routes nodig specifieker dan de routes gedefinieerd door aangekondigd uw ExpressRoute.  


**Stap 3: De routetabel naar het subnet met de App-Service-omgeving koppelen**

De laatste configuratiestap is om te koppelen van de tabel doorsturen naar het subnet waarin de App-Service-omgeving wordt geïmplementeerd.  De volgende opdracht koppelt de "DirectInternetRouteTable" aan de 'ASESubnet' die een App-Service-omgeving uiteindelijk moet bevatten.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**Stap 4: Laatste stappen**

Nadat de routetabel is gekoppeld aan het subnet, wordt het wordt aanbevolen voor het eerst testen en het beoogde effect bevestigen.  Bijvoorbeeld een virtuele machine implementeren in het subnet en controleert u of:


- Uitgaand verkeer naar Azure en niet-Azure eindpunten eerder in dit artikel is **niet** die doorloopt naar beneden de circuitlijnen ExpressRoute.  Is het belangrijk om te controleren of dit gedrag, aangezien als uitgaand verkeer uit het subnet is nog steeds wordt gedwongen tunnel on-premises Service-omgeving van een App maken, altijd mislukt. 
- DNS-lookups voor de eindpunten eerder zijn alle correct kan omzetten. 

Zodra de bovenstaande stappen worden bevestigd, moet u de virtuele machine verwijderen omdat het subnet moet worden 'lege' op het moment dat de App-Service-omgeving die is gemaakt.
 
Gaat u verder met het maken van een App-Service-omgeving!

## <a name="getting-started"></a>Aan de slag
Alle artikelen en hoe-bevindt zich voor de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor omgevingen met toepassingen Service](../app-service/app-service-app-service-environments-readme.md)aan.

Als u wilt beginnen met App-serviceomgevingen, raadpleegt u [Inleiding tot de App-omgeving][IntroToAppServiceEnvironment]

Zie voor meer informatie over het platform Azure App-Service, [Azure App-Service][AzureAppService].

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureDownloads]: http://azure.microsoft.com/en-us/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[NewPortal]:  https://portal.azure.com
 

<!-- IMAGES -->
