<properties
   pageTitle="Aanbevolen procedures voor beveiliging van Azure netwerk | Microsoft Azure"
   description="Dit artikel bevat een set aanbevolen procedures voor het gebruik van netwerk-beveiliging ingebouwd in Azure mogelijkheden."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="swadhwa"
   editor="TomShinder"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/25/2016"
   ms.author="TomSh"/>

# <a name="azure-network-security-best-practices"></a>Aanbevolen procedures voor beveiliging van Azure netwerk

Microsoft Azure kunt u virtuele machines en apparatuur verbinding maken met andere apparaten in het netwerk door deze te Azure virtuele netwerken. Een Azure Virtual Network is een virtueel netwerk constructie waarmee u verbinding virtueel netwerk interface kaarten maken met een virtueel netwerk toe te staan dat TCP/IP-basis communicatie tussen ingeschakeld netwerkapparaten. Azure virtuele Machines verbonden met een Azure Virtual Network kunnen verbinding maken met apparaten op hetzelfde Azure virtuele netwerk, verschillende Azure virtuele netwerken, op Internet of zelfs in uw eigen on-premises implementatie-netwerken.

In dit artikel bespreken we een verzameling aanbevolen procedures voor beveiliging van Azure netwerk. Deze aanbevolen procedures zijn afgeleid van onze ervaring met Azure netwerken en de ervaringen van klanten tevreden bent zelf.

Voor elke aangeraden leggen we:

- Wat de beste manier is
- Waarom u wilt deze aanbevolen procedure inschakelen
- Wat mogelijk het resultaat als u niet het wordt aanbevolen inschakelen
- Mogelijke alternatieven voor het wordt aanbevolen
- Hoe u kunt meer zodat het wordt aanbevolen

In dit artikel aanbevolen procedures voor beveiliging Azure-netwerk is gebaseerd op een advies consensus en Azure platformmogelijkheden en functiesets, die zijn op de tijd die in dit artikel is geschreven. Meningen en -technologieën wijzigen na verloop van tijd en in dit artikel worden bijgewerkt regelmatig om deze wijzigingen aan te geven.

Azure netwerk beveiliging aanbevolen procedures in dit artikel beschreven opnemen:

- Logisch segment subnetten
- De werking van routering bepalen
- U wordt gedwongen Tunneling inschakelen
- Virtuele netwerkapparaten gebruiken
- DMZs implementeren voor beveiliging passen
- Voorkomen dat blootgesteld aan Internet met specifieke WAN-verbindingen
- Beschikbaarheid en prestaties optimaliseren
- Globale taakverdeling gebruiken
- Uitschakelen RDP toegang tot Azure virtuele Machines
- Azure-Beveiligingscentrum inschakelen
- Uw datacenter in Azure uitbreiden


## <a name="logically-segment-subnets"></a>Logisch segment subnetten

[Azure virtuele netwerken](https://azure.microsoft.com/documentation/services/virtual-network/) zijn vergelijkbaar met een LAN op uw on-premises netwerk. Het idee achter een Azure Virtual Network is die u maakt een één privé IP-adres op basis van een spatie netwerk waarop u alle uw [Azure virtuele Machines](https://azure.microsoft.com/services/virtual-machines/)kunt plaatsen. Het IP-adres privéruimtes beschikbaar zijn in de klas A (10.0.0.0/8), klasse B (172.16.0.0/12) en klasse C (192.168.0.0/16) bereiken.

Lijkt op wat u on-premises implementatie, u moet een grotere adresruimte segmenteren in subnetten. U kunt [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) gebaseerd subnetten beginselen uw subnetten maken.

Automatisch routering tussen subnetten gebeurt en u hoeft niet te routeren tabellen handmatig configureren. De standaardinstelling is echter dat er geen toegang tot besturingselementen voor netwerk tussen de subnetten die u op het Azure virtuele netwerk maken. Om te kunnen de toegang tot besturingselementen voor netwerk tussen subnetten hebt gemaakt, moet u iets tussen de subnetten gezet.

Een van de dingen die u gebruiken kunt voor het uitvoeren van deze taak is een [Beveiligingsgroep netwerk](../virtual-network/virtual-networks-nsg.md) (NSG). NSGs zijn eenvoudige statuscontrole systemen voor pakketcontrole die gebruikmaken van de 5-tupel (de bron-IP, bronpoort doel-IP-, doelpoort en laag 4 protocol) benadering maken toestaan/weigeren regels voor netwerkverkeer. U kunt toestaan of weigeren van verkeer naar en vanuit één IP-adres, aan en uit meerdere IP-adressen of zelfs naar en vanuit hele subnetten.

Met behulp van NSGs voor netwerk-toegangsbeheer tussen subnetten, kunt u resources die bij de dezelfde of een ander rol in hun eigen subnetten horen plaatsen. Bijvoorbeeld het ware een eenvoudige 3 lagen-toepassing die bestaat uit een weblaag: een logica toepassingslaag en een databaselaag. U plaatsen virtuele machines die deel uitmaakt van elk van deze niveaus in hun eigen subnetten. U kunt vervolgens NSGs gebruiken om te bepalen verkeer tussen de subnetten:

- Web laag virtuele machines verbindingen met de computers van de logica toepassing kan alleen worden geïnitieerd en verbindingen van Internet kan alleen worden accepteren
- Toepassing logica virtuele machines verbindingen met databaselaag kan alleen worden geïnitieerd en accepteert alleen verbindingen vanuit de weblaag
- Database laag virtuele machines kan geen verbinding met iets buiten hun eigen subnet tot stand brengen en verbindingen vanuit de logica toepassingslaag kan alleen worden geaccepteerd.

Lees het artikel voor meer informatie over het netwerk beveiligingsgroepen en hoe u deze logisch segmenten uw Azure virtuele netwerken kunt gebruiken, [Wat is een beveiligingsgroep netwerk](../virtual-network/virtual-networks-nsg.md) (NSG).

## <a name="control-routing-behavior"></a>De werking van routering bepalen

Als u een virtuele machine op een Azure Virtual Network plaatst, ziet u dat de virtuele machine verbinding met andere VM op hetzelfde Azure virtuele netwerk maken kunt, zelfs als de andere virtuele machines in verschillende subnetten zijn. De reden waarom dit mogelijk is is er is een verzameling systeem routes die standaard zijn ingeschakeld, die dit type communicatie toestaan. Deze standaardroutes toestaan virtuele machines op hetzelfde Azure virtuele netwerk om te starten verbindingen met elkaar en met Internet (voor uitgaande communicatie met alleen Internet).

De standaard-systeem-routes zijn handig voor veel implementatie-scenario's, maar er zijn situaties waarin u wilt aanpassen van de configuratie van de routering voor uw implementaties. Deze aanpassingen kunt u het adres van de volgende hop te bereiken specifieke bestemmingen configureren.

Het is raadzaam dat u gebruiker gedefinieerd Routes configureren wanneer u een virtueel netwerk beveiliging toestel, waarin wordt beschreven in een latere aanbevolen implementeert.

> [AZURE.NOTE] gebruiker gedefinieerd Routes zijn niet vereist en de standaard systeem routes werken in de meeste gevallen.

U kunt meer informatie over de gebruiker gedefinieerde Routes en hoe u deze configureert Lees het artikel [Wat zijn de gebruiker gedefinieerde Routes en IP-doorsturen](../virtual-network/virtual-networks-udr-overview.md).

## <a name="enable-forced-tunneling"></a>U wordt gedwongen Tunneling inschakelen

Voor een beter begrip afgedwongen tunneling, is het handig zijn om te begrijpen wat 'gesplitste tunneling' is.
De meest voorkomende voorbeeld van gesplitste tunneling wordt weergegeven met een VPN-verbindingen. Stel dat u een VPN-verbinding van de chatruimte hotel met uw bedrijfsnetwerk. Deze verbinding, kunt u verbinding maakt met bronnen op uw bedrijfsnetwerk en alle communicatie met bronnen op uw bedrijfsnetwerk Ga via de tunnel VPN.

Wat gebeurt er wanneer u wilt verbinden met bronnen op Internet? Wanneer gesplitste tunnel is ingeschakeld, wordt deze verbindingen Ga rechtstreeks naar Internet en niet via de tunnel VPN. Sommige beveiligingsexperts kunnen Hiermee kunt u een mogelijke risico worden en kunnen daarom aangeraden gesplitste tunneling worden uitgeschakeld en alle verbindingen, bestemd voor Internet en bestemd voor bedrijfsresources, leest u over de VPN-tunnel. Het voordeel hiervoor is dat verbindingen met Internet vervolgens tot en met het bedrijfsnetwerk beveiligingsapparaten die het geval zou worden niet gedwongen als de VPN-client verbinding met Internet buiten de VPN-tunnel.

Nu we terughalen dit virtuele machines in een netwerk met een Azure Virtual. De standaardroutes voor een virtueel netwerk Azure toestaan virtuele machines om te starten verkeer met Internet. Dit kan ook een beveiligingsrisico vertegenwoordigen terwijl deze uitgaande verbindingen aanvallen van een virtuele machine vergroten kunnen en door onbevoegden worden gebruikt.
Om die reden is het raadzaam in te schakelen afgedwongen tunneling op uw virtuele machines wanneer u meerdere lokale verbinding tussen uw Azure Virtual Network en uw on-premises netwerk hebt. We gaan wordt uitleggen cross lokale connectivity verderop in dit Azure netwerken aanbevolen procedures-document.

Als u niet een cross lokale verbinding hebt, controleert u of u profiteren van netwerk beveiligingsgroepen (eerder besproken) of Azure virtuele beveiliging toestellen (besproken volgende) om te voorkomen dat uitgaande verbindingen met Internet uit uw Azure virtuele Machines netwerk.

Meer informatie over wordt gedwongen tunneling en het inschakelen van, lees het artikel [PowerShell en Azure resourcemanager configureren gedwongen Tunneling gebruiken](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md).

## <a name="use-virtual-network-appliances"></a>Virtuele netwerkapparaten gebruiken

Beveiligingsgroepen netwerk en de gebruiker gedefinieerde routering kunt zorgen voor een bepaalde mate van netwerkbeveiliging bij het netwerk- en transport lagen van het [OSI-model](https://en.wikipedia.org/wiki/OSI_model), gaan er zijn situaties waarin u wordt wilt of moet beveiliging op hoog niveau van de stapel inschakelen. In deze situaties, is het raadzaam dat u virtuele netwerk beveiliging toestellen is verstrekt door Azure partners implementeren.

Beveiliging van Azure netwerkapparaten kunnen bieden sterk verbeterde beveiligingsniveaus wat is verstrekt door besturingselementen voor het niveau van netwerk. Enkele van de mogelijkheden van netwerk beveiliging verstrekt door een virtueel netwerk beveiligingsapparaten zijn:

- Gebruik
- Indringers detectie/indringers preventie
- Beveiligingsprobleem management
- Beheer van de toepassing
- Afwijking netwerk gebaseerde detectie
- Web filteren
- Antivirussoftware
- Botnet beveiliging

Als u een hoger niveau van network security dan kunt u met niveau toegang tot besturingselementen voor netwerk vereist, klikt u vervolgens raadzaam is dat u onderzoeken en implementeren van Azure virtuele beveiliging netwerkapparaten.

Voor meer informatie over welke Azure virtuele beveiliging netwerkapparaten beschikbaar zijn, en hun mogelijkheden, gaat u naar de [Azure Marketplace](https://azure.microsoft.com/marketplace/) en het zoeken naar 'beveiliging' en "netwerkbeveiliging".

##<a name="deploy-dmzs-for-security-zoning"></a>DMZs implementeren voor beveiliging passen
Een DMZ of "omtrek netwerk" is een fysieke of logische netwerksegment dat is ontworpen om biedt extra tussen uw activa en Internet. De bedoeling de DMZ is gespecialiseerde access besturingselement netwerkapparaten plaats op de rand van het netwerk DMZ, zodat alleen de gewenste verkeer is toegestaan voorbij de beveiliging-apparaat en uw Azure virtuele netwerk.

DMZs zijn handig omdat u zich met uw netwerk beheren, richten kunt controle, vastleggen en rapportage op de apparaten op de rand van de Azure Virtual Network. U zou hier meestal inschakelen DDoS preventie, indringers detectie/indringers preventie systemen (id's / IP-Adressen), firewallregels en beleidsregels, web filteren, netwerk antimalware en meer. De beveiliging netwerkapparaten tussen Internet en uw Azure Virtual Network en er een interface op beide netwerken.

Dit is de eenvoudige ontwerp van een DMZ, zijn er veel verschillende DMZ-ontwerpen, zoals dubbelzijdig, tri-homed, multi-homed, enzovoort.

We raden voor alle geheim implementaties implementeert een DMZ om het beveiligingsniveau netwerk voor uw Azure resources te verbeteren.

Lees het artikel [Microsoft-Cloudservices en netwerkbeveiliging](../best-practices-network-security.md)meer informatie over DMZs en hoe u deze implementeert in Azure wordt aangegeven.

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Voorkomen dat blootgesteld aan Internet met specifieke WAN-verbindingen
Veel organisaties hebt gekozen de route hybride IT. In hybride IT zijn sommige informatieactiva van het bedrijf in Azure wordt aangegeven, terwijl de anderen on-premises implementatie blijven. In veel gevallen wordt sommige onderdelen van een service wordt uitgevoerd in Azure terwijl de andere onderdelen blijven on-premises implementatie.

In de hybride IT scenario is er meestal een type cross-premises connectivity. Dit cross-premises connectivity kan het bedrijf hun on-premises implementatie-netwerken verbinden met Azure virtuele netwerken. Er zijn twee cross-premises connectiviteitsoplossingen beschikbaar:

- Site-naar-site VPN
- ExpressRoute

[Site-naar-site VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) vertegenwoordigt een virtuele particuliere verbinding tussen uw on-premises netwerk en een Azure Virtual Network. Deze verbinding vindt plaats via Internet en u kunt informatie "tunnel" binnen een versleutelde koppeling tussen uw netwerk en Azure. Site-naar-site VPN is een beveiligde, volwaardige technologie die is geïmplementeerd door ondernemingen allerlei tientallen jaren te horen. Tunnel versleuteling wordt uitgevoerd met behulp van [IPsec tunnelmodus](https://technet.microsoft.com/library/cc786385.aspx).

Site-naar-site VPN is een vertrouwde, betrouwbare en waarop deze is opgezet technologie, verkeer binnen de tunnel via Internet verloopt. Bovendien is de bandbreedte relatief beperkt tot maximaal over 200Mbps.

Als u een uitzonderlijke niveau van de beveiliging of prestaties voor uw verbindingen cross-premises vereist, is het raadzaam dat u Azure ExpressRoute voor uw connectivity cross-premises gebruikt. ExpressRoute is een speciale WAN koppeling tussen uw on-premises locatie of een Exchange-hostingprovider. Omdat dit een telco-verbinding is, wordt uw gegevens niet via Internet worden verzonden en daarom wordt niet blootgesteld aan de mogelijke risico's in internetcommunicatie.

Lees het artikel [Technisch overzicht ExpressRoute](../expressroute/expressroute-introduction.md)voor meer informatie over de werking van Azure ExpressRoute en het implementeren.

## <a name="optimize-uptime-and-performance"></a>Beschikbaarheid en prestaties optimaliseren
Bestaat uit de drie vakken van vandaag meest bepalende beveiligingsmodel vertrouwelijkheid, integriteit en beschikbaarheid (CIA). Vertrouwelijkheid is over versleuteling en privacy, integriteit gaat ervoor te zorgen dat gegevens worden niet gewijzigd door onbevoegden en beschikbaarheid is over ervoor te zorgen dat de gemachtigde personen toegang tot de informatie die ze gemachtigd zijn om toegang te zijn. Fout bij in een van deze gebieden vertegenwoordigt een mogelijke inbreuk beveiligingsfuncties in.

Beschikbaarheid kan worden beschouwd als over beschikbaarheid en prestaties. Als een service niet actief is, kunnen gegevens kan niet worden geopend. Als prestaties zo slecht is dat de gegevens onbruikbaar, kunnen we de gegevens die moeten worden niet toegankelijk kunt. Daarom uitgeschakeld moeten we doen wat kunnen wij om ervoor te zorgen dat onze services hebben optimale beschikbaarheid en prestaties.
Een populaire en effectieve methode voor het verbeteren van beschikbaarheid en prestaties is het gebruik van taakverdeling. Taakverdeling is een methode netwerkverkeer distribueren voor servers die deel uitmaken van een service. Bijvoorbeeld als u de front-endwebservers als onderdeel van uw service hebt, kunt u taakverdeling distributie van het verkeer via uw meerdere front-endwebservers.

Deze verdeling van verkeer Hiermee verhoogt u beschikbaarheid omdat als een van de webservers niet beschikbaar is, de taakverdeling dan stoppen met het verzenden van verkeer op die server en verkeer omgeleid naar de servers die nog steeds online zijn. Taakverdeling kunt u er ook prestaties, omdat de processor-, netwerk- en geheugen realiseren voor het serveren van aanvragen is verdeeld over alle het selectievakje laden in overeenstemming servers.

Het is raadzaam dat u gebruikmaken van taakverdeling wanneer dit mogelijk is en zo nodig voor de services. We wordt geschiktheid in de volgende secties adres.
Op het niveau van Azure Virtual Network biedt Azure dat u met drie primair laden van taakverdeling opties:

- HTTP-gebaseerde taakverdeling
- Externe taakverdeling
- Interne taakverdeling

## <a name="http-based-load-balancing"></a>HTTP-gebaseerde taakverdeling
HTTP-gebaseerde taakverdeling baseert beslissingen over welke server voor het verzenden van verbindingen met de kenmerken van het HTTP-protocol. Azure heeft een HTTP-taakverdeling die door de naam van de toepassingsgateway gaat.

Wij raden u ons Azure-toepassingsgateway wanneer:

- Toepassingen waarvoor aanmeldingsaanvragen van dezelfde gebruiker/client sessie naar dezelfde back-enddatabase virtuele machine hebt bereikt. Voorbeelden hiervan zou worden winkelen winkelwagen apps en e-mail-endwebservers.
- Toepassingen die u vrijgeven webserver-farms van SSL beë boven wilt door te profiteren van de toepassingsgateway [SSL offload](https://f5.com/glossary/ssl-offloading) functie.
- Toepassingen, zoals een netwerk voor contentlevering, die is vereist meerdere HTTP-aanvragen op de dezelfde langdurige TCP-verbinding met worden gerouteerd of laden naar verschillende back-enddatabase servers verdeeld.

Lees het artikel [Overzicht van de Gateway toepassing](../application-gateway/application-gateway-introduction.md)meer informatie over de werking van Azure-toepassingsgateway en hoe u deze kunt gebruiken in uw implementaties.

## <a name="external-load-balancing"></a>Externe taakverdeling
Externe taakverdeling vindt plaats wanneer binnenkomende verbindingen van Internet verdeeld over uw servers bevinden in een Azure Virtual Network worden. De externe taakverdeling van Azure kunt u deze mogelijkheid en het is raadzaam dat u deze gebruiken wanneer u de Plaktoetsen sessies niet vereist of SSL offload.

In tegenstelling tot HTTP gebaseerde taakverdeling, de externe taakverdeling gebruikt informatie bij het netwerk- en transport lagen van het OSI netwerken model beslissingen nemen op welke server balance '-verbinding met laden.

Het is raadzaam die u externe taakverdeling gebruikt telkens als er [stateless toepassingen](http://whatis.techtarget.com/definition/stateless-app) , verzoeken voor oproepen van Internet accepteren.

Lees voor meer informatie over de werking van de Azure externe taakverdeling en hoe u implementeren kunt, moet u het artikel [aan de slag voor het maken van een Internet die tegenover elkaar liggen taakverdeling in resourcemanager via PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="internal-load-balancing"></a>Interne taakverdeling
Interne taakverdeling is vergelijkbaar met het externe taakverdeling en gebruikt de methode balance '-verbindingen met de servers daarachter laden. De enige verschil is dat de taakverdeling in dit geval verbindingen accepteert van virtuele machines die zich niet op Internet. In de meeste gevallen zijn de verbindingen die zijn geaccepteerd voor taakverdeling gestart door apparaten in een Azure virtuele netwerk.

Het is raadzaam interne taakverdeling voor scenario's die van deze mogelijkheid gebruikmaken, zoals wanneer u moet laden balance '-verbindingen met SQL-Servers of interne endwebservers te gebruiken.

Lees het artikel [aan de slag voor het maken van een interne taakverdeling via PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md#update-an-existing-load-balancer)meer informatie over hoe Azure interne taakverdeling werkt en hoe u deze kunt implementeren.

## <a name="use-global-load-balancing"></a>Globale taakverdeling gebruiken
Openbare cloud computing maken het mogelijk om te implementeren globaal verdeeld toepassingen die zich bevindt in datacenters overal ter wereld onderdelen hebben. Dit is mogelijk op Microsoft Azure vanwege van Azure globale datacenter aanwezigheid. In tegenstelling tot de taakverdeling van technologieën die eerder maakt globale taakverdeling het mogelijk om te services beschikbaar, zelfs wanneer volledige datacenters mogelijk niet meer beschikbaar.

U kunt dit type globale taakverdeling in Azure wordt aangegeven door te profiteren van [Azure verkeer Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)krijgen. Verkeer Manager maken is het mogelijk om te laden balance '-verbindingen met uw services op basis van de locatie van de gebruiker.

Als de gebruiker het verzoek uw service van de Europese Unie, wordt de verbinding bijvoorbeeld doorgestuurd naar uw services in een datacenter van de Europese Unie bevindt. In dit onderdeel van verkeer Manager globale taakverdeling helpt prestaties verbeteren omdat sneller dan verbinding maakt met datacenters die ver weg verbinding maken met het dichtstbijzijnde datacenter is geladen.

Klik aan de beschikbaarheid van de zorgt globale taakverdeling ervoor dat uw service beschikbaar is, zelfs als een hele datacenter moet nu beschikbaar.

Bijvoorbeeld als een Azure datacenter moet worden niet beschikbaar vanwege milieu redenen of vanwege bijvoorbeeld (zoals regionale netwerkstoringen), verbindingen met uw service zou worden gerouteerd naar het dichtstbijzijnde datacenter van de online. Deze algemene taakverdeling wordt uitgevoerd door gebruik te maken van DNS-beleidsregels die u in verkeer Manager kunt maken.

Het is raadzaam om verkeer Manager te gebruiken voor de cloud-oplossingen die u ontwikkelen die heeft een sterk uiteen verdeelde bereik tussen meerdere regio's en het hoogste niveau van beschikbaarheid mogelijke vereist.

Lees het artikel [Wat is verkeer Manager](../traffic-manager/traffic-manager-overview.md)als u wilt weten over Azure verkeer Manager en hoe u deze implementeert.

## <a name="disable-rdpssh-access-to-azure-virtual-machines"></a>Uitschakelen RDP/SSH toegang tot Azure virtuele Machines
Het is mogelijk Azure virtuele Machines met het [Remote Desktop Protocol](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) en de protocollen [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) te bereiken. Deze protocollen kunnen u virtuele machines beheren vanaf externe locaties en zijn standaard in datacenter computing.

De beveiliging op problemen met het gebruik van deze protocollen via Internet is dat onbevoegden [felle](https://en.wikipedia.org/wiki/Brute-force_attack) bladwijzers gebruiken kunnen om naar Azure virtuele Machines toegang te krijgen. Zodra de onbevoegden toegang krijgen, kunnen ze uw VM gebruiken als een starten voor het verlies van andere computers op uw Azure virtuele netwerk of zelfs aanvallen netwerkapparaten buiten Azure.

Reden is het raadzaam uit te schakelen directe RDP en SSH toegang aan uw Azure virtuele Machines van Internet. Nadat u directe RDP en SSH toegang van Internet is uitgeschakeld, hebt u andere opties die u gebruiken kunt voor toegang tot deze virtuele machines voor extern beheer:

- VPN punt-naar-site
- Site-naar-site VPN
- ExpressRoute

[Punt-naar-site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) is een andere term voor een RAS-client/server verbinding. Een punt-naar-site VPN kunt één gebruiker verbinding maken met een Azure Virtual Network via Internet. Nadat de verbinding punt-naar-site is gemaakt, kunnen de gebruiker RDP of SSH met verbinding maken met een virtuele machines in Azure virtuele netwerk waarmee de gebruiker via VPN punt-naar-site verbonden bevindt. Hiermee wordt ervan uitgegaan dat de gebruiker kan deze virtuele machines hebt bereikt.

VPN punt-naar-site is veiliger dan directe RDP of SSH verbindingen omdat de gebruiker heeft om te verifiëren tweemaal voordat u verbinding maakt met een virtuele machine. Eerst de gebruiker moet verifiëren (en worden toegestaan) tot stand brengen van de VPN-verbinding van het punt-naar-site; tweede, de gebruiker moet verifiëren (en worden toegestaan) tot stand brengen van de sessie RDP of SSH.

Een [site-naar-site VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) verbindt een hele netwerk met een ander netwerk via Internet. U kunt een VPN-verbinding voor de website naar uw on-premises netwerk verbinden met een Azure Virtual Network. Als u een site-naar-site VPN, gebruikers op implementeert wordt uw on-premises netwerk kan verbinding maken met virtuele machines op uw Azure virtuele netwerk via het protocol RDP of SSH via de website naar VPN-verbinding en hoeft u om directe RDP of SSH toegang te krijgen via Internet.

U kunt ook een specifieke WAN-verbinding functionaliteit die vergelijkbaar is met de VPN van de site-naar-site. De belangrijkste verschillen zijn 1. de specifieke WAN-verbinding via niet Internet en 2. specifieke WAN-verbindingen zijn meestal meer stabiele en zodat. Azure biedt u een oplossing specifieke WAN-verbinding in de vorm van [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/).

## <a name="enable-azure-security-center"></a>Azure-Beveiligingscentrum inschakelen
Azure Beveiligingscentrum kunt u voorkomen dat, detecteren en reageren op threats en vindt dat u meer inzicht in, en controle over, de beveiliging van uw Azure resources. Deze biedt geïntegreerde beveiliging cmdlets voor controle en beleid verschillende uw Azure-abonnementen, helpt detecteren threats die mogelijk anders mis en werkt met een brede selectie van beveiligingsoplossingen.

Azure Beveiligingscentrum kunt optimaliseren en controleren van de beveiliging van het netwerk door:

- Aanbevelingen voor netwerk-beveiliging leveren
- De status van uw beveiliging netwerkconfiguratie bewaken
- U wordt gewaarschuwd voor netwerk gebaseerd threats, zowel op het niveau eindpunt en netwerk

Het is raadzaam Azure Beveiligingscentrum in te schakelen voor al uw Azure implementaties.

Lees het artikel [Inleiding tot Azure Beveiligingscentrum](../security-center/security-center-intro.md)meer informatie over Azure Beveiligingscentrum en hoe u deze inschakelen voor uw implementaties.

## <a name="securely-extend-your-datacenter-into-azure"></a>Uw datacenter veilig uitbreiden in Azure
Veel enterprise IT organisaties om uit te vouwen in de cloud in plaats van hun on-premises implementatie-datacenters groeiende op zoek bent. Deze uitbreiding vertegenwoordigt een uitbreiding van bestaande IT-infrastructuur in de openbare cloud. Door gebruik te maken van cross-premises implementatie connectiviteitsopties is het mogelijk te uw Azure virtuele netwerken behandelen als alleen een ander subnet op de infrastructuur van uw lokale netwerk.

Er is echter een groot aantal planning en ontwerp problemen die moeten eerst worden toegewezen. Dit is vooral belangrijk in het gebied van de beveiliging van het netwerk. Een van de beste manieren om te begrijpen hoe u zo'n ontwerp benadert is om een voorbeeld weer te geven.

Microsoft heeft gemaakt het [Datacenter van de extensie verwijzing Architectuurdiagram](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84#content) en ondersteunende activa laat zien hoe een datacenter verlenging eruitziet. Hier vindt u een voorbeeld van de verwijzing implementatie die u gebruiken kunt om te plannen en ontwerpen van een beveiligde enterprise datacenter van de extensie in de cloud. We raden u dit document als u een idee van de belangrijkste onderdelen van een veilige oplossing.

Bekijk de video [Uw Datacenter uitbreiden naar Microsoft Azure](https://www.youtube.com/watch?v=Th1oQQCb2KA)meer informatie over hoe u uw datacenter veilig uitbreidt in Azure.
