<properties
   pageTitle="Het uitvoeren van een Windows-app op elk apparaat met Azure RemoteApp | Microsoft Azure"
   description="Informatie over het delen van een Windows-app met uw gebruikers met behulp van Azure RemoteApp."
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="run-any-windows-app-on-any-device-with-azure-remoteapp"></a>Een Windows-app op elk apparaat met Azure RemoteApp uitvoeren

> [AZURE.IMPORTANT]
> Azure RemoteApp is wordt gestopt. Lees de [aankondiging](https://go.microsoft.com/fwlink/?linkid=821148) voor meer informatie.

U kunt uitvoeren een Windows-toepassing overal op elk gewenst apparaat, nu, serieus - alleen met behulp van Azure RemoteApp. Of u nu een maatwerktoepassing op 10 jaar geleden geschreven of een Office-app, wordt uw gebruikers niet meer hoeven te worden gekoppeld aan een specifiek besturingssysteem (zoals Windows XP) voor deze enkele toepassingen.

Uw gebruikers kunnen ook met Azure RemoteApp, hun eigen Android- of Apple-apparaten gebruiken en krijgen de dezelfde stijl die ze hebben gekregen op Windows (of op een Windows-telefoon). Hiervoor hostingprovider van uw Windows-toepassing in een verzameling Windows virtuele machines op Azure - uw gebruikers hebben toegang tot deze vanaf elke locatie ze een internetverbinding hebt. 

Lees verder voor een voorbeeld van hoe u dit wilt doen.

In dit artikel gaan we Access delen met alle gebruikers. U kunt echter een app. Zo lang maken als u uw app op een computer met Windows Server 2012 R2 installeren kunt, kunt u het delen van de volgende stappen uit. De [vereisten voor de app](remoteapp-appreqs.md) om ervoor te zorgen dat uw app werkt, kunt u bekijken.

Houd er rekening mee dat omdat toegang een database is en we die database nuttig zijn, ons bezoek een paar extra stappen om te laten toegang van gebruikers de Access-gegevens delen. Als uw app niet een database, of u niet nodig voor uw gebruikers hebt moeten kunnen voor toegang tot een bestandsshare, kunt u deze stappen in deze zelfstudie overslaan

> [AZURE.NOTE] <a name="note"></a>U hebt een Azure-account om te voltooien van deze zelfstudie nodig:
> - U kunt [een Azure-account gratis openen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F): U tegoeden krijgt u kunt uitproberen betaalde Azure services en zelfs nadat ze gebruikt afgerond kunt u het account en gebruik vrij te geven Azure services, zoals Websites. Uw creditcard nooit brengt, tenzij u expliciet uw instellingen wijzigen en vragen om te betalen.
> - U kunt [MSDN abonnee voordelen activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): uw MSDN-abonnement kunt u tegoeden elke maand die u voor betaalde Azure-services gebruiken kunt.


## <a name="create-a-collection-in-remoteapp"></a>Maken van een siteverzameling in RemoteApp

Beginnen met het maken van een siteverzameling. De verzameling fungeert als een container voor uw apps en gebruikers. Elke siteverzameling is gebaseerd op een afbeelding: u kunt uw eigen kleurenschema samenstellen of gebruik een voorzien van uw abonnement. Voor deze zelfstudie gebruiken we de afbeelding van Office 2013-proefabonnement - bevat de app die we wilt delen.

1. Klik in de portal Azure Schuif omlaag in de structuur van de navigatiebalk linkerpagina totdat er RemoteApp. Hiermee opent u die pagina.
2. Klik op **maken een RemoteApp-siteverzameling**.
3. Klik op **snelle maken** en voer een naam voor uw siteverzameling.
4. Selecteer het gebied dat u wilt gebruiken om te maken van uw siteverzameling. Selecteer het gebied dat zich het dichtst bij de locatie waar uw gebruikers toegang de app tot geografisch voor een optimale ervaring. In deze zelfstudie wordt bijvoorbeeld de gebruikers die zich bevinden in Redmond, Washington. Het eerstvolgende Azure gebied is **West VS**.
5. Selecteer het Facturering abonnement waarop dat u wilt gebruiken. Het eenvoudige Facturering abonnement past 16 gebruikers toe op een grote Azure VM, terwijl het standaard Facturering abonnement 10 gebruikers op een grote Azure VM heeft. Als voorbeeld van een algemene werkt het eenvoudige plan goed voor de werkstroom voor het type invoer van gegevens. Voor een app productiviteit, zoals Office, kunt u de standaard-abonnement.
6. Ten slotte selecteert u de afbeelding Office 2013 Professional. Deze afbeelding bevat Office 2013-apps. Herinnering - in deze afbeelding is maar goed voor proefabonnement verzamelingen en POCs. U ' in deze afbeelding in een verzameling productie niet gebruiken.
7. Klik nu op **maken RemoteApp-siteverzameling**.

![Maken van een siteverzameling cloud in RemoteApp](./media/remoteapp-anyapp/ra-anyappcreatecollection.png)

Hiermee wordt gestart met het maken van uw siteverzameling, maar het duurt maximaal een uur.

U bent nu klaar om uw gebruikers.

## <a name="share-the-app-with-users"></a>De app delen met gebruikers

Wanneer uw siteverzameling is gemaakt, is het tijd publiceren van toegang aan gebruikers en toevoegen van de gebruikers die toegang tot het moeten hebben.

Als u bent gegaan weg van het knooppunt Azure RemoteApp terwijl de verzameling is wordt gemaakt, kijkt te leren terug naar de startpagina van Azure.

2. Klik op de verzameling die u eerder in voor toegang tot extra opties en configureren van de siteverzameling hebt gemaakt.
![Een nieuwe RemoteApp cloud-verzameling](./media/remoteapp-anyapp/ra-anyappcollection.png)
3. Klik op het tabblad **publiceren** , klik op **publiceren** onderaan in het scherm en klik vervolgens op **programma's van het menu Start publiceren**.
![Publiceren van een RemoteApp-programma](./media/remoteapp-anyapp/ra-anyapppublish.png)
4. Selecteer de apps die u wilt publiceren in de lijst. We hebben voor ons doel gekozen Access. Klik op **Voltooien**. Wacht totdat de apps om te publiceren op voltooien.
![Publicatie van Access in RemoteApp](./media/remoteapp-anyapp/ra-anyapppublishaccess.png)


1. Zodra de app is publiceren, hoofd voltooid via naar het tabblad **Gebruikerstoegang** om toe te voegen van alle gebruikers die toegang tot uw apps hebben. Voer gebruikersnamen (e-mailadres) voor uw gebruikers en klik vervolgens op **Opslaan**.

![Gebruikers toevoegen aan RemoteApp](./media/remoteapp-anyapp/ra-anyappaddusers.png)


1. Nu is het tijd aan uw gebruikers informeren over deze nieuwe apps en hoe u deze activeert. Klik hiertoe uw gebruikers een e-mail verzenden die deze wijst naar de URL van extern bureaublad-client downloaden.
![De client URL voor RemoteApp downloaden](./media/remoteapp-anyapp/ra-anyappurl.png)

## <a name="configure-access-to-access"></a>Toegang tot Access configureren

Sommige apps nodig extra configuratie nadat u ze via RemoteApp geïmplementeerd. Met name voor toegang moet gaan we een bestandsshare op Azure die voor elke gebruiker toegankelijk te maken. (Als u niet wilt dat doen, kunt u een [hybride siteverzameling](remoteapp-create-hybrid-deployment.md) [in plaats van de siteverzameling van onze cloud] waarmee uw gebruikers toegang tot bestanden en informatie over het lokale netwerk.) Vervolgens moet we onze gebruikers een lokaal station op hun computer toewijzen aan het Azure bestandssysteem informeren.

Het eerste gedeelte dat u als beheerder doen. Vervolgens hebben we enkele stappen uitvoeren voor uw gebruikers.

1. Begin met het publiceren van de opdracht lijn interface (cmd.exe). Selecteer **cmd**in het tabblad **publiceren** , en klik vervolgens op **publiceren > publiceren programma pad met**.
2. Voer de naam van de app en het pad. Voor ons doel, "File Explorer" Als de naam en "% SYSTEMDRIVE%\windows\explorer.exe" Als het pad te gebruiken.
![Het bestand cmd.exe publiceren.](./media/remoteapp-anyapp/ra-publishcmd.png)
3. U moet nu een Azure [opslag-account](../storage/storage-create-storage-account.md)maken. We onze 'accessstorage,' met de naam een naam die duidelijk herkenbare dus kiezen. (Als u wilt misquote Highlander, kunnen er slechts één 'accessstorage.") ![Our Azure opslag-account](./media/remoteapp-anyapp/ra-anyappazurestorage.png)
4. Ga nu terug naar uw dashboard zodat u kunt het pad toegang hebt tot uw opslag (eindpunt locatie). U kunt hiermee in iets, dus controleer of dat u deze ergens kopiëren.
![Het pad van de account opslag](./media/remoteapp-anyapp/ra-anyappstoragelocation.png)
5. Nadat het account opslag is gemaakt, moet u vervolgens de primaire toegangstoets. Klik op **beheren toegangstoetsen**en kopieer de primaire toegangstoets.
6. Nu de context van de opslag-account instellen en een nieuwe bestandsshare maken om toegang te krijgen. Voer de volgende cmdlets in een verhoogde Windows PowerShell-venster:

        $ctx=New-AzureStorageContext <account name> <account key>
        $s = New-AzureStorageShare <share name> -Context $ctx

    Zodat voor onze delen, de cmdlets die we uitvoeren zijn:

        $ctx=New-AzureStorageContext accessstorage <key>
        $s = New-AzureStorageShare <share name> -Context $ctx


Dit is nu van de gebruiker inschakelen. Laat eerst uw gebruikers een [RemoteApp-client](remoteapp-clients.md)installeren. Vervolgens moeten de gebruikers toewijzen van een station van de account die Azure bestandsshare die u hebt gemaakt en de toegang tot bestanden toevoegen. Dit is hoe ze doen:

1. Toegang tot de gepubliceerde apps in de RemoteApp-client. Start het programma cmd.exe.
2. Voer de volgende opdracht station van uw computer naar de share kunt toewijzen:

        net use z: \\<accountname>.file.core.windows.net\<share name> /u:<user name> <account key>

    Als u de parameter **/ permanente** op Ja instelt, persistent het toegewezen station is voor sessies.
1. Nu de Verkenner app starten vanuit RemoteApp. Kopieer alle Access-bestanden die u wilt gebruiken in de gedeelde app naar de bestandsshare.
![Access-bestanden opslaan in een Azure delen](./media/remoteapp-anyapp/ra-anyappuseraccess.png)
1. Tot slot open Access en open de database die u zojuist hebt gedeeld. Hier ziet u uw gegevens in Access uitgevoerd vanuit de cloud.
![Een reële Access-database uitgevoerd vanuit de cloud](./media/remoteapp-anyapp/ra-anyapprunningaccess.png)

Zorg ervoor dat u een RemoteApp-client installeren nu met Access kunt u op elk apparaat.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe maken van een siteverzameling, probeert u het maken van een [siteverzameling die gebruikmaakt van Office 365](remoteapp-tutorial-o365anywhere.md). Of u een [hybride siteverzameling ](remoteapp-create-hybrid-deployment.md)die toegang uw lokale netwerk tot kunt maken.

<!--Image references-->
 
