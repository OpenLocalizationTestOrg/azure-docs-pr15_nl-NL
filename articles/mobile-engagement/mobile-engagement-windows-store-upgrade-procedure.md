<properties 
    pageTitle="Universele Apps voor Windows SDK upgraden Procedures" 
    description="Universele Apps voor Windows SDK bijwerken Procedures voor Azure mobiele betrokkenheid"                     
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

#<a name="windows-universal-apps-sdk-upgrade-procedures"></a>Universele Apps voor Windows SDK upgraden Procedures

Als u hebt al een oudere versie van betrokkenheid geïntegreerd in uw toepassing, hebt u de volgende punten overwegingen bij het bijwerken van de SDK.

U moet mogelijk verschillende procedures volgen als u verschillende versies van de SDK hebt gemist. Als u migreert van 0.10.1 naar 0.11.0 moet u eerst de procedure 'van 0.9.0 naar 0.10.1"Volg vervolgens de procedure 'van 0.10.1 naar 0.11.0' bijvoorbeeld.

##<a name="from-330-to-340"></a>Uit 3.3.0 naar 3.4.0

### <a name="test-logs"></a>Test-Logboeken

Console-logboeken geproduceerd door de SDK kunnen nu zijn ingeschakeld/uitgeschakeld/gefilterd. U kunt u dit aanpassen door de eigenschap bijwerken `EngagementAgent.Instance.TestLogEnabled` naar een van de waarde die verkrijgbaar is bij het `EngagementTestLogLevel` opsomming, bijvoorbeeld:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a>Resources

De overlay bereik is verbeterd. Deze maakt deel uit van de bronnen voor het pakket van SDK NuGet.

Terwijl u een upgrade uitvoeren naar de nieuwe versie van de SDK, kunt u kiezen of u wilt behouden van uw bestaande bestanden uit de map overlay uw resources al dan niet:

* Als de vorige overlay voor u werkt of als u integreert de `WebView` elementen handmatig vervolgens u beslissen kunt om uw bestaande bestanden te houden, het nog steeds werkt. 
* Als u bijwerken naar de nieuwe overlay wilt, alleen het geheel vervangen `overlay` map van uw resources met de nieuwe sectie die u uit het pakket SDK (UWP apps: na de upgrade, kunt u de nieuwe overlay-map krijgen van % USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [AZURE.WARNING] Gebruik van de nieuwe overlay overschrijft aanpassingen op de vorige versie.

##<a name="from-320-to-330"></a>Uit 3.2.0 naar 3.3.0

### <a name="resources"></a>Resources
Deze stap betreft alleen aangepaste resources. Als u de resources die is verstrekt door de SDK (html, afbeeldingen, overlay) hebt aangepast hebt u deze voordat u gaat upgraden back-up en opnieuw toepassen uw aanpassingen op bijgewerkte resources.

##<a name="from-310-to-320"></a>Uit 3.1.0 naar 3.2.0

### <a name="resources"></a>Resources
Deze stap betreft alleen aangepaste resources. Als u de resources die is verstrekt door de SDK (html, afbeeldingen, overlay) hebt aangepast hebt u deze voordat u gaat upgraden back-up en opnieuw toepassen uw aanpassingen op bijgewerkte resources.

### <a name="webview-integration"></a>Webweergave-integratie
Er zijn enkele verbeteringen aan een ander apparaat formulier factoren geïntroduceerd in deze versie. Zorg ervoor dat de integratie van de webweergave overeenkomen met het volgende:

In uw XAML pagina ():

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

En de bijbehorende:. cs-bestand:

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated to within a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
            
              #region to implement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update the webview when the app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update the webview when the app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure to detach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }
              
              /* "width" and "height" are the current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif
            
              /// <summary>
              /// Set your webview elements to the correct size.
              /// </summary>
              /// <param name="width">The width of your current display.</param>
              /// <param name="height">The height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }
            
              /// <summary>
              /// Handler that takes the Windows.Current.SizeChanged and indicates that webviews have to be resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes the ApplicationView.VisibleBoundsChanged and indicates that webviews have to be resized
              /// </summary>
              /// <param name="sender">The related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;
            
                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

##<a name="from-200-to-300"></a>Uit 2.0.0 naar 3.0.0

### <a name="resources"></a>Resources
Deze stap betreft alleen aangepaste resources. Als u de resources die is verstrekt door de SDK (html, afbeeldingen, overlay) hebt aangepast hebt u deze voordat u gaat upgraden back-up en opnieuw toepassen uw aanpassingen op bijgewerkte resources.

##<a name="from-111-to-200"></a>Uit 1.1.1 naar 2.0.0

Het volgende beschreven hoe u het migreren van een SDK-integratie van de service van Capptain SAS Capptain in een app mogelijk gemaakt door Azure Mobile betrokkenheid. 

> [Azure.IMPORTANT] Capptain en Mobile betrokkenheid zijn niet dezelfde services en het migreren van de client-app in de onderstaande procedure alleen worden gemarkeerd. Migreren van de SDK in de app migreert geen gegevens van de Capptain-servers met de servers Mobile betrokkenheid

Als u vanuit een eerdere versie migreert, raadpleegt u de website Capptain om te migreren naar 1.1.1 eerst de volgende procedure toepassen

### <a name="nuget-package"></a>Nuget pakket

Vervang **Capptain.WindowsPhone** door **MicrosoftAzure.MobileEngagement** Nuget-pakket.

### <a name="applying-mobile-engagement"></a>Mobiele betrokkenheid toepassen

De SDK wordt de term `Engagement`. U moet bijwerken van uw project zodat deze overeenkomen met deze wijziging.

U moet verwijderen van uw huidige Capptain nuget-pakket. Houd rekening met dat alle wijzigingen in de map Capptain bronnen worden verwijderd. Als u wilt behouden Breng deze bestanden vervolgens een kopie van deze.

Installeer daarna het nieuwe Microsoft Azure Engagement nuget pakket aan uw project. U vindt deze rechtstreeks op [nuget website]. of hier index. Deze actie vervangt alle resources-bestanden die worden gebruikt door betrokkenheid en wordt de nieuwe betrokkenheid DLL toegevoegd aan uw projectverwijzingen.

U moet uw projectverwijzingen wissen door te Capptain DLL verwijzingen verwijderen. Als u dit niet doet, wordt de versie van Capptain conflicteert en fouten er gebeurt.

Als u Capptain resources hebt aangepast, wordt de inhoud van uw oude bestanden kopiëren en plak deze in de nieuwe betrokkenheid-bestanden. Houd er rekening mee dat zowel xaml en cs bestanden moeten worden bijgewerkt.

Wanneer u klaar bent met deze stappen hebt u alleen voor het vervangen van oude Capptain verwijzingen door de nieuwe betrokkenheid verwijzingen.

1. Alle Capptain naamruimten moeten worden bijgewerkt.

    Voor de migratie:
    
        using Capptain.Agent;
        using Capptain.Reach;
    
    Na de migratie:
    
        using Microsoft.Azure.Engagement;

2. Alle Capptain klassen met 'Capptain' moeten 'Betrokkenheid' bevatten.

    Voor de migratie:
    
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
    
    Na de migratie:
    
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }

3. Voor xaml-bestanden wijzigen Capptain naamruimte en kenmerken ook.

    Voor de migratie:
    
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
    
    Na de migratie:
    
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>

4. Wijzigingen van de pagina overlay
    > [AZURE.IMPORTANT] Overlay ook wijzigingen. De nieuwe naamruimte is `Microsoft.Azure.Engagement.Overlay`. Er moet worden gebruikt in zowel cs als xaml-bestanden. Bovendien `CapptainGrid` is naam `EngagementGrid`, `capptain_notification_content` en `capptain_announcement_content` heten `engagement_notification_content` en `engagement_announcement_content`.
    
    Voor overlay:
    
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
    
    Deze verandert in:
    
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>

5. Voor de andere bronnen zoals illustraties van Capptain en HTML-bestanden, houd er rekening mee dat ze ook hebt gekregen om te gebruiken "Betrokkenheid".

### <a name="project-declaration"></a>Declaratie van project

Klik op Package.appxmanifest `File Type Associations` heeft bijgewerkt op basis van:

 -   capptain\_hebt bereikt\_inhoud te betrokkenheid\_hebt bereikt\_inhoud
 -   capptain\_log\_bestand naar betrokkenheid\_log\_bestand

### <a name="application-id--sdk-key"></a>Toepassings-ID / SDK-toets

Betrokkenheid gebruikt een verbindingsreeks. U hoeft te geven van een toepassing-ID en een sleutel SDK met mobiele betrokkenheid, hoeft u alleen te geven van een verbindingsreeks. U kunt instellen deze aan uw bestand EngagementConfiguration.

De configuratie betrokkenheid kan worden ingesteld in uw `Resources\EngagementConfiguration.xml` bestand van uw project.

Bewerken dit bestand om op te geven:

-   De verbindingsreeks van de toepassing tussen labels `<connectionString>` en `<\connectionString>`.

Als u deze in plaats daarvan gedurende runtime opgeeft wilt, kunt u de volgende methode voordat de initialisatie van de agent betrokkenheid bellen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
    
    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

De verbindingsreeks voor uw toepassing wordt weergegeven in de klassieke Azure-Portal.

### <a name="items-name-change"></a>Items naam wijzigen

Alle items met de naam *capptain* hebben *betrokkenheid*naam gekregen. Op dezelfde manier voor *Capptain* naar *betrokkenheid*.

Voorbeelden van veelgebruikte Capptain items:

-   CapptainConfiguration EngagementConfiguration nu de naam
-   CapptainAgent EngagementAgent nu de naam
-   CapptainReach EngagementReach nu de naam
-   CapptainHttpConfig EngagementHttpConfig nu de naam
-   GetCapptainPageName GetEngagementPageName nu de naam

Houd er rekening mee dat naam heeft ook gevolgen voor overschreven methoden.

 
