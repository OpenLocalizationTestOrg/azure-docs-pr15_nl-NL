<properties
    pageTitle="Windows-Universal geavanceerde rapportage met MobileApps betrokkenheid"
    description="Azure mobiele betrokkenheid integreren met universele Apps voor Windows"                  
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-reporting-with-the-windows-universal-apps-engagement-sdk"></a>Geavanceerde rapportage met het Windows universele Apps betrokkenheid SDK

> [AZURE.SELECTOR]
- [Universele Windows](mobile-engagement-windows-store-advanced-reporting.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-apparaat](mobile-engagement-android-advanced-reporting.md)

In dit onderwerp wordt beschreven extra rapportage-scenario's in uw Windows universele toepassing. Deze scenario's bevatten de opties die u wilt toepassen op de app die is gemaakt in de handleiding aan de [Slag](mobile-engagement-windows-store-dotnet-get-started.md) kunt.

## <a name="prerequisites"></a>Vereisten voor

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

Voordat u deze zelfstudie begint, moet u eerst de zelfstudie [Aan de slag](mobile-engagement-windows-store-dotnet-get-started.md) , dat wil opzettelijk directe en eenvoudig zeggen uitvoeren. Deze zelfstudie behandelt extra opties die kunt u kiezen uit.

## <a name="specifying-engagement-configuration-at-runtime"></a>Configuratie van de betrokkenheid gedurende runtime opgeven

De configuratie betrokkenheid zijn ondergebracht in de `Resources\EngagementConfiguration.xml` bestand van uw project, dat wil zeggen waar deze is opgegeven in het onderwerp [Aan de slag](mobile-engagement-windows-store-dotnet-get-started.md) .

Maar u kunt het ook opgeven gedurende runtime: u kunt de volgende methode voordat de initialisatie van de agent betrokkenheid bellen:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a>Aanbevolen methode: overbelastingen uw `Page` klassen

Als u wilt activeren op de melding van de logboekbestanden die zijn vereist betrokkenheid te berekenen van gebruikers, sessies, activiteiten, loopt en technische statistieken, Controleer alle uw `Page` onderliggend klassen overnemen van de `EngagementPage` klassen.

Hier volgt een voorbeeld voor een pagina van de toepassing. U kunt hetzelfde te doen voor alle pagina's van uw toepassing.

### <a name="c-source-file"></a>C# bronbestand

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

### <a name="xaml-file"></a>XAML-bestand

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

### <a name="override-the-default-behaviour"></a>Het gedrag van de standaard overschrijven

Standaard is de naam van de klasse van de pagina als de naam van de activiteit, klikt u met geen extra gerapporteerd. Als de klas gebruikmaakt van het achtervoegsel 'Pagina', wordt deze door betrokkenheid verwijderd.

Als u wilt overschrijven het standaardgedrag voor de naam, moet u deze code toevoegen:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

Naar het rapporteren van extra informatie met uw activiteit, voegt u deze code:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

Deze methoden worden aangeroepen vanuit de `OnNavigatedTo` methode van de pagina.

### <a name="alternate-method-call-startactivity-manually"></a>Alternatieve methode: bellen `StartActivity()` handmatig

Als u niet kunt of niet wilt overbelastingen uw `Page` klassen, in plaats daarvan kunt u uw activiteiten starten door te bellen `EngagementAgent` rechtstreeks methoden.

Het is raadzaam om te bellen kwijt te `StartActivity` binnen uw `OnNavigatedTo` methode van de pagina.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [AZURE.IMPORTANT]  Controleer of dat u uw sessie correct beëindigen.
>
> De Windows universele SDK oproepen automatisch de `EndActivity` methode wanneer de toepassing is gesloten. Het is dus **ten zeerste** aanbevolen om te bellen de `StartActivity` methode wanneer de activiteit van de gebruiker wijzigen en bellen op **nooit** de `EndActivity` methode. Deze methode krijgt de server betrokkenheid dat de huidige gebruiker de toepassing, die van invloed zijn alle toepassingslogboeken aan de heeft verlaten.

## <a name="advanced-reporting"></a>Geavanceerde rapportage

(Optioneel) u kunt rapporteren toepassingsspecifieke gebeurtenissen, fouten en taken, kunt doen, gebruikt u de andere methoden zijn gevonden in de `EngagementAgent` class. De API betrokkenheid kunt gebruik van geavanceerde mogelijkheden van alle betrokkenheid.

Zie [het gebruik van de geavanceerde Mobile betrokkenheid labelen API in uw Windows universele-app](mobile-engagement-windows-store-use-engagement-api.md)voor meer informatie.
