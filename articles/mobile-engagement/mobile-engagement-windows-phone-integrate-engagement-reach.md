<properties 
    pageTitle="Windows Phone Silverlight bereik SDK-integratie" 
    description="Azure mobiele betrokkenheid bereik integreren met Windows Phone Silverlight-Apps"                    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article"
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-reach-sdk-integration"></a>Windows Phone Silverlight bereik SDK-integratie

U moet de integratie procedure wordt beschreven in de [Windows Phone Silverlight betrokkenheid SDK integratie](mobile-engagement-windows-phone-integrate-engagement.md) voordat u deze handleiding volgen.

##<a name="embed-the-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a>De betrokkenheid hebt bereikt SDK insluiten in uw Windows Phone Silverlight-project

U hoeft niet alles om toe te voegen. `EngagementReach`verwijzingen en bronnen zijn al beschikbaar in uw project.

> [AZURE.TIP]  U kunt afbeeldingen uit de `Resources` map van uw project, met name het huisstijl-pictogram (die standaard naar het pictogram betrokkenheid).

##<a name="add-the-capabilities"></a>De mogelijkheden toevoegen

De betrokkenheid hebt bereikt SDK moet enkele extra mogelijkheden.

Open uw `WMAppManifest.xml` archiveren en zorg dat de volgende mogelijkheden worden aangegeven:

-   `ID_CAP_PUSH_NOTIFICATION`
-   `ID_CAP_WEBBROWSERCOMPONENT`

De eerste fase wordt gebruikt door de service MPNS toe te staan dat de weergave van mailpop-upmelding. In het tweede voorbeeld wordt gebruikt voor het insluiten van een browser-taak in de SDK.

Bewerken de `WMAppManifest.xml` archiveren en toevoegen in de `<Capabilities />` tag:

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

##<a name="enable-the-microsoft-push-notification-service"></a>De Microsoft-Push Notification-Service inschakelen

De **Microsoft Push Notification-Service** (MPNS genoemd) wilt gebruiken uw `WMAppManifest.xml` bestand moet hebben een `<App />` markeren met een `Publisher` kenmerk ingesteld op de naam van uw project.

##<a name="initialize-the-engagement-reach-sdk"></a>Het bereik betrokkenheid SDK geïnitialiseerd

### <a name="engagement-configuration"></a>Betrokkenheid configuratie

De configuratie betrokkenheid zijn ondergebracht in de `Resources\EngagementConfiguration.xml` bestand van uw project.

Bewerken dit bestand als u wilt bereiken configuratie opgeven:

-   *Optioneel*, geef aan of de systeemeigen push (MPNS) is geactiveerd of niet tussen `<enableNativePush>` en `</enableNativePush>` tags (`true` al dan niet standaard).
-   *Optioneel*, geven de naam van het kanaal push tussen `<channelName>` en `</channelName>` tags bieden dezelfde dat uw toepassing momenteel mogelijk gebruiken of deze leeg laat.

Als u deze in plaats daarvan gedurende runtime opgeeft wilt, kunt u de volgende methode voordat de initialisatie van de agent betrokkenheid bellen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether the native push (MPNS) is activated or not. */
    
    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide the same channel name that your application may currently use. */
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [AZURE.TIP] De naam van het kanaal MPNS push van uw toepassing, kunt u opgeven. Betrokkenheid maakt standaard een naam op basis van de toepassings-id. U hebt hoeft de naam te geven uzelf, behalve als u wilt gebruiken het kanaal push buiten betrokkenheid.

### <a name="engagement-initialization"></a>Initialisatie van de betrokkenheid

Wijzig de `App.xaml.cs`:

-   Toevoegen aan uw `using` instructies:

        using Microsoft.Azure.Engagement;

-   Invoegen `EngagementReach.Instance.Init` vlak na `EngagementAgent.Instance.Init` in `Application_Launching` :

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }

-   Invoegen `EngagementReach.Instance.OnActivated` in de `Application_Activated` methode:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

> [AZURE.IMPORTANT] De `EngagementReach.Instance.Init` wordt uitgevoerd in een speciale thread. U beschikt niet over het zelf te doen.

##<a name="app-store-submission-considerations"></a>Aandachtspunten voor de App store indienen

Microsoft gelden enkele regels bij gebruik van de push-meldingen:

Sectie 2,9 uit de documentatie Microsoft [Beleidsregels van toepassing] :

1) U moet de gebruiker te accepteren om deel te push-meldingen ontvangen vragen. Klik in uw instellingen voor een manier om uit te schakelen van de push-meldingen toevoegen.

Het object EngagementReach biedt twee methoden voor het beheer van de opt-in/opt-out, `EnableNativePush()` en `DisableNativePush()`. U kon, bijvoorbeeld een optie maken in de instellingen met een wisselknop MPNS in-of uitschakelen.

U kunt ook voor het deactiveren van MPNS tot en met de configuratie betrokkenheid\<windows-telefoon-sdk-bereik-configuratie\>.

> 2.9.1) de toepassing moet eerst Beschrijf de meldingen te verstrekken en **toestemming van de gebruiker express (opt-in)**en **moet ondersteuning bieden voor een om waarmee de gebruiker afmelden kan bij de push-meldingen ontvangen**. Alle meldingen verschaft met de Microsoft Push Notification-Service moet overeenkomen met de beschrijving aan de gebruiker en moet voldoen alle toepasselijke [Beleidsregels voor toepassingen]  [ Content Policies] en [Aanvullende vereisten voor specifieke toepassing grafiektypen].

2) Gebruik niet te veel push-meldingen. Betrokkenheid verwerkt meldingen voor u.

> 2.9.2) de toepassing en het gebruik van de Push Notification-Service van Microsoft niet te gebruiken netwerkcapaciteit of de bandbreedte van het Microsoft Push Notification-Service of anders ten onrechte belasten een Windows Phone of andere Microsoft-apparaat of service met overtollige push-meldingen, zoals bepaald door Microsoft in de redelijk keuze, en moet beschadigen of geen goed met een Microsoft-netwerken of servers of een servers van derden of de netwerken verbonden met de Push Notification-Service van Microsoft.

3) Niet te vertrouwen op MPNS criticals informatie te verzenden. Betrokkenheid gebruikt MPNS, zodat deze regel ook voor de campagnes gemaakt binnen de betrokkenheid front geldt.

> 2.9.3) het Microsoft Push Notification-Service mogelijk niet gebruikt voor het verzenden van berichten die opdracht kritieke of anderszins zijn kunnen invloed hebben op zaken van leven of dood, inclusief zonder beperking kritieke meldingen gerelateerd aan een medisch hulpmiddel of voorwaarde. MICROSOFT GEEFT UITDRUKKELIJK GEEN ENKELE DAT HET GEBRUIK VAN DE MICROSOFT-PUSH NOTIFICATION-SERVICE OF DE BEZORGING VAN MICROSOFT PUSH NOTIFICATION-SERVICEMELDINGEN WORDEN ONONDERBROKEN, ZONDER FOUTEN, OF ANDERSZINS GEGARANDEERD VOORDOEN OP BASIS VAN REALTIME.

**We garanderen niet dat uw toepassing wordt geaccepteerd door het validatieproces als u bieden geen ondersteuning voor deze aanbevelingen.**

##<a name="handle-data-push-optional"></a>Greep gegevens push (optioneel)

Als u wilt dat uw toepassing moeten kunnen bereiken gegevens ladingen ontvangen, moet u aan het implementeren van twee gebeurtenissen van de klasse EngagementReach:

    EngagementReach.Instance.DataPushStringReceived += (body) =>
    {
       Debug.WriteLine("String data push message received: " + body);
       return true;
    };
    
    EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
    {
       Debug.WriteLine("Base64 data push message received: " + encodedBody);
       // Do something useful with decodedBody like updating an image view
       return true;
    };

U kunt zien dat de terugbellen van elke methode geeft als een Booleaanse waarde resultaat. Betrokkenheid stuurt een feedback naar de back-enddatabase nadat de push gegevens. Als de terugbellen ONWAAR is, retourneert de `exit` feedback worden verzenden. Anders worden `action`. Als er geen terugbellen is ingesteld voor de gebeurtenissen de `drop` feedback keert terug naar betrokkenheid.

> [AZURE.WARNING] Betrokkenheid kan niet veelvouden feedback voor een push gegevens te ontvangen. Als u wilt verschillende handlers instellen op een gebeurtenis, houd er rekening mee dat het soort feedback naar de laatste overeen wordt een verzonden. In dit geval wordt aangeraden retourneert altijd hetzelfde resultaat om te voorkomen duidelijker is feedback op de front.

##<a name="customize-ui-optional"></a>Aanpassen van UI (optioneel)

### <a name="first-step"></a>Eerste stap

We kunt u het bereik UI aanpassen.

Als u wilt doen, u moet maken van een subcategorie van de `EngagementReachHandler` class.

**Voorbeeld van een Code:**

    using Microsoft.Azure.Engagement;
    
    namespace Example
    {
       internal class ExampleReachHandler : EngagementReachHandler
       {
          // Override EngagementReachHandler methods depending on your needs
       }
    }

Vervolgens stelt u de inhoud van de `EngagementReach.Instance.Handler` veld met uw aangepaste object in uw `App.xaml.cs` klasse binnen de `Application_Launching` methode.

**Voorbeeld van een Code:**

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [AZURE.NOTE] Betrokkenheid standaard in een eigen implementatie van `EngagementReachHandler`. U hoeft te maken van uw eigen en als u dit doet, moet u niet elke methode overschrijven. Het standaardgedrag is om de betrokkenheid grondtal object te selecteren.

### <a name="layouts"></a>Indelingen

Standaard wordt bereik gebruik van de ingesloten bronnen van de DLL voor meldingen en pagina's weergeven.

U kunt echter besluiten gebruik van uw eigen resources aan de uw huisstijl in deze onderdelen.

U kunt negeren `EngagementReachHandler` methoden in uw subcategorie te geven betrokkenheid gebruik van de indelingen:

**Voorbeeld van een Code:**

    // In your subclass of EngagementReachHandler
    
    public override string GetTextViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetWebViewAnnouncementUri()
    {
       // return the path of your own xaml
    }
    
    public override string GetPollUri()
    {
       // return the path of your own xaml
    }
    
    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [AZURE.TIP] De `CreateNotification` methode kan null retourneren. De melding niet weergegeven en de campagne bereik wordt verwijderd.

Om te vereenvoudigen uw implementatie indeling, bieden we ook onze eigen xaml die als basis voor uw code dienen kan. Ze zich bevinden in het archief betrokkenheid SDK (/ src/bereik /).

> [AZURE.WARNING] De bronnen die wij bieden zijn exacte dezelfde als die we gebruiken. Dus als u ze rechtstreeks wijzigen wilt, vergeet niet om de naamruimte en de naam te wijzigen.

### <a name="notification-position"></a>Melding positie

Een melding voor de app wordt standaard weergegeven linksonder kant van de toepassing. U kunt dit gedrag kunt wijzigen door te overschrijven de `GetNotificationPosition` methode van de `EngagementReachHandler` object.

    // In your subclass of EngagementReachHandler
    
    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of the EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

Op dit moment kunt u kiezen tussen de `BOTTOM` (standaard) en `TOP` posities.

### <a name="launch-message"></a>Bericht starten

Wanneer een gebruiker klikt op de Systeemmelding van een (een mailpop), betrokkenheid Hiermee start u de app, de inhoud van de push-berichten laden en weergeven van de pagina voor de bijbehorende campagne.

Er treedt een vertraging op tussen de lancering van de toepassing en de weergave van de pagina (afhankelijk van de snelheid van uw netwerk).

Om aan te geven aan de gebruiker iets wordt geladen, moet u een visuele gegevens, zoals een voortgangsbalk of een voortgangsindicator opleveren. Betrokkenheid verwerken niet zelf, maar een paar handlers voor u is.

Als u wilt de terugbellen implementeren, volgt te werk:

    /* The application has launched and the content is loading.
     * You should display an indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };
    
    /* The application has finished loading the content and the page
     * is about to be displayed.
     * You should hide the indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };
    
    /* The content has been loaded, but an error has occurred.
     * You can provide an information to the user.
     * You should hide the indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

U kunt de terugbellen instellen in uw `Application_Launching` methode van uw `App.xaml.cs` bestand, bij voorkeur voordat de `EngagementReach.Instance.Init()` bellen.

> [AZURE.TIP] Elke handler wordt aangeroepen door de UI-Thread. Er geen zorgen te maken wanneer u een berichtvenster of iets UI-gerelateerde gebruikt.

[Beleidsregels voor toepassingen]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[Aanvullende vereisten voor specifieke toepassingstypen]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx
 
