<properties
    pageTitle="VMware virtuele machines en fysieke servers repliceren naar Azure met Azure Site herstel | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe herstel van Azure-Site om de goedkeuringen herhaling, failover en herstel van on-premises implementatie VMware virtuele machines en Windows/Linux fysieke servers Azure implementeren."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery"></a>VMware virtuele machines en fysieke servers repliceren naar Azure met Azure sites worden hersteld

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmware-to-azure.md)
- [Klassieke Portal](site-recovery-vmware-to-azure-classic.md)
- [Klassieke-Portal (verouderd)](site-recovery-vmware-to-azure-classic-legacy.md)


De Site herstel van Azure-service draagt bij aan uw strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel (BCDR) door orchestrating herhaling, failover en herstel van virtuele machines en fysieke servers. Computers kunnen worden gerepliceerd naar Azure of naar een secundaire on-premises implementatie-Datacenter. Voor een kort overzicht lezen [Wat Azure Site herstel is?](site-recovery-overview.md).

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe u:

- **VMware repliceren virtuele machines naar Azure**, Site-herstel te coördineren herhaling, failover en herstel van on-premises implementatie VMware virtuele machines met Azure storage implementeren.
- **Repliceren fysieke servers Azure**-implementatie van Azure Site herstel te coördineren herhaling, failover en herstel van on-premises implementatie fysieke Windows en Linux-servers Azure.

>[AZURE.NOTE] In dit artikel wordt beschreven hoe worden gerepliceerd naar Azure. Als u repliceren VMware VMs of Windows/Linux fysieke servers met een secundaire datacenter wilt, volgt u de instructies in [dit artikel](site-recovery-vmware-to-vmware.md).

Opmerkingen of vragen onder van dit artikel of op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)posten.

## <a name="enhanced-deployment"></a>Verbeterde implementatie

In dit artikel bevat instructies voor een uitgebreide implementatie in de klassieke Azure-portal bevat. Het is raadzaam om dat u deze versie gebruiken voor alle nieuwe implementaties. Als u al hebt geïmplementeerd met de eerdere versie van de oudere wordt u aangeraden waarop u naar de nieuwe versie migreren. Lees [meer](site-recovery-vmware-to-azure-classic-legacy.md##migrate-to-the-enhanced-deployment) over migratie.

De uitgebreide implementatie is bijgewerkt. Hier volgt een overzicht van de verbeteringen die we hebt aangebracht:

- **Geen infrastructuur VMs in Azure wordt aangegeven**: gegevens wordt overgenomen door rechtstreeks bij een account Azure opslag. Daarnaast voor replicatie en overname bij storing bestaat niet nodig voor het instellen van een infrastructuur VMs (configuratieserver, basispagina doelserver) zo we nodig in de oudere implementatie.  
- **Geïntegreerde installatie**: één installatie biedt eenvoudige instellingen en schaalbaarheid voor on-premises implementatie onderdelen.
- **Secure implementatie**: al het verkeer is versleuteld en herhaling management communicatie via HTTPS 443 worden verzonden.
- **Herstel wordt verwezen**: ondersteuning voor vastlopen en toepassing consistente herstel verwijst voor Windows en Linux-omgevingen en ondersteuning biedt voor zowel één VM en multi-VM consistente configuraties.
- **Failover testen**: ondersteuning voor ononderbroken test failover naar Azure, zonder die invloed hebben op productie of onderbreken herhaling.
- **Niet-geplande failover**: ondersteuning voor niet-geplande failover naar Azure met een uitgebreide optie VMs automatisch afgesloten voordat failover wordt uitgevoerd.
- **Failback**: geïntegreerde failback die wordt overgenomen door alleen deltawijzigingen terug naar de on-premises implementatie-site.
- **vSphere 6.0**: beperkte ondersteuning voor VMware Vsphere 6.0 implementaties.


## <a name="how-does-site-recovery-help-protect-virtual-machines-and-physical-servers"></a>Hoe kan Site herstel beter beveiligen virtuele machines en fysieke servers?


- VMware beheerders kunnen externe beveiliging Azure van bedrijven werkbelasting en toepassingen op VMware virtuele machines configureren. Managers voor de server kunnen fysiek on-premises implementatie Windows en Linux-servers gerepliceerd naar Azure.
- De Site herstel van Azure-console biedt één enkele locatie voor eenvoudige instellen en beheren van replicatie, failover, en het herstelproces.
- Als u VMware virtuele machines die worden beheerd door een server vCenter repliceren, kunnen sites worden hersteld die VMs automatisch detecteren. Als computers zich in een host ESXi ontdekt Site herstel VMs op de host.
- Eenvoudig failovers uitvoeren vanaf de infrastructuur van uw on-premises implementatie naar Azure en foutherstel (herstellen) van Azure met VMware VM servers in de on-premises implementatie-site.
- Herstel-abonnementen die toepassing werkbelasting die worden doorverbonden op meerdere computers groeperen configureren. U kunt niet via deze plannen en sites worden hersteld consistentie multi-VM zodat computers waarop de dezelfde werkbelasting samen aan een punt consistente gegevens kunnen worden hersteld.


## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

### <a name="windows64-bit-only"></a>Windows (alleen 64 bits)
- Windows Server 2008 R2 SP1 +
- Windows Server 2012
- Windows Server 2012 R2

### <a name="linux-64-bit-only"></a>Linux (alleen 64 bits)
- Rode rol Enterprise Linux 6,7, 7.1, 7.2
- CentOS 6.5, 6.6, 6,7, 7.0, 7.1, 7.2
- Oracle Enterprise Linux 6.4, 6.5 waarop rood rol compatibele kernel of Unbreakable Enterprise Kernel Release 3 (UEK3)
- SUSE Linux Enterprise Server 11 SP3

## <a name="scenario-architecture"></a>Scenario-architectuur

Scenario-onderdelen:

- **Een on-premises implementatie management server**: de server wordt uitgevoerd herstel van de Site-onderdelen:
    - **Configuratieserver**: coördinaten communicatie en processen voor het replicatie en herstel van gegevens beheren.
    - **Processerver**: fungeert als een gateway herhaling. Deze gegevens worden ontvangen van beveiligde bron machines, optimaliseert met caching, compressie en versleuteling en herhaling gegevens verzendt naar Azure opslag. Ook omgaat met push-installatie van service mobiliteit voor beveiligde computers en automatische detectie van VMware VMs uitvoert.
    - **Basispagina doelserver**: herhaling gegevens tijdens failback van Azure verwerkt.
    U kunt ook een management server die als de processerver van een alleen fungeert als u wilt schalen dat uw implementatie implementeren.
- **De mobiliteit service**: dit onderdeel wordt geïmplementeerd op elke computer (VMware VM of fysieke server) die u wilt repliceren naar Azure. Er wordt vastgelegd schrijft gegevens op de computer en ze doorstuurt naar de processerver.
- **Azure**: U hoeft te maken van een VMs Azure om af te handelen herhaling en overname bij storing. De Site herstel-service beheren van gegevens worden verwerkt en gegevens wordt overgenomen door rechtstreeks Azure opslag. Gerepliceerde Azure VMs worden automatisch omhoog of alleen bij een storing naar Azure. Echter als u wilt mislukken van Azure terug naar de site on-premises implementatie moet u voor het instellen van een VM Azure fungeert als een processerver.


De afbeelding ziet u de manier waarop deze onderdelen werken.

![architectuur](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Afbeelding 1: VMware/fysieke naar Azure** (gemaakt Henry Robalino)


## <a name="capacity-planning"></a>Capaciteit plannen

Wanneer u van plan bent capaciteit, als volgt van wat u nodig hebt om na te denken over:

- **De bronomgeving**, capaciteit plannings- of van de VMware infrastructuur en de bron de vereisten van de computer.
- **De server management**, Planning voor de servers voor het beheer van on-premises implementatie die Site herstel onderdelen uitvoeren.
- **Netwerkbandbreedte uit de bron naar doel**-Planning voor netwerkbandbreedte nodig is voor replicatie tussen het bron- en Azure

### <a name="source-environment-considerations"></a>Aandachtspunten voor de bron-omgeving

- **Dagelijks maximum snelheid wijzigen**, een beveiligde machine alleen de beschikking over één processerver en een enkel proces-server kan maximaal 2 TB aan gegevens wijzigen per dag verwerken. 2 TB is dus dat de maximale dagelijkse gegevens rente die wordt ondersteund voor een beveiligde machine wijzigen.
- **Maximum doorvoer**, een gerepliceerde machine kunt toevoegen aan één opslag-account in Azure wordt aangegeven. Een standaard-opslag-account kan maximaal 20.000 aanvragen per seconde worden verwerkt en het is raadzaam dat u het aantal IO's / s over een bron-machine aan 20.000 behouden. Voor voorbeeld als u een bron-machine met 5 schijven en elke schijf hebt gegenereerd 120 IO's / s (8 kB grootte) op de bron, wordt de parameter zijn binnen de Azure per schijf IO's / s limiet van 500. Het getal = totale bron IO's / s/20000 opslag-accounts die zijn vereist.


### <a name="management-server-considerations"></a>Overwegingen bij server

De server wordt uitgevoerd herstel van de Site-onderdelen die gegevens optimalisatie, replicatie en management verwerken. Er moet kunnen worden afgehandeld het dagelijkse wijzigen tarief capaciteit over alle werkbelasting in het beveiligde computers waarop en voldoende bandbreedte continu gegevens worden gerepliceerd naar Azure opslag heeft. Name:

- De processerver herhaling gegevens ontvangt van beveiligde machines en optimaliseert met caching, compressie en versleuteling voor verzenden naar Azure. De server moet hebben voldoende bronnen die u wilt deze taken uitvoeren.
- Het processerver gebruikt schijfcache. Het is raadzaam een afzonderlijke cache-dvd van 600 GB of meer wijzigingen aan de gegevens die zijn opgeslagen in het geval van netwerk knelpunt of -storing verwerken. Tijdens de implementatie kunt u de cache configureren op een schijf waarop ten minste 5 GB opslagruimte beschikbaar, maar 600 GB is de minimale aanbeveling.
- Als een goede gewoonte is het raadzaam dat de server management zich bevinden op de hetzelfde netwerk en LAN-segment als de machines die u wilt beveiligen. Dit kan zich bevinden in een netwerk met een ander maar machines die u wilt beveiligen L3 netwerk zichtbaarheid ernaar nodig hebt.

Grootte aanbevelingen voor de server worden in de volgende tabel samengevat.

**Management server processor** | **Geheugen** | **Cachegrootte Schijfopruiming** | **De snelheid van gegevens wijzigen** | **Beveiligde machines**
--- | --- | --- | --- | ---
8 vCPUs (2-sockets * 4 cores @ 2,5 GHz) | 16 GB | 300 GB | 500 GB of kleiner | Een management server met deze instellingen repliceren minder dan 100 machines implementeren.
12 vCPUs (2-sockets * 6 cores @ 2,5 GHz) | 18 GB | 600 GB | 500 GB tot 1 TB | Een management server met deze instellingen repliceren tussen 100-150 machines implementeren.
16 vCPUs (2-sockets * 8 cores @ 2,5 GHz) | 32 GB | 1 TB | 1 TB tot 2 TB | Een management server met deze instellingen repliceren tussen 150-200 machines implementeren.
Een ander processerver implementeren | | | > 2 TB | Aanvullende CMYK-servers implementeren als u meer dan 200 machines bent repliceren, of als de dagelijkse gegevens wijzigt tarief groter is dan 2 TB.

Waarbij geldt:

- Elke broncomputer is geconfigureerd met 3 schijven van 100 GB.
- We benchmarking opslag van 8 SA's stations van 10 K RPM gebruikt met RAID 10 voor cache schijf metingen.

### <a name="network-bandwidth-from-source-to-target"></a>Netwerkbandbreedte uit bron naar doelsite
Zorg ervoor dat u de bandbreedte die nodig om het eerste replicatie en delta herhaling met de [capaciteit teamplanner hulpmiddel](site-recovery-capacity-planner.md) berekenen

#### <a name="throttling-bandwidth-used-for-replication"></a>De bandbreedte gebruikt voor replicatie beperken

VMware verkeer gerepliceerd naar Azure doorloopt naar een specifieke processerver. U kunt de bandbreedte die beschikbaar is voor Site-herstel replicatie op die server als volgt beperken:

1. Open de module MMC van Microsoft Azure back-up op de belangrijkste management server of op een server management extra ingericht proces-servers. Standaard door een snelkoppeling voor Microsoft Azure back-up wordt gemaakt op bureaublad, of kunt u vinden in: C:\Program Files\Microsoft Azure herstel Services Agent\bin\wabadmin.
2. Klik op **Eigenschappen wijzigen**in de module.

    ![De bandbreedte](./media/site-recovery-vmware-to-azure-classic/throttle1.png)

3. Klik op het tabblad **Throttling** Geef de bandbreedte die kan worden gebruikt voor de toepassing plannen en sites worden hersteld herhaling.

    ![De bandbreedte](./media/site-recovery-vmware-to-azure-classic/throttle2.png)

U kunt desgewenst ook instellen via PowerShell beperken. Hier volgt een voorbeeld:

    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)

#### <a name="maximizing-bandwidth-usage"></a>Bandbreedtegebruik maximaliseren
Als u wilt verhogen van de bandbreedte gebruikt voor replicatie door Azure Site herstel moet u een registersleutel wijzigen.

De volgende sleutel bepaalt het aantal threads per schijf repliceren die worden gebruikt wanneer repliceren

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication\UploadThreadsPerVM

 In een netwerk 'overprovisioned' moet deze registersleutel worden gewijzigd van de standaardwaarden. Ondersteunen we een maximum van 32.  


[Meer informatie](site-recovery-capacity-planner.md) over het plannen van gedetailleerde capaciteit.

### <a name="additional-process-servers"></a>Aanvullende CMYK-servers

Als u wilt beveiligen van meer dan 200 machines of de snelheid van dagelijkse wijzigen groter is dat 2 TB kunt u toevoegen extra servers u omgaat met het selectievakje laden. Als u wilt verkleinen out u kunt u in het volgende doen:

- Het aantal servers voor het beheer vergroten. U kunt bijvoorbeeld maximaal 400 machines met twee management servers beveiligen.
- Aanvullende CMYK-servers toevoegen en typt u deze zo instellen dat verkeer in plaats van (of naast) worden de server.

Deze tabel worden een scenario waarbij beschreven:

- U instellen de oorspronkelijke management server als een configuratieserver te gebruiken.
- U instellen een extra processerver.
- U configureren beveiligde virtuele machines de aanvullende CMYK-server.
- Elke broncomputer beveiligde is geconfigureerd met drie schijven van 100 GB.

**Oorspronkelijke management server**<br/><br/>(configuratieserver) | **Aanvullende CMYK-server**| **Cachegrootte Schijfopruiming** | **De snelheid van gegevens wijzigen** | **Beveiligde machines**
--- | --- | --- | --- | ---
8 vCPUs (2-sockets * 4 cores @ 2,5 GHz), 16 GB geheugen vereist | 4 vCPUs (2-sockets * 2 cores @ 2,5 GHz), 8 GB geheugen vereist | 300 GB | 250 GB of kleiner | U kunt 85 of minder machines repliceren.
8 vCPUs (2-sockets * 4 cores @ 2,5 GHz), 16 GB geheugen vereist | 8 vCPUs (2-sockets * 4 cores @ 2,5 GHz), 12 GB geheugen vereist | 600 GB | 250 GB tot 1 TB | U kunt repliceren tussen 85-150 machines.
12 vCPUs (2-sockets * 6 cores @ 2,5 GHz), 18 GB geheugen vereist | 12 vCPUs (2-sockets * 6 cores @ 2,5 GHz) 24 GB geheugen vereist | 1 TB | 1 TB tot 2 TB | U kunt repliceren tussen 150-225 machines.


De manier waarop u uw servers schalen wordt, hangt af van uw voorkeur voor een schaal van of schalen out model.  U omhoog met het implementeren van een paar geavanceerde management en proces servers wilt verkleinen, of met het implementeren van meer servers met minder resources wilt verkleinen. Bijvoorbeeld: als u wilt beveiligen 220 machines kunt u een van de volgende kunt doen:

- De oorspronkelijke management server configureren met 12vCPU, 18 GB geheugen, een processerver extra met 12vCPU, 24 GB geheugen, en beveiligde machines alleen de aanvullende CMYK-server configureren.
- Of u kunt ook u kan twee management-servers (2 x 8vCPU, 16 GB RAM) en twee aanvullende CMYK-servers (1 x 8vCPU) en 4vCPU x 1 worden afgehandeld 135 + 85 (220) machines configureren en beveiligde machines als u wilt gebruiken, alleen de aanvullende CMYK-servers configureren.


[Volg deze instructies](#deploy-additional-process-servers) voor het instellen van een aanvullende CMYK-server.

## <a name="before-you-start-deployment"></a>Voordat u implementatie begint

De tabellen samenvatting maken van de vereisten voor de implementatie van dit scenario.


### <a name="azure-prerequisites"></a>Azure vereisten

**Vereiste** | **Meer informatie**
--- | ---
**Azure-account**| U moet een [Microsoft Azure](https://azure.microsoft.com/) -account. U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstelproces is Site prijzen.
**Azure-opslag** | U moet een Azure opslag-account voor de opslag gerepliceerde gegevens. Gerepliceerde gegevens worden opgeslagen in de opslagruimte van de Azure en Azure VMs zijn omhoog of bij een storing. <br/><br/>Hebt u een [standaard geografische-redundante opslag-account](../storage/storage-redundancy.md#geo-redundant-storage)nodig. Het account moet worden in hetzelfde gebied, als de Site herstel-service en zijn gekoppeld aan hetzelfde abonnement. Houd er rekening mee replicatie naar premium opslag accounts momenteel wordt niet ondersteund en niet mag worden gebruikt.<br/><br/>We het verplaatsen van opslag-accounts die zijn gemaakt met behulp van de [nieuwe portal van Azure](../storage/storage-create-storage-account.md) via resourcegroepen wordt niet ondersteund. [Lees over](../storage/storage-introduction.md) Azure opslagruimte.<br/><br/>
**Azure-netwerk** | U moet een Azure virtuele netwerk dat Azure VMs maakt verbinding met bij een storing. Het Azure virtuele netwerk moet zich in hetzelfde gebied, als de Site herstel kluis.<br/><br/>Opmerking mislukt terug na failover naar Azure u moet een VPN-verbinding (of Azure ExpressRoute) instellen vanuit het Azure netwerk naar de on-premises implementatie-site.


### <a name="on-premises-prerequisites"></a>Vereisten voor on-premises implementatie

**Vereiste** | **Meer informatie**
--- | ---
**Management server** | Moet u een on-premises implementatie Windows 2012 R2-server waarop een virtuele machine of fysieke server. Alle on-premises implementatie Site herstel onderdelen zijn geïnstalleerd op deze management server<br/><br/> Het is raadzaam om dat u de server als een hoge beschikbaarheid VMware VM implementeren. Failback naar de site on-premises implementatie van Azure is altijd worden VMware VMs ongeacht of u heeft overgenomen VMs of fysieke servers. Als u niet de server Management als een VMware VM moet u een afzonderlijk diamodel doelserver instellen als een VM VMware failback verkeer ontvangen configureren.<br/><br/>De server mag geen een domeincontroller.<br/><br/>De server moet hebben een statische IP-adres.<br/><br/>De hostnaam van de server moet 15 tekens of minder.<br/><br/>De landinstelling van het besturingssysteem moet alleen Engels zijn.<br/><br/>De server is internettoegang vereist.<br/><br/>Moet u uitgaande toegang hebben van de server als volgt: tijdelijk toegang op http-80 tijdens de installatie van de onderdelen van de Site herstel (om te downloaden MySQL); Lopend uitgaande toegang via HTTPS 443 voor replicatiebeheer; Lopend uitgaande toegang via HTTPS 9443 voor replicatieverkeer (deze poort kan worden aangepast)<br/><br/> Controleer of dat deze URL's zijn toegankelijk vanaf de server: <br/>- \*. hypervrecoverymanager.windowsazure.com<br/>- \*. accesscontrol.windows.net<br/>- \*. backup.windowsazure.com<br/>- \*. blob.core.windows.net<br/>- \*. store.core.windows.net<br/>-https://www.msftncsi.com/ncsi.txt<br/>- [https://dev.mysql.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi]( https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi")<br/><br/>Als u IP-adres gebaseerde firewallregels op de server hebt, controleert u dat de regels communicatie met Azure toestaan. Moet u toestaan dat het [IP-bereiken gebruikt Azure Datacenter](https://www.microsoft.com/download/details.aspx?id=41653) en de poort HTTPS (443). U moet ook wit lijst IP-adresbereiken voor Azure buiten het gebied van uw abonnement en voor West VS. Het URL- [https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi](https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi " https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi") is voor het downloaden van MySQL.
**VMware vCenter/ESXi host**: | U moet een of meer vMware vSphere ESX/ESXi hypervisors beheren van uw VMware virtuele machines, ESX/ESXi versie 6.0, 5.5 of 5.1 uitgevoerd met de meest recente updates.<br/><br/> Het is raadzaam om dat u een VMware vCenter server als u wilt beheren van uw hosts ESXi implementeren. Moet dat deze vCenter versie 6.0 of 5.5 worden uitgevoerd met de meest recente updates.<br/><br/>Houd er rekening mee dat sites worden hersteld biedt geen ondersteuning voor nieuwe vCenter en vSphere 6.0 functies zoals cross vCenter vMotion, virtuele volumes en opslag DRS. Ondersteuning voor site-herstel is beperkt tot functies die ook beschikbaar in versie 5.5 zijn.
**Beveiligde machines**: | **AZURE**<br/><br/>Computers die u wilt beveiligen, moeten voldoen met [Azure vereisten](site-recovery-best-practices.md#azure-virtual-machine-requirements) voor het maken van Azure VMs.<br><br/>Als u wilt verbinden met de VMs Azure na failover vervolgens moet u de lokale firewall extern bureaublad verbindingen hebt ingeschakeld.<br/><br/>Afzonderlijke schijfcapaciteit op beveiligde computers niet mag zijn meer dan 1023 GB. Een VM kan maximaal 64 schijven hebben (dus maximaal 64 TB). Als er overwegen schijven die groter zijn dan 1 TB databasereplicatie zoals SQL Server altijd in- of Oracle gegevens beveiliging.<br/><br/>Minimale 2 GB beschikbare ruimte voor de installatie voor de installatie van onderdelen.<br/><br/>Gedeeld schijf Gast clusters worden niet ondersteund. Als u een gegroepeerd implementatie databasereplicatie zoals SQL Server altijd in- of Oracle gegevens beveiliging kunt u overwegen hebt.<br/><br/>Unified Extensible Firmware Interface UEFI () / Extensible Firmware Interface(EFI) opstarten wordt niet ondersteund.<br/><br/>Namen van de computer moeten tussen 1 en 63 tekens (letters, cijfers en afbreekstreepjes) bevatten. De naam moet beginnen met een letter of getal en eindigen met een letter of een getal. U kunt de naam van de Azure wijzigen nadat een machine is beveiligd.<br/><br/>**VMware VMs**<br/><br>U moet VMware vSphere PowerCLI 6.0 installeren. Klik op de server management (configuratieserver).<br/><br/>VMware VMs die u wilt beveiligen moet hebben VMware extra geïnstalleerd en uit te voeren.<br/><br/>Als de bron VM NIC-koppeling heeft wordt deze geconverteerd naar een enkel NIC na failover naar Azure.<br/><br/>Als beveiligde VMs een iSCSI-schijf hebt vervolgens converteert sites worden hersteld de beveiligde VM iSCSI-schijf naar een bestand VHD wanneer de VM Azure overgenomen wordt. Als iSCSI-doel kunt bereikbaar zijn via de VM Azure wordt deze verbinding maken met iSCSI-doel en in principe Zie twee schijven – de schijf VHD de VM Azure en de bron iSCSI-schijf. In dit geval moet u het iSCSI-doel dat wordt weergegeven op de mislukte via Azure VM verbreken.<br/><br/>[Meer informatie](#vmware-permissions-for-vcenter-access) over de machtigingen van de VMware-gebruiker die nodig zijn voor sites worden hersteld.<br/><br/> **WINDOWS SERVER MACHINES (op VMware VM of fysieke server)**<br/><br/>De server moet worden uitgevoerd op een ondersteunde 64-bits besturingssysteem wordt uitgevoerd: Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2 met bij minimaal SP1.<br/><br/>Het besturingssysteem moeten worden geïnstalleerd op C:\ schijf en de schijf OS moet een Windows-schijf eenvoudige (OS niet mag worden geïnstalleerd op een Windows dynamische schijf).<br/><br/>Voor Windows Server 2008 R2 machines moeten u hebben .NET Framework 3.5.1 is geïnstalleerd.<br/><br/>U moet een beheerdersaccount opgeven (moet een lokale beheerder op de Windows-computer zijn) voor de push-installatie de Service mobiliteit op Windows-servers. Als de opgegeven account een account niet van het domein is moet u de toegang van externe gebruikers besturingselement op de lokale computer uitschakelen. [Meer informatie](#install-the-mobility-service-with-push-installation).<br/><br/>Site-herstel ondersteunt VMs met RDM schijf.  Tijdens failback malen Site herstel de schijf RDM als de oorspronkelijke bron VM en RDM schijf beschikbaar is. Als ze zijn niet beschikbaar, tijdens failback maakt herstel van de Site een nieuw bestand VMDK voor elke schijf.<br/><br/>**LINUX MACHINES**<br/><br/>Moet u een ondersteunde 64-bits besturingssysteem wordt uitgevoerd: rood rol Enterprise Linux 6,7; Centos 6.5, 6.6,6.7; Oracle Enterprise Linux 6.4, 6.5 waarop rood rol compatibele kernel of Unbreakable Enterprise Kernel Release 3 (UEK3), SUSE Linux Enterprise Server 11 SP3.<br/><br/>/ etc/hosts-bestanden op beveiligde machines mogen bevatten gegevens die de lokale hostnaam aan IP-adressen die is gekoppeld aan alle netwerkadapters toewijzen. <br/><br/>Als u wilt verbinden met een Azure virtuele machines waarop Linux na failover Secure Shell mailclient (ssh), zorg ervoor dat de Secure Shell-service op de computer van de beveiligde is ingesteld om te beginnen automatisch op het opstarten en dat firewallregels toestaan een ssh verbinding toe.<br/><br/>Beveiliging kan alleen worden ingeschakeld voor Linux machines met de volgende opslag: File system (EXT3, ETX4, ReiserFS, XFS); Paden software-apparaat toewijzing (paden)); Volume manager: (LVM2). Fysieke servers met HP CCISS controller opslagmedia worden niet ondersteund. Het bestandssysteem ReiserFS wordt alleen ondersteund op SUSE Linux Enterprise Server 11 SP3.<br/><br/>Site-herstel ondersteunt VMs met RDM schijf.  Herstel van de Site niet opnieuw gebruiken de schijf RDM tijdens failback voor Linux. In plaats daarvan deze Hiermee maakt u een nieuw bestand VMDK voor elke corresponderende RDM schijf.

Zorg ervoor dat u de instelling disk.enableUUID=true in Parameters voor de configuratie van VM in VMware instellen alleen voor Linux VM. Als deze rij niet bestaat, toevoegen. Dit is vereist voor een consistente UUID bieden aan de VMDK, zodat deze correct koppelt. Bedenk ook dat zonder deze instelling, failback, zal het downloaden van een volledige zelfs als de VM beschikbaar op prem. is Deze instelling toe te voegen, zorgt u ervoor dat alleen deltawijzigingen worden overgebracht terug tijdens failback.

## <a name="step-1-create-a-vault"></a>Stap 1: Maak een kluis

1. Meld u aan bij de [beheerportal](https://manage.windowsazure.com/).
2. **Gegevensservices**uitvouwen > **Herstel Services** en klik op **Site herstel kluis**.
3. Klik op **nieuwe maken** > **snel tabellen maken**.
4. Voer in het vak **naam**een beschrijvende naam voor de kluis.
5. Selecteer in de **regio**, de geografische regio voor de kluis. Als u wilt controleren bekijken ondersteunde regio's geografische beschikbaarheid in [Azure Site herstel prijzen Details](https://azure.microsoft.com/pricing/details/site-recovery/)
6. Klik op **maken kluis**.
    ![Nieuwe kluis](./media/site-recovery-vmware-to-azure-classic/quick-start-create-vault.png)

Controleer de statusbalk om te bevestigen dat de kluis is gemaakt. De kluis worden, als **actief** vermeld op de hoofdpagina van **Herstel Services** .

## <a name="step-2-set-up-an-azure-network"></a>Stap 2: Een Azure-netwerk instellen

Een Azure netwerk instellen, zodat na failover Azure VMs worden verbonden met een netwerk en zodat failback naar de on-premises implementatie-site kunt werken zoals verwacht.

1. In de portal van Azure > **virtueel netwerk maken** Geef de netwerknaam. IP-adres bereik en subnet naam.
2. U moet VPN/ExpressRoute aan het netwerk toevoegen als u moet doen failback. VPN/ExpressRoute kunnen worden toegevoegd bij het netwerk zelfs na failover.

[Lees meer](../virtual-network/virtual-networks-overview.md) over Azure-netwerken te gebruiken.

> [AZURE.NOTE] [Migratie van netwerken](../resource-group-move-resources.md) via resourcegroepen binnen hetzelfde abonnement of via abonnementen wordt niet ondersteund voor netwerken die zijn gebruikt voor het implementeren van sites worden hersteld.

## <a name="step-3-install-the-vmware-components"></a>Stap 3: De VMware-onderdelen installeren

Als u wilt repliceren VMware virtuele installeren machines de volgende VMware-onderdelen op de server management:

1. [Download](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) en installeer VMware vSphere PowerCLI 6.0.
2. Start opnieuw op de server.


## <a name="step-4-download-a-vault-registration-key"></a>Stap 4: Een kluis registratie-sleutel downloaden

1. Server open vanuit het beheer de Site herstel-console in Azure wordt aangegeven. Klik in de pagina **Herstel Services** op de kluis om de pagina snel aan de slag te openen. Snel aan de slag kan ook worden geopend op elk gewenst moment met het pictogram.

    ![Snel aan de slag-pictogram](./media/site-recovery-vmware-to-azure-classic/quick-start-icon.png)

2. Klik op de pagina **Snel starten** op **Voorbereiden doel Resources** > **een registratiegegevens-sleutel downloaden**. De registratiebestand wordt automatisch gegenereerd. Dit geldt voor 5 dagen nadat deze gegenereerd.


## <a name="step-5-install-the-management-server"></a>Stap 5: De server management installeren
> [AZURE.TIP] Controleer of dat deze URL's zijn toegankelijk vanaf de server:
>
- *. hypervrecoverymanager.windowsazure.com
- *. accesscontrol.windows.net
- *. backup.windowsazure.com
- *. blob.core.windows.net
- *. store.core.windows.net
- https://dev.mysql.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi
- https://www.msftncsi.com/ncsi.txt




[AZURE.VIDEO enhanced-vmware-to-azure-setup-registration]

1. Klik op de pagina **Snel aan de slag** het geïntegreerd installatiebestand naar de server te downloaden.

2. Het installatiebestand om setup te starten in de installatiewizard Site herstel Unified uitvoeren.

3.  Selecteer **de configuratieserver en processerver installeren**in **voordat u begint** .

    ![Voordat u begint](./media/site-recovery-vmware-to-azure-classic/combined-wiz1.png)
4. Klik op **ik ga akkoord** als u wilt downloaden en installeren van MySQL van **Derden Software-licentie** . 

    ![= Derde partij software](./media/site-recovery-vmware-to-azure-classic/combined-wiz105.PNG)

5. Blader in **registratie** en selecteer de registratie-sleutel die u hebt gedownload van de kluis.

    ![Registratie](./media/site-recovery-vmware-to-azure-classic/combined-wiz3.png)

6. Geef op hoe de Provider die wordt uitgevoerd op de configuratieserver maakt verbinding met Azure Site herstel via internet op **Instellingen voor Internet** .

    - Als u verbinding met de proxy die momenteel ingesteld op de computer wilt selecteert u **verbinding maken met bestaande proxy-instellingen**.
    - Als u wilt dat de Provider verbinding selecteert u **verbinding maken rechtstreeks zonder proxy**.
    - Als de bestaande proxy is verificatie vereist of als u wilt een aangepaste proxy gebruiken voor de verbinding Provider, selecteert u **verbinding maken met aangepaste proxy-instellingen**.
        - Als u een aangepaste proxy gebruiken moet u het adres, poorten en referenties opgeven
        - Als u een proxyserver gebruikt hebt u al mag de volgende URL's:
            - *. hypervrecoverymanager.windowsazure.com;    
            - *. accesscontrol.windows.net; 
            - *. backup.windowsazure.com; 
            - *. blob.core.windows.net; 
            - *. store.core.windows.net
            

    ![Firewall](./media/site-recovery-vmware-to-azure-classic/combined-wiz4.png)

7. **Controle van vereisten** setup wordt uitgevoerd een selectievakje om ervoor te zorgen dat installatie kan worden uitgevoerd. 

    
    ![Vereisten voor](./media/site-recovery-vmware-to-azure-classic/combined-wiz5.png)

     Als er een waarschuwing over het **synchroniseren van de globale tijd controleren verschijnt** controleert u of die de tijd op de systeemklok (**datum en tijd** -instellingen) is hetzelfde als de gewenste tijdzone.

    ![TimeSyncIssue](./media/site-recovery-vmware-to-azure-classic/time-sync-issue.png)

8. Maak in **MySQL-configuratie** referenties voor aanmelding bij de MySQL-server-instantie die wordt geïnstalleerd.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz6.png)

9. In **De Details van de omgeving** selecteren of u gaat VMware VMs repliceren. Als u bent, controleert setup dat PowerCLI 6.0 is geïnstalleerd.

    ![MySQL](./media/site-recovery-vmware-to-azure-classic/combined-wiz7.png)

10. In **Installatielocatie** selecteren waar u de binaire bestanden installeren en de cache opslaan. Kunt u een station met ten minste 5 GB opslagruimte beschikbaar, maar het is raadzaam een cache-station met ten minste 600 GB beschikbare ruimte te.

    ![Locatie installeren](./media/site-recovery-vmware-to-azure-classic/combined-wiz8.png)

11. Geef de luisteraar ervan af (netwerkadapter en SSL-poort) waarop de configuratieserver worden gegevens verzenden en ontvangen herhaling in **Netwerk selecteren** . Kunt u de standaard poort (9443). Naast deze poort, worden poort 443 gebruikt door een webserver die orchestrates herhaling bewerkingen. 443 niet mag worden gebruikt voor het ontvangen van replicatie-verkeer is toegestaan.


    ![Netwerk selecteren](./media/site-recovery-vmware-to-azure-classic/combined-wiz9.png)



12.  Controleer de gegevens in het **Overzicht** en klikt u op **installeren**. Wanneer de installatie is voltooid wordt een wachtwoordzin gegenereerd. U moet deze wanneer u replicatie inschakelt zodat deze kopiëren en op een veilige locatie bewaren.

    ![Overzicht](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)



13.  Controleer de gegevens in het **Overzicht** .

    ![Overzicht](./media/site-recovery-vmware-to-azure-classic/combined-wiz10.png)

>[AZURE.WARNING]Microsoft Azure herstel servicemedewerker van proxy moet instellen.
>Start een toepassing met de naam 'Microsoft Azure herstel Services Shell' in het menu Start van Windows nadat de installatie voltooid is. Voer de volgende lijst met opdrachten voor het instellen van de proxy-instellingen in het opdrachtvenster dat wordt geopend.
>
    $pwd = ConvertTo-SecureString -String ProxyUserPassword
    Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumb – ProxyUserName domain\username -ProxyPassword $pwd
    net stop obengine
    net start obengine



### <a name="run-setup-from-the-command-line"></a>Het installatieprogramma uitvoert vanaf de opdrachtregel

U kunt ook de geïntegreerd wizard uitvoeren vanaf de opdrachtregel, als volgt:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Waarbij geldt:

- / ServerMode: verplicht. Hiermee geeft u of de installatie de configuratie en proces servers of alleen het processerver installeren moet (gebruikt voor het installeren van extra proces servers). Invoerwaarden: CS, PS
- InstallDrive: verplicht. Hiermee geeft u de map waarin de onderdelen zijn geïnstalleerd.
- / MySQLCredFilePath. Verplicht. Hiermee geeft u het pad naar een bestand waar de referenties voor de MySQL-server verhaal zijn. De sjabloon om het bestand te maken krijgen.
- / VaultCredFilePath. Verplicht. Locatie van het bestand kluis-referenties
- / EnvType. Verplicht. Type installatie. Waarden: VMware, NonVMware
- / PSIP en /CSIP. Verplicht. IP-adres van het proces en configuratieserver.
- / PassphraseFilePath. Verplicht. Locatie van het bestand wachtwoordzin.
- / ByPassProxy. Optioneel. Hiermee geeft u op dat de server is verbonden met Azure zonder proxy.
- / ProxySettingsFilePath. Optioneel. Hiermee geeft u instellingen voor een aangepaste proxy (standaardproxy op de server is verificatie vereist), of aangepaste proxy




## <a name="step-6-set-up-credentials-for-the-vcenter-server"></a>Stap 6: Referenties voor de server vCenter instellen

> [AZURE.VIDEO enhanced-vmware-to-azure-discovery]

De processerver kan VMware VMs die worden beheerd door een vCenter-server automatisch detecteren. Voor automatische detectie moet Site herstel een account en de referenties die toegang heeft tot de vCenter-server. Dit is niet relevant als u alleen de fysieke servers bent repliceren.

Ga als volgt als volgt:

1. Klik op de vCenter server maken van een rol (**Azure_Site_Recovery**) op het niveau van de vCenter met de [vereiste machtigingen](#vmware-permissions-for-vcenter-access).
2. De rol **Azure_Site_Recovery** toewijzen aan een gebruiker vCenter.

    >[AZURE.NOTE] Een gebruikersaccount vCenter dat de alleen-lezen-rol heeft failover kan worden uitgevoerd zonder beveiligde bron machines afgesloten. Als u wilt afsluiten die machines moet u de rol Azure_Site_Recovery. Houd er rekening mee dat als u alleen VMs VMware naar Azure migreert en te failback hoeft de rol alleen-lezen voldoende is.

3. Als u wilt toevoegen is voor het account **cspsconfigtool**openen. Het is alleen beschikbaar als een snelkoppeling op het bureaublad en bevindt in de map van de \home\svsystems\bin [locatie installeren].
2. Klik op het tabblad **Accounts beheren** op **Account toevoegen**.

    ![Account toevoegen](./media/site-recovery-vmware-to-azure-classic/credentials1.png)

3. In **Account Details** toevoegen referenties die kunnen worden gebruikt voor toegang tot de vCenter-server. Houd er rekening mee dat het meer dan 15 minuten totdat de naam van het account wordt weergegeven in de portal kan duren. Als u direct bijwerken, klikt u op Vernieuwen op het tabblad **Servers van de configuratie** .

    ![Meer informatie](./media/site-recovery-vmware-to-azure-classic/credentials2.png)

## <a name="step-7-add-vcenter-servers-and-esxi-hosts"></a>Stap 7: VCenter servers en ESXi hosts toevoegen

Als u bezig bent met het repliceren van VMware VMs moet u een vCenter server (of ESXi host) toevoegen.

1. Klik op de **Servers** > **Configuration Servers** tab-toets, selecteert u de configuratieserver > **toevoegen vCenter server**.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter1.png)

2. Voeg de server vCenter of ESXi host details, de naam van het account dat u hebt opgegeven voor toegang tot de server vCenter in de vorige stap en de processerver die wordt gebruikt om te ontdekken VMware VMs die worden beheerd door de server vCenter. Houd er rekening mee dat het vCenter server of de ESXi host zich bevinden in hetzelfde netwerk als de server waarop de processerver is geïnstalleerd.

    >[AZURE.NOTE] Als u de vCenter-server of ESXi host met een account dat u geen hebt op de server vCenter of host beheerdersbevoegdheden toevoegt, Controleer of de vCenter of ESXi-accounts zijn deze bevoegdheden ingeschakeld: Datacenter, gegevensopslag, map, Jost, netwerk, Resource, virtuele machine, vSphere verdeeld veranderen. De server vCenter moet bovendien de bevoegdheden van de weergaven opslag.

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter2.png)

3. Wanneer discovery voltooid is wordt de server vCenter worden vermeld in het tabblad **Servers van de configuratie** .

    ![vCenter](./media/site-recovery-vmware-to-azure-classic/add-vcenter3.png)


## <a name="step-8-create-a-protection-group"></a>Stap 8: Maak een groep beveiliging

> [AZURE.VIDEO enhanced-vmware-to-azure-protection]


Beveiliging groepen zijn logische groeperingen van virtuele machines of fysieke servers waarop u wilt beveiligen met dezelfde beveiligingsinstellingen. U instellingen voor documentbeveiliging toepassen op een groep beveiliging en deze instellingen zijn toegepast op alle virtuele machines/fysieke computers die u aan de groep toevoegt.

1. **Beveiligde Items**openen > **Groep beveiliging** en klik op een groep beveiliging toe te voegen.

    ![Beveiliging groep maken](./media/site-recovery-vmware-to-azure-classic/protection-groups1.png)

2. Klik op de pagina **Instellingen voor documentbeveiliging groep opgeven** Geef een naam voor de groep en selecteer in het vak **van** de configuratieserver waarop u wilt maken van de groep. **Doel** is Azure.

    ![Instellingen voor documentbeveiliging-groep](./media/site-recovery-vmware-to-azure-classic/protection-groups2.png)

3. Klik op de pagina **Instellingen voor replicatie opgeven** door de replicatie-instellingen die worden gebruikt voor alle computers in de groep te configureren.

    ![Beveiliging groep herhaling](./media/site-recovery-vmware-to-azure-classic/protection-groups3.png)

    - **Meerdere VM consistentie**: als u dit inschakelt dat wordt gemaakt gedeelde toepassing consistente herstel punten op de computers in de groep beveiliging. Deze instelling is meest relevante wanneer alle computers in de groep beveiliging de dezelfde werklast worden uitgevoerd. Alle computers worden dezelfde gegevens bondig worden hersteld. Dit is beschikbaar of u bent repliceren VMware VMs of Windows/Linux fysieke servers.
    - **Drempelwaarde voor vrijgegeven Productieorder**: Hiermee stelt u de vrijgegeven Productieorder. Waarschuwingen wordt gegenereerd wanneer de doorlopende gegevens beveiliging replicatie groter is dan de geconfigureerde vrijgegeven Productieorder drempelwaarde.
    - **Herstel wijst u bewaren**: Hiermee geeft u het venster bewaarbeleid. Beveiligde machines kunnen herstellen naar een willekeurige plaats in dit venster.
    - **Frequentie van toepassing-consistente momentopname**: Hiermee wordt opgegeven hoe vaak herstel punten die van toepassing-consistente momentopnamen wordt gemaakt.

Na het klikken op het vinkje wordt een groep beveiliging worden gemaakt met de naam die u hebt opgegeven. In Addition een tweede groep voor de beveiliging wordt gemaakt met de naam < beveiliging-groep-naam-Failback). Deze groep beveiliging wordt gebruikt als u niet terug naar de site on-premises implementatie na failover naar Azure. U kunt de groepen beveiliging controleren terwijl ze zijn gemaakt op de pagina **Beveiligde Items** .

## <a name="step-9-install-the-mobility-service"></a>Stap 9: Installeer de service mobiliteit

De eerste stap bij het inschakelen van beveiliging voor virtuele machines en fysieke servers is de mobiliteit-service installeren. U kunt dit op twee manieren doen:

- Automatisch push en de service installeren op elke computer van de processerver. Houd er rekening mee dat wanneer u machines aan een groep beveiliging toevoegen en die nu wel zich een juiste versie van de mobiliteit service push-installatie niet.
- Automatisch de service installeren via uw enterprise push-methode zoals WSUS of System Center Configuration Manager. Controleer of dat u hebt de server management ingesteld voordat u dit doet.
- Handmatig installeren op elke computer die u wilt beveiligen. maken weet u u hebt ingesteld de server voordat u dit doet.


### <a name="install-the-mobility-service-with-push-installation"></a>Installeren van de service mobiliteit met push-installatie

Wanneer u machines aan een groep beveiliging toevoegt is automatisch de service mobiliteit gedrukt en op elke computer geïnstalleerd door het processerver.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Voorbereiden voor automatische push op Windows-computers

Hier volgt informatie over voor het voorbereiden van Windows-computers, zodat de mobiliteit-service automatisch kan worden geïnstalleerd door het processerver.

1.  Maak een account die kan worden gebruikt door de processerver voor toegang tot de computer. Het account moet beheerdersbevoegdheden (lokaal of domein). Houd er rekening mee dat deze referenties alleen voor push-installatie van de service mobiliteit gebruikt worden.

    >[AZURE.NOTE] Als u een account niet gebruikt moet u de toegang van externe gebruikers besturingselement op de lokale computer uitschakelen. Klik hiertoe in het register onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System toevoegen de DWORD-vermelding LocalAccountTokenFilterPolicy met een waarde van 1 onder. Om toe te voegen van het register vermelding van een CLI opdracht opent of via PowerShell invoert **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  Klik op het Windows Firewall van de computer die u wilt beveiligen, selecteert u **toestaan een app of functie via Firewall** en **Bestands- en printerdeling** en **Windows Management Instrumentation**inschakelen. U kunt het firewallbeleid met een GPO configureren voor machines die deel uitmaakt van een domein.

    ![Firewallinstellingen](./media/site-recovery-vmware-to-azure-classic/mobility1.png)

2. Het account dat u hebt gemaakt toevoegen:

    - Open **cspsconfigtool**. Het is alleen beschikbaar als een snelkoppeling op het bureaublad en bevindt in de map van de \home\svsystems\bin [locatie installeren].
    - Klik op het tabblad **Accounts beheren** op **Account toevoegen**.
    - Het account dat u hebt gemaakt toevoegen. Na het toevoegen van het account dat moet u de referenties opgeven wanneer u een computer aan een groep beveiliging toevoegt.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Voorbereiden voor automatische push op Linux-servers

1.  Zorg ervoor dat de Linux-computer die u wilt beveiligen, zoals is beschreven in de [On-premises vereisten](#on-premises-prerequisites)wordt ondersteund. Controleer of er netwerkverbinding is tussen de computer die u wilt beveiligen en de server die processerver wordt uitgevoerd het.

2.  Maak een account die kan worden gebruikt door de processerver voor toegang tot de computer. Het account moet een gebruiker hoofdmap op de bronserver Linux. Houd er rekening mee dat deze referenties alleen voor push-installatie van de service mobiliteit gebruikt worden.

    - Open **cspsconfigtool**. Het is alleen beschikbaar als een snelkoppeling op het bureaublad en bevindt in de map van de \home\svsystems\bin [locatie installeren].
    - Klik op het tabblad **Accounts beheren** op **Account toevoegen**.
    - Het account dat u hebt gemaakt toevoegen. Na het toevoegen van het account dat moet u de referenties opgeven wanneer u een computer aan een groep beveiliging toevoegt.

3.  Schakel op het bestand/etc/hosts op de bronserver Linux bevat gegevens die de lokale hostnaam aan IP-adressen die is gekoppeld aan alle netwerkadapters toewijzen.
4.  Installeer de meest recente openssh, openssh-server, openssl-pakketten op de computer die u wilt beveiligen.
5.  Controleer of dat SSH is ingeschakeld en wordt uitgevoerd op poort 22.
6.  SFTP subsysteem en wachtwoord voor verificatie in het bestand sshd_config als volgt inschakelen:

    - Meld u aan als de hoofdmap.
    - Zoek in het bestand /etc/ssh/sshd_config-bestand, de regel die met PasswordAuthentication begint.
    - Verwijder de opmerkingen bij de lijn en wijzig de waarde van **geen** op **Ja**.
    - Zoek de regel die met **subsysteem begint** en verwijder de opmerkingen bij de regel.

        ![Linux](./media/site-recovery-vmware-to-azure-classic/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>De service mobiliteit handmatig installeren

Het installatieprogramma's zijn beschikbaar in C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository.

Bron-besturingssysteem | Servicebestand voor installatie mobiliteit
--- | ---
Windows Server (alleen 64 bits) | Microsoft-ASR_UA_9. *.0.0_Windows_* release.exe
CentOS 6.4, 6.5, 6.6 (alleen 64 bits) | Microsoft-ASR_UA_9. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux Enterprise Server 11 SP3 (alleen 64 bits) | Microsoft-ASR_UA_9. *.0.0_SLES11-SP3-64_*release.tar.gz
Oracle Enterprise Linux 6.4, 6.5 (alleen 64 bits) | Microsoft-ASR_UA_9. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-manually-on-a-windows-server"></a>Handmatig installeren op een Windows server


1. Download en voer het installatieprogramma van relevante.
2. Selecteer **mobiliteit-service**in **voordat u begint **.

    ![Mobiliteit-service](./media/site-recovery-vmware-to-azure-classic/mobility3.png)

3. Geef het IP-adres van de server management en de wachtwoordzin die is gegenereerd tijdens de installatie van de onderdelen van de server management in **Configuratiegegevens voor de Server** . U kunt de wachtwoordzin ophalen door te voeren: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** op de server management.

    ![Mobiliteit-service](./media/site-recovery-vmware-to-azure-classic/mobility6.png)

4. Laat de standaardlocatie op **Locatie installeren** en klik op **volgende** om installatie te starten.
5. Voortgang van uw **Installatie** installatie kan worden bewaakt en start de computer opnieuw op als u hierom wordt gevraagd.

U kunt ook installeren vanaf de opdrachtregel:

UnifiedAgent.exe [/ rol < Agent/MasterTarget >] [/ InstallLocation <Installation Directory>] [/ CSIP <IP address of CS to be registered with>] [/ PassphraseFilePath <Passphrase file path>] [/ LogFilePath <Log File Path>]

Waarbij geldt:

- / Rol: verplicht. Hiermee geeft u of de service mobiliteit moet worden geïnstalleerd.
- / InstallLocation: verplicht. Geeft aan waar de service installeren.
- / PassphraseFilePath: verplicht. Hiermee geeft u de configuratie server wachtwoordzin.
- / LogFilePath: verplicht. Hiermee geeft u log setup bestanden locatie

#### <a name="uninstall-mobility-service-manually"></a>Mobiliteit service handmatig verwijderen

Mobiliteit-Service kan worden verwijderd, het programma toevoegen verwijderen via het Configuratiescherm of opdrachtregel gebruiken.

De opdracht verwijderen van de opdrachtregel met mobiliteit-Service is

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}

#### <a name="modify-the-ip-address-of-the-management-server"></a>Het IP-adres van de server management wijzigen

Na het uitvoeren van de wizard kunt u het IP-adres van de server management als volgt wijzigen:

1. Open het bestand hostconfig.exe (die zich op het bureaublad).
2. U kunt het IP-adres van de server management wijzigen op het tabblad **Algemeen** .

    >[AZURE.NOTE] U moet alleen het IP-adres van de server management wijzigen. Het poortnummer voor management servercommunicatie moet 443 en gebruik HTTPS naar links moet worden ingeschakeld. De wachtwoordzin niet mag worden gewijzigd.

    ![IP-adres van Management server](./media/site-recovery-vmware-to-azure-classic/host-config.png)

#### <a name="install-manually-on-a-linux-server"></a>Handmatig installeren op een server Linux:

1. Kopieer het juiste tar-archief op basis van de bovenstaande tabel met de Linux-computer die u wilt beveiligen.
2. Open een shellprogramma en het archief ingepakte tar op een lokaal pad extraheren door te voeren:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Een bestand passphrase.txt maken in de lokale map waarin u de inhoud van het archief tar uitgepakt. Moet u doen dit kopieert de wachtwoordzin uit C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase op de server en sla deze in passphrase.txt door te voeren *`echo <passphrase> >passphrase.txt`* in shell.
4. De service mobiliteit installeren door in te voeren *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Geef het interne IP-adres van de server management en zorg ervoor dat poort 443 is geselecteerd.

**U kunt ook installeren vanaf de opdrachtregel**:

1. Kopieer de wachtwoordzin uit C:\Program Files (x86) \InMage Systems\private\connection op de server management en opslaan als 'passphrase.txt' op de server management. Voer deze opdrachten. In ons voorbeeld is het IP-adres van de management server 104.40.75.37 en de HTTPS-poort 443 moet zijn:

Installeren op een productieserver:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Installeren op de basispagina doelserver:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


## <a name="step-10-enable-protection-for-a-machine"></a>Stap 10: Beveiliging voor een machine inschakelen

Als beveiliging wilt inschakelen u een virtuele machines en fysieke servers toevoegen aan een groep beveiliging. Voordat u begint, houd rekening met het volgende als u bent VMware virtuele machines beveiligen:

- VMware VMs elke 15 minuten zijn gevonden en het kan worden weergegeven in de portal-Site herstel na discovery meer dan 15 minuten duren.
- De wijzigingen op de virtuele machine (zoals VMware hulpmiddelen voor installatie) kunnen ook worden bijgewerkt bij het herstellen van de Site meer dan 15 minuten duren.
- U kunt de laatste keer dat ontdekt controleren voor VMware VMs in het veld **Laatste contactpersoon op** voor de host van de server/ESXi vCenter op het tabblad **Servers van de configuratie** .
- Als u een groep beveiliging al hebt gemaakt en voegt u een vCenter Server of ESXi host na die, kan meer dan 15 minuten voor de portal-Site herstel van Azure te vernieuwen en virtuele machines kunnen worden vermeld in het dialoogvenster **machines toevoegen aan een groep beveiliging** duren.
- Als u doorgaan direct wilt met de computers toevoegen aan groep zonder te wachten de geplande discovery beveiliging, selecteert u de configuratieserver (Klik niet op deze) en klik op de knop **vernieuwen** .

Daarnaast rekening met het volgende:

- Het is raadzaam om uw groepen beveiliging te bouwen, zodat ze uw werkbelasting. Voeg bijvoorbeeld computers met een specifieke toepassing tot dezelfde groep.
- Wanneer u machines aan een groep beveiliging toevoegt, wordt de processerver automatisch verplaatst en wordt de service mobiliteit geïnstalleerd als deze niet al is geïnstalleerd. Houd er rekening mee dat u over de push om voorbereiden beschikken moet zoals is beschreven in de vorige stap.


Machines toevoegen aan een groep beveiliging:

1. Klik op **Items beveiligde** > **Groep beveiliging** > **Machines** > Machines toevoegen. Een goede gewoonte \As
2. In **virtuele Machines selecteren** als u VMware virtuele machines beveiligen bent, selecteer een vCenter-server die wordt beheerd uw virtuele machines (of de EXSi host waarop ze werkt met) en selecteer vervolgens de machines.

    ![Beveiliging inschakelen](./media/site-recovery-vmware-to-azure-classic/enable-protection2.png)

3.  In de **virtuele Machines selecteren** als u fysieke servers beveiligen bent, in de wizard **Fysieke Machines toevoegen** vindt u de IP-adres en het beschrijvende naam. Selecteer het besturingssysteem gezin.

    ![Beveiliging inschakelen](./media/site-recovery-vmware-to-azure-classic/enable-protection1.png)

4. Selecteer het account opslagruimte u gebruikt voor replicatie en selecteert u of de instellingen moeten worden gebruikt voor alle werkbelasting in **Target Resources opgeven** . Houd er rekening mee dat er momenteel premium opslag accounts worden niet ondersteund.

    >[AZURE.NOTE] 1. we het verplaatsen van opslag-accounts die zijn gemaakt met behulp van de [nieuwe portal van Azure](../storage/storage-create-storage-account.md) via resourcegroepen wordt niet ondersteund.                           2.[migratie van opslag accounts](../resource-group-move-resources.md) via resourcegroepen binnen hetzelfde abonnement of via abonnementen wordt niet ondersteund voor opslag-accounts gebruikt voor het implementeren van sites worden hersteld.

    ![Beveiliging inschakelen](./media/site-recovery-vmware-to-azure-classic/enable-protection3.png)

5. Selecteer het account in **Accounts opgeven** u [geconfigureerd](#install-the-mobility-service-with-push-installation) voor automatische installatie van de service mobiliteit wilt gebruiken.

    ![Beveiliging inschakelen](./media/site-recovery-vmware-to-azure-classic/enable-protection4.png)

6. Klik op het vinkje hebt machines toegevoegd aan de groep beveiliging en te starten eerste replicatie voor elke computer.

    >[AZURE.NOTE] Als push-installatie is de service wordt automatisch geïnstalleerd op computers waarop niet deze terwijl ze bent toegevoegd aan de groep beveiliging mobiliteit voorbereid. Nadat de service installatie is wordt een taak beveiliging wordt gestart en mislukt. Na de fout moet u handmatig start opnieuw op elke computer waarop de mobiliteit-service is geïnstalleerd heeft. Na het opnieuw starten de taak beveiliging opnieuw begint en eerste replicatie plaatsvindt.

U kunt de status op de pagina **taken** controleren.

![Beveiliging inschakelen](./media/site-recovery-vmware-to-azure-classic/enable-protection5.png)

Daarnaast beveiliging status kan worden gecontroleerd in de **Beveiligde Items** > <protection group name> > **virtuele Machines**. Nadat de eerste replicatie is voltooid en gegevens worden gesynchroniseerd, machine-status wordt gewijzigd naar** beveiligd**.

![Beveiliging inschakelen](./media/site-recovery-vmware-to-azure-classic/enable-protection6.png)


## <a name="step-11-set-protected-machine-properties"></a>Stap 11: Set beveiligde machine eigenschappen

1. Nadat een computer een **beveiligde** status heeft kunt u de eigenschappen failover configureren. Groepsdetails Selecteer van de computer en open het tabblad **configureren** in de beveiliging.
2. Herstel van de site automatisch wordt voorgesteld eigenschappen voor de VM Azure en vastgesteld dat het lokale netwerk instellingen.

    ![VM-eigenschappen instellen](./media/site-recovery-vmware-to-azure-classic/vm-properties1.png)

3. U kunt deze instellingen wijzigen:

    -  **De naam van de Azure VM**: dit is de naam die wordt gegeven op de computer in Azure wordt aangegeven na failover. De naam moet voldoen aan vereisten voor Azure.

    -  **Grootte van de Azure VM**: het aantal netwerkadapters wordt bepaald door de grootte die u voor de doeltoepassing virtuele machine opgeeft. [Lees meer](../virtual-machines/virtual-machines-linux-sizes.md/#size-tables) over de grootte en adapters. Houd rekening met:

        - Wanneer u de grootte voor een virtuele machine wijzigen en de instellingen opslaat, wordt het aantal netwerkadapter wordt gewijzigd wanneer u het tabblad **configureren** volgende keer opent. Het aantal netwerkadapters van target virtuele machines is minimale van het aantal netwerkadapters op bron virtuele machine en het maximum aantal netwerkadapters worden ondersteund door de grootte van de virtuele machine gekozen.
            - Als het aantal netwerkadapters op de broncomputer kleiner dan of gelijk is aan het aantal adapters toegestaan voor de grootte van de computer doel is, klikt u vervolgens heeft het doel hetzelfde aantal adapters als de bron.
            - Als het aantal adapters voor de bron virtuele machine groter is dan het getal dat is toegestaan voor de doelgrootte en vervolgens de doeltoepassing maximaal wordt gebruikt.
            - Als u een bron-machine heeft twee netwerkadapters en de grootte van de computer doel ondersteunt vier, de doelcomputer heeft twee adapters bijvoorbeeld. Als de broncomputer twee adapters heeft, maar de doelgrootte van de ondersteunde alleen een ondersteunt wordt slechts één adapter hebben op de doelcomputer.
        - Als de virtuele machine heeft verbonden meerdere netwerkadapters die alle adapters moeten met hetzelfde Azure netwerk.
    - **Azure-netwerk**: U moet een Azure netwerk dat Azure VMs worden verbonden met na failover opgeven. Als u niet opgeeft wordt niet de VMs Azure aangesloten op een netwerk. Daarnaast moet u een Azure netwerk kiezen als u failback van Azure naar de on-premises implementatie-site wilt. Failback vereist een VPN-verbinding tussen een Azure netwerk en een on-premises netwerk.
    - **Azure IP-adres/subnet**: voor elke netwerkadapter selecteert u het subnet waarin de VM Azure moet worden verbonden. Houd rekening met:
        - Als de netwerkadapter van de broncomputer is geconfigureerd voor gebruik van een statische IP-adres kunt u een statische IP-adres voor de VM Azure opgeven. Als u een statische IP-adres niet opgeeft worden, alle beschikbare IP-adressen toegewezen. Als het target IP-adres is opgegeven, maar deze nog gebruikt door een andere VM in Azure wordt aangegeven, failover mislukt. Als de netwerkadapter van de broncomputer is geconfigureerd voor gebruik van DHCP wordt hebt u DHCP als de instelling voor Azure.

## <a name="step-12-create-a-recovery-plan-and-run-a-failover"></a>Stap 12: Een plan voor herstel maken en uitvoeren van een failover

> [AZURE.VIDEO enhanced-vmware-to-azure-failover]

U kunt een failover voor één computer worden uitgevoerd, of mislukt over meerdere virtuele machines die dezelfde taak uitvoeren of de dezelfde werklast uitvoeren. Mislukt op meerdere computers tegelijkertijd die u deze aan een herstel-abonnement toevoegen.

### <a name="create-a-recovery-plan"></a>Maak een herstelplan

1. Klik op de pagina **Herstel van abonnement** op **Herstel-abonnement toevoegen** en een herstel-abonnement toevoegen. Geef de details van het abonnement en **Azure** selecteert als het doel.

    ![Plan voor herstel configureren](./media/site-recovery-vmware-to-azure-classic/recovery-plan1.png)

2. Selecteer een groep beveiliging in **VM selecteren** en selecteer machines in de groep toevoegen aan het abonnement herstel.

    ![Virtuele machines toevoegen](./media/site-recovery-vmware-to-azure-classic/recovery-plan2.png)

U kunt het plan voor het maken van groepen en sequentie de volgorde waarin machines in het abonnement herstel worden overgenomen. U kunt ook scripts en aanwijzingen voor handmatige acties toevoegen. Scripts kunnen worden gemaakt handmatig of met via [Azure automatisering Runbooks](site-recovery-runbook-automation.md). [Meer informatie](site-recovery-create-recovery-plans.md) over het aanpassen van herstel-abonnementen.

## <a name="run-a-failover"></a>Een failover uitvoeren

Houd rekening met voordat u een failover uitvoert:


- Zorg ervoor dat de server wordt uitgevoerd en beschikbaar - anders failover, mislukt.
- Als u een notitie niet-geplande failover die uitvoeren:

    - Indien mogelijk moet u afgesloten primaire machines voordat u een niet-geplande failover uitvoert. Dit zorgt ervoor dat de bron- en replica machines die worden uitgevoerd op hetzelfde moment geen hebt. Als u bezig bent met het repliceren van VMware VMs kunt Klik wanneer u een niet-geplande failover uitvoert u opgeven dat sites worden hersteld beste planning op de bron-machines afsluiten stellen. Afhankelijk van de status van de primaire site mogelijk of werkt mogelijk niet. Als u bezig bent met het repliceren van fysieke servers niet mogelijk deze optie is sites worden hersteld.
    - Wanneer u een niet-geplande failover uitvoert wordt stopgezet repliceren van primaire computers zodat alle gegevens delta won't worden doorgeschakeld nadat een niet-geplande failover begint.

- Als u verbinden met de replica virtuele machine in Azure wordt aangegeven na failover wilt, moet u verbinding met extern bureaublad inschakelen op de broncomputer voordat u de overname uitvoeren en RDP-verbinding via de firewall toestaan. U moet ook RDP toestaan op de openbare eindpunt van de Azure virtuele machine na failover. Volg deze [Aanbevolen procedures](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) om ervoor te zorgen dat RDP werkt na een failover.

>[AZURE.NOTE] Als u de beste prestaties wanneer u een failover in Azure uitvoeren, moet u ervoor zorgen dat u de Azure-Agent in de beveiligde computer hebt geïnstalleerd. Hiermee kunt u in het opstarten sneller en kunt u er ook in diagnose in geval van problemen. Linux-agent kan worden gevonden [hier](https://github.com/Azure/WALinuxAgent) - en Windows agent vindt u [hier](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="run-a-test-failover"></a>Een failover test uitvoeren

Een failover testen om te simuleren uw failover- en herstelbestanden processen in een geïsoleerd netwerk dat niet van invloed op uw productieomgeving uitvoeren en normale replicatie kan worden voortgezet normaal. Test failover start op de bron en u kan worden uitgevoerd op een aantal manieren oplossen:

- **Een Azure-netwerk niet opgeeft**: als u een failover test zonder een netwerk de test gewoon moeten worden gecontroleerd dat virtuele machines starten en correct in Azure weergegeven uitvoeren. Virtuele machines wordt niet met een Azure netwerk na failover aangesloten.
- **Geef een Azure-netwerk**: dit type failover controleert dat de hele replicatieomgeving afgerond zoals verwacht afkomstig is en dat Azure virtuele machines zijn verbonden met het opgegeven netwerk.


1. Op de pagina **Herstel abonnementen** het abonnement en klik op **Testen Failover**.

    ![Virtuele machines toevoegen](./media/site-recovery-vmware-to-azure-classic/test-failover1.png)

2. Selecteer **geen** om aan te geven dat u niet wilt gebruiken een Azure netwerk voor de overname test of Selecteer het netwerk waarin de test VMs zijn na failover verbonden in **Testen Failover bevestigen** . Klik op het vinkje om te beginnen de overname.

    ![Virtuele machines toevoegen](./media/site-recovery-vmware-to-azure-classic/test-failover2.png)

3. Voortgang failover op het tabblad **taken** .

    ![Virtuele machines toevoegen](./media/site-recovery-vmware-to-azure-classic/test-failover3.png)

4. Nadat de overname is voltooid ook moet u de replica Azure zien machine worden weergegeven in de portal van Azure > **virtuele Machines**. Als u wilt een RDP-verbinding met de VM Azure initiëren moet u poort 3389 op het eindpunt VM openen.

5. Nadat u bent klaar is, wanneer failover bereikt de voltooien testfase, klik op volledige testen om te voltooien. Records in notities en sla eventuele opmerkingen die zijn gekoppeld aan de overname test.

6. Klik op **de toets overname is voltooid** om op te schonen automatisch de testomgeving. Nadat dit is gebeurd wordt de overname test de status van een **voltooid** weergegeven. Alle elementen of VMs automatisch gemaakt tijdens de test overname worden verwijderd. Houd er rekening mee dat als een test failover meer dan twee weken voordat blijft deze voltooid door te dwingen.



### <a name="run-an-unplanned-failover"></a>Een niet-geplande failover uitvoeren

Niet-geplande failover is gestart vanaf Azure en kan worden uitgevoerd, zelfs als de primaire site is niet beschikbaar.


1. De pagina **Herstel plannen** Selecteer van het abonnement en klik op **Failover** > **Niet-geplande Failover**.

    ![Virtuele machines toevoegen](./media/site-recovery-vmware-to-azure-classic/unplanned-failover1.png)

2. Als u bezig bent met het repliceren van VMware virtuele machines kunt u selecteren om te proberen en on-premises implementatie VMs afsluiten. Dit efficiënt mogelijk is en failover blijft of de hoeveelheid is geslaagd of niet. Als dit niet lukt details van deze fout wordt weergegeven op het tabblad **taken **> **Ongeplande failover-taken**.

    ![Virtuele machines toevoegen](./media/site-recovery-vmware-to-azure-classic/unplanned-failover2.png)

    >[AZURE.NOTE] Deze optie is niet beschikbaar als u bezig bent met het repliceren van fysieke servers. U moet u kunt uitproberen en afsluiten die handmatig indien mogelijk.

3. Controleer in **Failover bevestigen** of de richting van de failover (op Azure) en selecteer het herstelproces is punt die u wilt gebruiken voor de overname. Als u meerdere VM ingeschakeld wanneer u replicatie-eigenschappen hebt geconfigureerd, kunt u terugzetten naar de meest recente toepassing of vastlopen consistente herstel komma. U kunt ook selecteren **wijst u aangepaste herstel** herstellen op een eerder in tijd. Klik op het vinkje om te beginnen de overname.

    ![Virtuele machines toevoegen](./media/site-recovery-vmware-to-azure-classic/unplanned-failover3.png)

3. Wacht totdat de niet-geplande failover taak is voltooid. U kunt de voortgang van failover op het tabblad **taken** controleren. Houd er rekening mee dat zelfs als er fouten optreden tijdens een niet-geplande overname het abonnement herstel wordt uitgevoerd totdat deze voltooid is. U moet ook kunnen om te zien van de replica Azure machine worden weergegeven in de virtuele Machines in de portal van Azure.

### <a name="connect-to-replicated-azure-virtual-machines-after-failover"></a>Verbinding maken met Azure virtuele machines gerepliceerd na failover

De verbinding met gerepliceerd virtuele machines in Azure nadat failover hier is wat u nodig hebt:

1. Een verbinding met extern bureaublad moet worden ingeschakeld op de primaire computer.
2. Het Windows Firewall op de primaire computer toe te staan dat RDP.
3. Na failover moet u RDP toevoegen aan de openbare eindpunt voor Azure virtuele machine.

[Lees meer](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) over dit instelt.


## <a name="deploy-additional-process-servers"></a>Aanvullende CMYK-servers implementeren

Als u wilt schalen dat bij de implementatie dan 200 bron machines of de snelheid van de totale dagelijkse lospeuteren groter is dan 2 TB, moet u aanvullende CMYK-servers worden afgehandeld het netwerkverkeer. Voor het instellen van een aanvullende CMYK-server de vereisten in [aanvullende CMYK-servers](#additional-process-servers) controleren en volg de instructies voor het instellen van de processerver. Na het instellen van de server kunt u de machines bron om het te gebruiken.

### <a name="set-up-an-additional-process-server"></a>Een extra processerver instellen

U instellen een extra processerver als volgt:

- De wizard geïntegreerd om te configureren van een server management als alleen de processerver van een uitvoeren.
- Als u wilt repliceren met alleen de nieuwe processerver beheren moet u uw beveiligde machines hiervoor migreren.

### <a name="install-the-process-server"></a>Het processerver installeren

1. Download het geïntegreerd installatiebestand voor de installatie van de Site herstel onderdelen op de pagina snel starten. Voer setup uit.
2. Selecteer **aanvullende CMYK-servers toevoegen aan de nieuwe schaal out implementatie**in **voordat u begint** .

    ![Processerver toevoegen](./media/site-recovery-vmware-to-azure-classic/add-ps1.png)

3. Voltooi de wizard op dezelfde manier als u wanneer hebt u [instellen van](#step-5-install-the-management-server) de eerste management server.

4. Geef het IP-adres van de oorspronkelijke management server waarop u de configuratieserver en de wachtwoordzin geïnstalleerd in **Configuratiegegevens voor de Server** . Op de oorspronkelijke management server uitgevoerd ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** de wachtwoordzin verkrijgt.

    ![Processerver toevoegen](./media/site-recovery-vmware-to-azure-classic/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Machines voor het gebruik van de nieuwe processerver te migreren

1. Open **Configuration Servers** > **Server** > naam van de oorspronkelijke management server > **Server-gegevens**.

    ![Proces mailserver bijwerken](./media/site-recovery-vmware-to-azure-classic/update-process-server1.png)

2. Klik op **De Server proces wijzigen** naast de server die u wilt wijzigen in de lijst **Process Servers** .

    ![Proces mailserver bijwerken](./media/site-recovery-vmware-to-azure-classic/update-process-server2.png)

3. In **De Server proces wijzigen** > **Proces doelserver** selecteert u de nieuwe management server en selecteer vervolgens de virtuele machines waarmee de nieuwe processerver worden verwerkt. Klik op het pictogram van de informatie voor informatie over de server. De gemiddelde ruimte die nodig is om elke geselecteerde virtuele machine gerepliceerd naar de nieuwe processerver wordt weergegeven om te laden beslissingen. Klik op het vinkje om te beginnen repliceren naar de nieuwe processerver.

    ![Proces mailserver bijwerken](./media/site-recovery-vmware-to-azure-classic/update-process-server3.png)




## <a name="vmware-permissions-for-vcenter-access"></a>VMware machtigingen voor vCenter toegang

De processerver kan VMs automatisch detecteren op een server vCenter. Als u wilt uitvoeren van automatische detectie moet u een rol (Azure_Site_Recovery) op het niveau van de vCenter toe te staan dat herstel van de Site voor toegang tot de server vCenter definiëren. Als u alleen moet VMware machines migreren naar Azure en niet hoeft te failback van Azure, kunt u een alleen-lezen rol dat groot genoeg is. U de machtigingen hebt ingesteld, zoals wordt beschreven in [stap 6: instellen van referenties voor de server vCenter](#step-6-set-up-credentials-for-the-vcenter-server) de rolmachtigingen in de volgende tabel worden samengevat.

**Rol** | **Meer informatie** | **Machtigingen**
--- | --- | ---
Azure_Site_Recovery rol | VMware VM discovery |Deze bevoegdheden voor de server in het midden v toewijzen:<br/><br/>Gegevensopslag -> toewijzen ruimte, bladeren gegevensopslag, laag niveau bestand bewerkingen., bestand verwijderen, Update VM bestanden<br/><br/>Netwerk -> netwerk toewijzen<br/><br/>Resource -> VM toewijzen aan resourcegroep, migreren VM uitgeschakeld, migreren VM ingeschakeld<br/><br/>Taken maken taak, updatetaak -><br/><br/>VM-configuratie ><br/><br/>VM-beheren > -> vraag beantwoorden, apparaat verbinding., CD configureren media, configureren diskette media uitschakelen, Power op, installeert u de hulpmiddelen voor VMware<br/><br/>VM -> voorraad maken, Register, registratie -><br/><br/>VM -> Provisioning -> toestaan VM downloaden, toestaan VM bestanden uploaden<br/><br/>VM -> momentopnamen -> momentopnamen verwijderen
vCenter gebruikersrol | VMware VM discovery/Failover zonder afsluiten van bron VM | Deze bevoegdheden voor de server in het midden v toewijzen:<br/><br/>Data Center object –> doorgeven aan onderliggende Object, rol = alleen-lezen <br/><br/>De gebruiker is toegewezen aan datacenter niveau en kan dus toegang heeft tot alle objecten in het datacenter.  Als u de toegang beperken wilt, kunt u de beheerdersrol **geen toegang** met het object **doorgeven aan kind** toewijzen aan de onderliggende objecten (ESX hosts, datastores, VMs en netwerken te gebruiken).
vCenter gebruikersrol | Failover en foutherstel | Deze bevoegdheden voor de server in het midden v toewijzen:<br/><br/>Datacenter van de object-doorgeven aan onderliggende object, rol = Azure_Site_Recovery<br/><br/>De gebruiker is toegewezen aan datacenter niveau en kan dus toegang heeft tot alle objecten in het datacenter.  Als u beperken van toegang wilt, moet u de rol **geen toegang **met de **doorgeven aan onderliggende object** toewijzen aan het onderliggende object (ESX hosts, datastores, VMs en netwerken te gebruiken).  



## <a name="third-party-software-notices-and-information"></a>Kennisgeving over software van derden en informatie

Niet doen vertalen of Localize

De software en firmware die wordt uitgevoerd in de Microsoft-product of service is gebaseerd op of materiaal uit de onderstaande projecten implementeren (gezamenlijk "derden Code').  Microsoft is niet oorspronkelijke auteur van de Code van derden.  De oorspronkelijke copyrightmelding en de licentie, waaronder Microsoft dergelijke Code van derden, ontvangen worden vierde hieronder ingesteld.

De informatie in de sectie A, wordt met betrekking tot onderdelen van derden Code uit de onderstaande projecten. Deze licenties en de gegevens worden alleen ter informatie gegeven.  Deze derden-Code is voor u wordt relicensed door Microsoft onder de licentievoorwaarden voor Microsoft software voor de Microsoft-product of service.  

De informatie in de sectie B betreft derden Code-onderdelen die beschikbaar worden gesteld aan u door Microsoft onder de oorspronkelijke licentievoorwaarden.

Het volledige bestand kan worden gevonden op het [Microsoft Downloadcentrum](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft behoudt alle rechten niet specifiek verleend hierin, of door implicatie wordt ' estoppel ' of anderszins.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over failback](site-recovery-failback-azure-to-vmware-classic.md) hoe u kunt uw mislukte overbrengen in Azure-computers terug naar uw on-premises omgeving.
