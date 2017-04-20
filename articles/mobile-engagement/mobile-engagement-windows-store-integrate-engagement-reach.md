<properties 
    pageTitle="Universele Apps voor Windows SDK integratie hebt bereikt" 
    description="Azure mobiele betrokkenheid bereik integreren met universele Apps voor Windows"
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="windows-universal-apps-reach-sdk-integration"></a>Universele Apps voor Windows SDK integratie hebt bereikt

U moet de integratie procedure wordt beschreven in de [Integratie van Windows universele betrokkenheid SDK](mobile-engagement-windows-store-integrate-engagement.md) voordat u deze handleiding volgen.

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a>De betrokkenheid hebt bereikt SDK insluiten in uw Windows universele project

U hoeft niet alles om toe te voegen. `EngagementReach`verwijzingen en bronnen zijn al beschikbaar in uw project.

> [AZURE.TIP] U kunt afbeeldingen uit de `Resources` map van uw project, met name het huisstijl-pictogram (die standaard naar het pictogram betrokkenheid). Klik op universele Apps kunt u ook verplaatsen de `Resources` map op uw gedeelde project voor het delen van de inhoud tussen apps, maar u moet houden de `Resources\EngagementConfiguration.xml` bestand op de standaardlocatie, omdat deze afhankelijke platform.

## <a name="enable-the-windows-notification-service"></a>De Windows-Notification-Service inschakelen

### <a name="windows-8x-and-windows-phone-81-only"></a>Windows 8.x en Windows Phone 8.1 alleen

Pas de **Windows Notification-Service** (WNS genoemd) gebruiken uw `Package.appxmanifest` bestand op `Application UI` klikt u op `All Image Assets` in het vak links bots. Aan de rechterkant van het vak in `Notifications`, wijzigen `toast capable` uit `(not set)` naar `(Yes)`.

### <a name="all-platforms"></a>Alle platforms

U moet uw app bij uw Microsoft-account en naar het platform betrokkenheid synchroniseren. Hiervoor moet u een account maken of meld u aan de [windows-ontwikkelaar center](https://dev.windows.com). Nadat u hebt die een nieuwe toepassing maken en de beveiligings-id en geheime sleutel vinden. Ga op de frontend betrokkenheid van uw app-instelling in `native push` en uw referenties plakken. Daarna, klik met de rechtermuisknop op uw project, selecteer `store` en `Associate App with the Store...`. U hoeft de toepassing die u hebt maken voordat te synchroniseren te selecteren.

## <a name="initialize-the-engagement-reach-sdk"></a>Het bereik betrokkenheid SDK geïnitialiseerd

Wijzig de `App.xaml.cs`:

-   Invoegen `EngagementReach.Instance.Init` vlak na `EngagementAgent.Instance.Init` in uw `InitEngagement` methode:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
          EngagementReach.Instance.Init(e);
        }

    De `EngagementReach.Instance.Init` wordt uitgevoerd in een speciale thread. U beschikt niet over het zelf te doen.

> [AZURE.NOTE] Als u pushmeldingen ergens anders in uw toepassing hebt u [uw kanaal push delen](#push-channel-sharing) met het betrokkenheid hebt bereikt.

## <a name="integration"></a>Integratie

Betrokkenheid zijn er twee manieren om toe te voegen van de app-spandoek bereik en verspreide weergaven voor aankondigingen en polls in uw toepassing: de overlay-integratie en de handmatige integratie van web-weergaven. U moet niet beide aanpak op dezelfde pagina combineren.

De keuze tussen de twee integratie kan zo worden samengevat:

-   U kunt de overlay-integratie als uw pagina's al overneemt van de Agent `EngagementPage`, is het alleen een kwestie van het vervangen van `EngagementPage` door `EngagementPageOverlay` en `xmlns:engagement="using:Microsoft.Azure.Engagement"` door `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` op uw pagina's.
-   U kunt de webweergaven handmatige integratie kiezen als u wilt plaatsen exact de gebruikersinterface hebt bereikt binnen de pagina's of als u niet wilt dat een ander overnameniveau toevoegen aan uw pagina's. 

### <a name="overlay-integration"></a>Overlay-integratie

De betrokkenheid overlay wordt dynamisch toegevoegd voor de elementen van de gebruikersinterface die wordt gebruikt voor weergave van bereik campagnes op uw pagina. Als de overlay niet op uw indeling aansluiten overwegen u de webweergaven handmatige-integratie in plaats daarvan.

In uw .xaml bestandswijziging `EngagementPage` verwijzing naar`EngagementPageOverlay`

-   Toevoegen aan de declaraties naamruimten:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

-   Vervang `engagement:EngagementPage` met `engagement:EngagementPageOverlay`:

**Met EngagementPage:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
        
            <!-- Your layout -->
        </engagement:EngagementPage>

**Met EngagementPageOverlay:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">
        
            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

Klik in het bestand:. cs een melding geven voor uw pagina in `EngagementPageOverlay` in plaats van `EngagementPage` en importeer `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

-   Vervang `EngagementPage` met `EngagementPageOverlay`:

**Met EngagementPage:**

            using Microsoft.Azure.Engagement;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**Met EngagementPageOverlay:**

            using Microsoft.Azure.Engagement.Overlay;
            
            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


De overlay betrokkenheid voegt een `Grid` element boven aan de pagina op basis van de indeling en de twee `WebView` elementen één voor de banner en de andere één voor de verspreide weergave.

U kunt de overlay-elementen rechtstreeks in de `EngagementPageOverlay.cs` bestand.

### <a name="web-views-manual-integration"></a>Handmatige integratie met het web weergaven

Bereik doorzoeken uw pagina's voor de twee `WebView` elementen die verantwoordelijk is voor het weergeven van de banner en de verspreide weergave. Het enige wat u moet doen is toe te voegen van deze twee `WebView` elementen ergens in uw pagina's, Hier volgt een voorbeeld:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


In dit voorbeeld de `WebView` elementen aanpassen aan de container die automatisch opnieuw groter scherm draaiing of het venster grootte wijzigen voor het geval wordt uitgerekt.

> [AZURE.WARNING] Het is belangrijk dat het hetzelfde naming `engagement_notification_content` en `engagement_announcement_content` voor de `WebView` elementen. Bereik is identificeren door hun naam. 

## <a name="handle-datapush-optional"></a>Greep datapush (optioneel)

Als u wilt dat uw toepassing moeten kunnen bereiken gegevens ladingen ontvangen, moet u aan het implementeren van twee gebeurtenissen van de klasse EngagementReach:

In App.xaml.cs in de constructor App() toevoegen:

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

## <a name="customize-ui-optional"></a>Aanpassen van UI (optioneel)

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

Vervolgens stelt u de inhoud van de `EngagementReach.Instance.Handler` veld met uw aangepaste object in uw `App.xaml.cs` klasse binnen de `App()` methode.

**Voorbeeld van een Code:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [AZURE.NOTE]Betrokkenheid standaard in een eigen implementatie van `EngagementReachHandler`.
> U hoeft te maken van uw eigen en als u dit doet, moet u niet elke methode overschrijven. Het standaardgedrag is om de betrokkenheid basistoewijzing object te selecteren.

### <a name="web-view"></a>Webweergave

Standaard wordt bereik gebruik van de ingesloten bronnen van de DLL voor meldingen en pagina's weergeven.

Mogelijkheden voor een volledige aanpassing bieden we alleen webweergave gebruiken. Als u aanpassen van indelingen wilt, overschreven door rechtstreeks de bestanden resources `EngagementAnnouncement.html` en `EngagementNotification.html`. Betrokkenheid moet alle code in `<body></body>` goed te laten werken. Maar u kunt markering outer toevoegen `engagement_webview_area`.

U kunt echter besluiten uw eigen resources te gebruiken.

U kunt negeren `EngagementReachHandler` methoden in uw subcategorie te geven betrokkenheid op basis van uw indelingen, maar moet u ervoor ingesloten de betrokkenheid om:

**Voorbeeld van een Code:**
            
            // In your subclass of EngagementReachHandler
            
            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


Is standaard AnnouncementHTML `ms-appx-web:///Resources/EngagementAnnouncement.html`. Staat dit voor het ontwerpen van de inhoud van een bericht push (tekst aankondiging, Web anoucement en peiling aankondiging) HTML-bestand. AnnouncementName is `engagement_announcement_content`. Dit is de naam van het ontwerp webweergave in uw xaml-pagina.

NotfificationHTML is `ms-appx-web:///Resources/EngagementNotification.html`. Staat dit voor het ontwerpen van de melding van een bericht push HTML-bestand. NotfificationName is `engagement_notification_content`. Dit is de naam van het ontwerp webweergave in uw xaml-pagina.

### <a name="customization"></a>Aanpassing

U kunt aanpassen voor meldingen en aankondiging webweergave heeft u moet als u betrokkenheid object behouden. Zorg dat webweergave uitvoeren object driemaal - de eerste keer in uw xaml, een tweede keer in uw bestand:. cs in de 'setwebview()'-methode en derde tijd in de html-bestand wordt beschreven.

-   In uw xaml wordt het huidige grafische indeling webweergave onderdeel beschreven.
-   U kunt in uw bestand:. cs "setwebview()" waarin u de dimensie van de twee Webweergave (melding, aankondiging) instellen definiëren. Dit is zeer effectieve wanneer de grootte van de toepassing is wijzigen.
-   In de html-bestand betrokkenheid beschrijven we de inhoud webweergave, ontwerp en de elementen posities tussen elkaar.

### <a name="launch-message"></a>Bericht starten

Wanneer een gebruiker klikt op de Systeemmelding van een (een mailpop), betrokkenheid Start de toepassing, de inhoud van de push-berichten laden en weergeven van de pagina voor de bijbehorende campagne.

Er treedt een vertraging op tussen de lancering van de toepassing en de weergave van de pagina (afhankelijk van de snelheid van uw netwerk).

Om aan te geven aan de gebruiker iets wordt geladen, moet u een visuele gegevens, zoals een voortgangsbalk of een voortgangsindicator opleveren. Betrokkenheid verwerken niet zelf, maar een paar handlers voor u is.

Implementatie van de terugbellen in App.xaml.cs in "Openbare App() {}" toevoegen:

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

U kunt de terugbellen instellen in uw 'Openbare App() {}'-methode van uw `App.xaml.cs` bestand, bij voorkeur voordat de `EngagementReach.Instance.Init()` bellen.

> [AZURE.TIP] Elke handler wordt aangeroepen door de UI-Thread. Er geen zorgen te maken wanneer u een berichtvenster of iets UI-gerelateerde gebruikt.

##<a id="push-channel-sharing"></a>Push-kanaal delen

Als u in uw toepassing push-meldingen voor andere doeleinden gebruikt hebt u het delen van de functie van de betrokkenheid SDK push-kanaal gebruiken. Dit is om te voorkomen gemiste push.

- U kunt uw eigen push-kanaal om de betrokkenheid hebt bereikt initialisatie te bieden. De SDK wordt gebruikt in plaats van het aanvragen van een nieuwe record.

Initialisatie van de betrokkenheid hebt bereikt bijwerken met uw kanaal push in de `InitEngagement` methode uit de `App.xaml.cs` bestand:
    
    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    
    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

- U kunt ook als u alleen gebruiken het kanaal push wilt nadat de initialisatie bereik en u een terugbellen op betrokkenheid instellen kunt om het kanaal push eenmaal hebt bereikt wordt deze gemaakt door de SDK.

Uw terugbellen instellen op een plaats **nadat** de initialisatie van de bereiken:

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Aangepaste kleurenschema tip

Wij bieden aangepast kleurenschema gebruiken. U kunt verschillende soorten URI vanuit betrokkenheid frontend moet worden gebruikt in uw toepassing betrokkenheid verzenden. Standaard kleurenschema zoals `http, ftp, ...` zijn beheren met Windows, een venster wordt prompt als ze geen standaardtoepassing op het apparaat is geïnstalleerd. U kunt ook uw eigen aangepaste kleurenschema maken voor uw toepassing.

Is de eenvoudige manier voor het instellen van een aangepast kleurenschema in uw toepassing open uw `Package.appxmanifest` Ga in `Declarations` Configuratiescherm. Selecteer `Protocol` schuiven in de beschikbare declaraties vak en voeg deze toe. Bewerken de `Name` veld met uw nieuwe protocol de gewenste naam.

Bewerk nu als u wilt gebruiken dit protocol, uw `App.xaml.cs` met de `OnActivated` methode en vergeet niet ook hier betrokkenheid geïnitialiseerd:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);
            
              //TODO design action to do when app is launch
            
              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;
            
                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion
 
