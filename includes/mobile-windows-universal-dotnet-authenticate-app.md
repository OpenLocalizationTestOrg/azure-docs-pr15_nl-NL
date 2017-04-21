
1. Open het gedeelde projectbestand MainPage.cs en het volgende codefragment toevoegen aan de klasse MainPage:
    
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

3. Opmerking-out of de oproep door naar de methode **RefreshTodoItems** in de bestaande **OnNavigatedTo** methode overschrijven verwijderen.

    Hiermee voorkomt u dat de gegevens worden geladen voordat de gebruiker is geverifieerd. Vervolgens gaat toevoegen u een knop **aanmelden** bij de app die verificatie activeert.

4. Het volgende codefragment toevoegen aan de klasse MainPage:

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Hide the login button and load items from the mobile app.
                ButtonLogin.Visibility = Windows.UI.Xaml.Visibility.Collapsed;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. Klik in de Windows Store-app project het projectbestand MainPage.xaml openen en voegt u het volgende element van de **knop** vlak voor het element waarin de knop **Opslaan** :

        <Button Name="ButtonLogin" Click="ButtonLogin_Click" 
                        Visibility="Visible">Sign in</Button>

6. In het Windows Phone Store-app-project, voegt u het volgende element van de **knop** in de **ContentPanel**, na het element **tekstvak** :

        <Button Grid.Row ="1" Grid.Column="1" Name="ButtonLogin" Click="ButtonLogin_Click" 
            Margin="10, 0, 0, 0" Visibility="Visible">Sign in</Button>

8. Open het gedeelde App.xaml.cs project-bestand en voeg de volgende code:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            // Windows Phone 8.1 requires you to handle the respose from the WebAuthenticationBroker.
            #if WINDOWS_PHONE_APP
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Completes the sign-in process started by LoginAsync.
                // Change 'MobileService' to the name of your MobileServiceClient instance. 
                App.MobileService.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
            #endif

            base.OnActivated(args);
        }

    Als de methode **OnActivated** al bestaat, toe te voegen de `#if...#endif` codeblok.

9. Druk op F5 om te de Windows Store-app uitvoeren, klikt u op de knop **aanmelden** en meld u aan bij de app met de door u gekozen identiteitsprovider. 

    Wanneer u zich heeft aangemeld-in, de app moet worden uitgevoerd zonder fouten en u moet kunnen uw backend query en updates in de gegevens.

10. Met de rechtermuisknop op de Windows Phone Store-app-project, klikt u op **instellen als opstartproject**en Herhaal de vorige stap om te bevestigen dat de Windows Phone Store-app ook goed werkt.  

 