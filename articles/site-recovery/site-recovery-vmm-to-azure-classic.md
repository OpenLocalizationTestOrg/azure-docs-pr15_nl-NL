<properties
    pageTitle="Hyper-V virtuele machines in VMM wolken repliceren naar Azure | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe repliceren Hyper-V virtuele machines op Hyper-V hosts bevinden in System Center VMM wolken naar Azure."
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
    ms.topic="hero-article"
    ms.date="05/06/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure"></a>Hyper-V virtuele machines in VMM wolken repliceren naar Azure

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-vmm-to-azure.md)
- [PowerShell - resourcemanager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klassieke Portal](site-recovery-vmm-to-azure-classic.md)
- [PowerShell - klassiek](site-recovery-deploy-with-powershell.md)



De Site herstel van Azure-service draagt bij aan uw strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel (BCDR) door orchestrating herhaling, failover en herstel van virtuele machines en fysieke servers. Computers kunnen worden gerepliceerd naar Azure of naar een secundaire on-premises implementatie-Datacenter. Voor een kort overzicht lezen [Wat Azure Site herstel is?](site-recovery-overview.md).

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe Site-herstel als u wilt repliceren Hyper-V virtuele machines op Hyper-V hostservers die in VMM privé wolken naar Azure bevinden zich implementeren.

Lees het artikel bevat vereisten voor het scenario en ziet u hoe u het instellen van een Site herstel kluis, krijgen van de Azure Site herstel Provider geïnstalleerd op de bronserver VMM, de server in de kluis registreren, een Azure opslag-account toevoegen, het installeren van de Azure herstel Services-agent op Hyper-V hostservers, configureren van instellingen voor documentbeveiliging voor VMM wolken die wordt toegepast op alle beveiligde virtuele machines , en vervolgens de beveiliging voor deze virtuele machines inschakelen. Voltooien door te testen van de overname om ervoor te zorgen dat alles werkt zoals verwacht.

Opmerkingen of vragen onder van dit artikel of op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)posten.

## <a name="architecture"></a>Architectuur

![Architectuur](./media/site-recovery-vmm-to-azure-classic/topology.png)

- De Provider herstel van Azure-Site op de VMM tijdens herstel van de Site-implementatie is geïnstalleerd en de VMM-server is geregistreerd in de Site herstel kluis. De Provider communiceert met Site-herstel worden afgehandeld replicatie-integratie.
- De agent Azure herstel Services is geïnstalleerd op Hyper-V hostservers tijdens de implementatie van sites worden hersteld. Het verwerkt repliceren met Azure storage.


## <a name="azure-prerequisites"></a>Azure vereisten

Hier ziet u wat u moet in Azure wordt aangegeven.

**Vereiste** | **Meer informatie**
--- | ---
**Azure-account**| U moet een [Microsoft Azure](https://azure.microsoft.com/) -account. U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstelproces is Site prijzen.
**Azure-opslag** | U moet een Azure opslag-account voor de opslag gerepliceerde gegevens. Gerepliceerde gegevens worden opgeslagen in de opslagruimte van de Azure en Azure VMs zijn omhoog of bij een storing. <br/><br/>Hebt u een [standaard geografische-redundante opslag-account](../storage/storage-redundancy.md#geo-redundant-storage)nodig. Het account moet in hetzelfde gebied, als de Site herstel-service en zijn gekoppeld aan hetzelfde abonnement. Houd er rekening mee replicatie naar premium opslag accounts momenteel wordt niet ondersteund en niet mag worden gebruikt.<br/><br/>[Lees over](../storage/storage-introduction.md) Azure opslagruimte.
**Azure-netwerk** | U moet een Azure virtuele netwerk dat Azure VMs maakt verbinding met bij een storing. Het Azure virtuele netwerk moet zich in hetzelfde gebied, als de Site herstel kluis.

## <a name="on-premises-prerequisites"></a>Vereisten voor on-premises implementatie

Hier ziet u wat u nodig hebt on-premises implementatie.

**Vereiste** | **Meer informatie**
--- | ---
**VMM** | U moet ten minste één VMM server als een fysieke of virtuele zelfstandige server of een virtueel cluster geïmplementeerd. <br/><br/>De server VMM moet systeem Center 2012 R2 worden uitgevoerd met de meest recente cumulatieve updates.<br/><br/>U moet ten minste één cloud geconfigureerd op de server VMM.<br/><br/>De bron-cloud die u wilt beveiligen, moet een of meer VMM hostgroepen bevatten.<br/><br/>Meer informatie over het instellen van VMM wolken in [Stapsgewijze instructies: privé wolken maken met System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) in Keith Mayer van blog.
**Hyper-V** | U moet een of meer Hyper-V hostservers of clusters in de cloud VMM. De host-server nodig hebt en een of meer VMs. <br/><br/>De server Hyper-V moet worden uitgevoerd op ten minste **Windows Server 2012 R2** met de functie Hyper-V of **Microsoft Hyper-V Server 2012 R2** hebt en de meest recente updates hebt geïnstalleerd.<br/><br/>Een Hyper-V server VMs die u wilt beveiligen met moet zich bevinden in een wolk VMM.<br/><br/>Als u werkt met Hyper-V in een notitie cluster die makelaar cluster wordt niet automatisch gemaakt als u een statische IP-adres gebaseerde cluster hebt. U moet de makelaar cluster handmatig configureren. [Meer informatie](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) in het blogbericht van Aidan Finn.
**Beveiligde machines** | VMs die u wilt beveiligen, moeten voldoen aan [vereisten voor Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements).


## <a name="network-mapping-prerequisites"></a>Vereisten voor netwerk-toewijzing
Wanneer u virtuele machines in Azure netwerk toewijzing kaarten tussen VM netwerken op de bronserver VMM beveiligen en afstemmen van Azure-netwerken te gebruiken om in te schakelen van de volgende handelingen uit:

- Alle computers welke-foutherstel op hetzelfde netwerk met elkaar verbinden kunt, ongeacht welk abonnement herstel ze zijn in.
- Als een netwerkgateway ingesteld op het doel Azure netwerk is, worden virtuele machines kunt koppelen aan andere on-premises implementatie virtuele machines.
- Als u niet zo configureert netwerk toewijzing alleen virtuele machines die in hetzelfde herstelplan mislukken kunnen verbinding maken met elkaar na failover naar Azure.

Als u wilt implementeren netwerk toewijzing hebt u het volgende nodig:

- De virtuele machines die u wilt beveiligen op de bronserver VMM moeten zijn verbonden met een netwerk VM. Dit netwerk moet zijn gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud.
- Een Azure netwerk waarmee gerepliceerde virtuele machines verbinding na failover maken kunt. U kunt dit netwerk selecteren op het moment van failover. Het netwerk moet in hetzelfde gebied, als uw abonnement Azure sites worden hersteld.

Voorbereiden voor het netwerk toewijzen als volgt:

1. [Lees informatie over](site-recovery-network-mapping.md) vereisten voor toewijzing.
2. VM-netwerken te gebruiken in VMM voorbereiden:

    - [Logische netwerken instellen](https://technet.microsoft.com/library/jj721568.aspx).
    - [VM netwerken instellen](https://technet.microsoft.com/library/jj721575.aspx).


## <a name="step-1-create-a-site-recovery-vault"></a>Stap 1: Een Site herstel kluis maken

1. Meld u aan bij de [Beheerportal](https://portal.azure.com) van de VMM-server die u wilt registreren.
2. Klik op **gegevensservices** > **herstel Services** > **Site herstel kluis**.
3. Klik op **nieuwe maken** > **snel tabellen maken**.
4. Voer in het vak **naam**een beschrijvende naam voor de kluis.
5. Selecteer in de **regio**, de geografische regio voor de kluis. Ondersteunde regio's Zie geografische beschikbaar is in [Azure Site herstel prijzen Details](https://azure.microsoft.com/pricing/details/site-recovery/)controleren.
6. Klik op **maken kluis**.

    ![Nieuwe kluis](./media/site-recovery-vmm-to-azure-classic/create-vault.png)

Controleer de statusbalk om te bevestigen dat de kluis is gemaakt. De kluis worden, als **actief** vermeld op de hoofdpagina van herstel Services.

## <a name="step-2-generate-a-vault-registration-key"></a>Stap 2: Een kluis registratiegegevens sleutel genereren

Genereer een registratiegegevens-sleutel in de kluis. Nadat u de Provider van Azure Site herstel downloaden en op de server VMM installeren, gebruikt u deze toets naar de server VMM in de kluis registreren.

1. Klik in de pagina **Herstel Services** op de kluis om de pagina snel aan de slag te openen. Snel aan de slag kan ook worden geopend op elk gewenst moment met het pictogram.

    ![Snel aan de slag-pictogram](./media/site-recovery-vmm-to-azure-classic/qs-icon.png)

2. Selecteer in de vervolgkeuzelijst **tussen een on-premises VMM-site en Microsoft Azure**.
3. Klik op **genereren registratiegegevens sleutel** bestand in **VMM Servers voorbereiden**. Het belangrijkste bestand wordt automatisch gegenereerd en geldt voor 5 dagen nadat deze gegenereerd. Als u bent geen toegang heeft tot de portal van Azure van de server VMM moet u dit bestand kopiëren naar de server.

    ![Registratie-toets](./media/site-recovery-vmm-to-azure-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Stap 3: De Provider van Azure Site herstel installeren

1. **Aan**de slag > **voorbereiden VMM servers**, klikt u op **Microsoft Azure Site herstel Provider is downloaden voor installatie op VMM servers** om de meest recente versie van het installatiebestand Provider.
2. Dit bestand op de bronserver VMM uitvoeren.

    >[AZURE.NOTE] Als VMM wordt geïmplementeerd in een cluster en u bent de Provider voor de installatie is de eerste keer installeren op een actief knooppunt en klaar bent met de installatie van de server VMM in de kluis registreren. Installeer de Provider op de andere knooppunten. Houd er rekening mee dat als u een de Provider upgrade uitvoert u op alle knooppunten upgraden moet omdat moeten ze al worden uitgevoerd dezelfde versie als Provider.

3. Het installatieprogramma prerequirements controleert en vraagt machtigingen voor de service VMM om te beginnen met Provider setup stoppen. De Service VMM worden gestart wanneer setup is voltooid. Als u de installatie uitvoert op een cluster VMM wordt u gevraagd de rol Cluster stoppen.

4. In de **Microsoft Update** kunt u kiezen op updates. Met deze instelling ingeschakeld Provider updates worden geïnstalleerd op basis van uw Microsoft Update-beleid.

    ![Updates van Microsoft](./media/site-recovery-vmm-to-azure-classic/updates.png)


5.  De installatielocatie voor de Provider is ingesteld op ** <SystemDrive>\Program Files\Microsoft systeem Center 2012 R2\Virtual Machine Manager\bin**. Klik op **installeren**.

    ![InstallLocation](./media/site-recovery-vmm-to-azure-classic/install-location.png)

6. Klik op **registreren** om te registreren van de server in de kluis nadat de Provider is geïnstalleerd.

    ![InstallComplete](./media/site-recovery-vmm-to-azure-classic/install-complete.png)

9. Controleer of de naam van de kluis waarin de server worden geregistreerd in het vak **de naam van de kluis**. Klik op *volgende*.

    ![Serverregistratie](./media/site-recovery-vmm-to-azure-classic/vaultcred.PNG)

7. Geef op hoe de Provider die wordt uitgevoerd op de server VMM verbinding maakt met Internet in **Internetverbinding** . Selecteer **verbinding maken met bestaande proxy-instellingen** voor het gebruik van de standaard Internet-verbindingsinstellingen voor de server hebt geconfigureerd.

    ![Instellingen voor Internet](./media/site-recovery-vmm-to-azure-classic/proxydetails.PNG)

    - Als u wilt gebruiken van een aangepaste proxy moet u deze instellen voordat u de Provider installeren. Wanneer u aangepaste proxy-instellingen configureren een toets uitgevoerd als u wilt controleren van de proxyverbinding.
    - Als u gebruik maakt van een aangepaste proxy of uw standaardproxy u de gegevens voor de proxy moet, inclusief de proxyadres en poort Voer-verificatie is vereist.
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

    ![LastPage](./media/site-recovery-vmm-to-azure-classic/provider13.PNG)

Metagegevens van de server VMM wordt na registratie opgehaald door Azure sites worden hersteld. De server wordt weergegeven op het tabblad **VMM-Servers** op de pagina **Servers** in de kluis.

### <a name="command-line-installation"></a>Opdrachtregel installatie

De Provider van Azure Site herstel kan ook worden geïnstalleerd via de volgende opdrachtregel. Deze methode kan worden gebruikt voor het installeren van de provider op een Server Core voor Windows Server 2012 R2.

1. Download de sleutel Provider installatie en registratie naar een map. Bijvoorbeeld: C:\ASR.
2. De System Center VM Manager-service stoppen
3. Ophalen uit een opdrachtprompt met verhoogde bevoegdheid het installatieprogramma van Provider met de volgende opdrachten:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Installeer de provider als volgt:

        C:\ASR> setupdr.exe /i

5. Registreren als volgt de Provider:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Waar zijn parameters als volgt:

 - **/Credentials** : verplichte parameter die de locatie waarin het belangrijkste registratie-bestand zich bevindt  
 - **/FriendlyName** : parameter is vereist voor de naam van de Hyper-V host-server die wordt weergegeven in de portal Azure sites worden hersteld.
 - **/EncryptionEnabled** : optionele parameter om op te geven als u versleuteling uw virtuele machines in Azure wordt aangegeven (bij rest-codering wilt). De bestandsnaam moet hebben de **extensie** .
 - **/proxyAddress** : optionele parameter die het adres van de proxyserver.
 - **/proxyport** : optionele parameter die de poort van de proxyserver.
 - **/proxyUsername** : optionele parameter waarmee de naam van de gebruiker proxy.
 - **/proxyPassword** : optionele parameter die het proxywachtwoord.  


## <a name="step-4-create-an-azure-storage-account"></a>Stap 4: Een Azure opslag-account maken

1. Als u niet over een account Azure opslag klikt u op **een Azure opslag-Account toevoegen** aan een account maken.
2. Maak een account met geografische-herhaling is ingeschakeld. Er moet in hetzelfde gebied, als de Site herstel van Azure-service en zijn gekoppeld aan hetzelfde abonnement.

    ![Opslag-account](./media/site-recovery-vmm-to-azure-classic/storage.png)

> [AZURE.NOTE] [Migratie van opslag accounts](../resource-group-move-resources.md) via resourcegroepen binnen hetzelfde abonnement of via abonnementen wordt niet ondersteund voor opslag-accounts gebruikt voor het implementeren van sites worden hersteld.

## <a name="step-5-install-the-azure-recovery-services-agent"></a>Stap 5: De Agent van de Services Azure herstel installeren

Het installeren van de Azure herstel Services-agent op elke Hyper-V host-server in de cloud VMM.

1. Klik op de **Werkbalk Snel starten** > **downloaden Azure Site herstel Services Agent en installeer op hosts** voor informatie over de meest recente versie van het installatiebestand agent.

    ![Herstel Services Agent installeren](./media/site-recovery-vmm-to-azure-classic/install-agent.png)

2. Voer het installatiebestand op elke Hyper-V host-server.
3. Klik op **volgende**op de pagina **Vereisten voor controle** . Eventuele ontbrekende vereisten wordt automatisch geïnstalleerd.

    ![Vereisten voor herstel Services Agent](./media/site-recovery-vmm-to-azure-classic/agent-prereqs.png)

4. Klik op de pagina **Instellingen voor de installatie** opgeven waar u voor het installeren van de agent en selecteer de locatie van de cache waarin back-metagegevens wordt geïnstalleerd. Klik vervolgens op **installeren**.
5. Klik op **sluiten** om de wizard nadat de installatie is voltooid.

    ![Register MARS Agent](./media/site-recovery-vmm-to-azure-classic/agent-register.png)

### <a name="command-line-installation"></a>Opdrachtregel installatie

U kunt ook de Services-Agent van Microsoft Azure herstel installeren vanaf de opdrachtregel met deze opdracht:

    marsagentinstaller.exe /q /nu

## <a name="step-6-configure-cloud-protection-settings"></a>Stap 6: Configureren cloud instellingen voor documentbeveiliging

Nadat de server VMM is geregistreerd, kunt u instellingen voor documentbeveiliging cloud kunt configureren. U kunt de optie **synchroniseren cloud gegevens met de kluis** ingeschakeld wanneer u de Provider hebt geïnstalleerd, zodat alle wolken op de server VMM wordt weergegeven op het tabblad <b>Beveiligde Items</b> in de kluis.

![Gepubliceerde Cloud](./media/site-recovery-vmm-to-azure-classic/clouds-list.png)

1. Klik op **beveiliging voor VMM wolken instellen**op de pagina snel starten.
2. Klik op het tabblad **Beveiligde Items** op de cloud die u wilt configureren en Ga naar het tabblad **configuratie** .
3. Selecteer in de **doellijst** **Azure**.
4. Selecteer de opslag van Azure-account dat u voor replicatie gebruikt in **Opslag-Account** .
5. Stel de **versleutelen opgeslagen gegevens** op **uit**. Deze instelling geeft aan dat gegevens moeten worden gecodeerd gerepliceerd tussen de on-premises-site en Azure.
6. Laat de standaardinstelling in **frequentie kopiëren** . Deze waarde bepaalt hoe vaak de gegevens moeten worden gesynchroniseerd tussen de bronsite en doelsites locaties.
7. Laat de standaardinstelling in **behouden herstel punten voor**opgeven. Met een standaardwaarde van nul, wordt alleen de meest recente herstel komma voor een primaire virtuele machine opgeslagen op een replicaserver host.
8. Laat de standaardinstelling bij **frequentie van toepassing-consistente momentopnamen**. Deze waarde geeft aan hoe vaak momentopnamen maken. Volume schaduw Copy Service (VSS) momentopnamen gebruiken om ervoor te zorgen dat de toepassingen in een consistente status zijn wanneer de momentopname wordt gemaakt.  Als u een waarde instelt, zorg er dan voor dat kleiner is dan het aantal aanvullende herstel punten die u configureren.
9. Geef in het **herhaling begintijd**, wanneer aanvankelijke replicatie van gegevens naar Azure moet beginnen. De tijdzone op de server van Hyper-V host worden gebruikt. Het is raadzaam dat u de eerste replicatie na kantooruren plannen.

    ![Cloud replicatie-instellingen](./media/site-recovery-vmm-to-azure-classic/cloud-settings.png)

Nadat u de instellingen opslaan wordt een taak wordt gemaakt en klik op het tabblad **taken** kan worden gecontroleerd. Alle Hyper-V hostservers in de cloud van de bron VMM moeten worden geconfigureerd voor replicatie.

Na het opslaan kunnen cloud-instellingen op het tabblad **configureren** worden gewijzigd. Als u wilt wijzigen van de doellocatie of het doel opslag account moet u de cloud-configuratie verwijderen en klik vervolgens opnieuw configureren de cloud. Houd er rekening mee dat als u de opslag-account wijzigen dat de wijziging alleen voor virtuele machines die zijn ingeschakeld voor beveiliging gebruikt wordt nadat het opslag-account is gewijzigd. Bestaande virtuele machines worden niet gemigreerd naar het nieuwe opslag-account.

## <a name="step-7-configure-network-mapping"></a>Stap 7: Netwerk toewijzing configureren
Voordat u begint of er netwerk toewijzing virtuele machines op de bronserver VMM verbonden bent met een netwerk VM. Daarnaast een of meer Azure virtuele netwerken maken. Houd er rekening mee dat meerdere VM-netwerken kunnen worden toegewezen aan één Azure netwerk.

1. Klik op de pagina snel starten op **netwerken toewijzen**.
2. Selecteer de bronserver VMM in **bronlocatie**, klik op het tabblad **netwerken te gebruiken** . Selecteer in de **doellocatie** Azure.
3. In de **bron** -netwerken te gebruiken een lijst met VM netwerken die zijn gekoppeld aan de server VMM worden weergegeven. In de **doel** -netwerken te gebruiken worden de Azure netwerken die zijn gekoppeld aan het abonnement weergegeven.
4. Selecteer de bron VM-netwerk en klik op **Map**.
5. Selecteer op de pagina **Selecteer een netwerk met een doel** het doel Azure netwerk dat u wilt gebruiken.
6. Klik op het vinkje om het toewijzingsproces te voltooien.

    ![Cloud replicatie-instellingen](./media/site-recovery-vmm-to-azure-classic/map-networks.png)

Nadat u de instellingen opslaan een taak voor het bijhouden van de voortgang van de toewijzing begint en deze kan worden gecontroleerd op het tabblad taken. Een bestaande replica virtuele machines die met het netwerk van de VM bron overeenkomen worden verbonden met het doel Azure netwerken te gebruiken. Nieuwe virtuele machines die met het netwerk van de VM bron verbonden zijn worden verbonden met het netwerk voor toegewezen Azure na herhaling. Als u een bestaande toewijzing met een nieuw netwerk wijzigt, wordt replica virtuele machines worden verbonden met de nieuwe instellingen.

Houd er rekening mee dat als het doelnetwerk meerdere subnetten heeft en een van deze subnetten dezelfde naam als subnet waarop de bron virtuele machine zich bevindt heeft, en u vervolgens de replica virtuele machine worden verbonden met dat doel subnet na failover. Als er geen subnet doellijst met een overeenkomende naam, wordt de virtuele machine niet worden verbonden met het eerste subnet in het netwerk.

> [AZURE.NOTE] [Migratie van netwerken](../resource-group-move-resources.md) via resourcegroepen binnen hetzelfde abonnement of via abonnementen wordt niet ondersteund voor netwerken die zijn gebruikt voor het implementeren van sites worden hersteld.

## <a name="step-8-enable-protection-for-virtual-machines"></a>Stap 8: Beveiliging voor virtuele machines inschakelen

Nadat de servers, wolken en netwerken correct zijn geconfigureerd, kunt u beveiliging voor virtuele machines in de cloud. Houd rekening met het volgende:

- Virtuele machines moet voldoen aan [vereisten voor Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements).
- Beveiliging kunnen besturingssysteem en besturingssysteem moeten schijfeigenschappen worden ingesteld voor de virtuele machine. Wanneer u een virtuele machine maakt in VMM met een sjabloon VM kunt u de eigenschap instellen. U kunt ook deze eigenschappen voor bestaande virtuele machines instellen op de tabbladen **Algemeen** en **Hardwareconfiguratie** VM eigenschappen. Als u niet deze eigenschappen in VMM instellen kunt u zult kunt u deze in de portal-Site herstel van Azure configureren.

    ![Virtuele machine maken](./media/site-recovery-vmm-to-azure-classic/enable-new.png)

    ![VM eigenschappen wijzigen](./media/site-recovery-vmm-to-azure-classic/enable-existing.png)


1. Als u wilt inschakelen, beveiliging, op het tabblad **virtuele Machines** in de cloud waarin de virtuele machine zich bevindt, klikt u op **Beveiliging inschakelen** > **virtuele machines toevoegen**.
2. Selecteer in de lijst met virtuele machines in de cloud, de sectie die u wilt beveiligen.

    ![VM beveiliging inschakelen](./media/site-recovery-vmm-to-azure-classic/select-vm.png)

    Voortgang van de actie **Beveiliging inschakelen** in het tabblad **taken** , met inbegrip van de eerste replicatie bijhouden. Nadat de taak **Voltooien beveiliging** wordt uitgevoerd is de virtuele machine gereed voor failover. Nadat de beveiliging is ingeschakeld en virtuele machines worden gerepliceerd, kunt u zult kunnen zien ze in Azure wordt aangegeven.


    ![VM beveiliging taak](./media/site-recovery-vmm-to-azure-classic/vm-jobs.png)

3. Controleer of de eigenschappen van het virtuele machine en wijzigen zoals vereist.

    ![Controleer of virtuele machines](./media/site-recovery-vmm-to-azure-classic/vm-properties.png)


4. Klik op het tabblad **configureren** van de eigenschappen van het virtuele machine kunnen volgende netwerkeigenschappen worden gewijzigd.





- **Aantal netwerkadapters op de doelsite virtuele machine** - het aantal netwerkadapters wordt bepaald door de grootte die u voor de doeltoepassing virtuele machine opgeeft. [VM grootte specificaties](../virtual-machines/virtual-machines-linux-sizes.md#size-tables) voor het aantal adapters worden ondersteund door de grootte VM controleren. Wanneer u de grootte voor een virtuele machine wijzigen en de instellingen opslaat, wordt het aantal netwerkadapter wordt gewijzigd wanneer u pagina **configureren** de volgende keer opent. Het aantal netwerkadapters van target virtuele machines is het minimum aantal netwerkadapters op bron virtuele machine en het maximum aantal netwerkadapters worden ondersteund door de grootte van de virtuele machine gekozen, als volgt:

    - Als het aantal netwerkadapters op de broncomputer kleiner dan of gelijk is aan het aantal adapters toegestaan voor de grootte van de computer doel is, klikt u vervolgens heeft het doel hetzelfde aantal adapters als de bron.
    - Als het aantal adapters voor de bron virtuele machine groter is dan het getal dat is toegestaan voor de doelgrootte en vervolgens de doeltoepassing maximaal wordt gebruikt.
    - Als u een bron-machine heeft twee netwerkadapters en de grootte van de computer doel ondersteunt vier, wordt de doelcomputer bijvoorbeeld twee adapters hebben. Als de broncomputer twee adapters heeft, maar de doelgrootte van de ondersteunde alleen een ondersteunt wordt slechts één adapter hebben op de doelcomputer.    

- **Netwerk van de doelsite virtuele machine** - met het netwerk waarnaar de virtuele machine maakt verbinding met wordt bepaald door netwerk toewijzing van het netwerk van bron virtuele machine. Als de bron virtuele machine meer dan één netwerkadapter heeft en bron netwerken zijn toegewezen aan verschillende netwerken op doel, klik moet u kiezen tussen een van de doel-netwerken te gebruiken.
- **Subnet van elke netwerkadapter** - voor elk netwerkadapter kunt u het subnet waaraan de mislukte via virtuele machine wilt verbinden.
- **Target IP-adres** - als de netwerkadapter van bron virtuele machine is geconfigureerd voor gebruik van een statische IP-adres en vervolgens u het IP-adres voor de doeltoepassing virtuele machine opgeeft kunt. Deze functie gebruiken het IP-adres van een virtuele machine van bron behouden na een failover. Als u geen IP-adres is opgegeven wordt vervolgens alle beschikbare IP-adressen uitgedrukt tot de netwerkadapter op het moment van failover. Als het target IP-adres is opgegeven, maar wordt al gebruikt door een andere virtuele machine uitgevoerd in Azure mislukt failover.  

    ![Netwerkeigenschappen wijzigen](./media/site-recovery-vmm-to-azure-classic/multi-nic.png)

>[AZURE.NOTE] Linux virtuele machines met statische IP-adres worden niet ondersteund.

## <a name="test-the-deployment"></a>De implementatie testen

Test uw implementatie kunt u uitvoeren van een failover test voor een enkele virtuele machine, of een plan voor herstel die bestaat uit meerdere virtuele machines maken en uitvoeren van een failover test voor het abonnement.  

Test failover simuleert uw failover- en herstelbestanden om in een geïsoleerd netwerk. Houd rekening met:

- Als u verbinden met de virtuele machine in Azure wordt aangegeven met extern bureaublad na de overname wilt, moet u verbinding met extern bureaublad op de virtuele machine inschakelen voordat u de overname test uitvoeren.
- Na failover gebruikt u een openbare IP-adres verbinding maken met de virtuele machine in Azure wordt aangegeven met extern bureaublad. Als u dit doen wilt, moet u zorgen er geen domeinbeleid dat voorkomen dat u verbinding maakt met een virtuele machine met een openbare adres.

>[AZURE.NOTE] Als u de beste prestaties wanneer u een failover in Azure uitvoeren, moet u ervoor zorgen dat u de Azure-Agent in de beveiligde computer hebt geïnstalleerd. Hiermee kunt u in het opstarten sneller en kunt u er ook in diagnose in geval van problemen. Linux-agent kan worden gevonden [hier](https://github.com/Azure/WALinuxAgent) - en Windows agent vindt u [hier](http://go.microsoft.com/fwlink/?LinkID=394789)

### <a name="create-a-recovery-plan"></a>Maak een herstelplan

1. Voeg een nieuw abonnement op het tabblad **Herstel van abonnement** . Geef een naam, **VMM** in de **gegevensbron van het type**, en wordt de bronserver VMM in **bron**, het doel Azure.

    ![Herstel planning maken](./media/site-recovery-vmm-to-azure-classic/recovery-plan1.png)

2. Selecteer in de pagina **Selecteer virtuele Machines** virtuele machines toevoegen aan het abonnement herstel. Deze virtuele machines worden toegevoegd aan de standaardgroep voor herstel-abonnement, groep 1. Maximaal 100 virtuele machines in een plan voor één herstel is getest.

- Als u wilt controleren of de eigenschappen van het virtuele machine voordat u deze toevoegt aan het abonnement, klikt u op de virtuele machine op de eigenschappenpagina van de cloud waarin bevindt zich bevindt. U kunt ook de eigenschappen van het virtuele machine configureren in de VMM-console.
- Alle van de virtuele machines die worden weergegeven is voor de beveiliging ingeschakeld. De lijst bevat beide virtuele machines die zijn ingeschakeld voor beveiliging en eerste replicatie is voltooid en die zijn ingeschakeld voor de beveiliging met aanvankelijke herhaling in behandeling. Alleen virtuele machines met eerste replicatie voltooid kan worden uitgevoerd als onderdeel van een abonnement herstel.

    ![Herstel planning maken](./media/site-recovery-vmm-to-azure-classic/select-rp.png)

Nadat een herstel-abonnement is gemaakt deze weergegeven in het tabblad **Herstel van abonnement** . U kunt ook [Azure automatisering runbooks](site-recovery-runbook-automation.md) toevoegen aan het abonnement herstel acties tijdens een overname automatiseren.

### <a name="run-a-test-failover"></a>Een failover test uitvoeren

Er zijn twee manieren een failover test uitvoeren naar Azure.

- **Failover zonder een Azure netwerk testen**, dit type test failover Hiermee wordt gecontroleerd of de virtuele machine correct in Azure wordt aangegeven verschijnt. De virtuele machine wordt niet aangesloten op een Azure netwerk na failover.
- **Failover met een Azure netwerk testen**, dit type failover controles die de hele replicatieomgeving afgerond zoals verwacht wordt geleverd en die niet via de virtuele machines worden verbonden met het opgegeven doel Azure netwerk. Voor het verwerken van subnet, voor test wordt failover het subnet van de test virtuele machine worden kunt uitvoeren op basis van het subnet van de replica virtuele machine. Dit is verschillende naar gewone herhaling wanneer het subnet van een replica virtuele machine is gebaseerd op het subnet van de bron virtuele machine.

Als u uitvoeren van een failover test voor een virtuele machine ingeschakeld voor beveiliging Azure zonder op te geven van een Azure doelnetwerk dat u niet nodig hebt wilt voor het voorbereiden van alles. Naar een failover test uitvoeren met een doel Azure netwerk, u een nieuw Azure netwerk die heeft geïsoleerd maken vanuit het Azure productienetwerk (zich standaard gedraagt moet wanneer u een nieuw netwerk in Azure maken). Bekijk [een failover test uitvoeren](site-recovery-failover.md#run-a-test-failover) voor meer informatie.


U moet ook de infrastructuur voor de gerepliceerde virtuele machine werkt zoals verwacht instellen. Bijvoorbeeld een virtuele machine met domeincontroller en DNS kunnen worden gerepliceerd naar Azure met Azure Site herstel en kan worden gemaakt in de testnetwerk door gebruik te testen Failover. Kijk naar de sectie [testen failover overwegingen voor active directory](site-recovery-active-directory.md#considerations-for-test-failover) voor meer informatie.

Een test failover do voert u de volgende handelingen uit:

1. Klik op het tabblad **Herstel van abonnement** Selecteer het abonnement en klik op de **Toets Failover**.
2. Selecteer op de pagina **Bevestigen Test Failover** **geen** of een specifieke Azure netwerk.  Houd er rekening mee dat als u geen de overname test moeten worden gecontroleerd of de virtuele machine correct gerepliceerd naar selecteert Azure maar wordt niet gecontroleerd door uw netwerkconfiguratie herhaling.

    ![Geen netwerk](./media/site-recovery-vmm-to-azure-classic/test-no-network.png)

3. Als gegevensversleuteling is ingeschakeld voor de cloud, schakelt u het certificaat dat is uitgegeven tijdens de installatie van de Provider op de server VMM, wanneer u de optie om weeknummers gegevenscodering voor een wolk ingeschakeld in **Versleuteling-toets** .
4. Klik op het tabblad **taken** kunt u failover voortgang bijhouden. U moet ook mogelijk om de VM test replica in de portal van Azure weer te geven. Als u ingesteld access virtuele machines uit uw on-premises netwerk bent kunt u een verbinding met extern bureaublad naar de virtuele machine initiëren.
5. Wanneer de overname de fase **voltooid testen bereikt** , klikt u op **Voltooid testen** om te voltooien van de overname test. U kunt inzoomen op het tabblad **taak** voor het bijhouden van failover voortgang en status en om uit te voeren acties die u nodig hebt.
6. Na failover zult u zien van de replica VM testen in de portal van Azure. Als u ingesteld access virtuele machines uit uw on-premises netwerk bent kunt u een verbinding met extern bureaublad naar de virtuele machine initiëren. Ga als volgt:

    1. Controleer of de virtuele machines is gestart.
    2. Als u verbinden met de virtuele machine in Azure wordt aangegeven met extern bureaublad na de overname wilt, moet u verbinding met extern bureaublad op de virtuele machine inschakelen voordat u de overname test uitvoeren. U moet ook een RDP-eindpunt toevoegen op de virtuele machine. U kunt gebruikmaken van een [Azure automatisering Runbooks](site-recovery-runbook-automation.md) om dat te doen.
    3. Nadat failover wanneer u een openbare IP-adres gebruiken om te verbinden met de virtuele machine in Azure wordt aangegeven met extern bureaublad, zorg er dan hebt u geen domeinbeleid dat voorkomen dat u verbinding maakt met een virtuele machine met een openbare adres.

7.  Ga als volgt te werk nadat de test voltooid is:
    - Klik op **de toets overname is voltooid**. De testomgeving automatisch uit te schakelen en de test virtuele machines verwijderen opschonen.
    - Klik op **notities** als u wilt opnemen en sla eventuele opmerkingen die zijn gekoppeld aan de overname test.

>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [het instellen van herstel-abonnementen](site-recovery-create-recovery-plans.md) en [overname bij storing](site-recovery-failover.md).
