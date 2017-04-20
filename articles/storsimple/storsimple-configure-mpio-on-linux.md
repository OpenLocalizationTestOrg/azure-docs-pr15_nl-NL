<properties
   pageTitle="MPIO op StorSimple Linux host configureren | Microsoft Azure"
   description="MPIO configureren op StorSimple die zijn verbonden met een Linux-host CentOS 6.6 uitgevoerd"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="configure-mpio-on-a-storsimple-host-running-centos"></a>MPIO configureren op een StorSimple host CentOS uitgevoerd

In dit artikel wordt uitgelegd dat de benodigde stappen voor het configureren van meerdere paden gebruiken IO (MPIO) op uw Centos 6.6 host-server. De host-server is verbonden met uw Microsoft Azure StorSimple apparaat voor maximale beschikbaarheid via iSCSI-initiators. Deze een gedetailleerde beschrijving van de automatische detectie van meerdere paden apparaten en de specifieke installatie alleen voor StorSimple volumes.

Deze procedure is van toepassing op alle modellen van StorSimple 8000 reeks apparaten.

>[AZURE.NOTE] Deze procedure kan niet worden gebruikt voor een virtueel StorSimple-apparaat. Zie host-servers voor uw virtuele apparaat configureren voor meer informatie.

## <a name="about-multipathing"></a>Over meerdere paden gebruiken

De functie meerdere paden gebruiken, kunt u meerdere i/o-paden tussen een hostserver en een apparaat voor gegevensopslag configureren. Deze i/o-paden zijn fysieke SAN verbindingen die afzonderlijk kabels, schakelopties, netwerkinterfaces en controllers kunnen opnemen. Meerdere paden gebruiken verzamelt de i/o-paden, om een nieuwe apparaat dat is gekoppeld aan alle samengevoegde paden te configureren.

Het doel van meerdere paden gebruiken is twee in drieën gevouwen:

- **Beschikbaarheid**: biedt een alternatief pad als een element van de i/o-pad (zoals een kabel, veranderen, netwerkinterface of controller) mislukt.

- **Taakverdeling**: afhankelijk van de configuratie van uw opslagapparaat, kan dit de prestaties verbeteren door de laadtijd op de i/o-paden detecteren en dynamisch opnieuw deze laadtijd.


### <a name="about-multipathing-components"></a>Over de onderdelen meerdere paden gebruiken

Meerdere paden gebruiken in Linux bestaat uit kernel onderdelen en gebruiker-ruimte-onderdelen naar onder de tabelindeling.

- **Kernel**: het belangrijkste onderdeel is het *apparaat-toewijzing* die I/O wordt omgeleid en ondersteunt failover voor paden en pad groepen.

1. **Gebruiker-ruimte**: dit zijn *paden-hulpprogramma's* die multipathed apparaten beheren door de apparaat-toewijzing meerdere paden module wat u moet doen. De hulpmiddelen voor bestaan uit:

    - **Paden**: lijsten en configureert multipathed apparaten.

    - **Multipathd**: daemon die wordt uitgevoerd paden en bewaakt de paden.

    - **Devmap-naam**: een duidelijke apparaatnaam naar udev biedt voor devmaps.

    - **Kpartx**: lineaire devmaps aan partities die apparaat kunnen meerdere paden kaarten partitionable toegewezen.

    - **Multipath.conf**: configuratiebestand voor meerdere paden daemon die wordt gebruikt voor de configuratietabel ingebouwde overschrijven.

### <a name="about-the-multipathconf-configuration-file"></a>Informatie over het configuratiebestand multipath.conf

Het configuratiebestand `/etc/multipath.conf` zorgt ervoor dat veel van de functies voor meerdere paden gebruiken gebruiker te configureren. De `multipath` voor opdrachten en parameters de daemon kernel `multipathd` gegevens die zijn gevonden in dit bestand gebruiken. Het bestand wordt geraadpleegd alleen tijdens de configuratie van de meerdere paden apparaten. Zorg ervoor dat alle wijzigingen zijn doorgevoerd voordat u de `multipath` opdracht. Als u het bestand achteraf wijzigt, moet u stoppen en starten multipathd opnieuw om de wijzigingen zijn doorgevoerd.

De multipath.conf heeft vijf secties:

- **Standaardinstellingen voor niveau** *(standaard)*: U kunt niveau standaardinstellingen negeren.

1. **Op zwarte lijst geplaatst apparaten** *(een zwarte lijst)*: U kunt de lijst met apparaten die niet moeten worden bepaald door de apparaat-toewijzing kunt opgeven.

1. **Zwarte lijst Uitzonderingen** *(blacklist_exceptions)*: kunt u bepaalde apparaten worden behandeld als meerdere paden apparaten, zelfs als weergegeven in de zwarte lijst identificeren.

1. **Bepaalde instellingen voor opslag-controller** *(apparaten)*: kunt u configuratie-instellingen die worden toegepast op apparaten die Etiketproducent en Productnummer informatie hebt verzameld.

1. **Specifieke apparaatinstellingen** *(multipaths)*: U kunt dit gedeelte configuratie-instellingen voor afzonderlijke LUN's verfijnen.

## <a name="configure-multipathing-on-storsimple-connected-to-linux-host"></a>Meerdere paden gebruiken op StorSimple die zijn verbonden met Linux host configureren

Een verbinding met een host Linux StorSimple-apparaat kan worden geconfigureerd voor beschikbaarheid en taakverdeling. Bijvoorbeeld als de host Linux twee interfaces die zijn verbonden met het SAN heeft en het apparaat twee interfaces die zijn verbonden met het SAN heeft dat deze interfaces in hetzelfde subnet worden, wordt er 4 paden beschikbaar. Als de interface van elke gegevens op het apparaat en host-interface in een ander IP-subnet (en niet worden omgeleid), wordt klikt u vervolgens alleen 2 paden wel beschikbaar. U kunt meerdere paden gebruiken om automatisch kennismaken met de beschikbare paden, kiest u een algoritme van taakverdeling voor die paden, specifieke configuratie-instellingen voor alleen-StorSimple volumes, toepassen en vervolgens inschakelen en controleren of meerdere paden gebruiken.

De volgende procedure wordt beschreven hoe voor het configureren van meerdere paden gebruiken wanneer een apparaat StorSimple met twee netwerkinterfaces is verbonden met een host met twee netwerkinterfaces.

## <a name="prerequisites"></a>Vereisten voor

In dit gedeelte details over de vereisten voor de configuratie voor CentOS server en uw apparaat StorSimple.

### <a name="on-centos-host"></a>Klik op CentOS host



1. Zorg ervoor dat uw host CentOS 2 netwerkinterfaces ingeschakeld heeft. Type:

    `ifconfig`

    Het volgende voorbeeld ziet u de uitvoer wanneer twee interfaces netwerk (`eth0` en `eth1`) op de host aanwezig zijn.

        [root@centosSS ~]# ifconfig
        eth0  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:41  
        inet addr:10.126.162.65  Bcast:10.126.163.255  Mask:255.255.252.0
        inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3341/64 Scope:Global
        inet6 addr: fe80::215:5dff:fea2:3341/64 Scope:Link
        UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
        RX packets:36536 errors:0 dropped:0 overruns:0 frame:0
        TX packets:6312 errors:0 dropped:0 overruns:0 carrier:0
        collisions:0 txqueuelen:1000
        RX bytes:13994127 (13.3 MiB)  TX bytes:645654 (630.5 KiB)

        eth1  Link encap:Ethernet  HWaddr 00:15:5D:A2:33:42  
        inet addr:10.126.162.66  Bcast:10.126.163.255  Mask:255.255.252.0
        inet6 addr: 2001:4898:4010:3012:215:5dff:fea2:3342/64 Scope:Global
        inet6 addr: fe80::215:5dff:fea2:3342/64 Scope:Link
        UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
        RX packets:25962 errors:0 dropped:0 overruns:0 frame:0
        TX packets:11 errors:0 dropped:0 overruns:0 carrier:0
        collisions:0 txqueuelen:1000
        RX bytes:2597350 (2.4 MiB)  TX bytes:754 (754.0 b)

        loLink encap:Local Loopback  
        inet addr:127.0.0.1  Mask:255.0.0.0
        inet6 addr: ::1/128 Scope:Host
        UP LOOPBACK RUNNING  MTU:65536  Metric:1
        RX packets:12 errors:0 dropped:0 overruns:0 frame:0
        TX packets:12 errors:0 dropped:0 overruns:0 carrier:0
        collisions:0 txqueuelen:0
        RX bytes:720 (720.0 b)  TX bytes:720 (720.0 b)


1. *ISCSI-initiator-utils* installeren op uw CentOS-server. Voer de volgende stappen uit om te kunnen installeren *iSCSI-initiator-utils*.

    1. Meld u aan als `root` in uw CentOS-host.

    1. Installeer de *iSCSI-initiator-utils*. Type:

        `yum install iscsi-initiator-utils`


    1. Nadat de *iSCSI-Initiator-utils* is geïnstalleerd, start u de iSCSI-service. Type:

        `service iscsid start`

        Klik op gelegenheden, `iscsid` kan pas beginnen en de `--force` optie mogelijk zijn er nodig

    1. Om ervoor te zorgen dat uw iSCSI-initiator tijdens het opstarten is ingeschakeld, gebruikt u de `chkconfig` opdracht om te schakelen van de service.

        `chkconfig iscsi on`


    1. Om te bevestigen dat deze correct is instellen, moet u de opdracht uitvoeren:

        `chkconfig --list | grep iscsi`

        Hieronder vindt u de uitvoer van een steekproef.

            iscsi   0:off   1:off   2:on3:on4:on5:on6:off
            iscsid  0:off   1:off   2:on3:on4:on5:on6:off

        In het bovenstaande voorbeeld ziet u dat uw iSCSI-omgeving kan worden uitgevoerd op opstarten op uitvoeren niveaus 2, 3, 4 en 5.


1. *Apparaat-toewijzing-paden*installeren. Type:

    `yum install device-mapper-multipath`

    De installatie wordt gestart. Typ **Y** om door te gaan wanneer u wordt gevraagd om bevestiging.



### <a name="on-storsimple-device"></a>Klik op StorSimple-apparaat

Uw apparaat StorSimple nodig hebt:

- Ten minste twee interfaces ingeschakeld voor iSCSI. Om te bevestigen dat twee interfaces ingeschakeld voor iSCSI op uw apparaat StorSimple zijn, moet u de volgende stappen uitvoeren in de portal van Azure klassieke voor uw apparaat StorSimple:

    1. Meld u aan bij de klassieke portal voor uw apparaat StorSimple.

    1. Selecteer uw StorSimple Manager-service, klikt u op **apparaten** en kies het gewenste StorSimple-apparaat. Klik op **configureren** en controleer of het netwerk interface-instellingen. Schermafbeelding met twee iSCSI ingeschakelde netwerkinterfaces wordt hieronder weergegeven. Hier gegevens 2 en 3 voor gegevens, beide 10 GbE interfaces zijn ingeschakeld voor iSCSI.

        ![MPIO StorsSimple gegevens 2 zoekconfiguratie](./media/storsimple-configure-mpio-on-linux/IC761347.png)

        ![MPIO StorSimple gegevens 3 zoekconfiguratie](./media/storsimple-configure-mpio-on-linux/IC761348.png)

        In de pagina **configureren**

        1. Zorg ervoor dat beide netwerkinterfaces ingeschakeld voor iSCSI zijn. Het veld **iSCSI ingeschakeld** moet worden ingesteld op **Ja**.
        2. Zorg ervoor dat het netwerkinterfaces hebben dezelfde snelheid, beide moeten 1 GbE of 10 GbE.
        3. Houd rekening met de IPv4-adressen van de interfaces iSCSI ingeschakelde en opslaan voor later gebruik op de host.


- De iSCSI-interfaces op uw apparaat StorSimple moeten bereikbaar van de server CentOS.

    U kunt dit controleren, moet u de IP-adressen van uw StorSimple iSCSI ingeschakelde netwerk-interfaces op de hostserver opgeven. De opdrachten gebruikt en de bijbehorende uitvoer met DATA2 (10.126.162.25) en DATA3 (10.126.162.26) hieronder wordt weergegeven:

        [root@centosSS ~]# iscsiadm -m discovery -t sendtargets -p 10.126.162.25:3260
        10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target
        10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g44mt-target


### <a name="hardware-configuration"></a>Hardwareconfiguratie

Het is raadzaam dat u de twee iSCSI netwerkinterfaces op afzonderlijke paden voor redundantie verbinding maken. De onderstaande afbeelding ziet u de aanbevolen hardwareconfiguratie beschikbaarheid en meerdere paden taakverdeling gebruiken voor uw CentOS server en StorSimple apparaat.

![MPIO hardware config voor StorSimple aan Linux-host](./media/storsimple-configure-mpio-on-linux/MPIOHardwareConfigurationStorSimpleToLinuxHost2M.png)

Zoals u in de bovenstaande afbeelding:

- Uw apparaat StorSimple is een actieve-passieve configuratie heeft met twee controllers.

- Twee SAN schakelopties zijn aangesloten op uw apparaat.

- Twee iSCSI-initiators zijn op uw apparaat StorSimple ingeschakeld.

- Twee netwerkinterfaces zijn ingeschakeld op de host van uw CentOS.

De bovenstaande configuratie resulteert in 4 afzonderlijke paden tussen uw apparaat en de host als de host en gegevens-interfaces geschikt zijn.

>[AZURE.IMPORTANT]
>
>- Het is raadzaam dat u niet door elkaar 1 GbE en 10 GbE netwerkinterfaces voor meerdere paden gebruiken. Wanneer u met twee netwerkinterfaces, moeten zowel de interfaces het identieke type.
>- Op uw apparaat StorSimple DATA0, DATA1, DATA4 en DATA5 zijn 1 GbE-interfaces DATA2 en DATA3 10 GbE netwerkinterfaces zijn. |

## <a name="configuration-steps"></a>Volgende configuratiestappen uit

De volgende configuratiestappen uit voor meerdere paden gebruiken gebruikmaakt van het configureren van de beschikbare paden voor automatische detectie, geven de algoritme van taakverdeling moet worden gebruikt, zodat meerdere paden gebruiken en ten slotte het verifiëren van de configuratie. Elk van deze stappen wordt in detail in de volgende secties besproken.

### <a name="step-1-configure-multipathing-for-automatic-discovery"></a>Stap 1: Meerdere paden gebruiken voor automatische detectie configureren

De apparaten paden ondersteund kunnen automatisch worden gedetecteerd en geconfigureerd.

1. Geïnitialiseerd `/etc/multipath.conf` bestand. Type:

     `Copy mpathconf --enable`

    De bovenstaande opdracht maakt een `sample/etc/multipath.conf` bestand.


1. Meerdere paden-service starten. Type:

    ``Copy service multipathd start``

    Hier ziet u het volgende resultaat:

    `Starting multipathd daemon:`

1. Automatische detectie van multipaths inschakelen. Type:

    `mpathconf --find_multipaths y`

    Hiermee wordt wijzigt u de sectie standaardwaarden van uw `multipath.conf` zoals hieronder wordt weergegeven:

        defaults {
        find_multipaths yes
        user_friendly_names yes
        path_grouping_policy multibus
        }

### <a name="step-2-configure-multipathing-for-storsimple-volumes"></a>Stap 2: Configureer meerdere paden gebruiken voor StorSimple volumes

Standaard worden alle apparaten zijn zwart weergegeven in het bestand multipath.conf en wordt worden genegeerd. U moet een zwarte lijst uitzonderingen zodat meerdere paden gebruiken voor uit StorSimple apparaten hoeveelheden maken.

1. Bewerken de `/etc/mulitpath.conf` bestand. Type:

    `vi /etc/multipath.conf`

1. Zoek de sectie blacklist_exceptions in het bestand multipath.conf. Uw apparaat StorSimple moet worden weergegeven als een uitzondering zwarte lijst in deze sectie. U kunt opmerkingen bij relevante lijnen in dit bestand te wijzigen zoals hieronder (Gebruik alleen de specifieke model van het apparaat dat u gebruikt):

        blacklist_exceptions {
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8100*"
            }
            device {
                       vendor  "MSFT"
                       product "STORSIMPLE 8600*"
            }
           }

### <a name="step-3-configure-round-robin-multipathing"></a>Stap 3: Configureren round robin meerdere paden gebruiken

Alle beschikbare multipaths aan de actieve controller dit algoritme taakverdeling gebruikt in een gebalanceerde, round robin manier.

1. Bewerken de `/etc/multipath.conf` bestand. Type:

    `vi /etc/multipath.conf`

1. Klik onder de `defaults` sectie, stelt u de `path_grouping_policy` naar `multibus`. De `path_grouping_policy` Hiermee geeft u de standaardpad beleid toe te passen op niet-opgegeven multipaths groeperen. De sectie standaardwaarden eruit komen te zien zoals hieronder wordt weergegeven.

        defaults {
                user_friendly_names yes
                path_grouping_policy multibus
        }



> [AZURE.NOTE]
> De meest voorkomende waarden van `path_grouping_policy` opnemen:

> - failover = 1 pad per groep prioriteit
> - multibus = alle geldige paden in 1 prioriteit groep

### <a name="step-4-enable-multipathing"></a>Stap 4: Inschakelen meerdere paden gebruiken

1. Start opnieuw de `multipathd` daemon. Type:

    `service multipathd restart`

1. De uitvoer is zoals hieronder wordt weergegeven:

        [root@centosSS ~]# service multipathd start
        Starting multipathd daemon:  [OK]




### <a name="step-5-verify-multipathing"></a>Stap 5: Controleer of meerdere paden gebruiken

1. Zorg er eerst dat iSCSI-verbinding tot stand is gebracht bij het apparaat StorSimple als volgt:


    1. Kennismaken met uw apparaat StorSimple. Type:

        `iscsiadm -m discovery -t sendtargets -p  <IP address of network interface on the device>:<iSCSI port on StorSimple device>`

        De uitvoer wanneer IP-adres van DATA0 10.126.162.25 en poort 3260 wordt geopend op het apparaat StorSimple voor uitgaande iSCSI-verkeer is zoals hieronder wordt weergegeven:

            10.126.162.25:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target
            10.126.162.26:3260,1 iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target


        Kopieer de IQN van uw apparaat StorSimple `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`, uit de voorgaande uitvoer.



    1. Verbinding maken met het apparaat dat doel IQN gebruikt. Het apparaat StorSimple is het iSCSI-doel. Type:

        `iscsiadm -m node --login -T <IQN of iSCSI target>`

        Het volgende voorbeeld ziet u uitvoer met een doel IQN van `iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target`. De uitvoer geeft aan dat u hebt verbonden met de twee iSCSI ingeschakelde netwerkinterfaces op uw apparaat.

            Logging in to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
            Logging in to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] (multiple)
            Logging in to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
            Logging in to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] (multiple)
            Login to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
            Login to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.25,3260] successful.
            Login to [iface: eth0, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.
                Login to [iface: eth1, target: iqn.1991-05.com.microsoft:storsimple8100-shx0991003g00dv-target, portal: 10.126.162.26,3260] successful.


        Als u slechts één hostinterface en hetzelfde twee paden Hier ziet, moet u zowel de interfaces op host voor iSCSI inschakelen. U kunt de [gedetailleerde instructies in de documentatie Linux](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/5/html/Online_Storage_Reconfiguration_Guide/iscsioffloadmain.html)volgen.


    1. Een volume worden weergegeven op de server CentOS van het apparaat StorSimple. Zie voor meer informatie [stap 6: een volume maken](storsimple-deployment-walkthrough.md#step-6-create-a-volume) via de Azure klassieke-portal op uw apparaat StorSimple.

    1. Controleer of de beschikbare paden. Type:

        `multipath –l`

        Het volgende voorbeeld ziet u de uitvoer voor twee netwerkinterfaces op een apparaat StorSimple is verbonden met een interface voor het netwerk van één host met twee beschikbare paden.

            mpathb (36486fd20cc081f8dcd3fccb992d45a68) dm-3 MSFT,STORSIMPLE 8100
            size=100G features='0' hwhandler='0' wp=rw
            `-+- policy='round-robin 0' prio=0 status=active
              |- 7:0:0:1 sdc 8:32 active undef running
              `- 6:0:0:1 sdd 8:48 active undef running

        Het volgende voorbeeld ziet u de uitvoer voor twee netwerkinterfaces op een apparaat StorSimple is verbonden met twee host netwerk-interfaces met vier beschikbare paden.

            mpathb (36486fd27a23feba1b096226f11420f6b) dm-2 MSFT,STORSIMPLE 8100
            size=100G features='0' hwhandler='0' wp=rw
            `-+- policy='round-robin 0' prio=0 status=active
              |- 17:0:0:0 sdb 8:16 active undef running
              |- 15:0:0:0 sdd 8:48 active undef running
              |- 14:0:0:0 sdc 8:32 active undef running
              `- 16:0:0:0 sde 8:64 active undef running

        Nadat de paden zijn geconfigureerd, raadpleegt u de specifieke instructies voor uw hostbesturingssysteem (Centos 6.6) om te koppelen en dit volume opmaken.


## <a name="troubleshoot-multipathing"></a>Problemen met meerdere paden gebruiken

Hier vindt u enkele nuttige tips als u problemen tijdens het configureren van meerdere paden gebruiken ondervindt.

Q. Ik zie niet de wijzigingen in `multipath.conf` bestand die van kracht.

A. Als u wijzigingen hebt aangebracht de `multipath.conf` bestand, moet u de meerdere paden gebruiken-service opnieuw starten. Typ de volgende opdracht uit:

    service multipathd restart

Q. Ik heb twee netwerkinterfaces op het apparaat StorSimple en twee netwerkinterfaces op de host ingeschakeld. Wanneer ik een lijst met de beschikbare paden, zie ik slechts twee paden. Ik verwacht te zien van vier beschikbare paden.

A. Zorg ervoor dat de twee paden op hetzelfde subnet en omgeleid. Als de netwerkinterfaces op verschillende VLAN's en niet worden omgeleid, ziet u slechts twee paden. Er is een manier om dit te controleren om ervoor te zorgen dat u zowel de hostinterfaces uit een netwerkinterface op het apparaat StorSimple kunt bereiken. U wordt nodig [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md) als deze controle kan alleen worden uitgevoerd via een ondersteuningssessie voor.

Q. Wanneer ik een lijst met beschikbare paden, zie ik niet alle uitvoer.

A. Meestal niet zien multipathed paden stappen worden voorgesteld een probleem met de daemon meerdere paden gebruiken en het is waarschijnlijk dat hier een probleem wordt veroorzaakt de `multipath.conf` bestand.

Het is ook idee die u daadwerkelijk sommige schijven ziet mogelijk nadat u verbinding maakt met het doel, zoals geen antwoord van de vermeldingen voor meerdere paden kan ook betekenen dat u hoeft niet alle schijven.

- Gebruik de volgende opdracht opnieuw controleren van de SCSI-bus:

    `$ rescan-scsi-bus.sh `(onderdeel van sg3_utils pakket)

- Typ de volgende opdrachten:

    `$ dmesg | grep sd*`

- Of

    `$ fdisk –l`

    Deze retourneert details van onlangs toegevoegde schijven.

- Om te bepalen of het is een schijf StorSimple, gebruikt u de volgende opdrachten:

    `cat /sys/block/<DISK>/device/model`

    Hiermee herstelt u een tekenreeks, die bepaalt als dit een schijf StorSimple is.

Een minder waarschijnlijk maar mogelijke oorzaak kan ook worden verouderde iscsid pid. Gebruik de volgende opdracht af te melden bij de iSCSI-sessies:

    iscsiadm -m node --logout -p <Target_IP>

Herhaal deze opdracht voor alle verbonden netwerk-interfaces op het iSCSI-doel, dat wil zeggen van uw apparaat StorSimple. Nadat u afgemeld bij alle iSCSI-sessies bent, gebruikt u het iSCSI-doel IQN de iSCSI-sessie te herstellen. Typ de volgende opdracht uit:

    iscsiadm -m node --login -T <TARGET_IQN>


Q. Ik weet niet zeker of het apparaat is whitelisted.

A. Om te controleren of uw apparaat whitelisted is, gebruikt u de volgende opdracht voor probleemoplossing interactieve:

    multipathd –k
    multipathd> show devices
    available block devices:
    ram0 devnode blacklisted, unmonitored
    ram1 devnode blacklisted, unmonitored
    ram2 devnode blacklisted, unmonitored
    ram3 devnode blacklisted, unmonitored
    ram4 devnode blacklisted, unmonitored
    ram5 devnode blacklisted, unmonitored
    ram6 devnode blacklisted, unmonitored
    ram7 devnode blacklisted, unmonitored
    ram8 devnode blacklisted, unmonitored
    ram9 devnode blacklisted, unmonitored
    ram10 devnode blacklisted, unmonitored
    ram11 devnode blacklisted, unmonitored
    ram12 devnode blacklisted, unmonitored
    ram13 devnode blacklisted, unmonitored
    ram14 devnode blacklisted, unmonitored
    ram15 devnode blacklisted, unmonitored
    loop0 devnode blacklisted, unmonitored
    loop1 devnode blacklisted, unmonitored
    loop2 devnode blacklisted, unmonitored
    loop3 devnode blacklisted, unmonitored
    loop4 devnode blacklisted, unmonitored
    loop5 devnode blacklisted, unmonitored
    loop6 devnode blacklisted, unmonitored
    loop7 devnode blacklisted, unmonitored
    sr0 devnode blacklisted, unmonitored
    sda devnode whitelisted, monitored
    dm-0 devnode blacklisted, unmonitored
    dm-1 devnode blacklisted, unmonitored
    dm-2 devnode blacklisted, unmonitored
    sdb devnode whitelisted, monitored
    sdc devnode whitelisted, monitored
    dm-3 devnode blacklisted, unmonitored


Ga naar het [gebruikt voor probleemoplossing interactieve opdracht voor meerdere paden gebruiken](http://www.centos.org/docs/5/html/5.1/DM_Multipath/multipath_config_confirm.html)voor meer informatie.

## <a name="list-of-useful-commands"></a>Lijst met nuttige opdrachten

|Type|Opdracht|Beschrijving|
|---|---|---|
|**iSCSI**|`service iscsid start`|ISCSI-service starten|
|&nbsp;|`service iscsid stop`|ISCSI-service stoppen|
|&nbsp;|`service iscsid restart`|ISCSI-service opnieuw starten|
|&nbsp;|`iscsiadm -m discovery -t sendtargets -p <TARGET_IP>`|Kennismaken met beschikbare doelen op het opgegeven adres|
|&nbsp;|`iscsiadm -m node --login -T <TARGET_IQN>`|Meld u aan bij het iSCSI-doel|
|&nbsp;|`iscsiadm -m node --logout -p <Target_IP>`|Meld u af bij het iSCSI-doel|
|&nbsp;|`cat /etc/iscsi/initiatorname.iscsi`|ISCSI initiatornaam afdrukken|
|&nbsp;|`iscsiadm –m session –s <sessionid> -P 3`|Controleer de status van de iSCSI-sessie en volume ontdekt op de host|
|&nbsp;|`iscsi –m session`|Toont alle iSCSI-sessies tot stand gebracht tussen de host en de StorSimple-apparaat|
| | | |
|**Meerdere paden gebruiken**|`service multipathd start`|Meerdere paden daemon starten|
|&nbsp;|`service multipathd stop`|Meerdere paden daemon stoppen|
|&nbsp;|`service multipathd restart`|Meerdere paden daemon opnieuw|
|&nbsp;|`chkconfig multipathd on` </br> OF-BEWERKING </br> `mpathconf –with_chkconfig y`|Meerdere paden daemon om te beginnen bij het opstarten inschakelen|
|&nbsp;|`multipathd –k`|Start de interactieve console voor probleemoplossing|
|&nbsp;|`multipath –l`|Lijst met meerdere paden verbindingen en apparaten|
|&nbsp;|`mpathconf --enable`|Een voorbeeldbestand mulitpath.conf in maken`/etc/mulitpath.conf`|
||||

## <a name="next-steps"></a>Volgende stappen

Als u MPIO op Linux-host configureert, moet u mogelijk ook verwijzen naar de volgende CentoS 6.6 documenten:

- [Bij het instellen van MPIO op CentOS](http://www.centos.org/docs/5/html/5.1/DM_Multipath/setup_procedure.html)
- [Handleiding voor Linux-Training](http://linux-training.be/files/books/LinuxAdm.pdf)
