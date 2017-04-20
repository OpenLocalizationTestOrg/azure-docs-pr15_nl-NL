<properties
    pageTitle="Migreren naar Azure Premium opslag | Microsoft Azure"
    description="Uw bestaande virtuele machines met Azure Premium Storage migreren. Premium opslagruimte biedt krachtige, lage latentie schijfondersteuning voor ik/O-intensief werkbelasting op Azure virtuele Machines uitgevoerd."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="yuemlu"/>


# <a name="migrating-to-azure-premium-storage"></a>Migreren naar Azure Premium-opslag

## <a name="overview"></a>Overzicht

Azure Premium opslagruimte biedt krachtige, lage latentie schijfondersteuning voor virtuele machines ik/O-intensief werkbelasting uitgevoerd. U kunt profiteren van de snelheid en prestaties van deze schijven uitvoeren door te migreren van uw toepassing VM schijven met Azure Premium Storage.

Het doel van deze handleiding is om nieuwe gebruikers van Azure Premium Storage betere voorbereiden om te maken van een soepele overgang van hun huidige systeem naar Premium opslag te helpen. De gids adressen drie van de belangrijkste onderdelen van dit proces: 

  - [Het gebruik van de migratie naar Premium opslagruimte plannen](#plan-the-migration-to-premium-storage)
  - [Voorbereiden en virtuele vaste schijven kopiëren naar een Premium-opslag](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
  - [Azure virtuele Machine Premium opslag maken](#create-azure-virtual-machine-using-premium-storage)

U kunt VMs migreren van andere platforms met Azure Premium Storage of bestaande Azure VMs migreren van standaardopslag naar Premium opslag. Deze handleiding behandelt de stappen voor beide twee scenario's. Volg de stappen die zijn opgegeven in de desbetreffende sectie afhankelijk van uw scenario.

>[AZURE.NOTE] U kunt een Functieoverzicht van de en prijzen van Premium opslagruimte in Premium opslag vinden: [High-Performance-opslag voor Azure virtuele machines werkbelasting](storage-premium-storage.md). Het is raadzaam een virtuele machine schijf hoog IO's / s met Azure Premium Storage vereisen voor de beste prestaties voor uw toepassing migreren. Als de schijf niet voor hoge IO's / s vereist is, kunt u kosten kunt beperken door deze in de standaardopslag, waarmee VM schijfgegevens worden opgeslagen op vaste schijven (harde schijven) in plaats van SSD onderhouden.

Voltooiing van het migratieproces in zijn geheel mogelijk extra acties zowel voor en na de stappen in deze handleiding. Voorbeelden hiervan zijn configureren via virtuele netwerken of eindpunten of u wijzigingen aanbrengt code vanuit de toepassing zelf waarin u mogelijk enkele downtime in uw toepassing. Deze acties uniek zijn voor elke toepassing en u moet ze samen met de stappen in deze handleiding om de volledige overgang naar de Premium-opslag zo weinig mogelijk te voltooien.


## <a name="plan-the-migration-to-premium-storage"></a>Het gebruik van de migratie naar Premium opslagruimte plannen

In dit gedeelte zorgt ervoor dat u klaar bent voor de migratiestappen in dit artikel en u helpt bij het maken van de beste beslissing op VM en schijf typen.

### <a name="prerequisites"></a>Vereisten voor
- Moet u een Azure-abonnement. Als u deze niet hebt, kunt u deze kunt maken van een abonnement van een maand [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/) of Ga naar [Azure prijzen](https://azure.microsoft.com/pricing/) voor meer opties.
- Als u wilt uitvoeren PowerShell-cmdlets, moet u de Microsoft Azure PowerShell-module. Zie [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md) voor de installatie punt en de installatie-instructies.
- Wanneer u van plan bent Azure VMs waarop Premium opslag gebruiken, moet u de Premium-opslag kunnen VMs gebruiken. U kunt de standaard- en Premium opslag schijven gebruiken met Premium opslagruimte nodig VMs. Premium schijven zijn in de toekomst beschikbaar met meer VM typen. Zie voor meer informatie over alle beschikbare Azure VM schijftypen en formaten, [grootte voor virtuele machines](../virtual-machines/virtual-machines-windows-sizes.md) en de [puntgrootten voor Cloudservices](../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Overwegingen

Een VM Azure ondersteunt verschillende Premium schijven koppelen zodat uw toepassingen kunnen maximaal 64 TB aan opslagcapaciteit per VM hebben. Met de Premium-opslag, kunnen uw toepassingen 80.000 IO's / s (input/output-bewerkingen per seconde) per VM en 2000 MB per tweede schijfdoorvoer per VM met erg laag vertragingstijden voor meer bewerkingen bereiken. U hebt opties in een groot aantal VMs en schijven. In dit gedeelte is om te helpen u om een optie die het beste past bij uw werkzaamheden te zien.

#### <a name="vm-sizes"></a>VM grootte
De Azure VM grootte specificaties worden vermeld in [formaten voor virtuele machines](../virtual-machines/virtual-machines-windows-sizes.md). Bekijk de prestatie-eigenschappen van virtuele machines die werken met de Premium-opslag en kiest u de meest geschikte VM grootte die het beste past bij uw werkzaamheden. Zorg ervoor dat er voldoende bandbreedte beschikbaar op uw VM is om de schijf verkeer door te sturen.


#### <a name="disk-sizes"></a>Schijfgrootte
Er zijn drie soorten schijven die kunnen worden gebruikt met uw VM en elk heeft specifieke IO's / s en doorvoer limieten. Rekening deze limieten wanneer het schijftype voor uw VM kiezen op basis van de behoeften van uw toepassing wat betreft capaciteit, prestaties en schaalbaarheid en piek wordt geladen.

|Type Premium opslag-schijf|P10|P20|P30|
|:---:|:---:|:---:|:---:|
|Grootte van de schijf|128 GB|512 GB|1024 GB (1 TB)|
|IO's / s per schijf|500|2300|5000|
|Doorvoer per schijf|100 MB per seconde|150 MB per seconde|200 MB per seconde|

Bepalen of aanvullende gegevensschijven nodig is voor uw VM zijn afhankelijk van uw werkzaamheden. U kunt meerdere permanente gegevensschijven toevoegen aan uw VM. Indien nodig kunt u op de schijven om uit te breiden en de prestaties van het volume stripe. (Zie Wat is schijfsegmentering [hier](storage-premium-storage-performance.md#disk-striping).) Als u Premium gegevensschijven [Opslag spaties]met stripe[4], moet u deze configureren met één kolom voor elke schijf die wordt gebruikt. Anders de algehele prestaties van het volume van het volgende mogelijk lager dan verwacht vanwege ongelijkmatige spreiding van verkeer op de schijven. Voor Linux VMs kunt u het hulpprogramma *mdadm* gebruiken om te bereiken hetzelfde. Zie het artikel [Software RAID configureren op Linux](../virtual-machines/virtual-machines-linux-configure-raid.md) voor meer informatie.

#### <a name="storage-account-scalability-targets"></a>Opslag account schaalbaarheid doelen
Premium opslag-accounts zijn de volgende schaalbaarheid streefcijfers naast de [schaalbaarheid van Azure-opslag en prestaties doelen](storage-scalability-targets.md). Als uw toepassingsvereisten voor de de doelen schaalbaarheid van een account één opslag overschrijdt, uw toepassing gebruik van meerdere opslag-accounts maken en uw gegevens partitioneren deze opslag-accounts.

|Totale Account capaciteit|Totale bandbreedte voor een lokaal redundante opslag-Account|
|:--|:---|
|Capaciteit schijf: 35TB<br />Momentopname van capaciteit: 10 TB|Maximaal 50 gigabits per seconde voor Inkomend + uitgaand|

Voor de meer informatie over de opslag van de Premium-specificaties, raadpleegt u [schaalbaarheid en prestaties doelen wanneer u een Premium-opslag gebruikt](storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

#### <a name="disk-caching-policy"></a>In de cache beleid schijf
Standaard schijf caching van beleid is *Alleen-lezen* voor alle gegevens van de Premium schijven en *Alleen-lezen-schrijven* voor de Premium-besturingssysteem schijf gekoppeld aan de VM. De configuratieinstelling van deze wordt aanbevolen om de optimale prestaties voor IOs van uw toepassing. Uitschakelen schijf in cache opslaan zodat u betere toepassingsprestaties kunt bereiken voor veel schrijven of alleen-schrijven gegevensschijven (zoals logboekbestanden van SQL Server). De cache-instellingen voor bestaande gegevensschijven kunnen worden bijgewerkt via [Azure-Portal](https://portal.azure.com) of de parameter *- HostCaching* van de cmdlet *Set-AzureDataDisk* .

#### <a name="location"></a>Locatie
Kies een locatie waar Azure Premium opslag beschikbaar is. Zie [Azure Services per regio](https://azure.microsoft.com/regions/#services) voor actuele informatie over de beschikbare locaties. VMs zich bevindt in de regio hetzelfde als de opslagruimte-account dat hierin de schijven voor VM veel betere prestaties krijgt dan als ze in verschillende regio's zijn.

#### <a name="other-azure-vm-configuration-settings"></a>Andere Azure VM configuratie-instellingen
Wanneer u een VM Azure maakt, wordt u gevraagd om bepaalde VM-instellingen te configureren. Onthoud dat enkele instellingen voor de levensduur van de VM, worden opgelost terwijl u kunt wijzigen of anderen later toevoegen. Bekijk deze Azure VM configuratie-instellingen en zorg ervoor dat deze om te voldoen aan vereisten voor werkbelasting correct zijn geconfigureerd.

### <a name="optimization"></a>Optimalisatie

[Azure Premium Storage: ontwerp voor krachtige](storage-premium-storage-performance.md) bevat richtlijnen voor het samenstellen van krachtige toepassingen met Azure Premium opslag. De richtlijnen gecombineerd met aanbevolen procedures prestaties van toepassing op technologieën die door de toepassing gebruikt, kunt u volgen.


## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Voorbereiden en virtuele vaste schijven kopiëren naar een Premium-opslag

De volgende sectie bevat richtlijnen voor het voorbereiden van VHD's uit uw VM en VHD's kopiëren naar Azure-opslag.

- [Scenario 1: "Ik ben migreren bestaande Azure VMs met Azure Premium Storage."](#scenario1)
- [Scenario 2: "Ik ben migreren VMs vanaf andere platforms met Azure Premium Storage."](#scenario2)

### <a name="prerequisites"></a>Vereisten voor

Als u wilt de VHD's voorbereiden voor migratie, hebt u het volgende nodig:

- Een Azure-abonnement, een opslag-account en een container in dat opslag-account waarmee u uw VHD kunt kopiëren. Houd er rekening mee dat de bestemming opslag-account kan een standaard- of Premium opslag account afhankelijk van uw behoeften.
- Een hulpmiddel de VHD generaliseren als u van plan bent om te maken van meerdere VM exemplaren van deze. Bijvoorbeeld sysprep voor Windows of virt-sysprep voor Ubuntu.
- Een hulpmiddel voor het VHD-bestand uploaden naar de opslag-account. Zie [gegevens met het hulpprogramma AzCopy-opdrachtregel overbrengen](storage-use-azcopy.md) of een [Azure opslag explorer](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx)gebruiken. Deze handleiding worden uw VHD met de functie AzCopy kopiëren.

> [AZURE.NOTE] Als u de optie synchroon kopiëren met AzCopy, kiest voor optimale prestaties, kopieert u uw VHD door een van deze hulpprogramma's uit te voeren vanuit een Azure-VM die in het gebied dezelfde als de bestemming opslag-account is. Als u een VHD vanuit een VM Azure in een andere regio kopieert, is het mogelijk dat uw prestaties langzamer afspelen.
>
> Voor het kopiëren van een grote hoeveelheid gegevens via beperkte bandbreedte, overweeg het [gebruik van de service Azure importeren/exporteren om gegevens met Blob Storage](storage-import-export-service.md); Hiermee kunt u uw gegevens door verzending vaste schijven overbrengen naar een Azure datacenter. Gegevens kopiëren naar een standaard-opslag-account alleen kunt u de service Azure importeren/exporteren. Als u de gegevens hebt in uw standaard opslag-account, kunt u de [Kopie Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx) of AzCopy wilt overbrengen van de gegevens naar uw account van de opslagruimte premium.
>
> Houd er rekening mee dat Microsoft Azure biedt alleen ondersteuning voor vaste grootte VHD-bestanden. VHDX bestanden of dynamische VHD's worden niet ondersteund. Als u een dynamische VHD hebt, kunt u deze converteren naar vaste grootte met de cmdlet [Converteren VHD](http://technet.microsoft.com/library/hh848454.aspx) .

### <a name="scenario1"></a>Scenario 1: "Ik ben migreren bestaande Azure VMs met Azure Premium Storage."

Als u bestaande Azure VMs migreert, de VM stoppen, VHD's per het type VHD gewenste voorbereiden en kopieer de VHD met AzCopy of PowerShell.

De VM moet volledig omlaag naar de beginwaarden migreren. Er is een downtime totdat de migratie is voltooid.

#### <a name="step-1-prepare-vhds-for-migration"></a>Stap 1. VHD's migratie voorbereiden

Als u bestaande Azure VMs naar Premium opslag migreert, is uw VHD mogelijk:

- Een afbeelding van de algemene besturingssysteem
- Een unieke besturingssysteem schijf
- Een gegevensschijf

We begeleiden hieronder volgende 3 scenario's voor het voorbereiden van uw VHD.

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a>Een algemene besturingssysteem VHD gebruiken om te maken van meerdere exemplaren van VM

Als u een VHD die wordt gebruikt voor het maken van meerdere algemene Azure VM exemplaren uploaden wilt, moet u eerst VHD met een hulpprogramma sysprep generalize. Dit geldt naar een VHD on-premises of in de cloud. De gegevens van een specifiek voor machine verwijdert Sysprep uit de VHD.

>[AZURE.IMPORTANT] Een momentopname of back-up maken uw VM voordat u deze terugzetten. Actieve sysprep wordt stoppen en het exemplaar VM toewijzing. Volg de onderstaande stappen naar sysprep een Windows-besturingssysteem VHD. Houd er rekening mee dat de opdracht Sysprep uitvoert, moet u de virtuele machine afgesloten. Zie [Overzicht van Sysprep](http://technet.microsoft.com/library/hh825209.aspx) of [Technische naslaginformatie](http://technet.microsoft.com/library/cc766049.aspx)voor meer informatie over Sysprep.

1. Open een opdrachtpromptvenster als beheerder.
2. Voer de volgende opdracht Sysprep openen:

        %windir%\system32\sysprep\sysprep.exe

3. In het systeemhulpprogramma voor het voorbereiden select Voer systeem kant-en-klare Experience (OOBE), schakel het selectievakje Generalize, selecteer **Afsluiten**en klik vervolgens op **OK**, zoals wordt weergegeven in de onderstaande afbeelding. Sysprep wordt generalize van het besturingssysteem en het systeem afsluiten.

    ![][1]

Gebruik voor een VM Ubuntu virt sysprep om te bereiken hetzelfde. Zie [virt sysprep](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) voor meer informatie. Zie ook enkele van de bron openen [inrichting van Linux-Server-software](http://www.cyberciti.biz/tips/server-provisioning-software.html) voor andere Linux-besturingssystemen.

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a>Een unieke besturingssysteem VHD gebruiken om te maken van één exemplaar VM

Als u een toepassing wordt uitgevoerd de VM waarvoor u de specifieke gegevens van de computer hebt, u de VHD niet generalize. Een niet-generalized VHD kan worden gebruikt om een unieke Azure VM exemplaar te maken. Bijvoorbeeld, hebt u domeincontroller op uw VHD, wordt sysprep uitvoeren kunt u niet effectief als domeincontroller. Bekijk de toepassingen die op uw VM en de invloed van sysprep worden uitgevoerd voordat u de VHD noemt wordt uitgevoerd.

##### <a name="register-data-disk-vhd"></a>Gegevensschijf VHD registreren

Als u gegevensschijven in Azure moeten worden gemigreerd hebt, moet u ervoor zorgen dat de VMs die gebruikmaken van deze gegevensschijven worden afgesloten.

Volg de stappen hieronder beschreven VHD naar Azure Premium Storage kopiëren en te registreren als een gegevensschijf ingerichte.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Stap 2. De bestemming voor uw VHD maken

Maak een account opslag voor het onderhouden van uw VHD's. Houd rekening met de volgende punten bij het plannen van de opslaglocatie van uw VHD's:

- Het doel Premium opslag-account.
- De opslaglocatie van het account moet hetzelfde als de Premium-opslag kunnen Azure VMs u tijdens de laatste fase wilt maken. U kan kopiëren naar een nieuwe opslag-account of -abonnement met de dezelfde opslag-account op basis van uw behoeften voldoet.
- Kopiëren en opslaan van de accountsleutel opslag van het account van de opslagruimte bestemming voor de volgende fase.

Voor gegevensschijven, kunt u sommige gegevensschijven in een standaard-opslag-account (bijvoorbeeld schijven met lagere opslag) behouden, maar we raden u alle gegevens voor productie werkbelasting premium opslag gebruiken verplaatsen.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>Stap 3. VHD met AzCopy of PowerShell kopiëren

U moet uw container pad en opslag accountsleutel verwerkingstijd van een van de volgende twee opties zoeken. Container pad en opslag accountsleutel vindt u in de **Portal van Azure** > **opslag**. De container-URL is bijvoorbeeld "https://myaccount.blob.core.windows.net/mycontainer/".

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>Optie 1: Een VHD met AzCopy (asynchroon kopie) kopiëren

AzCopy gebruikt, kunt u eenvoudig uploaden de VHD via Internet. Afhankelijk van de grootte van de VHD's, kan dit tijd duren. Onthoud dat de opslaglimieten voor account ingress/egress controleren als deze optie gebruikt. Zie [Azure opslag schaalbaarheid en prestaties doelen](storage-scalability-targets.md) voor meer informatie.

1. Download en installeer AzCopy taken: [meest recente versie van AzCopy](http://aka.ms/downloadazcopy)
2. Open Azure PowerShell en Ga naar de map waarin AzCopy is geïnstalleerd.
3. Gebruik de volgende opdracht uit het bestand VHD uit 'Bron' naar "Bestemming" te kopiëren.

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Voorbeeld:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd

    Hier volgen beschrijvingen van de parameters in de opdracht AzCopy gebruikt:

 - * */Gegevensbron: * &lt;bron&gt;:*** locatie van de map of de URL van de container opslag waarin de VHD.
 - * */SourceKey: * &lt;bron-en-accountsleutel&gt;:*** accountsleutel opslag van het bron-opslag-account.
 - * */Dest: * &lt;bestemming&gt;:*** opslag container URL voor de VHD om te kopiëren.
 - * */DestKey: * &lt;dest accountsleutel&gt;:*** accountsleutel opslag van de bestemming opslag-account.
 - * */Patroon: * &lt;bestandsnaam&gt;:*** Geef de naam van de VHD om te kopiëren.

Zie [overbrengen van gegevens met het hulpprogramma AzCopy-opdrachtregelopties](storage-use-azcopy.md)voor meer informatie over het gebruik van AzCopy hulpmiddel.

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>Optie 2: Een VHD met PowerShell (gesynchroniseerde kopie) kopiëren

U kunt ook het VHD-bestand met de PowerShell-cmdlet Start-AzureStorageBlobCopy kopiëren. Gebruik de volgende opdracht uit op Azure PowerShell VHD kopiëren. Vervang de waarden in <> door de betreffende waarden uit de bron- en doeltabellen opslag-account. Als u wilt deze opdracht gebruikt, moet u een zogenaamd VHD's in uw account van de opslagruimte doel hebben. Als de container niet bestaat, maakt u een voordat u de opdracht uitvoert.

    $sourceBlobUri = <source-vhd-uri>

    $sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

    $destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

    Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext

Voorbeeld:

    C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

    C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

    C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

    C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext

### <a name="scenario2"></a>Scenario 2: "Ik ben migreren VMs vanaf andere platforms met Azure Premium Storage."

Als u VHD uit niet - Azure Cloudopslag naar Azure migreert, moet u eerst de VHD exporteren naar een lokale map. Het pad is voltooid bron van de lokale map waarin VHD handige is opgeslagen, en vervolgens AzCopy te uploaden naar Azure Storage.

#### <a name="step-1-export-vhd-to-a-local-directory"></a>Stap 1. VHD exporteren naar een lokale map

##### <a name="copy-a-vhd-from-aws"></a>Kopieer een VHD uit AWS

1. Als u AWS gebruikt, moet u het exemplaar EC2 exporteren naar een VHD in een Emmertje Amazon S3. Volg de stappen beschreven in de documentatie Amazon voor Amazon EC2 exemplaren voor het installeren van de functie van de opdrachtregel-interface (CLI) Amazon EC2 en voert u de opdracht maken-exemplaar-exporteren-taak het exemplaar EC2 exporteren naar een VHD-bestand exporteren. Zorg ervoor dat u **VHD** gebruiken voor de schijf & #95; afbeelding & #95; OPMAAK variabele wanneer u de opdracht **maken-exemplaar-exporteren-taak** . Het geëxporteerde VHD-bestand is opgeslagen in de Amazon S3 Emmertje u tijdens dat proces wijst.

        aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
        --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX

2. Het VHD-bestand downloaden via het emmertje S3. Selecteer het VHD-bestand, klikt u vervolgens **Acties** > **downloaden**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Een VHD kopiëren vanuit andere niet-Azure-cloud

Als u VHD uit niet - Azure Cloudopslag naar Azure migreert, moet u eerst de VHD exporteren naar een lokale map. Kopieer het bronpad voltooid van de lokale map waarin VHD is opgeslagen.

##### <a name="copy-a-vhd-from-on-premise"></a>Een VHD van on-premises kopiëren

Als u VHD vanuit een on-premises omgeving migreert, moet u het pad van de volledige bronbestanden waarin VHD is opgeslagen. Het bronpad kan een server of bestandsshare worden.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Stap 2. De bestemming voor uw VHD maken

Maak een account opslag voor het onderhouden van uw VHD's. Houd rekening met de volgende punten bij het plannen van de opslaglocatie van uw VHD's:

- Het doel-opslag-account kan worden standaard- of premium opslagruimte afhankelijk van het vereiste voor uw toepassing.
- Het gebied van de account opslag moet hetzelfde als de Premium-opslag kunnen Azure VMs u tijdens de laatste fase wilt maken. U kan kopiëren naar een nieuwe opslag-account of -abonnement met de dezelfde opslag-account op basis van uw behoeften voldoet.
- Kopiëren en opslaan van de accountsleutel opslag van het account van de opslagruimte bestemming voor de volgende fase.

Wij raden u alle gegevens voor productie werkbelasting premium opslag gebruiken verplaatsen.

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a>Stap 3. De VHD uploaden naar Azure opslag

Nu dat u uw VHD in de lokale adreslijst hebt, kunt u AzCopy of AzurePowerShell het VHD-bestand uploaden naar Azure Storage. Beide opties worden hier beschreven:

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a>Optie 1: Gebruik van Azure PowerShell toevoegen-AzureVhd het VHD-bestand te uploaden

    Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>

Een voorbeeld <Uri> **_'https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd'_**mogelijk zijn. Een voorbeeld <FileInfo> **_'C:\path\to\upload.vhd'_**mogelijk zijn.

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a>Optie 2: AzCopy gebruiken om de VHD-bestand te uploaden

AzCopy gebruikt, kunt u eenvoudig uploaden de VHD via Internet. Afhankelijk van de grootte van de VHD's, kan dit tijd duren. Onthoud dat de opslaglimieten voor account ingress/egress controleren als deze optie gebruikt. Zie [Azure opslag schaalbaarheid en prestaties doelen](storage-scalability-targets.md) voor meer informatie.

1. Download en installeer AzCopy taken: [meest recente versie van AzCopy](http://aka.ms/downloadazcopy)
2. Open Azure PowerShell en Ga naar de map waarin AzCopy is geïnstalleerd.
3. Gebruik de volgende opdracht uit het bestand VHD uit 'Bron' naar "Bestemming" te kopiëren.

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Voorbeeld:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd

    Hier volgen beschrijvingen van de parameters in de opdracht AzCopy gebruikt:

 - * */Gegevensbron: * &lt;bron&gt;:*** locatie van de map of de URL van de container opslag waarin de VHD.
 - * */SourceKey: * &lt;bron-en-accountsleutel&gt;:*** accountsleutel opslag van het bron-opslag-account.
 - * */Dest: * &lt;bestemming&gt;:*** opslag container URL voor de VHD om te kopiëren.
 - * */DestKey: * &lt;dest accountsleutel&gt;:*** accountsleutel opslag van de bestemming opslag-account.
 - **/BlobType: pagina:** Geeft aan dat de bestemming een blob pagina.
 - * */Patroon: * &lt;bestandsnaam&gt;:*** Geef de naam van de VHD om te kopiëren.

Zie [overbrengen van gegevens met het hulpprogramma AzCopy-opdrachtregelopties](storage-use-azcopy.md)voor meer informatie over het gebruik van AzCopy hulpmiddel.

##### <a name="other-options-for-uploading-a-vhd"></a>Andere opties voor het uploaden van een VHD

U kunt ook een VHD uploaden naar uw opslag-account met een van de volgende manieren:

- [Azure opslag kopie Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure opslag Explorer uploaden BLOB 's](https://azurestorageexplorer.codeplex.com/)
- [Opslag importeren/exporteren Service REST API verwijzing](https://msdn.microsoft.com/library/dn529096.aspx)

>[AZURE.NOTE] Het is raadzaam om met behulp van de Service importeren/exporteren als geschatte tijd uploaden is langer dan zeven dagen. U kunt [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) gebruiken voor het schatten van de tijd van de eenheid voor het formaat en de overdracht van gegevens.
>
> Importeren/exporteren kan worden gebruikt om te kopiëren naar een standaard-opslag-account. U moet kopiëren van standaard opslag naar premium opslag account met behulp van een hulpmiddel zoals AzCopy.


## <a name="create-azure-virtual-machine-using-premium-storage"></a>Azure VMs Premium opslag maken

Nadat de schijf is geüpload of worden gekopieerd naar het gewenste opslag-account, volgt u de instructies in deze sectie om de VHD registreren als een afbeelding van de OS of OS schijf afhankelijk van uw scenario en maak vervolgens een exemplaar VM hieruit. De gegevensschijf VHD kan worden gekoppeld aan de VM zodra deze is gemaakt. Een migratie voorbeeldscript aan het einde van deze sectie. Dit eenvoudige script niet overeenkomen met alle scenario's. Mogelijk moet u het script om aan te passen met uw specifieke scenario bijwerken. Als dit script voor uw scenario geldt, raadpleegt u hieronder [Een migratie voorbeeldscript](#a-sample-migration-script).

### <a name="checklist"></a>Controlelijst

1.  Wacht totdat alle VHD schijven kopiëren is voltooid.
2.  Controleer of de dat Premium-opslag is beschikbaar in de regio die u naar migreert.
3.  Bepaal de nieuwe VM reeks die u wilt gebruiken. Moet een Premium-opslag staat en de grootte moet zijn afhankelijk van de beschikbaarheid in de regio en op basis van uw behoeften voldoet.
4.  Bepaal de exacte VM grootte die u wilt gebruiken. VM grootte moet groot genoeg is ter ondersteuning van het aantal gegevensschijven die u hebt. Bijvoorbeeld Als u 4 gegevensschijven hebt, moet de VM 2 of meer cores hebben. U kunt wellicht ook processing power, geheugen en netwerkbandbreedte nodig heeft.
5.  Maak een account Premium-opslag in de doelregio. Dit is het account dat u voor de nieuwe VM gebruiken wilt.
6.  De huidige VM details handige, met inbegrip van de lijst met schijven en bijbehorende VHD BLOB's hebben.

Uw toepassing voor downtime voorbereiden. Als u wilt een schone migratie uitvoeren, moet u alle verwerking stoppen in het huidige systeem. Alleen vervolgens krijgt u deze naar consistente status die u naar het nieuwe platform migreren kunt. Downtime duur hangt voor de hoeveelheid gegevens in de schijven om te migreren.

>[AZURE.NOTE] Als u een Azure resourcemanager VM vanaf een gespecialiseerde VHD schijf maakt, raadpleegt u aan [deze sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) voor het implementeren van resourcemanager VM met bestaande schijf.

### <a name="register-your-vhd"></a>Uw VHD registreren

Een VM OS VHD maken of een gegevensschijf aan een nieuwe VM hebt toegevoegd, moet u deze eerst registreren. Volg de onderstaande stappen afhankelijk van uw VHD scenario.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Generalized besturingssysteem VHD meerdere exemplaren van Azure VM maken

Nadat generalized OS afbeelding VHD wordt geüpload naar de opslag-account, registreren als een **Afbeelding van Azure VM** zodat u een of meer exemplaren van VM vanuit deze maken kunt. Gebruik de volgende PowerShell-cmdlets voor het registreren van uw VHD als een afbeelding Azure VM OS. Geef de URL van de volledige container waar VHD is gekopieerd naar.

    Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows

Kopiëren en de naam van deze nieuwe Azure VM afbeelding opslaan. In het bovenstaande voorbeeld is het *OSImageName*.

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Unieke besturingssysteem VHD een enkel exemplaar van de Azure VM maken

Nadat de unieke OS VHD wordt geüpload naar de opslag-account, registreren als een **Azure OS schijf** zodat u een exemplaar VM vanuit deze maken kunt. Gebruik deze PowerShell-cmdlets voor het registreren van uw VHD als een Azure OS schijf. Geef de URL van de volledige container waar VHD is gekopieerd naar.

    Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"

Kopiëren en de naam van deze nieuwe Azure OS schijf opslaan. In het bovenstaande voorbeeld is het *OSDisk*.

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a>Gegevens schijf VHD moet worden toegevoegd aan nieuwe Azure VM stijlen

Nadat de gegevensschijf VHD wordt geüpload naar opslag-account, registreren als een schijf Azure-gegevens zodat deze kan worden gekoppeld aan uw nieuwe DS-reeks, DSv2 reeks of GS reeks Azure VM exemplaar.

Gebruik deze PowerShell-cmdlets voor het registreren van uw VHD als een schijf Azure-gegevens. Geef de URL van de volledige container waar VHD is gekopieerd naar.

    Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"

Kopiëren en de naam van deze nieuwe schijf van de Azure-gegevens opslaan. In het bovenstaande voorbeeld is het *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Een Premium-opslag kunnen VM maken

Wanneer de afbeelding OS of OS schijf zijn geregistreerd, maak een nieuwe DS-reeks, DSv2-reeks of GS-reeks VM. U gebruikt de besturingssysteem afbeelding of het besturingssysteem schijfnaam die u hebt geregistreerd. Selecteer het type VM uit de Premium-opslag laag. In het onderstaande voorbeeld gebruiken we de grootte van de VM *Standard_DS2* .

>[AZURE.NOTE] De grootte van de schijf om ervoor te zorgen dat dit overeenkomt met uw capaciteit en prestaties vereisten en de grootte van de Azure schijfruimte bijwerken.

Volg de stapsgewijze PowerShell-cmdlets hieronder voor het maken van de nieuwe VM. Eerst de algemene parameters instellen:

    $serviceName = "yourVM"
    $location = "location-name" (e.g., West US)
    $vmSize ="Standard_DS2"
    $adminUser = "youradmin"
    $adminPassword = "yourpassword"
    $vmName ="yourVM"
    $vmSize = "Standard_DS2"

Maak eerst een cloudservice waarin u uw nieuwe VMs hosten.

    New-AzureService -ServiceName $serviceName -Location $location

Maak vervolgens het exemplaar Azure VM uit het OS afbeelding of het besturingssysteem schijf die u hebt geregistreerd afhankelijk van uw scenario.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Generalized besturingssysteem VHD meerdere exemplaren van Azure VM maken

De een of meer nieuwe DS reeks Azure VM exemplaren maken gebruik van de **Azure OS afbeelding** die u hebt geregistreerd. Geef de naam van deze OS afbeelding in de configuratie VM bij het maken van nieuwe VM, zoals hieronder wordt weergegeven.

    $OSImage = Get-AzureVMImage –ImageName "OSImageName"

    $vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

    Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

    New-AzureVM -ServiceName $serviceName -VM $vm

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Unieke besturingssysteem VHD een enkel exemplaar van de Azure VM maken

Een nieuwe Azure VM instantie voor DS reeks met de **Azure OS schijf** die u hebt geregistreerd maken. Geef de naam van deze OS schijf in de configuratie VM bij het maken van de nieuwe VM, zoals hieronder wordt weergegeven.

    $OSDisk = Get-AzureDisk –DiskName "OSDisk"

    $vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

    New-AzureVM -ServiceName $serviceName –VM $vm

Geef andere VM Azure-gegevens, zoals een cloudservice, regio, opslag-account, beschikbaarheid instellen en in de cache beleid. Houd er rekening mee dat het exemplaar VM met die gekoppeld besturingssysteem of gegevensschijven, zijn moet zodat het geselecteerde cloud-service, regio en opslag account op dezelfde locatie als het onderliggende VHD's van deze schijven zijn moet.

### <a name="attach-data-disk"></a>Gegevensschijf hebt toegevoegd

Ten slotte, als u gegevensschijf VHD's hebt geregistreerd, koppel deze aan de nieuwe Premium opslagruimte nodig Azure VM.

Gebruikt u de volgende PowerShell-cmdlet voor het koppelen van gegevensschijf voor de nieuwe VM en het beleid voor het in de cache opgeven. Klik in het onderstaande voorbeeld worden de in de cache beleid is ingesteld op *alleen-lezen*.

    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

    Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

    Update-AzureVM  -VM $vm

>[AZURE.NOTE] Er zijn mogelijk extra stappen nodig voor de ondersteuning van uw toepassing die is niet vallen deze handleiding.

### <a name="checking-and-plan-backup"></a>Controleren en back-up plannen

Zodra de nieuwe VM actief is, kunt u met de dezelfde aanmeldings-id en wachtwoord is als het oorspronkelijke VM en controleer of dat alles werkt zoals verwacht. Alle instellingen, met inbegrip van de volgende hoeveelheden, zou aanwezig zijn in de nieuwe VM.

De laatste stap is het plannen van back-up en onderhoudsplanning voor de nieuwe VM op basis van de behoeften van de toepassing.

### <a name="a-sample-migration-script"></a>Een voorbeeldscript voor de migratie

Als er meerdere VMs om te migreren, wordt automatisering via PowerShell-scripts handig zijn. Hier volgt een voorbeeldscript waarmee de migratie van een VM. Opmerking die onder script alleen een voorbeeld is en er zijn enkele aanbevelingen over de huidige VM schijven. Mogelijk moet u het script om aan te passen met uw specifieke scenario bijwerken.

De hypothesen zijn:

- Klassieke Azure VMs die u maakt.
- Uw bron OS schijven en de bron gegevensschijven zijn in hetzelfde opslag-account en dezelfde container. Als uw OS schijven en gegevensschijven niet op dezelfde plaats, kunt u AzCopy of Azure PowerShell opslag accounts en containers worden VHD's overschreven. Verwijzen naar de vorige stap: [Kopie VHD met AzCopy of PowerShell](#copy-vhd-with-azcopy-or-powershell). Een andere optie voor het bewerken van dit script om te voldoen aan uw scenario is, maar het is raadzaam via AzCopy of PowerShell nadat het is eenvoudiger en sneller.

Hieronder vindt u het automatiseringsscript. Tekst vervangen door uw gegevens en het script om aan te passen met uw specifieke scenario bijwerken.

>[AZURE.NOTE] Met het bestaande script, behoudt niet de configuratie van het netwerk van uw gegevensbron VM. U moet opnieuw-config de netwerkinstellingen op uw gemigreerde VMs.

    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED “AS IS” WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location “Southeast Asia”

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you’d like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the sourve vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more custimization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location

#### <a name="optimization"></a>Optimalisatie

De configuratie van uw huidige VM kan worden aangepast specifiek als u wilt werken ook met de standaard schijven. Bijvoorbeeld naar de prestaties verbeteren door u te veel schijven gebruiken in een gestreepte volume. Bijvoorbeeld, in plaats van 4 schijven afzonderlijk op Premium opslag, u mogelijk het optimaliseren van de kosten doordat één schijf. Optimalisaties toe, zoals deze moeten worden afgehandeld door geval en aangepaste stappen moeten na de migratie. Bedenk ook, dit proces mogelijk niet goed werken voor databases en toepassingen die afhankelijk van de indeling van de schijf gedefinieerd in de instellingen zijn.

##### <a name="preparation"></a>Voorbereiding van

1.  Voltooi de eenvoudige migratie zoals beschreven in de eerdere sectie. Optimalisaties moeten worden uitgevoerd op de nieuwe VM na de migratie.
2.  De nieuwe schijfgrootte die u nodig hebt voor de geoptimaliseerde configuratie definiëren.
3.  Hiermee bepaalt u de toewijzing van de huidige schijven/naar de nieuwe schijf specificaties.

##### <a name="execution-steps"></a>Stappen worden uitgevoerd

1.  Maak de nieuwe schijven met de juiste grootte op de Premium-opslag VM.
2.  Meld u aan bij de VM en kopieer de gegevens van het huidige volume naar de nieuwe diskette die is toegewezen aan dat volume. Deze stap herhalen voor de huidige hoeveelheden die moeten worden toegewezen aan een nieuwe schijf.
3.  Vervolgens de toepassingsinstellingen om te schakelen naar de nieuwe schijven wijzigen en loskoppelen van de oude hoeveelheden.

Zie [Prestaties optimaliseren](storage-premium-storage-performance.md#optimizing-application-performance)voor het optimaliseren van de toepassing voor betere schijfprestaties.

### <a name="application-migrations"></a>Toepassing migraties

Databases en andere complexe toepassingen kunnen speciale stappen vragen gedefinieerd door de toepassingsprovider voor de voor de migratie. Zie de documentatie desbetreffende toepassing. Bijvoorbeeld meestal databases kunnen worden gemigreerd tot en met back-up en herstellen.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende bronnen voor specifieke scenario's voor het migreren virtuele machines:

- [Migreren van Azure virtuele Machines tussen opslag-Accounts](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
- [Maken en uploaden van een Windows Server VHD naar Azure.](../virtual-machines/virtual-machines-windows-classic-createupload-vhd.md)
- [Maken en het uploaden van een virtuele harde schijf waarop het besturingssysteem Linux](../virtual-machines/virtual-machines-linux-classic-create-upload-vhd.md)
- [Migreren virtuele Machines van Amazon AWS naar Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Zie ook de volgende bronnen voor meer informatie over de opslag van Azure en Azure virtuele Machines:

- [Azure-opslag](https://azure.microsoft.com/documentation/services/storage/)
- [Azure virtuele Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)
- [Premium-opslag: High-Performance opslag voor Azure virtuele machines werkbelasting](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
