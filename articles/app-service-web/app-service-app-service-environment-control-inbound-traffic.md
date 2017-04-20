<properties 
    pageTitle="Het beheren van binnenkomende verkeer naar een Service van de App-omgeving" 
    description="Meer informatie over het configureren van netwerk beveiligingsregels voor het beheren van binnenkomende verkeer naar een App-Service-omgeving." 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>Het beheren van binnenkomende verkeer naar een Service van de App-omgeving

## <a name="overview"></a>Overzicht ##
Een App-Service-omgeving kan worden gemaakt in **een van beide** een virtueel resourcemanager Azure-netwerk, **of** een klassieke implementatie model [virtueel netwerk][virtualnetwork].  Een nieuwe virtuele netwerk- en nieuw subnet kunnen worden gedefinieerd op het moment dat een App-Service-omgeving die is gemaakt.  Een App-Service-omgeving kan ook worden gemaakt in een bestaande virtuele netwerk- en bestaande subnet.  Met een recente wijziging in juni 2016, kunnen ASEs nu worden geïmplementeerd in virtuele netwerken die openbare-adresbereiken of RFC1918 adres spaties (dat wil zeggen gebruiken persoonlijke adressen).  Voor meer informatie over het maken van een App-Service-omgeving Zie [hoe u een App-Service-omgeving maken][HowToCreateAnAppServiceEnvironment].

Een App-Service-omgeving moet altijd binnen een subnet worden gemaakt omdat een subnet vindt u de rand van een netwerk die kan worden gebruikt om binnenkomende verkeer achter boven apparaten en services vergrendelen zodat HTTP en HTTPS-verkeer alleen uit bepaalde boven IP-adressen wordt geaccepteerd.

Binnenkomende en uitgaande netwerkverkeer op een subnet wordt geregeld met behulp van een [beveiligingsgroep netwerk][NetworkSecurityGroups]. Binnenkomende verkeer bepalen vereist beveiligingsregels voor netwerken in een netwerk-beveiligingsgroep maken en klik vervolgens toewijzen de netwerkbeveiliging groeperen het subnet met de App-Service-omgeving.

Wanneer een beveiligingsgroep netwerk is toegewezen aan een subnet, wordt binnenkomend verkeer op apps in de App-omgeving is toegestane/geblokkeerde op basis van de toestaan en regels die zijn gedefinieerd in de beveiligingsgroep van netwerk weigeren.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="network-ports-used-in-an-app-service-environment"></a>Netwerk poorten gebruiken in een App Service-omgeving ##
Voordat u vergrendelen binnenkomend netwerkverkeer met een beveiligingsgroep netwerk, is het belangrijk te weten de set vereiste en optionele netwerkpoorten die worden gebruikt door een App-Service-omgeving.  Per ongeluk sluiten uitschakelen verkeer naar bepaalde poorten kan resulteren in verlies van functionaliteit in een App-Service-omgeving.

Hier volgt een lijst met poorten die worden gebruikt door een App-Service-omgeving. Alle poorten zijn **TCP**, tenzij anderszins duidelijk vermeld:

- 454: **poort vereist** die worden gebruikt door Azure-infrastructuur voor het beheren en onderhouden van App-serviceomgevingen via SSL.  Blokkeer niet verkeer naar deze poort.  Deze poort is altijd gekoppeld aan de openbare VIP van een ASE.
- 455: **poort vereist** die worden gebruikt door Azure-infrastructuur voor het beheren en onderhouden van App-serviceomgevingen via SSL.  Blokkeer niet verkeer naar deze poort.  Deze poort is altijd gekoppeld aan de openbare VIP van een ASE.
- 80: standaardpoort voor binnenkomende HTTP-verkeer naar apps uitgevoerd in de App-Service van abonnement in een App-Service-omgeving.  Op een ASE ILB is ingeschakeld, worden deze poort is gekoppeld aan de ILB-adres van de ASE.
- 443: standaardpoort voor binnenkomende SSL-verkeer naar apps uitgevoerd in de App-Service van abonnement in een App-Service-omgeving.  Op een ASE ILB is ingeschakeld, worden deze poort is gekoppeld aan de ILB-adres van de ASE.
- 21: besturingselement kanaal voor FTP.  Deze poort kan veilig worden geblokkeerd als FTP niet wordt gebruikt.  Klik op een ASE ILB is ingeschakeld, kunt deze poort zijn gekoppeld aan het adres ILB voor een ASE.
- 990: besturingselement kanaal voor TRANSFEREERT.  Deze poort kan veilig worden geblokkeerd als TRANSFEREERT niet wordt gebruikt.  Klik op een ASE ILB is ingeschakeld, kunt deze poort zijn gekoppeld aan het adres ILB voor een ASE.
- 10001-10020: gegevenskanalen voor FTP.  Net als met het kanaal besturingselement kunnen deze poorten worden veilig geblokkeerd als FTP niet wordt gebruikt.  Klik op een ASE ILB is ingeschakeld, kunt deze poort zijn gekoppeld aan van de ASE ILB adres.
- 4016: gebruikt voor foutopsporing op afstand met Visual Studio 2012.  Deze poort kan veilig worden geblokkeerd als de functie niet wordt gebruikt.  Op een ASE ILB is ingeschakeld, worden deze poort is gekoppeld aan de ILB-adres van de ASE.
- 4018: gebruikt voor foutopsporing op afstand met Visual Studio-2013.  Deze poort kan veilig worden geblokkeerd als de functie niet wordt gebruikt.  Klik op een ASE ILB ingeschakelde deze poort afhankelijk van het ILB-adres van de ASE.
- 4020: gebruikt voor foutopsporing op afstand met Visual Studio-2015.  Deze poort kan veilig worden geblokkeerd als de functie niet wordt gebruikt.  Op een ASE ILB is ingeschakeld, worden deze poort is gekoppeld aan de ILB-adres van de ASE.

## <a name="outbound-connectivity-and-dns-requirements"></a>Uitgaande connectiviteit en DNS-vereisten ##
Voor een App-Service-omgeving goed, vereist deze uitgaande toegang tot verschillende eindpunten. Er wordt een volledige lijst met de externe eindpunten die worden gebruikt door een ASE in de sectie 'Netwerkconnectiviteit vereist' van het artikel [Netwerkconfiguratie voor ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) .

App Service omgevingen vragen om een geldige DNS-infrastructuur is geconfigureerd voor het virtuele netwerk.  Als de DNS-configuratie voor welke reden dan ook is gewijzigd nadat een App-Service-omgeving is gemaakt, kunnen ontwikkelaars een App-Service-omgeving volgt te werk om de nieuwe DNS-configuratie afdwingen.  Aan de bovenkant van het App-omgeving management blad in de [portal van Azure] activeert lopende omgeving opnieuw opstarten met het pictogram "Opnieuw" gevestigd[ NewPortal] veroorzaakt de omgeving volgt te werk om de nieuwe DNS-configuratie.

Het is ook aanbevolen dat een aangepaste DNS-servers op de vnet setup tijd vóór het maken van een App-Service-omgeving vooraf zijn.  Als een virtueel netwerk DNS-configuratie wordt gewijzigd terwijl een App-Service-omgeving wordt gemaakt, die leidt mislukken proces maken van de App-omgeving.  In een soortgelijke vein, als er een aangepaste DNS-server op het andere uiteinde van een gateway VPN bestaat en de DNS-server niet bereikbaar is of niet beschikbaar is, is het proces voor het maken van App-omgeving, ook mislukt.

## <a name="creating-a-network-security-group"></a>Een beveiligingsgroep netwerk maken ##
Volledige Raadpleeg voor informatie over hoe netwerk werk beveiligingsgroepen de volgende [informatie][NetworkSecurityGroups].  De details onder touch op worden gemarkeerd van netwerk-beveiligingsgroepen, met een focus over het configureren en een beveiligingsgroep netwerk toepassen op een subnet met een App-Service-omgeving.

**Notitie:** Beveiligingsgroepen netwerk kunnen worden geconfigureerd grafisch met behulp van de [Azure-Portal](https://portal.azure.com) of via Azure PowerShell.

Beveiligingsgroepen netwerk worden eerst gemaakt als een zelfstandig product entiteit die is gekoppeld aan een abonnement. Aangezien netwerk beveiligingsgroepen zijn gemaakt in een Azure regio, moet u zorgen dat de beveiligingsgroep van het netwerk is gemaakt in hetzelfde gebied, als de App-Service-omgeving.

Het volgende voorbeeld toont een beveiligingsgroep netwerk maken:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Nadat een beveiligingsgroep netwerk is gemaakt, worden een of meer netwerk beveiligingsregels worden toegevoegd aan deze.  Aangezien de set regels wijzigen kan, wordt het wordt aanbevolen om het nummeringsschema gebruikt voor de regel prioriteiten zodat u deze eenvoudig aanvullende regels invoegen na verloop van tijd.

Het volgende voorbeeld ziet u een regel die expliciet toegang tot de management poorten die nodig zijn voor de Azure infrastructuur verleent voor het beheren en onderhouden van een App-Service-omgeving.  Houd er rekening mee dat alle management verkeer via SSL loopt en is beveiligd met clientcertificaten, zodat u daar ook de poorten zijn geopend ze niet toegankelijk door een entiteit dan Azure infrastructuur zijn.


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    

Wanneer u toegang tot poort 80 en 443 "verbergen" een App-Service-omgeving achter boven apparaten of services te vergrendelen, moet u weten het boven IP-adres.  Als u een web-toepassing firewall (WAF) gebruikt, de WAF wordt bijvoorbeeld een eigen IP-adres (of adressen) waarin deze worden gebruikt bij proxy-verkeer naar een volgende App-omgeving.  U moet dit IP-adres gebruiken in de parameter *SourceAddressPrefix* van een regel voor netwerk.

Klik in het onderstaande voorbeeld is inkomende verkeer van een specifiek boven IP-adres expliciet toegestaan.  De adres- *1.2.3.4* wordt gebruikt als tijdelijke aanduiding voor het IP-adres van een boven WAF.  Wijzig de waarde zodat deze overeenkomen met het adres dat wordt gebruikt door uw boven apparaat of de service.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
Desgewenst FTP-ondersteuning is, kunnen de volgende regels worden gebruikt als een sjabloon om toegang te verlenen aan de FTP-besturingselement poort en gegevens channel-poorten.  Aangezien FTP een statuscontrole protocol is, kunt u mogelijk niet kunnen worden gerouteerd FTP-verkeer via een traditionele HTTP-/ HTTPS-firewall of proxyserver apparaat.  In dit geval moet u de *SourceAddressPrefix* instellen op een andere waarde - bijvoorbeeld het IP-adres cellenbereik ontwikkelaars of implementatie machines op waarmee FTP-clients wordt uitgevoerd. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Notitie:** het gegevensbereik voor de poort van kanaal kan veranderen tijdens de proefperiode.)

Als externe foutopsporing met Visual Studio wordt gebruikt, demonstratie de volgende regels om toegang te verlenen.  Er is een aparte regel voor elke ondersteunde versie van Visual Studio, omdat elke versie een andere poort voor foutopsporing op afstand gebruikt.  Als u met de FTP-toegang, mogelijk niet correct externe foutopsporing verkeer flow via een traditionele WAF of proxyapparaat.  Het bereik van de IP-adres van Visual Studio-computers ontwikkelaars kunt in plaats daarvan de *SourceAddressPrefix* worden ingesteld.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-to-a-subnet"></a>Een beveiligingsgroep netwerk toewijzen aan een Subnet ##
Een beveiligingsgroep netwerk heeft een regel voor standaard die al dan niet toestaan van toegang tot alle externe-verkeer is toegestaan.  Het resultaat van het combineren van het netwerk beveiligingsregels hierboven beschreven en de standaardregel blokkeren van binnenkomende verkeer, is dat alleen verkeer van adres dat is gekoppeld aan een actie *toestaan* bereiken kunnen verkeer naar apps uitgevoerd in een omgeving met App-Service verzenden.

Nadat u een beveiligingsgroep netwerk wordt gevuld met beveiligingsregels, moet deze worden toegewezen aan het subnet met de App-Service-omgeving.  De opdracht toewijzing wordt verwezen naar zowel de naam van het virtuele netwerk waarin de App-Service-omgeving zich bevindt, als de naam van het subnet waar de App-Service-omgeving die is gemaakt.  

Het volgende voorbeeld ziet u een netwerk-beveiligingsgroep wordt toegewezen aan een subnet en een virtueel netwerk:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Zodra de toewijzing aan een groep netwerk beveiliging is uitgevoerd (de toewijzing is een langdurige bewerkingen en kan een paar minuten duren), alleen binnenkomende verkeer overeenkomen met regels voor *toestaan* wordt apps in de App-omgeving is bereikt.

Het volgende voorbeeld ziet voor volledigheid verwijderen en de beveiligingsgroep van het netwerk van het subnet dus reke‑ koppelen:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Aandachtspunten voor het expliciete IP-SSL ##
Als een app is geconfigureerd met een expliciete IP-SSL (geldt voor ASEs met alleen een openbare VIP), in plaats van de standaard IP-adres van de App-Service-omgeving, HTTP- en HTTPS-verkeer loopt in het subnet via een andere set poorten dan poort 80 en 443.

De afzonderlijke paar poorten die door elk IP-SSL-adres vindt u in de portal gebruikersinterface van de App-Service-omgeving details UX blade.  Selecteer "alle instellingen"--> "IP-adressen".  Het blad 'IP-adressen' ziet u een tabel met alle expliciet geconfigureerde IP-SSL-adressen voor de App-Service-omgeving, samen met de speciale poort paar gegevenspunten die wordt gebruikt om te leiden HTTP en HTTPS-verkeer dat is gekoppeld aan elk IP-SSL-adres.  Is dit poort paar dat worden gebruikt voor de parameters DestinationPortRange moet tijdens het configureren van regels in een netwerk-beveiligingsgroep.

Een app op een ASE is geconfigureerd voor IP-SSL, externe klanten wordt niet weergegeven als niet nodig hebt over het toewijzen van speciale poort paar zorgen te maken.  Verkeer tot de apps die doorloopt normaal gesproken de geconfigureerde IP-SSL-adres.  De vertaling op twee speciale poort plaatsvindt automatisch intern tijdens de laatste fase verkeer naar het subnet met de ASE. 

## <a name="getting-started"></a>Aan de slag

Als u wilt beginnen met App-serviceomgevingen, raadpleegt u [Inleiding tot de App-omgeving][IntroToAppServiceEnvironment]

Alle artikelen en hoe-bevindt zich voor de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor omgevingen met toepassingen Service](../app-service/app-service-app-service-environments-readme.md)aan.

Zie voor meer rond apps veilig verbinding maakt met backend resource in een omgeving van de Service-App [veilig verbinding maakt met Backend resources uit een App-Service-omgeving][SecurelyConnecttoBackend]

Zie voor meer informatie over het platform Azure App-Service, [Azure App-Service][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[SecurelyConnecttoBackend]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->
 
