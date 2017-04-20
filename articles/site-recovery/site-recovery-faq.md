<properties
    pageTitle="Azure Site herstel: Veelgestelde vragen | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe populaire vragen over het herstellen van Azure-Site."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="get-started-article"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/10/2016"
    ms.author="raynew"/>


# <a name="azure-site-recovery-frequently-asked-questions-faq"></a>Azure Site herstel: Veelgestelde vragen

In dit artikel bevat veelgestelde vragen over het herstellen van Azure-Site. Als u vragen hebt na het lezen van dit artikel, zet u ze op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>Algemene

### <a name="what-does-site-recovery-do"></a>Wat doet herstel van de Site?

Site-herstel draagt bij aan uw bedrijfscontinuïteit en noodgevallen herstel (BCDR) verkoopstrategie, door orchestrating en automatiseren replicatie vanaf on-premises implementatie virtuele machines en fysieke servers met Azure en met een secundaire datacenter. [Meer informatie](site-recovery-overview.md).


### <a name="what-can-site-recovery-protect"></a>Wat kan het beveiligen van herstel van de Site?

- **Hyper-V virtuele machines**: Site herstel eventuele uitgevoerd op een VM Hyper-V werkbelasting kunt beschermen.
- **Fysieke servers**: Site herstel kunt beveiligen met een fysieke servers met Windows- of Linux.
- **VMware virtuele machines**: Site herstel eventuele uitgevoerd in een VM VMware werkbelasting kunt beschermen.

### <a name="does-site-recovery-support-the-azure-resource-manager-model"></a>Ondersteunt Site herstel het model Azure Resource Manager?

Naast de herstel van de Site in de portal van Azure klassieke is herstel van de Site beschikbaar in de Azure-portal met ondersteuning voor de Resource-Manager. Federatieve van implementatie Site herstel van de Azure portal beschikt u over een gestroomlijnde implementatie-ervaring en u kunt VMs en fysieke servers repliceren in klassieke opslag of resourcemanager opslag. Hier volgen de ondersteunde implementaties:

- [VMware VMs of fysieke servers naar Azure repliceren in de portal van Azure](site-recovery-vmware-to-azure.md)
- [Hyper-V VMs in VMM wolken repliceren naar Azure in de portal van Azure](site-recovery-vmm-to-azure.md)
- [Hyper-V VMs (zonder VMM) repliceren naar Azure in de portal van Azure](site-recovery-hyper-v-site-to-azure.md)
- [Hyper-V VMs in VMM wolken repliceren met een secundaire site in de portal van Azure](site-recovery-vmm-to-vmm.md)


### <a name="what-do-i-need-in-hyper-v-to-orchestrate-replication-with-site-recovery"></a>Wat moet ik in Hyper-V goedkeuringen replicatie met herstel van de Site?

Voor de server van Hyper-V host hangt wat u nodig hebt het scenario. Raadpleeg de vereisten Hyper-V in:

- [Hyper-V VMs (zonder VMM) worden gerepliceerd naar Azure](site-recovery-hyper-v-site-to-azure.md#before-you-start)
- [Hyper-V VMs (met VMM) worden gerepliceerd naar Azure](site-recovery-vmm-to-azure.md#before-you-start)
- [Hyper-V VMs repliceren met een secundaire datacenter](site-recovery-vmm-to-vmm.md#before-you-start)

- Als u met een secundaire datacenter Lees meer over [ondersteunde gastbesturingssystemen voor Hyper-V VMs](https://technet.microsoft.com/library/mt126277.aspx)repliceren bent.
- Als u naar Azure repliceren bent, ondersteunt Site herstel alle Gast-besturingssystemen die [ondersteund door Azure worden](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Kan ik VMs beveiligen wanneer Hyper-V op een client-besturingssysteem wordt uitgevoerd?

Nee, moeten VMs zich bevinden op een server van Hyper-V host die wordt uitgevoerd op een ondersteunde Windows server-computer. Als u wilt beveiligen met een clientcomputer kunt u deze als een fysieke machine [Azure](site-recovery-vmware-to-azure.md) of een [secundaire datacenter](site-recovery-vmware-to-vmware.md)repliceren.


### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Welke werkbelasting kan ik beveiligen met herstel van de Site?

Herstel van de Site kunt u de meeste werkbelasting uitvoeren op een ondersteunde VM of fysieke server beveiligen. Herstel van de site biedt ondersteuning voor replicatie-toepassing-functionaliteit, zodat apps naar een intelligente staat herstellen kunnen. Dit wordt geïntegreerd met het Microsoft-toepassingen zoals SharePoint, Exchange, Dynamics, SQL Server en Active Directory en nauw met toonaangevende leveranciers, inclusief de Oracle, SAP, IBM en rood rol. [Meer informatie](site-recovery-workload.md) over werkbelasting beveiliging.


### <a name="do-hyper-v-hosts-need-to-be-in-vmm-clouds"></a>Hyper-V hosts moet worden in VMM wolken?

Als u repliceren met een secundaire datacenter, wilt en vervolgens Hyper-V VMs moet zich op host Hyper-V servers bevinden in een wolk VMM. Als u repliceren naar Azure wilt, kunt u VMs repliceren op Hyper-V hostservers met of zonder VMM wolken. [Meer informatie](site-recovery-hyper-v-site-to-azure.md).

### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Kan ik herstel van de Site met VMM implementeren als ik slechts één VMM server?

Ja. Kunt u VMs repliceren in Hyper-V servers in de cloud VMM naar Azure of u kunt repliceren tussen VMM wolken op dezelfde server. Voor on-premises on-premises implementatie replicatie wordt u aangeraden dat er een VMM-server in de primaire en secundaire sites.  [Meer weten?](site-recovery-single-vmm.md)

### <a name="what-physical-servers-can-i-protect"></a>Welke fysieke servers kan ik beveiligen?

U kunt de fysieke servers met Windows en Linux naar Azure of naar een secundaire site repliceren. Systeemvereisten voor [algemene informatie over de](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) besturingssystemen.  Dezelfde eisen gelden ongeacht of u fysieke servers Azure of een secundaire site repliceren bent.

Houd er rekening mee dat fysieke servers als VMs in Azure wordt aangegeven wordt uitgevoerd als uw server on-premises implementatie uitvalt. Failback met een on-premises implementatie fysieke server momenteel niet ondersteund, maar u kunt niet terug naar een virtuele machine waarop Hyper-V of VMware.


### <a name="what-vmware-vms-can-i-protect"></a>Welke VMs VMware kan ik beveiligen?

Als u wilt beveiligen VMware VMs moet u een hypervisor vSphere en virtuele machines met VMware's. Ook aangeraden dat er een server VMware vCenter voor het beheren van de hypervisors. [Meer informatie](site-recovery-vmware-to-azure.md#protected-machine-prerequisites) over de exacte vereisten voor het repliceren VMware servers en VMs Azure of een secundaire site.

### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Kan ik herstel voor mijn filialen met Site-herstel beheren?

Ja. Wanneer u Site herstel herhaling en overname bij storing goedkeuringen in uw filialen gebruikt, krijgt u een geïntegreerd integratie en de weergave van alle werkbelasting in uw tak office op een centrale locatie. U kunt eenvoudig failovers uitvoeren en herstel van alle vertakkingen van uw hoofdkantoor, niet de vertakkingen bezoekt beheren.

## <a name="security"></a>Beveiliging

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Replicatiegegevens verzonden naar de Site herstel-service?

Nee, herstel van de Site niet snijpunt gerepliceerde gegevens en geen informatie over wat wordt uitgevoerd op uw virtuele machines of fysieke servers.
Replicatie is uitwisselen tussen on-premises implementatie Hyper-V hosts, VMware hypervisors, of een fysieke servers en Azure opslag of uw secundaire site. Herstel van de site heeft geen mogelijkheid om te snijpunt die gegevens. Alleen de metagegevens nodig goedkeuringen herhaling en overname bij storing wordt verzonden naar de Site herstel-service.

Herstel van de site is ISO 27001:2013, 27018, HIPAA DPA gecertificeerd en wordt SOC2 en FedRAMP JAB beoordelingen.


### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Zelfs de metagegevens van onze on-premises implementatie moet blijven voor naleving redenen, binnen dezelfde geografische regio. Herstel van de Site kan helpen ons?

Ja. Wanneer u een Site herstel kluis in een gebied maakt, we ervoor zorgen dat alle metagegevens die we nodig om te schakelen en goedkeuringen herhaling en failover blijft binnen die regio bevindt zich geografische grens.

### <a name="does-site-recovery-encrypt-replication"></a>Site herstel herhaling versleutelen?

Virtuele machines en fysieke servers wordt repliceren tussen on-premises implementatie sites van versleuteling in overdracht ondersteund. Voor en fysieke servers repliceren naar Azure virtuele machines, worden zowel versleuteling in overdracht en versleuteling in rust (in Azure) ondersteund.


## <a name="replication"></a>Herhaling


### <a name="are-there-any-prerequisites-for-replicating-virtual-machines-to-azure"></a>Zijn er vereisten voor het virtuele machines repliceren naar Azure?

Virtuele machines die u wilt repliceren naar Azure moeten voldoen aan [vereisten voor Azure](site-recovery-best-practices.md#virtual-machines).

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Kan ik Hyper-V generatie 2 virtuele machines repliceren naar Azure?

Ja. Site-herstel converteert van generatie 2 naar generatie 1 tijdens een overname. De computer wordt bij failback geconverteerd terug naar generatie 2. [Meer informatie](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### <a name="if-i-replicate-to-azure-how-do-i-pay-for-azure-vms"></a>Als ik naar Azure repliceren hoe ik betalen voor Azure VMs?

Tijdens de normale replicatie gegevens worden gerepliceerd naar geografische-redundante Azure opslag en hoeft u te betalen eventuele IaaS Azure virtuele machines, een belangrijk voordeel ten leveren. Als u een failover Azure uitvoert, Site herstel IaaS Azure virtuele machines automatisch gemaakt en daarna u worden gefactureerd voor de berekeningscluster bronnen die u in Azure wordt aangegeven verbruikt.


### <a name="is-there-an-sdk-i-can-use-to-automate-the-asr-workflow"></a>Is er een SDK ik gebruiken kunt om automatisch de ASR-werkstroom?

Ja. U kunt Site herstel werkstromen met de Rest API PowerShell of de SDK Azure kunt automatiseren. Momenteel ondersteunde scenario's voor de implementatie van Site-herstel via PowerShell:

- [Hyper-V VMs in VMMs wolken repliceren met Azure PowerShell klassieke](site-recovery-deploy-with-powershell.md)
- [Repliceren Hyper-V VMs in VMMs wolken naar Azure PowerShell resourcemanager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Hyper-V VMs zonder VMM repliceren met Azure PowerShell klassieke](site-recovery-hyper-v-site-to-azure-classic.md)
- [Hyper-V VMs zonder VMM repliceren naar Azure PowerShell resourcemanager](site-recovery-deploy-with-powershell-resource-manager.md)


### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-do-i-need"></a>Als ik naar Azure welk type opslag-account repliceren heb ik nodig?

- **Azure klassieke portal**: als u herstel van de Site in de portal van Azure klassieke distribueren bent, moet u een [standaard geografische-redundante opslag-account](../storage/storage-redundancy.md#geo-redundant-storage). Premium opslagruimte momenteel niet ondersteund. Het account moet zich in dezelfde gebied, als de Site herstel kluis.

- **Azure-portal**: als u Site herstel in de portal van Azure implementeert bent, moet u een LRS of GRS opslag-account. Het is raadzaam GRS zodat gegevens robuuste als er een regionale storing optreedt of als de primaire regio kan niet worden hersteld. Het account moet zich in dezelfde gebied, als de kluis herstel Services. Premium opslag wordt alleen ondersteund als u VMware VMs of fysieke servers repliceren bent.

### <a name="how-often-can-i-replicate-data"></a>Hoe vaak kan ik gegevens repliceren?

- **Hyper-v** Hyper-V VMs kunnen worden gerepliceerd als elke 30 seconden, 5 minuten of 15 minuten. Als u hebt ingesteld met SAN herhaling is replicatie synchroon.
- **VMware en fysieke servers:** De frequentie van een replicatie is hier niet relevant zijn. Replicatie is doorlopend.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Kan ik uitbreiden met herhaling van bestaande herstel site naar een andere tertiaire site?

Uitgebreide of gekoppelde replicatie wordt niet ondersteund. Deze functie in [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication)aanvragen.


### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Kan ik een offline replicatie de eerste keer dat ik naar Azure repliceren?

Hiermee wordt niet ondersteund. Deze functie in het [Feedbackforum](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from)aanvragen.


### <a name="can-i-exclude-specific-disks-from-replication"></a>Kan ik specifieke schijven uitsluiten van replicatie?

Dit wordt ondersteund wanneer u [repliceren VMware VMs en fysieke servers](site-recovery-vmware-to-azure.md#exclude-disks-from-replication) naar Azure bent, met behulp van de Azure portal.


### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Kan ik virtuele machines met dynamische schijven repliceren?

Dynamische schijven worden ondersteund wanneer repliceren Hyper-V virtuele machines. Ze worden ook ondersteund wanneer VMware VMs en fysieke machines repliceren naar Azure. De schijf besturingssysteem moet een eenvoudige schijf.

### <a name="can-i-add-a-new-machine-to-an-existing-replication-group"></a>Kan ik een nieuwe computer toevoegen aan een bestaande replicatiegroep?

Nieuwe machines toevoegen aan bestaande replicatie wordt ondersteund. Selecteer de replicatiegroep (van 'Gerepliceerde items' blade) en klik met de rechtermuisknop/selecteren contextmenu in de replicatiegroep hiertoe en selecteer de gewenste optie.

![Toevoegen aan groep herhaling](./media/site-recovery-faq/add-server-replication-group.png)

### <a name="can-i-throttle-bandwidth-allotted-for-hyper-v-replication-traffic"></a>Kan ik de bandbreedte die is toegewezen voor Hyper-V herhaling verkeer beperken?

Ja. U vindt meer informatie over de bandbreedte in de implementatie-artikelen beperken:

- [Capaciteit, planning voor repliceren VMware VMs en fysieke servers](site-recovery-vmware-to-azure.md#step-5-capacity-planning)
- [Capaciteit, planning voor het repliceren van Hyper-V VMs in VMM wolken](site-recovery-vmm-to-azure.md#step-5-capacity-planning)
- [Capaciteit, planning voor Hyper-V VMs zonder VMM repliceren](site-recovery-hyper-v-site-to-azure.md#step-5-capacity-planning)

## <a name="failover"></a>Failover


### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-virtual-machines-after-failover"></a>Als ik ben niet via Azure, hoe krijg ik toegang tot de Azure virtuele machines na failover?

U kunt de VMs Azure openen via een beveiligde internetverbinding, via een VPN-verbinding voor de site-naar-site of via Azure ExpressRoute. U moet een aantal dingen om verbinding te kunnen voorbereiden. Meer informatie in:

- [Verbinding maken met Azure VMs na overname van VMware VMs of fysieke servers](hsite-recovery-vmware-to-azure.md#step-7-test-the-deployment)
- [Verbinding maken met Azure VMs na overname van Hyper-V VMs in VMM wolken](site-recovery-vmm-to-azure.md#step-7-test-your-deployment)
- [Verbinding maken met Azure VMs na overname van Hyper-V VMs zonder VMM](site-recovery-hyper-v-site-to-azure.md#step-7-test-the-deployment)


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Als ik via Azure hoe Azure ervoor zorgen dat niet is mijn gegevens robuuste?

Azure is bedoeld voor flexibiliteit. Herstel van de site is al ontworpen voor failover met een secundaire Azure datacenter, volgens de SLA Azure zo nodig. Als dit gebeurt, we zorgen ervoor dat de metagegevens van uw en kluizen binnen hetzelfde geografische gebied dat u hebt gekozen voor uw kluis blijven.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Als ik tussen twee datacenters repliceren ben wat gebeurt er als mijn primaire datacenter een onverwachte storing ervaringen?

U kunt een niet-geplande failover uit de secundaire site activeren. Herstel van de site nodig niet connectivity van de primaire site voor het uitvoeren van de overname.

### <a name="is-failover-automatic"></a>Is de overname automatische?

Er is geen automatische failover. U start failovers met één klik in de portal of u kunt [Site herstel PowerShell](https://msdn.microsoft.com/library/dn850420.aspx) gebruiken om te leiden tot een failover. Terug verbroken is een eenvoudige actie in de portal-Site herstellen.

Als u wilt automatiseren u kunnen on-premises implementatie Orchestrator of Operations Manager gebruiken om te bepalen de uitval van een virtuele machines en klikt u vervolgens de overname met de SDK activeren.

- [Lees meer](site-recovery-create-recovery-plans.md) over herstel abonnementen.
- [Lees meer](site-recovery-failover.md) over failover.
- [Lees meer](site-recovery-failback-azure-to-vmware.md) over mislukte back-VMware VMs en fysieke servers


## <a name="service-providers"></a>Serviceproviders


### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Ik ben een serviceprovider. Werkt Site herstel voor speciale en gedeelde infrastructuur modellen?

Ja, ondersteunt Site herstel beide speciale en gedeelde infrastructuur-modellen.

### <a name="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service"></a>Voor een serviceprovider, is de identiteit van mijn tenant gedeeld met de Site herstel-service?

Nee. Tenant identiteit blijft anonieme. Uw tenants hoeft geen toegang tot de portal-Site herstellen. Alleen de beheerder van de serviceprovider communiceert met de portal.


### <a name="will-tenant-application-data-ever-go-to-azure"></a>Wordt tenant toepassingsgegevens ooit terechtkomen in Azure?

Wanneer repliceren tussen service provider eigendom sites, wordt de toepassingsgegevens nooit gaat naar Azure. Gegevens worden gecodeerd in overdracht en gerepliceerde rechtstreeks tussen de serviceprovider-sites.

Als u naar Azure repliceren bent, worden de toepassingsgegevens wordt verzonden Azure opslag maar niet op de Site herstel-service. Gegevens is versleuteld in overdracht en blijft gecodeerd in Azure wordt aangegeven.


### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Ontvang mijn tenants een factuur voor Azure services?

Nee. Facturering relatie van Azure is rechtstreeks bij de provider. Serviceproviders bent verantwoordelijk voor het genereren van specifieke facturen voor hun tenants.

### <a name="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times"></a>Als ik naar Azure repliceren ben, hebben we nodig virtuele machines uitvoeren in Azure allen tijde?

Nee, gegevens worden gerepliceerd naar een account Azure opslag van uw abonnement. Wanneer u een test failover (DR meer details) of een werkelijke failover uitvoert, Site-herstel virtuele machines automatisch gemaakt in uw abonnement.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>Zorgt u er tenantniveau moeten worden geïsoleerd wanneer ik naar Azure repliceren?

Ja.

### <a name="what-platforms-do-you-currently-support"></a>Welke platforms momenteel ondersteund?

Wordt ondersteund Azure Pack, Cloud Platform systeem en System Center (2012 en hoger) implementaties gebaseerd. [Meer informatie](https://technet.microsoft.com/library/dn850370.aspx) over de integratie van Azure Pack en sites worden hersteld.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Ondersteund één Azure Inpakken en één VMM serverimplementaties?

Ja, kunt u Hyper-V virtuele machines repliceren naar Azure of repliceren tussen serviceprovider sites.  Houd er rekening mee dat als u tussen serviceprovider sites repliceren, integratie van Azure runbook is niet beschikbaar.


## <a name="next-steps"></a>Volgende stappen

- Lees de [Site-overzicht](site-recovery-overview.md)
- Meer informatie over [Site herstel architectuur](site-recovery-components.md)  
