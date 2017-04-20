<properties
    pageTitle="Het gebruik van de API betrokkenheid op iOS"
    description="Meest recente iOS SDK - het gebruik van de API betrokkenheid op iOS"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-use-the-engagement-api-on-ios"></a>Het gebruik van de API betrokkenheid op iOS

Dit document is een invoegtoepassing voor het document het integreren betrokkenheid op iOS: biedt diepteas details over het gebruik van de API betrokkenheid rapporteren van de toepassing statistieken.

Houd er rekening mee dat als u alleen betrokkenheid rapporteren van uw toepassing sessies, activiteiten, loopt en technische informatie wilt, klikt u vervolgens de eenvoudigste manier is om te maken van alle uw aangepaste `UIViewController` objecten overnemen van de bijbehorende `EngagementViewController` class.

Als u wilt doen meer, bijvoorbeeld als u wilt rapporteren toepassing specifieke gebeurtenissen, fouten en taken, of als u moet rapporteren van uw toepassing activiteiten op een andere manier dan de geïmplementeerd in de `EngagementViewController` klassen, moet u de betrokkenheid-API gebruiken.

De API betrokkenheid is verstrekt door de `EngagementAgent` class. Een exemplaar van deze klasse kan worden opgehaald door de ondersteuning voor de `[EngagementAgent shared]` statische methode (Houd er rekening mee dat het `EngagementAgent` object dat het resultaat is een singleton).

Voordat u een API-gesprekken, de `EngagementAgent` object moet worden geïnitialiseerd door het aanroepen van de methode`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

##<a name="engagement-concepts"></a>Betrokkenheid concepten

De volgende onderdelen Verfijn de algemene [Mobiele betrokkenheid concepten](mobile-engagement-concepts.md) voor de iOS-platform.

### <a name="session-and-activity"></a>`Session`en`Activity`

Een *activiteit* wordt meestal veroorzaakt door één scherm van de toepassing, dat wil zeggen de *activiteit* wordt gestart wanneer het scherm wordt weergegeven en stopt wanneer het scherm is gesloten: dit het geval is, wanneer u de betrokkenheid SDK is geïntegreerd met behulp van de `EngagementViewController` klassen.

Maar *activiteiten* kunnen ook handmatig met behulp van de betrokkenheid-API worden beheerd. In deze sectie geeft het splitsen van een bepaald scherm in verschillende sub delen voor meer informatie over het gebruik van dit scherm (bijvoorbeeld tot bekende hoe vaak en hoe lang dialoogvensters worden gebruikt in dit scherm).

##<a name="reporting-activities"></a>Rapportage van activiteiten

### <a name="user-starts-a-new-activity"></a>Gebruiker start een nieuwe activiteit

            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

U moet bellen `startActivity()` telkens wanneer de gebruiker activiteit wijzigingen. De eerste aanroep voor deze functie wordt een nieuwe gebruikerssessie gestart.

### <a name="user-ends-his-current-activity"></a>Gebruiker eindigt haar huidige activiteit

            [[EngagementAgent shared] endActivity];

> [AZURE.WARNING] U moet **nooit** deze functie bellen door zelf, behalve als u wilt splitsen één gebruik van de toepassing in meerdere sessies: een oproep voor deze functie zo is, onmiddellijk, de huidige sessie wilt beëindigen een volgende aanroep `startActivity()` een nieuwe sessie wilt starten. Deze functie wordt automatisch aangeroepen door de SDK wanneer uw toepassing is gesloten.

##<a name="reporting-events"></a>Rapportage van gebeurtenissen

### <a name="session-events"></a>Sessiegebeurtenissen

Sessiegebeurtenissen worden meestal gebruikt om de acties uitgevoerd door een gebruiker tijdens zijn sessie.

**Voorbeeld zonder extra gegevens:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**Voorbeeld met extra gegevens:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a>Zelfstandige gebeurtenissen

In tegenstelling tot sessiegebeurtenissen, kunnen de zelfstandige gebeurtenissen buiten de context van een sessie worden gebruikt.

**Voorbeeld:**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

##<a name="reporting-errors"></a>Rapportage van fouten

### <a name="session-errors"></a>Sessie fouten

Sessie fouten worden meestal gebruikt om de fouten die invloed hebben op de gebruiker tijdens zijn sessie.

**Voorbeeld:**

    /** The user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* The user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a>Zelfstandige versie van fouten

In tegenstelling tot sessie fouten, kunnen de zelfstandige fouten buiten de context van een sessie worden gebruikt.

**Voorbeeld:**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

##<a name="reporting-jobs"></a>Taken rapporteren

**Voorbeeld:**

Stel dat u wilt rapporteren van de duur van uw aanmeldingsproces:

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a>Rapportfouten gedurende een project

Fouten kunnen zijn gerelateerd aan een lopend project in plaats van dat wordt gerelateerd aan de sessie van de huidige gebruiker.

**Voorbeeld:**

Stel dat u wilt rapporteren van een fout tijdens uw aanmeldingsproces:

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try to sign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a>Gebeurtenissen gedurende een project

Gebeurtenissen kunnen zijn gerelateerd aan een lopend project in plaats van dat wordt gerelateerd aan de sessie van de huidige gebruiker.

**Voorbeeld:**

Stel dat we hebben een sociaal netwerk en we een taak voor het rapport gebruiken de totale tijd waarin de gebruiker is verbonden met de server. De gebruiker van zijn vrienden kunt ontvangen, is dit een taakgebeurtenis.

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

##<a name="extra-parameters"></a>Extra parameters

Willekeurige gegevens kan worden gekoppeld aan gebeurtenissen, fouten, activiteiten en taken.

Deze gegevens kan worden onderverdeeld, de site gebruikmaakt van iOS NSDictionary class.

Extra's kunnen bevatten `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` of andere `NSDictionary` exemplaren.

> [AZURE.NOTE] De extra parameter wordt omgezet in JSON. Als u verschillende objecten dan de wilt hierboven beschreven doorgeeft, moet u de volgende methode implementeren in uw klas:
>
             -(NSString*)JSONRepresentation;
>
> De methode moet een JSON-weergave van het object te retourneren.

### <a name="example"></a>Voorbeeld

    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a>Limieten

#### <a name="keys"></a>Toetsen

Elke sleutel in de `NSDictionary` moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*`

Dit betekent dat sleutels met ten minste één letter beginnen moeten, gevolgd door letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte

Extra's zijn beperkt tot **1024** tekens per gesprek (één keer gecodeerd in JSON door de betrokkenheid agent).

In het vorige voorbeeld is de JSON verzonden naar de server 58 tekens bevatten:

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

##<a name="reporting-application-information"></a>Informatie over toepassingen melden

U kunt handmatig melden voor het bijhouden van informatie (of een andere toepassing nauwkeurige gegevens) Gebruik de `sendAppInfo:` functie.

Opmerking deze informatie stapsgewijs kan worden verzonden: alleen de meest recente waarde voor een bepaalde sleutel voor een bepaald apparaat worden bewaard.

Gebeurtenis-extra's, zoals de `NSDictionary` klasse wordt gebruikt voor informatie over toepassingen abstracte, houd er rekening mee dat matrices of onderliggend woordenlijsten wordt behandeld als platte tekenreeksen (via JSON serialisatie).

**Voorbeeld:**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a>Limieten

#### <a name="keys"></a>Toetsen

Elke sleutel in de `NSDictionary` moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*`

Dit betekent dat sleutels met ten minste één letter beginnen moeten, gevolgd door letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte

Toepassingsinformatie zijn beperkt tot **1024** tekens per gesprek (één keer gecodeerd in JSON door de betrokkenheid-agent).

In het vorige voorbeeld is de JSON verzonden naar de server 44 tekens bevatten:

    {"birthdate":"1983-12-07","gender":"female"}
