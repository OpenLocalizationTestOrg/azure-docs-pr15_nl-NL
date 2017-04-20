<properties
    pageTitle="Hyper-V virtuele machines in VMM wolken repliceren naar de site van een secundaire VMM | Microsoft Azure"
    description="In dit artikel wordt beschreven hoe Hyper-V VMs in VMM wolken repliceren met een secundaire VMM-site met Azure sites worden hersteld."
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

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site"></a>Hyper-V virtuele machines in VMM wolken repliceren naar een secundaire VMM-site

> [AZURE.SELECTOR]
- [Azure-portal](site-recovery-vmm-to-vmm.md)
- [Klassieke portal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShell - resourcemanager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

De Site herstel van Azure-service draagt bij aan uw strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel (BCDR) door orchestrating herhaling, failover en herstel van virtuele machines en fysieke servers. Computers kunnen worden gerepliceerd naar Azure of naar een secundaire on-premises implementatie-Datacenter. Voor een kort overzicht lezen [Wat Azure Site herstel is?](site-recovery-overview.md)

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe repliceren Hyper-V virtuele machines op Hyper-V hostservers die worden beheerd in VMM wolken bij secundaire VMM-site met van Azure sites worden hersteld.

Het artikel vereisten bevat, ziet u hoe u een Site herstel kluis instellen, de Provider van Azure Site herstel installeren op bron en doelgerichte VMM servers de servers registreren in de kluis, configureren van instellingen voor documentbeveiliging voor VMM wolken en vervolgens de beveiliging voor Hyper-V VMs inschakelen. Voltooien door te testen van de overname om ervoor te zorgen dat alles werkt zoals verwacht.

Opmerkingen of vragen onder van dit artikel of op het [Forum van Azure herstel Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)posten.

## <a name="architecture"></a>Architectuur

De onderstaande afbeelding ziet u de andere communicatiekanalen en poorten die door Azure Site herstel voor integratie en herhaling

![Topologie van E2E](./media/site-recovery-vmm-to-vmm-classic/e2e-topology.png)

## <a name="before-you-start"></a>Voordat u begint

Controleer of dat u deze vereisten hebt geplaatst:

**Vereisten voor** | **Meer informatie**
--- | ---
**Azure**| Hebt u een [Microsoft Azure](https://azure.microsoft.com/) -account nodig. U kunt beginnen met een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over het herstelproces is Site prijzen.
**VMM** | U moet ten minste één VMM-server.<br/><br/>De server VMM ten minste moet worden uitgevoerd systeem Center 2012 SP1 met de meest recente cumulatieve updates.<br/><br/>Als u instellen beveiliging met één VMM server wilt, moet u ten minste twee wolken geconfigureerd op de server.<br/><br/>Als u implementeren beveiliging met twee VMM servers wilt, moet elke server ten minste één cloud geconfigureerd op de primaire VMM server die u wilt beveiligen en één cloud geconfigureerd op de secundaire VMM server die u wilt gebruiken voor beveiliging en herstel<br/><br/>Alle VMM wolken moet het Hyper-V mogelijkheid profiel instellen.<br/><br/>De bron-cloud die u wilt beveiligen, moet een of meer VMM hostgroepen bevatten.<br/><br/>Meer informatie over het instellen van VMM wolken in [Stapsgewijze instructies: privé wolken maken met System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx) in Keith Mayer van blog.
**Hyper-V** | U moet een of meer Hyper-V host-servers in de primaire en secundaire VMM hostgroepen en een of meer virtuele machines op elke Hyper-V host-server.<br/><br/>De host en doelsites Hyper-V-servers ten minste moeten worden uitgevoerd Windows Server 2012 met de functie Hyper-V en de meest recente updates hebt geïnstalleerd.<br/><br/>Een Hyper-V server VMs die u wilt beveiligen met moet zich bevinden in een wolk VMM.<br/><br/>Als u Hyper-V in een cluster uitvoert, houd er rekening mee dat makelaar cluster wordt niet automatisch gemaakt als u een statische IP-adres gebaseerde cluster hebt. U moet de makelaar cluster handmatig configureren. [Meer informatie](https://www.petri.com/use-hyper-v-replica-broker-prepare-host-clusters) in het blogbericht van Aidan Finn.
**Netwerk toewijzing** | U kunt configureren netwerk toewijzing om ervoor te zorgen dat gerepliceerde virtuele machines optimaal worden teruggezet op secundaire Hyper-V hostservers na failover en dat ze verbinding kunnen maken met de juiste VM-netwerken te gebruiken. Als u het netwerk toewijzing niet configureert, wordt niet replica VMs aangesloten op een netwerk na failover.<br/><br/>Als u wilt instellen netwerk toewijzing tijdens de implementatie, zorg dat de virtuele machines op de host bronserver Hyper-V zijn verbonden met een netwerk VMM VM. Dit netwerk moet zijn gekoppeld aan een logisch netwerk dat is gekoppeld aan de cloud. < br /<br/>De cloud doeladres op de secundaire VMM-server die u voor herstel gebruikt corresponderende VM netwerken geconfigureerd nodig hebt en deze op zijn beurt moet zijn gekoppeld aan een bijbehorende logisch netwerk dat is gekoppeld aan de doel-cloud.<br/><br/>[Meer informatie](site-recovery-network-mapping.md) over het netwerk toewijzing.
**Opslag toewijzing** | Wanneer u een virtuele machine op een bronserver van Hyper-V host naar een Hyper-V host doelserver repliceren, wordt gerepliceerde gegevens al dan niet standaard opgeslagen in de standaardlocatie die wordt aangegeven voor de doeltoepassing Hyper-V host in Hyper-V-beheer. Voor meer controle wilt over waar gerepliceerde gegevens worden opgeslagen, kunt u opslagruimte toewijzing configureren<br/><br/> Als u wilt configureren opslag toewijzing, moet u instellen opslag classificaties op de bron en afstemmen VMM servers voordat u implementatie begint. [Meer informatie](site-recovery-storage-mapping.md).


## <a name="step-1-create-a-site-recovery-vault"></a>Stap 1: Een Site herstel kluis maken

1. Meld u aan bij de [Beheerportal](https://portal.azure.com) van de VMM-server die u wilt registreren.

2. **Gegevensservices**uitvouwen > **Herstel Services** en klik op **Site herstel kluis**.

3. Klik op **nieuwe maken** > **snel tabellen maken**.

4. Voer in het vak **naam**een beschrijvende naam voor de kluis.

5. Selecteer het geografische gebied voor de kluis in **regio** . Ondersteunde regio's Zie geografische beschikbaar is in [Azure Site herstel prijzen Details](http://go.microsoft.com/fwlink/?LinkId=389880)controleren.

6. Klik op **maken kluis**.

    ![Kluis maken](./media/site-recovery-vmm-to-vmm-classic/create-vault.png)

Controleer op de statusbalk of de kluis is gemaakt. De kluis worden, als **actief** vermeld op de hoofdpagina van herstel Services.

## <a name="step-2-generate-a-vault-registration-key"></a>Stap 2: Een kluis registratiegegevens sleutel genereren

Genereer een registratiegegevens-sleutel in de kluis. Nadat u de Provider van Azure Site herstel downloaden en op de server VMM installeren, gebruikt u deze toets naar de server VMM in de kluis registreren.

1. Klik in de pagina **Herstel Services** op de kluis om de pagina snel aan de slag te openen. Snel aan de slag kan ook worden geopend op elk gewenst moment met het pictogram.

    ![Snel aan de slag-pictogram](./media/site-recovery-vmm-to-vmm-classic/quick-start-icon.png)

2. Selecteer in de vervolgkeuzelijst **tussen twee on-premises implementatie VMM sites**.
3. Klik op **genereren registratie belangrijke bestand**in **VMM Servers voorbereiden**. Het belangrijkste bestand wordt automatisch gegenereerd en geldt voor 5 dagen nadat deze gegenereerd. Als u bent geen toegang heeft tot de portal van Azure van de server VMM moet u dit bestand kopiëren naar de server.

    ![Registratie-toets](./media/site-recovery-vmm-to-vmm-classic/register-key.png)

## <a name="step-3-install-the-azure-site-recovery-provider"></a>Stap 3: De Provider van Azure Site herstel installeren

4. Klik op de pagina **Snel aan de slag** in **voorbereiden VMM servers**, klikt u op **Downloaden Microsoft Azure Site herstel-Provider voor installatie op VMM servers** om de meest recente versie van het installatiebestand Provider.

2. Dit bestand op de bronserver VMM uitvoeren.

    >[AZURE.NOTE] Als VMM wordt geïmplementeerd in een cluster en u bent de Provider voor de installatie is de eerste keer installeren op een actief knooppunt en klaar bent met de installatie van de server VMM in de kluis registreren. Installeer de Provider op de andere knooppunten. Houd er rekening mee dat als u een de Provider upgrade uitvoert u op alle knooppunten upgraden moet omdat moeten ze al worden uitgevoerd dezelfde versie als Provider.

3. Het installatieprogramma biedt een paar **Oude vereisten controleren** en aanvragen machtigingen voor de service VMM om te beginnen met Provider setup stoppen. De Service VMM worden gestart wanneer setup is voltooid. Als u de installatie uitvoert op een cluster VMM wordt u gevraagd de rol Cluster stoppen.

4. In de **Microsoft Update** kunt u kiezen op updates. Met deze instelling ingeschakeld Provider updates worden geïnstalleerd op basis van uw Microsoft Update-beleid.

    ![Updates van Microsoft](./media/site-recovery-vmm-to-vmm-classic/ms-update.png)

5. De locatie voor de installatie is ingesteld op ** <SystemDrive>\Program Files\Microsoft systeem Center 2012 R2\Virtual Machine Manager\bin**. Klik op de knop installeren om te beginnen met het installeren van de Provider.

    ![InstallLocation](./media/site-recovery-vmm-to-vmm-classic/install-location.png)

6. Klik op **registreren** om te registreren van de server in de kluis nadat de Provider is geïnstalleerd.

    ![InstallComplete](./media/site-recovery-vmm-to-vmm-classic/install-complete.png)
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

13.  Klik op **volgende** om het proces te voltooien. Metagegevens van de server VMM wordt na registratie opgehaald door Azure sites worden hersteld. De server wordt weergegeven in **VMM Servers** > **Servers** in de kluis.

    ![Servers](./media/site-recovery-vmm-to-vmm-classic/provider13.PNG)

### <a name="command-line-installation"></a>Opdrachtregel installatie

De Provider van Azure Site herstel kan ook worden geïnstalleerd vanaf de opdrachtregel. Deze methode kan worden gebruikt voor het installeren van de provider op een Server CORE voor Windows Server 2012 R2.

1. Download de sleutel Provider installatie en registratie naar een map. Bijvoorbeeld C:\ASR.
2. De System Center Virtual Machine Manager-Service stoppen
3. Het installatieprogramma van Provider halen door deze opdrachten vanaf een opdrachtprompt met **beheerdersbevoegdheden** uit te voeren:

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q

4. Installeer de provider door te voeren:

        C:\ASR> setupdr.exe /i

5. De provider registreren door te voeren:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>     

Waar de parameters zijn:

 - **/Credentials**: verplichte parameter die de locatie waarin het belangrijkste registratie-bestand zich bevindt  
 - **/FriendlyName**: parameter is vereist voor de naam van de Hyper-V host-server die wordt weergegeven in de portal Azure sites worden hersteld.
 - **/EncryptionEnabled**: optionele Parameter die u alleen in de VMM Azure scenario gebruiken moet als u versleuteling van uw virtuele machines bij in rust in Azure nodig hebt. Zorg ervoor dat de naam van het bestand dat u opgeeft heeft de extensie **.pfx** .
 - **/proxyAddress**: optionele parameter die het adres van de proxyserver.
 - **/proxyport**: optionele parameter die de poort van de proxyserver.
 - **/proxyUsername**: optionele parameter die de Proxy-gebruikersnaam (als proxy is verificatie vereist).
 - **/proxyPassword**: optionele parameter die het wachtwoord voor het verifiëren met de proxyserver (als proxy is verificatie vereist).  

## <a name="step-4-configure-cloud-protection-settings"></a>Stap 4: Configureren cloud instellingen voor documentbeveiliging

Nadat VMM servers zijn geregistreerd, kunt u instellingen voor documentbeveiliging cloud configureren. Als u de optie **synchroniseren cloud gegevens met de kluis** hebt ingeschakeld tijdens de installatie van de Provider zodat alle wolken op de server VMM weergegeven op het tabblad **Beveiligde Items** in de kluis. Als u niet u hebt een specifieke cloud kunt synchroniseren met Azure Site herstel in het tabblad **Algemeen** van de eigenschappenpagina van cloud in de VMM-console.

![Gepubliceerde Cloud](./media/site-recovery-vmm-to-vmm-classic/clouds-list.png)

1. Klik op **beveiliging voor VMM wolken instellen**op de pagina snel starten.
2. Selecteer op het tabblad **VMM wolken** de cloud die u wilt configureren en Ga naar het tabblad **configuratie** .
3. Selecteer in de **doeltoepassing**, **VMM**.
4. Selecteer in de **doellocatie**, de server op locatie VMM die worden beheerd in de cloud die u wilt gebruiken voor herstel.
4. Selecteer in de **cloud doel**, de doel-cloud die u wilt gebruiken voor de overname van virtuele machines in de cloud bron. Houd rekening met:

    - Het is raadzaam dat u selecteert een doel-wolk die voldoet aan herstelvereisten voor de virtuele machines die u kunt beschermen.
    - Een wolk kunt alleen een paar één cloud behoort, de vorm van een primaire of een cloud-doel.

5. Opgeven hoe vaak in **frequentie kopiëren**, gegevens moeten worden gesynchroniseerd tussen 5he bronsite en doelsites locaties. Houd er rekening mee dat deze instelling is alleen wanneer de host Hyper-V Windows Server 2012 R2 wordt uitgevoerd. Voor andere servers wordt een standaardinstelling van vijf minuten gebruikt.
6. Opgeven of u wilt maken van extra herstel wordt verwezen in **Extra herstel punten**opgeven. De standaardwaarde nul geeft aan dat alleen de meest recente herstel komma voor een primaire virtuele machine is opgeslagen op een replicaserver host. Houd er rekening mee dat het inschakelen van meerdere herstel punten extra opslagruimte vereist voor de momentopnamen die zijn opgeslagen op een bepaald moment herstellen. Standaard worden herstel punten per uur, gemaakt zodat elk opsommingsteken herstel van een uur aan gegevens bevat. De waarde in een herstel punten die u voor de virtuele machine in de console VMM toewijst mag geen kleiner dan de waarde die u toewijst in de console Azure sites worden hersteld.
7. Geef in de **frequentie van toepassing-consistente momentopnamen**, hoe vaak u toepassingen consistente momentopnamen maken. Hyper-V gebruikt twee typen momentopnamen, een momentopname van een standaard waarmee een incrementele momentopname van de hele virtuele machine en een momentopname van een toepassing consistente dat een momentopname van het punt-in-tijd van de toepassingsgegevens in de virtuele machine. Volume schaduw Copy Service (VSS) toepassing consistente momentopnamen gebruiken om ervoor te zorgen dat de toepassingen in een consistente status zijn wanneer de momentopname wordt gemaakt. Houd er rekening mee dat als u de toepassing consistente momentopnamen inschakelt, dit invloed is op de prestaties van toepassingen op bron virtuele machines. Zorg ervoor dat de ingestelde waarde kleiner is dan het aantal aanvullende herstel punten die u configureren.

    ![Instellingen voor documentbeveiliging configureren](./media/site-recovery-vmm-to-vmm-classic/cloud-settings.png)

8. Geef in het **gegevens overbrengen compressie**, of gerepliceerde gegevens die worden overgebracht moeten worden gecomprimeerd.
9. Opgeven hoe verkeer tussen de servers primair en herstelbestanden Hyper-V host wordt geverifieerd bij **verificatie**. Selecteer HTTPS tenzij u een actieve Kerberos-omgeving geconfigureerd hebt. Azure Site herstel configureert automatisch certificaten voor HTTPS-verificatie. Geen handmatige configuratie is vereist. Als u Kerberos selecteert, wordt een Kerberos-tickets worden gebruikt voor de onderlinge verificatie van de hostservers. Standaard wordt poort 8083 en 8084 (voor certificaten) geopend in het Windows Firewall op de Hyper-V host-servers. Houd er rekening mee dat deze instelling is alleen van toepassing voor Hyper-V hostservers waarop Windows Server 2012 R2.
10. Wijzig het poortnummer in waarop de bronsite en doelsites host-computers voor replicatie-verkeer is toegestaan beluisteren in **poort**. U zou de instelling bijvoorbeeld wijzigen als u wilt toepassen Quality of Service (QoS) netwerkbandbreedte beperken voor replicatie-verkeer is toegestaan. Controleer of de poort niet wordt gebruikt door een andere toepassing en dat dit geopend in de firewallinstellingen is.
11. Opgeven hoe de eerste replicatie van gegevens uit de bron doellocaties worden verwerkt, voordat de normale replicatie wordt gestart in **herhaling methode**:

    - **Via netwerk**-gegevens kopiëren via het netwerk kan zijn tijdrovende en systeembronnen. Het is raadzaam dat u deze optie gebruiken als de cloud virtuele machines met relatief kleine virtuele vaste schijven bevat, en als de primaire site is aangesloten op de secundaire site via een verbinding met breed bandbreedte. U kunt opgeven dat de kopie moet direct gestart of Selecteer een tijd. Als u netwerk replicatie gebruikt, wordt u aangeraden na kantooruren te plannen.
    - **Offline**-deze methode wordt aangegeven dat de eerste replicatie wordt worden uitgevoerd met externe media. Het is handig als u wilt voorkomen dat verslechtering van in netwerkprestaties of voor geografisch externe locaties. Deze methode kunt u de locatie exporteren op de cloud bron en de locatie van het importeren in de cloud doel opgeven. Wanneer u beveiliging voor een virtuele machine inschakelt, wordt de virtuele harde schijf wordt gekopieerd naar de locatie van de opgegeven exporteren. U Stuur dit naar de doelsite en kopiëren naar de locatie van het importeren. Het systeem wordt de geïmporteerde gegevens naar de replica virtuele machines gekopieerd.

12. Selecteer **Replica VM verwijderen** om op te geven dat de replica virtuele machine moet worden verwijderd als u stopt met het beveiligen van de virtuele machine door de optie **beveiliging voor de virtuele machine verwijderen** op het tabblad virtuele Machines van de cloud-eigenschappen. Met deze instelling is ingeschakeld, wanneer u de beveiliging uitschakelen de virtuele machine wordt verwijderd uit Azure sites worden hersteld, de herstel van de Site-instellingen voor de virtuele machine worden verwijderd wanneer de VMM-console en de replica wordt verwijderd.

    ![Instellingen voor documentbeveiliging configureren](./media/site-recovery-vmm-to-vmm-classic/cloud-settings-replica.png)

Nadat u de instellingen opslaan wordt een taak wordt gemaakt en klik op het tabblad **taken** kan worden gecontroleerd. Alle Hyper-V hostservers in de cloud van de bron VMM moeten worden geconfigureerd voor replicatie. Cloud instellingen kunnen worden gewijzigd op het tabblad **configureren** . Als u wilt wijzigen van de doelsite locatie- of doellijst cloud moet u de cloud-configuratie verwijderen en vervolgens opnieuw configureren de cloud.

### <a name="prepare-for-offline-initial-replication"></a>Voorbereiden voor offline eerste replicatie

U moet de volgende acties voorbereiden op de eerste replicatie offline doen:

- Op de bronserver, moet u een padlocatie van waaruit het exporteren van gegevens wordt gehouden. Volledig beheer toewijzen voor machtigingen voor NTFS en delen met de service VMM op het pad exporteren. Op de doelserver, moet u de locatie van een pad van waaruit het importeren van gegevens wordt uitgevoerd. Dezelfde machtigingen op dit pad importeren toewijzen.
- Als het pad importeren of exporteren wordt gedeeld, wijst u beheerder, Power gebruiker, Operator afdrukken of Server Operator groepslidmaatschap voor het account van de service VMM op de externe computer waarop de gedeelde zich bevindt.
- Als u uw accounts uitvoeren als gebruikt om toe te voegen hosts, klik op de paden importeren / exporteren, gelezen toewijzen en schrijfmachtigingen voor de accounts uitvoeren als in VMM.
- De aandelen importeren / exporteren moeten niet zich bevinden op elke computer gebruikt als Hyper-V hostserver, omdat loopback-configuratie niet wordt ondersteund door Hyper-V.
- In Active Directory beperkte hostserver waarop virtuele machines die u wilt beveiligen, inschakelen en configureren op elke Hyper-V delegering te vertrouwen van de externe computers waarop de paden importeren / exporteren, als volgt bevinden:
    1. Open op de domeincontroller, **Active Directory-gebruikers en Computers**.
    2. Klik in de consolestructuur op **domeinnaam** > **Computers**.
    3. Met de rechtermuisknop op de naam van de Hyper-V host server > **Eigenschappen**.
    4. Klik op het tabblad **overdracht** op T**roest opgegeven services alleen op deze computer**.
    5. Klik op **elk protocol voor verificatie gebruiken**.
    6. Klik op **toevoegen** > **gebruikers en Computers**.
    7. Typ de naam van de computer waarop het pad exporteren > **OK**. In de lijst met beschikbare services, houd de CTRL-toets ingedrukt en klik op **cifs** > **OK**. Herhaal voor de naam van de computer waarop het pad importeren. Herhaal zo nodig voor extra Hyper-V hostservers.

## <a name="step-5-configure-network-mapping"></a>Stap 5: Netwerk toewijzing configureren
1. Klik op de pagina snel starten op **netwerken toewijzen**.
2. Selecteer de bronserver VMM waaruit u wilt toewijzen netwerken en vervolgens op de doelserver VMM waaraan de netwerken wordt toegewezen. De lijst met de bron-netwerken te gebruiken en hun bijbehorende doel-netwerken worden weergegeven. Een lege waarde wordt weergegeven voor netwerken die momenteel niet zijn toegewezen.
3. Selecteer een netwerk in een **netwerk op bron** > **kaart**. De service wordt de netwerken VM op de doelserver gedetecteerd en worden deze weergegeven. Klik op het informatiepictogram naast de namen van de bronsite en doelsites het netwerk om weer te geven van de subnetten voor elk netwerk.

    ![Netwerk toewijzing configureren](./media/site-recovery-vmm-to-vmm-classic/network-mapping1.png)

4. Selecteer in het dialoogvenster een van de netwerken VM uit de VMM doelserver.

    ![Selecteer een doelnetwerk](./media/site-recovery-vmm-to-vmm-classic/network-mapping2.png)

5. Wanneer u een doelnetwerk selecteert, wordt de beveiligde wolken die gebruikmaken van het Bronnetwerk worden weergegeven. Beschikbare doel-netwerken die gekoppeld aan de wolken gebruikt voor de beveiliging zijn worden ook weergegeven. Het is raadzaam dat u een doelnetwerk die beschikbaar is voor alle wolken die u voor de beveiliging gebruikt selecteert. Of u kunt ook gaat u naar de Server VMM en wijzig de cloud-eigenschappen als u wilt toevoegen van het logische netwerk overeenkomt met het netwerk vm die u wilt kiezen.
6. Klik op het vinkje om het toewijzingsproces te voltooien. Een taak wordt voor het bijhouden van de voortgang van de toewijzing. U kunt deze bekijken op het tabblad **taken** .


## <a name="step-6-configure-storage-mapping"></a>Stap 6: Opslag toewijzing configureren
Wanneer u een virtuele machine op een bronserver van Hyper-V host naar een Hyper-V host doelserver repliceren, wordt gerepliceerde gegevens al dan niet standaard opgeslagen in de standaardlocatie die wordt aangegeven voor de doeltoepassing Hyper-V host in Hyper-V-beheer. Voor meer controle wilt over waar gerepliceerde gegevens worden opgeslagen, kunt u opslagruimte toewijzingen als volgt configureren:



1. Opslag classificaties definiëren op zowel de bronsite en doelsites VMM-servers. [Meer informatie](https://technet.microsoft.com/library/gg610685.aspx). Classificaties moeten zijn beschikbaar voor de servers van de host Hyper-V in bronsite en doelsites wolken. Classificaties hoeft te hebben van hetzelfde type opslag. U kunt bijvoorbeeld een bron-indeling met SMB aandelen een doel-indeling met CSVs afzetten.
2. Nadat de classificaties zijn op hun plaats staan, kunt u toewijzingen kunt maken. Dit wilt doen, klik op de pagina **Quick Start** > **opslag toewijzen**.
3. Klik op het tabblad **opslag** > **opslag classificaties toewijzen**.
4. Klik op het tabblad **opslag classificaties toewijzen** classificaties op de bron selecteren en afstemmen VMM-servers. Instellingen opslaan.

    ![Selecteer een doelnetwerk](./media/site-recovery-vmm-to-vmm-classic/storage-mapping.png)


## <a name="step-7-enable-virtual-machine-protection"></a>Stap 7: Beveiliging van virtuele machine inschakelen
Nadat de servers, wolken en netwerken correct zijn geconfigureerd, kunt u beveiliging voor virtuele machines in de cloud.

1. Klik op het tabblad **virtuele Machines** in de cloud waarin de virtuele machine zich bevindt op **Beveiliging inschakelen** > **virtuele machines toevoegen**.
2. Selecteer in de lijst met virtuele machines in de cloud, de sectie die u wilt beveiligen.

    ![VM beveiliging inschakelen](./media/site-recovery-vmm-to-vmm-classic/enable-protection.png)

3. Voortgang van de actie beveiliging inschakelen in het tabblad **taken** , met inbegrip van de eerste replicatie bijhouden. Nadat de taak voltooien beveiliging wordt uitgevoerd is de virtuele machine gereed voor failover. Nadat de beveiliging is ingeschakeld en virtuele machines worden gerepliceerd, kunt u zult kunnen zien ze in Azure wordt aangegeven.

    ![VM beveiliging taak](./media/site-recovery-vmm-to-vmm-classic/vm-jobs.png)

>[AZURE.NOTE] U kunt ook beveiliging voor virtuele machines in de console VMM inschakelen. Klik op **Beveiliging inschakelen** op de werkbalk in het tabblad **Herstel van Azure-Site** in de virtuele machine-eigenschappen.

### <a name="on-board-existing-virtual-machines"></a>Ingebouwde bestaande virtuele machines

Als u bestaande virtuele machines in VMM die zijn repliceren met Hyper-V Replica moet u ingebouwde ze voor de beveiliging van Azure Site herstel als volgt:

1. Controleer of er primaire en secundaire wolken. Zorg ervoor dat de Hyper-V-server de bestaande virtuele machine hostingprovider bevindt zich in de primaire cloud en dat de Hyper-V-server de replica virtuele machine hostingprovider bevindt zich in de secundaire cloud. Zorg ervoor dat u de instellingen voor documentbeveiliging voor de wolken hebt geconfigureerd. De instellingen moeten overeenkomen met die momenteel is geconfigureerd voor Hyper-V Replica. Anders VM herhaling werkt mogelijk niet zoals verwacht.
2. Vervolgens de beveiliging voor de primaire virtuele machine inschakelen. Azure herstel van de Site en VMM zorgt ervoor dat dat de dezelfde replica host en VM wordt aangetroffen, en Azure Site herstel dan opnieuw gebruiken en herstellen met behulp van de instellingen die tijdens het configureren van de cloud.


## <a name="test-your-deployment"></a>Test uw implementatie

Test uw implementatie kunt u uitvoeren van een failover test voor een enkele virtuele machine, of een plan voor herstel die bestaat uit meerdere virtuele machines maken en uitvoeren van een failover test voor het abonnement.  Test failover simuleert uw failover- en herstelbestanden om in een geïsoleerd netwerk.

### <a name="create-a-recovery-plan"></a>Maak een herstelplan

1. Klik op het tabblad **Herstel van abonnement** op **Herstel plannen maken**.
2. Geef een naam voor het plan voor herstel en bronsite en doelsites VMM-servers. De bronserver moet virtuele machines die zijn ingeschakeld voor failover- en herstelbestanden hebben. Selecteer **Hyper-V** alleen wolken die zijn geconfigureerd voor Hyper-V herhaling weergeven.

    ![Herstel planning maken](./media/site-recovery-vmm-to-vmm-classic/recovery-plan1.png)

3. Selecteer in de **virtuele Machine selecteert**, herhaling groepen. Alle virtuele machines die is gekoppeld aan de replicatiegroep worden geselecteerd en toegevoegd aan het abonnement herstel. Deze virtuele machines worden toegevoegd aan de standaardgroep voor herstel-abonnement, groep 1. u kunt meer groepen toevoegen als nodig is. Houd er rekening mee dat nadat herhaling virtuele machines wordt gestart volgens de volgorde van de herstelgroepen-abonnement.

    ![Virtuele machines toevoegen](./media/site-recovery-vmm-to-vmm-classic/recovery-plan2.png)

Na een herstel is abonnement gemaakt, deze wordt weergegeven in de lijst op het tabblad **Herstel van abonnement** .

###<a name="run-a-test-failover"></a>Een failover test uitvoeren

1. Klik op het tabblad **Herstel van abonnement** Selecteer het abonnement en klik op de **Toets Failover**.
2. Selecteer op de pagina **Bevestigen Test Failover** **geen**. Opmerking die met deze optie ingeschakeld in de mislukte plaats replica virtuele machines wordt niet aangesloten op een netwerk. Hierdoor wordt de virtuele machine wordt overgenomen zoals verwacht, maar test uw netwerk-omgeving voor replicatie niet getest. Bekijk [een failover test uitvoeren](site-recovery-failover.md#run-a-test-failover) voor meer informatie over het gebruik van verschillende netwerken opties.
3. De test virtuele machine wordt gemaakt op dezelfde host als host waarop de replica virtuele machine bestaat. Deze is toegevoegd aan de dezelfde cloud waarin de replica virtuele machine zich bevindt.

### <a name="run-a-recovery-plan"></a>Een plan voor herstel uitvoeren
De replica virtuele machine mogelijk na replicatie geen een IP-adres dat is hetzelfde als het IP-adres van de primaire virtuele machine. Virtuele machines worden bijgewerkt via de DNS-server die ze gebruiken nadat ze starten. U kunt ook een script als u wilt bijwerken van de DNS-Server om ervoor te zorgen een tijdige update toevoegen.

#### <a name="script-to-retrieve-the-ip-address"></a>Script om op te halen, het IP-adres
In dit voorbeeldscript uitvoeren om het IP-adres ophalen.

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

#### <a name="script-to-update-dns"></a>Script om het bijwerken van DNS
In dit voorbeeldscript uitvoeren om bijwerken van DNS, het IP-adres dat u hebt opgehaald met het vorige voorbeeldscript opgeven.

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="privacy-information-for-site-recovery"></a>Informatie over Site herstel de privacy

Hier vindt aanvullende informatie over privacy voor de Microsoft Azure Site herstellen service (""). Als u wilt weergeven op de privacyverklaring voor Microsoft Azure-services, raadpleegt u de [Privacyverklaring van Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=324899)

**Functie: registratie**

- **Beschrijving**: server met service registreert, zodat virtuele machines kunnen worden beveiligd
- **Gegevens verzameld**: nadat de Service registreren worden verzameld, verwerkt en verzendt management certificaatgegevens van de server VMM die op te geven met de naam van de Service van de server VMM en de naam van VM wolken op de server VMM herstel heeft aangewezen.
- **Gebruik van gegevens**:
    - Management certificaat, dit wordt gebruikt om te identificeren en de geregistreerde VMM-server voor toegang tot de Service wordt geverifieerd. De openbare sleutel gedeelte van het certificaat wordt gebruikt voor het beveiligen van een token dat alleen de geregistreerde VMM server kunt toegang krijgen tot de Service. De server moet deze token gebruiken voor toegang tot de Service-functies.
    - Naam van de server VMM, de naam van de server de VMM is verplicht voor het identificeren en communiceren met de juiste VMM-server waarop de wolken zich bevinden.
    - Namen van de server VMM cloud, de naam van de cloud is vereist bij gebruik van de Service-cloud functie koppelen/unpairing hieronder beschreven. Wanneer u uw cloud uit een primaire datacenter met een ander cloud in het midden van de gegevens herstel koppelen besluit, worden de namen van alle wolken van het datacenter herstel worden gepresenteerd.

- **Keuze**: deze informatie is een essentieel onderdeel van het registratieproces Service omdat hiermee u en de Service aan te geven van de VMM server waarvoor u bieden Azure Site hersteld, ook wilt als in de juiste geregistreerde VMM-server te identificeren. Als u niet dat deze informatie naar de Service verzenden wilt, gebruikt u deze Service niet in. Als u uw server registreren en vervolgens later wilt unregister deze, kunt u dit doen door de informatie over de server VMM verwijderen uit de Service-portal (dit is de portal van Azure).

**Voorziening: Beveiliging van Azure Site herstel inschakelen**

- **Beschrijving**: het Azure Site herstel-Provider is geïnstalleerd op de server VMM is het kanaal voor communicatie met de Service. De Provider is een DLL-bestand (DLL) die worden gehost in het proces VMM. Nadat de Provider is geïnstalleerd, wordt de functie 'Datacenter herstel' in de beheerconsole VMM ingeschakeld. Een nieuw of bestaand virtuele machines in een wolk kunt een eigenschap met de naam 'Datacenter herstel' ter bescherming van de virtuele machine inschakelen. Als deze eigenschap is ingesteld, verzendt de Provider de naam en ID van de virtuele machine naar de Service. De virtuele beveiliging is ingeschakeld door Windows Server 2012- of Windows Server 2012 R2 Hyper-V replicatie-technologie. De gegevens VM worden gerepliceerd van de ene Hyper-V host naar een andere (meestal zich in een datacenter verschillende 'herstellen').

- **Gegevens verzameld**: de Service worden verzameld, verwerkt, en metagegevens voor de virtuele machine, waaronder de naam, ID virtueel netwerk en de naam van de cloud verstuurt waarbij deze hoort.

- **Gebruik van gegevens**: de Service wordt de bovenstaande gegevens gebruikt om te vullen de VM-gegevens op de Service-portal.

- **Keuze**: dit is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet dat deze informatie die naar de Service is verzonden wilt, niet Azure-Site hersteld voor een virtuele machines inschakelen. Houd er rekening mee dat alle gegevens die zijn verzonden door de Provider naar de Service is verzonden via HTTPS.

**Functie: Plan voor**

- **Beschrijving**: deze functie helpt u bij het maken van een abonnement integratie voor het datacenter 'herstellen'. U kunt de volgorde waarin de virtuele machines of een groep van virtuele machines moet worden gestart op de site met herstel definiëren. U kunt ook een geautomatiseerde scripts als u wilt uitvoeren of een handmatige actie moet worden uitgevoerd, klikt u op het moment van herstel voor elke virtuele machine opgeven. Failover (in het volgende gedeelte behandeld) wordt meestal geactiveerd op het niveau herstel plannen voor gecoördineerde herstel.

- **Gegevens verzameld**: de Service worden verzameld, verwerkt, en metagegevens voor het abonnement herstel, inclusief VM metagegevens en metagegevens van een automatische scripts en de handmatige actie notities verstuurt.

- **Gebruik van gegevens**: de metagegevens hierboven beschreven voor het maken van het abonnement herstel in de portal van uw Service wordt gebruikt.

- **Keuze**: dit is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet dat deze informatie die naar de Service is verzonden wilt, niet samenstellen herstel van abonnement in deze Service.

**Voorziening: Netwerk toewijzing**

- **Beschrijving**: met deze functie kunt u gegevens van het netwerk van de primaire Datacenter Datacenter voor de herstel toewijzen. Wanneer de virtuele machines worden hersteld op de site herstel, wordt deze toewijzing helpt bij het vaststellen van netwerkconnectiviteit voor hen.

- **Gegevens verzameld**: als onderdeel van de functie voor het netwerk, de Service worden verzameld, verwerkt, en verstuurt de metagegevens van de logische netwerken voor elke site (primaire en datacenter).

- **Gebruik van gegevens**: de Service wordt de metagegevens gebruikt om te vullen van de portal van uw Service waar u de gegevens van het netwerk kunt afzetten.

- **Keuze**: dit is een essentieel onderdeel van de Service en kan niet worden uitgeschakeld. Als u niet dat deze gegevens worden verzonden naar de Service wilt, hoeft u de functie voor het netwerk.

**Voorziening: Failover - geplande, niet-geplande, test**

- **Beschrijving**: deze functie kunt overname van een virtuele machine uit één VMM beheerde Datacenter naar een andere VMM beheerde Datacenter. De actie failover wordt geactiveerd door de gebruiker op de Service-portal. Mogelijke redenen om een failover bevat een niet-geplande gebeurtenis (bijvoorbeeld in geval van een natuurlijke disaster0; een geplande gebeurtenis (bijvoorbeeld datacenter van taakverdeling); een test failover (bijvoorbeeld een herstel abonnement try-out).

De Provider op de server VMM hoogte gesteld van de gebeurtenis van de Service en een actie failover uitgevoerd op de host Hyper-V via VMM interfaces. Werkelijke overname van de virtuele machine van de ene Hyper-V host naar een andere (meestal wordt uitgevoerd in een datacenter verschillende "herstel") wordt uitgevoerd door de technologie voor Windows Server 2012- of Windows Server 2012 R2 Hyper-V-herhaling. Nadat de overname voltooid is, worden de Provider die is geïnstalleerd op de server VMM van het datacenter 'herstellen' het succes-gegevens naar de Service.

- **Gegevens verzameld**: de Service gebruikt de bovenstaande gegevens om de status van de gegevens van de actie failover op de Service-portal vullen.

- **Gebruik van gegevens**: de Service gebruikt de bovenstaande gegevens als volgt:

    - Management certificaat, dit wordt gebruikt om te identificeren en de geregistreerde VMM-server voor toegang tot de Service wordt geverifieerd. De openbare sleutel gedeelte van het certificaat wordt gebruikt voor het beveiligen van een token dat alleen de geregistreerde VMM server kunt toegang krijgen tot de Service. De server moet deze token gebruiken voor toegang tot de Service-functies.
    - Naam van de server VMM, de naam van de server de VMM is verplicht voor het identificeren en communiceren met de juiste VMM-server waarop de wolken zich bevinden.
    - Namen van de server VMM cloud, de naam van de cloud is vereist bij gebruik van de Service-cloud functie koppelen/unpairing hieronder beschreven. Wanneer u uw cloud uit een primaire datacenter met een ander cloud in het midden van de gegevens herstel koppelen besluit, worden de namen van alle wolken van het datacenter herstel worden gepresenteerd.

- **Keuze**: dit is een essentieel onderdeel van de service en kan niet worden uitgeschakeld. Als u niet dat deze gegevens worden verzonden naar de Service wilt, hoeft u deze Service.

## <a name="next-steps"></a>Volgende stappen

Nadat u een failover testen om te controleren van dat uw omgeving werkt zoals verwacht, [meer informatie over](site-recovery-failover.md) verschillende soorten failovers hebt uitgevoerd.
