<properties
    pageTitle="Azure AD Xamarin aan de slag | Microsoft Azure"
    description="Het maken van een Xamarin-toepassing die wordt geïntegreerd met Azure AD voor het aanmelden en u belt Azure AD beveiligde API's OAuth gebruiken."
    services="active-directory"
    documentationCenter="xamarin"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-xamarin-app"></a>Azure AD integreren in een Xamarin-App

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Xamarin kunt u mobiele apps schrijven in C# die kunnen worden uitgevoerd op iOS, Android en Windows (mobiele apparaten en pc's). Als u een app met Xamarin maakt, kunt Azure AD u eenvoudig en ingewikkelde voor u om uw gebruikers met hun Active Directory-accounts te verifiëren. Bovendien kunt uw toepassing veilig eventuele web API die zijn beveiligd met Azure AD, zoals de Office 365-API of de API Azure in beslag neemt.

Azure AD biedt Xamarin-apps die nodig hebt voor toegang tot beveiligde bronnen voor de bibliotheek van Active Directory-verificatie of ADAL. De enige doel van ADAL in leven is vergemakkelijkt terwijl uw app toegangstokens krijgen. U kunt zien hoe gemakkelijk het is, wordt hier we een "Directory Searcher" app maken die:

-   Wordt uitgevoerd op iOS, Android bureaublad van Windows, Windows Phone en Windows Store.
- Een enkel draagbare klassenbibliotheek (PCL) gebruikt voor de verificatie van gebruikers en tokens krijgen voor de grafiek van Azure AD-API
-   Hiermee wordt gezocht in een map voor gebruikers met een bepaald UPN.

Als u wilt de volledige werken-toepassing maakt, moet u:

2. Uw ontwikkelomgeving Xamarin instellen
2. Uw toepassing met Azure AD registreren.
3. Installeren en configureren van ADAL.
5. Gebruik ADAL tokens ophalen van Azure AD.

Om gestart, [een skelet project downloaden](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip) of [het voltooide voorbeeld](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Elk is een oplossing Visual Studio-2013. U moet ook een Azure AD-tenant waarin u kunt gebruikers maken en een toepassing registreren. Als u een tenant, [leren hoe u een](active-directory-howto-tenant.md)nog geen hebt.

## <a name="0-set-up-your-xamarin-development-environment"></a>*0. uw ontwikkelomgeving Xamarin instellen*
Omdat deze zelfstudie projecten voor iOS, Android en Windows bevat, moet u Visual Studio en Xamarin samen. Als u wilt de benodigde omgeving hebt gemaakt, de instructies voltooid op [Setup en installeer voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) op MSDN. Deze instructies zijn materiaal dat u bekijken kunt voor meer informatie over Xamarin terwijl u wacht voor het installatieprogramma's om te voltooien. 

Wanneer u de benodigde setup hebt voltooid, opent u de oplossing in Visual Studio aan de slag. U vindt u zes projecten: vijf platform-specifieke projecten en één draagbare klassenbibliotheek die voor alle platforms, worden gedeeld`DirectorySearcher.cs`

## <a name="1-register-the-directory-searcher-application"></a>*1. de toepassing van de Searcher Directory registreren*
Als u wilt uw app om tokens inschakelen, moet u eerst registreren in uw Azure AD-tenant en deze toestemming voor toegang tot de API Azure AD Graph verlenen:

-   Meld u aan bij de [Azure Management Portal](https://manage.windowsazure.com)
-   Klik in de navigatiebalk linkerpagina op in **Active Directory**
-   Selecteer een tenant waarin om de toepassing te registreren.
-   Klik op het tabblad **toepassingen** en klikt u op **toevoegen** in de onder-vel.
-   Volg de aanwijzingen en maak een nieuwe **Systeemeigen clienttoepassing**.
    -   De **naam** van de toepassing wordt uw toepassing voor eindgebruikers beschrijven
    -   De **Uri omleiden** is een combinatie van kleurenschema en de tekenreeks die Azure AD wordt gebruikt om terug te keren token antwoorden. Voer een waarde, bijvoorbeeld `http://DirectorySearcher`.
-   Wanneer u de registratie hebt uitgevoerd, wordt AAD toegewezen uw app een unieke client-id. U moet deze waarde in de volgende secties, zodat deze kopiëren op het tabblad **configureren** .
- Ook in het tabblad **configureren** , zoek de sectie "Machtigingen aan andere toepassingen". Toevoegen voor de toepassing 'Azure Active Directory' en de **toegang van uw organisatie Directory** machtiging onder **Gedelegeerd machtigingen**. Hiermee schakelt u de toepassing om query's in de grafiek-API voor gebruikers.

## <a name="2-install--configure-adal"></a>*2. installeren en configureren van ADAL*
U kunt nu dat u een toepassing in Azure AD hebt, ADAL installeren en uw identiteit-gerelateerde programmacode schrijven. In de volgorde voor ADAL moeten kunnen communiceren met Azure AD, moet u deze voorzien van vindt u informatie over de registratie van uw app.
-   Beginnen met het toevoegen van ADAL aan elk van de projecten in de oplossing met de Package Manager-Console.

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
`

- U moet zoals u ziet dat de twee verwijzingen wordt gemaakt bibliotheek worden toegevoegd aan elk project - het gedeelte PCL van ADAL en een gedeelte platform / regiospecifieke.

-   Open in het project DirectorySearcherLib `DirectorySearcher.cs`. Wijzig de waarden voor het lid van class zodat de waarden die u in de Portal Azure hebt ingevoerd. Uw code verwijst naar deze waarden wanneer ADAL wordt gebruikt.
    -   De `tenant` is het domein van uw Azure AD-tenant, bijvoorbeeld contoso.onmicrosoft.com.
    -   De `clientId` is de clientId van de toepassing die u hebt gekopieerd in de portal.
    - De `returnUri` is de redirectUri die u hebt ingevoerd in de portal, bijvoorbeeld `http://DirectorySearcher`.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Gebruik ADAL Tokens vanuit AAD*
*Almost* alle van de app verificatie logica ligt in `DirectorySearcher.SearchByAlias(...)`. In de projecten platform / regiospecifieke hoeft is om door te geven van een contextuele parameter voor de `DirectorySearcher` PCL.

- Open eerst `DirectorySearcher.cs` en toevoegen van een nieuwe parameter voor de `SearchByAlias(...)` methode. `IPlatformParameters`de contextuele parameter weer die omvat de platform-specifieke objecten die ADAL nodig zijn voor verificatie is.

```C#
public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
{
```

-   Vervolgens geïnitialiseerd de `AuthenticationContext` -ADAL bevindt zich primaire class. Dit is waar u ADAL doorgeven de coördinaten deze moet communiceren met Azure AD. Vervolgens bellen `AcquireTokenAsync(...)`, werken met de `IPlatformParameters` object en de stroom verificatie moet een token terugkeren naar de app wordt aangeroepen.

```C#
...
    AuthenticationResult authResult = null;
    try
    {
        AuthenticationContext authContext = new AuthenticationContext(authority);
        authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
    }
    catch (Exception ee)
    {
        results.Add(new User { error = ee.Message });
        return results;
    }
...
```
- `AcquireTokenAsync(...)`eerst probeert te retourneren van een token voor de gevraagde resource (in dit geval de grafiek-API) zonder dat de gebruiker hun referenties invoeren (via caching of oude tokens vernieuwen). Alleen als nodig is, wordt dit de gebruiker het Azure AD-teken in pagina weergegeven voordat u bij het verkrijgen van de gevraagde token.


- U kunt het toegangstoken vervolgens koppelen aan het verzoek Graph API in de koptekst autorisatie:

```C#
...
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
...
```

Dat is alles voor de `DirectorySearcher` PCL en uw app bevindt zich identiteit-gerelateerde code.  Alle blijft is om te bellen de `SearchByAlias(...)` methode in weergaven van elk platform, en waarin de code voor het afhandelen van correct de levenscyclus van de gebruikersinterface voor het toevoegen van nodig.

####<a name="android"></a>Android:
- In `MainActivity.cs`, een oproep toevoegen aan `SearchByAlias(...)` in de knop klikt u op handler:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
```
- U moet ook overschrijven de `OnActivityResult` levenscyclus van de methode voor het doorsturen van elke verificatie wordt omgeleid naar de juiste methode.  ADAL biedt een helpmethode voor dit in Android:

```C#
...
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
...
```

####<a name="windows-desktop"></a>Windows-bureaublad:
- In `MainWindow.xaml.cs`, gewoon bellen naar `SearchByAlias(...)` doorgeven van een `WindowInteropHelper` in van het bureaublad `PlatformParameters` object:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="ios"></a>iOS:
- In `DirSearchClient_iOSViewController.cs`, de iOS `PlatformParameters` gewoon krijgt een verwijzing naar de weergave Controller object:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="windows-universal"></a>Windows-Universal:
- Open in Windows universele, `MainPage.xaml.cs` en implementeren de `Search` methode, die een helpmethode in een gedeelde project gebruikt voor het bijwerken van UI zo nodig.

```C#
...
    List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

Gefeliciteerd! U hebt nu een werkende Xamarin-app met de mogelijkheid om te verifiëren van gebruikers en veilig bellen Web API's voor het gebruik van OAuth 2.0 op vijf andere platforms. Als u nog niet is gedaan, is nu de tijd aan het vullen van uw tenant met enkele gebruikers. Voer uw DirectorySearcher-app en meld u aan met een van deze gebruikers. Zoeken naar andere gebruikers op basis van hun UPN.

ADAL kunt u heel gemakkelijk algemene identiteit functies opnemen in uw app. Dit zorgt voor alle het werk dat dirty voor u - Cachebeheer, OAuth protocolondersteuning, presenteren van de gebruiker met een aanmelding UI, vernieuwen verlopen tokens en meer. Alles wat u moet weten is één API aanroep, `authContext.AcquireToken*(…)`.

Voor een verwijzing naar de voltooide steekproef (zonder de configuratiewaarden) vindt u [hier](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). U kunt nu gaat u naar extra identiteit scenario's. U kunt proberen:

[Een .NET Web API met Azure AD Secure >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
