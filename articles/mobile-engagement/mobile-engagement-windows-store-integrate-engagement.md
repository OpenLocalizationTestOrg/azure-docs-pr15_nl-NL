<properties 
    pageTitle="Windows universele Apps betrokkenheid SDK-integratie" 
    description="Azure mobiele betrokkenheid integreren met universele Apps voor Windows"                  
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

# <a name="windows-universal-apps-engagement-sdk-integration"></a>Windows universele Apps betrokkenheid SDK-integratie

> [AZURE.SELECTOR] 
- [Universele Windows](mobile-engagement-windows-store-integrate-engagement.md) 
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
- [iOS](mobile-engagement-ios-integrate-engagement.md) 
- [Android-apparaat](mobile-engagement-android-integrate-engagement.md) 

De eenvoudigste manier om te activeren betrokkenheid van analyses en functies in uw Windows universele toepassing Monitoring voor deze procedure wordt beschreven.

De volgende stappen zijn selecteren om het rapport van logboeken die nodig zijn voor het berekenen van alle statistische gegevens over gebruikers, sessies, activiteiten, loopt en Technicals activeren. Het rapport van logboeken die nodig zijn voor het berekenen van andere statistieken worden aangepast, zoals gebeurtenissen, fouten en taken moet worden uitgevoerd handmatig de betrokkenheid-API gebruiken (Zie [het gebruik van de geavanceerde Mobile betrokkenheid labelen API in uw Windows universele app](mobile-engagement-windows-store-use-engagement-api.md) sinds deze statistieken afhankelijk van de toepassing zijn.

## <a name="supported-versions"></a>Ondersteunde versies

De Mobile betrokkenheid SDK voor Windows universele-Apps kunnen alleen worden geïntegreerd in Windows Runtime en universele Windows-Platform toepassingen maken voor:

-   Windows 8
-   Windows 8.1
-   Windows Phone 8.1
-   Windows-10 (desktop en mobiele gezinnen)

> [AZURE.NOTE] Als u Windows Phone Silverlight hebt samengesteld verwijzen naar de [Windows Phone Silverlight-integratie procedure](mobile-engagement-windows-phone-integrate-engagement.md).

## <a name="install-the-mobile-engagement-universal-apps-sdk"></a>De mobiele betrokkenheid universele Apps SDK installeren

### <a name="all-platforms"></a>Alle platforms

De mobiele betrokkenheid SDK voor Windows universele App is beschikbaar als een Nuget-pakket *MicrosoftAzure.MobileEngagement*genoemd. U kunt dit installeren vanuit de Visual Studio Nuget pakket Manager.

### <a name="windows-8x-and-windows-phone-81"></a>Windows 8.x en Windows Phone 8.1

NuGet implementeert automatisch de SDK bronnen in de `Resources` map in de hoofdmap van uw toepassingsproject.

### <a name="windows-10-universal-windows-platform-applications"></a>Windows 10 universele Windows-Platform-toepassingen

NuGet bevat de SDK resources in uw UWP-toepassing nog niet automatisch implementeren. U moet dit handmatig doen totdat de implementatie van resources in NuGet terug:

1.  Open de Verkenner.
2.  Navigeer naar de volgende locatie (**x.x.x** is de versie van betrokkenheid u installeert): *% USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*
3.  Slepen en neerzetten van de map **bronnen** van de Verkenner naar de hoofdmap van uw project in Visual Studio.
4.  Selecteer van uw project in Visual Studio en het pictogram **bestanden Alles weergeven** boven aan de **Oplossing Explorer**activeren.
5.  Sommige bestanden worden niet opgenomen in het project. Als u wilt importeren Klik ze in één keer met de rechtermuisknop op de map **bronnen** , **uitsluiten van project** en vervolgens een andere Klik met de rechtermuisknop op de map **bronnen** , **opnemen in project** de hele map opnieuw opnemen. Alle bestanden uit de map **Resources** worden nu opgenomen in uw project.

## <a name="add-the-capabilities"></a>De mogelijkheden toevoegen

De betrokkenheid SDK moet een aantal mogelijkheden van de Windows SDK pakketbestanden.

Open uw `Package.appxmanifest` archiveren en zorg dat de volgende mogelijkheden worden aangegeven:

-   `Internet (Client)`

## <a name="initialize-the-engagement-sdk"></a>De betrokkenheid SDK geïnitialiseerd

### <a name="engagement-configuration"></a>Betrokkenheid configuratie

De configuratie betrokkenheid zijn ondergebracht in de `Resources\EngagementConfiguration.xml` bestand van uw project.

Bewerken dit bestand om op te geven:

-   De verbindingsreeks van de toepassing tussen labels `<connectionString>` en `<\connectionString>`.

Als u deze in plaats daarvan gedurende runtime opgeeft wilt, kunt u de volgende methode voordat de initialisatie van de agent betrokkenheid bellen:
          
          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

De verbindingsreeks voor uw toepassing wordt weergegeven in de klassieke Azure-Portal.

### <a name="engagement-initialization"></a>Initialisatie van de betrokkenheid

Wanneer u een nieuw project maakt een `App.xaml.cs` bestand is gegenereerd. Deze klasse overneemt van `Application` en bevat veel belangrijke methoden. Dit wordt ook gebruikt initialisatie van de betrokkenheid SDK.

Wijzig de `App.xaml.cs`:

-   Toevoegen aan uw `using` instructies:

        using Microsoft.Azure.Engagement;

-   Een methode als u wilt delen van de initialisatie van de betrokkenheid eenmaal voor alle gesprekken definiëren:

        private void InitEngagement(IActivatedEventArgs e)
        {
          EngagementAgent.Instance.Init(e);
        
          // or
        
          EngagementAgent.Instance.Init(e, engagementConfiguration);
        }
        
-   Bellen `InitEngagement` in de `OnLaunched` methode:

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
          InitEngagement(e);
        }

-   Wanneer uw toepassing wordt gestart met een aangepast kleurenschema, een andere toepassing of voor de opdrachtregel en vervolgens de `OnActivated` methode wordt genoemd. U moet ook de betrokkenheid SDK initiëren wanneer de app wordt geactiveerd. Klik hiervoor overschrijven `OnActivated` methode:

        protected override void OnActivated(IActivatedEventArgs args)
        {
          InitEngagement(args);
        }

> [AZURE.IMPORTANT] We ontmoedigen raden u om toe te voegen van de initialisatie van de betrokkenheid in een andere locatie van de toepassing.

## <a name="basic-reporting"></a>Eenvoudige rapportage

### <a name="recommended-method-overload-your-page-classes"></a>Aanbevolen methode: overbelastingen uw `Page` klassen

Om te activeren in het rapport van alle de logboeken aan de betrokkenheid te berekenen van gebruikers, sessies, activiteiten, loopt en technische statistieken vereist, kunt u gewoon maken alle uw `Page` onderliggend klassen overnemen van de `EngagementPage` klassen.

Hier volgt een voorbeeld van hoe u deze stap herhalen voor een pagina van de toepassing. U kunt hetzelfde te doen voor alle pagina's van uw toepassing.

#### <a name="c-source-file"></a>C# bronbestand

Wijzigen van de pagina `.xaml.cs` bestand:

-   Toevoegen aan uw `using` instructies:

        using Microsoft.Azure.Engagement;

-   Vervang `Page` met `EngagementPage`:

**Zonder betrokkenheid:**
    
        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**Met betrokkenheid:**

        using Microsoft.Azure.Engagement;
        
        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [AZURE.IMPORTANT] Als uw pagina overschrijft de `OnNavigatedTo` methode, zorg ervoor dat u belt `base.OnNavigatedTo(e)`. Anders de activiteit niet worden gerapporteerd (de `EngagementPage` oproepen `StartActivity` binnen de `OnNavigatedTo` methode).

#### <a name="xaml-file"></a>XAML-bestand

Wijzigen van de pagina `.xaml` bestand:

-   Toevoegen aan de declaraties naamruimten:

        xmlns:engagement="using:Microsoft.Azure.Engagement"

-   Vervang `Page` met `engagement:EngagementPage`:

**Zonder betrokkenheid:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**Met betrokkenheid:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-the-default-behaviour"></a>Het gedrag van de standaard overschrijven

Standaard is de naam van de klasse van de pagina als de naam van de activiteit, klikt u met geen extra gerapporteerd. Als de klas gebruikmaakt van het achtervoegsel 'Pagina', betrokkenheid worden dan ook verwijderd.

Als u overschrijven de standaard-gedrag voor de naam wilt, gewoon u dit toevoegen aan uw code:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Als u rapporteren enkele extra gegevens met uw activiteit wilt, kunt u dit toevoegen aan uw code:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Deze methoden worden aangeroepen vanuit de `OnNavigatedTo` methode van de pagina.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatieve methode: bellen `StartActivity()` handmatig

Als u niet kunt of niet wilt overbelastingen uw `Page` klassen, in plaats daarvan kunt u uw activiteiten starten door te bellen `EngagementAgent` rechtstreeks methoden.

Het is raadzaam te bellen `StartActivity` binnen uw `OnNavigatedTo` methode van de pagina.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Controleer of dat u uw sessie correct beëindigen.
> 
> De Windows universele SDK oproepen automatisch de `EndActivity` methode wanneer de toepassing is gesloten. Het is dus **ten zeerste** aanbevolen om te bellen de `StartActivity` methode wanneer de activiteit van de gebruiker wijzigen en bellen op **nooit** de `EndActivity` methode, deze methode verzendt naar betrokkenheid server dat huidige gebruiker heeft de toepassing achterblijven, dat dit is van invloed op alle toepassingslogboeken aan de.

## <a name="advanced-reporting"></a>Geavanceerde rapportage

U kunt rapport toepassing specifieke gebeurtenissen, fouten en taken, hiervoor gebruik desgewenst de andere methoden zijn gevonden in de `EngagementAgent` class. De betrokkenheid-API kunt alle betrokkenheid van geavanceerde mogelijkheden gebruiken.

Zie [het gebruik van de geavanceerde Mobile betrokkenheid labelen API in uw Windows universele-app](mobile-engagement-windows-store-use-engagement-api.md)voor meer informatie.

##<a name="advanced-configuration"></a>Geavanceerde configuratie

### <a name="disable-automatic-crash-reporting"></a>Automatische foutenrapportage uitschakelen

U kunt het automatische vastlopen rapportage-functie van betrokkenheid uitschakelen. Vervolgens gaat u als een onverwerkte uitzondering wordt uitgevoerd, ze toch niet betrokkenheid doet.

> [AZURE.WARNING] Als u van plan bent deze functie uitschakelen, let dat wanneer een onverwerkte vastlopen in uw app wordt uitgevoerd, betrokkenheid niet stuurt dat de vastlopen **en** wordt niet sluit de sessie en taken.

Als u wilt uitschakelen automatische foutenrapportage, alleen uw configuratie afhankelijk van de manier waarop die u deze gedeclareerd aanpassen:

#### <a name="from-engagementconfigurationxml-file"></a>Uit `EngagementConfiguration.xml` bestand

Rapport vastlopen ingesteld op `false` tussen `<reportCrash>` en `</reportCrash>` tags.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Uit `EngagementConfiguration` object tijdens runtime

Rapport vastlopen ingesteld op onwaar met behulp van uw EngagementConfiguration-object.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        
        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Burstmodus

Standaard logboeken op de betrokkenheid service-rapporten in realtime. Als uw toepassing Logboeken zeer vaak rapporten, is het beter in de buffer de logboeken en om deze te melden in één keer op een gewone tijd grondtal (dit is de modus' burst' genoemd).

Bel de methode hiervoor:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Het argument is een waarde (in **milliseconden)**. Als u activeren de realtime logboekregistratie wilt, belt op elk moment alleen de methode zonder een parameter of met de waarde 0.

De modus burst iets langere levensduur maar meteen invloed heeft op de Monitor betrokkenheid: alle sessies en taken duur afgerond op de drempelwaarde voor burst (dus sessies en taken korter dan de drempelwaarde voor burst zijn mogelijk niet zichtbaar). Het wordt aanbevolen een burst drempel niet meer dan 30000 (30s) gebruiken. U moet weten dat opgeslagen logboeken zijn beperkt tot 300 items. Als het verzenden van te lang is kunt u sommige logboeken gaan verloren.

> [AZURE.WARNING] De drempelwaarde voor burst kan niet worden geconfigureerd met een lager dan 1s periode. Als u probeert te doen, wordt de SDK een spoor met de fout wordt weergegeven en wordt automatisch ingesteld op de standaardwaarde, dat wil zeggen 0s. Hiermee wordt de SDK als u wilt rapporteren van de logboeken in realtime activeren.

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
 
