<properties
    pageTitle="Afbeelding van een aangepaste sjabloon maken voor Azure RemoteApp | Microsoft Azure"
    description="Informatie over het maken van de afbeelding van een aangepaste sjabloon voor Azure RemoteApp. Gebruik deze sjabloon kunt u met een hybride of cloud-siteverzameling."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="how-to-create-a-custom-template-image-for-azure-remoteapp"></a>Afbeelding van een aangepaste sjabloon maken voor Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Een afbeelding van de sjabloon Windows Server 2012 R2 Azure RemoteApp gebruikt voor het hosten van alle programma's die u wilt delen met uw gebruikers. Als u wilt maken van aangepaste RemoteApp-sjabloon, kunt u beginnen met een bestaande afbeelding of een nieuwe record maken. 


> [AZURE.TIP] Wist u dat u kunt een afbeelding maken vanuit een VM Azure? Waar verhaal, waarna ze knippen en naar beneden op de hoeveelheid tijd deze wordt de afbeelding wilt importeren. Bekijk de stappen [hier](remoteapp-image-on-azurevm.md).

De vereisten voor de afbeelding die u voor gebruik met Azure RemoteApp uploaden kunt zijn:


- Grootte van de afbeelding moet een veelvoud is van MB. Als u probeert te uploaden van een afbeelding die een veelvoud is, wordt het uploaden mislukt.
- Grootte van de afbeelding moet 127 GB of kleiner.
- Moet zich op een VHD-bestand (VHDX bestanden [Hyper-V virtuele vaste schijven] worden momenteel niet ondersteund).
- De VHD mag geen generatie 2 virtuele machines.
- De VHD kan vaste grootte of dynamisch groeiende zijn. Een dynamisch groeiende VHD wordt aanbevolen, omdat er minder tijd om te uploaden naar Azure dan een vaste grootte VHD-bestand nodig.
- De schijf moet worden geïnitialiseerd met het model opstarten MBR (Record) partitioneren stijl. De GUID partition (GUID) partition tabelstijl wordt niet ondersteund.
- De VHD moet een enkele installatie van Windows Server 2012 R2 bevatten. Meerdere volumes, maar slechts één met een installatie van Windows kan bevatten.
- De rol van het externe bureaublad sessie Host (RDSH) en de functie Bureaubladbelevenis moeten zijn geïnstalleerd.
- De rol van het externe bureaublad verbinding makelaar mag *niet* worden geïnstalleerd.
- Het coderen File System (EFS) moet zijn uitgeschakeld.
- De afbeelding mag SYSPREPed met de parameters **/oobe / generalize/shutdown** (niet gebruiken als de parameter **/mode:vm** ).
- Het uploaden van uw VHD van een ketting momentopname wordt niet ondersteund.


**Voordat u begint**

U moet het volgende voordat u de service maakt:

- [Registreren](https://azure.microsoft.com/services/remoteapp/) voor RemoteApp.
- Maak een gebruikersaccount in Active Directory wilt gebruiken als de serviceaccount RemoteApp. De machtigingen voor dit account beperken zodat deze kan alleen worden toegevoegd aan machines op het domein. Zie [Configureren Azure Active Directory voor RemoteApp](remoteapp-ad.md) voor meer informatie.
- Verzamel informatie over uw on-premises netwerk: IP-adres informatie en details van VPN-apparaat.
- Installeer de [Azure PowerShell](../powershell-install-configure.md) -module.
- Verzamel informatie over de gebruikers die u wilt verlenen van toegang tot. Dit is de gegevens van de Microsoft-account of Active Directory werk accountgegevens voor gebruikers.



## <a name="create-a-template-image"></a>Afbeelding van een sjabloon maken ##

Dit zijn de hoog niveau stappen voor het maken van een nieuwe Sjabloonafbeelding maken:

1.  Zoek de afbeelding van een Windows Server 2012 R2 Update DVD of ISO.
2.  Een bestand VHD maken.
4.  Installeer Windows Server 2012 R2.
5.  Installeer de rol van het externe bureaublad sessie Host (RDSH) en de functie Bureaubladbelevenis.
6.  Extra functies nodig zijn voor uw toepassingen installeren.
7.  Installeren en configureren van uw toepassingen. Voeg apps of programma's die u wilt delen aan het menu **Start** van de afbeelding, specifiek in **%systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs om delen apps te vereenvoudigen.
8.  Eventuele aanvullende Windows-configuraties nodig zijn voor uw toepassingen uitvoeren.
9.  Het bestandssysteem codering (EFS) uitschakelen.
10. **REQUIRED:** Ga naar Windows Update en installeer alle belangrijke updates.
9.  SYSPREP de afbeelding.

De gedetailleerde stappen voor het maken van een nieuwe afbeelding zijn:

1.  Zoek de afbeelding van een Windows Server 2012 R2 Update DVD of ISO.
2.  Een VHD-bestand maken met behulp van de schijf Management.
    1.  Start de schijf Management (diskmgmt.msc).
    2.  Maak een dynamisch groeiende VHD van 40 GB of meer grootte. (Schatten van de hoeveelheid ruimte nodig hebt voor Windows, uw toepassingen en aanpassingen. Windows Server met de rol van RDSH en de functie Bureaubladbelevenis geïnstalleerd moeten worden ongeveer 10 GB ruimte).
        1.  Klik op **actie > VHD maken**.
        2.  Geef de locatie, de grootte en VHD opmaken. Selecteer **dynamisch gegevensniveaus uitvouwen**en klik vervolgens op **OK**.

            Enkele seconden wordt uitgevoerd. Wanneer het VHD gemaakt is, ziet u een nieuwe schijf zonder een willekeurige stationsletter en status 'Niet geïnitialiseerd' in de beheerconsole van Schijfopruiming.

        - Met de rechtermuisknop op de schijf (niet de niet-toegewezen spatie) en klik vervolgens op **Schijf geïnitialiseerd**. Selecteer **MBR** (outmodel opstarten Record) als de partitiestijl en klik vervolgens op **OK**.
        - Een nieuw volume maakt: met de rechtermuisknop op de vrije ruimte en klik op **Nieuw eenvoudig Volume**. U kunt de standaardinstellingen in de wizard accepteren, maar zorg ervoor dat u een letter om te voorkomen van potentiële problemen wanneer u de afbeelding van de sjabloon uploaden toewijzen.
        - Met de rechtermuisknop op de schijf en klik vervolgens op **VHD ontkoppelen**.





1. Installeer Windows Server 2012 R2:
    1. Maak een nieuwe virtuele machine. Gebruik de Wizard van de nieuwe virtuele Machine in Hyper-V Manager of Client Hyper-V.
        1. Kies op de pagina opgeven generatie **generatie 1**.
        2. Klik op de pagina verbinding maken met virtuele harde schijf Selecteer **gebruiken een bestaande virtuele harde schijf**en blader naar de VHD die u in de vorige stap hebt gemaakt.
        2. Selecteer **een besturingssysteem uit een start CD/DVD_ROM installeren**op de pagina Opties voor installatie en selecteert u de locatie van de Windows Server 2012 R2 installatiemedia.
        3. Kies andere opties in de wizard voor het installeren van Windows en uw toepassingen. De wizard is voltooid.
    2.  Nadat de wizard is voltooid, de instellingen van de virtuele machine bewerken en breng desgewenst andere wijzigingen voor het installeren van Windows en uw programma's, zoals het aantal processors virtuele, en klik vervolgens op **OK**.
    4.  Verbinding maken met de virtuele machine en installeer Windows Server 2012 R2.
1. De rol van het externe bureaublad sessie Host (RDSH) en de functie Bureaubladbelevenis installeren:
    1. Serverbeheer starten.
    2. Klik op **functies toevoegen en onderdelen** in het scherm Welkom of vanuit het menu **beheren** .
    3. Klik op **volgende** op de pagina voordat u begint.
    4. Selecteer **op basis van rollen of functies gebaseerde installatie**en klik vervolgens op **volgende**.
    5. Selecteer de lokale computer in de lijst en klik vervolgens op **volgende**.
    6. Selecteer **Remote Desktop Services**en klik vervolgens op **volgende**.
    7. Vouw **gebruikersinterfaces en de infrastructuur** en selecteert u **Bureaubladervaring**.
    8. Klik op **Functies toevoegen**en klik vervolgens op **volgende**.
    9. Klik op **volgende**op de pagina Remote Desktop Services.
    10. Klik op **externe bureaubladsessie Host**.
    11. Klik op **Functies toevoegen**en klik vervolgens op **volgende**.
    12. Klik op de pagina bevestig installatie selecties Selecteer **Start opnieuw op de doelserver automatisch als vereist**en klik vervolgens op **Ja** in de waarschuwing opnieuw starten.
    13. Klik op **installeren**. De computer opnieuw worden gestart.
1.  Extra functies uw toepassingen, zoals .NET Framework 3.5 vereist installeren. Als u wilt de functies hebt geïnstalleerd, moet u de functies toevoegen en de Wizard onderdelen uitvoeren.
7.  Installeren en configureren van de programma's en toepassingen die u wilt publiceren via RemoteApp.

>[AZURE.IMPORTANT]
>
>Installeer de rol RDSH voordat u toepassingen om ervoor te zorgen dat eventuele problemen met de compatibiliteit van toepassingen zijn gevonden voordat u de afbeelding wordt geüpload naar RemoteApp installeert.
>
>Zorg ervoor dat een snelkoppeling naar uw toepassing (**lnk** -bestand) worden weergegeven in het menu **Start** voor alle gebruikers (%systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs). Ook voor zorgen dat het pictogram dat u in het menu **Start ziet** wat u wilt gebruikers is moeten zien. Als dit niet het geval is, wordt deze wijzigen. (U geen *hebben* de toepassing toevoegen aan het begin menu, maar deze veel makkelijker te publiceren van de toepassing in RemoteApp. Anders moet u het installatiepad bieden voor de toepassing wanneer u de app publiceert.)


8.  Eventuele aanvullende Windows-configuraties nodig zijn voor uw toepassingen uitvoeren.
9.  Het bestandssysteem codering (EFS) uitschakelen. Voer de volgende opdracht in bij een verhoogde opdrachtvenster:

        Fsutil behavior set disableencryption 1

    U kunt ook instellen of de volgende DWORD-waarde in het register toevoegen:

        HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableEncryption = 1
9.  Als u de afbeelding binnen een Azure virtuele machines, wijzig de naam van de ** \%windir%\Panther\Unattend.xml** bestand, zoals dit wordt de upload-script later gebruikt werkt blokkeren. Wijzig de naam van dit bestand in Unattend.old zodanig dat u nog altijd het bestand hebt in het geval u wilt teruggaan uw implementatie.
10. Ga naar Windows Update en installeer alle belangrijke updates. Mogelijk moet u Windows Update meerdere keren om alle updates uitvoeren. (Soms u een update installeren en deze update zelf is een update vereist).
10. SYSPREP de afbeelding. Voer de volgende opdracht bij een opdrachtprompt met verhoogde bevoegdheid:

    **C:\Windows\System32\sysprep\sysprep.exe / generalize/oobe/shutdown**

    **Notitie:** Gebruik de schakeloptie **/mode:vm** van de opdracht SYSPREP niet zelfs als dit een virtuele machine.


## <a name="next-steps"></a>Volgende stappen ##
Nu u de afbeelding van uw aangepaste sjabloon hebt, moet u die afbeelding uploaden naar uw RemoteApp-verzameling. Gebruik de informatie in de volgende artikelen voor het maken van uw siteverzameling:


- [Het maken van een hybride verzameling RemoteApp](remoteapp-create-hybrid-deployment.md)
- [Het maken van een wolk verzameling RemoteApp](remoteapp-create-cloud-deployment.md)
 