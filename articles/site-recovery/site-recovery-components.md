<properties
    pageTitle="Hoe werkt herstel van de Site? | Microsoft Azure"
    description="In dit artikel bevat een overzicht van de Site herstel architectuur"
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
    ms.topic="get-started-article"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="how-does-azure-site-recovery-work"></a>Hoe werkt herstel van Azure-Site?

Lees dit artikel voor meer informatie over de onderliggende architectuur van de Site herstel van Azure-service en de onderdelen waaruit deze werken. 

Opmerkingen of vragen onder van dit artikel of op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)posten.


## <a name="overview"></a>Overzicht

Organisaties moeten een BCDR strategie die bepaalt hoe apps, werkbelasting en gegevens blijven actief en beschikbaar tijdens geplande en ongeplande downtime en herstellen naar de normale arbeidsvoorwaarden zo snel mogelijk. Uw strategie BCDR moet u bedrijfsgegevens veilige en worden hersteld en ervoor te zorgen dat werkbelasting continu beschikbaar blijven wanneer noodgevallen plaatsvindt. 

Herstel van de site is een Azure-service vergeleken met uw strategie BCDR door orchestrating herhaling van fysieke servers on-premises implementatie en virtuele machines in de cloud (Azure) of met een secundaire datacenter. Wanneer er bijvoorbeeld optreden in uw hoofdlocatie bent, u niet de secundaire locatie wilt behouden apps en werkbelasting beschikbaar. U terug naar uw hoofdlocatie bent niet en er wordt naar de normale bewerkingen. Klik hier als u meer wilt weten in [Wat herstel van de Site is?](site-recovery-overview.md)

## <a name="site-recovery-in-the-azure-portal"></a>Herstel van de site van de Azure-portal

Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken en werken met resources: het model Azure resourcemanager en het beheermodel klassieke services. Azure heeft ook twee portals – de [Azure klassieke portal](https://manage.windowsazure.com/) die ondersteuning biedt voor het implementatiemodel klassieke en de [Azure-portal](https://portal.azure.com) met ondersteuning voor beide implementatiemodellen.

Herstel van de site is beschikbaar in de klassieke portal zowel de Azure-portal. In de portal van Azure klassieke kunt u herstel van de Site met het model voor het beheer van klassieke services ondersteunen. In de portal van Azure kunt u de klassieke model of bron Model implementaties ondersteunen. [Lees meer](site-recovery-overview.md#site-recovery-in-the-azure-portal) over het implementeren in de portal van Azure.

De informatie in dit artikel is van toepassing op Klassiek en Azure portal implementaties. Verschillen worden vermeld, indien van toepassing.

## <a name="deployment-scenarios"></a>Scenario's voor implementatie

Herstel van de site kan worden geïmplementeerd goedkeuringen herhaling in een aantal scenario's:

- **VMware repliceren virtuele machines**: U kunt on-premises implementatie VMware virtuele machines repliceren naar Azure of naar een secundaire datacenter.
- - **Fysieke machines repliceren**: U kunt repliceren fysieke machines naar Azure of met een secundaire datacenter uitgevoerd van Windows of Linux. Het proces voor het repliceren van fysieke machines is nagenoeg hetzelfde als het proces voor het repliceren VMware VMs
- **Repliceren Hyper-V VMs (zonder VMM)**: U kunt Hyper-V VMs die niet worden beheerd door VMM naar Azure repliceren.
- **Repliceren Hyper-V VMs beheerd in System Center VMM wolken**: U kunt on-premises implementatie Hyper-V virtuele machines waarop Hyper-V hostservers in VMM wolken naar Azure of met een secundaire datacenter repliceren. U kunt repliceren standaard Hyper-V Replica of SAN herhaling gebruiken.
- **Migreren VMs**: U kunt Site herstel van [Azure IaaS VMs migreren](site-recovery-migrate-azure-to-azure.md) tussen regio's of [AWS Windows exemplaren](site-recovery-migrate-aws-to-azure.md) migreren naar Azure IaaS VMs. Momenteel alleen migratie, wat betekent dat u kunt niet via deze VMs, maar u kunt geen ze terug niet wordt ondersteund.

Herstel van de site kan de meeste apps waarop deze VMs en fysieke servers repliceren. U kunt een volledige samenvatting van de ondersteunde apps in ophalen [welke werkbelasting kunt Azure Site herstel beveiligen?](site-recovery-workload.md)


## <a name="replicate-to-azure-vmware-virtual-machines-or-physical-windowslinux-servers"></a>Repliceren naar Azure: VMware virtuele machines of fysieke Windows/Linux-servers

Er zijn verschillende manieren repliceren VMware VMs met sites worden hersteld.

- **Met behulp van de Azure portal**-wanneer u Site herstel implementeert in de Azure-portal u via VMs met klassieke service manager storage of aan de Resource-Manager mislukken kan. VMware VMs repliceren in de portal van Azure brengt een aantal voordelen, waaronder de mogelijkheid te worden gerepliceerd naar classic of resourcemanager opslagruimte in Azure wordt aangegeven. [Meer informatie](site-recovery-vmware-to-azure.md).
- **Met behulp van de klassieke portal**-kunt u de Site herstel implementeren in de klassieke-portal met behulp van een verbeterde ervaring. Dit moet worden gebruikt voor alle nieuwe implementaties in de klassieke portal. In deze installatie u kan alleen worden uitgevoerd voor VMs klassieke opslagruimte in Azure en niet mag resourcemanager opslag. [Meer informatie](site-recovery-vmware-to-azure-classic.md). Er is ook een [oudere ervaring](site-recovery-vmware-to-azure-classic-legacy.md) voor het instellen van VMware herhaling in de klassieke portal. Deze niet mag worden gebruikt voor nieuwe implementaties.  Als u hebt al geïmplementeerd met de oudere ervaring [meer informatie over het migreren van](site-recovery-vmware-to-azure-classic-legacy.md#migrate-to-the-enhanced-deployment) de uitgebreide implementatie.



De architectuur vereisten voor het gebruik van de Site herstel repliceren VMware VMs/fysieke servers in de portal van Azure of de Azure klassieke-portal (enhanced) lijken, met een paar verschillen:

- Als u in de portal van Azure implementeert kunt u deze kunt repliceren naar resourcemanager gebruikgemaakt van opslag en resourcemanager netwerken gebruiken om verbinding te maken van de Azure VMs na failover.
- Wanneer u beide LRS implementeert in de portal van Azure en GRS opslag wordt ondersteund. Klik in de klassieke portal GRS is vereist.
- Het implementatieproces is vereenvoudigd en gebruiksvriendelijker in de portal van Azure.


Hier volgt wat u nodig hebt:

- **Azure-account**: U moet een Microsoft Azure-account.
- **Azure opslag**: moet u een account Azure opslag voor de opslag gerepliceerde gegevens. U kunt een klassieke account of een resourcemanager opslag-account. Het account is LRS of GRS wanneer u in de portal van Azure implementeert. Gerepliceerde gegevens worden opgeslagen in de opslagruimte van de Azure en Azure VMs zijn omhoog of bij een storing. 
- **Azure-netwerk**: U moet een Azure virtuele netwerk dat Azure VMs wordt verbonden wanneer ze zijn gemaakt failover wordt uitgevoerd. Ze kunnen netwerken die zijn gemaakt in het model van de manager klassieke service of in het model resourcemanager worden in de portal van Azure.
- **Configuratieserver voor on-premises**: U moet een on-premises implementatie Windows Server 2012 R2 machine die wordt uitgevoerd de configuratieserver en andere onderdelen sites worden hersteld. Als u bezig bent met het repliceren van VMware VMs moet dit een maximaal beschikbare VMware VM. Als u wilt repliceren fysieke servers kan de computer fysiek zijn. Deze Site herstel-onderdelen worden geïnstalleerd op de computer:
    - **Configuratieserver**: communicatie tussen uw on-premises omgeving en Azure-coördinaten en gegevens herhaling en herstelbestanden beheert.
    - **Processerver**: fungeert als een gateway herhaling. Deze herhaling gegevens ontvangt van beveiligde bron machines, optimaliseert met caching, compressie en versleuteling en de gegevens verzonden naar Azure opslag. Ook omgaat met push-installatie van de service mobiliteit voor beveiligde computers en automatische detectie van VMware VMs voert. Als uw implementatie in omvang groeit, kunt u aanvullende afzonderlijk speciale CMYK-servers om af te handelen toeneemt hoeveelheden herhaling verkeer kunt toevoegen.
    - **Basispagina doelserver**: herhaling gegevens tijdens failback van Azure verwerkt. 
- **VMware VMs of fysieke servers repliceren**: elke computer waarop u wilt repliceren naar Azure moet het onderdeel voor mobiliteit-service is geïnstalleerd. Deze service wordt vastgelegd schrijft gegevens op de computer en deze naar de processerver doorstuurt. Dit onderdeel kan handmatig worden geïnstalleerd of kan worden gedrukt en automatisch door de processerver wordt geïnstalleerd wanneer u replicatie voor een machine inschakelen.
- **vSPhere hosts/vCenter server**: U moet een of meer vSphere hostservers met VMware VMs. Het is raadzaam dat u een vCenter-server als u wilt beheren die hosts implementeren.
- **Failback**: Hier ziet u wat u nodig hebt:
    - **Fysieke fysiek failback wordt niet ondersteund**: Dit houdt in dat als u fysieke servers Azure mislukken en vervolgens wilt terug mislukt, u moet niet terug naar een VM VMware. U kunt geen niet terug naar een fysieke server. U moet een VM Azure mislukt terug naar en als u niet de configuratieserver als een VM VMware implementeren moet u voor het instellen van een afzonderlijk diamodel doelserver als een VM VMware. Dit is omdat de basispagina doelserver samenwerkt en koppelt aan VMware opslag de schijven herstellen voor een VM VMware nodig.
    - - **Tijdelijke processerver in Azure wordt aangegeven**: als u mislukt terug van Azure na failover moet u een Azure VM geconfigureerd als de processerver van een wilt, worden afgehandeld herhaling van Azure instellen. Nadat failback is voltooid, kunt u deze VM verwijderen.
    - **VPN-verbinding**: voor failback u moet een VPN-verbinding (of Azure ExpressRoute) instellen van het Azure-netwerk naar de on-premises implementatie-site.
    - **Afzonderlijke on-premises implementatie basispagina doelserver**: de on-premises implementatie basispagina doelserver failback verwerkt. De basispagina doelserver al dan niet standaard op de server is geïnstalleerd, maar als u bent terug grotere hoeveelheden verkeer verbroken u moet instellen een afzonderlijke on-premises implementatie basispagina doelserver daartoe.

**Algemene architectuur**

![Enhanced](./media/site-recovery-components/arch-enhanced.png)

**Onderdelen voor implementatie**

![Enhanced](./media/site-recovery-components/arch-enhanced2.png)

**Failback**

![Verbeterde failback](./media/site-recovery-components/enhanced-failback.png)


- [Meer informatie](site-recovery-vmware-to-azure.md#azure-prerequisites) over vereisten voor Azure portal-implementatie.
- [Meer informatie](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) over implementatievereisten voor uitgebreide in de klassieke portal.
- [Meer informatie](site-recovery-failback-azure-to-vmware.md) over failback in de portal Auzre.
- [Meer informatie](site-recovery-failback-azure-to-vmware-clas- [Learn more](site-recovery-failback-azure-to-vmware-classic.md) about failback in the Auzre portal.sic.md) over failback in de klassieke portal.

## <a name="replicate-to-azure-hyper-v-vms-not-managed-by-vmm"></a>Repliceren naar Azure: Hyper-V VMs niet beheerd door VMM

U kunt repliceren Hyper-V VMs die niet worden beheerd door System Center VMM naar Azure met Site-herstel als volgt:

- **Met behulp van de Azure portal**-wanneer u Site herstel implementeert in de Azure-portal u via VMs met klassieke storage of aan de Resource-Manager mislukken kan. [Meer informatie](site-recovery-hyper-v-site-to-azure.md).
- **Met behulp van de klassieke portal**-kunt u de Site herstel implementeren in de klassieke portal. In deze installatie u kan alleen worden uitgevoerd voor VMs klassieke opslagruimte in Azure en niet mag resourcemanager opslag. [Meer informatie](site-recovery-hyper-v-site-to-azure-classic.md).

De architectuur voor beide implementaties is vergelijkbaar, behalve dat:

- Als u in de portal van Azure implementeert kunt u deze kunt repliceren met resourcemanager storage en resourcemanager netwerken gebruiken om verbinding te maken van de Azure VMs na failover.
- Het implementatieproces is vereenvoudigd en gebruiksvriendelijker in de portal van Azure.

Hier volgt wat u nodig hebt:

- **Azure-account**: U moet een Microsoft Azure-account.
- **Azure opslag**: moet u een account Azure opslag voor de opslag gerepliceerde gegevens. U kunt een klassieke account of een account van de opslagruimte resourcemanager gebruiken in de portal van Azure. U kunt alleen een klassieke account gebruiken in de klassieke portal. Gerepliceerde gegevens worden opgeslagen in de opslagruimte van de Azure en Azure VMs zijn gemaakt bij een storing.
- **Azure-netwerk**: U moet een Azure netwerk dat Azure VMs wordt verbonden wanneer ze na failover zijn gemaakt. 
- **Hyper-v host**: U moet een of meer Windows Server 2012 R2 Hyper-V host-server. Tijdens de implementatie van de Site herstel installeert u de Provider herstel van Azure-Site en de Microsoft Azure herstel Services-agent op de host.
- **Hyper-V VMs**: U moet een of meer VMs op de server van Hyper-V host. Azure Site herstel-Provider en de agent Azure herstel Services op de host Hyper-V tijdens de implementatie van sites worden hersteld. De Provider coördinaten en orchestrates replicatie met de Site herstel-service via internet. De agent omgaat met gegevens herhaling gegevens via HTTPS 443. Communicatie van de Provider zowel de agent zijn beveiligd en versleuteld. Gerepliceerde gegevens in Azure opslag is ook versleuteld.

**Algemene architectuur**

![Hyper-V site Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


- [Meer informatie](site-recovery-hyper-v-site-to-azure.md#azure-prerequisites) over vereisten voor Azure portal-implementatie.
- [Meer informatie](site-recovery-hyper-v-site-to-azure-classic.md#azure-prerequisites) over vereisten voor klassieke portal-implementatie.



## <a name="replicate-to-azure-hyper-v-vms-managed-by-vmm"></a>Repliceren naar Azure: Hyper-V VMs beheerd door VMM

U kunt als volgt Hyper-V VMs in VMM wolken repliceren naar Azure met sites worden hersteld:

- **Met behulp van de Azure portal**-wanneer u Site herstel implementeert in de Azure-portal u via VMs met klassieke storage of aan de Resource-Manager mislukken kan. [Meer informatie](site-recovery-vmm-to-azure.md).
- **Met behulp van de klassieke portal**-kunt u de Site herstel implementeren in de klassieke portal. In deze installatie u kan alleen worden uitgevoerd voor VMs klassieke opslagruimte in Azure en niet mag resourcemanager opslag. [Meer informatie](site-recovery-vmm-to-azure-classic.md).

De architectuur voor beide implementaties is vergelijkbaar, behalve dat:

- Als u in de portal van Azure implementeert kunt u deze kunt repliceren naar resourcemanager gebruikgemaakt van opslag en resourcemanager netwerken gebruiken om verbinding te maken van de Azure VMs na failover.
- Het implementatieproces is vereenvoudigd en gebruiksvriendelijker in de portal van Azure.


Hier volgt wat u nodig hebt:

- **Azure-account**: U moet een Microsoft Azure-account.
- **Azure opslag**: moet u een account Azure opslag voor de opslag gerepliceerde gegevens. U kunt een klassieke account of een account van de opslagruimte resourcemanager gebruiken in de portal van Azure. U kunt alleen een klassieke account gebruiken in de klassieke portal. Gerepliceerde gegevens worden opgeslagen in de opslagruimte van de Azure en Azure VMs zijn gemaakt bij een storing.
- **Azure-netwerk**: moet u voor het instellen van het netwerk toewijzing zodat Azure VMs met de juiste netwerken verbonden zijn wanneer ze na failover hebt gemaakt. 
- **VMM server**: U moet een of meer on-premises implementatie VMM servers waarop systeem Center 2012 R2 en instellen met een of meer privé wolken. Als u in de Azure portal moet u logische en VM netwerken die zijn ingesteld zodat u de toewijzing van netwerk kunt configureren. Klik in de klassieke portal is dit optioneel.  Een netwerk VM moet zijn gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud.
- **Hyper-v host**: U moet een of meer Windows Server 2012 R2 Hyper-V hostservers in de cloud VMM.
- **Hyper-V VMs**: U moet een of meer VMs op de server van Hyper-V host.

**Algemene architectuur**

![VMM naar Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)

- [Meer informatie](site-recovery-vmm-to-azure.md#azure-requirements) over vereisten voor Azure portal-implementatie.
- [Meer informatie](site-recovery-vmm-to-azure-classic.md#before-you-start) over vereisten voor klassieke portal-implementatie.




## <a name="replicate-to-a-secondary-site-vmware-virtual-machines-or-physical-servers"></a>Repliceren naar een secundaire site: VMware virtuele machines of fysieke servers 

VMware VMs of fysieke servers naar een secundaire site gerepliceerd als download InMage Scout die deel van het abonnement Azure sites worden hersteld uitmaakt. Worden kan gedownload van de Azure-portal of van de Azure klassieke portal. 

U de componentservers in elke site (configuratie, proces, basispagina doel) instellen en de Unified-Agent installeren op computers die u wilt repliceren. Na de eerste replicatie verzendt de agent op elke computer delta herhaling wijzigingen op de processerver. De processerver optimaliseert de gegevens en verplaatst deze naar de basispagina doelserver op de secundaire site. De configuratieserver beheert de herhaling.

Hier volgt wat u nodig hebt:

**Azure-account**: U dit scenario met InMage Scout implementeren. U kunt dit moet u een Azure-abonnement. Nadat u een kluis herstel van de Site hebt gemaakt kunt u InMage Scout download en installeer de meest recente updates voor het instellen van de implementatie.
**Proces-server (primaire site)**: het onderdeel van de server proces instellen op uw primaire site worden afgehandeld caching, compressie en optimalisatie van gegevens. Het verwerkt ook push-installatie van de Agent Unified voor computers die u wilt beveiligen. 
**VMware ESX/ESXi en vCenter-server (primaire site)**: als u bent VMware VMs beveiligen moet u een hypervisor VMware EXS/ESXi en eventueel ook een server VMware vCenter voor het beheren van hypervisors.
- **VMs/fysieke servers (primaire site)**: VMware VMs of Windows/Linux fysieke servers u wilt beveiligen is nodig de Unified-Agent is geïnstalleerd. De Unified-Agent is ook geïnstalleerd op de computers die fungeert als de basispagina doelserver. De agent fungeert als een provider van de communicatie tussen alle onderdelen. 
- - **Configuratieserver (secundaire site)**: de configuratieserver is het eerste onderdeel dat u hebt geïnstalleerd en dit geïnstalleerd op de secundaire site kunt beheren, configureren en controleren van de implementatie met behulp van de website management of de vContinuum-console. Er is slechts één configuratieserver in een implementatie en deze moet worden geïnstalleerd op een computer waarop Windows Server 2012 R2 wordt uitgevoerd.
- **vContinuum-server (secundaire site)**: dit geïnstalleerd op dezelfde locatie (secundaire site) als de configuratieserver. Bevat een console voor het beheren en controleren van uw beveiligde-omgeving. In een standaardinstallatie is de vContinuum-server de eerste basispagina doelserver en de Unified-Agent heeft geïnstalleerd.
- **Basispagina doelserver (secundaire site)**: de basispagina doelserver gerepliceerde gegevens bevat. Deze gegevens van de processerver ontvangt, maakt een machine replica in de secundaire site en het bewaarbeleid gegevenspunten bevat. Het aantal basispagina doelservers die u nodig hebt, is afhankelijk van het aantal machines die u bent beveiligen. Als u wilt mislukt terug naar de primaire site moet u ook er een basispagina doelserver. 

**Algemene architectuur**

![VMware VMware](./media/site-recovery-components/vmware-to-vmware.png)


## <a name="replicate-to-a-secondary-site-hyper-v-vms-managed-by-vmm"></a>Repliceren naar een secundaire site: Hyper-V VMs beheerd door VMM


U kunt repliceren Hyper-V VMs die worden beheerd door System Center VMM met een secundaire datacenter met Site-herstel als volgt:

- **Met behulp van de Azure portal**-wanneer u een Site herstel implementeert in de portal van Azure. [Meer informatie](site-recovery-hyper-v-site-to-azure.md).
- **Met behulp van de klassieke portal**-kunt u de Site herstel implementeren in de klassieke portal. [Meer informatie](site-recovery-hyper-v-site-to-azure-classic.md).

De architectuur voor beide implementaties is vergelijkbaar, behalve dat:

- Als u in de portal van Azure implementeert moet u de toewijzing netwerk instellen. Dit is optioneel in de klassieke portal.
- Het implementatieproces is vereenvoudigd en gebruiksvriendelijker in de portal van Azure.
- - Klassieke portal [opslag toewijzing](site-recovery-storage-mapping.md) is beschikbaar als u in de Azure wordt aangegeven implementeert.

Hier volgt wat u nodig hebt:

- **Azure-account**: U moet een Microsoft Azure-account.
- **VMM server**: wordt aangeraden een VMM-server in de primaire-site en in de secundaire site, elk met ten minste één VMM privé cloud. De server moet ten minste worden uitgevoerd systeem 2012 SP1 met de meest recente updates voor centreren en verbinding met internet. Wolken moeten hebben het Hyper-V mogelijkheid profiel instellen. U kunt de Provider van Azure Site herstel installeren op de server VMM. De Provider coördinaten en orchestrates replicatie met de Site herstel-service via internet. Communicatie tussen de Provider en Azure zijn beveiligd en versleuteld.
- **Hyper-V server**: Hyper-V hostservers zich bevinden in de primaire en secundaire VMM wolken. De host servers ten minste moeten worden uitgevoerd Windows Server 2012 met de meest recente updates hebt geïnstalleerd en verbinding met internet. Gegevens worden gerepliceerd tussen de primaire en secundaire Hyper-V hostservers via de LAN- of VPN met Kerberos of certificaat verificatie.  
- **Beveiligde machines**: de host bronserver Hyper-V moet ten minste één VM die u wilt beveiligen.

**Algemene architectuur**

![On-premises implementatie naar on-premises implementatie](./media/site-recovery-components/arch-onprem-onprem.png)


- [Meer informatie](site-recovery-vmm-to-vmm.md#azure-prerequisites) over implementatievereisten voor in de portal van Azure.
- - [Meer informatie](site-recovery-vmm-to-vmm-classic.md#before-you-start) over implementatievereisten voor in de portal van Azure klassieke.




## <a name="replicate-to-a-secondary-site-with-san-replication-hyper-v-vms-managed-by-vmm"></a>Repliceren naar een secundaire site met SAN herhaling: Hyper-V VMs beheerd door VMM

U kunt repliceren Hyper-V VMs beheerd in VMM wolken naar een secundaire SAN replicatie met behulp van de Azure klassieke portal-site. Dit scenario momenteel niet ondersteund in de nieuwe Azure-portal. 

In dit scenario tijdens de implementatie van de Site herstel installeert u de Provider van Azure Site herstel op VMM-servers. De Provider coördinaten en orchestrates replicatie met de Site herstel-service via internet. Gegevens worden gerepliceerd tussen de primaire en secundaire opslag matrices synchroon SAN herhaling gebruiken.

Hier volgt wat u nodig hebt:

**Azure-account**: U moet een Azure-abonnement
- **SAN matrix**: een [ondersteund SAN matrix](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx) beheerd door de primaire VMM-server. Het SAN deelt de infrastructuur van een netwerk met een ander SAN matrix in de secundaire site.
- **VMM server**: wordt aangeraden een VMM-server in de primaire-site en in de secundaire site, elk met ten minste één VMM privé cloud. De server moet ten minste worden uitgevoerd systeem 2012 SP1 met de meest recente updates voor centreren en verbinding met internet. Wolken moeten hebben het Hyper-V mogelijkheid profiel instellen.
- **Hyper-V server**: Hyper-V hostservers bevinden in de primaire en secundaire VMM wolken. De host servers ten minste moeten worden uitgevoerd Windows Server 2012 met de meest recente updates hebt geïnstalleerd en verbinding met internet.
- **Beveiligde machines**: de host bronserver Hyper-V moet ten minste één VM die u wilt beveiligen.

**SAN herhaling architectuur**

![SAN herhaling](./media/site-recovery-components/arch-onprem-onprem-san.png)

[Meer informatie](site-recovery-vmm-san.md#before-you-start) over implementatievereisten voor.
### <a name="on-premises"></a>On-premises implementatie



## <a name="hyper-v-protection-lifecycle"></a>Levenscyclus van de beveiliging van Hyper-V

Deze werkstroom ziet u het proces voor het beveiligen, repliceren en verbroken via Hyper-V virtuele machines. 

1. **Beveiliging inschakelen**: U de Site herstel kluis instellen, replicatie-instellingen voor een VMM cloud of Hyper-V-site configureren en beveiliging voor VMs inschakelen. Een taak genoemd **Beveiliging inschakelen** wordt gestart en in het tabblad **taken** kan worden gecontroleerd. De taak wordt gecontroleerd dat de computer aan de vereisten voldoet en roept vervolgens de methode [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) waarmee ingesteld replicatie naar Azure met de instellingen die u hebt geconfigureerd. De taak **Beveiliging inschakelen** roept ook de methode [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) om een volledige VM replicatie geïnitialiseerd.
2. **Eerste herhaling**: een momentopname VM is die u hebt gemaakt en virtuele vaste schijven gerepliceerde één voor één worden totdat ze bent alle gekopieerd naar Azure of naar het secundaire datacenter. De tijd die nodig is voltooid, is afhankelijk van de grootte VM, netwerkbandbreedte en de eerste replicatie-methode. Als Schijfopruiming wijzigingen optreden tijdens de eerste replicatie wordt uitgevoerd worden bijgehouden in het beheer van Hyper-V Replica herhaling deze wijzigingen Als Hyper-V herhaling Logboeken (.hrl) die in dezelfde map als de schijven bevinden zich. Elke schijf heeft een gekoppeld .hrl-bestand dat wordt verzonden naar secundaire opslag. Houd er rekening mee dat de bestanden momentopname en log schijf resources gebruiken terwijl de eerste replicatie wordt uitgevoerd. Als de eerste replicatie is voltooid de momentopname VM wordt verwijderd en de wijzigingen van de schijf delta in het logboek zijn gesynchroniseerd en samengevoegd.
3. **Voltooien beveiliging**: nadat eerste replicatie is voltooid voor de taak **Voltooien beveiliging** netwerk en andere instellingen die na replicatie wordt zo geconfigureerd dat de virtuele machine is beveiligd. Als u naar Azure repliceren bent moet u mogelijk de instellingen voor de virtuele machine aanpassen zodat de werkmap gereed voor failover is. U kunt nu een failover testen om te controleren dat alles werkt zoals verwacht uitvoeren.
4. **Replicatie**: na de eerste replicatie Deltasynchronisatie is gestart volgens replicatie-instellingen. 
    - **Replicatie is mislukt**: als delta replicatie mislukt, en een volledige replicatie zou erg nadelig voor de bandbreedte of tijd, zal het opnieuw optreedt. Voor voorbeeld als de bestanden .hrl heeft bereikt 50% van de schijfgrootte vervolgens de VM worden gemarkeerd voor het opnieuw. Het opnieuw wordt de hoeveelheid gegevens die zijn verzonden door controlesommen van de bronsite en doelsites virtuele machines en alleen de delta verzenden geminimaliseerd. Nadat het opnieuw is voltooid wordt delta replicatie hervat. Standaard opnieuw synchroniseren automatisch wordt uitgevoerd buiten kantooruren is gepland, maar u kunt een virtuele machine handmatig synchroniseren.
    - **Replicatie-fout**: als het herhaling foutbericht Er is een ingebouwde opnieuw. Als er een herstelbare fout zoals een fout verificatie of machtiging, of een machine replica een ongeldige status is, wordt klikt u vervolgens geen opnieuw geprobeerd. Als er een herstelbare fout zoals een netwerkfout of lage schijf ruimte/geheugen en vervolgens een totaalwaarde met groter wordende intervallen tussen nieuwe pogingen (1, 2, 4, 8, 10, en klik vervolgens op elke 30 minuten).
4. **Gepland/ongeplande failovers**: U kunt of niet gepland failover uitvoeren naar wens. Als u uitvoeren van een geplande failover en bron VMs afsluiten om ervoor te zorgen er geen gegevens verloren gaan. Nadat replica VMs zijn gemaakt bent deze in een doorvoeren in behandeling staat geplaatst. U moet ze om te voltooien van de overname hebt toegewezen, tenzij u bent repliceren met SAN in dat geval doorvoeren automatische. Nadat de primaire site actief is kan failback optreden. Als u hebt gerepliceerd naar Azure omgekeerde is replicatie automatische. Anders starten u omgekeerde herhaling handmatig.
 

![werkstroom](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Volgende stappen

[Voorbereiden voor implementatie](site-recovery-best-practices.md)
