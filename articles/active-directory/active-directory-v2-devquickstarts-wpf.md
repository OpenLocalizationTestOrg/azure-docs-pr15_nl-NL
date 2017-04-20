<properties
pageTitle="Azure Active Directory v2.0 .NET systeemeigen App | Microsoft Azure"
description="Het maken van een systeemeigen .NET-app die zich gebruikers met beide persoonlijk Microsoft-Account en accounts voor werk- of schoolaccount."
services="active-directory"
documentationCenter=""
authors="dstrockis"
manager="mbaldwin"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="dotnet"
ms.topic="article"
ms.date="07/30/2016"
ms.author="dastrock; vittorib"/>

# <a name="add-sign-in-to-a-windows-desktop-app"></a>Aanmelden bij een Windows-bureaublad-app toevoegen

Met de het eindpunt v2.0, kunt u snel verificatie naar uw bureaublad-apps met ondersteuning voor beide persoonlijk Microsoft-accounts en werk- of schoolaccount accounts toevoegen.  Bovendien kunt uw app veilig communiceren met een back-end-web api, evenals [De Microsoft Graph](https://graph.microsoft.io) en een paar van de [Office 365 Unified API](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2).

> [AZURE.NOTE] Niet alle Azure Active Directory (AD) scenario's en functies worden ondersteund door het eindpunt v2.0.  Als u wilt bepalen als u het eindpunt v2.0 moet gebruiken, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

Voor [systeemeigen .NET-apps die worden uitgevoerd op een apparaat](active-directory-v2-flows.md#mobile-and-native-apps)biedt Azure AD de Microsoft identiteit verificatie bibliotheek of MSAL.  De enige doel van MSAL in leven is vergemakkelijkt terwijl uw app tokens krijgen voor het bellen van webservices.  U kunt zien hoe gemakkelijk het is, wordt hier we een .NET WPF-takenlijst app maken die:

- De gebruiker zich aanmeldt en haalt toegang tot tokens met het [OAuth 2.0 verificatie-protocol](active-directory-v2-protocols.md#oauth2-authorization-code-flow).
- Veilig roept een backend takenlijst webservice die ook door OAuth 2.0 is beveiligd.
- De gebruiker zich af.

## <a name="download-sample-code"></a>Voorbeeld van een code downloaden

De code voor deze zelfstudie worden [GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet)bijgehouden.  Als u wilt volgen, kunt u [downloaden van de app skelet als een ZIP-](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) of het skelet klonen:

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

De voltooide app wordt aangeboden aan het einde van deze zelfstudie ook.

## <a name="register-an-app"></a>Een app hebt geregistreerd
Een nieuwe app maken op [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)of Volg deze [gedetailleerde stappen](active-directory-v2-app-registration.md).  Zorg ervoor dat:

- Kopiëren naar beneden de **Toepassings-Id** toegewezen aan uw app, moet u deze snel.
- De **Mobile** -platform voor de app toevoegen.

## <a name="install--configure-msal"></a>Installeren en configureren van MSAL
Nu dat u een app geregistreerd bij Microsoft hebt, kunt u MSAL installeren en uw identiteit-gerelateerde programmacode schrijven.  In de volgorde voor MSAL moeten kunnen het eindpunt v2.0 communiceren, moet u deze voorzien van vindt u informatie over de registratie van uw app.

-   Begin door MSAL toe te voegen aan het TodoListClient project met de Package Manager-Console.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

-   Open in het project TodoListClient `app.config`.  Vervang de waarden van de elementen in de `<appSettings>` sectie om de waarden die u in de portal van de registratie van de app invoert aan te geven.  Uw code verwijst naar deze waarden wanneer MSAL wordt gebruikt.
    -   De `ida:ClientId` is de **Toepassings-Id** van de app die u hebt gekopieerd in de portal.

- Open in het project takenlijst van-Service, `web.config` in de hoofdmap van het project.  
    - Vervang de `ida:Audience` waarde met dezelfde **Toepassings-Id** van de portal.

## <a name="use-msal-to-get-tokens"></a>MSAL gebruiken om te krijgen van tokens
Het eenvoudige principe achter MSAL is dat als uw app een toegangstoken moet, u gewoon belt `app.AcquireToken(...)`, en MSAL doet de rest.  

-   In de `TodoListClient` project, open `MainWindow.xaml.cs` en zoek naar de `OnInitialized(...)` methode.  De eerste stap is initialisatie van de app `PublicClientApplication` -MSAL van primaire class dat staat voor systeemeigen toepassingen.  Dit is waar u MSAL de coördinaten die deze nodig hebt om te communiceren met Azure AD en hoe deze tokens in cache doorgeven.

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

- Wanneer u de app start, die we wilt controleren en Zie als de gebruiker al is aangemeld bij de app.  Echter we niet wilt dat alleen nog een UI-aanmelding roepen - we de gebruiker op 'aanmelden"hiervoor maken.  Ook in de `OnInitialized(...)` methode:

```C#
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.
    
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

- Als de gebruiker is niet aangemeld en ze op de knop 'Aanmelden', die we wilt aanroepen van een UI-aanmelding en de gebruiker die hun referenties invoeren.  De knop aanmelden handler implementeren:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
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


}
```

- Als de gebruiker is positief of negatief-in, MSAL wordt ontvangen en een token voor u in de cache en kunt u doorgaan met bellen de `GetTodoList()` methode probleemloos.  Enige dat er nog ophalen van een gebruiker taken willen implementeren, is de `GetTodoList()` methode.

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If the user clicked the 'clear cache' button,
        // clear the MSAL token cache and show the user as signed out.
        // It's also necessary to clear the cookies from the browser
        // control so the next user has a chance to sign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a>Uitvoeren

Gefeliciteerd! U hebt nu een .NET WPF-app voor het werken met de mogelijkheid om te verifiëren gebruikers & veilig bellen Web API's met OAuth 2.0.  Voer uw beide projecten en meld u aan met een persoonlijk Microsoft-account of een werk- of schoolaccount-account.  Voeg taken toe aan de takenlijst van die gebruiker.  Meld u af en meld u weer aan als een andere gebruiker om weer te geven hun takenlijst te klikken.  De app te sluiten en opnieuw worden uitgevoerd.  Zoals u ziet hoe sessie van de gebruiker blijft intact - komt dit doordat de app in de cache opgeslagen tokens in een lokaal bestand.

MSAL kunt u heel gemakkelijk algemene identiteit functies opnemen in uw app via zowel privé- en -accounts.  Dit zorgt voor alle het werk dat dirty voor u - Cachebeheer, OAuth protocolondersteuning, presenteren van de gebruiker met een aanmelding UI, vernieuwen verlopen tokens en meer.  Alles wat u moet weten is één API aanroep, `app.AcquireTokenAsync(...)`.

Voor een verwijzing naar kunt de voltooide steekproef (zonder de configuratiewaarden) [wordt weergegeven als een .zip hier](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip)of u klonen deze uit GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>Volgende stappen

U kunt nu verplaatsen naar meer geavanceerde onderwerpen.  U kunt proberen:

- [De TodoListService Web API met het eindpunt v2.0 beveiligen](active-directory-v2-devquickstarts-dotnet-api.md)

Raadpleeg voor meer bronnen:  

- [De handleiding voor ontwikkelaars van v2.0 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "msal" tag >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>Beveiligingsupdates voor onze producten

We raden u aan meldingen ontvangen van wanneer-incidenten plaatsgevonden door te bezoeken van [deze pagina](https://technet.microsoft.com/security/dd252948) en abonneren op advies beveiligingsmeldingen.
