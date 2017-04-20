In dit artikel bevat een reeks bewezen procedures voor het uitvoeren van een Linux virtuele machine (VM) op Azure, met aandacht voor schaalbaarheid, beschikbaarheid, beheer en beveiliging. Azure ondersteunt verschillende populaire Linux-onderzoeken, inclusief CentOS, Debian, rood rol Enterprise, Ubuntu en FreeBSD uitgevoerd. Zie voor meer informatie [Azure en Linux][azure-linux].

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen: [Resourcemanager] [ resource-manager-overview] en klassieke. In dit artikel worden de resourcemanager, waarin u wordt aangeraden voor nieuwe implementaties gebruikt.

Wordt niet aanbevolen met behulp van een enkel VM voor productie-werkbelastingen, omdat er geen omhoog-time-service SLA (level agreement) voor één VMs op Azure. Als u de SLA, moet u meerdere VMs in een [beschikbaarheid instellen]implementeren[availability-set]. Zie voor meer informatie, [meerdere VMs op Azure uitgevoerd][multi-vm]. 

## <a name="architecture-diagram"></a>Architectuurdiagram

Een VM in Azure wordt aangegeven inrichting heeft betrekking op meer bewegende onderdelen dan alleen de VM zelf. Er zijn berekeningscluster, netwerken en opslag-elementen die moet u rekening houden.

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft Downloadcentrum][visio-download]. In dit diagram is op de "berekeningscluster - één VM' pagina.

![[0]][0]

- **Resourcegroep.** Een [_resourcegroep_] [ resource-manager-overview] is een container met verwante resources. Een resourcegroep waarin de bronnen voor deze VM maken.

- **VM**. U kunt een VM uit een lijst met gepubliceerde afbeeldingen of uit een virtuele harde schijf (VHD)-bestand dat u naar Azure-blobopslag uploaden inrichten.

- **OS schijf.** De schijf OS is een VHD opgeslagen in [de opslagruimte van de Azure][azure-storage]. Dat betekent dat deze zich blijft voordoen zelfs als de host uitvalt. De schijf OS is `/dev/sda1`.

- **Tijdelijke schijf.** De VM wordt gemaakt met een tijdelijke schijf. Deze schijf is opgeslagen op een fysiek station op de hostcomputer. Het is _niet_ opgeslagen in Azure opslag en mogelijk, verdwijnen tijdens opnieuw opstarten en andere gebeurtenissen van de levenscyclus van VM. Gebruik deze schijf alleen voor tijdelijke gegevens, zoals pagina of omwisselen bestanden. De tijdelijke schijf `/dev/sdb1` en is gekoppeld aan `/mnt/resource` of `/mnt`.

- **Gegevensschijven.** Een [schijf met gegevens] [ data-disk] een permanente VHD gebruikt voor de toepassingsgegevens is. Gegevensschijven worden opgeslagen in Azure opslag, zoals de schijf OS.

- **Virtuele netwerk (VNet) en subnet.** Elke VM in Azure wordt aangegeven in een (VNet), wordt geïmplementeerd in subnetten die is verdere verdeeld.

- **Openbare IP-adres.** Een openbare IP-adres is nodig om te communiceren met de VM&mdash;bijvoorbeeld via SSH.

- **Netwerk-interface (NIC)**. De NIC kunt de VM om te communiceren met het virtuele netwerk.

- **Beveiligingsgroep netwerk (NSG)**. De [NSG] [ nsg] wordt gebruikt om te netwerkverkeer naar het subnet toestaan/weigeren. U kunt een NSG koppelen met een afzonderlijke NIC of met een subnet. Als u deze aan een subnet koppelen, wordt de NSG regels toegepast op alle VMs in dat subnet.
 
- **Diagnostische gegevens.** Diagnostische gegevens vastleggen is essentieel voor beheer en probleemoplossing bij de VM.

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en resourcetypen, zodat deze architectuur verwijzing kan worden deze is ingericht veel verschillende manieren. We hebben een resourcemanager Azure-sjabloon om te kunnen installeren van de architectuur van de verwijzing die deze aanbevelingen volgt opgegeven. Als u wilt maken van de architectuur van uw eigen verwijzing moet u deze aanbevelingen volgt, tenzij u specifieke vereisten die een aanbeveling niet past hebt. 

### <a name="vm-recommendations"></a>VM aanbevelingen

We raden de DS - en GS-reeks omdat deze formaten machine [Premium opslag ondersteunen][premium-storage]. Selecteer een van deze computer formaten tenzij u een gespecialiseerde werkbelasting zoals high performance computing hebt. Zie voor meer informatie [VM formaten][virtual-machine-sizes]. Wanneer u een bestaande werklast verplaatst naar Azure, begint u met de VM-grootte die het meest overeenkomt met uw on-premises-servers. De prestaties van uw werkelijke werkbelasting betreffende CPU vervolgens meten, geheugen en schijfruimte invoer/uitvoer bewerkingen per seconde (IO's / s) en de tekengrootte aanpassen indien nodig. Als u meerdere NIC nodig hebt, moet u op de hoogte van de limiet NIC voor elke grootte.  

Wanneer u de VM en andere resources inrichten, moet u een locatie opgeven. In het algemeen, kies een locatie die zich het dichtst bij uw interne gebruikers of klanten. Echter zijn niet alle VM maten beschikbaar op alle locaties. Zie voor meer informatie [Services per regio][services-by-region]. U kunt de grootte VM beschikbaar op een opgegeven locatie, voert u de volgende opdracht uit Azure opdrachtregel-interface (CLI):

```
    azure vm sizes --location <location>
```

Zie voor informatie over het kiezen van de afbeelding van een gepubliceerde VM [navigeren en selecteer Azure virtuele machines afbeeldingen][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Schijf en opslagruimte aanbevelingen

Voor de beste schijf i/o-prestaties raden [Premium opslag][premium-storage], welke gegevens worden opgeslagen op solid-state stations (SSD). Kosten is gebaseerd op de grootte van de ingerichte schijf. IO's / s en doorvoer (dat wil zeggen de gegevens tarief overbrengen) ook, hangt af van de schijfgrootte van de, dus wanneer u een schijf inrichten, alle drie factoren overwegen (capaciteit, IO's / s en doorvoer). 

Één opslag-account kan 1 tot en met 20 VMs ondersteunen.

Voeg een of meer gegevensschijven. Wanneer u een VHD maakt, is het niet-opgemaakte. Meld u aan bij de VM de schijf te formatteren. De gegevensschijven weergeven als `/dev/sdc`, `/dev/sdd`, enzovoort. U kunt uitvoeren `lsblk` voor een overzicht van de apparaten blokkeren, inclusief de schijven. Als u wilt gebruiken op een gegevensschijf, maak een systeem partition en de bestandsnaam en de schijf koppelen. Bijvoorbeeld:

```bat
    # Create a partition.
    sudo fdisk /dev/sdc     # Enter 'n' to partition, 'w' to write the change.     
    
    # Create a file system.
    sudo mkfs -t ext3 /dev/sdc1
    
    # Mount the drive.
    sudo mkdir /data1
    sudo mount /dev/sdc1 /data1
```

Als u veel van gegevensschijven hebt, worden op de hoogte van de totale i/o-beperkingen van het account opslag. Zie voor meer informatie, [VM schijf limieten][vm-disk-limits].

Wanneer u een gegevensschijf toevoegt, wordt een logische eenheid getal (LUN)-ID toegewezen aan de schijf. Desgewenst kunt u de ID van de LUN &mdash; bijvoorbeeld als u bent vervangen van een schijf en wilt bewaren van dezelfde LUN-ID, of er een app die Hiermee wordt gezocht naar een specifieke LUN-ID. Vergeet niet dat LUN-id's uniek zijn voor elke schijf moet.

U wilt wijzigen van de i/o-planner, optimaliseren voor prestaties op solid-state stations (SSD) (gebruikt door de Premium-opslag). Een algemene aanbeveling wordt de planner Nooperation gebruikt voor SSD, maar u moet een hulpmiddel zoals [iostat] gebruiken om de schijf o prestaties voor uw bepaalde werkbelasting te houden.

- Maak een aparte opslag-account waarin de diagnostische logboeken voor de beste prestaties. Een standaardaccount lokaal redundante opslag (LRS) is voldoende voor diagnostische logboeken.


### <a name="network-recommendations"></a>Aanbevelingen voor netwerken

Het openbare IP-adres kan zijn dynamische of statische. De standaardinstelling is dynamische.

- Een [statische IP-adres] reserveren[ static-ip] als moet u een vaste IP-adres dat wordt niet gewijzigd &mdash; als u wilt maken van een A-record in DNS of het IP-adres nodig hebt om te worden whitelisted bijvoorbeeld.

- U kunt ook een volledig gekwalificeerde domeinnaam (FQDN) voor het IP-adres maken. U kunt vervolgens een [CNAME-record] hebt geregistreerd[ cname-record] in DNS die naar de FQDN-naam verwijst. Zie voor meer informatie [maken een volledig gekwalificeerde domeinnaam in de portal van Azure][fqdn].

Alle NSGs bevatten een set [standaardregels][nsg-default-rules], met inbegrip van een regel die alle inkomende internetverkeer blokkeert. De standaardregels kunnen niet worden verwijderd, maar ze kunnen worden vervangen door andere regels. Als u wilt inschakelen internetverkeer, maak regels die toestaan van binnenkomende verkeer naar bepaalde poorten &mdash; bijvoorbeeld poort 80 voor HTTP.  

Als u wilt inschakelen SSH, moet u een regel toevoegen aan de NSG waarmee binnenkomende verkeer met TCP-poort 22.

## <a name="scalability-considerations"></a>Aandachtspunten voor de schaalbaarheid

U kunt een VM schalen omhoog of omlaag door [de grootte VM][vm-resize]. 

Als u horizontaal verkleinen, plaatst u twee of meer VMs in een beschikbaarheid achter een taakverdeling instellen. Zie voor meer informatie [meerdere VMs op Azure uitgevoerd][multi-vm].

## <a name="availability-considerations"></a>Aandachtspunten voor de beschikbaarheid

Zoals eerder vermeld, moet u er geen SLA voor een enkele VM is. Als u de SLA, moet u meerdere VMs implementeren in een set beschikbaarheid.

Uw VM kan worden beïnvloed door [gepland onderhoud] [ planned-maintenance] of [niet-gepland onderhoud][manage-vm-availability]. U kunt [VM start opnieuw op Logboeken] [ reboot-logs] om te bepalen of een VM opnieuw opstarten wordt veroorzaakt door gepland onderhoud.

VHD's worden ondersteund door [Azure Storage][azure-storage], die worden gerepliceerd voor levensduur en beschikbaarheid.

Als u wilt beveiligen tegen verlies van gegevens per ongeluk tijdens normale bewerkingen (bijvoorbeeld vanwege een gebruikersfout), moet u ook point-in-time back-ups, [blob momentopnamen] implementeren[ blob-snapshot] of een ander hulpmiddel.

## <a name="manageability-considerations"></a>Aandachtspunten voor de beheerbaarheid

**Resourcegroepen.** Zet nauw verbonden resources die de levenscyclus van de dezelfde in dezelfde [resourcegroep]delen[resource-manager-overview]. Resourcegroepen kunnen u implementeren en controleren van bronnen als een groep en facturering kosten per resourcegroep samenvouwen. U kunt ook resources verwijderen als een set, dat wil bijzonder nuttig zijn voor test-implementaties zeggen. Geef resources betekenisvolle namen. Die gemakkelijker naar een specifieke bron zoekt en begrijpt de rol. Zie [aanbevolen naamgevingsconventies voor Azure Resources][naming conventions].

**SSH**. Voordat u een VM Linux maken, Genereer een paar openbare persoonlijke sleutels van 2048-bits RSA. Gebruik het bestand met openbare sleutel wanneer u de VM maakt. Zie voor meer informatie [hoe u gebruik SSH met Linux en Mac op Azure][ssh-linux].

**VM diagnostische gegevens.** Inschakelen cmdlets voor controle en diagnostische hulpprogramma's, met inbegrip van eenvoudige systeemstatus maatstaven, diagnostisch hulpprogramma infrastructuur logboeken en [opstarten diagnostisch hulpprogramma][boot-diagnostics]. Opstarten diagnostische hulpprogramma's kunt u een diagnose stellen bij opstarten mislukt als uw VM in een niet-opgestart wordt. Zie [cmdlets voor controle en hulpprogramma's voor diagnose inschakelen]voor meer informatie[enable-monitoring].  

De volgende opdracht uit CLI kunt diagnostische hulpprogramma's:

```
    azure vm enable-diag <resource-group> <vm-name>
```

**Een VM stoppen.** Azure maakt onderscheid tussen 'Gestopt' en 'Deallocated' toestand. U wordt geheven wanneer de status VM 'gestopt'. U bent geen btw geheven wanneer de VM opgeheven.

Gebruik de volgende opdracht uit CLI toewijzing van een VM:

```
    azure vm deallocate <resource-group> <vm-name>
```

- De knop **stoppen** in de portal van Azure deallocates ook de VM. Echter als u met het besturingssysteem afsluit terwijl u bent aangemeld, wordt de VM gestopt, maar _niet_ ongedaan, zodat u nog steeds betalen.

**Een VM verwijderen.** Als u een VM verwijdert, worden de VHD's worden niet verwijderd. Dat betekent dat u kunt de VM veilig verwijderen zonder gegevens kwijt te raken. Echter nog steeds brengt voor opslag. Als u wilt verwijderen de VHD, kunt u het bestand verwijderen uit [blobopslag][blob-storage].

Als u wilt voorkomen dat per ongeluk wordt verwijderd, gebruikt u een [resource vergrendelen] [ resource-lock] de hele resource groep of vergrendelen afzonderlijke resources, zoals de VM vergrendelen. 

## <a name="security-considerations"></a>Veiligheidsoverwegingen

OS updates automatiseren met behulp van de [OSPatching] VM extensie. Dit toestel installeren als u de VM inrichten. U kunt opgeven hoe vaak u patches installeren en of op te starten na herstellen.

Gebruik [Rolgebaseerd toegangsbeheer] [ rbac] (RBAC) toegang tot de Azure bronnen die u implementeert te beheren. RBAC kunt u autorisatie rollen toewijzen aan teamleden DevOps. Bijvoorbeeld kan de rol van de lezer Azure resources weergeven, maar niet maken, beheren of verwijderen. Sommige functies zijn specifiek voor bepaalde Azure resourcetypen. Bijvoorbeeld kunt de rol Inzender VM opnieuw starten of een VM toewijzing, de beheerderswachtwoord opnieuw instellen, een VM maken, enzovoort. Andere [ingebouwde RBAC rollen] [ rbac-roles] die handig kan zijn voor deze verwijzing architectuur [DevTest Labs gebruiker] toe te voegen[ rbac-devtest] en [Netwerk Inzender][rbac-network]. 

Een gebruiker kan worden toegewezen aan meerdere rollen en u kunt aangepaste rollen voor nog meer fijnmazige machtigingen maken.

> [AZURE.NOTE] RBAC doet geen afbreuk aan de acties die een gebruiker is aangemeld voor een VM kan uitvoeren. Deze machtigingen zijn afhankelijk van het accounttype op de Gast OS.   

Gebruik [controlelogboeken] [ audit-logs] om te zien van de inrichting van acties en andere gebeurtenissen VM.

- Houd rekening met [Azure schijfversleuteling] [ disk-encryption] als u nodig hebt voor het coderen van de schijven OS en gegevens. 

## <a name="solution-deployment"></a>Implementatie van oplossingen

Een [implementatie] [ github-folder] architectuur waarop u deze aanbevolen procedures voor een verwijzing is beschikbaar. Deze verwijzing architectuur bevat een virtueel netwerk (VNet), netwerk-beveiligingsgroep (NSG) en een enkel virtuele machine (VM).

Er zijn verschillende manieren om te implementeren van de architectuur van deze verwijzing. De eenvoudigste manier is de volgende stappen te volgen: 

1. Klik met de rechtermuisknop op de knop onderstaande en selecteer een van beide 'koppeling openen in nieuw tabblad"of 'Koppeling openen in nieuw venster'.
[![Implementeren naar Azure](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)

2. Nadat de koppeling is geopend in de portal van Azure, moet u waarden opgeven voor een deel van de instellingen: 
    - De naam van de **resourcegroep** is al gedefinieerd in het parameterbestand, dus **Nieuw** te selecteren en voer `ra-single-vm-rg` in het tekstvak.
    - Selecteer de regio in de vervolgkeuzelijst **locatie** .
    - Bewerk de **Sjabloon hoofdsite Uri** of de **Parameter hoofdsite Uri** tekstvakken niet.
    - Selecteer het **Type besturingssysteem** uit de vervolgkeuzelijst, **linux**.
    - Controleer de bepalingen en voorwaarden en klik op het selectievakje **dat ik ga akkoord met de bovenstaande voorwaarden** .
    - Klik op de knop **aanschaffen** .

3. Wacht totdat de implementatie om te voltooien.

4. De parameterbestanden bevatten een beheerder vastgelegde-gebruikersnaam en wachtwoord en het wordt ten zeerste aanbevolen dat u direct beide wijzigen. Klik op de VM met de naam `ra-single-vm0 `in de Portal Azure. Klik op **wachtwoord opnieuw** in de sectie **ondersteuning + probleemoplossing** . Selecteer **wachtwoord opnieuw** in het vak van de vervolgkeuzelijst **modus** en selecteer vervolgens een nieuwe **gebruikersnaam** en **wachtwoord**. Klik op de knop **bijwerken** om te voorkomen van de nieuwe gebruikersnaam en wachtwoord.

Zie voor informatie over andere manieren om te implementeren van deze verwijzing architectuur, het Leesmij-bestand in de [richtlijnen-één-vm] [ github-folder] Github map.

## <a name="customize-the-deployment"></a>De implementatie aanpassen

Als u de implementatie aan uw behoeften, voert u de instructies in de [richtlijnen-één-vm] [ github-folder] pagina.

## <a name="next-steps"></a>Volgende stappen

Om de [SLA voor virtuele Machines] [ vm-sla] als u wilt toepassen, moet u twee of meer exemplaren in een beschikbaarheid instellen implementeren. Zie voor meer informatie, [meerdere VMs op Azure uitgevoerd][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]: ../articles/virtual-machines/virtual-machines-windows-create-availability-set.md
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-linux]: ../articles/virtual-machines/virtual-machines-linux-azure-overview.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-linux-about-disks-vhds.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-linux-portal-create-fqdn.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm/
[iostat]: https://en.wikipedia.org/wiki/Iostat
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-linux-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[OSPatching]: https://github.com/Azure/azure-linux-extensions/tree/master/OSPatching
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-linux-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[select-vm-image]: ../articles/virtual-machines/virtual-machines-linux-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssh-linux]: ../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-linux-sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_0/
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[components]: #Solution-components
[blocks]: https://github.com/mspnp/template-building-blocks
[0]: ./media/guidance-blueprints/compute-single-vm.png "Eén Linux VM architectuur in Azure wordt aangegeven"

