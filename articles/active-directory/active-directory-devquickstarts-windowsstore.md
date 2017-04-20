<properties
    pageTitle="Azure AD Windows Store aan de slag | Microsoft Azure"
    description="Het maken van een Windows Store-toepassing die wordt geïntegreerd met Azure AD voor het aanmelden en u belt Azure AD beveiligde API's OAuth gebruiken."
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-with-a-windows-store-app"></a>Azure AD integreren met een Windows Store-App

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Als u een app voor de Windows Store ontwikkelt, kunt Azure AD u eenvoudige en duidelijke voor u om uw gebruikers met hun Active Directory-accounts te verifiëren.  Bovendien kunt uw toepassing veilig eventuele web API die zijn beveiligd met Azure AD, zoals de Office 365-API of de API Azure in beslag neemt.

Voor Windows Store-bureaublad-apps die nodig hebt voor toegang tot beveiligde bronnen, biedt Azure AD de Active Directory-verificatie Library of ADAL.  De enige doel van ADAL in leven is vergemakkelijkt terwijl uw app toegangstokens krijgen.  U kunt zien hoe gemakkelijk het is, wordt hier we een 'Directory Searcher' Windows Store app maken die:

-   Haalt toegang tot tokens voor het bellen van de Azure AD Graph-API met het [OAuth 2.0 verificatie-protocol](https://msdn.microsoft.com/library/azure/dn645545.aspx).
-   Hiermee wordt gezocht in een map voor gebruikers met een bepaald UPN.
-   Positief of negatief gebruikers af.

Als u wilt de volledige werken-toepassing maakt, moet u:

2. Uw toepassing met Azure AD registreren.
3. Installeren en configureren van ADAL.
5. Gebruik ADAL tokens ophalen van Azure AD.

Om gestart, [een skelet project downloaden](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip) of [het voltooide voorbeeld](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Elk is een oplossing Visual Studio-2015.  U moet ook een Azure AD-tenant waarin u kunt gebruikers maken en een toepassing registreren.  Als u een tenant, [leren hoe u een](active-directory-howto-tenant.md)nog geen hebt.

## <a name="1-register-the-directory-searcher-application"></a>*1. de toepassing van de Searcher Directory registreren*
Als u wilt uw app om tokens inschakelen, moet u eerst registreren in uw Azure AD-tenant en deze toestemming voor toegang tot de API Azure AD Graph verlenen:

-   Meld u aan bij de [Azure Management Portal](https://manage.windowsazure.com)
-   Klik in de navigatiebalk linkerpagina op in **Active Directory**
-   Selecteer een tenant waarin om de toepassing te registreren.
-   Klik op het tabblad **toepassingen** en klikt u op **toevoegen** in de onder-vel.
-   Volg de aanwijzingen en maak een nieuwe **Systeemeigen clienttoepassing**.
    -   De **naam** van de toepassing wordt uw toepassing voor eindgebruikers beschrijven
    -   De **Uri omleiden** is een combinatie van kleurenschema en de tekenreeks die Azure AD wordt gebruikt om terug te keren token antwoorden.  Tijdelijke voor een aanduidingswaarde invoert nu, bijvoorbeeld `http://DirectorySearcher`.  We wordt deze waarde later vervangen.
-   Wanneer u de registratie hebt uitgevoerd, wordt AAD toegewezen uw app een unieke client-id.  U moet deze waarde in de volgende secties, zodat deze kopiëren op het tabblad **configureren** .
- Ook in het tabblad **configureren** , zoek de sectie 'Machtigingen aan andere toepassingen'.  Voor de toepassing "Azure Active Directory" toevoegen de toegangsmachtiging **de map als de gebruiker die zijn aangemeld bij** onder **Gedelegeerd machtigingen**.  Hiermee schakelt u de toepassing om query's in de grafiek-API voor gebruikers.

## <a name="2-install--configure-adal"></a>*2. installeren en configureren van ADAL*
U kunt nu dat u een toepassing in Azure AD hebt, ADAL installeren en uw identiteit-gerelateerde programmacode schrijven.  In de volgorde voor ADAL moeten kunnen communiceren met Azure AD, moet u deze voorzien van vindt u informatie over de registratie van uw app.
-   Beginnen met het toevoegen van ADAL aan het DirectorySearcher project met de Package Manager-Console.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   Open in het project DirectorySearcher `MainPage.xaml.cs`.  Vervang de waarden in de `Config Values` regio's met de waarden die u in de Portal Azure hebt ingevoerd.  Uw code verwijst naar deze waarden wanneer ADAL wordt gebruikt.
    -   De `tenant` is het domein van uw Azure AD-tenant, bijvoorbeeld contoso.onmicrosoft.com.
    -   De `clientId` is de clientId van de toepassing die u hebt gekopieerd in de portal.
-   U moet nu kennismaken met de terugbellen-uri voor de Windows Store-app.  Een onderbrekingspunt instelt op deze regel in de `MainPage` methode:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- Maak de oplossing, ervoor te zorgen dat alle pakket verwijzingen worden hersteld.  Als pakketten ontbreken, van de Nuget Package Manager openen en herstellen van de pakketten.
- De app uitvoeren en kopieer reserveer de waarde van `redirectUri` wanneer het onderbrekingspunt is bereikt.  Dit moet er ongeveer zo

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

- Weer op het tabblad van het **configureren** van uw toepassing in de beheerportal Azure, kunt u de waarde van de **RedirectUri** vervangen door deze waarde.  

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Gebruik ADAL Tokens vanuit AAD*
Het eenvoudige principe achter ADAL is dat als uw app een toegangstoken moet, aanroept `authContext.AcquireToken(…)`, en ADAL doet de rest.  

-   De eerste stap is initialisatie van de app `AuthenticationContext` -ADAL bevindt zich primaire class.  Dit is waar u ADAL doorgeven de coördinaten deze nodig hebt om te communiceren met Azure AD en hoe deze tokens in cache.

```C#
public MainPage()
{
    ...

    authContext = new AuthenticationContext(authority);
}
```

- Zoek nu de `Search(...)` methode, die wordt aangeroepen wanneer de gebruiker op de knop 'Search' in de gebruikersinterface van de app.  Deze methode zorgt ervoor dat een GET-aanvraag tot de API Azure AD Graph op query voor gebruikers waarvan UPN met het opgegeven zoekterm begint.  Query's op de grafiek-API, moet u een access_token in opnemen, maar de `Authorization` koptekst van de aanvraag - dit is waar ADAL in wordt geleverd.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
        }
        return;
    }
    ...
}
```
- Wanneer uw app een token aanvraagt door te bellen `AcquireTokenAsync(...)`, ADAL wordt geprobeerd om terug te keren een token zonder dat de gebruiker wordt gevraagd om referenties.  Als ADAL vaststelt dat de gebruiker aanmelden moet bij een token ophalen, wordt deze weergeven van een aanmeldingsdialoogvenster, referenties van de gebruiker verzamelen en retourneren een token na verificatie is geslaagd.  Als ADAL kan niet een token retourneren voor welke reden dan ook, is de `AuthenticationResult` status is een fout.

- Nu is het gebruik van de de access_token die u zojuist hebt opgehaald.  Ook in de `Search(...)` methode, het token als bijlage toevoegen aan de grafiek API GET-aanvraag in de koptekst autorisatie:

```C#
// Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

```
- U kunt ook de `AuthenticationResult` object wilt weergeven van informatie over de gebruiker in uw app, zoals van de gebruikers-id:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- Tot slot kunt u ADAL aan te melden van de gebruiker uit als u ook de toepassing.  Wanneer de gebruiker op de knop 'Afmelden', we horen om ervoor te zorgen dat de volgende aanroep `AcquireTokenAsync(...)` een (+) in de weergave wordt weergegeven.  Met ADAL, is dit is zo eenvoudig als het token cache wissen:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Gefeliciteerd! U nu een Windows Store-app met de mogelijkheid om te verifiëren van gebruikers, veilig bellen met OAuth 2.0 Web-API's hebt en basisinformatie over de gebruiker.  Als u nog niet is gedaan, is nu de tijd aan uw tenant met enkele gebruikers vullen.  Voer uw DirectorySearcher-app en meld u aan met een van deze gebruikers.  Zoeken naar andere gebruikers op basis van hun UPN.  De app te sluiten en opnieuw worden uitgevoerd.  Zoals u ziet hoe sessie van de gebruiker blijft behouden.  Meld u af (via het contextmenu om weer te geven van de onderste balk) en meld u weer aan als een andere gebruiker.

ADAL kunt u heel gemakkelijk al deze algemene identiteit functies opnemen in uw toepassing.  Dit zorgt voor alle het werk dat dirty voor u - Cachebeheer, OAuth protocolondersteuning, presenteren van de gebruiker met een aanmelding UI, vernieuwen verlopen tokens en meer.  Alles wat u moet weten is één aanroep API, `authContext.AcquireToken*(…)`.

Voor een verwijzing naar de voltooide steekproef (zonder de configuratiewaarden) vindt u [hier](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  U kunt nu gaat u naar extra identiteit scenario's.  U kunt proberen:

[Een .NET Web API met Azure AD Secure >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
