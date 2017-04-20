<properties 
    pageTitle="Netwerkarchitectuur overzicht van de App Service omgevingen" 
    description="Overzicht van de architectuur van netwerk topologie ofApp Service-omgevingen." 
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

# <a name="network-architecture-overview-of-app-service-environments"></a>Netwerkarchitectuur overzicht van de App Service omgevingen

## <a name="introduction"></a>Inleiding ##
App-serviceomgevingen aanmaakt altijd binnen een subnet van een [virtueel netwerk] [ virtualnetwork] -apps die worden uitgevoerd in een App-Service-omgeving kunnen communiceren met privé eindpunten bevinden binnen de dezelfde virtuele netwerktopologie.  Aangezien klanten de onderdelen van hun virtuele netwerkinfrastructuur vergrendelen mogelijk, is het belangrijk dat u weet welke soorten netwerk communicatie loopt die zich bij een App-Service-omgeving voordoen.

## <a name="general-network-flow"></a>Algemeen netwerk stroom ##
 
Als een App Service-omgeving (ASE) gebruikmaakt van een openbare virtuele IP-adres (VIP) voor apps, worden alle binnenkomende verkeer op die openbare VIP binnenkomt.  Dit geldt ook voor HTTP en HTTPS-verkeer is toegestaan voor apps, maar ook andere verkeer voor FTP, externe functionaliteit voor foutopsporing en Azure beheertaken uit te voeren.  Zie het artikel over het [beheren van binnenkomende verkeer] voor een volledige lijst met de specifieke poorten (vereiste en optionele) die beschikbaar zijn op de openbare VIP[ controllinginboundtraffic] in een App-Service-omgeving. 

App Service ook omgevingen actieve apps die afhankelijk zijn van alleen een virtueel netwerk interne adres, ook wel een ILB (interne taakverdeling)-adres genoemd.  Klik op een ILB ingeschakeld ASE, HTTP en HTTPS-verkeer is toegestaan voor apps, evenals externe foutopsporing oproepen, worden op het adres ILB worden bezorgd.  Meest voorkomende ILB-ASE configuraties komen FTP/TRANSFEREERT verkeer zijn ook in het adres ILB.  Azure management bewerkingen wordt nog steeds naar poorten 454/455 op de openbare VIP van een ILB ingeschakeld echter ASE.

De onderstaande afbeelding ziet een overzicht van de verschillende binnenkomende en uitgaande netwerk loopt voor een App-Service-omgeving waarin de apps zijn gekoppeld aan een openbare virtuele IP-adres:

![Algemeen netwerk loopt][GeneralNetworkFlows]

Een App-Service-omgeving kunt communiceren met een verscheidenheid aan privé klant eindpunten.  Apps in de App-Service-omgeving actief kunnen bijvoorbeeld database server (s) die worden uitgevoerd op IaaS virtuele machines in de dezelfde virtuele netwerktopologie verbinden.

>[AZURE.IMPORTANT] De weergave Netwerkdiagram bekijkt, zijn de "andere berekenen bronnen" geïmplementeerd in een ander Subnet uit de App-Service-omgeving. Bronnen in hetzelfde Subnet met de ASE implementeert blokkeert connectiviteit uit ASE met deze resources (behalve voor specifieke binnen-ASE omleiding). Dashboard implementeren naar een ander Subnet in plaats daarvan (in de dezelfde VNET). De App-Service-omgeving vervolgens kunnen om verbinding te maken. Er is geen extra configuratie vereist.

App Service omgevingen communiceren ook met Sql-database en Azure Storage resources nodig zijn voor het beheren en een App-Service besturingsomgeving.  Enkele van de Sql- en resources die een App-Service-omgeving met communiceert bevinden zich in hetzelfde gebied, als de App-Service-omgeving, terwijl de anderen zich bevinden in externe Azure regio's.  Hierdoor is uitgaande verbinding met Internet altijd vereist om een App-Service-omgeving correct. 

Aangezien een App-Service-omgeving is geïmplementeerd in een subnet, kan netwerk beveiligingsgroepen kunnen worden gebruikt om te bepalen binnenkomende verkeer naar het subnet.  Zie voor meer informatie over het beheren van binnenkomende verkeer naar een App-Service-omgeving, het volgende [artikel][controllinginboundtraffic].

Voor meer informatie over het uitgaande verbinding met Internet toestaan uit de omgeving van een App-Service, raadpleegt het volgende artikel over het werken met [Express Route][ExpressRoute].  Dezelfde aanpak die worden beschreven in het artikel is van toepassing wanneer u werkt met Site-naar-Site-connectiviteit en gebruikt u wordt gedwongen tunneling.

## <a name="outbound-network-addresses"></a>Uitgaande netwerkadressen ##
Wanneer een App-Service-omgeving uitgaande oproepen, is een IP-adres altijd gekoppeld aan de uitgaande oproepen.  Het specifieke IP-adres dat wordt gebruikt, is afhankelijk van of het eindpunt dat wordt aangeroepen zich in de virtuele netwerktopologie, of buiten de virtuele netwerktopologie bevindt.

Als het eindpunt dat wordt aangeroepen **buiten** van de virtuele netwerktopologie, is de openbare VIP van de App-Service-omgeving met de uitgaande adres (of de uitgaande adres NAT) dat wordt gebruikt.  Dit adres vindt u in de gebruikersinterface van de portal voor de App-Service-omgeving in eigenschappen blade.
 
![Uitgaande IP-adres][OutboundIPAddress]

Dit adres kan ook worden bepaald voor ASEs die u alleen een openbare VIP hebt door te maken van een app in de App-omgeving, en vervolgens een *nslookup* uit te voeren in van de app-adres. De resulterende IP-adres is zowel de openbare VIP, evenals uitgaande NAT-adres van de App-Service-omgeving.

Als het eindpunt dat wordt aangeroepen **in** van de virtuele netwerktopologie, zijn uitgaande van de bellen app de het interne IP-adres van de afzonderlijke berekeningscluster resource die de app uitvoeren.  Er is echter niet persistent toewijzing van virtueel netwerk interne IP-adressen aan apps.  Apps kunnen navigeren via verschillende berekeningscluster resources en de groep met beschikbare berekeningscluster resources in een App-Service-omgeving vanwege schaalbaarheid bewerkingen kunnen wijzigen.

Echter, aangezien een App-Service-omgeving altijd binnen een subnet bevindt is, wordt gegarandeerd dat het interne IP-adres van een berekeningscluster resource die een app uitvoeren altijd binnen het bereik CIDR van het subnet wordt liggen.  Hierdoor wanneer fijnmazige ACL's of beveiligingsgroepen op netwerk worden gebruikt voor toegang tot andere eindpunten binnen het virtuele netwerk secure, moet het subnetbereik met de App-Service-omgeving u toegang is verleend.

In het volgende diagram ziet u deze concepten uitgebreider:

![Uitgaande netwerkadressen][OutboundNetworkAddresses]

In het bovenstaande diagram:

- Aangezien de openbare VIP van de App-Service-omgeving is 192.23.1.2, is dat het uitgaande IP-adres dat is gebruikt bij het maken van oproepen naar 'Internet' eindpunten.
- Het bereik CIDR van het met subnet voor de App-Service-omgeving is 10.0.1.0/26.  Andere eindpunten binnen de dezelfde virtuele netwerkinfrastructuur ziet oproepen van apps als die afkomstig zijn van ergens in het adresbereik van dit.

## <a name="calls-between-app-service-environments"></a>Oproepen tussen App Service omgevingen ##
Een meer complexe scenario kan optreden als u meerdere App serviceomgevingen in hetzelfde virtuele netwerk implementeren en uitgaande oproepen van één App Service omgeving naar een andere App-omgeving aanbrengen.  Dit soort cross App Service-omgeving oproepen wordt ook als "Internet" oproepen verwerkt.

In het volgende diagram ziet u een voorbeeld van een gelaagde architectuur met apps op een App-omgeving (bijvoorbeeld "Deur" WebApps) apps bellen op een tweede App-omgeving (bijvoorbeeld interne back-enddatabase API apps niet moeten worden toegankelijk zijn vanuit Internet). 

![Oproepen tussen App Service omgevingen][CallsBetweenAppServiceEnvironments] 

In het bovenstaande voorbeeld bevat de App-Service-omgeving "ASE één" een uitgaande IP-adres van 192.23.1.2.  Als een app op deze wordt App Service-omgeving maakt een uitgaande oproep door naar een app uitvoeren op een tweede App Service-omgeving ("ASE twee") in hetzelfde virtuele netwerk, de uitgaande oproep bevinden worden behandeld als een 'Internet'-gesprek.  Hierdoor het netwerkverkeer binnengekomen op de tweede App-omgeving wordt weergegeven als die afkomstig zijn van 192.23.1.2 (dat wil zeggen niet de subnet adres van het bereik van de eerste omgeving voor App-Service).

Hoewel oproepen tussen verschillende App serviceomgevingen worden behandeld als "Internet"-gesprekken, als beide omgevingen App-Service bevinden zich in dezelfde Azure regio het netwerkverkeer blijft in een netwerk met de regionale Azure en wordt niet fysiek flow via de openbare Internet.  Hierdoor kunt u een beveiligingsgroep netwerk op het subnet van de tweede App Service-omgeving alleen toestaan binnenkomende oproepen uit de eerste App Service-omgeving (waarvan uitgaande IP-adres is 192.23.1.2), zodat beveiligde communicatie tussen de App-Service-omgevingen.

## <a name="additional-links-and-information"></a>Aanvullende koppelingen en informatie ##
Alle artikelen en hoe-bevindt zich voor de App serviceomgevingen zijn beschikbaar in het [Leesmij-bestand voor omgevingen met toepassingen Service](../app-service/app-service-app-service-environments-readme.md)aan.

Meer informatie over binnenkomende poorten die worden gebruikt door de App serviceomgevingen en netwerk beveiligingsgroepen gebruiken om te bepalen binnenkomende verkeer is beschikbaar [hier][controllinginboundtraffic].

Informatie over het gebruik van de gebruiker gedefinieerde routes om te verlenen uitgaande internettoegang voor App-serviceomgevingen is beschikbaar in dit [artikel][ExpressRoute]. 


<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[ExpressRoute]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

