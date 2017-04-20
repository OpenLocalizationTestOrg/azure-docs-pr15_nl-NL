<properties
    pageTitle="Azure App Service WebApp geavanceerde config en uitbreidingen"
    description="Gebruik XML-Document Transformation(XDT) declaraties transformeren van het bestand ApplicationHost.config in uw web-app van Azure App-Service en privé extensies om in te schakelen beheer van de aangepaste acties toevoegen."
    authors="cephalin"
    writer="cephalin"
    editor="mollybos"
    manager="wpickett"
    services="app-service"
    documentationCenter=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="azure-app-service-web-app-advanced-config-and-extensions"></a>Azure App Service WebApp geavanceerde config en uitbreidingen

Met behulp van [XML-Document-transformatie](http://msdn.microsoft.com/library/dd465326.aspx) (XDT) declaraties, kunt u het bestand [ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig) transformeren in uw web-app in Azure App-Service. U kunt ook XDT declaraties privé extensies om in te schakelen van aangepaste web-app beheer van de acties toevoegen. In dit artikel bevat een voorbeeld PHP Manager web app extensie waarmee beheer van PHP instellingen via een webservice-interface.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

##<a id="transform"></a>Geavanceerde configuratie via ApplicationHost.config
Het platform App Service biedt flexibiliteit en controle voor de configuratie van de web-app. Hoewel de standaard ApplicationHost.config IIS-configuratiebestand niet beschikbaar is voor directe bewerken in een App-Service, ondersteunt het platform een declaratieve ApplicationHost.config transformeren model dat is gebaseerd op XML-Document transformatie (XDT).

Als u wilt gebruikmaken van deze functionaliteit transformeren, maakt u een ApplicationHost.xdt-bestand met XDT-inhoud en plaats onder de hoofdmap van de site (d:\home\site) in de [Kudu Console](https://github.com/projectkudu/kudu/wiki/Kudu-console). Mogelijk moet u de Web-App voor wijzigingen pas van kracht opnieuw starten.

In het onderstaande applicationHost.xdt voorbeeld ziet hoe u een nieuwe aangepaste omgevingsvariabele toevoegen aan een web-app waarin PHP 5.4.

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.webServer>
                <fastCgi>
                    <application>
                        <environmentVariables>
                                <environmentVariable name="CONFIGTEST" value="TEST" xdt:Transform="Insert" xdt:Locator="XPath(/configuration/system.webServer/fastCgi/application[contains(@fullPath,'5.4')]/environmentVariables)" />
                        </environmentVariables>
                    </application>
                </fastCgi>
        </system.webServer>
    </configuration>


Een logboekbestand met transformeren status en details is beschikbaar via de FTP-hoofdmap onder LogFiles\Transform.

Raadpleeg [https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples)voor aanvullende voorbeelden.

**Opmerking**<br />
Elementen in de lijst met modules onder `system.webServer` niet kan worden verwijderd of nabesteld, maar toevoegingen aan de lijst zijn mogelijk.


##<a id="extend"></a>Uw web-app uitbreiden

###<a id="overview"></a>Overzicht van privé-web-app extensies

App-Service ondersteunt web app-extensies als een uitbreidingspunt voor beheertaken. Sommige App Service platform-functies worden in feite geïmplementeerd als vooraf geïnstalleerde uitbreidingen. Terwijl de vooraf geïnstalleerde platform-extensies kunnen niet worden gewijzigd, kunt u maken en configureren van privé uitbreidingen voor uw eigen web-app. Deze functionaliteit is ook is afhankelijk van XDT declaraties. De belangrijkste stappen voor het maken van een privé web app-extensie zijn de volgende:

1. -App extensie **inhoud**: een webtoepassing worden ondersteund door de Service-App maken
2. Web app-extensie **declaratie**: een ApplicationHost.xdt-bestand maken
3. Web app-extensie- **implementatie**: inhoud in de map SiteExtensions onder plaatsen`root`

Interne koppelingen voor de web-app moet verwijzen naar een pad ten opzichte van de opgegeven in het bestand ApplicationHost.xdt pad naar de toepassing. Er een wijziging naar het bestand ApplicationHost.xdt is vereist voor de Prullenbak van een web-app.

**Opmerking**: als u meer informatie voor deze belangrijkste elementen beschikbaar is binnen [https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions](https://github.com/projectkudu/kudu/wiki/Azure-Site-Extensions).

Een gedetailleerd voorbeeld wordt om te illustreren de stappen voor het maken en inschakelen van een privé web app-extensie opgenomen. De broncode voor het volgende voorbeeld PHP Manager kan worden gedownload van [https://github.com/projectkudu/PHPManager](https://github.com/projectkudu/PHPManager).

###<a id="SiteSample"></a>Voorbeeld van web app-extensie: PHP Manager

PHP Manager is de uitbreiding van een web-app waarmee web app-beheerders eenvoudig bekijken en hun PHP instellingen configureren via een webservice-interface in plaats van te PHP ini-bestanden rechtstreeks wijzigen. Algemene configuratiebestanden voor PHP opnemen het php.ini-bestand bevindt zich onder Program Files en de. user.ini bestand in de hoofdmap van uw web-app. Aangezien het bestand php.ini zich niet rechtstreeks bewerkt op het App-Service-platform, gebruikmaakt van de extensie PHP Manager de. user.ini bestand instelling wijzigingen toepassen.

####<a id="PHPwebapp"></a>De webtoepassing PHP Manager

Hieronder ziet u de startpagina van de Manager PHP-implementatie.

![TransformSitePHPUI][TransformSitePHPUI]

Zoals u ziet, is een web app-extensie net als een gewone webtoepassing, maar met een extra ApplicationHost.xdt-bestand in de hoofdmap van de web-app (meer informatie over het bestand ApplicationHost.xdt zijn beschikbaar in het volgende gedeelte van dit artikel) geplaatst.

De extensie PHP Manager is gemaakt met de Visual Studio ASP.NET MVC 4 Web Application-sjabloon. De weergave volgen in Solution Explorer ziet u de structuur van de extensie PHP Manager.

![TransformSiteSolEx][TransformSiteSolEx]

De enige speciale logica voor bestand I/O nodig is om aan te geven waar de map wwwroot van de web-app zich bevindt. Als u het volgende voorbeeld wordt weergegeven, omgeving variabele 'thuis' geeft aan dat het pad naar de hoofdmap van de web-app en het pad wwwroot kan worden samengesteld door 'site\wwwroot' toe te voegen:

    /// <summary>
    /// Gives the location of the .user.ini file, even if one doesn't exist yet
    /// </summary>
    private static string GetUserSettingsFilePath()
    {
            var rootPath = Environment.GetEnvironmentVariable("HOME"); // For use on Azure Websites
            if (rootPath == null)
            {
                rootPath = System.IO.Path.GetTempPath(); // For testing purposes
            };
            var userSettingsFile = Path.Combine(rootPath, @"site\wwwroot\.user.ini");
            return userSettingsFile;
    }


Nadat u het pad van de hebt, kunt u normale bestand i/o-bewerkingen te lezen en schrijven naar bestanden.

Één punt rekening met de web-app extensies met betrekking tot de afhandeling van interne koppelingen.  Als u alle koppelingen in uw HTML-bestanden die de absolute paden aan interne koppelingen op uw web-app geven hebt, moet u ervoor zorgen dat deze koppelingen worden voorafgegaan door de Extensienaam als de hoofdsite. Dit is nodig omdat de hoofdmap voor uw extensie is "/`[your-extension-name]`/ ' in plaats van dat wordt alleen"/", zodat een interne koppelingen dienovereenkomstig gewijzigd moeten worden bijgewerkt. Stel dat uw code bevat een koppeling naar het volgende:

`"<a href="/Home/Settings">PHP Settings</a>"`

Wanneer de koppeling deel van een web app-extensie uitmaakt, moet de koppeling in de volgende notatie:

`"<a href="/[your-site-name]/Home/Settings">Settings</a>"`

U kunt deze vereiste omzeilen door hetzij alleen relatieve paden binnen uw webtoepassing of in het geval van ASP.NET-toepassingen gebruiken met behulp van de `@Html.ActionLink` methode die de desbetreffende koppelingen voor u gemaakt.

####<a id="XDT"></a>Het bestand applicationHost.xdt

Hiermee gaat u de code voor uw web app-extensie onder %HOME%\SiteExtensions\[uw-extensie-naam]. Wij bellen dit naar de hoofdsite van de extensie.  

Als u wilt uw web app-extensie hebt geregistreerd met het bestand applicationHost.config, moet u een bestand met de naam van ApplicationHost.xdt in de hoofdmap van de extensie plaatsen. De inhoud van het bestand ApplicationHost.xdt ziet er als volgt uit:

    <?xml version="1.0"?>
    <configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
        <system.applicationHost>
                <sites>
                    <site name="%XDT_SCMSITENAME%" xdt:Locator="Match(name)">
                        <!-- NOTE: Add your extension name in the application paths below -->
                        <application path="/[your-extension-name]" xdt:Locator="Match(path)" xdt:Transform="Remove" />
                        <application path="/[your-extension-name]" applicationPool="%XDT_APPPOOLNAME%" xdt:Transform="Insert">
                            <virtualDirectory path="/" physicalPath="%XDT_EXTENSIONPATH%" />
                        </application>
                    </site>
                </sites>
        </system.applicationHost>
    </configuration>

De naam die u als de Extensienaam van de selecteert moet dezelfde naam hebben als de hoofdmap van de extensie.

Dit is het effect van het toevoegen van een nieuwe toepassingspad naar de `system.applicationHost` lijst met sites onder de SCM-site. De SCM-site is een eindpunt beheer van de site met specifieke toegangsreferenties. Dit is de URL `https://[your-site-name].scm.azurewebsites.net`.  

    <system.applicationHost>
        ...
        <site name="~1[your-website]" id="1716402716">
                <bindings>
                    <binding protocol="http" bindingInformation="*:80: [your-website].scm.azurewebsites.net" />
                    <binding protocol="https" bindingInformation="*:443: [your-website].scm.azurewebsites.net" />
                </bindings>
                <traceFailedRequestsLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles" />
                <detailedErrorLogging enabled="false" directory="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\LogFiles\DetailedErrors" />
                <logFile logSiteId="false" />
                <application path="/" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="D:\Program Files (x86)\SiteExtensions\Kudu\1.24.20926.5" />
                </application>
                <!-- Note the custom changes that go here -->
                <application path="/[your-extension-name]" applicationPool="[your-website]">
                    <virtualDirectory path="/" physicalPath="C:\DWASFiles\Sites\[your-website]\VirtualDirectory0\SiteExtensions\[your-extension-name]" />
                </application>
            </site>
    </sites>
      ...
    </system.applicationHost>

###<a id="deploy"></a>Web app-extensie voor implementatie

Als u wilt de extensie voor uw web-app hebt geïnstalleerd, kunt u FTP gebruiken om te kopiëren van alle bestanden van uw webtoepassing naar de `\SiteExtensions\[your-extension-name]` map van de web-app waarop u wilt installeren van de extensie.  Zorg ervoor dat de ApplicationHost.xdt-bestand kopiëren naar deze locatie ook. Start opnieuw op uw web-app als de extensie wilt inschakelen.

U zou moeten kunnen zien van uw toestelnummer van de web-app op:

`https://[your-site-name].scm.azurewebsites.net/[your-extension-name]`

Houd er rekening mee dat de URL er ongeveer net als de URL voor uw web-app, hetzelfde behalve dat dit HTTPS gebruikt en ".scm" bevat.

Het is mogelijk om te schakelen van alle persoonlijke (niet vooraf geïnstalleerde) uitbreidingen voor uw web-app tijdens de ontwikkeling en onderzoeken door de instellingen van een app toe te voegen met de toets `WEBSITE_PRIVATE_EXTENSIONS` en een waarde van `0`.

>[AZURE.NOTE] Als u aan de slag met Azure App Service wilt voordat u zich registreert voor een Azure-account, gaat u naar de [App-Service probeert](http://go.microsoft.com/fwlink/?LinkId=523751), waar u direct een tijdelijk starter in de browser in de App-Service maken kunt. Geen creditcards vereist; geen verplichtingen.

## <a name="whats-changed"></a>Wat er gewijzigd
* Zie voor een handleiding voor het wijzigen van Websites naar App Service: [Azure App-Service en de invloed op bestaande Azure-Services](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[TransformSitePHPUI]: ./media/web-sites-transform-extend/TransformSitePHPUI.png
[TransformSiteSolEx]: ./media/web-sites-transform-extend/TransformSiteSolEx.png
 
