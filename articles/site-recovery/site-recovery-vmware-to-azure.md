<properties
    pageTitle="VMware virtuele machines en fysieke servers naar Azure met Azure Site herstel repliceren in de portal van Azure | Microsoft Azure"
    description="Wordt beschreven hoe u het implementeren van Azure Site herstellen om de goedkeuringen herhaling, failover en herstel van on-premises implementatie VMware virtuele machines en Windows/Linux fysieke servers Azure met behulp van de Azure portal"
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
    ms.date="08/12/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-machines-to-azure-with-azure-site-recovery-using-the-azure-portal"></a>VMware virtuele machines en fysieke machines repliceren naar Azure met Azure Site herstel met behulp van de Azure portal

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmware-to-azure.md)
- [Azure klassieke](site-recovery-vmware-to-azure-classic.md)
- [Azure klassieke (verouderd)](site-recovery-vmware-to-azure-classic-legacy.md)

Welkom bij Azure Site herstel! Lees dit artikel als u wilt repliceren on-premises implementatie VMware virtuele machines of Windows/Linux fysieke servers naar Azure Azure Site herstel gebruiken in de portal van Azure.

> [AZURE.NOTE] Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken en werken met resources: Azure Resource Manager (ARM) en klassiek. Azure, heeft ook twee portals – de Azure klassieke portal die ondersteuning biedt voor het implementatiemodel klassieke en de Azure-portal met ondersteuning voor beide implementatiemodellen.

Herstel van de site van de Azure-portal vindt u een aantal nieuwe functies:

- De services Azure back-up en herstellen van Azure-Site worden gecombineerd tot een enkel herstel Services kluis, zodat u kunt instellen en beheren van bedrijfscontinuïteit en herstel (BCDR) vanaf één locatie. U kunt in het geïntegreerd dashboard bewaken en bewerkingen beheren binnen uw on-premises implementatie-sites en de Azure openbare cloud.
- Gebruikers met Azure abonnementen deze is ingericht met het programma dat Cloud oplossing (CSP) van kunnen Site herstel bewerkingen in de portal van Azure nu beheren.
- Herstel van de site in de portal van Azure kunt machines gerepliceerd naar ARM opslag-accounts. Failover wordt uitgevoerd maakt herstel van de Site op ARM gebaseerde VMs in Azure wordt aangegeven.
- Site-herstel blijft ondersteuning voor replicatie naar klassieke opslag-accounts. Herstel van de Site maakt failover, VMs met het klassieke model.

Na het lezen van dit artikel bericht commentaar onderaan in de opmerkingen Disqus. Technische vragen op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Overzicht

Organisaties moeten een BCDR strategie die bepaalt hoe apps, werkbelasting en gegevens blijven actief en beschikbaar tijdens geplande en ongeplande downtime en herstellen naar de normale arbeidsvoorwaarden zo snel mogelijk. Uw strategie BCDR moet u bedrijfsgegevens veilige en worden hersteld en ervoor te zorgen dat werkbelasting continu beschikbaar blijven wanneer noodgevallen plaatsvindt.

Herstel van de site is een Azure-service vergeleken met uw strategie BCDR door orchestrating herhaling van fysieke servers on-premises implementatie en virtuele machines in de cloud (Azure) of met een secundaire datacenter. Wanneer er bijvoorbeeld optreden in uw hoofdlocatie bent, u niet de secundaire locatie wilt behouden apps en werkbelasting beschikbaar. U terug naar uw hoofdlocatie bent niet en er wordt naar de normale bewerkingen. Klik hier als u meer wilt weten in [Wat Azure Site herstel is?](site-recovery-overview.md)

In dit artikel vindt alle gegevens die u moet repliceren on-premises implementatie VMware VMs en Windows/Linux fysieke servers Azure. Het bevat een overzicht architectuur, plannen, informatie en implementatiestappen voor het configureren van Azure, on-premises-servers, replicatie-instellingen en capaciteit plannen. Nadat u de infrastructuur hebt ingesteld, kunt u replicatie op computers die u wilt beveiligen en controleer dat die failover werkt inschakelen.

## <a name="business-advantages"></a>Voordelen voor uw bedrijf

- Site-herstel biedt buiten het bedrijf bescherming voor bedrijven werkbelasting en toepassingen op VMware VMs en fysieke servers.
- De portal herstel Services biedt één enkele locatie als u wilt instellen, beheren en herhaling, failover en herstel controleren.
- Site-herstel kan VMware VMs toegevoegd aan vSphere hosts automatisch detecteren.
- U kunt eenvoudig failovers uit de infrastructuur van uw on-premises implementatie uitvoeren naar Azure en foutherstel (herstellen) van Azure met VMware VM servers in uw on-premises site.
- U kunt meerdere VM en herhaling groepen maken, zodat de toepassing werkbelasting doorverbonden over meerdere computers repliceren op hetzelfde moment. Alle computers in een groep herhaling hebt vastlopen consistente en app-consistente herstel punten wanneer ze mislukken. Voor failover, kunt u meerdere computers in herstel-pakketten verzamelen zodat doorverbonden toepassing werkbelasting samen mislukken.

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

Dit zijn de onderdelen scenario:

- **Configuratie-server**: een on-premises implementatie machine die coördinaten communicatie en processen voor het replicatie en herstel van gegevens beheren. Op deze computer voert u uit een enkel installatiebestand om de configuratieserver en deze extra onderdelen te installeren:
    - **Processerver**: fungeert als een gateway herhaling. Deze herhaling gegevens ontvangt van beveiligde bron machines, optimaliseert met caching, compressie en versleuteling en verzonden naar Azure opslag. Ook omgaat met push-installatie van de service mobiliteit voor beveiligde computers en automatische detectie van VMware VMs voert. De standaard-proces-server is geïnstalleerd op de configuratieserver. U kunt extra zelfstandige proces servers als u wilt schalen dat uw implementatie kunt implementeren.
    - **Basispagina doelserver**: herhaling gegevens tijdens failback van Azure verwerkt.

- **Mobiliteit service**: dit onderdeel wordt geïmplementeerd op elke computer (VMware VM of fysieke server) die u wilt repliceren naar Azure. Er wordt vastgelegd schrijft gegevens op de computer en ze doorstuurt naar de processerver.
- **Azure**: U hoeft te maken van een VMs Azure om af te handelen herhaling en overname bij storing naar Azure.  U hoeft een Azure een Azure opslag-account voor het opslaan van gerepliceerde gegevens en een Azure virtuele netwerk zodat Azure VMs zijn verbonden met een netwerk na failover-abonnement. De opslag-account en netwerk, moeten in dezelfde gebied, als de kluis herstel Services.
- **Failback**: wanneer u naar de site van uw on-premises implementatie van Azure mislukt na een failover klaar bent, moet u een VM Azure als tijdelijke processerver maken. Wanneer failback voltooid is, kunt u deze verwijderen. Voor failback moet u eveneens een VPN-(of Azure ExpressRoute) verbinding tussen uw on-premises site en het Azure netwerk waarin uw VMs Azure bevinden. Als failback-verkeer intensief is moet u mogelijk ook voor het instellen van een speciale outmodel doelserver lokale computer. Voor lichtere verkeer kan de standaard basispagina doelserver uitgevoerd op de configuratieserver worden gebruikt.


De afbeelding ziet u de manier waarop deze onderdelen werken.

![architectuur](./media/site-recovery-vmware-to-azure/v2a-architecture-henry.png)

**Afbeelding 1: VMware/fysieke naar Azure**

## <a name="azure-prerequisites"></a>Azure vereisten

Hier ziet u wat u nodig hebt in Azure wordt aangegeven om te implementeren in dit scenario.

**Vereiste** | **Meer informatie**
--- | ---
**Azure-account**| U moet een [Microsoft Azure](http://azure.microsoft.com/) -account. U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstelproces is Site prijzen.
**Azure-opslag** | Gerepliceerde gegevens worden opgeslagen in de opslagruimte van de Azure en Azure VMs zijn gemaakt bij een storing. <br/><br/>Om op te slaan gegevens moet u een standaard- of de premium-opslag-account in hetzelfde gebied, als de kluis herstel Services.<br/><br/>U kunt een LRS of GRS opslag-account. Het is raadzaam GRS zodat gegevens robuuste als er een regionale storing optreedt of als de primaire regio kan niet worden hersteld. [Meer informatie](../storage/storage-redundancy.md).<br/><br/> [Premium opslag](../storage/storage-premium-storage.md) wordt meestal gebruikt voor virtuele machines die een consistente IO krachtige en lage latentie naar host IO intensief werkbelasting nodig.<br/><br/> Als u een premium-account gebruiken om op te slaan gerepliceerde gegevens wilt, moet u ook een standaard-opslag-account om op te slaan replicatie-logboeken die lopende wijzigingen in de on-premises gegevens vastleggen.<br/><br/> Houd er rekening mee dat opslag-accounts die zijn gemaakt in de portal van Azure via resourcegroepen kunnen niet worden verplaatst. Beveiliging op premium opslag accounts in Centraal India en Zuid-India wordt ook momenteel niet ondersteund.<br/><br/> [Lees over](../storage/storage-introduction.md) Azure opslagruimte.
**Azure-netwerk** | U moet een Azure virtuele netwerk dat Azure VMs maakt verbinding met bij een storing. Het Azure virtuele netwerk moet zich in hetzelfde gebied, als de kluis herstel Services.
**Failback van Azure** | U moet een tijdelijk processerver instellen als een VM Azure. Wanneer u klaar bent om te mislukt terug en verwijdert u deze wanneer fail terug voltooid is, kunt u dit maken.<br/><br/> Als u wilt mislukt moet terug u een VPN-verbinding (of Azure ExpressRoute) van de Azure netwerk de on-premises implementatie-site.

## <a name="configuration-server--scale-out-process-prerequisites"></a>Configuratieserver / schaal out proces vereisten

U kunt instellen op een lokale computer als de configuratieserver / schalen processerver.

**Vereiste** | **Meer informatie**
--- | ---
**Configuratieserver voor**| Moet u een on-premises implementatie fysieke of virtuele machine met Windows Server 2012 R2. Alle onderdelen on-premises implementatie herstel van de Site zijn op deze computer geïnstalleerd.<br/><br/>Voor VMware VM replicatie, wordt u aangeraden dat u de server als een hoge beschikbaarheid VMware VM implementeren. Als u bezig bent met het repliceren van fysieke machines kan een fysieke server zijn op de computer.<br/><br/> Failback naar de site on-premises implementatie van Azure is altijd VMware VMs ongeacht of u via VMs of fysieke servers is mislukt. Als u niet de configuratieserver als een VMware VM moet u een afzonderlijk diamodel doelserver instellen als een VM VMware failback verkeer ontvangen implementeren.<br/><br/>Als de server een VM VMware is, moet het type netwerkadapter VMXNET3. Als u een ander type netwerkadapter gebruiken moet u een [VMware bijwerken](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1) op de server vSphere 5.5 te installeren.<br/><br/>De server moet hebben een statische IP-adres.<br/><br/>De server mag geen een domeincontroller.<br/><br/>De hostnaam van de server moet 15 tekens of minder.<br/><br/>Het besturingssysteem moet alleen Engels zijn.<br/><br/> U moet VMware vSphere PowerCLI 6.0 installeren. Klik op de configuratieserver.<br/><br/>De configuratieserver moet internettoegang. Uitgaande toegang is vereist als volgt:<br/><br/>Tijdelijke access op http-80 tijdens de installatie van de onderdelen van de Site herstel (om te downloaden MySQL)<br/><br/>Lopend uitgaande toegang via HTTPS 443 voor replicatiebeheer<br/><br/>Lopend uitgaande toegang via HTTPS 9443 voor replicatieverkeer (deze poort kan worden aangepast)<br/><br/>De server moet ook toegang tot de volgende URL's, zodat deze verbinding met Azure maken kunt: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net<br/><br/>Als u IP-adres gebaseerde firewallregels op de server hebt, controleert u dat de regels communicatie met Azure toestaan. Moet u toestaan dat het [IP-bereiken gebruikt Azure Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) en de (443) HTTPS-protocol.<br/><br/>IP-adresbereiken voor Azure buiten het gebied van uw abonnement en voor West Amerikaans toestaan.<br/><br/>Toestaan dat deze URL voor de MySQL-download:.http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi


## <a name="vmware-vcentervsphere-host-prerequisites"></a>VMware vCenter/vSphere host vereisten

**Vereiste** | **Meer informatie**
--- | ---
**vSphere**| U moet een of meer VMware vSphere hypervisors.<br/><br/>Hypervisors moet vSphere versie 6.0, 5.5 of 5.1 worden uitgevoerd met de meest recente updates.<br/><br/>Het is raadzaam dat uw vSphere hosts en vCenter servers in hetzelfde netwerk als de processerver bevinden zich (dit is het netwerk waarin de configuratieserver zich bevindt tenzij u een speciale processerver hebt ingesteld).
**vCenter** | Het is raadzaam dat u een VMware vCenter server als u wilt beheren van uw hosts vSphere implementeert. Moet dat deze vCenter versie 6.0 of 5.5 worden uitgevoerd met de meest recente updates.<br/><br/>Houd er rekening mee dat sites worden hersteld biedt geen ondersteuning voor nieuwe vCenter en vSphere 6.0 functies zoals cross vCenter vMotion, virtuele volumes en opslag DRS. Ondersteuning voor site-herstel is beperkt tot functies die ook beschikbaar in versie 5.5 zijn.


## <a name="protected-machine-prerequisites"></a>Beveiligde machine vereisten

**Vereiste** | **Meer informatie**
--- | ---
**On-premises (VMware VMs)** | VMware VMs die u wilt beveiligen moet hebben VMware extra geïnstalleerd en uit te voeren.<br/><br/> Computers die u wilt beveiligen, moeten voldoen met [Azure vereisten](site-recovery-best-practices.md#azure-virtual-machine-requirements) voor het maken van Azure VMs.<br/><br/>Afzonderlijke schijfcapaciteit op beveiligde computers niet mag zijn meer dan 1023 GB. Een VM kan maximaal 64 schijven hebben (dus maximaal 64 TB). <br/><br/>Minimale 2 GB beschikbare ruimte voor de installatie voor de installatie van onderdelen.<br/><br/>Beveiliging van VMs met versleutelde schijven wordt niet ondersteund.<br/><br/>Gedeeld schijf Gast clusters worden niet ondersteund.<br/><br/>**Poort 20004** moet worden geopend in de beveiligde VM lokale firewall, als u wilt inschakelen **consistentie van toepassing**.<br/><br/>Machines waarvoor Unified Extensible Firmware Interface UEFI () / Extensible Firmware Interface(EFI) opstarten wordt niet ondersteund.<br/><br/>Namen van de computer moeten tussen 1 en 63 tekens (letters, cijfers en afbreekstreepjes) bevatten. De naam moet beginnen met een letter of getal en eindigen met een letter of een getal. Nadat u replicatie voor een computer hebt ingeschakeld, kunt u de naam van de Azure wijzigen.<br/><br/>Als de bron VM NIC-koppeling heeft wordt deze geconverteerd naar een enkel NIC na failover naar Azure.<br/><br/>Als beveiligde virtuele machines een iSCSI-schijf hebt vervolgens converteert sites worden hersteld de beveiligde VM iSCSI-schijf naar een bestand VHD wanneer de VM Azure overgenomen wordt. Als het iSCSI-doel kunt bereikbaar zijn via de VM Azure wordt er brengen en in principe Zie twee schijven – de schijf VHD de VM Azure en de bron iSCSI-schijf. In dit geval moet u het iSCSI-doel dat wordt weergegeven op de VM Azure verbreken.
**Windows-computers (fysiek of VMware)** | De computer moet worden uitgevoerd op een ondersteunde 64-bits besturingssysteem wordt uitgevoerd: Windows Server 2012 R2, Windows Server 2012 of Windows Server 2008 R2 met bij minimaal SP1.<br/><br/> Het besturingssysteem moeten worden geïnstalleerd op de schijf C:\. De schijf OS moet een eenvoudige Windows-schijf en geen dynamische. De gegevensschijf kan zijn dynamische.<br/><br/>Site-herstel ondersteunt VMs op een dvd RDM. Tijdens failback malen Site-herstel de schijf RDM als de oorspronkelijke bron VM en RDM schijf beschikbaar is. Als ze zijn niet beschikbaar, tijdens failback maakt herstel van de Site een nieuw bestand VMDK voor elke schijf.
**Linux machines** (phyical of VMware)|  Moet u een ondersteunde 64-bits besturingssysteem wordt uitgevoerd: Red rol Enterprise Linux 6.7,7.1,7.2; Centos 6.5, 6.6,6.7,7.0,7.1,7.2; Oracle Enterprise Linux 6.4, 6.5 waarop rood rol compatibel kernel of Unbreakable Enterprise Kernel Release 3 (UEK3), SUSE Linux Enterprise Server 11 SP3.<br/><br/>/ etc/hosts-bestanden op beveiligde machines mogen bevatten gegevens die de lokale hostnaam aan IP-adressen die is gekoppeld aan alle netwerkadapters toewijzen.<br/><br/>Als u wilt verbinden met een Azure virtuele machines waarop Linux na failover Secure Shell mailclient (ssh), zorg ervoor dat de Secure Shell-service op de computer van de beveiligde is ingesteld om te beginnen automatisch op het opstarten en dat firewallregels toestaan een ssh verbinding toe.<br/><br/>De hostnaam, punten, apparaat, en Linux systeempaden bestands- en mapnamen (bijvoorbeeld/enzovoort /; /usr) moet zijn in het Engels alleen.<br/><br/>Beveiliging kan alleen worden ingeschakeld voor Linux machines met de volgende opslag: File system (EXT3, ETX4, ReiserFS, XFS); Paden software-apparaat toewijzing (paden)); Volume manager: (LVM2). Fysieke servers met HP CCISS controller opslagmedia worden niet ondersteund. Het bestandssysteem ReiserFS wordt alleen ondersteund op SUSE Linux Enterprise Server 11 SP3.<br/><br/>Site-herstel ondersteunt VMs op een dvd RDM.  Herstel van de Site niet opnieuw gebruiken de schijf RDM tijdens failback voor Linux. In plaats daarvan deze Hiermee maakt u een nieuw bestand VMDK voor elke corresponderende RDM schijf.<br/><br/>Zorg ervoor dat u de instelling disk.enableUUID=true in de configuratieparameters van de VM in VMware instellen. Maak de invoer als deze niet bestaat. Deze geven een consistente UUID de VMDK zodat deze correct koppelt aan het nodig is. Deze instelling toe te voegen ook zorgt ervoor dat alleen deltawijzigingen worden overgebracht terug naar de on-premises implementatie tijdens failback en niet een volledige replicatie.
**Mobiliteit-Service** |  **Windows**: gaat u als u wilt automatisch de service mobiliteit push naar VMs waarop u een beheerdersaccount (lokale beheerder op de Windows-computer) opgeven moet dat de processerver kan een push-installatie Windows wordt uitgevoerd.<br/><br/>**Linux**: om automatisch de service mobiliteit naar VMs waarop Linux u een account maken die kan worden gebruikt door de processerver moet om een push-installatie.<br/><br/> Standaard worden alle schijven op een computer gerepliceerd. [Een dvd van replicatie uitsluiten](#exclude-disks-from-replication), moet de service mobiliteit handmatig worden geïnstalleerd op de computer vóór het inschakelen van herhaling.<br/>

## <a name="prepare-for-deployment"></a>Voorbereiden voor implementatie

Voorbereiden voor implementatie, u moet:

1. [Een Azure netwerk instellen](#set-up-an-azure-network) waarin Azure VMs zich bevinden wanneer ze bent of omhoog na failover. Daarnaast voor failback moet u voor het instellen van een VPN-verbinding (of Azure ExpressRoute) van de Azure netwerk naar uw on-premises implementatie-site.
2. [Een account Azure opslag instellen](#set-up-an-azure-storage-account) voor gerepliceerde gegevens.
3. [Een account voorbereiden](#prepare-an-account-for-automatic-discovery) op de server vCenter of vSphere host zodat Site herstel kan automatisch detecteren VMware VMs die zijn toegevoegd.
4. [De configuratieserver voorbereiden](#prepare-the-configuration-server) om ervoor te zorgen dat deze kan toegang tot de vereiste URL's en vSphere PowerCLI 6.0 installeren.


### <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen

- Het netwerk moet in hetzelfde Azure gebied, als dat waarin u de kluis herstel Services wordt implementeert.
- Afhankelijk van de resource model dat u gebruiken wilt voor is mislukt via Azure VMs, moet u het Azure network [ARM](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) of [klassieke modus](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)instellen.
- Mislukken van Azure terug naar de site van uw on-premises implementatie VMware moet u een VPN-verbinding (of een verbinding Azure ExpressRoute) van de Azure netwerk waarin de gerepliceerde Azure VMs bevinden, naar de on-premises netwerk waarin de configuratieserver zich bevindt.
- [Algemene informatie over](../vpn-gateway/vpn-gateway-site-to-site-create.md) de ondersteunde implementatie-modellen VPN naar website verbindingen, en hoe u [een verbinding instellen](../vpn-gateway/vpn-gateway-site-to-site-create.md#create-your-virtual-network).
- U kunt ook [Azure ExpressRoute](../expressroute/expressroute-introduction.md)instellen. [Meer informatie](../expressroute/expressroute-howto-vnet-portal-classic.md) over het instellen van een Azure netwerk met ExpressRoute.

> [AZURE.NOTE] [Migratie van netwerken](../resource-group-move-resources.md) via resourcegroepen binnen hetzelfde abonnement of via abonnementen wordt niet ondersteund voor netwerken die zijn gebruikt voor het implementeren van sites worden hersteld.

### <a name="set-up-an-azure-storage-account"></a>Een Azure opslag-mailaccount instellen

- U moet een standaard- of een premium Azure opslag-account hebben voor gegevens gerepliceerd naar Azure. Het account moet zich in dezelfde gebied, als de kluis herstel Services. Afhankelijk van de resource model dat u gebruiken wilt voor is mislukt via Azure VMs, stelt u een account in [ARM](../storage/storage-create-storage-account.md) of [klassieke modus](../storage/storage-create-storage-account-classic-portal.md).
- Als u een premium-account voor gerepliceerde gegevens die u moet maken van een extra standaard account om op te slaan replicatie-logboeken die lopende wijzigingen in de on-premises gegevens vastleggen gebruikt.  

> [AZURE.NOTE] [Migratie van opslag accounts](../resource-group-move-resources.md) via resourcegroepen binnen hetzelfde abonnement of via abonnementen wordt niet ondersteund voor opslag-accounts gebruikt voor het implementeren van sites worden hersteld.

### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voor automatische detectie voorbereiden

De Site herstel proces-server kunt VMware VMs automatisch detecteren in vSphere hosts of op een vCenter-server die worden beheerd hosts. Als u wilt uitvoeren automatische discovery Site herstel referenties die toegang tot de VMware-server. Dit is niet relevant als u alleen de fysieke machines bent repliceren.

1. Als u wilt gebruiken een speciale rekening voor automatische detectie maken van een rol (bijvoorbeeld Azure_Site_Recovery) op het niveau van de vCenter met de [vereiste machtigingen](#vmware-account-permissions).
2. Maak een nieuwe gebruiker op de vSphere-host of vCenter-server en de rol toewijzen aan de gebruiker. U laten later Site herstel weten over deze referenties, zodat deze kan automatische detectie uitvoeren.

    >[AZURE.NOTE] Een gebruikersaccount vCenter met een alleen-lezen rol failover kan worden uitgevoerd, maar kan niet worden beveiligd bron machines afgesloten. Als u wilt afsluiten die machines moet u de rol [Azure_Site_Recovery](#vmware-account-permissions) . Als u alleen VMs VMware naar Azure migreert en te failback hoeft zijn de rol alleen-lezen is voldoende.

### <a name="prepare-the-configuration-server"></a>De configuratieserver voorbereiden

1.  Zorg ervoor dat de computer die u voor de configuratieserver gebruikt aan de [vereisten voldoet](#configuration-server-prerequisites). Met name zorg ervoor dat de computer is verbonden met internet met de volgende instellingen:

    - Toegang tot deze URL's toestaan: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net
    - Toegang tot [http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi](http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi) MySQL downloaden toestaan.
    - Toestaan dat firewall communicatie met Azure met de [Azure datacenter IP-bereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653) en het (443) HTTPS-protocol.

2.  Download en installeer [VMware vSphere PowerCLI 6.0](https://developercenter.vmware.com/tool/vsphere_powercli/6.0) op de configuratieserver. (Momenteel andere versies van PowerCLI worden niet ondersteund, met inbegrip van R-versies van versie 6.0.)


## <a name="create-a-recovery-services-vault"></a>Een kluis herstel Services maken

1. Meld u aan bij de [portal van Azure](https://portal.azure.com).
2. Klik op **nieuwe** > **Management** > **back-up- en Site-herstel (OMS)**. U kunt ook klikken op **Bladeren** > **Herstel Services kluis** > **toevoegen**.

    ![Nieuwe kluis](./media/site-recovery-vmware-to-azure/new-vault3.png)

3. Geef in het vak **naam** een beschrijvende naam voor de kluis. Als u meer dan één abonnement hebt, selecteert u een van deze.
4. [Een nieuwe resourcegroep maken](../resource-group-template-deploy-portal.md) of Selecteer een bestaande eigenschap. Geef een Azure regio. Machines worden gerepliceerd naar dit gebied. Houd er rekening mee dat Azure opslagruimte en netwerken die worden gebruikt voor herstel van de Site moeten worden in hetzelfde gebied. Als u wilt controleren bekijken ondersteunde regio's geografische beschikbaarheid in [Azure Site herstel prijzen Details](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Als u wilt snel toegang tot de kluis vanuit het Dashboard klikt u op **vastmaken aan dashboard** en klik vervolgens op **maken**.

    ![Nieuwe kluis](./media/site-recovery-vmware-to-azure/new-vault-settings.png)

De nieuwe kluis wordt weergegeven op het **Dashboard** > **alle resources**, en klik op het hoofdweergavegebied **herstel Services kluizen** blade.

## <a name="getting-started"></a>Aan de slag

Herstel van de site biedt een aan de slag-ervaring is ontworpen om u en uit te voeren zo snel mogelijk. Er controleert de vereisten en begeleidt u bij de stappen die u nodig hebt om sites worden hersteld geïmplementeerd.

Selecteer het type van machines die u wilt repliceren en waar u wilt repliceren naar. U instellen kunt de infrastructuur van on-premises-servers, Azure instellingen herhaling beleidsregels en capaciteitsplanning. Nadat uw infrastructuur schakelt u replicatie voor VMs en fysieke servers. U kunt vervolgens failovers voor specifieke machines uitvoeren of herstel abonnementen mislukt op meerdere computers maken.

Begint aan de slag door te kiezen hoe u wilt implementeren sites worden hersteld. De stroom aan de slag verandert enigszins afhankelijk van uw replicatievereisten voor.


## <a name="step-1-choose-your-protection-goals"></a>Stap 1: Uw doelstellingen beveiliging kiezen

Selecteer wat u wilt repliceren en waar u wilt repliceren naar.

1. Klik in het blad **herstel Services kluizen** uw kluis en klik op **Instellingen**.
2. In **Instellingen** > **Aan de slag** Klik op **Site-herstel** > **stap 1: voorbereiden infrastructuur** > **beveiliging doel**.

    ![Kies doelstellingen](./media/site-recovery-vmware-to-azure/choose-goals.png)

3. Selecteer **Naar Azure**in **beveiliging doel** en selecteer **Ja, met VMware vSphere Hypervisor**. Klik vervolgens op **OK**.

    ![Kies doelstellingen](./media/site-recovery-vmware-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Stap 2: De bronomgeving configureren

De configuratieserver instellen en registreren in de kluis herstel Services. Als u bezig bent met het repliceren van VMware VMs het VMware-account dat u gebruikt voor automatische detectie opgeven.

1. Klik op **stap 1: voorbereiden infrastructuur** > **bron**. Klik in **voorbereiden bron** als u niet over een configuratieserver op **+ configuratieserver voor** een toe te voegen.

    ![Bron instellen](./media/site-recovery-vmware-to-azure/set-source1.png)

2. In de **Server toevoegen** blade controleren weergegeven die **Configuratieserver** in **Server Typ**.
3. Controleer of [vereisten](#configuration-server-prerequisites)voordat u de configuratieserver instelt. In bepaalde selectievakje dat de computer toegang hebben tot de gewenste URL's.
4.  Download het installatiebestand Site herstel Unified instellen.
5.  Download de registratie-toets kluis. U moet dit wanneer u Unified Setup uitvoert. De sleutel is ongeldig voor 5 dagen nadat u deze hebt gegenereerd.

    ![Bron instellen](./media/site-recovery-vmware-to-azure/set-source2.png)

6.  Op de computer die u als de configuratieserver gebruikt, Unified Setup uit om te installeren van de configuratieserver, de processerver en de basispagina doelserver.


### <a name="run-site-recovery-unified-setup"></a>Locatie-herstel Unified instellen

1.  Voer het installatiebestand Unified-instelling.
2.  Selecteer **de configuratieserver en processerver installeren**in **voordat u begint** .

    ![Voordat u begint](./media/site-recovery-vmware-to-azure/combined-wiz1.png)

3. Klik op **ik ga akkoord** als u wilt downloaden en installeren van MySQL van **Derden Software-licentie** .

    ![= Derde partij software](./media/site-recovery-vmware-to-azure/combined-wiz105.PNG)

4. Blader in **registratie** en selecteer de registratie-sleutel die u hebt gedownload van de kluis.

    ![Registratie](./media/site-recovery-vmware-to-azure/combined-wiz3.png)

5. Geef op hoe de Provider die wordt uitgevoerd op de configuratieserver maakt verbinding met Azure Site herstel via internet op **Instellingen voor Internet** .

    - Als u verbinding met de proxy die momenteel ingesteld op de computer wilt selecteert u **verbinding maken met bestaande proxy-instellingen**.
    - Als u wilt dat de Provider verbinding selecteert u **verbinding maken rechtstreeks zonder proxy**.
    - Als de bestaande proxy is verificatie vereist of als u wilt een aangepaste proxy gebruiken voor de verbinding Provider, selecteert u **verbinding maken met aangepaste proxy-instellingen**.
        - Als u een aangepaste proxy gebruiken moet u het adres, poorten en referenties opgeven
        - Als u een proxyserver gebruikt hebt u al mogen de URL's die worden beschreven in [vereisten](#configuration-server-prerequisites).

    ![Firewall](./media/site-recovery-vmware-to-azure/combined-wiz4.png)

6. **Controle van vereisten** setup wordt uitgevoerd een selectievakje om ervoor te zorgen dat installatie kan worden uitgevoerd. Als er een waarschuwing over het **synchroniseren van de globale tijd controleren verschijnt** controleert u of die de tijd op de systeemklok (**datum en tijd** -instellingen) is hetzelfde als de gewenste tijdzone.

    ![Vereisten voor](./media/site-recovery-vmware-to-azure/combined-wiz5.png)

7. Maak in **MySQL-configuratie** referenties voor aanmelding bij de MySQL-server-instantie die wordt geïnstalleerd.

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz6.png)

8. In **De Details van de omgeving** selecteren of u gaat VMware VMs repliceren. Als u bent, controleert setup dat PowerCLI 6.0 is geïnstalleerd.

    ![MySQL](./media/site-recovery-vmware-to-azure/combined-wiz7.png)

9. In **Installatielocatie** selecteren waar u de binaire bestanden installeren en de cache opslaan. Kunt u een station met ten minste 5 GB opslagruimte beschikbaar, maar het is raadzaam een cache-station met ten minste 600 GB beschikbare ruimte te.

    ![Locatie installeren](./media/site-recovery-vmware-to-azure/combined-wiz8.png)

10. Geef de luisteraar ervan af (netwerkadapter en SSL-poort) waarop de configuratieserver worden gegevens verzenden en ontvangen herhaling in **Netwerk selecteren** . Kunt u de standaard poort (9443). Naast deze poort, worden poort 443 gebruikt door een webserver die orchestrates herhaling bewerkingen. 443 niet mag worden gebruikt voor het ontvangen van replicatie-verkeer is toegestaan.


    ![Netwerk selecteren](./media/site-recovery-vmware-to-azure/combined-wiz9.png)



11.  Controleer de gegevens in het **Overzicht** en klikt u op **installeren**. Wanneer de installatie is voltooid wordt een wachtwoordzin gegenereerd. U moet deze wanneer u replicatie inschakelt zodat deze kopiëren en op een veilige locatie bewaren.

    ![Overzicht](./media/site-recovery-vmware-to-azure/combined-wiz10.png)

12.  Nadat de registratie de server is voltooid wordt weergegeven in de **Instellingen** > blade**Servers** in de kluis.



#### <a name="run-setup-from-the-command-line"></a>Het installatieprogramma uitvoert vanaf de opdrachtregel

U kunt de configuratieserver vanaf de opdrachtregel instellen:

    UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]

Parameters:

- / ServerMode: verplicht. Hiermee geeft u of de configuratie- en proces servers moeten worden geïnstalleerd of alleen het processerver. Invoerwaarden: CS, PS
- InstallLocation: verplicht. De map waarin de onderdelen zijn geïnstalleerd.
- / MySQLCredsFilePath. Verplicht. Het bestandspad waarin de referenties voor de MySQL-server zijn opgeslagen. Het bestand moet in deze indeling:
    - [MySQLCredentials]
    - MySQLRootPassword = "<Password>"
    - MySQLUserPassword = "<Password>"
- / VaultCredsFilePath. Verplicht. De locatie van het bestand kluis-referenties
- / EnvType. Verplicht. Het type installatie. Waarden: VMware, NonVMware
- / PSIP en /CSIP. Verplicht. Het IP-adres van het proces en configuratieserver.
- / PassphraseFilePath. Verplicht. De locatie van het bestand wachtwoordzin.
- / BypassProxy. Optioneel. Hiermee wordt opgegeven dat de configuratieserver verbinding met Azure zonder proxy maakt.
- / ProxySettingsFilePath. Optioneel. Proxy-instellingen (de standaardproxy vereist verificatie, of een aangepaste proxy). Het bestand moet in deze indeling:
    - [ProxySettings]
    - ProxyAuthentication = "Ja/Nee"
    - Proxy IP = "IP-adres >"
    - ProxyPort = "<Port>"
    - ProxyUserName = "<User Name>"
    - ProxyPassword = "<Password>"
- DataTransferSecurePort. Optioneel. Poortnummer moet worden gebruikt voor replicatiegegevens.
- SkipSpaceCheck. Optioneel. Controle van de ruimte voor cache overslaan.
- AcceptThirdpartyEULA. Verplicht. Markering houdt aanvaarding van derden gebruiksrechtovereenkomst.
- ShowThirdpartyEULA. Verplicht. Geeft de overeenkomst van derden. Als die is opgegeven als invoer worden alle andere parameters worden genegeerd.

### <a name="add-the-vmware-account-used-for-automatic-discovery"></a>Het VMware-account gebruikt voor automatische detectie toevoegen

 Wanneer u voorbereid voor implementatie kunt u [een account VMware gemaakt](#prepare-an-account-for-automatic-discovery) die Site herstel voor automatische detectie gebruiken kunt nodig hebt. Dit account als volgt toevoegen:

1. Open **CSPSConfigtool.exe**. Het is alleen beschikbaar als een snelkoppeling op het bureaublad en bevindt in de map van de \home\svsystems\bin [locatie installeren].
2. Klik op **Accounts beheren** > **Account toevoegen**.

    ![Account toevoegen](./media/site-recovery-vmware-to-azure/credentials1.png)

3. Toevoegen in **De Details van** het account dat wordt gebruikt voor automatische detectie. Houd er rekening mee dat deze kan 15 minuten of langer duren voordat de naam van het account wordt weergegeven in de portal. Als u direct bijwerken, klikt u op **Configuratie-Servers** > servernaam > **Server vernieuwen**.

    ![Meer informatie](./media/site-recovery-vmware-to-azure/credentials2.png)

### <a name="connect-to-vsphere-hosts-and-vcenter-servers"></a>Verbinding maken met vSphere hosts en vCenter servers

Als u bezig bent met het repliceren van VMware VMs verbinding maken met de vSphere hosts en vCenter-servers.

1. Controleer of dat de configuratieserver netwerktoegang tot de vSphere hosts en vCenter-servers heeft.
2. Klik op **de infrastructuur voorbereiden** > **bron**. Selecteer de configuratieserver in **voorbereiden bron** en klik op **+ vCenter** als een vSphere host of vCenter server wilt toevoegen.
3. Geef een beschrijvende naam voor de vSphere host of vCenter server in **vCenter toevoegen** en geef de IP-adres of FQDN-naam van de server. Laat de poort 443 tenzij uw VMware-servers worden geconfigureerd voor het luisteren naar aanvragen op een andere poort. Selecteer het account waarmee u verbinding maakt met de server VMware. Klik op **OK**.

    ![VMware](./media/site-recovery-vmware-to-azure/vmware-server.png)

    >[AZURE.NOTE] Als u de vCenter-server of vSphere host met een account dat u geen hebt op de server vCenter of host beheerdersbevoegdheden toevoegt, klikt u vervolgens ervoor zorgen dat het account deze ingeschakeld heeft: Datacenter, gegevensopslag, map, Host, netwerk, Resource, virtuele machine, vSphere verdeeld veranderen. De server vCenter moet bovendien de bevoegdheden van de weergaven opslag.

Herstel van de site maakt verbinding met VMware-servers met de instellingen die u hebt opgegeven en VMs ontdekt.

## <a name="step-3-set-up-the-target-environment"></a>Stap 3: De doelomgeving configureren

Controleer of dat u een account opslag voor replicatie en een Azure netwerk waarmee Azure VMs verbinding maakt na failover hebt.

1.  Klik op **de infrastructuur voorbereiden** > **doeltoepassing** en selecteer het Azure abonnement dat u wilt gebruiken.
2.  Geef de implementatiemodel dat u wilt gebruiken voor VMs na failover.
3.  Herstel van de site wordt gecontroleerd dat er een of meer compatibele Azure opslag accounts en netwerken.

    ![Doel](./media/site-recovery-vmware-to-azure/gs-target.png)

4.  Als u een account opslag nog niet hebt gemaakt en u wilt maken van een ARM gebruikt klikt u op **+ opslag account** moet die inline.  Geef een accountnaam, type, abonnement en locatie op het blad **opslag-account maken** . Het account moet zich in hetzelfde gebied, als de kluis herstel Services.

    ![Opslag](./media/site-recovery-vmware-to-azure/gs-createstorage.png)

    Houd rekening met:

    - Als u wilt een opslag account maken met het klassieke model kunt u dit doen in de portal van Azure. [Meer informatie](../storage/storage-create-storage-account-classic-portal.md)
    - Als u een premium-opslag-account gebruikt voor gerepliceerde gegevens moet u een extra opslagruimte van de standaard-mailaccount wilt instellen logboeken store herhaling die vastleggen lopende wijzigingen on-premises gegevens.

    > [AZURE.NOTE] Beveiliging op premium opslag accounts in Centraal India en Zuid-India is momenteel niet ondersteund.

4.  Selecteer een Azure-netwerk. Als u een netwerk dat nog niet hebt gemaakt en u wilt doen die gebruikt ARM klikt u op **+ netwerk** moet die inline. Geef de netwerknaam van een, adresbereiken subnet details, abonnement en locatie op het blad **virtueel netwerk maken** . Het netwerk moet op dezelfde locatie als het herstelproces is Services kluis.

    ![Netwerk](./media/site-recovery-vmware-to-azure/gs-createnetwork.png)

    Als u wilt maken van een netwerk met behulp van het klassieke model kunt u dit doen in de portal van Azure. [Meer informatie](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

## <a name="step-4-set-up-replication-settings"></a>Stap 4: Herhaling instellingen instellen

1. U maakt een nieuwe herhaling beleid klikt u op **voorbereiden infrastructuur** > **Replicatie-instellingen** > **+ maken en koppelen**.
2. Geef de naam van een beleid in **maken en koppelen beleid** .
3. **Vrijgegeven Productieorder**drempelwaarde: de limiet vrijgegeven Productieorder opgeven. Waarschuwingen worden gegenereerd wanneer continue herhaling deze limiet overschrijdt.
5. Geef in **herstel wijst u bewaren**, op binnen enkele uren hoe lang het bewaarbeleid-venster wordt voor elk opsommingsteken herstel. Beveiligde machines kunnen herstellen naar een willekeurige plaats in een venster. Maximaal 24 uur bewaarbeleid wordt ondersteund voor machines gerepliceerd naar premium opslag.
6. Geef in de **App-consistente momentopname frequentie**, hoe vaak (in minuten) herstel punten die van toepassing-consistente momentopnamen wordt gemaakt.
7. Wanneer u een replicatiebeleid maken, al dan niet standaard is een overeenkomende beleid automatisch gemaakt voor failback. Tot voorbeeld als het replicatiebeleid is **verkoper-beleid** wordt de failbackbeleid **verkoper-beleid-failback**. Dit beleid wordt niet gebruikt totdat u een failback initiëren.  
8. Klik op **OK** om het beleid te maken.

    ![Replicatiebeleid](./media/site-recovery-vmware-to-azure/gs-replication2.png)

9. Wanneer u een nieuw beleid maakt, is deze automatisch gekoppeld aan de configuratieserver. Klik op **OK**.

    ![Replicatiebeleid](./media/site-recovery-vmware-to-azure/gs-replication3.png)


## <a name="step-5-capacity-planning"></a>Stap 5: Capaciteit plannen

Nu dat u uw basic hebt infrastructuur instellen u kunt denken over het plannen van capaciteit en bepalen of u aanvullende informatie nodig hebt.

Herstel van de site biedt een planner capaciteit waarmee u de juiste resources toewijzen voor uw bronomgeving, de site herstel onderdelen, netwerken en opslag. U kunt de planner uitvoeren in de snelle modus voor schattingen op basis van een gemiddelde aantal VMs, schijven en opslag of in de gedetailleerde weergave waarin u afbeeldingen op het niveau van de werkbelasting kunt invoert. Voordat u begint moet u:

- Verzamel informatie over uw replicatieomgeving, inclusief VMs, schijven per VMs en opslagruimte per schijf.
- Schatting van de dagelijkse wijzigen (lospeuteren)-mate dat u voor gerepliceerde gegevens hebt. U kunt de [capaciteit van vSphere toestel planning](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) gebruiken om te helpen u dit kunt doen.

1.  Klik op **downloaden** om te downloaden van het hulpmiddel en voer dit uit. [Lees het artikel](site-recovery-capacity-planner.md) waarop het hulpmiddel.
2.  Als u klaar bent met Selecteer **Ja** **hebt u doorgenomen capaciteit plannen?**

    ![Capaciteit plannen](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)

De onderstaande tabel wordt een aantal van punten om u te helpen met capaciteit, planning voor dit scenario vastgelegd.


**Onderdeel** | **Meer informatie**
--- | --- | ---
**Herhaling** | **Dagelijks maximum snelheid wijzigen**, een beveiligde machine alleen de beschikking over één processerver en een enkel proces-server kan een dagelijks verwerken wijzigen classificeren maximaal 2 TB. 2 TB is dus dat de maximale dagelijkse gegevens rente die wordt ondersteund voor een beveiligde machine wijzigen.<br/><br/> **Maximum doorvoer**, een gerepliceerde machine kunt toevoegen aan één opslag-account in Azure wordt aangegeven. Een standaard-opslag-account kan maximaal 20.000 aanvragen per seconde worden verwerkt en het is raadzaam dat u het aantal IO's / s over een bron-machine aan 20.000 behouden. Voor voorbeeld als u een bron-machine met 5 schijven en elke schijf hebt gegenereerd 120 IO's / s (8 kB grootte) op de bron, wordt de parameter zijn binnen de Azure per schijf IO's / s limiet van 500. Het getal = totale bron IO's / s/20000 opslag-accounts die zijn vereist.
**Configuratieserver voor** | De configuratieserver moet kunnen worden afgehandeld het dagelijkse wijzigen tarief capaciteit over alle werkbelasting in het beveiligde computers waarop en moet voldoende bandbreedte continu gegevens worden gerepliceerd naar Azure opslag.<br/><br/> Als een goede gewoonte is het raadzaam dat de configuratieserver zich bevinden op de hetzelfde netwerk en LAN-segment als de machines die u wilt beveiligen. Dit kan zich bevinden in een netwerk met een ander maar machines die u wilt beveiligen L3 netwerk zichtbaarheid ernaar nodig hebt.<br/><br/> Aanbevelingen voor de configuratieserver voor de grootte worden in de onderstaande tabel samengevat.
**Processerver** | De eerste processerver is geïnstalleerd op de configuratieserver standaard. U kunt aanvullende CMYK-servers als u wilt schalen van uw omgeving implementeren. Houd rekening met:<br/><br/> De processerver herhaling gegevens ontvangt van beveiligde machines en optimaliseert met caching, compressie en versleuteling voor verzenden naar Azure. De computer van de server proces moet hebben voldoende bronnen die u wilt deze taken uitvoeren.<br/><br/> Het processerver gebruikt schijfcache. Het is raadzaam een afzonderlijke cache-dvd van 600 GB of meer wijzigingen aan de gegevens die zijn opgeslagen in het geval van netwerk knelpunt of -storing verwerken.

### <a name="size-recommendations-for-the-configuration-server"></a>Grootte aanbevelingen voor de configuratieserver

**CPU** | **Geheugen** | **Cachegrootte Schijfopruiming** | **De snelheid van gegevens wijzigen** | **Beveiligde machines**
--- | --- | --- | --- | ---
8 vCPUs (2-sockets * 4 cores @ 2,5 GHz) | 16 GB | 300 GB | 500 GB of kleiner | Minder dan 100 machines repliceren.
12 vCPUs (2-sockets * 6 cores @ 2,5 GHz) | 18 GB | 600 GB | 500 GB tot 1 TB | Repliceren tussen 100-150 machines.
16 vCPUs (2-sockets * 8 cores @ 2,5 GHz) | 32 GB | 1 TB | 1 TB tot 2 TB | Repliceren tussen 150-200 machines.
Een ander processerver implementeren | | | > 2 TB | Aanvullende CMYK-servers implementeren als u meer dan 200 machines bent repliceren, of als de dagelijkse gegevens wijzigt tarief groter is dan 2 TB.

Waarbij geldt:

- Elke broncomputer is geconfigureerd met 3 schijven van 100 GB.
- We benchmarking opslag van 8 SA's stations van 10 K RPM gebruikt met RAID 10 voor cache schijf metingen.

### <a name="size-recommendations-for-the-process-server"></a>Grootte aanbevelingen voor de processerver

Als u wilt beveiligen met meer dan 200 machines of de snelheid van dagelijkse wijzigen groter dan 2 TB is kunt u aanvullende CMYK-servers voor het verwerken van de belasting herhaling toevoegen. Als u wilt verkleinen out u kunt u in het volgende doen:

- Vergroot het aantal configuratieservers. U kunt bijvoorbeeld beveiligen maximaal 400 machines met twee configuratieservers.
- Aanvullende CMYK-servers toevoegen en typt u deze zo instellen dat verkeer in plaats van (of naast) worden de configuratieserver.

Deze tabel worden een scenario waarbij beschreven:

- U bent niet van plan de configuratieserver gebruiken als een processerver.
- U kunt een aanvullende CMYK-server hebt ingesteld.
- U hebt beveiligde virtuele machines gebruik van de aanvullende CMYK-server configureren.
- Elke broncomputer beveiligde is geconfigureerd met drie schijven van 100 GB.

**Configuratieserver voor** | **Aanvullende CMYK-server**| **Cachegrootte Schijfopruiming** | **De snelheid van gegevens wijzigen** | **Beveiligde machines**
--- | --- | --- | --- | ---
8 vCPUs (2-sockets * 4 cores @ 2,5 GHz), 16 GB geheugen vereist | 4 vCPUs (2-sockets * 2 cores @ 2,5 GHz), 8 GB geheugen vereist | 300 GB | 250 GB of kleiner | Repliceren 85 of minder machines.
8 vCPUs (2-sockets * 4 cores @ 2,5 GHz), 16 GB geheugen vereist | 8 vCPUs (2-sockets * 4 cores @ 2,5 GHz), 12 GB geheugen vereist | 600 GB | 250 GB tot 1 TB | Repliceren tussen 85-150 machines.
12 vCPUs (2-sockets * 6 cores @ 2,5 GHz), 18 GB geheugen vereist | 12 vCPUs (2-sockets * 6 cores @ 2,5 GHz) 24 GB geheugen vereist | 1 TB | 1 TB tot 2 TB | Repliceren tussen 150-225 machines.


De manier waarop u uw servers schalen wordt, hangt af van uw voorkeur voor een schaal van of schalen out model.  U omhoog met het implementeren van een paar geavanceerde configuratie en proces servers wilt verkleinen, of met het implementeren van meer servers met minder resources wilt verkleinen. Bijvoorbeeld: als u wilt beveiligen 220 machines kunt u een van de volgende kunt doen:

- De configuratieserver met 12vCPU, 18 GB geheugen, een processerver extra met 12vCPU, 24 GB geheugen, instellen en beveiligde machines alleen de aanvullende CMYK-server configureren.
- U kunt ook kan u twee configuratieservers (2 x 8vCPU, 16 GB RAM) en twee aanvullende CMYK-servers (1 x 8vCPU) en 4vCPU x 1 worden afgehandeld 135 + 85 (220) machines configureren en beveiligde machines als u wilt gebruiken, alleen de aanvullende CMYK-servers configureren.

[Volg deze instructies](#deploy-additional-process-servers) voor het instellen van een aanvullende CMYK-server.

### <a name="network-bandwidth-considerations"></a>Aandachtspunten voor de bandbreedte netwerk

U kunt de capaciteit teamplanner hulpmiddel voor het berekenen van de bandbreedte die u nodig voor replicatie (eerste replicatie en klik vervolgens delta hebt). Als u wilt bepalen welke bandbreedtegebruik voor replicatie hebt u een paar opties:

- **De bandbreedte**: VMware verkeer die wordt overgenomen door Azure doorloopt in een specifieke processerver. U kunt de bandbreedte op de computers met als proces servers beperken.
- **Bandbreedte van invloed zijn op**: kunt u de bandbreedte gebruikt voor replicatie met een paar registersleutels beïnvloeden:
    - De registerwaarde **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\UploadThreadsPerVM** Hiermee geeft u het aantal threads die worden gebruikt voor de overdracht van gegevens (eerste of delta replicatie) van een schijf. Een hogere waarde Hiermee verhoogt u de netwerkbandbreedte gebruikt voor replicatie.
    - De **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\DownloadThreadsPerVM** Hiermee geeft u het aantal threads gebruikt voor de overdracht van gegevens tijdens failback.

#### <a name="throttle-bandwidth"></a>De bandbreedte

1. Open de module MMC van Microsoft Azure back-up op de computer die fungeert als de processerver. Een snelkoppeling voor back-up van Microsoft Azure is al dan niet standaard beschikbaar op het bureaublad of in C:\Program Files\Microsoft Azure herstel Services Agent\bin\wabadmin.
2. Klik op **Eigenschappen wijzigen**in de module.

    ![De bandbreedte](./media/site-recovery-vmware-to-azure/throttle1.png)

3. Klik op het tabblad **Throttling** Selecteer **internetbandbreedte beperken voor back-bewerkingen inschakelen**, en de limieten voor werk instellen en niet-werk uren. Geldige bereiken zijn 512 k tot 102 Mbps per seconde.

    ![De bandbreedte](./media/site-recovery-vmware-to-azure/throttle2.png)

U kunt ook de cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) gebruiken om in te stellen beperken. Hier volgt een voorbeeld:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting-NoThrottle** wordt aangegeven dat er geen beperking vereist is.


#### <a name="influence-network-bandwidth"></a>Invloed netwerkbandbreedte

1. Ga in het register naar **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
    - Beïnvloeden van de bandbreedte-verkeer is toegestaan op een schijf vermenigvuldigt de waarde de **UploadThreadsPerVM**wijzigen of de sleutel maken als deze niet bestaat.
    - Als u wilt de bandbreedte voor failback verkeer van Azure van invloed zijn op, moet u de waarde **DownloadThreadsPerVM**wijzigen.
2. De standaardwaarde is 4. In een netwerk 'overprovisioned' worden deze registersleutels gewijzigd van de standaardwaarden. Het maximale aantal is 32. Verkeer naar het optimaliseren van de waarde bewaken.

## <a name="step-6-replicate-applications"></a>Stap 6: Repliceren toepassingen

Zorg ervoor dat machines die u wilt repliceren zijn voorbereid voor de installatie van service mobiliteit en herhaling in te schakelen.

### <a name="install-the-mobility-service"></a>De service mobiliteit installeren

De eerste stap bij het inschakelen van beveiliging voor virtuele machines en fysieke servers is de mobiliteit-service installeren. U kunt dit op een aantal manieren doen:

- **Proces server push**: wanneer u replicatie op een computer inschakelt, push en installeer het onderdeel van de service mobiliteit van de processerver. Houd er rekening mee dat doen push-installatie zich niet als machines al een omhoog todate-versie van het onderdeel gebruikt.
- **Enterprise push**: automatisch Installeer het onderdeel met behulp van uw onderneming push-processen zoals WSUS of System Center Configuration Manager of [Azure automatisering en configuratie van de gewenste staat](./site-recovery-automate-mobility-service-install.md). Stel de configuratieserver voordat u dit doet.
- **Handmatige installatie**: het onderdeel handmatig installeren op elke computer waarop u wilt repliceren. Stel de configuratieserver voordat u dit doet.


#### <a name="prepare-for-automatic-push-on-windows-machines"></a>Voorbereiden voor automatische push op Windows-computers

Hier volgt informatie over voor het voorbereiden van Windows-computers, zodat de mobiliteit-service automatisch kan worden geïnstalleerd door het processerver.

1.  Maak een account die kan worden gebruikt door de processerver voor toegang tot de computer. Het account moet beheerdersbevoegdheden (lokaal of domein) en deze wordt alleen gebruikt voor de push-installatie.

    >[AZURE.NOTE] Als u een account niet gebruikt moet u de toegang van externe gebruikers besturingselement op de lokale computer uitschakelen. Klik hiertoe in het register onder HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System de DWORD-vermelding toevoegen LocalAccountTokenFilterPolicy met een waarde van 1. De registervermelding toevoegen vanuit een type CLI **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**.

2.  Klik op het Windows Firewall van de computer die u wilt beveiligen, selecteert u **toestaan een app of functie via Firewall**. **Bestands- en printerdeling** en **WMI**inschakelen. U kunt de firewallinstellingen configureren met een GPO voor machines die deel uitmaakt van een domein.

    ![Firewallinstellingen](./media/site-recovery-vmware-to-azure/mobility1.png)

2. Het account dat u hebt gemaakt toevoegen:

    - Open **cspsconfigtool**. Het is alleen beschikbaar als een snelkoppeling op het bureaublad en bevindt in de map van de \home\svsystems\bin [locatie installeren].
    - Klik op het tabblad **Accounts beheren** op **Account toevoegen**.
    - Het account dat u hebt gemaakt toevoegen. Na het toevoegen van het account dat moet u de referenties opgeven wanneer u replicatie voor een machine inschakelen.


#### <a name="prepare-for-automatic-push-on-linux-servers"></a>Voorbereiden voor automatische push op Linux-servers

1.  Zorg ervoor dat de Linux-computer die u wilt beveiligen, zoals is beschreven in de [beveiligde machine vereisten](#protected-machine-prerequisites)wordt ondersteund. Controleer of er netwerkconnectiviteit tussen de Linux-computer en de processerver.

2.  Maak een account die kan worden gebruikt door de processerver voor toegang tot de computer. Het account moet een gebruiker hoofdmap op de bronserver Linux en deze wordt alleen gebruikt voor de push-installatie.

    - Open **cspsconfigtool**. Het is alleen beschikbaar als een snelkoppeling op het bureaublad en bevindt in de map van de \home\svsystems\bin [locatie installeren].
    - Klik op het tabblad **Accounts beheren** op **Account toevoegen**.
    - Het account dat u hebt gemaakt toevoegen. Na het toevoegen van het account dat moet u de referenties opgeven wanneer u replicatie voor een machine inschakelen.

3.  Schakel op het bestand/etc/hosts op de bronserver Linux bevat gegevens die de lokale hostnaam aan IP-adressen die is gekoppeld aan alle netwerkadapters toewijzen.
4.  Installeer de meest recente openssh, openssh-server, openssl-pakketten op de computer die u wilt repliceren.
5.  Controleer of dat SSH is ingeschakeld en wordt uitgevoerd op poort 22.
6.  SFTP subsysteem en wachtwoord voor verificatie in het bestand sshd_config als volgt inschakelen:

    - Meld u aan als de hoofdmap.
    - Zoek in het bestand /etc/ssh/sshd_config-bestand, de regel die met **PasswordAuthentication begint**.
    - Verwijder de opmerkingen bij de lijn en wijzig de waarde van **geen** op **Ja**.
    - Zoek de regel die met **subsysteem begint** en verwijder de opmerkingen bij de regel.

        ![Linux](./media/site-recovery-vmware-to-azure/mobility2.png)


### <a name="install-the-mobility-service-manually"></a>De Service mobiliteit handmatig installeren

Het installatieprogramma's zijn beschikbaar op de server Configuration in **C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.

Bron-besturingssysteem | Servicebestand voor installatie mobiliteit
--- | ---
Windows Server (alleen 64 bits) | Microsoft-ASR_UA_9. *.0.0_Windows_* release.exe
CentOS 6.4, 6.5, 6.6 (alleen 64 bits) | Microsoft-ASR_UA_9. *.0.0_RHEL6-64_*release.tar.gz
SUSE Linux Enterprise Server 11 SP3 (alleen 64 bits) | Microsoft-ASR_UA_9. *.0.0_SLES11-SP3-64_*release.tar.gz
Oracle Enterprise Linux 6.4, 6.5 (alleen 64 bits) | Microsoft-ASR_UA_9. *.0.0_OL6-64_*release.tar.gz


#### <a name="install-mobility-service-on-a-windows-server"></a>Mobiliteit Service installeren op een Windows-Server


1. Download en voer het installatieprogramma van relevante.
2. Selecteer **mobiliteit-service**in **voordat u begint** .

    ![Mobiliteit-service](./media/site-recovery-vmware-to-azure/mobility3.png)

3. Geef het IP-adres van de configuratieserver en de wachtwoordzin die is gegenereerd toen u Unified Setup hebt uitgevoerd in **Configuratiegegevens voor de Server** . U kunt de wachtwoordzin ophalen door te voeren: ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – v** op de configuratieserver.

    ![Mobiliteit-service](./media/site-recovery-vmware-to-azure/mobility6.png)

4. Laat de standaardinstelling in **Locatie installeren** en klik op **volgende** om installatie te starten.
5. Voortgang van uw **Installatie** installatie kan worden bewaakt en start de computer opnieuw op als u hierom wordt gevraagd. Na de installatie van de service kunt duurt ongeveer 15 minuten totdat de status om bij te werken in de portal.

#### <a name="install-mobility-service-on-a-windows-server-using-the-command-prompt"></a>Mobiliteit-Service installeren op een Windows-server met de opdrachtprompt

1. Kopieer het installatieprogramma van met een lokale map (bijvoorbeeld C:\Temp) op de server die u wilt beveiligen. Het installatieprogramma vindt u op de Server Configuration onder de **[locatie installeren] \home\svsystems\pushinstallsvc\repository**. Het pakket voor de Windows-besturingssystemen heeft een naam die vergelijkbaar is met Microsoft-ASR_UA_9.3.0.0_Windows_GA_17thAug2016_release.exe
2. **Wijzig de naam van** dit bestand naar MobilitySvcInstaller.exe
3. Voer de volgende opdracht kunt ophalen van de MSI installer </br>

        C:\> cd C:\Temp
        C:\Temp> MobilitySvcInstaller.exe /q /xC:\Temp\Extracted
        C:\Temp> cd Extracted
        C:\Temp\Extracted> UnifiedAgent.exe /Role "Agent" /CSEndpoint "IP Address of Configuration Server" /PassphraseFilePath <Full path to the passphrase file>

#####<a name="full-command-line-syntax"></a>De syntaxis van de volledige opdrachtregel

    UnifiedAgent.exe [/Role <Agent/MasterTarget>] [/InstallLocation <Installation Directory>] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>] [/LogFilePath <Log File Path>]<br/>

**Parameters**

- **/Role:** Verplicht. Hiermee geeft u of de service mobiliteit moet worden geïnstalleerd. Invoerwaarden Agent | MasterTarget
- **/InstallLocation:** Verplicht. Geeft aan waar de service installeren.
- **/PassphraseFilePath:** Verplicht. De configuratie server wachtwoordzin.
- **/LogFilePath:** Verplicht. De locatie waar de installatielogboekbestanden moeten worden gemaakt.



#### <a name="uninstall-mobility-service-manually"></a>Mobiliteit service handmatig verwijderen

Mobiliteit-Service kan worden verwijderd, het programma toevoegen verwijderen via het Configuratiescherm of opdrachtregel gebruiken.

De opdracht verwijderen van de opdrachtregel met mobiliteit-Service is

    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1}


#### <a name="install-mobility-service-on-a-linux-server-using-command-line"></a>Mobiliteit-Service installeren op een Linux-server met opdrachtregel

1. Kopieer het juiste tar-archief op basis van de bovenstaande tabel met de Linux-computer die u wilt repliceren.
2. Open een shellprogramma en het archief ingepakte tar op een lokaal pad extraheren door te voeren:`tar -xvzf Microsoft-ASR_UA_8.5.0.0*`
3. Een bestand passphrase.txt maken in de lokale map waarin u de inhoud van het archief tar uitgepakt. Moet u doen dit kopieert de wachtwoordzin uit C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase op de configuratieserver en sla deze in passphrase.txt door te voeren *`echo <passphrase> >passphrase.txt`* in shell.
4. Installeer de service mobiliteit door te voeren *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Geef het interne IP-adres van de configuratieserver en zorg ervoor dat poort 443 is geselecteerd. Na de installatie van de service kunt duurt ongeveer 15 minuten totdat de status om bij te werken in de portal.

**U kunt ook installeren vanaf de opdrachtregel**:

1. Kopieer de wachtwoordzin uit C:\Program Files (x86) \InMage Systems\private\connection op de configuratieserver en opslaan als 'passphrase.txt' op de configuratieserver. Voer deze opdrachten. In ons voorbeeld is het IP-adres van de configuratie-server 104.40.75.37 en de HTTPS-poort 443 moet zijn:

Installeren op een productieserver:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt

Installeren op de basispagina doelserver:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 443 -s y -c https -P passphrase.txt


### <a name="enable-replication"></a>Replicatie inschakelen

#### <a name="before-you-start"></a>Voordat u begint

Als u VMware virtuele repliceren bent machines Houd rekening met het volgende:

- VMware VMs elke 15 minuten zijn gevonden en kan duren 15 minuten of langer worden weergegeven in de portal na discovery. Eveneens discovery kunt 15 minuten of langer duren wanneer u een nieuwe vCenter server of vSphere host toevoegt.
- De wijzigingen op de virtuele machine (zoals VMware hulpmiddelen voor installatie) kunnen ook duren 15 minuten of langer moeten worden bijgewerkt in de portal.
- U kunt de laatste keer dat ontdekt controleren voor VMware VMs in het veld **Laatste contactpersoon op** voor de host van de server/vSphere vCenter op het blad **Configuratie-Servers** .
- Als u wilt toevoegen machines voor replicatie zonder te wachten de geplande discovery, markeer de configuratieserver (Klik niet op deze) en klik op de knop **vernieuwen** .
- Wanneer u replicatie inschakelen als de computer is het processerver automatisch voorbereid, installeert de mobiliteit-service op is geïnstalleerd.

#### <a name="exclude-disks-from-replication"></a>Schijven uitsluiten van replicatie

Wanneer u replicatie inschakelt, al dan niet standaard worden alle schijven op een computer gerepliceerd. U kunt schijven uitsluiten van replicatie. Zoals u mogelijk niet wilt repliceren schijven met tijdelijke gegevens of gegevens die heeft vernieuwd telkens wanneer een computer of toepassing opnieuw wordt gestart (bijvoorbeeld pagefile.sys of SQL Server tempdb). Als u wilt uitsluiten schijven rekening met het volgende:

- U kunt alleen uitsluiten schijven die al op de mobiliteit-service is geïnstalleerd. U moet [de service mobiliteit handmatig installeren](#install-the-mobility-service-manually) omdat de mobiliteit-service is alleen is geïnstalleerd met behulp van de push om nadat replicatie is ingeschakeld.
- Alleen standaard schijven kunnen worden uitgesloten van replicatie. U kunt geen OS of dynamische schijven uitsluiten.
- Nadat de replicatie is ingeschakeld kunt u deze niet toevoegen of verwijderen van schijven voor replicatie. Als u wilt toevoegen of een schijf u moet beveiliging voor de computer uitschakelt en weer inschakelt deze uitsluiten.
- Als u een schijf die nodig is voor een toepassing voor uitsluiten, na failover naar Azure moet u deze handmatig maken in Azure wordt aangegeven dat de gerepliceerde toepassing kan worden uitgevoerd. U kunt ook kan u integreren met Azure Automatisering een abonnement herstel de schijf maken tijdens een overname van de computer.
- Schijven die u handmatig in Azure maken terug mislukt. Bijvoorbeeld als u meer dan drie schijven mislukt en maakt twee rechtstreeks in Azure wordt aangegeven, alle vijf mislukt terug. U kunt geen schijven handmatig gemaakt op basis van failback uitsluiten.

**Replicatie nu als volgt inschakelen**:

1. Klik op **stap 2: toepassing repliceren** > **bron**. Wanneer u replicatie voor de eerste keer hebt ingeschakeld moet u klikken op **+ repliceren** in de kluis replicatie voor extra machines inschakelen.
2. Klik in het blad **bron** > **bron** selecteren de configuratieserver.
3. Selecteer in **type computer** **virtuele Machines** of **Fysieke Machines**.
4. Selecteer de vCenter-server die de vSphere-host beheert in **vCenter/vSphere Hypervisor** of selecteert u de host. Deze instelling is van belang als u bezig bent met het repliceren van fysieke machines niet.
5. Selecteer de processerver. Dit is de naam van de configuratieserver als u eventuele aanvullende CMYK-servers hebt gemaakt. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. Selecteer het abonnement kluis in **doeltoepassing** en selecteer het model (classic of resource management) die u wilt gebruiken in Azure wordt aangegeven na failover in **Post-failover implementatiemodel** .
7. Selecteer het Azure opslag-account dat u gebruikt voor het repliceren van gegevens. Houd rekening met:

    - U kunt een standaard-opslag-account of premium selecteren. Als u een premium-account moet u een extra opslagruimte van de standaard-account voor lopende herhaling logboeken opgeven. Accounts moeten zich in hetzelfde gebied, als de kluis herstel Services.
    - Als u wilt gebruiken een andere opslag-account dan die hebt u u kunt [een account maakt](#set-up-an-azure-storage-account). Klik op **Nieuw**om een opslag account met behulp van het model ARM. Als u wilt een opslag account maken met het klassieke model doen kunt die [in de portal van Azure](../storage/storage-create-storage-account-classic-portal.md).

8. Selecteer de Azure netwerk en subnet waarmee Azure VMs verbinding maakt wanneer ze bent of omhoog na failover. Het netwerk moet zich in hetzelfde gebied, als de kluis herstel Services. Selecteer **configureren nu voor geselecteerde computers** de netwerkinstelling toepassen op alle computers die u selecteren voor de beveiliging. Selecteer **later configureren** om te selecteren van de Azure netwerk per computer. Als u niet over een netwerk moet u [een account maakt](#set-up-an-azure-network). Klik op **Nieuw**om een netwerk met behulp van het model ARM. Als u wilt maken van een netwerk met behulp van het klassieke model doen kunt die [in de portal van Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Selecteer een subnet, indien van toepassing. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-vmware-to-azure/enable-replication3.png)

9. In de **virtuele Machines** > **virtuele machines selecteren** op en selecteer elke computer die u wilt repliceren. U kunt alleen machines waarvoor replicatie kan worden ingeschakeld selecteren. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-vmware-to-azure/enable-replication5.png)

10. In de **Eigenschappen van** > **eigenschappen configureren**, selecteer het account dat wordt gebruikt door de processerver naar automatisch de service mobiliteit op de computer installeren. Standaard worden alle schijven gerepliceerd. Klik op **Alle schijven** en schakelt u niet wilt repliceren schijven. Klik vervolgens op **OK**. U kunt aanvullende eigenschappen later instellen.

    ![Replicatie inschakelen](./media/site-recovery-vmware-to-azure/enable-replication6.png)

11. In de **Instellingen voor replicatie** > **replicatie-instellingen configureren** Controleer of het juiste replicatiebeleid is geselecteerd. U kunt de beleidsinstellingen herhaling in **Instellingen**wijzigen > **herhaling beleidsregels** > beleidsnaam > **Instellingen bewerken**. Wijzigingen die u op een beleid toepast worden toegepast tot repliceren en nieuwe machines.

12. **Multi-VM consistentie** inschakelen als u wilt verzamelen machines naar een groep herhaling en geef een naam voor de groep. Klik vervolgens op **OK**. Houd rekening met:

    - Computers in herhaling repliceren groeperen en hebben gedeeld vastlopen consistente en app-consistente herstel punten wanneer ze mislukken.
    - Het is raadzaam dat u verzamelen VMs en fysieke servers zodat ze uw werkbelasting. Inschakelen multi-VM consistentie kan van invloed zijn op de prestaties van de werkbelasting en mag alleen worden gebruikt als machines de dezelfde werklast worden uitgevoerd en u consistentie nodig hebt.

    ![Replicatie inschakelen](./media/site-recovery-vmware-to-azure/enable-replication7.png)

13. Klik op **inschakelen herhaling**. U kunt de voortgang van de taak **Beveiliging inschakelen** in **Instellingen**bijhouden > **taken** > **Site herstel taken**. Nadat de taak **Voltooien beveiliging** wordt uitgevoerd van de computer is klaar voor failover.

> [AZURE.NOTE] Als de computer is voorbereid voor push-installatie die het onderdeel van de service mobiliteit wordt geïnstalleerd wanneer bescherming tegen is ingeschakeld. Na het onderdeel is geïnstalleerd op de computer die beveiliging taak begint en mislukt. Na de fout moet u elke computer handmatig opnieuw te starten. Na het opnieuw starten de taak beveiliging opnieuw begint en eerste replicatie plaatsvindt.

### <a name="view-and-manage-vm-properties"></a>Weergeven en beheren van VM eigenschappen

Het is raadzaam dat u de eigenschappen van de broncomputer controleren. Vergeet niet dat de naam van de Azure VM aan [vereisten Azure virtuele machines voldoen moet](site-recovery-best-practices.md#azure-virtual-machine-requirements).

1. Klik op **Instellingen** > **gerepliceerde items** > en selecteert u de computer. Het blad **Essentials** bevat informatie over instellingen voor machines en status.

2. In de **Eigenschappen** kunt u replicatie en overname bij storing informatie voor VM weergeven.

    ![Replicatie inschakelen](./media/site-recovery-vmware-to-azure/test-failover2.png)

3. In het **netwerk en rekenkracht** > **berekenen eigenschappen** kunt u de grootte van de naam en doelsites Azure VM opgeven. De naam om te voldoen aan vereisten voor Azure als u wilt wijzigen.
U kunt ook bekijken en toevoegen van informatie over de doelnetwerk, subnet en IP-adres dat wordt toegewezen aan de VM Azure. Houd rekening met het volgende:

    - U kunt het target IP-adres instellen. Als u niet een adres, de mislukte via machine opgeeft DHCP gebruikt. Als u een adres dat is niet beschikbaar bij failover hebt ingesteld, wordt de overname niet uitvoeren. Hetzelfde target IP-adres kan worden gebruikt voor test failover als het adres in het netwerk van de failover-test beschikbaar is.
    - Het aantal netwerkadapters wordt bepaald door de grootte die u voor de doeltoepassing virtuele machine, als volgt opgeeft:

        - Als het aantal netwerkadapters op de broncomputer kleiner dan of gelijk is aan het aantal adapters toegestaan voor de grootte van de computer doel is, klikt u vervolgens heeft het doel hetzelfde aantal adapters als de bron.
        - Als het aantal adapters voor de bron virtuele machine groter is dan het getal dat is toegestaan voor de doelgrootte en vervolgens de doeltoepassing maximaal wordt gebruikt.
        - Als u een bron-machine heeft twee netwerkadapters en de grootte van de computer doel ondersteunt vier, de doelcomputer heeft twee adapters bijvoorbeeld. Als de broncomputer twee adapters heeft, maar de doelgrootte van de ondersteunde alleen een ondersteunt wordt slechts één adapter hebben op de doelcomputer.     
    - Als de VM meerdere netwerkadapters die ze alle verbinding maken met hetzelfde netwerk heeft worden.

    ![Replicatie inschakelen](./media/site-recovery-vmware-to-azure/test-failover4.png)

4. In **schijven** kunt u het besturingssysteem en gegevensschijven bekijken op de VM die worden gerepliceerd.


## <a name="step-7-test-the-deployment"></a>Stap 7: De implementatie testen

U kunt een failover test voor een enkele virtuele machine of een herstel-abonnement met een of meer virtuele machines uitvoeren om te testen van de implementatie.


### <a name="prepare-for-failover"></a>Voorbereiden voor failover

- Als u wilt uitvoeren van een test failover is het raadzaam dat u een nieuw Azure netwerk die heeft geïsoleerd vanuit het Azure productienetwerk maken (dit is zich standaard gedraagt wanneer u een nieuw netwerk in Azure maken). [Meer informatie](site-recovery-failover.md#run-a-test-failover) over het uitvoeren van test failovers.
- Als u de beste prestaties wanneer u niet Azure, de Azure-Agent op de beveiligde computer te installeren. Wordt sneller opstarten en helpt bij het oplossen van. Installeer de [Linux](https://github.com/Azure/WALinuxAgent) of [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) -agent.
- Als u wilt uw implementatie volledig testen moet u een infrastructuur voor de gerepliceerde machine werkt zoals verwacht. Als u testen wilt, Active Directory en DNS kunt u een virtuele machine maken als een domeincontroller met DNS- en repliceren dit naar Azure met Azure sites worden hersteld. Lees meer in [test failover aandachtspunten voor Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Zorg ervoor dat de configuratieserver wordt uitgevoerd. Anders failover, mislukt.
- Als u schijven hebt uitgesloten van replicatie moet u mogelijk deze schijven handmatig maken in Azure wordt aangegeven na failover zodat de toepassing wordt uitgevoerd, zoals verwacht.
- Houd rekening met het volgende als u wilt uitvoeren van een niet-geplande failover in plaats van een failover testen:

    - Indien mogelijk moet u afgesloten primaire machines voordat u een niet-geplande failover uitvoert. Dit zorgt ervoor dat de bron- en replica machines die worden uitgevoerd op hetzelfde moment geen hebt. Als u bezig bent met het repliceren van VMware VMs kunt u vervolgens opgeven dat herstel van de Site een aanbevolen inspanning afsluiten de bron-machines stellen. Afhankelijk van de status van de primaire site mogelijk of werkt mogelijk niet. Als u bezig bent met het repliceren van fysieke servers niet mogelijk deze optie is sites worden hersteld.
    - Wanneer u een niet-geplande failover uitvoert wordt stopgezet repliceren van primaire computers zodat alle gegevens delta won't worden doorgeschakeld nadat een niet-geplande failover begint. Ook als u een niet-geplande failover op een abonnement herstel uitvoert wordt deze uitgevoerd totdat voltooid, zelfs als er een fout optreedt.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Verbinding maken met Azure VMs na failover voorbereiden

Als u wilt verbinden met Azure VMs met RDP na failover, controleert u of doen u het volgende:

Klik **op de lokale computer voordat failover wordt uitgevoerd**:

- Voor toegang via internet RDP inschakelen, zorg ervoor dat alle TCP- en UDP regels worden toegevoegd voor de **openbare**en zorg ervoor dat RDP is toegestaan in de **Windows Firewall** -> **toegestane apps en -functies** voor alle profielen.
- Voor toegang via de verbinding van een site-naar-site RDP inschakelen op de computer en zorg ervoor dat RDP is toegestaan in de **Windows Firewall** -> **toegestane apps en -functies** voor het **domein** en **particuliere** netwerken.
- Installeer de [Azure VM agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) op de lokale computer.
- [De service mobiliteit handmatig installeren](#install-the-mobility-service-manually) op computers in plaats van de processerver gebruiken om automatisch de service. Dit komt omdat de push-installatie is alleen gebeurt er als de computer is ingeschakeld voor replicatie.
- Zorg ervoor dat het besturingssysteem SAN-beleid is ingesteld op OnlineAll. [Meer informatie]( https://support.microsoft.com/kb/3031135)
- De IPSec-service uitschakelen voordat u de overname uitvoert.

**Klik op het Azure VM na failover**:

- Voeg een openbare eindpunt voor de RDP-protocol (poort 3389) en geef de referenties voor aanmelding.
- Controleer of er geen domeinbeleid dat voorkomen dat u verbinding maakt met een virtuele machine met een openbare adres.
- Maak verbinding met. Controleer of de VM wordt uitgevoerd als u geen verbinding maken. Lees dit [artikel](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)voor meer tips.


Als u toegang wilt tot een Azure VM waarop Linux na failover Secure Shell mailclient gebruikt (ssh), doet u het volgende:

Klik **op de lokale computer voordat failover wordt uitgevoerd**:

- Zorg ervoor dat de service Secure Shell op de VM Azure is ingesteld op automatisch starten op het opstarten.
- Controleer dat firewallregels een SSH-verbinding met deze.

**Klik op het Azure VM na failover**:

- De groep beveiligingsregels voor netwerken op de mislukte via VM en het Azure subnet waaraan dit is aangesloten moeten binnenkomende verbindingen met de poort SSH toestaan.
- Een openbare eindpunt moet worden gemaakt zodat binnenkomende verbindingen op de SSH poort (TCP-poort 22 al dan niet standaard).
- Als de VM toegankelijk is via een VPN-verbinding (Express Route of site naar site VPN) kan de client rechtstreeks via verbinding maken met de VM SSH worden gebruikt.

**Klik op het Azure Windows/Linux VM na failover**:

Als er een netwerk-beveiligingsgroep die is gekoppeld aan de virtuele Machine of het subnet waaraan de machine behoort, zorg ervoor dat de beveiligingsgroep van netwerk heeft een uitgaande regel toe te staan dat HTTP-/ HTTPS. Zorg ook dat de DNS-records van het netwerk met welke virtuele machine is ophalen niet meer dan juist is geconfigureerd. De overname kan anders met fout: 'PreFailoverWorkflow taak WaitForScriptExecutionTask time-out' time-out. Als u wilt weten over dit in detail, raadpleegt u sectie op herstel van de [controle- en de gids voor probleemoplossing](site-recovery-monitoring-and-troubleshooting.md#recovery).

## <a name="run-a-test-failover"></a>Een failover test uitvoeren

1. Mislukt op één computer, in **Instellingen** > **Gerepliceerd Items**, klikt u op de VM > **+ toets failover-** pictogram.

    ![Test failover](./media/site-recovery-vmware-to-azure/test-failover1.png)

2. Via een abonnement herstel, klikt u in **Instellingen**mislukt > **Herstel van abonnement**, met de rechtermuisknop op het abonnement > **Test Failover**. Als u wilt maken met een herstel plannen [Volg deze instructies](site-recovery-create-recovery-plans.md).

3. Selecteer het Azure netwerk waarnaar Azure VMs worden verbonden nadat storing in **Test Failover** .
4. Klik op **OK** om te beginnen met de overname. U kunt de voortgang bijhouden door te klikken op de VM om de eigenschappen te openen of op de **Toets Failover** taak in de naam van de kluis > **Instellingen** > **taken** > **herstel van de Site taken**.
5. Wanneer de overname de status **voltooid testen bereikt** , het volgende doen:

    1. Bekijk de replica virtuele machine in de portal van Azure. Controleer of de virtuele machine is gestart.
    2. Als u ingesteld access virtuele machines uit uw on-premises netwerk bent kunt u een verbinding met extern bureaublad naar de virtuele machine initiëren.
    3. Klik op **voltooid testen** om te voltooien.

        ![Test failover](./media/site-recovery-vmware-to-azure/test-failover6.png)


    4. Klik op **notities** als u wilt opnemen en sla eventuele opmerkingen die zijn gekoppeld aan de overname test.
    5. Klik op **de toets overname is voltooid** om op te schonen automatisch de testomgeving. Nadat dit is gebeurd wordt de overname test de status van een **voltooid** weergegeven.
    6.  In deze fase worden een bepaalde elementen of VMs automatisch aangemaakt door Site herstel tijdens een de overname test verwijderd. Eventuele aanvullende elementen die u hebt gemaakt voor test failover niet worden verwijderd.

    > [AZURE.NOTE] Als een test failover meer blijft dan twee weken voordat deze voltooid door te dwingen.


6. Nadat de overname is voltooid ook moet u de replica Azure zien machine worden weergegeven in de portal van Azure > **virtuele Machines**. U moet ervoor zorgen dat de VM de juiste grootte, heeft dit is verbonden met het juiste netwerk, is en dat deze wordt uitgevoerd.
7. Als u [ervoor dat voor verbindingen na failover](#prepare-to-connect-to-azure-vms-after-failover) u verbinding maken met de VM Azure moet.

## <a name="monitor-your-deployment"></a>Bewaak uw implementatie

Hier ziet u hoe u de configuratie-instellingen, status en servicestatus voor de implementatie van uw Site herstel kunt controleren:

1. Klik op de naam van de kluis voor toegang tot het dashboard **Essentials** . In dit dashboard kunt u de Site herstel taken, herhaling status, herstel-abonnementen, server gezondheid en gebeurtenissen.  U kunt Essentials om weer te geven van de tegels en -indelingen die voor u zijn, waaronder de status van andere sites worden hersteld en back-up kluizen nuttigste aanpassen.<br>
![Essentials](./media/site-recovery-vmware-to-azure/essentials.png)

2. U kunt siteservers (VMM of configuratie-servers) die voordoet zich probleem en de gebeurtenissen die door herstel van de Site in de afgelopen 24 uur controleren in de tegel van de **Servicestatus** .
3. U kunt beheren en herhaling in de **Items gerepliceerd**, **Herstel van abonnement**, controleren en **Site herstel taken** tegels. U kunt inzoomen in taken in **Instellingen** -> **taken** -> **Site herstel taken**.


## <a name="deploy-additional-process-servers"></a>Aanvullende CMYK-servers implementeren

Als u hebt in verkleinen out uw implementatie dan 200 bron machines of een totale dagelijkse lospeuteren tarief van meer dan 2 TB, moet u aanvullende CMYK-servers worden afgehandeld het netwerkverkeer.

De [grootte aanbevelingen voor proces servers](#size-recommendations-for-the-process-server) controleren en volg deze instructies voor het instellen van de processerver. Na het instellen van de server hebt u migreren machines bron om het te gebruiken.

### <a name="install-an-additional-process-server"></a>Een extra processerver installeren

1. In **Instellingen** > **Site herstel servers** klikt u op de configuratieserver > **proces-server**.

    ![Processerver toevoegen](./media/site-recovery-vmware-to-azure/migrate-ps1.png)

2. Klik op **proces-server (on-premises implementatie)**in **Servertype** .

    ![Processerver toevoegen](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

3. Download het installatiebestand Site herstel Unified en het processerver installeren en registreren in de kluis worden uitgevoerd.
4. Selecteer **aanvullende CMYK-servers toevoegen aan de nieuwe schaal out implementatie**in **voordat u begint** .
5. Voltooi de wizard op dezelfde manier als u wanneer hebt u [instellen van](#step-2-set-up-the-source-environment) de configuratieserver.

    ![Processerver toevoegen](./media/site-recovery-vmware-to-azure/add-ps1.png)

6. Geef het IP-adres van de configuratieserver en de wachtwoordzin in **Configuratiegegevens voor de Server** . Voor de wachtwoordzin uitvoeren ** <SiteRecoveryInstallationFolder>\home\sysystems\bin\genpassphrase.exe – n** op de configuratieserver.

    ![Processerver toevoegen](./media/site-recovery-vmware-to-azure/add-ps2.png)

### <a name="migrate-machines-to-use-the-new-process-server"></a>Machines voor het gebruik van de nieuwe processerver te migreren

1. In **Instellingen** > **herstel van de Site-servers** op de configuratieserver en vouwt u **proces-servers**.

    ![Proces mailserver bijwerken](./media/site-recovery-vmware-to-azure/migrate-ps2.png)

2. Met de rechtermuisknop op de processerver die momenteel in gebruik en **klikt u op**.

    ![Proces mailserver bijwerken](./media/site-recovery-vmware-to-azure/migrate-ps3.png)

3. Selecteer in het **proces doelserver selecteert**, de nieuwe processerver die u wilt gebruiken en selecteer vervolgens de virtuele machines waarmee de nieuwe processerver worden verwerkt. Klik op het pictogram van de informatie voor informatie over de server. Om te laden beslissingen, wordt de gemiddelde ruimte die nodig is om elke geselecteerde virtuele machine gerepliceerd naar de nieuwe processerver weergegeven. Klik op het vinkje om te beginnen repliceren naar de nieuwe processerver.

## <a name="vmware-account-permissions"></a>Machtigingen van de VMware-account

De processerver kan VMs automatisch detecteren op een server vCenter. Als u wilt uitvoeren van automatische detectie moet u het definiëren van [een rol (Azure_Site_Recovery)](#prepare-an-account-for-automatic-discovery) herstel van de Site voor toegang tot de VMware-server. Als u alleen moet VMware machines migreren naar Azure en niet hoeft te failback van Azure, kunt u een alleen-lezen rol dat groot genoeg is. De vereiste machtigingen worden in de volgende tabel samengevat.

**Rol** | **Meer informatie** | **Machtigingen**
--- | --- | ---
Azure_Site_Recovery rol | VMware VM discovery |Deze bevoegdheden voor de server in het midden v toewijzen:<br/><br/>Gegevensopslag -> toewijzen ruimte, bladeren gegevensopslag, laag niveau bestand bewerkingen., bestand verwijderen, Update VM bestanden<br/><br/>Netwerk -> netwerk toewijzen<br/><br/>Resource -> VM toewijzen aan resourcegroep, migreren VM uitgeschakeld, migreren VM ingeschakeld<br/><br/>Taken maken taak, updatetaak -><br/><br/>VM-configuratie ><br/><br/>VM-beheren > -> vraag beantwoorden, apparaat verbinding., CD configureren media, configureren diskette media uitschakelen, Power op, installeert u de hulpmiddelen voor VMware<br/><br/>VM -> voorraad maken, Register, registratie -><br/><br/>VM -> Provisioning -> toestaan VM downloaden, toestaan VM bestanden uploaden<br/><br/>VM -> momentopnamen -> momentopnamen verwijderen
vCenter gebruikersrol | VMware VM discovery/Failover zonder afsluiten van bron VM | Deze bevoegdheden voor de server in het midden v toewijzen:<br/><br/>Data Center object –> doorgeven aan onderliggende Object, rol = alleen-lezen <br/><br/>De gebruiker is toegewezen aan datacenter niveau en kan dus toegang heeft tot alle objecten in het datacenter.  Als u de toegang beperken wilt, kunt u de beheerdersrol **geen toegang** met het object **doorgeven aan kind** toewijzen aan de onderliggende objecten (vSphere hosts, datastores, VMs en netwerken te gebruiken).
vCenter gebruikersrol | Failover en foutherstel | Deze bevoegdheden voor de server in het midden v toewijzen:<br/><br/>Datacenter van de object-doorgeven aan onderliggende object, rol = Azure_Site_Recovery<br/><br/>De gebruiker is toegewezen aan datacenter niveau en kan dus toegang heeft tot alle objecten in het datacenter.  Als u beperken van toegang wilt, moet u de rol **geen toegang** met de **doorgeven aan onderliggende object** toewijzen aan het onderliggende object (vSphere hosts, datastores, VMs en netwerken te gebruiken).  
## <a name="next-steps"></a>Volgende stappen

- [Meer informatie](site-recovery-failover.md) over de verschillende soorten failover.
- [Meer informatie over failback](site-recovery-failback-azure-to-vmware.md) hoe u kunt uw mislukte overbrengen in Azure-computers terug naar uw on-premises omgeving.

## <a name="third-party-software-notices-and-information"></a>Kennisgeving over software van derden en informatie

Niet doen vertalen of Localize

De software en firmware die wordt uitgevoerd in de Microsoft-product of service is gebaseerd op of materiaal uit de onderstaande projecten implementeren (gezamenlijk "derden Code').  Microsoft is niet oorspronkelijke auteur van de Code van derden.  De oorspronkelijke copyrightmelding en de licentie, waaronder Microsoft dergelijke Code van derden, ontvangen worden vierde hieronder ingesteld.

De informatie in de sectie A, wordt met betrekking tot onderdelen van derden Code uit de onderstaande projecten. Deze licenties en de gegevens worden alleen ter informatie gegeven.  Deze derden-Code is voor u wordt relicensed door Microsoft onder de licentievoorwaarden voor Microsoft software voor de Microsoft-product of service.  

De informatie in de sectie B betreft derden Code-onderdelen die beschikbaar worden gesteld aan u door Microsoft onder de oorspronkelijke licentievoorwaarden.

Het volledige bestand kan worden gevonden op het [Microsoft Downloadcentrum](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft behoudt alle rechten niet specifiek verleend hierin, of door implicatie wordt ' estoppel ' of anderszins.
