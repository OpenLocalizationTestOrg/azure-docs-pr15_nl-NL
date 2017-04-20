<properties
    pageTitle="Aan de slag met verificatie in voor de Mobile-Apps in Xamarin.Forms-app | Microsoft Azure"
    description="Leer hoe u de Mobile-Apps gebruiken om gebruikers van uw app Xamarin formulieren via allerlei identiteitsprovider, inclusief AAD, Google, Facebook, Twitter en Microsoft te verifiëren."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinforms-app"></a>Verificatie toevoegen aan uw Xamarin.Forms-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

##<a name="overview"></a>Overzicht

In dit onderwerp ziet u hoe u gebruikers van een App-Service Mobile-App van uw clienttoepassing verifiëren. In deze zelfstudie kunt u verificatie toevoegen aan het Xamarin.Forms quickstart project met een identiteitsprovider die wordt ondersteund door de App-Service. Nadat wordt is geverifieerd en gemachtigd door uw Mobile-App, de gebruikers-ID-waarde wordt weergegeven en is mogelijk voor toegang tot gegevens in een beperkte tabel.

##<a name="prerequisites"></a>Vereisten voor

Voor het beste resultaat in deze zelfstudie, is het raadzaam dat u eerst de zelfstudie voor het [maken van een app Xamarin.Forms](app-service-mobile-xamarin-forms-get-started.md) worden voltooid. Nadat u deze zelfstudie hebt voltooid, hebt u een Xamarin.Forms-project dat is een app van de takenlijst van meerdere platforms.

Als u het gedownloade snel aan de slag server-project niet gebruikt, moet u het pakket van de extensie verificatie toevoegen aan uw project. Voor meer informatie over de server extensie-pakketten, raadpleegt u [werken met de .NET-endserver SDK voor Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Registreren van uw app voor verificatie en App-Services configureren

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Beperkingen instellen voor geverifieerde gebruikers

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]


##<a name="add-authentication-to-the-portable-class-library"></a>Verificatie toevoegen aan de portable klassenbibliotheek

Mobile-Apps gebruikt de methode van de extensie [LoginAsync] op de [MobileServiceClient] moeten aanmelden een gebruiker met App Service-verificatie. In dit voorbeeld wordt de stroom van een server worden beheerd verificatie die wordt weergegeven van de provider aanmeldingsproblemen interface in de app gebruikt. Zie voor meer informatie [authenticatie Server worden beheerd](app-service-mobile-dotnet-how-to-use-client-library.md#serverflow). Als u een betere ervaring in uw productie-app, kunt u overwegen in plaats daarvan met [Client beheerde verificatie](app-service-mobile-dotnet-how-to-use-client-library.md#clientflow). 

Als u wilt verifiëren met een Xamarin.Forms project, kunt u een interface **IAuthenticate** definiëren in de Portable klassenbibliotheek voor de app. U kunt ook de gebruikersinterface die is gedefinieerd in de Portable klassenbibliotheek om toe te voegen een knop **aanmelden** , waarmee de gebruiker om te beginnen verificatie bijwerken. Gegevens worden geladen na een succesvolle verificatie van de mobiele app-end.

U moet de **IAuthenticate** -interface voor elk platform worden ondersteund door uw app implementeren.


1. In Visual Studio of Xamarin Studio, open App.cs uit het project met **draagbare** in de naam, dat wil draagbare klassenbibliotheek project zeggen, voegt u het volgende `using` instructie:

        using System.Threading.Tasks;

2. In App.cs, voeg de volgende `IAuthenticate` interface definitie onmiddellijk voordat de `App` klasse definitie.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }

3. De volgende statische leden toevoegen aan de **App** class initialisatie van de interface met een bepaalde platform-implementatie.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }

4. TodoList.xaml openen vanuit het project draagbare klassenbibliotheek, voegt u het volgende element van de **knop** in het element van de indeling *buttonsPanel* na de bestaande knop: 

        <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30" 
            Clicked="loginButton_Clicked"/>

    Deze knop activeert verificatie server worden beheerd met de mobiele app-end.

5. TodoList.xaml.cs openen vanuit het project draagbare klassenbibliotheek en vervolgens toevoegen het volgende veld toe aan de `TodoList` class:

        // Track whether the user has authenticated. 
        bool authenticated = false;


6. De methode **OnAppearing** vervangen door de volgende code:

        protected override async void OnAppearing()
        {
            base.OnAppearing();
    
            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data 
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    Dit zorgt ervoor dat de gegevens alleen vernieuwd van de service nadat de gebruiker is geverifieerd.

7. De volgende handler voor de gebeurtenis **Clicked** toevoegen aan de **takenlijst van** klasse:

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }

8. Uw wijzigingen op te slaan en opnieuw opbouwen het draagbare klassenbibliotheek project bevestigt geen fouten.


##<a name="add-authentication-to-the-android-app"></a>Verificatie toevoegen aan de Android-app

Hier vindt u de interface **IAuthenticate** implementeren in het project Android-app. Dit gedeelte overslaan als Android-apparaten worden niet ondersteund.

1. Klik in Visual Studio of Xamarin Studio, met de rechtermuisknop op het **droid** project, klikt u vervolgens **als opstartproject instellen**.

2. Druk op F5 om te start van het project in Foutopsporing en controleer of dat een onverwerkte uitzondering met een statuscode van 401 (niet gemachtigd) treedt op nadat de app wordt gestart. Dit gebeurt omdat toegang op de backend beperkt tot gemachtigde gebruikers alleen is.

3. MainActivity.cs openen in de Android project en voeg de volgende `using` instructies:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Update voor de klas **MainActivity** implementatie van de interface **IAuthenticate** , als volgt:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate


5. Update voor de klas **MainActivity** door toe te voegen een veld **MobileServiceUser** en een methode **verifiëren** , die voor de interface **IAuthenticate** , als volgt vereist is:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this,
                    MobileServiceAuthenticationProvider.Facebook);
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }


    Als u een identiteitsprovider dan Facebook gebruikt, kiest u een andere waarde voor [MobileServiceAuthenticationProvider].

6. Voeg de volgende code toe met de methode **OnCreate** van de klasse **MainActivity** voordat u de oproep door naar `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    Dit zorgt ervoor dat de verificator worden geïnitialiseerd voordat de belasting voor de app.

7. Bouw die tabellen opnieuw de app, worden uitgevoerd en aanmelden met de verificatieprovider u hebt gekozen en controleert u of dat u toegang tot gegevens als een geverifieerde gebruiker zijn.

##<a name="add-authentication-to-the-ios-app"></a>Verificatie toevoegen aan de iOS-app

Hier vindt u de interface **IAuthenticate** implementeren in het project iOS-app. Dit gedeelte overslaan als iOS-apparaten worden niet ondersteund.

1. Klik in Visual Studio of Xamarin Studio, met de rechtermuisknop op het project **iOS** , klikt u vervolgens **als opstartproject instellen**.

2. Druk op F5 aan het project in foutopsporing start en controleer of dat een onverwerkte uitzondering met een statuscode van 401 (niet gemachtigd) treedt op nadat de app wordt gestart. Dit gebeurt omdat toegang op de backend beperkt tot gemachtigde gebruikers alleen is.

4. AppDelegate.cs openen in de iOS-project en voeg de volgende `using` instructies:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. Update voor de klas **AppDelegate** implementatie van de interface **IAuthenticate** , als volgt:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate

5. Update voor de klas **AppDelegate** door toe te voegen een veld **MobileServiceUser** en een methode **verifiëren** , die voor de gebruikersinterface van **IAuthenticate** als volgt vereist is:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;                        
                    }
                }        
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();         

            return success;
        }

    Als u een identiteitsprovider dan Facebook gebruikt, kiest u een andere waarde voor [MobileServiceAuthenticationProvider].

6. De volgende regel met code toevoegen aan de methode **FinishedLaunching** voordat u de oproep door naar `LoadApplication()`: 

        App.Init(this);

    Dit zorgt ervoor dat de verificator worden geïnitialiseerd voordat de app wordt geladen.

7. Bouw die tabellen opnieuw de app, worden uitgevoerd en aanmelden met de verificatieprovider u hebt gekozen en controleert u of dat u toegang tot gegevens als een geverifieerde gebruiker zijn.


##<a name="add-authentication-to-windows-app-projects"></a>Verificatie toevoegen aan projecten voor Windows-app

Hier vindt u de interface **IAuthenticate** implementeren in de Windows 8.1 en Windows Phone 8.1 app projecten. De dezelfde stappen gelden voor universele Windows-Platform (UWP) projecten. Dit gedeelte overslaan als Windows apparaten worden niet ondersteund.

1. In Visual Studio, met de rechtermuisknop op de **WinApp** of het project **WinPhone81** , klikt u vervolgens **als opstartproject instellen**.

2. Druk op F5 om te start van het project in Foutopsporing en controleer of dat een onverwerkte uitzondering met een statuscode van 401 (niet gemachtigd) treedt op nadat de app wordt gestart. Dit gebeurt omdat toegang op de backend beperkt tot gemachtigde gebruikers alleen is.

3. MainPage.xaml.cs openen voor het Windows-app-project en voeg de volgende `using` instructies:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Vervang `<your_Portable_Class_Library_namespace>` met de naamruimte voor uw draagbare klassenbibliotheek.

4. Update voor de klas **MainPage** implementatie van de interface **IAuthenticate** , als volgt:

        public sealed partial class MainPage : IAuthenticate


5. Update voor de klas **MainPage** door toe te voegen een veld **MobileServiceUser** en een methode **verifiëren** , die voor de interface **IAuthenticate** , als volgt vereist is:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }
                
            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }


    Als u een identiteitsprovider dan Facebook gebruikt, kiest u een andere waarde voor [MobileServiceAuthenticationProvider].

6. Voeg de volgende regel met code in de constructor voor de klas **MainPage** voordat u de oproep door naar `LoadApplication()`:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);
 
    Vervang `<your_Portable_Class_Library_namespace>` met de naamruimte voor uw draagbare klassenbibliotheek.  
    Als dit het project WinApp, kunt u omlaag naar stap 8 overslaan. De volgende stap geldt alleen voor het WinPhone81 project, waar u moet uitvoeren van de terugbellen login.

7. (Optioneel) In het project van de app **WinPhone81** open App.xaml.cs en voeg de volgende `using` instructies:

        using Microsoft.WindowsAzure.MobileServices;
        using <your_Portable_Class_Library_namespace>;

    Vervang `<your_Portable_Class_Library_namespace>` met de naamruimte voor uw draagbare klassenbibliotheek.

8.  De volgende **OnActivated** methode overschrijven toevoegen aan de **App** class:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            // We just need to handle activation that occurs after web authentication. 
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Get the client and call the LoginComplete method to complete authentication.
                var client = TodoItemManager.DefaultManager.CurrentClient as MobileServiceClient;
                client.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
        }

    Wanneer de methode overschrijven al bestaat, de voorwaardelijke code toe te voegen uit het bovenstaande fragment.

7. Bouw die tabellen opnieuw de app, worden uitgevoerd en aanmelden met de verificatieprovider u hebt gekozen en controleert u of dat u toegang tot gegevens als een geverifieerde gebruiker zijn.

##<a name="next-steps"></a>Volgende stappen

Nu u deze zelfstudie basisverificatie hebt uitgevoerd, kunt u doorgaat op een van de volgende zelfstudies:

+ [Push-meldingen toevoegen aan uw app](app-service-mobile-xamarin-forms-get-started-push.md)  
  Informatie over het toevoegen van push-meldingen-ondersteuning naar uw app en uw backend Mobile-App als u wilt gebruiken van Azure melding Hubs push-meldingen configureren.

+ [Offline synchronisatie voor uw app inschakelen](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Leer hoe u Offlineondersteuning uw app met een backend Mobile-App toevoegen. Offline synchronisatie kan eindgebruikers om te communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er helemaal geen netwerkverbinding.

<!-- Images. -->

<!-- URLs. -->
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
[LoginAsync]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[MobileServiceClient]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx

