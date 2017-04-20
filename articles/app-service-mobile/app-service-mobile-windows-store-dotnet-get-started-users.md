<properties
    pageTitle="Verificatie toevoegen aan uw app universele Windows-Platform (UWP) | Azure mobiele Apps"
    description="Informatie over het gebruik van Azure App Service Mobile-Apps om gebruikers van uw universele Windows-Platform (UWP)-app met een verscheidenheid aan identiteitsprovider, inclusief te verifiëren: AAD, Google, Facebook, Twitter en Microsoft."
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-windows-app"></a>Verificatie toevoegen aan uw Windows-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In dit onderwerp ziet u hoe cloud gebaseerde verificatie toevoegen aan uw mobiele app. In deze zelfstudie toevoegen u verificatie aan het universele Windows-Platform (UWP) quickstart-project voor Mobile-Apps met een identiteitsprovider die wordt ondersteund door Azure App-Service. Na wordt is geverifieerd en gemachtigd door uw backend Mobile-App, wordt de waarde van de gebruikers-ID weergegeven.

Deze zelfstudie is gebaseerd op de quickstart Mobile-Apps. Eerst moet u de zelfstudie [aan de slag met Mobile-Apps](app-service-mobile-windows-store-dotnet-get-started.md)uitvoeren.

##<a name="register"></a>Registreren van uw app voor verificatie en de App-Service configureren

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Beperkingen instellen voor geverifieerde gebruikers

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

U kunt nu verifiëren dat anonieme toegang tot uw back-end is uitgeschakeld. Met de app UWP project instellen als het opstartproject, implementeren en uitvoeren van de app; Controleer of dat een onverwerkte uitzondering met een statuscode van 401 (niet gemachtigd) treedt op nadat de app wordt gestart. Dit gebeurt omdat de app probeert te krijgen tot uw mobiele App-Code als niet-geverifieerde gebruiker, maar de tabel *TodoItem* nu is verificatie vereist.

Vervolgens wordt u de app om gebruikers te verifiëren voordat het aanvragen van resources uit uw App-Service bijwerken.

##<a name="add-authentication"></a>Verificatie toevoegen aan de app

1. In de UWP app projectbestand MainPage.cs en het volgende codefragment toevoegen aan de klasse MainPage:
    
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;

        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now signed in - {0}", user.UserId);

                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }

    Deze code wordt geverifieerd door de gebruiker met een Facebook-account. Als u een identiteitsprovider dan Facebook gebruikt, wijzigt u de waarde van **MobileServiceAuthenticationProvider** boven aan de waarde voor uw provider.

3. Opmerking-out of de oproep door naar de methode **ButtonRefresh_Click** (of de methode **InitLocalStoreAsync** ) in de bestaande **OnNavigatedTo** methode overschrijven verwijderen. Hiermee voorkomt u dat de gegevens worden geladen voordat de gebruiker is geverifieerd. Vervolgens gaat toevoegen u een knop **aanmelden** bij de app die verificatie activeert.

4. Het volgende codefragment toevoegen aan de klasse MainPage:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Open het projectbestand MainPage.xaml, zoek naar het element waarin de knop **Opslaan** en vervang deze door de volgende code:

        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>

9. Druk op F5 om te de app uitvoeren, klikt u op de knop **aanmelden** en meld u aan bij de app met de door u gekozen identiteitsprovider. Nadat uw aanmeldingsproblemen gelukt is, wordt de app wordt uitgevoerd zonder fouten en kunt u uw back-end query en brengt u updates aan gegevens.


##<a name="tokens"></a>Store het verificatietoken op de client

Het vorige voorbeeld blijkt een standaard-aanmelden, waarvoor de client contact opnemen met de identiteitsprovider zowel de App-Service elke keer dat de app wordt gestart. Niet alleen niet deze methode efficiënt verloopt, u kunt uitvoeren in gebruik-relateert problemen moeten veel klanten probeert te starten u app op hetzelfde moment. Een betere benadering is het Autorisatietoken die het resultaat van uw App-Service in cache en probeert u het gebruik van deze eerste vóór het gebruik van een provider gebaseerde aanmelden.

>[AZURE.NOTE]U kunt het token uitgegeven door de App-Services, ongeacht of u van client beheerde of service-beheerd verificatie gebruikmaakt cache. In deze zelfstudie wordt verificatie service worden beheerd.

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Volgende stappen

Nu u deze zelfstudie basisverificatie hebt uitgevoerd, kunt u doorgaat op een van de volgende zelfstudies:

+ [Push-meldingen toevoegen aan uw app](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Informatie over het toevoegen van push-meldingen-ondersteuning naar uw app en uw backend Mobile-App als u wilt gebruiken van Azure melding Hubs push-meldingen configureren.

+ [Offline synchronisatie voor uw app inschakelen](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Leer hoe u Offlineondersteuning uw app met een backend Mobile-App toevoegen. Offline synchronisatie kan eindgebruikers om te communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs als er helemaal geen netwerkverbinding.


<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md

