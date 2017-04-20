<properties
   pageTitle="Microsoft cloud services en netwerk beveiliging | Microsoft Azure"
   description="Informatie over enkele van de belangrijkste functies die beschikbaar zijn in Azure wordt aangegeven voor het maken van beveiligde netwerkomgevingen"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jonor;sivae"/>

# <a name="microsoft-cloud-services-and-network-security"></a>Microsoft cloud services en netwerk-beveiliging

Microsoft cloudservices bieden ' hyperscale '-services en -infrastructuur, enterprise-grade mogelijkheden en kiezen uit diverse opties voor hybride connectivity. Klanten kunnen kiezen voor toegang tot deze services via Internet of met Azure ExpressRoute, waarmee privé netwerkconnectiviteit. Het Microsoft Azure-platform kan klanten naadloos hun infrastructuur uitbreiden in de cloud en uit meerdere niveaus bestaande architecturen maken. Bovendien kunt derden verbeterde mogelijkheden inschakelen door te bieden van beveiligingsservices en virtuele toestellen. In dit artikel biedt een overzicht van beveiliging en architectuur problemen die klanten rekening houden moeten bij gebruik van Microsoft-cloudservices geraadpleegd via het ExpressRoute. Dit omvat tevens veiliger services in Azure virtuele netwerken maken.

## <a name="fast-start"></a>Snel starten
De volgende logica grafiek kunt u een specifiek voorbeeld van de vele beveiligingstechnieken beschikbaar voor communicatie met het Azure platform verwijzen. Zoek in het voorbeeld die het beste past bij uw hoofdletters/kleine letters voor beknopt overzicht. Voor een gedetailleerde uitleg waar, gaat u verder lezen door het papier.
![Beveiliging opties stroomdiagram][0]

[Voorbeeld 1: Maken op een netwerk omtrek (ook wel bekend als DMZ, gedemilitariseerde zone en gecontroleerd subnet) om te helpen beschermen toepassingen netwerk beveiligingsgroepen (NSGs).](#example-1-build-a-simple-dmz-with-nsgs)</br>
[Voorbeeld 2: Een omtrek-netwerk om te helpen beschermen toepassingen met een firewall en NSGs opbouwen.](#example-2-build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs)</br>
[Voorbeeld 3: Een netwerk omtrek ter bescherming van netwerken met een firewall, door gebruiker gedefinieerd route (UDR) en NSG opbouwen.](#example-3-build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg)</br>
[Voorbeeld 4: Een hybride-verbinding met een site-naar-site, virtuele toestel VPN (VPN) toevoegen.](#example-4-adding-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[Voorbeeld 5: Een hybride-verbinding met een site-naar-site, Azure gateway VPN toevoegen.](#example-5-adding-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn)</br>
[Voorbeeld 6: Een hybride-verbinding met ExpressRoute toevoegen.](#example-6-adding-a-hybrid-connection-with-expressroute)</br>
Voorbeelden voor het toevoegen van verbindingen tussen virtuele netwerken, beschikbaarheid en het koppelen van de service worden, toegevoegd aan dit document in de volgende maanden.

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Beveiliging tegen de naleving en de infrastructuur van Microsoft
Microsoft heeft een leiderschap positie ondersteunende naleving initiatieven vereist door bedrijven die u hebt gemaakt. De volgende zijn enkele van de naleving van certificaten voor Azure: ![Azure naleving badges][1]

Zie de naleving van gegevens in het [Vertrouwenscentrum in Microsoft](https://azure.microsoft.com/support/trust-center/compliance/)voor meer informatie.

Microsoft heeft een volledig methode die u wilt beveiligen met een cloudinfrastructuur nodig om uit te voeren ' hyperscale ' globale services. Microsoft cloud-infrastructuur bevat hardware, software, netwerken en administratieve en medewerkers, naast de fysieke datacenters.

![Azure beveiligingsfuncties][2]

Deze methode biedt een veiliger foundation voor klanten die hun services in de cloud Microsoft implementeren. De volgende stap is voor klanten ontwerpen en maken van een beveiligingsarchitectuur als u wilt beveiligen van deze services.

## <a name="traditional-security-architectures-and-perimeter-networks"></a>Traditionele beveiliging architecturen en omtrek netwerken
Hoewel Microsoft intensief belegt in de cloudinfrastructuur beveiligen, moeten ook klanten beveiligen hun cloudservices en resourcegroepen. Een meerdere niveaus aanpak voor beveiliging biedt de beste bescherming. Een zone omtrek netwerk beveiliging beschermen interne netwerk resources uit een niet-vertrouwde netwerk. Een netwerk omtrek verwijst naar de randen of delen van het netwerk die Internet en de beveiligde onderneming IT-infrastructuur tussen.

De core-infrastructuur is in normale enterprise-netwerken te gebruiken, intensief toegevoegd aan de verbindingen, met meerdere lagen van beveiligingsapparaten als. De rand van elke laag die bestaat uit apparaten en beleid afdwingen wordt verwezen. Apparaten kunnen zijn de volgende handelingen uit: firewalls, Distributed weigering Service (DDoS) preventie, detectie van een geopende of beveiliging systemen (id's / IP-Adressen) en VPN-apparaten. Afdwingen kan de vorm van een firewall beleidsregels, toegangsbeheerlijsten (ACL's) of specifieke routering duren. De eerste regel van beveiliging in het netwerk, rechtstreeks accepteren van binnenkomende verkeer van Internet, is een combinatie van deze regelingen aanvallen blokkeren en schadelijke verkeer terwijl verder legitiem aanvragen bij het netwerk. Deze wegen rechtstreeks aan resources in het netwerk omtrek. Deze resource kan vervolgens "" met communiceren resources in het netwerk, door de volgende grens voor validatie grondigere eerste. De buitenste laag wordt het netwerk omtrek genoemd omdat dit deel van het netwerk wordt blootgesteld aan Internet, meestal met andere vorm van de beveiliging op beide zijden. De volgende afbeelding ziet een voorbeeld van een netwerk met één subnet omtrek in een bedrijfsnetwerk, met twee beveiligingsgrenzen.

![Een netwerk omtrek in een bedrijfsnetwerk][3]

Zijn er veel architecturen gebruikt voor de implementatie van een omtrek netwerk, van een eenvoudige taakverdeling vóór een web-farm, met een netwerk met meerdere subnet omtrek met uiteenlopende regelingen bij elke grens voor verkeer blokkeren en beveiligen van de grondigere lagen van het bedrijfsnetwerk bevinden. Hoe het netwerk omtrek is gemaakt, is afhankelijk van de specifieke behoeften van de organisatie en de algehele risicotolerantie.

Wanneer de werkbelasting klanten naar openbare wolken verplaatst, is het belangrijk dat het vergelijkbare mogelijkheden ondersteuning voor de netwerkarchitectuur omtrek in Azure wordt aangegeven om te voldoen aan naleving en beveiligingsvereisten die zijn. In dit document bevat richtlijnen op hoe klanten een veilig netwerk-omgeving in Azure kunnen maken. Het bevat informatie over het netwerk omtrek, maar bevat ook een uitgebreide bespreking van vele aspecten van beveiliging van het netwerk. De volgende vragen in deze discussie kennis:

- Hoe kan een netwerk omtrek met Azure worden gemaakt?
- Wat zijn enkele van de Azure functies die beschikbaar zijn voor het maken van de omtrek en-netwerk?
- Hoe kunnen back-enddatabase werkbelasting worden beveiligd?
- Hoe worden internetcommunicatie naar de werkbelasting in Azure beheerd?
- Hoe kunnen de on-premises implementatie-netwerken worden beveiligd tegen implementaties in Azure wordt aangegeven?
- Wanneer moeten systeemeigen Azure beveiligingsfuncties in plaats van derden toestellen of services worden gebruikt?

In het volgende diagram ziet u verschillende lagen van beveiliging die Azure klanten biedt. Deze lagen zijn zowel native in het Azure platform zelf en klant gedefinieerde functies:

![De beveiligingsarchitectuur van Azure][4]

Inkomende van Internet, Azure DDoS helpt tegen grootschalige aanvallen op Azure beschermen. De volgende laag is klant gedefinieerde openbare eindpunten, die worden gebruikt om te bepalen welke verkeer via de cloudservice kunt doorgeven aan het virtuele netwerk. Systeemeigen Azure virtuele netwerk moeten worden geïsoleerd zorgt ervoor dat voltooid moeten worden geïsoleerd en alle andere netwerken en dat alleen verkeersstromen via paden van de gebruiker die is geconfigureerd en methoden. Deze paden en methoden zijn de volgende laag, waar de NSGs, UDR en virtuele netwerkapparaten kunnen worden gebruikt om te maken van beveiligingsgrenzen als u wilt beveiligen van de toepassing implementaties in het beveiligde netwerk.

De volgende sectie biedt een overzicht van Azure virtuele netwerken. Deze virtuele netwerken worden gemaakt met klanten en wat hun geïmplementeerd werkbelasting zijn verbonden met zijn. Virtuele netwerken zijn de basis van alle netwerkbeveiligingsfuncties vereist tot stand brengen van een netwerk omtrek als u wilt beveiligen met een klant-implementaties in Azure wordt aangegeven.

## <a name="overview-of-azure-virtual-networks"></a>Overzicht van Azure virtuele netwerken
Voordat de Azure virtuele netwerken internetverkeer bereiken kan, zijn er twee lagen voor waardepapier inherent aan het Azure platform:

1.  **DDoS beveiliging**: DDoS beveiliging is een laag van de Azure fysieke netwerk dat het Azure platform zelf tegen grootschalige op Internet gebaseerde aanvallen beschermen. Deze aanvallen voor het gebruik van meerdere "bots" knooppunten in een poging naar een internetservice overspoeld. Azure heeft een krachtige DDoS bescherming mesh op alle inkomende internetconnectiviteit. Deze DDoS beveiliging laag heeft geen configureerbare kenmerken van de gebruiker en is niet toegankelijk zijn voor de klant. Dit Azure beschermen als een platform van grootschalige aanvallen, maar wordt niet rechtstreeks beveiligen met een afzonderlijke Klanttoepassing. Aanvullende lagen vermogen kunnen worden geconfigureerd door de klant voor een gelokaliseerde aanval. Bijvoorbeeld als klant A is aanvallen met een grootschalige DDoS-aanval op een openbare eindpunt, voorkomen Azure dat verbindingen met deze service. Klant A kan mislukken met een ander virtuele netwerk of service-eindpunt niet nodig zijn voor de aanval om service te herstellen. Deze moet worden vermeld dat hoewel klant A kan worden beïnvloed op dat eindpunt, geen andere services buiten dat eindpunt geldt. Daarnaast ziet andere klanten en services geen gevolgen van die aanval.
2.  **Service-eindpunten**: eindpunten toestaan cloud services- of bronbestand groepen openbare Internet IP-adressen en poorten die worden aangeboden. Het eindpunt wordt NAT (Network Address Translation) gebruikt voor het routeren van verkeer naar het interne adres en poort op het Azure virtuele netwerk. Dit is de primaire pad van externe verkeer naar het virtuele netwerk binnenkomen. De service-eindpunten zijn gebruiker worden geconfigureerd om te bepalen welke verkeer wordt doorgegeven en hoe en waar deze wordt omgezet in de virtuele netwerk.

Zodra verkeer het virtuele netwerk bereikt, zijn er veel functies die een rol spelen. Azure virtuele netwerken zijn de basis voor klanten om hun werkbelasting en waar eenvoudige netwerk beveiligingsniveau is van toepassing te koppelen. Dit is een private network (een virtueel netwerk overlay) in Azure wordt aangegeven voor klanten met de volgende voorzieningen en kenmerken:
 
- **Verkeer moeten worden geïsoleerd**: een virtueel netwerk is de rand van de moeten worden geïsoleerd verkeer op het Azure platform. Virtuele machines (VMs) in een virtueel netwerk niet kunt communiceren rechtstreeks naar VMs in een ander virtueel netwerk, zelfs als beide virtuele netwerken worden gemaakt van dezelfde klant. Dit is een kritieke eigenschap die ervoor zorgt klant VMs dat en communicatie binnen een virtueel netwerk privé blijft.
- **Multitier topologie**: virtuele netwerken kunnen klanten multitier topologie definiëren door afzonderlijk adres spaties voor verschillende elementen of 'lagen' van hun werkbelasting te ontwerpen subnetten toewijzen. Deze logische groeperingen en topologieën kunnen klanten verschillende-beleid op basis van de typen werkbelasting definiëren en ook verkeersstromen tussen de lagen te beheren.
- **Cross-premises connectivity**: klanten kunnen tot stand brengen cross-premises connectiviteit tussen een virtueel netwerk en meerdere on-premises implementatie sites of andere virtuele netwerken in Azure wordt aangegeven. Hiervoor kunnen klanten Azure VPN Gateways of virtuele toestellen van derden gebruiken. Azure ondersteunt naar website (S2S) VPN's met standaard IPsec/IKE protocollen en ExpressRoute privé connectivity.
- **NSG** kunnen klanten regels (ACL's) maken op het gewenste niveau specifieker: netwerk interfaces, afzonderlijke VMs of virtuele subnetten. Klanten kunnen toestaan of weigeren van de communicatie tussen de werkbelasting binnen een virtueel netwerk, van systems in van de klant netwerken via cross-premises-connectiviteit in access bepalen of rechtstreeks internetcommunicatie.
- **UDR** en **IP-doorsturen** kunnen klanten de communicatiepaden tussen verschillende niveaus binnen een virtueel netwerk definiëren. Klanten kunnen implementeren van een firewall, -id's / IP-Adressen en andere virtuele apparaten en netwerkverkeer routeren via deze beveiligingsapparaten voor beveiliging grens wordt afgedwongen, controle en controle.
- **Virtuele netwerkapparaten** in de Azure Marketplace: beveiliging-apparaten zoals firewalls, netwerktaakverdelers en id's / IP-Adressen zijn beschikbaar in de Azure Marketplace en de afbeeldingengalerie VM. Klanten kunnen deze apparaten in hun virtuele netwerken en met name bij de beveiligingsgrenzen van hun (inclusief de omtrek en netwerk-subnetten) om te voltooien van een beveiligd netwerk-omgeving van een implementeren.

Met deze functies en mogelijkheden is een voorbeeld van hoe een netwerkarchitectuur omtrek kan worden opgebouwd in Azure wordt aangegeven in het volgende:

![Een netwerk omtrek in een Azure virtuele netwerk][5]

## <a name="perimeter-network-characteristics-and-requirements"></a>Omtrek netwerkkenmerken en vereisten
Het netwerk omtrek is bedoeld om de front-end van het netwerk, rechtstreeks in aanraking komt communicatie via Internet worden. De binnenkomende pakketten moeten gaan door de toestellen beveiliging, zoals de firewall, -id's en IP-Adressen, alvorens de back-end-servers. Internet-gebonden-pakketten van de werkbelasting kunnen ook door de toestellen beveiliging in het netwerk omtrek voor afdwingen, inspectie en controle doeleinden, voordat u het netwerk verlaat stromen. Het netwerk omtrek kunt bovendien cross-premises VPN gateways tussen klant virtuele netwerken en on-premises implementatie netwerken hosten.

### <a name="perimeter-network-characteristics"></a>Omtrek netwerkkenmerken
Verwijst naar de vorige afbeelding, zijn enkele van de kenmerken van een netwerk met een goede omtrek als volgt:

- Internetverbinding:
    - De omtrek en netwerk subnet zelf is internetgerichte, rechtstreeks communiceren met Internet.
    - Openbare IP-adressen, VIP's en/of service eindpunten doorgeven internetverkeer naar de front-netwerk en apparaten.
    - Binnenkomende verkeer van Internet passeert beveiligingsapparaten voordat u andere bronnen op het front-netwerk.
    - Als uitgaande beveiliging is ingeschakeld, passeert wordt verkeer beveiligingsapparaten, als de laatste stap, voordat u doorgeven met Internet.
- Beveiligde netwerk:
    - Er is geen directe pad van Internet naar de infrastructuur core.
    - Kanalen de infrastructuur core moeten bladeren door de beveiligingsapparaten zoals NSGs, firewalls of VPN-apparaten.
    - Andere apparaten moeten geen brug voor Internet- en de infrastructuur core.
    - Beveiligingsapparaten van de internetgerichte zowel het beveiligde netwerk die tegenover elkaar liggen grenzen van de netwerkverbindingen (bijvoorbeeld de twee firewall pictogrammen weergegeven in de vorige afbeelding) mogelijk daadwerkelijk een enkel virtuele toestel met gesplitste regels of interfaces voor elke grens. (Dat wil zeggen één apparaat, logisch gescheiden, werken met de belasting voor beide grenzen van het netwerk omtrek.)
- Andere algemene procedures en voorwaarden:
    - Werkbelasting moeten kritieke bedrijfsgegevens niet opslaan.
    - Toegang en updates voor omtrek netwerkconfiguraties en implementaties zijn beperkt tot alleen beheerders.

### <a name="perimeter-network-requirements"></a>Vereisten voor het netwerk van omtrek
Volg deze richtlijnen van virtueel netwerkvereisten voor de uitvoering van een netwerk met een succesvolle omtrek zodat deze kenmerken:

- **Subnet architectuur:** Geef het virtuele netwerk dat een hele subnet als het netwerk omtrek streeft, gescheiden van andere subnetten in hetzelfde virtuele netwerk. Dit zorgt ervoor dat het verkeer tussen het netwerk van de omtrek en andere interne of persoonlijke subnet lagen stromen via een firewall of virtuele toestel-id's / IP-Adressen op de grenzen subnet met de gebruiker gedefinieerde routes.
- **NSG:** De omtrek en netwerk subnet zelf moet zijn geopend voor communicatie toestaan met Internet, maar dat betekent niet dat moeten worden klanten NSGs overslaan. Ga als volgt algemene procedures voor beveiliging als u wilt minimaliseren van het netwerk oppervlak blootgesteld aan Internet. De externe adresbereiken voor toegang tot de implementaties of de specifieke toepassingsprotocollen en poorten die geopend zijn toegestaan vergrendelen. Er zijn mogelijk echter omstandigheden waarin dit niet altijd mogelijk is. Bijvoorbeeld als de website van een externe klanten in Azure wordt aangegeven hebt, het netwerk omtrek moet toestaan de binnenkomende web aanmeldingsaanvragen van openbare IP-adressen, maar open de web-toepassing poorten alleen: TCP:80 en TCP:443.
- **Omleiden tabel:** De omtrek en netwerk subnet zelf moet kunnen rechtstreeks communiceren met Internet, maar mag niet worden directe communicatie van en naar de vorige end of on-premises implementatie netwerken zonder te gaan via een firewall of toestel.
- **Configuratie beveiliging:** Om te controleren pakketten tussen het netwerk van de omtrek en de rest van de beveiligde netwerken, de beveiligingsapparaten zoals firewall, -id's en IP-Adressen en te routeren mogelijk apparaten multi-homed. Ze kunnen afzonderlijke NIC's voor het netwerk van de omtrek en de back-enddatabase subnetten hebben. De NIC's in het netwerk omtrek communiceren rechtstreeks naar en vanuit het Internet, met de bijbehorende NSGs en de omtrek en netwerk routeren tabel. De verbinding maken met de back-enddatabase subnetten NIC beperkte meer NSGs en routeren tabellen van de corresponderende back-enddatabase subnetten.
- **Toestel beveiligingsfunctionaliteit:** De beveiliging toestellen meestal geïmplementeerd in het netwerk van de omtrek en voert u de volgende functionaliteit:
    - Firewall: Afdwingen firewallregels of -besturingselement beleid voor de verzoeken voor oproepen.
    - Detectie en preventie gevaar: detecteren en schadelijke aanvallen van Internet te beperken.
    - Controle- en logboekregistratie: onderhouden gedetailleerde logboeken voor controle en analyse.
    - Reverse-proxyserver: de verzoeken voor oproepen omleiden naar de corresponderende back-end-servers. Dit heeft betrekking op toewijzing en meestal vertalen van het doel-adressen op de front-apparaten, firewalls, naar de adressen van de back-end-server.
    - Doorsturen van proxy: NAT leveren en uitvoering van de controle-instellingen voor de communicatie is gestart vanaf binnen het virtuele netwerk met Internet.
    - Router: Doorsturen van binnenkomende en meerdere subnet verkeer binnen het virtuele netwerk.
    - VPN-apparaat: fungeert als de cross-premises VPN gateways voor cross-premises VPN connectiviteit tussen klant on-premises implementatie netwerken en Azure virtuele netwerken.
    - VPN-server: VPN-clients verbinding maken met Azure virtuele netwerken accepteren.

>[AZURE.TIP] De volgende twee groepen afzonderlijk houden: de personen die toegang hebben tot het tandwiel voor beveiliging netwerk omtrek en de personen die zijn geautoriseerd als de beheerders van de ontwikkeling, implementatie of bewerkingen van de toepassing. Ervoor dat deze groepen afzonderlijk kunt voor een scheiding van taken en voorkomen dat één persoon zowel beveiliging van toepassingen en besturingselementen voor netwerk-beveiliging niet kan overslaan.

### <a name="questions-to-be-asked-when-building-network-boundaries"></a>Vragen om te worden gesteld wanneer het bouwen van netwerk grenzen
In deze sectie, tenzij specifiek is vermeld, de term "netwerken" verwijst naar privé Azure virtuele netwerken die zijn gemaakt door de beheerder van een abonnement. De term verwijzing geen naar de onderliggende fysieke netwerken binnen Azure.

Azure virtuele netwerken worden ook vaak gebruikt voor het uitbreiden van traditionele on-premises implementatie-netwerken te gebruiken. Het is mogelijk om te nemen aan website of ExpressRoute hybride netwerken oplossingen met omtrek netwerkarchitecturen. Dit is een belangrijk punt in het netwerk beveiligingsgrenzen samenstellen.

De volgende drie vragen zijn te beantwoorden als u een netwerk met een netwerk omtrek en meerdere beveiligingsgrenzen samenstelt kritieke.

#### <a name="1-how-many-boundaries-are-needed"></a>1) hoeveel grenzen nodig zijn?
De eerste beslissingspunt is te bepalen hoeveel beveiligingsgrenzen nodig zijn in een bepaalde scenario:

- Één plaats: op de omtrek en front-netwerk, tussen het virtuele netwerk en Internet.
- Twee grenzen: een op Internet kant van het netwerk omtrek, en een andere tussen de omtrek en netwerk subnet en de back-enddatabase subnetten die in de Azure virtuele netwerken.
- Drie grenzen: één op Internet kant van het netwerk omtrek, één tussen de omtrek en netwerk en back-enddatabase subnetten en een tussen de back-enddatabase subnetten en de on-premises netwerk.
- N grenzen: een variabele getal. Afhankelijk van de beveiligingsvereisten die zijn, moet u er echt een willekeurig aantal beveiligingsgrenzen die kunnen worden toegepast in een bepaald netwerk is.

Het aantal en het type grenzen is nodig varieert afhankelijk van het risico u bereid bent van een bedrijf en het specifieke scenario dat wordt geïmplementeerd. Dit is vaak een gemeenschappelijke beslissing die zijn aangebracht door meerdere groepen binnen een organisatie, inclusief vaak een risico en naleving team, een netwerk en platform team en het ontwikkelteam van een toepassing. Personen met kennis van beveiliging, de betreffende gegevens en de technologieën die wordt gebruikt, moet een zeg hebben in deze beschikking om ervoor te zorgen de juiste beveiliging koers voor elke implementatie.

>[AZURE.TIP] Gebruik het kleinste aantal grenzen die voldoen aan de beveiligingsvereisten die zijn voor een bepaalde situatie. Met meer grenzen moeilijker bewerkingen en probleemoplossing kunnen zijn, evenals de management realiseren die nodig zijn voor het meerdere grens beleid beheren na verloop van tijd. Echter verhogen onvoldoende grenzen risico. Zoeken naar het saldo wordt kritieke.

![Hybride netwerk met drie beveiligingsgrenzen][6]

De bovenstaande afbeelding ziet u een algemeen beeld te geven van een netwerk met drie beveiliging grens. De grenzen zijn tussen de omtrek en-netwerk en Internet, de Azure front-end en back-end privé subnetten, en het Azure back-enddatabase subnet en de on-premises implementatie-bedrijfsnetwerk.

#### <a name="2-where-are-the-boundaries-located"></a>2) waar zich de grenzen bevinden?
Als het aantal grenzen zijn besloten, is waar u ze implementeren in de volgende beslissingspunt. Er zijn in het algemeen drie opties:
- Gebruik een Internet gebaseerde tussenliggende-service (bijvoorbeeld een cloudgebaseerde Web application firewall die niet wordt beschreven in dit document)
- Gebruik van functies van systeemeigen en/of virtuele netwerkapparaten in Azure
- Gebruik van fysieke apparaten op de on-premises netwerk

De opties zijn in zuiver Azure-netwerken kan systeemeigen Azure-functies (bijvoorbeeld Azure netwerktaakverdelers) of virtuele netwerkapparaten uit het uitgebreide partner-systeem van Azure (bijvoorbeeld Check Point firewalls).

Als een begrenzing tussen Azure en een on-premises netwerk nodig is, kunnen de beveiligingsapparaten aan beide zijden van de verbinding (of beide zijden) bevinden. Dus moet een besluit worden uitgevoerd op de locatie te plaatsen beveiliging tandwiel.

In de vorige afbeelding, het netwerk Internet-naar-omtrek de voorgrond-to-back-end-grenzen geheel deel uitmaken van Azure en moeten systeemeigen Azure functies of netwerk virtuele toestellen. Beveiligingsapparaten op de rand tussen Azure (back-enddatabase subnet) en het bedrijfsnetwerk bevinden zich op de Azure kant of de zijkant van de on-premises implementatie, of zelfs een combinatie van apparaten op beide zijden. Kunnen er aanzienlijk voor- en nadelen aan beide opties die serieus worden gebruikt.

Gebruik van bestaande fysieke beveiliging tandwiel op de lokale netwerk kant bevat bijvoorbeeld het voordeel dat er geen nieuwe tandwiel nodig is. Deze moet opnieuw configureren. Het nadeel, is echter dat al het verkeer u terug van Azure naar de on-premises netwerk keert moet kunnen worden bekeken door het tandwiel voor beveiliging. Azure-naar-Azure verkeer veroorzaken dus aanzienlijk latentie en invloed toepassingsprestaties en gebruiker ervaart u, als deze is gedwongen terug naar de on-premises netwerk voor beveiliging wordt afgedwongen.

#### <a name="3-how-are-the-boundaries-implemented"></a>3) hoe worden de grenzen geïmplementeerd?
Elke beveiligingsgrens heeft waarschijnlijk de mogelijkheid van de verschillende vereisten (bijvoorbeeld-id's en firewallregels op Internet kant van de omtrek en netwerk, maar alleen ACL's tussen de omtrek en netwerk en back-enddatabase subnet). Bepalen welke apparaten u wilt gebruiken, is afhankelijk van de vereisten scenario en beveiliging. Klik in de volgende sectie bespreken voorbeelden 1, 2 en 3 bepaalde opties die kunnen worden gebruikt. Controleren van de Azure systeemeigen netwerkfuncties en de apparaten die beschikbaar zijn in Azure van de partner-systeem, ziet u de allerlei opties die beschikbaar zijn voor het oplossen van vrijwel elke scenario.

Een andere belangrijke implementatie beslissingspunt is het koppelen van de on-premises netwerk met Azure. Moet u de Azure virtuele gateway of een virtuele toestel netwerk gebruiken? Deze opties worden in de volgende sectie (voorbeelden 4, 5 en 6) uitgebreider besproken.

Daarnaast kunnen verkeer tussen virtuele netwerken binnen Azure mogelijk nodig. Deze scenario's wordt, op een later tijdstip toegevoegd.

Zodra u weet dat de antwoorden op de vorige vragen, kunt de sectie [Snel starten](#fast-start) achterhalen welke voorbeelden meest geschikt zijn voor een bepaalde scenario.

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>Voorbeelden: Beveiligingsgrenzen met Azure virtuele netwerken samenstellen
### <a name="example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs"></a>Voorbeeld 1: Een omtrek-netwerk om te helpen beschermen toepassingen met NSGs opbouwen
[Terug naar snelle start](#fast-start) | [gedetailleerde instructies voor het volgende voorbeeld samenstellen][Example1]

![Binnenkomende omtrek netwerk met NSG][7]

#### <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement dat de volgende onderdelen bevat:

- Twee cloud services: "FrontEnd001" en "BackEnd001"
- Een virtueel netwerk, "CorpNetwork", met twee subnetten: 'FrontEnd' en "BackEnd"
- Een netwerk-beveiligingsgroep die is toegepast op beide subnetten
- Een Windows-server met een webserver toepassing ("IIS01")
- Twee Windows-servers die toepassing back-end-servers ("AppVM01", "AppVM02")
- Een Windows-server met een DNS-server ("DNS01")

Voor de scripts en een resourcemanager Azure-sjabloon, raadpleegt u de [gedetailleerde opbouwen instructies][Example1].

#### <a name="nsg-description"></a>NSG beschrijving
In dit voorbeeld is een groep NSG ingebouwd en klik vervolgens met zes regels worden geladen.

>[AZURE.TIP] In het algemeen, moet u uw specifieke "Toestaan" regels maakt eerst, gevolgd door de algemene "Weigeren" regels. De opgegeven prioriteit bepaalt welke regels eerst worden geëvalueerd. Als verkeer toe te passen op een bepaalde regel wordt gevonden, wordt geen verdere regels worden geëvalueerd. NSG regels kunnen toepassen in de binnenkomende of uitgaande richting (vanuit het perspectief van het subnet).

Declaratieve, worden de volgende regels worden gemaakt voor binnenkomende verkeer:

1.  Interne DNS-verkeer (poort 53) is toegestaan.
2.  RDP-verkeer (poort 3389) van Internet naar een virtuele Machine is toegestaan.
3.  HTTP-verkeer (poort 80) van Internet naar de webserver (IIS01) is toegestaan.
4.  Een verkeer (alle poorten) van IIS01 naar AppVM1 is toegestaan.
5.  Al het verkeer (alle poorten) van Internet naar het hele virtuele netwerk (beide subnetten) is geweigerd.
6.  Een verkeer (alle poorten) van het front subnet naar de back-enddatabase subnet is geweigerd.

Met deze regels is gebonden aan elk subnet, als een HTTP-aanvraag inkomende van Internet naar de webserver, beide regels 3 is (toestaan) en 5 (weigeren) wilt toepassen. Maar omdat regel 3 een hogere prioriteit heeft, alleen zou worden toegepast en regel 5 zou niet komen afspelen. Dus de HTTP-aanvraag mogen de webserver. Als dat dezelfde verkeer is probeert te bereiken van de server DNS01, regel 5 (weigeren) zou de eerste om toe te passen en het verkeer niet mogen worden doorgegeven aan de server. Regel 6 (weigeren) blokkeert het front subnet in gesprekken met het back-enddatabase subnet (behalve toegestane verkeer in regels 1 en 4). Het netwerk back-enddatabase beveiligt u Hiermee geval onbevoegden de webtoepassing op de front-end storen. De hacker zou beperkte toegang hebben tot de back-enddatabase "beveiligde" netwerk (alleen voor resources die worden aangeboden op de server AppVM01).

Er is een standaard uitgaande regel waarmee verkeer af met Internet. In dit voorbeeld bent we uitgaand verkeer toestaan en alle uitgaande regels niet wordt gewijzigd. Als u wilt vergrendelen verkeer in beide richtingen, de gebruiker gedefinieerde routering is vereist (Zie voorbeeld 3).

#### <a name="conclusion"></a>Sluiten
Dit is een relatief eenvoudige en eenvoudige manier om het back-enddatabase subnet van binnenkomende verkeer isoleren. Voor meer informatie raadpleegt u de [gedetailleerde opbouwen instructies][Example1]. Deze instructies zijn:

- Het maken van dit netwerk omtrek met PowerShell-scripts.
- Het maken van dit netwerk omtrek met een resourcemanager Azure-sjabloon.
- Gedetailleerde beschrijvingen van elke opdracht NSG.
- Gedetailleerde verkeer stroom scenario's, met hoe verkeer is toegestaan of geweigerd in elke laag.


 ### <a name="example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs"></a>Voorbeeld 2: Een omtrek-netwerk om te helpen beschermen toepassingen met een firewall en NSGs opbouwen
[Terug naar snelle start](#fast-start) | [gedetailleerde instructies voor het volgende voorbeeld samenstellen][Example2]

![Binnenkomende omtrek netwerk met NVA en NSG][8]

#### <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement dat de volgende onderdelen bevat:

- Twee cloud services: "FrontEnd001" en "BackEnd001"
- Een virtueel netwerk, "CorpNetwork", met twee subnetten: 'FrontEnd' en "BackEnd"
- Een netwerk-beveiligingsgroep die is toegepast op beide subnetten
- Een netwerk virtuele toestel, in dit geval een firewall, verbonden met het front subnet
- Een Windows-server met een webserver toepassing ("IIS01")
- Twee Windows-servers die toepassing back-end-servers ("AppVM01", "AppVM02")
- Een Windows-server met een DNS-server ("DNS01")

Voor de scripts en een resourcemanager Azure-sjabloon, raadpleegt u de [gedetailleerde opbouwen instructies][Example2].

#### <a name="nsg-description"></a>NSG beschrijving
In dit voorbeeld is een groep NSG ingebouwd en klik vervolgens met zes regels worden geladen.

>[AZURE.TIP] In het algemeen, moet u uw specifieke "Toestaan" regels maakt eerst, gevolgd door de algemene "Weigeren" regels. De opgegeven prioriteit bepaalt welke regels eerst worden geëvalueerd. Als verkeer toe te passen op een bepaalde regel wordt gevonden, wordt geen verdere regels worden geëvalueerd. NSG regels kunnen toepassen in de binnenkomende of uitgaande richting (vanuit het perspectief van het subnet).

Declaratieve, worden de volgende regels worden gemaakt voor binnenkomende verkeer:

1.  Interne DNS-verkeer (poort 53) is toegestaan.
2.  RDP-verkeer (poort 3389) van Internet naar een virtuele Machine is toegestaan.
3.  Een internetverkeer (alle poorten) op het netwerk virtuele toestel (firewall) is toegestaan.
4.  Een verkeer (alle poorten) van IIS01 naar AppVM1 is toegestaan.
5.  Al het verkeer (alle poorten) van Internet naar het hele virtuele netwerk (beide subnetten) is geweigerd.
6.  Een verkeer (alle poorten) van het front subnet naar de back-enddatabase subnet is geweigerd.

Met deze regels is gebonden aan elk subnet, als een HTTP-aanvraag inkomende van Internet naar de firewall, beide regels 3 is (toestaan) en 5 (weigeren) wilt toepassen. Maar omdat regel 3 een hogere prioriteit heeft, alleen zou worden toegepast en regel 5 zou niet komen afspelen. Dus de HTTP-aanvraag mogen de firewall. Als dat dezelfde verkeer is probeert te bereiken van de IIS01-server, maar wel ingeschakeld het front subnet is, regel 5 (weigeren) wilt toepassen, en het verkeer niet mogen worden doorgegeven aan de server. Regel 6 (weigeren) blokkeert het front subnet in gesprekken met het back-enddatabase subnet (behalve toegestane verkeer in regels 1 en 4). Het netwerk back-enddatabase beveiligt u Hiermee geval onbevoegden de webtoepassing op de front-end storen. De hacker zou beperkte toegang hebben tot de back-enddatabase "beveiligde" netwerk (alleen voor resources die worden aangeboden op de server AppVM01).

Er is een standaard uitgaande regel waarmee verkeer af met Internet. In dit voorbeeld bent we uitgaand verkeer toestaan en alle uitgaande regels niet wordt gewijzigd. Als u wilt vergrendelen verkeer in beide richtingen, de gebruiker gedefinieerde routering is vereist (Zie voorbeeld 3).

#### <a name="firewall-rule-description"></a>De beschrijving van de regel firewall
Klik op de firewall, moet doorstuurregels worden gemaakt. Aangezien in dit voorbeeld alleen routes internetverkeer in gebonden aan de firewall en vervolgens naar de webserver, slechts één doorschakelen NAT (NAT) is regel nodig.

De doorstuurregel accepteert een inkomende bronadres dat de raakt de firewall probeert te bereiken HTTP (poort 80 of 443 voor HTTPS). Er heeft verzonden afmelden bij de lokale interface van de firewall en omgeleid naar de webserver met het IP-adres van 10.0.1.5.

#### <a name="conclusion"></a>Sluiten
Dit is een relatief eenvoudige manier voor het beveiligen van uw toepassing met een firewall en het back-enddatabase subnet van binnenkomende verkeer isoleren. Voor meer informatie raadpleegt u de [gedetailleerde opbouwen instructies][Example2]. Deze instructies zijn:

- Het maken van dit netwerk omtrek met PowerShell-scripts.
- Het maken van dit voorbeeld met een sjabloon Azure Resource Manager.
- Gedetailleerde beschrijvingen van de regel voor elke NSG voor opdrachten en parameters firewall.
- Gedetailleerde verkeer stroom scenario's, met hoe verkeer is toegestaan of geweigerd in elke laag.

### <a name="example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-udr-and-nsg"></a>Voorbeeld 3: Een netwerk omtrek ter bescherming van netwerken met een firewall, UDR en NSG opbouwen
[Terug naar snelle start](#fast-start) | [gedetailleerde instructies voor het volgende voorbeeld samenstellen][Example3]

![Bidirectionele omtrek netwerk met NVA, NSG en UDR][9]

#### <a name="environment-description"></a>Beschrijving van de omgeving
In dit voorbeeld is er een abonnement dat de volgende onderdelen bevat:

- Drie cloud services: "SecSvc001", "FrontEnd001" en "BackEnd001"
- Een virtueel netwerk, "CorpNetwork", met drie subnetten: "SecNet", 'FrontEnd' en "BackEnd"
- Een netwerk virtuele toestel, in dit geval een firewall, die zijn verbonden met het subnet SecNet
- Een Windows-server met een webserver toepassing ("IIS01")
- Twee Windows-servers die toepassing back-end-servers ("AppVM01", "AppVM02")
- Een Windows-server met een DNS-server ("DNS01")

Voor de scripts en een resourcemanager Azure-sjabloon, raadpleegt u de [gedetailleerde opbouwen instructies][Example3].

#### <a name="udr-description"></a>UDR beschrijving
Standaard worden de volgende systeem routes gedefinieerd als:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

De VNETLocal is altijd de prefix(es) gedefinieerde adres van het virtuele netwerk voor dat specifieke netwerk (dat wil zeggen deze wijzigingen uit virtueel netwerk met virtueel netwerk, afhankelijk van hoe elke specifieke virtueel netwerk wordt gedefinieerd). De resterende systeem routes zijn statische en standaard aangegeven in de tabel.

In dit voorbeeld worden twee routeren tabellen gemaakt, één voor elke de front-end en back-end-subnetten. Elke tabel is met statische routes geschikt te maken voor het opgegeven subnet geladen. In dit voorbeeld heeft elke tabel drie routes die alle verkeer (0.0.0.0/0) via de firewall (volgende hop = virtuele toestel IP-adres):

1. Lokale subnetverkeer met geen volgende Hop gedefinieerd als u wilt dat lokale subnet verkeer is toegestaan, worden omzeild de firewall.
2. Virtuele netwerkverkeer met een volgende Hop gedefinieerd als firewall; Hiermee wordt de standaardregel waarmee lokale virtuele netwerkverkeer om te leiden rechtstreeks genegeerd.
3. Alle resterende verkeer (0/0) met een volgende Hop gedefinieerd als de firewall.

>[AZURE.TIP] Niet met het lokale subnet-fragment in het UDR, worden lokale subnet communicatie afgebroken. 
> - In ons voorbeeld 10.0.1.0/24 VNETLocal aan te wijzen als anderszins, blijft van pakket de webserver (10.0.1.4) naar een andere lokale server (bijvoorbeeld) 10.0.1.25 bestemd mislukt, als ze worden verzonden via naar de NVA, waarin deze naar het subnet stuurt, kritieke is en het subnet wordt opnieuw verzenden naar de NVA enzovoort.
> - Kans om een routeren lus is meestal hoger op meerdere NIC's toestellen die rechtstreeks met elk subnet die ze communiceert, dat wil vaak van traditionele verbonden zijn, on-premises toestellen zeggen. 

Zodra de routering tabellen zijn gemaakt, zijn ze hun subnetten afhankelijk. De front subnet routeren tabel eenmaal hebt gemaakt en die is gebonden aan het subnet, er als volgt uit:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

>[AZURE.NOTE] UDR kunnen nu worden toegepast op de gateway-subnet waarop de circuitlijnen ExpressRoute is verbonden.
>
> Voorbeelden van het inschakelen van uw netwerk omtrek met ExpressRoute of site-naar-site netwerken worden weergegeven in de voorbeelden 3 en 4.


#### <a name="ip-forwarding-description"></a>Beschrijving IP-doorsturen
IP-doorschakelen is een functie companion naar UDR. Dit is een instelling op een virtuele toestel die kan worden ontvangen verkeer niet specifiek is geadresseerd aan het toestel en stuurt vervolgens dat verkeer naar de uiteindelijke bestemming.

Bijvoorbeeld als een aanvraag verkeer van AppVM01 op de server DNS01 indient, zou UDR doorsturen dit naar de firewall. Met het IP-doorsturen ingeschakeld, wordt het verkeer naar de bestemming van de DNS01 (10.0.2.4) geaccepteerd door het toestel (adres 10.0.0.4) en vervolgens worden doorgestuurd naar de uiteindelijke bestemming (10.0.2.4). Zonder het IP-doorsturen ingeschakeld op de firewall, zou verkeer niet worden geaccepteerd door het toestel Hoewel de routetabel de firewall als de volgende hop bevat. Als u wilt gebruiken een virtuele toestel, is het belangrijk moet denken om het IP-doorsturen inschakelen in combinatie met UDR.

#### <a name="nsg-description"></a>NSG beschrijving
In dit voorbeeld is een groep NSG ingebouwd en vervolgens met een enkele regel is geladen. Deze groep wordt nu gebonden alleen aan de front-end en back-end-subnetten (niet de SecNet). Declaratieve wordt de volgende regel opgebouwd:

- Al het verkeer (alle poorten) van Internet naar het hele virtuele netwerk (alle subnetten) is geweigerd.

Hoewel NSGs in dit voorbeeld worden gebruikt, is het hoofddoel als een secundaire laag beveiligd tegen handmatige onjuiste configuratie. Het doel is om alle inkomende verkeer van Internet naar de front- of back-enddatabase subnetten te blokkeren. Verkeer alleen door de SecNet subnet, de firewall moet stromen (en kiest u vervolgens, indien nodig, kunnen de front- of back-end-subnetten). Plus, met de regels UDR op hun plaats staan, zou al het verkeer die u deze in de front- of back-end-subnetten plaatst doorgestuurd uit naar de firewall (waardering UDR). De firewall ziet dit als een asymmetrische stroom en uitgaand verkeer wilt neerzetten. Er zijn dus drie lagen van beveiliging in de subnetten beschermen:
- Geen geopende eindpunten op de FrontEnd001 en BackEnd001 cloud services.
- NSGs verkeer weigeren van Internet.
- De firewall weghalen asymmetrische-verkeer is toegestaan.

Een interessant punt met betrekking tot de NSG in dit voorbeeld is dat deze slechts één regel, dat wil zeggen weigeren internetverkeer naar het hele virtuele netwerk, inclusief het subnet beveiliging bevat. Echter, aangezien het NSG is alleen gekoppeld aan de front-end en back-end-subnetten, de regel niet is verwerkt op verkeer naar het subnet beveiliging (inkomend). Hierdoor wordt verkeer flow subnet, de beveiliging.

#### <a name="firewall-rules"></a>Firewallregels
Klik op de firewall, moet doorstuurregels worden gemaakt. Aangezien de firewall geblokkeerd door of doorsturen van alle inkomend, uitgaand en binnen virtuele netwerkverkeer is, zijn er veel firewallregels nodig. De beveiligingsservice openbare IP-adres (op verschillende poorten), wordt ook, druk op alle binnenkomende verkeer om te worden verwerkt door de firewall. Een goede gewoonte is de logische loopt voordat u de subnetten instelt diagram te en later Herschrijf de firewallregels, om te voorkomen. De volgende afbeelding is een logische weergave van de firewallregels voor dit voorbeeld:
 
![Logische weergave van de firewallregels][10]

>[AZURE.NOTE] Op basis van het netwerk virtuele toestel gebruikt, varieert de poorten management. In dit voorbeeld wordt een Barracuda NextGen Firewall verwezen, wordt die poorten 22, 801 en 807 gebruikt. Raadpleeg de documentatie van de leverancier toestel als u wilt zoeken naar de exacte poorten gebruikt voor het beheer van het apparaat wordt gebruikt.

#### <a name="firewall-rules-description"></a>Beschrijving van de regels firewall
De beveiliging subnet wordt niet weergegeven in de voorgaande logische diagram. Dit komt doordat de firewall het enige resource op dat subnet is, en dit diagram wordt weergegeven de firewallregels en hoe ze logisch toestaan of verkeersstromen, niet de werkelijke routering pad weigeren. Daarnaast de externe poorten geselecteerd voor de RDP-verkeer hoger zijn varieerde poorten (8014 – 8026) en zijn geselecteerd enigszins uitlijnen met de laatste twee achttallen van het lokale IP-adres voor gemakkelijker leesbaarheid (bijvoorbeeld lokale serveradres 10.0.1.4 is gekoppeld aan externe poort 8014). Een niet-conflicterende hoger poorten en-echter kunnen worden gebruikt.

In dit voorbeeld moeten we zeven soorten regels:

- Externe regels (voor binnenkomende verkeer):
  1.    Regel voor het beheer van Firewall: deze App omleiden regel staat verkeer worden doorgegeven aan de poorten beheer van het netwerk virtuele toestel.
  2.    Regels voor RDP (voor elke Windows-server): deze vier regels (één voor elke server) beheer van de verschillende servers via RDP toestaan. Dit kan ook worden samengevoegd in één regel, afhankelijk van de mogelijkheden van het netwerk virtuele toestel die wordt gebruikt.
  3.    Regels voor het verkeer van toepassing: Er zijn twee van deze regels voor de eerste voor de front-webverkeer en de tweede voor het back-enddatabase verkeer (bijvoorbeeld webserver naar gegevenslaag). De configuratie van deze regels is afhankelijk van de netwerkarchitectuur (waar uw servers worden geplaatst) en verkeer loopt (welke richting de verkeersstromen en welke poorten worden gebruikt).
      - De eerste regel kan het verkeer werkelijke toepassing de toepassingsserver te bereiken. Terwijl de andere regels toestaan voor beveiliging en beheer, wordt verkeer regels van toepassing zijn wat toestaan van externe gebruikers of services voor toegang tot de toepassingen. Voor dit voorbeeld is één webserver op poort 80. Een regel van de toepassing één firewall wordt dus binnenkomende verkeer omgeleid naar de externe IP-adres, het web servers interne IP-adres. De sessie omgeleide verkeer zou doen via NAT worden omgezet in de interne server.
      - De tweede regel is de back-enddatabase regel toe te staan dat de webserver contact opnemen met de AppVM01-server (maar niet AppVM02) via een poort.
- Interne regels (voor binnen virtuele netwerkverkeer)
  4.    Uitgaande op Internet regel: met deze regel staat verkeer van een netwerk worden doorgegeven aan de geselecteerde netwerken. Deze regel is meestal een standaardregel al op de firewall, maar in de fase uitgeschakeld. Deze regel moet worden ingeschakeld voor dit voorbeeld.
  5.    DNS-regel: met deze regel alleen DNS (poort 53) verkeer worden doorgegeven aan de DNS-server is toegestaan. Voor deze omgeving is meeste verkeer van de front-end naar de back-end geblokkeerd. Deze regel kunt specifiek DNS bij een lokale subnet.
  6.    Subnet, subnet regel: met deze regel is toe te staan dat een server in de back-enddatabase subnet verbinding maken met een server op het front subnet (maar niet andersom).
- Foutveilige regel (voor verkeer die niet voldoet aan een van de vorige):
  7.    Alle verkeer regel weigeren: dit moet altijd de laatste regel (in prioriteit) en als zodanig als netwerkverkeer niet overeenkomt met de voorafgaand aan de regels van deze door deze regel wordt verwijderd. Dit is een standaardregel en meestal geactiveerd. Geen wijzigingen zijn in het algemeen nodig.

>[AZURE.TIP] Klik op de tweede regel in de verkeer toepassing, om te vereenvoudigen dit voorbeeld is elke poort toegestaan. In een scenario voor reële moeten de meest specifieke poorten en -adresbereiken worden gebruikt om te verminderen aanvallen van deze regel.

Zodra alle vorige regels zijn gemaakt, is het belangrijk om te controleren van de prioriteit van elke regel om ervoor te zorgen verkeer wordt toegestaan of geweigerd naar wens. In dit voorbeeld worden de regels in de volgorde van prioriteit.

#### <a name="conclusion"></a>Sluiten
Dit is een manier om meer complexe maar vollediger beveiligen en te isoleren van het netwerk dan de voorgaande Column. (Voorbeeld 2 wordt alleen de toepassing beveiligd en voorbeeld 1 geïsoleerd net subnetten). Dit ontwerp kan voor verkeer in beide richtingen bewaken en beschermen niet alleen de binnenkomende toepassingsserver maar netwerk wordt beveiligingsbeleid afgedwongen voor alle servers in dit netwerk. Ook, afhankelijk van het apparaat gebruikt, volledige verkeer controle en chatstatus kunnen worden bereikt. Voor meer informatie raadpleegt u de [gedetailleerde opbouwen instructies][Example3]. Deze instructies zijn:

- Het maken van dit netwerk zoals omtrek met PowerShell-scripts.
- Het maken van dit voorbeeld met een sjabloon Azure Resource Manager.
- Gedetailleerde beschrijvingen van elke UDR, NSG opdracht en om de firewallregel.
- Gedetailleerde verkeer stroom scenario's, met hoe verkeer is toegestaan of geweigerd in elke laag.

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-virtual-private-network-vpn"></a>Voorbeeld 4: Een hybride verbinding met een site-naar-site, virtuele toestel VPN (VPN) toevoegen
[Terug naar snelle start](#fast-start) | Gedetailleerde opbouwen instructies beschikbaar binnenkort

![Omtrek netwerk met NVA verbonden hybride netwerk][11]

#### <a name="environment-description"></a>Beschrijving van de omgeving
Hybride netwerken met een netwerk virtuele toestel (NVA) kan worden toegevoegd aan de omtrek en typen die worden beschreven in de voorbeelden 1, 2 of 3.

Zoals u ziet in de vorige afbeelding, wordt een VPN-verbinding via Internet (site-naar-site) wordt gebruikt om verbinding maken met een on-premises netwerk met een Azure virtuele netwerk via een NVA.

>[AZURE.NOTE] Als u ExpressRoute met de optie Azure openbare Peering is ingeschakeld gebruikt, kan een statische route moet worden gemaakt. Dit moet doorsturen naar het NVA VPN IP-adres van uw bedrijf Internet en niet via de ExpressRoute WAN. De NAT nodig op de optie ExpressRoute Azure openbare Peering kan de sessie VPN verbreken.

Als u de VPN-verbinding hebt op hun plaats staan, wordt de NVA de centrale hub voor alle netwerken en subnetten. De firewall doorstuurregels bepalen welke verkeersstromen zijn toegestaan, via NAT worden omgezet, worden omgeleid of worden niet weergegeven (zelfs voor verkeersstromen tussen de on-premises netwerk en Azure).

Verkeersstromen moeten zorgvuldig, als deze kunnen worden geoptimaliseerd of niet beschikbaar is weergegeven door deze ontwerppatroon, afhankelijk van de specifieke use-case worden beschouwd.

Gebruik van de omgeving die is ingebouwd in voorbeeld 3 en vervolgens toe te voegen een site-naar-site VPN hybride netwerkverbinding, genereert het ontwerp van de volgende:

![Omtrek netwerk met NVA verbonden met een VPN-verbinding voor de site-naar-site][12]

De router on-premises implementatie, of een ander netwerkapparaat die compatibel is met uw NVA voor VPN, zou de VPN-client. Dit apparaat fysiek zou verantwoordelijk voor het starten en onderhouden van de VPN-verbinding met uw NVA.

Logisch naar de NVA, het netwerk ziet er zo vier afzonderlijke 'beveiligingszones' met de regels op de NVA wordt de primaire directeur van verkeer tussen deze zones:

![Domainexplorer-bp NVA vanuit het perspectief van][13]

#### <a name="conclusion"></a>Sluiten
Het toevoegen van een site-naar-site VPN hybride netwerk-verbinding met een Azure virtuele netwerk kunt de on-premises netwerk uitbreiden naar Azure op een veilige manier. Klik in via een VPN-verbinding kan uw verkeer is versleuteld en stuurt via Internet. De NVA in dit voorbeeld is een centrale locatie om af te dwingen het beveiligingsbeleid beheren. Zie de instructies voor gedetailleerde opbouwen (binnenkort uitgebracht) voor meer informatie. Deze instructies zijn:

- Het maken van dit netwerk zoals omtrek met PowerShell-scripts.
- Het maken van dit voorbeeld met een sjabloon Azure Resource Manager.
- Gedetailleerde verkeer stroom scenario's, met hoe verkeersstromen via dit ontwerp.

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn"></a>Voorbeeld 5: Een hybride verbinding met een site-naar-site, Azure gateway VPN toevoegen
[Terug naar snelle start](#fast-start) | Gedetailleerde opbouwen instructies beschikbaar binnenkort

![Omtrek netwerk met gateway verbonden hybride netwerk][14]

#### <a name="environment-description"></a>Beschrijving van de omgeving
Hybride netwerken via een VPN-Azure-gateway kan worden toegevoegd aan beide omtrek netwerk typen die worden beschreven in de voorbeelden 1 of 2.

Zoals u in de bovenstaande afbeelding, wordt een VPN-verbinding via Internet (site-naar-site) gebruikt om verbinding maken met een on-premises netwerk met een Azure virtuele netwerk via een VPN-Azure-gateway.

>[AZURE.NOTE] Als u ExpressRoute met de optie Azure openbare Peering is ingeschakeld gebruikt, kan een statische route moet worden gemaakt. Dit moet doorsturen naar het NVA VPN IP-adres van uw bedrijf Internet en niet via de ExpressRoute WAN. De NAT nodig op de optie ExpressRoute Azure openbare Peering kan de sessie VPN verbreken.

De volgende afbeelding ziet de twee netwerk randen in deze optie. Klik op de eerste rand bepalen de NVA en NSGs verkeersstromen tussen Azure en Internet en voor binnen Azure-netwerken te gebruiken. De tweede rand is de gateway Azure VPN, dat wil de rand van een volledig afzonderlijk en geïsoleerd netwerk tussen on-premises en Azure zeggen.

Verkeersstromen moeten zorgvuldig, als deze kunnen worden geoptimaliseerd of niet beschikbaar is weergegeven door deze ontwerppatroon, afhankelijk van de specifieke use-case worden beschouwd.

Gebruik van de omgeving die is ingebouwd in voorbeeld 1 en vervolgens toe te voegen een site-naar-site VPN hybride netwerkverbinding, genereert het ontwerp van de volgende:

![Omtrek netwerk met gateway verbonden met een ExpressRoute-verbinding][15]

#### <a name="conclusion"></a>Sluiten
Het toevoegen van een site-naar-site VPN hybride netwerk-verbinding met een Azure virtuele netwerk kunt de on-premises netwerk uitbreiden naar Azure op een veilige manier. Met de systeemeigen Azure VPN gateway, uw verkeer is IPSec versleuteld en stuurt via Internet. Het gebruik van de gateway Azure VPN kan ook een lagere kosten-optie (geen extra licenties kosten net als bij derde partij NVAs) bieden. Dit is voordeligste in voorbeeld 1, waarin geen NVA wordt gebruikt. Zie de instructies voor gedetailleerde opbouwen (binnenkort uitgebracht) voor meer informatie. Deze instructies zijn:

- Het maken van dit netwerk zoals omtrek met PowerShell-scripts.
- Het maken van dit voorbeeld met een sjabloon Azure Resource Manager.
- Gedetailleerde verkeer stroom scenario's, met hoe verkeersstromen via dit ontwerp.

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>Voorbeeld 6: Een hybride verbinding met ExpressRoute toevoegen
[Terug naar snelle start](#fast-start) | Gedetailleerde opbouwen instructies beschikbaar binnenkort

![Omtrek netwerk met gateway verbonden hybride netwerk][16]

#### <a name="environment-description"></a>Beschrijving van de omgeving
Hybride netwerken met een ExpressRoute privé peering verbinding kan worden toegevoegd aan beide omtrek netwerk typen die worden beschreven in de voorbeelden 1 of 2.

Zoals u in de bovenstaande afbeelding, biedt ExpressRoute privé peering een directe verbinding tussen uw on-premises netwerk en het Azure virtuele netwerk. Verkeer transits alleen de serviceprovider netwerk en het Microsoft Azure-netwerk, nooit gebruik aanraken Internet.

>[AZURE.NOTE] Er zijn bepaalde beperkingen wanneer u UDR met ExpressRoute, vanwege de complexiteit van dynamische routering gebruikt in de Azure virtuele gateway. Hierna ziet u als volgt:
>
>- UDR moeten niet worden toegepast op de gateway-subnet waarop de ExpressRoute gekoppelde Azure virtuele gateway is verbonden.
>- De ExpressRoute gekoppelde Azure virtuele gateway mogen niet het apparaat NextHop voor andere UDR subnetten die is gebonden.
>
>
<br />

>[AZURE.TIP] ExpressRoute zakelijke netwerkverkeer mensen uit de Internet voor een betere beveiliging blijft en aanzienlijk betere prestaties. Daarnaast kunnen serviceovereenkomsten van uw provider ExpressRoute. De Gateway Azure kunt maximaal 2 Gb/s met ExpressRoute, doorgeven dat met site-naar-site VPN's, de maximumdoorvoer Azure Gateway 200 Mb/s is.

Zoals u in het volgende diagram, met deze optie heeft de omgeving nu twee netwerk randen. De NVA en NSG bepalen verkeersstromen voor binnen Azure-netwerken te gebruiken en tussen Azure en Internet, terwijl de gateway de rand van een volledig afzonderlijk en geïsoleerd netwerk tussen on-premises en Azure is.

Verkeersstromen moeten zorgvuldig, als deze kunnen worden geoptimaliseerd of niet beschikbaar is weergegeven door deze ontwerppatroon, afhankelijk van de specifieke use-case worden beschouwd.

Gebruik van de omgeving die is ingebouwd in voorbeeld 1 en vervolgens toe te voegen een netwerkverbinding ExpressRoute voor hybride, genereert het ontwerp van de volgende:

![Omtrek netwerk met gateway verbonden met een ExpressRoute-verbinding][17]

#### <a name="conclusion"></a>Sluiten
Het toevoegen van een netwerkverbinding ExpressRoute privé Peering kunt de on-premises netwerk uitbreiden naar Azure in een beveiligd, onderste latentie, hoger uitvoering wijze. Bovendien de systeemeigen Azure-Gateway, zoals in dit voorbeeld beschikt u over een lagere kosten-optie (geen extra licenties bij derde partij NVAs). Zie de instructies voor gedetailleerde opbouwen (binnenkort uitgebracht) voor meer informatie. Deze instructies zijn:

- Het maken van dit netwerk zoals omtrek met PowerShell-scripts.
- Het maken van dit voorbeeld met een sjabloon Azure Resource Manager.
- Gedetailleerde verkeer stroom scenario's, met hoe verkeersstromen via dit ontwerp.

## <a name="references"></a>Verwijzingen
### <a name="helpful-websites-and-documentation"></a>Handige websites en documentatie
- Access Azure met Azure resourcemanager:
- Toegang krijgen tot het Azure met PowerShell: [https://azure.microsoft.com/documentation/articles/powershell-install-configure/](./powershell-install-configure.md)
- Virtuele netwerken documentatie: [https://azure.microsoft.com/documentation/services/virtual-network/](https://azure.microsoft.com/documentation/services/virtual-network/)
- Beveiliging groep documentatie netwerk: [https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/](./virtual-network/virtual-networks-nsg.md)
- Door gebruiker gedefinieerde routeren documentatie: [https://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/](./virtual-network/virtual-networks-udr-overview.md)
- Azure virtuele gateways: [https://azure.microsoft.com/documentation/services/vpn-gateway/](https://azure.microsoft.com/documentation/services/vpn-gateway/)
- [Https://azure.microsoft.com/documentation/articles/vpn-gateway-create-site-to-site-rm-powershell](./vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) naar website VPN's:
- ExpressRoute documentatie (Zorg ervoor dat de secties 'Aan de slag' en "How To" uitchecken): [https://azure.microsoft.com/documentation/services/expressroute/](https://azure.microsoft.com/documentation/services/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "Beveiliging opties stroomdiagram"
[1]: ./media/best-practices-network-security/compliancebadges.png "Azure naleving Badges"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "Azure beveiligingsfuncties"
[3]: ./media/best-practices-network-security/dmzcorporate.png "Een DMZ in een bedrijfsnetwerk"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "De beveiligingsarchitectuur van Azure"
[5]: ./media/best-practices-network-security/dmzazure.png "Een DMZ in een Azure Virtual Network"
[6]: ./media/best-practices-network-security/dmzhybrid.png "Hybride netwerk met drie beveiligingsgrenzen"
[7]: ./media/best-practices-network-security/example1design.png "Binnenkomende DMZ met NSG"
[8]: ./media/best-practices-network-security/example2design.png "Binnenkomende DMZ met NVA en NSG"
[9]: ./media/best-practices-network-security/example3design.png "Bidirectionele DMZ met NVA, NSG en UDR"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "Logische weergave van de firewallregels"
[11]: ./media/best-practices-network-security/example4designoptions.png "DMZ met NVA verbonden hybride netwerk"
[12]: ./media/best-practices-network-security/example4designs2s.png "DMZ met NVA die zijn verbonden met een VPN-verbinding voor de Site-naar-Site"
[13]: ./media/best-practices-network-security/example4networklogical.png "Domainexplorer-bp NVA vanuit het perspectief van"
[14]: ./media/best-practices-network-security/example5designoptions.png "DMZ met Azure Gateway verbonden naar website hybride netwerk"
[15]: ./media/best-practices-network-security/example5designs2s.png "DMZ met Azure Gateway via VPN van Site-naar-Site"
[16]: ./media/best-practices-network-security/example6designoptions.png "DMZ met Azure Gateway verbonden ExpressRoute hybride netwerk"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "DMZ met Azure Gateway met behulp van een verbinding ExpressRoute"

<!--Link References-->
[Example1]: ./virtual-network/virtual-networks-dmz-nsg-asm.md
[Example2]: ./virtual-network/virtual-networks-dmz-nsg-fw-asm.md
[Example3]: ./virtual-network/virtual-networks-dmz-nsg-fw-udr-asm.md
[Example4]: ./virtual-network/virtual-networks-hybrid-s2s-nva-asm.md
[Example5]: ./virtual-network/virtual-networks-hybrid-s2s-agw-asm.md
[Example6]: ./virtual-network/virtual-networks-hybrid-expressroute-asm.md
[Example7]: ./virtual-network/virtual-networks-vnet2vnet-direct-asm.md
[Example8]: ./virtual-network/virtual-networks-vnet2vnet-transit-asm.md
