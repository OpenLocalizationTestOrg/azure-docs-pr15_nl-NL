<properties
    pageTitle="Enterprise-klasse WordPress op Azure App Service | Microsoft Azure"
    description="Meer informatie over het hosten van een site van de enterprise-klasse WordPress op Azure App-Service"
    services="app-service\web"
    documentationCenter=""
    authors="sunbuild"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.devlang="php"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="web"
    ms.date="10/24/2016"
    ms.author="sumuth"/>

# <a name="enterprise-class-wordpress-on-azure-app-service"></a>Enterprise-klasse WordPress op Azure App-Service

Azure App-Service biedt een omgeving scalable, veilig en eenvoudig te gebruiken voor opdracht kritieke, grote schaal [WordPress] [ wordpress] sites. Microsoft zelf wordt uitgevoerd voor enterprise-klasse sites zoals [Office] [ officeblog] en [Bing] [ bingblog] blogs. In dit document ziet u hoe u kunt Azure App Service-WebApps kunt instellen en onderhouden van een enterprise-klasse, cloudgebaseerde WordPress site dat een grote hoeveelheid bezoekers kan worden verwerkt.

## <a name="architecture-and-planning"></a>Architectuur en plannen

Een eenvoudige WordPress-installatie heeft slechts twee vereisten.

* **MySQL-Database** - beschikbaar via de [ClearDB in de Azure Marketplace][cdbnstore], of kunt u uw eigen MySQL-installatie op Azure virtuele Machines met [Windows] beheren[ mysqlwindows] of [Linux][mysqllinux].

  > [AZURE.NOTE] ClearDB bevat verschillende MySQL-configuraties, met verschillende prestatie-eigenschappen voor elke configuratie. Zie de [Azure Store] [ cdbnstore] voor meer informatie over aanbiedingen beschikbaar via de Azure store of rechtstreeks zoals gezien op [ClearDB-website](http://www.cleardb.com/pricing.view).

* **PHP 5.2.4 of groter** -Azure App Service bevatten momenteel [PHP versies 5.4, 5.5, en 5.6][phpwebsite].

    > [AZURE.NOTE] Het is raadzaam om altijd worden uitgevoerd op de nieuwste versie van PHP om ervoor te zorgen er het meest recente beveiligingscorrecties.

### <a name="basic-deployment"></a>Eenvoudige implementatie

Alleen de basisvereisten gebruikt, kunt u een eenvoudige oplossing binnen een Azure gebied maken.

![een Azure WebApp en MySQL-Database die worden gehost in één Azure regio][basic-diagram]

Terwijl deze u kunt manier naar schalen uw toepassing door meerdere Web Apps-exemplaren van de site te maken, wordt alles gehost binnen de datacenters in een bepaalde geografische regio. Bezoekers van buiten deze regio ziet mogelijk traag antwoord tijden bij gebruik van de site, als de datacenters in dit gebied gaat omlaag, dus doet en uw toepassing.


### <a name="multi-region-deployment"></a>Meerdere landen/regio-implementatie

Met Azure [Verkeer Manager][trafficmanager], is het mogelijk te schalen dat uw site WordPress tussen meerdere geografische regio's terwijl slechts één URL bezoekers. Alle bezoekers worden via verkeer Manager en vervolgens worden gerouteerd naar een gebied op basis van taakverdeling configuratie van het laden.

![een Azure web-app, die worden gehost in meerdere gebieden CDBR beschikbaarheid router gebruiken om te leiden naar MySQL tussen regio 's][multi-region-diagram]

Binnen elke regio, de site WordPress zou nog steeds over meerdere exemplaren van de Web Apps worden aangepast, maar u deze schaalbaarheid regio specifieke; hoge verkeer regio's kunnen anders dan weinig verkeer bestanden worden vergroot.

Replicatie en zoekresultaten omleiden naar meerdere MySQL-Databases kunnen worden uitgevoerd met de ClearDB [CDBR hoge beschikbaarheid van de Router] [ cleardbscale] (Zie links) of [MySQL Cluster CGE][cge].

### <a name="multi-region-deployment-with-media-storage-and-caching"></a>Meerdere landen/regio-implementatie waarbij Mediaopslag en caching van
Als de site uploads of host media-bestanden accepteert, gebruikt u Azure-blobopslag. Als u nodig in cache opslaan hebt, kunt u het [bestand Vgx. cache][rediscache], [Memcache Cloud](http://azure.microsoft.com/gallery/store/garantiadata/memcached/), [MemCachier](http://azure.microsoft.com/gallery/store/memcachier/memcachier/)of een van de andere in de cache aanbiedingen in de [Azure Store](http://azure.microsoft.com/gallery/store/).

![een Azure web-app, die worden gehost in meerdere gebieden met behulp van de beschikbaarheid CDBR router voor MySQL, met beheerde Cache, blobopslag en CDN][performance-diagram]

Blobopslag is geografische zijn verdeeld over regio's al dan niet standaard, zodat u hoeft niet bang te repliceren van bestanden voor alle locaties. U kunt ook de Azure [Inhoud verdeling netwerk (CDN)] inschakelen[ cdn] voor blobopslag, welke bestanden tot het einde van knooppunten dichter naar de bezoekers van uw distribueert.

### <a name="planning"></a>Planning

#### <a name="additional-requirements"></a>Aanvullende vereisten

Dit wilt doen... | Gebruik deze opdracht...
------------------------|-----------
**Uploaden of grote bestanden opslaan** | [WordPress-invoegtoepassing voor het gebruik van Blob storage][storageplugin]
**E-mailbericht verzenden** | [SendGrid] [ storesendgrid] en de [WordPress-invoegtoepassing voor het gebruik van SendGrid][sendgridplugin]
**Aangepaste domeinnamen** | [Een aangepaste domeinnaam in Azure App-Service configureren][customdomain]
**HTTPS** | [HTTPS inschakelen voor een web-app in Azure App-Service][httpscustomdomain]
**Preproductie gegevensvalidatie** | [Tijdelijke omgevingen voor web-apps in Azure-Service voor App instellen][staging] <p>Overstappen van een web-app in de ontwikkeling naar productie ook Hiermee verplaatst u de configuratie WordPress. U moet ervoor zorgen dat alle instellingen zijn bijgewerkt naar de vereisten voor de app productie voordat van abonnement de gefaseerde-app in gebruik neemt.</p>
**Cmdlets voor controle en probleemoplossing** | [Diagnostische gegevens vastleggen voor web-apps in Azure App Service] [ log] en [Web-Apps in Azure App-Service controleren][monitor]
**Uw site implementeren** | [Een WebApp in Azure-Service voor App implementeren][deploy]

#### <a name="availability-and-disaster-recovery"></a>Herstel van beschikbaarheid

Dit wilt doen... | Gebruik deze opdracht...
------------------------|-----------
**Laden balance '-sites** of **geografische-sites distribueren** | [Route-verkeer met Azure verkeer Manager][trafficmanager]
**Een back-up en herstellen** | [Maak een back-up van een WebApp in Azure App Service] [ backup] en [herstellen van een WebApp in Azure App-Service][restore]

#### <a name="performance"></a>Prestaties

Prestaties in de cloud wordt bereikt hoofdzakelijk via caching en schalen; echter moeten het geheugen, bandbreedte en andere kenmerken van de Web-Apps hostingprovider worden beschouwd.

Dit wilt doen... | Gebruik deze opdracht...
------------------------|-----------
**Meer informatie over mogelijkheden voor het exemplaar van App-Service** | [Informatie, zoals de mogelijkheden van App Service niveaus prijzen][websitepricing]
**Cache-resources** | [Bestand Vgx. cache][rediscache], [Memcache Cloud](/gallery/store/garantiadata/memcached/), [MemCachier](/gallery/store/memcachier/memcachier/)of een van de andere in de cache aanbiedingen in de [Winkel van Azure](/gallery/store/)
**De schaal van uw toepassing aanpassen** | [De schaal van een web-app in Azure App Service] [ websitescale] en [ClearDB hoge beschikbaarheid routering][cleardbscale]. Als u kiest voor het hosten en beheren van uw eigen MySQL-installatie, moet u rekening houden [MySQL Cluster CGE] [ cge] voor schalen

#### <a name="migration"></a>Migratie

Er zijn twee methoden voor het migreren van een bestaande WordPress-site naar Azure App-Service.

* ** [WordPress exporteren] [ export] ** -dit Hiermee exporteert u de inhoud van uw blog, dat kan worden geïmporteerd naar een nieuwe WordPress-site op Azure App-Service voor het gebruik van de [WordPress importeur Plug][import].

    > [AZURE.NOTE] Hoewel deze procedure u kunt wilt migreren van uw inhoud, deze niet gemigreerd eventuele Plug-ins, thema's of andere aanpassingen. Deze moeten opnieuw handmatig worden geïnstalleerd.

* **Handmatige migratie** - [Back-up van uw site] [ wordpressbackup] en [database][wordpressdbbackup], klikt u vervolgens handmatig een WebApp in Azure App-Service te herstellen en bijbehorende MySQL-database als u wilt migreren ten zeerste aangepaste sites en geen verspillen Plug-ins, thema's en andere aanpassingen handmatig te installeren.

## <a name="step-by-step-instructions"></a>Stapsgewijze instructies

### <a name="create-a-wordpress-site"></a>Een site WordPress maken

1. Gebruik van de [Azure Marketplace] [ cdbnstore] om een MySQL-database van de grootte te maken die u in de sectie [architectuur en planning](#planning) in het regio('s) dat u uw site wordt gehost geïdentificeerd.

2. Volg de stappen in [een WordPress web-app in Azure App Service maken] [ createwordpress] om een WebApp WordPress te maken. Wanneer u de web-app maakt, selecteer **een bestaande MySQL-Database gebruiken** en selecteer de database in stap 1 hebt gemaakt.

Als u een bestaande WordPress site migreert, raadpleegt u [een bestaande site WordPress naar Azure migreren](#Migrate-an-existing-WordPress-site-to-Azure) na het maken van een nieuwe WebApp.

### <a name="migrate-an-existing-wordpress-site-to-azure"></a>Een bestaande WordPress site migreren naar Azure

Zoals vermeld in de sectie [architectuur en planning](#planning) , zijn er twee manieren om te migreren van een site WordPress.

* **exporteren en importeren** - voor-sites zonder veel aanpassen, of waarnaar u wilt alleen de inhoud verplaatsen.

* **back-up en herstellen** - voor-sites met een groot aantal aanpassing waarnaar u wilt verplaatsen alles.

Gebruik een van de volgende secties om te migreren van uw site.

#### <a name="the-export-and-import-method"></a>De methode exporteren en importeren

1. Gebruik [WordPress exporteren] [ export] exporteren van uw bestaande site.

2. Maak een WebApp met de stappen in de sectie [een WordPress-site maken](#Create-a-new-WordPress-site) .

3. Meld u aan bij uw site WordPress op Web Apps en klik op **Plug-ins** -> **Add New**. Zoeken naar en installeren, de **WordPress importeur** -invoegtoepassing.

4. Nadat de importeur-invoegtoepassing is geïnstalleerd, klikt u op **Hulpmiddelen voor** -> **importeren**en selecteer vervolgens **WordPress** gebruik van de invoegtoepassing voor WordPress importeur.

5. Klik op **Bestand kiezen**op de pagina **Importeren WordPress** . Blader naar het WXR-bestand dat is geëxporteerd vanuit uw bestaande WordPress-site en kies vervolgens **bestand uploaden en importeren**.

6. Klik op **verzenden**. Wordt u gevraagd dat het importeren geslaagd is.

8. Als u al deze stappen hebt voltooid, start u uw site uit de web-app blade opnieuw in de [portal van Azure][mgmtportal].

Na het importeren van de site, moet u mogelijk de volgende stappen om in te schakelen van de instellingen niet in het importbestand uitvoeren.

Als u dit hebt gebruikt... | Werkwijze
------------------ | ----------
**Permalinks** | Vanuit het dashboard WordPress van de nieuwe site op **Instellingen** -> **Permalinks** en vervolgens update de Permalinks-structuur
**afbeelding van de/media koppelingen** | Als koppelingen wilt bijwerken naar de nieuwe locatie, gebruikt u de [invoegtoepassing Velvet blauwe bijwerken URL's][velvet], een hulpmiddel zoeken en vervangen, of handmatig in uw database
**Thema 's** | Ga naar **uiterlijk** -> **thema** en het thema van de site naar wens bijwerken
**Menu 's** | Als uw thema menu's ondersteunt, kan het oude submap ingesloten in deze nog steeds door koppelingen naar de startpagina. Ga naar **uiterlijk** -> **menu's** en bijgewerkt om ze

#### <a name="the-backup-and-restore-method"></a>De methode back-up en herstellen

1. Maak een back-up van uw bestaande WordPress site met behulp van de informatie bij [WordPress back-ups][wordpressbackup].

2. Maak een back-up van uw bestaande database met behulp van de informatie bij het [back-ups van uw database][wordpressdbbackup].

3. Een database maken en terugzetten van de back-up.

    1. Een nieuwe database aanschaffen van [Azure Marketplace][cdbnstore], of een MySQL-database op een [Windows] setup[ mysqlwindows] of [Linux] [ mysqllinux] VM.

    2. Met een MySQL-client zoals [MySQL Workbench][workbench], verbinding maken met de nieuwe database en uw WordPress-database importeren.

    3. De database als u wilt wijzigen van de domein-items naar uw nieuwe domein met Azure App Service bijwerken. Bijvoorbeeld mywordpress.azurewebsites.net. Gebruik de [Zoeken en vervangen voor WordPress Databases Script] [ searchandreplace] overal veilig te wijzigen.

4. Een WebApp in de portal van Azure maken en publiceren van de WordPress back-up.

    1. Een WebApp maken in de [portal van Azure] [ mgmtportal] met een database met behulp van **Nieuw** -> **Web + Mobile** -> **Azure Marketplace** -> **Web Apps** -> **WebApp + SQL** (of **WebApp + MySQL**) -> **maken**. Configureer de vereiste instellingen als u wilt maken van een lege web-app.

    2. In de WordPress back-up, zoek het bestand **wp-config.php** en open dit in een editor. De volgende gegevens vervangen door de gegevens voor uw nieuwe MySQL-database.

        * **DB_NAME** - de gebruikersnaam van de database

        * **DB_USER** - de gebruikersnaam die is gebruikt voor toegang tot de database

        * **DB_PASSWORD** - wachtwoord van de gebruiker

        Na het aanpassen van deze vermeldingen, opslaan en sluit het bestand **wp-config.php** .

    3. Gebruik de [Deploy een WebApp in Azure App-Service] [ deploy] informatie om de implementatiemethode die u wilt gebruiken en klikt u vervolgens de WordPress back-up implementeren naar uw web-app in Azure App-Service.

5. Nadat de site WordPress is geïmplementeerd, moet u een gebruiken in de nieuwe site (als een App Service-web-app) toegang tot de *. azurewebsite.net-URL voor de site.

### <a name="configure-your-site"></a>Uw site configureren

Nadat de site WordPress is gemaakt of gemigreerd, gebruikt u de volgende informatie aan de prestaties verbeteren of extra functionaliteit inschakelen.

Dit wilt doen... | Gebruik deze opdracht...
------------- | -----------
**App-Service plannen modus, grootte en schaal inschakelen instellen** | [De schaal van een web-app in Azure App-Service][websitescale]
**Permanente databaseverbindingen hebt ingeschakeld** | Standaard WordPress gebruikt geen permanente databaseverbindingen, waardoor de verbinding met de database moet worden vertraagd na meerdere verbindingen. Als u permanente verbindingen, installeert u de (permanente verbindingen adapter invoegtoepassing) [https://wordpress.org/plugins/persistent-database-connection-updater/installation/]. 
**De prestaties verbeteren** | <ul><li><p><a href="http://ppe.blogs.msdn.com/b/windowsazure/archive/2013/11/18/disabling-arr-s-instance-affinity-in-windows-azure-web-sites.aspx">De cookie ARR uitschakelen</a> - kan de prestaties verbeteren als WordPress uitvoeren op meerdere exemplaren van de Web Apps</p></li><li><p>Cache inschakelen. <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Bestand Vgx cache.</a> (preview) kan worden gebruikt met de <a href="https://wordpress.org/plugins/redis-object-cache/">bestand Vgx. object cache WordPress Plug</a>, of gebruik een van de andere in de cache aanbiedingen van de <a href="/gallery/store/">Azure Store</a></p></li><li><p><a href="http://ruslany.net/2010/03/make-wordpress-faster-on-iis-with-wincache-1-1/">Hoe u WordPress sneller met Wincache</a> - Wincache is standaard ingeschakeld voor Web Apps</p></li><li><p><a href="../web-sites-scale/">Schaal een WebApp in Azure App-Service</a> en het gebruik <a href="http://www.cleardb.com/developers/cdbr/introduction">ClearDB hoge beschikbaarheid routering</a> of <a href="http://www.mysql.com/products/cluster/">MySQL Cluster CGE</a></p></li></ul>
**BLOB's gebruikt voor de opslag** | <ol><li><p><a href="../storage-create-storage-account/">Maak een account Azure Storage</a></p></li><li><p>Leer hoe u <a href="../cdn-how-to-use/">de inhoud verdeling netwerk (CDN) gebruik</a> naar geografische-gegevens die zijn opgeslagen in BLOB's distribueren.</p></li><li><p>Installeren en configureren van de <a href="https://wordpress.org/plugins/windows-azure-storage/">Azure-opslag voor WordPress Plug-in</a>.</p><p>Zie de <a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">gebruikershandleiding</a>voor gedetailleerde installatie en configuratie-informatie voor de invoegtoepassing.</p> </li></ol>
**E-mail inschakelen** | <a href="https://azure.microsoft.com/en-us/marketplace/partners/sendgrid/sendgrid-azure/">SendGrid</a> gebruik van de Store Azure inschakelen. De <a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified">SendGrid-invoegtoepassing</a> voor WordPress installeren.
**Een aangepaste domeinnaam configureren** | [Een aangepaste domeinnaam in Azure App-Service configureren][customdomain]
**HTTPS inschakelen voor een aangepaste domeinnaam** | [HTTPS inschakelen voor een web-app in Azure App-Service][httpscustomdomain]
**Laden balance '- of geografische-distributie van uw site** | [Doorsturen van verkeer met Azure verkeer Manager][trafficmanager]. Als u een aangepast domein gebruikt, raadpleegt u [een aangepaste domeinnaam in Azure App-Service configureren] [ customdomain] voor informatie over het gebruik van de Manager verkeer met aangepaste domeinnamen
**Automatische back-ups inschakelen** | [Back-up van een WebApp in Azure App-Service][backup]
**Diagnostische gegevens vastleggen** | [Diagnostische gegevens vastleggen voor web-apps in Azure App-Service][log]

## <a name="next-steps"></a>Volgende stappen

* [WordPress optimalisatie](http://codex.wordpress.org/WordPress_Optimization)

* [WordPress converteren naar meerdere locaties in Azure App-Service](web-sites-php-convert-wordpress-multisite.md)

* [ClearDB wizard voor Azure bijwerken](http://www.cleardb.com/store/azure/upgrade)

* [Hostingprovider WordPress in een submap van uw web-app in Azure App-Service](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)

* [Stapsgewijze instructies: Een WordPress-site met Azure maken](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx)

* [Uw bestaande WordPress-blog op Azure hosten](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)

* [Mooie permalinks in WordPress inschakelen](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)

* [Migreren en uitvoeren van uw blog WordPress op Azure App-Service](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)

* [Het uitvoeren van de WordPress gratis op Azure App-Service](http://architects.dzone.com/articles/how-run-wordpress-azure)

* [WordPress op Azure in twee minuten of kleiner](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/)

* [Een blog WordPress verplaatsen naar Azure - deel 1: een blog WordPress op Azure maken](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)

* [Een blog WordPress verplaatsen naar Azure - deel 2: uw inhoud overbrengen](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)

* [Een blog WordPress verplaatsen naar Azure - deel 3: instellen van uw aangepaste domein](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)

* [Een blog WordPress verplaatsen naar Azure - deel 4: mooie permalinks en regels URL herschrijven](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)

* [Een blog WordPress verplaatsen naar Azure - deel 5: vanuit een submap verplaatsen naar de hoofdsite](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)

* [Het instellen van een WordPress web-app in uw Azure-account](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)

* [Propping omhoog WordPress op Azure](http://www.johnpapa.net/wordpress-on-azure/)

* [Tips voor het WordPress op Azure](http://www.johnpapa.net/azurecleardbmysql/)

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

## <a name="whats-changed"></a>Wat er gewijzigd
* Zie voor een handleiding voor het wijzigen van Websites naar App Service: [Azure App-Service en de invloed op bestaande Azure-Services](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- URL List -->

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: web-sites-custom-domain-name.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: web-sites-configure-ssl-certificate.md
[mysqlwindows]: ../virtual-machines/virtual-machines-windows-classic-mysql-2008r2.md
[mysqllinux]: ../virtual-machines/virtual-machines-linux-classic-mysql-on-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: ../powershell-install-configure.md
[Azure CLI]: ../xplat-cli-install.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn/cdn-overview.md
