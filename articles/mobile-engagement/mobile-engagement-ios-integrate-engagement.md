<properties
    pageTitle="Azure Mobile betrokkenheid iOS SDK integratie | Microsoft Azure"
    description="Meest recente updates en procedures voor iOS SDK voor Azure Mobile betrokkenheid"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-engagement-on-ios"></a>Hoe u integreren betrokkenheid op iOS

> [AZURE.SELECTOR]
- [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-apparaat](mobile-engagement-android-integrate-engagement.md)

Deze procedure worden de eenvoudigste manier om te activeren betrokkenheid van analyses en functies in uw iOS-toepassing voor controle.

De betrokkenheid SDK vereist iOS6 + en Xcode 8: het implementatiedoel van uw toepassing moet ten minste iOS 6.

> [AZURE.NOTE]
> Als u echt XCode 7 afhankelijk kunt u de [iOS betrokkenheid SDK v3.2.4](https://aka.ms/r6oouh). Er is een bekende fout op de module bereik van deze vorige versie terwijl uitgevoerd op apparaten met iOS 10 raadpleegt u [de integratie van de module bereik](mobile-engagement-ios-integrate-engagement-reach.md) voor meer informatie. Als u de v3.2.4 SDK gebruiken en vervolgens alleen overslaan de `UserNotifications.framework` importeren in de volgende stap.

De volgende stappen zijn selecteren om het rapport van logboeken die nodig zijn voor het berekenen van alle statistische gegevens over gebruikers, sessies, activiteiten, loopt en Technicals activeren. Het rapport van logboeken die nodig zijn voor het berekenen van andere statistieken worden aangepast, zoals gebeurtenissen, fouten en taken moet worden uitgevoerd handmatig de betrokkenheid-API gebruiken (Zie [het gebruik van de geavanceerde Mobile betrokkenheid labelen API in uw iOS-app](mobile-engagement-ios-use-engagement-api.md) sinds deze statistieken afhankelijk van de toepassing zijn.

##<a name="embed-the-engagement-sdk-into-your-ios-project"></a>De betrokkenheid SDK insluiten in uw iOS-project

- Download de iOS SDK vanuit [hier](http://aka.ms/qk2rnj).

- De betrokkenheid SDK toevoegen aan uw iOS-project: Xcode, klik met de rechtermuisknop op het project en selecteer in **'U bestanden toevoegen aan...'** en kies de `EngagementSDK` map.

- Betrokkenheid vereist extra kaders om te werken: in de Projectverkenner, opent u het deelvenster van uw project en selecteer het juiste doel. Vervolgens, open het tabblad **'Fasen maken'** en toevoegen in het menu **' koppeling binaire met bibliotheken '** deze kaders:

    -   `UserNotifications.framework`-de koppeling als instellen`Optional`
    -   `AdSupport.framework`-de koppeling als instellen`Optional`
    -   `SystemConfiguration.framework`
    -   `CoreTelephony.framework`
    -   `CFNetwork.framework`
    -   `CoreLocation.framework`
    -   `libxml2.dylib`

> [AZURE.NOTE] Het kader AdSupport kan worden verwijderd. Betrokkenheid moet dit kader voor het verzamelen van de IDFA. Echter IDFA siteverzameling kan worden uitgeschakeld \<ios-sdk-betrokkenheid-idfa\> om te voldoen aan de nieuwe Apple-beleid aangaande deze.

##<a name="initialize-the-engagement-sdk"></a>De betrokkenheid SDK geïnitialiseerd

U moet uw gemachtigde toepassing wijzigen:

-   Aan de bovenkant van uw implementatiebestand, importeren de betrokkenheid-agent:

        [...]
        #import "EngagementAgent.h"

-   Betrokkenheid geïnitialiseerd binnen de methode '**applicationDidFinishLaunching:**'of'**toepassing: didFinishLaunchingWithOptions:**':

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
          [...]
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
          [...]
        }

##<a name="basic-reporting"></a>Eenvoudige rapportage

### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a>Aanbevolen methode: overbelastingen uw `UIViewController` klassen

Om te activeren in het rapport van alle de logboeken aan de betrokkenheid te berekenen van gebruikers, sessies, activiteiten, loopt en technische statistieken vereist, kunt u gewoon maken alle uw `UIViewController` onderliggend klassen overnemen van de `EngagementViewController` klassen (dezelfde regel voor `UITableViewController`  - \> `EngagementTableViewController`).

**Zonder betrokkenheid:**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**Met betrokkenheid:**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a>Alternatieve methode: bellen `startActivity()` handmatig

Als u niet kunt of niet wilt overbelastingen uw `UIViewController` klassen, in plaats daarvan kunt u uw activiteiten starten door te bellen `EngagementAgent`van rechtstreeks methoden.

> [AZURE.IMPORTANT] De iOS SDK automatisch oproepen de `endActivity()` methode wanneer de toepassing is gesloten. Het is dus *ten zeerste* aanbevolen om te bellen de `startActivity` methode wanneer de activiteit van de gebruiker wijzigen en bellen op *nooit* de `endActivity` methode, aangezien het aanroepen van deze methode forceert u de huidige sessie moet worden beëindigd.

##<a name="location-reporting"></a>Locatie-rapportage

Servicevoorwaarden voor Apple staan geen toepassingen locatie bijhouden voor alleen statistieken doeleinden te gebruiken. Het verdient dus locatie rapporten inschakelen alleen als toepassingen ook gebruikmaken van de locatie bijhouden om een andere reden.

Beginnen met iOS 8, moet u een beschrijving voor het gebruik van services voor locatie een tekenreeks voor de sleutel [NSLocationWhenInUseUsageDescription] of [NSLocationAlwaysUsageDescription] door in te stellen van uw app Info.plist-bestand in uw app opgeven. Als u Rapportlocatie op de achtergrond met betrokkenheid wilt, kunt u de sleutel NSLocationAlwaysUsageDescription toevoegen. In alle andere gevallen, moet u de sleutel NSLocationWhenInUseUsageDescription toevoegen.

### <a name="lazy-area-location-reporting"></a>Rapportage van fikse gebied locatie

Rapportage van fikse gebied locatie kunt rapporteren van het land, regio en plaats die is gekoppeld aan apparaten. Dit type locatie rapportage wordt alleen gebruikt voor netwerklocaties (gebaseerd op de cel-ID of WIFI). Het gebied van het apparaat wordt gerapporteerd maximaal eenmaal per sessie. De GPS is nooit gebruikt en kan dus dit type locatie rapport erg weinig (niet te zeggen Nee) heeft invloed op de kleine.

Gerapporteerde gebieden worden gebruikt voor het berekenen van geografische statistieken over gebruikers, sessies, gebeurtenissen en fouten. Ze kunnen ook worden gebruikt als criterium in een bereik campagnes. Het laatste bekende gebied gerapporteerd voor een apparaat kan worden opgehaald met behulp van het [Apparaat API].

Als u wilt inschakelen fikse gebied locatie rapportage, voegt u de volgende regel na initialisatie van de betrokkenheid-agent:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a>Rapportage van realtime-locatie

Rapportage van realtime-locatie kan rapporteren van de breedte en hoogte die is gekoppeld aan apparaten. Standaard dit type locatie rapportage wordt alleen gebruikt voor netwerklocaties (gebaseerd op de cel-ID of WIFI) en de rapportage is alleen beschikbaar wanneer de toepassing wordt uitgevoerd op voorgrond (dat wil zeggen tijdens een sessie).

Realtime locaties zijn *niet* gebruikt om te berekenen van statistieken. De enige doel is toe te staan dat het gebruik van realtime geografische-fencing \<bereik-publiek-geofencing\> criterium in een bereik campagnes.

Als u wilt inschakelen realtime locatie rapportage, voegt u de volgende regel na initialisatie van de betrokkenheid-agent:

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a>GPS gebaseerd rapportage

Realtime locatie rapportage alleen standaard gebaseerd netwerklocaties. Schakel het gebruik van GPS op basis van locaties (dit zijn uiterst nauwkeuriger) door toevoegen:

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a>Rapportage-achtergrond

Standaard is realtime locatie rapportage alleen actief wanneer de toepassing wordt uitgevoerd op voorgrond (dat wil zeggen tijdens een sessie). Als u wilt de melden ook op de achtergrond inschakelen, toevoegen:

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [AZURE.NOTE] Wanneer de toepassing wordt uitgevoerd op achtergrond, alleen locaties op basis van een netwerk worden gerapporteerd, zelfs als u de GPS ingeschakeld.

Uitvoering van deze functie wordt [startMonitoringSignificantLocationChanges] aangeroepen wanneer uw toepassing naar de achtergrond gaat. Let erop dat deze wordt automatisch start opnieuw uw toepassing in de achtergrond als een nieuwe locatie gebeurtenis binnenkomt.

##<a name="advanced-reporting"></a>Geavanceerde rapportage

(Optioneel) als u rapporteren toepassing specifieke gebeurtenissen, fouten en taken wilt, moet u de betrokkenheid-API gebruiken via de methoden van de `EngagementAgent` class. Een object van deze klasse kan worden opgehaald door de ondersteuning voor de `[EngagementAgent shared]` statische methode.

De API betrokkenheid stelt al betrokkenheid van geavanceerde mogelijkheden en wordt beschreven in het gebruik van de API betrokkenheid op iOS (en van de technische documentatie van de `EngagementAgent` class).

##<a name="disable-idfa-collection"></a>IDFA siteverzameling uitschakelen

Standaard wordt betrokkenheid de [IDFA] gebruikt ter identificatie van een gebruiker. Maar als u niet reclame ergens anders in de app gebruikt, u kan worden geweigerd door de App Store controleren proces. IDFA siteverzameling kan worden uitgeschakeld door toe te voegen van de preprocessor macro `ENGAGEMENT_DISABLE_IDFA` in uw bestand pch (of in de `Build Settings` van uw toepassing). Dit zorgt ervoor dat er is geen verwijzingen naar `ASIdentifierManager`, `advertisingIdentifier` of `isAdvertisingTrackingEnabled` in het bouwen van toepassing.

Integratie in het bestand **prefix.pch** :

    #define ENGAGEMENT_DISABLE_IDFA
    ...

U kunt controleren of dat de verzameling IDFA wordt uitgeschakeld in uw toepassing door te schakelen van de betrokkenheid test Logboeken. Zie de integratie testen\<ios-sdk-betrokkenheid-test-idfa\> documentatie voor meer informatie.

##<a name="disable-log-reporting"></a>Log melden uitschakelen

### <a name="using-a-method-call"></a>Middel van een methode-gesprek

Als u betrokkenheid om te stoppen met het verzenden van Logboeken wilt, kunt u bellen:

    [[EngagementAgent shared] setEnabled:NO];

In dit gesprek is permanente: site gebruikmaakt van `NSUserDefaults` de gegevens wilt opslaan.

Kunt u rapportage opnieuw door te bellen met dezelfde functie log inschakelen `YES`.

### <a name="integration-in-your-settings-bundle"></a>Integratie in uw bundel instellingen

In plaats van oproepen van deze functie kunt u ook deze instelling integreren rechtstreeks in uw bestaande `Settings.bundle` bestand. De tekenreeks `engagement_agent_enabled` moet worden gebruikt als een de voorkeur-id en deze moeten worden gekoppeld aan een wisselknop (`PSToggleSwitchSpecifier`).

Het volgende voorbeeld van `Settings.bundle` ziet u hoe u deze implementeert:

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[Apparaat API]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
