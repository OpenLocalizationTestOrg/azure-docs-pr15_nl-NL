<properties
    pageTitle="Netwerk toewijzing voor de beveiliging van Hyper-V VM met VMM bij het herstellen van Azure Site voorbereiden | Microsoft Azure"
    description="De toewijzing netwerk voor Hyper-V VM replicatie vanuit een on-premises implementatie-datacenter Azure of een secundaire site instellen."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>


# <a name="prepare-network-mapping-for-hyper-v-virtual-machine-protection-with-vmm-in-azure-site-recovery"></a>Netwerk toewijzing voor de beveiliging van Hyper-V VM met VMM bij het herstellen van Azure Site voorbereiden

Azure Site herstel draagt bij aan uw strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel (BCDR) door orchestrating herhaling, failover en herstel van virtuele machines en fysieke servers.

In dit artikel beschrijft netwerk toewijzing, die helpt u optimaal netwerkinstellingen configureren als u Site herstel repliceren Hyper-V virtuele machines gebruikt in VMM wolken tussen twee on-premises implementatie datacenters of tussen een datacenter van de on-premises implementatie en Azure bevindt. Houd er rekening mee dat als u Hyper-V VMs zonder een wolk VMM repliceren bent of repliceren VMware VMs of fysieke servers, in dit artikel niet relevant.

Opmerkingen of vragen onder van dit artikel of op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)posten.


## <a name="overview"></a>Overzicht

Netwerk toewijzing wordt gebruikt wanneer herstel van Azure-Site wordt geïmplementeerd op Hyper-V virtuele machines repliceren naar Azure of naar een secundaire datacenter met Hyper-V Replica of SAN herhaling.

- **Hyper-V repliceren virtuele machines in VMM wolken tussen twee on-premises implementatie datacenters**, netwerk toewijzing kaarten tussen VM-netwerken te gebruiken op een bronserver VMM en VM-netwerken te gebruiken op een doelserver VMM het volgende doen:

    - **Verbinding maken met virtuele machines na failover**, zorgt ervoor dat virtuele machines worden verbonden met de juiste netwerken na failover. De replica virtuele machine wordt niet worden verbonden met het netwerk die is toegewezen aan het Bronnetwerk.
    - **Plaats replica virtuele machines op hostservers**, plaats optimaal replica virtuele machines op Hyper-V host-servers. Replica virtuele machines wordt geplaatst op hosts die toegang heeft tot de toegewezen VM netwerken.
    - **Geen toewijzing netwerk**, als u het netwerk toewijzing niet configureert, gerepliceerde virtuele machines na failover niet wordt aangesloten op een VM-netwerken te gebruiken.

- **Hyper-V repliceren virtuele machines in een on-premises implementatie VMM cloud naar Azure**-toewijzing kaarten tussen VM netwerken op de bronserver VMM netwerk en afstemmen van Azure-netwerken te gebruiken om uit te voeren van de volgende handelingen uit:
    - **Verbinding maken met virtuele machines na failover**, alle computers welke-foutherstel op hetzelfde netwerk met elkaar verbinden kunt, ongeacht welk abonnement herstel ze zijn in.
    - **Netwerkgateway**, als een netwerkgateway is ingesteld op het doel Azure netwerk, virtuele machines verbinding kunt maken met andere on-premises implementatie virtuele machines.
    - **Geen toewijzing netwerk**, als u het netwerk toewijzing niet configureert, alleen virtuele machines die overname in hetzelfde herstelplan kunnen verbinden met elkaar na fail via Azure.


## <a name="network-mapping-example"></a>Voorbeeld van de toewijzing netwerk

Netwerk-toewijzing kan worden geconfigureerd tussen VM netwerken op twee VMM servers of op een enkele VMM-server als twee sites worden beheerd door dezelfde server. Wanneer toewijzing correct is geconfigureerd en replicatie is ingeschakeld, wordt een virtuele machine op de hoofdlocatie bent verbonden met een netwerk en de replica op de doellocatie worden verbonden met het netwerk voor de toegewezen.

Als netwerken hebt wanneer u een netwerk met een doel VM tijdens netwerk toewijzing selecteert in VMM, correct zijn ingesteld, wordt de VMM bron wolken met het netwerk van de VM bron weergegeven, samen met de beschikbare doel VM-netwerken op de doel-wolken die worden gebruikt voor beveiliging.

Hier volgt een voorbeeld om te laten zien deze methode. U gaat nu een organisatie met twee locaties in New York en Chicago.

**Locatie** | **VMM server** | **VM-netwerken te gebruiken** | **Toegewezen aan**
---|---|---|---
New York | VMM-NewYork| VMNetwork1-NewYork | Toegewezen aan VMNetwork1-Chicago
 |  | VMNetwork2-NewYork | Niet toegewezen
Chicago | VMM-Chicago| VMNetwork1-Chicago | Toegewezen aan VMNetwork1-NewYork
 | | VMNetwork2-Chicago | Niet toegewezen

Met dit voorbeeld is:

- Wanneer een replica virtuele machine wordt gemaakt voor een virtuele machine die is gekoppeld aan VMNetwork1-NewYork, wordt deze niet worden verbonden met VMNetwork1-Chicago.
- Wanneer een replica virtuele machine wordt gemaakt voor VMNetwork2-NewYork of VMNetwork2-Chicago, wordt deze niet verbonden met een netwerk.

Hier ziet u hoe VMM wolken zijn ingesteld in ons voorbeeld-organisatie, en de logische netwerken die zijn gekoppeld aan de wolken.

### <a name="cloud-protection-settings"></a>Instellingen voor documentbeveiliging cloud

**Beveiligde cloud** | **Beveiligen van cloud** | **Domainexplorer-BP (New York)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>NB</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>
SilverCloud2 | <p>NB</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>

### <a name="logical-and-vm-network-settings"></a>Logische functie en de VM netwerkinstellingen

**Locatie** | **Logische netwerk** | **Gekoppelde VM-netwerk**
---|---|---
New York | LogicalNetwork1-NewYork | VMNetwork1-NewYork
Chicago | LogicalNetwork1-Chicago | VMNetwork1-Chicago
 | LogicalNetwork2Chicago | VMNetwork2-Chicago

### <a name="target-networks"></a>Doel-netwerken te gebruiken

Op basis van deze instellingen wanneer u het netwerk VM selecteert, ziet in de volgende tabel u de opties die beschikbaar zijn.

**Selecteer** | **Beveiligde cloud** | **Cloud beveiligen** | **TARGET netwerk beschikbaar**
---|---|---|---
VMNetwork1-Chicago | SilverCloud1 | SilverCloud2 | Beschikbaar
 | GoldCloud1 | GoldCloud2 | Beschikbaar
VMNetwork2-Chicago | SilverCloud1 | SilverCloud2 | Niet beschikbaar
 | GoldCloud1 | GoldCloud2 | Beschikbaar



## <a name="multiple-subnets"></a>Meerdere subnetten

Als het doelnetwerk meerdere subnetten heeft en een van deze subnetten heeft dezelfde naam als het subnet waarop de bron virtuele machine zich bevindt, worden klikt u vervolgens de replica virtuele machine verbonden met dat doel subnet na failover. Als er geen subnet doellijst met een overeenkomende naam, wordt de virtuele machine niet worden verbonden met het eerste subnet in het netwerk.


### <a name="failback"></a>Failback

Als u wilt zien wat er gebeurt als failback (omgekeerde replicatie), moet u Stel dat dat VMNetwork1-NewYork is toegewezen aan VMNetwork1-Arnhem, met de volgende instellingen.


**VM** | **Verbonden met VM-netwerk**
---|---
VM1 | VMNetwork1-netwerk
VM2 (replica van VM1) | VMNetwork1-Chicago

Met deze instellingen, laten we bekijken wat gebeurt er in een aantal mogelijke scenario's.

**Scenario** | **Resultaat**
---|---
Geen wijziging in het eigenschappenvenster van het netwerk van VM-2 na failover. | VM-1 blijft verbonden met het Bronnetwerk.
Eigenschappen van het netwerk van VM-2 worden gewijzigd nadat de failover en nadat de verbinding is verbroken. | VM-1 nadat de verbinding is verbroken.
Eigenschappen van het netwerk van VM-2 worden gewijzigd nadat de failover en is verbonden met VMNetwork2-Chicago. | Als VMNetwork2-Chicago niet is toegewezen, kan VM-1 moet worden beëindigd.
Netwerk mapping van VMNetwork1-Chicago wordt gewijzigd. | VM-1 worden verbonden met het netwerk nu toegewezen aan VMNetwork1-Chicago.


## <a name="next-steps"></a>Volgende stappen

Nu dat u een beter begrip van de toewijzing van netwerk hebt, [aan de slag met sites worden hersteld implementatie](site-recovery-best-practices.md).
