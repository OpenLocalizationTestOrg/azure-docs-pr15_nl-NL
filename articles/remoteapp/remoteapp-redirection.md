<properties
    pageTitle="Omleiding gebruiken in Azure RemoteApp | Microsoft Azure"
    description="Meer informatie over het configureren en gebruiken van omleiding in RemoteApp"
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

# <a name="using-redirection-in-azure-remoteapp"></a>Omleiding in Azure RemoteApp gebruiken

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Apparaatomleiding van kunt uw gebruikers die werken met externe-apps met behulp van de apparaten die zijn bijgevoegd bij hun lokale computer, telefoon of tablet. Als u Skype via Azure RemoteApp hebt opgegeven, moet uw gebruikers-bijvoorbeeld de camera op hun PC om te werken met Skype is geïnstalleerd. Dit geldt ook voor printers, luidsprekers, beeldschermen en een bereik van USB-verbinding randapparaten.

RemoteApp maakt gebruik van de Remote Desktop Protocol (RDP) en RemoteFX voor omleiding.

## <a name="what-redirection-is-enabled-by-default"></a>Welke omleiding is standaard ingeschakeld?
Wanneer u RemoteApp gebruikt, zijn de volgende omleidingen al dan niet standaard ingeschakeld. De gegevens tussen haakjes weergegeven de RDP-instelling.

- Geluiden afspelen op de lokale computer (voor**afspelen op deze computer**). (audiomode:i:0)
- Geluid ontvangen via de lokale computer vastleggen en verzenden naar de externe computer (**Record van deze computer**). (audiocapturemode:i:1)
- Afdrukken met lokale printers (redirectprinters:i:1)
- COM-poorten (redirectcomports:i:1)
- Smartcard-apparaat (redirectsmartcards:i:1)
- Klembord (mogelijkheid om te kopiëren en plakken) (redirectclipboard:i:1)
- Schakel type lettertype Demping (lettertype Effenen toestaan: i:1)
- De oproep doorschakelen alle ondersteunde PP-apparaten. (devicestoredirect:s: *)

## <a name="what-other-redirection-is-available"></a>Welke andere omleiding is beschikbaar?
Twee omleiding opties zijn standaard uitgeschakeld:

- Stationsomleiding (stationstoewijzing): van uw lokale computer stations worden toegewezen stations in de externe sessie. Hiermee kunt u opslaan of bestanden openen vanuit uw lokale vaste schijven terwijl u in de externe sessie werkt.
- USB-omleiding: U kunt de USB-apparaten verbonden met uw lokale computer binnen de externe sessie.

## <a name="change-your-redirection-settings-in-remoteapp"></a>Uw omleidingsinstellingen in RemoteApp wijzigen
U kunt de instellingen voor omleiding voor een siteverzameling wijzigen met behulp van de Microsoft Azure PowerShell met SDK. Nadat u de nieuwe PowerShell en SDK hebt geïnstalleerd, eerst configureert u deze voor het beheren van uw abonnement, zoals wordt beschreven in [het installeren en configureren van Azure PowerShell](../powershell-install-configure.md).

Gebruikt u een opdracht als volgt te werk om in te stellen van de aangepaste RDP-eigenschappen:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

(Houd er rekening mee dat *'n* wordt gebruikt als scheidingsteken tussen afzonderlijke eigenschappen.)

Als u een lijst met welke aangepaste RDP-eigenschappen zijn geconfigureerd, moet u de volgende cmdlet uitvoeren. Houd er rekening mee dat alleen aangepaste eigenschappen worden weergegeven als uitvoer resultaten en niet de standaard-eigenschappen:  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

Wanneer u aangepaste eigenschappen instelt moet u alle aangepaste eigenschappen opgeven telkens wanneer; de instelling hersteld anders uitgeschakeld.   

### <a name="common-examples"></a>Algemene voorbeelden
Gebruik de volgende cmdlet wilt stationsomleiding inschakelen:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*”

Gebruik deze cmdlet om in te schakelen zowel USB-station omleiding:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

Gebruik deze cmdlet Klembord delen uitschakelen:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0”

> [AZURE.IMPORTANT] Zorg ervoor dat u afmelden volledig alle gebruikers in de siteverzameling (en niet alleen verbreekt) voordat u de wijziging testen. Om ervoor te zorgen zijn volledig afgemeld, gaat u naar het tabblad **sessies** in de verzameling in de portal van Azure en alle gebruikers die zijn verbroken of aangemeld afmelden. Soms kan het enkele seconden voor de lokale stations weergeven in Verkenner binnen de sessie duren.

## <a name="change-usb-redirection-settings-on-your-windows-client"></a>Instellingen van de USB-omleiding op uw Windows-client wijzigen

Als u USB-omleiding gebruiken op een computer die is verbonden met RemoteApp wilt, zijn er 2 acties die moeten gebeuren. 1 - moet uw beheerder USB-omleiding op het niveau van de siteverzameling inschakelen via Azure PowerShell. 2 - Klik op elk apparaat waarop u wilt gebruiken van de USB-omleiding, moet u een groepsbeleid waarmee deze inschakelen. Deze stap moet worden uitgevoerd voor elke gebruiker die wil USB-omleiding gebruiken.

> [AZURE.NOTE] USB-omleiding met Azure RemoteApp wordt alleen ondersteund voor Windows-computers.

### <a name="enable-usb-redirection-for-the-remoteapp-collection"></a>USB-omleiding voor de collectie RemoteApp inschakelen
Gebruik de volgende cmdlet wilt inschakelen USB-omleiding op het niveau van de siteverzameling:

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-the-client-computer"></a>USB-omleiding voor de client inschakelen

USB-omleidingsinstellingen configureren op uw computer:

1. Open de Editor voor lokaal groepsbeleid (GPEDIT. MSC). (Gpedit.msc uitgevoerd vanaf een opdrachtprompt).
2. Open **Computer Configuration\Policies\Administrative Templates\Windows onderdelen\Extern Desktop Services\Remote verbinding bureaublad Client\RemoteFX USB-apparaatomleiding**.
3. Dubbelklik op **toestaan RDP-omleiding van andere ondersteunde RemoteFX USB-apparaten van deze computer**.
4. Selecteer **ingeschakeld**en selecteer vervolgens **beheerders en gebruikers in de toegangsrechten RemoteFX USB-omleiding**.
5. Open een opdrachtprompt met beheerdersmachtigingen en voer de volgende opdracht:

        gpupdate /force
6. Start de computer opnieuw op.

U kunt ook het Beheerhulpmiddel Groepsbeleid maken en toepassen van het beleid van de USB-omleiding voor alle computers in uw domein:

1. Meld u aan bij de domeincontroller als de domeinbeheerder van het.
2. Open de beheerconsole voor Groepsbeleid. (Klik op **Start > Systeembeheer > beheer van Groepsbeleid**.)
3. Ga naar het domein of organisatie-eenheid waarvoor u wilt maken van het beleid.
4. Met de rechtermuisknop op het **Standaardbeleid**en klik vervolgens op **bewerken**.
5. Open **Computer Configuration\Policies\Administrative Templates\Windows onderdelen\Extern Desktop Services\Remote verbinding bureaublad Client\RemoteFX USB-apparaatomleiding**.
6. Dubbelklik op **toestaan RDP-omleiding van andere ondersteunde RemoteFX USB-apparaten van deze computer**.
7. Selecteer **ingeschakeld**en selecteer vervolgens **beheerders en gebruikers in de toegangsrechten RemoteFX USB-omleiding**.
8. Klik op **OK**.  
