<properties
    pageTitle="Overzicht van de Windows-virtuele Machines | Microsoft Azure"
    description="Meer informatie over het maken en beheren van Windows virtuele machines in Azure wordt aangegeven."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="davidmu"/>

# <a name="overview-of-windows-virtual-machines-in-azure"></a>Overzicht van Windows virtuele machines in Azure wordt aangegeven

Azure virtuele Machines (VM) is een van de verschillende soorten [op aanvraag, scalable bronnen](../app-service-web/choose-web-site-cloud-service-vm.md) die Azure biedt. Meestal kiezen u een VM wanneer u meer controle over de IT-omgeving dan de andere opties bieden nodig hebt. In dit artikel vindt u informatie over wat u rekening moet houden voordat u maakt een VM, hoe u deze hebt gemaakt en hoe u deze beheren.

Een VM Azure beschikt u over de flexibiliteit om virtualization zonder te kopen en voor het behoud van de fysieke hardware die wordt uitgevoerd als deze. Echter nog steeds moet u voor het behoud van de VM door het uitvoeren van taken, zoals configureren, herstellen en installeren van de software die wordt uitgevoerd op is geïnstalleerd.

Azure virtuele machines kan worden gebruikt op verschillende manieren. Er zijn enkele voorbeelden:

- **Ontwikkel- en** – Azure VMs aanbieding een snelle en eenvoudige manier om te maken van een computer met specifieke configuraties vereist om code en testen van een toepassing.
- **Toepassingen in de cloud** – omdat de aanvraag voor uw toepassing kan variëren, kan het zinvol economic uit te voeren op een VM in Azure wordt aangegeven. U betaalt voor extra VMs wanneer u deze nodig hebt en deze afsluiten wanneer u niet.
- **Uitgebreide datacenter** – virtuele machines in een netwerk met Azure virtuele worden eenvoudig verbonden met het netwerk van uw organisatie.

Het aantal VMs die gebruikmaakt van uw toepassing kunt omhoog en out schalen naar wat verplicht is voor het aan uw wensen voldoet.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>Wat heb ik nodig om na te denken over voordat u maakt een VM?

Er zijn altijd een groot aantal [Ontwerpoverwegingen](virtual-machines-windows-infrastructure-virtual-machine-guidelines.md) wanneer u een toepassing infrastructuur in Azure bouwen. Deze aspecten van een VM zijn belangrijk om na te denken over voordat u begint:

- De namen van uw toepassing resources
- De locatie waar de resources worden opgeslagen
- De grootte van de VM
- Het maximum aantal VMs die kan worden gemaakt
- Het besturingssysteem dat de VM wordt uitgevoerd
- De configuratie van de VM nadat deze is gestart
- De verwante resources die nog moet worden de VM

### <a name="naming"></a>Naam geven

Een virtuele machine heeft een [naam](virtual-machines-windows-infrastructure-naming-guidelines.md) die is toegewezen en er een computernaam als onderdeel van het besturingssysteem. De naam van een VM mag maximaal 15 tekens.

Als u Azure gebruikt om te maken van de schijf besturingssysteem werkt, zijn de computernaam van de en de naam van de VM hetzelfde. Als u [uploaden en gebruiken van uw eigen afbeelding](virtual-machines-windows-upload-image.md) met een eerder geconfigureerde besturingssysteem wordt uitgevoerd en deze gebruiken om u te maken van een virtuele machine, de namen kunnen afwijken. Het is raadzaam dat wanneer u uw eigen afbeeldingsbestand uploadt, u de naam van de computer in het besturingssysteem maakt en de virtuele machine dezelfde naam.

### <a name="locations"></a>Locaties

Alle resources die zijn gemaakt in Azure zijn verdeeld over meerdere [geografische regio's](https://azure.microsoft.com/regions/) overal ter wereld. Meestal wordt het gebied **locatie** genoemd wanneer u een VM maakt. Voor een VM geeft de locatie waar de virtuele vaste schijven worden opgeslagen.

Deze tabel worden enkele van de manieren waarop die u een lijst met beschikbare locaties gaat.

| Methode | Beschrijving |
| ------ | ----------- |
| Azure-portal | Selecteer een locatie in de lijst wanneer u een VM maakt. |
| Azure PowerShell | Gebruik de opdracht [Get-AzureRmLocation](https://msdn.microsoft.com/library/mt619449.aspx) . |
| REST API | De bewerking voor de [lijst met locaties](https://msdn.microsoft.com/library/dn790540.aspx) gebruiken. |

### <a name="vm-size"></a>VM grootte

De [grootte](virtual-machines-windows-sizes.md) van de VM die u gebruikt, wordt bepaald door de werklast die u wilt uitvoeren. De grootte die u vervolgens kiest bepaalt factoren zoals het verwerken van kracht, geheugen en opslagcapaciteit. Azure biedt een breed scala van formaten ter ondersteuning van vele soorten worden gebruikt.

Azure kosten een [prijs per uur](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) op basis van de grootte en het besturingssysteem van de VM. Gedeeltelijke uur kosten Azure alleen voor de minuten gebruikt. Opslag is prijs en afzonderlijk in rekening gebracht.

### <a name="vm-limits"></a>VM limieten

Uw abonnement heeft standaard [quotumlimiet](../azure-subscription-service-limits.md) in de locatie waarop van invloed op de implementatie van veel VMs voor uw project zijn kan. De huidige limiet op basis van de per abonnement is 20 VMs per regio. Limieten kunnen worden verhoogd door het indienen van een ondersteuningsticket aanvragen van een grotere.

### <a name="operating-system-disks-and-images"></a>Besturingssysteem schijven en afbeeldingen

[Virtuele vaste schijven](virtual-machines-windows-about-disks-vhds.md) virtuele machines gebruiken om op te slaan hun besturingssysteem (OS) en gegevens. VHD's worden ook gebruikt voor de afbeeldingen die u kiezen kunt uit om een besturingssysteem te installeren. 

Azure biedt veel [marketplace afbeeldingen](https://azure.microsoft.com/marketplace/virtual-machines/) gebruiken met verschillende versies en Windows-besturingssystemen voor servers typen. Marketplace-afbeeldingen worden aangeduid met afbeelding publisher, aanbieding sku en versie (meestal versie is opgegeven als laatste). 

In deze tabel ziet enkele manieren dat u de gegevens voor een afbeelding kunt vinden.

| Methode | Beschrijving |
| ------ | ----------- |
| Azure-portal | De waarden zijn automatisch voor u opgegeven wanneer u een afbeelding gebruiken selecteert. |
| Azure PowerShell | [Get-AzureRMVMImagePublisher](https://msdn.microsoft.com/library/mt603484.aspx) -locatie "locatie"<BR>[Get-AzureRMVMImageOffer](https://msdn.microsoft.com/library/mt603824.aspx) -locatie "locatie"-Publisher "publisherName"<BR>[Get-AzureRMVMImageSku](https://msdn.microsoft.com/library/mt619458.aspx) -locatie "locatie"-Publisher "publisherName"-"offerName" bieden |
| REST API 's | [Lijst met afbeelding uitgevers](https://msdn.microsoft.com/library/mt743702.aspx)<BR>[Afbeelding van de lijst biedt](https://msdn.microsoft.com/library/mt743700.aspx)<BR>[Lijst met afbeelding SKU 's](https://msdn.microsoft.com/library/mt743701.aspx) |

U kunt kiezen om te [uploaden en uw eigen afbeelding gebruiken](virtual-machines-windows-upload-image.md) en wanneer u dat doet, de naam van de uitgever, aanbieding en sku niet worden gebruikt.

### <a name="extensions"></a>Uitbreidingen

VM [extensies](virtual-machines-windows-extensions-features.md) geven uw VM extra mogelijkheden tot en met de configuratie van de post-implementatie en geautomatiseerde taken.

Deze algemene taken kunnen worden uitgevoerd met extensies:

- **Aangepaste scripts worden uitgevoerd** – de [Aangepaste Script extensie](virtual-machines-windows-extensions-customscript.md) , kunt u de werkbelasting configureren op de VM door het script uit te voeren wanneer de VM is ingericht.
- **Distribueren en beheren van configuraties** – de [extensie PowerShell gewenst staat configuratie (DSC)](virtual-machines-windows-extensions-dsc-overview.md) kunt u DSC op een VM instellen voor het beheren van configuraties en omgevingen.
- **Diagnostische gegevens verzamelen** – de [Extensie van Azure diagnostische hulpprogramma's](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) , kunt u de VM te verzamelen naar diagnostische gegevens die kunnen worden gebruikt om te controleren van de status van uw toepassing configureren.

### <a name="related-resources"></a>Verwante resources

De resources in deze tabel worden gebruikt door de VM en moeten bestaan of worden gemaakt wanneer de VM is gemaakt.

| Resource | Vereist | Beschrijving |
| -------- | -------- | ----------- |
| [Resourcegroep](../azure-resource-manager/resource-group-overview.md) | Ja | De VM moet zijn opgenomen in een resourcegroep. |
| [Opslag-account](../storage/storage-create-storage-account.md) | Ja | De VM vereisten voor de opslag-account voor de opslag van de virtuele vaste schijven. |
| [Virtuele netwerk](../virtual-network/virtual-networks-overview.md) | Ja | De VM moet lid zijn van een virtueel netwerk. |
| [Openbare IP-adres](../virtual-network/virtual-network-ip-addresses-overview-arm.md) | Nee | De VM kan een openbare IP-adres dat is toegewezen aan deze op afstand toegang toe hebben. |
| [Netwerkinterface](../virtual-network/virtual-network-network-interface-overview.md) | Ja | De VM vereisten voor de netwerkinterface om te communiceren in het netwerk. |
| [Gegevensschijven](virtual-machines-windows-attach-disk-portal.md) | Nee | De VM kunt gegevensschijven om uit te vouwen opslagcapaciteit opnemen. |

## <a name="how-do-i-create-my-first-vm"></a>Hoe kan ik mijn eerste VM maken?

Hebt u verschillende opties voor het maken van uw VM. De keuze die u maakt, is afhankelijk van de omgeving die u bezig bent. 

Deze tabel vindt u informatie om u aan de slag voor het maken van uw VM.

| Methode | Artikel |
| ------ | ------- |
| Azure-portal | [Een virtuele machine met Windows met behulp van de portal maken](virtual-machines-windows-hero-tutorial.md) |
| Sjablonen | [Een virtuele Windows-computer met een resourcemanager-sjabloon maken](virtual-machines-windows-ps-template.md) |
| Azure PowerShell | [Maken van een Windows-VM via PowerShell](virtual-machines-windows-ps-create.md) |
| Client SDK 's | [Azure Resources met C# implementeren](virtual-machines-windows-csharp.md) |
| REST API 's | [Maken of een VM bijwerken](https://msdn.microsoft.com/library/mt163591.aspx) |

U ermee nooit hiervoor, maar af en toe er iets mis gaat. Als deze situatie voor u gebeurt, kijkt u naar de informatie in [problemen met resourcemanager implementatieproblemen met het maken van een virtuele Windows-computer in Azure wordt aangegeven](virtual-machines-windows-troubleshoot-deployment-new-vm.md).

## <a name="how-do-i-manage-the-vm-that-i-created"></a>Hoe beheer ik de VM dat ik heb gemaakt?

VMs kunnen worden beheerd via een browser gebaseerde-portal opdrachtregel extra met ondersteuning voor het uitvoeren van scripts, of rechtstreeks via API's. Enkele typische beheertaken die u kunt uitvoeren op het punt informatie over een VM, aanmelden bij een VM, de beschikbaarheid van beheren en het maken van back-ups.

### <a name="get-information-about-a-vm"></a>Informatie over een VM

In deze tabel ziet u enkele van de manier waarop u vind informatie over een VM.

| Methode | Beschrijving |
| ------ | ----------- |
| Azure-portal | Klik in het menu hub **virtuele Machines** op en selecteer vervolgens de VM in de lijst. Op het blad voor de VM hebt u toegang tot overzichtsinformatie, waarden, en doelstellingen volgen. |
| Azure PowerShell | Zie [beheren Azure virtuele Machines resourcemanager en PowerShell gebruiken](virtual-machines-windows-ps-manage.md)voor informatie over het gebruik van PowerShell voor het beheren van VMs. |
| REST API | De bewerking [ophalen VM informatie](https://msdn.microsoft.com/library/mt163682.aspx) gebruiken om informatie over een VM. |
| Client SDK 's | Zie voor informatie over het gebruik van C# voor het beheren van VMs [Azure virtuele Machines beheren met behulp van Azure resourcemanager en C#](virtual-machines-windows-csharp-manage.md). |

### <a name="log-on-to-the-vm"></a>Meld u aan bij de VM

U gebruikt de knop koppelen in de Azure-portal een extern bureaublad-sessie te [starten](virtual-machines-windows-connect-logon.md). Soms kan fout gaan wanneer u probeert een externe verbinding te gebruiken. Als deze situatie voor u gebeurt, raadpleegt u de help-informatie in [problemen met extern bureaublad-verbindingen met een Azure virtuele machines waarop Windows wordt uitgevoerd](virtual-machines-windows-troubleshoot-rdp-connection.md).

### <a name="manage-availability"></a>Beschikbaarheid van beheren

Het is belangrijk voor u kunnen begrijpen hoe om ervoor te [zorgen dat de beschikbaarheid](virtual-machines-windows-manage-availability.md) voor uw toepassing. Deze configuratie heeft betrekking op het maken van meerdere VMs om ervoor te zorgen dat ten minste één wordt uitgevoerd.

In de volgorde voor uw implementatie in aanmerking komt voor onze 99.95 VM Service Level Agreement, moet u twee of meer VMs uitgevoerd van uw werkzaamheden binnen een [beschikbaarheid instellen](virtual-machines-windows-infrastructure-availability-sets-guidelines.md)implementeren. Deze configuratie zorgt ervoor dat uw VMs zijn verdeeld over meerdere foutenstructuuranalyse domeinen en worden geïmplementeerd op hosts met verschillende onderhoud windows. De volledige [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) wordt uitgelegd dat de gegarandeerd beschikbaarheid van Azure als geheel.
 
### <a name="back-up-the-vm"></a>Back-up van de VM
 
Een [kluis herstel Services](../backup/backup-introduction-to-azure-backup.md) wordt gebruikt voor het beveiligen van gegevens en activa in Azure back-up- en Azure sites worden hersteld. U kunt een kluis herstel Services te [implementeren en beheren van back-ups voor resourcemanager geïmplementeerd VMs via PowerShell](../backup/backup-azure-vms-automation.md). 

## <a name="next-steps"></a>Volgende stappen

- Als de bedoeling aangeeft is voor gebruik met Linux VMs, kijkt u naar [Azure en Linux](virtual-machines-linux-azure-overview.md).
- Meer informatie over de richtlijnen rond het instellen van uw infrastructuur in dit [voorbeeld Azure-infrastructuur Stapsgewijze instructies](virtual-machines-windows-infrastructure-example.md).
- Zorg ervoor dat u de [Aanbevolen procedures voor het uitvoeren van een Windows-VM op Azure](virtual-machines-windows-guidance-compute-single-vm.md)volgen.


