<properties 
    pageTitle="Het gebruik van de betrokkenheid API op Windows Universal" 
    description="Het gebruik van de betrokkenheid API op Windows Universal"            
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

#<a name="how-to-use-the-engagement-api-on-windows-universal"></a>Het gebruik van de betrokkenheid API op Windows Universal

In dit document is een invoegtoepassing voor het document [het integreren betrokkenheid op Windows universele](mobile-engagement-windows-store-integrate-engagement.md): biedt diepteas details over het gebruik van de API betrokkenheid rapporteren van de toepassing statistieken.

Houd er rekening mee dat als u alleen betrokkenheid rapporteren van uw toepassing sessies, activiteiten, loopt en technische informatie wilt, klikt u vervolgens de eenvoudigste manier is alle uw `Page` onderliggend klassen overnemen van de `EngagementPage` class.

Als u wilt doen meer, bijvoorbeeld als u wilt rapporteren toepassing specifieke gebeurtenissen, fouten en taken, of als u moet rapporteren van uw toepassing activiteiten op een andere manier dan de geïmplementeerd in de `EngagementPage` klassen, moet u de betrokkenheid-API gebruiken.

De API betrokkenheid is verstrekt door de `EngagementAgent` class. U kunt toegang tot deze methoden tot en met `EngagementAgent.Instance`.

Zelfs als de module agent is niet geïnitialiseerd, elke oproep tot de API is uitgesteld en opnieuw worden uitgevoerd wanneer de agent beschikbaar is.

##<a name="engagement-concepts"></a>Betrokkenheid concepten

De volgende onderdelen Verfijn de algemene [Mobiele betrokkenheid concepten](mobile-engagement-concepts.md) voor het universele Windows-platform.

### <a name="session-and-activity"></a>`Session`en`Activity`

Een *activiteit* wordt meestal veroorzaakt door één pagina van de toepassing, dat wil zeggen de *activiteit* wordt gestart wanneer de pagina wordt weergegeven en stopt wanneer de pagina is gesloten: dit het geval is, wanneer u de betrokkenheid SDK is geïntegreerd met behulp van de `EngagementPage` class.

Maar *activiteiten* kunnen ook handmatig met behulp van de betrokkenheid-API worden beheerd. Hiermee kunt u het splitsen van een bepaalde pagina in verschillende sub delen voor meer informatie over het gebruik van deze pagina (bijvoorbeeld om te weten hoe vaak en hoe lang dialoogvensters worden gebruikt in deze pagina).

##<a name="reporting-activities"></a>Rapportage van activiteiten

### <a name="user-starts-a-new-activity"></a>Gebruiker start een nieuwe activiteit

#### <a name="reference"></a>Verwijzing

            void StartActivity(string name, Dictionary<object, object> extras = null)

U moet bellen `StartActivity()` telkens wanneer de gebruiker activiteit wijzigingen. De eerste aanroep voor deze functie wordt een nieuwe gebruikerssessie gestart.

> [AZURE.IMPORTANT] De SDK oproepen automatisch de methode EndActivity wanneer de toepassing is gesloten. Dus is het raadzaam om te bellen van de methode StartActivity wanneer de activiteit van de wijzigingen van de gebruiker en op nooit de aanroept EndActivity, aangezien het aanroepen van deze methode forceert u de huidige sessie moet worden beëindigd.

#### <a name="example"></a>Voorbeeld

            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Gebruiker eindigt haar huidige activiteit

#### <a name="reference"></a>Verwijzing

            void EndActivity()

Dit eindigt de activiteit en de sessie. U moet deze methode niet bellen, tenzij u weet wat u doet.

#### <a name="example"></a>Voorbeeld

            EngagementAgent.Instance.EndActivity();

##<a name="reporting-jobs"></a>Taken rapporteren

### <a name="start-a-job"></a>Een taak starten

#### <a name="reference"></a>Verwijzing

            void StartJob(string name, Dictionary<object, object> extras = null)

U kunt de taak sommige taken bijhouden die gedurende een periode.

#### <a name="example"></a>Voorbeeld

            // An upload begins...
            
            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");
            
            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>Een taak beëindigen

#### <a name="reference"></a>Verwijzing

            void EndJob(string name)

Zodra u een taak bijgehouden door een taak is beëindigd, moet u de methode EndJob voor deze taak, bellen door het opgeven van de taaknaam.

#### <a name="example"></a>Voorbeeld

            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends
            
            EngagementAgent.Instance.EndJob("uploadData");

##<a name="reporting-events"></a>Rapportage van gebeurtenissen

Er zijn drie soorten gebeurtenissen:

-   Zelfstandige gebeurtenissen
-   Sessiegebeurtenissen
-   Taak gebeurtenissen

### <a name="standalone-events"></a>Zelfstandige gebeurtenissen

#### <a name="reference"></a>Verwijzing

            void SendEvent(string name, Dictionary<object, object> extras = null)

Zelfstandige gebeurtenissen kunnen zich voordoen buiten de context van een sessie.

#### <a name="example"></a>Voorbeeld

            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Sessiegebeurtenissen

#### <a name="reference"></a>Verwijzing

            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Sessiegebeurtenissen worden meestal gebruikt om de acties uitgevoerd door een gebruiker tijdens zijn sessie.

#### <a name="example"></a>Voorbeeld

**Zonder gegevens:**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");
            
            // or
            
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**Met gegevens:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>Taak gebeurtenissen

#### <a name="reference"></a>Verwijzing

            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

Taak gebeurtenissen worden meestal gebruikt om de acties uitgevoerd door een gebruiker tijdens een taak.

#### <a name="example"></a>Voorbeeld

            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

##<a name="reporting-errors"></a>Rapportage van fouten

Er zijn drie soorten fouten:

-   Zelfstandige versie van fouten
-   Sessie fouten
-   Taak-fouten

### <a name="standalone-errors"></a>Zelfstandige versie van fouten

#### <a name="reference"></a>Verwijzing

            void SendError(string name, Dictionary<object, object> extras = null)

In tegenstelling tot sessie fouten, kunnen zich voordoen zelfstandige fouten buiten de context van een sessie.

#### <a name="example"></a>Voorbeeld

            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Sessie fouten

#### <a name="reference"></a>Verwijzing

            void SendSessionError(string name, Dictionary<object, object> extras = null)

Sessie fouten worden meestal gebruikt om de fouten die invloed hebben op de gebruiker tijdens zijn sessie.

#### <a name="example"></a>Voorbeeld

            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>Taakfouten

#### <a name="reference"></a>Verwijzing

            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Fouten kunnen zijn gerelateerd aan een lopend project in plaats van dat wordt gerelateerd aan de sessie van de huidige gebruiker.

#### <a name="example"></a>Voorbeeld

            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

##<a name="reporting-crashes"></a>Rapportage loopt

De agent biedt twee methoden te handelen loopt.

### <a name="send-an-exception"></a>Een uitzondering verzenden

#### <a name="reference"></a>Verwijzing

            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Voorbeeld

U kunt een uitzondering op elk gewenst moment verzenden door te bellen:

            EngagementAgent.Instance.SendCrash(aCatchedException);

U kunt ook een optionele parameter gebruiken om de betrokkenheid sessie op hetzelfde moment dan het verzenden van het vastlopen te beëindigen. Bel het volgende doen:

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Als u dat doet, wordt de sessie en de taken worden gesloten net na het verzenden van het vastlopen.

### <a name="send-an-unhandled-exception"></a>Een onverwerkte uitzondering verzenden

#### <a name="reference"></a>Verwijzing

            void SendCrash(Exception e)

Betrokkenheid bevat ook een methode voor het verzenden van onverwerkte uitzonderingen als u **uitgeschakeld** betrokkenheid automatische **vastlopen** rapportage hebt. Dit is met name handig in de toepassing UnhandledException gebeurtenis-handler.

Deze methode wordt **altijd** beëindigen de betrokkenheid sessie en taken na wordt genoemd.

#### <a name="example"></a>Voorbeeld

U kunt het implementeren van uw eigen handler UnhandledExceptionEventArgs. Bijvoorbeeld toevoegen de `Current_UnhandledException` methode van de `App.xaml.cs` bestand:

            // In your App.xaml.cs file
            
            // Code to execute on Unhandled Exceptions
            void Current_UnhandledException(object sender, UnhandledExceptionEventArgs e)
            {
               EngagementAgent.Instance.SendCrash(e.Exception,false);
            }

In App.xaml.cs in "Openbare App() {}" toevoegen:

            Application.Current.UnhandledException += Current_UnhandledException;

##<a name="device-id"></a>Apparaat-Id

            String EngagementAgent.Instance.GetDeviceId()

U kunt de betrokkenheid apparaat-id krijgen door de ondersteuning voor deze methode.

##<a name="extras-parameters"></a>Extra's parameters

Willekeurige gegevens kan worden gekoppeld aan een gebeurtenis, een fout, een activiteit of een taak. Deze gegevens kunnen worden onderverdeeld met een woordenlijst. Sleutels en waarden kunnen van elk type zijn.

Extra's gegevens zijn serienummer zodat als u wilt uw eigen type invoegen in extra's moet u een gegevenscontract voor dit type toevoegen.

### <a name="example"></a>Voorbeeld

Maak een nieuwe klasse 'Contactpersoon'.

            using System.Runtime.Serialization;
            
            namespace Microsoft.Azure.Engagement
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }
            
                // Properties
            
                [DataMember]
                public int Age
                {
                  get;
                  set;
                }
            
                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

Vervolgens we wordt toegevoegd een `Person` exemplaar naar een extra.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);
            
            EngagementAgent.Instance.SendEvent("Event", extras);

> [AZURE.WARNING] Als u andere soorten objecten hebt opgeslagen, zorg er dan voor dat hun methode ToString() wordt geïmplementeerd om menselijke leesbare tekenreeks als resultaat.

### <a name="limits"></a>Limieten

#### <a name="keys"></a>Toetsen

Elke sleutel in het object moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Dit betekent dat sleutels met ten minste één letter beginnen moeten, gevolgd door letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte

Extra's zijn beperkt tot **1024** tekens per gesprek.

##<a name="reporting-application-information"></a>Informatie over toepassingen melden

### <a name="reference"></a>Verwijzing

            void SendAppInfo(Dictionary<object, object> appInfos)

U kunt handmatig functie voor het bijhouden van informatie (of een andere toepassing-specifieke informatie) met de SendAppInfo() melden.

Opmerking deze gegevens stapsgewijs kan worden verzonden: alleen de meest recente waarde voor een bepaalde sleutel voor een bepaald apparaat worden bewaard. Een woordenlijst gebruiken zoals gebeurtenis extra's,\<object, object\> om te koppelen van gegevens.

### <a name="example"></a>Voorbeeld

            Dictionary<object, object> appInfo = new Dictionary<object, object>()
              {
                {"birthdate", "1983-12-07"},
                {"gender", "female"}
              };
            
            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Limieten

#### <a name="keys"></a>Toetsen

Elke sleutel in het object moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Dit betekent dat sleutels met ten minste één letter beginnen moeten, gevolgd door letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte

Toepassingsinformatie is beperkt tot **1024** tekens per gesprek.

In het vorige voorbeeld is de JSON verzonden naar de server 44 tekens bevatten:

            {"birthdate":"1983-12-07","gender":"female"}

##<a name="logging"></a>Logboekregistratie
###<a name="enable-logging"></a>Logboekregistratie inschakelen

De SDK kan worden geconfigureerd om te testen Logboeken in de IDE-console produceren.
Deze logboeken zijn niet standaard geactiveerd. U kunt u dit aanpassen door de eigenschap bijwerken `EngagementAgent.Instance.TestLogEnabled` naar een van de waarde die verkrijgbaar is bij het `EngagementTestLogLevel` opsomming, bijvoorbeeld:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();
 
