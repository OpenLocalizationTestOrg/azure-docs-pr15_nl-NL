<properties
   pageTitle="Implementatie van Azure Active Directory | Microsoft Azure"
   description="Het implementeren van een beveiligde hybride netwerkarchitectuur met Azure Active Directory."
   services="guidance,virtual-network,active-directory"
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
   ms.date="09/28/2016"
   ms.author="telmos"/>

# <a name="implementing-azure-active-directory"></a>Implementatie van Azure Active Directory

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

In dit artikel wordt beschreven aanbevolen procedures voor het lokale AD (Active Directory) domeinen en forests integreren met Azure Active Directory voor cloud gebaseerde identiteitsverificatie.

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen: [Resourcemanager] [ resource-manager-overview] en klassieke. Deze verwijzing architectuur gebruikt resourcemanager, waarin u wordt aangeraden voor nieuwe implementaties.

U kunt directory en -identiteit services, zoals die door AD-Directory Services (AD DS) om identiteiten te verifiëren. Deze identiteiten kunnen deel uitmaakt van gebruikers, computers, toepassingen en andere bronnen die deel uitmaken van de rand van een waardepapier. U kunt directory hosten kan en identiteit services on-premises implementatie, maar in een hybride scenario waar elementen van een toepassing in Azure bevinden zich efficiënter om uit te breiden deze functionaliteit in de cloud. Deze methode verkleint de latentie die worden veroorzaakt door te sturen verificatie en voor lokale autorisatieaanvragen vanuit de cloud weer terug naar de map en -identiteit services worden uitgevoerd on-premises implementatie.

Azure biedt twee oplossingen voor de uitvoering van directory en -identiteit services in de cloud:

1. U kunt [Azure Active Directory (AAD)] [ azure-active-directory] maken van een AD-domein in de cloud en dit koppelen aan een on-premises AD-domein. AAD kunt u eenmalige aanmelding (SSO) configureren voor gebruikers die werken met toepassingen via de cloud. AAD gebruikmaakt van [Azure AD Connect] [ azure-ad-connect] uitgevoerd on-premises implementatie objecten en identiteiten repliceren gehouden in de on-premises implementatie-bibliotheek in de cloud.

    U kunt ook AAD implementeren zonder een on-premises adreslijst. In dit geval fungeert AAD als de primaire bron van alle informatie van de identiteit in plaats van met gegevens uit een on-premises adreslijst gerepliceerd.

    Opmerking: de map in de cloud is **niet** een uitbreiding van een on-premises adreslijst. In plaats daarvan is een kopie met dezelfde objecten en identiteiten. Wijzigingen in deze items on-premises implementatie worden gekopieerd naar de cloud, maar wijzigingen aanbrengen in de cloud **zijn niet** terug naar de on-premises domein worden gerepliceerd.

    >[AZURE.NOTE] De Azure Active Directory Premium-versies ondersteuning terugschrijven van wachtwoorden van gebruikers, zodat uw on-premises gebruikers zelf opnieuw instellen van wachtwoorden uitvoeren in de cloud. Dit is de enige informatie die AAD terug naar de on-premises adreslijst synchroniseert. Zie voor meer informatie over de verschillende versies van AAD en hun eigenschappen, [Azure Active Directory-edities][aad-editions].

2. U kunt een VM AD DS uitvoert als domeincontroller in Azure wordt aangegeven, de infrastructuur van uw bestaande AD (waaronder AD DS en AD FS) uitbreiden implementeren van uw on-premises implementatie-datacenter. Deze methode werkt komt vaker voor scenario's waarin de on-premises netwerk en Azure virtuele netwerken ingevoegd als formulier met een VPN-en/of ExpressRoute verbinding. Deze oplossing ondersteunt ook bidirectionele replicatie zodat u wijzigingen aanbrengen in de cloud en on-premises, waar u het meest geschikt is.

Deze architectuur is gericht op oplossing 1. Zie voor meer informatie over de tweede oplossing, [Active Directory uitbreiden naar Azure][guidance-adds].

Veelvoorkomend gebruik dozen voor deze architectuur opnemen:

- Leveren eenmalige aanmelding voor eindgebruikers SaaS en andere toepassingen uitgevoerd in de cloud, met dezelfde referenties die zij opgeven voor on-premises implementatie-toepassingen.

- Publicatie van on-premises implementatie webtoepassingen tot en met de cloud voor toegang tot externe gebruikers die deel uitmaakt van uw organisatie.

- Selfservice-mogelijkheden voor eindgebruikers, zoals hun wachtwoorden opnieuw instellen en groepsbeheer overdragen implementeren. 

    >[AZURE.NOTE] Deze mogelijkheden kunnen worden beheerd door een beheerder via Groepsbeleid.

- Situaties waarin de on-premises netwerk en Azure virtuele netwerk hostingprovider cloud-toepassingen worden niet rechtstreeks zijn gekoppeld via een VPN-tunnel of ExpressRoute circuitlijnen.

Houd er rekening mee dat AAD geen volledige functionaliteit van AD biedt. Bijvoorbeeld omgaat AAD momenteel alleen met gebruikersverificatie in plaats van computerverificatie. Sommige toepassingen en services, zoals SQL Server, kunnen vragen om verificatie in dat geval deze oplossing niet nodig is. Daarnaast kunnen AAD inhoud mogelijk niet geschikt voor systemen waar onderdelen over de grens op-premises/cloud migreren kunnen in plaats van alleen die wordt gekopieerd.

> [AZURE.NOTE] Bekijk voor een gedetailleerde uitleg van de werking van Azure Active Directory, [Microsoft Active Directory uitleg][aad-explained].

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram wordt de belangrijke onderdelen in deze architectuur gemarkeerd. Lees voor meer informatie over de elementen van de werklast in Azure wordt aangegeven, [VMs uitgevoerd voor een architectuur N lagen op Azure][implementing-a-multi-tier-architecture-on-Azure]:

> [AZURE.NOTE] Voor eenvoudig, dit diagram alleen ziet u de verbindingen rechtstreeks zijn gerelateerd aan AAD en wordt niet web browser verzoek omleidingen weer of andere gerelateerde verkeer die kan optreden als onderdeel van het proces van de Federatie verificatie en -identiteit protocol. Bijvoorbeeld: een gebruiker (on-premises implementatie of extern) van meestal toegang heeft tot een web-app via een browser en de web-app kan de browser om te verifiëren van het verzoek tot en met AAD transparant leiden. Als geverifieerd, kan het verzoek worden doorgegeven naar de WebApp gebruiken samen met de juiste identiteit-informatie.

[! [0]][0]

- **Azure Active Directory-tenant**. Dit is een exemplaar van AAD gemaakt door uw organisatie. Het fungeert als een eenvoudige adreslijstservice voor cloud-toepassingen (deze bevat objecten die zijn gekopieerd vanuit de on-premises AD), en identiteit services bevat.

- **Web laag subnet**. Dit subnet bevat VMs die een cloudgebaseerde maatwerktoepassing ontwikkeld door uw organisatie en voor welke AAD als een identiteit makelaar fungeren kan implementeren.

- **On-premises AD DS-server**. Uw organisatie waarschijnlijk heeft al een bestaande adreslijstservice zoals AD DS. U kunt veel van de items synchroniseren in uw adreslijst AD DS (zoals gebruiker en informatie over het) met AAD, om in te schakelen AAD naar deze gegevens gebruiken om de identiteit verifiëren.

- **Azure AD Connect synchronisatieserver**. Dit is een lokale computer die wordt uitgevoerd van de Azure AD Connect-synchronisatie-service. U kunt deze service installeren met behulp van de Azure AD Connect-software. De synchronisatie-service van Azure AD Connect synchroniseert informatie (gebruikers, groepen, contactpersonen, enzovoort) in de on-premises implementatie AD naar AAD in de cloud. Bijvoorbeeld: u kunt inrichten of groepen en gebruikers on-premises implementatie deprovision en deze wijzigingen zijn doorgegeven aan AAD. De synchronisatie-service van Azure AD Connect geeft informatie aan de tenant AAD.

    >[AZURE.NOTE] Vanwege de beveiliging van de gebruiker wachtwoorden niet rechtstreeks in AAD opgeslagen. In plaats daarvan bevat AAD een hash elke wachtwoord. Dit is voldoende om te controleren of wachtwoord van een gebruiker. Als een gebruiker een wachtwoord opnieuw instellen moet, moet dit uitgevoerde on-premises en de nieuwe hash verzonden naar AAD. AAD Premium-edities bevatten functies waarmee u van deze taak automatiseren kunnen zodat gebruikers kunnen hun eigen wachtwoorden opnieuw instellen.

## <a name="recommendations"></a>Aanbevelingen

In deze sectie bevat een overzicht van aanbevelingen voor AAD als volgt implementeren.

- AD Connect
- Beveiliging

### <a name="azure-ad-connect-sync-service-recommendations"></a>Azure AD Connect synchronisatie service aanbevelingen

Synchronisatie heeft betrekking op ervoor te zorgen dat gebruikers identiteit informatie in de cloud consistent zijn met die gehouden on-premises is. Het doel van de synchronisatie-service van Azure AD Connect is om deze consistentie te. De volgende punten samenvatten aanbevelingen voor de uitvoering van de synchronisatie-service van Azure AD Connect:

- Voordat u implementeert Azure AD Connect synchroniseren, moet u de vereisten van de synchronisatie van uw organisatie bepalen (wat u kunt synchroniseren, uit welke domeinen, en hoe vaak. Het artikel [bepalen directory-synchronisatie vereisten] [ aad-sync-requirements] beschrijving van de punten waarmee u rekening moet houden.

- U kunt de Azure AD Connect-synchronisatie-service volgens een VM uitvoeren of een computer die worden gehost on-premises implementatie. Afhankelijk van de volatiliteit van de informatie in uw adreslijst AD, nadat de eerste synchronisatie met AAD is uitgevoerd de werklast van de synchronisatie-service van Azure AD Connect is waarschijnlijk niet hoog. Met een VM, kunt u eenvoudiger schalen dat de server (monitor de activiteit zoals is beschreven in de sectie [Overwegingen voor controle](#monitoring-considerations) om te bepalen of dit nodig is).

- Als u meerdere domeinen van de on-premises implementatie in een bos hebt, kunt u opslaan en synchroniseren van gegevens voor de hele bos een enkel AAD-tenant (dit is de aanbevolen werkwijze). Filteren van gegevens voor identiteiten die in meer dan één domein, optreden zodat elke identiteit wordt slechts eenmaal in AAD in plaats van te dupliceren weergegeven als dit tot inconsistenties leiden kan wanneer gegevens worden gesynchroniseerd. Deze methode is vereist implementatie van meerdere Azure AD Connect-synchronisatie-servers. Zie het scenario meerdere AAD in de sectie [topologie overwegingen](#topology-considerations) voor meer informatie. 

- Gebruik filteren om de gegevens die zijn opgeslagen in AAD alleen die aan die nodig zijn beperkt. Bijvoorbeeld: uw organisatie mogelijk niet wilt opslaan informatie over niet-actieve of niet-persoonlijke accounts in AAD. Filteren op basis van een groep, op basis van een domein, op basis van een organisatie-eenheid of kenmerk gebaseerde kan zijn en u kunt filters om te genereren van regels voor meer complexe combineren. U kunt bijvoorbeeld selecteren voor het synchroniseren van alleen objecten in een domein gehouden met een bepaalde waarde in een geselecteerd kenmerk. Zie voor gedetailleerde informatie [Azure AD Connect-synchronisatie: configureren filteren][aad-filtering].

- Als u wilt implementeren beschikbaarheid voor de synchronisatie-service van AD Connect, een secundaire tijdelijk opslaan server wordt uitgevoerd. Zie de sectie [topologie overwegingen](#topology-considerations) voor meer informatie.

### <a name="security-recommendations"></a>Aanbevelingen voor beveiliging

De volgende items Samenvatting de primaire beveiligingsaanbevelingen voor de uitvoering van AAD, afhankelijk van de vereisten van uw organisatie:

- Het beheren van wachtwoorden van gebruikers.

    Als u een premium-editie van AAD gebruikt, kunt u wachtwoord terugschrijven uit AAD naar uw on-premises adreslijst inschakelen door een aangepaste installatie van Azure AD Connect te voeren:

    [! [9]][9]

    Deze functie kan gebruikers hun eigen wachtwoorden van binnen de portal van Azure opnieuw, maar alleen nadat u hebt bekeken wachtwoordbeleid voor beveiliging van uw organisatie moet worden ingeschakeld. Zoals u kunt beperken welke gebruikers hun wachtwoord kunnen wijzigen, en kunt u het wachtwoord management-ervaring aanpassen. Zie voor meer informatie [Wachtwoordbeheer aan te passen aan de behoeften van uw organisatie][aad-password-management].

- Beveiliging voor on-premises implementatie-toepassingen die toegankelijk is extern onderhouden.

    Gebruik de Azure AD-toepassingsproxy gecontroleerde toegang geven tot webtoepassingen on-premises implementatie van externe gebruikers tot en met AAD. Deze methode beveiligt uw toepassingen door niet weer te geven in rechtstreeks Internet. Alleen gebruikers met geldige referenties in het telefoonboek van uw Azure zijn kunnen bereiken van uw toepassingen. Zie voor meer informatie het artikel [Toepassingsproxy inschakelen in de portal van Azure][aad-application-proxy].

- Actief AAD cmdlets voor controle op tekenen van verdachte activiteiten.

    Houd rekening met AAD Premium P2-editie, waaronder AAD identiteitsbeveiliging. Identiteitsbeveiliging maakt gebruik van geavanceerde machine learning algoritmen en heuristiek detecteren afwijkingen en risico gebeurtenissen die aangeven mogelijk dat een identiteit meer veilig is. Bijvoorbeeld, kan dit mogelijk afwijkende activiteit zoals onregelmatig aanmeldingsproblemen activiteiten, aanmeldingen van onbekende bronnen of van IP-adressen met verdachte activiteit of aanmeldingen van apparaten die mogelijk geïnfecteerd detecteren. Met deze gegevens, identiteitsbeveiliging genereert rapporten en waarschuwingen waarmee u kunt deze gebeurtenissen risico onderzoeken en voert u juiste remediation of risicobeperking actie. Zie voor meer informatie [Azure Active Directory identiteitsbeveiliging][aad-identity-protection].

    U kunt ook de rapportfunctie van AAD in de portal van Azure gebruiken om de verdachte en andere-beveiliging activiteiten die binnen uw systeem te houden. AAD kunt een reeks Overzichtsrapporten genereren:

    [! [17]][17]

    Zie voor meer informatie over het gebruik van deze rapporten [Azure Active Directory rapportage Guide][aad-reporting-guide].

## <a name="topology-considerations"></a>Aandachtspunten voor de topologie

Als u een on-premises adreslijst met AAD integreert, configureert u Azure AD Connect implementatie van een topologie die het meest overeenkomt met de vereisten van uw organisatie. Topologieën die ondersteuning biedt voor Azure AD Connect onder andere het volgende:

- **Eén bos, één AAD-map**. In deze topologie kunt u Azure AD Connect gebruiken voor het synchroniseren van objecten en identiteit informatie in een of meer domeinen in één on-premises bos met een enkel AAD-tenant. Dit is de standaard-topologie geïmplementeerd door de aangepaste installatie van Azure AD Connect.

    [! [1]][1]

    Meerdere Azure AD Connect-synchronisatie-servers voor verschillende domeinen in de dezelfde on-premises implementatie bos verbinding met dezelfde AAD tenant, tenzij u een server Azure AD Connect-synchronisatie uitvoert in de modus tijdelijke niet maken (Zie onderstaande Server tijdelijke).

- **Meerdere forests, één AAD-map**. Gebruik deze topologie als er meer dan één on-premises implementatie bos. U kunt identiteitsgegevens samenvoegen zodat elke unieke gebruiker eenmaal wordt weergegeven in de map AAD, zelfs als dezelfde gebruiker in meer dan één bos bestaat. Alle forests dezelfde Azure AD Connect synchronisatieserver gebruiken. De synchronisatie-server Azure AD Connect heeft geen deel uitmaken van elk domein, maar deze bereikbaar moet zijn uit alle forests:

    [! [2]][2]

    In deze topologie, niet apart Azure AD Connect gebruikt servers elk on-premises implementatie bos verbinden met een enkel AAD-tenant synchroniseren. Dit kan resulteren in gedupliceerde identiteit informatie in AAD als gebruikers in meer dan één bos aanwezig zijn.

- **Meerdere forests, afzonderlijk topologieën**. Deze methode kunt u afzonderlijke forests identiteit informatie in een enkel AAD-tenant samenvoegen. Deze strategie is nuttig als u forests van andere organisaties (na een fusie of overname, bijvoorbeeld combineren wilt) en de identiteitsgegevens voor elke gebruiker is die slechts één bos:

    Als de GAL's in elke bos worden gesynchroniseerd, kan een gebruiker in één bos aanwezig zijn in een andere als contactpersoon zijn. Dit kan gebeuren als uw organisatie is bijvoorbeeld GALSync geïmplementeerd met Forefront identiteitsbeheer 2010 of Microsoft identiteitsbeheer 2016. In dit scenario kunt u opgeven dat gebruikers door hun *e* -kenmerk moeten worden geïdentificeerd. U kunt ook met de kenmerken *ObjectSID* en *msExchMasterAccountSID* identiteiten matchen. Dit is handig als u een of meer resource forests met uitgeschakelde accounts hebt.

- **Tijdelijke server**. In deze configuratie, kunt u een tweede exemplaar van de Azure AD Connect-synchronisatie-server parallel met de eerste uitvoeren. Deze structuur ondersteunt, zoals scenario's:

    - Beschikbaarheid.

    - Testen en implementeren van een nieuwe configuratie van de server Azure AD Connect-synchronisatie.

    - Inleiding tot een nieuwe server en buiten bedrijf stellen een oude configuratie. 

    In deze situaties is de tweede exemplaar wordt uitgevoerd in de *modus waarin u tijdelijk opslaat*. De server records geïmporteerde objecten en van synchronisatiegegevens in de database, maar de gegevens niet overgedragen aan AAD. Alleen als u tijdelijk opslaan-modus uitschakelen de server begint schrijven van gegevens naar AAD en ook wordt gestart uitvoeren wachtwoord schrijven-achterzijde van in de on-premises implementatie-mappen wanneer van toepassing:

    [! [4]][4]

    Zie voor meer informatie [Azure AD Connect-synchronisatie: operationele taken en overwegingen][aad-connect-sync-operational-tasks].

- **Meerdere AAD-mappen**. Het wordt aanbevolen dat u één AAD map voor een organisatie maken, maar er zijn situaties waarin u partition informatie over afzonderlijke AAD-mappen moet. In dit geval moet u ervoor zorgen dat elk object uit de on-premises implementatie bos wordt alleen in één AAD-map weergegeven, om te voorkomen van problemen met synchronisatie en wachtwoord terugschrijven.

    Als u wilt dit scenario implementeren, configureren afzonderlijk Azure AD Connect servers voor elke map AAD synchroniseren en gebruikmaken van het filteren van zodat elke Azure AD Connect-synchronisatie-server wordt uitgevoerd op een wederzijds set gegroepeerde objecten: 

    [! [5]][5]

Zie voor meer informatie over deze topologieën, [topologieën voor Azure AD Connect][aad-topologies].

## <a name="user-sign-in-considerations"></a>Gebruiker aanmelden overwegingen

Standaard de AAD-service wordt ervan uitgegaan dat gebruikers zich aanmelden doordat hetzelfde wachtwoord dat ze on-premises implementatie gebruiken en de synchronisatie-server Azure AD Connect Wachtwoordsynchronisatie tussen de on-premises domein en AAD configureert. In veel organisaties wordt dit nodig is, maar kunt u overwegen de bestaande beleid en de infrastructuur van uw organisatie. Bijvoorbeeld:

- Het beveiligingsbeleid van uw organisatie kan voorkomen synchroniseren ogen van de cloud.

- U mogelijk nodig dat gebruikers naadloze SSO ondervinden (zonder extra wachtwoord wordt gevraagd) wanneer machines het bedrijfsnetwerk bevinden toegang krijgen tot cloud resources in domein samengevoegd.

- Uw organisatie mogelijk al ADFS of een derde partij Federatie provider geïmplementeerd. U kunt configureren AAD voor het gebruik van deze infrastructuur implementeren verificatie en eenmalige aanmelding, in plaats van met wachtwoordinformatie in de cloud.

Zie [Azure AD verbinding gebruiker (+) op Opties]voor meer informatie[aad-user-sign-in].

## <a name="azure-ad-application-proxy-considerations"></a>Azure AD-toepassing proxy overwegingen

Azure AD gebruiken voor toegang tot on-premises implementatie-toepassingen.

- Laten zien uw on-premises implementatie-webtoepassingen toepassingsproxy verbindingslijnen door het onderdeel Azure AD-toepassing proxy beheren gebruiken. De toepassing proxy verbindingslijn wordt geopend een uitgaande netwerkverbinding met de toepassingsproxy Azure AD. Externe gebruikers aanvragen worden gerouteerd terug van AAD via deze verbinding naar de Webapps. Deze methode Hiermee verwijdert u moet binnenkomende poorten openen in de on-premises implementatie-firewall, aanvallen die worden aangeboden door uw organisatie wordt afgetrokken.

Zie voor meer informatie, [publiceren toepassingen met Azure AD-toepassingsproxy][aad-application-proxy].

## <a name="object-synchronization-considerations"></a>Aandachtspunten voor de synchronisatie object

De standaardconfiguratie van Azure AD Connect synchroniseert objecten van uw lokale AD-map op basis van de set regels die zijn opgegeven in het artikel [Azure AD Connect-synchronisatie: informatie over de standaardconfiguratie][aad-connect-sync-default-rules]. Alleen objecten die voldoen aan deze regels worden gesynchroniseerd, anderen worden genegeerd. Bijvoorbeeld, objecten van de gebruiker moet een unieke *sourceAnchor* -kenmerk en het kenmerk *accounEnabled* moet worden ingevuld. Gebruikersobjecten die ik heb een *sAMAccountName* -kenmerk of die beginnen met de tekst *AAD_* of *MSOL_* worden niet gesynchroniseerd. Azure AD Connect geldt verschillende regels voor de gebruiker, contactpersoon, groep, ForeignSecurityPrincipal en Computer objecten. Als u nodig hebt voor het wijzigen van de standaardset met regels, gebruikt u de synchronisatie regeleditor is geïnstalleerd met Azure AD Connect (ook beschreven in [Azure AD Connect-synchronisatie: informatie over de standaardconfiguratie][aad-connect-sync-default-rules]).

U kunt uw eigen filters als u wilt beperken de objecten die kan worden gedownload door domein of OU definiëren. Of complexere aangepaste filteren, zoals wordt beschreven in implementeert [Azure AD Connect-synchronisatie: configureren filteren][aad-filtering].

## <a name="security-considerations"></a>Veiligheidsoverwegingen

Voorwaardelijke toegangsbeheer gebruiken om te weigeren verificatieaanvragen van onverwachte bronnen:

- [Azure meervoudige verificatie MFA ()] activeren[ azure-multifactor-authentication] als een gebruiker probeert verbinding te maken van een niet-vertrouwde locatie (zoals op Internet) in plaats van een vertrouwde netwerk.

- Gebruik het type apparaat platform van de gebruiker (iOS, Android, Windows Mobile, Windows) om te bepalen-beleid voor toepassingen en functies.

- De status ingeschakeld/uitgeschakeld van gebruikers apparaten opnemen en opnemen in de access-beleid controles. Bijvoorbeeld als een gebruikerswachtwoord telefoon diefstal of moet deze worden geregistreerd als uitgeschakeld als u wilt voorkomen dat deze wordt gebruikt om toegang te krijgen.

- Het niveau van toegang aan een gebruiker op basis van groepslidmaatschap beheren. Regels voor [dynamische lidmaatschap AAD] [ aad-dynamic-membership-rules] groepsbeheer vereenvoudigen. Voor een kort overzicht van hoe dit werkt, raadpleegt u [Inleiding tot dynamische lidmaatschappen voor groepen][aad-dynamic-memberships].

- Voorwaardelijke risico clienttoegangsbeleid met AAD identiteitsbeveiliging gebruiken om geavanceerde beveiliging op basis van ongebruikelijke aanmeldingsproblemen activiteiten of andere gebeurtenissen.

Zie voor meer informatie [Azure Active Directory voorwaardelijke toegang][aad-conditional-access].

## <a name="scalability-considerations"></a>Aandachtspunten voor de schaalbaarheid

Schaalbaarheid is gericht door de service AAD en de configuratie van de synchronisatie-server Azure AD Connect:

- Voor de service AAD hoeft u niet te configureren nog opties voor het implementeren van schaalbaarheid. De service AAD ondersteunt schaalbaarheid op basis van replica's. AAD implementeert een één primaire replica, welke ingangen schrijven bewerkingen, en meerdere alleen-lezen secundaire partities. AAD wordt omgeleid poging tot schrijft aangebracht ten opzichte van secundaire replica's in de primaire replica. AAD vindt u eventuele consistentie. Alle wijzigingen in de primaire replica worden doorgegeven aan de secundaire replica's. Als de meeste bewerkingen op AAD gelezen in plaats van schrijft worden, wordt deze architectuur ook aangepast.

    Zie voor meer informatie [Azure AD: achter de schermen van onze directory geografische-redundante, ten zeerste beschikbaar, verdeelde cloud][aad-scalability].

- Voor de server Azure AD Connect synchroniseren, moet u bepalen hoeveel objecten u bent waarschijnlijk het synchroniseren van uw lokale map. Als u minder dan 100.000 objecten hebt, kunt u de standaard SQL Server Express LocalDB software Azure AD Connect voorzien. Als u een groot aantal objecten hebt, moet u een versie van SQL Server installeren en uitvoeren van een aangepaste installatie van de Azure AD Connect opgeven dat dit een bestaand exemplaar van SQL Server moet gebruiken.

## <a name="availability-considerations"></a>Aandachtspunten voor de beschikbaarheid

Als u met de schaalbaarheid bezwaren, betrekking hebben op beschikbaarheid van de AAD-service en de configuratie van Azure AD Connect:

- De AAD-service is ontworpen beschikbaarheid. Er zijn geen beschikbaarheidopties van de gebruiker te configureren. Geografische distributed is en dat wordt uitgevoerd in meerdere gegevens gecentreerd verspreiding overal ter wereld, met geautomatiseerde failover. Als een datacenter niet beschikbaar is, AAD zorgt ervoor dat uw directorygegevens beschikbaar voor access exemplaar in ten minste twee meer regionaal verspreid datacenters is.

    >[AZURE.NOTE] De SLA voor AAD basisfilters en Premium servicegaranties ten minste 99,9% beschikbaarheid. Er is geen SLA voor de gratis laag van AAD. Zie voor meer informatie, [SLA voor Azure Active Directory][sla-aad].

- Als u wilt verhogen van de beschikbaarheid van de synchronisatie-server Azure AD Connect kunt u een tweede exemplaar uitvoeren in tijdelijke modus, zoals wordt beschreven in de sectie [topologie overwegingen](#topology-considerations) . 

    Ook als u het exemplaar van SQL Server Express LocalDB die wordt geleverd met Azure AD Connect niet gebruikt, klikt u rekening moet houden beschikbaarheid voor SQL Server. Houd er rekening mee dat het alleen beschikbaarheid-oplossing ondersteund SQL cluster is. Oplossingen zoals spiegelen en klik op altijd worden niet ondersteund door Azure AD Connect.

    Zie voor aanvullende overwegingen over het beheren van de beschikbaarheid van de server Azure AD Connect-synchronisatie en het herstellen van na een storing [Azure AD Connect-synchronisatie: operationele taken en overwegingen - herstel][aad-sync-disaster-recovery].

## <a name="management-considerations"></a>Overwegingen bij

Er zijn twee aspecten AAD beheren:

1. Beheer van AAD in de cloud.

2. De synchronisatie-servers van Azure AD Connect onderhouden.

AAD biedt de volgende opties voor het beheren van domeinen en mappen in de cloud:

- [Azure Active Directory PowerShell-Module][aad-powershell]. Gebruik deze module als u nodig hebt om een script algemene Azure AD beheertaken zoals Gebruikersbeheer, domeinbeheer en voor het configureren van eenmalige aanmelding.

- Azure AD management blade in de portal van Azure. Deze blade beschikt u over een interactieve management-weergave van de map en kunt u bepalen en de meeste aspecten van AAD configureren.

    [! [10]][10]

Azure AD Connect installaties de volgende hulpprogramma's waarmee u zich voor het behoud van de services voor het synchroniseren van Azure AD Connect vanaf uw on-premises implementatie-computers.

- De Microsoft Azure Active Directory-Connect-console. Dit hulpmiddel kunt u de configuratie van de server Azure AD-synchronisatie wijzigen, aanpassen hoe synchronisatie plaatsvindt, inschakelen of tijdelijk opslaan-modus uitschakelen en overschakelen van de gebruiker aanmeldingsproblemen modus (u kunt inschakelen AD FS Meld u aan met de infrastructuur van uw on-premises implementatie).

- De Manager van de synchronisatie-Service. Gebruik het tabblad *bewerkingen* in dit hulpprogramma voor het beheren van het synchronisatieproces en detecteren of delen van het proces is mislukt. U kunt de synchronisatie van handmatig met deze functie activeren. 

    [! [12]][12]

    Het tabblad *verbindingslijnen* kunt u de verbindingen voor de domeinen (on-premises implementatie en in de cloud) waaraan de synchronisatie-engine is gekoppeld:

    [! [13]][13]

-  De synchronisatie-regels-Editor. Gebruik dit hulpprogramma om aan te passen van de manier waarop objecten wanneer ze worden gekopieerd tussen een on-premises adreslijst en AAD in de cloud worden getransformeerd. Dit hulpmiddel kunt u opgeven extra kenmerken en objecten voor synchronisatie en filters om te bepalen welke exemplaren moeten of niet moeten worden gesynchroniseerd.

    Zie voor meer informatie de sectie synchronisatie regel Editor in het document [Azure AD Connect-synchronisatie: informatie over de standaardconfiguratie][aad-connect-sync-default-rules].

De pagina [Azure AD Connect-synchronisatie: aanbevolen procedures voor het wijzigen van de standaardconfiguratie] [ aad-sync-best-practices] aanvullende informatie en tips voor het beheren van Azure AD Connect bevat.

## <a name="monitoring-considerations"></a>Overwegingen voor controle

Met behulp van een reeks agenten geïnstalleerd on-premises wordt statuscontrole uitgevoerd:

- Azure AD Connect-installaties een agent die wordt informatie over de synchronisatie van vastgelegd. Gebruik het blad Azure Active Directory verbinding maken met status in de portal van Azure om te controleren van de servicestatus en prestaties van Azure AD Connect. Zie voor meer informatie [Gebruik Azure AD verbinding servicestatus voor synchronisatie][aad-health].

- Als u wilt controleren van de status van de AD DS-domeinen en mappen van Azure, de Azure op AD verbinding maken met status voor AD DS-agent op een computer binnen de on-premises domein te installeren. Gebruik het blad Azure Active Directory verbinding maken met status in de portal van Azure AD DS monitor. Zie voor meer informatie [Gebruik Azure AD status verbinding maken met AD DS][aad-health-adds]

- Installeer de Azure op AD verbinding maken met status voor AD FS-agent om te controleren van de status van AD FS uitgevoerd in on-premises en het blad Azure Active Directory verbinding maken met status in de portal van Azure gebruiken om de AD DS te houden. Zie [Gebruik van Azure AD status verbinding maken met AD FS] voor meer informatie[aad-health-adfs]

Zie voor meer informatie over het installeren van de AD verbinding systeemstatus kunt vinden en hun vereisten [Azure AD verbinding systeemstatus Agent installatie][aad-agent-installation].

## <a name="sample-solution"></a>Voorbeeld-oplossing

In dit gedeelte worden de stappen voor het samenstellen van een steekproef-oplossing die u gebruiken kunt om te testen van de configuratie van AAD. De oplossing omvat de volgende elementen:

- Een webtoepassing met n lagen, volgens de beschrijving namelijk [VMs uitgevoerd voor een architectuur N lagen op Azure]structuur[implementing-a-multi-tier-architecture-on-Azure].

- Een test clientcomputer. Het is raadzaam om met behulp van een andere VM voor deze computer. De configuratie van deze VM is niet belangrijk is, zo lang maken als u beheerderstoegang hebt en deze verbinding met de n-aantal lagen-webtoepassing maken kunt.

- Een gesimuleerd on-premises omgeving waarin een eigen domein gemaakt met AD DS.

Dit zijn de scenario's die illustreren van de volgende stappen uit:

- Inschakelen toegang tot de n-aantal lagen webtoepassing uitgevoerd in de cloud aan externe gebruikers met AAD-wachtwoordverificatie leveren.

- Toegang tot de n-aantal lagen webtoepassing uitgevoerd in de cloud naar gebruikers met AAD-wachtwoordverificatie leveren binnen de organisatie, inschakelen.

### <a name="prerequisites"></a>Vereisten voor

De volgende stappen wordt ervan uitgegaan dat de volgende vereisten:

- U hebt een bestaand Azure-abonnement waarin u resourcegroepen kunt maken.

- U kunt de [Interface van Azure-opdrachtregel]hebt geïnstalleerd[azure-cli].

- U hebt gedownload en geïnstalleerd van de meest recente versie. Zie [hier] [ azure-powershell-download] voor instructies.

- U kunt een kopie van de [makecert] hebt geïnstalleerd[ makecert] hulpprogramma op de clientcomputer test.

- U hebt toegang tot een development-computer waarop Visual Studio is geïnstalleerd.

### <a name="create-the-n-tier-web-application-architecture"></a>De architectuur van de toepassing n lagen web maken

1. Maken van een map met de naam `Scripts` waarvan een submap `Parameters`.

2. Download de [Deploy-ReferenceArchitecture.ps1] [ solution-script] PowerShell-script naar de map.

3. Maak een andere submap Windows in de map Parameters.

4. De volgende bestanden naar de map Parameters/Windows downloaden:

    - [virtualNetwork.parameters.json][vnet-parameters-windows]

    - [networkSecurityGroup.parameters.json][nsg-parameters-windows]

    - [webTierParameters.json][webtier-parameters-windows]

    - [businessTierParameters.json][businesstier-parameters-windows]

    - [dataTierParameters.json][datatier-parameters-windows]

    - [managementTierParameters.json][managementtier-parameters-windows]

5. Bewerk het bestand **Deploy-ReferenceArchitecture.ps**1 in de map Scripts, waarna de volgende regel, zodat de resourcegroep die moet worden gemaakt of voor het opslaan van de VM en resources die zijn gemaakt door het script wijzigen:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-ntier-rg"
        ```

6. Bewerk de volgende bestanden in de map met **parameters/venster**s en stel de `resourceGroup` waarde in de `virtualNetworkSettings` sectie in elk van deze bestanden moeten hetzelfde als die u hebt opgegeven in het **Deploy-ReferenceArchitecture.ps1** script-bestand.

    - networkSecurityGroup.parameters.json

    - webTierParameters.json

    - businessTierParameters.json

    - dataTierParameters.json

    - managementTierParameters.json

7. Open een Azure PowerShell-venster, verplaatsen naar de map Scripts en voer de volgende opdracht:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows ntier
        ```

    Vervang **<subscription id>** met uw Azure abonnement-ID.

    Voor **<location>**, Geef een Azure regio, zoals *eastus* of *westus*.

8. Wanneer het script is voltooid, gebruikt u de Azure-portal naar het openbare IP-adres van de verdeling van de web-laag belasting (*AB aad-ntier-web-kg*):

    [! [18]][18]

9. Meld u aan bij uw test client computer (of VM) en controleer of u toegang hebt tot de web-laag met behulp van Internet Explorer om naar het openbare IP-adres van de verdeling van de web-laag belasting. De standaard IIS-pagina moet worden weergegeven.

### <a name="simulate-configuration-of-a-public-web-site"></a>Configuratie van een openbare website simuleren

>[AZURE.NOTE] De volgende stappen simuleren de web-laag koppelen aan de DNS-naam www.contoso.com door het hostsbestand op de clientcomputer test te wijzigen. Bovendien zijn de VMs web laag geconfigureerd met zelfondertekende certificaten en vals hostingprovider certificeringsinstantie. Dit is voor het testen van uitsluitend en **dat moet u deze technieken in een productieomgeving niet gebruiken**.

1. Op de clientcomputer test bewerken van het bestand **C:\Windows\System32\drivers\etc\hosts** met behulp van Kladblok en een fragment waarin de naam www.contoso.com gekoppeld aan het openbare IP-adres van de verdeling van de web-laag belasting toevoegen:

        ```text
        # Copyright (c) 1993-2009 Microsoft Corp.
        #
        # This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
        #
        # This file contains the mappings of IP addresses to host names. Each
        # entry should be kept on an individual line. The IP address should
        # be placed in the first column followed by the corresponding host name.
        # The IP address and the host name should be separated by at least one
        # space.
        #
        # Additionally, comments (such as these) may be inserted on individual
        # lines or following the machine name denoted by a '#' symbol.
        #
        # For example:
        #
        #      102.54.94.97     rhino.acme.com          # source server
        #       38.25.63.10     x.acme.com              # x client host
        
        # localhost name resolution is handled within DNS itself.
        #   127.0.0.1       localhost
        #   ::1             localhost
        
        52.165.38.64    www.contoso.com
        ```

2. Controleer of dat u nu naar www.contoso.com vanaf de clientcomputer test bladeren kunt. Dezelfde standaard IIS pagina worden weergegeven als voorheen.

3. Klik op de clientcomputer test open een opdrachtpromptvenster als beheerder en gebruik het hulpprogramma makecert tot c
4. een certificaat valse basiscertificeringsinstantie maken:

        ```
        makecert -sky exchange -pe -a sha256 -n "CN=MyFakeRootCertificateAuthority" -r -sv MyFakeRootCertificateAuthority.pvk MyFakeRootCertificateAuthority.cer -len 2048
        ```

    Controleer of de volgende bestanden worden gemaakt:

    - MyFakeRootCertificateAuthority.cer

    - MyFakeRootCertificateAuthority.pvk

4. Voer de volgende opdracht voor het installeren van de test basiscertificeringsinstantie:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Gebruik de certificeringsinstantie testen om een certificaat voor www.contoso.com te genereren:

        ```
        makecert -sk pkey -iv MyFakeRootCertificateAuthority.pvk -a sha256 -n "CN=www.contoso.com" -ic MyFakeRootCertificateAuthority.cer -sr localmachine -ss my -sky exchange -pe
        ```

6. De opdracht **mmc** uitvoeren en de module Certificaten voor het computeraccount van de lokale computer toevoegen.

7. In de */Certificates (lokale Computer) / persoonlijk/certificaat/* opslaan, exporteren van het certificaat www.contoso.com met bijbehorende persoonlijke sleutel naar een bestand met de naam www.contoso.com.pfx:

    [! [20]][20]

8. Ga terug naar de Azure-portal en verbinding maken met de laag management VM (*AB-aad-ntier-mgmt-vm1*). De standaardnaam van de gebruiker is *testuser* met wachtwoord *AweS0me@PW*:

    [! [21]][21]
    
9. Uit de laag management VM verbinding maken met elk van de weblaag VMs beurtelings met gebruikersnaam *testuser* en wachtwoord *AweS0me@PW*, en de volgende taken uitvoeren. Houd er rekening mee dat het VMs de particuliere adressen IP 10.0.1.4, 10.0.1.5 en 10.0.1.6 hebben:

    >[AZURE.NOTE] De weblaag VMs hoeft slechts privé IP-adressen. U kunt alleen verbinding maken met deze met behulp van de laag management VM.

    1. Kopieer de bestanden *www.contoso.com.pfx* en *MyFakeRootCertificateAuthority.cer* van de clientcomputer test.
    
    2. Open een opdrachtpromptvenster als beheerder, verplaatsen naar de map die de bestanden die u hebt gekopieerd en voer de volgende opdrachten:
    
        ```
        certutil.exe -privatekey -importPFX my www.contoso.com.pfx NoExport

        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

    3. Voer de `mmc` opdracht, de module Certificaten voor het computeraccount van de lokale computer kunt toevoegen en controleer of dat de volgende certificaten zijn geïnstalleerd:

        - \Certificates (lokale Computer) \Personal\Certificates\ www.contoso.com

        - \Certificates (lokale Computer) \Trusted hoofdsite-certificering Authorities\Certificates\MyFakeRootCertificateAuthority

    4. Start de beheerconsole van Internet Information Services (IIS) en Ga naar *de website Sites\Default* op de computer.

    5. Klik in het deelvenster *Acties* op *Bindingen*en een https-binding met behulp van de www.contoso.com SSL-certificaat toevoegen:

        [! [22]][22]

10. Ga terug naar de clientcomputer testen en verifiëren dat u kunt nu naar https://www.contoso.com bladeren.

### <a name="create-an-azure-active-directory-tenant"></a>Maak een tenant Azure Active Directory

1. Met de Azure-portal, maakt u een Azure Active Directory-tenant zoals *myaadname*. onmicrosoft.com, waar *myaadname* de naam van een map die u hebt geselecteerd is.

2. De *beheerder* de rol GlobalAdmin naar de map met de naam van een gebruiker toevoegen. Noteer het nieuwe wachtwoord.

3. Met Internet Explorer, blader naar https://account.activedirectory.windowsazure.com/ en meld u aan als admin@ *myaadname*. onmicrosoft.com. Wijzig uw wachtwoord wanneer u wordt gevraagd.

### <a name="create-and-deploy-a-test-web-application"></a>Maak en implementeer een webtoepassing testen

1. Visual Studio met op de computer ontwikkeling, maakt u een ASP.NET-webtoepassing met de naam ContosoWebApp1 (gebruikmaken van .NET Framework 4.5.2):

    [! [23]][23]

2. Selecteer de sjabloon *MVC* , de verificatie wijzigen om te *werken en School-accounts*en geef de naam van uw AAD-tenant. Een App-Service niet worden maken in de cloud.

    [! [24]][24]

3. Maken en uitvoeren van de toepassing test de verificatie. Voer in de aanmeldingspagina de naam van het account admin@ *myaadname*. onmicrosoft.com, het wachtwoord opgeven en klik vervolgens op *aanmelden*:

    [! [25]][25]

4. Controleer of AAD toestemming voor u aanmelden en lees uw profiel vraagt, en begint vervolgens met het uitvoeren van de webtoepassing met de identiteit van de beheerder.

5. Internet Explorer sluiten en terugkeren naar Visual Studio.

6. Open het bestand web.config en in de `<appSettings>` sectie, wijzigt u de waarde van de *ida: PostLogoutRedirectUri* -toets om *https://www.contoso.com:443 /*. Sla het bestand.

7. Klik in het *Oplossingverkenner* -venster met de rechtermuisknop op het project ContosoWebApp1, klik op *publiceren*.

8. Klik in het venster *Web publiceren* op *aangepast*. Een aangepast profiel benoemde *ContosoWebApp1*maken.

9. Klik op de pagina *verbinding* de *publiceermethode* instellen op *Bestandssysteem* en geef een map genaamd *ContosoWebApp*, zich bevindt in een handige locatie op de computer.

10. Stel de *configuratie* naar *versie*op de pagina *-Instellingen* .

11. Klik op *publiceren*op de pagina *Preview* .

12. Verbinding maken met elk webserver beurtelings (via de laag management VM) en de volgende taken uitvoeren:

    1. Kopieer de map *ContosoWebApp* en de inhoud van de computer van de ontwikkeling naar de map *C:\inetpub* .

    2. De beheerconsole van Internet Information Services (IIS) gebruikt, navigeer naar *de website Sites\Default* op de computer.

    3. Klik in het deelvenster *Acties* op *Basis-instellingen*en het fysieke pad van de website wijzigen in *%SystemDrive%\inetpub\ContosoWebApp*:

        [! [26]][26]

### <a name="publish-the-test-web-application-through-aad"></a>De webtoepassing testen door AAD publiceren

1. Meld u aan bij de Azure-portal en Ga naar uw adreslijst AAD.

2. Klik op het tabblad *toepassingen* op de toepassing ContosoWebApp1.

3. Verifiëren dat uw toepassing is toegevoegd aan de map en klik vervolgens op het tabblad *configureren* .

4. Wijzig de *Teken-op-URL* naar https://www.contoso.com:443, en stel de *URL van het antwoord* op https://www.contoso.com:443 (dezelfde URL).

5. Sla de configuratie.

6. Navigeer naar https://www.contoso.com op de clientcomputer test. Verifiëren dat AAD u om uw referenties vraagt en meld u vervolgens.

>[AZURE.NOTE] Dit is het eindpunt voor het eerste scenario: toegang bieden tot de n-aantal lagen webtoepassing uitgevoerd in de cloud aan externe gebruikers met AAD-wachtwoordverificatie leveren.

### <a name="create-a-simulated-on-premises-environment-with-active-directory"></a>Een gesimuleerd on-premises omgeving maken met Active Directory

De on-premises omgeving bevat twee domeincontrollers voor de `contoso.com` domein en twee servers voor het hosten van de Azure AD Connect service synchroniseren. De servers voor het hosten van Azure AD Connect zijn niet domein-gekoppeld.

1. Ga terug naar de map met het script gebruikt om te maken van de webtoepassing N lagen in Verkenner.

2. Klik in de map Parameters toevoegen een andere sub-map met de naam `Onpremise`.

3. De volgende bestanden naar Parameters/lokale map downloaden:

    - [toevoegen-telt-domein-controller.parameters.json][add-adds-domain-controller-parameters]

    - [Maak-telt-bos-extension.parameters.json][create-adds-forest-extension-parameters]

    - [virtualMachines-adc.parameters.json][virtualMachines-adc-parameters]

    - [virtualMachines-adc-joindomain.parameters.json][virtualMachines-adc-joindomain-parameters]

    - [virtualMachines-adds.parameters.json][virtualMachines-adds-parameters]

    - [virtualNetwork.parameters.json][virtualNetwork-parameters]

    - [virtualNetwork-telt-dns.parameters.json][virtualNetwork-adds-dns-parameters]

5. Bewerk het bestand Deploy-ReferenceArchitecture.ps1 in de map Scripts, waarna de volgende regel, zodat de resourcegroep die moet worden gemaakt of voor het opslaan van de VM en resources die zijn gemaakt door het script wijzigen:

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-onpremise-rg"
        ```

6. Bewerk de volgende bestanden in de map Parameters/lokale en stel de `resourceGroup` waarde in de `virtualNetworkSettings` sectie in elk van deze bestanden moeten hetzelfde als die u hebt opgegeven in het Deploy-ReferenceArchitecture.ps1 script-bestand.

    - virtualMachines-adds.parameters.json

    - virtualMachines-adc.parameters.json

    - virtualNetwork.parameters.json

    - virtualNetwork-telt-dns.parameters.json

7. Open een Azure PowerShell-venster, verplaatsen naar de map Scripts en voer de volgende opdracht:

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows onpremise
        ```

    Vervang `<subscription id>` met uw Azure abonnement-ID.

    Voor `<location>`, typt u een Azure gebied, bijvoorbeeld `eastus` of `westus`.

### <a name="install-and-configure-the-azure-ad-connect-sync-service"></a>De service Azure AD Connect-synchronisatie installeren en configureren

De configuratie geïllustreerde in deze stappen bestaat uit twee instanties van de Azure AD Connect sync-service. De eerste is actief terwijl het tweede is geconfigureerd in de modus tijdelijk opslaan en op te geven snelle failover beschikbaarheid als de eerste server is mislukt.

1. Gebruik van de Azure-portal, Ga naar de resourcegroep vasthouden van de VMs voor de Azure AD Connect sync services (*AB-aad-lokale-rg* al dan niet standaard), en verbinding maken met de *AB-aad-lokale-adc-vm1* VM. Meld u aan als *testuser* met wachtwoord *AweS0me@PW*.

2. Azure AD Connect downloaden van [hier][aad-connect-download].

3. Het installatieprogramma van Azure AD Connect uitvoeren en een installatie met de optie *aanpassen* uitvoeren.

    >[AZURE.NOTE] U kunt de optie *Expresinstellingen* niet gebruiken omdat de VM Azure AD Connect-synchronisatie hostingservice niet domein behoren is.

4. Klik op de pagina *vereiste onderdelen* niets de optionele configuratie-instellingen de standaardopties accepteren.

5. Selecteer op de pagina *aanmelding* *Wachtwoordsynchronisatie*.

6. Voer op de pagina *verbinding maken met Azure AD* admin@ *myaadname*. onmicrosoft.com, waar *myaadname* de naam van uw AAD-tenant is. Voer het wachtwoord voor het account.

7. Geef op de pagina *verbinding maken met uw mappen* contoso.com voor de bos (Typ de waarde in omdat deze niet wordt weergegeven in de vervolgkeuzelijst) CONTOSO\testuser invoeren voor de gebruikersnaam in te voeren, geef AweS0me@PW voor het wachtwoord, en klik vervolgens op *Map toevoegen*.

8. Accepteer de standaardwaarde voor de UPN op de pagina *configuratie van aanmeldingsproblemen Azure AD* . *Doorgaan zonder een geverifieerde domeinen*controleren.

    >[AZURE.NOTE] De map contoso.com worden weergegeven als *Niet geverifieerd*. Als u wilt controleren of een domeinnaam, moet u een TXT-record voor uw domeinnaam registrar. In dit voorbeeld is de on-premises domein niet geregistreerd extern. Zie voor meer informatie [toevoegen een aangepaste domeinnaam met Azure Active Directory][aad-custom-directory].

9. Selecteer *synchroniseren alle domeinen en organisatie-eenheden* (de standaardinstelling) op de pagina *domein en organisatie-eenheid filteren* .

10. Klik op de pagina *een unieke aanduiding voor uw gebruikers* de standaardwaarden te accepteren.

11. Selecteer *alle gebruikers en apparaten synchroniseren* (de standaardinstelling) op de pagina *gebruikers en apparaten filteren* .

12. Selecteer op de pagina *optionele onderdelen* *wachtwoord write-backs*.

13. Klik op de pagina *gereed is voor het configureren van* selecteert u *de code synchronisatie starten wanneer configuratie is voltooid*, maar laat *inschakelen tijdelijk opslaan van de modus* uitgeschakeld.

14. Bevestigen dat de configuratieproces is voltooid zonder fouten en sluit het installatieprogramma.

15. In de portal Azure verbinding maken met de *AB-aad-lokale-adc-vm2* VM. Meld u aan als *testuser* met wachtwoord *AweS0me@PW*.

16. Azure AD Connect downloaden en voer vervolgens het installatieprogramma.

17. De wizard gebruiken en reageren zoals is beschreven in de stappen 3 tot en met 12.

18. Selecteer op de pagina *gereed is voor het configureren van* *begin van het synchronisatieproces wanneer configuratie is voltooid*en **ook selecteren om het** *tijdelijk opslaan modus inschakelen*. Hierdoor wordt de service voor het synchroniseren van Azure AD Connect voor meer informatie over accounts en objecten van het domein contoso.com ophalen en ze hebt opgeslagen in de database, maar deze niet wordt deze informatie aan bij uw tenant AAD verzenden.

14. Bevestigen dat de configuratieproces is voltooid zonder fouten en sluit het installatieprogramma.

### <a name="test-the-aad-configuration"></a>Test de configuratie AAD

1. Gebruik van de Azure-portal, overschakelen naar uw adreslijst AAD, opent u het blad Azure Active Directory, klikt u op *gebruikers en groepen*en klik vervolgens op *alle gebruikers* om de lijst met gebruikers en groepen die zijn gesynchroniseerd met de map weer te geven. Hier ziet u gebruikers voor de volgende accounts:

    - beheerder (admin@ *myaadname*. onmicrosoft.com)

    - On-Premises synchronisatie Adreslijstserviceaccount (Sync_ADC1_*nnnnnnnnnnnn*@*myaadname*. onmicrosoft.com)

    - On-Premises synchronisatie Adreslijstserviceaccount (Sync_ADC2_*nnnnnnnnnnnn*@*myaadname*. onmicrosoft.com)

2. Klik in de portal Azure Navigeer naar de resourcegroep vasthouden van de VMs voor de AD DS-domeincontrollers (*AB-aad-lokale-rg* al dan niet standaard) en verbinding maken met de *AB-aad-lokale-ad-vm1* VM. Meld u aan als *testuser* met wachtwoord *AweS0me@PW*.

3. Een nieuwe domeingebruiker met de naam Diane Tibbot, met aanmeldingsnaam met de console *Active Directory-gebruikers en Computers* toevoegen dianet@contoso.com. Geef een wachtwoord van uw keuze. De gebruiker lid zijn van de lokale beheerdersgroep maken (dit is alleen zodat u lokaal als deze gebruiker later aanmelden kunt-alleen computers in het domein DCs zijn).

4. Ga naar de *AB-aad-lokale-adc-vm1* VM, een PowerShell-venster openen en voer de volgende opdrachten:

        ```[powershell]
        Import-Module ADSync
        Start-ADSyncSyncCycle -PolicyType Delta
        ```

    Controleer of de opdracht *Success*geeft als resultaat.

    >[AZURE.NOTE] Deze stap is nodig omdat het al dan niet standaard het synchronisatieproces volgens de planning moet uitvoeren om de 30 minuten. Deze opdrachten afdwingen een synchronisatie direct worden uitgevoerd.

5. Ga terug naar de Azure-portal, overschakelen naar uw adreslijst AAD openen van het blad Azure Active Directory, klikt u op *gebruikers en groepen*en klik vervolgens op *alle gebruikers*. U ziet nu Diane Tibbot (dianet@ *myaadname*. onmicrosoft.com) worden weergegeven in de lijst met gebruikers.

6. Klik in het blad Azure Active Directory *Bedrijfstoepassingen*op en klik vervolgens op *alle toepassingen*. Klik op de *ContosoWebApp1* -toepassing en klik vervolgens op *gebruikers en groepen*. Klik in de werkbalk op *toevoegen*. Klik op *gebruikers en groepen*en selecteer *Lynn Tibbot*. Klik op *toewijzen*.

7. Met Internet Explorer, navigeer naar de site https://account.activedirectory.windowsazure.com. Meld u aan als dianet@ *myaadname*. onmicrosoft.com met het wachtwoord dat u eerder hebt opgegeven.

    >[AZURE.NOTE] Klik niet op het pictogram ContosoWebApp in de lijst met toepassingen. AAD wordt geprobeerd de webtoepassing vinden op het openbaar vermelde DNS-adres voor www.contoso.com, die verschilt van het adres van de verdeling van de belasting van uw web-laag.

8. Klik op het tabblad *profiel* . De details van de gebruiker (als u een opgegeven wanneer deze is gemaakt) moeten worden weergegeven.

    >[AZURE.NOTE] Als u op *wachtwoord wijzigen*, kunt u zijn niet toegestaan voor deze taak zoals deze bewerking moet worden ingeschakeld door een beheerder eerst. Zie voor meer informatie [aan de slag met wachtwoordbeheer][aad-password-management].

### <a name="enable-on-premises-users-to-run-the-application-by-using-authentication-through-aad"></a>On-premises gebruikers de toepassing wilt uitvoeren met behulp van authenticatie via AAD inschakelen

1. Ga terug naar de domeincontroller *AB-aad-lokale-ad-vm1* VM.

2. Open de *DNS-beheer* -console en een nieuwe hostrecord voor www.contoso.com toevoegen. Geef het openbare IP-adres van de verdeling van de web-laag belasting.

3. Kopieer het bestand *MyFakeRootCertificateAuthority.cer* van de test clientcomputer (u deze bestanden gemaakt in de procedure [Simulate configuratie van een openbare website](#simulate-configuration-of-a-public-web-site)
    
4. Open een opdrachtpromptvenster als beheerder, verplaatsen naar de map die het bestand dat u zojuist hebt gekopieerd en voer de volgende opdracht:

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. Met Internet Explorer, navigeer naar https://www.contoso.com. Controleer of de AAD aanmeldingspagina voor de webtoepassing ContosoWebApp1 wordt weergegeven.

6. Meld u aan als dianet@ *myaadname*. onmicrosoft.com. De toepassing moet worden uitgevoerd en u goed aanmelden.

## <a name="next-steps"></a>Volgende stappen

- Informatie over de aanbevolen procedures voor het [uitbreiden van uw domein on-premises implementatie Hiermee Azure][adds-extend-domain]

- Informatie over de aanbevolen procedures voor het [maken van een Hiermee resource bos] [ adds-resource-forest] in Azure wordt aangegeven

<!-- links -->
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[aad-explained]: https://youtu.be/tj_0d4tR6aM
[aad-editions]: ../active-directory/active-directory-editions.md
[guidance-adds]: ./guidance-iaas-ra-secure-vnet-ad.md
[sla-aad]: https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/
[azure-multifactor-authentication]: ../multi-factor-authentication/multi-factor-authentication.md
[aad-conditional-access]: ../active-directory//active-directory-conditional-access.md
[aad-dynamic-membership-rules]: ../active-directory/active-directory-accessmanagement-groups-with-advanced-rules.md
[aad-dynamic-memberships]: https://youtu.be/Tdiz2JqCl9Q
[aad-user-sign-in]: ../active-directory/active-directory-aadconnect-user-signin.md
[aad-sync-requirements]: ../active-directory/active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md
[aad-topologies]: ../active-directory/active-directory-aadconnect-topologies.md
[aad-filtering]: ../active-directory/active-directory-aadconnectsync-configure-filtering.md
[aad-scalability]: https://blogs.technet.microsoft.com/enterprisemobility/2014/09/02/azure-ad-under-the-hood-of-our-geo-redundant-highly-available-distributed-cloud-directory/
[aad-connect-sync-default-rules]: ../active-directory/active-directory-aadconnectsync-understanding-default-configuration.md
[aad-identity-protection]: ../active-directory/active-directory-identityprotection.md
[aad-password-management]: ../active-directory/active-directory-passwords-customize.md
[aad-application-proxy]: ../active-directory/active-directory-application-proxy-enable.md
[aad-connect-sync-operational-tasks]: ../active-directory/active-directory-aadconnectsync-operations.md#staging-mode
[aad-custom-domain]: ../active-directory/active-directory-add-domain.md
[aad-powershell]: https://msdn.microsoft.com/library/azure/mt757189.aspx
[aad-sync-disaster-recovery]: ../active-directory/active-directory-aadconnectsync-operations.md#disaster-recovery
[aad-sync-best-practices]: ../active-directory/active-directory-aadconnectsync-best-practices-changing-default-configuration.md
[aad-adfs]: ../active-directory/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs
[aad-health]: ../active-directory/active-directory-aadconnect-health-sync.md
[aad-health-adds]: ../active-directory/active-directory-aadconnect-health-adds.md
[aad-health-adfs]: ../active-directory/active-directory-aadconnect-health-adfs.md
[aad-agent-installation]: ../active-directory/active-directory-aadconnect-health-agent-install.md
[aad-reporting-guide]: ../active-directory/active-directory-reporting-guide.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-powershell-download]: ../powershell-install-configure.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/Deploy-ReferenceArchitecture.ps1
[vnet-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/webTier.parameters.json
[businesstier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/businessTier.parameters.json
[datatier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/dataTier.parameters.json
[managementtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/managementTier.parameters.json
[makecert]: https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/add-adds-domain-controller.parameters.json
[create-adds-forest-extension-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/create-adds-forest-extension.parameters.json
[virtualMachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adds.parameters.json
[virtualNetwork-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork.parameters.json
[virtualNetwork-adds-dns-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork-adds-dns.parameters.json
[virtualMachines-adc-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc.parameters.json
[virtualMachines-adc-joindomain-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc-joindomain.parameters.json
[aad-connect-download]: http://www.microsoft.com/download/details.aspx?id=47594
[aad-custom-directory]: ../active-directory/active-directory-add-domain.md
[aad-password-management]: ../active-directory/active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[0]: ./media/guidance-identity-aad/figure1.png "Cloud identiteit architectuur met Azure Active Directory"
[1]: ./media/guidance-identity-aad/figure2.png "Één van bos, één AAD directory topologie"
[2]: ./media/guidance-identity-aad/figure3.png "Meerdere forests, één AAD directory topologie"
[4]: ./media/guidance-identity-aad/figure5.png "Tijdelijke Servertopologie"
[5]: ./media/guidance-identity-aad/figure6.png "Topologie van meerdere AAD-mappen"
[6]: ./media/guidance-identity-aad/figure7.png "Een aangepaste installatie van Azure AD verbinden met synchronisatie met een specifieke exemplaar van SQL Server selecteren"
[7]: ./media/guidance-identity-aad/figure8.png "De methode voor eenmalige aanmelding voor aanmelding opgeven"
[8]: ./media/guidance-identity-aad/figure9.png "Domein en organisatie-eenheid filteropties opgeven"
[9]: ./media/guidance-identity-aad/figure10.png "Wachtwoord terugschrijven inschakelen"
[10]: ./media/guidance-identity-aad/figure11.png "Het blad voor het beheer van Azure AD in de portal"
[11]: ./media/guidance-identity-aad/figure12.png "De Azure AD Connect-console"
[12]: ./media/guidance-identity-aad/figure13.png "Het tabblad bewerkingen in het servicebeheer van synchronisatie"
[13]: ./media/guidance-identity-aad/figure14.png "Het tabblad verbindingslijnen in het servicebeheer van synchronisatie"
[14]: ./media/guidance-identity-aad/figure15.png "De synchronisatie-Editor voor regels"
[15]: ./media/guidance-identity-aad/figure16.png "Het blad Azure Active Directory verbinding maken met status in de status van de synchronisatie met Azure-portal"
[16]: ./media/guidance-identity-aad/figure17.png "Het blad Azure Active Directory verbinding maken met status in de AD DS servicestatus met Azure-portal"
[17]: ./media/guidance-identity-aad/figure18.png "Beveiligingsrapporten beschikbaar in de portal van Azure"
[18]: ./media/guidance-identity-aad/figure19.png "De markering van het openbare IP-adres van de verdeling van de web-laag belasting Azure-portal"
[19]: ./media/guidance-identity-aad/figure20.png "Internet Explorer gebruiken om naar het openbare IP-adres van de verdeling van de web-laag belasting"
[20]: ./media/guidance-identity-aad/figure21.png "De certificaten module met het certificaat www.contoso.com"
[21]: ./media/guidance-identity-aad/figure22.png "Verbinding maken met de laag management VM"
[22]: ./media/guidance-identity-aad/figure23.png "De HTTPS-binding voor de standaardwebsite maken"
[23]: ./media/guidance-identity-aad/figure24.png "De webtoepassing ContosoWebApp1 maken"
[24]: ./media/guidance-identity-aad/figure25.png "De eigenschappen van de verificatie van de webtoepassing ContosoWebApp1 instellen"
[25]: ./media/guidance-identity-aad/figure26.png "Aanmelden bij Azure AAD vanuit de webtoepassing ContosoWebApp1"
[26]: ./media/guidance-identity-aad/figure27.png "De map voor de standaard-website wijzigen"