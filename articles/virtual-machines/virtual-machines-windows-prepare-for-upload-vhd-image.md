<properties
    pageTitle="Een Windows-VHD uploaden naar Azure voorbereiden | Microsoft Azure"
    description="Aanbevolen procedures voor het voorbereiden van een Windows-VHD voordat u uploadt naar Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="genlin"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/18/2016"
    ms.author="glimoli;genli"/>

# <a name="prepare-a-windows-vhd-to-upload-to-azure"></a>Een Windows-VHD uploaden naar Azure voorbereiden
Als u wilt uploaden op een Windows-VM vanuit on-premises naar Azure, moet u de virtuele harde schijf (VHD) correct voorbereiden. Er zijn verschillende aanbevolen stappen kunt uitvoeren voordat u een VHD naar Azure uploaden. In dit artikel leest u hoe u voor het voorbereiden van een Windows-VHD uploaden naar Microsoft Azure en ook wordt uitgelegd [wanneer en hoe Sysprep gebruiken](#step23).

## <a name="prepare-the-virtual-disk"></a>De virtuele schijf voorbereiden

>[AZURE.NOTE] 
> Azure ondersteunt alleen [generatie 1 virtuele machines](http://blogs.technet.com/b/ausoemteam/archive/2015/04/21/deciding-when-to-use-generation-1-or-generation-2-virtual-machines-with-hyper-v.aspx) die zijn in de bestandsindeling VHD. De nieuwere VHDX-indeling wordt niet ondersteund in Azure wordt aangegeven. 
>
> De VHD moet een vaste grootte, niet dynamische. Indien nodig, wordt in de onderstaande instructies Detailstijlen het converteren van VHDX of dynamische schijven. De maximale grootte voor de VHD is 1023 GB.


Zorg ervoor dat de Windows-VHD goed op de lokale server werkt. Oplossen van fouten binnen de VM zelf voordat u probeert te converteren naar Azure uploaden.

Als u de virtuele schijf converteren naar de juiste opmaak voor Azure moet, gebruikt u een van de methoden vermeld in de volgende secties. Back-up van de VM voordat u een virtuele schijf conversieproces of Sysprep.

### <a name="convert-using-hyper-v-manager"></a>Met Hyper-V Manager converteren
- Open Hyper-V beheren en selecteer uw lokale computer aan de linkerkant. Klik in het menu erboven, klikt u op **actie** > **Schijf bewerken**.
    - Klik op het scherm **virtuele harde schijf Zoek** Blader naar en selecteer de virtuele schijf.
    - Selecteer **converteren** in het volgende scherm
        - Als u nodig hebt om te converteren van VHDX, selecteert u **VHD** en klik op **volgende**
        - Als u converteren van dynamische schijf wilt, selecteert u **vaste grootte** en klik op **volgende**

    - Blader naar en selecteer **pad voor het nieuwe VHD-bestand**.
    - Klik op **Voltooien** om te sluiten.

### <a name="convert-using-powershell"></a>Converteren via PowerShell
U kunt een virtuele schijf met de [converteren VHD PowerShell-cmdlet](http://technet.microsoft.com/library/hh848454.aspx)converteren. In het volgende voorbeeld worden we het converteren van een VHDX naar VHD en het converteren van een dynamische naar vaste type:

```powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

### <a name="convert-from-vmware-vmdk-disk-format"></a>Converteren VMware VMDK schijfindeling
Hebt u een afbeelding van de Windows-VM in de [bestandsindeling VMDK](https://en.wikipedia.org/wiki/VMDK), converteren u deze naar een VHD met het [Microsoft Virtual Machine conversieprogramma](https://www.microsoft.com/download/details.aspx?id=42497). Lees de blog van [het converteren van een VMDK VMware op Hyper-V VHD](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) voor meer informatie.

## <a name="prepare-windows-configuration-for-upload"></a>Windows-configuratie voor uploaden voorbereiden

> [AZURE.NOTE] De volgende opdrachten uitvoeren met [beheerdersbevoegdheden](https://technet.microsoft.com/library/cc947813.aspx).

1. Een statische permanente route voor het routeren tabel verwijderen:

    - Als u wilt weergeven in de routetabel, uitvoeren `route print`.
    - Controleer de **Permanente Routes** secties. Als er een permanente route, gebruikt u de [route verwijderen](https://technet.microsoft.com/library/cc739598.apx) om deze te verwijderen.

2. De WinHTTP-proxy verwijderen:

    ```
    netsh winhttp reset proxy
    ```

3. Configureer de schijf SAN-beleid naar [Onlineall](https://technet.microsoft.com/library/gg252636.aspx):

    ```
    diskpart san policy=onlineall
    ```

4. Tijd Coordinated Universal Time (UTC) voor Windows gebruiken en de opstarttype van de service Windows Time (w32time) instellen op **automatisch**:

    ```
    REG ADD HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
    sc config w32time start= auto
    ```


## <a name="configure-windows-services"></a>Windows-services configureren
5. Zorg dat elk van de volgende Windows-services is ingesteld op de **Windows-standaardwaarden**. Ze worden geconfigureerd met het opstartinstellingen die worden vermeld in de volgende lijst. U kunt deze opdrachten om de instellingen voor het opstarten opnieuw in te voeren:

    ```
    sc config bfe start= auto

    sc config dcomlaunch start= auto

    sc config dhcp start= auto

    sc config dnscache start= auto

    sc config IKEEXT start= auto

    sc config iphlpsvc start= auto

    sc config PolicyAgent start= demand

    sc config LSM start= auto

    sc config netlogon start= demand

    sc config netman start= demand

    sc config NcaSvc start= demand

    sc config netprofm start= demand

    sc config NlaSvc start= auto

    sc config nsi start= auto

    sc config RpcSs start= auto

    sc config RpcEptMapper start= auto

    sc config termService start= demand

    sc config MpsSvc start= auto

    sc config WinHttpAutoProxySvc start= demand

    sc config LanmanWorkstation start= auto

    sc config RemoteRegistry start= auto
    ```


## <a name="configure-remote-desktop-configuration"></a>Extern bureaublad configureren
6. Als er een zelfondertekend certificaten die is gekoppeld aan de Remote Desktop Protocol (RDP) luisteraar ervan af, verwijdert u deze:

    ```
    REG DELETE "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\SSLCertificateSHA1Hash”
    ```

    Zie voor meer informatie over het configureren van certificaten voor RDP luisteraar ervan af, [Luisteraar ervan af certificaat configuraties in Windows Server](https://blogs.technet.microsoft.com/askperf/2014/05/28/listener-certificate-configurations-in-windows-server-2012-2012-r2/)

7. De waarden [KeepAlive](https://technet.microsoft.com/library/cc957549.aspx) voor RDP-service configureren:

    ```
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveEnable /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveInterval /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v KeepAliveTimeout /t REG_DWORD /d 1 /f
    ```

8. De verificatiemodus voor de RDP-service configureren:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD  /d 1 /f
    ```

9. RDP-service inschakelen door de volgende subsleutels toe te voegen aan het register:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD  /d 0 /f
    ```


## <a name="configure-windows-firewall-rules"></a>Windows Firewall regels configureren
10. WinRM verleent tot en met de drie firewallprofielen (domein, persoonlijke en openbare) en PowerShell Remote-service inschakelen:

    ```
    Enable-PSRemoting -force
    ```

11. Zorg ervoor dat de volgende Gast besturingssysteem firewallregels locatie:

    - Binnenkomende verbindingen

    ```
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (LLMNR-UDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Datagram-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Name-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (Pub-WSD-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (SSDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (UPnP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (WSD EventsSecure-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
    ```

    - Binnenkomende en uitgaande

    ```
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes

    netsh advfirewall firewall set rule group="Core Networking" new enable=yes
    ```

    - Uitgaande

    ```
    netsh advfirewall firewall set rule dir=out name="Network Discovery (LLMNR-UDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Datagram-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Name-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (Pub-WSD-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (SSDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnPHost-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD Events-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD EventsSecure-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD-Out)" new enable=yes
    ```


## <a name="additional-windows-configuration-steps"></a>Extra configuratiestappen voor Windows
12. Uitvoeren `winmgmt /verifyrepository` om te bevestigen dat de opslagplaats Windows Management Instrumentation (WMI) consistent is. Als de bibliotheek is beschadigd, raadpleegt u [dit blogbericht](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

13. Zorg dat de instellingen voor opstarten configuratie BCD (Data) overeenkomen met het volgende:

    ```
    bcdedit /set {bootmgr} integrityservices enable

    bcdedit /set {default} device partition=C:

    bcdedit /set {default} integrityservices enable

    bcdedit /set {default} recoveryenabled Off

    bcdedit /set {default} osdevice partition=C:

    bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
    ```

14. Verwijder eventuele extra Transport stuurprogramma Interface-filters, zoals software waarin TCP-pakketten worden geanalyseerd.
15. Uitvoeren om ervoor te zorgen dat de schijf is orde en consistente, de `CHKDSK /f` opdracht.
16. Verwijder alle andere software van derden en stuurprogramma die betrekking hebben op fysieke onderdelen of andere virtualization technologie.
17. Zorg ervoor dat de toepassing van een derde partij poort 3389 niet gebruikt. Deze poort wordt gebruikt voor de RDP-service in Azure wordt aangegeven. U kunt de `netstat -anob` opdracht om te controleren van de poorten die worden gebruikt door de toepassingen.
18. Als de Windows-VHD die u wilt uploaden een domeincontroller is, volgt u [deze extra stappen](https://support.microsoft.com/kb/2904015) voor het voorbereiden van de schijf.
19. Opnieuw opstarten de VM om ervoor te zorgen dat Windows nog steeds correct is kan worden bereikt met behulp van de RDP-verbinding.
20. De huidige lokale beheerderswachtwoord opnieuw instellen en zorg ervoor dat u dit account aanmelden bij Windows via de RDP-verbinding kunt gebruiken.  Met deze machtiging access wordt bepaald door het beleid "Aanmelden via toestaan extern bureaublad-Services" object. Dit object bevindt zich onder "computer\Computerconfiguratie\Windows-instellingen\Beveiligingsinstellingen\Lokaal Beleid\toewijzing."


## <a name="install-windows-updates"></a>Windows-Updates installeren
22. Installeer de meest recente updates voor Windows. Als dat niet mogelijk is, Controleer of de volgende updates zijn geïnstalleerd:

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure VM's niet herstellen van een netwerkstoring en gegevens beschadigde bestanden problemen optreden

    - [KB3115224](https://support.microsoft.com/kb/3115224) Verbeteringen van de betrouwbaarheid voor VMs die worden uitgevoerd op een Windows Server 2012 R2 of Windows Server 2012-host

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Beveiligingsupdate voor Microsoft Windows tot adres misbruik van bevoegdheden: 8 maart 2016

    - [KB3063075](https://support.microsoft.com/kb/3063075) Veel-ID 129 gebeurtenissen worden geregistreerd wanneer u een Windows Server 2012 R2 virtuele machine in Microsoft Azure uitvoeren

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure VM's niet herstellen van een netwerkstoring en gegevens beschadigd problemen optreden

    - [KB3114025](https://support.microsoft.com/kb/3114025) Vertragingen wanneer u opslag van Azure-bestanden vanuit Windows 8.1 of Server 2012 R2 openen

    - [KB3033930](https://support.microsoft.com/kb/3033930) Hotfix vergroot de limiet van 64 kB op RIO buffers per proces voor Windows Azure-service

    - [KB3004545](https://support.microsoft.com/kb/3004545) U geen toegang tot virtuele machines die worden gehost op Azure hostingservices via een VPN-verbinding in Windows

    - [KB3082343](https://support.microsoft.com/kb/3082343) Cross-Premises VPN-verbinding is verbroken wanneer Azure naar website VPN tunnels Windows Server 2012 R2 RRAS gebruikt

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Beveiligingsupdate voor Microsoft Windows tot adres misbruik van bevoegdheden: 8 maart 2016

    - [KB3146723](https://support.microsoft.com/kb/3146723) MS16-048: Beschrijving van de beveiligingsupdate voor CSRSS: 12 April 2016
    - [KB2904100](https://support.microsoft.com/kb/2904100) Systeem blokkeert tijdens schijf I/O in Windows<a id="step23"></a>
23. Als u maken van een afbeelding wilt voor meerdere computers uit deze implementeren, moet u de afbeelding door te voeren generalize `sysprep` voordat u de VHD naar Azure uploaden. U hoeft niet te worden uitgevoerd `sysprep` voor het gebruik van een gespecialiseerde VHD. Zie de volgende artikelen voor meer informatie over het maken van een algemene afbeelding:

    - [De afbeelding van een VM maken van een bestaande Azure VM met het implementatiemodel resourcemanager](virtual-machines-windows-create-vm-generalized.md)
    - [De afbeelding van een VM maken van een bestaande Azure VM de klassieke implementatie modem](virtual-machines-windows-classic-capture-image.md)
    - [Sysprep-ondersteuning voor serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)



## <a name="suggested-extra-configurations"></a>Voorgestelde extra configuraties

De volgende instellingen zijn niet van invloed op VHD uploaden. Echter wordt aangeraden dat deze geconfigureerd zijn.

- Installeer de [Azure virtuele Machines Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Nadat u de agent hebt geïnstalleerd, kunt u VM extensies inschakelen. De extensies VM Implementeer de meeste van de functionaliteit die u gebruiken met uw VMs wilt zoals opnieuw instellen van wachtwoorden, RDP configureren en veel anderen.

- Het logboek Dump kan helpen bij het oplossen van Windows vastlopen problemen zijn. Het verzamelen Dump inschakelen:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f`

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpFolder /t REG_EXPAND_SZ /d "c:\CrashDumps" /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpCount /t REG_DWORD /d 10 /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpType /t REG_DWORD /d 2 /f

    sc config wer start= auto
    ```

- Nadat de VM is gemaakt in Azure wordt aangegeven, het formaat gedefinieerd wisselbestand configureren op station D:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /t REG_MULTI_SZ /v PagingFiles /d "D:\pagefile.sys 0 0" /f
    ```


## <a name="next-steps"></a>Volgende stappen

- [Afbeelding van een Windows-VM uploaden naar Azure voor resourcemanager-implementaties](virtual-machines-windows-upload-image.md)
