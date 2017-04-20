<properties
    pageTitle="Voorbereiden op een Site herstel implementatie | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe voorbereiden op het implementeren van replicatie met Azure sites worden hersteld."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="prepare-for-azure-site-recovery-deployment"></a>Voorbereiden voor de implementatie van Azure sites worden hersteld

Lees dit artikel voor een overzicht van hoog niveau van de implementatievereisten voor elk herhaling scenario ondersteund door de service Azure sites worden hersteld. Nadat u de algemene vereisten voor elk scenario leest, kunt u een koppeling maken naar specifieke implementatie details voor elk implementatie.

Na het lezen van dit artikel bericht opmerkingen of vragen aan de onderkant van het artikel of op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Overzicht

Organisaties moeten een BCDR strategie die bepaalt hoe apps, werkbelasting en gegevens blijven actief en beschikbaar tijdens geplande en ongeplande downtime en herstellen naar de normale arbeidsvoorwaarden zo snel mogelijk. Uw strategie BCDR moet u bedrijfsgegevens veilige en worden hersteld en ervoor te zorgen dat werkbelasting continu beschikbaar blijven wanneer noodgevallen plaatsvindt. 

Herstel van de site is een Azure-service vergeleken met uw strategie BCDR door orchestrating herhaling van fysieke servers on-premises implementatie en virtuele machines in de cloud (Azure) of met een secundaire datacenter. Wanneer er bijvoorbeeld optreden in uw hoofdlocatie bent, u niet de secundaire locatie wilt behouden apps en werkbelasting beschikbaar. U terug naar uw hoofdlocatie bent niet en er wordt naar de normale bewerkingen. Klik hier als u meer wilt weten in [Wat herstel van de Site is?](site-recovery-overview.md)

## <a name="select-your-deployment-model"></a>Selecteer uw implementatiemodel

Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken en werken met resources: het model Azure resourcemanager en het beheermodel klassieke services. Azure heeft ook twee portals – de [Azure klassieke portal](https://manage.windowsazure.com/) die ondersteuning biedt voor het implementatiemodel klassieke en de [Azure-portal](https://ms.portal.azure.com/) met ondersteuning voor beide implementatiemodellen.

Herstel van de site is beschikbaar in de klassieke portal zowel de Azure-portal. In de portal van Azure klassieke kunt u herstel van de Site met het model voor het beheer van klassieke services ondersteunen. In de portal van Azure kunt u het klassieke model of resourcemanager implementaties ondersteunen. [Lees meer](site-recovery-overview.md#site-recovery-in-the-azure-portal) over het implementeren in de portal van Azure.

Wanneer u bent kiezen een notitie implementatie-model dat:

- Het is raadzaam om u de [Azure-portal](https://portal.azure.com) en het resourcemanager model indien mogelijk.
- Herstel van de site biedt een eenvoudiger en meer intuïtieve aan de slag ervaring in de portal van Azure.
- U kunt met de portal van Azure, machines repliceren naar klassieke zowel resourcemanager opslagruimte in Azure wordt aangegeven. Bovendien kunt u in de portal van Azure LRS of GRS opslag-accounts gebruiken.
- De portal van Azure worden de services-Site herstel en back-up gecombineerd tot een enkel herstel Services kluis, zodat u kunt instellen en beheren van BCDR services vanaf één locatie.
- Gebruikers met Azure abonnementen deze is ingericht met het programma dat Cloud oplossing (CSP) van kunnen Site herstel bewerkingen in de portal van Azure nu beheren.
- VMware VMs of fysieke machines naar Azure repliceren in de portal van Azure biedt een aantal nieuwe functies, met inbegrip van ondersteuning voor premium opslag en de mogelijkheid tot specifieke schijven uitsluiten van replicatie.


## <a name="select-your-deployment-scenario"></a>Selecteer uw implementatiescenario

**Implementatie** | **Meer informatie** | **Azure-portal** | **Klassieke portal** | **PowerShell**
---|---|---|---|---
**VMware VMs naar Azure** | VMware VMs repliceren met Azure storage | In de Azure portal VMs kan worden uitgevoerd voor naar klassieke of resourcemanager opslag<br/><br/> Implementatie in de [portal van Azure](site-recovery-vmware-to-azure.md) biedt een gestroomlijnde implementatie-ervaring en een aantal functievoordelen. | U kunt in de portal van Azure klassieke implementeren met de [Verbeterde model](site-recovery-vmware-to-azure-classic.md) en naar klassieke Azure opslag mislukken.<br/><br/> Er is ook een oudere model die niet mag worden gebruikt voor nieuwe implementaties. |  PowerShell worden momenteel niet ondersteund.
**Hyper-V VMs naar Azure** | Hyper-V VMs met Azure storage. VMs kunnen zijn op Hyper-V hosts in System Center VMM wolken of zonder VMM beheerd. | Portal VMs kan in de Azure wordt aangegeven via niet worden classic of resourcemanager opslag.<br/><br/> De Azure portal biedt een gestroomlijnde implementatie-ervaring. [Meer informatie](site-recovery-vmm-to-azure.md) over Hyper-V VMs in VMM wolken repliceren. [Meer informatie](site-recovery-hyper-v-site-to-azure.md) over repliceren Hyper-V VMs (zonder VMM).| Klik in de klassieke Azure portal u kan worden uitgevoerd voor VMs naar klassieke Azure opslag | PowerShell-implementatie wordt ondersteund.
**Fysiek servers Azure** | Fysieke Windows/Linux-servers repliceren met Azure storage | Portal servers kunnen in de Azure wordt aangegeven via niet worden classic of resourcemanager opslag.<br/><br/> Implementatie in de [portal van Azure](site-recovery-vmware-to-azure.md) biedt een gestroomlijnde implementatie-ervaring en een aantal functievoordelen. | U kunt in de portal van Azure klassieke implementeren met de [Verbeterde model](site-recovery-vmware-to-azure-classic.md) en naar klassieke Azure opslag mislukken.<br/><br/> Er is ook een oudere model die niet mag worden gebruikt voor nieuwe implementaties. | PowerShell-implementatie worden momenteel niet ondersteund.
**VMware VMs/fysieke servers naar secundaire site* | Repliceren VMware VMs of fysieke Windows/Linux-servers op een secundaire-site. [Meer informatie en download](http://download.microsoft.com/download/E/0/8/E08B3BCE-3631-4CED-8E65-E3E7D252D06D/InMage_Scout_Standard_User_Guide_8.0.1.pdf) de gebruikershandleiding InMage Scout. | Niet beschikbaar in de portal van Azure | Klik in de klassieke portal downloadt u InMage Scout uit een kluis sites worden hersteld. | PowerShell-implementatie wordt niet momenteel ondersteund
**Hyper-V VMs naar een secundaire-site** | Hyper-V VMs in VMM wolken repliceren in een secundaire cloud | U kunt in de [portal van Azure](site-recovery-vmm-to-vmm.md) Hyper-V VMs in VMM wolken repliceren naar een secundaire site Hyper-V Replica alleen gebruiken. SAN worden momenteel niet ondersteund. | U kunt in de portal van Azure klassieke Hyper-V VMs in VMM wolken repliceren naar een secundaire site met [Hyper-V Replica](site-recovery-vmm-to-vmm-classic.md) of [SAN herhaling](site-recovery-vmm-san.md) | PowerShell-implementatie wordt ondersteund



## <a name="check-what-you-need-for-deployment"></a>Wat u nodig hebt voor implementatie controleren

### <a name="replicate-to-azure"></a>Repliceren naar Azure

**Vereiste** | **Meer informatie** 
---|---
**Azure-account** | U moet een [Microsoft Azure](http://azure.microsoft.com/) -account.<br/><br/> U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstelproces is Site prijzen.
**Azure-opslag** | Gerepliceerde gegevens worden opgeslagen in de opslagruimte van de Azure en Azure VMs zijn gemaakt bij een storing. Als u wilt repliceren naar Azure, moet u een [account van Azure opslag](../storage/storage-introduction.md).<br/><br/>Als u in de klassieke portal-Site herstel moet u een of meer [GRS opslag standaardaccount](..storage/storage-redundancy.md#geo-redundant-storage).<br/><br/> Als u in de portal van Azure kunt u GRS of LRS opslag.<br/><br/> Als u VMware VMs of fysieke servers in de opslagruimte van Azure portal premium repliceren bent wordt ondersteund. Houd er rekening mee dat als u een premium-opslag-account gebruikt moet u eveneens een standaard-opslag-account om op te slaan replicatie-logboeken die lopende wijzigingen in de on-premises gegevens vastleggen. [Premium opslag](../storage/storage-premium-storage.md) wordt meestal gebruikt voor virtuele machines die een consistente IO krachtige en lage latentie naar host IO intensief werkbelasting nodig.<br/><br/> Als u een premium-account gebruiken om op te slaan gerepliceerde gegevens wilt, moet u ook een standaard-opslag-account om op te slaan replicatie-logboeken die lopende wijzigingen in de on-premises gegevens vastleggen.
**Azure-netwerk** | Als u wilt repliceren naar Azure, moet u een Azure netwerk dat Azure VMs wordt verbonden wanneer ze na failover zijn gemaakt.<br/><br/> Als u in de klassieke portal gebruikt u een klassieke netwerk. Als u in de portal van Azure distribueren bent, kunt u een classic of resourcemanager netwerk.<br/><br/> Het netwerk moet zich in hetzelfde gebied, als de kluis.
**Netwerk toewijzing (VMM naar Azure)** | Als u bent VMM gerepliceerd naar Azure, [netwerk-toewijzing](site-recovery-network-mapping.md) zorgt ervoor dat Azure VMs zijn verbonden met de juiste netwerken na failover.<br/><br/> Als u wilt netwerk toewijzing instellen moet u VM-netwerken te gebruiken in de portal VMM configureren.
**On-premises implementatie** | **VMware VMs**: U hebt een lokale computer nodig met herstel van de Site-onderdelen, VMware vSphere hosts/vCenter server en VMs die u wilt repliceren. [Meer informatie](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).<br/><br/> **Fysieke servers**: als u bezig bent met het repliceren van fysieke servers moet u een on-premises implementatie-computers waarop een herstel van de Site-onderdelen en fysieke servers die u wilt repliceren. [Meer informatie](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). Als u [terug mislukt](site-recovery-failback-azure-to-vmware.md) na failover naar Azure wilt moet u een VMware infrastructure dat doen.<br/><br/> **Hyper-V VMs**: als u wilt repliceren Hyper-V VMs in VMM wolken moet u een server VMM en Hyper-V hosts op welke VMs die u wilt beveiligen bevinden. [Meer informatie](site-recovery-vmm-to-azure.md#on-premises-prerequisites).<br/><br/> Als u wilt repliceren Hyper-V VMs zonder VMM moet u Hyper-V hosts waarop VMs zich bevinden. [Meer informatie](site-recovery-hyper-v-site-to-azure.md#on-premises-prerequisites).
**Beveiligde machines** | Beveiligde machines die worden gerepliceerd naar Azure moeten voldoen aan [vereisten Azure](#azure-virtual-machine-requirements) hieronder beschreven.


### <a name="replicate-to-a-secondary-site"></a>Repliceren naar een secundaire site

**Vereiste** | **Meer informatie** 
---|---
**Azure-account** | U moet een [Microsoft Azure](http://azure.microsoft.com/) -account.<br/><br/> U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstelproces is Site prijzen.
**On-premises implementatie** | **VMware VMs**: In de primaire site moet u een processerver voor caching, comprimeren en coderen van herhaling gegevens voordat u deze naar de secundaire site verzendt. In de secundaire site installeert u een configuratieserver om te beheren en controleert de implementatie- en een basispagina doelserver. Ook aangeraden een server vContinuum om eenvoudiger te beheren. Daarnaast moet u een of meer VMware vSphere hosts en eventueel ook een vCenter-server. [Meer weten?](site-recovery-vmware-to-vmware.md)<br/><br/> **Hyper-V VMs in VMM wolken**: U moet VMM servers instellen en Hyper-V hosts die VMs bevat die u wilt repliceren. [Meer informatie](site-recovery-vmm-to-vmm.md#on-premises-prerequisites).
**Netwerk toewijzing (VMM naar VMM)** | Als u bent VMM gerepliceerd naar VMM, [netwerk-toewijzing](site-recovery-network-mapping.md) zorgt ervoor dat replica VMs zijn verbonden met de juiste netwerken na failover optimaal op Hyper-V hosts worden geplaatst. Als u wilt netwerk toewijzing instellen moet u VM netwerken configureren op uw VMM-servers.
**Opslag toewijzing** | Als u bent VMM gerepliceerd naar VMM kunt u [opslagruimte toewijzing](site-recovery-storage-mapping.md) (optioneel) instellen om op te geven van het doel van de opslagruimte voor gerepliceerde VMs. Opslag-toewijzing kan worden ingesteld voor zowel Hyper-V Replica als SAN replicatie.<br/><br/> Opslag toewijzing momenteel niet ondersteund in de portal van Azure.


## <a name="verify-url-access"></a>Controleren of de URL-toegang

Zorg ervoor dat deze URL's zijn toegankelijk vanaf servers.

**URL** | **VMM naar VMM** | **VMM naar Azure** | **Hyper-V in op Azure** | **VMware Azure**
---|---|---|---|---
*. accesscontrol.windows.net | Toestaan | Toestaan | Toestaan | Toestaan
*. backup.windowsazure.com | Niet vereist | Toestaan | Toestaan | Toestaan
*. hypervrecoverymanager.windowsazure.com | Toestaan | Toestaan | Toestaan | Toestaan
*. store.core.windows.net | Toestaan | Toestaan | Toestaan | Toestaan
*. blob.core.windows.net | Niet vereist | Toestaan | Toestaan | Toestaan
https://www.msftncsi.com/ncsi.txt | Toestaan | Toestaan | Toestaan | Toestaan
https://dev.mysql.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | Niet vereist | Niet vereist | Niet vereist | Toestaan

## <a name="azure-virtual-machine-requirements"></a>Azure virtuele machines vereisten

U kunt Site herstellen om virtuele machines en de fysieke servers met een besturingssysteem ondersteund door Azure implementeren. Dit geldt ook voor de meeste versies van Windows en Linux. U moet om ervoor te zorgen dat on-premises implementatie virtuele machines die u wilt beveiligen Azure vereisten voldoen.


**Functie** | **Vereisten** | **Meer informatie**
---|---|---
Hyper-V host | Windows Server 2012 R2 moet worden uitgevoerd | Vereisten voor controle mislukt als het besturingssysteem niet worden ondersteund
VMware hypervisor  | Ondersteund besturingssysteem | [Systeemvereisten controleren](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment)
Gast-besturingssysteem | Hyper-V Azure herhaling: Site herstel ondersteuning biedt voor alle besturingssystemen die [ondersteund door Azure worden](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx). <br/><br/> Voor VMware en fysieke server replicatie: de Windows en Linux [vereisten](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) controleren | Vereisten voor controle mislukt als niet worden ondersteund.
Gast besturingssysteem architectuur | 64-bits | Vereisten voor controle mislukt als niet worden ondersteund
Grootte van de schijf besturingssysteem |  Maximaal 1023 GB | Vereisten voor controle mislukt als niet worden ondersteund
Besturingssysteem schijf tellen | 1 | Vereisten voor controle mislukt als niet worden ondersteund.
Gegevens schijf tellen | 16 of minder (maximumwaarde is een functie van de grootte van de virtuele machine wordt gemaakt. 16 = XL) | Vereisten voor controle mislukt als niet worden ondersteund
Gegevens schijf VHD grootte | Maximaal 1023 GB | Vereisten voor controle mislukt als niet worden ondersteund
Netwerkadapters. | Meerdere adapters worden ondersteund |
Statisch IP-adres | Ondersteund | Als de primaire virtuele machine is een statische IP-adres kunt u het statische IP-adres voor de virtuele machine die worden gemaakt in Azure wordt aangegeven. Houd er rekening mee dat statische IP-adres van een linux virtuele machine Hyper-v waarop niet wordt ondersteund.
iSCSI-schijf | Niet ondersteund | Vereisten voor controle mislukt als niet worden ondersteund
Gedeelde VHD | Niet ondersteund | Vereisten voor controle mislukt als niet worden ondersteund
FC Schijfopruiming | Niet ondersteund | Vereisten voor controle mislukt als niet worden ondersteund
Opmaak van de harde schijf| VHD <br/><br/> VHDX | Hoewel VHDX momenteel wordt niet ondersteund in Azure wordt aangegeven, converteert Site-herstel automatisch VHDX naar VHD wanneer u niet Azure. Als u niet terug naar de on-premises implementatie blijven de virtuele machines gebruikt u de notatie VHDX.
BitLocker | Niet ondersteund | BitLocker moet worden uitgeschakeld voordat het beveiligen van een virtuele machine.
De naam van de virtuele machines| Tussen 1 en 63 tekens. Alleen letters, cijfers en afbreekstreepjes. Moet beginnen en eindigen met een letter of een getal | De waarde in de virtuele machine eigenschappen bij het herstellen van de Site bijwerken
VM type | <p>Generatie 1</p> <p>2 - Windows generatie</p> |  VM generatie 2 met OS schijftype eenvoudige schijf waaronder 1 of 2 gegevensvolumes met schijf opmaak als VHDX die kleiner is dan 300GB wordt ondersteund. Linux generatie 2 virtuele machines worden niet ondersteund. [Meer informatie](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)



## <a name="optimizing-your-deployment"></a>Uw implementatie optimaliseren

Gebruik de volgende tips waarmee u kunt optimaliseren en schaal van de implementatie.

- **De grootte van besturingssysteem volume**: wanneer u een virtuele machine naar Azure repliceren het volume van het besturingssysteem minder dan 1 TB moet zijn. Als er meer volumes dan deze kunt u handmatig verplaatsen ze naar een andere schijf voordat u implementatie begint.
- **Gegevens schijf grootte**: als u naar Azure repliceren bent kunt u maximaal 32 gegevensschijven hebben op een virtuele machine, elk voorzien van maximaal 1 TB. U kunt effectief repliceren en via een ~ 32 TB virtuele machine is mislukt.
- **Herstel abonnement limieten**: herstel van de Site kunnen worden aangepast aan duizenden virtuele machines. Herstel abonnementen zijn ontworpen als een model voor toepassingen die moet worden niet via doorgevoerd zodat we Beperk het aantal machines in een herstelplan 50.
- **Azure-service limieten**: elke Azure-abonnement wordt geleverd met een reeks standaardlimieten op cores, cloud services enzovoort. Het is raadzaam een test failover valideren van de beschikbaarheid van resources in uw abonnement uit te voeren. U kunt deze limieten via Azure ondersteuning wijzigen.
- **Capaciteit planning**: Lees meer over het [plannen van capaciteit](site-recovery-capacity-planner.md) voor sites worden hersteld.
- **Replicatie bandbreedte**: als u korte op herhaling bandbreedte rekening met het volgende:
    - **ExpressRoute**: Site herstel werkt met Azure ExpressRoute en WAN-zowel zoals Riverbed. [Lees meer](http://blogs.technet.com/b/virtualization/archive/2014/07/20/expressroute-and-azure-site-recovery.aspx) over ExpressRoute.
    - **Replicatie-verkeer is toegestaan**: gebruik van de Site herstel een slimme aanvankelijke replicatie met alleen gegevens van bouwstenen en niet de hele VHD uitvoert. Alleen wijzigingen worden gerepliceerd tijdens de lopende replicatie.
    - **Netwerkverkeer**: U kunt bepalen netwerkverkeer gebruikt voor replicatie door het instellen van [Windows QoS](https://technet.microsoft.com/library/hh967468.aspx) met een beleid op basis van de bestemming IP-adres en poort.  Bovendien als u bent repliceren naar herstel van Azure-Site met de back-up van Azure-agent kunt u configureren beperken voor die agent. [Meer informatie](https://support.microsoft.com/kb/3056159).
- **RTO**: meten herstel tijd doel (RTO) u met Site-herstel verwachten kunt het is raadzaam u een failover test uitvoeren en de taken herstel van de Site te analyseren hoeveel deze afstemmen weergave gaat naar de bewerkingen uitgevoerd. Als u bent niet via Azure, voor de beste RTO raadzaam dat u alle handmatige acties automatiseren integreren met Azure automatisering en herstelbestanden abonnementen.
- **Vrijgegeven Productieorder**: Site herstel ondersteunt een dicht bij de synchroon herstel punt doelstelling (vrijgegeven Productieorder) wanneer u naar Azure repliceren. Hiermee wordt ervan uitgegaan voldoende bandbreedte tussen uw datacenter en Azure.


##<a name="service-urls"></a>URL 's
Zorg ervoor dat deze URL's zijn toegankelijk vanaf de server


**URL 's** | **VMM naar VMM** | **VMM naar Azure** | **Hyper-V Site Azure** | **VMware Azure**
---|---|---|---|---
 \*. accesscontrol.windows.net | Toegang tot vereist  | Toegang tot vereist  | Toegang tot vereist  | Toegang tot vereist
 \*. backup.windowsazure.com |  | Toegang tot vereist  | Toegang tot vereist  | Toegang tot vereist
 \*. hypervrecoverymanager.windowsazure.com | Toegang tot vereist  | Toegang tot vereist  | Toegang tot vereist  | Toegang tot vereist
 \*. store.core.windows.net | Toegang tot vereist  | Toegang tot vereist  | Toegang tot vereist  | Toegang tot vereist
 \*. blob.core.windows.net |  | Toegang tot vereist  | Toegang tot vereist  | Toegang tot vereist
 https://www.msftncsi.com/ncsi.txt | Toegang tot vereist  | Toegang tot vereist  | Toegang tot vereist  | Toegang tot vereist
 https://dev.mysql.com/Get/Archives/MySQL-5.5/MySQL-5.5.37-Win32.msi | | | | Toegang tot vereist


## <a name="next-steps"></a>Volgende stappen

U kunt na leren en vergelijken algemene implementatievereisten lezen de uitgebreide vereisten en implementeren van elk scenario starten.

- [VMware virtuele machines repliceren naar Azure](site-recovery-vmware-to-azure-classic.md)
- [Fysieke servers repliceren naar Azure](site-recovery-vmware-to-azure-classic.md)
- [Hyper-V server in VMM wolken repliceren naar Azure](site-recovery-vmm-to-azure.md)
- [Hyper-V virtuele machines (zonder VMM) repliceren naar Azure](site-recovery-hyper-v-site-to-azure.md)
- [Hyper-V VMs repliceren naar een secundaire site](site-recovery-vmm-to-vmm.md)
- [Hyper-V VMs repliceren naar een secundaire site met SAN](site-recovery-vmm-san.md)
- [Hyper-V VMs repliceren met één VMM server](site-recovery-single-vmm.md)
