<properties
   pageTitle="Overzicht van de beveiliging Azure netwerk | Microsoft Azure"
   description=" In dit artikel kunt u gemakkelijk kunt begrijpen wat Microsoft Azure te bieden in het gebied van network security heeft. Wij bieden eenvoudige toelichtingen voor core netwerk beveiligingsconcepten en vereisten en gegevens op wat Azure te bieden voor elk van deze gebieden heeft. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-network-security-overview"></a>Overzicht van de Azure netwerk-beveiliging

Microsoft Azure bevat een robuuste netwerkinfrastructuur ter ondersteuning van uw toepassing en vereisten voor service-connectiviteit. Netwerkconnectiviteit is het mogelijk dat tussen bronnen in Azure tussen on-premises implementatie en Azure gehost resources, en naar en vanuit het Internet- en Azure.

Het doel van dit artikel is het gemakkelijker maken om berichten te begrijpen wat Microsoft Azure te bieden in het gebied van network security heeft. Wij bieden hier eenvoudige toelichtingen voor core netwerk beveiliging begrippen en vereisten. We ook vindt u informatie over wat Azure te bieden voor elk van deze gebieden heeft. Zijn er veel koppelingen naar andere inhoud toe die u kunt om een beter begrip voor de gebieden waarin u geïnteresseerd bent.

In dit artikel overzicht van de beveiliging van de Azure wordt richten op het volgende:

- Azure netwerken
- Netwerktoegang
- Externe toegang en cross-premises connectiviteit
- Beschikbaarheid
- Logboekregistratie
- Mailnamen omzetten
- DMZ architectuur
- Azure Beveiligingscentrum

## <a name="azure-networking"></a>Azure netwerken

Virtuele machines moet netwerkconnectiviteit. Als u wilt dat dit vereiste wordt ondersteund, vereist Azure virtuele machines kunnen niet worden verbonden met een Azure Virtual Network. Een Azure Virtual Network is een logische constructie gebouwd de fysieke Azure netwerk-structuur. Elke logische Azure Virtual Network is geïsoleerd van alle andere Azure virtuele netwerken. Hiermee kunt zorgen dat netwerkverkeer in uw implementaties niet toegankelijk zijn voor andere Microsoft Azure-klanten is.

Meer informatie:

- [Overzicht van Virtual Network](../virtual-network/virtual-networks-overview.md)

## <a name="network-access-control"></a>Netwerktoegang
Toegangsbeheer voor netwerk is de act connectivity en naar bepaalde apparaten of subnetten binnen een Azure Virtual Network te beperken. Het doel van netwerktoegangsbeheer is om ervoor te zorgen dat uw virtuele machines en services toegankelijk wordt gemaakt voor alleen gebruikers en apparaten dat u ze toegankelijk wilt zijn. Toegang tot besturingselementen voor zijn gebaseerd op toestaan of weigeren beslissingen voor verbindingen naar en vanuit uw VM of de service.

Azure ondersteunt verschillende typen toegangsbeheer netwerk. Hierbij:

- Besturingselement voor netwerk-laag
- Besturingselement routeren en u wordt gedwongen tunneling
- Beveiliging van virtuele netwerkapparaten

### <a name="network-layer-control"></a>Network laag Control
Een secure implementatie is vereist sommige maateenheid voor netwerk-toegangsbeheer. Het doel van netwerktoegangsbeheer is om ervoor te zorgen dat uw virtuele machines en de netwerkservices die worden uitgevoerd voor deze virtuele machines alleen voor gebruik met andere netwerkapparaten communiceren kunnen die ze nodig hebben om te communiceren met en alle andere verbindingspogingen worden geblokkeerd.

Als u Basisnetwerk toegangsbeheer op gebruikersniveau (gebaseerd op IP-adres en het TCP- en UDP-protocol) nodig hebt, kunt u beveiligingsgroepen netwerk gebruiken. Een netwerk beveiliging groep (NSG) is een eenvoudige statuscontrole pakket filteren firewall en deze kunt u voor toegangsbeheer op basis van een [5-tupel](https://www.techopedia.com/definition/28190/5-tuple). NSGs bieden geen toepassing laag inspectie of toegang tot besturingselementen voor geverifieerd.

Meer informatie:

- [Beveiligingsgroepen netwerk](../virtual-network/virtual-networks-nsg.md)

### <a name="route-control-and-forced-tunneling"></a>Route-beheer en afgedwongen Tunneling
De mogelijkheid om te bepalen de werking van de routering op uw Azure virtuele netwerken is van een kritieke netwerk beveiliging en toegang tot de mogelijkheid van de besturing. Als routering is onjuist geconfigureerd, toepassingen en services die worden gehost op uw virtuele computer mogelijk verbinding maken met apparaten die u niet dat ze verbinding maken wilt met, inclusief apparaten eigendom en beheer van mogelijke onbevoegden.

Azure netwerken ondersteunt de mogelijkheid om aan te passen het routeren gedrag voor netwerkverkeer op uw Azure virtuele netwerken. Hiermee kunt u standaard routeren items in de tabel in uw Azure Virtual Network wijzigen. Controle over de werking van routering kunt u ervoor zorgen dat al het verkeer van bepaalde apparaat of groep apparaten binnenkomt of uw Azure Virtual Network tot en met een specifieke locatie verlaat.

Bijvoorbeeld mogelijk u een virtueel netwerk beveiliging toestel op uw Azure Virtual Network. Gewenste om ervoor te zorgen dat al het verkeer naar en vanuit uw Azure Virtual Network dat toestel virtuele beveiliging doorloopt. U kunt dit doen door [Gebruiker gedefinieerd Routes](../virtual-network/virtual-networks-udr-overview.md) configureren in Azure wordt aangegeven.

[U wordt gedwongen tunneling](https://www.petri.com/azure-forced-tunneling) is een om die u gebruiken kunt om ervoor te zorgen dat uw services niet zijn toegestaan om te starten van een verbinding met apparaten op Internet. Houd er rekening mee dat deze verschilt van binnenkomende verbindingen accepteert en klik vervolgens reageert hierop toe. Front-endwebservers moeten reageren op verzoek van Internethosts en zodat Internet-die afkomstig zijn verkeer is toegestaan inkomende naar deze endwebservers en de endwebservers mogen beantwoorden.

Wat u niet wilt toestaan, is een front-end webserver om te starten van een uitgaande verzoek. Deze verzoeken mogelijk een beveiligingsrisico vertegenwoordigen, omdat deze verbindingen kunnen worden gebruikt om het downloaden van de schadelijke software. Zelfs als u dat deze front-servers om te starten uitgaande aanvragen met Internet wilt, wilt u mogelijk zorgen dat ze om te gaan via uw on-premises implementatie web proxy's, zodat u kunt profiteren van URL filteren en vastleggen.

Zou willen u in plaats daarvan afgedwongen tunneling gebruiken om te voorkomen dat dit. Wanneer u het afgedwongen tunneling inschakelt, worden alle verbindingen met Internet via uw on-premises implementatie gateway gedwongen. U kunt configureren afgedwongen tunneling door te profiteren van de gebruiker gedefinieerde Routes.

Meer informatie:

- [Wat zijn de gebruiker gedefinieerde Routes en IP-doorsturen](../virtual-network/virtual-networks-udr-overview.md)

### <a name="virtual-network-security-appliances"></a>Beveiliging van virtuele netwerkapparaten
Beveiligingsgroepen netwerk, gebruiker gedefinieerd Routes en afgedwongen tunneling zorgen voor u een beveiligingsniveau bij het netwerk- en transport lagen van het [OSI-model](https://en.wikipedia.org/wiki/OSI_model), er zijn situaties mogelijk wanneer u beveiliging wilt inschakelen op een niveau hoger zijn dan het netwerk.

Bijvoorbeeld mogelijk de beveiligingsvereisten die zijn onder andere:

- Verificatie en machtiging vóór het toestaan van toegang tot uw toepassing
- Detectie van een geopende en indringers antwoord
- Toepassing laag controle op hoog niveau protocollen
- URL's filteren
- Netwerk niveau antivirussoftware en antimalware
- Beveiliging tegen bots
- Toegangsbeheer voor toepassingen
- Aanvullende DDoS-beveiliging (boven het DDoS beveiliging de Azure stof zelf verstrekt)

U kunt deze beveiligingsfuncties uitgebreide netwerk met behulp van een partneroplossing Azure openen. U kunt de meest recente Azure partner netwerk beveiligingsoplossingen vinden door te bezoeken van de [Azure Marketplace](https://azure.microsoft.com/marketplace/) en zoeken naar 'beveiliging' en "netwerkbeveiliging".

## <a name="secure-remote-access-and-cross-premises-connectivity"></a>Veilige externe toegang en Cross Premises-connectiviteit
Setup, configuratie en beheer van uw behoeften Azure resources te worden uitgevoerd op afstand. Daarnaast wilt u mogelijk [hybride IT](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx) -oplossingen die lokale onderdelen hebben implementeren in de Azure openbare cloud. Deze scenario's vereisen veilige externe toegang.

Azure netwerken ondersteunt de volgende veilige externe toegang-scenario's:

- Individuele werkstations verbinden met een Azure Virtual Network
- Uw on-premises netwerk verbinden met een Azure virtuele netwerk met een VPN-verbinding
- Uw on-premises netwerk verbinden met een Azure virtuele netwerk met een specifieke WAN-verbinding
- Azure virtuele netwerken met elkaar verbinden

### <a name="connect-individual-workstations-to-an-azure-virtual-network"></a>Individuele werkstations verbinden met een Azure Virtual Network
Er zijn situaties mogelijk wanneer u inschakelen van afzonderlijke ontwikkelaars of bewerkingen personeel wilt voor het beheren van virtuele machines en services in Azure wordt aangegeven. Bijvoorbeeld, moet u toegang tot een virtuele machine in een netwerk met een Azure virtuele en uw beveiligingsbeleid kan geen RDP of SSH externe toegang tot afzonderlijke virtuele machines. In dit geval kunt u een VPN-verbinding punt-naar-site.

De punt-naar-site VPN-verbinding gebruikt het protocol [SSTP VPN](https://technet.microsoft.com/library/cc731352.aspx) waarmee u kunt een privé- en beveiligde verbinding tussen de gebruiker en het Azure virtuele netwerk instellen. Zodra de VPN-verbinding tot stand is gebracht, kunnen de gebruiker RDP of SSH via de VPN-verbinding in een virtuele machine op het Azure virtuele netwerk (op ervan uitgaande dat de gebruiker verifiëren kan en gemachtigd).

Meer informatie:

- [Een punt-naar-Site-verbinding met een virtueel netwerk via PowerShell configureren](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-vpn"></a>Uw On-Premises netwerk verbinden met een Azure virtuele netwerk met een VPN-verbinding
U kunt uw hele bedrijfsnetwerk of gedeelten van het verbinden met een Azure Virtual Network. Dit is in hybride IT veelvoorkomende scenario's waarin bedrijven [hun datacenter van de on-premises implementatie in Azure uitbreiden](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). In veel gevallen wordt bedrijven delen van een service in Azure en onderdelen on-premises implementatie, zoals wanneer een oplossing front-endwebservers in Azure en back-enddatabase databases on-premises bevat hosten. Deze soort "cross-premises" verbindingen Zorg ook dat beheer van Azure gevestigd resources meer beveiligen en inschakelen van scenario's zoals Active Directory-domeincontroller uit te breiden naar Azure.

Een manier om dit te doen is via een [VPN van site-naar-site](https://www.techopedia.com/definition/30747/site-to-site-vpn). Het verschil tussen een VPN-verbinding voor de site-naar-site en een VPN-punt-naar-site is een VPN-verbinding voor de punt-naar-site een één apparaat verbindt met een netwerk met Azure Virtual, terwijl een VPN-verbinding voor de site-naar-site verbindt een hele netwerk (zoals uw on-premises netwerk) met een Azure Virtual Network. Site-naar-site VPN's naar een Azure Virtual Network gebruiken de zeer veilige modus van IPsec tunnel VPN protocol.

Meer informatie:

- [Een Resource Manager VNet maken met een VPN-verbinding van het site-naar-site met behulp van de Azure-Portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Planning en ontwerp voor VPN gateway](../vpn-gateway/vpn-gateway-plan-design.md)

### <a name="connect-your-on-premises-network-to-an-azure-virtual-network-with-a-dedicated-wan-link"></a>Uw On-premises netwerk verbinden met een Azure virtuele netwerk met een specifieke WAN-verbinding
Punt-naar-site en site-naar-site VPN-verbindingen zijn geschikt voor het inschakelen van meerdere lokale connectivity. Sommige organisaties overwegen echter dat de volgende nadelen:

- VPN-verbindingen gegevens via Internet verplaatsen: dit beschrijft deze verbindingen met potentiële beveiligingskwesties die nodig zijn voor het verplaatsen van gegevens via een openbaar netwerk. Bovendien worden niet betrouwbaarheid en beschikbaarheid voor Internet-verbindingen gegarandeerd.
- VPN-verbindingen met Azure virtuele netwerken mogelijk bandbreedte beperkt voor sommige toepassingen en doeleinden, als ze max af bij rond 200Mbps worden beschouwd.

Organisaties die het hoogste niveau van beveiliging en beschikbaarheid meestal nodig voor hun cross-premises-verbindingen met specifieke WAN-verbindingen verbinding maken met externe sites. Azure vindt u de mogelijkheid om te gebruiken van een specifieke WAN-verbinding die u kunt uw on-premises netwerk verbinden door een virtueel Azure-netwerk kunt gebruiken. Dit is ingeschakeld via Azure ExpressRoute.

Meer informatie:

- [ExpressRoute technisch overzicht](../expressroute/expressroute-introduction.md)

### <a name="connect-azure-virtual-networks-to-each-other"></a>Azure virtuele netwerken met elkaar verbinden
Het is mogelijk om door u te veel Azure virtuele netwerken gebruiken voor uw implementaties. Er zijn verschillende redenen waarom u dit kunt doen. Een van de redenen mogelijk vereenvoudigen beheer; een andere mogelijk vanwege de beveiliging. Ongeacht de motivatie of waarom voor het plaatsen van resources in verschillende Azure virtuele netwerken, is het mogelijk dat er een situaties waarin u wilt dat resources op elk van de netwerken te gebruiken om te communiceren met elkaar.

Eén optie zou voor services op één Azure Virtual Network verbinding maken met services op een ander Azure virtuele netwerk door "weer touw" via Internet. De verbinding wilt starten op één Azure Virtual Network, leest u over Internet en keert u terug naar de bestemming Azure Virtual Network. Deze optie wordt de verbinding het deel uitmaken van een Internet-communicatie beveiligingskwesties weergegeven.

De optie voor een betere mogelijk een Azure virtuele VPN voor netwerk-naar-Azure virtuele Network site-naar-site maken. Deze Azure virtuele netwerk-naar-Azure virtuele netwerk naar website VPN gebruikt hetzelfde [IPsec tunnelmodus](https://technet.microsoft.com/library/cc786385.aspx) protocol als de cross-premises site-naar-site VPN-verbinding hierboven genoemde.

Het voordeel van het gebruik van een Azure virtuele netwerk-naar-Azure virtuele netwerk naar website VPN is dat de VPN-verbinding tot stand is gebracht via de stof Azure netwerk; deze verbinding geen via Internet. Zo krijgt u een extra niveau van beveiliging vergeleken met site-naar-site VPN's die verbinding via Internet maken.

Meer informatie:

- [Een verbinding VNet-naar-VNet configureren met behulp van Azure resourcemanager en PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="availability"></a>Beschikbaarheid
Beschikbaarheid is een belangrijk onderdeel van een beveiligingsprogramma. Als uw gebruikers en systemen geen toegang wat ze moeten toegang hebben tot via het netwerk tot, de service is gehackt kan worden beschouwd. Azure heeft netwerken technologieën die ondersteuning bieden voor de volgende hoge beschikbaarheid regelingen:

- HTTP-gebaseerde taakverdeling
- Netwerk niveau taakverdeling
- Globale taakverdeling

Taakverdeling is een om ontworpen voor gelijkmatig verdelen connecties tussen meerdere apparaten. De doelstellingen van taakverdeling zijn:

- Beschikbaarheid: vergroot wanneer u op meerdere apparaten balance '-verbindingen hebt geladen, een of meer van de apparaten niet beschikbaar zijn en wordt de services worden uitgevoerd op de resterende online apparaten kunnen blijven voldoen aan de inhoud van de service
- Prestaties verbeteren: wanneer u op meerdere apparaten balance '-verbindingen hebt geladen, één apparaat niet moet de treffer processor uitvoeren. In plaats daarvan de verwerking en geheugen vereisten voor het serveren van de inhoud wordt verdeeld over meerdere apparaten.

### <a name="http-based-load-balancing"></a>HTTP-gebaseerde taakverdeling
Organisaties die vaak webservices uitvoeren willen hebben van een verdeling op basis van een HTTP belasting vóór deze webservices om te zorgen dat voldoende niveaus van prestaties en betere beschikbaarheid. In tegenstelling tot traditionele netwerk gebaseerde netwerktaakverdelers, zijn de beslissingen die zijn aangebracht door HTTP gebaseerde van taakverdeling netwerktaakverdelers gebaseerd op kenmerken van de HTTP-protocol, niet op de protocollen voor netwerk- en -lagen.

Als u wilt bieden u HTTP gebaseerde van taakverdeling voor uw webservices, biedt Azure de Gateway Azure-toepassing. De Gateway Azure-toepassing worden ondersteund in:

- HTTP-gebaseerde taakverdeling – laden taakverdeling beslissingen zijn gemaakt op basis van kenmerk speciale het HTTP-protocol
- Cookie gebaseerde sessie affiniteit – deze mogelijkheid ervoor zorgt dat verbindingen naar een van de servers achter die taakverdeling tussen de client en server intact blijft. Hierdoor wordt stabiliteit van transacties gegarandeerd.
- SSL offload – wanneer een clientverbinding tot stand is gebracht met de verdeling van de belasting, dat sessie tussen de client en de taakverdeling is versleuteld met de HTTPS (SSL /) protocol. Echter om te verbeteren, hebt u de optie om de verbinding tussen de taakverdeling en de webserver achter het laden de verdeling van gebruik het protocol HTTP (leesbare). Dit is genoemd "SSL offload" omdat de endwebservers achter de taakverdeling niet de processor realiseren die nodig zijn voor versleuteling optreden en daarom kunnen serviceaanvragen sneller moet.
- URL gebaseerde inhoud routering – deze functie maakt het mogelijk voor de verdeling van de belasting beslissingen nemen op waarnaar moet worden doorgestuurd verbindingen op basis van de doel-URL. Dit biedt veel meer flexibiliteit dan oplossingen die u de werklast van taakverdeling beslissingen op basis van IP-adressen.

Meer informatie:

- [Overzicht van de Gateway toepassing](../application-gateway/application-gateway-introduction.md)

### <a name="network-level-load-balancing"></a>Netwerk niveau taakverdeling
In tegenstelling tot HTTP gebaseerde taakverdeling beslissingen netwerk niveau taakverdeling laden taakverdeling op basis van IP-adres en poort (TCP of UDP) getallen.
U kunt de voordelen van het netwerk niveau taakverdeling in Azure wordt aangegeven met behulp van taakverdeling Azure toegang. Er zijn enkele belangrijke kenmerken van de verdeling van de Azure belasting:

- Netwerk niveau taakverdeling op basis van IP-adres en poort getallen
- Ondersteuning voor elke toepassing laag protocol
- Saldi laden naar Azure virtuele machines en cloud services-exemplaren van de rol
- Kan worden gebruikt voor zowel internetgerichte (externe taakverdeling) en niet-Internet die tegenover elkaar liggen (interne taakverdeling)-toepassingen en virtuele machines
- Eindpunt monitoring, die wordt gebruikt om te bepalen als een van de services achter de taakverdeling niet beschikbaar geworden

Meer informatie:

- [Taakverdeling Internet die tegenover elkaar liggen tussen meerdere virtuele Machines of services](../load-balancer/load-balancer-internet-overview.md)
- [Overzicht van de verdeling van interne laden](../load-balancer/load-balancer-internal-overview.md)

### <a name="global-load-balancing"></a>Globale taakverdeling
Sommige organisaties wilt het hoogste niveau van de beschikbaarheid van mogelijke. Een manier om deze doelstelling hebt bereikt is host toepassingen in globaal verdeelde datacenters. Wanneer een toepassing wordt gehost in datacenters zich bevindt in de hele wereld, is deze mogelijk voor een hele geopolitieke regio niet meer beschikbaar zijn en de toepassing nog steeds omhoog en wordt uitgevoerd.

Naast de beschikbaarheid van de voordelen die u openen door toepassingen in globaal verdeelde datacenters hostingprovider, kunt u ook de prestatievoordelen van de openen. De prestatievoordelen van deze kunnen worden verkregen met behulp van de functie waarmee wordt u omgeleid zodat aanvragen voor de service met het datacenter die het dichtst bij het apparaat dat de aanvraag maakt.

Globale taakverdeling kunt bieden u beide van de volgende voordelen. In Azure wordt aangegeven, kunt u de voordelen van globale taakverdeling met behulp van Azure verkeer Manager toegang.

Meer informatie:

- [Wat is verkeer Manager?](../traffic-manager/traffic-manager-overview.md)

## <a name="logging"></a>Logboekregistratie
Logboekregistratie op een netwerkniveau is een belangrijke functie voor een waardepapier netwerk. In Azure wordt aangegeven, kunt u zich aanmeldt voor netwerk-beveiligingsgroepen om netwerkniveau logboekregistratie informatie verkregen gegevens. Met NSG logboekregistratie krijgt u informatie uit:

- Controlelogboeken bijhouden – deze logboeken worden gebruikt voor alle bewerkingen die zijn verzonden naar uw Azure abonnementen weergeven. Deze logboeken zijn standaard ingeschakeld en in de portal van Azure kunnen worden gebruikt.
- Gebeurtenislogboeken – deze logboeken bevatten informatie over welke regels NSG zijn toegepast.
- Logboeken aan de teller – deze logboeken u laten weten hoe vaak elke regel NSG is toegepast als u wilt weigeren of dat verkeer is toegestaan.

U kunt ook [Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/), een hulpmiddel van de visualisatie krachtige gegevens gebruiken om te bekijken en deze logboeken analyseren.

Meer informatie:

- [Log Analytics voor netwerk-beveiligingsgroepen (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)

## <a name="name-resolution"></a>Mailnamen omzetten
Naamresolutie is een kritieke functie voor alle services die u in Azure wordt aangegeven host. Vanuit het beveiligingsperspectief, van kan tot onbevoegden aanvragen uit uw sites aan de een website omleiden inbreuk op de naam resolutie-functie leiden. Secure naamresolutie is vereist voor al uw cloudservices die worden gehost.

Er zijn twee soorten naamresolutie u adres moet:

- Interne naamresolutie – interne naamresolutie wordt gebruikt door services op uw Azure virtuele netwerken, uw on-premises implementatie-netwerken te gebruiken of beide. Namen voor interne naamresolutie zijn niet toegankelijk via Internet. Voor een optimale beveiliging is het is belangrijk dat het schema voor het omzetten van interne naam niet toegankelijk zijn voor externe gebruikers is.
- Externe naamresolutie – externe naamresolutie wordt gebruikt door mensen en apparaten buiten uw on-premises implementatie en Azure virtuele netwerken. Hierna ziet u de namen die zijn zichtbaar met Internet en worden gebruikt om verbinding met uw services cloudgebaseerde sturen.

Voor interne naamresolutie hebt u twee opties:

- Een Azure virtuele netwerk-DNS-server – wanneer u een nieuwe Azure Virtual Network, maakt een DNS-server voor u wordt gemaakt. Deze DNS-server kunt u de namen van de zich bevindt in een netwerk met die Azure virtuele machines verhelpen. Deze DNS-server kan niet worden geconfigureerd en wordt beheerd door de manager Azure stof, waardoor deze een oplossing van een beveiligde naam resolutie.
- Breng uw eigen DNS-server: u hebt de mogelijkheid een DNS-server van uw eigen kiezen voor uw Azure virtuele netwerk plaatsen. Deze DNS-server mogelijk dat een Active Directory geïntegreerd DNS-server of een speciale DNS-server-oplossing verstrekt door een Azure partner, kunt u van Azure Marketplace.

Meer informatie:

- [Overzicht van Virtual Network](../virtual-network/virtual-networks-overview.md)
- [Beheren van DNS-Servers die worden gebruikt door een virtueel netwerk (VNet)](../virtual-network/virtual-networks-manage-dns-in-vnet.md)

Voor het omzetten door externe DNS hebt u twee opties:

- Uw eigen externe DNS-server on-premises te hosten
- Uw eigen externe DNS-server hosten bij een provider

Veel grote organisaties wordt gehost in hun eigen DNS-servers on-premises implementatie. Ze kunnen dit doen omdat ze hebben de netwerken expertise en wereldwijde aanwezigheid kunt doen.

In de meeste gevallen is het beter om het hosten van uw DNS-naam resolutie services bij een provider. Deze serviceproviders hebben netwerk expertise en wereldwijde aanwezigheid zodat deze zeker zeer hoog beschikbaar voor uw naam resolutie services. Beschikbaarheid is essentieel voor DNS-services omdat als uw naam resolutie services mislukt, kan niemand zich kunnen bereiken van uw internetverbinding services.

Azure biedt u een hoge beschikbaarheid en zodat externe DNS-oplossing in de vorm van Azure DNS. Deze oplossing met de naam van de externe resolutie maakt gebruik van de overal ter wereld Azure DNS-infrastructuur. Dit kunt u voor het hosten van uw domein in Azure met dezelfde referenties, API's, hulpmiddelen en facturering als uw andere Azure services. Als onderdeel van Azure, ook neemt de krachtige beveiliging-besturingselementen in het platform ingebouwd.

Meer informatie:

- [Azure DNS-overzicht](../dns/dns-overview.md)

## <a name="dmz-architecture"></a>DMZ architectuur
Veel enterprise organisaties gebruiken DMZs om segmenten hun netwerken te gebruiken om een buffer-zone tussen Internet en hun services te maken. Het DMZ-gedeelte van het netwerk wordt beschouwd als een laag beveiligingsniveau zone en geen activa hoogwaardige in dat netwerksegment worden geplaatst. Meestal ziet u beveiliging netwerkapparaten die u moet een netwerkinterface op het segment DMZ en een ander netwerkinterface verbonden met een netwerk met virtuele machines en -services die binnenkomende verbindingen van Internet accepteren.

Er zijn een aantal variaties van DMZ ontwerpen en het besluit tot het implementeren van een DMZ, en klik vervolgens welk type DMZ gebruiken als u toch wilt gebruiken, is gebaseerd op uw netwerk beveiligingsvereisten die zijn.

Meer informatie:

- [Microsoft-Cloudservices en netwerkbeveiliging](../best-practices-network-security.md)

## <a name="azure-security-center"></a>Azure Beveiligingscentrum
Beveiligingscentrum kunt u voorkomen dat, detecteren en reageren op threats en vindt dat u meer inzicht in, en controle over, de beveiliging van uw Azure resources. Deze biedt geïntegreerde beveiliging cmdlets voor controle en beleid verschillende uw Azure-abonnementen, helpt detecteren threats die mogelijk anders mis en werkt met een brede selectie van beveiligingsoplossingen.

Azure Beveiligingscentrum kunt optimaliseren en controleren van de beveiliging van het netwerk door:

- Aanbevelingen voor netwerk-beveiliging leveren
- De status van uw beveiliging netwerkconfiguratie bewaken
- U wordt gewaarschuwd voor netwerk gebaseerd threats, zowel op het niveau eindpunt en netwerk

Meer informatie:

- [Inleiding tot Azure Beveiligingscentrum](../security-center/security-center-intro.md)
