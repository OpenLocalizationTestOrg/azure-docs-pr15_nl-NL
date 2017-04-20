<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure"
    description="Het maken van een Windows-bureaubladtoepassing hebt met aanmelden, aanmelding, en het beheer van gebruikersprofielen met behulp van Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure AD B2C: Een Windows-bureaublad-app maken

Met behulp van Azure Active Directory (Azure AD) B2C, kunt u krachtige selfservice-identiteit beheerfuncties toevoegen aan uw bureaublad-app in een paar korte stappen. In dit artikel wordt uitgelegd hoe u om een .NET Windows Presentation Foundation (WPF) "takenlijst"-toepassing waarin gebruiker aanmelding, aanmelden en beheer van gebruikersprofielen te maken. De app biedt ondersteuning voor aanmelding en aanmelden met behulp van een gebruikersnaam in te voeren of e-mailbericht. Dit wordt ook ondersteuning voor aanmelding en aanmelden met behulp van de accounts van sociale zoals Facebook en Google bevatten.

## <a name="get-an-azure-ad-b2c-directory"></a>Een map Azure AD B2C ophalen

Voordat u Azure AD B2C gebruiken kunt, moet u een map maken of tenant.  Een map is een container voor al uw gebruikers, apps, groepen en meer. Als u niet een al hebt, [Maak een map B2C](active-directory-b2c-get-started.md) voordat u verder in deze handleiding.

## <a name="create-an-application"></a>Een toepassing maken

Vervolgens moet u een app maken in uw adreslijst B2C. Het resultaat is Azure AD-gegevens die zijn vereist voor het veilig communiceren met uw app. Volg [deze instructies](active-directory-b2c-app-registration.md)om een app maken.  Zorg ervoor dat:

- Neem een **native client** in de toepassing.
- Kopieer de **URI omleiden** `urn:ietf:wg:oauth:2.0:oob`. De standaard-URL voor dit voorbeeld is.
- Kopieer de **Toepassings-ID** die is toegewezen aan uw app. Moet u deze later.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Maak uw apparaten

In Azure AD B2C, wordt elke gebruikerservaring gedefinieerd door een [beleid](active-directory-b2c-reference-policies.md). In dit voorbeeld bevat drie identiteit ervaringen: registreren, aanmelden en profiel bewerken. U moet maken van een beleid voor elk type, zoals beschreven in de [naslagartikel beleid](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy). Wanneer u de drie beleidsregels maakt, moet u naar:

- Kies **Gebruikers-ID aanmelden** of **registreren E-mail** in het blad identiteit-providers.
- Kies **naam weer te geven** en andere aanmelding kenmerken in uw aanmelding beleid.
- Kies **weergavenaam** en **Object-ID** claims als toepassing vorderingen die voortvloeien uit elk beleid. U kunt ook andere claims.
- De **naam** van elke beleid kopiëren nadat u deze hebt gemaakt. Er is het voorvoegsel `b2c_1_`.  U hebt de volgende namen beleid later nodig.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nadat u de drie beleidsregels hebt gemaakt, kunt u bent uw app maken.

## <a name="download-the-code"></a>De code downloaden

De code voor deze zelfstudie [GitHub worden bijgehouden](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet). Maak de steekproef terwijl u door kunt u [een skelet project een ZIP-bestand downloaden](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). U kunt ook de skelet klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

De voltooide app is ook [beschikbaar als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) of op de `complete` tak van de dezelfde opslagplaats.

Nadat u de voorbeeldcode hebt gedownload, opent u het bestand Visual Studio .sln aan de slag. De `TaskClient` project wordt de WPF-bureaubladtoepassing hebt waarin de gebruiker interactie met. Deze oproepen voor de toepassing van deze zelfstudie wordt een back-enddatabase taak web API, die wordt gehost in Azure wordt aangegeven, die worden opgeslagen van elke gebruiker takenlijst te klikken.  U hoeft niet te maken van het web API, we dat al wel hebt uitgevoerd voor u.

Meer informatie over hoe een web API veilig wordt geverifieerd aanvragen met behulp van Azure AD B2C, raadpleegt u de [web API aan de slag artikel](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="execute-policies"></a>Beleidsregels uitvoeren
Uw app communiceert met Azure AD B2C door te sturen verificatieberichten die het beleid dat ze wilt uitvoeren als onderdeel van de HTTP-aanvraag opgeeft. Voor .NET-bureaubladtoepassingen, kunt u de Preview-versie van Microsoft verificatie bibliotheek (MSAL) gebruiken voor het verzenden van berichten van OAuth 2.0-verificatie, beleidsregels uitvoeren, en vind van tokens die web API's bellen.

### <a name="install-msal"></a>MSAL installeren
Toevoegen MSAL naar de `TaskClient` project met behulp van de Visual Studio Package Manager-Console.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Geef de details van uw B2C
Open het bestand `Globals.cs` en elk van de eigenschapswaarden vervangen door uw eigen. Deze klasse wordt gebruikt in de gehele `TaskClient` verwijzing veelgebruikte waarden.

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]


### <a name="create-the-publicclientapplication"></a>De PublicClientApplication maken
De primaire klasse van MSAL is `PublicClientApplication`. Deze klasse vertegenwoordigt uw toepassing in het systeem Azure AD B2C. Bij een exemplaar van het maken van de app-initalizes `PublicClientApplication` in `MainWindow.xaml.cs`. Dit kan worden gebruikt in het venster.

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app, 
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };
    
    ...
```

### <a name="initiate-a-sign-up-flow"></a>Een aanmelding stroom initiëren
Wanneer een gebruiker naar positief of negatief omhoog kiest, die u wilt starten een aanmelding stroom die gebruikmaakt van het registreren beleid die u hebt gemaakt. Met behulp van MSAL die u zojuist belt `pca.AcquireTokenAsync(...)`. De parameters die u doorgeven aan `AcquireTokenAsync(...)` bepalen welke token u ontvangt, het beleid gebruikt in het verificatieverzoek en meer.

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user 
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }

            MessageBox.Show(message);
        }

        return;
    }
}
```

### <a name="initiate-a-sign-in-flow"></a>Een stroom aanmeldingsproblemen initiëren
U kunt een stroom aanmeldingsproblemen starten op dezelfde manier dat u een aanmelding stroom tot stand brengen. Wanneer een gebruiker zich aanmeldt, zodat u dezelfde aanroep van MSAL, deze tijd met behulp van uw beleid aanmelden:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>De stroom van een profiel bewerken initiëren
Klik nogmaals kunt u een beleid profiel bewerken op dezelfde manier uitvoeren:

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

In alle gevallen, geeft MSAL ofwel een token in `AuthenticationResult` of een uitzondering. Telkens wanneer u een token uit MSAL ophalen, kunt u de `AuthenticationResult.User` object naar de gebruikersgegevens in de app, zoals de gebruikersinterface bijwerken. ADAL slaat ook het token voor gebruik in andere delen van de toepassing.


### <a name="check-for-tokens-on-app-start"></a>Controleren op tokens op app starten
U kunt ook MSAL gebruiken om aanmeldingsproblemen status van de gebruiker bij te houden.  In deze app willen we de gebruiker moet aangemeld blijven, zelfs nadat ze de app sluiten en opnieuw openen.  Terug in de `OnInitialized` overschrijven, voert u de MSAL `AcquireTokenSilent` methode om te controleren: in cache opgeslagen tokens:

```C#
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
    return;
}
```

## <a name="call-the-task-api"></a>De taak API bellen
U hebt nu MSAL gebruikt beleidsregels uitvoeren en tokens kunt ophalen.  Als u een deze tokens gebruiken om te bellen van de taak API wilt, kunt u opnieuw gebruiken van MSAL `AcquireTokenSilent` methode om te controleren: in cache opgeslagen tokens:

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
        }
        else
        {
            // An unexpected error occurred.
            string message = ex.Message;
            if (ex.InnerException != null)
            {
                message += "Inner Exception : " + ex.InnerException.Message;
            }
            MessageBox.Show(message);
        }

        return;
    }
    ...
```

Wanneer de oproep door naar `AcquireTokenSilentAsync(...)` is uitgevoerd en een token is gevonden in de cache, kunt u het token voor toevoegen de `Authorization` koptekst van de HTTP-aanvraag. De taak web API wordt deze koptekst gebruiken om het verzoek om te lezen takenlijst van de gebruiker te verifiëren:

```C#
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a>De gebruiker zich afmeldt
Tot slot kunt u MSAL sessie van een gebruiker met de app beëindigen wanneer de gebruiker selecteert **Afmelden**.  Wanneer u een MSAL gebruikt, is dit doen door te schakelen alle de tokens uit de cache voor token:

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Tot slot maken en uitvoeren van de steekproef.  Zich registreren voor de app met behulp van de naam van een e-mailadres of de gebruiker. Meld u af en meld u weer aan als de gebruiker. Van die gebruiker profiel bewerken. Meld u af en meld u aan met behulp van een andere gebruiker.

## <a name="add-social-idps"></a>Sociale IDPs toevoegen

De app ondersteunt momenteel alleen gebruiker aanmelding en de aanmeldingsopties die gebruikmaken van **lokale accounts**. Dit zijn opgeslagen in de map B2C accounts met een gebruikersnaam en wachtwoord. U kunt met behulp van Azure AD B2C ondersteuning voor andere identiteitsproviders (IDPs) toevoegen zonder een van uw code.

Om toe te voegen sociale IDPs naar uw app, gaat u eerst de gedetailleerde instructies in deze artikelen te volgen. Voor elke IDP moeten worden ondersteund, moet u een toepassing in dat systeem registreren en verkrijgen van een client-ID.

- [Facebook instellen als een IDP](active-directory-b2c-setup-fb-app.md)
- [Google instellen als een IDP](active-directory-b2c-setup-goog-app.md)
- [Amazon instellen als een IDP](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn instelt als een IDP](active-directory-b2c-setup-li-app.md)

Nadat u de identiteitsprovider aan uw adreslijst B2C toevoegt, moet u elk van uw drie beleid voor het opnemen van de nieuwe IDPs, bewerken, zoals beschreven in de [naslagartikel beleid](active-directory-b2c-reference-policies.md). Nadat u uw beleid opslaat, voert u de app opnieuw uit. Ziet u de nieuwe IDPs toegevoegd als aanmelden en aanmeldingsopties in elk van uw identiteit ervaringen.

U kunt experimenteren met uw beleid en de effecten voor uw app steekproef toekijken. Toevoegen of verwijderen van IDPs, manipuleren van toepassing op claims of aanmelding kenmerken wijzigen. Experimenteren totdat u kunt zien hoe beleidsregels, verificatieaanvragen en MSAL met elkaar verbinden.

Voor verwijzing, in de voltooide voorbeeld [wordt weergegeven als een ZIP-bestand](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). U kunt dit ook klonen van GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
