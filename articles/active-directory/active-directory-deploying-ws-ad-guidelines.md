<properties
   pageTitle="Richtlijnen voor het implementeren van Windows Server Active Directory op Azure virtuele Machines | Microsoft Azure"
   description="Als u hoe AD-domeinservices en AD Federation Services on-premises implementatie weet, leert u hoe ze werken op Azure virtuele machines."
   services="active-directory"
   documentationCenter=""
   authors="femila"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/27/2016"
   ms.author="femila"/>

# <a name="guidelines-for-deploying-windows-server-active-directory-on-azure-virtual-machines"></a>Richtlijnen voor het implementeren van Windows Server Active Directory op Azure virtuele machines

In dit artikel wordt uitgelegd dat de belangrijke verschillen tussen de Windows Server Active Directory Domain Services (AD DS) en Active Directory Federation Services (AD FS) on-premises versus ze op Microsoft Azure virtuele machines implementeren.

## <a name="scope-and-audience"></a>Zoekbereik en publiek

Dit artikel is bedoeld voor al ervaring met lokale Active Directory implementeren. Deze de verschillen tussen het distribueren van Active Directory op Microsoft Azure virtuele machines/Azure virtuele netwerken en traditionele on-premises Active Directory-implementaties, bedekt. Azure virtuele machines en Azure virtuele netwerken uitmaken deel van een infrastructuur-als-een-Service (IaaS) aanbod voor organisaties om te profiteren van bronnen in de cloud.

Zie de [Implementatiehandleiding voor AD DS](https://technet.microsoft.com/library/cc753963) of [de AD FS-implementatie plannen](https://technet.microsoft.com/library/dn151324.aspx) zo nodig voor items die niet bekend met AD-implementatie bent.

In dit artikel wordt ervan uitgegaan dat de lezer vertrouwd met de volgende begrippen:

- Windows Server AD DS-implementatie en beheer
- Installatie en configuratie van DNS voor de ondersteuning van een Windows Server AD DS-infrastructuur
- Windows Server AD FS-implementatie en beheer
- Implementeren, configureren en beheren van gebruikmakende toepassingen leveranciers (websites en web services) die Windows Server AD FS-tokens kunnen gebruiken
- Algemene VM concepten, zoals het configureren van een virtuele machine, virtuele schijven en virtuele netwerken

In dit artikel worden de vereisten voor een hybride implementatiescenario in welke Windows Server AD DS of AD FS zijn gedeeltelijk uitgevouwen on-premises implementatie en gedeeltelijk geïmplementeerd op Azure virtuele machines gemarkeerd. Het document behandelt eerst de kritieke verschillen tussen het uitvoeren van Windows Server AD DS en AD FS op Azure virtuele machines versus on-premises implementatie en belangrijke beslissingen die betrekking hebben op ontwerpen en implementeren. De rest van het papier wordt uitgelegd richtlijnen voor elk van de beschikking wordt verwezen uitgebreider en hoe u de richtlijnen van toepassing op verschillende scenario's voor implementatie.

In dit artikel wordt niet de configuratie van [Azure Active Directory](http://azure.microsoft.com/services/active-directory/), beschreven welke is een REST gebaseerde service identiteitsbeheer en mogelijkheden voor het beheer van access voor cloud-toepassingen. Azure Active Directory (Azure AD) en Windows Server AD DS zijn, echter ontworpen om samen te werken om te bieden een oplossing voor identiteits- en toegangsbeheer voor vandaag hybride IT-omgevingen en moderne toepassingen. Meer informatie over de verschillen en de relaties tussen Windows Server AD DS en Azure AD, zodat het volgende overwegen:

1. U kunt Windows Server AD DS in de cloud op Azure virtuele machines uitvoeren wanneer u Azure gebruikt voor het uitbreiden van uw on-premises implementatie-datacenter in de cloud.
2. U kunt Azure AD gebruiken voor uw gebruikers eenmalige aanmelding tot Software-als-een-Service (SaaS)-toepassingen. Microsoft Office 365 gebruikmaakt van deze technologie, bijvoorbeeld en toepassingen op Azure of andere platforms cloud kunnen ook gebruiken.
3. U kunt Azure AD (de Access Control-Service) gebruiken om gebruikers Meld u aan te laten in het gebruik van identiteiten uit Facebook, Google, Microsoft en andere identiteitsproviders naar toepassingen die worden gehost in de cloud of on-premises implementatie.

Zie voor meer informatie over deze verschillen, [Azure identiteit](fundamentals-identity.md).

## <a name="related-resources"></a>Verwante resources

U kunt downloaden en de [Evaluatie van Azure virtuele machines gereedheid](https://www.microsoft.com/download/details.aspx?id=40898)uitvoeren. De beoordeling wordt automatisch uw on-premises omgeving controleren en een aangepaste genereren op basis van de richtlijnen in dit onderwerp vindt u de omgeving migreren naar Azure gevonden.

We raden u ook eerst de zelfstudies, handleidingen en video's die betrekking op de volgende onderwerpen:

- [Een virtueel netwerk alleen de Cloud in de Portal van Azure configureren](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)
- [Een VPN-verbinding voor de Site-naar-Site configureren in de Portal van Azure](../vpn-gateway/vpn-gateway-site-to-site-create.md)
- [Een nieuwe Active Directory-bos installeren op een Azure virtuele netwerk](active-directory-new-forest-virtual-machine.md)
- [Een replicadomeincontroller voor Active Directory op Azure installeren](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
- [Microsoft Azure IT Pro IaaS: (01) VM over grondbeginselen](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
- [Microsoft Azure IT Pro IaaS: (05) maken van virtuele netwerken en Cross-Premises-connectiviteit](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)

## <a name="introduction"></a>Inleiding

De fundamentele vereisten voor het implementeren van Windows Server Active Directory op Azure virtuele machines verschillen weinig van deze implementeert in de on-premises implementatie virtuele machines (en tot op zekere fysieke machines). Bijvoorbeeld in geval van Windows Server AD DS, als de domeincontrollers (DCs) die u op Azure virtuele machines implementeert kopieën in een bestaande zijn on-premises implementatie bedrijfs-domein, en vervolgens de Azure-implementatie grotendeels op dezelfde manier verwerkt kunt als u een willekeurige extra Windows Server Active Directory-site mogelijk behandelen. Dat wil zeggen, moeten subnetten worden gedefinieerd in Windows Server AD DS, een site hebt gemaakt, de subnetten gekoppeld voor deze site en verbonden met andere sites die gebruikmaken van de juiste site-koppelingen. Er zijn echter enkele verschillen die gelden voor alle Azure implementaties en sommige die variëren op basis van het specifieke scenario. Onderstaand vindt u twee belangrijke verschillen:

### <a name="azure-virtual-machines-may-need-connectivity-to-the-on-premises-corporate-network"></a>Azure virtuele machines mogelijk moet u verbinding met het bedrijfsnetwerk bevinden on-premises implementatie.

Azure virtuele machines terug verbinden met een on-premises implementatie-bedrijfsnetwerk vereist Azure virtuele netwerk op, dat bevat een site-naar-site of site-naar-punt virtuele particuliere netwerk (VPN) onderdeel Azure virtuele machines naadloos verbinding te maken en on-premises machines. Dit onderdeel VPN kan ook inschakelen voor on-premises implementatie domein lidcomputers toegang hebben tot een Windows Server Active Directory-domein waarvan domeincontrollers uitsluitend op Azure virtuele machines worden gehost. Het is belangrijk te weten echter dat als de VPN-verbinding mislukt, verificatie en andere bewerkingen die afhankelijk van Windows Server Active Directory zijn, ook mislukt. Terwijl gebruikers zich kunnen aanmelden met behulp van bestaande: in cache opgeslagen referenties, alle peer-to-peer of client-naar-server verificatiepogingen waarvoor tickets nog moeten worden uitgegeven of geworden verlopen, mislukt.

Zie [Virtual Network](http://azure.microsoft.com/documentation/services/virtual-network/) voor een video demonstratie en een lijst met zelfstudies met stapsgewijze instructies, inclusief [een VPN-verbinding van het Site-naar-Site in de portal van Azure configureren](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [AZURE.NOTE] U kunt ook Windows Server Active Directory implementeren in een netwerk met een Azure virtuele die geen verbinding met een on-premises netwerk. De richtlijnen in dit onderwerp is echter ervan uitgegaan dat een Azure virtuele netwerk wordt gebruikt, omdat het IP-adressen van de mogelijkheden die essentieel voor Windows Server zijn.

### <a name="static-ip-addresses-must-be-configured-with-azure-powershell"></a>Statische IP-adressen moeten worden geconfigureerd met Azure PowerShell.

Dynamische adressen al dan niet standaard zijn toegewezen, maar de cmdlet Set-AzureStaticVNetIP gebruiken in plaats daarvan een statisch IP-adres toewijzen. Die Hiermee stelt u een statische IP-adres zijn dat via service retoucheren en VM afsluiten/opnieuw starten blijft. Zie [statische interne IP-adres voor virtuele machines](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)voor meer informatie.

## <a name="BKMK_Glossary"></a>Termen en definities

Hier volgt een niet-uitgebreide lijst met voorwaarden voor verschillende Azure technologieën die in dit artikel wordt verwezen.

- **Azure virtuele machines**: het IaaS aanbod Azure waarmee klanten kunnen implementeren VMs uitgevoerd vrijwel elke traditioneel on-premises implementatie serverwerklast.

- **Azure virtuele netwerk**: de Netwerkservice in Azure wordt aangegeven waarmee klanten maken en beheren van virtuele netwerken in Azure wordt aangegeven en ze veilig te koppelen aan hun eigen on-premises implementatie netwerkinfrastructuur via een VPN (VPN).

- **Virtuele IP-adres**: een internetgerichte IP-adres dat is niet gekoppeld aan een specifieke computer of netwerk interfacekaart. Cloudservices zijn een virtueel IP-adres voor het ontvangen van netwerkverkeer waarin omgeleid naar een VM Azure toegewezen. Een virtueel IP-adres is een eigenschap van een cloudservice die een of meer Azure virtuele machines kan bevatten. Bedenk ook dat een Azure virtuele netwerk een of meer cloudservices kan bevatten. Virtuele IP-adressen bieden systeemeigen taakverdeling mogelijkheden.

- **Dynamische IP-adres**: dit is het IP-adres dat is alleen interne. Deze moet worden geconfigureerd als een statische IP-adres (via de cmdlet Set-AzureStaticVNetIP) voor VMs die als host van de rollen van domeincontroller/DNS-server.

- **Service retoucheren**: het proces waarin Azure automatisch geeft als een service met actief opnieuw resultaat wanneer wordt vastgesteld dat de service is mislukt. Service retoucheren is een van de aspecten van Azure die ondersteuning biedt voor beschikbaarheid en tolerantie. Terwijl waarschijnlijk, is het resultaat volgen van een service incident retoucheren voor een domeincontroller waarop een VM is vergelijkbaar met een niet-geplande opnieuw opstarten, maar heeft een paar nadelen:

 - Het virtuele netwerkadapter in VM wordt gewijzigd
 - Het MAC-adres van de virtuele netwerkadapter wordt gewijzigd
 - De Processor/CPU-ID van de VM wordt gewijzigd
 - De IP-configuratie van de virtuele netwerkadapter verandert niet zo lang maken als de VM wordt gekoppeld aan een virtueel netwerk en het IP-adres van de VM statisch.

 Geen van deze problemen invloed op Windows Server Active Directory, omdat er geen afhankelijkheid van de MAC-adres of Processor/CPU-ID en alle Windows Server Active Directory-implementaties op Azure worden aanbevolen moet worden uitgevoerd in een netwerk met een Azure virtuele als hierboven beschreven.

## <a name="is-it-safe-to-virtualize-windows-server-active-directory-domain-controllers"></a>Is het veilig virtueel van Windows Server Active Directory-domeincontroller?

Windows Server Active Directory DCs distribueren op Azure virtuele machines is onderhevig aan dezelfde richtlijnen als het uitvoeren van DCs on-premises implementatie in een virtuele machine. Het is verstandig veilige gevirtualiseerde DCs uitgevoerd, zolang de richtlijnen voor het back-up en herstellen van DCs worden gevolgd. Zie voor meer informatie over beperkingen en richtlijnen voor het uitvoeren van gevirtualiseerde DCs, [Domeincontrollers uitgevoerd in Hyper-V](https://technet.microsoft.com/library/dd363553).

Hypervisors geven of trivialize technologieën die ertoe van problemen voor veel gedistribueerde systemen leiden kunnen, met inbegrip van Windows Server Active Directory. Bijvoorbeeld op een fysieke server, u kunt een schijf klonen of niet-ondersteunde methoden gebruiken om te terugdraaien van de status van een server, inclusief met SANs enzovoort, maar hierbij op een fysieke server is veel moeilijker dan het herstellen van een momentopname VM in een hypervisor. Azure biedt functionaliteit hetgeen in dezelfde ongewenste voorwaarde resulteren kan. U moet de VHD-bestanden van DCs bijvoorbeeld niet kopiëren in plaats van de uitvoering van regelmatige back-ups omdat deze terug te zetten in een vergelijkbare situatie resulteren kan over het gebruik van functies voor het herstellen van momentopname.

Deze terugdraaien van acties introduceren USN bellen dat tot permanent uiteenlopende Staten tussen DCs leiden kunnen. Dat kan problemen veroorzaken, zoals:

- Zich objecten
- Inconsistente wachtwoorden
- Inconsistente kenmerkwaarden
- Schema problemen als het model schema wordt hersteld

Zie voor meer informatie over hoe DCs ondervindt, [USN en USN-hersteloptie](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx#usn_and_usn_rollback).

Beginnen met Windows Server 2012, [Extra garanties zijn ingebouwd in AD DS](https://technet.microsoft.com/library/hh831734.aspx). Deze beveiligingsmaatregelen beschermen gevirtualiseerde domeincontrollers ten opzichte van de genoemde problemen, zolang de onderliggende hypervisor platform VM-GenerationID ondersteunt. Azure ondersteunt VM-GenerationID, wat betekent dat domeincontrollers waarop Windows Server 2012 of hoger op Azure virtuele machines de extra beveiligingsfuncties.

> [AZURE.NOTE] U moet afsluiten en opnieuw opstarten een VM die wordt uitgevoerd als functie van de domeincontroller in Azure wordt aangegeven in het gastbesturingssysteem in plaats van met de optie **Afsluiten** in de portal van Azure klassieke. Tegenwoordig kunnen met behulp van de klassieke portal afsluiten van een VM zorgt ervoor dat de VM om te worden opgeheven. Een deallocated VM het voordeel van kosten niet actief is, maar ook worden opnieuw ingesteld de VM-GenerationID, namelijk ongewenste voor een domeincontroller. Wanneer de VM-GenerationID opnieuw is ingesteld, de invocationID van de AD DS-database is ook opnieuw instellen, de RID-groep wordt verwijderd en SYSVOL is gemarkeerd als niet-gezaghebbende. Zie [Inleiding tot Active Directory Domain Services (AD DS) Virtualization](https://technet.microsoft.com/library/hh831734.aspx) en [DFSR veilig virtualisatie](http://blogs.technet.com/b/filecab/archive/2013/04/05/safely-virtualizing-dfsr.aspx)voor meer informatie.

## <a name="why-deploy-windows-server-ad-ds-on-azure-virtual-machines"></a>Waarom implementeren Windows Server AD DS op Azure virtuele Machines?

Veel scenario's voor Windows Server AD DS-implementatie zijn zeer geschikt voor implementatie als VMs op Azure. Stel dat u hebt een bedrijf in Europa die vereist zijn voor gebruikers in een externe locatie in Azië verifiëren. Het bedrijf is Windows Server Active Directory-DCs in Azië niet eerder hebt geïmplementeerd vanwege de kosten voor het implementeren van beperkte en ze expertise als u wilt de servers post-implementatie beheren. Hierdoor worden verificatie aanmeldingsaanvragen van Azië verwerkt door DCs in Europa met niet-optimale resultaten. In dit geval kunt u een domeincontroller op een VM die u hebt opgegeven moet worden uitgevoerd binnen het Azure datacenter in Azië implementeren. Die domeincontroller koppelen aan een Azure virtuele netwerk die rechtstreeks naar de externe locatie is gekoppeld, wordt verificatie prestaties verbeteren.

Azure is ook zeer geschikt als vervanging naar anders dure noodgevallen herstel (DR)-sites. De relatief lage kosten van een klein aantal domeincontrollers en één virtuele netwerk op Azure hostingprovider vertegenwoordigt een aantrekkelijk alternatief.

Tot slot wilt u implementeren van een toepassing op Azure, zoals SharePoint, die is vereist Windows Server Active Directory, maar heeft geen afhankelijkheid van de on-premises netwerk of het zakelijke Windows Server Active Directory. In dit geval een geïsoleerd bos op Azure om te voldoen aan de SharePoint implementeren van serververeisten is optimaal. Klik nogmaals wordt implementeren van netwerktoepassingen die nodig connectivity naar de on-premises netwerk en het bedrijf Active Directory hebben ook ondersteund.

> [AZURE.NOTE] Aangezien deze een layer-3-verbinding, kunt lidservers waarop on-premises implementatie om te kunnen DCs die als Azure virtuele machines worden uitgevoerd in een netwerk met Azure virtuele ook uitschakelen door het onderdeel van de VPN waarmee connectiviteit tussen een Azure virtuele netwerk en een on-premises netwerk. Maar als de VPN niet beschikbaar is, communicatie tussen on-premises computers en Azure-domeincontrollers niet werkt, resulterende in verificatie en verschillende andere fouten.  

## <a name="contrasts-between-deploying-windows-server-active-directory-domain-controllers-on-azure-virtual-machines-versus-on-premises"></a>Dus iets anders dan tussen het implementeren van Windows Server Active Directory-domeincontroller op Azure virtuele Machines versus on-premises implementatie

- Voor een scenario Windows Server Active Directory-implementatie die meer dan een enkel VM bevat, is dit nodig is voor het gebruik van een Azure virtuele netwerk voor IP-adres consistentie. Houd er rekening mee dat deze handleiding wordt ervan uitgegaan dat DCs in een netwerk met een Azure virtual.

- Als u met de on-premises implementatie DCs, wordt statische IP-adressen aanbevolen. Een statische IP-adres kan alleen worden geconfigureerd met behulp van Azure PowerShell. Zie [statische interne IP-adres voor VMs](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/) voor meer informatie. Als u monitoring systemen of andere oplossingen voor statische IP-adresconfiguratie in het gastbesturingssysteem te controleren hebt, kunt u hetzelfde statische IP-adres toewijzen aan de eigenschappen van het netwerkadapter van VM. Maar houd er rekening mee dat de netwerkadapter worden verwijderd als de VM betrokken service retoucheren of in de klassieke portal is afgesloten en het adres opgeheven heeft. In dat geval moet het statische IP-adres binnen de Gast worden opnieuw in te stellen.

- VMs implementeren in een netwerk met een virtuele niet geven over het algemeen (of vereisen) connectiviteit met een on-premises netwerk; het virtuele netwerk kunt alleen deze mogelijkheid. U moet een virtueel netwerk voor persoonlijke communicatie tussen Azure en uw on-premises netwerk maken. U moet een VPN-eindpunt op de on-premises netwerk implementeren. De VPN wordt geopend vanuit Azure naar de on-premises netwerk. Voor meer informatie raadpleegt u [Virtuele netwerk-overzicht](../virtual-network/virtual-networks-overview.md) en [een VPN-verbinding van het Site-naar-Site in de Portal Azure configureren](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [AZURE.NOTE] Een optie voor het [maken van een VPN-verbinding voor de punt-naar-site](../vpn-gateway/vpn-gateway-point-to-site-create.md) is beschikbaar voor afzonderlijke computers met Windows rechtstreeks verbinden met een Azure virtuele netwerk.

- Of u nu een virtueel maakt netwerk of niet, Azure kosten voor egress verkeer, maar niet ingress. Verschillende opties voor Windows Server Active Directory-ontwerp invloed kunnen zijn op hoeveel netwerkverkeer egress wordt gegenereerd door een implementatie. Bijvoorbeeld bestandsgrootten een alleen-lezen domeincontroller implementeren (alleen-lezen domeincontroller) egress-verkeer is toegestaan omdat deze niet uitgaande gerepliceerd. Maar het besluit om te implementeren van een alleen-lezen domeincontroller moet worden gewogen tegen de behoefte schrijven bewerkingen ten opzichte van de domeincontroller en de [compatibiliteit](https://technet.microsoft.com/library/cc755190) met toepassingen en services op de site met RODC uit te voeren. Zie [Azure op overzichtelijke prijzen](http://azure.microsoft.com/pricing/)voor meer informatie over het verkeer kosten.

- Terwijl u voltooid hebt bepalen op welke server bronnen die u wilt gebruiken voor VMs on-premises implementatie, zoals RAM, schijf grootte en enzovoort, op Azure u moet selecteren uit een lijst met vooraf geconfigureerde server grootte. Een gegevensschijf is voor een domeincontroller, naast de schijf besturingssysteem nodig om te kunnen opslaan van de Windows Server Active Directory-database.

## <a name="can-you-deploy-windows-server-ad-fs-on-azure-virtual-machines"></a>Kunt u Windows Server AD FS op Azure virtuele machines implementeren?

Ja, kunt u Windows Server AD FS op Azure virtuele machines implementeren, en de [Aanbevolen procedures voor AD FS-implementatie](https://technet.microsoft.com/library/dn151324.aspx) on-premises implementatie gelden voor zowel AD FS-implementatie op Azure. Maar enkele van de aanbevolen procedures zoals taakverdeling en beschikbaarheid vereisen technologieën voorbij wat AD FS biedt zelf. Ze moeten worden verstrekt door de onderliggende infrastructuur. Laten we eens enkele van deze aanbevolen procedures en Zie hoe ze met behulp van Azure VMs en een Azure virtuele netwerken kunnen worden bereikt.

1. **Nooit laten zien beveiliging token service (STS) servers rechtstreeks met Internet.**

    Dit is belangrijk omdat de STS beveiligingstokens. Hierdoor worden servers STS zoals AD FS-servers met hetzelfde niveau van bescherming tegen als domeincontroller behandeld. Als een STS betrouwbaar is, hebben kwaadwillende gebruikers de mogelijkheid om te verlenen van toegangstokens mogelijk met claims van hun keuze voor gebruikmakende toepassingen van derden en andere STS-servers in organisaties vertrouwen.

2. **Active Directory-domeincontroller voor alle gebruikersdomeinen in hetzelfde netwerk als de AD FS-servers implementeren.**

    Active Directory Domain Services AD FS-servers gebruiken om gebruikers te verifiëren. Het wordt aanbevolen om te implementeren domeincontrollers in hetzelfde netwerk als de AD FS-servers. Hierdoor bedrijfscontinuïteit geval de koppeling tussen de Azure netwerk- en uw on-premises netwerk verbroken is en Hiermee kunt u lagere latentie en betere prestaties voor aanmeldingen.

3. **Meerdere AD FS-knooppunten voor beschikbaarheid en het verdelen van het selectievakje laden implementeren.**

    In de meeste gevallen het mislukken van een toepassing waarmee AD FS niet acceptabel is omdat de toepassingen die opgevolgd moeten beveiligingstokens zijn vaak bedrijfskritieke. Hierdoor en omdat AD FS bevindt zich nu in het kritieke pad naar de opdracht kritieke-toepassingen openen, de AD FS-service moet ten zeerste beschikbaar tot en met meerdere AD FS-proxy's en AD FS-servers. Als u wilt bereiken verdeling van aanvragen, zijn meestal netwerktaakverdelers geïmplementeerd vóór zowel de AD FS-proxy's als de AD FS-servers.

4. **Een of meer toepassingsproxy Web knooppunten voor internettoegang implementeren.**

    Als gebruikers nodig hebt voor toegang tot toepassingen die door de AD FS-service is beveiligd, moet de AD FS-service verkrijgbaar via internet. Dit is bereikt door de service Web toepassingsproxy implementeren. Het is raadzaam kunt implementeren van meer dan één knooppunt voor de toepassing van beschikbaarheid en taakverdeling.

5. **Toegang vanaf het Web toepassingsproxy knooppunten beperken tot interne netwerkbronnen.**

    Als u wilt toestaan van externe gebruikers toegang AD FS via internet, moet u implementeren Web toepassingsproxy knooppunten (of AD FS-Proxy in eerdere versies van Windows Server). De webtoepassing proxy knooppunten worden rechtstreeks blootgesteld aan Internet. Ze zijn niet verplicht voor het domein behoren worden en ze alleen toegang tot de AD FS-servers TCP-poorten 443 en 80 nodig hebt. Het wordt ten zeerste aanbevolen dat communicatie met alle andere computers (met name domeincontrollers) is geblokkeerd.

    Dit is meestal behaald on-premises implementatie door middel van een DMZ. Firewalls met een "witte" lijst bewerkingsmodus verkeer beperken uit de DMZ tot de on-premises netwerk, (dat wil zeggen alleen verkeer uit de opgegeven IP-adressen en poorten opgegeven is toegestaan en alle andere verkeer geblokkeerd).

Het volgende diagram ziet u een traditionele on-premises implementatie AD FS-implementatie.

![Diagram van een traditionele on-premises Active Directory Federation Services-implementatie](media/active-directory-deploying-ws-ad-guidelines/ADFS_onprem.png)

Echter omdat Azure biedt geen systeemeigen, volledige firewall videomogelijkheden, andere opties moeten worden gebruikt voor het beperken van verkeer. De volgende tabel ziet u elke optie en de voor- en nadelen.

| Optie | Gebruik maken | Nadeel |
| ------ | --------- | ------------ |
| [Azure netwerk ACL 's](virtual-networks-acl.md) | Minder dure en eenvoudiger eerste configuratie | Aanvullende netwerk ACL-configuratie vereist als een nieuwe VMs worden toegevoegd aan de implementatie |
| [Barracuda NG firewall](https://www.barracuda.com/products/ngfirewall) | "Witte" lijst de modus van bewerking en dit is geen ACL netwerkconfiguratie | Extra kosten en de complexiteit voor de eerste installatie |

De uitgebreide stappen voor het implementeren van AD FS zijn in dit geval:

1. Maak een virtueel netwerk met cross-premises-connectiviteit met een VPN- of [ExpressRoute](http://azure.microsoft.com/services/expressroute/).

2. Domeincontrollers op het virtuele netwerk distribueren. Deze stap is optioneel, maar aanbevolen.

3. Een domein behoren AD FS-servers op het virtuele netwerk implementeren.

4. Maak een [interne laden verdeeld instellen](http://azure.microsoft.com/blog/internal-load-balancing/) met de AD FS-servers bevat en het gebruik van een nieuwe privé IP-adres binnen het virtuele netwerk (een dynamische IP-adres).

  1. DNS om te maken van de FQDN-naam zodat deze verwijzen naar het persoonlijk (dynamische) IP-adres van de set interne taakverdeling bijwerken.

5. Maak een cloudservice (of een afzonderlijke virtual network) voor de Web-toepassingsproxy knooppunten.

6. De Web-toepassingsproxy knooppunten in een virtuele netwerk of cloudservice implementeren

  1. Een externe taakverdeling set waarin de knooppunten Web toepassingsproxy maken.

  2. Werk de externe DNS-naam (Fully Qualified) zodat deze verwijzen naar de cloud service openbare IP-adres (het virtuele IP-adres).

  3. AD FS-proxy's als u wilt gebruiken de FQDN-naam die met het interne taakverdeling instellen voor de AD FS-servers overeenkomt configureren.

  4. Op claims gebaseerde websites als u wilt gebruiken de externe FQDN-naam voor de claimprovider van hun bijwerken.

7. Het beperken van toegang tussen toepassingsproxy Web naar een willekeurige computer in het virtuele AD FS-netwerk.

Als u wilt beperken verkeer, de set taakverdeling voor de verdeling van de Azure interne belasting moet worden geconfigureerd voor alleen verkeer naar TCP-poorten 80 en 443 en alle andere verkeer naar het interne dynamische IP-adres van de set van taakverdeling is verbroken.

![Diagram van ADFS netwerk ACL's met TCP 443 + 80 toegestaan.](media/active-directory-deploying-ws-ad-guidelines/ADFS_ACLs.png)

Verkeer met de AD FS-servers zou mag alleen op basis van de volgende bronnen:

- De verdeling van de Azure interne belasting.
- Het IP-adres van een beheerder van de on-premises netwerk.

> [AZURE.WARNING] Het ontwerp moet Web toepassingsproxy knooppunten voorkomen dat u een andere VMs in het Azure virtuele netwerk of locaties op de on-premises netwerk. Die kunnen worden aangebracht firewallregels configureren in de on-premises implementatie toestel voor Express Route verbindingen of het apparaat VPN voor site-naar-site VPN-verbindingen.

Een nadeel van deze optie is dat u voor het configureren van het netwerk ACL's voor meerdere apparaten, zoals een interne taakverdeling, de AD FS-servers en andere servers die aan het virtuele netwerk wordt toegevoegd. Als elk apparaat wordt toegevoegd aan de implementatie zonder het configureren van netwerk ACL's om verkeer door te beperken, worden de volledige implementatie risico. Als het IP-adressen van de knooppunten Web toepassingsproxy verandert, het netwerk ACL's moet opnieuw worden ingesteld (wat betekent dat de proxy's moeten worden geconfigureerd voor gebruik van [statische dynamische IP-adressen](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)).

![ADFS op Azure met netwerk ACL's.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure.png)

Er is een andere optie voor het toestel [Barracuda NG Firewall](https://www.barracuda.com/products/ngfirewall) gebruiken om te beheren van verkeer tussen AD FS-proxyservers en de AD FS-servers. Deze optie voldoet aan de aanbevolen procedures voor beveiliging en betere beschikbaarheid en minder beheer na de eerste configuratie is vereist, omdat het toestel Barracuda NG Firewall een modus "witte" lijst van het beheer van de firewall biedt en deze kan worden geïnstalleerd rechtstreeks op een Azure virtuele netwerk. Die hoeven netwerk ACL's configureren voor elk gewenst moment die een nieuwe server wordt toegevoegd aan de implementatie. Maar deze optie wordt toegevoegd eerste installatie complexiteit en kosten.

In dit geval zijn twee virtuele netwerken geïmplementeerd in plaats van één. Wij bellen ze VNet1 en VNet2. VNet1 bevat de proxy's en VNet2 bevat de STSs en de netwerkverbinding terug naar het bedrijfsnetwerk bevinden. VNet1 is dus fysiek (die mogelijk vrijwel) geïsoleerd van VNet2 en op zijn beurt met het bedrijfsnetwerk. VNet1 vervolgens is verbonden met VNet2 met behulp van een speciaal tunneling technologie bekend als Transport onafhankelijke netwerk architectuur (TINA). De tunnel TINA wordt gekoppeld aan elk van de virtuele netwerken met een firewall Barracuda NG, één Barracuda op elk van de virtuele netwerken.  Voor hoge beschikbaarheid, is het aanbevolen dat u twee barracuda's in een netwerk met elke virtuele; implementeren Eén actief, de andere passieve. Ze bieden erg uitgebreide mogelijkheden gebruik waarmee ons nabootsen van de werking van een DMZ traditionele on-premises implementatie in Azure wordt aangegeven.

![ADFS op Azure met firewall.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure_firewall.png)

Zie voor meer informatie [AD FS: uitbreiden van een claims-hoogte on-premises implementatie front-end-toepassing met Internet](#BKMK_CloudOnlyFed).

### <a name="an-alternative-to-ad-fs-deployment-if-the-goal-is-office-365-sso-alone"></a>Een alternatief voor AD FS-implementatie als het doel alleen van Office 365 SSO is

Er is nog een alternatief voor het implementeren van AD FS helemaal als u alleen om in te schakelen-aanmelding voor Office 365. In dat geval kunt u gewoon DirSync implementeren met wachtwoord sync on-premises implementatie en het dezelfde eindresultaat bereiken met minimale implementatiecomplexiteit omdat deze methode niet voor AD FS of Azure vereist is.

De volgende tabel worden vergeleken werking van de processen aanmelden met en zonder het implementeren van AD FS.

| Office 365 eenmalige aanmelding met AD FS en DirSync | Office 365 dezelfde aanmelding met DirSync + Wachtwoordsynchronisatie |
| ------------- | ------------- |
| 1. de gebruiker zich aanmeldt bij een bedrijfsnetwerk en met Windows Server Active Directory is geverifieerd. | 1. de gebruiker zich aanmeldt bij een bedrijfsnetwerk en met Windows Server Active Directory is geverifieerd. |
| 2. de gebruiker probeert te openen van Office 365 (ik ben @contoso.com). | 2. de gebruiker probeert te openen van Office 365 (ik ben @contoso.com). |
| 3. office 365 wordt de gebruiker naar Azure AD. | 3. office 365 wordt de gebruiker naar Azure AD. |
| 4. Aangezien Azure AD de gebruiker niet verifiëren en kunt u dat er bestaat een vertrouwensrelatie met AD FS on-premises implementatie, wordt de gebruiker omgeleid naar de AD FS. | 4. azure AD niet rechtstreeks Kerberos-tickets accepteren en er bestaat geen vertrouwensrelatie zodat dit aanvraagt dat de gebruiker referenties invoeren. |
| 5. de gebruiker stuurt een Kerberos-tickets naar de AD FS-STS. | 5. de gebruiker hetzelfde wachtwoord on-premises implementatie invoert en Azure AD ze ten opzichte van de gebruikersnaam en wachtwoord die is gesynchroniseerd met DirSync is gevalideerd. |
| 6. AD FS transformeert de Kerberos-tickets als de vereiste token opmaak/vorderingen en wordt de gebruiker naar Azure AD. | 6. azure AD wordt de gebruiker omgeleid naar Office 365. |
| 7. de gebruiker geverifieerd bij Azure AD (een andere transformatie plaatsvindt). |  7. de gebruiker kan aanmelden bij Office 365 en OWA voor het gebruik van de Azure AD-token. |
| 8. azure AD wordt de gebruiker omgeleid naar Office 365. |  |
| 9. de gebruiker is stilte aangemeld bij Office 365. |  |

Klik in de Office 365 met DirSync met wachtwoord synchronisatie scenario (geen AD FS), wordt eenmalige aanmelding vervangen door 'dezelfde aanmelding' waar "dezelfde" betekent dat gebruikers opnieuw hun dezelfde referenties voor de on-premises implementatie invoeren moeten bij het openen van Office 365. Houd er rekening mee dat deze gegevens kan worden onthouden in de browser van de gebruiker om te beperken latere aanwijzingen.

### <a name="additional-food-for-thought"></a>Aanvullende eten for denken

- Als u een AD FS-proxy op een Azure virtuele machine implementeert, verbinding met de AD FS-servers nodig. Als ze on-premises implementatie zijn, is het aanbevolen dat u gebruikmaken van de site-naar-site VPN connectiviteit verstrekt door het virtuele netwerk kunnen de Web-toepassingsproxy knooppunten om te communiceren met de AD FS-servers.

- Als u een AD FS-server op een Azure virtuele machine implementeert, is connectiviteit met Windows Server Active Directory-domeincontroller, kenmerk winkels en configuratiedatabases nodig en wellicht ook een ExpressRoute of een site-naar-site VPN-verbinding tussen het Azure virtuele netwerk en de on-premises netwerk.

- Kosten zijn toegepast op alle verkeer van Azure virtuele machines (egress verkeer). Als kosten de drijvende factor zijn, is het raadzaam om te implementeren van de Web-toepassingsproxy knooppunten op Azure, blijft de AD FS-servers on-premises implementatie. Als de AD FS-servers worden geïmplementeerd op Azure virtuele machines ook, worden extra kosten gemaakt voor het verifiëren van on-premises gebruikers. Egress verkeer bijhoudt kosten ongeacht of deze de ExpressRoute of de site-naar-site VPN-verbinding is doorlopen.

- Als u besluit om te gebruiken van Azure systeemeigen server taakverdeling mogelijkheden voor betere beschikbaarheid van AD FS-servers, houd er rekening mee dat taakverdeling sondes die worden gebruikt om te bepalen de status van de virtuele machines in de cloudservice biedt. In het geval van Azure virtuele machines (in plaats van de rollen van Internet of een werknemer), moet een aangepaste test sinds de agent die op de standaard-testpakketten reageert niet aanwezig op Azure virtuele machines is worden gebruikt. Volgt de procedure waarmee u een aangepaste TCP-test kunt gebruiken, moet hiervoor alleen dat een TCP-verbinding (een TCP SYN-segment verzonden en met een TCP SYN-ACK-segment beantwoord) kunnen om VM toestand te brengen. U kunt de aangepaste test om alle TCP-poort waaraan uw virtuele machines actief luisteren te gebruiken.

> [AZURE.NOTE] Machines die moeten worden dezelfde reeks poorten rechtstreeks met Internet (zoals poort 80 en 443) kunnen niet dezelfde cloudservice delen. Het is, daarom aanbevolen dat u een speciale cloud maakt service voor uw Windows Server AD FS-servers om te voorkomen potentieel overlapt tussen Poortvereisten voor een toepassing en Windows Server AD FS.

## <a name="deployment-scenarios"></a>Scenario's voor implementatie

De volgende sectie wordt beschreven alledaagse implementatie-scenario's als u wilt de aandacht vestigen op belangrijke overwegingen die moeten worden genomen rekening. Elke scenario bevat koppelingen naar meer informatie over de beslissingen en factoren u rekening moet houden.

1. [AD DS: Een AD DS-hoogte-toepassing met geen vereiste voor bedrijfsnetwerk connectivity implementeren](#BKMK_CloudOnly)

    Bijvoorbeeld wordt een internetgerichte SharePoint-service geïmplementeerd op een Azure virtuele machine. De toepassing heeft geen afhankelijkheden-bedrijfsnetwerk resources. De toepassing Windows Server AD DS is vereist, maar niet vereist is voor het bedrijf AD DS van Windows Server.

2. [AD FS: Uitbreiden een claims-hoogte on-premises implementatie front-end-toepassing met Internet](#BKMK_CloudOnlyFed)

    Bijvoorbeeld, moet een claims-toepassing dat is geïmplementeerd on-premises implementatie is en die wordt gebruikt door zakelijke gebruikers worden toegankelijk zijn vanuit Internet. De toepassing moet rechtstreeks via Internet worden geopend door gebruik van hun eigen bedrijf identiteiten zakenpartners zowel door bestaande zakelijke gebruikers.

3. [AD DS: Windows Server AD DS-hoogte-toepassingen die is connectiviteit met het bedrijfsnetwerk bevinden vereist distribueren](#BKMK_HybridExt)

    Bijvoorbeeld wordt een LDAP-hoogte-toepassing die ondersteuning biedt voor geïntegreerde Windows-verificatie en Windows Server AD DS gebruikt als een opslagplaats voor gegevens configuratie en gebruikersprofiel geïmplementeerd op een Azure virtuele machine. Het is wenselijk voor de toepassing gebruikmaken van de bestaande zakelijke Windows Server AD DS en eenmalige aanmelding opgeven. De toepassing is niet claims-bekend.

### <a name="BKMK_CloudOnly"></a>1. AD DS: Een AD DS-hoogte-toepassing met geen vereiste voor bedrijfsnetwerk connectivity implementeren

![Alleen de cloud AD DS-implementatie](media/active-directory-deploying-ws-ad-guidelines/ADDS_cloud.png)
**afbeelding 1**

#### <a name="description"></a>Beschrijving

SharePoint wordt geïmplementeerd op een Azure virtuele machine en de toepassing heeft geen afhankelijkheden-bedrijfsnetwerk resources. De toepassing hoeft u Windows Server AD DS maar *niet* de zakelijke Windows Server AD DS vereisen. Geen Kerberos of Federatieve vertrouwensrelaties zijn vereist omdat gebruikers zelf ingerichte via de toepassing in het Windows Server AD DS-domein dat ook wordt gehost in de cloud op Azure virtuele machines.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Scenario overwegingen en hoe technologie gebieden worden toegepast op het scenario

- [Netwerktopologie](#BKMK_NetworkTopology): Maak een Azure virtuele netwerk, zonder cross-premises connectivity (ook wel bekend als site-naar-site connectivity).

- [Configuratie van de domeincontroller-implementatie](#BKMK_DeploymentConfig): een nieuwe domeincontroller implementeren in een nieuw, één domein, Windows Server Active Directory-bos. Dit moet worden geïmplementeerd samen met de Windows-DNS-server.

- [Topologie van Windows Server Active Directory-site](#BKMK_ADSiteTopology): Gebruik de standaard-Windows Server Active Directory-site (alle computers worden in de standaard-First-Site-Name).

- [IP-adressen en DNS](#BKMK_IPAddressDNS):

 - Een statische IP-adres voor de domeincontroller instellen met behulp van de cmdlet Set-AzureStaticVNetIP Azure PowerShell.
 - Installeren en configureren van Windows Server DNS op de domeincontroller (s) op Azure.
 - De eigenschappen virtueel netwerk configureren met de naam en het IP-adres van de VM waarop de rollen van de domeincontroller en DNS-server.

- [Wereldwijde catalogus](#BKMK_GC): de eerste domeincontroller in het bos moet een globale catalogus-server. Aanvullende DCs moeten ook worden geconfigureerd als GCs omdat in een enkel domein, de globale catalogus niet extra werk van de domeincontroller hoeft.

- [Plaatsing van de Windows Server AD DS-database en SYSVOL](#BKMK_PlaceDB): een gegevensschijf toevoegen aan DCs uitvoeren als Azure VMs om te bewaren van de Windows Server Active Directory-database, logboeken en SYSVOL.

- [Back-up en herstellen](#BKMK_BUR): bepalen waar u wilt opslaan van systeemstatus. Voeg desgewenst een andere gegevensschijf voor domeincontroller VM voor het opslaan van back-ups toe.

### <a name="BKMK_CloudOnlyFed"></a>2-AD FS: uitbreiden van een claims-hoogte on-premises implementatie front-end-toepassing met Internet

![Federatie met cross-premises connectivity](media/active-directory-deploying-ws-ad-guidelines/Federation_xprem.png)
**afbeelding 2**

#### <a name="description"></a>Beschrijving

Een claims-toepassing dat is geïmplementeerd on-premises implementatie is en die wordt gebruikt door zakelijke gebruikers moet worden direct toegankelijk vanaf Internet. De toepassing fungeert als een frontend web met een SQL-database waarin deze gegevens worden opgeslagen. De SQL-servers die worden gebruikt door de toepassing staan ook in het bedrijfsnetwerk. Twee Windows Server AD FS STSs en een taakverdeling zijn geïmplementeerd on-premises implementatie voor toegang tot de zakelijke gebruikers. De toepassing nu moet ook rechtstreeks via Internet worden geopend door gebruik van hun eigen bedrijf identiteiten zakenpartners zowel door bestaande zakelijke gebruikers.

In een poging om te vereenvoudigen en de installatie en configuratie behoeften van deze nieuwe vereiste, wordt besloten dat twee extra web frontends en twee Windows Server AD FS-proxyservers op Azure virtuele machines zijn geïnstalleerd. Alle vier VMs zullen worden blootgesteld rechtstreeks met Internet en connectiviteit met de on-premises netwerk met Azure Virtual Network naar website VPN mogelijkheid worden geleverd.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Scenario overwegingen en hoe IT-thema toepassen op het scenario

- [Netwerktopologie](#BKMK_NetworkTopology): Maak een Azure virtuele netwerk en [cross-premises-connectiviteit configureren](../vpn-gateway/vpn-gateway-site-to-site-create.md).

 > [AZURE.NOTE] Zorg ervoor dat de URL die is gedefinieerd binnen de certificaatsjabloon en de resulterende certificaten kunt bereikbaar zijn via de Windows Server AD FS-exemplaren actief zijn voor Azure voor elk van de Windows Server AD FS-certificaten. Dit kan cross-premises-connectiviteit met delen van de infrastructuur van uw PKI vragen. Voor voorbeeld als eindpunt van de CRL LDAP gebaseerde is en uitsluitend on-premises gehost vervolgens cross-premises connectivity moeten worden uitgevoerd. Als dit niet wenselijk, is het mogelijk dat deze moet met het gebruik van certificaten uitgegeven door een Certificeringsinstantie waarvan CRL toegankelijk via Internet is.

- [Configuratie van cloud services](#BKMK_CloudSvcConfig): Controleer of er twee cloudservices in volgorde twee taakverdeling virtuele IP-adressen. Het virtuele IP-adres van de eerste cloudservice wordt doorgestuurd naar de twee Windows Server AD FS-proxy VMs op poort 80 en 443. De Windows Server AD FS-proxy VMs geconfigureerd om te laten verwijzen naar het IP-adres van de on-premises implementatie-taakverdeling die de Windows Server AD FS-STSs de voorzijde's. Het virtuele IP-adres van de tweede cloudservice wordt doorgestuurd naar de twee VMs opnieuw uit te voeren de web-frontend op poort 80 en 443. Een aangepaste test om ervoor te zorgen dat de taakverdeling verkeer naar functioneel Windows Server AD FS-proxy en web frontend VMs alleen wordt u omgeleid zodat configureren.

- [Federatie serverconfiguratie](#BKMK_FedSrvConfig): Windows Server configureren AD FS federatieserver (STS) te genereren van beveiligingstokens voor de Windows Server Active Directory-bos gemaakt in de cloud. Stel Federatie claims provider vertrouwensrelaties met de verschillende partners die u wilt accepteren identiteiten van en gebruikmakende partij vertrouwensrelaties met de andere toepassingen die u wilt genereren tokens om te configureren.

    Windows Server AD FS-proxyservers zijn geïmplementeerd in een internetgerichte capaciteit om veiligheidsredenen in de meeste gevallen terwijl hun tegenhangers van Windows Server AD FS-federatie geïsoleerd van rechtstreekse internetconnectiviteit blijven. Ongeacht uw implementatiescenario, moet u uw cloudservice configureren met een virtuele IP-adres zijn dat, vindt u in een vrij toegankelijke IP-adres en poort die kan de werklast over de twee instanties van Windows Server AD FS-STS of de proxy exemplaren.

- [Windows Server AD FS beschikbaarheid-configuratie](#BKMK_ADFSHighAvail): het is raadzaam te implementeren van een Windows Server AD FS-farm met ten minste twee servers voor failover en taakverdeling. U mogelijk wilt u rekening moet houden met het Windows interne Database (WID) voor Windows Server AD FS-configuratiegegevens en gebruikt u de interne laden taakverdeling mogelijkheid van Azure distributie van verzoeken voor oproepen voor de servers in de farm.

Zie de [Implementatiehandleiding voor AD DS](https://technet.microsoft.com/library/cc753963)voor meer informatie.


### <a name="BKMK_HybridExt"></a>3. AD DS: Windows Server AD DS-hoogte-toepassingen die is connectiviteit met het bedrijfsnetwerk bevinden vereist distribueren

![Meerdere lokale AD DS-implementatie](media/active-directory-deploying-ws-ad-guidelines/ADDS_xprem.png)
**afbeelding 3**

#### <a name="description"></a>Beschrijving

Een LDAP-toepassing wordt geïmplementeerd op een Azure virtuele machine. Dit ondersteunt geïntegreerde Windows-verificatie en Windows Server AD DS als opslagplaats voor configuratie en gebruiker profielgegevens wordt gebruikt. Het doel is voor de toepassing gebruikmaken van de bestaande zakelijke Windows Server AD DS en eenmalige aanmelding opgeven. De toepassing is niet claims-bekend. Gebruikers moeten ook toegang hebben tot de toepassing rechtstreeks vanaf Internet. Als u wilt optimaliseren voor prestaties en kosten, wordt besloten dat twee controller die deel uitmaken van het bedrijfsdomein samen met de toepassing op Azure worden geïmplementeerd.

#### <a name="scenario-considerations-and-how-technology-areas-apply-to-the-scenario"></a>Scenario overwegingen en hoe IT-thema toepassen op het scenario

- [Netwerktopologie](#BKMK_NetworkTopology): Maak een Azure virtuele netwerk met [cross-premises connectivity](../vpn-gateway/vpn-gateway-site-to-site-create.md).

- [Installatiemethode](#BKMK_InstallMethod): implementeren replica DCs van het bedrijf Windows Server Active Directory-domein. U kunt voor een replica domeincontroller, Windows Server AD DS op de VM installeren en gebruiken (optioneel) de functie voor het installeren van Media (IFM) de hoeveelheid gegevens die moeten worden gerepliceerd naar de nieuwe domeincontroller tijdens de installatie te beperken. Zie [een replicadomeincontroller van Active Directory op Azure installeren](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)voor een zelfstudie. Zelfs als u IFM gebruikt, is het wellicht handiger om te bouwen van de virtuele domeincontroller on-premises implementatie en de hele virtuele hardeschijf (VHD) verplaatsen naar de cloud in plaats van Windows Server AD DS repliceren tijdens de installatie. Voor de veiligheid, is het aanbevolen dat u de VHD uit de on-premises netwerk, verwijderen zodra deze is gekopieerd naar Azure.

- [Topologie van Windows Server Active Directory-site](#BKMK_ADSiteTopology): een nieuwe Azure site maken in Active Directory-Sites en Services. Een object Windows Server Active Directory subnet als vertegenwoordigen het Azure virtuele netwerk en het subnet wilt toevoegen naar de site maken. Maak een nieuwe sitekoppeling met de nieuwe Azure site en de site waarin het Azure virtuele netwerk VPN eindpunt bevindt om te bepalen en Windows Server Active Directory-verkeer naar en vanuit Azure optimaliseren.

- [IP-adressen en DNS](#BKMK_IPAddressDNS):

 - Een statische IP-adres voor de domeincontroller instellen met behulp van de cmdlet Set-AzureStaticVNetIP Azure PowerShell.
 - Installeren en configureren van Windows Server DNS op de domeincontroller (s) op Azure.
 - De eigenschappen virtueel netwerk configureren met de naam en het IP-adres van de VM waarop de rollen van de domeincontroller en DNS-server.

- [Geografische distributed DCs](#BKMK_DistributedDCs): extra virtuele netwerken configureren naar wens. Als de topologie van uw Active Directory-site is vereist DCs in regio's die met verschillende gebieden van Azure, overeenkomen dan u wilt maken van Active Directory-sites dienovereenkomstig gewijzigd.

- [Alleen-lezen DCs](#BKMK_RODC): U mogelijk een alleen-lezen domeincontroller in de Azure site implementeert, afhankelijk van uw vereisten voor de uitvoering van schrijven bewerkingen ten opzichte van de domeincontroller en de compatibiliteit van toepassingen en services in de site met RODC. Zie voor meer informatie over de compatibiliteit van toepassingen, de [alleen - lezen domeincontrollers compatibiliteit handleiding](https://technet.microsoft.com/library/cc755190).

- [Wereldwijde catalogus](#BKMK_GC): GCs vereist zijn om de service aanmeldingsaanvragen in met meerdere domeinen forests. Als u een globale Catalogus op de Azure-site niet implementeren kan, wordt u kosten egress verkeer verificatieaanvragen foutwaarden of query's GCs in andere sites. Als u wilt dat verkeer minimaliseren, kunt u de universele groepslidmaatschap in cache opslaan van de Azure site in Active Directory-Sites en Services inschakelen.

    Als u een globale Catalogus implementeert, koppelingen naar sites en site koppelingen kosten zodanig configureren dat de globale Catalogus op de Azure-site is niet voorkeur krijgen als de domeincontroller van een bron van andere GCs die moeten de dezelfde gedeeltelijke domeinpartities repliceren.

- [Plaatsing van de Windows Server AD DS-database en SYSVOL](#BKMK_PlaceDB): een gegevensschijf toevoegen aan DCs uitvoeren op Azure VMs om te bewaren van de Windows Server Active Directory-database, logboeken en SYSVOL.

- [Back-up en herstellen](#BKMK_BUR): bepalen waar u wilt opslaan van systeemstatus. Voeg desgewenst een andere gegevensschijf voor domeincontroller VM voor het opslaan van back-ups toe.

## <a name="deployment-decisions-and-factors"></a>Implementatie beslissingen en factoren

In deze tabel bevat een overzicht van de gebieden van de Windows Server Active Directory-technologie dat problemen in de voorgaande scenario's en de bijbehorende beslissingen ondervindt u rekening moet houden met koppelingen naar meer details onderstaande. Bepaalde gebieden technologie inhoud mogelijk niet van toepassing op elke implementatiescenario en het is mogelijk dat bepaalde gebieden technologie belangrijker om een implementatiescenario voor dan andere gebieden technologie.

Bijvoorbeeld als u een replica domeincontroller op een virtueel netwerk implementeert en uw bos slechts één domein heeft, vervolgens naar het implementeren van een globale catalogus-server te kiezen in dat geval worden niet kritieke voor de implementatie omdat wordt er geen eventuele aanvullende replicatievereisten gemaakt. Aan de andere kant, als de bos meerdere domeinen heeft, klikt u vervolgens het besluit om te implementeren van een globale catalogus op een virtueel netwerk kan invloed hebben op bandbreedte, prestaties, verificatie, directory zoekacties, enzovoort.

| Gebied van Windows Server Active Directory-technologie | Beslissingen | Factoren |
| ---- | ---- | ---- |
| [Netwerktopologie](#BKMK_NetworkTopology) | Maakt u een virtueel netwerk? | <li>Vereisten voor toegang tot Corp resources</li> <li>Verificatie</li> <li>Accountbeheer</li> |
| [Configuratie van de domeincontroller-implementatie](#BKMK_DeploymentConfig) | <li>Een afzonderlijk bos zonder eventuele vertrouwensrelaties implementeren?</li> <li>Een nieuw bos met Federatie implementeren?</li> <li>Een nieuw bos met Windows Server Active Directory bos vertrouwen of Kerberos implementeren?</li> <li>Corp bos uitbreiden door te implementeren van een replica domeincontroller?</li> <li>Corp bos uitbreiden door het implementeren van een nieuw onderliggend domein of de structuurweergave van het domein?</li> | <li>Beveiliging</li> <li>Naleving</li> <li>Kosten</li> <li>Tolerantie en fouttolerantie</li> <li>Compatibiliteit van toepassingen</li> |
| [Topologie van Windows Server Active Directory-site](#BKMK_ADSiteTopology) | Hoe configureer u subnetten, sites en koppelingen naar sites met Azure Virtual Network te optimaliseren verkeer en minimaliseren kosten? | <li>Definities van subnet en site</li> <li>Site-link-eigenschappen en melding wijzigen</li> <li>Replicatie compressie</li> |
| [IP-adressen en DNS](#BKMK_IPAddressDNS) | Het configureren van IP-adressen en naamresolutie? | <li>Gebruik de cmdlet gebruiken de Set-AzureStaticVNetIP om het toewijzen van een statische IP-adres</li> <li>Windows Server DNS-server installeren en configureren van de eigenschappen van virtueel netwerk met de naam en het IP-adres van de VM waarop de serverrollen domeincontroller en DNS</li> |
| [Geografische distributed DCs](#BKMK_DistributedDCs) | Hoe worden gerepliceerd naar DCs op afzonderlijke virtuele netwerken? | Als de topologie van uw Active Directory-site is vereist DCs in regio's die overeenkomt met verschillende Azure gebieden, dan u wilt maken van Active Directory-sites dienovereenkomstig gewijzigd. [Virtuele netwerk configureren met virtueel netwerk Connectivity](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) repliceren tussen domeincontrollers op afzonderlijke virtuele netwerken. |
| [Alleen-lezen DCs](#BKMK_RODC) | Alleen-lezen of schrijven toekennen DCs gebruiken? | <li>HBI/PII kenmerken filteren</li> <li>Geheimen filteren</li> <li>Limiet uitgaand verkeer</li> |
| [Wereldwijde catalogus](#BKMK_GC) | Wereldwijde catalogus installeren? | <li>Zorg voor één domein bos, alle DCs GCs</li> <li>Voor met meerdere domeinen bos zijn GCs vereist voor verificatie</li> |
| [Installatiemethode](#BKMK_InstallMethod) | Het installeren van de domeincontroller in Azure wordt aangegeven? | Beide: <li>AD DS via Windows PowerShell of Dcpromo installeren</li> <li>VHD van een on-premises implementatie virtuele domeincontroller verplaatsen</li> |
| [Plaatsing van de Windows Server AD DS-database en SYSVOL](#BKMK_PlaceDB) | Waar voor de opslag van Windows Server AD DS-database, logboeken en SYSVOL? | Standaardwaarden Dcpromo.exe wijzigen. Deze kritieke Active Directory-bestanden *moeten* worden geplaatst op Azure gegevensschijven in plaats van besturingssysteem schijven die schrijven naar cache implementeren. |
| [Back-up en herstellen](#BKMK_BUR) | Hoe gegevens beschermen en herstellen? | Systeem voor back-ups maken |
| [Federatie serverconfiguratie](#BKMK_FedSrvConfig) | <li>Een nieuw bos met Federatie in de cloud implementeren?</li> <li>Lokale AD FS implementeren en weer te geven een proxy in de cloud?</li> | <li>Beveiliging</li> <li>Naleving</li> <li>Kosten</li> <li>Toegang tot toepassingen door zakenpartners</li> |
| [Configuratie van cloud services](#BKMK_CloudSvcConfig) | Een cloudservice is de eerste keer dat u een virtuele machine maakt impliciet geïmplementeerd. Moet u aanvullende cloudservices implementeren? | <li>Directe blootgesteld aan Internet vereist een VM of VMs?</li> <li> De service vereist taakverdeling?</li> |
| [Federatie serververeisten voor openbare en persoonlijke voor IP-adressen (dynamische IP versus virtuele IP)](#BKMK_FedReqVIPDIP) | <li>Moet de Windows Server AD FS-instantie worden worden bereikt rechtstreeks vanaf Internet?</li> <li>Vereist de toepassing wordt geïmplementeerd in de cloud eigen internetgerichte IP-adres en poort?</li> | Cloud-service voor elk virtuele IP-adres dat is vereist door uw implementatie maken |
| [Windows Server AD FS-beschikbaarheid configuratie](#BKMK_ADFSHighAvail) | <li>Het aantal knooppunten op mijn Windows Server AD FS-server-farm?</li> <li>Het aantal knooppunten om te implementeren in mijn Windows Server AD FS-proxy-farm?</li> | Tolerantie en fouttolerantie |

### <a name="BKMK_NetworkTopology"></a>Netwerktopologie

Om te voldoen aan de IP-adres consistentie en de DNS-vereisten voor Windows Server AD DS, is het moet u eerst een [Azure virtuele netwerk](../virtual-network/virtual-networks-overview.md) maken en uw virtuele machines aan te koppelen. Tijdens het maken, moet u bepalen of om uit te breiden desgewenst connectiviteit met uw bedrijfsnetwerk on-premises implementatie, waarmee verbinding transparant Azure virtuele machines met on-premises implementatie machines: Hiermee wordt bereikt traditionele VPN technologieën gebruiken en is vereist dat een VPN-eindpunt zichtbaar zijn op de rand van het bedrijfsnetwerk bevinden. Dat wil zeggen wordt de VPN vanaf Azure gestart met het bedrijfsnetwerk, niet vice versa.

Houd er rekening mee dat extra gesprekskosten zijn van toepassing wanneer u een virtueel netwerk naar uw on-premises netwerk buiten de kosten die worden standaard die van toepassing op elke VM uitbreidt. Specifiek, zijn er kosten voor CPU-tijd van de gateway Azure Virtual Network en voor de egress verkeer gegenereerd door elke VM die met de computers van de on-premises implementatie via het VPN communiceert. Zie [Azure op overzichtelijke prijzen](http://azure.microsoft.com/pricing/)voor meer informatie over kosten voor netwerk-verkeer is toegestaan.

### <a name="BKMK_DeploymentConfig"></a>Configuratie van de domeincontroller-implementatie

De manier waarop u de domeincontroller configureren, is afhankelijk van de vereisten voor de service die u wilt uitvoeren op Azure. U kunt een nieuw bos, geïsoleerd uit uw eigen bedrijf bos, voor het testen van een bewijs-van-concept, een nieuwe toepassing of enkele andere projectgerelateerde korte termijn die moeten worden adreslijstservices, maar niet specifieke toegang tot interne bedrijfsresources bijvoorbeeld mogelijk implementeren.

Een voordeel een geïsoleerd bos die domeincontroller niet gerepliceerd met on-premises implementatie DCs, waardoor er minder uitgaand netwerkverkeer gegenereerd door het systeem zelf, rechtstreeks kostenverlaging. Zie [Azure op overzichtelijke prijzen](http://azure.microsoft.com/pricing/)voor meer informatie over kosten voor netwerk-verkeer is toegestaan.

Stel een ander voorbeeld u privacy vereisten voor een service, maar de service is afhankelijk van toegang tot uw interne Windows Server Active Directory. Als u bent gemachtigd voor gegevens voor de service in de cloud, kunt u een nieuw onderliggend domein voor uw interne bos op Azure implementeren. In dit geval kunt u een domeincontroller voor het nieuwe onderliggende domein (zonder de globale catalogus om u te helpen verbeteren privacy) implementeren. Dit scenario, samen met een replica domeincontroller implementatie, is een virtueel netwerk voor verbinding met uw on-premises implementatie DCs vereist.

Als u een nieuw bos hebt gemaakt, kiest u of [Active Directory-vertrouwensrelaties](https://technet.microsoft.com/library/cc771397) of [Federatie vertrouwensrelaties](https://technet.microsoft.com/library/dd807036)wilt gebruiken. Saldo vanaf de vereisten die wordt bepaald door de compatibiliteit, beveiliging, naleving, kosten en tolerantie. Om te profiteren van [Selectieve verificatie](https://technet.microsoft.com/library/cc755844) kunt u bijvoorbeeld om een nieuw bos op Azure implementeren en een Windows Server Active Directory-vertrouwensrelatie tussen de on-premises implementatie bos en de cloud-bos te maken. Als de toepassing op claims-functionaliteit is, echter u mogelijk federation-vertrouwensrelaties in plaats van Active Directory bos vertrouwensrelaties implementeren. Een andere factor zijn de kosten meer gegevens repliceren te verlengen van uw on-premises implementatie Windows Server Active Directory in de cloud of meer uitgaand verkeer grond verificatie en het laden van een query te genereren.

Vereisten voor beschikbaarheid en fouttolerantie zijn ook van invloed op uw keuze. Bijvoorbeeld als de koppeling wordt onderbroken, vertrouwen toepassingen die gebruikmaken van een Kerberos-vertrouwensrelatie of een Federatie zijn alle waarschijnlijk helemaal niet goed tenzij u voldoende infrastructuur op Azure hebt geïmplementeerd. Alternatieve implementatieconfiguraties zoals replica DCs (schrijfbare of RODC) vergroot de kans dat bestand tegen koppeling bijvoorbeeld.

### <a name="BKMK_ADSiteTopology"></a>Topologie van Windows Server Active Directory-site

U moet correct definiëren sites en koppelingen naar sites om te kunnen verkeer optimaliseren en kosten te minimaliseren. Sites, -koppelingen naar sites en subnetten van invloed zijn op de topologie herhaling tussen DCs en de stroom van verificatie-verkeer is toegestaan. Houd rekening met de volgende verkeer kosten en implementeren en configureren van DCs volgens de vereisten van uw implementatiescenario:

- Er zijn nominale kosten per uur voor de gateway zelf:

 - Dit kan worden gestart en gestopt wens naar

 - Als gestopt, Azure VMs worden geïsoleerd van het bedrijfsnetwerk bevinden

- Binnenkomende verkeer is gratis

- Uitgaand verkeer is BTW geheven, op basis van [Azure op overzichtelijke prijzen](http://azure.microsoft.com/pricing/). U kunt de eigenschappen van de site-koppeling tussen de on-premises implementatie-sites en de cloud-sites als volgt optimaliseren:

 - Als u meerdere virtuele netwerken gebruikt, configureert u de site-koppelingen en de kosten per correct als u wilt voorkomen dat Windows Server AD DS prioriteit van de Azure-site op een dat dezelfde machtigingsniveaus service gratis kan leveren. U kunt ook uitschakelen van de brug alle-(BASL)-koppelingsoptie site (de knop is standaard ingeschakeld). Dit zorgt ervoor dat alleen direct-verbonden sites met elkaar worden gerepliceerd. DCs in transitief verbonden sites zijn niet meer kunnen repliceren rechtstreeks met elkaar, maar moeten vermenigvuldigen via een gemeenschappelijke site of sites. Als de tussenliggende sites niet meer beschikbaar om een bepaalde reden replicatie tussen DCs in transitief verbonden sites wordt niet uitgevoerd zelfs als verbinding tussen de sites beschikbaar is. Ten slotte, waarbij de secties Overview overgankelijke herhaling gedrag wenselijk blijven, site maken koppelingsbruggen die de juiste site-koppelingen en sites, zoals on-premises bedrijfsnetwerk sites bevatten.

 - [Kosten van site configureren](https://technet.microsoft.com/library/cc794882) op de juiste manier om te voorkomen dat dit onbedoeld kan worden verkeer. Bijvoorbeeld als **Probeert dichtstbijzijnde Site** instelling is ingeschakeld, controleert u of de sites virtueel netwerk niet het eerstvolgende doordat de kosten die verbonden zijn van het object koppeling naar site die de Azure site terug naar het bedrijfsnetwerk bevinden verbindt.

 - Site koppeling [intervallen](https://technet.microsoft.com/library/cc794878) en [schema's](https://technet.microsoft.com/library/cc816906) op basis van de consistentie vereisten en tarief weer dat object wijzigingen configureren. Replicatie planning uitlijnen met latentie tolerantie. DCs repliceren alleen de laatste status van een waarde, zodat kosten verlagen replicatie-interval kunt opslaan als er een voldoende object snelheid wijzigen.

- Wanneer kosten worden geminimaliseerd een prioriteit is, zorg er dan replicatie is gepland en het bericht niet is ingeschakeld. Dit is de standaardconfiguratie wanneer repliceren tussen sites. Dit is niet belangrijk als u een alleen-lezen domeincontroller op een virtueel netwerk implementeert omdat de alleen-lezen domeincontroller uitgaande wijzigingen niet gerepliceerd. Maar als u een schrijfbare domeincontroller implementeert, moet u ervoor zorgen dat de sitekoppeling niet is geconfigureerd voor het repliceren updates met onnodige frequentie. Als u een globale catalogus-server (globale Catalogus) implementeert, zorg dat elke andere site waarin een globale Catalogus wordt overgenomen door domeinpartities van een bron domeincontroller in een site die is verbonden met een koppeling naar site of koppelingen naar sites met lagere kosten dan de globale Catalogus in de Azure-site.


- Het is mogelijk verder nog steeds netwerkverkeer die zijn gegenereerd door replicatie tussen sites door te wijzigen van de algoritme van de compressie herhaling te beperken. De compressiealgoritme van de wordt bepaald door de algoritme van REG_DWORD register vermelding HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Replicator compressie. De standaardwaarde is 3, die betrekking heeft op de algoritme van de Xpress comprimeren. Kunt u de waarde 2, waardoor de algoritme van de MSZip is gewijzigd. Hiermee vergroot u de compressie in de meeste gevallen, maar dit gebeurt kosten van CPU-gebruik. Zie [hoe Active Directory herhaling topologie werkt](https://technet.microsoft.com/library/cc755994)voor meer informatie.

### <a name="BKMK_IPAddressDNS"></a>IP-adressen en DNS

Azure virtuele machines worden "adressen DHCP lease" al dan niet standaard toegewezen. Omdat Azure virtuele netwerk dynamische adressen met een virtuele machine de levensduur van de virtuele machine aanhouden, zijn de vereisten van Windows Server AD DS is voldaan.

Hierdoor wanneer u een dynamisch adres op Azure gebruikt, bent u in feite een statisch IP-adres gebruiken omdat deze geschikt voor de periode van de lease is en de periode van de lease gelijk aan de levensduur van de cloudservice is.

Het dynamische adres is echter opgeheven als de VM afgesloten is. Als u wilt voorkomen dat het IP-adres dat wordt opgeheven, kunt u [Set-AzureStaticVNetIP als een statische IP-adres wilt toewijzen](http://social.technet.microsoft.com/wiki/contents/articles/23447.how-to-assign-a-private-static-ip-to-an-azure-vm.aspx).

Voor naamresolutie, implementeren uw eigen (of gebruikmaken van uw bestaande) DNS-serverinfrastructuur; Azure-voorwaarde DNS voldoet niet aan de behoeften van de naam van de geavanceerde resolutie van Windows Server AD DS. Bijvoorbeeld deze geen ondersteuning bieden voor dynamische SRV-records, enzovoort. Naamresolutie is een onderdeel van de kritieke configuratie voor DCs en clients domein behoren. DCs moeten staat resource records geregistreerd en het oplossen van andere domeincontroller resource records zijn.
Voor foutenstructuuranalyse tolerantie en prestaties redenen is dit het Windows-DNS-Server-service installeren op de DCs waarop Azure optimale. Vervolgens moet u de netwerkeigenschappen Azure virtuele configureren met de naam en het IP-adres van de DNS-server. Wanneer andere VMs op het virtuele netwerk begint, worden hun DNS-resolvercache clientinstellingen geconfigureerd met DNS-server als onderdeel van de toewijzing van de dynamische IP-adressen.

> [AZURE.NOTE] U kunt niet deelnemen aan lokale computers naar een Active Directory van Windows Server AD DS-domein dat wordt gehost op Azure rechtstreeks via Internet. De Poortvereisten voor Active Directory en de domein-join-bewerking niet handig rechtstreeks weergeven worden de benodigde poorten en in feite een hele domeincontroller met Internet.

VMs registreren hun naam van de DNS-automatisch bij het opstarten of als er een naamwijziging.

Zie [een nieuwe Active Directory-bos op Microsoft Azure installeren](active-directory-new-forest-virtual-machine.md)voor meer informatie over dit voorbeeld en een ander voorbeeld om te zien hoe het inrichten van de eerste VM en installeren van AD DS op is geïnstalleerd. Zie voor meer informatie over het gebruik van Windows PowerShell [Installeren Azure PowerShell](../powershell-install-configure.md) en [Cmdlets voor gebruikersbeheer van Azure](https://msdn.microsoft.com/library/azure/jj152841).

### <a name="BKMK_DistributedDCs"></a>Geografische distributed DCs

Azure biedt voordelen wanneer meerdere DCs in verschillende virtuele netwerken hostingprovider:

- Meerdere site fouttolerantie

- Fysieke nabijheid filialen (lagere latentie)

Zie voor informatie over het configureren van rechtstreekse communicatie tussen virtuele netwerken, [virtuele netwerk virtuele netwerkconnectiviteit configureren](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

### <a name="BKMK_RODC"></a>Alleen-lezen DCs

U moet kiezen of u wilt alleen-lezen of schrijven toekennen DCs implementeren. U mogelijk worden blootgesteld aan RODC implementeren, omdat u geen fysieke controle over deze, maar RODC zijn ontworpen om te worden geïmplementeerd op locaties waar de fysieke beveiliging risico, zoals tak kantoren is.

Azure bevat niet de fysieke veiligheidsrisico van een kantoor presenteren, maar RODC mogelijk nog steeds bewijzen moeten efficiënte, omdat de functies die ze bieden zijn zeer geschikt voor deze omgevingen zij vanwege de sterk afwijkt. Bijvoorbeeld RODC hebben geen uitgaande replicatie en mogen selectief vullen geheimen (wachtwoorden). Klik op het nadeel, het gebrek aan deze geheimen kan het nodig zijn op aanvraag uitgaand verkeer om te verifiëren of als een gebruiker of wordt geverifieerd. Maar geheimen kunnen worden selectief vooraf ingevuld en opgeslagen in de cache.

RODC bieden een extra voordeel in en rond HBI en PII bezwaren omdat u de kenmerken die met gevoelige gegevens naar de alleen-lezen domeincontroller gefilterd kenmerk is ingesteld (VA) kunt toevoegen. De VA is een aanpasbare verzameling kenmerken die niet worden gerepliceerd naar RODC. Als u zijn niet toegestaan of niet wilt opslaan PII of HBI op Azure, kunt u de VA als beveiliging. Zie voor meer informatie, [gefilterde kenmerk alleen-lezen domeincontroller instellen [(https://technet.microsoft.com/library/cc753459)].

Zorg ervoor dat toepassingen zijn compatibel met RODC die u wilt gebruiken. Veel Windows Server Active Directory-toepassingen werken ook met RODC, maar sommige toepassingen kunnen uitvoeren inefficiënt of mislukt als ze geen toegang tot een schrijfbare domeincontroller. Zie de [handleiding voor compatibiliteit DCs alleen-lezen](https://technet.microsoft.com/library/cc755190)voor meer informatie.

### <a name="BKMK_GC"></a>Wereldwijde catalogus

U moet kiezen of u voor het installeren van een globale catalogus ° (c). In een enkel domein, moet u alle DCs configureren als globale catalogus-servers. Kosten wordt niet verhoogd omdat er geen aanvullende replicatie-verkeer zullen zijn.

In een met meerdere domeinen bos zijn GCs nodig om uit te vouwen Universal groepslidmaatschap tijdens de verificatie. Als u een globale Catalogus niet implementeren kan, genereert werkbelasting in het virtuele netwerk die worden geverifieerd bij een domeincontroller op Azure indirect uitgaande verificatie-verkeer is toegestaan om de query GCs on-premises implementatie tijdens elke verificatiepoging.

De kosten die is gekoppeld aan GCs zijn minder overzichtelijk omdat ze elk domein (in-onderdeel) hosten. Als de werklast een internetgerichte-service host en gebruikers ten opzichte van Windows Server AD DS worden geverifieerd, is het mogelijk dat de kosten volledig onverwachte. Om te beperken ° c-query's buiten de cloud-site tijdens de verificatie, kunt u [inschakelen universele groepslidmaatschap in cache opslaan](https://technet.microsoft.com/library/cc816928).

### <a name="BKMK_InstallMethod"></a>Installatiemethode

U moet kiezen hoe u de DCs installeert op het virtuele netwerk:

- Nieuwe DCs verhogen Zie [een nieuwe Active Directory-bos in een netwerk met een Azure virtuele installeren](active-directory-new-forest-virtual-machine.md)voor meer informatie.

- Verplaats de VHD van een on-premises implementatie virtuele domeincontroller naar de cloud. In dit geval moet u ervoor zorgen dat de on-premises virtuele domeincontroller is 'verplaatst,' niet "gekopieerde" of "gekloonde."

Gebruik alleen Azure virtuele machines voor (in plaats van Azure "web" of "werknemer" rol VMs). Ze duurzaam zijn en levensduur van staat voor een domeincontroller is vereist. Azure virtuele machines zijn ontworpen voor werkbelastingen zoals DCs.

Gebruik geen SYSPREP implementeren of DCs klonen. De mogelijkheid om te klonen DCs is alleen beschikbaar begin in Windows Server 2012. De klonen functie vereist ondersteuning voor VMGenerationID in de onderliggende hypervisor. Hyper-V in Windows Server 2012 en Azure virtuele netwerken beide ondersteuning voor VMGenerationID, zoals leveranciers van derden virtualization doen.

### <a name="BKMK_PlaceDB"></a>Plaatsing van de Windows Server AD DS-database en SYSVOL

Selecteer waar u de Windows Server AD DS-database, logboeken en SYSVOL. Ze moeten worden geïmplementeerd op Azure-gegevens schijven.

> [AZURE.NOTE] Azure gegevensschijven zijn beperkt tot 1 TB.

Gegevens schijfstations doen niet cache schrijft al dan niet standaard. De schijfstations die zijn gekoppeld aan een VM gebruik schrijven van caching van gegevens. Schrijven van caching maken die ervoor dat het schrijven wil duurzame Azure opslag voordat de transactie voltooid vanuit het perspectief van het besturingssysteem van de VM is. Levensduur, kosten van iets langzamer afspelen schrijft krijgen.

Dit is belangrijk is voor Windows Server AD DS omdat hypothesen die zijn aangebracht door de domeincontroller schijf caching van schrijven-behind ongeldig. Windows Server AD DS probeert te schrijven caching uitschakelt, maar het is snel aan de schijf IO systeem te voldoen aan deze. Bij het schrijven naar cache uitschakelen mogelijk, onder bepaalde omstandigheden USN-hersteloptie lingering objecten en andere problemen waardoor introduceren.

Ga als volgt te werk als aangeraden voor virtual:

- Stel de instelling Host Cache voorkeur op de gegevensschijf Azure-voor geen. Hiermee voorkomt u problemen met het schrijven naar cache voor AD DS-bewerkingen.

- De database, logboeken en SYSVOL opslaan op een van beide dezelfde gegevensschijf of gegevens apart schijven. Dit is meestal een andere schijf dan de schijf waarop het besturingssysteem zelf. De belangrijkste takeaway is dat de Windows Server AD DS-database en SYSVOL niet moeten worden opgeslagen op een schijftype Azure-besturingssysteem. Het installatieproces van AD DS installeert standaard deze onderdelen in % hoofdmap % map, die niet voor Azure geschikt is.

### <a name="BKMK_BUR"></a>Back-up en herstellen

Rekening moet houden wat is wordt niet ondersteund voor back-up en herstellen van een domeincontroller in het algemeen en meer, met name uitgevoerd in een VM. Zie [back-up en herstellen overwegingen voor gevirtualiseerde](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv#backup_and_restore_considerations_for_virtualized_domain_controllers).

Systeem back-ups via alleen back-software die is specifiek op de hoogte van de back-vereisten voor Windows Server AD DS, zoals Windows Server back-up maken.

Kopieer of VHD-bestanden van DCs klonen in plaats van de uitvoering van regelmatige back-ups niet. Een herstellen moet ooit vereist, zodat gebruiken gekloonde of gekopieerde VHD's zonder Windows Server 2012 en een ondersteunde hypervisor USN bellen tot leidt doen.

### <a name="BKMK_FedSrvConfig"></a>Federatie serverconfiguratie

De configuratie van Windows Server AD FS-federatieservers (STSs) is gedeeltelijk afhankelijk of de toepassingen die u wilt implementeren op Azure nodig hebt voor toegang tot bronnen op uw on-premises netwerk.

Als de toepassingen overeenkomen met de volgende criteria voldoet, kunt u de toepassingen in moeten worden geïsoleerd implementeren uit uw on-premises netwerk.

- Ze accepteren SAML beveiligingstokens

- Ze zijn exposable met Internet

- Ze kunnen geen toegang tot lokale bronnen

In dit geval Windows Server AD FS STSs als volgt configureren:

1. Een geïsoleerd één domein bos configureren op Azure.

2. Federatieve toegang bieden tot de bos door het configureren van een Windows Server AD FS-federatie server-farm.

3. Windows Server AD FS (federation-serverfarm en federation-proxy serverfarm) in de on-premises implementatie bos configureren.

4. Een Federatie vertrouwensrelatie definiëren tussen de on-premises en Azure exemplaren van Windows Server AD FS.

Aan de andere kant, als de toepassingen toegang tot lokale bronnen, kunt u configureren Windows Server AD FS Zorg dat de toepassing op Azure als volgt:

1. Connectiviteit tussen on-premises implementatie-netwerken te gebruiken en Azure configureren.

2. Een Windows Server AD FS-federatie server-farm configureren in de on-premises implementatie bos.

3. Een Windows Server AD FS-federatie server proxy-farm configureren op Azure.

Deze configuratie heeft het voordeel van het verkleinen van het risico van lokale bronnen, vergelijkbaar met het configureren van Windows Server AD FS met toepassingen in een netwerk met een omtrek.

Opmerking in beide scenario's, kunt u vertrouwensrelaties relaties definiëren met meer identiteitsprovider, als business-to-business samenwerking is vereist.

### <a name="BKMK_CloudSvcConfig"></a>Configuratie van cloud services

Cloudservices zijn nodig als u wilt laten zien een VM rechtstreeks met Internet of om weer te geven van een toepassing internetgerichte verdeeld. Dit is mogelijk omdat elke cloudservice één configureerbare virtuele IP-adres biedt.

### <a name="BKMK_FedReqVIPDIP"></a>Federatie serververeisten voor openbare en persoonlijke voor IP-adressen (dynamische IP versus virtuele IP)

Elke Azure virtuele machine ontvangt een dynamisch IP-adres. Een dynamisch IP-adres is een toegankelijke alleen binnen Azure privéadres. In de meeste gevallen echter wordt deze nodig zijn voor het configureren van een virtuele IP-adres van uw Windows Server AD FS-implementaties. Het virtuele IP is nodig om weer te geven van Windows Server AD FS-eindpunten met Internet en worden gebruikt door federatieve partners en klanten voor verificatie en dagelijkse beheertaken. Een virtueel IP-adres is een eigenschap van een cloudservice die een of meer Azure virtuele machines bevat. Als de claims-toepassing geïmplementeerd op Azure en Windows Server AD FS internetgerichte zowel delen algemene poorten, elk moeten worden een virtueel IP-adres van een eigen en daarom worden om één cloudservice voor de toepassing en een tweede voor Windows Server AD FS te maken.

Zie [termen en definities](#BKMK_Glossary)voor definities van de voorwaarden virtuele IP-adres en dynamische IP-adres.

### <a name="BKMK_ADFSHighAvail"></a>Windows Server AD FS-beschikbaarheid configuratie

Het is mogelijk te implementeren zelfstandige versie van Windows Server AD FS-federatie services, wordt het wordt aanbevolen om te implementeren van een farm met ten minste twee knooppunten voor AD FS STS zowel proxy's voor productieomgevingen.

Zie [AD FS 2.0 implementatie topologie overwegingen](https://technet.microsoft.com/library/gg982489) in de [AD FS 2.0 ontwerp Guide](https://technet.microsoft.com/library/dd807036) te bepalen welke configuratieopties implementatie beste aansluiten op uw specifieke wensen.

> [AZURE.NOTE] Om te krijgen van taakverdeling voor Windows Server AD FS eindpunten op Azure, alle leden van de Windows Server AD FS-farm in de dezelfde cloudservice configureren en gebruiken van de laden taakverdeling videomogelijkheden van Azure voor HTTP (standaard 80) en HTTPS-poorten (standaard 443). Zie [Azure taakverdeling test](https://msdn.microsoft.com/library/azure/jj151530)voor meer informatie.
Windows Server laden verdelen NLB (Network) wordt niet ondersteund op Azure.
