<properties
    pageTitle="Hyper-V virtuele machines in VMM wolken repliceren naar een secundaire VMM-site met behulp van de Azure portal | Microsoft Azure"
    description="Beschreven hoe u het implementeren van Azure Site herstellen om de goedkeuringen herhaling, failover en herstel van Hyper-V VMs in VMM wolken naar een secundaire VMM-site met behulp van de Azure portal."
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
    ms.date="08/23/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-the-azure-portal"></a>Hyper-V virtuele machines in VMM wolken repliceren naar een secundaire VMM-site met behulp van de Azure portal

> [AZURE.SELECTOR]
- [Azure-portal](site-recovery-vmm-to-vmm.md)
- [Klassieke portal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell - resourcemanager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Welkom bij Azure Site herstel! In dit artikel als u wilt repliceren on-premises implementatie Hyper-V virtuele machines beheerd in System Center Virtual Machine Manager (VMM) wolken aan een secundaire site wordt gebruikt. In dit artikel wordt beschreven hoe replicatie Azure Site herstel gebruiken in de portal van Azure hebt ingesteld.

> [AZURE.NOTE] Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken en werken met resources: Azure resourcemanager en klassiek. Azure, heeft ook twee portals – de Azure klassieke portal die ondersteuning biedt voor het implementatiemodel klassieke en de Azure-portal met ondersteuning voor beide implementatiemodellen.


Azure Site herstel van de Azure-portal bevat diverse nieuwe functies:

- In de Azure worden-portal, de back-up van Azure en Azure Site herstel services gecombineerd tot een enkel herstel Services kluis, zodat u kunt instellen en beheren van bedrijfscontinuïteit en herstel (BCDR) vanaf één locatie. Een geïntegreerd dashboard kunt u controleren en beheren van bewerkingen binnen uw on-premises implementatie-sites en de Azure openbare cloud.
- Gebruikers met Azure abonnementen deze is ingericht met het programma dat Cloud oplossing (CSP) van kunnen Site herstel bewerkingen in de portal van Azure nu beheren.


Lees dit artikel en posten commentaar onderaan in de opmerkingen Disqus. Technische vragen op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Overzicht

Organisaties moeten een BCDR strategie die bepaalt hoe apps, werkbelasting en gegevens blijven actief en beschikbaar tijdens geplande en ongeplande downtime en herstellen naar de normale arbeidsvoorwaarden zo snel mogelijk. Uw strategie BCDR moet u bedrijfsgegevens veilige en worden hersteld en ervoor te zorgen dat werkbelasting continu beschikbaar blijven wanneer noodgevallen plaatsvindt.

Herstel van de site is een Azure-service vergeleken met uw strategie BCDR door orchestrating herhaling van fysieke servers on-premises implementatie en virtuele machines in de cloud (Azure) of met een secundaire datacenter. Wanneer er bijvoorbeeld optreden in uw hoofdlocatie bent, u niet de secundaire locatie wilt behouden apps en werkbelasting beschikbaar. U terug naar uw hoofdlocatie bent niet en er wordt naar de normale bewerkingen. Klik hier als u meer wilt weten in [Wat Azure Site herstel is?](site-recovery-overview.md)

In dit artikel vindt alle gegevens die u moet repliceren on-premises implementatie Hyper-V VMs in VMM wolken aan een secundaire VMM-site. Het bevat een overzicht architectuur, plannen, informatie en implementatiestappen voor het configureren van on-premises-servers, replicatie-instellingen en capaciteit plannen. Nadat u de infrastructuur hebt ingesteld, kunt u replicatie op computers die u wilt beveiligen en controleer dat die failover werkt inschakelen.

## <a name="business-advantages"></a>Voordelen voor uw bedrijf

- Site-herstel biedt bescherming voor bedrijven werkbelasting en toepassingen op Hyper-V VMs door ze worden gerepliceerd naar een secundaire Hyper-V-server.
- De portal herstel Services biedt één enkele locatie als u wilt instellen, beheren en herhaling, failover en herstel controleren.
- U kunt eenvoudig uitvoeren failovers uit de infrastructuur van uw primaire on-premises implementatie van de secundaire site op primaire aan het secundaire site en failback (herstellen) uitvoeren.
- U kunt herstel abonnementen met meerdere computers zodanig configureren dat doorverbonden toepassing werkbelasting samen mislukken.

## <a name="scenario-architecture"></a>Scenario-architectuur

Dit zijn de onderdelen scenario:

- **Primaire-site**: de primaire-site er zijn een of meer Hyper-V host wordt uitgevoerd op servers VMs bron die u wilt repliceren. Deze primaire host-servers bevinden zich in een VMM privé-cloud.
- **Secundaire site**: de secundaire site er zijn een of meer Hyper-V host wordt uitgevoerd op servers doel VMs waaraan u de primaire VMs repliceren. Deze hostservers bevinden zich in een VMM privé-cloud. De cloud kan zijn op de primaire server (als u slechts één VMM server hebt) of op een secundaire VMM-server.
- **Provider**: tijdens de implementatie van de Site-herstel, u de Provider herstel van Azure-Site op de servers VMM installeren en registreren die servers in een kluis herstel Services. De Provider die wordt uitgevoerd op de server VMM communiceert met Site-herstel via HTTPS 443 repliceren integratie. Replicatie gegevens tussen primaire en secundaire Hyper-V host-servers. Gerepliceerde gegevens blijft binnen de on-premises implementatie-sites en netwerken en wordt niet verzonden naar Azure. Meer informatie over [privacy](#privacy-information-for-site-recovery).

![Topologie van E2E](./media/site-recovery-vmm-to-vmm/architecture.png)

### <a name="data-privacy-overview"></a>Overzicht van privacyverklaring

In deze tabel bevat een overzicht van hoe gegevens worden opgeslagen in dit scenario:
****
Actie | **Meer informatie** | **Verzamelde gegevens** | **Gebruik** | **Vereist**
--- | --- | --- | --- | ---
**Registratie** | U registreren een server VMM in een kluis herstel Services. Als u later wilt registratie van een server, kunt u dit doen door de servergegevens verwijderen uit de Azure-portal. | Nadat een VMM-server is geregistreerd herstel van de Site worden verzameld, processen, en de metagegevens over de server VMM en de namen van de VMM wolken gedetecteerd door sites worden hersteld. | De gegevens wordt gebruikt om te identificeren en communiceren met de juiste VMM-server en instellingen voor de juiste VMM wolken configureren. | Deze functie is vereist. Als u niet wilt dat deze informatie sturen naar de Site herstel kunt u de Site herstel-service niet gebruiken.
**Replicatie inschakelen** | De Provider van Azure Site herstel is geïnstalleerd op de server VMM en is het kanaal voor communicatie met de service sites worden hersteld. De Provider is een DLL-bestand (DLL) die worden gehost in het proces VMM. Nadat de Provider is geïnstalleerd, wordt de functie 'Datacenter herstel' in de beheerconsole VMM ingeschakeld. Nieuwe en bestaande VMs kunnen deze functie om in te schakelen beveiliging voor een VM inschakelen. | Met deze eigenschap is ingesteld verzendt de Provider de naam en ID van VM naar sites worden hersteld.  Replicatie is ingeschakeld door Windows Server 2012- of Windows Server 2012 R2 Hyper-V Replica. De gegevens VM worden gerepliceerd van de ene Hyper-V host naar een andere (meestal zich in een datacenter verschillende 'herstellen'). | De metagegevens van de site herstel worden gebruikt om te vullen de VM informatie in de portal van Azure. | Deze functie is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet dat deze informatie sturen wilt, geen Site hersteld voor VMs inschakelen. Houd er rekening mee dat alle gegevens die zijn verzonden door de Provider naar herstel van de Site is verzonden via HTTPS.
**Plan voor herstel** | Herstel abonnementen hulp bij het samenstellen van een abonnement integratie voor het herstelproces is Datacenter. U kunt de volgorde waarin VMs of een groep van virtuele machines moet worden gestart op de site met herstel definiëren. U kunt ook een geautomatiseerde scripts als u wilt uitvoeren of een handmatige actie moet worden uitgevoerd, klikt u op het moment van herstel voor elke VM opgeven. Failover wordt meestal geactiveerd op het niveau van het abonnement herstel voor gecoördineerde herstel. | Herstel van de site worden verzameld, verwerkt, en metagegevens voor het abonnement herstel, inclusief VM metagegevens en metagegevens van een automatische scripts en de handmatige actie notities verstuurt. | De metagegevens wordt gebruikt voor het maken van het abonnement herstel in de portal van Azure. | Deze functie is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet dat deze informatie sturen naar sites worden hersteld wilt, niet herstel abonnementen maken.
**Netwerk toewijzing** | Kaarten netwerkgegevens vanaf het primaire Datacenter Datacenter voor de herstel. Wanneer VMs worden hersteld op de site herstel, wordt netwerk toewijzing helpt bij het maken van verbinding met het netwerk. | Herstel van de site worden verzameld, verwerkt en verzendt de metagegevens van de logische netwerken voor elke site (primaire en datacenter). | De metagegevens wordt gebruikt om te vullen netwerkinstellingen, zodat u de gegevens van het netwerk kunt afzetten. | Deze functie is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet dat deze informatie sturen naar sites worden hersteld wilt, niet netwerk toewijzing gebruiken.
**Failover (geplande/ongeplande/test)** | Failover wordt overgenomen VMs uit één VMM beheerde Datacenter naar de andere. De actie failover wordt handmatig in de portal van Azure geactiveerd. | De Provider op de server VMM per Site herstel van de gebeurtenis failover is gesteld en een actie failover op de host Hyper-V wordt uitgevoerd via VMM interfaces. Werkelijke overname van een VM is van de ene Hyper-V host naar een andere en verwerkt door Windows Server 2012- of Windows Server 2012 R2 Hyper-V Replica. Wanneer failover voltooid is, worden de Provider op de server VMM in het midden van de gegevens herstel success-gegevens naar sites worden hersteld. | Herstel van de site gebruikt de gegevens die zijn verzonden naar de status van de gegevens van de actie failover in de portal van Azure vullen. | Deze functie is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet dat deze informatie sturen naar sites worden hersteld wilt, niet failover gebruikt.


## <a name="azure-prerequisites"></a>Azure vereisten

Hier is wat u moet in Azure implementeren in dit scenario:

**Vereisten voor** | **Meer informatie**
--- | ---
**Azure**| Hebt u een [Microsoft Azure](http://azure.microsoft.com/) -account nodig. U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstelproces is Site prijzen.


## <a name="on-premises-prerequisites"></a>Vereisten voor on-premises implementatie

Hier is wat u moet in de primaire en secundaire on-premises implementatie-sites implementeren in dit scenario:

**Vereisten voor** | **Meer informatie**
--- | ---
**VMM** | Het is raadzaam om de implementatie van een server VMM in de primaire-site en een VMM in de secundaire site.<br/><br/> U kunt ook [repliceren tussen wolken op één VMM server](site-recovery-single-vmm.md). Hiervoor moet u ten minste twee wolken geconfigureerd op de server VMM.<br/><br/> VMM servers ten minste moeten worden uitgevoerd systeem Center 2012 SP1 met de meest recente updates.<br/><br/> Elke VMM-server moet op een of meer wolken geconfigureerd en alle wolken moet de capaciteit van Hyper-V-profiel instellen. <br/><br/>Wolken moeten een of meer VMM hostgroepen bevatten.<br/><br/>Meer informatie over het instellen van VMM wolken bij het [configureren van de VMM cloud stof](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), en [Stapsgewijze instructies: privé wolken maken met System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM servers moeten internettoegang.
**Hyper-V** | Hyper-V servers ten minste moeten worden uitgevoerd Windows Server 2012 met de functie Hyper-V en de meest recente updates hebt geïnstalleerd.<br/><br/> Een server Hyper-V moet een of meer VMs bevatten.<br/><br/>  Hyper-V hostservers zich bevinden in hostgroepen in de primaire en secundaire VMM wolken.<br/><br/> Als u Hyper-V in een cluster in Windows Server 2012 R2 uitvoert moet u installeren [2961977 bijwerken](https://support.microsoft.com/kb/2961977)<br/><br/> Als u in een cluster Hyper-V in Windows Server 2012 uitvoert, houd er rekening mee dat makelaar cluster wordt niet automatisch gemaakt als u een statische IP-adres gebaseerde cluster hebt. U moet de makelaar cluster handmatig configureren. [Meer informatie](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Provider** | Tijdens de implementatie van de Site-herstel, kunt u de Provider van Azure Site herstel installeren op VMM-servers. De Provider communiceert met Site-herstel via HTTPS 443 goedkeuringen herhaling. Replicatie gegevens tussen de primaire en secundaire Hyper-V servers via het LAN of een VPN-verbinding.<br/><br/> De Provider die wordt uitgevoerd op de server VMM nodig heeft toegang tot deze URL's: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Daarnaast kunt firewall communicatie van de VMM-servers met de [Azure datacenter IP-bereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653) en toestaan de (443) HTTPS-protocol.

## <a name="prepare-for-deployment"></a>Voorbereiden voor implementatie

U moet implementatie voorbereiden:

1. [De server VMM voorbereiden](#prepare-the-vmm-server) voor implementatie sites worden hersteld.
2. [Voorbereiden voor netwerk-toewijzing](#prepare-for-network-mapping). Netwerken zo instellen dat u de toewijzing van netwerk kunt configureren.

### <a name="prepare-the-vmm-server"></a>De server VMM voorbereiden

Zorg ervoor dat de server VMM aan de [vereisten voldoet](#on-premises-prerequisites) en toegang de vermelde URL tot hebben.


### <a name="prepare-for-network-mapping"></a>Voorbereiden voor netwerk-toewijzing

Netwerk toewijzing kaarten tussen VMM VM netwerken op de primaire en secundaire VMM servers:

- Plaats optimaal replica VMs op secundaire Hyper-V hosts na failover.
- Replica VMs verbinden met juiste VM-netwerken te gebruiken.
- Als u geen netwerk toewijzing replica die VMS niet wordt aangesloten op een netwerk na failover configureren.
- Als u wilt netwerk instellen is tijdens het herstellen van de Site toewijzing hier implementatie wat u nodig hebt:

    - Zorg ervoor dat VMs op de bronserver voor de host van Hyper-V zijn verbonden met een netwerk VMM VM. Dit netwerk moet zijn gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud.
    - Controleer of de secundaire cloud dat u voor herstel gebruikt heeft een bijbehorende VM netwerk is geconfigureerd. VM netwerk moet zijn gekoppeld aan een logisch netwerk dat is gekoppeld aan de secundaire cloud.

- [Meer informatie](site-recovery-network-mapping.md) over de werking van netwerk-toewijzing.

## <a name="prepare-for-deployment-with-a-single-vmm-server"></a>Voorbereiden voor implementatie met één VMM server

Als u slechts één VMM server kunt u VMs repliceren in Hyper-V hosts in de cloud VMM naar [Azure](site-recovery-vmm-to-azure.md) of naar een secundaire VMM cloud. Het is raadzaam om dat de eerste optie omdat repliceren tussen wolken niet naadloze, maar als u nodig hebt u dit doet is wat u moet doen:

1. **VMM op een VM Hyper-V instellen**. Als we dit doet wordt u aangeraden dat u de SQL Server-instantie die worden gebruikt door VMM op de dezelfde VM colocate. Dit bespaart u tijd als slechts één VM moet worden gemaakt. Als u wilt gebruiken extern exemplaar van SQL Server en een storing, moet u die instantie herstellen voordat u VMM kunt herstellen.
2. **Zorg ervoor dat de server VMM heeft ten minste twee wolken geconfigureerd**. Eén cloud bevat de VMs u wilt repliceren en de andere cloud fungeert als locatie voor de secundaire. De cloud met de VMs die u wilt beveiligen, moet voldoen aan [vereisten](#on-premises-prerequisites).
3. Herstel van de Site instellen, zoals wordt beschreven in dit artikel. Maken en de VMM-server in de kluis registreren, een beleid herhaling instellen en replicatie inschakelen. U moet opgeven dat die eerste replicatie vindt plaats via het netwerk.
4. Bij het instellen van netwerk-toewijzing kunt u het netwerk VM voor de primaire cloud toewijzen aan het netwerk VM voor de secundaire cloud.
5. In de beheerconsole van Hyper-V Hyper-V Replica inschakelen op de host van Hyper-V waarin de VM VMM en herhaling op de VM inschakelen. Controleer of dat u niet de VMM virtuele machine toevoegen aan wolken die zijn beveiligd met het herstellen van een Site om ervoor te zorgen dat Hyper-V Replica instellingen worden niet door de sites worden hersteld overschreven.
6. Als u herstel-abonnementen voor failover maakt kunt u dezelfde VMM-server gebruiken voor de bronsite en doelsites.
7. U mislukken en als volgt herstellen:

    - Handmatig de VM VMM naar de secundaire site Hyper-V Manager gebruiken met een geplande failover overgenomen.
    - Via de VMs mislukt.
    - Nadat de primaire VMM VM is hersteld, aanmelden bij de portal Azure -> herstel Services kluis en uitvoeren van een niet-geplande overname van de VMs vanaf de secundaire site naar de primaire-site.
    - Wanneer de niet-geplande overname voltooid is, zijn alle resources toegankelijk vanaf de primaire site opnieuw.


### <a name="create-a-recovery-services-vault"></a>Een kluis herstel Services maken

1. Meld u aan bij de [portal van Azure](https://portal.azure.com).
2. Klik op **nieuwe** > **Management** > **herstel Services**. U kunt ook klikken op **Bladeren** > **Herstel Services** kluizen > **toevoegen**.

    ![Nieuwe kluis](./media/site-recovery-vmm-to-vmm/new-vault3.png)

3. Geef in het vak **naam** een beschrijvende naam voor de kluis. Als u meer dan één abonnement hebt, selecteert u een van deze.
4. [Een nieuwe resourcegroep maken](../resource-group-template-deploy-portal.md) of Selecteer een bestaande eigenschap en geef een Azure regio. Machines worden gerepliceerd naar dit gebied. Als u wilt controleren bekijken ondersteunde regio's geografische beschikbaarheid in [Azure Site herstel prijzen Details](https://azure.microsoft.com/pricing/details/site-recovery/)
4. Als u wilt snel toegang tot de kluis vanuit het Dashboard klikt u op **vastmaken aan dashboard** > **maken kluis**.

    ![Nieuwe kluis](./media/site-recovery-vmm-to-vmm/new-vault-settings.png)

De nieuwe kluis wordt weergegeven op het **Dashboard** > **alle resources**, en klik op het hoofdweergavegebied **herstel Services kluizen** blade.




## <a name="getting-started"></a>Aan de slag

Herstel van de site biedt een aan de slag ervaring waarmee u zo snel mogelijk implementeren. Aan de slag controleert de vereisten en begeleidt u bij de Site herstel implementatie in de juiste volgorde stappen.

In aan de slag selecteert u het type van machines die u wilt repliceren en waar u wilt repliceren naar. U on-premises implementatie-servers instellen, replicatie-beleid maken en capaciteitsplanning uitvoert. Nadat u hebt ingesteld met de infrastructuur van uw schakelt u replicatie voor VMs. U kunt uitvoeren failovers voor specifieke machines of maak herstel abonnementen op meerdere computers is mislukt.

Begint aan de slag door te kiezen hoe u wilt implementeren sites worden hersteld. De stroom aan de slag verandert enigszins afhankelijk van uw replicatievereisten voor.

## <a name="step-1-choose-your-protection-goal"></a>Stap 1: Uw doel beveiliging kiezen

Selecteer wat u wilt repliceren en waar u wilt repliceren naar.

1. Klik in het blad **herstel Services kluizen** uw kluis en klik op **Instellingen**.
2. In **Instellingen** > **Aan de slag** Klik op **Site-herstel** > **stap 1: voorbereiden infrastructuur** > **beveiliging doel**.
3. Selecteer **bij herstel site**in **beveiliging doel** en selecteer **Ja, met Hyper-V**.
4. Selecteer **Ja** om aan te geven dat u VMM gebruikt voor het beheren van de Hyper-V-hosts en selecteer **Ja** als u een secundaire VMM-server. Als u replicatie tussen wolken op één VMM server klik u op **Nee**. Klik vervolgens op **OK**.

    ![Kies doelstellingen](./media/site-recovery-vmm-to-vmm/choose-goals.png)


## <a name="step-2-set-up-the-source-environment"></a>Stap 2: De bronomgeving configureren

De Provider van Azure Site herstel op VMM servers installeren en registreren servers in de kluis.

1. Klik op **stap 2: voorbereiden infrastructuur** > **bron**.

    ![Bron instellen](./media/site-recovery-vmm-to-vmm/goals-source.png)

2. Klik op **+ VMM** als u wilt toevoegen van een server VMM in **voorbereiden bron** .

    ![Bron instellen](./media/site-recovery-vmm-to-vmm/set-source1.png)

2. In de **Server toevoegen** blade controleren dat **System Center VMM server** wordt weergegeven in **servertype** en dat de server VMM voldoet aan de [vereisten en URL vereisten](#on-premises-prerequisites).
4. Download het installatiebestand Azure Site herstel-Provider.
5. Download de registratie-toets. U moet dit wanneer u het installatieprogramma uitvoert. De sleutel is ongeldig voor 5 dagen nadat u deze hebt gegenereerd.

    ![Bron instellen](./media/site-recovery-vmm-to-vmm/set-source3.png)

6. Installeer de Provider herstel van Azure-Site op de server VMM.

> [AZURE.NOTE] U hoeft niet te installeren expliciet op Hyper-V host-servers.


### <a name="set-up-the-azure-site-recovery-provider"></a>De Provider herstel van Azure-Site instellen

1. Voer de Provider installatiebestand op elke VMM-server. Als VMM wordt geïmplementeerd in een cluster en u bent de Provider voor de installatie is de eerste keer installeren op een actief knooppunt en klaar bent met de installatie van de server VMM in de kluis registreren. Installeer de Provider op de andere knooppunten. Knooppunten moeten alle dezelfde versie van de Provider uitgevoerd.
2. Setup een paar prerequirement controles uitgevoerd en vraagt machtigingen voor de VMM-service stoppen. De Service VMM worden gestart wanneer setup is voltooid. Als u de installatie uitvoert op een cluster VMM wordt u gevraagd de rol Cluster stoppen.

2.  In de **Microsoft Update** kunt u ervoor kiezen op updates zodat Provider updates worden geïnstalleerd overeenkomstig uw beleid van Microsoft Update.
3. Accepteren of wijzig de standaardlocatie voor de installatie van Provider in **installatie** en klikt u op **installeren**.

    ![Locatie installeren](./media/site-recovery-vmm-to-vmm/provider-location.png)

3. Nadat de installatie is voltooid klikt u op **registreren** om te registreren van de server in de kluis.

    ![Locatie installeren](./media/site-recovery-vmm-to-vmm/provider-register.png)

9. Controleer of de naam van de kluis waarin de server worden geregistreerd in het vak **de naam van de kluis**. Klik op *volgende*.

    ![Serverregistratie](./media/site-recovery-vmm-to-vmm-classic/vaultcred.PNG)

7. Geef op hoe de Provider die wordt uitgevoerd op de server VMM verbinding maakt met Internet in **Internetverbinding** . Selecteer **verbinding maken met bestaande proxy-instellingen** voor het gebruik van de standaard Internet-verbindingsinstellingen voor de server hebt geconfigureerd.

    ![Instellingen voor Internet](./media/site-recovery-vmm-to-vmm-classic/proxydetails.PNG)

    - Als u wilt gebruiken van een aangepaste proxy moet u deze instellen voordat u de Provider installeren. Wanneer u aangepaste proxy-instellingen configureren een toets uitgevoerd als u wilt controleren van de proxyverbinding.
    - Als u gebruik maakt van een aangepaste proxy of uw standaardproxy u opgeven van de gegevens voor de proxy moet, inclusief de proxyadres en poort-verificatie is vereist.
    - Volgende URL's moet toegankelijk zijn vanuit de VMM-Server en de hosts Hyper-v
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Toestaan dat het IP-adressen die worden beschreven in (443) [IP-bereiken gebruikt Azure Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) en HTTPS-protocol. U moet wit-lijst met IP-bereiken van de Azure regio die u wilt gebruiken en die van West VS.
    - Als u een aangepaste proxy die een VMM RunAs-account (DRAProxyAccount) wordt gemaakt automatisch met de opgegeven proxyreferenties gebruiken. De proxyserver zodanig configureren dat dit account succes kan verifiëren. De accountinstellingen VMM RunAs kunnen worden gewijzigd in de VMM-console. Ga als volgt te openen van de werkruimte **Instellingen** , **beveiliging**uitvouwen, en klikt u op **Uitvoeren als Accounts**, wijzigt u het wachtwoord voor DRAProxyAccount. U moet de VMM-service opnieuw starten zodat deze instelling wordt pas van kracht.


8. Selecteer de sleutel die u hebt gedownload van Azure Site herstel en gekopieerd naar de server VMM in **Registratiegegevens sleutel**.


10.  De instelling versleuteling wordt alleen gebruikt wanneer u Hyper-V VMs in VMM wolken naar Azure repliceren bent. Als u naar een secundaire site repliceren bent wordt deze niet gebruikt.

11.  Geef in het vak **servernaam**een beschrijvende naam voor de server VMM in de kluis. Geef de naam van het VMM cluster rol in de configuratie van een cluster.
12.  Klik in **metagegevens van synchroniseren-cloud** Selecteer of u wilt synchroniseren van metagegevens voor alle wolken op de server VMM met de kluis. Deze actie moet slechts één keer aan elke server worden gewerkt. Als u niet dat alle wolken synchroniseren wilt, kunt u deze instelling uitgeschakeld laat en elke cloud afzonderlijk in de cloud-eigenschappen in de console VMM synchroniseren.

13.  Klik op **volgende** om het proces te voltooien. Metagegevens van de server VMM wordt na registratie opgehaald door Azure sites worden hersteld. De server wordt weergegeven op het tabblad **VMM-Servers** op de pagina **Servers** in de kluis.

    ![Server](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

11. Nadat de server onderdeel beschikbaar in de Site herstel-console in **bron is** > **voorbereiden bron** selecteert u de VMM-server en de cloud waarin de Hyper-V-host bevindt. Klik vervolgens op **OK**.

#### <a name="command-line-installation"></a>Opdrachtregel installatie

De Provider herstel van Azure Site kan worden geïnstalleerd vanaf de opdrachtregel. Deze methode kan worden gebruikt voor het installeren van de Provider op de Server Core voor Windows Server 2012 R2.

1. Download de sleutel Provider installatie en registratie naar een map. Bijvoorbeeld C:\ASR.
2. De System Center Virtual Machine Manager-Service stoppen.
3. Vanuit een opdrachtprompt met verhoogde bevoegdheid deze opdrachten kunt ophalen van het installatieprogramma van Provider uitvoeren:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Deze opdracht voor het installeren van de Provider uitvoeren:

        C:\ASR> setupdr.exe /i

5. Voer deze opdrachten de server registreren in de kluis:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Waar de parameters zijn:

 - **/Credentials**: verplichte parameter die de locatie waarin het belangrijkste registratie-bestand zich bevindt  
 - **/FriendlyName**: parameter is vereist voor de naam van de Hyper-V host-server die wordt weergegeven in de portal Azure sites worden hersteld.
 - **/EncryptionEnabled**: optionele Parameter die u alleen gebruiken wanneer VMM gerepliceerd naar Azure.
 - **/proxyAddress**: optionele parameter die het adres van de proxyserver.
 - **/proxyport**: optionele parameter die de poort van de proxyserver.
 - **/proxyUsername**: optionele parameter die de Proxy-gebruikersnaam (als proxy is verificatie vereist).
 - **/proxyPassword**: optionele parameter die het wachtwoord voor het verifiëren met de proxyserver (als proxy is verificatie vereist).  

## <a name="step-3-set-up-the-target-environment"></a>Stap 3: De doelomgeving configureren

Selecteer de doelserver VMM en de cloud.

1. Klik op **de infrastructuur voorbereiden** > **doeltoepassing** en selecteer de doelserver VMM u wilt gebruiken.
2.  Wolken op de server die zijn gesynchroniseerd met sites worden hersteld worden, weergegeven. Selecteer de doel-cloud.

    ![Doel](./media/site-recovery-vmm-to-vmm/target-vmm.png)

## <a name="step-4-set-up-replication-settings"></a>Stap 4: Herhaling instellingen instellen

1. U maakt een nieuwe herhaling beleid klikt u op **voorbereiden infrastructuur** > **Replicatie-instellingen** > **+ maken en koppelen**.

    ![Netwerk](./media/site-recovery-vmm-to-vmm/gs-replication.png)

2. Geef de naam van een beleid in **maken en koppelen beleid** . Het type bronsite en doelsites moet **Hyper-V**.
3. Selecteer welk besturingssysteem wordt uitgevoerd op de host in **Hyper-V host versie** .

    > [AZURE.NOTE] De cloud VMM Hyper-V hosts met verschillende (ondersteunde) versies van Windows Server kan bevatten, maar een replicatiebeleid wordt toegepast hosts waarop hetzelfde besturingssysteem wordt uitgevoerd. Als er hosts met meer dan één besturingssysteemversie vervolgens afzonderlijk herhaling beleidsregels te maken.

4. Opgeven hoe verkeer tussen de servers primair en herstelbestanden Hyper-V host wordt geverifieerd bij **verificatietype** en **verificatiepoort** . Selecteer **certificaat** tenzij u een actieve Kerberos-omgeving hebt. Azure Site herstel configureert automatisch certificaten voor HTTPS-verificatie. U hoeft te ondernemen handmatig. Standaard wordt poort 8083 en 8084 (voor certificaten) geopend in het Windows Firewall op de Hyper-V host-servers. Als u **Kerberos**selecteert, wordt een Kerberos-tickets worden gebruikt voor de onderlinge verificatie van de hostservers. Houd er rekening mee dat deze instelling is alleen van toepassing voor Hyper-V hostservers waarop Windows Server 2012 R2.
3. Geef in het **kopiëren van frequentie** op hoe vaak u wilt repliceren delta gegevens na de eerste replicatie (elke 30 seconden, 5 of 15 minuten).
4. Geef in **herstel wijst u bewaren**, op binnen enkele uren hoe lang het bewaarbeleid-venster wordt voor elk opsommingsteken herstel. Beveiligde machines kunnen herstellen naar een willekeurige plaats in een venster.
6. In de **App-consistente momentopname frequentie** Geef op hoe vaak (1-12 uur) herstel punten die van toepassing-consistente momentopnamen gemaakt. Hyper-V gebruikt twee typen momentopnamen, een momentopname van een standaard waarmee een incrementele momentopname van de hele virtuele machine en een momentopname van een toepassing consistente dat een momentopname van het punt-in-tijd van de toepassingsgegevens in de virtuele machine. Volume schaduw Copy Service (VSS) toepassing consistente momentopnamen gebruiken om ervoor te zorgen dat de toepassingen in een consistente status zijn wanneer de momentopname wordt gemaakt. Houd er rekening mee dat als u de toepassing consistente momentopnamen inschakelt, dit invloed is op de prestaties van toepassingen op bron virtuele machines. Zorg ervoor dat de ingestelde waarde kleiner is dan het aantal aanvullende herstel punten die u configureren.
7. Geef in het **gegevens overbrengen compressie** of gerepliceerde gegevens die worden overgebracht moeten worden gecomprimeerd.
8. Selecteer **replica VM verwijderen** om op te geven dat de replica virtuele machine moet worden verwijderd als u de beveiliging voor de bron VM uitschakelt. Als u deze instelling, inschakelt wanneer u beveiliging voor de bron deze verwijderd van de Site herstel-console VM uitschakelt, herstel van de Site-instellingen voor de VMM worden verwijderd uit de VMM-console en de replica wordt verwijderd.
3. In **aanvankelijke herhaling methode** als u via het netwerk bent repliceren opgeven of starten van de eerste replicatie of plannen. Om op te slaan netwerkbandbreedte wilt u mogelijk deze buiten uw bezet uren plannen. Klik vervolgens op **OK**.

    ![Replicatiebeleid](./media/site-recovery-vmm-to-vmm/gs-replication2.png)

6. Wanneer u een nieuw beleid maakt, is deze automatisch gekoppeld aan de cloud VMM. Klik op **OK**in **herhaling beleid** . U kunt extra VMM wolken (en de VMs erin) koppelen aan dit beleid herhaling in **Instellingen** > **herhaling** > beleidsnaam > **VMM Cloud koppelen**.

    ![Replicatiebeleid](./media/site-recovery-vmm-to-vmm/policy-associate.png)

### <a name="prepare-for-offline-initial-replication"></a>Voorbereiden voor offline eerste replicatie

U kunt offline replicatie voor de kopie van de oorspronkelijke gegevens kunt doen. U kunt dit als volgt voorbereiden:

- Op de bronserver, moet u een padlocatie van waaruit het exporteren van gegevens wordt gehouden. Volledig beheer toewijzen voor machtigingen voor NTFS en delen met de service VMM op het pad exporteren. Op de doelserver, moet u de locatie van een pad van waaruit het importeren van gegevens wordt uitgevoerd. Dezelfde machtigingen op dit pad importeren toewijzen.
- Als het pad importeren of exporteren wordt gedeeld, wijst u beheerder, Power gebruiker, Operator afdrukken of Server Operator groepslidmaatschap voor het account van de service VMM op de externe computer waarop de gedeelde zich bevindt.
- Als u uw accounts uitvoeren als gebruikt om toe te voegen hosts, klik op de paden importeren / exporteren, gelezen toewijzen en schrijfmachtigingen voor de accounts uitvoeren als in VMM.
- De aandelen importeren / exporteren moeten niet zich bevinden op elke computer gebruikt als Hyper-V hostserver, omdat loopback-configuratie niet wordt ondersteund door Hyper-V.
- In Active Directory beperkte hostserver waarop virtuele machines die u wilt beveiligen, inschakelen en configureren op elke Hyper-V delegering te vertrouwen van de externe computers waarop de paden importeren / exporteren, als volgt bevinden:
    1. Open op de domeincontroller, **Active Directory-gebruikers en Computers**.
    2. Klik in de consolestructuur op **domeinnaam** > **Computers**.
    3. Met de rechtermuisknop op de naam van de Hyper-V host server > **Eigenschappen**.
    4. Klik op **deze computer mag overdragen aan de opgegeven services alleen**op het tabblad **overdracht** .
    5. Klik op **elk protocol voor verificatie gebruiken**.
    6. Klik op **toevoegen** > **gebruikers en Computers**.
    7. Typ de naam van de computer waarop het pad exporteren > **OK**. In de lijst met beschikbare services, houd de CTRL-toets ingedrukt en klik op **cifs** > **OK**. Herhaal voor de naam van de computer waarop het pad importeren. Herhaal zo nodig voor extra Hyper-V hostservers.


### <a name="configure-network-mapping"></a>Netwerk toewijzing configureren

Netwerk toewijzing tussen bronsite en doelsites netwerken instellen.

- [Alleen](#prepare-for-network-mapping) voor een kort overzicht van welke netwerk-toewijzing bevat. In de toevoeging [lezen](site-recovery-network-mapping.md) voor een uitgebreidere uitleg.
- Controleer of dat virtuele machines op VMM servers zijn verbonden met een netwerk VM.

Configureer toewijzing als volgt:

1. **Instellingen** > **Site herstel infrastructuur** > **Netwerk toewijzing** > **netwerk toewijzingen** klikt u op **+ netwerk toewijzen**.

    ![Netwerk toewijzing](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. Selecteer in het tabblad van de **toewijzing van netwerk toevoegen** de bron en afstemmen VMM-servers. De VM netwerken die zijn gekoppeld aan de servers VMM worden opgehaald.
3. Selecteer in de **bron-netwerk**, het netwerk dat u wilt gebruiken in de lijst met VM netwerken die zijn gekoppeld aan de primaire VMM-server.
6. Selecteer het netwerk dat u wilt gebruiken op de secundaire VMM-server in de **doel-netwerk** . Klik vervolgens op **OK**.

    ![Netwerk toewijzing](./media/site-recovery-vmm-to-vmm/network-mapping2.png)

Hier ziet u wat gebeurt er wanneer netwerk toewijzing begint:

- Een bestaande replica virtuele machines die met het netwerk van de VM bron overeenkomen worden verbonden met het netwerk VM.
- Nieuwe virtuele machines die met het netwerk van de VM bron verbonden zijn worden verbonden met het doelnetwerk toegewezen na herhaling.
- Als u een bestaande toewijzing met een nieuw netwerk wijzigt, wordt replica virtuele machines worden verbonden met de nieuwe instellingen.
- Als het doelnetwerk meerdere subnetten heeft en een van deze subnetten heeft dezelfde naam als subnet waarop de bron virtuele machine zich bevindt, worden klikt u vervolgens de replica virtuele machine verbonden met dat doel subnet na failover. Als er geen subnet doellijst met een overeenkomende naam, wordt de virtuele machine niet worden verbonden met het eerste subnet in het netwerk.

### <a name="configure-storage-mapping"></a>Opslag toewijzing configureren
Wanneer u een virtuele machine op een bronserver van Hyper-V host naar een Hyper-V host doelserver repliceren, wordt gerepliceerde gegevens al dan niet standaard opgeslagen in de standaardlocatie die wordt aangegeven voor de doeltoepassing Hyper-V host in Hyper-V-beheer. Voor meer controle wilt over waar gerepliceerde gegevens worden opgeslagen, kunt u opslagruimte toewijzing configureren<br/><br/> Als u wilt configureren opslag toewijzing, moet u instellen opslag classificaties op de bron en afstemmen VMM servers voordat u implementatie begint. Opslag toewijzing tot en met de nieuwe portal van Azure is momenteel niet ondersteund. Dit kan echter worden ingeschakeld via Powershell. [Meer informatie](site-recovery-vmm-to-vmm-powershell-resource-manager.md#step-6-configure-storage-mapping).

## <a name="step-5-capacity-planning"></a>Stap 5: Capaciteit plannen

Nu dat u uw basic hebt infrastructuur instellen u kunt denken over het plannen van capaciteit en bepalen of u aanvullende informatie nodig hebt.

Herstel van de site biedt een planner op basis van Excel capaciteit waarmee u de juiste resources toewijzen voor uw bronomgeving, de site herstel onderdelen, netwerken en opslag. U kunt de planner uitvoeren in de snelle modus voor schattingen op basis van een gemiddelde aantal VMs, schijven en opslag of in de gedetailleerde weergave waarin u afbeeldingen op het niveau van de werkbelasting kunt invoert. Voordat u begint moet u:

- Verzamel informatie over uw replicatieomgeving, inclusief VMs, schijven per VMs en opslagruimte per schijf.
- Schatting van de dagelijkse wijzigen (lospeuteren)-mate dat u voor gerepliceerde delta gegevens hebt. U kunt de [capaciteit planner voor Hyper-V Replica](https://www.microsoft.com/download/details.aspx?id=39057) gebruiken om te helpen u dit kunt doen.

1.  Klik op **downloaden** om te downloaden van het hulpmiddel en voer dit uit. [Lees het artikel](site-recovery-capacity-planner.md) waarop het hulpmiddel.
2.  Wanneer u klaar bent Selecteer **Ja** in het **hebt u de capaciteit Planner uitvoeren**?

    ![Capaciteit plannen](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="control-bandwidth"></a>Besturingselement bandbreedte

Als u realtime delta herhaling informatie toe aan de capaciteit teamplanner Hyper-V Replica hebt verzameld, wordt de capaciteit op basis van een Excel-planner hulpmiddel helpt u bij het berekenen van de bandbreedte die u nodig voor replicatie (eerste en delta hebt). U kunt het NetQos beleid via Groepsbeleid of Windows PowerShell configureren om te bepalen hoeveel bandbreedte gebruikt voor replicatie. Er zijn een paar manieren kunt u dit doen:

**PowerShell** | **Meer informatie**
--- | ---
**Nieuw - NetQosPolicy "QoS bestemming subnet"** | Verkeer uit een Hyper-V-host met Windows Server 2012 R2-en een secundaire subnet beperken. Wordt gebruikt als de primaire en secundaire subnetten verschillen.
**Nieuw - NetQosPolicy "QoS naar doelpoort"** | Verkeer uit een Hyper-V-host met Windows Server 2012 R2 naar een doelpoort beperken.
**Nieuwe - NetQosPolicy "Vertraging verkeer van VMM"** | Verkeer van vmms.exe beperken. Hiermee wordt verkeer Hyper-V en Live-migratie verkeer beperken. U kunt overeenkomen met IP-protocollen en poorten om te verfijnen.

U kunt de bandbreedte gewicht instellingen gebruiken of verkeer door bits per secundair beperken. Als u een cluster gebruikt die u moet dit doen op alle clusterknooppunt. Zie voor meer informatie:


- Blog van Thomas Maurer op [Hyper-V Replica-verkeer beperken](http://www.thomasmaurer.ch/2013/12/throttling-hyper-v-replica-traffic/)
- Aanvullende informatie over de [cmdlet New-NetQosPolicy](https://technet.microsoft.com/library/hh967468.aspx).


## <a name="step-6-enable-replication"></a>Stap 6: Inschakelen herhaling

Nu als volgt herhaling inschakelen:

1. Klik op **stap 2: toepassing repliceren** > **bron**. Wanneer u replicatie voor de eerste keer hebt ingeschakeld, klik u op **+ repliceren** in de kluis replicatie voor extra machines inschakelen.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-vmm/enable-replication1.png)

2. Klik in het blad **bron** > Selecteer VMM server en de cloud waarin de Hyper-V-hosts die u wilt repliceren bevinden. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-vmm/enable-replication2.png)

3. Controleer of de secundaire VMM-server en de cloud in het blad **doel** .
4. Selecteer in de **virtuele machines** de VMs die u wilt beveiligen in de lijst.

    ![VM beveiliging inschakelen](./media/site-recovery-vmm-to-vmm/enable-replication5.png)

U kunt de voortgang van de actie **Beveiliging inschakelen** in instellingen bijhouden > **taken** > **herstel van de Site taken**. Nadat de taak **Voltooien beveiliging** wordt uitgevoerd is de virtuele machine gereed voor failover.


>[AZURE.NOTE] U kunt ook beveiliging voor virtuele machines in de console VMM inschakelen. Klik op **Beveiliging inschakelen** op de werkbalk in de virtuele machine-eigenschappen > tabblad **Herstel van Azure-Site** .

Wanneer u replicatie hebt ingeschakeld, kunt u eigenschappen weergeven voor de VM in **Instellingen** > **Items gerepliceerd** > vm-naam. Klik op het dashboard **Essentials** ziet u informatie over het beleid herhaling voor de VM en de status ervan. Klik op **Eigenschappen** voor meer informatie.

### <a name="onboard-existing-virtual-machines"></a>Ingebouwde bestaande virtuele machines

Als u bestaande virtuele machines in VMM die zijn repliceren met Hyper-V Replica kunt u deze boord ze voor de beveiliging van Azure Site herstel als volgt:

1. Zorg ervoor dat de Hyper-V-server de bestaande VM hostingprovider bevindt zich in de primaire cloud en dat de Hyper-V-server de replica virtuele machine hostingprovider bevindt zich in de secundaire cloud.
2. Zorg ervoor dat een replicatiebeleid is geconfigureerd voor de primaire VMM cloud.
2. Replicatie voor de primaire virtuele machine inschakelen. Azure herstel van de Site en VMM zorgt ervoor dat dat de dezelfde replica host en VM wordt aangetroffen, en Azure Site herstel dan opnieuw gebruiken en breng met de opgegeven instellingen.


## <a name="step-7-test-your-deployment"></a>Stap 7: Test uw implementatie

Als u wilt testen, uw implementatie kunt u een failover test voor een enkele virtuele machine uitvoeren of maak een herstel-abonnement met een of meer virtuele machines.

### <a name="prepare-for-failover"></a>Voorbereiden voor failover

- Als u wilt uw implementatie volledig testen moet u een infrastructuur voor de gerepliceerde machine werkt zoals verwacht. Als u testen wilt, Active Directory en DNS kunt u een virtuele machine maken als een domeincontroller met DNS- en repliceren dit naar Azure met Azure sites worden hersteld. Lees meer in [test failover aandachtspunten voor Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- De instructies in dit artikel wordt uitgelegd hoe u het uitvoeren van een failover test met geen netwerk. Deze optie wordt getest dat de VM overgenomen wordt, maar moet u netwerkinstellingen voor VM won't testen. [Meer informatie](site-recovery-failover.md#run-a-test-failover) over andere opties.
- Houd rekening met het volgende als u wilt uitvoeren van een niet-geplande failover in plaats van een failover testen:

    - Indien mogelijk moet u afgesloten primaire machines voordat u een niet-geplande failover uitvoert. Dit zorgt ervoor dat de bron- en replica machines die worden uitgevoerd op hetzelfde moment geen hebt.
    - Wanneer u een niet-geplande failover uitvoert wordt stopgezet repliceren van primaire computers zodat alle gegevens delta won't worden doorgeschakeld nadat een niet-geplande failover begint. Ook als u een niet-geplande failover op een abonnement herstel uitvoert wordt deze uitgevoerd totdat voltooid, zelfs als er een fout optreedt.




### <a name="run-a-test-failover"></a>Een failover test uitvoeren

1. Via een enkele VM in **Instellingen**mislukt > **Gerepliceerd Items**, klikt u op de VM > **+ toets Failover**.

    ![Test failover](./media/site-recovery-vmm-to-vmm/test-failover.png)

2. Via een abonnement herstel, klikt u in **Instellingen**mislukt > **Herstel van abonnement**, met de rechtermuisknop op het abonnement > **Test Failover**. Als u wilt maken met een herstel plannen [Volg deze instructies](site-recovery-create-recovery-plans.md).
2. Selecteer in de **Test Failover**, **geen**. Met deze optie test u dat de VM wordt overgenomen zoals verwacht, maar de gerepliceerde VM niet wordt aangesloten op een netwerk.

    ![Test failover](./media/site-recovery-vmm-to-vmm/test-failover1.png)

3. Klik op **OK** om te beginnen met de overname. U kunt de voortgang bijhouden door te klikken op de VM om de eigenschappen te openen of op de taak **Test Failover** in **Instellingen** > **taken** > **herstel van de Site taken**.
4. Wanneer de taak failover de fase **voltooid testen bereikt** , het volgende doen:

    -  Bekijk de replica VM in de secundaire VMM cloud.
    -  Klik op **de test voltooid** op voltooien van de overname test.
    -  Klik op **notities** als u wilt opnemen en sla eventuele opmerkingen die zijn gekoppeld aan de overname test.

5. De test virtuele machine wordt gemaakt op dezelfde host als host waarop de replica virtuele machine bestaat. Deze is toegevoegd aan de dezelfde cloud waarin de replica virtuele machine zich bevindt.
6. Nadat u hebt gecontroleerd die beginnen VMs succes klikt u op **de toets overname is voltooid**. In deze fase zijn een automatisch gemaakt door Site herstel tijdens een de overname test elementen verwijderd.  

    > [AZURE.NOTE] Als een test failover meer blijft dan twee weken voordat deze voltooid door te dwingen.



### <a name="update-dns-with-the-replica-vm-ip-address"></a>Update DNS met de replica VM IP address

Nadat de replica VM failover mogelijk geen hetzelfde IP-adres als de primaire virtuele machine.

- Virtuele machines worden bijgewerkt via de DNS-server die ze gebruiken nadat ze starten.
- U kunt ook handmatig DNS als volgt bijwerken:

#### <a name="retrieve-the-ip-address"></a>Het IP-adres ophalen

In dit voorbeeldscript uitvoeren om het IP-adres ophalen.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="update-dns"></a>Update DNS

In dit voorbeeldscript uitvoeren om bijwerken van DNS, het IP-adres dat u hebt opgehaald met het vorige voorbeeldscript opgeven.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

## <a name="next-steps"></a>Volgende stappen

Nadat u hebt uw implementatie is ingesteld en uit te voeren, [voor meer informatie](site-recovery-failover.md) over de verschillende soorten failovers.
