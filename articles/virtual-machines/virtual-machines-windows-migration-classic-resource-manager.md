<properties
    pageTitle="Platform ondersteund migratie van IaaS resources uit klassieke naar Azure resourcemanager | Microsoft Azure"
    description="In dit artikel begeleidt bij de migratie platform ondersteund van resources van klassiek naar Azure resourcemanager"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="singhkays"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kasing"/>

# <a name="platform-supported-migration-of-iaas-resources-from-classic-to-azure-resource-manager"></a>Platform ondersteund migratie van IaaS resources uit klassieke naar Azure resourcemanager

In dit artikel wordt beschreven hoe we de migratie van infrastructuur als een (IaaS) serviceresources uit het klassieke naar resourcemanager implementatiemodellen bent inschakelen. U vindt meer informatie over [resourcemanager Azure-functies en voordelen](../azure-resource-manager/resource-group-overview.md). We Detailstijlen het koppelen van resources uit de twee implementatiemodellen die naast elkaar in uw abonnement gebruiken via virtuele netwerk naar website gateways. 

## <a name="goal-for-migration"></a>Doel voor migratie

Resourcemanager kunt implementeren van complexe toepassingen tot en met sjablonen, virtuele machines configureert met behulp van VM extensions en implementeren van toegangsbeheer en labels. Azure resourcemanager bevat scalable, parallelle implementatie voor virtuele machines in beschikbaarheid sets. Het nieuwe implementatie-objectmodel biedt ook beheer van productlevenscyclus van berekeningscluster, netwerk en opslag onafhankelijk. Er is ten slotte een focus over het inschakelen van beveiliging al dan niet standaard met het afdwingen van virtuele machines in een virtueel netwerk.

Vrijwel alle functies uit het implementatiemodel klassieke worden ondersteund voor berekeningscluster-, netwerk- en opslag onder Azure resourcemanager. Om te profiteren van de nieuwe mogelijkheden in Azure resourcemanager, kunt u bestaande implementaties van het model Klassiek implementatie migreren.

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Wijzigingen in uw automatisering en uitrusting na de migratie

Als onderdeel van het migreren van uw resources van het model Klassiek implementatie aan het implementatiemodel resourcemanager, moet u uw bestaande automatisering of tooling om ervoor te zorgen dat deze nog steeds werkt na de migratie bijwerken.

## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>Betekenis van de migratie van IaaS resources uit klassieke naar resourcemanager

Voordat we op de details inzoomen, sluit u het verschil tussen gegevens-vlak en management-vlak bewerkingen op de resources IaaS nu behandeld.

- *Vlak van Management* worden de oproepen die in het vlak van management of de API voor het wijzigen van resources kunnen komen. Bijvoorbeeld beheren bewerkingen, zoals een VM maken, opnieuw starten van een VM en bijwerken van een virtueel netwerk met een nieuwe subnet de lopende bronnen. Ze niet rechtstreeks van invloed zijn op verbinding maken met de exemplaren.
- *Gegevens vlak* (toepassing) de runtime van de toepassing zelf beschreven en heeft betrekking op interactie met exemplaren die niet door de API Azure. Toegang tot uw website of bepaalde gegevens uit een actieve SQL Server-instantie of een server MongoDB kan worden beschouwd als gegevens luchtvervoer of toepassing interactie. Ook worden gegevens vlak een blob kopiëren vanuit een opslag-account en toegang tot een openbare IP-adres RDP of SSH in de virtuele machine. Deze bewerkingen behouden de toepassing uitvoert via berekeningscluster, netwerken en opslag.

>[AZURE.NOTE] In sommige gevallen migratie het Azure platform stopt deallocates en opnieuw is opgestart uw virtuele machines. Dit bijhoudt een korte gegevens-vlak downtime.

## <a name="supported-scopes-of-migration"></a>Ondersteunde bereiken van de migratie

Er zijn drie migratie bereiken die zijn gericht op hoofdzakelijk berekeningscluster-, netwerk- en opslag. 

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Migratie van virtuele machines (niet in een virtueel netwerk)

Beveiliging wordt in het implementatiemodel resourcemanager al dan niet standaard afgedwongen voor uw toepassingen. Alle VMs moeten in een virtueel netwerk in het model resourcemanager. De Azure platform opnieuw is opgestart (`Stop`, `Deallocate`, en `Start`) de VMs als onderdeel van de migratie. U hebt twee opties voor de virtuele netwerken:

- U kunt het platform maken een nieuw virtuele netwerk en de virtuele machine migreren naar het nieuwe virtuele netwerk aanvragen.
- U kunt de virtuele machine migreren naar een bestaand virtual netwerk in resourcemanager.

>[AZURE.NOTE] In dit bereik migratie zijn zowel de bewerkingen management-vlak als de gegevens-vlak-bewerkingen niet toegestaan voor een periode tijdens de migratie.

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Migratie van virtuele machines (in een virtueel netwerk)

Alleen de metagegevens is voor de meeste VM configuraties migreren tussen de modellen voor klassieke en resourcemanager-implementatie. De onderliggende VMs worden op dezelfde hardware in hetzelfde netwerk en met dezelfde opslag uitgevoerd. De bewerkingen management-vlak mogelijk niet toegestaan voor een bepaalde periode tijdens de migratie. Echter blijft het vlak gegevens werken. Dat wil zeggen de toepassingen die naast VMs (klassieke) wordt uitgevoerd kunnen niet in rekening gebracht downtime tijdens de migratie.

De volgende configuraties worden momenteel niet ondersteund. Als ondersteuning is toegevoegd in de toekomst sommige VMs in deze configuratie downtime kunt oplopen (gaan toewijzing tot en met stoppen en opnieuw beginnen VM bewerkingen).

-   U hebt meer dan één beschikbaarheid instellen in een enkel cloudservice.
-   U hebt een of meer sets met beschikbaarheid en VMs die niet in een beschikbaarheid instellen in een enkel cloudservice.

>[AZURE.NOTE] In dit bereik migratie, het vlak van management niet zijn toegestaan voor een periode tijdens de migratie. Voor bepaalde configuraties zoals hierboven, gegevens-vlak downtime plaatsvindt.

### <a name="storage-accounts-migration"></a>Opslag accounts migratie

Als u wilt toestaan naadloze migratie, kunt u resourcemanager VMs implementeren in een klassieke opslag-account. Met deze mogelijkheid, netwerk- en rekenkracht resources kunnen en afzonderlijk van opslag accounts moeten worden gemigreerd. Wanneer u via uw virtuele Machines en virtuele netwerken migreert, moet u over uw accounts opslag om uit te voeren van het migratieproces migreren. 

>[AZURE.NOTE] Het implementatiemodel resourcemanager heeft geen het concept van het klassieke afbeeldingen en schijven. Wanneer het opslag-account wordt gemigreerd, klassieke afbeeldingen en schijven zijn niet zichtbaar in de stapel resourcemanager, maar de back-ups VHD's blijven in het opslag-account. 

## <a name="unsupported-features-and-configurations"></a>Niet-ondersteunde functies en configuraties

We ondersteunen niet momenteel sommige functies en configuraties. De volgende secties worden onze aanbevelingen rond ze.

### <a name="unsupported-features"></a>Niet-ondersteunde functies

De volgende functies worden momenteel niet ondersteund. U kunt optioneel verwijderen deze instellingen, de VMs migreren en de instellingen in het implementatiemodel resourcemanager weer inschakelen.

Resource-provider | Functie
---------- | ------------
Berekenen | Niet-gekoppelde VM schijven.
Berekenen | VM afbeeldingen.
Netwerk | Eindpunt ACL's.
Netwerk | Virtuele netwerkgateways (site naar site, Azure ExpressRoute, toepassingsgateway, wijs naar de site).
Netwerk | Virtuele netwerken met VNet Peering. (VNet migreren naar ARM en vervolgens op hetzelfde niveau) Meer informatie over [VNet Peering] (.. /Virtual-Network/Virtual-Network-peering-Overview.MD).
Netwerk | Verkeer Manager profielen.

### <a name="unsupported-configurations"></a>Niet-ondersteunde configuraties

De volgende configuraties worden momenteel niet ondersteund.

Service | Configuratie | Aanbeveling
---------- | ------------ | ------------
Resourcemanager | Rol op basis van Access besturingselement (RBAC) voor klassieke resources | Omdat de URI van de bronnen is gewijzigd na de migratie, is het aanbevolen dat u van plan bent de RBAC beleid-updates die moeten plaatsvinden na de migratie.
Berekenen | Meerdere subnetten die is gekoppeld aan een VM | De subnetconfiguratie om te verwijzen naar alleen subnetten bijwerken.
Berekenen | Virtuele machines die deel uitmaakt van een virtueel netwerk, maar geen een expliciete subnetten die is toegewezen | U kunt desgewenst de VM verwijderen.
Berekenen | Virtuele machines die waarschuwingen, automatisch schalen beleidsregels hebt | De migratie doorloopt en deze instellingen worden niet weergegeven. Het wordt ten zeerste aanbevolen dat u uw omgeving evalueren voordat u de migratie uitvoeren. U kunt ook de configuratie van de instellingen voor meldingen nadat de migratie is voltooid.
Berekenen | XML VM extensies (BGInfo 1.*, Visual Studio foutopsporing Web implementeren en externe foutopsporing) | Dit wordt niet ondersteund. Het wordt aanbevolen dat u deze extensies uit de virtuele machine verwijderen kunt doorgaan met migratie of wordt deze automatisch verwijderd tijdens het migratieproces.
Berekenen | Diagnostisch hulpprogramma opstarten met Premium-opslag | De functie voor opstarten diagnostische hulpprogramma's voor de VMs uitschakelen voordat u verdergaat met de migratie. Nadat de migratie voltooid is, kunt u opnieuw opstarten diagnostische gegevens in de stapel resourcemanager inschakelen. Daarnaast kunnen BLOB's die worden gebruikt voor schermafbeelding en seriële Logboeken verwijderen zodat u niet meer wordt geheven voor deze BLOB's.
Berekenen | Cloudservices met web/werknemer rollen | Dit is momenteel niet ondersteund.
Netwerk | Virtuele netwerken die virtuele machines en web/werknemer rollen bevatten |  Dit is momenteel niet ondersteund.
Azure App-Service | Virtuele netwerken die App Service omgevingen bevatten | Dit is momenteel niet ondersteund.
Azure HDInsight | Virtuele netwerken die HDInsight services bevatten | Dit is momenteel niet ondersteund.
Microsoft Dynamics Levenscyclusservices | Virtuele netwerken die virtuele machines die worden beheerd door Dynamics Levenscyclusservices bevatten | Dit is momenteel niet ondersteund.
Berekenen | Azure Beveiligingscentrum extensies met een VNET met een VPN-gateway of ER gateway met on-premises DNS-server | Uitbreidingen Azure Beveiligingscentrum automatisch geïnstalleerd op uw virtuele Machines bewaken hun beveiliging en waarschuwingen te verhogen. Deze extensies krijgen meestal automatisch geïnstalleerd als het beleid Beveiligingscentrum Azure op het abonnement dat is ingeschakeld. Tijdens de migratie van de gateway is momenteel niet ondersteund en de gateway moet worden verwijderd voordat u verder gaat met de migratie doorvoeren, gaat de internettoegang met VM opslag account verloren wanneer de gateway wordt verwijderd. De migratie niet worden voortgezet als dit gebeurt als het Gast agent status blob kan niet worden gevuld. Het wordt aanbevolen voor het uitschakelen van Azure Beveiligingscentrum beleid op het abonnement 3 uur voordat u verder gaat met de migratie.

## <a name="the-migration-experience"></a>De migratie-ervaring

Voordat u de migratie-ervaring, worden de volgende wordt aanbevolen:

- Zorg ervoor dat de resources die u wilt migreren niet een niet-ondersteunde functies of configuraties gebruikt. Meestal het platform worden gedetecteerd deze problemen en wordt een fout gegenereerd.
- Als er VMs die niet in een virtueel netwerk, worden ze gestopt en opgeheven als onderdeel van de voorbereidingsbewerking. Als u niet dat het openbare IP-adres kwijtraakt wilt, vindt u in het IP-adres reserveren voordat u de voorbereidingsbewerking activeert. Als de VMs zich in een virtueel netwerk, zijn ze echter niet gestopt en opgeheven.
- De migratie tijdens buiten kantooruren zo ins voor onverwachte fouten die tijdens de migratie optreden kunnen plannen.
- De huidige configuratie van uw VMs downloaden via PowerShell, opdrachtregel-interface (CLI) opdrachten of REST API's zodat u eenvoudiger voor validatie wanneer de voorbereiden stap voltooid is.
- Werk uw scripts automatisering/uitoefening voor het verwerken van het implementatiemodel resourcemanager voordat u de migratie te starten. Desgewenst kunt u doen GET-bewerkingen wanneer de resources in de voorbereide staat worden.
- Evalueren van het beleid RBAC die zijn geconfigureerd in de klassieke IaaS bronnen en plan voor nadat de migratie voltooid is.

De Migratiewerkstroom wordt als volgt bepaald

![Schermafbeelding met de Migratiewerkstroom](./media/virtual-machines-windows-migration-classic-resource-manager/migration-workflow.png)

>[AZURE.NOTE] Alle bewerkingen die worden beschreven in de volgende secties worden idempotency is ingeschakeld. Als er een probleem dan een niet-ondersteunde functie of een configuratiefout, het wordt aanbevolen dat u opnieuw de voorbereiden proberen, of bewerking afgebroken. Het Azure platform wordt geprobeerd de actie opnieuw.

### <a name="validate"></a>Valideren

De bewerking valideren is de eerste stap in het migratieproces. Het doel van deze stap is om te analyseren van gegevens in de achtergrond voor de resources onder migratie en gelukt/mislukt als de resources migratie kan zijn.

U selecteert de virtual network of de gehoste service (als dit niet een virtueel netwerk is) dat u wilt valideren voor migratie.

* Als de resource kan migratie is, bevat het Azure platform alle redenen waarom dit wordt niet ondersteund voor de migratie.

### <a name="prepare"></a>Voorbereiden

De voorbereidingsbewerking is de tweede stap bij het migratieproces. Het doel van deze stap is simuleren de transformatie van de bronnen voor IaaS van klassiek naar resourcemanager bronnen en presenteren dit elkaar waarin u kunt visualiseren.

U selecteert de virtual network of de gehoste service (als dit niet een virtueel netwerk is) dat u wilt migratie voorbereiden.

* Als de resource kan migratie is, wordt het Azure platform stopt van het migratieproces en Hiermee wordt de reden waarom de voorbereiden is mislukt.
* Als de resource kan migratie, aan de eerste vergrendelingen Azure platform de bewerkingen management-vlak voor de resources onder migratie worden weergegeven. Bijvoorbeeld: u bent niet mogen een gegevensschijf toevoegen aan een VM onder migratie.

Het Azure platform begint vervolgens met de migratie van metagegevens van klassiek naar resourcemanager voor de migratie resources.

Nadat de voorbereidingsbewerking voltooid is, hebt u de optie de informatiebronnen in beide klassieke visualiseren en resourcemanager. Voor elke cloudservice in het implementatiemodel klassieke, maakt het Azure platform de naam van een resource-groep waarop het patroon `cloud-service-name>-migrated`.

>[AZURE.NOTE] Virtuele Machines die niet in een netwerk met een klassieke virtuele gestopt deallocated in deze fase van de migratie.

### <a name="check-manual-or-scripted"></a>Check (handmatig of script)

In de stap controleren, kunt u desgewenst de configuratie die u eerder hebt gedownload gebruiken voor het valideren van de migratie uitziet. U kunt ook kunt u aanmelden bij de portal en controle de eigenschappen en bronnen voor het valideren van metagegevens migratie tevreden.

Als u een virtueel netwerk migreert, wordt de meeste configuratie van virtuele machines niet gestart. U kunt voor toepassingen op deze VMs valideren dat de toepassing nog steeds actief is.

U kunt uw monitoring/automatisering en operationele scripts om te zien als de VMs werkt zoals verwacht en als uw bijgewerkte scripts goed werkt testen. Alleen GET-bewerkingen worden ondersteund wanneer de resources in de voorbereide staat worden.

Er is geen set tijdvenster waarvóór u nodig hebt om door te voeren van de migratie. U kunt zo veel tijd als u in deze status wilt uitvoeren. Het vlak van management is echter vergrendeld voor deze resources totdat u afbreken of vast te leggen.

Als u eventuele problemen ziet, kunt u altijd de migratie te breken en Ga terug naar het implementatiemodel klassieke. Nadat u teruggaat, wordt het Azure platform management-vlak bewerkingen op de resources geopend zodat u normaal gesproken op deze VMs in het implementatiemodel klassieke kunt weer.

### <a name="abort"></a>Afbreken

Afbreken is een optionele stap die u kunt uw wijzigingen aan het implementatiemodel klassieke ongedaan maken en stoppen de migratie.

>[AZURE.NOTE] Deze bewerking kan niet worden uitgevoerd nadat u de doorvoerbewerking hebt geactiveerd.  

### <a name="commit"></a>Doorvoeren

Nadat u de validatie, kunt u de migratie doorvoeren. Resources worden niet meer weergegeven in de klassieke en zijn alleen beschikbaar in het implementatiemodel resourcemanager. De gemigreerde resources kunnen worden beheerd alleen in de nieuwe portal.

>[AZURE.NOTE] Dit is een bewerking idempotency is ingeschakeld. Als dit mislukt, is het aanbevolen dat u de bewerking opnieuw. Als deze zich blijft voordoen mislukt, een ondersteuningsticket maken of een forumbericht maken met een tag ClassicIaaSMigration op onze [VM-forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Dit abonnement migratie invloed op een van mijn bestaande services of toepassingen die worden uitgevoerd op Azure virtuele machines?**

Nee. De VMs (klassieke) zijn volledig ondersteunde services in de algemene beschikbaarheid. U kunt blijven gebruik deze bronnen voor het uitvouwen van uw footprint op Microsoft Azure.

**Wat gebeurt er met mijn VMs als ik niet van plan bent over het migreren van in de nabije toekomst?**

We zijn niet bestandstypen de bestaande klassieke API's en het model van de resource. Wij willen migratie gemakkelijker Bezig de geavanceerde functies die beschikbaar in het implementatiemodel resourcemanager zijn. Het is raadzaam dat u [enkele van de verbeteringen](virtual-machines-windows-compare-deployment-models.md) die deel uitmaken van IaaS onder resourcemanager bekijken.

**Wat betekent dit migratie-abonnement voor mijn bestaande tooling?**

Bijwerken van uw tooling aan het implementatiemodel resourcemanager-is een van de belangrijkste wijzigingen die u hebt voor in uw migratieplannen.

**Hoe lang de downtime management-vlak worden?**

Dit is afhankelijk van het aantal resources die zijn wordt gemigreerd. Voor kleinere implementaties (een paar tientallen VMs), moet de hele migratie minder dan een uur duren. De migratie kan een paar uur duren voor grootschalige implementaties (honderden VMs).

**Kan ik terug nadat het migreren van mijn resources zijn doorgevoerd in resourcemanager implementeren?**

Zo lang maken als de resources in de voorbereide staat worden, kunt u de migratie afbreken. Ongedaan maken wordt niet ondersteund als de bronnen hebt gemigreerd tot en met de doorvoerbewerking.

**Kan ik terugdraaien mijn migratie als de doorvoerbewerking mislukt?**

U kunt geen migratie afbreken als de doorvoerbewerking mislukt. Alle migratiebewerkingen, inclusief de doorvoerbewerking zijn idempotency is ingeschakeld. Dus is het raadzaam dat u opnieuw de bewerking na een korte tijdnotatie proberen. Als u nog steeds een foutbericht betrokken, een ondersteuningsticket maken of een forumbericht maken met het label ClassicIaaSMigration op onze [VM-forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows).

**Heb ik een ander express route circuitlijnen kopen als ik me IaaS onder resourcemanager gebruiken?**

Nee. Laatst ingeschakelde [ExpressRoute circuits uit het klassieke aan het implementatiemodel resourcemanager verplaatsen](../expressroute/expressroute-move.md). U hoeft niet te kopen van een nieuwe ExpressRoute circuitlijnen als u dat gedaan hebt.

**Wat gebeurt er als had ik Rolgebaseerd toegangsbeheer beleidsregels voor mijn klassieke IaaS resources geconfigureerd?**

Tijdens de migratie Transformeren de resources van klassiek naar resourcemanager. Dus is het raadzaam dat u van plan bent de RBAC beleid-updates die moeten plaatsvinden na de migratie.

**Wat gebeurt er als ik gebruik Azure Site herstel- of Azure vandaag?**

Als u wilt migreren van uw VM die zijn ingeschakeld voor back-up en Zie [ik hebt back-up gemaakt mijn klassieke VMs in back-kluis. Nu wil ik mijn VMs migreren van klassieke modus resourcemanager modus. Hoe kan ik back-up ze in herstel services kluis?](../backup/backup-azure-backup-ibiza-faq.md#i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode-how-can-i-backup-them-in-recovery-services-vault)

**Kan ik mijn abonnement of resources om te zien als zij kunnen migratie valideren?**

Ja. In de migratieoptie platform ondersteund is de eerste stap bij het voorbereiden voor migratie voor het valideren van de resources migratie kan zijn. Geval het valideren is mislukt, kunt u berichten ontvangt om alle redenen die de migratie kan niet worden voltooid.

**Wat gebeurt er als ik een fout quotum ondervindt bij het voorbereiden van de resources IaaS voor migratie?**

Het is raadzaam dat u de migratie te breken en meld op een verzoek voor ondersteuning van naar de quota in de regio waar u de VMs migreert verhogen. Nadat het verzoek quotum is goedgekeurd, kunt u beginnen migratiestappen opnieuw uitvoeren.

**Hoe meld ik een probleem?**

Uw problemen en vragen over migratie naar onze [VM-forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WAVirtualMachinesforWindows), met het trefwoord ClassicIaaSMigration posten. Het is raadzaam om alle uw vragen posten op deze forum. Als u een ondersteuningscontract hebt, bent u Welkom bij het aanmelden als u ook een ondersteuningsticket.

**Wat gebeurt er als ik niet tevreden bent de namen van de resources die het platform hebt gekozen tijdens de migratie?**

Alle bronnen die u namen voor in het implementatiemodel klassieke expliciet opgeeft worden tijdens de migratie bewaard. In sommige gevallen worden nieuwe resources gemaakt. Bijvoorbeeld: een netwerkinterface wordt gemaakt voor elke VM. Wordt de mogelijkheid om te bepalen de namen van deze nieuwe resources die zijn gemaakt tijdens de migratie momenteel niet ondersteund. Meld u aan uw stemmen voor deze functie op het [Feedbackforum van Azure](http://feedback.azure.com).

* *ik krijg een bericht *' VM is rapportage van de algemene status van de agent als niet gereed. Daarom kan niet de VM worden gemigreerd. Zorg ervoor dat de VM-Agent is rapportage van algehele agent status gereed'* of *"VM bevat waarvan u de Status niet wordt gerapporteerd uit de VM extensie. Daarom deze VM kan niet worden gemigreerd."***

Dit bericht is ontvangen wanneer de VM geen uitgaande verbinding met internet. De VM-agent wordt uitgaande verbindingen om het account Azure opslag voor het bijwerken van de status agent om de vijf minuten hebt bereikt.


## <a name="next-steps"></a>Volgende stappen
Nu dat u de migratie van klassieke IaaS resources aan resourcemanager kennen, kunt u beginnen met het migreren van resources.

- [Technische uitgebreide kennismaking op platform ondersteund migratie van klassiek naar Azure resourcemanager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
- [PowerShell gebruiken om te migreren naar Azure resourcemanager IaaS resources van klassiek](virtual-machines-windows-ps-migration-classic-resource-manager.md)
- [CLI gebruiken om te migreren naar Azure resourcemanager IaaS resources van klassiek](virtual-machines-linux-cli-migration-classic-resource-manager.md)
- [Een klassieke virtuele machine naar Azure resourcemanager klonen met behulp van de community PowerShell-scripts](virtual-machines-windows-migration-scripts.md)
