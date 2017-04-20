<properties
   pageTitle="Een secure hybride netwerkarchitectuur implementeren in Azure | Microsoft Azure"
   description="Het implementeren van een beveiligde hybride netwerkarchitectuur in Azure wordt aangegeven."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-dmz-between-azure-and-your-on-premises-datacenter"></a>Een DMZ tussen Azure en uw on-premises implementatie-datacenter implementeren

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

In dit artikel wordt beschreven aanbevolen procedures voor de uitvoering van een beveiligde hybride netwerk waarmee ook een on-premises netwerk Azure. Deze verwijzing architectuur implementeert een DMZ tussen een on-premises netwerk en een Azure virtuele netwerk door gebruik van de gebruiker gedefinieerde routes (UDRs). De DMZ bevat ten zeerste beschikbare netwerkhulpprogramma virtuele toestellen (NVAs) die beveiligingsfunctionaliteit zoals firewalls en pakket controle implementeren. Alle uitgaand verkeer uit het VNet is dwingen tunnel met Internet via de on-premises netwerk zodat deze kan worden gecontroleerd. 

Een verbinding met uw on-premises implementatie-datacenter dat is geïmplementeerd via een [VPN-gateway]is vereist voor deze architectuur[ra-vpn], of een [ExpressRoute] [ ra-expressroute] verbinding.

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen: [Resourcemanager] [ resource-manager-overview] en klassieke. Deze verwijzing architectuur gebruikt resourcemanager, waarin u wordt aangeraden voor nieuwe implementaties.

Veelvoorkomend gebruik dozen voor deze architectuur opnemen:

- Hybride toepassingen waar werkbelasting uitvoeren gedeeltelijk on-premises implementatie en deels in Azure.

- Azure-infrastructuur waarmee binnenkomende verkeer van on-premises en internet.

- Toepassingen die zijn vereist voor uitgaand verkeer controle. Dit is vaak een wettelijke vereiste van veel commerciële systemen en kan helpen om te voorkomen dat openbaarmaking van privé-gegevens.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram wordt de belangrijke onderdelen in deze architectuur gemarkeerd:

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft Downloadcentrum][visio-download]. Dit diagram is op de pagina "DMZ--privé".

[! [0]][0]

- **On-premises netwerk.** Dit is een netwerk met computers en apparaten die zijn verbonden via een privé LAN geïmplementeerd in een organisatie.

- **Azure virtuele netwerk (VNet).** De VNet host de toepassing en andere resources die zijn uitgevoerd in de cloud.

- **Gateway.** De gateway biedt connectiviteit tussen de routers in de on-premises netwerk en het VNet.

- **Virtuele toestel netwerk (NVA).** NVA is een algemene term waarmee een VM uitvoeren van taken zoals toestaan of weigeren van access als een firewall, optimaliseren WAN-bewerkingen (met inbegrip van Netwerkcompressie), aangepaste routering of andere netwerkfunctionaliteit wordt beschreven. 

- **Weblaag, zakelijke laag en gegevens laag subnetten.** Hierna ziet u subnetten hostingprovider de VMs en services die een voorbeeld 3 lagen-toepassing die wordt uitgevoerd in de cloud implementeren. Zie [Windows VMs uitgevoerd voor een architectuur N lagen op Azure] [ ra-n-tier] voor meer informatie.

- **Door de gebruiker gedefinieerde routes (UDR).** [Door gebruiker gedefinieerde routes] [ udr-overview] de stroom van IP-verkeer binnen Azure VNets definiëren. 

> [AZURE.NOTE] Afhankelijk van de vereisten van uw VPN-verbinding, kunt u de rand Gateway Protocol (BGP) routes configureren als alternatief voor over het gebruik van UDRs willen implementeren van de doorstuurregels die verkeer door de achterwaarts door de on-premises netwerk.

- **Management subnet.** Dit subnet bevat VMs die management en controlefuncties voor de onderdelen die worden uitgevoerd in de VNet implementeren. 

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en resourcetypen, zodat deze architectuur verwijzing kan worden deze is ingericht veel verschillende manieren. We hebben een resourcemanager Azure-sjabloon om te kunnen installeren van de architectuur van de verwijzing die deze aanbevelingen volgt opgegeven. Als u wilt maken van de architectuur van uw eigen verwijzing moet u deze aanbevelingen volgt, tenzij u specifieke vereisten die een aanbeveling niet past hebt.

### <a name="rbac-recommendations"></a>RBAC aanbevelingen

Verschillende RBAC rollen voor het beheren van de informatiebronnen in uw toepassing maken. Houd rekening met het maken van een DevOps [aangepaste rol] [ rbac-custom-roles] met machtigingen om de infrastructuur voor de toepassing te beheren. Houd rekening met het maken van een gecentraliseerde IT-beheerder [aangepaste rol] [ rbac-custom-roles] voor het beheren van netwerkbronnen, en een afzonderlijke beveiliging voor IT-beheerder [aangepaste rol] [ rbac-custom-roles] veilig netwerkbronnen, zoals de NVAs beheren. 

De rol van de DevOps moet machtigingen voor het implementeren van de toepassingsonderdelen, evenals de monitor en opnieuw starten VMs bevatten. De centrale rol voor IT-beheerder moet de machtigingen om te controleren netwerkbronnen opnemen. Geen van deze rollen moeten toegang hebben tot de NVA bronnen zoals deze beperkt tot de beveiliging rol voor IT-beheerder worden moet.

### <a name="resource-group-recommendations"></a>Resource groep aanbevelingen

Azure resources zoals VMs, VNets en netwerktaakverdelers kunnen eenvoudig worden beheerd door ze te groeperen in resourcegroepen. Vervolgens kunt u de rollen hierboven toewijzen aan elke resourcegroep beperken van toegang.

U wordt aangeraden het maken van de volgende opties:

- Een resourcegroep met de subnetten (exclusief de VMs), NSGs en de gateway-bronnen voor de verbinding met de on-premises netwerk. De centrale rol voor IT-beheerder toewijzen aan deze resourcegroep.

- Een resourcegroep die de VMs bevat voor de NVAs (inclusief de taakverdeling), het vak sprong en andere VMs management en de UDR voor de gateway-subnet waardoor al het verkeer via de NVAs. De beveiliging IT-beheerdersrol toewijzen aan deze resourcegroep.

- Afzonderlijk resourcegroepen voor elke toepassingslaag die de taakverdeling en VMs bevatten. Houd er rekening mee dat deze resourcegroep de subnetten voor elke laag niet mag bevatten. De rol DevOps toewijzen aan deze resourcegroep.

### <a name="virtual-network-gateway-recommendations"></a>Aanbevelingen voor de gateway virtuele netwerken

On-premises implementatie verkeer wordt doorgegeven aan de VNet door een virtueel netwerkgateway. Het is raadzaam een [VPN-Azure gateway] te[ guidance-vpn-gateway] of een [Azure ExpressRoute gateway][guidance-expressroute]. 

### <a name="nva-recommendations"></a>NVA aanbevelingen

NVAs bieden verschillende services voor het beheren en netwerkverkeer controleren. De Azure Marketplace biedt diverse derde leverancier NVAs, waaronder:

- [Barracuda Web Application-Firewall] [ barracuda-waf] en [Barracuda NextGen Firewall][barracuda-nf]

- [Samenhangende netwerken VNS3 Firewall/Router/VPN][vns3]

- [Fortinet FortiGate-VM][fortinet]

- [SecureSphere Web Application Firewall][securesphere]

- [DenyAll Web Application Firewall][denyall]

- [Punt vSEC controleren][checkpoint]

Als geen van deze NVAs derden aan uw vereisten voldoet, kunt u een aangepaste NVA VMs gebruik kunt maken. Voor een voorbeeld van het maken van aangepaste NVAs, raadpleegt u de DMZ in deze verwijzing architectuur waarmee de volgende functionaliteit:

- Verkeer is gerouteerd via [IP doorschakelen] [ ip-forwarding] op de NVA NIC's.

- Verkeer is toegestaan te bladeren door de NVA alleen als deze is zo nodig. Elke NVA VM in de verwijzing architectuur is een eenvoudige Linux router met binnenkomende verkeer binnengekomen op netwerk interface *eth0*en uitgaand verkeer overeenkomende regels gedefinieerd door aangepaste scripts verzonden via netwerk interface *eth1*. 

- Verkeer doorgestuurd naar het subnet management niet langs de NVAs en de NVAs kunnen alleen worden geconfigureerd vanaf het subnet management. Als het verkeer naar het subnet management moet worden gerouteerd via de NVAs, is er geen route naar het subnet management om op te lossen de NVAs als ze moeten niet.  

- Het VMs voor de NVA worden opgenomen in een beschikbaarheid achter een taakverdeling instellen. De UDR in het subnet gateway wordt u omgeleid zodat NVA aanvragen voor de taakverdeling.

Een andere aanbeveling aandachtspunten maakt meerdere NVAs in reeks verbinding met elke NVA uitvoeren van een beveiligingstaak gespecialiseerde. Hiermee kunt elke beveiligingsfunctie op basis van de per NVA worden beheerd. Een NVA implementeren van een firewall kan bijvoorbeeld in de reeks worden geplaatst, met een NVA identiteit services uitgevoerd. De verhouding voor eenvoudige beheer van is het toevoegen van extra netwerk hops die mogelijk latentie vergroten, dus zorgen dat dit niet van invloed op prestaties van de toepassing.

### <a name="nsg-recommendations"></a>NSG aanbevelingen

De gateway VPN beschrijft een openbare IP-adres van de verbinding met de on-premises netwerk. Wij aanraden voor het maken van een beveiligingsgroep netwerk (NSG) voor de binnenkomende NVA subnet regels voor het blokkeren van al het verkeer niet die afkomstig zijn van de on-premises netwerk.

Ook aangeraden om NSGs voor elk subnet om een bepaald tweede niveau van bescherming tegen binnenkomende verkeer een onjuist geconfigureerde of uitgeschakelde NVA overslaan te implementeren. Het web laag subnet in de verwijzing architectuur implementeert, bijvoorbeeld een NSG met een regel als u wilt negeren, alle aanvragen afgezien van het ontvangen van de on-premises netwerk (192.168.0.0/16) of het VNet en een andere regel die alle aanvragen niet op poort 80, worden genegeerd. 

### <a name="internet-access-recommendations"></a>Aanbevelingen voor Internet-toegang

[Dwingen-tunnel] [ azure-forced-tunneling] alle uitgaande internetverkeer via uw on-premises netwerk met de site-naar-site VPN tunnel en de route met het internet via netwerk adres tranlation (Translation). Zo te voorkomen dat per ongeluk lekkage van vertrouwelijke gegevens opgeslagen in de gegevenslaag van uw en ook toestaan inspectie en controle van alle uitgaand verkeer.

> [AZURE.NOTE] Niet volledig internetverkeer vanaf het web, business- en -lagen blokkeren. Als deze lagen Azure PaaS-services die ze zijn afhankelijk van openbare IP-adressen voor VM diagnostische gegevens vastleggen gebruiken, downloadt u VM extensies en andere functionaliteit. Azure diagnostische gegevens worden ook dat onderdelen kunnen lezen en met een internet-afhankelijke Azure opslag-account schrijven vereist.

Verder raden controleren uitgaande internetverkeer is dwingen tunnel correct. Als u een VPN-verbinding met de [Routering en remote access service gebruikt] [ routing-and-remote-access-service] gebruiken op een server on-premises implementatie, een hulpmiddel zoals [WireShark] [ wireshark] of [Microsoft bericht Analyzer](https://www.microsoft.com/en-us/download/details.aspx?id=44226).

### <a name="management-subnet-recommendations"></a>Management subnet aanbevelingen

Het subnet management bevat het sprong die wordt uitgevoerd management en controle functionaliteit. De volgende aanbevelingen voor het vak sprong implementeren:
- Maak een openbare IP-adres van het vak sprong geen. 
- Één route toegang tot het vak sprong via de binnenkomende gateway en het implementeren van een NSG in de management subnet, alleen reageren op aanvragen van de toegestane route maken.
- Uitvoering van alle beveiligde beheertaken naar het vak sprong beperken.

### <a name="nva-recommendations"></a>NVA aanbevelingen

Een laag 7 opnemen NVA toepassing verbindingen op het niveau van de NVA beëindigen en onderhouden van affiniteit bij de backend-lagen. Dit zorgt ervoor symmetric connectivity waarin antwoord verkeer van de backend-lagen tot en met de NVA als resultaat.

## <a name="scalability-considerations"></a>Aandachtspunten voor de schaalbaarheid

De architectuur verwijzing implementeert een taakverdeling waarmee on-premises implementatie netwerkverkeer aan een groep NVA apparaten. Zoals eerder is beschreven, de NVA-apparaten zijn VMs netwerkverkeer routeren regels uitvoeren en zijn geïmplementeerd in een [beschikbaarheid instellen][availability-set]. Deze ontwerpen kunt u de doorvoer van de NVAs controleren na verloop van tijd en NVA apparaten toevoegen in antwoord op stijging laden.

De standaard SKU VPN gateway ondersteunt continue doorvoer van maximaal 100 Mbps. De hoge prestaties SKU biedt maximaal 200 Mbps. Voer een upgrade uit naar een gateway ExpressRoute voor hogere bandbreedte. ExpressRoute biedt maximaal 10 GB/s bandbreedte met lagere latentie dan een VPN-verbinding.

> [AZURE.NOTE] De [uitvoering van een hybride netwerkarchitectuur met Azure en On-premises VPN] artikelen[ guidance-vpn-gateway] en [implementeren van een hybride netwerkarchitectuur met Azure ExpressRoute] [ guidance-expressroute] problemen die zich rond de schaalbaarheid van Azure gateways beschrijven.

## <a name="availability-considerations"></a>Aandachtspunten voor de beschikbaarheid

De architectuur verwijzing implementeert een taakverdeling distribueren aanmeldingsaanvragen van on-premises implementatie aan een groep NVA apparaten in Azure wordt aangegeven. De NVA-apparaten zijn VMs uitvoeren van regels voor netwerk-verkeer is toegestaan routing en zijn geïmplementeerd in een [beschikbaarheid instellen][availability-set]. De taakverdeling regelmatig zoekopdracht in een systeemstatus-test geïmplementeerd op elke NVA en eventuele niet reageert NVAs worden verwijderd uit de groep. 

Als u Azure ExpressRoute gebruikt voor connectiviteit tussen het VNet en on-premises netwerk, [een VPN-gateway voor failover configureren] [ guidance-vpn-failover] als de verbinding ExpressRoute niet beschikbaar is.

Zie de artikelen die [een hybride netwerkarchitectuur met Azure en On-premises VPN implementeren] voor specifieke informatie over de beschikbaarheid voor VPN en ExpressRoute verbindingen onderhouden,[ guidance-vpn-gateway] en [implementeren van een hybride netwerkarchitectuur met Azure ExpressRoute][guidance-expressroute].

## <a name="manageability-considerations"></a>Aandachtspunten voor de beheerbaarheid

Alle-toepassing en het monitoren van de resource moeten worden uitgevoerd door het vak sprong in het subnet management. Afhankelijk van uw toepassingsvereisten voor de, moet u mogelijk extra bronnen voor controle toevoegen in het subnet management, maar opnieuw een van deze extra bronnen moet worden geraadpleegd via het vak sprong.

Als gateway-connectiviteit uit uw on-premises netwerk met Azure niet actief is, kunt u het vak sprong nog steeds bereiken door te implementeren van een PIP, toevoegen aan het vak van de lijnsprong en externe in van internet.

Van elke laag subnet in de verwijzing architectuur door NSG regels is beveiligd en kan het nodig is om te maken van een regel om te openen 3389 voor RDP-toegang op Windows VMs of poort 22 voor SSH toegang op Linux VMs zijn. Andere management en controlehulpmiddelen mogelijk regels om extra poorten te openen.

Als u voor de connectiviteit tussen uw on-premises implementatie datacenter en Azure ExpressRoute gebruikt, gebruikt u de [Azure Connectivity Toolkit (AzureCT)] [ azurect] om te controleren en problemen met verbinding oplossen.

> [AZURE.NOTE] U vindt aanvullende informatie die specifiek is gericht op controle en beheer van de VPN en ExpressRoute verbindingen in de [uitvoering van een hybride netwerkarchitectuur met Azure en On-premises VPN] artikelen[ guidance-vpn-gateway] en [implementeren van een hybride netwerkarchitectuur met Azure ExpressRoute][guidance-expressroute].

## <a name="security-considerations"></a>Veiligheidsoverwegingen

Deze verwijzing architectuur implementeert meerdere niveaus van beveiliging: 

### <a name="routing-all-on-premises-user-requests-through-the-nva"></a>Omleiding van alle on-premises implementatie gebruikersaanvragen tot en met de NVA

De UDR in het subnet gateway blokkeert alle gebruiker verzoeken afgezien van het ontvangen van on-premises implementatie. De fasen UDR toegestane aanvragen voor het NVAs in het privé DMZ-subnet en deze aanvragen aan de toepassing op worden doorgegeven als ze zijn toegestaan door de regels NVA. Andere routes kunnen worden toegevoegd aan de UDR, maar zorgen dat ze niet per ongeluk de NVAs of de administratieve verkeer blok bedoeld voor het beheer van subnet overslaan.

De verdeling van de belasting vóór de NVAs fungeert ook als een beveiligingsapparaat door het verkeer op poorten die zijn niet geopend in de regels van taakverdeling negeren. De netwerktaakverdelers in de verwijzing architectuur luistert alleen voor HTTP-aanvragen op poort 80 en HTTPS-verzoeken op poort 443. Eventuele aanvullende regels die zijn toegevoegd aan de netwerktaakverdelers moeten worden aangegeven en het verkeer om ervoor te zorgen er geen beveiligingskwesties moet worden gecontroleerd.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Gebruik van NSGs om te blokkeren/keer verkeer tussen lagen

Elk van de lagen web en business gegevens beperken het verkeer tussen deze NSGs gebruiken. Dat wil zeggen de laag bedrijven gebruikt een NSG blokkeren van al het verkeer die niet afkomstig uit de weblaag zijn en de gegevenslaag gebruikt een NSG blokkeren van al het verkeer die niet afkomstig uit de laag bedrijven zijn. Als er een vereiste dat de regels NSG bredere toegang tot deze niveaus uitvouwen, wegen deze vereisten is voldaan ten opzichte van de beveiligingsrisico's. Elke nieuwe binnenkomende pad staat voor een verkoopkans per ongeluk of purposeful gegevens zijn lekken of toepassing beschadigd.

### <a name="devops-access"></a>DevOps access

De bewerkingen die DevOps kunt uitvoeren op elke laag met [RBAC] beperken[ rbac] om machtigingen te beheren. Wanneer het toewijzen van machtigingen, gebruikt u het [principe van het laagste bevoegdheidsniveau][security-principle-of-least-privilege]. Meld u aan alle beheerdersbewerkingen en uitvoeren van gewone controles om ervoor te zorgen dat er configuratiewijzigingen zijn gepland.

> [AZURE.NOTE] Voor meer uitgebreide informatie, voorbeelden en scenario's over het beheren van netwerkbeveiliging met Azure, raadpleegt [Microsoft-cloudservices en netwerkbeveiliging][cloud-services-network-security]. Zie [aan de slag met Microsoft Azure-beveiliging]voor gedetailleerde informatie over het beveiligen van resources in de cloud,[getting-started-with-azure-security]. Voor meer informatie over adressering van beveiliging via een verbinding Azure gateway Zie [een hybride netwerkarchitectuur met Azure en On-premises VPN implementeren] [ guidance-vpn-gateway] en [implementeren van een hybride netwerkarchitectuur met Azure ExpressRoute][guidance-expressroute].

## <a name="solution-deployment"></a>Implementatie van oplossingen

Een implementatie van een verwijzing architectuur dat deze aanbevelingen implementeert is beschikbaar op Github. De architectuur van deze verwijzing bevat een virtueel netwerk (VNet), netwerk-beveiligingsgroep (NSG), taakverdeling en twee virtuele machines (VMs).

De architectuur verwijzing kan worden geïmplementeerd met Windows- of Linux VMs aan de hand van de volgende instructies uit: 

1. Klik met de rechtermuisknop op de knop onderstaande en selecteer een van beide 'koppeling openen in nieuw tabblad"of 'Koppeling openen in nieuw venster':  
[![Implementeren naar Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet%2Fazuredeploy.json)

2. Nadat de koppeling is geopend in de portal van Azure, moet u waarden opgeven voor een deel van de instellingen: 
    - De naam van de **resourcegroep** is al gedefinieerd in het parameterbestand, dus **Nieuw** te selecteren en voer `ra-private-dmz-rg` in het tekstvak.
    - Selecteer de regio in de vervolgkeuzelijst **locatie** .
    - Bewerk de **Sjabloon hoofdsite Uri** of de **Parameter hoofdsite Uri** tekstvakken niet.
    - Controleer de bepalingen en voorwaarden en klik op het selectievakje **dat ik ga akkoord met de bovenstaande voorwaarden** .
    - Klik op de knop **aanschaffen** .

3. Wacht totdat de implementatie om te voltooien.

4. De parameterbestanden zijn hard gecodeerde beheerder-gebruikersnaam en wachtwoord voor alle VMs en het wordt ten zeerste aanbevolen dat u direct beide wijzigen. Voor elke VM in de implementatie, selecteert u deze in de portal van Azure en klik vervolgens op in het **wachtwoord opnieuw** in het blad **ondersteuning + probleemoplossing** . Selecteer **wachtwoord opnieuw** in het vak van de vervolgkeuzelijst **modus** en selecteer vervolgens een nieuwe **gebruikersnaam** en **wachtwoord**. Klik op de knop **bijwerken** om te voorkomen.

## <a name="next-steps"></a>Volgende stappen

- Lees hoe u een [DMZ tussen Azure en Internet](./guidance-iaas-ra-secure-vnet-dmz.md)implementeert.
- Lees hoe u een [hoge beschikbaarheid hybride netwerkarchitectuur](./guidance-hybrid-network-expressroute-vpn-failover.md)implementeert.

<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-create-availability-set.md
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[azure-forced-tunneling]: https://azure.microsoft.com/en-gb/documentation/articles/vpn-gateway-forced-tunneling-rm/
[barracuda-nf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/barracuda-ng-firewall/
[barracuda-waf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf/
[checkpoint]: https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/
[cloud-services-network-security]: https://azure.microsoft.com/documentation/articles/best-practices-network-security/
[denyall]: https://azure.microsoft.com/marketplace/partners/denyall/denyall-web-application-firewall/
[fortinet]: https://azure.microsoft.com/marketplace/partners/fortinet/fortinet-fortigate-singlevmfortigate-singlevm/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[ip-forwarding]: ../virtual-network/virtual-networks-udr-overview.md#IP-forwarding
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[ra-n-tier]: ./guidance-compute-n-tier-vm.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[rbac]: ../active-directory/role-based-access-control-configure.md
[rbac-custom-roles]: ../active-directory/role-based-access-control-custom-roles.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[routing-and-remote-access-service]: https://technet.microsoft.com/library/dd469790(v=ws.11).aspx
[securesphere]: https://azure.microsoft.com/marketplace/partners/imperva/securesphere-waf-for-azr/
[security-principle-of-least-privilege]: https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vns3]: https://azure.microsoft.com/marketplace/partners/cohesive/cohesiveft-vns3-for-azure/
[wireshark]: https://www.wireshark.org/
[0]: ./media/blueprints/hybrid-network-secure-vnet.png "Hybride netwerkarchitectuur Secure"
