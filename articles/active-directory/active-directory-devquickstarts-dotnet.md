<properties
    pageTitle="Azure AD .NET aan de slag | Microsoft Azure"
    description="Het maken van een Windows-bureaublad .NET-toepassing die wordt geïntegreerd met Azure AD voor het aanmelden en Azure AD-oproepen beveiligde API's OAuth gebruiken."
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>Azure AD integreren in een Windows-bureaublad WPF-App

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Als u een bureaubladtoepassing ontwikkelt, kunt Azure AD u eenvoudige en duidelijke voor u om uw gebruikers met hun Active Directory-accounts te verifiëren.  Bovendien kunt uw toepassing veilig eventuele web API die zijn beveiligd met Azure AD, zoals de Office 365-API of de API Azure in beslag neemt.

Voor systeemeigen clients .NET die nodig hebt voor toegang tot beveiligde bronnen, biedt Azure AD de Active Directory-verificatie Library of ADAL.  De enige doel van ADAL in leven is vergemakkelijkt terwijl uw app toegangstokens krijgen.  U kunt zien hoe gemakkelijk het is, gaat hier we maken een toepassing voor .NET WPF-takenlijst die:

-   Haalt toegang tot tokens voor het bellen van de Azure AD Graph-API met het [OAuth 2.0 verificatie-protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Hiermee wordt gezocht in een map voor gebruikers met een opgegeven alias.
-   Positief of negatief gebruikers af.

Als u wilt de volledige werken-toepassing maakt, moet u:

2. Uw toepassing met Azure AD registreren.
3. Installeren en configureren van ADAL.
5. Gebruik ADAL tokens ophalen van Azure AD.

Om gestart, [de app-skelet downloaden](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) of [het voltooide voorbeeld](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  U moet ook een Azure AD-tenant waarin u kunt gebruikers maken en een toepassing registreren.  Als u een tenant, [leren hoe u een](active-directory-howto-tenant.md)nog geen hebt.

## <a name="1-register-the-directorysearcher-application"></a>*1. de toepassing DirectorySearcher registreren*
Als u wilt uw app om tokens inschakelen, moet u eerst registreren in uw Azure AD-tenant en deze toestemming voor toegang tot de API Azure AD Graph verlenen:

-   Meld u aan bij de Azure beheerportal
-   Klik in de navigatiebalk linkerpagina op in **Active Directory**
-   Selecteer een tenant waarin om de toepassing te registreren.
-   Klik op het tabblad **toepassingen** en klikt u op **toevoegen** in de onder-vel.
-   Volg de aanwijzingen en maak een nieuwe **Systeemeigen clienttoepassing**.
    -   De **naam** van de toepassing wordt uw toepassing voor eindgebruikers beschrijven
    -   De **Uri omleiden** is een combinatie van kleurenschema en de tekenreeks die Azure AD wordt gebruikt om terug te keren token antwoorden.  Voer van een bepaalde waarde in met uw toepassing, bijvoorbeeld `http://DirectorySearcher`.
-   Wanneer u de registratie hebt uitgevoerd, wordt AAD toegewezen uw app een unieke client-id.  U moet deze waarde in de volgende secties, zodat deze kopiëren op het tabblad **configureren** .
- Ook in het tabblad **configureren** , zoek de sectie "Machtigingen aan andere toepassingen".  Toevoegen voor de toepassing 'Azure Active Directory' en de **toegang van uw organisatie Directory** machtiging onder **Gedelegeerd machtigingen**.  Hiermee schakelt u de toepassing om query's in de grafiek-API voor gebruikers.

## <a name="2-install--configure-adal"></a>*2. installeren en configureren van ADAL*
U kunt nu dat u een toepassing in Azure AD hebt, ADAL installeren en uw identiteit-gerelateerde programmacode schrijven.  In de volgorde voor ADAL moeten kunnen communiceren met Azure AD, moet u deze voorzien van vindt u informatie over de registratie van uw app.
-   Begin door ADAL toe te voegen aan het DirectorySearcher project met de Package Manager-Console.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Open in het project DirectorySearcher `app.config`.  Vervang de waarden van de elementen in de `<appSettings>` sectie om de waarden die u in de Portal Azure invoert aan te geven.  Uw code verwijst naar deze waarden wanneer ADAL wordt gebruikt.
    -   De `ida:Tenant` is het domein van uw Azure AD-tenant, bijvoorbeeld contoso.onmicrosoft.com.
    -   De `ida:ClientId` is de clientId van de toepassing die u hebt gekopieerd in de portal.
    -   De `ida:RedirectUri` is de omleiding url die u in de portal geregistreerd.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Gebruik ADAL Tokens vanuit AAD*
Het eenvoudige principe achter ADAL is dat als uw app een toegangstoken moet, aanroept `authContext.AcquireTokenAsync(...)`, en ADAL doet de rest.  

-   In de `DirectorySearcher` project, open `MainWindow.xaml.cs` en zoek naar de `MainWindow()` methode.  De eerste stap is initialisatie van de app `AuthenticationContext` -ADAL bevindt zich primaire class.  Dit is waar u ADAL doorgeven de coördinaten deze nodig hebt om te communiceren met Azure AD en hoe deze tokens in cache.

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

- Zoek nu de `Search(...)` methode, die wordt gestart wanneer de gebruiker cliks de 'Search' naar de knop in de gebruikersinterface van de app.  Deze methode zorgt ervoor dat een GET-aanvraag tot de API Azure AD Graph op query voor gebruikers waarvan UPN met het opgegeven zoekterm begint.  Query's op de grafiek-API, moet u een access_token in opnemen, maar de `Authorization` koptekst van de aanvraag - dit is waar ADAL in wordt geleverd.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
- Wanneer uw app een token aanvraagt door te bellen `AcquireTokenAsync(...)`, ADAL wordt geprobeerd om terug te keren een token zonder dat de gebruiker wordt gevraagd om referenties.  Als ADAL vaststelt dat de gebruiker aanmelden moet bij een token ophalen, wordt het weergeven van een aanmeldingsdialoogvenster, referenties van de gebruiker verzamelen en retourneren een token na verificatie is geslaagd.  Als ADAL kan niet een token retourneren voor welke reden dan ook is, deze in de weergave van een `AdalException`.
- U ziet dat de `AuthenticationResult` object bevat een `UserInfo` object die kan worden gebruikt voor het verzamelen van gegevens u uw app moet mogelijk.  In de DirectorySearcher, `UserInfo` wordt gebruikt voor het aanpassen van de app-gebruikersinterface van de gebruikers-id.

- Wanneer de gebruiker op de knop 'Afmelden', we horen om ervoor te zorgen dat de volgende aanroep `AcquireTokenAsync(...)` aan de gebruiker gevraagd aan te melden.  Met ADAL, is dit is zo eenvoudig als het token cache wissen:

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

- Echter als de gebruiker wordt niet te klikken op de knop 'Afmelden', wordt moet voor het behoud van de sessie van de gebruiker voor de volgende keer dat ze de DirectorySearcher worden uitgevoerd.  Wanneer de app wordt gestart, kunt u controleren van ADAL token cache voor een bestaande token en bijwerken van de gebruikersinterface dienovereenkomstig gewijzigd.  In de `CheckForCachedToken()` methode, een ander gesprek tot stand brengen `AcquireTokenAsync(...)`, ditmaal geven in de `PromptBehavior.Never` parameter.  `PromptBehavior.Never`laat weten ADAL dat de gebruiker moet niet worden gevraagd voor het aanmelden en ADAL in plaats daarvan een uitzondering genereren moet wanneer het niet lukt om terug te keren een token.

```C#
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

Gefeliciteerd! U nu een .NET WPF-toepassingen met de mogelijkheid om te verifiëren van gebruikers, veilig bellen met OAuth 2.0 Web-API's hebt en basisinformatie over de gebruiker.  Als u nog niet is gedaan, is nu de tijd aan het vullen van uw tenant met enkele gebruikers.  Voer uw DirectorySearcher-app en meld u aan met een van deze gebruikers.  Zoeken naar andere gebruikers op basis van hun UPN.  De app te sluiten en opnieuw worden uitgevoerd.  Zoals u ziet hoe sessie van de gebruiker blijft behouden.  Meld u af en meld u weer aan als een andere gebruiker.

ADAL kunt u heel gemakkelijk al deze algemene identiteit functies opnemen in uw toepassing.  Dit zorgt voor alle het werk dat dirty voor u - Cachebeheer, OAuth protocolondersteuning, presenteren van de gebruiker met een aanmelding UI, vernieuwen verlopen tokens en meer.  Alles wat u moet weten is één API aanroep, `authContext.AcquireTokenAsync(...)`.

Voor een verwijzing naar de voltooide steekproef (zonder de configuratiewaarden) vindt u [hier](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  U kunt nu gaat u naar extra scenario's.  U kunt proberen:

[Een .NET Web API met Azure AD Secure >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
