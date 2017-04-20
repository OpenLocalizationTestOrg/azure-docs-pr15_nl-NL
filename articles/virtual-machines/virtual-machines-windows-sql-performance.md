<properties
    pageTitle="Prestaties aanbevolen procedures voor SQL Server | Microsoft Azure"
    description="Aanbevolen procedures bevat voor het optimaliseren van de prestaties van SQL Server in Microsoft Azure VM's."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/07/2016"
    ms.author="jroth" />

# <a name="performance-best-practices-for-sql-server-in-azure-virtual-machines"></a>Prestaties aanbevolen procedures voor SQL Server in Azure virtuele Machines

## <a name="overview"></a>Overzicht

Dit onderwerp bevat aanbevolen procedures voor het optimaliseren van de prestaties van SQL Server in Microsoft Azure Virtual Machine. Tijdens het uitvoeren van SQL Server in Azure virtuele Machines, is het raadzaam dat u doorgaan met het dezelfde databaseprestaties optimaliseren van de opties die van toepassing op SQL Server in on-premises server-omgeving zijn. De prestaties van een relationele database in een openbare cloud is afhankelijk van veel factoren zoals de grootte van een virtuele machine en de configuratie van de gegevensschijven.

Bij het maken van SQL Server-afbeeldingen, [Houd rekening met het inrichten van uw VMs in de portal van Azure](virtual-machines-windows-portal-sql-server-provision.md). SQL Server VMs deze is ingericht in de Portal met resourcemanager Implementeer alle deze aanbevolen procedures, met inbegrip van de opslagconfiguratie.

In dit artikel is gericht op de *beste* prestaties voor SQL Server aan van Azure VMs. Als uw werkzaamheden minder vergt, kunt u elke optimalisatie hieronder ziet u mogelijk niet nodig. Houd rekening met de prestaties en de werklast patronen u deze aanbevelingen evalueren.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="quick-check-list"></a>Lijst met snelle selectievakjes

Hier volgt een lijst met snelle controle voor optimale prestaties van SQL Server op Azure virtuele Machines:

|Gebied|Optimalisaties|
|---|---|
|[VM grootte](#vm-size-guidance)|[DS3](virtual-machines-windows-sizes.md#standard-tier-ds-series) of hoger voor SQL Enterprise edition.<br/><br/>[DS2](virtual-machines-windows-sizes.md#standard-tier-ds-series) of hoger voor edities van SQL standaard- en Web.|
|[Opslag](#storage-guidance)|Gebruik de [Premium-opslag](../storage/storage-premium-storage.md). Standaard opslag wordt alleen aanbevolen voor ontwikkelaar/testen.<br/><br/>Houd de [opslag-account](../storage/storage-create-storage-account.md) en SQL Server VM in dezelfde regio.<br/><br/>Azure [geografische-redundante opslag](../storage/storage-redundancy.md) (geografische-replicatie) op het account opslag uitschakelen.|
|[Schijven](#disks-guidance)|Gebruik ten minste 2 [P30 schijven](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) (1 voor logboekbestanden; 1 voor gegevensbestanden en TempDB).<br/><br/>Vermijd het gebruik van besturingssysteem of tijdelijke schijven voor opslag van de database of logboekregistratie.<br/><br/>De lezen inschakelen op de schijven hostingprovider gegevensbestanden en TempDB caching.<br/><br/>Schakel geen caching op het logboekbestand hostingprovider schijven.<br/><br/>Belangrijk: De SQL Server-service stoppen bij het wijzigen van de cache-instellingen voor een schijf Azure VM.<br/><br/>Meerdere Azure gegevensschijven om grotere IO gegevensdoorvoer stripe.<br/><br/>Notatie met beschreven grootte van de toewijzing.|
|[I/O](#io-guidance)|Database pagina compressie inschakelen.<br/><br/>Schakel direct bestand initialisatie voor gegevensbestanden.<br/><br/>Beperkingen instellen voor in- of uitschakelen van autogrow voor de database.<br/><br/>Uitschakelen autoshrink voor de database.<br/><br/>Verplaats alle databases naar gegevensschijven, zoals systeemdatabases.<br/><br/>SQL Server-fout logboeken en doelcellen bestandsmappen naar gegevensschijven verplaatsen.<br/><br/>Standaard back-up en database bestandslocaties-instelling.<br/><br/>Vergrendelde pagina's inschakelen.<br/><br/>SQL Server prestaties reparaties toepassen.|
|[Functie specifiek](#feature-specific-guidance)|Back-up rechtstreeks met blob storage.|

Raadpleeg voor meer informatie over *hoe* en *Waarom* u deze optimalisaties, de details en de richtlijnen van de volgende secties.

## <a name="vm-size-guidance"></a>VM grootte richtlijnen

Het wordt aanbevolen dat u de volgende [virtuele machines grootten gebruikt](virtual-machines-windows-sizes.md)voor prestaties gevoelige toepassingen:

- **SQL Server Enterprise Edition**: DS3 of hoger

- **SQL Server Standard en Web edities**: DS2 of hoger


## <a name="storage-guidance"></a>Opslag richtlijnen

DS-reeks (samen met DSv2- en de reeks GS-) VMs ondersteuning [Premium opslag](../storage/storage-premium-storage.md). Premium opslag geschikt is voor alle werkbelasting in productie.

> [AZURE.WARNING] Standaard opslag heeft verschillende vertragingstijden en bandbreedte en alleen geschikt is voor de werkbelasting ontwikkelaar/testen. Productie werkbelasting moeten Premium opslag gebruiken.

Bovendien is het raadzaam dat u uw account Azure opslag maken in de dezelfde Datacenter als uw SQL Server virtuele machines doorverbinden vertragingen verkleinen. Wanneer u een account opslag maakt, geografische-replicatie uitschakelen als consistente schrijven volgorde over meerdere schijven is niet gegarandeerd. In plaats daarvan kunt configureren van een SQL Server-herstel-technologie voor noodgevallen tussen twee Azure datacenters. Zie [beschikbaarheid en herstel voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-high-availability-dr.md)voor meer informatie.

## <a name="disks-guidance"></a>Schijven richtlijnen

Er zijn drie belangrijkste schijftypen op een Azure-VM:

- **OS schijf**: wanneer u een Azure virtuele machines maakt, het platform ten minste één schijf (aangeduid met het station **C** ) voor VM voor uw besturingssysteem schijf koppelt. Deze schijf is een VHD opgeslagen als een pagina blob in opslag.
- **Tijdelijke schijf**: Azure virtuele Machines bevatten een andere schijf genoemd van de tijdelijke schijf (als de **D**: station). Dit is een schijf op het knooppunt dat kan worden gebruikt voor kladgebied ruimte.
- **Schijven van gegevens**: U kunt ook extra schijven toevoegen aan uw VM als gegevensschijven en deze wordt opgeslagen in opslag als BLOB van pagina's.

De volgende secties worden aanbevelingen voor het gebruik van deze verschillende schijven.

### <a name="operating-system-disk"></a>Besturingssysteem Schijfopruiming

Een schijf besturingssysteem is een VHD die u kunt starten en koppelen als een actieve versie van een besturingssysteem en is gelabeld als station **C** .

Standaard caching van beleid op de schijf besturingssysteem is **Alleen-lezen/schrijven**. Voor prestaties gevoelige toepassingen, wordt u aangeraden gegevensschijven te gebruiken in plaats van de schijf besturingssysteem gebruikt. Zie het gedeelte op gegevensschijven hieronder.

### <a name="temporary-disk"></a>Tijdelijke schijf

De schijf tijdelijke opslag, als de **D**: station, blijft niet behouden met Azure-blobopslag. Sla geen uw gebruiker-databasebestanden of de gebruiker transactie-logboekbestanden op de **D**: station.

Voor D-reeks, Dv2-reeks en G-serie VMs is het tijdelijke station op deze VMs SSD gebaseerde. Als uw werkzaamheden intensief gebruik van TempDB maakt (bijvoorbeeld voor tijdelijke objecten of complexe joins) TempDB opslaan op het station **D** kan leiden tot sneller TempDB worden verwerkt en lagere TempDB latentie.

Voor VMs die ondersteuning bieden voor Premium opslag (DS-reeks, DSv2-reeks en GS-reeks), wordt u aangeraden TempDB en/of Buffer groep extensies opslaan op een schijf die ondersteuning biedt voor Premium opslagruimte met lees ingeschakelde cache. Er is een uitzondering deze aanbeveling; Als uw gebruik TempDB veel schrijven is, kunt u betere prestaties kunt bereiken door op te slaan TempDB op het lokale station **D** , dat wil ook SSD-gebaseerd op de grootte van deze computer zeggen.

### <a name="data-disks"></a>Gegevensschijven

- **Gebruik van gegevensschijven voor gegevens en logboekbestanden**: ten minste 2 Premium opslag [P30 schijven](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) waarin één schijf bevat de logboekbestanden en de andere bevat de bestanden die gegevens en TempDB gebruiken.

- **Gesegmenteerd schijf te verdelen**: meer worden verwerkt, kunt u aanvullende gegevensschijven toevoegen en gebruiken van schijf gesegmenteerd te verdelen. Om te bepalen hoeveel gegevensschijven, moet u het aantal IO's / s beschikbaar voor uw gegevens- en logbestanden schijven analyseren. Zie voor informatie die de tabellen op IO's / s per [VM grootte](virtual-machines-windows-sizes.md) en schijf grootte in het volgende artikel: [Premium-opslag gebruiken voor schijven](../storage/storage-premium-storage.md). Gebruik de volgende deze richtlijnen:

    - Voor Windows 8/Windows Server 2012 of hoger, gebruikt u [Opslag spaties](https://technet.microsoft.com/library/hh831739.aspx). Stel Streepgrootte aan 64 KB voor OLTP werkbelasting en 256 KB voor magazijnbeheer werkbelasting gegevens wilt voorkomen dat van invloed op de prestaties vanwege partition uitlijning. Bovendien instellen aantal kolommen = aantal fysieke schijven. Als u wilt een opslagruimte configureren met meer dan 8 schijven moet u PowerShell (niet Serverbeheer UI) het aantal kolommen zodat deze overeenkomt met het aantal schijven expliciet instellen. Zie voor meer informatie over het configureren van [Opslag spaties](https://technet.microsoft.com/library/hh831739.aspx), [Opslag spaties Cmdlets in Windows PowerShell](https://technet.microsoft.com/library/jj851254.aspx)

    - Voor Windows 2008 R2 of eerder, kunt u dynamische schijven (OS gestreept volumes) en de Streepgrootte is altijd 64 KB. Houd er rekening mee dat deze optie is afgeschaft op Windows 8/Windows Server 2012. Zie de instructie ondersteuning bij [virtuele schijf-Service is overgang naar Windows opslag Management API](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx)voor informatie.

    - Als uw werkzaamheden niet log intensief is en niet speciale IO's / s hoeft, kunt u slechts één opslag van toepassingen kunt configureren. Anders maakt u twee opslag van toepassingen, één voor de logboekbestanden en een andere opslagruimte toepassingen voor de bestanden die gegevens en TempDB. Hiermee bepaalt u het aantal dat is gekoppeld aan elke opslag van toepassingen op basis van uw verwachtingen laden schijven. Houd er rekening mee dat verschillende VM grootte kunnen u verschillende getallen van gekoppelde gegevensschijven. Zie [grootten voor virtuele Machines](virtual-machines-windows-sizes.md)voor meer informatie.

    - Als u niet Premium opslag (ontwikkelaar/testen scenario's) gebruikt, wordt de aanbeveling is het maximum aantal gegevensschijven worden ondersteund door uw [VM grootte](virtual-machines-windows-sizes.md) toevoegen en gebruiken van schijf gesegmenteerd te verdelen.

- **Caching van beleid**: schijven van de gegevens voor de Premium-opslag, lees caching op de gegevensschijven hostingprovider gegevensbestanden en TempDB alleen inschakelen. Als u Premium-opslag niet gebruikt, Schakel geen eventuele caching op alle gegevensschijven. Zie voor instructies over het configureren van de schijfcaching, de volgende onderwerpen: [Set-AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) en [Set-AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx).

    >[AZURE.WARNING] De SQL Server-service stoppen bij het wijzigen van de cache-instelling van Azure VM schijven om te voorkomen dat de mogelijkheid van beschadigingen van de database.

- **Grootte van de toewijzing NTFS**: wanneer u de gegevensschijf opmaakt, het wordt aanbevolen dat u de grootte van een toewijzing van 64 KB per eenheid voor gegevens en logboekbestanden, evenals TempDB gebruiken.

- **Best practices voor beheer van schijf**: wanneer een gegevensschijf verwijderen of wijzigen van het cachetype de SQL Server-service stoppen tijdens de wijziging. Wanneer de in de cache-instellingen op de schijf OS worden gewijzigd, Azure is, wordt de VM stopt, het cachetype wijzigen en opnieuw is opgestart de VM. Wanneer de cache-instellingen van een gegevensschijf worden gewijzigd, wordt de VM niet is gestopt, maar de gegevensschijf is losgekoppeld van de VM tijdens de wijziging en klik vervolgens opnieuw.

    >[AZURE.WARNING] Fout bij de SQL Server-service stoppen tijdens deze bewerkingen kan leiden tot de database beschadigde bestanden.

## <a name="io-guidance"></a>I/o-richtlijnen

- De beste resultaten met Premium opslagmedia bereikt wanneer u uw toepassing en aanvragen parallelize. Premium opslag is bedoeld voor scenario's waarin de IO wachtrijdiepte groter dan 1, is zodat u weinig of geen prestatieverbeteringen voor één thread seriële aanvragen ziet (ook als deze opslag intensief zijn). Bijvoorbeeld, kan dit voor invloed op de single thread-testresultaten van prestaties hulpprogramma's voor gegevensanalyse, zoals SQLIO.

- Overweeg het gebruik van [database pagina compressie](https://msdn.microsoft.com/library/cc280449.aspx) terwijl deze prestaties van i/o-intensief werkbelasting kunt verbeteren. De gegevenscompressie mogelijk echter de processorgebruik op de databaseserver verhogen.

- Overweeg om direct bestand initialisatie verkleinen van de tijd die is vereist voor de bestandstoewijzing van het oorspronkelijke te. Om te profiteren van direct bestand initialisatie, moet u de serviceaccount van SQL Server (MSSQLSERVER) met SE_MANAGE_VOLUME_NAME verlenen en toe te voegen aan het beveiligingsbeleid **Volumeonderhoudstaken uitvoeren** . Als u de afbeelding van een SQL Server-platform voor Azure gebruikt, wordt het standaardaccount voor service (NT Service\MSSQLSERVER) is niet toegevoegd aan het beveiligingsbeleid **Volumeonderhoudstaken uitvoeren** . Met andere woorden, is direct bestand initialisatie niet ingeschakeld in de afbeelding van een SQL Server Azure-platform. Nadat de SQL Server-account is toegevoegd aan het beveiligingsbeleid **Volumeonderhoudstaken uitvoeren** , de SQL Server-service opnieuw te starten. Er zijn beveiligingsoverwegingen voor het gebruik van deze functie. Zie voor meer informatie [Initialisatie van de Database-bestand](https://msdn.microsoft.com/library/ms175935.aspx).

- **autogrow** wordt beschouwd als alleen een gebeurtenis voor onverwachte groei. De groei van uw gegevens- en logbestanden dagelijks met autogrow niet beheren. Als autogrow wordt gebruikt, vooraf het bestand met de schakeloptie grootte te vergroten.

- Zorg ervoor dat **autoshrink** om te voorkomen dat overbodige belasting die een negatieve invloed kan zijn op de prestaties is uitgeschakeld.

- Verplaats alle databases naar gegevensschijven, zoals systeemdatabases. Zie [System-Databases verplaatsen](https://msdn.microsoft.com/library/ms345408.aspx)voor meer informatie.

- SQL Server-fout logboeken en doelcellen bestandsmappen naar gegevensschijven verplaatsen. Dit is mogelijk in SQL Server Configuration Manager met de rechtermuisknop op uw SQL Server-instantie eigenschappen te selecteren. Instellingen van de fout logboeken en doelcellen kunnen worden gewijzigd op het tabblad **Opstartparameters** . De map Dump is opgegeven in het tabblad **Geavanceerd** . De volgende schermafbeelding ziet u waar de parameter error log opstarten.

    ![Schermafbeelding van de SQL-foutenlogboek](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

- Standaard back-up en database bestandslocaties-instelling. Gebruik van de aanbevelingen in dit onderwerp en breng de wijzigingen in het venster Server eigenschappen. Zie voor instructies voor het [weergeven of wijzigen de standaard-locaties voor gegevens en logboekbestanden (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). De volgende schermafbeelding ziet u waar u deze wijzigingen aanbrengen.

    ![Logboekbestanden van SQL-gegevens en back-up maken](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)

- Vergrendelde pagina's te beperken IO en eventuele activiteiten Paginering inschakelen. Zie [inschakelen van de pagina's vergrendelen in het geheugen optie (Windows)](https://msdn.microsoft.com/library/ms190730.aspx)voor meer informatie.

- Als u SQL Server 2012 worden uitgevoerd, installeert u Service Pack 1 cumulatieve Update 10. Deze update bevat de correctie voor slechte prestaties in I/O tijdens het uitvoeren van Selecteer into, instructie tijdelijke tabel in SQL Server 2012. Zie dit [knowledge base-artikel](http://support.microsoft.com/kb/2958012)voor informatie.

- Houd rekening met eventuele gegevensbestanden comprimeren bij het verplaatsen van in of uit van Azure.

## <a name="feature-specific-guidance"></a>Functie voor specifieke richtlijnen

Sommige implementaties mogelijk extra prestatievoordelen met meer geavanceerde configuratie-technieken bereiken. De volgende lijst worden enkele SQL Server-functies die u helpen kunnen bij het bereiken van betere prestaties gemarkeerd:

- **Back-up met Azure storage**: als back-ups maken voor SQL Server in Azure virtuele machines wordt uitgevoerd, kunt u [SQL Server back-up op URL](https://msdn.microsoft.com/library/dn435916.aspx). Deze functie is beschikbaar vanaf SQL Server 2012 SP1 CU2 en aanbevolen voor reservekopieën maken op de gegevensschijven gekoppelde. Wanneer u back-up/herstellen van Azure opslag, volgt u de aanbevelingen gegeven aan [SQL Server back-URL aanbevolen procedures en probleemoplossing en terugzetten van back-ups die zijn opgeslagen in de opslagruimte van de Azure](https://msdn.microsoft.com/library/jj919149.aspx). U kunt ook deze back-ups met [Back-up wordt automatisch voor SQL Server in Azure virtuele Machines](virtual-machines-windows-classic-sql-automated-backup.md)automatiseren.

    Voordat u SQL Server 2012, kunt u [SQL Server back-up hulpprogramma Azure](https://www.microsoft.com/download/details.aspx?id=40740). Dit hulpmiddel kunt u helpen om uit te breiden back-up doorvoer met meerdere back-up streep doelen.

- **SQL Server-gegevensbestanden in Azure wordt aangegeven**: deze nieuwe functie, [SQL Server-gegevensbestanden in Azure wordt aangegeven](https://msdn.microsoft.com/library/dn385720.aspx), is beschikbaar vanaf SQL Server-2014. Met SQL Server met gegevensbestanden in Azure wordt aangegeven, ziet u vergelijkbare prestatie-eigenschappen als met Azure gegevensschijven.

## <a name="next-steps"></a>Volgende stappen

Als u geïnteresseerd bent in SQL Server en Premium opslagruimte uitgebreider, raadpleegt u het artikel [Gebruik Azure Premium opslagruimte met SQL Server op virtuele Machines](virtual-machines-windows-classic-sql-server-premium-storage.md).

Zie voor aanbevolen procedures voor beveiliging, [Beveiligingsoverwegingen voor SQL Server in Azure virtuele Machines](virtual-machines-windows-sql-security.md).

Andere onderwerpen die voor SQL Server-VM op [SQL Server op Azure virtuele Machines overzicht](virtual-machines-windows-sql-server-iaas-overview.md)bekijken.
