In dit artikel bevat een reeks bewezen procedures voor het uitvoeren van een virtuele Windows-computer (VM) op Azure, met aandacht voor schaalbaarheid, beschikbaarheid, beheer en beveiliging. 

> [AZURE.NOTE] Azure heeft twee verschillende implementatiemodellen: [Azure resourcemanager] [ resource-manager-overview] en klassieke. In dit artikel worden de resourcemanager, waarin u wordt aangeraden voor nieuwe implementaties gebruikt.

Wordt niet aanbevolen met behulp van een enkel VM voor productie-werkbelastingen, omdat er geen omhoog-tijd SLA service level agreement () voor één VMs op Azure. Als u de SLA, moet u meerdere VMs in een [beschikbaarheid instellen]implementeren[availability-set]. Zie voor meer informatie, [meerdere Windows VMs op Azure uitgevoerd][multi-vm]. 

## <a name="architecture-diagram"></a>Architectuurdiagram

Een VM in Azure wordt aangegeven inrichting heeft betrekking op meer bewegende onderdelen dan alleen de VM zelf. Er zijn berekeningscluster, netwerken en opslag van elementen.

> Een Visio-document met dit Architectuurdiagram is beschikbaar op het [Microsoft Downloadcentrum][visio-download]. In dit diagram is op de "berekeningscluster - één VM pagina.

![[0]][0]


- **Resourceveld groep.** Een [_resourcegroep_] [ resource-manager-overview] is een container met verwante resources. Een resourcegroep waarin de bronnen voor deze VM maken.

- **VM**. U kunt een VM uit een lijst met gepubliceerde afbeeldingen of uit een virtuele harde schijf (VHD)-bestand dat u naar Azure-blobopslag uploaden inrichten.

- **OS schijf.** De schijf OS is een VHD opgeslagen in [de opslagruimte van de Azure][azure-storage]. Dat betekent dat deze zich blijft voordoen zelfs als de host uitvalt.

- **Tijdelijke schijf.** De VM wordt gemaakt met een tijdelijke schijf (de `D:` station in Windows). Deze schijf is opgeslagen op een fysiek station op de hostcomputer. Het is _niet_ opgeslagen in Azure opslag en mogelijk, verdwijnen tijdens opnieuw opstarten en andere gebeurtenissen van de levenscyclus van VM. Gebruik deze schijf alleen voor tijdelijke gegevens, zoals pagina of omwisselen bestanden.

- **Gegevensschijven.** Een [schijf met gegevens] [ data-disk] een permanente VHD gebruikt voor de toepassingsgegevens is. Gegevensschijven worden opgeslagen in Azure opslag, zoals de schijf OS.

- **Virtuele netwerk (VNet) en subnet.** Elke VM in Azure is geïmplementeerd in een VNet, waarin wordt onderverdeeld in subnetten.

- **Openbare IP-adres.** Een openbare IP-adres is nodig om te communiceren met de VM&mdash;bijvoorbeeld via Extern bureaublad (RDP).

- **Netwerk-interface (NIC)**. De NIC kunt de VM om te communiceren met het virtuele netwerk.

- **Beveiligingsgroep netwerk (NSG)**. De [NSG] [ nsg] wordt gebruikt om te netwerkverkeer naar het subnet toestaan/weigeren. U kunt een NSG koppelen met een afzonderlijke NIC of met een subnet. Als u deze aan een subnet koppelen, wordt de NSG regels toegepast op alle VMs in dat subnet.
 
- **Diagnostische gegevens.** Diagnostische gegevens vastleggen is essentieel voor beheer en probleemoplossing bij de VM.

## <a name="recommendations"></a>Aanbevelingen

Azure biedt veel verschillende bronnen en resourcetypen, zodat deze architectuur verwijzing kan worden deze is ingericht veel verschillende manieren. We hebben een resourcemanager Azure-sjabloon om te kunnen installeren van de architectuur van de verwijzing die deze aanbevelingen volgt opgegeven. Als u wilt maken van de architectuur van uw eigen verwijzing moet u deze aanbevelingen volgt, tenzij u specifieke vereisten die een aanbeveling niet past hebt.

### <a name="vm-recommendations"></a>VM aanbevelingen

We raden de DS - en GS-reeks omdat deze formaten machine [Premium opslag ondersteunen][premium-storage]. Selecteer een van deze computer formaten tenzij u een gespecialiseerde werkbelasting zoals high performance computing hebt. Zie voor meer informatie [VM formaten][virtual-machine-sizes]. Wanneer u een bestaande werklast verplaatst naar Azure, begint u met de VM-grootte die het meest overeenkomt met uw on-premises-servers. Vervolgens maateenheid voor de prestaties van uw werkelijke werkbelasting met betrekking tot processor en geheugen schijf invoer/uitvoer bewerkingen per seconde (IO's / s) en de tekengrootte aanpassen indien nodig. Als u meerdere NIC nodig hebt, moet u op de hoogte van de limiet NIC voor elke grootte.  

Wanneer u de VM en andere resources inrichten, moet u een locatie opgeven. In het algemeen, kies een locatie die zich het dichtst bij uw interne gebruikers of klanten. Echter zijn niet alle VM maten beschikbaar op alle locaties. Zie voor meer informatie [Services per regio][services-by-region]. U kunt de grootte VM beschikbaar op een opgegeven locatie, voert u de volgende opdracht uit Azure opdrachtregel-interface (CLI):

```
    azure vm sizes --location <location>
```

Zie voor informatie over het kiezen van de afbeelding van een gepubliceerde VM [navigeren en selecteer Azure virtuele machines afbeeldingen][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Aanbevelingen van schijf en opslag

Voor de beste schijf i/o-prestaties raden [Premium opslag][premium-storage], welke gegevens worden opgeslagen op effen staat stations (SSD). Kosten is gebaseerd op de grootte van de ingerichte schijf. IO's / s en doorvoer ook zijn afhankelijk van de grootte van de schijf, dus wanneer u een schijf inrichten, houd rekening met alle drie factoren (capaciteit, IO's / s en doorvoer). 

Één opslag-account kan 1 tot en met 20 VMs ondersteunen.

Voeg een of meer gegevensschijven. Wanneer u een nieuwe VHD maakt, is het niet-opgemaakte. Meld u aan bij de VM de schijf te formatteren.

Als u een groot aantal gegevensschijven hebt, worden op de hoogte van de totale i/o-beperkingen van het account opslag. Zie voor meer informatie, [VM schijf limieten][vm-disk-limits].

Maak een aparte opslag-account waarin de diagnostische logboeken voor de beste prestaties. Een standaardaccount lokaal redundante opslag (LRS) is voldoende voor diagnostische logboeken.

Indien mogelijk, kunt u de toepassingen installeren op een gegevensschijf, niet de schijf OS. Sommige oudere toepassingen kunnen echter moet onderdelen installeren op station C:. In dat geval kunt u [het formaat wijzigen van de schijf OS] [ resize-os-disk] via PowerShell.

### <a name="network-recommendations"></a>Aanbevelingen voor netwerken

Het openbare IP-adres kan zijn dynamische of statische. De standaardinstelling is dynamische.

- Een [statische IP-adres] reserveren[ static-ip] als moet u een vaste IP-adres dat wordt niet gewijzigd &mdash; als u wilt maken van een A-record in DNS of het IP-adres nodig hebt om te worden whitelisted bijvoorbeeld.

- U kunt ook een volledig gekwalificeerde domeinnaam (FQDN) voor het IP-adres maken. U kunt vervolgens een [CNAME-record] hebt geregistreerd[ cname-record] in DNS die naar de FQDN-naam verwijst. Zie voor meer informatie [maken een volledig gekwalificeerde domeinnaam in de portal van Azure][fqdn].

Alle NSGs bevatten een set [standaardregels][nsg-default-rules], met inbegrip van een regel die alle inkomende internetverkeer blokkeert. De standaardregels kunnen niet worden verwijderd, maar ze kunnen worden vervangen door andere regels. Als u wilt inschakelen internetverkeer, maak regels die toestaan van binnenkomende verkeer naar bepaalde poorten &mdash; bijvoorbeeld poort 80 voor HTTP.  

Voeg een regel voor het NSG waarmee binnenkomende verkeer naar TCP-poort 3389 om in te schakelen RDP.

## <a name="scalability-considerations"></a>Aandachtspunten voor de schaalbaarheid

U kunt een VM schalen omhoog of omlaag door [de grootte VM][vm-resize]. 

Als u horizontaal verkleinen, plaatst u twee of meer VMs in een beschikbaarheid achter een taakverdeling instellen. Zie voor meer informatie [meerdere Windows VMs op Azure uitgevoerd][multi-vm].

## <a name="availability-considerations"></a>Aandachtspunten voor de beschikbaarheid

Zoals eerder vermeld, moet u er geen SLA voor een enkele VM is. Als u de SLA, moet u meerdere VMs implementeren in een set beschikbaarheid.

Uw VM kan worden beïnvloed door [gepland onderhoud] [ planned-maintenance] of [niet-gepland onderhoud][manage-vm-availability]. U kunt [VM start opnieuw op Logboeken] [ reboot-logs] om te bepalen of een VM opnieuw opstarten wordt veroorzaakt door gepland onderhoud.

VHD's worden ondersteund door [Azure Storage][azure-storage], die worden gerepliceerd voor levensduur en beschikbaarheid.

Als u wilt beveiligen tegen verlies van gegevens per ongeluk tijdens normale bewerkingen (bijvoorbeeld vanwege een gebruikersfout), moet u ook point-in-time back-ups, [blob momentopnamen] implementeren[ blob-snapshot] of een ander hulpmiddel.

## <a name="manageability-considerations"></a>Aandachtspunten voor de beheerbaarheid

**Resourcegroepen.** Zet nauw verbonden resources die de levenscyclus van de dezelfde in een dezelfde [resourcegroep]delen[resource-manager-overview]. Resourcegroepen kunnen u implementeren en controleren van bronnen als een groep en facturering kosten per resourcegroep samenvouwen. U kunt ook resources verwijderen als een set, dat wil bijzonder nuttig zijn voor test-implementaties zeggen. Geef resources betekenisvolle namen. Die gemakkelijker naar een specifieke bron zoekt en begrijpt de rol. Zie [aanbevolen naamgevingsconventies voor Azure Resources][naming conventions].

**VM diagnostische gegevens.** Inschakelen cmdlets voor controle en diagnostische hulpprogramma's, met inbegrip van eenvoudige systeemstatus maatstaven, diagnostisch hulpprogramma infrastructuur logboeken en [opstarten diagnostisch hulpprogramma][boot-diagnostics]. Opstarten diagnostische hulpprogramma's kunt u een diagnose stellen bij een fout bij het opstarten als uw VM in een niet-opgestart wordt. Zie [cmdlets voor controle en hulpprogramma's voor diagnose inschakelen]voor meer informatie[enable-monitoring]. Gebruik van de [Siteverzameling van Azure Log] [ log-collector] -extensie voor Azure platform logboeken verzamelen en uploadt u ze naar Azure opslag.   

De volgende opdracht uit CLI kunt diagnostische hulpprogramma's:

```
    azure vm enable-diag <resource-group> <vm-name>
```

**Een VM stoppen.** Azure maakt onderscheid tussen 'Gestopt' en 'Deallocated' toestand. U wordt geheven wanneer de status VM 'gestopt'. U bent geen btw geheven wanneer de VM opgeheven.

Gebruik de volgende opdracht uit CLI toewijzing van een VM:

```
    azure vm deallocate <resource-group> <vm-name>
```

De knop **stoppen** in de portal van Azure deallocates ook de VM. Echter als u met het besturingssysteem afsluit terwijl u bent aangemeld, wordt de VM gestopt, maar _niet_ ongedaan, zodat u nog steeds betalen.

**Een VM verwijderen.** Als u een VM verwijdert, worden de VHD's worden niet verwijderd. Dat betekent dat u kunt de VM veilig verwijderen zonder gegevens kwijt te raken. Echter nog steeds brengt voor opslag. Als u wilt verwijderen de VHD, kunt u het bestand verwijderen uit [blobopslag][blob-storage].

Als u wilt voorkomen dat per ongeluk wordt verwijderd, gebruikt u een [resource vergrendelen] [ resource-lock] de hele resource groep of vergrendelen afzonderlijke resources, zoals de VM vergrendelen. 

## <a name="security-considerations"></a>Veiligheidsoverwegingen

Gebruik [Beveiligingscentrum Azure] [ security-center] voor een centrale weergave van de beveiligingsstatus van uw Azure resources. Beveiligingscentrum bewaakt mogelijke beveiligingskwesties zoals systeem is bijgewerkt, antimalware, en een volledig beeld van de status van de beveiliging van uw implementatie geeft. 

- Beveiligingscentrum is per Azure abonnement geconfigureerd. Beveiliging verzamelen inschakelen, zoals wordt beschreven in [Met Beveiligingscentrum].

- Bij het verzamelen van gegevens is ingeschakeld, wordt elke VMs gemaakt onder abonnement automatisch gecontroleerd door Beveiligingscentrum.

**Beheer van patches.** Als ingeschakeld, Beveiligingscentrum Hiermee wordt gecontroleerd of beveiligingsupdates en essentiële updates verdwenen zijn. [Groepsbeleidsinstellingen] gebruiken[ group-policy] op de VM automatische updates inschakelen.

**Antimalware.** Als ingeschakeld, Beveiligingscentrum Hiermee wordt gecontroleerd of antimalware software is geïnstalleerd. U kunt ook Beveiligingscentrum gebruiken om de software antimalware uit in de portal van Azure te installeren.

Gebruik [Rolgebaseerd toegangsbeheer] [ rbac] (RBAC) toegang tot de Azure bronnen die u implementeert te beheren. RBAC kunt u autorisatie rollen toewijzen aan teamleden DevOps. Bijvoorbeeld kan de rol van de lezer Azure resources weergeven, maar niet maken, beheren of verwijderen. Sommige functies zijn specifiek voor bepaalde Azure resourcetypen. Bijvoorbeeld kunt de rol Inzender VM opnieuw of een VM toewijzing, de beheerderswachtwoord opnieuw instellen, een nieuwe VM maken, enzovoort. Andere [ingebouwde RBAC rollen] [ rbac-roles] die handig kan zijn voor deze verwijzing architectuur [DevTest Labs gebruiker] toe te voegen[ rbac-devtest] en [Netwerk Inzender][rbac-network]. Een gebruiker kan worden toegewezen aan meerdere rollen en u kunt aangepaste rollen voor nog meer fijnmazige machtigingen maken.

> [AZURE.NOTE] RBAC doet geen afbreuk aan de acties die een gebruiker is aangemeld bij een VM kan uitvoeren. Deze machtigingen zijn afhankelijk van het accounttype op de Gast OS.   

Als u het lokale beheerderswachtwoord opnieuw, wilt uitvoeren de `vm reset-access` Azure CLI opdracht.

```
    azure vm reset-access -u <user> -p <new-password> <resource-group> <vm-name>
```

Gebruik [controlelogboeken] [ audit-logs] om te zien van de inrichting van acties en andere gebeurtenissen VM.

Houd rekening met [Azure schijfversleuteling] [ disk-encryption] als u nodig hebt voor het coderen van de schijven OS en gegevens. 

## <a name="solution-deployment"></a>Implementatie van oplossingen

Een [implementatie] [ github-folder] architectuur waarop u deze aanbevolen procedures voor een verwijzing is beschikbaar. Deze verwijzing architectuur bevat een virtueel netwerk (VNet), netwerk-beveiligingsgroep (NSG) en een enkel virtuele machine (VM).

Er zijn verschillende manieren om te implementeren van de architectuur van deze verwijzing. De eenvoudigste manier is de volgende stappen te volgen: 

1. Klik met de rechtermuisknop op de knop onderstaande en selecteer een van beide 'koppeling openen in nieuw tabblad"of 'Koppeling openen in nieuw venster'.  
[![Implementeren naar Azure](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)

2. Nadat de koppeling is geopend in de portal van Azure, moet u waarden opgeven voor een deel van de instellingen: 
    - De naam van de **resourcegroep** is al gedefinieerd in het parameterbestand, dus **Nieuw** te selecteren en voer `ra-single-vm-rg` in het tekstvak.
    - Selecteer de regio in de vervolgkeuzelijst **locatie** .
    - Bewerk de **Sjabloon hoofdsite Uri** of de **Parameter hoofdsite Uri** tekstvakken niet.
    - Selecteer het **Type besturingssysteem** uit de vervolgkeuzelijst, **windows**.
    - Controleer de bepalingen en voorwaarden en klik op het selectievakje **dat ik ga akkoord met de bovenstaande voorwaarden** .
    - Klik op de knop **aanschaffen** .

3. Wacht totdat de implementatie om te voltooien.

4. De parameterbestanden bevatten een beheerder vastgelegde-gebruikersnaam en wachtwoord en het wordt ten zeerste aanbevolen dat u direct beide wijzigen. Klik op de VM met de naam `ra-single-vm0 `in de Portal Azure. Klik vervolgens op **wachtwoord opnieuw** in het blad **ondersteuning + probleemoplossing** . Selecteer **wachtwoord opnieuw** in het vak van de vervolgkeuzelijst **modus** en selecteer vervolgens een nieuwe **gebruikersnaam** en **wachtwoord**. Klik op de knop **bijwerken** om te voorkomen van de nieuwe gebruikersnaam en wachtwoord.

Zie voor informatie over andere manieren om te implementeren van deze verwijzing architectuur, het Leesmij-bestand in de [richtlijnen-één-vm][github-folder]] Github map. 

## <a name="customize-the-deployment"></a>De implementatie aanpassen

Als u wijzigen van de implementatie wilt aan uw behoeften, volgt u de instructies in het [Leesmij][github-folder]. 

## <a name="next-steps"></a>Volgende stappen

Om de [SLA voor virtuele Machines] [ vm-sla] als u wilt toepassen, moet u twee of meer exemplaren in een set beschikbaarheid implementeren. Zie voor meer informatie, [meerdere VMs op Azure uitgevoerd][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]: ../articles/virtual-machines/virtual-machines-windows-create-availability-set.md
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-windows-about-disks-vhds.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-windows-portal-create-fqdn.md
[github-folder]: http://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm
[group-policy]: https://technet.microsoft.com/en-us/library/dn595129.aspx
[log-collector]: https://azure.microsoft.com/en-us/blog/simplifying-virtual-machine-troubleshooting-using-azure-log-collector/
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-windows-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-windows-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[resize-os-disk]: ../articles/virtual-machines/virtual-machines-windows-expand-os-disk.md
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[security-center]: https://azure.microsoft.com/en-us/services/security-center/
[select-vm-image]: ../articles/virtual-machines/virtual-machines-windows-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[Met Beveiligingscentrum]: ../articles/security-center/security-center-get-started.md#use-security-center
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-windows-sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_0/
[0]: ./media/guidance-blueprints/compute-single-vm.png "Eén Windows VM architectuur in Azure wordt aangegeven"
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[blocks]: https://github.com/mspnp/template-building-blocks
