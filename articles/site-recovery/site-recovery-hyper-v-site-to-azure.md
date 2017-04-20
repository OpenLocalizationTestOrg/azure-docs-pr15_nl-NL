<properties
    pageTitle="Hyper-V virtuele machines (zonder VMM) repliceren naar Azure Azure Site herstel gebruikt met de portal van Azure | Microsoft Azure"
    description="Wordt beschreven hoe u het implementeren van Azure Site herstellen om de goedkeuringen herhaling, failover en herstel van on-premises implementatie Hyper-V VMs die niet worden beheerd door VMM naar Azure met behulp van de Azure portal"
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
    ms.date="09/19/2016"
    ms.author="raynew"/>


# <a name="replicate-hyper-v-virtual-machines-without-vmm-to-azure-using-azure-site-recovery-with-the-azure-portal"></a>Hyper-V virtuele machines (zonder VMM) repliceren naar Azure Azure Site herstel gebruikt met de portal van Azure

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-hyper-v-site-to-azure.md)
- [Azure klassieke](site-recovery-hyper-v-site-to-azure-classic.md)
- [PowerShell - resourcemanager](site-recovery-deploy-with-powershell-resource-manager.md)



Welkom bij Azure Site herstel! In dit artikel als u wilt repliceren on-premises implementatie Hyper-V virtuele machines die **niet worden** beheerd in System Center virtuele Machines Manager (VMM) wolken naar Azure wordt gebruikt. In dit artikel wordt beschreven hoe replicatie Azure Site herstel gebruiken in de portal van Azure hebt ingesteld.

> [AZURE.NOTE] Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model.md) voor het maken en werken met resources: Azure resourcemanager en klassiek. Azure, heeft ook twee portals – de Azure klassieke portal die ondersteuning biedt voor het implementatiemodel klassieke en de Azure-portal met ondersteuning voor beide implementatiemodellen.

> Azure sites worden hersteld ondersteunt het herstelproces is en de migratie van Hyper-V virtuele machines naar Azure. De stappen in dit artikel identiek toepassen tijdens het configureren van replicatie naar Azure voor herstel of over het migreren van VMs naar Azure

Azure Site herstel van de Azure-portal vindt u een aantal nieuwe functies:

- In de Azure worden-portal, Azure back-up- en Azure Site herstel services gecombineerd tot een enkel herstel Services kluis zodat u kunt instellen en beheren van bedrijfscontinuïteit en herstel (BCDR) vanaf één locatie. Een geïntegreerd dashboard kunt u controleren en beheren van bewerkingen binnen uw on-premises implementatie-sites en de Azure openbare cloud.
- Gebruikers met Azure abonnementen deze is ingericht met het programma dat Cloud oplossing (CSP) van kunnen Site herstel bewerkingen in de portal van Azure nu beheren.
- Herstel van de site in de portal van Azure kunt machines gerepliceerd naar resourcemanager opslag-accounts. Herstel van de Site wordt failover, resourcemanager gebaseerde VMs gemaakt in Azure wordt aangegeven.
- Site-herstel blijft ondersteuning voor replicatie naar klassieke opslag accounts en overname van VMs met het implementatiemodel klassieke.


Na het lezen in dit artikel uw feedback posten onderaan in de sectie Disqus opmerkingen. Technische vragen op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="overview"></a>Overzicht


Organisaties moeten een BCDR strategie die bepaalt hoe apps, werkbelasting en gegevens blijven actief en beschikbaar tijdens geplande en ongeplande downtime en herstellen naar de normale arbeidsvoorwaarden zo snel mogelijk. Uw strategie BCDR blijven behouden zakelijke gegevens veilig en worden hersteld, en ervoor te zorgen dat werkbelasting continu beschikbaar wanneer noodgevallen plaatsvindt.

Herstel van de site is een Azure-service vergeleken met uw strategie BCDR door orchestrating herhaling van fysieke servers on-premises implementatie en virtuele machines in de cloud (Azure) of met een secundaire datacenter. Wanneer er bijvoorbeeld optreden in uw hoofdlocatie bent, kunt u niet naar de secundaire locatie wilt behouden apps en werkbelasting beschikbaar. U kunt niet terug naar uw hoofdlocatie bent wanneer naar de normale bewerkingen wordt. Klik hier als u meer wilt weten in [Wat Azure Site herstel is?](site-recovery-overview.md)

In dit artikel vindt alle gegevens die u moet repliceren on-premises implementatie Hyper-V virtuele machines die **niet worden** beheerd in System Center virtuele Machines Manager (VMM) wolken naar Azure. Het bevat een overzicht architectuur, plannen, informatie en implementatiestappen voor het configureren van servers voor on-premises implementatie, Azure, een herhaling beleid en capaciteit plannen. Nadat u de infrastructuur hebt ingesteld kunt u replicatie op computers die u wilt beveiligen inschakelen en uitvoeren van een failover test de opzet valideren. U kunt ook uw VMs naar migreert Azure eerste uitvoering van een geplande failover en vult u de migratie.

## <a name="business-advantages"></a>Voordelen voor uw bedrijf

- Biedt buiten het bedrijf (Azure) failover voor bedrijven werkbelasting en toepassingen op Hyper-V virtuele machines.
- Biedt één herstel Services console voor eenvoudige instellen en beheren van replicatie, failover en herstel processen.
- Kunt u eenvoudig failovers uitvoeren vanaf de infrastructuur van uw on-premises implementatie naar Azure, en fail af (herstellen) van Azure naar de on-premises implementatie-site.
- U kunt herstel abonnementen met meerdere computers, zodanig configureren dat doorverbonden toepassing werkbelasting samen mislukken.

## <a name="scenario-architecture"></a>Scenario-architectuur

Dit zijn de onderdelen scenario:

- **Hyper-V host of cluster**: On-premises Hyper-V hostservers of clusters. De Hyper-V hosts VMs die u wilt beveiligen met worden verzameld in logische Hyper-V sites tijdens de implementatie van sites worden hersteld.
- **Azure Site herstel Provider en herstel services agent**: tijdens de implementatie u de Provider van Azure Site herstel als de Microsoft Azure herstel Services-agent op installeren Hyper-V host-servers. De Provider communiceert met Azure Site herstel via HTTPS 443 repliceren integratie. De agent op de server van Hyper-V host gegevens worden gerepliceerd naar Azure opslag via HTTPS 443 al dan niet standaard.
- **Azure**: U moet een Azure-abonnement, een Azure opslag-mailaccount wilt gerepliceerd winkelgegevens en een Azure virtuele netwerk zodat Azure VMs na failover zijn verbonden met een netwerk.

![Hyper-V sitearchitectuur](./media/site-recovery-hyper-v-site-to-azure/architecture.png)



## <a name="azure-prerequisites"></a>Azure vereisten

Hier ziet u wat u nodig hebt in Azure wordt aangegeven om te implementeren in dit scenario.

**Vereiste** | **Meer informatie**
--- | ---
**Azure-account**| U moet een [Microsoft Azure](http://azure.microsoft.com/) -account. U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstelproces is Site prijzen.
**Azure-opslag** | U moet een standaard-opslag-account. U kunt een LRS of GRS opslag-account. Het is raadzaam GRS zodat gegevens robuuste als er een regionale storing optreedt of als de primaire regio kan niet worden hersteld. [Meer informatie](../storage/storage-redundancy.md). Het account moet zich in dezelfde gebied, als de kluis herstel Services.<br/><br/> Premium opslag wordt niet ondersteund.<br/><br/> Gerepliceerde gegevens worden opgeslagen in de opslagruimte van de Azure en Azure VMs zijn gemaakt bij een storing.<br/><br/> [Lees over](../storage/storage-introduction.md) Azure opslagruimte.
**Azure-netwerk** | U moet een Azure virtuele netwerk dat Azure VMs maakt verbinding met bij een storing. Het Azure virtuele netwerk moet zich in hetzelfde gebied, als de kluis herstel Services.

## <a name="on-premises-prerequisites"></a>Vereisten voor on-premises implementatie

Hier ziet u wat u nodig hebt on-premises implementatie.

**Vereiste** | **Meer informatie**
--- | ---
**Hyper-V** | Een of meer on-premises implementatie servers waarop **Windows Server 2012 R2** met de meest recente updates en de functie Hyper-V ingeschakeld of **Microsoft Hyper-V Server 2012 R2**wordt uitgevoerd.<br/><br/>De server Hyper-V moet een of meer virtuele machines bevatten.<br/><br/>Hyper-V servers moeten zijn verbonden met Internet, rechtstreeks of via een proxy.<br/><br/>Hyper-V servers moeten reparaties die worden genoemd in [KB2961977](https://support.microsoft.com/en-us/kb/2961977 "KB2961977") geïnstalleerd hebben.
**Provider en agent** | Tijdens de implementatie van Azure Site herstel installeert u de Provider herstel van Azure-Site. De Provider-installatie wordt ook de Azure herstel Services-Agent installeren op elke Hyper-V virtuele machines die u wilt beveiligen. Alle Hyper-V-servers in een kluis herstel van de Site moeten dezelfde versie van de Provider en agent hebben.<br/><br/>De Provider moet verbinding maken met Azure Site herstel via Internet. Verkeer kan worden verzonden rechtstreeks of via een proxy. Houd er rekening mee dat HTTPS gebaseerd proxy niet wordt ondersteund. De proxyserver moet toegang geeft tot: <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blog.core.windows.net <br/><br/> *Store.Core.Windows.NET <br/><br/> https://www.msftncsi.com/ncsi.txt<br/><br/>Als u IP-adres gebaseerde firewallregels op de server hebt, controleert u dat de regels communicatie met Azure toestaan. Moet u toestaan dat het [IP-bereiken gebruikt Azure Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) en de poort HTTPS (443).<br/><br/>IP-adresbereiken voor Azure buiten het gebied van uw abonnement en voor West Amerikaans toestaan.

## <a name="protected-machine-prerequisites"></a>Beveiligde machine vereisten


**Vereiste** | **Meer informatie**
--- | ---
**Beveiligde VMs** | Voordat u niet over een VM hebt u nodig hebt om ervoor te zorgen dat de naam die wordt toegewezen aan de VM Azure [Azure vereisten](site-recovery-best-practices.md#azure-virtual-machine-requirements)voldoet. Wanneer u replicatie voor de VM hebt ingeschakeld, kunt u de naam wijzigen.<br/><br/> Afzonderlijke schijfcapaciteit op beveiligde computers niet mag zijn meer dan 1023 GB. Een VM kan maximaal 16 schijven hebben (dus tot 16 TB).<br/><br/> Gedeeld schijf Gast clusters worden niet ondersteund.<br/><br/> Als de bron VM NIC-koppeling heeft wordt deze geconverteerd naar een enkel NIC na failover naar Azure.<br/><br/>Beveiligen van VMs waarop Linux met een statisch IP-adres wordt niet ondersteund.

## <a name="prepare-for-deployment"></a>Voorbereiden voor implementatie

Voorbereiden voor implementatie, u moet:

1. [Een Azure netwerk instellen](#set-up-an-azure-network) waarin Azure VMs zich bevinden wanneer ze na failover hebt gemaakt.
2. [Een account Azure opslag instellen](#set-up-an-azure-storage-account) voor gerepliceerde gegevens.
3. [Voorbereiden het Hyper-V hosts](#prepare-the-hyper-v-hosts) om ervoor te zorgen dat ze toegang tot de gewenste URL's.

### <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen

Stel een Azure netwerk. U moet dit zodat de VMs Azure gemaakt nadat failover zijn verbonden met een netwerk.

- Het netwerk moet in de regio hetzelfde als de relatie waarin u de kluis herstel Services wordt implementeert.
- Afhankelijk van de resource model dat u gebruiken wilt voor is mislukt via Azure VMs, moet u het Azure network [Resourcemanager](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) of [klassieke modus](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)instellen.
- Het is raadzaam om dat u een netwerk hebt ingesteld, voordat u begint. Als u niet dat u moet tijdens de implementatie van sites worden hersteld.

> [AZURE.NOTE] [Migratie van netwerken](../resource-group-move-resources.md) via resourcegroepen binnen hetzelfde abonnement of via abonnementen wordt niet ondersteund voor netwerken die zijn gebruikt voor het implementeren van sites worden hersteld.

### <a name="set-up-an-azure-storage-account"></a>Een Azure opslag-mailaccount instellen

- U moet een standaard Azure opslag-account hebben voor gegevens gerepliceerd naar Azure.
- Afhankelijk van de resource model dat u gebruiken wilt voor is mislukt via Azure VMs, hebt u een account instellen in [Resourcemanager](../storage/storage-create-storage-account.md) of [klassieke modus](../storage/storage-create-storage-account-classic-portal.md).
- Het is raadzaam dat u een opslag account instellen voordat u begint. Als u niet dat u moet tijdens de implementatie van sites worden hersteld. De accounts moeten in hetzelfde gebied, als de kluis herstel Services.

> [AZURE.NOTE] [Migratie van opslag accounts](../resource-group-move-resources.md) via resourcegroepen binnen hetzelfde abonnement of via abonnementen wordt niet ondersteund voor opslag-accounts gebruikt voor het implementeren van sites worden hersteld.

### <a name="prepare-the-hyper-v-hosts"></a>De hosts Hyper-V voorbereiden

- Zorg ervoor dat de hosts Hyper-V voldoen aan de [vereisten](#on-premises-prerequisites).

### <a name="create-a-recovery-services-vault"></a>Een kluis herstel Services maken

1. Meld u aan bij de [portal van Azure](https://portal.azure.com).
2. Klik op **nieuwe** > **Management** > **back-up- en Site-herstel (OMS)**. U kunt ook klikken op **Bladeren** > **Herstel Services** kluizen > **toevoegen**.

    ![Nieuwe kluis](./media/site-recovery-hyper-v-site-to-azure/new-vault3.png)

3. Geef in het vak **naam** een beschrijvende naam voor de kluis. Als u meer dan één abonnement hebt, selecteert u een van deze.
4. [Een nieuwe resourcegroep maken](../resource-group-template-deploy-portal.md) of Selecteer een bestaande eigenschap en geef een Azure regio. Machines worden gerepliceerd naar dit gebied. Ondersteunde regio's Zie geografische beschikbaar is in [Azure Site herstel prijzen Details](https://azure.microsoft.com/pricing/details/site-recovery/) controleren
4. Als u wilt snel toegang tot de kluis vanuit het Dashboard klikt u op **vastmaken aan dashboard** en klik op **maken kluis**.

    ![Nieuwe kluis](./media/site-recovery-hyper-v-site-to-azure/new-vault-settings.png)

De nieuwe kluis wordt weergegeven op het **Dashboard** > **alle resources**, en klik op het hoofdweergavegebied **herstel Services kluizen** blade.

## <a name="getting-started"></a>Aan de slag

Herstel van de site biedt een aan de slag ervaring waarmee u zo snel mogelijk implementeren. Aan de slag controleert de vereisten en begeleidt u bij de Site herstel implementatie in de juiste volgorde stappen.

In aan de slag selecteert u het type van machines die u wilt repliceren en waar u wilt repliceren naar. U instellen servers van de on-premises implementatie, Azure opslag accounts en netwerken. Replicatie-beleid maken en capaciteitsplanning uitvoert. Nadat u hebt ingesteld met de infrastructuur van uw schakelt u replicatie voor VMs. U kunt uitvoeren failovers voor specifieke machines of maak herstel abonnementen op meerdere computers is mislukt.

Begint aan de slag door te kiezen hoe u wilt implementeren sites worden hersteld. De stroom aan de slag verandert enigszins afhankelijk van uw replicatievereisten voor.



## <a name="step-1-choose-your-protection-goals"></a>Stap 1: Uw doelstellingen beveiliging kiezen

Selecteer wat u wilt repliceren en waar u wilt repliceren naar.

1. Klik in het blad **herstel Services kluizen** uw kluis en klik op **Instellingen**.
2. In **Instellingen** > **Aan de slag** Klik op **Site-herstel** > **stap 1: voorbereiden infrastructuur** > **beveiliging doel**.

    ![Kies doelstellingen](./media/site-recovery-hyper-v-site-to-azure/choose-goals.png)

3. Selecteer **Naar Azure**in **beveiliging doel** en selecteer **Ja, met Hyper-V**. Selecteer **Nee** om te bevestigen dat u VMM niet gebruikt. Klik vervolgens op **OK**.

    ![Kies doelstellingen](./media/site-recovery-hyper-v-site-to-azure/choose-goals2.png)


## <a name="step-2-set-up-the-source-environment"></a>Stap 2: De bronomgeving configureren

De Hyper-V-site instellen, de Provider herstel van Azure-Site en de Azure herstel Services-agent op Hyper-V hosts installeren en registreren van de hosts in de kluis.


1. Klik op **stap 2: voorbereiden infrastructuur** > **bron**. Als u wilt een nieuwe Hyper-V-site toevoegen als een container voor uw Hyper-V hosts of clusters, klikt u op **+ Hyper-V-Site**.

    ![Bron instellen](./media/site-recovery-hyper-v-site-to-azure/set-source1.png)

2. Geef in het blad **Hyper-V maken site** een naam voor de site. Klik vervolgens op **OK**. Selecteer de site die u zojuist hebt gemaakt.

    ![Bron instellen](./media/site-recovery-hyper-v-site-to-azure/set-source2.png)

3. Klik op **+ Hyper-V Server** als u wilt toevoegen van een server naar de site.
4. Bij **Server toevoegen** > **servertype** controleren dat **Hyper-V server** wordt weergegeven. Zorg ervoor dat de Hyper-V server die u wilt toevoegen aan de [vereisten](#on-premises-prerequisites) voldoet en kan voor toegang tot de opgegeven URL's.
4. Download het installatiebestand Azure Site herstel-Provider. U kunt dit bestand voor de installatie van de Provider zowel de herstel Services-agent op elke host Hyper-V uitvoeren.
5. Download de registratie-toets. U moet dit wanneer u het installatieprogramma uitvoert. De sleutel is ongeldig voor 5 dagen nadat u deze hebt gegenereerd.

    ![Bron instellen](./media/site-recovery-hyper-v-site-to-azure/set-source3.png)

6. Voer de Provider installatiebestand op elke host die u hebt toegevoegd aan de site Hyper-V. Als u de installatie uitvoert op een cluster Hyper-V, voert u setup op elk clusterknooppunt. Installeren en registreren van elke knooppunten Hyper-V zorgt u ervoor dat virtuele machines beveiligd blijven, zelfs als ze op knooppunten migreren.

### <a name="install-the-provider-and-agent"></a>De Provider en agent installeren

1. Voer de Provider installatiebestand.
2. In de **Microsoft Update** kunt u ervoor kiezen op updates zodat Provider updates worden geïnstalleerd overeenkomstig uw beleid van Microsoft Update.
3. Accepteren of wijzig de standaardlocatie voor de installatie van Provider in **installatie** en klikt u op **installeren**.
4. Pagina **Kluis instellingen** klikt u op **Bladeren** om de belangrijkste kluis-bestand dat u hebt gedownload te selecteren. Geef het abonnement Azure sites worden hersteld, de naam van de kluis en de Hyper-V-site die de server Hyper-V hoort.

    ![Serverregistratie](./media/site-recovery-hyper-v-site-to-azure/provider3.png)

5. Geef op hoe de Provider die wordt geïnstalleerd op de server maakt verbinding met Azure Site herstel via internet in **Proxy-instellingen** .

- Als u wilt dat de Provider verbinding selecteert u **verbinding maken rechtstreeks zonder proxy**.
- Als u wilt communiceren met de proxy die momenteel ingesteld op de server selecteert u **verbinding maken met bestaande proxy-instellingen**.
- Als uw bestaande proxy is verificatie vereist of als u wilt een aangepaste proxy gebruiken voor de Provider verbinding Selecteer **verbinding maken met aangepaste proxy-instellingen**.
- Als u een aangepaste proxy gebruiken moet u het adres, poorten en referenties opgeven
- Als u gebruikmaakt van een tabelmaakquery proxy zijn weet u de URL's die worden beschreven in de [vereisten](#on-premises-prerequisites) toegestaan door de werkmap.

    ![Internet](./media/site-recovery-hyper-v-site-to-azure/provider7.PNG)

6. Klik op **registreren** om te registreren van de server in de kluis nadat de installatie is voltooid.

![Locatie installeren](./media/site-recovery-hyper-v-site-to-azure/provider2.png)

7. Nadat de registratie is voltooid metagegevens van de Hyper-V server is opgehaald door herstel van Azure-Site en de server wordt weergegeven op de **Instellingen** > **Site herstel infrastructuur** > **Hyper-V Hosts** blade.


### <a name="command-line-installation"></a>Opdrachtregel installatie

De Provider van Azure Site herstel en agent kunnen ook worden geïnstalleerd via de volgende opdrachtregel. Deze methode kan worden gebruikt voor het installeren van de provider op een Server Core voor Windows Server 2012 R2.

1. Download de sleutel Provider installatie en registratie naar een map. Bijvoorbeeld C:\ASR.
2. Vanuit een opdrachtprompt met verhoogde bevoegdheid deze opdrachten kunt ophalen van het installatieprogramma van Provider uitvoeren:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Deze opdracht voor het installeren van de onderdelen uitvoeren:

            C:\ASR> setupdr.exe /i

4. Voer deze opdrachten de server registreren in de kluis: CD C:\Program Files\Microsoft Azure Site herstel Provider\
            C:\Program Files\Microsoft Azure Site herstel Provider\> DRConfigurator.exe /r /Friendlyname <friendly name of the server> /referenties<path of the credentials file>

<br/>
Waarbij geldt:

- **/Credentials** : verplichte parameter die de locatie waarin het belangrijkste registratie-bestand zich bevindt  
- **/FriendlyName** : parameter is vereist voor de naam van de Hyper-V host-server die wordt weergegeven in de portal Azure sites worden hersteld.
- **/proxyAddress** : optionele parameter die het adres van de proxyserver.
- **/proxyport** : optionele parameter die de poort van de proxyserver.
- **/proxyUsername** : optionele parameter die de Proxy-gebruikersnaam (als proxy is verificatie vereist).
- **/proxyPassword** : optionele parameter die het wachtwoord voor het verifiëren met de proxyserver (als proxy is verificatie vereist).


## <a name="step-3-set-up-the-target-environment"></a>Stap 3: De doelomgeving configureren

Geef de opslag van Azure-account moet worden gebruikt voor replicatie en het Azure netwerk waarmee Azure VMs verbinding maakt na failover.

1.  Klik op **de infrastructuur voorbereiden** > **doeltoepassing** en selecteer het Azure abonnement dat u wilt gebruiken.
2.  Geef de implementatiemodel dat u wilt gebruiken voor VMs na failover.
3.  Herstel van de site wordt gecontroleerd dat er een of meer compatibele Azure opslag accounts en netwerken.

    ![Opslag](./media/site-recovery-hyper-v-site-to-azure/select-target.png)

4.  Als u een account opslag nog niet hebt gemaakt en u wilt maken van een Resource Manager gebruiken klikt u op **+ opslagruimte** account moet die inline. Geef een accountnaam, type, abonnement en locatie op het blad **opslag-account maken** . Het account moet zich in dezelfde locatie als het herstelproces is Services kluis.

    ![Opslag](./media/site-recovery-hyper-v-site-to-azure/gs-createstorage.png)

    Als u wilt een opslag account maken met het klassieke model doen kunt die [in de portal van Azure](../storage/storage-create-storage-account-classic-portal.md).

5.  Als u een Azure-netwerk nog niet hebt gemaakt en u wilt maken van een Resource Manager gebruiken Klik op **+ netwerk** moet die inline. Geef de netwerknaam van een, adresbereiken subnet details, abonnement en locatie op het blad **virtueel netwerk maken** . Het netwerk moet op dezelfde locatie als het herstelproces is Services kluis.

    ![Netwerk](./media/site-recovery-hyper-v-site-to-azure/gs-createnetwork.png)

    Als u wilt maken van een netwerk met behulp van het klassieke model doen kunt die [in de portal van Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).


## <a name="step-4-set-up-replication-settings"></a>Stap 4: Herhaling instellingen instellen

1. U maakt een nieuwe herhaling beleid klikt u op **voorbereiden infrastructuur** > **Replicatie-instellingen** > **+ maken en koppelen**.

    ![Netwerk](./media/site-recovery-hyper-v-site-to-azure/gs-replication.png)

2. Geef de naam van een beleid in **maken en koppelen beleid** .
3. Geef in het **kopiëren van frequentie** op hoe vaak u wilt repliceren delta gegevens na de eerste replicatie (elke 30 seconden, 5 of 15 minuten).
4. Geef in **herstel wijst u bewaarbeleid**, op binnen enkele uren hoe lang het venster bewaarbeleid voor elk opsommingsteken herstel. Beveiligde machines kunnen herstellen naar een willekeurige plaats in een venster.
6. In de **App-consistente momentopname frequentie** Geef op hoe vaak (1-12 uur) herstel punten die van toepassing-consistente momentopnamen gemaakt. Hyper-V gebruikt twee typen momentopnamen, een momentopname van een standaard waarmee een incrementele momentopname van de hele virtuele machine en een momentopname van een toepassing consistente dat een momentopname van het punt-in-tijd van de toepassingsgegevens in de virtuele machine. Volume schaduw Copy Service (VSS) toepassing consistente momentopnamen gebruiken om ervoor te zorgen dat de toepassingen in een consistente status zijn wanneer de momentopname wordt gemaakt. Houd er rekening mee dat als u de toepassing consistente momentopnamen inschakelt, dit invloed is op de prestaties van toepassingen op bron virtuele machines. Zorg ervoor dat de ingestelde waarde kleiner is dan het aantal aanvullende herstel punten die u configureren.
3. Geef op wanneer u de eerste replicatie starten in **eerste herhaling begintijd** . De replicatie vindt plaats via de bandbreedte van uw internet, zodat u kunt deze buiten uw bezet uren plannen. Klik vervolgens op **OK**.

    ![Replicatiebeleid](./media/site-recovery-hyper-v-site-to-azure/gs-replication2.png)

Wanneer u een nieuw beleid maakt, is deze automatisch gekoppeld aan de site Hyper-V. Klik op **OK**. U kunt een site Hyper-V (en de VMs erin) koppelen aan meerdere herhaling beleidsregels in **Instellingen** > **herhaling** > beleidsnaam > **Hyper-V Site koppelen**.

## <a name="step-5-capacity-planning"></a>Stap 5: Capaciteit plannen

Nu dat u uw basic hebt infrastructuur instellen u kunt denken over het plannen van capaciteit en bepalen of u aanvullende informatie nodig hebt.

Herstel van de site biedt een planner capaciteit waarmee u de juiste resources toewijzen voor uw bronomgeving, de site herstel onderdelen, netwerken en opslag. U kunt de planner uitvoeren in de snelle modus voor schattingen op basis van een gemiddelde aantal VMs, schijven en opslag of in de gedetailleerde weergave waarin u afbeeldingen op het niveau van de werkbelasting kunt invoert. Voordat u begint moet u:

- Verzamel informatie over uw replicatieomgeving, inclusief VMs, schijven per VMs en opslagruimte per schijf.
- Schatting van de dagelijkse wijzigen (lospeuteren)-mate dat u voor gerepliceerde gegevens hebt. U kunt de [Capaciteit Planner voor Hyper-V Replica](https://www.microsoft.com/download/details.aspx?id=39057) gebruiken om te helpen u dit kunt doen.

1.  Klik op **downloaden** om te downloaden van het hulpmiddel en voer dit uit. [Lees het artikel](site-recovery-capacity-planner.md) waarop het hulpmiddel.
2.  Wanneer u klaar bent Selecteer **Ja** in het **hebt u de capaciteit Planner uitvoeren**?

    ![Capaciteit plannen](./media/site-recovery-hyper-v-site-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Aandachtspunten voor de bandbreedte netwerk

U kunt de capaciteit teamplanner hulpmiddel voor het berekenen van de bandbreedte die u nodig voor replicatie (eerste replicatie en klik vervolgens delta hebt). Als u wilt bepalen welke bandbreedtegebruik voor replicatie hebt u een paar opties:

- **De bandbreedte**: Hyper-V-verkeer die wordt overgenomen door Azure doorloopt in een specifieke Hyper-V-host. U kunt de bandbreedte op de hostserver beperken.
- **Bandbreedte aanpassen**: kunt u de bandbreedte gebruikt voor replicatie met een paar registersleutels beïnvloeden.

#### <a name="throttle-bandwidth"></a>De bandbreedte

1. Open de module MMC van Microsoft Azure back-up op de server van Hyper-V host. Een snelkoppeling voor back-up van Microsoft Azure is al dan niet standaard beschikbaar op het bureaublad of in C:\Program Files\Microsoft Azure herstel Services Agent\bin\wabadmin.
2. Klik op **Eigenschappen wijzigen**in de module.
3. Klik op het tabblad **Throttling** Selecteer **internetbandbreedte beperken voor back-bewerkingen inschakelen**, en de limieten voor werk instellen en niet-werk uren. Geldige bereiken zijn 512 k tot 102 Mbps per seconde.

    ![De bandbreedte](./media/site-recovery-hyper-v-site-to-azure/throttle2.png)

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

## <a name="step-6-enable-replication"></a>Stap 6: Inschakelen herhaling

Nu als volgt herhaling inschakelen:

1. Klik op **stap 2: toepassing repliceren** > **bron**. Wanneer u replicatie voor de eerste keer hebt ingeschakeld moet u klikken op **+ repliceren** in de kluis replicatie voor extra machines inschakelen.

    ![Replicatie inschakelen](./media/site-recovery-hyper-v-site-to-azure/enable-replication.png)

2. Klik in het blad **bron** > Selecteer de Hyper-V-site. Klik vervolgens op **OK**.
3. Selecteer het abonnement kluis en het failover-model dat u wilt gebruiken in Azure wordt aangegeven (classic of resource management) na failover in **doel** .
4. Selecteer het opslag-account dat u wilt gebruiken. Als u wilt gebruiken een andere opslag-account dan die hebt u u kunt [een account maakt](#set-up-an-azure-storage-account). Klik op **Nieuw**om een opslag account met behulp van het model resourcemanager. Als u wilt een opslag account maken met het klassieke model doen kunt die [in de portal van Azure](../storage/storage-create-storage-account-classic-portal.md). Klik vervolgens op **OK**.
5.  Selecteer de Azure netwerk en subnet waarmee Azure VMs verbinding maakt wanneer ze bent of omhoog na failover. Selecteer **configureren nu voor geselecteerde computers** de netwerkinstelling toepassen op alle computers die u selecteren voor de beveiliging. Selecteer **later configureren** om te selecteren van de Azure netwerk per computer. Als u wilt gebruiken van een ander netwerk van die hebt u u kunt [een account maakt](#set-up-an-azure-network). Klik op **Nieuw**om een netwerk met behulp van het model resourcemanager. Als u wilt maken van een netwerk met behulp van het klassieke model doen kunt die [in de portal van Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Selecteer een subnet, indien van toepassing. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-hyper-v-site-to-azure/enable-replication11.png)

6. In de **virtuele Machines** > **virtuele machines selecteren** op en selecteer elke computer die u wilt repliceren. U kunt alleen machines waarvoor replicatie kan worden ingeschakeld selecteren. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-hyper-v-site-to-azure/enable-replication5.png)

11. In de **Eigenschappen van** > **eigenschappen configureren**, selecteer het besturingssysteem voor de geselecteerde VMs en de schijf OS. Bevestigen dat de naam van de Azure VM (doelnaam) voldoet aan de eisen van [Azure virtuele machines](site-recovery-best-practices.md#azure-virtual-machine-requirements) en als u wilt wijzigen. Klik vervolgens op **OK**. U kunt aanvullende eigenschappen later instellen.

    ![Replicatie inschakelen](./media/site-recovery-hyper-v-site-to-azure/enable-replication6.png)

12. In de **Instellingen voor replicatie** > **replicatie-instellingen configureren**, selecteer het replicatiebeleid dat u wilt toepassen voor de beveiligde VMs. Klik vervolgens op **OK**. U kunt het beleid herhaling in **Instellingen**wijzigen > **herhaling beleidsregels** > beleidsnaam > **Instellingen bewerken**. Wijzigingen die u toepast worden gebruikt voor machines die al zijn repliceren en nieuwe machines.

    ![Replicatie inschakelen](./media/site-recovery-hyper-v-site-to-azure/enable-replication7.png)

U kunt de voortgang van de taak **Beveiliging inschakelen** in **Instellingen**bijhouden > **taken** > **herstel van de Site taken**. Nadat de taak **Voltooien beveiliging** wordt uitgevoerd van de computer is klaar voor failover.

### <a name="view-and-manage-vm-properties"></a>Weergeven en beheren van VM eigenschappen

Het is raadzaam dat u de eigenschappen van de broncomputer controleren.

1. Klik op **Instellingen** > **Beveiligde Items** > **Items gerepliceerd** > en selecteert u de computer.

    ![Replicatie inschakelen](./media/site-recovery-hyper-v-site-to-azure/test-failover1.png)

2. In de **Eigenschappen** kunt u replicatie en overname bij storing informatie voor VM weergeven.

    ![Replicatie inschakelen](./media/site-recovery-hyper-v-site-to-azure/test-failover2.png)

3. In het **netwerk en rekenkracht** > **berekenen eigenschappen** kunt u de grootte van de naam en doelsites Azure VM opgeven. De naam om te voldoen aan vereisten voor Azure als u wilt wijzigen. U kunt ook weergeven en wijzigen van informatie over de doelnetwerk, subnet en IP-adres dat wordt toegewezen aan de VM Azure. Houd rekening met het volgende:

    - U kunt het target IP-adres instellen. Als u niet een adres, de mislukte via machine opgeeft DHCP gebruikt. Als u een adres dat is niet beschikbaar bij failover hebt ingesteld, wordt de overname, mislukt. Hetzelfde target IP-adres kan worden gebruikt voor test failover als het adres in het netwerk van de failover-test beschikbaar is.
    - Het aantal netwerkadapters wordt bepaald door de grootte die u voor de doeltoepassing virtuele machine, als volgt opgeeft:

        - Als het aantal netwerkadapters op de broncomputer kleiner dan of gelijk is aan het aantal adapters toegestaan voor de grootte van de computer doel is, klikt u vervolgens heeft het doel hetzelfde aantal adapters als de bron.
        - Als het aantal adapters voor de bron virtuele machine groter is dan het getal dat is toegestaan voor de doelgrootte en vervolgens de doeltoepassing maximaal wordt gebruikt.
        - Als u een bron-machine heeft twee netwerkadapters en de grootte van de computer doel ondersteunt vier, de doelcomputer heeft twee adapters bijvoorbeeld. Als de broncomputer twee adapters heeft, maar de doelgrootte van de ondersteunde alleen een ondersteunt wordt slechts één adapter hebben op de doelcomputer.     
        - Als de VM meerdere netwerkadapters die ze alle verbinding maken met hetzelfde netwerk heeft worden.

    ![Replicatie inschakelen](./media/site-recovery-hyper-v-site-to-azure/test-failover4.png)

5.  In **schijven** kunt u het besturingssysteem en gegevensschijven bekijken op de VM die worden gerepliceerd.


## <a name="step-7-test-the-deployment"></a>Stap 7: De implementatie testen

U kunt een failover test voor een enkele virtuele machine of een herstel-abonnement met een of meer virtuele machines uitvoeren als u wilt testen van de implementatie.


### <a name="prepare-for-test-failover"></a>Test failover voorbereiden

- Als u wilt uitvoeren van een test failover is het raadzaam dat u een nieuw Azure netwerk die heeft geïsoleerd vanuit het Azure productienetwerk maken (dit is zich standaard gedraagt wanneer u een nieuw netwerk in Azure maken). [Meer informatie](site-recovery-failover.md#run-a-test-failover) over het uitvoeren van test failovers.
- Als u de beste prestaties wanneer u niet Azure, de Azure-Agent op de beveiligde computer te installeren. Wordt sneller opstarten en helpt bij het oplossen van. Installeer de [Linux](https://github.com/Azure/WALinuxAgent) of [Windows](http://go.microsoft.com/fwlink/?LinkID=394789) -agent.
- Als u wilt uw implementatie volledig testen moet u een infrastructuur voor de gerepliceerde machine werkt zoals verwacht. Als u testen wilt, Active Directory en DNS kunt u een virtuele machine maken als een domeincontroller met DNS- en repliceren dit naar Azure met Azure sites worden hersteld. Lees meer in [test failover aandachtspunten voor Active Directory](site-recovery-active-directory.md#considerations-for-test-failover).
- Houd rekening met het volgende als u wilt uitvoeren van een niet-geplande failover in plaats van een failover testen:

    - Indien mogelijk moet u afgesloten primaire machines voordat u een niet-geplande failover uitvoert. Dit zorgt ervoor dat de bron- en replica machines die worden uitgevoerd op hetzelfde moment geen hebt.
    - Wanneer u een niet-geplande failover uitvoert wordt stopgezet repliceren van primaire computers zodat alle gegevens delta won't worden doorgeschakeld nadat een niet-geplande failover begint. Ook als u een niet-geplande failover op een abonnement herstel uitvoert wordt deze uitgevoerd totdat voltooid, zelfs als er een fout optreedt.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Verbinding maken met Azure VMs na failover voorbereiden

Als u wilt verbinden met Azure VMs met RDP na failover, controleert u of doen u het volgende:

Klik **op de lokale computer voordat failover wordt uitgevoerd**:

- Voor toegang via internet RDP inschakelen, zorg ervoor dat alle TCP- en UDP regels worden toegevoegd voor de **openbare**en zorg ervoor dat RDP is toegestaan in de **Windows Firewall** -> **toegestane apps en -functies** voor alle profielen.
- Voor toegang via de verbinding van een site-naar-site RDP inschakelen op de computer en zorg ervoor dat RDP is toegestaan in de **Windows Firewall** -> **toegestane apps en -functies** voor het **domein** en **particuliere** netwerken.
- Installeer de [Azure VM agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) op de lokale computer.
- Zorg ervoor dat het besturingssysteem SAN-beleid is ingesteld op OnlineAll. [Meer informatie]( https://support.microsoft.com/kb/3031135)
- De IPSec-service uitschakelen voordat u de overname uitvoert.

**Klik op het Azure VM na failover**:

- Een openbare IP-adres toevoegen aan de NIC die is gekoppeld aan de VM Azure toe te staan dat RDP.
- Controleer of er geen domeinbeleid dat voorkomen dat u verbinding maakt met een virtuele machine met een openbare adres.
- Maak verbinding met. Controleer of de VM wordt uitgevoerd als u geen verbinding maken. Lees dit [artikel](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)voor meer tips.

Als u toegang wilt tot een Azure VM waarop Linux na failover Secure Shell mailclient gebruikt (ssh), doet u het volgende:

Klik **op de lokale computer voordat failover wordt uitgevoerd**:

- Zorg ervoor dat de service Secure Shell op de VM Azure is ingesteld op automatisch starten op het opstarten.
- Controleer dat firewallregels een SSH-verbinding met deze.

**Klik op het Azure VM na failover**:

- De groep beveiligingsregels voor netwerken op de mislukte via VM en het Azure subnet waaraan dit is aangesloten moeten binnenkomende verbindingen met de poort SSH toestaan.
- Een openbare eindpunt moet worden gemaakt zodat binnenkomende verbindingen op de SSH poort (TCP-poort 22 al dan niet standaard).
- Als de VM toegankelijk is via een VPN-verbinding (Express Route of site-naar-site VPN) kan de client rechtstreeks via verbinding maken met de VM SSH worden gebruikt.

### <a name="run-a-test-failover"></a>Een failover test uitvoeren

Ga op de toets failover voert u de volgende handelingen uit:

1. Via een enkele VM in **Instellingen**mislukt > **Gerepliceerd Items**, klikt u op de VM > **+ toets Failover**.

    ![Test failover](./media/site-recovery-hyper-v-site-to-azure/run-failover1.png)

2. Via een abonnement herstel, klikt u in **Instellingen**mislukt > **Herstel van abonnement**, met de rechtermuisknop op het abonnement > **Test Failover**. Als u wilt maken met een herstel plannen [Volg deze instructies](site-recovery-create-recovery-plans.md).

3. Selecteer het Azure netwerk waarnaar Azure VMs worden verbonden nadat storing in **Test Failover** .

    ![Test failover](./media/site-recovery-hyper-v-site-to-azure/run-failover2.png)

4. Klik op **OK** om te beginnen met de overname. U kunt de voortgang bijhouden door te klikken op de VM de properies openen of op de taak **Test Failover** in **Instellingen** > **herstel van de Site taken**.
5. Wanneer de overname de fase **voltooid testen bereikt** , het volgende doen:
    1. Bekijk de replica virtuele machine in de portal van Azure. Controleer of de virtuele machine is gestart.
    2. Als u ingesteld access virtuele machines uit uw on-premises netwerk bent kunt u een verbinding met extern bureaublad naar de virtuele machine initiëren.
    3. Klik op **de test voltooid** als u wilt voltooien.
    4. Klik op **notities** als u wilt opnemen en sla eventuele opmerkingen die zijn gekoppeld aan de overname test.
    5. Klik op **de toets overname is voltooid**. De testomgeving automatisch uit te schakelen en de test virtuele machine verwijderen opschonen.
    6. In deze fase worden een bepaalde elementen of VMs automatisch aangemaakt door Site herstel tijdens een de overname test verwijderd. Eventuele aanvullende elementen die u hebt gemaakt voor test failover niet worden verwijderd.

    > [AZURE.NOTE] Als een test failover meer blijft dan twee weken voordat deze voltooid door te dwingen.

6. Nadat de overname is voltooid ook moet u de replica Azure zien machine worden weergegeven in de portal van Azure > **virtuele Machines**. U moet ervoor zorgen dat de VM de juiste grootte, heeft dit is verbonden met het juiste netwerk, is en dat deze wordt uitgevoerd.
7. Als u [ervoor dat voor verbindingen na failover](#prepare-to-connect-to-Azure-VMs-after-failover) u verbinding maken met de VM Azure moet.


## <a name="failover"></a>Failover

Nadat de eerste replicatie is voltooid voor uw machines, kunt u failovers aanroepen als nodig is. Site-herstel ondersteunt verschillende soorten failovers - Test failover, gepland failover en niet-geplande failover.
[Meer informatie](site-recovery-failover.md) over de verschillende typen failovers en gedetailleerde beschrijvingen van wanneer en hoe ze uitvoeren.

> [AZURE.NOTE] Als de bedoeling aangeeft virtuele machines migreren naar Azure is, wordt aangeraden dat u een [geplande failover-bewerking](site-recovery-failover.md#run-a-planned-failover-primary-to-secondary) gebruiken om de virtuele machines migreren naar Azure. Zodra de gemigreerde toepassing is gevalideerd in Azure wordt aangegeven met test failover, gebruikt u de stappen in de [Volledige migratie](#Complete-migration-of-your-virtual-machines-to-Azure) genoemde om de migratie van uw virtuele machines te voltooien. U hoeft niet te starten een doorvoeren of verwijderen. Volledige migratie voltooit u de migratie, verwijdert u de beveiliging voor de virtuele machine en Azure Site herstel facturering voor de machine stopt.


###<a name="run-an-unplanned-failover"></a>Een niet-geplande Failover uitvoeren

Dit moet worden gekozen wanneer een primaire-site niet toegankelijk verandert vanwege een onverwachte incident, zoals een power storing of virus aanval. Deze procedure wordt beschreven hoe een "niet-geplande failover' voor een abonnement herstel uitvoeren. U kunt ook de overname voor een enkele virtuele machine uitvoeren op het tabblad virtuele Machines. Controleer voordat u begint, of alle virtuele machines die u wilt mislukken eerste replicatie is voltooid.

1. Selecteer **herstel van abonnement > recoveryplan_name**.
2. Klik op het blad herstel-abonnement op **Niet-geplande Failover**.
3. Kies de bronsite en doelsites locaties op de pagina **niet-geplande Failover** .
4. Selecteer **virtuele machines afsluiten en de meest recente gegevens synchroniseren** om op te geven dat sites worden hersteld proberen moet de beveiligde virtuele machines afsluiten en de gegevens te synchroniseren, zodat de nieuwste versie van de gegevens worden overgenomen.
5. Na de overname zijn de virtuele machines in een doorvoeren in behandeling staat.  Klik op **doorvoeren** als u wilt doorvoeren van de overname.

[Meer informatie](site-recovery-failover.md#run-an-unplanned-failover)

## <a name="complete-migration-of-your-virtual-machines-to-azure"></a>Migratie van uw virtuele machines naar Azure voltooien


>[AZURE.NOTE] De volgende stappen alleen van toepassing als u virtuele machines naar Azure migreert

1. Geplande failover uitvoeren als genoemde [hier](site-recovery-failover.md)
2. In **Instellingen > gerepliceerd items**, met de rechtermuisknop op de virtuele machine en selecteert u **Volledige migratie**

    ![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

2. Klik op **OK** om te voltooien van de migratie. U kunt de voortgang bijhouden door te klikken op de VM om de eigenschappen te openen of met behulp van de taak voltooid migratie in **Instellingen > Site-herstel taken**.


## <a name="monitor-your-deployment"></a>Bewaak uw implementatie

Hier ziet u hoe u de configuratie-instellingen, status en servicestatus voor de implementatie van uw Site herstel kunt controleren:

1. Klik op de naam van de kluis voor toegang tot het dashboard **Essentials** . In dit dashboard kunt u de Site herstel taken, herhaling status, herstel-abonnementen, server gezondheid en gebeurtenissen.  U kunt Essentials om weer te geven van de tegels en -indelingen die voor u zijn, waaronder de status van andere sites worden hersteld en back-up kluizen nuttigste aanpassen.

    ![Essentials](./media/site-recovery-hyper-v-site-to-azure/essentials.png)

2. U kunt siteservers die voordoet zich probleem en de gebeurtenissen die door herstel van de Site in de afgelopen 24 uur controleren in de tegel van de **Servicestatus** .
3. U kunt beheren en herhaling in de **Items gerepliceerd**, **Herstel van abonnement**, controleren en **Site herstel taken** tegels. U kunt inzoomen op taken in **Instellingen** -> **taken** -> **Site herstel taken**.
