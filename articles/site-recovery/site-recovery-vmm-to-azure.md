<properties
    pageTitle="Hyper-V virtuele machines in VMM wolken repliceren naar Azure met herstel van de Site met de portal van Azure | Microsoft Azure"
    description="Wordt beschreven hoe u het implementeren van Azure Site herstellen om de goedkeuringen herhaling, failover en herstel van Hyper-V VMs in VMM wolken naar Azure met behulp van de Azure portal"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/16/2016"
    ms.author="raynew"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-azure-site-recovery-with-the-azure-portal--microsoft-azure"></a>Hyper-V virtuele machines in VMM wolken repliceren naar Azure Azure Site herstel gebruikt met de portal van Azure | Microsoft Azure

> [AZURE.SELECTOR]
- [Azure-portal](site-recovery-vmm-to-azure.md)
- [Azure-klassiek](site-recovery-vmm-to-azure-classic.md)
- [Resourcemanager PowerShell](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [PowerShell-klassiek](site-recovery-deploy-with-powershell.md)

Welkom bij Azure Site herstel! In dit artikel als u wilt repliceren on-premises implementatie Hyper-V virtuele machines beheerd in System Center Virtual Machine Manager (VMM) wolken naar Azure Azure Site herstel gebruiken in de portal van Azure wordt gebruikt.

> [AZURE.NOTE]Azure heeft twee verschillende [implementatiemodellen](../resource-manager-deployment-model
> ) voor het maken en werken met resources: Azure resourcemanager en klassiek. Azure, heeft ook twee portals – de Azure klassieke portal die ondersteuning biedt voor het implementatiemodel klassieke en de Azure-portal met ondersteuning voor beide implementatiemodellen.


Azure Site herstel van de Azure-portal bevat diverse nieuwe functies:

- De services Azure back-up en herstellen van Azure-Site worden in de portal van Azure gecombineerd tot een enkel herstel Services kluis, zodat u kunt instellen en beheren van bedrijfscontinuïteit en herstel (BCDR) vanaf één locatie. Een geïntegreerd dashboard kunt u controleren en beheren van bewerkingen binnen uw on-premises implementatie-sites en de Azure openbare cloud.
- Gebruikers met Azure abonnementen deze is ingericht met het programma dat Cloud oplossing (CSP) van kunnen Site herstel bewerkingen in de portal van Azure nu beheren.
- Herstel van de site in de portal van Azure kunt machines repliceren met Azure resourcemanager opslag accounts. Herstel van de Site wordt failover, resourcemanager gebaseerde VMs gemaakt in Azure wordt aangegeven.
- Site-herstel blijft ondersteuning voor replicatie naar klassieke opslag-accounts. Herstel van de Site maakt failover, VMs met het klassieke model.


Lees dit artikel en posten commentaar onderaan in de opmerkingen Disqus. Technische vragen op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Overzicht

Organisaties moeten een BCDR strategie die bepaalt hoe apps, werkbelasting en gegevens blijven actief en beschikbaar tijdens geplande en ongeplande downtime en herstellen naar de normale arbeidsvoorwaarden zo snel mogelijk. Uw strategie BCDR moet u bedrijfsgegevens veilige en worden hersteld en ervoor te zorgen dat werkbelasting continu beschikbaar blijven wanneer noodgevallen plaatsvindt.

Herstel van de site is een Azure-service vergeleken met uw strategie BCDR van orchestrating replicatie van fysieke servers on-premises implementatie en virtuele machines in de cloud (Azure) of met een secundaire datacenter. Wanneer er bijvoorbeeld optreden in uw hoofdlocatie bent, u niet de secundaire locatie wilt behouden apps en werkbelasting beschikbaar. U terug naar uw hoofdlocatie bent niet en er wordt naar de normale bewerkingen. Klik hier als u meer wilt weten in [Wat Azure Site herstel is?](site-recovery-overview.md)

In dit artikel vindt alle gegevens die u moet repliceren on-premises implementatie Hyper-V VMs in VMM wolken naar Azure. Het bevat een overzicht architectuur, plannen, informatie en implementatiestappen voor het configureren van Azure, on-premises-servers, replicatie-instellingen en capaciteit plannen. Nadat u de infrastructuur hebt ingesteld, kunt u replicatie op computers die u wilt beveiligen en controleer dat die failover werkt inschakelen.


## <a name="business-advantages"></a>Voordelen voor uw bedrijf

- Site-herstel biedt buiten het bedrijf bescherming voor bedrijven werkbelasting en toepassingen op Hyper-V VMs.
- De portal herstel Services biedt één enkele locatie als u wilt instellen, beheren en herhaling, failover en herstel controleren.
- U kunt eenvoudig failovers uit de infrastructuur van uw on-premises implementatie uitvoeren naar Azure en foutherstel (herstellen) van Azure met Hyper-V hostservers in uw on-premises site.
- U kunt herstel abonnementen met meerdere computers zodanig configureren dat doorverbonden toepassing werkbelasting samen mislukken.


## <a name="scenario-architecture"></a>Scenario-architectuur


Dit zijn de onderdelen scenario:

- **VMM server**: een on-premises implementatie VMM server met een of meer wolken.
- **Hyper-V host of cluster**: Hyper-V hostservers of clusters beheerd in VMM wolken.
- **Azure Site herstel Provider en herstel services agent**: tijdens de implementatie u de Provider van Azure Site herstel installeren op de server VMM en de Microsoft Azure herstel Services-agent op Hyper-V hostservers. De Provider op de server VMM communiceert met Site-herstel via HTTPS 443 repliceren integratie. De agent op de server van Hyper-V host gegevens worden gerepliceerd naar Azure opslag via HTTPS 443 al dan niet standaard.
- **Azure**: U moet een Azure-abonnement, een Azure opslag-mailaccount wilt gerepliceerd winkelgegevens en een Azure virtuele netwerk zodat Azure VMs na failover zijn verbonden met een netwerk.

![Topologie van E2A](./media/site-recovery-vmm-to-azure/architecture.png)


## <a name="azure-prerequisites"></a>Azure vereisten

Hier ziet u wat u moet in Azure wordt aangegeven in dit scenario implementeren.

**Vereiste** | **Meer informatie**
--- | ---
**Azure-account**| Hebt u een [Microsoft Azure](http://azure.microsoft.com/) -account nodig. U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstelproces is Site prijzen.
**Azure-opslag** | U moet een standaard Azure opslag-account voor de opslag gerepliceerde gegevens. U kunt een LRS of GRS opslag-account. Het is raadzaam GRS zodat gegevens robuuste als er een regionale storing optreedt of als de primaire regio kan niet worden hersteld. [Meer informatie](../storage/storage-redundancy.md). Het account moet zich in dezelfde gebied, als de kluis herstel Services.<br/><br/>Premium opslag wordt niet ondersteund.<br/><br/> Gerepliceerde gegevens worden opgeslagen in de opslagruimte van de Azure en Azure VMs zijn gemaakt bij een storing. <br/><br/> [Lees over](../storage/storage-introduction.md) Azure opslagruimte.
**Azure-netwerk** | U moet een Azure virtuele netwerk die Azure VMs verbinding maken met bij een storing. Het netwerk moet zich in hetzelfde gebied, als de kluis herstel Services.

## <a name="on-premises-prerequisites"></a>Vereisten voor on-premises implementatie

Hier ziet u wat u nodig hebt on-premises implementatie

**Vereiste** | **Meer informatie**
--- | ---
**VMM**| Een of meer VMM servers waarop systeem Center 2012 R2. Elke VMM-server moet een of meer wolken geconfigureerd hebben. Een wolk moet bevatten:<br/><br/> Een of meer VMM hostgroepen.<br/><br/> Een of meer Hyper-V hostservers of clusters in elke hostgroep.<br/><br/>[Meer informatie](http://www.server-log.com/blog/2011/8/26/vmm-2012-and-the-clouds.html) over het instellen van VMM wolken.
**Hyper-V** | Hyper-V hostservers ten minste moeten worden uitgevoerd met de functie Hyper-V of **Microsoft Hyper-V Server 2012 R2** , **Windows Server 2012 R2** hebt en de meest recente updates hebt geïnstalleerd.<br/><br/> Een server Hyper-V moet een of meer VMs bevatten.<br/><br/> Een Hyper-V hostserver of cluster met VMs die u wilt repliceren moet worden beheerd in een wolk VMM.<br/><br/>Hyper-V servers moeten zijn verbonden met Internet, rechtstreeks of via een proxy.<br/><br/>Hyper-V servers moeten reparaties die worden genoemd in artikel [2961977](https://support.microsoft.com/kb/2961977) geïnstalleerd hebben.<br/><br/>Hyper-V hostservers hebt internettoegang nodig om de gegevensreplicatie te Azure.
**Provider en agent** | Tijdens de implementatie van Azure Site herstel installeert u de Provider herstel van Azure-Site op de server VMM en de herstel Services-agent op Hyper-V hosts. De Provider en agent moeten verbinding maken met Azure via internet rechtstreeks of via een proxy. Houd er rekening mee dat een proxy HTTPS gebaseerde wordt niet ondersteund. De proxyserver op de VMM-server en Hyper-V hosts moet toegang geeft tot: <br/><br/> *. hypervrecoverymanager.windowsazure.com <br/> <br/> *. accesscontrol.windows.net <br/><br/> *. backup.windowsazure.com <br/> <br/> *. blob.core.windows.net <br/><br/> *. store.core.windows.net<br/><br/>Als u IP-adres gebaseerde firewallregels op de server VMM hebt, controleert u dat de regels communicatie met Azure toestaan. Moet u toestaan dat het [IP-bereiken gebruikt Azure Datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) en de poort HTTPS (443).<br/><br/>IP-adresbereiken voor Azure buiten het gebied van uw abonnement en voor West Amerikaans toestaan.<br/><br/>Bovendien. de proxyserver op de server VMM nodig heeft toegang tot https://www.msftncsi.com/ncsi.txt


## <a name="protected-machine-prerequisites"></a>Beveiligde machine vereisten


**Vereiste** | **Meer informatie**
--- | ---
**Beveiligde VMs** | Controleer voordat u niet over een VM, of dat de naam die is toegewezen aan de VM Azure [Azure vereisten](site-recovery-best-practices.md#azure-virtual-machine-requirements)voldoet. Wanneer u replicatie voor de VM hebt ingeschakeld, kunt u de naam wijzigen. <br/><br/> Afzonderlijke schijfcapaciteit op beveiligde computers niet mag zijn meer dan 1023 GB. Een VM kan maximaal 16 schijven hebben (dus tot 16 TB).<br/><br/> Gedeeld schijf Gast clusters worden niet ondersteund.<br/><br/> Unified Extensible Firmware Interface UEFI () / Extensible Firmware Interface(EFI) opstarten wordt niet ondersteund.<br/><br/> Als de bron VM NIC-koppeling heeft wordt deze geconverteerd naar een enkel NIC na failover naar Azure.<br/><br/>Beveiligen van VMs waarop Linux met een statisch IP-adres wordt niet ondersteund.

## <a name="prepare-for-deployment"></a>Voorbereiden voor implementatie

U moet implementatie voorbereiden:

1. [Een Azure netwerk instellen](#set-up-an-azure-network) waarin Azure VMs zich na failover bevinden.
2. [Een account Azure opslag instellen](#set-up-an-azure-storage-account) voor gerepliceerde gegevens.
4. [De server VMM voorbereiden](#prepare-the-vmm-server) voor implementatie sites worden hersteld.
5. [Voorbereiden voor netwerk-toewijzing](#prepare-for-network-mapping). Netwerken zo instellen dat u netwerk toewijzing tijdens de implementatie van de Site herstel kunt configureren.

### <a name="set-up-an-azure-network"></a>Een Azure-netwerk instellen

U moet een Azure netwerk zodat de VMs Azure gemaakt nadat failover maakt verbinding met deze.

- Het netwerk moet in de regio hetzelfde als de relatie waarin u de kluis herstel Services implementeert.
- Afhankelijk van de resource model dat u gebruiken wilt voor is mislukt via Azure VMs, moet u het Azure network [Resourcemanager](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) of [klassieke modus](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)instellen.
- Het is raadzaam om dat u een netwerk hebt ingesteld, voordat u begint. Als u niet, moet u tijdens de implementatie van sites worden hersteld.

> [AZURE.NOTE] [Migratie van netwerken](../resource-group-move-resources.md) via resourcegroepen binnen hetzelfde abonnement of via abonnementen wordt niet ondersteund voor netwerken die zijn gebruikt voor het implementeren van sites worden hersteld.


### <a name="set-up-an-azure-storage-account"></a>Een Azure opslag-mailaccount instellen

- U moet een standaard Azure opslag-account hebben voor gegevens gerepliceerd naar Azure. Het account moet zich in dezelfde gebied, als de kluis herstel Services.
- Afhankelijk van de resource model dat u gebruiken wilt voor is mislukt via Azure VMs, moet u een account in [Resourcemanager](../storage/storage-create-storage-account.md) of [klassieke modus](../storage/storage-create-storage-account-classic-portal.md)instellen.
- Het is raadzaam dat u een account instellen voordat u begint. Als u niet, moet u tijdens de implementatie van sites worden hersteld.

> [AZURE.NOTE] [Migratie van opslag accounts](../resource-group-move-resources.md) via resourcegroepen binnen hetzelfde abonnement of via abonnementen wordt niet ondersteund voor opslag-accounts gebruikt voor het implementeren van sites worden hersteld.

### <a name="prepare-the-vmm-server"></a>De server VMM voorbereiden

- Zorg ervoor dat de server VMM aan de [vereisten voldoet](#on-premises-prerequisites).
- Tijdens de implementatie van de Site-herstel, kunt u opgeven dat alle wolken op een server VMM beschikbaar in de portal van Azure zijn moeten. Als u alleen specifieke wolken wordt weergegeven in de portal wilt, kunt u deze instelling de cloud in de beheerconsole VMM inschakelen.


### <a name="prepare-for-network-mapping"></a>Voorbereiden voor netwerk-toewijzing

Moet u de toewijzing netwerk tijdens de implementatie van de Site herstel instellen. Netwerk toewijzing kaarten tussen bron VMM VM netwerken en afstemmen van Azure-netwerken te gebruiken om in te schakelen van de volgende handelingen uit:

- Machines die in hetzelfde netwerk mislukken kunnen koppelen aan elkaar grenzen, zelfs als ze niet op dezelfde manier of in hetzelfde herstelplan overgenomen bent.
- Als een netwerkgateway is ingesteld op het doel Azure netwerk, kan Azure virtuele machines on-premises implementatie virtuele machines kunt verbinden.
- Als u wilt netwerk instellen is hier toewijzing wat u nodig hebt voor het voorbereiden van:

    - Zorg ervoor dat VMs op de bronserver voor de host van Hyper-V zijn verbonden met een netwerk VMM VM. Dit netwerk moet zijn gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud.
    - Een Azure netwerk als beschreven [bovenstaande](#set-up-an-azure-network)

- [Meer informatie](site-recovery-network-mapping.md) over de werking van netwerk-toewijzing.


## <a name="create-a-recovery-services-vault"></a>Een kluis herstel Services maken

1. Meld u aan bij de [portal van Azure](https://portal.azure.com).
2. Klik op **nieuwe** > **Management** > **herstel Services**. U kunt ook klikken op **Bladeren** > **Herstel Services** kluizen > **toevoegen**.

    ![Nieuwe kluis](./media/site-recovery-vmm-to-azure/new-vault3.png)

3. Geef in het vak **naam**een beschrijvende naam voor de kluis. Als u meer dan één abonnement hebt, selecteert u een van deze.
4. [Een resourcegroep maken](../resource-group-template-deploy-portal.md), of Selecteer een bestaande eigenschap. Geef een Azure regio. Machines worden gerepliceerd naar dit gebied. Ondersteunde regio's Zie geografische beschikbaar is in [Azure Site herstel prijzen Details](https://azure.microsoft.com/pricing/details/site-recovery/) controleren
4. Als u snel toegang tot de kluis vanuit het Dashboard wilt, klikt u op **vastmaken aan dashboard** > **maken kluis**.

    ![Nieuwe kluis](./media/site-recovery-vmm-to-azure/new-vault-settings.png)

De nieuwe kluis wordt weergegeven op het **Dashboard** > **alle resources**, en klik op het hoofdweergavegebied **herstel Services kluizen** blade.

## <a name="getting-started"></a>Aan de slag

Herstel van de site biedt een aan de slag ervaring waarmee u zo snel mogelijk implementeren. Aan de slag controleert de vereisten en begeleidt u bij de Site herstel implementatie in de juiste volgorde stappen.

In aan de slag selecteert u het type van machines die u wilt repliceren en waar u wilt repliceren naar. U instellen servers van de on-premises implementatie, Azure opslag accounts en netwerken. Replicatie-beleid maken en capaciteitsplanning uitvoert. Nadat u hebt ingesteld met de infrastructuur van uw schakelt u replicatie voor VMs. U kunt uitvoeren failovers voor specifieke machines of maak herstel abonnementen op meerdere computers is mislukt.

Begint aan de slag door te kiezen hoe u wilt implementeren sites worden hersteld. De stroom aan de slag verandert enigszins afhankelijk van uw replicatievereisten voor.



## <a name="step-1-choose-your-protection-goals"></a>Stap 1: Uw doelstellingen beveiliging kiezen

Selecteer wat u wilt repliceren en waar u wilt repliceren naar.

1. Klik in het blad **herstel Services kluizen** uw kluis en klik op **Instellingen**.
2. Klik op **Site-herstel**in **Aan de slag**  > **stap 1: voorbereiden infrastructuur** > **beveiliging doel**.

    ![Kies doelstellingen](./media/site-recovery-vmm-to-azure/choose-goals.png)

3. Selecteer **Naar Azure**in **beveiliging doel** en selecteer **Ja, met Hyper-V**. Selecteer **Ja** om te bevestigen dat u VMM gebruikt voor het beheren van Hyper-V hosts en de herstel-site. Klik vervolgens op **OK**.

    ![Kies doelstellingen](./media/site-recovery-vmm-to-azure/choose-goals2.png)



## <a name="step-2-set-up-the-source-environment"></a>Stap 2: De bronomgeving configureren

De Provider herstel van Azure-Site op de server VMM installeren en registreren van de server in de kluis. Het installeren van de Azure herstel Services-agent op Hyper-V hosts.

1. Klik op **stap 2: voorbereiden infrastructuur** > **bron**.

    ![Bron instellen](./media/site-recovery-vmm-to-azure/set-source1.png)

2. Klik op **+ VMM** als u wilt toevoegen van een server VMM in **voorbereiden bron** .

    ![Bron instellen](./media/site-recovery-vmm-to-azure/set-source2.png)

3. In de **Server toevoegen** blade controleren dat **System Center VMM server** wordt weergegeven in **servertype** en dat de server VMM voldoet aan de [vereisten en URL vereisten](#on-premises-prerequisites).
4. Download het installatiebestand Azure Site herstel-Provider.
5. Download de registratie-toets. U moet dit wanneer u het installatieprogramma uitvoert. De sleutel is geldig vijf dagen nadat u deze hebt gegenereerd.

    ![Bron instellen](./media/site-recovery-vmm-to-azure/set-source3.png)

6. Installeer de Provider herstel van Azure-Site op de server VMM.


### <a name="set-up-the-azure-site-recovery-provider"></a>De Provider herstel van Azure-Site instellen

1.  Voer de Provider installatiebestand.
2. In de **Microsoft Update** kunt u ervoor kiezen op updates zodat Provider updates worden geïnstalleerd overeenkomstig uw beleid van Microsoft Update.
3. Accepteren of wijzig de standaardlocatie voor de installatie van Provider in **installatie**, en klikt u op **installeren**.

    ![Locatie installeren](./media/site-recovery-vmm-to-azure/provider2.png)

4. Wanneer de installatie is voltooid klikt u op **registreren** om te registreren van de server VMM in de kluis.
5. De pagina **Kluis instellingen** , klik op **Bladeren** om de belangrijkste kluis-bestand te selecteren. Geef het abonnement herstel van Azure-Site en de naam van de kluis.

    ![Serverregistratie](./media/site-recovery-vmm-to-azure/provider10.PNG)

6. Klik bij **Internetverbinding**opgeven hoe de Provider die wordt uitgevoerd op de server VMM maakt verbinding met Site-herstel via internet.

    - Als u de Provider om rechtstreeks verbinding te schakelt u **rechtstreeks verbinding maken met Azure Site herstel zonder proxy**.
    - Als uw bestaande proxy is verificatie vereist of als u wilt gebruiken van een aangepaste proxy Selecteer **verbinding maken met Azure Site herstel een proxyserver gebruikt**.
    - Als u een aangepaste proxy gebruikt, geeft u het adres, poorten en referenties.
    - Als u een proxyserver gebruikt hebt u al mogen de URL's die worden beschreven in [vereisten](#on-premises-prerequisites).
    - Als u een aangepaste proxy die een VMM RunAs-account (DRAProxyAccount) wordt gemaakt automatisch met de opgegeven proxyreferenties gebruiken. De proxyserver zodanig configureren dat dit account succes kan verifiëren. De accountinstellingen VMM RunAs kunnen worden gewijzigd in de VMM-console. Vouw in de **Instellingen voor** **beveiliging** > **Uitvoeren als Accounts**en wijzigt u het wachtwoord voor DRAProxyAccount. U moet de VMM-service opnieuw starten zodat deze instelling wordt pas van kracht.

    ![Internet](./media/site-recovery-vmm-to-azure/provider13.PNG)

7. Accepteer of wijzig de locatie van een SSL-certificaat dat automatisch wordt gegenereerd om gegevens te coderen. Dit certificaat is gebruikt als u gegevensversleuteling voor een wolk die zijn beveiligd met Azure in de portal-Site herstel van Azure inschakelt. Dit certificaat beveiligen. Wanneer u een failover naar Azure uitvoert moet u deze wilt versleutelen, als gegevensversleuteling is ingeschakeld.


8. Geef in het vak **servernaam**een beschrijvende naam voor de server VMM in de kluis. Geef de naam van het VMM cluster rol in een cluster configureren.
9. **Synchronisatie cloud metagegevens** inschakelen als u wilt synchroniseren van metagegevens voor alle wolken op de server VMM met de kluis. Deze actie moet slechts één keer aan elke server worden gewerkt. Als u niet dat alle wolken synchroniseren wilt, kunt u deze instelling uitgeschakeld laat en elke cloud afzonderlijk in de cloud-eigenschappen in de console VMM synchroniseren. Klik op **registreren** om het proces te voltooien.

    ![Serverregistratie](./media/site-recovery-vmm-to-azure/provider16.PNG)

10. Registratie wordt gestart. Nadat de registratie is voltooid, de server wordt weergegeven op de **Instellingen** > blade**Servers** in de kluis.


#### <a name="command-line-installation-for-the-azure-site-recovery-provider"></a>Opdrachtregel-installatie oplossen voor de Provider herstel van Azure-Site

De Provider herstel van Azure Site kan worden geïnstalleerd vanaf de opdrachtregel. Deze methode kan worden gebruikt voor het installeren van de Provider op de Server Core voor Windows Server 2012 R2.

1. Download de sleutel Provider installatie en registratie naar een map. Bijvoorbeeld: C:\ASR.
2. Vanuit een opdrachtprompt met verhoogde bevoegdheid deze opdrachten kunt ophalen van het installatieprogramma van Provider uitvoeren:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Deze opdracht voor het installeren van de onderdelen uitvoeren:

            C:\ASR> setupdr.exe /i

4. Voer deze opdrachten de server registreren in de kluis:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Waarbij geldt:

- **/Credentials**: verplichte parameter die aangeeft waar de registratie belangrijke bestand zich bevindt.  
- **/FriendlyName**: parameter is vereist voor de naam van de Hyper-V host-server die wordt weergegeven in de portal Azure sites worden hersteld.
- - **/EncryptionEnabled**: optionele parameter wanneer u bent Hyper-V VMs in VMM repliceren wolken naar Azure. Geef als u wilt versleutelen virtuele machines in Azure (bij rest-codering). Zorg ervoor dat de naam van het bestand een **.pfx** -extensie heeft. Versleuteling is standaard uitgeschakeld.
- **/proxyAddress**: optionele parameter die het adres van de proxyserver.
- **/proxyport**: optionele parameter die de poort van de proxyserver.
- **/proxyUsername**: optionele parameter die de proxy-gebruikersnaam (als proxy is verificatie vereist).
- **/proxyPassword**: optionele parameter die het wachtwoord om te verifiëren met proxyserver (als de proxy is verificatie vereist).


### <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>Installeren van de Azure herstel Services-agent op Hyper-V hosts

1. Nadat u de Provider hebt ingesteld, moet u het installatiebestand voor de Azure herstel Services-agent downloaden. Op elke server Hyper-V in de cloud VMM setup uitvoeren.

    ![Hyper-V sites](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)

2. Klik op **volgende**op de pagina **Vereisten controleren** . Eventuele ontbrekende vereisten wordt automatisch geïnstalleerd.

    ![Vereisten voor herstel Services Agent](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)

3. Klik op de pagina **Instellingen voor de installatie** accepteren of locatie voor de installatie en locatie van de cache wijzigen. U kunt de cache configureren op een schijf waarop ten minste 5 GB opslagruimte beschikbaar, maar het is raadzaam een cache-station met 600 GB of meer van de beschikbare ruimte te. Klik vervolgens op **installeren**.
4. Nadat de installatie is voltooid, klikt u op **sluiten** om terug te voltooien.

    ![Register MARS Agent](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

#### <a name="command-line-installation-for-azure-site-recovery-services-agent"></a>Opdrachtregel-installatie oplossen voor Azure Site herstel Services-agent

U kunt de Microsoft Azure herstel Services-Agent installeren vanaf de opdrachtregel met behulp van de volgende opdracht uit:

     marsagentinstaller.exe /q /nu

#### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Internettoegang proxy instellen voor de Site herstel van Hyper-V hosts

De agent herstel Services wordt uitgevoerd op Hyper-V hosts moet om Azure toegang tot internet voor VM replicatie. Als u internet via een proxy benaderen bent, stelt u dit als volgt:

1. Open de module MMC van Microsoft Azure back-up op de Hyper-V-host. Een snelkoppeling voor back-up van Microsoft Azure is al dan niet standaard beschikbaar op het bureaublad of in C:\Program Files\Microsoft Azure herstel Services Agent\bin\wabadmin.
2. Klik op **Eigenschappen wijzigen**in de module.
3. Geef op het tabblad **Configuratie van Proxy** proxy-servergegevens.

    ![Register MARS Agent](./media/site-recovery-vmm-to-azure/mars-proxy.png)

4. Zorg ervoor dat de agent de URL's die worden beschreven in de [vereisten](#on-premises-prerequisites)kunt bereiken.


## <a name="step-3-set-up-the-target-environment"></a>Stap 3: De doelomgeving configureren

Geef de opslag van Azure-account moet worden gebruikt voor replicatie en het Azure netwerk waarmee Azure VMs verbinding maakt na failover.

1.  Klik op **de infrastructuur voorbereiden** > **doeltoepassing** en selecteer het Azure abonnement dat u wilt gebruiken.
2.  Geef de implementatiemodel dat u wilt gebruiken voor VMs na failover.
3.  Herstel van de site wordt gecontroleerd dat er een of meer compatibele Azure opslag accounts en netwerken.

    ![Opslag](./media/site-recovery-vmm-to-azure/compatible-storage.png)

4.  Als u een account opslag nog niet hebt gemaakt en u wilt maken van een gebruikt resourcemanager, klikt u op **+ opslag account** moet die inline.  Geef een accountnaam, type, abonnement en locatie op het blad **opslag-account maken** . Het account moet zich in dezelfde locatie als het herstelproces is Services kluis.

    ![Opslag](./media/site-recovery-vmm-to-azure/gs-createstorage.png)

    Houd rekening met:

    - Als u een opslag account maken met het klassieke model wilt, kunt u dat doen in de portal van Azure. [Meer informatie](../storage/storage-create-storage-account-classic-portal.md)
    - Als u een premium-opslag-account voor gerepliceerde gegevens gebruikt, moet u een extra opslagruimte van de standaard-mailaccount instellen voor de opslag van replicatie-logboeken die lopende wijzigingen in de on-premises gegevens vastleggen.

4.  Als u een Azure-netwerk nog niet hebt gemaakt en u wilt maken van een Resource Manager gebruiken Klik op **+ netwerk** moet die inline. Geef de netwerknaam van een, adresbereiken subnet details, abonnement en locatie op het blad **virtueel netwerk maken** . Het netwerk moet op dezelfde locatie als het herstelproces is Services kluis.

    ![Netwerk](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

    Als u wilt maken van een netwerk met behulp van het klassieke model kunt u dit doen in de portal van Azure. [Meer informatie](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Netwerk toewijzing configureren

- [Alleen](#prepare-for-network-mapping) een beknopt overzicht van welke netwerk-toewijzing bevat. [Lees dit](site-recovery-network-mapping.md) voor een uitgebreidere uitleg.
- Controleer of dat virtuele machines op de server VMM zijn verbonden met een netwerk VM en of u ten minste één Azure virtuele netwerk hebt gemaakt. Meerdere VM-netwerken kunnen worden toegewezen aan één Azure netwerk.

Configureer toewijzing als volgt:

1. In **Instellingen** > **Site herstel infrastructuur** > **netwerk toewijzingen** > **Netwerk toewijzen**, klikt u op het pictogram **+ netwerk toewijzen** .

    ![Netwerk toewijzing](./media/site-recovery-vmm-to-azure/network-mapping1.png)

2. Selecteer op de **toewijzing van netwerk toevoegen** de bronserver VMM en **Azure** als het doel.
3. Controleer of het abonnement en het implementatiemodel na failover.
4. Selecteer in de **bron-netwerk**, de bron on-premises VM netwerk die u wilt toewijzen in de lijst die is gekoppeld aan de VMM-server.
5. Selecteer in de **doel-netwerk**, het Azure netwerk in welke replica Azure VMs zich bevinden wanneer ze zijn gemaakt. Klik vervolgens op **OK**.

    ![Netwerk toewijzing](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Hier ziet u wat gebeurt er wanneer netwerk toewijzing begint:

- Bestaande VMs in een netwerk met de bron VM zijn verbonden met het netwerk wanneer toewijzing begint. Nieuwe VMs verbonden met het netwerk van de VM bron zijn verbonden met het netwerk voor toegewezen Azure terwijl de replicatie plaatsvindt.
- Als u een bestaande netwerk-toewijzing wijzigt, wordt replica virtuele machines worden verbonden met de nieuwe instellingen.
- Als het doelnetwerk meerdere subnetten heeft en een van deze subnetten heeft dezelfde naam als subnet waarop de bron virtuele machine zich bevindt, worden klikt u vervolgens de replica virtuele machine verbonden met dat doel subnet na failover.
- Als er geen subnet doellijst met een overeenkomende naam, wordt de virtuele machine niet worden verbonden met het eerste subnet in het netwerk.



## <a name="step-4-set-up-replication-settings"></a>Stap 4: Herhaling instellingen instellen


1. Als u wilt een nieuwe replicatiebeleid maken, klikt u op **voorbereiden infrastructuur** > **Replicatie-instellingen** > **+ maken en koppelen**.

    ![Netwerk](./media/site-recovery-vmm-to-azure/gs-replication.png)

2. Geef in het **maken en koppelen beleid**, de naam van een beleid.
3. Geef in het **interval kopiëren**, hoe vaak u wilt repliceren delta gegevens na de eerste replicatie (elke 30 seconden, 5 of 15 minuten).
4. Geef in **herstel wijst u bewaarbeleid**, op binnen enkele uren hoe lang het venster bewaarbeleid voor elk opsommingsteken herstel. Beveiligde machines kunnen herstellen naar een willekeurige plaats in een venster.
6. In de **App-consistente momentopname frequentie**, Geef op hoe vaak (1-12 uur) herstel punten die van toepassing-consistente momentopnamen gemaakt. Hyper-V gebruikt twee typen momentopnamen, een momentopname van een standaard waarmee een incrementele momentopname van de hele virtuele machine en een momentopname van een toepassing consistente dat een momentopname van het punt-in-tijd van de toepassingsgegevens in de virtuele machine. Volume schaduw Copy Service (VSS) toepassing consistente momentopnamen gebruiken om ervoor te zorgen dat de toepassingen in een consistente status zijn wanneer de momentopname wordt gemaakt. Houd er rekening mee dat als u de toepassing consistente momentopnamen inschakelt, dit invloed is op de prestaties van toepassingen op bron virtuele machines. Zorg ervoor dat de ingestelde waarde kleiner is dan het aantal aanvullende herstel punten die u configureren.
3. Aangeven in **aanvankelijke herhaling begintijd**, wanneer u de eerste replicatie starten. De replicatie vindt plaats via de bandbreedte van uw internet, zodat u kunt deze buiten uw bezet uren plannen.
5. Geef op of moet worden gecodeerd op andere gegevens in Azure opslag in **versleutelen-gegevens die zijn opgeslagen op Azure**. Klik vervolgens op **OK**.

    ![Replicatiebeleid](./media/site-recovery-vmm-to-azure/gs-replication2.png)

6. Wanneer u een nieuw beleid maakt, is deze automatisch gekoppeld aan de cloud VMM. Klik op **OK**. U kunt extra VMM wolken (en de VMs erin) koppelen aan dit beleid herhaling in **Instellingen** > **herhaling** > beleidsnaam > **VMM Cloud koppelen**.

    ![Replicatiebeleid](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="step-5-capacity-planning"></a>Stap 5: Capaciteit plannen

Nu dat u uw basic hebt infrastructuur instellen u kunt denken over het plannen van capaciteit en bepalen of u aanvullende informatie nodig hebt.

Herstel van de site biedt een planner capaciteit waarmee u de juiste resources toewijzen voor uw bronomgeving, de site herstel onderdelen, netwerken en opslag. U kunt de planner uitvoeren in de snelle modus voor schattingen op basis van een gemiddelde aantal VMs, schijven en opslag of in de gedetailleerde weergave waarin u afbeeldingen op het niveau van de werkbelasting kunt invoert. Voordat u begint moet u:

- Verzamel informatie over uw replicatieomgeving, inclusief VMs, schijven per VMs en opslagruimte per schijf.
- Schatting van de dagelijkse wijzigen (lospeuteren)-mate dat u voor gerepliceerde gegevens hebt. U kunt de [capaciteit planner voor Hyper-V Replica](https://www.microsoft.com/download/details.aspx?id=39057) gebruiken om te helpen u dit kunt doen.

1.  Klik op **downloaden** om te downloaden van het hulpmiddel en voer dit uit. [Lees het artikel](site-recovery-capacity-planner.md) waarop het hulpmiddel.
2.  Wanneer u klaar bent Selecteer **Ja** in het **hebt u de capaciteit Planner uitvoeren**?

    ![Capaciteit plannen](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Aandachtspunten voor de bandbreedte netwerk

U kunt de capaciteit teamplanner hulpmiddel voor het berekenen van de bandbreedte die u nodig voor replicatie (eerste replicatie en klik vervolgens delta hebt). Als u wilt bepalen welke bandbreedtegebruik voor replicatie hebt u een paar opties:

- **De bandbreedte**: Hyper-V-verkeer die wordt overgenomen door een secundaire site doorloopt in een specifieke Hyper-V-host. U kunt de bandbreedte op de hostserver beperken.
- **Bandbreedte aanpassen**: kunt u de bandbreedte gebruikt voor replicatie met een paar registersleutels beïnvloeden.

#### <a name="throttle-bandwidth"></a>De bandbreedte

1. Open de module MMC van Microsoft Azure back-up op de server van Hyper-V host. Een snelkoppeling voor back-up van Microsoft Azure is al dan niet standaard beschikbaar op het bureaublad of in C:\Program Files\Microsoft Azure herstel Services Agent\bin\wabadmin.
2. Klik op **Eigenschappen wijzigen**in de module.
3. Klik op het tabblad **Throttling** Selecteer **internetbandbreedte beperken voor back-bewerkingen inschakelen**, en de limieten voor werk instellen en niet-werk uren. Geldige bereiken zijn 512 k tot 102 Mbps per seconde.

    ![De bandbreedte](./media/site-recovery-vmm-to-azure/throttle2.png)

U kunt ook de cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) gebruiken om in te stellen beperken. Hier volgt een voorbeeld:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting-NoThrottle** wordt aangegeven dat er geen beperking vereist is.


#### <a name="influence-network-bandwidth"></a>Invloed netwerkbandbreedte

De registerwaarde **UploadThreadsPerVM** bepaalt het aantal threads die worden gebruikt voor de overdracht van gegevens (aanvankelijke of delta replicatie) van een schijf. Een hogere waarde Hiermee verhoogt u de netwerkbandbreedte gebruikt voor replicatie. De registerwaarde **DownloadThreadsPerVM** Hiermee geeft u het aantal threads gebruikt voor de overdracht van gegevens tijdens failback.

1. Ga in het register naar **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.

    - De waarde **UploadThreadsPerVM** wijzigen (of de sleutel maken als deze niet bestaat) naar besturingselement threads gebruikt voor replicatie Schijfopruiming.
    - De waarde **DownloadThreadsPerVM** wijzigen (of de sleutel maken als deze niet bestaat) naar besturingselement threads gebruikt voor failback verkeer van Azure.
2. De standaardwaarde is 4. In een netwerk 'overprovisioned' worden deze registersleutels gewijzigd van de standaardwaarden. Het maximale aantal is 32. Verkeer naar het optimaliseren van de waarde bewaken.

## <a name="step-6-enable-replication"></a>Stap 6: Inschakelen herhaling

Nu als volgt herhaling inschakelen:

1. Klik op **stap 2: toepassing repliceren** > **bron**. Wanneer u replicatie voor de eerste keer hebt ingeschakeld klik u op **+ repliceren** in de kluis replicatie voor extra machines inschakelen.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication1.png)

2. Klik in het blad **bron** > selecteert u de VMM-server en de cloud waarin de Hyper-V-hosts bevinden. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication-source.png)

3. Selecteer het abonnement, post-failover implementatiemodel en de opslag-account dat u voor gerepliceerde gegevens gebruikt in de **doellijst** .

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication-target.png)

4. Selecteer het opslag-account dat u wilt gebruiken. Als u wilt gebruiken een andere opslag-account dan die hebt u u kunt [een account maakt](#set-up-an-azure-storage-account). Klik op **Nieuw**om een opslag account met behulp van het model resourcemanager. Als u een opslag account maken met het klassieke model wilt, doet u dat [in de portal van Azure](../storage/storage-create-storage-account-classic-portal.md). Klik vervolgens op **OK**.
5. Selecteer de Azure netwerk en subnet waarmee Azure VMs verbinding maakt wanneer ze bent of omhoog na failover. Selecteer **configureren nu voor geselecteerde computers** de netwerkinstelling toepassen op alle computers die u selecteren voor de beveiliging. Selecteer **later configureren** om te selecteren van de Azure netwerk per computer. Als u wilt gebruiken van een ander netwerk van die hebt u u kunt [een account maakt](#set-up-an-azure-network). Klik op **Nieuw**om een netwerk met behulp van het model resourcemanager. Als u wilt maken van een netwerk met behulp van het klassieke model doen kunt die [in de portal van Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Selecteer een subnet, indien van toepassing. Klik vervolgens op **OK**.
6. In de **virtuele Machines** > **virtuele machines selecteren** op en selecteer elke computer die u wilt repliceren. U kunt alleen machines waarvoor replicatie kan worden ingeschakeld selecteren. Klik vervolgens op **OK**.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication5.png)

5. In de **Eigenschappen van** > **eigenschappen configureren**, selecteer het besturingssysteem voor de geselecteerde VMs en de schijf OS. Klik vervolgens op **OK**. U kunt aanvullende eigenschappen later instellen.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication6.png)


12. In de **Instellingen voor replicatie** > **replicatie-instellingen configureren**, selecteer het replicatiebeleid dat u wilt toepassen voor de beveiligde VMs. Klik vervolgens op **OK**. U kunt het beleid herhaling in **Instellingen**wijzigen > **herhaling beleidsregels** > beleidsnaam > **Instellingen bewerken**. Wijzigingen die u toepast, worden gebruikt voor machines die al zijn repliceren en nieuwe machines.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication7.png)

U kunt de voortgang van de taak **Beveiliging inschakelen** in **Instellingen**bijhouden > **taken** > **herstel van de Site taken**. Nadat de taak **Voltooien beveiliging** wordt uitgevoerd van de computer is klaar voor failover.

### <a name="view-and-manage-vm-properties"></a>Weergeven en beheren van VM eigenschappen

Het is raadzaam dat u de eigenschappen van de broncomputer controleren. Vergeet niet dat de naam van de Azure VM aan [vereisten Azure virtuele machines voldoen moet](site-recovery-best-practices.md#azure-virtual-machine-requirements).

1. Klik op **Instellingen** > **Beveiligde Items** > **Items gerepliceerd** > en selecteert u de computer om de details weer te geven.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/vm-essentials.png)

2. In de **Eigenschappen** kunt u replicatie en overname bij storing informatie voor VM weergeven.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/test-failover2.png)

3. In het **netwerk en rekenkracht** > **berekenen eigenschappen** kunt u de grootte van de naam en doelsites Azure VM opgeven. De naam om te voldoen aan [vereisten voor Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements) als u wilt wijzigen. U kunt ook weergeven en wijzigen van informatie over de doelnetwerk, subnet en IP-adres dat toegewezen aan de VM Azure. Houd rekening met het volgende:

    - U kunt het target IP-adres instellen. Als u niet een adres, de mislukte via machine opgeeft DHCP gebruikt. Als u een adres dat is niet beschikbaar bij failover hebt ingesteld, wordt de overname, mislukt. Hetzelfde target IP-adres kan worden gebruikt voor test failover als het adres in het netwerk van de failover-test beschikbaar is.
    - Het aantal netwerkadapters wordt bepaald door de grootte die u voor de doeltoepassing virtuele machine, als volgt opgeeft:

        - Als het aantal netwerkadapters op de broncomputer kleiner dan of gelijk is aan het aantal adapters toegestaan voor de grootte van de computer doel is, klikt u vervolgens heeft het doel hetzelfde aantal adapters als de bron.
        - Als het aantal adapters voor de bron virtuele machine groter is dan het getal dat is toegestaan voor de doelgrootte en de grootte van de doelsite maximale wordt gebruikt.
        - Als u een bron-machine heeft twee netwerkadapters en de grootte van de computer doel ondersteunt vier, de doelcomputer heeft twee adapters bijvoorbeeld. Als de broncomputer twee adapters heeft, maar de doelgrootte van de ondersteunde alleen een ondersteunt wordt slechts één adapter hebben op de doelcomputer.     
        - Als de VM meerdere netwerkadapters die ze alle verbinding maken met hetzelfde netwerk heeft worden.

    ![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/test-failover4.png)

5.  In **schijven** kunt u het besturingssysteem en gegevensschijven bekijken op de VM die worden gerepliceerd.



## <a name="step-7-test-your-deployment"></a>Stap 7: Test uw implementatie

U kunt een failover test voor een enkele virtuele machine of een herstel-abonnement met een of meer virtuele machines uitvoeren als u wilt testen van de implementatie.


### <a name="prepare-for-failover"></a>Voorbereiden voor failover

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

- Voeg een openbare eindpunt voor de RDP-protocol (poort 3389) en geef de referenties voor aanmelding.
- Controleer of er geen domeinbeleid dat voorkomen dat u verbinding maakt met een virtuele machine met een openbare adres.
- Maak verbinding met. Als u geen verbinding kunt maken, controleert u of de VM wordt uitgevoerd. Lees dit [artikel](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)voor meer tips.

Als u toegang wilt tot een Azure VM waarop Linux na failover Secure Shell mailclient gebruikt (ssh), doet u het volgende:

Klik **op de lokale computer voordat failover wordt uitgevoerd**:

- Zorg ervoor dat de service Secure Shell op de VM Azure is ingesteld op automatisch starten op het opstarten.
- Controleer dat firewallregels een SSH-verbinding met deze.

**Klik op het Azure VM na failover**:

- De groep beveiligingsregels voor netwerken op de mislukte via VM en het Azure subnet waaraan dit is aangesloten moeten binnenkomende verbindingen met de poort SSH toestaan.
- Een openbare eindpunt moet worden gemaakt zodat binnenkomende verbindingen op de SSH poort (TCP-poort 22 al dan niet standaard).
- Als de VM toegankelijk is via een VPN-verbinding (Express Route of site naar site VPN) kan de client rechtstreeks via verbinding maken met de VM SSH worden gebruikt.


### <a name="run-a-test-failover"></a>Een failover test uitvoeren

Ga op de toets failover voert u de volgende handelingen uit:

1. Via een enkele VM in **Instellingen**mislukt > **Gerepliceerd Items**, klikt u op de VM > **+ toets Failover**.
2. Via een abonnement herstel, klikt u in **Instellingen**mislukt > **Herstel van abonnement**, met de rechtermuisknop op het abonnement > **Test Failover**. Als u wilt maken met een herstel plannen [Volg deze instructies](site-recovery-create-recovery-plans.md).

3. Selecteer het Azure netwerk waarmee Azure VMs verbinding maken nadat storing in **Test Failover** .
4. Klik op **OK** om te beginnen met de overname. U kunt de voortgang bijhouden door te klikken op de VM om de eigenschappen te openen of op de taak **Test Failover** in **Instellingen** > **herstel van de Site taken**.
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


## <a name="monitor-your-deployment"></a>Bewaak uw implementatie

Hier ziet u hoe u de configuratie-instellingen, status en servicestatus voor de implementatie van uw Site herstel kunt controleren:

1. Klik op de naam van de kluis voor toegang tot het dashboard **Essentials** . In dit dashboard kunt u de Site herstel taken, herhaling status, herstel-abonnementen, server gezondheid en gebeurtenissen.  U kunt Essentials om weer te geven van de tegels en -indelingen die voor u zijn, waaronder de status van andere sites worden hersteld en back-up kluizen nuttigste aanpassen.

    ![Essentials](./media/site-recovery-vmm-to-azure/essentials.png)

2. U kunt siteservers (VMM of configuratie-servers) die voordoet zich probleem en de gebeurtenissen die door herstel van de Site in de afgelopen 24 uur controleren in de tegel van de **Servicestatus** .
3. U kunt beheren en herhaling in de **Items gerepliceerd**, **Herstel van abonnement**, controleren en **Site herstel taken** tegels. U kunt inzoomen op taken in **Instellingen** -> **taken** -> **Site herstel taken**.


## <a name="next-steps"></a>Volgende stappen

Nadat u hebt uw implementatie is ingesteld en uit te voeren, [voor meer informatie](site-recovery-failover.md) over de verschillende soorten failover.
