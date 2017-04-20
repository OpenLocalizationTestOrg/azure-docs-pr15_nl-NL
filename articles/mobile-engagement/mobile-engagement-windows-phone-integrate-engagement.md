<properties 
    pageTitle="Windows Phone Silverlight betrokkenheid SDK-integratie" 
    description="Azure mobiele betrokkenheid integreren met Windows Phone Silverlight-Apps"                  
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-engagement-sdk-integration"></a>Windows Phone Silverlight betrokkenheid SDK-integratie

> [AZURE.SELECTOR] 
- [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android-apparaat](mobile-engagement-android-integrate-engagement.md) 

De eenvoudigste manier om te activeren Azure Mobile betrokkenheid analyses en controle van functies in uw Windows Phone Silverlight-toepassing voor deze procedure wordt beschreven.

De volgende stappen zijn selecteren om het rapport van logboeken die nodig zijn voor het berekenen van alle statistische gegevens over gebruikers, sessies, activiteiten, loopt en Technicals activeren. Het rapport van logboeken die nodig zijn voor het berekenen van andere statistieken worden aangepast, zoals gebeurtenissen, fouten en taken moet worden uitgevoerd handmatig de betrokkenheid-API gebruiken (Zie onderstaande [het gebruik van de geavanceerde Mobile betrokkenheid labelen API in uw Windows Phone Silverlight-app](mobile-engagement-windows-phone-use-engagement-api.md) ) omdat deze statistieken afhankelijk zijn van toepassing.

##<a name="supported-versions"></a>Ondersteunde versies

De SDK met betrokkenheid Mobile voor Windows Silverlight kunnen alleen worden geïntegreerd in toepassingen maken voor:

-   Windows Phone 8.0
-   Windows Phone 8.1 Silverlight

> [AZURE.NOTE] Als u Windows Phone 8.1 (niet-Silverlight) hebt samengesteld verwijzen naar de [Windows universele integratie procedure](mobile-engagement-windows-store-integrate-engagement.md).

##<a name="install-the-mobile-engagement-silverlight-sdk"></a>De SDK van mobiele betrokkenheid Silverlight installeren

De SDK met betrokkenheid Mobile voor Windows Silverlight is beschikbaar als een Nuget-pakket *MicrosoftAzure.MobileEngagement*genoemd. U kunt dit installeren vanuit de Visual Studio Nuget pakket Manager. 

##<a name="add-the-capabilities"></a>De mogelijkheden toevoegen

De betrokkenheid SDK moet een aantal mogelijkheden van de Windows Phone Silverlight SDK pakketbestanden.

Open uw `WMAppManifest.xml` archiveren en zorg dat de volgende mogelijkheden zijn gedefinieerd in de `Capabilities` Configuratiescherm:

-   `ID_CAP_NETWORKING`
-   `ID_CAP_IDENTITY_DEVICE`

##<a name="initialize-the-engagement-sdk"></a>De betrokkenheid SDK geïnitialiseerd

### <a name="engagement-configuration"></a>Betrokkenheid configuratie

De configuratie betrokkenheid zijn ondergebracht in de `Resources\EngagementConfiguration.xml` bestand van uw project.

Bewerken dit bestand om op te geven:

-   De verbindingsreeks van de toepassing tussen labels `<connectionString>` en `<\connectionString>`.

Als u deze in plaats daarvan gedurende runtime opgeeft wilt, kunt u de volgende methode voordat de initialisatie van de agent betrokkenheid bellen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

De verbindingsreeks voor uw toepassing wordt weergegeven in de klassieke Azure-Portal.

### <a name="engagement-initialization"></a>Initialisatie van de betrokkenheid

Wanneer u een nieuw project maakt een `App.xaml.cs` bestand is gegenereerd. Deze klasse overneemt van `Application` en bevat veel belangrijke methoden. Dit wordt ook gebruikt initialisatie van de betrokkenheid SDK.

Wijzig de `App.xaml.cs`:

-   Toevoegen aan uw `using` instructies:

        using Microsoft.Azure.Engagement;

-   Invoegen `EngagementAgent.Instance.Init` in de `Application_Launching` methode:

        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
          EngagementAgent.Instance.Init();
        }

-   Invoegen `EngagementAgent.Instance.OnActivated` in de `Application_Activated` methode:

        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
        }

> [AZURE.WARNING] We ontmoedigen raden u om toe te voegen van de initialisatie van de betrokkenheid in een andere locatie van de toepassing. Echter, moet u zich realiseren dat de `EngagementAgent.Instance.Init` methode wordt uitgevoerd op een speciale thread en niet op de UI-thread.

##<a name="basic-reporting"></a>Eenvoudige rapportage

### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a>Aanbevolen methode: overbelastingen uw `PhoneApplicationPage` klassen

Om te activeren in het rapport van alle de logboeken aan de betrokkenheid te berekenen van gebruikers, sessies, activiteiten, loopt en technische statistieken vereist, kunt u gewoon maken alle uw `PhoneApplicationPage` onderliggend klassen overnemen van de `EngagementPage` klassen.

Hier volgt een voorbeeld van hoe u deze stap herhalen voor een pagina van de toepassing. U kunt hetzelfde te doen voor alle pagina's van uw toepassing.

#### <a name="c-source-file"></a>C# bronbestand

Wijzigen van de pagina `.xaml.cs` bestand:

-   Toevoegen aan uw `using` instructies:

        using Microsoft.Azure.Engagement;

-   Vervang `PhoneApplicationPage` met `EngagementPage` :

**Zonder betrokkenheid:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**Met betrokkenheid:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.WARNING] Als uw pagina van overneemt de `OnNavigatedTo` methode, pas op dat laat de `base.OnNavigatedTo(e)` bellen. Anders worden de activiteit niet gerapporteerd. Daadwerkelijk, de `EngagementPage` is bellen `StartActivity` binnen de `OnNavigatedTo` methode.

#### <a name="xaml-file"></a>XAML-bestand

Wijzigen van de pagina `.xaml` bestand:

-   Toevoegen aan de declaraties naamruimten:

        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"

-   Vervang `phone:PhoneApplicationPage` met `engagement:EngagementPage` :

**Zonder betrokkenheid:**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**Met betrokkenheid:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">
        
            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-the-default-behavior"></a>Het standaardgedrag te overschrijven

Standaard is de naam van de klasse van de pagina als de naam van de activiteit, klikt u met geen extra gerapporteerd. Als de klas gebruikmaakt van het achtervoegsel 'Pagina', betrokkenheid worden dan ook verwijderd.

Als u overschrijven het standaardgedrag voor de naam wilt, gewoon u dit toevoegen aan uw code:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

Als u rapporteren enkele extra informatie met uw activiteit wilt, kunt u dit toevoegen aan uw code:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

Deze methoden worden aangeroepen vanuit de `OnNavigatedTo` methode van de pagina.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatieve methode: bellen `StartActivity()` handmatig

Als u niet kunt of niet wilt overbelastingen uw `PhoneApplicationPage` klassen, in plaats daarvan kunt u uw activiteiten starten door te bellen `EngagementAgent` rechtstreeks methoden.

Het is raadzaam om te bellen kwijt te `StartActivity` binnen uw `OnNavigatedTo` methode van uw PhoneApplicationPage.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [AZURE.IMPORTANT] Controleer of dat u uw sessie correct beëindigen.
>
> De SDK oproepen automatisch de `EndActivity` methode wanneer de toepassing is gesloten. Het is dus **ten zeerste** aanbevolen om te bellen de `StartActivity` methode wanneer de activiteit van de gebruiker wijzigen en bellen op **nooit** de `EndActivity` methode. Deze methode stuurt een bericht naar de server betrokkenheid, dat de huidige gebruiker de toepassing verlaten heeft en dit van invloed op alle toepassingslogboeken aan de.

##<a name="advanced-reporting"></a>Geavanceerde rapportage

U kunt rapport toepassing specifieke gebeurtenissen, fouten en taken, hiervoor gebruik desgewenst de andere methoden zijn gevonden in de `EngagementAgent` class. De betrokkenheid-API kunt alle betrokkenheid van geavanceerde mogelijkheden gebruiken.

Zie [het gebruik van de geavanceerde Mobile betrokkenheid labelen API in uw Windows Phone Silverlight-app](mobile-engagement-windows-phone-use-engagement-api.md)voor meer informatie.

##<a name="advanced-configuration"></a>Geavanceerde configuratie

### <a name="disable-automatic-crash-reporting"></a>Automatische foutenrapportage uitschakelen

U kunt het automatische vastlopen rapportage-functie van betrokkenheid uitschakelen. Vervolgens gaat u als een onverwerkte uitzondering wordt uitgevoerd, ze toch niet betrokkenheid doet.

> [AZURE.WARNING] Als u van plan bent deze functie uitschakelen, let dat wanneer een onverwerkte vastlopen in uw app wordt uitgevoerd, betrokkenheid niet de vastlopen **en** deze niet wordt gesloten stuurt, de sessie en taken.

Als u wilt uitschakelen automatische foutenrapportage, alleen uw configuratie afhankelijk van de manier waarop die u deze gedeclareerd aanpassen:

#### <a name="from-engagementconfigurationxml-file"></a>Uit `EngagementConfiguration.xml` bestand

Rapport vastlopen ingesteld op `false` tussen `<reportCrash>` en `</reportCrash>` tags.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Uit `EngagementConfiguration` object tijdens runtime

Rapport vastlopen ingesteld op onwaar met behulp van uw EngagementConfiguration-object.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Burstmodus

Standaard logboeken op de betrokkenheid service-rapporten in realtime. Als uw toepassing Logboeken zeer vaak rapporten, is het beter in de buffer de logboeken en om deze te melden in één keer op een gewone tijd grondtal (dit is de modus' burst' genoemd).

Bel de methode hiervoor:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Het argument is een waarde (in **milliseconden)**. Als u activeren de realtime logboekregistratie wilt, belt op elk moment alleen de methode zonder een parameter of met de waarde 0.

De modus burst iets langere levensduur maar meteen invloed heeft op de Monitor betrokkenheid: alle sessies en taken duur afgerond op de drempelwaarde voor burst (dus sessies en taken korter dan de drempelwaarde voor burst zijn mogelijk niet zichtbaar). Het wordt aanbevolen een burst drempel niet meer dan 30000 (30s) gebruiken. U moet weten dat opgeslagen logboeken zijn beperkt tot 300 items. Als het verzenden van te lang is kunt u sommige logboeken gaan verloren.

> [AZURE.WARNING] De drempelwaarde voor burst kan niet worden geconfigureerd met een lagere periode dan één seconde. Als u probeert te doen, de SDK een spoor met de fout en wordt automatisch ingesteld op de standaardwaarde en wordt weergegeven dat wil zeggen nul seconden. Hiermee wordt de SDK als u wilt rapporteren van de logboeken in realtime activeren.
 
