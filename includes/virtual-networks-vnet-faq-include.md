## <a name="virtual-network-basics"></a>Basisbeginselen van Virtual Network

### <a name="what-is-an-azure-virtual-network-vnet"></a>Wat is een Azure virtuele netwerk (VNet)?

U kunt VNets inrichten en beheren van virtuele particuliere netwerken (VPN's) in Azure wordt aangegeven en, de VNets met andere VNets in Azure wordt aangegeven of met uw on-premises (optioneel) een koppeling IT-infrastructuur om hybride of meerdere lokale oplossingen te maken. Elke VNet die u maakt een eigen blok CIDR heeft en kan worden gekoppeld aan andere netwerken VNets en on-premises zo lang maken als de blokken CIDR niet conflict. U hebt ook besturingselementen van DNS-server-instellingen voor VNets en de VNet gesegmenteerd in subnetten.

Gebruik VNets naar:

- Een speciale privé alleen de cloud virtueel netwerk maken

    U kunt een cross-premises-configuratie soms niet vereisen voor uw oplossing. Wanneer u een VNet maakt, kunnen uw services en VMs binnen uw VNet rechtstreeks en veilig met elkaar communiceren in de cloud. Dit verkeer veilig binnen de VNet blijft, maar nog steeds kunt u eindpunt verbindingen configureren voor de VMs en de services waarvoor internetcommunicatie als onderdeel van uw oplossing.

- Uw datacenter veilig uitbreiden

    U kunt met VNets, traditionele naar website VPN's van (S2S) als u de capaciteit van de datacenter veilig wilt verkleinen maken. IPSEC S2S VPN's gebruiken voor een beveiligde verbinding tussen uw zakelijke VPN gateway en Azure.

- Hybride cloud-scenario's inschakelen

    VNets flexibiliteit de ter ondersteuning van een bereik van hybride cloud-scenario's. U kunt elk gewenst type on-premises systeem zoals mainframes en Unix-systemen-toepassingen cloudgebaseerde veilig koppelen.

### <a name="how-do-i-know-if-i-need-a-virtual-network"></a>Hoe weet ik of ik een virtueel netwerk nodig?

Ga naar het [Virtuele netwerk overzicht](../articles/virtual-network/virtual-networks-overview.md) als u wilt zien van een tabel beschikking waarmee u kunt bepalen van de beste optie voor het ontwerp van netwerk voor u.

### <a name="how-do-i-get-started"></a>Hoe ga ik aan de slag?

Ga naar [de Virtual Network-documentatie](https://azure.microsoft.com/documentation/services/virtual-network/) aan de slag. Deze pagina bevat koppelingen naar veelvoorkomende configuratiestappen, evenals informatie waarmee u kunt de zaken aan bod die moet u rekening houden bij het ontwerpen van uw virtuele netwerk begrijpen.

### <a name="what-services-can-i-use-with-vnets"></a>Voor welke services kan ik gebruiken met VNets?

VNets kan worden gebruikt met een verscheidenheid aan verschillende Azure services, zoals Cloudservices (PaaS), virtuele Machines en Web Apps. Er zijn echter enkele services die niet worden ondersteund op een VNet. Controleer de specifieke service die u wilt gebruiken en controleert u of deze compatibel.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Kan ik VNets zonder cross-premises-connectiviteit gebruiken?

Ja. U kunt een VNet gebruiken zonder naar website connectivity. Dit is vooral handig als u wilt uitvoeren domeincontrollers en SharePoint-farms in Azure wordt aangegeven.

## <a name="virtual-network-configuration"></a>Virtuele netwerkconfiguratie

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Welke hulpprogramma's kan ik gebruiken om te maken van een VNet?

U kunt de volgende hulpprogramma's gebruiken om te maken of een virtueel netwerk configureren:

- Azure-Portal (voor klassieke en resourcemanager VNets).

- Een netwerk configuratiebestand (netcfg - voor alleen klassieke VNets). Zie [configureren een virtueel netwerk met een netwerk configuratie-bestand](../articles/virtual-network/virtual-networks-using-network-configuration-file.md).

- PowerShell (voor klassieke en resourcemanager VNets).

- Azure CLI (voor klassieke en resourcemanager VNets).

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Welke adresbereiken kan ik gebruiken in mijn VNets?

U kunt openbare IP-adresbereiken en een IP-adresbereiken gedefinieerd in [RFC 1918](http://tools.ietf.org/html/rfc1918)gebruiken.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Kan ik openbare IP-adressen in mijn VNets hebben?

Ja. Zie voor meer informatie over het openbare IP-adresbereiken [openbare IP-adresruimte in een virtueel netwerk (VNet)](../articles/virtual-network/virtual-networks-public-ip-within-vnet.md). Houd er rekening mee dat het openbare IP-adressen niet direct toegankelijk is via Internet worden.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-virtual-network"></a>Is er een beperking voor het aantal subnetten in mijn virtuele netwerk?

Er is geen limiet van het aantal subnetten die u vanuit een VNet gebruiken. Alle subnetten volledig moeten zijn opgenomen in de virtuele netwerk-adresruimte en mogen met elkaar niet overlappen.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Gelden er beperkingen voor het werken met IP-adressen binnen deze subnetten?

Azure behoudt enkele IP-adressen binnen elk subnet. De eerste en laatste IP-adressen van de subnetten gereserveerd voor protocol-conformiteit, samen met 3 meer adressen die worden gebruikt voor Azure-services.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Hoe klein en hoe groot kan worden VNets en subnetten?

Het kleinste subnet dat wordt ondersteund is een /29 en de grootste is een /8 (via CIDR subnet definities).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Kan ik mijn VLAN's naar Azure VNets gebruiken?

Nee. VNets zijn Layer-3-overlays. Azure biedt geen ondersteuning voor elke laag-2-semantiek.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Kan ik aangepaste beleidsregels voor het routeren van op mijn VNets en subnetten opgeven?

Ja. U kunt de gebruiker gedefinieerde routering (UDR). Voor meer informatie over UDR, gaat u naar [gebruiker gedefinieerd Routes en IP-doorsturen](../articles/virtual-network/virtual-networks-udr-overview.md).

### <a name="do-vnets-support-multicast-or-broadcast"></a>Ondersteunen VNets multicast- of broadcast?

Nee. Microsoft biedt geen ondersteuning multicast- of broadcast.

### <a name="what-protocols-can-i-use-within-vnets"></a>Welke protocollen kan ik binnen VNets gebruiken?

U kunt standaard IP-protocollen binnen VNets gebruiken. Multicast-, broadcast, IP-in-IP-encapsulated pakketten en algemene Routing Encapsulation (GRE)-pakketten worden echter geblokkeerd binnen VNets. Standaard protocollen die werken zijn:

- TCP
- UDP
- ICMP

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Kan ik mijn standaard routers binnen een VNet ping?

Nee.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Kan ik tracert via een diagnose stellen bij connectivity?

Nee.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Kan ik subnetten toevoegen nadat de VNet is gemaakt?

Ja. Subnetten kunnen worden toegevoegd aan VNets op elk gewenst moment zo lang maken als de subnetadres niet deel van een ander subnet in de VNet uitmaakt.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Kan ik de grootte van mijn subnet wijzigen nadat ik deze hebt gemaakt?

U kunt toevoegen, verwijderen, uitvouwen of een subnet verkleinen als er geen VMs of de services die erin wordt geïmplementeerd via PowerShell-cmdlets of het bestand NETCFG. U kunt ook toevoegen, verwijderen, uitvouwen of geen voorvoegsels verkleinen, zolang de subnetten die VMs of services bevatten, worden niet beïnvloed door de wijziging.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Kan ik subnetten wijzigen nadat ik ze gemaakt?

Ja. U kunt toevoegen, verwijderen en de CIDR blokken die worden gebruikt door een VNet aanpassen.

### <a name="can-i-connect-to-the-internet-if-i-am-running-my-services-in-a-vnet"></a>Kan ik verbinding maken met internet als ik mijn services in een VNet?

Ja. Alle services geïmplementeerd binnen een VNet kunnen verbinding maken met internet. Elke cloudservice die zijn geïmplementeerd in Azure wordt aangegeven heeft een openbaar gebruikt VIP toegewezen. U moet definiëren invoer eindpunten voor PaaS rollen en eindpunten voor virtuele machines betere prestaties verbindingen van internet te accepteren.

### <a name="do-vnets-support-ipv6"></a>Ondersteunen VNets IPv6?

Nee. U kunt IPv6 niet gebruiken met VNets op dit moment.

### <a name="can-a-vnet-span-regions"></a>Kan een VNet regio's beslaan?

Nee. Een VNet is beperkt tot één regio.

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Kan ik een VNet verbinden met een ander VNet in Azure wordt aangegeven?

Ja. U kunt VNet VNet communicatie maken met behulp van de REST API's of Windows PowerShell. U kunt ook VNets via VNet Peering verbinding te maken. Meer details over peering [hier.](../articles/virtual-network/virtual-network-peering-overview.md)

## <a name="name-resolution-dns"></a>Naamresolutie (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Wat zijn mijn DNS-opties voor VNets?

Gebruik de Beslissingstabel op de pagina [Naamresolutie voor VMs en rol exemplaren](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) als leidraad voor alle DNS-opties die beschikbaar.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Kan ik DNS-servers opgeven voor een VNet?

Ja. IP-adressen van DNS-server kunt u opgeven in de VNet-instellingen. Hiermee worden toegepast als de standaard DNS-server (s) voor alle VMs in de VNet.

### <a name="how-many-dns-servers-can-i-specify"></a>Hoeveel DNS-servers kan ik opgeven?

U kunt maximaal 12 DNS-servers.

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Kan ik mijn DNS-servers niet wijzigen nadat ik het netwerk hebt gemaakt?

Ja. U kunt de lijst DNS-server voor uw VNet op elk gewenst moment wijzigen. Als u uw DNS-serverlijst wijzigt, moet u elk van de VMs opnieuw in uw VNet zodat ze volgt te werk om de nieuwe DNS-server.


### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Wat is geleverde Azure DNS en met VNets werkt dit?

Azure-voorwaarde DNS is een meerdere tenant DNS-service van Microsoft. Azure registreert al uw VMs en exemplaren van de rol in deze service. Deze service biedt naamresolutie door hostname voor VMs en rol-exemplaren die zijn opgenomen in de dezelfde cloudservice en FQDN voor VMs en exemplaren van de rol in de dezelfde VNet.

> [AZURE.NOTE] Er is een beperking op dit moment met de eerste 100 cloudservices in het virtuele netwerk voor cross-tenant naamresolutie met Azure-voorwaarde DNS. Als u uw eigen DNS-server gebruikt, is deze beperking niet van toepassing.

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>Kan ik mijn DNS-instellingen op een per-VM overschrijven / soort_jaar service?

Ja. U kunt de DNS-servers instellen per cloud service managementrapporten om de standaardinstellingen voor netwerk te overschrijven. Echter is het raadzaam gehele netwerk-DNS zoveel mogelijk te gebruiken.

### <a name="can-i-bring-my-own-dns-suffix"></a>Kan ik mijn eigen DNS-achtervoegsel?

Nee. U kunt een aangepaste DNS-achtervoegsel opgeven voor uw VNets.

## <a name="vnets-and-vms"></a>VNets en VMs

### <a name="can-i-deploy-vms-to-a-vnet"></a>Kan ik VMs implementeren naar een VNet?

Ja.

### <a name="can-i-deploy-linux-vms-to-a-vnet"></a>Kan ik Linux VMs implementeren naar een VNet?

Ja. U kunt een distro Linux worden ondersteund door Azure implementeren.

### <a name="what-is-the-difference-between-a-public-vip-and-an-internal-ip-address"></a>Wat is het verschil tussen een openbare VIP en een interne IP-adres?

- Een interne IP-adres is een IP-adres dat is toegewezen aan elke VM binnen een VNet door DHCP. Het is niet openbare omlaag. Als u een VNet hebt gemaakt, wordt het interne IP-adres van het bereik dat u hebt opgegeven in de subnetinstellingen van uw VNet toegewezen. Als u nog geen een VNet, worden een interne IP-adres nog steeds toegewezen. Het interne IP-adres blijft met de VM voor de levensduur, tenzij die VM toewijzing ongedaan is gemaakt.

- Een openbare VIP is het openbare IP-adres dat is toegewezen aan de verdeling van service of laden in de cloud. Niet is rechtstreeks aan uw NIC. VM toegewezen De VIP blijft met de cloudservice die is toegewezen totdat alle VMs in dat cloudservice zijn vrijgegeven of verwijderd. Op dat moment is deze uitgebracht.

### <a name="what-ip-address-will-my-vm-receive"></a>Welke IP-adres ontvangt mijn VM?

- **Interne IP-adres-** Als u een VM dashboard naar een VNet implementeren, ontvangt de VM een interne IP-adres van een groep interne IP-adressen die u opgeeft. VMs communiceren binnen de VNets met behulp van interne IP-adressen. Hoewel Azure een dynamische interne IP-adres toewijst, kunt u een statische adres voor uw VM aanvragen. Meer informatie over statische interne IP-adressen, gaat u naar [het instellen van een statische interne IP-adres](../articles/virtual-network/virtual-networks-reserved-private-ip.md).

- **VIP-** Uw VM is ook gekoppeld aan een VIP, hoewel een VIP nooit aan de VM rechtstreeks toegewezen is. Een VIP is een openbare IP-adres zijn dat kan worden toegewezen aan uw cloudservice. U kunt desgewenst een VIP reserveren voor uw cloudservice.

- **ILPIP-** U kunt ook een exemplaar niveau openbare IP-adres (ILPIP). ILPIPs rechtstreeks zijn die is gekoppeld aan de VM, in plaats van de cloudservice. Meer informatie over ILPIPs, gaat u naar [Het openbare IP-overzicht exemplaar niveau](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).

### <a name="can-i-reserve-an-internal-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Kan ik een interne IP-adres reserveren voor een VM dat ik op een later tijdstip wilt maken?

Nee. Een interne IP-adres kan niet worden gereserveerd. Als een interne IP-adres beschikbaar is worden deze toegewezen aan een rol of VM exemplaar de DHCP-server. Die VM al dan niet de fase die u wilt dat het interne IP-adres moet worden toegewezen aan. U kunt echter het interne IP-adres van een reeds gemaakte VM om alle beschikbare interne IP-adres te wijzigen.

### <a name="do-internal-ip-addresses-change-for-vms-in-a-vnet"></a>Interne IP-adressen wijzigen voor VMs in een VNet doen?

Ja. Interne IP-adressen gedurende met de VM de levensduur tenzij de VM toewijzing ongedaan is gemaakt. Wanneer een VM toewijzing ongedaan is gemaakt, wordt het interne IP-adres uitgebracht, tenzij u een statische interne IP-adres voor uw VM gedefinieerd. Als de VM gewoon gestopt (en niet in de status **gestopt (Deallocated)**hebt opgeslagen) is het IP-adres, blijven toegewezen voor VM.

### <a name="can-i-manually-assign-ip-addresses-to-nics-in-vms"></a>Kan ik IP-adressen handmatig naar NIC in VMs toewijzen?

Nee. U moet een interface-eigenschappen van VMs niet wijzigen. Wijzigingen kunnen leiden tot mogelijk is verbroken voor VM.

### <a name="what-happens-to-my-ip-addresses-if-i-shut-down-a-vm"></a>Wat gebeurt er met Mijn IP-adressen als ik een VM afsluiten?

Niets. De IP-adressen (openbare VIP en interne IP-adres) blijven met uw cloudservice of VM.

> [AZURE.NOTE] Als u afsluiten gewoon de VM wilt, geen gebruikt u de beheerportal kunt doen. De knop Afsluiten wordt momenteel, de virtuele machine toewijzing.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-re-deploying"></a>Kan ik overstappen VMs één subnet naar een ander subnet in een VNet zonder te opnieuw implementeren?

Ja. Meer informatie vindt u [hier](../articles/virtual-network/virtual-networks-move-vm-role-to-subnet.md).

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Kan ik een statische MAC-adres configureren voor mijn VM?

Nee. Een MAC-adres kan geen statisch worden geconfigureerd.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-it-has-been-created"></a>Het MAC-adres ongewijzigd blijft voor mijn VM zodra deze is gemaakt?

Ja, het MAC-adres ongewijzigd blijft voor een VM Hoewel de VM is gestopt (deallocated) en uitgebracht.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Kan ik verbinding maken met internet uit een VM in een VNet?

Ja. Alle services geïmplementeerd binnen een VNet kunnen verbinding maken met Internet. Bovendien heeft elke cloudservice die zijn geïmplementeerd in Azure wordt aangegeven een openbaar gebruikt VIP toegewezen. U hoeft te definiëren invoer eindpunten voor PaaS rollen en eindpunten voor VMs betere prestaties verbindingen van Internet te accepteren.

## <a name="vnets-and-services"></a>VNets en -Services

### <a name="what-services-can-i-use-with-vnets"></a>Voor welke services kan ik gebruiken met VNets?

U kunt alleen berekeningscluster services binnen VNets gebruiken. Berekent de services zijn beperkt tot Cloudservices (web en werknemer rollen) en VMs.

### <a name="can-i-use-web-apps-with-virtual-network"></a>Kan ik Web Apps gebruiken met Virtual Network?

Ja. U kunt de Web-Apps in een VNet ASE (App Service omgeving) met implementeren. Toevoegen aan dat Web Apps veilig kunt verbinding maken en bronnen in uw VNet Azure toegang als u punt-naar-site geconfigureerd voor uw VNet hebt. Zie de volgende onderwerpen voor meer informatie:


- [Maken van WebApps in een App Service-omgeving](../articles/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)

- [Integratie van Web Apps virtuele netwerken](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)

- [Integratie van VNet en hybride verbindingen met WebApps gebruiken](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/)

- [Een web-app integreren met een Azure Virtual Network](../articles/app-service-web/web-sites-integrate-with-vnet.md)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Kan ik cloudservices met web en werknemer rollen (PaaS) in een VNet implementeren?

Ja. U kunt PaaS services binnen VNets implementeren.

### <a name="how-do-i-deploy-paas-roles-to-a-vnet"></a>Hoe kan ik PaaS rollen implementeren naar een VNet?

U kunt dit doen door het opgeven van de naam van de VNet en de toewijzingen van de /subnet rol in de sectie netwerk configuratie van uw service te configureren. U hoeft niet bijwerken van uw binaire bestanden.

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Kan ik mijn services en afmelden bij VNets verplaatsen?

Nee. U kunt services en afmelden bij VNets niet verplaatsen. U moet verwijderen en opnieuw implementeren de service om deze te verplaatsen naar een andere VNet.

## <a name="vnets-and-security"></a>VNets en beveiliging

### <a name="what-is-the-security-model-for-vnets"></a>Wat is het beveiligingsmodel voor VNets?

VNets zijn volledig geïsoleerd uit elkaar en andere services van de Azure infrastructuur. Een VNet is een grens vertrouwen.

### <a name="can-i-define-acls-or-nsgs-on-my-vnets"></a>Kan ik ACL's of NSGs definiëren op mijn VNets?

Nee. U kunt geen ACL's of NSGs naar VNets koppelen. Echter ACL's kunnen worden gedefinieerd op invoer eindpunten voor VMs die zijn geïmplementeerd naar een VNets en NSGs kan worden gekoppeld aan subnetten of NIC's.

### <a name="is-there-a-vnet-security-whitepaper"></a>Is er een VNet beveiliging whitepaper?

Ja. U kunt deze downloaden [hier](http://go.microsoft.com/fwlink/?LinkId=386611).

## <a name="apis-schemas-and-tools"></a>API's, schema's en hulpprogramma 's

### <a name="can-i-manage-vnets-from-code"></a>Kan ik VNets van code beheren?

Ja. U kunt REST API's VNets en cross-premises connectivity beheren. Meer informatie vindt u [hier](http://go.microsoft.com/fwlink/?LinkId=296833).

### <a name="is-there-tooling-support-for-vnets"></a>Is er ondersteuning tooling voor VNets?

Ja. U kunt hulpmiddelen voor PowerShell en opdrachtregel gebruiken voor een groot aantal platforms. Meer informatie vindt u [hier](http://go.microsoft.com/fwlink/?LinkId=317721).
