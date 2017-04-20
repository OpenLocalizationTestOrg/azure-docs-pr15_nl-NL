<properties 
   pageTitle="Mislukt achterste VMware virtuele machines en fysieke servers van Azure VMware (verouderd) | Microsoft Azure" 
   description="In dit artikel wordt beschreven hoe mislukt terug VMware virtuele machines die is gerepliceerd naar Azure met Azure sites worden hersteld." 
   services="site-recovery" 
   documentationCenter="" 
   authors="ruturaj" 
   manager="mkjain" 
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="storage-backup-recovery" 
   ms.date="10/05/2016"
   ms.author="ruturajd@microsoft.com"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-from-azure-to-vmware-with-azure-site-recovery-legacy"></a>Mislukt achterste VMware virtuele machines en fysieke servers van Azure VMware met Azure Site herstel (verouderd)

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-failback-azure-to-vmware.md)
- [Azure klassieke Portal](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure klassieke-Portal (verouderd)](site-recovery-failback-azure-to-vmware-classic-legacy.md)


De Site herstel van Azure-service draagt bij aan uw strategie voor bedrijven bedrijfscontinuïteit en noodgevallen herstel (BCDR) door orchestrating herhaling, failover en herstel van virtuele machines en fysieke servers. Computers kunnen worden gerepliceerd naar Azure of naar een secundaire on-premises implementatie-Datacenter. Voor een kort overzicht lezen [Wat Azure Site herstel is?](site-recovery-overview.md)

## <a name="overview"></a>Overzicht

In dit artikel wordt beschreven hoe achterste VMware virtuele machines en Windows/Linux fysieke servers van Azure naar de site van uw on-premises implementatie mislukt nadat u vanuit uw on-premises implementatie-site hebt gerepliceerd naar Azure.

>[AZURE.NOTE] In dit artikel beschrijft een oudere scenario. U moet alleen de instructies in dit artikel gebruiken als u naar [deze oudere](site-recovery-vmware-to-azure-classic-legacy.md)instructies Azure gerepliceerd. Als u met behulp van de [uitgebreide implementatie](site-recovery-vmware-to-azure-classic-legacy.md) instellen en voert u de instructies in [dit artikel](site-recovery-failback-azure-to-vmware-classic.md) terug mislukt. 


## <a name="architecture"></a>Architectuur

In dit diagram staat voor het scenario failover en foutherstel. De blauwe regels zijn de verbindingen die worden gebruikt tijdens een overname. De rode lijnen zijn de verbindingen die worden gebruikt tijdens failback. De regels met pijlen Ga via internet.

![](./media/site-recovery-failback-azure-to-vmware/vconports.png)

## <a name="before-you-start"></a>Voordat u begint 

- U moet zijn mislukt via uw VMware VMs of fysieke servers en moeten ze worden uitgevoerd in Azure wordt aangegeven.
- Houd er rekening mee dat u kunt alleen niet terug VMware virtuele machines en Windows/Linux fysieke servers van Azure VMware virtuele machines in de on-premises implementatie primaire-site.  Als u terug een fysieke machine verbroken bent, failover naar Azure wordt converteren naar een VM Azure en failback VMware wordt converteren naar een VM VMware.

Hier ziet u hoe u failback instellen:

1. **Failback onderdelen instellen**: U moet een server vContinuum instellen on-premises en wijs de configuratieserver VM in Azure wordt aangegeven. U kunt ook een processerver instellen als een VM Azure gegevens terug naar de on-premises implementatie basispagina doelserver verzenden. U de processerver hebt geregistreerd met de configuratieserver die de overname verwerkt. U installeren een on-premises implementatie basispagina doelserver. Als u een Windows-basispagina doelserver moet functie deze ingesteld automatisch tijdens de installatie van vContinuum. Als u nodig Linux hebt moet u deze handmatig instellen op een afzonderlijke server.
2. **Beveiliging en foutherstel inschakelen**: nadat u de onderdelen hebt ingesteld, in vContinuum moet u beveiliging inschakelen voor Azure VMs overgenomen. U hebt uitgevoerd een gereedheidscontrole op de VMs en een failover van Azure naar uw on-premises implementatie-site. Nadat failback is voltooid reprotect on-premises implementatie machines zodat ze gaan repliceren naar Azure.



## <a name="step-1-install-vcontinuum-on-premises"></a>Stap 1: Installeer vContinuum on-premises implementatie

U moet voor het installeren van een server vContinuum on-premises en wijs de configuratieserver.

1.  [VContinuum downloaden](http://go.microsoft.com/fwlink/?linkid=526305). 
2.  Vervolgens downloaden de versie [vContinuum bijwerken](http://go.microsoft.com/fwlink/?LinkID=533813) .
3. Installeer de meest recente versie van vContinuum. Klik op **volgende**op **de welkomstpagina** .
    ![](./media/site-recovery-failback-azure-to-vmware/image2.png)
4.  Geef het IP-adres van de CX-server en de serverpoort CX op de eerste pagina van de wizard. Selecteer **gebruiken HTTPS**.

    ![](./media/site-recovery-failback-azure-to-vmware/image3.png)

5.  Zoek de configuratieserver IP-adres op het tabblad **Dashboard** van de configuratieserver VM in Azure wordt aangegeven.
    ![](./media/site-recovery-failback-azure-to-vmware/image4.png)

6.  Zoek de configuratieserver openbare HTTPS-poort op het tabblad van de **eindpunten** van de configuratieserver VM in Azure wordt aangegeven.

    ![](./media/site-recovery-failback-azure-to-vmware/image5.png)

7. Geef op de pagina **Details van CS wachtwoordzin** de wachtwoordzin die u hebt genoteerd ingedrukt wanneer u de configuratieserver geregistreerd. Als u niet meer weet deze inchecken **C:\\Program Files (x86)\\InMage systemen\\privé\\connection.passphrase** op de configuratieserver VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image6.png)

8.  Op de pagina **Selecteer doellocatie** opgeven waar u de server vContinuum installeren en klik op **installeren**.

    ![](./media/site-recovery-failback-azure-to-vmware/image7.png)

9. Nadat de installatie is voltooid, kunt u vContinuum starten.
    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)


## <a name="step-2-install-a-process-server-in-azure"></a>Stap 2: Een processerver installeren in Azure wordt aangegeven 

U moet een processerver installeren in Azure wordt aangegeven, zodat de VMs in Azure wordt aangegeven de gegevens terug naar een on-premises implementatie basispagina doelserver kunnen verzenden. 

1.  Selecteer een nieuwe processerver toevoegen op de pagina **Configuratieservers** in Azure wordt aangegeven.

    ![](./media/site-recovery-failback-azure-to-vmware/image9.png)

2.  Een processerver opgeven de naam en een naam en een wachtwoord verbinding maken met de virtuele machine als beheerder. Selecteer de configuratieserver waarnaar u wilt de processerver registreren. Dit moet u gebruikt om te beveiligen en mislukt via uw virtuele machines dezelfde server.
3.  Geef het Azure netwerk waarin de processerver moet worden geïmplementeerd. Moet hetzelfde netwerk als de configuratieserver. Geef een unieke IP-adres dat is geselecteerd subnet en implementatie begint.

    ![](./media/site-recovery-failback-azure-to-vmware/image10.png)

4.  Een taak wordt als u wilt implementeren van de processerver geactiveerd.

    ![](./media/site-recovery-failback-azure-to-vmware/image11.png)

5.  Nadat de processerver wordt geïmplementeerd in Azure wordt aangegeven kunt u zich aanmelden bij de server met de referenties die u hebt opgegeven. Het processerver op dezelfde manier als die u de on-premises implementatie processerver geregistreerd registreren. 

    ![](./media/site-recovery-failback-azure-to-vmware/image12.png)

>[AZURE.NOTE] De servers die zijn geregistreerd tijdens failback worden niet zichtbaar onder VM eigenschappen bij het herstellen van de Site. Ze zijn alleen zichtbaar onder het tabblad **Servers** van de configuratieserver waaraan ze bent geregistreerd. Het duurt ongeveer 10-15 minuten totdat ze de processerver wordt weergegeven op het tabblad.


## <a name="step-3-install-a-master-target-server-on-premises"></a>Stap 3: Installeer een basispagina doelserver on-premises implementatie

Afhankelijk van uw bronbesturingssysteem virtuele machines moet u een Linux installeren of on-premises een basispagina doel-servers van Windows.

### <a name="deploy-a-windows-master-target-server"></a>Een basispagina doelserver van Windows implementeren

Een basispagina doel van Windows wordt al meegeleverd met vContinuum-instelling. Wanneer u de vContinuum installeert, is ook een basispagina server geïmplementeerd op dezelfde computer en op de configuratieserver geregistreerd.

1.  Als u wilt beginnen implementatie, maakt u een lege on-premises implementatie op de ESX-host waarop u wilt herstellen van de VMs van Azure van de computer.

2.  Zorg ervoor dat er ten minste twee schijven die zijn gekoppeld aan de VM zijn: een wordt gebruikt voor het besturingssysteem en de andere wordt gebruikt voor het bewaarbeleid-station.

3.  Installeer het besturingssysteem.

4.  Installeer de vContinuum op de server. Dit ook installatie van de basispagina doelserver is voltooid.

### <a name="deploy-a-linux-master-target-server"></a>Een basispagina doelserver Linux implementeren

1.  Als u wilt beginnen implementatie, maakt u een lege on-premises implementatie op de ESX-host waarop u wilt herstellen van de VMs van Azure van de computer.

2.  Zorg ervoor dat er ten minste twee schijven die zijn gekoppeld aan de VM zijn: een wordt gebruikt voor het besturingssysteem en de andere wordt gebruikt voor het bewaarbeleid-station.

3.  Installeer het besturingssysteem Linux. Het Linux basispagina target-systeem moet LVM niet gebruiken voor hoofdsite of bewaarbeleid opslag spaties. Een basispagina doelserver Linux is geconfigureerd om te voorkomen dat LVM partities/schijven discovery al dan niet standaard.
4.  Partities die u kunt maken:

    ![](./media/site-recovery-failback-azure-to-vmware/image13.png)

5.  Verrichten de hieronder posten installatiestappen voordat u de installatie van de basispagina doel.


#### <a name="post-os-installation-steps"></a>Bericht OS installatiestappen

Als u de SCSI-id's voor elk van SCSI harde schijf in een Linux virtuele machine, inschakelen met de parameter "schijf. EnableUUID = TRUE "als volgt:

1. Uw virtuele computer afgesloten.
2. Met de rechtermuisknop op de vermelding VM in het deelvenster aan de linkerkant > **Instellingen bewerken**.
3. Klik op het tabblad **Opties** . Selecteer **Geavanceerd\>algemene item** > **Configuratieparameters**. De optie **Parameters configuratie** is alleen beschikbaar als de computer wordt afgesloten.

    ![](./media/site-recovery-failback-azure-to-vmware/image14.png)

4. Hiermee wordt gecontroleerd of een rij met **schijf. EnableUUID** bestaat. Als deze is ingesteld op **Onwaar** en stel deze in **waar** (niet hoofdlettergevoelig). Als bestaat en is ingesteld op waar, klikt u op **Annuleren** en testen van de opdracht SCSI binnen het gastbesturingssysteem nadat deze opgestart omhoog. Als niet bestaat klikt u op **Rij toevoegen**.
5. Schijf toevoegen. EnableUUID in de kolom **naam** . Stel de waarde als waar. Geen de bovenstaande waarden samen met dubbele aanhalingstekens toevoegen.

    ![](./media/site-recovery-failback-azure-to-vmware/image15.png)

#### <a name="download-and-install-the-additional-packages"></a>De extra pakketten downloaden en installeren

Opmerking: Controleer of dat het systeem heeft internetconnectiviteit vóór het downloaden en installeren van de aanvullende pakketten.

\#YUM -y xfsprogs perl lsscsi rsync wget kexec-'s installeren

Deze opdracht downloadt u deze 15-pakketten van CentOS 6.6 opslagplaats en deze is geïnstalleerd:

BC-1.06.95-1.el6.x86\_64. rpm

busybox-1.15.1-20.el6.x86\_64. rpm

elfutils-libs-0.158-3.2.el6.x86\_64. rpm

kexec-hulpmiddelen-2.0.0-280.el6.x86\_64. rpm

lsscsi-0.23-2.el6.x86\_64. rpm

lzo-2.03-3.1.el6\_5.1.x86\_64. rpm

Perl-5.10.1-136.el6\_6.1.x86\_64. rpm

Perl-Module-Pluggable-3.90-136.el6\_6.1.x86\_64. rpm

Perl-Pod-Hiermee heft u-trilling 1,04-136.el6\_6.1.x86\_64. rpm
    
Perl-Pod-eenvoudige-3.13-136.el6\_6.1.x86\_64. rpm

Perl-libs-5.10.1-136.el6\_6.1.x86\_64. rpm

Perl-versie-0,77-136.el6\_6.1.x86\_64. rpm

rsync-3.0.6-12.el6.x86\_64. rpm

treffende-1.1.0-1.el6.x86\_64. rpm

wget-1.12-5.el6\_6.1.x86\_64. rpm

Opmerking: Als de broncomputer Reiser of XFS bestandssysteem voor de hoofdmap of opstarten apparaat gebruikt, klikt u vervolgens volgende pakketten moeten worden gedownload en geïnstalleerd op de basispagina doel Linux voordat u beveiliging.

\#cd-/usr/local

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

\#rpm - ivh kmod-reiserfs-0,0-1.el6.elrepo.x86\_64. rpm reiserfs-utils-3.6.21-1.el6.elrepo.x86\_64. rpm

\#wget <http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

\#rpm - ivh xfsprogs-3.1.1-16.el6.x86\_64. rpm

#### <a name="apply-custom-configuration-changes"></a>Aangepaste configuratiewijzigingen toepassen

Zorg ervoor dat u de vorige sectie hebt voltooid voordat deze wijzigingen is toegepast, en vervolgens als volgt te werk:


1. Kopieer de RHEL 6-64 Unified Agent binaire naar de zojuist gemaakte OS.

2. Deze opdracht naar de binaire untar uitvoert: **- zxvf oelframe \<bestandsnaam\> **

3. Deze opdracht om te machtigen uitvoert: \# **type chmod 755./ApplyCustomChanges.sh**

4. Het script uitvoeren: ** \# ./ApplyCustomChanges.sh**. Voer het script slechts eenmaal op de server. Start opnieuw op de server nadat het script wordt uitgevoerd.



### <a name="install-the-linux-server"></a>De Linux-server installeren


1. [Download](http://go.microsoft.com/fwlink/?LinkID=529757) het bestand voor installatie.
2. Kopieer het bestand naar de Linux outmodel Target virtuele machine met een clientprogramma sftp van uw keuze. U kunt ook Meld u aan bij de Linux basispagina target virtuele machine en wget gebruikt om te downloaden van het installatiepakket uit de onderstaande koppeling
3. Zich aanmeldt bij de Linux basispagina doel server VM gebruikt een ssh client van uw keuze
4. Als u bent verbonden met het Azure netwerk waarin u de doelserver van uw Linux via een VPN-verbinding geïmplementeerd gebruikt u de interne IP-adres voor de server die u in het tabblad van de **Dashboard** VM en poort 22 verbinding maken met het Linux basispagina doel Server met Secure Shell kunt vinden.
5. Als u verbinding maakt met de basispagina doelserver Linux via een openbare internetverbinding de Linux basispagina doelserver van openbare virtuele IP-adres gebruiken (via het tabblad van het **Dashboard** virtuele machines) en de openbare eindpunt gemaakt voor ssh aanmelden bij de servder Linux.
6. De bestanden extraheren uit het archief gzipped Linux basispagina doel Server installer tar door te voeren: *"– xvzf Microsoft-ASR oelframe\_UA\_8.2.0.0\_RHEL6 64\"* uit de map waarin het installatiebestand.

    ![](./media/site-recovery-failback-azure-to-vmware/image16.png)

7. Als u het installatieprogramma van bestanden naar een andere map wijzigen in de map uitgepakt zijn waarin de inhoud van de tar archiveren uitgepakt. Uit dit pad uitvoeren "sudo. / install.sh".

    ![](./media/site-recovery-failback-azure-to-vmware/image17.png)

8. Wanneer u wordt gevraagd om te kiezen Selecteer een primaire rol **2 (outmodel doel)**. Laat de andere interactieve installatieopties standaardinstellingen.
9. Wacht totdat de installatie om door te gaan en de gebruikersinterface van de zoekconfiguratie Host moet worden weergegeven. Het hulpprogramma hostconfiguratie voor de basispagina sarget Linux Server is een opdrachtregelhulpprogramma. Het formaat niet wijzigen de ssh client hulpprogramma venster. Gebruik de pijltoetsen om te selecteren de optie **globale** en druk op ENTER op het toetsenbord.

    ![](./media/site-recovery-failback-azure-to-vmware/image18.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image19.png)


10. Voer in het veld **IP** het interne IP-adres van de configuratieserver (u vindt deze in het tabblad **Dashboard** van de configuratieserver VM) en druk op ENTER. Voer in **poort** **22** en druk op ENTER. 
11.  Laat **Gebruik HTTPS** als **Ja**en druk op ENTER.
12.  Geef de wachtwoordzin die is gegenereerd op de configuratieserver. Als u gebruikmaakt van een STOPVERF client van een Windows-computer naar ssh Linux virtual machine kunt u Shift + Insert om de inhoud van het Klembord te plakken. De wachtwoordzin naar het lokale Klembord met Ctrl + C kopiëren en plakken met Shift + Insert. Druk op ENTER.
13.  Pijl-rechts gebruiken om te navigeren om te sluiten en druk op ENTER. Wacht totdat de installatie is voltooid.

    ![](./media/site-recovery-failback-azure-to-vmware/image20.png)

Als om een bepaalde reden u kan niet worden geregistreerd de doelserver van uw Linux op de configuratieserver kunt u dit doen door de host configuratiehulpprogramma voeren uit /usr/local/ASR/Vx/bin/hostconfigcli (u moet eerst toegangsmachtigingen naar deze map instellen door type chmod uit te voeren als supergebruiker).


#### <a name="validate-master-target-registration"></a>Valideer de registratie van de basispagina doel

U kunt valideren dat de basispagina doelserver is geregistreerd op de configuratieserver in Azure Site herstel kluis > **Configuratieserver** > **Server-gegevens**.

>[AZURE.NOTE] Na de registratie van de basispagina doelserver als het foutbericht configuratiefouten die de virtuele machine mogelijk zijn verwijderd uit Azure of die eindpunten worden niet correct is geconfigureerd, namelijk Hoewel de basispagina doelconfiguratie door de Azure dndpoints wordt gedetecteerd wanneer het doel van de basispagina wordt geïmplementeerd in Azure wordt aangegeven, dit geldt niet voor een on-premises implementatie basispagina doel server on-premises. Dit geen invloed op failback en kunt u deze fouten negeren. 



## <a name="step-4-protect-the-virtual-machines-to-the-on-premises-site"></a>Stap 4: De virtuele machines naar de site on-premises implementatie beveiligen

U moet VMs naar de site on-premises implementatie beveiligen voordat u niet terug.

### <a name="before-you-begin"></a>Voordat u begint

Wanneer een VM is Azure via is mislukt, wordt een extra temp station voor paginabestand toegevoegd. Dit is een extra station die meestal niet vereist is door uw mislukte via VM omdat deze mogelijk al een station die specifiek zijn voor het paginabestand. Voordat u een omgekeerde bescherming van de virtuele machines, moet u ervoor zorgen dat dit station wordt gehouden offline zodat deze wordt niet beveiligd. Ga als volgt als volgt:

1.  Beheer van de Computer openen en selecteer opslagbeheer zodat deze een lijst met de schijven online en die zijn bijgevoegd bij de computer.
2.  Selecteer de tijdelijke schijf die zijn bijgevoegd bij de computer en kies om deze offline weer te geven. 

### <a name="protect-the-vms"></a>De VMs beveiligen

1. Klik in de portal Azure Controleer de lidstaten van de virtuele machine om ervoor te zorgen dat overgenomen heeft zich.

    ![](./media/site-recovery-failback-azure-to-vmware/image21.png)

2. Start vContinuum op uw computer.

    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)

3. Klik op **Nieuwe beveiliging** en selecteer het type bewerking systeem de 

4.  Klik in het nieuwe venster dat openen Selecteer het **type besturingssysteem** > **Details ophalen** voor het gewenste terug mislukt VMs. In **de details van de primaire server**, identificeren en selecteer de virtuele machines die u wilt beveiligen. VMs worden vermeld onder de hostnaam van vCenter die ze vóór failover op waren.
5.  Wanneer u een virtuele machine te beveiligen selecteren (en dat nog niet is via Azure) biedt een pop-upvenster twee vermeldingen voor de virtuele machine. Dit komt omdat de configuratieserver vastgesteld twee instanties van de virtuele machines geregistreerd. Moet u het fragment voor de on-premises implementatie VM verwijderen, zodat u de juiste VM kunt beveiligen. Als u wilt identificeren de juiste Azure VM-vermelding hier, kunt u Meld u aan bij de VM Azure en Ga naar C:\Program Files (x86) \Microsoft Azure Site Recovery\Application Data\etc. In het bestand drscout.conf, welke de host-ID. Klik in het dialoogvenster vContinuum door de kolom waarvoor u de host-ID op de VM gevonden te behouden. Verwijder alle overige gegevens. Als u wilt selecteren van de juiste VM kunt u verwijzen naar het IP-adres. Het IP-adres bereik on-premises implementatie is de on-premises implementatie VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image22.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image23.png)

6. Stop de virtuele machine op de server vCenter. U kunt ook de VMs on-premises implementatie verwijderen.
7. Geef de on-premises implementatie MT server waarnaar u wilt beveiligen van de VMs. Klik hiertoe verbinding maken met de vCenter-server waarnaar u wilt terug mislukt.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

8. Selecteer de basispagina doelserver op basis van de host waarnaar u wilt herstellen van de VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

9. Geef de optie herhaling voor elk van de virtuele machines. Hiervoor moet u selecteert u het herstelproces is kant gegevensopslag waarnaar de VMs wordt hersteld. De tabel worden de verschillende opties die u nodig hebt om aan te bieden voor elke VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image25.png)

    **Optie** | **Optie aanbevolen waarde**
    ---|---
    IP-adres van het proces | Selecteer het processerver geïmplementeerd in Azure wordt aangegeven
    Bewaarbeleid grootte in MB| 
    Bewaarbeleid waarde | 1
    Dagen/uur | Dagen
    Consistentie interval | 1
    Selecteer doel gegevensopslag | De gegevensopslag beschikbaar op de site herstellen. De gegevensopslag moet voldoende ruimte en beschikbaar zijn voor de ESX host waarop u wilt herstellen van de virtuele machine.


10. De eigenschappen die de virtuele machine na failover naar on-premises-site behaalt configureren. De eigenschappen worden in de volgende tabel samengevat.

    ![](./media/site-recovery-failback-azure-to-vmware/image26.png)

    **Eigenschap** | **Meer informatie**
    ---|---
    Netwerkconfiguratie| Voor elke netwerkadapter gedetecteerd, selecteert u deze en klik op **wijzigen** als u wilt het failback IP-adres voor de virtuele machine configureren. 
    Hardwareconfiguratie| Geef de processor en geheugen voor VM. Instellingen kunnen worden toegepast op alle VMs die u wilt beveiligen. Als u wilt identificeren de juiste waarden voor de processor en geheugen, kunt u verwijzen naar de grootte van de rol IAAS VMs en raadpleegt u het aantal cores en geheugen die is toegewezen.
    Weergavenaam | Na failback naar on-premises implementatie, kunt u de virtuele machines de namen in de voorraad vCenter zullen zien. De standaardnaam is de naam van de VM computer-host. Als u wilt identificeren de naam van de VM, kunt u verwijzen naar de lijst VM in de groep beveiliging.
    NAT-configuratie | In meer detail onderstaande

    ![](./media/site-recovery-failback-azure-to-vmware/image27.png)

#### <a name="configure-nat-settings"></a>NAT-instellingen configureren

1. Als u wilt de beveiliging van de virtuele machines inschakelt, moeten twee communicatiekanalen tot stand worden gebracht. Het eerste kanaal is tussen de virtuele machine en de processerver. Dit kanaal worden de gegevens van de VM verzameld en verzonden naar de processerver stuurt de gegevens naar de basispagina doelserver. Als de processerver en de virtuele machine worden beveiligd zich op hetzelfde Azure virtuele netwerk u niet hoeft te NAT-instellingen. Anders opgeven NAT-instellingen. Bekijk het openbare IP-adres van de processerver in Azure wordt aangegeven. 

    ![](./media/site-recovery-failback-azure-to-vmware/image28.png)

2. Het tweede kanaal is tussen de processerver en de basispagina doelserver. De optie voor het gebruik van NAT al dan niet, is afhankelijk van of u met behulp van een gebaseerd VPN-verbinding of communiceren via internet. Selecteer de NAt niet als u VPN gebruikt, maar alleen als u een internetverbinding gebruikt.

    ![](./media/site-recovery-failback-azure-to-vmware/image29.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image30.png)

3. Als u de on-premises implementatie virtuele machines als opgegeven dat nog niet hebt verwijderd en de gegevensopslag die u terug naar zich nog steeds worden verbroken de oude VMDK bevat moet u ervoor te zorgen dat de failback die VM wordt gemaakt in een nieuwe locatie. Hiervoor selecteert u de instellingen voor **Geavanceerd** en een andere map om te zetten naar in de **Instellingen voor de map**. Laat de andere opties met de standaardinstellingen. De instellingen voor de map toepassen op alle servers.

    ![](./media/site-recovery-failback-azure-to-vmware/image31.png)

4. Voer een gereedheidscontrole om ervoor te zorgen dat de virtuele machines gereed bent om te worden beveiligd terug naar de on-premises implementatie. 

    ![](./media/site-recovery-failback-azure-to-vmware/image32.png)


5. Wacht totdat deze om te voltooien. Als het goed op alle VMs is kunt u een naam voor de beveiliging plannen. Klik vervolgens op **beveiligen** moet beginnen.

    ![](./media/site-recovery-failback-azure-to-vmware/image33.png)


6. U kunt de voortgang in vContinuum controleren.

    ![](./media/site-recovery-failback-azure-to-vmware/image34.png)

7. U kunt VM beveiliging in de portal-Site herstel controleren na de stap **Die activeren beveiliging plannen** is voltooid.

    ![](./media/site-recovery-failback-azure-to-vmware/image35.png)

8. Hier ziet u de exacte status op de VM te klikken en de voortgang bewaken.

    ![](./media/site-recovery-failback-azure-to-vmware/image36.png)

## <a name="prepare-the-failback-plan"></a>Het plan voor failback voorbereiden

U kunt een failback-abonnement met vContinuum zodat de toepassing kan worden niet terug naar de on-premises implementatie-site op elk gewenst moment kunt voorbereiden. Deze herstel-abonnementen zijn vergelijkbaar met de abonnementen herstel bij het herstellen van de Site.

1.  VContinuum starten en selecteer **abonnementen beheren** > **herstellen.** U kunt zien van lijst met alle abonnementen die worden uitgevoerd VMs zijn gebruikt. U kunt de dezelfde abonnementen gebruiken om te herstellen.

    ![](./media/site-recovery-failback-azure-to-vmware/image37.png)

2. Selecteer het abonnement beveiliging en alle van de VMs die u wilt herstellen erin. Als u elke VM selecteren kunt u meer informatie, inclusief de doelserver ESX en de bron VM schijf zien. Klik op **volgende** om te beginnen met de Wizard herstellen en selecteer de VMs die u wilt herstellen.

    ![](./media/site-recovery-failback-azure-to-vmware/image38.png)

3. U kunt terugzetten op basis van meerdere opties, maar we raden te u **Nieuwste Tag** gebruiken en selecteer **toepassen voor alle VMs** om ervoor te zorgen dat de meest recente gegevens uit de virtuele machine wordt gebruikt.
4. Voer de **gereedheidscontrole.** Hiermee wordt gecontroleerd als de juiste parameters worden geconfigureerd VM herstellen. Klik op **volgende** als u alle controles zijn voltooid. Als dit niet het logboek controleren en de fouten op te lossen.

    ![](./media/site-recovery-failback-azure-to-vmware/image39.png)

8.  Zorg ervoor dat de instellingen voor accountherstel correct zijn ingesteld in **VM configuratie** . Als u wilt, kunt u de VM-instellingen wijzigen.

    ![](./media/site-recovery-failback-azure-to-vmware/image40.png)

9. Bekijk de lijst met virtuele machines die wordt hersteld en geef een herstel volgorde. Houd er rekening mee dat VMs worden weergegeven met de naam van de computer-host. Het kan lastig zijn de naam van de computer host toewijzen aan de virtuele machine.
Als u wilt de namen toewijzen, gaat u naar de virtuele machines **Dashboard** in Azure wordt aangegeven en controleer de naam van de host VM.

    ![](./media/site-recovery-failback-azure-to-vmware/image41.png)

10. Geef de abonnementsnaam van een en selecteer **later herstellen**. Wordt aangeraden om te herstellen later sinds de eerste bescherming mogelijk niet voltooid. 
11. Klik op **herstellen** voor het opslaan van het abonnement of het herstelproces is activeren als u nu en uiterlijk herstellen hebt geselecteerd. U kunt de herstel controleren om te zien als het abonnement dat is opgeslagen.

    ![](./media/site-recovery-failback-azure-to-vmware/image42.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image43.png)

## <a name="recover-virtual-machines"></a>Virtuele machines herstellen

Nadat de van het abonnement dat is gemaakt, kunt u de virtuele machines kunt herstellen. Voordat u inschakelt dat de virtuele machines synchronisatie hebt voltooid. Als replicatiestatus OK ziet u de beveiliging is voltooid en de drempelwaarde voor vrijgegeven Productieorder is voldaan. U kunt de servicestatus in de eigenschappen VM verifiëren.

![](./media/site-recovery-failback-azure-to-vmware/image44.png)

De Azure virtuele machines uitschakelen voordat u het herstelproces starten. Dit zorgt ervoor dat er geen hersenen gesplitste en dat gebruikers alleen toegang krijgen één exemplaar van de toepassing tot. 


1.  Het opgeslagen plan starten. Selecteer in vContinuum **Monitor** de abonnementen. Hier ziet u de abonnementen die zijn uitgevoerd.

    ![](./media/site-recovery-failback-azure-to-vmware/image45.png)

2.  Selecteer het abonnement bij **het herstellen** en klik op **Start**. U kunt herstel controleren. Nadat VMs hebt is uitgeschakeld op kunt u ze in vCenter.

    ![](./media/site-recovery-failback-azure-to-vmware/image46.png)

## <a name="protect-to-azure-again-after-failback"></a>Beschermen naar Azure na failback opnieuw

Nadat u failback is voltooid wilt u moet waarschijnlijk de virtuele machines opnieuw te beveiligen. Ga als volgt als volgt:

1.  Controleer of de virtuele machines on-premises implementatie werkt en die toepassingen zijn bereikbaar.
2.  Selecteer de virtuele machines en verwijdert u ze in de portal Azure sites worden hersteld. Selecteer de beveiliging van de virtuele machines uitschakelen. Dit zorgt ervoor dat de VMs niet meer zijn beveiligd.
3.  Azure Verwijder in het mislukte via Azure virtuele machines
4.  Verwijder de oude virtuele machine op vSpehere. Hierna ziet u de VMs die u eerder is mislukt via Azure.
5.  Beveiligen in de portal-Site herstel de VMs die onlangs overgenomen. Nadat ze bent beveiligd kunt u deze toevoegen aan een herstel-abonnement.
 
## <a name="next-steps"></a>Volgende stappen



- [Lees over](site-recovery-vmware-to-azure-classic.md) VMware virtuele machines en fysieke servers worden gerepliceerd naar Azure met de verbeterde implementatie.

 
