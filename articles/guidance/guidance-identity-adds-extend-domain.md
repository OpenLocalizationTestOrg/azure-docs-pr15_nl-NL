<properties
   pageTitle="Azure architectuur referentie - IaaS: Active Directory uitbreiden naar Azure | Microsoft Azure"
   description="Het implementeren van een beveiligde hybride netwerkarchitectuur met Active Directory-autorisatie in Azure wordt aangegeven."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
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
   ms.date="07/19/2016"
   ms.author="telmos"/>

# <a name="extending-active-directory-directory-services-adds-to-azure"></a>Active Directory-adreslijstservices (Hiermee) uitbreiden naar Azure

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

In dit artikel worden aanbevolen procedures voor het uitbreiden van uw omgeving voor Active Directory (AD) naar Azure verdeelde verificatieservices verlenen met [Active Directory Domain Services (AD DS)][active-directory-domain-services]. Deze architectuur breidt die in de [uitvoering van een beveiligde hybride netwerkarchitectuur in Azure] artikelen beschreven[ implementing-a-secure-hybrid-network-architecture] en [implementeren van een beveiligde hybride netwerkarchitectuur met internettoegang in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access].

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen: [Resourcemanager] [ resource-manager-overview] en klassieke. Deze verwijzing architectuur gebruikt resourcemanager, waarin u wordt aangeraden voor nieuwe implementaties.

AD DS kunt u de identiteit verifiëren. Deze identiteiten kunnen deel uitmaakt van gebruikers, computers, toepassingen en andere bronnen die deel uitmaken van een beveiligingsdomein. U kunt lokale AD DS hosten kan, maar in een hybride scenario waar elementen van een toepassing in Azure bevinden zich efficiënter deze functionaliteit en de AD-opslagplaats repliceren in de cloud. Deze methode verkleint de latentie die worden veroorzaakt door te sturen verificatie en voor lokale autorisatieaanvragen vanuit de cloud weer terug naar AD DS on-premises implementatie uitgevoerd. 

Er zijn twee manieren voor het hosten van uw adreslijstservices in Azure wordt aangegeven:

1. U kunt [Azure Active Directory (AAD)] [ azure-active-directory] maken van een nieuw AD-domein in de cloud en dit koppelen aan een on-premises AD-domein. Vervolgens setup [Azure AD Connect] [ azure-ad-connect] op pas repliceren identiteiten gehouden de bibliotheek van de on-premises implementatie in de cloud. Opmerking: de map in de cloud is **niet** een uitbreiding van de on-premises-systeem, het is een kopie die de dezelfde identiteiten bevat. Wijzigingen in deze identiteiten on-premises implementatie wordt gekopieerd naar de cloud, maar de wijzigingen hebt aangebracht in de cloud **niet** worden gerepliceerd terug naar de on-premises domein. Bijvoorbeeld moeten wachtwoorden worden uitgevoerd on-premises implementatie en Azure AD Connect gebruiken om te kopiëren van de wijziging in de cloud. Ook, houd er rekening mee dat het hetzelfde exemplaar van AAD kan worden gekoppeld aan meer dan één exemplaar van AD DS; AAD bevat de identiteit van elke AD-bibliotheek waaraan deze is gekoppeld.

    AAD is handig voor situaties waarin de on-premises netwerk en Azure virtuele netwerk hostingprovider de cloud-resources zijn niet rechtstreeks gekoppeld via een VPN-tunnel of ExpressRoute circuitlijnen. Hoewel deze oplossing eenvoudige is, deze mogelijk niet geschikt voor systemen waar onderdelen over de grens op-premises/cloud kunnen migreren als volgt opnieuw configureren van AAD kan vereisen. Bovendien omgaat AAD alleen met gebruikersverificatie in plaats van computerverificatie. Sommige toepassingen en services, zoals SQL Server, kunnen vragen om verificatie in dat geval deze oplossing niet nodig is.

2. U kunt een VM AD DS uitvoert als domeincontroller in Azure wordt aangegeven, uitbreiden van uw bestaande AD-infrastructuur van uw on-premises implementatie-datacenter implementeren. Deze methode werkt komt vaker voor scenario's waarin de on-premises netwerk en Azure virtuele netwerken ingevoegd als formulier met een VPN-en/of ExpressRoute verbinding. Deze oplossing ondersteunt ook bidirectionele replicatie zodat u wijzigingen aanbrengen in de cloud en on-premises, waar u het meest geschikt is. Afhankelijk van uw beveiligingsvereisten die zijn zijn de AD-installatie in de cloud:
    - deel van het domein dat aangehouden on-premises implementatie
    - een nieuw domein binnen een gedeelde bos
    - een afzonderlijk bos

<!-- we might want to add info on how to choose from the three options above -->

Deze architectuur is gericht op oplossing 2, met behulp van het dezelfde AD DS-domein in Azure en on-premises.

Veelvoorkomend gebruik dozen voor deze architectuur opnemen:

- Hybride toepassingen waar werkbelasting uitvoeren gedeeltelijk on-premises implementatie en deels in Azure.

- Toepassingen en services die verificatie uitvoeren via Active Directory.

## <a name="architecture-diagram"></a>Architectuurdiagram

In het volgende diagram wordt de belangrijke onderdelen in deze architectuur (*Klik op om in te zoomen*) gemarkeerd. Lees voor meer informatie over de onderdelen die niet beschikbaar bij [uitvoering van een beveiligde hybride netwerkarchitectuur in Azure wordt aangegeven] [ implementing-a-secure-hybrid-network-architecture] en [implementeren van een beveiligde hybride netwerkarchitectuur met internettoegang in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **On-premises netwerk.** De on-premises netwerk bevat lokale AD-servers die verificatie en machtiging voor onderdelen bevinden on-premises implementatie kunnen uitvoeren.

- **AD-Servers.** Hierna ziet u domeincontrollers implementatie van directoryservices (AD DS) wordt uitgevoerd met VMs in de cloud. Deze servers kunnen de verificatie van onderdelen die in uw Azure virtuele netwerk uitgevoerd bieden.

- **Active Directory-subnet.** De AD DS-servers worden gehost in een afzonderlijk subnet. Regels voor NSG de AD DS-servers beveiligen en een firewall tegen verkeer uit onverwachte bronnen kunnen bevatten.

- **Azure Gateway en AD-synchronisatie.**. De gateway Azure biedt een verbinding tussen de on-premises netwerk en het VNet Azure. Dit is een [VPN-verbinding] [ azure-vpn-gateway] of [Azure ExpressRoute][azure-expressroute]. Alle synchronisatieaanvragen tussen de AD-servers in de cloud en on-premises verloopt via de gateway. Door gebruiker gedefinieerd routes (UDRs) verwerken omleiding voor de synchronisatie-verkeer die rechtstreeks naar de AD-server in de cloud worden doorgegeven en niet langs de bestaande netwerk virtuele toestellen (NVAs) gebruikt in dit scenario.

    Zie voor meer informatie over het configureren van UDRs en de NVAs [uitvoering van een beveiligde hybride netwerkarchitectuur in Azure wordt aangegeven][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Aanbevelingen

In deze sectie bevat een overzicht van de aanbevelingen voor de uitvoering van AD DS in Azure wordt aangegeven, die betrekking hebben op:

- VM aanbevelingen.

- Aanbevelingen voor netwerken.

- Aanbevelingen voor beveiliging. 

- Aanbevelingen voor Active Directory-Site.

- Active Directory FSMO plaatsing aanbevelingen.

>[AZURE.NOTE] Het document [richtlijnen voor het implementeren van Windows Server Active Directory op Azure virtuele Machines] [ ad-azure-guidelines] bevat meer gedetailleerde informatie over veel van de volgende punten.

### <a name="vm-recommendations"></a>VM aanbevelingen

Maak VMs met voldoende resources worden afgehandeld van het verwachte volume verkeer. De grootte van de machines hostingprovider AD DS on-premises als uitgangspunt gebruiken. Monitor het Resourcegebruik; u kunt het formaat van de VMs en verkleinen als ze te groot is zijn. Zie voor meer informatie over het formaat wijzigen van AD DS-domeincontrollers, [Capaciteit, Planning voor Active Directory Domain Services][capacity-planning-for-adds].

Een schijf virtuele gegevens apart voor het opslaan van de database, logboeken en SYSVOL voor AD maken. Sla deze items niet op dezelfde schijf als het besturingssysteem. Houd er rekening mee dat gegevensschijven die zijn gekoppeld aan een VM standaard schrijven van caching gebruiken. Dit formulier cache kan echter conflicteren met de vereisten van AD DS. Stel de instelling *Host Cache Taalvoorkeur* op de gegevensschijf op *geen*daarom. Zie voor meer informatie, [plaatsing van de Windows Server AD DS-database en SYSVOL][adds-data-disks].

Ten minste twee VMs AD DS als domeincontroller uitvoert met uw Azure virtuele netwerk vanwege de [beschikbaarheid van](#Availability-considerations) implementeren.

### <a name="networking-recommendations"></a>Netwerken aanbevelingen

Configureer de netwerkinterface voor elk van de AD DS met statische IP-adressen in persoonlijke hostingprovider VMs. Deze configuratie ondersteunt beter DNS op elk van de AD-VMs. Zie voor meer informatie, [het instellen van een statische privé IP-adres in de portal van Azure][set-a-static-ip-address].

> [AZURE.NOTE] Geef de AD DS VMs openbare IP-adressen niet. Zie [beveiligingsoverwegingen] [ security-considerations] voor meer informatie.

Moet u ervoor zorgen dat verkeer vrij kunt flow tussen de AD-servers in de cloud en on-premises implementatie:

- NSG regels toevoegen aan de AD-subnetten die binnenkomende verkeer vanuit on-premises toestaan. Zie [Active Directory en de Active Directory Domain Services-Poortvereisten]voor gedetailleerde informatie over de poorten die gebruikmaken van AD DS,[ad-ds-ports].

- Zorg ervoor dat UDR tabellen AD DS-verkeer via de NVAs gebruikt in dit scenario niet doorsturen. Voor uw eigen implementaties, afhankelijk van welke NVAs die u gebruikt, wilt u mogelijk dat verkeer omleiden.

### <a name="security-recommendations"></a>Aanbevelingen voor beveiliging

AD DS servers verwerken verificatie en worden daarom zeer gevoelige items. Voorkomen dat de AD DS-servers directe blootgesteld aan Internet. Plaats de AD DS-servers in een afzonderlijk subnet, met een eigen firewall. Zorg ervoor dat de benodigde AD DS gebruiken voor verificatie en autorisatie en de synchronzing servers poorten open blijven, maar alle andere poorten sluiten. Zie voor meer informatie, [Active Directory en de Active Directory Domain Services-Poortvereisten][ad-ds-ports]. De [onderdelen van de oplossing] [ solution-components] in de sectie verderop in dit document, ziet de NSG regels die in de steekproef-oplossing worden gebruikt om poorten te openen.

U kunt NSG regels gebruiken om te maken van een eenvoudige firewall. Als u meer uitgebreide bescherming vereist kunt u de omtrek van een extra beveiliging rond servers implementeren met behulp van een paar subnetten en NVAs, zoals wordt beschreven door het [implementeren van een beveiligde hybride netwerkarchitectuur met internettoegang in Azure]document[implementing-a-secure-hybrid-network-architecture-with-internet-access].

-Versleuteling of BitLocker Azure schijf de schijf waarop de AD DS-database versleutelen.

### <a name="active-directory-site-recommendations"></a>Aanbevelingen voor Active Directory-Site

U kunt sites in AD DS groep bijeenhouden domein controller die zijn verbonden via een snelle koppeling gebruiken. Domeincontrollers in dezelfde AD DS site gerepliceerd hun directorywijzigingen automatisch en weinig configuratie is vereist voor het verwerken van de replicatie.

Het wordt aanbevolen om te bepalen herhaling verkeer tussen Azure en uw on-premises implementatie-datacenter, een afzonderlijke AD DS-site voor de adresruimte die worden gebruikt door Azure toevoegen. U kunt een koppeling naar site configureren tussen uw on-premises AD DS-sites en meerdere sites replicatie efficiënter beheren.

U kunt ook de scheiding site gebruiken om toe te passen verschillende GPO (GPO) computers gecombineerd in Azure wordt aangegeven, en om te profiteren van toepassingen die 'site op de hoogte', zoals System Center Configuration Manager zijn.

### <a name="active-directory-fsmo-placement-recommendations"></a>Active Directory FSMO plaatsing aanbevelingen

Flexibele Single outmodel bewerking (FSMO)-servers zijn gespecialiseerde domeincontroller, reposnsible voor consistentie van de gegevens over de verschillende instellingen in AD DS onderstaande.

- **Schema basispagina**. Dit is een bos hele rol die de structuur van het schema die worden gebruikt door de AD DS onderhoudt.

- **Domein Naming basispagina**. Dit is een bos hele rol die worden beheerd met informatie over domeinnamen binnen de bos.

- **Primaire domeincontroller (primaire domeincontroller)**. Dit is een rol hele domein. De primaire domeincontroller verwerkt wachtwoord updates en geblokkeerde accounts. Deze wordt door andere DCs geraadpleegd wanneer verificatie serviceaanvragen verkeerde wachtwoorden hebt. De primaire domeincontroller ook omgaat met Groepsbeleid updates en is het doel domeincontroller voor oudere toepassingen en sommige beheerprogramma's die sommige schrijfbare bewerkingen uitvoeren.

- **Relatieve-ID (RID) basispagina**. RID's worden gebruikt om u te helpen objecten in een map worden geïdentificeerd. Deze server is verantwoordelijk voor het genereren van een groep gehele domein RID's voor gebruik door alle AD-servers binnen het domein.

- **De functie infrastructuur**. Een object in een domein kunt verwijzingen maken naar een object in een ander domein. Deze functie hele domein is verantwoordelijk voor het bijwerken van een object beveiligings-id en DN-naam in de verwijzing naar een ander domein object. Houd er rekening mee dat een server uitvoering van deze rol ook kan niet als een globale catalogus ° (c)-server fungeren.

Zie voor meer informatie, [Active Directory-FSMO-rollen in Windows][AD-FSMO-roles-in-windows].

In dit scenario wordt aangeraden dat u vermijden FSMO-functies implementeert de domein-controller in Azure wordt aangegeven. 

## <a name="availability-considerations"></a>Aandachtspunten voor de beschikbaarheid

Maak een beschikbaarheid instellen voor de AD-servers. Zorg ervoor dat er ten minste twee servers in de set. De AD-servers in de cloud moet domeincontrollers binnen hetzelfde domein. Hiermee schakelt u automatische replicatie tussen servers.

U kunt wellicht ook servers aanwijzen als [stand-by operations outmodellen] [ standby-operations-masters] geval connectiviteit met een server die fungeert als een FSMO-functie is mislukt.

## <a name="security-considerations"></a>Veiligheidsoverwegingen

Als alle beheerdersbewerkingen van het domein waarschijnlijk zijn moet worden uitgevoerd met de DCs on-premises implementatie, kunt u DCs maken in de cloud alleen-lezen. Een alleen-lezen domeincontroller wordt alleen onderhoudt een subset van de referenties van de gebruikers (genoeg verificatie lokaal uitvoeren) en kan worden geconfigureerd naar cache informatie alleen voor specifieke gebruikers. Daarom als een alleen-lezen domeincontroller betrouwbaar is, heeft alleen de informatie voor gebruikers in de cache gevolgen, in plaats van de details van elke account in het domein. Zie voor meer informatie [Alleen-lezen domeincontrollers][read-only-dc].

Ga als volgt aanbevolen procedure voor het instellen en onderhouden van wachtwoorden van gebruikers in AD om te helpen de beveiligingsprobleem van afzonderlijke gebruikersaccounts minimaliseren, en pogingen naar inbraak ontmoedigen. Zie [Aanbevolen procedures voor het wachtwoordbeleid afdwingen]voor meer informatie,[best_practices_ad_password_policy]. Ook zijn dat u daarbij welke groepen u toewijzen aan gebruikers. Bijvoorbeeld gewone gebruikers lid van de groep beheerders van de onderneming, groep Schema-beheerders en groep Domain Admins niet maken.

## <a name="scalability-considerations"></a>Aandachtspunten voor de schaalbaarheid

Er kan AD worden automatisch aangepast voor domeincontrollers die deel van hetzelfde domein uitmaken. Aanvragen zijn verdeeld over elke afzonderlijk binnen een domein. U kunt een andere domeincontroller toevoegen en deze automatisch worden gesynchroniseerd met het domein. Een afzonderlijk taakverdeling om verkeer controller binnen het domein niet configureert. Zorgen dat alle domeincontrollers onvoldoende geheugen en opslag resources worden afgehandeld de domeindatabase. in het ideale geval alle domeincontroller VMs-even groot maken.

## <a name="management-considerations"></a>Overwegingen bij

Kopieer de VHD-bestanden van domeincontrollers in plaats van de uitvoering van regelmatige back-ups omdat deze terug te zetten leiden inconsistenties in staat tussen domeincontrollers tot kan niet.

Afsluiten en opnieuw opstarten een VM die wordt uitgevoerd als functie van de domeincontroller in Azure wordt aangegeven in het gastbesturingssysteem in plaats van met de optie Afsluiten in de Portal Azure. Met behulp van de Azure-Portal een VM afsluiten zorgt ervoor dat de VM om te worden opgeheven. Deze actie wordt de VM-GenerationID, welke ongewenste voor een domeincontroller is hersteld. Wanneer de VM-GenerationID opnieuw is ingesteld, wordt ook opnieuw in de invocationID van de AD-bibliotheek, de RID-groep wordt verwijderd en SYSVOL is gemarkeerd als niet-gezaghebbende.

## <a name="monitoring-considerations"></a>Overwegingen voor controle

Verbroken hoe een netwerk van AD-servers worden bewaakt en kan leiden tot problemen, zoals:

- **Aanmeldingsfout**. Aanmeldingsfout kan overal in het domein of bos optreden als de resolutie van een vertrouwen relatie of naam mislukt of als een globale catalogus-server universele groepslidmaatschap kan niet bepalen.

- **Accountvergrendeling**. Accounts van gebruikers en -service kunnen worden uitgesloten als de primaire domeincontroller niet beschikbaar is of replicatie tussen verschillende domeincontrollers mislukt.

- **Domeincontroller is mislukt**. Als het station met het bestand Ntds.dit wordt voldoende schijfruimte uitgevoerd, kan de domeincontroller kan niet meer.

- **Toepassing is mislukt**. Toepassingen die essentieel voor uw bedrijf, zoals Microsoft Exchange of een ander e-mailtoepassing zijn kunnen mislukken als adres adresboek query's naar de map is mislukt.

- **Inconsistente directory-gegevens**. Als replicatie voor een langere periode mislukt, wordt dubbele objecten kunnen worden gemaakt in de adreslijst en kunnen het nodig zijn de uitgebreide diagnose en de tijd om te voorkomen.

- **Account maken is mislukt**. Een domeincontroller is kan gebruikers- of computeraccounts maken als deze verstrekt relatieve id's zaken en de hoofdlijst RID niet beschikbaar is.

- **Beveiliging beleid is mislukt**. Als de gedeelde map SYSVOL niet correct gerepliceerd, worden Groepsbeleid objecten en beveiligingsbeleid voor apparaten niet goed toegepast op clients.

AD-servers zorgvuldig bewaken en zorg ervoor dat snel corrigerende maatregelen nemen. Maak een controlelijst met taken die moeten worden uitgevoerd met een juiste interval voor controle. U kunt bijvoorbeeld de volgende kritieke taken dagelijks plannen:

- Onderzoeken en waarschuwingen gemeld door domeincontroller, oplossen 

- Controleer of alle domeincontrollers kunnen communiceren en replicatie werkt.

- Zorg ervoor dat SYSVOL gedeelde blijft.

- Tijd-synchronisatieproblemen corrigeren.

- Controleren op schijfruimte op de fysieke stations die worden gebruikt door AD.

U kunt andere steeds terugkerende taken minder vaak kan uitvoeren. U kan bijvoorbeeld vertrouwensrelaties bekijken en controleren op verouderde of verbroken vertrouwensrelaties wekelijkse en controleer of alle domeincontrollers worden uitgevoerd met de dezelfde servicepacks en warm fix patches maandelijks.

Zie voor meer informatie [Monitoring Active Directory][monitoring_ad]. U kunt hulpprogramma's zoals [Microsoft Systems Center] installeren[ microsoft_systems_center] op de monitoring server (Zie de [Architectuurdiagram][architecture]) zodat deze taken uitvoeren. 

## <a name="solution-components"></a>Onderdelen van oplossingen

De oplossing, mits voor deze architectuur Hiermee maakt u een netwerk met een beveiligde hybride volgens de beschrijving van de [uitvoering van een beveiligde hybride netwerkarchitectuur in Azure wordt aangegeven] documenten[ implementing-a-secure-hybrid-network-architecture] en [implementeren van een beveiligde hybride netwerkarchitectuur met internettoegang in Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access], maar met de toevoeging van de volgende items:

- Een Azure resourcegroep benoemde *basename*- dns-rg, waar *basename* een voorvoegsel is opgegeven bij het distribueren van de oplossing.

- Twee Azure VMs *basename*genoemd-ad1-vm en *basename*-ad2-vm, die is gemaakt in de groep *basename*- dns-rg resource. Deze VMs zijn geconfigureerd als AD-servers met adreslijstservices en DNS geïnstalleerd en geconfigureerd.

- Een NSG benoemde *basename*-ad-nsg in de groep *basename*- ntwk-rg Azure resource. Deze resourcegroep maakt deel uit van de infrastructuur die het netwerk secure hybride vormen, maar de nieuwe NSG is een aanvulling waarmee wordt gedefinieerd inkomende beveiligingsregels voor de AD-servers zoals wordt weergegeven in de volgende tabel:


    Prioriteit|Naam|Bron|Bestemming|Service|Actie|
    --------|----|------|-----------|-------|------|
    170|vnet-naar-port53|10.0.0.0/16|Een|Custom(ANY/53)|Toestaan|
    180|vnet-naar-port88|10.0.0.0/16|Een|Custom(ANY/88)|Toestaan|
    190|vnet-naar-port135|10.0.0.0/16|Een|Custom(ANY/135)|Toestaan|
    200|vnet-naar-port137-9|10.0.0.0/16|Een|Custom(ANY/137-139)|Toestaan|
    210|vnet-naar-port389|10.0.0.0/16|Een|Custom(ANY/389)|Toestaan|
    220|vnet-naar-port464|10.0.0.0/16|Een|Custom(ANY/464)|Toestaan|
    230|vnet-naar-rpc-dynamische|10.0.0.0/16|Een|Custom(ANY/49152-65535)|Toestaan|
    240|onprem-ad-naar-port53|192.168.0.0/24|Een|Custom(ANY/53)|Toestaan|
    250|onprem-ad-naar-port88|192.168.0.0/24|Een|Custom(ANY/88)|Toestaan|
    260|onprem-ad-naar-port135|192.168.0.0/24|Een|Custom(ANY/135)|Toestaan|
    270|onprem-ad-naar-port389|192.168.0.0/24|Een|Custom(ANY/389)|Toestaan|
    280|onprem-ad-naar-port464|192.168.0.0/24|Een|Custom(ANY/464)|Toestaan|
    290|Mgmt-rdp-toestaan|10.0.0.128/25|Een|Custom(ANY/3389)|Toestaan|
    300|gateway-toestaan|10.0.255.224/27|Een|Custom(ANY/ANY)|Toestaan|
    310|Selfservice toestaan|10.0.255.192/27|Een|Custom(ANY/ANY)|Toestaan|
    320|vnet-weigeren|Een|Een|Custom(ANY/ANY)|Toestaan|

    AD DS gebruikt poorten 53, 89 135, 389 en 464 accepteren binnenkomende replicatie en verificatie-verkeer is toegestaan. In deze tabel worden de domeincontroller on-premises implementatie is in het adres ruimte 192.168.0.0/24 (uw adresruimte kan variëren - u deze gegevens opgeven als een parameter aan de sjablonen die zijn geïmplementeerd door de oplossing.

    De NSG definieert ook de volgende beveiligingsregels voor de uitgaande waarmee synchronisatie en autorisatie verkeer terug naar de on-premises netwerk:

    Prioriteit|Naam|Bron|Bestemming|Service|Actie|
    --------|----|------|-----------|-------|------|
    100|out-port53|Een|192.168.0.0/24|Custom(ANY/53)|Toestaan|
    110|out-port88|Een|192.168.0.0/24|Custom(ANY/88)|Toestaan|
    120|out-port135|Een|192.168.0.0/24|Custom(ANY/135)|Toestaan|
    130|out-port389|Een|192.168.0.0/24|Custom(ANY/389)|Toestaan|
    140|out-poort 445 toe|Een|192.168.0.0/24|Custom(ANY/445)|Toestaan|
    150|out-port464|Een|192.168.0.0/24|Custom(ANY/464)|Toestaan|
    160|out-rpc-dynamische|Een|192.168.0.0/24|Custom(ANY/49152-65535)|Toestaan|

Het script voorzien van de oplossing kunt u ook de volgende taken uitvoeren:

- De *basename*worden toegevoegd-ad1-vm en *basename*-ad2-servers vm als domeincontroller op het domein. U kunt deze servers weergeven in de *Active Directory-gebruikers en Computers* console in de domeincontroller on-premises implementatie:

![[1]][1]

- Een nieuwe subnet (10.0.0.0/16) wordt gemaakt voor een AD-site met de naam Azure-VNet-Ad-Site op het domein. Deze site bevat de *basename*-ad1-vm en *basename*-ad2-vm-servers. 

- IP-transport tussen sites instellingen die het replicatie-interval tussen de on-premises implementatie-site en de domeincontrollers in de cloud configureren wordt toegevoegd. U kunt de instellingen voor de subnet, sites en transport-instellingen in de *Active Directory-Sites en Servers* console in de on-premises implementatie domeincontroller zien:

![[2]][2]

## <a name="deployment"></a>Implementatie

De oplossing voorbeeld heeft de volgende prerequsites:

- Verbinden met de gateway Azure VPN u uw on-premises domein al hebt geconfigureerd, en die u hebt geconfigureerd DNS en Routering en RAS services om te ondersteunen, een VPN-verbinding hebt geïnstalleerd.


- U kunt de nieuwste versie van de Azure CLI hebt geïnstalleerd. [Volg deze instructies voor meer informatie][cli-install].

- Als u de oplossing vanuit Windows distribueren bent, moet u een hulpmiddel waarmee een shell we vaker doen, bijvoorbeeld [Het bureaublad GitHub][github-desktop].

>[AZURE.NOTE] Als u geen toegang tot een bestaande on-premises domein, kunt u een testomgeving met de [onpremdeploy.sh] [ onpremdeploy] bash script. Dit script Hiermee maakt u een netwerk- en VM in de cloud die overeenkomt met een zeer eenvoudige on-premises implementatie-installatie. Dit script voordat u bewerken en instellen van de volgende variabelen gedefinieerd aan het begin van het bestand:
>
> - **BASE_NAME**. Een voorvoegsel door gebruiker gedefinieerd voor de resourcegroep en VM die door het script is gemaakt. Dit item zijn **niet meer dan 5 tekens** anders die het script probeert te genereren van een VM met een ongeldige naam en mislukt.
> 
> - **Abonnement**. Uw abonnement op Azure-ID. De resourcegroep wordt gemaakt in deze suscription.
> 
> - **Locatie**. De Azure locatie waarin de resourcegroep, zoals *eastus* of *westus*maken.
> 
> - **ADMIN_USER_NAME**. De naam voor het beheerdersaccount in VM wilt gebruiken.
> 
> - **ADMIN_PASSWORD**. Het wachtwoord voor het beheerdersaccount.

De volgende stappen om te maken van de steekproef-oplossing uitvoeren:

1. Download en bewerken van de [azuredeploy.sh] [ azuredeploy] script en de volgende parameters instellen aan het begin van het bestand:

    - **BASE_NAME**. Een door de gebruiker gedefinieerde voorvoegsel voor de resourcegroepen en VMs die door het script is gemaakt. Als voorheen, moet dit item niet **langer dan 5 tekens**.

    - **Abonnement**. Uw abonnement op Azure-ID.

    - **OS-type**. Het besturingssysteem (*Windows* of *Linux*) gebruiken voor het web en business data access-laag VMs. Houd er rekening mee dat alle AD-servers gemaakt door het script uitvoeren van Windows Server 2012, ongeacht deze instelling.

    - **' Domeinnaam '**. De naam van de on-premises domein. Houd er rekening mee dat als u de omgeving die is gemaakt door het script onpremdeploy.sh gebruikt, dit *contoso.com moet*.

    - **Locatie**. De Azure locatie waarin u de resourcegroepen maken.

    - **ADMIN_USER_NAME**. De naam voor de administrator-accounts in de verschillende VMs wilt gebruiken.

    - **ADMIN_PASSWORD**. Het wachtwoord voor het beheerdersaccount.

    - **ON_PREMISES_PUBLIC_IP**. Het openbare IP-adres van de computer van de VPN on-premises implementatie.

    - **ON_PREMISES_ADDRESS_SPACE**. De interne adresruimte van de on-premises netwerk. Als u de omgeving die is gemaakt door het script onpremdeploy.sh gebruikt, moet dit 192.168.0.0/16.

    - **VPN_IPSEC_SHARED_KEY**. De IPSec gedeelde sleutel die wordt gebruikt voor het maken van de VPN-verbinding tussen de on-premises netwerk en de gateway Azure VPN.

    - **ON_PREMISES_DNS_SERVER_ADDRESS**. Het IP-adres van de lokale DNS-server. Als u de omgeving die is gemaakt door het script onpremdeploy.sh gebruikt, moet dit 192.168.0.4

    - **ON_PREMISES_DNS_SUBNET_PREFIX** Het adresvoorvoegsel van de on-premises implementatie subnet. Als u de omgeving die is gemaakt door het script onpremdeploy.sh gebruikt, moet dit 192.168.0.0/24.

    >[AZURE.NOTE] Het script maakt om op te slaan resources en de tijd, geen de access-lagen en grote ondernemingen of gegevens. Als u deze items vereist, kunt u de volgende sectie in het script azuredeploy.sh opmerkingen bij:
    >
    >
    > ```
    > #### # create biz tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_BIZ_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${BIZ_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${BIZ_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_BIZ_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > #### 
    > #### # create db tier
    > #### TEMPLATE_URI=${URI_BASE}/ARMBuildingBlocks/Templates/bb-ilb-backend-http-https.json
    > #### SUBNET_NAME_PREFIX=${DEPLOYED_DB_SUBNET_NAME_PREFIX}
    > #### ILB_IP_ADDRESS=${DB_ILB_IP_ADDRESS}
    > #### NUMBER_VMS=${DB_NUMBER_VMS}
    > #### 
    > #### RESOURCE_GROUP=${BASE_NAME}-${SUBNET_NAME_PREFIX}-tier-rg
    > #### VM_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VM_COMPUTER_NAME_PREFIX=${SUBNET_NAME_PREFIX}
    > #### VNET_RESOURCE_GROUP=${NTWK_RESOURCE_GROUP}
    > #### VNET_NAME=${DEPLOYED_VNET_NAME}
    > #### SUBNET_NAME=${DEPLOYED_DB_SUBNET_NAME}
    > #### PARAMETERS="{\"baseName\":{\"value\":\"${BASE_NAME}\"},\"vnetResourceGroup\":{\"value\":\"${VNET_RESOURCE_GROUP}\"},\"vnetName\":{\"value\":\"${VNET_NAME}\"},\"subnetName\":{\"value\":\"${SUBNET_NAME}\"},\"adminUsername\":{\"value\":\"${ADMIN_USER_NAME}\"},\"adminPassword\":{\"value\":\"${ADMIN_PASSWORD}\"},\"subnetNamePrefix\":{\"value\":\"${SUBNET_NAME_PREFIX}\"},\"ilbIpAddress\":{\"value\":\"${ILB_IP_ADDRESS}\"},\"osType\":{\"value\":\"${OS_TYPE}\"},\"numberVMs\":{\"value\":${NUMBER_VMS}},\"vmNamePrefix\":{\"value\":\"${VM_NAME_PREFIX}\"},\"vmComputerNamePrefix\":{\"value\":\"${VM_COMPUTER_NAME_PREFIX}\"}}"
    > #### 
    > #### echo
    > #### echo
    > #### echo azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > ####      azure group create --name ${RESOURCE_GROUP} --location ${LOCATION} --subscription ${SUBSCRIPTION}
    > #### echo
    > #### echo
    > #### echo azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ####      azure group deployment create --template-uri ${TEMPLATE_URI} -g ${RESOURCE_GROUP} -p ${PARAMETERS} --subscription ${SUBSCRIPTION}
    > ```

2. Open de opdrachtprompt shell we vaker doen en verplaatsen naar de map met het script azuredeploy.sh.

3. Meld u aan bij uw Azure-account. Voer de volgende opdracht in de shell we vaker doen:

    ```
    azure login
    ```

    Volg de instructies in verbinding maken met Azure.

4. De opdracht uitvoeren `./azuredeploy.sh`, en wacht terwijl het script wordt gemaakt voor de netwerkinfrastructuur.

5. Klik bij de prompt *Controleer of dat de VNet is gemaakt*door de Azure-portal te gebruiken om te controleren dat een resourcegroep *basename*- ntwk-rg met de naam is gemaakt en dat deze items die overeenkomen met die wordt weergegeven in de volgende afbeelding bevat:

    ![[3]][3]

    >[AZURE.NOTE] *Basename* is ingesteld op *cloud* in de voorbeelden wordt weergegeven, wanneer het script is uitgevoerd.

    Klik op de *basename*- vnet VNet op *subnetten*en controleer of dat de subnetten die hieronder zijn gemaakt:

    ![[4]][4]

6. Klik bij de prompt in het venster van de shell we vaker doen op een toets drukt en wacht tot de weblaag en taakverdeling worden gemaakt.

7. Klik bij de prompt *Controleer of dat de laag Web juist is gemaakt*door de Azure-portal te gebruiken om te controleren dat een resourcegroep *basename*web-laag-rg genoemd is gemaakt en items die vergelijkbaar met de onderstaande tabel bevat:

    ![[5]][5]

8. Klik bij de prompt in het venster van de shell we vaker doen op een toets drukt en wacht tot de NVAs worden gemaakt.

9. Klik bij de prompt *Controleer of dat de NVA juist is gemaakt*door de Azure-portal te gebruiken om te controleren dat een resourcegroep *basename*- mgmt-rg genoemd is gemaakt met de volgende onderwerpen:

    ![[6]][6]

10. Klik bij de prompt in het venster van de shell we vaker doen op een toets drukt en wacht tot de jumpbox wordt gemaakt.

11. Klik bij de prompt *Controleer of dat de jumpbox juist is gemaakt*door de Azure-portal te gebruiken om te controleren dat de volgende items zijn toegevoegd aan de resourcegroep *basename*- mgmt-rg:

    - Een set beschikbaarheid genoemd *basename*- jb-als.

    - Een VM benoemde *basename*- jb-vm.

    - De netwerkinterface van een genoemd *basename*- jb-NIC

12. Klik bij de prompt in het venster van de shell we vaker doen op een toets drukt en wacht tot de Azure VPN gateway en de verbinding worden gemaakt. Houd er rekening mee dat deze stap duurt maximaal 30 minuten duren.

13. Klik bij de prompt *Controleer of dat de gateway VPN juist is gemaakt*door de Azure-portal te gebruiken om te controleren dat de volgende items zijn toegevoegd aan de resourcegroep *basename*- ntwk-rg:

    - Een gateway lokale netwerk op in de lokale lgw genoemd.
    
    - Een gateway virtueel netwerk *basename*- vpngw genoemd.

    - Een gateway-verbinding met de naam *basename*- vnet-vpnconn. Houd er rekening mee dat de status van deze verbinding mogelijk *niet verbonden* als u het einde van de on-premises implementatie van de verbinding; nog niet hebt geconfigureerd u gaat dit later.

14. Klik bij de prompt in het venster van de shell we vaker doen op een toets drukt en wacht tot de VMs en andere bronnen voor de DMZ worden gemaakt.

15. Klik bij de prompt *Controleer of dat de DMZ juist is gemaakt*door de Azure-portal te gebruiken om te controleren dat een resourcegroep *basename*-dmz-rg genoemd is gemaakt met de volgende onderwerpen:

    ![[7]][7]

16. Druk op een toets bij de prompt in het venster van de shell we vaker doen. De volgende vragen moeten worden weergegeven:

    ```text
    Manual Step...

    Please configure your on-premises network to connect to the Azure VNet

    Make sure that you can connect to the on-premises AD server from the Azure VMs
    ```

    Meld u aan bij uw lokale computer die is verbonden met de Azure gateway en de verbinding correct configureren. Statische routes toevoegen aan de on-premises implementatie gateway-apparaat, dat wordt u omgeleid requestsfor het bereik voor het adres van 10.0.0.0/16 tot en met de gateway bij naar de VNet zodat. De stappen hiervoor zijn afhankelijk van hoe u verbinding maakt. Zie [een hybride netwerkarchitectuur met Azure en On-premises VPN implementeren] [ implementing-a-hybrid-network-architecture-with-vpn] voor meer informatie.

    Houd er rekening mee dat u het openbare IP-adres van de gateway Azure VPN vinden kunt met behulp van de Azure-portal om te bekijken van de gateway *basename*- vpngw in de groep *basename*- ntwk-rg resource:

    ![[8]][8]

    U kunt bepalen of de verbinding correct is gemaakt door te zoeken bij de status van de *basename*- vnet-vpnconn verbinding. Deze moet worden ingesteld op *verbonden*.

    Open de om verbinding te testen, een verbinding met extern bureaublad naar de jumpbox (10.0.0.254 beschikbaar) van een computer die zich in uw on-premises netwerk.

17. Druk op een toets bij de prompt in het venster van de shell we vaker doen. Aan de volgende vragen, *Druk op een toets bij de instelling VNet voor de VNet zodat deze verwijzen naar de on-premises implementatie DNS*, op een toets drukt en wacht tot de DNS-instellingen voor de VNet zijn bijgewerkt naar de waarde die u hebt opgegeven als de parameter **ON_PREMISES_DNS_SERVER_ADDRESS** in het script azuredeploy.sh.

18. Klik bij de prompt *Controleer of dat de DNS-server-instelling op het VNet is bijgewerkt*, de Azure-portal te gebruiken om te bekijken van de instelling van de *DNS-servers* van de *basename*- vnet VNet in de groep *basename*- ntwk-rg resource. Deze moet worden ingesteld op *Aangepaste DNS*en de *primaire DNS server* het adres van uw DNS-server on-premises implementatie moet zijn:

    ![[9]][9]

19. Klik bij de prompt *op een toets te maken van de resourcegroep voor de AD-servers* in het venster van de shell we vaker doen op een toets drukt en wacht tot de resourcegroep voor het vasthouden van de AD-servers in de cloud wordt gemaakt.

20. Klik bij de prompt *op een toets te maken van de VMs voor de AD-servers* in het venster van de shell we vaker doen op een toets drukt en wacht totdat de VMs worden gemaakt en toegevoegd aan de resourcegroep.

21. Wanneer de *Druk op een toets deel te nemen aan de VMs naar de on-premises domein* wordt weergegeven, gaat u naar de Azure-portal en controleer of dat de groep *basename*- dns-rg is gemaakt en dat deze twee VMS bevat (*basename*-ad1-vm en *basename*-ad2-vm):

    ![[10]][10]

22. In de groep *basename*- ntwk-rg resource controleren dat een NSG is gemaakt *basename*genoemd-ad-nsg. Bekijk de regels voor binnenkomende en uitgaande voor deze NSG. Ze moeten overeenkomen met die wordt vermeld in de tabellen in de [onderdelen van de oplossing] [ solution-components] sectie.

23. Klik bij de prompt in het venster van de shell we vaker doen op een toets drukt en wacht tot de VMs worden toegevoegd aan de on-premises domein.

24. Aan de *gaat u naar de lokale Active Directory-server om te bevestigen dat de computers zijn toegevoegd aan de domeinen* wordt gevraagd, verbinding maken met uw lokale computer en de *Active Directory-gebruikers en Computers* -console gebruiken om te controleren dat beide VMs zijn toegevoegd aan het domein:

    ![[11]][11]

25. Klik bij de prompt in het venster van de shell we vaker doen op een toets drukt en wacht tot de site van AD replicatie wordt gemaakt in het domein.

26. Aan de *gaat u naar de lokale Active Directory-server om te bevestigen dat de site replicatie is gemaakt* wordt gevraagd, gebruikt u de console *Active Directory-Sites en Services* om te controleren dat een replicatie-site met de naam *Azure-Vnet-Ad-Site* is gemaakt, samen met een IP-transport tussen sites-koppeling genoemd *AzureToOnpremLink*en een subnet dat verwijst naar de VNet:

    ![[12]][12]

27. Klik bij de prompt in het venster van de shell we vaker doen op een toets drukt en het script installeert adreslijstservices en DNS op elk van de AD-VMs.

28. Wanneer de prompt *oplossing Meld u aan bij elke Azure AD-server om te bevestigen dat Directory Services is geconfigureerd* wordt weergegeven, een verbinding met extern bureaublad openen vanuit een on-premises implementatie-computer naar de jumpbox (*basename*- jb-vm) en vervolgens een andere verbinding met extern bureaublad openen vanuit de jumpbox naar de eerste AD-server (*basename*-ad1-vm). Meld u aan met de **domeinnaam**, **ADMIN_USER_NAME**en **ADMIN_PASSWORD** die u hebt opgegeven in het script azuredeploy.sh. Controleer met Serverbeheer, of dat de AD DS en DNS rollen beide zijn toegevoegd. Herhaal deze procedure voor het tweede AD-server (*basename*-ad2-vm).

29. Druk op een toets bij de prompt in het venster van de shell we vaker doen. Wanneer het bericht *Druk op een toets om in te stellen van de Azure VNet-DNS-instellingen te laten verwijzen naar de DNS-records in Azure wordt aangegeven* wordt weergegeven op een toets drukt en het script bij de DNS-instellingen voor de VNet toestaan.

30. Wanneer de prompt *Controleer of dat de VNet DNS instelling is bijgewerkt verwijzing de DNS-records van Azure VM servers* wordt weergegeven, wordt u met behulp van de Azure portal controleren de instelling van de *DNS-Servers* van de *basename*- vnet VNet in de groep *basename*- ntwk-rg resource. De primaire en secundaire DNS-servers moeten nu verwijzen naar de twee AD-VMs:

    ![[13]][13]

31. Start opnieuw op elk van de AD-VMs voordat u verdergaat. Deze stap is nodig om ervoor te zorgen dat ze elke verdergaan de juiste DNS-instellingen van Azure. Wacht totdat beide VMs worden uitgevoerd voordat u verdergaat.

32. Druk op een toets bij de prompt in het venster van de shell we vaker doen. Klik bij de volgende prompt, *Druk op een toets om toe te passen van de gateway UDR subnet, de gateway (deze is mogelijk verwijderd)*, op een toets drukt en het script te vernieuwen van de gateway UDR toestaan.

33. Controleer of het script is voltooid.

## <a name="next-steps"></a>Volgende stappen

- Informatie over de aanbevolen procedures voor het [maken van een Hiermee resource bos] [ adds-resource-forest] in Azure wordt aangegeven.

- Informatie over de aanbevolen procedures voor het [maken van een ADFS-infrastructuur] [ adfs] in Azure wordt aangegeven.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[architecture]: #architecture_diagram
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[capacity-planning-for-adds]: http://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx
[ad-ds-ports]: https://technet.microsoft.com/library/dd772723(v=ws.11).aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048(v=ws.11).aspx
[powershell-ad]: https://technet.microsoft.com/en-us/library/ee617195.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[cli-install]: https://azure.microsoft.com/documentation/articles/xplat-cli-install
[github-desktop]: https://desktop.github.com/
[sssd-and-active-directory]: https://help.ubuntu.com/lts/serverguide/sssd-ad.html
[set-a-static-ip-address]: https://azure.microsoft.com/documentation/articles/virtual-networks-static-private-ip-arm-pportal/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[adds-data-disks]: https://msdn.microsoft.com/library/azure/jj156090.aspx#BKMK_PlaceDB
[AD-FSMO-recommendations]: #active_directory_FSMO_placement_recommendations
[AD-FSMO-roles-in-windows]: https://support.microsoft.com/en-gb/kb/197132
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[transfer-FSMO-roles]: https://technet.microsoft.com/library/cc816946(v=ws.10).aspx
[view-fsmo-roles]: https://technet.microsoft.com/library/cc816893(v=ws.10).aspx
[read-only-dc]: https://technet.microsoft.com/library/cc732801(v=ws.10).aspx
[AD-sites-and-subnets]: https://blogs.technet.microsoft.com/canitpro/2015/03/03/step-by-step-setting-up-active-directory-sites-subnets-site-links/
[sites-overview]: https://technet.microsoft.com/library/cc782048(v=ws.10).aspx
[implementing-adfs]: ./guidance-iaas-ra-secure-vnet-adfs.md
[onpremdeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/onpremdeploy.sh
[azuredeploy]: https://github.com/mspnp/blueprints/blob/master/ARMBuildingBlocks/guidance-iaas-ra-ad-extension/azuredeploy.sh
[solution-components]: #solution_components

[0]: ./media/guidance-iaas-ra-secure-vnet-ad/figure1.png "Secure hybride netwerkarchitectuur met Active Directory"
[1]: ./media/guidance-iaas-ra-secure-vnet-ad/figure2.png "De vermelding van de twee Azure VMs als servers console van Active Directory-gebruikers en Computers"
[2]: ./media/guidance-iaas-ra-secure-vnet-ad/figure3.png "Het Active Directory-Sites en Services-console met de replicatie-instellingen voor de site in de cloud"
[3]: ./media/guidance-iaas-ra-secure-vnet-ad/figure4.png "De inhoud van de basename-ntwk-rg resourcegroep"
[4]: ./media/guidance-iaas-ra-secure-vnet-ad/figure5.png "De subnetten die in de VNet basename-vnet"
[5]: ./media/guidance-iaas-ra-secure-vnet-ad/figure6.png "De items in de weblaag"
[6]: ./media/guidance-iaas-ra-secure-vnet-ad/figure7.png "De NVAs in de groep basename-mgmt-rg resource"
[7]: ./media/guidance-iaas-ra-secure-vnet-ad/figure8.png "De resources in de groep basename-dmz-rg resource"
[8]: ./media/guidance-iaas-ra-secure-vnet-ad/figure9.png "Zoeken naar het openbare IP-adres van de gateway Azure VPN"
[9]: ./media/guidance-iaas-ra-secure-vnet-ad/figure10.png "De DNS-server-instellingen voor de * basename *-vnet VNet"
[10]: ./media/guidance-iaas-ra-secure-vnet-ad/figure11.png "De * basename *-dns-rg resourcegroep waarin de AD-server VMs"
[11]: ./media/guidance-iaas-ra-secure-vnet-ad/figure12.png "De vermelding van de AD-server VMs als leden van het domein console van Active Directory-gebruikers en Computers"
[12]: ./media/guidance-iaas-ra-secure-vnet-ad/figure13.png "Het Active Directory-Sites en Services-console met de replicatie-site voor de Azure AD-servers"
[13]: ./media/guidance-iaas-ra-secure-vnet-ad/figure14.png "De DNS-server-instellingen voor de basename-vnet VNet verwijst naar de AD-servers in de cloud"