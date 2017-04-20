
<properties 
    pageTitle="Hoe u uw Office 365-abonnement met Azure RemoteApp | Microsoft Azure"
    description="Leer hoe u uw Office 365-abonnement in Azure RemoteApp kunt gebruiken om te delen van Office-apps."
    services="remoteapp"
    documentationCenter="" 
    authors="piotrci" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-to-use-your-office-365-subscription-with-azure-remoteapp"></a>Hoe u uw Office 365-abonnement met Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

Wist u dat u uw bestaande Office 365-abonnement in Azure RemoteApp gebruiken kunt om te delen van Office-apps vanuit de cloud? Lees verder voor informatie over het Office 365 + Azure RemoteApp-opties, maken inclusief koppelingen naar artikelen over Office 365 die u helpen optimaal van uw abonnement.

## <a name="how-do-i-use-office-365-accounts-for-azure-remoteapp"></a>Hoe gebruik ik Office 365-accounts voor Azure RemoteApp?
Uitchecken van Peter nieuw artikel voor alle informatie: [het gebruik van Azure RemoteApp met Office 365-gebruikersaccounts](remoteapp-o365user.md)

## <a name="can-i-use-my-office-365-subscription-to-run-office-applications-in-azure-remoteapp"></a>Kan ik mijn Office 365-abonnement Office-toepassingen uitvoeren in Azure RemoteApp gebruiken?

Ja! Met uw Office 365-abonnement is in feite de enige manier om uw Office-toepassingen om Azure RemoteApp weer te geven.

(Notitie: als uw implementatie van Azure RemoteApp wordt geleverd door een hostingprovider partner, ze mogelijk u voorziet van Office-licenties op basis van een [Service Provider Licensing Agreement](http://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx))


Het mooie over uw Office 365-abonnement is dat u de dezelfde gebruikerslicentie op veel verschillende platforms en verschillende omgevingen kunt, met inbegrip van de Azure cloud gebruiken. Wanneer u Office-toepassingen in Azure RemoteApp gebruiken moet u deze niet meer licenties aanschaffen of de bestaande licenties configureren in een speciale manier. Hoeft u Office 365-abonnement met [Office 365 ProPlus](https://technet.microsoft.com/library/Gg702619.aspx)is.

Office 365 ProPlus kunt [gedeelde computeractivering](https://technet.microsoft.com/library/Dn782860.aspx) : dit onderdeel kunt tijdelijke op basis van een gebruiker activering van Office in virtuele en cloud-omgevingen zoals Azure RemoteApp (en Remote Desktop Services).

Welke Office 365-abonnementen krijg ik Office 365 ProPlus? Bekijk de [beschikbare Service binnen elk plan](https://technet.microsoft.com/library/office-365-plan-options.aspx) -tabel. Houd er rekening mee dat niet alle abonnementen krijg ik Office 365 ProPlus (bijvoorbeeld de Office 365 voor bedrijven-abonnement). Als uw abonnement niet ondersteunt, kunt u een upgrade naar een abonnement die (bijvoorbeeld Office 365 Education E3 doet).

## <a name="ok-so-how-are-my-office-365-proplus-licenses-used-with-azure-remoteapp"></a>OK, dus hoe zijn mijn Office 365 ProPlus licenties gebruikt met Azure RemoteApp?

Elke gebruikerslicentie voor Office 365 ProPlus kunt u één gebruiker activeren van Office-toepassingen op maximaal 5 computers- plus tablets en -telefoons. Elke activering is geregistreerd bij de gebruiker totdat ze Office op het apparaat deactiveren. (Gebruikers kunnen hun apparaten in de [Office 365-portal](https://portal.office365.com/)beheren).

Met Azure RemoteApp één gebruiker mogelijk Meld u aan bij verscheidene computers op dezelfde dag zonder dat u het merkt. Dat komt doordat de service die automatisch worden beheerd en schalen van resources in de cloud, terwijl de gebruiker ziet alleen de apps en -programma's die u hebt gedeeld. Voor deze Office 365 ProPlus scenario een gedeelde computer activering modus biedt-betekent dit dat die gebruiker geen hoeft dat te doen een Licentiebeheer voor toegang tot de resources en dat de afzonderlijke computers niet meegeteld de limiet van 5 computer activering.

Zo lang maken als u (de beheerder) Office 365 ProPlus licenties aan uw gebruikers toewijzen, kunnen ze Office gebruiken op hun persoonlijke apparaten en via uw Azure RemoteApp-verzameling.

## <a name="which-office-applications-can-i-use-with-office-365-and-azure-remoteapp"></a>Welke Office-toepassingen kan ik gebruiken met Office 365 en Azure RemoteApp?

U kunt uw Office 365-abonnement kunt gebruiken om te activeren en delen van Office 2013 in Azure RemoteApp-implementaties. We ondersteunen momenteel niet het gebruik van andere versies van Office met Azure RemoteApp. Dit geldt ook voor Office 2003, Office 2007, Office 2010 en Office 2016.

## <a name="what-about-visio-pro-or-project-pro"></a>Hoe zit het met Visio Pro of een nieuw Project Pro?

De Office 365 ProPlus afbeelding deel uitmaakt van uw abonnement RemoteApp bevat functies voor Visio Pro en Project Pro. Maar u kunt uw abonnement op Office 365 ProPlus niet gebruiken voor deze programma's activeren: elke ze hun eigen licentie hebben. Klik in de [Office 365-portal](https://portal.office365.com/)kunt u deze activeren. 

Er geen licentie voor deze programma's als u niet wilt gebruiken. Alleen de programma's die u wilt gebruiken - en slaat u de andere activeren. U kunt deze nog steeds zien in de afbeelding, maar u kunt ze niet gebruiken. 

## <a name="how-do-i-get-started-with-office-365-and-azure-remoteapp"></a>Hoe ga ik aan de slag met Office 365 en Azure RemoteApp?

Als u weet dat de details van licenties voor Office 365, laten we u aan de slag met Azure RemoteApp - het is heel eenvoudig:

Wanneer u uw Azure RemoteApp-siteverzameling maakt, gebruikt u de afbeelding van de **Office 365 ProPlus (abonnement vereist)** .

![Azure RemoteApp-afbeelding met Office 365 Pro Plus](./media/remoteapp-officesubscription/remoteapp-officeimage.png)


Deze afbeelding bevat de meest recente versie van Windows Server en Office 365 ProPlus. Nadat u de verzameling (inclusief publicerende apps), uw gebruikers gewoon configureren Meld u aan bij Azure RemoteApp (via hun RemoteApp-client) en hun Office 365-referenties opgeven voor een Office-apps. Licenties worden automatisch geleverd zonder een set up of management vereist.

## <a name="can-i-create-a-custom-image-with-office-365-proplus"></a>Kan ik een aangepaste afbeelding maken met Office 365 ProPlus?

U kunt een aangepaste afbeelding maken voor uw siteverzameling met Office 365 ProPlus. Er zijn 2 keuzen - gebruik van de Azure-galerij die wij bieden of u kunt uw eigen aangepaste afbeelding maken en er Office 365 ProPlus installeren.

### <a name="use-the-azure-gallery-image"></a>Gebruik de met Azure-galerijafbeelding

De eenvoudigste manier om Office 365 ProPlus implementeren naar een siteverzameling is om te [beginnen met een van de galerijafbeeldingen Azure](remoteapp-image-on-azurevm.md) deel uitmaakt van uw abonnement Azure RemoteApp. Zorg ervoor dat u de afbeelding **Windows Server extern bureaublad met Office 365 ProPlus vooraf geïnstalleerd** kiezen. Vervolgens installeert u eventuele andere gewenste apps op die afbeelding en u bent klaar.

### <a name="use-a-custom-image"></a>Een aangepaste afbeelding gebruiken

U kunt altijd een aangepaste afbeelding maken: u kunt een [Azure VM](remoteapp-image-on-azurevm.md) of het [maken van de afbeelding lokaal](remoteapp-create-custom-image.md) maken en uploaden naar Azure. In beide gevallen moet dat u Office 365 ProPlus met het knooppunt voor de activering gedeelde computer installeren. Het [Office-implementatieprogramma](http://blogs.technet.com/b/odsupport/archive/2014/07/11/using-the-office-deployment-tool.aspx) gebruiken en volg de [instructies](https://technet.microsoft.com/library/Dn782858.aspx) voor de installatie.  

### <a name="disable-automatic-updates-for-office-365-proplus-in-your-custom-image---important"></a>Automatische updates voor Office 365 ProPlus uitschakelen in uw aangepaste afbeelding: belangrijke

Uw aangepaste afbeelding wordt gebruikt door Azure RemoteApp als een sjabloon voor het toevoegen van extra bronnen als de vraag uit uw gebruikers toeneemt. Als u wilt voorkomen dat vertragingen en verbindingsproblemen, moet u automatische updates uitschakelen voor Office in de afbeelding. Als u niet het geval is, wordt klikt u vervolgens elke resource die zijn gemaakt met deze sjabloon automatisch bijgewerkt wanneer deze wordt gestart. Gebruik in plaats daarvan het standaard Azure RemoteApp-proces voor het bijwerken van uw aangepaste afbeelding. Zodat u de Office-toepassingen eenmaal op de afbeelding van de sjabloon bijwerkt en laat Azure RemoteApp kunt verrichten de updates voor toegang tot uw gebruikers.

Voeg het volgende in het Office-implementatieprogramma configuratiebestand om uit te schakelen automatische updates:

        <Updates Enabled="FALSE" />

Het configuratiebestand moet nu deze regels bevatten:
    
        <Display Level="NONE" AcceptEULA="TRUE" />
        <Property Name="SharedComputerLicensing" Value="1" />
        <Updates Enabled="FALSE" />

## <a name="so-how-can-i-update-an-image-with-office-365-proplus"></a>Hoe kan ik een afbeelding met Office 365 ProPlus bijwerken?

Zijn er veel redenen voor het bijwerken van de afbeelding in uw siteverzameling. Hier volgen een paar:

- Download de nieuwste updates voor Windows 
- Download de nieuwste updates voor Office 365 ProPlus-toepassing
- Uw aangepaste app bijwerken
- Andere instellingen voor de afbeelding zelf wijzigen

Voor de end-to-end stappen voor het bijwerken van uw siteverzameling als de afbeelding die u bijgewerkt wilt gebruiken, gaat u [hier](remoteapp-update.md). Maar voor informatie over het bijwerken van de afbeelding en de Office 365 ProPlus, raadpleegt u de volgende informatie.

U hebt twee opties voor het bijwerken van uw afbeelding - vervangen van uw afbeelding met een volledig nieuw of handmatig bijwerken van uw bestaande afbeelding.

### <a name="replace-your-image-with-the-latest-azure-gallery-image--add-customizations"></a>Uw afbeelding vervangen door de meest recente Azure-galerij + aanpassingen toevoegen
Met deze optie wordt u de werkdruk in Microsoft Windows Server en Office 365 ProPlus updates kunt verrichten. In plaats van uw bestaande afbeelding wordt bijgewerkt, maakt u een compleet nieuwe afbeelding op basis van de meest recente galerij. Herhaal alle stappen die u hebt voordat u de afbeelding aanpassen - aangepaste apps installeren wijzigen van de afbeelding van de configuratie, enzovoort.

De met galerijafbeeldingen worden regelmatig bijgewerkt, zodat u eenvoudig, kan er als u weet dat uw apps voor Windows Server en Office 365 ProPlus up-to-date zijn. De verhouding is natuurlijk dat u moet uw aanpassingen toepassen telkens wanneer u een nieuwe afbeelding. U kunt scripts wilt automatiseren met het instellen van uw aanpassingen maken.

### <a name="manually-update-your-existing-image"></a>Uw bestaande afbeelding handmatig bijwerken

Met deze optie wordt u standaard Windows's updates toepassen op de afbeelding. Voor Office 365 ProPlus, gebruikt u het Office-implementatieprogramma downloaden en installeren van de meest recente updates of versies van Office 365 ProPlus.

> [AZURE.IMPORTANT] Vergeet niet: de Office 365 ProPlus automatische updates uitschakelen.

Meer informatie over het gebruik van de Office-implementatieprogramma voor updates nodig?

- [Klik-en-klaar implementeren voor Office 365-producten met behulp van de Office-implementatieprogramma](https://technet.microsoft.com/library/JJ219423.aspx)
- [Distribueren en bijwerken van Office 365 ProPlus met het Office-implementatieprogramma](https://channel9.msdn.com/Events/Ignite/2015/BRK3168) (video)
- [Instellingen van de update voor Office 365 ProPlus configureren](https://technet.microsoft.com/library/dn761708.aspx)
