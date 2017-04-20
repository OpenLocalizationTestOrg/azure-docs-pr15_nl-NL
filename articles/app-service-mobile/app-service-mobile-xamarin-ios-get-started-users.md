<properties
    pageTitle="Aan de slag met verificatie in voor de Mobile-Apps in de Xamarin voor iOS"
    description="Leer hoe u de Mobile-Apps gebruiken om gebruikers van uw Xamarin iOS-app via allerlei identiteitsprovider, inclusief AAD, Google, Facebook, Twitter en Microsoft te verifiëren."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinios-app"></a>Verificatie toevoegen aan uw Xamarin.iOS-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In dit onderwerp ziet u hoe u gebruikers van een App-Service Mobile-App van uw clienttoepassing verifiëren. In deze zelfstudie kunt u verificatie toevoegen aan het Xamarin.iOS quickstart project met een identiteitsprovider die wordt ondersteund door de App-Service. Nadat wordt is geverifieerd en gemachtigd door uw Mobile-App, de gebruikers-ID-waarde wordt weergegeven en is mogelijk voor toegang tot gegevens in een beperkte tabel.

Eerst moet u de zelfstudie [een Xamarin.iOS-app maken op]uitvoeren. Als u het gedownloade snel aan de slag server-project niet gebruikt, moet u het pakket van de extensie verificatie toevoegen aan uw project. Voor meer informatie over de server extensie-pakketten, raadpleegt u [werken met .NET endserver SDK voor Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>Registreren van uw app voor verificatie en App-Services configureren

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>Beperkingen instellen voor geverifieerde gebruikers

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. in Visual Studio of Xamarin Studio, de client-project niet uitvoeren op een apparaat of emulator. Controleer of dat een onverwerkte uitzondering met een statuscode van 401 (niet gemachtigd) treedt op nadat de app wordt gestart. De fout is aangemeld bij de console van foutopsporing. Dus Visual Studio ziet u de fout in het uitvoervenster.

&nbsp;&nbsp;Deze fout onbevoegde gebeurt omdat de app probeert te krijgen tot uw backend Mobile-App als een niet-geverifieerde gebruiker. De tabel *TodoItem* nu is verificatie vereist.

U wordt vervolgens de app client naar verzoek bronnen bijwerken van de Mobile-App-end met een geverifieerde gebruiker.

##<a name="add-authentication-to-the-app"></a>Verificatie toevoegen aan de app

In deze sectie wijzigt u de app als u wilt een aanmeldingsvenster weergegeven voordat het weergeven van gegevens in. Wanneer de app wordt gestart, wordt niet geen verbinding maken met uw App-Service en gegevens kunnen niet worden weergegeven. Nadat de eerste keer dat de gebruiker uitvoert de penbeweging vernieuwen het aanmeldingsvenster verschijnt. Nadat de succesvolle login de lijst met items die moeten worden weergegeven.

1. In het project client, opent u het bestand **QSTodoService.cs** en voeg de volgende met instructie en `MobileServiceUser` met accessor aan de klasse QSTodoService:

    ```
        using UIKit;
    ```

        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

2. Nieuwe methode met de naam **verifiëren** naar **QSTodoService** met de volgende definitie toevoegen:


        public async Task Authenticate(UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Als u een identiteitsprovider dan een Facebook gebruikt, wijzigt u de waarde doorgegeven aan **LoginAsync** boven aan een van de volgende handelingen uit: _MicrosoftAccount_, _Twitter_, _Google_of _WindowsAzureActiveDirectory_.

3. Open **QSTodoListViewController.cs**. De methodedefinitie van **ViewDidLoad** verwijderen van de oproep door naar **RefreshAsync()** aan het einde wijzigen:

        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            todoService = QSTodoService.DefaultService;
           await todoService.InitializeStoreAsync ();

           RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync ();
           }

            // Comment out the call to RefreshAsync
            // await RefreshAsync ();
        }


4. Wijzig de methode **RefreshAsync** om te verifiëren als **de eigenschap** null is. Voeg de volgende code boven aan de methodedefinitie:

        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method

5. Verbonden met uw Xamarin bouwen Host op uw Mac, de client-project hebt samengesteld een apparaat of emulator uitvoeren in Visual Studio of Xamarin Studio. Controleer of de app geen gegevens bevat.

    Voer de beweging vernieuwen trekken omlaag in de lijst met items, waardoor het aanmeldingsvenster wordt weergegeven. Zodra u geldige referenties hebt, de app wordt de lijst met items die moeten worden weergegeven en u updates kunt maken met de gegevens.


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Een Xamarin.iOS-app maken]: app-service-mobile-xamarin-ios-get-started.md
