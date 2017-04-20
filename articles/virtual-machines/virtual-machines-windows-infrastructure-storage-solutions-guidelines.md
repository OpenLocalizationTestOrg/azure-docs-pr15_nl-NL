<properties
    pageTitle="Richtlijnen voor opslag-oplossingen | Microsoft Azure"
    description="Meer informatie over de belangrijkste ontwerpen en implementeren richtlijnen voor het implementeren van opslagoplossingen in Azure infrastructuurservices."
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="storage-infrastructure-guidelines"></a>Richtlijnen voor opslag-infrastructuur

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

In dit artikel ligt de nadruk met informatie over de opslag en ontwerpoverwegingen voor de realisatie van prestaties optimale virtuele machine (VM).


## <a name="implementation-guidelines-for-storage"></a>Implementatie van richtlijnen voor opslag

Beslissingen:

- Moet u de standaard- of Premium opslagruimte voor uw werkzaamheden gebruiken?
- Moet u Schijfopruiming gesegmenteerd te verdelen schijven groter zijn dan 1023 GB maken?
- Hebt u Schijfopruiming gesegmenteerd te verdelen om te bereiken optimale o prestaties voor uw werkzaamheden nodig?
- Welke instellen opslag accounts hebt u nodig voor het hosten van uw IT-werkbelasting of infrastructuur?

Taken:

- Bekijk i/o-vereisten van de toepassingen die u implementeert en de juiste nummer en de typen opslag accounts plannen.
- De set met uw naamgevingsconventie opslag-accounts maken. U kunt Azure PowerShell of de portal.


## <a name="storage"></a>Opslag

Azure opslag is een belangrijk onderdeel van de implementatie en het beheer van virtuele machines (VMs) en toepassingen. Azure opslag kunt u services voor het opslaan van gegevens uit een bestand, ongestructureerde gegevens en berichten en het is ook deel uit van de infrastructuur ondersteunende VMs.

Er zijn twee soorten opslag accounts beschikbaar ter ondersteuning van VMs:

- Standaard opslag accounts u toegang krijgt tot blobopslag (gebruikt voor het opslaan van Azure VM schijven), tabelopslag, wachtrij opslag- en bestandsopslag.
- [Opslag van de Premium](../storage/storage-premium-storage.md) -accounts bieden krachtige, lage latentie schijfondersteuning voor intensief i/o--werkbelastingen, zoals SQL-Servers in een cluster AlwaysOn. Premium opslag ondersteunt momenteel alleen Azure VM schijven.

Azure maakt VMs met een besturingssysteem schijf, een tijdelijke schijf en nul of meer optionele gegevensschijven. Het besturingssysteem schijf en de gegevensschijven zijn pagina Azure BLOB's, terwijl de tijdelijke schijf lokaal is opgeslagen op het knooppunt waar de computer zich bevindt. Wees voorzichtig bij het ontwerpen van toepassingen alleen deze tijdelijke schijf gebruiken voor niet-permanente gegevens, zoals de VM tussen hosts tijdens een onderhoudsgebeurtenis mogelijk worden gemigreerd. Gegevens die zijn opgeslagen op de tijdelijke schijf wordt gaat verloren.

Levensduur en betere beschikbaarheid wordt geleverd door de onderliggende Azure Storage-omgeving om ervoor te zorgen dat uw gegevens beveiligen tegen ongeplande onderhoud of hardware pas uit fouten blijft. Als u uw omgeving Azure Storage ontwerpt, kunt u kiezen VM opslag repliceren:

- lokaal binnen een bepaald Azure datacenter
- via Azure datacenters binnen een bepaald gebied
- via Azure datacenters tussen de verschillende regio 's

Lees [meer over de replicatieopties voor maximale beschikbaarheid](../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Besturingssysteem schijven en gegevensschijven hebt een maximale grootte van 1023 gigabyte (GB). De maximale grootte van een blob 1024 GB is en dat de metagegevens (voettekst) van het VHD-bestand (een GB is 1024<sup>3</sup> bytes) moet bevatten. U kunt opslagruimte spaties in Windows Server 2012 groter zijn dan deze limiet door groepsgewijze samen gegevensschijven als u wilt presenteren logische volumes groter zijn dan 1023GB voor uw VM.

Werkt de volgende limieten schaalbaarheid bij het ontwerpen van uw Azure Storage-implementaties: Zie [limieten voor het abonnement en -service van Microsoft Azure, quota's en beperkingen](azure-subscription-service-limits.md#storage-limits) voor meer informatie. Zie ook [Azure opslag schaalbaarheid en prestaties doelen](../storage/storage-scalability-targets.md).

Voor de opslag van de toepassing, kunt u opslaan ongestructureerde objectgegevens zoals documenten, afbeeldingen, back-ups, configuratiegegevens, Logboeken, enzovoort. blobopslag gebruiken. De toepassing kunt in plaats van uw toepassing is virtual opslaan die zijn bijgevoegd bij de VM, schrijven rechtstreeks naar Azure-blobopslag. Blobopslag bevat ook de optie van [warm- en leuke opslag lagen](../storage/storage-blob-storage-tiers.md) afhankelijk van het benodigde beschikbaarheid en de kostenbeperkingen.


## <a name="striped-disks"></a>Gestreepte schijven
Naast zodat u kunt schijven groter zijn dan 1023 GB, maken in veel gevallen verbetert gesegmenteerd te verdelen met voor gegevensschijven de prestaties door meerdere BLOB's naar achteren verplaatsen de opslag van voor één volume toe te staan. Door gesegmenteerd te verdelen, gaat de I/O vereist om gegevens uit één logische schijf lezen en schrijven door parallel.

Azure legt beperkingen ten aanzien van het aantal gegevensschijven en hoeveel bandbreedte beschikbaar, afhankelijk van de grootte VM. Zie [grootten voor virtuele machines](virtual-machines-windows-sizes.md)voor meer informatie.

Als u Schijfopruiming gesegmenteerd te verdelen voor Azure gegevensschijven gebruikt, kunt u de volgende richtlijnen:

- Gegevensschijven moet altijd de maximale grootte (1023 GB).
- De maximale gegevensschijven toegestaan voor de grootte VM bijvoegen.
- Opslag spaties gebruiken.
- Vermijd het gebruik van Azure gegevensschijf cache-opties (caching van beleid = geen).

Zie [opslagruimte spaties - ontwerpen voor de prestaties](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx)voor meer informatie.


## <a name="multiple-storage-accounts"></a>Meerdere opslag-accounts

Bij het ontwerpen van uw Azure Storage-omgeving, kunt u meerdere accounts van opslag als het aantal VMs u toeneemt implementeren. Hiermee kunt uit de I/O verdelen over de onderliggende Azure Storage-infrastructuur te onderhouden optimale prestaties voor uw VMs en toepassingen. Bij het ontwerpen van de toepassingen die u wilt implementeren, kunt u de i/o-vereisten die elke VM heeft en balance '-out die VMs Azure Storage-accounts. Proberen te voorkomen dat de groepering van alle de hoge in-en vragen VMs in met maar één of twee opslag-accounts.

Voor meer informatie over de i/o-mogelijkheden van de verschillende opties voor de opslag van Azure en enkele aanbevolen maximumwaarden, raadpleegt u [Azure opslag schaalbaarheid en prestaties doelen](../storage/storage-scalability-targets.md).


## <a name="next-steps"></a>Volgende stappen

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 