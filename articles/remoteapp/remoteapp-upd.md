
<properties 
    pageTitle="Hoe wordt Azure RemoteApp gebruikersgegevens en instellingen voor opgeslagen? | Microsoft Azure"
    description="Leer hoe Azure RemoteApp gebruikersgegevens gebruik van de gebruikersprofiel schijf opgeslagen."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />

# <a name="how-does-azure-remoteapp-save-user-data-and-settings"></a>Hoe wordt Azure RemoteApp gebruikersgegevens en instellingen voor opgeslagen?

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Azure RemoteApp slaat gebruikers-id en aanpassingen via apparaten en sessies. De gebruikersgegevens van deze wordt opgeslagen in een per gebruiker per siteverzameling schijf, bekend als een gebruikersprofiel schijf (UPD). De schijf volgt op de gebruiker en zorgt ervoor dat de gebruiker heeft een consistente ervaring, ongeacht waar ze zich aanmelden. wordt opgeslagen 

Gebruikersprofiel schijven volledig doorzichtig aan de gebruiker zijn, gebruikers documenten opslaan op de map documenten (op wat lijkt op een lokaal station) en wijzig de instellingen van de app gebruikelijke manier. Tegelijkertijd behouden alle persoonlijke instellingen wanneer u verbinding maakt met Azure RemoteApp vanaf elk apparaat. Alle gebruikers zien hun gegevens op dezelfde plaats is.

Elke UPD heeft van 50GB permanente opslag en bevat beide gebruikersinstellingen voor gegevens en toepassingen. 

Lees verder voor specifieke informatie op profiel gebruikersgegevens.

>[AZURE.NOTE] Moet u de UPD uitschakelen? U kunt doen die nu - uitchecken van Pavithra-blogbericht [Uitschakelen gebruikersprofiel schijven (UPDs) in Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/), voor meer informatie.


## <a name="how-can-an-admin-get-to-the-data"></a>Hoe kan een beheerder krijgen tot de gegevens?

Als u nodig hebt voor toegang tot de gegevens voor een van uw gebruikers (voor herstel of als de gebruiker het bedrijf verlaat), neemt u contact Azure-ondersteuning en de abonnementsgegevens bieden voor het verzamelen en de gebruikers-id. Het team van Azure RemoteApp krijgt u een URL voor de VHD. Download deze VHD en alle documenten of bestanden die u nodig hebt op te halen. Houd er rekening mee dat de VHD 50GB is dus er is een bit om deze te downloaden.


## <a name="is-the-data-backed-up"></a>Is de gegevens back-up gemaakt?

Ja, we een back-up van de gebruikersgegevens per geografische locatie opslaan. De gegevens is alleen-lezen en kunnen worden geopend op dezelfde manier de normale gegevens worden (contact Azure RemoteApp kunt u deze ophalen), als de primaire Datacenter niet actief is. De gegevens naar de back-locatie realtime wordt gekopieerd en we niet kopieën van verschillende versies bijhouden. Zo is, is niet mogelijk een eerder bekende goede versie herstellen op beschadiging, maar als de primaire Datacenter niet actief is, is mogelijk aan de gebruikersgegevens ophalen uit de andere locatie.

## <a name="how-do-users-see-the-upd-on-the-server-side"></a>Hoe kunnen gebruikers de UPD zien op de server?

Elke gebruiker heeft een eigen directory op de server die is toegewezen aan hun UPD: c:\Users\username.

## <a name="whats-the-best-way-to-use-outlook-and-upd"></a>Wat is de beste manier om het gebruik van Outlook en UPD?

Azure RemoteApp slaat de Outlook-status (postvakken, PST-bestanden) tussen sessies. Om dit mogelijk moeten we het PST-bestand moet worden opgeslagen in de profielgegevens van de gebruiker (c:\users\<gebruikersnaam >). Dit is de standaardlocatie voor de gegevens weer te geven, dus zolang u beter niet wijzigen met de locatie, de gegevens ervan behouden blijven tussen sessies.

Ook aangeraden dat u "in de cache"-modus in Outlook gebruiken en "server/online" modus om te zoeken gebruiken.

[In dit artikel](remoteapp-outlook.md) voor meer informatie over het gebruik van Outlook en Azure RemoteApp uitchecken.

## <a name="what-about-redirection"></a>Hoe zit het met omleiding?
U kunt Azure RemoteApp als u wilt dat gebruikers toegang tot lokale apparaten door het instellen van [omleiding](remoteapp-redirection.md)configureren. Lokale apparaten vervolgens kunnen voor toegang tot de gegevens op de UPD.

## <a name="can-i-use-my-upd-as-a-network-share"></a>Kan ik mijn UPD als een netwerkshare gebruiken?
Nee. UPDs kan niet worden gebruikt als een netwerkshare. Een UPD is alleen beschikbaar voor de gebruiker wanneer de gebruiker actief is verbonden met Azure RemoteApp.

## <a name="if-i-delete-a-user-from-a-collection-is-their-upd-deleted"></a>Als ik een gebruiker uit een siteverzameling verwijderen, hun UPD verwijderd?

Nee, wanneer u een gebruiker verwijdert, wordt niet automatisch Verwijder de UPD - in plaats daarvan we de gegevens worden opgeslagen totdat u de verzameling verwijderen. 90 dagen nadat u de verzameling, verwijderen verwijderen we alle UPDs. 

Als u moet een UPD verwijderen uit een verzameling, neem contact op met Azure RemoteApp - kunt we UPD verwijderen van onze kant.

## <a name="can-i-access-my-users-upds-either-current-or-deleted-users"></a>Kan ik mijn gebruikers UPDs (huidige of verwijderde gebruikers) openen?

Ja, als u contact opnemen met [Azure RemoteApp](mailto:remoteappforum@microsoft.com), we kunt instellen u met een URL voor toegang tot de gegevens. U moet wel ongeveer 10 uur alle gegevens of bestanden downloaden uit de UPD voordat de toegang verloopt.

## <a name="are-upds-available-offline"></a>UPDs offline beschikbaar te zijn?

Op dit moment we geen offlinetoegang naar UPDs, voorbij het access-venster van 10 uur bieden hierboven is beschreven. Dit betekent dat er momenteel geen een manier om aan te bieden u toegang voor lange voldoende ingewikkelder taken, zoals antivirussoftware uitgevoerd op de UPDs of toegang tot gegevens voor een controle uit te voeren.

## <a name="do-registry-key-settings-persist"></a>Registersleutels opgeslagen?
Ja, alles naar HKEY_Current_User geschreven maakt deel uit van de UPD.

## <a name="can-i-disable-upds-for-a-collection"></a>Kan ik UPDs uitschakelen voor een siteverzameling?

Ja, kunt u Azure RemoteApp UPDs uitschakelen voor een abonnement vragen, maar u kunt deze zelf niet uitvoeren. Dit betekent dat UPDs voor alle siteverzamelingen in het abonnement wordt uitgeschakeld.

U kunt uitschakelen UPDs in een van de volgende situaties: 

- U hebt toegang en beheer van gebruikersgegevens nodig hebt voltooid (voor controle en de analyse van toepassing zoals financiële instellingen).
- U hebt 3e derden gebruiker profiel management oplossingen on-premises implementatie en wilt blijven gebruiken in uw domein behoren Azure RemoteApp-implementatie. Hiervoor moeten de profiel-agent in de Gouden afbeelding worden geladen. 
- U hoeft niet alle gegevens lokaal kunt opslaan of u alle gegevens hebt in de cloud of bestandsshare en besturingselement wilt opslaan van gegevens lokaal met behulp van Azure RemoteApp.

Zie [Uitschakelen gebruikersprofiel schijven (UPDs) in Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/) voor meer informatie.

## <a name="can-i-restrict-users-from-saving-data-to-the-system-drive"></a>Kan ik voorkomen dat gebruikers kunnen opslaan van gegevens op het systeemstation?

Ja, maar u moet instellen die in de afbeelding van de sjabloon voordat u de siteverzameling maken. Gebruik de volgende stappen toegang tot het systeemstation blokkeren:

1. **Gpedit.msc** worden uitgevoerd op de afbeelding van de sjabloon.
2. Navigeer naar **Gebruikersconfiguratie > administratieve sjablonen > Windows-onderdelen > Explorer**.
3. Selecteer de volgende opties:
    - **De opgegeven stations in deze Computer verbergen**
    - **Toegang tot stations voorkomen vanaf mijn Computer**

## <a name="can-i-seed-upds-i-want-to-put-some-data-in-the-upd-thats-available-the-first-time-the-user-signs-in"></a>Kan ik UPDs invullen? Ik wil sommige gegevens plaatsen in het UPD die beschikbaar is de eerste keer dat de gebruiker zich aanmeldt.

Ja, wanneer u de afbeelding van de sjabloon maakt, kunt u informatie toevoegen aan het standaardprofiel. Deze informatie wordt vervolgens toegevoegd aan de UPD.

## <a name="can-i-change-the-size-of-the-upd-depending-on-how-much-data-i-want-to-store"></a>Kan ik wijzigen met de grootte van de UPD afhankelijk van de hoeveelheid gegevens die ik wil opslaan?

Nee, alle UPDs 50 GB opslagruimte hebben. Als u verschillende hoeveelheden gegevens opslaan wilt, probeert u het volgende:

1. UPDs voor de collectie uitschakelen.
2. Een bestandsshare voor gebruikers toegang hebben tot instellen.
3. Het bestand delen via een opstartscript geladen. Zie hieronder voor meer informatie over opstartscripts in Azure RemoteApp.
4. Directe gebruikers om op te slaan dat alle gegevens naar het bestand delen.


## <a name="how-do-i-run-a-startup-script-in-azure-remoteapp"></a>Hoe kan ik een opstartscript uitvoeren in Azure RemoteApp?

Als u een opstartscript uitvoeren wilt, kunt u beginnen met het maken van een geplande taak in de afbeelding van de sjabloon die u wilt gebruiken voor de collectie. (Doen deze *voordat* u sysprep uitvoert.) 

![Een systeemtaak maken](./media/remoteapp-upd/upd1.png)

![Een systeemtaak maken die wordt uitgevoerd wanneer een gebruiker zich aanmeldt](./media/remoteapp-upd/upd2.png)

Klik op het tabblad **Algemeen** Zorg ervoor dat u het **Gebruikersaccount** onder beveiliging op "BUILTIN\Users." wijzigen

![Het gebruikersaccount aan een groep wijzigen](./media/remoteapp-upd/upd4.png)

De geplande taak wordt uw opstartscript referenties van de gebruiker gestart. De taak te voeren om een wanneer een gebruiker zich aanmeldt.

![De trigger voor de taak 'op aanmelden"instellen](./media/remoteapp-upd/upd3.png)

U kunt ook [op basis van Groepsbeleid opstartscripts](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx)gebruiken. 

## <a name="what-about-placing-a-startup-script-in-the-start-menu-would-that-work"></a>Hoe zit een opstartscript plaatsen in het menu Start? Zou dat werkt?

Met andere woorden, kan ik maakt een type-bestand dat een script config-venster wordt uitgevoerd en sla deze op de s\Opstarten c:\ProgramData\Microsoft\Windows\Start en hebt uitgevoerd wanneer een gebruiker een RemoteApp-sessie start script?

Nee, die wordt niet ondersteund met Azure RemoteApp, waarbij gebruik wordt RDSH, die ook biedt geen ondersteuning voor opstartscripts in het menu Start.

## <a name="can-i-use-mstscexe-the-remote-desktop-program-to-configure-logon-scripts"></a>Kan ik mstsc.exe (het programma Extern bureaublad) gebruiken voor het configureren van gebruikers?

Nee, ondersteund niet door Azure RemoteApp.

## <a name="can-i-store-data-on-the-vm-locally"></a>Kan ik gegevens op de VM lokaal opslaan?

Nee, gegevens die zijn opgeslagen op de VM niet in de UPD niet verloren. Er is een hoge kans dat de gebruiker krijgt geen de dezelfde VM de volgende keer dat ze zich bij Azure RemoteApp aanmelden. We doen gebruiker-VM permanente, niet behouden zodat de gebruiker wordt niet Meld u aan bij de dezelfde VM, en de gegevens verloren. Als we de verzameling bijwerkt, worden de bestaande VMs bovendien vervangen door een nieuwe set VMs - dat betekent dat alle gegevens die zijn opgeslagen op de VM zelf gaat verloren. De aanbeveling is voor de opslag van gegevens in de UPD, gedeelde opslag zoals Azure-bestanden, een bestandsserver binnen een VNET of op de cloud met een systeem voor de opslag van cloud zoals DropBox.

## <a name="how-do-i-mount-an-azure-file-share-on-a-vm-using-powershell"></a>Hoe ik een bestandsshare van Azure koppelen op een VM, via PowerShell?

U kunt de cmdlet Net-PSDrive het station, als volgt koppelen:

    New-PSDrive -Name <drive-name> -PSProvider FileSystem -Root \\<storage-account-name>.file.core.windows.net\<share-name> -Credential :<storage-account-name>


U kunt ook uw referenties opslaan door het volgende uit te voeren:

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>


Waarmee u overslaan de - parameter referentie in de cmdlet New-PSDrive.
