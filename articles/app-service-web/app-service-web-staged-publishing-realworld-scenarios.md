<properties
  pageTitle="DevOps omgevingen effectief gebruiken voor uw web-app"
  description="Informatie over het gebruik van implementatie sleuven instellen en beheren van meerdere ontwikkelomgeving biedt voor uw toepassing"
  services="app-service\web"
  documentationCenter=""
  authors="sunbuild"
  manager="yochayk"
  editor=""/>

<tags
  ms.service="app-service"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="web"
  ms.date="10/24/2016"
  ms.author="sumuth"/>

# <a name="use-devops-environments-effectively-for-your-web-apps"></a>DevOps omgevingen effectief gebruiken voor uw web-apps

In dit artikel leest u kunt instellen en beheren van web application implementaties voor meerdere versies van uw toepassing zoals ontwikkeling, waarin u tijdelijk opslaat, q & a en productie. Elke versie van uw toepassing kan worden beschouwd als ontwikkelomgeving voor specifieke dat u nodig hebt in uw implementatieproces. Q & a-omgeving kan bijvoorbeeld door uw team van ontwikkelaars worden gebruikt, kunt u de kwaliteit van de toepassing testen voordat u de wijzigingen aan productie push.
Bij het instellen van meerdere development-omgevingen kan een uitdaging zijn als u wilt bijhouden, de resources (berekeningscluster, in de browser, database, cache enz.) beheren en code implementeren in omgevingen.

## <a name="setting-up-a-non-production-environment-stagedevqa"></a>Instellen van een niet-productieve-omgeving (fase, ontwikkelaar, q & a)
Nadat u een productie web-app up-to-date en wordt uitgevoerd hebt, wordt de volgende stap is om een niet-productieve omgeving te maken. Om te kunnen gebruiken zorg implementatie sleuven dat in de **standaard** - of **Premium** App Service abonnement-modus wordt uitgevoerd. Implementatie sleuven zijn daadwerkelijk live WebApps gebruiken met hun eigen hostnamen. Web app-inhoud en configuratie elementen kunnen worden omgewisseld tussen twee implementatie sleuven, inclusief de productie slot. Uw toepassing naar een slot implementatie implementeert heeft de volgende voordelen:

1. U kunt wijzigingen van de web-app in een tijdelijk opslaan implementatie slot valideren voordat u deze met de productie boven verwisselen.
2. Zorgt ervoor dat op alle exemplaren van de slot zijn temperatuur voordat het wordt omgewisseld in gebruik neemt u eerst een WebApp implementatie in een slot en deze in bedrijf verwisselen. Hierdoor downtime wanneer u uw web-app implementeren. De omleiding verkeer naadloze is en geen aanvragen worden verbroken vanwege omwisselen bewerkingen. Deze hele werkstroom kan worden geautomatiseerd door te configureren [Automatisch uitwisselen](web-sites-staged-publishing.md#configure-auto-swap-for-your-web-app) wanneer oude omwisselen gegevensvalidatie niet nodig is.
3. Na een omwisselen bevat de slot met eerder gefaseerde WebApp nu de vorige productie WebApp. Als de wijzigingen die is omgezet in de productie slot zijn niet zoals verwacht, kunt u de dezelfde omwisselen als u uw "laatste bekende goede WebApp" onmiddellijk wilt uitvoeren terug.

Raadpleeg instellen van een tijdelijk opslaan implementatie slot, [tijdelijke omgevingen voor web-apps in Azure App Service instellen](web-sites-staged-publishing.md). Elke omgeving moet een eigen set van resources, opnemen van voorbeeld als u web-app wordt gebruikt een database en vervolgens zowel productie en tijdelijk opslaan web-app moeten verschillende databases gebruiken. Tijdelijk opslaan van ontwikkeling omgeving resources zoals database, opslag of cache voor het instellen van uw tijdelijke ontwikkelomgeving toevoegen.

## <a name="examples-of-using-multiple-development-environments"></a>Voorbeelden van het gebruik van meerdere ontwikkeling omgevingen

Een project zou een bron code management met ten minste twee omgevingen, een ontwikkel- en -omgeving, maar wanneer met behulp van inhoudsbeheersystemen, toepassingskaders enzovoort we kunnen optreden problemen waar de toepassing niet ondersteunt dit scenario uit het vak volgen. Dit geldt voor een deel van de populaire kaders hieronder beschreven. Een groot aantal vragen komt bij mee tijdens het werken met een CMS/kaders zoals

1. Hoe u dit uitfaden opsplitsen in verschillende omgevingen
2. Welke bestanden kan ik wijzigen en lukt van invloed zijn op framework versie updates
3. Het beheren van configuratie per omgeving
4. Het beheren van core framework versie updates-updates versie modules/Plug-ins

Er zijn tal van manieren voor het instellen van een omgeving met meerdere voor uw project en de onderstaande voorbeelden zijn een slechts één van deze manieren de desbetreffende toepassingen.

### <a name="wordpress"></a>WordPress
In deze sectie leert u hoe u een werkstroom voor implementatie met sleuven voor WordPress instelt. WordPress zoals de meeste CMS-oplossingen biedt geen ondersteuning voor het werken met meerdere ontwikkeling omgevingen afmelden bij het vak. App Service Web Apps bevat enkele functies die het gemakkelijker maken om op te slaan configuratie-instellingen buiten uw code.

Voordat u maakt een tijdelijk opslaan slot, uw toepassingscode instellen voor de ondersteuning van meerdere omgevingen. Ondersteuning van meerdere omgevingen in WordPress die u wilt bewerken `wp-config.php` op uw lokale ontwikkeling web-app de volgende code hebt toegevoegd aan het begin van het bestand. Dit is kan de toepassing moet de juiste configuratie op basis van de geselecteerde omgeving kiezen.

```
// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) {
// local development
 $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) || (strpos(getenv('WP_ENV'),'prod' )!== false ))
//single file for all azure development environments
 $config_file = 'config/wp-config.azure.php';
}
$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
// include the config file if it exists, otherwise WP is going to fail
require_once $path. $config_file;
```

Een map maakt onder web app-hoofdsite genoemd `config` en toevoegen van een bestand twee bestanden: `wp-config.azure.php` en `wp-config.local.php` respectievelijk uw azure en lokale omgeving.

Kopieer de volgende handelingen uit in `wp-config.local.php` :

```
<?php
// MySQL settings
/** The name of the database for WordPress */

define('DB_NAME', 'yourdatabasename');

/** MySQL database username */
define('DB_USER', 'yourdbuser');

/** MySQL database password */
define('DB_PASSWORD', 'yourpassword');

/** MySQL hostname */
define('DB_HOST', 'localhost');
/**
 * For developers: WordPress debugging mode.
 * * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 */
define('WP_DEBUG', true);

//Security key settings
define('AUTH_KEY', 'put your unique phrase here');
define('SECURE_AUTH_KEY','put your unique phrase here');
define('LOGGED_IN_KEY','put your unique phrase here');
define('NONCE_KEY', 'put your unique phrase here');
define('AUTH_SALT', 'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT', 'put your unique phrase here');
define('NONCE_SALT', 'put your unique phrase here');

/**
 * WordPress Database Table prefix.
 *
 * You can have multiple installations in one database if you give each a unique
 * prefix. Only numbers, letters, and underscores please!
 */
$table_prefix = 'wp_';
```

Het instellen van de van bovenstaande beveiligingssleutels kan helpen voorkomen dat uw web-app wordt gehackte, dus gebruiken unieke waarden. Als u de tekenreeks voor beveiliging toetsen moet hierboven genoemde genereren, kunt u naar de automatische genereren om te maken van nieuwe sleutels/waarden via deze [koppeling] (https://api.wordpress.org/secret-key/1.1/salt) gaan

Kopieer de volgende code in `wp-config.azure.php`:


``` <?php
    // MySQL settings
    /** The name of the database for WordPress */
    
    define('DB_NAME', getenv('DB_NAME'));
    
    /** MySQL database username */
    define('DB_USER', getenv('DB_USER'));
    
    /** MySQL database password */
    define('DB_PASSWORD', getenv('DB_PASSWORD'));
    
    /** MySQL hostname */
    define('DB_HOST', getenv('DB_HOST'));
    
    /**
    * For developers: WordPress debugging mode.
    *
    * Change this to true to enable the display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging to investigate issues without displaying to end user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on the page */
    
    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);
    
    //Security key settings
    /** If you need to generate the string for security keys mentioned above, you can go the automatic generator to create new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
    define('AUTH_KEY',getenv('DB_AUTH_KEY'));
    define('SECURE_AUTH_KEY', getenv('DB_SECURE_AUTH_KEY'));
    define('LOGGED_IN_KEY', getenv('DB_LOGGED_IN_KEY'));
    define('NONCE_KEY', getenv('DB_NONCE_KEY'));
    define('AUTH_SALT', getenv('DB_AUTH_SALT'));
    define('SECURE_AUTH_SALT', getenv('DB_SECURE_AUTH_SALT'));
    define('LOGGED_IN_SALT',  getenv('DB_LOGGED_IN_SALT'));
    define('NONCE_SALT',  getenv('DB_NONCE_SALT'));
    
    /**
    * WordPress Database Table prefix.
    *
    * You can have multiple installations in one database if you give each a unique
    * prefix. Only numbers, letters, and underscores please!
    */
    $table_prefix = getenv('DB_PREFIX');
```

#### <a name="use-relative-paths"></a>Relatieve paden gebruiken
Tot slot is voor het configureren van de app WordPress als u wilt relatieve paden gebruiken. WordPress bevat URL-gegevens in de database. Hierdoor verplaatsen inhoud van één omgeving naar een andere moeilijker als u nodig hebt bij de database telkens wanneer u van lokale naar de fase of fasen naar productieomgevingen verplaatst. De kans op pesterijen problemen die kan worden veroorzaakt met het implementeren van een database telkens wanneer u van één omgeving naar een ander implementeren gebruik de [relatieve hoofdsite-invoegtoepassing voor koppelingen](https://wordpress.org/plugins/root-relative-urls/) die kan worden geïnstalleerd met WordPress beheerder dashboard of handmatig downloaden vanaf [hier](https://downloads.wordpress.org/plugin/root-relative-urls.zip)om te voorkomen.


Voegt u de volgende gegevens naar uw `wp-config.php` bestand voordat u de `That's all, stop editing!` Opmerking:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

Activeren via de invoegtoepassing voor de `Plugins` menu in WordPress beheerder-dashboard. Permalink instellingen voor WordPress app opslaan.

#### <a name="the-final-wp-configphp-file"></a>Het uiteindelijke `wp-config.php` bestand
Alle WordPress Core-updates is niet van invloed op uw `wp-config.php`, `wp-config.azure.php` en `wp-config.local.php` bestanden. Klik in het einde deze hoe `wp-config.php` er als volgt uit bestand

```
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web web app, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include the config file if it exists, otherwise WP is going to fail
  require_once $path. $config_file;
}

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>Een tijdelijke-mailomgeving instellen
Voorwaarde dat u hebt al een WordPress web-app uitvoeren op Azure Web, meld u aan bij de [Preview-versie van Azure Management portal](http://portal.azure.com) en gaat u naar uw WordPress web-app. Apps als u dit niet kunt maken van marketplace. Meer informatie, [klikt u hier](web-sites-php-web-site-gallery.md).
Klik op instellingen-implementatie > sleuven -> toevoegen om te maken van een slot implementatie met de naam fase. Een slot implementatie is een andere webtoepassing delen dezelfde bronnen als de primaire web-app hiervoor hebt gemaakt.

![Fase implementatie slot maken](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

Stel dat een andere MySQL-database toevoegen `wordpress-stage-db` aan uw resourcegroep `wordpressapp-group`.

 ![MySQL-database toevoegen aan resourcegroep](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

De tekenreeksen verbinding voor uw fase implementatie slot zodat deze verwijzen naar de zojuist gemaakte database bijwerken `wordpress-stage-db`. Opmerking dat uw productie web app, `wordpressprodapp` en tijdelijk opslaan web-app `wordpressprodapp-stage` moet verwijzen naar andere databases.

#### <a name="configure-environment-specific-app-settings"></a>Omgeving / regiospecifieke app-instellingen configureren
Ontwikkelaars kunnen sleutelwaarden tekenreeks paren opslaan in Azure als onderdeel van de configuratie-informatie die is gekoppeld aan een App-instellingen genoemd in de browser. Gedurende runtime, App Service Web Apps automatisch voor u deze waarden zijn opgehaald en beschikbaar maken voor de code die wordt uitgevoerd in uw web-app. Van een waardepapier perspectief die is een nette zijde profiteren sinds gevoelige informatie, zoals tekenreeksen van database-verbinding met een wachtwoord nooit worden weergegeven als normale tekst in een bestand, zoals `wp-config.php`.

Dit proces hieronder is handig als u uitvoeren terwijl deze wijzigingen in bestand zowel wijzigingen in de database voor WordPress app bevat:
- WordPress versie-upgrade
- Nieuwe toevoegen of bewerken of Plug-ins upgraden
- Nieuwe toevoegen of bewerken of het upgraden van thema 's

App-instellingen voor configureren:

- databasegegevens
- in- / uitschakelen WordPress logboekregistratie inschakelen
- Beveiligingsinstellingen WordPress

![App-instellingen voor Wordpress WebApp](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Zorg ervoor dat u de volgende app-instellingen voor uw productie web app en fase slot hebt toegevoegd. Houd er rekening mee dat de productie WebApp en de tijdelijke WebApp verschillende databases gebruiken.
Schakel het selectievakje **Slot instelling** voor de instellingen voor parameters behalve WP_ENV. Hiermee wordt de configuratie voor uw web-app, samen met de bestandsinhoud en database uitwisselen. Als **Slot instelling** **ingeschakeld is**, app-instellingen voor de web-app en configuratie van de tekenreeks verbinding niet verplaatst in omgevingen bij het uitvoeren van een bewerking UITWISSELEN en daarom als databasewijzigingen aanwezig zijn dit niet verbroken uw productie web-app.

De plaatselijke ontwikkeling omgeving web-app dashboard implementeren naar fase WebApp en de database met behulp van WebMatrix of hulpprogramma('s) van uw keuze zoals FTP, cijfer of PhpMyAdmin.

![Matrix publiceren web-dialoogvenster voor WordPress WebApp](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

Blader en test uw tijdelijk opslaan web-app. Gezien een scenario waarbij het thema van de web-app worden bijgewerkt, volgt de tijdelijke web-app.

![Blader tijdelijk opslaan in de browser voordat sleuven verwisselen](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)


 Als alle er goed uitziet, klikt u op op de knop **uitwisselen** op uw web-app uw inhoud verplaatsen naar de productieomgeving tijdelijk opslaan. In dit geval uitwisselen u de web-app en de database in omgevingen tijdens elke **uitwisselen** .

![Voorbeeld van wijzigingen voor WordPress uitwisselen](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

 > [AZURE.NOTE]
 >Als u een scenario waarbij u alleen push-bestanden (geen updates van de database moet) hebt, klikt u vervolgens gerelateerde **controleren** de **Slot instelling** voor alle de database *app-instellingen* en *verbindingsinstellingen tekenreeksen* in web-app instelling blade binnen de portal Azure preview voordat u het OMWISSELEN uitvoert. In dit geval DB_NAME, DB_HOST, DB_PASSWORD, DB_USER, moet standaardinstelling verbinding tekenreeks niet weergegeven in de preview-wijzigingen bij het uitvoeren van een **uitwisselen**. AT lang, wanneer u volledige de bewerking **uitwisselen** de WordPress web-app wordt de updates zijn **alleen**bestanden.

Voordat u een OMWISSELEN doet, volgt u de web-app van productie WordPress ![productie in de browser voordat sleuven verwisselen](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

Na de bewerking OMWISSELEN, is het thema op uw web-app voor productie bijgewerkt.

![De WebApp productie na sleuven verwisselen](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

In een situatie als u **terugdraaien wilt**, kunt u Ga naar de productie web app-instellingen en klik op de knop **omwisselen** naar de WebApp en de database uit voor het tijdelijk opslaan slot uitwisselen. Een belangrijk punt te onthouden is dat als wijzigingen in de database opgenomen in een bewerking **uitwisselen** op elk gewenst moment zijn, klikt u vervolgens de volgende keer dat u opnieuw dashboard naar uw tijdelijk opslaan web-app implementeren, moet u de database implementeren gewijzigd in de huidige database voor het tijdelijk opslaan web-app dat mogelijk de vorige productiedatabase of de fase-database.

#### <a name="summary"></a>Overzicht
Het proces voor een toepassing met een database generaliseren

1. Installatie van toepassing op uw lokale omgeving
2. Configuratie van de omgeving specifieke (lokale en Azure Web App) opnemen
3. Uw omgevingen in App Service Web Apps-tijdelijke, productie instellen
4. Als er een productietoepassing op Azure is gebeurd, Synchroniseer uw productie-inhoud (bestanden/code + database) bij lokale en tijdelijk opslaan-omgeving.
5. Ontwikkel uw toepassing op uw lokale omgeving
6. Plaats uw web-app productie onder onderhoud of vergrendelde modus en synchronisatiedatabase inhoud uit voor tijdelijke en ontwikkelaar omgevingen
7. Implementeren naar tijdelijke omgeving en testen
8. Implementeren naar productieomgeving
9. Herhaal stap 4 tot en met 6

### <a name="umbraco"></a>Umbraco
In deze sectie leert u hoe de CMS Umbraco een aangepaste module gebruikt om het implementeren van over meerdere DevOps-omgeving. In dit voorbeeld biedt u een andere methode voor het beheren van meerdere development-omgevingen.

[Umbraco CMS](http://umbraco.com/) is een van de popular.NET CMS oplossingen die worden gebruikt door veel ontwikkelaars waarmee [Courier2](http://umbraco.com/products/more-add-ons/courier-2) module implementeren van ontwikkeling naar tijdelijke naar productieomgevingen. U kunt eenvoudig een lokale ontwikkelomgeving voor een Umbraco CMS-web-app via Visual Studio of WebMatrix maken.

1. Maak een Umbraco web-app met Visual Studio, [klikt u hier](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget).
2. Een Umbraco web-app maken met WebMatrix, [klikt u hier](http://umbraco.com/help-and-support/video-tutorials/getting-started/working-with-webmatrix).

Altijd Vergeet niet te verwijderen de `install` map onder uw toepassing en nooit uploadt dit naar de fase of productie web-apps. Voor deze zelfstudie ik gebruikt WebMatrix

#### <a name="set-up-a-staging-environment"></a>Een testomgeving instellen
- Maak een slot implementatie bovengenoemde voor Umbraco CMS WebApp, ervan uitgaande dat u al een Umbraco CMS-web-app omhoog en uit te voeren. Als dit niet kunt u een van de marketplace.

- Werk de verbindingsreeks voor uw fase implementatie slot zodat deze verwijzen naar de zojuist gemaakte database, **umbraco-fase-db**. Uw productie WebApp (umbraositecms-1) en het tijdelijk opslaan in de browser (umbracositecms-1-fase) **moet** verwijzen naar andere databases.

![Verbindingsreeks voor de gefaseerde WebApp met de nieuwe tijdelijk opslaan database bijwerken](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

- Klik op **ophalen publicatie-instellingen** voor de implementatie slot **fase**. Hiermee wordt een publiceren instellingenbestand downloaden dat alle gegevens Visual Studio of Web Matrix vereist om het publiceren van uw toepassing vanuit lokale ontwikkeling WebApp naar Azure WebApp op te slaan.

 ![Get publicatie-instelling van het tijdelijk opslaan web-app](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)

- Open de plaatselijke ontwikkeling web-app in **WebMatrix** of **Visual Studio**. In deze zelfstudie ik Web Matrix gebruik en moet u eerst het bestand publiceren instellingen voor het tijdelijk opslaan web-app importeren

![Publicatie-instellingen voor Umbraco met behulp van de Matrix Web importeren](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

- Wijzigingen in het dialoogvenster controleren en implementeren van uw lokale web-app op uw Azure web-apps, *umbracositecms-1-fase*. Wanneer u bestanden rechtstreeks in uw tijdelijk opslaan web-app implementeren wordt u weglaat bestanden in de `~/app_data/TEMP/` map terwijl deze worden gegenereerd wanneer de fase web-app de eerste keer gestart. U moet ook weglaten de `~/app_data/umbraco.config` bestand als dit doet, wordt opnieuw worden gegenereerd.

![Publiceren wijzigingen controleren in de web-matrix](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

- Na het succes publiceren de Umbraco lokale web-app naar tijdelijke WebApp, bladert u naar uw tijdelijk opslaan web-app en uitvoeren van een paar tests sprake is van eventuele problemen.

#### <a name="set-up-courier2-deployment-module"></a>Courier2 implementatie module instellen
Met [Courier2](http://umbraco.com/products/more-add-ons/courier-2) module kunt u inhoud, opmaakmodellen, ontwikkeling modules push en meer met een eenvoudige met de rechtermuisknop op uit een tijdelijk opslaan in de browser naar productie WebApp voor een meer gratis implementaties van problemen en risico's van uw web-app productie verbreken wanneer een update implementeert verminderen.
Een licentie voor Courier2 kopen voor het domein `*.azurewebsites.net` en uw aangepaste domein (bijvoorbeeld http://abc.com) nadat u de licentie hebt aangeschaft, plaatst u de gedownloade licentie (. LIC-bestand) in de `bin` map.

![Decoratieve licentiebestand onder de map bin](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

De Courier2 downloaden vanaf [hier](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Meld u aan bij uw web-app voor fase, zeg http://umbracocms-site-stage.azurewebsites.net/umbraco en klik op **ontwikkelaars** Menu en selecteer **pakketten**. Klik op de lokale pakket **installeren**

![Umbraco installatieprogramma](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

Upload het courier2-pakket met het installatieprogramma.

![Inpakken voor courier module uploaden](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

Als u wilt configureren u moeten bijwerken courier.config bestand onder de map **Config** van uw web-app.

```xml
<!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1.azurewebsites.net</url>
      <user>0</user>
      <!--<login>user@email.com</login> -->
      <!-- <password>user_password</password>-->
      <!-- <passwordEncoding>Clear</passwordEncoding>-->
      </repository>
 </repositories>
 ```

Klik onder `<repositories>`, voert u de URL en de gebruiker-gegevens voor de site voor productie. Als u standaard Umbraco lidmaatschap provider gebruikt, voegt u de ID voor het beheer van de gebruiker in <user> sectie. Als u een aangepaste Umbraco lidmaatschapsprovider gebruikt, gebruikt u `<login>`,`<password>` naar Courier2 module weten hoe u verbinding maken met de productiesite. Bekijk de [documentatie](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation) voor Courier-module voor meer informatie.

Op dezelfde manier Courier module installeren op uw productiesite en configureert u deze punt bij fase WebApp in de desbetreffende courier.config-bestand zoals hier wordt getoond

```xml
 <!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1-stage.azurewebsites.net</url>
      <user>0</user>
      </repository>
 </repositories>
```

Klik op tabblad Courier2 in Umbraco CMS web app-dashboard en schakelt u locaties. Ziet u de naam van de bibliotheek zoals vermeld `courier.config`. Deze stap herhalen op zowel uw productie als tijdelijke Webapps.

![Web weergave-app doelopslagplaats](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

U kunt nu bepaalde inhoud van site tijdelijk opslaan naar productielocatie implementeren. Ga naar de inhoud en selecteer een bestaande pagina of een nieuwe pagina maken. Ik een bestaande pagina selecteren vanuit mijn web-app waarin de titel van de pagina **aan** de slag – nieuwe wordt gewijzigd en nu Klik op **Opslaan en publiceren**.

![Titel van pagina wijzigen en publiceren](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

Selecteer nu de gewijzigde pagina en *met de rechtermuisknop op* alle opties weergeven. Klik op **Courier** implementatie dialoogvenster weergeven. Klik op **Deploy** implementatie starten

![Courier module implementatie dialoogvenster](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

De wijzigingen bekijken en klik op Doorgaan.

![Wijzigingen voor Courier module implementatie dialoogvenster controleren](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

Implementatie log ziet als de implementatie geslaagd is.

 ![Logboeken aan de implementatie van Courier module weergeven](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

Blader uw productie web-app om te zien als de wijzigingen zijn doorgevoerd.

 ![Blader productie WebApp](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Meer informatie over het gebruik van Courier, moet u de documentatie doornemen.

#### <a name="how-to-upgrade-umbraco-cms-version"></a>Umbraco CMS versie upgraden

Courier helpt niet met een upgrade van de ene versie van Umbraco CMS naar een andere implementeren. Bij een upgrade Umbraco CMS-versie, moet u controleren op compatibiliteitsproblemen met uw aangepaste modules of derden modules en de Umbraco Core-bibliotheken. Als een aanbevolen procedure

1. ALTIJD back-up uw WebApp en de database voordat u een upgrade uitvoert. Klik op Azure Web App, kunt u instellen automatische back-ups voor uw websites met de back-up aanbevelen en herstellen van uw site als functie gebruik terugzetten als u een nodig. Zie [hoe u de back-up van uw web-app](web-sites-backup.md) en [hoe uw web-app herstellen](web-sites-restore.md)voor meer informatie.

2. Controleer of de derde partij-pakketten die u gebruikt compatibel met de versie die u een uitvoert zijn naar upgrade. Klik op van het pakket downloadpagina, bekijkt u de Project-compatibiliteit met Umbraco CMS-versie.

Voor meer informatie over het bijwerken van uw web-app lokaal, volgt u de richtlijnen als genoemde [hier](https://our.umbraco.org/documentation/getting-started/set up/upgrading/general).

Nadat uw lokale development-site wordt bijgewerkt, publiceert u de wijzigingen naar waarin u tijdelijk opslaat in de browser. Test uw toepassing en als alle er goed uitziet, gebruikt u **uitwisselen** knop **uitwisselen** uw tijdelijk opslaan bij productie WebApp-site. Wanneer de bewerking **uitwisselen** wordt uitgevoerd, kunt u de wijzigingen worden beïnvloed weergeven in uw web-app configuratie. Met deze bewerking **uitwisselen** zijn we de WebApps en databases verwisselen. Dit betekent, nadat het OMWISSELEN de productie web-app wordt nu wijs umbraco-fase-db database en tijdelijk opslaan in de browser wordt met umbraco-productcatalogus-db-database.

![Voorbeeld voor het implementeren van Umbraco CMS uitwisselen](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Het voordeel van verwisselen zowel de WebApp en de database:
1. Kunt u terugkeren naar de vorige versie van uw web-app met een ander **uitwisselen** als er problemen toepassing zijn.
2. Voor een upgrade moet u bestanden en -database waarin u tijdelijk opslaat in de browser naar productie WebApp en database implementeren. Zijn er veel zaken aan bod die fout kunnen gaan bij het implementeren van bestanden en -database. We kunnen met behulp van de functie **omwisselen** van sleuven downtime reduceren tijdens een upgrade en verkleinen van de kans op pesterijen fouten die optreden kunnen bij het implementeren van wijzigingen.
3. Kunt u de mogelijkheid om te doen **A / B testen** [testen in productie](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) gebruiken

In dit voorbeeld ziet u de flexibiliteit van het platform waarbij u aangepaste modules die vergelijkbaar is met de module Umbraco Courier implementatie beheren in omgevingen kunt maken.

## <a name="references"></a>Verwijzingen
[Agile software development met Azure App-Service](app-service-agile-software-development.md)

[Tijdelijke omgevingen voor web-apps in Azure-Service voor App instellen](web-sites-staged-publishing.md)

[Hoe webtoegang tot de implementatie van niet-productieve sleuven blokkeren](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
