<properties
    pageTitle="Een cloudservice integreren met Azure CDN | Microsoft Azure"
    description="Een zelfstudie waarin u u hoe leert u een cloudservice dat inhoud van een geïntegreerde Azure CDN-eindpunt fungeert implementeren"
    services="cdn, cloud-services"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor="tysonn"/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>


# <a name="intro"></a>Een cloudservice integreren met Azure CDN

Een cloudservice kan worden geïntegreerd met Azure CDN, alle inhoud van de cloudservice locatie. Deze methode kunt u de volgende voordelen:

- Eenvoudig distribueren en afbeeldingen, scripts en opmaakmodellen in adreslijsten van uw cloudservice project bijwerken
- Eenvoudige upgrade NuGet pakketten in uw cloudservice, zoals jQuery of Bootstrap versies
- Uw webtoepassing en uw CDN served inhoud alle uit dezelfde Visual Studio interface beheren
- Geïntegreerd implementatie werkstroom voor uw webtoepassing en uw inhoud CDN served
- ASP.NET bundeling en minification integreren met Azure CDN

## <a name="what-you-will-learn"></a>Wat u leert ##

In deze zelfstudie leert u hoe u:

-   [Een eindpunt Azure CDN integreren met uw cloudservice en dienen van statische inhoud in uw webpagina's van Azure CDN](#deploy)
-   [Cache-instellingen naar statische inhoud in de cloudservice configureren](#caching)
-   [Inhoud van de acties controller via Azure CDN dienen](#controller)
-   [Serveren totdat gebundelde en inhoud via Azure CDN minified behoud van het script-ervaring in Visual Studio foutopsporing](#bundling)
-   [Gebruik uw scripts en CSS configureren als uw CDN Azure offline is](#fallback)

## <a name="what-you-will-build"></a>Wat u wordt maken ##

U wordt een cloud-service Webrol met behulp van de sjabloon ASP.NET MVC implementeren, code toevoegen om te dienen inhoud uit een geïntegreerde Azure CDN, zoals een afbeelding, controller actie resultaten en de standaard JavaScript en CSS-bestanden, en ook code schrijven om te configureren de fallback om pakketten served in het geval dat de CDN offline is.

## <a name="what-you-will-need"></a>Wat u nodig hebt ##

Deze zelfstudie heeft de volgende vereisten:

-   Een actieve [Microsoft Azure-account](/account/)
-   Visual Studio-2015 met [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)

> [AZURE.NOTE] U hebt een Azure-account om te voltooien van deze zelfstudie nodig:
> + U kunt [een Azure-account gratis openen](/pricing/free-trial/) - u tegoeden krijgt u kunt uitproberen betaalde Azure services en zelfs nadat ze gebruikt afgerond kunt u het account en gebruik vrij te geven Azure services, zoals Websites.
> + U kunt [MSDN abonnee voordelen activeren](/pricing/member-offers/msdn-benefits-details/) : uw MSDN-abonnement kunt u tegoeden elke maand die u voor betaalde Azure-services gebruiken kunt.

<a name="deploy"></a>
## <a name="deploy-a-cloud-service"></a>Een cloudservice implementeren ##

In deze sectie, wordt u standaard ASP.NET MVC toepassingssjabloon in Visual Studio-2015 implementeren naar een rol cloud service Web en klikt u vervolgens integreren met een nieuw CDN-eindpunt. Volg de onderstaande instructies:

1. Maak in Visual Studio-2015 verlengt, een nieuwe Azure cloudservice in het menu door te gaan naar **Bestand > Nieuw > Project > Cloud > Azure-Cloudservice**. Een naam geven en klik op **OK**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)

2. Selecteer **ASP.NET Webrol** en klik op de **>** knop. Klik op OK.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)

3. Selecteer **MVC** en klik op **OK**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)

4. Nu deze rol Web publiceren naar een Azure cloudservice. Met de rechtermuisknop op het project cloud-service en selecteer **publiceren**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)

5. Als u hebt nog niet aangemeld bij Microsoft Azure, klikt u op de vervolgkeuzelijst **... een account toevoegen** en klik op het menu-item van **een account toevoegen** .

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)

6. In de aanmeldingspagina, meld u aan met het Microsoft-account dat u gebruikt om uw Azure-account te activeren.
7. Nadat u bent aangemeld, klikt u op **volgende**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)

8. Ervan uitgaande dat een cloud-service of opslag-account kunt u dit nog niet hebt gemaakt, kunt Visual Studio u beide maken. Typ de naam van de gewenste service en selecteer het gewenste gebied in het dialoogvenster **Cloudservice maken en -Account** . Klik vervolgens op **maken**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)

9. Op de pagina publiceren, Controleer de configuratie en klik op **publiceren**.

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)

    >[AZURE.NOTE] Het publicatieproces voor cloudservices duurt lang. De inschakelen Web implementeren voor alle rollen optie kunt aanbrengen voor foutopsporing in uw cloudservice veel sneller doordat snel (maar tijdelijke) updates aan uw Web-rollen. Zie [een Cloudservice met de hulpmiddelen Azure publiceren](http://msdn.microsoft.com/library/ff683672.aspx)voor meer informatie over deze optie.

    Wanneer het **Logboek met Microsoft Azure activiteit** wordt weergegeven dat status publiceren **voltooid is**, maakt u een CDN-eindpunt dat geïntegreerd met deze cloudservice.

    >[AZURE.WARNING] Als na publiceren, wordt in de cloudservice gedistribueerde een foutenscherm wordt weergegeven, is het waarschijnlijk omdat de cloudservice die u hebt geïmplementeerd [Gast OS .NET 4.5.2 geen bevat](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates).  U kunt dit probleem omzeilen door te [implementeren .NET 4.5.2 als een taak opstarten](../cloud-services/cloud-services-dotnet-install-dotnet.md).

## <a name="create-a-new-cdn-profile"></a>Een nieuw CDN-profiel maken

Een profiel CDN is een verzameling CDN-eindpunten.  Elk profiel bevat een of meer CDN-eindpunten.  U kunt meerdere profielen gebruiken om te organiseren van uw eindpunten CDN op internet domein, webtoepassing of andere criteria.

> [AZURE.TIP] Als u al een CDN-profiel dat u wilt gebruiken voor deze zelfstudie hebt, gaat u naar [een nieuw CDN-eindpunt maken](#create-a-new-cdn-endpoint).

[AZURE.INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]

## <a name="create-a-new-cdn-endpoint"></a>Maak een nieuwe CDN-eindpunt

**Een nieuwe CDN-eindpunt voor uw account opslag maken**

1. Ga in de [Beheerportal van Azure](https://portal.azure.com)aan uw profiel CDN.  U mogelijk hebt vastgemaakt deze aan het dashboard in de vorige stap.  Als u niet, u vinden kunt door te **Bladeren**en vervolgens **CDN-profielen**klikken en te selecteren in het profiel dat u van plan om uw eindpunt aan toe.

    Het blad CDN-profiel wordt weergegeven.

    ![CDN-profiel][cdn-profile-settings]

2. Klik op de knop **Eindpunt toevoegen** .

    ![Knop eindpunt toevoegen][cdn-new-endpoint-button]

    Het blad **toevoegen een eindpunt** wordt weergegeven.

    ![Eindpunt blade toevoegen][cdn-add-endpoint]

3. Voer een **naam** voor dit CDN-eindpunt.  Deze naam wordt gebruikt voor toegang tot uw in de cache bronnen van het domein `<EndpointName>.azureedge.net`.

4. Selecteer in de vervolgkeuzelijst **Origin type** *cloudservice*.  

5. Selecteer in de vervolgkeuzelijst **Origin hostname** uw cloudservice.

6. Laat de standaardinstellingen voor **Origin pad**, **Origin host kop**en **Protocol/Origin poort**.  U moet ten minste één protocol (HTTP of HTTPS) opgeven.

7. Klik op de knop **toevoegen** om te maken van het nieuwe eindpunt.

8. Nadat het eindpunt is gemaakt, wordt deze weergegeven in een lijst met eindpunten voor het profiel. De lijstweergave ziet u de URL die u wilt gebruiken voor toegang tot inhoud in cache, alsmede het oorspronkelijke domein.

    ![CDN-eindpunt][cdn-endpoint-success]

    > [AZURE.NOTE] Het eindpunt wordt onmiddellijk pas weer beschikbaar voor gebruik.  Het kan maximaal 90 minuten duren voordat de registratie via het netwerk CDN doorgeven. Gebruikers die u probeert te gebruiken van de domeinnaam CDN onmiddellijk wordt statuscode 404 totdat de inhoud beschikbaar via de CDN is.

## <a name="test-the-cdn-endpoint"></a>Het eindpunt CDN testen

Wanneer de status van de publicatie **voltooid is**, open een browservenster en navigeer naar * *http://<cdnName>*.azureedge.net/Content/bootstrap.css**. In mijn instelling is deze URL:

    http://camservice.azureedge.net/Content/bootstrap.css

Die overeenkomt met de volgende URL origin bij het eindpunt CDN:

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

Wanneer u navigeren naar * *http://*&lt;cdnName >*.azureedge.net/Content/bootstrap.css**, afhankelijk van uw browser, wordt u gevraagd om te downloaden of de bootstrap.css die u hebt gekregen van uw gepubliceerde Web-app te openen.

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

U hebt ook toegang tot elke openbaar URL op * *http://*&lt;servicenaam >*.cloudapp.net/**, rechtstreeks vanuit uw CDN-eindpunt. Bijvoorbeeld:

-   Een JS-bestand uit het pad/script
-   Een inhoudsbestand uit de/inhoud pad
-   Geen controller/actie
-   Als de queryreeks is ingeschakeld op uw CDN-eindpunt, een URL met querytekenreeksen met de

Ja, met de bovenstaande configuratie, kunt u de hele cloudservice van hosten * *http://*&lt;cdnName >*.azureedge.net/**. Als ik ga naar **http://camservice.azureedge.net/ **, ik krijg het resultaat van de actie van start/Index.

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

Dit betekent niet, maar dat dit altijd is een goed idee (of in het algemeen een goed idee) naar een hele cloudservice via Azure CDN fungeren. Enkele van de beperkingen zijn:

-   Hiervoor is uw hele site openbaar zijn, omdat Azure CDN geen privé inhoud op dit moment niet kan dienen.
-   Als het eindpunt CDN voor welke reden dan ook offline gaat, of gepland onderhoud of de gebruiker is opgetreden en uw hele cloudservice offline gaat tenzij de klanten kunnen worden omgeleid naar de URL origin * *http://*&lt;servicenaam >*.cloudapp.net/**.
-   Ook niet met de aangepaste Cachebeheer-instellingen (Zie [configureren cache-opties voor statische bestanden in uw cloudservice](#caching)), een CDN-eindpunt de prestaties van zeer-dynamische inhoud niet worden verbeterd. Als u probeert te laden van de startpagina van uw CDN-eindpunt als wordt weergegeven boven, u ziet dat dit ten minste 5 seconden duurde de eerste keer, dat wil een eenvoudig pagina zeggen van de standaardstartpagina laden. Stel wat gebeurt er met de clientervaring als deze pagina bevat dynamische inhoud die elke minuut moet bijwerken. Dynamische inhoud uit een CDN-eindpunt vereist korte cache verlooptijd, die veelgebruikte Cachemissers bij het eindpunt CDN equivalent. Hiermee pijn doet de prestaties van uw cloudservice en het doel van een CDN defeats.

Het alternatief is om te bepalen welke inhoud moet fungeren van Azure CDN op basis van door geval in uw cloudservice. Daartoe hebt u al hoe u afzonderlijke bestanden met inhoud vanuit het eindpunt CDN gezien. Ik ziet u hoe u de actie van een specifieke controller door het eindpunt CDN dienen in de [inhoud van de acties controller via Azure CDN fungeren](#controller).

<a name="caching"></a>
## <a name="configure-caching-options-for-static-files-in-your-cloud-service"></a>Cache-opties voor statische bestanden in de cloudservice configureren ##

Met de integratie van de Azure CDN in uw-cloudservice, kunt u opgeven hoe het gewenste statische inhoud in de cache opgeslagen in het CDN-eindpunt. Klik hiertoe *Web.config* openen vanuit uw Web rol project (bijvoorbeeld WebRole1) en toevoegen een `<staticContent>` element dat moet worden `<system.webServer>`. De onderstaande XML Hiermee configureert u de cache verstrijkt 3 dagen.  

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

Nadat u dit doet, wordt alle statische bestanden in uw cloudservice dezelfde regel in de cache CDN toekijken. Een *Web.config* -bestand in een andere map toevoegen en uw instellingen er toevoegen voor meer gedetailleerde besturingselement van cache-instellingen. Bijvoorbeeld een *Web.config* -bestand toevoegen aan de map *\Content* en de inhoud vervangen door de volgende XML:

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

Deze instelling zorgt ervoor dat alle statische bestanden uit de map *\Content* mogen worden opgeslagen voor 15 dagen.

Voor meer informatie over het configureren van de `<clientCache>` element, raadpleegt u [Client-Cache &lt;clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache).

In de [inhoud van de acties controller via Azure CDN dienen](#controller)leert ik ook u hoe u de cache-instellingen voor controller actie resultaten in de cache CDN kunt configureren.

<a name="controller"></a>
## <a name="serve-content-from-controller-actions-through-azure-cdn"></a>Inhoud van de acties controller via Azure CDN dienen ##

Wanneer u een rol cloud service Web met Azure CDN integreren, hoeft u relatief eenvoudig inhoud van de acties controller tot en met de CDN Azure fungeren. Dan wel levert uw cloudservice rechtstreeks via Azure CDN (gedemonstreerd hierboven), ziet [Maarten Balliauw](https://twitter.com/maartenballiauw) u hoe u dit doen met een leuke MemeGenerator controller in [degressieve latentie op webpagina's maken met de CDN Azure](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN). Ik zal gewoon Reproduceer deze hier.

Stel dat in uw cloudservice die u wilt genereren memes op basis van een jonge chucks Norris afbeelding (foto door [Rolf Light](http://www.flickr.com/photos/alan-light/218493788/)) als volgt:

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

U hebt een eenvoudige `Index` actie waarmee de klanten kunnen de overtreffende trap opgeven in de afbeelding, klikt u vervolgens de meme wordt gegenereerd zodra ze posten naar de actie. Gezien het feit chucks Norris is, kunt u deze pagina wordt omgezet sterk populaire globaal zou verwachten. Dit is een goed voorbeeld voor gedeeltelijk dynamische inhoud met Azure CDN.

Volg de stappen hierboven om deze actie in controller in te stellen:

1. Klik in de map *\Controllers* maakt een nieuw:. cs-bestand *MemeGeneratorController.cs* genoemd en de inhoud vervangen door de volgende code. Zorg ervoor dat het gemarkeerde gedeelte vervangen door de naam van uw CDN.  

        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Drawing;
        using System.IO;
        using System.Net;
        using System.Web.Hosting;
        using System.Web.Mvc;
        using System.Web.UI;

        namespace WebRole1.Controllers
        {
            public class MemeGeneratorController : Controller
            {
                static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();

                public ActionResult Index()
                {
                    return View();
                }

                [HttpPost, ActionName("Index")]
                public ActionResult Index_Post(string top, string bottom)
                {
                    var identifier = Guid.NewGuid().ToString();
                    if (!Memes.ContainsKey(identifier))
                    {
                        Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                    }

                    return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
                }

                [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
                public ActionResult Show(string id)
                {
                    Tuple<string, string> data = null;
                    if (!Memes.TryGetValue(id, out data))
                    {
                        return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                    }

                    if (Debugger.IsAttached) // Preserve the debug experience
                    {
                        return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                    else // Get content from Azure CDN
                    {
                        return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                    }
                }

                [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
                public ActionResult Generate(string top, string bottom)
                {
                    string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                    Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);

                    using (Graphics graphics = Graphics.FromImage(bitmap))
                    {
                        SizeF size = new SizeF();
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                        }
                        using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                        {
                            graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                        }
                    }

                    MemoryStream ms = new MemoryStream();
                    bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                    return File(ms.ToArray(), "image/png");
                }

                private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
                {
                    // Compute actual size, shrink if needed
                    while (true)
                    {
                        size = g.MeasureString(text, font);

                        // It fits, back out
                        if (size.Height < i.Height &&
                             size.Width < i.Width) { return font; }

                        // Try a smaller font (90% of old size)
                        Font oldFont = font;
                        font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                        oldFont.Dispose();
                    }
                }
            }
        }

2. Met de rechtermuisknop in de standaard `Index()` actie en selecteer **Weergave toevoegen**.

    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)

3.  Accepteer de onderstaande instellingen en klik op **toevoegen**.

    ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)

4. Open de nieuwe *Views\MemeGenerator\Index.cshtml* en de inhoud vervangen door de volgende eenvoudige HTML-code voor het indienen van de overtreffende trap:

        <h2>Meme Generator</h2>

        <form action="" method="post">
            <input type="text" name="top" placeholder="Enter top text here" />
            <br />
            <input type="text" name="bottom" placeholder="Enter bottom text here" />
            <br />
            <input class="btn" type="submit" value="Generate meme" />
        </form>

5. Publiceer de cloudservice opnieuw en Ga naar * *http://*&lt;servicenaam >*.cloudapp.net/MemeGenerator/Index** in uw browser.

Wanneer u de formulierwaarden te verzenden `/MemeGenerator/Index`, de `Index_Post` actie methode geeft als resultaat een koppeling naar de `Show` actie methode met de desbetreffende invoer identificatie. Als u de koppeling klikt, kunt u de volgende code bereiken:  

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }

        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

Als uw lokale foutopsporing is gekoppeld, krijgt u de normale foutopsporing-ervaring met een lokale omleiding. Als deze wordt uitgevoerd in de cloudservice, worden klikt u vervolgens deze omgeleid naar:

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

Die overeenkomt met de volgende URL origin op uw CDN-eindpunt:

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>


U kunt de `OutputCacheAttribute` kenmerk op de `Generate` methode kunt u opgeven hoe het resultaat van de actie moet worden opgeslagen in de cache, die aan de Azure CDN worden geaccepteerd. De onderstaande code Geef een verloopbeleid cache 1 uur (3600 seconden).

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

Daarnaast kan u dienen van inhoud van een actie controller in uw cloudservice via uw CDN Azure, klikt u met de gewenste optie in de cache.

Klik in de volgende sectie wordt ik uitgelegd hoe u moet de gebundelde en minified scripts en CSS via Azure CDN fungeren.

<a name="bundling"></a>
## <a name="integrate-aspnet-bundling-and-minification-with-azure-cdn"></a>ASP.NET bundeling en minification integreren met Azure CDN ##

Scripts en CSS opmaakmodellen niet vaak worden gewijzigd en voornaamste kandidaten voor de cache Azure CDN zijn. Voor de hele Webrol via uw CDN Azure is de eenvoudigste manier om bundeling en minification integreren met Azure CDN. Echter als u niet wilt mogelijk u dit wilt doen, leest u hoe u deze terwijl het bevriezen van de gewenste ontwikkelaars ervaring van ASP.NET bundeling en minification, zoals:

-   Uitstekende foutopsporing modus ervaring
-   Gestroomlijnde implementatie
-   Direct updates voor clients voor script/CSS versie-upgrades
-   Fallback om wanneer uw eindpunt CDN mislukt
-   Code wijziging minimaliseren

Open in het project **WebRole1** die u hebt gemaakt in [een eindpunt Azure CDN met uw Azure website- en serveren totdat statische inhoud in uw webpagina's van Azure CDN integreren](#deploy) *App_Start\BundleConfig.cs* en bekijk de `bundles.Add()` methode oproepen.

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

De eerste `bundles.Add()` instructie voegt u een bundel script op de virtuele map `~/bundles/jquery`. Open vervolgens *Views\Shared\_Layout.cshtml* om te zien hoe de script bundel-code wordt weergegeven. U moet kunnen de volgende regel met code Razor vindt:

    @Scripts.Render("~/bundles/jquery")

Wanneer deze Razor-code wordt uitgevoerd in de rol van Azure Web, worden deze goed weergegeven een `<script>` tag voor de script bundel die vergelijkbaar is met de volgende handelingen uit:

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

Echter wanneer deze wordt uitgevoerd in Visual Studio door te typen `F5`, worden deze elke script-bestand in de bundel afzonderlijk op de goed weergegeven (in het voorbeeld hierboven slechts één script-bestand is in de bundel):

    <script src="/Scripts/jquery-1.10.2.js"></script>

Hiermee kunt u fouten opsporen in de JavaScript-code in uw ontwikkelomgeving terwijl gelijktijdige clientverbindingen (bundeling) verkleinen en het verbeteren van bestand prestaties (minification) in productie downloaden. Dit is een handige functie te bewaren met Azure CDN-integratie. Bovendien omdat de weergegeven bundel al een versietekenreeks automatisch gegenereerde bevat, u wilt dat de functionaliteit repliceren zodat de wanneer u uw versie jQuery tot en met NuGet bijwerkt, deze kan worden bijgewerkt op de client zo snel mogelijk.

Volg de onderstaande stappen voor integratie ASP.NET bundeling en minification met uw CDN-eindpunt.

1. Terug in *App_Start\BundleConfig.cs*, wijzigt u de `bundles.Add()` methoden voor het gebruik van een andere [bundel constructor](http://msdn.microsoft.com/library/jj646464.aspx), dat een CDN-adres. Klik hiertoe vervangen de `RegisterBundles` methodedefinitie met de volgende code:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            bundles.UseCdn = true;
            var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
                .GetName().Version.ToString();
            var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                        "~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                        "~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you're
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                        "~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                        "~/Scripts/bootstrap.js",
                        "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Zorg ervoor dat u voor het vervangen van `<yourCDNName>` met de naam van uw CDN Azure.

    In duidelijke taal die u wilt instellen `bundles.UseCdn = true` en een zorgvuldig ontworpen CDN-URL toegevoegd aan elke bundel. Bijvoorbeeld: de eerste constructor in de code:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))

    is hetzelfde als:

        new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))

    Deze opbouwfunctie leest ASP.NET bundeling en minification weergave van afzonderlijke scriptbestanden wanneer foutopsporing lokaal wordt uitgevoerd, maar het opgegeven CDN-adres gebruiken voor toegang tot het script in kwestie. Bedenk wel twee belangrijke kenmerken met deze zorgvuldig ontworpen CDN-URL:

    -   De oorsprong voor deze CDN-URL is `http://<yourCloudService>.cloudapp.net/bundles/jquery?v=<W.X.Y.Z>`, welke daadwerkelijk de virtuele map van de bundel script in de cloudservice is.
    -   Aangezien u CDN constructor gebruikt, bevat de CDN script-code voor de bundel niet langer de automatisch gegenereerde tekenreeks in de weergegeven URL. Telkens wanneer de bundel script is aangepast als u wilt dat een gemiste cache op uw CDN Azure, moet u handmatig een versietekenreeks met unieke genereren. Tegelijkertijd, moet deze versietekenreeks unieke tot en met de levensduur van de implementatie treffers in cache op uw CDN Azure maximaliseren nadat de bundel wordt geïmplementeerd ongewijzigd blijven.
    -   De queryreeks v = < W.X.Y.Z > worden uit *Properties\AssemblyInfo.cs* in uw project Web-rol. U kunt een werkstroom voor implementatie waarin de versie constructie verhoogd telkens wanneer u naar Azure publiceren hebben. Of, kunt u alleen *Properties\AssemblyInfo.cs* wijzigen in uw project voor het automatisch de versietekenreeks opgehoogd telkens wanneer u samenstelt, met het jokerteken "*". Bijvoorbeeld:

            [assembly: AssemblyVersion("1.0.0.*")]

        Hier werkt op een andere strategie stroomlijnen u genereert een unieke tekenreeks voor de duur van een implementatie.

3. Publiceer de cloudservice opnieuw en toegang tot de startpagina.

4. Bekijk de HTML-code voor de pagina. U moet mogelijk om de CDN-URL weergegeven, met een versietekenreeks unieke telkens wanneer u wijzigingen opnieuw naar uw cloudservice publiceren weer te geven. Bijvoorbeeld:  

        ...

        <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>

        <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>

        ...

        <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>

        <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>

        ...

5. Visual Studio, fouten opsporen in de cloudservice in Visual Studio door te typen `F5`.,

6. Bekijk de HTML-code voor de pagina. Nog steeds ziet u elk afzonderlijk weergegeven zodat u kunt een consistente foutopsporing in Visual Studio-ervaring hebben scriptbestand.  

        ...

            <link href="/Content/bootstrap.css" rel="stylesheet"/>
        <link href="/Content/site.css" rel="stylesheet"/>

            <script src="/Scripts/modernizr-2.6.2.js"></script>

        ...

            <script src="/Scripts/jquery-1.10.2.js"></script>

            <script src="/Scripts/bootstrap.js"></script>
        <script src="/Scripts/respond.js"></script>

        ...   

<a name="fallback"></a>
## <a name="fallback-mechanism-for-cdn-urls"></a>Fallback om CDN-URL 's ##

Wanneer uw Azure CDN-eindpunt om welke reden dan ook is mislukt, wilt u uw webpagina moeten slimme voor toegang tot uw webserver origin als de fallback optie voor het laden van JavaScript- of Bootstrap. Het is ernstige kwijtraken afbeeldingen op uw website vanwege CDN niet beschikbaar, maar veel meer slechte essentieel pagina functionaliteit van uw scripts en opmaakmodellen kwijtraken.

De klasse [bundel](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) bevat een eigenschap met de naam van [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) waarmee u kunt het configureren van de fallback om CDN is mislukt. Als u wilt deze eigenschap gebruikt, de volgende stappen uit te voeren:

1. Klik in uw project Web rol *App_Start\BundleConfig.cs*, waar u de URL van een CDN toegevoegd in elke [bundel constructor](http://msdn.microsoft.com/library/jj646464.aspx), openen en breng de volgende gemarkeerde wijzigingen fallback om toevoegen aan de standaard-pakketten:  

        public static void RegisterBundles(BundleCollection bundles)
        {
            var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
                .GetName().Version.ToString();
            var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
            bundles.UseCdn = true;

            bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))
                        { CdnFallbackExpression = "window.jquery" }
                        .Include("~/Scripts/jquery-{version}.js"));

            bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval"))
                        { CdnFallbackExpression = "$.validator" }
                        .Include("~/Scripts/jquery.validate*"));

            // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
            // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
            bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer"))
                        { CdnFallbackExpression = "window.Modernizr" }
                        .Include("~/Scripts/modernizr-*"));

            bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                        { CdnFallbackExpression = "$.fn.modal" }
                        .Include(
                                "~/Scripts/bootstrap.js",
                                "~/Scripts/respond.js"));

            bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                        "~/Content/bootstrap.css",
                        "~/Content/site.css"));
        }

    Wanneer `CdnFallbackExpression` is niet null, script is toegevoegd in de HTML om te testen of de bundel is geladen en zo niet, de bundel rechtstreeks vanuit de webserver origin openen. Deze eigenschap moet zijn ingesteld op een JavaScript-expressie die wordt gecontroleerd of de desbetreffende CDN bundel goed is geladen. De expressie die nodig is om te testen van elke bundel is afhankelijk van de inhoud. Voor de standaard-pakketten bovenstaande:

    -   `window.jquery`is gedefinieerd in jquery-{versie} js
    -   `$.validator`is gedefinieerd in jquery.validate.js
    -   `window.Modernizr`is gedefinieerd in modernizer-{versie} js
    -   `$.fn.modal`is gedefinieerd in bootstrap.js

    U mogelijk opgevallen dat ik niet ingesteld CdnFallbackExpression voor de `~/Cointent/css` bundel. Dit is omdat er is momenteel een [fout in System.Web.Optimization](https://aspnetoptimization.codeplex.com/workitem/104) die invoegt een `<script>` tag voor de fallback CSS in plaats van de verwachte `<link>` tag.

    Er is echter een goede [Stijl bundel gebruik](https://github.com/EmberConsultingGroup/StyleBundleFallback) door [Ember consultingservices groep](https://github.com/EmberConsultingGroup)worden aangeboden.

2. De tijdelijke oplossing voor CSS gebruiken, een nieuw:. cs-bestand maken in uw Web rol-project *App_Start* map *StyleBundleExtensions.cs*en de inhoud te vervangen door de [code uit GitHub](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs).

4. Geef in *App_Start\StyleFundleExtensions.cs*, de naamruimte van de rol van uw Web-naam (bijvoorbeeld **WebRole1**).

3. Ga terug naar `App_Start\BundleConfig.cs` en wijzigt u de laatste `bundles.Add` instructie met de volgende gemarkeerde code:  

        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));

    Deze nieuwe extensie methode gebruikt de dezelfde idee om te vanaf een script in de HTML-code om te controleren van de DOM voor de een overeenkomende klassenaam, de regelnaam van de en regel waarde die is gedefinieerd in het CSS-bundel en valt terug naar de webserver origin als het niet om de overeenkomst te vinden.

4. De cloudservice opnieuw publiceren en gebruiken van de startpagina.

5. Bekijk de HTML-code voor de pagina. U ziet ingevoegd scripts ongeveer als volgt uit:    

        ...

        <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
        <script>(function() {
                        var loadFallback,
                            len = document.styleSheets.length;
                        for (var i = 0; i < len; i++) {
                            var sheet = document.styleSheets[i];
                            if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                                var meta = document.createElement('meta');
                                meta.className = 'sr-only';
                                document.head.appendChild(meta);
                                var value = window.getComputedStyle(meta).getPropertyValue('width');
                                document.head.removeChild(meta);
                                if (value !== '1px') {
                                    document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                                }
                            }
                        }
                        return true;
                    }())||document.write('<script src="/Content/css"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
        <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>

        ...

            <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
        <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>

            <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
        <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>

        ...


    Houd er rekening mee dat ingevoegd script voor de CSS-bundel nog steeds de onjuiste overblijfsel van bevat de `CdnFallbackExpression` eigenschap in de regel:

        }())||document.write('<script src="/Content/css"><\/script>');</script>

    Maar sinds het eerste deel van de || expressie retourneert altijd true (in de regel die direct boven), kunnen de functie document.write() nooit wordt uitgevoerd.

## <a name="more-information"></a>Meer informatie ##
- [Overzicht van het netwerk Azure Contentlevering (CDN)](http://msdn.microsoft.com/library/azure/ff919703.aspx)
- [Azure CDN gebruiken](cdn-create-new-endpoint.md)
- [ASP.NET bundeling en Minification](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)



[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png
