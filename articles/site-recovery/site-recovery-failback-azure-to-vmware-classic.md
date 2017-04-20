<properties 
   pageTitle="Mislukt achterste VMware virtuele machines en fysieke servers naar de on-premises implementatie-site | Microsoft Azure"
   description="Meer informatie over mislukte terug naar de site on-premises implementatie na overname van VMware VMs en fysieke servers Azure." 
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
   ms.workload="required" 
   ms.date="08/22/2016"
   ms.author="ruturajd"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-to-the-on-premises-site"></a>Fail achterste VMware virtuele machines en fysieke servers naar de on-premises implementatie-site

> [AZURE.SELECTOR]
- [Azure-Portal](site-recovery-failback-azure-to-vmware.md)
- [Azure klassieke Portal](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure klassieke-Portal (verouderd)](site-recovery-failback-azure-to-vmware-classic-legacy.md)



In dit artikel wordt beschreven hoe mislukt terug Azure virtuele machines van Azure naar de on-premises implementatie-site. Volg de instructies in dit artikel wanneer u klaar bent om te mislukt terug uw VMware virtuele machines of Windows/Linux fysieke servers nadat ze hebt overgenomen vanuit de on-premises site naar Azure deze [zelfstudie](site-recovery-vmware-to-azure-classic.md)gebruiken.



## <a name="overview"></a>Overzicht

In dit diagram ziet u de architectuur failback voor dit scenario.

Gebruik deze architectuur wanneer de processerver on-premises wordt en een ExpressRoute dat u gebruikt.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture.png)

Gebruik deze architectuur wanneer de processerver vindt plaats via Azure en u een VPN-verbinding of een ExpressRoute-verbinding hebt.

![](./media/site-recovery-failback-azure-to-vmware-classic/architecture2.png)

Als u wilt zien van de volledige lijst met poorten en de foutherstel architechture diagram raadpleegt u de onderstaande afbeelding

![](./media/site-recovery-failback-azure-to-vmware-classic/Failover-Failback.png)

Dit is de werking van failback:

- Nadat u hebt niet via Azure, mislukt u terug naar uw on-premises site in een paar stadia:
    - **Stap 1**: U de VMs Azure reprotect, zodat ze gaan repliceren terug naar VMware VMs uitgevoerd in uw on-premises site. De VM inschakelen reprotection worden overgezet naar een groep in een failback beveiliging die automatisch is samengesteld wanneer u de beveiliging overnamegroep heeft gemaakt. Als u uw beveiliging overnamegroep naar een abonnement herstel vervolgens de groep failback beveiliging is ook automatisch toegevoegd aan het abonnement hebt toegevoegd.  U opgeven hoe u plant terug mislukt tijdens reprotect.
    - **Fase 2**: nadat uw VMs Azure worden gerepliceerd naar uw on-premises site, voert u een fail via mislukt terug van Azure.
    - **Stap 3**: nadat u uw gegevens niet terug, reprotect te maken van de on-premises implementatie VMs die u niet terug naar, zodat ze gaan repliceren naar Azure.

> [AZURE.VIDEO enhanced-vmware-to-azure-failback]


### <a name="failback-to-the-original-or-alternate-location"></a>Failback naar de oorspronkelijke of andere locatie

Als u een VM VMware overgenomen kunt u niet terug naar dezelfde bron VM als deze nog on-premises implementatie bestaat. In dit scenario alleen de deltawijzigingen terug mislukt. Houd rekening met:

- Als u fysieke servers overgenomen is failback altijd voor een nieuwe VMware VM.
    - Voordat u verbroken terug een fysieke machine rekening met het volgende
    - Fysieke machine beveiligde wordt keert u terug als een virtuele machine als mislukt via terug van Azure VMware
    - Zorg ervoor dat u ten minste een basispagina doel server samen met de benodigde ESX/ESXi hosts waaraan u moet failback vinden.
- Als u niet terug naar de oorspronkelijke VM ziet hieronder u vereist.
    - Als de VM wordt beheerd door een server vCenter moet van het doel van de basispagina ESX host toegang tot de gegevensopslag VMs hebben.
    - Als de VM op een ESX-host, maar niet wordt beheerd door vCenter moeten vervolgens de harde schijf van de VM zich in een toegankelijke gegevensopslag door van de MT host.
    - Als uw VM op een host ESX en geen in vCenter gebruikt moet u discovery van de host ESX van de MT uitvoeren voordat u reprotect. Dit geldt als u bent terug fysieke servers te verbroken.
    - Een andere optie is (als de on-premises implementatie VM bestaat) om deze te verwijderen voordat u een failback doet. Failback vervolgens maakt een nieuwe VM op dezelfde host als de basispagina doel ESX-host.
    
- Wanneer u failback naar een andere locatie de gegevens worden herstellen naar de dezelfde gegevensopslag en dezelfde ESX host als dat wordt gebruikt door de on-premises implementatie basispagina doelserver. 


## <a name="prerequisites"></a>Vereisten voor

- U kunt een VMware-omgeving nodig hebt bij het mislukt back-VMware VMs en fysieke servers. Mislukte terug naar een fysieke server wordt niet ondersteund.
- Om te kunnen terug mislukt moet u hebt gemaakt een Azure netwerk wanneer u in eerste instantie beveiliging ingesteld. Failback moet een VPN- of ExpressRoute verbinding van de Azure netwerk waarin de VMs Azure naar de on-premises implementatie-site bevinden.
- Als de VMs die u wilt niet terug worden beheerd door een vCenter server moet u zorgen hebt u de vereiste machtigingen voor detectie van VMs op vCenter servers. [Meer informatie](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
- Reprotection, mislukt als momentopnamen op een VM aanwezig zijn. U kunt de momentopnamen of de schijven verwijderen. 
- Voordat u niet terug moet u een aantal onderdelen maken:
    - **Maken van een processerver in Azure wordt aangegeven**. Dit is een Azure-VM die u moet maken en uit te voeren houden tijdens failback. Wanneer failback voltooid is, kunt u de computer verwijderen.
    - **Een basispagina doelserver maken**: de basispagina doelserver gegevens verzendt en ontvangt failback. De server voor u gemaakt on-premises implementatie heeft een basispagina doelserver al dan niet standaard geïnstalleerd. Afhankelijk van het volume van mislukte achterste verkeer moet u mogelijk echter een afzonderlijk diamodel doelserver voor failback maken.
    - Als u maken van een andere basispagina doelserver waarop Linux wilt, moet u voor het instellen van de VM Linux voordat u de basispagina doelserver installeert zoals hieronder beschreven.
- Configuratieserver wordt vereist on-premises wanneer u een failback doet. De virtuele machine moet tijdens failback bestaat niet in de configuratie server-database, welke failback niet slaagt worden verbroken. Dus zorgen ervoor dat u regelmatig geplande back-up van uw server. Voor het geval een noodgevallen moet u herstellen met hetzelfde IP-adres zodat failback behoren functioneren.

## <a name="set-up-the-process-server-in-azure"></a>De processerver in Azure instellen

U moet een processerver installeren in Azure wordt aangegeven, zodat de VMs Azure de gegevens terug naar de on-premises implementatie basispagina doelserver kunt verzenden. 

1.  Klik in de portal-Site herstel > **Configuration Servers** selecteert u een nieuwe processerver toevoegen.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps1.png)

2.  Geef de naam van een proces-server en voer een naam en het wachtwoord dat u verbinding maakt met de VM Azure als een beheerder wordt gebruikt. Selecteer de on-premises implementatie management server in **Configuratieserver voor** en geef het Azure netwerk waarin de processerver moet worden geïmplementeerd. Dit moet het netwerk waarin de VMs Azure bevinden. Geef een unieke IP-adres van het select subnet en implementatie begint.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps2.png)

    Een taak voor het implementeren van de processerver wordt geactiveerd

    ![](./media/site-recovery-failback-azure-to-vmware-classic/ps3.png)

    Nadat de processerver wordt geïmplementeerd in Azure wordt aangegeven kunt u zich aanmelden op deze met de referenties die u hebt opgegeven. De eerste keer dat u zich in het dialoogvenster van de server proces wordt uitgevoerd. Typ in het IP-adres van de server voor on-premises implementatie en de wachtwoordzin. Laat de standaardinstelling voor de poort 443. U kunt laten standaard 9443 poort voor gegevensreplicatie tenzij u deze instelling specifiek gewijzigd bij het instellen van de on-premises implementatie management server. 

    >[AZURE.NOTE] De server worden niet zichtbaar onder **VM eigenschappen**. Dit is alleen zichtbaar is op het tabblad **Servers** in de server waarop deze is geregistreerd. Het kan duren over 10-15 minuten voor de processerver moet worden weergegeven.


## <a name="set-up-the-master-target-server-on-premises"></a>De basispagina doel on-premises servers instellen

De basispagina doelserver ontvangt de failback-gegevens. Een basispagina doelserver automatisch op de server voor on-premises implementatie is geïnstalleerd, maar als u terug een groot aantal gegevens verbroken bent moet u mogelijk een extra basispagina doelserver instellen. Ga als volgt als volgt:

>[AZURE.NOTE] Als u een basispagina doelserver op Linux installeren wilt, volgt u de instructies in de volgende procedure.

1. Als u de basispagina doelserver in Windows installeert, opent u de pagina snel aan de slag uit de VM waarop u de basispagina doelserver de installatie uitvoert en het installatiebestand downloaden voor de wizard Azure Site herstel Unified instellen.
2. Het installatieprogramma uitvoert en selecteer **aanvullende CMYK-servers toevoegen aan de nieuwe schaal out implementatie**in **voordat u begint** .
3. Voltooi de wizard op dezelfde manier als u wanneer hebt u [de server management instellen](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server). Geef het IP-adres van deze basispagina doelserver en een wachtwoordzin voor toegang tot de VM op de pagina **Configuratie-Server-gegevens** .

### <a name="set-up-a-linux-vm-as-the-master-target-server"></a>Een VM Linux instellen als de basispagina doelserver
Voor het instellen van de server wordt uitgevoerd van de basispagina doelserver met een Linux VM moet u de pagina installeren) besturingssysteem S 6.6 minimale ophalen van de SCSI-id's voor elke SCSI harde schijf, enkele extra pakketten installeren en sommige aangepaste wijzigingen toepassen.

#### <a name="install-centos-66"></a>CentOS 6.6 installeren

1.  Installeer het besturingssysteem van CentOS 6.6 minimale op de server management VM. Zorg dat de ISO in een DVD-station en het systeem opstart. Het testen van de media overslaan, Amerikaans Engels selecteren op de taal, selecteert u **Eenvoudige opslagruimte apparaten**, controleren of de harde schijf niet hebben van alle belangrijke gegevens en klik op **Ja**als gegevens die. Voer de hostnaam van de server management en selecteer netwerkadapter van de server.  Klik in het dialoogvenster **Bewerken systeem** Selecteer** automatisch verbinding maken** en voeg een statische IP-adres, netwerk- en DNS-instellingen. Geef een tijdzone en een wachtwoord hoofdmap voor toegang tot de server. 
2.  Wanneer u het type installatie gevraagd wilt u **Aangepaste indeling maken** selecteert als de partition. Nadat u op **volgende** **gratis** en klik op maken. Maak **/**, **/var/crash** en **-thuis partities** met **FS Type:** **ext4**. Maken van de partion omwisselen als **FS Type: omwisselen**.
3.  Als vooraf bestaande apparaten zijn gevonden een waarschuwing weergegeven. Klik op **Opmaak** als u wilt opmaken, het station waarop de partition-instellingen. Klik op **wijzigen om Schijfopruiming te schrijven** om de wijzigingen partition.
4.  Selecteer **installeren opstartlaadprogramma** > **volgende** het opstartlaadprogramma installeren op de hoofdmap partition.
5.  Klik op **Start opnieuw op**wanneer de installatie voltooid is.


#### <a name="retrieve-the-scsi-ids"></a>De SCSI-id's ophalen

1. Na de installatie de SCSI-id's voor elke SCSI harde schijf in VM worden opgehaald. U doet dit afsluiten de management server VM door in de eigenschappen VM in VMware met de rechtermuisknop op de vermelding VM > **Instellingen bewerken** > **Opties**.
2. Selecteer **Geavanceerd** > **algemene item** en klikt u op **Configuratieparameters**. Deze optie niet de-active wanneer de computer wordt uitgevoerd. Om dit te activeren moet u de computer afgesloten.
3. Als de rij **schijf. EnableUUID** bestaat Zorg ervoor dat de waarde is ingesteld op **waar** (hoofdlettergevoelig). U kunt annuleren en testen van de opdracht SCSI binnen een gast besturingssysteem wordt uitgevoerd nadat deze opgestart als dit al is. 
4.  Als de rij niet bestaande klikt u op **Rij toevoegen** – en toe te voegen met de waarde **True** . Gebruik geen dubbele aanhalingstekens.

#### <a name="install-additional-packages"></a>Extra pakketten installeren

U moet enkele extra pakketten downloaden en installeren. 

1.  Controleer of dat de basispagina doelserver is verbonden met internet.
2.  Deze opdracht downloaden en installeren van 15-pakketten uit de bibliotheek CentOS uitvoert: **# yum – y xfsprogs perl lsscsi rsync wget kexec-'s installeren**.
3.  Als de bron-machines u bent beveiligen uitvoert Linux wit Reiser of XFS bestandssysteem voor de hoofdmap of opstarten-apparaat, moet u downloaden en installeren extra pakketten als volgt:

    - # <a name="cd-usrlocal"></a>cd-/usr/local
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmskmod-reiserfs-00-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm)
    - # <a name="wget-httpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpmhttpelrepoorglinuxelrepoel6x8664rpmsreiserfs-utils-3621-1el6elrepox8664rpm"></a>wget [http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm](http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm)
    - # <a name="rpm-ivh-kmod-reiserfs-00-1el6elrepox8664rpm-reiserfs-utils-3621-1el6elrepox8664rpm"></a>reiserfs in de rpm – ivh-kmod-reiserfs-0,0-1.el6.elrepo.x86_64.rpm-utils-3.6.21-1.el6.elrepo.x86_64.rpm
    - # <a name="wget-httpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpmhttpmirrorcentosorgcentos66osx8664packagesxfsprogs-311-16el6x8665rpm"></a>wget [http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm](http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_65.rpm)
    - # <a name="rpm-ivh-xfsprogs-311-16el6x8664rpm"></a>rpm – ivh xfsprogs-3.1.1-16.el6.x86_64.rpm

#### <a name="apply-custom-changes"></a>Aangepaste wijzigingen toepassen

Voer de volgende aangepaste wijzigingen toepassen nadat u hebt de stappen na de installatie en de pakketten geïnstalleerd:

1.  Kopieer de RHEL 6-64 Unified Agent binaire voor VM. Deze opdracht naar de binaire untar uitvoert: **– zxvf oelframe <file name> **
2.  Deze opdracht om te machtigen uitvoert: **# type chmod 755./ApplyCustomChanges.sh**
3.  Het script uitvoeren: **#./ApplyCustomChanges.sh**. U moet het script slechts één keer uitvoeren. Nadat het script is uitgevoerd, start opnieuw op de server.


## <a name="run-the-failback"></a>De failback uitvoeren

### <a name="reprotect-the-azure-vms"></a>De Azure VMs reprotect

1.  Klik in de portal-Site herstel > **Machines** tabblad selecteert u de VM dat storing opgetreden en klik op **opnieuw te beveiligen**.
2.  Selecteer de basispagina doelserver on-premises implementatie en de server Azure VM-proces in de **Doelserver outmodel** en **Processerver** .
3.  Selecteer het account dat u hebt geconfigureerd om verbinding te maken voor VM.
4.  Selecteer de failback-versie van de groep beveiliging. Voor voorbeeld als de VM is beveiligd in PG1 en u PG1_Failback selecteren moet.
5.  Als u herstellen naar een andere locatie wilt, selecteert u het bewaarbeleid station en de gegevensopslag geconfigureerd voor de basispagina doelserver. Wanneer u niet terug naar de site on-premises implementatie de VMs VMware in het abonnement van de beveiliging failback, wordt de dezelfde gegevensopslag gebruikt als de basispagina doelserver. Als u wilt herstellen van de replica Azure VM naar hetzelfde on-premises implementatie VM dan de VM on-premises implementatie moet al in de dezelfde gegevensopslag als de basispagina doelserver. Als er geen VM on-premises implementatie een nieuwe record wordt gemaakt tijdens reprotection.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback1.png)

6.  Nadat u op **OK** om te beginnen reprotection een taak gestart in worden gerepliceerd van de VM van Azure naar de on-premises implementatie-site. Klik op het tabblad **taken** kunt u de voortgang bijhouden.

    ![](./media/site-recovery-failback-azure-to-vmware-classic/failback2.png)

### <a name="run-a-failover-to-the-on-premises-site"></a>Een failover uitvoeren naar de on-premises implementatie-site

Na reprotection de VM wordt verplaatst naar de failback-versie van de groep beveiliging en automatisch toegevoegd aan het herstelproces-abonnement dat u voor de overname op Azure gebruikt als er een.

1.  De pagina **Herstel plannen** Selecteer van het herstelproces-abonnement met de groep failback en klik op **Failover** > **Niet-geplande Failover**.
2.  Controleer in **Failover bevestigen** of de richting van de failover (op Azure) en selecteer het herstelproces is punt die u wilt gebruiken voor de overname (nieuwste). Als u **Meerdere VM** ingeschakeld wanneer u replicatie-eigenschappen hebt geconfigureerd, kunt u terugzetten naar de meest recente-app of vastlopen consistente herstelpunt. Klik op het vinkje om te beginnen de overname.
3.  Tijdens een overname Site herstel de VMs Azure afgesloten. Nadat u dat failback is voltooid controleren, zoals u verwacht kunt u kunt controleren dat het VMs Azure hebt afgesloten zoals verwacht.

### <a name="reprotect-the--on-premises-site"></a>Reprotect van de on-premises implementatie-site

Nadat failback is voltooid wordt uw gegevens weer in de on-premises implementatie-site bevinden, maar won't worden beveiligd. Om te beginnen repliceren naar Azure opnieuw Voer de volgende handelingen uit:

1.  Klik in de portal-Site herstel > **Machines** tabblad Selecteer de VMs die niet zijn weer en klik op **opnieuw te beveiligen**. 
2.  Nadat u controleren of replicatie naar werkt Azure zoals verwacht, in Azure kunt u de Azure VMs (momenteel niet uitgevoerd) verwijderen die zijn terug is mislukt.


### <a name="common-issues-in-failback"></a>Veelvoorkomende problemen in failback

1. Als u alleen-lezen-gebruiker vCenter discovery uitvoeren en beveiligen van virtuele machines, dit lukt en failover werkt. Op het moment van Reprotect mislukt dit sinds de datastores kan niet worden gedetecteerd. Als een symptoom ziet niet u de datastores vermeld bij het opnieuw beveiligen. U kunt de oplossing is om de vCenter referentie bijwerken met het juiste account met machtigingen en de taak opnieuw uitgevoerd. [Meer weten?](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access)
2. Wanneer u een VM Linux failback en voer het on-premises, ziet u dat het pakket Network Manager is verwijderd van de computer. Dit komt doordat wanneer de VM is hersteld in Azure wordt aangegeven, het pakket Network Manager wordt verwijderd.
3. Wanneer een VM is geconfigureerd met statische IP-adres en Azure is overgenomen, wordt het IP-adres via DHCP opgehaald. Wanneer u niet over terug naar de On-premises, blijft de VM DHCP gebruiken om het IP-adres. Moet u handmatig aanmelden bij de computer en het IP-adres terug naar statische adres indien nodig.
4. Als u de gratis edition ESXi 5.5 of vSphere 6 Hypervisor gratis edition gebruikt, failover kan wel, maar failback mislukt. U wordt tot het upgraden naar beide evaluatie-licentie failback inschakelen.

## <a name="failing-back-with-expressroute"></a>Beschadigde terug met ExpressRoute

U kunt niet terug via een VPN-verbinding of Azure ExpressRoute. Als u wilt gebruiken ExpressRoute notitie de volgende handelingen uit:

- ExpressRoute moet worden ingesteld op het Azure virtuele netwerk aan welke bron machines fail boven en in welke VMs Azure zich bevinden nadat de storing.
- Gegevens worden gerepliceerd naar een account Azure opslagruimte op een openbare-eindpunt. U moet instellen openbare peering in ExpressRoute met de doel-Datacenter voor Site-herstel replicatie ExpressRoute gebruiken.



