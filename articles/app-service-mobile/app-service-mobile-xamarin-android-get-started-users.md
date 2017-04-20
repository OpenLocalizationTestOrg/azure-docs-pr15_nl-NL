<properties
    pageTitle="Aan de slag met verificatie in voor de Mobile-Apps in Xamarin Android"
    description="Leer hoe u de Mobile-Apps gebruiken om gebruikers van uw Xamarin Android-app via allerlei identiteitsprovider, inclusief AAD, Google, Facebook, Twitter en Microsoft te verifiëren."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinandroid-app"></a>Verificatie toevoegen aan uw Xamarin.Android-app

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In dit onderwerp ziet u hoe u gebruikers van een mobiele App van uw clienttoepassing verifiëren. In deze zelfstudie kunt u verificatie toevoegen aan het quickstart project met een identiteitsprovider die wordt ondersteund door Azure Mobile-Apps. Na wordt is geverifieerd en geautoriseerd in de Mobile-App, wordt de waarde van de gebruikers-ID weergegeven.

Deze zelfstudie is gebaseerd op de quickstart Mobile-App. Ook eerst moet u de zelfstudie [een Xamarin.Android-app maken op]uitvoeren. Als u het gedownloade snel aan de slag server-project niet gebruikt, moet u het pakket van de extensie verificatie toevoegen aan uw project. Voor meer informatie over de server extensie-pakketten, raadpleegt u [werken met de .NET-endserver SDK voor Azure Mobile-Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

##<a name="register"></a>Registreren van uw app voor verificatie en App-Services configureren

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Beperkingen instellen voor geverifieerde gebruikers

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Klik in Visual Studio of Xamarin Studio, het client-project niet uitvoeren op een apparaat of emulator. Controleer of dat een onverwerkte uitzondering met een statuscode van 401 (niet gemachtigd) treedt op nadat de app wordt gestart. Dit gebeurt omdat de app probeert te krijgen tot uw backend Mobile-App als een niet-geverifieerde gebruiker. De tabel *TodoItem* nu is verificatie vereist.

U wordt vervolgens de app client naar verzoek bronnen bijwerken van de Mobile-App-end met een geverifieerde gebruiker.

##<a name="add-authentication"></a>Verificatie toevoegen aan de app

De app wordt bijgewerkt als gebruikers verplicht moeten Tik op de knop **aanmelden** en worden geverifieerd voordat gegevens worden weergegeven.

1. De volgende code hebt toegevoegd aan de klasse **TodoActivity** :

        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook);
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");

                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }

        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;

                // Load the data.
                OnRefreshItemsSelected();
            }
        }

    Hiermee maakt u een nieuwe methode voor het verifiëren van een gebruiker en een handler methode voor een nieuwe knop voor het **aanmelden** . De gebruiker in het bovenstaande voorbeeldcode wordt geverifieerd met behulp van een Facebook-aanmelding. Een dialoogvenster wordt gebruikt voor het weergeven van de gebruikers-ID wanneer deze is geverifieerd.

    > [AZURE.NOTE] Als u een identiteitsprovider dan Facebook gebruikt, wijzigt u de waarde doorgegeven aan **LoginAsync** boven aan een van de volgende handelingen uit: _MicrosoftAccount_, _Twitter_, _Google_of _WindowsAzureActiveDirectory_.

3. In de methode **OnCreate** , verwijderen of opmerking bij de volgende regel met code:

        OnRefreshItemsSelected ();

4. Klik in het document Activity_To_Do.axml toevoegen de volgende definitie van de knop *LoginUser* voordat u de bestaande *AddItem* knop:

        <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />

5. Het volgende element toevoegen aan het Strings.xml resources-bestand:

        <string name="login_button_text">Sign in</string>

6. De client-project niet uitvoeren op een apparaat of emulator in Visual Studio of Xamarin Studio, en meld u aan met uw door u gekozen identiteitsprovider.

    Wanneer u zich heeft aangemeld-in, de app uw aanmeldings-ID en de lijst met items die moeten worden weergegeven en u updates kunt maken met de gegevens.


<!-- URLs. -->
[Een Xamarin.Android-app maken]: app-service-mobile-xamarin-android-get-started.md
