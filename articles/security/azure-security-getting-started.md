<properties
   pageTitle="Aan de slag met Microsoft Azure beveiliging | Microsoft Azure"
   description="Dit artikel bevat een overzicht van de mogelijkheden van Microsoft Azure beveiliging en algemene overwegingen voor organisaties die hun activa naar een cloud-provider migreert."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/19/2016"
   ms.author="yuridio"/>

#<a name="getting-started-with-microsoft-azure-security"></a>Aan de slag met Microsoft Azure-beveiliging

Bij het maken of IT-activa migreren naar een cloud-provider, kunt u zijn te vertrouwen op de mogelijkheden van de organisatie de toepassingen en de gegevens die u naar hun services en de beveiliging besturingselementen verkrijgt u om te bepalen de beveiliging van uw activa cloudgebaseerde belasten te beschermen.

Van Azure-infrastructuur is ontworpen van de functie voor toepassingen voor miljoenen klanten tegelijk hostingprovider en biedt de betrouwbare basis waarop bedrijven hun beveiliging behoeften kunnen. Daarnaast biedt Azure u een groot aantal configureerbare beveiligingsopties en de mogelijkheid om te bepalen ze zodat u kunt de beveiliging om te voldoen aan de unieke vereisten voor uw implementaties aanpassen.

In dit overzichtsartikel op Azure beveiliging besproken:

-   Azure-services en functies die u gebruiken kunt om te helpen beveiligen van uw services en gegevens binnen Azure

-   Hoe Microsoft beveiligt de Azure infrastructuur beveiligen van uw gegevens en toepassingen

##<a name="identity-and-access-management"></a>Identiteits-en access


Toegang tot de IT-infrastructuur, gegevens en -toepassingen beheren is kritieke. Deze mogelijkheden worden in Microsoft Azure geleverd door services zoals Azure Active Directory, Azure Storage en ondersteuning voor veel standaarden en API's.

[Azure Active Directory](../active-directory/active-directory-whatis.md) (Azure AD) is een identiteit opslagplaats en engine waarmee verificatie, autorisatie en toegangsbeheer voor gebruikers, groepen en objecten van een organisatie. Azure AD biedt ontwikkelaars ook een efficiënte manier om het integreren van identiteitsbeheer in hun toepassingen. Industriële standaard protocollen zoals [SAML 2.0](https://en.wikipedia.org/wiki/SAML_2.0), [WS-Federation](https://msdn.microsoft.com/library/bb498017.aspx)en [OpenID verbinding](http://openid.net/connect/) maakt mogelijk aanmeldingsproblemen op allerlei platforms zoals .NET, Java, Node.js en PHP.

De grafiek op basis van een REST API kunnen ontwikkelaars te lezen en schrijven naar de map van elk platform. Tot en met ondersteuning voor [OAuth 2.0](http://oauth.net/2/), ontwikkelaars mobiele kunnen maken en webtoepassingen die integreren met Microsoft en derden web API's en hun eigen secure web API's. Bron openen clientbibliotheken zijn beschikbaar voor .net-, Windows Store-, iOS- en android-apparaat met extra bibliotheken gewerkt.

### <a name="how-azure-enables-identity-and-access-management"></a>Hoe Azure kunt identiteits-en access

Azure AD kan worden gebruikt als een zelfstandig product cloud-map voor uw organisatie of als een geïntegreerde oplossing met uw bestaande lokale Active Directory. Sommige functies zijn directory-synchronisatie en eenmalige aanmelding (SSO). Deze het bereik van uw bestaande on-premises implementatie identiteiten uitbreiden in de cloud en de beheerders en eindgebruikers te verbeteren.

Enkele andere mogelijkheden voor identiteits-en toegang zijn:

-   Azure AD kunt [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) SaaS-toepassingen, ongeacht waar deze worden gehost. Sommige-toepassingen zijn gekoppeld aan Azure AD en anderen wachtwoord eenmalige aanmelding gebruiken. Federatieve toepassingen ondersteunt soms ook inrichting van de gebruiker en wachtwoord vaulting.

-   Toegang tot gegevens in [Azure opslag](https://azure.microsoft.com/services/storage/) wordt beheerd via verificatie. Elke opslag-Account heeft een primaire sleutel ([Opslag Accountsleutel](https://msdn.microsoft.com/library/azure/ee460785.aspx)of SAK) en een secundaire geheime sleutel (de Access-handtekening gedeeld of SA's).

-   Azure AD biedt identiteit als een Service via federatie ( [Active Directory Federation Services](../active-directory/fundamentals-identity.md), synchronisatie en replicatie gebruiken met on-premises implementatie-mappen.

-   [Azure meervoudige verificatie MFA ()](../multi-factor-authentication/multi-factor-authentication.md) is de meervoudige verificatie-service die, moeten gebruikers ook controleren of aanmeldingen met een mobiele app, telefoongesprek of SMS-bericht. Dit is beschikbaar voor gebruik met Azure AD Beveilig lokale bronnen met de Server Azure-MFA, en aangepaste toepassingen en mappen met de SDK.

-   [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) kunt u Azure virtuele machines toevoegen aan een domein zonder te hoeven domeincontrollers distribueren. Gebruikers kunnen aanmelden bij deze virtuele machines met hun zakelijke Active Directory-referenties en beheren van een domein behoren virtuele machines groepsbeleid afdwingen van beveiliging basislijnen op al uw Azure virtuele machines.

-   [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) biedt u een hoge beschikbaarheid, globale, identiteit management service voor consumenten gerichte toepassingen die wordt aangepast aan honderden miljoenen identiteiten. Dit kan worden geïntegreerd in mobile- en web platforms. Uw consumenten kunnen aanmelden bij al uw toepassingen via aanpasbare ervaringen met behulp van hun bestaande accounts van sociale of door te maken van nieuwe referenties.

##<a name="data-access-control-and-encryption"></a>Toegangsbeheer voor gegevens en versleuteling

Microsoft maakt gebruik van de beginselen van scheiding van taken en [Laagste bevoegdheidsniveau](https://en.wikipedia.org/wiki/Principle_of_least_privilege) overal in Azure bewerkingen. Toegang tot gegevens Azure medewerkers uw expliciete toestemming vereist en is verleend op een "just-in-time"-basis die is geregistreerd en worden gecontroleerd, klikt u vervolgens ingetrokken na voltooiing van de betrokkenheid.

Daarnaast biedt Azure meerdere mogelijkheden voor het beschermen van gegevens in overdracht en in rust, met inbegrip van versleuteling voor gegevens, bestanden, toepassingen, services, communicatie en stations. U hebt de optie voor het coderen van gegevens voor het plaatsen in Azure wordt aangegeven, evenals toetsen opslaan in uw on-premises implementatie-datacenters.

![Microsoft Antimalware in Azure wordt aangegeven](./media/azure-security-getting-started/sec-azgsfig1.PNG)

### <a name="azure-encryption-technologies"></a>Azure-versleuteling technologieën

U kunt details op beheerderstoegang in uw omgeving abonnement verzamelen met behulp van [Azure AD-rapportage](../active-directory/active-directory-reporting-audit-events.md). Hebt u de optie [BitLocker station codering](https://technet.microsoft.com/library/cc732774.aspx) configureren op VHD's met vertrouwelijke informatie in Azure wordt aangegeven.

Andere mogelijkheden in Azure wordt aangegeven dat u helpen kan bij het beveiligen van uw gegevens bevatten:

-   Softwareontwikkelaars kunnen versleuteling in de toepassingen die ze implementeren in Azure maken met het Windows [CryptoAPI](https://msdn.microsoft.com/library/ms867086.aspx) en .NET Framework.

- Client kant versleuteling voor Microsoft-blobopslag kunt u de toetsen volledig beheren.  De storage-service is nooit ziet de toetsen en geen decoderen van de gegevens.

-   [Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) (met de [RMS SDK](https://msdn.microsoft.com/library/dn758244.aspx) biedt bestands- en gegevensniveau versleuteling en preventie van gegevensuitvoering lek tot en met toegangsbeheer op basis van het beleid.

-   Azure ondersteunt [tabel- en kolom niveau codering (TDE/wissen)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database.aspx) in SQL Server virtuele Machines, en ondersteunt derden on-premises key management servers in klanten datacenters.

-   Opslag Account toetsen, gedeeld Access handtekeningen management certificaten en andere sleutels zijn uniek voor elke Azure tenant.

-   Azure [StorSimple](http://www.microsoft.com/server-cloud/products/storsimple/overview.aspx) hybride opslag versleutelt gegevens via 128-bit openbare / privé combinatie van een sleutel voordat u deze met Azure Storage uploaden.

-   Azure ondersteunt en veel methoden, met inbegrip van SSL/TLS, IPsec en AES, afhankelijk van de gegevenstypen en containers en transport gebruikt.

##<a name="virtualization"></a>Virtualization

De Azure platform gebruikt een gevirtualiseerde omgeving. Gebruikersexemplaren fungeren als zelfstandige virtuele machines waarvoor geen toegang tot een fysieke hostserver en deze moeten worden geïsoleerd wordt afgedwongen fysieke [processor (ring-0/ring-3) bevoegdheidsniveaus](https://en.wikipedia.org/wiki/Protection_ring).

Bellen 0 is het meest bevoegdheden en 3 de minste. De Gast OS wordt uitgevoerd in een lagere rechten bellen 1 en toepassingen in de laatste bevoegdheden bellen 3. Deze virtualization van fysieke bronnen leidt tot een duidelijke scheiding tussen Gast OS en hypervisor, waardoor extra beveiliging scheiding tussen de twee.

Van Azure Hypervisor fungeert als een micro-kernel en alle hardware toegangsaanvragen uit Gast VMs worden doorgegeven aan de host voor de verwerking van met een gedeeld geheugen interface VMBus genoemd. Hiermee voorkomt u dat gebruikers onbewerkte lezen/schrijven/uitvoeren toegang krijgen tot het systeem en verkleint u het risico van het delen van systeembronnen.

![Microsoft Antimalware in Azure wordt aangegeven](./media/azure-security-getting-started/sec-azgsfig2.PNG)

### <a name="how-azure-implements-virtualization"></a>Hoe Azure virtualization implementeert

Azure gebruikt een firewall, hypervisor (pakketfilter), die is geïmplementeerd in de hypervisor en geconfigureerd door een agent van de controller stof. Hiermee kunt tenants beveiligen tegen ongeoorloofde toegang. Standaard wanneer een VM is gemaakt, al het verkeer is geblokkeerd en klikt u vervolgens de stof controller-agent configureert voor het pakketfilter om toe te voegen *regels en uitzonderingen* als u wilt dat geautoriseerde verkeer is toegestaan.

Er zijn twee soorten regels die zijn geprogrammeerde hier:

-   **Configuratie van de computer of regels voor infrastructuur**: standaard alle communicatie is geblokkeerd. Er zijn uitzonderingen toe te staan dat een VM verzenden en ontvangen van DHCP en DNS-verkeer is toegestaan. VMs kunnen ook verkeer naar "openbare" internet verzenden en verkeer naar andere VMs binnen de cluster en OS activering server verzenden. De VMs toegestane lijst met uitgaande bestemmingen bevat geen Azure router subnetten, Azure management back-end en andere Microsoft-eigenschappen.

-   **Configuratiebestand rol**: Hiermee definieert u de binnenkomende ACL's op basis van de service-model de tenants. Bijvoorbeeld als een tenant een front Web poort 80 op een bepaalde VM bevat, klikt u vervolgens Azure geopend TCP-poort 80 naar alle IP-adressen als u bent een eindpunt in het model [Beheer van Azure-Service](../resource-manager-deployment-model.md) configureren. Als de VM een back-end of werknemer rol uitgevoerd heeft, klikt u vervolgens openen we de rol werknemer alleen voor VM binnen dezelfde tenant.

##<a name="isolation"></a>Moeten worden geïsoleerd

Onderhouden scheiding om te voorkomen dat onbevoegden en onbedoeld overdracht van gegevens tussen implementaties in een gedeelde meerdere tenant-architectuur is een andere belangrijke cloud security behoefte.

Azure implementeert [netwerktoegang](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/) en scheiding tot en met VLAN moeten worden geïsoleerd, ACL's, balancers en IP-filters laden. Extern netwerkverkeer dat uw virtual machine(s) is beperkt tot poorten en protocollen die u definieert. Netwerk filteren om te voorkomen dat vervalste verkeer is geïmplementeerd en binnenkomende en uitgaande verkeer wordt beperkt tot vertrouwde platformonderdelen. Verkeer stroom beleid wordt geïmplementeerd op grens beveiliging apparaten die verkeer weigeren al dan niet standaard.

![Microsoft Antimalware in Azure wordt aangegeven](./media/azure-security-getting-started/sec-azgsfig3.PNG)

NAT (Network Address Translation) wordt gebruikt voor het scheiden van interne netwerkverkeer van externe-verkeer is toegestaan. Interne verkeer is niet extern omgeleid. [Virtuele IP-adressen](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx) die zijn extern geschikt worden omgezet in [Interne dynamische IP-](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx) adressen die zijn alleen geschikt binnen Azure.

Externe verkeer naar Azure virtuele machines is met firewall via toegangsbeheerlijsten (ACL's) op routers, netwerktaakverdelers, en schakelt u Layer 3. Alleen bepaalde bekende protocollen zijn toegestaan. ACL's zijn op hun plaats staan die afkomstig zijn van Gast VMs naar andere VLAN's gebruikt voor het beheer van verkeer beperken. Daarnaast verkeer via IP-filters voor het hostbesturingssysteem gefilterd, het verkeer op beide gegevens voor de koppeling en netwerk lagen verder te beperken.

### <a name="how-azure-implements-isolation"></a>Hoe Azure moeten worden geïsoleerd implementeert

De Controller uit Azure stof is verantwoordelijk voor het toewijzen van resources infrastructuur tenant werkbelasting en worden beheerd in één richting communicatie van de host naar VMs. De Azure hypervisor wordt afgedwongen geheugen en proces scheiding tussen VMs en veilig netwerkverkeer routeert met gast OS tenants. Azure implementeert ook moeten worden geïsoleerd voor tenants, opslag en virtuele-netwerken te gebruiken:

-   Elke Azure AD-tenant is logisch geïsoleerd beveiligingsgrenzen gebruiken.

-   Azure opslag Accounts uniek zijn voor elk abonnement en access moet worden geverifieerd door een opslag-Account te gebruiken.

-   Virtuele netwerken zijn logisch geïsoleerd via een combinatie van unieke privé IP-adressen, firewalls en IP-ACL's. Laden balancers route verkeer naar de juiste tenants op basis van de definities van het eindpunt.

##<a name="virtual-network-and-firewall"></a>Virtuele netwerk- en firewall

De [verdeelde en virtuele netwerken](http://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx) in Azure ervoor te zorgen dat uw persoonlijke netwerkverkeer logisch geïsoleerd van verkeer op andere Azure virtuele netwerken.

![Microsoft Antimalware in Azure wordt aangegeven](./media/azure-security-getting-started/sec-azgsfig4.PNG)

Uw abonnement kan bevatten meerdere geïsoleerd particuliere netwerken (en firewall, taakverdeling en NAT opnemen).

Azure biedt drie primaire niveaus van netwerk scheiding in elke Azure cluster logisch scheiden verkeer. [Virtuele LAN](https://azure.microsoft.com/services/virtual-network/) (VLAN's) worden gebruikt voor het scheiden van klantverkeer van de rest van de Azure netwerk. Toegang tot het Azure network van buiten het cluster is beperkt tot en met netwerktaakverdelers.

Netwerkverkeer naar en vanuit VMs verloopt via de hypervisor virtuele schakeloptie. Het IP-filter-onderdeel in de hoofdmap OS geïsoleerd de hoofdsite VM uit de VMs Gast en de Gast VMs van elkaar. Filteren van verkeer naar de communicatie tussen tenant knooppunten en openbare Internet (gebaseerd op een van de klant service te configureren), ze scheiden van andere tenants beperken wordt uitgevoerd.

Het IP-filter helpt voorkomen dat Gast VMs uit:

- Vervalste verkeer genereren

- Ontvangen verkeer niet gericht aan deze

- Verwijzen naar de eindpunten van de beveiligde infrastructuur

- Verzenden of ontvangen van ongeschikte broadcast-verkeer

U kunt uw virtuele machines naar [Azure virtuele netwerken](https://azure.microsoft.com/documentation/services/virtual-network/)plaatsen. Deze virtuele netwerken zijn vergelijkbaar met de netwerken waarmee u in on-premises omgevingen configureert, waar ze zich doorgaans gekoppeld aan een virtuele schakeloptie bevinden. Virtuele machines die zijn gekoppeld aan hetzelfde Azure virtuele netwerk kunt communiceren met elkaar zonder aanvullende configuratie. U hebt ook de optie voor het configureren van verschillende subnetten binnen uw Azure Virtual Network.

U kunt de volgende Azure Virtual Network technologieën gebruiken om te helpen beveiligde communicatie op uw Azure virtuele netwerk:

-   [**Beveiligingsgroepen netwerk (NSG)**](../virtual-network/virtual-networks-nsg.md). U kunt een NSG naar besturingselement verkeer naar een of meer exemplaren van virtuele machine (VM) gebruiken in uw virtuele netwerk. Een NSG access besturingselement regels bevat die toestaan of weigeren van verkeer op basis van verkeer richting, protocol, bronadres en poort en doeladres en poort.

-   [**Door gebruiker gedefinieerde-mailroutering**](../virtual-network/virtual-networks-udr-overview.md). U kunt bepalen de routering van pakketten door een virtueel toestel door te maken door de gebruiker gedefinieerde routes die opgeven van de volgende hop voor pakketten die doorloopt naar een specifieke subnet, gaat u een uw virtueel netwerk beveiliging toestel.

-   [**IP-doorsturen**](../virtual-network/virtual-networks-udr-overview.md). Een virtueel netwerk beveiliging toestel moet kunnen binnenkomende verkeer dat niet is geadresseerd aan zelf ontvangen. Als u wilt toestaan dat een VM verkeer die zijn gericht aan andere bestemmingen ontvangen, u IP-doorsturen inschakelen voor VM.

-   [**U wordt gedwongen tunneling**](../vpn-gateway/vpn-gateway-about-forced-tunneling.md). Afgedwongen tunneling kunt u omleiden of "afdwingen" al het verkeer via Internet gebonden gegenereerd door uw virtuele machines in een Azure virtuele terug naar uw on-premises locatie via een VPN-site-naar-site tunnel voor inspectie en controle

-   [ **Eindpunt** ACL's](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md). U kunt bepalen welke computers zijn toegestaan binnenkomende verbindingen van Internet naar een virtuele machine op uw Azure virtuele netwerk door het eindpunt ACL's definiëren.

-   [**Partneroplossingen netwerk beveiliging**](https://azure.microsoft.com/marketplace/). Er zijn een aantal partner network beveiligingsoplossing waartoe u toegang hebt van Azure Marketplace.

### <a name="how-azure-implements-virtual-networks-and-firewall"></a>Hoe Azure virtuele netwerken en firewall implementeert

Azure implementeert firewalls pakket filteren op alle host en als gast VMs al dan niet standaard. Windows-besturingssysteem afbeeldingen uit de galerie met Azure hebt ook Windows Firewall is standaard ingeschakeld. Netwerktaakverdelers bij de omtrek van de Azure openbare omlaag netwerken besturingselement communicatie op basis van IP-ACL's beheerd door beheerders van de klant.

Als de gegevens van een klant Azure als onderdeel van de normale bewerkingen of tijdens een noodgevallen verplaatst, gebeurt dit via versleutelde communicatiekanalen. Er zijn andere mogelijkheden overgenomen door Azure in virtuele netwerken en firewall te gebruiken:

-   **Systeemeigen Host Firewall**: Azure stof en opslag worden uitgevoerd op een systeemeigen OS waarvoor geen hypervisor en dus windows firewall is geconfigureerd met de bovenstaande twee sets met regels. Opslag wordt uitgevoerd systeemeigen optimaliseren.

-   **Host Firewall**: de hostfirewall is het hostbesturingssysteem waarmee de hypervisor beveiligen. De regels zijn geprogrammeerde toestaan dat alleen de controller stof en vakken contact opnemen met het hostbesturingssysteem op een specifieke poort springen. De andere uitzonderingen zijn wilt toestaan dat DHCP-antwoord en DNS-antwoorden. Azure gebruik een machine configuratiebestand die de sjabloon van van regels voor het hostbesturingssysteem firewall. De host zelf is beveiligd tegen externe aanvallen door een Windows-firewall die is geconfigureerd dat alleen toegestaan communicatie van bekende, geverifieerde bronnen.

-   **Gast Firewall**: wordt overgenomen door de regels in het filter voor het pakket van VM veranderen maar geprogrammeerde in verschillende software (dat wil zeggen de stuk Windows Firewall van de Gast OS). De firewall van Gast VM kan worden geconfigureerd communicatie beperken of naar de Gast VM, zelfs als de communicatie is toegestaan op configuraties bij de host IP-Filter. U kunt bijvoorbeeld de Gast VM firewall gebruiken om te beperken communicatie tussen twee van uw VNets die is geconfigureerd voor het met elkaar verbinden.

-   **Opslag Firewall (FW)**: de firewall op de front-opslag verkeer moet zijn alleen op 80/443 en andere poorten nodig hulpprogramma worden gefilterd. De firewall op de opslag van de back-enddatabase beperkt communicatie als u wilt alleen afkomstig zijn van opslag front-servers.

-   **Virtuele netwerkgateway**: [Azure virtuele netwerkgateways](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) fungeren als de cross bedrijfsruimten gateways uw werkbelasting in Azure Virtual Network verbinden met uw aan premises-sites. Dit is vereist voor de verbinding te maken met lokale sites via [IPSec-site-naar-site VPN tunnels](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)of via [ExpressRoute](../expressroute/expressroute-introduction.md) circuits. Voor IPsec/IKE VPN tunnels gateways IKE handshakes uitvoeren en stand brengen van de tunnels IPsec S2S VPN tussen de virtuele netwerken en klik op de lokale sites. Virtuele netwerkgateways beëindigen ook [punt-naar-site VPN's](../vpn-gateway/vpn-gateway-point-to-site-create.md).

##<a name="secure-remote-access"></a>Externe toegang beveiligen

Gegevens die zijn opgeslagen in de cloud moeten voldoende waarborgen ingeschakeld om te voorkomen zwakke en vertrouwelijkheid en integriteit terwijl in overdracht hebben. Dit geldt ook voor netwerk-besturingselementen die worden gecombineerd met van een organisatie-beleid, kunt controleren identiteits- en toegangsbeheer regelingen voor het beheer.

Ingebouwde cryptographic technologie kunt u versleutelen communicatie in en tussen tussen Azure regio's en van Azure implementaties met on-premises implementatie-datacenters. Beheerderstoegang tot virtuele machines tot en met [Extern bureaublad-sessies](../virtual-machines/virtual-machines-windows-classic-connect-logon.md), [Externe Windows PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/09/07/weekend-scripter-remoting-the-cloud-with-windows-azure-and-powershell.aspx)en de beheerportal Azure is altijd versleuteld.

Als u wilt uitbreiden veilig uw on-premises implementatie-datacenter in de cloud, Azure biedt zowel [naar website VPN](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) en [punt-naar-site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md), evenals specifieke verbindingen met [ExpressRoute](../expressroute/expressroute-introduction.md) (verbindingen met Azure virtuele netwerken via VPN worden gecodeerd).

### <a name="how-azure-implements-secure-remote-access"></a>Hoe Azure implementeert veilige externe toegang

Verbindingen met Azure-Portal moeten altijd worden geverifieerd, en die nodig zijn SSL/TLS. U kunt management certificaten om in te schakelen secure management configureren. Industriële standaard secure protocollen zoals [SSTP](https://technet.microsoft.com/magazine/2007.06.cableguy.aspx) en [IPsec](https://en.wikipedia.org/wiki/IPsec) volledig worden ondersteund.

[Azure ExpressRoute](../expressroute/expressroute-introduction.md) kunt u persoonlijke verbindingen tussen Azure datacenters en dat op uw locatie of in een omgeving met anderen samenwerkt locatie-infrastructuur maken. ExpressRoute verbindingen gaan niet via de openbare Internet. Ze bieden meer betrouwbaarheid, snellere snelheden, onderste vertragingstijden en hoger beveiliging dan typische op Internet gebaseerde koppelingen. In sommige gevallen ExpressRoute verbindingen gebruiken om gegevens te brengen tussen on-premises en Azure kan ook worden verkregen aanzienlijk kostenvoordelen.

##<a name="logging-and-monitoring"></a>Logboekregistratie en controle

Azure biedt geverifieerde logboekregistratie van beveiliging gebeurtenissen die een audittrail genereren en is ontworpen om u te zijn tegen wordt geknoeid. Dit geldt ook voor systeeminformatie, zoals de gebeurtenislogboeken beveiliging in Azure infrastructuur VMs en Azure AD. Beveiliging gebeurtenis monitoring bevat voor het verzamelen van gebeurtenissen zoals wijzigingen in de IP-adressen van DHCP of DNS-server, poging tot toegang tot poorten en protocollen of IP-adressen die worden geblokkeerd door ontwerp, wijzigingen in beveiligingsinstellingen beleid of firewall, account of een groep maken, onverwachte processen of stuurprogramma installatie.

![Microsoft Antimalware in Azure wordt aangegeven](./media/azure-security-getting-started/sec-azgsfig5.PNG)

Controlelogboeken opname bevoegdheden gebruikerstoegang en activiteiten, geautoriseerde en onbevoegde toegangspogingen systeem uitzonderingen en informatie beveiligingsgebeurtenissen worden bewaard gedurende een bepaalde periode. Het behoud van uw logboeken is naar eigen inzicht omdat u log verzamelen en bewaren om uw eigen vereisten configureren.

### <a name="how-azure-implements-logging-and-monitoring"></a>Hoe Azure implementeert logboekregistratie en controle

Ongeacht of ze systeemeigen of virtuele zijn, Azure Management agenten (MA) en Azure beveiliging Monitor (ASM) agenten implementeren in elke berekeningscluster, opslag of stof knooppunt onder beheer. Elke Management-Agent is geconfigureerd om te worden geverifieerd bij een serviceaccount voor de opslag van team met een certificaat hebt aangeschaft bij de winkel Azure certificaat- en doorsturen vooraf geconfigureerd diagnostische en gebeurtenisgegevens in de opslagruimte-account. Deze agenten zijn niet geïmplementeerd in klanten virtuele machines.

Beheerders van Azure toegang tot logboeken via een webportal voor geverifieerde en gecontroleerde toegang tot de logboeken. Een beheerder kan parseren, filteren, relateren en analyseren Logboeken. De Azure-team opslag serviceaccounts logboekbestanden zijn beveiligd tegen directe beheerderstoegang om te voorkomen dat ten opzichte van log wordt geknoeid.

Microsoft verzamelt Logboeken vanaf netwerkapparaten die gebruikmaken van het protocol Syslog en vanaf host-servers met Microsoft controle siteverzameling Services (ACS). Deze logboeken worden in een logboekdatabase waaruit waarschuwingen worden gegenereerd voor verdachte gebeurtenissen rechtstreeks naar de beheerder van een Microsoft geplaatst. De beheerder kan bekijken en deze logboeken analyseren.

[Diagnostisch hulpprogramma Azure](https://msdn.microsoft.com/library/azure/gg433048.aspx) is een functie van Azure waarmee u kunt de diagnostische gegevens verzamelen van een toepassing wordt uitgevoerd in Azure wordt aangegeven. Dit is diagnostische gegevens voor foutopsporing en probleemoplossing, meten van de prestaties, Resourcegebruik, wordt verkeer analyse en capaciteit, planning bewaken en controle. Nadat de diagnostische gegevens worden verzameld, kan deze worden doorgeschakeld naar een account Azure opslag voor permanente. Overdrachten ofwel kunnen worden gepland of op aanvraag.

##<a name="threat-mitigation"></a>Bedreiging risicobeperking

Azure dienst naast moeten worden geïsoleerd, versleuteling en filteren, een aantal bedreiging risicobeperking regelingen en processen te beveiligen infrastructuur en services. Hierbij interne besturingselementen en -technologieën die worden gebruikt om te detecteren en een geavanceerd threats zoals DDoS, escalatie van bevoegdheden en de [OWASP Top-10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)te verhelpen.

Beperk de kans op pesterijen-incidenten de besturingselementen voor beveiliging en risicobeheer die Microsoft op hun plaats staan heeft voor het beveiligen van de cloudinfrastructuur. Maar in het geval van een incident voordoet, het team beveiliging Incident Management (SIM) in het team van Microsoft onlineservices beveiliging en naleving (OSSC) is klaar 24 x 7 om te reageren.

### <a name="how-azure-implements-threat-mitigation"></a>Hoe Azure bedreiging risicobeperking implementeert

Azure heeft beveiliging besturingselementen op hun plaats staan willen implementeren bedreiging risicobeperking en waarmee klanten Verklein mogelijke threats in hun omgevingen. De onderstaande lijst bevat een overzicht van de bedreiging risicobeperking mogelijkheden aangeboden door Azure:

-   [Azure Anti-Malware](../security/azure-security-antimalware.md) is al dan niet standaard op alle infrastructuurservers ingeschakeld. U kunt desgewenst deze binnen uw eigen VMs inschakelen.

-   Microsoft onderhoudt voortdurende controle over servers, netwerken en threats detecteren in en voorkomen zwakke-toepassingen. Beheerders van de afwijkende gedrag, zodat ze corrigerende maatregelen nemen op zowel de interne als de externe threats een melding weergegeven geautomatiseerde waarschuwingen.

-   Hebt u de optie 3e derden beveiligingsoplossingen binnen uw abonnement, zoals web application firewalls van [Barracuda](https://techlib.barracuda.com/ng54/deployonazure)implementeren.

-   Aanpak van Microsoft voor het testen van er bevat "[Rood teams](http://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf)", waarbij medewerkers van Microsoft-beveiliging (niet-klant) live systemen in Azure wordt aangegeven om te testen van beveiliging tegen echte, geavanceerde permanente threats aanvallen.

-   Geïntegreerde implementatie systemen beheren de distributie en de installatie van beveiligingspatches binnen het Azure platform.

##<a name="next-steps"></a>Volgende stappen

[Azure-Vertrouwenscentrum](https://azure.microsoft.com/support/trust-center/)

[Teamblog van Azure beveiliging](http://blogs.msdn.com/b/azuresecurity/)

[Antwoord van Microsoft Beveiligingscentrum](https://technet.microsoft.com/library/dn440717.aspx)

[Active Directory-Blog](http://blogs.technet.com/b/ad/)
