<properties
    pageTitle="Active Directory en DNS met Azure Site herstel beveiligen | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe u een noodgevallen herstel-oplossing implementeert voor Active Directory met behulp van Azure sites worden hersteld."
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="08/31/2016"
    ms.author="pratshar"/>

# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Active Directory en DNS met Azure Site herstel beveiligen

Enterprise-toepassingen zoals SharePoint, Dynamics AX en SAP, hangt af van Active Directory en de infrastructuur van een DNS-correct. Wanneer u een oplossing voor het herstellen van noodgevallen voor toepassingen maakt, is het belangrijk dat u weet dat u wilt beveiligen en herstellen van Active Directory en DNS voordat u de andere toepassingsonderdelen, om ervoor te zorgen dat alles correct wanneer noodgevallen plaatsvindt.

Herstel van de site is een Azure-service waarmee problemen oplossen door het orchestrating herhaling, failover en herstel van virtuele machines. Site-herstel ondersteunt een aantal replicatie-scenario's voor consistente bescherming, en naadloos failover virtuele machines en privé, openbare of hoster wolken-toepassingen.

Herstel van de Site gebruikt, kunt u een volledige geautomatiseerde gaan maken voor Active Directory. Wanneer zich onderbrekingen voordoen, kunt u een failover initiëren in enkele seconden vanaf elke locatie en Active Directory aan de slag gaan in een paar minuten. Als u hebt Active Directory voor meerdere toepassingen, zoals SharePoint en SAP geïmplementeerd in uw primaire site en u wilt de volledige site mislukken, kunt u niet over Active Directory eerst met behulp van sites worden hersteld en klikt u vervolgens over de andere toepassingen die gebruikmaken van toepassingsspecifieke herstel abonnementen is mislukt.

In dit artikel wordt uitgelegd hoe u een noodgevallen herstel-oplossing maken voor Active Directory, het uitvoeren van geplande, niet-geplande en test failovers met een plan voor één muisklik, de ondersteunde configuraties en vereisten.  U moet vertrouwd zijn met Active Directory en Azure Site herstel voordat u begint.

Er zijn twee aanbevolen opties op basis van de complexiteit van uw omgeving.

### <a name="option-1"></a>Optie 1

Als u een klein aantal toepassingen en één domeincontroller hebt en u wilt mislukt over de hele site, klikt u vervolgens het beste gebruiken Site herstel de domeincontroller gerepliceerd naar de secundaire site (of u bent verbroken via Azure of een secundaire site). Dezelfde gerepliceerde virtuele machine kan ook worden gebruikt voor test failover.

### <a name="option-2"></a>Optie 2

Als er een groot aantal toepassingen en er meer dan één domeincontroller in de omgeving is, of als u van plan bent mislukt via een paar toepassingen per keer, is het aanbevolen dat naast het domeincontroller virtuele machine met Site-herstel repliceren moet u ook een extra domeincontroller op de doelsite (Azure of een secundaire on-premises implementatie-datacenter) instellen.

>[AZURE.NOTE] Zelfs als u de optie 2 implementeren bent, voor het uitvoeren van een failover test moet u nog steeds repliceren van de domeincontroller met sites worden hersteld. Lees [testen failover overwegingen](#considerations-for-test-failover) voor meer informatie.


De volgende secties wordt uitgelegd hoe u beveiliging voor een domeincontroller bij het herstellen van de Site inschakelen en het instellen van een domeincontroller in Azure wordt aangegeven.


## <a name="prerequisites"></a>Vereisten voor

- Een on-premises implementatie van Active Directory en DNS-server.
- Een kluis Azure Site herstel Services in een Microsoft Azure-abonnement.
- Als u bent repliceren van het hulpprogramma Azure virtuele machines gereedheid Assessment op VMs om ervoor te zorgen en-klaar met Azure zijn compatibel met Azure VMs en Azure Site herstel Services.


## <a name="enable-protection-using-site-recovery"></a>Beveiliging herstel van de Site met inschakelen


### <a name="protect-the-virtual-machine"></a>De virtuele machine beveiligen

Beveiliging van het domeincontroller/DNS virtuele machine bij het herstellen van de Site inschakelen. Herstel van de Site-instellingen op basis van het type VM (Hyper-V of VMware) configureren. Het is raadzaam een vastlopen consistente herhaling frequentie van 15 minuten.

###<a name="configure-virtual-machine-network-settings"></a>VM netwerkinstellingen configureren

Voor het domeincontroller/DNS virtuele machine, netwerkinstellingen bij het herstellen van de Site zodanig configureren dat de VM wordt verbonden met het juiste netwerk na failover. Bijvoorbeeld als u Hyper-V VMs naar Azure repliceren bent kunt u de VM in de cloud VMM of in de groep beveiliging voor het configureren van de netwerkinstellingen zoals hieronder wordt weergegeven

![VM netwerkinstellingen](./media/site-recovery-active-directory/VM-Network-Settings.png)

## <a name="protect-active-directory-with-active-directory-replication"></a>Active Directory beveiligen met Active Directory herhaling

### <a name="site-to-site-protection"></a>Beveiliging van de site-naar-site

Een domeincontroller op de secundaire site maken en geef de naam van het domein dat op de primaire-site wordt gebruikt wanneer u de server om een domeincontroller te verhogen. U kunt de module **Active Directory-Sites en Services** voor het configureren van instellingen op de koppeling siteobject waaraan de sites die worden toegevoegd. Instellingen configureren op een site-koppeling, kunt u bepalen wanneer replicatie plaatsvindt tussen twee of meer sites, en hoe vaak. Zie [Replicatie tussen Sites plannen](https://technet.microsoft.com/library/cc731862.aspx) voor meer informatie.

###<a name="site-to-azure-protection"></a>Site-naar-Azure-beveiliging

Volg de instructies voor het [maken van een domeincontroller in een Azure virtuele netwerk](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Wanneer u de server naar een domeincontroller promoveren geeft u dezelfde domeinnaam die wordt gebruikt op de primaire-site.

Klik [opnieuw configureren de DNS-server voor het virtuele netwerk](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), gebruik van de DNS-server in Azure wordt aangegeven.

![Azure-netwerk](./media/site-recovery-active-directory/azure-network.png)

## <a name="test-failover-considerations"></a>Test failover-overwegingen

Storing in een netwerk die uit productienetwerk heeft geïsoleerd, zodat er geen invloed op productie werkbelasting test.

De meeste toepassingen vereisen ook de aanwezigheid van een domeincontroller en een DNS-server, functie, dus voordat van de toepassing via mislukte, een domeincontroller worden gemaakt in het geïsoleerd netwerk moet moet worden gebruikt voor test failover. De eenvoudigste manier hiervoor is beveiliging op de domeincontroller/DNS virtuele machine met Site-herstel inschakelen en uitvoeren van een test overname van die virtuele machine, voordat u een test overname van het abonnement herstel voor de toepassing. Hier ziet u hoe u dit doen:

1. Beveiliging bij het herstellen van de Site voor het domeincontroller/DNS virtuele machine inschakelen.
2. Een geïsoleerd netwerk maken. Een virtueel netwerk gemaakt in Azure al dan niet standaard wordt geïsoleerd van andere netwerken. Het is raadzaam dat het IP-adresbereiken voor dit netwerk dezelfde als die van uw productienetwerk is. Niet naar website connectivity in dit netwerk inschakelen.
3. Geef een DNS-IP address in het netwerk die is gemaakt, als het IP-adres dat u de DNS-virtuele machine u verwacht. Als u naar Azure repliceren bent, moet u vervolgens het IP-adres voor de VM die worden gebruikt op overname in **Target IP** -instelling in VM eigenschappen opgeven. Als u naar repliceren bent een ander on-premises site en u gebruikt DHCP volgen de instructies voor het [instellen van DNS en DHCP voor test failover](site-recovery-failover.md#prepare-dhcp)

>[AZURE.NOTE] Het IP-adres dat is toegewezen aan een virtuele machine tijdens een de overname van een test komt overeen met het IP-adres dat deze tijdens een een overname of niet gepland krijgt als het IP-adres beschikbaar in het netwerk van de failover-test is. Als dit niet wordt weergegeven, klikt u vervolgens de virtuele machine ontvangt over een ander IP-adres dat is beschikbaar in het netwerk van de failover-test.

4. Klik op het domeincontroller virtuele machine een test overname van deze worden uitgevoerd in het geïsoleerd netwerk. Meest recente beschikbare toepassing consistente herstelpunt van het domeincontroller virtuele machine gebruiken voor de overname test. 
5. Voer een failover test voor de toepassing herstel-abonnement.
6. Nadat de test is voltooid, markeer de testtaak failover van domeincontroller virtuele machine en van het abonnement herstel 'Voltooid' op het tabblad **taken** in de portal-Site herstel.

### <a name="dns-and-domain-controller-on-different-machines"></a>DNS en domeincontroller op verschillende computers

Ook als DNS niet in dezelfde virtuele machine als de domeincontroller moet u een VM DNS voor de overname test maken. Als ze zich op de dezelfde VM bevinden, kunt u dit gedeelte overslaan.

U kunt een nieuwe DNS-server gebruiken en maak de vereiste zones. Als uw Active Directory-domein contoso.com is, kunt u bijvoorbeeld een DNS-zone maken met de naam contoso.com. De items die overeenkomt met Active Directory moeten worden bijgewerkt in DNS, als volgt:

1. Ervoor zorgen dat deze instellingen op hun plaats staan voordat u andere VM in het abonnement herstel verschijnt:

    - De zone moet de naam na de naam van de hoofdsite bos.
    - De zone moet back-bestand.
    - De zone moet zijn ingeschakeld voor secure en niet-beveiligde updates.
    - De resolvercache van het domeincontroller virtuele machine moet verwijzen naar het IP-adres van de DNS-virtuele machine.

2. Voer de volgende opdracht op domeincontroller virtuele machine:

    `nltest /dsregdns`

3. Een zone toevoegen op de DNS-server en een vermelding toevoegen voor deze DNS-updates onbeveiligde toestaan:

        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1


## <a name="next-steps"></a>Volgende stappen

Alleen [welke werkbelasting kan ik beveiligen?](../site-recovery/site-recovery-workload.md) voor meer informatie over het beveiligen van enterprise werkbelasting met Azure sites worden hersteld.
