<properties 
    pageTitle="WordPress converteren naar meerdere locaties in Azure App-Service" 
    description="Meer informatie over het maken van een bestaande web-app voor WordPress is gemaakt via de galerie in Azure en converteren naar WordPress meerdere locaties" 
    services="app-service\web" 
    documentationCenter="php" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>



# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a>WordPress converteren naar meerdere locaties in Azure App-Service

## <a name="overview"></a>Overzicht

*Door [Ben Lobaugh][ben-lobaugh], [Microsoft openen Technologies Inc.][ms-open-tech]*

In deze zelfstudie leert u hoe kunt u een bestaande web-app voor WordPress is gemaakt via de galerie in Azure en omzetten in een installatie WordPress meerdere locaties. Ook leert u hoe u een aangepast domein toewijzen aan elk van de subsites binnen de installatie.

Ervan wordt uitgegaan dat u een bestaande installatie van WordPress hebben. Als u niet het geval is, volgt u de richtlijnen van [een website WordPress vanuit de galerie in Azure maken][website-from-gallery].

Conversie van een bestaande WordPress één site installeren op meerdere locaties is meestal eenvoudig en veel van de eerste stappen die hier afkomstig zijn rechtstreeks van het [Maken van een netwerk] [ wordpress-codex-create-a-network] pagina op de [WordPress Codex](http://codex.wordpress.org).

Laten we aan de slag.

## <a name="allow-multisite"></a>Meerdere locaties toestaan

U moet eerst inschakelen meerdere locaties tot en met de `wp-config.php` bestand met de **WP\_toestaan\_meerdere locaties** constante. Er zijn twee manieren uw web app-bestanden bewerken: de eerste tot en met FTP- en de tweede tot en met cijfer is. Als u niet bekend met een van de volgende manieren instellen bent, raadpleegt u de volgende zelfstudies:

* [PHP websites onderhouden met MySQL- en FTP-][website-w-mysql-and-ftp-ftp-setup]

* [De website PHP met MySQL en cijfer][website-w-mysql-and-git-git-setup]

Open de `wp-config.php` -bestand met de editor van uw keuze en voeg de volgende bovenstaande de `/* That's all, stop editing! Happy blogging. */` lijn.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Zorg ervoor dat u het bestand opslaat en uploadt dit terug naar de server!

## <a name="network-setup"></a>Netwerk instellen

Meld u aan in naar het gebied *wp-beheerder* van uw web-app en u ziet een nieuw item in het menu **Extra** **Netwerkinstellingen**genoemd. Klik op **Netwerk-instelling** en voer de details van uw netwerk.

![Instellingenscherm netwerk][wordpress-network-setup]

Deze zelfstudie gebruikt het schema van de site *submappen* omdat dit altijd werkt moet en we aangepaste domeinen voor elke subsite verderop in deze zelfstudie instellen gaat. Echter, moet het mogelijk zijn te setup een subdomein installeren als u een domein via het jokerteken [Azure-Portal](https://portal.azure.com) en configuratie van DNS-correct worden toegewezen.

Submap instellingen Zie voor meer informatie over het subdomein tegenover de [soorten meerdere locaties netwerk] [ wordpress-codex-types-of-networks] -artikel over de WordPress Codex.

## <a name="enable-the-network"></a>Het netwerk inschakelen

Het netwerk is nu geconfigureerd in de database, maar er is een resterende stap bij het inschakelen van de netwerkfunctionaliteit voor het. Voltooien de `wp-config.php` instellingen en zorg ervoor `web.config` correct stuurt elke site.


Wanneer u op de knop **installeren** op de installatiepagina van *Netwerk* , WordPress bijwerken wordt geprobeerd de `wp-config.php` en `web.config` bestanden. U moet altijd het selectievakje de bestanden om ervoor te zorgen dat de updates zijn aangebracht. Als dit niet het geval is, wordt dit scherm u de vereiste updates worden aangeboden. Bewerken en sla de bestanden.


Na aanbrengen back-deze updates die u moet Meld u af en meld u aan bij het wp-admin-dashboard.

Er moet nu een menu Extra op de balk gelabelde **Mijn Sites**-beheerder. In dit menu kunt u aangeven van uw nieuwe netwerk via het **Netwerk Admin** -dashboard.

## <a name="adding-custom-domains"></a>Aangepaste domeinen toevoegen

De [Toewijzing van WordPress GM domein] [ wordpress-plugin-wordpress-mu-domain-mapping] invoegtoepassing kunt u probleemloos aangepaste domeinen toevoegen aan een site in uw netwerk. In de volgorde voor de invoegtoepassing werkt, moet u enkele aanvullende instellingen op de Portal en ook bij uw domeinregistrar.

## <a name="enable-domain-mapping-to-the-web-app"></a>Domein toewijzen aan de web-app

De **gratis** [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) abonnement modus biedt geen ondersteuning voor aangepaste domeinen toevoegen aan Web Apps. Moet u overschakelen naar ** **gedeeld** standaardmodus** . Dit wilt doen:

* Meld u aan bij de Portal Azure en zoek naar uw web-app. 
* Klik op het tabblad **vergroten** in **Instellingen**.
* Schakel onder **Algemeen** *gedeeld* of *standaard*
* Klik op **Opslaan**

Mogelijk ontvangt u een bericht gevraagd om te controleren of de wijziging en Bevestig dat uw web-app kan nu kosten in rekening gebracht een, afhankelijk van het gebruik en de andere configuratie die u instelt.

Het duurt een paar seconden verwerkingstijd van de nieuwe instellingen, dus is een goed moment om te beginnen bij het instellen van uw domein.

## <a name="verify-your-domain"></a>Uw domein bevestigen

Voordat Azure-WebApps u een domein toewijzen aan de site kunt, moet u eerst controleren of u de machtiging voor het toewijzen van het domein. Hiervoor moet u een nieuwe CNAME-record toevoegen aan uw DNS-vermelding.

* Meld u aan bij van uw domein DNS-beheer
* Een nieuwe CNAME- *awverify* maken
* Wijst *awverify* *awverify. YOUR_DOMAIN.azurewebsites.NET*

Het duurt enige tijd voor de DNS-wijzigingen in de volledige kracht, dus gaat u als de volgende stappen werken niet onmiddellijk koffie, maken en vervolgens keert u terug en probeer het opnieuw.

## <a name="add-the-domain-to-the-web-app"></a>Het domein toevoegen aan de web-app

Ga terug naar uw web-app via de Portal Azure, klikt u op **Instellingen**en klik vervolgens op **aangepaste domeinen en SSL**.

Wanneer de *SSL-instellingen* worden weergegeven, ziet u de velden waar u de domeinen die u wilt toewijzen aan uw web-app ingevoerd. Als u een domein is hier niet wordt vermeld, is het niet mogelijk voor toewijzing binnen WordPress, ongeacht hoe de DNS van het domein ingesteld is.

![Dialoogvenster voor aangepaste domeinen beheren][wordpress-manage-domains]

Nadat uw domein in het tekstvak te typen, wordt Azure Controleer of de CNAME-record die u eerder hebt gemaakt. Als de DNS-records is niet volledig doorgegeven wordt een rode indicator weergegeven. Als deze geslaagd is, ziet u een groen vinkje. 

Let op het IP-adres onderaan in het dialoogvenster. Moet u Hiermee kunt u de A-record voor uw domein instellen.

## <a name="setup-the-domain-a-record"></a>Configuratie van het domein een record

Als de andere stappen aangebracht zijn, kunt u het domein kunt nu naar uw Azure web-app via een DNS-A-record toewijzen. 

Het is belangrijk te weten hier dat Azure-WebApps CNAME- en een records accepteren, maar u *moet* een A-record gebruiken om te schakelen van de toewijzing van het juiste domein. Een CNAME kan niet worden doorgestuurd naar een andere CNAME, namelijk wat Azure met YOUR_DOMAIN.azurewebsites.net voor u gemaakt.

Met het IP-adres van de vorige stap, Ga terug naar uw DNS-beheer en configuratie van de A-record, zodat deze verwijzen naar die IP van.


## <a name="install-and-setup-the-plugin"></a>Installeren en instellen van de invoegtoepassing

Meerdere locaties WordPress nog momenteel geen gebruikmaakt van een ingebouwde methode voor het toewijzen van aangepaste domeinen. Er is echter een invoegtoepassing ' [WordPress GM domein toewijzen] ' genoemd[ wordpress-plugin-wordpress-mu-domain-mapping] die voegt de functionaliteit voor u. Meld u aan bij het gedeelte netwerkbeheerder van uw site en de **Toewijzing van WordPress GM domein** -invoegtoepassing te installeren.

Na het installeren en activeren van de invoegtoepassing, gaat u naar **Instellingen** > **Domein toewijzen** aan het configureren van de invoegtoepassing. Invoer in het eerste tekstvak *Server IP-adres*, het IP-adres dat u gebruikt voor het instellen van de A-record voor het domein. Stel eventuele *Opties voor domein* gewenste (posities zijn vaak fijn) en klik op **Opslaan**.

## <a name="map-the-domain"></a>Het domein toewijzen

Ga naar het **Dashboard** voor de site die u wilt het domein dat u wilt toewijzen. Klik op **Extra** > **Domein toewijzen** en typt u het nieuwe domein in het tekstvak en klik op **toevoegen**.

Standaard wordt het nieuwe domein op het domein van de site automatisch gegenereerde worden herschreven. Als u dat al het verkeer verzonden naar het nieuwe domein wilt, het selectievakje *hoofddomein voor dit blog* voordat u deze opslaat. U kunt een onbeperkt aantal domeinen toevoegen aan een site, maar slechts één primaire kan zijn.

## <a name="do-it-again"></a>Opnieuw doen

Azure-WebApps kunt u een onbeperkt aantal domeinen toevoegen aan een web-app. U moet de secties en **configuratie van het domein een record** van **uw domein verifiëren** voor elk domein uitvoeren als een ander domein wilt toevoegen.  

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

## <a name="whats-changed"></a>Wat er gewijzigd
* Zie voor een handleiding voor het wijzigen van Websites naar App Service: [Azure App-Service en de invloed op bestaande Azure-Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png

 
